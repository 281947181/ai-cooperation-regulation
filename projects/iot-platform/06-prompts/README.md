# iot-platform 项目私有 Prompt 片段

本目录用于保存 iot-platform 项目的可复用 Prompt 片段。

不要保存单轮临时 Prompt。

## 使用原则

每轮任务 Prompt 应引用：

```text
constitution.md
runtime.md
projects/iot-platform/README.md
本轮涉及的项目子目录文档
tasks/task-template.md
```

## 可复用片段方向

后续可按模块补充：

```text
device-management.md
flowable.md
mqtt-config.md
docker-deploy.md
frontend-page.md
```

## Codex 默认提醒

```text
本项目为工业 IoT 平台，必须遵守远程仓库优先协作原则。
任务开始前基于远程最新代码，任务完成后默认提交并推送远程仓库。
不得无目的扫描整个仓库，不得顺手重构无关模块。
```
