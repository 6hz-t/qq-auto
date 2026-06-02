# QQ 每日自动任务 - 等级加速

基于 GitHub Actions 的 QQ 每日自动任务，每天定时执行等级加速、签到等操作，助力 QQ 等级升级。

## ✨ 功能

| 功能 | 说明 |
|------|------|
| 📊 等级查询 | 查询当前等级、加速天数、好友排名 |
| ⏰ 登录加速 | 每日登录获取基础活跃天数 |
| 🎵 QQ音乐听歌 | 模拟QQ音乐听歌1小时，获取加速天数 |
| 🎮 QQ手游加速 | 模拟手游登录，获取加速天数 |
| 💎 会员签到 | QQ会员每日签到领成长值 |
| 🌟 空间签到 | QQ空间每日签到 |
| 📨 推送通知 | 支持Server酱/PushPlus/Telegram推送结果 |

## 🚀 快速部署

### 1. Fork 或导入此仓库到你的 GitHub

### 2. 获取 QQ Cookie

1. 电脑浏览器打开 [m.qzone.com](https://m.qzone.com) 并登录
2. 按 `F12` 打开开发者工具 → **Application** → **Cookies**
3. 找到以下关键 Cookie 值：
   - `skey` — 主鉴权令牌（**必需**）
   - `p_skey` — Qzone 鉴权令牌（空间签到需要）
   - `uin` — QQ 号标识
   - 其他 cookie 也可以一起复制

**推荐方式**：在开发者工具 → **Network** 中随便点击一个请求，找到 Request Headers 中的 `Cookie:` 字段，**完整复制整个 Cookie 字符串**。

> ⚠️ Cookie 中必须包含 `skey`，否则脚本无法运行。如果需要 QQ 空间签到，还需包含 `p_skey`。

### 3. 配置 GitHub Secrets

在你的 GitHub 仓库 → **Settings** → **Secrets and variables** → **Actions** 中添加：

#### 必需配置

| Secret 名 | 说明 |
|-----------|------|
| `QQ_COOKIE` | QQ 登录的完整 Cookie 字符串（从浏览器复制） |

#### 可选配置

| Secret 名 | 默认值 | 说明 |
|-----------|--------|------|
| `ENABLE_QQ_MUSIC` | true | 是否启用QQ音乐听歌加速 |
| `ENABLE_QQ_GAME` | true | 是否启用QQ手游加速 |
| `PUSH_KEY` | 空 | 推送通知密钥 |
| `PUSH_TYPE` | 空 | 推送类型：`serverchan` / `pushplus` / `telegram` |

### 4. 手动测试

1. 进入仓库 → **Actions** → **QQ Daily Task**
2. 点击 **Run workflow** → **Run workflow**
3. 查看运行日志确认是否成功

### 5. 自动执行

配置完成后，每天北京时间 **07:00** 自动执行。如需修改时间，编辑 `.github/workflows/qq.yml` 中的 cron 表达式：

```yaml
schedule:
  - cron: '0 23 * * *'  # UTC 23:00 = 北京时间 07:00
```

常用时间对照：

| 北京时间 | cron 表达式 |
|---------|------------|
| 06:00 | `0 22 * * *` |
| 07:00 | `0 23 * * *` |
| 08:00 | `0 0 * * *` |
| 12:00 | `0 4 * * *` |

## 📨 推送通知配置

### Server酱（推荐）

1. 注册 [sct.ftqq.com](https://sct.ftqq.com/)
2. 获取 SendKey
3. 设置 Secrets：`PUSH_TYPE=serverchan`，`PUSH_KEY=你的SendKey`

### PushPlus

1. 关注微信公众号「PushPlus推送」
2. 获取 Token
3. 设置 Secrets：`PUSH_TYPE=pushplus`，`PUSH_KEY=你的Token`

### Telegram

1. 创建 Bot 获取 Token
2. 获取 Chat ID
3. 设置 Secrets：`PUSH_TYPE=telegram`，`PUSH_KEY=BotToken|ChatId`

## ⚠️ 注意事项

1. **Cookie 有效期**：QQ Cookie 中 skey 的有效期约 1-2 天，过期后需重新获取并更新 Secret。这是目前最大的限制。
2. **风控风险**：腾讯可能对频繁的 API 请求进行风控，建议不要同时在多个地方登录同一账号。
3. **GitHub Actions 限制**：免费账户每月 2000 分钟，每次执行约 1-2 分钟。
4. **隐私安全**：Cookie 属于敏感信息，请勿泄露，只存储在 GitHub Secrets 中。
5. **执行延迟**：GitHub Actions 定时任务可能有几分钟延迟，属正常现象。
6. **仓库活跃**：GitHub 会暂停不活跃仓库的定时任务，请保持仓库活跃（定期有 commit 或手动执行 workflow）。

## 🔧 故障排查

| 问题 | 解决方案 |
|------|---------|
| Cookie 无效 | 重新获取 Cookie，确保包含 skey |
| 空间签到跳过 | Cookie 中需要包含 p_skey |
| 等级信息获取失败 | 检查 skey 是否过期 |
| Actions 未执行 | 检查仓库是否活跃，GitHub 可能暂停不活跃仓库的定时任务 |
| 加速任务未生效 | 腾讯可能更新了接口，需等待脚本更新 |

## 🔑 关于 Cookie 获取的详细说明

由于 QQ 现在已不再支持密码直接登录，获取 Cookie 的方式如下：

### 方式一：浏览器 F12（推荐）

1. 使用 Chrome/Edge 浏览器打开 https://m.qzone.com
2. 扫码登录 QQ
3. 按 F12 打开开发者工具
4. 切换到 **Network（网络）** 标签
5. 刷新页面，点击任意请求
6. 在 Request Headers 中找到 `Cookie:` 字段，完整复制

### 方式二：浏览器扩展

1. 安装 Cookie 管理扩展（如 EditThisCookie、Cookie-Editor）
2. 登录 m.qzone.com
3. 使用扩展导出当前站点的 Cookie
4. 将导出的 Cookie 字符串保存到 GitHub Secrets

## 📄 License

MIT
