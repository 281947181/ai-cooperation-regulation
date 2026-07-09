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
目标 1：通过 / 未通过 / 部分通过
说明：{{goal_1_result}}

目标 2：通过 / 未通过 / 部分通过
说明：{{goal_2_result}}

目标 3：通过 / 未通过 / 部分通过
说明：{{goal_3_result}}
```

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
