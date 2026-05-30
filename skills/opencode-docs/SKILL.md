---
name: opencode-docs
description: OpenCode AI 编码代理中文参考指南。用于在终端、IDE 或 Web 界面中使用 OpenCode 进行 AI 辅助编程、代码审查、自动化任务。当用户询问如何使用 OpenCode、配置代理、设置提供商、集成 GitHub、执行 CLI 命令、自定义工具或排查问题时触发。
---

# OpenCode AI 编码代理参考

OpenCode 是一个开源的 AI 编码代理，提供终端界面、桌面应用和 IDE 扩展等多种使用方式。支持 75+ LLM 提供商，也可运行本地模型。

## 核心概念

### 安装方式

```bash
# 安装脚本（推荐）
curl -fsSL https://opencode.ai/install | bash

# npm
npm install -g opencode-ai

# Homebrew (macOS/Linux)
brew install anomalyco/tap/opencode

# Arch Linux
sudo pacman -S opencode
```

### 启动 OpenCode

```bash
# 终端界面（TUI）
opencode

# 指定项目目录
opencode /path/to/project

# Web 界面
opencode web

# CLI 模式（直接执行命令）
opencode run "解释这段代码"
```

### 配置提供商

1. 运行 `/connect` 命令连接提供商
2. 设置 API 密钥
3. 使用 `/models` 选择模型

### 文件引用

使用 `@` 在消息中引用文件：
```
How is auth handled in @packages/functions/src/api/index.ts
```

### Bash 命令

以 `!` 开头的消息会作为 shell 命令执行：
```
!ls -la
```

## 常用命令

| 命令 | 说明 | 快捷键 |
|------|------|--------|
| `/connect` | 添加提供商 | |
| `/init` | 初始化项目 AGENTS.md | `Ctrl+X I` |
| `/models` | 选择模型 | `Ctrl+X M` |
| `/undo` | 撤销最后一条消息 | `Ctrl+X U` |
| `/redo` | 重做 | `Ctrl+X R` |
| `/share` | 分享会话 | `Ctrl+X S` |
| `/compact` | 压缩上下文 | `Ctrl+X C` |
| `/help` | 显示帮助 | `Ctrl+X H` |
| `/new` | 新建会话 | `Ctrl+X N` |
| `/sessions` | 切换会话 | `Ctrl+X L` |
| `/themes` | 切换主题 | `Ctrl+X T` |
| `/thinking` | 切换思考显示 | |
| `/exit` | 退出 | `Ctrl+X Q` |

## 代理模式

- **Build**（默认）：完整工具访问的开发代理
- **Plan**：只读规划和分析代理（edit/bash 默认需要审批）
- **General**：通用研究代理
- **Explore**：只读代码探索代理

使用 `Tab` 键在主代理间切换。

## 配置文件

主配置文件：`opencode.json` 或 `opencode.jsonc`（支持注释）

配置位置优先级（后面覆盖前面）：
1. 远程配置（`.well-known/opencode`）
2. 全局配置（`~/.config/opencode/opencode.json`）
3. 自定义配置（`OPENCODE_CONFIG` 环境变量）
4. 项目配置（项目中的 `opencode.json`）
5. `.opencode` 目录（代理、命令、插件）
6. 内联配置（`OPENCODE_CONFIG_CONTENT` 环境变量）

## 工具

内置工具：
- `bash` - 执行 shell 命令
- `edit` - 精确字符串替换修改文件
- `write` - 创建/覆盖文件
- `read` - 读取文件内容
- `grep` - 正则表达式搜索
- `glob` - 模式匹配查找文件
- `patch` - 应用补丁
- `skill` - 加载技能
- `todowrite` - 管理待办列表
- `webfetch` - 获取网页内容
- `websearch` - 网络搜索
- `question` - 向用户提问

## 权限控制

```json
{
  "permission": {
    "edit": "deny",
    "bash": "ask",
    "*": "allow"
  }
}
```

- `allow` - 直接运行
- `ask` - 提示审批
- `deny` - 阻止

## MCP 服务器

通过 Model Context Protocol 添加外部工具：

```json
{
  "mcp": {
    "sentry": {
      "type": "remote",
      "url": "https://mcp.sentry.dev/mcp",
      "oauth": {}
    }
  }
}
```

## 参考文档

详细文档见 `references/` 目录：
- `@path references/getting-started.md` - 快速入门指南
- `@path references/configuration.md` - 完整配置参考
- `@path references/providers.md` - LLM 提供商配置
- `@path references/tools.md` - 工具详细说明
- `@path references/agents.md` - 代理配置
- `@path references/cli-reference.md` - CLI 命令参考
- `@path references/github-integration.md` - GitHub 集成
- `@path references/troubleshooting.md` - 故障排除

## 快捷键

默认前导键：`Ctrl+X`

常用快捷键：
- `Ctrl+X N` - 新建会话
- `Ctrl+X L` - 会话列表
- `Ctrl+X M` - 模型列表
- `Ctrl+X C` - 压缩上下文
- `Tab` - 切换代理模式
- `Ctrl+T` - 切换模型变体

## 相关资源

- 官方文档：https://opencode.ai/docs/zh-cn
- GitHub：https://github.com/anomalyco/opencode
- Discord：https://opencode.ai/discord