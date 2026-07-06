# plm 开发规约

## 基本要求

本项目开发必须遵守：

```text
constitution.md
runtime.md
projects/plm/README.md
projects/plm/01-architecture/README.md
```

本项目已经进入生产运行阶段。所有开发默认以稳定现网为前提。

## 远程协作

主开发仓库：

```text
281947181/plm
```

默认分支：

```text
main
```

每轮任务必须以远程 `main` 最新状态为基线，完成后默认提交并推送远程仓库。

旧仓库 `plm_william` 和 `plm_frontend_william` 仅用于历史追溯，不作为日常开发入口。

## 本地开发路径

推荐本地路径：

```text
/Users/zc/Documents/PLM/plm
```

推荐打开方式：

- IDEA 打开：`/Users/zc/Documents/PLM/plm/backend`
- VSCode 打开：`/Users/zc/Documents/PLM/plm/frontend`

虽然 IDE 分别打开子目录，但 Git 仓库根目录仍是：

```text
/Users/zc/Documents/PLM/plm/.git
```

前后端提交、拉取和推送均统一发生在主仓库。

## 后端开发

目录：

```text
backend/
```

启动：

```bash
cd backend
mvn spring-boot:run
```

访问：

```text
http://localhost:18080
```

构建：

```bash
cd backend
mvn clean package -DskipTests
```

构建产物：

```text
backend/target/plm-server.jar
```

启动前必须检查：

```text
backend/src/main/resources/application.yml
backend/src/main/resources/application-druid.yml
```

后端本地 IDE 开发默认激活：

```text
druid,local
```

生产 Docker 部署必须设置：

```text
SPRING_PROFILES_ACTIVE=druid,prod
```

## 前端开发

目录：

```text
frontend/
```

首次运行：

```bash
cd frontend
cp .env.development.example .env.development
npm install
npm run dev
```

访问：

```text
http://localhost:18081
```

生产构建：

```bash
cd frontend
cp .env.production.example .env.production
npm install
npm run build:prod
```

构建产物：

```text
frontend/dist/
```

前端开发服务器配置位于：

```text
frontend/vite.config.js
```

默认将 `/dev-api` 和 Springdoc 请求代理到：

```text
http://localhost:18080
```

## 环境与提交限制

不得提交：

```text
node_modules/
target/
dist/
真实 .env
日志
数据库数据卷
ONLYOFFICE 运行数据卷
上传文件
密钥
证书
```

前端实际 `.env.*` 文件不提交，只提交 `.example` 模板。

## 依赖管理

后端：

- Spring Boot 版本为 2.5.15。
- Java 版本为 17。
- Flowable 版本为 6.8.1。
- MinIO SDK 版本为 8.2.1。
- MySQL 驱动版本为 9.3.0。

前端：

- 使用 npm。
- 依赖树由 `frontend/package-lock.json` 锁定。
- Univer 相关依赖已固定兼容版本，避免图标导出不兼容导致构建失败。
- ONLYOFFICE 前端脚本由 Document Server 提供，不在前端 package 依赖中安装。
- 不得直接执行 `npm audit fix --force`，因为可能触发 ECharts 大版本升级和插件破坏性降级。

## Docker 与部署开发

当前迁入项目未包含 Dockerfile、Docker Compose、Nginx 或 CI/CD 配置，不得在未验证现网方案前擅自新增完整部署架构。

新增部署模板时：

- 必须放在 `deploy/`。
- 使用相对于仓库根目录的 `backend/` 和 `frontend/` 路径。
- 遵守配置、数据、日志外置原则。
- 遵守镜像稳定、业务内容可替换原则。
- 不提交密钥、真实 `.env`、数据库文件、上传文件和运行日志。

## Codex 修改边界

后端任务默认只读取：

```text
backend/
docs/
deploy/
```

前端任务默认只读取：

```text
frontend/
docs/
deploy/
```

文件预览、ONLYOFFICE、MinIO、Flowable、权限相关任务可跨前后端读取，但必须说明原因。

禁止：

- 大范围格式化旧代码。
- 修改旧仓库作为日常开发入口。
- 未评估影响直接升级依赖。
- 未确认生产影响直接修改域名、端口、反向代理、Docker 网络或 ONLYOFFICE 配置。
- 绕过统一 FilePreview 组件直接在业务页面实现预览逻辑。

## 验证要求

后端修改至少说明：

- 是否执行 `mvn clean package -DskipTests` 或等价验证。
- 是否影响 MySQL、Redis、MinIO、Flowable 或 ONLYOFFICE 配置。
- 是否影响权限、菜单、数据范围或文件访问。

前端修改至少说明：

- 是否执行 `npm run build:prod` 或等价验证。
- 是否影响路由、菜单、权限、接口字段或文件预览。
- 是否影响 `package-lock.json`。

部署修改至少说明：

- 是否影响现网访问链路。
- 是否影响 `willexp.tech:12358` 或 `preview.willexp.tech:12358`。
- 是否需要重新构建镜像及原因。
- 是否影响数据卷、上传文件或 ONLYOFFICE 运行数据。
