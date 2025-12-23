# 企业微信快速部署指南 / Enterprise WeChat Quick Start Guide

<div align="center">

**[中文](#中文指南)** | **[English](#english-guide)**

</div>

---

## 中文指南

### 📱 30秒快速部署到企业微信

本指南将帮助你在 **30秒** 内完成 TrendRadar 到企业微信的部署配置。

#### 第一步：获取企业微信机器人 Webhook（15秒）

**手机端操作：**

1. 打开企业微信 App → 进入目标内部群聊
2. 点击右上角 **"..."** 按钮 → 选择 **"消息推送"**
3. 点击 **"添加"** → 名称输入 **"TrendRadar"**
4. **复制 Webhook 地址**（长按复制），点击保存

**PC 端操作：**

1. 打开企业微信 PC 客户端 → 进入目标群聊
2. 点击群设置图标（⚙️）→ 找到 **"群机器人"**
3. 点击 **"添加机器人"** → 名称输入 **"TrendRadar"**
4. **复制 Webhook 地址**，点击完成

> 💡 **Webhook 地址示例：**  
> `https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`

#### 第二步：Fork 本项目（5秒）

1. 点击本仓库页面右上角的绿色 **[Use this template]** 按钮
2. 选择 **"Create a new repository"**
3. 点击创建（无需修改任何设置）

#### 第三步：配置 GitHub Secret（10秒）

1. 在你 Fork 后的仓库中，进入：  
   **`Settings`** → **`Secrets and variables`** → **`Actions`** → **`New repository secret`**

2. 添加以下配置：
   - **Name（名称）**：`WEWORK_WEBHOOK_URL`（直接复制粘贴，不要手打）
   - **Secret（值）**：粘贴你在第一步复制的 Webhook 地址

3. 点击 **"Add secret"** 保存

#### ✅ 完成！

配置完成后，项目会自动每小时运行一次，将热点新闻推送到你的企业微信群。

---

### 💬 推送到个人微信（无需企业微信 App）

> 💡 **想直接在个人微信接收通知？** 无需安装企业微信 App！

通过企业微信的应用消息功能，可以将通知直接推送到个人微信。

#### 操作步骤：

**第一步：添加额外的 GitHub Secret**

在你的仓库中，按照前面的方式添加第二个 Secret：

1. 进入 **`Settings`** → **`Secrets and variables`** → **`Actions`** → **`New repository secret`**
2. 添加配置：
   - **Name（名称）**：`WEWORK_MSG_TYPE`
   - **Secret（值）**：`text`
3. 保存

**第二步：在企业微信中关联个人微信**

1. 手机打开企业微信 App
2. 点击 **"我"** → **"设置"**
3. 找到 **"关联微信"** 或 **"微信插件"**
4. 按提示完成关联
5. 开启 **"接收消息"** 功能

**第三步：配置完成**

现在你会在个人微信的"企业微信"插件中收到通知，无需安装企业微信 App。

> ⚠️ **注意事项：**
> - 个人微信推送为**纯文本格式**（无 Markdown 样式）
> - 首次可能需要在企业微信中接收一条消息后才会同步到个人微信
> - 确保在个人微信的企业微信插件中已允许接收该企业的消息

#### 效果对比：

| 推送方式 | 消息格式 | 需要 App | 接收位置 |
|---------|---------|---------|---------|
| 企业微信群机器人 | Markdown（富文本） | ✅ 需要企业微信 | 企业微信群聊 |
| 个人微信推送 | 纯文本 | ❌ 不需要 | 个人微信→企业微信插件 |

---

### 📋 更多进阶配置（可选）

#### 1. 修改推送频率

默认每小时第33分钟运行。如需修改：

1. 进入你的仓库：`.github/workflows/crawler.yml`
2. 找到第 38 行：`- cron: "33 * * * *"`
3. 修改第一个数字（0-59），例如：
   - `"0 * * * *"` → 每小时整点运行
   - `"15 * * * *"` → 每小时第15分钟运行
   - `"*/30 * * * *"` → 每30分钟运行一次

#### 2. 配置关键词过滤

默认推送所有热点，如需只接收特定关键词新闻：

1. 进入你的仓库：`config/frequency_words.txt`
2. 每行添加一个关键词，例如：
   ```
   人工智能
   比亚迪
   特斯拉
   ```
3. 保存文件

**关键词语法：**
- 普通词：`人工智能` → 包含该词的新闻
- 必须词：`+比亚迪` → 标题必须包含
- 过滤词：`!广告` → 排除包含该词的新闻
- 数量限制：`人工智能@5` → 最多显示5条

#### 3. 修改推送模式

编辑 `config/config.yaml` 第 76 行：

```yaml
report:
  mode: "current"  # 可选: "daily" | "incremental" | "current"
```

- **`daily`**（当日汇总）：每次推送当天所有匹配新闻
- **`current`**（当前榜单）：推送当前榜单匹配新闻（默认）
- **`incremental`**（增量模式）：只推送新增内容，零重复

---

### 🔧 签到续期机制

本项目采用 **7天签到续期** 机制：

- 每个周期为 7 天，到期后自动暂停
- 续期方法：在 Actions 页面运行 **"Check In"** workflow
- 操作路径：**`Actions`** → **`Check In`** → **`Run workflow`**

> 💡 **为什么需要签到？**  
> GitHub Actions 是宝贵的公共资源。签到机制确保资源分配给真正需要的用户，避免无效运行。

如需长期稳定运行，建议使用 [Docker 部署](../README.md#6-docker-部署)。

---

### ❓ 常见问题

<details>
<summary><b>Q1: 没有收到推送消息？</b></summary>

**检查步骤：**

1. 确认 Webhook 地址正确（在企业微信群里应该能看到 "TrendRadar" 机器人）
2. 检查 GitHub Actions 是否运行成功：
   - 进入你的仓库 → **Actions** 标签
   - 查看最近的 "Get Hot News" workflow 运行状态
3. 检查是否在 7 天签到周期内
4. 查看 workflow 日志中是否有错误信息

</details>

<details>
<summary><b>Q2: 推送内容太多/太少？</b></summary>

**解决方案：**

- **太多**：在 `config/frequency_words.txt` 中添加关键词过滤
- **太少**：检查关键词配置是否过于严格，或留空接收所有新闻
- 调整推送模式为 `incremental` 避免重复内容

</details>

<details>
<summary><b>Q3: 如何停止推送？</b></summary>

**方法一**：禁用 workflow
- 进入 **Actions** → 点击 **"Get Hot News"** → 右上角 **"..."** → **"Disable workflow"**

**方法二**：删除 GitHub Secret
- 进入 **Settings** → **Secrets** → 删除 `WEWORK_WEBHOOK_URL`

</details>

<details>
<summary><b>Q4: 支持多个企业微信群吗？</b></summary>

**支持！** 使用分号分隔多个 Webhook：

```
https://qyapi.weixin.qq.com/xxx/key1;https://qyapi.weixin.qq.com/xxx/key2
```

最多支持 3 个群（可在 `config/config.yaml` 中调整 `max_accounts_per_channel`）

</details>

<details>
<summary><b>Q5: 个人微信推送和企业微信群推送有什么区别？</b></summary>

**主要区别：**

| 特性 | 企业微信群机器人 | 个人微信推送 |
|------|----------------|------------|
| 需要安装 App | ✅ 需要企业微信 | ❌ 不需要（在个人微信查看） |
| 消息格式 | Markdown 富文本 | 纯文本 |
| 接收位置 | 企业微信群聊 | 个人微信的企业微信插件 |
| 配置复杂度 | 简单（1个Secret） | 中等（2个Secret+关联） |

**推荐选择：**
- 如果有企业微信 App → 使用群机器人（消息更美观）
- 不想装企业微信 App → 使用个人微信推送

</details>

---

### 📞 获取帮助

- **GitHub Issues**：[提交问题](https://github.com/sansan0/TrendRadar/issues)
- **官方文档**：[完整 README](../README.md)
- **微信公众号**：硅基茶水间（见主 README）

---

## English Guide

### 📱 Deploy to Enterprise WeChat in 30 Seconds

This guide will help you deploy TrendRadar to Enterprise WeChat (WeCom) in just **30 seconds**.

#### Step 1: Get WeWork Bot Webhook (15s)

**Mobile:**

1. Open WeWork App → Enter target internal group chat
2. Tap **"..."** button (top right) → Select **"Message Push"**
3. Tap **"Add"** → Name: **"TrendRadar"**
4. **Copy Webhook URL** (long press to copy), tap Save

**Desktop:**

1. Open WeWork PC client → Enter target group chat
2. Click group settings icon (⚙️) → Find **"Group Bot"**
3. Click **"Add Bot"** → Name: **"TrendRadar"**
4. **Copy Webhook URL**, click Done

> 💡 **Webhook URL Example:**  
> `https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`

#### Step 2: Fork This Repository (5s)

1. Click the green **[Use this template]** button at the top right of this repository page
2. Select **"Create a new repository"**
3. Click create (no need to modify any settings)

#### Step 3: Configure GitHub Secret (10s)

1. In your forked repository, go to:  
   **`Settings`** → **`Secrets and variables`** → **`Actions`** → **`New repository secret`**

2. Add the following configuration:
   - **Name**: `WEWORK_WEBHOOK_URL` (copy & paste, don't type manually)
   - **Secret**: Paste the Webhook URL you copied in Step 1

3. Click **"Add secret"** to save

#### ✅ Done!

After configuration, the project will automatically run every hour and push trending news to your WeWork group.

---

### 💬 Push to Personal WeChat (No WeWork App Required)

> 💡 **Want to receive notifications directly on personal WeChat?** No need to install the WeWork App!

Using WeWork's application message feature, you can push notifications directly to personal WeChat.

#### Steps:

**Step 1: Add Additional GitHub Secret**

In your repository, add a second Secret following the same process:

1. Go to **`Settings`** → **`Secrets and variables`** → **`Actions`** → **`New repository secret`**
2. Add configuration:
   - **Name**: `WEWORK_MSG_TYPE`
   - **Secret**: `text`
3. Save

**Step 2: Link Personal WeChat in WeWork**

1. Open WeWork App on mobile
2. Tap **"Me"** → **"Settings"**
3. Find **"Link WeChat"** or **"WeChat Plugin"**
4. Follow prompts to complete linking
5. Enable **"Receive Messages"** feature

**Step 3: Configuration Complete**

Now you'll receive notifications in personal WeChat's "WeWork" plugin, without needing the WeWork App installed.

> ⚠️ **Notes:**
> - Personal WeChat push uses **plain text format** (no Markdown styling)
> - You may need to receive one message in WeWork first before it syncs to personal WeChat
> - Make sure the WeWork plugin in personal WeChat allows receiving messages from your organization

#### Comparison:

| Push Method | Message Format | Requires App | Receive Location |
|------------|----------------|--------------|------------------|
| WeWork Group Bot | Markdown (rich text) | ✅ Requires WeWork | WeWork group chat |
| Personal WeChat | Plain text | ❌ Not required | Personal WeChat → WeWork plugin |

---

### 📋 More Advanced Configuration (Optional)

#### 1. Change Push Frequency

Default runs at minute 33 of every hour. To modify:

1. Go to your repository: `.github/workflows/crawler.yml`
2. Find line 38: `- cron: "33 * * * *"`
3. Change the first number (0-59), for example:
   - `"0 * * * *"` → Run at the top of every hour
   - `"15 * * * *"` → Run at minute 15 of every hour
   - `"*/30 * * * *"` → Run every 30 minutes

#### 2. Configure Keyword Filtering

Default pushes all trending news. To receive only specific keywords:

1. Go to your repository: `config/frequency_words.txt`
2. Add one keyword per line, for example:
   ```
   AI
   Tesla
   Bitcoin
   ```
3. Save the file

**Keyword Syntax:**
- Normal word: `AI` → News containing this word
- Required word: `+Tesla` → Title must contain
- Filter word: `!ads` → Exclude news containing this word
- Count limit: `AI@5` → Show maximum 5 items

#### 3. Change Push Mode

Edit line 76 in `config/config.yaml`:

```yaml
report:
  mode: "current"  # Options: "daily" | "incremental" | "current"
```

- **`daily`** (Daily Summary): Push all matched news of the day
- **`current`** (Current Rankings): Push current ranking matches (default)
- **`incremental`** (Incremental): Push only new content, zero duplicates

---

### 🔧 Check-in Renewal Mechanism

This project uses a **7-day check-in renewal** mechanism:

- Each cycle is 7 days, automatically pauses when expired
- Renewal method: Run **"Check In"** workflow in Actions page
- Path: **`Actions`** → **`Check In`** → **`Run workflow`**

> 💡 **Why check-in?**  
> GitHub Actions is a valuable shared resource. Check-in ensures resources go to users who truly need them, avoiding wasted runs.

For long-term stable operation, we recommend [Docker deployment](../README.md#6-docker-deployment).

---

### ❓ FAQ

<details>
<summary><b>Q1: Not receiving push messages?</b></summary>

**Check Steps:**

1. Verify Webhook URL is correct (you should see "TrendRadar" bot in WeWork group)
2. Check if GitHub Actions ran successfully:
   - Go to your repository → **Actions** tab
   - Check recent "Get Hot News" workflow run status
3. Verify within the 7-day check-in cycle
4. Review workflow logs for error messages

</details>

<details>
<summary><b>Q2: Too much/too little content pushed?</b></summary>

**Solutions:**

- **Too much**: Add keyword filtering in `config/frequency_words.txt`
- **Too little**: Check if keyword config is too strict, or leave empty to receive all news
- Change push mode to `incremental` to avoid duplicate content

</details>

<details>
<summary><b>Q3: How to stop push?</b></summary>

**Method 1**: Disable workflow
- Go to **Actions** → Click **"Get Hot News"** → Top right **"..."** → **"Disable workflow"**

**Method 2**: Delete GitHub Secret
- Go to **Settings** → **Secrets** → Delete `WEWORK_WEBHOOK_URL`

</details>

<details>
<summary><b>Q4: Support multiple WeWork groups?</b></summary>

**Yes!** Use semicolons to separate multiple Webhooks:

```
https://qyapi.weixin.qq.com/xxx/key1;https://qyapi.weixin.qq.com/xxx/key2
```

Maximum 3 groups supported (adjustable via `max_accounts_per_channel` in `config/config.yaml`)

</details>

<details>
<summary><b>Q5: What's the difference between Personal WeChat push and WeWork group push?</b></summary>

**Main Differences:**

| Feature | WeWork Group Bot | Personal WeChat Push |
|---------|------------------|---------------------|
| Requires App | ✅ Needs WeWork | ❌ Not required (view in personal WeChat) |
| Message Format | Markdown rich text | Plain text |
| Receive Location | WeWork group chat | Personal WeChat's WeWork plugin |
| Configuration | Simple (1 Secret) | Medium (2 Secrets + linking) |

**Recommendation:**
- Have WeWork App → Use group bot (better formatting)
- Don't want WeWork App → Use personal WeChat push

</details>

---

### 📞 Get Help

- **GitHub Issues**: [Submit Issue](https://github.com/sansan0/TrendRadar/issues)
- **Official Documentation**: [Full README](../README.md)
- **WeChat Official Account**: Silicon Tea Room (see main README)

---

<div align="center">

**Made with ❤️ for Enterprise WeChat Users**

[🔝 Back to Top](#企业微信快速部署指南--enterprise-wechat-quick-start-guide)

</div>
