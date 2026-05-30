---
name: string-fit
description: >
  Extract user-facing translatable strings from source code projects of any size and translate them into multiple languages.
  Use this skill whenever the user needs to internationalize (i18n) a codebase, find hardcoded strings that need translation,
  create translation files, extract UI strings, localize an app, or mentions i18n, l10n, internationalization, localization,
  multi-language support, translation extraction, or wants to scan source code for translatable text.
  Also use when the user asks to "find all strings that need translation", "extract text for translation",
  "create translation JSON", "scan codebase for i18n", or wants to prepare a project for multi-language support.
  Works with any programming language (JS/TS, Python, Java, Go, Rust, C++, PHP, Ruby, Swift, Kotlin, etc.).
  Outputs structured JSON with source locations and translations to `string.fit/` directory.
---

# String Fit — 源码多语言翻译提取 Skill

从任意规模的源码项目中智能提取面向用户的字符串，并翻译为多国语言。

## 核心原则

- **只提取 UI 字符串**：面向最终用户的可见文本。排除日志信息、错误码、变量名、技术标识符、内部键名等。
- **记录完整位置信息**：每个字符串都要记录所在文件路径、行号、列号。
- **全部 JSON 化**：流程中每个阶段的产出都用 JSON 存储。
- **子智能体并行**：大项目通过子智能体分工加速。

## 输出目录

所有产出固定存放在项目根目录下的 `string.fit/`：

```
string.fit/
├── config.json              # 本次任务配置
├── file-index.json          # 源码文件清单（按语言分类）
├── extracted.json           # 提取的原始字符串（含位置）
├── filtered.json            # 过滤后的 UI 字符串
├── translations/
│   ├── zh.json              # 中文翻译
│   ├── ja.json              # 日文翻译
│   └── ...                  # 其他语言
└── final.json               # 最终完整翻译文件
```

## 工作流程

### Phase 1: 扫描与索引

1. 根据用户指定的源码范围，扫描所有源码文件
2. 按编程语言分类，生成 `file-index.json`
3. 同时生成 `config.json` 记录任务配置

**file-index.json 格式：**
```json
{
  "source_root": "src/",
  "total_files": 42,
  "by_language": {
    "typescript": ["src/app.ts", "src/components/Button.tsx"],
    "python": ["src/main.py", "src/utils.py"]
  }
}
```

### Phase 2: 并行提取字符串

将文件按语言分组，为每组启动子智能体并行提取。

**子智能体 prompt 模板：**
```
提取以下源码文件中所有字符串字面量，记录每个字符串的：
- value: 字符串内容
- file: 文件路径（相对路径）
- line: 行号
- column: 列号
- context: 上下文代码（前后各2行）
- language: 编程语言

只提取字符串字面量，不要做过滤判断。
将结果保存为 JSON 数组。

文件列表：[文件路径列表]
输出路径：string.fit/extracted-{language}.json
```

合并所有子智能体的结果到 `extracted.json`。

### Phase 3: 智能过滤

分析 `extracted.json`，过滤出真正的 UI 字符串。

**过滤规则 — 保留：**
- JSX/HTML 模板中的文本内容
- 按钮文字、标签文字、提示文字、占位符文字
- 页面标题、导航文字、菜单项
- 表单标签、验证错误提示
- 弹窗/对话框内容
- Toast/通知消息
- 用户可见的状态文字

**过滤规则 — 排除：**
- 日志消息（console.log, logger.info 等中的字符串）
- 错误码/状态码（"E001", "HTTP_404"）
- CSS 类名、ID 选择器
- 正则表达式字符串
- 变量名、函数名、属性名
- 文件路径、URL 模板
- 数据库查询字符串、SQL
- 技术配置值
- 纯数字字符串
- 单字符字符串（除非是 UI 符号如 "×" 关闭按钮）

将过滤结果保存为 `filtered.json`。

**filtered.json 格式：**
```json
{
  "strings": [
    {
      "id": "str_001",
      "value": "Welcome to our app",
      "file": "src/pages/Home.tsx",
      "line": 24,
      "column": 12,
      "context": "<h1>Welcome to our app</h1>",
      "language": "typescript",
      "confidence": 0.95,
      "reason": "JSX heading text, clearly user-facing"
    }
  ],
  "total_extracted": 500,
  "total_filtered": 120,
  "filter_rate": 0.76
}
```

### Phase 4: 并行翻译

为每种目标语言启动独立的子智能体并行翻译。

**子智能体 prompt 模板：**
```
将以下字符串翻译为 {target_language}。

要求：
1. 保持专业术语的一致性
2. 考虑上下文（context 字段提供了代码上下文）
3. 如果是技术产品，使用行业通用译法
4. 保留占位符变量（如 {name}, %s, {{count}} 等）
5. 对于不适合翻译的字符串（如图标符号），保持原样

输入文件：string.fit/filtered.json
输出文件：string.fit/translations/{lang_code}.json
```

**翻译文件格式（translations/zh.json）：**
```json
{
  "language": "zh",
  "language_name": "中文",
  "translated_at": "2026-05-21T10:30:00Z",
  "strings": [
    {
      "id": "str_001",
      "original": "Welcome to our app",
      "translated": "欢迎使用我们的应用"
    }
  ]
}
```

### Phase 5: 合并最终结果

将所有翻译合并为 `final.json`。

**final.json 格式（i18n 标准格式）：**
```json
{
  "version": "1.0.0",
  "source_language": "en",
  "target_languages": ["zh", "ja", "ko", "fr", "de", "es"],
  "generated_at": "2026-05-21T10:30:00Z",
  "source_root": "src/",
  "strings": [
    {
      "id": "str_001",
      "original": "Welcome to our app",
      "source": {
        "file": "src/pages/Home.tsx",
        "line": 24,
        "column": 12,
        "language": "typescript",
        "context": "<h1>Welcome to our app</h1>"
      },
      "translations": {
        "zh": "欢迎使用我们的应用",
        "ja": "私たちのアプリへようこそ",
        "ko": "우리 앱에 오신 것을 환영합니다",
        "fr": "Bienvenue dans notre application",
        "de": "Willkommen in unserer App",
        "es": "Bienvenido a nuestra aplicación"
      }
    }
  ],
  "statistics": {
    "total_strings": 120,
    "total_translations": 720,
    "languages": 6,
    "files_scanned": 42
  }
}
```

## 子智能体调度策略

采用**混合并行策略**：

```
Phase 1 (扫描)          → 主智能体串行
    ↓
Phase 2 (提取)          → 按语言分组，每组一个子智能体并行
    ↓
Phase 3 (过滤)          → 主智能体串行（需要全局判断）
    ↓
Phase 4 (翻译)          → 按目标语言分组，每种语言一个子智能体并行
    ↓
Phase 5 (合并)          → 主智能体串行
```

**调度要点：**
- Phase 2 和 Phase 4 是主要并行点
- 每个子智能体处理独立的文件组或语言，无共享状态
- 主智能体负责合并和协调

## 使用方式

用户触发时通常提供：
1. **源码范围**：目录路径或 glob 模式，如 `src/` 或 `src/**/*.ts`
2. **目标语言**：语言代码列表，如 `["zh", "ja", "ko"]`

如果用户未明确指定，使用 `question` 工具询问。

## 注意事项

- 大项目（>100 文件）务必使用子智能体并行
- 小项目（<20 文件）可以直接处理，无需子智能体
- 提取阶段宁可多提取，过滤阶段再精简
- 翻译时注意保持术语一致性，可以在翻译前先从 filtered.json 中提取专业术语表
- 如果源码中已有 i18n 框架（如 react-i18next, vue-i18n），识别并尊重已有的翻译键
