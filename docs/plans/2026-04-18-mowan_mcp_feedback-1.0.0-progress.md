# mowan_mcp_feedback-1.0.0 进度日志

## 2026-04-18

### 已完成

- 确认当前需求边界：
  - 只改名
  - 只增加皮肤切换
  - 不改其他业务逻辑
  - 保留原作者署名和许可证
- 读取并使用了 `brainstorming` 与 `planning-with-files` 工作方式。
- 检查了现有 `docs` 目录结构。
- 创建了以下计划文件：
  - `docs/plans/2026-04-18-mowan_mcp_feedback-1.0.0-task_plan.md`
  - `docs/plans/2026-04-18-mowan_mcp_feedback-1.0.0-findings.md`
  - `docs/plans/2026-04-18-mowan_mcp_feedback-1.0.0-progress.md`
- 完成第一轮代码改造：
  - 将项目名改为 `mowan-mcp-feedback`
  - 将版本改为 `1.0.0`
  - 将控制台脚本改为 `mowan-mcp-feedback`
  - 更新了部分用户可见名称与版本展示
  - 新增深色/浅色主题切换
  - 主题状态使用 `localStorage` 持久化
  - 仓库 URL 已切换到 `https://github.com/limowan/mowan-mcp-feedback`
  - 新增 `.cursor/mcp.json` 用于本地 MCP 加载
  - 更新 GitHub 发布工作流中的项目名、安装命令和仓库链接
- 完成第一轮本地验证：
  - `uv run python -m compileall src`
  - `uv run python -m mcp_feedback_enhanced version`
  - `uv run python -m mcp_feedback_enhanced test --web`
  - Playwright 快照验证主题按钮和主题选择框状态
  - `uv build`

### 当前判断

- 本次应优先完成“命名与发布策略”的基线确认，再开始改代码。
- UI 改造应尽量限定在前端模板层，避免引入后端风险。
- 当前第一轮改造已达到“本地可运行、主题可切换、包可构建”的状态。

### 下一步

- 整理 GitHub 建仓和同步步骤。
- 整理 PyPI 发布步骤。
- 把本地改动推到 `limowan/mowan-mcp-feedback`。
- 在 PyPI 中创建 `mowan-mcp-feedback` 并绑定 GitHub Trusted Publisher。
