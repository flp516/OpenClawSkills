# Feishu Channel Setup Skill

帮助用户配置飞书 Channel，实现通过飞书与小艺对话。

## 前置要求

- OpenClaw 运行环境
- 飞书企业账号（管理员权限）
- 公网可访问的服务器（或使用 ngrok）

## 配置步骤概览

1. 创建飞书企业自建应用
2. 配置应用权限
3. 获取 App ID 和 App Secret
4. 配置 OpenClaw
5. 配置事件订阅
6. 发布应用并测试

## 使用方式

用户说：「帮我配置飞书 channel」「我想在飞书里和你对话」

## 技术说明

- 飞书 API 文档：https://open.feishu.cn/document
- 事件订阅方式：Webhook 或长链接
- 默认端口：18789

## 相关文件

- `GUIDE.md` - 详细配置指导书
- `config-template.json` - 配置模板
