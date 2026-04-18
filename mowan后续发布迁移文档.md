# mowan 后续发布迁移文档

## 1. 现在这两个问题有什么影响

### 1. README / PyPI 里还有少量旧文案

影响不大，不会影响安装、运行、发布。

它的实际影响主要是：

- 别人看 PyPI 页面时，可能会看到一两句过时说明，容易有点困惑
- 对外展示不够干净，像是“改完了但文案没收尾”

大白话就是：

功能没坏，面子上还差一点收口。

### 2. GitHub Actions 里的 Node 20 弃用提示

这个现在也**不影响当前发布**，因为 `1.0.0` 已经发成功了。

但它是一个后面要处理的提醒：

- GitHub Actions 从 **2026年6月2日** 起会默认切到 Node 24
- Node 20 会在 **2026年9月16日** 从 runner 里移除

如果相关 Action 到时候还没升级，未来某次发布可能会出问题。

大白话就是：

现在能用，不是立刻爆炸的问题，但后面最好补一下版本升级。

## 2. 这次迁移的核心结论

你后面准备把本地文件夹从：

```text
C:\AI编程项目\Cursor对话专用MCP\gl_mcp_feedback-2.1.0
```

改成：

```text
C:\AI编程项目\Cursor对话专用MCP\mowan-mcp-feedback
```

这个改名本身**不会影响 PyPI 上已经发布的包名**，因为 PyPI 认的是：

- 项目名：`mowan-mcp-feedback`
- 版本号：`1.0.0`

本地目录名改掉以后，真正会受影响的主要是两类东西：

1. 本地源码版 MCP 配置里的绝对路径
2. 你自己平时本地开发时用到的目录路径

## 3. 改名后，MCP 怎么配置

### 3.1 Cursor 里加载本地源码版 MCP

你项目里的配置文件是：

```text
.cursor/mcp.json
```

改目录名以后，这里的 `--directory` 路径要一起改。

建议改成下面这样：

```json
{
  "mcpServers": {
    "mowan-feedback-local": {
      "command": "uv",
      "args": [
        "run",
        "--directory",
        "C:\\AI编程项目\\Cursor对话专用MCP\\mowan-mcp-feedback",
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
      "autoApprove": [
        "interactive_feedback"
      ]
    }
  }
}
```

注意：

- 这里跑的是**你本地源码**
- 不是 PyPI
- 所以你改了界面、改了代码，Cursor 这里会直接吃到最新本地版本

### 3.2 Codex 里加载本地源码版 MCP

你本机 Codex 的全局配置文件是：

```text
C:\Users\Limmer\.codex\config.toml
```

里面现在也有一条本地 MCP，目录改名后也要一起改：

```toml
[mcp_servers.mowan-feedback-local]
type = "stdio"
command = "uv"
args = ["run", "--directory", "C:\\AI编程项目\\Cursor对话专用MCP\\mowan-mcp-feedback", "python", "-m", "mcp_feedback_enhanced"]
enabled = true
```

如果改完后新开 Codex 会话没看到这个 MCP，通常做这两个动作就行：

1. 关掉当前会话，重新开一个新会话
2. 或者重启 Codex

### 3.3 以后别的电脑怎么配

如果你想让别的电脑直接用已发布版本，不跑本地源码，最简单就是：

```json
{
  "mcpServers": {
    "mowan-feedback": {
      "command": "uvx",
      "args": [
        "mowan-mcp-feedback"
      ],
      "timeout": 14400,
      "autoApprove": [
        "interactive_feedback"
      ]
    }
  }
}
```

这个方式的特点是：

- 简单
- 不依赖本地源码路径
- 适合别的电脑直接装好就用

## 4. 本地怎么测试这个 MCP

进入项目根目录后，先同步依赖：

```powershell
uv sync
```

然后按下面顺序测：

### 4.1 看版本是否正常

```powershell
uv run python -m mcp_feedback_enhanced version
```

如果输出版本号正常，说明入口没坏。

### 4.2 看 Web UI 能不能正常启动

```powershell
uv run python -m mcp_feedback_enhanced test --web
```

这一步主要检查：

- 页面能不能打开
- 深色 / 浅色切换是否正常
- 反馈页面是否还能正常工作

### 4.3 在 Cursor 里走一次真实 MCP

把 `.cursor/mcp.json` 的路径改对后：

1. 重载 Cursor
2. 确认 MCP 列表里有 `mowan-feedback-local`
3. 实际调用一次 `interactive_feedback`
4. 看浏览器页面能不能正常弹出、提交、回传

## 5. 以后版本怎么更新

以后如果你继续改这个项目，最小更新步骤建议按这个来。

### 5.1 先改代码

你这类改动，优先只动必要内容：

- 前端样式
- 文案
- 发布说明
- 配置

如果不是必须，先不要乱动业务逻辑。

### 5.2 改版本号

至少改这两个地方：

1. `pyproject.toml`
2. `src/mcp_feedback_enhanced/__init__.py`

比如从 `1.0.0` 改成 `1.0.1`。

注意：

GitHub Actions 的发布工作流会检查：

- Git tag 版本
- `pyproject.toml` 里的版本

这两个必须一致，不一致会直接发布失败。

### 5.3 本地先测

建议每次发版前至少跑这几个：

```powershell
uv run python -m compileall src
uv run python -m mcp_feedback_enhanced version
uv run python -m mcp_feedback_enhanced test --web
uv build
uvx twine check dist/*
```

大白话就是：

- 先确认代码能跑
- 再确认包能打出来
- 再确认包描述没坏

## 6. 以后怎么发布到 GitHub 和 PyPI

你现在已经配好了：

- GitHub 仓库：`https://github.com/limowan/mowan-mcp-feedback`
- PyPI Trusted Publisher
- GitHub Actions 自动发布

所以你以后发布会很简单。

### 6.1 正常发布流程

先提交代码：

```powershell
git add .
git commit -m "feat: 这里写本次更新内容"
git push origin main
```

然后打版本 tag：

```powershell
git tag v1.0.1
git push origin v1.0.1
```

推送 tag 后，GitHub Actions 会自动做这些事：

1. 检查 tag 版本和 `pyproject.toml` 版本是否一致
2. 自动构建包
3. 自动上传到 PyPI
4. 自动创建 GitHub Release

### 6.2 发布完后去哪里看

发布后建议检查这三个地方：

1. GitHub Actions
2. GitHub Releases
3. PyPI 项目页

链接：

- GitHub Actions: `https://github.com/limowan/mowan-mcp-feedback/actions`
- GitHub Releases: `https://github.com/limowan/mowan-mcp-feedback/releases`
- PyPI: `https://pypi.org/project/mowan-mcp-feedback/`

## 7. 常见坑

### 7.1 本地目录改名了，但 MCP 还是老路径

这是最常见的问题。

现象通常是：

- Cursor 里 MCP 起不来
- Codex 里本地 MCP 消失
- 报找不到目录或模块

原因很简单：

绝对路径还指着旧目录。

优先检查：

- `.cursor/mcp.json`
- `C:\Users\Limmer\.codex\config.toml`

### 7.2 本地目录名改了，不等于 Python 包内部模块名也要改

目前项目对外名字是：

- PyPI 项目名：`mowan-mcp-feedback`
- 命令名：`mowan-mcp-feedback`

但内部 Python 模块还是：

```text
mcp_feedback_enhanced
```

这个现在是正常的，不影响使用。

所以你现在**不需要**因为改本地文件夹名，就顺手把内部包目录也改掉。

不然改动面会变大，出错概率会明显上升。

### 7.3 上游署名和许可证不要删

这个项目是基于上游改造的。

所以后续继续发版时，建议继续保留：

- 上游来源说明
- MIT 许可证
- 原作者署名信息

这既是合规问题，也是整理项目来源最省心的做法。

## 8. 你现在最推荐的实际做法

按顺序做就行：

1. 把本地文件夹改名为 `mowan-mcp-feedback`
2. 改 `.cursor/mcp.json` 里的本地路径
3. 改 `C:\Users\Limmer\.codex\config.toml` 里的本地路径
4. 重新打开 Cursor / Codex 测一下本地 MCP
5. 后面每次改完代码，先本地测，再打 tag 发布

如果你想要最省心的原则，就记一句：

**本地开发看路径，正式发布看版本号和 tag。**
