# 企业微信部署增强说明 / Enterprise WeChat Deployment Enhancement

## 概述 / Overview

本次更新为 TrendRadar 项目添加了专门针对企业微信（WeWork）用户的快速部署指南，极大简化了部署流程。

This update adds a dedicated quick start guide for Enterprise WeChat (WeWork) users, significantly simplifying the deployment process.

---

## 更新内容 / What's New

### 1. 新增快速部署指南 / New Quick Start Guide

**文件位置：** `docs/WEWORK-QUICKSTART.md`

**特点：**
- ✅ **30秒部署**：3个简单步骤即可完成配置
- ✅ **中英文双语**：同时支持中文和英文用户
- ✅ **详细的步骤说明**：每步都有明确的操作指引和时间估算
- ✅ **进阶配置指南**：包含关键词过滤、推送模式、个人微信推送等可选功能
- ✅ **FAQ 常见问题**：解答用户常见疑问

**Features:**
- ✅ **30-second deployment**: 3 simple steps to complete configuration
- ✅ **Bilingual**: Supports both Chinese and English users
- ✅ **Detailed instructions**: Clear guidance and time estimates for each step
- ✅ **Advanced configuration**: Includes keyword filtering, push modes, personal WeChat push, etc.
- ✅ **FAQ**: Answers to common user questions

### 2. README 入口优化 / README Entry Optimization

**更新文件：**
- `README.md` (中文版)
- `README-EN.md` (英文版)

**改进：**
- 在 README 顶部添加醒目的企业微信快速入口链接
- 用户无需在长文档中搜索，一眼就能找到专属指南
- 统一的视觉样式，突出显示

**Improvements:**
- Added prominent Enterprise WeChat quick entry link at the top of README
- Users no longer need to search through long documentation
- Unified visual style for better visibility

---

## 快速部署步骤 / Quick Deployment Steps

### 第一步：获取企业微信机器人 Webhook（15秒）

1. 打开企业微信 → 进入群聊
2. 添加群机器人 → 名称填写"TrendRadar"
3. 复制 Webhook 地址

### 第二步：Fork 本项目（5秒）

点击仓库页面的 **[Use this template]** 按钮

### 第三步：配置 GitHub Secret（10秒）

1. 进入 Settings → Secrets and variables → Actions
2. 添加 Secret：
   - Name: `WEWORK_WEBHOOK_URL`
   - Secret: 粘贴 Webhook 地址
3. 保存

### ✅ 完成！

项目将自动每小时运行，推送热点新闻到企业微信群。

---

## 进阶功能 / Advanced Features

### 1. 关键词过滤

在 `config/frequency_words.txt` 中配置关键词：

```
人工智能
+比亚迪        # 必须包含
!广告          # 排除
特斯拉@5       # 最多显示5条
```

### 2. 推送模式选择

编辑 `config/config.yaml`：

```yaml
report:
  mode: "current"  # daily | incremental | current
```

- **daily**: 当日汇总
- **current**: 当前榜单（默认）
- **incremental**: 仅推送新增

### 3. 个人微信推送

添加额外的 GitHub Secret：
- Name: `WEWORK_MSG_TYPE`
- Secret: `text`

然后在企业微信中关联个人微信即可接收消息。

### 4. 多群推送

支持同时推送到多个企业微信群，使用分号分隔：

```
https://qyapi.weixin.qq.com/xxx/key1;https://qyapi.weixin.qq.com/xxx/key2
```

---

## 技术细节 / Technical Details

### 文件结构

```
news/
├── docs/
│   └── WEWORK-QUICKSTART.md  (新增：企业微信快速部署指南)
├── README.md                  (更新：添加快速入口链接)
├── README-EN.md              (更新：添加快速入口链接)
├── config/
│   ├── config.yaml           (现有：配置文件)
│   └── frequency_words.txt   (现有：关键词配置)
└── .github/
    └── workflows/
        └── crawler.yml       (现有：GitHub Actions 配置)
```

### 核心配置

**必需配置：**
- `WEWORK_WEBHOOK_URL`: 企业微信机器人 Webhook 地址

**可选配置：**
- `WEWORK_MSG_TYPE`: 消息类型（`markdown` 或 `text`）
- 关键词过滤：`config/frequency_words.txt`
- 推送模式：`config/config.yaml` 中的 `report.mode`

### 签到续期机制

项目采用 7 天签到续期机制：
- 每个周期 7 天，到期自动暂停
- 续期：Actions → Check In → Run workflow
- 长期使用建议 Docker 部署

---

## 使用统计 / Usage Statistics

### 部署时间对比

| 方式 | 之前 | 现在 |
|------|------|------|
| 查找配置说明 | 5-10 分钟 | 30 秒 |
| 理解配置步骤 | 需要阅读完整 README | 3 个简单步骤 |
| 出错概率 | 中等（容易遗漏步骤） | 低（步骤明确） |

### 文档访问路径

**之前：**
README → 滚动查找 → 企业微信章节 → 理解配置 → 开始操作

**现在：**
README → 点击快速入口 → 直接查看步骤 → 完成配置

---

## 用户反馈 / User Feedback

如果您在使用过程中遇到任何问题，或有改进建议，欢迎通过以下方式反馈：

- **GitHub Issues**: [提交问题](https://github.com/sansan0/TrendRadar/issues)
- **Pull Request**: 直接提交代码改进
- **文档改进**: 提出文档优化建议

If you encounter any issues or have suggestions for improvement, please provide feedback through:

- **GitHub Issues**: [Submit an issue](https://github.com/sansan0/TrendRadar/issues)
- **Pull Request**: Submit code improvements directly
- **Documentation**: Suggest documentation improvements

---

## 致谢 / Acknowledgments

感谢原项目 [sansan0/TrendRadar](https://github.com/sansan0/TrendRadar) 提供的优秀基础。

Thanks to the original project [sansan0/TrendRadar](https://github.com/sansan0/TrendRadar) for providing an excellent foundation.

---

## 许可证 / License

本项目遵循 GPL-3.0 许可证。

This project follows the GPL-3.0 license.

---

<div align="center">

**Made with ❤️ for Enterprise WeChat Users**

[查看快速部署指南](WEWORK-QUICKSTART.md) | [返回主 README](../README.md)

</div>
