---
name: dingtalk-channel-setup
description: 钉钉Channel配置技能。帮助用户快速配置钉钉机器人与OpenClaw的集成，包括安装插件、配置凭证、重启服务。触发词：配置钉钉、钉钉channel、钉钉机器人、dingtalk配置。
license: MIT
metadata:
  author: flp516
  version: "2.0"
---

# 钉钉 Channel 配置技能

帮助用户快速配置钉钉机器人与OpenClaw的集成。

## 功能描述

- 安装钉钉Channel插件 (`clawdbot-dingtalk` - OpenClaw官方插件)
- 配置OpenClaw的 `openclaw.json` 文件
- 重启Gateway服务
- 验证配置状态

## 前置条件

### 钉钉开发者平台配置

用户需要先在钉钉开发者平台创建应用机器人：
1. 访问 https://open-dev.dingtalk.com/fe/app
2. 创建应用 → 添加机器人能力
3. **配置权限范围**（消息接收、消息发送）
4. 消息接收模式选择 **Stream模式**
5. 获取 Client ID 和 Client Secret
6. **发布应用**（重要！未发布则机器人不生效）

### OpenClaw环境检查

1. OpenClaw已安装（`which openclaw`）
2. Node.js已安装（v18+，`node --version`）
3. OpenClaw配置文件存在（`~/.openclaw/openclaw.json`）

## 配置步骤

### 1. 安装插件

```bash
mkdir -p ~/.openclaw/extensions/dingtalk
cd ~/.openclaw/extensions/dingtalk
npm init -y
npm install clawdbot-dingtalk@latest
```

### 2. 配置 openclaw.json

在 `~/.openclaw/openclaw.json` 中添加以下配置：

```json
{
  "channels": {
    "dingtalk": {
      "clientId": "你的Client ID",
      "clientSecret": "你的Client Secret",
      "enabled": true
    }
  },
  "entries": {
    "dingtalk": {
      "enabled": true
    }
  },
  "installs": {
    "dingtalk": {
      "source": "npm",
      "spec": "clawdbot-dingtalk@latest",
      "installPath": "/home/sandbox/.openclaw/extensions/dingtalk/node_modules/clawdbot-dingtalk",
      "version": "1.0.0"
    }
  },
  "plugins": {
    "allow": ["feishu", "dingtalk", "xiaoyi-channel"],
    "load": {
      "paths": [
        "/home/sandbox/openclaw/node_modules/openclaw/extensions/feishu",
        "/home/sandbox/.openclaw/extensions/dingtalk/node_modules/clawdbot-dingtalk"
      ]
    }
  }
}
```

### 3. 重启Gateway

```bash
openclaw gateway stop
openclaw gateway
```

### 4. 验证配置

```bash
# 检查日志
tail -100 /tmp/openclaw/openclaw-$(date +%Y-%m-%d).log | grep -i "DingTalk"

# 应该看到:
# [gateway/channels/dingtalk] [default] starting DingTalk provider
# DingTalk: configured
```

## 常见问题

### Q: 日志显示 "unknown channel id: dingtalk"

**原因**: 插件未正确加载或配置文件未更新

**解决方案**:
1. 确认插件安装路径正确
2. 确认 `plugins.load.paths` 包含插件路径
3. 重启gateway

### Q: 日志显示 "DingTalk: not configured"

**原因**: 缺少 Client ID 或 Client Secret

**解决方案**:
1. 检查 `channels.dingtalk` 配置
2. 确认凭证正确

### Q: 机器人搜索不到

**原因**: 应用未发布或权限未配置

**解决方案**:
1. 确认应用已发布到正式版或测试版
2. 确认已申请"消息接收"和"消息发送"权限
3. 发布后等待几分钟生效

### Q: 如何找到机器人

**方法**:
1. 钉钉App → 右上角"+" → "添加好友/机器人" → 搜索机器人名称
2. 通讯录 → 企业 → "机器人"分类下查找

### Q: Stream模式连接失败

**原因**: Stream模式未正确连接

**解决方案**:
1. 确认钉钉开发者平台配置了Stream模式
2. 检查网络连接
3. 查看gateway日志排查错误

## 插件信息

- **包名**: `clawdbot-dingtalk`
- **来源**: OpenClaw官方
- **Channel ID**: `dingtalk`
- **功能**: Stream模式、私聊/群聊、图片/文件消息

## 更新日志

### v2.0
- 插件来源从 `@largezhou/ddingtalk` 更新为 OpenClaw 官方 `clawdbot-dingtalk`
- Channel ID 从 `ddingtalk` 统一为 `dingtalk`

## 相关链接

- [钉钉开发者平台](https://open-dev.dingtalk.com/)
- [钉钉机器人文档](https://open.dingtalk.com/document/org/robot-overview)
- [OpenClaw文档](https://docs.openclaw.ai)
