---
title: "Telegram 机器人在群里收不到普通消息：关闭隐私模式还是设为管理员"
canonical: https://fatevia.cn/blog/articles/telegram-bot-group-privacy-mode-admin-2.html
canonical_en: https://fatevia.cn/en/blog/articles/telegram-bot-group-privacy-mode-admin-2.html
date: 2026-10-18
source: https://fatevia.cn/
---

# Telegram 机器人在群里收不到普通消息：关闭隐私模式还是设为管理员

本文件是网站文章的副本。正式页面：[Telegram 机器人在群里收不到普通消息：关闭隐私模式还是设为管理员](https://fatevia.cn/blog/articles/telegram-bot-group-privacy-mode-admin-2.html)

Telegram 机器人在群里只响应命令、收不到普通消息，多半是隐私模式在起作用。本文说明隐私模式的接收范围、BotFather 关闭步骤、为何改完仍无效，以及何时才需要管理员权限。

## 现象的根因通常是隐私模式，而不是代码

新建的 Telegram 机器人默认开启隐私模式（Group Privacy）。在群里，开启该模式的机器人只会收到与它直接相关的消息，而不是群内所有发言。所以常见现象是：/命令有反应，普通聊天内容在 getUpdates 或 Webhook 里一条也看不到。这是平台的设计行为，调整日志级别或重写处理函数都不会改变它。

## 隐私模式下机器人能收到什么

按官方文档，隐私模式下机器人能收到：以斜杠开头的命令消息、通过 @用户名 提到机器人的消息、对机器人自己所发消息的回复，以及群内的服务消息（成员加入、退出等）。其余普通文本、图片和转发内容不会推送。因此，只做命令式交互的机器人通常不需要改动任何设置；需要做关键词监听、内容审核、统计群聊活跃度的机器人才必须解决这个问题。具体细节以 Telegram 官方 Bot API 文档当时的说明为准。

## 在 BotFather 里关闭隐私模式的步骤

打开与 BotFather 的对话，发送 /mybots，选择目标机器人，进入 Bot Settings，再进入 Group Privacy，选择 Turn off。也可以用 /setprivacy 命令，选中机器人后选 Disable。操作后 BotFather 会提示当前状态。这个设置是机器人级别的，不是单个群级别的，关闭后所有已加入和将要加入的群都按新规则处理。

## 改完仍然无效：要把机器人移出群再重新拉入

这是最常见的坑。隐私模式的变更不会对已在群里的机器人即时生效，官方的做法是先把机器人移出群，再重新添加。重新添加后，可以发一条不带命令、不提及机器人的普通消息，看更新里是否出现 message 对象来验证。如果群很多，需要逐个群处理，这也是在上线前就确定好隐私模式设置的原因。

## 什么时候才需要设为管理员

机器人被设为群管理员后，无论隐私模式是否开启，都能收到群内所有消息。所以单从收消息看，管理员是关闭隐私模式的替代手段。但管理员权限的目的是执行操作：删除消息、封禁成员、置顶、邀请链接管理等。判断标准很简单：只需要读取消息，就关闭隐私模式；需要删消息或限制成员，就设为管理员，并且只勾选用到的权限。仅为了收消息就授予完整管理员权限，会扩大机器人 token 泄露时的损失范围。

## 关了隐私模式仍收不到时的其他排查点

逐项核对：一是 Webhook 与 getUpdates 不能同时使用，设置了 Webhook 后轮询会返回冲突错误；二是 setWebhook 或 getUpdates 的 allowed_updates 是否漏掉了 message；三是群升级为超级群后 chat_id 会变化，旧 ID 需要更新；四是机器人收不到其他机器人发出的消息，这是平台限制；五是频道内容以 channel_post 而不是 message 到达，字段名不同；六是论坛话题群里要读取 message_thread_id 才能区分话题。若以上都排除仍无更新，再检查服务器是否真正收到请求。

## Telegram 频道

- [企业出海综合服务、开发者账号与定制开发](https://t.me/s/GoogleAppleDev)
- [苹果开发者账号](https://t.me/s/iOSDeveloperService)
- [谷歌开发者账号](https://t.me/s/GoogleDeveloperService)
- [谷歌广告账户](https://t.me/s/GoogleAdsChina)
- [TG 机器人定制](https://t.me/s/TGBotDevService)

---

# Telegram Bot Not Seeing Group Messages: Disable Privacy Mode or Make It Admin?

This file mirrors the site article. Official page: [Telegram Bot Not Seeing Group Messages: Disable Privacy Mode or Make It Admin?](https://fatevia.cn/en/blog/articles/telegram-bot-group-privacy-mode-admin-2.html)

If your Telegram bot only reacts to commands in groups, privacy mode is the usual cause. This article covers what the bot can receive, how to disable it in BotFather, why changes may not apply, and when admin rights are justified.

## The cause is usually privacy mode, not your code

New Telegram bots have privacy mode (Group Privacy) enabled by default. In a group, such a bot only receives messages that are directly related to it, not everything members write. That explains the typical symptom: slash commands work, but ordinary chat never shows up in getUpdates or at your webhook. It is platform behavior, so changing log levels or rewriting handlers will not fix it.

## What a bot receives while privacy mode is on

According to the official docs, the bot gets slash commands, messages that mention it by @username, replies to its own messages, and service messages such as members joining or leaving. Plain text, photos and forwards are not delivered. A command-only bot usually needs no change. Bots that do keyword monitoring, moderation or activity statistics must change this. Check the current Bot API documentation for the exact list.

## Turning privacy mode off in BotFather

Open a chat with BotFather, send /mybots, pick the bot, go to Bot Settings, then Group Privacy, and choose Turn off. Alternatively use /setprivacy and select Disable for the bot. The setting applies to the bot as a whole, not to individual groups, so it covers every group the bot is in or joins later.

## If nothing changes, remove the bot and add it again

This is the most common trap. The new privacy setting does not take effect for groups the bot is already in. Remove the bot from the group and add it back. To verify, post a plain message with no command or mention and check whether a message object appears in the updates. With many groups this is tedious, so decide on the privacy setting before rollout.

## When admin rights are the right choice

A bot that is a group admin receives all messages regardless of privacy mode, so admin works as an alternative for reading. But admin exists for actions: deleting messages, banning members, pinning, managing invite links. A simple rule: if you only need to read, disable privacy mode; if you need to delete or restrict, make it admin and tick only the permissions it uses. Granting full admin just to read messages enlarges the damage if the bot token leaks.

## Other causes if messages still do not arrive

Check these in order: webhook and getUpdates cannot be used together, and polling returns a conflict error while a webhook is set; allowed_updates may exclude message; a group upgraded to a supergroup gets a new chat_id; bots do not receive messages sent by other bots; channel content arrives as channel_post, not message; in forum groups, read message_thread_id to tell topics apart. If all of that checks out and no updates appear, confirm that your server actually receives requests.

## Telegram channels

- [Enterprise overseas services: developer accounts and custom development](https://t.me/s/GoogleAppleDev)
- [Apple developer accounts](https://t.me/s/iOSDeveloperService)
- [Google developer accounts](https://t.me/s/GoogleDeveloperService)
- [Google Ads accounts](https://t.me/s/GoogleAdsChina)
- [Telegram bots](https://t.me/s/TGBotDevService)
