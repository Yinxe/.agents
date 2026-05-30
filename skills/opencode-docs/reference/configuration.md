# OpenCode 配置参考

## 配置文件格式

OpenCode 支持 **JSON** 和 **JSONC**（带注释的 JSON）格式。

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "model": "anthropic/claude-sonnet-4-5",
  "autoupdate": true,
  "server": {
    "port": 4096
  }
}
```

## 配置位置

配置文件是**合并**在一起的，后面的配置覆盖前面的。

优先级顺序：
1. **远程配置**（`.well-known/opencode`）- 组织默认值
2. **全局配置**（`~/.config/opencode/opencode.json`）
3. **自定义配置**（`OPENCODE_CONFIG` 环境变量）
4. **项目配置**（项目中的 `opencode.json`）
5. **`.opencode` 目录** - 代理、命令、插件
6. **内联配置**（`OPENCODE_CONFIG_CONTENT` 环境变量）

## 配置目录结构

`.opencode` 和 `~/.config/opencode` 子目录：
- `agents/` - 代理定义
- `commands/` - 自定义命令
- `modes/` - 模式定义
- `plugins/` - 插件
- `skills/` - 代理技能
- `tools/` - 自定义工具
- `themes/` - 主题

## Schema 选项

### TUI 配置

```json
{
  "tui": {
    "scroll_speed": 3,
    "scroll_acceleration": {
      "enabled": true
    },
    "diff_style": "auto"
  }
}
```

- `scroll_acceleration.enabled` - 启用 macOS 风格滚动加速
- `scroll_speed` - 滚动速度（默认 3）
- `diff_style` - 差异渲染方式：`"auto"` 或 `"stacked"`

### 服务器配置

```json
{
  "server": {
    "port": 4096,
    "hostname": "0.0.0.0",
    "mdns": true,
    "mdnsDomain": "myproject.local",
    "cors": ["http://localhost:5173"]
  }
}
```

### 工具配置

```json
{
  "tools": {
    "write": false,
    "bash": false
  }
}
```

### 模型配置

```json
{
  "model": "anthropic/claude-sonnet-4-5",
  "small_model": "anthropic/claude-haiku-4-5"
}
```

```json
{
  "provider": {
    "anthropic": {
      "options": {
        "timeout": 600000,
        "setCacheKey": true
      }
    }
  }
}
```

### 代理配置

```json
{
  "agent": {
    "code-reviewer": {
      "description": "Reviews code for best practices",
      "model": "anthropic/claude-sonnet-4-5",
      "prompt": "You are a code reviewer...",
      "tools": {
        "write": false,
        "edit": false
      }
    }
  }
}
```

### 默认代理

```json
{
  "default_agent": "plan"
}
```

### 分享配置

```json
{
  "share": "manual"  // "auto" | "disabled"
}
```

### 命令配置

```json
{
  "command": {
    "test": {
      "template": "Run the full test suite...",
      "description": "Run tests with coverage",
      "agent": "build",
      "model": "anthropic/claude-haiku-4-5"
    }
  }
}
```

### 快捷键配置

在 `tui.json` 中配置：

```json
{
  "$schema": "https://opencode.ai/tui.json",
  "keybinds": {
    "leader": "ctrl+x",
    "session_compact": "<leader>c"
  }
}
```

### 自动更新

```json
{
  "autoupdate": false  // 或 "notify"
}
```

### 格式化程序

```json
{
  "formatter": {
    "prettier": {
      "disabled": true
    }
  }
}
```

### 权限配置

```json
{
  "permission": {
    "edit": "ask",
    "bash": "ask"
  }
}
```

权限值：`"allow"` | `"ask"` | `"deny"`

### 压缩配置

```json
{
  "compaction": {
    "auto": true,
    "prune": true,
    "reserved": 10000
  }
}
```

### 文件监视器

```json
{
  "watcher": {
    "ignore": ["node_modules/**", "dist/**"]
  }
}
```

### MCP 服务器

```json
{
  "mcp": {
    "sentry": {
      "type": "remote",
      "url": "https://mcp.sentry.dev/mcp",
      "enabled": true
    }
  }
}
```

### 插件

```json
{
  "plugin": ["opencode-helicone-session"]
}
```

### 指令

```json
{
  "instructions": ["./custom-instructions.md"]
}
```

### 禁用/启用提供商

```json
{
  "disabled_providers": ["openai", "gemini"]
}
```

```json
{
  "enabled_providers": ["anthropic", "openai"]
}
```

## 变量

### 环境变量

```json
{
  "model": "{env:OPENCODE_MODEL}",
  "provider": {
    "anthropic": {
      "options": {
        "apiKey": "{env:ANTHROPIC_API_KEY}"
      }
    }
  }
}
```

### 文件引用

```json
{
  "instructions": ["./custom-instructions.md"],
  "provider": {
    "openai": {
      "options": {
        "apiKey": "{file:~/.secrets/openai-key}"
      }
    }
  }
}
```

## 环境变量

| 变量 | 说明 |
|------|------|
| `OPENCODE_CONFIG` | 自定义配置文件路径 |
| `OPENCODE_CONFIG_DIR` | 自定义配置目录 |
| `OPENCODE_CONFIG_CONTENT` | 内联 JSON 配置 |
| `OPENCODE_AUTO_SHARE` | 自动分享会话 |
| `OPENCODE_DISABLE_AUTOUPDATE` | 禁用自动更新 |
| `OPENCODE_ENABLE_EXA` | 启用 Exa 搜索工具 |
| `OPENCODE_SERVER_PASSWORD` | 服务器认证密码 |
| `OPENCODE_TUI_CONFIG` | TUI 配置文件路径 |