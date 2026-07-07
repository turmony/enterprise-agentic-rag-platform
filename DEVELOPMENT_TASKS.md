# 开发任务清单

本文件用于跟踪 `enterprise-agentic-rag-platform` 的实际开发进度。任务粒度按“能独立提交、能独立测试、能明确验收”拆分。

## 状态说明

- `[ ]` 未开始
- `[~]` 开发中
- `[x]` 已完成
- `[!]` 阻塞

## TDD 约定

- 每个核心模块先写测试，再写实现。
- 每个阶段至少保留一个可运行的集成测试或端到端验证路径。
- 业务逻辑不要只测 Controller，重点测试权限、检索、引用、Agent 路由、可靠性判断。
- 外部依赖优先使用 Testcontainers、Mock Server 或测试实例。
- LangGraph 节点要测试输入 state、输出 state 和条件路由结果。

## 1. 仓库与项目骨架

### 开发任务

- [x] 初始化 Git 仓库
- [x] 创建 GitHub 远端仓库
- [x] 推送 `main` 分支到远端
- [x] 添加 `.gitignore`
- [x] 添加 `.gitattributes`
- [x] 编写 `PROJECT_DESIGN.md`
- [x] 编写 `DEVELOPMENT_TASKS.md`
- [ ] 初始化根目录结构：`backend`、`agent-runtime`、`admin-frontend`、`chat-frontend`、`deploy`、`docs`
- [ ] 编写根目录 `README.md`
- [ ] 编写本地开发环境说明：JDK、Maven、Node、Python、Docker、Ollama
- [ ] 约定提交规范：`feat`、`fix`、`docs`、`test`、`refactor`、`chore`

### 测试任务

- [ ] 验证仓库 clone 后目录结构完整
- [ ] 验证 `README.md` 中启动命令可执行

### 验收标准

- [ ] 新机器 clone 后能根据文档准备开发环境
- [ ] Git 提交作者信息不包含 `chatgpt`
- [ ] 远端仓库默认分支为 `main`

## 2. Docker Compose 基础设施

### 开发任务

- [ ] 编写 `deploy/docker-compose.yml`
- [ ] 配置 PostgreSQL 16 服务
- [ ] 配置 Redis 服务
- [ ] 配置 MinIO 服务
- [ ] 配置 Qdrant 服务
- [ ] 配置 Elasticsearch 服务
- [ ] 配置 Ollama 服务连接说明
- [ ] 拉取默认主推理模型 `qwen3:14b-q4_K_M`
- [ ] 拉取快速推理模型 `qwen3:8b-q4_K_M`
- [ ] 拉取默认嵌入模型 `bge-m3:latest`
- [ ] 可选拉取长上下文实验模型 `qwen3.5:9b-q4_K_M`
- [ ] 编写 `.env.example`
- [ ] 规划本地数据卷目录
- [ ] 配置服务健康检查
- [ ] 编写基础设施启动脚本
- [ ] 编写基础设施停止脚本

### 测试任务

- [ ] PostgreSQL 连接测试
- [ ] Redis ping 测试
- [ ] MinIO bucket 创建测试
- [ ] Qdrant collection 创建测试
- [ ] Elasticsearch health 测试
- [ ] Ollama 模型列表访问测试
- [ ] Ollama 推理模型可调用测试
- [ ] Ollama Embedding 模型可调用测试
- [ ] 本机 4060 Ti 16GB 推理延迟基准测试

### 验收标准

- [ ] Windows 11 上 `docker compose up -d` 可启动全部依赖
- [ ] 所有基础设施健康检查通过
- [ ] 本地数据目录被 `.gitignore` 忽略

## 3. Spring Boot 后端基础

### 开发任务

- [ ] 初始化 Spring Boot 3.x + Java 17 项目
- [ ] 配置 Maven 多环境 profile
- [ ] 集成 MyBatis Plus
- [ ] 集成 Sa-Token
- [ ] 集成 PostgreSQL 驱动
- [ ] 集成 Redis 客户端
- [ ] 集成 MinIO SDK
- [ ] 配置统一响应结构
- [ ] 配置统一异常处理
- [ ] 配置参数校验
- [ ] 配置 OpenAPI / Swagger
- [ ] 配置日志格式
- [ ] 配置审计字段自动填充
- [ ] 配置逻辑删除

### 测试任务

- [ ] Spring Context 启动测试
- [ ] 数据库连接测试
- [ ] Redis 连接测试
- [ ] MyBatis Plus Mapper 基础测试
- [ ] 全局异常处理测试
- [ ] 参数校验测试

### 验收标准

- [ ] `backend` 可本地启动
- [ ] `/actuator/health` 或自定义 health 接口正常
- [ ] Swagger 能展示接口文档

## 4. 用户、权限与知识库

### 开发任务

- [ ] 设计 `sys_user`
- [ ] 设计 `sys_dept`
- [ ] 设计 `sys_role`
- [ ] 设计 `sys_user_role`
- [ ] 实现登录接口
- [ ] 实现退出登录接口
- [ ] 实现当前用户信息接口
- [ ] 实现基础用户管理接口
- [ ] 实现基础部门管理接口
- [ ] 实现基础角色管理接口
- [ ] 设计 `kb_space`
- [ ] 设计 `kb_member`
- [ ] 实现知识库创建接口
- [ ] 实现知识库编辑接口
- [ ] 实现知识库删除接口
- [ ] 实现知识库列表接口
- [ ] 实现知识库成员授权接口
- [ ] 实现用户可访问知识库查询
- [ ] 实现知识库访问权限判断服务

### 测试任务

- [ ] 登录成功测试
- [ ] 登录失败测试
- [ ] 未登录访问拦截测试
- [ ] 用户角色关系测试
- [ ] 知识库 owner 权限测试
- [ ] 知识库用户授权测试
- [ ] 知识库部门授权测试
- [ ] 知识库角色授权测试
- [ ] 无权限访问知识库测试

### 验收标准

- [ ] 用户只能看到自己有权限的知识库
- [ ] 知识库权限判断可被检索模块复用
- [ ] 权限相关接口有集成测试

## 5. 文档管理与解析

### 开发任务

- [ ] 设计 `kb_document`
- [ ] 设计 `kb_document_chunk`
- [ ] 设计 `kb_document_parse_task`
- [ ] 设计 `kb_document_index_task`
- [ ] 实现文档上传接口
- [ ] 上传文件保存到 MinIO
- [ ] 记录 `original_file_name`
- [ ] 记录 `display_name`
- [ ] 记录 `file_ext`
- [ ] 记录 `mime_type`
- [ ] 记录 `file_hash`
- [ ] 实现重复文件检测
- [ ] 实现 PDF 文本解析
- [ ] 实现 DOCX 文本解析
- [ ] 实现 XLSX 表格解析
- [ ] 实现 PPTX 文本解析
- [ ] 实现 Markdown 解析
- [ ] 实现 TXT 解析
- [ ] 实现 HTML 解析
- [ ] 实现解析失败状态记录
- [ ] 实现文档禁用
- [ ] 实现文档删除
- [ ] 实现文档重新解析

### 引用定位任务

- [ ] PDF 提取页码
- [ ] DOCX 提取标题路径
- [ ] DOCX 尽可能保留页码或段落序号
- [ ] XLSX 提取 sheet 名
- [ ] XLSX 提取行列范围
- [ ] PPTX 提取 slide 编号
- [ ] Markdown 提取标题路径和行号
- [ ] TXT 提取行号范围
- [ ] HTML 提取标题路径或 selector

### 测试任务

- [ ] PDF 解析测试
- [ ] DOCX 解析测试
- [ ] XLSX 解析测试
- [ ] PPTX 解析测试
- [ ] Markdown 解析测试
- [ ] TXT 解析测试
- [ ] HTML 解析测试
- [ ] 文件 hash 去重测试
- [ ] MinIO 上传测试
- [ ] 解析失败状态测试
- [ ] 引用定位字段完整性测试

### 验收标准

- [ ] 至少 7 类首期文档格式可以解析
- [ ] 每个 chunk 都能追溯到文件名、后缀名和位置
- [ ] 文档解析失败不会导致系统异常退出

## 6. Chunk 切分与索引

### 开发任务

- [ ] 设计通用 chunk 数据结构
- [ ] 实现按标题切分
- [ ] 实现按 token 长度切分
- [ ] 实现 chunk overlap
- [ ] 实现表格 chunk 策略
- [ ] 实现 PPT chunk 策略
- [ ] 实现 chunk content hash
- [ ] 保存 chunk 到 PostgreSQL
- [ ] 配置 Ollama Embedding 模型
- [ ] 默认 Embedding 模型固定为 `bge-m3:latest`
- [ ] 记录 chunk 使用的 embedding model name
- [ ] 记录 chunk 使用的 embedding vector dimension
- [ ] 实现 Embedding 模型变更后的重建索引提示
- [ ] 实现 Ollama Embedding 客户端
- [ ] 实现 embedding 批处理
- [ ] 实现 embedding 失败重试
- [ ] 创建 Qdrant collection
- [ ] 写入 Qdrant point
- [ ] 创建 Elasticsearch index
- [ ] 写入 Elasticsearch document
- [ ] 实现重新索引
- [ ] 实现索引状态统计

### 测试任务

- [ ] 短文本切分测试
- [ ] 长文本切分测试
- [ ] 标题边界切分测试
- [ ] 表格切分测试
- [ ] overlap 测试
- [ ] embedding mock 测试
- [ ] embedding 批处理测试
- [ ] Qdrant 写入测试
- [ ] Elasticsearch 写入测试
- [ ] 重复 chunk 去重测试
- [ ] 重新索引测试

### 验收标准

- [ ] 文档上传后可生成 chunk
- [ ] chunk 可写入 Qdrant 和 Elasticsearch
- [ ] 索引任务状态可查询

## 7. Java Retrieval API

### 开发任务

- [ ] 设计 `/internal/retrieval/search` 请求结构
- [ ] 设计 Evidence Candidate 响应结构
- [ ] 实现 internal token 校验
- [ ] 根据 user_id 重新查询权限
- [ ] 构建 metadata filter
- [ ] 调用 Ollama 生成 query embedding
- [ ] 查询 Qdrant
- [ ] 查询 Elasticsearch
- [ ] 合并向量检索结果
- [ ] 合并 BM25 检索结果
- [ ] 实现 chunk 去重
- [ ] 实现分数归一化
- [ ] 实现粗排序
- [ ] 实现权限前置过滤
- [ ] 实现权限后二次校验
- [ ] 实现 retrieval stats
- [ ] 返回引用定位字段
- [ ] 记录检索审计日志

### 测试任务

- [ ] internal token 缺失测试
- [ ] 用户无知识库权限测试
- [ ] metadata filter 构建测试
- [ ] Qdrant 查询 mock 测试
- [ ] Elasticsearch 查询 mock 测试
- [ ] 混合检索合并测试
- [ ] 去重测试
- [ ] 权限过滤测试
- [ ] Evidence Candidate 字段完整性测试
- [ ] Retrieval API 契约测试

### 验收标准

- [ ] LangGraph 只能通过 Java Retrieval API 检索
- [ ] 无权限 chunk 不会进入返回结果
- [ ] 返回结果包含文件名、后缀和精确定位

## 8. LangGraph Runtime 基础

### 开发任务

- [ ] 初始化 FastAPI 项目
- [ ] 配置 Python 依赖管理
- [ ] 配置 LangGraph
- [ ] 配置 Ollama LLM 客户端
- [ ] 配置默认主推理模型 `qwen3:14b-q4_K_M`
- [ ] 配置快速推理模型 `qwen3:8b-q4_K_M`
- [ ] 支持按 Agent 节点选择模型
- [ ] 支持配置 `num_ctx`、temperature、timeout
- [ ] 配置 Java Backend 客户端
- [ ] 实现 `/health`
- [ ] 实现 `/agent/runs`
- [ ] 实现 `/agent/runs/{runId}/resume`
- [ ] 定义 `RagAgentState`
- [ ] 定义 Agent 节点输入输出模型
- [ ] 定义 trace event 模型
- [ ] 实现事件回调 Java 后端
- [ ] 实现错误处理
- [ ] 实现超时配置

### 测试任务

- [ ] FastAPI health 测试
- [ ] Ollama LLM mock 测试
- [ ] 主推理模型配置读取测试
- [ ] 快速推理模型配置读取测试
- [ ] Agent 节点模型选择测试
- [ ] Java Backend client mock 测试
- [ ] `RagAgentState` schema 测试
- [ ] trace event schema 测试
- [ ] `/agent/runs` 契约测试

### 验收标准

- [ ] Agent Runtime 可独立启动
- [ ] Java 后端可调用 Agent Runtime
- [ ] Agent Runtime 可回调 Java 记录事件

## 9. LangGraph Multi-Agent MVP

### 开发任务

- [ ] 实现 Supervisor Agent
- [ ] 实现 Query Analysis & Retrieval Planning Agent
- [ ] 识别问题类型：FACT_QA、SUMMARY、COMPARE、HOW_TO、TROUBLESHOOTING、CHAT、UNSUPPORTED
- [ ] 生成 normalized question
- [ ] 生成 vector queries
- [ ] 生成 keyword queries
- [ ] 生成 metadata constraints
- [ ] 生成 retrieval strategy
- [ ] 实现 Hybrid Retrieval Agent
- [ ] 调用 Java Retrieval API
- [ ] 实现 Context Engineering Agent
- [ ] 实现 chunk 去重
- [ ] 实现上下文压缩
- [ ] 实现 Evidence Pack 构建
- [ ] 实现 Answer Generation Agent
- [ ] 生成结构化 citations
- [ ] 实现 Verification Agent
- [ ] 实现 PASS 路由
- [ ] 实现 REFUSE 路由
- [ ] 实现 RETRIEVE_MORE 路由
- [ ] 实现基础失败重试

### 测试任务

- [ ] Query Planning 输出测试
- [ ] Retrieval strategy 测试
- [ ] Evidence Pack 构建测试
- [ ] Answer citation 格式测试
- [ ] Verification verdict 测试
- [ ] PASS 分支测试
- [ ] REFUSE 分支测试
- [ ] RETRIEVE_MORE 分支测试
- [ ] 固定样例 Agent 图端到端测试

### 验收标准

- [ ] 一个问题可以完整经过多 Agent 工作流
- [ ] Agent 每个节点都有 trace event
- [ ] 最终答案必须经过 Verification Agent

## 10. Chat、SSE 与 Agent Trace

### 开发任务

- [ ] 设计 `chat_session`
- [ ] 设计 `chat_message`
- [ ] 设计 `agent_run`
- [ ] 设计 `agent_trace_event`
- [ ] 设计 `agent_checkpoint_ref`
- [ ] 实现创建会话接口
- [ ] 实现发送消息接口
- [ ] 创建 user message
- [ ] 创建 agent_run
- [ ] 调用 LangGraph Runtime
- [ ] 实现 `/internal/agent/events`
- [ ] trace event 落库
- [ ] 更新 agent_run 状态
- [ ] 实现 `/api/agent/runs/{runId}/stream`
- [ ] 推送 RUN_STARTED
- [ ] 推送 NODE_STARTED
- [ ] 推送 NODE_OUTPUT
- [ ] 推送 RETRIEVAL_RESULT
- [ ] 推送 VERIFICATION_RESULT
- [ ] 推送 TOKEN_STREAM
- [ ] 推送 RUN_COMPLETED
- [ ] 推送 RUN_FAILED
- [ ] 保存 assistant message

### 前端任务

- [ ] 初始化 NextChat 二次开发目录
- [ ] 替换默认模型请求层
- [ ] 对接 Spring Boot 发送消息接口
- [ ] 对接 SSE stream
- [ ] 展示会话列表
- [ ] 展示消息列表
- [ ] 展示流式答案
- [ ] 展示 Agent Trace 侧边栏
- [ ] 展示引用证据面板
- [ ] 展示 Verification 结果

### 测试任务

- [ ] chat session API 测试
- [ ] message API 测试
- [ ] agent_run 状态流转测试
- [ ] trace event 落库测试
- [ ] SSE 事件格式测试
- [ ] 前端 SSE 解析测试

### 验收标准

- [ ] 用户提问后页面能看到流式执行过程
- [ ] 页面能展示最终答案和引用
- [ ] 刷新页面后能恢复历史消息和 trace

## 11. RAG 可靠性增强

### 开发任务

- [ ] 定义 Evidence Pack 标准字段
- [ ] 定义 citation JSON 快照结构
- [ ] Answer Agent 严格基于 Evidence Pack 回答
- [ ] Answer Agent 输出 unsupported points
- [ ] Verification Agent 实现 claim extraction
- [ ] Verification Agent 实现 evidence checking
- [ ] Verification Agent 实现 citation checking
- [ ] Verification Agent 实现 contradiction checking
- [ ] Verification Agent 实现 confidence scoring
- [ ] 实现 hallucination flags
- [ ] 实现拒答模板
- [ ] 实现低证据重检索策略
- [ ] 实现引用快照保存
- [ ] 实现可靠性指标记录

### 测试任务

- [ ] 无 evidence 拒答测试
- [ ] citation 指向不存在 evidence 测试
- [ ] citation 不支持 claim 测试
- [ ] partial support 测试
- [ ] contradicted 测试
- [ ] hallucination flag 测试
- [ ] confidence score 阈值测试
- [ ] 引用快照历史稳定性测试

### 验收标准

- [ ] 答案关键结论有 citation
- [ ] 引用不支持答案时不能直接 PASS
- [ ] 无证据问题必须拒答或重检索

## 12. 管理后台前端

### 开发任务

- [ ] 初始化 vue-vben-admin 二次开发目录
- [ ] 配置登录页
- [ ] 对接 Sa-Token 登录接口
- [ ] 配置 API request client
- [ ] 配置菜单和路由
- [ ] 实现知识库管理页
- [ ] 实现知识库成员权限页
- [ ] 实现文档上传页
- [ ] 实现文档列表页
- [ ] 实现文档详情页
- [ ] 实现 chunk 查看页
- [ ] 实现索引任务状态页
- [ ] 实现 Agent Run 列表页
- [ ] 实现 Agent Trace 详情页
- [ ] 实现系统审计日志页
- [ ] 实现模型配置页
- [ ] 实现 Prompt 模板页

### 测试任务

- [ ] 登录页测试
- [ ] API client token 注入测试
- [ ] 知识库列表页面测试
- [ ] 文档上传页面测试
- [ ] Agent Trace 页面渲染测试
- [ ] 权限按钮显示测试

### 验收标准

- [ ] 管理后台能覆盖知识库、文档、Trace、审计核心功能
- [ ] 页面刷新后登录态正常
- [ ] 无权限用户看不到管理入口

## 13. Human-in-the-loop 与长期记忆

### 开发任务

- [ ] 接入 LangGraph PostgreSQL Checkpointer
- [ ] 保存 thread_id 与 run_id 关系
- [ ] 保存 checkpoint 引用
- [ ] 实现 HUMAN_REVIEW verdict
- [ ] 实现 LangGraph interrupt
- [ ] 设计 `human_review_task`
- [ ] 设计 `human_review_record`
- [ ] 创建人工审核任务
- [ ] Agent Run 状态改为 WAITING_REVIEW
- [ ] 实现审核通过
- [ ] 实现审核拒绝
- [ ] 实现审核编辑答案
- [ ] 实现要求补充检索
- [ ] 实现 LangGraph resume
- [ ] 设计 `long_term_memory`
- [ ] 设计 `memory_access_log`
- [ ] 实现 Memory Manager Agent
- [ ] 实现长期记忆读取
- [ ] 实现长期记忆写入候选
- [ ] 实现长期记忆启用、禁用、删除
- [ ] 实现长期记忆管理页面

### 测试任务

- [ ] Checkpointer 写入测试
- [ ] interrupt 触发测试
- [ ] approve resume 测试
- [ ] reject resume 测试
- [ ] edit resume 测试
- [ ] retrieve_more resume 测试
- [ ] 长期记忆写入策略测试
- [ ] 长期记忆读取权限测试
- [ ] memory access log 测试

### 验收标准

- [ ] 低置信度问题可以暂停等待人工审核
- [ ] 人工审核后 LangGraph 可以恢复执行
- [ ] 长期记忆可追溯、可禁用、可删除

## 14. RAG 评测体系

### 开发任务

- [ ] 设计 `eval_dataset`
- [ ] 设计 `eval_case`
- [ ] 设计 `eval_run`
- [ ] 设计 `eval_case_result`
- [ ] 实现评测集管理
- [ ] 实现评测样例导入
- [ ] 实现评测样例编辑
- [ ] 实现批量评测任务
- [ ] 调用 Agent Runtime 执行评测
- [ ] 计算 retrieval_recall
- [ ] 计算 retrieval_precision
- [ ] 计算 citation_accuracy
- [ ] 计算 answer_correctness
- [ ] 计算 faithfulness
- [ ] 计算 hallucination_rate
- [ ] 计算 refusal_accuracy
- [ ] 统计 latency_ms
- [ ] 统计 token usage
- [ ] 实现评测看板
- [ ] 实现按模型版本筛选
- [ ] 实现按 prompt 版本筛选

### 测试任务

- [ ] 评测样例导入测试
- [ ] retrieval_recall 计算测试
- [ ] citation_accuracy 计算测试
- [ ] refusal_accuracy 计算测试
- [ ] eval_run 状态流转测试
- [ ] 评测看板数据聚合测试

### 验收标准

- [ ] 至少有 20 条 Golden Set 样例
- [ ] 可以批量运行评测
- [ ] 可以查看整体指标和单 case 详情

## 15. 高级 LangGraph 能力

### 开发任务

- [ ] 展示 checkpoint 列表
- [ ] 展示 checkpoint state 摘要
- [ ] 实现 Time Travel replay
- [ ] 实现 Fork Run
- [ ] 支持修改 top_k 后重跑
- [ ] 支持修改 retrieval_strategy 后重跑
- [ ] 支持修改 prompt_version 后重跑
- [ ] 实现原 run 与 fork run 对比
- [ ] 实现 Node Cache
- [ ] cache key 加入 user_permission_hash
- [ ] cache key 加入 prompt_version
- [ ] cache key 加入 model_name
- [ ] 实现 Retry Policy
- [ ] 实现 Qdrant 失败降级
- [ ] 实现 Elasticsearch 失败降级
- [ ] 实现 Memory 写入失败不阻塞答案

### 测试任务

- [ ] Time Travel replay 测试
- [ ] Fork Run 测试
- [ ] run 对比测试
- [ ] cache key 权限隔离测试
- [ ] cache 命中测试
- [ ] retry 成功测试
- [ ] fallback 测试

### 验收标准

- [ ] 可以从指定 checkpoint 重新运行
- [ ] 可以对比两次 run 的答案、引用和指标
- [ ] 缓存不会造成越权结果复用

## 16. 项目收尾与简历材料

### 开发任务

- [ ] 补充完整 README
- [ ] 补充部署文档
- [ ] 补充接口文档
- [ ] 补充数据库 ER 图
- [ ] 补充 LangGraph 工作流图
- [ ] 补充演示数据
- [ ] 补充演示脚本
- [ ] 补充常见问题
- [ ] 整理简历项目描述
- [ ] 整理面试讲解稿
- [ ] 整理技术难点说明

### 测试任务

- [ ] 全量后端测试
- [ ] 全量 Agent Runtime 测试
- [ ] 前端构建测试
- [ ] Docker Compose 一键启动测试
- [ ] 端到端演示链路测试

### 验收标准

- [ ] 新用户可以按 README 跑通项目
- [ ] 演示链路不依赖外部云模型
- [ ] 简历描述和项目实际能力一致

## 全局完成标准

- [ ] 本地 Docker Compose 可一键启动基础依赖
- [ ] Spring Boot 后端可启动
- [ ] LangGraph Runtime 可启动
- [ ] 管理后台可登录
- [ ] 聊天页面可提问
- [ ] 至少支持 3 类文档完成上传、解析、索引、问答、引用
- [ ] 至少有 20 条 Golden Set 测试样例
- [ ] 一次问答能展示完整 Agent Trace
- [ ] 答案能展示文件名、后缀名和精确引用位置
- [ ] Verification Agent 能识别无证据答案并拒答
- [ ] 人工审核流程可以暂停并恢复 LangGraph 执行
- [ ] 长期记忆可管理、可追溯、可禁用
- [ ] 评测看板能展示核心 RAG 指标
- [ ] Time Travel 或 Fork Run 至少完成一个可演示版本
- [ ] README、设计文档、任务清单同步更新
