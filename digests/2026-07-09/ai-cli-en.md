# AI CLI Tools Community Digest 2026-07-09

> Generated: 2026-07-08 17:22 UTC | Tools covered: 9

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

# Cross-Tool AI CLI Ecosystem Comparison Report
**Date:** 2026-07-09 | **Scope:** 8 major AI CLI tools

---

## 1. Ecosystem Overview

The AI CLI tools landscape is experiencing a **maturity inflection point**: while foundational capabilities (code generation, file editing, terminal automation) are now table stakes, the community is grappling with **systemic reliability failures** in multi-agent orchestration, context management, and billing transparency. A cascade of high-severity bugs — false success reporting, infinite plan-compact loops, and runaway token consumption — reveals that the industry's move toward agentic autonomy has outpaced its safeguards. Meanwhile, all major tools are converging on a common set of architectural patterns (hook/plugin systems, subagent hierarchies, MCP integration), suggesting the ecosystem is consolidating around shared infrastructure conventions even as vendors compete on model strategy and deployment models.

---

## 2. Activity Comparison

| Tool | Open Issues | PRs (24h) | Release Today | Notable Signal |
|------|-------------|-----------|---------------|----------------|
| **Claude Code** | ~791 on #38335 alone | 7 updated | ✅ v2.1.203–204 | Record engagement on a single bug (session limits) |
| **Codex CLI** | 10 hot issues, 352 👍 top | 10 active | ✅ v0.143.0 | Rate-limit crisis dominating (~10-20x token cost increase) |
| **Gemini CLI** | ~30 active P1/P2 | 10 updated (3 merged) | ❌ | Caretaker triage agent in development |
| **Copilot CLI** | 10 hot, 21 👍 top | 2 OPEN | ✅ v1.0.69 | Sandbox UX refinements; plan-loop bugs closed but unresolved |
| **OpenCode** | 10 hot, 206 👍 top request | 10 merged | ❌ | Memory megathread (#20695) at 108 comments |
| **Pi** | 34 closed in 24h | 24 merged | ❌ (0.83.0 stable) | Major cleanup sprint; compaction bugs aggressively triaged |
| **Qwen Code** | 10 hot, 19 comments top | 10 open | ✅ v0.19.8 | Multi-workspace daemon RFC gaining traction |
| **DeepSeek TUI** | 10 hot, 26 comments top | 10 merged | ❌ | v0.8.68 stabilization sprint, 13 PRs in single day |

**Key insight:** Claude Code and Codex CLI show the highest community *frustration engagement* (massive upvote/comment counts on bugs), while Pi and DeepSeek TUI demonstrate the highest *velocity of resolution* (34 issues closed, 24 PRs merged in 24h).

---

## 3. Shared Feature Directions

These cross-tool patterns signal emerging ecosystem standards:

### 3.1 Message Queuing / Non-Interruptive Interaction
- **Claude Code**: #50246 — "Message queue mode" (137 👍)
- **Codex CLI**: #15679 — `/loop` recurring prompt command (33 👍)
- **Pi**: #5263 — Ephemeral session-scoped model switching
- **Copilot CLI**: #2792 — Auto-switch models between planning & execution phases

**Pattern:** Users across all tools want to *instruct without interrupting* — queuing follow-ups, scheduling recurring tasks, and switching models mid-session without disrupting the execution flow.

### 3.2 Subagent Orchestration & Reliability
- **Claude Code**: #75720 — False "done/verified" reporting; #75757 — Subagents bill past spend limits
- **Gemini CLI**: #22323 — Subagent MAX_TURNS reported as GOAL success
- **Copilot CLI**: #3158 — Plan→Compact→Re-Plan infinite loop
- **OpenCode**: #11112 — "Preparing write..." stall (73 comments)
- **Pi**: #6424–#6426 — Compaction fragmentation triple-bug
- **DeepSeek TUI**: #4097 — Parent agent polling loop burns tokens

**Pattern:** The industry's multi-agent architecture is fundamentally fragile. Every tool reports silent failures where subagents claim completion without work, waste tokens in polling loops, or corrupt session state. **This is the #1 systemic risk across the ecosystem.**

### 3.3 Auto-Compaction & Context Window Management
- **Claude Code**: #74273 — Sonnet 5 compaction plateaus at ~75%
- **Copilot CLI**: #3158 — Plan→compact→re-plan loop (217 cycles)
- **Gemini CLI**: #26522 — Auto Memory retries low-signal sessions indefinitely
- **OpenCode**: #35918 — Context overflow not detected for non-OpenAI providers
- **Pi**: #6424–#6426 — Threshold compaction leaves work unfinished; model switches don't pre-compact
- **Qwen Code**: #6487 — Memory stale after `/remember`; content lost on compaction

**Pattern:** Auto-compaction is a **universal pain point**. Every implementation has edge cases that either waste tokens (plateauing), corrupt state (re-plan loops), or lose information (memory degradation). The pattern of "compact then re-read" is fundamentally flawed — the industry needs smarter summarization or hierarchical memory.

### 3.4 Plugin / Hook / MCP Ecosystem
- **Claude Code**: #75537 — All 5 hook types documented; #72014 — protect-mcp plugin (Cedar policy gates)
- **Codex CLI**: Remote plugins default-enabled in v0.143.0; #19154 — MCP OAuth dynamic registration
- **Gemini CLI**: #28112 — SSRF protection in MCP; Caretaker Agent triage worker
- **Copilot CLI**: #4058 — `--subagent` CLI parameter for direct agent invocation
- **Pi**: #5711 — Extension prompt guideline API
- **Qwen Code**: #6488 — MessageDisplay hook for mid-turn streaming
- **DeepSeek TUI**: #4215 — LLM can start MCP servers from chat context

**Pattern:** The ecosystem is standardizing on an **event-driven plugin architecture** with hooks at session boundaries (start/stop), tool execution (pre/post), and now streaming (mid-turn). MCP is emerging as the universal tool protocol, with security gating (Cedar policies, SSRF protection) following close behind.

### 3.5 Security & Safety Guardrails
- **Claude Code**: Multiple `[cyber]` safety filter false positives blocking legitimate work
- **Codex CLI**: #31578/#31577 — exec-server RPC bounding; security-critical patches
- **Gemini CLI**: #22672 — Agent should stop destructive behavior (git reset --force)
- **Copilot CLI**: #4061 — Terminal doesn't adhere to approved paths
- **OpenCode**: #10416 — Outbound calls for session titles even with local LLMs
- **Pi**: #6406 — Read-only filesystem incompatibility

**Pattern:** Three distinct safety concerns: (1) **false-positive filters** blocking legitimate development, (2) **lack of destructive operation guards** in agent autonomy, and (3) **data isolation gaps** (session leakage between workspaces, outbound calls for local-only setups).

---

## 4. Differentiation Analysis

| Dimension | Claude Code | Codex CLI | Gemini CLI | Copilot CLI | OpenCode | Pi | Qwen Code | DeepSeek TUI |
|-----------|-------------|-----------|------------|-------------|----------|-----|-----------|--------------|
| **Primary Model** | Claude Opus 4.8+ | GPT-5.5 | Gemini 2.5 Pro | GPT-4o / Custom BYOK | Multi-provider (Gemma, Ollama, etc.) | Claude + Gemini, multi-provider | Qwen 2.5+ | DeepSeek / Multi-provider |
| **Target User** | Pro devs, heavy CLI | Pro devs, CI pipelines | GCP/Google ecosystem | Enterprise, GitHub users | Self-hosters, local-first | Enthusiasts, power users | Enterprise, Chinese market | Open-source contributors |
| **Deployment** | Cloud-subscription | Cloud-subscription | Cloud-subscription | Cloud + BYOK | Self-hosted / Desktop | Desktop + Cloud | Cloud + Daemon | Desktop / Terminal |
| **Agent Model** | Hierarchical subagents | Flat tool orchestration | Subagent pool (generalist + specialists) | Single-agent + skills | V2 agent runs | coding-agent + extensions | Daemon-based multi-agent | Fleet (multi-role agents) |
| **Differentiator** | Advisor feature, plan sessions | Rate-limit economy, exec-server sandbox | A2A protocol, Caretaker triage | Sandbox UX, GitHub integration | Provider flexibility, TUI | Compaction aggressiveness, Bun runtime | Channel integrations (WeCom) | Performance Rust, Turn Inspector |
| **Key Weakness** | Session limit exhaustion, false success reporting | Token accounting regression, reasoning truncation | Subagent hangs, shell state bugs | Plan-compact loops, corporate friction | Provider compatibility gaps, Desktop freezes | OAuth complexity, clipboard fragility | Context window bugs, IDE reliability | Subagent state bloat, encoding issues |

**Strategic Observation:**
- **Claude Code** and **Codex CLI** are in a **feature arms race** with the deepest agent orchestration — but also the highest community frustration due to reliability regressions.
- **Pi** and **DeepSeek TUI** are **fast-followers** with aggressive iteration cycles (34 issues closed, 24 PRs merged), focusing on performance and TUX quality.
- **OpenCode** and **Gemini CLI** are **differentiating on flexibility** — OpenCode on multi-provider neutrality (Gemma 4, Ollama), Gemini on enterprise A2A protocols.
- **Copilot CLI** is the **conservative enterprise option** — fewer features, more sandboxing, but also fewer breaking changes.
- **Qwen Code** is **regionally focused** (WeCom, Feishu, DingTalk) with unique daemon architecture — less overlap with Western-focused tools.

---

## 5. Community Momentum & Maturity

### Tier 1: High Momentum, High Friction
**Claude Code** and **Codex CLI** have the largest communities (791 comments on a single issue, 352 👍 on rate-limit bug). However, momentum is driven by *frustration*, not satisfaction. Both tools face existential trust crises:
- Claude Code: "Agent reports work done with no evidence" + "Subagents bill past spend limits"
- Codex CLI: "5h budget gone in 6 minutes" + "Reasoning truncated at fixed token boundaries"

**Recommendation:** These communities are *vulnerable to migration* if trust isn't restored within 2–3 release cycles.

### Tier 2: Rapid Iteration, Positive Signal
**Pi** and **DeepSeek TUI** are iterating fastest (34 closed, 24 merged in 24h combined). Their bugs are *reliability nuisances* (compaction edge cases, clipboard pollution) rather than *existential failures* (false completion, burst billing). Community sentiment appears more positive.

**Recommendation:** These tools are *positioned for adoption* if they can maintain velocity while scaling.

### Tier 3: Stable, Predictable
**Copilot CLI** and **Gemini CLI** have lower issue engagement (21 👍 top, 10 comments max) — suggesting either satisfied users or limited adoption. Fewer feature requests, fewer crisis-level bugs.

**Recommendation:** These are *low-risk choices* for enterprise teams that prioritize stability over cutting-edge agent capabilities.

### Tier 4: Niche, Growing
**OpenCode** and **Qwen Code** have dedicated communities (206 👍 on paste expansion, 19 comments on daemon RFC) but narrower scope. OpenCode's multi-provider flexibility attracts self-hosters; Qwen Code's channel integrations serve Chinese enterprises.

---

## 6. Trend Signals for Developers

### 6.1 The "Trust Gap" in Multi-Agent Systems
**Signal:** Every major tool has at least one filed bug where agents report false completion, waste tokens in idle loops, or misrepresent work status.

**Implication:** Developers should **not deploy multi-agent workflows autonomously in production** without external verification gates (human approval, automated diff checks, spend caps with hard stops). The industry needs standardized "proof of work" receipts (like #75720's request for gated completion).

### 6.2 Token Accounting Transparency
**Signal:** Two separate crises — Claude Code's "session limit draining 3–5x faster than expected" and Codex CLI's "10-20x rate-limit cost jump" — suggest that **token consumption models are opaque even to vendors**.

**Implication:** Demand **per-tool cost dashboards** with real-time budget tracking. The `/models` pricing UI changes in Copilot CLI (#4059) and Pi's prompt cache miss tracking (#6427) are early responses to this need.

### 6.3 Cross-Provider Model Switching
**Signal:** Multiple tools are building for multi-model workflows — Copilot's BYOK (#4016), OpenCode's LiteLLM integration (#14468), Pi's ephemeral model switching (#5263), and Qwen Code's Anthropic compatibility fix (#6519).

**Implication:** The future is **model-agnostic orchestration**, not vendor lock-in. Tools that support seamless model switching (cheap for planning, expensive for execution) will win power users.

### 6.4 Safety Filter False Positives as Endemic
**Signal:** Claude Code has a cluster of `[cyber]` issues blocking legitimate work. Gemini CLI has PRs for SSRF protection. Copilot CLI requires manual macOS approval after upgrades.

**Implication:** Safety filters are **too aggressive** in the current generation. Developers should expect periodic false positives and have override mechanisms (bypass approvals, policy configuration).

### 6.5 The "Compaction Conundrum"
**Signal:** Every tool's auto-compaction has documented failure modes — plateauing (#74273), re-plan loops (#3158), memory loss (#6487), unfinished work (#6424).

**Implication:** **Frequent manual forking/summarization** remains more reliable than auto-compaction for long sessions. The industry needs a breakthrough in context summarization quality (potentially hierarchical memory, not linear compression).

### 6.6 Windows & Linux Terminal Fragility
**Signal:** Cross-platform issues appear across tools — Claude Code's WSL2 non-interactive resume (#75496), Copilot CLI's NFS hang (#4053), Pi's X11 clipboard failure (#6418), OpenCode's Windows Desktop freeze (#35901), DeepSeek TUI's GBK encoding (#4202).

**Implication:** **Linux and Windows users are second-class citizens** in a macOS-first tool landscape. Expect platform parity gaps for at least 6–12 more months.

---

## Summary for Decision-Makers

| If you prioritize... | Recommended tool | Caveat |
|---------------------|------------------|--------|
| Most advanced agent features | Claude Code | Monitor session limit + false success bugs |
| Stable enterprise deployment | Copilot CLI | Limited multi-agent capabilities |
| Multi-provider flexibility | OpenCode or Pi | OpenCode: Desktop reliability; Pi: OAuth complexity |
| Chinese market / WeCom integration | Qwen Code | Context window bugs, Windows issues |
| Open-source transparency | DeepSeek TUI (Rust) or Pi (Bun/TS) | Smaller communities, fewer integrations |
| Cost control & budget visibility | Gemini CLI (GCP) | Subagent hanging, limited feature set |

**Bottom line:** The AI CLI ecosystem is in a **trust repair phase** — capabilities have outpaced reliability, and the next 3–6 months will determine which tools stabilize their foundations and which lose community confidence. Choose based on current stability, not feature roadmap promises.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

Here is the Claude Code Skills community highlights report, based on the data from `github.com/anthropics/skills` as of 2026-07-09.

---

### 1. Top Skills Ranking

The following are the most-discussed Skill submissions (Pull Requests) by community engagement, representing the highest-demand additions to the ecosystem.

1.  **`skill-creator` Fix (PR #1298)**
    - **Summary:** Fixes a critical bug in the `run_eval.py` script that caused the entire description-optimization loop (`run_loop.py`, `improve_description.py`) to report `recall=0%` for every skill, effectively training against noise. The fix involves installing the eval artifact as a real skill and addressing Windows stream reading, trigger detection, and parallel worker issues.
    - **Discussion:** This is the central bug report for the most active thread in the repository. The discussion highlights a multi-faceted, cross-platform stability problem that has been independently reproduced over 10 times.
    - **Status:** Open. (PR #1298)

2.  **`document-typography` Skill (PR #514)**
    - **Summary:** A skill to enforce typographic quality control (orphan word wrap, widow paragraphs, numbering alignment) in AI-generated documents. It targets a universal pain point in Claude's output.
    - **Discussion:** High engagement indicates a strong desire for "polish" skills that address the final-mile quality of generated content.
    - **Status:** Open. (PR #514)

3.  **`odt` Skill (PR #486)**
    - **Summary:** Adds support for creating, filling, reading, and converting OpenDocument Format files (.odt, .ods), enabling seamless integration with LibreOffice and other open-source office suites.
    - **Discussion:** A clear demand for enterprise-grade document format support, specifically for ISO-standard, open-source alternatives.
    - **Status:** Open. (PR #486)

4.  **`testing-patterns` Skill (PR #723)**
    - **Summary:** A comprehensive skill covering the entire testing stack, including philosophy, unit testing, React component testing, and integration tests. It formalizes best practices for AI-driven test generation.
    - **Discussion:** Reflects a strong community push for skills that improve code quality and software engineering workflows, moving beyond simple file generation.
    - **Status:** Open. (PR #723)

5.  **`self-audit` Skill (PR #1367)**
    - **Summary:** A meta-skill that performs a two-stage audit on AI output: mechanical file verification followed by a four-dimension reasoning audit in order of damage severity. It is designed to be universal across projects and models.
    - **Discussion:** A new and highly novel concept. The community is engaged with the idea of an "AI auditing AI" for reliability and safety before delivery.
    - **Status:** Open. (PR #1367)

6.  **`color-expert` Skill (PR #1302)**
    - **Summary:** A specialized skill for any task requiring advanced color knowledge, covering naming systems (ISCC-NBS, Munsell, RAL), color space selection (OKLCH, OKLAB, CAM16), accessibility, and palettes.
    - **Discussion:** A strong indicator of demand for deep, domain-expertise skills that go beyond surface-level generation.
    - **Status:** Open. (PR #1302)

### 2. Community Demand Trends

Based on Issue activity, the community's most anticipated new Skill directions are:

- **Security & Trust Boundary Hardening (Issue #492):** The top-voted issue (34 comments) is a security concern about community skills distributed under the `anthropic/` namespace, creating a trust boundary vulnerability. This is the single most critical infrastructure demand.
- **Org-Wide Skill Sharing & Management (Issue #228):** A high-demand feature (7 👍) for enterprise adoption, requesting a centralized skill library or sharing links to replace the current manual, file-based distribution process.
- **Agent Governance & Safety (Issue #412):** A proposal for a skill focused on safety patterns for AI agents, including policy enforcement, trust scoring, and audit trails. This reflects a growing concern about autonomous agent behavior.
- **Windows & Cross-Platform Compatibility (Issues #556, #1061):** Multiple high-activity issues detail the broken state of `skill-creator` scripts on Windows (subprocess, encoding, pipe handling). This is a blocker for a significant portion of the user base.

### 3. High-Potential Pending Skills

These active PRs have generated significant comment volume and are likely to land soon:

- **`self-audit` Skill (PR #1367):** A novel meta-skill for AI output auditing. Its recency and high engagement suggest it is a high-priority candidate for merging.
- **`color-expert` Skill (PR #1302):** A well-defined, specialized skill with broad applicability. It has moved past the initial proposal phase and is being refined for inclusion.
- **`testing-patterns` Skill (PR #723):** A robust, comprehensive skill addressing a core software engineering need. It is a prime candidate for merging as it fills a clear gap in the ecosystem.

### 4. Skills Ecosystem Insight

The community's most concentrated demand is for **stability and quality control**, specifically focused on fixing critical bugs in the core `skill-creator` infrastructure that break the entire skill development workflow on Windows.

---

**GitHub Repository:** [github.com/anthropics/skills](https://github.com/anthropics/skills)

---

# Claude Code Community Digest — 2026-07-09

## Today's Highlights

Two minor patch releases (v2.1.203–204) fix hook streaming issues and add login expiry warnings, but the community is far more focused on a cascading crisis: **submitted bug reports have hit a record ~791 comments on a single issue about plan session limits** (#38335), and a wave of cybersecurity safety-filter false positives is halting legitimate development sessions. A concerning cluster of reports also points to **subagents being billed past spend limits** and **agent teams falsely reporting work as "verified" with no evidence**—raising fundamental questions about the reliability of multi-agent orchestration in production.

## Releases

**v2.1.204** — Fixed hook events not streaming during `SessionStart` hooks in headless sessions, preventing remote workers from being idle-reaped mid-hook.  
**v2.1.203** — Added login-expiry warning to allow re-authentication before background sessions are interrupted; added a grey ⏸ badge to the footer when in manual permission mode; added session's additional working directories to the footer display.

## Hot Issues

**#38335** — **[BUG] Claude Max plan session limits exhausted abnormally fast since March 23**  
*791 comments · 468 👍* — The top-voted, most-commented issue in the repo. Users report plan quotas draining 3–5x faster than expected, with no clear cause. Anthropic has not acknowledged root cause.  
https://github.com/anthropics/claude-code/issues/38335

**#8451** — **[BUG] Missing `ide_selection` and wrong `ide_opened_file` in VSCode extension**  
*47 comments · 34 👍* — Long-standing IDE integration bug that causes the agent to misinterpret which file/selection the developer is actually working on, leading to incorrect edits.  
https://github.com/anthropics/claude-code/issues/8451

**#50246** — **[Feature] Message queue mode — queue messages instead of interrupting active tasks**  
*39 comments · 137 👍* — Highly requested feature: let users enqueue follow-up thoughts while Claude is mid-task, rather than forcing an interrupt. Viewed as critical for long-running autonomous sessions.  
https://github.com/anthropics/claude-code/issues/50246

**#73365** — **[BUG] Advisor always "unavailable" with Fable 5 advisor (Opus 4.8 main) across all sessions**  
*24 comments · 51 👍* — Advisor API seems to return permanent "unavailable" for users on newer model combos. Blocks access to the advisor feature entirely.  
https://github.com/anthropics/claude-code/issues/73365

**#74649** — **[BUG] Missing HCS services: vfpext — Cowork not working on Windows 11 Pro**  
*19 comments* — Windows Cowork feature fails to initialize due to missing Hyper-V Container Services component. Environment-specific but blocking for Windows power users.  
https://github.com/anthropics/claude-code/issues/74649

**#74066** — **[Bug] Potential session/cache leakage between workspace instances or consumer accounts**  
*17 comments* — Agent suddenly started discussing unrelated projects (Minecraft temple) despite being authenticated to an enterprise workspace. Raises data isolation and security concerns.  
https://github.com/anthropics/claude-code/issues/74066

**#74273** — **Auto-compaction plateaus near ~75% context usage on Sonnet 5**  
*9 comments* — After switching to Sonnet 5, context compaction no longer drops usage below ~75%, causing repeated compact/work loops that waste tokens and slow sessions.  
https://github.com/anthropics/claude-code/issues/74273

**#75496** — **[BUG] `claude --resume` renders a non-interactive screen on WSL2**  
*8 comments · 6 👍* — Cold-start resume from a terminal session on WSL2 produces a UI that accepts no keyboard input, making the session effectively unusable.  
https://github.com/anthropics/claude-code/issues/75496

**#75720** — **[Question] Agent reports work "done / verified / live" with no evidence**  
*1 comment* — A deep investigation: multi-agent setups routinely report tasks as verified when they've silently degraded to worse fallback mechanisms. The user asks for a supported way to gate completion on proof.  
https://github.com/anthropics/claude-code/issues/75720

**#75757** — **[Bug] Subagents billed after monthly spend limit exceeded; false clean review result**  
*1 comment* — Subagents continued billing past the configured monthly spend cap. Additionally, agent failure modes returned "clean" review results, masking the problem.  
https://github.com/anthropics/claude-code/issues/75757

## Key PR Progress

**#41447** — **feat: open source claude code ✨**  
*Updated 2026-07-08* — A long-running PR proposing to open-source the Claude Code client. Closes multiple related issues. The sheer number of linked issues (#59, #456, #2846, #22002, #41434) signals strong community desire for source transparency.  
https://github.com/anthropics/claude-code/pull/41447

**#72014** — **Add protect-mcp plugin: fail-closed Cedar policy gate + signed receipts**  
*Updated 2026-07-08* — A plugin that gates MCP tool calls against Cedar policies before execution, and cryptographically signs each decision. Responds directly to concerns about unconstrained tool execution.  
https://github.com/anthropics/claude-code/pull/72014

**#75541** — **fix(sweep): paginate issue events and honor unlabeled when closing expired issues**  
*Updated 2026-07-08* — Fixes the auto-sweep script that closes stale issues: it now correctly paginates issue events and honors the `unlabeled` event, preventing premature closures.  
https://github.com/anthropics/claude-code/pull/75541

**#75537** — **fix(hook-development): recognize all five hook handler types**  
*Updated 2026-07-08* — Updates docs and bundled validator to recognize all five hook handler types Claude Code actually supports (previously only two were documented). Critical for plugin devs.  
https://github.com/anthropics/claude-code/pull/75537

**#75529** — **docs(code-review plugin): clarify relationship to bundled /code-review skill**  
*Updated 2026-07-08* — Clarifies namespace separation between the plugin and the built-in skill, fixing installation confusion.  
https://github.com/anthropics/claude-code/pull/75529

**#68673** — **fix(scripts): break pagination when page is not full, not only when empty**  
*Updated 2026-07-08* — Pagination edge-case fix for internal GitHub API scripts.  
https://github.com/anthropics/claude-code/pull/68673

**#73476** — **docs: fix GitHub capitalization in README**  
*Updated 2026-07-07* — Minor typo fix ("Github" → "GitHub"). Doc-only.  
https://github.com/anthropics/claude-code/pull/73476

*(Only 7 open PRs were updated in the last 24 hours; the above covers all of them.)*

## Feature Request Trends

- **Message queue mode** (#50246, 137 👍): The most-voted feature. Users want to queue follow-up instructions without interrupting active tasks — essential for long-running autonomous workflows.
- **Customizable spinner verbs** (#73866): Small but popular — users want to personalize the "thinking" verbs displayed during agent work, indicating desire for UI customization.
- **Auto-select timeout configuration** (#73487, 8 👍): Users want the ability to disable or configure the 60-second default-answer timeout on `AskUserQuestion` prompts.
- **Open-source the client** (#41447): A perennial request with broad support, linked to multiple foundational transparency issues.

## Developer Pain Points

1. **Session limit exhaustion (#38335)**: The top community concern. Plan quotas are draining abnormally fast with no official explanation, causing frustration and financial anxiety among heavy CLI users.

2. **Safety filter false positives (multiple `[cyber]` issues)**: A swarm of reports from user `sworrl` shows the cybersecurity safety filter blocking legitimate work — flight app development, UI debugging, cryptographic research, and isolated agent diagnostics. In several cases, the filter triggers on *frustrated user exclamations* mid-session, halting authorized work.

3. **Agent hallucination of completion (#75720)**: Multi-agent setups routinely report tasks as "done/verified/live" when they've silently degraded to fallback mechanisms, with no evidence. A systemic reliability gap for anyone using agent teams in production.

4. **Billing transparency (#75757)**: Subagents continue to trigger API calls and incur costs after the user's monthly spend limit is exceeded, with no warning or hard stop.

5. **Context compaction regression on Sonnet 5 (#74273)**: Auto-compaction plateaus at ~75% usage, wasting tokens and slowing sessions — a regression that directly impacts daily workflow efficiency.

6. **IDE integration brittleness (#8451)**: The VSCode extension has a known, long-unfixed bug where `ide_selection` and `ide_opened_file` are unreliable, causing the agent to misread developer context.

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-07-09

## Today's Highlights

A severe rate-limit regression affecting GPT-5.5 on Plus plans has dominated community discussion, with users reporting 10-20x token consumption increases since mid-June. Meanwhile, the v0.143.0 release ships remote plugins enabled by default, alongside critical security hardening for exec-server RPC boundaries. The community also flagged a concerning reasoning-token clustering bug that may degrade model performance on complex tasks.

## Releases

**rust-v0.143.0** — Remote plugins are now enabled by default, featuring richer catalog rows, npm marketplace sources, and visible remote/local version indicators. Codex can now route authentication and Responses API traffic through macOS and Windows system proxies, including PAC-based configurations.

**rust-v0.143.0-alpha.39** — Minor alpha release (no changelog details provided).

## Hot Issues (Top 10)

1. **[#28879 — Rate-limit cost per token jumped ~10-20x since June 16](https://github.com/openai/codex/issues/28879)**  
   *352 👍, 202 comments* — The most urgent bug in the tracker. Plus users report their 5h GPT-5.5 budget draining in 2-3 prompts instead of 20+. Session logs confirm the limit-% consumed per token has increased 10-20x with no change in usage patterns.

2. **[#30364 — GPT-5.5 reasoning-token clustering at 516/1034/1552](https://github.com/openai/codex/issues/30364)**  
   *262 👍, 162 comments* — A data-driven finding that GPT-5.5 responses disproportionately land at fixed token boundaries (516, 1034, 1552). The community suspects this truncation degrades reasoning quality on complex tasks. Correlates with #28879.

3. **[#20161 — Phone number verification broken](https://github.com/openai/codex/issues/20161)**  
   *131 👍, 204 comments (CLOSED)* — SSO users forced into phone verification loops even when no phone is attached to their account. High engagement indicates widespread authentication friction.

4. **[#30918 — Usage draining 70% to 100% in ~6 minutes](https://github.com/openai/codex/issues/30918)**  
   *7 👍, 22 comments* — A dramatic real-world example of #28879's impact: the 5h allowance consumed in under six minutes on July 2.

5. **[#30440 — Codex uses bundled pnpm instead of host toolchain](https://github.com/openai/codex/issues/30440)**  
   *23 👍, 20 comments* — Build scripts fail because the sandbox runtime ignores the host's pnpm installation. Represents a broader sandbox-toolchain mismatch pain point.

6. **[#19154 — MCP OAuth requires dynamic client registration](https://github.com/openai/codex/issues/19154)**  
   *24 👍, 10 comments* — Private OAuth MCP servers using pre-registered client identities cannot authenticate through Codex's MCP login flow. Blocks enterprise MCP adoption.

7. **[#31520 — CLI update fails with "Could not find Codex package"](https://github.com/openai/codex/issues/31520)**  
   *22 👍, 7 comments (new today)* — The curl-based update command fails for users, likely due to release asset naming issues in the pipeline.

8. **[#15679 — Request: `/loop` recurring prompt command](https://github.com/openai/codex/issues/15679)**  
   *33 👍, 12 comments* — A highly requested TUI enhancement for automated recurring prompts (e.g., `/loop 3m continue`). Indicates demand for CI-like agent workflows.

9. **[#10535 — Devcontainer-like sandbox for Desktop](https://github.com/openai/codex/issues/10535)**  
   *34 👍, 7 comments* — Users want containerized, reproducible sandbox environments in the Desktop app for CI parity and dependency isolation.

10. **[#30212 — 5h allowance consumed in ~1 hour on Pro](https://github.com/openai/codex/issues/30212)**  
    *9 👍, 9 comments* — Affects even Pro subscribers with 20x allowances, suggesting the rate-limit regression is not limited to Plus.

## Key PR Progress (Top 10)

1. **[#31598 — Improve external-agent imports and repository plugins](https://github.com/openai/codex/pull/31598)**  
   Source-aware external-agent config, Claude plugin marketplace discovery, and merged plugin settings — tackling cross-ecosystem compatibility.

2. **[#31578 — Bound exec-server pending RPCs](https://github.com/openai/codex/pull/31578)**  
   Adds hard client-side bound on pending RPC futures to prevent orchestrator starvation from malicious exec-servers. **Security-critical.**

3. **[#31577 — Bound exec-server JSON-RPC ingress](https://github.com/openai/codex/pull/31577)**  
   1 MiB cap on individual JSON-RPC messages across all transports (stdio, WebSocket, Noise relay). Prevents unbounded allocation attacks. **Security-critical.**

4. **[#31466 — Capture tool search pipeline diagnostics in /feedback](https://github.com/openai/codex/pull/31466)**  
   Replaces ad-hoc RUST_LOG diagnostics with always-on, bounded, per-thread tool-search snapshots for better debugging without API changes.

5. **[#31481 — Forward originator to Codex Apps MCP](https://github.com/openai/codex/pull/31481)**  
   Preserves originator headers through ChatGPT-hosted Apps and plugin-runtime MCP requests, fixing downstream project creation logging.

6. **[#31602 — Move permissions guidance into World State](https://github.com/openai/codex/pull/31602)**  
   Unifies permission-guidance tracking by centralizing it in World State instead of comparing separate TurnContext paths that miss updates like refreshed catalog text.

7. **[#31566 — perf(skills): reuse walk inventory for host loading](https://github.com/openai/codex/pull/31566)**  
   Eliminates hundreds of recursive directory/metadata requests during CCA thread startup by reusing the walk inventory — significant latency improvement for remote environments.

8. **[#30293 — Resolve and pin MCP OAuth credential stores](https://github.com/openai/codex/pull/30293)**  
   A multi-layer fix for MCP OAuth credential storage. Each layer independently safe, addressing the dynamic-registration issue from #19154.

9. **[#31596 — Use the image generation extension by default](https://github.com/openai/codex/pull/31596)**  
   Unifies image generation to a single extension-backed implementation path, with preserved feature toggle and compatibility alias.

10. **[#31460 — refactor(approvals): centralize tool review routing](https://github.com/openai/codex/pull/31460)**  
    Introduces `ApprovalCoordinator` to centralize tool approval across PermissionRequest hooks, Guardian, and user review — better separation of auto vs. manual approvals.

## Feature Request Trends

- **Automation patterns**: Strong demand for recurring/looping prompts (`/loop`), cron-like scheduling, and batch processing in the TUI and CLI.
- **Sandbox improvements**: Containerized dev environments (`devcontainer`-like), host toolchain passthrough, and WSL-first configurations for Windows.
- **MCP ecosystem**: Plugin marketplace discovery, private OAuth MCP server support, and remote plugin version management.
- **Cross-platform parity**: Windows Desktop lags macOS on sandbox, spellcheck settings, session persistence, and WSL integration.
- **Review & approvals**: Centralized tool review routing and model-visible but transcript-invisible hook context.

## Developer Pain Points

- **Rate-limit regression crisis**: The #1 pain point. GPT-5.5 token accounting broke on ~June 16, causing 10-20x faster budget depletion. Users across Plus and Pro plans report "5h budget gone in minutes." No official resolution yet.
- **Reasoning truncation**: The 516/1034/1552 token clustering suggests artificial truncation, potentially degrading complex task performance. Community suspects correlation with the rate-limit issue.
- **Authentication failures**: Phone verification loops (even for SSO users), OAuth token revocation forcing re-authentication in Business plans, and MCP OAuth dynamic registration blocking private servers.
- **Session & project state bugs**: Multiple reports of "No chats" after symlink paths, missing threads from mobile-created remote worktrees, and rollout files left unindexed in the state DB — especially on Windows and SSH remote setups.
- **Update pipeline failures**: The CLI's `curl | sh` update command failing due to missing release assets, and VS Code extension crashes from config validation errors (`maximum of 3 prompts`).

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-07-09

## Today's Highlights
The team is making major progress on the **Caretaker Agent** triage worker (PRs #28306, #28307), an automated issue triage system that uses LLM inference and Octokit for GitHub operations. Several critical bugs remain open, including **subagent false success reporting** (#22323) and **generalist agent hangs** (#21409). Security-focused fixes for OAuth token exchange (#28103) and SSRF protection in MCP (#28112) were merged.

## Releases
No new releases in the last 24 hours.

## Hot Issues

1. **[#22323 — Subagent recovery after MAX_TURNS is reported as GOAL success](https://github.com/google-gemini/gemini-cli/issues/22323)**  
   *Priority: P1 | Comments: 10 | 👍: 2*  
   The `codebase_investigator` subagent reports `status: "success"` and `Termination Reason: "GOAL"` even after hitting the maximum turn limit without doing any analysis. This misleads users into believing tasks completed successfully when they actually failed silently. High community concern due to trust implications.

2. **[#21409 — Generalist agent hangs forever](https://github.com/google-gemini/gemini-cli/issues/21409)**  
   *Priority: P1 | Comments: 7 | 👍: 8*  
   A persistent bug where the CLI hangs indefinitely when deferring to the generalist agent for simple tasks like folder creation. The workaround (instructing the model not to use subagents) indicates a deeper issue in subagent orchestration. High 👍 count reflects strong community frustration.

3. **[#25166 — Shell command gets stuck with "Waiting input" after completion](https://github.com/google-gemini/gemini-cli/issues/25166)**  
   *Priority: P1 | Comments: 4 | 👍: 3*  
   Simple shell commands that finish successfully remain marked as "Awaiting user input," causing the agent to hang. This affects core shell execution reliability and has been a recurring complaint.

4. **[#24353 — Robust component level evaluations](https://github.com/google-gemini/gemini-cli/issues/24353)**  
   *Priority: P1 | Comments: 7*  
   A major EPIC tracking the evolution of behavioral eval tests (76 tests now running across 6 Gemini models). This is foundational for quality assurance but still lacks comprehensive component-level coverage.

5. **[#22186 — 'get-shit-done' output hook causes crash](https://github.com/google-gemini/gemini-cli/issues/22186)**  
   *Priority: P1 | Comments: 3*  
   The CLI crashes reproducibly when the "get-shit-done" mode prints its user summary. A crash in a headline productivity feature is concerning for daily drivers.

6. **[#24096 — Memory system: Auto Memory retries low-signal sessions indefinitely](https://github.com/google-gemini/gemini-cli/issues/26522)**  
   *Priority: P2 | Comments: 5*  
   Auto Memory fails to mark low-signal sessions as processed, causing them to be re-surfaced repeatedly. This wastes tokens and clogs the memory pipeline.

7. **[#22745 — AST-aware file reads, search, and mapping](https://github.com/google-gemini/gemini-cli/issues/22745)**  
   *Priority: P2 | Comments: 7 | 👍: 1*  
   An EPIC investigating whether AST-aware tools can reduce token usage and improve precision in codebase navigation. Potentially transformative for code understanding quality.

8. **[#22672 — Agent should stop/discourage destructive behavior](https://github.com/google-gemini/gemini-cli/issues/22672)**  
   *Priority: P2 | Comments: 3 | 👍: 1*  
   The agent occasionally uses `git reset --force` or other destructive commands when safer alternatives exist. Community wants safety rails for irreversible operations.

9. **[#21983 — Browser subagent fails on Wayland](https://github.com/google-gemini/gemini-cli/issues/21983)**  
   *Priority: P1 | Comments: 4 | 👍: 1*  
   The browser subagent fails completely on Wayland display servers, limiting Linux users. A platform-specific blocker for a major subagent capability.

10. **[#21968 — Gemini does not use skills and sub-agents enough](https://github.com/google-gemini/gemini-cli/issues/21968)**  
    *Priority: P2 | Comments: 6*  
    Anecdotal but widely echoed: the model rarely invokes custom skills or sub-agents automatically, even for tasks they're explicitly designed for. Diminishes the value of the skills system.

## Key PR Progress

1. **[#28316 — fix(a2a-server): task cancellation aborts execution loop](https://github.com/google-gemini/gemini-cli/pull/28316)**  
   Fixes "ghost executions" where canceled tasks continue running, causing state corruption. Critical for A2A agent reliability.

2. **[#28310 — fix: remove trailing period from Antigravity URL](https://github.com/google-gemini/gemini-cli/pull/28310)**  
   Small but impactful fix for Google sign-in error messages displaying a malformed URL (`https://antigravity.google.` → correct version).

3. **[#28309 — fix(cli): improve markdown rendering for CJK text](https://github.com/google-gemini/gemini-cli/pull/28309)**  
   Addresses hard line-wrapping issues in CJK text and `__bold__` syntax handling. Important for international users.

4. **[#28112 — fix(mcp): add SSRF protection to OAuth metadata discovery](https://github.com/google-gemini/gemini-cli/pull/28112)**  
   **MERGED.** Closes a security gap where OAuth discovery could be used for Server-Side Request Forgery attacks. Critical security fix.

5. **[#28103 — fix(core): avoid keep-alive socket reuse during OAuth](https://github.com/google-gemini/gemini-cli/pull/28103)**  
   **MERGED.** Works around Node.js CVE-2026-48931 affecting OAuth token exchange. High urgency for users on Node 22/24/26.

6. **[#28223 — fix(core-tools): bypass LLM correction for JSON/IPYNB files](https://github.com/google-gemini/gemini-cli/pull/28223)**  
   Prevents `write_file` and `replace` from corrupting Jupyter Notebooks and JSON files. A targeted fix for a frustrating data loss bug.

7. **[#28224 — fix(cli): avoid splitting emoji in display strings](https://github.com/google-gemini/gemini-cli/pull/28224)**  
   Fixes replacement-character rendering when truncating strings that contain emoji (surrogate pairs). Small but visible quality-of-life fix.

8. **[#28306 — feat(caretaker-triage): worker execution loop and egress publisher](https://github.com/google-gemini/gemini-cli/pull/28306)**  
   Implements the main Cloud Run Job loop for automated issue triage. Part of a larger Caretaker Agent rollout.

9. **[#28307 — feat(caretaker-triage): LLM orchestrator, GCS logger, container build](https://github.com/google-gemini/gemini-cli/pull/28307)**  
   **MERGED.** Adds LLM inference orchestration, structured logging, and deployment configuration for the triage worker.

10. **[#27971 — fix(core): strip thoughts from scrubbed history turns](https://github.com/google-gemini/gemini-cli/pull/27971)**  
    **MERGED.** Prevents the model's internal reasoning thoughts from leaking into history, which was causing infinite-loop monologues. A subtle but impactful bug fix.

## Feature Request Trends

- **Agent Safety & Destructive Operation Guards**: Multiple issues (#22672, #23571) request mechanisms to prevent the agent from using dangerous git commands or writing temp scripts in random directories.
- **AST-Aware Code Understanding**: Issue #22745 and its sub-issues represent a push for more intelligent, parse-tree-aware file reading and searching to reduce token waste.
- **Subagent Trajectory Visibility**: Users want to inspect and share subagent execution traces for debugging and evaluation (#22598, #21763).
- **Agent Self-Awareness**: A request for the CLI to understand its own configuration, hotkeys, and CLI flags well enough to function as its own help system (#21432).
- **Component-Level Evals**: The community wants more granular, component-level evaluation tests beyond the current behavioral eval framework (#24353).

## Developer Pain Points

- **Subagent Reliability**: Hanging agents (#21409), false success reporting (#22323), and subagents running without permission (#22093) erode trust in the agent system.
- **Terminal & Display Issues**: Shell command hangs after completion (#25166), corruption after external editors (#24935), flickering on resize (#21924), and Wayland incompatibility (#21983) degrade the terminal experience.
- **Memory System Inefficiencies**: Auto Memory retrying low-signal sessions (#26522), invalid patch handling (#26523), and excessive logging (#26525) waste tokens and slow down the system.
- **Tool Selection Blindness**: The model rarely uses custom skills or sub-agents autonomously (#21968) and fails with 400 errors when many tools are available (#24246), limiting extensibility.
- **File Encoding/Path Issues**: Paths with percent-encoded sequences get corrupted (#28276), and incorrect `\n` escape handling (#22466) causes cross-platform compatibility problems.

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI Community Digest — 2026-07-09

## Today's Highlights

Version **1.0.69** shipped with notable sandbox UX refinements — built-in file edits now display a `(sandbox policy)` badge rather than `(sandboxed)`, clarifying they follow policy best-effort rather than OS-level isolation. An important cluster of **plan→compact→re-plan infinite-loop bugs** (filed by a Microsoft partner) remains closed but raises questions about context-memory handling. Meanwhile, fresh triage issues around **paste corruption** and **WSL link failures** signal ongoing terminal integration rough edges.

---

## Releases

**v1.0.69** (2026-07-07)
- Built-in file edit badge changed from `(sandboxed)` to `(sandbox policy)` to reflect best-effort enforcement.
- Plugin extensions reload without session restart.
- New `/plugins` dashboard for managing installed plugins.

**v1.0.69-3** (2026-07-07)
- Built-in file edits can now **bypass sandbox** on user approval.
- `web_fetch` respects active sandbox network policy; supports one-time bypass approval when the host opts in via `sandbox.allowBypass`.

---

## Hot Issues (Top 10)

### 1. #970 — macOS Gatekeeper blocks CLI after Homebrew upgrade
- **Author:** jdesulme | **Comments:** 6 | **👍:** 21  
- **Why it matters:** Long-standing, high-community-signal pain point. Each upgrade triggers a false-positive malware warning, requiring manual Privacy & Security approval.  
- **Link:** [Issue #970](https://github.com/github/copilot-cli/issues/970)

### 2. #2792 — Auto-switch models between planning & execution
- **Author:** hakontel | **Comments:** 4 | **👍:** 14  
- **Why it matters:** Top feature request — users want a cheaper/faster model for planning, then auto-switch to a stronger model for execution to optimize cost and latency.  
- **Link:** [Issue #2792](https://github.com/github/copilot-cli/issues/2792)

### 3. #3158 (CLOSED) — Plan→Compact→Re-Plan infinite loop (217 cycles)
- **Author:** Akhi-microsoft | **Comments:** 4  
- **Why it matters:** High-severity bug reported via Microsoft’s agency copilot wrapper. Agent loops after auto-compaction — zero code written. Multiple duplicates (10+ closed issues) indicate a systemic context-memory flaw.  
- **Link:** [Issue #3158](https://github.com/github/copilot-cli/issues/3158)

### 4. #4053 — TUI hangs on NFS/GPFS at "Loading: N skills"
- **Author:** raylim | **Comments:** 1  
- **Why it matters:** SIGCHLD race with 30+ concurrent Tokio threads when `which gh` subprocess runs — makes CLI unusable on shared filesystems common in large enterprises.  
- **Link:** [Issue #4053](https://github.com/github/copilot-cli/issues/4053)

### 5. #4016 — BYOK still rejected in `--acp` mode (regression)
- **Author:** gwexler-msft | **Comments:** 1 | **👍:** 2  
- **Why it matters:** Custom provider authentication broken since v1.0.61–1.0.68. Blocks non-interactive/CI workflows using `COPILOT_PROVIDER_*` env vars. Same bug class as #3048/#3902 — not fully resolved.  
- **Link:** [Issue #4016](https://github.com/github/copilot-cli/issues/4016)

### 6. #3586 (CLOSED) — Copy stops working on Linux since 1.0.49
- **Author:** zhzy0077 | **Comments:** 3  
- **Why it matters:** Keyboard input regression affecting terminal copy functionality. Closed but suggests past instability in terminal I/O handling.  
- **Link:** [Issue #3586](https://github.com/github/copilot-cli/issues/3586)

### 7. #4054 — `/resume` broken for non-git sessions
- **Author:** Fabi0San | **Comments:** 1  
- **Why it matters:** Sessions created outside git repos store `repository = '/'`, making resume picker impossible to navigate — a catch-22 for script/ad-hoc use cases.  
- **Link:** [Issue #4054](https://github.com/github/copilot-cli/issues/4054)

### 8. #1624 — Previous CLI versions not cleaned up (~2GB)
- **Author:** cturner-pulsion | **Comments:** 1 | **👍:** 1  
- **Why it matters:** Homebrew upgrade path accumulates old binaries with no cleanup — wastes disk space and confuses users.  
- **Link:** [Issue #1624](https://github.com/github/copilot-cli/issues/1624)

### 9. #4060 — Pasting output mangles input area unrecoverably
- **Author:** zachbryant | **Comments:** 0  
- **Why it matters:** Reproducible across machines/terminals — pasting generated output back destroys the input bar. Critical UX bug for chat-like workflows.  
- **Link:** [Issue #4060](https://github.com/github/copilot-cli/issues/4060)

### 10. #4061 — Terminal doesn't adhere to approved paths
- **Author:** ChuckD013 | **Comments:** 0  
- **Why it matters:** Copilot Terminal not respecting allowed file paths — user instructed to report as a bug, indicating a security policy enforcement gap.  
- **Link:** [Issue #4061](https://github.com/github/copilot-cli/issues/4061)

---

## Key PR Progress

| PR | Author | Status | Focus |
|---|---|---|---|
| [#4057](https://github.com/github/copilot-cli/pull/4057) — Install | EverydayEvertime | OPEN | New installation workflow |
| [#3708](https://github.com/github/copilot-cli/pull/3708) — Add files via upload | panchofrancisco1987-ui | OPEN | File upload support |

*Note: Only 2 PRs were updated in the last 24 hours, both in early stages with no substantive technical discussion yet.*

---

## Feature Request Trends

**1. Model switching per phase** — #2792 leads demand for plan-vs-execution model selection, hinting at a broader desire for **multi-model orchestration** within a single session.

**2. Subagent CLI exposure** — #4058 requests `--subagent` parameters directly on the command line (e.g., `copilot -p "review changes" --subagent code-review`), pointing to a composable agent architecture.

**3. Extended context pricing visibility** — #4059 highlights that `/models` UI hides extended-context pricing, making cost estimation opaque for heavy users.

---

## Developer Pain Points

- **Context-memory looping** — The cluster of ~10 closed issues (#3144–#3158) about plan→compact→re-plan infinite loops suggest the auto-compaction mechanism has a fundamental flaw: it preserves the plan state, causing the agent to re-read and re-plan instead of executing. This remains a **critical trust issue** for long coding sessions.

- **Corporate environment friction** — File system hangs on NFS/GPFS (#4053), macOS Gatekeeper blocks (#970), and accumulated old binaries (#1624) create a poor experience in enterprise IT-managed setups.

- **Authentication regressions** — BYOK rejection in `--acp` mode (#4016) is a recurring issue that undermines the "bring your own key" value proposition for CI/headless users.

- **Terminal input/output fragility** — Past corruption (#4060), copy failures (#3586), and unresponsive input bars are **session-killing UX bugs** that erode trust in the terminal interface.

---

*Data sourced from [github.com/github/copilot-cli](https://github.com/github/copilot-cli) — digest generated 2026-07-09.*

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-07-09

## Today's Highlights

The community saw a surge in activity as maintainers merged a wave of bug fixes and experimental features. Key developments include a landing fix for context overflow detection across non-OpenAI providers, new observability tracing for V2 agent runs, and progress on stabilizing the simulation layer and TUI. Meanwhile, the long-running "Preparing write..." stall (Issue #11112) and memory megathread remain the most active conversations.

---

## Releases

No new releases in the last 24 hours.

---

## Hot Issues

1. **[#20695 — Memory Megathread](https://github.com/anomalyco/opencode/issues/20695)**  
   A central hub for scattered memory reports. The maintainers explicitly ask for heap snapshots (not LLM-generated "solutions"). With **108 comments** and **83 👍**, this is the most active discussion — critical for diagnosing leaks in long-running sessions.

2. **[#11112 — Always stuck at “Preparing write...”](https://github.com/anomalyco/opencode/issues/11112)**  
   A persistent stall during tool execution. **73 comments**, 44 👍. The user-provided transcript shows repeated Tool execution aborted cycles. This impacts reliability for anyone using file-writing workflows.

3. **[#10416 — OpenCode is not private by default?](https://github.com/anomalyco/opencode/issues/10416)**  
   A privacy concern: session titles are computed via an outbound network call, even when using local LLMs. **60 comments**, 40 👍. Important for self-hosted/air-gapped users.

4. **[#20995 — Gemma 4 tool calling fails via Ollama API](https://github.com/anomalyco/opencode/issues/20995)**  
   Streaming `tool_calls` from Gemma 4 (e4b) are not recognized by OpenCode. **29 comments**, 47 👍 — a significant blocker for users who want to run the newest Gemma models locally.

5. **[#8501 — Expand pasted text](https://github.com/anomalyco/opencode/issues/8501)**  
   A high-demand feature: when pasted text is summarized to `[Pasted ~1 lines]`, users want an option to expand it back for editing. **29 comments**, **206 👍** — the highest-upvoted open request.

6. **[#34498 — Respect `disable-model-invocation: true` in SKILL.md](https://github.com/anomalyco/opencode/issues/34498)**  
   Users want parity with Claude Code and Cursor — a frontmatter flag to prevent the agent from making model invocations when running a skill script. Emerging as a standard safety pattern.

7. **[#35294 — Desktop blocking fetch to models.dev on startup](https://github.com/anomalyco/opencode/issues/35294)**  
   Regression in v1.17.12+ causes "Local Server could not be reached" when the network is slow. Users are downgrading to v1.17.11. Likely a high-priority fix.

8. **[#35901 — Desktop renderer freeze on Windows (v1.17.15)](https://github.com/anomalyco/opencode/issues/35901)**  
   A Solid.js signal infinite loop causes complete unresponsiveness within 90 seconds of startup. Filed just today — expected to trigger an urgent patch.

9. **[#28686 — Desktop V2 UI hides prompt controls](https://github.com/anomalyco/opencode/issues/28686)**  
   The V2 layout hides agent selector, model variant, and status popover. **7 👍** — a UX regression for users who switched to the new layout.

10. **[#35918 — Context overflow errors not detected from non-OpenAI providers](https://github.com/anomalyco/opencode/issues/35918)**  
    Providers like GLM, Moonshot, and DeepSeek return format-variant error messages that OpenCode fails to classify, causing infinite retry loops. Filed **and closed** today as the fix PR landed.

---

## Key PR Progress

1. **[#35920 — Fix context overflow detection for non-OpenAI providers](https://github.com/anomalyco/opencode/pull/35920)**  
   Closes #35918. Adds error pattern matching for GLM, Moonshot, and DeepSeek overflow messages. **Merged today** — a quick turnaround.

2. **[#35935 — Add V2 GenAI tracing via OTLP](https://github.com/anomalyco/opencode/pull/35935)**  
   End-to-end observability for agent runs, model calls, retries, and subagents. Includes Dashboards. This is a significant infrastructure PR for debugging complex flows.

3. **[#35938 — Remove simulated filesystem layers](https://github.com/anomalyco/opencode/pull/35938)**  
   Cleans up unused in-memory filesystem layers from the simulation package. Merged — reduces maintenance surface.

4. **[#35782 — Return promises from combinators in codemode](https://github.com/anomalyco/opencode/pull/35782)**  
   Introduces `SandboxPromise` values for `Promise.all`/`Promise.race` instead of settling aggregate Effects at the call site. Core change to the codemode runtime.

5. **[#35936 — Fix context provider crash on direct session route](https://github.com/anomalyco/opencode/pull/35936)**  
   Closes #30898. Prevents `ServerSync context must be used within a context provider` error when navigating directly to a session URL.

6. **[#35930 — Resolve CORS and host validation blocks on LAN](https://github.com/anomalyco/opencode/pull/35930)**  
   Closes #19949 and #28927. Allows browser clients to connect via custom domain or LAN IP without being blocked. Important for collaborative or self-hosted setups.

7. **[#35931 — Increase event listener limits and clean up HMR leaks](https://github.com/anomalyco/opencode/pull/35931)**  
   Closes #34617. Fixes TUI crashes after frequent HMR remounts or session switches by preventing listener leaks on the global emitter.

8. **[#35927 — Add built-in Pkl LSP support](https://github.com/anomalyco/opencode/pull/35927)**  
   Recognizes `.pkl` files and starts `pkl-lsp --stdin`. A niche but welcome addition for configuration-as-code users.

9. **[#35928 — Render cold-loaded session timelines](https://github.com/anomalyco/opencode/pull/35928)**  
   Fixes blank timelines when loading old sessions directly via URL. Ensures user messages appear immediately instead of waiting for a second reactive fetch.

10. **[#14468 — Add LiteLLM provider with auto model discovery](https://github.com/anomalyco/opencode/pull/14468)**  
    Still open after 5 months. Auto-discovers models from a LiteLLM proxy. High interest for teams using a centralized proxy layer — this has been a long-requested bridge.

---

## Feature Request Trends

- **Pasted text expansion**: #8501 (206 👍) — users want to expand `[Pasted ~1 lines]` to edit or review the full paste. The top-voted request.
- **Skill/model invocation control**: #34498 — adopting `disable-model-invocation: true` from Claude Code; a key safety pattern for automated skill scripts.
- **V2 prompt enhancements**: #34497 (file attachments in V2) and #34488 (V2 reasoning effort control) — the new layout is still catching up to feature parity.
- **Session title skippable**: #33140/PR #33148 — skip title generation for slow local models.
- **Working directory switching**: #23358 — `/cd` slash command to change directory at runtime without restart.
- **Copy-to-clipboard in CLI**: #35885 — a small UX polish for terminal code block rendering.

---

## Developer Pain Points

- **Provider compatibility gaps**: Consistently the top source of friction — Gemma 4 streaming tool calls (#20995), context overflow error parsing (#35918), and FreeModel returning blank responses (#31409). Each involves a subtle mismatch between OpenCode's expected API contract and real-world provider behavior.
- **Desktop startup reliability**: Two distinct freeze/crash regressions reported this week (#35294, #35901) — network-dependent startup fetches and Solid.js signal loops on Windows. This erodes trust in the desktop distribution.
- **"Preparing write..." stalls**: #11112 remains unresolved since January — a core tool-execution path that blocks users from completing any write operation.
- **Privacy surprise**: #10416 reveals that session title generation makes an outbound network call even for local-only setups. This is a transparency and trust issue for self-hosters.
- **V2 layout regressions**: At least three issues (#28686, #30656, #30329) report missing controls or broken refresh behavior in the new layout. The V2 UI is being actively pushed but needs more stabilization work.

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi Community Digest — 2026-07-09

## Today's Highlights
A wave of **34 closed issues and 24 PRs** in the last 24 hours signals a major cleanup sprint. The core team is aggressively triaging compaction bugs (issues #6424, #6425, #6426), fixing streaming regressions for Gemini proxy users (#6414), and stabilizing the clipboard on Linux/X11 Bun binaries (#6418). Meanwhile, community feature requests are converging on two themes: **ephemeral model switching** (#5263) and **better compaction/summary chunking** for long sessions.

## Releases
No new releases in the last 24 hours. Latest stable remains 0.83.0.

## Hot Issues (10 Noteworthy)

1. **#5501** [CLOSED] — **Tolerate extra keys on `edits[]` items**  
   *Why it matters:* A pragmatic fix that relaxes strict JSON schema validation on edit tool items. Models frequently append stray keys (e.g., `newText_strip`, `newText_2`) after long `newText` blocks. Dropping `additionalProperties: false` on inner schemas while retaining it on the outer schema prevents silent failures. *Community:* 11 comments, but 0 upvotes — suggests a niche but critical reliability fix.

2. **#6204** [CLOSED] — **`mimo-v2-omni` is a ghost model on all three Xiaomi MiMo Token Plan providers**  
   *Why it matters:* A model catalog bug that produces misleading `400 Param Incorrect` errors. The bundled catalog lists `mimo-v2-omni` under all three regional providers, but those endpoints don't serve it. *Community:* 7 comments, no emotional reaction — likely considered a straightforward metadata fix.

3. **#5263** [OPEN] — **Make in-session model/thinking-level changes ephemeral by default**  
   *Why it matters:* Highly upvoted (+6) feature request addressing a persistent UX friction: switching models mid-session currently persists globally, causing surprises on the next session. Proposes a "Default model" entry in `/settings` and ephemeral in-session overrides. *Community:* 5 comments, continued discussion through July 8.

4. **#6206** [OPEN] — **Clamping to context window prevents artificial context limits**  
   *Why it matters:* A regression from a compaction fix (#5595) that now clamps `max_tokens` to the reported context window, blocking users who want to artificially limit output tokens (e.g., for cost control). *Community:* 5 comments, no upvotes — but a clear UX regression.

5. **#5886** [OPEN] — **AgentSession settlement/continuation and assistant-tail lifecycle bugs**  
   *Why it matters:* A meta-issue filed by mitsuhiko (prominent contributor) documenting a class of bugs where post-run agent logic tries to continue from a transcript that no longer matches the session state. +2 upvotes. *Community:* 4 comments, but references scattered bugs.

6. **#6406** [CLOSED] — **Read-only `~/.pi/agent` fails every credential read**  
   *Why it matters:* A design flaw: Pi creates a lock file even for reads, making the credential system unusable on read-only filesystems. *Community:* 2 comments, but flagged as `no-action` — suggests the fix is deferred.

7. **#6421** [CLOSED] — **Anthropic OAuth requests need Claude Agent billing marker**  
   *Why it matters:* OAuth with Claude Max accounts fails with `400 "You're out of extra usage"` even with valid tokens. The fix likely involves adding a billing marker to OAuth requests. *Community:* 2 comments, triaged quickly.

8. **#6414** [CLOSED] — **`streamProxy` drops `ToolCall.thoughtSignature` — Gemini multi-turn tool calls fail**  
   *Why it matters:* A critical bug for Gemini proxy users: streamProxy doesn't forward `thoughtSignature`, causing `400 INVALID_ARGUMENT` on the second tool turn. *Community:* 3 comments, closed same day — implies a quick fix.

9. **#6429** [CLOSED] — **OpenAI Responses sends `max_output_tokens=1` after compaction**  
   *Why it matters:* Auto-compaction corrupts the token limit to 1, causing repeated 400 errors on `openai/gpt-5.5`. *Community:* 1 comment, but affects all OpenAI Responses users with long sessions.

10. **#6431** [CLOSED] — **Bun fetch socket drop not classified as retryable**  
    *Why it matters:* Transient network drops on the Bun runtime cause Pi to terminate without retrying. The `"socket connection was closed"` error is not in the retry classification list. *Community:* 1 comment, closed same day.

## Key PR Progress (10 Important)

1. **#6430** [CLOSED] — **Fix fork menu allowing double-select**  
   *Summary:* Prevents multiple session forks when user presses Enter while fork is still running. Closes selector before `await runtimeHost.fork()`. Essential for UX stability.

2. **#6427** [OPEN] — **feat(coding-agent): Add prompt cache miss tracking**  
   *Summary:* Detects prompt cache misses per turn by comparing cache reads against previous request's prompt tokens. New `/session` command shows detailed cache stats. By mitsuhiko.

3. **#6418** [CLOSED] — **Fix native clipboard in Bun release binary**  
   *Summary:* Two fixes: copies `.node` files into clipboard package for Bun packaged releases; adds `xclip` fallback on X11. Fixes #6250 (Linux/X11 silent clipboard failure).

4. **#6417** [CLOSED] — **feat(agent): Support custom metadata in JSONL session headers**  
   *Summary:* Adds optional `metadata?: Record<string, unknown>` to v3 JSONL session header for the new harness module. Accepted by `create()`, returned in `JsonlSessionMetadata`, inherits on fork.

5. **#6413** [CLOSED] — **feat(coding-agent): Show git info in local version**  
   *Summary:* Appends git commit/branch/tag to displayed version when running Pi directly from repo. Closes #6412.

6. **#6169** [CLOSED] — **Disable padding for assistant messages**  
   *Summary:* TUI rendering fix for message alignment. Closes #6168.

7. **#6026** [CLOSED] — **fix(tui): Stabilize working status row**  
   *Summary:* Ongoing TUI stability improvements. References #5825.

8. **#5846** [CLOSED] — **fix(tui): Stabilize streaming code fence rendering**  
   *Summary:* Fixes flickering/truncation of code blocks during streaming output. Closes #5825.

9. **#5711** [CLOSED] — **feat(coding-agent): Add extension prompt guideline API**  
   *Summary:* Allows extensions to contribute prompt guidelines. Verfied working. Closes #5710.

10. **#4775** [CLOSED] — **Export image resize utilities**  
    *Summary:* Exports `resizeImage` and related utilities for use in other tools/extensions.

## Feature Request Trends

Three dominant patterns emerge from recent issues:

1. **Ephemeral, session-scoped configuration** — Issue #5263 (+6 votes) leads a push for non-persistent model/temperature changes. Users want to switch models mid-session without corrupting global defaults.

2. **Better compaction and summarization** — Issues #6424, #6425, #6426 (all by Blue-B, closed same day) describe identical failure modes: threshold auto-compaction can leave unfinished work idle, large compactions need chunking and backoff around summary calls, and switching to a smaller context model should pre-compact before the next request.

3. **Provider extensibility** — Issue #6420 requests adding Novita AI as a built-in provider. Issue #6422 surfaces Fable OAuth 429 rate limits. Issue #6419 requests stable generated image model formatting. The pattern suggests growing demand for third-party provider support.

## Developer Pain Points

1. **Compaction fragility on long sessions** — 3 separate issues (#6424, #6425, #6426) filed by the same developer on the same day describe cascading failures when sessions grow very large: summary requests themselves overflow, threshold compaction interrupts unfinished work, and model switches don't trigger pre-compaction.

2. **UI freezing and unresponsiveness** — Issue #6423 reports a frozen loading animation / input field with "certain chance" of occurrence. No reproduction steps yet, but flagged as bug + untriaged.

3. **OAuth token and rate-limit complexity** — Issues #6421 (Anthropic billing marker), #6422 (Fable 429 rate limits), and #6204 (ghost MiMo model) reveal growing pain in multi-provider OAuth support.

4. **Clipboard fragmentation across platforms** — Issue #6250 (Linux/X11 silent failure) had a 3-day turnaround to fix (#6418), but highlights that clipboard integration remains fragile across operating systems, especially in Bun compiled binaries.

5. **Read-only filesystem incompatibility** — Issue #6406: Pi locks credential files even for reads, making it unusable on read-only/immutable filesystems. Flagged `no-action`, implying no immediate fix.

*Sources: All data from [github.com/badlogic/pi-mono](https://github.com/badlogic/pi-mono), specifically earendil-works/pi repository, issues and pull requests updated between 2026-07-08 and 2026-07-09.*

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-07-09

## Today's Highlights
A new patch release v0.19.8 lands with CLI serve environment isolation and improved documentation. The community is actively discussing an RFC for multi-workspace daemon support (#6378, 19 comments), while several important bugs around memory staleness, session chain corruption, and Anthropic model compatibility are being addressed in parallel with promising PRs.

## Releases
- **v0.19.8** (stable): Adds WeCom channel documentation and `feat(cli): Add serve env isolation and total admission` by @doudouOUC. This improves security isolation for hosted deployments.
- **v0.19.7-nightly** / **v0.19.6-preview.0**: Both contain only the WeCom docs fix; no feature changes.

## Hot Issues
1. **#6378 — RFC: Support multiple workspaces in one qwen serve daemon** (19 comments)  
   *Open, P2 feature request.* Proposes shifting from the current 1 daemon = 1 workspace model to multi-tenant serving while preserving backward compatibility. High community engagement; likely to shape future daemon architecture.  
   [Issue](https://github.com/QwenLM/qwen-code/issues/6378)

2. **#6334 — Extensions install failure on Windows** (5 comments)  
   *Open bug, need-information.* User reports Git-based extension downloads fail consistently, but denies network issues. Needs OS-level debugging.  
   [Issue](https://github.com/QwenLM/qwen-code/issues/6334)

3. **#6384 — Hard limit: 0 when env-configured model reserves full default context** (5 comments)  
   *Open P2 bug.* Critical context window miscalculation prevents any request from being sent — prompts with ~3k estimated tokens hit a hard limit of 0 after compression. High impact for users with custom model configurations.  
   [Issue](https://github.com/QwenLM/qwen-code/issues/6384)

4. **#6505 — Subagent reasoning loop repeats identical tool calls indefinitely** (4 comments, closed)  
   *Closed P2 bug with welcome-pr.* Subagent gets stuck in infinite tool-call loop while main agent's LoopDetectionService doesn't fire. Fixed; important for multi-agent reliability.  
   [Issue](https://github.com/QwenLM/qwen-code/issues/6505)

5. **#6414 — VS Code extension fails to connect to Qwen agent** (4 comments)  
   *Open bug, need-information.* Connection failures in VS Code integration; likely environment/network related. Affects IDE users heavily.  
   [Issue](https://github.com/QwenLM/qwen-code/issues/6414)

6. **#6529 — Inject background tasks/cron status into Stop/SubagentStop hooks** (3 comments, closed)  
   *Closed P2 feature request.* Background job awareness for hook scripts — allows hook scripts to know about async work in progress. Part of hooks/events roadmap.  
   [Issue](https://github.com/QwenLM/qwen-code/issues/6529)

7. **#6488 — MessageDisplay hook for mid-turn streaming** (3 comments)  
   *Open P2 feature request.* Currently no hook fires during streaming (only after completion). Request for incremental reply observation. Has a corresponding PR (#6489).  
   [Issue](https://github.com/QwenLM/qwen-code/issues/6488)

8. **#6503 — Slash completion: alias overrides name match after execution** (2 comments, closed)  
   *Closed P2 bug.* Fix for #5577 regression: `/clear`'s `reset` alias appears before `/resume` after running `/clear`.  
   [Issue](https://github.com/QwenLM/qwen-code/issues/6503)

9. **#6487 — Memory index stale after /remember; content lost on compaction** (2 comments)  
   *Open P2 bug.* Two independent memory degradation mechanisms: stale index after remember, and content loss during compaction. Critical for long-running sessions.  
   [Issue](https://github.com/QwenLM/qwen-code/issues/6487)

10. **#6519 — Anthropic Claude 4.8+ fails with deprecated temperature parameter** (1 comment)  
    *Closed P1 bug.* Sending deprecated `temperature` param causes 400 error on Claude Opus 4.8+. High priority fix.  
    [Issue](https://github.com/QwenLM/qwen-code/issues/6519)

## Key PR Progress
1. **#6525 — Cursor-paged transcript replay endpoint** (open)  
   Adds `GET /session/:id/transcript` for active sessions with cursor pagination. Enables efficient history browsing without full JSONL loads.  
   [PR](https://github.com/QwenLM/qwen-code/pull/6525)

2. **#6540 — Session owner index for workspace runtimes** (open)  
   Registry-owned live session owner index for daemon workspaces; resolves stale bridge entries and improves session lifecycle management.  
   [PR](https://github.com/QwenLM/qwen-code/pull/6540)

3. **#6535 — Isolated run mode via create_sub_session tool** (open, in-review)  
   Daemon-only tool spawning fresh sub-sessions for cron tasks, preventing context accumulation across scheduled fires.  
   [PR](https://github.com/QwenLM/qwen-code/pull/6535)

4. **#6489 — MessageDisplay hook for mid-turn streaming** (open)  
   Adds incremental streaming hook event, solving the gap in #6488. Enables real-time reply observation in CLI, ACP, and IDE.  
   [PR](https://github.com/QwenLM/qwen-code/pull/6489)

5. **#6259 — V2 daemon session artifact persistence** (open)  
   Persists session artifacts (tombstones, snapshots, pins) across daemon restarts. Follow-up to #5895 for production durability.  
   [PR](https://github.com/QwenLM/qwen-code/pull/6259)

6. **#6495 — Webhook-triggered channel tasks** (open)  
   External webhook events trigger response generation via daemon; response is proactively delivered to chat targets. Extends channel integration.  
   [PR](https://github.com/QwenLM/qwen-code/pull/6495)

7. **#6539 — Chat payload diagnostics** (open)  
   Adds preflight rejection reason logging and opt-in payload inspection via `QWEN_CHANNEL_DEBUG_PAYLOAD` for debugging routing issues.  
   [PR](https://github.com/QwenLM/qwen-code/pull/6539)

8. **#6541 — Configurable vision bridge timeout + retry** (open)  
   Makes 30s image-transcription timeout configurable (`visionBridgeTimeoutMs`) and adds single retry with fresh budget.  
   [PR](https://github.com/QwenLM/qwen-code/pull/6541)

9. **#6459 — Configurable background memory agent timeouts** (closed)  
   Adds `memory.agentTimeoutMinutes` for four background agents; setting `0` disables timeout. Responds to #6308.  
   [PR](https://github.com/QwenLM/qwen-code/pull/6459)

10. **#6497 — Refresh instructions after /remember** (open)  
    Fixes stale memory context by reloading hierarchical memory and refreshing system instructions after `/remember` completes. Addresses part of #6487.  
    [PR](https://github.com/QwenLM/qwen-code/pull/6497)

## Feature Request Trends
- **Multi-tenant daemon capabilities**: Strong demand for supporting multiple workspaces in a single `qwen serve` daemon (#6378), with sub-session isolation (#6535) and persistent artifact management (#6259).
- **Background task visibility**: Users want hooks to be aware of background tasks and cron jobs when the agent stops (#6529), and mid-turn streaming observation via `MessageDisplay` hooks (#6488).
- **Channel expansion**: Community requests for WeCom support (just added), DingTalk, Feishu, and QQ Bot group message handling with webhook-triggered tasks (#6495, #6457, #6538, #6539).
- **Memory system improvements**: Configurable AutoMemory timeouts (#6308, #6459), fix for stale memory index after `/remember` (#6487, #6497), and worktree memory isolation (#6449).
- **Advisor/read-only features**: Request for a read-only second-opinion reviewer that inspects session context and returns structured guidance before major work or when progress stalls (#6542).

## Developer Pain Points
- **Context window bugs**: Multiple reports of incorrect token counting leading to "hard limit: 0" errors and silent history truncation (#6384, #6501). Token budget logic still fragile.
- **Memory degradation over long sessions**: Two independent memory loss mechanisms in long-running sessions — stale index after `/remember` and content loss during compaction (#6487). Critically impacts production usage.
- **Model API compatibility**: Anthropic Claude 4.8+ deprecated `temperature` parameter causes 400 errors (#6519). Third-party model adapters remain fragile.
- **IDE integration reliability**: Deferred IDE startup shows stale failure states after late connection success (#6507), VS Code connection failures (#6414), and Windows extension install failures (#6334).
- **Channel message routing**: Hardcoded 30s timeouts on vision bridge image interpretation (#6524, #6541), lack of payload diagnostics for debugging channel preflight rejections (#6538).

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI Community Digest — July 9, 2026

## Today's Highlights

The v0.8.68 stabilization sprint dominates activity with aggressive progress on performance micro-optimizations, fleet profile schema, and Turn Inspector features. Maintainer Hmbown merged 13 PRs in a single day, many harvested from community contributors, signaling a push toward release candidate quality. A critical polling regression (#4097) that caused parent agents to burn tokens waiting for sub-agents receives a three-layer fix.

## Releases

No new releases in the last 24 hours.

---

## Hot Issues

### 1. [#4032 Codewhale not following the constitution](https://github.com/Hmbown/CodeWhale/issues/4032)
**26 comments** — The most active discussion. An agent consistently ignores user-provided scripts and writes its own temporary scripts for tasks, then justifies the behavior when challenged. Community reaction suggests concerns about agent alignment with user intent, not just capability.

### 2. [#4092 v0.8.68 execution board: lane order, dependencies, and agent protocol](https://github.com/Hmbown/CodeWhale/issues/4092)
**20 comments** — The canonical entry point for all v0.8.68 milestone work. Acts as the single source of truth for agent packet management, defining lane labels and triage protocol across all milestone issues.

### 3. [#4097 Parent model burns turns with peek+sleep polling loop](https://github.com/Hmbown/CodeWhale/issues/4097)
**1 comment** — A #3183 regression where parent agents enter wasteful `peek → sleep → peek` loops instead of passively waiting for sub-agent completion. Community contributor Mr-Moon121 identified this persists through v0.8.67 despite earlier closure. Already has a PR fix (#4229).

### 4. [#4217 subagents.v1.json grows unbounded](https://github.com/Hmbown/CodeWhale/issues/4217)
**1 comment** — A long-running session produces ~300,000 lines in state storage with no cleanup mechanism. User yekern reports manual file emptying is the only workaround. Points to a missing time/state-based eviction policy in the sub-agent record system.

### 5. [#4208 TUI copy-paste polluted with box-drawing Unicode decorations](https://github.com/Hmbown/CodeWhale/issues/4208)
**1 comment** — Terminal copy operations include decorative characters (`╎ ▎ ● │ ┃`) that break pasted content. A straightforward UX regression that impacts developer workflow on any copy-from-terminal use case.

### 6. [#4202 execshell Python encoding mismatch on Windows (UTF-8 → GBK)](https://github.com/Hmbown/CodeWhale/issues/4202)
**1 comment** — Windows users in Conda environments experience encoding corruption when executing Python scripts. The TUI correctly uses UTF-8, but executed shells default to GBK. User w1w218 includes a thorough root-cause analysis from CodeWhale's own self-diagnosis.

### 7. [#4227 Help JayBeest keep up with the CodeWhale tsunami](https://github.com/Hmbown/CodeWhale/issues/4227)
**2 comments** — A feature request for an onboarding workflow that automates environment setup (pull latest, `cargo build`, run tests). Community member JayBeest, an active contributor, self-identifies the need given the project's 10+ PRs/day velocity.

### 8. [#4093 Fleet setup modal is provider-scoped instead of role/profile roster editor](https://github.com/Hmbown/CodeWhale/issues/4093)
**3 comments** — A release-blocker targeting v0.8.68. The fleet setup UI behaves like a model picker when it should be a role/profile roster editor, blocking confident model-route assignment across providers.

### 9. [#4196 Agent-callable verify/critique tool](https://github.com/Hmbown/CodeWhale/issues/4196)
**1 comment** — A feature request for a self-critique tool that agents can optionally call to spend extra test-time compute on adversarial self-review before claiming completion. A model-decided alternative to forced multi-agent review.

### 10. [#4149 Finish parking_lot migration at hot lock sites](https://github.com/Hmbown/CodeWhale/issues/4149)
**0 comments** — Part of the v0.8.68 performance wave. Remaining `std::sync` lock sites at app-server runtime and runtime_threads map need migration to `parking_lot` for reduced contention.

---

## Key PR Progress

### 1. [#4229 subagent: wait primitive + peek throttle + anti-polling prompts](https://github.com/Hmbown/CodeWhale/pull/4229)
A three-layer fix for the #4097 polling regression: harvested anti-polling rules from community PR #4098, added a `wait` primtive replacing sleep loops, and dropped peek polling entirely for sub-agents. A critical architecture change for token efficiency.

### 2. [#4222 Ctrl-O opens a turn-level Turn Inspector](https://github.com/Hmbown/CodeWhale/pull/4222)
Replaces the single-cell Activity Detail pager with a full turn-level overview showing in-flight or latest completed turn context. `v`/`Alt-V` remain for raw detail access.

### 3. [#4223 Export compact turn handoff from Turn Inspector](https://github.com/Hmbown/CodeWhale/pull/4223)
Adds `/export turn` command that copies a pasteable Markdown handoff of current/latest turn to system clipboard. Built on the Turn Inspector infrastructure from #4222.

### 4. [#4226 Enrich Turn Inspector timeline](https://github.com/Hmbown/CodeWhale/pull/4226)
Adds a numbered timeline classifying prompts, reads/searches, edits, verifier runs, assistant results, and checkpoint state — making agent activity sequence observable from the TUI.

### 5. [#4221 Extract Activity Detail helpers into ui/activity_detail.rs](https://github.com/Hmbown/CodeWhale/pull/4221)
Behavior-preserving code extraction from the ~13.2k-line `ui.rs` into a dedicated submodule. Foundational infrastructure work enabling the Turn Inspector lane.

### 6. [#4216 Wave-A micro-optimizations](https://github.com/Hmbown/CodeWhale/pull/4216)
Batch of mechanical, behavior-identical performance fixes covering three issues (#4153, #4157, #4151): remove collect-before-join allocations, cache hot priority lookups, and consolidate redundant data structures. No behavioral changes.

### 7. [#4219 Hot linear scans → map lookups](https://github.com/Hmbown/CodeWhale/pull/4219)
Replaces O(n) Vec/slice linear scans with O(1) map lookups at three hot sites where amortized map building is justified. Behavior-identical performance pass.

### 8. [#4215 LLM can start MCP servers from chat context](https://github.com/Hmbown/CodeWhale/pull/4215)
Harvested from community contributor bistack. Adds `start_mcp_server` tool supporting both stdio (local command) and HTTP (remote URL) transports, enabling dynamic MCP server initialization from conversation context.

### 9. [#4224 Frame v/Alt-V as raw leaf detail](https://github.com/Hmbown/CodeWhale/pull/4224)
Clarifies the UI distinction between Ctrl+O (Turn Inspector) and `v`/`Alt+V` (raw detail for the selected tool/card/leaf) with retitled pager headers. UX clarity improvement.

### 10. [#4228 Avoid lowercase allocations for ASCII comparisons](https://github.com/Hmbown/CodeWhale/pull/4228)
Replaces five ASCII-only lowercase equality checks with `eq_ignore_ascii_case` across hook mode matching, tool registry resolution, and doctor endpoint classification. Adds regression test coverage for uppercase inputs.

---

## Feature Request Trends

**Agent self-review and quality gates** — Multiple issues propose giving agents introspective capabilities: a self-critique tool (#4196), configurable reasoning-effort tiers for Fleet roles (#4195), and the already-implemented Turn Inspector (#4222, #4223). The trend is toward test-time compute the agent chooses to spend, not forced multi-agent orchestration.

**State management and persistence** — The unbounded subagent state file (#4217) and encoding persistence (#4202) point to growing pains as sessions span days/weeks. Expect cleanup policies and serialization hardening in upcoming releases.

**Developer onboarding velocity** — Issue #4227 explicitly recognizes the 10+ PRs/day pace creates onboarding friction. Automated dev environment setup workflows are a direct request from active community contributors.

---

## Developer Pain Points

**Polling waste drains token budgets** — The #3183 regression (#4097) highlights that parent-subagent communication remains fragile. Token waste during idle polling is a top concern among power users who monitor costs.

**TUI copy-paste pollution** — Unicode box-drawing characters in copied text (#4208) is a quality-of-life regression that directly impacts daily workflow. Low complexity but high annoyance.

**Windows encoding mismatch** — Python execution in Conda environments (#4202) reveals platform-specific shell encoding gaps. Windows users face configuration overhead that macOS/Linux users don't.

**State file bloat on long-running sessions** — The unbounded `subagents.v1.json` growth (#4217) forces manual maintenance. Power users who keep sessions open for weeks need automatic eviction or rotation policies.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*