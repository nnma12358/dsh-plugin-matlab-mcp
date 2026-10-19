# dsh-plugin-matlab-mcp（桌面版）

把 MATLAB MCP Server 注册成 bundle 层，使 `mcp__matlab__*` 工具出现在 dsh desktop 里，
并且会出现在「已安装插件」列表、可整体开关。

## 与 web 版的差异

| 项 | web 版 | 桌面版（本包） |
|---|---|---|
| `command` | `/mnt/c/.../matlab-mcp-server.exe`（WSL interop） | `C:\Users\<用户名>\.matlab\...\matlab-mcp-server.exe`（原生） |
| `cwd` | `/mnt/c/.../agentic-toolkits` | `C:\Users\<用户名>\.matlab\agentic-toolkits` |
| `env` | 不设（interop 继承 Windows 环境） | 显式补 `SystemRoot` / `WINDIR` |

其余参数（MATLAB 根目录、extension-file、工程目录、日志目录、超时）与 web 一致。
关掉本插件即卸载全部 `mcp__matlab__*` 工具。

> **安装前必读**：`cordis.patch.yml` 中的 `C:\Users\<用户名>` 是占位符，
> 请替换为你的实际路径（MATLAB MCP Server 可执行文件、extension-file、日志目录）。

## 安装

桌面端 profile 由 Electron 应用独占管理（`dsh plugin --profile desktop …` 会被官方拒绝）。二选一：
应用内「设置 → 插件 → 添加本地插件」；或把目录放进
`%USERPROFILE%\.dsh\profiles\desktop\plugins\`，在该 profile 的 `package.json` 加 `link:` 依赖、
包名加入 `dsh.profile.bundles`，再用自带 pnpm 装依赖。装完重启 dsh desktop。

纯源码包，无构建步骤：`main` / `exports` 指向 `lib/`，`dsh.bundle.patch` 指向 `cordis.patch.yml`。
