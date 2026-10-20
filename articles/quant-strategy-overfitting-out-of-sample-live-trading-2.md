---
title: "量化策略是不是过拟合：样本外回测达不到，就不该上实盘吗"
canonical: https://fatevia.cn/blog/articles/quant-strategy-overfitting-out-of-sample-live-trading-2.html
canonical_en: https://fatevia.cn/en/blog/articles/quant-strategy-overfitting-out-of-sample-live-trading-2.html
date: 2026-10-20
source: https://fatevia.cn/
---

# 量化策略是不是过拟合：样本外回测达不到，就不该上实盘吗

本文件是网站文章的副本。正式页面：[量化策略是不是过拟合：样本外回测达不到，就不该上实盘吗](https://fatevia.cn/blog/articles/quant-strategy-overfitting-out-of-sample-live-trading-2.html)

样本外表现明显差于样本内，不一定等于过拟合，也不等于一票否决。本文说明如何区分过拟合、市场状态切换与样本外设计缺陷，并给出上实盘、缩小仓位或退回研究的判断标准。

## 「样本外达不到就不上实盘」这句话只对了一半

这个原则的方向是对的：没有经过未参与调参数据检验的策略，没有理由承担真实资金风险。但「达不到」本身需要先定义。达不到样本内的收益，几乎是常态，因为样本内结果包含了选参时的乐观偏差。真正要问的是三件事：样本外是否仍有正的期望，衰减幅度是否在预期范围内，亏损形态是否与样本内逻辑一致。只要这三条有一条不成立，才需要认真考虑不上线。

## 先确认样本外本身是干净的

很多「样本外失败」或「样本外成功」都不可信，因为样本外已经被污染。常见污染路径包括：看过样本外结果后又回头改参数或改规则；用全样本计算标准化、分位数、特征筛选；多个策略变体都在同一段样本外上挑最好的一个；样本外区间太短，只覆盖单一行情。一旦发生其中任何一种，这段数据就退化成了第二个样本内。此时的结论不是「策略过拟合」，而是「还没有可用的样本外」，需要重新留出一段从未接触的数据，或改用前向验证。

## 过拟合、市场状态切换和成本模型错误，表现不同

样本外变差有三类常见根因，处理方式完全不同。过拟合的特征是：参数稍作扰动，绩效就大幅塌陷；收益集中在少数几笔交易或少数几个标的；规则越调越复杂，却讲不出经济或结构上的理由。状态切换的特征是：参数邻域仍然平滑，但策略依赖的波动率、流动性或趋势属性在样本外消失，亏损集中在某个明确的行情段。成本模型错误的特征是：信号层面的毛收益与样本内接近，差距主要出现在手续费、滑点和成交假设上，高换手策略尤其明显。把这三者混为一谈，会导致该修成本模型的去重新调参，该放弃的却继续加规则。

## 用参数稳定性和前向验证替代单次切分

单次样本内外切分只给出一个结果，运气成分很大。更有信息量的做法是滚动前向验证：每个窗口用历史选参，用紧随其后的未见数据评估，把所有窗口的样本外结果拼接起来看分布，而不是看某一个数字。同时画出关键参数的绩效热力图。若最优点孤立、四周迅速变差，说明选中的是噪声；若存在一片相对平坦的高原，即使最优点略有偏移，策略也更可能稳健。参数个数、尝试过的组合数量也要记录，因为试得越多，最好那一个越可能只是多重比较的产物。

## 给上线决定一套可执行的标准

可以事先写下通过条件，再去看结果，避免事后合理化。一组实用的判断：样本外夏普或收益保留了样本内的相当一部分，具体比例由你的风险偏好决定，但要在看结果前确定；滚动窗口中多数窗口为正，而不是靠一两个窗口拉高；参数邻域内绩效平滑；扣除保守估计的成本后仍有正期望；最大回撤落在你能承受的范围内。全部满足可以按计划仓位上线；部分满足可以用很小的资金跑实盘，把它当作最后一段样本外；核心条件不满足，则应退回研究，而不是靠加规则补救。

## 小资金实盘不是回测的替代品，但可以是最后一道检验

在回测证据不够强但逻辑成立时，小规模、限定亏损上限的实盘可以暴露回测看不到的问题：撮合延迟、拒单、行情断档、容量限制。前提是事先写好止损规则和观察周期，并把实盘结果与回测在同口径下逐笔对账。若实盘偏差能被成本和成交假设解释，说明回测框架可信；若偏差无法解释，应暂停并排查，而不是加仓。这一步的目的是验证流程，不是用真钱继续调参。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Is Your Quant Strategy Overfit? What to Do When Out-of-Sample Results Fall Short

This file mirrors the site article. Official page: [Is Your Quant Strategy Overfit? What to Do When Out-of-Sample Results Fall Short](https://fatevia.cn/en/blog/articles/quant-strategy-overfitting-out-of-sample-live-trading-2.html)

Weak out-of-sample results do not automatically mean overfitting, and they are not always a veto. This article separates overfitting from regime change and cost-model errors, and sets criteria for going live, sizing down, or returning to research.

## The rule is right in direction, but incomplete

Not risking real capital on a strategy that has never faced unseen data is sound. But "falling short" needs a definition. Performance below the in-sample figure is normal, because in-sample results carry the optimism of parameter selection. The useful questions are whether out-of-sample expectancy is still positive, whether the decay is within a range you anticipated, and whether losses look like what the strategy's logic would predict. Failing on one of these is what justifies holding back.

## Confirm the out-of-sample set is actually clean

Many out-of-sample verdicts, good or bad, are unreliable because the data has been contaminated. Typical routes: tweaking rules after seeing the out-of-sample result; computing normalization, quantiles, or feature selection on the full dataset; choosing the best of several variants on the same holdout; using a window so short it covers a single market condition. In any of these cases the holdout has become a second in-sample set. The conclusion is not that the strategy is overfit, but that you do not yet have a valid out-of-sample test, and need fresh untouched data or forward validation.

## Overfitting, regime change, and cost errors look different

There are three common causes of a weaker out-of-sample run, and each calls for a different response. Overfitting shows up as performance collapsing under small parameter changes, profit concentrated in a handful of trades or symbols, and rules that keep growing without a structural rationale. Regime change shows up as a smooth parameter neighborhood but a missing property in the new data, such as volatility, liquidity, or trend persistence, with losses clustered in an identifiable period. Cost-model error shows up as gross signal returns resembling in-sample, with the gap appearing after fees, slippage, and fill assumptions, especially at high turnover. Treating them as one problem leads to retuning what needed a cost fix and patching what should have been dropped.

## Use parameter stability and walk-forward instead of one split

A single split yields one number with a lot of luck in it. Walk-forward validation is more informative: select parameters on past data, evaluate on the data immediately after, repeat, and examine the distribution of stitched-together out-of-sample results. Also plot a performance heatmap over key parameters. An isolated peak with poor surroundings suggests noise was selected; a broad plateau suggests robustness even if the optimum shifts. Record how many parameters and combinations you tried, since the best of many trials can be a multiple-comparison artifact.

## Write the go-live criteria before looking at results

Setting conditions in advance avoids rationalizing afterward. A practical set: out-of-sample performance retains a meaningful share of in-sample performance, with the threshold fixed beforehand according to your risk tolerance; most walk-forward windows are positive rather than one or two carrying the total; the parameter neighborhood is smooth; expectancy stays positive after conservative costs; and maximum drawdown is within what you can bear. If all hold, deploy at planned size. If some hold, run a very small live allocation as a final out-of-sample stage. If core conditions fail, go back to research rather than adding rules to patch it.

## Small live trading checks the process, not the edge alone

When backtest evidence is moderate but the logic is sound, a small live run with a hard loss cap can expose what backtests cannot: latency, rejected orders, data gaps, capacity limits. Fix stop rules and an observation period in advance, and reconcile live fills against the backtest trade by trade under the same assumptions. If differences are explained by costs and execution, the framework is credible. If they are not, pause and investigate instead of scaling up. The purpose is validating the pipeline, not continuing to tune with real money.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
