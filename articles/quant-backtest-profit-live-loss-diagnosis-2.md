---
title: "量化策略回测赚钱、实盘亏钱怎么查：滑点、手续费、延迟和未来函数"
canonical: https://fatevia.cn/blog/articles/quant-backtest-profit-live-loss-diagnosis-2.html
canonical_en: https://fatevia.cn/en/blog/articles/quant-backtest-profit-live-loss-diagnosis-2.html
date: 2026-10-18
source: https://fatevia.cn/
---

# 量化策略回测赚钱、实盘亏钱怎么查：滑点、手续费、延迟和未来函数

本文件是网站文章的副本。正式页面：[量化策略回测赚钱、实盘亏钱怎么查：滑点、手续费、延迟和未来函数](https://fatevia.cn/blog/articles/quant-backtest-profit-live-loss-diagnosis-2.html)

回测盈利而实盘亏损时，按未来函数、成交假设、手续费、滑点和延迟五类原因逐层拆解，并给出用实盘成交记录反推差异的判断标准与停止条件。

## 先区分：是信号失效，还是执行损耗

回测赚钱、实盘亏钱，原因只有两大类。一类是回测本身不可信，比如未来函数、幸存者偏差、过拟合，这类问题意味着策略从来没有真实的优势。另一类是策略有优势，但被成交价差、手续费、延迟和冲击成本吃掉。两类的处理完全不同：前者要推倒重来，后者可以优化执行。排查的第一步不是改参数，而是把同一时间段的实盘成交记录和回测成交记录逐笔对齐，看差距出现在信号层（该不该下单）还是成交层（以什么价格、什么时间成交）。

## 未来函数：回测好得不正常时先查它

未来函数指策略在某个时点使用了当时尚不可得的信息。常见形式包括：用当根K线的收盘价、最高价、最低价生成信号，又在同一根K线的开盘或收盘价成交；指标计算中使用了后续数据重新修正的值，如部分平台会回溯修改的指标或财报数据；复权处理不当，用后复权价格直接判断历史价位；跨周期取数时，高周期K线在未收盘前就被当作已完成。判断方法：把信号时点强制后移一根K线重跑，如果收益大幅塌陷，基本可以确认存在前视。另一个信号是回测曲线过于平滑、胜率和盈亏比同时异常高，这类结果应先怀疑数据与逻辑，而不是庆幸。

## 成交假设：回测默认你总能按理想价格成交

很多回测框架默认以信号K线的收盘价或下一根开盘价成交，且不限量。实盘中，限价单可能挂不上，市价单会吃掉盘口多档，价格跳空时止损单的成交价远差于触发价。对高频换手、小市值、低流动性品种，这个差距可以大于全部策略利润。检查要点：回测使用的是什么价格成交、是否考虑盘口深度和成交量上限、限价单是否按「价格触及即成交」处理。后者会系统性高估被动成交率，因为价格触及你的挂单价不代表你排在队列前面。

## 手续费与滑点：用换手率算出盈亏平衡点

手续费包括佣金、印花税或交易所费率、资金费率、融资利息等，各市场构成不同，具体以交易所和券商当时公布的费率为准。滑点则是预期价与实际成交价的差。两者的共同特征是与交易次数成正比，所以最直接的判断方式是计算单笔交易的平均毛利，再与单笔往返成本对比。如果回测里平均每笔毛利只比往返成本高一点点，那么实盘成本略有偏差，策略就会由盈转亏。可以做敏感性测试：把手续费和滑点在回测中分别加到原假设的两倍，若收益曲线转负，说明策略对成本几乎没有安全边际，不适合直接实盘。

## 延迟：把信号到成交的时间链拆开量

延迟不只是网络延迟，而是一条链：行情到达、策略计算、风控检查、报单发出、交易所撮合、回报确认。在日线或小时级策略上，这些毫秒到秒级的延迟通常可以忽略；在分钟以下、依赖价差或盘口变化的策略上，延迟可能直接决定盈亏方向。排查方法是在实盘日志里记录每一笔单的信号时间戳、发单时间戳、成交时间戳，统计分位数而不只是平均值，因为尾部延迟常在行情剧烈波动时出现，恰好是策略最需要成交的时刻。然后在回测中人为加入同样分布的延迟重跑，看收益保留多少。

## 过拟合与样本问题：成本都对得上仍然亏时

如果逐笔对账后发现信号一致、成交价差和费用也在预期内，实盘依然持续亏损，问题多半在策略本身：参数是在同一段样本上反复调优得出的，或策略只适应了特定行情阶段。判断标准包括：参数在邻近取值上表现是否稳定，而不是只有某个孤立值有效；样本外和滚动前向测试是否仍盈利；收益是否集中在少数几笔极端交易上。实盘时间还很短时也要谨慎，几十笔交易的盈亏波动本身可能就足以解释偏差，不应据此得出结论。

## 何时停止、何时修正

可以用三个条件判断下一步。第一，实盘与回测的逐笔信号不一致，先修复数据和逻辑，暂停加仓。第二，信号一致但成交成本显著高于假设，更新回测成本模型后重新评估，若盈亏平衡消失则降低换手或放弃该策略。第三，成本与延迟都吻合、亏损幅度仍在回测最大回撤范围内，则可能只是正常波动，应保持小仓位继续观察，同时事先设定超出回测回撤多少即停机的阈值。无论哪种情形，都不要在没有定位原因前反复调参，这会进一步加重过拟合。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Backtest Profitable, Live Trading Losing: Checking Slippage, Fees, Latency and Look-Ahead Bias

This file mirrors the site article. Official page: [Backtest Profitable, Live Trading Losing: Checking Slippage, Fees, Latency and Look-Ahead Bias](https://fatevia.cn/en/blog/articles/quant-backtest-profit-live-loss-diagnosis-2.html)

When a quant strategy profits in backtests but loses live, work through look-ahead bias, fill assumptions, fees, slippage and latency, and use trade-level reconciliation to decide whether to fix or stop.

## Separate a broken signal from execution leakage

A strategy that wins in backtests and loses live has one of two problems. Either the backtest was never valid, through look-ahead bias, survivorship bias or overfitting, so no real edge existed. Or the edge is real but fills, fees, latency and market impact consume it. The responses differ: the first means rebuilding, the second means improving execution. Before changing any parameter, align live fills with backtest fills trade by trade over the same period and see whether the gap appears at the signal layer (should an order exist) or the fill layer (price and timing).

## Look-ahead bias: suspect it first when results look too good

Look-ahead bias means the strategy used information that was not available at decision time. Typical forms: generating a signal from the current bar's close, high or low and filling on that same bar; using indicators or fundamentals that get revised later; applying back-adjusted prices to judge historical price levels; and reading a higher-timeframe bar before it has closed. A quick test is to delay every signal by one bar and rerun. If performance collapses, look-ahead is almost certain. An unusually smooth curve with both a high win rate and a high payoff ratio is another warning sign to audit data and logic before celebrating.

## Fill assumptions: backtests often assume ideal prices

Many frameworks fill at the signal bar's close or the next open, with no size limit. Live, limit orders may never fill, market orders walk the book, and stop orders gap through their trigger. For high-turnover or illiquid instruments this gap can exceed the entire profit. Check what price the backtest fills at, whether it respects depth and volume, and how limit orders are treated. Treating a limit order as filled whenever price touches it systematically overstates passive fills, because touching your price does not mean you were first in the queue.

## Fees and slippage: compute the break-even from turnover

Costs include commissions, exchange fees, taxes where applicable, funding rates and financing interest, and they vary by venue, so use the broker or exchange's current schedule. Slippage is the difference between expected and actual fill price. Both scale with trade count, so compare average gross profit per trade with round-trip cost. If the margin is thin, small errors flip the sign. Run a sensitivity test with fees and slippage at twice the assumption. If the curve turns negative, the strategy has almost no cost buffer and is not ready for live capital.

## Latency: measure the whole chain, not just the network

Latency is a chain: market data arrival, strategy computation, risk checks, order submission, exchange matching and acknowledgement. For daily or hourly strategies it is usually negligible. For sub-minute strategies that depend on spreads or order book changes, it can decide the sign of returns. Log the signal, submit and fill timestamps for each order and look at percentiles rather than averages, since tail delays tend to occur in fast markets, exactly when fills matter most. Then inject a similar delay distribution into the backtest and see how much return survives.

## Overfitting and sample size: when costs reconcile but losses continue

If signals match, fill prices and fees are within expectation, and live results still lag, the strategy itself is the likely cause. Parameters may have been tuned repeatedly on one sample, or the logic fits only one market regime. Useful tests: whether neighboring parameter values also perform, rather than a single isolated optimum; whether out-of-sample and walk-forward results stay profitable; and whether profit depends on a handful of extreme trades. Also be careful with very short live histories. A few dozen trades can produce deviations that noise alone explains.

## When to stop and when to fix

Three conditions guide the next step. If live and backtest signals disagree, fix data and logic and pause scaling. If signals match but realized costs are well above assumptions, update the cost model, re-evaluate, and cut turnover or drop the strategy if break-even disappears. If costs and latency match and losses stay within the backtested drawdown range, it may be normal variance: keep size small and set a stop threshold in advance for drawdown beyond the backtest range. In every case, avoid repeated parameter tuning before the cause is located, since that deepens overfitting.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
