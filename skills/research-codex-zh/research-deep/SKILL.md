---
name: research-deep
description: 读取单只股票的投资研究 outline，按多 Agent 模式并行执行行业、护城河、财务、增长、预期、估值、风险等模块，先产出中间结论，再合并为结构化 JSON。
---

# Research Deep - 多 Agent 深度研究

## 触发方式
`/research-deep`

## Workflow

### Step 1：读取研究配置

在当前目录定位 `*/outline.yaml`，读取：

- `target_company`
- `core_questions`
- `key_variables`
- `source_priority`
- `module_agents`
- `execution.output_dir`
- `execution.module_dir`

### Step 2：创建模块工作目录

在研究目录下准备：

```text
module-results/
  |- industry.md
  |- moat.md
  |- financial.md
  |- growth.md
  |- expectation.md
  |- valuation.md
  |- risk.md
results/
  |- {target_company_slug}.json
```

### Step 3：并行运行子 Agent

如果宿主支持 sub-agents，就并行启动以下模块；否则顺序执行，但仍按模块拆开：

1. `Industry Analyst`
2. `Moat Analyst`
3. `Financial Analyst`
4. `Growth Analyst`
5. `Market Expectation Analyst`
6. `Valuation Analyst`
7. `Risk Analyst`

每个模块必须：

- 有明确目标
- 使用外部数据支持
- 输出中间结论

每个模块都先写入自己的 `module-results/*.md`。

### Step 4：模块任务要求

以下要求适用于每一个子 Agent。

**统一研究原则**

1. 优先分析商业质量，而不是估值
2. 禁止仅用 PE / PB 做结论
3. 必须优先围绕护城河、ROIC、自由现金流、行业结构展开
4. 必须明确区分：
   - `✅ 事实`
   - `📊 机构观点`
   - `🧠 市场预期 / 叙事`
5. 数据不足时标记 `[uncertain]`
6. 所有重要结论都必须可被证伪

**Industry Analyst**

目标：

- 判断行业是否赚钱
- 判断竞争格局
- 判断产业链利润分配
- 判断是否长期内卷

输出到 `module-results/industry.md`

**Moat Analyst**

目标：

- 判断护城河是否存在
- 护城河类型与强度
- ROIC 3–5 年趋势
- 定价权是否存在
- 赚钱来自能力还是周期

输出评级：

- `强 / 中 / 弱 / 无`

输出到 `module-results/moat.md`

**Financial Analyst**

目标：

- 比较净利润与经营现金流
- 判断 FCF 稳定性
- 判断资本开支依赖

输出：

- 现金流质量评级

输出到 `module-results/financial.md`

**Growth Analyst**

目标：

- 拆解价格驱动
- 拆解量驱动
- 拆解扩张驱动
- 判断增长是否可持续

输出到 `module-results/growth.md`

**Market Expectation Analyst**

目标：

- 判断市场当前共识
- 提取一致预期（收入 / 利润）
- 判断当前叙事

输出到 `module-results/expectation.md`

**Valuation Analyst**

目标：

- 判断当前价格隐含增长
- 估算未来 3 年收益率（IRR）
- 输出 Bear / Base / Bull 三情景

输出到 `module-results/valuation.md`

**Risk Analyst**

目标：

- 定义什么情况出现会导致投资逻辑失败
- 列出必须持续跟踪的指标

输出到 `module-results/risk.md`

### Step 5：合并模块结果

在全部模块完成后，协调层必须：

- 读取全部 `module-results/*.md`
- 将中间结论合并进主研究标的 JSON
- 按 `fields.yaml` 输出结构化结果到 `results/{target_company_slug}.json`

JSON 中必须覆盖：

- 行业结构分析
- 公司质量分析
- 财务与现金流
- 增长拆解
- 市场预期分析
- 预期差识别
- 估值与回报率
- 情景分析
- 证伪机制
- 最终决策草案

### Step 6：校验

合并完成后运行：

```text
python ~/.codex/skills/research/validate_json.py -f {fields_path} -j {output_path}
```

只有校验通过，deep 阶段才算完成。

## Output

- `module-results/*.md`
- `results/{target_company_slug}.json`
