---
name: research-report
description: 将多 Agent 深度研究结果整理成最终投资决策报告，只输出决策所需的核心结论、预期差、三情景估值、风险与证伪点及总结表。
---

# Research Report - 最终投资决策报告

## 触发方式
`/research-report`

## Workflow

### Step 1：读取最终研究结果

读取：

- `outline.yaml`
- `fields.yaml`
- `module-results/*.md`
- `results/{target_company_slug}.json`

### Step 2：投资委员会汇总

把模块结果交给 `Investment Committee` 进行最终综合判断。

Investment Committee 必须回答：

1. 是否是长期优质企业（compounder）
2. 是否具备持续高 ROIC 能力
3. 当前市场是否错误定价（mispricing）
4. 未来 1–3 年合理回报率
5. 是否值得重仓

### Step 3：生成最终报告脚本

生成 `generate_report.py`，并要求脚本只输出以下最终格式。

## Final Output Format

### 核心结论

必须一句话。

### 投资判断

简洁回答：

- 是否属于长期优质企业
- 是否具备持续高 ROIC
- 是否存在错误定价
- 未来 1–3 年合理回报率大致是多少
- 最终结论只能从以下四项中选择一个：
  - `明显低估（重仓）`
  - `结构机会（跟踪）`
  - `合理估值（观望）`
  - `明显高估（回避）`

### 预期差总结

必须明确：

- 市场当前在预期什么
- 市场可能错在哪里
- 哪些变量尚未被定价

### 三情景估值

必须输出表格：

| 情景 | 假设 | 利润 | 估值 | 目标价 | IRR |
| --- | --- | --- | --- | --- | --- |
| Bear |  |  |  |  |  |
| Base |  |  |  |  |  |
| Bull |  |  |  |  |  |

### 风险与证伪点

必须明确：

- 什么情况出现会导致投资逻辑失败
- 哪些指标必须持续跟踪

### 总结表

| 维度 | 内容 |
| --- | --- |
| 看多逻辑 |  |
| 看空逻辑 |  |
| 核心变量 |  |
| 最大风险 |  |
| 最终结论 |  |

### Final Question

必须回答：

`如果你是芒格，你是否会重仓这家公司？为什么？`

## Hard Rules

- 禁止空话
- 禁止仅用 PE 判断
- 所有结论必须可验证、可跟踪、可证伪

### Step 4：执行脚本

运行：

```text
python {topic}/generate_report.py
```

## Output

- `{topic}/generate_report.py`
- `{topic}/report.md`
