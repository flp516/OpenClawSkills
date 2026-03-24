---
name: weixin-clawbot-setup
description: 微信 ClawBot 插件配置指南。帮助用户在 OpenClaw 中配置微信 ClawBot 插件，实现通过微信与 OpenClaw 对话。触发词：微信ClawBot配置、微信插件安装、微信扫码绑定、openclaw-weixin。
---

# 微信 ClawBot 配置技能

## 概述

本技能帮助用户在 OpenClaw 中配置微信 ClawBot 插件，实现通过微信与 OpenClaw 进行对话。

## 前置条件

- OpenClaw 已安装并正常运行
- 手机上安装了微信
- OpenClaw Gateway 正在运行

## 微信版本要求

### iOS 微信

| 要求 | 说明 |
|------|------|
| **最低版本** | 8.0.70 及以上 |
| **推荐版本** | 最新版本 |
| **支持状态** | ✅ 完全支持 |

### Android 微信

| 要求 | 说明 |
|------|------|
| **最低版本** | 8.0.70 及以上（需等待官方逐步放量） |
| **推荐版本** | 最新版本 |
| **支持状态** | ✅ 支持（部分功能可能受限） |

**Android 用户注意**：
- 截至 2026 年 3 月，8.0.70 版本仅对 iOS 端开放，安卓用户需要等待官方逐步放量
- 如果没有插件权限，可以通过运行安装命令后扫码获得插件入口
- 未更新微信版本的安卓用户，每次关闭后需要重新扫码连接

### 鸿蒙微信

| 平台 | 版本 | 支持状态 | 备注 |
|------|------|----------|------|
| **鸿蒙双框** | HarmonyOS 5.0 以下 | ✅ 支持 | 兼容模式，类似 Android |
| **鸿蒙单框** | HarmonyOS 5.0 及以上 | ❌ 不支持 | 微信鸿蒙原生版，暂不支持 |

**版本说明**：
- **鸿蒙双框**：HarmonyOS 5.0 以下版本（4.x 及更早），微信以兼容模式运行（类似 Android），支持 ClawBot
- **鸿蒙单框**：HarmonyOS 5.0 及以上版本，微信为鸿蒙原生应用，目前暂不支持 ClawBot

## 微信版本支持总览

| 平台 | 版本要求 | 支持状态 | 备注 |
|------|----------|----------|------|
| **iOS** | 8.0.70+ | ✅ 完全支持 | 推荐使用 |
| **Android** | 8.0.70+ | ✅ 支持 | 需等待官方放量 |
| **鸿蒙双框** | HarmonyOS 5.0 以下 | ✅ 支持 | 兼容模式 |
| **鸿蒙单框** | HarmonyOS 5.0+ | ❌ 不支持 | 暂不支持 |

## 配置步骤

### 步骤一：更新微信版本

⚠️ **重要**：请先确保微信已更新到最新版本（iOS 需要 8.0.70 及以上）。

- **iOS**：App Store → 搜索「微信」→ 更新
- **Android**：应用商店 → 搜索「微信」→ 更新

### 步骤二：安装微信插件

在 OpenClaw 终端执行：

```bash
npx -y @tencent-weixin/openclaw-weixin-cli@latest install
```

等待安装完成，终端会显示一个二维码。

### 步骤三：微信扫码绑定

1. 打开手机微信
2. 点击右上角「+」→「扫一扫」
3. 扫描终端显示的二维码
4. 在微信中确认绑定
5. 等待终端显示「绑定成功」提示

### 步骤四：重启 Gateway

⚠️ **关键步骤**：扫码成功后，必须重启 Gateway 才能生效！

```bash
openclaw gateway restart
```

或者：

```bash
openclaw gateway stop
openclaw gateway
```

### 步骤五：验证连接

检查微信插件是否配置成功：

```bash
openclaw channels list
```

输出应包含：

```
Chat channels:
- openclaw-weixin <account-id>: configured, enabled
```

### 步骤六：测试对话

1. 在微信中找到「ClawBot」或绑定的公众号/小程序
2. 发送一条消息，例如「你好」
3. 等待 OpenClaw 回复

## 微信界面操作说明

### 扫码绑定

1. 打开微信 → 右上角「+」→「扫一扫」
2. 扫描二维码后，会显示绑定确认页面
3. 点击「确认绑定」或「关注」
4. 绑定成功后，会出现新的对话窗口

### 发送消息

1. 在微信聊天列表中找到 ClawBot 对话
2. 像普通聊天一样发送消息
3. OpenClaw 会自动回复

## 小艺 Claw 窗口操作说明

### 安装插件

```bash
npx -y @tencent-weixin/openclaw-weixin-cli@latest install
```

### 查看插件状态

```bash
openclaw channels list
```

### 重启 Gateway

```bash
openclaw gateway restart
```

### 查看日志

```bash
tail -f /tmp/openclaw/openclaw-$(date +%Y-%m-%d).log
```

### 查看凭证文件

```bash
ls -la ~/.openclaw/openclaw-weixin/
cat ~/.openclaw/openclaw-weixin/accounts.json
```

## 常见问题 Q&A

### Q1: 扫码成功但微信没有回复？

**原因**：Gateway 没有重启，新的凭证没有加载。

**解决方案**：
```bash
openclaw gateway restart
```

### Q2: `openclaw channels list` 中没有 openclaw-weixin？

**原因**：凭证文件不存在或格式错误。

**解决方案**：
1. 检查凭证文件是否存在：
   ```bash
   ls -la ~/.openclaw/openclaw-weixin/accounts.json
   ```
2. 如果不存在，重新运行安装命令：
   ```bash
   npx -y @tencent-weixin/openclaw-weixin-cli@latest install
   ```
3. 扫码后重启 Gateway

### Q3: 二维码过期了怎么办？

**解决方案**：重新运行安装命令，会生成新的二维码：
```bash
npx -y @tencent-weixin/openclaw-weixin-cli@latest install
```

### Q4: Gateway 启动失败？

**原因**：端口被占用或权限问题。

**解决方案**：
1. 检查是否有其他 Gateway 进程：
   ```bash
   ps aux | grep openclaw
   ```
2. 杀掉旧进程：
   ```bash
   kill -9 <pid>
   ```
3. 重新启动：
   ```bash
   openclaw gateway
   ```

### Q5: 如何取消绑定？

**解决方案**：
1. 删除凭证文件：
   ```bash
   rm -rf ~/.openclaw/openclaw-weixin/
   ```
2. 重启 Gateway：
   ```bash
   openclaw gateway restart
   ```
3. 在微信中取消关注或删除对话

### Q6: 提示「Unsupported channel: openclaw-weixin」？

**原因**：插件没有正确安装。

**解决方案**：
1. 重新安装插件：
   ```bash
   npx -y @tencent-weixin/openclaw-weixin-cli@latest install
   ```
2. 检查插件目录：
   ```bash
   ls -la ~/.openclaw/extensions/openclaw-weixin/
   ```

### Q7: 如何查看详细的错误日志？

**解决方案**：
```bash
tail -100 /tmp/openclaw/openclaw-$(date +%Y-%m-%d).log | grep -i "weixin\|error"
```

### Q8: 多个微信账号如何切换？

**解决方案**：
1. 查看已绑定的账号：
   ```bash
   cat ~/.openclaw/openclaw-weixin/accounts.json
   ```
2. 删除不需要的账号凭证：
   ```bash
   rm ~/.openclaw/openclaw-weixin/accounts/<account-id>.json
   ```
3. 更新 accounts.json 文件
4. 重启 Gateway

### Q9: 鸿蒙单框微信（HarmonyOS 5.0+）无法使用？

**原因**：鸿蒙单框微信是鸿蒙原生应用，目前 ClawBot 暂不支持。

**解决方案**：
- 等待后续更新支持
- 或使用其他设备（iOS/Android/鸿蒙双框）进行绑定

### Q10: iOS 微信扫码后提示「无法打开页面」？

**原因**：网络问题或微信版本过旧。

**解决方案**：
1. 更新微信到最新版本（至少 8.0.70）
2. 检查网络连接
3. 尝试使用蜂窝网络或切换 Wi-Fi

### Q11: Android 用户没有插件权限怎么办？

**原因**：插件仍在逐步放量中，部分用户暂未获得权限。

**解决方案**：
1. 直接运行安装命令：
   ```bash
   npx -y @tencent-weixin/openclaw-weixin-cli@latest install
   ```
2. 扫码连接后，微信会收到更新提示
3. 更新完成后，手机微信会出现「微信ClawBot」插件
4. 再次扫码完成连接

**注意**：未更新微信版本的安卓用户，每次关闭后需要重新扫码连接。

### Q12: 微信版本低于 8.0.70 能用吗？

**答案**：**不能**。

微信 ClawBot 插件需要微信 8.0.70 及以上版本才能使用。请先更新微信到最新版本。

### Q13: 一个微信号可以连接多个 OpenClaw 吗？

**答案**：**不能**。

一个微信号只能连接一个 OpenClaw 实例，但一个 OpenClaw 可以同时连接多个微信号。

### Q14: ClawBot 支持哪些消息类型？

| 消息类型 | 接收 | 发送 |
|----------|------|------|
| 文本消息 | ✅ | ✅ |
| 图片 | ✅ | ✅ |
| 视频 | ✅ | ✅ |
| 文件 | ✅ | ✅ |
| 语音消息 | ✅ | ❌ |
| 主动发送消息 | - | ✅ |

**注意**：微信不支持图文混合输入，只能逐条发送。

## 关键文件路径

| 文件/目录 | 说明 |
|----------|------|
| `~/.openclaw/extensions/openclaw-weixin/` | 插件安装目录 |
| `~/.openclaw/openclaw-weixin/accounts.json` | 账户索引文件 |
| `~/.openclaw/openclaw-weixin/accounts/*.json` | 各账户凭证文件 |
| `~/.openclaw/openclaw.json` | OpenClaw 主配置文件 |
| `/tmp/openclaw/openclaw-*.log` | 运行日志 |

## 快速检查清单

- [ ] 微信版本 ≥ 8.0.70：微信 → 我 → 设置 → 关于微信
- [ ] 插件已安装：`ls ~/.openclaw/extensions/openclaw-weixin/`
- [ ] 凭证已保存：`cat ~/.openclaw/openclaw-weixin/accounts.json`
- [ ] Gateway 已重启：`openclaw gateway status`
- [ ] 插件已启用：`openclaw channels list | grep weixin`
- [ ] 微信已绑定：在微信中找到 ClawBot 对话

## 总结

配置微信 ClawBot 的核心步骤：

1. **更新微信** → 确保版本 ≥ 8.0.70
2. **安装插件** → `npx -y @tencent-weixin/openclaw-weixin-cli@latest install`
3. **扫码绑定** → 用微信扫描二维码
4. **重启 Gateway** → `openclaw gateway restart`
5. **验证连接** → `openclaw channels list`

**最重要的一步**：扫码成功后，**必须重启 Gateway** 才能生效！