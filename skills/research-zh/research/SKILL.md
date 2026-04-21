---
name: invest_reasearch
description: 针对单只股票或单家公司执行完整投资研究，从立项、证据收集、护城河与 ROIC 分析、增长与预期差、估值与风险、JSON 校验到最终投资结论一次完成。适用于需要一个完整投资判断，而不是拆成多个 skill 分步运行的场景。
---

# Invest Reasearch Skill

## 触发方式
`/invest_reasearch <公司名称或股票代码>`

## 任务目标
围绕单一上市公司完成一次端到端投资研究。

除非用户明确要求停在中间产物，否则不要把任务拆成额外 skill，也不要要求用户再运行别的命令。

最终必须回答这五个问题：

1. 这是不是长期优质企业？
2. 它是否具备持续较高的 ROIC 和现金创造能力？
3. 市场当前是否存在错误定价？
4. 未来 1 到 3 年的合理回报区间大致是多少？
5. 最终投资动作是什么？

## 研究原则

- 先看商业质量、行业结构和再投资空间，再看估值。
- 优先使用最新且高质量的来源：财报、公告、业绩会、投资者演示、交易所披露、行业数据、可靠第三方研究。
- 明确区分 `事实`、`市场预期`、`推断`。
- 缺乏证据支持的内容标记为 `[uncertain]`。
- 每个重要结论都要附带证伪条件和后续跟踪指标。
- 如果当前目录已有研究产物，先检查后续跑，避免重复生成。
- 如果存在 `references/research_checklist.md`，在最终下结论前把它当作硬约束清单使用。

## 工作流

### Step 1：确认研究范围

确认或合理推断出形成投资结论所需的最小输入：

- 公司名称
- ticker、交易所、记账货币
- 希望覆盖的时间范围
- 最低可接受回报率或收益门槛

如果当前目录已经有 `outline.yaml`、`fields.yaml`、`module-results/`、`results/` 或 `report.md`，先读取它们，再决定是续跑、局部刷新还是整体重做。

### Step 2：生成或刷新研究计划

创建或更新 `{company_slug}/outline.yaml` 和 `{company_slug}/fields.yaml`。

`outline.yaml` 至少包含：

- `topic`
- `target_company`
- `ticker`
- `research_type: invest_reasearch_stock`
- `core_questions`
- `key_variables`
- `source_priority`
- `modules`
- `execution`

`modules` 应覆盖：

- `industry`
- `moat`
- `financial`
- `growth`
- `expectation`
- `valuation`
- `risk`
- `investment_committee`

`fields.yaml` 应定义以下类别：

- 研究框架
- 行业结构
- 护城河与质量
- 财务与现金流
- 增长驱动
- 市场预期
- 估值与回报
- 风险与证伪
- 最终结论

每个字段定义至少包含：

- `name`
- `description`
- `detail_level`

并始终保留 `uncertain` 字段。

### Step 3：收集证据并完成模块分析

通过联网检索和原始资料阅读收集证据。若宿主支持并行助手，就按模块拆分；否则顺序执行，但保持同样的模块结构。

把模块笔记写入：

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

每个模块笔记都必须以这四部分收尾：

- 关键事实
- 核心解释
- 未决问题
- 证伪点

### Step 4：合并结构化结果并校验

读取全部模块笔记，合并成一个结构化 JSON：

```text
{company_slug}/results/{target_company_slug}.json
```

规则：

- JSON 必须和 `fields.yaml` 对齐
- 保留 `uncertain` 数组，列出尚未解决的字段
- 不要悄悄省略缺失模块
- 明确区分证据和判断

写完 JSON 后，运行与本 skill 同目录的 `validate_json.py` 进行校验。持续修正覆盖缺口，直到校验通过，或者可以明确说明阻塞原因。

### Step 5：生成最终投资备忘录

生成 `{company_slug}/report.md`。只有在“后续会重复刷新同一份报告”时，才额外生成 `generate_report.py`；否则直接写 `report.md` 即可。

报告至少包含：

- 一句话核心结论
- 商业质量与护城河总结
- ROIC、现金流与增长总结
- 市场预期差
- Bear / Base / Bull 三情景表
- 关键风险与证伪点
- 跟踪清单
- 最终投资结论

最终结论只能四选一：

- `明显低估（重仓）`
- `结构机会（跟踪）`
- `合理估值（观望）`
- `明显高估（回避）`

### Step 6：向用户返回结果

先给出最终投资判断，再指出保存下来的产物路径。如果把握度不足，要具体说明还缺哪类证据。

## 输出结构

```text
{current_working_directory}/{company_slug}/
  outline.yaml
  fields.yaml
  module-results/
  results/{target_company_slug}.json
  generate_report.py   # 可选
  report.md
```
