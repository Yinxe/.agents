# OpenCode 代理参考

## 概述

代理是专门的 AI 助手，可针对特定任务和工作流程配置。

## 代理类型

### 主代理

主代理是直接交互的主要助手。使用 `Tab` 键或 `switch_agent` 快捷键切换。

- **Build**（默认）：完整工具访问
- **Plan**：只读规划和分析（edit/bash 默认需要审批）

### 子代理

子代理由主代理调用执行专门任务，也可通过 `@` 提及手动调用。

- **General**：通用研究代理，完整工具访问
- **Explore**：只读代码探索代理

## 内置代理

| 代理 | 模式 | 说明 |
|------|------|------|
| Build | primary | 默认开发代理，完整工具 |
| Plan | primary | 规划分析代理，受限工具 |
| General | subagent | 通用研究代理 |
| Explore | subagent | 只读代码探索 |
| Compaction | primary | 隐藏系统代理，上下文压缩 |
| Title | primary | 隐藏系统代理，生成会话标题 |
| Summary | primary | 隐藏系统代理，创建会话摘要 |

## 代理配置

### JSON 配置

```json
{
  "agent": {
    "build": {
      "mode": "primary",
      "model": "anthropic/claude-sonnet-4-20250514",
      "prompt": "{file:./prompts/build.txt}",
      "tools": {
        "write": true,
        "edit": true,
        "bash": true
      }
    },
    "plan": {
      "mode": "primary",
      "model": "anthropic/claude-haiku-4-20250514",
      "tools": {
        "write": false,
        "edit": false,
        "bash": false
      }
    },
    "code-reviewer": {
      "description": "Reviews code for best practices",
      "mode": "subagent",
      "model": "anthropic/claude-sonnet-4-20250514",
      "prompt": "You are a code reviewer...",
      "tools": {
        "write": false,
        "edit": false
      }
    }
  }
}
```

### Markdown 配置

将 Markdown 文件放在 `~/.config/opencode/agents/` 或 `.opencode/agents/`：

```markdown
---
description: Reviews code for quality and best practices
mode: subagent
model: anthropic/claude-sonnet-4-20250514
temperature: 0.1
tools:
  write: false
  edit: false
  bash: false
---
You are in code review mode. Focus on:
- Code quality and best practices
- Potential bugs and edge cases
- Performance implications
- Security considerations
```

文件名即为代理名称，如 `review.md` 创建名为 `review` 的代理。

## 配置选项

### description

代理功能的简要描述（必需）。

```json
{
  "agent": {
    "review": {
      "description": "Reviews code for best practices"
    }
  }
}
```

### temperature

控制响应的随机性和创造力（0.0-1.0）。

```json
{
  "agent": {
    "plan": {
      "temperature": 0.1
    },
    "creative": {
      "temperature": 0.8
    }
  }
}
```

- 0.0-0.2：集中确定性
- 0.3-0.5：平衡
- 0.6-1.0：更有创造力

### maxSteps

代理在被强制停止前的最大迭代次数。

```json
{
  "agent": {
    "quick-thinker": {
      "description": "Fast reasoning",
      "prompt": "You are a quick thinker...",
      "steps": 5
    }
  }
}
```

### disable

禁用代理。

```json
{
  "agent": {
    "review": {
      "disable": true
    }
  }
}
```

### prompt

自定义系统提示词文件路径。

```json
{
  "agent": {
    "review": {
      "prompt": "{file:./prompts/code-review.txt}"
    }
  }
}
```

### model

覆盖默认模型。

```json
{
  "agent": {
    "plan": {
      "model": "anthropic/claude-haiku-4-20250514"
    }
  }
}
```

### tools

控制代理可用的工具。

```json
{
  "agent": {
    "plan": {
      "tools": {
        "write": false,
        "bash": false
      }
    }
  }
}
```

使用通配符：
```json
{
  "tools": {
    "mymcp_*": false
  }
}
```

### permission

配置权限为 `allow`、`ask`、`deny`。

```json
{
  "permission": {
    "edit": "deny"
  },
  "agent": {
    "build": {
      "permission": {
        "edit": "ask"
      }
    }
  }
}
```

为特定 bash 命令设置权限：

```json
{
  "agent": {
    "build": {
      "permission": {
        "bash": {
          "git push": "ask",
          "grep *": "allow"
        }
      }
    }
  }
}
```

使用 glob 模式：

```json
{
  "permission": {
    "bash": {
      "*": "ask",
      "git *": "allow"
    }
  }
}
```

### mode

- `primary` - 主代理
- `subagent` - 子代理
- `all` - 两者兼可

### hidden

隐藏子代理从 `@` 自动补全菜单。

```json
{
  "agent": {
    "internal-helper": {
      "mode": "subagent",
      "hidden": true
    }
  }
}
```

### task permission

控制代理可以通过 Task 工具调用哪些子代理。

```json
{
  "agent": {
    "orchestrator": {
      "mode": "primary",
      "permission": {
        "task": {
          "*": "deny",
          "orchestrator-*": "allow",
          "code-reviewer": "ask"
        }
      }
    }
  }
}
```

### color

UI 颜色：`"#FF5733"` 或 `primary`/`secondary`/`accent`/`success`/`warning`/`error`/`info`。

```json
{
  "agent": {
    "creative": {
      "color": "#ff6b6b"
    }
  }
}
```

### top_p

控制响应多样性（温度替代方案）。

```json
{
  "agent": {
    "brainstorm": {
      "top_p": 0.9
    }
  }
}
```

## 创建代理

```bash
opencode agent create
```

交互式命令引导创建代理。

## 使用场景

| 代理 | 用途 |
|------|------|
| Build | 完整开发工作 |
| Plan | 分析和规划，不修改代码 |
| Review | 代码审查，只读访问 |
| Debug | 问题排查 |
| Docs | 文档编写 |