# Enterprise-RAG-Knowledge-Base

一个面向企业制度文档的 Agentic RAG 项目。项目目标不是简单做“文档问答 Demo”，而是模拟企业员工在请假、报销、IT 权限、采购审批、预算付款、信息安全等制度场景下的真实提问，并用可评估、可复盘的工程链路提升回答准确性。

当前系统支持多格式制度文档解析、混合检索、查询改写、意图识别、重排序、检索质量控制、Redis 分层缓存、FastAPI 接口、Streamlit 页面，以及 RAGAS + 自定义指标的离线评估。


## 项目亮点

- 基于 `LangGraph` 搭建 `改写 -> 检索 -> 反思 -> 生成` 的 Agentic RAG 工作流。
- 使用 `BM25 + Milvus 向量检索 + RRF 融合`，兼顾关键词命中和语义召回。
- 加入查询意图识别与制度类型过滤，提升复杂制度场景下的检索稳定性。
- 使用 `BGE-Reranker` 做重排序，并加入分数阈值、断崖截断、去重、来源规则加权等质量控制。
- 支持 Redis 分层缓存，包括检索缓存、回答缓存和版本化缓存键。
- 支持 `PDF / Word / Excel / Markdown / TXT` 等制度文档扩展，适合模拟真实企业知识库。
- 提供复杂评估集、RAGAS 指标、自定义检索指标、分类统计和错误样本分析。

## 技术栈

- 工作流编排：`LangGraph`、`LangChain`
- 大模型：`qwen-plus`，通过 OpenAI 兼容接口调用
- 向量模型：`BGE-M3`
- 向量库：`Milvus`
- 关键词检索：`rank_bm25`
- 重排序：`BGE-Reranker-v2`
- API 服务：`FastAPI`
- 前端演示：`Streamlit`
- 缓存：`Redis`
- 评估：`RAGAS`、`Pandas`
- 部署：`Docker`、`Docker Compose`

## 核心流程

```text
用户问题
  -> Query Rewrite：结合历史对话改写口语化问题
  -> Intent Detection：识别请假、报销、IT、采购、预算等制度意图
  -> Hybrid Retrieval：BM25 + 向量检索 + 多查询融合
  -> Rerank & Filter：FlashRank 重排、阈值过滤、去重、来源规则加权
  -> Reflection：判断检索质量是否足够，不足时重试
  -> Answer Generation：严格基于制度文档生成结构化答案



## 项目结构

```text
.
├── src/
│   ├── agent/              # LangGraph 工作流、节点和状态
│   ├── api/                # FastAPI 接口
│   ├── cache/              # Redis 缓存
│   ├── document/           # 文档解析、切分、元数据
│   ├── evaluation/         # 评估指标与数据处理
│   └── retrieval/          # 混合检索、意图识别、查询改写、重排序
├── data/raw/               # 原始制度文档
├── data/raw/enhanced/      # 增强版制度语料
├── test/                   # 索引、问答、评估、消融实验脚本
├── reports/                # 评估报告输出
├── docker-compose.yml
├── Dockerfile
├── requirements.txt
├── .env.example
└── README.md
```
