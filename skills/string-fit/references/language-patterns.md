# 各语言字符串提取指南

本文件提供各编程语言中字符串字面量的识别模式，帮助子智能体准确提取。

## TypeScript / JavaScript

### 字符串形式
```typescript
"double quoted string"
'single quoted string'
`template literal ${variable}`
```

### UI 字符串常见位置
- JSX 文本节点：`<p>Hello World</p>`
- JSX 属性：`<Button label="Submit" />`
- 组件 props：`title="Dashboard"`
- 对象属性值：`{ label: 'Settings', value: 'settings' }`（label 是 UI，value 可能是键）
- 数组项：`['Home', 'About', 'Contact']`
- 函数参数：`toast.success('Saved successfully')`
- 模板字符串中的静态部分：`` `Welcome, ${name}!` `` → 提取 "Welcome, " 和 "!"

### 排除模式
- `console.log("debug info")` — 日志
- `logger.error("something failed")` — 日志
- `import "styles.css"` — import 语句
- `require("./module")` — 模块路径
- `process.env.NODE_ENV` — 环境变量
- 正则表达式：`/pattern/g`
- CSS-in-JS 样式值：`` styled.div`color: red` ``

## Python

### 字符串形式
```python
"double quoted"
'single quoted'
"""triple double quoted"""
'''triple single quoted'''
f"f-string {variable}"
```

### UI 字符串常见位置
- Django/Flask 模板中的文本
- 表单字段标签：`label="Email Address"`
- 按钮文字：`text="Submit"`
- 消息提示：`messages.success('Profile updated')`
- 序列化器字段：`help_text="Enter your email"`
- 管理后台：`verbose_name="User Profile"`

### 排除模式
- `logging.info("...")` — 日志
- `print("debug...")` — 调试输出
- 文件路径：`open("/path/to/file")`
- SQL 查询：`cursor.execute("SELECT ...")`
- 正则表达式：`re.compile(r"pattern")`
- 装饰器参数中的技术字符串

## Java

### 字符串形式
```java
"double quoted string"
```

### UI 字符串常见位置
- Android `TextView` 文本：`textView.setText("Hello")`
- 按钮文字：`button.setText("Click Me")`
- 对话框标题/内容：`setTitle("Confirm")`
- Toast 消息：`Toast.makeText(context, "Saved", ...)`
- 菜单项：`menu.add("Settings")`
- 表单验证消息

### 排除模式
- `System.out.println("...")` — 日志/调试
- `log.info("...")` — 日志
- 类名/方法名字符串反射
- 文件路径
- SQL 语句
- 注解中的技术字符串：`@RequestMapping("/api/users")`

## Go

### 字符串形式
```go
"double quoted"
`backtick raw string`
```

### UI 字符串常见位置
- HTTP 响应消息
- 模板文本
- CLI 帮助文本
- 表单验证错误
- 用户提示消息

### 排除模式
- `fmt.Println("...")` — 日志/调试
- `log.Printf("...")` — 日志
- 文件路径
- URL 路径
- SQL 查询
- JSON tag：``json:"field_name"``

## Rust

### 字符串形式
```rust
"double quoted"
r#"raw string"#
```

### UI 字符串常见位置
- Web 框架模板文本
- CLI 帮助/错误消息
- 用户提示
- 表单验证

### 排除模式
- `println!("...")` — 日志/调试
- `log::info!("...")` — 日志
- 文件路径
- SQL 查询
- 属性宏中的技术字符串

## PHP

### 字符串形式
```php
"double quoted"
'single quoted'
"interpolated $variable"
```

### UI 字符串常见位置
- HTML 模板中的文本
- 表单标签
- 按钮文字
- 验证消息
- Flash 消息

### 排除模式
- `echo "debug..."` — 调试
- `error_log("...")` — 日志
- 文件路径
- SQL 查询
- 数组键名

## Ruby

### 字符串形式
```ruby
"double quoted"
'single quoted'
%Q{percent quoted}
%q{percent single quoted}
"interpolated #{variable}"
```

### UI 字符串常见位置
- ERB 模板文本
- 表单标签
- Flash 消息
- 验证错误
- 按钮/链接文字

### 排除模式
- `puts "debug..."` — 调试
- `Rails.logger.info("...")` — 日志
- 文件路径
- SQL 查询
- Symbol：`:symbol_name`

## Swift

### 字符串形式
```swift
"double quoted"
```

### UI 字符串常见位置
- `Text("Hello")` — SwiftUI
- `UILabel` 文本
- `UIAlertController` 标题/消息
- 按钮标题
- 导航栏标题

### 排除模式
- `print("...")` — 日志/调试
- 文件路径
- 键路径字符串
- 技术标识符

## Kotlin

### 字符串形式
```kotlin
"double quoted"
"""multi-line"""
```

### UI 字符串常见位置
- `Text("Hello")` — Jetpack Compose
- `textView.text = "Hello"`
- 对话框标题/消息
- Toast 消息
- 菜单项

### 排除模式
- `println("...")` — 日志/调试
- `Log.d("...", "...")` — 日志
- 文件路径
- SQL 查询
- 注解中的技术字符串

## C#

### 字符串形式
```csharp
"double quoted"
@"verbatim string"
$"interpolated {variable}"
```

### UI 字符串常见位置
- XAML 文本
- 按钮文字
- 标签文字
- 消息框
- 表单验证

### 排除模式
- `Console.WriteLine("...")` — 日志/调试
- `Debug.Log("...")` — 日志
- 文件路径
- SQL 查询
- 特性参数中的技术字符串

## HTML / 模板文件

### 提取目标
- 标签内文本：`<p>Hello World</p>`
- 属性值：`title="Tooltip text"`, `placeholder="Enter name"`, `alt="Image description"`
- `aria-label` 值
- `data-*` 属性中的用户可见文本

### 排除
- `class="..."` — CSS 类名
- `id="..."` — HTML ID
- `href="..."` — 链接地址
- `src="..."` — 资源路径
- `style="..."` — 内联样式
- `<script>` 和 `<style>` 标签内容

## Vue SFC (.vue)

### 提取目标
- `<template>` 中的文本节点
- 组件属性中的 UI 字符串
- `v-bind:title`, `v-bind:placeholder` 等绑定的字符串值

### 排除
- `<script>` 中的日志/调试
- CSS 样式
- 技术配置

## 通用排除规则（适用于所有语言）

以下模式在任何语言中都应排除：

1. **日志/调试**：任何日志框架调用中的字符串
2. **文件/路径**：看起来像文件路径的字符串
3. **URL**：看起来像 URL 的字符串
4. **SQL**：SQL 查询语句
5. **正则**：正则表达式模式
6. **CSS**：CSS 选择器或样式值
7. **技术键名**：看起来像配置键、环境变量名的字符串
8. **纯数字**：只包含数字的字符串
9. **空字符串**：`""`
10. **单字符**：除非是 UI 符号（如 "×", "→", "✓"）
