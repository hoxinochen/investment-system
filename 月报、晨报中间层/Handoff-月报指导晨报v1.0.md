SCHEMA_NAME: Monthly-to-Daily Research Handoff  
SCHEMA_VERSION: 1.0  
STATUS: FROZEN  
AUTHORITY_ROLE: Handoff Schema Authority  
SCOPE: Monthly Final -> Daily Morning Research Continuity  
RETENTION_POLICY: PERMANENT  
  
============================================================  
1. PURPOSE  
============================================================  
本 Schema 规定 Monthly-to-Daily Research Handoff（月报→晨报研究交接快照）的结构、边界、激活方式与读取规则。  
  
核心目标：  
- 把已冻结 Monthly Final（月度正式研究报告）的最终研究状态压缩成下一阶段晨报可直接消费的 Research Prior（研究先验）；  
- 避免晨报每天重新读取整篇 Monthly Final；  
- 避免研究连续性依赖某一个 Chat 的上下文；  
- 与 Daily Audit Ledger（日度审计账本）共同形成 Snapshot + Event Stream（快照 + 事件流）的状态重建机制。  
  
Handoff 保存的是“进入下一阶段时从什么世界模型开始工作”，不是上月全文摘要，也不是新的预测记录。  
  
============================================================  
2. AUTHORITY BOUNDARY  
============================================================  
Handoff IS:  
- Frozen Research Prior derived from an approved Monthly Final  
- Starting State for the next daily research cycle  
- Compact operational projection of the Monthly Final  
  
Handoff IS NOT:  
- Monthly Research Methodology Authority  
- Daily Morning Report Prompt / Method Authority  
- Audit Protocol Authority  
- Prediction Record Authority  
- Reality Authority  
  
权威关系：  
- [INV-MONTHLY][AUTHORITY]：规定月报如何研究；  
- [INV-MONTHLY][FINAL]：保存经过 Challenge Phase 后冻结的正式月度研究成果；  
- [INV-MONTHLY][HANDOFF]：从 Monthly Final 提取下一阶段的起始研究状态；  
- [INV-AUDIT][SPEC]：规定日度研究事件如何记录；  
- [INV-AUDIT][DAILY]：保存正式 Forecast/Baseline/Hypothesis/Method 等事件历史。  
  
Handoff 不得覆盖、修改或重新定义上述任何 Authority。  
  
============================================================  
3. CREATION & ACTIVATION  
============================================================  
只有来源 Monthly Final 已经明确处于 FINAL_FROZEN 状态后，才允许创建对应 Handoff。  
  
Handoff 生命周期：  
DRAFT -> 用户审阅/批准 -> FROZEN  
  
只有 STATUS: FROZEN 的 Handoff 才能作为晨报 Research Prior 使用。  
  
Subject 严格格式：  
[INV-MONTHLY][HANDOFF] YYYY-MM  
  
其中 YYYY-MM 表示 Handoff 被激活的自然月，不代表从该月1日开始生效。  
  
必须使用真实时间：  
CREATED_AT: 实际创建时间  
VALID_FROM: 实际批准/激活时间  
VALID_UNTIL: SUPERSEDED  
  
严禁为使月份整齐而把 VALID_FROM 回填到月初。  
  
新 Handoff 激活后，旧 Handoff 不删除、不修改；旧 Handoff 的有效区间自然截止于新 Handoff 的 VALID_FROM。  
  
============================================================  
4. REQUIRED METADATA  
============================================================  
每封正式 Handoff 至少包含：  
  
SCHEMA_NAME: Monthly-to-Daily Research Handoff  
SCHEMA_VERSION: 1.0  
STATUS: FROZEN  
HANDOFF_PERIOD: YYYY-MM  
CREATED_AT: ISO-8601 timestamp  
VALID_FROM: ISO-8601 timestamp  
VALID_UNTIL: SUPERSEDED  
  
SOURCE_MONTHLY_FINAL: [INV-MONTHLY][FINAL] YYYY-MM  
SOURCE_MONTHLY_FINAL_VERSION:  
MONTHLY_AUTHORITY_VERSION:  
AUDIT_SPEC_VERSION:  
  
不得从未冻结 Review Draft 直接生成正式 Handoff。  
  
============================================================  
5. BASELINE_SEED  
============================================================  
BASELINE_SEED 保存 Handoff 激活时的 Starting World Model（起始世界模型）。  
  
推荐按可独立变化的维度拆分，而不是只保存一句总判断，例如：  
  
BASELINE_SEED:  
CHINA_AGGREGATE_DEMAND:  
  state:  
  confidence:  
CHINA_STRUCTURAL_GROWTH:  
  state:  
  confidence:  
US_GROWTH:  
  state:  
  confidence:  
US_INFLATION:  
  state:  
  confidence:  
FED_REACTION_FUNCTION:  
  state:  
  confidence:  
US_LONG_END:  
  state:  
  confidence:  
AI_INDUSTRY:  
  state:  
  confidence:  
  
可根据当月实际研究状态增加/删减维度；不得为了模板完整而硬填无意义字段。  
  
============================================================  
6. SCENARIO_SEED  
============================================================  
保存 Monthly Final 冻结时的 Current Scenario State（当前情景状态）及值得监控的迁移方向。  
  
SCENARIO_SEED:  
PRIMARY_STATE:  
  state:  
  description:  
WATCH_STATES:  
  - ...  
  
不要求复制 Monthly Final 中完整 Scenario Tree（情景树）；只保存晨报下一阶段需要继承的当前状态与关键 Watch States（观察状态）。  
  
============================================================  
7. ACTIVE_THESES  
============================================================  
只保存下一阶段仍有决策价值的核心研究结论。  
  
ACTIVE_THESES:  
THESIS_01:  
  topic:  
  thesis:  
  status: ACTIVE  
  source_final_section:  
  
THESIS 是 Monthly Final 的研究综合结论，不自动等于正式 Forecast。  
  
不得把已经结束、仅解释历史波动、或对下一阶段无研究价值的结论继续携带。  
  
============================================================  
8. ACTIVE_FORECAST_REFERENCES  
============================================================  
用于携带在 Handoff 激活时仍处于有效评价窗口、尚未完成评价的正式 Forecast 引用，避免晨报重复创建同一预测。  
  
ACTIVE_FORECAST_REFERENCES:  
- forecast_id:  
  origin_daily:  
  evaluation_window:  
  status: OPEN  
  
这里只保存引用与必要状态，不复制或重写 Forecast 原文。  
  
Prediction Authority 始终是对应 [INV-AUDIT][DAILY] 原始记录；Handoff 不得改变 forecast judgment、direction、confidence、evaluation_rule 或 invalidation。  
  
若后续 Forecast 置信度或状态变化，必须通过新的 Daily Ledger Event 引用原 Forecast ID 表达。  
  
============================================================  
9. OPEN_HYPOTHESES  
============================================================  
保存月底仍未解决、且下一阶段值得继续检验的机制假说。  
  
OPEN_HYPOTHESES:  
HYPOTHESIS_01:  
  hypothesis_id: OPTIONAL  
  topic:  
  hypothesis:  
  status: OPEN / STRENGTHENED / WEAKENED  
  source_pointer:  
  
若假说源自正式 Daily Ledger，应保留 hypothesis_id 引用；原始 Hypothesis Authority 仍为对应 Daily Ledger。  
  
============================================================  
10. VALIDATION_TARGETS  
============================================================  
VALIDATION_TARGETS 是 Handoff 的核心字段之一。  
  
每个 Validation Target（验证目标）应围绕一个研究问题，而不是单独一个数据点。  
  
VALIDATION_TARGETS:  
VT_01:  
  target:  
  linked_thesis:  
  linked_hypothesis: OPTIONAL  
  indicators:  
    - ...  
  positive_confirmation:  
  disconfirming_signal:  
  baseline_relevance:  
  
推荐保留 3-10 个真正重要的验证目标；研究价值优先于数量。  
  
============================================================  
11. STATE_TRANSITION_TRIGGERS  
============================================================  
保存 Monthly Final 预先定义、供晨报识别状态迁移的关键触发条件。  
  
STATE_TRANSITION_TRIGGERS:  
TRIGGER_01:  
  from_state:  
  watch_state:  
  condition:  
  confirmation:  
  affected_baseline:  
  
晨报必须区分：  
NOISE（噪音） / TRIGGER（触发） / CONFIRMATION（确认）。  
  
单项新数据通常只能触发 Watch，不应自动重写 Baseline，除非满足 Handoff / Monthly Final 已定义的确认条件，或出现足以构成正式 BASELINE_CHANGED 的新证据。  
  
============================================================  
12. ASSET_PRICING_PRIORS  
============================================================  
保存进入下一阶段时的重要资产定价先验，而不是买卖指令。  
  
ASSET_PRICING_PRIORS:  
ASSET_OR_THEME:  
  fundamentals:  
  earnings_or_cashflow:  
  valuation_constraint:  
  dominant_drivers:  
    - ...  
  price_direction_confidence:  
  
重点保留 Dominant Driver（主导定价因子）及基本面判断与价格方向置信度之间的区别。  
  
Handoff 不得把资产定价先验升级为新的正式 Forecast；若晨报需要创建 Forecast，仍必须遵守 Audit SPEC。  
  
============================================================  
13. MORNING_RESEARCH_PRIORITIES  
============================================================  
保存下一阶段晨报应优先研究/验证的 3-8 个问题。  
  
MORNING_RESEARCH_PRIORITIES:  
14. ...  
15. ...  
  
这些内容属于 Research Focus（研究重点），不是新的方法论 Authority。  
  
Handoff 不得通过 MORNING_RESEARCH_PRIORITIES 静默修改晨报 Prompt、Audit SPEC、Forecast Schema 或已冻结方法规则。  
  
任何正式 METHOD_CHANGED 仍必须遵守既有治理与 Audit SPEC。  
  
============================================================  
16. KNOWN_UNCERTAINTIES  
============================================================  
明确保存 Monthly Final 尚未解决的重要不确定性，防止晨报把暂时占优的解释当成事实。  
  
KNOWN_UNCERTAINTIES:  
- ...  
  
应优先记录：  
- 主解释与替代解释仍难区分的问题；  
- 数据修订/测量风险；  
- 尚未完成验证的关键传导链；  
- 可能改变资产 Dominant Driver 的变量。  
  
============================================================  
15. DO_NOT_CARRY_FORWARD  
============================================================  
显式记录不应继续污染下一阶段上下文的内容类别或具体对象。  
  
DO_NOT_CARRY_FORWARD:  
- 已完成评价且不再影响当前研究的问题  
- 已关闭/否定的 Hypothesis  
- 已失效且评价完成的短期 Forecast  
- 纯历史新闻与一次性事件  
- 只用于解释上月价格波动、无持续机制意义的叙事  
  
Handoff 的目标是 Research Compression（研究压缩），而不是保存更多文字。  
  
============================================================  
16. CURRENT STATE RECONSTRUCTION  
============================================================  
晨报不得把 Handoff 本身当成永远最新的 Current State（当前状态）。  
  
当前研究状态应按以下方式重建：  
  
Current Research State at T  
= Latest Valid FROZEN Handoff with VALID_FROM <= T  
+ all relevant Daily Ledger Events after that Handoff VALID_FROM and before T  
  
应用顺序按事件真实时间/语义时间进行，并遵守各 Daily Ledger 自身 SPEC_VERSION。  
  
典型增量事件包括：  
- BASELINE_CHANGED  
- BASELINE_CONFIDENCE_CHANGED  
- FORECAST_CREATED  
- FORECAST_CONFIDENCE_CHANGED  
- HYPOTHESIS_CREATED  
- HYPOTHESIS_STATUS_CHANGED  
- METHOD_CHANGED  
  
Handoff 是 Snapshot（快照）；Daily Ledger 是 Event Stream（事件流）。  
  
============================================================  
17. DUPLICATE & CONFLICT HANDLING  
============================================================  
读取 Handoff Schema 时：  
- 0 个有效冻结 Schema -> HANDOFF_SCHEMA_UNAVAILABLE  
- 1 个当前有效冻结 Schema -> 正常读取  
- 多个同版本冻结 Schema -> HANDOFF_SCHEMA_DUPLICATE，fail-closed，不任意选择  
- 多个不同冻结版本 -> 使用语义版本号最高、且声明可用于当前 Handoff 的版本  
  
读取 Handoff 时：  
- 只使用 STATUS: FROZEN  
- 只使用 VALID_FROM <= 当前分析时间的 Handoff  
- 若多个 Handoff 的有效区间冲突且无法通过 VALID_FROM 唯一确定最新状态 -> HANDOFF_AMBIGUOUS，停止使用 Handoff，不自行合并  
  
读取异常不得阻断晨报正文生成。  
  
============================================================  
18. FAILURE HANDLING  
============================================================  
Handoff 属于研究连续性增强层，不得成为晨报正文的单点故障。  
  
若 Schema 或 Handoff 不可用/歧义：  
- 晨报仍必须完成正常的过去24小时研究与输出；  
- 不得假装已读取 Research Prior；  
- 不得用聊天记忆或旧 Handoff 冒充当前 Handoff；  
- 可在晨报末尾简短报告 HANDOFF_SCHEMA_UNAVAILABLE / HANDOFF_UNAVAILABLE / HANDOFF_AMBIGUOUS。  
  
Audit Ledger 持久化仍由 [INV-AUDIT][SPEC] 独立治理，不因 Handoff 失败而改变。  
  
============================================================  
19. IMMUTABILITY & VERSIONING  
============================================================  
Schema 与正式 Handoff 均遵循 write-once-read-many：  
允许 READ / SEARCH；  
禁止 UPDATE / SEND / DELETE / REPLACE / overwrite。  
  
Schema v1.0 冻结后不得原地修改；未来变化创建 v1.1 / v2.0。  
  
已冻结 Handoff 不得更新为“最新状态”。月内状态变化只能进入后续 Daily Ledger；下一次 Monthly Final 冻结后再生成新的 Handoff Snapshot。  
  
============================================================  
20. LANGUAGE & MACHINE READABILITY  
============================================================  
机器字段优先保持英文与稳定枚举。  
  
面向用户显示 Handoff 内容时，专业英文术语第一次出现应提供中文标注，例如：  
Research Prior（研究先验）  
Baseline Seed（基线起始状态）  
Dominant Driver（主导定价因子）  
State Transition Trigger（状态迁移触发器）  
  
============================================================  
21. DESIGN PRINCIPLE  
============================================================  
Monthly Final 保存完整研究成果。  
Handoff 保存下一阶段可执行的研究起点。  
Daily Ledger 保存月内正式状态变化。  
  
因此：  
Monthly Final -> Handoff Snapshot -> Daily Event Stream -> Next Monthly Audit -> Next Monthly Final  
  
目标不是让晨报继承更多文本，而是让研究状态可以在不同 Chat、不同自动任务运行之间可靠重建，同时保持 Prediction Authority、Reality Authority 与 Method Authority 的边界清晰。