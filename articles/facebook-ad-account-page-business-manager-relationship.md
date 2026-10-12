---
title: "Facebook广告账户、公共主页和商务管理平台的关系：资产归属、权限层级与结构选择"
canonical: https://fatevia.cn/blog/articles/facebook-ad-account-page-business-manager-relationship.html
canonical_en: https://fatevia.cn/en/blog/articles/facebook-ad-account-page-business-manager-relationship.html
date: 2026-10-12
source: https://fatevia.cn/
---

# Facebook广告账户、公共主页和商务管理平台的关系：资产归属、权限层级与结构选择

本文件是网站文章的副本。正式页面：[Facebook广告账户、公共主页和商务管理平台的关系：资产归属、权限层级与结构选择](https://fatevia.cn/blog/articles/facebook-ad-account-page-business-manager-relationship.html)

拆解 Facebook 广告账户、公共主页与商务管理平台（Business Manager / Meta Business Suite）的归属与权限关系，说明投放身份、资产迁移限制与多客户场景下的结构选择标准。

## 三者不在同一层：主页是身份，广告账户是计费与投放容器，商务管理平台是资产与权限的管理层

公共主页（Page）是广告在 Facebook 和 Instagram 上展示时使用的品牌身份。广告账户（Ad Account）是创建广告、记录花费、绑定付款方式、保存受众与像素授权的容器。商务管理平台（Business Manager，现在通常在 Meta Business Suite / Business Settings 中管理）不直接投放广告，它负责持有或代理这些资产，并决定谁能对哪个资产做什么。所以三者不是并列的三个账号，而是「身份、投放容器、管理层」三个层级。

## 没有商务管理平台也能投放，但资产挂在个人账号上

个人 Facebook 账号可以直接创建主页和广告账户，这时两者的管理权限都绑定在这个人的个人身份上。能跑广告，但风险集中：个人账号被限制、人员离职或密码丢失，主页和广告账户会一起受影响。把资产放入商务管理平台后，权限由 Business 下的人员和角色承担，个人账号只是登录入口。判断是否需要建立商务管理平台，可以看一条：投放是否会由两人以上长期协作，或资产是否属于公司而非个人。

## 资产有「拥有」和「申请访问」两种关系，决定后续能否拆分

在 Business Settings 里，一个资产对某个 Business 有两种状态：拥有（Owned）和被授予访问（Partner / 代理访问）。拥有意味着该资产归属于这个 Business，一般只能归属一个 Business。被授予访问则是资产仍属于其他 Business，你只获得操作权限，对方可以随时撤销。这一点在代理投放里最关键：广告主把主页和广告账户分享给代理商，和把资产转给代理商拥有，后果完全不同。具体可迁移的范围和限制，以 Meta 当时的帮助中心和后台提示为准。

## 广告账户投放时必须选一个主页，两者的权限要分别配

创建广告时需要选择用于展示的主页（以及可选的 Instagram 账号）。常见故障是：人员有广告账户的广告权限，却没有该主页的相应权限，结果在选主页时看不到或无法发布广告。排查时应分别检查两处：该人员在广告账户上的角色，以及在主页上的任务权限。商务管理平台把这两类权限放在同一个后台里，但它们仍是各自独立授予的，不会因为拥有其一而自动获得另一个。

## 像素、受众、付款方式跟着谁走，要在迁移前确认

像素（数据集）、自定义受众和付款方式并不是简单附属在主页上。像素通常归属于某个 Business 或广告账户并可共享给其他广告账户，自定义受众依赖其所在广告账户的权限，付款方式绑定在广告账户层。改变资产归属或更换广告账户前，要确认历史数据、受众和转化事件是否会随之保留或需要重建。无法确认时，应先在小范围内测试共享或迁移，不要直接对正在投放的主账户操作。

## 按场景选结构：自投、多人协作、服务多个客户

自己单人、预算小、品牌单一，可以先用个人身份加主页，但应尽早补建商务管理平台，因为后期迁移受限制。公司内多人协作，建议主页和广告账户由公司的 Business 拥有，成员按角色分配权限，并至少保留两名管理员。服务多个客户的代理商，应让客户的 Business 拥有其主页和广告账户，再向代理 Business 授予访问，这样终止合作时客户能收回资产。需要提醒的是，商务管理平台可能要求验证，验证要求和额度限制会变化，以平台当时页面为准。

## 出现受限或异常时，先判断是哪一层的问题

广告账户被限制，不等于主页或商务管理平台有问题，三者的限制状态可以单独出现。排查顺序可以是：先看账户质量或 Business 的提示，确认受限对象是广告账户、主页还是整个 Business；再查支付与身份验证状态；最后按平台给出的申诉入口处理。如果受限对象是整个 Business，则其下资产可能同时受影响，这也是不宜把全部资产和全部管理员集中在单一个人账号上的原因。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Facebook Ad Account vs Page vs Business Manager: Ownership, Permissions and How to Structure Them

This file mirrors the site article. Official page: [Facebook Ad Account vs Page vs Business Manager: Ownership, Permissions and How to Structure Them](https://fatevia.cn/en/blog/articles/facebook-ad-account-page-business-manager-relationship.html)

How a Facebook ad account, Page and Business Manager (Meta Business Suite) relate: who owns what, how permissions are layered, and which structure fits solo, team or agency advertising.

## They sit at different layers: identity, billing container, and management

A Page is the brand identity your ads appear under on Facebook and Instagram. An ad account is the container where ads are built, spend is recorded, payment methods are attached and pixel or audience access is granted. Business Manager, now managed through Meta Business Suite and Business Settings, does not run ads itself. It holds or proxies those assets and controls who can do what with each one. So they are not three parallel accounts but three layers.

## You can advertise without a Business, but assets hang off a personal profile

A personal Facebook profile can create a Page and an ad account directly, and admin rights then rest on that person. Ads run fine, but risk concentrates: a restricted profile, a departing employee or a lost password can affect both assets at once. Inside a Business, permissions belong to people and roles, and the personal profile is just a login. A practical test: if more than one person will work on ads long term, or the assets belong to a company rather than an individual, set up a Business.

## Owning an asset and having access to it are different relationships

In Business Settings an asset is either owned by the Business or shared with it as partner access. Ownership generally ties an asset to one Business. Partner access gives you operating rights while the asset stays with another Business, which can revoke it at any time. This matters most for agencies: sharing a Page and ad account with an agency is very different from transferring ownership. What can be moved, and under what limits, depends on Meta's current help pages and in-product prompts.

## Ads need both an ad account role and a Page permission

When creating an ad you pick a Page, and optionally an Instagram account, to publish as. A common failure is that someone has advertiser rights on the ad account but no task permission on the Page, so the Page doesn't appear or the ad can't be published. Check both places separately. Business Settings puts them in one console, but they are granted independently, and having one does not imply the other.

## Check where pixels, audiences and payment methods live before moving anything

Pixels (datasets), custom audiences and payment methods are not simply attached to the Page. A pixel usually belongs to a Business or ad account and can be shared to others, custom audiences depend on permissions in their ad account, and payment methods sit at the ad account level. Before changing ownership or switching ad accounts, confirm whether history, audiences and conversion events carry over or must be rebuilt. If that is unclear, test sharing on a low-stakes asset first rather than touching the live primary account.

## Pick a structure by scenario: solo, team or agency

A solo advertiser with a small budget and one brand can start with a profile and Page, but should add a Business early because later migration has limits. A company with several people should have the Business own the Page and ad account, assign roles, and keep at least two admins. An agency serving multiple clients is better off having each client's Business own its Page and ad account and grant access to the agency Business, so the client can take assets back when the engagement ends. Business verification requirements and limits change, so follow the platform's current pages.

## When something is restricted, identify which layer it hit

A restricted ad account does not mean the Page or Business is in trouble; each can carry its own status. Start by confirming whether the restriction applies to the ad account, the Page or the whole Business, then check payment and identity verification, then use the appeal path the platform provides. If the whole Business is restricted, assets under it may be affected together, which is a reason not to concentrate every asset and every admin under one personal profile.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
