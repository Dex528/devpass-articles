---
title: "Google Play 应用商店链接在哪里获取:包名拼接、Play Console 入口与带参数下载链接"
canonical: https://fatevia.cn/blog/articles/google-play-store-link-and-app-download-url.html
canonical_en: https://fatevia.cn/en/blog/articles/google-play-store-link-and-app-download-url.html
date: 2026-10-12
source: https://fatevia.cn/
---

# Google Play 应用商店链接在哪里获取:包名拼接、Play Console 入口与带参数下载链接

本文件是网站文章的副本。正式页面：[Google Play 应用商店链接在哪里获取:包名拼接、Play Console 入口与带参数下载链接](https://fatevia.cn/blog/articles/google-play-store-link-and-app-download-url.html)

说明 Google Play 应用商店链接的组成规则、从网页端和 Play Console 获取的方式,以及如何为不同应用、渠道生成带 referrer 的下载链接并验证。

## 链接的本质是包名加固定前缀

Google Play 上每个应用的详情页地址都遵循同一格式:https://play.google.com/store/apps/details?id=包名。其中 id 就是应用的 applicationId(Android 项目中 build.gradle 里的 applicationId,而不是应用显示名称)。只要知道包名,不需要登录任何后台,就可以拼出该应用的链接。这也意味着:同一个应用在不同国家、不同语言下的链接主体是一致的,差异只体现在可选参数上。

## 三个可靠的获取位置

第一,已上架的应用,直接在浏览器打开 play.google.com 搜索应用,地址栏中的 URL 即商店链接。第二,Play Console 中进入对应应用,在「主要商店信息」或应用概览页能看到商店详情页入口,点击后跳转到的就是公开链接。第三,在自己的项目里读取 applicationId 直接拼接。需要注意:应用必须已发布到至少一个可见轨道(正式版)且在目标国家可用,公开链接才会正常打开;仅处于内部测试或未发布状态时,普通用户打开通常是「找不到该项」。

## 不同应用批量生成链接的做法

如果你管理多个应用,最稳妥的方法是维护一张包名清单,用固定模板批量替换 id。不要手工复制地址栏,因为浏览器地址栏可能带有 hl(语言)、gl(国家)等与归因无关的参数,混入后会让链接不统一。建议清单至少包含:应用名称、包名、对应账号、上架状态。包名一经发布无法更改,所以它是唯一可靠的主键。

## 带来源参数的下载链接:referrer

要区分不同投放渠道带来的安装,可在链接后追加 &referrer=编码后的参数串,例如 utm_source、utm_medium、utm_campaign 组合。参数值需要做 URL 编码,多个键值对之间的 & 也要编码成 %26,否则会被截断。安装后,应用内可通过 Play Install Referrer API 读取这串内容,用于归因。此参数只在从商店安装后的首次启动才可读,且保留时间有限,所以应在首次启动时就读取并上报。

## market:// 与 intent 链接的适用场景

在 Android 应用或网页内唤起 Play 商店,可使用 market://details?id=包名,或 intent:// 形式。它们适合应用内「去评分」「去更新」入口。但这类链接在没有安装 Play 商店的设备、桌面浏览器或部分系统中无法打开,所以对外分享、广告落地页和二维码,应优先使用 https 的网页链接,由 Play 自行判断是否唤起商店 App。

## 发布前后如何验证链接可用

建议用三种环境各测一次:已登录 Google 账号的手机、未安装 Play 商店的环境(或桌面浏览器)、目标国家的网络环境。若链接打不开,按顺序排查:包名是否拼错、应用是否已通过审核并发布到正式轨道、目标国家是否在分发范围内、应用是否被暂停或下架、设备是否满足最低系统版本。链接中带 referrer 后无法显示详情页时,先去掉参数对比,通常是编码问题。具体审核状态和展示规则,以 Play Console 当时的页面为准。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Where to Find a Google Play Store Link and How to Build Download URLs for Multiple Apps

This file mirrors the site article. Official page: [Where to Find a Google Play Store Link and How to Build Download URLs for Multiple Apps](https://fatevia.cn/en/blog/articles/google-play-store-link-and-app-download-url.html)

How Google Play store links are structured, where to get them from the web and Play Console, and how to generate per-app, per-channel URLs with a referrer parameter.

## A Play link is a fixed prefix plus the package name

Every public Google Play listing follows one pattern: https://play.google.com/store/apps/details?id=PACKAGE_NAME. The id is the app's applicationId, set in the Android build config, not its display name. If you know the package name, you can build the link without logging into anything. Language and country variants share the same base URL; they differ only by optional parameters.

## Three reliable places to get it

For a live app, search on play.google.com and copy the address bar URL. In Play Console, open the app and follow the link to its store listing from the overview or main store listing area. In your own project, read applicationId and assemble the URL. The link only works for regular users once the app is published on a production track and available in their country. An app that exists only in internal testing or is unpublished typically shows a 'not found' page.

## Generating links for many apps

Keep a table of package names and apply one template. Avoid copying from the browser bar, which may carry hl or gl parameters that make links inconsistent. Useful columns are app name, package name, owning account and release status. The package name cannot be changed after publishing, so it is the only dependable key.

## Tracking channels with the referrer parameter

Append &referrer= followed by an encoded string such as utm_source, utm_medium and utm_campaign. The value must be URL-encoded, including the & between inner pairs (as %26), or it gets truncated. After installation, the app can read it through the Play Install Referrer API on first launch. It is available for a limited window, so read and report it immediately on first run.

## When to use market:// or intent links

market://details?id=PACKAGE_NAME and intent links suit in-app prompts such as rate or update buttons. They fail on desktop browsers and devices without the Play Store. For ads, shared pages and QR codes, use the https link and let the platform decide whether to open the app.

## Verifying the link before you ship it

Test on a signed-in phone, on a desktop browser, and from the target country's network. If a link fails, check in order: typo in the package name, whether the app is approved and on a production track, country availability, whether the app is suspended or removed, and device minimum version. If adding referrer breaks the page, remove it and compare; the cause is usually encoding. Review states and display rules can change, so rely on the current Play Console pages.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
