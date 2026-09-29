# 使用指南

本目录收录了 13 个面向 AI 编程助手的 skill。每个子目录中的 `SKILL.md` 是对应 skill 的完整工作说明，`scripts/`、`references/`、`templates/` 和 `assets/` 等目录提供配套内容。Skill 提供工作流程指引；需要调用外部 CLI、浏览器或宿主功能时，还要完成下方列出的准备。

## Skill 一览

| Skill | 作用与适用场景 |
| --- | --- |
| [agent-browser](agent-browser/SKILL.md) | 通过浏览器自动化 CLI 操作网站和 Electron 应用，适合网页交互、表单、截图、端到端检查和界面探索。 |
| [ax](ax/SKILL.md) | 获取网页或文件内容、查看页面结构，并将列表和表格提取为结构化数据；适合网页查阅和轻量抓取，不适用于需要 JavaScript 渲染的 SPA。 |
| [diagnosing-bugs](diagnosing-bugs/SKILL.md) | 系统诊断难复现的缺陷和性能退化：建立能复现问题的反馈循环，再缩小范围、验证原因、修复并做回归检查。 |
| [diagram-design](diagram-design/SKILL.md) | 创建带有明确视觉层级的 HTML/SVG 图表，例如架构图、流程图、时序图、状态图、数据图和组织图；也可读取 Mermaid、draw.io、Excalidraw 图的结构。 |
| [domain-modeling](domain-modeling/SKILL.md) | 澄清项目中的领域术语、概念边界和关系，并把已确认的词汇和决策记录到 `CONTEXT.md`、ADR 等文档中。 |
| [find-docs](find-docs/SKILL.md) | 查询指定编程语言、框架、库、SDK 或 CLI 的最新官方文档，适合确认 API、配置、迁移和使用方法。 |
| [find-skills](find-skills/SKILL.md) | 在公开 skill 目录中搜索已有 skill、评估来源和适用性，并给出安装方式。 |
| [grilling](grilling/SKILL.md) | 通过分轮访谈压力测试计划、设计、决策或想法；适合实现前厘清约束和分支选择。 |
| [improve-codebase-architecture](improve-codebase-architecture/SKILL.md) | 扫描代码库的架构摩擦点，以可视化 HTML 报告展示模块改进候选，再深入讨论用户选中的方向。 |
| [karpathy-guidelines](karpathy-guidelines/SKILL.md) | 编码时强调先理解问题、保持改动精简、避免过度设计，并用可验证的标准检查结果。 |
| [planning-with-files](planning-with-files/SKILL.md) | 为多步骤实现和研究任务维护持久的计划、发现和进度文件，帮助跨阶段恢复上下文。 |
| [ponytail](ponytail/SKILL.md) | 在保证正确性的前提下寻找最简单、最短的实现，优先复用现有代码、标准库和平台能力。 |
| [product-decision-agent](product-decision-agent/SKILL.md) | 用中文分析互联网产品、运营、增长、商业化、数据和协作问题，识别关键阻塞并给出可执行的优先行动。 |

## 使用前的准备

### 通用准备

1. 确认所用的 AI 助手支持读取 skill，并已将需要的 `SKILL.md` 加入其 skill 搜索或加载路径。仅把文件放在本目录中，不保证每种 AI 助手都会自动加载它。
2. 需要联网的 skill 还要求当前环境能够访问目标网站或包仓库。不要把登录当成默认前提；只有对应服务明确要求或需要更高额度时再登录。

### 按 skill 准备工具

| Skill | 前置条件与设置 |
| --- | --- |
| `agent-browser` | 需要 Node.js/npm 来安装 CLI，并需要安装其自动化浏览器。执行 `npm i -g agent-browser`，然后执行 `agent-browser install` 下载 Chrome for Testing。首次运行 CLI 工作流前先执行 `agent-browser skills get core`，按需用 `agent-browser skills get <name>` 加载 Electron、Slack 等专项流程。 |
| `ax` | 需要先安装 `ax` 命令并确保它在 `PATH` 中。macOS/Linux 安装脚本见下方；也可按 [上游安装说明](https://github.com/yusukebe/ax#install) 使用 Nix。之后可用 `ax --help` 查看参数。 |
| `find-docs` | 使用 `npx ctx7@latest` 运行 Context7 CLI，因此需要 Node.js/npm、`npx` 和网络连接；无需全局安装。默认文档查询不要求登录。需要更高额度时，可选执行 `npx ctx7@latest login`。 |
| `find-skills` | 使用 `npx skills find` 搜索和 `npx skills add` 安装，因此需要 Node.js/npm、`npx` 和访问 skill 目录/GitHub 的网络连接；无需全局安装。 |
| `diagram-design` | 生成图表本身不要求额外 CLI。若要导入 Mermaid、draw.io、Excalidraw 文件或运行 SVG 导出、自检脚本，需要 Python 3；当前脚本使用 Python 标准库，不需要额外 `pip` 包。例如：`python3 scripts/self_check.py <diagram.html>`。 |
| `planning-with-files` | 辅助脚本需要 Python 3 和对应系统的命令环境：macOS/Linux 使用 shell 脚本，Windows 使用 PowerShell 脚本。自动上下文注入、生命周期 hooks 和斜杠命令是否可用，取决于 AI 助手及安装方式；纯手动维护计划文件不需要配置 hooks。 |
| `improve-codebase-architecture` | 该 skill 的流程会调用子代理，并引用本目录当前未收录的 `codebase-design` skill。要使用完整流程，宿主需要支持子代理和 skill 调用，并额外安装/加载 `codebase-design`；使用 Skills CLI 时可执行 `npx skills add mattpocock/skills@codebase-design`。生成并打开报告需要本地 HTML 文件查看器；报告里的 CDN 资源需要网络连接。 |
| `diagnosing-bugs` | 通常使用项目已有的测试、CLI 或服务即可。仅在最后的人工交互回路中使用 `scripts/hitl-loop.template.sh` 时，需要 Bash 环境。 |

`domain-modeling`、`grilling`、`karpathy-guidelines`、`ponytail` 和 `product-decision-agent` 没有额外的 CLI 安装要求；只需 AI 助手能加载对应 skill。是否需要项目文档或代码库，取决于当前任务。

### 安装 ax（macOS/Linux）

```bash
curl -fsSL https://ax.yusuke.run/install | sh
```

### 常用命令速查

```bash
# 浏览器自动化
npm i -g agent-browser
agent-browser install
agent-browser skills get core

# 网页读取与官方文档检索
ax https://example.com --outline
npx ctx7@latest library React "useEffect cleanup"

# 查找已有 skill
npx skills find react performance
```
