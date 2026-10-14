---
title: "iOS 与 Google Play 如何设置内购：商品类型、协议税务与服务端验证的对应关系"
canonical: https://fatevia.cn/blog/articles/ios-google-play-in-app-purchase-setup.html
canonical_en: https://fatevia.cn/en/blog/articles/ios-google-play-in-app-purchase-setup.html
date: 2026-10-14
source: https://fatevia.cn/
---

# iOS 与 Google Play 如何设置内购：商品类型、协议税务与服务端验证的对应关系

本文件是网站文章的副本。正式页面：[iOS 与 Google Play 如何设置内购：商品类型、协议税务与服务端验证的对应关系](https://fatevia.cn/blog/articles/ios-google-play-in-app-purchase-setup.html)

对比 App Store Connect 与 Google Play Console 的内购配置流程，说明商品类型选择、付费协议与税务前置条件、沙盒测试和服务端校验，帮助判断何时配置完整、何时可以上线。

## 先确认：内购配置在开发者账号之外还有三层前置条件

很多人搜这个词，以为注册了开发者账号就能直接建商品。实际上账号只是入口，内购要跑通还需要：协议与收款资料已生效、应用已创建且包名或 Bundle ID 与代码一致、客户端接入了对应的计费库。Apple 侧需要在 App Store Connect 的「协议、税务和银行业务」中签署付费应用协议并填好银行与税务信息；Google 侧需要在 Play Console 建立付款资料（merchant 账户），并让应用至少上传过一个带计费权限的构建。任一层缺失，常见现象是商品列表为空、状态一直不可用，而不是明确报错。

## 商品类型决定后续所有选择，建好后不能随意改

两个平台都把商品分为一次性购买和订阅，但细分不同。Apple 有消耗型、非消耗型、自动续期订阅和非续期订阅；Google Play 则分为一次性商品（可设置是否可重复购买）和订阅（订阅下再分基础方案与优惠）。选择标准看权益：用完即失的虚拟币类用消耗型或可重复购买的一次性商品；永久解锁功能用非消耗型或不可重复购买的一次性商品；持续提供服务才用订阅。商品 ID 一旦创建，通常不能删除后复用，命名建议带上类型和档位，避免日后出现语义混乱的 ID。

## App Store Connect 的配置路径与容易漏掉的字段

在对应 App 的「App 内购买项目」或「订阅」中创建商品，需要填写参考名称、产品 ID、价格档位、本地化的显示名称与描述，并上传审核用截图。订阅需先建订阅群组，同群组内的档位互为升降级关系，用户同一时间只能持有其中一个。商品状态若停在「缺少元数据」，通常是缺本地化信息或审核截图。首次提交内购时，商品需要随应用版本一起送审，并在版本页面的内购区域勾选关联；价格、可售地区也要确认已设置。

## Google Play Console 的配置路径与容易漏掉的字段

在「通过 Google Play 获利」下的产品区域创建一次性商品或订阅，填写产品 ID、名称、描述和价格。订阅需要配置基础方案（计费周期、是否自动续期、宽限期等），可再叠加免费试用或首期优惠。要点是：应用必须先上传包含 BILLING 相关依赖的构建到某个测试轨道，商品才能被客户端查询到；测试账号需加入许可测试名单，并通过测试轨道的链接安装，直接用调试包安装往往查不到商品。商品要设为有效状态才可售。

## 测试要分环境，别用生产结果判断配置是否正确

Apple 使用沙盒测试账号，或本地用 StoreKit 配置文件做离线调试；沙盒里的订阅周期会被压缩，不代表真实续期节奏。Google 使用许可测试人员，测试购买会标注为测试订单且不真实扣款。判断可以上线的标准至少包括：购买成功后权益正确发放；取消、失败和待处理状态不会误发权益；重复启动后能恢复已购内容；订阅在续期、过期、退款后权益能随之变化。只测到购买成功就上线，是内购问题最常见的来源。

## 服务端验证和状态同步，决定账务是否可靠

客户端返回的购买结果不应作为唯一依据。Apple 推荐使用 App Store Server API 与服务器通知来获取交易与订阅状态，Google 对应的是 Play Developer API 的购买校验，以及实时开发者通知（RTDN）。服务端应保存订单标识，做幂等处理，防止重复发放；Google 的一次性商品和订阅购买需要在规定时间内确认（acknowledge），否则会被自动退款，消耗型商品则要在发放后消耗。具体接口版本和时限以平台当前文档为准。纯单机、无账号体系的小应用可以先只做本地校验，但只要涉及跨设备权益或较高金额，就应该上服务端。

## 上线前的复核清单与停止条件

提交前逐项核对：协议与税务状态为有效；商品 ID 与代码常量完全一致；价格与地区符合预期；Apple 内购已随版本送审，Google 商品已激活；恢复购买入口可用（Apple 审核通常关注这一点）；订阅页面清晰展示价格、周期和续订说明。若沙盒或测试轨道仍查不到商品，先停下排查协议、包名和轨道状态，而不是反复改代码。平台费率、税务规则和审核要求会调整，涉及金额的部分以当时的平台页面为准。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Setting Up In-App Purchases on iOS and Google Play: Product Types, Agreements, and Server Validation

This file mirrors the site article. Official page: [Setting Up In-App Purchases on iOS and Google Play: Product Types, Agreements, and Server Validation](https://fatevia.cn/en/blog/articles/ios-google-play-in-app-purchase-setup.html)

A side-by-side look at configuring in-app purchases in App Store Connect and Google Play Console: product type choice, agreement and tax prerequisites, sandbox testing, and server-side verification before launch.

## Three prerequisites sit between a developer account and a working purchase

Having an account does not mean you can sell. Three things must be in place first: signed agreements with payout details, an app record whose Bundle ID or package name matches your code, and a billing library integrated in the client. On Apple, the Paid Applications agreement plus banking and tax information must be active in App Store Connect. On Google, you need a payments profile, and the app must have a build with billing permission uploaded. When one is missing, the usual symptom is an empty product list or an unavailable status, not a clear error message.

## Pick the product type by entitlement, because it is hard to change later

Both stores split products into one-time purchases and subscriptions, with different subcategories. Apple has consumable, non-consumable, auto-renewable, and non-renewing subscriptions. Google has one-time products (with an option for repeat purchase) and subscriptions with base plans and offers. Choose by what the user receives: currency that gets spent maps to consumable or repeatable products, a permanent unlock maps to non-consumable or single-purchase products, and ongoing service maps to subscriptions. Product IDs generally cannot be reused after deletion, so name them with type and tier to avoid confusion later.

## App Store Connect: where to configure and which fields get missed

Create products under the app's In-App Purchases or Subscriptions section. You need a reference name, product ID, price, localized display name and description, and a review screenshot. Subscriptions go into a subscription group, where tiers are mutually exclusive and define upgrade and downgrade paths. A status of Missing Metadata usually means a localization or screenshot gap. For a first submission, products must be attached to the app version in the in-app purchases section of that version page, or they will not go through review with it.

## Google Play Console: where to configure and which fields get missed

Create one-time products or subscriptions under the monetization area, with ID, name, description, and price. Subscriptions need a base plan covering billing period, renewal, and grace behavior, and can carry trials or introductory offers. The key constraint is that a build including the billing dependency must already be on a testing track, or the client cannot query products. Testers must be on the license testing list and install through the track link; a locally installed debug build often returns no products. Products must also be set to active.

## Test per environment, and do not judge correctness from production

Apple offers sandbox accounts and local StoreKit configuration files; sandbox subscription durations are shortened and do not reflect real renewal timing. Google uses license testers, and test orders are marked as such without real charges. Before shipping, verify that entitlements are granted correctly on success, that cancelled, failed, and pending states grant nothing, that restore works after reinstall, and that renewal, expiry, and refund update access. Testing only the happy path is the most common source of billing bugs.

## Server validation is what makes the ledger trustworthy

A client-reported success should not be the only source of truth. Apple recommends the App Store Server API and server notifications; Google provides the Play Developer API for purchase verification and real-time developer notifications. Store order identifiers and make grants idempotent. On Google, purchases must be acknowledged within the required window or they are refunded automatically, and consumables must be consumed after delivery. Check current documentation for exact API versions and deadlines. A small offline app can start with local checks, but cross-device entitlements or meaningful revenue justify a backend.

## Pre-launch checklist and when to stop and investigate

Confirm agreements and tax status are active, product IDs match the code exactly, prices and regions are right, Apple products are attached to the submitted version, Google products are active, a restore path exists, and subscription screens state price, period, and renewal terms clearly. If products still do not load in sandbox or on a test track, stop and recheck agreements, package identifiers, and track status instead of changing code repeatedly. Fees, tax rules, and review requirements change, so rely on the platform pages current at the time.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
