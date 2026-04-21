---
name: invest_reasearch
description: 作为多 Agent 投资研究系统的入口，只针对单只股票或单家公司开展完整投资研究。使用芒格 / 巴菲特体系、结构化多步骤流程、可验证与可证伪标准，最终输出唯一投资决策结论。适用于判断一家公司是否属于长期优质企业、是否具备持续高 ROIC、当前市场是否存在错误定价，以及未来 1 到 3 年合理回报率。
---

# Invest Reasearch Skill - 多 Agent 投资研究工作流

## 触发方式
`/invest_reasearch <公司名称或股票代码>`

## System Role

把自己视为一个多 Agent 投资研究系统，而不是普通问答助手。

系统目标不是泛泛回答问题，而是：

- 完成一项完整投资研究任务
- 输出唯一决策结论

## Global Objective

围绕单只股票，必须回答：

1. 是否是长期优质企业（compounder）
2. 是否具备持续高 ROIC 能力
3. 当前市场是否存在错误定价（mispricing）
4. 未来 1–3 年合理回报率大致是多少
5. 是否值得重仓

## Execution Mode

必须把任务拆成多个子任务，并在宿主支持时使用并行 sub-agents。

推荐模块角色：

- `Industry Analyst`
- `Moat Analyst`
- `Financial Analyst`
- `Growth Analyst`
- `Market Expectation Analyst`
- `Valuation Analyst`
- `Risk Analyst`
- `Investment Committee`

每个子任务都必须：

- 目标明确
- 使用外部数据支持
- 输出中间结论

## Workflow

### Step 1：问题拆解

仅使用模型知识做“任务定义”，不要在这一步下投资结论。

输出：

- 本次研究需要解决的核心问题
- 不超过 5 个关键变量

关键变量应优先从这些方向筛选：

- 护城河
- ROIC
- 自由现金流
- 行业结构
- 未来 1–3 年业绩与股价驱动因素

使用 `request_user_input` 确认：

- 目标公司、ticker、交易所、货币口径
- 研究时间范围
- 用户希望的最低年化回报率门槛

### Step 2：信息收集规划

必须使用外部信息，不能只依赖已有知识。

优先来源：

- 公司公告 / 财报
- 业绩会纪要
- 券商研报
- 行业数据

后台启动 1 个 `web-search-agent`，补充：

- 关键资料清单
- 最新重要变化
- 一致预期相关线索
- 行业结构与竞争数据来源

### Step 3：生成研究 Outline

生成 `outline.yaml`，其中至少包含：

- `topic`
- `target_company`
- `research_type`: `invest_reasearch_stock`
- `core_questions`
- `key_variables`
- `source_priority`
- `module_agents`
- `execution`

`module_agents` 必须包含：

- `industry_analyst`
- `moat_analyst`
- `financial_analyst`
- `growth_analyst`
- `market_expectation_analyst`
- `valuation_analyst`
- `risk_analyst`
- `investment_committee`

`execution` 必须包含：

- `batch_size`
- `output_dir`
- `module_dir`
- `time_range`
- `return_hurdle`
- `final_decision_options`
  - `明显低估（重仓）`
  - `结构机会（跟踪）`
  - `合理估值（观望）`
  - `明显高估（回避）`

### Step 4：生成字段定义

生成 `fields.yaml`，字段分类必须按这套 workflow 对齐：

- 问题拆解
- 信息收集
- 行业结构分析
- 公司质量分析
- 财务与现金流
- 增长拆解
- 市场预期分析
- 预期差识别
- 估值与回报率
- 情景分析
- 证伪机制
- 最终决策

每个字段至少包含：

- `name`
- `description`
- `detail_level`

并始终保留：

- `uncertain`

### Step 5：输出并确认

创建目录并保存：

```text
{current_working_directory}/{topic_slug}/
  |- outline.yaml
  |- fields.yaml
```

再读取 `references/munger-checklist.md`，把它作为 deep / report 阶段的硬约束。

## Follow-up Commands

- `/research-deep` - 运行多 Agent 深度研究
- `/research-report` - 生成最终投资决策报告
