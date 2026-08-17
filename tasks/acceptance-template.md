# 任务验收模板

本模板用于 ChatGPT 或人工在 Codex 完成开发后进行验收。

验收时必须同时核对：

```text
constitution.md
runtime.md
AGENTS.md
PROJECT_BASELINE.md
docs/ 中与本轮任务相关的已验收文档
本轮任务 Prompt
Codex 汇报内容
远程仓库最新提交
```

本模板不重复长期规约，只提供验收检查结构。

---

## 1. 基本信息

```text
项目名称：{{project_name}}
任务名称：{{task_title}}
目标分支：{{target_branch}}
远程仓库：{{repository}}
提交 Hash：{{commit_hash}}
Push 状态：已推送 / 未推送 / 不适用
验收时间：{{accepted_at}}
验收人：{{reviewer}}
```

## 1.1 Task Contract 与 Acceptance 身份

```text
Task Contract ID：{{task_contract_id}}
Task Contract Version：{{task_contract_version}}
Task Contract Hash：{{task_contract_hash}}
Task Contract Locator：{{task_contract_locator}}
Task Mode：{{investigation | implementation | validation | mixed}}
Contract Resolution：FOUND_EXACT / NOT_FOUND / AMBIGUOUS / HASH_MISMATCH
Review ID：{{task_contract_id}}/{{percent-encoded-branch}}/{{commit-full-sha}}
```

验收必须先恢复原始 Contract 并校验 Hash。只有 `FOUND_EXACT` 且 Hash 匹配，才能读取 Remote Git Commit 进入实现验收；其余状态直接 `BLOCKED`。

### 1.1.1 Locator 可解析性前置门禁

在 Contract Resolution 中，验收者必须先判断 `task_contract_locator` 是否属于**验收目标可独立访问的共享事实源**。该判断先于实现代码检查，且不得用 Codex 汇报摘要、记忆或当前聊天中的二手描述代替原 Contract。

以下情况必须判定为不可接受的 Locator：

- `/Users/...`、`/tmp/...`、`~/...`、`C:\...` 等 Codex 本机路径。
- `.codex/attachments/`、临时粘贴文件、下载目录、会话附件缓存等执行机私有位置。
- `localhost`、`127.0.0.1`、本机端口、浏览器本地存储或临时 HTTP 服务。
- 只能在原聊天 UI、原浏览器会话或原运行时中解释的 opaque ID。
- 任何不能由当前验收目标重新读取完整原始 Contract 源文本的位置。

当 `acceptance_bridge=required` 时，Contract 应为 `PERSISTED`，且 Locator 指向可读取的远程持久化事实源。推荐形式为 `git://<owner>/<repo>/<ref>/tasks/contracts/<task_contract_id>.md`，优先使用不可变 commit SHA 作为 `<ref>`。

若 Request 已经到达验收目标但 Locator 属于上述本地/临时位置，则：

1. `contract_resolution` 必须为 `NOT_FOUND`，最终 `verdict` 必须为 `BLOCKED`。
2. `issues` 必须明确指出这是 Contract 交接/签发阶段的 Locator 可移植性错误，不得描述为实现代码失败。
3. 不得继续执行 Implementation Inspection，也不得根据 Codex 汇报反推需求。
4. 若能够取得**完全相同的原始 Contract 源文本**，应原样持久化到共享事实源并重新验收；若无法精确恢复原文，则必须签发新的 Version 和 Hash。

正常流程中，这类问题应在 Task Contract 签发门禁和 Codex Implementation 前置门禁中被拦截，不应等到提交代码后才暴露。

## 1.2 Multi-Agent 核对

```text
是否使用 Subagent：是 / 否
是否符合 Contract 允许范围：是 / 否 / 不适用
是否存在未经授权的 Subagent 写入：是 / 否
是否存在多个 Agent 并行修改共享源码：是 / 否
Codex 主代理是否检查最终 diff：是 / 否 / 无法确认
Codex 主代理是否执行最终验证：是 / 否 / 无法确认
Subagent 重大风险是否闭环：是 / 否 / 不适用
Terra Reviewer findings 是否已处理或解释：是 / 否 / 不适用
```

## 2. 远程协作基线核对

```text
是否基于远程最新代码开发：是 / 否 / 无法确认
是否已提交本轮修改：是 / 否 / 不适用
是否已推送到远程仓库：是 / 否 / 不适用
远程分支是否包含本轮提交：是 / 否 / 无法确认
```

如果未推送到远程仓库，默认不视为可交接完成状态，除非本轮任务明确声明“不提交”“不推送”或“仅本地临时验证”。

## 3. 项目文档基线核对

```text
是否存在 AGENTS.md：是 / 否 / 不适用
是否存在 PROJECT_BASELINE.md：是 / 否 / 不适用
是否读取了任务相关 docs/ 文档：是 / 否 / 不适用 / 无法确认
AGENTS.md 是否仍反映项目长期边界：是 / 否 / 无法确认
PROJECT_BASELINE.md 是否仍反映当前阶段状态：是 / 否 / 无法确认
```

问题说明：

```text
{{project_document_baseline_issue}}
```

## 4. 规约一致性核对

```text
是否违反 constitution.md：是 / 否 / 无法确认
是否违反 runtime.md：是 / 否 / 无法确认
是否违反 AGENTS.md：是 / 否 / 无法确认
是否违反 PROJECT_BASELINE.md：是 / 否 / 无法确认
是否存在任务 Prompt 与长期规约冲突但未说明：是 / 否 / 无法确认
```

问题说明：

```text
{{regulation_issue_detail}}
```

## 5. 任务目标核对

逐条核对本轮任务目标。

```text
AC-01：{{稳定验收项 ID}}
requirement：{{合同原文或精确引用}}
result：PASS / FAIL / PARTIAL
evidence：{{Remote Commit 中的文件、测试或运行证据}}

AC-02：{{稳定验收项 ID}}
requirement：{{合同原文或精确引用}}
result：PASS / FAIL / PARTIAL
evidence：{{证据}}

AC-N：按合同实际数量继续，不得机械限制为三项。
```

`task_mode=investigation` 时改验收：检查是否未写入文件、未提交/推送、未越过读取范围，且返回证据覆盖每个 AC；不把“未推送”判为失败。

## 6. 修改范围核对

检查 Codex 是否只修改了允许范围内的文件。

```text
允许范围内修改：是 / 否 / 无法确认
发现无关修改：是 / 否 / 无法确认
发现大范围格式化：是 / 否 / 无法确认
发现无关依赖升级：是 / 否 / 无法确认
无关修改说明：{{scope_issue_detail}}
```

## 7. 代码质量核对

检查内容：

```text
是否存在重复代码：是 / 否 / 无法确认
是否存在过度设计：是 / 否 / 无法确认
是否存在魔法值：是 / 否 / 无法确认
是否破坏项目现有风格：是 / 否 / 无法确认
是否引入无关依赖：是 / 否 / 无法确认
是否可以用明显更少代码完成：是 / 否 / 无法确认
是否存在明显安全风险：是 / 否 / 无法确认
```

问题说明：

```text
{{quality_issue_detail}}
```

## 8. 构建与测试核对

```text
后端编译：通过 / 未通过 / 未执行 / 不适用
前端构建：通过 / 未通过 / 未执行 / 不适用
单元测试：通过 / 未通过 / 未执行 / 不适用
接口测试：通过 / 未通过 / 未执行 / 不适用
Docker 配置校验：通过 / 未通过 / 未执行 / 不适用
其他验证：{{other_validation}}
```

未执行的项目必须说明原因：

```text
{{validation_skip_reason}}
```

## 9. Docker 相关核对

仅当本轮涉及 Docker、部署、镜像、Compose 或运行脚本时填写。

```text
是否优先使用 Docker Compose：是 / 否 / 不适用
配置、数据、日志是否外置：是 / 否 / 不适用
可外挂的业务内容是否优先外挂：是 / 否 / 不适用
是否存在无必要重新构建镜像：是 / 否 / 不适用
是否修改 Dockerfile：是 / 否 / 不适用
是否说明重新构建镜像的原因：是 / 否 / 不适用
```

问题说明：

```text
{{docker_issue_detail}}
```

## 10. 功能验收

说明功能是否满足预期。

```text
功能完整性：通过 / 未通过 / 部分通过 / 不适用
异常状态处理：通过 / 未通过 / 部分通过 / 不适用
权限与数据范围：通过 / 未通过 / 部分通过 / 不适用
前后端字段一致性：通过 / 未通过 / 部分通过 / 不适用
文档同步：通过 / 未通过 / 部分通过 / 不适用
```

## 11. PROJECT_BASELINE.md 动态维护核对

验收者必须判断本轮任务是否改变当前阶段动态基线。

```text
本轮是否改变当前阶段目标：是 / 否 / 无法确认
本轮是否产生新的已验收事实：是 / 否 / 无法确认
本轮是否产生新的架构/接口/部署/数据决策：是 / 否 / 无法确认
本轮是否改变风险、限制或下一轮输入：是 / 否 / 无法确认
PROJECT_BASELINE.md 是否需要更新：是 / 否 / 无法确认
PROJECT_BASELINE.md 是否已经更新并提交：是 / 否 / 不适用 / 无法确认
是否需要将稳定事实沉淀到 docs/：是 / 否 / 无法确认
```

如果需要更新，必须明确写出更新要求：

```text
PROJECT_BASELINE.md 更新要求：
1. {{baseline_update_1}}
2. {{baseline_update_2}}
3. {{baseline_update_3}}

需要沉淀到 docs/ 的内容：
1. {{docs_update_1}}
2. {{docs_update_2}}
```

## 12. 风险评估

记录当前仍存在的风险。

```text
风险 1：{{risk_1}}
影响：{{risk_1_impact}}
建议：{{risk_1_suggestion}}

风险 2：{{risk_2}}
影响：{{risk_2_impact}}
建议：{{risk_2_suggestion}}
```

## 13. 验收结论

```text
验收结论：通过 / 退回修改 / 暂缓合并
原因：{{acceptance_result_reason}}
后续动作：{{next_action}}
```

如果验收通过，后续动作必须说明下一轮是否以更新后的 `PROJECT_BASELINE.md` 为输入。

## 13.1 机器可识别结果

```text
[CODEX_ACCEPTANCE_RESULT]
protocol_version: {{版本}}
task_contract_id: {{id}}
task_contract_version: {{version}}
task_contract_hash: {{hash}}
review_id: {{review_id}}
repository: {{canonical repository key}}
branch: {{exact branch}}
commit: {{40-character lowercase Git SHA}}
contract_resolution: FOUND_EXACT | NOT_FOUND | AMBIGUOUS | HASH_MISMATCH
commit_resolution: FOUND | NOT_FOUND | MISMATCH
verdict: PASS | REWORK | BLOCKED
contract_checks:
  AC-01: PASS | FAIL | PARTIAL
  AC-N: PASS | FAIL | PARTIAL
issues:
  - {{issue_or_empty}}
next_action: {{动作}}
```

验收分为 Contract Resolution、Implementation Inspection、Contract Compliance Acceptance 三阶段。`PASS` 不能由 Terra Review 替代；`REWORK` 和 `BLOCKED` 在 V1 默认停止自动执行，禁止无限返工循环。

Bridge 未能取得有效响应时，不生成上述 ChatGPT Result；由本地 Bridge 单独记录：

```text
[CODEX_ACCEPTANCE_BRIDGE_STATUS]
protocol_version: {{版本}}
review_id: {{review_id}}
bridge_status: DELIVERY_FAILED | RESPONSE_TIMEOUT | INVALID_RESPONSE | IDENTITY_MISMATCH | TARGET_NOT_FOUND
 evidence: {{错误或超时证据}}
next_action: {{动作}}
```

## 14. 退回修改要求

如果退回，必须明确：

```text
1. 必须修改的问题：{{required_fix}}
2. 不允许修改的范围：{{denied_change_scope}}
3. 重新提交后的验证要求：{{revalidation_required}}
4. 是否允许继续沿用当前方案：{{allow_current_solution}}
5. 是否必须重新提交并推送远程：是 / 否
6. 是否必须同步修正 PROJECT_BASELINE.md：是 / 否
```
