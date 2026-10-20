---
title: "iOS 推送发不出去怎么办：APNs 密钥和推送证书该续哪一个"
canonical: https://fatevia.cn/blog/articles/ios-push-apns-key-vs-certificate-renewal-2.html
canonical_en: https://fatevia.cn/en/blog/articles/ios-push-apns-key-vs-certificate-renewal-2.html
date: 2026-10-20
source: https://fatevia.cn/
---

# iOS 推送发不出去怎么办：APNs 密钥和推送证书该续哪一个

本文件是网站文章的副本。正式页面：[iOS 推送发不出去怎么办：APNs 密钥和推送证书该续哪一个](https://fatevia.cn/blog/articles/ios-push-apns-key-vs-certificate-renewal-2.html)

iOS 推送突然失效时，先看服务端用的是 APNs 密钥（.p8）还是推送证书（.p12）。本文按 APNs 返回的错误码区分过期、环境不匹配和令牌问题，并说明续证书还是改用密钥。

## 先确认服务端用的是哪种认证，再决定续什么

APNs 支持两种服务端认证方式：基于令牌的 APNs 认证密钥（.p8 文件，配合 Key ID 和 Team ID 签发 JWT），以及基于证书的推送证书（.p12 或 .cer，有效期一年）。「续哪一个」取决于你的推送服务现在加载的是哪一种。如果服务端配置里是 .p12 加密码，问题多半是证书到期；如果配置里是 .p8、Key ID 和 Team ID，证书到期与你无关，该查的是令牌签发和环境。第三方推送平台（如 Firebase、极光、个推等）的后台里也会让你二选一上传，同样先看那里当前填的是什么。

## APNs 返回的状态和原因，比「发不出去」更能定位问题

不要只看客户端有没有收到，先看服务端日志里 APNs 的 HTTP 状态和 reason 字段。常见对应关系：BadCertificate、BadCertificateEnvironment 或连接被直接拒绝，指向证书失效、证书与环境不符；InvalidProviderToken 指向 Key ID、Team ID 或签名私钥有误；ExpiredProviderToken 指向 JWT 签发时间过旧；TooManyProviderTokenUpdates 指向服务端刷新令牌过于频繁；BadDeviceToken 通常是设备令牌与环境不匹配或已失效；DeviceTokenNotForTopic 和 TopicDisallowed 指向 apns-topic（一般是 Bundle ID）不对。其中只有前两类与证书到期直接相关，其余续证书不会有任何帮助。

## APNs 密钥不会过期，但容易死在令牌刷新上

APNs 认证密钥本身没有到期日，同一个密钥可用于团队下的所有 App，并同时适用于开发和生产环境。它失效的原因通常是被人在开发者后台吊销，或服务端拿错了 Key ID。更常见的故障在 JWT：APNs 要求同一个令牌不要长时间复用，也不要过于频繁地重新生成，一般做法是每 20 到 60 分钟之间刷新一次。服务端从不刷新会得到 ExpiredProviderToken，每次请求都重签则可能得到 TooManyProviderTokenUpdates。另外服务器时钟偏差过大也会让令牌被判为无效，排查时顺手核对时间同步。

## 证书到期后「续期」实际是重新生成并替换

推送证书到期后不能原地延期，流程是在 Certificates, Identifiers & Profiles 里为对应 App ID 新建推送证书，用新的 CSR 签发，导出为 .p12，再替换到服务端或第三方推送平台。App 本身通常不用重新发版，因为设备令牌由系统和 Bundle ID、环境决定，不随服务端证书变化。要注意区分开发与生产两个环境：如果当初只替换了其中一个，TestFlight 与 App Store 版本走生产环境，Xcode 直接安装的调试包走沙盒环境，结果就是一边正常、一边失败。Apple 通常会在到期前发邮件提醒，但邮件发给 Account Holder 等角色，团队里没人看这个邮箱就容易错过。

## 什么情况下建议直接改用 APNs 密钥

如果你有多个 App、需要同时维护开发和生产环境，或者不想每年重复一次证书替换，改用 .p8 更省事：一个密钥覆盖团队下所有 App，不会因时间到期而中断。代价有三点。服务端或第三方平台必须支持令牌认证，旧的自研推送库可能需要升级。.p8 只能在创建时下载一次，必须妥善保管，丢失只能新建。每个团队可创建的密钥数量有限，具体上限以后台页面为准。迁移时可先在服务端保留证书作为回退，上线密钥验证通过后再移除，避免切换窗口里推送中断。

## 换了凭证仍然失败时的停止条件

替换证书或密钥后，用一台已知正常的真机发一条测试推送。如果仍然返回 BadDeviceToken，不要继续折腾凭证，转去核对三件事：设备令牌是在哪个环境下获取的，服务端请求的是沙盒还是生产网关，以及该令牌是否来自已卸载重装前的旧安装。如果返回的是 Unregistered，说明该设备令牌已不再有效，应从库中清理而不是重试。凭证、topic 和环境都核对一遍仍然失败，再去检查 App 是否在 Capabilities 里开启了 Push Notifications，以及描述文件是否包含 aps-environment 权限，这类问题与服务端凭证无关。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# iOS Push Notifications Not Sending: Renew the APNs Key or the Push Certificate?

This file mirrors the site article. Official page: [iOS Push Notifications Not Sending: Renew the APNs Key or the Push Certificate?](https://fatevia.cn/en/blog/articles/ios-push-apns-key-vs-certificate-renewal-2.html)

When iOS push stops working, first check whether your server authenticates with an APNs key (.p8) or a push certificate (.p12). This guide maps APNs error reasons to expiry, environment mismatch and token problems, and explains when to migrate to a key.

## Identify which authentication method your server uses first

APNs accepts two provider authentication methods: a token-based APNs auth key (a .p8 file used with a Key ID and Team ID to sign a JWT), and a certificate-based push certificate (.p12 or .cer, valid for one year). Which one you should renew depends on what your push service is loading right now. If the config holds a .p12 and a password, certificate expiry is a prime suspect. If it holds a .p8, Key ID and Team ID, expiry is irrelevant and you should look at token signing and environment instead. Third-party push platforms such as Firebase let you upload either, so check what is currently configured there.

## The APNs response reason tells you more than a missing notification

Start from the HTTP status and reason string in your server logs, not from whether the device received anything. BadCertificate, BadCertificateEnvironment or a refused connection point to an invalid certificate or a certificate used against the wrong environment. InvalidProviderToken points to a wrong Key ID, Team ID or signing key. ExpiredProviderToken means the JWT is too old. TooManyProviderTokenUpdates means you regenerate it too often. BadDeviceToken is usually a device token from the other environment or one that is no longer valid. DeviceTokenNotForTopic and TopicDisallowed point to a wrong apns-topic, normally the Bundle ID. Only the first group is related to certificate expiry; renewing a certificate will not fix the rest.

## The key never expires, but token refresh often breaks

An APNs auth key has no expiration date, works for every app under the team, and covers both development and production. It stops working when someone revokes it in the developer portal or when the server uses the wrong Key ID. The more common failure sits in the JWT. APNs expects a token to be reused for a while but not forever, so servers typically refresh it somewhere between every 20 and 60 minutes. Never refreshing yields ExpiredProviderToken; signing a new token for every request can yield TooManyProviderTokenUpdates. Large clock drift on the server can also make tokens look invalid, so check time sync as well.

## Renewing a certificate means creating and swapping a new one

An expired push certificate cannot be extended. You create a new one for the App ID in Certificates, Identifiers & Profiles, sign it with a fresh CSR, export it as .p12, and replace it on your server or push platform. The app usually does not need a new release, because device tokens depend on the system, Bundle ID and environment rather than on the server certificate. Watch the two environments: TestFlight and App Store builds use production, while builds installed from Xcode use the sandbox. Replacing only one of them gives you the classic symptom of pushes working on one build and failing on another. Apple usually emails a reminder before expiry, but it goes to specific team roles, and if nobody reads that inbox it is easy to miss.

## When migrating to an APNs key makes sense

If you run several apps, maintain both environments, or simply want to stop repeating a yearly certificate swap, a .p8 key is simpler: one key covers the whole team and nothing times out. The costs are real. Your server or third-party platform must support token-based auth, and an older custom push library may need an upgrade. The .p8 can be downloaded only once at creation, so store it securely, because a lost key means creating a new one. The number of keys per team is limited, so check the portal for the current cap. Keep the certificate as a fallback during migration and remove it only after the key has been verified in production.

## When to stop swapping credentials

After replacing the certificate or key, send a test push to a known-good physical device. If you still get BadDeviceToken, stop touching credentials and verify three things: which environment the device token was obtained in, whether your request targets the sandbox or production gateway, and whether the token comes from an install that has since been removed. If you get Unregistered, the token is dead and should be purged from your database rather than retried. If credentials, topic and environment all check out and sending still fails, look at whether the Push Notifications capability is enabled and whether the provisioning profile contains the aps-environment entitlement. Those problems have nothing to do with server-side credentials.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
