---
name: qq-clawbot-setup
description: 安装和配置 QQ ClawBot 插件（QQ OpenClaw Plugin）。当用户要求安装 QQ 插件、配置 QQ 机器人、设置 QQ 消息通道时使用此技能。支持安装官方插件、配置 AppId/AppSecret 凭证、验证连接状态。
license: MIT
metadata:
  author: flp516
  version: "2.0"
---

# QQ ClawBot 安装配置指南

## 概述

QQ ClawBot 是腾讯官方推出的 OpenClaw 插件，支持通过 QQ 私聊、群聊、频道与 ClawBot 对话，以及调用 QQ 相关工具（频道管理、定时提醒等）。

## 前置条件

1. 已注册 QQ 开放平台账号：https://q.qq.com/
2. 已创建机器人并获取 AppID 和 AppSecret
3. 机器人已通过审核并发布

## 安装流程

### 步骤 1：安装 QQ ClawBot 插件

```bash
openclaw plugins install @tencent-connect/openclaw-qqbot@latest
```

### 步骤 2：修复插件目录权限

插件安装后，需要修复目录权限（否则会被安全机制阻止加载）：

```bash
chmod 755 ~/.openclaw/extensions/openclaw-qqbot
chmod 755 ~/.openclaw/extensions
```

### 步骤 3：配置凭证

使用正确的配置命令设置 AppID 和 AppSecret：

```bash
openclaw config set 'channels.qqbot.appId' '<your-appId>'
openclaw config set 'channels.qqbot.clientSecret' '<your-clientSecret>'
```

**重要**：
- 不要使用 `openclaw channels add` 命令，它不会正确设置凭证
- AppID 和 clientSecret 必须从 QQ 开放平台获取，确保正确无误

### 步骤 4：将插件加入允许列表

```bash
openclaw config set plugins.allow '["openclaw-qqbot", "weibo-openclaw-plugin", "xiaoyi-channel"]'
```

### 步骤 5：重启 Gateway

```bash
pkill -f "openclaw gateway" 2>/dev/null
sleep 2
openclaw gateway &
```

### 步骤 6：验证安装

```bash
openclaw status
```

确认输出中 `QQ Bot` 显示 `ON · OK · configured`。

### 步骤 7：检查连接日志

```bash
tail -100 /tmp/openclaw/openclaw-$(date +%Y-%m-%d).log | grep -i qqbot
```

确认看到以下关键日志：
- `Access token obtained successfully` — 凭证有效
- `WebSocket connected` — WebSocket 连接成功
- `Hello received` — 握手成功
- `Ready with 群聊+私信+频道+交互` — 功能就绪
- `Gateway ready` — Gateway 启动完成

## 内置工具

安装成功后，以下工具可用：

| 工具名称 | 功能说明 |
|---------|---------|
| `qqbot_channel_api` | QQ 频道管理 API（频道列表、子频道、成员、发帖等） |
| `qqbot_remind` | QQ 定时提醒（创建、查询、删除） |

## 机器人功能

QQ ClawBot 支持以下功能：

| 功能 | 说明 |
|-----|------|
| 私聊对话 | 用户可以直接与机器人私聊 |
| 群聊对话 | 机器人在群聊中响应消息 |
| 频道对话 | 机器人在 QQ 频道中响应消息 |
| 斜杠命令 | `/bot-ping`、`/bot-version`、`/bot-help` |
| 富媒体消息 | 图片、语音、视频、文件 |
| Markdown 格式 | 支持 Markdown 格式消息 |

## 常见问题

### Q: 插件安装后显示 "unloaded"

检查插件目录权限：
```bash
ls -la ~/.openclaw/extensions/openclaw-qqbot
```

如果权限是 `777`，需要修复为 `755`：
```bash
chmod 755 ~/.openclaw/extensions/openclaw-qqbot
```

### Q: Gateway 显示 "invalid appid or secret"

这是凭证错误，请确认：
1. AppID 和 clientSecret 是否正确
2. 是否从 QQ 开放平台正确复制
3. 是否有多余的空格或换行

### Q: QQ Bot 显示 "OK · configured" 但不响应消息

检查连接日志：
```bash
tail -100 /tmp/openclaw/openclaw-$(date +%Y-%m-%d).log | grep -i qqbot
```

如果没有看到 `WebSocket connected` 和 `Ready` 日志，说明连接未成功。

### Q: 如何获取 AppID 和 AppSecret？

1. 登录 QQ 开放平台：https://q.qq.com/
2. 进入「机器人管理」页面
3. 选择你的机器人
4. 在「基本信息」页面查看 AppID
5. 在「开发设置」页面查看或重置 AppSecret

## 相关链接

- [QQ 开放平台](https://q.qq.com/)
- [OpenClaw 文档](https://docs.openclaw.ai)
- [ClawHub 技能市场](https://clawhub.com)
