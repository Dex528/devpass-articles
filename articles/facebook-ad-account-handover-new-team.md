---
title: "Facebook 广告账户如何安全地交接给新团队？权限迁移、资产核对与责任划分"
canonical: https://fatevia.cn/blog/articles/facebook-ad-account-handover-new-team.html
canonical_en: https://fatevia.cn/en/blog/articles/facebook-ad-account-handover-new-team.html
date: 2026-10-14
source: https://fatevia.cn/
---

# Facebook 广告账户如何安全地交接给新团队？权限迁移、资产核对与责任划分

本文件是网站文章的副本。正式页面：[Facebook 广告账户如何安全地交接给新团队？权限迁移、资产核对与责任划分](https://fatevia.cn/blog/articles/facebook-ad-account-handover-new-team.html)

Meta 广告账户交接的关键是先分清运营权与所有权，再按资产清单迁移权限、支付、像素和主页，最后用验收清单划清责任。本文说明各步骤的判断条件和停止点。

## 先判断要交的是运营权还是所有权

多数交接只需要运营权：让新团队在商务管理平台（现在界面多称业务组合）里获得广告账户的管理权限，原账户的所有者不变。所有权转移是另一件事，指把广告账户的归属从一个 Business 换到另一个 Business。前者可随时调整、可回退；后者涉及两边的管理员确认，且平台对已归属账户的再次转移有限制，具体规则以 Meta 当时的帮助页面和后台提示为准。判断标准很简单：如果旧公司仍持有该资产的法律或财务责任，或者交接可能只是阶段性的，就只转运营权。

## 动手前先导出一份资产清单

在「商务设置」里逐类核对，不要只看广告账户。至少包括：广告账户（账户编号、状态、币种、时区）、主页、Instagram 账号、像素或数据集、产品目录、域名验证、自定义受众与共享受众、应用、系统用户及其令牌、支付方式。每一项记录三个字段：当前所有者是谁，谁有完全控制权，是否被其他系统依赖，例如第三方归因工具或 CRM 通过 Conversions API 在写入数据。清单漏掉的资产，往往是交接后才发现投放中断或数据断流的原因。

## 权限迁移的顺序：先加人，验证后再减人

在商务设置的「用户」下，先把新团队的负责人以管理员身份加入，再按职责给其他成员分配资产级权限，投手给广告账户的管理广告权限，分析人员只给查看权限，财务只开放账单相关项。管理员应使用本人账号并开启双重验证，不要让新团队共用旧成员的登录凭据，共用会让操作无法追溯，也容易触发异常登录验证。新管理员确认能正常编辑广告、访问主页和像素之后，才移除旧成员。保留至少两名可信管理员，避免单人离职后无人能恢复权限。

## 支付方式和账单要单独处理

支付方式通常绑定在广告账户或业务层面，卡的持有人、账单邮箱和已产生的未结账款不会因为加了管理员就自动变更。交接时要确认三件事：新团队使用哪张支付方式，当前是否有欠款或待扣费，旧卡是否会在交接后继续被扣款。在新支付方式被确认可用之前，不要删除旧方式，否则可能因为扣款失败导致投放暂停。发票抬头和税务信息是否能变更，取决于账户当时的设置，以后台显示为准。

## 像素、转化 API 与受众是最容易被忽略的依赖

广告账户换人管理后，像素和数据集如果仍归旧 Business 所有，新团队只拿到使用权就够，但必须确认该资产已分配给广告账户，并且新团队有编辑权限，否则无法调整事件或排查转化。使用 Conversions API 的站点，需要核对令牌由谁生成、存放在哪台服务器；令牌属于要移除的成员或系统用户时，移除前先重新生成并替换。自定义受众和再营销名单如果由旧账户创建，要确认共享关系保留，否则新账户可能看不到历史受众。

## 用书面验收划清交接前后的责任

交接当天建立一份验收记录，内容包括：交接时间点、账户状态与是否有受限或审核提示、日预算与正在投放的广告系列、未结账单、各资产的新旧权限对比、已移除的人员。账户在交接前产生的违规记录和限制历史会跟着账户走，不会因换团队而清零，所以投放素材、落地页和主页内容也应在验收时一并检查。约定清楚从哪个时间点起，由哪一方对政策违规、账单争议和数据安全负责。

## 出现这些情况时，停止直接转移

账户当前处于受限、待审核或支付异常状态；资产归属不清，旧团队无法提供管理员权限；或者对方要求你提供个人登录信息而不是添加管理员。这几种情况下，先走官方的申诉或归属验证流程，不要用共享登录或转手账号的方式绕过。账户是个人号名下的广告账户时，也建议先把它纳入企业商务管理平台再谈交接，否则权限始终依附于某个人的账号。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Handing Over a Facebook Ad Account to a New Team: Permissions, Asset Audit and Accountability

This file mirrors the site article. Official page: [Handing Over a Facebook Ad Account to a New Team: Permissions, Asset Audit and Accountability](https://fatevia.cn/en/blog/articles/facebook-ad-account-handover-new-team.html)

A safe Meta ad account handover starts by separating operating access from ownership, then moving permissions, billing, pixels and Pages against an asset list, and closing with a written acceptance check.

## Decide first: operating access or ownership

Most handovers only need operating access. The new team gets admin or asset-level permissions inside the business portfolio (formerly Business Manager), and the account's owner stays the same. Ownership transfer moves the ad account from one business to another, needs approval on both sides, and Meta restricts repeated moves, so check the current help page and the prompts in Business Settings. If the old company still carries legal or billing responsibility, or the handover may be temporary, transfer access only.

## Export an asset list before changing anything

In Business Settings, go through every asset type, not just the ad account: ad accounts (ID, status, currency, time zone), Pages, Instagram accounts, pixels or datasets, catalogs, verified domains, custom and shared audiences, apps, system users with their tokens, and payment methods. For each, note the current owner, who has full control, and what depends on it, such as an attribution tool or CRM writing through the Conversions API. Missed dependencies are the usual reason campaigns stall or data goes missing after a handover.

## Add the new people first, remove the old ones after testing

Under Users, add the new team lead as an admin with their own account and two-factor authentication, then assign asset-level access by role: manage-campaign access for media buyers, view-only for analysts, billing-only for finance. Do not hand over someone's login credentials. Shared logins destroy the audit trail and often trigger login checks. Once the new admins confirm they can edit ads and reach the Page and pixel, remove the departing members. Keep at least two trusted admins so one departure cannot lock the business out.

## Treat billing as its own workstream

Payment methods are tied to the ad account or business, and adding admins changes neither the cardholder, the billing contact nor any outstanding balance. Confirm which method the new team will use, whether there is unpaid spend, and whether the old card will keep being charged. Do not delete the old method until the new one is verified, or a failed charge can pause delivery. Whether invoice and tax details can be edited depends on the account's settings at the time, so rely on what the interface shows.

## Pixels, Conversions API and audiences are the hidden dependencies

If the pixel or dataset stays under the old business, the new team only needs usage rights, but it must be assigned to the ad account with edit permission, or event setup and debugging will fail. For Conversions API setups, check who generated the token and where it is stored. If it belongs to a member or system user you plan to remove, regenerate and replace it first. Audiences built under the old account may need their sharing relationship confirmed, or the new setup may not see them.

## Close with a written acceptance record

On handover day, record the timestamp, account status including any restriction or review notices, daily budgets and live campaigns, unpaid bills, before-and-after permissions per asset, and removed users. Violation history and restrictions stay with the account, so review creatives, landing pages and Page content at the same time. Agree in writing who owns policy issues, billing disputes and data security from which date.

## When to stop and use the official route

Stop if the account is restricted, under review or has a payment problem; if ownership is unclear and the old team cannot provide admin rights; or if anyone asks you to share personal logins instead of adding admins. Use Meta's appeal or ownership verification flow in those cases. If the ad account sits under a personal profile, move it under a business portfolio before the handover, otherwise access stays tied to one individual's account.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
