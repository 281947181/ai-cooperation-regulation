# plm 项目知识库

## 项目定位

`plm` 是 PLM 产品研发及销售系统的主仓库，已进入生产运行阶段。

当前优先级：

```text
稳定运行 > 功能迭代 > 架构优化 > 技术重构
```

后续分析、开发和部署建议默认以“不影响现网运行”为前提，不轻易推翻已有架构。

## 关联仓库

```text
仓库：281947181/plm
默认分支：main
仓库形态：monorepo，前后端统一维护
```

历史来源：

```text
原后端仓库：281947181/plm_william
原前端仓库：281947181/plm_frontend_william
```

后端和前端已通过 `git subtree` 迁入当前主仓库并保留历史。后续开发默认以 `281947181/plm` 为唯一主开发入口，旧仓库仅作为历史来源和追溯依据，不再作为日常开发入口。

## 当前目录结构

```text
plm/
├── backend/   # Spring Boot 后端项目根目录
├── frontend/  # Vue 3 + Vite 前端项目根目录
├── deploy/    # 部署模板和部署说明
├── docs/      # 项目文档、迁移记录和基线文档
├── README.md
└── .gitignore
```

## 技术栈摘要

后端：

- RuoYi 3.9.2。
- Spring Boot 2.5.15。
- Java 17。
- Spring Security。
- JWT。
- Redis。
- MyBatis。
- Druid。
- Flowable。
- MinIO SDK。
- ONLYOFFICE Docs 集成。
- MySQL。

前端：

- Vue 3。
- Vite。
- Element Plus。
- Pinia。
- Vue Router。
- Axios。
- ECharts。
- BPMN / bpmn-js。
- Univer。
- ONLYOFFICE Docs API。

部署：

- 生产运行采用 Docker。
- 当前仓库迁移基准未包含 Dockerfile、Docker Compose、Nginx 或 CI/CD 配置。
- 当前发布产物为后端 `backend/target/plm-server.jar` 和前端 `frontend/dist/`。
- 仓库级部署文件统一放在 `deploy/`。

## 本地开发入口

后端：

```bash
cd backend
mvn spring-boot:run
```

后端访问：

```text
http://localhost:18080
```

前端：

```bash
cd frontend
cp .env.development.example .env.development
npm install
npm run dev
```

前端访问：

```text
http://localhost:18081
```

前端开发代理目标：

```text
http://localhost:18080
```

## 文档导航

```text
01-architecture/README.md  架构、部署、文件预览、网络和数据边界
02-development/README.md   开发约束、构建命令、Codex注意事项
03-business/README.md      产品研发、合同需求、项目立项、物料、生产、附件和流程业务
04-decisions/README.md     架构决策记录
05-history/README.md       迁移历史、现网状态、风险和技术债
06-prompts/README.md       项目私有Prompt片段
99-reference/README.md     术语、关键文档和参考资料
```

## 信息可信度

本知识库基于 `281947181/plm` 仓库中的 README、项目基线文档、前后端说明、部署说明、依赖配置和文件预览架构文档整理。

如项目代码后续变化，应同步更新本知识库。
