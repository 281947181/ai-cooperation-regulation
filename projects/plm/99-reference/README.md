# plm 参考资料

## 仓库内关键文档

PLM 主仓库中已确认的关键文档：

```text
README.md
backend/README.md
frontend/README.md
deploy/README.md
docs/PLM_PROJECT_BASELINE.md
docs/FILE_PREVIEW_ARCHITECTURE.md
docs/ARCHIVE_VIRTUAL_FILE_PREVIEW_ROADMAP.md
docs/migration.md
```

## 关键术语

monorepo：前端、后端、部署文档和项目文档统一维护在一个 Git 仓库中。

RuoYi：PLM 后端和前端二次开发的基础开源框架。软著或产品资产边界应排除 RuoYi 原始框架代码和第三方组件。

FilePreview：PLM 统一文件预览入口组件，业务页面不得绕过它直接实现预览。

PreviewRegistry：前端预览能力注册和分发中心，用于将不同文件类型路由到对应 Provider。

Provider：具体文件类型预览实现，如 PdfProvider、ImageProvider、OfficeProvider。

ONLYOFFICE Docs：PLM 当前用于 Office 和 PDF 只读预览的文档服务。

MinIO：PLM 业务文件统一对象存储，运行在 NAS。

## 访问与端口

本地：

```text
后端：http://localhost:18080
前端：http://localhost:18081
ONLYOFFICE Docs 本地开发：http://localhost:18082
```

生产：

```text
PLM 主系统：https://willexp.tech:12358
ONLYOFFICE 预览：https://preview.willexp.tech:12358
```

## 重要配置路径

后端：

```text
backend/src/main/resources/application.yml
backend/src/main/resources/application-druid.yml
```

前端：

```text
frontend/.env.development.example
frontend/.env.production.example
frontend/vite.config.js
frontend/package-lock.json
```

ONLYOFFICE：

```text
deploy/onlyoffice/
```

## 依赖与版本摘要

后端：

```text
Spring Boot 2.5.15
Java 17
Flowable 6.8.1
MinIO SDK 8.2.1
MySQL Connector/J 9.3.0
```

前端：

```text
Vue 3.5.26
Vite 6.4.1
Element Plus 2.13.1
Pinia 3.0.4
Vue Router 4.6.4
Axios 1.13.2
ECharts 5.6.0
bpmn-js ^18.15.0
```

ONLYOFFICE：

```text
onlyoffice/documentserver:9.0.4
```

## 参考原则

- 所有 PLM 分析默认基于 `docs/PLM_PROJECT_BASELINE.md`。
- 所有文件预览相关任务默认基于 `docs/FILE_PREVIEW_ARCHITECTURE.md`。
- 所有部署任务默认先检查 `deploy/README.md` 和 `deploy/onlyoffice/README.md`。
- 所有前端依赖变更必须检查 `frontend/package-lock.json`。
- 所有后端依赖变更必须检查 `backend/pom.xml`。
