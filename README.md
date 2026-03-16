# A-Stockit

<div align="center">

![Python 3.11+](https://img.shields.io/badge/Python-3.11%2B-3776AB?logo=python&logoColor=white)
![Bundle First](https://img.shields.io/badge/Distribution-Bundle--First-0F766E)
![China A-Share](https://img.shields.io/badge/Market-China%20A--Share-C62828)
![License](https://img.shields.io/badge/License-MIT-green)

**面向 AI Agent 框架的 Bundle-first A 股研究与决策技能库**

**Bundle-first A-share research and decision skill library for AI Agent frameworks**

[中文](#中文) | [English](#english)

</div>

---

## 中文

### A-Stockit 解决什么问题？

A-Stockit 面向更广泛的 A 股用户，而不只是专业量化团队。

很多现有 Agent 都能聊天、写代码、调用工具，但在 A 股场景下仍然缺少一层明确的能力组织：

- 通用 Agent 不知道如何把 `自选股`、`行情快照`、`技术解读`、`决策建议`、`复盘` 串成一个稳定工作流
- A 股数据、新闻、基本面、执行计划和历史产物经常分散在不同脚本、笔记和聊天上下文中
- 用户需要的往往不是“自动交易机器人”，而是一个能被现有 Agent 可靠调用、可复查、可复用的 A 股能力包

A-Stockit 的做法是把这些能力整理成一个 `bundle-first`、`definition-first` 的技能包，让 Agent 明确知道应该调用什么技能、会生成什么产物、哪些能力已经代码化、哪些仍然只是稳定的工作流定义。

### 目标用户

- 关注 A 股市场、希望用 Agent 辅助观察和复盘的普通用户
- 需要自选股、筛选、简报和决策判断的活跃交易者或独立研究者
- 需要接入 A 股能力的 Agent 框架开发者
- 希望把研究流程做成可复用产物的进阶用户，包括量化研究者

### A-Stockit 的定位

A-Stockit **不是**独立 App，也不是一键全自动交易系统。

它是一个面向主流 Agent 框架的 A 股技能包：

- `skills/astockit/<skill>/SKILL.md` 是公开技能定义
- 对于已经代码化的能力，技能目录会附带本地 `run.py`
- 共享运行时、市场逻辑、研究模块和持久化能力统一放在 `_src/`
- 运行产物统一落在 `_artifacts/`，便于后续复盘、回测和历史检索

### 安装

本章只说明如何把 A-Stockit 集成到现有 Agent 中，不重复介绍这些 Agent 本身的安装过程。

先获取本仓库：

```bash
git clone https://github.com/yourusername/A-Stockit.git
cd A-Stockit
```

说明：

- 如果 Agent 只读取 `SKILL.md` 技能定义，通常不需要先把本仓库通过 `pip` 安装到环境里
- 如果你后续要运行带 `run.py` 的 code-backed 技能，再按需安装 `pandas`、`akshare`、`lark-oapi` 等依赖即可
- 当前 `skills/astockit/<skill>/run.py` 会直接把 `skills/astockit/_src` 加入 `sys.path`

#### 1. 集成到 OpenClaw 🦞

OpenClaw 当前会从 `<workspace>/skills` 和 `~/.openclaw/skills` 加载技能目录，工作区目录优先级更高。

项目级：

```bash
cd /path/to/your-project
mkdir -p skills
ln -s /path/to/A-Stockit/skills/astockit skills/astockit
```

用户级：

```bash
mkdir -p ~/.openclaw/skills
ln -s /path/to/A-Stockit/skills/astockit ~/.openclaw/skills/astockit
```

如果你使用的是共享工作区，优先推荐项目级路径；如果你希望多项目复用，再选择用户级路径。

#### 2. 集成到 Codex

Codex 当前会从仓库或目录树中的 `.agents/skills` 以及用户级 `$HOME/.agents/skills` 读取技能定义。

项目级：

```bash
cd /path/to/your-project
mkdir -p .agents/skills
ln -s /path/to/A-Stockit/skills/astockit .agents/skills/astockit
```

用户级：

```bash
mkdir -p ~/.agents/skills
ln -s /path/to/A-Stockit/skills/astockit ~/.agents/skills/astockit
```

如果你的仓库已经有上层 `.agents/skills` 目录，也可以把 `astockit` 放在更高一层，让同一仓库下的多个子目录共享这组技能。

#### 3. 集成到 Claude Code

Claude Code 当前会从项目级 `.claude/skills/<skill-name>/SKILL.md` 和用户级 `~/.claude/skills/<skill-name>/SKILL.md` 读取技能定义。

项目级：

```bash
cd /path/to/your-project
mkdir -p .claude/skills
ln -s /path/to/A-Stockit/skills/astockit .claude/skills/astockit
```

用户级：

```bash
mkdir -p ~/.claude/skills
ln -s /path/to/A-Stockit/skills/astockit ~/.claude/skills/astockit
```

如果这组能力只服务于单个项目，优先使用项目级路径；如果希望在多个项目里复用，再使用用户级路径。

### 架构设计

根据当前仓库状态，A-Stockit 更准确的说法是：它由 **四个公开 surface、一个共享实现 surface，以及一个持久化产物 surface** 组成。

```text
skills/astockit/
├── index.md               # bundle 级定位、路由默认值、边界说明
├── _registry/             # 机器可读元数据
│   ├── bundle.json
│   ├── runtime.json
│   ├── skills.json
│   └── strategies.json
├── _docs/                 # 说明文档、接口定义、运行时文档、作者指南
│   ├── authoring/
│   ├── audit/
│   ├── contracts/
│   ├── runtime/
│   └── skills/
├── _src/                  # 共享 Python 实现
│   ├── core/              # 配置、注册表、运行时、存储
│   ├── market/            # 行情、看板、自选股、基本面上下文
│   ├── research/          # 分析、新闻、评估、paper trading 逻辑
│   ├── strategies/        # 策略预设与信号逻辑
│   ├── backtest/          # 回测模型与引擎
│   └── integrations/      # 对外通知能力
├── _artifacts/            # 会话状态、运行产物、清单与导出物
│   ├── backtests/
│   ├── exports/
│   ├── history/
│   ├── manifests/
│   ├── paper/
│   ├── reports/
│   ├── runs/
│   └── sessions/
└── <skill>/               # 单个技能目录
    ├── SKILL.md           # 公开技能定义
    └── run.py             # 仅在 code-backed 技能存在
```

下图用更紧凑的方式概括公开 surface、执行边界和持久化产物流向：

![A-Stockit 中文架构图](assets/architecture-zh.svg)

当前 bundle 状态可以直接从仓库中验证：

- **24 个公开注册技能 + 1 个内部恢复 helper（`fix-everything`）**
- **18 个 code-backed 技能执行器**
- **6 个 workflow-only 技能定义**
- **20 个 runtime 命令注册项**

也就是说，A-Stockit 现在既不是“只有提示词的技能集”，也不是“只靠 Python API 的库”，而是一个同时提供路由定义、运行时实现和持久化产物的完整技能 bundle；`fix-everything` 作为内部恢复 helper 存在，但不计入公开技能面。

更详细的边界与路由说明见 [skills/astockit/index.md](skills/astockit/index.md)。

### 当前技能覆盖面

当前公开技能面加上内部恢复 helper，大致覆盖以下几类 A 股工作流：

- **标的整理与数据准备**：`watchlist-import`, `watchlist`, `market-data`, `stock-data`, `fundamental-context`, `data-sync`
- **解读与研究**：`market-brief`, `analysis`, `market-analyze`, `technical-scan`, `news-intel`, `market-recap`
- **筛选、决策与执行规划**：`market-screen`, `decision-support`, `decision-dashboard`, `strategy-design`, `paper-trading`
- **历史、评估与运维**：`analysis-history`, `backtest-evaluator`, `reports`, `session-status`, `strategy-chat`, `feishu-notify`, `model-capability-advisor`
- **内部恢复**：`fix-everything`

完整目录见 [skills/astockit/_docs/skills/index.md](skills/astockit/_docs/skills/index.md)。

### 文档

- [skills/astockit/index.md](skills/astockit/index.md)：bundle 级路由、边界和设计目标
- [skills/astockit/_docs/contracts/runtime-interface.md](skills/astockit/_docs/contracts/runtime-interface.md)：运行时接口定义
- [skills/astockit/_docs/authoring/quant-workflow-framework.md](skills/astockit/_docs/authoring/quant-workflow-framework.md)：工作流框架

### 直接使用本仓库或参与开发

对于开发者或具有相关技术背景的用户，如果你希望直接使用本仓库，或者参与本项目开发，可以选择通过 `pip` 安装本仓库。

```bash
python -m pip install -e .
```

按需安装可选依赖：

```bash
python -m pip install -e .[a_share]
python -m pip install -e .[feishu]
python -m pip install -e .[a_share,feishu]
```

这样就可以直接使用本地执行入口：

```bash
python3 skills/astockit/market-brief/run.py 600519 --source auto
python3 skills/astockit/market-screen/run.py 600519 300750 000858 --top 3
python3 skills/astockit/watchlist-import/run.py --text "600519,300750,000858"
```

这部分不建议作为没有相关技术背景的用户的首选入口，更推荐通过现有 Agent 框架调用 A-Stockit。

### 贡献

提交变更前请先阅读 [CONTRIBUTING.md](CONTRIBUTING.md) 和 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)。

### 许可证

本项目采用 [MIT License](LICENSE)。

---

## English

### What Problem Does A-Stockit Solve?

A-Stockit is aimed at a broad set of A-share users, not only professional quant teams.

Many existing Agents can already chat, write code, and call tools, but they still lack a clear A-share operating layer:

- a general-purpose Agent does not know how to turn `watchlists`, `market snapshots`, `technical reads`, `decision guidance`, and `retrospectives` into one coherent workflow
- A-share data, news, fundamentals, execution plans, and historical artifacts are often scattered across scripts, notes, and conversation context
- many users do not want a black-box trading bot, but an A-share capability bundle that an existing Agent can call, inspect, and reuse

A-Stockit packages that layer as a `bundle-first`, `definition-first` skill bundle so the host Agent knows which skill to route to, what artifacts it may produce, which capabilities are already code-backed, and which ones are still stable workflow definitions.

### Who Is This For?

- Everyday A-share users who want AI-assisted watchlists, market review, and decision prep
- Active traders and independent researchers who need screening, briefings, and clearer decision framing
- Agent framework developers who want to plug A-share capabilities into an existing Agent
- Advanced users, including quant researchers, who want reproducible, artifact-driven workflows

### Positioning

A-Stockit is **not** a standalone app and it is **not** a one-click autonomous trading system.

It is an A-share skill bundle for mainstream Agent frameworks:

- `skills/astockit/<skill>/SKILL.md` is the public skill definition
- code-backed skills ship a local `run.py` beside the definition
- shared runtime, market logic, research modules, and persistence live under `_src/`
- durable outputs are written under `_artifacts/` for later review, backtesting, and history-aware routing

### Installation

This chapter only explains how to integrate A-Stockit into an existing Agent. It does not repeat how to install those Agents themselves.

First, get this repository:

```bash
git clone https://github.com/yourusername/A-Stockit.git
cd A-Stockit
```

Notes:

- if an Agent only reads `SKILL.md` skill definitions, you usually do not need to install this repo with `pip` first
- if you later want to run code-backed skills with `run.py`, install runtime dependencies such as `pandas`, `akshare`, or `lark-oapi` as needed
- current `skills/astockit/<skill>/run.py` executors inject `skills/astockit/_src` into `sys.path`

#### 1. Integrate with OpenClaw

OpenClaw currently loads skills from `<workspace>/skills` and `~/.openclaw/skills`, with the workspace path taking precedence.

Project-level:

```bash
cd /path/to/your-project
mkdir -p skills
ln -s /path/to/A-Stockit/skills/astockit skills/astockit
```

User-level:

```bash
mkdir -p ~/.openclaw/skills
ln -s /path/to/A-Stockit/skills/astockit ~/.openclaw/skills/astockit
```

If you are working in a shared workspace, prefer the project-level path. If you want to reuse the bundle across multiple projects, use the user-level path.

#### 2. Integrate with Codex

Codex currently reads skills from `.agents/skills` inside the repository or directory tree, and from the user-level `$HOME/.agents/skills`.

Project-level:

```bash
cd /path/to/your-project
mkdir -p .agents/skills
ln -s /path/to/A-Stockit/skills/astockit .agents/skills/astockit
```

User-level:

```bash
mkdir -p ~/.agents/skills
ln -s /path/to/A-Stockit/skills/astockit ~/.agents/skills/astockit
```

If your repository already has a higher-level `.agents/skills` directory, you can also place `astockit` there so multiple subdirectories share the same bundle.

#### 3. Integrate with Claude Code

Claude Code currently reads skill definitions from project-level `.claude/skills/<skill-name>/SKILL.md` and user-level `~/.claude/skills/<skill-name>/SKILL.md`.

Project-level:

```bash
cd /path/to/your-project
mkdir -p .claude/skills
ln -s /path/to/A-Stockit/skills/astockit .claude/skills/astockit
```

User-level:

```bash
mkdir -p ~/.claude/skills
ln -s /path/to/A-Stockit/skills/astockit ~/.claude/skills/astockit
```

If the bundle only belongs to one project, prefer the project-level path. If you want to reuse it across many projects, use the user-level path.

### Architecture

Based on the current repository state, A-Stockit is best described as **four public surfaces, one shared implementation surface, and one durable artifact surface**.

```text
skills/astockit/
├── index.md               # bundle positioning, routing defaults, boundary guide
├── _registry/             # machine-readable metadata
│   ├── bundle.json
│   ├── runtime.json
│   ├── skills.json
│   └── strategies.json
├── _docs/                 # documentation, interface definitions, runtime docs, authoring notes
│   ├── authoring/
│   ├── audit/
│   ├── contracts/
│   ├── runtime/
│   └── skills/
├── _src/                  # shared Python implementation
│   ├── core/              # config, registry, runtime, storage
│   ├── market/            # market data, dashboards, watchlists, fundamentals
│   ├── research/          # analysis, news, evaluation, paper-trading logic
│   ├── strategies/        # strategy presets and signal logic
│   ├── backtest/          # backtest models and engine
│   └── integrations/      # outbound integrations
├── _artifacts/            # session state, runs, manifests, reports, exports
│   ├── backtests/
│   ├── exports/
│   ├── history/
│   ├── manifests/
│   ├── paper/
│   ├── reports/
│   ├── runs/
│   └── sessions/
└── <skill>/               # individual skill directory
    ├── SKILL.md           # public skill definition
    └── run.py             # only for code-backed skills
```

The diagram below summarizes the same surfaces, execution boundary, and artifact flow in a lower-density layout:

![A-Stockit architecture diagram](assets/architecture.svg)

The live bundle state can be verified directly from the repo:

- **24 public registered skills + 1 internal recovery helper (`fix-everything`)**
- **18 code-backed executors**
- **6 workflow-only skill definitions**
- **20 runtime command registrations**

That means A-Stockit is no longer just a prompt bundle and it is also not only a Python helper library. It is a full skill bundle with routing definitions, shared runtime implementation, and durable artifact storage, with `fix-everything` kept as an internal recovery helper rather than part of the public skill surface.

For the bundle-level routing and boundary guide, see [skills/astockit/index.md](skills/astockit/index.md).

### Current Skill Coverage

The current public skill surface plus the internal recovery helper broadly cover these A-share workflows:

- **Universe intake and data prep**: `watchlist-import`, `watchlist`, `market-data`, `stock-data`, `fundamental-context`, `data-sync`
- **Interpretation and research**: `market-brief`, `analysis`, `market-analyze`, `technical-scan`, `news-intel`, `market-recap`
- **Screening, decisions, and execution planning**: `market-screen`, `decision-support`, `decision-dashboard`, `strategy-design`, `paper-trading`
- **History, evaluation, and operations**: `analysis-history`, `backtest-evaluator`, `reports`, `session-status`, `strategy-chat`, `feishu-notify`, `model-capability-advisor`
- **Internal recovery**: `fix-everything`

See the full catalog in [skills/astockit/_docs/skills/index.md](skills/astockit/_docs/skills/index.md).

### Documentation

- [skills/astockit/index.md](skills/astockit/index.md): bundle routing, boundaries, and design goals
- [skills/astockit/_docs/contracts/runtime-interface.md](skills/astockit/_docs/contracts/runtime-interface.md): runtime interface definition
- [skills/astockit/_docs/authoring/quant-workflow-framework.md](skills/astockit/_docs/authoring/quant-workflow-framework.md): workflow framework

### Using The Repo Directly Or Contributing

For developers or users with a relevant technical background, if you want to use this repository directly or participate in development, you can install the repo with `pip`:

```bash
python -m pip install -e .
```

Optional extras:

```bash
python -m pip install -e .[a_share]
python -m pip install -e .[feishu]
python -m pip install -e .[a_share,feishu]
```

This path is a better fit when:

- you want to call the repo as a Python package directly
- you are contributing, debugging, or validating packaging behavior
- you want explicit local control over dependencies and extras

After that, you can directly use local executors:

```bash
python3 skills/astockit/market-brief/run.py 600519 --source auto
python3 skills/astockit/market-screen/run.py 600519 300750 000858 --top 3
python3 skills/astockit/watchlist-import/run.py --text "600519,300750,000858"
```

This is not the preferred entry path for general users. For most users, the better path is to call A-Stockit through an existing Agent.

### Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) and [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) before sending changes.

### License

This project is licensed under the [MIT License](LICENSE).
