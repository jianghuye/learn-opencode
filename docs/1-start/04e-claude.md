---
title: "连接 Claude（Anthropic）：5 分钟接入 Claude 3.5/3.7 | OpenCode 教程"
subtitle: "连接 Claude（Anthropic）"
course: OpenCode 中文实战课
stage: 第一阶段
lesson: "1.4e"
duration: 15 分钟
practice: 5 分钟
level: 新手
description: 学习配置 Anthropic Claude 提供商，获取 API Key，了解 Claude 3.5/3.7 模型特点，完成首次对话。
tags:
  - 模型
  - Claude
  - Anthropic
  - API Key
prerequisite:
  - 1.2 安装
  - 1.3 网络配置
---

# 连接 Claude（Anthropic）

Claude 是 Anthropic 的大模型，在复杂代码任务上表现出色，拥有 200K tokens 的长上下文窗口。

在国内网络环境下，通常需要先完成 [1.3 网络配置](./03-network)，否则可能出现连接超时。

---

## 学完你能做什么

- 获取 Anthropic API Key 并配置到 OpenCode
- 了解 Claude 3.5 Sonnet、Claude 3.7 Sonnet 等模型的特点
- 使用 Claude 模型完成首次对话

## 你现在的困境

你可能已经：

- 听说过 Claude 的代码生成能力很强
- 有 Anthropic 的 API Key 或 Claude Max 订阅
- 想在 OpenCode 中使用 Claude 模型

但不知道怎么配置，或者不知道选哪个模型。

## 什么时候用这一招

- 已有 Anthropic API Key 或 Claude Max 订阅
- 需要使用 Claude 的长上下文能力（200K tokens）
- 对代码质量和推理能力有较高要求

---

## 🎒 开始前的准备

::: warning 前置条件
- **Anthropic 账号**：需要在 [console.anthropic.com](https://console.anthropic.com) 注册
- **API Key 或 Claude Max 订阅**：两者选其一
- **网络环境**：需要访问 Anthropic API（可能需要科学上网）
:::

---

## 核心思路

配置 Anthropic Claude 提供商的流程：

```
获取 API Key → 配置到 OpenCode → 验证连接
```

---

## 跟我做

### 第 1 步：获取 Anthropic API Key

访问 Anthropic 控制台获取 API Key：

1. 打开 [console.anthropic.com](https://console.anthropic.com)
2. 登录或注册账号
3. 点击 **API Keys** → **Create Key**
4. 复制生成的 API Key（格式：`sk-ant-...`）

::: warning 安全提示
- API Key 只显示一次，请立即保存
- 不要将 API Key 提交到代码仓库
- 定期轮换 API Key
:::

**你应该看到**：类似 `sk-ant-api03-xxxx-xxxx-xxxx` 的字符串

### 第 2 步：配置到 OpenCode

OpenCode 提供两种配置方式，选择适合你的：

#### 方式 1：使用环境变量（推荐）

将 API Key 设置为环境变量：

::: code-group

```bash [macOS/Linux（临时）]
export ANTHROPIC_API_KEY=sk-ant-api03-your-key-here
opencode
```

```bash [macOS/Linux（永久）]
# 添加到 ~/.bashrc 或 ~/.zshrc
echo 'export ANTHROPIC_API_KEY=sk-ant-api03-your-key-here' >> ~/.zshrc
source ~/.zshrc
opencode
```

```powershell [Windows（临时）]
$env:ANTHROPIC_API_KEY="sk-ant-api03-your-key-here"
opencode
```

```powershell [Windows（永久）]
# 添加到系统环境变量
[System.Environment]::SetEnvironmentVariable("ANTHROPIC_API_KEY", "sk-ant-api03-your-key-here", "User")
```

:::

#### 方式 2：使用 OpenCode 认证命令

运行认证命令，OpenCode 会将 API Key 保存到本地：

```bash
opencode auth login
```

然后：

1. 选择 `Anthropic`
2. 粘贴 API Key
3. 按回车完成

**你应该看到**：`Done` 或 `Login successful`

::: details 认证存储位置
OpenCode 将认证信息存储在：
- **所有平台**：`~/.local/share/opencode/auth.json`（遵循 XDG 规范）
:::

### 第 3 步：验证配置

验证 Anthropic 提供商是否配置成功：

```bash
opencode auth list
```

**你应该看到**：

```
Credentials ~/.local/share/opencode/auth.json
● Anthropic api

Environment
● Anthropic ANTHROPIC_API_KEY
```

这表示你已经成功配置了 Anthropic 提供商。

### 第 4 步：查看可用模型

查看 Anthropic 提供的所有模型：

```bash
opencode models
```

**你应该看到**类似：

```
anthropic/claude-3-5-haiku-20241022
anthropic/claude-3-5-sonnet-20241022
anthropic/claude-3-7-sonnet-20250219
anthropic/claude-opus-4-20250514
...
```

::: tip 查看完整模型信息
使用 `opencode models --verbose` 可查看模型名称、成本等详细元数据。
:::

### 第 5 步：进行首次对话

启动 OpenCode 并选择 Claude 模型：

```bash
cd /path/to/your/project
opencode
```

在 OpenCode 界面中，输入你的第一个问题：

```
帮我写一个简单的 HTTP 服务器
```

**你应该看到**：

1. AI 开始生成回复（使用 Claude 模型）
2. 可能会调用工具创建文件
3. 给出完整的代码和说明

---

## Claude 模型选择指南

Anthropic 提供多个 Claude 模型，根据你的需求选择：

| 模型 | 模型 ID | 上下文窗口 | 特点 | 适合场景 |
|------|---------|-----------|------|---------|
| Claude 3.7 Sonnet | `claude-3-7-sonnet-20250219` | 200K | 最新最强，推理能力出色 | 复杂任务、代码生成、重构 |
| Claude 3.5 Sonnet | `claude-3-5-sonnet-20241022` | 200K | 平衡性能和成本 | 日常开发、代码审查 |
| Claude Opus 4 | `claude-opus-4-20250514` | 200K | 旗舰级性能，最昂贵 | 超复杂任务、研究性工作 |
| Claude 3.5 Haiku | `claude-3-5-haiku-20241022` | 200K | 快速响应，成本低 | 简单任务、快速问答 |

::: tip 推荐选择
- **默认选择**：Claude 3.7 Sonnet（性能最强）
- **成本敏感**：Claude 3.5 Sonnet（性价比高）
- **快速响应**：Claude 3.5 Haiku（速度最快）
:::

---

## 检查点 ✅

完成以上步骤后，你应该能够：

- [ ] 在 `opencode auth list` 中看到 Anthropic 提供商
- [ ] 在 `opencode models` 中看到 Claude 系列模型
- [ ] 启动 OpenCode 并使用 Claude 模型对话
- [ ] 看到模型返回使用 Claude 的标识

---

## 踩坑提醒

### API Key 无效

**症状**：提示 `Authentication failed` 或 `Invalid API Key`

**原因**：
- API Key 格式错误（未以 `sk-ant-` 开头）
- API Key 过期或被撤销
- API Key 没有权限访问指定模型

**解决**：
1. 检查 API Key 格式是否正确
2. 在 Anthropic 控制台重新生成 API Key
3. 确认账号有足够余额或有效订阅

### 网络连接失败

**症状**：提示 `Network error` 或 `Connection timeout`

**原因**：
- 无法访问 Anthropic API（需要科学上网）
- 网络不稳定

**解决**：
1. 检查网络连接
2. 配置代理（如需要）：

```bash
export HTTP_PROXY=http://your-proxy:port
export HTTPS_PROXY=http://your-proxy:port
```

### 模型不可用

**症状**：提示 `Model not found` 或模型列表为空

**原因**：
- API Key 没有访问该模型的权限
- 模型已下线或更名

**解决**：
1. 检查 Anthropic 控制台的模型访问权限
2. 使用 `opencode models` 查看可用模型
3. 升级账号套餐以访问更多模型

---

## 本课小结

这节课你学会了：

1. **获取 API Key**：从 Anthropic 控制台获取 API Key
2. **配置认证**：使用环境变量或 `opencode auth login` 配置
3. **验证连接**：确认配置成功并查看可用模型
4. **模型选择**：了解不同 Claude 模型的特点和使用场景

**关键命令**：

| 命令 | 作用 |
|------|------|
| `opencode auth login` | 配置提供商认证 |
| `opencode auth list` | 查看已配置的提供商 |
| `opencode models` | 查看可用模型 |

**Claude 模型特点**：

- ✅ 长上下文窗口（200K tokens）
- ✅ 代码生成能力强
- ✅ 推理能力出色（Claude 3.7 Sonnet）

---

## 下一课预告

> 下一课我们学习 **[Claude Code 中转](./04f-claudecode-relay)**。
>
> 你会学到：
> - 什么是 Claude Code 中转服务
> - 如何配置中转端点
> - 中转与官方 API 的区别

---

## 附录：源码参考

<details>
<summary><strong>点击展开查看源码位置</strong></summary>

> 更新时间：2026-02-14

| 功能 | 文件路径 | 行号 |
|------|---------|------|
| Anthropic 提供商定义 | [`src/provider/provider.ts`](https://github.com/anomalyco/opencode/blob/main/packages/opencode/src/provider/provider.ts) | 93-103 |
| 环境变量检测 | [`src/provider/provider.ts`](https://github.com/anomalyco/opencode/blob/main/packages/opencode/src/provider/provider.ts) | 841-850 |
| 模型数据加载 | [`src/provider/models.ts`](https://github.com/anomalyco/opencode/blob/main/packages/opencode/src/provider/models.ts) | 87-104 |
| 认证管理 | [`src/auth/index.ts`](https://github.com/anomalyco/opencode/blob/main/packages/opencode/src/auth/index.ts) | 37-70 |

**关键配置**：

- **环境变量名**：`ANTHROPIC_API_KEY`
- **认证类型**：仅支持 API Key（不支持 OAuth）
- **自动加载**：`false`（需要手动配置 API Key）

</details>
