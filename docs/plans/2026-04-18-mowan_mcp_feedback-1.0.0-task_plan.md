# mowan_mcp_feedback-1.0.0 任务清单

## 背景

- 当前项目基于第三方开源项目 `gl-mcp-feedback` 进行二次改造。
- 本次改造目标很克制：
  - 只做项目改名，面向后续以自己的名字发布。
  - 只增加皮肤切换功能，新增一个浅色/白色皮肤。
  - 不改业务逻辑，不改现有反馈流程，不改接口协议，不改会话机制。
  - 必须保留原项目署名、许可证和必要来源说明。
- 未来目标：
  - 先在本地完成改造和测试。
  - 再发布到自己的 GitHub 仓库。
  - 再发布到 PyPI，方便在其他电脑通过 `uvx` 直接使用。

## 命名约定

- 当前用户给出的标识名：`mowan_mcp_feedback-1.0.0`
- 为了兼容 Python 打包规范，本任务先按下面方式执行：
  - 任务/版本标识：`mowan_mcp_feedback-1.0.0`
  - 计划中的 PyPI 项目名候选：`mowan-mcp-feedback`
  - 计划中的版本号：`1.0.0`
- 原因：
  - PyPI 的“项目名”和“版本号”是两个字段，通常不把版本号直接写进项目名。
  - 下划线、点、横线在 PyPI 名称归一化里容易混淆，正式发布时优先用横线名更稳。

## 范围锁定

- [x] 建立任务清单、发现记录、进度日志文件
- [x] 确认发布命名方案
- [x] 只修改品牌标识相关名称，不修改核心业务逻辑
- [x] 增加皮肤切换能力，保留现有深色皮肤
- [x] 新增浅色/白色皮肤
- [ ] 确保默认使用方式不被破坏
- [ ] 保留原作者署名、LICENSE、来源说明
- [x] 补充本地测试说明
- [ ] 补充 GitHub 发布准备说明
- [ ] 补充 PyPI 发布说明

## 实施阶段

### 阶段 1：发布前命名与边界确认

- [x] 统一项目发布名、命令名、仓库名
- [x] 确认哪些名字要改，哪些内部 Python 模块名暂时不改
- [x] 列出必须保留的原作者信息和许可证文件
- [ ] 确认 README 中哪些安装命令、示例配置要一起改

交付结果：
- 一个稳定的命名方案，保证未来本地运行、GitHub 仓库、PyPI 包名、MCP 配置名互相不打架。

### 阶段 2：皮肤切换设计

- [x] 梳理当前深色样式入口文件
- [x] 设计“深色 + 浅色”双主题最小改造方案
- [x] 只改前端模板和样式，不动 Python 业务逻辑
- [x] 明确主题切换的持久化方式
- [x] 确保等待页和主反馈页都支持浅色皮肤

交付结果：
- 一套最小侵入的主题切换方案。

### 阶段 3：本地开发与验证

- [x] 在本地以源码方式运行项目
- [x] 本地打开 Web UI 检查深浅主题切换
- [x] 检查原有反馈输入、提交、WebSocket 连接是否正常
- [x] 检查 MCP 调用方式是否仍可用
- [x] 记录本地测试步骤和预期现象

交付结果：
- 可重复执行的本地测试流程。

### 阶段 4：面向发布的整理

- [ ] 修改 `pyproject.toml` 中的项目名、脚本名、仓库信息
- [ ] 检查版本号、README、包描述
- [ ] 补充发布前检查项
- [ ] 设计 GitHub 仓库同步步骤
- [ ] 设计 PyPI 发布步骤

交付结果：
- 一个可以对外发布的包结构。

### 阶段 5：发布与跨电脑使用

- [ ] 推送到自己的 GitHub 仓库
- [ ] 在 GitHub 上保留来源说明
- [ ] 先验证 TestPyPI 或正式 PyPI 路径
- [ ] 发布到 PyPI
- [ ] 在另一台电脑上通过 `uvx <包名>` 验证安装和运行
- [ ] 给出最终 MCP 配置示例

交付结果：
- 其他电脑可以直接安装和使用。

## 本次改造的明确限制

- 不做 UI 大改版。
- 不重构后端架构。
- 不改 WebSocket 协议。
- 不改反馈数据结构。
- 不改会话管理逻辑。
- 不改图片处理逻辑。
- 不改原功能行为，除非是为了兼容主题切换和发布改名。

## 本地测试目标

- 主题切换按钮存在且可用。
- 深色皮肤保持现状可用。
- 浅色皮肤可读性正常，不刺眼，不发灰。
- 页面刷新后主题状态符合设计。
- `interactive_feedback` 工具仍能正常拉起页面。

## 本地测试步骤

### 1. 安装依赖

```bash
uv sync
```

### 2. 检查 Python 入口是否正常

```bash
uv run python -m mcp_feedback_enhanced version
```

预期：
- 能输出 `Mowan MCP Feedback v1.0.0`

### 3. 本地启动 Web UI 测试页

```bash
uv run python -m mcp_feedback_enhanced test --web
```

预期：
- 本地启动 Web 服务
- 自动打开浏览器
- 页面能看到主题切换按钮
- 可以在深色 / 浅色之间切换

### 4. 本地验证 MCP 包能否构建

```bash
uv build
```

预期：
- 生成：
  - `dist/mowan_mcp_feedback-1.0.0.tar.gz`
  - `dist/mowan_mcp_feedback-1.0.0-py3-none-any.whl`

### 5. 本地以 MCP 方式接入 Cursor

本地开发阶段建议先不要用 `uvx` 拉 PyPI，而是直接指向当前源码目录。

示例配置：

```json
{
  "mcpServers": {
    "mowan-feedback-local": {
      "command": "uv",
      "args": [
        "run",
        "--directory",
        "C:\\AI编程项目\\Cursor对话专用MCP\\gl_mcp_feedback-2.1.0",
        "python",
        "-m",
        "mcp_feedback_enhanced"
      ],
      "timeout": 14400,
      "env": {
        "MCP_DEBUG": "false",
        "http_proxy": "",
        "https_proxy": "",
        "all_proxy": ""
      },
      "autoApprove": ["interactive_feedback"]
    }
  }
}
```

预期：
- Cursor 直接跑你本地改过的代码
- 这样最适合本地调试

## GitHub 同步步骤

### 1. 新建你自己的 GitHub 仓库

建议仓库名：

```text
mowan-mcp-feedback
```

### 2. 在本地初始化或接入 Git

如果当前目录还不是你自己的 Git 仓库，常规步骤如下：

```bash
git init
git add .
git commit -m "feat: rename to mowan-mcp-feedback and add light theme"
```

### 3. 绑定你自己的远程仓库

```bash
git remote add origin <your-github-repo-url>
git branch -M main
git push -u origin main
```

### 4. 推送前必须检查

- `pyproject.toml` 中项目名和版本正确
- `README.md` 的仓库链接已改成你的 GitHub 地址
- `project.urls` 已改成你的 GitHub 地址
- `LICENSE` 保留
- README 中保留上游来源说明

## PyPI 发布步骤

### 方案选择

建议用：

- GitHub 仓库作为源码真源
- PyPI Trusted Publishing 作为发布方式

原因：
- 比手动 token 更稳
- 后续发新版更方便

### 1. 先创建 PyPI 账号

- 注册 PyPI
- 登录后准备创建项目发布

### 2. 先在本地确认构建通过

```bash
uv build
```

### 3. 推荐先发 TestPyPI

如果只是先试发，建议先测 TestPyPI，确认包名、README、安装都正常，再发正式 PyPI。

### 4. 本地手动发布方式

如果你先手动发布，可以使用：

```bash
uv publish
```

说明：
- 需要先准备 PyPI token
- 或后续改成 GitHub Actions Trusted Publishing

### 5. 正式推荐方式：GitHub Actions 自动发布

建议流程：

1. 在 GitHub 上推送 tag 或 release
2. GitHub Actions 负责执行：
   - `uv build`
   - `uv publish`
3. PyPI 使用 Trusted Publisher 识别 GitHub 工作流身份

### 6. 发布后在其他电脑使用

MCP 配置改成：

```json
{
  "mcpServers": {
    "mowan-feedback": {
      "command": "uvx",
      "args": ["mowan-mcp-feedback"],
      "timeout": 14400,
      "env": {
        "MCP_DEBUG": "false",
        "http_proxy": "",
        "https_proxy": "",
        "all_proxy": ""
      },
      "autoApprove": ["interactive_feedback"]
    }
  }
}
```

预期：
- 其他电脑无需手动拷贝源码
- 直接通过 PyPI 安装并运行

## 发布目标

- GitHub：作为源码仓库与后续同步源。
- PyPI：作为分发渠道，方便其他电脑 `uvx` 运行。
- MCP 使用目标：最终形态应支持类似下面的配置。

```json
{
  "mcpServers": {
    "mowan-feedback": {
      "command": "uvx",
      "args": ["mowan-mcp-feedback"],
      "timeout": 14400,
      "autoApprove": ["interactive_feedback"]
    }
  }
}
```

## 风险与注意事项

- 发布名不能和 PyPI 上已有项目重名。
- PyPI 项目名和版本号是分开的，不能把版本策略做进名字里。
- 这是二次改造项目，README 和 LICENSE 里必须保留原作者来源信息。
- 主题切换最好做成前端层改造，避免把风险带到 Python 业务层。

## 当前状态

- 当前阶段：阶段 4 准备中
- 下一步：整理 GitHub 发布步骤和 PyPI 发布步骤
