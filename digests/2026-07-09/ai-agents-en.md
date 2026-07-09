# OpenClaw Ecosystem Digest 2026-07-09

> Issues: 500 | PRs: 500 | Projects covered: 13 | Generated: 2026-07-09 03:55 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [NanoBot](https://github.com/HKUDS/nanobot)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [PicoClaw](https://github.com/sipeed/picoclaw)
- [NanoClaw](https://github.com/qwibitai/nanoclaw)
- [NullClaw](https://github.com/nullclaw/nullclaw)
- [IronClaw](https://github.com/nearai/ironclaw)
- [LobsterAI](https://github.com/netease-youdao/LobsterAI)
- [TinyClaw](https://github.com/TinyAGI/tinyagi)
- [Moltis](https://github.com/moltis-org/moltis)
- [CoPaw](https://github.com/agentscope-ai/CoPaw)
- [ZeptoClaw](https://github.com/qhkm/zeptoclaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw Deep Dive

# OpenClaw Project Digest — 2026-07-09

## Today’s Overview
OpenClaw shows **high engineering velocity** with 500 issues and 500 PRs touched in the last 24h. The community is actively surfacing deep systemic issues — particularly around session-state integrity, message delivery, and multi-agent coordination. 109 PRs were merged/closed today, reflecting a rapid fix rate, yet the backlog of 391 open PRs and 457 open active issues signals that the project is simultaneously grappling with scaling and architectural complexity. No new release was shipped in the last 24h, so the codebase is currently absorbing a large batch of fixes and features without a formal version bump.

## Releases
*None in the last 24h.*

## Project Progress — Merged / Closed PRs Today
While the full list of closed PRs is not exhaustively shown, several important fixes and improvements were completed today (July 9). Selected notable merges:

*   **Fix: avoid UTF-16 truncation in session title/preview** ([#102090]) – eliminates garbled unicode in derived session names.
*   **Fix: disable Discord WebSocket receiver limits** ([#89041]) – resolves a crash-loop regression on some Discord traffic patterns.
*   **Fix: Control UI surrogate-safe text clamping** ([#102225]) – prevents emoji boundary corruption in the webchat interface.
*   **Fix: split encoded video URLs from captions in Discord** ([#101815]) – corrects media attachment handling.
*   **Fix: preserve text before Feishu media attachments** ([#102397]) – prevents lost visible prose when streaming cards are active.
*   **Fix: surface OpenAI chat completions refusals as visible text** ([#102334]) – improves error transparency.

The active PR queue is dominated by safety/security hardening, provider-specific fixes, and tool-call reliability improvements. Notable active PRs include:

*   **gating xAI server-side tools to xAI models only** ([#97629])
*   **native announceTarget for subagent completion routing** ([#101248])
*   **memorizing exec auto-reviewer verdicts to avoid re-billing** ([#102369])
*   **fixing OAuth-only xAI tools missing from CLI-backend sessions** ([#102003])

## Community Hot Topics (Most Active Discussions)

1.  **Text between tool calls leaks to messaging channels** ([#25592]) – 35 comments, 🦞 diamond lobster priority.
    *   *Underlying need*: Users demand strict isolation between internal LLM reasoning/narration and end-user visible messages, especially across Slack, iMessage, etc.

2.  **Subagent completion silently lost — no retry, no notification, no auto-restart on timeout** ([#44925]) – 21 comments.
    *   *Underlying need*: Reliable subagent orchestration without silent failures, especially for long-running tasks.

3.  **Centralized filename encoding utility for multi-encoding Content-Disposition handling** ([#48788]) – 18 comments.
    *   *Underlying need*: Proper handling of international filenames (Chinese, Japanese, Korean, etc.) across all channel adapters.

4.  **Steer mode does not inject messages mid-turn for main sessions** ([#48003]) – 15 comments, 👍3.
    *   *Underlying need*: Real-time message injection during active tool-calling turns for interactive control.

5.  **Multi-agent orchestration is unstable: concurrent agents add/config overwrites, session-lock failures, detached child work** ([#43367]) – 13 comments.
    *   *Underlying need*: Production-grade multi-agent concurrency and reliable config management.

6.  **Feature: allow private network access in web_fetch** ([#39604]) – 13 comments, 👍11 (most upvoted).
    *   *Underlying need*: Reach internal APIs and services for enterprise workflows.

7.  **Native Anthropic path: replaying historical `thinking` blocks bricks long tool-use threads** ([#94228]) – 13 comments, 👍2.
    *   *Active bug that permanently breaks sessions*; high user pain.

The high comment volume and presence of “diamond lobster” ratings indicate that session-state, message-delivery, and security issues dominate community attention.

## Bugs & Stability

**Critical / P0 regressions (release-blockers):**

*   **Live Docs are ahead of release** ([#48920]) – configuration features documented but not available in stable, causing crash-loops; 7 comments, 👍3.
*   **Session hangs indefinitely when compaction times out, causing repeated duplicate message sends** ([#43661]) – P0, 7 comments, 👍2.

**High-severity open bugs (P1):**

*   **Text-leak between tool calls** ([#25592]) – `impact:session-state, security, message-loss`, with linked PR open.
*   **Subagent completion silently lost** ([#44925]) – `impact:session-state, message-loss`.
*   **Steer mode broken** ([#48003]) – `impact:session-state, message-loss`, linked PR open.
*   **Discord leaks internal tool-call traces** ([#44905]) – `impact:security, message-loss`.
*   **Anthropic native thinking block bricks sessions** ([#94228]) – `impact:session-state, message-loss`.
*   **Orphaned lock files not cleared on gateway restart** ([#49603]) – `impact:session-state, crash-loop`, linked PR open.
*   **Cron isolated agentTurn skips delivery after recovered tool error** ([#94846]) – `impact:session-state, message-loss`.
*   **Agent heartbeat routes to wrong agent’s session** ([#99912]) – new bug (July 4), `impact:session-state, message-loss`.
*   **Google Vertex crash: “Cannot convert undefined or null to object”** ([#38327]) – regression in 2026.3.2, `impact:auth-provider, crash-loop`.

All critical bugs show clear user pain points: **session integrity**, **message delivery reliability**, and **multi-agent state management**. Many have fix PRs already linked or in progress, which suggests the maintainers are actively responding.

## Feature Requests & Roadmap Signals

Top feature requests by user engagement and discussion:

*   **Opt-in private network fetch** ([#39604]) – 11 thumbs-up; likely to land soon given high demand.
*   **MathJax/LaTeX support in Control UI** ([#42840]) – 9 thumbs-up; strong desire for scientific communication.
*   **YAML config support** ([#45758]) – 7 comments, 👍2; DevOps-friendliness.
*   **Pre-reset agentic memory flush** ([#45608]) – 11 comments, 👍4; reduces data loss before session resets.
*   **Configurable mediaLocalRoots for image tool** ([#47856]) – iMessage attachment access.
*   **Gateway lifecycle hooks (onSubagentComplete, onToolCallThreshold, etc.)** ([#43454]) – event-driven extensibility.
*   **Distributed Agent Runtime RFC** ([#42026]) – architectural split of control plane and agent runtime.
*   **First-class Telegram reaction triggers** ([#47677]) – deeper Telegram integration.
*   **Per-agent cost budget enforcement** ([#42475]) – spend governance.
*   **Centralized filename encoding** ([#48788]) – cross-channel internationalization.

**Prediction for next release:** The next version will likely focus on **stability over features** — fixing the session-state and message-leak bugs that dominate P0/P1. The private network fetch and MathJax features may slip in if they are low-risk, but the overall shape indicates a patch release (e.g., 2026.3.14 or 2026.7.10) resolving the compaction/timeout crashes and tool-call trace leaks.

## User Feedback Summary

Users are **deeply engaged but clearly strained** by three classes of issues:

1.  **Silent failures and data loss** – subagent completions vanish, cron sessions overwrite files, session locks become stale. Trust in agent autonomy is eroded when results disappear.
2.  **Leakage and security** – internal tool-call commentary appearing in Slack/Discord/iMessage, untrusted issue bodies injected into prompts. Users want **strict boundary enforcement**.
3.  **Configuration and documentation mismatch** – live docs reference features not yet released, environment variables create nested directories, heartbeat settings route to wrong agents. The DX gap between documented and actual behaviour is a recurring frustration.

Nevertheless, the community is **actively contributing fixes and detailed repro cases**. The thumbs-up counts on feature requests show enthusiasm, and many issues include test reproduction steps, demonstrating a mature user base invested in the project’s success.

## Backlog Watch — Items Needing Maintainer Attention

Several important issues and PRs have been open for weeks with **no assigned fix or are marked `stale`**, yet they affect core functionality:

*   **Feishu: read image tool result loses media before final outbound payload** ([#41744]) – stale, P1, message loss, no fix PR.
*   **Early abort response templates not populated** ([#45314]) – stale, 8 comments, P2, linked PR but no maintainer review.
*   **OPENCLAW_HOME produces nested .openclaw/ directory** ([#45765]) – stale regression, 8 comments, linked PR open.
*   **Cost dashboard omits .jsonl.reset archive files** ([#46252]) – stale, undercounting spend, linked PR open.
*   **Subagent sessions persist after completion, main session unresponsive** ([#47975]) – P1, `needs-live-repro`, no fix PR.
*   **Playwright assertion error crashes Gateway** ([#45224]) – P1, crash-loop, `needs-live-repro`.
*   **Cron agent jobs silently time out during LLM outages** ([#45494]) – regression, `needs-live-repro`, no fix PR.

These items represent **latent reliability risks** that could suddenly trigger user-facing incidents. Maintainer triage is needed to either escalate them or confirm they are already addressed in pending PRs.

---

*Data window: 2026-07-09 24h activity on [openclaw/openclaw](https://github.com/openclaw/openclaw).*

---

## Cross-Ecosystem Comparison

# Cross-Project Comparison Report — Personal AI Assistant Ecosystem

## 1. Ecosystem Overview
The open‑source personal AI assistant and agent space remained highly active on July 9, 2026, with several projects undergoing rapid iteration and others focusing on stability hardening. The landscape is clearly led by OpenClaw as the reference implementation with massive community scale, while a diverse set of specialized forks, alternatives, and niche tools address specific deployment models, operating environments, and user interfaces. Shared pain points across nearly all projects are session‑state integrity, message delivery reliability, and multi‑agent coordination, signaling that the next wave of maturity will be defined by robustness rather than raw features. A notable trend is the convergence on OpenAI‑compatible APIs and runtime plugin architectures to improve interoperability and extensibility.

## 2. Activity Comparison
Metrics reflect total issues and pull requests touched (opened, updated, or closed) in the 24‑hour window, as reported in the individual digests. Health scores are analyst‑assessed, factoring in responsiveness, known regressions, and current backlog pressure.

| Project        | Issues (24h) | PRs (24h) | Release in 24h? | Health Score & Notes                     |
|----------------|--------------|-----------|------------------|-------------------------------------------|
| **OpenClaw**   | ~500         | ~500      | None             | Moderate – massive scale, high fix rate but many P0/P1 bugs and backlog |
| **NanoBot**    | 21           | 23        | None             | High – swift security responses, active polish |
| **Hermes Agent**| 50           | 50        | None             | Moderate – heavy merge day, but critical desktop crash regression |
| **PicoClaw**   | 2            | 4         | None             | High – stable, incremental improvements |
| **NanoClaw**   | 2            | 28        | None             | Moderate – high velocity but critical silent‑drop bug exists |
| **NullClaw**   | 0            | 0         | –                | Quiet – no activity recorded |
| **IronClaw**   | 18           | 50        | None             | Moderate – bug‑bash phase, several P2 routine failures |
| **LobsterAI**  | 3            | 12        | None             | High – responsive, critical fix merged same‑day |
| **TinyClaw**   | 0            | 1*        | None             | High – quiet, but recent security hardening merged |
| **Moltis**     | 0            | 1         | None             | High – stable, one panic fix proposed |
| **CoPaw (QwenPaw)**| 34        | 46        | v2.0.0‑beta.4    | Low – fast beta iteration, multiple data‑loss and crash bugs |
| **ZeptoClaw**  | 0            | 0         | –                | Quiet – no activity recorded |
| **ZeroClaw**   | 50           | 50        | None             | Moderate – high ambition, but many stale critical bugs |

*TinyClaw PR: merged on July 8; no new same‑day activity otherwise.*

## 3. OpenClaw’s Position
**Advantages over peers:** OpenClaw’s community is an order of magnitude larger, with ~500 issues and ~500 PRs touched daily. It supports the widest range of messaging channels, providers, and agentic tools, and serves as the upstream for several other projects in this comparison (e.g., LobsterAI explicitly generates OpenClaw configurations). The contributor base regularly lands 100+ merged PRs per day, dwarfing all others.

**Technical approach differences:** OpenClaw is a monolithic, high‑integration gateway that handles every aspect of session management, tool execution, and channel adaptation directly. In contrast, peers like NanoClaw and ZeroClaw offer similar multi‑channel functionality but with a lighter footprint (Rust in ZeroClaw’s case) or a more CLI‑focused developer experience. Hermes and CoPaw concentrate heavily on rich desktop UI and visual workflows, while OpenClaw’s primary interfaces remain web‑based and CLI.

**Community size comparison:** OpenClaw’s issue and PR volume is 5–10× that of the next most active projects (ZeroClaw, Hermes, IronClaw, CoPaw). This scale is both a strength (vast ecosystem) and a challenge (P0/P1 regressions in session integrity, multi‑agent config, and live‑docs mismatch create daily user pain).

## 4. Shared Technical Focus Areas
Several requirements repeatedly surface across the ecosystem, indicating industry‑wide priorities:

- **Session‑state integrity and message delivery reliability**  
  *OpenClaw, Hermes, NanoClaw, CoPaw, ZeroClaw* – silent message drops, missing subagent completions, and context‑compaction data loss are the top user complaints. All projects are evolving their session lifecycle, scroll/compaction logic, and retry mechanisms.

- **Multi‑agent coordination and isolation**  
  *OpenClaw, LobsterAI, NanoBot, ZeroClaw* – users want distinct agent workspaces, safe concurrent tool execution, and explicit control planes (list/kill subagents). Accidental cross‑agent config overwrites and heartbeat routing errors highlight the need for stronger isolation.

- **Context management (compaction, thinking blocks, tool‑call history)**  
  *CoPaw, Hermes, ZeroClaw, OpenClaw* – both automated context summarization and preservation of reasoning and tool‑call sequences are critical. Multiple projects are fixing trimming bugs (CoPaw’s scroll, Hermes’ summarization stalls) and adding per‑tool‑output compression proposals.

- **Security hardening: authentication, file access, and message leakage**  
  *NanoBot, TinyClaw, OpenClaw, ZeroClaw* – unauthenticated token minting (NanoBot), internal tool‑trace leaks (OpenClaw, NanoBot), and workspace file protection (.ignore files in ZeroClaw, path‑traversal fixes in TinyClaw) are top concerns.

- **Cross‑channel internationalisation and filename handling**  
  *OpenClaw, Hermes* – UTF‑16 truncation, Chinese character rendering bugs on Windows, and non‑ASCII filename percent‑encoding errors demand centralized encoding utilities and platform‑aware fixes.

- **OpenAI API compatibility and standardisation**  
  *ZeroClaw, NanoBot* – exposing `/v1/chat/completions` and tightening the provider contract are seen as high‑demand features that enable broader client integration.

## 5. Differentiation Analysis
While all projects are personal AI assistants/agent frameworks, they diverge sharply in focus, target audience, and architectural choices:

- **OpenClaw** – the universal, self‑hosted gateway for power users and developers; highly modular but also complex, with the deepest provider and channel support.
- **NanoBot** – polished WebUI with a strong emphasis on guided onboarding, subagent management, and task‑specific model routing; geared toward users who want a complete out‑of‑the‑box assistant.
- **Hermes Agent** – rich desktop‑native experience (Electron) with Kanban plugin SDK, extensive QQ/WeCom integration, and a focus on the Chinese market. Its architectural rewrite (layout‑tree shell) signals a long‑term vision of a plugin‑hosted desktop.
- **PicoClaw** – edge‑oriented gateway for Sipeed hardware (NanoKVM) with monitoring integrations; lightweight, targeting embedded/IoT use.
- **NanoClaw** – CLI‑first, developer‑focused fork with scheduled tasks and Discord automation; positions itself as a scriptable agent runtime.
- **IronClaw** – enterprise automation platform with routines, Trace Commons observability, multi‑tenant workspaces, and NEA‑25 extension manifest; currently in a v2 “Reborn” rewrite.
- **LobsterAI** – IM‑centric multi‑agent coworking with native OpenClaw integration; strong in memory management and agent scoping for collaboration.
- **TinyClaw** – minimalist, security‑audited agent framework with heavy emphasis on allowlists and safe file handling; low community noise.
- **Moltis** – specialized calendar‑sync agent; narrow scope, focused on robust CalDAV interoperability.
- **CoPaw** – user‑facing desktop/console agent (QwenPaw v2) with a rapid beta cadence, strong memory and scroll work, and a growing set of IM channels; currently unstable but ambitious.
- **ZeroClaw** – Rust‑based agent emphasizing compile‑to‑WASM plugins, in‑app upgrades, and an OpenAI‑compatible endpoint; aiming for a high‑security, extensible runtime.

## 6. Community Momentum & Maturity
Activity can be grouped into three tiers:

- **Hyper‑active, scaling hard:** OpenClaw, ZeroClaw, Hermes, IronClaw, CoPaw. These projects are either in heavy bug‑bashing phases (IronClaw, CoPaw) or continuously merging large feature sets (OpenClaw, ZeroClaw, Hermes). They exhibit high issue throughput but also accumulate regressions and stale backlogs.

- **Actively maintaining, responsive:** NanoBot, NanoClaw, LobsterAI. Rapid security fixes (NanoBot), steady feature trains (NanoClaw scheduled tasks), and same‑day critical bug corrections (LobsterAI) demonstrate healthy, sustainable cadences.

- **Stabilizing or quiet:** PicoClaw, TinyClaw, Moltis, NullClaw, ZeptoClaw. These projects release few changes, reflecting either high stability, niche scopes, or low resource availability. No negative signals, but no rapid evolution either.

Maturity varies: OpenClaw and NanoBot have the most extensive feature sets and user bases, while CoPaw and IronClaw v2 are clearly in early‑adopter/beta territory. ZeroClaw is architecturally maturing but still wrangling the transition from compile‑time features to runtime plugins.

## 7. Trend Signals
Extracted directly from community feedback and roadmap discussions, the following signals are valuable for AI agent developers and product planners:

- **Reliability is the new frontier** – message loss, silent failures, and context corruption are the top complaints across the board. Users now demand exactly‑once delivery, clear error propagation, and deterministic session management over new capabilities.

- **Multi‑agent is entering production** – the move from single‑assistant to cowork/subagent patterns requires robust isolation (workspaces, configs, identities) and operational tooling like `list`/`kill` commands. Projects that don’t solve this will lose advanced users.

- **Security must be built in, not bolted on** – unauthenticated endpoints, internal tool‑trace leaks, and missing file sandboxing have led to critical incident responses in multiple projects. Expect .ignore file mechanisms, secret‑scoped environment variables, and PR‑review hardening to become table stakes.

- **OpenAI API compatibility is a requirement** – enabling standard clients (Open WebUI, LobeChat, custom SDKs) via `/v1/chat/completions` is a frequently requested feature; projects without it risk being bypassed for simpler integrations.

- **Desktop and TUI UX matters for trust** – users want to see full terminal commands, interleaved thinking/tool calls, and scrollable approvals. Rich, transparent interfaces (Hermes, CoPaw) build confidence in agent autonomy.

- **Internationalisation gaps are a real blocker** – non‑ASCII filenames, multi‑byte text truncation on Windows, and localized UI elements are hindering adoption outside English‑speaking markets. Centralized encoding utilities and platform‑specific fixes are becoming a necessity.

- **Runtime extensibility over compile‑time features** – both ZeroClaw (WASM plugins) and IronClaw (extension manifest v2) are moving toward a world where channels and tools can be installed post‑ship, reducing binary size and enabling community‑led ecosystems.

These insights reflect the collective experience of thousands of users and contributors, making them a practical compass for anyone building or deploying personal AI agents in the near term.

---

## Peer Project Reports

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

# NanoBot Project Digest — 2026-07-09

## 1. Today’s Overview
The project saw a burst of activity in the last 24 hours with 21 issues updated (8 open, 13 closed) and 23 pull requests updated (12 open, 11 merged/closed). No new release was published. The team moved quickly to close several security‑related issues and merge multiple enhancements, signalling a healthy, responsive maintenance cadence. The focus remained on hardening authentication flows, polishing the WebUI, and enriching the agent’s tooling, while architectural discussions around model routing and sub‑agent control continued in the community.

## 2. Releases
No new releases were published in this period.

## 3. Project Progress
The following features and fixes landed through merged or closed pull requests:

- **WebUI Diff View** (#4828) – File edits now render as GitHub‑like unified diffs, with per‑user toggling and collapsible large diffs.
- **Localhost Bootstrap Token Restoration** (#4856) – Restored full localhost WebUI bootstrap for configs without a secret; required for the embedded WebUI to function again after recent hardening.
- **Node.js Bump** (#4460) – Upgraded the runtime to Node 24, keeping the web frontend on a current LTS line.
- **Telegram Vision Support** (#12) – Base64‑encodes images sent via Telegram and passes them to the LLM in multimodal format, enabling image understanding.
- **Config Refresh Without Interaction** (#4852) – Added `nanobot onboard --refresh` to update configuration files in automated setups.
- **Documentation & Search Improvements** (#4850) – Restructured the README and added search‑oriented guide entry pages for common tasks.
- **Internal Refactors** (#4848, #4849) – Extracted turn‑hook assembly from the agent loop; split WebUI bootstrap WebSocket tokens from REST API tokens. Both improve code maintainability and token‑issuance clarity.

## 4. Community Hot Topics
The most active discussion items (by comments/reactions) reveal where user interest is concentrated:

- **Task‑Specific Model Configuration** (#912, open, 5 comments, 👍3) – Users want to assign different models (e.g., for conversation vs. tool use vs. browser use). This remains the top‑voted open feature request.
- **Prompt Prefix Preservation Architecture** (#2463, closed, 13 comments) – A detailed architectural discussion about whether the agent should re‑send the identical prompt prefix. The thread generated significant debate before it was closed.
- **Subagent Control Plane MVP** (#1006, open, 3 comments) – Proposes `list`/`kill` commands for running subagents, pointing to a need for better operational visibility.
- **SimpleX Chat Channel** (#240, open, 👍2) – Interest in a decentralized, phone‑number‑free channel alternative.

## 5. Bugs & Stability
Several security vulnerabilities were reported and fixed within hours, demonstrating a high‑severity response capability:

- **Critical – Unauthenticated Token Minting** (#4825, #4827, #4826) – Any local process could call `/webui/bootstrap` and obtain an API bearer token. Fixed via PR #4849 and #4856; all related issues are closed as of 2026-07-08.
- **Critical – Unauthenticated OpenAI‑Compatible Endpoint** (#4078) – The `/v1/chat/completions` endpoint accepted unauthenticated requests. Closed on 2026-07-08.
- **Medium – Progress Streaming Leaks Internal Tool Calls** (#954, closed) – User‑facing chat displayed `exec()` and other internal tool calls. Addressed.
- **Medium – minimax-m2.7 Failure on 2nd Request** (#2450, closed) – Ollama Cloud users saw `APIConnectionError` after the first successful call. Resolved.
- **Open – Unbounded Disk Growth from Media** (#896, 1 comment) – Telegram and Discord media files are never deleted, potentially exhausting disk space. No fix merged yet.
- **Open – Agent Cannot Access Host Filesystem** (#940, 2 comments) – Skills written by the agent land in a sandbox workspace, not the actual installation folder, blocking media processing and skill portability. Still open.

## 6. Feature Requests & Roadmap Signals
The following requests are likely candidates for upcoming versions based on popularity, open pull requests, and community signals:

- **Task‑Specific Models** (#912) – Already highly upvoted, this would allow fine‑grained model selection per operation type.
- **Subagent Control Plane** (#1006) – Adding operational commands (`list`/`kill`) for subagents is a natural next step after basic spawn support.
- **Multi‑Tenant Gateway** (#936) – A single gateway configuration managing multiple agents; would drastically simplify deployment.
- **Core `nano_timer` Tool** (open PR #4853) – Adds a simple time/calendar tool; likely to be merged soon.
- **Guided Channel Setup Flows** (open PR #4855) – Productizing the channel onboarding experience; already in review.

## 7. User Feedback Summary
Users continue to push for better operational control and more flexible model usage. Complaints about hallucinations in the `exec` tool (#937) were acknowledged, though the issue was closed as it reflects model behaviour rather than a library defect. Security concerns around token issuance were met with swift, transparent fixes, which helped maintain trust. Persistent pain points include the inability to cleanly expose the host filesystem to the agent (#940) and the lack of media file lifecycle management (#896). Overall, the community appears engaged and constructive, with feature requests aligned to real deployment needs.

## 8. Backlog Watch
Several important issues have remained open without resolution for months (all marked `stale` from February 2026). These could become blockers or cause recurring problems:

- **#896 – Media files never cleaned up** (opened 2026-02-20, 1 comment). Unbounded disk growth in production environments deserves attention.
- **#940 – AI agent cannot access host filesystem** (2026-02-21, 2 comments). This limits skill reuse and media processing; a core workflow limitation.
- **#931 – Native sandbox interface for untrusted plugins** (2026-02-21, 1 comment). Security‑conscious users are asking for isolation before the plugin ecosystem grows.
- **#990 – Pre‑handler hook for zero‑token message routing** (2026-02-22, 1 comment). Minor but could save tokens in high‑volume channels.
- **#912 – Task‑specific model configuration** – Though recently discussed, it dates from 2026-02-20 and would benefit from a formal design proposal to unblock contributors.

*All links lead to the corresponding GitHub issue or pull request on [HKUDS/nanobot](https://github.com/HKUDS/nanobot).*

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

## Hermes Agent Project Digest – 2026-07-09

### 1. Today’s Overview
The repository saw equally high activity on both Issues and PRs (50 each in the last 24 hours), with **29 PRs merged/closed** and only **2 issues closed**, indicating a heavy merge day after a burst of recent fixes. No new releases were tagged, but the community is grappling with a desktop update (`0.17.0`) that introduced a critical crash loop, as well as a flurry of bug reports around platform integrations (WeCom, QQ bot) and session lifecycle. The maintainers closed a large batch of reliability and edge-case fixes, while two ambitious design-overhaul PRs (desktop architecture and Kanban plugin) remain open for community feedback. Overall, the project’s momentum is high, with a focus on hardening multi-platform support and rethinking the desktop shell.

### 2. Releases
No new official release was published today. (The desktop app `0.17.0` was recently deployed via the auto-updater but not via a GitHub release.)

### 3. Project Progress – Merged & Closed PRs (today)
Numerous fixes and quality-of-life improvements advanced, all by contributor `x7peeps` unless noted:

- **Vision auth fix** [#47038] – OpenRouter credential silently dropped in auxiliary vision client → 401s fixed.  
- **Conversational followup routing** [#47043] – Gateway now detects replies to the bot’s own messages and routes them as FIFO queued events, preventing stream interruption.  
- **PII-safe platform detection** [#47045] – Plugin authors can mark their platform as `pii_safe`, avoiding hardcoded logic.  
- **Windows crash resilience** [#47055] – Faulthandler, DNS resolution errors, and asyncio transport OSError handled to prevent silent crashes on Windows.  
- **OpenCode Go model catalog** [#47104] – Added 5 missing models (minimax-m3, deepseek-v4-flash, etc.) to the curated list.  
- **Curator locked categories** [#47108] – New config `curator.locked_categories` to exempt entire skill categories from auto-transitions, avoiding tedious per-skill pinning.  
- **Context reference expansion** [#57780] [#59043] – `asyncio.gather` calls now use `return_exceptions=True`, preventing a single failed URL/file reference from killing all expansions.  
- **MCP subprocess timeouts** [#60820] – `subprocess.run` in MCP bootstrap/git clone now has timeouts to avoid indefinite hangs.  
- **Dashboard cron scans offloaded** [#45491] (by `BlackishGreen33`) – Profile scanning moved off the FastAPI event loop for better responsiveness.  
- **Desktop dev boot fix** [#61277] (by `OutThisLife`) – Stopped using `tsx` to boot Electron main after the TS migration caused a startup crash.

### 4. Community Hot Topics

- **[#39691] feat(compression): integrate headroom-ai for tool output compression** (9 comments, 👍12)  
  Users want the agent to compress *individual tool outputs* (not just the whole conversation) to reduce token waste. The proposal to integrate the `headroom-ai` library has strong support, reflecting a pain point with large tool results bloating context and causing premature summarisations.

- **[#59224] Bug: Classic CLI `/resume` listing hides Desktop (and other non-CLI) sessions** (8 comments)  
  The resume listing filters by `source="cli"`, making sessions started from Desktop or WebUI invisible. A usability gap that confuses cross-client workflows; the discussion highlights the need for a unified session inventory.

- **[#39534] [Bug]: Chinese prompt cut off in Desktop chat window on Windows** (7 comments)  
  Multi-byte character rendering issue that truncates prompts after certain updates; important for international users and widely reported with screenshots.

- **[#58646] QQ bot adapter startup failure – `is_reconnect` keyword argument** (7 comments)  
  The gateway passes `is_reconnect=True` to `QQAdapter.connect()`, which doesn’t accept it, causing a `TypeError`. High impact for QQ bot users; currently unresolved.

- **[#18241] Feature: TUI show thinking blocks and tool calls in chronological order** (4 👍)  
  Reasoning models interleave thought and actions; users want the TUI to preserve that order for transparency rather than grouping by type.

### 5. Bugs & Stability

| Severity | Issue | Description | Fix PR? |
|----------|-------|-------------|---------|
| 🔴 Critical | [#61268] (closed, likely hotfixed) | Desktop 0.17.0 update throws `Composer is not available` error and causes infinite restart loop via updater. | None directly; possibly rolled back. |
| 🔴 High | [#61220] | Session expiry finalization does not set `end_reason='session_reset'`, allowing stale recovery to reopen expired sessions with full history. | No |
| 🔴 High | [#61270] | Gateway startup blocks on Nous Portal OAuth `device_code` exhaustion with no user-visible error; tool gateway fails to degrade. | No |
| 🟠 Medium | [#61195] | `delegation.base_url` is correctly resolved but the subagent’s API call still routes to OpenRouter, causing 401. | #61275 (open) |
| 🟠 Medium | [#61211]/[#61212] (duplicate) | WeCom file upload with Chinese filename percent-encoding exceeds Windows `MAX_PATH`, causing `FileNotFoundError`. | No |
| 🟠 Medium | [#60715] | Nous inference API completely unreachable for some users; all models timeout. Possibly regional/infrastructure. | No |
| 🟡 Low | [#61207] | `/plan` no longer writes a `plan.md` file. Regression, but quickly flagged. | No |
| 🟡 Low | [#35419] | Fallback provider activation is silent on messaging gateways (notices never flushed). | Open, no PR |

*Other known regressions still open:* [#48098] stale “Summarizing thread” status after compaction resumes, [#5254] tool call repeating with LM Studio, [#39838] `notification_sources` config documented but never read.

### 6. Feature Requests & Roadmap Signals

- **[#61246] Minimize to System Tray on Close** – Windows users especially want the same behaviour as QQ/WeChat; this could be a quick win for desktop UX.
- **[#61193] Show full terminal commands in Desktop** – Commands run by the agent are truncated; a rudimentary expand or tooltip would greatly improve trust.
- **[#61249] Approval bar truncation** – Multi-line diffs are cut to one line; users want a scrollable preview before approving tool calls.
- **[#53617] Option to keep reasoning panel expanded** – Thinking models’ reasoning panel auto-collapses; persistent expand is requested for transparency.
- **[#52807] UI for configuring custom_providers** – Avoid manual YAML edits; likely to gain traction as third-party providers proliferate.
- **[#50718] Unread markers, “needs input” cues, and OS badges** – Session management polish that would benefit Desktop and TUI.
- **Ambitious redesigns (open PRs):** [#60638] contribution-driven desktop shell on a layout tree, [#61173] Kanban plugin built on that SDK. These suggest a long-term vision of a modular, plugin-hosted desktop UI, but they’re still under review.

Prediction for next release: Low-hanging fruit like system-tray minimize, full-command visibility, and the `delegation.base_url` fix (#61275) are likely to land first; the larger desktop overhaul will need longer incubation.

### 7. User Feedback Summary

- **Frustration with Desktop regressions:** 0.17.0 crashed the app on startup, leaving users in an unrecoverable update loop. The fix was assumed to be rolled back or hotpatched quickly, but it shook confidence.
- **Internationalization pain:** Chinese and non-ASCII filenames/inputs cause rendering glitches (Desktop prompt cut-off) and file operations failures (WeCom filename exceeding path limit). This is a recurring theme and reflects Windows-specific platform bugs.
- **Clarity and transparency:** Users consistently ask for more visibility—seeing the full terminal commands, the reasoning process in order, and multi-line approval diffs—underscoring a desire to understand and trust the agent’s actions.
- **Session management confusion:** Hidden Desktop sessions from `/resume` and missing unread/input cues make multi-client workflows error-prone.
- **Positive note on contributor velocity:** The community appreciates the rapid-fire fixes from `x7peeps` and the ambition of `OutThisLife`’s desktop rewrite, though the latter’s scope makes some wary of instability.

### 8. Backlog Watch – Items Needing Maintainer Attention

- **[#39691] Tool output compression** – 9 comments, 12 👍, a well-defined feature with high efficiency impact. No active PR visible; could be a candidate for a community contribution.
- **[#59224] `/resume` listing bug** – 8 comments, clear usability issue that spans multiple interfaces. Might be quick to fix but seems to have stalled.
- **[#39534] Chinese text cutoff on Windows** – 7 comments, affects a growing user base. No resolution after a month.
- **[#5254] Tool call repetition with LM Studio** – 4 comments, linked to an OpenAI Codex issue; suggests a provider-level inefficiency that may need upstream coordination.
- **[#18241] Chronological thinking/tool order in TUI** – 4 👍, but not a high comment count; still, it’s a frequently requested improvement for reasoning model users.
- **Old open PRs [#30196] (gateway --replace across profile mismatch)** and **[#52829] (fork child exclusion in resume walk)** both aim to fix subtle state management bugs; they’ve been open for weeks and could silently impact reliability.

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

## PicoClaw Project Digest – 2026-07-09

### 1. Today’s Overview
In the past 24 hours, the repository saw moderate maintenance and enhancement activity: 2 open issues, 1 open pull request, and 3 merged/closed PRs. No new releases were published. The closed PRs delivered a new monitoring channel, a gateway reliability improvement, and a critical bug fix for Anthropic vision models. Community discussion remained modest, centred on a NanoKVM compatibility bug and a feature request for QQ streaming. The overall pulse suggests a healthy but quiet development cycle focused on incremental quality-of-life improvements.

### 2. Releases
*No new releases today.*

### 3. Project Progress
Three pull requests were merged/closed today, advancing both features and stability:

- **Gateway bind fallback** ([#2278](https://github.com/sipeed/picoclaw/pull/2278)) – When the configured loopback bind fails, the gateway now automatically falls back to a wildcard bind protected by a CIDR allowlist. Improves startup reliability on constrained or containerised environments.
- **Grafana Alertmanager webhook channel** ([#2251](https://github.com/sipeed/picoclaw/pull/2251)) – A new input-only channel that accepts webhook payloads from Grafana Alertmanager, formats them into readable messages, and can trigger skills. Expands PicoClaw’s monitoring integration ecosystem.
- **Anthropic vision fix** ([#3234](https://github.com/sipeed/picoclaw/pull/3234)) – Image media attached to user messages were being silently dropped for the Anthropic provider, causing vision models to respond “can’t see”. The fix embeds images directly in the request body, restoring vision support.

### 4. Community Hot Topics
- **[Issue #3195](https://github.com/sipeed/picoclaw/issues/3195)** – *[BUG] OpenAI GPT does not work on NanoKVM with default config*  
  2 comments. Users trying to run PicoClaw on Sipeed’s NanoKVM hit a hard blocker when following the documented model configuration for GPT models. The exchange suggests a platform-specific configuration mismatch or missing default.  
  **Underlying need:** Seamless out-of-the-box operation on Sipeed’s own hardware platform.

- **[Issue #3201](https://github.com/sipeed/picoclaw/issues/3201)** – *[Feature] Support streaming output for QQ channel*  
  1 comment. The QQ channel currently lacks token-by-token streaming, unlike Telegram and WebSocket channels. Users want a more interactive LLM experience.  
  **Underlying need:** Feature parity across all supported messaging channels.

- **[PR #3226](https://github.com/sipeed/picoclaw/pull/3226)** – *fix(tools): stop write_file from coaching destructive overwrite*  
  Open, no comments yet. The change prevents the `write_file` tool from framing an existing file as “replace” and coaching the model toward overwriting it, addressing a subtle safety concern.  
  **Underlying need:** Safer file operations when the agent updates its own memory.

### 5. Bugs & Stability
- **High** – [Issue #3195](https://github.com/sipeed/picoclaw/issues/3195): OpenAI GPT models unusable on NanoKVM with default configuration. Blocks the entire use case for users on that platform. No fix or maintainer response yet.
- **Resolved** – [PR #3234](https://github.com/sipeed/picoclaw/pull/3234): Anthropic vision models could not see attached images; fixed by patching the provider request builder.
- No other regressions or crashes have been reported in the last 24 hours.

### 6. Feature Requests & Roadmap Signals
- **QQ channel streaming** (Issue #3201) is a clear feature request with direct comparison to existing channels. Its presence signals a natural roadmap extension for channel capabilities.
- The merge of the **Grafana Alertmanager channel** suggests the project is interested in growing its monitoring/alerting ecosystem; similar webhook adapters may follow.
- **Tool behaviour refinement** (PR #3226) indicates an ongoing effort to make the agent’s tool usage safer and less destructive, possibly leading to a dedicated memory‑write tool.
- **Prediction:** A near-term release could bundle QQ streaming support, along with the file‑write safety improvement and the gateway fallback already in `main`.

### 7. User Feedback Summary
- **Pain point:** Deploying PicoClaw on a Sipeed NanoKVM is currently broken for GPT users, leading to immediate frustration.
- **Desired UX:** Users expect the same real‑time streaming feel on QQ that they enjoy on Telegram, indicating that channel‑specific gaps hurt perceived quality.
- **Safety concern:** The community is wary of the agent being inadvertently guided to overwrite files, hence the proactive fix PR.
- **Engagement:** Despite the issues, users are filing detailed bug reports and contributing improvements, showing sustained interest in the project.

### 8. Backlog Watch
- **[Issue #3195](https://github.com/sipeed/picoclaw/issues/3195)** (open since 2026-06-30) – NanoKVM GPT compatibility bug has not yet received a maintainer reply or diagnostic guidance. A fix is needed to unblock Sipeed’s own hardware users.
- **[Issue #3201](https://github.com/sipeed/picoclaw/issues/3201)** (stale, since 2026-07-01) – The QQ streaming request risks being forgotten. A maintainer acknowledgement or “planned” tag would set expectations.
- Both items are relatively fresh but warrant attention to keep the community confident that voices are heard.

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw Project Digest — 2026-07-09

## Today’s Overview
Development velocity remains high with 28 pull requests updated in the last 24 hours, including 4 merges or closures, and 2 new issues opened (none closed). Core-team work is heavily concentrated on scheduled tasks, agent template wizards, and provider robustness. A critical silent failure bug in the opencode provider (#2985) was reported, while several quality-of-life enhancements around Discord and CLI surfaced. No new releases were made today.

## Releases
None today.

## Project Progress (Merged / Closed PRs)
- **#1702 – Fix IPC message loss loop** *(closed)* – `fix: break for-await loop on result to prevent IPC message loss`. Resolves a low‑level agent‑runner bug where messages could be dropped during for‑await iteration.
- **#2978 – Core-team labelling automatio** *(closed)* – CI workflow change: PRs from core‑team members now automatically receive a `core-team` label, streamlining triage.
- **#2980 – `ncl` CLI verb‑level arguments and server‑rendered human view** *(closed)* – Part 1 of the scheduled‑tasks train. Adds strict argument validation, deep help, and a human‑friendly rendering layer for CLI responses.
- One additional closed PR not displayed in the top 20 – further advancement in tooling or fixes.

## Community Hot Topics
The graph shows “top 20 by comment count” but individual comment counts are not present; the following items are the most significant in terms of impact and activity:

- **#2985 – opencode provider silently drops replies**  
  *[Issue]* – The bot produces no reply and no error when `session.idle` is missed in the final text snapshot, leaving the agent’s answer unread. This is a high‑priority delivery failure affecting long‑running agentic turns.  
  → Underlying need: reliable message delivery across all providers, especially under heavy load.

- **#2981 – Scheduled tasks control plane and isolated sessions**  
  *[PR, open]* – Part 2 of the scheduled‑tasks train, introducing `ncl tasks`, per‑series isolated sessions, run history, and a pre‑task approval gate. This is a large feature that will bring cron‑like scheduling to self‑hosted NanoClaw instances.

- **#2984 – Auto‑rename Discord threads by topic** *(Issue)* – Requests a `rename_thread` tool so agents can give descriptive names to Discord threads instead of date‑stamped defaults.  
  → Underlying need: better organisation and scan‑ability in busy Discord servers.

- **#2742 – The PR Factory recipe** *(PR, open since June 11)* – A published recipe that spawns a worker agent on every new PR, reviews it, and posts a threaded Slack summary. This has drawn interest as a blueprint for autonomous project workflows.

## Bugs & Stability (Ranked by Severity)
1. **Critical: OpenCode provider silent no‑reply (#2985)** – Agent turn completes but no message is delivered and no error is raised; users experience a “bot ignored me” failure. No fix PR yet.
2. **High: Claude tool allowlist mismatch with pinned CLI (#2982)** – `TOOL_ALLOWLIST` references tools that don’t exist on the pinned `claude-code` CLI (`Task`, `TodoWrite`, etc.). This can cause agent tool‑calling failures or unexpected errors. Fix PR open.
3. **Medium: WhatsApp Cloud bridge instance key collision (#2913, fix open)** – The bridge hardcodes `whatsapp` as the instance key, colliding with the native Baileys adapter; a distinct `whatsapp-cloud` key is being introduced.
4. **Medium: Codex file event delivery (#2770)** – Codex images generate `file` events that the provider union doesn’t declare, breaking builds and dropping the image from delivery. Fix PR open.
5. **Low: Discord bare URLs wrapped as masked links (#2979, fix open)** – A dependency bump corrects a visual annoyance where plain URLs were shown as masked links.

## Feature Requests & Roadmap Signals
- **Scheduled tasks (#2981)** – A full task orchestration system with `ncl tasks`, run history, and pre‑task gating. This is the most likely feature to land in the next release cycle, given its multi‑part integration and core‑team focus.
- **Discord thread auto‑rename (#2984)** – A standalone tool that lets the agent rename threads by topic, improving Discord channel organisation. Likely a quick, high‑value addition.
- **Agent template wizard (#2909)** – Setup wizard flow that stamps a first agent from pre‑defined templates; part of a broader template system (#2890).
- **Reject‑with‑reason for OneCLI credential cards (#2941)** – Extends the approval system to allow operators to reject credential prompts with a reason, which is injected back into the failed tool call.
- **Instance‑wide default agent provider (#2906)** – Simplifies multi‑group management by allowing a single default AI provider for new groups.

## User Feedback Summary
- Pain points: silent message drop (#2985) erodes trust in the bot; Discord users find it impossible to scan dozens of date‑named threads (#2984).
- The steady stream of “fix” and “follows‑guidelines” PRs from both core team and community indicates a healthy ecosystem, but the opencode bug highlights remaining reliability gaps.
- The PR Factory recipe (#2742) and scheduled tasks signal strong user appetite for automated, self‑hosted agent teams running complex workflows.

## Backlog Watch
- **#2742 – PR Factory recipe** – Open since June 11, no merge activity. This is a requested feature that would showcase NanoClaw’s automation power; it may be stalled awaiting core review.
- **#2770 – Codex file event build‑fix** – Open since June 14, essential for Codex image generation to work.
- **#2798 – CHANGELOG expansion for v2.1.17** – Open since June 17; documentation debt, not blocking but should be merged to keep release notes up‑to‑date.
- **#2873 – Split pre‑flight from credentials for skill updates** – Open since June 27; important to allow running `/update-skills` without re‑entering credentials.

---

*All links point to the nanocoai/nanoclaw repository on GitHub.*

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw Project Digest — 2026-07-09

## 1. Today’s Overview
The IronClaw repository saw a high volume of activity with 18 issues updated (5 closed) and 50 pull requests updated (10 merged/closed) in the last 24 hours. The project remains firmly in the “Reborn” v2 rewrite phase, with engineering effort split between addressing bugs from the ongoing bug bash, landing architectural refactors (notably the NEA-25 extension cutover), and shipping incremental WebUI v2 improvements. The daily failure taxonomy again recorded upstream provider rate-limiting, but overall CI stability appears acceptable. No new releases were cut today.

## 2. Releases
*No new releases recorded in the last 24 hours.*

## 3. Project Progress (Merged/Closed PRs & Completed Fixes)
Ten pull requests were merged or closed today. The following resolved issues demonstrate concrete feature progress and bug closures:

- **i18n coverage for Reborn Projects page** – Issue [#5768](https://github.com/nearai/ironclaw/issues/5768) was closed; previously hardcoded English labels in project summary cards are now localized.
- **Automation rename capability** – Issue [#5419](https://github.com/nearai/ironclaw/issues/5419) was closed; users can now edit automation names, addressing truncation ambiguities.
- **Incorrect UI timestamps** – Issue [#3535](https://github.com/nearai/ironclaw/issues/3535) (a P1 bug) was closed; conversation timestamps now reflect the actual message time.
- **Tool permission dropdown refreshed** – Issue [#5770](https://github.com/nearai/ironclaw/issues/5770) was closed; the native `<select>` was replaced with a custom dropdown consistent with WebUI v2 design.
- **Nightly E2E failure recovery** – Issue [#4108](https://github.com/nearai/ironclaw/issues/4108) was closed after the failing `web-regressions` job was stabilised.

These closures indicate healthy velocity on both user-facing polish and CI reliability.

## 4. Community Hot Topics
*Note: Comment counts on PRs were not available in today’s dataset; issue picks are based on the highest recorded interaction (3 comments).*

- **Approval notifications disappear** – [#5553](https://github.com/nearai/ironclaw/issues/5553) (P2 bug) has drawn attention because it breaks the interactive approval flow required for automations (e.g., web access). Users report notifications flashing once or failing to appear, preventing task continuation. A fix PR ([#5681](https://github.com/nearai/ironclaw/pull/5681)) is open and aims to include the active thread in notification polling.
- **Traces Commons enrollment CLI + hosted-user login links** – [#5858](https://github.com/nearai/ironclaw/pull/5858) is an XL PR that closes gaps in instance-wide Trace Commons enrollment, a foundational observability feature. Its complexity and cross-cutting scope make it a likely discussion point among core contributors.

**Underlying needs**: Users require reliable, visible approval signals for security-sensitive automations, and administrators need a smooth pathway to opt-in to shared tracing infrastructure.

## 5. Bugs & Stability
A series of bug-bash issues were filed recently, several marked P2 (high priority). The most impactful active bugs:

- **[P2] Approval notifications disappear** – [#5553](https://github.com/nearai/ironclaw/issues/5553) – Fix candidate in PR [#5681](https://github.com/nearai/ironclaw/pull/5681) (open).
- **[P2] Routine run actions not clickable** – [#5837](https://github.com/nearai/ironclaw/issues/5837) – “Open run” and “Logs” buttons are non-functional, blocking failure diagnosis. No fix PR yet.
- **[P2] Run fails with context compaction error despite successful tool execution** – [#5838](https://github.com/nearai/ironclaw/issues/5838) – Runs abort after multiple tool calls, producing `context compaction could not complete`. No fix PR yet.
- **[P2] Routine fails every scheduled run with “No thread attached”** – [#5836](https://github.com/nearai/ironclaw/issues/5836) – Systemic failure in `ironclaw-issues-slack-summary` routine. No fix PR yet.
- **[P2] Slack disconnect request rejected by agent** – [#5834](https://github.com/nearai/ironclaw/issues/5834) – Agent responds with unrelated content, leaving Slack permanently connected. A unification PR ([#5851](https://github.com/nearai/ironclaw/pull/5851)) may address the underlying cleanup logic.
- **[P3] “Jump to latest” button appears unnecessarily and overlaps content** – [#5835](https://github.com/nearai/ironclaw/issues/5835) – UX annoyance, no fix PR.

Additionally, the daily failure taxonomy ([#5859](https://github.com/nearai/ironclaw/issues/5859)) notes upstream provider rate-limiting saturated the pinchbench run, but this is an external dependency issue.

## 6. Feature Requests & Roadmap Signals
Several open issues and PRs hint at the near-term feature set:

- **WebChat attachment file-count limit increase** – [#5820](https://github.com/nearai/ironclaw/issues/5820) requests lifting the 10-file cap and surfacing overflow errors. This is a direct user pain-point from real workflows.
- **Private install of tools** – PRs [#5525](https://github.com/nearai/ironclaw/pull/5525) (private tool installation by non-admin users) and [#5780](https://github.com/nearai/ironclaw/pull/5780) (admin-installed private skills) signal a move toward multi-tenant isolation and user-owned extensions.
- **Streaming assistant text in WebUI** – [#5821](https://github.com/nearai/ironclaw/pull/5821) will deliver live model progress over SSE, improving perceived responsiveness.
- **Chat terminal shortcut preference** – [#5824](https://github.com/nearai/ironclaw/pull/5824) adds a user-facing Settings toggle, indicating refinement of power-user features.
- **Workspace filesystem scoping** – [#5831](https://github.com/nearai/ironclaw/pull/5831) replaces fixed filesystem views with caller-scoped resolvers, a prerequisite for secure multi-user workspaces.
- **Extension manifest v2 cutover** – PRs [#5839](https://github.com/nearai/ironclaw/pull/5839), [#5842](https://github.com/nearai/ironclaw/pull/5842), and [#5848](https://github.com/nearai/ironclaw/pull/5848) are executing the NEA-25 architecture change, removing legacy channel rails and enforcing zero-legacy invariant. This work is foundational for future extensibility.

**Prediction for next version**: Likely includes private tool installations, streaming chat responses, and further WebUI v2 polish (attachment limit adjustments, terminal shortcut preference). The NEA-25 refactor may land as a breaking change soon.

## 7. User Feedback Summary
Today’s bug reports reveal clear pain points:
- **Approval workflow is unreliable** – users cannot reliably approve automation steps, breaking critical tasks.
- **Routines are silently failing** – both UI (non-clickable actions) and runtime (“No thread attached”, context compaction) block automation reliability.
- **Slack integration is fragile** – disconnect attempts are ignored, eroding trust in the platform’s ability to manage connections.
- **File upload limits are too restrictive** – real users hit the 10-file cap repeatedly and lose files silently, causing workflow friction.
- **Quality-of-life fixes appreciated** – the closure of automation rename, i18n gaps, and timestamp corrections aligns with user requests and improves daily usability.

## 8. Backlog Watch
Items that lack a clear fix and may need maintainer attention:

- **Admin panel token re-issue missing** – [#5856](https://github.com/nearai/ironclaw/issues/5856) – Follow-up to a recent admin UI update; no PR yet. Critical for secure user management.
- **Approval notification disappearance** – [#5553](https://github.com/nearai/ironclaw/issues/5553) – While a PR is open, the bug has been present for a week and directly impacts automation workflows.
- **Routine “No thread attached” failure** – [#5836](https://github.com/nearai/ironclaw/issues/5836) – This appears to be a systemic regression affecting core scheduling; it needs immediate triage.
- **Legacy v1 test cleanup series** – [#5826](https://github.com/nearai/ironclaw/issues/5826), [#5827](https://github.com/nearai/ironclaw/issues/5827), [#5828](https://github.com/nearai/ironclaw/issues/5828) – While not user-facing, these housekeeping tasks remove noise from CI; they are open and assigned but not yet integrated.

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# LobsterAI Project Digest — July 9, 2026

## 1. Today’s Overview
The project closed 12 pull requests today, delivering a large batch of fixes and enhancements across cowork collaboration, agent isolation, IM scoping, memory search, and UI improvements. While no new release was published, the rapid merge cadence shows strong maintainer engagement. On the issue side, three items were touched — one critical stale report was closed, but a new regression about agent configuration override sparked immediate attention and was promptly resolved by a merged fix. Overall, the project is in an active maintenance phase with a focus on stability and multi-agent correctness.

## 2. Releases
*No new releases were published in this period.*

## 3. Project Progress (Merged/Closed PRs today)
The following 12 pull requests were merged or closed, directly improving the product:

- **Agent isolation & configuration**
  - [#2295](https://github.com/netease-youdao/LobsterAI/pull/2295) `fix(agent): scope USER.md bootstrap file per agent workspace` – Solved a critical regression where editing “about you” in any agent silently overwrote all other agents’ USER.md files. (Addresses [#2293](https://github.com/netease-youdao/LobsterAI/issues/2293).)
  - [#2298](https://github.com/netease-youdao/LobsterAI/pull/2298) `fix(im): scope channel session mappings by agent` – IM conversation mappings now respect agent boundaries, preventing cross-agent session mix-ups.
- **Cowork & collaboration**
  - [#2300](https://github.com/netease-youdao/LobsterAI/pull/2300) `fix(cowork): support attachments in steer queue` – Enables file attachments, dragged files, pasted content and images in follow-up messages queued during active cowork turns.
  - [#2299](https://github.com/netease-youdao/LobsterAI/pull/2299) `fix(cowork): sync subagent child tool history` – Child session pages now show tool call/results from materialized subagent workflows.
  - [#2296](https://github.com/netease-youdao/LobsterAI/pull/2296) `feat(cowork): add minimizable permission prompts` – Permission prompts in cowork sessions can now be minimized, with a pending-confirmation bar above the input.
  - [#1402](https://github.com/netease-youdao/LobsterAI/pull/1402) `fix(cowork): keep all files from multi-select attachment picker (#1384)` – Fixed a bug where selecting multiple files in a single dialog only retained the last one.
- **Memory & OpenClaw integration**
  - [#2301](https://github.com/netease-youdao/LobsterAI/pull/2301) `fix(openclaw): explicitly disable memory dreaming` – Ensures generated OpenClaw config cleanly disables dreaming when LobsterAI’s memory dreaming is off, removing stale cron jobs.
  - [#2297](https://github.com/netease-youdao/LobsterAI/pull/2297) `fix(openclaw): default memory search to local FTS` – Falls back to local trigram FTS keyword search when embedding search is disabled, including a migration for existing indexes.
- **Scheduled tasks & UX**
  - [#1404](https://github.com/netease-youdao/LobsterAI/pull/1404) `feat(scheduledTasks): time control optimization` – Replaced the native time input with a custom, theme-aligned time picker for better usability on Electron.
  - [#1406](https://github.com/netease-youdao/LobsterAI/pull/1406) `fix(scheduled-task): fallback notify channel list when IM filter is empty (#1329)` – Repaired the notification channel dropdown showing no entries when no IM platform was toggled on.
- **Security & i18n**
  - [#1401](https://github.com/netease-youdao/LobsterAI/pull/1401) `fix: 修复请求安全性问题` – Upgraded SSE request ID generation from `Math.random()` to `crypto.randomUUID()` to prevent prediction and potential data stream hijacking.
  - [#1403](https://github.com/netease-youdao/LobsterAI/pull/1403) `fix(i18n): add delete translation key (#1361)` – Added the missing ‘delete’ translation entry, fixing the English fallback in Chinese UI.

## 4. Community Hot Topics
- **[#1400](https://github.com/netease-youdao/LobsterAI/issues/1400) (7 comments) – Gateway restart loop & custom LLM failure after v4.1 upgrade**  
  This closed but heavily discussed issue describes a situation where the gateway repeatedly fails to start after an upgrade, and a custom `qwen3.5-plus` LLM becomes unusable due to a web-extractor dependency conflict. The user provided contact details seeking direct help. The issue was closed (likely by the stale bot) without a visible fix PR, leaving uncertainty about whether the underlying problems were ever resolved.

- **[#2293](https://github.com/netease-youdao/LobsterAI/issues/2293) (1 comment) – USER.md overwritten across all agents**  
  A newly opened issue where changing the USER.md or “About you” for one agent immediately overwrites the same file for all other agents, breaking multi-agent workflows. The reporter confirmed the regression persists even after directly editing workspace files. This was already addressed today by PR #2295, which scopes USER.md per agent.

**Underlying needs**: Users are building complex multi-agent setups and demand strict configuration isolation. The severity of these reports indicates that unintentional cross-agent data sharing is a core pain point requiring rigorous testing.

## 5. Bugs & Stability
- **Critical – Cross-agent USER.md override ([#2293](https://github.com/netease-youdao/LobsterAI/issues/2293))**  
  A regression where any agent’s “about you” settings silently replaces USER.md in all workspaces. **Fix merged**: [#2295](https://github.com/netease-youdao/LobsterAI/pull/2295) scopes reading/writing to the correct agent workspace immediately after report.

- **Severe – Gateway startup loop after version 4.1 upgrade ([#1400](https://github.com/netease-youdao/LobsterAI/issues/1400))**  
  The gateway repeatedly restarts, completely breaking the service. Also, a custom LLM (`qwen3.5-plus`) cannot be invoked due to a web-extractor conflict, possibly related to automatic configuration conflicts. The issue is **closed** but does not appear to have a linked resolution PR, making it a potential lingering risk for upgraders.

- **Minor – No validation for duplicate scheduled task names ([#1348](https://github.com/netease-youdao/LobsterAI/issues/1348))**  
  Users can create multiple scheduled tasks with identical names, which can lead to confusion. Still open, no immediate fix.

## 6. Feature Requests & Roadmap Signals
- **Skills management ([#1346](https://github.com/netease-youdao/LobsterAI/pull/1346))** – A long-open, stale PR aiming to introduce a skills management system. The feature is explicitly requested by the community and remains unmerged.
- **Enhanced Cron scheduling for tasks ([#1347](https://github.com/netease-youdao/LobsterAI/pull/1347))** – Another stale PR providing custom cron expressions, agent/model binding, and a visual cron builder. The recent UI improvements ([#1404](https://github.com/netease-youdao/LobsterAI/pull/1404)) suggest interest in refining the scheduled task experience, so a more complete cron solution may be revisited in a future release.
- **Attachments in steered cowork messages ([#2300](https://github.com/netease-youdao/LobsterAI/pull/2300)) and minimizable permission prompts ([#2296](https://github.com/netease-youdao/LobsterAI/pull/2296))**, now merged, are likely to appear in the next version, further smoothing the cowork workflow.

## 7. User Feedback Summary
- **Pain points**  
  - Upgrade stability remains a major concern: user `danielmonlite` describes a completely broken instance post‑upgrade ([#1400](https://github.com/netease-youdao/LobsterAI/issues/1400)).  
  - Multi-agent configuration isolation is fragile; `yepcn` discovered that any agent’s personalization silently overwrites others, effectively preventing distinct agent personas ([#2293](https://github.com/netease-youdao/LobsterAI/issues/2293)).  
  - Users manually inspect and modify workspace files, indicating that UI‑only configuration feels untrustworthy when bugs occur.

- **Positive signals**  
  - The rapid turnaround for the USER.md bug (issue opened July 7, fix merged July 8) demonstrates responsiveness that the community values.  
  - The volume of merged cowork and IM fixes indicates direct attention to collaboration features, which are central to the tool’s use cases.

## 8. Backlog Watch
- **[#1346](https://github.com/netease-youdao/LobsterAI/pull/1346) — Feat/skills management**  
  Stale since April 2026 with no comments or maintainer activity. A highly desired feature languishing without review.
- **[#1347](https://github.com/netease-youdao/LobsterAI/pull/1347) — Cron custom scheduling and agent selector**  
  Also stale since April; introduces deep improvements to scheduled tasks. Needs a maintainer to push it forward or re-engage the author.
- **[#1400](https://github.com/netease-youdao/LobsterAI/issues/1400) — Gateway restart loop** (closed but possibly unresolved)  
  Despite being closed, the severe symptoms and user outreach warrant a verification that the root cause is addressed in the current codebase; otherwise, it could re‑emerge for other upgraders.
- **[#1348](https://github.com/netease-youdao/LobsterAI/issues/1348) — Duplicate scheduled task names**  
  Open and stale, no fix proposed. Low severity but a paper‑cut that harms usability in a frequently used feature.

</details>

<details>
<summary><strong>TinyClaw</strong> — <a href="https://github.com/TinyAGI/tinyagi">TinyAGI/tinyagi</a></summary>

# TinyClaw Project Digest — 2026-07-09

## 1. Today’s Overview
The project showed a low level of activity today, with no new issues opened or updated and no new releases. The sole change was a pull request (#44) that was merged yesterday after nearly five months of review; it delivers a comprehensive security hardening across chat, file handling, and update mechanisms. The repository’s issue tracker remains empty, indicating either a quiet development cycle or a pattern of direct contributions without formal issue tracking. Overall, the project appears stable but slow-moving in terms of community contributions.

## 2. Releases
*No new releases to report.*

## 3. Project Progress
- **Merged PR #44: Harden channel auth, file safety, and update integrity**  
  *Author: coreyone* | *Closed: 2026-07-08* | [PR #44](https://github.com/TinyAGI/tinyagi/pull/44)  
  This merge delivers the full set of security improvements from the code‑review audit. Key changes include enforcement of sender allowlists (default‑on) across Telegram, Discord, WhatsApp, and queue processing; restrictions on outbound file handling to prevent path traversal and dangerous file types; and integrity checks for bundle updates/installation to guard against tampering. No breaking changes were introduced; these are hardening measures that tighten existing functionalities.

## 4. Community Hot Topics
No issues or pull requests had any comments or reactions during the last 24 hours. The closed PR #44 received no thumbs‑up or discussion during its lifetime, suggesting that security audits are handled by the core team with little community interaction. The community remains largely passive for now.

## 5. Bugs & Stability
No bugs, crashes, or regressions were reported today. The previously merged PR #44 fixes several potential vulnerabilities (unauthorised sender access, insecure file writes, compromised updates) that could have led to instability or abuse in production environments. The fix is already merged, so there are no outstanding security-related bug reports.

## 6. Feature Requests & Roadmap Signals
No explicit feature requests were filed or discussed today. The content of PR #44 implies that the maintainer(s) are prioritising security hardening as a core part of the agent framework’s maturation. It is plausible that the next release (if any) will bundle these security enhancements as a minor/patch version, possibly accompanied by documentation on the new allowlist configurations and file‑handling policies.

## 7. User Feedback Summary
There is no direct user feedback from issues or comments today. The lack of public issues may indicate that users are not encountering visible problems, or that feedback is channelled outside of GitHub. The fact that a major security pull request was merged without fanfare suggests a developer‑led, stability‑focused project with minimal open‑source community noise.

## 8. Backlog Watch
No outstanding issues or pull requests currently need maintainer attention. The repository’s issue tracker is empty, and all open PRs have been resolved. There is no backlog of long‑unanswered items as of this date.

</details>

<details>
<summary><strong>Moltis</strong> — <a href="https://github.com/moltis-org/moltis">moltis-org/moltis</a></summary>

# Moltis Project Digest — 2026-07-09

## 1. Today’s Overview
Repository activity remains low, with only a single pull request submitted in the last 24 hours and no new issues, merged changes, or releases. The opened PR targets a stability problem in the CalDAV integration that can cause an outright panic when handling non-ASCII datetime strings from remote servers. No other contributions, discussions, or maintenance signals were observed, indicating a quiet day overall with one concrete reliability improvement awaiting review.

## 2. Releases
*No new releases have been published.*

## 3. Project Progress
No pull requests were merged or closed today. The only advancement was the creation of a fix:
- **[fix(caldav): avoid panic on non-ASCII datetime in normalise_datetime](https://github.com/moltis-org/moltis/pull/1145)** – A proposed correction to prevent a crash when the `normalise_datetime` function encounters datetime values that contain non-ASCII digits, such as those served by some remote CalDAV servers. The fix is not yet merged.

## 4. Community Hot Topics
Today’s only item is PR #1145 (open, 0 comments, 0 reactions). It does not currently exhibit active discussion, but its existence highlights a real-world interop pain point. The underlying need is for Moltis to robustly handle malformed or unexpected datetime formats from external services, a requirement common in calendar synchronisation workflows.

## 5. Bugs & Stability
**High severity – CalDAV panic on non-ASCII datetime values**
- **Symptom**: The `normalise_datetime` function inside `crates/caldav/src/ical.rs` panics when processing a DATE-format string that contains non-ASCII digit characters (e.g., fullwidth digits or characters from certain locales). The issue occurs because the code uses a byte‑indexed slice after an ASCII‑only digit guard only for the 8‑character branch, but the later processing does not account for multi‑byte characters in other branches.
- **Impact**: Any synchronisation with a CalDAV server that returns such a value will crash the application, potentially causing data loss or service disruption for users.
- **Resolution status**: A fix PR ([#1145](https://github.com/moltis-org/moltis/pull/1145)) has been submitted but not yet reviewed or merged. The patch adjusts the check and parsing logic to avoid the panic and return a proper error.

*No other bugs or regressions were reported today.*

## 6. Feature Requests & Roadmap Signals
No feature requests were logged in the last 24 hours, and there are no signals suggesting upcoming roadmap changes. The current focus appears entirely on stability and correctness fixes for existing functionality.

## 7. User Feedback Summary
The sole contributor to PR #1145 (likely an end‑user or downstream developer) reports encountering a panic in production due to a specific CalDAV server that emits datetimes with non‑ASCII digits. This reveals:
- **Pain point**: Moltis’ calendar stack is fragile when faced with unexpected server‑side formats; a simple parsing glitch can bring down the entire process.
- **Use case**: Real‑world CalDAV sync where remote servers do not strictly follow standard ASCII digit formats, which may be more common in internationalised deployments.
- **Sentiment**: The issue is framed as a bug fix, indicating dissatisfaction with the current robustness, but the prompt submission of a patch suggests a constructive community that is willing to contribute fixes. No negative sentiment beyond the panic itself.

## 8. Backlog Watch
The only actionable item needing maintainer attention today is the open bug fix PR:
- **[#1145 – fix(caldav): avoid panic on non-ASCII datetime](https://github.com/moltis-org/moltis/pull/1145)** has no comments or reviews yet. Given that it resolves a potential crash, a timely review would be beneficial to improve stability.

No other long‑unanswered issues or pull requests are visible from today’s data snapshot.

</details>

<details>
<summary><strong>CoPaw</strong> — <a href="https://github.com/agentscope-ai/CoPaw">agentscope-ai/CoPaw</a></summary>

# CoPaw Project Digest — 2026-07-09

## 1. Today’s Overview
CoPaw (agentscope-ai/QwenPaw) sustained very high development velocity over the past 24 hours: **34 issues** (14 still open, 20 closed) and **46 pull requests** (32 open, 14 merged/closed) were updated, while **v2.0.0-beta.4** shipped. The activity reflects an aggressive push to stabilise the v2.0 beta line, with the community simultaneously exposing critical bugs in context compaction, channel integrations, and tool execution, and submitting a variety of feature requests. The maintainers appear focused on hardening scroll/context handling, expanding channel support, and landing early v2 features.

## 2. Releases
**v2.0.0-beta.4** was released today.
- Changelog:
  - *chore(version):* bump version to 2.0.0b4 (by @rayrayraykk)
  - *fix(scroll):* protect the active turn, add graduated pressure relief, and make recall failures unmistakable (by @niceIrene)  
  No formal breaking-change notes were included, but this is a fast-moving beta series; the scroll fix directly targets context-compaction failures reported in the wild.
- Tag: https://github.com/agentscope-ai/QwenPaw/releases/tag/v2.0.0-beta.4

## 3. Project Progress (Merged/Closed PRs Today)
The following notable PRs were closed overnight (many likely merged into the beta.4 release or main branch):
- [#5871](https://github.com/agentscope-ai/QwenPaw/pull/5871) – *fix(scroll): anchor the live turn with a seam banner in the eviction index*, ensuring the real current request is clearly separated from archived turns after scroll compaction.
- [#5848](https://github.com/agentscope-ai/QwenPaw/pull/5848) – *fix(scroll): label un-headlined evicted spans in the eviction index*, preventing “(no milestone)” gaps that confused models.
- [#5792](https://github.com/agentscope-ai/QwenPaw/pull/5792) – *fix(agents): stop dropping self-paired tool messages during sanitation* — repairs a bug where valid AgentScope 2.0 tool call/tool result pairs were removed.
- Multiple closed issues (see below) also indicate that fixes for context loss, infinite loops, tool-approval popups, and silent message drops have landed.

## 4. Community Hot Topics (Most Active Discussions)

| Issue / PR | Comments | Status |
|------------|----------|--------|
| [#5757](https://github.com/agentscope-ai/QwenPaw/issues/5757) – **Feishu (Lark) channel stops replying after first message** | 13 | OPEN |
| [#5846](https://github.com/agentscope-ai/QwenPaw/issues/5846) – **v2.0.0b3: tool approval popup appears even when auto-approve is on** | 10 | CLOSED |
| [#5171](https://github.com/agentscope-ai/QwenPaw/issues/5171) – **Context compression drops all memories, task breaks completely** | 9 | CLOSED |
| [#5379](https://github.com/agentscope-ai/QwenPaw/issues/5379) – **Internal Server Error after Python install on Windows** | 8 | OPEN |
| [#5797](https://github.com/agentscope-ai/QwenPaw/issues/5797) – **Feature: user switch for scheduled-task popup notifications** | 6 | OPEN |
| [#5725](https://github.com/agentscope-ai/QwenPaw/issues/5725) – **Console browser freezes during streaming; recovers after response** | 5 | OPEN |
| [#5860](https://github.com/agentscope-ai/QwenPaw/issues/5860) – **v2.0.0-beta.3: frequent conversation progress loss and infinite loops** | 3 | OPEN |

**Underlying needs:**  
- Reliable instant-messaging channel integration (Feishu, Matrix).  
- Deterministic, user-controllable context management that never silently discards information.  
- Smooth, non-intrusive UX: toggleable notifications, no unwanted popups, and responsive streaming.  
- A stable v2.0 experience that does not lose the user’s place or loop endlessly.

## 5. Bugs & Stability (Ranked by Severity)
*Critical*  
- [#5860](https://github.com/agentscope-ai/QwenPaw/issues/5860) – **v2.0.0-beta.3 loses conversation state and enters infinite loops** (OPEN, no direct fix PR yet).  
- [#5757](https://github.com/agentscope-ai/QwenPaw/issues/5757) – **Feishu channel ignores subsequent messages** (OPEN, blocks enterprise users).  
- [#5379](https://github.com/agentscope-ai/QwenPaw/issues/5379) – **Startup crash with Internal Server Error** on Windows (OPEN, blocks new users).  
- [#5856](https://github.com/agentscope-ai/QwenPaw/issues/5856) / [#5858](https://github.com/agentscope-ai/QwenPaw/issues/5858) – **Context compaction destroys tool_call structure & drops assistant messages**, causing 400 errors (OPEN; related scroll PRs #5871, #5848 may alleviate).  

*High*  
- [#5725](https://github.com/agentscope-ai/QwenPaw/issues/5725) – **Console UI lag during streaming** (OPEN).  
- [#5259](https://github.com/agentscope-ai/QwenPaw/issues/5259) – **Windows vector index not persistent; must rebuild on every launch** (OPEN).  
- [#5868](https://github.com/agentscope-ai/QwenPaw/issues/5868) – **Matrix channel token login fails with `M_MISSING_TOKEN`** (fix PR [#5873](https://github.com/agentscope-ai/QwenPaw/pull/5873) open).  
- [#5872](https://github.com/agentscope-ai/QwenPaw/issues/5872) – **Docker `browser_use` fails due to dbus connection error** (OPEN).  

Several already have corresponding fix PRs: [#5873](https://github.com/agentscope-ai/QwenPaw/pull/5873) for Matrix, scroll fixes from beta.4, and [#5866](https://github.com/agentscope-ai/QwenPaw/pull/5866) addresses a related `rm` security bypass. The `preserve_thinking` default reversal in [#5870](https://github.com/agentscope-ai/QwenPaw/pull/5870) may also help with reasoning loops.

## 6. Feature Requests & Roadmap Signals
Based on open issues and PRs, the following capabilities appear to be in the pipeline:
- **Notification controls** – [#5797](https://github.com/agentscope-ai/QwenPaw/issues/5797) (popup toggle) and [#5852](https://github.com/agentscope-ai/QwenPaw/issues/5852) (sound alert for tool approval) signal strong demand for customisable alerts.
- **Vision fallback for text-only models** – PR [#5726](https://github.com/agentscope-ai/QwenPaw/pull/5726) adds an automatic image-description fallback, greatly widening multimodal support.
- **Memory enhancements** – PR [#5692](https://github.com/agentscope-ai/QwenPaw/pull/5692) (reranker for memory search) and the long-standing [#4171](https://github.com/agentscope-ai/QwenPaw/pull/4171) (title-diffing memory-distill plugin) indicate ongoing investment in agent memory.
- **Channel expansion** – New channels for Azure Bot ([#5849](https://github.com/agentscope-ai/QwenPaw/pull/5849)) and Zalo ([#5801](https://github.com/agentscope-ai/QwenPaw/pull/5801)) are in review, as well as a Creator Page for plugin discovery ([#5874](https://github.com/agentscope-ai/QwenPaw/pull/5874)).
- **UX polish** – System tray minimise ([#5312](https://github.com/agentscope-ai/QwenPaw/issues/5312) closed), slash-command autocomplete ([#5869](https://github.com/agentscope-ai/QwenPaw/pull/5869)), and drag‑and‑drop upload fixes show steady UI refinement.

It is plausible that the next beta (or v2.0.0-rc) will bundle the vision fallback, memory reranker, notification toggles, and at least one new channel.

## 7. User Feedback Summary
- **Pain points:** Feishu channel unreliability, context management that deletes critical information, startup errors, and browser performance degradation. Beta testers of v2.0.0-beta.3 are experiencing conversation loss and infinite loops, eroding confidence.
- **Use cases:** Desktop agents that run long tasks unattended, scheduled workflows that need discreet notifications, multi-model setups that require vision or custom tool flows, and enterprise IM integration (Feishu, Matrix, Zalo).
- **Satisfaction/dissatisfaction:** Users appreciate the rapid release cadence and rich feature set, but the stability gaps in the v2.0 beta line are causing frustration. The community is actively filing detailed bug reports and participating in PR reviews, indicating high engagement and a desire for the platform to succeed.

## 8. Backlog Watch (Important Items Needing Maintainer Attention)
- **PR [#4171](https://github.com/agentscope-ai/QwenPaw/pull/4171)** – *Memory-distill tool plugin* (open since 2026-05-10, under review). This long-standing contribution could significantly improve long‑term agent memory, but has not yet landed.
- **PR [#5726](https://github.com/agentscope-ai/QwenPaw/pull/5726)** – *Vision fallback* (open since 2026-07-02, no merge activity). Many users with text‑only models are waiting for this.
- **Issue [#5259](https://github.com/agentscope-ai/QwenPaw/issues/5259)** – *Windows vector index persistence* (open since 2026-06-17, 4 comments, no acknowledged fix). Directly breaks memory search on Windows.
- **Issue [#5379](https://github.com/agentscope-ai/QwenPaw/issues/5379)** – *Internal Server Error on startup* (open since 2026-06-22, 8 comments). Recurrent reports suggest a non‑trivial installation/configuration problem.
- **Issue [#5797](https://github.com/agentscope-ai/QwenPaw/issues/5797)** – *Notification toggle* (open since 2026-07-06, 6 comments). While not critical, it represents a recurring UX debate that would benefit from a definitive design decision.

*All links point to `https://github.com/agentscope-ai/QwenPaw/`.*

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw Project Digest — 2026-07-09

## 1. Today’s Overview
The ZeroClaw repository saw high activity with **50 issues** and **50 pull requests** updated in the last 24 hours, but **no new releases** were published. Open issues still outnumber closed (40 open vs. 10 closed), and most PRs (45 of 50) remain unmerged, indicating a heavy pipeline. Community engagement remains strong around long‑standing bugs (cron tooling, dialog loss) and architectural RFCs (.ignore files, runtime plugins, OpenAI API compatibility). Maintainers are actively shepherding multiple RFC‑tracker issues and landing high‑risk enhancements through large PRs.

## 2. Releases
*(No new releases or pre‑releases today.)*

## 3. Project Progress
- **5 pull requests were merged or closed** today (exact details not listed in the top‑20 preview).  
- **4 issues were closed** in the same period:
  - [#4873](https://github.com/zeroclaw-labs/zeroclaw/issues/4873) – Feishu (Lark) integration no longer calls the agent by default; resolved.  
  - [#6173](https://github.com/zeroclaw-labs/zeroclaw/issues/6173) – `model_switch` tool not persisting across conversation turns; fix merged.  
  - [#8334](https://github.com/zeroclaw-labs/zeroclaw/issues/8334) – `skills install`/`list`/`remove` targeting `data_dir` incorrectly on multi‑agent setups; resolved.  
  - [#8553](https://github.com/zeroclaw-labs/zeroclaw/issues/8553) – Agent unable to use environment variables as HTTP request secrets; fixed.

**Notable open PRs pushing features forward:**
- [#8486](https://github.com/zeroclaw-labs/zeroclaw/pull/8486) – **OpenAI‑compatible chat completions endpoint** (closing #8550). XL‑sized, high‑risk, but critical for standard LLM client integration.  
- [#8173](https://github.com/zeroclaw-labs/zeroclaw/pull/8173) – In‑app upgrade with auto‑restart from the web dashboard, implementing RFC #8170.  
- [#8880](https://github.com/zeroclaw-labs/zeroclaw/pull/8880) – Approval broker with group membership and quorum (SOP HITL series), stacked on #8848.  
- [#8707](https://github.com/zeroclaw-labs/zeroclaw/pull/8707) – Operator‑bind identities without the `/bind` code round‑trip for Telegram channels, fixing alias awareness.

## 4. Community Hot Topics
*Most discussed issues and PRs today, based on comment count.*

1. **[#5862](https://github.com/zeroclaw-labs/zeroclaw/issues/5862) – ZeroClaw does not know it can add cron** (13 comments)  
   Users report the agent denies having cron capability, even though `zeroclaw cron` exists. The underlying need: agent tool awareness needs to be explicitly injected so it can schedule tasks. Marked `needs-author-action` and `stale-candidate`.

2. **[#6034](https://github.com/zeroclaw-labs/zeroclaw/issues/6034) – Single/multi‑turn dialog loses user messages** (7 comments)  
   A critical bug (S1) occurring with `custom` providers where some user messages vanish, blocking entire workflows. Severity is high, with a custom provider returning 400 errors.

3. **[#8424](https://github.com/zeroclaw-labs/zeroclaw/issues/8424) – RFC: .ignore file mechanism for workspace file protection** (7 comments)  
   A long‑requested security feature to shield sensitive files like `.env` or `config.yaml` from the AI agent, beyond the current `forbidden_paths` that only blocks outside paths. Needs author action.

4. **[#8850](https://github.com/zeroclaw-labs/zeroclaw/issues/8850) – Move optional channels & tools from compile‑time feature flags to runtime plugins** (4 comments)  
   The RFC to replace Cargo features with WASM runtime plugins is drawing architectural interest; it would allow a stock binary to gain features without recompilation.

5. **[#8550](https://github.com/zeroclaw-labs/zeroclaw/issues/8550) – Add OpenAI‑compatible chat completions endpoint** (4 comments)  
   Highly desired to let standard clients (Open WebUI, LobeChat, custom SDKs) connect directly. Related PR #8486 is in progress.

## 5. Bugs & Stability
*Ranked by severity (S0 = critical, S1 = blocked workflow, S2 = degraded).*

- **S0 – Data loss / security risk**  
  - [#6672](https://github.com/zeroclaw-labs/zeroclaw/issues/6672) – `reasoning_content` not passed back in agentic tool‑call loops with Xiaomi thinking‑mode models. Fix not yet available.

- **S1 – Workflow blocked**  
  - [#6034](https://github.com/zeroclaw-labs/zeroclaw/issues/6034) – User messages lost with custom providers. Needs reproduction and author input.  
  - [#6002](https://github.com/zeroclaw-labs/zeroclaw/issues/6002) – Telegram assistant not clearly addressed, causing conversation handling issues.  
  - ~~[#8553](https://github.com/zeroclaw-labs/zeroclaw/issues/8553)~~ – Agent can’t use environment variables as secrets (closed with fix).  

- **S2 – Degraded behavior**  
  - [#8762](https://github.com/zeroclaw-labs/zeroclaw/issues/8762) – Anthropic provider fixed timeout cuts off long legitimate turns (~120 s).  
  - [#8724](https://github.com/zeroclaw-labs/zeroclaw/issues/6724) – Channels supervisor crash‑loops when all configured channels have `enabled=false`.  
  - [#8578](https://github.com/zeroclaw-labs/zeroclaw/issues/8578) – ZeroCode fails to terminate the process after connection errors.  
  - [#8875](https://github.com/zeroclaw-labs/zeroclaw/issues/8875) – Config migration warning points to `config migrate` without showing parse error (new today).  
  - [#6558](https://github.com/zeroclaw-labs/zeroclaw/issues/6558) – Providers error on some compatible endpoints (e.g., Qwen via DashScope), possible config issue.

**Open fixing PRs:**  
- [#8866](https://github.com/zeroclaw-labs/zeroclaw/pull/8866) – Share MCP registry across heartbeat ticks to avoid reconnection churn.  
- [#8873](https://github.com/zeroclaw-labs/zeroclaw/pull/8873) – UTF‑8 safe stdin truncation fix for the exit prompt (tracks #7828).  
- [#8725](https://github.com/zeroclaw-labs/zeroclaw/pull/8725) – Refuse to start webhook listener without a configured secret (security hardening).

## 6. Feature Requests & Roadmap Signals
*Significant enhancements that may shape upcoming versions.*

- **OpenAI Chat Completions API** – #8550 + RFC #8603 + PR #8486. High‑risk, high‑demand; likely to land in the next release.  
- **Runtime plugins (WASM)** – #8850 moves channels & tools from compile‑time feature flags to installable WASM plugins, reducing binary size and enabling post‑ship extensibility.  
- **.ignore file workspace protection** – #8424 (RFC) addresses a major security gap; will likely be accepted as it aligns with sandboxing work.  
- **Per‑agent custom environment variables** – #8226 (RFC) introduces `runtime_context` and `runtime_secrets` blocks to resolve multi‑tenancy identity and token issues.  
- **In‑app upgrade with auto‑restart** – PR #8173 implements RFC #8170; nearing completion.  
- **Multi‑session support in web chat** – #7543 request for session sidebar (new/switch/rename/delete) is popular.  
- **Context compression** – #7673 proposes a provider pipeline decorator to automatically compress prompts before they hit the model.  
- **Rust→Wasm frontend** – #8132 suggests replacing React/Vite with Dioxus/Leptos, eliminating Node.js build dependency.  
- **Enhanced file_read tool** – #8602 plans line caps, charset detection, paged PDF, and notebook awareness.

**Prediction:** The next release (likely a minor version bump) will include the OpenAI‑compatible endpoint and the .ignore file mechanism, given their high activity and RFC acceptance.

## 7. User Feedback Summary
*Real‑world pain points and sentiments extracted from issues.*

- **Cron unawareness is confusing:** Users expect the agent to know about `zeroclaw cron` and are frustrated when it says it lacks the tool (#5862).  
- **Message loss ruins reliability:** Several users on custom/Qwen providers experience disappearing user messages (#6034), leading to broken conversations and unsuable workflows.  
- **Telegram binding friction:** Operators struggle with the `/bind` flow and alias misconfiguration; the community created a PR (#8707) to eliminate the round‑trip.  
- **Feishu integration only calls LLM, not agent:** (#4873) Resolved, but shows channel‑specific gaps.  
- **Android Termux installation is unsupported:** Users try to run ZeroClaw on mobile but hit platform‑binary incompatibility (#7911).  
- **Long‑turn timeout with Anthropic:** Power users doing document synthesis hit 120‑s timeouts, perceiving the provider as “broken” (#8762).  
- **Desire for OpenAI API parity:** Many users want to drop Open WebUI or custom integrations, so the chat‑completions endpoint (#8550) is highly anticipated.  
- **Security concerns:** Requests for .ignore (#8424) and secret environment variables (#8226) indicate that sensitive workspace files are a top worry for enterprise users.

## 8. Backlog Watch
*Open issues and PRs needing maintainer attention (long‑standing with stale or needs‑author flags).*

- [#5862](https://github.com/zeroclaw-labs/zeroclaw/issues/5862) – Cron tool awareness, open since April, `stale-candidate`, needs author action.  
- [#6034](https://github.com/zeroclaw-labs/zeroclaw/issues/6034) – User message loss, open since April, needs reproduction input from author.  
- [#6002](https://github.com/zeroclaw-labs/zeroclaw/issues/6002) – Telegram addressing issue, open since April, needs author feedback.  
- [#6672](https://github.com/zeroclaw-labs/zeroclaw/issues/6672) – Reasoning content loss (S0), stale‑candidate, no fix PR yet.  
- [#6558](https://github.com/zeroclaw-labs/zeroclaw/issues/6558) – Provider errors, stale‑candidate, needs author investigation.  
- [#6517](https://github.com/zeroclaw-labs/zeroclaw/issues/6517) – Context overflow causing hallucination, stale‑candidate.  
- [#7911](https://github.com/zeroclaw-labs/zeroclaw/issues/7911) – Android Termux setup support request, no maintainer engagement.  

On the PR side, large unmerged efforts like [#8486](https://github.com/zeroclaw-labs/zeroclaw/pull/8486) (OpenAI endpoint, XL‑sized, high‑risk) and [#8880](https://github.com/zeroclaw-labs/zeroclaw/pull/8880) (SOP broker) are near‑ready but require thorough review. The community would benefit from maintainer prioritisation of these backlog items to unblock recurring pain points.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*