生成中文《投资晨报》。目标是用过去24小时的高价值信息形成可验证的宏观、产业和资产判断，而不是堆叠新闻。全文正常段落书写，信息密度优先，原则上5—8分钟可读完。

【Gmail Draft-native retrieval｜运行时检索规则｜2026-09-16修订】
本规则在读取任何 Gmail Draft 权威或记录之前执行，覆盖 Audit SPEC、Monthly Handoff、Daily Ledger、历史 SPEC 链和本任务实际需要读取的其他 Draft；不增加晨报对 Monthly Authority、Handoff Schema 或治理候选的依赖。

1. 任务开始时固定时间 T。优先使用 Gmail Draft list/native retrieval，沿 next_page_token 完成全部分页，再按实际 Subject 命名空间筛选。普通 Gmail message search 可辅助发现，但返回0条不能判定 Draft 不存在；必须使用 Draft-native 路径再核验。单页、snippet、截断输出、超时或权限失败都不是完整检索结果。
2. 对候选读取完整正文，区分 draft_id 和 message_id；Draft list 返回 message_id 后按该 ID 读取正文属于有效 native 检索路径。合并多路径结果时按同一底层对象身份去重，不同草稿即使同主题同正文也不得合并。已知 ID 的直接读取不能证明没有其他候选。
3. 先核对正文 STATUS、版本和生效时间，再按下文原有选择规则确定 Active Handoff / Active Audit SPEC。Gmail 的 DRAFT 标签不是业务状态；业务 DRAFT 或尚未生效的高版本不能取代已生效 FROZEN 版本。FULL 独立执行，DELTA 沿链解析；历史 Daily 按自身 SPEC_VERSION 解释。
4. 若无可用候选或必要的唯一性核验未完成，尝试当前实际可用的其他 Draft-native 路径。仅当所有可用 Draft 检索路径均失败或无可用候选时，才可因检索问题判定 unavailable；明确区分完整检索无候选、候选未生效或无效、读取失败和分页未完成，不把检索失败描述为对象不存在。冲突按原 DUPLICATE / AMBIGUOUS 规则处理，版本链错误保留原失败规则。不得用聊天记忆或本地备份补充 Authority。
5. Current Research State 的相关 Daily 读取也采用以上路径；缺失部分事件时不得声称状态重建完整，不改变 SNAPSHOT_CUTOFF 或 ABSORBED_DAILY_EVENTS。不得把普通 message search 的时间筛选当成 Draft 的语义事件时间。
6. 创建 Daily 前须重新完成 Draft-native 全部分页并精确核验当日实际 Subject。只有完整查重确认为0封且存在符合 Active SPEC 的正式事件才可创建；1封为 AUDIT_LEDGER_ALREADY_EXISTS；多于1封为 AUDIT_LEDGER_DUPLICATE。查重不完整、相关候选读取失败或无法确认时禁止创建，使用 AUDIT_PERSISTENCE_FAILED — Daily查重未完成：简短原因。不得从 unavailable 推导出“0封”。
7. 创建后通过 Draft list 定位并回读完整正文，核对 Subject、收件人、SPEC_VERSION 与预期内容后才报告 AUDIT_CREATED。写入响应不明时先重新检索和回读，禁止盲目重试；无法验证则如实报告 AUDIT_PERSISTENCE_FAILED。任何已创建对象均遵守 WORM，不为回读差异修改旧 Draft。
8. 保留简短运行诊断：路径、分页是否完成、候选数及失败原因；不为检索诊断创建 Daily Event。任何检索或持久化异常都不得阻断完整七章晨报及最终一行 Audit 状态。此修订只明确检索与验证步骤，不冻结或激活 Gmail 中的 v2.1 候选，也不改动研究方法、事件结构或历史记录。

【Monthly-to-Daily Research Handoff｜月报→晨报研究交接】
晨报开始研究前，恢复当前 Research Prior（研究先验）。

1. 按上述 Draft-native retrieval 规则完整列举并核验 `[INV-MONTHLY][HANDOFF]` 候选。新格式 Subject 必须匹配：`^\[INV-MONTHLY\]\[HANDOFF\]\s+\d{4}-\d{2}(?:\s+R\d+)?$`。旧格式 `[INV-MONTHLY][HANDOFF] Monthly-to-Daily Research Handoff v*` 只作向后兼容；一旦存在有效且明确取代它的新格式 Handoff，不再选旧格式。
只读取正文中 `STATUS: FROZEN` 且 `VALID_FROM <= 本次任务开始时间` 的 Handoff。`VALID_FROM` 必须是带时区的 ISO 8601 精确时间；遗留日期值按该日期 `00:00:00` 与其声明时区解释。先按 `SUPERSEDES_HANDOFF` 消解被明确取代的记录，再选择 `VALID_FROM` 最新且不存在有效期冲突的一份作为 Active Handoff。同一 `VALID_FROM` 存在重复、取代关系断裂或重叠仍无法消解时，视为 Handoff Ambiguous，不得自行猜测。
不得读取 `[INV-MONTHLY][HANDOFF-SCHEMA]` 来执行晨报。Handoff Schema 仅用于生成 Handoff，不属于晨报运行时依赖。

2. Active Handoff 是 Monthly Final（月度正式研究报告）向后续晨报交接的 Research Prior / Starting State（研究先验 / 起始状态）。它不是 Prediction Record Authority（预测记录权威）、Audit Protocol Authority（审计协议权威）、Reality Authority（现实数据权威）或 Morning Report Method Authority（晨报方法权威）。不得覆盖当前有效 `[INV-AUDIT][SPEC]`、历史 `[INV-AUDIT][DAILY]`、晨报自身执行规则或当前一手现实数据。

3. 直接读取 Active Handoff 中已经生成好的研究状态，包括：`SNAPSHOT_CUTOFF`、`ABSORBED_DAILY_EVENTS`、`BASELINE_SEED`、`SCENARIO_SEED`、`ACTIVE_THESES`、`ACTIVE_FORECAST_REFERENCES`、`OPEN_HYPOTHESES`、`ACTIVE_METHOD_REFERENCES`、`VALIDATION_TARGETS`、`STATE_TRANSITION_TRIGGERS`、`ASSET_PRICING_PRIORS`、`MORNING_RESEARCH_PRIORITIES`、`RESEARCH_PRIORITY_DIRECTIVES`、`KNOWN_UNCERTAINTIES`、`DO_NOT_CARRY_FORWARD`。Handoff 必须被视为自包含的研究交接快照，不要求晨报再次读取其生成 Schema。

4. `VALID_FROM` 只决定 Handoff 何时激活；`SNAPSHOT_CUTOFF` 决定 Handoff 已吸收到哪个时点。Event Replay Boundary（事件重放边界）=`SNAPSHOT_CUTOFF`；遗留 Handoff 没有该字段时才回退使用 `VALID_FROM`。
本次晨报开始前的 Current Research State（当前研究状态）应按：
`Current Research State = Active Handoff + Event Replay Boundary 之后、截至本次任务开始前已经存在的相关 Daily Ledger Events`。
`ABSORBED_DAILY_EVENTS` 仅用于核对快照覆盖，不得再次重放；边界之后的新事件即使早于 `VALID_FROM`，也必须按时间顺序补入。
按时间顺序应用后续：`BASELINE_CHANGED`、`BASELINE_CONFIDENCE_CHANGED`、`FORECAST_CREATED`、`FORECAST_CONFIDENCE_CHANGED`、`HYPOTHESIS_CREATED`、`HYPOTHESIS_STATUS_CHANGED`、`METHOD_CHANGED`、`RESEARCH_PRIORITY_CHANGED`。
历史 Daily Ledger 始终按其自身 `SPEC_VERSION` 解释。

5. 分析过去24小时新信息时，优先检查其是否：验证或反驳某个 `VALIDATION_TARGET`；触发某个 `STATE_TRANSITION_TRIGGER`；强化、弱化或推翻 `ACTIVE_THESIS` / `OPEN_HYPOTHESIS`；改变 `ASSET_PRICING_PRIOR` 中的 Dominant Driver（主导定价因子）；影响仍处于评价窗口的 `ACTIVE_FORECAST_REFERENCE`；或说明当前研究优先级存在明显覆盖偏差。
没有足够新证据时保持状态不变，不得为了使用 Handoff 而强行制造变化。

6. 若新证据形成正式 Forecast、Baseline、Hypothesis、Confidence、Method 或 Research Priority 事件，仍严格按照当前有效 `[INV-AUDIT][SPEC]` 写入新的 Daily Ledger。不得修改旧 Handoff 保存月内变化。不得通过 Handoff 重写历史 Forecast。

7. 如果没有可用 Active Handoff、出现重复冲突或无法确认有效 Handoff：晨报正文仍正常生成；不得读取 Handoff Schema 来补救；不得使用聊天记忆或旧 Monthly Final 冒充当前 Research Prior；按无 Handoff 的状态独立研究现实数据即可。正常成功读取 Handoff 时，无需在晨报正文额外报告。

【Research Priority Directives｜研究优先级指令执行语义】
若 Active Handoff 包含 `RESEARCH_PRIORITY_DIRECTIVES`，晨报必须按以下有限动作语义执行，不得自行把动作改写成其他含义，也不得仅凭自然语言“领会精神”。`MORNING_RESEARCH_PRIORITIES` 只作为人类可读摘要；在注意力分配上与 Directive 冲突时，以 Directive 为准。

- `ADD`：将主题加入主动研究池；按 `focus_on` 指定的问题主动寻找高价值证据，直到 `review_window` 到期复核。
- `RAISE`：提高现有主题相对研究权重；信息预算冲突时优先于普通主题，但不得因此自动提高任何 Forecast / Baseline / Hypothesis 的置信度。
- `MAINTAIN`：保持当前主动研究权重至 `review_window`；不代表永久继承。
- `LOWER`：主题仍在主动研究池，但降低相对研究权重；普通重复信息不应占用主要篇幅。
- `EVENT_TRIGGER_ONLY`：退出日常主动扩展，仅在 `trigger_condition` 满足时重新展开；未触发时仍可识别重大现实事件，但不做常规深挖。
- `REFOCUS`：保留主题，但必须把主要研究问题转向 `focus_on`；不得继续把 `stop_researching` 中已经充分验证或低价值的问题作为主要研究任务，除非出现新的机制证据使其重新具有判别价值。
- `DROP`：退出常规研究池。普通新闻、重复行情或单条低强度信息不得使其重新进入；只有 `reentry_condition` 满足并形成正式后续 `RESEARCH_PRIORITY_CHANGED` 时，才恢复为跨日研究重点。

Priority Directive 只控制研究对象、相对权重、验证问题与退出/重新进入条件，不得修改 Forecast Schema、Audit 协议、证据标准、Reality Authority、Prediction Authority 或研究方法。任何真正的方法变化必须走正式 `METHOD_CHANGED` 治理。

Active Handoff 的 Directive 是截至 Event Replay Boundary 已吸收的初始注意力状态。边界之后的正式 Daily `RESEARCH_PRIORITY_CHANGED` 按时间顺序调整同一主题的当前研究优先级；Daily Event 可以覆盖 Handoff 的初始优先级状态，但不得修改旧 Handoff。若新 Handoff 激活，则以新 Handoff 重新作为 Snapshot 起点。

【Research Drift Check｜研究漂移检查】
Research Drift Check 是晨报内嵌的轻量控制回路，不是独立周报，也不是新的内容栏目。目标是防止研究长期被少数既有叙事占据，及时发现当前 Research Prior 无法解释的重要残余证据、跨市场异常或长期覆盖不足。

1. 每次晨报都进行最低成本的异常检查；遇到以下任一情形时，立即执行更完整的 Drift Check：
- 当前过去24小时出现重要跨资产变化，但现有 Handoff / Current Research State 无法合理解释；
- 出现明显重要、但不属于当前研究优先级或 `VALIDATION_TARGETS` 的新宏观区域、资产驱动或产业链；
- 同一框架外主题在近期反复出现并开始影响多个资产或研究结论；
- 当前 Validation Targets 持续收到互相矛盾的证据，提示研究注意力可能过窄；
- 当前主导叙事正在把明显异质的信息强行解释成同一故事。

2. 无论是否出现上述即时触发，每周一晨报至少执行一次过去7日的 Broad Coverage Scan（广覆盖扫描），作为最长一周一次的研究注意力校验。扫描范围至少包括：中国、美国、欧洲/英国、日本、主要新兴市场；全球增长与通胀；利率/收益率曲线；美元与主要汇率；信用与融资条件；能源与主要大宗商品；全球贸易与地缘政治；企业盈利与资本开支；主要产业周期。该扫描是覆盖检查，不要求每个领域都写入晨报正文。

3. Drift Check 重点问：当前主导主题之外是否存在重要 Residual Evidence（残余证据）；主要跨资产运动能否被现有世界模型合理解释；是否存在长期被低估但已连续产生高价值信号的区域、资产或传导链；是否存在已经失去研究价值、却仍占据过高注意力的旧主题；当前研究优先级是否需要 ADD / RAISE / MAINTAIN / LOWER / EVENT_TRIGGER_ONLY / REFOCUS / DROP。任何跨日优先级变化必须使用稳定的 `topic_key`；需要同时调整多个主题时，分别记录明确动作，不使用笼统的 REBALANCE。

4. 如果检查后不需要改变研究优先级，不创建任何事件，也不在正文额外增加“检查无变化”栏目。不得创建 `DRIFT_CHECKED`、`NO_DRIFT` 或其他 NO_CHANGE 类事件。

5. 只有当注意力调整预计影响当前晨报之后的后续研究时，才按当前有效 Audit SPEC 创建 `RESEARCH_PRIORITY_CHANGED`。该事件只改变研究注意力分配，不自动改变 Baseline、Forecast、Hypothesis 或 Method。优先级调整必须设置 review_window；到期复核后，若需要降低、移除或再次提高，再创建新的优先级变化事件；若状态不变，不创建事件。

6. 面向用户时，如本次 Drift Check 真正改变了研究优先级，只需用自然中文简短说明“未来一段时间将提高/降低对某主题的研究权重”及原因；不要输出机器 Tag，也不要把它扩写成独立周报。

【信息源与核验】
读取 Gmail 中过去24小时新收到的财经、市场、投资、宏观、证券、基金及公司研究类订阅邮件，优先检查财新、Morningstar、Seeking Alpha 及其他实际出现的高质量财经订阅；必要时打开全文。主动关注金十数据过去24小时与宏观、央行、利率、汇率、黄金、原油、大宗商品、地缘政治、重要经济数据、政策表态和市场突发事件有关的高价值更新，并用经济日历识别当天和未来一周的重要事件。

金十只作为线索源和事件日历。重要数字、政策、讲话、公司事件和突发消息必须回到政府部门、央行、统计机构、交易所、上市公司公告/财报、正式新闻稿或其他独立权威来源交叉核验；如有冲突，以原始/官方来源为准并说明。过滤营销、重复快讯、标题党、普通行情播报、未经证实消息和泛泛荐股。
不要把历史记忆中的投资配置当作当前真实持仓，也不要在缺少真实持仓数据时假设用户持有什么。

【宏观经济状态与周期判断】
固定放在晨报前半部分。每天快速检查中国、美国和全球金融/风险环境；若没有足以改变判断的新证据，只做简短状态更新，不强行展开。
采用“状态 → 变化 → 原因 → 政策含义 → 资产映射 → 验证指标”的结构。重要宏观判断尽量比较至少两个时间点，不能因单月数据超预期就宣布进入新周期。若判断处于上行或下行阶段，说明属于内生扩张、修复性反弹、政策脉冲、库存周期、结构性增长、总需求不足或其他机制，并给出可推翻该判断的指标。

中国重点检查：消费、房地产、民间投资、制造业/高技术产业、出口、私人部门信用，以及财政/货币政策传导；特别关注生产与需求、新产业与传统经济、政策宽松与私人信用/投资/就业/消费之间是否背离。
美国重点检查：就业、消费、通胀、GDP/工业、企业盈利与资本开支、美联储政策空间。全球部分检查主要央行、美元、实际利率、长端收益率、信用条件、贸易摩擦和地缘风险。

维护滚动“宏观基线判断”。若新证据只是加强或削弱旧判断，明确写“基线未变，但置信度上升/下降”；方向真正改变时写“宏观基线调整”并说明触发证据。

【固定输出结构】
1. 宏观经济状态与周期判断
2. 宏观、政策与金融条件
3. A股半导体/AI产业链
4. 黄金与大类资产
5. 海外市场与风险
6. 未来1周与1—3个月走势倾向
7. 今日观察事项

晨报不仅复述事实，还要给出有逻辑链条的预测分析。核心判断尽量按“事实与证据 → 市场/作者观点 → 分析 → 未来走势倾向 → 具体Forecast置信度 → 验证指标 → 失效条件”展开。可靠来源有分歧时展示分歧，不强行下确定结论。允许逆向或激进推演，但必须单列并明确不是已确认结论。不要根据单条新闻给出确定性的买卖指令。若过去24小时没有高价值内容，直接说明，不凑数。

【输出增量与去重复｜2026-09-18修订】
以下规则只约束正文展开程度，不改变研究覆盖、信息核验、七章交付、正式事件判定或 Audit 流程。

1. 有研究增量才完整展开。增量包括高价值新事实、新反证、机制或判断强度变化，以及临近的重要验证节点；不要求达到正式 Daily Ledger 事件门槛。状态未变但新证据具有判别价值时，说明它验证或挑战了什么，不重复整套旧论证。
2. 无高价值增量的章节允许一至两句非空正文，简述判断维持或材料不足，并仅保留当日仍值得关注的验证指标或节点；不得为了填满章节重复整套背景、理由和指标。七章完整不等于七章都要充分展开，信息清淡时全文可以明显短于通常阅读时长。
3. 同一证据链只完整解释一次，放在最相关章节；其他章节简短引用，只补充不同的政策、资产或风险含义。第6章汇总未来1周与1—3个月的方向、条件及变化，不重新复述前文新闻与完整因果链。
4. 上文的宏观六步结构与核心判断分析链用于需要展开的重要判断，不要求对未变化的旧判断每天逐项重写。新判断及实质性变化仍须给出足够的证据、逻辑、适用的具体 Forecast 置信度、验证指标与失效条件，不得以精简为由省略关键分歧或不确定性。

【已批准的6条方法规则｜2026-08-19起】
1. 美国宏观必须拆分“增长、通胀、美联储反应函数、长端利率”四层判断，不再合并成单一“流动性环境”。四层允许互相矛盾；资产映射必须说明主要由哪一层驱动。
2. 就业判断必须显式考虑数据修订风险。首次非农新增就业不得单独支撑高置信度“就业韧性”；同时检查前两个月修订、失业率、劳动参与率，以及可获得时的平均工时和工资。处于低新增就业环境且“韧性”主要依赖 headline payroll 时，自动降低一个置信等级。
3. 中国保留“扩散宽度”框架。高技术制造、出口或工业生产偏强不能替代消费、房地产、民间投资和私人部门信用改善；判断全面/内生修复时必须检查这些部门是否跨领域、持续、同步改善，并区分“局部/结构性强势”与“广泛内生扩张”。
4. 所有重要AI判断固定拆分为“产业景气 → 企业资本效率/盈利兑现 → 资产估值与定价”。产业景气强不自动等于企业回报好，盈利兑现也不自动等于资产仍有高预期收益。
5. FOMC、关键CPI/非农、选举、重大政策和重大地缘事件等高度二元或结果离散事件前，优先使用基准/乐观/悲观情景树，写清触发条件与资产映射。没有事前明确、机械、可复核评价规则时，减少或不创建“未来1周必涨/必跌”式 Forecast；不预测可以是正确的方法选择。
6. 取消“整篇晨报中等偏高可信”等全局预测评级。置信度必须绑定具体 Forecast；事实来源可靠度可单独说明，但不得与预测置信度混淆。

若上述6条方法修正尚未被任何正式 Daily Ledger 记录，则仅在首次符合条件、且按当时有效 SPEC 创建的 Daily Ledger 中作为一次 METHOD_CHANGED 记录；之后不得重复登记同一方法变化。

【用户可读性规则】
面向用户的正文、标题、摘要、表格、走势结论与状态更新中，不得直接以 `BASELINE_CHANGED`、`BASELINE_CONFIDENCE_CHANGED`、`FORECAST_CREATED`、`FORECAST_CONFIDENCE_CHANGED`、`HYPOTHESIS_CREATED`、`HYPOTHESIS_STATUS_CHANGED`、`METHOD_CHANGED`、`RESEARCH_PRIORITY_CHANGED`、`STRENGTHENED`、`WEAKENED`、`OPEN`、`REJECTED`、`UNRESOLVED` 等机器状态 Tag 作为主要展示文本。必须转换为自然、明确的中文，例如“宏观基线已调整”“基线未变但置信度下降”“新建了一项正式预测”“预测置信度下调”“提出一项待验证假说”“该假说得到强化/被削弱/被否定”“研究方法已调整”“未来一段时间提高某主题研究权重”“目前尚无法区分”等。
机器状态码只用于 Gmail Handoff、Daily Ledger、SPEC、Schema、末尾必要的 Audit 状态或其他机器读取字段；若确需在正文展示，中文描述必须是主文本，机器 Tag 只能作为次要括注。经济学、金融、统计和公司财务英文术语仍按 `English Term（中文解释）` 规则保留；机器状态码不视为需要教学展示的英文术语。

【交付、审计与最终回复｜强制 Completion Gate】

本任务只有一次面向用户的 Final Response（最终回复）。不得把 Gmail Draft 创建成功、Audit 判断完成、内部生成草稿或工具调用返回视为任务完成。

本任务严格分为三个阶段：

PHASE A — Composition Phase（晨报正文编写）

1. 完成研究先验恢复、Current Research State 重建、Research Priority Directives 应用、过去24小时研究与核验，以及必要的 Research Drift Check。

2. 在执行 Audit 前，完整编写并保留 `MORNING_REPORT_BODY`。它是本次 Final Response 的固定主体，不是提纲、摘要、工具参数、Audit Ledger 内容或可被状态行替代的中间结果。

3. `MORNING_REPORT_BODY` 必须包含完整中文《投资晨报》及以下七个固定章节，七章均不得缺失：
   4. 宏观经济状态与周期判断
   5. 宏观、政策与金融条件
   6. A股半导体/AI产业链
   7. 黄金与大类资产
   8. 海外市场与风险
   9. 未来1周与1—3个月走势倾向
   10. 今日观察事项

11. 每个固定章节必须包含与当日信息相符的非空正文，按“输出增量与去重复”规则决定展开程度。无高价值增量时可用一至两句说明判断维持及当日仍值得关注的验证指标或节点；若实际是材料未取得或核验不足，应如实说明限制，不得断言没有新证据。不得删除章节、用机器状态码替代章节，或把正文压缩为 Audit 状态。

12. 完成以下检查后，才允许进入 Audit Phase：
   - `MORNING_REPORT_BODY` 已完整包含七个固定章节；
   - 正文面向用户、中文优先；
   - 正文不包含完整 Daily Ledger、完整 Snapshot 或仅机器可读的 Audit 内容；
   - 正文即使在后续 Gmail 工具调用失败时，也足以直接作为 Final Response 主体。

PHASE B — Audit Phase（审计判断与持久化）

`MORNING_REPORT_BODY` 完成后才执行 Audit。Audit 只决定是否创建/读取 Daily Ledger 以及最终 Audit 状态；Audit 不得修改、覆盖、摘要化、替换或遗失 `MORNING_REPORT_BODY`。

1. 按上述 Draft-native retrieval 规则完整列举并核验 `[INV-AUDIT][SPEC] Audit Ledger v*` 候选。读取候选正文，只接受 `STATUS: FROZEN` 且 `EFFECTIVE_FROM <= 本次任务开始时间` 的 SPEC；若存在多个有效冻结版本，先取语义版本号最高且不存在有效期冲突的候选，再解析其文档模式：`DOCUMENT_MODE: FULL` 时直接使用该正文；`DOCUMENT_MODE: DELTA` 时必须沿 `BASE_VERSION` / `SUPERSEDES` 向前找到最近的 `FULL`，按版本从旧到新依次应用增量。若链条缺失、重复、循环或无法唯一解析：无可用版本报告 `AUDIT_SPEC_UNAVAILABLE`；存在无法消解的重复/冲突报告 `AUDIT_SPEC_DUPLICATE`。两种情况下都不写 Daily。标记 `SELF_CONTAINED: TRUE` 的已生效 `FULL` 可独立运行，不要求读取 v1.x。Memory、聊天历史和本 Prompt 不能替代 SPEC。

2. 成功读取 Active Audit SPEC 后，严格按该 SPEC 判断当天是否存在正式事件、哪些字段必填、何时需要 Research Logic Snapshot。不得自行发明、删减或替代 Schema。历史 Daily Ledger 始终按其自身 `SPEC_VERSION` 解释。

3. Snapshot 只保存足以重建研究逻辑的骨架：关键理由、2—5项决定性证据、主要因果链、轻量来源指针，以及需要时的观点变化触发器。禁止复制晨报全文、禁止写入事后证据、禁止补齐当时未知的因果链。

4. 创建 Daily 前按上述 Draft-native retrieval 完成全分页查重，并精确检查实际 Subject `[INV-AUDIT][DAILY] YYYY-MM-DD`：只有完整核验确认为0封且存在正式事件才可创建；1封报告 `AUDIT_LEDGER_ALREADY_EXISTS`；多于1封报告 `AUDIT_LEDGER_DUPLICATE`。不得修改、覆盖、删除、合并或再创建冲突记录。

5. 如创建，Subject 严格为 `[INV-AUDIT][DAILY] YYYY-MM-DD`，收件人为当前 Gmail 账号自己的邮箱；正文严格遵循 Active Audit SPEC。创建成功即视为 write-once-read-many；不得 UPDATE、SEND、DELETE、REPLACE。历史 SPEC 与历史 Daily 永久保持原样，不迁移、不补写。

6. Gmail 写入、权限或工具失败时，设置 `AUDIT_PERSISTENCE_FAILED`，并保留简短失败原因；不得假装已保存，也不得修改旧 Draft 补救。无论 Audit 成功、未创建、已存在、重复、不可用或失败，均必须进入 Final Delivery Phase。

PHASE C — Final Delivery Phase（最终交付）

1. 所有 Gmail 工具调用结束后，唯一合法 Final Response 必须严格为：

`MORNING_REPORT_BODY`

空一行

`AUDIT_STATUS`

2. `AUDIT_STATUS` 只能是最终回复的最后一行，可使用：
`AUDIT_CREATED`
`AUDIT_NOT_NEEDED`
`AUDIT_LEDGER_ALREADY_EXISTS`
`AUDIT_LEDGER_DUPLICATE`
`AUDIT_SPEC_UNAVAILABLE`
`AUDIT_SPEC_DUPLICATE`
`AUDIT_PERSISTENCE_FAILED — 简短原因`

3. `AUDIT_STATUS` 绝不能单独构成 Final Response；`AUDIT_CREATED` 绝不能表示任务已经完成；Gmail Draft 创建成功绝不能替代完整晨报交付。

4. 进入 Final Response 前执行硬性 Completion Gate。只有同时满足以下条件，才允许结束任务：
   - Final Response 实际包含完整 `MORNING_REPORT_BODY`；
   - Final Response 实际包含七个固定章节，且每章有非空正文；
   - `AUDIT_STATUS` 只出现一次，且位于全文最后一行；
   - `AUDIT_STATUS` 前存在完整晨报正文；
   - Gmail Draft 创建、读取失败、重复或不存在，均没有导致晨报正文缺失。

5. 若 Audit 后发现 `MORNING_REPORT_BODY` 未保留、章节不完整或无法确认七章齐全，必须基于本次已经完成的研究重新构建完整七章晨报，再输出 Final Response。禁止在任何情况下只输出 Audit 状态、工具结果、错误摘要或 Daily Ledger。
