# Investment Research Skill for Claude Code / OpenCode / Codex

[English](README.md) | [中文](README.zh.md)

> If this project helps you, please consider giving it a star.
>
> This repository is an investment-research adaptation of [Deep-Research-skills](https://github.com/Weizhena/Deep-Research-skills). Credit and thanks to the original project for the workflow design, structure, and starting point.
>
> Inspired by [RhinoInsight: Improving Deep Research through Control Mechanisms for Model Behavior and Context](https://arxiv.org/abs/2511.18743)

This repository now exposes a single main skill, `invest_reasearch`, for each platform/language package. The internal workflow still uses outline, module analysis, structured JSON, and final reporting, but the user no longer has to hop across separate `research-deep` and `research-report` skills.

![Investment Research Workflow](workflow.png)

## What The Skill Does

`invest_reasearch` is designed for end-to-end research on one listed company or stock:

- understand the business model, moat, and industry structure
- evaluate ROIC, cash flow quality, and growth durability
- identify consensus expectations and possible mispricing
- frame bear / base / bull outcomes
- produce a structured JSON result and a final investment memo

## Typical Outputs

Running the skill can produce:

- `outline.yaml` for scope, key questions, and execution settings
- `fields.yaml` for the research schema
- `module-results/*.md` for industry, moat, financial, growth, expectation, valuation, and risk notes
- `results/{company}.json` for the structured research result
- `report.md` for the final decision memo

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

## Command

> **Claude Code 2.1.0+**: Direct `/skill-name` trigger is supported.
>
> **Older versions**: Use `run /skill-name` instead.
>
> **Codex**: You can trigger the skill from `/skills` -> `List Skills`, or ask naturally, for example `Use the invest_reasearch skill to research NVIDIA`.

| Command | What It Does |
|---------|---------------|
| `/invest_reasearch` | Runs the full workflow: scope the company, collect evidence, analyze modules, validate JSON, and generate the final memo |

## Example

```text
/invest_reasearch NVIDIA
```

For Codex, you can also say:

```text
Use the invest_reasearch skill to research NVIDIA and give me a final investment view
```

By default, the skill will:

1. build or refresh the research plan
2. collect evidence and produce module notes
3. merge the structured JSON result
4. generate the final investment memo

If you only want an intermediate artifact, say so explicitly, for example: "stop after outline.yaml".

## Output Layout

```text
{working-directory}/{company_slug}/
  outline.yaml
  fields.yaml
  module-results/
  results/{target_company_slug}.json
  generate_report.py   # optional
  report.md
```

## Why This Repo Exists

This repo is a respectful continuation of `Deep-Research-skills`, but with the workflow pointed toward:

- company and stock research
- investment-quality evidence gathering
- expectation-gap analysis
- valuation and return framing
- final investment memo generation

The repository continues to build on that upstream structure rather than pretending to be unrelated.

## Need Help?

You can ask:

```text
Help me understand this investment research skill project and how it works
```

## Acknowledgements

- [Deep-Research-skills](https://github.com/Weizhena/Deep-Research-skills)
- [RhinoInsight: Improving Deep Research through Control Mechanisms for Model Behavior and Context](https://arxiv.org/abs/2511.18743)

## License

MIT
