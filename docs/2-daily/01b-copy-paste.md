---
title: 2.1b 如何复制内容
subtitle: 告别 Ctrl+C 的困惑
course: OpenCode 中文实战课
stage: 第二阶段
lesson: "2.1b"
duration: 5 分钟
practice: 5 分钟
level: 新手
description: 为什么 Ctrl+C 不能复制？学会 OpenCode 高效复制的三种姿势。
tags:
  - 复制粘贴
  - 基础操作
  - Windows
  - Mac
prerequisite:
  - 2.1 界面与操作
---

# 2.1b 如何复制内容

> 💡 **新手第一问**："为什么我按 Ctrl+C 程序就退出了？！"

## 📝 课程笔记

本课核心知识点整理：

<img src="/images/2-daily/01b-copy-paste-notes.mini.jpeg" alt="复制操作学霸笔记" data-zoom-src="/images/2-daily/01b-copy-paste-notes.jpeg" />

---

## 学完你能做什么

- 理解为什么不能用 Ctrl+C 复制
- 掌握鼠标选中即复制的"黑科技"
- 学会一键复制 AI 的长回复
- 解决 Windows 终端选不中文字的问题

---

## 💥 为什么 Ctrl+C 不管用？

对于习惯了 Word 和浏览器的用户来说，这可能是最大的文化冲击。

在终端（OpenCode 运行的黑框框）的世界里，`Ctrl+C` 通常被用作中断信号，而不是复制快捷键。

- **在 Word 里**：`Ctrl+C` = 📷 **拍照**（复制内容）
- **在终端里**：`Ctrl+C` = 🛑 **中断**

因此，在 OpenCode 里按 `Ctrl+C` 并不能复制选中的内容。

---

## 🚀 方法一：鼠标流（最直觉）

这是 OpenCode 专门为现代用户设计的"隐藏技能"，**Win 和 Mac 通用**。

### 操作步骤

1. **按下鼠标左键**，拖动选择你想复制的文字。
2. **松开鼠标左键**。
3. **完成！** 🎉

是的，你没看错。你不需要按右键，也不需要按任何键。在你**松开鼠标的一瞬间**，OpenCode 会自动把选中的内容复制到剪贴板，并提示 `Copied to clipboard`。

---

## ⌨️ 方法二：键盘流（最快）

当你只想要 **AI 刚才说的那段话**（比如一大段代码），用鼠标拖太累了。

### 操作步骤

1. 按下 **Leader 键**（默认是 <kbd>Ctrl</kbd>+<kbd>X</kbd>）。
2. 松开所有键。
3. 按下 <kbd>Y</kbd>。

**记忆口诀**：`Y` = **Y**ank（这是程序员圈子里"复制"的黑话）。

> 想要更多快捷键？请查看 **[2.3 常用快捷键推荐](./03-shortcuts)**。

---

## 📜 方法三：命令流（最全）

当你想把**整个对话记录**（你说的话 + AI 说的话）都保存下来，发给朋友或者存到文档里。

### 操作步骤

1. 在输入框输入 `/copy`。
2. 按 <kbd>Enter</kbd> 回车。

整个对话历史就会被完整复制。

---

## 🛡️ 保底大招：强制系统复制

如果上面的"黑科技"不管用，或者你就是想用系统原生的方式复制。

| 系统 | 操作 |
|------|------|
| **Mac** | 按住 <kbd>Option (⌥)</kbd> + 鼠标拖选 → <kbd>Command</kbd>+<kbd>C</kbd> |
| **Windows** | 按住 <kbd>Shift</kbd> + 鼠标拖选 → <kbd>右键</kbd> 或 <kbd>Enter</kbd> |

**使用场景**：
- OpenCode 卡死没反应时
- 想要精确复制报错信息时
- 终端窗口被其他程序劫持时

---

## 跟我做

### 第 1 步：体验 Ctrl+C
1. 在输入框随便打几个字（不要发出去）。
2. 按 <kbd>Ctrl</kbd>+<kbd>C</kbd>。
3. 看到输入框被清空了吗？因为 Ctrl+C 被识别为了中断信号。

### 第 2 步：体验"自动复制"
1. 让 AI 随便说句话（比如输入 `hi`）。
2. 用鼠标选中 AI 回复的几个字。
3. **松开鼠标**。
4. 找个记事本，按 `Ctrl+V` 粘贴。
5. 成功了吗？

### 第 3 步：体验"强制复制"
1. 回到 OpenCode。
2. **Mac 用户**：按住 <kbd>Option</kbd>，**Windows 用户**：按住 <kbd>Shift</kbd>。
3. 用鼠标拖选一段文字。
4. 这种方式绕过了 OpenCode，直接使用了终端系统的选择功能。

---

## 🚑 故障排查：屏幕出现乱码怎么办？

**现象**：如果你是 Windows 用户，在按了 `Ctrl+C` 退出后，可能会发现动动鼠标屏幕上就会出现一堆类似 `[555;38;16M` 的乱码。

**原因**：
- OpenCode 启动时开启了"鼠标监听模式"。
- `Ctrl+C` 在 Windows 上是一种暴力强制退出的信号（SIGINT）。
- 导致 OpenCode 还没来得及告诉终端"关闭鼠标监听"，程序就被杀死了。
- 终端以为你还在程序里，拼命把鼠标坐标发给屏幕，就变成了乱码。

> 🐛 **已知问题**：截止到 OpenCode **v1.1.25** 版本，此问题在 Windows 上尚未完全修复。这属于底层技术限制，建议按照下方表格操作。

**解决方法**：
1. **急救**：直接关闭当前终端窗口，重新打开。
2. **预防（强烈推荐）**：不要用 `Ctrl+C` 退出程序！请使用 **Ctrl+D**。

| 退出方式 | 性质 | 结果 |
|---------|------|------|
| <kbd>Ctrl</kbd>+<kbd>C</kbd> | 🔪 **硬杀** (Force Kill) | 容易导致鼠标乱码，不推荐 |
| <kbd>Ctrl</kbd>+<kbd>D</kbd> | 👋 **告别** (Soft Exit) | 优雅退出，自动清理，**推荐** |

> **提示**：`Ctrl+D` 会触发 OpenCode 的清理逻辑，确保鼠标模式被正确关闭。

---

## 📚 延伸阅读

- 想要掌握更多快捷键？
  👉 **[2.3 常用快捷键推荐](./03-shortcuts)**

- 想要修改快捷键（比如把 Ctrl+C 改回来）？
  👉 **[5.6b 快捷键自定义](../5-advanced/06b-keybinds)**

---

## 下一课预告

> 下一课我们学习 **[管理对话](./02-sessions)**。
>
> 你会学到：
> - 如何新建、切换、重命名会话
> - 如何撤销不满意的回复
> - 如何导出聊天记录

---

## 附录：源码参考

<details>
<summary><strong>点击展开查看源码位置</strong></summary>

> 更新时间：2026-01-18

OpenCode 的"鼠标松开即复制"逻辑并非终端默认行为，而是写在代码里的高级特性：

| 功能 | 文件路径 | 关键逻辑 |
|-----|---------|----------|
| 鼠标复制逻辑 | [`src/cli/cmd/tui/app.tsx`](https://github.com/anomalyco/opencode/blob/dev/packages/opencode/src/cli/cmd/tui/app.tsx) | `onMouseUp` 事件监听 |
| 剪贴板工具 | [`src/cli/cmd/tui/util/clipboard.ts`](https://github.com/anomalyco/opencode/blob/dev/packages/opencode/src/cli/cmd/tui/util/clipboard.ts) | 跨平台剪贴板支持 |

**核心代码片段 (`app.tsx`)**：
```typescript
// 当鼠标松开时
onMouseUp={async () => {
  // 1. 获取选中的文本
  const text = renderer.getSelection()?.getSelectedText()
  if (text && text.length > 0) {
    // 2. 调用剪贴板 API 复制
    await Clipboard.copy(text)
    // 3. 提示用户
    toast.show({ message: "Copied to clipboard" })
    // 4. 清除选中状态
    renderer.clearSelection()
  }
}}
```
</details>
