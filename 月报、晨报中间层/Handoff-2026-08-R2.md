SCHEMA_NAME: Monthly-to-Daily Research Handoff
SCHEMA_VERSION: 2.0
STATUS: FROZEN
HANDOFF_ID: 2026-08-R2
HANDOFF_PERIOD: 2026-08
CREATED_AT: 2026-08-21T11:37:30+08:00
VALID_FROM: 2026-08-21T12:07:58+08:00
SNAPSHOT_CUTOFF: 2026-08-20T09:25:50+08:00
VALID_UNTIL: SUPERSEDED
SUPERSEDES_HANDOFF: [INV-MONTHLY][HANDOFF] Monthly-to-Daily Research Handoff v1.2
CORRECTIVE_REBASELINE: TRUE

SOURCE_MONTHLY_FINAL_SUBJECT: [INV-MONTHLY][FINAL] 2026-07 | 投资与宏观月度复盘
SOURCE_MONTHLY_FINAL_PERIOD: 2026-07
SOURCE_MONTHLY_FINAL_VERSION: Final v1.0
MONTHLY_AUTHORITY_VERSION: 2.0
AUDIT_SPEC_VERSION: Audit Ledger v2.0

ABSORBED_DAILY_EVENTS:
- [INV-AUDIT][DAILY] 2026-08-19 / 20260819-METHOD-01
- [INV-AUDIT][DAILY] 2026-08-19 / 20260819-US-RATES-01
- [INV-AUDIT][DAILY] 2026-08-19 / 20260819-AI-VALUATION-01
- [INV-AUDIT][DAILY] 2026-08-20 / 20260820-US-RATES-01-C1
- [INV-AUDIT][DAILY] 2026-08-20 / 20260820-AI-VALUATION-01-S1

## BASELINE_SEED

CHINA_AGGREGATE_DEMAND:
  state: 总需求不足，消费、房地产、民间投资和私人部门信用尚未形成同步改善
  confidence: MEDIUM_HIGH

CHINA_STRUCTURAL_GROWTH:
  state: 高技术制造、出口和部分产业投资保持结构性韧性，但尚未扩散为全面修复
  confidence: HIGH

US_GROWTH:
  state: 增长明显降温，但尚未确认衰退；首次就业数据必须结合修订、失业率、参与率、工时和工资判断
  confidence: MEDIUM

US_LONG_END:
  state: 财政供给、期限溢价和能源通胀风险使长端利率易维持高位震荡，但财政部回购构成政策缓冲变量
  confidence: MEDIUM_LOW

AI_INDUSTRY:
  state: 产业需求仍强，研究重点已进入资本效率、现金流、融资成本与估值纪律验证阶段
  confidence: HIGH_ON_DEMAND_MEDIUM_ON_ASSET_RETURN

## SCENARIO_SEED

PRIMARY_STATE:
  state: STRUCTURAL_GROWTH_WITH_WEAK_AGGREGATE_DEMAND_AND_TIGHT_PRICING_DISCIPLINE
  description: 中国结构增长与总需求偏弱并存；美国增长降温未确认衰退；AI强需求与高资本效率要求并存。

WATCH_STATES:
- US_GROWTH_COOLING_NO_RECESSION_CONFIRMED
- US_LONG_END_HIGH_VOLATILITY_WITH_POLICY_BUFFER
- AI_STRONG_DEMAND_VALUATION_DISCIPLINE

## ACTIVE_THESES

THESIS_01:
  topic: 中国扩散宽度
  thesis: 产业强势不等于总量经济修复，必须验证私人需求是否跨领域持续改善。
  status: ACTIVE
  source_final_section: 中国宏观基线与扩散宽度

THESIS_02:
  topic: AI资本效率
  thesis: AI需求真实性已得到较强验证，下一阶段核心是收入、FCF、ROIC、融资成本与估值能否消化高Capex。
  status: ACTIVE
  source_final_section: AI产业与资产定价

THESIS_03:
  topic: 美国长端利率
  thesis: 增长降温不必然带来长端收益率持续下行，财政、期限溢价、能源风险与政策回购需分层判断。
  status: ACTIVE
  source_pointer: [INV-AUDIT][DAILY] 2026-08-19 / 2026-08-20

## ACTIVE_FORECAST_REFERENCES

- forecast_id: 20260819-US-RATES-01
  origin_daily: [INV-AUDIT][DAILY] 2026-08-19
  evaluation_window: 2026-08-19 -> 2026-09-16
  status: OPEN
  latest_confidence: MEDIUM_LOW
  latest_confidence_event: 20260820-US-RATES-01-C1
  authority_note: 原始 judgment、direction、validation、invalidation 与 evaluation_rule 仍以 2026-08-19 Daily Ledger 为准

## OPEN_HYPOTHESES

HYPOTHESIS_01:
  hypothesis_id: 20260819-AI-VALUATION-01
  topic: AI资本效率与估值
  hypothesis: AI产业需求仍强，但资产定价的边际约束正转向资本效率、融资成本和估值能否消化高Capex。
  status: STRENGTHENED
  source_pointer: [INV-AUDIT][DAILY] 2026-08-19 / 2026-08-20

HYPOTHESIS_02:
  topic: 中国私人需求扩散
  hypothesis: 中国私人需求能否从产业增长向消费、地产、民间投资和私人信用扩散。
  status: OPEN
  source_pointer: [INV-MONTHLY][FINAL] 2026-07 | 投资与宏观月度复盘

HYPOTHESIS_03:
  topic: 美国增长质量
  hypothesis: 美国是否进入低就业增长但生产率与利润仍能维持的增长状态。
  status: OPEN
  source_pointer: [INV-MONTHLY][FINAL] 2026-07 | 投资与宏观月度复盘

## ACTIVE_METHOD_REFERENCES

- method_event_id: 20260819-METHOD-01
  source_daily: [INV-AUDIT][DAILY] 2026-08-19
  summary: 美国宏观四层拆分、就业修订风险、中国扩散宽度、AI三层框架、二元事件情景树、置信度绑定具体Forecast。

## VALIDATION_TARGETS

VT_01:
  target: 中国产业增长是否扩散到私人需求
  linked_thesis: THESIS_01
  indicators: 消费、房地产销售与融资、民间投资、居民与企业中长期信用
  positive_confirmation: 至少两个私人需求领域跨月持续改善
  disconfirming_signal: 结构产业强势继续与私人需求恶化并存
  baseline_relevance: 中国总需求基线

VT_02:
  target: 美国增长降温是否升级为衰退状态
  linked_hypothesis: HYPOTHESIS_03
  indicators: 就业修订、工时、失业率、参与率、实际消费、企业招聘、利润与信用条件
  positive_confirmation: 多个增长与就业质量指标同步持续恶化
  disconfirming_signal: 低就业增长但消费、利润和生产率保持稳定
  baseline_relevance: 美国增长基线

VT_03:
  target: AI Capex能否转化为持续资本回报
  linked_thesis: THESIS_02
  linked_hypothesis: HYPOTHESIS_01
  indicators: AI Revenue / Capex、FCF、利润率、ROIC、债务融资成本、财报后价格反应
  positive_confirmation: 收入、FCF与ROIC持续快于资本投入改善
  disconfirming_signal: Capex与融资继续扩张但现金流、利润率和价格反应同步恶化
  baseline_relevance: AI产业与资产定价先验

VT_04:
  target: 美国长端利率Forecast的剩余评价窗口
  linked_thesis: THESIS_03
  indicators: 10年/30年收益率、期限溢价、实际利率、能源价格、长债拍卖、财政部回购
  positive_confirmation: 长端保持高位震荡并持续受财政、期限溢价或能源风险影响
  disconfirming_signal: 长端持续显著下行且财政与能源约束未形成影响
  baseline_relevance: 20260819-US-RATES-01

## STATE_TRANSITION_TRIGGERS

TRIGGER_01:
  from_state: CHINA_STRUCTURAL_GROWTH_WEAK_AGGREGATE_DEMAND
  watch_state: CHINA_PRIVATE_DEMAND_BROADENING
  condition: 消费、地产、民间投资、私人信用中至少两个领域跨月同步改善
  confirmation: 排除单次补贴、基数效应或单月噪音
  affected_baseline: CHINA_AGGREGATE_DEMAND

TRIGGER_02:
  from_state: US_GROWTH_COOLING_NO_RECESSION_CONFIRMED
  watch_state: US_RECESSION_RISK_RISING
  condition: 就业质量、实际消费、利润和信用条件出现多维同步恶化
  confirmation: 修订后数据与至少两个非就业指标一致
  affected_baseline: US_GROWTH

## ASSET_PRICING_PRIORS

AI_ASSETS:
  fundamentals: 产业需求保持强势
  earnings_or_cashflow: 需要继续验证收入、FCF与ROIC是否覆盖高Capex
  valuation_constraint: 长端利率、融资成本与高估值
  dominant_drivers: 资本效率、折现率、财报兑现
  price_direction_confidence: LOW_TO_MEDIUM

US_LONG_END:
  fundamentals: 增长降温但财政供给与能源风险仍在
  earnings_or_cashflow: NOT_APPLICABLE
  valuation_constraint: 期限溢价与政策回购缓冲
  dominant_drivers: 财政供给、期限溢价、能源通胀、财政部回购、增长数据
  price_direction_confidence: MEDIUM_LOW

GOLD:
  fundamentals: 中期结构性支撑仍在
  earnings_or_cashflow: NOT_APPLICABLE
  valuation_constraint: 实际利率与美元
  dominant_drivers: 实际利率、美元、风险溢价
  price_direction_confidence: MEDIUM

## MORNING_RESEARCH_PRIORITIES

1. 验证中国产业增长是否扩散到私人需求。
2. 提高对美国就业质量、数据修订与增长状态的研究权重。
3. 提高对AI资本效率、融资成本与折现率的研究权重。
4. 继续跟踪仍在评价窗口内的美国长端利率Forecast。

## RESEARCH_PRIORITY_DIRECTIVES

DIRECTIVE_01:
  topic_key: china_private_demand
  topic: 中国私人需求扩散
  action: MAINTAIN
  reason: 产业强势尚未扩散为消费、地产、民间投资和私人信用的同步改善
  focus_on:
  - 跨领域、跨月的私人需求改善证据
  review_window: 2026-Q4
  source_final_section: 中国宏观基线与扩散宽度

DIRECTIVE_02:
  topic_key: us_growth_quality
  topic: 美国增长质量
  action: RAISE
  reason: 首次就业数据与后续修订暴露出增长韧性判断的数据版本风险
  focus_on:
  - 就业修订、工时、失业率和参与率
  - 消费、利润、招聘与信用条件是否同步走弱
  review_window: 2026-Q4
  source_final_section: 美国增长与数据版本风险

DIRECTIVE_03:
  topic_key: ai_capital_efficiency
  topic: AI资本效率与估值
  action: RAISE
  reason: 需求真实性较强，下一阶段判别价值来自FCF、ROIC、融资成本和折现率
  focus_on:
  - AI Revenue / Capex、FCF、利润率与ROIC
  - 债务融资成本与财报后价格反应
  stop_researching:
  - 重复证明AI基础设施需求是否真实
  review_window: 2026-Q4
  source_final_section: AI产业与资产定价

## KNOWN_UNCERTAINTIES

- 美国低就业增长究竟意味着衰退风险上升，还是生产率提高下的增长结构变化。
- 财政部回购对长端期限溢价的缓冲能否持续。
- AI强需求能否稳定转化为FCF和ROIC，而不是主要体现为更高资本密集度。
- 中国结构产业增长能否跨越政策脉冲并扩散到私人部门。

## DO_NOT_CARRY_FORWARD

- 不把AI产业景气直接等同于所有AI资产上涨。
- 不把单次就业初值直接等同于美国经济趋势。
- 不把中国局部产业强势解释为全面复苏。
- 不把通胀下降机械映射为长端利率持续下行或资产全面上涨。
