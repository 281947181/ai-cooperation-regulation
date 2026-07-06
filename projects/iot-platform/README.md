# iot-platform 项目知识库

## 项目定位

`iot-platform` 是工业 IoT 管理平台项目，面向设备管理、设备数据采集、配置下发、流程管理和中心侧运维管理。

核心目标不是一次性演示系统，而是形成可部署、可维护、可扩展的工业 IoT 平台。

## 关联仓库

```text
仓库：281947181/iot-platform
默认分支：main
```

## 技术栈摘要

后端：

- Java 17。
- Spring Boot。
- RuoYi Fast 单体版本。
- Flowable。
- MyBatis / MyBatis Plus 按实际代码确定。

前端：

- Vue 3。
- Element Plus。
- RuoYi Vue3 前端体系。

数据与中间件：

- MySQL 或 PostgreSQL 存储业务数据。
- TDengine 或其他时序库存储高频采集数据。
- Redis。
- MQTT。
- MinIO 按文件存储需求引入。

部署：

- Docker Compose。
- 配置、数据、日志外置。
- 能外挂的应用源码、静态资源、脚本优先外挂。

## 文档导航

```text
01-architecture/README.md  架构、模块、数据和部署边界
02-development/README.md   开发约束、Codex注意事项、验证要求
03-business/README.md      设备、流程、权限、配置下发等业务模型
04-decisions/README.md     架构决策记录
05-history/README.md       已知问题、技术债、历史记录
06-prompts/README.md       项目私有Prompt片段
99-reference/README.md     术语和参考资料
```

## 信息可信度

本知识库基于历史讨论、已确认技术方向和仓库元信息整理。未直接从代码中确认的细节，应在后续开发中由 Codex 读取仓库后补充修正。
