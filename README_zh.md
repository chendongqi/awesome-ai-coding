# Awesome AI Coding

> 一个精心整理的 AI 编程相关资源集合，涵盖代码生成、智能体开发、RAG、LLM 应用等领域的优秀工具、框架和项目。

[![Awesome](https://cdn.jsdelivr.net/gh/sindresorhus/awesome@d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

[English](README.md) | [中文](#)

> 💡 **提示**: 使用 `Ctrl+F` 或 `Cmd+F` 快速搜索你需要的资源

> 📝 **持续更新中**: 欢迎提交 Issue 或 Pull Request 来补充更多优质资源

---

## 📑 目录

- [AI 代码编辑器与 IDE](#ai-代码编辑器与-ide)
- [AI 智能体框架](#ai-智能体框架)
- [RAG 与知识库](#rag-与知识库)
- [LLM 开发框架](#llm-开发框架)
- [代码审查与测试](#代码审查与测试)
- [提示词工程](#提示词工程)
- [MCP 协议与工具](#mcp-协议与工具)
- [代码分析与理解](#代码分析与理解)
- [Agent Skills](#agent-skills)
- [开发工具与加速](#开发工具与加速)
- [学习资源](#学习资源)
- [ClaudeCode Rules](#claudecode-rules)

---

## AI 代码编辑器与 IDE

### 桌面编辑器

- **[Cursor](https://cursor.sh/)** - AI 代码编辑器，支持代码生成、重构和智能补全
- **[GitHub Copilot](https://github.com/features/copilot)** - GitHub 官方 AI 代码补全工具
- **[Replit](https://replit.com/)** - 在线 AI 编程环境
- **[Bolt](https://bolt.new/)** - 通过 AI 聊天创建应用和网站的在线平台
- **[Devin AI](https://www.cognition-labs.com/)** - AI 软件工程师
- **[Trae](https://trae.io/)** - 字节跳动推出的 AI Coding IDE
- **[Windsurf](https://windsurf.com/)** - 完整的 AI 编码 IDE，内置 Cascade AI 助手，支持 MCP、插件和自动化工作流
- **[CodeBuddy](https://codebuddy.ai/)** - AI 编程助手

### 终端工具

- **[Claude Code](https://claude.ai/)** - Anthropic 的 AI 编程助手
- **[OpenCode](https://github.com/opencode-ai/opencode)** - 强大的终端 AI 编码代理，支持多模型、MCP 和 LSP 协议
- **[Codex](https://openai.com/api/)** - OpenAI 的代码生成终端工具
- **[Gemini CLI](https://github.com/google/gemini-cli)** - Google Gemini 命令行工具

---

## AI 智能体框架

### 多智能体系统

- **[MetaGPT](https://github.com/FoundationAgents/MetaGPT)** - 多智能体框架，模拟软件公司工作流程，实现从需求到代码的自动化生成
- **[CrewAI](https://github.com/joaomdmoura/crewAI)** - 多智能体协作框架
- **[AutoGen](https://github.com/microsoft/autogen)** - 微软的多智能体对话框架
- **[JoyAgent-JDGeni](https://github.com/jd-opensource/joyagent-jdgenie)** - 京东开源的端到端产品级通用智能体系统
- **[500 AI Agents](https://github.com/ashishpatel26/500-AI-Agents-Projects)** - 精心策划的 AI 智能体用例集合，涵盖各个行业
- **[Agent Design Patterns](https://github.com/fzy2012/rhzl-Agentic-Design-Patterns-cn)** - AI 智能体设计模式中文翻译项目

### 智能体开发框架

- **[LangGraph](https://github.com/langchain-ai/langgraph)** - LangChain 的智能体编排框架
- **[AgentEvolv](https://github.com/modelscope/AgentEvolver)** - 支持自主学习和优化的智能体框架

### 编码智能体工具

- **[Claude Code Plugins](https://github.com/wshobson/agents)** - 完整的生产级系统，结合 108 个专业 AI 智能体、15 个多智能体工作流编排器、129 个 Agent Skills 和 72 个开发工具，组织成 72 个聚焦插件，专为 Claude Code 设计（27.2k+ stars）
- **[Serena](https://github.com/oraios/serena)** - 强大的开源编码智能体工具包，提供语义检索和代码编辑能力，本地运行无需 API Key
- **[OpenAutoGLM](https://github.com/zai-org/open-autoglm)** - 自动操作用户 Android 手机完成任务的智能体系统，支持自然语言控制
- **[SuperClaude Framework](https://github.com/superclaude-org/superclaude_framework)** - 完整的软件开发工作流框架，专为 AI 编码代理设计，提供 30 个命令和 16 个专业化智能体

---

## RAG 与知识库

### RAG 框架

- **[RAGFlow](https://github.com/infiniflow/ragflow)** - 基于 RAG 的知识库问答系统

### 向量数据库

- **[Supabase](https://github.com/supabase/supabase)** - 开源 Firebase 替代品，支持向量数据库和 AI 嵌入
- **[Pinecone](https://www.pinecone.io/)** - 托管向量数据库服务
- **[Weaviate](https://github.com/weaviate/weaviate)** - 开源向量搜索引擎

### 知识图谱

- **[Graphiti](https://github.com/getzep/graphiti)** - 实时知识图谱构建框架，支持 Neo4j、FalkorDB、Kuzu 等图数据库，可用于代码理解系统

---

## LLM 开发框架

### 框架与工具

- **[LangChain](https://github.com/langchain-ai/langchain)** - 构建 LLM 应用的框架
- **[LlamaIndex](https://github.com/run-llama/llama_index)** - LLM 数据框架
- **[Haystack](https://github.com/deepset-ai/haystack)** - 端到端 NLP 框架

### LLM 应用示例

- **[Awesome LLM Apps](https://github.com/Shubhamsaboo/awesome-llm-apps)** - 精心策划的 LLM 应用集合，涵盖 RAG、智能体、MCP、语音 AI 等，支持多种模型提供商（OpenAI、Anthropic、Google、XAI 以及开源模型）
- **[Agents Towards Production](https://github.com/NirDiamant/agents-towards-production)** - 面向生产级 AI Agent 开发的实用型教程与代码仓库，涵盖架构设计、开发、集成到部署、监控、评估等全流程

---

## 代码审查与测试

### 代码审查工具

- **[CodeRabbit](https://coderabbit.ai/)** - AI 代码审查助手

---

## 提示词工程

### 提示词工具

- **[System Prompt Leak](https://github.com/asgeirtj/system_prompts_leaks)** - 收集主流大模型（ChatGPT、Claude、Gemini 等）的系统提示词
- **[System Prompts and Models of AI Tools](https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools)** - 包含超过 30,000+ 行的 AI 工具结构和功能洞察，涵盖代码开发类 AI 工具的系统提示词

### 提示词资源

- **[Awesome Prompts](https://github.com/f/awesome-chatgpt-prompts)** - ChatGPT 提示词集合
- **[Prompt Engineering Guide](https://www.promptingguide.ai/)** - 提示词工程指南

---

## MCP 协议与工具

### MCP 框架

- **[Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers)** - MCP 服务器集合，涵盖文件访问、数据库连接、API 集成等

### MCP 工具

- **[SandBase CLI](https://github.com/sandbaseai/cli)** - 开源 CLI 与 MCP 桥接工具，通过一个账户将 Claude Code、Codex、Cursor 等 AI 编程智能体连接到 2,000+ 个 AI 模型和 API

### MCP 服务器平台

- **[MCP.so](https://mcp.so/zh)** - MCP 服务器平台和目录
- **[MCP.ad](https://mcp.ad/servers)** - MCP 服务器发现平台，收录 33,000+ 个 MCP 服务器
- **[Cursor Directory](https://cursor.directory/)** - Cursor 社区平台，探索和生成规则、浏览 MCP 服务器
- **[Pulse MCP](https://www.pulsemcp.com/)** - MCP 服务器平台
- **[Glama MCP](https://glama.ai/mcp)** - MCP 服务器目录和资源

---

## 代码分析与理解

### 代码理解工具

- **[DeepWiki](https://deepwiki.com/)** - GitHub 仓库深度理解和组织工具，将 `github.com` 改为 `deepwiki.com` 即可使用，支持 Devin 驱动对话问答

### 代码分析

- **[CodeQL](https://github.com/github/codeql)** - GitHub 的代码分析工具

---

## Agent Skills

### 官方 Skills 集合

- **[Superpowers](https://github.com/obra/superpowers)** - 完整的软件开发工作流框架，基于可组合的"技能"系统，专为编码代理设计（33.9k+ stars）
- **[Anthropic Skills](https://github.com/anthropics/skills)** - Anthropic 官方的 Agent Skills 公共仓库，包含示例、指南和最佳实践（50.2k+ stars）
- **[OpenAI Skills](https://github.com/openai/skills)** - OpenAI 官方的 Agent Skills 集合
- **[Trail of Bits Skills](https://github.com/trailofbits/skills)** - Trail of Bits 的安全相关 Agent Skills

### 社区 Skills 集合

- **[Antigravity Awesome Skills](https://github.com/sickn33/antigravity-awesome-skills)** - 专为 Claude Code/Antigravity/Cursor 设计的终极 Agent Skills 集合，包含 552+ 个经过实战验证的高性能技能，涵盖架构、开发、安全、基础设施、数据/AI、业务等领域（5.1k+ stars）
- **[Vercel Labs Agent Skills](https://github.com/vercel-labs/agent-skills)** - Vercel Labs 的 Agent Skills 集合
- **[AWS Agent Skills](https://github.com/itsmostafa/aws-agent-skills)** - AWS 相关的 Agent Skills
- **[Obsidian Skills](https://github.com/kepano/obsidian-skills)** - Obsidian 笔记软件的 Agent Skills
- **[Agent Skills for Context Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering)** - 上下文工程相关的 Agent Skills

### 资源集合与平台

- **[Claude Code Plugins](https://github.com/wshobson/agents)** - 完整的 Claude Code 插件系统，包含 72 个聚焦插件、108 个专业智能体、129 个 Agent Skills 和 15 个工作流编排器，用于智能自动化和多智能体编排（27.2k+ stars）
- **[SkillsMP](https://skillsmp.com/)** - AI Agent Skill 市场平台，收录 58,925+ 个开源技能，支持 AI 语义搜索，发现和探索社区构建的 AI 技能
- **[skill0](https://skill0.io/)** - 技能市场和目录平台，用于发现和探索社区构建的 AI 技能，收录 423+ 个精选技能
- **[Awesome Claude Skills](https://github.com/ComposioHQ/awesome-claude-skills)** - 精心整理的 Claude Skills 资源列表，包含资源和工具，用于自定义 Claude AI 工作流，涵盖开发、通信、创意、生产力、协作和安全等领域（27.7k+ stars）
- **[Awesome Claude Skills (TravisVN)](https://github.com/travisvn/awesome-claude-skills)** - 精心整理的 awesome Claude Skills 资源列表，包含资源和工具，用于自定义 Claude AI 工作流，特别专注于 Claude Code（6.2k+ stars）
- **[Awesome Claude Skills (BehiSecc)](https://github.com/BehiSecc/awesome-claude-skills)** - 精心整理的 Claude Skills 资源列表，涵盖文档技能、开发工具、数据分析、科学研究、写作、媒体、健康、协作和安全等领域（4.7k+ stars）
- **[NotebookLM Skill](https://github.com/PleasePrompto/notebooklm-skill)** - Claude Code Skill，让 Claude Code 直接与 Google NotebookLM 笔记本通信，查询上传文档并获得基于来源的、带引用的答案
- **[Awesome Agent Skills](https://github.com/heilcheng/awesome-agent-skills)** - 精心整理的 Agent Skills 资源集合
- **[Awesome Agent Skills (Libukai)](https://github.com/libukai/awesome-agent-skills)** - 另一个优秀的 Agent Skills 资源集合
- **[OpenSkills](https://github.com/numman-ali/openskills)** - 开源 Agent Skills 集合
- **[Skills](https://skills.sh)** - 开放Agent Skills生态系统

---

## 开发工具与加速

### 文档工具

- **[MarkItDown](https://github.com/microsoft/markitdown)** - Microsoft 开源的文档转换工具，支持多种格式转 Markdown，特别适合与 LLM 配合使用

### 项目管理

- **[Spec-Kit](https://github.com/github/spec-kit)** - GitHub 的规范驱动开发工具包（33.9k+ stars），支持 AI 辅助开发，与 GitHub Copilot 深度集成
- **[OpenSpec](https://github.com/Fission-AI/OpenSpec)** - 开源规范工具，支持 API 规范和代码生成
- **[BMAD-METHOD](https://github.com/bmad-code-org/bmad-method)** - AI 代理框架，支持代码库扁平化（`npx bmad-method flatten`）和多智能体协作，帮助团队快速将自然语言需求落地为代码

---

## 学习资源

### 教程与指南

- **[AI Code Guide](https://github.com/automata/aicodeguide)** - AI 编码路线图和学习指南，涵盖 AI 辅助编程工具、实践和最佳实践
- **[Agent Design Patterns CN](https://github.com/fzy2012/rhzl-Agentic-Design-Patterns-cn)** - AI 智能体设计模式中文翻译，包含完整代码示例，支持本地运行和 Google Colab 在线运行

### 社区资源

- **[Awesome AI](https://github.com/cssmagic/awesome-ai)** - AI 大型语言模型、AI 辅助编程等领域的常用资料
- **[Awesome LLM Resources](https://github.com/WangRongsheng/awesome-LLM-resources)** - LLM 资源集合

---

## ClaudeCode Rules

本节包含 ClaudeCode 的开发规则和指南，涵盖各种编程语言、框架和最佳实践。

### 语言相关规则

- **[Python](rules/python.md)** - Python 开发规则，包括类型提示、异步模式和最佳实践
- **[Go](rules/go.md)** - Go 开发指南和约定
- **[Rust](rules/rust.md)** - Rust 开发规则和安全实践
- **[Node.js](rules/nodejs.md)** - Node.js 开发规则和模式
- **[iOS Swift](rules/ios-swift.md)** - iOS Swift 开发指南
- **[Android App](rules/android-app.md)** - Android 应用开发规则
- **[Android System](rules/android-system.md)** - Android 系统级开发指南

### 框架与平台规则

- **[React Frontend](rules/react-frontend.md)** - React 前端开发规则和模式
- **[Vue Frontend](rules/vue-frontend.md)** - Vue.js 前端开发指南
- **[Backend](rules/backend.md)** - 后端开发规则和最佳实践
- **[Docker](rules/docker.md)** - Docker 容器化规则和实践

### 开发实践

- **[Agents](rules/agents.md)** - 智能体编排和开发规则
- **[Coding Style](rules/coding-style.md)** - 通用编码风格指南
- **[Patterns](rules/patterns.md)** - 设计模式和架构指南
- **[Performance](rules/performance.md)** - 性能优化规则
- **[Security](rules/security.md)** - 安全最佳实践和指南
- **[Testing](rules/testing.md)** - 测试策略和实践
- **[Git Workflow](rules/git-workflow.md)** - Git 工作流和提交约定
- **[Hooks](rules/hooks.md)** - Git 钩子和自动化规则

---

## 🤝 贡献

欢迎提交 Issue 或 Pull Request 来补充更多优质资源！

### 贡献指南

1. Fork 本仓库
2. 创建你的特性分支 (`git checkout -b feature/AmazingResource`)
3. 提交你的更改 (`git commit -m 'Add some AmazingResource'`)
4. 推送到分支 (`git push origin feature/AmazingResource`)
5. 开启一个 Pull Request

---

## 📄 许可证

本项目采用 [MIT License](LICENSE) 许可证。

---

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=chendongqi/awesome-ai-coding&type=Date)](https://star-history.com/#chendongqi/awesome-ai-coding&Date)

如果这个项目对你有帮助，请给它一个 Star ⭐

---

## 📧 联系方式

如有问题或建议，请通过 Issue 联系我们。

---

**最后更新**: 2026年1月29日
