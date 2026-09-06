# AI Open Source Trends 2026-09-06

> Sources: GitHub Trending + GitHub Search API | Generated: 2026-09-06 04:06 UTC

---

# AI Open Source Trends Report — 2026-09-06

*Note: Trending-list projects show today-only star deltas when total-stars data was not provided in the source; Topic Search projects show total stars from the GitHub topic snapshot.*

## 1. Today's Highlights

Today’s GitHub trending list is dominated by the “Agent Skills” layer rather than new model releases. Skill repositories such as [mattpocock/skills](https://github.com/mattpocock/skills), [anthropics/skills](https://github.com/anthropics/skills), [humanlayer/skills](https://github.com/humanlayer/skills), and [blader/humanizer](https://github.com/blader/humanizer) are surging, suggesting that the open-source community is now standardizing how agent capabilities are packaged, shared, and reused across Claude Code, Codex, OpenCode, and Cursor. At the same time, local inference is moving closer to agent runtimes: [magnitudedev/magnitude](https://github.com/magnitudedev/magnitude) is an inference server explicitly designed to plug local models into existing coding agents. The overall pattern is clear: model access is becoming commodity infrastructure, while agent harnesses, skills, and persistent memory are becoming the real differentiation layer.

## 2. Top Projects by Category

### 🔧 AI Infrastructure

- [magnitudedev/magnitude](https://github.com/magnitudedev/magnitude) — today +674. Open-source inference server that matches local models to available hardware and plugs into the agent you already use.
- [ollama/ollama](https://github.com/ollama/ollama) — ⭐180,257. The leading local model runner; its description already highlights support for Kimi-K2.6, GLM-5.2, MiniMax, DeepSeek, GPT-OSS, and Qwen.
- [huggingface/transformers](https://github.com/huggingface/transformers) — ⭐164,845. The foundational open-source model framework for text, vision, audio, and multimodal models.
- [langchain-ai/langchain](https://github.com/langchain-ai/langchain) — ⭐145,730. The agent engineering platform that keeps expanding into workflow, memory, and tool-calling abstractions.
- [headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom) — ⭐69,051. Token-and-context compression layer for coding agents and RAG pipelines, increasingly important as context windows fill up.
- [CopilotKit/CopilotKit](https://github.com/CopilotKit/CopilotKit) — ⭐37,214. Frontend stack and Generative UI framework for building agent interfaces across React, Angular, and mobile.
- [open-compass/opencompass](https://github.com/open-compass/opencompass) — ⭐7,393. Broad LLM evaluation platform supporting 100+ datasets and many major models.
- [apache/casbin-gateway](https://github.com/apache/casbin-gateway) — ⭐598. Security gateway for AI and MCP HTTP traffic; an early sign of agent-governance infrastructure.

### 🤖 AI Agents / Workflows

- [mattpocock/skills](https://github.com/mattpocock/skills) — today +2,692. A practical `.agents` skill directory for real engineering workflows and one of the fastest-growing repos of the day.
- [anthropics/skills](https://github.com/anthropics/skills) — today +475. Anthropic’s official Agent Skills repository; important because it signals a public, cross-agent packaging target.
- [affaan-m/ECC](https://github.com/affaan-m/ECC) — ⭐250,061; today +1,314. An agent-harness performance optimization system targeting Claude Code, Codex, OpenCode, Cursor, and beyond.
- [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) — ⭐242,073; today +575. “The agent that grows with you”; one of the clearest signals that persistent, self-evolving agents are now mainstream.
- [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) — ⭐128,113; today +2,845. Makes coding agents “think like the laziest senior dev” and avoid writing unnecessary code.
- [anomalyco/opencode](https://github.com/anomalyco/opencode) — today +725. An open-source coding agent that is quickly becoming a cross-tool integration target.
- [browser-use/browser-use](https://github.com/browser-use/browser-use) — ⭐112,411. The standard open-source bridge for giving AI agents real browser-based web automation.
- [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph) — ⭐41,107. Production-oriented orchestration for resilient, stateful agent workflows.

### 📦 AI Applications

- [f/prompts.chat](https://github.com/f/prompts.chat) — ⭐169,429. The famous “Awesome ChatGPT Prompts” evolution; self-hostable prompt sharing and discovery.
- [open-webui/open-webui](https://github.com/open-webui/open-webui) — ⭐151,072. Self-hosted, user-friendly AI interface for local and API-based LLMs.
- [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) — ⭐120,888. Generates short videos from a topic using LLMs and automated AI workflows.
- [career-ops-hq/career-ops](https://github.com/career-ops-hq/career-ops) — ⭐70,254. Open-source AI job-search agent that scans listings, scores roles, and tailors CVs inside coding CLIs.
- [ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis) — ⭐64,670. LLM-driven multi-market stock analysis with dashboards, news monitoring, and scheduled alerts.
- [hugohe3/ppt-master](https://github.com/hugohe3/ppt-master) — ⭐52,241. AI converts documents or topics into native PowerPoint decks with real shapes, charts, and transitions.
- [CherryHQ/cherry-studio](https://github.com/CherryHQ/cherry-studio) — ⭐51,489. Multi-model AI productivity studio with autonomous agents and 300+ assistants.
- [agentscope-ai/QwenPaw](https://github.com/agentscope-ai/QwenPaw) — ⭐34,940. Easy-to-install personal AI assistant with cloud or local deployment and multi-channel chat support.

### 🧠 LLMs / Training

- [rasbt/LLMs-from-scratch](https://github.com/rasbt/LLMs-from-scratch) — ⭐104,400. The most popular educational path for building a ChatGPT-like LLM in PyTorch from scratch.
- [jingyaogong/minimind](https://github.com/jingyaogong/minimind) — ⭐58,847. Demonstrates training a 64M-parameter LLM from scratch in about two hours.
- [skyzh/tiny-llm](https://github.com/skyzh/tiny-llm) — ⭐4,543. A hands-on guide for systems engineers learning LLM inference on Apple Silicon.
- [llm-jp/awesome-japanese-llm](https://github.com/llm-jp/awesome-japanese-llm) — ⭐1,425. A community-maintained overview of Japanese LLMs and resources.
- [anseryuer/Local_LLM_Deployment_Guide_Chinese](https://github.com/anseryuer/Local_LLM_Deployment_Guide_Chinese) — ⭐51. Chinese-language teaching material for deploying local LLMs.

### 🔍 RAG / Knowledge

- [firecrawl/firecrawl](https://github.com/firecrawl/firecrawl) — ⭐176,950. The now-standard context API for search, scraping, and web interaction at scale.
- [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) — ⭐115,110. Turns any codebase and docs into a queryable knowledge graph and runs as a skill for Claude Code, Cursor, Codex, and Gemini CLI.
- [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) — ⭐93,300. Persistent cross-session context for Claude Code and other coding agents.
- [infiniflow/ragflow](https://github.com/infiniflow/ragflow) — ⭐90,104. Leading open-source RAG engine with deep agent capability integration.
- [mem0ai/mem0](https://github.com/mem0ai/mem0) — ⭐64,752. A production-oriented memory layer that gives AI agents persistent long-term context.
- [Mintplex-Labs/anything-llm](https://github.com/Mintplex-Labs/anything-llm) — ⭐65,667. All-in-one local-first RAG and agent workspace for your own documents and models.
- [run-llama/llama_index](https://github.com/run-llama/llama_index) — ⭐52,032. The leading document agent and OCR data framework for RAG workloads.
- [milvus-io/milvus](https://github.com/milvus-io/milvus) — ⭐45,989. Cloud-native vector database for scalable ANN search; still core RAG infrastructure.
- [qdrant/qdrant](https://github.com/qdrant/qdrant) — ⭐34,402. High-performance vector database frequently used with the current generation of AI agents.

## 3. Trend Signal Analysis

The explosive community attention today is clearly on **Agent Skills and portable agent behavior**. Nearly one-third of the trending list is skills-related — [mattpocock/skills](https://github.com/mattpocock/skills), [anthropics/skills](https://github.com/anthropics/skills), [humanlayer/skills](https://github.com/humanlayer/skills), [blader/humanizer](https://github.com/blader/humanizer), [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail), and [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design). The shared vocabulary — “from my .agents directory,” “Claude Code, Codex, OpenCode, Cursor” — shows that the market is moving from building new agent runtimes to distributing reusable skills for existing runtimes. Anthropic’s official skills repo appearing on the hot list gives this layer a canonical signal.

A second clear direction is **agent-native local inference**. [magnitudedev/magnitude](https://github.com/magnitudedev/magnitude) sells itself not as another model runner but as an inference server plugged into the agent you already use. Meanwhile [ollama/ollama](https://github.com/ollama/ollama) now advertises support for an unusually broad set of recent open-weight models such as Kimi-K2.6, GLM-5.2, MiniMax, GPT-OSS, and Qwen. This connects to a broader industry trend: as frontier open models churn weekly, developers are standardizing on agent harnesses and local serving layers instead of rebinding to a single vendor model.

The third signal is **memory and context as infrastructure**. Projects like [claude-mem](https://github.com/thedotmack/claude-mem), [mem0](https://github.com/mem0ai/mem0), and [cognee](https://github.com/topoteretes/cognee) treat agent context as a persistent asset rather than a single chat session. That aligns with the rise of long-running “agents that grow with you,” such as [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent), and it is likely to become the next major battleground in the open-source agent stack.

## 4. Community Hot Spots

- **Agent Skills directories are the new “awesome lists.”** Repos like [mattpocock/skills](https://github.com/mattpocock/skills) and [humanlayer/skills](https://github.com/humanlayer/skills) show that the community is racing to build the package registry of agent capabilities. Worth watching closely for emerging naming and structure conventions.
- **Cross-agent compatibility is the new moat.** [ECC](https://github.com/affaan-m/ECC), [anomalyco/opencode](https://github.com/anomalyco/opencode), and [everything-claude-code](https://github.com/WorldFlowAI/everything-claude-code) all advertise compatibility with multiple coding agents, indicating that users want one skill/harness layer across every CLI.
- **Memory and context compression are heating up.** [claude-mem](https://github.com/thedotmack/claude-mem), [mem0](https://github.com/mem0ai/mem0), and [headroom](https://github.com/headroomlabs-ai/headroom) attack the same problem from different angles: context is too large, too fragmented, and too expensive.
- **Local inference is becoming agent-centric.** [Ollama](https://github.com/ollama/ollama) keeps adding new model architectures, while [Magnitude](https://github.com/magnitudedev/magnitude) optimizes which local model runs on which hardware for the agent already in use.
- **Vertical agentic applications are maturing.** [career-ops](https://github.com/career-ops-hq/career-ops), [ppt-master](https://github.com/hugohe3/ppt-master), and [daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis) show that open-source AI projects are increasingly shipping end-to-end practical agents rather than only frameworks.

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*