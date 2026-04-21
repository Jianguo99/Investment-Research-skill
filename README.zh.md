# Investment Research Skill for Claude Code / OpenCode / Codex

[English](README.md) | [中文](README.zh.md)

> 如果这个项目对你有帮助，欢迎点个 star。

> 本仓库是一个面向投资研究场景的改写版本，整体工作流、结构设计和起点致敬并承接自 [Deep-Research-skills](https://github.com/Weizhena/Deep-Research-skills)。感谢原仓库提供的基础，这个仓库后续也会继续在这个基础上朝投资研究方向迭代。

> 灵感来源：[RhinoInsight: Improving Deep Research through Control Mechanisms for Model Behavior and Context](https://arxiv.org/abs/2511.18743)

这个项目把原本偏通用的结构化调研工作流，转成更适合投资研究的 skill 集合。它面向分析师和投资者，帮助完成研究范围搭建、资料搜集、公司比较、证据整理，以及最终研究报告或投资备忘录草稿的生成。

目前仓库仍然保留了上游不少命名与目录结构，例如 `research`、`research-deep` 以及对应的 skill 目录，这样后续继续修改时更容易和原始仓库对照。也正因为如此，这个仓库虽然保留兼容命名，但 README 的定位、示例和后续方向都会明确围绕投资研究展开。

![Investment Research Workflow](workflow.png)

## 这个 Skill 适合做什么

- **公司研究**：理解商业模式、业务结构、管理层背景和战略定位
- **行业研究**：梳理产业链、竞争格局、行业驱动因素和长期趋势
- **投资逻辑研究**：整理成长驱动、催化剂、核心假设以及反证材料
- **尽职调查**：汇总风险因素、潜在红旗、监管信息和多来源交叉验证
- **可比分析**：建立 peer list，并按统一字段收集多个公司或资产的信息

## 典型产出

通过这套工作流，你可以让 AI 协助生成：

- 一份结构化的研究清单或覆盖范围
- 一套可复用的公司/行业研究字段模板
- 每个研究对象的证据型笔记
- 一份 markdown 研究报告或投资备忘录草稿

## 安装

克隆当前仓库或你自己的 fork：

```bash
git clone <this-repo-or-your-fork-url> Investment-Research-skill
cd Investment-Research-skill
```

### Claude Code

```bash
# 中文版
cp -r skills/research-zh/* ~/.claude/skills/

# 英文版
cp -r skills/research-en/* ~/.claude/skills/

# 必需：安装 web search agent 和模块
cp agents/web-search-agent.md ~/.claude/agents/
cp -r agents/web-search-modules ~/.claude/agents/

# 必需：安装 Python 依赖
pip install pyyaml
```

### OpenCode（默认: gpt-5.4）

```bash
# Skills
cp -r skills/research-zh/* ~/.claude/skills/   # 或者使用 research-en 英文版

# 必需：为当前 shell 启用 web search
export OPENCODE_ENABLE_EXA=1

# 可选：写入 ~/.bashrc，永久生效
echo 'export OPENCODE_ENABLE_EXA=1' >> ~/.bashrc
source ~/.bashrc

# 必需：安装 web search agent 和模块
cp agents/web-search-opencode.md ~/.config/opencode/agents/web-search.md
cp -r agents/web-search-modules ~/.config/opencode/agents/

# 必需：安装 Python 依赖
pip install pyyaml
```

> **重要**：在 OpenCode 中，要使用 web search，需要设置 `OPENCODE_ENABLE_EXA=1`。单纯 `export` 只对当前 shell 生效，写入 `~/.bashrc` 后才会长期生效。如果不设置，就只能使用 `web fetch`，对于证据搜集和多来源交叉验证会明显不够。

### Codex

```bash
# 英文版
mkdir -p ~/.codex/skills ~/.codex/agents
cp -r skills/research-codex-en/* ~/.codex/skills/

# 中文版
mkdir -p ~/.codex/skills ~/.codex/agents
cp -r skills/research-codex-zh/* ~/.codex/skills/

# 必需：安装 web researcher agent 和模块
cp agents-codex/web-researcher.toml ~/.codex/agents/
cp -r agents-codex/web-search-modules ~/.codex/agents/

# 必需：安装 Python 依赖
pip install pyyaml
```

使用下面任一方式更新 `~/.codex/config.toml`。

**方式 A：自动脚本**

```bash
cd Investment-Research-skill
bash scripts/install-codex.sh
```

**方式 B：手动编辑**

```toml
suppress_unstable_features_warning = true

[features]
multi_agent = true
default_mode_request_user_input = true

[agents.web_researcher]
description = "Use this agent when you need internet research for investment work, such as company analysis, industry mapping, regulatory checks, transcript collection, or gathering evidence from multiple public sources. Use it when you need creative search strategies, broad source coverage, and organized findings."
config_file = "agents/web-researcher.toml"
```

## 命令

> **兼容性说明**：当前命令名仍然沿用上游 `research-*` 命名，便于后续维护和对照，但这里的推荐使用场景已经明确切到投资研究。

> **Claude Code 2.1.0+**：支持直接用 `/skill-name` 触发。
>
> **旧版本**：请使用 `run /skill-name`。
>
> **Codex**：你可以从 `/skills` -> `List Skills` 里触发，也可以直接自然语言说明，例如 `Use the invest_reasearch skill to build an outline for China's brokerages`。

| 命令 | 在投资研究中的用途 |
|------|--------------------|
| `/invest_reasearch` | 生成初始研究 outline，包括研究对象和需要收集的字段 |
| `/research-deep` | 对每个研究对象做更深入的联网检索和证据搜集，并行整理结果 |
| `/research-report` | 把整理好的 JSON 结果转成可读的 markdown 报告或备忘录草稿 |

## 工作流示例

> **示例主题**：中国券商行业

### 阶段 1：搭建研究范围

```text
/invest_reasearch 中国券商行业
```

**会发生什么**：你给出一个投资研究主题，skill 会先帮你提出结构化的研究范围。

对于 Codex，也可以这样说：

```text
Use the invest_reasearch skill to build an outline for China's brokerages
```

**你会得到什么**：一组待研究的公司、细分方向或可比标的，以及一套字段列表，例如业务概况、收入结构、估值、催化剂、风险点和近期动态。

### 阶段 2：深度调查

```text
/research-deep
```

**会发生什么**：工作流会逐个检索每个研究对象，从多个公开来源收集证据，并按对象整理材料。

**你会得到什么**：每个公司或主题的详细资料，例如商业模式、财务背景、估值背景、行业地位、关键风险、催化剂和重要更新。

### 阶段 3：生成报告

```text
/research-report
```

**会发生什么**：前面收集到的数据会被整合成结构化的 markdown 输出。

**你会得到什么**：`report.md`，一份可继续编辑的研究报告或投资备忘录草稿，包含分节和目录。

## 为什么有这个仓库

这个仓库并不回避自己的来源。它就是在 `Deep-Research-skills` 的基础上，朝下面这些方向继续推进的一个致敬式改写版本：

- 股票和公司研究
- 行业和主题映射
- 尽调导向的证据搜集
- 投资备忘录生成

换句话说，它不是假装从零开始，而是明确地站在原仓库基础上继续往前做。

## 需要帮助？

如果你想让助手解释这个仓库的定位，可以直接这样问：

```text
帮我理解这个投资研究 skill 项目，以及它如何基于 Deep-Research-skills 继续演进
```

## 致谢

- [Deep-Research-skills](https://github.com/Weizhena/Deep-Research-skills)
- [RhinoInsight: Improving Deep Research through Control Mechanisms for Model Behavior and Context](https://arxiv.org/abs/2511.18743)

## 许可证

MIT
