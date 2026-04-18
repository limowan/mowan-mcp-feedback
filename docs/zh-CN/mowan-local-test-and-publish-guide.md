# mowan-mcp-feedback 傻瓜教程

这份教程只做三件事：

1. 在本地把 MCP 跑起来
2. 在 Cursor / Codex 里加载它测试
3. 推到 GitHub，再发布到 PyPI

---

## 一、你现在已经有什么

当前项目已经完成了这些准备：

- 项目名：`mowan-mcp-feedback`
- 版本号：`1.0.0`
- 本地源码版 MCP 配置文件：
  - [`.cursor/mcp.json`](</C:/AI编程项目/Cursor对话专用MCP/gl_mcp_feedback-2.1.0/.cursor/mcp.json>)
- GitHub 仓库：
  - [limowan/mowan-mcp-feedback](https://github.com/limowan/mowan-mcp-feedback)
- GitHub 自动发布工作流：
  - [`.github/workflows/publish.yml`](</C:/AI编程项目/Cursor对话专用MCP/gl_mcp_feedback-2.1.0/.github/workflows/publish.yml>)

---

## 二、本地先跑起来

### 第 1 步：安装依赖

在项目根目录执行：

```bash
uv sync
```

### 第 2 步：检查版本命令

```bash
uv run python -m mcp_feedback_enhanced version
```

看到下面这种输出就说明入口正常：

```text
Mowan MCP Feedback v1.0.0
作者: 鼬君夏纪
上游来源: https://github.com/Minidoracat/mcp-feedback-enhanced
```

### 第 3 步：直接打开 Web UI 测试页

```bash
uv run python -m mcp_feedback_enhanced test --web
```

你应该能看到：

- 自动打开浏览器
- 页面右上角有主题切换按钮
- 可以切到浅色皮肤
- 刷新后主题状态会保留

---

## 三、在 Cursor 里加载本地 MCP

### 方式 A：直接用项目里的配置

项目里已经放好了：

- [`.cursor/mcp.json`](</C:/AI编程项目/Cursor对话专用MCP/gl_mcp_feedback-2.1.0/.cursor/mcp.json>)

内容是：

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

### 第 1 步

用 Cursor 打开这个项目目录。

### 第 2 步

让 Cursor 读取项目里的 `.cursor/mcp.json`。

如果没自动识别，就手动把上面的 JSON 配到 Cursor 的 MCP 设置里。

### 第 3 步

重载窗口或重启 Cursor。

### 第 4 步

确认 MCP 列表里出现：

```text
mowan-feedback-local
```

### 第 5 步

在 Cursor 里触发这个 MCP，验证：

- 能拉起反馈页面
- 浅色主题能切换
- 原来的反馈逻辑没坏

---

## 四、在 Codex 里加载本地 MCP

Codex 全局配置文件在：

- [`C:\Users\Limmer\.codex\config.toml`](</C:/Users/Limmer/.codex/config.toml>)

我已经帮你加了一段：

```toml
[mcp_servers.mowan-feedback-local]
type = "stdio"
command = "uv"
args = ["run", "--directory", "C:\\AI编程项目\\Cursor对话专用MCP\\gl_mcp_feedback-2.1.0", "python", "-m", "mcp_feedback_enhanced"]
enabled = true
```

### 你现在要做什么

1. 关闭当前 Codex 会话或新开一个新会话
2. 重新进入这个项目
3. 看新会话里是否加载到 `mowan-feedback-local`

### 注意

- 这类全局 MCP 配置通常不是“当前对话立刻热加载”
- 最稳的做法是：开一个新线程，或者重启 Codex Desktop

---

## 五、把代码推到 GitHub

### 第 1 步：检查远程仓库

```bash
git remote -v
```

如果还没绑你的仓库：

```bash
git remote add origin https://github.com/limowan/mowan-mcp-feedback.git
```

### 第 2 步：提交代码

```bash
git add .
git commit -m "feat: rename to mowan-mcp-feedback and add light theme"
```

### 第 3 步：推送

```bash
git branch -M main
git push -u origin main
```

---

## 六、发布到 PyPI

推荐方式：**GitHub 自动发布到 PyPI**

也就是：

- 代码推 GitHub
- 打 tag
- GitHub Actions 自动发 PyPI

### 第 1 步：先去 PyPI 注册账号

如果没有账号，先注册：

- [PyPI](https://pypi.org/)

### 第 2 步：在 PyPI 创建项目

项目名填：

```text
mowan-mcp-feedback
```

如果这个名字没被占用，就可以继续。

### 第 3 步：配置 Trusted Publisher

在 PyPI 里把 GitHub 仓库绑定到这个项目。

核心信息：

- GitHub owner: `limowan`
- GitHub repo: `mowan-mcp-feedback`
- workflow 文件：`.github/workflows/publish.yml`
- environment：可选，没有也能先跑

### 第 4 步：本地确认构建没问题

```bash
uv build
```

### 第 5 步：打 tag 并推送

```bash
git tag v1.0.0
git push origin v1.0.0
```

### 第 6 步：GitHub Actions 自动发布

仓库里的工作流会自动：

1. 检查 tag 版本和 `pyproject.toml` 版本是否一致
2. 执行 `uv build`
3. 检查包
4. 发布到 PyPI
5. 创建 GitHub Release

---

## 七、别的电脑怎么用

等 PyPI 发布成功后，其他电脑里的 MCP 配置就可以直接写成：

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

这样就不需要拷源码了。

---

## 八、出问题先查什么

### 1. MCP 没加载出来

先重启 Cursor / Codex。

### 2. 本地命令跑不起来

先试：

```bash
uv run python -m mcp_feedback_enhanced version
```

如果这一步都不通，说明是环境问题，不是 MCP 配置问题。

### 3. 页面起不来

先试：

```bash
uv run python -m mcp_feedback_enhanced test --web
```

### 4. 发布失败

先查：

- PyPI 项目名是不是已经被占用
- Trusted Publisher 有没有绑对仓库和 workflow
- tag 版本和 `pyproject.toml` 版本是否一致

---

## 九、你现在最该做的顺序

1. 先在 Cursor 里加载本地 MCP 测一遍
2. 再在 Codex 新开会话测一遍
3. 没问题后把代码推到 GitHub
4. 在 PyPI 绑定 Trusted Publisher
5. 打 `v1.0.0` tag 发布
