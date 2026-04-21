# Investment Research Checklist (Munger Framework | Agent Version)

## 🎯 PURPOSE
This checklist is a **hard constraint system** for investment decisions.

It ensures:
- Consistent analysis across companies
- Avoidance of critical mistakes
- Alignment with Munger-style investing

Agent MUST validate all conclusions against this checklist.

---

# 🧠 SECTION 1: BUSINESS QUALITY (Moat First)

- [ ] 公司是否具备护城河？
- [ ] 护城河类型明确（成本 / 技术 / 网络 / 品牌 / 切换成本）
- [ ] 护城河是否在增强（而不是减弱）
- [ ] 行业是否长期能赚钱（非结构性内卷）

### 🚨 Kill Conditions
- [ ] 无护城河 → 标记高风险
- [ ] 行业长期价格战 → 降级

---

# 💰 SECTION 2: RETURN ON CAPITAL (ROIC Core)

- [ ] ROIC ≥ 15%（优秀公司标准） :contentReference[oaicite:1]{index=1}
- [ ] ROIC 过去3–5年稳定或提升
- [ ] ROIC 高于资本成本

### 🚨 Kill Conditions
- [ ] ROIC < 10% → 基本排除
- [ ] ROIC波动剧烈 → 可能周期股

---

# 💵 SECTION 3: CASH FLOW QUALITY

- [ ] 自由现金流（FCF）长期为正
- [ ] 经营现金流 ≥ 净利润
- [ ] 不依赖持续融资

📌 原则：
> 利润可以造假，现金流很难造假 :contentReference[oaicite:2]{index=2}

### 🚨 Kill Conditions
- [ ] FCF长期为负
- [ ] 利润增长但现金流恶化

---

# 📈 SECTION 4: GROWTH QUALITY

- [ ] 增长来源清晰：
  - [ ] 价格驱动（优）
  - [ ] 需求增长（中）
  - [ ] 资本扩张（弱）
- [ ] 增长不依赖持续烧钱
- [ ] 增长逻辑可持续（≥3年）

### 🚨 Kill Conditions
- [ ] 增长完全依赖融资/扩张
- [ ] 需求不可持续

---

# 🏭 SECTION 5: INDUSTRY STRUCTURE

- [ ] 行业集中度合理（非完全竞争）
- [ ] 产业链利润分配合理
- [ ] 公司有定价权（或部分定价权）

### 🚨 Kill Conditions
- [ ] 行业极度内卷
- [ ] 客户掌握全部议价权

---

# 👤 SECTION 6: MANAGEMENT QUALITY

- [ ] 管理层资本配置理性
- [ ] 无频繁低效并购
- [ ] 股东友好（分红 / 回购）

### 🚨 Kill Conditions
- [ ] 持续稀释股东
- [ ] 重大治理问题

---

# ⚖️ SECTION 7: VALUATION DISCIPLINE（回报率视角）

- [ ] 当前价格隐含增长合理
- [ ] 未来3年 IRR ≥ 12–15%
- [ ] 不依赖“估值提升”赚钱

📌 原则：
> 投资回报 = 企业回报 + 估值变化

### 🚨 Kill Conditions
- [ ] 需要极高增长才能合理
- [ ] 明显依赖市场情绪

---

# 🧠 SECTION 8: EXPECTATION GAP（预期差）

- [ ] 市场一致预期清晰
- [ ] 已识别市场错误点
- [ ] 存在未被定价变量

### 🚨 Kill Conditions
- [ ] 完全一致预期（无预期差）
- [ ] 逻辑已被市场充分定价

---

# ⚠️ SECTION 9: RISK & FRAGILITY

- [ ] 无单点依赖（客户 / 产品 / 政策）
- [ ] 无致命监管风险
- [ ] 无高杠杆风险

### 🚨 Kill Conditions
- [ ] 单一客户占比过高
- [ ] 高负债 + 现金流差

---

# 🔬 SECTION 10: FALSIFIABILITY（证伪机制）

必须明确：

- [ ] 哪些变量出错 → 投资逻辑失败
- [ ] 哪些指标必须跟踪

---

# 📊 SECTION 11: SCORING SYSTEM（用于Agent）

每项评分（0–5）：

| 维度 | 权重 |
|------|------|
| 护城河 | 25% |
| ROIC | 20% |
| 现金流 | 15% |
| 增长质量 | 15% |
| 行业结构 | 10% |
| 管理层 | 5% |
| 估值 | 10% |

👉 输出：

- ≥ 4.0 → A级（可重仓）
- 3.0–4.0 → B级（跟踪）
- < 3.0 → C级（回避）

---

# 🚨 FINAL DECISION RULE（强制执行）

Agent MUST:

1. 如果 ≥2个 Kill Conditions 被触发：
   → 自动降级为“回避”

2. 如果：
   - ROIC高 + FCF强 + 有护城河
   → 才允许“重仓”

3. 否则：
   → 默认“观望”

---

# 🧠 CORE PHILOSOPHY

> Avoid stupidity, then seek quality, then consider price.

---

# 🔥 FINAL CHECK（必须回答）

- [ ] 如果你是芒格，会不会重仓？
- [ ] 如果错了，最可能错在哪？
