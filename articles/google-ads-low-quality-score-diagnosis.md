---
title: "Google Ads 搜索广告质量得分低怎么办？预期点击率、相关性与落地页体验解析"
canonical: https://fatevia.cn/blog/articles/google-ads-low-quality-score-diagnosis.html
canonical_en: https://fatevia.cn/en/blog/articles/google-ads-low-quality-score-diagnosis.html
date: 2026-10-15
source: https://fatevia.cn/
---

# Google Ads 搜索广告质量得分低怎么办？预期点击率、相关性与落地页体验解析

本文件是网站文章的副本。正式页面：[Google Ads 搜索广告质量得分低怎么办？预期点击率、相关性与落地页体验解析](https://fatevia.cn/blog/articles/google-ads-low-quality-score-diagnosis.html)

质量得分低不等于必须优化。本文说明质量得分与广告评级的关系,如何在关键词列中查看三项子指标,三项各自的常见根因,以及什么情况下应该修改、什么情况下可以不动。

## 先分清:质量得分是诊断指标,拍卖用的是广告评级

质量得分(1~10)是关键词层级的诊断参考,由预期点击率、广告相关性、落地页体验三项汇总而来。每次拍卖实际决定展示资格和排位的是广告评级,它由出价、拍卖时的广告质量、评级门槛、搜索情境以及素材和其他广告形式的预期影响共同决定。所以后台显示的 4 分不等于每次拍卖都按 4 分计算。它的价值在于告诉你哪一项质量信号偏弱,而不是一个需要拉高的 KPI。具体计算方式和展示口径以 Google Ads 帮助中心当时的说明为准。

## 先调出三项子状态,再决定改什么

在「关键词」页面点击列设置,从质量得分分组中加入质量得分、预期点击率、广告相关性、落地页体验四列。三项子状态分别显示为低于平均水平、平均水平、高于平均水平。如果需要对比变化,可以加入对应的历史列。新关键词或展示量很少的关键词常显示为空,这是数据不足,不是低分。排查时只看有一定展示量的关键词,并优先处理花费占比高的词,而不是按分数从低到高逐个改。

## 预期点击率偏低:通常是搜索意图与广告承诺没对上

预期点击率衡量的是在排除排位和素材展示形式影响后,这个关键词触发你广告时被点击的可能性。偏低时先看搜索字词报告:如果触发的是泛意图或无关查询,点击率信号自然被稀释,需要加否定关键词、收紧匹配方式。其次看标题是否直接回应了搜索者要找的东西,如具体型号、地区、价格区间或服务类型,而不是通用品牌话术。最后检查素材是否完整,多个标题和描述的组合是否足够多样。注意,仅把出价调高并不会改善这一项。

## 广告相关性偏低:广告组结构过宽是最常见的根因

广告相关性衡量广告文案与关键词意图的贴合程度。一个广告组里塞了十几个意图不同的关键词,广告就只能写成折中版本,结果对谁都不够贴。处理方式是按意图拆分广告组,让每组关键词共享一套有针对性的标题和描述。响应式搜索广告里,若固定位置的标题过多,会限制系统组合出与查询更贴合的版本,没有合规需要时不要把大多数标题都固定。关键词本身若是宽泛词,也要确认广告里确实出现了对应的核心概念。

## 落地页体验偏低:先查可用性,再查内容一致性

落地页体验关注的是访问者到达页面后能否顺利得到广告所承诺的内容。排查顺序建议是:移动端打开速度和是否有布局错位;页面内容是否与广告和关键词一致,而不是统一跳首页;导航和联系方式、公司信息、隐私政策等透明度要素是否清楚;是否有弹窗、强制注册等干扰进入的设计。修改页面后,状态不会立刻更新,需要积累新的流量数据才会重新评估,所以不要连续几天反复改动页面。

## 什么时候该动,什么时候该停

可以作为决策依据的标准有三条。第一,关键词展示量高、花费占比大且某一项子状态为低于平均水平,值得优先处理。第二,如果该关键词转化成本在目标范围内,三项即使只有一项偏低,也没必要为了分数大改。第三,品牌词或展示极少的长尾词,分数波动多半来自样本少,不值得投入。另外,如果你按上述顺序调整一轮后,同类关键词仍长期低于平均,应回头检查业务与关键词是否匹配,例如落地页根本没有对应的产品,这时换关键词或换页面比继续微调文案更有效。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Low Quality Score in Google Ads: Diagnosing Expected CTR, Ad Relevance and Landing Page Experience

This file mirrors the site article. Official page: [Low Quality Score in Google Ads: Diagnosing Expected CTR, Ad Relevance and Landing Page Experience](https://fatevia.cn/en/blog/articles/google-ads-low-quality-score-diagnosis.html)

A low Quality Score is not always a problem to fix. This article explains how it relates to Ad Rank, where to see the three component ratings, common root causes for each, and when to leave a keyword alone.

## Quality Score is a diagnostic; Ad Rank decides the auction

Quality Score (1 to 10) is a keyword-level diagnostic summarizing three components: expected CTR, ad relevance and landing page experience. What actually drives eligibility and position in each auction is Ad Rank, which combines your bid, ad quality at auction time, rank thresholds, search context, and the expected impact of assets and other ad formats. A reported 4 does not mean every auction treats you as a 4. Its use is to show which quality signal is weak, not to serve as a KPI to push up. For exact definitions, rely on the current Google Ads Help documentation.

## Pull the three component ratings before changing anything

On the Keywords page, open column settings and add Quality Score, Expected CTR, Ad relevance and Landing page experience from the Quality Score group. Each component shows Below average, Average or Above average. Historical versions of these columns help if you want to compare changes. New or low-volume keywords often show a blank, which means insufficient data rather than a low score. Focus on keywords with meaningful impressions and start with the ones that carry the most spend, not the lowest numbers.

## Low expected CTR usually means intent and ad promise are misaligned

Expected CTR estimates how likely your ad is to be clicked when triggered by this keyword, with the effects of position and ad format factored out. When it is below average, start with the search terms report. If broad or irrelevant queries trigger the ad, the click signal gets diluted, so add negatives and tighten match types. Next, check whether headlines answer what the searcher is looking for, such as a specific model, location or service type, rather than generic brand copy. Finally, make sure your assets are complete and varied enough. Raising the bid alone does not improve this component.

## Low ad relevance is most often an ad group structure problem

Ad relevance reflects how closely the ad copy matches the keyword's intent. When one ad group holds a dozen keywords with different intents, the ad has to be a compromise and fits none of them well. Split ad groups by intent so each shares copy written for it. In responsive search ads, pinning many headlines limits the system's ability to assemble versions matching the query, so avoid pinning most headlines unless compliance requires it. For broad keywords, confirm the core concept actually appears in the ad.

## Low landing page experience: check usability first, then consistency

Landing page experience looks at whether visitors can easily get what the ad promised. A practical order: mobile load speed and layout problems; whether the page content matches the ad and keyword instead of sending everyone to the homepage; transparency elements like contact details, company information and a privacy policy; and intrusive elements such as pop-ups or forced sign-ups. After you change a page, the rating does not update immediately because it needs new traffic data, so avoid editing the page repeatedly over a few days.

## When to act and when to stop

Three criteria help. First, a keyword with high impressions, a large share of spend and a Below average component is worth fixing first. Second, if its conversion cost is within target, one weak component is not a reason for a major rework. Third, brand terms and very low-volume long-tail keywords often swing because of small samples and are not worth the effort. If a round of changes in this order still leaves a group of keywords below average, revisit the match between business and keywords. If the landing page simply does not offer the product being searched, changing keywords or pages will do more than rewriting copy.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
