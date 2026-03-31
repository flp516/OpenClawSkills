---
name: weixin-clawbot-setup
description: 微信 ClawBot 插件配置指南。帮助用户在小艺 Claw 中配置微信 ClawBot 插件，实现通过微信与小艺 Claw 进行对话。触发词：微信ClawBot配置、微信插件安装、微信扫码绑定、openclaw-weixin。
---

# 微信 ClawBot 配置技能

## 概述

本技能帮助用户在小艺 Claw 中配置微信 ClawBot 插件，结合用户在微信端扫描等配置操作，实现通过微信与小艺 Claw 进行对话。

## 微信版本支持

**小艺 Claw 可对接不同系统手机上的微信**，以下为不同系统手机的微信版本要求：

<table>
  <thead>
    <tr>
      <th style="text-align: center;">平台</th>
      <th>待连接微信的版本要求</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3" style="text-align: center; vertical-align: middle;"><strong>小艺 Claw（HarmonyOS 鸿蒙）</strong></td>
      <td>iOS 微信 8.0.70 及以上</td>
    </tr>
    <tr>
      <td>Android 微信 8.0.69 及以上</td>
    </tr>
    <tr>
      <td>HarmonyOS（鸿蒙）微信 8.0.16 及以上</td>
    </tr>
  </tbody>
</table>

注：截至2026/03/31，鸿蒙版微信8.0.16未发布，发布及支持时间请以实际体验为准

## 配置步骤

⚠️ **重要提示**：
1. 以下步骤必须在**同一个会话中连续完成**，中途不要结束对话
2. 如果安装过程要重启 Gateway，连接可能断开，断开后请稍后继续追问微信 Channel 安装进展

### 步骤一：更新微信版本

请先确保微信已更新到支持版本：
- **iOS**：App Store → 搜索「微信」→ 更新
- **Android**：应用商店 → 搜索「微信」→ 更新
- **鸿蒙**：华为应用市场 → 搜索「微信」→ 更新

### 步骤二：安装/登录微信插件

**首次安装**（插件目录不存在时）：
```bash
npx -y @tencent-weixin/openclaw-weixin-cli@latest install
```

**已安装插件，生成登录二维码**：
```bash
openclaw channels login --channel openclaw-weixin
```

⚠️ **重要**：插件已安装后，请使用 `login` 命令生成二维码，不要重复使用 `install`。

### 步骤三：微信扫码绑定

1. 打开手机微信 → 右上角「+」→「扫一扫」
2. 扫描二维码或点击二维码链接
3. 在微信中确认绑定
4. 等待显示「绑定成功」

⚠️ **关键**：扫码成功后，**不要结束当前对话**，立即继续执行后续步骤！

### 步骤四：启用插件配置

扫码成功后，**立即**在配置文件中启用插件：

1. 编辑配置文件：
   ```bash
   nano ~/.openclaw/openclaw.json
   ```

2. 在 `plugins.entries` 中添加：
   ```json
   "openclaw-weixin": {
     "enabled": true
   }
   ```

3. 保存并验证：
   ```bash
   cat ~/.openclaw/openclaw.json | grep -A 3 "openclaw-weixin"
   ```

### 步骤五：重启 Gateway

修改配置后，**立即**重启 Gateway：
```bash
openclaw gateway restart
```

### 步骤六：验证连接

```bash
openclaw channels list
```

输出应包含：
```
- openclaw-weixin <account-id>: configured, enabled
```

### 步骤七：测试对话

在微信中找到 ClawBot 对话，发送消息测试。

---

## 常见问题

### Q1: 扫码成功但微信没有回复？

**原因**：Gateway 没有重启，凭证没有加载。

**解决**：
```bash
openclaw gateway restart
```

### Q2: 二维码过期了怎么办？

**解决**：
```bash
openclaw channels login --channel openclaw-weixin
```

### Q3: 提示「Unsupported channel: openclaw-weixin」？

**原因**：插件没有正确安装。

**解决**：
```bash
npx -y @tencent-weixin/openclaw-weixin-cli@latest install
```

### Q4: 如何取消绑定？

**解决**：
```bash
rm -rf ~/.openclaw/openclaw-weixin/
openclaw gateway restart
```

### Q5: 扫码成功后，为什么微信发消息没有回复？

**原因**：插件没有在 `openclaw.json` 的 `plugins.entries` 中启用。

**诊断**：
```bash
cat ~/.openclaw/openclaw.json | grep -A 5 "openclaw-weixin"
```

如果没有 `"openclaw-weixin": { "enabled": true }`，请按步骤四启用插件。

---

## 命令速查表

| 场景 | 命令 |
|------|------|
| **首次安装插件** | `npx -y @tencent-weixin/openclaw-weixin-cli@latest install` |
| **生成登录二维码** | `openclaw channels login --channel openclaw-weixin` |
| **查看插件状态** | `openclaw channels list` |
| **重启 Gateway** | `openclaw gateway restart` |
| **查看日志** | `tail -f /tmp/openclaw/openclaw-$(date +%Y-%m-%d).log` |
| **查看凭证** | `cat ~/.openclaw/openclaw-weixin/accounts.json` |

---

## 关键文件路径

| 文件/目录 | 说明 |
|----------|------|
| `~/.openclaw/extensions/openclaw-weixin/` | 插件安装目录 |
| `~/.openclaw/openclaw-weixin/accounts.json` | 账户凭证文件 |
| `~/.openclaw/openclaw.json` | OpenClaw 主配置文件 |

---

## 配置检查清单

- [ ] 微信版本符合要求
- [ ] 插件已安装：`ls ~/.openclaw/extensions/openclaw-weixin/`
- [ ] 凭证已保存：`cat ~/.openclaw/openclaw-weixin/accounts.json`
- [ ] **插件已启用**：`cat ~/.openclaw/openclaw.json | grep -A 3 "openclaw-weixin"` ⚠️ 关键！
- [ ] Gateway 已重启：`openclaw gateway status`
- [ ] 插件已加载：`openclaw channels list | grep weixin`

---

## 总结

**核心步骤（必须在同一会话中连续完成）**：

1. 更新微信 → 确保版本符合要求
2. 安装/登录插件 → 首次用 `install`，后续用 `login`
3. 扫码绑定 → 用微信扫描二维码
4. 启用插件 → 在 `openclaw.json` 中添加配置 ⚠️ **容易遗漏！**
5. 重启 Gateway → `openclaw gateway restart`
6. 验证连接 → `openclaw channels list`
7. 测试对话 → 在微信中发送消息

**最容易失败的原因**：扫码成功后对话结束，没有在同一会话中完成后续步骤！

**成功秘诀**：扫码成功后，**立即、连续**执行「启用插件 → 重启 Gateway → 验证」三步！
