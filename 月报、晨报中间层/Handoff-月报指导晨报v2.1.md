SCHEMA_NAME: Monthly-to-Daily Research Handoff
SCHEMA_VERSION: 2.1
STATUS: FROZEN
EFFECTIVE_FROM: 2026-09-16T12:09:41+08:00
SUPERSEDES: Monthly-to-Daily Research Handoff Schema v2.0
DOCUMENT_MODE: FULL
SELF_CONTAINED: TRUE
BASE_VERSION: NONE
AUTHORITY_ROLE: Handoff Producer Contract
SCOPE: Frozen Monthly Final -> Self-contained Daily Research Prior
RETENTION_POLICY: PERMANENT

============================================================  
1. PURPOSE  
============================================================  
本 Schema 规定 Monthly-to-Daily Research Handoff（月报→晨报研究交接快照）的生成结构、边界、激活方式与跨周期执行语义。  
  
核心目标：  
- 把已冻结 Monthly Final 的最终研究状态压缩成下一阶段晨报可直接消费的 Research Prior（研究先验）；  
- 避免晨报每天重读完整 Monthly Final；  
- 避免研究连续性依赖 Chat 上下文；  
- 与 Daily Audit Ledger 共同形成 Snapshot + Event Stream（快照 + 事件流）；  
- 把月度 Attention Calibration（注意力校准）结果编译成有限、明确、可执行的 Priority Directives（研究优先级指令），避免依赖模型自行领会自然语言。  
  
Handoff 保存的是“进入下一阶段时从什么世界模型和研究注意力状态开始工作”，不是月报全文摘要，也不是新的预测记录或方法 Authority。  
  
============================================================  
2. PRODUCER / CONSUMER BOUNDARY  
============================================================  
本 Schema 是 Writer Contract / Producer Contract（写入契约）。  
  
只有 Handoff Generator 在从已冻结 Monthly Final 生成 Handoff 时读取本 Schema。  
  
晨报运行时：  
- 直接读取 Active Handoff；  
- 不读取 [INV-MONTHLY][HANDOFF-SCHEMA]；  
- 不依赖 Schema 才能解释一个已生成的 Handoff；  
- Handoff 必须自包含全部晨报需要执行的字段与语义。  
  
如果未来 Schema 发生不兼容变化，应通过新的 Schema / Morning Prompt 版本治理处理，而不是让晨报运行时反复读取 Schema 来“猜”格式。  
  
============================================================  
3. AUTHORITY BOUNDARY  
============================================================  
Handoff IS:  
- Frozen Research Prior derived from an approved Monthly Final  
- Starting State for the next daily research cycle  
- Compact operational projection of the Monthly Final  
- Initial research-attention state at VALID_FROM  
  
Handoff IS NOT:  
- Monthly Research Methodology Authority  
- Daily Morning Report Method Authority  
- Audit Protocol Authority  
- Prediction Record Authority  
- Reality Authority  
  
权威关系：  
- [INV-MONTHLY][AUTHORITY]：规定月报如何研究、如何做 Attention Calibration；  
- [INV-MONTHLY][FINAL]：保存经过 Challenge Phase 后冻结的正式月度研究成果；  
- 本 Schema：规定如何把 Final 编译为 Handoff；  
- [INV-MONTHLY][HANDOFF]：晨报直接消费的自包含快照；  
- [INV-AUDIT][SPEC]：规定日度事件如何记录；  
- [INV-AUDIT][DAILY]：保存正式 Forecast / Baseline / Hypothesis / Method / Research Priority 等事件历史。  
  
Handoff 不得覆盖、修改或重新定义上述任何 Authority。  
  
============================================================  
4. CREATION & ACTIVATION  
============================================================  
只有来源 Monthly Final 明确处于 FINAL_FROZEN 后，才允许创建对应 Handoff。  
  
Handoff 生命周期：DRAFT -> 用户审阅 / 批准 -> FROZEN。  
只有 STATUS: FROZEN 的 Handoff 可作为晨报 Research Prior。  
  
Subject 格式：
[INV-MONTHLY][HANDOFF] YYYY-MM Rn

YYYY-MM 表示激活所在自然月，Rn 表示同月第几份交接快照，例如 R1、R2。
Consumer 先校验固定命名空间和年月，再按 VALID_FROM 选择最新有效记录；显示标题和本地文件名不参与识别。

Monthly Final Subject 格式：
[INV-MONTHLY][FINAL] YYYY-MM | 可变的人类标题

识别正则：
^\[INV-MONTHLY\]\[FINAL\]\s+\d{4}-\d{2}(?:\s+\|\s+.+)?$

Final 正文必须包含 ARTIFACT_TYPE: MONTHLY_FINAL、STATUS: FINAL_FROZEN、REPORT_PERIOD。  
  
必须使用真实、带时区的 ISO-8601 时间：
CREATED_AT: 实际创建时间
VALID_FROM: 实际批准 / 激活时间
SNAPSHOT_CUTOFF: 已纳入快照的最后事件时间
VALID_UNTIL: SUPERSEDED
SUPERSEDES_HANDOFF: 被替代 Handoff 的 Subject；无则写 NONE  
  
严禁为了月份整齐回填 VALID_FROM。  
新 Handoff 激活后，旧 Handoff 不删除、不修改；旧 Handoff 在新 VALID_FROM 后被 supersede。  
  
============================================================  
5. REQUIRED METADATA  
============================================================  
每封正式 Handoff 至少包含：  
  
SCHEMA_NAME: Monthly-to-Daily Research Handoff  
SCHEMA_VERSION: 2.1
STATUS: FROZEN
HANDOFF_ID:
HANDOFF_PERIOD: YYYY-MM
CREATED_AT: ISO-8601 timestamp
VALID_FROM: ISO-8601 timestamp
SNAPSHOT_CUTOFF: ISO-8601 timestamp
VALID_UNTIL: SUPERSEDED
SUPERSEDES_HANDOFF:

SOURCE_MONTHLY_FINAL_SUBJECT:
SOURCE_MONTHLY_FINAL_PERIOD:
SOURCE_MONTHLY_FINAL_VERSION:
MONTHLY_AUTHORITY_VERSION:
AUDIT_SPEC_VERSION:  
  
不得从未冻结 Review Draft 生成正式 Handoff。  
  
============================================================  
6. BASELINE_SEED  
============================================================  
保存 Handoff 激活时的 Starting World Model。按可独立变化维度拆分，不为模板完整硬填。  
  
示意：  
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
  
============================================================  
7. SCENARIO_SEED  
============================================================  
只保存晨报下一阶段需要继承的当前情景与关键 Watch State，不复制完整情景树。  
  
SCENARIO_SEED:  
PRIMARY_STATE:  
  state:  
  description:  
WATCH_STATES:  
  - ...  
  
============================================================  
8. ACTIVE_THESES  
============================================================  
只保存下一阶段仍有决策 / 研究价值的核心结论。  
  
ACTIVE_THESES:  
THESIS_01:  
  topic:  
  thesis:  
  status: ACTIVE  
  source_final_section:  
  
THESIS 不自动等于 Forecast。  
  
============================================================  
9. ACTIVE_FORECAST_REFERENCES  
============================================================  
只引用 Handoff 激活时仍处于评价窗口、尚未完成评价的正式 Forecast。  
  
ACTIVE_FORECAST_REFERENCES:  
- forecast_id:  
  origin_daily:  
  evaluation_window:  
  status: OPEN  
  
Prediction Authority 始终是原始 Daily Ledger。Handoff 不得改变 Forecast 的 judgment、direction、confidence、evaluation_rule、invalidation 或生命周期。  
  
============================================================  
10. OPEN_HYPOTHESES  
============================================================  
保存月底仍未解决、且下一阶段值得继续检验的机制假说。  
  
OPEN_HYPOTHESES:  
HYPOTHESIS_01:  
  hypothesis_id: OPTIONAL  
  topic:  
  hypothesis:  
  status: OPEN / STRENGTHENED / WEAKENED  
  source_pointer:  
  
若源自 Daily Ledger，应保留原 hypothesis_id。  
  
============================================================  
11. VALIDATION_TARGETS  
============================================================  
围绕研究问题而非单独数据点，推荐 3–10 个真正重要目标。  
  
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
  
============================================================  
12. STATE_TRANSITION_TRIGGERS  
============================================================  
保存供晨报识别状态迁移的关键触发条件。  
  
STATE_TRANSITION_TRIGGERS:  
TRIGGER_01:  
  from_state:  
  watch_state:  
  condition:  
  confirmation:  
  affected_baseline:  
  
晨报必须区分 NOISE / TRIGGER / CONFIRMATION。单项数据通常只能进入 Watch，除非满足确认规则或足以形成正式 BASELINE_CHANGED。  
  
============================================================  
13. ASSET_PRICING_PRIORS  
============================================================  
保存重要资产定价先验，不是买卖指令。  
  
ASSET_PRICING_PRIORS:  
ASSET_OR_THEME:  
  fundamentals:  
  earnings_or_cashflow:  
  valuation_constraint:  
  dominant_drivers:  
    - ...  
  price_direction_confidence:  
  
必须保留 Dominant Driver 与基本面判断 / 价格方向置信度的区别。Handoff 不得把定价先验升级成新的 Forecast。  
  
============================================================  
14. MORNING_RESEARCH_PRIORITIES  
============================================================  
保存下一阶段晨报优先研究 / 验证的问题摘要，推荐 3–8 项。  
  
MORNING_RESEARCH_PRIORITIES:  
15. ...  
16. ...  
  
该字段是 Human-readable Summary（人类可读摘要），不是执行指令 Authority。  
  
如果 MORNING_RESEARCH_PRIORITIES 与 RESEARCH_PRIORITY_DIRECTIVES 在注意力分配上发生冲突，以 RESEARCH_PRIORITY_DIRECTIVES 为准。  
  
============================================================  
17. RESEARCH_PRIORITY_DIRECTIVES  
============================================================  
该字段把 Monthly Attention Calibration 的结果编译成晨报可以稳定执行的有限控制指令。  
  
严禁用自由文本发明新的 action。action 只允许：  
- ADD  
- RAISE  
- MAINTAIN  
- LOWER  
- EVENT_TRIGGER_ONLY  
- REFOCUS  
- DROP  
  
通用结构：  
  
RESEARCH_PRIORITY_DIRECTIVES:  
DIRECTIVE_01:  
  topic_key:  
  topic:  
  action:  
  reason:  
  focus_on:  
    - ...  
  stop_researching:  
    - ...  
  trigger_condition:  
  reentry_condition:  
  review_window:  
  source_final_section:  
  
字段要求按 action 变化，禁止为模板完整硬填。  
  
15.1 ADD  
含义：把此前不属于当前常规研究重点的主题加入主动研究池。  
最低要求：topic_key, topic, action, reason, focus_on, review_window。  
  
15.2 RAISE  
含义：提高现有主题的相对研究权重；信息预算冲突时优先于普通主题。  
最低要求：topic_key, action, reason, focus_on, review_window。  
不得因此自动提高 Forecast / Baseline / Hypothesis 置信度。  
  
15.3 MAINTAIN  
含义：月度审计确认现有研究权重合理，继续保持。  
最低要求：topic_key, action, reason, review_window。  
MAINTAIN 不等于“永久继承”。下一月仍需重新取得资格。  
  
15.4 LOWER  
含义：主题仍保留在主动研究池，但降低相对权重。  
最低要求：topic_key, action, reason, review_window。  
  
15.5 EVENT_TRIGGER_ONLY  
含义：退出常规主动研究，不再为了该主题每日主动扩展；只有 trigger_condition 满足时才重新展开研究。  
最低要求：topic_key, action, reason, trigger_condition, review_window。  
  
15.6 REFOCUS  
含义：主题继续保留，但研究问题必须改变；用于停止重复充分性证据，转向判别性证据、竞争解释、反事实或新的验证问题。  
最低要求：topic_key, action, reason, focus_on, stop_researching, review_window。  
晨报不得继续把 stop_researching 中的问题作为该主题的主要研究任务，除非出现真正的新机制证据。  
  
15.7 DROP  
含义：主题退出下一周期常规研究池。普通新闻、重复行情或单条低强度信息不得自动使其重新进入。  
最低要求：topic_key, action, reason, reentry_condition。  
满足 reentry_condition 后，晨报仍应依据当时有效 Audit SPEC 创建新的 RESEARCH_PRIORITY_CHANGED，才把该主题重新纳入跨日研究状态。  
  
============================================================  
16. PRIORITY DIRECTIVE EXECUTION BOUNDARY  
============================================================  
Priority Directive 可以改变：  
- 研究对象是否进入主动研究池；  
- 相对研究权重；  
- 当前需要验证的问题；  
- 哪些重复问题应停止研究；  
- 事件触发 / 退出 / 重新进入条件。  
  
Priority Directive 不得改变：  
- Forecast Schema；  
- Audit 写入协议；  
- 证据等级标准；  
- Reality Authority；  
- Prediction Authority；  
- Monthly / Morning Method Authority；  
- 已冻结历史记录。  
  
任何真正的方法变化必须走 METHOD_CHANGED 及对应治理，不得通过 Handoff 偷渡。  
  
============================================================  
17. DAILY EVENT PRECEDENCE  
============================================================  
Handoff Priority Directives 是截至 Event Replay Boundary 已吸收的初始注意力状态，不是永久命令。  
  
Current Research Priority at T  
= Active Handoff RESEARCH_PRIORITY_DIRECTIVES  
+ all relevant RESEARCH_PRIORITY_CHANGED events after Event Replay Boundary and before T  

Event Replay Boundary 使用 `SNAPSHOT_CUTOFF`；仅对缺少该字段的历史 Handoff 回退使用 `VALID_FROM`。
  
对于同一 topic_key：  
- Event Replay Boundary 之后更晚的正式 Daily RESEARCH_PRIORITY_CHANGED 覆盖 Handoff 初始 Directive；  
- 新 Handoff 激活后，以新 Handoff 为新的 Snapshot 起点，再叠加其后的 Daily Events。  
  
Reality Evidence 始终可以触发新的研究状态事件；DROP / EVENT_TRIGGER_ONLY 不是禁止观察现实，而是限制常规主动研究预算。  
  
============================================================  
18. KNOWN_UNCERTAINTIES  
============================================================  
保存尚未解决的重要不确定性，防止晨报把暂时占优的解释当事实。  
  
KNOWN_UNCERTAINTIES:  
- ...  
  
优先记录主解释 vs 替代解释、数据修订风险、尚未验证的传导链、可能改变 Dominant Driver 的变量。  
  
============================================================  
19. DO_NOT_CARRY_FORWARD  
============================================================  
显式记录不应继续污染下一阶段研究上下文的内容，例如：已完成评价的问题、已关闭假说、已失效 Forecast、纯历史一次性新闻、无持续机制意义的旧叙事。  
  
DO_NOT_CARRY_FORWARD 负责 Research Compression / Context Garbage Collection，不负责研究优先级执行。  
  
如果一个主题需要明确退出主动研究，必须同时存在 RESEARCH_PRIORITY_DIRECTIVES 中的 DROP 或 EVENT_TRIGGER_ONLY；晨报不得仅凭 DO_NOT_CARRY_FORWARD 猜测注意力动作。  
  
============================================================
19A. CORRECTIVE HANDOFF / REBASELINE
============================================================
正常情况下每个 Monthly Final 生成一次新 Handoff，月内变化写入 Daily Ledger，不重复生成 Handoff。

只有旧 Handoff 漏掉仍有效 Forecast / Hypothesis / Priority、存在时间排序歧义或格式无法消费时，才创建 Corrective Handoff。不得修改旧 Handoff。

Corrective Handoff 必须：
- 声明 SUPERSEDES_HANDOFF；
- 使用精确 VALID_FROM 与 SNAPSHOT_CUTOFF；
- 吸收旧 Handoff 至 SNAPSHOT_CUTOFF 之间的全部有效 Daily Events；
- 保存仍在评价窗口内的 Forecast ID 与最新置信度；
- 保存 Hypothesis ID 与最新状态；
- 将 Priority 写成结构化 RESEARCH_PRIORITY_DIRECTIVES。

============================================================
20. CURRENT STATE RECONSTRUCTION  
============================================================  
Current Research State at T  
= Latest Valid FROZEN Handoff with VALID_FROM <= T  
+ all relevant Daily Ledger Events after that Handoff Event Replay Boundary and before T  

`VALID_FROM` 决定 Handoff 是否已经激活；Event Replay Boundary 决定哪些 Daily Events 已被快照吸收。存在 `SNAPSHOT_CUTOFF` 时必须使用它，仅对历史 Handoff 回退到 `VALID_FROM`。
  
典型增量事件包括：  
- BASELINE_CHANGED  
- BASELINE_CONFIDENCE_CHANGED  
- FORECAST_CREATED  
- FORECAST_CONFIDENCE_CHANGED  
- HYPOTHESIS_CREATED  
- HYPOTHESIS_STATUS_CHANGED  
- METHOD_CHANGED  
- RESEARCH_PRIORITY_CHANGED  
  
历史 Daily Ledger 始终按其自身 SPEC_VERSION 解释。  
  
============================================================  
21. DUPLICATE & CONFLICT HANDLING  
============================================================  
本节区分 Producer 与 Consumer。  
  
Handoff Generator 读取 Schema 时：  
- 0 个可用冻结 Schema -> HANDOFF_SCHEMA_UNAVAILABLE；  
- 多个同版本冻结 Schema -> HANDOFF_SCHEMA_DUPLICATE，fail-closed；  
- 多个不同冻结版本 -> 使用语义版本号最高且适用于当前生成任务的版本。  
  
晨报运行时不读取 Schema。  
  
晨报读取 Handoff 时：  
- 只使用 STATUS: FROZEN；  
- 只使用 VALID_FROM <= 当前任务开始时间；  
- 若多个 Handoff 有效区间冲突且无法通过 VALID_FROM 唯一确定最新状态 -> HANDOFF_AMBIGUOUS；  
- 读取异常不得阻断晨报正文生成。  
  
============================================================  
22. FAILURE HANDLING  
============================================================  
若没有可用 Active Handoff、出现歧义或读取失败：  
- 晨报仍完成正常研究与输出；  
- 不得读取 Handoff Schema 作为运行时补救；  
- 不得用聊天记忆、旧 Monthly Final 或任意旧 Handoff 冒充当前 Research Prior；  
- Audit Ledger 持久化仍由 [INV-AUDIT][SPEC] 独立治理。  
  
============================================================  
23. IMMUTABILITY & VERSIONING  
============================================================  
Schema 与正式 Handoff 均遵循 write-once-read-many。  
允许 READ / SEARCH；禁止 UPDATE / SEND / DELETE / REPLACE / overwrite。  
  
本 v2.1 是完整、自包含的 Producer Contract，生成新 Handoff 不依赖任何 v1.x Schema 正文。
未来 FULL Schema 直接执行；DELTA Schema 必须声明 BASE_VERSION，并沿链回溯至最近的 FULL 后按旧到新叠加。
历史 v1.x Schema 与 Handoff 永久保留并按自身版本解释。
已冻结 Handoff 不得更新为“最新状态”；月内变化进入 Daily Ledger，除月度切换或明确的 Corrective Handoff 外不重新生成快照。  
  
============================================================  
24. LANGUAGE & HUMAN READABILITY  
============================================================  
机器字段与 enum 保持稳定英文。  
面向用户展示时，把机器状态转换成自然中文；真正的经济学、金融、统计、公司财务英文术语可中英标注。  
  
============================================================  
25. DESIGN PRINCIPLE  
============================================================  
Monthly Final -> Handoff Generator obeys Handoff Schema -> Self-contained Handoff Snapshot -> Daily Event Stream -> Next Monthly Audit -> Next Monthly Final  
  
Monthly 负责判断并校准；Schema 负责把判断编译成稳定契约；Morning 负责执行 Active Handoff + later Daily Events。  
  
不要依赖“月报写得聪明、晨报读得聪明”；要依赖有限动作集、明确字段语义和事件优先级。

============================================================
DRAFT-NATIVE RETRIEVAL — Gmail Draft 检索协议
============================================================
适用于本制品职责范围内所有存放在 Gmail Draft 的运行时权威与记录：Audit SPEC、Monthly Authority、Handoff Schema、Monthly Final、Monthly Handoff、Daily Ledger，以及职责范围内需要读取的 Governance artifact。此规则不扩大各消费者的 Authority 边界；晨报仍不读取 Monthly Authority / Handoff Schema 作为自身方法或运行补救。

1. 普通 Gmail message search 仅是发现入口。返回 0 条、索引延迟或搜索失败均不能证明 Draft 不存在；在作出缺失、unavailable 或允许创建的判断前，必须调用 Draft list/native retrieval 再核验。优先直接采用 Draft-native 路径。
2. Draft list 必须沿 next_page_token 遍历到分页结束，再在本地按目标命名空间、精确 Subject 与正文元数据筛选。不得把单页、snippet、工具输出截断或搜索摘要当作完整结果。列表返回 draft_id 与 message_id 时分别保存，不得混用；以 message_id 读取完整 MIME 正文是有效的 list→read 路径。已有可靠 ID 可直接读取，但不能据此证明无其他候选或无重复。
3. 合并各检索路径的候选，按底层 Draft / message 身份去重；同一对象被多路径命中不计为重复，不同对象即使正文一致也不能擅自合并。读取目标候选完整正文，核对实际 Subject、业务 STATUS、版本、EFFECTIVE_FROM / VALID_FROM 与 SUPERSEDES。Gmail DRAFT 标签不等于正文业务 DRAFT；高版本业务 DRAFT 不能屏蔽已生效 FROZEN 版本。
4. 若尚无可用候选或无法完成必要的唯一性核验，必须尝试当前实际可用的其他 Draft-native 路径；不能臆造不可调用接口。只有所有可用 Draft 检索路径均已失败或无可用候选，才可报告检索意义的 unavailable。区分：完整列举后确无候选、候选未生效/结构无效、读取/权限失败、分页不完整；不得把后几类描述为制品不存在。已有候选冲突按原 DUPLICATE / AMBIGUOUS 规则处理，不得伪装成缺失。版本链校验失败仍适用原失败规则，并明确原因。
5. 运行前固定任务开始时间 T；生效选择沿用本制品原有规则。FULL 独立执行，DELTA 依链解析；历史记录按自身版本解释。当前研究状态仍由有效 Handoff + replay boundary 后截至 T 的 Daily Events 重建，不因检索修复改变 SNAPSHOT_CUTOFF。
6. Daily duplicate check 的零结果必须来自已完成的 Draft-native 全部分页及精确实际 Subject 核验。普通搜索 0 条、权限错误、分页中断或相关候选读不全均不是“0封”。查重不确定时禁止创建，报告查重未完成并保留原因；不得借 unavailable 推导出允许写入。发现 1 封则 ALREADY_EXISTS，多于 1 封则 DUPLICATE；均不得覆盖、合并、删除或补写历史。创建前再次核验当日 Subject；写入响应不明确时先 native 检索和回读，禁止盲目重试造成重复。
7. 仅在各制品既有写入条件全部满足时写入；写后通过 Draft list→完整正文回读核验 Subject、版本、状态与内容，不把工具返回成功当作唯一持久化证据。读取失败不得阻断晨报完整正文；审计状态必须如实报告。此协议不授权修改、发送或删除历史对象。
8. 执行时保留简短检索诊断：使用过的路径、分页是否结束、候选数量、失败原因及最终选择；不为检索诊断新建 Daily Event，不建立额外永久登记簿。

============================================================
MIGRATION NOTES / ACTIVATION GATE
============================================================
版本：2.1；完整、自包含 FULL；基于 Gmail 当前冻结 Monthly-to-Daily Research Handoff Schema v2.0 全文迁移，保留其研究语义、事件类型与 Authority 边界，仅增加 Draft 检索可靠性规则及必要版本引用。
本版本已获用户批准冻结，自 2026-09-16T12:09:41+08:00 生效；SUPERSEDES 指向的 v2.0 保持原样，继续用于历史解释。
本次不修改历史 SPEC、Schema、Authority、Monthly Final、Handoff 或 Daily；不生成纠偏快照或追补 Daily。
Schema v2.1 生效后新生成 Handoff 标注 SCHEMA_VERSION: 2.1；现有 v2.0 Handoff 保持有效，其读取使用消费者的 Draft-native 规则，无需重建研究快照。
ACTIVATION_RECORD: 用户于 2026-09-16 明确批准冻结三份候选；晨报与月报 Scheduled Task 的 Draft-native 检索入口均已保存并回读确认。冻结时间为 2026-09-16T12:09:41+08:00。后续运行仍按任务开始时间选择已生效版本；本次仅验证配置与存储链，不声称已完成下一次定时运行。

