---
title: "Google Play App Bundle 上传失败怎么办？AAB 签名、版本号与上传错误排查"
canonical: https://fatevia.cn/blog/articles/google-play-aab-upload-failed-signing-version-code.html
canonical_en: https://fatevia.cn/en/blog/articles/google-play-aab-upload-failed-signing-version-code.html
date: 2026-10-12
source: https://fatevia.cn/
---

# Google Play App Bundle 上传失败怎么办？AAB 签名、版本号与上传错误排查

本文件是网站文章的副本。正式页面：[Google Play App Bundle 上传失败怎么办？AAB 签名、版本号与上传错误排查](https://fatevia.cn/blog/articles/google-play-aab-upload-failed-signing-version-code.html)

AAB 上传被 Play Console 拒绝时,先按签名密钥、版本号、清单元数据三层定位。本文说明指纹比对、versionCode 冲突、debug 签名、targetSdk 与上传密钥重置的判断方法。

## 先判断报错落在哪一层,再动手改

Play Console 对 AAB 的拒绝大致发生在三层:签名校验(包是否由已登记的上传密钥签名)、清单元数据(applicationId、versionCode、targetSdkVersion)、包结构(原生库架构、模块、体积)。不同层的修复动作完全不同,所以先读完整报错的第一句,不要直接重新打包。常用的三条本地命令是:./gradlew signingReport 查看各 variant 实际使用的签名;keytool -list -v -keystore 你的密钥库 查看证书 SHA-1;bundletool dump manifest --bundle=app.aab 查看包里真正写入的 applicationId 与 versionCode。报错文案会随控制台版本调整,以你页面上显示的为准。

## 「signed with the wrong key」:本质是指纹不一致

启用 Play App Signing 后,Google 保管应用签名密钥,你手里的是上传密钥。控制台在「应用完整性」的应用签名页面会显示已登记的上传密钥证书指纹,上传时 Play 用它比对 AAB 的签名。报错说明两者不同。常见根因有四类:本机与 CI 使用了不同的 keystore;CI 的 secret 里放的是旧文件或 debug 配置;换电脑后重新生成了 keystore;alias 填错导致取到另一把密钥。处理办法是把 signingReport 或 keytool 输出的 SHA-1 与控制台逐位对照。如果确认原密钥已丢失,不要反复重打包,指纹不变就不会通过。应在控制台申请重置上传密钥:生成新密钥,导出证书提交申请,生效时间以平台页面说明为准。

## 版本号冲突:versionCode 在整个应用范围内唯一

versionCode 是写入清单的整数,用于判断新旧,versionName 只是展示字符串,改它不能解决冲突。已经上传到该应用的构件会占用版本号,与它属于测试轨道还是正式轨道无关,也不要指望删除草稿发布来释放。versionCode 只能递增,上限为 2100000000。排查时先用 bundletool 读出待传包的 versionCode,再到控制台的应用包资源库查看已用的最大值。CI 里最稳的做法是让 versionCode 从单调递增的构建序号生成,避免人工填写。如果你在 versionCode 里编码了日期或渠道位,要提前算好是否会超过上限。versionCode 写在已签名的包里,不能直接改文件,只能改配置后重新构建并签名。

## debug 签名、包名与 targetSdk 的拒绝原因

这三类错误都出自清单或签名配置。其一是 debug 签名:用 assembleDebug 或沿用调试签名配置产出的包会被拒,需要确认 release 的 signingConfig 已生效。其二是包名:applicationId 必须与目标应用一致,新应用的 com.example 之类受限前缀不能使用,且包名一旦在 Play 上占用就不能更换。其三是 targetSdkVersion:Play 对新应用和更新有最低目标 API 要求,并随年份调整,具体数值与期限以 Play Console 政策页面为准,不要照抄旧文章里的数字。若包含原生库,还需提供 64 位版本,缺失时会在上传阶段提示。

## 包本身的问题:文件类型、模块与体积

有些失败与签名无关。先确认上传的是 bundleRelease 产出的 .aab,而不是 APK 改了后缀。用 bundletool validate --bundle=app.aab 能提前发现结构错误。使用动态功能模块或 Play Asset Delivery 时,模块的 manifest 配置、最低 SDK 与基础模块不一致也会导致拒绝。体积方面,AAB 上传大小与用户下载大小是两套限制,具体上限以官方文档当前说明为准。若 bundletool 校验通过、签名指纹一致、versionCode 未被占用,但仍被拒,问题大概率在模块配置或政策声明,而不在打包流程。

## 什么时候该停下来换路径

可以用三个条件决定下一步。指纹不一致且原密钥无法找回:停止重试,走上传密钥重置。版本号、包名、指纹三项都已核对仍失败:保存控制台完整报错,在本地用 bundletool 对照清单,再考虑是否为政策或模块问题。通过 Publishing API 或自动化工具上传时返回 403 或权限类错误:这是服务账号在 Play Console 中的应用权限问题,与 AAB 内容无关,应检查该账号是否被授予对应应用的发布权限。把失败归到正确的层之后,每次只改一项并重新验证,比整体重打包更快。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Google Play App Bundle Upload Failed: Troubleshooting AAB Signing, Version Code and Package Errors

This file mirrors the site article. Official page: [Google Play App Bundle Upload Failed: Troubleshooting AAB Signing, Version Code and Package Errors](https://fatevia.cn/en/blog/articles/google-play-aab-upload-failed-signing-version-code.html)

When Play Console rejects an AAB, sort the error into signing key, version code, or manifest metadata first. This guide covers fingerprint checks, versionCode conflicts, debug signing, targetSdk, and upload key reset.

## Sort the error into a layer before changing anything

Play Console rejects an AAB at roughly three layers: signature validation (was it signed with the registered upload key), manifest metadata (applicationId, versionCode, targetSdkVersion), and bundle structure (native library ABIs, modules, size). Each layer has a different fix, so read the first sentence of the full error before rebuilding. Three local commands cover most diagnosis: ./gradlew signingReport shows which signing config each variant actually uses; keytool -list -v -keystore your.jks prints the certificate SHA-1; bundletool dump manifest --bundle=app.aab shows the applicationId and versionCode that were really written into the file. Error wording changes over time, so trust what your console shows.

## The wrong-key error means the fingerprints differ

With Play App Signing, Google holds the app signing key and you hold an upload key. The App signing page under App integrity shows the upload certificate fingerprint Play has on record, and each upload is checked against it. The error means your AAB was signed by something else. Typical causes: local and CI builds use different keystores, the CI secret holds an old file or a debug config, a new machine generated a fresh keystore, or a wrong alias picked another key. Compare the SHA-1 from signingReport or keytool with the console value character by character. If the original key is truly lost, rebuilding will not help because the fingerprint stays the same. Request an upload key reset in Play Console with a newly generated key's certificate. Timing is described on the platform's own page.

## Version code conflicts are scoped to the whole app

versionCode is the integer in the manifest that Play uses to order releases. versionName is only a display string, so changing it does not clear a conflict. Artifacts already uploaded to the app hold their version codes regardless of track, and deleting a draft release should not be relied on to free one. The value must increase and cannot exceed 2100000000. Read the code from your bundle with bundletool, then compare it with the highest value in the app bundle explorer. In CI, derive versionCode from a monotonic build number instead of typing it by hand. If you encode dates or flavors into the number, check early that it will not overflow. The code lives inside a signed bundle, so you must fix the config, rebuild and re-sign.

## Debug signing, package name and targetSdk rejections

All three come from signing or manifest configuration. Debug signing: bundles built with a debug signing config are refused, so confirm the release signingConfig is actually applied to the variant you build. Package name: applicationId must match the existing app, restricted prefixes such as com.example are not accepted for new apps, and a package name claimed on Play cannot be changed later. targetSdkVersion: Play sets a minimum target API for new apps and updates, and it moves yearly. Use the number on the current Play Console policy page, not one from an old article. If your app ships native code, 64-bit libraries are also required, and a missing ABI is reported at upload.

## When the file itself is the problem

Some failures have nothing to do with signing. Confirm you are uploading the .aab from bundleRelease, not an APK renamed. Run bundletool validate --bundle=app.aab to catch structural errors early. With dynamic feature modules or Play Asset Delivery, a module manifest or minSdk that disagrees with the base module can cause rejection. On size, upload size and user download size are separate limits, and the current numbers belong in the official documentation. If validation passes, the fingerprint matches and the versionCode is unused but the upload still fails, suspect module configuration or a policy declaration rather than the build pipeline.

## Knowing when to stop and change path

Three conditions decide the next move. Fingerprint mismatch with no recoverable key: stop retrying and go through the upload key reset. Version code, package name and fingerprint all verified but the error persists: save the full console message and compare it against the local manifest, then look at policy or module causes. A 403 or permission error when uploading through the Publishing API or an automation tool points to the service account's per-app permissions in Play Console, not to the bundle. Once the failure is placed in the right layer, change one thing at a time and re-verify, which is faster than rebuilding everything.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
