🧠 Personal Knowledge Management Agent


基于 Java + Spring Boot + Xiaomi MiMo 的智能个人知识管理助手


一个面向个人用户的智能知识管理Agent，支持多格式文档摄入、自动摘要标注、语义检索、知识问答和多模态理解，让你的知识库从"存了就忘"变成"随用随取"。


✨ 核心功能


功能	说明
📄 多格式文档摄入	支持 PDF、Word、Markdown、网页等格式的自动解析与结构化处理
🏷️ 智能摘要与标签	利用 MiMo 大模型自动提取文档关键信息，生成摘要并打标签分类
🔍 语义检索	基于向量数据库实现知识的语义级检索，告别关键词匹配的局限
💬 知识问答	基于个人知识库的智能问答，支持多轮对话与上下文关联
🖼️ 多模态理解	处理文档中的图表、公式等非文本内容（MiMo-V2.5 多模态能力）
🔗 知识关联推荐	自动发现文档间的关联关系，构建知识网络


🏗️ 技术架构


plaintext
┌─────────────┐
│   用户输入    │  文档上传 / 检索请求 / 问答对话
└──────┬──────┘
       │
       ▼
┌──────────────────────────────────────────────┐
│              Spring Boot 后端                 │
│                                              │
│  ┌──────────┐  ┌───────────┐  ┌───────────┐ │
│  │ 文档解析  │→│  向量化存储 │→│ 检索 + 生成 │ │
│  │ Apache    │  │ Embedding │  │ MiMo API  │ │
│  │ Tika/PDF  │  │ + Milvus  │  │ + RAG     │ │
│  └──────────┘  └───────────┘  └───────────┘ │
│       │                              │       │
│       ▼                              ▼       │
│  ┌──────────┐                  ┌───────────┐ │
│  │ 结构化存储 │                  │  知识图谱  │ │
│  │ PostgreSQL │                  │ (Optional)│ │
│  └──────────┘                  └───────────┘ │
└──────────────────────────────────────────────┘
       │
       ▼
┌─────────────┐
│   输出响应    │  摘要 / 检索结果 / 问答回答 / 关联推荐
└─────────────┘



🛠️ 技术栈


类别	技术	说明
语言	Java 17	LTS 版本，稳定可靠
框架	Spring Boot 3.x	企业级后端框架
AI 编排	LangChain4j	Java 生态的 LLM 编排框架
向量数据库	Milvus	高性能向量检索引擎
文档解析	Apache Tika	支持 50+ 种文件格式
PDF 处理	Apache PDFBox	PDF 精细解析
文档处理	Apache POI	Word / Excel 处理
大模型	MiMo-V2.5-Pro	长上下文推理，Agent 深度适配
多模态	MiMo-V2.5	文本、图像、视频、音频理解
构建工具	Maven	依赖管理与项目构建
数据库	PostgreSQL	结构化数据存储


📁 项目结构


plaintext
knowledge-management-agent/
├── src/
│   ├── main/
│   │   ├── java/com/knowledge/agent/
│   │   │   ├── KnowledgeAgentApplication.java
│   │   │   ├── config/
│   │   │   │   ├── MilvusConfig.java
│   │   │   │   ├── MimoApiConfig.java
│   │   │   │   └── AppConfig.java
│   │   │   ├── controller/
│   │   │   │   ├── DocumentController.java
│   │   │   │   ├── SearchController.java
│   │   │   │   ├── ChatController.java
│   │   │   │   └── SummaryController.java
│   │   │   ├── service/
│   │   │   │   ├── DocumentService.java
│   │   │   │   ├── EmbeddingService.java
│   │   │   │   ├── SearchService.java
│   │   │   │   ├── ChatService.java
│   │   │   │   ├── SummaryService.java
│   │   │   │   └── MultimodalService.java
│   │   │   ├── model/
│   │   │   │   ├── Document.java
│   │   │   │   ├── Chunk.java
│   │   │   │   └── ChatMessage.java
│   │   │   ├── repository/
│   │   │   │   ├── DocumentRepository.java
│   │   │   │   └── VectorRepository.java
│   │   │   └── util/
│   │   │       ├── TextSplitter.java
│   │   │       └── FileUtil.java
│   │   └── resources/
│   │       ├── application.yml
│   │       └── prompt/
│   │           ├── summary.prompt
│   │           ├── qa.prompt
│   │           └── tag.prompt
│   └── test/
│       └── java/com/knowledge/agent/
│           └── service/
│               ├── DocumentServiceTest.java
│               └── SearchServiceTest.java
├── pom.xml
└── README.md



🚀 快速开始
环境要求


JDK 17+
Maven 3.8+
Milvus 2.x（本地或远程实例）
MiMo API Key（申请地址）
安装步骤


bash
# 克隆仓库
git clone https://github.com/你的用户名/knowledge-management-agent.git
cd knowledge-management-agent

# 配置环境变量
export MIMO_API_KEY=your_api_key
export MILVUS_HOST=localhost
export MILVUS_PORT=19530

# 构建项目
mvn clean install -DskipTests

# 启动服务
mvn spring-boot:run



服务启动后访问 http://localhost:8080


📡 API 接口预览
文档管理


http
POST   /api/documents/upload        # 上传文档（支持多文件）
GET    /api/documents                # 获取文档列表
GET    /api/documents/{id}           # 获取文档详情
DELETE /api/documents/{id}           # 删除文档

语义检索


http
POST   /api/search                   # 语义检索
GET    /api/search/similar/{id}      # 查找相似文档

知识问答


http
POST   /api/chat                     # 知识库问答
GET    /api/chat/history             # 获取对话历史

摘要与标签


http
POST   /api/summary/{documentId}     # 生成文档摘要
GET    /api/tags                     # 获取所有标签
GET    /api/tags/{tag}/documents     # 按标签筛选文档



🗺️ 路线图

 v0.1 - 项目架构搭建与基础配置
 v0.2 - 多格式文档解析与结构化处理
 v0.3 - 向量化存储与语义检索
 v0.4 - 知识问答（RAG）
 v0.5 - 多模态理解与知识关联
 v1.0 - 完整功能发布与文档完善


🤝 参与贡献


欢迎提交 Issue 和 Pull Request！


Fork 本仓库
创建特性分支 (git checkout -b feature/amazing-feature)
提交改动 (git commit -m 'Add some amazing feature')
推送到分支 (git push origin feature/amazing-feature)
提交 Pull Request


📄 License


本项目基于 MIT License 开源。


🙏 致谢


Xiaomi MiMo - 提供强大的大模型能力支持
LangChain4j - Java AI 编排框架
Milvus - 高性能向量数据库
Apache Tika - 通用文档解析工具
