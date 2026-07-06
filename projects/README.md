# 项目级规约目录

本目录维护不同项目的项目级规约。

项目级规约用于描述单个项目的长期约束，包括：

- 项目目标。
- 技术栈。
- 目录结构。
- 架构边界。
- 数据边界。
- 部署方式。
- AI 开发注意事项。
- 验收要求。

项目级规约不保存单轮任务 Prompt。

## 命名建议

每个项目建议使用独立 Markdown 文件：

```text
projects/
├── iot-platform.md
├── edge-gateway.md
├── scada-platform.md
├── cad-viewer.md
└── project-template.md
```

## 使用方式

项目启动时，应先创建项目规约。

每轮任务执行前，任务提示词必须引用：

1. `constitution.md`
2. `runtime.md`
3. 当前项目对应的 `projects/*.md`
4. `tasks/task-template.md`

如果项目规约与个人顶层规约冲突，以 `constitution.md` 为准。

如果项目规约与任务要求冲突，必须先说明冲突点，不能直接开发。
