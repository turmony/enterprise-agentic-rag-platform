# 开发任务清单

状态说明：

- `[ ]` 未开始
- `[~]` 开发中
- `[x]` 已完成

开发原则：

- 所有核心业务逻辑采用 TDD。
- 每个任务先补测试，再写实现。
- 每个阶段完成后必须能运行一条可演示链路。
- 不追求一次做全，优先打通核心 RAG 闭环。

## 1. 项目基础

- [ ] 初始化 Spring Boot 3.x + Java 17 单体项目
- [ ] 初始化 FastAPI + LangGraph Runtime 项目
- [ ] 初始化 Docker Compose：PostgreSQL、Redis、MinIO、Qdrant、Elasticsearch、Ollama
- [ ] 初始化管理后台前端：vue-vben-admin
- [ ] 初始化聊天前端：NextChat
- [ ] 建立统一代码规范、配置文件和本地启动说明

测试要求：

- [ ] Spring Boot 应用上下文启动测试
- [ ] FastAPI health check 测试
- [ ] Docker Compose 基础服务连通性测试

## 2. 用户权限与知识库

- [ ] 集成 Sa-Token 登录
- [ ] 实现用户、部门、角色基础表
- [ ] 实现知识库 `kb_space`
- [ ] 实现知识库成员权限 `kb_member`
- [ ] 实现知识库列表、创建、编辑、删除 API

测试要求：

- [ ] 登录鉴权测试
- [ ] 知识库权限判断单元测试
- [ ] 知识库 API 集成测试

## 3. 文档接入与索引

- [ ] 实现文档上传到 MinIO
- [ ] 实现 PDF / DOCX / XLSX / PPTX / MD / TXT / HTML 解析
- [ ] 实现 chunk 切分与定位信息提取
- [ ] 实现 `kb_document` 和 `kb_document_chunk`
- [ ] 调用 Ollama 生成 embedding
- [ ] 写入 Qdrant 向量索引
- [ ] 写入 Elasticsearch BM25 索引
- [ ] 实现文档解析和索引任务状态

测试要求：

- [ ] 不同文件类型解析测试
- [ ] chunk 切分边界测试
- [ ] 引用定位字段测试
- [ ] embedding 调用 mock 测试
- [ ] Qdrant / Elasticsearch 索引集成测试

## 4. Java Retrieval API

- [ ] 实现 `/internal/retrieval/search`
- [ ] 实现 query embedding
- [ ] 实现 Qdrant 向量检索
- [ ] 实现 Elasticsearch BM25 检索
- [ ] 实现检索结果合并、去重、排序
- [ ] 实现检索前置权限过滤
- [ ] 实现检索后二次权限校验
- [ ] 返回 Evidence Candidate 结构

测试要求：

- [ ] 混合检索合并排序测试
- [ ] 权限过滤测试
- [ ] 引用字段完整性测试
- [ ] Retrieval API 契约测试

## 5. LangGraph Multi-Agent MVP

- [ ] 定义 `RagAgentState`
- [ ] 实现 Supervisor Agent
- [ ] 实现 Query Analysis & Retrieval Planning Agent
- [ ] 实现 Hybrid Retrieval Agent
- [ ] 实现 Context Engineering Agent
- [ ] 实现 Answer Generation Agent
- [ ] 实现 Verification Agent
- [ ] 实现基础条件路由：PASS / REFUSE / RETRIEVE_MORE
- [ ] LangGraph 调用 Java Retrieval API

测试要求：

- [ ] Agent State schema 测试
- [ ] Router 分支测试
- [ ] Retrieval API 契约测试
- [ ] Verification verdict 测试
- [ ] 固定样例端到端 Agent 图测试

## 6. Chat、SSE 与 Trace

- [ ] 实现 `chat_session`
- [ ] 实现 `chat_message`
- [ ] 实现 `agent_run`
- [ ] 实现 `agent_trace_event`
- [ ] Spring Boot 调用 LangGraph Runtime
- [ ] LangGraph 事件回调 Spring Boot
- [ ] Spring Boot SSE 推送前端
- [ ] NextChat 改造为调用 Spring Boot SSE
- [ ] 聊天页展示 Agent Trace、引用证据、校验结果

测试要求：

- [ ] Agent Run 状态流转测试
- [ ] Trace Event 落库测试
- [ ] SSE 事件格式测试
- [ ] Chat API 集成测试

## 7. RAG 可靠性增强

- [ ] 实现 Evidence Pack 标准结构
- [ ] Answer Agent 输出结构化 citation
- [ ] Verification Agent 实现 claim extraction
- [ ] Verification Agent 实现 evidence checking
- [ ] Verification Agent 实现 citation checking
- [ ] 实现拒答策略
- [ ] 实现可靠性指标记录

测试要求：

- [ ] Citation 支持性测试
- [ ] 无证据拒答测试
- [ ] 错误引用检测测试
- [ ] Golden Set 初版测试

## 8. Human-in-the-loop 与长期记忆

- [ ] 接入 LangGraph PostgreSQL Checkpointer
- [ ] 实现 Human Review interrupt
- [ ] 实现人工审核任务表和页面
- [ ] 实现 approve / reject / edit / retrieve_more
- [ ] 实现 LangGraph resume
- [ ] 实现 `long_term_memory`
- [ ] 实现 Memory Manager Agent
- [ ] 实现长期记忆管理页面

测试要求：

- [ ] interrupt / resume 图测试
- [ ] 人工审核状态流转测试
- [ ] 长期记忆写入策略测试
- [ ] 长期记忆读取权限测试

## 9. 评测、回放与高级能力

- [ ] 实现 eval_dataset、eval_case、eval_run、eval_case_result
- [ ] 实现 RAG 批量评测
- [ ] 实现评测看板
- [ ] 实现 checkpoint 列表查看
- [ ] 实现 Time Travel 回放
- [ ] 实现 Fork Run
- [ ] 实现 Node Cache
- [ ] 实现 Retry Policy
- [ ] 实现 Prompt 版本对比

测试要求：

- [ ] 评测指标计算测试
- [ ] Time Travel 回放测试
- [ ] Fork Run 对比测试
- [ ] 缓存 key 权限隔离测试
- [ ] Retry / fallback 测试

## 完成标准

- [ ] 本地 Docker Compose 可一键启动基础依赖
- [ ] 后端、Agent Runtime、前端均有启动说明
- [ ] 至少支持 3 类文档完成上传、解析、索引、问答、引用
- [ ] 至少有 20 条 Golden Set 测试样例
- [ ] 一次问答能展示完整 Agent Trace
- [ ] 答案能展示文件名、后缀名和精确引用位置
- [ ] Verification Agent 能识别无证据答案并拒答
- [ ] 人工审核流程可以暂停并恢复 LangGraph 执行
- [ ] README 或设计文档同步更新
