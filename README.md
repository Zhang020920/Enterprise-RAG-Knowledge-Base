# Wind-RAG-Agent

一个面向**风电设备智能运维**场景的 Agentic RAG 知识平台。

项目针对风机设备手册、检修规程、故障案例、设备参数等知识分散，以及人工检索效率低、复杂故障定位困难等问题，构建基于 Java 的企业级 Agentic RAG 链路，支持智能路由、多路检索、故障问答、会话记忆及 MCP 工具调用。

在 RAG 基础能力之上，进一步针对风电领域设备型号、部件术语和故障描述进行 Embedding 领域微调，并结合 RAGAS 建立检索与生成质量评测闭环。

## 项目亮点

* **Agentic RAG 智能路由**：基于树形多级意图识别动态路由知识检索与 MCP 工具调用，结合 Query Rewrite、子问题拆分和条件重试处理复杂故障问题。
* **多路混合检索**：支持意图定向与全局检索并行召回，通过 RRF 融合候选结果，并结合 Reranker 二阶段精排提升检索质量。
* **风电运维知识工程**：支持 `PDF / Word / Excel / Markdown` 等多格式文档解析，通过 Pipeline 完成清洗、分块、元数据增强及向量化入库。
* **高并发与高可用治理**：基于 `Redis + Redisson` 实现分布式排队与并发控制，结合 Lua 原子操作、熔断降级和模型自动切换保障服务稳定性。
* **领域微调与评测闭环**：基于风电领域错召回样本构造 `Query-Positive-Hard Negative` 数据，通过 LoRA/QLoRA 微调 Embedding 模型，并使用 RAGAS 持续评估优化。

## 技术栈

* 后端框架：`Java 17`、`Spring Boot`
* 数据访问：`MyBatis-Plus`
* 向量检索：`Milvus`
* 关系数据库：`PostgreSQL`
* 缓存与并发控制：`Redis`、`Redisson`
* 消息队列：`RocketMQ`
* 文档解析：`Apache Tika`
* 大模型：`Qwen LLM`
* 向量模型：`BGE Embedding`
* 模型微调：`PEFT`、`LoRA/QLoRA`
* RAG 评估：`RAGAS`
* 工具协议：`MCP`

## 核心流程

```text
风电运维问题
      │
      ▼
Intent Detection
树形意图识别 / 查询词映射
      │
      ▼
Query Enhancement
Query Rewrite / 子问题拆分
      │
      ▼
Hybrid Retrieval
意图定向检索 + 全局检索
      │
      ▼
RRF Fusion + Reranker
候选融合 / 二阶段精排
      │
      ├──────────────► MCP Tools
      │                 工具发现与调用
      ▼
Answer Generation
基于检索上下文生成故障排查方案
      │
      ▼
RAGAS Evaluation
检索与生成质量评测
      │
      ▼
Embedding Fine-tuning
错召回样本 → Hard Negative → 领域优化
```


## 优化方向

* 基于风电设备型号、部件名称和故障描述持续扩充领域训练数据。
* 利用错召回 Case 自动构造 Hard Negative，迭代优化 Embedding 检索能力。
* 结合 RAGAS 与人工标注数据持续评估 Faithfulness、Answer Relevancy、Context Precision 和 Context Recall。
* 探索历史故障案例与实时设备数据结合，实现更智能的风机故障辅助诊断。
