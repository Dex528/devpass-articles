---
title: "苹果开发者账号 Account Holder 怎么转移：交接不能只改密码"
canonical: https://fatevia.cn/blog/articles/apple-developer-account-holder-transfer-2.html
canonical_en: https://fatevia.cn/en/blog/articles/apple-developer-account-holder-transfer-2.html
date: 2026-11-17
source: https://fatevia.cn/
---

# 苹果开发者账号 Account Holder 怎么转移：交接不能只改密码

本文件是网站文章的副本。正式页面：[苹果开发者账号 Account Holder 怎么转移：交接不能只改密码](https://fatevia.cn/blog/articles/apple-developer-account-holder-transfer-2.html)

说明苹果开发者账号交接时 Account Holder 的转移条件、操作顺序，以及证书、密钥、协议和续费这些不会随角色自动迁移的资产，并给出验收与撤权的判断标准。

## 改密码交付的是登录能力，不是账号责任

Apple ID 只是登录凭据，Account Holder 是开发者计划里的法律责任人角色，每个团队只有一个。把 Apple ID 密码和验证码手机一并交给接手人，表面上能进后台，实际上协议签署人、续费责任和 Apple 的认证邮件仍然指向原来的人，也违背 Apple ID 归个人使用的原则。一旦原持有人换手机号、离职或注销邮箱，团队会在最不方便的时候失去恢复渠道。所以交接要分两件事：把角色正式转给新持有人，再逐项迁移角色不会带走的资产。

## 先确认账号类型，再确认接手人资格

组织账号才存在角色转移这条路。接手人通常需要已经是该团队成员，并具备 Admin 权限，Apple ID 要开启双重认证，同时应当是有权代表该主体签署协议的人，比如法人、董事或获授权的负责人。个人账号的 Account Holder 绑定的是自然人本人，一般不能把角色转给别人，需要换人时更现实的做法是把 App 转移到对方的账号。具体条件和入口以 Apple 当时的官方文档为准。还有一点要分清：角色转移不会更换主体名称和 D-U-N-S 信息，公司实体本身变了，要走 Apple 开发者支持的主体变更流程，不是转角色。

## 推荐的操作顺序：先授权，再转移，最后核对

先由现任 Account Holder 在「用户和访问」里邀请接手人，并设为 Admin，让对方用自己的 Apple ID 接受邀请并确认双重认证可用。随后现任持有人在开发者账号的会员资格相关页面发起 Account Holder 转移，入口名称可能随改版变化，以页面实际显示为准。接手人收到通知后登录确认，并接受最新的协议。转移完成后，不要立刻移除原持有人，先确认新持有人能看到会员资格有效期、协议状态和团队成员列表，这一步是后面撤权的前提。

## 角色转移带不走的东西要逐项迁移

这些资产需要单独交接：分发证书对应的私钥，只存在于创建者的钥匙串里，需要导出 .p12 或重新签发；APNs 的 .p8 密钥只能在创建时下载一次，丢了只能新建，且同类密钥数量有上限；App Store Connect API 密钥按创建者与权限归属，CI 里的 Fastlane、Xcode Cloud、Match 仓库口令要重新配置；Firebase、极光等第三方推送服务里上传的 APNs 凭据也要同步更换；「协议、税务和银行」里的收款与税务信息要由新持有人核对；会员费的付款方式和自动续费状态也要检查，续费失败会让 App 在商店下架。

## 原持有人联系不上时，不要借账号绕过去

持有人已离职、失联或无法收到验证码时，自行借用其 Apple ID 登录属于高风险做法，容易触发账号安全锁定，也无法产生合规的角色变更。更稳妥的做法是通过 Apple 开发者支持提交变更请求，通常需要证明主体身份、新持有人的授权关系和联系方式，所需材料与时长以 Apple 当时的说明为准，不要假设固定天数。同时尽早盘点还能操作的资产，比如已在后台的 Admin 能否导出 App 清单、能否新建 API 密钥，为拿到持有权后的迁移争取时间。

## 验收通过后再撤权，用一次完整发布作为标准

建议把「新环境完成一次完整发布闭环」作为撤权门槛：用新持有人或新签发的证书打包上传一个构建到 TestFlight，用新的 APNs 密钥发出一条测试推送，CI 使用新的 API 密钥跑通一次流水线。三项都通过后，再撤销旧证书和旧密钥，并移除原持有人的访问权限。若在验收中发现任何一项仍依赖旧人的凭据，就暂停撤权，先补迁移。撤销证书会影响新的签名与构建，已上架 App 通常不会因此消失，但具体影响仍要按证书类型确认。

## Telegram 频道

- [谷歌开发者账号购买｜苹果开发者账号购买｜iOS](https://t.me/s/GoogleAppleDev)
- [iOS苹果开发者账号购买｜个人公司](https://t.me/s/iOSDeveloperService)
- [Google Play谷歌开发者账号购买｜个人组织老号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户购买｜Google Ads开户](https://t.me/s/GoogleAdsChina)
- [Telegram机器人定制开发｜TG机器人](https://t.me/s/TGBotDevService)

---

# Transferring the Apple Developer Account Holder Role Safely: A Password Change Is Not a Handover

This file mirrors the site article. Official page: [Transferring the Apple Developer Account Holder Role Safely: A Password Change Is Not a Handover](https://fatevia.cn/en/blog/articles/apple-developer-account-holder-transfer-2.html)

How the Account Holder role moves on an Apple Developer team, what the new holder needs, and which certificates, keys, agreements and renewal settings stay behind and must be migrated separately.

## Handing over a password hands over access, not accountability

An Apple ID is only a login. Account Holder is the role that carries legal responsibility in the Developer Program, and a team has exactly one. If you pass along the Apple ID password and the phone that receives verification codes, the new person can sign in, but agreements, renewal responsibility and Apple's verification emails still point to the old holder. It also goes against the expectation that an Apple ID belongs to one person. When that person changes number or leaves, the team loses its recovery path at the worst time. Treat the handover as two jobs: transfer the role formally, then migrate the assets the role does not carry.

## Check the account type and the successor's eligibility first

Role transfer applies to organization accounts. The successor generally needs to be an existing team member with Admin access, two-factor authentication on their own Apple ID, and the authority to bind the organization to agreements, such as an officer or an authorized signatory. An individual account is tied to the person who enrolled and typically cannot hand the role to someone else. The usual alternative is transferring the apps to the other party's account. Always confirm the conditions on Apple's current documentation. Also note that a role transfer does not change the legal entity name or D-U-N-S data. If the company itself changed, that goes through Apple Developer Support as an entity change.

## A workable sequence: grant, transfer, verify

Invite the successor under Users and Access as an Admin, and have them accept with their own Apple ID and confirm two-factor works. The current Account Holder then starts the Account Holder transfer from the membership area of the developer account. Menu names change over time, so follow what the page shows. The successor confirms the request and accepts the latest agreements. Do not remove the previous holder yet. First check that the new holder can see membership status, agreement state and the team member list, because that check is what makes revoking access safe later.

## What the role transfer does not move

Several items need separate handling. Private keys behind distribution certificates live in the creator's keychain, so export a .p12 or issue new certificates. An APNs .p8 key can only be downloaded once when created, so a lost one means creating a new key, and there is a cap on how many you can hold. App Store Connect API keys are tied to how they were created, and CI systems such as Fastlane, Xcode Cloud or a Match repository need new credentials. Push providers like Firebase need the APNs credential replaced too. Banking and tax details under Agreements, Tax, and Banking should be reviewed by the new holder, and so should the payment method and auto-renew setting, since a lapsed membership can lead to apps being removed from sale.

## If the current holder is unreachable, don't borrow the account

When the holder has left or cannot receive codes, signing in with their Apple ID is risky. It can trigger account locks and does not produce a legitimate role change. Contact Apple Developer Support to request the change. Expect to prove the entity's identity and the successor's authorization, with requirements and timing set by Apple at the time, so do not plan around a fixed number of days. Meanwhile, use any Admin access you already have to inventory apps and create new API keys, so migration can start as soon as control is confirmed.

## Revoke old access only after one full release cycle works

Use a concrete gate: upload a build to TestFlight with the new signing setup, send a test push with the new APNs key, and run the CI pipeline with the new API key. When all three pass, revoke the old certificates and keys and remove the previous holder's access. If any step still depends on the old person's credentials, pause the revocation and finish the migration first. Revoking a certificate affects future signing and builds. Live apps are generally not pulled, but the exact impact depends on the certificate type, so check before you revoke.

## Telegram channels

- [Buy Google & Apple developer accounts](https://t.me/s/GoogleAppleDev)
- [Buy iOS Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Buy Google Play developer accounts](https://t.me/s/GoogleDeveloperService)
- [Buy Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bot custom development](https://t.me/s/TGBotDevService)
