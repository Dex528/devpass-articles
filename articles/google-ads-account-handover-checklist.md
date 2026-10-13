---
title: "Google Ads 账户交接需要检查什么？管理员权限、付款责任与历史数据核验"
canonical: https://fatevia.cn/blog/articles/google-ads-account-handover-checklist.html
canonical_en: https://fatevia.cn/en/blog/articles/google-ads-account-handover-checklist.html
date: 2026-10-13
source: https://fatevia.cn/
---

# Google Ads 账户交接需要检查什么？管理员权限、付款责任与历史数据核验

本文件是网站文章的副本。正式页面：[Google Ads 账户交接需要检查什么？管理员权限、付款责任与历史数据核验](https://fatevia.cn/blog/articles/google-ads-account-handover-checklist.html)

Google Ads 账户交接不是改一个邮箱。本文按管理员权限、付款资料、经理账号关联、转化追踪和变更历史五个层面，说明交接前后要核对的字段与停止交接的条件。

## 先纠正一个前提：Google Ads 没有单独的「所有权转移」按钮

在后台里，账户的实际控制权由几组彼此独立的东西共同决定：谁拥有管理员访问权限，谁控制付款资料，账户挂在哪个经理账号（MCC）下，以及关联的 Google 账号、GA4、Merchant Center 等资产归谁。所谓交接，是把这几组控制权逐项迁移并确认，而不是让新负责人拿到旧邮箱密码。只改其中一项，账户在表面上能用，但付款、数据或恢复渠道仍然握在前任手里。

## 管理员权限：至少两名可联系的管理员，且各自用自己的 Google 账号

进入「管理 → 访问权限和安全」，先看用户列表里每个人的访问级别和状态。需要核对三点：第一，新负责人是否已接受邀请并被授予管理员级别，而不是标准级别，标准级别不能管理其他用户；第二，旧管理员的邮箱是否仍属于公司可控的域名或仍在使用，邮箱失效后收不到验证与恢复邮件；第三，是否存在多人共用一个登录账号的情况。共用登录会让两步验证绑定在某个人的手机上，交接后最容易出现无法登录。建议新管理员确认可独立登录后，再降级或移除旧管理员，顺序不要反过来。

## 付款责任：付款资料是独立对象，权限交接不会自动带走它

广告费用对应的是 Google 付款资料，它有自己的用户与角色体系，不在 Google Ads 的用户列表里管理。交接时要在「结算」相关页面确认几项：付款资料名称与付款方主体是否与实际承担费用的公司一致，当前付款方式是自动付款、手动付款还是月结，付款方式绑定的卡或账户是谁的，以及是否存在未结清余额或待扣款。如果卡仍是前任的个人卡，交接完成后扣款失败就会触发暂停投放。具体的付款选项、资格与限额以平台当时的页面为准，不要凭旧经验判断。

## 经理账号与关联资产：确认账户挂在谁的 MCC 下

在账户的「链接的账号」或经理账号相关页面查看上级经理账号。如果账户由前任的代理商或个人 MCC 管理，那么对方可以保留访问权，甚至可能持有 API 调用所用的开发者令牌。需要决定是保留关联、改挂到自己的 MCC，还是解除关联。同时逐个核对 GA4、Merchant Center、YouTube 频道、Search Console 的关联状态：关联在，不代表对方资产的管理员是你。资产侧权限不在这里，需要分别到对应产品里确认。

## 历史数据核验：转化设置与变更历史比报表数字更重要

报表里的花费和点击只能说明过去发生了什么，交接后真正影响投放的是转化操作、受众列表、自动化规则和脚本。建议逐项导出或截图记录：转化操作的来源、计数方式、归因模型，以及对应的标签或 GA4 事件是否仍在网站上正常触发；受众列表的数据来源与成员有效期；自动化规则、脚本和自动应用的建议是否开启。再查看变更历史，了解近期预算、出价策略和定位的修改记录，保留期限以后台实际显示为准。若发现无人能解释的转化操作或定时脚本，先标记并保持原状，不要在交接当天顺手清理。

## 什么时候应该暂停交接

出现以下任一情况，建议先停下来而不是继续接手：账户目前是暂停或受限状态但原因没有书面说明；唯一管理员联系不上；付款资料的主体与实际广告主不一致且无法修改；对方只愿意提供登录凭据而不愿添加你为用户；你无法确认账户是在与你有真实业务关系的主体名下开立。最后一点需要特别留意：把账户转给无关第三方使用，容易触碰平台对账户滥用与广告主身份真实性的要求，风险会随账户一起转移。合规的做法是在同一业务主体或明确的代理授权关系内，通过添加用户与关联经理账号来完成。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Google Ads Account Handover Checklist: Admin Access, Billing Responsibility and History Verification

This file mirrors the site article. Official page: [Google Ads Account Handover Checklist: Admin Access, Billing Responsibility and History Verification](https://fatevia.cn/en/blog/articles/google-ads-account-handover-checklist.html)

Handing over a Google Ads account takes more than swapping an email. This checklist covers admin access, payments profile, manager account links, conversion tracking and change history, plus when to stop the handover.

## There is no single ownership transfer button

Control of a Google Ads account is spread across several separate things: who holds admin access, who controls the payments profile, which manager account (MCC) the account sits under, and who administers linked assets such as GA4 or Merchant Center. A handover means migrating and confirming each of these. Giving the new owner an old login only moves one of them, and the account can look fine until billing, data or recovery depends on the previous person.

## Admin access: two reachable admins, each on their own Google account

Open Admin, then Access and security, and review each user's access level and status. Check three things. The new lead has accepted the invitation and holds Admin, since Standard cannot manage other users. The previous admin's email is still on a domain you control or is still monitored, because verification and recovery mail goes there. And nobody is sharing one login, which ties two-step verification to a single phone and is the usual cause of lockouts after a handover. Confirm the new admin can sign in independently before downgrading or removing the old one, not the other way around.

## Billing responsibility: the payments profile is its own object

Ad spend is charged through a Google payments profile with its own users and roles, separate from the Google Ads user list. Verify in the billing pages that the profile's legal name matches the entity actually paying, whether the account uses automatic payments, manual payments or monthly invoicing, whose card or bank account is attached, and whether there is an unpaid balance. If the card still belongs to the previous person, a failed charge after handover can pause delivery. Payment options and eligibility change, so rely on what the platform shows at the time.

## Manager account and linked assets: find out whose MCC sits above you

In the linked accounts or manager account section, look at the parent manager account. If an agency or an individual MCC manages the account, they may keep access and may also hold the developer token used for API calls. Decide whether to keep the link, move the account under your own MCC, or unlink it. Then check each linked GA4 property, Merchant Center account, YouTube channel and Search Console property. A link being present does not mean you administer the asset on the other side; that has to be confirmed in each product.

## Historical data: conversion setup and change history matter more than the report totals

Spend and clicks show what happened. What shapes performance after handover is the conversion actions, audience lists, automated rules and scripts. Record each conversion action's source, counting method and attribution model, and confirm the tag or GA4 event still fires on the site. Note where audience lists get their data and their membership duration, and whether rules, scripts or auto-applied recommendations are on. Then read the change history for recent budget, bidding and targeting edits; how far back it goes is shown in the interface. If you find a conversion action or scheduled script nobody can explain, flag it and leave it as is rather than cleaning it up on day one.

## When to pause the handover

Stop if any of these apply: the account is suspended or restricted with no written explanation; the only admin is unreachable; the payments profile name does not match the real advertiser and cannot be changed; the other side offers credentials but refuses to add you as a user; or you cannot confirm the account was opened by an entity with a genuine business relationship to you. The last point matters because passing an account to an unrelated party can run against platform rules on misrepresentation and account misuse, and that risk moves with the account. A sound handover happens inside one business entity or a documented agency authorization, using added users and manager links.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
