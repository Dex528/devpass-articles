---
title: "App Store 小企业计划怎么申请：佣金降到 15% 要满足什么条件"
canonical: https://fatevia.cn/blog/articles/app-store-small-business-program-15-percent-commission-2.html
canonical_en: https://fatevia.cn/en/blog/articles/app-store-small-business-program-15-percent-commission-2.html
date: 2026-10-18
source: https://fatevia.cn/
---

# App Store 小企业计划怎么申请：佣金降到 15% 要满足什么条件

本文件是网站文章的副本。正式页面：[App Store 小企业计划怎么申请：佣金降到 15% 要满足什么条件](https://fatevia.cn/blog/articles/app-store-small-business-program-15-percent-commission-2.html)

App Store 小企业计划把符合条件的佣金从 30% 降到 15%。本文说明 100 万美元门槛的收益口径、关联账号合并计算、Account Holder 注册步骤，以及超线后的处理方式。

## 门槛看的是上一年度开发者收益，不是流水

小企业计划的核心条件是：上一个自然年内，你名下所有应用在 App Store 产生的「开发者收益」（proceeds）不超过 100 万美元。符合后，付费应用和应用内购买的佣金从标准的 30% 降到 15%。这里的比较对象是你实际拿到手的金额，而不是用户支付的总额。具体数字和适用范围以 Apple 当时的官方页面与协议为准，下面的判断都基于这个口径。

## 收益口径：扣掉税和佣金之后才算

开发者收益是用户付款扣除税费（如增值税、销售税）和 Apple 佣金之后归你的部分。按 30% 佣金粗略换算，100 万美元收益对应约 143 万美元的不含税用户付款。所以一个应用「年流水 120 万美元」并不一定超线，反过来也不能拿后台的总销售额直接和 100 万比较。判断时应使用 App Store Connect 销售与趋势或财务报告里的 proceeds 字段，并统一币种口径，不要自己用流水估算。

## 注册由 Account Holder 在 App Store Connect 完成

这个计划不是人工审核企业规模，而是资格确认加主动注册。符合条件的开发者需要由 Account Holder 登录 App Store Connect，在计划注册入口提交确认，入口位置和页面名称随后台改版可能变化，以当前界面为准。注册前先确认两件事：付费应用协议（Paid Applications Agreement）处于有效状态，税务与银行信息已完整。Admin 或 App Manager 角色通常无法代为提交，所以团队里要提前确认谁持有 Account Holder。

## 关联账号会合并计算，拆账号规避没有意义

如果多个开发者账号之间存在关联，例如同一实体控制或共同持有，Apple 会把它们的收益合并计算，并要求注册时如实说明。常见的错误是把一个产品线拆到两个主体名下，以为各自都在 100 万以内。一旦被认定为关联，合并收益超线，已享受的优惠可能被取消。应用转让进来时也要留意，被转让应用的收益记录会影响接收方的统计，这类细节应在转让前对照官方说明确认。

## 生效时间与超线后的处理：看当年累计，不是看上年

注册通过后，15% 通常从下一个财务月开始适用于符合条件的交易，不会回溯已结算的月份。已加入的开发者如果当年累计收益超过 100 万美元，从超线之后的交易开始恢复标准佣金，直到年底。下一年能否继续享受，取决于这一年的总收益是否仍高于门槛。新开发者没有上年数据时，一般按当年累计收益判断。所以接近门槛时，应按月跟踪当年累计，而不是等年底再算。

## 不符合条件时，先看有没有其他 15% 路径

收益超过门槛并不意味着只能全程 30%。订阅在同一订阅者付费满一年后，佣金本来就会降到 15%，这和小企业计划是两套规则，可以分别核算。Apple 另有针对特定类型内容的合作计划，是否适用取决于应用类别与资格，不应默认套用。判断标准很直接：如果上年收益明显低于 100 万美元，就应注册；如果已接近门槛，就要做收益预测，并评估是否值得为了短期优惠调整定价或发布节奏。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# How to Apply for the App Store Small Business Program and Get the 15% Commission

This file mirrors the site article. Official page: [How to Apply for the App Store Small Business Program and Get the 15% Commission](https://fatevia.cn/en/blog/articles/app-store-small-business-program-15-percent-commission-2.html)

What the App Store Small Business Program actually requires: the $1M proceeds test, how associated accounts are combined, who can enroll, and what changes when you pass the cap.

## The test is prior-year proceeds, not gross sales

The program reduces the standard 30% commission to 15% on paid apps and in-app purchases if your total App Store proceeds in the previous calendar year were $1 million or less. The comparison uses what you receive, not what customers pay. Exact thresholds and scope can change, so treat Apple's current program page and agreement as the authority.

## Proceeds exclude taxes and Apple's commission

Proceeds are customer payments minus taxes such as VAT or sales tax, and minus Apple's commission. At a 30% rate, $1 million in proceeds corresponds to roughly $1.43 million in pre-tax customer spend. So an app with $1.2 million in gross billings may still qualify, and the reverse reading of your dashboard is a common mistake. Use the proceeds fields in Sales and Trends or the financial reports, in one consistent currency, rather than estimating from revenue.

## Enrollment is done by the Account Holder in App Store Connect

This is not a manual review of your company size. Eligible developers opt in through App Store Connect, and the Account Holder is the role that submits it. The menu location may change with interface updates. Before you start, confirm that the Paid Applications Agreement is active and that tax and banking details are complete. If your team does not know who holds the Account Holder role, settle that first, because Admin or App Manager roles typically cannot enroll on its behalf.

## Associated accounts are combined, so splitting entities does not help

If developer accounts are associated, for example under common ownership or control, their proceeds are aggregated and you are expected to disclose the relationship at enrollment. Splitting one product line across two entities to stay under the cap is a fragile plan: if Apple treats them as associated, the combined total can disqualify you. App transfers deserve a check too, since the transferred app's revenue history can affect the receiving account's totals. Confirm the rules before you transfer.

## Timing and the cap: track the current year, not just last year

After approval, the 15% rate normally applies to eligible transactions from the next fiscal month and is not backdated to settled periods. If your proceeds in the current year pass $1 million, transactions after that point revert to the standard rate for the rest of the year. Whether you qualify the following year depends on that year's total. New developers without prior-year data are generally assessed on current-year proceeds. If you are near the line, review cumulative proceeds monthly instead of waiting for year end.

## If you do not qualify, check the other 15% paths

Passing the cap does not mean every sale stays at 30%. Subscriptions already drop to 15% once a subscriber has paid for more than a year, and that rule is separate from the Small Business Program, so model them separately. Apple also runs partner programs for certain content types, and eligibility depends on category, so do not assume they apply. A practical rule: enroll if last year was comfortably under $1 million, and if you are close, forecast proceeds before you adjust pricing or release plans around the discount.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
