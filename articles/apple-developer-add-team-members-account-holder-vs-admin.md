---
title: "Apple 开发者账号如何添加开发人员：Account Holder 与 Admin 的权限边界"
canonical: https://fatevia.cn/blog/articles/apple-developer-add-team-members-account-holder-vs-admin.html
canonical_en: https://fatevia.cn/en/blog/articles/apple-developer-add-team-members-account-holder-vs-admin.html
date: 2026-10-15
source: https://fatevia.cn/
---

# Apple 开发者账号如何添加开发人员：Account Holder 与 Admin 的权限边界

本文件是网站文章的副本。正式页面：[Apple 开发者账号如何添加开发人员：Account Holder 与 Admin 的权限边界](https://fatevia.cn/blog/articles/apple-developer-add-team-members-account-holder-vs-admin.html)

说明在 App Store Connect 的「用户与访问」中邀请成员的步骤，对比 Account Holder 与 Admin 的职责差异，并给出外包、离职交接时分配角色的判断标准。

## 添加成员的入口在 App Store Connect，不在证书后台

邀请成员的操作在 App Store Connect 的「用户与访问」（Users and Access）里完成。点击加号，填写对方姓名与邮箱，选择角色，必要时限定可访问的 App，然后发送邀请。对方用邮件里的链接接受后，才会成为正式成员。邮箱需要对应一个已启用双重认证的 Apple 账户，否则对方多半卡在登录或接受邀请这一步。developer.apple.com 上的成员列表与此同源，不需要在两处各加一次。邀请界面里可选的角色和附加选项会随账号类型与你自己的角色变化，以页面当时显示为准。

## Account Holder 是法律主体，只有一个

Account Holder 是注册 Apple Developer Program 时的签约人，代表个人或公司对协议负责。每个团队只有一位，无法通过邀请新增第二位。涉及协议接受、会员资格续费、App 转让等法律或财务责任的操作，通常只有 Account Holder 能完成。因此它更像「责任主体」，而不是日常操作账号。公司账号里，这一角色应绑定在有法律授权的人身上，并且这个 Apple 账户的邮箱和双重认证手机号要由公司掌控，而不是某位工程师的私人邮箱和手机。

## Admin 管团队和资源，但管不了 Account Holder

Admin 可以邀请和移除成员、调整角色、管理 App 与大部分开发资源，是日常的团队管理者。它与 Account Holder 的核心区别在于：Admin 不是协议签约方，不能改动 Account Holder 本人，也无法替代它完成专属的法律与会员操作。Admin 权限很大，误删成员、改动证书都可能影响整个团队的发布，所以数量宜控制在能互相兜底的 2 到 3 人。至于银行、税务等财务设置具体由谁能做，以平台当前的权限说明为准，不要凭旧经验假设。

## 按工作内容选角色，别默认给 Admin

选角色可以用一个简单标准：对方需要改动团队本身吗？不需要就不给 Admin。纯写代码、上传构建的工程师，用 Developer 或 App Manager 并限定到具体 App 即可。运营和市场只需 Marketing 或查看报表类角色。财务对账用 Finance 或报表角色。外包团队尤其适合限定 App 范围，项目结束后立即移除。如果某人需要证书与描述文件，先看角色是否自带，再看邀请表单是否提供额外勾选项，而不是直接升级到 Admin。

## 邀请发出后没反应，按这个顺序查

邀请状态会在「用户与访问」里显示为待处理。如果对方说没收到，先核对邮箱是否与对方实际使用的 Apple 账户一致，再查垃圾邮件。如果对方接受后看不到 App，多半是邀请时只勾选了部分 App，或角色本身没有对应入口，回到成员详情里调整即可。如果对方无法登录，检查双重认证是否启用。邀请有时效，过期后需要删除旧记录并重新发送。重发两次仍失败时，应怀疑邮箱对应的 Apple 账户状态异常，让对方换一个邮箱而不是继续重试。

## 人员变动前先处理 Account Holder 和 Admin

团队里最容易出事的是 Account Holder 离职。账号本人不在，续费、协议更新、App 转让都会卡住。Apple 提供转移 Account Holder 的流程，需要在会员详情中发起并由新人确认，应在离职前完成，而不是离职后再补救。普通 Admin 离职时，先确认仍有至少一位 Admin 可用，再移除其权限，并检查其名下是否还有只有他知道的 API 密钥或 CI 凭据。建议每季度对一次成员清单，把不再参与项目的人移除。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# How to Add Team Members to an Apple Developer Account: Account Holder vs Admin

This file mirrors the site article. Official page: [How to Add Team Members to an Apple Developer Account: Account Holder vs Admin](https://fatevia.cn/en/blog/articles/apple-developer-add-team-members-account-holder-vs-admin.html)

How to invite people in App Store Connect Users and Access, what separates Account Holder from Admin, and how to pick roles for contractors, staff changes and handovers.

## Invitations start in App Store Connect, not the certificates portal

Go to Users and Access in App Store Connect, click the plus button, enter the person's name and email, pick a role, optionally limit them to specific apps, and send the invite. They become a member only after accepting it from the email. The address must belong to an Apple Account with two-factor authentication enabled, or the invitee will often stall at sign-in. The member list on developer.apple.com comes from the same source, so you do not add people twice. Available roles and extra options vary with account type and your own role, so trust what the form shows at the time.

## The Account Holder is the legal entity, and there is only one

The Account Holder is the person who signed the Apple Developer Program agreement on behalf of an individual or organization. Each team has exactly one, and you cannot add a second through an invite. Actions tied to legal agreements, membership renewal and app transfer generally require this role. Treat it as the accountable party rather than a daily working login. For a company, bind it to someone with legal authority, and keep the Apple Account email and 2FA phone number under company control instead of a single engineer's personal ones.

## Admins run the team but cannot replace the Account Holder

An Admin can invite and remove members, change roles and manage apps and most development resources. The key difference is that an Admin is not the contracting party, cannot modify the Account Holder, and cannot take over the Account Holder's legal and membership actions. Admin access is broad, and an accidental removal or certificate change can block releases for everyone, so keep the count to two or three people who can cover for each other. Which financial settings an Admin can touch is defined by Apple's current permission documentation, so check it rather than relying on older experience.

## Choose roles by the work, not by default to Admin

A simple test: does this person need to change the team itself? If not, skip Admin. Engineers who write code and upload builds fit Developer or App Manager, scoped to specific apps. Marketing staff need Marketing or reporting-oriented roles, and finance can use Finance or reporting access. For contractors, limit app scope and remove them when the project ends. If someone needs certificates or profiles, check whether the role already includes them or whether the invite form offers an extra option, before escalating to Admin.

## When an invite goes nowhere, check in this order

Pending invites show their status in Users and Access. If the invitee says nothing arrived, confirm the email matches the Apple Account they actually use, then check spam. If they accepted but cannot see an app, the invite was probably scoped to a subset of apps or the role lacks that area, which you can fix on the member's detail page. If they cannot sign in, verify two-factor authentication. Invites expire, so delete the stale record and send a new one. If two resends fail, suspect a problem with that Apple Account and ask for a different email instead of retrying.

## Handle the Account Holder and Admins before people leave

The costliest failure is an Account Holder leaving. Renewals, agreement updates and app transfers stall when the one person who can act is gone. Apple provides a flow to transfer the Account Holder role, started from membership details and confirmed by the successor, and it should be completed before departure. When an Admin leaves, confirm at least one other Admin remains, then remove access and check for API keys or CI credentials only that person knew. Review the member list each quarter and remove anyone no longer on the project.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
