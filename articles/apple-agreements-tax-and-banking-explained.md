---
title: "苹果开发者账号的 Agreements、Tax and Banking 是做什么的:协议、税务与收款设置详解"
canonical: https://fatevia.cn/blog/articles/apple-agreements-tax-and-banking-explained.html
canonical_en: https://fatevia.cn/en/blog/articles/apple-agreements-tax-and-banking-explained.html
date: 2026-10-12
source: https://fatevia.cn/
---

# 苹果开发者账号的 Agreements、Tax and Banking 是做什么的:协议、税务与收款设置详解

本文件是网站文章的副本。正式页面：[苹果开发者账号的 Agreements、Tax and Banking 是做什么的:协议、税务与收款设置详解](https://fatevia.cn/blog/articles/apple-agreements-tax-and-banking-explained.html)

说明 App Store Connect 中 Agreements, Tax, and Banking 的三类内容:协议类型、税务表单与银行账户,以及哪些App功能和上架行为会受它们影响,什么时候必须补全。

## 它管的是商业关系,不是开发权限

Agreements, Tax, and Banking(协议、税务与银行)是 App Store Connect 里管理账号商业资格的模块。Apple Developer Program License Agreement 决定你能不能开发和分发,而这个模块里的协议、税务和银行信息,决定你能不能在 App Store 上进行商业交易:卖付费应用、卖应用内购买、订阅,以及收到分成款。它与证书、描述文件、TestFlight 的技术权限是两套体系,技术上能打包上传,不代表商业上能收费。

## 协议分两类:免费应用协议与付费应用协议

页面里通常有两块:Free Apps 协议与 Paid Apps 协议。Free Apps 协议在接受 Developer Program 条款后一般随账号状态生效,发布完全免费且无内购的 App 只需要它。Paid Apps 协议需要由账号持有人(Account Holder)或具备相应权限的角色在线接受,并且要先把税务和银行信息填完整,状态才会变为 Active。付费下载、内购、订阅都依赖这份协议处于有效状态。如果你的 App 只做免费分发,可以暂时不管它。

## 税务表单决定预扣税与合规身份

Tax Forms 部分要求按主体所在地和类型提交相应表单。常见情况是美国以外的主体提交美国税务表格(例如 W-8 系列),个人与公司的填写项不同;美国主体则提交 W-9。此外还可能涉及其他国家或地区的税务信息、增值税号等。填写的姓名、主体类型要与开发者账号中的主体信息一致,否则可能被要求更正。具体适用表格、税率以及是否享受税收协定待遇,取决于你的税务居民身份,应以 Apple 页面提示和你的税务顾问意见为准,不要照搬别人的填法。

## 银行账户用于收款,名称与币种要对得上

Banking 部分是绑定收款账户,Apple 按销售所在区域结算后打款。需要填写账户持有人名称、银行信息(如 SWIFT/账号等,字段随地区而不同)。实务上最容易出问题的是持有人名称与开发者主体不一致、账户不支持接收对应币种或外币入账,以及填写后账户验证没有通过。个人账号用个人名下账户,公司账号用公司名下账户,这是最稳妥的对应关系;跨主体收款会增加审核与对账风险。

## 状态没生效时,会在哪里卡住

典型现象:创建内购商品时提示需要协议,商品在提交时状态异常,或 App 无法设置价格。这时先看 Agreements, Tax, and Banking 页面中 Paid Apps 一栏的状态,是否有 New Terms Available(需要重新接受新条款)、Pending 或需要补充信息的提示。Apple 更新协议后,要求持有人重新接受,未接受前相关功能可能受限。判断规则很简单:只要 App 涉及任何收费,上架前就应确认 Paid Apps 为 Active,而不是等到提交审核时才发现。

## 权限与时机:谁来操作、何时处理

协议接受通常需要账号持有人,管理员或 Finance 角色是否能编辑税务与银行信息取决于账号角色设置,以你账号里实际可见的页面为准。更换团队成员或持有人时,务必先确认谁掌握这块权限。建议在做第一个付费功能之前就完成设置,而不是等到准备提交前。审核与验证所需时间不固定,以平台当时的页面提示为准,留出缓冲。如果信息提交后长时间没有变化,或页面要求的资料你无法提供,应联系 Apple 支持,而不是反复修改提交。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# What Agreements, Tax, and Banking Does in an Apple Developer Account

This file mirrors the site article. Official page: [What Agreements, Tax, and Banking Does in an Apple Developer Account](https://fatevia.cn/en/blog/articles/apple-agreements-tax-and-banking-explained.html)

A practical explanation of the Agreements, Tax, and Banking section in App Store Connect: which agreements apply, why tax forms and bank accounts matter, and when missing setup blocks paid features.

## It governs commercial eligibility, not build access

Agreements, Tax, and Banking is the part of App Store Connect that determines whether your account can transact. Certificates, provisioning profiles and TestFlight control what you can build and distribute technically. This section controls whether you can sell paid apps, in-app purchases and subscriptions, and whether Apple can pay you. An app can upload fine and still be unable to charge users if this section is incomplete.

## Two agreement types, with different triggers

You will usually see a Free Apps agreement and a Paid Apps agreement. The free one is generally enough if your app has no price and no in-app purchases. The Paid Apps Agreement must be accepted by the Account Holder or a role with the right permission, and it only becomes Active after tax and banking details are complete. Paid downloads, in-app purchases and subscriptions all depend on it being Active.

## Tax forms tie your legal identity to payouts

Tax forms depend on who you are and where you are based. Non-US entities commonly submit a US form from the W-8 family, while US entities submit a W-9, and other regions may require additional tax IDs. The name and entity type should match your developer account details. Which form applies, and which withholding rate or treaty benefit you qualify for, depends on your tax residency. Follow the prompts on Apple's page and consult a tax professional rather than copying someone else's entries.

## Bank accounts: name, currency and entity must line up

The banking section sets where Apple sends proceeds. Required fields vary by region, but the usual failure points are an account holder name that differs from the developer entity, an account that cannot receive the payout currency, and failed account verification. The safest pairing is simple: individual accounts use accounts in the individual's name, organization accounts use accounts in the organization's name.

## How to recognize when this section is the blocker

Typical symptoms include a prompt about agreements when creating in-app purchases, trouble setting a price, or purchase products behaving oddly at submission. Open the page and check the Paid Apps row: look for statuses such as New Terms Available, Pending, or a request for more information. If Apple has published updated terms, the Account Holder must accept them or related features may be limited. If the app charges for anything, confirm Active status before submission, not during it.

## Who handles it, and when

Accepting agreements normally requires the Account Holder. Whether other roles can edit tax and banking depends on role settings and what your account actually shows. When team members or the holder change, confirm who controls this section first. Set it up before building the first paid feature, and leave buffer time because verification duration is not fixed and depends on what Apple's page shows at the time. If status does not move or you cannot supply the requested documents, contact Apple Developer Support rather than resubmitting repeatedly.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
