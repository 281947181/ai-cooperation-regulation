# 项目知识库模板

本模板用于创建 `projects/<project-name>/` 目录化项目知识库。

新项目不要再使用“一个项目一个 Markdown 文件”的模式。

## 推荐目录

```text
projects/<project-name>/
├── README.md
├── 01-architecture/
│   └── README.md
├── 02-development/
│   └── README.md
├── 03-business/
│   └── README.md
├── 04-decisions/
│   └── README.md
├── 05-history/
│   └── README.md
├── 06-prompts/
│   └── README.md
└── 99-reference/
    └── README.md
```

## README.md

应包含：

- 项目定位。
- 关联仓库。
- 默认分支。
- 当前状态。
- 技术栈摘要。
- 文档导航。
- 信息可信度说明。

## 01-architecture/README.md

应包含：

- 总体架构。
- 模块边界。
- 数据模型边界。
- 接口边界。
- 部署拓扑。
- 安全边界。

## 02-development/README.md

应包含：

- 项目特有开发规范。
- 本项目允许和禁止的修改方式。
- 编译、构建、测试命令。
- 项目特有 Codex 注意事项。

不得重复 `constitution.md` 和 `runtime.md` 中已有的通用规则。

## 03-business/README.md

应包含：

- 业务对象。
- 业务流程。
- 权限模型。
- 状态机或生命周期。
- 核心业务术语。

## 04-decisions/README.md

应包含：

- 架构决策记录索引。
- ADR 编号规则。
- 已确定的重大技术取舍。

ADR 建议命名：

```text
ADR-001-xxx.md
ADR-002-xxx.md
```

## 05-history/README.md

应包含：

- 项目里程碑。
- 重要变更。
- 已知问题。
- 技术债。
- 历史排查记录。

## 06-prompts/README.md

应包含：

- 项目私有 Prompt 片段。
- 可复用任务提示词结构。
- 模块级 Codex 提示约束。

不要保存单轮临时 Prompt。

## 99-reference/README.md

应包含：

- 术语表。
- 外部链接。
- 参考文档。
- FAQ。
