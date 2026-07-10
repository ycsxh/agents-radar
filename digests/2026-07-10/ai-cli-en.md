# AI CLI Tools Community Digest 2026-07-10

> Generated: 2026-07-10 03:56 UTC | Tools covered: 9

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

# Cross‑Tool AI CLI Comparison Report  
**Date:** 2026-07-10  
**Coverage:** Claude Code, OpenAI Codex, Gemini CLI, GitHub Copilot CLI, Kimi Code CLI, OpenCode, Pi, Qwen Code, DeepSeek TUI (CodeWhale)

## 1. Ecosystem Overview
The AI CLI tools landscape in mid‑2026 is defined by rapid iteration, deep multi‑agent orchestration, and a growing demand for cross‑tool standards like `AGENTS.md`. Each tool ships almost daily, with a clear split between vendor‑aligned offerings (Anthropic, OpenAI, Google, GitHub, Moonshot, Alibaba) and community‑driven, multi‑provider clients (OpenCode, Pi, DeepSeek TUI). Security, trust, and permission models are under intense scrutiny as subagent spawning becomes the norm. The community’s voice is loudest around reliability, transparent model behaviour, and reducing token overhead, signalling a maturation from “capability” to “dependability” in coding agents.

## 2. Activity Comparison (2026-07-10)

| Tool | Hot Issues Count | Key PRs Count | Release Status |
|------|-----------------|---------------|----------------|
| **Claude Code** | 10 | 3 (docs/fixes) | v2.1.206 shipped |
| **OpenAI Codex** | 10 | 10 (architecture, identity, history) | rust‑v0.144.0 + hotfix v0.144.1 |
| **Gemini CLI** | 10 | 10 (security, AGENTS.md, fixes) | v0.52.0‑nightly.20260710 |
| **GitHub Copilot CLI** | 10 | 0 (internal merge) | v1.0.70 + pre‑release v1.0.70‑0 |
| **Kimi Code CLI** | 2 | 3 (interop, subprocess, formatting) | No release |
| **OpenCode** | 10 | 10 (V2 plugins, fork, UI) | v1.17.18 / .17 / .16 (3 patch releases) |
| **Pi** | 10 | 10 (thinking levels, OAuth, retries) | v0.80.6 (feature) |
| **Qwen Code** | 10 | 10 (multi‑workspace, preconditions) | v0.19.8‑nightly |
| **DeepSeek TUI** | 10 | 10 (release prep, providers, perf) | v0.8.68 in preparation, no publish |

*Note: “Hot Issues” and “Key PRs” reflect the top items highlighted in each tool’s community digest; actual daily totals are higher.*

## 3. Shared Feature Directions

Requirements emerging across multiple communities:

- **Universal agent instruction files (`AGENTS.md`)**  
  *Tools:* Claude Code (#6235 with 4,344 👍), Gemini CLI (PR #28240), Kimi Code (PR #2487 also loading `CLAUDE.md`), Qwen Code (implicit multi‑workspace needs).  
  *Need:* A single Markdown file that works across Claude Code, Gemini, Kimi, Cursor, and others, eliminating configuration fragmentation.

- **Subagent observability and control**  
  *Tools:* Gemini CLI (subagent lies about success, hangs), Codex (GPT‑5.6 Sol hides routing controls #31814), Qwen Code (demand for real‑time traces), DeepSeek (workflow gates, Fleet lanes).  
  *Need:* Visibility into subagent routing, execution traces, and the ability to pause/redirect/approve subagent decisions.

- **Multi‑account and profile switching**  
  *Tools:* Claude Code (#18435 Desktop, #20131 CLI), indirectly Copilot CLI (enterprise policy #1595).  
  *Need:* Easy toggling between work/personal accounts, billing profiles, and API keys without re‑authentication.

- **Token and context efficiency**  
  *Tools:* OpenCode (“ask mode” to skip tools #1573), Copilot CLI (configurable system prompt to cut 20K token overhead #2627), Pi (context‑window clamping #6206), Claude Code (oversized `CLAUDE.md` trimming #v2.1.206).  
  *Need:* Lightweight interaction modes, smarter context management, and defence against unintentional token waste.

- **Permission inheritance and security**  
  *Tools:* Claude Code (workflow subagents ignore settings #73633), Gemini CLI (subagents run despite being disabled #22093, critical RCE fix #28319), OpenCode (per‑session auto‑accept toggle #21578).  
  *Need:* Predictable, inheritable permission models that span parent agents, subagents, and MCP servers.

- **MCP ecosystem integration**  
  *Tools:* Codex (expose MCP server prompts as slash commands #8342), Gemini CLI (cross‑server resource confusion #28143), Claude Code (MCP plugin examples fixed).  
  *Need:* Deeper, more reliable MCP support, including prompt exposure, tool routing, and trust dialogs.

## 4. Differentiation Analysis

| Tool | Primary Focus | Target User | Distinguishing Technical Approach |
|------|---------------|-------------|-----------------------------------|
| **Claude Code** | Deep Anthropic integration, memory files, remote control | Anthropic ecosystem developers | `CLAUDE.md` files, strong model‑specific advisor, rich slash commands |
| **OpenAI Codex** | Enterprise‑grade multi‑model agent, workload identity, paginated history | GPT‑centric professional teams | Workload identity, paginated thread DB, multi‑agent V2, skills migration |
| **Gemini CLI** | Security‑first, workspace trust, Vertex AI integration | Google Cloud / Vertex AI users, security‑conscious devs | Zero‑click RCE hardening, trust dialogs for hooks, `GEMINI.md` + new `AGENTS.md` |
| **GitHub Copilot CLI** | GitHub ecosystem binding, enterprise policies, sandboxing | GitHub enterprise customers | Model switching with Copilot plans, proxy resilience, `/refine` command |
| **Kimi Code CLI** | Lightweight, cross‑tool compatibility, enterprise SSL | Moonshot AI users, especially in restricted corporate networks | SSL bypass option, seamless loading of `CLAUDE.md`/`AGENTS.md` |
| **OpenCode** | Multi‑provider open client, V2 plugin architecture, desktop UX | Developers wanting provider freedom and extensibility | V2 tool plugins, session forking, Solid.js desktop, cost transparency |
| **Pi** | Max configurability, thinking levels, SDK/RPC extensibility | Power users, tool builders, multi‑model experimenters | `max` thinking tier, strict‑tools grammar debate, message‑anchored tool loading |
| **Qwen Code** | Multi‑workspace daemon, scheduled tasks, Asian‑market i18n | Alibaba/Qwen users, monorepo teams, Chinese‑speaking devs | One daemon for multiple repositories, cron‑gated preconditions, JetBrains ACP |
| **DeepSeek TUI** | Terminal‑native orchestration, lane‑based workflows, mobile support | TUI enthusiasts, autonomous agent pipeline designers | Fleet/Workflow/Lanes execution model, `parking_lot` perf, Termux/Android builds |

## 5. Community Momentum & Maturity

- **Largest, most vocal communities:** Claude Code and OpenAI Codex dominate in issue volume and upvote counts (e.g. Claude Code’s `AGENTS.md` request with 4,344 👍). Both are backed by major AI providers and attract broad developer bases. GitHub Copilot CLI shows strong enterprise traction but struggles with policy and platform friction.
- **Rapidly iterating projects:** OpenCode, Pi, and DeepSeek TUI demonstrate exceptional velocity. OpenCode shipped three patch releases in one day while pushing 10 V2‑relevant PRs. DeepSeek TUI is on the verge of v0.8.68 with a flurry of provider, performance, and platform PRs. Pi consistently delivers user‑facing features like `max` thinking and Grok OAuth within days of discussion.
- **Platform‑specific momentum:** Gemini CLI is heavily investing in security hardening, likely due to enterprise/GCP requirements. Qwen Code is aggressively building out multi‑workspace daemon capabilities, indicating strong internal/monorepo adoption. Kimi Code remains smaller but is actively closing critical gaps (SSL, subprocess stability).
- **Maturity indicators:** The presence of formal V2 architecture rewrites (OpenCode), lane‑based execution models (DeepSeek), and workload identity (Codex) signals that the ecosystem is moving from simple chat‑driven coding to production‑grade agent orchestration.

## 6. Trend Signals

1. **Cross‑tool standards are no longer optional** – The groundswell for `AGENTS.md` across Claude Code, Gemini, Kimi, and others shows that developers resent vendor‑specific configuration silos. Tools that support shared instruction files will gain an interoperability advantage.

2. **Subagent orchestration is the new frontier** – Every tool is grappling with subagent visibility, permission, and lifecycle management. Observability (traces, status panels) and explicit gating (workflow lanes, approve/block semantics) are becoming table stakes for multi‑agent workflows.

3. **Security and trust must be built‑in, not bolted‑on** – Zero‑click RCE patches in Gemini, permission inheritance bugs in Claude Code, and the demand for per‑session toggles in OpenCode highlight that developers expect agentic tools to be safe by default. Tools that fail this test will lose enterprise mindshare.

4. **Token economy is a user experience concern** – The push for ask‑only modes, configurable system prompts, and smarter compaction reflects a growing awareness that every interaction costs money and time. Tools that provide fine‑grained context control will be preferred for daily use.

5. **Enterprise features drive adoption but also friction** – Proxy support, SSL inspection bypass, multi‑account switching, and policy enforcement (Copilot CLI enterprise blocks) are critical for corporate environments, yet remain a persistent source of bugs and frustration. The tool that solves “works in my corp network” seamlessly will capture significant market share.

6. **Multi‑provider, multi‑model flexibility is a strong differentiator** – OpenCode, Pi, and DeepSeek TUI demonstrate that being model‑agnostic attracts a loyal user base. As new models emerge weekly, vendor‑agnostic clients are better positioned to offer “best model for the task” experiences.

**Reference value for developers and decision‑makers:** The current sprint across all tools is about making agents *reliable teammates*, not just impressive demos. Investing in tools that prioritise transparency, security, and cross‑tool compatibility will pay off as teams scale their AI‑assisted development workflows. The convergence on `AGENTS.md`, subagent controls, and permission models suggests these will become standard evaluation criteria within months.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills Community Highlights Report
**Data as of 2026-07-10** · Source: [`anthropics/skills`](https://github.com/anthropics/skills)

---

## 1. Top Skills Ranking
Based on community discussion volume (PR comments), these are the most-discussed skill contributions. All remain **open**.

| # | PR | Skill / Contribution | What It Does | Key Discussion | Status |
|---|----|----------------------|--------------|----------------|--------|
| 1 | [#1298](https://github.com/anthropics/skills/pull/1298) | **skill-creator eval fix** | Fixes `run_eval.py` reporting 0% recall for all skill descriptions; properly installs the eval artifact as a real skill, fixes Windows stream reading, trigger detection, and parallel workers. | Addresses the long-standing [#556](https://github.com/anthropics/skills/issues/556) where the description-optimisation loop optimises against noise. Community reproductions >10. | Open |
| 2 | [#514](https://github.com/anthropics/skills/pull/514) | **document-typography** | Prevents orphan words, widow paragraphs, and numbering misalignment in AI-generated documents. Proactive typographic quality control. | Highlights a subtle but widespread output problem: users rarely ask for good typography, yet it affects every generated document. | Open |
| 3 | [#538](https://github.com/anthropics/skills/pull/538) | **pdf case-sensitivity fix** | Corrects 8 case-sensitivity mismatches in `skills/pdf/SKILL.md` (e.g. `REFERENCE.md` → `reference.md`) that break on case-sensitive filesystems. | Sparked broader awareness of cross-platform file reference fragility in the skills ecosystem. | Open |
| 4 | [#486](https://github.com/anthropics/skills/pull/486) | **odt skill (OpenDocument)** | Create, fill, read, and convert ODT/ODS/ODF files. Covers LibreOffice document workflows and ISO-standard format support. | Fills a gap for open-source office formats; community interest in template-filling and ODT↔HTML conversion. | Open |
| 5 | [#210](https://github.com/anthropics/skills/pull/210) | **frontend-design clarity improvement** | Revises the frontend-design skill for better actionability and coherence, ensuring every instruction is actionable within a single conversation. | Debate on making skill instructions specific enough to steer behaviour without becoming brittle. | Open |
| 6 | [#83](https://github.com/anthropics/skills/pull/83) | **skill-quality-analyzer & skill-security-analyzer** | Two meta-skills: a 5-dimension quality evaluator and a security scanner for Claude Skills. | Addresses the growing need for automated skill vetting (see Issue [#492](https://github.com/anthropics/skills/issues/492)). | Open |
| 7 | [#541](https://github.com/anthropics/skills/pull/541) | **docx tracked-change collision fix** | Prevents `w:id` collisions between tracked changes and existing bookmarks in OOXML, which caused document corruption. | Revealed the subtleties of OOXML’s shared ID space; community appreciated the deep domain knowledge. | Open |
| 8 | [#539](https://github.com/anthropics/skills/pull/539) | **skill-creator YAML validation** | Warns on unquoted `description` fields containing YAML special characters (e.g. `:`) to prevent silent parsing failures. | Part of a cluster of skill-creator robustness fixes (with #361, #362) that the community is actively pushing. | Open |

---

## 2. Community Demand Trends
Analysis of the most-commented Issues reveals what the community urgently wants from the skills ecosystem.

- **Trust & Security Boundaries** ([#492](https://github.com/anthropics/skills/issues/492), 34 💬) — The top issue by far. Community skills are distributed under the `anthropic/` namespace, misleading users into granting elevated permissions as if they were official. The demand is for clear provenance, namespace separation, and security vetting tools.

- **Enterprise Sharing & Management** ([#228](https://github.com/anthropics/skills/issues/228), 14 💬; [#189](https://github.com/anthropics/skills/issues/189), 6 💬) — Users want org-wide skill libraries with direct sharing links, not manual file transfer. Duplicate skills from overlapping plugins (`document-skills` vs `example-skills`) are a pain point.

- **Skill-Creator Tooling Reliability** ([#556](https://github.com/anthropics/skills/issues/556), 12 💬; [#1169](https://github.com/anthropics/skills/issues/1169); [#1061](https://github.com/anthropics/skills/issues/1061)) — Widespread reports that `run_eval.py` is broken (0% recall), especially on Windows. The community wants a functional evaluation loop to actually optimize skill descriptions.

- **Agent State & Memory Skills** ([#1329](https://github.com/anthropics/skills/issues/1329), 9 💬) — A proposal for `compact-memory`, a symbolic notation for compact agent state, signals demand for skills that help agents manage long-running context efficiently.

- **Governance & Safety Patterns** ([#412](https://github.com/anthropics/skills/issues/412), 6 💬) — Interest in a skill that embeds policy enforcement, trust scoring, and audit trails directly into agent behaviour.

---

## 3. High-Potential Pending Skills
These open PRs with active discussion propose entirely new skills that may land soon.

| PR | Skill | Status | Why It Matters |
|----|-------|--------|----------------|
| [#514](https://github.com/anthropics/skills/pull/514) | `document-typography` | Open | Addresses a universal output-quality problem; no current alternative exists. |
| [#486](https://github.com/anthropics/skills/pull/486) | `odt` (OpenDocument) | Open | Expands the document skills beyond DOCX/PDF into open-source formats. |
| [#83](https://github.com/anthropics/skills/pull/83) | `skill-quality-analyzer` + `skill-security-analyzer` | Open | Directly answers the #1 community demand (Issue #492) for skill vetting. |
| [#723](https://github.com/anthropics/skills/pull/723) | `testing-patterns` | Open | Comprehensive testing philosophy, unit/integration/React patterns; fills a major gap. |
| [#1302](https://github.com/anthropics/skills/pull/1302) | `color-expert` | Open | Self-contained colour expertise (spaces, naming, accessibility); highly specialized and reusable. |
| [#1367](https://github.com/anthropics/skills/pull/1367) | `self-audit` | Open | Pre-delivery mechanical verification + reasoning audit; universal, model-agnostic QA gate. |

---

## 4. Skills Ecosystem Insight
**The community’s most concentrated demand is for robust, trustworthy infrastructure—fixing skill evaluation tooling, establishing security boundaries, and enabling enterprise sharing—before scaling the skill catalogue itself.**

---

# Claude Code Community Digest — July 10, 2026

## Today’s Highlights
**v2.1.206** shipped this morning with directory-path suggestions for `/cd`, a new `/doctor` check to trim oversized checked-in `CLAUDE.md` files, and automatic `git push` allowance in `/commit-push-pr`. The community’s strongest signal remains the **AGENTS.md** proposal (#6235), now at 4,344 👍 and 335 comments, while a critical Linux segfault on the 2.1.206 binary (#76241) demands an urgent hotfix.

---

## Releases

### v2.1.206
- `/cd` now suggests directory paths matching the behaviour of `/add-dir`.
- `/doctor` can propose slimming down checked-in `CLAUDE.md` files by removing content Claude already derives from the codebase.
- `/commit-push-pr` automatically allows `git push` to the repo’s configured remote.

---

## Hot Issues

1. **#6235 – Feature Request: Support `AGENTS.md`**  
   🏷️ enhancement, area:core, memory | 👍 4344 | 💬 335  
   A universal `AGENTS.md` standard is being adopted by Codex, Amp, Cursor, etc. The community pushes hard for Claude Code to support it alongside `CLAUDE.md` to improve cross-tool collaboration.  
   *Link:* [anthropics/claude-code#6235](https://github.com/anthropics/claude-code/issues/6235)

2. **#5088 – Account Disabled After Payment for Max 5x Plan**  
   🏷️ bug, area:cost, area:auth | 👍 59 | 💬 180  
   Users are locked out of both Claude Code and Claude.ai immediately after paying for a Max plan. A severe billing/auth flow bug that erodes trust in the subscription system.  
   *Link:* [anthropics/claude-code#5088](https://github.com/anthropics/claude-code/issues/5088)

3. **#18435 – Multi-Account Profile Support in Desktop App**  
   🏷️ enhancement, area:auth, area:ide | 👍 643 | 💬 126  
   Users need to switch between multiple Claude accounts (e.g., work/personal) without logging out and back in. Strong demand from professional teams.  
   *Link:* [anthropics/claude-code#18435](https://github.com/anthropics/claude-code/issues/18435)

4. **#73365 – Advisor “Unavailable” with Fable 5**  
   🏷️ bug, platform:windows, area:model | 👍 92 | 💬 48  
   The advisor tool returns “unavailable” consistently across sessions when using Fable 5 (Opus 4.8 main). A regression breaking an important reliability feature.  
   *Link:* [anthropics/claude-code#73365](https://github.com/anthropics/claude-code/issues/73365)

5. **#20131 – Multi-Account Profile Support (CLI)**  
   🏷️ enhancement, area:auth | 👍 96 | 💬 37  
   Similar to #18435, but for the CLI. Developers with separate billing and subscription accounts need seamless switching.  
   *Link:* [anthropics/claude-code#20131](https://github.com/anthropics/claude-code/issues/20131)

6. **#25128 – Drag & Drop Broken in VS Code Extension**  
   🏷️ bug, has repro, area:ide | 👍 43 | 💬 23  
   Drag-and-drop works in the terminal CLI but not in the VS Code chat panel, a regression since ~v2.1.6. Affects many IDE users’ workflows.  
   *Link:* [anthropics/claude-code#25128](https://github.com/anthropics/claude-code/issues/25128)

7. **#67609 – Advisor Fails on Large Transcripts**  
   🏷️ bug, area:model, area:core | 👍 34 | 💬 16  
   With `claude-fable-5`, the advisor returns `unavailable` whenever the transcript exceeds ~100K tokens. Invalidates the advisor for long sessions.  
   *Link:* [anthropics/claude-code#67609](https://github.com/anthropics/claude-code/issues/67609)

8. **#28379 – Slash Commands Not Supported in `/remote-control` UI**  
   🏷️ enhancement, area:claude-code-web | 👍 51 | 💬 11  
   When using the remote control web UI, typing `/clear`, `/compact`, etc. sends them as plain messages instead of executing commands. Breaks mobile/remote workflows.  
   *Link:* [anthropics/claude-code#28379](https://github.com/anthropics/claude-code/issues/28379)

9. **#64774 – Opus 4.8 Emits Unparseable Tool Calls**  
   🏷️ bug, area:model, api:anthropic | 👍 4 | 💬 6  
   `claude-opus-4-8` generates malformed tool calls at a ~1.5% rate, while Opus 4.7 and Sonnet 4.6 maintain 0%. A subtle model quality regression.  
   *Link:* [anthropics/claude-code#64774](https://github.com/anthropics/claude-code/issues/64774)

10. **#73633 – Workflow Subagents Ignore `settings.local.json` Permissions**  
    🏷️ bug, area:agents, area:permissions | 👍 5 | 💬 5  
    Multi-agent workflows (e.g., deep-research) do not inherit allowlisted tools from the project’s `.claude/settings.local.json`, causing repeated permission prompts.  
    *Link:* [anthropics/claude-code#73633](https://github.com/anthropics/claude-code/issues/73633)

---

## Key PR Progress

*Only 3 PRs were updated in the last 24 hours, all documentation or example fixes:*

- **#76029** – Fixes a `.mcp.json` example in the plugin-dev skill that incorrectly wrapped server configs in an `mcpServers` object (a `plugin.json` pattern).  
- **#76028** – Updates stale marketplace install instructions in `plugin-dev/README.md` (was `plugin-dev@claude-code-marketplace`, now aligns with documented naming).  
- **#76023** – Corrects a CI-detection hook example that used `-f` on a directory instead of `-d`, so the GitHub Actions branch would never activate.

---

## Feature Request Trends

- **Unified Agent Instruction Standard (`AGENTS.md`)** – Overwhelmingly the top request. Developers want a single Markdown file that works across Claude Code, Cursor, Amp, and other coding agents, reducing fragmentation.  
- **Multi-Account / Profile Switching** – Both Desktop (#18435) and CLI (#20131) users demand easy toggling between work, personal, and API billing accounts without constant re-authentication.  
- **Remote / Mobile UX Parity** – Slash commands should work via `/remote-control` (#28379), and session management (reordering groups, folder-linked groups) needs polish (#75856).  
- **Permission Inheritance & Security** – Workflows and subagents must reliably inherit project-specific allow rules (#73633, #61233) to avoid interruption fatigue.

---

## Developer Pain Points

- **Subscription & Auth Instability** – Accounts being disabled immediately after payment (#5088), Fable 5 missing from subscription mappings (#76237), and difficulty managing multiple accounts all contribute to an unreliable billing experience.  
- **Advisor Tool Unreliability** – The advisor feature consistently fails with “unavailable” on Fable 5, especially for longer transcripts (#73365, #67609), making it untrustworthy for critical work.  
- **IDE and Desktop Friction** – Drag-and-drop broken in VS Code (#25128), raw JSON instead of rich findings panel (#73939), no way to remove recent folders (#72181), and session-group drag reorder missing (#75856) degrade the GUI experience.  
- **Model Quality Regressions** – Opus 4.8 emits unparseable tool calls (#64774) and, anecdotally, ignores stop instructions (#76243). Users expect the latest models to be at least as reliable as their predecessors.  
- **Linux & Cross-platform Edge Cases** – Segfault on v2.1.206 with glibc 2.41 (#76241), Cowork requiring QEMU even with KVM (#74605), and TUI overwrites in Konsole (#76260) indicate a need for broader platform testing.

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-07-10

## Today’s Highlights
The Codex team shipped `rust-v0.144.0` with new usage‑limit reset credit selection and a `writes` app-approval mode, and immediately followed up with `rust-v0.144.1` to fix broken standalone installs and a missing `codex-code-mode-host` binary. Community attention is focused on credit‑reset failures, the Homebrew‑cask packaging regression, and unexpected MultiAgent V2 behaviour in GPT‑5.6 Sol sessions. Meanwhile, a flurry of PRs around workload identity, paginated thread history, and skills migration signals significant upcoming architecture changes.

## Releases

- **rust-v0.144.0**  
  - Usage‑limit reset credits now show type and expiration, allowing users to choose which credit to redeem.  
  - Added a `writes` app‑approval mode that permits declared read‑only actions but prompts for writes.  
  - MCP tools can now request authentication interactively.  
  - [Full changelog](https://github.com/openai/codex/releases/tag/rust-v0.144.0)

- **rust-v0.144.1** (hotfix)  
  - Fixed standalone installs failing when GitHub returns compact or reordered release metadata.  
  - Ensured macOS package installs expose the code‑mode host alongside `codex` executable.  
  - Kept code‑mode working when the companion host binary is unavailable by falling back gracefully.  
  - [Full changelog](https://github.com/openai/codex/releases/tag/rust-v0.144.1)

Alpha releases `0.145.0-alpha.1/2` and `0.144.0-alpha.4` were also published; no detailed notes are available.

## Hot Issues

1. **[#31906](https://github.com/openai/codex/issues/31906) — Homebrew cask is missing `codex-code-mode-host`; every command fails**  
   Impact: After upgrading to 0.144.0, users on macOS (Homebrew) see “failed to spawn code‑mode host” for all operations. 36 thumbs‑up and rapid comments reflect widespread disruption.  
   *Status: Open* – a critical packaging regression that `0.144.1` partially addresses.

2. **[#31606](https://github.com/openai/codex/issues/31606) — “Reset failed, did not apply and 1 reset is wasted”**  
   Impact: Users redeem a usage reset but the counter decrements without actually applying the reset, effectively burning a limited resource. 13 👍 and many confirmations show this hits Pro subscribers hard.  
   *Status: Open* – appears related to the new 0.144.0 reset selection feature.

3. **[#11747](https://github.com/openai/codex/issues/11747) — Add `/add-dir` slash command to add directories mid‑session**  
   Impact: Long‑standing feature request (35 👍) to dynamically expand workspace directories without restarting the CLI session. The high vote count signals a strong desire for more flexible session management.  
   *Status: Open (enhancement).*

4. **[#29532](https://github.com/openai/codex/issues/29532) — Persistent SQLite TRACE log churn on macOS after v0.142.0**  
   Impact: Despite initial fixes, verbose `~/.codex/logs_2.sqlite` writes continue to cause disk I/O and noise. 35 comments, 8 👍; developers tracking performance regressions.  
   *Status: Open.*

5. **[#31814](https://github.com/openai/codex/issues/31814) — GPT‑5.6 Sol hides subagent routing controls by default**  
   Impact: MultiAgent V2 sets `hide_spawn_agent_metadata = true`, removing visibility over subagent routing decisions and making the model feel opaque. 11 comments, 11 👍; many consider this a regression in transparency.  
   *Status: Open.*

6. **[#13009](https://github.com/openai/codex/issues/13009) — Spark model rejects `reasoning.summary` with unsupported_parameter**  
   Impact: When using `gpt-5.3-codex-spark`, valid reasoning summaries are rejected, breaking agent flows. 15 comments, 4 👍; long‑standing but still unresolved.  
   *Status: Open.*

7. **[#28919](https://github.com/openai/codex/issues/28919) — Windows Codex app missing “control other devices” tab**  
   Impact: Remote control feature parity is broken on Windows, blocking cross‑device workflows. 16 👍 and 8 comments confirm it’s a common pain for Windows users.  
   *Status: Open.*

8. **[#8342](https://github.com/openai/codex/issues/8342) — Expose MCP Server Prompts as slash commands**  
   Impact: Feature request (22 👍) to match Claude Code’s behaviour, turning MCP server prompts into native slash commands for discoverability.  
   *Status: Open (enhancement).*

9. **[#31664](https://github.com/openai/codex/issues/31664) — Reasoning summaries render literal `<!-- -->` placeholders**  
   Impact: Placeholder comments leak into the TUI and `exec --json` output, cluttering the user interface. 15 👍; developers find it unprofessional.  
   *Status: Open.*

10. **[#29396](https://github.com/openai/codex/issues/29396) — Invalid transport error when setting `mcp_servers.codex_apps.startup_timeout_sec`**  
    Impact: Following the documented fix for MCP timeouts leads to a cryptic transport error, trapping users in a dead end. 9 👍, 3 comments.  
    *Status: Open.*

## Key PR Progress

1. **[#32008](https://github.com/openai/codex/pull/32008) — Keep workload identity credentials out of child processes**  
   Removes workload‑identity selectors and assertions from shell, hook, and MCP child environments after applying overrides. Part 1 of a 3‑stack effort (together with [#32009](https://github.com/openai/codex/pull/32009) RFC‑token‑exchange client and [#32010](https://github.com/openai/codex/pull/32010) integration) to enable secure, rotating identity tokens without leaking them.

2. **[#30131](https://github.com/openai/codex/pull/30131) — Add paginated thread history database**  
   Introduces a `thread_history` SQLite database with initial `thread_turns` and `thread_items` tables. This is the storage foundation for future paginated, fork‑aware conversation history. Accompanied by [#31731](https://github.com/openai/codex/pull/31731) (fork bases) and [#31859](https://github.com/openai/codex/pull/31859) (ordinals for paginated records).

3. **[#31891](https://github.com/openai/codex/pull/31891) — Extract reverse JSONL scanner**  
   A reusable typed `ReverseJsonlScanner` for backwards reading of rollout files, used by paginated history and session indexing. Includes proper error handling for malformed records.

4. **[#31482](https://github.com/openai/codex/pull/31482) — Migrate plugin commands into skills**  
   Converts legacy plugin commands into a first‑class skills model, eliminating a dependency cycle. Moves command‑to‑skill conversion into `codex-core-plugins` and handles atomic install staging.

5. **[#31810](https://github.com/openai/codex/pull/31810) — Pipeline ancestor discovery performance**  
   Parallelises root‑marker lookahead and AGENTS/skills directory checks during remote project startup, reducing serial I/O waits.

6. **[#32006](https://github.com/openai/codex/pull/32006) — Use unique IDs for review history messages**  
   Prevents `/review` from reusing the same ID across turns, ensuring each synthesized history message carries a distinct identity.

7. **[#31688](https://github.com/openai/codex/pull/31688) — Preserve fractional WebSocket TBT metric precision**  
   Records Responses WebSocket time‑to‑byte‑token metrics as `f64`, carrying sub‑millisecond precision through runtime summaries while keeping the TUI display unchanged.

8. **[#32011](https://github.com/openai/codex/pull/32011) — Keep CCA models cache in memory**  
   Adds an in‑memory `ModelsCacheManager` for `codex-cloud-agent` sessions, avoiding filesystem writes for short‑lived cloud environments while preserving file‑backed caching for local Codex.

9. **[#31655](https://github.com/openai/codex/pull/31655) — Move workspace roots onto environments**  
   Binds workspace roots to the selected execution environment instead of thread‑global session state, fixing divergence between `cwd` and roots and simplifying remote executor sandboxing.

10. **[#31998](https://github.com/openai/codex/pull/31998) — Add URI‑native sandbox state metadata for MCP**  
    Introduces capability detection (`sandbox-state-meta-v2`) and URI‑native `SandboxState` metadata, enabling remote and executor‑backed MCP servers to describe sandbox paths without relying on host‑local path conversions.

Additional notable PRs: [#32002](https://github.com/openai/codex/pull/32002) filesystem search path typing, [#31950](https://github.com/openai/codex/pull/31950) generic permission path models, and the prompt‑hooks series ([#26268](https://github.com/openai/codex/pull/26268), [#24634](https://github.com/openai/codex/pull/24634), [#26267](https://github.com/openai/codex/pull/26267)) nearing completion.

## Feature Request Trends

- **Dynamic session workspace**: `/add-dir` slash command to extend working directories mid‑session remains the top‑voted enhancement (35 👍).  
- **MCP prompt integration**: Users want MCP server prompts exposed as native slash commands, bringing parity with other tools (22 👍).  
- **Desktop pet APIs**: Custom interaction hooks for Codex desktop pets, enabling richer interactions beyond the built‑in options.  
- **Credit management controls**: A toggle to prevent automatic consumption of purchased credits when included usage expires (2 👍), alongside ongoing frustration with reset mechanics.  
- **UI defaults**: Option to open Chat in a full window by default, avoiding the compact pop‑up first step.  
- **MultiAgent transparency**: Demand for control over subagent routing and metadata visibility (sparked by #31814).

The common thread is a desire for more **configurable session behaviour**, deeper **MCP ecosystem integration**, and **transparent model steering** as agent complexity grows.

## Developer Pain Points

- **Installer & packaging regressions**: Critical binaries missing from Homebrew cask (#31906) and standalone install failures (#31906, #31979) break workflows after every release, highlighting a need for improved release validation.  
- **Credit reset reliability**: The reset feature (both its initial bug #31606 and the broader desire for credit‑use toggles #28382) saps trust in usage limits, especially for Pro subscribers who rely on predictable quota management.  
- **Model‑version‑specific quirks**: GPT‑5.6 Sol’s MultiAgent V2 hiding routing controls (#31814) and forcing reserved tool functions (#31864) cause hard failures; Spark model’s parameter rejection (#13009) remains unresolved. These signal that newer model families ship with undocumented breaking changes.  
- **MCP tool exposure & transport**: Server tools discovered but not surfaced in Desktop threads (#19425), cryptic transport errors when following docs (#29396), and computer‑use plugin runtime mismatches (#22822) make MCP integration fragile.  
- **Cross‑platform feature gaps**: Windows lags behind in remote control UI (#28919), and macOS 15.7.x struggles with bundled Computer Use binaries (#22822). Developers consistently ask for parity.  
- **Performance & log churn**: SQLite log spam (#29532) and Windows sluggishness (#31692) degrade the development experience, particularly on long‑lived sessions.  
- **Session continuity**: CLI agent restarts during long tool‑heavy turns (#14824) and long conversations showing only recent history after updates (#31995) undercut trust in session persistence.  

Overall, the community is excited about Codex’s rapid iteration but is increasingly vocal about **stability of core workflows**, **transparent behaviour changes in new models**, and **consistent cross‑platform support**.

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest – July 10, 2026

## Today’s Highlights
A fresh nightly release lands with a fix for thought leakage in scrubbed history, while activity surges around critical agent hangs, subagent misreporting, and an infinite auth loop locking out Windows users. Security PRs are trending: a zero‑click RCE in the A2A server and a trust‑dialog disclosure are being patched aggressively.

---

## Releases
**v0.52.0-nightly.20260710.ga4c91ce19**  
- `fix(core): strip thoughts from scrubbed history turns and resolve thought leakage` ([#27971](https://github.com/google-gemini/gemini-cli/pull/27971)) – ensures sensitive model thoughts are properly removed from history when scrubbing.  
- `Refactor: exclude transient CI configuration files from workspace context` – keeps build system internals out of the agent’s context, reducing noise.

---

## Hot Issues

1. **[Subagent recovery after MAX_TURNS is reported as GOAL success](https://github.com/google-gemini/gemini-cli/issues/22323)** (p1, 10 💬, 2 👍)  
   The `codebase_investigator` subagent lies about termination, claiming success when it actually hit the turn limit. This hides real interruptions and breaks trust in subagent output.  

2. **[Generalist agent hangs](https://github.com/google-gemini/gemini-cli/issues/21409)** (p1, 7 💬, 8 👍)  
   Delegating to the generalist agent causes an indefinite hang, even for trivial tasks like folder creation. Users are forced to instruct the model not to use subagents entirely. Top‑voted frustration of the period.  

3. **[Gemini does not use skills and sub-agents enough](https://github.com/google-gemini/gemini-cli/issues/21968)** (p2, 6 💬)  
   Custom skills and sub‑agents are almost never invoked automatically, even when the task aligns perfectly with their description. Users must explicitly mention them, defeating the purpose of automation.  

4. **[Infinite auth loop](https://github.com/google-gemini/gemini-cli/issues/28341)** (p1, 4 💬, 1 👍)  
   OAuth flow restarts endlessly on Windows, rendering the CLI unusable. Downgrading versions does not help – a top‑priority blocker that landed yesterday.  

5. **[Shell command execution gets stuck with "Waiting input"](https://github.com/google-gemini/gemini-cli/issues/25166)** (p1, 4 💬, 3 👍)  
   After a simple CLI command finishes, the agent stays frozen, still showing “Awaiting user input”. The shell is done, but Gemini never notices.  

6. **[Stop Auto Memory from retrying low‑signal sessions indefinitely](https://github.com/google-gemini/gemini-cli/issues/26522)** (p2, 5 💬)  
   The memory background extraction agent keeps re‑processing sessions it already decided to ignore, wasting turns and model calls.  

7. **[Browser subagent fails in Wayland](https://github.com/google-gemini/gemini-cli/issues/21983)** (p1, 4 💬, 1 👍)  
   The browser subagent cannot operate under Wayland compositors, limiting multi‑modal workflows on modern Linux desktops.  

8. **[get‑shit‑done output hook causes crash](https://github.com/google-gemini/gemini-cli/issues/22186)** (p1, 3 💬)  
   The hook crashes Gemini right as it finishes printing a summary, often after long‑running multi‑step tasks. A silent killer of productivity.  

9. **[Agent should stop/discourage destructive behavior](https://github.com/google-gemini/gemini-cli/issues/22672)** (p2, 3 💬, 1 👍)  
   The model occasionally uses `git reset --hard` or `--force` when safer options exist, posing a risk in critical repositories and database operations.  

10. **[Subagents running without permission since v0.33.0](https://github.com/google-gemini/gemini-cli/issues/22093)** (p2, 2 💬)  
    Despite having agents disabled in all configurations, subagents (generalist) appear and run, raising concerns about configuration enforcement and permissions.

---

## Key PR Progress

- **[Fix trust dialog disclosure for runnable hooks](https://github.com/google-gemini/gemini-cli/pull/28346)** (open, p1 security)  
  Ensures folder‑trust dialogs inspect the real hook definitions that get executed, preventing misleading trust prompts and adding a warning for command hooks.  

- **[Enforce workspace trust during environment loading to prevent RCE](https://github.com/google-gemini/gemini-cli/pull/28319)** (open, critical security)  
  Patches a zero‑click remote code execution vulnerability in the A2A server by refactoring the startup sequence to require workspace trust before loading environments.  

- **[Add support for AGENTS.md out of the box](https://github.com/google-gemini/gemini-cli/pull/28240)** (open, p1)  
  No more manual settings tweaks – the CLI now looks for `AGENTS.md` alongside `GEMINI.md` by default, making it instantly compatible with many existing projects.  

- **[Ignore stale update_topic calls after session reset](https://github.com/google-gemini/gemini-cli/pull/28153)** (closed)  
  Prevents a race where an orphaned `update_topic` tool call could overwrite the topic state after a `/clear`, eliminating confusing topic flashes.  

- **[Detect available editors lazily to avoid slow startup](https://github.com/google-gemini/gemini-cli/pull/28144)** (closed)  
  Moves editor probing out of the startup hot path, significantly reducing CLI launch time on Windows where process creation is expensive.  

- **[Respect .gitignore/.geminiignore in skill resource listing](https://github.com/google-gemini/gemini-cli/pull/28149)** (closed)  
  Skills no longer leak ignored or sensitive file names to the model; the folder structure is filtered with the same ignore rules as the workspace.  

- **[Resolve MCP resources by server to prevent cross‑server confusion](https://github.com/google-gemini/gemini-cli/pull/28143)** (closed)  
  When two MCP servers expose the same resource URI, the system now correctly routes requests and approvals to the intended server instead of mixing them up.  

- **[Honor GOOGLE_CLOUD_LOCATION for Vertex AI with API key](https://github.com/google-gemini/gemini-cli/pull/28142)** (closed)  
  Fixes a silent routing bug where regional location settings were ignored when using an API key, forcing all traffic to the global endpoint.  

- **[Ensure task cancellation aborts execution loop](https://github.com/google-gemini/gemini-cli/pull/28316)** (open)  
  Cancelling a task in Agent Mode now truly terminates the execution stream, fixing “ghost executions” that continued to run invisibly.  

- **[Bypass LLM correction for JSON and IPYNB files in write_file and replace](https://github.com/google-gemini/gemini-cli/pull/28223)** (closed)  
  A surgical fix that stops the model from corrupting Jupyter notebooks and strict JSON files during writes or replacements – a long‑standing data‑loss risk.

---

## Feature Request Trends
- **Subagent observability and trajectory sharing** – users want subagent runs visible and shareable via `/chat share`, not buried in internal recordings.  
- **Smarter agent self‑awareness** – demands are growing for the agent to accurately describe its own flags, hotkeys, and limitations, acting as its own expert guide.  
- **AST‑aware tooling** – investigations into code‑structure‑aware file reads, search, and mapping could dramatically reduce token waste and turn counts.  
- **Built‑in safety guardrails** – stopping destructive git commands and dangerous database operations without explicit user approval is becoming a baseline expectation.  
- **Memory system refinement** – deterministic redaction, patch quarantine, and no‑retry‑on‑low‑signal are all wanted to make Auto Memory more trustworthy and efficient.  
- **Eval infrastructure for CI** – the appearance of `eval:validate` and better failure formatting points to a strong demand for gating agent quality in pipelines.

---

## Developer Pain Points
- **Agent reliability is brittle** – generalist hangs, shell‑command “waiting input” freezes, and subagent false‑success reports repeatedly break workflows.  
- **Authentication hell** – the infinite OAuth loop on Windows has left users completely blocked, with no workaround across multiple versions.  
- **Subagents ignore configuration** – settings like `maxTurns` are silently disregarded, and subagents can run even when disabled globally, eroding confidence in the permission model.  
- **Workspace clutter** – the model scatters temporary scripts across random directories, making clean commits unnecessarily difficult.  
- **Tool scaling limits** – encountering a 400 error when more than ~128 tools are available hints at fragile tool registration that hasn’t caught up with the growing ecosystem.  
- **Terminal and editor corruption** – flicker on resize and corruption after exiting external editors degrade the UI experience, especially in long‑running sessions.

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI Community Digest – 2026-07-10

## Today’s Highlights
A significant double release lands: v1.0.70 brings official GPT-5.6 model support while a companion pre‑release (v1.0.70‑0) introduces on‑demand shell sandboxing and the `/refine` command. The community is also focused on a wave of TUI crashes in the latest pre‑release, fresh enterprise policy hurdles, and a long‑awaited resolution for project‑scoped plugins.

---

## Releases

### v1.0.70
- **New model:** GPT-5.6 support added.
- **Error UX:** `mcp` and `skill` command failures now show a single `Error` prefix.
- **Agent parsing:** Real parse error surfaced when `--agent` selects a malformed custom agent.
- **Networking:** `web_fetch` now works through mandatory HTTPS proxies (fixing #4019).
- **UI:** Gists tab hides the `/` search; superseded subagent runs are treated as cancelled.

### v1.0.70‑0 (pre‑release)
- **Plugin pinning:** Pin plugins to an exact commit SHA via `sha` in plugin source config.
- **Sandbox toggles:** `--sandbox` / `--no-sandbox` flags let you turn the OS‑level shell sandbox on or off for a single session (useful with `-p`), without changing your saved setting.
- **New command:** `/refine` added for rewriting.

---

## Hot Issues

1. **#1595 – Enterprise policy blocks model listing**  
   *28 comments · 10 👍*  
   Users with valid Enterprise Copilot subscriptions cannot list or use models, getting “access denied by Copilot policy” despite showing remaining premium requests. The issue has been open since February and remains a serious blocker for enterprise teams.  
   🔗 https://github.com/github/copilot-cli/issues/1595

2. **#970 – macOS Gatekeeper blocks Copilot after every Homebrew upgrade**  
   *7 comments · 21 👍*  
   Each `brew upgrade` triggers “Apple could not verify ‘copilot’ is free of malware”, forcing users to manually approve in Privacy & Security. The high number of upvotes reflects widespread frustration among Mac developers.  
   🔗 https://github.com/github/copilot-cli/issues/970

3. **#1665 – Project‑scoped Copilot CLI plugins** *(now closed)*  
   *13 comments · 18 👍*  
   A highly requested feature to allow plugins to be installed per‑project/repository instead of globally. The issue was closed recently, sparking hope that repo‑level plugin support has landed.  
   🔗 https://github.com/github/copilot-cli/issues/1665

4. **#4069 – TUI wedges mid‑turn: screen clears, input dead, Ctrl+\\ ignored**  
   *7 comments · 7 👍*  
   In the latest 1.0.70‑0 pre‑release on WSL2 + Windows Terminal, the terminal becomes completely unresponsive during streaming. The Rust JSON‑RPC transport writes EIO then EPIPE, leaving users with a hard‑killed session. Multiple similar reports are emerging.  
   🔗 https://github.com/github/copilot-cli/issues/4069

5. **#2627 – Configurable system prompt to slash token overhead**  
   *3 comments · 18 👍*  
   The Copilot CLI system prompt burns ~20,500 tokens at startup, plus ~8,500 for tool definitions. Users want the ability to slim down this fixed overhead for their workflows. This is one of the most upvoted feature requests.  
   🔗 https://github.com/github/copilot-cli/issues/2627

6. **#4019 – `web_fetch` fails with mandatory HTTP proxies** *(closed, fixed in v1.0.70)*  
   *3 comments · 0 👍*  
   Corporate users behind proxies could not use `/research` or web‑fetch. The fix in v1.0.70 unblocks a core productivity feature for many enterprise developers.  
   🔗 https://github.com/github/copilot-cli/issues/4019

7. **#4077 – TUI black‑screen hang mid‑turn (Windows Terminal, 1.0.70‑0)**  
   *2 comments · 2 👍*  
   Another TUI crash variant where the screen goes black but content survives; recoverable via `--resume`. This and #4069 suggest a regression in the pre‑release’s terminal handling.  
   🔗 https://github.com/github/copilot-cli/issues/4077

8. **#1675 – Checkpoint restore permanently deletes all untracked files**  
   *2 comments · 0 👍*  
   Using “restore to checkpoint” runs `git clean -fd` at the repo root, erasing all untracked files with no undo. This can cause serious data loss for users leveraging checkpoints during experiments.  
   🔗 https://github.com/github/copilot-cli/issues/1675

9. **#2792 – Automatic model switching between planning and execution**  
   *4 comments · 14 👍*  
   Users want to dedicate a powerful model for planning then switch to a cheaper/faster model for execution automatically. A popular efficiency‑oriented request.  
   🔗 https://github.com/github/copilot-cli/issues/2792

10. **#4067 – `model` field in `settings.json` not applied on startup**  
    *0 comments · 0 👍* (new)  
    Despite setting a valid model in `~/.copilot/settings.json`, CLI falls back to the default `claude-sonnet-5`. This undermines the configuration model and leads to confusion.  
    🔗 https://github.com/github/copilot-cli/issues/4067

---

## Key PR Progress
No pull requests were merged or updated in the last 24 hours. The release activity (v1.0.70 and v1.0.70‑0) likely reflects internal branch merges directly to the release channel.

---

## Feature Request Trends
Several clear themes are emerging from the issue tracker:

- **Model selection granularity:** Users want model family aliases (e.g., `opus`, `sonnet` – #4068), automatic planning‑to‑execution model switching (#2792), and a default model for `/fleet` subagents (#2193).
- **Configuration & cost control:** Editable system prompts (#2627), custom headers for BYOK endpoints (#3399), and extended‑context pricing visibility (#4059) all aim to give power users deeper control over token usage and cost.
- **Plugin & agent customization:** Support for project‑scoped plugins (#1665, now closed) and configurable MCP tools for the built‑in research agent (#4076) highlight the demand for per‑project and per‑workload agent tailoring.
- **Scheduling enhancements:** Issues around scheduled prompts (#4079, #4078) indicate a desire for robust, non‑destructive task queuing when using `/every` or `/after`.

---

## Developer Pain Points

- **Terminal stability:** TUI hangs, screen clears, and unresponsive sessions plague the latest pre‑release (#4069, #4077). WSL2/Windows Terminal users are disproportionately affected.
- **Enterprise friction:** Policy‑based model blocking (#1595), mandatory proxy incompatibilities (fixed in v1.0.70, #4019), and macOS Gatekeeper interruptions (#970) make Copilot CLI hard to adopt in locked‑down corporate environments.
- **Session management fragility:** Sessions sometimes become invisible in the picker (#4071) or cannot be found for resume (#3931), and checkpoint rollback can destroy untracked work (#1675).
- **Configuration drift:** The `model` setting in `settings.json` being ignored at startup (#4067) creates uncertainty about which model is actually in use.
- **Resource overload:** Too many MCP servers cause continuous context compaction (#3024), and repeated file opens for event logging trigger Windows Defender CPU spikes (#4063).

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI Community Digest — July 10, 2026

## Today's Highlights
A fix for subprocess crashes and a new interoperability enhancement lead today's activity. The community is also discussing an SSL bypass option for restrictive corporate environments, and a patch to improve tool-call summaries.

## Releases
No new version was released in the last 24 hours.

## Hot Issues
- **#2458 [enhancement] Add option to ignore SSL certificate**  
  *dmorsin* reports that organization‑controlled antivirus uses MiTM SSL inspection, causing login failures because the CLI rejects the injected certificate. The request: a flag or config key to disable certificate verification for networks where this is unavoidable. 5 comments discuss workarounds and security implications.  
  🔗 https://github.com/MoonshotAI/kimi-cli/issues/2458

- **#2318 [bug] request reached organization TPD rate limit, current: 1505241**  
  *globalvideos272-lab* encountered a rate‑limit error with an implausibly high token‑per‑day value, suggesting a miscalculation in the TPD counter. One comment and one upvote indicate early-stage interest; the issue remains unconfirmed.  
  🔗 https://github.com/MoonshotAI/kimi-cli/issues/2318

## Key PR Progress
- **#2487 feat(agent): support loading CLAUDE.md alongside AGENTS.md**  
  Adds automatic discovery of `CLAUDE.md` (and `.claude/CLAUDE.md`) in the same function that loads `AGENTS.md`. Projects that already use Claude Code’s configuration files will now be picked up seamlessly by Kimi CLI.  
  🔗 https://github.com/MoonshotAI/kimi-cli/pull/2487

- **#2324 fix(web): handle BrokenPipeError in SessionProcess.send_message**  
  Guards writes to a subprocess’s stdin after it may have exited between the `start()` call and the actual `write`. Catches `BrokenPipeError` to prevent a crash when the runner process disappears unexpectedly.  
  🔗 https://github.com/MoonshotAI/kimi-cli/pull/2324

- **#2449 fix(string): strip newlines in shorten_middle before the length check**  
  `shorten_middle` is used to produce single‑line summaries of tool‑call arguments. Previously, newline characters could cause the function to return early before collapsing whitespace, yielding multi‑line output that broke formatting. This patch moves the newline stripping before the length check.  
  🔗 https://github.com/MoonshotAI/kimi-cli/pull/2449

## Feature Request Trends
- **Flexible SSL handling in enterprise networks**: #2458 is the clearest signal. Developers behind corporate proxies, deep‑packet inspection, or antivirus MiTM need a trusted way to bypass strict certificate validation, ideally via a configuration flag.  
- **Cross‑tool configuration compatibility**: PR #2487 reflects a demand for interoperability with other AI coding assistants (like Claude Code). Users want to reuse existing setup files without duplication.

## Developer Pain Points
- **Network & SSL friction in managed environments** – The inability to ignore self‑signed or injected certificates blocks adoption on corporate machines.  
- **Subprocess stability** – `BrokenPipeError` (#2324) reveals that the session runner can die mid‑communication, leading to silent failures that are hard to debug.  
- **Output formatting quirks** – Edge cases in string truncation (#2449) break tool‑call summaries, making it harder to read long arguments in logs and prompts.  
- **Rate‑limit confusion** – The impossible TPD value in #2318 suggests either a bug or unclear documentation around quota calculation, which frustrates users hitting limits unexpectedly.

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-07-10

## 1. Today’s Highlights
OpenCode shipped three patch releases (v1.17.18, v1.17.17, v1.17.16) that polish Copilot pricing, Meta/Grok model handling, and desktop UX. The V2 architecture continues to be the dominant development theme, with active work on tool plugins, session forking, file watching, and UI refinements. Community discussion remains focused on session safety (unarchive, data loss), cost transparency, and the desire for a lightweight “ask mode” to save tokens.

---

## 2. Releases

- **[v1.17.18](https://github.com/anomalyco/opencode/releases/tag/v1.17.18)**  
  *Bugfix:* Prevents crashes and incorrect pricing when GitHub Copilot returns models with a zero billing batch size.  
  *Improvement:* Adds a model-specific system prompt for Meta Muse Spark.

- **[v1.17.17](https://github.com/anomalyco/opencode/releases/tag/v1.17.17)**  
  *Core:* Improved Meta model handling for reasoning variants and provider requests.  
  *Desktop:* Fixed clipped descenders in the model selector, added a dismissible tabs intro popup, and refreshed the help entry point.

- **[v1.17.16](https://github.com/anomalyco/opencode/releases/tag/v1.17.16)**  
  *Core:* Exposed reasoning effort variants for Grok models; improved xAI prompt cache routing and PDF support in Responses models.  
  *Desktop:* Added “Open containing folder” action for projects and a composer add menu for files/commands.

---

## 3. Hot Issues

1. **#12393 [CLOSED] – How to unarchive sessions in opencode-desktop**  
   The most upvoted issue (👍27, 16 comments) reveals a gap in session management: once archived, sessions become invisible to many users. The solution was resolved, but it underscores the need for a clear archive/unarchive UI.  
   🔗 [anomalyco/opencode#12393](https://github.com/anomalyco/opencode/issues/12393)

2. **#1573 [CLOSED] – Add an ASK MODE to save tokens**  
   A simple “hi” costs 17.7K tokens because of tool/MCP context. The request for a dedicated lightweight mode without tools/agents resonates with many (11 comments, 👍2). It highlights a tension between full agent capabilities and token economy.  
   🔗 [anomalyco/opencode#1573](https://github.com/anomalyco/opencode/issues/1573)

3. **#10815 [CLOSED] – Data loss due to accidental session closure (macOS)**  
   The common shortcut Cmd-Shift-Delete force-closes a session without confirmation, leading to lost work (👍4, 6 comments). A loud reminder that global shortcuts must be safe-by-default in IDEs.  
   🔗 [anomalyco/opencode#10815](https://github.com/anomalyco/opencode/issues/10815)

4. **#21578 [CLOSED] – Restore per-session auto-accept permissions toggle**  
   Users miss the session-level toggle for auto-accept that was moved to global Settings in v1.4.0 (👍4, 6 comments). The preference for granular, temporary permission overrides remains strong.  
   🔗 [anomalyco/opencode#21578](https://github.com/anomalyco/opencode/issues/21578)

5. **#34489 [CLOSED] – V2: add tool registration to Promise plugins**  
   A pivotal V2 enhancement that implements plugin-based tool registration, paving the way for a cleaner, extensible tool system. Highly active (5 comments) and now closed.  
   🔗 [anomalyco/opencode#34489](https://github.com/anomalyco/opencode/issues/34489)

6. **#24090 [CLOSED] – Assistant messages missing `tool_calls` field breaks OpenAI-compatible providers**  
   History replays strip `tool_calls`, causing failures with third-party endpoints (5 comments). A core compatibility fix that ensures OpenCode works reliably across diverse model backends.  
   🔗 [anomalyco/opencode#24090](https://github.com/anomalyco/opencode/issues/24090)

7. **#34430 [OPEN] – Implement V2 session.fork API**  
   An open feature request for forking sessions from a specific message or timeline boundary (4 comments). This would enable advanced collaboration and experimentation workflows.  
   🔗 [anomalyco/opencode#34430](https://github.com/anomalyco/opencode/issues/34430)

8. **#35013 [OPEN] – V2 auto-compaction bypass for Fable/Zen request-size 400**  
   A subtle V2 bug where long Fable sessions exceed byte limits before token limits, skipping auto-compaction (3 comments). Critical for reliability with high-throughput agents.  
   🔗 [anomalyco/opencode#35013](https://github.com/anomalyco/opencode/issues/35013)

9. **#36199 [OPEN] – Session stalls when upstream returns zero token usage**  
   A newly reported stall: a valid 200 response with `usage: {prompt_tokens:0, completion_tokens:0}` hangs the session (2 comments). Indicates a fragile assumption about provider usage data.  
   🔗 [anomalyco/opencode#36199](https://github.com/anomalyco/opencode/issues/36199)

10. **#34835 [OPEN] – V2: improve UX for provider content-filter finishes**  
    Provider blocks (content-filter) are recorded but the user sees an awkward “finished” state with no actionable message (4 comments). A UX friction point that needs a graceful fallback.  
    🔗 [anomalyco/opencode#34835](https://github.com/anomalyco/opencode/issues/34835)

---

## 4. Key PR Progress

1. **#36202 – fix(app): timeline width jumps when review panel is open/closed**  
   Fixes an annoying layout bug where the timeline switches between fixed and full width, improving desktop UX consistency.  
   🔗 [anomalyco/opencode#36202](https://github.com/anomalyco/opencode/pull/36202)

2. **#36038 – fix(app): preserve model selection across draft remounts**  
   Scopes transient model selection to each draft tab and persists state across provider/route remounts, preventing loss of user intent.  
   🔗 [anomalyco/opencode#36038](https://github.com/anomalyco/opencode/pull/36038)

3. **#36073 – feat(app): visual improvements**  
   Polishes terminal/review panel backgrounds, fixes `LineComment` fonts, and switches terminal tabs to a more modern look.  
   🔗 [anomalyco/opencode#36073](https://github.com/anomalyco/opencode/pull/36073)

4. **#36184 – fix(tui): reconcile shells after reconnect**  
   Retains shell locations across reconnects and clears stale counts, making terminal sessions resilient to restarts.  
   🔗 [anomalyco/opencode#36184](https://github.com/anomalyco/opencode/pull/36184)

5. **#36182 – fix(app): wrap session creation state updates in startTransition**  
   Uses Solid’s `startTransition` to batch post-session mutations, eliminating UI flicker and intermediate states on new session creation.  
   🔗 [anomalyco/opencode#36182](https://github.com/anomalyco/opencode/pull/36182)

6. **#36180 – refactor(core): simplify tool admission flow**  
   Reduces tool admission logic to `materialize(permissions?)`, removes stale terminology, and enforces a clean invariant for tool execution. A key V2 prep refactor.  
   🔗 [anomalyco/opencode#36180](https://github.com/anomalyco/opencode/pull/36180)

7. **#36179 – fix: create root span per prompt for OTEL trace isolation**  
   Ensures each prompt gets its own root span when `OTEL_EXPORTER_OTLP_ENDPOINT` is set, fixing trace grouping across sessions.  
   🔗 [anomalyco/opencode#36179](https://github.com/anomalyco/opencode/pull/36179)

8. **#36042 – feat(tui): show subagent status in sidebar**  
   Adds a real-time subagent status panel in the TUI, giving visibility into child sessions started by subagents.  
   🔗 [anomalyco/opencode#36042](https://github.com/anomalyco/opencode/pull/36042)

9. **#36172 – fix(app): preload more timeline messages**  
   Boosts the initial message fetch from 2 to 20, keeping the UI filled while larger history loads happen in the background.  
   🔗 [anomalyco/opencode#36172](https://github.com/anomalyco/opencode/pull/36172)

10. **#36129 – refactor(form): model links as fields**  
    Introduces model URL requests as `link` fields inside the form schema, supporting opening/copying/completing links in TUI and streamlining MCP form handling.  
    🔗 [anomalyco/opencode#36129](https://github.com/anomalyco/opencode/pull/36129)

---

## 5. Feature Request Trends

- **V2 Plugin & Extensibility Foundation**  
  Multiple issues track the V2 tool plugin API (#34489, #34491 echoes), model-context hooks (#35408), and resource tool porting (#34546). The community wants a stable plugin surface before V2 stabilizes.

- **Session Forking & Advanced Navigation**  
  `session.fork` (#34430) and double‑click pane maximize (#35314) indicate a desire for more flexible workspace management akin to established IDEs.

- **Permission & Directory Granularity**  
  Requests for per‑session auto‑accept (#21578) and multi‑directory access (#23041) reflect that users need fine‑grained, context‑aware security not a global toggle.

- **Context Efficiency & Compaction**  
  Background incremental compaction (#16466) and the “ask mode” (#1573) highlight token cost awareness; users expect smarter, less disruptive context management.

- **UI & Feedback Richness**  
  Improved thinking‑progress indicators (#26381) and content‑filter UX (#34835) are recurring themes—developers want transparency during long operations and graceful error states.

---

## 6. Developer Pain Points

- **Resource Leaks & Stability**  
  Leaking `.so` files from ripgrep workers (#23804), orphaned MCP server processes (#26714), and endless tool‑execution loops (#26627) create frustration and resource exhaustion. These are being actively patched.

- **Desktop Session Safety**  
  Accidental closure without confirmation (#10815), hidden archived sessions (#12393), and pasted images being ignored (#26064) degrade the desktop experience. Users expect consumer‑grade data protection.

- **Model‑Specific Quirks & Provider Gaps**  
  Incomplete JSON when using `gpt-5.3-codex` (#20599), missing `tool_calls` in replays (#24090), and context‑limit discrepancies (#31064) expose fragility in the provider abstraction. The zero‑token stall (#36199) is a new edge case.

- **Token‑Waste in “Just Chatting”**  
  The lack of a low‑overhead mode (#1573) forces every interaction through the full agent/MCP pipeline, burning tokens unnecessarily. A persistent ask for a “/ask” command or similar.

- **Navigation & History**  
  Sticky history navigation (#26769) and model overrides surviving `/new` (#26741) show that session state management can be unintuitive, breaking mental models.

- **V2 Migration Gaps**  
  Pre‑runner errors that vanish (#34839), auto‑compaction bypasses (#35013), and missing @‑tag support (#34387) are concrete pain points for early V2 adopters, signalling that the transition is still in heavy iteration.

---

*Curated by the OpenCode community analysis bot. Data sourced from [anomalyco/opencode](https://github.com/anomalyco/opencode) activity on 2026-07-10.*

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

## Pi Community Digest – 2026-07-10

### Today’s Highlights
The `max` thinking level debuts in v0.80.6, unlocking the full reasoning power of GPT-5.6 Sol and adaptive Claude models. Meanwhile, a long-awaited fix lands for thinking‑block stripping in newer Anthropic models, and the community continues to debate strict tools/grammars and ephemeral model changes at high volume.

---

### Releases
**v0.80.6** – [Release notes](https://github.com/earendil-works/pi/releases/tag/v0.80.6)  
- **`max` thinking level** – A new opt‑in reasoning tier above `xhigh`, natively supported on GPT-5.6 and adaptive Claude models. Exposed via `--thinking max`, the SDK, RPC, and model selection. Custom themes can define `thinkingMax`. Full details in the [CLI Reference](https://github.com/ear) (link truncated in source).

*Note: v0.80.5 was published but contained no feature details.*

---

### Hot Issues
1. **[#6306 – Support Strict Tools / Grammar](https://github.com/earendil-works/pi/issues/6306)** (⏳ open, 22 comments)  
   *Why it matters:* The SDK currently has no way to express “free‑form” vs. “strict” tools or grammar‑aware probing, while OpenAI SDKs already support custom LARK/rust‑regex schemas.  
   *Community:* Heavy discussion on how to expose grammar constraints without fragmenting the tool API, led by mitsuhiko.

2. **[#6097 – Add support for ‘max’ thinking level](https://github.com/earendil-works/pi/issues/6097)** (🎉 resolved in v0.80.6, 16 👍)  
   *Why it mattered:* Users wanted the sixth thinking tier for GPT‑5.6 Sol. This request received the most thumbs‑up in the issue tracker; the community’s enthusiasm directly shaped the v0.80.6 feature.

3. **[#5263 – Make in-session model and thinking-level changes ephemeral by default](https://github.com/earendil-works/pi/issues/5263)** (⏳ open, 6 👍)  
   *Why it matters:* Currently `/model` or `/thinking` changes persist across sessions, leading to accidental global overrides. A “Default model” picker in `/settings` is proposed instead.  
   *Community:* 6 likes and detailed discussion about preserving explicit defaults vs. per‑session experimentation.

4. **[#6206 – Clamping to context window prevents artificial context limits](https://github.com/earendil-works/pi/issues/6206)** (⏳ open, 7 comments)  
   *Why it matters:* A recent fix clamps `max_tokens` to the reported context window, but users who deliberately set lower limits (distinct from token budget) are now blocked. The community debates whether the clamp should respect explicit intent.

5. **[#6303 – Exponential retry backoff has no cap despite `retry.provider.maxRetryDelayMs`](https://github.com/earendil-works/pi/issues/6303)** (⏳ open, 1 👍)  
   *Why it matters:* `getRetrySettings()` omits `maxDelayMs`, so backoff grows unbounded — attempt 7 alone waits ~4 minutes under default settings. The issue highlights a gap between config and runtime.

6. **[#6210 – `/scoped-models` cannot select model ids containing brackets](https://github.com/earendil-works/pi/issues/6210)** (⏳ open, 6 comments)  
   *Why it matters:* Brackets in custom model IDs (e.g. `[1m]`) break the selector pattern, showing “No models match …”. A regression for users naming models with common delimiters.

7. **[#5886 – AgentSession settlement/continuation and assistant‑tail lifecycle bugs](https://github.com/earendil-works/pi/issues/5886)** (⏳ open, 2 👍)  
   *Why it matters:* A meta‑issue by mitsuhiko capturing a recurring class of bugs where post‑run logic tries to continue from a stale transcript, causing flaky agent behaviour.

8. **[#6376 – Thinking blocks inappropriately stripped in newer Claude models](https://github.com/earendil-works/pi/issues/6376)** (✅ closed, fixed by [#6457](https://github.com/earendil-works/pi/pull/6457), 5 comments)  
   *Why it mattered:* Fable 5, Sonnet 5, Opus 4.7/4.8 no longer returned thinking text from Anthropic, and Pi stripped the empty blocks after the second call. The fix now always sends the thinking blocks, preserving reasoning chains.

9. **[#6363 – Add an extension/RPC event for “agent run fully settled / idle”](https://github.com/earendil-works/pi/issues/6363)** (✅ closed, linked to [#2023](https://github.com/earendil-works/pi/issues/2023) which had 14 comments and 5 👍)  
   *Why it matters:* Extensions needed a reliable idle event (not the error‑prone `agent_end`) to sync state when the agent has finished processing. This closed the long‑standing #2023 `runWhenIdle` demand.

10. **[#6378 – “nothing Ic an do to fix this error”](https://github.com/earendil-works/pi/issues/6378)** (⏳ open, 1 👍)  
   *Why it matters:* A user‑facing error about exceeding context length with no clear mitigation. The issue draws attention to the need for better error guidance and automatic compression hints.

---

### Key PR Progress
1. **[#6467 – fix(package-manager): restore missing git package deps + pnpm‑friendly install flags](https://github.com/earendil-works/pi/pull/6467)**  
   Addresses extension loading failures when `node_modules` are missing, especially for pnpm users.

2. **[#6457 – fix: send anthropic thinking blocks also when thinking text is empty](https://github.com/earendil-works/pi/pull/6457)**  
   Fixes [#6376](https://github.com/earendil-works/pi/issues/6376); ensures thinking blocks are always forwarded to preserve reasoning in newer Claude models.

3. **[#6474 – feat(ai): support message‑anchored tool loading](https://github.com/earendil-works/pi/pull/6474)**  
   Proof‑of‑concept that allows tools to be introduced mid‑conversation on supported backends, avoiding the need to declare all tools upfront.

4. **[#6471 – fix(ai): correct GPT-5.6 Codex context window](https://github.com/earendil-works/pi/pull/6471)**  
   Updates the context window for GPT-5.6 Sol/Terra/Luna from 272k to 372k tokens, matching upstream Codex metadata.

5. **[#6470 – feat(coding-agent): expand ~ in shellPath setting](https://github.com/earendil-works/pi/pull/6470)**  
   Enables tilde expansion in the `shell` config (e.g., `~/.local/bin/agent-shell-sandbox`), closing [#6458](https://github.com/earendil-works/pi/issues/6458).

6. **[#6463 – fix(coding-agent): cancel auto-retry when switching models](https://github.com/earendil-works/pi/pull/6463)**  
   Prevents a confusing situation where `/model` switches the model but an in‑flight auto‑retry continues, now cancelled cleanly.

7. **[#6460 – feat(ai): add xAI Grok SuperGrok OAuth provider](https://github.com/earendil-works/pi/pull/6460)**  
   Adds device‑code OAuth for SuperGrok subscribers, parallel to existing API key support, complete with tests.

8. **[#6449 – add ResourceExhausted as a retryable error](https://github.com/earendil-works/pi/pull/6449)**  
   Marks rate‑limit and resource‑exhaustion errors as retryable, improving reliability under high load.

9. **[#6427 – feat(coding-agent): add prompt cache miss tracking](https://github.com/earendil-works/pi/pull/6427)**  
   Detects and warns about prompt cache misses in the transcript and `/session` info, helping users understand when idle gaps or model switches invalidate the cache.

10. **[#6216 – feat: Add Amazon Bedrock Mantle OpenAI Responses provider](https://github.com/earendil-works/pi/pull/6216)**  
    Open PR integrating [Amazon Bedrock Mantle’s OpenAI Responses API](https://docs.aws.amazon.com/bedrock/latest/userguide/bedrock-mantle.html) using the official OpenAI Node SDK Bedrock provider.

---

### Feature Request Trends
- **Ephemeral model/thinking changes** – A strong desire to make in‑session `/model` and `/thinking` switches temporary, with a single canonical default set via `/settings` ([#5263](https://github.com/earendil-works/pi/issues/5263)).  
- **Agent‑idle hooks** – Long‑standing demand for a “run‑fully‑settled” event (resolved via [#6363](https://github.com/earendil-works/pi/issues/6363), originally [#2023](https://github.com/earendil-works/pi/issues/2023)).  
- **Strict tools & grammar** – Users want programmatic control over tool schema validation and grammar‑based probing ([#6306](https://github.com/earendil-works/pi/issues/6306)).  
- **Retry & backoff configurability** – The community asks for capped exponential backoff and better hook into retry limits ([#6303](https://github.com/earendil-works/pi/issues/6303)).  
- **Fast sessions & SQLite storage** – Exploration of SQLite session storage for faster load/search ([#5804](https://github.com/earendil-works/pi/issues/5804)).  
- **OAuth login for Grok** – Delivered via [#6460](https://github.com/earendil-works/pi/pull/6460).  
- **Better error messages** – Especially around context‑length overflows and ambiguous CLI feedback.

---

### Developer Pain Points
- **Context‑window clamping** – The hard clamp on `max_tokens` prevents deliberate undersizing, a common advanced workflow ([#6206](https://github.com/earendil-works/pi/issues/6206)).  
- **Unbounded retry delays** – Exponential backoff without a cap can stall sessions for minutes, despite a config knob existing ([#6303](https://github.com/earendil-works/pi/issues/6303)).  
- **Model ID brackets break selector** – A surprising limitation that trips users naming models with versioning patterns ([#6210](https://github.com/earendil-works/pi/issues/6210)).  
- **Stale agent state after interruption** – Hitting Escape while an extension hook never settles leaves the TUI stuck in `Working...` ([#6234](https://github.com/earendil-works/pi/issues/6234), now fixed).  
- **Thinking blocks disappearing** – Newer Anthropic models stopped returning thinking text, and Pi silently stripped the remainder (fixed).  
- **Missing idle event** – Extensions had to hack around `agent_end`; the new idle event settles this (closed).  
- **Prompt template newline collapse** – Multi‑line arguments were collapsed to spaces, breaking templates that relied on formatting ([#4973](https://github.com/earendil-works/pi/issues/4973), noted as closed).  
- **Extension installation paths** – Documentation mismatches between declared and actual install locations cause discovery failures ([#6400](https://github.com/earendil-works/pi/issues/6400)).

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-07-10

## 1. Today’s Highlights
The `v0.19.8` nightly ships critical fixes for infinite subagent loops and session history corruption, while the cua-driver-rs fork adds relative‑coordinate support with prebuilt binaries. The multi‑workspace daemon effort advances significantly, with five PRs completing phase‑4 surface for ACP, workspace picker, session listing, channel workers, and extensions. Community activity remains focused on regressions in image paste, IDE integration, and a growing desire for real‑time subagent observability.

## 2. Releases
**v0.19.8-nightly.20260710.205430235**  
- Stop repeated subagent tool-call loops ([#6543](https://github.com/QwenLM/qwen-code/pull/6543))  
- Session: detect and mark broken history chains  
- cua-driver-rs v0.7.1: prebuilt binaries (macOS universal, Linux x86_64+arm64, Windows) with relative‑coordinate fork (macOS codesigned + notarized)  

## 3. Hot Issues (top 10 by developer interest)
1. **[#6378 RFC: Multi‑workspace daemon](https://github.com/QwenLM/qwen-code/issues/6378)** (20 💬)  
   Proposes one `qwen serve` daemon to host multiple workspaces, preserving backward compatibility. High community engagement, now actively being implemented (see phase‑4 PRs).

2. **[#6560 Restore in‑chat image & document uploads](https://github.com/QwenLM/qwen-code/issues/6560)** (18 💬)  
   Users report loss of drag‑and‑drop and Ctrl+V paste for images/docs. A top‑voted UX regression; duplicates exist on macOS ([#6590](https://github.com/QwenLM/qwen-code/issues/6590)) and Windows ([#6577](https://github.com/QwenLM/qwen-code/issues/6577)).

3. **[#6581 JetBrains ACP: agent does not receive user prompt](https://github.com/QwenLM/qwen-code/issues/6581)** (8 💬)  
   IntelliJ integration sends only bootstrap context, not the actual user prompt. Breaks the assistant workflow for a significant IDE audience.

4. **[#6565 Connection Internal Error (日本/中文 users)](https://github.com/QwenLM/qwen-code/issues/6565)** (7 💬)  
   “糟糕！连接到 Qwen Coder 时出现问题” – users hit Internal Error while connecting, suggesting auth or plan‑token instability. Needs triage.

5. **[#3696 Comprehensive hot‑reload for skills, extensions, MCP](https://github.com/QwenLM/qwen-code/issues/3696)** (5 💬)  
   Long‑running tracking issue to let config, MCP servers, LSPs, and extensions reload without restart. Partial implementation exists; full reload is a frequently requested DX improvement.

6. **[#6629 Cron parser drops step in `5/15` expressions](https://github.com/QwenLM/qwen-code/issues/6629)** (3 💬)  
   Single‑value step syntax like `5/15` matches only `5`, not stepping from 5. Undermines scheduled‑task reliability; affects any automation relying on precise cron.

7. **[#6595 qwen3.7-max leaks `<analysis>/<summary>` tags](https://github.com/QwenLM/qwen-code/issues/6595)** (3 💬)  
   In long‑context tool‑use, internal tags appear in assistant output, breaking follow‑up actions. Impacts agent reliability with the default model.

8. **[#6600 `--debug` prints log path but never creates the file](https://github.com/QwenLM/qwen-code/issues/6600)** (4 💬)  
   Debug logging broken in v0.19.8; developers cannot collect logs for troubleshooting. Hinders issue reproduction.

9. **[#6487 Memory index stale after /remember & lost on compaction](https://github.com/QwenLM/qwen-code/issues/6487)** (2 💬)  
   `MEMORY.md` goes stale, and memory content disappears during chat compaction. Long‑session users lose critical context.

10. **[#6614 Glob tool OOM on large path before truncation](https://github.com/QwenLM/qwen-code/issues/6614)** (2 💬)  
    Unrestricted `**` on a large repo causes JavaScript heap OOM, killing the process. Threatens agent stability in real‑world monorepos.

## 4. Key PR Progress (10 significant pull requests)
### Multi‑workspace daemon (phase 4)
- **[#6621 ACP transport per workspace](https://github.com/QwenLM/qwen-code/pull/6621)** – Exposes a dedicated ACP endpoint for each registered workspace when the daemon hosts multiple workspaces.  
- **[#6625 Workspace picker in Web Shell](https://github.com/QwenLM/qwen-code/pull/6625)** – Adds a dropdown to select the target workspace for new sessions, with sidebar updates.  
- **[#6631 Session listing for non‑primary workspaces](https://github.com/QwenLM/qwen-code/pull/6631)** – Archived/organized views and group filtering now work for all workspaces.  
- **[#6635 Channel workers grouped by workspace](https://github.com/QwenLM/qwen-code/pull/6635)** – Daemon‑managed channels bind to the correct workspace, not only the primary.  
- **[#6638 Extensions REST per workspace](https://github.com/QwenLM/qwen-code/pull/6638)** – Workspace‑qualified endpoints for extension management alongside the existing primary ones.

### Other notable PRs
- **[#6619 Scheduled‑task precondition](https://github.com/QwenLM/qwen-code/pull/6619)** – Isolated scheduled tasks can evaluate a cron‑driven condition before dispatching the prompt, enabling smarter automation.  
- **[#5396 Reduce UI flicker](https://github.com/QwenLM/qwen-code/pull/5396)** – Throttling, `startTransition`, and batching updates eliminate jank across the interface.  
- **[#5928 Project‑local `todosDirectory`](https://github.com/QwenLM/qwen-code/pull/5928)** – Persists todo‑write output inside the project (e.g. `.qwen/todos`) for Git‑syncable task state.  
- **[#6626 Markdown table readability](https://github.com/QwenLM/qwen-code/pull/6626)** – Adds density toggle, zebra striping, expand/collapse, and tooltips to web‑shell tables.  
- **[#5980 Auth‑modified env vars take priority](https://github.com/QwenLM/qwen-code/pull/5980)** – Fixes 401 errors when changing model supplier via `/auth` and starting a new session; auth settings now override inherited system env vars.

## 5. Feature Request Trends
- **Multi‑workspace daemon** – Overwhelming community support for a single serve process managing multiple repositories, with workspace‑scoped sessions, channels, and extensions.  
- **Media uploads & clipboard integration** – Strong demand to bring back drag‑and‑drop, Ctrl+V paste for images/documents, currently broken across macOS/Windows standalone installations.  
- **Hot‑reload for config, skills, MCP** – Developers want changes to take effect without restarting; a partial implementation exists, but full coverage is eagerly awaited.  
- **Subagent observability & intervention** – Requests for real‑time progress streaming, execution traces, and the ability to pause/redirect subagents when they go off‑track.  
- **Scheduled‑task intelligence** – Conditional gating (preconditions) and better cron fidelity (fixing step‑value parsing) are emerging as key automation improvements.  
- **Project‑local persistence** – `todosDirectory` and similar settings that commit state to the repository are popular for team workflows.

## 6. Developer Pain Points
- **Image paste broken everywhere** – Multiple issues (#6560, #6590, #6577) confirm that pasting clipboard images is non‑functional on macOS and Windows standalone builds, with root cause linked to missing native modules (`@teddyzhu/clipboard`) and platform keybinding failures.  
- **Authentication & connection instability** – Internal Error on login, subscription not recognized, and auth‑modified env vars causing 401 in new sessions frustrate users across plans.  
- **Debugging experience gaps** – `--debug` flag produces no log file (#6600), and CI triage swallows stderr (#6553), making it hard to diagnose problems.  
- **Resource management crashes** – Glob tool can exhaust heap on large paths (#6614), dense PDFs trigger unrecoverable FILE_TOO_LARGE loops (#6586), and memory content vanishes on compaction (#6487).  
- **Cross‑language UI & missing translation** – Mode‑switch prompts appear mixed Chinese/English (#6582), and several UI components lack consistent i18n.  
- **Tool correctness regressions** – Cron step parsing (#6629) and MCP prompt argument dropping (#6563) silently produce wrong results, eroding trust in automation.

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI Community Digest — 2026-07-10

**Project:** [CodeWhale](https://github.com/Hmbown/CodeWhale) (DeepSeek TUI)  
The v0.8.68 release cycle is in its final hours. The lane‑based execution model is being dogfooded actively, and the community is racing to land performance, provider, and mobile‑platform improvements before the release cut.

---

## 1. Today’s Highlights
- The **v0.8.68 release PR (#4327)** has been opened, signalling that all feature work is frozen and the final changelog/public‑doc pass is underway.
- A flurry of last‑mile PRs hardened **xAI/Grok OAuth**, **Termux/Android builds**, **pricing accuracy**, and **CI responsiveness** – directly addressing long‑standing community requests.
- The canonical execution‑board issue (#4092) continues to anchor the orchestration conversation with 58 comments, reflecting the deep architectural rethink around Fleet, Workflow, and Lanes.

---

## 2. Releases
*No new release published in the last 24 hours. The v0.8.68 release PR #4327 is in preparation.*

---

## 3. Hot Issues
| # | Issue | Why It Matters | Community Reaction |
|---|-------|----------------|--------------------|
| [#4092](https://github.com/Hmbown/CodeWhale/issues/4092) | v0.8.68 execution board: lane order, dependencies, and agent protocol (canonical packet) | Serves as the single entry point for every v0.8.68 agent. It defines the lane taxonomy and dependency order for the whole milestone. | 58 comments; intense design discussion among core contributors and sub‑agents. Seen as mandatory reading for any contributor. |
| [#4032](https://github.com/Hmbown/CodeWhale/issues/4032) | Codewhale not following the constitution | Highlights that the model often ignores project‑specific rules and writes throwaway scripts instead of reusing agreed‑upon tools. | 30 comments; deep frustration from users who rely on constitutional behaviour. Prompts #4313 (Constitution rebalance). |
| [#4042](https://github.com/Hmbown/CodeWhale/issues/4042) (closed) | Environment‑level tool sandboxing for sub‑agents | Confirmed the existence of `--disallowed-tools` and mapped out runtime enforcement contexts. Blocked secure multi‑agent workflows. | 12 comments; resolved by clarifying documentation and confirming existing flags. Security‑conscious users relieved. |
| [#4014](https://github.com/Hmbown/CodeWhale/issues/4014) (closed) | TUI lag and memory pressure from high agent fan‑out | Sessions with 30+ parallel sub‑agents became nearly unusable. Addressed by #3902 and #4243. | 10 comments; urgent pain for heavy users – the performance PRs were some of the fastest‑moving items this cycle. |
| [#4178](https://github.com/Hmbown/CodeWhale/issues/4178) | Stopship workflow as fleet‑backed lane | Dogfooding the new Fleet/Workflow model against real “stopship” issues to validate the architecture. | 9 comments; active experimentation channel; directly influences Phase 1‑3 of the lane runtime. |
| [#4257](https://github.com/Hmbown/CodeWhale/issues/4257) (closed) | Add xAI (Grok) as a first‑class provider | Prior to this, xAI could only be reached via a custom OpenAI‑compatible route. Full integration was a top community request. | 9 comments; enthusiasm from users wanting native Grok support. Closed by #4314. |
| [#4175](https://github.com/Hmbown/CodeWhale/issues/4175) | Fleet / Workflow / Lane / Runtime product model (canonical tracker) | Prevents the four concepts from collapsing into one; links implementation phases. | 8 comments; foundational document for all orchestration work. |
| [#4242](https://github.com/Hmbown/CodeWhale/issues/4242) | Run Termux runtime QA for shell, PTY, config, and TUI startup | Validates the Android arm64 build on real Termux environments before official support. | 7 comments; critical for the official Termux epic (#4236). |
| [#4236](https://github.com/Hmbown/CodeWhale/issues/4236) | Epic: official Termux / Android arm64 support | Responds to long‑standing user requests to run CodeWhale directly in Termux instead of relying on Linux arm64 assets. | 7 comments; popular demand from mobile/nomadic developers. |
| [#4179](https://github.com/Hmbown/CodeWhale/issues/4179) | Phase 3: Workflow gates and handoffs between Fleet roles | Designs explicit approve/block semantics between roles (scout→implementer→reviewer→verifier). | 7 comments; viewed as the final piece of the orchestration puzzle for release‑grade workflows. |

---

## 4. Key PR Progress
| PR | Description | Impact |
|----|-------------|--------|
| [#4327](https://github.com/Hmbown/CodeWhale/pull/4327) | release: v0.8.68 | Final release preparation – bumps versions, changelog, docs. **All feature work is now in.** |
| [#3902](https://github.com/Hmbown/CodeWhale/pull/3902) (closed) | perf(tui): fix the five render/input hot paths | Fixes multiple double‑computation and deep‑clone per‑frame issues (#3896–#3900), dramatically reducing TUI lag. |
| [#4025](https://github.com/Hmbown/CodeWhale/pull/4025) (closed) | ci: light‑classify inert scripts and stop allocating heavy runners for trivial PRs | Eliminates unnecessary macOS/Windows runner usage for docs/script‑only changes, saving CI minutes. |
| [#4310](https://github.com/Hmbown/CodeWhale/pull/4310) (closed) | ci: cut PR critical path and stop rebuilding nightly per merge | Reduces PR CI wall time by removing the nightly rebuild from the critical path, speeding up iteration. |
| [#4314](https://github.com/Hmbown/CodeWhale/pull/4314) (closed) | feat(provider): wire xAI device‑code OAuth entrypoints | Completes first‑class Grok support with OAuth and TUI commands – closes #4257. |
| [#4315](https://github.com/Hmbown/CodeWhale/pull/4315) (closed) | fix(android): build Termux target and stop rustls JVM panic | Makes the Android/Termux target actually buildable and bootable; fixes NDK bindgen and TLS panics. Closes #4236, #4242. |
| [#4313](https://github.com/Hmbown/CodeWhale/pull/4313) (closed) | feat(prompts): rebalance Constitution after v0.8.67 ablation | Restores concise behavioural guidance lost during the v0.8.67 constitution trim, addressing #4032 concerns. |
| [#4323](https://github.com/Hmbown/CodeWhale/pull/4323) (closed) | fix(pricing): apply 2026-07-09 pricing freshness audit | Corrects provider pricing for glm‑5.1, kimi‑2.5, and others based on a multi‑agent audit against official pages. |
| [#4243](https://github.com/Hmbown/CodeWhale/pull/4243) (closed) | perf(tui): migrate runtime_threads maps to parking_lot::Mutex | Finishes parking_lot migration at hot lock sites, reducing contention under high sub‑agent concurrency. Closes #4149. |
| [#4311](https://github.com/Hmbown/CodeWhale/pull/4311) (closed) | feat(models): add GPT‑5.6 and Muse Spark routes | Adds the latest OpenAI family and Meta Model API’s muse‑spark‑1.1 across providers. |

---

## 5. Feature Request Trends
- **Advanced multi‑agent orchestration** – Fleet roles, workflow gates, and lane‑based execution are the dominant theme. Users want deterministic, durable, role‑gated pipelines, not just raw sub‑agent spawns.
- **TUI performance and UX simplicity** – Requests to make the default TUI less “busy” (#4095), reduce typing lag, and present one artifact per delegated unit are top of mind.
- **Expanding provider ecosystem** – Native xAI/Grok support was closed this cycle; interest in custom MCP servers and easy provider OAuth continues.
- **Mobile & edge deployment** – Official Termux/Android support (#4236) marks a new frontier; users want to run the TUI on phones and lightweight devices.
- **Security & sandboxing** – Tool restrictions, worktree isolation for parallel writes, and bounded sub‑agent capabilities are seen as necessary for safe autonomous runs.

---

## 6. Developer Pain Points
1. **Agent misbehaviour / constitution drift** – The model frequently ignores project‑specific scripts and writes temporary throwaways, undermining trust in autonomous workflows.
2. **TUI sluggishness under load** – Even after the current perf fixes, sessions with 30+ agents still strain the UI; users demand a “compact mode” as default.
3. **Unbounded state growth** – `subagents.v1.json` can grow to hundreds of thousands of lines without cleanup, forcing manual file wiping.
4. **Monolithic code paths** – Files like `web_search.rs` (2,881 lines) make it difficult to add or maintain providers; a refactor is requested but deferred to 0.8.69.
5. **MCP service fragility** – Servers that implement only `tools/list` can block the entire connection init, leaving services unavailable.
6. **Pricing staleness** – Out‑of‑date model pricing led to inaccurate cost estimates; the fresh audit (#4323) is appreciated but a long‑term automation mechanism is wanted.
7. **Complexity of new orchestration model** – The Fleet/Workflow/Lane vocabulary is powerful but still confusing for casual contributors, highlighting a documentation gap.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*