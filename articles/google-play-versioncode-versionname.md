---
title: "Google Play 应用版本号怎么设置:versionCode 与 versionName 的区别"
canonical: https://fatevia.cn/blog/articles/google-play-versioncode-versionname.html
canonical_en: https://fatevia.cn/en/blog/articles/google-play-versioncode-versionname.html
date: 2026-10-14
source: https://fatevia.cn/
---

# Google Play 应用版本号怎么设置:versionCode 与 versionName 的区别

本文件是网站文章的副本。正式页面：[Google Play 应用版本号怎么设置:versionCode 与 versionName 的区别](https://fatevia.cn/blog/articles/google-play-versioncode-versionname.html)

versionCode 决定 Google Play 能否接受新包并触发更新,versionName 只是展示给用户的字符串。本文说明两者的作用、Gradle 配置方式、递增规则,以及上传被拒时的排查思路。

## versionCode 是机器比较用的整数,versionName 是给人看的字符串

versionCode 是正整数,Android 系统和 Google Play 用它判断两个 APK 或 AAB 谁更新。versionName 是任意字符串,例如「2.4.1」,只在应用详情页和系统设置里展示,系统不会拿它做新旧比较。所以更新能否被识别、能否覆盖安装,取决于 versionCode;用户看到的版本说明则来自 versionName。两者没有强制的数学关系,但团队最好用固定规则把它们关联起来,方便从线上问题反查到具体构建。

## 在 Gradle 里怎么写

现代 Android 项目通常在模块级 build.gradle(或 build.gradle.kts)的 defaultConfig 中设置:versionCode 填整数,versionName 填字符串。修改后需要重新构建并签名,Play Console 读取的是包内 manifest 合并后的最终值,而不是你源码里某一处的文字。如果使用了 productFlavors 或 buildTypes 覆盖,要以最终产物的 manifest 为准,可以用 Android Studio 的 APK Analyzer 或 bundletool 检查生成包中的实际数值。

## 递增规则:同一应用的每次上传都必须更大

Play Console 要求同一包名下,新上传的包 versionCode 必须高于该应用已上传过的所有版本,包括已下架、仅在内部测试轨道或草稿状态的版本。常见现象是:你以为线上是 120,但某次内部测试已经传过 125,这时再传 121 会被拒绝。判断标准很简单:以 Play Console 中各轨道已出现过的最大值为基线,新版本在此基础上加 1 或更多。versionCode 有平台规定的上限(约 21 亿),日常使用远不会触及,但不要用时间戳之类过长的数字,以免后期触顶或难以阅读。

## 三种常见的编号策略与取舍

第一种是手动递增,适合小团队,简单但容易忘记改或在分支合并时冲突。第二种是由 CI 的构建序号生成,保证单调递增,但需要确认重跑构建、多分支并行时不会产生重复或回退。第三种是按语义版本编码,例如把 2.4.1 映射为 20401,可读性好,但要预留位数并避免热修复时没有空间。选择时看两点:是否存在多条发布分支,以及是否需要从 versionCode 直接反推版本。多分支并行时,优先选择由 CI 统一分配的方案。

## 拆分 APK 与多产物时要预留编号空间

如果你同时发布多个 ABI 或屏幕密度的拆分 APK,或者有多个产物共用同一应用条目,不同产物需要各自唯一的 versionCode,并且要保证某些设备上较新的产物编号更高。常见做法是在基础编号上乘以一个倍数再加产物偏移量。使用 AAB 时由 Google Play 生成分发 APK,通常不需要自己处理拆分编号,但如果仍在发布多个独立 APK,这一点就必须提前设计。

## 上传被拒或更新不推送时怎么排查

先看错误提示是否指向 versionCode 已被使用,这种情况只需提高数值并重新打包。若包已上传但用户收不到更新,依次确认:发布轨道和国家地区是否包含该用户;分阶段发布的比例;设备是否满足 minSdk 与其他兼容性条件;以及用户设备上已安装版本的 versionCode 是否确实更低。如果两次排查后仍无法解释,应以 Play Console 的版本详情页和设备目录兼容性提示为准,而不是反复重传。具体限制和提示文案以 Play Console 当时页面为准。

## versionName 的写法与不该做的事

versionName 可自由命名,但建议统一格式,例如主版本.次版本.修订号,并和发布说明、崩溃日志中的标识保持一致。不要把 versionName 当作更新判断依据,也不要在线上已发布后随意改动编号规则,否则日志归因会混乱。一旦某个 versionCode 已用于上传,即便该版本未发布,也不应再复用,需要回滚时应发布一个 versionCode 更高的新包,内容可以回到旧代码。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Google Play App Version Numbers: versionCode vs versionName Explained

This file mirrors the site article. Official page: [Google Play App Version Numbers: versionCode vs versionName Explained](https://fatevia.cn/en/blog/articles/google-play-versioncode-versionname.html)

versionCode decides whether Google Play accepts a new build and treats it as an update, while versionName is only a display string. This covers Gradle setup, increment rules, and what to check when an upload is rejected.

## versionCode is for machines, versionName is for people

versionCode is a positive integer that Android and Google Play use to decide which build is newer. versionName is a free-form string such as "2.4.1" that appears on the store listing and in system settings; the system never compares it. Whether an update is recognized and installs over an existing app depends on versionCode alone. The two have no enforced relationship, but a consistent mapping makes it much easier to trace a production issue back to a specific build.

## Where to set them in Gradle

In a modern Android project, both live in the defaultConfig block of the module-level build.gradle or build.gradle.kts: an integer for versionCode and a string for versionName. Play Console reads the merged manifest inside the built artifact, not the line in your source. If productFlavors or buildTypes override the values, check the final output with APK Analyzer or bundletool to confirm what was actually packaged.

## Every upload must exceed everything uploaded before

For a given package name, a new upload needs a versionCode higher than any previously uploaded build, including builds on internal testing tracks, drafts, or releases that were never rolled out. A typical failure: you assume production is at 120, but an internal build at 125 already exists, so uploading 121 is rejected. Use the highest value that has appeared on any track as your baseline and go higher. The platform cap is around 2.1 billion, so avoid oversized schemes like long timestamps that make numbers hard to read or risk running out of room.

## Three numbering strategies and their trade-offs

Manual increments suit small teams but are easy to forget and collide across merged branches. CI-generated numbers from a build counter stay monotonic, though you need to verify that reruns and parallel branches never yield duplicates or regressions. Semantic encoding, such as mapping 2.4.1 to 20401, is readable but needs reserved digits and leaves little room for hotfixes if poorly planned. Decide based on whether you run multiple release branches and whether you need to derive the version from the code. With parallel branches, let CI assign the number.

## Split APKs and multiple artifacts need reserved headroom

If you ship separate APKs per ABI or density, or several artifacts under one listing, each needs a unique versionCode, and on devices that can accept more than one, the intended build should carry the higher number. A common pattern multiplies a base number and adds an artifact offset. With AAB, Google Play generates the delivered APKs, so you rarely handle this yourself, but if you still publish multiple standalone APKs, plan the scheme up front.

## When uploads fail or updates do not reach users

If the error says the versionCode is already used, raise it and rebuild. If the upload succeeded but users see no update, check in order: whether the track and countries include them, the staged rollout percentage, device compatibility such as minSdk, and whether the installed versionCode is actually lower. If two rounds of checks do not explain it, rely on the release details and device compatibility messages in Play Console rather than re-uploading repeatedly. Exact limits and wording follow what Play Console shows at the time.

## Writing versionName and what to avoid

versionName is free text, but a stable format like major.minor.patch matching release notes and crash report labels works best. Do not use it to decide update logic, and do not change your numbering rules after releasing, or log attribution gets confusing. Once a versionCode has been uploaded, do not reuse it, even if that build never shipped. To roll back, rebuild the older code and publish it with a higher versionCode.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
