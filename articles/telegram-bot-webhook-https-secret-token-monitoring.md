---
title: "Telegram Bot 如何实现 Webhook：HTTPS 部署、secret_token 验证与收不到消息的排查"
canonical: https://fatevia.cn/blog/articles/telegram-bot-webhook-https-secret-token-monitoring.html
canonical_en: https://fatevia.cn/en/blog/articles/telegram-bot-webhook-https-secret-token-monitoring.html
date: 2026-10-13
source: https://fatevia.cn/
---

# Telegram Bot 如何实现 Webhook：HTTPS 部署、secret_token 验证与收不到消息的排查

本文件是网站文章的副本。正式页面：[Telegram Bot 如何实现 Webhook：HTTPS 部署、secret_token 验证与收不到消息的排查](https://fatevia.cn/blog/articles/telegram-bot-webhook-https-secret-token-monitoring.html)

说明 Telegram Bot 使用 setWebhook 的部署条件、HTTPS 与证书要求、secret_token 请求验证、getWebhookInfo 字段解读，以及 Webhook 接收不到消息时按现象定位根因的方法。

## Webhook 的本质：Telegram 主动 POST，你的服务必须尽快返回 2xx

Webhook 模式下，Telegram 服务器把每条 Update 以 HTTPS POST 的形式推送到你登记的地址，请求体是 JSON。Telegram 只关心 HTTP 状态码：返回 2xx 视为已送达，非 2xx 或超时则会重试一段时间，之后丢弃。这意味着两件事。第一，处理逻辑不应放在请求线程里同步完成，应先落队列或快速 ack，再异步处理。第二，如果你的服务因为业务异常一直返回 500，Telegram 会持续重试同一批 Update，造成积压，而不是跳过。注册方式是调用 Bot API 的 setWebhook，核心参数是 url，常用的还有 secret_token、allowed_updates、max_connections 和 drop_pending_updates。

## HTTPS 部署的硬条件：端口、证书链与域名一致

Telegram 要求 Webhook 地址为 HTTPS。官方文档当前支持的端口为 443、80、88、8443，具体以 Bot API 文档当时的说明为准。使用公共 CA 签发的证书时，最常见的问题不是证书过期，而是服务器只下发了叶子证书、缺少中间证书，浏览器能容忍但 Telegram 的校验会失败。域名需要与证书的 CN 或 SAN 一致，且必须是能被 Telegram 服务器解析到的公网地址，内网域名和只写在本机 hosts 里的域名都不行。使用自签名证书也可以，但要在 setWebhook 时通过 certificate 参数上传公钥证书，反向代理和证书轮换都会因此变复杂，生产环境更适合用 Let's Encrypt 之类的公共证书加 Nginx 或 Caddy 终止 TLS。另外，Webhook 与 getUpdates 长轮询互斥：设置了 Webhook 后，getUpdates 会报错，切回轮询要先 deleteWebhook。

## 请求验证：用 secret_token 校验，而不是依赖地址保密

Webhook 地址一旦泄露，任何人都能向它 POST 伪造的 Update，比如冒充管理员发送指令。较旧的做法是把 Bot Token 或随机串放进 URL 路径，这只能算弱混淆，会出现在访问日志和反代日志里。更规范的做法是在 setWebhook 时传入 secret_token（长度和字符集限制见官方文档），此后 Telegram 每次请求都会带上请求头 X-Telegram-Bot-Api-Secret-Token。服务端应在解析 body 之前先比对该头，建议使用常量时间比较，不匹配直接返回 403 并不记录完整 body。这个机制验证的是请求来源持有你设置的密钥，并不等于验证 Update 内容的业务合法性：用户身份、chat_id 权限、回调按钮的 callback_data 仍需自行校验。IP 白名单可作为补充，Telegram 公布过出站网段，但网段可能变化，不宜作为唯一手段。

## 用 getWebhookInfo 定位问题，先看这几个字段

收不到消息时，先调用 getWebhookInfo，不要先改代码。url 为空，说明 Webhook 根本没有登记成功，或被 deleteWebhook、另一处程序覆盖了。pending_update_count 持续增长，说明 Telegram 已经在推但你的服务没有成功接收。last_error_date 和 last_error_message 是最有价值的字段，常见内容包括连接超时、TLS 握手失败、证书校验失败、返回 4xx 或 5xx 状态码。ip_address 可以确认 Telegram 解析到的是不是你预期的服务器，排查 DNS 切换后仍指向旧机房的情况很有用。如果 last_error_message 为空、pending_update_count 为 0，但仍没有消息，问题多半出在 Bot 侧，比如 allowed_updates 过滤掉了该类型的更新，或者群组里开启了隐私模式导致 Bot 看不到普通消息。

## 按现象归因：收不到消息的五类常见根因

现象一，getWebhookInfo 显示 TLS 或证书错误：多为中间证书缺失、域名不匹配或使用了不受信任的证书，用外部 SSL 检测工具检查证书链即可确认。现象二，显示 connection timed out：防火墙、安全组未放行对应端口，或反代后端处理太慢，应让接口先返回 200 再异步处理。现象三，显示 wrong response status code 4xx 或 5xx：路径与路由不一致、secret_token 校验失败、反代把请求体或 Content-Type 改坏、框架抛出异常。现象四，pending_update_count 很大且不下降：服务长期返回非 2xx，修复后 Telegram 会陆续补发，如果这些旧消息不应再处理，可在重设 Webhook 时使用 drop_pending_updates。现象五，没有任何错误但特定消息收不到：检查 allowed_updates 是否漏掉了 callback_query、chat_member 之类的类型，以及 Bot 在群里的权限和隐私模式设置。同一份排查，按这个顺序走，通常几步就能缩小范围。

## 生产监控：不要等用户反馈才发现 Webhook 失效

Webhook 失效往往是静默的，Bot 不会报错，只是不再响应。至少应监控三项。其一，定时调用 getWebhookInfo，对 url 变化、last_error_date 更新、pending_update_count 超过阈值发出告警，阈值按你的日常流量设定。其二，记录接收端的指标：每分钟收到的 Update 数、非 2xx 比例、处理耗时分位数，流量突然归零同样是告警条件。其三，对证书到期时间做独立检查，自动续期失败是 Webhook 在某天凌晨突然中断的常见原因。幂等性也要考虑：Telegram 重试可能导致同一 update_id 多次到达，用 update_id 去重，涉及发货、扣款等动作的逻辑尤其需要。多实例部署时，去重存储要放在共享层，而不是进程内存。

## 何时选 Webhook，何时仍用长轮询

有稳定公网域名、HTTPS 终止能力，并且需要横向扩容或低延迟响应时，Webhook 更合适，它没有空轮询开销，也便于接入现有网关与监控。本地开发、内网环境、无法暴露公网端口，或者流量很小、不想维护证书的个人 Bot，长轮询更省事。如果必须在本地调试 Webhook，可以用隧道工具临时暴露地址，但隧道域名通常会变化，每次变化都要重新 setWebhook，上线前务必改回固定域名。官方的限制与参数可能调整，实施前以 Telegram Bot API 当时的文档为准。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# How to Implement a Telegram Bot Webhook: HTTPS Setup, secret_token Verification and Debugging Missed Updates

This file mirrors the site article. Official page: [How to Implement a Telegram Bot Webhook: HTTPS Setup, secret_token Verification and Debugging Missed Updates](https://fatevia.cn/en/blog/articles/telegram-bot-webhook-https-secret-token-monitoring.html)

How to set up a Telegram Bot webhook with setWebhook, meet the HTTPS and certificate requirements, verify requests with secret_token, read getWebhookInfo, and trace why a webhook stops receiving messages.

## What a webhook really asks of your server

With a webhook, Telegram pushes each Update to your registered URL as an HTTPS POST with a JSON body. It only looks at the status code. A 2xx response counts as delivered. A non-2xx response or a timeout triggers retries for a while, after which the update is dropped. Two consequences follow. Acknowledge fast and do the real work asynchronously, for example through a queue. And if your handler keeps returning 500 on a bad code path, Telegram keeps resending the same updates and the backlog grows instead of skipping them. You register with setWebhook. The parameters that matter most are url, secret_token, allowed_updates, max_connections and drop_pending_updates.

## HTTPS requirements: ports, chains and hostnames

The webhook URL must be HTTPS. The Bot API has documented ports 443, 80, 88 and 8443; check the current documentation, since this can change. With a certificate from a public CA, the usual failure is not expiry but a missing intermediate certificate. Browsers often tolerate an incomplete chain, while Telegram's validation does not. The hostname must match the certificate's CN or SAN and resolve publicly. Internal names or entries in a local hosts file will not work. Self-signed certificates are allowed if you upload the public certificate through the certificate parameter, but rotation and proxying get awkward. For production, a public CA certificate terminated at Nginx or Caddy is simpler. Webhooks and getUpdates are mutually exclusive: once a webhook is set, getUpdates fails until you call deleteWebhook.

## Verify the sender with secret_token, not an obscure URL

Anyone who learns your webhook URL can POST forged updates, such as fake admin commands. Putting a random string or the bot token in the URL path is weak obscurity, and it ends up in access logs. Pass secret_token to setWebhook instead. Telegram then adds an X-Telegram-Bot-Api-Secret-Token header to every request. Check it before parsing the body, compare in constant time, and return 403 on mismatch without logging the payload. This proves the request came from something holding your secret. It does not authorize the content. You still have to check user identity, chat permissions and callback_data yourself. IP allowlisting using Telegram's published ranges can be an extra layer, but the ranges can change, so do not rely on it alone.

## Start debugging with getWebhookInfo

When messages stop arriving, call getWebhookInfo before touching code. An empty url means the webhook is not registered, or was removed or overwritten by another process. A growing pending_update_count means Telegram is trying to deliver and your server is not accepting. last_error_date and last_error_message are the most useful fields. Typical values cover connection timeouts, TLS handshake or certificate failures, and 4xx or 5xx responses. ip_address shows where Telegram resolved your host, which helps when DNS still points to an old server. If there is no error and the pending count is zero, the cause is usually on the bot side: allowed_updates filtering out the update type, or privacy mode hiding ordinary group messages.

## Five common causes, matched to symptoms

TLS or certificate errors usually mean a missing intermediate, a hostname mismatch or an untrusted certificate. An external SSL checker will confirm the chain. A connection timeout points to a firewall or security group blocking the port, or a backend too slow behind the proxy. Return 200 first and process later. A 4xx or 5xx status suggests a route mismatch, a failed secret_token check, a proxy altering the body or Content-Type, or an unhandled exception. A large pending count that never drops means the server has been failing for a while. Once fixed, Telegram redelivers, and if the old updates should not be processed, use drop_pending_updates when re-registering. Finally, if there is no error but certain updates are missing, check allowed_updates for types like callback_query or chat_member, plus the bot's group permissions and privacy mode. Working through them in this order narrows things down quickly.

## Production monitoring: catch silent failure early

A broken webhook is usually silent. The bot just stops responding. Monitor at least three things. Poll getWebhookInfo on a schedule and alert on url changes, a new last_error_date, or a pending count above a threshold based on your normal traffic. Track receiver metrics: updates per minute, the share of non-2xx responses and handler latency percentiles. Traffic dropping to zero is also an alert condition. Check certificate expiry independently, because a failed auto-renewal is a common reason a webhook dies overnight. Also plan for idempotency. Retries can deliver the same update_id more than once, so deduplicate on it, especially for actions like fulfillment or charges. With several instances, keep the dedup store in a shared layer rather than process memory.

## Webhook or long polling: a decision rule

Choose a webhook when you have a stable public domain, can terminate TLS, and want horizontal scaling or low latency. It avoids idle polling and fits existing gateways and monitoring. Choose long polling for local development, private networks, hosts that cannot expose a port, or small bots where certificate upkeep is not worth it. If you must test a webhook locally through a tunnel, remember the tunnel hostname often changes, which means calling setWebhook again each time, and switch back to a fixed domain before release. Parameters and limits may change, so confirm against the current Telegram Bot API documentation before you deploy.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
