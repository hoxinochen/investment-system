SCHEMA_NAME: Monthly-to-Daily Research Handoff  
SCHEMA_VERSION: 1.2  
STATUS: FROZEN  
SUPERSEDES: Monthly-to-Daily Research Handoff Schema v1.1  
AUTHORITY_ROLE: Handoff Producer Contract  
SCOPE: Frozen Monthly Final -> Self-contained Daily Research Prior  
RETENTION_POLICY: PERMANENT

版本迁移说明

本版本基于 Handoff Schema v1.1 增量升级。v1.1 的字段、Authority Boundary、生命周期、失败处理和不可变规则继续有效。

1. Research Priority State 统一

Handoff 的研究优先级必须使用稳定的 `topic_key` 和以下有限动作：  
`ADD / RAISE / MAINTAIN / LOWER / EVENT_TRIGGER_ONLY / REFOCUS / DROP`。

最低通用结构：

```text
RESEARCH_PRIORITY_DIRECTIVES:
DIRECTIVE_01:
  topic_key:
  topic:
  action:
  reason:
  focus_on:
  stop_researching:
  trigger_condition:
  reentry_condition:
  review_window:
  source_final_section:
```

字段按动作需要填写，不为模板完整硬填。`RESEARCH_PRIORITY_DIRECTIVES` 必须包含实际结构化指令，不得只写自然语言说明。

2. 与 Daily Event 的统一接口

Daily `RESEARCH_PRIORITY_CHANGED` 必须使用相同的 `topic_key` 和动作集合。  
同一 `topic_key` 在 Handoff `VALID_FROM` 之后出现更晚的正式 Daily Event 时，以该事件更新当前优先级状态，但不修改旧 Handoff。

兼容旧 Audit SPEC 时：

- `REMOVE` 解释为 `DROP`；
- `REBALANCE` 只作为历史动作读取；新事件应拆成一个或多个明确主题动作。

3. State Snapshot + Event Stream

```text
Current Research State at T
= Latest Valid FROZEN Handoff with VALID_FROM <= T
+ Handoff VALID_FROM 后至 T 前的有效 Daily Ledger Events
```

晨报只读取 Active Handoff 和后续 Daily Events，不在运行时读取 Handoff Schema。

4. 命名空间

Handoff Schema Subject：  
`[INV-MONTHLY][HANDOFF-SCHEMA] Monthly-to-Daily Research Handoff Schema v1.2`

实际月度 Handoff Subject 继续使用 `[INV-MONTHLY][HANDOFF]` 命名空间。Schema 与实际 Handoff 不得共用 Subject 命名空间。

设计原则

Monthly Final -> Handoff Generator -> Self-contained Handoff Snapshot -> Daily Event Stream -> Next Monthly Audit。
