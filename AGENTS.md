# 投资系统维护说明

## 适用范围

本文件适用于“投资系统”根目录及全部子目录。
它只约束 Codex 如何维护系统，不替代晨报 Prompt、月报 Prompt 或任何业务规则正文。

## 系统分工

- Gmail Draft 保存云端运行对象：冻结规则、Daily Ledger、Monthly Final 与 Handoff。
- Obsidian 保存本地规则备份、Prompt 备份、月报产出和人工可读材料。
- Scheduled Task Prompt 负责晨报与月报的日常执行。
- Chat 只用于讨论、Challenge、人工确认和展示结果，不作为长期状态或 Authority。

## 开始工作

- 不依赖聊天记忆判断当前状态。
- 涉及系统现状、版本或漏洞时，优先只读搜索 Gmail，读取当前有效的 `FROZEN` 对象及必要历史链。
- 本地文件用于备份和编辑；云端状态未核实时，不得把本地内容宣称为当前云端事实。
- 只读取完成任务所需的命名空间，不无目的遍历整个邮箱。

## Authority 与版本

- 运行时按各 Prompt 和冻结规则解析当前有效版本。
- `DOCUMENT_MODE: FULL` 直接独立执行；`DELTA` 必须沿 `BASE_VERSION` / `SUPERSEDES` 回溯到最近的 `FULL` 后按旧到新叠加。
- 当前完整版本应能脱离 v1.x 独立运行；v1.x 仅保留用于历史记录解释。
- 不允许用旧月报、聊天内容或本文件补齐缺失的正式 Authority。

## 修改与冻结

- `FROZEN` 对象遵循 write-once-read-many；不得静默修改、覆盖、删除或迁移。
- 修正规则时创建新版本；默认优先形成完整、自包含的最新版本，确有必要时才使用增量版本。
- 标准顺序：只读核对 → 本地与 Gmail `DRAFT` → 内容校验 → 用户批准 → 更新 Scheduled Task Prompt → 用户确认 → 冻结并回读验证。
- 未到用户批准门槛时必须停下，不得提前冻结。
- Gmail 与本地规则副本应保持正文一致；不建立额外哈希注册表或复杂追踪系统。

## Handoff 与 Daily

- 正常月度切换从已冻结 Monthly Final 生成一份新 Handoff。
- 月内普通变化写入 Daily Ledger，不反复生成 Handoff。
- 只有状态断裂、错误快照或用户明确要求纠偏时，才生成 Corrective Handoff，并明确取代旧 Handoff。
- Monthly Final 的本地文件名不参与云端识别；以 Gmail Subject 命名空间和正文元数据为准。

## Prompt 边界

- 晨报 Prompt 与月报 Prompt 的本地备份位于 `投资系统prompt/`。
- 两份 Scheduled Task Prompt 不保存到 Gmail。
- 默认由用户手动复制本地 Prompt 到 Scheduled Task；复制完成前不得激活依赖新 Prompt 的规则。
- 不在 `AGENTS.md` 中重复晨报方法、月报方法或 Handoff Schema。

## 授权边界

- 回答、审查、诊断或制定方案时可以执行相关只读检查，但不得据此实施写入。
- 用户明确要求修改或构建时，可完成范围内的本地变更和非破坏性校验。
- Gmail 写入、规则冻结、邮件发送、删除、覆盖历史对象或扩大工作范围，必须有用户明确授权。
- 不发送 Gmail 中的系统 Draft，除非用户明确要求发送。

## 完成标准

- 写入后回读关键对象，确认 Subject、状态、版本、生效时间和正文一致性。
- 明确报告哪些对象只是 Gmail Draft、哪些在正文语义上已 `FROZEN`，不得混淆“未发送”和“未冻结”。
- 工作结束时直接说明“已完成”，或说明当前停在哪个审批点以及用户下一步只需做什么。
