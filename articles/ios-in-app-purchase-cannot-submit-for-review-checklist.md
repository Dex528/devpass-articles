---
title: "iOS App 内购无法提交审核：商品状态、协议与沙盒测试排查"
canonical: https://fatevia.cn/blog/articles/ios-in-app-purchase-cannot-submit-for-review-checklist.html
canonical_en: https://fatevia.cn/en/blog/articles/ios-in-app-purchase-cannot-submit-for-review-checklist.html
date: 2026-10-11
source: https://fatevia.cn/
---

# iOS App 内购无法提交审核：商品状态、协议与沙盒测试排查

本文件是网站文章的副本。正式页面：[iOS App 内购无法提交审核：商品状态、协议与沙盒测试排查](https://fatevia.cn/blog/articles/ios-in-app-purchase-cannot-submit-for-review-checklist.html)

内购商品卡在「缺少元数据」、无法随版本提交或沙盒拉不到商品时，通常不是单点故障。本文按协议、商品配置、构建绑定和 StoreKit 测试顺序拆解根因与判断标准。

## 先分清：是商品提交不了，还是提交后被拒

这两类问题的排查路径完全不同。前者发生在 App Store Connect 内，表现为商品状态停在「缺少元数据」(Missing Metadata)、提交按钮不可用，或版本页面里找不到「App 内购买项目」入口。后者是商品已进入「正在等待审核」，随后收到 Guideline 2.1 或 3.1.1 一类的拒绝说明。前者查配置和账号状态，后者查审核员实际看到的行为。把两者混在一起，容易在沙盒里反复调试一个根本没提交成功的商品。

## 协议、税务和银行信息决定商品能否真正生效

付费商品（含内购、订阅）依赖账号持有人（Account Holder）在「协议、税务和银行业务」中接受并生效「付费 App」协议，并补全税务与银行信息。协议处于待处理、过期或信息缺失时，商品可以创建，但沙盒中常拿不到商品，提交时也可能受阻。注意只有 Account Holder 能接受协议，Admin 或 App Manager 看到的是提示而不是操作入口。判断标准很直接：该页面中付费 App 一栏应为「有效」，且没有红色待办提示。具体字段和状态文案以 App Store Connect 当时页面为准。

## 「缺少元数据」的常见来源

商品创建后需要补齐的字段通常包括：参考名称、产品 ID、价格、至少一种本地化的显示名称和描述，以及用于审核的截图。非消耗型和订阅类商品还要检查订阅群组、订阅时长等额外字段。产品 ID 一旦创建不可修改，也不能复用已删除商品的 ID，所以命名要在代码里和后台里保持一致。多数「缺少元数据」问题来自审核截图未上传，或本地化信息只填了一种而未设置为可用状态。逐项对照商品详情页的警告标记，比反复点击提交更有效。

## 首个内购商品必须随新版本一起提交

一个 App 的第一个内购商品，需要在 App 版本页面的「App 内购买项目和订阅」区域选中，并与一个包含该功能的构建版本一同提交审核。如果这个区域不显示，先确认是否已有可选的构建版本，以及商品状态是否已达到「准备提交」。很多人在商品单独点提交后找不到按钮，是因为首个商品不走独立提交路径。此后新增商品是否能独立提交，以平台当时的流程为准。

## 沙盒拉不到商品：按这个顺序缩小范围

代码里 Product 请求返回空数组或 invalid product identifiers，优先查这几项：Bundle ID 与 App Store Connect 中的 App 是否一致；代码中的产品 ID 是否逐字符匹配；付费 App 协议是否有效；商品是否至少到达「准备提交」；设备上是否登录了正确的沙盒账号。新建商品在后台生效有延迟，这属于已知现象，但延迟没有固定时长可依赖，所以若上述各项都对，再等待而不是改代码。另外要区分两种测试环境：Xcode 的 StoreKit Configuration 文件是本地模拟，不依赖后台商品；沙盒测试则必须依赖后台配置。用本地配置文件测通，并不能证明后台商品配置正确。

## 被拒后先复现审核员的路径

审核反馈常见的指向有三类：审核员找不到购买入口或无法完成购买；购买后权益没有发放；恢复购买功能缺失或无响应。处理时用全新安装的构建，按审核备注里的步骤走一遍，确认购买入口无需额外账号权限即可到达，购买成功后立即发放权益，并提供明确的「恢复购买」入口。审核环境走的是沙盒，所以服务端若校验收据，必须能正确处理沙盒和生产两种环境，常见做法是先请求生产验证，遇到沙盒收据对应的状态码再回退到沙盒。数字内容通过外部支付引导，则要对照 Guideline 3.1.1 及其对应地区的例外条款，规则会变化，以官方文本为准。

## 什么时候该停止调试，换思路

如果协议有效、商品为「准备提交」、产品 ID 和 Bundle ID 全部核对无误，沙盒仍连续失败，就不要继续改代码。此时可以换一台设备和新的沙盒测试账号验证，或在 Xcode 中对同一产品 ID 用 StoreKit 配置文件对比，以判断问题在代码还是在后台。仍无法定位时，通过 App Store Connect 的联系我们提交工单，附上 App 的 Apple ID、商品 ID、复现步骤和截图，比在审核回复里反复解释更有效。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# iOS In-App Purchase Won't Submit for Review: Agreements, Product Status and Sandbox

This file mirrors the site article. Official page: [iOS In-App Purchase Won't Submit for Review: Agreements, Product Status and Sandbox](https://fatevia.cn/en/blog/articles/ios-in-app-purchase-cannot-submit-for-review-checklist.html)

When an in-app purchase is stuck at Missing Metadata, won't attach to a version, or returns no products in sandbox, the cause is usually one of a few linked settings. This checklist orders them by dependency.

## Separate two problems: can't submit, and rejected after submission

They have different causes. If the product is stuck at Missing Metadata, the submit option is unavailable, or the in-app purchase section is absent from the version page, the problem is configuration or account state. If the product reached Waiting for Review and then came back with a Guideline 2.1 or 3.1.1 note, the problem is what the reviewer actually experienced. Debugging sandbox behavior for a product that was never properly submitted wastes time, so settle which case you are in first.

## Agreements, tax and banking gate everything else

Paid products depend on the Account Holder accepting the Paid Apps Agreement and completing tax and banking details under Agreements, Tax, and Banking. With the agreement pending, expired or incomplete, products can still be created, but sandbox often returns nothing and submission can be blocked. Only the Account Holder can accept it; Admins see a notice but no action. The check is simple: the Paid Apps entry should read Active with no outstanding warnings. Exact labels and fields follow whatever App Store Connect currently shows.

## What usually causes Missing Metadata

A product typically needs a reference name, product ID, price, at least one localization with display name and description, and a review screenshot. Subscriptions also need a valid group and duration. The product ID cannot be edited after creation and cannot reuse an ID from a deleted product, so keep it identical in code and in the console. In practice, the most common gaps are a missing review screenshot and a localization that was entered but not enabled. Read the warning markers on the product page one by one instead of resubmitting repeatedly.

## The first in-app purchase ships with a new app version

For an app's first in-app purchase, you select it in the In-App Purchases and Subscriptions area of the version page and submit it together with a build that includes the feature. If that area is missing, check that a build is attached and that the product has reached Ready to Submit. People often look for a standalone submit button that does not exist for the first product. Whether later products can be submitted independently depends on the current App Store Connect flow.

## When sandbox returns no products, narrow it in this order

If a product request returns an empty list or invalid identifiers, check the bundle ID against the app record, compare the product ID character by character, confirm the Paid Apps Agreement is active, confirm the product is at least Ready to Submit, and make sure the device is signed into a sandbox account. New products can take time to propagate, but there is no reliable duration, so wait only after the other checks pass. Also distinguish environments: an Xcode StoreKit configuration file is a local simulation that does not depend on console products, so passing there does not prove the console setup is correct.

## After a rejection, reproduce the reviewer's path

Rejections generally point to one of three things: the reviewer could not find or complete the purchase, entitlements were not granted afterward, or restore purchases was missing or unresponsive. Test on a clean install and follow your review notes literally. The purchase entry should be reachable without special account privileges, the entitlement should apply immediately, and a visible restore option should exist. Review runs in sandbox, so if your server validates receipts it must handle both production and sandbox, commonly by trying production first and falling back when the response indicates a sandbox receipt. If you steer users to external payment for digital goods, compare against Guideline 3.1.1 and the regional exceptions, which change over time; rely on the current official text.

## When to stop debugging and change approach

If the agreement is active, the product is Ready to Submit, and the product ID and bundle ID match, but sandbox keeps failing, stop editing code. Try another device with a fresh sandbox tester, and run the same product ID against a StoreKit configuration file to see whether the fault sits in code or in the console. If you still cannot isolate it, open a support request through App Store Connect with the app's Apple ID, product ID, reproduction steps and screenshots. That is more effective than repeated explanations in the review thread.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
