# AI Open Source Trends 2026-07-09

> Sources: GitHub Trending + GitHub Search API | Generated: 2026-07-08 17:22 UTC

---

# AI Open Source Trends Report — 2026-07-09

## 1. Today's Highlights

The AI open-source ecosystem today is dominated by a massive wave of **agentic automation tools**, with job-search and office-productivity agents topping the trending charts. **MadsLorentzen/ai-job-search** exploded with +5,071 stars, showcasing how Claude Code-powered autonomous job applications have become a killer use case. Meanwhile, **iOfficeAI/OfficeCLI** (+1,712 stars) and **addyosmani/agent-skills** (+1,322 stars) signal a broader push to give AI agents production-grade tool access to real-world software suites. The emergence of **system prompts leaks** as a trending category (+1,226 stars) reveals the community's hunger to understand and reverse-engineer the latest LLM behaviors from Anthropic, OpenAI, Google, and others. Peripherally, lightweight in-process vector databases (Alibaba's **zvec**) and WiFi-based sensing (**RuView**) are gaining traction as specialized AI infrastructure components.

## 2. Top Projects by Category

### 🔧 AI Infrastructure (Frameworks, SDKs, Inference Engines, Dev Tools, CLI)

- **[vllm-project/vllm](https://github.com/vllm-project/vllm)** ⭐85,715 — High-throughput LLM inference serving engine, critical for production deployment of open models.
- **[ollama/ollama](https://github.com/ollama/ollama)** ⭐175,733 — The go-to local model runner, now supporting Kimi-K2.6, GLM-5.1, DeepSeek, and more.
- **[huggingface/transformers](https://github.com/huggingface/transformers)** ⭐162,378 — The definitive model-definition framework for text, vision, audio, and multimodal models.
- **[Picovoice/picollm](https://github.com/Picovoice/picollm)** ⭐315 — On-device LLM inference using X-bit quantization, enabling truly local AI.
- **[langchain-ai/langchain](https://github.com/langchain-ai/langchain)** ⭐141,310 — The agent engineering platform, foundational for building LLM-powered applications.
- **[0xPlaygrounds/rig](https://github.com/0xPlaygrounds/rig)** ⭐7,861 — Modular, scalable LLM application framework in Rust, gaining traction for performance-critical agent systems.
- **[LancerLab/croqtile](https://github.com/LancerLab/croqtile)** ⭐34 — A next-gen AI-native kernel programming DSL, early-stage but notable for pushing AI closer to hardware.

### 🤖 AI Agents / Workflows (Agent Frameworks, Automation, Multi-Agent Systems)

- **[MadsLorentzen/ai-job-search](https://github.com/MadsLorentzen/ai-job-search)** ⭐0 (+5,071 today) — Claude Code-powered job application framework: evaluates jobs, tailors CVs, writes cover letters autonomously. **Today's #1 trending project.**
- **[addyosmani/agent-skills](https://github.com/addyosmani/agent-skills)** ⭐0 (+1,322 today) — Production-grade engineering skills for AI coding agents, bridging the gap between toy demos and real-world use.
- **[iOfficeAI/OfficeCLI](https://github.com/iOfficeAI/OfficeCLI)** ⭐0 (+1,712 today) — Open-source CLI for AI agents to read, edit, and automate Word/Excel/PowerPoint files without Office installation.
- **[NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)** ⭐211,464 — "The agent that grows with you" — a fast-growing, highly starred general-purpose agent framework.
- **[santifer/career-ops](https://github.com/santifer/career-ops)** ⭐59,163 — AI job search agent that scans portals, scores listings, and tailors CVs locally in your AI coding CLI.
- **[Panniantong/Agent-Reach](https://github.com/Panniantong/Agent-Reach)** ⭐53,137 — Multi-platform social media reader for AI agents (Twitter, Reddit, YouTube, GitHub, Bilibili, etc.) — zero API fees.
- **[CherryHQ/cherry-studio](https://github.com/CherryHQ/cherry-studio)** ⭐48,315 — AI productivity studio with smart chat, autonomous agents, and 300+ assistants.
- **[wonderwhy-er/DesktopCommanderMCP](https://github.com/wonderwhy-er/DesktopCommanderMCP)** ⭐0 (+20 today) — MCP server giving Claude terminal control, file system search, and diff editing capabilities.

### 📦 AI Applications (Specific Apps, Vertical Solutions)

- **[bradautomates/claude-video](https://github.com/bradautomates/claude-video)** ⭐0 (+948 today) — Gives Claude the ability to watch videos: downloads, extracts frames, transcribes, hands to Claude.
- **[mvanhorn/last30days-skill](https://github.com/mvanhorn/last30days-skill)** ⭐0 (+373 today) — Research agent that summarizes any topic from Reddit, X, YouTube, HN, Polymarket, and the web.
- **[TauricResearch/TradingAgents](https://github.com/TauricResearch/TradingAgents)** ⭐91,833 — Multi-agent LLM financial trading framework, a vertical application with explosive community interest.
- **[ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis)** ⭐55,851 — LLM-powered multi-market stock analysis with real-time news, dashboards, and automated notifications.
- **[hugohe3/ppt-master](https://github.com/hugohe3/ppt-master)** ⭐37,737 — AI generates editable PowerPoint presentations from any document, with native animations and voiceovers.
- **[Diolinux/PhotoGIMP](https://github.com/Diolinux/PhotoGIMP)** ⭐0 (+2,031 today) — Patch for GIMP 3+ to mimic Photoshop UI — while not AI itself, its massive popularity reflects the creative tooling ecosystem.

### 🧠 LLMs / Training (Model Weights, Training Frameworks, Fine-Tuning Tools)

- **[galilai-group/stable-pretraining](https://github.com/galilai-group/stable-pretraining)** ⭐281 — Minimal, reliable library for pretraining foundation and world models.
- **[open-compass/opencompass](https://github.com/open-compass/opencompass)** ⭐7,172 — Comprehensive LLM evaluation platform supporting 100+ datasets.
- **[chrisliu298/awesome-llm-unlearning](https://github.com/chrisliu298/awesome-llm-unlearning)** ⭐607 — Curated resources for machine unlearning in LLMs — an emerging safety-critical direction.
- **[testtimescaling/testtimescaling.github.io](https://github.com/testtimescaling/testtimescaling.github.io)** ⭐107 — Survey on test-time scaling in LLMs — reflecting the industry's pivot toward inference-time compute.
- **[multimindlab/multimind-sdk](https://github.com/multimindlab/multimind-sdk)** ⭐92 — Unified interface for local + hosted models, fine-tuning, agent tools, and hybrid RAG.

### 🔍 RAG / Knowledge (Vector Databases, Retrieval-Augmented Generation, Knowledge Management)

- **[infiniflow/ragflow](https://github.com/infiniflow/ragflow)** ⭐84,604 — Leading open-source RAG engine fusing cutting-edge retrieval with agent capabilities.
- **[milvus-io/milvus](https://github.com/milvus-io/milvus)** ⭐45,133 — Cloud-native vector database for scalable ANN search — industry standard.
- **[Qdrant/qdrant](https://github.com/qdrant/qdrant)** ⭐33,043 — High-performance vector database and search engine for next-gen AI.
- **[alibaba/zvec](https://github.com/alibaba/zvec)** ⭐14,308 (+370 today) — Lightweight, lightning-fast in-process vector database — trending today for its embedded deployment model.
- **[NousSearch/LEANN](https://github.com/StarTrail-org/LEANN)** ⭐12,655 — 97% storage savings for private RAG on personal devices (MLsys2026 publication).
- **[mem0ai/mem0](https://github.com/mem0ai/mem0)** ⭐60,411 — Universal memory layer for AI agents, enabling persistent context across sessions.
- **[thedotmack/claude-mem](https://github.com/thedotmack/claude-mem)** ⭐86,428 — Captures, compresses, and injects agent session context across sessions — works with Claude Code, OpenClaw, Codex, etc.
- **[headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom)** ⭐57,807 — Compresses tool outputs and RAG chunks by 60-95% before reaching the LLM — a token optimization essential.

## 3. Trend Signal Analysis

The **#1 trend today** is the commoditization of **agentic job and career tools**. Between **MadsLorentzen/ai-job-search** (+5,071 stars), **santifer/career-ops** (+59k cumulative), and **iOfficeAI/OfficeCLI** (+1,712 today), the community is aggressively building agents that automate the entire job-search lifecycle — from scanning listings to tailoring CVs to writing cover letters. This represents a paradigm shift: developers are no longer building "AI toys" but production-grade, self-hosted personal career assistants.

A second major signal is the **explosion of system prompt reverse-engineering**. The project **system_prompts_leaks** (+1,226 stars today) has extracted and shared the actual system prompts from Claude Fable 5, GPT 5.5, Gemini 3.5 Flash, Copilot, Grok, and many more. This indicates a community-wide hunger to understand how frontier models are being instructed — and to replicate or improve upon those designs in open-source agents.

Third, **persistent memory for agents** has reached critical mass. **claude-mem** (86k stars), **mem0** (60k stars), **TencentDB-Agent-Memory** (+351 today), and **cognee** (27k stars) all solve the same core problem: giving AI agents long-term, cross-session memory. This is the missing piece for moving agents from "one-shot taskers" to "co-workers" that learn and grow.

Finally, **in-process vector databases** are emerging as a hot new category. Alibaba's **zvec** (+370 today) is a lightweight, lightning-fast vector database that runs inside your application process — no separate server needed. This contrasts with the heavyweight cloud-native vector DBs (Milvus, Qdrant) and suggests a growing need for **embedded AI retrieval** in edge devices and desktop applications.

**New directions appearing today**: WiFi-based spatial intelligence (**RuView**), AI-native kernel programming DSLs (**croqtile**), and token-efficiency middleware (**headroom**) all point toward a maturing ecosystem where AI agents need richer sensory inputs, more efficient compute, and tighter integration with hardware.

## 4. Community Hot Spots

- **🤖 Job-Search Automation Agents** — **MadsLorentzen/ai-job-search** (+5k stars) and **santifer/career-ops** (+59k stars) are the hottest category. These represent the first truly "useful" autonomous agents that deliver measurable real-world value. **Worth focusing on**: building skill integrations for these frameworks.

- **📄 Office & Document Automation for AI** — **iOfficeAI/OfficeCLI** (+1.7k today) and **hugohe3/ppt-master** (+37k stars) show agents are moving into actual productivity software. **Worth focusing on**: building connectors for Google Docs, Notion, or enterprise document management systems.

- **🔑 System Prompt Engineering & Reverse Engineering** — **system_prompts_leaks** (+1.2k today) has become a must-watch resource. Understanding how Anthropic, OpenAI, and Google structure their prompts is critical for building competitive open-source agents. **Worth focusing on**: contributing reverse-engineered prompts or building prompt optimization tools.

- **💾 Agent Memory & Context Persistence** — **claude-mem** (86k stars) and **mem0** (60k stars) are establishing the standard for cross-session memory. **TencentDB-Agent-Memory** (+351 today) adds a 4-tier progressive pipeline. **Worth focusing on**: integrating these memory systems into your agent frameworks.

- **🔍 Embedded Vector Search** — **alibaba/zvec** (+370 today) and **lancedb** (10k stars) represent a shift toward lightweight, in-process vector databases that run alongside your application. **Worth focusing on**: deploying zvec for edge/IoT AI applications where latency and resource constraints matter.

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*