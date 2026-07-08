# OpenClaw Ecosystem Digest 2026-07-09

> Issues: 500 | PRs: 500 | Projects covered: 13 | Generated: 2026-07-08 17:22 UTC

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

Here is the OpenClaw project digest for **2026-07-09**.

---

### 1. Today's Overview

The OpenClaw project is exhibiting **extremely high activity** today, with 500 issues and 500 pull requests updated in the last 24 hours. However, this activity is primarily concentrated in **issue triage and discussion**, as 444 of those issues remain open and 382 PRs are still pending. The project has **zero new releases today**, and while the volume of changes is massive, the bottleneck appears to be on **maintainer review and merging**, with only 118 PRs being merged or closed. The backlog is heavily weighted toward high-priority, security-related, and regressive bugs, suggesting the project is in a **stabilization and hardening phase** following a period of rapid feature development.

### 2. Releases

No new releases were published today (2026-07-09). The latest available release remains from a prior date.

### 3. Project Progress

Today, **118 pull requests were merged or closed**, indicating significant progress despite the high number of open items. Key advancements include:

- **CI & Infrastructure:** A temporary fix was merged to disable a flaky QA smoke lane while it is being refactored ([PR #102256](https://openclaw/openclaw/pr/102256)).
- **Bug Fixes (Merged/Closed):** Significant fixes are progressing through the pipeline for:
    - **Cron jobs:** Preventing double dispatch after re-enabling a cron job while a run is in-flight ([PR #102255](https://openclaw/openclaw/pr/102255)).
    - **Telegram:** Stripping Markdown formatting during conversation context dedup to prevent duplicate messages ([PR #102259](https://openclaw/openclaw/pr/102259)).
    - **Provider Errors:** Avoiding retry loops on permanent provider errors for cron agent jobs ([PR #102265](https://openclaw/openclaw/pr/102265)).
- **New Features (Proposed):** Several high-impact PRs were proposed and are ready for review:
    - A **Signal setup wizard** to guide users through a proven transport setup ([PR #100906](https://openclaw/openclaw/pr/100906)).
    - A **GitHub Enterprise Copilot support** for data-residency auth ([PR #99221](https://openclaw/openclaw/pr/99221)).
    - **Interactive parity with Codex runtime**, adding structured "ask user a question," "plan mode," and "goal mode" to all OpenClaw sessions ([PR #102261](https://openclaw/openclaw/pr/102261)).

### 4. Community Hot Topics

The most active discussions today highlight user frustration with core reliability and critical security issues.

- **Security: Text Leaks & Prompt Injection:** The most commented issue, [#25592](https://openclaw/openclaw/issue/25592) (35 comments), details how text produced between tool calls leaks to messaging channels. This is a critical UX and potential security flaw. The related issue [#11829](https://openclaw/openclaw/issue/11829) (Security Roadmap: Protecting API Keys from Agent Access) remains a hot topic with 20 comments, showing deep community concern over secret management.
- **Race Conditions & Crashes:** Issue [#22676](https://openclaw/openclaw/issue/22676) (17 comments) describes a severe race condition in the Signal daemon that creates orphaned processes and send failures during restarts.
- **Performance Regression:** Issue [#85333](https://openclaw/openclaw/issue/85333) (15 comments) reports a massive 4-5x performance regression in `openclaw doctor --fix`, causing session snapshot path traversal bottlenecks.
- **Multi-Agent Instability:** Issue [#43367](https://openclaw/openclaw/issue/43367) (13 comments) highlights the core instability of multi-agent orchestration with concurrent agent config overwrites, session-lock failures, and detached child work.

### 5. Bugs & Stability

Today’s data reveals a **serious stability crisis**, with numerous **P1 and P0 regressions** and security bugs. The most critical issues are:

**Critical (P0):**
- **Session Hang on Compaction Timeout ([#43661](https://openclaw/openclaw/issue/43661)):** A P0 bug where a session hangs indefinitely when compaction times out, causing repeated duplicate message sends. This is a release blocker with high impact.
- **Codex GitHub Enterprise Auth ([#99221](https://openclaw/openclaw/pull/99221)):** A P0 PR fixing a critical compatibility/auth issue for GitHub Enterprise users.

**High Severity (P1):**
- **Signal Daemon Race Condition ([#22676](https://openclaw/openclaw/issue/22676)):** Orphaned processes and failures on restart.
- **`exec` Tool Env Var Inheritance ([#31583](https://openclaw/openclaw/issue/31583)):** A regression where `skills.entries.*.env` variables are not passed to subprocesses, breaking secret injection.
- **Write Tool Overwrite Mode ([#40001](https://openclaw/openclaw/issue/40001)):** Isolated cron sessions overwrite shared files instead of appending, causing data loss.
- **Sandbox `workspaceAccess: none` Read-Only ([#37634](https://openclaw/openclaw/issue/37634)):** The isolated workspace is mounted read-only, breaking tools that need to write.
- **Cron Silent Timeout ([#45494](https://openclaw/openclaw/issue/45494)):** Jobs silently time out during LLM API outages instead of fast-failing.

Several of these bugs have linked fix PRs in progress, including [#101928](https://openclaw/openclaw/pr/101928) (fixing re-entrant session write lock to resolve compaction hangs) and [#102255](https://openclaw/openclaw/pr/102255) (fixing cron double dispatch).

### 6. Feature Requests & Roadmap Signals

User requests are heavily focused on **control, safety, and extensibility**:

- **Sandbox & Resource Control:** High-demand requests include per-agent cost budgets ([#42475](https://openclaw/openclaw/issue/42475), 12 comments, 1 👍), ability to allow private network access for `web_fetch` ([#39604](https://openclaw/openclaw/issue/39604), 13 comments, 11 👍), and an append mode for the `write` tool ([#40001](https://openclaw/openclaw/issue/40001)).
- **Lifecycle Hooks:** A strong signal exists for extensibility hooks, such as `post_subagent_complete` ([#22358](https://openclaw/openclaw/issue/22358)) and general gateway lifecycle hooks ([#43454](https://openclaw/openclaw/issue/43454)). These would allow for automated workflows and trajectory logging.
- **Multi-Agent Enhancement:** Users are calling for major architectural changes to multi-agent collaboration, including capability profiling and shared blackboards ([#35203](https://openclaw/openclaw/issue/35203), 10 comments).
- **Predictions:** Given the volume of feedback and the P1 status, **per-agent cost budgets** and the **write tool append mode** are strong candidates for the next minor release. The policy-based gateway repairs in [PR #99776](https://openclaw/openclaw/pr/99776) and the new **interactive parity with Codex Runtime** ([PR #102261](https://openclaw/openclaw/pr/102261)) are likely to be merged soon.

### 7. User Feedback Summary

The user feedback today paints a picture of **frustration with instability and a desire for production-ready controls**.

- **Pain Points:**
    - **Reliability:** Users are experiencing crashes (race conditions, compaction timeouts), silent data loss (write tool overwriting), and regressions that break core workflows (env var inheritance).
    - **Safety:** There is significant anxiety about security, with users reporting message leaks, prompt injection vulnerabilities, and no way to enforce cost or access controls.
    - **Performance:** The 4-5x regression in `doctor --fix` is a major friction point for users attempting to maintain their systems.
- **Use Cases:**
    - **Development & Coding:** Bugs in the `write` tool, `exec` tool, and sandbox isolation are directly impacting developers using OpenClaw for automated code generation and testing.
    - **Enterprise & Security:** Users are demanding the tools to run OpenClaw in regulated environments, including cost governance, private network controls, and secure secret injection.
- **Satisfaction:** Users are clearly engaged and invested, providing detailed bug reports and feature requests. However, the sheer number of regressions ("worked before, now fails") suggests **declining user satisfaction** with the stability of recent releases.

### 8. Backlog Watch

Several high-impact items are languishing due to the need for maintainer or product decisions:

- **Bootstrap File Ignorance ([#29387](https://openclaw/openclaw/issue/29387)):** A P1 bug where per-agent bootstrap files (SOUL.md, AGENTS.md) are silently ignored. Has 14 comments and 5 👍 but is stuck waiting for decisions.
- **`gh-issues` Prompt Injection ([#45740](https://openclaw/openclaw/issue/45740)):** A critical security P1 bug with no linked fix PR. The issue body is injected directly into sub-agent prompts. Stalled on security review and product decision.
- **Discord Tool-Call Leaks ([#44905](https://openclaw/openclaw/issue/44905)):** A P1 bug where Discord channels leak raw tool-call JSON. Needs a live repro and product decision.
- **Memory Architecture Chaos ([#43747](https://openclaw/openclaw/issue/43747)):** A user report of completely inconsistent memory management behavior across colleagues, suggesting a deep architectural issue. Needs product decision.
- **Slack Progress Chrome ([#102082](https://openclaw/openclaw/pr/102082)):** A fix PR for suppressing annoying tool-progress messages on Slack is waiting for proof.

These items, particularly the security-related ones, represent significant risk if allowed to remain unattended.

---

## Cross-Ecosystem Comparison

# Cross-Project Ecosystem Comparison Report
**Date: 2026-07-09 | AI Agent Open-Source Landscape**

---

## 1. Ecosystem Overview

The personal AI assistant open-source ecosystem is experiencing an intense **stabilization phase** following a period of rapid feature expansion. Across the major projects, the dominant theme is **reliability over novelty**—communities are demanding production-grade security, predictable context management, and robust multi-agent orchestration rather than new capabilities. Security vulnerabilities are being discovered and patched at an accelerating rate (ZeroClaw, NanoBot, OpenClaw all had critical security fixes this week), signaling maturation of both the codebase and the security researcher community around these tools. A clear **architectural divergence** is emerging: while reference implementations like OpenClaw prioritize platform stability and enterprise controls, newer entrants like NanoBot and CoPaw are aggressively iterating on developer experience and novel interaction patterns. The ecosystem is fragmenting around **provider abstraction layers**, with each project handling multi-LLM support, context windows, and tool-calling differently—a sign that no dominant architecture has yet emerged.

---

## 2. Activity Comparison

| Project | Issues Updated (24h) | PRs Updated (24h) | Merged/Closed PRs | Release Status | Health Score* |
|---------|---------------------|-------------------|-------------------|----------------|---------------|
| **OpenClaw** | 500 | 500 | 118 | ❌ No release | 🟡 **Stabilizing** (high volume, low merge rate) |
| **ZeroClaw** | 50 | 50 | 14 | ❌ No release | 🟡 **Architectural churn** (major refactors in flight) |
| **Hermes Agent** | 50 | 50 | 6 | ✅ v0.18.2 (Jul 7) | 🟢 **Active** (rapid patches, feature work) |
| **CoPaw** | 38 | 44 | 15 | ✅ v2.0.0-beta.4 | 🟢 **High velocity** (beta iteration) |
| **NanoBot** | 9 | 27 | 10 | ❌ No release | 🟢 **Healthy** (strong security response) |
| **IronClaw** | 24 | 50 | 16 | ❌ No release (PR open) | 🟡 **Busy but buggy** (mid-refactor) |
| **LobsterAI** | 3 | 13 | 10 | ❌ No release | 🟢 **Maintenance mode** (low community friction) |
| **PicoClaw** | 6 | 3 | 4 | ❌ v0.2.9 stable | 🟢 **Moderate** (responsive maintainers) |
| **NanoClaw** | 1 | 38 | 11 | ❌ No release | 🟢 **Internal sprint** (team-focused) |
| **NullClaw** | 0 | 0 | 0 | ❌ No release | ⚪ **Inactive** |
| **TinyClaw** | 0 | 0 | 0 | ❌ No release | ⚪ **Inactive** |
| **Moltis** | 0 | 0 | 0 | ❌ No release | ⚪ **Inactive** |
| **ZeptoClaw** | 0 | 0 | 0 | ❌ No release | ⚪ **Inactive** |

*Health Score: 🟢 Active/Healthy | 🟡 Concerned/Transitional | 🔴 Critical | ⚪ Inactive*

---

## 3. OpenClaw's Position

**Advantages:**
- **Scale:** With 500 issues and PRs updated daily, OpenClaw has the largest community and ecosystem in the cohort. Its backlog size (444 open issues, 382 open PRs) reflects an order-of-magnitude larger userbase than any peer.
- **Maturity:** OpenClaw is the only project actively tackling **enterprise governance** (per-agent cost budgets, API key protection, policy-based gateways), positioning it for regulated deployments.
- **Platform breadth:** Signal setup wizard, GitHub Enterprise Copilot support, and Codex runtime parity indicate a **vertical integration strategy** spanning messaging, DevOps, and IDE workflows.

**Technical Approach Differences:**
- **Session architecture:** OpenClaw's session compaction and cron job handling are more sophisticated than peers—but also more fragile, as evidenced by the P0 compaction timeout hang (Issue #43661).
- **Security posture:** OpenClaw's prompt injection and tool-call leak issues (Issue #25592, #44905) suggest a **less aggressive security stance** than NanoBot, which patched three CVEs within 24 hours.
- **Monorepo vs modular:** Unlike IronClaw's crate-level modularization or ZeroClaw's WASM plugin vision, OpenClaw remains a monolithic reference implementation, which may hinder community contribution velocity.

**Community Size Comparison:**
- OpenClaw's daily activity (1,000+ GH events) dwarfs the next most active project (ZeroClaw at ~100 events). However, its **merge rate is only 23.6%** (118/500), compared to NanoBot's 37% (10/27) and PicoClaw's 100% (4/4). This suggests a **review bottleneck** that frustrates contributors.

---

## 4. Shared Technical Focus Areas

The following requirements are emerging across multiple projects, indicating industry consensus on pain points:

| Focus Area | Projects + Specific Needs |
|------------|--------------------------|
| **Security & Secret Management** | **OpenClaw** (#11829: API key protection), **NanoBot** (#4825-4827: token minting), **CoPaw** (#5866: rm bypass), **ZeroClaw** (#8781: stale advisories) — All projects are hardening authentication and secret isolation. |
| **Context Window & Memory Reliability** | **OpenClaw** (#43661: compaction hang), **CoPaw** (#5860: infinite loops), **ZeroClaw** (#5808: 32k budget overflow), **Hermes Agent** (#55578: session resurrection) — Context management is the #1 stability challenge. |
| **Multi-Agent Orchestration** | **OpenClaw** (#43367: agent overwrites), **LobsterAI** (#2285: subagent delegation), **ZeroClaw** (#7218: A2A discovery), **CoPaw** (#5139: swarm collaboration) — The ecosystem is converging on standardized delegation patterns. |
| **Provider Abstraction Layer** | **ZeroClaw** (#5937/#8854: unified providers), **OpenClaw** (#99221: GitHub Enterprise auth), **IronClaw** (#5844: compute-vs-think prompting) — Each project is building custom provider middleware, a sign of architectural divergence. |
| **Sandbox/Resource Control** | **OpenClaw** (#37634: workspace read-only), **ZeroClaw** (#8731: zombie MCP processes), **CoPaw** (#5864: approval levels) — Isolation and resource governance are table stakes for production use. |
| **Observability & Debugging** | **IronClaw** (#5837: run inspection), **OpenClaw** (#85333: doctor performance), **CoPaw** (#5852: notification sounds) — Users want better visibility into agent behavior and failure modes. |

---

## 5. Differentiation Analysis

| Dimension | OpenClaw | Hermes Agent | ZeroClaw | CoPaw | NanoBot | IronClaw |
|-----------|----------|--------------|----------|-------|---------|----------|
| **Target User** | Enterprise / power user | Developer / DevOps | Tinkerer / architect | General / IM user | Developer / security-conscious | Enterprise / automation |
| **Deployment Model** | Self-hosted monolith | Docker / Gateway | Native daemon + TUI | Cloud + Docker | CLI + WebUI | Crate + extensions |
| **Architecture** | Monolithic reference | Modular (releases) | WASM plugin (future) | 2.0 beta rewrite | Clean modular | Crate-based modular |
| **LLM Provider Strategy** | Built-in support | Adapter layer | Unified provider (WIP) | Multi-provider | Multi-provider | Provider-agnostic (tools-first) |
| **Multi-Agent Support** | Cron + subagent | Session-scoped | A2A discovery (RFC) | Cowork + delegation | Not core focus | Not core focus |
| **Community Engagement** | High volume, low merge rate | Rapid patches | Deep RFC discussions | Beta feedback loops | Responsive security | Internal sprint |
| **Differentiating Feature** | Platform completeness | Release velocity | Plugin extensibility | 2.0 beta innovation | Security rigor | Composition refactor |
| **Weakness** | Review bottleneck, regressions | Post-release bugs | S1 bugs linger | Beta instability | Small community | Buggy integrations |

---

## 6. Community Momentum & Maturity

**Tier 1: Rapid Iteration (beta/alpha phase)**
- **CoPaw** — v2.0 beta series with aggressive iteration (15 PRs/day). Highest risk/reward: exciting features (swarm, desktop automation) but core context management is unstable.
- **ZeroClaw** — Major architectural transformation (WASM plugins, provider rewrite). Momentum is high but S1 bugs (32k context overflow) remain unpatched for months.

**Tier 2: Stabilization & Hardening**
- **OpenClaw** — The 500-issue backlog is a double-edged sword: massive community engagement but the 23.6% merge rate signals a **bottleneck that needs maintainer scaling**. The P0 compaction timeout and P1 security leaks must be resolved to maintain trust.
- **Hermes Agent** — Rapid patch releases (v0.18.1 → v0.18.2 in one day) show strong incident response, but the volume of post-release bugs suggests inadequate pre-release testing.
- **NanoBot** — Best security response in the cohort (3 CVEs fixed in 24h). Smallest active community but highest code quality signal.

**Tier 3: Maintenance & Sprint Cycles**
- **IronClaw** — Mid-composition-refactor with breaking changes pending. Integration bugs (Slack, GitHub) impacting daily workflows.
- **LobsterAI** — Quiet maintenance; closed a major v4.1 startup loop bug. Low community friction but low innovation velocity.
- **PicoClaw** — Steady state with responsive maintainers; OAuth regressions need attention.

**Tier 4: Inactive**
- **NullClaw, TinyClaw, Moltis, ZeptoClaw** — No activity. Likely abandoned or seasonal projects.

---

## 7. Trend Signals

1. **Security is the new differentiator.** NanoBot's rapid CVE response and ZeroClaw's advisory cleanup signal that **trustworthiness is becoming a competitive advantage**. Projects that fail to address prompt injection, token leakage, and secret management will lose enterprise adoption.

2. **Context management is the hardest unsolved problem.** Every active project has at least one critical bug related to session state, compaction, or history pruning. **Deterministic replay and predictable context behavior** will be the key battleground for developer adoption in the next 6 months.

3. **Multi-agent orchestration is converging on delegation patterns.** OpenClaw's subagent delegation, LobsterAI's Cowork sessions, and ZeroClaw's A2A discovery all point toward a **standardized "agent-to-agent" protocol**. Early adopters of cross-platform agent communication (e.g., `.well-known/agent-card.json`) may define the industry standard.

4. **The "write tool" is a UX liability.** Multiple projects (OpenClaw #40001, IronClaw #5844, NanoBot #4828) are iterating on how agents interact with files. The default overwrite behavior causes silent data loss; **append mode and diff viewers** are emerging as table-stakes requirements.

5. **Enterprise controls are demanded, not optional.** Per-agent cost budgets, private network access controls, and admin-managed skills are being requested across OpenClaw, IronClaw, and ZeroClaw. **Governance is not a premium feature**—it's a prerequisite for production deployment.

6. **The best projects are listening to their bugs.** The projects with the highest user satisfaction (NanoBot, PicoClaw) are those that **patch reported issues within 24-48 hours**. The projects losing trust (OpenClaw's compaction hang, ZeroClaw's 3-month-old S1 bugs) are those where critical issues languish.

7. **Inactive projects are a signal of market consolidation.** With 4 of 13 projects showing zero activity, the ecosystem is **contracting toward a few dominant architectures**. Developers should evaluate project health (merge rate, security response, bug closure time) over raw GitHub stars when choosing a platform to build upon.

---

*Report generated from community digest data dated 2026-07-09. Analysis reflects relative project activity and may not capture private repository work or team-internal releases.*

---

## Peer Project Reports

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

# NanoBot Project Digest — 2026-07-09

## 1. Today's Overview

NanoBot shows **high activity** with 27 PRs and 9 issues updated in the last 24 hours. The project is in a **security consolidation phase** — three related security vulnerabilities (CVE-like localhost bootstrap token issuance) were reported and fixed within 48 hours, demonstrating strong incident response. Three merged PRs closed multiple security issues, while two enhancement PRs advanced the WebUI diff viewer and Slack dependency fixes. The backlog shows two open issues needing resolution (non-interactive config refresh, Matrix E2EE trust), and the overall velocity suggests a release may be forthcoming.

**Project health: Active, security-aware, maintainer-responsive.**

---

## 2. Releases

**No new releases today.** The last release date is unknown from the data.

---

## 3. Project Progress — Merged/Closed PRs Today (10 total)

### Security fixes (P1)
- **PR #4849** `fix(webui): gate bootstrap API token issuance` — Merged. Splits WebUI bootstrap WebSocket tokens from REST API tokens; only returns `api_token` after verifying `tokenIssueSecret` or static token. Closes issues #4825, #4826, #4827.
- **PR #4669** `fix: require api key for serve` — Merged. Requires configured API key before OpenAI-compatible API server starts; covers loopback startup rejection. Closes #4078 (unauthenticated `/v1/chat/completions` endpoint).

### Bug fixes
- **PR #4830** `Fix missing aiohttp slack dependency in pyproject.toml` — Merged. Adds `aiohttp>=3.9.0` to Slack dependencies. Closes #4829.
- **PR #4831** `fix(webui): keep prompt rail out of narrow chat columns` — Merged. CSS fix to prevent prompt rail overlap on narrow thread panes.

### Refactoring & documentation
- **PR #4848** `refactor(agent): extract turn hook assembly` — Merged. Moves per-turn hook assembly from `AgentLoop` into dedicated `turn_hooks` module; adds focused tests.
- **PR #4850** `docs: improve search entry pages` — Merged. Adds search-oriented capability section to README, plus docs/guides entry pages for chat apps, SDK, API, memory, WebUI, MCP, and gateway deployment.
- **PR #4852** `feat: non-interactive config refresh with 'nanobot onboard --refresh'` — Merged. Closes #4851. Adds `--refresh` flag to avoid interactive prompts for automated config updates.

### Other closed
- Other minor PRs closed (e.g., LangSmith docs clarification, CLI multiline input fix) without merging.

---

## 4. Community Hot Topics

### Most active issues
1. **#2463** `[CLOSED] Architectural issue: nanobot does not preserve the exact prompt prefix`  
   *(13 comments)*  
   [🔗](https://github.com/HKUDS/nanobot/issues/2463)  
   **Analysis:** Persistent architectural concern about conversation history fidelity. The prompt prefix sent to the model differs from persisted history, causing conflicts with OpenAI's prompt caching. **Underlying need:** Model-agnostic prompt caching support and deterministic replay of agent state.

2. **#4825** `[CLOSED] Unauthenticated localhost callers can mint WebUI API tokens`  
   *(3 comments)* + related #4826, #4827  
   [🔗](https://github.com/HKUDS/nanobot/issues/4825)  
   **Analysis:** Three tightly related security advisories were reported by the same researcher (YLChen-007) on 2026-07-07 and fixed within 24 hours. The bootstrap endpoint on loopback allowed any local process to obtain API bearer tokens without authentication. **Project responsiveness:** Excellent — all three closed with fix PR #4849 within a day.

### Most active pull requests
- **#4856** `fix(webui): restore localhost bootstrap API tokens` *(OPEN)* — Seeks to restore full localhost bootstrap while blocking remote unauthenticated access. Complementary to #4849 but still open for review.
- **#4828** `feat(webui): add file edit diff progress view` *(OPEN, P2)* — GitHub-like unified diffs for file edits in WebUI; per-user toggle between diff and summary views. Community interest high for developer-focused workflows.

---

## 5. Bugs & Stability

| Severity | Issue | Summary | Has Fix PR? |
|----------|-------|---------|-------------|
| **Critical** | #4825/4826/4827 | Unauthenticated localhost bootstrap token minting — any local process could get API tokens | ✅ #4849 (merged) |
| **High** | #4078 | OpenAI `/v1/chat/completions` accepted unauthenticated requests, skipped auth middleware | ✅ #4669 (merged) |
| **Medium** | #4829 | Missing `aiohttp` in Slack dependencies broke Slack plugin entirely | ✅ #4830 (merged) |
| **Medium** | #2450 | `minimax-m2.7` via Ollama Cloud fails on 2nd+ request (stale issue, closed without fix visible) | ❓ |
| **Low** | #4841 | Matrix bot device shows as 'untrusted' — cross-signing missing | ❌ Open issue |

### Regression risk
- **PR #4832** `fix(cli): handle CSI-u Shift+Enter` *(OPEN)* — Reports a regression from #4614 where removal of Shift+Enter handling left CSI escape sequences dumped on certain terminals. PR is small and in review.

---

## 6. Feature Requests & Roadmap Signals

### Likely for next version (based on merged PRs and open PRs with maintainer activity)
| Feature | Issue/PR | Probability |
|---------|----------|-------------|
| **Non-interactive `nanobot onboard --refresh`** | #4851 / #4852 | ✅ **Already merged** — will be in next release |
| **WebUI diff viewer for file edits** | #4828 (OPEN P2) | High — rich PR with maintainer as author |
| **RTK command rewriter for exec** | #4854 (OPEN P2) | Medium-High — new security layer for tool execution |
| **Guided channel setup flows** | #4855 (OPEN) | Medium — Feishu/WeChat Work/WhatsApp additions |
| **`nano_timer` core tool** | #4853 (OPEN P1) | Medium — requested for timezone calendar awareness |

### Longer-term roadmap signals
- **Matrix E2EE trust** (#4841) — Cross-signing/SAS verification is missing; community pain point for Matrix users.
- **Prompt prefix preservation** (#2463) — Architectural issue for OpenAI prompt caching compatibility; may require deeper rework.
- **LangSmith integration clarity** (#4847) — README claims working integration but users report breakage; documentation fix merged but no code fix.

---

## 7. User Feedback Summary

### Pain points expressed
- **Security concerns** — Three vulnerability reports in one day (bootstrap token issuance) show users are actively auditing the project. All were quickly resolved, which builds trust.
- **Dependency gaps** — Slack plugin unusable due to missing `aiohttp` dependency. Showed gap in testing CI for optional plugins.
- **Prompt caching conflicts** — `ronny-rentner` (#2463) detailed how persisted history mismatches actual prompts sent to OpenAI, breaking prompt caching savings. Advanced user diving deep into architectural details.
- **Matrix trust UX** — `orrinwitt` (#4841) reports frustration with untrusted bot device in Element clients, no path to resolve.

### Use cases observed
- **Development workflows** — PR #4828 (diff viewer) signals strong interest in using NanoBot for code editing with visual change tracking.
- **Automated deployments** — PR #4851/4852 (non-interactive refresh) was requested specifically for "systems configured to update themselves automatically or semi-automatically."
- **Enterprise security** — Several reporters appear to be security researchers or engineers probing deployment edge cases (loopback, unauthenticated endpoints).

### Satisfaction signals
- Multiple issues closed with "thank you" from reporters.
- No negative feedback about maintainer responsiveness observed.

---

## 8. Backlog Watch

### High-priority items needing attention

| Issue | Created | Days Open | Priority | Reason |
|-------|---------|-----------|----------|--------|
| **#2463** `Prompt prefix preservation` | 2026-03-25 | **106 days** | Architectural | Deep architectural issue blocking OpenAI prompt caching; 13 comments, no resolution. Affects cost optimization for all OpenAI users. |
| **#2450** `Ollama Cloud minimax-m2.7 fails on 2nd+ request` | 2026-03-24 | **107 days** | Bug | Closed without visible fix. May be stale/resolved elsewhere but users may still hit this. |
| **#4841** `Matrix bot device untrusted` | 2026-07-07 | **2 days** | Enhancement | New issue; no comments or maintainer response yet. Growing Matrix community may escalate. |
| **#2873** `Fix discord forwarded referenced messages` | 2026-04-06 | **94 days** | Bug | Open PR with test coverage, no maintainer review since April. Discord users losing forwarded message content. |

### Concerning signals
- **PR #2873** (Discord referenced messages) has been open for **3 months** with zero maintainer interaction despite having regression tests. Suggests Discord channel maintainer bandwidth may be limited.
- **Issue #2463** (prompt prefix) is the most commented issue in the entire dataset (13 comments) and represents a fundamental architectural tension between NanoBot's history model and OpenAI's API expectations. No assignee or milestone.

---

*Digest generated 2026-07-09. Data source: GitHub API for HKUDS/nanobot.*

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

Here is the project digest for **Hermes Agent** based on activity captured on **2026-07-09**.

---

## Hermes Agent Project Digest: 2026-07-09

### 1. Today's Overview
Project activity is **extremely high**, with **50 issues** and **50 pull requests** updated in the last 24 hours, signaling a major push toward stability and security. The development team released two patch versions (v0.18.1 and v0.18.2) on July 7, rolling up approximately 660 fixes and introducing a critical dependency fix for the WhatsApp platform. The community is heavily focused on bug reports related to session state corruption, cross-user security boundaries, and provider compatibility, while maintainers are responding with rapid security fixes.

### 2. Releases
Two tagged releases were identified in the data span (July 7, 2026):

- **v2026.7.7.2 (v0.18.2)** - [Release Link](https://github.com/nousresearch/hermes-agent/releases/tag/v2026.7.7.2)
    - **Date:** July 7, 2026 (same-day patch)
    - **Change:** Fixed a WhatsApp Baileys dependency issue by unpinning the library from a git commit and using the published version `7.0.0-rc13`.
    - **Impact:** Resolves build failures for Docker images and tagged releases.

- **v2026.7.7 (v0.18.1)** - [Release Link](https://github.com/nousresearch/hermes-agent/releases/tag/v2026.7.7)
    - **Date:** July 7, 2026
    - **Change:** Major patch release rolling up ~660 PRs merged since v0.18.0 (July 1).
    - **Impact:** Includes bug fixes, hardening, and in-progress feature work essential for downstream consumers.
    - **Note:** No breaking changes or specific migration steps were noted in the release notes.

### 3. Project Progress
In the last 24 hours, **6 pull requests were merged or closed**, indicating steady progress on bug fixing and features:

- **Security & Scoping:** Two critical security PRs were opened by `necoweb3` to scope `session_search` (PR [#60982](https://github.com/nousresearch/hermes-agent/pull/60982)) and `cron` jobs (PR [#61016](https://github.com/nousresearch/hermes-agent/pull/61016)) to the caller's own `user_id` in multi-user environments.
- **Platform Fixes:**
    - `fix(qqbot)` (PR [#56550](https://github.com/nousresearch/hermes-agent/pull/56550)): Accepted the `is_reconnect` flag in the QQBot adapter, fixing a `TypeError` on gateway startup.
    - `fix(dashboard)` (PR [#61026](https://github.com/nousresearch/hermes-agent/pull/61026)): Fixed the "Full backup" button by passing the `-o` flag correctly.
    - `fix(kanban)` (PR [#61035](https://github.com/nousresearch/hermes-agent/pull/61035)): Improved logic for surfacing active PR respawn guards.
- **Core Infrastructure:** A PR for `refactor(memory)` (PR [#60953](https://github.com/nousresearch/hermes-agent/pull/60953)) introduces parallel memory providers with prefetch lane isolation to reduce Time-To-First-Token (TTFT) latency.

### 4. Community Hot Topics
The following discussions are generating the most engagement:

- **#34390 - Dashboard Reverse Proxy Access** ([Issue](https://github.com/nousresearch/hermes-agent/issues/34390)): A feature request for `--allowed-hosts` flag for the dashboard. With 12 comments, this is the most discussed topic, driven by users wanting secure access via Tailscale and reverse proxies.
- **#6626 - Gemma 4 Tool Calling** ([Issue](https://github.com/nousresearch/hermes-agent/issues/6626)): A user is struggling to integrate Gemma 4 via vLLM with the `gemma4` parser. The 10 comments and 4 reactions indicate significant interest in supporting newer local models.
- **#53443 - QQBot Reconnect Bug** ([Issue](https://github.com/nousresearch/hermes-agent/issues/53443)): A high-severity bug preventing QQBot from starting. The 8 comments reflect a critical platform gap for Chinese market users.
- **#523 - Local Model Setup Skill** ([Issue](https://github.com/nousresearch/hermes-agent/issues/523)): Despite being the oldest issue in the list (created in March), it maintains 4 comments and 3 👍 reactions. This highlights a persistent need for better Ollama/llama.cpp/vLLM onboarding.

### 5. Bugs & Stability
Several high-severity bugs were reported or discussed today. Fix PRs are noted where applicable.

- **Critical (P1):**
    - **xAI OAuth Fallback (##32617)** ([Issue](https://github.com/nousresearch/hermes-agent/issues/32617)): On provider switch, stale encrypted content from xAI causes a 400 error. **No fix PR linked.**
    - **Model Switch Provider Mismatch (##47828)** ([Issue](https://github.com/nousresearch/hermes-agent/issues/47828)): Switching model via `/model` keeps the old provider's base URL, causing 400 errors. **No fix PR linked.**
    - **Fallback Bug Report (##60955)** ([Issue](https://github.com/nousresearch/hermes-agent/issues/60955)): A general fallback bug report. Attached file suggests session corruption.
    - **Security: Unscoped Cron Jobs (PR #61016)** ([PR](https://github.com/nousresearch/hermes-agent/pull/61016)): Fix PR open. Mitigates a serious vulnerability where any user can manage other users' cron jobs in a multi-user gateway.

- **High (P2):**
    - **OpenRouter `system` Kwarg (##60821)** ([Issue](https://github.com/nousresearch/hermes-agent/issues/60821)): Crashes on OpenAI-compatible endpoints. Duplicate reported (##61030). **No fix PR linked.**
    - **Voice Message Deadlock (##61008)** ([Issue](https://github.com/nousresearch/hermes-agent/issues/61008)): Mid-stream voice messages cause interrupt recursion and session hangs. **Fix PR #61027** is open to address this.
    - **Desktop Session Resurrection (##55578)** ([Issue](https://github.com/nousresearch/hermes-agent/issues/55578)): Async delegation completions revive old sessions. **No fix PR linked.**
    - **CLI Model Config (##25106)** ([Issue](https://github.com/nousresearch/hermes-agent/issues/25106)): `--global` model switch doesn't persist `base_url` and `api_mode`. **No fix PR linked.**

### 6. Feature Requests & Roadmap Signals
The following features are likely candidates for the next minor release based on community demand and existing PRs:

- **Executive v2 (PR #60549):** A default-off control loop with a provider adapter layer and session-state surface. This is a major architectural change currently being forward-ported.
- **Cloudflare Tunnel (PR #61034):** A new `hermes tunnel` command to expose local services with an idle-reset dead man's switch.
- **MCP Server Instructions (PR #58182):** Surfacing MCP server initialize instructions into the system prompt, improving tool usage context.
- **Local Model Setup Guide (Issue #523):** The persistent demand for better local model onboarding suggests this may be prioritized as a documentation or skill enhancement.
- **No API Key for Local Providers (Issue #41370):** Users want to connect to local LLM endpoints without an API key, which is a common friction point.

### 7. User Feedback Summary
- **Pain Points:**
    - **Session State Instability:** Multiple reports (##48098, ##55578, ##50688, ##58684) describe sessions becoming stale, resurrecting incorrectly, or delivering results to the wrong context. This is the single largest category of user frustration.
    - **Provider Incompatibility:** Users are hitting hard errors with OpenRouter (##60821) and Deepseek (##57864, ##60338), often related to argument mismatches or timeouts.
    - **Cross-User Security:** Two security PRs today directly address user fears about data leaks in shared gateways (session_search and cron jobs).
- **Satisfaction:** The rapid release of v0.18.1 and v0.18.2 (covering 660 PRs) signals to users that the team is responsive, but the volume of post-release bugs suggests a need for more thorough pre-release testing.

### 8. Backlog Watch
The following items require maintainer attention due to age, severity, or lack of response:

- **Issue #523 (Local Model Setup Skill):** Created **March 2026** (over 4 months ago). A highly-voted feature request with no official response documented. Users are looking for guidance on Ollama/llama.cpp/vLLM.
- **Issue #6626 (Gemma 4 Support):** Created **April 2026**. The user is blocked on tool calling with Gemma 4. This is a model that is gaining popularity, and lack of official support could plateau adoption.
- **Issue #25106 / #25107 (CLI/Gateway Model Config):** Created **May 2026**. Persistent bugs around `base_url` and `api_mode` persistence after model switching remain unmerged. These are likely systemic issues affecting multiple user workflows.

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

Here is the project digest for PicoClaw, generated from the provided GitHub data for **2026-07-09**.

---

## PicoClaw Project Digest — 2026-07-09

### 1. Today's Overview
PicoClaw is currently in a **moderate activity phase**. While no new releases were published today, the maintainers were highly responsive, merging 4 pull requests in the last 24 hours, including a significant fix for Anthropic vision models. The community is reporting several critical bugs (particularly around tool call handling and OAuth login failures), and there are signs of **maintainer backlog management** via automated stale-labeling, with 6 issues updated recently. The project appears stable but is experiencing friction with specific edge-case configurations and third-party integrations.

### 2. Releases
**No new releases** were published on this date. The latest stable version remains **v0.2.9**, as referenced in multiple open bug reports.

### 3. Project Progress (Merged/Closed PRs)
All 4 PRs updated in the last 24 hours were closed/merged, indicating a burst of code integration.

- **[PR #3234] CHORE (anthropic_messages): embed image media in user messages** *(Merged 2026-07-08)*
  - **Critical fix:** The `anthropic_messages` provider was dropping image data (from `load_image`) because the `buildRequestBody` function ignored `msg.Media`. Vision models (e.g., Claude) were replying "can't see images." This patch embeds media into user messages.
  - *Significance:* Directly fixes a broken feature for Anthropic users.
  - **Link:** [PR #3234](https://github.com/sipeed/picoclaw/pull/3234)

- **[PR #2278] feat(gateway): fallback to wildcard bind with CIDR allowlist** *(Merged 2026-07-08)*
  - **Enhancement:** Improves gateway startup reliability on systems where loopback binding is unavailable (e.g., containers, VMs). Adds a fallback to a wildcard bind with a CIDR allowlist for security.
  - *Significance:* Improves deployability on non-standard network environments.
  - **Link:** [PR #2278](https://github.com/sipeed/picoclaw/pull/2278)

- **[PR #2251] feat(channels): add Grafana Alertmanager webhook channel** *(Merged 2026-07-08)*
  - **New Feature:** Adds an input-only channel that exposes a webhook endpoint for Grafana alerts. Supports triggering specific skills upon incoming alerts.
  - *Significance:* Expands PicoClaw's integration into observability and DevOps use cases.
  - **Link:** [PR #2251](https://github.com/sipeed/picoclaw/pull/2251)

- **[PR #3157] feat: add Android ADB remote operations tool** *(Merged 2026-07-07)*
  - **New Feature:** An experimental tool for interacting with Android devices via ADB. Provides primitives for screenshots, UI hierarchy, taps, swipes, and wake, without exposing arbitrary shell execution.
  - *Significance:* Opens the door for mobile device automation and testing.
  - **Link:** [PR #3157](https://github.com/sipeed/picoclaw/pull/3157)

### 4. Community Hot Topics
The most active discussions revolve around **OAuth integration failures** and **LLM provider quirks**.

- **[Issue #3093] [CLOSED] Feature Request: SimpleX or Tox support**
  - *Comments: 5 | 👍: 1*
  - **Analysis:** A user is requesting decentralized, privacy-focused messaging protocols (SimpleX, Tox, Wire). This signals a demand for **off-grid or metadata-minimized communication channels**. While closed as stale, the underlying need for privacy-preserving gateways is persistent.
  - **Link:** [Issue #3093](https://github.com/sipeed/picoclaw/issues/3093)

- **[Issue #3197 & #3196] [OPEN] BUG: Codex and Anti Gravity OAuth login not working**
  - *Comments: 1 each (no maintainer reply yet)*
  - **Analysis:** Two separate reports from the same user (nyawitniorang) pointing to the same root cause: OAuth login flows for the Codex and Anti Gravity services are broken in v0.2.9. This appears to be a **blocking regression** for users relying on these specific login providers.
  - **Link:** [Issue #3197](https://github.com/sipeed/picoclaw/issues/3197), [Issue #3196](https://github.com/sipeed/picoclaw/issues/3196)

### 5. Bugs & Stability
Several bugs are active, with **OAuth login failures** ranked as the highest severity due to their blocking nature.

- **Severity: High (Blocking)**
  - **OAuth Login Failures (Codex & Anti Gravity)** — Issues #3196 & #3197
    - *Status:* Open. No fix PR exists yet. Affects v0.2.9.
  - **GPT-5.4 failure on NanoKVM** — Issue #3195
    - *Status:* Open. A user reports that default configuration fails to yield any response, suggesting a provider compatibility issue or missing API key handling.
    - **Link:** [Issue #3195](https://github.com/sipeed/picoclaw/issues/3195)

- **Severity: Medium (Functional gaps)**
  - **Volcengine Doubao tool call leakage** — Issue #3153
    - *Status:* Open. Raw `<seed:tool_call>` XML is returned to the user instead of being executed. This is a **model/provider glue issue** for the `doubao-seed-2.0-pro` model.
    - **Link:** [Issue #3153](https://github.com/sipeed/picoclaw/issues/3153)
  - **Task Repetition** — Issue #3159
    - *Status:* Closed (stale). A Chinese-language report indicated that the agent repeats prior tasks (e.g., fetching US news again when asking for French news). This suggests a **context management or prompt injection bug**.
    - **Link:** [Issue #3159](https://github.com/sipeed/picoclaw/issues/3159)

### 6. Feature Requests & Roadmap Signals
- **Privacy Messaging (SimpleX/Tox)** — Issue #3093: A persistent request for fully encrypted, decentralized channels. Likely will not land in the next minor release but indicates a roadmap direction for privacy-conscious users.
- **Mobile Device Automation (ADB)** — PR #3157 (just merged): This experimental feature is now in main. It is likely to be refined in the next v0.3.x release.
- **DevOps Integration (Grafana)** — PR #2251 (just merged): The new Grafana channel points toward PicoClaw being used as an intelligent alert responder. Expect documentation and stabilization in the next version.
- **Prediction:** The next release (v0.3.0 or v0.2.10) will likely focus on **fixing the OAuth regression** (high priority) and improving **vision model media handling** (just fixed via PR #3234).

### 7. User Feedback Summary
- **Pain Point (New Users):** Setup on specialized hardware (NanoKVM) and specific providers (GPT-5.4) is failing with default configs, causing frustration for new integrators.
- **Pain Point (OAuth Users):** Two users reported identical OAuth failures for Codex and Anti Gravity services, indicating a **broken authentication flow in v0.2.9**.
- **Satisfaction Signal:** The presence of 4 merged PRs in one day (including a critical Anthropic fix and a new Grafana channel) suggests a healthy core development cycle, even if bug reports are piling up.
- **Use Case Insight:** A user is deploying PicoClaw on Debian 13 with DeepSeek v4 and encountering task repetition, highlighting that **multi-turn task execution reliability** remains unpolished.

### 8. Backlog Watch
Several items remain open with no recent maintainer engagement, flagged as "stale."

- **[Issue #3153] Volcengine Doubao tool call leakage** — Last updated 2026-07-07 (no fix PR). This is a specific provider integration bug that is likely low priority for the core team but blocks a user in production.
  - **Link:** [Issue #3153](https://github.com/sipeed/picoclaw/issues/3153)

- **[Issue #3195] OpenAI GPT does not work on NanoKVM** — Last updated 2026-07-07. No maintainer reply. This may be an environment-specific configuration issue, but the lack of a diagnosis is concerning.
  - **Link:** [Issue #3195](https://github.com/sipeed/picoclaw/issues/3195)

- **[Issue #3196 & #3197] OAuth login not working (Codex / Anti Gravity)** — Last updated 2026-07-07. No maintainer reply. **These should be prioritized** as they are likely regressions and affect basic authentication.
  - **Link:** [Issue #3196](https://github.com/sipeed/picoclaw/issues/3196), [Issue #3197](https://github.com/sipeed/picoclaw/issues/3197)

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

Here is the NanoClaw project digest for July 9, 2026.

---

## NanoClaw Project Digest | 2026-07-09

### 1. Today's Overview
The project shows **very high development velocity** today, driven by a five-part scheduled-tasks feature train now merging into the main branch. While public issue traffic is minimal (one new feature request), internal team activity is intense, with **38 pull requests** updated in the last 24 hours—27 open and 11 merged or closed. A major `ncl CLI` infrastructure upgrade (Part 1/6, merged) and a follow-up task-control plane (Part 2/5, open) signal tightly coordinated feature delivery. Overall project health is strong, with the core team executing on planned roadmap items.

### 2. Releases
**No new releases today.** The latest published builds remain unchanged.

### 3. Project Progress
**11 PRs were merged or closed today**, representing significant forward motion on core infrastructure:
- **ncl CLI Revamp (#2980, merged):** Every `ncl` verb now has strict argument validation, deep help, and server-rendered human-view responses. This is Part 1/6 of the scheduled-tasks feature train.
- **CI Automation (#2978, merged):** PRs from core team members now automatically receive a `core-team` label, reducing manual triage overhead.
- **Scheduled Tasks Train Continues (#2981, open):** This open PR (Part 2/5) adds the full `ncl tasks` control plane with isolated sessions, run history, and script-gate infrastructure. It supersedes an earlier sketch (#2947), indicating the feature scope expanded during implementation.
- **Tool Allowlist Reconciliation (#2982, open):** A critical fix aligning the agent runner's tool allowlist with the pinned Claude CLI version (2.1.197), removing five nonexistent tools and adding a drift guard. This prevents agent failures from upstream API changes.
- **Harness Capability Toggles (#2983, open):** Extending the policy of disabling duplicate harness capabilities, this PR turns off agent-teams and workflow capabilities by default, moving their toggles into a shared `ConfigProvider`.

### 4. Community Hot Topics
With only one open issue and zero comments on all PRs today, there is **no active community discussion**. The single open item is a feature request:

- **#2984 (Open):** *"feat: auto-rename Discord threads by topic"*
  - **Need:** Users running busy Discord servers find NanoClaw's default thread names (e.g., "Thread 7/8/2026, 3:28 PM") unmanageable. The request asks for an agent-side tool to let the host agent rename threads to concise topic names. This is a low-friction quality-of-life improvement for Discord operators.

### 5. Bugs & Stability
No critical crashes or regressions were reported today. Identified issues are lower severity:
- **(Resolved) Tool Allowlist Drift (#2982, open fix):** The agent runner's `TOOL_ALLOWLIST` referenced five tools that no longer exist in the pinned Claude CLI, including `Task` (renamed to `Agent`). The fix removes these entries and adds a build-time drift guard. **Severity: Medium**—could cause silent agent failure when those tools are invoked.
- **Codex Reconnect Pooling (#2878, open, older):** Stale or revoked OneCLI tokens cause Codex agents to fail mid-conversation. The existing fix ensures it doesn't falsely report success, but a full reconnect path is still pending.

### 6. Feature Requests & Roadmap Signals
The primary roadmap signal is the **scheduled-tasks train**, which is reshaping the CLI and task management experience. Predictions for the next release:
- **High confidence:** `ncl tasks` CLI commands (`list`, `cancel`, `pause`, `resume`) will ship, giving operators control over runaway tasks.
- **Medium confidence:** Per-group harness capability toggles and the template setup wizard (#2909) will land, making first-agent setup easier.
- **Low confidence:** The Discord thread rename tool (#2984) is lightweight enough to be included as a community skill in a patch release.

### 7. User Feedback Summary
- **Pain Point:** Discord thread naming is the only direct user complaint today. Users want topic-based renaming over timestamps for scanability.
- **Satisfaction:** The rapid merge of the `ncl CLI` improvements (#2980) suggests internal satisfaction with the direction—more structured, validated commands.
- **No negative feedback** around performance, reliability, or breaking changes was recorded.

### 8. Backlog Watch
Several long-lived PRs remain open without maintainer resolution:
- **#2742 (PR Factory, opened June 11):** A published recipe for PR review & triage. High community value, but has seen no activity in 28 days. May be waiting on the scheduled-tasks train to stabilize before merge.
- **#2798 (Changelog for v2.1.17, opened June 17):** A docs-only PR updating the changelog. Low risk, could be merged quickly to close a documentation gap.
- **#2873 (Skill pre-flight split, opened June 27):** Enables `/update-skills` to refresh code separately from credentials. Important for skill maintainers, but has stalled. Likely blocked by internal architecture decisions.
- **#2913 / #2914 (WhatsApp Cloud fixes, opened July 2):** Fixes an instance-key collision between WhatsApp Cloud and native Baileys adapter. Documentation follows in #2914. These are blocking real WhatsApp Cloud deployments and warrant prioritization.

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw Project Digest — 2026-07-09

## Today's Overview

IronClaw is in a period of **high active development**, with 50 pull requests and 24 issues updated in the last 24 hours — one of the busiest days observed. The project is mid-way through a major **composition refactor** (the "god-crate dissection" series), with steps n9 and n10 now in progress to reorganize automation and WebUI clusters into dedicated modules. While no new releases were cut today, a release PR (#5598) remains open, signaling an upcoming version bump with breaking changes to `ironclaw_common`, `ironclaw_skills`, and the main `ironclaw` binary. **Stability concerns persist**: a bug bash has uncovered multiple P2-severity issues around Slack integration, routine failures, and GitHub API errors, but the team is actively landing fixes, including automation renaming (#5765) and timestamp persistence (#5764).

## Releases

**None** — no new releases published in the last 24 hours.

However, PR [#5598](https://github.com/nearai/ironclaw/pull/5598) (open, sized M, risk medium) prepares the following release candidates:
- `ironclaw_common`: 0.4.2 → **0.5.0** (breaking changes)
- `ironclaw_skills`: 0.3.0 → **0.4.0** (breaking changes)
- `ironclaw`: 0.24.0 → **0.29.1** (major version jump)
- `ironclaw_safety`: 0.2.2 → 0.2.3 (compatible)
- `ironclaw_skill_learning`: 0.1.0 → 0.1.1 (compatible)

## Project Progress

**16 PRs closed/merged** in the last 24 hours. Notable advances:

### Features
- **[#5765](https://github.com/nearai/ironclaw/pull/5765)** — **Automation renaming** is now supported: inline rename controls added to the WebUI v2 detail panel, backed by trigger storage and API changes. (Sized XL, low risk, human-verified)
- **[#5764](https://github.com/nearai/ironclaw/pull/5764)** — **Chat timeline timestamps** are now durable: per-message `created_at`/`updated_at` persisted across in-memory and filesystem stores, fixing a long-standing QA bug (#3535).
- **[#5772](https://github.com/nearai/ironclaw/pull/5772)** — **Full i18n coverage for the Reborn Projects page**: replaced all hardcoded English labels with localization keys across summary, grids, inspectors, and empty states. Addresses issue #5768.

### Infrastructure & CI
- **[#5840](https://github.com/nearai/ironclaw/pull/5840)** — CI fix: full clippy matrix now runs in the merge queue, preventing green merges that break `main`. Also fixes libsql-only dead code warnings.
- **[#5770](https://github.com/nearai/ironclaw/issues/5770)** — Closed: improved Reborn tool permission selects with a custom dropdown, replacing browser-native `<select>` for dark-mode consistency.

### Cleanup & Housekeeping
- Two issues closed for v1 coverage cleanup: [#5828](https://github.com/nearai/ironclaw/issues/5828) (stale references) and [#5827](https://github.com/nearai/ironclaw/issues/5827) (orphaned fixtures).

## Community Hot Topics

### Most Active Discussions

1. **[[OPEN] Issue #5702](https://github.com/nearai/ironclaw/issues/5702)** — *GitHub issue search and create capabilities fail with HTTP 403*
   - *4 comments, 0 👍* — Reported during bug bash (P2). The agent's GitHub integration returns `operation_failed` when searching or creating issues, despite proper configuration. No fix PR yet.

2. **[[OPEN] Issue #5705](https://github.com/nearai/ironclaw/issues/5705)** — *Terminal icon in chat UI has no disable option*
   - *2 comments, 0 👍* — Users cannot hide the terminal icon from the chat interface. Tagged P3, related to but distinct from issue #5555 (floating button overlap).

3. **[[OPEN] Issue #5788](https://github.com/nearai/ironclaw/issues/5788)** — *Daily ironclaw failure taxonomy — 2026-07-08*
   - *0 comments* — Automated daily analysis of pinchbench failures. Identifies 3 of 4 non-pass tasks as `gws_*` integration tasks scoring 0 due to a harness defect layered on a real problem. This "taxonomy" series is a signal that the team is systematically triaging benchmark regressions.

**Underlying need**: These issues reveal two patterns — (a) users need reliable third-party integrations (GitHub, Slack) to trust the agent for daily workflows, and (b) UI customizability (hiding unused features) is a recurring request that indicates the interface is becoming feature-dense.

## Bugs & Stability

**8 new bugs reported today**, with severity distribution:

### P2 (High Severity)
- **[[#5837](https://github.com/nearai/ironclaw/issues/5837)]** — Routine run "Open run" and "Logs" buttons are **not clickable** on failed runs. Users cannot inspect failures. *No fix PR.*
- **[[#5838](https://github.com/nearai/ironclaw/issues/5838)]** — Runs fail with **context compaction error** after successful tool execution. Error: *"context compaction could not complete. Retry with a shorter request."* *No fix PR.*
- **[[#5836](https://github.com/nearai/ironclaw/issues/5836)]** — Scheduled routine fails every 5 minutes with **"No thread attached"** — 0% success rate for the `ironclaw-issues-slack-summary` routine. Appears systemic. *No fix PR.*
- **[[#5834](https://github.com/nearai/ironclaw/issues/5834)]** — **Slack disconnect request incorrectly rejected** by agent. Agent claims it cannot perform the action and returns unrelated content. Users cannot disconnect Slack through the agent. *No fix PR.*

### P3 (Low Severity)
- **[[#5835](https://github.com/nearai/ironclaw/issues/5835)]** — "Jump to latest" button appears unnecessarily and overlaps chat content.
- **[[#5820](https://github.com/nearai/ironclaw/issues/5820)]** — WebChat silently drops files beyond the 10-file limit without error feedback.
- **[[#5705](https://github.com/nearai/ironclaw/issues/5705)]** — Terminal icon has no disable option (reported earlier, still open).

### Resolved Today
- **[[#5787](https://github.com/nearai/ironclaw/issues/5787)]** — Closed: flaky test `slack_pairing_redeem_rejects_expired_code` caused by tokio clock vs chrono wall-clock TTL race.
- **[[#5466](https://github.com/nearai/ironclaw/issues/5466)]** — Closed: Parallel same-tenant turn-runs vs `FilesystemTurnStateStore` CAS/libsql backend (~10% failure).
- **[[#5467](https://github.com/nearai/ironclaw/issues/5467)]** — Closed: In-memory `ApprovalRequestStore::discard_pending` diverges from filesystem (no tombstone → id reuse allowed).
- **[[#3535](https://github.com/nearai/ironclaw/issues/3535)]** — Closed: UI timestamps incorrect for conversations (fixed by #5764).

### CI / Test Stability
- **[[#4108](https://github.com/nearai/ironclaw/issues/4108)]** — Nightly E2E failed again (ongoing, open since May 27). The `web-regressions` job consistently fails. This is a **long-running concern**.

## Feature Requests & Roadmap Signals

### In Progress (PRs Open)
- **Stream WebUI assistant text over projections** ([#5821](https://github.com/nearai/ironclaw/pull/5821), sized XL) — Adds SSE support for streaming model output through the Reborn/WebUI path. This is a major UX improvement.
- **One unified Slack extension** ([#5845](https://github.com/nearai/ironclaw/pull/5845), sized XL, low risk) — Retires `slack_bot` and `slack_personal` in favor of a single `slack` extension. Part of the NEA-25 stack (4/7) to simplify the extension surface.
- **Extension-surface discovery replaces connectable-channels** ([#5842](https://github.com/nearai/ironclaw/pull/5842), sized XL, low risk) — Net −900 lines; replaces the parallel channel registry with a tagged enum on `RebornExtensionInfo.surfaces`. NEA-25 stack PR 3/7.
- **Admin installed and private skills** ([#5780](https://github.com/nearai/ironclaw/pull/5780), sized XL, low risk) — Adds support for organization-managed skills with visibility controls.
- **Tell agent to compute with tools, not in its head** ([#5844](https://github.com/nearai/ironclaw/pull/5844), sized XS) — System prompt addition directing the agent to offload calculations (statistics, conversions, aggregations) to tools instead of doing them internally.

### Prediction for Next Release
Based on open PRs and the release train (#5598), the next version (0.29.x) will likely include:
- Breaking changes to `ironclaw_common` and `ironclaw_skills` APIs
- The **unified Slack extension** (NEA-25 stack)
- **Automation renaming** (included)
- **Persistent chat timestamps** (included)
- **Streaming assistant text** (likely, if #5821 merges before release)
- **Admin-managed skills** (likely)
- **Composition refactor** — the `automation/` and `webui/` module groupings (n9, n10) are pure renames with no behavioral change, so they should ship without risk.

## User Feedback Summary

### Pain Points (Explicit User Reports)
- **GitHub integration broken** (#5702): Cannot search or create issues via the agent despite integration being configured — blocks a core workflow.
- **Slack disconnect impossible** (#5834): Agent refuses to disconnect Slack; users feel locked into the integration.
- **Scheduled routines all failing** (#5836): The `ironclaw-issues-slack-summary` routine has 0% success rate with "No thread attached" error — renders scheduled automation unusable.
- **Context compaction crashes** (#5838): Heavy multi-tool runs succeed in execution but crash during compaction, wasting time and losing results.
- **Cannot rename automations** (now fixed by #5765): Auto-generated names were too long or truncated.
- **Cannot inspect failed runs** (#5837): "Open run" and "Logs" buttons are dead on failure — users cannot diagnose problems.
- **WebChat silently drops files** (#5820): More than 10 attached files are silently skipped; no error message. User hit this twice in real workflow usage.

### Satisfaction Signals
- **Automation renaming** (PR #5765) and **chat timestamp persistence** (PR #5764) directly address longstanding community complaints — likely to improve satisfaction.
- **Design system tokens + /playground** ([#5563](https://github.com/nearai/ironclaw/pull/5563)) is still open but represents deep investment in UX quality from design leadership.

## Backlog Watch

### Long-Unresolved Issues Needing Attention

1. **[[#4108](https://github.com/nearai/ironclaw/issues/4108)]** — **Nightly E2E failed** (open since May 27, ~43 days)
   - The nightly E2E pipeline has been failing consistently for over a month in the `web-regressions` job. This is a **critical infrastructure debt** — the team may be relying on this as a safety net, but it has been red since late May. PR #5841 (revive nightly deep CI) aims to address this, but success remains to be seen.

2. **[[#5557](https://github.com/nearai/ironclaw/issues/5557)]** — **Logs deep link requires opening twice** (open since July 2, ~7 days, P3)
   - Minor but annoying: clicking a Logs link from a routine run shows "Select conversation" instead of loading the linked thread on first click. Requires a second click to work. No fix PR yet.

3. **[[#5705](https://github.com/nearai/ironclaw/issues/5705)]** — **Terminal icon has no disable option** (open since July 6, ~3 days, P3)
   - Users cannot remove the terminal icon from the chat UI. While low severity, it's a simple UI customizability request that may grow in priority as the interface becomes more crowded.

4. **V1 Coverage Cleanup** — Issues [#5826](https://github.com/nearai/ironclaw/issues/5826), [#5827](https://github.com/nearai/ironclaw/issues/5827), [#5828](https://github.com/nearai/ironclaw/issues/5828) were opened today to delete legacy v1 test binaries, fixtures, and references. These are straightforward cleanup tasks with low risk, but need a maintainer to approve and merge the corresponding PRs (if they exist; none linked yet).

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

Here is the **LobsterAI Project Digest** for **2026-07-09**, generated from the provided GitHub data.

---

### 1. Today's Overview

Today marks a period of high maintenance velocity for LobsterAI, with **13 Pull Requests** updated and **10 closed/merged** in the last 24 hours. This heavy PR activity suggests a focused push on stabilization and feature refinement following the recent major version releases. However, the **3 updated issues** (mostly stale) and **0 new releases** indicate that while the engineering team is actively deploying fixes, the community backlog remains quiet, with no immediate new version planned. Project health appears stable, driven by internal development rather than external bug pressure.

### 2. Releases

No new releases were published today. The project remains on the version referenced by the community (likely v4.1, per Issue #1400).

### 3. Project Progress (Merged/Closed PRs)

A significant batch of **10 PRs** were closed today, primarily focusing on **bug fixes** and **feature enhancements** across the core renderer, OpenClaw, and Cowork modules.

**Key Fixes & Refinements:**
- **Agent Workspace Isolation (PR #2295):** Fixed a critical bug where editing the "about you" (USER.md) in one agent would overwrite settings in all other agents. The bootstrap file is now scoped per agent workspace.
- **IM Session Scoping (PR #2298):** Fixed session mapping collisions in OpenClaw IM, ensuring chat history and context are properly scoped by `agent_id` rather than just conversation ID.
- **Memory Search Stability (PR #2297):** Added a fix to default the memory search to local Full-Text Search (FTS) when embedding search is disabled, preventing gateway startup crashes for users with upgraded configurations.
- **SSE Security (PR #1401):** Merged a security fix replacing `Math.random()` with `crypto.randomUUID()` for SSE request IDs to prevent potential subscription hijacking.
- **UI/UX & i18n:** Fixed a multi-file attachment picker bug (PR #1402), resolved a Chinese UI language fallback for the "delete" key (PR #1403), and optimized the scheduled task creation time picker (PR #1404).

**Feature Advancements:**
- **Delegated Subagent Collaboration (PR #2285):** A major feature was merged allowing users to configure "subagent" delegation. Agents can now be allowed to delegate tasks to other specific agents, materialized as child Cowork sessions.
- **Minimizable Cowork Prompts (PR #2296):** Added UI support for minimizing permission prompts in the Cowork interface, improving multitasking during agent interactions.

### 4. Community Hot Topics

The community is largely inactive in new discussion today, with most activity centered on lingering issues.

- **"About You" Content Linking (Issue #2293 - OPEN):** This is the **newest and most active topic** with 1 comment. A user discovered that editing the USER.md for one agent propagates to all others, breaking the intended isolation per agent. This was **directly addressed by today’s merge (PR #2295)**, which likely resolves the user's concern.
- **V4.1 Startup Loop (Issue #1400 - CLOSED):** This was a high-severity, long-running issue. While closed today (likely marked stale), the severity of the feedback ("彻底瘫痪" / "completely paralyzed") highlights a critical stability regression that users faced between v3.30 and v4.1.

### 5. Bugs & Stability

| Severity | Issue | Description | Status |
| :--- | :--- | :--- | :--- |
| **High** | #1400 | v4.1 Gateway infinite restart loop; config conflict preventing LLM (Qwen 3.5) use. | **Closed** (Marked stale, likely resolved in prior patch) |
| **Medium** | #2293 | USER.md configuration syncs incorrectly across multiple agents (tied to #2295). | **Fixed** by PR #2295 (Merged today) |
| **Low** | #1348 | No validation for duplicate scheduled task names. | **Open** (Stale, awaiting attention) |

**Analysis:** The ecosystem is recovering from the v4.1 upgrade issues. The highest impact bug (startup loop) has been closed, and the medium impact bug regarding agent settings has a confirmed fix merged today.

### 6. Feature Requests & Roadmap Signals

The most significant roadmap signal today is the merging of **Subagent Delegation (PR #2285)**. This indicates a strategic push towards complex, collaborative agent workflows where agents can call upon each other autonomously.

Other pending features visible in the backlog include:
- **Enhanced Skills Management (PR #1346):** A long-standing PR (stale) for a detailed skills management system.
- **Advanced Scheduled Tasks (PR #1347):** A feature-rich PR (stale) adding Cron expressions, Agent/Model binding, and UI overhauls to the scheduling module.

**Prediction:** Given the strong momentum on multi-agent features (delegation, IM scoping, agent-specific workspaces), the **next minor release** will likely focus on stabilizing the Subagent collaboration feature announced in PR #2285 and resolving remaining backend issues from the v4.1 release.

### 7. User Feedback Summary

- **Frustration with Upgrade Regressions:** User `danielmonlite` (#1400) expressed extreme dissatisfaction with the v4.1 upgrade, describing the software as "completely paralyzed" due to a startup loop and LLM configuration conflicts.
- **Need for Granular Agent Identity:** User `yepcn` (#2293) clearly needs independent personalities/configurations for each agent, highlighting a use case where users manage multiple distinct agents for different roles. The fix in PR #2295 directly addresses this need.
- **Stability & Reliability:** The "stale" label on the v4.1 crash issue suggests users may have given up or migrated, indicating a need for more timely communication and patch releases for critical regressions.

### 8. Backlog Watch

The following items require maintainer attention to prevent permanent drift:

- **Issue #1348: Duplicate Task Names.** A simple UX bug (no validation) that has been unanswered since April. Low complexity, high impact on data integrity.
    - *Link:* [Issue #1348](https://github.com/netease-youdao/LobsterAI/issues/1348)
- **PR #1346: Skills Management.** An important feature PR that has been open for three months. Needs review and integration or a decision to close it.
    - *Link:* [PR #1346](https://github.com/netease-youdao/LobsterAI/pull/1346)
- **PR #1347: Scheduled Task Enhancements.** Another long-standing PR with significant UX improvements (Cron editor) that adds high value to power users.
    - *Link:* [PR #1347](https://github.com/netease-youdao/LobsterAI/pull/1347)

</details>

<details>
<summary><strong>TinyClaw</strong> — <a href="https://github.com/TinyAGI/tinyagi">TinyAGI/tinyagi</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>Moltis</strong> — <a href="https://github.com/moltis-org/moltis">moltis-org/moltis</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>CoPaw</strong> — <a href="https://github.com/agentscope-ai/CoPaw">agentscope-ai/CoPaw</a></summary>

Based on the provided GitHub data for **CoPaw (github.com/agentscope-ai/CoPaw)**, here is the project digest for **2026-07-09**.

---

## CoPaw Project Digest — 2026-07-09

### 1. Today's Overview
Project activity remains very high, with 38 Issues and 44 PRs updated in the last 24 hours, indicating a highly dynamic development cycle. The release of **v2.0.0-beta.4** signals continued aggressive iteration on the 2.0 branch, focusing on stability fixes for the "scroll" context manager and security hardening. The majority of activity centers on the **2.0 beta** series, where user reports of context loss and infinite loops are driving urgent patches. A notable strategic shift is visible with the introduction of a **"real-behavior-proof" PR gate** (PR #5844) to combat spam and enforce quality validations on contributions, suggesting the project is scaling its community contributions.

### 2. Releases
**New Release: v2.0.0-beta.4**
- **Key Changes:** Includes a fix for the `scroll` context management system to "protect the active turn" and add "graduated pressure relief," a direct response to user reports (e.g., #5746, #5860) where the system incorrectly evicted the current task. It also makes recall failures "unmistakable," improving error signaling for context loss.
- **Breaking Changes / Migration Notes:** None explicitly documented. As a beta release, users are expected to report regressions. The primary focus is stabilizing the new `scroll` algorithm before a stable 2.0 release.

### 3. Project Progress
**15 PRs were merged or closed today.** Key advancements include:
- **Core Stability & Context Management:** Multiple PRs target the "scroll" context manager, including fixing the labeling of evicted spans (PR #5848) and recovering malformed whitespace-prefixed JSON tool calls (PR #5841).
- **Security Hardening:** A critical fix (PR #5866) addresses a `rm` bypass in security detection that allowed `${HOME}` to be deleted. Another PR (PR #5745) redacts secrets from persisted dialog artifacts.
- **Approval Pipeline Fix:** A merged PR (PR #5864) fixes a mismatch where the runtime approval level was not being applied correctly to MCP (Model Context Protocol) Driver policies.
- **Massive Test Infrastructure Push:** A significant effort ("PR-A1" through "PR-F3") is adding hundreds of new unit tests across the backend (inbox, approvals, channels, security) and frontend (API contracts, hooks, stores), indicating a major push for reliability.

### 4. Community Hot Topics
- **[#5757: [Bug] Feishu (Lark) not replying to messages](https://github.com/agentscope-ai/QwenPaw/issues/5757)** (12 comments, Open) — Users report that after the first reply, the Feishu channel stops responding to subsequent messages, affecting both Docker and cloud instances. This is a high-impact integration issue for enterprise users.
- **[#5846: [Bug] Approval popup still appears in "Closed Mode"](https://github.com/agentscope-ai/QwenPaw/issues/5846)** (10 comments, Closed) — A workflow-breaking bug where the "auto-execute" setting fails, forcing users to manually approve every tool call. The issue's closure today suggests a fix was recently deployed.
- **[#5171: [Bug] Context compression deletes everything](https://github.com/agentscope-ai/QwenPaw/issues/5171)** (9 comments, Closed) — When a character/role file exceeds the token threshold, compression removes all context, causing task interruption. This core UX flaw highlights the fragility of the current memory system.
- **[#5860: [Bug] Frequent conversation progress loss and infinite loops in v2.0](https://github.com/agentscope-ai/QwenPaw/issues/5860)** (3 comments, Open) — A critical v2 bug where the agent "forgets" the current question and loops back to a previous task or repeats infinitely. This is the most severe stability issue reported for the 2.0 branch.

**Underlying Needs:** The community is primarily demanding **stability in core workflows** (context management, tool execution, channel integration). Users want the agent to be reliable for long-running, multi-turn tasks without losing state or requiring manual intervention.

### 5. Bugs & Stability
Bugs are ranked High (breaking), Medium (functional impact), Low (minor).

- **[High] #5860: v2.0 Frequent conversation progress loss and infinite loops.** Reported for v2.0.0-beta.3. This is a critical regression for the new version. Fixes are being actively developed (see `scroll` related PRs #5848, #5746).
- **[High] #5846: Approval popup in "Closed Mode".** Closed today, implying a fix was released, but the issue severely broke workflow automation.
- **[High] #5868: Matrix Channel token login failure.** A channel breaking bug after an upgrade, causing authentication errors.
- **[Medium] #5725: Browser lag during streaming.** A performance regression in the Console UI, likely related to DOM rendering under heavy streaming.
- **[Low-Medium] #5259: Windows vector index not persisted.** A platform-specific bug forcing users to rebuild memory on every startup.

**Fix PRs in Flight:** PR #5866 (security bypass), PR #5848 (scroll context), PR #5745 (secret redaction), PR #5761 (malformed tool-calls).

### 6. Feature Requests & Roadmap Signals
- **[#5852: System notification sound for approvals.](https://github.com/agentscope-ai/QwenPaw/issues/5852)** (2 comments, Open) — Users want auditory alerts for pending tool approvals, indicating a need for better background/ "invisible" interaction patterns.
- **[#5139: Native Agent Team / Swarm Collaboration.](https://github.com/agentscope-ai/QwenPaw/issues/5139)** (Closed) — A long-standing request for multi-agent collaboration, similar to "WorkBuddy" or "JiuwenSwarm."
- **[#5692: Reranker for memory search.](https://github.com/agentscope-ai/QwenPaw/pull/5692)** (Open PR) — A developer is actively implementing a reranker for the memory search pipeline, which would significantly improve retrieval accuracy.

**Prediction:** The **reranker** (PR #5692) is likely to land in v2.0.0-beta.5, as it directly addresses the memory/context retrieval issues that are the top complaint. The **system tray minimize** feature (#5312) is low-effort and may appear for the next desktop release.

### 7. User Feedback Summary
- **Dissatisfaction:** The primary pain point is the **instability of the v2.0 beta** series. Users report the system "loses its mind" or enters "loops," which kills trust in the software for automated tasks. The automated approval popup bug (#5846) frustrated power users relying on headless operation.
- **Use Cases:** Users are deploying CoPaw for **automated task execution** (cron jobs, heartbeat agents), **long-running research** (multi-step tool calls), and **IM integration** (Feishu, QQ, Matrix).
- **Mixed Feelings:** The new features (swarm, desktop automation) are exciting to the community, but the current focus on patching critical regressions suggests the userbase feels the project is pushing features faster than it is stabilizing the 2.0 core.

### 8. Backlog Watch
- **[Issue #5860] #5860 [Bug]: v2.0 Frequent conversation progress loss.** (2026-07-08, Open, 3 comments) — This is the most critical open issue. Requires immediate maintainer attention as it is a core reliability defect in the latest beta.
- **[Issue #5259] Windows vector index persistence.** (2026-06-17, Open, 4 comments) — This has been open for over three weeks. It is a platform-specific regression that degrades the experience for a significant user base (Windows desktop). No fix PR has been linked.

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

Here is the ZeroClaw project digest for **2026-07-09**.

---

## 1. Today's Overview

ZeroClaw is in a period of **intense architectural transformation**. The project saw significant activity with **50 issues and 50 PRs updated in the last 24 hours**, driven by three major parallel initiatives: a full rewrite of the provider architecture, a shift toward runtime WASM plugins, and major governance changes (Work Lanes). While no new releases were cut, the volume of open RFCs and large structural PRs (XL-sized) indicates the team is laying the groundwork for a significant **0.8.x or 0.9.0 release**. The project health appears robust, with high engagement from core contributors on complex, long-running items, though several critical bugs remain open.

## 2. Releases

**No new releases in the last 24 hours.**

## 3. Project Progress

**Merged/Closed PRs (Highlights):** 14 items were closed or merged. Key advancements include:
- **Localization Fix:** PR [#8769](https://github.com/zeroclaw-labs/zeroclaw/pull/8769) (fix: localize channel runtime replies) was updated, pushing forward the effort to eliminate hardcoded English strings from channel commands.
- **Security Hygiene:** PR [#8781](https://github.com/zeroclaw-labs/zeroclaw/pull/8781) (fix security: remove stale advisory ignores) was updated, cleaning up the `deny.toml` to pass CI gates.
- **SOP/Workflow Logic:** PR [#8771](https://github.com/zeroclaw-labs/zeroclaw/pull/8771) (fix sop: fall through to next step when `when` is false) advanced, enabling more complex multi-phase SOPs (Standard Operating Procedures).

**Major Open PRs in Progress:**
- **Provider Rewrite:** PR [#8854](https://github.com/zeroclaw-labs/zeroclaw/pull/8854) (refactor providers: typed builders) is a **size:XL** refactor unifying 15+ provider constructors, directly addressing the long-standing issue [#5937](https://github.com/zeroclaw-labs/zeroclaw/issues/5937).
- **Plugin System:** PR [#8661](https://github.com/zeroclaw-labs/zeroclaw/pull/8661) (feat plugins: execute WASM plugins out-of-process) is a **PROTOTYPE** PoC for running plugins in a sidecar process, aligning with the roadmap to move channels/tools to plugins.
- **Skills Overhaul:** PRs [#8313](https://github.com/zeroclaw-labs/zeroclaw/pull/8313) (feat skills: default to compact injection) and [#8638](https://github.com/zeroclaw-labs/zeroclaw/pull/8638) (feat skills: replace ClawHub with git-catalog) are reworking the entire skill injection and distribution model.

## 4. Community Hot Topics

1.  **RFC: Work Lanes & Board Automation [#6808](https://github.com/zeroclaw-labs/zeroclaw/issues/6808)**
    - **Activity:** 13 comments. Author: Audacity88.
    - **Analysis:** This is the most active issue. It is a governance RFC proposing automation to route work items without manual maintainer overhead. The high engagement suggests the community is deeply invested in project management structure as the codebase scales.

2.  **Refactor: Unify Providers Architecture [#5937](https://github.com/zeroclaw-labs/zeroclaw/issues/5937)**
    - **Activity:** 11 comments. Author: NiuBlibing.
    - **Analysis:** This is a critical architectural debt item. The community is heavily discussing the fragmentation in how providers (OpenAI, Anthropic, Ollama, etc.) handle `reqwest` clients and model parameters. The newly opened PR [#8854](https://github.com/zeroclaw-labs/zeroclaw/pull/8854) is the direct result of this RFC, showing strong maintainer response.

3.  **RFC: OpenAI Chat Completions Adapter [#8603](https://github.com/zeroclaw-labs/zeroclaw/issues/8603) & [#8550](https://github.com/zeroclaw-labs/zeroclaw/issues/8550)**
    - **Activity:** Combined 7 comments. Author: REL-mame.
    - **Analysis:** There is clear user demand for interoperability. Users want to use ZeroClaw as a drop-in backend for popular frontends like Open WebUI and LobeChat. The dedicated RFC and feature request signal this is a high-priority integration pain point.

## 5. Bugs & Stability

**Critical/High Severity (S1/S2) Issues Active:**

- **[Bug #5808](https://github.com/zeroclaw-labs/zeroclaw/issues/5808) (S1 - Workflow Blocked):** Default 32k context budget is exceeded by the system prompt on the first turn, causing a perpetual preemptive trim. **Risk: High.** This is a blocker for new users with default settings.
- **[Bug #8837](https://github.com/zeroclaw-labs/zeroclaw/issues/8837) (S2 - Degraded):** History trimming occurs silently even when history pruning is disabled, causing the agent to "forget" context mid-session. **Risk: High.** Marked as `needs-repro` and `needs-author-action`.
- **[Bug #8731](https://github.com/zeroclaw-labs/zeroclaw/issues/8731) (S2 - Degraded):** Stdio-based MCP servers accumulate as zombie processes under the daemon, degrading system resources over time. **Risk: High.** No fix PR currently linked.
- **[Bug #5628](https://github.com/zeroclaw-labs/zeroclaw/issues/5628) (S2 - Degraded):** Daemon installed as systemd service causes port conflicts when running manually. **Risk: High.** This is an old issue (April) that is still active, causing friction for developers.
- **[Bug #8410](https://github.com/zeroclaw-labs/zeroclaw/issues/8410) (S2 - Degraded):** Channel tasks cannot produce a silent "no-reply" outcome, forcing agents to send an unnecessary visible response. **Risk: High.**

**Recent Bug Fixes in Progress:**
- PR [#8796](https://github.com/zeroclaw-labs/zeroclaw/pull/8796) (fix zerocode: harden slash skill flow) targets TUI input stability.
- PR [#8845](https://github.com/zeroclaw-labs/zeroclaw/pull/8845) (fix zerocode: route live model switch) addresses a TUI misconfiguration where model changes via picker were not applying to active turns.

## 6. Feature Requests & Roadmap Signals

**Likely for Next Version (0.8.x → 0.9.0):**
- **Pluginification (RFC #8850):** The momentum to move channels and tools from compile-time features to runtime WASM plugins is very high. This will likely land in the next major release.
- **OpenAI Compatible Endpoint (RFC #8603 / Issue #8550):** Given the dedicated RFC and linked PRs, this is a strong candidate for a near-term feature.
- **A2A Agent Discovery (RFC #7218):** The `.well-known/agent-card.json` standard for multi-agent setups is being defined, signaling a shift toward interoperability.
- **Gemini Live Realtime Channel (RFC #8780):** An ambitious, optional channel for real-time audio-to-audio communication. This is a cutting-edge feature that may land as experimental.

**User-Requested Features:**
- **Per-Agent Prompt Injection Mode ([#7749](https://github.com/zeroclaw-labs/zeroclaw/issues/7749)):** Users want to run `full` and `compact` agents in the same install.
- **ZeroCode Config UI for JSON ([#8062](https://github.com/zeroclaw-labs/zeroclaw/issues/8062)):** Users find editing structured JSON config fields in the TUI difficult.
- **In-App Upgrade from Dashboard ([RFC #8170](https://github.com/zeroclaw-labs/zeroclaw/issues/8170)):** Users want a "Update & Restart" button in the web UI.

## 7. User Feedback Summary

- **Pain Point: Configuration Friction.** The bug regarding Telegram setup ([#8797](https://github.com/zeroclaw-labs/zeroclaw/issues/8797)) and the config editing UI ([#8062](https://github.com/zeroclaw-labs/zeroclaw/issues/8062)) indicate that initial configuration and ongoing management remain friction points.
- **Demand for Interop:** The push for the OpenAI-compatible endpoint ([#8550](https://github.com/zeroclaw-labs/zeroclaw/issues/8550), [#8603](https://github.com/zeroclaw-labs/zeroclaw/issues/8603)) reflects a user base that wants to use ZeroClaw as a backend for other tools, not just a standalone system.
- **Expectation of Stability:** The long-standing bugs around context budgeting ([#5808](https://github.com/zeroclaw-labs/zeroclaw/issues/5808)) and service management ([#5628](https://github.com/zeroclaw-labs/zeroclaw/issues/5628)) are likely eroding user confidence in running the daemon as a 24/7 service without manual intervention.

## 8. Backlog Watch

- **[Bug #5628](https://github.com/zeroclaw-labs/zeroclaw/issues/5628) (Daemon port conflict) - Created: 2026-04-11:** This is a **3-month-old** S2 (degraded behavior) bug. The `no-stale` label has been applied, but it has no linked fix PR. This represents a significant quality-of-life regression for developers.
- **[Bug #5808](https://github.com/zeroclaw-labs/zeroclaw/issues/5808) (32k Context Budget Overflow) - Created: 2026-04-16:** Another **3-month-old** S1 (workflow blocked) bug. While assigned and in status `in-progress`, the severity of this issue (breaking first-use experience) should demand more urgency.
- **PR [#6719](https://github.com/zeroclaw-labs/zeroclaw/pull/6719) (fix: persist model_switch) - Created: 2026-05-16:** This PR is **2 months old** and labeled `needs-author-action` and `stale-candidate`. The fix addresses a critical issue where switching models mid-conversation resets on the next turn. The project is at risk of losing this contribution if the author doesn't respond.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*