# AI CLI Tools Community Digest 2026-09-06

> Generated: 2026-09-06 04:06 UTC | Tools covered: 9

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

# Cross-Tool Comparison Report — AI Coding CLI Ecosystem
**Date:** 2026-09-06 · **Scope:** Claude Code, OpenAI Codex, Gemini CLI, GitHub Copilot CLI, Kimi Code CLI, OpenCode, Pi (pi-mono), Qwen Code, DeepSeek TUI / CodeWhale

---

## 1. Ecosystem Overview

The AI coding CLI ecosystem has entered a reliability-and-trust phase: feature velocity remains high, but community attention has shifted to silent failures, session/history integrity, Windows desktop instability, and packaging defects. Five of nine tools shipped releases in the digest window (Claude Code v2.1.263, Gemini CLI nightly, Pi v0.85.1, Qwen Code preview + two nightlies, CodeWhale v0.9.12), while OpenAI Codex, GitHub Copilot CLI, Kimi Code CLI, and OpenCode focused on issue triage and stabilization PRs. Multi-agent workflows are mainstreaming, and subagent correctness — ignored definitions, missing tools, false success signals — is now a top complaint across Claude Code, Gemini CLI, Codex, Copilot CLI, and Qwen Code. Meanwhile, MCP integration is maturing from "can we connect" to "do the contracts hold," with argument-corruption, poisoned servers, and startup-configuration bugs appearing across most tools. Release engineering is visibly struggling to keep pace with feature growth: broken npm tarballs (Pi), self-update corruption (Copilot CLI), repeated release-pipeline failures (Qwen Code), and migration incompatibilities (Gemini CLI hooks) all surfaced within a single 24-hour window.

---

## 2. Activity Comparison

Figures reflect items surfaced/updated in each project's 24-hour digest window, not full repository totals.

| Tool | Issues (24h surfaced) | PR Activity (24h) | Release Status |
|---|---|---|---|
| **Claude Code** | 10 hot issues; top thread #91870 (112 comments, 72 👍) | 3 PRs updated | ✅ v2.1.263 shipped (bug fixes) |
| **OpenAI Codex** | 10 hot issues; #35746 (39 comments) remains unresolved | ~10 key PRs merged/closed (native voice, WebRTC, MSVC Bazel, TUI worktrees) | ❌ No release |
| **Gemini CLI** | 50 issues with activity | 23 PRs with activity | ✅ Nightly `v0.60.0-nightly.20260906` |
| **GitHub Copilot CLI** | Major triage wave; 10+ newly filed issues | 0 PRs updated | ❌ No release |
| **Kimi Code CLI** | 4 issues updated (3 closed, 1 new open) | 1 PR updated | ❌ No release |
| **OpenCode** | ~10 hot issues + large batch of June/July bugs closed | ~10 coordinated client/desktop resilience PRs | ❌ No release |
| **Pi (pi-mono)** | 10 hot issues; Windows thread #7547 (52 comments); #9132 (5 👍) | ~10 PRs (packaging fixes, system-message deltas, Meta provider) | ✅ v0.85.1 shipped (GPT-6 Astra) |
| **Qwen Code** | 10 hot issues + 3 "Release Failed" bot reports | ~10 key PRs (MCP startup fix, ACP delegation, workspace scoping) | ✅ 3 builds shipped (v0.23.1-preview.0 + 2 nightlies) |
| **DeepSeek TUI / CodeWhale** | 25 issues updated (10 selected) | 10 key PRs (Windows computer-use, CRLF, release automation) | ✅ v0.9.12 shipped (rebrand release) |

**Read on volume:** Gemini CLI has the highest measured activity (50 issues + 23 PRs in 24h). OpenCode shows the most responsive maintenance pattern — a fresh SQLite bug (#47566) received a fix PR (#47567) the same day. Copilot CLI and Kimi Code CLI represent the two extremes of the PR pipeline: Copilot has issue velocity but zero code movement; Kimi has minimal activity overall.

---

## 3. Shared Feature Directions

**1. Plugin/extension extensibility with safe lifecycle control.** Claude Code's Function Hooks proposal (#91870, Express/Koa-style `next` model) is the largest design conversation in the ecosystem this week. Pi is architecting mid-session prompt/tool changes as system-message deltas (#9116/#9117) to eliminate full-context rewrites. OpenCode users want session-lifecycle hooks and background tasks that can inject into active sessions. Qwen Code is scoping extension catalogs to workspace runtimes (#11086).

**2. Windows reliability and parity — across nearly every tool.** Claude Code has five distinct Windows issues (crash recovery, topmost-window corruption, OAuth refresh failures, updater hangs); Codex has launch failures and model-catalog gaps; Copilot CLI's auto-update breaks the running `copilot.exe`; Pi maintains a 52-comment Windows coordination thread plus TUI blockers; DeepSeek/CodeWhale is fixing Windows false-success computer-use reports and CRLF overwrite bugs; Qwen Code has ACP failures in IntelliJ IDEA on Windows. Cross-platform testing is the ecosystem's clearest shared debt.

**3. Agent/subagent correctness and observability.** Dispatched subagents inheriting the wrong prompt/tools (Claude Code #92426), missing tools referenced in prompt text (Claude Code #92134, Copilot CLI #4729), subagents reporting `MAX_TURNS` as success (Gemini CLI #22323), and agents fabricating user answers (OpenCode #35741) all point to a systemic need: subagent definitions must be enforced, and their trajectories must be auditable (Gemini users want subagent context in `/bug` reports; Codex users want a persistent subagent activity indicator).

**4. Durable session/history integrity.** Codex's rollout-ordinal corruption (#35746) now has a Windows variant (#43142); Copilot CLI sessions are invalidated by desktop upgrades (#4734); Gemini CLI crashes on malformed checkpoint history (#29195); OpenCode's 1.17.x migration hid sessions behind a never-back-filled column. Export fidelity is the companion issue: Qwen Code ships 19.5 MB HTML exports for empty sessions, Pi's `/export` silently drops model-visible context, and Kimi's VS Code renderer drops characters from transcripts.

**5. MCP and tool-contract correctness.** Argument serialization is a recurring failure class: Copilot CLI corrupts `open_canvas` args with trailing `}{}` (#4721), Kimi Code CLI needs recursive decoding of double-encoded JSON (#2513), Gemini CLI JSON-encodes MCP prompt text (#29205), and Claude Code lists HTTP-MCP tools that cannot actually be called (#86875). Qwen Code's persisted MCP config not loading at startup (#7771) finally has a fix PR (#11145).

**6. Billing/quota/usage transparency.** Codex users report reconnect loops burning credits (#43045) and silent weekly-quota drops (#42765); OpenCode's dashboard sums per-model percentages to block accounts despite real dollar spend being under cap (#47547); Pi found 1-hour cache writes billed at the 5-minute rate (#9210). Metering accuracy is becoming a trust boundary for paid agentic usage.

**7. Model-routing controls.** Users across tools want explicit control over which model runs, and tools keep silently overriding that choice: Gemini rewrites pinned `gemini-2.5-flash` to a flash-family default (two competing fix PRs, #29217/#29222); Copilot CLI unexpectedly routes to GPT-5 mini mid-task (#4732); Codex's Windows desktop cannot see eligible GPT-6 Astra in the picker (#42853). Enterprise model enable/disable governance (Copilot #4272) and configurable fallback classifiers (Claude Code #74311) round out the theme.

**8. Context-window economy.** Requests for AST-aware file reads/mapping (Gemini #22745), idle proactive compaction aligned to prompt-cache TTL (Copilot #4724), and mid-session system-message deltas (Pi #9116/#9117) all target the same problem: token bloat in long sessions is a UX and cost issue.

---

## 4. Differentiation Analysis

**Model-ecosystem alignment.** Claude Code, Gemini CLI, Kimi Code CLI, and Qwen Code are anchored to their respective model families (Anthropic, Google, Moonshot, Qwen). Codex, Pi, OpenCode, and CodeWhale are model-agnostic/provider-flexible by design — Codex around OpenAI subscriptions, Pi and OpenCode around broad provider catalogs (Copilot, Bedrock, Ollama, gateways), and CodeWhale toward local models and automation.

**Interaction surface.** Qwen Code pushes the richest web-shell/daemon frontend with transcript-window navigation and workflow-DAG visualization. Codex invests in native voice (WebRTC, audio RTP, helper-backed sessions) and managed TUI worktrees. OpenCode and Pi treat the TUI/desktop/serve multi-surface split as first-class. Copilot CLI is unique in coupling deeply to the GitHub desktop app's session manager — which makes it the most vulnerable to self-update and packaging regressions.

**Architectural priorities.** Claude Code is driving the deepest plugin-safety design conversation (#91870) and enterprise extension governance. Gemini CLI is pursuing OS-level sandboxing to safely unlock shell fluency (#19873) plus AST-aware context tooling. Pi is consolidating around provider-gateway correctness and incremental context delivery. Qwen Code is the most advanced on ACP interoperability — including a design where a subagent turn can delegate to an external agent (Claude Code first) over ACP (#11003). Codex shows the most forward-looking client investment (native voice builds, Bazel tooling for Windows/MSVC). DeepSeek/CodeWhale is differentiating on computer-use automation from a Rust TUI, with honest status reporting as an explicit goal after Windows false-success bugs.

**Community character.** Claude Code attracts large design debates with enterprise stake-holders discussing plugin safety. Gemini CLI operates with the most structured engineering process (P1/P2 labels, epics, `need-retesting` states, competing PRs for the same regression). OpenCode and Pi have the most responsive maintainer loops — same-day fix PRs and fast patch releases. Kimi Code CLI is comparatively quiet, and its digest reads like a maintenance-mode day.

---

## 5. Community Momentum & Maturity

**Most engaged discussions:** Claude Code dominates raw engagement — the Function Hooks proposal (#91870) has 112 comments and the Windows crash issue (#53247) has 66. Copilot CLI's oldest request (cancelable queued messages, #1857) has accumulated 28 👍 since March, showing steady but smaller-scale demand.

**Rapid iteration:** Codex merged the largest feature batch despite no release — native voice builds, WebRTC audio transport, and managed TUI worktrees — indicating an active development cycle between releases. Qwen Code shipped three builds in 24 hours but also logged three "Release Failed" bot issues, illustrating high cadence with fragile delivery. Gemini CLI's 50-issue/23-PR day and nightly releases make it the highest-volume tracker in the ecosystem.

**Responsive stabilization:** OpenCode's coordinated PR series against Desktop/Web client connection failures (stalled streams, wedged request queues, 192 redundant MCP fetches in 7 seconds, CORS preflight doubling) demonstrates mature diagnosis and swift fixing. Pi shipped v0.85.1 with GPT-6 Astra and immediately produced fix PRs for the packaging regression that broke fresh installs — a sign of attention, but also of release-process immaturity.

**Maturity signals to watch:** Claude Code and Codex carry the longest-running unresolved architectural issues (Function Hooks design; rollout-ordinal corruption since July). Gemini CLI's structured labeling and two competing PRs for the same model-rewrite regression suggest a healthy but occasionally redundant review culture. CodeWhale's rebrand from DeepSeek TUI creates short-term migration friction for existing users but signals product commitment. Kimi Code CLI appears lowest-velocity, with zero public discussion on most closed issues.

---

## 6. Trend Signals

1. **Windows desktop stability is the ecosystem's shared bottleneck.** Across seven tools, the loudest pain is the same: crash recovery requires reboot, refresh tokens die after sleep, topmost-window corruption breaks Alt+Tab, TUI input redraws per keystroke. For tool builders, Windows CI and crash-forensics investment is now a competitive differentiator, not a portability afterthought.

2. **Silent corruption of outcomes is the new trust crisis.** False success from subagents that never ran analysis (Gemini #22323), user-facing text folded into hidden reasoning (Copilot #4735), characters dropped at the render layer (Kimi #2635), Bash truncation misreported as quoting errors (Claude Code #85111), and security globs silently not matching top-level files (Claude Code #87079) — all share a signature: the tool reports a plausible result that is wrong. Expect demand for wire-level forensic exports and model-visible-context fidelity to grow.

3. **Agent-to-agent protocols (ACP-style delegation) are the next frontier.** Qwen Code's external-executor subagent design delegates turns to another vendor's CLI over ACP; subagent definitions, tool surfaces, and lifecycle events must become portable and verifiable. Communities are already reporting contract mismatches (Claude Code's missing `SendMessage` tool; Copilot's research agent referencing an unavailable `github/get_me` tool). Interop standards for agent identity, tool registry, and trajectory sharing will determine which ecosystem becomes the integration hub.

4. **Release engineering is the weakest layer.** Pi shipped an unimportable npm package; Copilot CLI's auto-update rewrote its own running binary; Qwen's release workflow failed three times in one day; Gemini's Claude Code → Gemini hooks migration silently converted seconds to milliseconds and camelCased event keys incorrectly. Migration compatibility tests and publish guardrails are now as important as agent logic — and should be treated as product surfaces, not internal chores.

5. **Metering accuracy will gate enterprise adoption.** Quota drops with no sessions run, reconnect loops burning credits, summed-percentage billing math, and wrong cache-rate charges appeared across Codex, OpenCode, and Pi in the same 24-hour window. As agentic usage moves to consumption pricing, dollar-accurate accounting and per-session cost attribution are prerequisites — not billing niceties.

6. **Attention economics are reshaping feature design.** The converging asks — AST-aware file reads to avoid token firehosing, proactive compaction aligned to prompt-cache TTL, system-message deltas instead of full prompt rewrites — show that context-window management is becoming a core UX feature rather than an infrastructure detail. Tools that make token spending visible and controllable will win long-session workflows.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills Community Highlights
*Data snapshot: github.com/anthropics/skills — 2026-09-06*

## 1. Top Skills Ranking

**#1 — skill-creator evaluation fix** ([PR #1298](https://github.com/anthropics/skills/pull/1298)) — *Open*
Fixes `run_eval.py`'s 0% recall bug, which renders the skill-description optimization loop useless across all skill creators. The most-discussed PR in the repo, referencing 10+ independent reproductions (issue [#556](https://github.com/anthropics/skills/issues/556)). It is the centerpiece of a notable bug-fix cluster also covered by [#1099](https://github.com/anthropics/skills/pull/1099) and [#1050](https://github.com/anthropics/skills/pull/1050) — all targeting Windows subprocess/encoding failures. This is the community's clearest consensus: the skill-authoring toolchain itself is currently untrustworthy.

**#2 — document-typography** ([PR #514](https://github.com/anthropics/skills/pull/514)) — *Open*
New skill for typographic quality control in generated documents: orphan-word wrapping, stranded section headers, and numbering misalignment. Positioned as a universal finishing layer for any document Claude produces — a high-value, cross-domain addition.

**#3 — scnet-hpc** ([PR #1615](https://github.com/anthropics/skills/pull/1615)) — *Open*
New skill for operating SCNet HPC clusters via profile-based SSH and Slurm workflows: job generation, cluster discovery, accelerator guidance. It is a representative "domain operations" skill for scientific computing, actively discussed and recently updated.

**#4 — ODT skill** ([PR #486](https://github.com/anthropics/skills/pull/486)) — *Open*
Adds OpenDocument creation, template filling, and ODT→HTML parsing for `.odt`/`.ods` files — effectively extending the document-skills suite into the ISO-standard open-source format. Addresses an obvious gap in the existing docx/pdf coverage.

**#5 — frontend-design revision** ([PR #210](https://github.com/anthropics/skills/pull/210)) — *Open*
Refactors the frontend-design skill for clarity, actionability, and single-conversation executability. Discussion centers on making design guidance concrete enough to steer Claude's behavior without human interpretation — part of the broader "skills as operational instructions, not documentation" movement (see issue [#202](https://github.com/anthropics/skills/issues/202)).

**#6 — skill-quality-analyzer + skill-security-analyzer** ([PR #83](https://github.com/anthropics/skills/pull/83)) — *Open*
Adds two meta-skills: a five-dimension quality analyzer (structure, documentation, resources, etc.) and a security analyzer for community skills. Directly responds to the trust-boundary concerns raised in issue [#492](https://github.com/anthropics/skills/issues/492) — the community is building tooling to police its own ecosystem.

**#7 — docx tracked-changes ID fix** ([PR #541](https://github.com/anthropics/skills/pull/541)) — *Open*
Fixes document corruption when DOCX tracked changes collide with existing bookmark `w:id` values in OOXML's shared ID space. A concrete reliability fix for the most business-critical document skill.

---

## 2. Community Demand Trends

**Skill governance, security, and trust** — the single loudest issue thread is [#492](https://github.com/anthropics/skills/issues/492) (43 comments): community skills distributed under the `anthropic/` namespace enable trust-boundary abuse. Related proposals such as agent-governance ([#412](https://github.com/anthropics/skills/issues/412)) and enterprise access-control patterns for SharePoint ([#1175](https://github.com/anthropics/skills/issues/1175)) show demand for skills that secure agent workflows, not just perform tasks.

**Reliable evaluation tooling** — the run_eval.py 0% trigger-rate bug ([#556](https://github.com/anthropics/skills/issues/556), 12 comments) plus mcp-builder's broken evaluation harness ([#1390](https://github.com/anthropics/skills/issues/1390)) indicate that skill *authors* lack trustworthy tooling to validate whether their skills actually trigger — a meta-problem slowing all other skill development.

**Context-window efficiency** — [#1487](https://github.com/anthropics/skills/issues/1487) reports the `claude-api` skill eagerly injecting ~156k tokens in a single tool call; the compact-memory proposal ([#1329](https://github.com/anthropics/skills/issues/1329)) seeks symbolic notation to compress agent state. The community increasingly treats context discipline as a first-class skill requirement.

**Distribution and sharing infrastructure** — org-wide skill libraries ([#228](https://github.com/anthropics/skills/issues/228), 16 comments), duplicate content from overlapping plugins ([#189](https://github.com/anthropics/skills/issues/189)), skills disappearing due to file-management edge cases ([#62](https://github.com/anthropics/skills/issues/62)), and exposing skills as MCPs ([#16](https://github.com/anthropics/skills/issues/16)) all point to demand for mature packaging, versioning, and enterprise distribution.

**Meta-skill quality standards** — skill-creator itself violates best practices ([#202](https://github.com/anthropics/skills/issues/202)) and reads like developer docs; the ecosystem wants skills that are operational, token-efficient instructions — plus quality-gate pipelines ([#1385](https://github.com/anthropics/skills/issues/1385)) to enforce this.

---

## 3. High-Potential Pending Skills

These open PRs carry active discussion and address concrete needs; likely to land or merge soon:

- **skill-creator eval suite fixes** ([#1298](https://github.com/anthropics/skills/pull/1298)) — the highest-engagement PR; merged fixes here would unblock reliable skill iteration for every contributor. Companion Windows fixes in [#1099](https://github.com/anthropics/skills/pull/1099) and [#1050](https://github.com/anthropics/skills/pull/1050) form a coherent fix-package.
- **document-typography** ([#514](https://github.com/anthropics/skills/pull/514)) — small, broadly applicable quality skill with clear scope; strong candidate for a smooth merge.
- **Hivemind: multi-agent orchestration** ([#1628](https://github.com/anthropics/skills/pull/1628)) — novel zero-cost delegation of mechanical work to headless opencode workers on free models, keeping Claude as the sole planner/reviewer — a cost-optimization direction with clear resonance.
- **buffer-api Agent Skill** ([#1627](https://github.com/anthropics/skills/pull/1627)) — portable GraphQL API skill for scheduling/analyzing social posts from any agent — characteristic of the growing "portable API integration" skill category.
- **testing-patterns** ([#723](https://github.com/anthropics/skills/pull/723)) — comprehensive testing-stack skill (Trophy model, unit, React Testing Library, edge cases); substantial and recently updated.
- **ServiceNow platform skill** ([#568](https://github.com/anthropics/skills/pull/568)) — broad enterprise platform coverage (ITSM, ITOM, SecOps, ITAM, FSM, CSDM), actively maintained over a long open period — a heavyweight enterprise-domain addition.
- **self-audit reasoning quality gate** ([#1367](https://github.com/anthropics/skills/pull/1367)) — mechanical file verification plus a four-dimension reasoning audit, aligned with the ecosystem's quality-gate trajectory (see [#1385](https://github.com/anthropics/skills/issues/1385)).
- **claude-api model retirement update** ([#1607](https://github.com/anthropics/skills/pull/1607)) — small correctness fix marking four retired model IDs; a safe, likely fast-tracked merge.
- **pyxel retro-game skill** ([#525](https://github.com/anthropics/skills/pull/525)) — SDK integration pairing an MCP server with a creative-coding workflow; ongoing activity despite a long open window.

---

## 4. Skills Ecosystem Insight

The community's most concentrated demand is not new domain skills but *meta-infrastructure*: reliable skill-evaluation tooling, context-window discipline, and trust/safety governance — the ecosystem needs to trust and verify its skills before it can scale its catalog.

---

# Claude Code Community Digest — 2026-09-06

## Today's Highlights
Claude Code shipped v2.1.263 with bug fixes and reliability improvements. Community attention is concentrated on a major Function Hooks proposal for plugins, a recurring cluster of Windows desktop reliability failures, and several open agent/subagent correctness reports. Only three pull requests were updated in the last 24 hours, so the PR section is intentionally short.

## Releases
**v2.1.263** — Changelog is minimal: bug fixes and reliability improvements. No new user-facing features were listed.

## Hot Issues
1. **Function Hooks for plugins** — [#91870](https://github.com/anthropics/claude-code/issues/91870)  
   The biggest conversation this week: 112 comments and 72 👍. The proposal would add side-effect-tracked, composable hooks with an Express/Koa-style `next` model, potentially making plugins dramatically more powerful while remaining safe.

2. **Claude Desktop fails to launch on Windows after crash** — [#53247](https://github.com/anthropics/claude-code/issues/53247)  
   66 comments and 29 👍. An orphaned Silo / Job Object can block relaunch until logoff or reboot. This has become one of the most visible Windows desktop reliability issues.

3. **Windows desktop “freeze-then-vanish” crash with no forensic trace** — [#89679](https://github.com/anthropics/claude-code/issues/89679)  
   A recurring crash with a reproduction, but no trace in standard logging channels. The “has repro” tag is valuable, but the lack of forensic data will make this hard to root-cause.

4. **Bash tool silently truncates commands over ~8 KB** — [#85111](https://github.com/anthropics/claude-code/issues/85111)  
   Important for daily Claude Code use: long commands can be truncated and reported as a quoting error, hiding the real cause. This is the kind of silent failure that can mislead models and users for a long time.

5. **HTTP-transport MCP tools listed but unreachable** — [#86875](https://github.com/anthropics/claude-code/issues/86875)  
   `/mcp` shows the server connected and lists its tools, but direct calls fail with “No such tool available,” even after restart. Since HTTP MCP servers are common for remote integrations, this is a significant ecosystem reliability issue.

6. **Windows/VS Code OAuth refresh token rejected after sleep/wake** — [#90688](https://github.com/anthropics/claude-code/issues/90688)  
   Since 2.1.247, the refresh token is raced or lost during extension startup, forcing users to `/login` daily after every wake. Severe productivity impact for Windows VS Code users.

7. **Windows desktop window intermittently gains `WS_EX_TOPMOST`** — [#92337](https://github.com/anthropics/claude-code/issues/92337)  
   A fresh open report correlates the always-on-top state with `LocalSessions.setFocusedSession`, breaking normal Alt+Tab behavior. This may explain earlier always-on-top reports that were previously closed as invalid.

8. **Agent tool ignores the subagent definition** — [#92426](https://github.com/anthropics/claude-code/issues/92426)  
   New report: dispatched subagents inherit the dispatcher’s system prompt and tool surface instead of the subagent definition. If confirmed, this is a core correctness bug for multi-agent workflows.

9. **ListAgents references a missing `SendMessage` tool** — [#92134](https://github.com/anthropics/claude-code/issues/92134)  
   Tool descriptions instruct the model to call `SendMessage`, but the tool is not present in the build. This can trap running subagents in un-correctable loops, especially on Windows.

10. **CLI and desktop resolve different plugin identities** — [#92427](https://github.com/anthropics/claude-code/issues/92427)  
    A macOS plugin bug: the same plugin is resolved differently by CLI and desktop, leading to inconsistent hooks and tools depending on which host launches Claude. Important for plugin authors targeting both surfaces.

## Key PR Progress
Only 3 PRs were updated in the reporting window, so all available PRs are listed.

1. **Fix invalid YAML frontmatter in all `pr-review-toolkit` agents** — [#87077](https://github.com/anthropics/claude-code/pull/87077)  
   Agent descriptions contained unquoted dialogue lines that YAML parsed as nested mappings, resulting in empty frontmatter and missing names/descriptions/models. This would make review agents load incorrectly.

2. **Make `**` glob patterns match zero-depth paths** — [#87079](https://github.com/anthropics/claude-code/pull/87079)  
   Security guidance globs delegated to `fnmatch`, where `**/*.ts` requires a literal `/` and therefore misses top-level TypeScript files. Security rules were silently not applying to zero-depth paths.

3. **Fix `validate-agent.sh` aborting on first warning** — [#89404](https://github.com/anthropics/claude-code/pull/89404)  
   Fixes public issue #83803. Under `set -e`, arithmetic expressions such as `((warning_count++))` can abort the script when they evaluate to zero. The script also stopped false-flagging valid agents.

## Feature Request Trends
- **Deeper plugin extensibility** is the dominant feature theme, led by the Function Hooks proposal — [#91870](https://github.com/anthropics/claude-code/issues/91870).
- **Cross-machine configuration sync** remains requested: synchronize `~/.claude` skills, plugins, MCP config, and statusline through an Anthropic account — [#66303](https://github.com/anthropics/claude-code/issues/66303).
- **More model-routing controls** are emerging: users want configurable fallback models/effort for the Fable classifier — [#74311](https://github.com/anthropics/claude-code/issues/74311) — and automatic bug fixing instead of handing bugs to another model — [#92428](https://github.com/anthropics/claude-code/issues/92428).
- **UI notification controls**: users want the ability to dismiss or mark-as-read Recents indicators in Cowork on claude.ai — [#92430](https://github.com/anthropics/claude-code/issues/92430).

## Developer Pain Points
- **Windows reliability is the loudest pain point**: crash recovery requires reboot — [#53247](https://github.com/anthropics/claude-code/issues/53247); trace-free crashes make debugging nearly impossible — [#89679](https://github.com/anthropics/claude-code/issues/89679); topmost-window corruption breaks Alt+Tab — [#92337](https://github.com/anthropics/claude-code/issues/92337); the updater can hang on sideloaded MSIX installs — [#92432](https://github.com/anthropics/claude-code/issues/92432).
- **Authentication is a recurring blocker**: Windows/VS Code refresh tokens fail after sleep/wake — [#90688](https://github.com/anthropics/claude-code/issues/90688); some Pro/Max users are blocked by “organization has disabled subscription access” errors — [#75944](https://github.com/anthropics/claude-code/issues/75944).
- **Silent tool-contract mismatches are common**: Bash truncation is misreported as quoting errors — [#85111](https://github.com/anthropics/claude-code/issues/85111); MCP tools appear connected but cannot be called — [#86875](https://github.com/anthropics/claude-code/issues/86875); agents are told to use tools that do not exist — [#92134](https://github.com/anthropics/claude-code/issues/92134).
- **Subagent behavior is a growing concern**: subagents ignore their definitions and inherit the dispatcher’s prompt/tools — [#92426](https://github.com/anthropics/claude-code/issues/92426) — while CLI and desktop disagree about which plugins are installed — [#92427](https://github.com/anthropics/claude-code/issues/92427).
- **Model guidance is still Windows-unaware in places**: auto-mode system messages instruct use of Bash on Windows — [#92407](https://github.com/anthropics/claude-code/issues/92407) — and classifier refusals can still append “use `head` instead of `cat`” tool-substitution advice after a deliberate security block — [#92411](https://github.com/anthropics/claude-code/issues/92411).

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-09-06

## Today's Highlights

No release was published in the last 24 hours, but the PR queue was active: a large batch of closed PRs landed around native voice builds, Windows/MSVC Bazel tooling, WebRTC audio transport, and TUI worktree management. On the issue tracker, attention remains concentrated on rollout/history corruption in long-running sessions and Windows desktop reliability, especially around model catalog and runtime staging issues. The most-commented issue, [#35746](https://github.com/openai/codex/issues/35746), is still unresolved and has a new Windows-specific variant in [#43142](https://github.com/openai/codex/issues/43142).

## Releases

No new releases were published in the last 24 hours.

## Hot Issues

- [#35746 — Paginated history drops valid flattened rollout records and reuses ordinals](https://github.com/openai/codex/issues/35746)  
  Open since Jul 28 with 39 comments. Rollout history projections lose valid records and reuse ordinals, causing stale or incomplete conversation history in long-lived sessions. This is the highest-signal session-integrity issue right now.

- [#16759 — Full Access still shows permission prompts; users ask for a YOLO mode](https://github.com/openai/codex/issues/16759)  
  Closed after 12 comments. The Full Access setting still triggers permission prompts, which reduces its value for trusted/business workflows. The thread reflects a recurring desire for an explicit no-prompt mode.

- [#42853 — GPT-6 Astra missing from model picker for eligible ChatGPT Pro account](https://github.com/openai/codex/issues/42853)  
  Pro users on the Windows desktop app cannot select GPT-6 Astra despite eligibility. 10 comments, with a fast-follow duplicate in [#43143](https://github.com/openai/codex/issues/43143), suggesting broader catalog-sync breakage.

- [#42501 — Windows app fails to launch UI when cua_node cannot copy node_repl.exe](https://github.com/openai/codex/issues/42501)  
  After updating to a newer Store package, Codex spawns `ChatGPT.exe` processes but never shows a window. The staging failure in `cua_node` makes the app unusable on affected Windows machines.

- [#41922 — Chats become unusable after context compaction and print internal validation errors](https://github.com/openai/codex/issues/41922)  
  Windows desktop users report that context compaction corrupts chat state and breaks future turns. Long-session reliability remains a major concern.

- [#16579 — Windows: allow configuring the default session shell via config](https://github.com/openai/codex/issues/16579)  
  7 comments and 45 👍. Users who prefer Git Bash or another supported shell are forced to use PowerShell by default. This is one of the clearest configuration feature requests for Windows.

- [#41849 — VS Code Remote-SSH reconnect leaves stale app-server holding thread writer](https://github.com/openai/codex/issues/41849)  
  Remote reconnects can start a second app-server while the old one holds the thread writer, causing new sessions to be blocked with “This is open in another app.” Important for remote-development workflows.

- [#43045 — GPT-6 Astra WebSocket reconnect loop continuously consumes purchased credits](https://github.com/openai/codex/issues/43045)  
  A reconnect loop runs until HTTPS fallback, burning paid credits without completing work. Low comment count, but immediately harmful for affected Plus users.

- [#43142 — Windows resume reuses two rollout ordinals after trailing token_count records](https://github.com/openai/codex/issues/43142)  
  New report showing resumed desktop sessions remain stuck on older history even though newer messages exist in the durable JSONL. This looks like a Windows-specific manifestation of the ordinal reuse bug in #35746.

- [#42765 — Weekly Codex limit dropped from ~45% to 0% with no sessions run](https://github.com/openai/codex/issues/42765)  
  A Pro desktop user lost their entire weekly quota in a period with no active sessions. Silent quota accounting issues are especially worrying for paid users.

## Key PR Progress

- [#43147 — Gate experimental context by model capability at session startup](https://github.com/openai/codex/pull/43147)  
  Prevents experimental context activation when the current model does not support it, and stops child sessions from incorrectly inheriting parent token-budget activation.

- [#43120 — Add managed worktree creation to TUI session commands](https://github.com/openai/codex/pull/43120)  
  Adds `/worktree` and enables `/new` and `/fork` to create managed worktrees, making isolated session checkouts available from the TUI.

- [#43113 — Save subagent and memory opt-ins through the app server](https://github.com/openai/codex/pull/43113)  
  Routes TUI prompt choices through the app server so subagent and memory preferences persist for new threads without mutating the current thread.

- [#43097 — Add a helper-backed realtime WebRTC session API](https://github.com/openai/codex/pull/43097)  
  Introduces `RealtimeWebrtcSession` with startup, answer negotiation, audio controls, level meters, and error reporting for helper-backed voice sessions.

- [#43090 — Send processed microphone audio over RTP in voice-host](https://github.com/openai/codex/pull/43090)  
  Connects captured/processed microphone audio to the outgoing media transport instead of draining it locally, while preserving mute boundaries and limiting stale audio.

- [#43079 — Add opt-in local audio devices to the voice helper](https://github.com/openai/codex/pull/43079)  
  Adds `openDevices` and `setAudioControls` to the helper protocol. Microphone and speaker devices are opt-in and default to muted/suppressed until after negotiation.

- [#43100 — Add bounded incoming Opus RTP handling to the voice host](https://github.com/openai/codex/pull/43100)  
  Adds 64-packet and 2 MiB limits for outstanding media, with a 64 KiB per-packet cap, preventing unbounded queuing in realtime audio paths.

- [#43126 — Expose native Windows build tools through Bazel targets](https://github.com/openai/codex/pull/43126)  
  Patches Windows support so MSVC runtime and SDK tool binaries survive setup and remain available to Bazel consumers building native components.

- [#43114 — Add Bazel preparation for native voice runtimes](https://github.com/openai/codex/pull/43114)  
  Adds `//third_party/voice:native_runtime` for macOS and GNU Linux, with receipt validation and source-manifest checks before runtime export.

- [#43099 — Add receipt-verified native voice SDK export](https://github.com/openai/codex/pull/43099)  
  Adds an SDK export path that verifies shared libraries against inspection receipts and records target/source metadata, improving reproducibility for native voice dependencies.

## Feature Request Trends

- **Trusted no-prompt workflows**: Users continue to ask for Full Access to genuinely suppress permission prompts, including explicit “YOLO mode” style behavior ([#16759](https://github.com/openai/codex/issues/16759)).

- **Windows configuration and parity**: Requests include a configurable default session shell ([#16579](https://github.com/openai/codex/issues/16579)) and consistent model picker/catalog behavior between Windows desktop and other clients ([#42853](https://github.com/openai/codex/issues/42853), [#43143](https://github.com/openai/codex/issues/43143)).

- **Continuous usage instead of rolling caps**: Users want the weekly quota pool to be consumable continuously, with an optional “weekly-pool mode” that avoids the 5-hour cap interrupting active engineering work ([#43135](https://github.com/openai/codex/issues/43135)).

- **Better visibility into agent activity**: A persistent subagent activity indicator is requested for the main TUI ([#43148](https://github.com/openai/codex/issues/43148)), and top-level tasks created by agents should be visible in search/remote clients ([#32614](https://github.com/openai/codex/issues/32614)).

- **MCP/OAuth interop**: The CLI should include requested scopes during MCP OAuth dynamic client registration, which currently blocks services such as Fastmail ([#20503](https://github.com/openai/codex/issues/20503)).

## Developer Pain Points

- **Session/history corruption after resume or compaction**: Rollout ordinals are reused, pagination drops valid records, and compacted chats become unusable. See [#35746](https://github.com/openai/codex/issues/35746), [#41922](https://github.com/openai/codex/issues/41922), [#43142](https://github.com/openai/codex/issues/43142), and [#43129](https://github.com/openai/codex/issues/43129).

- **Windows app/runtime instability**: Users repeatedly hit launch failures, missing model catalog entries, and stale UI state on Windows desktop. See [#42501](https://github.com/openai/codex/issues/42501), [#42853](https://github.com/openai/codex/issues/42853), and [#41267](https://github.com/openai/codex/issues/41267).

- **Remote workflow state leaks**: Remote-SSH reconnects and remote control sessions leave stale app-servers, locks, or hidden threads behind. See [#41849](https://github.com/openai/codex/issues/41849), [#40167](https://github.com/openai/codex/issues/40167), [#31110](https://github.com/openai/codex/issues/31110), and [#32614](https://github.com/openai/codex/issues/32614).

- **Unexpected quota/credit consumption**: WebSocket reconnect loops and silent weekly-limit drops make usage accounting unpredictable. See [#43045](https://github.com/openai/codex/issues/43045) and [#42765](https://github.com/openai/codex/issues/42765).

- **Computer Use still has rough edges**: Problems include stuck Computer Use UI states ([#43159](https://github.com/openai/codex/issues/43159)), deterministic bridge latency on Windows ([#42790](https://github.com/openai/codex/issues/42790)), and broken drag behavior on macOS ([#43047](https://github.com/openai/codex/issues/43047)).

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

### 1. Today's Highlights

Gemini CLI shipped the nightly `v0.60.0-nightly.20260906.g85aca163f`; 50 issues and 23 PRs saw activity in the last 24 hours. The dominant theme this cycle is **agent reliability and trust**: a P1 bug shows subagents that hit `MAX_TURNS` being reported as `GOAL` success (#22323), the generalist agent hangs indefinitely for some users (#21409), and shell commands can get stuck at "Waiting input" after completing (#25166). On the PR side, the most actionable work is in Claude Code → Gemini CLI hooks migration correctness, MCP prompt handling, and two competing PRs to stop auto-rewriting an explicitly pinned `gemini-2.5-flash` model.

### 2. Releases

- **[v0.60.0-nightly.20260906.g85aca163f](https://github.com/google-gemini/gemini-cli/compare/v0.60.0-nightly.20260905.g85aca163f...v0.60.0-nightly.20260906.g85aca163f)** — Automated nightly release; no manual changelog. Diff vs. prior nightly is available via the compare link above. Version bump PR: [#29223](https://github.com/google-gemini/gemini-cli/pull/29223).

### 3. Hot Issues

- **[#22323 — Subagent recovery after MAX_TURNS is reported as GOAL success](https://github.com/google-gemini/gemini-cli/issues/22323)** — `P1 · agent · bug` — The `codebase_investigator` subagent returns `status: "success"` even when it hit the max-turn limit before doing any analysis. This is dangerous: automation and users will trust a false success signal. Open since March with 13 comments; now in `need-retesting`.

- **[#21409 — Generalist agent hangs](https://github.com/google-gemini/gemini-cli/issues/21409)** — `P1 · agent · bug` — Simple operations like folder creation hang for up to an hour when the model defers to the generalist agent. 8 👍 makes it the most community-validated bug in this batch; workaround is instructing the model never to use subagents.

- **[#25166 — Shell execution stuck with "Waiting input" after command completes](https://github.com/google-gemini/gemini-cli/issues/25166)** — `P1 · core · bug` — Occurs repeatedly even with trivial, non-interactive commands; the shell panel stays active indefinitely. High frustration for day-to-day terminal usage.

- **[#26525 — Add deterministic redaction and reduce Auto Memory logging](https://github.com/google-gemini/gemini-cli/issues/26525)** — `P2 · security · bug` — Auto Memory sends transcript content to an extraction model *before* any redaction prompt runs, and may log existing skills. A real security-review concern for anyone using memory features with sensitive code.

- **[#21968 — Gemini does not use skills and sub-agents enough](https://github.com/google-gemini/gemini-cli/issues/21968)** — `P2 · agent · bug` — Anecdotal but widely resonant: the model ignores relevant custom `gradle`/`git` skills unless explicitly told to use them, undermining the Skills feature's value.

- **[#19873 — Leverage model's bash affinity via Zero-Dependency OS Sandboxing](https://github.com/google-gemini/gemini-cli/issues/19873)** — `P2 · agent · enhancement` — Ambitious proposal: let Gemini 3 use native POSIX tool chains while sandboxing at the OS level and routing post-execution intent. 9 comments show designs still in flux.

- **[#22745 — Assess the impact of AST-aware file reads, search, and mapping](https://github.com/google-gemini/gemini-cli/issues/22745)** — `P2 · agent · epic` — Tracks investigations into AST-aware tooling to reduce token noise (e.g., precisely reading method bounds in one call). Points to a meaningful direction for context-window economy.

- **[#21983 — Browser subagent fails in Wayland](https://github.com/google-gemini/gemini-cli/issues/21983)** — `P1 · agent/browser · bug` — Browser subagent fails under Wayland; important given the increasing Linux/Wayland developer share.

- **[#22267 — Browser Agent ignores settings.json overrides (e.g., maxTurns)](https://github.com/google-gemini/gemini-cli/issues/22267)** — `P2 · agent · bug` — `AgentRegistry` merges settings but the Browser Agent doesn't apply them. Configuration silently no-ops — a reproducibility trap.

- **[#24246 — Gemini CLI encounters 400 error with >128 tools](https://github.com/google-gemini/gemini-cli/issues/24246)** — `P2 · agent · bug` — With many tools enabled, requests exceed model tool limits and fail hard; users expect the agent to scope tools rather than crash.

### 4. Key PR Progress

- **[#29205 — fix(cli): submit MCP prompt text without JSON encoding](https://github.com/google-gemini/gemini-cli/pull/29205)** — `P2 · core/agent · S` — `McpPromptLoader` was JSON-encoding MCP prompt responses, corrupting embedded quotes/newlines. Replaces the encoded-output expectation with a focused regression test.

- **[#29125 — fix(cli): convert hook timeout from seconds to milliseconds](https://github.com/google-gemini/gemini-cli/pull/29125)** — `P2 · core · S` — Fixes [#29122](https://github.com/google-gemini/gemini-cli/issues/29122). Claude Code hook timeouts are seconds; Gemini's runner expects ms, so migrated `"timeout": 30` became 30 ms. Any Claude migration is affected.

- **[#29124 — fix(cli): correct SubagentStop event key in hooks migration](https://github.com/google-gemini/gemini-cli/pull/29124)** — Fixes [#29123](https://github.com/google-gemini/gemini-cli/issues/29123). Claude Code uses `SubagentStop` (lowercase "a"), but the migration mapped `SubAgentStop`, silently dropping hooks. Same class of bug as #29125: migrations need exact compatibility testing.

- **[#29163 — fix(cli): prevent crash during authentication in git repositories](https://github.com/google-gemini/gemini-cli/pull/29163)** — `P1 · security · L` — The `useGitBranchName` hook crashes startup when `.git` is inaccessible (macOS Seatbelt/restricted permissions). Important for sandboxed/CI environments.

- **[#29195 — fix(checkpoint): degrade non-array history instead of crashing resume](https://github.com/google-gemini/gemini-cli/pull/29195)** — `P2 · core · S` — A checkpoint with valid JSON but non-array `history` crashed `/resume`; now it degrades to an empty checkpoint like other corrupt files. Defensive fix for long-running sessions.

- **[#29217 — fix(config): don't rewrite explicit gemini-2.5-flash model selection](https://github.com/google-gemini/gemini-cli/pull/29217)** — `P1/P2 · core/agent · M` — Broad `model.endsWith('flash')` matching silently rewrote an explicit `--model gemini-2.5-flash` to `gemini-3.5-flash` on GA backends.

- **[#29222 — fix(config): prevent rewriting explicitly pinned flash models](https://github.com/google-gemini/gemini-cli/pull/29222)** — `P1/P2 · core/agent · S` — Same root cause and fix as #29217, by a different author, submitted a day later. Maintainers should deduplicate; community signal that this regression affects real users.

- **[#29211 — fix(cli): stop scheduling state updates from inside a state updater](https://github.com/google-gemini/gemini-cli/pull/29211)** — `P2 · agent · M` — Nested `setState` calls inside `useInputHistoryStore.addInput()` violate React updater purity and can cause flaky input-history behavior. Good catch on a subtle concurrency bug.

- **[#29126 — fix(a2a-server): mount express.json before a2a sdk routes](https://github.com/google-gemini/gemini-cli/pull/29126)** — Fixes [#29073](https://github.com/google-gemini/gemini-cli/issues/29073). A2A SDK routes received `req.body === undefined`, breaking JSON-RPC; small mount-order fix with broad impact for A2A server users.

- **[#28967 — fix(cli): prevent clearing terminal scrollback on static refresh](https://github.com/google-gemini/gemini-cli/pull/28967)** *(closed)* — Fixes [#28954](https://github.com/google-gemini/gemini-cli/issues/28954): `refreshStatic()` called `clearTerminal` in non-alternate-buffer mode, wiping scrollback on xterm/Alacritty/GNOME Terminal. Worth tracking even in closed state if you use Gemini CLI outside the alternate buffer.

### 5. Feature Request Trends

- **Agentic autonomy with guardrails**: Users want the agent to *choose* skills/subagents on its own ([#21968](https://github.com/google-gemini/gemini-cli/issues/21968)) but also to avoid destructive `git reset`/`--force` behavior ([#22672](https://github.com/google-gemini/gemini-cli/issues/22672)).
- **Codebase understanding via ASTs**: A cluster of issues proposes AST-aware reading/search/mapping ([#22745](https://github.com/google-gemini/gemini-cli/issues/22745), [#22746](https://github.com/google-gemini/gemini-cli/issues/22746)) plus "tactful extraction" to avoid token firehosing ([#19561](https://github.com/google-gemini/gemini-cli/issues/19561)).
- **Native OS sandboxing**: Zero-dependency sandboxing plus post-execution intent routing ([#19873](https://github.com/google-gemini/gemini-cli/issues/19873)) is the flagship proposal for unlocking Gemini's bash fluency safely.
- **Browser agent resilience**: Persistent-profile lock recovery, settings overrides, and Wayland support ([#22232](https://github.com/google-gemini/gemini-cli/issues/22232), [#22267](https://github.com/google-gemini/gemini-cli/issues/22267), [#21983](https://github.com/google-gemini/gemini-cli/issues/21983)).
- **Persistent task tracking**: Multiple requests to replace in-context `WriteToDo` with file-based CRUD tracking ([#18836](https://github.com/google-gemini/gemini-cli/issues/18836), [#21000](https://github.com/google-gemini/gemini-cli/issues/21000)).
- **Subagent observability**: Shareable subagent trajectories via `/chat share` ([#22598](https://github.com/google-gemini/gemini-cli/issues/22598)) and subagent context included in `/bug` reports ([#21763](https://github.com/google-gemini/gemini-cli/issues/21763)).

### 6. Developer Pain Points

- **Silent false success**: `MAX_TURNS` and interruptions are reported as goal success, eroding trust in autonomous runs ([#22323](https://github.com/google-gemini/gemini-cli/issues/22323)); bug reports lack subagent context to debug this ([#21763](https://github.com/google-gemini/gemini-cli/issues/21763)).
- **Unexpected hangs**: Generalist-agent deferrals ([#21409](https://github.com/google-gemini/gemini-cli/issues/21409)), completed shell commands stuck at "Waiting input" ([#25166](https://github.com/google-gemini/gemini-cli/issues/25166)), and interactive prompts (e.g., `vite`) ([#22465](https://github.com/google-gemini/gemini-cli/issues/22465)) are recurring workflow killers.
- **Configuration that silently doesn't apply**: Browser agent ignoring `settings.json` ([#22267](https://github.com/google-gemini/gemini-cli/issues/22267)), symlinked custom agents not recognized ([#20079](https://github.com/google-gemini/gemini-cli/issues/20079)), and explicit models being rewritten ([#29217](https://github.com/google-gemini/gemini-cli/pull/29217), [#29222](https://github.com/google-gemini/gemini-cli/pull/29222)).
- **Workspace and context hygiene**: Models scattering temp scripts across directories ([#23571](https://github.com/google-gemini/gemini-cli/issues/23571)) and large file reads bloating context by ~15k tokens/turn ([#19561](https://github.com/google-gemini/gemini-cli/issues/19561)).
- **Security around memory and destructive commands**: Secrets enter model context before redaction ([#26525](https://github.com/google-gemini/gemini-cli/issues/26525)), and the model occasionally reaches for `--force`/`git reset` when safer alternatives exist ([#22672](https://github.com/google-gemini/gemini-cli/issues/22672)).

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI Community Digest — 2026-09-06

## Today's Highlights

No releases or PRs landed in the last 24 hours; activity is concentrated in a major wave of newly filed triage issues. The dominant themes are upgrade regressions (#4734, #4728), MCP tool-runtime defects (#4731, #4721, #4729), and transparency bugs that hide or lose assistant output (#4735, #4733). The most persistent community request remains cancelable queued messages (#1857), which has accumulated 28 👍 and 11 comments since March.

## Releases

No new releases in the last 24 hours.

## Hot Issues

1. **[#1857 — Allow cancel/removal of enqueued messages before execution](https://github.com/github/copilot-cli/issues/1857)**  
   The most-discussed open request (28 👍, 11 comments). Users want to cancel or remove messages queued via `Ctrl+Q`/`Ctrl+Enter` while the agent is busy or during `/compact`; today there is no escape hatch once messages are queued. Community interest has grown steadily since March.

2. **[#4734 — "Worktree missing" on all project sessions after upgrade to desktop 2.98.0 / runtime 1.1.15](https://github.com/github/copilot-cli/issues/4734)**  
   A freshly filed regression report that affects every existing and new worktree-backed session after an auto-update. Severe for anyone relying on worktree-based workflows, with no workaround documented yet.

3. **[#4728 — Auto-update rewrites the running `copilot.exe`, breaking the GitHub Copilot desktop app](https://github.com/github/copilot-cli/issues/4728)**  
   Running the CLI in a terminal can silently break the bundled desktop app session manager; all existing sessions fail with "Session unavailable." A packaging/self-update defect with broad blast radius.

4. **[#4725 — Frequent JavaScript heap out of memory on Linux](https://github.com/github/copilot-cli/issues/4725)**  
   The CLI crashes every few minutes with Mark-Compact allocation failures during long-running sessions, pointing to a memory leak. Stability is a top concern for users running the agent continuously.

5. **[#4731 — MCP `tools/list` refresh blocked by a just-cancelled tool call permanently strips server tools](https://github.com/github/copilot-cli/issues/4731)**  
   After a client-side timeout, the runtime sends a `tools/list` refresh back into the still-busy server; when it times out, that server's tools disappear for the rest of the process. Effectively a poisoned-MCP-server bug.

6. **[#4735 — User-facing text before a tool call is reclassified as "Thought for Ns" and never shown](https://github.com/github/copilot-cli/issues/4735)**  
   Multi-paragraph assistant text emitted before a tool call gets folded into a collapsed reasoning summary, so developers never see it. A transparency/regression issue that erodes trust in agent output.

7. **[#4721 — `open_canvas` arguments corrupted by JSON-RPC serialization](https://github.com/github/copilot-cli/issues/4721)**  
   Tool-call arguments are concatenated with a trailing `}{}`, producing malformed JSON and truncating `open_canvas` values. Points to a serialization bug affecting MCP/canvas extensions.

8. **[#4729 — Built-in research agent instructs subagents to call unavailable `github/get_me` tool](https://github.com/github/copilot-cli/issues/4729)**  
   The research subagent prompt references a GitHub MCP tool that isn't exposed in the session; the agent visibly burns tokens reconciling the mismatch. An agent-prompt ↔ available-tools consistency bug.

9. **[#4732 — Unprompted switch to GPT-5 mini stops mid-task and produces empty patches](https://github.com/github/copilot-cli/issues/4732)**  
   A user reports the model switched to GPT-5 mini, made patches empty, and declared itself done; retries stop again. Raises concerns about default model routing and silent reliability regressions.

10. **[#4723 — `--interactive <prompt>` silently dropped when using a local plugin custom agent](https://github.com/github/copilot-cli/issues/4723)**  
   The TUI starts with the requested agent, but the startup prompt is never submitted. Silent failures like this are especially dangerous for scripted/automated usage.

Also noteworthy: enterprise model-selection issue [#4272](https://github.com/github/copilot-cli/issues/4272) and the `streaming: false` `message_delta` bug [#4677](https://github.com/github/copilot-cli/issues/4677) were both closed during the period.

## Key PR Progress

No pull requests were merged or updated in the last 24 hours.

## Feature Request Trends

- **Keyboard and queue control**: Users want finer control over input — canceling enqueued messages (#1857) and making `Ctrl+E` accept inline autocomplete suggestions Emacs-style (#4736).
- **Proactive context compaction**: Auto-compaction on idle, aligned to the model's ~5-minute prompt-cache TTL, to avoid re-reading the full uncached context on the next turn (#4724).
- **Enterprise model flexibility**: Continued demand for admins to explicitly enable/disable new models instead of confusing greyed-out states (#4272).
- **Resilient sessions**: Implicit feature demand behind upgrade/reload regressions: sessions should survive desktop app upgrades, auto-updates, and editor reloads without losing worktrees or reconnect state (#4734, #4728, #4726).

## Developer Pain Points

- **Upgrade/self-update breakage**: Auto-updates corrupt the running binary and invalidate existing sessions (#4728, #4734).
- **Stability under long-running use**: Recurring OOM crashes on Linux interrupt continuous agent workflows (#4725).
- **MCP integration fragility**: Timed-out calls poison servers (#4731), JSON-RPC args get corrupted (#4721), and built-in prompts reference tools that don't exist (#4729).
- **Silent loss of work or output**: Startup prompts disappear (#4723), user-facing text is collapsed into hidden reasoning (#4735), and token-limit truncation can drop responses and their continuations (#4733).
- **Model behavior unpredictability**: Unexpected model routing (GPT-5 mini) that stops mid-task or returns empty patches undermines trust in generated results (#4732).

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI Community Digest — 2026-09-06

## Today's Highlights
No new release shipped in the last 24 hours, and community activity was light: maintainers closed three long-dormant bugs (Zed IDE Windows launch failure, recurring “Authorization failed” errors on Linux, and shell prompt losing cwd/git-branch context) while a new VS Code extension rendering bug surfaced. The single PR touched in the window hardens tool-call argument decoding against double-encoded JSON from the Moonshot API.

## Hot Issues
Only 4 issues received updates in the window; all are covered below.

1. **[#1284 — [bug] Does not launch in Zed IDE ACP panel in Windows (CLOSED)](https://github.com/MoonshotAI/kimi-cli/issues/1284)**
   kimi-cli 1.14.0 on Windows NT 10.0.26200 x64 fails to start inside the Zed IDE ACP panel. Open since late February, closed on 2026-09-06 with a single comment. Matters because Windows + Zed is a growing IDE-cli workflow, and an ACP launch failure blocks the entire agentic loop for those users.

2. **[#1350 — [bug] 频繁出现 Authorization failed, please check your login status (CLOSED)](https://github.com/MoonshotAI/kimi-cli/issues/1350)**
   Debian 12 user on kimi 1.17.0 with `kimi-for-coding` reports repeated authorization failures despite a successful `/login`. Closed 2026-09-06 after nearly six months with zero public discussion. Matters because auth flakiness is a high-friction blocker—users can't trust `/login` to persist across sessions.

3. **[#1349 — shell prompt no longer shows cwd/git branch; request configurable display (CLOSED)](https://github.com/MoonshotAI/kimi-cli/issues/1349)**
   A regression reduced the shell prompt to decorative glyphs (`✨ / 💫 / $`), dropping the current working directory and git branch. The author explicitly asks for a configurable prompt. Closed 2026-09-06, 0 comments. Matters because interactive CLI work depends on prompt context to verify which repo/directory commands will touch.

4. **[#2635 — VS Code extension: streamed chat text drops individual characters at render/copy layer (OPEN)](https://github.com/MoonshotAI/kimi-cli/issues/2635)**
   Filed 2026-09-05: characters intermittently vanish from rendered assistant output in the Kimi Code VS Code chat panel. Wire-log verification confirms the model output is intact, pointing to render/copy-layer loss. No comments yet. Matters because missing characters corrupt transcripts and copied code snippets, undermining trust in chat history.

## Key PR Progress
1 PR was updated in the window.

1. **[#2513 — fix(kosong): recursively decode double-encoded tool-call arguments (OPEN)](https://github.com/MoonshotAI/kimi-cli/pull/2513)**
   Open since 2026-07-19, last touched 2026-09-06. The Moonshot API can return `function.arguments` with nested array/object values as JSON strings; a single `json.loads` leaves values like `todos` as strings and breaks Pydantic validation (`Input should be a valid list`). The PR adds a shared `decode_tool_arguments` helper for recursive decoding—an important correctness fix for any tool/agent workflows built on kosong.

## Feature Request Trends
With only 4 issues in the window, signal is limited but coherent. The one explicit feature request is **configurable shell prompt display** (#1349)—users want control over showing cwd and git-branch context. The implicit theme across the other reports is **fidelity and portability of the interactive surface**: the CLI should behave identically on Windows IDE panels (#1284), maintain stable authenticated sessions on Linux (#1350), and never lose streamed output in editor extensions (#2635). The distilled direction: “let users see and trust their working context everywhere the CLI runs.”

## Developer Pain Points
- **Auth reliability**: repeated “Authorization failed” after successful `/login` on Debian 12 makes session/token persistence feel fragile (#1350).
- **Windows/IDE integration gaps**: the CLI doesn't even launch in Zed's ACP panel on Windows (#1284).
- **Lost repo context**: recent prompt regressions hid cwd and git branch, making it harder to verify where commands will execute during long interactive sessions (#1349).
- **Corrupted streamed output**: VS Code extension drops characters at the render/copy layer, so chat history and copied code can't be fully trusted even when the wire log is correct (#2635).

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-09-06

**Source:** [github.com/anomalyco/opencode](https://github.com/anomalyco/opencode)

## 1. Today's Highlights

No releases shipped, but 2026-09-06 was a high-velocity stabilization day: a large batch of older bugs (mostly from June/July) was closed, and the core team pushed a coordinated series of PRs fixing Desktop/Web client connection resilience — stalled event streams, wedged request queues, excessive re-syncs, and CORS preflight overhead. Two fresh issues filed today also got immediate attention: an SQLite multi-process locking bug ([#47566](https://github.com/anomalyco/opencode/issues/47566)) already has a fix PR open ([#47567](https://github.com/anomalyco/opencode/pull/47567)), and a new Go subscription billing miscalculation ([#47547](https://github.com/anomalyco/opencode/issues/47547)) looks like a dashboard-math defect. A visible community contribution wave also landed — several Desktop UI polish PRs from contributor `iamdavidhill` were closed ([#47394](https://github.com/anomalyco/opencode/pull/47394), [#47444](https://github.com/anomalyco/opencode/pull/47444), [#47375](https://github.com/anomalyco/opencode/pull/47375), [#47211](https://github.com/anomalyco/opencode/pull/47211), [#47378](https://github.com/anomalyco/opencode/pull/47378), [#47387](https://github.com/anomalyco/opencode/pull/47387)).

## 2. Releases

None published in the last 24 hours. The next release will likely aggregate the Desktop/Web client reliability fixes and the SQLite retry logic described below.

## 3. Hot Issues

1. **[OPEN] #32747 — `@` file mentions do not include files created after startup** ([link](https://github.com/anomalyco/opencode/issues/32747))  
   Newly created files are missing from the `@` picker until restart. With 15 comments and 13 👍, this is the most-discussed issue in the window; root cause points to a stale TUI file-search index (not a filesystem-watcher bug). Important because long-running sessions are the default workflow for agentic coding.

2. **[OPEN] #47547 — Go subscription blocked: monthly usage shows 100% via sum of per-model percentages** ([link](https://github.com/anomalyco/opencode/issues/47547))  
   Filed today. The usage dashboard appears to sum per-model percentages (DeepSeek V4 Flash 47.8%, V4 Pro 34.7%, etc.) to 100%, blocking the account, even though actual dollar spend is well under the ~$60 plan cap. High-impact billing bug — users can lose access mid-month due to bad math.

3. **[OPEN] #47566 — Concurrent opencode processes on one database fail with SQLITE_BUSY** ([link](https://github.com/anomalyco/opencode/issues/47566))  
   Filed today. Multiple processes sharing a data directory contend for SQLite's single write lock; statements waiting past the 5s `busy_timeout` die with "Failed to execute statement". Directly relevant to developers running TUI + Desktop + `opencode serve` in parallel. Fix already proposed in [#47567](https://github.com/anomalyco/opencode/pull/47567).

4. **[OPEN] #42627 — Desktop: remote server on Windows cannot load projects/sessions from macOS client** ([link](https://github.com/anomalyco/opencode/issues/42627))  
   macOS Desktop connects to a Windows `opencode serve` instance, but remote projects/sessions are unusable even though the underlying API works. Cross-platform remote workflows remain broken; open since August 14.

5. **[CLOSED] #35741 — WebChat LLM hallucinates user responses** ([link](https://github.com/anomalyco/opencode/issues/35741))  
   In agent/build mode, the model asks a clarifying question, then fabricates the user's answer ("the user chose option 1") and continues autonomously. A serious agentic-loop correctness issue, now closed.

6. **[CLOSED] #35690 / #35750 — Session history disappears after upgrading to 1.17.x** ([link #35690](https://github.com/anomalyco/opencode/issues/35690), [link #35750](https://github.com/anomalyco/opencode/issues/35750))  
   Migrations from ≤1.14 or 1.17.13 into 1.17.14 hid older sessions because a new `path` column was never back-filled. Sessions remained intact in `opencode.db` — a scary data-loss false alarm caused by an incomplete migration.

7. **[CLOSED] #35009 — High resource usage after updating 1.17.11 → 1.17.13** ([link](https://github.com/anomalyco/opencode/issues/35009))  
   Reports of ~1GB RSS and ~22% CPU during otherwise normal conversation sessions. Regression was investigated and closed today.

8. **[CLOSED] #31916 / #32046 — Large diffs freeze the UI** ([link #31916](https://github.com/anomalyco/opencode/issues/31916), [link #32046](https://github.com/anomalyco/opencode/issues/32046))  
   TUI hangs on "Preparing to write..." and the Desktop renderer becomes unresponsive when computing diffs of 150+ lines — unbounded diff rendering, affecting both surfaces. Closed after months.

9. **[CLOSED] #34030 — OpenCode can't invoke enterprise third-party models added to GitHub Copilot** ([link](https://github.com/anomalyco/opencode/issues/34030))  
   Copilot Enterprise accounts with custom/model catalog additions could not see or use those models from OpenCode. Enterprise/proxy deployments depend on this working.

10. **[CLOSED] #35742 — Plugins are not updated to latest version** ([link](https://github.com/anomalyco/opencode/issues/35742))  
    `opencode plugin <name>@latest --force` still served the plugin from cache; only pinning an explicit version triggered a download. Annoying for anyone iterating on plugin releases.

## 4. Key PR Progress

1. **[#47567 — `fix(core)`: retry SQLite statements on lock timeout**](https://github.com/anomalyco/opencode/pull/47567) (OPEN, `[needs:compliance]`)  
   Closes [#47566](https://github.com/anomalyco/opencode/issues/47566). Instead of treating `SQLITE_BUSY` as a fatal `Effect.orDie`, retry with backoff. This is the right fix for multi-process/shared-data-dir workflows.

2. **[#47571 — `fix(client)`: detect stalled event streams and resync on foreground**](https://github.com/anomalyco/opencode/pull/47571) (OPEN)  
   A phone locked with the app open comes back to a frozen session because the suspended socket never reports failure on the hung `reader.read()`. Adds stall detection and automatic re-sync on foreground.

3. **[#47572 — `fix(app)`: time out requests the server never answers**](https://github.com/anomalyco/opencode/pull/47572) (OPEN)  
   Dead sockets can leave `fetch` waiting minutes for headers; with only 4 in-flight request slots, a few dead requests wedge every later API call. Adds client-side request timeouts.

4. **[#47573 — `fix(app)`: refresh queued inputs when the connection returns**](https://github.com/anomalyco/opencode/pull/47573) (OPEN)  
   Companion to #47571: `session.pending` queued inputs were lost after a reconnect because they were only loaded keyed on session ID. Now replayed once the session metadata reloads.

5. **[#47560 — `fix(desktop)`: keep server CORS headers so preflights cache**](https://github.com/anomalyco/opencode/pull/47560) (CLOSED)  
   Every Desktop API call was doubled by an `OPTIONS` preflight: a 310s netlog showed 759 real calls + 759 preflights, with Chromium's preflight cache logging `hit-and-fail` 591 times. This meaningfully cuts local-server round trips.

6. **[#47561 — `fix(client)`: coalesce catalog refetches from event bursts**](https://github.com/anomalyco/opencode/pull/47561) (CLOSED)  
   A Desktop netlog showed `GET /api/mcp` fetched **192 times in 7 seconds** — one refetch per MCP-server status event during startup. Coalescing invalidate+sync events turns event bursts into a single fetch.

7. **[#47564 — `fix(app)`: keep slow git reads from filling the request queue**](https://github.com/anomalyco/opencode/pull/47564) (OPEN)  
   Slow endpoints (e.g. `GET /api/vcs` at p50 ~1.8s) can occupy all 4 request-queue slots during session-tab mounts, starving everything else. Separates long-running reads from the request queue's pacing.

8. **[#47565 — `fix(app)`: pace directory re-sync after reconnect**](https://github.com/anomalyco/opencode/pull/47565) (OPEN)  
   On reconnect, each active directory triggered both a queued refresh and an immediate bypassing sync — a ~20-directory burst. This PR routes re-syncs through the existing two-at-a-time pacing.

9. **[#47548 — `feat(core)`: discover Bedrock credentials in the provider plugin**](https://github.com/anomalyco/opencode/pull/47548) (CLOSED)  
   Follow-up to the native Bedrock route's AWS default credential chain ([#47436](https://github.com/anomalyco/opencode/pull/47436)): wires discovery into the plugin so `amazon-bedrock` is actually usable with EC2/IMDS or shared-config credentials.

10. **[#47555 — `fix(tui)`: stop fetching placeholder session id on `--continue`**](https://github.com/anomalyco/opencode/pull/47555) (OPEN)  
    Fixes [#47556](https://github.com/anomalyco/opencode/issues/47556). `opencode --continue` seeds a `"dummy"` placeholder session ID; the route eagerly fetches it and the server 400s (IDs must start with `ses_`). Removes the placeholder fetch and settles only once the real session is resolved.

## 5. Feature Request Trends

- **Unify Desktop and TUI storage/session state.** Users keep hitting the split-brain problem: Desktop sessions invisible in CLI ([#29071](https://github.com/anomalyco/opencode/issues/29071)), and a direct request to unify directories for sessions, plugins, and agents across Desktop and TUI ([#35703](https://github.com/anomalyco/opencode/issues/35703)). This is the strongest recurring direction — one workspace, many frontends.
- **Plugin session-lifecycle and background control.** Repeated requests for session lifecycle hooks ([#28695](https://github.com/anomalyco/opencode/issues/28695)) and the ability for `background.extend()` to inject messages into an *active* session loop ([#35728](https://github.com/anomalyco/opencode/issues/35728)). Plugin authors want first-class multi-agent/background workflows, not fire-and-forget.
- **Provider parity and enterprise flexibility.** Enterprise Copilot third-party models ([#34030](https://github.com/anomalyco/opencode/issues/34030)), explicit Claude reasoning support on compatible backends ([#35733](https://github.com/anomalyco/opencode/issues/35733)), a clear GPT-5.5 tool-calling path on custom OpenAI-compatible providers ([#35732](https://github.com/anomalyco/opencode/issues/35732)), and Bedrock IMDS autoloading ([#35798](https://github.com/anomalyco/opencode/issues/35798)) all point to the same need: no provider/model left behind.
- **Cost & usage transparency.** Users want the dashboard math to reflect real dollars, not summed percentages ([#47547](https://github.com/anomalyco/opencode/issues/47547)), and are confused by unexpectedly high session costs after switching models ([#35792](https://github.com/anomalyco/opencode/issues/35792)). Expect pressure for per-session cost attribution and provider-accurate pricing.
- **Desktop GUI quality-of-life.** Feature requests include a Codex-style sidebar browser preview for live frontend iteration ([#35751](https://github.com/anomalyco/opencode/issues/35751)) — a signal that Desktop is increasingly treated as the primary surface.

## 6. Developer Pain Points

- **Upgrade regressions erode trust.** The 1.17.13/1.17.14 wave produced session-disappearing migrations ([#35690](https://github.com/anomalyco/opencode/issues/35690), [#35750](https://github.com/anomalyco/opencode/issues/35750)), RAM/CPU spikes ([#35009](https://github.com/anomalyco/opencode/issues/35009)), and large-diff UI freezes ([#31916](https://github.com/anomalyco/opencode/issues/31916), [#32046](https://github.com/anomalyco/opencode/issues/32046)). A recurring pattern: new sessions work fine while existing sessions degrade — see also Go models appearing "stuck" only in old sessions on Windows ([#35611](https://github.com/anomalyco/opencode/issues/35611)).
- **Stale indexes and caches.** Files created after startup don't appear in `@` mentions ([#32747](https://github.com/anomalyco/opencode/issues/32747)); `plugin@latest --force` ignores `latest` ([#35742](https://github.com/anomalyco/opencode/issues/35742)); MCP catalog events trigger 192 redundant fetches ([#47561](https://github.com/anomalyco/opencode/pull/47561)). The recurring fix pattern is coalescing, cache invalidation, and re-sync — clearly needed.
- **Single-writer SQLite under multi-process usage.** The 5s `busy_timeout` + fatal-error path ([#47566](https://github.com/anomalyco/opencode/issues/47566)) punishes users who legitimately run TUI, Desktop, and remote server processes against one data directory.
- **Platform asymmetry.** Windows-specific stalls (TUI diff writing, Desktop renderer freezes, slow inference in existing sessions) and the macOS-client → Windows-server mismatch ([#42627](https://github.com/anomalyco/opencode/issues/42627)) show cross-platform testing is still catching up.
- **Agent-model behavior quirks on third-party backends.** DeepSeek returning DSML/XML instead of JSON tool calls ([#34676](https://github.com/anomalyco/opencode/issues/34676)), GLM-5.2 looping on file reads ([#35784](https://github.com/anomalyco/opencode/issues/35784)), and the model answering its own clarifying questions in WebChat ([#35741](https://github.com/anomalyco/opencode/issues/35741)) all add up: developers spend significant time diagnosing model/provider behavior rather than their own code.

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi Community Digest — 2026-09-06

## Today's Highlights

Pi shipped **v0.85.1** with native GPT-6 Astra support across OpenAI API keys and Codex subscriptions, but the 0.85.x packaging regression — undeclared `pi-server`/`pi-client` runtime dependencies — continues to break fresh installs and subagent child sessions. Multiple PRs are now landing fixes for packaging, extension-runner races, and a broader architecture push to deliver mid-session prompt/tool changes as system-message deltas ([#9116](https://github.com/earendil-works/pi/pull/9116), [#9117](https://github.com/earendil-works/pi/pull/9117)). Community attention this week is concentrated on Windows/TUI reliability (a 52-comment coordination thread) and provider-gateway correctness issues affecting Copilot, Vercel gateway, and local Ollama models.

## Releases

**v0.85.1** — GPT-6 Astra is now available through OpenAI API keys and OpenAI Codex subscriptions. See the [provider documentation](https://github.com/earendil-works/pi/blob/v0.85.1/packages/coding-agent/docs/providers.md) for API-key setup and Codex subscription details.

## Hot Issues

- **[#7547 — How do you use Pi on Windows? What issues are you seeing?](https://github.com/earendil-works/pi/issues/7547)** *(Open)* — The community's main Windows coordination thread with **52 comments**. There are too many supported ways to run Pi on Windows, making it unclear where maintainers should focus energy versus deferring to external tooling. This is the pulse point for Windows developer experience.

- **[#5023 — Terminal scrolls to the beginning without reason](https://github.com/earendil-works/pi/issues/5023)** *(Closed)* — A disruptive and long-standing bug (filed in May) where the terminal randomly jumps to the session start and then fast-scrolls to the end while the model is working. **19 comments and 3 👍** show it is a frequently hit annoyance for users watching long agentic runs.

- **[#9132 — 0.85.0: published dist/cli.js statically imports undeclared @earendil-works/pi-server](https://github.com/earendil-works/pi/issues/9132)** *(Closed)* — The npm tarball for 0.85.0 shipped a non-bundled build with an undeclared runtime dependency. **5 👍 — the highest-reacted issue in this batch** — reflecting how quickly fresh `npm install` users hit a broken package root.

- **[#9218 — Global install 0.85.1: subagent child runs fail](https://github.com/earendil-works/pi/issues/9218)** *(Closed)* — A follow-on packaging regression: `@earendil-works/pi-client` and `pi-server` are omitted from the dependency list, so `pi-subagents` child sessions cannot start. This matters because subagent delegation is core to agentic workflows.

- **[#8684 — PI_OFFLINE silently disables provider model discovery](https://github.com/earendil-works/pi/issues/8684)** *(Open)* — Documented as limiting startup housekeeping, `PI_OFFLINE` actually disables all provider model-catalog discovery for the session. Undocumented scope changes like this are dangerous for users running custom/offline configurations.

- **[#8896 — /export HTML silently drops context sent to the model](https://github.com/earendil-works/pi/issues/8896)** *(Open)* — Custom messages with `display: false` are quietly omitted from HTML exports, even though they were part of the provider context. This is an audit/export-fidelity problem — users cannot inspect what the model actually saw.

- **[#6300 — Windows: input line redrawn on every keystroke](https://github.com/earendil-works/pi/issues/6300)** *(Open)* — On Windows 10, each character appears on a new line, making the TUI effectively unusable. Still open after two months with 8 comments, this is one of the clearest Windows TUI blockers.

- **[#9209 — GitHub Copilot GPT-6 Astra routed to unsupported Chat Completions endpoint](https://github.com/earendil-works/pi/issues/9209)** *(Closed)* — Pi routes `github-copilot/gpt-6-astra` to `/chat/completions`, which Copilot rejects with `400 unsupported_api_for_model`. First-day friction for the headline GPT-6 Astra feature.

- **[#9212 — Sonnet-5 via gateway: 13% of edit calls truncated to `edits:[{}]`](https://github.com/earendil-works/pi/issues/9212)** *(Closed)* — A provider-level reliability data point: 18 of 134 `edit` tool calls from `claude-sonnet-5` via `vercel-ai-gateway` failed schema validation due to truncated arguments. For agentic editing workflows, failure rates in the teens are serious.

- **[#9216 — Ollama qwen3.8:27b: stream "terminated" errors + auto-compaction stops re-triggering](https://github.com/earendil-works/pi/issues/9216)** *(Closed)* — A clean 0.84.x → 0.85.x regression for local-model users, pairing repeated `stopReason: "terminated"` errors with auto-compaction only firing once. Local and self-hosted LLM users are a key Pi constituency, so regressions here get attention quickly.

## Key PR Progress

- **[#9170 — fix(coding-agent): declare pi-server runtime dependency](https://github.com/earendil-works/pi/pull/9170)** *(Closed)* — Fixes the 0.85.0 packaging defect that made the public package root unimportable on fresh installs. Critical release-integrity repair.

- **[#9172 — fix(coding-agent): prevent broken package root publication](https://github.com/earendil-works/pi/pull/9172)** *(Closed)* — Builds on #9170 as a guard against the same class of packaging defect being published again. Automated protection rather than one-off fix.

- **[#9222 — fix(coding-agent): reject reload during active session operations](https://github.com/earendil-works/pi/pull/9222)** *(Open)* — Prevents an RPC-mode extension reload from invalidating the runner while a tool is still executing, which previously caused error results to be stored and sent back to the model.

- **[#9116 — feat(ai): add mid-conversation system messages](https://github.com/earendil-works/pi/pull/9116)** *(Open)* — First layer of the #8998 split: anything that changes mid-session (extension tools, prompts, metadata) can flow as a system message instead of rewriting the whole conversation. Foundational architecture work by mitsuhiko.

- **[#9117 — feat(coding-agent): deliver prompt and tool changes as system message deltas](https://github.com/earendil-works/pi/pull/9117)** *(Open)* — The second stacked layer: wires the coding agent to send prompt/tool loadout changes as deltas, eliminating full top-level prompt rewrites between requests. Meaningful token and context-window savings for long sessions.

- **[#9096 — feat(ai, coding-agent): add Meta provider with Muse subscription OAuth](https://github.com/earendil-works/pi/pull/9096)** *(Open)* — Adds Meta's Muse as a built-in provider. Notable quirks flagged by the author: refresh tokens are re-minted daily from an identity token, and streaming is currently burst-style rather than incremental.

- **[#9214 — Invoke skills and prompt templates mid-sentence](https://github.com/earendil-works/pi/pull/9214)** *(Closed)* — Directly addresses the popular #8457 request, letting `/skill:name args` and `/template args` expand inline in the middle of a message rather than only at the start.

- **[#9163 — feat(tui): simplify clipboard handling](https://github.com/earendil-works/pi/pull/9163)** *(Closed)* — Replaces a heavyweight Rust clipboard dependency with a much smaller native shim, removing an obstacle to building on NixOS and other platforms.

- **[#9137 — feat(coding-agent): add Nix flake](https://github.com/earendil-works/pi/pull/9137)** *(Open, WIP)* — mitsuhiko's WIP Nix flake addresses reproducible/distributable installs — a recurring community request alongside the packaging issues.

- **[#7970 — feat(coding-agent): show when fullscreen transcript is scrolled up](https://github.com/earendil-works/pi/pull/7970)** *(Closed)* — Adds a `↓` indicator in the status row when the transcript is not following the live end; scrolling back to bottom clears it. Implements #7908, with a small visual demo GIF in the PR.

## Feature Request Trends

- **Broader native provider and API coverage.** Users keep asking for first-class providers and newer API capabilities: Requesty as a native provider ([#5473](https://github.com/earendil-works/pi/issues/5473)), LLM Gateway + DevPass providers ([#7610](https://github.com/earendil-works/pi/pull/7610)), OpenAI async tool calling ([#9113](https://github.com/earendil-works/pi/issues/9113)), and server-side compaction/stateful continuation for OpenAI Responses and Codex ([#7317](https://github.com/earendil-works/pi/issues/7317), [#6676](https://github.com/earendil-works/pi/issues/6676)).
- **Mid-session invocation and context fidelity.** Skills and templates usable mid-sentence ([#8457](https://github.com/earendil-works/pi/issues/8457), now in PR #9214), package namespaces to avoid resource collisions ([#8834](https://github.com/earendil-works/pi/issues/8834)), exports that include everything sent to the model ([#8896](https://github.com/earendil-works/pi/issues/8896)), and system-message-delta delivery ([#9116](https://github.com/earendil-works/pi/pull/9116), [#9117](https://github.com/earendil-works/pi/pull/9117)).
- **TUI ergonomics and consistency.** Users want uniform keybinding paradigms across menus ([#9199](https://github.com/earendil-works/pi/issues/9199)), a move of the pending indicator into the input border ([#1932](https://github.com/earendil-works/pi/issues/1932)), and scroll-away awareness in fullscreen transcripts ([#7908](https://github.com/earendil-works/pi/issues/7908)).
- **First-class distribution & packaging.** A formal Nix flake ([#9137](https://github.com/earendil-works/pi/pull/9137)), a coherent Windows support story ([#7547](https://github.com/earendil-works/pi/issues/7547)), and publication guardrails to stop broken tarballs ([#9172](https://github.com/earendil-works/pi/pull/9172)).

## Developer Pain Points

- **Broken npm packages.** Missing runtime dependencies in published releases break fresh installs and subagent sessions: #9132 and #9218.
- **Windows TUI blockers.** Per-keystroke input redraws ([#6300](https://github.com/earendil-works/pi/issues/6300)), stuck IME candidate windows ([#5200](https://github.com/earendil-works/pi/issues/5200)), and incorrect fullscreen image rendering ([#9169](https://github.com/earendil-works/pi/issues/9169)) make Windows a rough experience despite the large developer population there ([#7547](https://github.com/earendil-works/pi/issues/7547)).
- **Gateway/provider reliability and cost errors.** Truncated edit tool calls ([#9212](https://github.com/earendil-works/pi/issues/9212)), wrong-endpoint routing for Copilot GPT-6 Astra ([#9209](https://github.com/earendil-works/pi/issues/9209)), inert `vercelGatewayRouting` config ([#9211](https://github.com/earendil-works/pi/issues/9211)), and 1h cache writes billed at the 5m rate ([#9210](https://github.com/earendil-works/pi/issues/9210)).
- **Race conditions in agent/extension lifecycles.** Slash-command autocomplete races with fast typing/IME ([#9220](https://github.com/earendil-works/pi/issues/9220)), reload during active tool execution ([#9222](https://github.com/earendil-works/pi/pull/9222)), invalidated extension runners during `/new` ([#9182](https://github.com/earendil-works/pi/pull/9182)), and compact messages missing the immediate overflow retry ([#9051](https://github.com/earendil-works/pi/issues/9051)).
- **Undocumented or over-broad behavior.** Environment variables that silently exceed their documented scope ([#8684](https://github.com/earendil-works/pi/issues/8684)) and exports that silently omit model-visible context ([#8896](https://github.com/earendil-works/pi/issues/8896)).
- **Local-model regressions.** Ollama stream `terminated` failures and one-shot auto-compaction after upgrading to 0.85.x ([#9216](https://github.com/earendil-works/pi/issues/9216)) show the cost of regressions in the local-inference path.

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-09-06

## 1. Today's Highlights

The web-shell gets a major new capability across all recent release tracks: dynamic workflow-run visualization and management ([#10594](https://github.com/QwenLM/qwen-code/pull/10594)), alongside a performance improvement for session-workflow derivation. Meanwhile, the export-bloat saga continues to dominate discussion — a 19.5 MB exported HTML file ([#11031](https://github.com/QwenLM/qwen-code/issues/11031)) and a 6 MB mermaid bundle ([#11091](https://github.com/QwenLM/qwen-code/issues/11091)) show the "stop embedding the Web Shell runtime" effort is still mid-flight. Finally, daemon/serve reliability is again the top bug theme: a new P1 warns that background shell output can be silently dropped when the session runtime recycles ([#11119](https://github.com/QwenLM/qwen-code/issues/11119)), while the long-standing MCP-config-at-startup bug ([#7771](https://github.com/QwenLM/qwen-code/issues/7771)) finally has a fix PR up.

## 2. Releases

Three builds landed in the last 24 hours:

- **v0.23.1-preview.0** — current preview channel
- **v0.23.0-nightly.20260905.0c945a6136**
- **v0.23.0-nightly.20260905.e3d26283e6**

All three ship the same two headline changes:

- **feat(web-shell):** visualize and manage dynamic workflow runs ([#10594](https://github.com/QwenLM/qwen-code/pull/10594))
- **perf(web-shell):** derive the session workflow project more efficiently (truncated in release notes)

Note: the v0.23.1-preview.0 release workflow hit `quality` job failures twice ([#11098](https://github.com/QwenLM/qwen-code/issues/11098), [#11114](https://github.com/QwenLM/qwen-code/issues/11114)), and nightly `0c945a6136` failed its `integration_docker` job ([#11138](https://github.com/QwenLM/qwen-code/issues/11138)) — release-pipeline reliability remains shaky.

## 3. Hot Issues

- **[#11091 — fix(export): mermaid (~6 MB) still flattened into the exported transcript renderer](https://github.com/QwenLM/qwen-code/issues/11091)** — Highest engagement today (6 comments). Follows the #9812 move to a CDN-loaded, SRI-pinned renderer; mermaid and other heavy deps are still being bundled into the transcript entry, undoing much of the size win.
- **[#11031 — [P1] stop embedding the Web Shell runtime in every HTML file](https://github.com/QwenLM/qwen-code/issues/11031)** — Exporting an *empty* session produces a 19.5 MB HTML file because the full React/Web Shell graph is copied into each artifact. The core of the export-data roadmap; affects every user who shares transcripts.
- **[#11119 — [P1] background shell output and wake notifications silently dropped when the session runtime recycles](https://github.com/QwenLM/qwen-code/issues/11119)** — New and serious: a CI-polling background shell keeps running, but after its starting turn ends, output and wake notifications vanish, wedging the session. Central to the background-automation roadmap; needs design discussion.
- **[#10780 — [P1, closed] reconnect floods the shared transport with full history replay and command snapshot](https://github.com/QwenLM/qwen-code/issues/10780)** — On long-lived daemon sessions, every reconnect pushed multi-megabyte replays through the bounded NDJSON transport, stalling unrelated sessions. Closed this cycle — an important scalability win for `qwen serve`.
- **[#10710 — [closed] reloading a session whose turn was killed mid-flight hides already-persisted assistant messages](https://github.com/QwenLM/qwen-code/issues/10710)** — When an ACP channel tears down mid-turn (e.g. the `ndjson_queue_limit_exceeded` case), persisted assistant output becomes invisible on session reload because no terminal event was emitted. Illustrates the daemon's edge-case handling improving.
- **[#7771 — persisted mcp_config is not loaded into main-process MCP proxy at startup](https://github.com/QwenLM/qwen-code/issues/7771)** — Open since July 26; users restarting the desktop app lose MCP servers until a manual reload. Finally has a fix in review: [#11145](https://github.com/QwenLM/qwen-code/pull/11145).
- **[#9704 — tool result write delay causes transient "Tool result missing from saved history"](https://github.com/QwenLM/qwen-code/issues/9704)** — Loading a live transcript during tool execution can show a scary placeholder even though the result is eventually persisted. Addressed by [#11144](https://github.com/QwenLM/qwen-code/pull/11144), which barriers disk reads behind in-flight writes.
- **[#10989 — daemon prompt authority is only polled where the sidebar is mounted, so the spinner fix is inert in the VS Code companion](https://github.com/QwenLM/qwen-code/issues/10989)** — The #9487 loading-indicator fix is grounded in live daemon prompt state, but that state is only fetched by a sidebar-mounted hook — leaving the VS Code companion surface showing stale indicators. A cross-surface architecture gap.
- **[#11141 — ACP in IntelliJ IDEA 26.1.1 on Windows cannot display or answer questions](https://github.com/QwenLM/qwen-code/issues/11141)** — Chinese-language report using Qwen Code 0.23.0 / glm-5.3. Three comments and growing; highlights remaining ACP parity issues on Windows IDEs.
- **[#11109 — release.yml repeats work the same run already did, and one 20-minute step verifies nothing](https://github.com/QwenLM/qwen-code/issues/11109)** — Two release runs timed out today. Combined with the three "Release Failed" bot issues, this captures the community's frustration with a flaky, over-long release pipeline.

## 4. Key PR Progress

- **[#11086 — feat(serve): scope extensions to workspace runtimes](https://github.com/QwenLM/qwen-code/pull/11086)** — Reconciles the global extension catalog into live workspace runtimes, exposing workspace-qualified daemon/SDK access and updating extension management, the composer add menu, and `@`-mentions. A significant architectural step for multi-workspace isolation.
- **[#11003 — delegate a subagent turn to an external agent over ACP (Claude Code first)](https://github.com/QwenLM/qwen-code/pull/11003)** — Lets a subagent definition declare an `executor` command so the turn runs in an external coding agent over ACP, with all events re-published as the subagent's own. Opens up a genuinely new extension model.
- **[#11015 — implement named-session worktree reset (Part 4B)](https://github.com/QwenLM/qwen-code/pull/11015)** — `/clear`, `/new`, and `/reset` now operate on a selected worktree-isolated task, giving it a fresh conversation while preserving its daemon-attested worktree, files, and branch.
- **[#11054 — headless global turn navigation (Phase 2A data layer)](https://github.com/QwenLM/qwen-code/pull/11054)** — Adds bounded turn metadata and historical transcript caches, exact live/persisted locators, and prompt reconciliation as a headless Web Shell layer — groundwork for a later virtual turn rail.
- **[#11053 — global turn navigation Phase 2 client data layer](https://github.com/QwenLM/qwen-code/pull/11053)** — Companion to #11054: the Web Shell now records which transcript slice each admitted fetch produced and treats unloaded ranges as explicit values, fixing the data-model gaps in the current transcript window.
- **[#10938 — make Session Workflow dependencies navigable and quiet its chrome](https://github.com/QwenLM/qwen-code/pull/10938)** — Redesign pass over the plan DAG: steps lead with the step name rather than status, dependency navigation is added, and inspector chrome is toned down.
- **[#9466 — anchor rewind mapping to stable prompt identity](https://github.com/QwenLM/qwen-code/pull/9466)** — Rewind no longer relies on positional turn order; it resolves targets through persisted prompt identity, surviving renumbering on session resume and headless `-p --rewind` paths.
- **[#11145 — fix(serve): load persisted MCP config after ACP preheat at startup](https://github.com/QwenLM/qwen-code/pull/11145)** — Directly targets #7771: calls `reconcileMcpConfiguration()` immediately after preheat so persisted MCP servers are live at daemon startup instead of after manual reload.
- **[#11144 — fix(cli): barrier live transcript reads behind tool-result writes](https://github.com/QwenLM/qwen-code/pull/11144)** — Fixes the #9704 race by gating `sessionTranscript` disk reads behind in-flight tool-result writes and routing restore cleanup through the shared `finalizeDanglingForRestore` helper.
- **[#11094 — test(integration): deflake the /compress E2E event budget](https://github.com/QwenLM/qwen-code/pull/11094)** — Stops the interactive chat-compression suite from scoring slow-but-correct compressions as failures by disabling the background memory extractor and widening the telemetry wait.

## 5. Feature Request Trends

- **Slim export artifacts.** The clearest directional signal: stop inlining the full Web Shell/React/daemon runtime into each exported HTML file. Requests cluster around externalizing the renderer to a pinned CDN and removing mermaid and daemon hook code from the transcript entry ([#11031](https://github.com/QwenLM/qwen-code/issues/11031), [#11091](https://github.com/QwenLM/qwen-code/issues/11091), [#11100](https://github.com/QwenLM/qwen-code/issues/11100), [#11142](https://github.com/QwenLM/qwen-code/issues/11142)).
- **Global turn navigation and workflow visibility.** Two PRs landing the Phase 2 data layers for session-wide turn navigation ([#11054](https://github.com/QwenLM/qwen-code/pull/11054), [#11053](https://github.com/QwenLM/qwen-code/pull/11053)), plus navigable Session Workflow DAGs ([#10938](https://github.com/QwenLM/qwen-code/pull/10938)) and the triple-projection perf finding ([#10865](https://github.com/QwenLM/qwen-code/issues/10865)), point to richer session inspection as a major direction.
- **Workspace-scoped and named sessions.** Extension catalogs scoped to workspace runtimes ([#11086](https://github.com/QwenLM/qwen-code/pull/11086)), named-session worktree resets ([#11015](https://github.com/QwenLM/qwen-code/pull/11015)), and the orphan-reap worktree cleanup request ([#11024](https://github.com/QwenLM/qwen-code/issues/11024)) show demand for stronger isolation and lifecycle control per workspace/session.
- **Background automation on the daemon.** The wedge bug [#11119](https://github.com/QwenLM/qwen-code/issues/11119) and the external-agent delegation design ([#11003](https://github.com/QwenLM/qwen-code/pull/11003)) both push toward long-running, daemon-hosted work that must survive turn boundaries.
- **MCP/ACP integration robustness.** Persisted MCP config should load on startup ([#7771](https://github.com/QwenLM/qwen-code/issues/7771)); ACP clients on Windows IDEs need to actually render and answer ([#11141](https://github.com/QwenLM/qwen-code/issues/11141)).

## 6. Developer Pain Points

- **Release and CI instability.** Three "Release Failed" bot issues today alone ([#11098](https://github.com/QwenLM/qwen-code/issues/11098), [#11114](https://github.com/QwenLM/qwen-code/issues/11114), [#11138](https://github.com/QwenLM/qwen-code/issues/11138)); two release runs timed out, with a step that spends 20 minutes verifying nothing ([#11109](https://github.com/QwenLM/qwen-code/issues/11109)); and release validation shares a runner host with PR CI, causing contention ([#10879](https://github.com/QwenLM/qwen-code/issues/10879)). The release host pins and bot retry loops are themselves becoming recurring noise.
- **Test infrastructure flakiness.** `vi.waitFor`'s hardcoded 1s default is a developer-machine figure — and 2,047 call sites inherit it ([#10892](https://github.com/QwenLM/qwen-code/issues/10892)). Add starved vitest worker RPCs ([#11103](https://github.com/QwenLM/qwen-code/pull/11103)), rasterization-sensitive capture assertions ([#10758](https://github.com/QwenLM/qwen-code/pull/10758)), and macOS E2E shard deaths ([#11134](https://github.com/QwenLM/qwen-code/pull/11134)), and the maintainer cost is clear.
- **Concurrency races in the core/serve layer.** Recurring pattern: async writes and reads racing each other — tool-result writes vs. transcript reads ([#9704](https://github.com/QwenLM/qwen-code/issues/9704)), pre-aborted tool requests queued behind an unrelated active batch ([#11146](https://github.com/QwenLM/qwen-code/issues/11146)), killed turns with no terminal event ([#10710](https://github.com/QwenLM/qwen-code/issues/10710)), and reconnect floods tearing down unrelated sessions ([#10780](https://github.com/QwenLM/qwen-code/issues/10780)).
- **Export size is a user-visible tax.** A 19.5 MB HTML file for an empty session ([#11031](https://github.com/QwenLM/qwen-code/issues/11031)) plus 6 MB of mermaid ([#11091](https://github.com/QwenLM/qwen-code/issues/11091)) makes transcript sharing slow and painful — and the fix is taking multiple rounds because the daemon React runtime is deeply embedded ([#11100](https://github.com/QwenLM/qwen-code/issues/11100)).
- **Cross-surface UI inconsistency.** Behavior fixed in the web shell doesn't always reach the VS Code companion ([#10989](https://github.com/QwenLM/qwen-code/issues/10989)), and basic editor interactions like `Cmd+A` still misbehave in the Web Shell composer ([#11108](https://github.com/QwenLM/qwen-code/issues/11108)).

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI / CodeWhale Community Digest — 2026-09-06

## Today's Highlights

The project’s public identity has shifted to **CodeWhale**, with v0.9.12 shipping as the first release under the new branding. After internal dogfooding, maintainers are prioritizing Windows fidelity, visible startup/progress status, and safer release automation. Key PRs today address Windows computer-use false-successes, CRLF overwrite bugs, and stale MCP “connecting” states.

## Releases

- [v0.9.12](https://github.com/Hmbown/Codewhale/releases/tag/v0.9.12) — Release notes identify **Codewhale** as the public product from Shannon Labs. The `codewhale` command, npm package, and release-asset names remain lowercase technical identifiers. The legacy npm package `deepseek-tui` is deprecated and receives no further releases, so v0.8.x users need to migrate to the new package/binary.

## Hot Issues

Selected 10 noteworthy issues from the 25 updated in the last 24h:

- [#5573 — v0.9.12 milestone tracker](https://github.com/Hmbown/Codewhale/issues/5573) — The “start here” coordination issue for release work; contains operator handoff notes, PR gates, and slice ordering. 24 comments, actively maintained.
- [#5620 — Context pressure warning is transient; agent does not react proactively](https://github.com/Hmbown/Codewhale/issues/5620) — Medium severity because context degradation happens silently and defeats a safety signal. 12 comments.
- [#5769 — Network errors sometimes cause the engine to stop](https://github.com/Hmbown/Codewhale/issues/5769) — Closed, but a useful signal that engine-level network failure handling still needs hardening. 5 comments.
- [#5820 — Ollama input budget collapses to 1024 tokens on 32K models](https://github.com/Hmbown/Codewhale/issues/5820) — Closed issue where the default 64K output reservation clamped the input window. Important for local-model users.
- [#5887 — MCP startup can stay on “20 connecting” for a long time](https://github.com/Hmbown/Codewhale/issues/5887) — Founder dogfooding report; users cannot tell whether startup is progressing or stalled.
- [#5901 — Custom theme overlays are missing from the `/theme` picker](https://github.com/Hmbown/Codewhale/issues/5901) — Users can configure custom themes, but the picker only shows compiled themes. 3 comments.
- [#5904 — Web fetch: JS-shell 200s fail extraction with no retry](https://github.com/Hmbown/Codewhale/issues/5904) — Cache-state-dependent fetch failures make agent reports misleading; no retry or browser escalation exists.
- [#5906 — Parked/cancelled agents hold write claims indefinitely](https://github.com/Hmbown/Codewhale/issues/5906) — Blocks sibling write-capable agents and prevents the same worktree path from being used again.
- [#5908 — Windows computer-use reports success after PowerShell failure](https://github.com/Hmbown/Codewhale/issues/5908) — Also includes `left_mouse_down` dropping the actual press. Multiple independent root causes documented.
- [#5909 — `write_file` silently converts CRLF files to LF on overwrite](https://github.com/Hmbown/Codewhale/issues/5909) — Inconsistent with `edit_file`, which preserves the existing line-ending style.

## Key PR Progress

Selected 10 important PRs from the last 24h:

- [#5897 — fix(mcp): show startup progress as each server connects](https://github.com/Hmbown/Codewhale/pull/5897) — Consumes MCP connection tasks as they complete so “20 connecting” is not held until the slowest server finishes.
- [#5903 — fix(computer-use): win32 backend reports PowerShell failures truthfully](https://github.com/Hmbown/Codewhale/pull/5903) — Fixes false-success reports by ensuring the User32 P/Invoke type is loaded in every action process.
- [#5911 — fix(tools): `write_file` preserves CRLF line endings](https://github.com/Hmbown/Codewhale/pull/5911) — Fixes #5909 and aligns `write_file` behavior with `edit_file`.
- [#5899 — fix(version): show published Cargo sources without the dev marker](https://github.com/Hmbown/Codewhale/pull/5899) — Installed crates.io packages should no longer report `codewhale 0.9.12 (dev)`.
- [#5905 — feat(tui): prioritize the Fleet menu surface](https://github.com/Hmbown/Codewhale/pull/5905) — Reduces `/fleet` usage line from 14 verbs to 5; less-used commands move one level deeper into `/fleet help`.
- [#5907 — feat(tui): list custom themes in picker](https://github.com/Hmbown/Codewhale/pull/5907) — Implements #5901 by scanning `$CODEWHALE_HOME/themes/` and appending custom overlay rows to the `/theme` picker.
- [#5902 — refactor(tui): adopt command shapes in session lifecycle slice](https://github.com/Hmbown/Codewhale/pull/5902) — FEAT-023 work under EPIC-005; converts commands like `/branch`, `/compact`, `/fork`, `/load`, `/new`, `/tree` to portable command shapes.
- [#5900 — fix: align shell guidance with execution](https://github.com/Hmbown/Codewhale/pull/5900) — Prevents models from assuming Bash syntax on PowerShell/cmd hosts by deriving guidance from the actual `ShellDispatcher`.
- [#5893 — fix(release): verify all crate tarballs before the first upload](https://github.com/Hmbown/Codewhale/pull/5893) — Runs one Cargo publication dry-run across all release crates so a broken TUI tarball cannot be hidden until after partial publishing.
- [#5895 — fix(computer-use): scope HarmonyOS cleanup to owned temporary files](https://github.com/Hmbown/Codewhale/pull/5895) — Fixes unsafe recursive deletion of `os.tmpdir()` parents after HarmonyOS file reads.

## Feature Request Trends

- **Session/ACP interoperability** — Users want ACP clients to enumerate/resume sessions and see active modes/models/config options: [#5863](https://github.com/Hmbown/Codewhale/issues/5863), [#5864](https://github.com/Hmbown/Codewhale/issues/5864).
- **More configuration surfaces in the TUI** — Custom theme overlays should appear in pickers [#5901](https://github.com/Hmbown/Codewhale/issues/5901), reasoning-only retry behavior should be configurable [#5867](https://github.com/Hmbown/Codewhale/pull/5867), and model effort picker rows need broader coverage [#5853](https://github.com/Hmbown/Codewhale/issues/5853).
- **Localization and non-English input support** — Chinese IME compatibility remains an open issue [#2323](https://github.com/Hmbown/Codewhale/issues/2323), and there is demand for localized Chinese documentation [#5482](https://github.com/Hmbown/Codewhale/issues/5482).
- **Clearer progress/status visibility** — MCP connection progress [#5887](https://github.com/Hmbown/Codewhale/issues/5887), Fleet menu simplification [#5888](https://github.com/Hmbown/Codewhale/issues/5888), and transparent model-catalog fallback [#5849](https://github.com/Hmbown/Codewhale/issues/5849).
- **Voice input** — Users want on-device speech recognition as the default, with API-key fallback and keyboard binding: [#5846](https://github.com/Hmbown/Codewhale/issues/5846).

## Developer Pain Points

- **Windows behavior inconsistencies** — CRLF overwrite bugs [#5909](https://github.com/Hmbown/Codewhale/issues/5909), PowerShell/computer-use false-success reports [#5908](https://github.com/Hmbown/Codewhale/issues/5908), and Bash-syntax assumptions on Windows shells [#5900](https://github.com/Hmbown/Codewhale/pull/5900).
- **Silent failure / misleading status** — Engines stopping on network errors [#5769](https://github.com/Hmbown/Codewhale/issues/5769), JS-rendered pages reported as unfetchable [#5904](https://github.com/Hmbown/Codewhale/issues/5904), and transient context-pressure warnings [#5620](https://github.com/Hmbown/Codewhale/issues/5620).
- **Resource cleanup and lock release** — Parked agents holding write claims [#5906](https://github.com/Hmbown/Codewhale/issues/5906), and temp-file cleanup deleting unrelated files on HarmonyOS [#5894](https://github.com/Hmbown/Codewhale/issues/5894).
- **Release and CI reliability** — Broken crate tarballs reaching publication [#5892](https://github.com/Hmbown/Codewhale/issues/5892), published packages mislabeled as `dev` [#5891](https://github.com/Hmbown/Codewhale/issues/5891), and flaky Windows worker-idle-timeout tests [#5898](https://github.com/Hmbown/Codewhale/issues/5898).

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*