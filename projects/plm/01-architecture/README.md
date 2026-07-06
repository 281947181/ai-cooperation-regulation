# plm 架构规约

## 总体定位

PLM 是已经进入生产运行阶段的产品研发及销售系统。后续架构分析和开发默认以稳定现网为前提。

架构优先级：

```text
稳定运行 > 功能迭代 > 架构优化 > 技术重构
```

不默认建议上云迁移、Kubernetes 重构、微服务拆分或大规模基础设施改造，除非任务明确提出。

## 仓库架构

当前主仓库为 `281947181/plm`，采用 monorepo：

```text
plm/
├── backend/   # Spring Boot 后端
├── frontend/  # Vue 3 + Vite 前端
├── deploy/    # 部署模板和部署说明
├── docs/      # 项目文档、迁移记录和基线文档
├── README.md
└── .gitignore
```

后端和前端均已从旧仓库通过 `git subtree` 迁入。日常开发只以 `281947181/plm` 为入口。

## 后端架构

后端位于：

```text
backend/
```

技术栈：

- RuoYi 3.9.2。
- Spring Boot 2.5.15。
- Java 17。
- Spring Security。
- JWT。
- Redis。
- MyBatis。
- Druid。
- Flowable 6.8.1。
- MinIO SDK 8.2.1。
- ONLYOFFICE Docs 集成。
- MySQL。

后端构建产物：

```text
backend/target/plm-server.jar
```

## 前端架构

前端位于：

```text
frontend/
```

技术栈：

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

前端构建产物：

```text
frontend/dist/
```

前端开发服务器默认访问：

```text
http://localhost:18081
```

前端开发代理目标：

```text
http://localhost:18080
```

## 部署架构

生产运行方式：

- 业务服务器采用 Docker 部署。
- NAS 侧运行 MinIO 对象存储。
- PLM 前端、PLM 后端、反向代理和 ONLYOFFICE Document Server 应加入同一个 Docker 网络。
- ONLYOFFICE 不直接暴露宿主机公网端口，由反向代理通过 `preview.willexp.tech:12358` 对外提供 HTTPS 访问。

当前仓库迁移基准未包含 Dockerfile、Docker Compose、Nginx 或 CI/CD 配置。新增部署模板时必须放入 `deploy/`，且不得提交密钥、真实 `.env`、数据库文件、上传文件和运行日志。

生产入口：

```text
https://willexp.tech:12358
https://preview.willexp.tech:12358
```

生产网络基线：

```text
企业宽带动态公网 IP
↓
光猫桥接
↓
iKuai PPPoE 拨号
↓
DDNS
↓
willexp.tech
↓
外网 12358
↓
iKuai 端口转发 12358 → 服务器 443
↓
服务器反向代理
↓
PLM 服务 / ONLYOFFICE Docs
```

## 文件存储架构

对象存储使用 NAS 上的 MinIO。

所有业务文件统一存储于 MinIO，包括合同附件、项目附件、图纸、图片、工艺文件、生产相关附件和流程附件。

前端不得直接访问 MinIO 永久公开地址。所有预览文件流必须经过后端接口输出，复用登录态、数据权限和文件下载权限逻辑。

## 文件预览架构

文件预览采用统一链路：

```text
业务页面
↓
PlmFile
↓
FilePreview
↓
PreviewRegistry
↓
PdfProvider / ImageProvider / OfficeProvider
```

业务页面只能调用统一 `FilePreview` 能力，不得直接依赖：

```text
PDF.js
iframe
ONLYOFFICE
video/audio
image preview
MinIO URL
文件后缀判断
```

当前已实现或纳入统一预览链路：

- 图片预览：`jpg/jpeg/png/gif/bmp/webp`。
- Office 只读预览：`doc/docx/xls/xlsx/ppt/pptx`。
- 扩展 Office 只读预览：`wps/wpt/et/ett/dps/dpt/txt/md/csv`。
- PDF 当前统一纳入 ONLYOFFICE 只读预览，由 `OfficeProvider` 承载。

当前不开放：

- `html/htm/xml/mht/mhtml`。
- Apple iWork 格式。
- Visio 格式。
- 视频、音频、压缩包、CAD/数模图纸。

## ONLYOFFICE 架构原则

ONLYOFFICE Docs 基线版本：

```text
onlyoffice/documentserver:9.0.4
```

原则：

- 禁止使用 `latest`。
- 升级 ONLYOFFICE 镜像必须作为单独任务评估。
- 只读 `view` 模式。
- `permissions.edit=false`。
- 不启用在线编辑、保存回写、协同编辑、批注和历史版本。
- ONLYOFFICE 内部下载入口关闭，统一使用 FilePreview 标题栏中的 PLM 原文件下载。
- 后端与 Document Server 的 JWT Secret 必须一致。

生产配置中，`document-server-internal-url` 必须使用 Docker 网络内地址 `http://plm-onlyoffice-document-server`，不得配置为 `http://localhost`。

## 后续重点架构方向

- Flowable 历史数据治理。
- MinIO 对象治理。
- Docker 升级与回滚能力。
- 数据库版本管理。
- 权限模型演进。
- 前端依赖安全升级。
- 后端 CORS 收敛。
- 文件预览后续扩展。
