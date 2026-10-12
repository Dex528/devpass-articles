---
title: "iOS Distribution Certificate 是什么：过期、吊销与重新生成的处理边界"
canonical: https://fatevia.cn/blog/articles/ios-distribution-certificate-expired-revoked-regenerate.html
canonical_en: https://fatevia.cn/en/blog/articles/ios-distribution-certificate-expired-revoked-regenerate.html
date: 2026-10-12
source: https://fatevia.cn/
---

# iOS Distribution Certificate 是什么：过期、吊销与重新生成的处理边界

本文件是网站文章的副本。正式页面：[iOS Distribution Certificate 是什么：过期、吊销与重新生成的处理边界](https://fatevia.cn/blog/articles/ios-distribution-certificate-expired-revoked-regenerate.html)

说明 Apple Distribution 证书的作用、私钥与描述文件的绑定关系，区分证书过期与吊销对已上架 App 和新构建的不同影响，并给出重新生成与签名修复的判断顺序。

## 证书只证明「谁在签名」，不决定 App 能不能继续运行

Apple Distribution 证书是开发者团队用于对发布构建签名的身份凭证，用于 App Store 分发、TestFlight 构建上传以及 Ad Hoc、企业类分发场景（具体可用类型取决于账号类型）。它由本机生成的私钥和 Apple 签发的证书两部分共同构成，签名时两者必须同时存在于钥匙串。理解这一点，就能解释多数「证书明明在后台却签不了名」的问题：后台只保存证书，不保存你的私钥。

## 过期：影响的是新签名，不是已上架的版本

证书有有效期，过期后无法再用它签出新的发布构建，Xcode 或 Fastlane 通常会提示找不到有效的签名身份，或描述文件包含的证书已失效。已经通过审核并在 App Store 上架的版本，其可用性不依赖这张证书继续有效，用户不需要因此重装。真正受影响的是你的下一次发版、热修复和 TestFlight 新构建。所以过期的处理时点是在下一次需要发版之前，而不是出现用户侧故障之后。具体有效期和续期规则以开发者后台证书页面为准。

## 吊销：先确认是谁、为什么吊销，再决定是否重建

吊销是在后台主动撤销证书，常见原因有：成员离队后被清理、私钥泄露、误点 Revoke、证书数量达到上限后为腾位而删除。吊销后，依赖该证书的描述文件会变为无效，需要重新生成。如果团队里多人共用同一张证书，贸然吊销会让其他人的本机和 CI 同时失效。操作前先在 Certificates 列表里核对 Created By 与类型，并确认 CI 环境、其他成员机器是否在用这张证书。企业类分发场景下，吊销还可能让已安装的内部分发应用无法继续启动，风险明显高于 App Store 场景，需要单独评估。

## 重新生成前先判断：缺的是证书，还是私钥

很多人看到签名报错就直接新建证书，其实常见根因是换了电脑或重装系统后丢了私钥。判断方法：在钥匙串访问里查看该证书，若证书下没有展开的私钥条目，说明本机缺私钥。此时有三种路径：从原电脑导出 .p12 再导入；使用 Xcode 的自动签名在当前机器生成新证书；或在后台撤销旧证书并新建。前两种不会破坏现有证书，第三种会使旧证书对应的描述文件失效。优先选择能保留旧证书的路径，只有私钥确认无法找回、或怀疑泄露时才吊销。

## 重新生成的实际顺序：证书、描述文件、构建配置三步闭环

重新生成后需要依次完成：在本机用钥匙串访问生成 CSR 并在后台创建新的 Apple Distribution 证书，下载安装；到 Profiles 中把依赖旧证书的发布描述文件重新编辑，勾选新证书后生成并下载；最后在 Xcode 或 CI 中更新签名身份与描述文件引用。使用手动签名或 Fastlane match 的团队，还要同步更新存放证书的仓库或存储。完成后用一次 Archive 并上传验证，而不是只看钥匙串里有没有证书，因为描述文件与证书不匹配时，报错通常在导出阶段才出现。

## 哪些情况应停止自行排查，转向账号层面处理

如果后台看不到证书创建入口、提示证书数量已达上限，或者你不是 Admin 角色而无权创建发布证书，问题就不在签名流程本身。前者需要清理不再使用且确认无依赖的证书，后者需要由 Account Holder 或 Admin 操作或调整角色。如果证书被吊销但团队内无人承认操作，应先检查成员变动记录和 CI 凭据来源，再重建，避免新证书再次被同一原因清掉。证书策略、数量上限与权限规则可能调整，以 Apple Developer 后台当时页面为准。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# iOS Distribution Certificates: What to Do When One Expires, Is Revoked, or Needs Regenerating

This file mirrors the site article. Official page: [iOS Distribution Certificates: What to Do When One Expires, Is Revoked, or Needs Regenerating](https://fatevia.cn/en/blog/articles/ios-distribution-certificate-expired-revoked-regenerate.html)

What an Apple Distribution certificate actually does, how the private key and provisioning profiles tie to it, how expiry differs from revocation, and a practical order for regenerating and fixing signing.

## A distribution certificate proves who signed, not whether an app keeps running

An Apple Distribution certificate is the identity your team uses to sign release builds for App Store submission, TestFlight uploads, and Ad Hoc or enterprise-style distribution, depending on account type. It has two halves: a private key generated on your machine and a certificate issued by Apple. Signing needs both in the keychain. That explains most cases where the certificate shows in the portal yet signing fails: the portal stores the certificate, never your private key.

## Expiry blocks new signatures, not versions already live

Once a certificate expires you can no longer sign new release builds with it. Xcode or Fastlane will report a missing valid signing identity, or a profile containing an invalid certificate. Versions already approved and live on the App Store do not depend on the certificate staying valid, so users do not need to reinstall anything. What breaks is your next release, hotfix, or TestFlight build. Handle expiry before the next planned release, not after something fails. Validity periods and renewal rules are shown on the certificates page in the developer portal and may change.

## Revocation: find out who did it and why before rebuilding

Revocation means the certificate was actively withdrawn in the portal. Typical causes include cleanup after a team member left, a suspected key leak, an accidental Revoke click, or freeing a slot after hitting the certificate limit. Profiles that depend on the revoked certificate become invalid and must be regenerated. If several people or a CI runner share one certificate, revoking it breaks all of them at once. Check Created By and type in the Certificates list, and confirm which machines and pipelines use it. For enterprise-style distribution, revocation can also stop installed in-house apps from launching, which carries much higher risk than the App Store case and deserves separate evaluation.

## Before regenerating, decide whether the certificate or the key is missing

A common root cause is a new or reinstalled machine without the private key, not a bad certificate. In Keychain Access, expand the certificate: if there is no private key entry beneath it, the key is missing locally. You then have three options: export a .p12 from the original machine and import it; let Xcode automatic signing create a new certificate on this machine; or revoke the old one and create another. The first two keep the existing certificate intact. The third invalidates profiles tied to the old one. Prefer a path that preserves the old certificate, and revoke only when the key is truly unrecoverable or may be compromised.

## Regeneration order: certificate, profile, then build configuration

Create a CSR in Keychain Access, use it to issue a new Apple Distribution certificate in the portal, then download and install it. Next, edit each distribution provisioning profile that referenced the old certificate, select the new one, and download the regenerated profile. Finally update the signing identity and profile references in Xcode or CI. Teams using manual signing or Fastlane match must also update wherever the certificate is stored. Validate with a real Archive and upload. A certificate in the keychain proves little, because a mismatch between certificate and profile often surfaces only at export time.

## When to stop debugging and look at account-level causes

If you cannot see the create-certificate option, the portal says the certificate limit is reached, or your role lacks permission to create distribution certificates, the issue is not in the signing workflow. For the limit, remove certificates you have confirmed are unused. For permissions, the Account Holder or an Admin has to act or adjust your role. If a certificate was revoked and nobody admits to it, review recent member changes and CI credential sources before rebuilding, or the new certificate may be removed for the same reason. Certificate limits and role rules can change, so rely on the developer portal as it appears at the time.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
