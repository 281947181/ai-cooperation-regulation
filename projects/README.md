# 项目知识库目录

本目录维护项目级规约。`projects/` 不再采用“一个项目一个 Markdown 文件”的模式，而采用“一个项目一个知识库”的模式。

项目级知识库用于沉淀单个项目的长期上下文，包括架构、开发约束、业务模型、架构决策、历史记录、项目私有 Prompt 片段和参考资料。

项目级知识库不保存单轮任务 Prompt。单轮任务 Prompt 应保存在具体项目仓库内部。

## 统一目录结构

每个项目建议使用以下结构：

```text
projects/
└── <project-name>/
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

## 目录职责

`README.md`：项目导航、仓库信息、当前状态、快速入口。

`01-architecture/`：长期稳定的技术架构、模块划分、数据模型、部署拓扑、安全设计。

`02-development/`：项目特有开发规范。只记录该项目特有规则，不重复 `constitution.md` 和 `runtime.md`。

`03-business/`：业务领域模型、业务流程、权限模型、核心对象定义。

`04-decisions/`：架构决策记录（ADR）。记录重大技术选择、取舍原因和后续影响。

`05-history/`：里程碑、重要变更、已知问题、技术债、历史排查记录。

`06-prompts/`：项目私有 Prompt 片段和可复用提示词模板。不要放顶层规约正文，不要放单轮临时 Prompt。

`99-reference/`：术语、外部链接、参考资料、FAQ。

## 使用方式

项目启动时：

1. 引用 `constitution.md`。
2. 引用 `runtime.md`。
3. 建立或更新当前项目的 `projects/<project-name>/` 知识库。
4. 在具体项目仓库内部维护单轮 Prompt 和阶段性任务记录。

每轮任务执行前，任务提示词必须引用：

1. `constitution.md`
2. `runtime.md`
3. 当前项目对应的 `projects/<project-name>/README.md`
4. 当前任务涉及的项目子目录文档
5. `tasks/task-template.md`

如果项目规约与个人顶层规约冲突，以 `constitution.md` 为准。

如果项目规约与任务要求冲突，必须先说明冲突点，不能直接开发。

## 当前项目

当前已建立知识库的项目包括：

```text
projects/iot-platform/
projects/plm/
projects/plm_william/
projects/plm_frontend_william/
projects/dev-platform/
projects/wealth_advisor/
projects/edge-gateway/
projects/scada-platform/
projects/cad-viewer/
```

其中 `edge-gateway`、`scada-platform`、`cad-viewer` 来自已讨论的项目方向，未必对应独立 GitHub 仓库。
