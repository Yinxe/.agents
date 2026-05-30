# OpenCode 快速入门

## 前提条件

- 现代终端模拟器：WezTerm、Alacritty、Ghostty、Kitty
- LLM 提供商的 API 密钥

## 安装

```bash
# 安装脚本（推荐）
curl -fsSL https://opencode.ai/install | bash

# npm
npm install -g opencode-ai

# Homebrew
brew install anomalyco/tap/opencode

# Arch Linux
sudo pacman -S opencode
```

## Windows 建议

推荐使用 WSL（Windows Subsystem for Linux）以获得最佳体验。

## 首次配置

### 1. 连接提供商

```bash
opencode
/connect
```

选择 opencode，浏览器打开 https://opencode.ai/auth，登录并添加账单信息，复制 API 密钥并粘贴。

### 2. 选择模型

```bash
/models
```

推荐使用 OpenCode Zen 模型，新用户友好。

### 3. 初始化项目

```bash
cd /path/to/project
opencode
/init
```

这会在项目根目录创建 `AGENTS.md` 文件，帮助 OpenCode 理解项目结构。

**建议将 AGENTS.md 提交到 Git。**

## 基本使用

### 提问

```
How is authentication handled in @packages/functions/src/api/index.ts
```

使用 `@` 引用文件，OpenCode 会自动读取文件内容。

### 添加功能

1. **制定计划**：按 `Tab` 切换到计划模式，描述需求
2. **迭代计划**：提供反馈，补充细节
3. **构建功能**：按 `Tab` 切换回构建模式，执行

### 直接修改

对于简单修改，直接让 OpenCode 执行：

```
We need to add authentication to the /settings route. Take a look at how
this is handled in @packages/functions/src/notes.ts and implement the same
logic in @packages/functions/src/settings.ts
```

### 撤销修改

```
/undo
```

会还原所有文件更改，可以调整提示词重新尝试。

## 分享会话

```
/share
```

生成当前对话链接并复制到剪贴板。

## 个性化配置

- 选择主题：`/themes`
- 自定义快捷键
- 配置代码格式化工具
- 创建自定义命令

## TUI 命令

| 命令 | 说明 |
|------|------|
| `/connect` | 添加提供商 |
| `/compact` | 压缩当前会话 |
| `/details` | 切换工具执行详情 |
| `/editor` | 打开外部编辑器 |
| `/exit` | 退出 |
| `/export` | 导出对话为 Markdown |
| `/help` | 显示帮助 |
| `/init` | 创建/更新 AGENTS.md |
| `/models` | 列出可用模型 |
| `/new` | 开始新会话 |
| `/redo` | 重做 |
| `/sessions` | 会话列表/切换 |
| `/share` | 分享当前会话 |
| `/themes` | 列出可用主题 |
| `/thinking` | 切换思考显示 |
| `/undo` | 撤销 |
| `/unshare` | 取消分享 |

## 编辑器配置

设置 `EDITOR` 环境变量以使用 `/editor` 和 `/export` 命令：

```bash
# Linux/macOS
export EDITOR=vim
export EDITOR="code --wait"

# Windows
set EDITOR=notepad
```