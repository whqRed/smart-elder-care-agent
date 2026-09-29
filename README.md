# smart-elder-care-agent
# Smart Elder Care Agent

基于 Java 微服务与 LangGraph 多 Agent 架构的智慧养老护理智能平台。

## 📖 项目介绍

Smart Elder Care Agent 是一个面向养老机构老人入住审核、健康管理以及养老知识服务场景的 AI 应用平台。

项目通过 Java 微服务承载核心业务流程，通过 Python Agent 服务提供智能推理能力，构建从健康数据采集、体检报告解析、健康指标结构化提取、健康风险评估到养老知识问答的完整 AI 应用链路。

系统采用 **Java + Python 跨语言微服务架构**：

- Java 服务负责用户管理、老人档案、健康数据、任务调度等业务能力。
- Python Agent 服务负责大模型调用、RAG 检索、多 Agent 协同推理等智能能力。

通过 LangGraph 构建 Supervisor Agent 调度体系，实现多个专业 Agent 的协同工作。

---

## 🏗️ 系统架构

```
                         User
                           |
                           |
                    Vue Frontend
                           |
                           |
              Spring Cloud Gateway
                           |
        --------------------------------
        |                              |
        |                              |
 Java Backend Service          Python Agent Service
        |                              |
        |                         LangGraph
        |                              |
        |                  -------------------------
        |                  |                       |
        |              QA Agent          Assessment Agent
        |                  |                       |
        |                  |                       |
        |              RAG知识库          健康风险评估
        |
        |
  -----------------------------
  |             |             |
 MySQL        Redis       RabbitMQ


                 Knowledge Base

                       |
        --------------------------------
        |                              |
     Milvus                       BGE-Reranker
        |
     BGE-M3
```

---

# ✨ 核心功能


## 1. 老人健康档案管理

支持养老机构老人基础信息以及健康数据管理。

主要功能：

- 老人基础信息管理
- 健康指标存储
- 历史体检数据查询
- 健康状态追踪

---

## 2. 体检报告智能解析

针对养老机构中大量 PDF 体检报告，通过 AI 自动完成非结构化数据解析。


处理流程：

```
PDF体检报告

        ↓

MinerU 文档解析

        ↓

文本内容提取

        ↓

LLM结构化信息抽取

        ↓

健康指标 JSON

        ↓

保存健康档案
```


实现能力：

- PDF 自动解析
- 健康指标提取
- 异常指标识别
- 结构化数据入库

---

# 3. LangGraph 多 Agent 协同


系统采用 Supervisor Agent 架构。


```
                    User

                     |

             Supervisor Agent

              /             \

             /               \

        QA Agent       Assessment Agent

             |               |

        知识检索          健康评估

```


Agent 职责：

| Agent            | 功能                                  |
| ---------------- | ------------------------------------- |
| Supervisor Agent | 根据用户需求进行任务规划和 Agent 调度 |
| QA Agent         | 养老知识问答、健康知识解释            |
| Assessment Agent | 健康风险分析、入住建议生成            |

---

# 4. RAG 知识增强问答


针对养老护理知识领域，构建知识增强问答系统。


流程：

```
养老知识文档

        ↓

文本切分

        ↓

BGE-M3 Embedding

        ↓

Milvus 向量数据库

        ↓

Hybrid Search

        ↓

BGE-Reranker 重排序

        ↓

LLM生成回答
```


优化方案：

- 父子文档切分
- 向量检索
- 混合检索
- 重排序降低模型幻觉
- Redis 热点问题缓存

---

# 5. 健康风险智能评估


结合老人健康指标以及养老护理标准知识库，实现智能辅助评估。


流程：

```
老人健康数据

        ↓

Function Calling

        ↓

获取健康档案

        ↓

RAG检索评估标准

        ↓

LLM分析

        ↓

生成评估结果
```


输出：

- 健康风险等级
- 异常指标分析
- 风险原因
- 护理建议

---

# 6. 异步任务处理


针对 PDF 解析以及大模型推理耗时较长的问题，引入 RabbitMQ 实现异步任务解耦。


流程：

```
用户上传报告

        ↓

Java生成任务ID

        ↓

RabbitMQ消息队列

        ↓

Python Agent消费

        ↓

AI任务处理

        ↓

结果回调Java
```


设计：

- 雪花算法生成任务 ID
- Redis SETNX 实现幂等控制
- 任务状态管理

---

# 🛠 技术栈


## Backend

- Java
- Spring Boot 3
- Spring Cloud Gateway
- MyBatis-Plus
- MySQL
- Redis
- RabbitMQ
- Nacos


## Agent Service

- Python
- FastAPI
- LangChain
- LangGraph


## AI

- LLM
- RAG
- Function Calling
- BGE-M3
- BGE-Reranker
- Milvus Lite
- MinerU

---

# 📂 项目结构


```
smart-elder-care-agent

├── backend
│   └── elder-service
│
├── agent-service
│   ├── agents
│   ├── rag
│   ├── tools
│   └── prompts
│
├── frontend
│
├── docs
│   ├── architecture.md
│   ├── database.md
│   └── api.md
│
└── docker
    └── docker-compose.yml

```

---

# 🚀 Quick Start


## Backend

```bash
cd backend/elder-service

mvn spring-boot:run
```


## Agent Service

```bash
cd agent-service

pip install -r requirements.txt

uvicorn app.main:app
```

---

# 📌 Development Roadmap


- [x] 项目架构设计

- [ ] Java业务服务开发

- [ ] Python Agent服务开发

- [ ] LangGraph Supervisor Agent

- [ ] RAG知识库构建

- [ ] 体检报告智能解析

- [ ] 健康风险评估

- [ ] 前后端联调

- [ ] Docker部署

---

# 🔮 Future Optimization


- Agent Memory
- Context Compression
- SSE 流式输出
- MCP 工具扩展
- 更多养老业务场景

---

# License

MIT