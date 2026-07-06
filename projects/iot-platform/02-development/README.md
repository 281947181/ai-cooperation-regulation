# iot-platform 开发规约

## 基本要求

本项目开发必须遵守：

```text
constitution.md
runtime.md
projects/iot-platform/README.md
```

本文件只记录 iot-platform 项目特有规则，不重复顶层规约。

## Codex 搜索边界

后端任务默认优先读取：

```text
backend/
src/main/java/
src/main/resources/
```

前端任务默认优先读取：

```text
frontend/
src/views/
src/api/
src/router/
src/store/
```

部署任务默认优先读取：

```text
docker/
docker-compose*.yml
nginx/
.env*
```

不得无目的扫描整个仓库。

## 后端开发约束

- 沿用 RuoYi 既有包结构、返回结构、权限注解和异常处理方式。
- 新增接口必须明确权限标识。
- 数据库变更必须提供 SQL 或迁移说明。
- 不得绕开 Service 直接在 Controller 中写复杂业务逻辑。
- 不得把 Flowable 运行状态直接当成唯一业务状态。

## 前端开发约束

- 菜单、路由、权限标识必须与后端保持一致。
- API 文件、页面文件、组件文件职责分离。
- 表单字段、列表字段必须与后端 VO/DTO 对齐。
- 不得把大量接口拼接、权限判断、状态转换堆在页面组件中。

## Docker 开发约束

- 默认使用 Docker Compose 运行和部署。
- 配置、数据、日志必须外置。
- 能外挂的应用源码、静态资源、脚本优先外挂。
- 不因普通业务代码修改而频繁重建镜像。
- 修改 Dockerfile、基础镜像、运行时依赖或启动流程时，才优先考虑重新构建镜像。

## 验证要求

后端修改至少说明：

- 是否编译通过。
- 是否涉及数据库变更。
- 是否涉及权限标识。
- 是否影响已有接口。

前端修改至少说明：

- 是否构建通过。
- 是否涉及菜单路由。
- 是否与后端字段一致。

部署修改至少说明：

- `docker compose config` 是否通过。
- 卷挂载、端口、环境变量是否清楚。
- 是否需要重新构建镜像及原因。
