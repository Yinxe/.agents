# OpenCode 工具参考

## 概述

工具允许 LLM 在代码库中执行操作。OpenCode 自带内置工具，也可通过自定义工具或 MCP 服务器扩展。

## 内置工具

### bash

执行 shell 命令。

```json
{
  "permission": {
    "bash": "allow"
  }
}
```

### edit

通过精确字符串替换修改文件。

```json
{
  "permission": {
    "edit": "allow"
  }
}
```

### write

创建新文件或覆盖现有文件。

```json
{
  "permission": {
    "edit": "allow"
  }
}
```

注意：`write` 由 `edit` 权限控制。

### read

读取文件内容。

```json
{
  "permission": {
    "read": "allow"
  }
}
```

### grep

使用正则表达式搜索文件内容。

```json
{
  "permission": {
    "grep": "allow"
  }
}
```

### glob

通过模式匹配查找文件。

```json
{
  "permission": {
    "glob": "allow"
  }
}
```

### patch

对文件应用补丁。

```json
{
  "permission": {
    "edit": "allow"
  }
}
```

### skill

加载技能（SKILL.md）。

```json
{
  "permission": {
    "skill": "allow"
  }
}
```

### todowrite

管理待办列表。

```json
{
  "permission": {
    "todowrite": "allow"
  }
}
```

### webfetch

获取网页内容。

```json
{
  "permission": {
    "webfetch": "allow"
  }
}
```

### websearch

网络搜索（需要 OpenCode 提供商或设置 `OPENCODE_ENABLE_EXA=1`）。

```json
{
  "permission": {
    "websearch": "allow"
  }
}
```

### question

向用户提问。

```json
{
  "permission": {
    "question": "allow"
  }
}
```

## 忽略模式

`grep` 和 `glob` 底层使用 ripgrep，默认遵循 `.gitignore`。

创建 `.ignore` 文件可以包含通常被忽略的文件：

```
!node_modules/
!dist/
!build/
```

## MCP 服务器

MCP（Model Context Protocol）服务器允许集成外部工具。

### 本地 MCP

```json
{
  "mcp": {
    "mcp_everything": {
      "type": "local",
      "command": ["npx", "-y", "@modelcontextprotocol/server-everything"]
    }
  }
}
```

### 远程 MCP

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

### MCP OAuth

大多数 MCP 服务器无需特殊配置，OpenCode 自动处理 OAuth。

预注册凭据：

```json
{
  "mcp": {
    "my-oauth-server": {
      "type": "remote",
      "url": "https://mcp.example.com/mcp",
      "oauth": {
        "clientId": "{env:MY_MCP_CLIENT_ID}",
        "clientSecret": "{env:MY_MCP_CLIENT_SECRET}",
        "scope": "tools:read tools:execute"
      }
    }
  }
}
```

### MCP 管理命令

```bash
opencode mcp add          # 添加 MCP 服务器
opencode mcp list         # 列出所有 MCP 服务器
opencode mcp auth <name>  # 认证 MCP 服务器
opencode mcp logout <name> # 移除凭据
opencode mcp debug <name>  # 调试 OAuth 连接
```

### MCP 配置示例

#### Sentry

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

认证：`opencode mcp auth sentry`

#### Context7

```json
{
  "mcp": {
    "context7": {
      "type": "remote",
      "url": "https://mcp.context7.com/mcp"
    }
  }
}
```

#### Grep by Vercel

```json
{
  "mcp": {
    "gh_grep": {
      "type": "remote",
      "url": "https://mcp.grep.app"
    }
  }
}
```

## 自定义工具

自定义工具允许定义 LLM 可以调用的自定义函数。见 `@path references/custom-tools.md`。