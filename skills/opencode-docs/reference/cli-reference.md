# OpenCode CLI 参考

## 概述

OpenCode CLI 默认启动 TUI，也可接受各种命令进行编程交互。

## tui

启动终端用户界面。

```bash
opencode [project]
```

### 标志

| 标志 | 简写 | 说明 |
|------|------|------|
| `--continue` | `-c` | 继续上一个会话 |
| `--session` | `-s` | 继续指定会话 |
| `--fork` | | 分叉会话 |
| `--prompt` | | 使用的提示词 |
| `--model` | `-m` | 模型（provider/model） |
| `--agent` | | 使用的代理 |
| `--port` | | 监听端口 |
| `--hostname` | | 监听主机名 |

## 命令

### agent

管理代理。

```bash
opencode agent [command]
```

- `create` - 创建新代理
- `list` - 列出所有代理

### attach

连接终端到 OpenCode 后端服务器。

```bash
opencode attach [url]
```

### auth

管理提供商凭据。

```bash
opencode auth [command]
```

- `login` - 添加提供商凭据
- `list` / `ls` - 列出已认证提供商
- `logout` - 移除提供商

### github

管理 GitHub 代理。

```bash
opencode github [command]
```

- `install` - 安装 GitHub 代理
- `run` - 运行 GitHub 代理

### mcp

管理 MCP 服务器。

```bash
opencode mcp [command]
```

- `add` - 添加 MCP 服务器
- `list` / `ls` - 列出 MCP 服务器
- `auth [name]` - 认证 MCP 服务器
- `logout [name]` - 移除凭据
- `debug <name>` - 调试 OAuth 连接

### models

列出可用模型。

```bash
opencode models [provider]
```

标志：
- `--refresh` - 刷新模型缓存
- `--verbose` - 显示详细元数据

### run

非交互模式运行。

```bash
opencode run [message..]
```

标志：

| 标志 | 简写 | 说明 |
|------|------|------|
| `--command` | | 运行的命令 |
| `--continue` | `-c` | 继续上一个会话 |
| `--session` | `-s` | 继续指定会话 |
| `--fork` | | 分叉会话 |
| `--share` | | 分享会话 |
| `--model` | `-m` | 模型 |
| `--agent` | | 代理 |
| `--file` | `-f` | 附加文件 |
| `--format` | | `default` 或 `json` |
| `--title` | | 会话标题 |
| `--attach` | | 连接到服务器 |
| `--port` | | 本地端口 |

### serve

启动无界面 HTTP 服务器。

```bash
opencode serve
```

标志：
- `--port` - 监听端口
- `--hostname` - 监听主机名
- `--mdns` - 启用 mDNS 发现
- `--cors` - 允许 CORS 来源

### session

管理会话。

```bash
opencode session [command]
```

- `list` - 列出所有会话

标志：
- `--max-count` / `-n` - 限制数量
- `--format` - `table` 或 `json`

### stats

显示 Token 用量和费用统计。

```bash
opencode stats
```

标志：
- `--days` - 最近 N 天
- `--tools` - 显示工具数量
- `--models` - 显示模型用量
- `--project` - 按项目筛选

### export

导出会话数据为 JSON。

```bash
opencode export [sessionID]
```

### import

导入会话数据。

```bash
opencode import <file|url>
```

### web

启动带 Web 界面的服务器。

```bash
opencode web
```

标志：
- `--port` - 监听端口
- `--hostname` - 监听主机名
- `--mdns` - 启用 mDNS
- `--cors` - 允许 CORS

### acp

启动 ACP（Agent Client Protocol）服务器。

```bash
opencode acp
```

标志：
- `--cwd` - 工作目录
- `--port` - 端口
- `--hostname` - 主机名

### uninstall

卸载 OpenCode。

```bash
opencode uninstall
```

标志：
- `--keep-config` / `-c` - 保留配置
- `--keep-data` / `-d` - 保留数据
- `--dry-run` - 显示但不删除
- `--force` / `-f` - 跳过确认

### upgrade

更新 OpenCode。

```bash
opencode upgrade [target]
```

标志：
- `--method` / `-m` - 安装方式

## 全局标志

| 标志 | 简写 | 说明 |
|------|------|------|
| `--help` | `-h` | 显示帮助 |
| `--version` | `-v` | 打印版本 |
| `--print-logs` | | 输出日志到 stderr |
| `--log-level` | | DEBUG/INFO/WARN/ERROR |

## 环境变量

| 变量 | 类型 | 说明 |
|------|------|------|
| `OPENCODE_AUTO_SHARE` | boolean | 自动分享 |
| `OPENCODE_CONFIG` | string | 配置文件路径 |
| `OPENCODE_CONFIG_DIR` | string | 配置目录 |
| `OPENCODE_CONFIG_CONTENT` | string | 内联配置 |
| `OPENCODE_DISABLE_AUTOUPDATE` | boolean | 禁用更新 |
| `OPENCODE_DISABLE_PRUNE` | boolean | 禁用清理 |
| `OPENCODE_DISABLE_TERMINAL_TITLE` | boolean | 禁用标题更新 |
| `OPENCODE_PERMISSION` | string | 内联权限配置 |
| `OPENCODE_DISABLE_DEFAULT_PLUGINS` | boolean | 禁用默认插件 |
| `OPENCODE_DISABLE_LSP_DOWNLOAD` | boolean | 禁用 LSP 下载 |
| `OPENCODE_ENABLE_EXPERIMENTAL_MODELS` | boolean | 启用实验模型 |
| `OPENCODE_DISABLE_AUTOCOMPACT` | boolean | 禁用自动压缩 |
| `OPENCODE_DISABLE_CLAUDE_CODE` | boolean | 禁用 .claude |
| `OPENCODE_ENABLE_EXA` | boolean | 启用 Exa 搜索 |
| `OPENCODE_SERVER_PASSWORD` | string | 服务器密码 |
| `OPENCODE_SERVER_USERNAME` | string | 服务器用户名 |
| `OPENCODE_MODELS_URL` | string | 自定义模型 URL |

## 实验性功能

| 变量 | 类型 | 说明 |
|------|------|------|
| `OPENCODE_EXPERIMENTAL` | boolean | 全部实验功能 |
| `OPENCODE_EXPERIMENTAL_ICON_DISCOVERY` | boolean | 图标发现 |
| `OPENCODE_EXPERIMENTAL_DISABLE_COPY_ON_SELECT` | boolean | 禁用选中复制 |
| `OPENCODE_EXPERIMENTAL_BASH_DEFAULT_TIMEOUT_MS` | number | bash 超时 |
| `OPENCODE_EXPERIMENTAL_OUTPUT_TOKEN_MAX` | number | 最大输出 Token |
| `OPENCODE_EXPERIMENTAL_FILEWATCHER` | boolean | 文件监听器 |
| `OPENCODE_EXPERIMENTAL_OXFMT` | boolean | oxfmt 格式化器 |
| `OPENCODE_EXPERIMENTAL_LSP_TOOL` | boolean | LSP 工具 |
| `OPENCODE_EXPERIMENTAL_EXA` | boolean | Exa 功能 |
| `OPENCODE_EXPERIMENTAL_LSP_TY` | boolean | Python TY LSP |
| `OPENCODE_EXPERIMENTAL_MARKDOWN` | boolean | Markdown 功能 |
| `OPENCODE_EXPERIMENTAL_PLAN_MODE` | boolean | 计划模式 |