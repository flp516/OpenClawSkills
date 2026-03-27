---
name: feishu-channel-setup
description: 飞书Channel配置技能。帮助用户配置飞书机器人与OpenClaw的集成，包括创建应用、配置权限、设置事件订阅。触发词：配置飞书、飞书channel、飞书机器人、feishu配置。
license: MIT
metadata:
  author: flp516
  version: "2.0"
---

# 飞书 Channel 配置技能

## 概述

帮助用户配置飞书 Channel，实现通过飞书与 OpenClaw 对话。

## 前置条件

- OpenClaw 运行环境
- 飞书企业账号（管理员权限）
- 公网可访问的服务器（或使用 ngrok）

## 安装流程

### 步骤 1：创建飞书企业自建应用

1. 登录 [飞书开放平台](https://open.feishu.cn/)
2. 进入「开发者后台」
3. 点击「创建企业自建应用」
4. 填写应用名称、描述、图标

### 步骤 2：配置应用权限

在应用详情页，配置以下权限：

| 权限名称 | 权限说明 |
|---------|---------|
| `im:message` | 获取与发送消息 |
| `im:message:send_as_bot` | 以应用身份发消息 |
| `contact:user.base:readonly` | 获取用户基本信息 |

### 步骤 3：获取 App ID 和 App Secret

在应用详情页的「凭证与基础信息」中获取：
- **App ID**
- **App Secret**

### 步骤 4：配置 OpenClaw

编辑 `~/.openclaw/openclaw.json`，在 `channels` 下添加：

```json
"feishu": {
  "enabled": true,
  "appId": "你的App ID",
  "appSecret": "你的App Secret",
  "encryptKey": "你的Encrypt Key（可选）",
  "verificationToken": "你的Verification Token（可选）"
}
```

### 步骤 5：配置事件订阅

1. 在飞书应用详情页，找到「事件订阅」
2. 设置请求网址：`https://your-domain.com/feishu/webhook`
3. 订阅以下事件：
   - `im.message.receive_v1`（接收消息）

### 步骤 6：发布应用并测试

1. 在飞书应用详情页，点击「申请发布」
2. 管理员审批通过后，应用生效
3. 在飞书中搜索应用名称，开始对话

## 常见问题

### Q: 事件订阅验证失败

- 确认请求网址可公网访问
- 检查 OpenClaw gateway 是否正常运行
- 确认端口 18789 未被占用

### Q: 消息无响应

- 确认事件订阅已开启
- 检查 OpenClaw 日志：`openclaw logs`
- 确认应用已发布并添加到可见范围

## 相关链接

- [飞书开放平台](https://open.feishu.cn/)
- [飞书 API 文档](https://open.feishu.cn/document)
- [OpenClaw 文档](https://docs.openclaw.ai)
- [ClawHub 技能市场](https://clawhub.com)
