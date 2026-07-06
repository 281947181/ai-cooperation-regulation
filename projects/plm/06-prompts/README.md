# plm 项目私有 Prompt 片段

本目录保存 plm 项目可复用 Prompt 片段。

不要保存单轮临时 Prompt。

## 每轮任务默认引用

```text
constitution.md
runtime.md
projects/plm/README.md
projects/plm/01-architecture/README.md
projects/plm/02-development/README.md
本轮涉及的业务或历史文档
tasks/task-template.md
```

## PLM 通用 Codex 前置约束

```text
本项目是已经生产运行的 PLM 产品研发及销售系统。
当前优先级为：稳定运行 > 功能迭代 > 架构优化 > 技术重构。
任务开始前必须基于远程 main 最新代码。
任务完成后默认必须 commit 并 push 到远程 main 或任务指定分支。
不得修改 plm_william 或 plm_frontend_william 作为日常开发入口。
不得大范围格式化旧代码。
不得在未评估影响前升级基础依赖。
```

## 后端任务片段

```text
后端位于 backend/。
启动命令：cd backend && mvn spring-boot:run。
构建命令：cd backend && mvn clean package -DskipTests。
构建产物：backend/target/plm-server.jar。
修改后必须说明是否影响 MySQL、Redis、MinIO、Flowable、ONLYOFFICE、权限或文件访问。
```

## 前端任务片段

```text
前端位于 frontend/。
首次运行需要复制 .env.development.example 为 .env.development。
开发命令：cd frontend && npm install && npm run dev。
生产构建：cd frontend && npm install && npm run build:prod。
构建产物：frontend/dist/。
修改后必须说明是否影响路由、菜单、权限、接口字段、文件预览或 package-lock.json。
```

## 文件预览任务片段

```text
文件预览必须通过 FilePreview + PreviewRegistry + Provider 架构实现。
业务页面不得直接依赖 PDF.js、iframe、ONLYOFFICE、MinIO URL 或文件后缀判断。
新增文件类型时，只允许新增 Provider 或扩展统一预览组件，不允许在业务页面堆叠判断。
当前 ONLYOFFICE 仅用于只读 view 模式，不启用编辑、保存回写、协同编辑、批注和历史版本。
```

## 部署任务片段

```text
当前仓库迁移基准未包含 Dockerfile、Docker Compose、Nginx 或 CI/CD 配置。
新增部署模板必须放在 deploy/。
不得提交密钥、真实 .env、数据库文件、上传文件和运行日志。
生产访问链路涉及 willexp.tech:12358 和 preview.willexp.tech:12358，修改前必须评估现网影响。
```

## 后续可补充片段

```text
flowable-history.md
minio-governance.md
onlyoffice-upgrade.md
cors-tightening.md
database-migration.md
```
