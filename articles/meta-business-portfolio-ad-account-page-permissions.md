---
title: "Meta 广告账户、商务资产组合与主页的关系：所有权、访问权与任务权限怎么拆"
canonical: https://fatevia.cn/blog/articles/meta-business-portfolio-ad-account-page-permissions.html
canonical_en: https://fatevia.cn/en/blog/articles/meta-business-portfolio-ad-account-page-permissions.html
date: 2026-10-13
source: https://fatevia.cn/
---

# Meta 广告账户、商务资产组合与主页的关系：所有权、访问权与任务权限怎么拆

本文件是网站文章的副本。正式页面：[Meta 广告账户、商务资产组合与主页的关系：所有权、访问权与任务权限怎么拆](https://fatevia.cn/blog/articles/meta-business-portfolio-ad-account-page-permissions.html)

梳理 Meta 商务资产组合（原 Business Manager）中广告账户、主页的归属关系，以及人员、合作伙伴与任务权限的区别，帮助你搭建不易失控的权限结构。

## 三者不是并列关系：资产组合是容器，账户和主页是被它持有的资产

商务资产组合（Business Portfolio，早期叫 Business Manager，常简称 BM）是管理层。广告账户、主页、Instagram 账号、数据集（Pixel）、目录等都是挂在它下面的资产。主页负责对外身份，也就是广告以谁的名义展示；广告账户负责投放、计费和报表。两者通过「广告需要一个主页或 Instagram 账号作为发布身份」产生关联，但彼此并不隶属。判断结构是否合理，先问三件事：每个资产归谁所有，谁能访问，访问的是哪一类任务。

## 所有权只有一个，访问权可以有很多

一个广告账户或主页，同一时间只能被一个资产组合「拥有」。其他组合或个人只能通过访问权使用它。这个区别决定后果：所有者组合持有资产的最终控制权，包括增删管理员、解除共享；被共享方即使有完整任务权限，也无法让资产脱离所有者。因此，如果你是代运营方或投放团队，用客户的主页和广告账户时，应清楚这些资产的所有者不是你。反过来，把自己的核心资产挂在别人名下，等于把控制权交给对方。资产能否迁移、迁移条件是什么，以后台当时提示为准，不要默认可以随时转出。

## 人员权限分两层：组合级身份与资产级任务

给一个人加权限，实际上涉及两层。第一层是组合级身份，通常分管理员访问和员工访问：管理员能管理全部设置与人员，员工只能看到被分配的资产。第二层是资产级任务，例如对广告账户可以分配管理账户、创建广告、查看效果等；对主页可以分配管理主页、创建广告、发布内容、查看洞察等。常见的错误是只加了广告账户权限，却没加主页的「创建广告」任务，结果投放时看不到可用主页。排查「选不到主页」「看不到广告账户」时，先核对这两层，而不是重复刷新或换浏览器。

## 合作伙伴共享：把资产给另一个组合，而不是给某个人

当对象是代理商、外包团队或关联公司时，更合适的方式是添加合作伙伴：用对方的商务资产组合编号，把指定资产共享过去，由对方的管理员再分配给其员工。好处是人员流动时，你不必逐个增删外部成员，只需撤销对该组合的共享。需要注意的是，共享的任务范围就是对方能做的上限，对方管理员可以在这个范围内再分配，但无法超出。给代理商只开「创建广告」和「查看效果」，通常比开完整管理权限更稳妥，因为后者可能涉及付款方式和支出限额等敏感设置。

## 哪些结构会让账号在出问题时无法自救

几类结构风险反复出现：组合里只有一名管理员，且该人账号被限制或离职，没人能恢复管理；主页只挂在某个员工的个人身份下，没有放入组合，员工账号异常时主页也一同失联；广告账户的付款方式和主页分别由不同主体持有，出现争议时责任难以划分；所有资产都放在同一个组合，一旦组合被审核或限制，影响范围覆盖全部业务。一个可执行的底线是：至少两名真实在职的管理员，核心主页与广告账户都归组合所有，权限按岗位分配并定期复核。是否启用双重验证、是否需要商务验证等要求，以 Meta 当时页面为准。

## 搭建与复核时的判断清单

新建结构时按顺序处理：先确认组合的主体信息与管理员，再把主页和广告账户归入组合，然后接入数据集与 Instagram 账号，最后才添加人员和合作伙伴。复核时逐项回答：每个资产的所有者是谁；每个人在每个资产上的任务是否超出岗位所需；外部合作方的共享是否仍有对应合同；离职人员是否已从组合移除。如果某个资产的所有权不清楚，或你无法证明自己有权共享它，应先停止新增投放并理清归属，而不是继续叠加权限。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Meta Ad Accounts, Business Portfolios and Pages: Ownership, Access and Task Permissions Explained

This file mirrors the site article. Official page: [Meta Ad Accounts, Business Portfolios and Pages: Ownership, Access and Task Permissions Explained](https://fatevia.cn/en/blog/articles/meta-business-portfolio-ad-account-page-permissions.html)

How ad accounts and Pages sit inside a Meta business portfolio (formerly Business Manager), and how people, partners and task-level permissions differ, so your access structure stays recoverable.

## A portfolio is the container; ad accounts and Pages are assets inside it

The business portfolio (earlier called Business Manager, often shortened to BM) is the management layer. Ad accounts, Pages, Instagram accounts, datasets (pixels) and catalogs are assets it holds. A Page is the public identity an ad runs under. An ad account handles delivery, billing and reporting. They connect because an ad needs a Page or Instagram account as its publishing identity, but neither one belongs to the other. To judge whether a setup is sound, ask who owns each asset, who can access it, and which tasks that access covers.

## Ownership is singular; access can be plural

An ad account or Page can be owned by only one portfolio at a time. Everyone else, whether another portfolio or an individual, uses it through granted access. That distinction has consequences. The owning portfolio keeps final control, including adding or removing admins and revoking shares. A party with full task access still cannot take the asset away from its owner. If you are an agency using a client's Page and ad account, you should be clear you are not the owner. If you place core assets under someone else's portfolio, you hand over control. Whether and how an asset can be moved depends on the prompts in the interface at that time, so do not assume it is freely transferable.

## People permissions work on two layers: portfolio role and asset task

Granting access to a person involves two layers. The first is the portfolio-level role, typically admin or employee access. Admins manage settings and people across the portfolio, while employees see only assigned assets. The second is asset-level tasks. On an ad account that might be managing the account, creating ads, or viewing performance. On a Page it could be managing the Page, creating ads, posting content or viewing insights. A frequent mistake is granting ad account access but not the Page's create-ads task, so no Page appears when setting up an ad. When a Page or account is missing from a dropdown, check both layers before clearing caches or switching browsers.

## Partner sharing grants access to a business, not to individuals

For agencies, contractors or affiliated companies, adding a partner is usually cleaner. You share specific assets with the other party's portfolio ID, and their admin then assigns them to their own staff. When people leave on their side, you do not edit individual users; you only revoke the share to that portfolio. The task scope you share is the ceiling. The partner's admin can distribute within it but not beyond. Giving an agency only create-ads and view-performance tasks is generally safer than full management, since the latter may touch payment methods and spending limits.

## Structures that leave you unable to recover

Several patterns recur. The portfolio has a single admin, and that person's account is restricted or they leave. A Page sits only under one employee's personal identity rather than inside the portfolio, so it disappears with that account. The payment method and the Page are held by different parties, which makes disputes hard to resolve. Every asset lives in one portfolio, so a review or restriction hits the entire business. A practical baseline is at least two active admins who are real, current staff, core Pages and ad accounts owned by the portfolio, and permissions assigned by role and reviewed regularly. Requirements such as two-factor authentication or business verification follow Meta's current pages.

## A checklist for setup and periodic review

When building the structure, work in this order: confirm the business details and admins, bring in Pages and ad accounts, connect datasets and Instagram accounts, then add people and partners. When reviewing, answer each of these: who owns each asset, whether each person's tasks exceed what their job needs, whether every external share still has a matching contract, and whether departed staff have been removed. If you cannot establish who owns an asset, or cannot show you are entitled to share it, pause new campaigns and sort out ownership first rather than layering on more permissions.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
