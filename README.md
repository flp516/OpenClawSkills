# OpenClawSkills

OpenClaw技能集合 - 包含各种Channel配置技能

## 技能列表

| 技能 | 说明 | 文档 |
|-----|------|------|
| [钉钉Channel配置](./dingtalk-channel-setup/) | 快速配置钉钉机器人与OpenClaw的集成 | [GUIDE.md](./dingtalk-channel-setup/GUIDE.md) |
| [飞书Channel配置](./feishu-channel-skill/) | 快速配置飞书机器人与OpenClaw的集成 | [GUIDE.md](./feishu-channel-skill/GUIDE.md) |
| [企业微信机器人配置](./skills/wecom-bot-setup/) | 快速配置企业微信智能机器人与OpenClaw的集成 | [SKILL.md](./skills/wecom-bot-setup/SKILL.md) |

## 快速开始

### 钉钉Channel配置

```bash
cd dingtalk-channel-setup
./scripts/configure.sh <clientId> <clientSecret>
```

### 飞书Channel配置

```bash
cd feishu-channel-skill
./scripts/configure.sh <appId> <appSecret>
```

## 目录结构

```
OpenClawSkills/
├── README.md                    # 项目说明
├── dingtalk-channel-setup/      # 钉钉Channel配置技能
│   ├── SKILL.md                 # 技能定义
│   ├── GUIDE.md                 # 详细指导书
│   ├── TEST_REPORT.md           # 自测报告
│   └── scripts/
│       └── configure.sh         # 自动配置脚本
├── feishu-channel-skill/        # 飞书Channel配置技能
│   ├── SKILL.md                 # 技能定义
│   ├── GUIDE.md                 # 详细指导书
│   └── scripts/
│       └── configure.sh         # 自动配置脚本
└── skills/
    └── wecom-bot-setup/         # 企业微信机器人配置技能
        ├── SKILL.md             # 技能定义
        ├── scripts/
        │   └── setup_wecom_bot.py  # 自动配置脚本
        └── references/
            └── quick-reference.md  # 快速参考
```

## 贡献

欢迎贡献新的技能！

## License

MIT

## 相关链接

- [OpenClaw文档](https://docs.openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [ClawHub技能市场](https://clawhub.com)
