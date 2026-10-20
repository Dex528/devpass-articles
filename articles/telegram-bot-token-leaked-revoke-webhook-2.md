---
title: "Telegram 机器人 Token 泄露了怎么办：先在 BotFather 撤销，再核对 webhook 与带 Token 的链接"
canonical: https://fatevia.cn/blog/articles/telegram-bot-token-leaked-revoke-webhook-2.html
canonical_en: https://fatevia.cn/en/blog/articles/telegram-bot-token-leaked-revoke-webhook-2.html
date: 2026-10-20
source: https://fatevia.cn/
---

# Telegram 机器人 Token 泄露了怎么办：先在 BotFather 撤销，再核对 webhook 与带 Token 的链接

本文件是网站文章的副本。正式页面：[Telegram 机器人 Token 泄露了怎么办：先在 BotFather 撤销，再核对 webhook 与带 Token 的链接](https://fatevia.cn/blog/articles/telegram-bot-token-leaked-revoke-webhook-2.html)

Token 泄露后应先在 BotFather 撤销再清理。本文说明撤销后要替换的位置、webhook 是否需要改、带 Token 的 API 与文件链接如何失效，以及怎样判断是否被滥用。

## 先撤销，再清理，顺序不能反

Bot Token 等同于机器人的全部控制权，持有者可以直接调用 Bot API 收发消息、读取更新、执行机器人已有的群管理权限。所以泄露后的第一步是让旧 Token 失效，而不是先去删代码或清 Git 历史，那些操作进行期间 Token 仍然有效。操作方式是在 BotFather 里发送 /revoke 并选择对应机器人，或者进入 /mybots，选中机器人后依次点 API Token 和 Revoke current token。撤销后旧 Token 立即失效，调用会返回 401 Unauthorized，BotFather 同时给出新 Token。代价是你的服务在换上新 Token 之前会中断，这个中断是可接受的，因为它也同时挡住了持有旧 Token 的人。

## 换 Token 要覆盖的不只是环境变量

新 Token 需要同步到所有读取它的地方：服务器环境变量、密钥管理服务、CI/CD 的 secrets、容器或编排配置、定时任务、以及接入了机器人的第三方自动化平台。容易漏的是用 Token 参与校验的逻辑。Mini App 的 initData 校验和 Login Widget 的 hash 校验，密钥都由机器人 Token 派生，只换了发送消息的客户端而忘了校验端，表现就是用户登录或打开小程序时校验全部失败。换完后用新 Token 调一次 getMe 确认身份，再触发一条真实消息走完整条链路。

## webhook 不会随 Token 变化，但要核对两件事

题目里「webhook 要一起改」需要更精确地理解：webhook 是绑定在机器人上的设置，撤销 Token 并不会清除或修改它，所以不是所有人都必须重设。需要处理的是两种情况。一是你的 webhook URL 路径里带了 Token，例如 /bot123456:ABC 或 /webhook/<token>，这个 URL 随 Token 一起泄露过，应换成新的随机路径，并在 setWebhook 时设置 secret_token，服务端校验请求头 X-Telegram-Bot-Api-Secret-Token。二是泄露期间别人可能已经用旧 Token 调过 setWebhook，把更新转到自己的服务器。用新 Token 调 getWebhookInfo，核对 url、pending_update_count 和 last_error_message，只要 url 不是你的，就重新 setWebhook。是否带上 drop_pending_updates 取决于积压的更新是否还有处理价值。

## 带 Token 的链接全部失效，文件地址要改存 file_id

Bot API 的调用地址 https://api.telegram.org/bot<TOKEN>/方法名，以及文件下载地址 https://api.telegram.org/file/bot<TOKEN>/<file_path>，都把 Token 写在路径里。这些地址可能出现在访问日志、前端代码、反向代理记录、已发给用户的下载链接，以及数据库里保存的完整文件 URL。撤销后它们全部返回错误，同时这些地方留着旧 Token 本身也是一条扫描线索，需要清理。长期设计上，数据库应保存 file_id，下载时再调 getFile 取临时 file_path。file_path 带有时效，不适合作为长期存储的地址，更不应该把含 Token 的下载链接直接发给用户。

## 判断是否被滥用：Telegram 不提供 Bot API 调用日志

你无法从 Telegram 一侧查到旧 Token 被调用过什么，只能靠间接痕迹。检查项包括：机器人所在群和频道的管理员操作日志，看有没有不是你触发的踢人、改权限、发消息；机器人名称、简介、命令列表是否被改；webhook 是否被改过；你自己服务器是否出现异常的请求来源和 409 冲突。需要加深审计的条件是：机器人是群或频道管理员、涉及支付或用户数据、webhook 曾异常。若使用了支付，BotFather 里配置的支付服务商 token 是另一套凭据，如果它与机器人 Token 存放在同一处，应到支付服务商后台一并轮换。

## 什么情况下可以收尾

满足以下几点即可认为本次处理完成：旧 Token 调 getMe 返回 401；新 Token 的 getWebhookInfo 与预期一致；业务消息链路和 Mini App 校验验证通过；仓库、CI 日志、前端产物里已无旧 Token。Git 历史中残留的已失效 Token 风险很低，但仍建议清理，避免扫描工具反复告警。后续把 Token 放进密钥管理，不写进前端或客户端 App，不放进 URL 路径，日志中对含 Token 的 URL 做脱敏。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Telegram Bot Token Leaked: Revoke It in BotFather First, Then Check Your Webhook and Token-Bearing URLs

This file mirrors the site article. Official page: [Telegram Bot Token Leaked: Revoke It in BotFather First, Then Check Your Webhook and Token-Bearing URLs](https://fatevia.cn/en/blog/articles/telegram-bot-token-leaked-revoke-webhook-2.html)

What to do when a Telegram bot token leaks: revoke it in BotFather, replace it everywhere, verify the webhook, retire URLs that embed the token, and judge whether the bot was abused.

## Revoke first, clean up second

A bot token is full control of the bot. Whoever holds it can call the Bot API to send messages, read updates and use any admin rights the bot has in groups. So the first move is to invalidate the token, not to delete code or rewrite Git history, because the token stays live while you do that. In BotFather, send /revoke and pick the bot, or open /mybots, select the bot, then API Token, then Revoke current token. The old token stops working immediately and calls with it return 401 Unauthorized. BotFather issues a new one. Your service will be down until the new token is deployed, which is an acceptable cost since it also locks out whoever has the old one.

## Replacing the token means more than one environment variable

Push the new token to every place that reads it: server environment, secret manager, CI/CD secrets, container or orchestration config, scheduled jobs and any third-party automation connected to the bot. The easy miss is validation logic derived from the token. Mini App initData checks and Login Widget hash checks use a key derived from the bot token, so if only the sending client is updated, users will fail login or fail to open the Mini App. After the swap, call getMe with the new token, then run a real message through the full path.

## The webhook does not change with the token, but check two things

The webhook is a setting attached to the bot, and revoking the token does not clear or modify it, so not everyone needs to reset it. Two cases need action. First, if your webhook URL contains the token, such as /bot123456:ABC or /webhook/<token>, that path leaked too. Move to a new random path and set secret_token in setWebhook, then verify the X-Telegram-Bot-Api-Secret-Token header on your server. Second, someone with the old token may have called setWebhook to redirect updates to their own server. Call getWebhookInfo with the new token and compare url, pending_update_count and last_error_message. If the url is not yours, set it again. Use drop_pending_updates only if the queued updates are no longer worth processing.

## URLs with the token are dead; store file_id instead of file links

Bot API calls at https://api.telegram.org/bot<TOKEN>/method and file downloads at https://api.telegram.org/file/bot<TOKEN>/<file_path> put the token in the path. Those URLs may sit in access logs, frontend code, proxy records, download links already sent to users, or full file URLs saved in your database. After revocation they all fail, and any copy of the old token left in those places is still worth removing. For the long term, store file_id and call getFile when you need to download. The file_path it returns is time-limited, so it is not a stable address, and a download link containing the token should never be handed to users.

## Judging whether the bot was abused: there is no Bot API call log

Telegram does not show you what was called with the old token, so you rely on indirect evidence. Check the admin action logs of groups and channels where the bot is present for kicks, permission changes or messages you did not trigger. Check whether the bot name, description or command list was altered. Check whether the webhook was changed. Look in your own server logs for unfamiliar request sources and 409 conflicts. Go deeper if the bot is an admin in groups or channels, handles payments or user data, or the webhook looked wrong. If payments are enabled, the payment provider token configured through BotFather is a separate credential. If it is stored alongside the bot token, rotate it with the provider as well.

## When you can call it done

You can close the incident when the old token returns 401 on getMe, the new token's getWebhookInfo matches what you expect, the message flow and Mini App validation work, and the old token no longer appears in repositories, CI logs or frontend bundles. A dead token left in Git history is low risk, but cleaning it avoids repeated scanner alerts. Going forward, keep the token in a secret manager, out of frontend code, client apps and URL paths, and mask token-bearing URLs in logs.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
