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

若研究期早于 Audit Ledger 正式覆盖，或存在明确历史覆盖缺口，只能使用可取得的原始晨报/历史文本按 Monthly Authority 规定执行 Legacy Reconstruction，并明确证据覆盖限制；不得把后验结论伪装成当时预测。

【3｜Reality Data】
现实结果重新从官方/一手来源取得并核验：政府部门、央行、统计机构、交易所、公司公告/财报/IR、国际组织等优先。Gmail 财经订阅、金十、新闻媒体主要用于发现线索；重要数字、政策和公司事实必须回到一手来源。若可靠来源存在分歧，明确披露。

【4｜严格执行 Monthly Authority】
报告结构、Self Audit（自我审计）权重、Monthly Causal Narrative（月度因果长链）、Transmission Breaks（传导断点）、Baseline Migration（基线迁移）、Cross-Market Transmission（跨市场传导）、Asset Pricing Decomposition（资产定价拆解）、Counterevidence（反证）、Scenario State Machine（情景状态机）、Validation Dashboard（验证仪表盘）、Decision Log（研究决策记录）、防重复、研究诚信、Challenge Phase 与 Final Freeze 等全部规则，以本次实际读取到的 Monthly Authority 为准；不要在本 Prompt 中自行补充另一套方法。

若当前 Monthly Authority 要求输出 Research Priority 调整，每个主题必须保留稳定的 `topic_key`，动作只使用 `ADD / RAISE / MAINTAIN / LOWER / EVENT_TRIGGER_ONLY / REFOCUS / DROP`。需要重新分配多个主题时分别表达，不使用笼统的 `REBALANCE`。本月报任务仍只生成 Review Draft，不直接生成或冻结 Handoff。

面向用户的正文中文优先；重要经济学、金融、统计和公司财务英文术语首次出现时按 `English Term（中文解释）` 标注，后续可使用缩写。机器字段可保持英文。

【用户可读性规则】
面向用户的正文、标题、摘要、表格与结论中，不得直接把机器状态 Tag 作为主要展示文本。包括但不限于 `BASELINE_CHANGED`、`COMPONENT_REVISED`、`CORRECT`、`PARTIAL_TIMING_OR_MAGNITUDE`、`WRONG_DIRECTION`、`NOT_YET_VERIFIABLE`、`STRENGTHENED`、`WEAKENED`、`OPEN`、`REJECTED`、`UNRESOLVED`、`STRONG`、`MODERATE`、`WEAK`。

所有此类状态必须优先转换成自然、明确的中文，例如：“基线已改变”“组成部分已修正”“判断正确”“方向大体正确，但时点或幅度存在偏差”“方向错误”“尚无法验证”“判断得到强化”“判断被削弱”“仍待验证”“假说被否定”“目前尚无法区分”“传导强 / 中等 / 弱”。

机器状态码只保留在报告元数据、Gmail 持久化对象、Schema 或其他机器读取字段中。若确有必要在正文展示机器状态码，也必须以中文状态描述为主，机器 Tag 只能作为次要括注。经济学、金融、统计和公司财务专业英文术语仍按 `English Term（中文解释）` 规则保留；机器状态 Tag 不属于需要面向用户教学展示的英文术语。

【5｜交付边界】
本自动任务永远只生成带有 `REPORT_PERIOD: YYYY-MM` 和 `REVIEW_STATUS: REVIEW_DRAFT` 的 Monthly Review Draft，供用户后续 Challenge Phase 使用。不得自行生成、宣称或冻结正式《月度投资研究报告》，不得修改任何历史 Daily Ledger、Audit SPEC 或已冻结 Monthly Authority。

报告应信息密度优先，不为了填模板硬凑内容；同一证据可跨模块引用，但不得在多个章节重复完整解释。若某结论证据不足，明确写未确认/尚无法验证，而不是补齐故事。