# Investment Research Skill for Claude Code / OpenCode / Codex

[English](README.md) | [中文](README.zh.md)

> If this project helps you, please consider giving it a star.

> This repository is an investment-research-oriented adaptation of [Deep-Research-skills](https://github.com/Weizhena/Deep-Research-skills). Credit and thanks to the original project for the workflow design, structure, and starting point. This repo is intended to continue evolving from that base toward investment research use cases.

> Inspired by [RhinoInsight: Improving Deep Research through Control Mechanisms for Model Behavior and Context](https://arxiv.org/abs/2511.18743)

This project turns the original structured research workflow into a skill set aimed at investment research. It is designed for analysts and investors who want help building a research universe, collecting evidence, comparing companies, and drafting organized outputs.

The current repository still keeps much of the upstream command naming and folder layout, such as `research`, `research-deep`, and related skill folders, so future modifications can stay easy to track. The wording, examples, and intended direction of this repo are focused on investment research.

![Investment Research Workflow](workflow.png)

## What This Skill Is For

- **Company Research**: Understand business models, segment mix, management background, and strategic positioning
- **Industry Research**: Map industry chains, market structure, competitive dynamics, and long-term trends
- **Investment Thesis Work**: Collect growth drivers, catalysts, key assumptions, and disconfirming evidence
- **Due Diligence**: Organize risk factors, red flags, regulatory issues, and multi-source verification
- **Comparable Analysis**: Build peer sets and gather consistent fields across multiple companies or assets

## Typical Output

With this workflow, you can use AI to help produce:

- A structured research universe or coverage list
- A reusable field schema for company and industry work
- Evidence-backed notes for each target
- A markdown research report or investment memo draft

## Installation

Clone this repository or your fork:

```bash
git clone <this-repo-or-your-fork-url> Investment-Research-skill
cd Investment-Research-skill
```

### Claude Code

```bash
# English version
cp -r skills/research-en/* ~/.claude/skills/

# Chinese version
cp -r skills/research-zh/* ~/.claude/skills/

# Required: install the web search agent and modules
cp agents/web-search-agent.md ~/.claude/agents/
cp -r agents/web-search-modules ~/.claude/agents/

# Required: install Python dependency
pip install pyyaml
```

### OpenCode (default: gpt-5.4)

```bash
# Skills
cp -r skills/research-en/* ~/.claude/skills/   # or use research-zh for Chinese

# Required: enable web search for the current shell
export OPENCODE_ENABLE_EXA=1

# Optional: make it persistent
echo 'export OPENCODE_ENABLE_EXA=1' >> ~/.bashrc
source ~/.bashrc

# Required: install the web search agent and modules
cp agents/web-search-opencode.md ~/.config/opencode/agents/web-search.md
cp -r agents/web-search-modules ~/.config/opencode/agents/

# Required: install Python dependency
pip install pyyaml
```

> **Important**: In OpenCode, web search requires `OPENCODE_ENABLE_EXA=1`. A plain `export` only affects the current shell. Writing it to `~/.bashrc` makes it persistent. Without it, you only get `web fetch`, which is much weaker for evidence collection and cross-source investigation.

### Codex

```bash
# English version
mkdir -p ~/.codex/skills ~/.codex/agents
cp -r skills/research-codex-en/* ~/.codex/skills/

# Chinese version
mkdir -p ~/.codex/skills ~/.codex/agents
cp -r skills/research-codex-zh/* ~/.codex/skills/

# Required: install the web researcher agent and modules
cp agents-codex/web-researcher.toml ~/.codex/agents/
cp -r agents-codex/web-search-modules ~/.codex/agents/

# Required: install Python dependency
pip install pyyaml
```

Add or update `~/.codex/config.toml` using either method below.

**Option A: Automatic script**

```bash
cd Investment-Research-skill
bash scripts/install-codex.sh
```

**Option B: Manual edit**

```toml
suppress_unstable_features_warning = true

[features]
multi_agent = true
default_mode_request_user_input = true

[agents.web_researcher]
description = "Use this agent when you need internet research for investment work, such as company analysis, industry mapping, regulatory checks, transcript collection, or gathering evidence from multiple public sources. Use it when you need creative search strategies, broad source coverage, and organized findings."
config_file = "agents/web-researcher.toml"
```

## Commands

> **Compatibility note**: command names currently keep the upstream `research-*` naming for easier maintenance, but the intended usage here is investment research.

> **Claude Code 2.1.0+**: Direct `/skill-name` trigger is supported.
>
> **Older versions**: Use `run /skill-name` instead.
>
> **Codex**: You can trigger these skills from `/skills` -> `List Skills`, or ask naturally, for example `Use the invest_reasearch skill to build an outline for China's brokerages`.

| Command | Investment Research Usage |
|---------|---------------------------|
| `/invest_reasearch` | Build an initial research outline, including targets and fields to collect |
| `/research-deep` | Run deeper web research on each target with parallel agents and evidence gathering |
| `/research-report` | Convert the collected JSON results into a readable markdown report or memo draft |

## Workflow Example

> **Example topic**: China's brokerages

### Phase 1: Build the Research Universe

```text
/invest_reasearch China's brokerages
```

**What happens**: You give the skill an investment topic, and it proposes a structured research scope.

For Codex, you can also say:

```text
Use the invest_reasearch skill to build an outline for China's brokerages
```

**What you get**: A list of companies, sub-segments, or comparable targets to investigate, plus a field list such as business overview, revenue mix, valuation, catalysts, risks, and recent developments.

### Phase 2: Deep Investigation

```text
/research-deep
```

**What happens**: The workflow searches the web for each target, gathers evidence from multiple sources, and organizes notes target by target.

**What you get**: Detailed material for each company or topic, including business model, financial context, valuation background, industry position, key risks, catalysts, and relevant updates.

### Phase 3: Generate the Report

```text
/research-report
```

**What happens**: The collected data is consolidated into a structured markdown output.

**What you get**: `report.md`, a readable research report or memo draft with sections and table of contents, ready for further editing.

## Why This Repo Exists

This repo is not trying to hide its origin. It is a respectful adaptation of `Deep-Research-skills`, with the goal of steering the workflow toward:

- equity and company research
- industry and thematic mapping
- diligence-oriented evidence collection
- investment memo generation

The repository will continue to be modified on top of that foundation rather than pretending to be built from scratch.

## Need Help?

If you want the assistant to explain this repo's positioning, you can ask:

```text
Help me understand this investment research skill project and how it builds on Deep-Research-skills
```

## Acknowledgements

- [Deep-Research-skills](https://github.com/Weizhena/Deep-Research-skills)
- [RhinoInsight: Improving Deep Research through Control Mechanisms for Model Behavior and Context](https://arxiv.org/abs/2511.18743)

## License

MIT
