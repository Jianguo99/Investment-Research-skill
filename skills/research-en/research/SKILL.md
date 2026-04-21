---
name: invest_reasearch
description: Conduct end-to-end investment research for a single public company or stock, from scoping and evidence collection through moat, ROIC, growth, valuation, risk, JSON validation, and the final decision memo. Use when you need one complete investment view rather than a split multi-command workflow.
---

# Invest Reasearch Skill

## Trigger
`/invest_reasearch <company name or ticker>`

## Mission
Complete one full investment-research workflow for a single listed company.

Do not split the work into follow-up skills or ask the user to run another command unless they explicitly want to stop at an intermediate artifact.

Always answer these five questions:

1. Is this a long-term high-quality business?
2. Does it sustain attractive ROIC and cash generation?
3. Is the market currently mispricing it?
4. What is the reasonable 1-3 year return range?
5. What is the final action?

## Operating Principles

- Start with business quality, industry structure, and reinvestment runway before valuation.
- Prefer primary and recent sources: filings, earnings calls, investor presentations, exchange disclosures, industry data, and reputable third-party research.
- Separate `Facts`, `Market Expectations`, and `Inference`.
- Mark unsupported or incomplete claims as `[uncertain]`.
- Every major conclusion needs a falsification condition and a monitoring indicator.
- If work artifacts already exist, inspect them first and resume from the latest trustworthy step instead of recreating everything.
- If `references/research_checklist.md` is present, use it as a hard gate before the final decision.

## Workflow

### Step 1: Scope the assignment

Confirm or infer the minimum inputs needed for an investable conclusion:

- company name
- ticker, exchange, and reporting currency
- preferred time range
- minimum acceptable return or hurdle rate

If the working directory already contains `outline.yaml`, `fields.yaml`, `module-results/`, `results/`, or `report.md`, read them first and decide whether to resume, refresh, or replace stale pieces.

### Step 2: Build or refresh the research plan

Create or update `{company_slug}/outline.yaml` and `{company_slug}/fields.yaml`.

`outline.yaml` must contain at least:

- `topic`
- `target_company`
- `ticker`
- `research_type: invest_reasearch_stock`
- `core_questions`
- `key_variables`
- `source_priority`
- `modules`
- `execution`

`modules` should cover:

- `industry`
- `moat`
- `financial`
- `growth`
- `expectation`
- `valuation`
- `risk`
- `investment_committee`

`fields.yaml` should define categories for:

- research framing
- industry structure
- moat and quality
- financials and cash flow
- growth drivers
- market expectations
- valuation and returns
- risk and falsification
- final decision

Every field definition should include:

- `name`
- `description`
- `detail_level`

Always keep a reserved `uncertain` field.

### Step 3: Collect evidence and run module analysis

Gather evidence with web research and primary-source review. If the host supports parallel helpers, split the work by module; otherwise run sequentially while keeping the same structure.

Write module notes to:

```text
{company_slug}/module-results/
  industry.md
  moat.md
  financial.md
  growth.md
  expectation.md
  valuation.md
  risk.md
```

Each module note must end with:

- key facts
- interpretation
- open questions
- falsification points

### Step 4: Merge structured output and validate it

Read all module notes and map them into a single structured JSON:

```text
{company_slug}/results/{target_company_slug}.json
```

Rules:

- align the JSON to `fields.yaml`
- keep an `uncertain` array listing unresolved fields
- do not silently drop missing sections
- distinguish clearly between evidence and judgment

Run the bundled `validate_json.py` located next to this skill after writing the JSON. Keep fixing coverage gaps until validation passes or you can clearly explain the blocker.

### Step 5: Produce the final investment memo

Generate `{company_slug}/report.md`. Create `generate_report.py` only when a reusable report-conversion script is genuinely helpful; otherwise write the report directly.

The report must contain:

- one-sentence core conclusion
- business quality and moat summary
- ROIC, cash flow, and growth summary
- market expectation gap
- bear / base / bull scenario table
- key risks and falsification points
- monitoring list
- final decision

Restrict the final decision to one of:

- `Clearly Undervalued (High Conviction)`
- `Structured Opportunity (Watchlist)`
- `Fairly Valued (Observe)`
- `Clearly Overvalued (Avoid)`

### Step 6: Return the result to the user

Share the final decision first, then point to the saved artifacts. If confidence is limited, explain exactly which evidence gap remains unresolved.

## Output Layout

```text
{current_working_directory}/{company_slug}/
  outline.yaml
  fields.yaml
  module-results/
  results/{target_company_slug}.json
  generate_report.py   # optional
  report.md
```
