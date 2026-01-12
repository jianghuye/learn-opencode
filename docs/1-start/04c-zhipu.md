---
title: 连接智谱（Z.AI / GLM）
subtitle: 中文强、Coding Plan 可选
course: OpenCode 中文实战课
stage: 第一阶段
lesson: "1.4c"
duration: 10 分钟
practice: 5 分钟
level: 新手
description: 获取智谱 API Key，并在 OpenCode 中选择 GLM 模型完成第一次对话。
tags:
  - 模型
  - 智谱
  - GLM
  - API Key
prerequisite:
  - 1.2 安装
---

# 连接智谱（Z.AI / GLM）

> 预计时间：5-10 分钟

智谱（Z.AI）在中文理解、写作/总结方面体验很好。

**特别推荐**：强烈建议订阅官方的 **GLM Coding Pro** 套餐（中间那个选项）。

### 为什么推荐 GLM Coding Pro？

- **视觉理解**：可以读取图片，让 AI 帮你分析截图或设计稿
- **联网搜索**：支持实时联网，获取最新技术文档和解决方案
- **OpenCode 自带模型区别**：OpenCode 虽然自带免费的 GLM-4.7，但仅限基础对话。如果你是 GLM Coding Pro 订阅用户，连接后即可解锁上述高级能力。

如果你还没看过“API Key 是什么”，建议先回到 [1.4 总览](./04-connect) 把概念弄清楚。

---

## 学完你能做什么

- 获取智谱（Z.AI）API Key
- 在 OpenCode 里连接智谱提供商
- 选择一个 GLM 模型并完成第一次对话

---

## 🎒 开始前的准备

- [ ] 完成了 [1.2 安装](./02-install)，能运行 `opencode`
- [ ] 已能访问智谱控制台并拿到 API Key

---

## 跟我做

### 第 1 步：注册并获取 API Key

推荐通过以下邀请链接注册/登录，可以直接定位到 GLM Coding Pro 页面：

- **专属注册链接（优惠 10%）**：[https://www.bigmodel.cn/glm-coding?ic=PNSI6JU6FP](https://www.bigmodel.cn/glm-coding?ic=PNSI6JU6FP)

> **注意**：
> 1. 使用 learnopencode 教程站专属链接购买可享受 **10% 优惠**。
> 2. 请选择网页上**中间的套餐（GLM Coding Pro）**进行购买。只有这个套餐才包含视觉理解和联网搜索功能，能最大化 OpenCode 的能力。
> 3. **丰俭由人**：如果是轻度使用，**最低的套餐其实就够用了**。不用非得买贵的，适合自己最重要。

拿到 Key 后先保存好（不要发群、不要提交到 GitHub）。

---

### 第 2 步：在 OpenCode 中连接智谱

启动 OpenCode：

```bash
opencode
```

输入：

```
/connect
```

在提供商列表里搜索并选择：
- `Z.AI`（按量计费）
- `Z.AI Coding Plan`（如果你订阅了官方 Coding Plan）

然后粘贴你的 API Key。

---

### 第 3 步：选择 GLM 模型

输入：

```
/models
```

选择你想用的模型（例如 `glm-4.7`）。

---

### 第 4 步：发送第一句话（验证成功）

```
你好，请介绍一下你自己
```

能收到回复就说明连接成功。

---

## 检查点 ✅

- [ ] `/models` 里能看到并选择 GLM 模型（例如 `glm-4.7`）
- [ ] 发送消息能收到 AI 回复

---

## 踩坑提醒

| 现象 | 原因 | 解决 |
|-----|------|------|
| 没有 `Z.AI Coding Plan` 选项 | OpenCode 版本太旧 | 升级 OpenCode：`opencode upgrade` |
| `API key invalid` | Key 格式错误 | 确认是完整的 Key（通常类似 `xxx.xxx`） |

---

## 下一步

- 回到 [1.4 总览](./04-connect) 选择下一条路线，或进入 [2.1 界面与基础操作](../2-daily/01-interface)