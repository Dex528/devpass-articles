---
title: "Google Ads 转化追踪显示 0 次转化：按标签、gclid 与计入设置分层排查"
canonical: https://fatevia.cn/blog/articles/google-ads-zero-conversions-tag-gclid-attribution.html
canonical_en: https://fatevia.cn/en/blog/articles/google-ads-zero-conversions-tag-gclid-attribution.html
date: 2026-10-14
source: https://fatevia.cn/
---

# Google Ads 转化追踪显示 0 次转化：按标签、gclid 与计入设置分层排查

本文件是网站文章的副本。正式页面：[Google Ads 转化追踪显示 0 次转化：按标签、gclid 与计入设置分层排查](https://fatevia.cn/blog/articles/google-ads-zero-conversions-tag-gclid-attribution.html)

Google Ads 转化次数为零时，先判断问题出在标签没触发、点击标识丢失，还是转化没被计入目标。本文按转化状态、标签请求、gclid 传递和转化设置给出排查顺序与停止条件。

## 先看转化操作的状态，再看广告系列报表

在 Google Ads 的「目标 > 转化 > 摘要」里，每个转化操作都有状态。常见的有「正在记录转化」「无近期转化」「未验证」「不活跃」「标签不活跃」等，具体名称和判定条件以后台当时的页面为准。这个状态比广告系列报表更接近真相：如果状态是未验证或标签不活跃，说明 Google 从未收到过有效的标签信号，问题在安装层，不必去调出价和素材。如果状态是「无近期转化」，说明标签曾经工作过，要查的是最近的改动，例如改版、换了结算页、上了 Cookie 弹窗。

## 把问题拆成三层：没触发、没归因、没计入

转化为零只是结果，根因落在三层之一。第一层，标签没有在转化动作发生时触发。第二层，标签触发了，但请求里没有可用的点击标识，无法对应到某次广告点击。第三层，转化已被记录，但没有进入你正在看的列或目标，比如转化操作被设为次要，或报表按点击日期归属而转化日期较晚。先确定在哪一层，再动手，否则很容易在标签上反复重装，而真正的原因是第三层的设置。

## 标签层：用请求而不是后台提示来确认触发

用 Tag Assistant 或浏览器开发者工具的 Network 面板，走一遍真实的转化路径，在转化页面观察是否发出了发往 Google 的转化请求，并核对请求里的 Google Ads ID 与转化标签是否和后台那条转化操作一致。常见故障有：复制了别的转化操作的标签；用 GTM 时只建了转化标签，没有配转化链接器（Conversion Linker）；触发条件绑在「感谢页」，但站点是单页应用，路由变化时并不会触发页面加载事件；表单提交成功后才跳转，但触发器绑在了按钮点击上，导致失败提交也被记录，或者异步请求在跳转时被取消。只要 Network 里看不到转化请求，就不要继续查归因。

## 归因层：gclid 在哪一跳丢了

标签触发后，Google 要靠点击标识把转化对应回广告。先确认账户开启了自动标记（auto-tagging），再沿着用户路径检查落地页 URL 上是否带 gclid，以及它在跳转中有没有被去掉。典型的丢失点有：落地页经过短链或中转页重定向时丢参数；转化发生在独立的支付域名或第三方预约系统，跨域后没有共享标识；浏览器或广告拦截器阻止了标签；站点的同意管理在用户未同意时不允许写入广告存储，而同意模式（Consent Mode）又没有正确配置。如果你做线下转化导入，gclid 必须在表单时被保存下来，丢了就无法回传。增强型转化可以补充归因，但它依赖正确采集的用户数据，不能替代基础标签。

## 计入层：转化存在，只是没出现在你看的地方

检查三处设置。其一，该转化操作是主要还是次要，次要转化不会进入「转化次数」列，只在「所有转化次数」里显示，也不会参与自动出价优化。其二，广告系列使用的是账户级目标还是自定义的广告系列级目标，如果该转化类别没有包含在内，它同样不会被计入。其三，时间口径：转化默认归属到点击发生的日期，且回传有延迟，所以点击后几天才下单的转化，出现在更早的日期上。需要对账时，加上「按转化时间」的列，再和后台订单对比。如果你通过 GA4 导入转化，还要确认 GA4 与 Google Ads 已关联，对应事件已标记为关键事件，导入的数据通常比直接标签更晚。

## 什么时候该停下来，什么时候换方案

可以用一个简单的标准：在真实广告点击路径下，如果转化请求能看到、请求带有点击标识、转化操作是主要且在目标内，但很久仍然为零，再继续调标签意义不大，此时应检查实际流量是否足够，以及是否真的发生过转化。反过来，如果站点大量转化发生在跨域支付或 App 内，网页标签天生拿不到完整路径，应改为导入离线转化、使用 GA4 或服务端方式回传，而不是继续堆标签。每次改动只动一层，并记录改动前后的状态，这样出了问题才能回退。注意不要为了测试而反复点击自己的广告，这会产生无效点击，也污染数据，用 Tag Assistant 和预览模式验证标签即可。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Google Ads Conversion Tracking Showing 0 Conversions: Debug the Tag, the gclid and the Counting Settings

This file mirrors the site article. Official page: [Google Ads Conversion Tracking Showing 0 Conversions: Debug the Tag, the gclid and the Counting Settings](https://fatevia.cn/en/blog/articles/google-ads-zero-conversions-tag-gclid-attribution.html)

When Google Ads reports zero conversions, the cause is either a tag that never fires, a click ID that gets lost, or a conversion that isn't counted in your goals. Here is the order to check them, and when to stop.

## Start with the conversion action status, not the campaign report

Under Goals > Conversions > Summary, each conversion action carries a status such as recording conversions, no recent conversions, unverified, or tag inactive. The exact labels and criteria depend on what the interface shows at the time. This status is closer to the truth than a campaign report. Unverified or inactive means Google has never received a usable tag signal, so the problem sits in installation and bids or creatives are irrelevant. No recent conversions means the tag worked before, so look at recent changes: a redesign, a new checkout page, a new cookie banner.

## Split the problem into three layers: not fired, not attributed, not counted

Zero is only the symptom. The tag may not fire when the conversion happens. It may fire without a usable click identifier, so Google cannot tie it to an ad click. Or the conversion may be recorded but not land in the column or goal you are looking at, for example because the action is secondary, or the report assigns it to an earlier click date. Identify the layer first. Otherwise you end up reinstalling the tag repeatedly when the real cause is a setting in the third layer.

## Tag layer: confirm firing from the request, not from the dashboard

Walk through a real conversion path with Tag Assistant or the browser's Network panel. On the conversion page, look for a request to Google and check that the Google Ads ID and conversion label match the action in your account. Typical faults: copying the label from a different conversion action; using GTM with a conversion tag but no Conversion Linker; binding the trigger to a thank-you page in a single-page app where route changes don't reload the page; or firing on a button click so failed submissions count, or the request is cancelled by the redirect. If no conversion request appears in the Network panel, don't move on to attribution yet.

## Attribution layer: where did the gclid go

Google relies on a click identifier to match a conversion to an ad. Confirm auto-tagging is on, then follow the user path and check that gclid appears on the landing URL and survives every hop. Common loss points: short links or interstitial redirects that drop parameters; a payment or booking system on a separate domain without cross-domain handling; browsers or blockers stopping the tag; consent tools that deny ad storage while Consent Mode is not configured correctly. For offline conversion imports, the gclid must be stored with the lead at form time, or it cannot be sent back. Enhanced conversions can add matching signals, but they depend on correctly collected data and do not replace a working base tag.

## Counting layer: the conversion exists but isn't where you look

Check three things. First, whether the action is primary or secondary: secondary actions stay out of the Conversions column, show only under All conversions, and do not drive automated bidding. Second, whether the campaign uses account-level goals or custom campaign-level goals, and whether this conversion category is included. Third, the time basis: conversions are by default attributed to the click date and arrive with a delay, so a purchase days after the click appears on an earlier date. For reconciliation, add the by-conversion-time columns and compare against your order records. If you import from GA4, confirm the properties are linked and the event is marked as a key event; imports usually arrive later than direct tags.

## When to stop debugging and when to change the method

A simple rule: if the request is visible on a real ad-click path, carries a click identifier, and the action is primary and within the goal, but the count stays at zero over a long period, more tag tweaking will not help. Check whether the traffic is large enough and whether any conversion actually happened. Conversely, if most conversions happen on a separate payment domain or inside an app, a web tag cannot see the full path. Move to offline imports, GA4, or server-side sending instead of adding more tags. Change one layer at a time and note the status before and after, so you can roll back. And don't click your own ads to test; that produces invalid clicks and noise. Tag Assistant and preview mode are enough.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
