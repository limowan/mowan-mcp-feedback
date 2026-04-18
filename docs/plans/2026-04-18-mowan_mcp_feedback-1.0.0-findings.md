# mowan_mcp_feedback-1.0.0 发现记录

## 2026-04-18

### 项目结构

- 仓库已有 `docs/architecture`、`docs/zh-CN`、`docs/zh-TW`、`docs/en`。
- 当前没有 `docs/plans` 目录，本次新建。

### 当前运行方式

- 当前 MCP 配置使用：
  - `command: uvx`
  - `args: ["gl-mcp-feedback"]`
- 这意味着当前实际运行的是 PyPI 上发布的包，或本机 `uv` 缓存中的该包版本。
- 不是直接运行当前本地源码目录。

### 当前代码入口

- 控制台脚本入口定义在 `pyproject.toml`。
- Python 主入口为 `mcp_feedback_enhanced.__main__:main`。
- Web UI 主要由以下文件承载：
  - `src/mcp_feedback_enhanced/web/templates/feedback.html`
  - `src/mcp_feedback_enhanced/web/templates/index.html`

### 主题相关观察

- 当前模板明显以深色主题为主。
- `feedback.html` 和 `index.html` 中存在大量内联 Tailwind 配置和颜色值。
- 当前适合采用“最小侵入双主题”方案：
  - 保留深色
  - 增加浅色
  - 尽量只动模板与前端状态持久化

### 发布相关观察

- 当前项目名为 `gl-mcp-feedback`。
- 用户提出的标识名为 `mowan_mcp_feedback-1.0.0`。
- 更适合发布到 PyPI 的正式项目名建议采用横线风格，例如 `mowan-mcp-feedback`。
- 版本号建议独立为 `1.0.0`，不要写进项目名字段。
- 当前已实际采用：
  - PyPI 项目名：`mowan-mcp-feedback`
  - 版本号：`1.0.0`
  - 控制台脚本名：`mowan-mcp-feedback`
  - GitHub 仓库：`https://github.com/limowan/mowan-mcp-feedback`

### 已完成的改造

- `pyproject.toml` 已改为新项目名和新版本号。
- `pyproject.toml` 中 `project.urls` 已切到你的 GitHub 仓库。
- 控制台脚本入口已从 `gl-mcp-feedback` 改为 `mowan-mcp-feedback`。
- Python 模块目录 `mcp_feedback_enhanced` 暂时保持不变，避免影响现有业务逻辑。
- `feedback.html` 已加入主题切换按钮和主题设置项。
- `index.html` 已加入等待页主题切换按钮。
- 主题持久化使用浏览器 `localStorage`，不涉及后端业务改造。
- 仓库内已创建 `.cursor/mcp.json`，用于 Cursor 直接加载本地源码版 MCP。
- `.github/workflows/publish.yml` 已更新为当前项目名和当前 GitHub 仓库链接。

### 本地验证结果

- `uv run python -m compileall src` 通过。
- `uv run python -m mcp_feedback_enhanced version` 输出正常，版本为 `1.0.0`。
- `uv run python -m mcp_feedback_enhanced test --web` 可以正常启动本地 Web UI。
- 页面主题按钮可以在深色和浅色之间切换。
- 设置页中的主题下拉框会跟随当前主题状态同步。
- 浏览器 `localStorage` 中保存键名：`mowan-mcp-feedback-theme`。
- `uv build` 成功生成以下产物：
  - `dist/mowan_mcp_feedback-1.0.0.tar.gz`
  - `dist/mowan_mcp_feedback-1.0.0-py3-none-any.whl`

### 当前残留事项

- 还没有真正把代码推到 `limowan/mowan-mcp-feedback`。
- 还没有在 PyPI 上创建项目并配置 Trusted Publisher。

### 署名与许可证

- 当前仓库含 `LICENSE` 文件。
- 本项目属于二次改造，应保留原许可证与来源说明。
