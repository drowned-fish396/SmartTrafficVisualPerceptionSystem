# 系统设计方案 V2.0 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 依据已确认的需求分析报告 V2.0，完整重写系统设计报告 V2.0，使架构、模块、数据、接口、部署和测试设计能够直接支撑后续实现与验收。

**Architecture:** 系统采用移动 Web 边端、云端服务和前端展示端三端协同架构。云端采用模块化单体设计，集中部署业务后端、两段 WebRTC 视频链路、任务视频留存、视觉算法、MySQL、本地文件存储和智能分析报告模块；前端通过 REST API、WebSocket 和 WebRTC 与云端交互。

**Tech Stack:** 移动 Web API、WebRTC、WebSocket/WSS、HTTPS REST、Python、FastAPI、OpenCV/FFmpeg、候选 YOLO/PaddleOCR/ByteTrack/FastFlow、外部多模态大模型 API、MySQL 8.4 LTS、Vue 3、TypeScript、ECharts、Windows 11、NVIDIA RTX 4060 8 GB。

## Global Constraints

- 只设计实时分析任务，不包含视频上传和离线分析任务。
- 系统全局同一时刻只允许一个活动任务。
- 边端到云端和云端到前端使用两段独立 WebRTC 视频链路。
- 云端到前端使用 WebSocket 推送结构化分析结果，前端负责标注叠加。
- 云端执行任务视频留存，完整任务视频和普通逐帧分析结果默认保存 7 天。
- 不设计模拟结果、演示数据替代或功能兜底方案。
- 智能分析报告模块归属云端，只生成单次已结束任务的报告，不设计聊天、工具调用或系统操作。
- 具体算法仍为候选路线，量化指标在真实数据和硬件实践后冻结。
- 系统设计报告采用项目建设者视角，不记录讨论过程和未采用方案。
- 正式设计图仅包含系统总体架构图、系统总体功能模块图和数据库 ER 图；其他流程与状态使用正文和表格表达。

---

### Task 1: 建立设计目录与需求追踪骨架

**Files:**
- Modify: `docs/系统设计报告V2.0.md`
- Reference: `docs/需求分析报告V2.0.md`
- Reference: `docs/superpowers/specs/2026-07-10-requirements-v2-design.md`

**Interfaces:**
- Consumes: `F-AUTH` 至 `F-SYS` 功能需求和 `A-CORE` 至 `A-REPORT` 验收要求。
- Produces: 设计章节目录、设计原则、系统边界和需求到设计追踪框架。

- [ ] **Step 1:** 删除现有 V2.0 草稿中的旧方案正文，建立引言、目标、架构、模块、流程、算法、数据、接口、前端、部署、安全、测试和追踪章节。
- [ ] **Step 2:** 在设计依据中只引用已确认的需求 V2.0、协作规格、V1.0 有效设计原则和硬件环境。
- [ ] **Step 3:** 为每个需求族明确对应设计章节，保证后续追踪表可以覆盖全部需求。
- [ ] **Step 4:** 搜索旧方案关键词，预期不出现 `RTMP`、`RTSP`、`视频上传`、`SQLite`、`模拟结果`、`工具调用`。

### Task 2: 设计总体架构与云边端协同流程

**Files:**
- Modify: `docs/系统设计报告V2.0.md`

**Interfaces:**
- Consumes: 三端职责、两段 WebRTC、WSS 控制与结果消息、全局单任务状态机。
- Produces: 总体架构、逻辑分层、部署节点、核心链路和任务时序设计。

- [ ] **Step 1:** 设计移动 Web 边端、云端模块化单体和前端展示端的职责边界。
- [ ] **Step 2:** 设计边端注册、任务创建即启动、正常结束、异常清理和全局任务名额释放流程。
- [ ] **Step 3:** 设计视频上行、播放下行、算法取帧、任务视频留存和结构化结果推送五条协同链路。
- [ ] **Step 4:** 设计最新结果直接显示、过期结果过滤、会话切换清除标注的同步机制。

### Task 3: 设计三端功能模块与权限

**Files:**
- Modify: `docs/系统设计报告V2.0.md`

**Interfaces:**
- Consumes: 管理员与监控员权限矩阵、五类场景模板、统一实时监控页面。
- Produces: 边端模块、云端模块、前端页面模块、权限校验和配置快照设计。

- [ ] **Step 1:** 设计移动 Web 边端的注册、权限、控制连接、摄像头和 WebRTC 模块。
- [ ] **Step 2:** 设计云端认证、设备、任务、视频、留存、算法、告警、历史、报告、文件和监控模块。
- [ ] **Step 3:** 设计统一实时监控页面及闸机、拥堵、禁停、道路异常专题面板。
- [ ] **Step 4:** 设计管理员维护模板与参数范围、监控员调整任务参数且不修改模板的校验流程。

### Task 4: 设计视觉算法与智能分析报告

**Files:**
- Modify: `docs/系统设计报告V2.0.md`

**Interfaces:**
- Consumes: 四类视觉业务输入输出、候选技术路线、真实数据验证流程、报告审核版本规则。
- Produces: 候选算法流水线、共享车辆检测、业务规则、告警生成和报告生成设计。

- [ ] **Step 1:** 设计 YOLO + PaddleOCR、YOLO + 区域统计、YOLO + ByteTrack、FastFlow + 道路规则四条候选流水线。
- [ ] **Step 2:** 明确候选路线验证、冻结和替换原则，不将待验证模型写成最终选型。
- [ ] **Step 3:** 设计四类交通告警的确定性规则、合并、等级和并发处理状态。
- [ ] **Step 4:** 设计报告事实模板、最小必要外发上下文、大模型正文、截图复核、审核归档和原子版本替换。

### Task 5: 设计 MySQL 与本地文件存储

**Files:**
- Modify: `docs/系统设计报告V2.0.md`

**Interfaces:**
- Consumes: 用户、设备、模型、模板、任务、逐帧结果、业务事件、告警、媒体文件和报告数据需求。
- Produces: MySQL 表结构、关系、索引、事务、文件目录和生命周期设计。

- [ ] **Step 1:** 设计用户权限、设备、模型、模板版本、任务状态和配置快照表。
- [ ] **Step 2:** 设计视频会话、逐帧结果、车牌、拥堵、禁停、道路异常和交通告警表。
- [ ] **Step 3:** 设计媒体文件、性能汇总、系统异常、操作日志和报告版本表。
- [ ] **Step 4:** 设计全局任务名额、告警领取、报告唯一有效归档的事务和并发约束。
- [ ] **Step 5:** 设计本地 `storage/` 文件目录、7 天清理、延期、校验和孤立文件检查。

### Task 6: 设计 REST、WebSocket 与 WebRTC 接口

**Files:**
- Modify: `docs/系统设计报告V2.0.md`

**Interfaces:**
- Consumes: 三端业务流程、状态机、权限、结果同步和报告流程。
- Produces: REST 端点、WSS 消息、WebRTC 信令、错误模型和幂等并发规则。

- [ ] **Step 1:** 设计认证、设备注册、模板模型、任务、告警、历史、文件和报告 REST API。
- [ ] **Step 2:** 设计边端控制 WSS 与前端实时结果 WSS 消息结构。
- [ ] **Step 3:** 设计两段 WebRTC 信令与任务、设备、视频会话的关联字段。
- [ ] **Step 4:** 设计统一响应、错误码、权限拒绝、版本冲突和请求幂等处理。

### Task 7: 设计部署、安全、可靠性与测试

**Files:**
- Modify: `docs/系统设计报告V2.0.md`

**Interfaces:**
- Consumes: Windows 11 云端硬件、独立前端电脑、局域网域名与受信证书、完整验收要求。
- Produces: 物理部署、启动顺序、安全边界、异常处理、测试矩阵和阈值冻结流程。

- [ ] **Step 1:** 设计云端电脑、前端电脑和手机的物理部署与网络端口边界。
- [ ] **Step 2:** 设计 HTTPS/WSS、账号认证、设备注册码、文件授权和大模型数据最小化。
- [ ] **Step 3:** 设计设备断线、WebRTC 故障、算法失败、存储不足、报告失败和服务停止处理。
- [ ] **Step 4:** 建立单元、接口、WebRTC、算法、前端、数据生命周期、报告和端到端验收矩阵。
- [ ] **Step 5:** 设计真实数据采集、候选技术验证和量化阈值冻结记录。

### Task 8: 全文一致性与独立读者审阅

**Files:**
- Modify: `docs/系统设计报告V2.0.md`
- Verify: `docs/需求分析报告V2.0.md`

**Interfaces:**
- Consumes: 完整系统设计报告和需求追踪表。
- Produces: 无旧方案残留、无占位、需求覆盖完整的可审阅设计报告。

- [ ] **Step 1:** 搜索 `TBD`、`TODO`、旧协议、视频上传、模拟结果、工具调用和 SQLite 残留并修正。
- [ ] **Step 2:** 逐项核对任务、告警、报告和数据状态是否与需求一致。
- [ ] **Step 3:** 核对每个需求族是否具有模块、数据、接口、页面和测试设计。
- [ ] **Step 4:** 运行 `git diff --check -- docs/系统设计报告V2.0.md`，预期无输出且退出码为 0。
- [ ] **Step 5:** 使用独立评审检查 Critical/Important 问题，修正后交用户审阅。
