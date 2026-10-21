---
title: "Google Play 广告 ID 声明与实际 SDK 不一致怎么办：数据安全表单和商店页要对哪几项"
canonical: https://fatevia.cn/blog/articles/google-play-advertising-id-sdk-mismatch-2.html
canonical_en: https://fatevia.cn/en/blog/articles/google-play-advertising-id-sdk-mismatch-2.html
date: 2026-10-21
source: https://fatevia.cn/
---

# Google Play 广告 ID 声明与实际 SDK 不一致怎么办：数据安全表单和商店页要对哪几项

本文件是网站文章的副本。正式页面：[Google Play 广告 ID 声明与实际 SDK 不一致怎么办：数据安全表单和商店页要对哪几项](https://fatevia.cn/blog/articles/google-play-advertising-id-sdk-mismatch-2.html)

广告 ID 声明、清单权限、数据安全表单和隐私政策对不上时，Google Play 可能提示政策问题。本文说明先查什么、哪几处必须同步，以及何时该删 SDK 而不是改声明。

## 先确认不一致发生在哪一层

所谓不一致，通常是下面四处中至少有两处说法不同：Play Console「应用内容」里的广告 ID 声明、最终 APK/AAB 合并后的清单权限、数据安全表单里的数据类型、隐私政策正文。先别急着改表单，应先判断哪一处反映的是真实行为。真实行为以代码和 SDK 实际运行为准，其余几处是对它的描述。

## 用合并后的清单和依赖树确认真实行为

很多团队自己没写广告相关代码，但广告、归因、统计类 SDK 会在清单合并时带入 com.google.android.gms.permission.AD_ID。在 Android Studio 的 Merged Manifest 视图，或对构建产物用 apkanalyzer 查看清单，才能看到最终结果。再对照依赖树，找出是哪个库引入的。同时在代码里搜索是否有读取广告 ID 的调用，例如 AdvertisingIdClient。判断标准：只要任何一条路径可能读取广告 ID，就按「使用」处理。无法确认某个 SDK 是否读取时，应以其官方文档或厂商书面说明为准，确认不了就按已收集处理。

## 声明、权限与代码三种常见错位的处理

第一种，声明写「不使用」，但清单里有 AD_ID 权限或 SDK 会读取。这是风险最高的一种，应改声明为「使用」，或移除对应 SDK 并在清单里用 tools:node="remove" 去掉权限，之后重新构建核对。第二种，声明写「使用」，但实际已删除相关 SDK，属于过度声明，不会造成同类风险，但会让商店页和表单显示不必要的数据收集，应在下次发版后改回。第三种，面向 Android 13（API 33）及以上的应用缺少 AD_ID 权限却依赖广告 ID，设备返回的会是全零值，广告归因和变现会静默失效，需要补权限并同步声明。

## 数据安全表单要对应的三项

广告 ID 在数据安全表单里归入「设备或其他 ID」。需要核对三点：该数据类型是否标为已收集；是否标为与第三方共享，广告和归因 SDK 把数据发给自己服务器时通常属于共享；用途是否与实际一致，例如广告或营销、分析、反欺诈。注意 Android ID 等其他设备标识同样落在这一类，不要只盯着广告 ID。SDK 的收集行为，Google 视为开发者自身责任，不能以「是第三方做的」为由省略。具体选项和措辞以 Play Console 当时页面为准。

## 商店页和政策页面要同步的几项

商店页面层面主要有三处：应用内容中的「是否包含广告」声明，应与实际展示广告的行为一致；隐私政策链接必须可访问，且正文要写明收集设备标识符、用途以及与哪些类别的第三方共享；目标受众设置。若应用面向儿童或包含儿童受众，家庭政策对广告 ID 的使用有更严格的限制，这类应用不应继续使用会读取广告 ID 的 SDK，此时应删 SDK，而不是改声明。

## 选择改声明还是删 SDK

判断依据是该 SDK 的业务价值和合规成本。如果广告变现、归因是产品收入的一部分，应保留 SDK，把声明、表单、隐私政策一次改齐。如果该 SDK 只做低价值统计，或使应用落入儿童受众等限制场景，删除更省事，也能同时缩小表单和隐私政策需要披露的范围。无论选哪条，改完都要重新构建并检查合并清单，再提交 Console 里的变更。若再次收到政策提示，先复核是否还有其他模块引入了权限，而不是反复修改表单措辞。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Google Play Advertising ID Declaration vs. Actual SDKs: What to Fix in the Data Safety Form and Store Listing

This file mirrors the site article. Official page: [Google Play Advertising ID Declaration vs. Actual SDKs: What to Fix in the Data Safety Form and Store Listing](https://fatevia.cn/en/blog/articles/google-play-advertising-id-sdk-mismatch-2.html)

When your advertising ID declaration, manifest permission, Data safety form and privacy policy disagree, Google Play may flag a policy issue. Here is what to check first and which items must be kept in sync.

## Find which layer disagrees

A mismatch usually means at least two of these four sources say different things: the advertising ID declaration in Play Console App content, the permissions in the merged manifest of your build, the Data safety form, and the privacy policy text. Do not edit the form first. Decide which source reflects real behavior. Code and SDK runtime behavior are the truth; the others only describe it.

## Verify behavior from the merged manifest and dependency tree

Many teams never write ad code themselves, yet an ads, attribution or analytics SDK can pull com.google.android.gms.permission.AD_ID in during manifest merge. Check the Merged Manifest view in Android Studio, or run apkanalyzer on the built artifact, then trace which library adds it through the dependency tree. Also search for calls such as AdvertisingIdClient. The rule: if any code path may read the advertising ID, treat the app as using it. If you cannot confirm what an SDK does, rely on its vendor documentation, and if that is unclear, treat it as collecting.

## Three common mismatches and their fixes

First, the declaration says no, but the manifest has AD_ID or an SDK reads the ID. This is the riskiest case. Change the declaration to yes, or remove the SDK and strip the permission with tools:node="remove", then rebuild and recheck. Second, the declaration says yes but the SDK is already gone. That is over-declaring. It is less risky but shows unnecessary collection, so correct it after the next release. Third, an app targeting Android 13 (API 33) or higher depends on the advertising ID without the AD_ID permission. The device returns a zeroed value, so attribution and monetization fail silently. Add the permission and update the declaration.

## Three things to check in the Data safety form

The advertising ID falls under Device or other IDs in the Data safety form. Check whether the type is marked as collected, whether it is marked as shared with third parties (ad and attribution SDKs that send data to their own servers usually count as sharing), and whether the purposes match reality, such as advertising or marketing, analytics or fraud prevention. Other device identifiers like Android ID belong to the same category, so do not look only at the advertising ID. Google treats SDK collection as your responsibility, so you cannot omit it because a third party does the collecting. Exact options and wording follow the Play Console page at the time.

## Store listing and policy items to keep aligned

Three items sit at the listing level. The Contains ads declaration must match whether ads are shown. The privacy policy link must work and name the identifiers collected, their purposes and the categories of third parties receiving them. And target audience settings matter: if the app is aimed at or includes children, the Families policy restricts advertising ID use much more tightly, so the right move there is to remove the SDK, not to change the declaration.

## Change the declaration or remove the SDK

Decide by the SDK's business value against its compliance cost. If ad revenue or attribution is part of your income, keep the SDK and update the declaration, form and privacy policy together. If it only does low-value analytics, or it puts you in a restricted case such as a child audience, removal is simpler and also shrinks what you must disclose. Either way, rebuild, recheck the merged manifest and then submit the Console changes. If you get another policy flag, look for other modules adding the permission rather than rewording the form again.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
