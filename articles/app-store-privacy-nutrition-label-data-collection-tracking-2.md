---
title: "App Store 隐私营养标签怎么填：收集的数据和追踪要声明到哪一级"
canonical: https://fatevia.cn/blog/articles/app-store-privacy-nutrition-label-data-collection-tracking-2.html
canonical_en: https://fatevia.cn/en/blog/articles/app-store-privacy-nutrition-label-data-collection-tracking-2.html
date: 2026-10-19
source: https://fatevia.cn/
---

# App Store 隐私营养标签怎么填：收集的数据和追踪要声明到哪一级

本文件是网站文章的副本。正式页面：[App Store 隐私营养标签怎么填：收集的数据和追踪要声明到哪一级](https://fatevia.cn/blog/articles/app-store-privacy-nutrition-label-data-collection-tracking-2.html)

App Store Connect 的 App 隐私问卷要对每类数据分别回答：是否收集、是否关联用户身份、是否用于追踪。本文说明这三层的判定条件、第三方 SDK 的归属，以及填错后的修改方式。

## 填的不是一个开关，而是每类数据的三层判断

隐私营养标签在 App Store Connect 的「App 隐私」页面填写，结构是按数据类型逐项作答。常见类别包括联系信息、位置、标识符、使用数据、诊断、购买记录、用户内容等。对每一个你收集的类别，要依次回答：用于哪些目的、是否与用户身份关联、是否用于追踪。所以「声明到哪一级」的答案是：先决定收不收集，再决定关不关联，最后决定算不算追踪。三层各自独立，不能只在最外层勾一个「是」就结束。具体类别名称和选项以 App Store Connect 当时的页面为准。

## 「收集」的判断标准是数据是否离开了设备

Apple 对收集的口径，核心是数据被传出设备，且你或第三方合作方可以访问并保留，而不是仅为实时完成一次请求而临时处理。只在本机处理、不上传的数据，通常不算收集。反过来，上传到你自己的服务器、发给统计或广告服务商，都算。常见漏项有三类：登录时填写的邮箱和昵称（联系信息）、崩溃日志与性能指标（诊断）、设备 ID 或广告标识符（标识符）。判断时不要只看自己写的代码，要按网络请求实际发出了什么来核对，可以用抓包工具对照一遍。

## 第三方 SDK 收集的数据也算你的，必须一并声明

App 隐私问卷要求涵盖你自己和第三方合作方的收集行为。接入广告、归因、统计、崩溃上报、推送或登录 SDK 后，这些组件收集的数据都要计入。实操上，逐个查看 SDK 厂商提供的隐私披露说明，把它们的数据类型与用途并入你的清单。SDK 升级后收集范围可能变化，所以每次升级依赖时应复核一次。另外，Apple 对部分常用 SDK 要求提供隐私清单文件（PrivacyInfo.xcprivacy），清单与问卷应当互相对得上，两边不一致是审核和用户质疑的常见来源。

## 「关联身份」取决于数据能否对应到具体用户或设备

如果收集的数据与账号、设备 ID、邮箱等关联，就应标记为关联用户身份。常见误判是崩溃日志：若日志里带了用户 ID 或账号信息，就是关联；若只有匿名的堆栈和设备型号且无法对应到人，才可按未关联处理。统计数据同理，只要事件流绑定了用户 ID 或持久设备标识，就按关联填写。标记为未关联，意味着你承诺没有也不会把它与身份对上，这个承诺要有实际的数据设计支撑，而不是默认勾选。

## 「追踪」有狭义定义，别和普通统计混为一谈

Apple 所说的追踪，是把你 App 里收集的用户或设备数据，与其他公司的 App、网站或线下数据关联，用于定向广告或广告效果衡量；或者把这类数据分享给数据经纪商。因此以下情况通常构成追踪：接入广告网络并用 IDFA 做跨 App 定向、与第三方归因平台共享设备标识用于投放衡量。仅在自己产品内部做统计、或服务商只代表你处理数据且不用于自身目的，一般不属于追踪。只要有任何一类数据用于追踪，就要在这类数据下勾选追踪，并且 App 在读取 IDFA 或进行跨应用追踪前，须通过 App Tracking Transparency 弹窗取得授权，并在 Info.plist 提供用途说明。问卷声明与 ATT 实现是否一致，也要自查。

## 填错了怎么办：随时可改，但要和实际行为同步

隐私答案可以在 App Store Connect 里随时修改，不需要提交新二进制，修改后会在商店页更新。但这不意味着可以随意填：标签与实际数据行为不符，可能在审核中被指出，也可能在后续版本被要求更正。需要立即复核的场景有：新增或更换 SDK、增加账号体系、开始投放广告或接入归因、改动日志上报字段。建议把数据清单维护成一份内部表格，列出数据类型、来源（自有或 SDK）、用途、是否关联、是否追踪，每次发版前对照更新。同时确认隐私政策链接中的描述与标签一致。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# App Store Privacy Nutrition Labels: How Far to Declare Collected Data and Tracking

This file mirrors the site article. Official page: [App Store Privacy Nutrition Labels: How Far to Declare Collected Data and Tracking](https://fatevia.cn/en/blog/articles/app-store-privacy-nutrition-label-data-collection-tracking-2.html)

The App Privacy questionnaire in App Store Connect asks three things per data type: is it collected, is it linked to identity, is it used for tracking. Here is how to decide each, how SDKs count, and how to fix a wrong answer.

## It is three decisions per data type, not one switch

The privacy label is filled in under App Privacy in App Store Connect, organized by data type: contact info, location, identifiers, usage data, diagnostics, purchases, user content and so on. For each type you collect, you answer which purposes it serves, whether it is linked to the user's identity, and whether it is used for tracking. So the question of how far to declare comes down to a sequence: collected or not, linked or not, tracking or not. Each layer is independent. Exact category names and options follow whatever App Store Connect shows at the time.

## Collection means the data leaves the device

Apple's notion of collection is data transmitted off the device in a way that you or your third-party partners can access and keep, beyond what is needed to serve a request in real time. Data processed only on-device generally does not count. Data sent to your server or to an analytics or ad provider does. The usual misses are account details such as email and display name (contact info), crash logs and performance metrics (diagnostics), and device or advertising identifiers (identifiers). Do not rely on reading your own code alone. Inspect the network requests your build actually sends, for example with a proxy tool.

## SDK data counts as yours

The questionnaire covers collection by you and by third-party partners. Once you integrate ads, attribution, analytics, crash reporting, push or login SDKs, whatever they collect belongs in your declaration. In practice, read each vendor's privacy disclosure and merge its data types and purposes into your inventory. Collection can change when an SDK updates, so recheck on every dependency upgrade. Apple also expects privacy manifest files (PrivacyInfo.xcprivacy) for certain common SDKs. The manifest and the questionnaire should tell the same story, and mismatches are a frequent trigger for review questions.

## Linked to identity depends on whether data maps to a user or device

If collected data is tied to an account, device ID or email, mark it as linked. Crash logs are the classic misjudgment: if they carry a user ID or account info they are linked, and only anonymous stack traces with no way to map back to a person can be treated as not linked. Analytics works the same way. An event stream keyed to a user ID or persistent device identifier is linked. Choosing not linked is a commitment that you neither do nor will join the data to identity, so it needs support in your actual data design.

## Tracking has a narrow definition, separate from ordinary analytics

Apple defines tracking as linking user or device data collected in your app with data from other companies' apps, websites or offline sources for targeted advertising or ad measurement, or sharing such data with data brokers. Typical cases are an ad network using IDFA for cross-app targeting, or sharing device identifiers with a third-party attribution platform for campaign measurement. First-party analytics inside your own product, or a service provider processing data only on your behalf, usually does not qualify. If any data type is used for tracking, mark it and also implement App Tracking Transparency: request authorization before reading IDFA or tracking across apps, with a usage description in Info.plist. Check that your declaration and your ATT implementation agree.

## Wrong answers can be edited anytime, but keep them in sync

You can change privacy answers in App Store Connect without uploading a new binary, and the product page updates afterward. That does not make guessing safe. Labels that contradict actual behavior can be flagged in review or later require correction. Re-audit whenever you add or swap an SDK, introduce accounts, start running ads or attribution, or change logging fields. A practical habit is an internal table listing data type, source (own code or SDK), purpose, linked or not, tracking or not, updated before each release. Make sure your privacy policy link describes the same practices as the label.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
