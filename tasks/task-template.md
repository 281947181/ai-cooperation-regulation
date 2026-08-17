# 单轮任务模板

本模板用于生成具体项目中的单轮任务提示词，同时约束任务分级、提示词粒度、阅读范围和验证范围。

具体任务 Prompt 不保存在本规约仓库中，应保存在目标项目自己的文档或任务记录中。

本模板只描述“本轮任务新增要求”，不重复 `constitution.md`、`runtime.md` 和项目级规约中已经定义的长期要求。

核心原则：提示词长度、阅读范围、分析深度、测试范围和执行动作必须与任务实际复杂度成正比。禁止机械套用大型开发模板。

---

## 1. 任务分级与模板选择

生成任务提示词前，必须先判断任务等级，再决定使用本模板中的哪些部分。

任务等级由实际影响范围决定，不由描述字数、用户语气或模板名称决定。无法确定时，优先按较小等级设计；只有发现明确的跨模块影响后，才允许升级等级并说明理由。

### 1.1 S 级：微小修改

适用于单个依赖、明确 Bug、配置项、SQL、接口字段、前端样式、局部交互或文档修正。

S 级提示词通常只需要说明：

1. 修改目标。
2. 修改范围或目标文件。
3. 验证方式。
4. 提交与推送要求。

S 级默认禁止要求：

- 全仓扫描或全架构分析。
- 阅读全部 README、专题设计和架构文档。
- 无条件读取 `PROJECT_BASELINE.md`。
- 输出架构设计方案。
- 制定全量测试矩阵。
- 分析与本次修改无关的 Docker、部署或基础设施。
- 编写与修改无关的大篇幅风险报告。

一个只修改 `pom.xml`、一个 SQL、一个配置项或一个 Vue 文件的任务，不允许生成数千字提示词，除非该修改确实引发跨模块、兼容性、数据或运行态风险。

### 1.2 M 级：中型修改

适用于一个完整页面、一个业务功能、一个边界明确的模块内修改，或少量前后端联动。

M 级允许按需加入：

- 阅读 `AGENTS.md`。
- 阅读与当前阶段决策直接相关的 `PROJECT_BASELINE.md`。
- 阅读本模块相关专题文档。
- 进行模块内影响分析。
- 执行与修改范围匹配的中等规模测试。

禁止因为任务达到 M 级，就自动升级为全仓分析或全量测试。

### 1.3 L 级：大型开发

适用于新模块、跨模块业务改造、数据库结构或数据迁移调整、工作流或权限模型改造、关键集成改造，以及影响多个运行服务的修改。

L 级可以在确有必要时要求较大范围代码与依赖分析、多层级测试、Docker 与部署链路分析、相关文档同步、风险评估和回滚设计。

即使是 L 级任务，也必须说明扩大阅读、分析和验证范围的具体理由，不得把“允许”理解为“全部必做”。

### 1.4 XL 级：架构任务

仅适用于新系统或大型子系统设计、核心架构升级、跨系统数据或权限或流程或部署架构重构，以及重大技术选型和长期基础设施调整。

XL 级才允许默认考虑全仓分析、全量依赖分析、大型测试矩阵、完整 Docker 与部署分析、全文档同步和系统级风险评估。

禁止将普通 Bug、局部页面、单模块功能或单文件修改标记为 XL 级。

### 1.5 提示词长度参考

以下区间用于检查任务复杂度，不是要求凑字数：

- S：通常 50～200 字。
- M：通常 300～800 字。
- L：通常 1000～3000 字。
- XL：仅在真正的架构任务中使用完整模板，长度按必要内容确定。

内容完整性优先于字数限制。存在关键安全、数据、兼容性或生产运行风险时可以超出区间，但不得借此恢复机械套用大型模板。

## 2. 按需阅读与最小必要原则

阅读范围必须遵循最小必要原则，只读取与本次任务直接相关的内容：

- `AGENTS.md`：任务需要了解项目结构、长期边界、禁止事项或关键入口时读取。
- `PROJECT_BASELINE.md`：任务依赖当前阶段目标、已验收事实、最新决策、当前风险或下一轮边界时读取。
- `docs/`：只读取本次涉及模块或专题对应的文档。

禁止为了一个局部修改，要求阅读所有 README、所有专题设计、所有架构文档或扫描整个 `docs/`。

规约文件本身也应按需引用。任务提示词可以要求执行者遵守仓库规约，不要求每轮复制全部规约正文。

## 3. 规约引用

执行本轮任务前，Codex 必须遵守：

```text
constitution.md
runtime.md
当前任务实际需要的项目级规约与文档
```

如本轮任务要求与规约冲突，必须先汇报冲突点和建议处理方式，不得直接开发。

## 4. 任务基本信息

```text
任务等级：{{S | M | L | XL}}
项目名称：{{project_name}}
项目仓库：{{project_repository}}
目标分支：{{target_branch}}
任务标题：{{task_title}}
任务类型：{{feature | bugfix | refactor | docs | deploy | investigation | other}}
```

## 4.1 Task Contract（正式任务可用）

L、XL、高风险 M 或需要多 Agent 时，任务必须提供以下身份字段；简单 S/M 可按需省略：

```text
task_contract_id: {{PROJECT-YYYYMMDD-序号}}
task_contract_version: {{整数}}
task_contract_hash: {{SHA-256，排除本字段自身}}
issuer: ChatGPT
project: {{project_name}}
repository: {{project_repository}}
target_branch: {{target_branch}}
task_level: {{S | M | L | XL}}
contract_mode: {{INLINE | PERSISTED}}
task_contract_locator: {{确定性恢复位置}}
task_mode: {{investigation | implementation | validation | mixed}}
execution_model: {{实际 model ID}}
execution_reasoning: {{实际 reasoning}}
execution_speed: {{standard | fast}}
```

`id` 是业务任务稳定身份，`version` 是合同版本，`hash` 绑定该版本正文。Codex 不得自行修改身份字段。`INLINE` 依赖可精确恢复的原始上下文，`PERSISTED` 将合同保存于目标项目的任务记录中；无法精确恢复时不得继续验收。

### 4.1.1 Task Contract Hash canonicalization

Hash 输入是 Contract Issuer 最终签发的**完整 Markdown Task Contract 源文本**，不是字段子集、渲染结果或代码块摘录。计算步骤固定如下：

1. 将源文本解码为 UTF-8；如果开头存在 UTF-8 BOM，删除 BOM。
2. 将所有 CRLF 和单独 CR 统一为 LF；不做 Unicode 归一化。
3. 在完成换行规范化后，完整源文本必须且只能有一行匹配正则 `^task_contract_hash:[^\n]*$`（多行模式）。将该匹配行冒号后的全部内容删除，使整行精确变为 `task_contract_hash:`；保留该行原有 LF。
4. 除上一步外，字段顺序、空行、缩进、尾随空格和正文全部保持不变。
5. 保持源文本原本是否具有文件末尾 LF：原来有则保留，原来没有则不添加。
6. 对所得 UTF-8 字节序列计算 SHA-256，输出 64 位小写十六进制字符串。

缺少该字段、出现重复字段或无法按上述规则唯一定位时，Issuer 不得签发，Consumer 必须将 Contract Resolution 判为 `HASH_MISMATCH`。唯一 Bootstrap 豁免固定为 `task_contract_id=AICR-20260808-001`、`task_contract_version=1` 且 `task_contract_hash=BOOTSTRAP-NOT-ENFORCED`；任何其他 ID、Version 或 Hash 均不得声明或继承该豁免。

### 4.1.2 Task Contract Locator 可移植性与共享可解析性

`task_contract_locator` 是 Contract Acceptance 的跨执行环境接口，不是 Codex 本机的文件提示。只要任务启用 Acceptance Bridge，Locator 就必须满足“验收目标可独立解析”：在不访问 Codex 本机文件系统、临时附件目录、浏览器缓存或原执行会话私有状态的前提下，验收目标能够仅依据 Locator 恢复**完全相同的原始 Contract 源文本**并校验 Hash。

以下 Locator 在 `acceptance_bridge=required | optional` 时一律禁止：

- Codex 本机绝对或相对文件路径，例如 `/Users/...`、`/tmp/...`、`~/...`、`C:\...`。
- `.codex/attachments/`、临时粘贴文件、下载目录、会话附件缓存等仅当前执行机可见的位置。
- `localhost`、`127.0.0.1`、本机端口、浏览器本地存储、临时 HTTP 服务。
- 仅当前聊天 UI、浏览器会话或私有运行时才能解释的 opaque ID；不得把“当前上下文里能看到”误当成跨环境可解析。
- 任何无法由验收目标重新读取并取得原始字节级正文的说明性位置。

自动验收的默认策略如下：

1. `acceptance_bridge=required` 时，Contract **必须使用 `PERSISTED`**，并在 Codex 开始实现前持久化到验收目标可读取的远程事实源；不得使用依赖本地附件或仅原会话上下文的 `INLINE`。
2. 推荐将 Contract 保存到目标项目仓库的 `tasks/contracts/<task_contract_id>.md`，Locator 使用可由验收目标读取的远程 Git 位置，例如 `git://<owner>/<repo>/<ref>/tasks/contracts/<task_contract_id>.md`。
3. 优先使用不可变 commit SHA 作为 `<ref>`；如使用分支名，合同文件在签发后必须视为 immutable，不得覆盖原版本。发生正文变化时必须增加 `task_contract_version`、重新计算 Hash，并使用新的可追溯文件或版本位置。
4. `acceptance_bridge=optional` 仅在 Issuer 能确认验收目标具有同一稳定共享事实源时才允许 `INLINE`；不能确认时同样必须转为 `PERSISTED`。
5. Contract 必须先完成持久化和可解析性确认，再签发给 Codex；禁止“先让 Codex 开发，结束时再补 Locator”。

签发门禁：ChatGPT/其他 Issuer 在生成正式 Task Contract 时必须检查 Locator 类型和验收目标可访问性。若 Locator 只在执行机本地存在，不得签发带 `required` 或 `optional` Acceptance Bridge 的 Contract。

执行门禁：Codex 在开始 Implementation 前必须检查 `contract_mode`、`acceptance_bridge` 和 `task_contract_locator` 的组合。发现上述禁止 Locator、`required + INLINE`、或远程 Locator 无法读取时，必须立即停止实现并汇报 `CONTRACT_LOCATOR_INVALID` / `CONTRACT_LOCATOR_UNRESOLVABLE`；不得等到代码提交后才把必然 `BLOCKED` 的请求发送给验收目标。

修复既有无效 Locator 时，不得依据 Codex 汇报摘要重建 Contract。若仍能取得**完全相同的原始 Contract 源文本**，可以原样持久化到共享事实源并保持原 Version/Hash；若原始正文已经无法精确恢复，必须签发新 Version 和新 Hash，不得伪装成原 Contract。

### 4.1.3 Acceptance Bridge 控制（按需）

需要自动交付 ChatGPT 验收时，Task Contract 增加：

```text
acceptance_bridge: required | optional | disabled
acceptance_target_key: <canonical repository key>
acceptance_response_timeout_seconds: <positive integer>
```

只有 `required` 才把 `$acceptance-bridge` 取得有效 Result 作为闭环条件。`optional` 失败时可以回退人工交付但必须明确报告；`disabled` 不得自动发送。Target key 必须是精确规范化 key，不得写私人 conversation URL、Project ID 或浏览器会话信息。

在调用 `$acceptance-bridge` 前还必须执行 Locator Preflight：确认 Locator 满足 4.1.2，且 `PERSISTED` Contract 当前可从共享事实源读取并按 4.1.1 重新计算得到请求中的 `task_contract_hash`。Preflight 失败时不得发送 Acceptance Request，应先修复 Contract 可解析性；这属于交接协议错误，不属于实现代码验收失败。

## 5. 任务背景

S 级任务无必要背景时可省略。其他等级只保留理解本轮任务所必需的背景。

```text
{{background}}
```

## 6. 本轮任务目标

只写本轮必须完成的目标，不写长期项目规范。

```text
1. {{goal_1}}
2. {{goal_2}}
3. {{goal_3}}
```

## 7. 本轮非目标范围

仅在存在扩大范围风险时填写。

```text
- {{non_goal_1}}
- {{non_goal_2}}
- {{non_goal_3}}
```

## 8. 本轮特殊约束

只填写本轮额外约束。通用技术栈、Docker 原则、提交推送原则、搜索策略、失败处理和汇报结构不得重复粘贴。

```text
- {{constraint_1}}
- {{constraint_2}}
- {{constraint_3}}
```

## 9. 允许修改范围

```text
- {{allowed_path_1}}
- {{allowed_path_2}}
- {{allowed_path_3}}
```

## 10. 禁止修改范围

仅在确有必要时填写。

```text
- {{denied_path_or_module_1}}
- {{denied_path_or_module_2}}
- {{denied_path_or_module_3}}
```

## 11. 本轮验收要求

只写本轮特有验收项。通用验收项仍以 `runtime.md` 和 `tasks/acceptance-template.md` 为准。

```text
1. {{acceptance_1}}
2. {{acceptance_2}}
3. {{acceptance_3}}
```

验证范围必须与修改范围匹配，不得对局部修改机械要求全量测试矩阵。

## 12. 本轮特殊风险

无明确特殊风险时省略，禁止为了填满模板而编造风险。

```text
- {{risk_1}}
- {{risk_2}}
- {{risk_3}}
```

## 13. 交付要求

除非本轮任务明确声明“不提交”“不推送”或“仅本地临时验证”，否则 Codex 必须：

```text
1. 开始前基于远程仓库最新代码。
2. 完成后提交本轮全部修改。
3. 完成后推送到远程仓库。
4. 汇报 commit hash、提交消息和 push 结果。
```

未推送到远程仓库的修改，不视为 ChatGPT 与 Codex 之间可交接的完成状态。

## 14. Codex 汇报要求

Codex 完成后按 `runtime.md` 的汇报规范输出结果。
