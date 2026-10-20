---
title: "量化实盘交易所 API 权限怎么开：只给交易不给提币，密钥泄露后怎么停"
canonical: https://fatevia.cn/blog/articles/exchange-api-trade-only-permission-leak-response-2.html
canonical_en: https://fatevia.cn/en/blog/articles/exchange-api-trade-only-permission-leak-response-2.html
date: 2026-10-20
source: https://fatevia.cn/
---

# 量化实盘交易所 API 权限怎么开：只给交易不给提币，密钥泄露后怎么停

本文件是网站文章的副本。正式页面：[量化实盘交易所 API 权限怎么开：只给交易不给提币，密钥泄露后怎么停](https://fatevia.cn/blog/articles/exchange-api-trade-only-permission-leak-response-2.html)

说明量化实盘创建交易所 API 密钥时哪些权限该开、哪些必须关，以及 IP 白名单、子账户隔离的作用，并给出密钥泄露后的处理顺序和判断标准。

## 「不给提币」只能挡住直接转走资金，挡不住亏损

关闭提币权限后，攻击者无法通过 API 把资产直接转到外部地址，这是必要条件，但不是充分条件。拿到交易权限的人仍可以在流动性差的合约或币对上挂出对自己有利的价格，再用你的账户去成交，让资金以亏损的方式流向对手账户。如果密钥同时带有划转权限，风险更大：资金可能被转进其他子账户或资金账户，再由别的路径处理。因此权限设计的目标不是「禁止提币」这一条，而是把一把密钥能造成的最大损失限制在一个你能接受的范围内。

## 创建密钥时的权限勾选清单

各交易所的权限名称和分组不同，以创建页面当时的说明为准，但思路相通。读取类权限（查询余额、订单、成交）按需开启。交易类权限只开策略实际用到的品种：只做现货就不要开杠杆和合约，只做 U 本位合约就不要开现货与借贷。提币权限必须关闭。内部划转、子账户划转、资金账户转账这类权限，除非策略需要自动调拨资金，否则同样关闭。部分交易所对修改杠杆、保证金模式、借币还币也有单独开关，没用到就不开。创建后回到详情页逐项核对，不要只依赖默认值。

## IP 白名单和子账户隔离各自解决什么问题

IP 白名单解决的是「密钥被偷走后能否在别处使用」。绑定后，只有来自指定出口 IP 的请求才会被受理。前提是你的出口 IP 固定：云服务器要使用固定公网地址，经过 NAT 网关的要绑定网关出口，家庭宽带动态 IP 不适合做白名单。子账户隔离解决的是「密钥泄露后损失上限」。把单个策略的资金放进独立子账户，只在该子账户下创建密钥，主账户资金不受牵连。两者作用不同：白名单降低被利用的概率，子账户限制被利用后的损失，建议同时使用，而不是二选一。

## 密钥怎么存、怎么用，决定了多数泄露是否会发生

实盘中的泄露多数来自工程环节，而不是交易所被攻破。常见路径包括：密钥写进代码仓库或配置文件后被推送；日志把完整请求头或签名参数打印出来；把策略代码和密钥一起发给第三方协助调试；在多台机器上复制同一份密钥，其中一台安全性较差。做法上，密钥放入环境变量或专用密钥管理服务，不进版本库；日志对密钥和签名字段做脱敏；每个策略、每个环境使用独立密钥，这样撤销一把不会让其他策略停摆；监控面板使用只读密钥，不复用交易密钥。

## 发现泄露后先撤销密钥，再查原因

处理顺序比排查本身更重要。第一步，登录交易所网页端，直接删除或禁用该密钥，而不是先分析来源，因为攻击者可能仍在下单。第二步，撤销密钥不会自动撤掉已有挂单或平掉持仓，需要在网页端检查并处理未成交委托和当前仓位，必要时用只减仓方式降低风险敞口。第三步，核对 API 调用记录、登录记录和成交明细，确认异常下单的时间与来源 IP。第四步，如果怀疑不止一把密钥泄露，或者账户本身可能被入侵，同时修改登录密码、重置 2FA、检查提币地址白名单。第五步，找到泄露来源并修复后，再创建新密钥，否则新密钥会再次暴露。若有资金损失，尽快保留记录并联系交易所客服，能否追回取决于交易所与具体情况。

## 哪些现象应当立即撤销，而不是继续观察

满足以下任一条件就应直接撤销密钥：出现不在策略日志里的委托或成交；API 记录里有不在白名单内的来源 IP；账户权限被改动而你没有操作；收到交易所的异常登录或密钥使用提醒；密钥已出现在公开仓库、聊天记录或截图中，即使暂时没有异常。判断依据是「无法解释的操作」，而不是「已经亏损」。撤销一把密钥的代价通常只是策略暂停几分钟，继续观察的代价可能是整个子账户的资金。

## 上线前做一次撤销演练，并把风控放在交易所之外

交易所不会替你限制单笔下单规模或每日亏损，所以策略侧需要自己的限额：单笔最大名义金额、单日最大亏损、异常报价偏离检查、下单频率上限，触发后自动停止下单。上线前把撤销流程完整走一遍：确认谁有权限在网页端删除密钥，确认备用设备能登录交易所，确认重建密钥与重启策略需要多久。这个耗时就是你真实的应急响应时间。如果演练发现无法在短时间内完成撤销，应先缩小子账户资金规模，再继续实盘。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Exchange API Permissions for Live Quant Trading: Trade-Only Keys and What to Do After a Leak

This file mirrors the site article. Official page: [Exchange API Permissions for Live Quant Trading: Trade-Only Keys and What to Do After a Leak](https://fatevia.cn/en/blog/articles/exchange-api-trade-only-permission-leak-response-2.html)

How to set exchange API permissions for live quant trading: which scopes to enable, which to keep off, how IP allowlists and sub-accounts limit damage, and the order of steps after a key leak.

## Disabling withdrawals stops direct transfers, not losses

With withdrawal permission off, an attacker cannot send your assets to an external address through the API. That is necessary but not sufficient. Someone holding a trade-enabled key can still quote a favorable price on an illiquid pair or contract and fill against it from your account, moving value to their counterparty as a trading loss. If the key also carries transfer permission, funds may be moved into other sub-accounts first. The real design goal is to cap the worst-case damage a single key can cause, with withdrawal off as only one of several controls.

## A permission checklist for key creation

Scope names and groupings differ between exchanges, so follow the description on the creation page at the time. The logic is consistent. Enable read access only as needed. Enable trading only for the product the strategy actually uses: no margin or derivatives for a spot-only bot, no spot or borrowing for a perpetuals-only bot. Keep withdrawal off. Keep internal transfer, sub-account transfer and fund-account transfer off unless the strategy rebalances capital automatically. Some exchanges also have separate switches for leverage, margin mode and borrow/repay; leave unused ones off. After creating the key, reopen its detail page and verify each item instead of trusting defaults.

## What an IP allowlist does versus what a sub-account does

An allowlist answers whether a stolen key can be used from somewhere else. Once bound, only requests from the listed egress IPs are accepted. It requires a stable address: a cloud server needs a fixed public IP, a NAT setup needs the gateway's egress address, and a residential dynamic IP is a poor fit. A sub-account answers how much can be lost if the key is abused. Put each strategy's capital in its own sub-account and create the key there, so the main account is not exposed. They solve different problems: the allowlist reduces the chance of misuse, the sub-account limits the loss when misuse happens. Use both.

## Most leaks come from engineering habits, not the exchange

Typical paths: a key committed to a repository, logs that print full request headers or signed parameters, strategy code shared with a third party along with its credentials, and the same key copied to several machines, one of them weakly protected. In practice, store keys in environment variables or a secrets manager, never in version control. Redact key and signature fields in logs. Use a separate key per strategy and per environment so revoking one does not halt everything. Give dashboards a read-only key rather than reusing the trading key.

## After a suspected leak, revoke first and investigate second

The order matters. First, log in to the exchange web interface and delete or disable the key; do not analyze first, because the attacker may still be placing orders. Second, revoking a key does not cancel existing orders or close positions, so check open orders and positions manually and reduce exposure, using reduce-only orders if available. Third, review API call logs, login history and fills to identify the time and source IP of unusual activity. Fourth, if more than one key or the account itself may be compromised, change the password, reset 2FA and check the withdrawal address allowlist. Fifth, find and fix the source of the leak before creating a replacement key, or the new one will be exposed too. If funds were lost, keep the records and contact exchange support promptly; recovery depends on the exchange and the circumstances.

## Signals that mean revoke now rather than keep watching

Revoke immediately if any of these appear: orders or fills that your strategy logs cannot explain; API records showing source IPs outside the allowlist; permission changes you did not make; an exchange alert about unusual login or key usage; or the key showing up in a public repository, chat or screenshot, even without visible damage. The test is whether you can explain the activity, not whether you have lost money yet. Revoking usually costs minutes of strategy downtime. Waiting can cost the whole sub-account balance.

## Run a revocation drill and keep risk limits outside the exchange

The exchange will not limit your order size or daily loss, so the strategy needs its own caps: maximum notional per order, maximum daily loss, a price-deviation check, and an order-rate limit that halts trading when breached. Before going live, rehearse the full response: who can delete the key in the web interface, whether a backup device can log in, and how long it takes to recreate the key and restart the strategy. That duration is your real response time. If the drill shows you cannot revoke quickly, reduce sub-account capital before trading live.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
