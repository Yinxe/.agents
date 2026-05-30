# OpenCode GitHub 集成

## 概述

OpenCode 可以与 GitHub 工作流集成。在 Issue 或 PR 评论中提及 `/opencode` 或 `/oc`，OpenCode 会在 GitHub Actions 运行器中执行任务。

## 功能

- **问题分类**：调查 Issue 并解释
- **修复与实现**：修复 Issue 或实现功能，新分支工作，提交 PR
- **安全可靠**：在你自己 的 GitHub 运行器中运行

## 安装

### 自动安装

```bash
opencode github install
```

引导完成 GitHub App 安装、工作流创建和密钥配置。

### 手动设置

1. **安装 GitHub App**
   前往 https://github.com/apps/opencode-agent

2. **添加工作流**

   `.github/workflows/opencode.yml`:

```yaml
name: opencode
on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]

jobs:
  opencode:
    if: |
      contains(github.event.comment.body, '/oc') ||
      contains(github.event.comment.body, '/opencode')
    runs-on: ubuntu-latest
    permissions:
      id-token: write
    steps:
      - name: Checkout repository
        uses: actions/checkout@v6
        with:
          fetch-depth: 1
          persist-credentials: false

      - name: Run OpenCode
        uses: anomalyco/opencode/github@latest
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        with:
          model: anthropic/claude-sonnet-4-20250514
```

3. **添加 Secrets**
   在 Settings → Secrets and variables → Actions 添加 API 密钥。

## 配置选项

| 选项 | 说明 |
|------|------|
| `model` | 使用的模型（必需）|
| `agent` | 使用的代理（默认 build）|
| `share` | 是否分享会话 |
| `prompt` | 自定义提示词 |
| `token` | GitHub 访问令牌 |

使用 `GITHUB_TOKEN`（无需安装 App）：

```yaml
permissions:
  id-token: write
  contents: write
  pull-requests: write
  issues: write
```

## 支持的事件

| 事件类型 | 触发方式 |
|----------|----------|
| `issue_comment` | Issue/PR 评论中 @ `/oc` |
| `pull_request_review_comment` | PR 代码行评论 |
| `issues` | Issue 创建/编辑（需 prompt）|
| `pull_request` | PR 创建/更新（自动审查）|
| `schedule` | Cron 定时任务（需 prompt）|
| `workflow_dispatch` | 手动触发（需 prompt）|

## 示例

### 定时任务

```yaml
name: Scheduled OpenCode Task
on:
  schedule:
    - cron: "0 9 * * 1"  # 每周一 9am

jobs:
  opencode:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: write
      pull-requests: write
      issues: write
    steps:
      - uses: actions/checkout@v6
        with:
          persist-credentials: false

      - uses: anomalyco/opencode/github@latest
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        with:
          model: anthropic/claude-sonnet-4-20250514
          prompt: |
            Review the codebase for TODO comments and create a summary.
            If you find issues, open an issue to track them.
```

### PR 审查

```yaml
name: opencode-review
on:
  pull_request:
    types: [opened, synchronize, reopened, ready_for_review]

jobs:
  review:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
      pull-requests: read
      issues: read
    steps:
      - uses: actions/checkout@v6
        with:
          persist-credentials: false

      - uses: anomalyco/opencode/github@latest
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          model: anthropic/claude-sonnet-4-20250514
          use_github_token: true
          prompt: |
            Review this pull request:
            - Check for code quality issues
            - Look for potential bugs
            - Suggest improvements
```

### Issue 分类

```yaml
name: Issue Triage
on:
  issues:
    types: [opened]

jobs:
  triage:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: write
      pull-requests: write
      issues: write
    steps:
      - name: Check account age
        id: check
        uses: actions/github-script@v7
        with:
          script: |
            const user = await github.rest.users.getByUsername({
              username: context.payload.issue.user.login
            });
            const created = new Date(user.data.created_at);
            const days = (Date.now() - created) / (1000 * 60 * 60 * 24);
            return days >= 30;
          result-encoding: string

      - uses: actions/checkout@v6
        if: steps.check.outputs.result == 'true'
        with:
          persist-credentials: false

      - uses: anomalyco/opencode/github@latest
        if: steps.check.outputs.result == 'true'
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        with:
          model: anthropic/claude-sonnet-4-20250514
          prompt: |
            Review this issue and provide guidance.
```

## 使用示例

### 解释 Issue

```
/opencode explain this issue
```

### 修复 Issue

```
/opencode fix this
```

创建新分支，提交包含修改的 PR。

### PR 修改

```
Delete the attachment from S3 when the note is removed /oc
```

修改提交到同一 PR。

### 代码行评论

在 PR "Files" 选项卡中对代码行评论：

```
/oc add error handling here
```

收到文件路径、行号和 diff 上下文。

## 自定义提示词

```yaml
- uses: anomalyco/opencode/github@latest
  with:
    model: anthropic/claude-sonnet-4-5
    prompt: |
      Review this pull request:
      - Check for code quality issues
      - Look for potential bugs
      - Suggest improvements
```

## 相关资源

- GitHub App: https://github.com/apps/opencode-agent
- 官方文档: https://opencode.ai/docs/zh-cn/github