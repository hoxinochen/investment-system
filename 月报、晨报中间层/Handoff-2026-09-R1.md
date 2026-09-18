SCHEMA_NAME: Monthly-to-Daily Research Handoff  
SCHEMA_VERSION: 2.1  
STATUS: FROZEN  
HANDOFF_ID: 2026-09-R1  
HANDOFF_PERIOD: 2026-09  
CREATED_AT: 2026-09-18T17:03:13+08:00  
VALID_FROM: 2026-09-18T17:03:13+08:00  
SNAPSHOT_CUTOFF: 2026-09-16T11:52:37+08:00  
VALID_UNTIL: SUPERSEDED  
SUPERSEDES_HANDOFF: [INV-MONTHLY][HANDOFF] 2026-08 R2  
  
SOURCE_MONTHLY_FINAL_SUBJECT: [INV-MONTHLY][FINAL] 2026-08 | 投资与宏观月度复盘  
SOURCE_MONTHLY_FINAL_PERIOD: 2026-08  
SOURCE_MONTHLY_FINAL_VERSION: Final v1.0  
MONTHLY_AUTHORITY_VERSION: 2.2  
AUDIT_SPEC_VERSION: 2.1  
  
ABSORBED_DAILY_EVENTS:  
- [INV-AUDIT][DAILY] 2026-08-27 / 20260827-US-GROWTH-01-C1  
- [INV-AUDIT][DAILY] 2026-08-29 / 20260829-US-GROWTH-QUALITY-01-S1  
- [INV-AUDIT][DAILY] 2026-08-30 / 20260830-GLOBAL-REFINED-PRODUCTS-01  
- [INV-AUDIT][DAILY] 2026-09-15 / 20260915-US-RATES-01-C2  
- [INV-AUDIT][DAILY] 2026-09-16 / 20260916-US-LONG-END-PRIORITY-01  
  
## BASELINE_SEED  
  
CHINA_AGGREGATE_DEMAND:  
state: 总需求不足，消费、房地产、民间投资和私人部门信用尚未形成跨领域、跨月的持续扩散改善  
confidence: MEDIUM_HIGH  
  
CHINA_STRUCTURAL_GROWTH:  
state: 高技术制造、出口和部分产业增长保持结构性韧性，但不能替代对私人总需求的独立判断  
confidence: HIGH  
  
US_GROWTH:  
state: 增长降温但尚未确认衰退；私人国内需求、企业利润、生产率和资本投资显示韧性，低就业流量需与供给和生产率结构共同解释  
confidence: MEDIUM_HIGH  
  
US_LONG_END:  
state: 长端收益率高位状态已得到较强现实支持，但 Fed、实际利率、财政供给、期限溢价、能源与全球资本需求的相对贡献仍未完全识别  
confidence: MEDIUM_HIGH  
  
AI_INDUSTRY:  
state: AI产业需求保持强势；下一阶段核心问题从需求真实性转向分行业资本效率、现金回报、融资成本与估值纪律  
confidence: HIGH_ON_DEMAND_MEDIUM_ON_CAPITAL_RETURN  
  
## SCENARIO_SEED  
  
PRIMARY_STATE:  
state: STRUCTURAL_GROWTH_WITH_WEAK_PRIVATE_DEMAND_US_RESILIENT_COOLING_AI_STRONG_DEMAND_HIGH_LONG_END  
 description: 中国结构性产业增长与私人总需求不足并存；美国增长放缓但尚未确认衰退；AI需求强而资本回报仍需验证；美国长端维持高位但驱动机制待进一步拆分。  
  
WATCH_STATES:  
- CHINA_PRIVATE_DEMAND_BROADENING  
- US_GROWTH_QUALITY_DIVERGENCE  
- AI_CAPITAL_EFFICIENCY_STRESS  
- US_LONG_END_DRIVER_SHIFT  
  
## ACTIVE_THESES  
  
THESIS_01:  
topic: 中国私人需求扩散  
thesis: 结构性产业增长不等于总需求修复；只有消费、地产、民间投资与私人信用出现跨领域且具有持续性的改善，才能确认扩散开始。  
status: ACTIVE  
source_final_section: 三、关键研究演化 / 中国  
  
THESIS_02:  
topic: AI资本效率与资产回报  
thesis: AI需求真实性已经得到较强验证，资产回报的边际研究价值主要来自增量资本投入能否转化为足够的现金流、资本回报并消化融资成本和估值。  
status: ACTIVE  
source_final_section: 三、关键研究演化 / AI  
  
THESIS_03:  
topic: 美国长端独立定价风险  
thesis: 美国增长和 Fed 路径不能机械决定长端利率；下一阶段应重点识别实际利率、期限溢价、财政供给、能源和全球资本需求的相对贡献。  
status: ACTIVE  
source_final_section: 二、预测与研究自审 / 本月最大成功；五、下一周期研究安排  
  
## ACTIVE_FORECAST_REFERENCES  
  
- NONE  
  
note: Forecast 20260819-US-RATES-01 的 evaluation_window 已于 2026-09-16 结束，不再作为 OPEN Forecast 携带；其后续研究价值已通过 20260916-US-LONG-END-PRIORITY-01 转化为独立 Research Priority。  
  
## OPEN_HYPOTHESES  
  
HYPOTHESIS_01:  
hypothesis_id: 20260819-AI-VALUATION-01  
topic: AI资本效率与估值  
hypothesis: AI产业需求仍强，但资产定价越来越取决于资本效率、融资成本、现金回报和估值能否消化高Capex。  
status: STRENGTHENED  
source_pointer: [INV-AUDIT][DAILY] 2026-08-19 / 2026-08-20; 2026-08 Final  
  
HYPOTHESIS_02:  
topic: 美国增长质量  
hypothesis: 低就业流量可能与劳动力供给放缓、生产率提高、资本深化及私人需求韧性并存，而不必然意味着已经进入广泛衰退。  
status: STRENGTHENED  
source_pointer: [INV-AUDIT][DAILY] 2026-08-29 / 20260829-US-GROWTH-QUALITY-01-S1  
  
HYPOTHESIS_03:  
topic: 中国私人需求扩散  
hypothesis: 结构性产业增长能否通过收入、投资、信用与资产负债表渠道扩散到消费、地产、民间投资和私人信用。  
status: OPEN  
source_pointer: [INV-MONTHLY][FINAL] 2026-08 | 投资与宏观月度复盘  
  
## VALIDATION_TARGETS  
  
VT_01:  
target: 中国私人需求是否开始形成真实扩散  
linked_thesis: THESIS_01  
linked_hypothesis: HYPOTHESIS_03  
indicators:  
- 消费趋势与可持续性  
- 房地产销售、融资与投资  
- 民间投资  
- 居民与企业私人信用  
positive_confirmation: 至少两个私人需求领域跨月、相互印证地持续改善，并排除主要由低基数、一次性补贴或短期政策脉冲造成的假扩散  
disconfirming_signal: 高技术和产业增长继续强势，但消费、地产、民投和私人信用仍无法形成持续同步改善  
baseline_relevance: CHINA_AGGREGATE_DEMAND  
  
VT_02:  
target: 美国低就业流量究竟反映结构变化还是需求恶化  
linked_hypothesis: HYPOTHESIS_02  
indicators:  
- 就业修订、工时、参与率与失业率  
- 实际消费与私人国内最终需求  
- 企业利润与资本开支  
- 信用条件与招聘需求  
positive_confirmation: 就业流量偏低但消费、利润、生产率、资本投资和信用条件保持相对稳定，支持结构性解释  
disconfirming_signal: 修订后就业质量、工时、实际消费、利润和信用条件出现多维同步恶化  
baseline_relevance: US_GROWTH  
  
VT_03:  
target: AI增量资本投入能否转化为足够的增量现金回报  
linked_thesis: THESIS_02  
linked_hypothesis: HYPOTHESIS_01  
indicators:  
- 云厂商 AI 收入、Capex、FCF、ROIC 与融资成本  
- 芯片公司收入增量、毛利率、现金流与资本需求  
- 半导体设备订单、收入、利润率、现金流与客户Capex持续性  
positive_confirmation: 主要产业环节的增量收入、现金流与资本回报持续改善，并足以覆盖新增资本与融资成本  
disconfirming_signal: Capex与融资持续扩大，但现金流、ROIC、利润率或财报后资产定价持续恶化  
baseline_relevance: AI_INDUSTRY  
  
VT_04:  
target: 美国长端高位的主导驱动如何分解  
linked_thesis: THESIS_03  
indicators:  
- Fed路径与政策利率预期  
- 实际利率与长期通胀补偿  
- 期限溢价  
- 财政供给与长债拍卖需求  
- 能源价格与全球资本需求  
positive_confirmation: 能够用跨期和跨资产证据识别至少一个相对稳定、独立于其他因素的主要增量驱动  
disconfirming_signal: 长端变化主要由Fed路径单一解释，财政、期限溢价或能源变量不再具有持续的增量解释力  
baseline_relevance: US_LONG_END  
  
VT_05:  
target: 财政信用是否对黄金具有独立且持续的增量解释力  
indicators:  
- 黄金与实际利率、美元的偏离  
- 财政风险定价与长端期限溢价  
- 避险与风险溢价变化  
positive_confirmation: 在控制实际利率、美元和一般风险溢价后，财政信用相关变化仍持续解释黄金异常表现  
disconfirming_signal: 黄金重新主要由实际利率、美元与一般风险溢价解释  
baseline_relevance: GOLD_ASSET_PRICING_PRIOR  
  
## STATE_TRANSITION_TRIGGERS  
  
TRIGGER_01:  
from_state: CHINA_WEAK_PRIVATE_DEMAND_WITH_STRUCTURAL_GROWTH  
watch_state: CHINA_PRIVATE_DEMAND_BROADENING  
condition: 消费、地产、民间投资、私人信用中至少两个领域出现跨月同步改善  
confirmation: 排除低基数、一次性补贴或短期政策脉冲，并要求改善具有持续性和相互印证  
 affected_baseline: CHINA_AGGREGATE_DEMAND  
  
TRIGGER_02:  
from_state: US_GROWTH_COOLING_NO_RECESSION_CONFIRMED  
watch_state: US_RECESSION_RISK_RISING  
condition: 就业质量、工时、实际消费、企业利润和信用条件出现多维同步恶化  
confirmation: 修订后数据与至少两个非就业指标一致，并持续超过单次数据窗口  
 affected_baseline: US_GROWTH  
  
TRIGGER_03:  
from_state: AI_STRONG_DEMAND_CAPITAL_RETURN_UNRESOLVED  
watch_state: AI_CAPITAL_EFFICIENCY_STRESS  
condition: 主要产业环节的Capex与融资继续增长，而FCF、ROIC或利润率出现持续恶化  
confirmation: 至少跨多个主要公司或连续报告期出现，不以单家公司单季价格反应作为确认  
 affected_baseline: AI_INDUSTRY  
  
## ASSET_PRICING_PRIORS  
  
AI_ASSETS:  
fundamentals: AI需求与基础设施景气保持强势  
earnings_or_cashflow: 需要按商业模式验证增量收入能否持续转化为FCF、ROIC与利润率改善  
valuation_constraint: 长端利率、融资成本与高估值  
dominant_drivers:  
- 分行业资本效率  
- 长期折现率  
- 财报兑现与现金回报  
price_direction_confidence: LOW_TO_MEDIUM  
  
US_LONG_END:  
fundamentals: 美国增长未确认衰退，通胀、财政供给与能源风险仍可能支撑长期风险补偿  
earnings_or_cashflow: NOT_APPLICABLE  
valuation_constraint: Fed路径、实际利率、期限溢价、财政供给与政策流动性支持  
dominant_drivers:  
- 实际利率  
- 期限溢价  
- 财政供给  
- 能源通胀  
- 全球资本需求  
price_direction_confidence: LOW_TO_MEDIUM  
  
GOLD:  
fundamentals: 中期结构性支撑仍存在，但单一驱动框架不足  
earnings_or_cashflow: NOT_APPLICABLE  
valuation_constraint: 实际利率与美元  
dominant_drivers:  
- 实际利率  
- 美元  
- 风险溢价  
- 财政信用（候选增量变量，未确认主导）  
price_direction_confidence: MEDIUM  
  
## MORNING_RESEARCH_PRIORITIES  
  
1. 优先寻找能够证实或推翻“中国私人需求开始扩散”的跨领域、跨月证据，减少重复确认“内需仍弱”。  
2. 将美国增长研究重新聚焦到低就业流量、生产率、消费、利润和信用条件为何能够或不能共存。  
3. AI停止重复证明需求真实性，按云厂商、芯片公司和设备公司分别研究增量资本回报、现金流与融资成本。  
4. 美国长端从“是否维持高位”的方向判断转向驱动机制分解，识别Fed、实际利率、期限溢价、财政供给和能源的增量贡献。  
5. 黄金维持多因子定价框架；只有在实际利率、美元和一般风险溢价不足以解释走势时，才提高财政信用解释的研究权重。  
6. 全球成品油与炼化供给退出常规主动扩展；仅在新的出口限制、炼厂中断、库存异常、裂解价差或运输成本冲击出现时重新展开。  
  
## RESEARCH_PRIORITY_DIRECTIVES  
  
DIRECTIVE_01:  
topic_key: china_private_demand  
topic: 中国私人需求扩散  
action: MAINTAIN  
reason: 中国总需求不足与结构性产业增长并存的基线仍获支持，但继续重复证明“内需弱”的边际信息价值下降；主动研究应集中于可证伪的扩散信号。  
review_window: 2026-09-18 -> 2026-10-18  
source_final_section: 五、下一周期研究安排  
  
DIRECTIVE_02:  
topic_key: us_growth_quality  
topic: 美国增长质量  
action: REFOCUS  
reason: “是否立即进入衰退”的重复讨论信息价值下降；下一阶段需要区分低就业流量究竟来自劳动力供给/生产率结构变化，还是需求恶化的早期阶段。  
focus_on:  
- 就业修订、工时、参与率与失业率  
- 实际消费与私人国内最终需求  
- 企业利润、资本开支与生产率  
- 信用条件与招聘需求  
stop_researching:  
- 仅依据单次非农初值反复证明或反驳“美国马上衰退”  
review_window: 2026-09-18 -> 2026-10-18  
source_final_section: 五、下一周期研究安排  
  
DIRECTIVE_03:  
topic_key: ai_capital_efficiency  
topic: AI资本效率与资产回报  
action: REFOCUS  
reason: AI需求真实性已经得到较强验证，下一阶段更有判别价值的问题是不同商业模式下的增量资本投入能否转化为足够的现金回报并消化融资成本与估值。  
focus_on:  
- 云厂商 AI 收入、Capex、FCF、ROIC 和融资成本  
- 芯片公司收入增量、毛利率、现金流与资本需求  
- 半导体设备订单、收入、利润率、现金流及客户Capex持续性  
stop_researching:  
- 把“AI基础设施需求是否真实”继续作为该主题的主要研究问题  
- 用单次财报后价格反应替代资本效率证据  
review_window: 2026-09-18 -> 2026-10-18  
source_final_section: 五、下一周期研究安排  
  
DIRECTIVE_04:  
topic_key: us_long_end_independent_risk  
topic: 美国长端独立定价风险  
action: MAINTAIN  
reason: 原正式Forecast的方向已获得较强支持且9月16日已转化为独立研究主题，但其主导驱动尚未充分识别；继续研究的价值来自机制分解，而非重复验证“长端是否高”。  
review_window: 2026-09-18 -> 2026-10-18  
source_final_section: 五、下一周期研究安排  
  
DIRECTIVE_05:  
topic_key: global_refined_products_supply  
topic: 全球成品油与炼化供给  
action: EVENT_TRIGGER_ONLY  
reason: 8月30日ADD在当时具有合理研究价值，但若没有新的供给、库存、裂解价差或运输异常，不应继续占用常规高频研究预算。  
trigger_condition: 出现新的重要柴油/成品油出口限制、主要炼厂中断、馏分油库存异常、裂解价差显著冲击或运输成本异常，并可能独立改变能源通胀或企业成本判断  
review_window: 2026-09-18 -> 2026-10-18  
source_final_section: 五、下一周期研究安排  
  
## KNOWN_UNCERTAINTIES  
  
- 美国低就业流量究竟主要反映劳动力供给与生产率结构变化，还是需求恶化的早期阶段。  
- 美国长端高位中Fed、实际利率、期限溢价、财政供给、能源与全球资本需求各自的增量贡献尚未充分识别。  
- AI强需求能否在不同商业模式下持续转化为足够的FCF、ROIC与利润率，而不是主要体现为更高资本密集度和融资需求。  
- 中国结构性产业增长能否跨越政策脉冲并扩散到消费、地产、民间投资和私人信用。  
- 财政信用是否对黄金具有独立且持续的增量解释力，当前仍未确认。  
- 8月中旬Fed路径预测失败的具体方法原因尚未充分识别；不能仅凭结果倒推为“缺乏条件化”或其他已确认方法缺陷。  
  
## DO_NOT_CARRY_FORWARD  
  
- 不继续携带“2026年9月Fed维持利率”为基准预测；该判断已被现实结果否定。  
- 不把 Forecast 20260819-US-RATES-01 继续作为 OPEN Forecast；其评价窗口已结束，后续价值由 us_long_end_independent_risk 研究主题承接。  
- 不把“AI需求是否真实”作为 ai_capital_efficiency 的主要常规研究问题，除非出现真正的新机制性反证。  
- 不把黄金财政信用解释写成已经确认的主导定价因子。  
- 不把2026-08-18晨报中的中国判断迁移写成正式 Daily Ledger Baseline Event；它是 Research Process Evidence。  
- global_refined_products_supply 在 trigger_condition 未满足时不进行常规主动扩展；该退出动作以本 Handoff 的 EVENT_TRIGGER_ONLY Directive 为准。