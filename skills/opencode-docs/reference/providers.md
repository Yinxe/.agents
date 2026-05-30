# LLM 提供商配置

OpenCode 使用 [AI SDK](https://ai-sdk.dev/) 和 [Models.dev](https://models.dev)，支持 **75+ LLM 提供商**。

## 连接提供商

```bash
/connect
```

凭据存储在 `~/.local/share/opencode/auth.json`。

## OpenCode Zen

OpenCode 团队验证推荐的模型列表，新用户推荐使用。

1. `/connect` → 选择 opencode
2. 登录 https://opencode.ai/auth
3. 复制 API 密钥
4. `/models` 查看推荐模型

## 支持的提供商

### Anthropic

```bash
/connect
# 选择 Anthropic，使用 OAuth 或手动输入 API 密钥
/models
```

### OpenAI

```bash
/connect
# 选择 ChatGPT Plus/Pro 或手动输入 API 密钥
/models
```

### Amazon Bedrock

配置方式：
- 环境变量：`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`
- AWS Profile：`AWS_PROFILE`
- Bearer Token：`AWS_BEARER_TOKEN_BEDROCK`

```json
{
  "provider": {
    "amazon-bedrock": {
      "options": {
        "region": "us-east-1",
        "profile": "my-aws-profile"
      }
    }
  }
}
```

认证优先级：
1. Bearer Token（`AWS_BEARER_TOKEN_BEDROCK`）
2. AWS 凭证链

### Azure OpenAI

1. 创建 Azure OpenAI 资源
2. 在 Azure AI Foundry 部署模型
3. `/connect` → Azure，输入 API 密钥
4. 设置 `AZURE_RESOURCE_NAME` 环境变量

### Google Vertex AI

```bash
export GOOGLE_APPLICATION_CREDENTIALS=/path/to/service-account.json
export GOOGLE_CLOUD_PROJECT=your-project-id
opencode
```

### 本地模型

#### Ollama

```json
{
  "provider": {
    "ollama": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Ollama (local)",
      "options": {
        "baseURL": "http://localhost:11434/v1"
      }
    }
  }
}
```

#### LM Studio

```json
{
  "provider": {
    "lmstudio": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "LM Studio (local)",
      "options": {
        "baseURL": "http://127.0.0.1:1234/v1"
      }
    }
  }
}
```

### 其他提供商

- **302.AI** - 302.AI 控制台生成 API 密钥
- **Cerebras** - Cerebras 控制台
- **Cloudflare AI Gateway** - 统一端点访问
- **DeepSeek** - DeepSeek 平台
- **Fireworks AI** - Fireworks AI 控制台
- **GitHub Copilot** - OAuth 认证
- **GitLab Duo** - GitLab 的 Claude 代理
- **Groq** - Groq 控制台
- **Hugging Face** - Inference Providers
- **OpenRouter** - OpenRouter 仪表盘
- **Together AI** - Together AI 控制台
- **Vercel AI Gateway** - Vercel 仪表盘
- **xAI** - xAI 控制台

## 自定义提供商

添加 OpenAI 兼容的自定义提供商：

1. `/connect` → 选择 **Other**
2. 输入提供商 ID（如 `myprovider`）
3. 输入 API 密钥
4. 在 `opencode.json` 中配置：

```json
{
  "provider": {
    "myprovider": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "My AI Provider",
      "options": {
        "baseURL": "https://api.myprovider.com/v1"
      },
      "models": {
        "my-model-name": {
          "name": "My Model Display Name"
        }
      }
    }
  }
}
```

## 配置选项

### 自定义 Base URL

```json
{
  "provider": {
    "anthropic": {
      "options": {
        "baseURL": "https://api.anthropic.com/v1"
      }
    }
  }
}
```

### 提供商特定选项

#### Amazon Bedrock

```json
{
  "provider": {
    "amazon-bedrock": {
      "options": {
        "region": "us-east-1",
        "profile": "my-aws-profile",
        "endpoint": "https://bedrock-runtime.us-east-1.vpce-xxxxx.amazonaws.com"
      }
    }
  }
}
```

#### Azure

```json
{
  "provider": {
    "azure": {
      "options": {
        "apiKey": "{env:AZURE_API_KEY}"
      }
    }
  }
}
```

### 模型选项

```json
{
  "provider": {
    "openai": {
      "models": {
        "gpt-5": {
          "options": {
            "reasoningEffort": "high",
            "textVerbosity": "low"
          }
        }
      }
    },
    "anthropic": {
      "models": {
        "claude-sonnet-4-5-20250929": {
          "options": {
            "thinking": {
              "type": "enabled",
              "budgetTokens": 16000
            }
          }
        }
      }
    }
  }
}
```

## 模型变体

### 内置变体

**Anthropic**：
- `high` - 高思考预算（默认）
- `max` - 最大思考预算

**OpenAI**：
- `none`, `minimal`, `low`, `medium`, `high`, `xhigh`

**Google**：
- `low`, `high`

### 自定义变体

```json
{
  "provider": {
    "openai": {
      "models": {
        "gpt-5": {
          "variants": {
            "thinking": {
              "reasoningEffort": "high",
              "textVerbosity": "low"
            }
          }
        }
      }
    }
  }
}
```

使用 `Ctrl+T` 切换变体。

## 模型选择

### 设置默认模型

```json
{
  "model": "anthropic/claude-sonnet-4-5"
}
```

### 推荐模型

- GPT 5.2 / GPT 5.1 Codex
- Claude Opus 4.5 / Claude Sonnet 4.5
- Minimax M2.1
- Gemini 3 Pro

## 故障排除

1. 检查认证：`opencode auth list`
2. 验证配置中的模型名称（格式：`provider/model`）
3. 确保 API 密钥有效

常见错误：
- `ProviderInitError` - 配置无效，清除存储后重新认证
- `ProviderModelNotFoundError` - 模型名称格式错误，应为 `<providerId>/<modelId>`