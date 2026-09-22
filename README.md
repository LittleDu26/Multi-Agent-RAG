# 多智能体 RAG 智能文档问答系统

基于多智能体与 RAG 架构的智能文档问答系统。通过 Agent 编排协同完成问题拆解、多路召回、结果重排序、证据校验、答案生成与质量审查；检索规划节点会结合问题类型和复杂度选择检索策略，以提升知识召回的准确性与回答可靠性。

## 技术栈

| 模块 | 技术 |
| --- | --- |
| 前端 | Vue 3、Vite、TypeScript |
| 后端 | FastAPI、Uvicorn、Pydantic Settings |
| Agent 编排 | LangChain、LangGraph、多 Agent 工作流 |
| 大模型与嵌入 | DashScope（OpenAI 兼容接口）、通义千问、Embedding |
| 混合检索 | ChromaDB 向量检索、BM25 关键词检索、Cohere Rerank |
| 知识图谱 | Neo4j、spaCy 实体与关系抽取 |
| 缓存与评估 | Redis、RAGAS、pytest |
| 工程化 | Docker Compose |

## 主要功能

### 文档处理与索引构建

支持 PDF、DOCX、TXT 文档上传、预览与解析；完成文本切块和 Embedding 生成，将文档块持久化写入 ChromaDB，并同步构建 BM25 索引与 Neo4j 实体关系图谱。

### 多 Agent 流程编排

基于 LangGraph 编排问题拆解、检索规划、召回协调、结果重排、证据校验、答案生成和质量审查等 Agent 节点，将 RAG 流程模块化，并支持链路元数据记录。

### 混合检索增强

融合 ChromaDB 向量检索、BM25 关键词检索与 Neo4j 知识图谱检索；由检索规划节点为不同问题组合检索器，兼顾语义相似、精确关键词和实体关系召回。

### 答案可靠性控制

对重排后的召回证据进行充分性校验；证据不足时回到检索规划阶段再次检索。答案生成后通过质量审查，并返回引用信息以支持结果溯源。

### 缓存与异步任务管理

使用 Redis 缓存 Embedding、查询 Embedding 和问答结果；文档变更后清理答案缓存。支持文档处理、智能问答与评估任务的异步执行和进度查询。

### 自动化评估

接入 RAGAS 构建批量评估流程，评估 faithfulness、answer relevancy、context precision、context recall 等指标，并导出 JSONL、CSV 与汇总结果。

## 项目流程

详细的文档入库、智能问答和 RAGAS 评估流程见 [流程说明](docs/workflow.md)。

## 快速启动

```bash
pip install -r requirements.txt
docker compose up -d
uvicorn src.server.main:app --reload
```

前端启动：

```bash
cd web
npm install
npm run dev
```

运行前请根据 `.env.example` 配置模型服务、Neo4j 与 Redis 连接信息。
