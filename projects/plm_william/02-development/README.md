# plm_william 开发规约

## 基本要求

本项目开发必须遵守：

```text
constitution.md
runtime.md
projects/plm_william/README.md
```

## 远程协作

该仓库默认分支为 `master`。每轮任务必须以远程 `master` 最新状态为基线，完成后默认提交并推送远程。

## 开发约束

- 未确认仓库职责前，不得与 `plm` 或 `plm_frontend_william` 混合修改。
- 不得大范围格式化历史代码。
- 不得在未确认构建命令前修改依赖版本。
- 如涉及 Docker，应优先外挂可替换业务内容，避免无必要镜像重建。

## 待补充

```text
构建命令：
启动命令：
测试命令：
主要目录：
```
