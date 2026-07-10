# AI 开源趋势日报 2026-07-10

> 数据来源: GitHub Trending + GitHub Search API | 生成时间: 2026-07-10 03:56 UTC

---

# AI 开源趋势日报
**日期：2026-07-10**

## 今日速览
今日 AI 开源社区呈现出 **“Agent 深度嵌入办公与求职流程”** 的爆发趋势，多个以智能体驱动的求职框架、Office 自动化工具以及代理配套技能集收获超高新增关注。同时，**系统提示大规模泄漏** 项目登上热榜，引发安全与对齐伦理讨论；轻量级 CPU TTS 模型 `pocket-tts` 的出现则呼应了端侧 AI 持续走热的背景。RAG/记忆领域依旧活跃，知识图谱式记忆和图结构查询方案成为新焦点。

---

## 各维度热门项目

### 🔧 AI 基础工具
| 项目 | Stars | 一句话说明 |
|------|-------|------------|
| [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) | 总⭐ — (今日 +2,554) | 面向 AI 编码代理的生产级工程技能集，直击代理代码质量痛点，今日大热。 |
| [iOfficeAI/OfficeCLI](https://github.com/iOfficeAI/OfficeCLI) | 总⭐ — (今日 +1,929) | 专为 AI 代理构建的 Office 命令行套件，无需安装 Office 即可自动化 Word/Excel/PPT，今日新增极快。 |
| [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) | 总⭐ — (今日 +1,391) | 知名设计系统 `.design.md` 文件合集，可让编码代理一键生成匹配 UI，是“设计即提示”的新基建。 |
| [ollama/ollama](https://github.com/ollama/ollama) | ⭐175,837 | 本地运行 Kimi-K2.6、GLM-5.1、DeepSeek 等模型的首选工具，仍然是开发者自托管推理的最热入口。 |
| [firecrawl/firecrawl](https://github.com/firecrawl/firecrawl) | ⭐148,493 | 大规模 Web 抓取与交互 API，已成为 LLM 连接互联网的标准数据管道。 |
| [huggingface/transformers](https://github.com/huggingface/transformers) | ⭐162,423 | 跨文本、视觉、音频的模型定义框架，AI 基础设施的基石级项目。 |
| [vllm-project/vllm](https://github.com/vllm-project/vllm) | ⭐85,856 | 高吞吐、低显存的推理引擎，支撑几乎所有主流模型的线上服务。 |
| [unclecode/crawl4ai](https://github.com/unclecode/crawl4ai) | 总⭐ — (今日 +215) | 开源 LLM 友好的 Web 爬虫与抓取器，持续为下游 Agent/RAG 供给干净数据。 |

### 🤖 AI 智能体/工作流
| 项目 | Stars | 一句话说明 |
|------|-------|------------|
| [MadsLorentzen/ai-job-search](https://github.com/MadsLorentzen/ai-job-search) | 总⭐ — (今日 +3,716) | 基于 Claude Code 的求职 Agent 框架——自动评估岗位、定制简历、撰写求职信并模拟面试，今日最火。 |
| [vxcontrol/pentagi](https://github.com/vxcontrol/pentagi) | 总⭐ — (今日 +535) | 全自主渗透测试 Agent 系统，将 AI 智能体带入攻防安全一线。 |
| [Significant-Gravitas/AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) | ⭐185,440 | 经典通用自主 Agent 平台，持续承载社区的“自治 Agent”愿景。 |
| [langgenius/dify](https://github.com/langgenius/dify) | ⭐148,352 | 生产就绪的 Agent 工作流开发平台，可视化编排与部署的热门选择。 |
| [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | ⭐141,421 | 已成为 Agent 工程的事实标准，统一了 LLM、工具和记忆的构建范式。 |
| [browser-use/browser-use](https://github.com/browser-use/browser-use) | ⭐104,003 | 让网站对 AI Agent 可访问，将浏览器变成自动化的操作界面。 |
| [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) | ⭐212,297 | “与你一起成长的 Agent”，社区关注度极高，有望重新定义个人 AI 伙伴。 |
| [affaan-m/ECC](https://github.com/affaan-m/ECC) | ⭐227,929 | Agent 性能优化系统，集技能、记忆、安全于一体，星数惊人。 |

### 📦 AI 应用
| 项目 | Stars | 一句话说明 |
|------|-------|------------|
| [bradautomates/claude-video](https://github.com/bradautomates/claude-video) | 总⭐ — (今日 +718) | 赋予 Claude 观看任意视频的能力：自动下载、抽帧、转录并提交分析，创意应用新形态。 |
| [open-webui/open-webui](https://github.com/open-webui/open-webui) | ⭐144,897 | 用户友好的自托管 AI 交互界面，支持 Ollama 和 OpenAI API，是个人 AI Hub。 |
| [CherryHQ/cherry-studio](https://github.com/CherryHQ/cherry-studio) | ⭐48,380 | AI 生产力工作室，内置智能聊天、自主代理与 300+ 助手。 |
| [hugohe3/ppt-master](https://github.com/hugohe3/ppt-master) | ⭐38,064 | AI 一键生成可编辑 PowerPoint，保留原生形状、图表与音频旁白，实用性强。 |
| [netdata/netdata](https://github.com/netdata/netdata) | ⭐79,577 | AI 驱动的全栈可观测性监控，为精益团队提供即时洞察。 |
| [deepfakes/faceswap](https://github.com/deepfakes/faceswap) | ⭐55,334 | 深度伪造换脸软件，长期活跃的 AI 图像应用标杆。 |

### 🧠 大模型/训练
| 项目 | Stars | 一句话说明 |
|------|-------|------------|
| [kyutai-labs/pocket-tts](https://github.com/kyutai-labs/pocket-tts) | 总⭐ — (今日 +235) | 可在 CPU 上运行的超轻量 TTS 模型，将高质量语音生成带入边缘设备。 |
| [asgeirtj/system_prompts_leaks](https://github.com/asgeirtj/system_prompts_leaks) | ⭐55,259 (今日 +1,125) | 提取并公开 Claude、GPT-5.5、Gemini、Grok 等模型的系统提示，暴露内部行为与约束。 |
| [tensorflow/tensorflow](https://github.com/tensorflow/tensorflow) | ⭐196,167 | 经典端到端机器学习框架，生态深厚。 |
| [pytorch/pytorch](https://github.com/pytorch/pytorch) | ⭐101,645 | 动态计算图与强 GPU 加速，大模型训练主流基座。 |
| [ultralytics/ultralytics](https://github.com/ultralytics/ultralytics) | ⭐59,300 | YOLO 系列目标检测、分割、位姿估计，视觉模型持续更新。 |
| [open-compass/opencompass](https://github.com/open-compass/opencompass) | ⭐7,182 | 覆盖 100+ 数据集的 LLM 评测平台，为模型能力提供标尺。 |

### 🔍 RAG/知识库
| 项目 | Stars | 一句话说明 |
|------|-------|------------|
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | ⭐86,640 | 跨会话持久化代理记忆：捕获、压缩并注入历史上下文，兼容 Claude Code、Codex、Gemini 等。 |
| [infiniflow/ragflow](https://github.com/infiniflow/ragflow) | ⭐84,718 | 融合 RAG 与 Agent 能力的领先开源引擎，构建强大的上下文层。 |
| [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) | ⭐81,375 | 将代码、SQL、文档、图片等一键转为可查询的知识图谱，提升 RAG 的结构化理解。 |
| [mem0ai/mem0](https://github.com/mem0ai/mem0) | ⭐60,505 | AI Agent 通用记忆层，为智能体提供持久、可共享的记忆。 |
| [run-llama/llama_index](https://github.com/run-llama/llama_index) | ⭐50,752 | 领先的文档 Agent 与 OCR 平台，RAG 的关键中间件。 |
| [milvus-io/milvus](https://github.com/milvus-io/milvus) | ⭐45,158 | 高性能云原生向量数据库，支撑大规模向量相似搜索。 |
| [VectifyAI/PageIndex](https://github.com/VectifyAI/PageIndex) | ⭐33,908 | 无向量、推理驱动的文档索引方案，代表“后向量”RAG 的新思路。 |
| [topoteretes/cognee](https://github.com/topoteretes/cognee) | ⭐27,437 | 基于知识图谱的开源 Agent 记忆平台，提供持久化长期记忆。 |

---

## 趋势信号分析
今日数据最显著的信号是 **AI Agent 与具体工作流（求职、办公）的深度融合**。`ai-job-search` 以单日 +3,716 星成为断层第一，`OfficeCLI` 紧随其后，二者均以编程代理为“执行引擎”，将传统手动流程转化为全自动管线，意味着社区对 **“Agent-First”应用** 的需求已从实验走向实用。与之配套的 `agent-skills` 和 `awesome-design-md` 则构建了 **“技能包 + 设计规范”混合资源层**，让代理能在编码时直接调用经过验证的 prompt 或设计令牌，显著降低生成式 AI 的返工率。与此同时，`system_prompts_leaks` 以 1,125 日增星登上热榜，反映出大模型黑盒透明度与安全争议的持续升温——当系统提示可被抽取并公开时，模型对齐、越狱防御和知识产权问题变得空前具象。在基础模型侧，`pocket-tts` 的轻量 TTS 模型强化了 **端侧、可私有化运行的语音能力**，这与近期企业级自托管和离线场景的需求爆发高度吻合。RAG 领域则出现“从向量到图谱”的迁移倾向：`graphify`、`cognee` 等项目用知识图谱替代或增强传统向量检索，试图让记忆更具语义关联和可解释性，这可能是下一波 Agent 长期记忆的底层范式。

---

## 社区关注热点
- **[MadsLorentzen/ai-job-search](https://github.com/MadsLorentzen/ai-job-search)** —— 将 AI 编码代理直接对准求职全流程，展示了 Agent 在个人效能工具上的高天花板，值得开发者复用其“框架+技能”模式。
- **[addyosmani/agent-skills](https://github.com/addyosmani/agent-skills)** —— 由 Google 工程师出品的代理技能集，若被广泛采纳，可能成为编码代理的“行业练习生手册”，直接影响代码生成质量。
- **[asgeirtj/system_prompts_leaks](https://github.com/asgeirtj/system_prompts_leaks)** —— 大量主流模型的系统提示被公开，无论用于缺陷挖掘、安全测试还是模型行为理解，都是极具冲击力的参考材料，也敲响厂商安全警钟。
- **[kyutai-labs/pocket-tts](https://github.com/kyutai-labs/pocket-tts)** —— 极低资源的 TTS 模型，推动语音合成走向任意 CPU 设备，为离线助手、嵌入式语音应用降低成本门槛。
- **[Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify)** —— “任何数据都变为知识图谱”的思路极简却强大，可能成为 RAG 管线中结构化上下文的标准组件，尤其适合需要复杂多跳推理的智能体。

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*