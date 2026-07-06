# plm 架构决策记录

## ADR 规则

重大技术选择必须记录 ADR。

建议命名：

```text
ADR-001-xxx.md
```

每个 ADR 应至少包含：

```text
背景：
决策：
原因：
替代方案：
影响：
后续动作：
```

## 已确定决策

### ADR-001：采用 monorepo 统一维护前后端

决策：

- 后端位于 `backend/`。
- 前端位于 `frontend/`。
- 部署和文档分别位于 `deploy/`、`docs/`。
- 旧后端仓库和旧前端仓库通过 `git subtree` 迁入并保留历史。
- 后续开发以 `281947181/plm` 为唯一主开发入口。

原因：

- 降低前后端协作和提交追踪复杂度。
- 便于 ChatGPT、Codex 和人工统一基于远程主仓库协作。

### ADR-002：当前阶段优先稳定运行，不做大规模架构重构

决策：

```text
稳定运行 > 功能迭代 > 架构优化 > 技术重构
```

原因：

- PLM 已进入生产运行阶段。
- 后续分析、开发和部署必须默认不影响现网运行。

### ADR-003：后端继续基于 RuoYi 3.9.2 + Spring Boot 2.5.15

决策：

- 维持当前 RuoYi/Spring Boot 架构。
- Java 版本为 17。
- 不因普通功能迭代升级 Spring Boot 大版本。

原因：

- 当前系统已经生产运行。
- 升级基础框架风险高，应作为单独任务评估。

### ADR-004：前端继续基于 Vue 3 + Vite + Element Plus

决策：

- 前端使用 Vue 3、Vite、Element Plus、Pinia、Vue Router、Axios。
- 前端依赖由 `package-lock.json` 锁定。
- 不直接执行 `npm audit fix --force`。

原因：

- 强制修复依赖可能触发 ECharts 大版本升级和插件破坏性降级。
- Univer 相关依赖存在兼容版本约束。

### ADR-005：文件预览统一走 FilePreview + Provider 架构

决策：

- 业务页面只调用统一 `FilePreview`。
- 文件类型分发通过 `PreviewRegistry` 和 Provider 扩展。
- 不允许业务页面直接依赖 PDF.js、ONLYOFFICE、MinIO URL 或文件后缀判断。

原因：

- 降低重复实现。
- 降低权限风险。
- 便于后续扩展 archive、cad、video、audio 等能力。

### ADR-006：Office 与 PDF 当前统一采用 ONLYOFFICE 只读预览

决策：

- Office 使用 ONLYOFFICE Docs 只读 view 模式。
- PDF 当前统一纳入 ONLYOFFICE 只读预览，由 `OfficeProvider` 承载。
- 不启用在线编辑、保存回写、协同编辑、批注和历史版本。

原因：

- 统一预览体验。
- 避免在当前阶段引入复杂协同编辑和回写风险。

### ADR-007：ONLYOFFICE 固定版本，不使用 latest

决策：

```text
onlyoffice/documentserver:9.0.4
```

禁止使用 `latest`。

原因：

- 文档服务升级可能影响预览、转换、JWT、接口兼容和运行数据。
- 升级必须作为单独任务评估。

### ADR-008：对象存储统一使用 NAS 上的 MinIO

决策：

- 所有业务文件统一存储于 MinIO。
- MinIO 运行在 NAS。
- 前端不得直接访问 MinIO 永久公开地址。

原因：

- 统一业务文件治理。
- 避免文件访问绕过后端权限控制。

### ADR-009：当前不默认引入 Kubernetes、微服务或上云迁移

决策：

默认不建议：

- 上云迁移。
- Kubernetes 重构。
- 微服务拆分。
- 大规模基础设施改造。

原因：

- 当前架构属于小团队、自建机房、自托管的务实方案。
- 当前重点是稳定性、可维护性、长期演进和风险治理。

## 待补充 ADR

```text
ADR-010：数据库版本管理机制，Flyway 或 Liquibase
ADR-011：后端 CORS 生产收敛策略
ADR-012：MinIO 对象命名和生命周期策略
ADR-013：Flowable 历史数据治理策略
ADR-014：文件预览后续扩展策略，archive/cad/video/audio
```
