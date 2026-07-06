# scada-platform 架构规约

## 默认技术栈方向

前端：

- Vue 3。
- Element Plus。
- Three.js / Canvas / SVG / WebGL 按场景选择。
- WebSocket 或 SSE 承载实时状态。

后端：

- Java 17。
- Spring Boot。
- 标准 REST API。
- WebSocket 网关或消息推送服务。

部署：

- Docker Compose。
- Nginx 提供静态资源和反向代理。
- 静态资源和可替换业务内容优先外挂。

## 架构原则

- 可视化对象与实时数据解耦。
- 静态模型、布局配置、实时状态分层管理。
- 告警逻辑不得只写在前端。
- 前端动画不得依赖高频全量刷新。
- 页面性能优先考虑增量更新、节流和状态差异更新。
