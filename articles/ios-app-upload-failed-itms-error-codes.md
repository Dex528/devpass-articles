---
title: "iOS App 上传失败怎么排查：按签名、校验、ITMS 错误码与构建版本逐层定位"
canonical: https://fatevia.cn/blog/articles/ios-app-upload-failed-itms-error-codes.html
canonical_en: https://fatevia.cn/en/blog/articles/ios-app-upload-failed-itms-error-codes.html
date: 2026-10-13
source: https://fatevia.cn/
---

# iOS App 上传失败怎么排查：按签名、校验、ITMS 错误码与构建版本逐层定位

本文件是网站文章的副本。正式页面：[iOS App 上传失败怎么排查：按签名、校验、ITMS 错误码与构建版本逐层定位](https://fatevia.cn/blog/articles/ios-app-upload-failed-itms-error-codes.html)

Xcode 上传 App 失败时，先判断错误发生在本地签名、上传校验还是 App Store Connect 处理阶段，再按 ITMS 错误码、证书、描述文件与版本号逐项定位。

## 先判断错误发生在哪个阶段，比读错误码更重要

上传失败的信息来自三个不同位置：Xcode 本地的 Archive 与签名阶段、上传前的校验与传输阶段、构建到达 App Store Connect 之后的服务端处理阶段。前两个阶段的错误直接显示在 Xcode 或 Transporter 里，第三个阶段通常以邮件形式发给账号持有人，并带 ITMS-9xxxx 编号。如果界面已经显示「Upload Successful」，之后才收到拒收邮件，就不要再回头折腾本地网络或登录状态，问题在包本身。反过来，如果连上传进度条都没出现，邮件里自然不会有错误码，应该查签名与认证。

## 本地签名阶段：证书、描述文件与 Bundle ID 三者必须对得上

常见现象是「No profiles for 'xxx' were found」「No signing certificate 'iOS Distribution' found」或「Provisioning profile doesn't include signing certificate」。根因通常是以下之一：本机钥匙串里没有该分发证书的私钥（证书在别的电脑上生成，本机只有公钥或根本没有）；描述文件绑定的是已吊销或已过期的证书；Bundle ID 与描述文件里的 App ID 不一致；Target 里勾选的 Team 不是实际持有该 App 的团队。判断办法：在 Signing & Capabilities 中看 Team 与 Bundle Identifier，再到开发者后台核对证书状态和描述文件包含的证书。若使用自动签名仍报错，先用 Xcode 的账号设置重新下载描述文件；手动签名则确认选的是 App Store 分发类型，而不是 Development 或 Ad Hoc。ITMS-90035（签名无效）与 ITMS-90161（描述文件无效）多半也来自这一层，表现为本地能打包、上传后被服务端拒绝。

## 认证与网络阶段：进度条不动、反复要求登录时看这里

这类失败不产生 ITMS 编号，典型表现是认证失败、超时、连接中断或卡在 Authenticating。排查顺序按成本从低到高：确认 Apple ID 在该团队中具备上传构建的角色权限；使用命令行 altool 或 Transporter 时，普通 Apple ID 密码不可用，需要 App 专用密码或 App Store Connect API Key；检查代理、公司防火墙或 VPN 是否干扰上传；条款与协议是否有待账号持有人接受的新版本，这也会阻断上传。用 Transporter 重传同一个 ipa，可以快速区分是 Xcode 的问题还是网络与账号的问题。同一个包在 Transporter 成功而 Xcode 失败，说明问题在 Xcode 的会话或签名配置。

## 构建版本冲突：版本号与构建号分别约束不同的东西

CFBundleShortVersionString 是对外展示的版本号，CFBundleVersion 是构建号。同一个版本号下，每次上传的构建号必须唯一且递增；重复使用会触发 ITMS-90189（Redundant Binary Upload）。如果该版本号已经提交并通过审核或已上线，再用相同或更低的版本号上传，会遇到 ITMS-90186、ITMS-90062 一类关于预发布序列已关闭或版本必须更高的提示。处理原则很简单：只改了代码未改版本，递增构建号；该版本已关闭，递增版本号并重置构建号。用 CI 自动打包的团队，建议由流水线统一生成构建号，避免两台机器各自上传出现冲突。

## 包内容校验：上传后才被拒的常见元数据与 SDK 问题

这一类错误出现在邮件里，对应的是包本身不合规。缺少隐私用途描述会触发 ITMS-90683，需要在 Info.plist 中补上对应权限的 Usage Description，而且要覆盖第三方 SDK 用到的权限。图标问题如 ITMS-90717，常见原因是 App Store 图标含 Alpha 通道。ITMS-90338 指向使用了非公开 API，需要定位是自己的代码还是第三方库。ITMS-90725 一类 SDK 版本提示表示构建所用的 Xcode 或 iOS SDK 低于平台当前要求，具体最低版本以 Apple 开发者新闻与上传要求页面当时的公告为准。处理方式是看邮件正文给出的具体路径，而不是只看编号，因为同一个编号下可能对应不同的二进制或 framework。

## 什么时候停止小修小补，换一种排查方式

同一个错误在你修改两次之后依然出现，说明根因判断有误，应该停止继续微调。此时做三件事：在 Xcode Organizer 里先执行 Validate App，让本地先暴露签名与元数据问题；清理 Derived Data 并从干净的 Archive 重新打包，排除缓存里的旧签名；展开上传日志，核对实际被签名的证书指纹与描述文件内包含的是否一致。如果三者都没问题而邮件仍反复报同一个服务端错误，保留邮件与日志，通过 Apple 开发者支持提交，不要继续用新的构建号去试，那只会堆积无效构建。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# iOS App Upload Failed: Diagnosing Signing, Validation, ITMS Codes and Build Numbers

This file mirrors the site article. Official page: [iOS App Upload Failed: Diagnosing Signing, Validation, ITMS Codes and Build Numbers](https://fatevia.cn/en/blog/articles/ios-app-upload-failed-itms-error-codes.html)

When an Xcode upload fails, first decide whether the error is in local signing, upload validation or App Store Connect processing, then work through ITMS codes, certificates, profiles and version numbers.

## Locate the stage of failure before reading the error code

Upload errors come from three places: local archiving and signing in Xcode, the pre-upload validation and transfer step, and server-side processing after the build reaches App Store Connect. The first two show up in Xcode or Transporter. The third usually arrives by email to the account holder with an ITMS-9xxxx code. If Xcode already says the upload succeeded and a rejection email follows, stop checking your network and login. The problem is in the binary. If the progress bar never moved, there will be no ITMS email, and you should look at signing and authentication instead.

## Local signing: certificate, profile and Bundle ID must agree

Typical messages are 'No profiles for ... were found', 'No signing certificate iOS Distribution found' and 'Provisioning profile doesn't include signing certificate'. The usual causes: the distribution certificate's private key is not in this Mac's keychain; the profile points to a revoked or expired certificate; the Bundle ID does not match the App ID in the profile; or the target is set to a team that does not own the app. Check Team and Bundle Identifier under Signing & Capabilities, then compare the certificate status and the certificates embedded in the profile in the developer portal. With manual signing, confirm the profile is an App Store distribution type, not Development or Ad Hoc. ITMS-90035 (invalid signature) and ITMS-90161 (invalid provisioning profile) often come from this layer too: the archive builds locally but the server rejects it.

## Authentication and network: when the upload stalls or keeps asking for login

These failures carry no ITMS code. You see authentication errors, timeouts or a hang at Authenticating. Work from cheapest to costliest. Confirm the Apple ID has a role that can upload builds for the team. With altool or Transporter, a regular Apple ID password will not work; use an app-specific password or an App Store Connect API key. Check whether a proxy, corporate firewall or VPN interferes. Check for updated agreements waiting for the account holder to accept, which also blocks uploads. Re-uploading the same ipa through Transporter separates Xcode problems from account or network problems: success there and failure in Xcode points to the Xcode session or signing setup.

## Build version conflicts: version and build number constrain different things

CFBundleShortVersionString is the user-facing version and CFBundleVersion is the build number. Within one version, each uploaded build number must be unique and increasing. Reusing one triggers ITMS-90189 (redundant binary upload). If that version has already been approved or released, uploading with the same or a lower version produces messages such as ITMS-90186 or ITMS-90062 about a closed pre-release train or a version that must be higher. The rule: if only the code changed, bump the build number; if the version is closed, bump the version and reset the build number. Teams using CI should let the pipeline generate build numbers so two machines never collide.

## Binary content checks: metadata and SDK problems that surface by email

These errors mean the package itself is non-compliant. A missing purpose string yields ITMS-90683; add the relevant usage description to Info.plist, including permissions requested by third-party SDKs. ITMS-90717 about the App Store icon is commonly caused by an alpha channel. ITMS-90338 points to non-public API use, so find out whether it is your code or a bundled library. SDK-version messages such as ITMS-90725 mean the Xcode or iOS SDK used is below the current requirement; the exact minimum is whatever Apple's developer news and upload requirements pages state at that time. Read the paths in the email body rather than just the code, since one code can refer to different binaries or frameworks.

## When to stop patching and change the approach

If the same error survives two fixes, your root-cause guess is wrong. Do three things: run Validate App in Organizer so local checks surface signing and metadata issues; clear Derived Data and archive again from a clean state to rule out cached signatures; and open the upload log to compare the certificate fingerprint actually used with the one in the profile. If all of that checks out and the server keeps returning the same error, keep the email and logs and contact Apple Developer Support. Do not keep uploading new build numbers, which only piles up invalid builds.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
