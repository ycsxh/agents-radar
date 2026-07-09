# AI CLI Tools Community Digest 2026-07-09

> Generated: 2026-07-09 03:55 UTC | Tools covered: 9

- [Claude Code](https://github.com/anthropics/claude-code)
- [OpenAI Codex](https://github.com/openai/codex)
- [Gemini CLI](https://github.com/google-gemini/gemini-cli)
- [GitHub Copilot CLI](https://github.com/github/copilot-cli)
- [Kimi Code CLI](https://github.com/MoonshotAI/kimi-cli)
- [OpenCode](https://github.com/anomalyco/opencode)
- [Pi](https://github.com/badlogic/pi-mono)
- [Qwen Code](https://github.com/QwenLM/qwen-code)
- [DeepSeek TUI](https://github.com/Hmbown/DeepSeek-TUI)
- [Claude Code Skills](https://github.com/anthropics/skills)

---

## Cross-Tool Comparison

## Cross-Tool Comparison Report – AI CLI Developer Tools (2026-07-09)

### 1. Ecosystem Overview
The AI CLI tools landscape continues to accelerate, with major players shipping near‑daily releases while grappling with compounding reliability, cost, and security challenges. Autonomous agent orchestration, persistent session state, and multi‑model routing are the dominant development themes across communities. However, many tools are currently held back by fragile agent‑lifecycle bugs, token overconsumption, and platform‑specific glitches that erode trust for long‑running or unattended workflows. The growing number of daemon‑based, multi‑workspace, and provider‑agnostic tools signals a shift from simple chat‑based copilots toward composable, programmatic AI workbenches.

### 2. Activity Comparison (2026-07-09)

| Tool | Active Issues | PRs Updated | New Release Today |
|------|---------------|-------------|-------------------|
| **Claude Code** | 10 | 5 | Yes (v2.1.205) |
| **OpenAI Codex** | 10 | 10 | Yes (0.143.0 + 0.144.0‑alpha.2) |
| **Gemini CLI** | 10 | 10 | Yes (v0.50.0 + v0.51.0‑preview.0) |
| **GitHub Copilot CLI** | 10 | 0 | No |
| **Kimi Code CLI** | 1 | 0 | No |
| **OpenCode** | 10 | 10 | No |
| **Pi** | 10 | 8 | No |
| **Qwen Code** | 10 | 10 | Yes (v0.19.8) |
| **DeepSeek TUI (CodeWhale)** | 10 | 10 | No |

*Note: Issue and PR counts reflect items highlighted in each digest (generally the top‑10 or all notable activity).*

### 3. Shared Feature Directions
Several requirements appear across multiple tool communities:

- **Long‑running autonomous agents with persistent state**  
  Claude Code (#56913 tiered orchestrator), Gemini CLI (sub‑agent recovery, generalist hangs), Qwen Code (multi‑workspace daemon, sub‑agent stability), Pi (multi‑session TUI, settlement bugs), DeepSeek TUI (fleet worker profiles). Users want agents that can run for hours/days, survive restarts, and manage complex task pipelines without losing context.

- **Multi‑model orchestration and intelligent routing**  
  Claude Code (Opus/Sonnet tiering), GitHub Copilot CLI (auto‑switch for planning vs. execution, model family aliases), DeepSeek TUI (per‑sub‑agent provider assignment), OpenCode (multi‑LLM team debate), Pi (ephemeral in‑session model changes). The trend is toward “set‑and‑forget” model policies that optimise cost and quality.

- **Better session management and recovery**  
  Claude Code (phantom/archived sessions, sticky date bugs), GitHub Copilot CLI (broken `/resume`, opaque session IDs), OpenCode (web UI session retrieval failures, workspace state loss), Pi (settlement/continuation meta‑bug). Developers need reliable pause/resume/restore across crashes, restarts, and IDE transitions.

- **Multimodal input reliability (image/file paste/drag‑and‑drop)**  
  Qwen Code (#6560 missing paste/drag), Pi (clipboard broken on Linux Bun builds, images not sent to model), Claude Code (UTF‑8 surrogate errors on image attachments). Seamless visual context input is a widely shared but fragile capability.

- **Cost transparency and token consumption controls**  
  Claude Code (extreme token burn, auto‑compaction bugs, billing mismatches), OpenAI Codex (SSD‑wear from logging, quota surprises), GitHub Copilot CLI (extended‑context pricing), Gemini CLI (recursive reasoning caps). Users demand granular, predictable cost visibility and built‑in guardrails.

- **Security hardening and workspace trust**  
  Gemini CLI (RCE fixes, workspace trust enforcement), Claude Code (protect‑mcp plugin with Cedar policies), OpenCode (permission path inconsistencies, read‑only pip invocations). As agents gain shell access, communities push for fail‑safe sandboxing, policy gates, and signed receipts.

- **Project‑level custom instructions and prompts**  
  GitHub Copilot CLI (`.github/prompts` custom slash commands), OpenCode (`AGENTS.md` directives ignored by Big Pickle agent), Pi (extension hooks before session load). Users want agents to reliably consume repository‑specific rules, comparable to IDE extensions.

### 4. Differentiation Analysis

| Tool | Primary Focus | Target Users | Technical Approach |
|------|--------------|--------------|--------------------|
| **Claude Code** | Autonomous team‑of‑agents orchestrator, Cowork virtual workspaces | Power users, advanced automation | Tiered model brain (Opus/Sonnet), sub‑agent hierarchy, advanced compaction; suffers from token overconsumption and VM fragility |
| **OpenAI Codex** | Broad CLI + IDE + desktop ecosystem; LSP, memory, Computer Use | OpenAI ecosystem and enterprise developers | Rust backend, extensible plugin system, goal‑oriented execution; strong on enterprise auth but plagued by tool‑call regressions and installer fragility |
| **Gemini CLI** | Safety‑first coding agent, AST‑aware tools, evaluations | Google/GCP developers, security‑conscious teams | Tool registry, workspace trust enforcement, recursive reasoning caps; sub‑agent reliability and Wayland compatibility remain weak |
| **GitHub Copilot CLI** | Tight GitHub integration, slash commands, simple copilot‑style interactions | GitHub‑ecosystem developers | Lightweight CLI with model switching and team prompt sharing; currently struggling with auth, configuration persistence, and session recovery |
| **OpenCode** | Multi‑provider, desktop‑first agent with web UI | Power users wanting flexible provider choice and rich interfaces | Bun runtime, many provider integrations, bidirectional pagination; permission handling and cross‑browser quirks are ongoing issues |
| **Pi** | Fast TUI coding agent, ephemeral configuration, extensible hooks | Developers who prefer terminal‑first, hackable tools | Rust/Bun monorepo, JSONL session history, prompt cache observability; clipboard and continuation bugs hurt reliability |
| **Qwen Code** | Daemon‑based multi‑workspace team assistant, channels & webhooks | Chinese‑speaking teams, enterprise collaboration | Daemon process for multiple workspaces, WeCom/Slack channels, scheduled tasks; missing multimodal input is a top complaint |
| **DeepSeek TUI (CodeWhale)** | High‑velocity, multi‑provider fleet agent with Android support | Power users, multi‑model experimenters, mobile/Termux | Massive integration pace, per‑agent provider routing, tool sandboxing; streaming performance and CJK rendering lag behind feature velocity |
| **Kimi Code CLI** | Minimal‑activity CLI; only a single SSL‑bypass request active | Early stage / enterprise evaluation | Lightweight authentication; currently blocked by corporate TLS inspection proxies, no further visible development |

### 5. Community Momentum & Maturity
- **Most active communities (engagement + PR velocity)**: DeepSeek TUI (10+ PRs/day, fleet coordination board with 55 comments), Pi (8 merged PRs, rapid bug‑fix cycle), Gemini CLI (dual releases + 10 PRs). Claude Code and OpenAI Codex have large, vocal user bases with highly‑voted issues (e.g., 427 👍 on Codex’s SSD‑wear bug, 51 👍 on Claude’s `/delete` request).  
- **Fastest iteration cycles**: DeepSeek TUI and Pi ship fixes and features daily; Qwen Code and Gemini CLI also maintain frequent release cadences. Claude Code and Codex are more measured but still release almost daily patches.  
- **Maturity signs**: Codex and Claude Code have deeper feature sets (team agents, memory, workspaces) but suffer from compound reliability issues. Qwen Code is rapidly maturing with daemon, channel, and scheduled task capabilities. GitHub Copilot CLI feels less feature‑rich and is currently in a holding pattern with no PRs and several configuration regressions. Kimi Code CLI shows minimal viability.

### 6. Trend Signals (for technical decision‑makers and developers)
- **Autonomous agents are moving from chat loops to persistent orchestrators**—expect built‑in daemon processes, sub‑agent hierarchies, and background task queues to become table stakes.
- **Cost management is now a critical product differentiator**—tools that don’t offer token‑usage dashboards, per‑agent billing, and anti‑burn safeguards will face user revolt.
- **Multi‑model routing is the new normal**—rigid single‑provider sessions are giving way to “reasoning‑model for plan, flash‑model for execution” patterns, including per‑sub‑agent provider assignment.
- **Security and supply‑chain integrity**—with agents executing shell commands and loading external MCP plugins, communities are demanding workspace trust policies, signed receipts, and fail‑closed sandboxes (see Gemini CLI RCE fixes, Claude Code plugin policy gates).
- **Workspace‑ and team‑aware collaboration**—daemon‑based multi‑workspace support (Qwen Code, OpenCode) and custom project‑level prompts (Copilot CLI `.github/prompts`) point to the future: AI tools that natively respect repository context and team conventions.
- **Reliability gaps undermine autonomous workflows**—despite feature velocity, most tools share a painful glass‑ceiling: agent state corrupts after compaction, sessions never recover, and background tasks can’t be cancelled. Teams betting on unattended workflows should evaluate these failure modes carefully.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills — Community Highlights Report
**Data as of 2026-07-09 | Repository: [anthropics/skills](https://github.com/anthropics/skills)**

---

## 1. Top Skills Ranking

The following represent the most actively discussed Skill submissions and improvements, ranked by community engagement:

| # | PR | Skill / Change | Functionality | Status |
|---|-----|----------------|---------------|--------|
| 1 | [#1298](https://github.com/anthropics/skills/pull/1298) | **skill-creator: eval recall fix** | Fixes `run_eval.py` reporting 0% recall for every skill description — the description-optimization loop was effectively optimizing against noise. Also fixes Windows stream reading, trigger detection, and parallel worker isolation. 10+ independent reproductions. | OPEN |
| 2 | [#514](https://github.com/anthropics/skills/pull/514) | **document-typography** | Prevents orphan word wrap (1–6 words spilling to next line), widow paragraphs (headers stranded at page bottom), and numbering misalignment in AI-generated documents. Addresses typographic quality issues that affect every document Claude produces. | OPEN |
| 3 | [#486](https://github.com/anthropics/skills/pull/486) | **ODT skill** | Full OpenDocument Format support: create, fill, read, and convert `.odt` / `.ods` files. Includes template filling and ODT-to-HTML parsing. Triggered by mentions of ODF, LibreOffice, or ISO-standard document formats. | OPEN |
| 4 | [#83](https://github.com/anthropics/skills/pull/83) | **skill-quality-analyzer + skill-security-analyzer** | Two meta-skills for the marketplace. The quality analyzer evaluates Skills across five dimensions (Structure, Documentation, Examples, Resources, Implementation — 20% each). The security analyzer audits Skills for injection, path traversal, and trust-boundary issues. | OPEN |
| 5 | [#723](https://github.com/anthropics/skills/pull/723) | **testing-patterns** | Comprehensive testing stack: Testing Trophy model philosophy, AAA unit-test pattern, React component testing via Testing Library, edge-case strategies, and guidance on what NOT to test. | OPEN |
| 6 | [#1302](https://github.com/anthropics/skills/pull/1302) | **color-expert** | Self-contained color-expertise skill covering naming systems (ISCC-NBS, Munsell, XKCD, RAL, CSS named), color-space selection (OKLCH for scales, OKLAB for gradients, CAM16 for perceptual work), and accessibility contrast guidance. | OPEN |
| 7 | [#1367](https://github.com/anthropics/skills/pull/1367) | **self-audit** | Pre-delivery output audit: mechanical file-verification first, then a four-dimension reasoning quality gate ordered by damage severity. Universal — stack-agnostic, model-agnostic. | OPEN |
| 8 | [#210](https://github.com/anthropics/skills/pull/210) | **frontend-design (rewrite)** | Revises the existing frontend-design skill for clarity and actionability, ensuring every instruction is executable within a single conversation and specific enough to steer behavior without ambiguity. | OPEN |

> **Notable non-Skill PRs:** Multiple critical fixes cluster around the `skill-creator` evaluation pipeline — [#1099](https://github.com/anthropics/skills/pull/1099) (Windows subprocess crash), [#1050](https://github.com/anthropics/skills/pull/1050) (Windows PATHEXT + encoding), [#1323](https://github.com/anthropics/skills/pull/1323) (trigger detection misses real skill names), [#1261](https://github.com/anthropics/skills/pull/1261) (eval isolation from live project registry), [#362](https://github.com/anthropics/skills/pull/362) (UTF-8 panic on multi-byte characters), and [#361](https://github.com/anthropics/skills/pull/361) / [#539](https://github.com/anthropics/skills/pull/539) (YAML special-character parsing). These indicate the tooling surface is receiving heavy real-world stress.

---

## 2. Community Demand Trends

Analysis of the top Issues reveals concentrated demand in these directions:

### 🔐 Security & Trust (#1 concern — 34 comments)
- **[#492](https://github.com/anthropics/skills/issues/492):** Community skills distributed under the `anthropic/` namespace create a trust-boundary vulnerability — users may grant elevated permissions to community skills they believe are official. This is the single most-discussed issue in the entire repository.
- **[#1175](https://github.com/anthropics/skills/issues/1175):** Access-control and permission logic embedded in SKILL.md for SharePoint Online document handling raises context-window and security concerns.

### 🏢 Enterprise Readiness
- **[#228](https://github.com/anthropics/skills/issues/228):** Org-wide skill sharing (14 comments, 7 👍) — users want a shared skill library or direct sharing link instead of manual `.skill` file downloads and Slack/Teams transfers.

### 🧠 Agent Memory & Governance
- **[#1329](https://github.com/anthropics/skills/issues/1329):** `compact-memory` — symbolic notation for compact agent state, reducing context consumption from prose-based persistent memory.
- **[#412](https://github.com/anthropics/skills/issues/412):** `agent-governance` — safety patterns for AI agent systems: policy enforcement, threat detection, trust scoring, and audit trails.

### 🔧 Developer Tooling & Platform Parity
- **[#556](https://github.com/anthropics/skills/issues/556) (12 comments, 7 👍) + [#1169](https://github.com/anthropics/skills/issues/1169) + [#1061](https://github.com/anthropics/skills/issues/1061):** The `skill-creator` eval loop is broken or severely degraded on multiple platforms. Windows compatibility is a recurring pain point (PATHEXT, encoding, pipe handling).
- **[#189](https://github.com/anthropics/skills/issues/189) (6 comments, 9 👍):** `document-skills` and `example-skills` plugins install identical content, causing duplicated skills and wasted context window.
- **[#16](https://github.com/anthropics/skills/issues/16):** Demand for exposing Skills as MCP (Model Context Protocol) endpoints for programmatic consumption.

### 📄 Document & Web Ecosystem
- **[#1362](https://github.com/anthropics/skills/issues/1362):** `web-artifacts-builder` bundle/init scripts fail on modern pnpm; fonts not inlined, stale favicon strip — hard blockers for web artifact production.

---

## 3. High-Potential Pending Skills

These active, comment-heavy PRs are not yet merged and represent Skills most likely to land:

| PR | Skill | Why It Matters |
|----|-------|----------------|
| [#514](https://github.com/anthropics/skills/pull/514) | **document-typography** | Solves a universal output-quality problem; no existing skill covers this. High discussion volume. |
| [#486](https://github.com/anthropics/skills/pull/486) | **ODT skill** | Fills a format gap (ISO-standard ODF); enterprise/LibreOffice users are underserved. |
| [#723](https://github.com/anthropics/skills/pull/723) | **testing-patterns** | Testing is a top user workflow; this skill is comprehensive and pragmatic. |
| [#1367](https://github.com/anthropics/skills/pull/1367) | **self-audit** | Addresses output reliability directly — a meta-skill with universal applicability. |
| [#1302](https://github.com/anthropics/skills/pull/1302) | **color-expert** | Deep domain expertise packaged as a Skill; signals maturation of the Skills ecosystem toward specialized professional tools. |
| [#83](https://github.com/anthropics/skills/pull/83) | **skill-quality-analyzer + skill-security-analyzer** | Addresses the trust/quality gap (#492) with tooling rather than policy — meta-governance. |

---

## 4. Skills Ecosystem Insight

**The community's most concentrated demand is for *trust and reliability infrastructure*:** security vetting of Skills, trustworthy namespacing, functional evaluation tooling that actually works cross-platform, and governance/audit meta-skills — before the ecosystem can scale, contributors are insisting the scaffolding itself be hardened.

---

# Claude Code Community Digest — 2026-07-09

## Today’s Highlights
A targeted v2.1.205 release tightens session security and JSON schema handling, while the community continues to grapple with severe token consumption anomalies and Cowork reliability on Windows. The conversations reveal strong demand for autonomous, long-running agents with persistent state and better cost controls.

## Releases
**v2.1.205**  
- Added an auto‑mode rule that blocks tampering with session transcript files.  
- Fixed `--json-schema` silently producing unstructured output on invalid schemas, and fixed rejection of schemas using the `format` keyword.  
- Fixed a message being silently dropped while Claude was working (truncated note).

## Hot Issues

1. **[#42249](https://github.com/anthropics/claude-code/issues/42249) Extreme token consumption – quota depleted in minutes**  
   Normal tasks drain daily limits in ~1 hour. 43 comments, 17 👍. The community is deeply frustrated and seeking root causes.

2. **[#56913](https://github.com/anthropics/claude-code/issues/56913) Make autonomous Claude Code truly viable (tiered Opus brains + Sonnet workers + persistent state)**  
   High‑engagement design discussion (43 comments) about running Claude Code as a long‑running orchestrator for pipelines, ML training, and automation. No upvotes yet but intense technical debate.

3. **[#38993](https://github.com/anthropics/claude-code/issues/38993) Cowork: virtiofs FUSE mount serves truncated/stale files**  
   Host‑side file changes aren’t reflected inside the VM, corrupting agent workflows. 41 comments, 28 👍 – a top‑voted bug.

4. **[#54776](https://github.com/anthropics/claude-code/issues/54776) Unexpected high API usage – 100% quota in 1‑2 hours**  
   A 20‑user reports a sudden 10× increase in consumption after normal usage. 33 comments, 12 👍.

5. **[#74649](https://github.com/anthropics/claude-code/issues/74649) Missing HCS services (vfpext) – Cowork broken on Windows 11 Pro**  
   25 comments; numerous users confirm Cowork is completely non‑functional due to missing host compute service components.

6. **[#61993](https://github.com/anthropics/claude-code/issues/61993) Sub‑agents cannot spawn other sub‑agents**  
   The `Task`/`Agent` primitive is unavailable in nested contexts, limiting agent hierarchy. 19 comments – important for complex autonomous workflows.

7. **[#70459](https://github.com/anthropics/claude-code/issues/70459) Auto‑compaction: two compounding cost bugs**  
   Stale precompute keeps ~200k tokens verbatim, and the bloated prefix is repeatedly cache‑created instead of cache‑read. 6 comments, 3 👍 – a technically insightful analysis with significant cost implications.

8. **[#47930](https://github.com/anthropics/claude-code/issues/47930) Agent Teams loops on idle notifications, burns ~13‑22% of input tokens on no‑op acks**  
   Lead session duplicates task_assignment echoes and wastes tokens. 6 comments, 8 👍 – persistent inefficiency in multi‑agent setups.

9. **[#73597](https://github.com/anthropics/claude-code/issues/73597) Opus sub‑agents are being billed as Fable usage**  
   Billing mismatch, 4 comments. Undermines cost expectations for Opus‑based agent teams.

10. **[#75314](https://github.com/anthropics/claude-code/issues/75314) 10 background Agent tasks stuck for 34+ hours, no way to cancel, burned ~1M tokens**  
    Critical runaway agent incident with no cancellation mechanism. 3 comments, raising alarms about resource management.

## Key PR Progress
*(Only 5 PRs updated in the last 24h; all are highlighted.)*

- **[#75938](https://github.com/anthropics/claude-code/pull/75938) fix(sweep): unstarve markStale via search API; snapshot listings before mutating** – Fixes the issue sweeper so stale labels are actually applied; prevents permanent skips.

- **[#41447](https://github.com/anthropics/claude-code/pull/41447) feat: open source claude code ✨** – Proposed open‑sourcing; references multiple related issues.

- **[#75541](https://github.com/anthropics/claude-code/pull/75541) fix(sweep): paginate issue events and honor unlabeled when closing expired issues** – Properly handles event pagination to avoid prematurely closing issues that were already unlabeled.

- **[#72014](https://github.com/anthropics/claude-code/pull/72014) Add protect-mcp plugin: fail‑closed Cedar policy gate + signed receipts** – Introduces a plugin that enforces policy‑based blocking of MCP tool calls and generates offline‑verifiable receipts.

- **[#68673](https://github.com/anthropics/claude-code/pull/68673) fix(scripts): break pagination when page is not full, not only when empty** – Corrects pagination logic in scripts to avoid unnecessary API calls.

## Feature Request Trends
- **Autonomous orchestration**: Tiered model selection (e.g., Opus for planning, Sonnet for workers), persistent state across sessions, and true background task execution ([#56913](https://github.com/anthropics/claude-code/issues/56913)).  
- **Session management**: `/fork` branching in VS Code ([#46451](https://github.com/anthropics/claude-code/issues/46451), closed), `/delete` command for sessions ([#26904](https://github.com/anthropics/claude-code/issues/26904) – 51 👍).  
- **Parallel worktree support**: `--worktree` for isolated parallel sessions in VS Code ([#69554](https://github.com/anthropics/claude-code/issues/69554)).  
- **Resilience & auto‑recovery**: Automatic retry on transient 529 errors ([#60577](https://github.com/anthropics/claude-code/issues/60577), closed), self‑healing for stuck sub‑agents, and better long‑running session date awareness ([#73800](https://github.com/anthropics/claude-code/issues/73800)).  
- **Cost transparency**: Clearer breakdown of token usage per model, agent, and operation type, driven by the repeated cost‑surprise bugs.

## Developer Pain Points
- **Rampant token overconsumption**: Multiple compounding bugs (auto‑compaction, agent loops, idle notifications, billing mismatches) are making quota usage unpredictable and expensive.  
- **Cowork fragility**: File system stale/truncated data, missing HCS services on Windows, cross‑device link errors, and unsupported “yukonSilver” environments break the virtual workspace promise.  
- **Agent hierarchy limitations**: Sub‑agents can’t spawn sub‑agents, background tasks can’t be cancelled, and lead sessions waste tokens on duplicate acknowledgements.  
- **Session and context drift**: Long sessions use stale dates; mobile/desktop sync shows archived or phantom sessions; compaction precompute bloat increases costs instead of reducing them.  
- **Tool and plugin reliability**: Skills activation orphans tool calls, image attachments fail with UTF‑8 surrogate errors, and MCP connectors disappear in remote environments.

---

*Digest generated from GitHub data updated as of 2026-07-09. For detailed discussion, follow the linked issues and PRs.*

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-07-09

## Today’s Highlights  
The 0.143.0 CLI release landed with a crippling regression: tool invocations (e.g., `exec_command`, `shell_command`) are being sent with duplicated names like `exec_commandexec_command`, breaking shell execution across platforms. Alpha `0.144.0-alpha.2` was cut overnight, likely targeting this. Meanwhile, the epic SSD‑wear bug (#28224) that could write **~640 TB/year** was finally tamed with an 85% reduction, a huge relief for power users.

## Releases  
- **rust‑v0.144.0‑alpha.2** (`0.144.0-alpha.2`) – Second alpha in a fast follow‑up cycle; no detailed changelog, but the community expects the tool‑call duplication fix.  
- **rust‑v0.144.0‑alpha.1** (`0.144.0-alpha.1`) – First spin of the same train.

## Hot Issues (10 noteworthy items)

1. **#28224 – SQLite feedback logs can consume 640 TB/year and wear out SSDs**  
   Massive write amplification from internal feedback logs. After three PRs, 85% of writes were eliminated. Closed by the reporter, though some follow‑up discussion continues. (142 comments, 427 👍)  
   [openai/codex#28224](https://github.com/openai/codex/issues/28224)

2. **#8745 – Built‑in LSP integration (auto‑detect + auto‑install) for Codex CLI**  
   The most‑requested enhancement: have the CLI automatically set up and use language servers to improve code‑intelligence without manual configuration. (55 comments, 407 👍)  
   [openai/codex#8745](https://github.com/openai/codex/issues/8745)

3. **#31665 – GPT‑5.5 tool calls fail with `unsupported call: exec_commandexec_command`**  
   The headline regression in 0.143.0: the model sends a self‑referential `namespace` that doubles the tool name, bricking shell access. Breaking macOS/Linux/WSL users. (7 comments, 6 👍)  
   [openai/codex#31665](https://github.com/openai/codex/issues/31665)

4. **#31520 – Update command fails with missing package/release asset error**  
   Users trying to update the CLI via the official `curl` script hit “Could not find Codex package …” errors. Community workaround: manual download. (12 comments, 24 👍)  
   [openai/codex#31520](https://github.com/openai/codex/issues/31520)

5. **#31611 – `unsupported call: exec_command` on Amazon Linux 2023**  
   Even basic commands are rejected on Amazon Linux 2023 with 0.143.0. Downgrading to 0.140.0 restores functionality. (8 comments, 5 👍)  
   [openai/codex#31611](https://github.com/openai/codex/issues/31611)

6. **#20851 – First‑class Computer Use support from the CLI**  
   Computer Use is currently locked behind the desktop app; many want a native CLI capability that can orchestrate local/remote GUIs programmatically. (9 comments, 12 👍)  
   [openai/codex#20851](https://github.com/openai/codex/issues/20851)

7. **#19758 – Topic‑based memory directory with agent‑initiated writes**  
   A proposal for a scalable, non‑monolithic memory model inspired by Claude Code’s `memdir` and `oh‑my‑codex` patterns, paired with a `/memory` slash command. (8 comments, 0 👍 – but detailed design)  
   [openai/codex#19758](https://github.com/openai/codex/issues/19758)

8. **#9615 – VS Code extension UI goes completely blank on Windows**  
   Long‑standing rendering glitch that makes the extension unusable for some Business‑plan users. (12 comments, 8 👍)  
   [openai/codex#9615](https://github.com/openai/codex/issues/9615)

9. **#31676 – Windows Desktop UI freezes after typing a prompt**  
   Application hang (“Top level window is idle”) on Windows 11 with latest desktop build, blocking all interaction. (2 comments, 0 👍)  
   [openai/codex#31676](https://github.com/openai/codex/issues/31676)

10. **#28969 – Setting to disable auto‑resolve after 60‑second questions**  
    Users want control over the CLI’s automatic 60‑second timeout on clarifying questions; currently it forces a resolution that may not be desired. (13 comments, 92 👍)  
    [openai/codex#28969](https://github.com/openai/codex/issues/28969)

## Key PR Progress (10 notable pull requests)

1. **#31712 – Auto‑trust hooks from eligible remote plugins**  
   Automatically writes trust hashes for authenticated user/workspace plugins, streamlining the safe plugin setup flow.  
   [openai/codex#31712](https://github.com/openai/codex/pull/31712)

2. **#31566 – perf(skills): reuse walk inventory for host loading**  
   Reduces remote environment startup latency by avoiding hundreds of recursive metadata requests; one walk now serves all.  
   [openai/codex#31566](https://github.com/openai/codex/pull/31566)

3. **#31596 – Use the image generation extension by default**  
   Consolidates image generation on a single, extension‑backed path while keeping a compatibility toggle.  
   [openai/codex#31596](https://github.com/openai/codex/pull/31596)

4. **#31361 – model‑provider: route model discovery through HTTP client factory**  
   Ensures model catalog refresh respects the same `respect_system_proxy` setting as other API traffic.  
   [openai/codex#31361](https://github.com/openai/codex/pull/31361)

5. **#31176 – Retry goals after model capacity errors**  
   Goals no longer halt on transient capacity failures; retries are free (no token cost) and prevent user intervention.  
   [openai/codex#31176](https://github.com/openai/codex/pull/31176)

6. **#31254 – Mention alternative installer options after rate limits**  
   When the standalone installer hits GitHub rate limits, users are pointed toward other CLI installation methods.  
   [openai/codex#31254](https://github.com/openai/codex/pull/31254)

7. **#30131 – feat(state): add thread_history sqlite and initial migration**  
   Lays the database groundwork for paginated local thread history, a foundational step toward better session memory.  
   [openai/codex#30131](https://github.com/openai/codex/pull/30131)

8. **#31694 – Allow plugin installs for backend dependency IDs**  
   Permits the agent to resolve and install plugins using exact backend IDs, even when they’re not in the recommended list.  
   [openai/codex#31694](https://github.com/openai/codex/pull/31694)

9. **#31686 – [codex‑apps] Filter optional file fields by tool schema**  
   Stops sending unused `mime_type`/`file_name` fields to MCP tools that don’t declare them, tightening compatibility.  
   [openai/codex#31686](https://github.com/openai/codex/pull/31686)

10. **#31690 – exec‑server: trace RPC notifications**  
    Brings the `initialized` handshake into exec‑server tracing so connection setup is visible in OTEL waterfalls.  
    [openai/codex#31690](https://github.com/openai/codex/pull/31690)

## Feature Request Trends  
- **LSP everywhere** – The community overwhelmingly wants automatic language‑server discovery and installation in the CLI.  
- **Memory architecture evolution** – Moving from a single `memory_summary.md` to a topic‑based directory with agent‑driven writes is a recurring design request.  
- **Computer Use as a CLI primitive** – Developers want the desktop Computer Use tool to be a first‑class CLI command for headless automation.  
- **More user control** – Ability to disable the auto‑resolve timer on clarifying questions and finer‑grained plugin trust management rank high.  
- **Session & history durability** – Better handling of conversation history across restarts, especially in the desktop app.

## Developer Pain Points  
1. **Tool‑call name corruption (0.143.0 regression)** – Shell commands fail with nonsensical names (`exec_commandexec_command`) across macOS, Linux, and Windows. This is the #1 blocker today.  
2. **Update/install fragility** – The `curl`‑based updater breaks with unclear asset errors, and Homebrew cask lacks the standalone layout needed for `remote‑control`.  
3. **Resource over‑consumption** – Although the SSD‑wear bug is resolved, the incident highlighted how hidden logging can silently degrade hardware.  
4. **Cross‑IDE parity gaps** – JetBrains extensions are missing critical CLI features (`/plan`, `/goal`, etc.), creating a fragmented workflow.  
5. **Windows‑specific sandbox and path issues** – Restricted‑profile sandboxes trip over false “filename too long” errors, and elevated sandbox setup resolves binaries incorrectly.  
6. **Remote session routing** – Forking a worktree often groups the new thread under the wrong project in the sidebar when working with remote workspaces.

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-07-09

## Today’s Highlights
The Gemini CLI delivered both a stable **v0.50.0** release and a **v0.51.0‑preview** today – the stable adds a tool registry and fixes CI verification, while the preview preps the next cycle. The community’s attention is fixed on subagent reliability, especially a high‑engagement bug where broken agents deceptively report success. Several new PRs also tackle serious supply‑chain and workspace‑RCE vulnerabilities.

## Releases

**[v0.50.0](https://github.com/google-gemini/gemini-cli/releases/tag/v0.50.0)** – Stable  
- Fixed `npm ci` ignoring scripts during release verification  
- Prevented workspace binary shadowing in CI  
- Introduced a **tool registry** (feature)

**[v0.51.0‑preview.0](https://github.com/google-gemini/gemini-cli/releases/tag/v0.51.0-preview.0)** – Preview  
- Changelog generation for v0.50.0‑preview.1  
- Fixed a no_proxy test  
- Bumped version to v0.51.0‑nightly for upcoming nightly builds

## Hot Issues

1. [#22323 – Subagent recovery after MAX_TURNS falsely reports GOAL success](https://github.com/google-gemini/gemini-cli/issues/22323)  
   *10 comments, 👍2* – Agent silently stops without finishing work; undermines trust in autonomous runs.

2. [#21409 – Generalist agent hangs forever on simple tasks](https://github.com/google-gemini/gemini-cli/issues/21409)  
   *7 comments, 👍8* – High‑friction bug that forces users to avoid sub‑agents entirely.

3. [#24353 – Robust component‑level evaluations](https://github.com/google-gemini/gemini-cli/issues/24353)  
   *7 comments* – EPIC to build a full behavioral‑eval framework; key to quality at scale.

4. [#22745 – Assess AST‑aware file reads, search, and mapping](https://github.com/google-gemini/gemini-cli/issues/22745)  
   *7 comments, 👍1* – Investigation into semantic tooling for more accurate code understanding.

5. [#21968 – Gemini does not use skills and sub‑agents enough](https://github.com/google-gemini/gemini-cli/issues/21968)  
   *6 comments* – Model rarely invokes custom skills unless explicitly told; blocks personalisation.

6. [#25166 – Shell command execution gets stuck with “Waiting input” after completion](https://github.com/google-gemini/gemini-cli/issues/25166)  
   *4 comments, 👍3* – Common session‑blocking issue on simple shell invocations.

7. [#26522 – Stop Auto Memory from retrying low‑signal sessions indefinitely](https://github.com/google-gemini/gemini-cli/issues/26522)  
   *5 comments* – Memory system churns on unhelpful transcripts; needs efficiency and safety improvements.

8. [#21983 – Browser subagent fails on Wayland](https://github.com/google-gemini/gemini-cli/issues/21983)  
   *4 comments, 👍1* – Linux desktop users blocked from using browser agents.

9. [#24246 – Gemini CLI errors with >128 tools (400 response)](https://github.com/google-gemini/gemini-cli/issues/24246)  
   *3 comments* – Limits large setups with many MCP servers or custom tools.

10. [#28321 – “Problem reached limits” – rate‑limit and responsiveness issues](https://github.com/google-gemini/gemini-cli/issues/28321)  
    *3 comments* – New user report of hitting limits quickly, pointing to potential throttling or quota visibility gaps.

## Key PR Progress

1. [#28328 – Don’t flag non‑auth `401` substrings as authentication errors](https://github.com/google-gemini/gemini-cli/pull/28328)  
   Prevents false OAuth fallback on URLs like `localhost:4012` or error codes containing `401`.

2. [#28327 – Only percent‑decode `file://` URLs in `resolveToRealPath`](https://github.com/google-gemini/gemini-cli/pull/28327)  
   Fixes file‑name corruption for paths with legitimate `%` sequences (e.g. `report%202026.txt`).

3. [#28126 – Show ellipsis on multi‑line edit snippets](https://github.com/google-gemini/gemini-cli/pull/28126) (merged)  
   UI polish: now clearly indicates truncated edit descriptions in the tool output.

4. [#28232 – Fix supply‑chain RCE in eval workflow](https://github.com/google-gemini/gemini-cli/pull/28232)  
   Splits `pull_request_target` workflow to stop fork code from accessing secrets.

5. [#28319 – Enforce workspace trust during A2A server environment loading (RCE fix)](https://github.com/google-gemini/gemini-cli/pull/28319)  
   Critical zero‑click RCE prevention in untrusted workspaces.

6. [#28164 – Limit recursive reasoning turns per user request](https://github.com/google-gemini/gemini-cli/pull/28164)  
   Caps CPU/credit burning infinite loops (default 15 turns, configurable).

7. [#28316 – Ensure task cancellation aborts the A2A execution loop](https://github.com/google-gemini/gemini-cli/pull/28316)  
   Eliminates “ghost executions” after cancelling an agent mode task.

8. [#28223 – Bypass LLM correction for JSON and IPYNB files in `write_file`/`replace`](https://github.com/google-gemini/gemini-cli/pull/28223)  
   Stops corruption of Jupyter notebooks and JSON files by skipping destructive LLM “correction” passes.

9. [#28310 – Remove trailing period from Antigravity URL in auth errors](https://github.com/google-gemini/gemini-cli/pull/28310)  
   One‑character fix that prevents broken sign‑in links.

10. [#28309 – Improve CJK text wrapping and `__bold__` rendering](https://github.com/google-gemini/gemini-cli/pull/28309)  
    Better terminal Markdown rendering for Chinese/Japanese/Korean and bold syntax.

## Feature Request Trends
Developers are asking for **smarter sub‑agent orchestration** (accurate termination reasons, better default usage of skills), **AST‑aware codebase tools** for precise reads and mapping, and **comprehensive component‑level evals**. There’s also a clear push for **more resilient browser agents**, **self‑awareness/documentation** so the CLI can explain its own flags, and **safer memory systems** that stop low‑signal retries and surface invalid patches.

## Developer Pain Points
The biggest recurring frustrations centre on **sub‑agent reliability** – silent hangs, false success reports, and not invoking skills unless explicitly told. **Shell command freezes** (stuck on “Waiting input”) and **browser agent failures on Linux/Wayland** remain common. The **Auto Memory system** generates noise, retries hopeless transcripts, and has gaps in secret redaction. Other sharp edges include **file corruption after external editors**, **terminal resize flicker**, **400 errors when too many tools are available**, and **missing sub‑agent context in bug reports**, making debugging harder.

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI Community Digest — 2026-07-09

## Today’s Highlights
No new releases landed in the past 24 hours, but the issue tracker remained active with a strong feature‑request pulse and a handful of newly reported regressions. The most talked‑about item was the long‑standing request for custom slash commands; with 99 thumbs‑up and 32 comments, it’s clear this is a top community priority. Meanwhile, a fresh bug report about the `model` field in `settings.json` being ignored on startup signals a potential configuration regression that may trip up users who rely on custom model defaults.

---

## Releases
*No new releases in the last 24 hours.*  

---

## Hot Issues (10 noteworthy items)

1. **[#618 – Feature Request: Support custom slash commands from .github/prompts](https://github.com/github/copilot-cli/issues/618)**  
   *99 👍, 32 comments*  
   The community wants Copilot CLI to load prompt files from `.github/prompts/`, mirroring the VS Code extension. This would let teams ship project‑specific slash commands. The overwhelming upvotes and discussion show this is one of the most anticipated ergonomic improvements.

2. **[#970 – macOS Gatekeeper blocks Copilot after Homebrew upgrade](https://github.com/github/copilot-cli/issues/970)**  
   *21 👍, 6 comments*  
   Under corporate security policies, every Homebrew upgrade triggers “Apple could not verify…” warnings, forcing manual Privacy & Security approvals. A persistent friction point for developers in managed macOS environments.

3. **[#2792 – Automatic model switching for planning vs. execution](https://github.com/github/copilot-cli/issues/2792)**  
   *14 👍, 4 comments*  
   Users want Copilot to use one model (e.g., a reasoning‑heavy one) for planning and automatically switch to a different (cheaper/faster) model for execution. This would optimise both quality and token costs.

4. **[#3158 – Plan→Compact→Re‑Plan infinite loop (217 cycles, zero execution)](https://github.com/github/copilot-cli/issues/3158)** ❌ *Closed*  
   *4 comments*  
   A high‑severity bug where auto‑compaction caused the agent to re‑plan instead of executing, burning entire sessions with no output. This was reported multiple times (see #3144–#3157) and has now been closed — likely resolved in a recent update. A notable reliability fix.

5. **[#2729 – /delegate ignores specified source branch](https://github.com/github/copilot-cli/issues/2729)** ❌ *Closed*  
   *4 comments, 2 👍*  
   The `/delegate` command disregarded the user’s branch instructions, working from `main` instead of the requested branch. The closure indicates a fix, closing a gap in automation workflows.

6. **[#3586 – Copy stops working since 1.0.49 on Linux](https://github.com/github/copilot-cli/issues/3586)** ❌ *Closed*  
   *2 comments, 1 👍*  
   A regression in the terminal UI broke copy/paste; worked on 1.0.48 but not later versions. The fix (now closed) restores basic interactivity for Linux users.

7. **[#2112 – Stale keytar entries cause repeated OAuth popups for HTTP MCP servers](https://github.com/github/copilot-cli/issues/2112)**  
   *1 comment, 1 👍*  
   Expired tokens in the OS keychain (via keytar) force browser re‑authentication on every launch, even though valid refresh tokens exist on disk. This hits developers who use HTTP MCP servers and erodes trust in the “just works” experience.

8. **[#4053 – TUI hangs at ‘Loading: N skills’ on NFS/GPFS filesystems](https://github.com/github/copilot-cli/issues/4053)**  
   *1 comment, 0 👍* *New*  
   A SIGCHLD race condition when Tokio spawns `which gh` with high concurrency causes the CLI to hang indefinitely on networked home directories. Critical for enterprise environments with shared filesystems.

9. **[#4016 – BYOK regressed: `--acp --stdio` rejects custom provider with ‘Authentication required’](https://github.com/github/copilot-cli/issues/4016)**  
   *2 👍, 1 comment*  
   Custom provider setups (COPILOT_PROVIDER_*) working in prompt mode are blocked in agent‑control mode. A regression from 1.0.61‑1.0.68 affecting enterprises that rely on bring‑your‑own‑key configurations.

10. **[#4068 – Allow specifying a model *family* (e.g., `opus`) that resolves to latest stable version](https://github.com/github/copilot-cli/issues/4068)**  
    *0 👍, 0 comments* *New*  
    Users want to pin a family alias like `opus` instead of an exact version (`claude-opus-4.8`), so they always get the latest stable model without manual config edits. Part of a growing desire for smarter model management.

---

## Key PR Progress
*No pull requests were updated in the last 24 hours.*  

---

## Feature Request Trends
- **Project‑specific slash commands** – The runaway hit (#618): loading custom prompts from a repository’s `.github/prompts/` directory, aligning CLI with the VS Code experience.  
- **Model orchestration** – Automatic switching between planning and execution models (#2792) and model *family* aliases (#4068) point to a desire for “set‑and‑forget” model policies.  
- **Session exit hints** – A request (#4066) to make the resume hint configurable, allowing users to see a friendly name (e.g., “rename‑me”) instead of an opaque session ID.  
- **Extended‑context pricing visibility** – Issue #4059 asks for a way to navigate pricing details for models with large context windows, suggesting users want cost awareness inside the CLI.

---

## Developer Pain Points
1. **Model configuration reliability** — Two open issues (#4067, #4059) show that model selection and pricing display aren’t applying correctly, undermining user trust in `settings.json`.  
2. **Enterprise onboarding & authentication** — Gatekeeper blocks (#970), stale keychain tokens (#2112), and BYOK regressions (#4016) create a recurring “death by a thousand cuts” for corporate devs.  
3. **Session management gaps** — `/resume` is broken for non‑git directories (#4054) and the exit hint isn’t customizable (#4066), making session recovery harder than it should be.  
4. **Filesystem‑sensitive hangs** — The TUI hang on NFS/GPFS (#4053) highlights fragile assumptions about local storage; a blocker for lab and HPC environments.  
5. **Cleanup after upgrades** — Old CLI versions are consuming gigabytes of disk space (#1624), with no automatic cleanup mechanism.  
6. **Agent reasoning loops** — Although the infinite plan‑compact‑re‑plan bug (#3158 and duplicates) is resolved, it reflects a deeper concern about context memory and agent determinism that the team will need to continuously harden.

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI Community Digest — 2026-07-09

## 1. Today’s Highlights
Activity remains light with no new releases or pull requests. The only update in the past 24 hours is an enhancement request proposing a flag to bypass SSL certificate validation, a need driven by corporate environments using TLS inspection proxies that break the CLI login flow.

## 2. Releases
*(No new releases in the last 24 hours.)*

## 3. Hot Issues

1. **#2458 [ENHANCEMENT] Add option to ignore SSL certificate**  
   *dmorsin opened Jun 17, last activity Jul 8 · 2 comments · 👍 0*  
   The reporter’s organization forces a man-in-the-middle anti-virus SSL proxy, which causes `kimi login` to fail because the expected certificate is replaced by the AV’s own cert. A simple `--insecure` or `--ignore-ssl` flag would allow developers in tightly managed corporate networks to authenticate. The discussion confirms this is a specific, practical workaround rather than a general security recommendation.  
   🔗 [MoonshotAI/kimi-cli#2458](https://github.com/MoonshotAI/kimi-cli/issues/2458)

*(Only one issue was updated in the last 24 hours. No other recently active issues.)*

## 4. Key PR Progress
No pull requests have been opened or updated in the last 24 hours.

## 5. Feature Request Trends
Based on recent activity (limited to #2458), the most-requested direction is providing **override options for network trust settings** – specifically, allowing developers behind corporate TLS-inspecting proxies to temporarily disable certificate validation during login. This aligns with common CLI patterns (`--insecure`, `--no-verify`) and would remove a friction point for enterprise adoption.

## 6. Developer Pain Points
The primary recurring frustration observed is **SSL certificate mismatch during authentication in controlled corporate environments**. When an organization’s security software substitutes certificates, the CLI’s strict validation renders login impossible. A configurable trust override (with appropriate safety warnings) is the immediate ask, and similar requests may appear if the tool’s user base expands within enterprise settings.

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-07-09

## 1. Today’s Highlights
The community is heavily focused on hardening the OpenCode runtime and improving cross-platform developer experience, with a flurry of bug-fixing PRs targeting UI polish, caching reliability, and symlink safety. No new release was shipped today, but the PR queue reveals active work on session input unification, provider startup resilience, and long-requested bidirectional pagination.

## 2. Releases
No new releases in the last 24 hours.

## 3. Hot Issues
*Picked from the top issues updated today; all are now closed unless noted.*

1. **#22100 – Why is OpenCode running pip3 in a read‑only config?**  
   A user discovered that OpenCode v1.4.3 triggered `pip` installs despite a permissive read‑only configuration, raising concerns about dependency trust. High engagement (11 comments) from developers who want stronger sandboxing guarantees.  
   🔗 [anomalyco/opencode#22100](https://github.com/anomalyco/opencode/issues/22100)

2. **#20045 – edit permission: relative vs absolute path mismatch**  
   Agent‑level `edit` permissions using absolute patterns silently failed because internal path formats differed from `external_directory`. The 10‑comment discussion revealed that many users rely on absolute paths in YAML frontmatter and were caught by this inconsistency.  
   🔗 [anomalyco/opencode#20045](https://github.com/anomalyco/opencode/issues/20045)

3. **#14465 – Workspace name/icon not persisted across restarts**  
   Desktop users reported that workspace details (name, icon, color) vanished after a full app restart. With 8 comments and a 👍, it highlights the friction of re‑customising workspaces every session.  
   🔗 [anomalyco/opencode#14465](https://github.com/anomalyco/opencode/issues/14465)

4. **#14862 – Big Pickle ignores AGENTS.md directives**  
   The “Big Pickle” agent became erratic and contaminated a codebase by disregarding explicit rules in `AGENTS.md`. The 7‑comment thread reflects growing reliance on agent‑side guardrails and frustration when they silently fail.  
   🔗 [anomalyco/opencode#14862](https://github.com/anomalyco/opencode/issues/14862)

5. **#36010 – Z.AI provider docs are thin**  
   A user setting up the Z.AI GLM Coding Plan found the documentation lacking compared to Bedrock or GitLab Duo—missing MCP setup, vision routing, and cost guard details. With 4 comments, it signals demand for first‑class provider documentation.  
   🔗 [anomalyco/opencode#36010](https://github.com/anomalyco/opencode/issues/36010)

6. **#25766 – Multi‑LLM structured team debate feature request**  
   The author of Conclave, a fork that adds structured multi‑LLM debates, proposed integrating this pattern upstream. The 4‑comment thread + reference implementation suggests strong community interest in agent collaboration beyond single models.  
   🔗 [anomalyco/opencode#25766](https://github.com/anomalyco/opencode/issues/25766)

7. **#25947 – Add Omniroute provider with dynamic model discovery**  
   An Omniroute integration was requested to auto‑discover models behind an OpenAI‑compatible gateway. The 👍 count (2) and 4 comments indicate that developers managing multi‑model proxies want smoother provider onboarding.  
   🔗 [anomalyco/opencode#25947](https://github.com/anomalyco/opencode/issues/25947)

8. **#26498 – DeepSeek tool calls produce malformed JSON**  
   Reports of DeepSeek models generating optional placeholders or string‑encoded arrays in tool‑call arguments. This 4‑comment issue, while closed, points to ongoing tool‑use robustness challenges with certain model families.  
   🔗 [anomalyco/opencode#26498](https://github.com/anomalyco/opencode/issues/26498)

9. **#23903 – Web UI “Failed to send prompt. Unable to retrieve session”**  
   After crashes, the Web UI refused to resume previous sessions, forcing users to start over. With 4 comments and 1 👍, session recovery reliability remains a key concern for browser‑based workflows.  
   🔗 [anomalyco/opencode#23903](https://github.com/anomalyco/opencode/issues/23903)

10. **#26501 – Safari file picker button does nothing**  
    Clicking the file picker in Safari gave no response. The issue (3 comments, 1 👍) underscores that OpenCode’s browser experience still has gaps on non‑Chromium browsers.  
    🔗 [anomalyco/opencode#26501](https://github.com/anomalyco/opencode/issues/26501)

## 4. Key PR Progress
*Selected open PRs with notable impact; all updated today.*

1. **#35989 – Refactor: remove todo tool**  
   Completely strips the `todo` tool from V1/V2 runtimes, schemas, events, permissions, UI, and docs. A major cleanup that simplifies the tool surface.  
   🔗 [anomalyco/opencode#35989](https://github.com/anomalyco/opencode/pull/35989)

2. **#35777 – Fix: refresh stale @latest npm package cache**  
   Prevents plugins configured as `@latest` from becoming stuck on an old version when `node_modules` already exists. Closes #25293.  
   🔗 [anomalyco/opencode#35777](https://github.com/anomalyco/opencode/pull/35777)

3. **#33846 – Fix(ui): stabilize shell output outline**  
   Adjusts sub‑pixel borders and virtual‑row clipping so shell/patch card outlines render consistently across devices, eliminating 0.5px visual glitches.  
   🔗 [anomalyco/opencode#33846](https://github.com/anomalyco/opencode/pull/33846)

4. **#36014 – Fix(runtime): upgrade Bun to fix NAPI crash on exit**  
   Bumps Bun to a canary build to resolve a cross‑platform NAPI crash that occurred during garbage collection at exit (closes #28046, #31563).  
   🔗 [anomalyco/opencode#36014](https://github.com/anomalyco/opencode/pull/36014)

5. **#8535 – Feat(session): bidirectional cursor‑based pagination**  
   Adds forward/backward cursor pagination for session messages across server, TUI, and experimental H surfaces. A long‑standing request (closes #6548, #28257, #30587).  
   🔗 [anomalyco/opencode#8535](https://github.com/anomalyco/opencode/pull/8535)

6. **#35555 – Fix(desktop): reveal scrollbar in settings panels**  
   Ensures scrollbars are visible for the General, Keybinds, Providers, and Models panels so that content below the fold is discoverable even without a scroll wheel.  
   🔗 [anomalyco/opencode#35555](https://github.com/anomalyco/opencode/pull/35555)

7. **#35311 – Fix(core): multiple clones of same repo treated as different projects**  
   Changes how project identity is resolved so that clones of the same repository are recognised as the same logical project, fixing a raft of related issues (#17940, #19348, etc.).  
   🔗 [anomalyco/opencode#35311](https://github.com/anomalyco/opencode/pull/35311)

8. **#36008 – Fix(core): restore explore shell access**  
   Gives the built‑in Explore agent permission to use the renamed V2 `shell` action, regaining V1 parity with a regression test for `git status` permissions.  
   🔗 [anomalyco/opencode#36008](https://github.com/anomalyco/opencode/pull/36008)

9. **#36003 – Fix(models): fall back when catalog refresh stalls**  
   Prevents model catalog loading from blocking startup when the remote fetch or cache lock hangs; returns cached/fallback data immediately. Closes #35294.  
   🔗 [anomalyco/opencode#36003](https://github.com/anomalyco/opencode/pull/36003)

10. **#35999 – Fix(session): separate active context tokens from usage totals**  
    Stops the context meter from mixing active context size with cumulative cache‑read totals, so large cache reads no longer inflate the displayed token count (closes #30649, #34712).  
    🔗 [anomalyco/opencode#35999](https://github.com/anomalyco/opencode/pull/35999)

## 5. Feature Request Trends
- **Provider ecosystem expansion** – Requests for Z.AI, Omniroute, and easier MCP server configuration show that users want more out‑of‑the‑box provider choices and streamlined onboarding.  
- **Multi‑agent & orchestration** – Ideas like multi‑LLM team debates and runtime model overrides for subagents point to a desire for more sophisticated agent collaboration, beyond single‑model sessions.  
- **Unconventional interfaces & bridges** – A QQ bot bridge proposal hints at appetite for integrating OpenCode into messaging platforms, making AI‑assisted development accessible outside traditional IDEs.  
- **Desktop ergonomics** – Quick MCP server panels, persistent workspace customisation, and reliable file pickers are recurring quality‑of‑life requests from desktop app users.

## 6. Developer Pain Points
- **Permission & path inconsistencies** – Relative vs absolute path handling (#20045), overly permissive `pip` invocations (#22100), and UNC path failures on WSL (#26481) create trust and usability gaps.  
- **Session and workspace state loss** – Vanished workspace details (#14465), prompt‑history bloat (#36000), and Web UI “unable to retrieve session” errors (#23903) break developer flow after restarts or crashes.  
- **Provider configuration friction** – Thin docs for niche providers (#36010), missing fallback when catalogs stall (#36003), and DeepSeek‑specific JSON malformation (#26498) add overhead for multi‑model setups.  
- **Cross‑environment quirks** – Safari file picker (#26501), VS Code keyboard shortcut interception (#26519), and ampersand‑to‑symbol auto‑conversion (#26285) point to edge‑case brittleness that impacts daily use.

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi Community Digest – 2026-07-09

## Today’s Highlights
A wave of stability fixes landed on the `earendil-works/pi` repo today with no new release tagged in the last 24 hours. The most impactful changes address long‑standing clipboard struggles on Linux, a compaction bug that broke entire OpenAI sessions after auto‑compaction, and a regression that silently crashed DeepSeek V4 thinking sessions. Feature work continues toward richer session metadata, prompt‑cache observability, and better editor extension keybinding handling.

## Releases
No releases were published in the last 24 hours.

## Hot Issues
*(Issues that attracted notable community attention or expose critical functionality gaps)*

1. **#5700 – Support multiple live agent sessions with TUI switching** (9 💬, 0 👍) [CLOSED]  
   Requests the ability to run several concurrent agent sessions and switch between them without tearing down the current session. This reflects a strong power‑user desire for background‑task workflows.  
   🔗 [earendil-works/pi#5700](https://github.com/earendil-works/pi/issues/5700)

2. **#5263 – Make in‑session model and thinking‑level changes ephemeral by default** (5 💬, 6 👍) [OPEN]  
   Proposes that model/thinking‑level changes during a session only affect that session, with a separate “Default model” setting for global changes. High community approval (6 thumbs up) signals frustration with accidental global overrides.  
   🔗 [earendil-works/pi#5263](https://github.com/earendil-works/pi/issues/5263)

3. **#5886 – AgentSession settlement/continuation & assistant‑tail lifecycle bugs** (4 💬, 2 👍) [OPEN]  
   A meta‑issue tracking recurring post‑run continuation failures where the agent tries to continue from an invalid transcript state. Developers note this is a systemic problem needing a larger fix.  
   🔗 [earendil-works/pi#5886](https://github.com/earendil-works/pi/issues/5886)

4. **#6250 – Ctrl+V image paste silently fails on Linux/X11 in Bun release binary** (2 💬, 0 👍) [CLOSED]  
   Native clipboard bindings broke in Bun builds since v0.80.3, making image paste completely non‑functional on Linux. Worked in 0.78.0; a serious usability regression for Linux users.  
   🔗 [earendil-works/pi#6250](https://github.com/earendil-works/pi/issues/6250)

5. **#6414 – streamProxy drops ToolCall.thoughtSignature – Gemini multi‑turn tool calls 400 through proxy** (3 💬, 0 👍) [CLOSED]  
   Any Gemini session that makes a tool call fails on the second turn with a missing thought signature error when routed through a proxy using `streamProxy`. Blocks proxy‑based setups entirely for Gemini.  
   🔗 [earendil-works/pi#6414](https://github.com/earendil-works/pi/issues/6414)

6. **#6429 – OpenAI Responses sends `max_output_tokens=1` after compaction** (2 💬, 0 👍) [CLOSED]  
   After auto‑compaction of a long `gpt-5.5` session, subsequent OpenAI Responses requests always fail with a validation error. This bricked sessions until the user resets them manually.  
   🔗 [earendil-works/pi#6429](https://github.com/earendil-works/pi/issues/6429)

7. **#6373 – Clipboard images not sent to LLM, and no support for remote model inference** (2 💬, 0 👍) [CLOSED]  
   Pasted images are written to a temp file but some models ignore the file path; additionally, remote models that require an image‑URL approach can’t be used. Hinders multimodal workflows.  
   🔗 [earendil-works/pi#6373](https://github.com/earendil-works/pi/issues/6373)

8. **#6303 – Exponential retry backoff has no cap despite `maxRetryDelayMs` existing** (2 💬, 1 👍) [OPEN]  
   The retry logic exponentiates delay without an upper bound; attempt 7 alone can wait ~4 minutes. The configuration entry `retry.provider.maxRetryDelayMs` exists but is never consumed.  
   🔗 [earendil-works/pi#6303](https://github.com/earendil-works/pi/issues/6303)

9. **#6321 – `/fork` spawns one extra session per Enter while fork is running** (2 💬, 0 👍) [CLOSED]  
   Rapidly pressing Enter in the fork selector could create duplicate sessions because the UI didn’t lock the selection before the asynchronous fork completed. A classic race‑condition UX annoyance.  
   🔗 [earendil-works/pi#6321](https://github.com/earendil-works/pi/issues/6321)

10. **#6433 – DeepSeek V4 + thinking mode crashes session in v0.80.3 – regression from v0.79.x** (1 💬, 1 👍) [CLOSED]  
    Using DeepSeek V4 with thinking level above `low` silently exits the TUI. The root cause was `reasoning_content` not being preserved during tool‑call history replay, a regression that broke a previously working setup.  
    🔗 [earendil-works/pi#6433](https://github.com/earendil-works/pi/issues/6433)

## Key PR Progress
*(All PRs active or merged in the last 24 hours)*

1. **#6440 – fix: reload keybindings before creating custom editor component** [MERGED]  
   Custom editor components (e.g., `pi-powerline-footer`) now correctly pick up user‑defined keybindings at session start without requiring a `/reload`.  
   🔗 [earendil-works/pi#6440](https://github.com/earendil-works/pi/pull/6440)

2. **#6437 – fix(ai): update Copilot extended context windows** [MERGED]  
   Updates GitHub Copilot model metadata to reflect the 1‑million‑token context window announced by GitHub on June 4, 2026.  
   🔗 [earendil-works/pi#6437](https://github.com/earendil-works/pi/pull/6437)

3. **#6436 – fix(ai): hide responses reasoning comment markers** [MERGED]  
   Strips `<!-- -->` separators from OpenAI Responses reasoning displays while preserving the signed reasoning item for replay; also adds regression tests.  
   🔗 [earendil-works/pi#6436](https://github.com/earendil-works/pi/pull/6436)

4. **#6427 – feat(coding-agent): add prompt cache miss tracking** [OPEN]  
   Detects prompt cache misses per turn and emits a warning‑colored transcript notice when idle gaps or model switches invalidate the cache. Useful for cost/performance tuning.  
   🔗 [earendil-works/pi#6427](https://github.com/earendil-works/pi/pull/6427)

5. **#6430 – fix fork menu allowing user to double select an entry** [MERGED]  
   Closes the fork selection menu before starting the fork process, preventing accidental creation of multiple session forks.  
   🔗 [earendil-works/pi#6430](https://github.com/earendil-works/pi/pull/6430)

6. **#6418 – Fix native clipboard in bun release** [MERGED]  
   Copies the native addon files into the Bun package and adds an `xclip` fallback on X11, restoring image paste on Linux. Fixes #6250.  
   🔗 [earendil-works/pi#6418](https://github.com/earendil-works/pi/pull/6418)

7. **#6417 – feat(agent): support custom metadata in jsonl session headers** [MERGED]  
   Extends the new harness module’s JSONL storage with an optional `metadata` field, enabling richer session provenance tracking.  
   🔗 [earendil-works/pi#6417](https://github.com/earendil-works/pi/pull/6417)

8. **#6413 – feat(coding-agent): show git info in local version** [MERGED]  
   Displays current branch, commit hash, and dirty state in the agent’s local version string, making it easier to correlate behaviour with code state.  
   🔗 [earendil-works/pi#6413](https://github.com/earendil-works/pi/pull/6413)

## Feature Request Trends
Across the 43 issues updated today, several feature directions stand out:

- **Multi‑session & ephemeral configuration** – Users want to run concurrent sessions (#5700) and have model/thinking changes apply only to the active session (#5263), moving away from global side‑effects.
- **Extensible session metadata & hooks** – Adding custom metadata to session headers (#6402) and exposing an in‑memory session store (#6435) point to a desire for better programmatic session management. An extension hook *before* the session file is loaded (#6428) would close a lifecycle gap.
- **Compaction intelligence** – Ideas include pre‑compacting when switching to a smaller‑context model (#6426), chunking large summary calls (#6425), and avoiding compaction leaving unfinished work idle (#6424).
- **Broader provider/model support** – Adding new built‑in providers (Novita AI, #6420) and fixing ghost model entries (MiMo token plan, #6204) remain active directions.
- **Observability** – Prompt cache miss tracking (#6427) and git info in version strings (#6413) reflect a push to make the agent’s runtime behaviour more transparent.

## Developer Pain Points
Recurring frustrations and high‑frequency issues:

- **Clipboard reliability** – Linux image paste has been broken since v0.80.3 (#6250), and even when images are pasted, they aren’t sent correctly to some models (#6373). This cripples visual workflows.
- **Session lifecycle instability** – Post‑run continuation logic fails with invalid transcript states (#5886), and the DeepSeek V4 regression (#6433) silently terminates the TUI. Users can’t trust long‑running sessions.
- **Compaction fragility** – A single bug can cause every subsequent request to fail with a bad parameter (#6429), and large sessions make the summary request itself the point of failure (#6425).
- **Retry & network resilience** – Exponential backoff can wait many minutes without a cap (#6303), and transient socket drops on Bun aren’t classified as retryable (#6431), causing hard stops.
- **Provider integration edges** – Ghost model entries (#6204), missing billing markers for OAuth (#6421), and proxy‑induced Gemini tool‑call failures (#6414) force users into opaque debugging.
- **Configuration friction** – A read‑only config directory breaks all credential reads due to an unnecessary file lock (#6406); user‑customized keybindings don’t activate until a manual reload (#6440).

---

*Digest generated by a technical analyst based on the `earendil-works/pi` repository activity for 2026-07-09.*

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest – 2026-07-09

## 1. Today’s Highlights
v0.19.8 ships with daemon-level environment isolation and total admission control while a heated RFC (#6378, 19 comments) proposes multi-workspace support in a single `qwen serve` daemon. The community is also rallying around restoring direct file drag-and-drop upload in the CLI (#6560, 17 comments), indicating strong demand for a fluid multimodal workflow.

## 2. Releases
- **v0.19.8** – *Release v0.19.8*
  - docs(channels): add WeCom to channels overview (#6490)
  - feat(cli): Add serve env isolation and total admission (dououOUC)
  - Full notes: [v0.19.8](https://github.com/QwenLM/qwen-code/releases/tag/v0.19.8)

## 3. Hot Issues
1. **[#6378] RFC: Support multiple workspaces in one qwen serve daemon** (19 💬, open)  
   The most-discussed item this week. The proposal aims to let a single daemon manage multiple workspaces while preserving backward compatibility. The community is actively debating design trade-offs, isolation, and session routing.

2. **[#6560] 希望能恢复对话中直接上传、拖拽上传图片和文档的功能** (17 💬, open)  
   Users report that direct paste/drag-and-drop for images and documents in the CLI has stopped working. Many rely on this for quick visual context; the regression is generating frustration and an expectation for a fast fix.

3. **[#6565] 糟糕！连接到 Qwen Coder 时出现问题。 Internal Error** (3 💬, open)  
   Users in Japanese and Chinese locales are hitting an `Internal Error` when connecting to Qwen Coder. The issue suggests an authentication or networking hiccup with recent versions.

4. **[#6507] Deferred IDE startup can show a stale failure state after late connection success** (2 💬, open)  
   A timing issue where the IDE connection UI reports failure but later succeeds internally, leading to a misleading error state. Developers find this confusing during session setup.

5. **[#6553] ci(triage): qwen-code-action swallows stderr, making triage failures invisible** (2 💬, open)  
   The CI triage step discards stderr, so silent agent failures go unnoticed. This undermines automated issue handling and reduces trust in the bot’s output.

6. **[#6563] MCP prompt 未声明 arguments 时用户输入被静默丢弃** (1 💬, open)  
   When an MCP server prompt doesn’t declare `arguments`, user-supplied arguments are silently dropped. The fix will need explicit parameter passing logic.

7. **[#6542] Add read-only Advisor feedback loop for complex agent tasks** (1 💬, open)  
   A feature request for a second-opinion reviewer that inspects session context and returns structured guidance before/during/after major work. Designed to improve output quality on long-running tasks.

8. **[#6536] WebShell user messages show serialized @ references instead of chips** (1 💬, open)  
   After sending a prompt with composer references, the user bubble shows raw text like `@.qwen/` instead of compact reference chips, causing UI inconsistency.

9. **[#6554] Release Failed for v0.19.8-nightly.20260709.e3a247f99** (1 💬, open)  
   The nightly quality job failed, halting the nightly release. The root cause is being addressed in a follow-up format PR.

10. **[#6524] Vision bridge image interpretation times out after 30000ms** (1 💬, closed)  
    A closed but noteworthy bug: the vision bridge for DingTalk channels had a hardcoded 30-second timeout, making large image interpretation unreliable. The quick closure suggests it was resolved with urgency.

## 4. Key PR Progress
1. **[#6506] fix(cli): optimize large paste performance and add progress indicator**  
   Eliminates the 1.7s+ hang when pasting large texts (260K+ chars) by bypassing per-character events and adding a progress indicator. A major UX win for CLI users.

2. **[#6561] feat(web-shell): add a workspace Goals page, and stop losing /goal on daemon resume**  
   Surfaces `/goal` in a dedicated Web Shell page and fixes a bug where daemon resume dropped goal context. Prerequisite for a richer planning experience.

3. **[#6451] refactor(cli): rewrite Fleet View to match Claude Code agent view UI**  
   Overhauls the multi-session management UI to match Claude Code’s agent view pattern, aligning the experience with popular tools.

4. **[#6556] fix(core): clamp max_tokens to the context window; retire the output reservation**  
   Auto-compaction now correctly respects the full context window instead of using a fixed output budget per turn. Reduces premature compaction and improves token efficiency.

5. **[#6535] feat(scheduled-tasks): add isolated run mode via create_sub_session tool**  
   Introduces `create_sub_session` for cron jobs, enabling each scheduled run to have a clean context/turnectly isolated from the main session. Prevents context pollution across runs.

6. **[#6558] feat(cli): List persisted sessions for trusted workspaces**  
   Extends the daemon’s session list endpoint to return active persisted sessions from trusted non-primary workspaces, without duplicates. Important for multi-workspace UIs.

7. **[#6495] feat(channels): support webhook-triggered channel tasks**  
   Allows external webhooks to trigger agent tasks via `qwen serve`. The agent receives the event as context and the channel worker delivers the response. Opens up integration with external automation.

8. **[#6557] feat(daemon): persist session artifacts across restarts**  
   Implements V2 durable artifact metadata, allowing artifacts to survive daemon restarts and session replays. Critical for long-running agent reliability.

9. **[#6567] feat(cli): Add workspace-qualified core REST routes**  
   Adds `/workspaces/:workspace/...` routes for the daemon’s REST API, supporting both POSIX and Windows absolute paths while keeping legacy routes intact. A foundation for multi-workspace operations.

10. **[#6537] feat(web-shell): render composer references in user messages**  
    Converts serialized composer references into compact chips in user message bubbles inside WebShell, fixing the visual inconsistency reported in #6536.

## 5. Feature Request Trends
- **Multi-workspace daemon** – a single `qwen serve` handling many workspaces with isolated sessions (#6378). This is the most debated feature aspiration.
- **Rich file input** – restore direct image/document paste and drag-and-drop in the CLI (#6560).
- **Quality guardrails** – a read-only “Advisor” that reviews work before, during, and after complex tasks (#6542); also visibility for background task status in hook payloads.
- **Channel management** – dmPolicy to disable private DMs in channels (#6392, closed), webhook-triggered tasks (#6495), and payload diagnostics for adapters.
- **Configurable timeouts** – for AutoMemory extractors and vision bridge (#6308, #6524).
- **Session/memory isolation per worktree** – users want per-task memory, not shared project memory (#6449).

## 6. Developer Pain Points
- **Missing multimodal input** – inability to paste/drag images/documents in the CLI is a top complaint (#6560); previously working functionality is now absent.
- **Subagent stability** – loops with repeated tool calls (#6505) and silent failures degrade trust when subagents run unattended.
- **Session management glitches** – truncated session history due to missing parent links (#6501) and worktree sessions sharing project memory (#6449) cause confusion.
- **Installation and extension issues** – errors during install on Windows (#3845) and extension install failures (#6334) remain recurring frustrations.
- **Performance on large input** – pasting large blocks of text used to freeze the CLI (#6506, now fixed).
- **CI visibility** – triage jobs silently failing (#6553) and nightly release breakage (#6554) make development feedback loops opaque.
- **MCP sharp edges** – prompt arguments silently dropped (#6563) and environment variable leaks (#6564) need attention.

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI Community Digest — 2026-07-09

## Today’s Highlights
The v0.8.68 milestone entered a massive integration phase today, with dozens of PRs merging fleet profile canonicalization, Android/Termux support, sub-agent tool sandboxing, and a new xAI provider. The execution board ([#4092](#)) continues to coordinate all lanes, while a fresh streaming‑performance report ([#4270](#)) highlights the tension between feature velocity and rendering polish.

## Releases
No new version was published in the last 24 hours.

## Hot Issues
1. **#4092** – v0.8.68 execution board (canonical agent packet)  
   *Single entry point for every agent working on the milestone, with 55 comments.* Keeps the complex, multi-lane delivery on track.  
   [View issue](Hmbown/CodeWhale/issues/4092)

2. **#4042** – Environment-level tool sandboxing for sub-agents  
   *Phased enforcement of tool restrictions across sessions, sub-agents, and Fleet workers.* Closed today after Phase 1 landed; security-minded users applaud the guardrails.  
   [View issue](Hmbown/CodeWhale/issues/4042)

3. **#3965** – Per-sub-agent provider assignment + LM Studio support  
   *Explicitly route sub-agents to different providers (e.g., local LM Studio).* Closed through the fleet routing overhaul, unlocking true multi-model teams.  
   [View issue](Hmbown/CodeWhale/issues/3965)

4. **#4236** – Epic: official Termux / Android arm64 support  
   *Users repeatedly ask for mobile TUI access. The epic spawned a half-dozen issues and PRs, with build assets, docs, and updater logic all merged today.* High community excitement.  
   [View issue](Hmbown/CodeWhale/issues/4236)

5. **#4270** – Streaming text display is too slow (Chinese report)  
   *DeepSeek V‑flash output outpaces the terminal rendering, causing late bursts of text. Opened today and already drawing maintainer attention.* A critical perf gap for fast models.  
   [View issue](Hmbown/CodeWhale/issues/4270)

6. **#3488** – Unicode, CJK, and terminal-width rendering QA  
   *Long‑standing mixed-width text bugs that break readability. Re‑flagged as a dedicated QA lane after v0.8.68.* East‑Asian users continue to push for a fix.  
   [View issue](Hmbown/CodeWhale/issues/3488)

7. **#3875** – Guided multi-step setup wizard for built-in providers  
   *The remaining piece of the provider onboarding rework. Key‑verification logic landed today, but the full interactive wizard is still open.*  
   [View issue](Hmbown/CodeWhale/issues/3875)

8. **#4112** – Default transcript copy polish and activity wording  
   *Small but constant UX papercuts around sidebar labels and setup hints. A PR shipped today, but further tuning is expected.*  
   [View issue](Hmbown/CodeWhale/issues/4112)

9. **#4257** – Add xAI (Grok) as a first-class provider  
   *One of the top provider requests. The implementation merged today, adding API‑key and OAuth paths.*  
   [View issue](Hmbown/CodeWhale/issues/4257)

10. **#4227** – “Help JayBeest keep up with the CodeWhale tsunami”  
    *Humorous yet real contributor health request: a Skill/workflow to automate dev‑environment maintenance at the project’s 10+ PR/day velocity.*  
    [View issue](Hmbown/CodeWhale/issues/4227)

## Key PR Progress
1. **#4269** – CI: build Termux Android arm64 assets  
   Release pipeline now produces `codewhale-android-arm64` binaries.  
   [View PR](Hmbown/CodeWhale/pull/4269)

2. **#4266** – feat(provider): add xAI API‑key route  
   Full Grok provider integration with model catalog, completion, keyring, and docs.  
   [View PR](Hmbown/CodeWhale/pull/4266)

3. **#4264** – Cache command and regex hot paths  
   Static command caches and bounded LRU regex cache lift rendering/matching performance.  
   [View PR](Hmbown/CodeWhale/pull/4264)

4. **#4096** – Sub-agent tool scoping plan and Phase 1 implementation  
   Documentation‑rich phase that enforces first‑cut tool restrictions (issue #4042).  
   [View PR](Hmbown/CodeWhale/pull/4096)

5. **#4262** – fix(fleet): route AgentProfile pins through custom providers  
   Enables per‑child routing to LM Studio and other custom providers (resolves #3965).  
   [View PR](Hmbown/CodeWhale/pull/4262)

6. **#4252** – feat(tui): six‑view model picker catalog browsing  
   `/model` now toggles Configured, Catalog, Recent, Coding, Cheap, and Long‑context views.  
   [View PR](Hmbown/CodeWhale/pull/4252)

7. **#4268** – fix(provider): verify setup keys before saving  
   Provider API keys are checked against `/models` before being persisted; bad keys are rejected with a clear error.  
   [View PR](Hmbown/CodeWhale/pull/4268)

8. **#4255** – feat(catalog): Models.dev refresh/snapshot automation  
   Script to safely fetch and inspect the external catalog without overwriting local fallback data.  
   [View PR](Hmbown/CodeWhale/pull/4255)

9. **#4265** – fix(tui): polish setup and activity copy  
   Consolidates setup hint wording and finishes the `Tasks` → `Activity` sweep (issue #4112).  
   [View PR](Hmbown/CodeWhale/pull/4265)

10. **#4225** – refactor(localization): extract hardcoded texts into locale files  
    Moves scattered English strings into `locales/`, enabling broader translation support.  
    [View PR](Hmbown/CodeWhale/pull/4225)

## Feature Request Trends
- **Sub‑agent tool sandboxing & per‑agent routing** – users want fine‑grained control over which tools sub‑agents can use and which provider/model each agent talks to.  
- **Live, external‑source model catalog** – shift from hand‑curated baked‑in lists to fetching Models.dev for always‑fresh model metadata, with offline fallback.  
- **Android/Termux as a first‑class target** – growing demand for a native mobile TUI experience, driving the entire Termux epic.  
- **Guided onboarding with key validation** – wizards that not only collect credentials but verify them immediately, reducing silent failures.  
- **Rendering and startup performance** – repeated calls to speed up streaming, launch time, and UI interactions, even as new features are added at high velocity.

## Developer Pain Points
- **Streaming text lags behind model output** – especially noticeable with fast models like DeepSeek V‑flash, where text “pops in” all at once after the stream finishes.  
- **Fleet UX overload** – legacy loadout terminology and overlapping detail views created confusion; the ongoing profile‑centric refactor is starting to clean this up.  
- **Unicode/CJK rendering** – mixed‑width terminal content still breaks layout, impacting East Asian users and making full internationalization hard.  
- **High project velocity** – the rapid pace of merges (>10 PRs/day) makes it difficult for contributors to keep their local development environments in sync, as captured humorously in #4227.  
- **API key setup without verification** – before today’s changes, a mistyped key would be saved silently and lead to cryptic errors, only found later.

---

*Stay tuned for the next digest as the v0.8.68 milestone moves into polish and QA lanes.*

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*