# 单轮任务模板

本模板用于生成具体项目中的单轮任务提示词。

具体任务 Prompt 不保存在本规约仓库中，应保存在目标项目自己的文档或任务记录中。

本模板只描述“本轮任务新增要求”，不重复 `constitution.md`、`runtime.md` 和 `projects/*.md` 中已经定义的长期规约。

---

## 1. 规约引用

执行本轮任务前，Codex 必须阅读并遵守：

```text
constitution.md
runtime.md
projects/{{project_rule_file}}
```

如本轮任务要求与上述规约冲突，必须先汇报冲突点和建议处理方式，不得直接开发。

## 2. 任务基本信息

```text
项目名称：{{project_name}}
项目仓库：{{project_repository}}
目标分支：{{target_branch}}
任务标题：{{task_title}}
任务类型：{{feature | bugfix | refactor | docs | deploy | investigation | other}}
```

## 3. 任务背景

```text
{{background}}
```

说明当前系统状态、前置任务、问题来源或业务原因。

## 4. 本轮任务目标

只写本轮必须完成的目标，不写长期项目规范。

```text
1. {{goal_1}}
2. {{goal_2}}
3. {{goal_3}}
```

## 5. 本轮非目标范围

明确本轮不做什么，防止 Codex 扩大范围。

```text
- {{non_goal_1}}
- {{non_goal_2}}
- {{non_goal_3}}
```

## 6. 本轮特殊约束

只填写本轮额外约束。

通用技术栈、Docker 原则、提交推送原则、搜索策略、失败处理、汇报结构等，不在本节重复维护，应以 `constitution.md`、`runtime.md` 和项目级规约为准。

```text
- {{constraint_1}}
- {{constraint_2}}
- {{constraint_3}}
```

## 7. 允许修改范围

```text
- {{allowed_path_1}}
- {{allowed_path_2}}
- {{allowed_path_3}}
```

## 8. 禁止修改范围

```text
- {{denied_path_or_module_1}}
- {{denied_path_or_module_2}}
- {{denied_path_or_module_3}}
```

## 9. 本轮验收要求

只写本轮特有验收项。

通用验收项仍以 `runtime.md` 和 `tasks/acceptance-template.md` 为准。

```text
1. {{acceptance_1}}
2. {{acceptance_2}}
3. {{acceptance_3}}
```

## 10. 本轮特殊风险

```text
- {{risk_1}}
- {{risk_2}}
- {{risk_3}}
```

## 11. 交付要求

除非本轮任务明确声明“不提交”“不推送”或“仅本地临时验证”，否则 Codex 必须遵守远程仓库优先协作原则：

```text
1. 开始前基于远程仓库最新代码。
2. 完成后提交本轮全部修改。
3. 完成后推送到远程仓库。
4. 汇报 commit hash、提交消息和 push 结果。
```

未推送到远程仓库的修改，不视为 ChatGPT 与 Codex 之间可交接的完成状态。

## 12. Codex 汇报要求

Codex 完成后按 `runtime.md` 的汇报规范输出结果。

本轮额外需要汇报：

```text
- {{extra_report_item_1}}
- {{extra_report_item_2}}
- {{extra_report_item_3}}
```

如无额外汇报要求，填写“无”。
