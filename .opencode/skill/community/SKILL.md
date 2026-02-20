---
name: community
description: 处理 GitHub PR 和 Issues 的工作流。用于审查 PR、回复 Issue、修复文档问题、感谢贡献者。触发场景：用户说"看下 PR"、"处理 issue"、"合并这个 PR"等。
---

# 社区贡献处理工作流

本 Skill 用于规范化处理 GitHub 上的 Pull Requests 和 Issues。

## 核心原则

1. **语言一致**：回复使用与贡献者相同的语言（中文 issue 用中文回复，英文 PR 用英文回复）
2. **及时感谢**：每个合并的 PR、关闭的 Issue 都要感谢贡献者
3. **验证后行动**：修改文档/代码前，先验证问题是否真实存在
4. **完整闭环**：修复 → 推送 → 感谢 → 关闭，一步都不能少

---

## PR 处理流程

### 1. 查看 PR

```bash
gh pr list --repo <owner/repo>
gh pr view <number> --repo <owner/repo>
```

或通过网页查看：`https://github.com/<owner/repo>/pull/<number>`

### 2. 分析 PR

必须检查：

| 检查项 | 说明 |
|--------|------|
| 变更内容 | 改了哪些文件？改动范围是否合理？ |
| 问题真实性 | PR 描述的问题是否存在？ |
| 修复方案 | 修复方式是否正确？有没有副作用？ |
| 截图/证据 | PR 是否提供了前后对比？ |

**输出格式**：

```markdown
## PR #X 分析报告

### 问题诊断
- 文件：xxx
- 问题：xxx

### 修复方案
- 改动：xxx

### 评估结论
| 维度 | 评估 |
|------|------|
| 问题是否真实存在 | ✅/❌ |
| 修复方案是否正确 | ✅/❌ |
| 副作用风险 | 低/中/高 |

### 建议：可以合并 / 需要修改 / 不予合并
```

### 3. 合并 PR

如果决定合并：

```bash
# 方法一：fetch + merge（推荐，可以自定义 commit message）
git fetch origin pull/<number>/head:pr-<number>
git merge pr-<number> --no-ff -m "Merge PR #<number>: <简短描述>"
git push origin main
git branch -d pr-<number>

# 方法二：gh merge
gh pr merge <number> --repo <owner/repo> --merge
```

### 4. 感谢贡献者

```bash
gh pr comment <number> --repo <owner/repo> --body "<感谢内容>"
```

**中文模板**：
```
感谢 @<username> 的贡献！🎉

<简要说明为什么这个 PR 有价值>。

已合并。
```

**英文模板**：
```
Thanks @<username> for the contribution! 🎉

<Brief explanation of why this PR is valuable>.

Merged.
```

---

## Issue 处理流程

### 1. 查看 Issue

```bash
gh issue view <number> --repo <owner/repo>
```

或通过网页查看：`https://github.com/<owner/repo>/issues/<number>`

### 2. 分析 Issue

必须检查：

| 检查项 | 说明 |
|--------|------|
| 问题类型 | Bug 报告 / 文档错误 / 功能请求 / 使用疑问 |
| 可复现性 | 问题是否可以验证？ |
| 影响范围 | 影响多少用户？严重程度？ |
| 修复难度 | 是否需要查看源码？ |

**如果涉及技术细节，需要查看源码验证**：
```bash
grep -rn "关键词" <源码目录>
cat <源码文件>
```

### 3. 修复问题

根据问题类型采取行动：

| 问题类型 | 行动 |
|----------|------|
| 文档错误 | 修改文档 |
| 代码 Bug | 修复代码 |
| 功能请求 | 评估后决定是否实现 |
| 使用疑问 | 回复解答，必要时更新 FAQ |

**修改后提交**：
```bash
git add <文件>
git commit -m "fix: <问题描述>

修复 #<number>：<详细说明>"
git push origin main
```

### 4. 感谢并关闭

```bash
# 先回复感谢
gh issue comment <number> --repo <owner/repo> --body "<感谢内容>"

# 再关闭（如果问题已解决）
gh issue close <number> --repo <owner/repo> --comment "已修复并合并。"
```

**中文模板**：
```
感谢 @<username> 的反馈！🎉

<验证结果或修复说明>。

已在 <commit-hash> 修复。

再次感谢帮助改进项目！
```

**英文模板**：
```
Thanks @<username> for the feedback! 🎉

<Verification result or fix explanation>.

Fixed in <commit-hash>.

Thanks again for helping improve the project!
```

---

## 快速参考

### 常用命令

```bash
# PR 相关
gh pr list --repo <owner/repo>
gh pr view <number> --repo <owner/repo>
gh pr comment <number> --repo <owner/repo> --body "..."
gh pr merge <number> --repo <owner/repo> --merge

# Issue 相关
gh issue list --repo <owner/repo>
gh issue view <number> --repo <owner/repo>
gh issue comment <number> --repo <owner/repo> --body "..."
gh issue close <number> --repo <owner/repo>
```

### 检查清单

**PR 合并前**：
- [ ] 问题真实存在
- [ ] 修复方案正确
- [ ] 无明显副作用
- [ ] 已准备感谢语

**Issue 关闭前**：
- [ ] 问题已理解
- [ ] 已验证/已修复
- [ ] 已推送代码
- [ ] 已回复感谢
