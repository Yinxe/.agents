# OpenCode 故障排除

## 日志

日志文件位置：
- **macOS/Linux**: `~/.local/share/opencode/log/`
- **Windows**: `%USERPROFILE%\.local\share\opencode\log`

日志文件按时间戳命名（如 `2025-01-09T123456.log`），保留最近 10 个。

设置日志级别：
```bash
opencode --log-level DEBUG
```

## 存储

OpenCode 存储位置：
- **macOS/Linux**: `~/.local/share/opencode/`
- **Windows**: `%USERPROFILE%\.local\share\opencode`

目录内容：
- `auth.json` - API 密钥、OAuth Token
- `log/` - 应用日志
- `project/` - 项目特定数据

## 桌面应用问题

### 快速检查

- 完全退出并重新启动
- 显示错误页面时点击**重新启动**并复制错误
- macOS：`OpenCode` 菜单 → **Reload Webview**

### 禁用插件

1. 检查全局配置 `~/.config/opencode/opencode.jsonc` 的 `plugin` 键
2. 临时设置为空数组 `[]`
3. 或临时移动插件目录

### 清除缓存

1. 退出 OpenCode Desktop
2. 删除缓存目录：
   - **macOS**: `~/.cache/opencode`
   - **Linux**: `~/.cache/opencode`
   - **Windows**: `%USERPROFILE%\.cache\opencode`

### 服务器连接问题

- 清除桌面默认服务器 URL
- 从配置中移除 `server.port` / `server.hostname`
- 检查 `OPENCODE_PORT` 环境变量

### Linux Wayland/X11

- 空白或崩溃：`OC_ALLOW_WAYLAND=1` 启动
- 情况恶化：在 X11 会话下启动

### Windows WebView2

安装或更新 Microsoft Edge **WebView2 Runtime**。

### Windows 性能问题

建议使用 [WSL](/docs/windows-wsl)。

### 通知不显示

仅在以下情况显示：
- 操作系统设置中为 OpenCode 启用通知
- 应用窗口未处于焦点状态

### 重置桌面应用存储

最后手段：

1. 退出应用
2. 删除以下文件：
   - `opencode.settings.dat`
   - `opencode.global.dat`
   - `opencode.workspace.*.dat`

位置：
- **macOS**: `~/Library/Application Support`
- **Linux**: `~/.local/share`
- **Windows**: `%APPDATA%`

## 常见问题

### OpenCode 无法启动

1. 检查日志中的错误消息
2. 使用 `--print-logs` 运行查看输出
3. 使用 `opencode upgrade` 确保最新版本

### 身份验证问题

1. TUI 中使用 `/connect` 重新认证
2. 检查 API 密钥是否有效
3. 确保网络允许连接提供商 API

### 模型不可用

1. 检查是否已通过提供商认证
2. 验证模型名称格式：`provider/model`
   - 正确：`openai/gpt-4.1`
   - 错误：`gpt-4.1`

3. 运行 `opencode models` 查看可用模型

### ProviderInitError

1. 验证提供商配置是否正确
2. 清除存储：
   ```bash
   rm -rf ~/.local/share/opencode
   ```
3. TUI 中使用 `/connect` 重新认证

### AI_APICallError

1. 清除提供商包缓存：
   ```bash
   rm -rf ~/.cache/opencode
   ```
2. 重新启动 OpenCode

### Linux 复制/粘贴

安装剪贴板工具：

**X11**:
```bash
apt install -y xclip
# 或
apt install -y xsel
```

**Wayland**:
```bash
apt install -y wl-clipboard
```

**无头环境**:
```bash
apt install -y xvfb
Xvfb :99 -screen 0 1024x768x24 > /dev/null 2>&1 &
export DISPLAY=:99.0
```

## 获取帮助

- **GitHub Issues**: https://github.com/anomalyco/opencode/issues
- **Discord**: https://opencode.ai/discord