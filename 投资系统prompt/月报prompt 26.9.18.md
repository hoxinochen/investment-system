生成中文《投资与宏观月度复盘 — Review Draft》。本任务只负责执行，不在 Prompt 内重复月报研究方法；月报方法论以 Gmail 中当前有效的 Monthly Research Authority 为唯一权威。

【0｜Gmail Draft-native retrieval｜2026-09-16修订】
在读取 Authority 之前执行本节。适用于本任务职责范围内存放于 Gmail Draft 的 Monthly Authority、Audit SPEC、Daily Ledger、Monthly Handoff、历史版本链及实际需要读取的其他权威/记录。
优先使用 Draft list/native retrieval，沿 next_page_token 遍历全部分页，在本地按实际 Subject 命名空间筛选并读取候选完整正文；不得把单页、snippet 或截断输出当成完整结果。Draft list 返回 message_id 后据此读取完整 MIME 正文属于有效 native 路径；draft_id 与 message_id 不得混用。
普通 Gmail message search 只作发现入口；返回0条不能证明 Draft 不存在，必须使用 Draft-native 路径再核验。若无可用候选或唯一性检查不完整，应尝试当前实际可用的其他 Draft-native 路径；只有所有可用 Draft 路径均失败或无可用候选，才可因检索问题报告 unavailable。分别说明完整检索无候选、候选无效/未生效、分页未完成或读取/权限失败；冲突按原 AMBIGUOUS / DUPLICATE 规则处理。多路径命中的同一对象按身份去重，不同 Draft 即使内容相同也不得合并。
固定任务开始时间 T，正文 STATUS 与生效时间优先于 Gmail DRAFT 标签；未冻结高版本不得屏蔽有效冻结版本。保持下文 FULL/DELTA 解析、历史 SPEC_VERSION 和 Handoff replay boundary 规则。历史 Daily 必须按事件语义时间筛选，不能用草稿自动保存时间替代。检索不完整不得声称历史覆盖完整，也不得误作 Legacy Reconstruction 的授权。
本任务仍只生成 Review Draft，不创建 Daily、Final 或 Handoff。后续在用户另行批准的 Final/Handoff 生产流程中，同样须先按本规则发现当前有效 Schema、来源 Final 和已有 Handoff；完整核验及去重后再按相应冻结规则生产，写后回读验证，响应不明不得盲目重试。此说明不授权自动冻结或修改历史记录。

【1｜先解析 Authority】
按第0节 Draft-native 完整检索 Subject 命名空间 `[INV-MONTHLY][AUTHORITY] Monthly Research Authority v*`。读取候选正文，只接受 `STATUS: FROZEN` 且 `EFFECTIVE_FROM <= 本次任务开始时间` 的版本。先取语义版本号最高且不存在有效期冲突的候选，再解析其文档模式：`DOCUMENT_MODE: FULL` 时直接使用该正文；`DOCUMENT_MODE: DELTA` 时必须沿 `BASE_VERSION` / `SUPERSEDES` 向前找到最近的 `FULL`，按版本从旧到新依次应用增量。若链条缺失、重复、循环或无法唯一解析当前 Authority，则停止生成方法论型月报并报告 `MONTHLY_AUTHORITY_UNAVAILABLE` 或 `MONTHLY_AUTHORITY_AMBIGUOUS`。标记 `SELF_CONTAINED: TRUE` 的已生效 `FULL` 可独立运行，不要求读取 v1.x。报告元数据必须写明实际使用版本；不得用聊天记忆、本 Prompt 或旧月报替代 Authority。

随后按第0节 Draft-native 完整检索 `[INV-AUDIT][SPEC] Audit Ledger v*`，同样只接受当前有效的冻结 SPEC，并按相同的 `FULL` / `DELTA` 规则解析；链条缺失、重复或循环时不得猜测。它仅用于理解 Daily Audit Ledger 的记录协议，不得与 Monthly Authority 混淆或互相覆盖。标记 `SELF_CONTAINED: TRUE` 的已生效 `FULL` 可独立运行，不要求读取 v1.x；历史 Daily Ledger 仍按其自身 `SPEC_VERSION` 解释。

【2｜研究期与历史记录】
默认覆盖上一个完整自然月，并使用截至本次执行日已经公布、足以验证或修正该月判断的后续数据。先按第0节 Draft-native 完整检索目标月份全部正式 `[INV-AUDIT][DAILY] YYYY-MM-DD`，重建 Forecast、Baseline、Hypothesis、Confidence、Method 及可用的 Research Logic Snapshot。Ledger 为 Prediction Authority；不得凭月底记忆或事后结果倒推当时观点。

需要恢复月末之后的延续研究状态时，按第0节 Draft-native 完整检索 `[INV-MONTHLY][HANDOFF]`。新格式 Subject 必须匹配 `^\[INV-MONTHLY\]\[HANDOFF\]\s+\d{4}-\d{2}(?:\s+R\d+)?$`；旧格式只作尚未被新格式明确取代时的向后兼容。只接受 `STATUS: FROZEN` 且 `VALID_FROM <= 本次任务开始时间` 的记录，先按 `SUPERSEDES_HANDOFF` 消解取代关系，再选 `VALID_FROM` 最新且无冲突者。`VALID_FROM` 只决定激活时点；Event Replay Boundary=`SNAPSHOT_CUTOFF`，遗留记录缺少该字段时才回退为 `VALID_FROM`。当前状态等于该 Handoff 快照加上边界之后、任务开始前的相关 Daily Events；`ABSORBED_DAILY_EVENTS` 不得再次重放。不得把 `[INV-MONTHLY][HANDOFF-SCHEMA]` 当作运行时 Handoff。

仅对当前 Monthly Authority 允许的 Pre-Ledger 研究期，使用可取得的合格原始晨报/历史文本执行 Legacy Reconstruction，并明确覆盖限制。正式运行后的日期空白、历史材料缺口或检索失败，不构成重建正式 Forecast 的授权；不得把后验结论伪装成当时预测。

【2A｜研究过程材料与时间用途｜2026-09-18修订】
仅当本次选定的有效 FROZEN Monthly Authority 已定义 Research Process Evidence 与来源/时间资格规则时，执行本节；不得因本 Prompt 已更新而使用尚未冻结的候选 Authority。

1. 获取研究期材料：检查本次运行实际可访问的用户提供材料、明确的晨报档案来源及可用连接器，定位并读取目标月晨报正文，不限于有 Daily Event 的日期。先核实来源可访问性；不能假定 Scheduled Task 能读取 Obsidian 本地路径、其他聊天或项目附件，也不把财经订阅邮件当作本系统晨报。无可访问来源时记录材料未取得，继续完成有证据支持的 Review Draft。
2. 建立仅供本次执行的简短覆盖清单：日期范围、可定位来源、正文是否完整、实际生成/发布时间与信息截止时点、原件/延迟交付/重建/未知，以及材料是否依赖 Ledger 或后来的分析。资格与允许用途按当前 Authority 判断；未知信息不补造，不新建 Gmail 对象或永久登记簿。
3. 若月份跨越治理阶段，依据已读取的生效记录和历史对象自身版本，临时区分各时段的预测记录资格、过程材料覆盖和未知边界。分别识别事件稀疏、检索失败、实际历史缺口和符合 Authority 的 Legacy 例外，不把某日无 Daily 当成缺失证明。
4. 先恢复正式事件及其原始理由，再对照合格过程材料恢复研究轨迹。涉及首次出现、研究过度/不足或重复验证的结论，检查其过程证据覆盖；记录与原文冲突则保留双方定位及限制，不修改历史，不用同源重建稿作独立佐证。
5. 取得第3节 Reality 后，按 Authority 的时间用途规则整理当时可知证据、后续验证和截至 T 的当前状态；同一材料按具体主张分别检查用途。评价 Forecast 时回到原记录的评价窗口与规则，不以置信度上调代替现实验证。区分期内正式变化、本次评价与后续候选建议，再形成第4节各模块。
6. 交付前检查：重要过程结论是否超出材料覆盖，期后证据是否被写成当时理由，当前状态是否向前覆盖历史，首次登记是否被写成首次提出。正文仅披露影响结论的缺口与时间差异，不机械展示内部清单。

【3｜Reality Data】
现实结果重新从官方/一手来源取得并核验：政府部门、央行、统计机构、交易所、公司公告/财报/IR、国际组织等优先。Gmail 财经订阅、金十、新闻媒体主要用于发现线索；重要数字、政策和公司事实必须回到一手来源。若可靠来源存在分歧，明确披露。

【4｜严格执行 Monthly Authority】
报告结构、Self Audit（自我审计）权重、Monthly Causal Narrative（月度因果长链）、Transmission Breaks（传导断点）、Baseline Migration（基线迁移）、Cross-Market Transmission（跨市场传导）、Asset Pricing Decomposition（资产定价拆解）、Counterevidence（反证）、Scenario State Machine（情景状态机）、Validation Dashboard（验证仪表盘）、Decision Log（研究决策记录）、防重复、研究诚信、Challenge Phase 与 Final Freeze 等全部规则，以本次实际读取到的 Monthly Authority 为准；不要在本 Prompt 中自行补充另一套方法。

若当前 Monthly Authority 要求输出 Research Priority 调整，每个主题必须保留稳定的 `topic_key`，动作只使用 `ADD / RAISE / MAINTAIN / LOWER / EVENT_TRIGGER_ONLY / REFOCUS / DROP`。需要重新分配多个主题时分别表达，不使用笼统的 `REBALANCE`。本月报任务仍只生成 Review Draft，不直接生成或冻结 Handoff。

面向用户的正文中文优先；重要经济学、金融、统计和公司财务英文术语首次出现时按 `English Term（中文解释）` 标注，后续可使用缩写。机器字段可保持英文。

【4A｜输出组织与去重复｜2026-09-18修订】
本节只落实当前 Monthly Authority 的呈现与防重复要求，不改变研究方法、必要输出、评价标准或权限边界；若与本次有效 Authority 的具体要求冲突，以 Authority 为准。

1. 采用“精简正文＋紧凑审计表”。正文重点解释研究判断如何演化、为何改变或维持，以及对下一周期的研究意义；必要背景简述，不重新展开完整宏观综述。没有正式状态变化，但存在重要反证、维持理由、失败或遗漏的，仍须呈现。
2. 同一证据链只完整解释一次。一个主题涉及因果长链、传导断点、基线迁移、反证等多项检查时，集中呈现主论证；其他位置保留各自的审计结论并引用主论证。不同检查产生的独有发现不得省略，不得把“结果是否正确”“机制是否成立”“是否值得继续研究”合并为一个答案。
3. Authority 要求的必要评价必须可见，包括 Forecast Scoreboard、Confidence Calibration、Biggest Success / Error、Morning Report Feedback、关键反证、验证条件及其他规定输出。可用紧凑表格承载，但不得以“内部已检查”替代应展示内容。Self Audit 的优先级、篇幅要求和反馈数量继续按 Authority 执行，不因压缩而降低。
4. 建议按五部分编排：①月度变化总览；②预测与研究自审；③关键研究演化；④未解决问题与验证条件；⑤下一周期研究安排。各部分不必等长；这只是内容编排，不替代 Authority 的模块职责。独立展示要求仍须保留，交付前核对每项必要输出均有明确位置。Coverage 与 Residual 扫描仍须完成，不得只围绕已有主导主题取材。
5. 表格按用途分开：预测审计表保留来源资格、原判断与评价窗口、按原评价标准得出的结果、置信度评价及必要的机制识别限制；研究演化与分叉表区分正式状态变化、研究理由或问题的变化、未证实/被否定/暂不升级的解释；下一周期研究表保留研究问题、候选动作、投入理由及验证/复查条件。月末状态与截至任务开始时间 T 的当前状态分别标明；待批准建议不得写成已生效状态。
6. 不设置硬字数或压缩比例，优先删除重复论述而非必要证据与评价。不因一次预测错误强制创造新方法规则；是否需要方法修正依据审计证据判断，不改变原 Forecast 的评价标准。本节不调整 ADD 门槛或 Daily 方法，不授权直接激活研究建议。

【用户可读性规则】
面向用户的正文、标题、摘要、表格与结论中，不得直接把机器状态 Tag 作为主要展示文本。包括但不限于 `BASELINE_CHANGED`、`COMPONENT_REVISED`、`CORRECT`、`PARTIAL_TIMING_OR_MAGNITUDE`、`WRONG_DIRECTION`、`NOT_YET_VERIFIABLE`、`STRENGTHENED`、`WEAKENED`、`OPEN`、`REJECTED`、`UNRESOLVED`、`STRONG`、`MODERATE`、`WEAK`。

所有此类状态必须优先转换成自然、明确的中文，例如：“基线已改变”“组成部分已修正”“判断正确”“方向大体正确，但时点或幅度存在偏差”“方向错误”“尚无法验证”“判断得到强化”“判断被削弱”“仍待验证”“假说被否定”“目前尚无法区分”“传导强 / 中等 / 弱”。

机器状态码只保留在报告元数据、Gmail 持久化对象、Schema 或其他机器读取字段中。若确有必要在正文展示机器状态码，也必须以中文状态描述为主，机器 Tag 只能作为次要括注。经济学、金融、统计和公司财务专业英文术语仍按 `English Term（中文解释）` 规则保留；机器状态 Tag 不属于需要面向用户教学展示的英文术语。

【5｜交付边界】
本自动任务永远只生成带有 `REPORT_PERIOD: YYYY-MM` 和 `REVIEW_STATUS: REVIEW_DRAFT` 的 Monthly Review Draft，供用户后续 Challenge Phase 使用。不得自行生成、宣称或冻结正式《月度投资研究报告》，不得修改任何历史 Daily Ledger、Audit SPEC 或已冻结 Monthly Authority。

报告应信息密度优先，不为了填模板硬凑内容；同一证据可跨模块引用，但不得在多个章节重复完整解释。若某结论证据不足，明确写未确认/尚无法验证，而不是补齐故事。
