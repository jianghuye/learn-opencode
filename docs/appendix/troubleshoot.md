---
title: "故障排除：快速定位和解决常见问题 | OpenCode 教程"
description: 快速定位和解决 OpenCode 使用中的常见问题。涵盖 API 错误、上下文溢出、权限拒绝、输出截断、重试机制等，附完整错误代码速查表和诊断清单。
---

# 故障排除

> 系统性地诊断和解决问题

## 📝 课程笔记

本课核心知识点整理：

<img src="/images/appendix/troubleshoot-notes.mini.jpeg"
     alt="故障排除学霸笔记"
     data-zoom-src="/images/appendix/troubleshoot-notes.jpeg" />

---

## 日志和存储位置

### 日志文件

| 平台 | 路径 |
|------|------|
| macOS/Linux | `~/.local/share/opencode/log/` |
| Windows | `%USERPROFILE%\.local\share\opencode\log\` |

日志文件以时间戳命名（如 `2025-01-09T123456.log`），保留最近 10 个。

```bash
# 查看最新日志
ls -lt ~/.local/share/opencode/log/ | head

# 开启调试日志
opencode --log-level DEBUG

# 打印日志到终端
opencode --print-logs
```

### 存储位置

| 数据类型 | 路径 |
|----------|------|
| 配置文件 | `~/.config/opencode/opencode.json` |
| 认证信息 | `~/.local/share/opencode/auth.json` |
| 日志 | `~/.local/share/opencode/log/` |
| 项目数据 | `~/.local/share/opencode/storage/` |
| 缓存 | `~/.cache/opencode/` |

---

## 启动问题

### 无法启动 / 命令找不到

**症状**：
```
zsh: command not found: opencode
```

**诊断步骤**：

```bash
# 1. 检查是否安装
which opencode
npm list -g opencode-ai

# 2. 检查 PATH
echo $PATH | tr ':' '\n' | grep -E "(npm|bin)"
```

**解决方案**：

```bash
# 重新安装
npm install -g opencode-ai

# 手动添加到 PATH
echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

---

### 启动崩溃 / 白屏

**症状**：启动后立即退出或显示空白屏幕

**诊断步骤**：

```bash
# 1. 查看错误日志
opencode --print-logs 2>&1 | head -50

# 2. 清除缓存重试
rm -rf ~/.cache/opencode
opencode

# 3. 重置配置
mv ~/.config/opencode/opencode.json ~/.config/opencode/opencode.json.bak
opencode
```

**常见原因**：
- 配置文件语法错误
- 终端不兼容
- 缓存损坏

---

### ProviderInitError

**症状**：
```
ProviderInitError: Failed to initialize provider
```

**诊断步骤**：

```bash
# 1. 验证提供商配置
opencode auth list

# 2. 清除存储重试
rm -rf ~/.local/share/opencode
```

**解决方案**：

1. 确认按照 [提供商指南](./providers) 正确设置
2. 重新使用 `/connect` 配置

---

## 认证问题
<AdInArticle />

### API Key 无效

**症状**：
```
AuthenticationError: Invalid API key
```

**诊断步骤**：

```bash
# 检查已配置的凭证
opencode auth list

# 检查环境变量
env | grep API_KEY
```

**解决方案**：

```bash
# 重新配置
opencode auth login
# 或在 TUI 中
/connect
```

---

### GitHub Copilot 认证失败

**症状**：
```
Error: 403 Forbidden
Please reauthenticate with the copilot provider to ensure
your credentials work properly with OpenCode.
```

**原因**：GitHub Copilot 的 OAuth Token 过期或失效。

**解决方案**：

```bash
# 重新认证 GitHub Copilot
opencode auth login
# 选择 github-copilot 提供商，按提示完成 OAuth 流程
```

如果使用 GitHub Enterprise：

```bash
# 在配置中指定 Enterprise URL
{
  "provider": {
    "github-copilot": {
      "options": {
        "enterpriseUrl": "https://github.your-company.com"
      }
    }
  }
}
```

---

### Token 配额超限

**症状**：
```
RateLimitError: quota exceeded
```

**解决方案**：
1. 等待配额重置
2. 升级账户计划
3. 切换到其他提供商/模型

```bash
# 切换模型
opencode -m zhipu/glm-4-flash
```

---

## 模型问题

### AI_APICallError

**症状**：
```
AI_APICallError: API call failed
```

**诊断步骤**：

```bash
# 1. 检查网络
curl -I https://api.anthropic.com

# 2. 清除提供商包缓存
rm -rf ~/.cache/opencode

# 3. 重启 opencode
```

**常见原因**：
- 网络问题
- API Key 无效
- 提供商包版本过旧

---

### ProviderModelNotFoundError

**症状**：
```
ProviderModelNotFoundError: Model not found
```

**原因**：模型引用格式错误

**正确格式**：`<providerId>/<modelId>`

**示例**：
- `anthropic/claude-sonnet-4-20250514`
- `openai/gpt-4o`
- `deepseek/deepseek-chat`

**解决方案**：

```bash
# 查看可用模型
opencode models
```

---

### 模型响应空白

**症状**：AI 没有回复或回复为空

**可能原因**：
1. 上下文超长被截断
2. 内容过滤触发
3. 模型负载过高

**解决方案**：

```
# 压缩上下文
/compact

# 新建会话
/new

# 切换模型
/models
```

---

## API 重试机制

遇到 `Too Many Requests`、`Provider is overloaded` 这类临时错误时，OpenCode 会自动重试，你不需要手动操作。

### 重试策略

| 参数 | 值 | 说明 |
|------|-----|------|
| 初始延迟 | 2 秒 | 首次重试的等待时间 |
| 退避因子 | 2x | 每次重试延迟翻倍 |
| 最大延迟（无响应头） | 30 秒 | 提供商没返回 `Retry-After` 头时的上限 |
| 最大延迟（有响应头） | 按响应头 | 提供商返回了 `Retry-After` 头时，按其指定的等待时间 |

::: details 重试延迟计算（提供商未返回响应头时）
```
首次重试: 2 秒
第二次:   4 秒
第三次:   8 秒
第四次:  16 秒
第五次:  30 秒（触及上限，不再增长）
```

如果提供商返回了 `Retry-After` 或 `Retry-After-Ms` 响应头，OpenCode 会优先使用提供商指定的等待时间，不受 30 秒上限限制。
:::

### 哪些错误会自动重试

| 错误 | 是否重试 | 说明 |
|------|---------|------|
| `Too Many Requests` (429) | ✅ 自动重试 | 速率限制 |
| `Provider is overloaded` (503) | ✅ 自动重试 | 服务过载 |
| `Rate Limited` | ✅ 自动重试 | 频率限制 |
| 上下文溢出 | ❌ 不重试 | 需要压缩或新建会话 |
| 认证失败 (401) | ❌ 不重试 | 需要修复 API Key |
| 免费额度用完 | ❌ 不重试 | 提示充值链接 |

::: tip
如果你看到 OpenCode 在等待重试，不用着急关掉。等一会儿通常就好了。如果频繁触发，考虑切换到其他模型。
:::

---

## 输出截断

当工具输出太长时，OpenCode 会自动截断，避免占用过多上下文空间。

### 截断规则

| 限制 | 默认值 | 说明 |
|------|--------|------|
| 最大行数 | 2,000 行 | 超过会被截断 |
| 最大字节数 | 50 KB | 超过会被截断 |
| 完整输出保留 | 7 天 | 截断后的完整文件会自动清理 |

### 截断后你会看到什么

```
...500 lines truncated...

The tool call succeeded but the output was truncated.
Full output saved to: /path/to/tool-output/tool_xxx
Use Grep to search the full content or Read with offset/limit
to view specific sections.
```

注意：截断不影响操作结果，只是显示被裁剪了。完整内容保存在本地文件中。

### 怎么处理截断的输出

1. **让 AI 用 Grep 搜索**：告诉 AI "在截断的输出中搜索 xxx"
2. **让 AI 分段读取**：AI 会用 Read 工具的 `offset` 和 `limit` 参数分段查看
3. **委托给子 Agent**：如果 AI 有 Task 工具权限，会建议委托给 explore agent 处理，这样更省上下文

::: tip
截断是正常行为，不是错误。大多数情况下 AI 会自动处理截断的输出，你不需要干预。
:::

---

## 权限拒绝

### 症状

操作被拒绝，显示：
```
The user rejected permission to use this specific tool call.
```

### 默认权限规则

OpenCode 对不同操作有不同的默认权限：

| 操作 | 默认权限 | 说明 |
|------|---------|------|
| 读取普通文件 | ✅ 允许 | 大多数文件直接读取 |
| 读取 `.env` 文件 | ⚠️ 需确认 | 保护敏感配置（API Key 等） |
| 读取 `.env.*` 文件 | ⚠️ 需确认 | 如 `.env.local`、`.env.production` |
| 读取 `.env.example` | ✅ 允许 | 示例文件没有敏感信息 |
| 访问项目外目录 | ⚠️ 需确认 | 防止越权访问 |
| 编辑文件（Build 模式） | ✅ 允许 | Build 模式默认可写 |
| 编辑文件（Plan 模式） | ❌ 拒绝 | Plan 模式禁止编辑普通文件（计划文件除外） |
| 执行 Bash 命令 | ✅ 允许 | 默认允许 |

### 解决方案

**单次允许**：在权限提示时按 `y` 确认

**始终允许**：按 `a` 键，该会话内不再询问同类操作

**修改配置**：在 `opencode.json` 中设置权限策略

```json
{
  "permission": {
    "read": "allow",
    "edit": "allow",
    "bash": "ask"
  }
}
```

也可以针对特定文件模式设置：

```json
{
  "permission": {
    "read": {
      "*": "allow",
      "*.env": "allow"
    }
  }
}
```

---

## 网络问题

### 连接超时

**症状**：
```
ETIMEDOUT / ECONNREFUSED / ECONNRESET
```

**诊断步骤**：

```bash
# 1. 测试基本网络
ping api.anthropic.com

# 2. 测试 HTTPS
curl -v https://api.anthropic.com

# 3. 测试代理
curl -x http://127.0.0.1:7890 https://api.anthropic.com
```

**解决方案**：

```bash
# 设置代理
export HTTP_PROXY=http://127.0.0.1:7890
export HTTPS_PROXY=http://127.0.0.1:7890

# 或使用国产模型（无需代理）
/connect  # 选择 Z.AI
```

---

### SSL/TLS 错误

**症状**：
```
UNABLE_TO_VERIFY_LEAF_SIGNATURE
CERT_HAS_EXPIRED
```

**解决方案**：

```bash
# 企业证书
export NODE_EXTRA_CA_CERTS=/path/to/cert.pem

# 临时禁用验证（不推荐生产使用）
export NODE_TLS_REJECT_UNAUTHORIZED=0
```

---

## 文件操作问题

### 文件写入失败

**症状**：AI 说修改了文件但实际没变

**诊断步骤**：

```bash
# 1. 检查文件权限
ls -la path/to/file

# 2. 检查 OpenCode 权限设置
cat ~/.config/opencode/opencode.json | jq .permission
```

**解决方案**：
- 确保文件可写
- 检查权限设置为 `allow` 或 `ask`
- 在权限提示时按 `y` 确认

---

### Git 撤销不工作

**症状**：`/undo` 没有效果

**诊断步骤**：

```bash
# 确认是 Git 仓库
git status
```

**解决方案**：
- 确保在 Git 仓库中
- 确保有可撤销的修改

---

## 界面问题

### 乱码显示

**症状**：界面显示方框或乱码

**解决方案**：

```bash
# 设置正确编码
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8

# 使用推荐终端
# WezTerm / Alacritty / iTerm2

# 使用 Nerd Fonts 字体
```

---

### 颜色不正确

**症状**：颜色显示异常或没有颜色

**解决方案**：

```bash
# 设置 truecolor
export COLORTERM=truecolor

# 设置终端类型
export TERM=xterm-256color

# 切换主题
/theme
```

---

## Linux 特有问题

### 剪贴板不工作

**症状**：复制粘贴失败

**解决方案**：

```bash
# X11
sudo apt install xclip
# 或
sudo apt install xsel

# Wayland
sudo apt install wl-clipboard

# 无头环境
sudo apt install xvfb
Xvfb :99 -screen 0 1024x768x24 > /dev/null 2>&1 &
export DISPLAY=:99.0
```

---

### npm 安装权限不足 (EACCES)

**症状**：
```
EACCES: permission denied
```

**解决方案**：

```bash
# 修复 npm 权限
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc

# 重新安装
npm install -g opencode-ai
```

---

## 快速诊断清单

遇到问题时，按这个顺序逐一排查：

- [ ] **检查版本**：`opencode --version`，确认是最新版
- [ ] **检查配置**：`opencode.json` 语法是否正确？（用 `jq .` 验证 JSON 格式）
- [ ] **检查认证**：`opencode auth list`，API Key 是否有效？OAuth 是否过期？
- [ ] **检查配额**：账户余额是否充足？是否触发速率限制？
- [ ] **检查网络**：能否访问提供商 API？（用 `curl` 测试）
- [ ] **检查权限**：是否有必要的文件/目录访问权限？
- [ ] **查看日志**：开启 debug 模式查看详细日志

```bash
# 快速诊断三连
opencode --version
opencode auth list
opencode --log-level DEBUG --print-logs
```

---

## 错误代码速查表

### 提供商 HTTP 错误

| 错误码 | 含义 | 可能原因 | 是否自动重试 |
|--------|------|---------|-------------|
| 400 | 请求格式错误 | 无效参数、缺少必填字段 | ❌ |
| 401 | 认证失败 | API Key 无效或过期 | ❌ |
| 403 | 权限不足 | 没有访问该模型的权限 | ❌ |
| 404 | 资源不存在 | 模型名称错误或已下线 | ⚠️ OpenAI 有时误报 |
| 413 | 请求体过大 | 上下文溢出（Cerebras/Mistral） | ❌ |
| 429 | 请求过多 | 触发速率限制或配额限制 | ✅ |
| 500 | 服务器错误 | 提供商内部错误 | ✅ |
| 503 | 服务不可用 | 提供商维护或过载 | ✅ |

### OpenCode 内部错误

| 错误类型 | 含义 | 解决方案 |
|---------|------|---------|
| `ContextOverflowError` | 上下文溢出 | `/compact` 压缩或 `/new` 新建会话 |
| `AI_APICallError` | API 调用失败 | 检查网络和认证 |
| `ProviderInitError` | 提供商初始化失败 | 检查配置和认证 |
| `ProviderModelNotFoundError` | 模型不存在 | 检查模型名称格式 `provider/model` |
| `RejectedError` | 用户拒绝了权限 | 在权限提示时按 `y` 允许 |
| `DeniedError` | 配置规则拒绝了操作 | 检查 `opencode.json` 的 `permission` 配置 |

### 上下文溢出的提供商错误信息

不同提供商报上下文溢出的错误信息不同，OpenCode 会统一识别：

| 提供商 | 错误信息模式 |
|--------|-------------|
| Anthropic | `prompt is too long` |
| OpenAI | `exceeds the context window` |
| Google Gemini | `input token count exceeds the maximum` |
| DeepSeek | `maximum context length is X tokens` |
| OpenRouter | `maximum context length is X tokens` |
| Groq | `reduce the length of the messages` |
| Amazon Bedrock | `input is too long for requested model` |
| xAI (Grok) | `maximum prompt length is X` |
| GitHub Copilot | `exceeds the limit of X` |
| llama.cpp | `exceeds the available context size` |
| LM Studio | `greater than the context length` |
| Cerebras/Mistral | `400 (no body)` / `413 (no body)` |
| MiniMax | `context window exceeds limit` |
| Moonshot/Kimi | `exceeded model token limit` |
| 通用 fallback | `context length exceeded` |

遇到这些错误，用 `/compact` 压缩上下文或 `/new` 新建会话。

---

## 诊断命令汇总

```bash
# 版本信息
opencode --version

# 系统信息
uname -a

# 调试日志
opencode --log-level DEBUG --print-logs

# 查看已认证的提供商
opencode auth list

# 查看可用模型
opencode models
```

---

## 获取帮助

如果以上方法都无法解决问题：

1. **收集信息**：
   ```bash
   opencode --version > debug.txt
   uname -a >> debug.txt
   cat ~/.config/opencode/opencode.json >> debug.txt 2>/dev/null
   ls -la ~/.local/share/opencode/log/ >> debug.txt
   ```

2. **提交 Issue**：[github.com/anomalyco/opencode/issues](https://github.com/anomalyco/opencode/issues)

---

## 相关资源

- [常见问题 FAQ](./faq) - 常见问题快速解答
- [网络配置](../1-start/03-network) - 网络设置详解
- [配置选项](./config-ref) - 配置参考
- [上下文压缩](../5-advanced/20-compaction) - 压缩机制详解
- [模型提供商](./providers) - 可用模型列表

::: tip 还是搞不定？
[加入社群](/community)，和 500+ 同路人一起交流，实时答疑。
:::
