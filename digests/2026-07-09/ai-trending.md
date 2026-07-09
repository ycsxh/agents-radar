# AI 开源趋势日报 2026-07-09

> 数据来源: GitHub Trending + GitHub Search API | 生成时间: 2026-07-09 03:55 UTC

---

# AI 开源趋势日报

**数据来源**：GitHub Trending（今日新增 stars） + AI 主题搜索（7 天内活跃）  
**筛选范围**：已剔除与 AI/ML 无关的通用工具（如 Prisma、Argo CD、PhotoGIMP、autoremesher 等），保留所有与人工智能明确相关的项目。

---

## 1. 今日速览

今日 AI 开源社区焦点明显集中在 **Agent 技能化与记忆持久化** 两大方向。一批可复用的 Agent 技能框架（`superpowers`、`agent-skills`）今日爆发式增长，表明开发者正从“造 Agent”转向“为 Agent 赋能”。与此同时，腾讯云发布的 `TencentDB-Agent-Memory` 将本机长期记忆带入智能体工作流，配合轻量级向量数据库 `zvec`，为去中心化、低延迟的 RAG 与 Agent 记忆方案提供了新选择。此外，`OfficeCLI` 让 Agent 能直接操作办公套件，`RuView` 则利用 WiFi 信号实现无视频的空间智能，展现 AI 与物理世界交互的新可能。

---

## 2. 各维度热门项目

### 🔧 AI 基础工具
*面向框架、SDK、推理引擎、沙箱与开发工具*

| 项目 | Stars | 一句话亮点 |
|------|-------|------------|
| [TencentCloud/CubeSandbox](https://github.com/TencentCloud/CubeSandbox) | ⭐0 (+564 today) | 为 AI Agent 设计的即时、并发、安全的轻量沙箱，保障代码执行隔离。 |
| [ollama/ollama](https://github.com/ollama/ollama) | ⭐175,764 | 一键运行 Kimi-K2.6、DeepSeek 等前沿模型，本地 LLM 推理的事实标准工具。 |
| [vllm-project/vllm](https://github.com/vllm-project/vllm) | ⭐85,747 | 高吞吐、低显存的 LLM 推理引擎，广泛应用于生产环境。 |
| [firecrawl/firecrawl](https://github.com/firecrawl/firecrawl) | ⭐147,928 | 面向 LLM 的规模化网页抓取与搜索 API，将互联网变成 Agent 的知识源。 |
| [wonderwhy-er/DesktopCommanderMCP](https://github.com/wonderwhy-er/DesktopCommanderMCP) | ⭐0 (+28 today) | 赋予 Claude 终端控制、文件搜索与差异编辑能力的 MCP 服务器。 |

### 🤖 AI 智能体/工作流
*覆盖 Agent 框架、自动化与多智能体协调*

| 项目 | Stars | 一句话亮点 |
|------|-------|------------|
| [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) | ⭐0 (+1,297 today) | 为 AI 编码 Agent 提供的生产级工程技能集合，今日快速登上热榜。 |
| [obra/superpowers](https://github.com/obra/superpowers) | ⭐0 (+1,116 today) | Agent 技能框架与开发方法论，可让 AI 代理获得可组合的“超能力”。 |
| [mvanhorn/last30days-skill](https://github.com/mvanhorn/last30days-skill) | ⭐0 (+352 today) | 跨 Reddit、X、YouTube、HN 等多源研究并合成总结的 Agent 技能。 |
| [langgenius/dify](https://github.com/langgenius/dify) | ⭐148,241 | 生产就绪的 Agent 工作流开发平台，低代码构建复杂数智能链路。 |
| [browser-use/browser-use](https://github.com/browser-use/browser-use) | ⭐103,761 | 让 AI Agent 像人类一样操作浏览器，自动化完成表单填写、数据抓取等任务。 |
| [Significant-Gravitas/AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) | ⭐185,435 | 经典自主智能体框架，仍在持续进化并引领 Agent 工程范式。 |
| [TauricResearch/TradingAgents](https://github.com/TauricResearch/TradingAgents) | ⭐91,911 | 多智能体驱动的金融交易框架，结合市场数据与 LLM 推理进行决策。 |

### 📦 AI 应用
*具体产品与垂直场景解决方案*

| 项目 | Stars | 一句话亮点 |
|------|-------|------------|
| [MadsLorentzen/ai-job-search](https://github.com/MadsLorentzen/ai-job-search) | ⭐0 (+5,079 today) | 基于 Claude Code 的 AI 求职框架，自动评估岗位、定制简历与面试准备，今日最火新项目。 |
| [iOfficeAI/OfficeCLI](https://github.com/iOfficeAI/OfficeCLI) | ⭐0 (+1,717 today) | 无安装式的 AI Agent 专供 Office 套件，让 Agent 直接读写 Word/Excel/PPT。 |
| [bradautomates/claude-video](https://github.com/bradautomates/claude-video) | ⭐0 (+951 today) | 赋予 Claude 观看并理解视频内容的能力，自动抽取帧与字幕。 |
| [ruvnet/RuView](https://github.com/ruvnet/RuView) | ⭐0 (+799 today) | 将普通 WiFi 信号转化为空间智能与生命体征监测，无需摄像头的感知方案。 |
| [hugohe3/ppt-master](https://github.com/hugohe3/ppt-master) | ⭐37,817 | AI 从文档生成真正可编辑的 PPT，含动画、图表及语音讲解，商用级高度。 |

### 🧠 大模型/训练
*模型权重、训练框架、微调与评估工具*

| 项目 | Stars | 一句话亮点 |
|------|-------|------------|
| [asgeirtj/system_prompts_leaks](https://github.com/asgeirtj/system_prompts_leaks) | ⭐0 (+1,218 today) | 泄露的 Claude、GPT-5.5、Gemini 3.5 Pro 等系统提示词合集，今日引爆社区。 |
| [ultralytics/ultralytics](https://github.com/ultralytics/ultralytics) | ⭐59,265 | YOLO 目标检测、分割与姿态估计的深度学习训练框架，工业标准之一。 |
| [tensorflow/tensorflow](https://github.com/tensorflow/tensorflow) | ⭐196,121 | 经典端到端机器学习框架，仍是大量实验部署的基石。 |
| [pytorch/pytorch](https://github.com/pytorch/pytorch) | ⭐101,606 | 动态神经网络核心框架，研究社区的优先选择。 |
| [huggingface/transformers](https://github.com/huggingface/transformers) | ⭐162,396 | 大模型定义与训练工具集，几乎涵盖所有主流文本、视觉、多模态模型。 |

### 🔍 RAG/知识库
*向量数据库、检索增强与知识管理*

| 项目 | Stars | 一句话亮点 |
|------|-------|------------|
| [TencentCloud/TencentDB-Agent-Memory](https://github.com/TencentCloud/TencentDB-Agent-Memory) | ⭐0 (+318 today) | 4 级渐进式管道的全本机 Agent 长期记忆，零外部 API 依赖，今日首发即热。 |
| [alibaba/zvec](https://github.com/alibaba/zvec) | ⭐14,474 (+395 today) | 超轻量、闪电般快速的进程内向量数据库，适合客户端与边缘 AI 场景。 |
| [mem0ai/mem0](https://github.com/mem0ai/mem0) | ⭐60,433 | 通用 AI 记忆层，为任何 Agent 提供持久上下文记忆。 |
| [infiniflow/ragflow](https://github.com/infiniflow/ragflow) | ⭐84,638 | 领先的开源 RAG 引擎，深度融合 RAG 与 Agent 能力，构建上下文增强层。 |
| [milvus-io/milvus](https://github.com/milvus-io/milvus) | ⭐45,142 | 云原生、高性能向量数据库，是许多 RAG 系统的核心存储。 |
| [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) | ⭐80,569 | 将代码、模式、文档转为可查询的知识图谱，赋能 Agent 跨文件推理。 |

---

## 3. 趋势信号分析

今日榜单传递出 **Agent 技能生态化** 的强烈信号。`superpowers` 和 `agent-skills` 等项目在单日内斩获上千 stars，说明社区不再满足于空泛的 Agent 框架，转而追求可复用、可组合的工程化能力模块。与此同时，`TencentDB-Agent-Memory` 与 `zvec` 的同日爆发，折射出**轻量化、本机化的记忆与检索**正成为 Agent 的关键需求——这与数据隐私、延迟敏感的 Edge/Client 场景高度契合。  

另一条线索是 **AI 与物理/办公世界的桥梁**：`OfficeCLI` 让 Agent 直接操控 Office 文档，`RuView` 用 WiFi 做空间感知，`claude-video` 赋予多模态视频理解。这些项目将 AI 的能力从纯文本推向更丰富的环境交互。  

值得警惕的是，`system_prompts_leaks` 以泄露形式展示了各大前沿模型的系统指令，暴露出模型安全与提示工程防御的薄弱点，这与近期 Anthropic、OpenAI 新版本发布后安全讨论升温密切相关。  

---

## 4. 社区关注热点

- **🆕 `MadsLorentzen/ai-job-search`**：求职场景的端到端 Agent 自动化，今日 5k+ 新增 stars，验证了“生活痛点+AI 自动化”的落地呼声。
- **🧠 `TencentDB-Agent-Memory` + `mem0`**：Agent 长期记忆方案密集出现，开发者需关注记忆管道的设计取舍（全本地 vs 云，图 vs 向量）。
- **⚔️ `obra/superpowers` 技能框架**：定义 Agent 能力组合的新方法论，有可能成为类似 DevOps 工具链的“Agent 能力即代码”范式。
- **👁️ `ruvnet/RuView`**：无摄像头的空间智能，技术栈涵盖信号处理与机器学习，为智慧家居与安防提供隐私友好方案。
- **🔍 `alibaba/zvec`**：进程内向量检索库，性能优异，是构建离线、移动端 RAG 应用的候选基石，可与现有向量数据库形成互补。

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*