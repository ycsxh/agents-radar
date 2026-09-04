# OpenClaw Ecosystem Digest 2026-09-04

> Issues: 500 | PRs: 500 | Projects covered: 13 | Generated: 2026-09-04 04:02 UTC

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

# OpenClaw Project Digest — 2026-09-04

## 1. Today's Overview

OpenClaw is operating at extremely high activity: both issue updates and PR updates hit the 500-item data cap in the last 24h (339 open/active issues, 161 closed; 415 open PRs, 85 merged/closed). A new minor release, **v2026.9.1**, shipped today, featuring Mermaid diagram rendering across the Control UI and native apps plus an improved install-to-chat onboarding flow. However, the release also triggered at least one new P0 Windows regression ([#137813](https://github.com/openclaw/openclaw/issues/137813)). The maintainer team (steipete, zeroaltitude, and others) is pushing a large wave of correctness refactors and UI fixes, and issue triage is aggressive, but recurring themes — Windows upgrade reliability, SQLite growth/corruption, and channel message loss — continue to dominate the backlog.

## 2. Releases

**v2026.9.1** (`ad6fe23`) was published today. Only partial release notes were captured:

- **Diagrams in every chat:** Mermaid blocks now render as diagrams in the Control UI and native macOS/iOS/Android apps, with enlarge previews and a mobile retry path when rendering fails. (#134913, #135746, #135470, #135342)
- **From install to chat:** (onboarding/UX improvements — notes truncated in data)

No explicit breaking-change or migration section was captured, but early user reports identify two upgrade hazards:
- Windows Scheduled Task installs: the regenerated `gateway.cmd` adds a new `--task-supervisor` flag that exits 0 silently and never spawns the child — Gateway never starts ([#137813](https://github.com/openclaw/openclaw/issues/137813)).
- An incompatible installed Codex payload can fail model runtime plugin generation after upgrade ([#137748](https://github.com/openclaw/openclaw/issues/137748), fix PR [#137777](https://github.com/openclaw/openclaw/pull/137777) open).

**Migration caution:** Windows users on 2026.8.2 should verify the Gateway service actually starts after update; a reinstall of `@openai/codex` may be required.

## 3. Project Progress

85 PRs were merged/closed and 161 issues closed in the last 24h. Observed progress signals:

- **Maintainer-led refactors (behavior-preserving)** landed or are in final review: explicit Gateway system-job reconciliation ([#137875](https://github.com/openclaw/openclaw/pull/137875)), removal of redundant protocol schema maps ([#137852](https://github.com/openclaw/openclaw/pull/137852)), SQLite metadata-allocation avoidance for typed reads ([#137862](https://github.com/openclaw/openclaw/pull/137862)), and one-time diagnostic field normalization ([#137676](https://github.com/openclaw/openclaw/pull/137676)).
- **Several high-severity bugs advanced to closed state**, indicating fixes landed: Gateway startup guard regression ([#107694](https://github.com/openclaw/openclaw/issues/107694)), doctor `--fix` deadlock on migration gates ([#134938](https://github.com/openclaw/openclaw/issues/134938)), Windows doctor final-restart failure ([#137377](https://github.com/openclaw/openclaw/issues/137377)), Codex plugin missing `node_modules` ([#135970](https://github.com/openclaw/openclaw/issues/135970)), Discord/Codex terminal `message` tool ([#106961](https://github.com/openclaw/openclaw/issues/106961)), and OAuth MCP servers missing on `claude-cli` runtime ([#134307](https://github.com/openclaw/openclaw/issues/134307)).
- **UI/app-layer fixes in review**: exposing config mode state programmatically ([#137439](https://github.com/openclaw/openclaw/pull/137439)), Archive availability on Apple clients ([#137401](https://github.com/openclaw/openclaw/pull/137401)), correct primary user in multi-agent profile hero ([#136736](https://github.com/openclaw/openclaw/pull/136736)), and a Control UI blank-page fix behind Cloudflare Rocket Loader ([#136129](https://github.com/openclaw/openclaw/pull/136129)).
- **New capability PRs**: guarded autopilot for Workboard ([#137809](https://github.com/openclaw/openclaw/pull/137809)), provider-guided reconnect actions in the UI ([#137648](https://github.com/openclaw/openclaw/pull/137648)), and Slack native session-control routing to owning runs ([#137863](https://github.com/openclaw/openclaw/pull/137863)).

## 4. Community Hot Topics

Most-discussed issues (by comment count):

| Issue | Title | Comments | Signal |
|---|---|---|---|
| [#94518](https://github.com/openclaw/openclaw/issues/94518) (closed) | DeepSeek cache hit rate <10% after 6.x upgrade | 11 | 👍10 — cost-critical for a major provider |
| [#114612](https://github.com/openclaw/openclaw/issues/114612) | `memory_index_chunks`/`memory_embedding_cache` unbounded SQLite growth | 11 | Production disk-fill risk, no retention policy |
| [#97616](https://github.com/openclaw/openclaw/issues/97616) | Unreaped hook/tool child processes → zombie accumulation | 10 | Runtime degradation; fix PR linked |
| [#96007](https://github.com/openclaw/openclaw/issues/96007) | Discord drops content after inline error text | 9 | Message loss on a major channel |
| [#110190](https://github.com/openclaw/openclaw/issues/110190) | Runtime context carrier placed after user message confuses models | 9 | Wasted reasoning tokens, needs product decision |

**Underlying needs:** (1) cost-sensitive enterprises need provider cache semantics preserved across upgrades; (2) long-running deployments need bounded, self-cleaning storage; (3) channel integrations (Discord/Telegram/WhatsApp) must never silently swallow or misrender content; (4) users want deterministic, debuggable prompt assembly.

## 5. Bugs & Stability

**Newly reported this week (updated/created 2026-09-04), ranked:**

| Severity | Issue | Description | Status |
|---|---|---|---|
| 🔴 P0 | [#137813](https://github.com/openclaw/openclaw/issues/137813) | Windows Gateway never starts after v2026.9.1 — new `--task-supervisor` exits 0 silently, child never spawns | Open; no fix PR yet — release regression |
| 🟠 P1 | [#137710](https://github.com/openclaw/openclaw/issues/137710) | Native Codex completion recorded but does not wake a `sessions_yield` parent | Open; no fix PR yet |
| 🟡 P2 | [#137705](https://github.com/openclaw/openclaw/issues/137705) | Telegram streaming leaks raw `[label](file:///…)` Markdown when scheme isn't allowlisted | Open; security-adjacent |
| 🟢 Fixed | [#137377](https://github.com/openclaw/openclaw/issues/137377) | Windows doctor `--fix` final restart failure; Scheduled Task left disabled | Closed |

**Persisting severe regressions still open:**

- **SQLite corruption class:** WAL checkpoint overwriting SQLite page 1 on ext4 ([#123327](https://github.com/openclaw/openclaw/issues/123327), P0) and recurring freelist corruption even on pristine DBs, 5 events in 5 days ([#126821](https://github.com/openclaw/openclaw/issues/126821), P0). Also related: forced memory reindex inflated shared DB to 35 GB ([#135347](https://github.com/openclaw/openclaw/issues/135347)); a mitigation/docs PR ([#137876](https://github.com/openclaw/openclaw/pull/137876)) is open.
- **Process hygiene:** zombie accumulation from hook/tool children ([#97616](https://github.com/openclaw/openclaw/issues/97616), P1) and orphaned `node server.js` workers after subagent/cron runs ([#86119](https://github.com/openclaw/openclaw/issues/86119), P1).
- **Upgrade blockers:** Windows de-DE 2026.8.2 upgrade leaves doctor blocked ([#136203](https://github.com/openclaw/openclaw/issues/136203), P0) and SSH command-executor hang regression ([#136183](https://github.com/openclaw/openclaw/issues/136183), P1).
- **Message/session-state:** Discord truncation after errors ([#96007](https://github.com/openclaw/openclaw/issues/96007)), context carrier mispositioning ([#110190](https://github.com/openclaw/openclaw/issues/110190)), and double-written transcripts ([#118185](https://github.com/openclaw/openclaw/issues/118185)).

**Fix PRs in flight for open bugs:** #137777 (harness plugin load failure), #136533 (heartbeat sessions ignoring transcript byte cap), #137435 (visible child thinking persistence), #137030 (bound live streams / Codex startup drain), #137660 (Side chat cancellation during context prep), #137853 (personal cooldown loss in fallback summaries), #137834 (iMessage echo-cache false positives).

## 6. Feature Requests & Roadmap Signals

Active user-requested features:

| Issue | Request | Signal |
|---|---|---|
| [#137872](https://github.com/openclaw/openclaw/issues/137872) | Policy-bound prompt hooks should enumerate authorized tool names | New today; P3 |
| [#127208](https://github.com/openclaw/openclaw/issues/127208) | One-off `/followup <message>` command | Updated today |
| [#126781](https://github.com/openclaw/openclaw/issues/126781) | Durable "Lobster" workflows from `/loop` and Automations | Open since Aug 20 |
| [#132781](https://github.com/openclaw/openclaw/issues/132781) | Use latest commentary as progress draft label | P3 |
| [#116716](https://github.com/openclaw/openclaw/issues/116716) | Strict failure policy for non-default context engines | Stale; P3 |
| [#72741](https://github.com/openclaw/openclaw/issues/72741) | Standard interface for external security/guardrail checks | Since Apr 27 |

**Likely next-version candidates:** the Workboard guarded autopilot feature ([PR #137809](https://github.com/openclaw/openclaw/pull/137809)) and provider-guided reconnect UI ([PR #137648](https://github.com/openclaw/openclaw/pull/137648)) are already implemented and under review, so both could land in v2026.9.2. The cron shell-precheck gate ([#112375](https://github.com/openclaw/openclaw/pull/112375)) remains a long-lived candidate awaiting proof. Security-adjacent requests (guardrail interface, strict context-engine failure) are likely to be prioritized given the current focus on safety boundaries.

## 7. User Feedback Summary

**Recurring pain points:**

- **Windows is the weakest upgrade path** — across 2026.8.1, 2026.8.2, and now 2026.9.1, Windows users report doctor deadlocks, broken Scheduled Tasks, locale-specific blockers, and silent Gateway startup failures (e.g., [#136203](https://github.com/openclaw/openclaw/issues/136203), [#137813](https://github.com/openclaw/openclaw/issues/137813), [#137377](https://github.com/openclaw/openclaw/issues/137377)). Windows automation account "alternating device metadata approvals" also still occurs ([#127176](https://github.com/openclaw/openclaw/issues/127176)).
- **SQLite data durability and unbounded growth** worry production operators: an instance grew to 35 GB after a reindex; another saw recurring corruption on both x86 and Raspberry Pi/ARM (ext4); production users are asking for safe recovery guidance rather than just fixes ([#123799](https://github.com/openclaw/openclaw/issues/123799), [#114612](https://github.com/openclaw/openclaw/issues/114612)).
- **DeepSeek users are cost-angry**: the 6.x boundary-aware caching change dropped hit rates below 10%, directly raising spend ([#94518](https://github.com/openclaw/openclaw/issues/94518), 10 👍).
- **Silent failure modes frustrate users**: agent reports memory save success while persistence is disabled ([#126906](https://github.com/openclaw/openclaw/issues/126906)); Discord/Telegram drops or misrenders content; context window silently falls back to 200k instead of the real 1M ([#127239](https://github.com/openclaw/openclaw/issues/127239)).

**Satisfaction signals:** the project responded quickly to several P0s within 24h (107694, 134938, 137377 closed), and v2026.9.1's diagram rendering and onboarding improvements were positively framed in the release notes. Maintainers are actively labeling, reproducing, and queueing fixes.

## 8. Backlog Watch

Items needing maintainer attention or decisions:

| Issue | Age | Severity | Why it matters |
|---|---|---|---|
| [#86119](https://github.com/openclaw/openclaw/issues/86119) | Since May 24 | P1 🐚 | Orphaned `node server.js` workers accumulate after subagent/cron runs; no fix PR |
| [#96007](https://github.com/openclaw/openclaw/issues/96007) | Since Jun 23 | P1 🐚 | Discord message truncation; linked PR open but needs live repro/decision |
| [#97616](https://github.com/openclaw/openclaw/issues/97616) | Since Jun 29 | P1 🦪 | Zombie process accumulation; fix PR linked but needs review |
| [#110190](https://github.com/openclaw/openclaw/issues/110190) | Since Jul 17 | P1 🦞 | Runtime context carrier causes model confusion; needs product decision |
| [#114612](https://github.com/openclaw/openclaw/issues/114612) | Since Jul 27 | P2 🦞 | SQLite tables without retention policy; no fix PR yet |
| [#126821](https://github.com/openclaw/openclaw/issues/126821) | Since Aug 20 | P0 🦪 | Recurring SQLite corruption (5 events in 5 days); still open |
| [#127148](https://github.com/openclaw/openclaw/issues/127148) | Since Aug 21 | P1 🐚 | Codex compaction acquires second app-server; updated today |
| [#136203](https://github.com/openclaw/openclaw/issues/136203) | Since Sep 2 | P0 🦞 | Windows de-DE upgrade blocks doctor; release-blocker UX |
| [#136183](https://github.com/openclaw/openclaw/issues/136183) | Since Sep 2 | P1 🦪 | SSH command-executor hang regression, persists across 2026.8.1/8.2 |
| [#123799](https://github.com/openclaw/openclaw/issues/123799) | Since Aug 14 | P1 🦪 | Production deployment needs safe upgrade/backport guidance for Codex compact 404 |

---

*Digest compiled from openclaw/openclaw GitHub data for 2026-09-04. All issue links: [issues](https://github.com/openclaw/openclaw/issues); PR links: [pulls](https://github.com/openclaw/openclaw/pulls).*

---

## Cross-Ecosystem Comparison

# Cross-Project Comparison Report — Personal AI Assistant / Agent Open Source Ecosystem
**Date:** 2026-09-04 | **Data:** 24-hour community digest snapshots from 13 projects

---

## 1. Ecosystem Overview

The landscape is consolidating around OpenClaw as the reference runtime, surrounded by a long tail of "Claw"-branded specialists and adjacent desktop/platform agents sharing its channel-and-session vocabulary. Nine of 13 tracked projects showed activity in the window; four (NullClaw, TinyClaw, Moltis, ZeptoClaw) are currently dormant. The dominant engineering effort across the ecosystem has shifted from model capability to *integration plumbing*: channel adapter fidelity, SQLite durability and retention, Gateway↔UI state synchronization, provider cache semantics, and permission boundaries now consume the majority of issue and PR traffic. Model providers are treated as swappable commodities behind a shared context/cache API, and the clearest competitive signals are reliability, security governance, and upgrade safety rather than novel agent behavior.

---

## 2. Activity Comparison

*OpenClaw figures hit the digest's 500-item cap in both issue and PR tracking; actual volume is higher. Health score is an analyst composite of throughput, merge/close efficiency, release cadence, responsiveness, and unresolved-severity burden (1–10).*

| Project | Issues updated (open) | Issues closed | PRs updated (open) | PRs merged/closed | Release status | Health |
|---|---|---|---|---|---|---|
| **OpenClaw** | 500+ (339) | 161 | 500+ (415) | 85 | ✅ **v2026.9.1 shipped today** | 9 |
| **ZeroClaw** | 50 (36) | 14 | 50 (49) | 1 | None | 8 |
| **Hermes Agent** | 50 (49) | 1 | 50 (47) | 3 | None | 6 |
| **CoPaw (QwenPaw)** | 27 (19) | 8 | 36 (21) | 15 | None — v2.2.0 stable in verification | 7 |
| **IronClaw** | 11 (8) | 3 | 18 (8) | 10 | None | 8 |
| **NanoBot** | 4 (3) | 1 | 25 (11) | 14 | None — 0.3.0 regression pending fix | 7 |
| **NanoClaw** | 5 (4) | 1 | 23 (20) | 3 | None | 5 |
| **LobsterAI** | 6 (4) | 2 | 15 (5) | 10 | None — 2026.8.31 / 2026.9.4 lines in flight | 6 |
| **PicoClaw** | 6 (5) | 1 | 8 (7) | 1 | None | 5 |
| NullClaw / TinyClaw / Moltis / ZeptoClaw | 0 | 0 | 0 | 0 | None | 1 |

**Key observations:**
- OpenClaw alone closed 161 issues and merged 85 PRs in 24 hours — exceeding the *total open item counts* of every other active project.
- IronClaw shows the strongest merge efficiency (10 of 18 PRs closed) despite modest volume.
- NanoBot demonstrates an exceptionally clean issue→fix→merge loop (both new WebUI bugs already paired with fix PRs).
- PicoClaw and NanoClaw show bottleneck risk: active community PRs sit unreviewed (PicoClaw #3340/#3347; NanoClaw's 6-PR provider-contract stack, only 3 merged total).
- ZeroClaw's 49-open-PRs vs. 1-merged-PR ratio is misleading in isolation — it closed 14 issues, indicating closure through stacked/in-flight branches and a disciplined RFC process.

---

## 3. OpenClaw's Position

**Advantages over peers:**
- **Scale advantage (~10×):** 500+ issues and 500+ PRs touched in 24h versus the 25–50 seen in the next tier. It operates at a volume where community triage, labeling, and rapid P0 response (three P0s closed within 24h this snapshot) are institutionalized.
- **Only daily shipper:** v2026.9.1 is the sole release published ecosystem-wide today. LobsterAI pins `openclaw.version` in its commercial product — direct evidence OpenClaw functions as an embedded reference runtime downstream.
- **Breadth of surface:** Control UI + native macOS/iOS/Android apps, doctor/CLI, scheduled tasks, 10+ messaging channels, Codex/model-runtime plugins, OAuth MCP servers. No peer covers this full matrix.
- **Ecosystem gravity:** the "Claw" naming and shared concepts (channels, gateway, sessions, Doctor) across ZeroClaw/PicoClaw/NanoClaw and dependencies in LobsterAI indicate it sets the mental model for the category.

**Technical approach differences:**
- Node-centric Gateway + service/scheduled-task operation model (`gateway.cmd`, `--task-supervisor`), SQLite shared store with embeddings and metadata-heavy typed reads — a *production data-layer posture* peers haven't reached (its 35 GB reindex incident is a scale problem most projects are too small to hit).
- Mermaid rendering and native-app delivery show investment in end-user presentation — uncommon among the more headless peers.

**Weak flanks:** Windows upgrade reliability is a recurring release-regression source (three consecutive releases with Windows P0s, including today's #137813), and persistent SQLite corruption P0s (#123327, #126821) remain unresolved — both areas where smaller Rust/Go implementations may claim advantage.

**Community size vs. peers:** OpenClaw's single-day issue closures (161) exceed Hermes/ZeroClaw's total touched items (50 each); NanoBot's four touched issues would be lost in OpenClaw's triage queue. The nearest "governed" communities (ZeroClaw's RFC process, Hermes' maintainer responsiveness) operate at roughly 10% of OpenClaw's raw volume.

---

## 4. Shared Technical Focus Areas

Requirements now emerging independently across multiple projects:

| Focus Area | Projects | Specific shared needs |
|---|---|---|
| **SQLite / local-data durability & lifecycle** | OpenClaw, NanoClaw, ZeroClaw | Corruption recovery on ext4; unbounded growth/retention policies (`memory_*` tables); WAL/lock ordering (`busy_timeout` before `journal_mode`); safe compaction; test isolation |
| **Windows & installer reliability** | OpenClaw, LobsterAI, Hermes | Scheduled-task/gateway startup failures; doctor deadlocks; DPI/console-window polish; non-interrupting app updates; locale-specific blockers |
| **Channel attachment & delivery fidelity** | OpenClaw, PicoClaw, CoPaw, NanoBot, ZeroClaw | Slack zero-size uploads; WeCom base64 images; Discord truncation after errors; Matrix stream failure propagation; Feishu stuck consumers; QQ auth breaks; approval-origin validation |
| **Gateway↔client state synchronization** | NanoBot, OpenClaw, Hermes, CoPaw, IronClaw | Stale spinner/reconnect states; session-title projection; hook registration under `serve`; Desktop/Web parity; session switching while streaming |
| **Provider cache affinity & context budgeting** | OpenClaw, NanoBot, IronClaw, Hermes, ZeroClaw, CoPaw | DeepSeek/Codex/Claude cache-hit preservation; cache-key headers (`session-id`, `prompt_cache_key`); dynamic context-window budgets; `context_length` config honoring; `x-opencode-session` compatibility |
| **Runtime security & governance boundaries** | ZeroClaw, CoPaw, OpenClaw, Hermes, IronClaw | Granular FS sandbox policy; verifiable-intent/credential-chain checks; approval gates reaching the right owner; guardrail/security-check interfaces; rejecting forged `approved` fields |
| **Agent memory reliability & observability** | CoPaw, OpenClaw, NanoBot, ZeroClaw | Background embedding jobs failing silently; unbounded vector tables; context-reuse visualization; memory status endpoints; persona preservation during compaction |
| **Sub-agent & cron lifecycle hygiene** | OpenClaw, Hermes, IronClaw, CoPaw | Orphaned workers/zombies; invisible child approval gates; cron dispatch under systemd; duplicate misfire execution; delivery-mode contracts |

---

## 5. Differentiation Analysis

| Project | Feature focus | Target user | Architectural signature |
|---|---|---|---|
| **OpenClaw** | Breadth-first all-in-one personal agent | Developers/operators building production personal assistants | Node/TS Gateway + SQLite core + native/web/chat surfaces; plugin runtimes (Codex, MCP, OAuth) |
| **ZeroClaw** | Security-hardened, governed agent core | Security-sensitive operators/enterprise | Rust; verifiable-intent, granular sandbox policies (Bubblewrap/Landlock/Seatbelt), ACP transcript/turn persistence, public RFC-tracker governance |
| **Hermes Agent** | Desktop "Bot Mode" prosumer agent | Desktop-centric power users, multi-profile | `serve`/Desktop backend with `config.yaml` shell hooks; unified slash-command ambition; mixture-of-agents config surface |
| **IronClaw** | Sandboxed agent infrastructure platform | Agent-infra engineers (nearai ecosystem) | Persistent per-user sandbox executor behind trusted kernel; TypeScript API-boundary validation; model-quality eval separated from infra defects |
| **CoPaw (QwenPaw)** | Memory-rich, team-ready agent hub | Advanced/Chinese-speaking, multi-user teams | ReMe/PowerContext long-term memory; Feishu/WeCom channels; v2.2.0 multi-tenant Hub; `SOUL.md` persona system |
| **LobsterAI** | Commercial consumer desktop wrapper | Mainstream Chinese desktop users | Electron app embedding/auto-managing the OpenClaw runtime (`openclaw.version` pin); in-app Agent Browser with MCP bridge; installer/update safety focus |
| **PicoClaw** | Lightweight edge/single-board agent | Hobbyists/SBC ARM users | Go single binary; local RKLLM models; channel integrations (Slack/QQ/LINE/IRC) |
| **NanoClaw** | Channel-adapter matrix & delivery contracts | Developers wiring many chat adapters | TypeScript; `@chat-adapter/*` dependency stack; declarative provider contracts; voice transcription V2 |
| **NanoBot** | Lean headless gateway | Researchers/resellers needing clean embeddable agent | Small WebUI + Matrix/Signal channels; cron lifecycle; Codex cache affinity; per-request context-reuse visualization |
| NullClaw / TinyClaw / Moltis / ZeptoClaw | — | — | Dormant/experimental |

---

## 6. Community Momentum & Maturity

**Tier 1 — Release-scale, extremely high momentum:** **OpenClaw**. Only project shipping daily releases; operational at a volume that strains its own digest tooling. Risk: release-triggered Windows regressions and SQLite corruption P0s accumulate despite throughput.

**Tier 2 — High-velocity hardening / pre-release:** **ZeroClaw, IronClaw, CoPaw, Hermes**. ZeroClaw shows the most mature governance (visible RFC decision queue, accepted-security-work closures) with heavy contributor throughput. IronClaw is rapidly stabilizing (TypeScript cleanup, main-branch regressions fixed same-window). CoPaw is feature-dense and pre-v2.2.0-verification with unresolved security/memory issues. Hermes responds same-day to P1s but carries an open P0 and duplicate Desktop-hook/gap reports signaling regression-management debt.

**Tier 3 — Stabilizing or review-bound:** **NanoBot, LobsterAI, NanoClaw, PicoClaw**. NanoBot is in a healthy stabilization phase post-0.3.0 (clean fix loops, one regression remaining). LobsterAI merges release-line PRs steadily but has March/April concurrency bugs and community PRs still untouched. NanoClaw's large provider-contract refactor stack is active but low-merge — staleness risk rising. PicoClaw is functionally blocked on maintainer review: core channels (QQ, Slack media) are broken while fix PRs sit open.

**Tier 4 — Dormant:** NullClaw, TinyClaw, Moltis, ZeptoClaw. Zero activity; treat as unmaintained or paused.

---

## 7. Trend Signals

*Implications extracted from cross-project community feedback; value for AI agent developers:*

| Trend | Evidence across projects | Implication for developers |
|---|---|---|
| **Context/cache economics are the #1 production cost lever** | DeepSeek cache hit-rate anger (OpenClaw), Codex cache-affinity fixes (NanoBot, IronClaw), `api_content` sidecar persistence (Hermes), prompt-budget accounting (IronClaw) | Build cache-aware prompt assembly and expose context/cache metrics from day one; silent cache-schema regressions directly hit users' wallets |
| **Permission/security expectations are moving from prompt-level to policy-level** | Sandbox-breach report (CoPaw), verifiable-intent chain verification (ZeroClaw), self-approval schema removal (ZeroClaw), shell-hook safety guards silently absent (Hermes), guardrail-interface requests (OpenClaw) | Agents need auditable runtime policy layers (sandbox, approval routing, hook registration) — users no longer trust prompt instructions as safety |
| **Local-first data durability is a trust moat** | SQLite corruption classes, unbounded memory tables, 35 GB reindex (OpenClaw); `busy_timeout` ordering and destructive test fixtures (NanoClaw) | Ship with WAL discipline, retention policies, and safe recovery/backport guidance before scale finds the bugs for you |
| **Silent failure is the most damaging failure mode** | False "memory saved" report (OpenClaw), hooks that never fire (Hermes), unhealthy `/health` as healthy (ZeroClaw), blank tool outputs with Langfuse (CoPaw) | Every state transition (stream end, hook registration, approval, persistence) needs an explicit, observable acknowledgment |
| **Windows delivery remains the enterprise weak flank** | Gateway never starts post-upgrade (OpenClaw #137813), doctor deadlocks/locale blockers, DPI/console/update-interrupt fixes (LobsterAI), Windows `rg`/path fixes (Hermes) | If you target enterprise desktops, treat Windows Scheduled Tasks, installers, and service startup as first-class CI-tested surfaces |
| **Gateway↔UI state synchronization is core reliability, not polish** | Spinner-stall after reconnect (NanoBot), blank Control UI behind CDN (OpenClaw), stale "Connected" after daemon exit (ZeroClaw), Desktop/Web parity gaps (CoPaw, Hermes) | Stream/status protocols need idempotent `onRunStatus`-style state reconciliation; session handoff bugs erode user trust faster than model errors |
| **Memory is becoming a pluggable first-class service** | ReMe embedding failures (CoPaw), PowerContext backend registry (CoPaw), unbounded SQLite memory tables (OpenClaw), context-reuse visualization (NanoBot), persona-preserving compaction (CoPaw, ZeroClaw) | Design memory as swappable, bounded, observable backends with status endpoints — not as hidden side tables |
| **Remote/mobile management of desktop agents is an emerging expectation** | Mobile remote-connection requests (CoPaw), iOS PWA fixes (NanoBot), native Apple clients (OpenClaw) | Prosumer/desktop agents will soon be judged by how well they can be supervised from a phone |

---

**Bottom line:** The ecosystem is maturing from "demo agents" into *operationally deployed agent infrastructure*. OpenClaw defines the category's breadth and pace but is vulnerable on Windows reliability and data-layer durability. ZeroClaw and IronClaw represent the strongest engineering-discipline counterpoints (security/Rust governance and sandbox/type-boundary rigor respectively). For new entrants, the highest-value whitespace is not another agent loop — it is durable local storage, permission-enforced hook/sandbox boundaries, cache-aware context assembly, and honest state observability across gateway, web, desktop, and mobile.

---

## Peer Project Reports

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

# NanoBot Project Digest — 2026-09-04

## 1. Today's Overview

NanoBot shows a high-velocity bugfix cadence: 25 PRs were updated in the last 24 hours, with **14 merged/closed** and 11 still open, while only 4 issues saw updates (3 open, 1 closed). No new releases were cut on this date. The merged work clusters around WebUI reliability, channel-specific fixes (Matrix, Signal), and provider/Codex edge cases — a pattern suggesting a stabilization phase after the 0.3.0 release, which also introduced at least one confirmed regression (#5645). Community discussion volume is low (≤1 comment per issue), but issue-to-PR pairing is strong: both newly reported WebUI bugs already have fix PRs in review.

---

## 2. Releases

**No new releases** for 2026-09-04. (Previous release context: issue #5645 indicates nanobot-ai 0.3.0 introduced a runtime-context regression relative to 0.2.2, so a follow-up patch release may be imminent.)

---

## 3. Project Progress

**Merged/Closed PRs in the last 24h (14 total)** — highlights:

### WebUI / Frontend
- **[#5514](https://github.com/HKUDS/nanobot/pull/5514) — fix(webui): clear stale stream state after Gateway reconnect** — closes the long-standing "spinning state" bug (#5512) by subscribing to `onRunStatus` updates instead of only live chat events.
- **[#5650](https://github.com/HKUDS/nanobot/pull/5650) — fix(webui): preserve Hero model preset during chat creation** — carries the Hero-selected model into optimistic sessions for first messages, with regression coverage.
- **[#5646](https://github.com/HKUDS/nanobot/pull/5646) — fix(webui): show language names only in their native form** — simplifies the language picker and removes unused English display names from the locale registry.

### Channels (Matrix / Signal)
- **[#5334](https://github.com/HKUDS/nanobot/pull/5334) — fix(channels): preserve indentation across message splits** — stops `lstrip()` from corrupting indented long messages; keeps Signal UTF-16 offsets aligned.
- **[#5637](https://github.com/HKUDS/nanobot/pull/5637) — fix(matrix): propagate stream delivery failures** — allows Matrix `send_delta()` failures to flow into the channel manager retry policy.
- **[#5385](https://github.com/HKUDS/nanobot/pull/5385) — fix(matrix): complete Element SAS request flow** — accepts modern `m.key.verification.request` events and properly rejects stale/conflicting requests.
- **[#5472](https://github.com/HKUDS/nanobot/pull/5472) — fix(signal): honor wildcard in inbound allowlists** — supports `*` in DM and group allowlists with receive-path regression tests.

### Providers / SDK / Agent
- **[#5413](https://github.com/HKUDS/nanobot/pull/5413) — fix(providers): apply fallback policy to raised errors** — closes an escape hatch where raised exceptions bypassed the LLM fallback chain.
- **[#5632](https://github.com/HKUDS/nanobot/pull/5632) — fix(provider): preserve Codex prompt cache affinity** — aligns `session-id` header and `prompt_cache_key` using a stable SHA-256-derived routing key.
- **[#5635](https://github.com/HKUDS/nanobot/pull/5635) — fix(sdk): preserve queued events on stream close** — prevents dropping the oldest unread event when closing a full stream queue.
- **[#5515](https://github.com/HKUDS/nanobot/pull/5515) — fix(agent): observe session reply timeout task failures** — surfaces message-bus failures from background timeout tasks instead of silently discarding them.
- **[#5629](https://github.com/HKUDS/nanobot/pull/5629) — fix(tool_hints): respect max_length for plain (non-path/non-command) tool values** — fixes over-long `grep` patterns, search queries, and globs in tool hint formatting.

---

## 4. Community Hot Topics

Comment/reaction volume is low overall (issues show ≤1 comment; PR threads are not recorded in this dataset), but two items generated the most user engagement:

- **[Issue #5644](https://github.com/HKUDS/nanobot/issues/5644) — Channel locale registry drops a locale on concurrent load** *(1 comment, open)* — reporter `top777` identified a real race condition at startup where `loadChannelLocale()` captures the per-channel map before `await`, so two concurrently loading locales can overwrite each other, silently losing a locale (e.g., `en`). A fix PR was opened the next day.
- **[Issue #5512](https://github.com/HKUDS/nanobot/issues/5512) — WebUI stalls in spinning state after Gateway restart** *(1 comment, closed)* — user-reported hang caused by the frontend never receiving the final `goal_status: idle` push. Closed by **[PR #5514](https://github.com/HKUDS/nanobot/pull/5514)**, demonstrating a clean user-report → fix → merge loop.

Underlying need: users are sensitive to **state synchronization between Gateway and WebUI** (stream status, session handoff, model presets) — the dominant theme across both issue traffic and merged PRs.

---

## 5. Bugs & Stability

Ranked by severity:

1. **Regression: Current Time runtime context absent by default in 0.3.0** — **[Issue #5645](https://github.com/HKUDS/nanobot/issues/5645)** *(open, no fix PR yet)*. High impact: a documented default behavior (`ContextBuilder.build_messages()` auto-injecting current time) silently changed between 0.2.2 and 0.3.0, affecting any agent relying on time-aware reasoning. No linked fix yet — needs maintainer triage.
2. **Concurrency bug: channel locale registry drops locales** — **[Issue #5644](https://github.com/HKUDS/nanobot/issues/5644)** *(open)*. Startup race can permanently drop a locale. Fix exists in **[PR #5651](https://github.com/HKUDS/nanobot/pull/5651)** (registers the map synchronously before awaiting), currently open.
3. **WebUI: session title not generated when frontend envelope lacks `webui` flag** — **[Issue #5647](https://github.com/HKUDS/nanobot/issues/5647)** *(open)*. Edge-case regression from #5528's `target_session_key` projection. Fix proposed in **[PR #5648](https://github.com/HKUDS/nanobot/pull/5648)**.
4. **WebUI stalls in spinning state after Gateway restart** — **[Issue #5512](https://github.com/HKUDS/nanobot/issues/5512)** *(closed)*. Resolved by merged **[PR #5514](https://github.com/HKUDS/nanobot/pull/5514)**.

---

## 6. Feature Requests & Roadmap Signals

- **[PR #5620](https://github.com/HKUDS/nanobot/pull/5620) — feat(cron): support configurable delivery and batch archive** *(open since 09-01)* — adds explicit per-cron delivery targets, batch-archive lifecycle state, and WebUI management. The largest feature in flight; if merged, cron jobs gain keep-or-archive semantics.
- **[PR #5649](https://github.com/HKUDS/nanobot/pull/5649) — feat(webui): visualize per-request context reuse** *(open)* — moves token usage out of assistant messages into a composer popover with stacked per-request context bars and persistent history. Signals a roadmap push toward **context-window observability**.
- **[PR #5639](https://github.com/HKUDS/nanobot/pull/5639) — fix: stabilize session labels, TUI streaming, and pairing prompts** *(open)* — includes an OpenTUI upgrade (0.5.3 → 0.5.10) to keep fenced code visible after stream completion; points to continued terminal-UI polish.
- **[PR #5641](https://github.com/HKUDS/nanobot/pull/5641) — fix(webui): iOS PWA tap and status-bar fixes** *(open)* — addresses iOS Safari `:hover`-chain tap swallowing, improving mobile PWA usability.

Prediction: context-reuse visualization (#5649) and cron lifecycle management (#5620) are strong candidates for the next minor release, alongside the pending 0.3.0 regression fix for #5645.

---

## 7. User Feedback Summary

- **Stability frustration (resolved):** WebUI hanging in a spinner after Gateway restarts (#5512) was a top user pain point; fixed via #5514, restoring confidence in reconnect behavior.
- **Silent configuration regression:** A user of nanobot-ai 0.3.0 (#5645) reported lost Current Time injection, an undocumented behavior change from 0.2.2 that breaks time-sensitive agent turns — a satisfaction risk for upgraders.
- **Internationalization correctness:** Report #5644 shows users running multi-locale channel setups hit data loss in translation registries at startup; a user-fixable race that also produced a community-submitted fix (PR #5651).
- **Preset/model expectations:** Hero chat creation no longer losing the selected model preset (#5650) addresses a UX round-trip annoyance during session handoff.

Overall sentiment: users are actively testing 0.3.0 edge cases and getting quick turnaround from both maintainers and external contributors — a healthy sign, though the #5645 regression suggests release validation gaps around runtime-context defaults.

---

## 8. Backlog Watch

- **[PR #5446](https://github.com/HKUDS/nanobot/pull/5446) — fix(codex): persist OAuth tokens in Nanobot data directory** *(open since 08-19, tagged `conflict`)* — needs maintainer attention: worthwhile fix for Codex token portability, but requires rebase/conflict resolution after two weeks.
- **[PR #5504](https://github.com/HKUDS/nanobot/pull/5504) — fix(ui): surface model retry status (NAN-34)** *(open since 08-24)* — large WebSocket/TUI/WebUI change; no update since 09-03. Watch for review latency.
- **[PR #5528 context]** — Issue #5647 points at #5528 as the origin of a title-projection regression; that PR has been merged, but its side effects are still generating follow-up fixes (#5648).
- **No unanswered issues are stale** — the oldest open issue (#5644) already has a fix PR, and none of the 4 tracked issues lack either a fix or a linked PR; the primary backlog risk is review throughput on older open PRs (#5446, #5504).

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent Project Digest — 2026-09-04

## Today's Overview

Activity remains high: 50 issues and 50 PRs were touched in the last 24 hours, with 49 issues still open/active and 47 PRs still open. One issue and three PRs reached a closed/merged state in this window. No release was published on 2026-09-04. The busiest areas are Desktop/`serve` hook and plugin registration gaps, cron dispatch compatibility, custom-provider auth/context-length bugs, and session/profile isolation. Multiple same-day fix PRs were opened against newly reported P1 bugs, indicating responsive maintainer review, but one P0 issue and several P1s remain without visible fixes.

## Releases

None.

## Project Progress

Three PRs were merged/closed in the window; the visible example is:

- [NousResearch/hermes-agent#77157](https://github.com/NousResearch/hermes-agent/pull/77157) — `fix(search)`: zero-match probes now fall back to `grep`, and native Windows `rg` path handling is fixed. This closes a long-standing `search_files` path-boundary issue on Windows.

One issue was also closed:

- [NousResearch/hermes-agent#15779](https://github.com/NousResearch/hermes-agent/issues/15779) — `/model` switch to a named custom provider ignored `custom_providers[].models.<model>.context_length`. This config-compatibility bug is now closed; related hardening continues in [PR #102645](https://github.com/NousResearch/hermes-agent/pull/102645).

## Community Hot Topics

Most-discussed items from the top 30 issues:

- [NousResearch/hermes-agent#96692](https://github.com/NousResearch/hermes-agent/issues/96692) — 11 comments. Spec for a unified slash-command registry and execution contract across CLI, gateway, TUI, plugins, and Desktop. Needs decision; the core demand is one predictable extension mechanism across all surfaces.

- [NousResearch/hermes-agent#69825](https://github.com/NousResearch/hermes-agent/issues/69825) — 7 comments. Shell hooks configured in `config.yaml` are parsed and pass `hermes hooks` checks but never fire in the Desktop `serve` backend because `register_from_config` is never called. This is a safety/trust issue for users relying on outcome-send or destructive-command guards.

- [NousResearch/hermes-agent#94726](https://github.com/NousResearch/hermes-agent/issues/94726) — 6 comments, 1 👍. Umbrella tracker for Desktop Bot Mode bugs. It demonstrates that many Bot Mode issues cluster into recognizable classes rather than isolated regressions.

- [NousResearch/hermes-agent#100858](https://github.com/NousResearch/hermes-agent/issues/100858) — 6 comments. Auxiliary vision with `custom:<name>` provider plus `base_url` sends `no-key-required` instead of the real API key, resulting in 401s. Related duplicate: [NousResearch/hermes-agent#76602](https://github.com/NousResearch/hermes-agent/issues/76602).

- [NousResearch/hermes-agent#97296](https://github.com/NousResearch/hermes-agent/issues/97296) — 5 comments. macOS 27 kanban dispatcher workers crash with SIGSEGV after `Popen(start_new_session=True)` forks the threaded gateway; worker logs are 0 bytes and PIDs are not alive at 60s.

- [NousResearch/hermes-agent#70422](https://github.com/NousResearch/hermes-agent/issues/70422) — 5 comments, 1 👍. Desktop composer can be accidentally dragged/pop-out merely by selecting text. Duplicate [NousResearch/hermes-agent#101318](https://github.com/NousResearch/hermes-agent/issues/101318) shows continued user frustration.

## Bugs & Stability

Top bugs visible this period, ranked by severity:

| Severity | Issue | Description | Fix status |
|---|---|---|---|
| P0 | [NousResearch/hermes-agent#102194](https://github.com/NousResearch/hermes-agent/issues/102194) | CLI path never persists the `api_content` sidecar; `<memory-context>`/tool-result decorations are dropped at turn boundaries, so the first API call of every new turn misses prompt cache. | No PR visible yet. |
| P1 | [NousResearch/hermes-agent#102486](https://github.com/NousResearch/hermes-agent/issues/102486) | systemd 249 rejects `OOMPolicy=kill`, making every restart-safe cron worker dispatch fail closed. | [PR #102655](https://github.com/NousResearch/hermes-agent/pull/102655) opened same day. |
| P1 | [NousResearch/hermes-agent#102526](https://github.com/NousResearch/hermes-agent/issues/102526) | Desktop `HERMES_HOME` override race can bind the backend to another profile's `state.db`, causing the default bot to open the wrong profile's chat. | No PR visible yet. |
| P1 | [NousResearch/hermes-agent#102574](https://github.com/NousResearch/hermes-agent/issues/102574) | Shared `PeriodicScheduler` runs callbacks inline on one thread; one blocked callback stalls turn-liveness, lease, and heartbeat timers. | No PR visible yet. |
| P1 | [NousResearch/hermes-agent#102504](https://github.com/NousResearch/hermes-agent/issues/102504) | `hermes serve` skips `_prepare_agent_startup` for the `serve` command, so config shell hooks never register. Duplicate of #69825. | No PR visible yet. |
| P2 | [NousResearch/hermes-agent#102592](https://github.com/NousResearch/hermes-agent/issues/102592) | Plugin-registered hooks (`pre_llm_call`, `post_llm_call`, …) never fire on `serve`/`dashboard` because plugin discovery is skipped at startup. | No PR visible yet. |
| P2 | [NousResearch/hermes-agent#76602](https://github.com/NousResearch/hermes-agent/issues/76602) / [NousResearch/hermes-agent#100858](https://github.com/NousResearch/hermes-agent/issues/100858) | Auxiliary vision with named custom provider + `base_url` downgrades credentials to `no-key-required`; providers return 401. | [PR #67055](https://github.com/NousResearch/hermes-agent/pull/67055) remains open. |
| P2 | [NousResearch/hermes-agent#70422](https://github.com/NousResearch/hermes-agent/issues/70422) / [NousResearch/hermes-agent#101318](https://github.com/NousResearch/hermes-agent/issues/101318) | Desktop composer still undocks on small drag gestures while selecting text; no disable option exists. | No PR visible yet. |
| P2 | [NousResearch/hermes-agent#98645](https://github.com/NousResearch/hermes-agent/issues/98645) | Valid `clarify` tool calls render as blank cards in Desktop, forcing 10-minute timeouts. | No PR visible yet. |
| P2 | [NousResearch/hermes-agent#101091](https://github.com/NousResearch/hermes-agent/issues/101091) | Desktop accepts mismatched provider/model/base_url combinations and injects the model into the wrong provider group. | No PR visible yet. |
| P2 | [NousResearch/hermes-agent#100870](https://github.com/NousResearch/hermes-agent/issues/100870) | Terminal brace-group rewriter omits a separator after `}`, breaking remote code-kernel spawns on the Docker backend. | No PR visible yet. |
| P2 | [NousResearch/hermes-agent#100855](https://github.com/NousResearch/hermes-agent/issues/100855) | `browser_exec` and real-profile daemons do not set `AGENT_BROWSER_SOCKET_DIR`, making them invisible to orphan reaping; a wedged daemon survived 47h across restarts. | [PR #100865](https://github.com/NousResearch/hermes-agent/pull/100865) open. |
| P2 | [NousResearch/hermes-agent#100381](https://github.com/NousResearch/hermes-agent/issues/100381) | `codex_app_server_auto=hermes` compaction triggers off a local mirror estimate, compacting tiny threads and thrashing long-lived sessions. | No PR visible yet. |
| P2 | [NousResearch/hermes-agent#100315](https://github.com/NousResearch/hermes-agent/issues/100315) | Codex reasoning-only events can indefinitely retain compression summary-progress authority. | No PR visible yet. |
| P3 | [NousResearch/hermes-agent#97296](https://github.com/NousResearch/hermes-agent/issues/97296) | Gateway-embedded kanban dispatcher crashes on macOS 27 when forking a threaded gateway. | No PR visible yet. |

The old closed issue [NousResearch/hermes-agent#15779](https://github.com/NousResearch/hermes-agent/issues/15779) indicates progress on custom-provider `context_length` handling, and [NousResearch/hermes-agent#102645](https://github.com/NousResearch/hermes-agent/pull/102645) is open to cover the compressor lazy path.

## Feature Requests & Roadmap Signals

Strong signals for future development:

- [NousResearch/hermes-agent#96692](https://github.com/NousResearch/hermes-agent/issues/96692) — Unified, versioned slash-command registry and invocation/result contract across every Hermes surface. This is the largest roadmap signal and is marked `needs-decision`.

- [NousResearch/hermes-agent#102643](https://github.com/NousResearch/hermes-agent/issues/102643) — Internationalization for slash-command descriptions; users want localized help text in the Desktop command popup.

- [NousResearch/hermes-agent#102597](https://github.com/NousResearch/hermes-agent/issues/102597) — Per-profile markers on session rows in the All-profiles recents list for easier multi-profile navigation.

- [NousResearch/hermes-agent#102582](https://github.com/NousResearch/hermes-agent/issues/102582) — Expose per-slot reasoning effort in `hermes moa configure`, since the schema already supports it.

- [NousResearch/hermes-agent#77952](https://github.com/NousResearch/hermes-agent/issues/77952) — Restore the last selected Desktop session when switching profiles.

- [NousResearch/hermes-agent#91329](https://github.com/NousResearch/hermes-agent/issues/91329) — Manage Bot Mode group membership directly from Group settings.

Likely near-term fixes in a next patch release are cron/systemd compatibility ([#102486](https://github.com/NousResearch/hermes-agent/issues/102486)), hook registration under `serve` ([#69825](https://github.com/NousResearch/hermes-agent/issues/69825) and duplicates), and custom-provider context length. The slash-command unification and multi-profile UI features are stronger candidates for a later minor release.

## User Feedback Summary

Users are reporting several high-friction, repeated problems:

- The Desktop composer drag/detach behavior is a continuing UI annoyance. Reports [#70422](https://github.com/NousResearch/hermes-agent/issues/70422) and [#101318](https://github.com/NousResearch/hermes-agent/issues/101318) describe accidental undocking while selecting text, with no setting to disable it.

- Users who disable Reasoning Blocks still see the full agent trace, including reasoning tokens and tool calls, in Desktop. [NousResearch/hermes-agent#93817](https://github.com/NousResearch/hermes-agent/issues/93817) calls this “unusable” at the reported priority.

- Users who configure config-file shell hooks as safety guards are discovering those guards are silently absent in Desktop/serve mode. The cluster around [NousResearch/hermes-agent#69825](https://github.com/nousresearch/hermes-agent/issues/69825), [#102504](https://github.com/NousResearch/hermes-agent/issues/102504), and [#102592](https://github.com/NousResearch/hermes-agent/issues/102592) is especially important because users believe protection is active when it is not.

- Custom vision/provider setups with `base_url` are failing with 401s because the wrong API key token is sent. Duplicate reports [#76602](https://github.com/NousResearch/hermes-agent/issues/76602) and [#100858](https://github.com/NousResearch/hermes-agent/issues/100858) suggest this is a common real-world configuration.

- There is platform-specific pain on Windows and macOS: Studio/Agent Bridge timeouts on Windows cold start, and macOS gateway crashes in kanban dispatch. These remain visibility concerns for cross-platform adoption.

Overall satisfaction is hard to infer from the tracker, but the high number of duplicate bug reports on Desktop hooks and composer behavior points to user frustration with regressions that persist across releases.

## Backlog Watch

Long-running or high-importance items that still need maintainer attention:

- [NousResearch/hermes-agent#31003](https://github.com/NousResearch/hermes-agent/pull/31003) — Security fix to reject redacted-secret placeholders in `write_file`/`patch` input; opened May 2026 and still not merged.

- [NousResearch/hermes-agent#69825](https://github.com/NousResearch/hermes-agent/issues/69825) — Desktop `serve` never registers shell hooks; open since July 2026 and now has newer duplicates. This should probably be treated as a root-cause priority.

- [NousResearch/hermes-agent#67055](https://github.com/NousResearch/hermes-agent/pull/67055) — Vision/named-provider transport preservation fix directly relevant to the 401 custom-provider bug cluster; open since July 2026.

- [NousResearch/hermes-agent#80138](https://github.com/NousResearch/hermes-agent/pull/80138) — Refactor extracting `auxiliary_client.py` fallback policy logic; marked P3 and `needs-decision`, open since August 2026.

- [NousResearch/hermes-agent#96692](https://github.com/NousResearch/hermes-agent/issues/96692) — Unified slash-command registry spec; highest-comment issue and `needs-decision`. A maintainer decision on scope will unblock multiple roadmap surfaces.

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

## PicoClaw Project Digest — 2026-09-04

### 1. Today's Overview

As of 2026-09-04, PicoClaw shows moderate but busy activity: 6 issues and 8 PRs were touched in the last 24 hours, leaving 5 issues open/active and 7 PRs open. No new release was published. Activity is mostly routine dependency bumps from Dependabot, but there is meaningful community work on Slack media uploads and Web UI lag. Project health signals are mixed: channel integrations remain fragile, while several user-submitted fixes are waiting for maintainer review or merge.

### 2. Releases

No new releases were published for PicoClaw on 2026-09-04. All release-related activity in the data is currently pending on open pull requests.

### 3. Project Progress

**Closed/merged PRs:**

- [PR #3329 — fix(line): warn on inert webhook_host / webhook_port instead of seeding them](https://github.com/sipeed/picoclaw/pull/3329)  
  This PR was closed in the last 24 hours. It addresses LINE channel settings that are declared, defaulted, and env-bound but never actually read. The fix warns users about inert `webhook_host` / `webhook_port` values instead of silently seeding them.

**Open but significant PRs:**

- [PR #3340 — fix(slack): set FileSize on media upload params](https://github.com/sipeed/picoclaw/pull/3340)  
  Fixes Slack media uploads by setting `FileSize` in `slack.UploadFileParameters`, avoiding client-side zero-size rejections.

- [PR #3347 — fix laggy interface](https://github.com/sipeed/picoclaw/pull/3347)  
  Addresses Web UI lag when chat history becomes long. The author reports successful manual testing on desktop and mobile Brave browsers.

**Dependency-bump PRs opened by Dependabot:**

- [PR #3364 — aws-sdk-go-v2 1.42.0 → 1.45.1](https://github.com/sipeed/picoclaw/pull/3364)
- [PR #3362 — golang.org/x/term 0.44.0 → 0.45.0](https://github.com/sipeed/picoclaw/pull/3362)
- [PR #3363 — ergochat/irc-go 0.6.0 → 0.7.0](https://github.com/sipeed/picoclaw/pull/3363)
- [PR #3361 — google.golang.org/protobuf 1.36.11 → 1.36.12](https://github.com/sipeed/picoclaw/pull/3361)
- [PR #3360 — larksuite/oapi-sdk-go/v3 3.9.4 → 3.11.0](https://github.com/sipeed/picoclaw/pull/3360)

### 4. Community Hot Topics

- [Issue #3281 — Web UI chat input is very laggy when history has a little bit long](https://github.com/sipeed/picoclaw/issues/3281)  
  The most active item, with 9 comments and 2 👍 reactions. Users consistently report that long chat history makes the Web UI input box nearly unusable. The linked fix PR [#3347](https://github.com/sipeed/picoclaw/pull/3347) has been submitted but is still open.

- [Issue #3338 — Slack does not attach image media content](https://github.com/sipeed/picoclaw/issues/3338)  
  Slack media uploads fail because `SendMedia` never sets `FileSize`. A fix PR [#3340](https://github.com/sipeed/picoclaw/pull/3340) is available.

- [Issue #3349 — QQ channel cannot be used](https://github.com/sipeed/picoclaw/issues/3349)  
  QQ channel auth returns `401 Authorization参数格式错误`, blocking the channel on Docker and Linux x86 versions. New root-cause analysis is tracked in [#3365](https://github.com/sipeed/picoclaw/issues/3365).

- [Issue #3339 — Antigravity generation returns generic 429 despite valid OAuth scopes and successful model discovery](https://github.com/sipeed/picoclaw/issues/3339)  
  A closed issue with 3 comments. Users report valid authentication but persistent “Resource has been exhausted” errors on every generation request.

Underlying community demand is clear: users need stable channel integrations, reliable provider error messages, and a Web UI that remains responsive with realistic conversation history.

### 5. Bugs & Stability

Ranked by severity:

1. **High — QQ channel completely nonfunctional**  
   [Issue #3349](https://github.com/sipeed/picoclaw/issues/3349) reports QQ failure with `failed to get websocket info: code:401`.  
   [Issue #3365](https://github.com/sipeed/picoclaw/issues/3365), opened 2026-09-04, identifies likely root cause: `botgo v0.2.1` plus `resty >= v2.17`. No fix PR exists yet.

2. **High — Slack media upload always fails**  
   [Issue #3338](https://github.com/sipeed/picoclaw/issues/3338) reports `file.upload.v2: file size cannot be 0`. Fix PR [#3340](https://github.com/sipeed/picoclaw/pull/3340) is open but not merged.

3. **Medium — Web UI input lag with long history**  
   [Issue #3281](https://github.com/sipeed/picoclaw/issues/3281) affects daily Web UI use. PR [#3347](https://github.com/sipeed/picoclaw/pull/3347) proposes a fix and has been tested by its author, but remains open.

4. **Medium — RKLLM model produces abnormal responses on ARM hardware**  
   [Issue #3346](https://github.com/sipeed/picoclaw/issues/3346) reports unexpected RKLLM replies on an ARM development board. No resolution or maintainer diagnosis appears in the provided data.

5. **Low / closed — Google Antigravity false 429s**  
   [Issue #3339](https://github.com/sipeed/picoclaw/issues/3339) was closed but the summary data does not show a user-facing fix. The symptom is provider-side error ambiguity rather than an auth failure.

### 6. Feature Requests & Roadmap Signals

There are no explicit new feature-request items in this 24-hour window, but several roadmap signals can be inferred from fix-oriented work:

- **Hybrid/local hardware support remains important.**  
  The RKLLM ARM issue ([#3346](https://github.com/sipeed/picoclaw/issues/3346)) and the QQ-on-Orange-Pi investigation ([#3365](https://github.com/sipeed/picoclaw/issues/3365)) show real deployment of PicoClaw on small ARM devices.

- **Config validation and user-facing warnings are desired.**  
  Closed PR [#3329](https://github.com/sipeed/picoclaw/pull/3329) would warn instead of silently seeding inert LINE settings. This points toward clearer configuration transparency.

- **Web UI rendering performance needs optimization.**  
  PR [#3347](https://github.com/sipeed/picoclaw/pull/3347) is likely to be important for the next release if maintainers review and merge it.

- **Channel API compatibility needs better dependency coupling.**  
  QQ’s botgo + resty conflict ([#3365](https://github.com/sipeed/picoclaw/issues/3365)) will probably require either an old `resty` pin or an upgrade of `botgo`.

Probably next version will absorb at least the Slack FileSize fix and possibly the Web UI lag fix if both PRs get maintainer attention.

### 7. User Feedback Summary

Real user pain points visible in this dataset:

- Web UI sessions with long histories become sluggish, hurting core chat usability ([#3281](https://github.com/sipeed/picoclaw/issues/3281)).
- Slack users cannot attach images at all because uploads are rejected before network transmission ([#3338](https://github.com/sipeed/picoclaw/issues/3338)).
- QQ-channel users are fully blocked by authorization errors; the issue is broad enough to appear in Docker, Linux x86, and ARM environments ([#3349](https://github.com/sipeed/picoclaw/issues/3349), [#3365](https://github.com/sipeed/picoclaw/issues/3365)).
- ARM/RKLLM users report incorrect AI replies, affecting local-model use cases ([#3346](https://github.com/sipeed/picoclaw/issues/3346)).
- Google Antigravity provider users find generic 429s confusing despite valid auth ([#3339](https://github.com/sipeed/picoclaw/issues/3339)).

Satisfaction is hard to measure from issues alone, but the presence of user-submitted fix PRs such as [#3347](https://github.com/sipeed/picoclaw/pull/3347) indicates a community willing to contribute fixes. The main dissatisfaction is around delays in maintainer review/merge of those fixes.

### 8. Backlog Watch

Issues and PRs that appear to need maintainer attention:

- [Issue #3281 — Web UI input lag](https://github.com/sipeed/picoclaw/issues/3281)  
  Open since 2026-07-21, with 9 comments and a ready fix PR [#3347](https://github.com/sipeed/picoclaw/pull/3347). This is one of the highest-impact stale issues currently open.

- [PR #3340 — Slack FileSize fix](https://github.com/sipeed/picoclaw/pull/3340)  
  Open since 2026-08-17 with no visible maintainer interaction in the data. It directly fixes a known Slack media bug ([#3338](https://github.com/sipeed/picoclaw/issues/3338)).

- [PR #3347 — Web UI lag fix](https://github.com/sipeed/picoclaw/pull/3347)  
  Open since 2026-08-27 and updated 2026-09-04. It addresses the most-commented issue in this digest and deserves review.

- [Issue #3349 / #3365 — QQ channel failure and root-cause analysis](https://github.com/sipeed/picoclaw/issues/3365)  
  QQ remains broken. The new root-cause report from 2026-09-04 should trigger dependency triage.

- [Issue #3346 — RKLLM abnormal replies](https://github.com/sipeed/picoclaw/issues/3346)  
  Open, stale-labeled, and with no visible maintainer response. ARM/RKLLM behavior likely needs reproduction help.

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw Digest — 2026-09-04

## Today’s Overview

NanoClaw shows a mixed activity day: issue volume is modest but PR volume is high. In the last 24 hours, 5 issues were updated — 4 still open and 1 closed — while 23 PRs were updated, with 20 remaining open and 3 closed/merged. No new release was published. The dominant activity is a large still-open provider-contract refactor series from zvi-fried, alongside SQLite/agent-runner reliability work from davekim917 and test-infrastructure cleanup from mmv. Most freshly reported bugs have no visible maintainer response yet, so community-facing review is likely the current bottleneck.

## Releases

None.

The latest-releases list is empty for this window, so there are no changelog entries, breaking-change notes, or migration instructions to report.

## Project Progress

Three PRs reached closed/merged status in the period; two are visible in the provided subset:

- [PR #3461 — chore(deps): bump all @chat-adapter/* + chat 4.29.0 -> 4.38.1](https://github.com/nanocoai/nanoclaw/pull/3461)  
  Closes a 9-minor-version gap across all channel adapter packages and the shared `chat` dependency. Keeps the `/add-<channel>` skill copies aligned with current adapter behavior.

- [PR #3126 — fix(agent-runner): never deliver silence, never deliver `<internal>` thinking](https://github.com/nanocoai/nanoclaw/pull/3126)  
  Changes agent-runner delivery rules so empty/silent blocks and internal thinking blocks are not sent onward as if they were user-visible content.

The third closed/merged PR is not visible in the top-20 subset. No feature-adding PR was shown as merged in this window; the larger feature work — provider contracts, Cursor support, and delivery-mode plumbing — remains open and under active update.

## Community Hot Topics

- [Issue #3706 — `ncl groups config add-mount` silently produces a broken double-nested path when `--container` is an absolute path](https://github.com/nanocoai/nanoclaw/issues/3706)  
  This is the only issue with any comment activity in the set, and it captures a common CLI ergonomics problem: the help text says “container path” without saying it must be relative, so an intuitive absolute path silently corrupts the resulting mount layout.

- [Provider-contract refactor cluster (#3581, #3584, #3585, #3586, #3588, #3591)](https://github.com/nanocoai/nanoclaw/pull/3581)  
  Although these PRs have no visible comments/reactions, the number of updated PRs signals heavy internal momentum around making provider behavior declarative, validated, and core-owned. The underlying need is broader provider support without weakening core guarantees.

- [PR #2003 — voice transcription V2, container-side](https://github.com/nanocoai/nanoclaw/pull/2003)  
  A long-running feature PR updated again in this window. It remains a notable user-facing desire: sovereign, container-local voice transcription.

## Bugs & Stability

Ranked roughly by severity:

- **High — [Issue #3705: `ncl tasks update --recurrence` doesn’t recompute `process_after`](https://github.com/nanocoai/nanoclaw/issues/3705)**  
  Changing a task from, e.g., weekly to daily leaves the next fire time on the old schedule. This is a user-visible task-scheduling bug with no visible fix PR yet.

- **Medium-High — [Issue #3706: absolute `--container` path creates a double-nested mount path](https://github.com/nanocoai/nanoclaw/issues/3706)**  
  The CLI accepts absolute paths despite implying relative-path semantics, then silently generates a broken path. No fix PR is visible.

- **Medium — [Issue #3709: SQLite tests use a fixed `/tmp` fixture root, causing concurrent vitest runs to delete each other’s databases](https://github.com/nanocoai/nanoclaw/issues/3709)**  
  A developer-experience and CI-isolation issue for worktrees running the same mailbox tests. [PR #3710](https://github.com/nanocoai/nanoclaw/pull/3710) helps by cleaning up leftover temp directories, but does not fully solve the fixed-root race.

- **Low — [Issue #3426: `send_card` docs promise callback buttons that the bridge drops](https://github.com/nanocoai/nanoclaw/issues/3426)**  
  Closed during this window. The bug made agents blame the platform when unsupported actions vanished.

Stability-focused PRs also active:

- [PR #3708 — set `busy_timeout` before `journal_mode` on outbound SQLite open](https://github.com/nanocoai/nanoclaw/pull/3708)  
  Prevents avoidable locking failures when `journal_mode` takes an exclusive lock.

- [PR #3440 — docker-driver: SELinux-blocked mounts and group-writable rw mounts](https://github.com/nanocoai/nanoclaw/pull/3440)  
  Fixes container mount failures under SELinux and a stray NUL byte issue.

## Feature Requests & Roadmap Signals

- [Issue #3704 — protected session-assembly hook on `SqliteAgentMailbox`](https://github.com/nanocoai/nanoclaw/issues/3704)  
  A fork maintainer is requesting an official protected seam for subclassing, rather than relying on the existing singular `compose.ts` slot. If accepted, this would be a deliberate extension API for mailbox implementations.

- [PR #3713 — record a per-agent-group delivery mode](https://github.com/nanocoai/nanoclaw/pull/3713)  
  Adds plumbing to record which delivery contract an agent group should follow. Nothing reads it yet, but it is groundwork for groups that cannot use the `<message to>` envelope contract.

- [PR #3711 — defer expensive inbound content until an agent will receive it](https://github.com/nanocoai/nanoclaw/pull/3711) and [PR #3712 — WhatsApp caption/media fixes](https://github.com/nanocoai/nanoclaw/pull/3712)  
  These point toward lazy inbound-content resolution and better channel-specific attachment handling.

- [PR #3355 and PR #3356 — Cursor provider payload + `/add-cursor` skill](https://github.com/nanocoai/nanoclaw/pull/3355)  
  Still open but likely to land after or alongside the provider-contract refactor series.

Likely next-version candidates: the delivery-mode groundwork (#3713), inbound lazy content (#3711), and the remaining provider-contract refactors (#3581/#3584/#3588/#3591), since they are already deeply prepared and are being updated actively.

## User Feedback Summary

Real user pain points captured in this window:

- **CLI validation gaps**: Users naturally pass absolute paths when the flag description does not require relative paths, and the resulting configuration fails silently.  
  → [Issue #3706](https://github.com/nanocoai/nanoclaw/issues/3706)

- **Task recurrence surprises**: Updating a task’s cron expression does not update the next scheduled fire, forcing users to manually fix or recompute schedules.  
  → [Issue #3705](https://github.com/nanocoai/nanoclaw/issues/3705)

- **Fork/extension friction**: A maintainer running a fork with custom SQLite tables and triggers wants a protected hook instead of fragile subclassing of the stock mailbox.  
  → [Issue #3704](https://github.com/nanocoai/nanoclaw/issues/3704)

- **Developer environment pain**: Fixed temp paths make concurrent test runs destructive, and full test runs leave hundreds of temp directories behind.  
  → [Issue #3709](https://github.com/nanocoai/nanoclaw/issues/3709), [PR #3710](https://github.com/nanocoai/nanoclaw/pull/3710)

No strong positive user sentiment was visible in this data slice; the signal is overwhelmingly about bugs, documentation mismatches, and missing extension seams.

## Backlog Watch

- [PR #2003 — voice transcription V2](https://github.com/nanocoai/nanoclaw/pull/2003)  
  Open since 2026-04-25, updated again on 2026-09-03. This is the oldest feature PR in the visible set and needs either review momentum or an explicit roadmap decision.

- [PR #3355 and #3356 — Cursor provider support](https://github.com/nanocoai/nanoclaw/pull/3355)  
  Open since 2026-08-19. The provider work has not been merged, and its long open lifetime increases merge risk as trunk evolves.

- [Provider-contract refactor stack #3581/#3584/#3585/#3586/#3588/#3591](https://github.com/nanocoai/nanoclaw/pull/3581)  
  Open since 2026-08-27 with many interdependent pieces. These need maintainer review/sequencing attention to avoid becoming stale or conflicting with each other.

- [Issue #3704](https://github.com/nanocoai/nanoclaw/issues/3704) is not old, but it has zero comments; a maintainer response is needed to determine whether the proposed session-assembly hook is a desired project direction.

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw Project Digest — 2026-09-04

## Today’s Overview

The reporting window ending 2026-09-04 showed strong sustained activity: 11 issues updated (8 open/active, 3 closed), 18 PRs updated (8 open, 10 merged/closed), and no releases published. The closed PR set was concentrated in WebUI TypeScript hardening, API boundary typing, loop-host performance, and CI/test stabilization. Two main-branch test regressions were fixed during this window, unblocking other work. Open PRs point toward the next focus areas: model-context-window budgeting, prompt-cache key propagation, and subagent approval-flow improvements.

## Releases

No new releases were published during this window.

## Project Progress

**Merged/closed PRs in the last 24h:**

- **WebUI TypeScript cleanup and API-boundary validation**
  - [#8037](https://github.com/nearai/ironclaw/pull/8037) — Removed 40 redundant `@ts-nocheck` directives and added a suppression ratchet.
  - [#8038](https://github.com/nearai/ironclaw/pull/8038) — Typed and validated frontend API boundaries with runtime decoders and request guards.
  - [#8039](https://github.com/nearai/ironclaw/pull/8039) — Removed `@ts-nocheck` from 64 production components/hooks.
  - [#8040](https://github.com/nearai/ironclaw/pull/8040) — Typed WebUI test infrastructure and removed all 94 test-side suppressions.
  - These close the corresponding cleanup issues [#8033](https://github.com/nearai/ironclaw/issues/8033), [#8035](https://github.com/nearai/ironclaw/issues/8035), and [#8036](https://github.com/nearai/ironclaw/issues/8036).

- **Loop/performance fixes**
  - [#8043](https://github.com/nearai/ironclaw/pull/8043) — Coalesced streamed text updates instead of re-sanitizing full text per delta, removing O(N·k) behavior.
  - [#7984](https://github.com/nearai/ironclaw/pull/7984) — Sized `tool_search` replies to the model’s first-look envelope instead of an independent budget.

- **Subagent workflow**
  - [#8046](https://github.com/nearai/ironclaw/pull/8046) — A child subagent’s approval/auth gate now reaches the owner’s inbox instead of staying invisible.

- **CI/test stabilization**
  - [#8055](https://github.com/nearai/ironclaw/pull/8055) — Fixed the WebUI asset test that was leaving `main` red and blocking open PRs.
  - [#8058](https://github.com/nearai/ironclaw/pull/8058) — Fixed the Reborn test failure by using the live extension id instead of the retired `"web-push"` spelling.
  - [#8060](https://github.com/nearai/ironclaw/pull/8060) — Gave whole-tree architecture scans real timeout headroom after previous runs nearly hit the hard kill.

## Community Hot Topics

The most-commented issues were both architectural/observability concerns rather than simple bug reports:

- [#7903](https://github.com/nearai/ironclaw/issues/7903) — *Decision spike: persistent per-user sandboxed executor behind the trusted host kernel* (2 comments). The underlying need is to preserve IronClaw’s authority boundary while avoiding host-to-sandbox plumbing for every new CLI command.

- [#8009](https://github.com/nearai/ironclaw/issues/8009) — *MCP egress errors flatten to `"response_error"`* (1 comment). The core need is diagnostic fidelity: discovery failures currently discard the underlying reason and byte counts, making hosted-MCP failures impossible to triage.

No PR discussion counts or reaction data were reported in this slice, so issue comment volume is the primary community signal available.

## Bugs & Stability

Ranked by severity:

1. **Main-branch test regressions**
   - [#8055](https://github.com/nearai/ironclaw/pull/8055) and [#8058](https://github.com/nearai/ironclaw/pull/8058) both fixed red-main conditions caused by earlier WebUI/API-boundary changes. Both PRs are closed, so this stability issue appears resolved.

2. **CI timeout risk**
   - [#8060](https://github.com/nearai/ironclaw/pull/8060) addressed whole-tree architecture scans that were finishing within seconds of the 180s hard kill. Fixed in the closed PR.

3. **Opaque MCP egress errors**
   - [#8009](https://github.com/nearai/ironclaw/issues/8009) remains open. Diagnostics are flattened to a single token, removing reason codes and byte counts. No fix PR is open yet.

4. **Potential host-api panic**
   - [#8056](https://github.com/nearai/ironclaw/pull/8056) fixes malformed embedded tool-result text causing a panic via unchecked byte-range slicing. Open fix PR exists.

5. **Responses cancel endpoint always fails**
   - [#8059](https://github.com/nearai/ironclaw/pull/8059) reports `POST /api/v1/responses/{id}/cancel` returning `400 invalid_request` for in-progress and completed runs, with an invalid hardcoded cancel reason. Open fix PR exists.

6. **WebUI slash-command UI bugs**
   - [#8066](https://github.com/nearai/ironclaw/issues/8066) — Command result cards collapse to border-only lines when results accumulate.
   - [#8063](https://github.com/nearai/ironclaw/issues/8063) — Active command can scroll out of view in the slash-command menu.
   - [#8064](https://github.com/nearai/ironclaw/issues/8064) — Command result cards cannot be dismissed.
   - [#8065](https://github.com/nearai/ironclaw/issues/8065) — Command metadata alignment is inconsistent.
   - These are open issues without fix PRs yet.

7. **Failure taxonomy**
   - [#8052](https://github.com/nearai/ironclaw/issues/8052) reports the officeqa suite’s 63 non-passes are genuine model-quality errors over OCR’d Treasury Bulletins, not an IronClaw infrastructure defect.

## Feature Requests & Roadmap Signals

- **Persistent sandbox architecture**
  - [#7903](https://github.com/nearai/ironclaw/issues/7903) is a high-risk decision spike about persistent per-user sandboxed executors. It is still open and likely needs a design/architecture decision.

- **Prompt-context budgeting**
  - [#8057](https://github.com/nearai/ironclaw/issues/8057) asks that the prompt budget account for identity, skills, tool schemas, and other non-transcript material.
  - [#8053](https://github.com/nearai/ironclaw/pull/8053) derives the loop’s context budget from the model’s advertised context window rather than a compiled-in constant.

- **LLM prompt-cache work**
  - [#8062](https://github.com/nearai/ironclaw/pull/8062) sends stable conversation cache keys on OpenAI Responses/Chat Completions paths.
  - [#8044](https://github.com/nearai/ironclaw/pull/8044) replaces the Claude-family allowlist with a denylist so newer families do not silently lose prompt caching.

- **Subagent concurrency and gating**
  - [#8046](https://github.com/nearai/ironclaw/pull/8046) advanced the R3 subagent inbox work.
  - [#8061](https://github.com/nearai/ironclaw/pull/8061) adds a concurrent-children cap and verifies replay of child-gate approval cards.

- **Slash-command UX**
  - Issues [#8063](https://github.com/nearai/ironclaw/issues/8063), [#8064](https://github.com/nearai/ironclaw/issues/8064), [#8065](https://github.com/nearai/ironclaw/issues/8065), and [#8066](https://github.com/nearai/ironclaw/issues/8066) cluster around the same feature area: slash-command menu navigation, dismissal, alignment, and result-card behavior.

If the open PRs merge, the next version is likely to include dynamic prompt-context budgeting, OpenAI-compatible cache-key propagation, Claude-family cache fixes, and WebUI slash-command polish.

## User Feedback Summary

Concrete user-reported pain points in this window:

- Slash-command result cards accumulate without a dismiss action and eventually collapse to thin borders, consuming conversation space.
- The slash-command menu is hard to scan because command names have inconsistent widths, and the active selection can leave the visible viewport.
- MCP discovery failures are undiagnosable because errors collapse to `"response_error"` with no underlying reason or byte counts.
- Prompt-budget accounting can let the assembled provider request exceed the loop’s intended context budget.
- Telegram users who start with `/start` receive the available-commands inventory instead of the pairing/connect notice; the fix is proposed in [#8054](https://github.com/nearai/ironclaw/pull/8054).
- A child subagent blocked on an approval or credential gate was previously invisible to its owner until completion; that is fixed by [#8046](https://github.com/nearai/ironclaw/pull/8046).

No direct satisfaction metrics are available in this data slice, but the issue mix is constructive: most reports are specific UI defects, diagnostic-quality gaps, or clearly-scoped enhancements.

## Backlog Watch

- [#7903](https://github.com/nearai/ironclaw/issues/7903) — Persistent sandboxed executor decision spike; open since 2026-08-26, high-risk, only 2 comments. Needs maintainer attention or a formal design outcome.
- [#8009](https://github.com/nearai/ironclaw/issues/8009) — Open since 2026-08-31 with only 1 comment; this is an observability gap affecting MCP integrations and needs a maintainer decision on error propagation.
- [#7988](https://github.com/nearai/ironclaw/pull/7988) — Nightly codebase knowledge-graph refresh PR open since 2026-08-29. Automated PRs still need review/merge or explicit closure.
- [#8044](https://github.com/nearai/ironclaw/pull/8044) — Claude prompt-cache denylist fix has been open since 2026-09-02 and is important for avoiding silent cache downgrades on newer model families.

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# LobsterAI Project Digest — 2026-09-04

## 1. Today's Overview

LobsterAI showed strong short-term delivery activity: 15 PRs were updated in the last 24h, with 10 closed/merged, and 6 issues were updated, of which 4 remain open. No new GitHub release was published in the snapshot. The project appears to be in a release-heavy engineering cycle: release prep for `2026.8.31` closed, changes for the `2026.9.4` line landed, and several Windows/installer and update-flow fixes were merged. Overlapping with that, a number of older issues and PRs have been marked stale, while several concurrency and version-policy concerns from March/April are still waiting for maintainer attention.

## 2. Releases

No new releases were published in the last 24h. The latest public release metadata for the project is still empty in this snapshot, so there are no changelogs, breaking changes, or migration notes to report.

## 3. Project Progress

The closed PR batch indicates focused work across release prep, desktop UX, installer reliability, and OpenClaw/dsh integration cleanup.

**Release preparation**
- [PR #2600](https://github.com/netease-youdao/LobsterAI/pull/2600) — Release prep for `2026.8.31`: guided first-run experience, faster Library browsing, support for sharing model-generated videos, clearer login/quota messaging, and stronger Windows installer recovery.
- [PR #2602](https://github.com/netease-youdao/LobsterAI/pull/2602) — Restored the interactive in-app Agent Browser for the `2026.9.4` release line, including the browser MCP bridge, persistent browser profile, encrypted saved credentials, approval-gated autofill, manual credential capture, and settings management.

**Update/app lifecycle safety**
- [PR #2609](https://github.com/netease-youdao/LobsterAI/pull/2609) — Update install now confirms before interrupting an active agent turn/scheduled task; quitting the app also prompts for confirmation. Mid-download cancel behavior was removed in favor of silent-download-then-notify.

**Windows/installer fixes**
- [PR #2605](https://github.com/netease-youdao/LobsterAI/pull/2605) — Declared the Windows installer DPI-aware to fix blurry icons.
- [PR #2606](https://github.com/netease-youdao/LobsterAI/pull/2606) — Launched helper processes without a console window on Windows.

**OpenClaw/dsh integration cleanup**
- [PR #2607](https://github.com/netease-youdao/LobsterAI/pull/2607) — Removed dsh MCP-server registration and delegated coding paths, dropping `dshCodeMcpServer`/`dshSessionClient`, the McpRuntime resolution hook, and obsolete OpenClaw config re-syncing, which also prevents plugin-bundle bloat.
- [PR #2608](https://github.com/netease-youdao/LobsterAI/pull/2608) — Follow-up docs/main PR for removing dsh MCP delegation.

**UI and i18n polish**
- [PR #2599](https://github.com/netease-youdao/LobsterAI/pull/2599) — Improved IM bot-card layout: multi-instance bot cards are limited to two responsive columns, empty add-bot cards stay compact, and card content is vertically centered.
- [PR #2603](https://github.com/netease-youdao/LobsterAI/pull/2603) — Refined the Chinese voice-input quota-exhausted message with clearer free-trial/subscription wording and compact duration formatting.
- [PR #2604](https://github.com/netease-youdao/LobsterAI/pull/2604) — Gave the exhausted voice-input button a stable dimmed state while keeping it clickable for the quota prompt, plus test coverage.

## 4. Community Hot Topics

The most active item by comment count is a stale-closed documentation bug:

- [Issue #1556](https://github.com/netease-youdao/LobsterAI/issues/1556) — **IM机器人配置指南 404** (3 comments). A user-reported broken link to the official IM bot configuration guide `https://lobsterai.youdao.com/LobsterAI-IM机器人配置指南.md`. It was created on 2026-04-08 and auto-closed as stale on 2026-09-03. Underlying need: documentation links in public docs should be continuously validated; a dead setup guide disrupts onboarding and integration.
- [Issue #1552](https://github.com/netease-youdao/LobsterAI/issues/1552) — **AI产物 Markdown 预览及文件卡片支持** (2 comments). Requests inline FileCards and previews after Write-tool calls, so users do not have to make the agent read files back or manually switch to the file manager. Closed as stale, but the use case is still visible in daily agent workflows.
- [Issue #2601](https://github.com/netease-youdao/LobsterAI/issues/2601) — **Support rendering MCP Apps / Prefab UI in the desktop client**. This is the most significant newly active issue: MCP servers using the `io.modelcontextprotocol/ui` extension can return interactive HTML UIs via `ui://` resources, but the desktop client does not render them. It reflects increasing demand for rich MCP-driven UI components inside the desktop client.

The snapshot also includes several open concurrency/security-relevant issues with one comment each, discussed in the next section.

## 5. Bugs & Stability

Ranked by potential severity:

1. **High — [Issue #1089](https://github.com/netease-youdao/LobsterAI/issues/1089): `CoworkRunner` lacks reentrancy protection.** `startSession()` and `continueSession()` are called fire-and-forget over IPC. Rapid messages or IM gateway batch delivery can cause two async loops on the same `sessionId`, leading to corrupted stream messages, duplicated messages, and shared-state races in `ActiveSession`. No fix PR is visible in the current data.
2. **High — [Issue #1088](https://github.com/netease-youdao/LobsterAI/issues/1088): Prefetch callback does not validate `turnToken`.** At `openclawRuntimeAdapter.ts:3809-3814`, prefetch channel messages can resume after the active turn has changed, potentially injecting stale user messages into the wrong turn. No linked fix is visible yet.
3. **Medium — [Issue #1082](https://github.com/netease-youdao/LobsterAI/issues/1082): OpenClaw version pinning risk.** The user reports `package.json → openclaw.version: v2026.3.2` and asks whether newer OpenClaw versions are supported. They mention national/security compliance requirements to update software to the latest version. This is a version-policy and trust concern rather than a crash.
4. **Low/UX — [Issue #1556](https://github.com/netease-youdao/LobsterAI/issues/1556): Broken IM configuration guide URL.** The documentation link 404s. It was closed as stale, which is not the same as being fixed.
5. **Fixed in the current batch — [PR #2609](https://github.com/netease-youdao/LobsterAI/pull/2609)** closes the risk that a scheduled task or agent turn is silently interrupted by update/install actions and app quit. Also [PR #2605](https://github.com/netease-youdao/LobsterAI/pull/2605) and [PR #2606](https://github.com/netease-youdao/LobsterAI/pull/2606) address Windows installer polish issues.

There are also two old bug-fix PRs still open:
- [PR #1087](https://github.com/netease-youdao/LobsterAI/pull/1087) — Fixes duplicate error messages when `continueSession()` fails.
- [PR #1081](https://github.com/netease-youdao/LobsterAI/pull/1081) — Fixes mixed-language MCP sync messages and scrollbar overflow in MCP edit dialogs.

## 6. Feature Requests & Roadmap Signals

The strongest roadmap signal is [Issue #2601](https://github.com/netease-youdao/LobsterAI/issues/2601): support MCP Apps / Prefab-style interactive HTML UIs in the desktop client. It points to ecosystem alignment with the emerging `io.modelcontextprotocol/ui` standard.

Other user-driven work waiting in the pipeline:

- [Issue #1552](https://github.com/netease-youdao/LobsterAI/issues/1552) asks for FileCards and built-in preview of agent-generated Markdown/HTML/code files. Although stale-closed, it captures a real gap in file-artifact workflows.
- [PR #1078](https://github.com/netease-youdao/LobsterAI/pull/1078) would push IM alert notifications when scheduled tasks fail, solving the current asymmetric success/failure notification behavior.
- [PR #1079](https://github.com/netease-youdao/LobsterAI/pull/1079) proposes a “current process” right-side panel in Cowork sessions, showing tool executions and red/green diff views for file writes/edits.
- [PR #1081](https://github.com/netease-youdao/LobsterAI/pull/1081) is primarily a bug-fix/UX PR, but it also improves full i18n coverage for MCP sync messages.

Predictions: the `2026.9.4` release line will likely include the restored in-app browser plus the app-update/quit safety changes already merged. MCP Apps rendering is less likely to land in the immediate next release unless it is already in progress, since [Issue #2601](https://github.com/netease-youdao/LobsterAI/issues/2601) was only opened in the last 24h.

## 7. User Feedback Summary

Real user pain points expressed in the updated items:

- **Documentation reliability is hurting quickstart onboarding.** [Issue #1556](https://github.com/netease-youdao/LobsterAI/issues/1556) shows an official configuration guide link 404ing for months.
- **Agent file outputs are not seamlessly reviewable.** [Issue #1552](https://github.com/netease-youdao/LobsterAI/issues/1552) describes a common workflow problem: after the agent writes a file, users must either ask it to read the file back into chat or leave the conversation to open it in the file manager.
- **Users want a richer MCP UI surface.** [Issue #2601](https://github.com/netease-youdao/LobsterAI/issues/2601) indicates that MCP servers are already delivering interactive HTML tools, and the desktop client currently cannot display them well.
- **Version/safety compliance is an active user concern.** [Issue #1082](https://github.com/netease-youdao/LobsterAI/issues/1082) links the pinned OpenClaw version to external security/regulatory pressure to keep dependencies updated.
- **Concurrency bugs undermine trust in real-time sessions.** Both [Issue #1089](https://github.com/netease-youdao/LobsterAI/issues/1089) and [Issue #1088](https://github.com/netease-youdao/LobsterAI/issues/1088) describe scenarios where rapid messaging can cause corrupted or cross-turn message handling.

Overall, user sentiment appears mixed: there is steady delivery in recent PRs, but several high-value bug reports and feature requests have remained unanswered for months and were touched by stale-bot rather than maintainers.

## 8. Backlog Watch

These items are old, open, and appear to need maintainer attention:

- [Issue #1082](https://github.com/netease-youdao/LobsterAI/issues/1082) — OpenClaw version support / compliance risk. Open since 2026-03-30, updated by stale-bot on 2026-09-03.
- [Issue #1088](https://github.com/netease-youdao/LobsterAI/issues/1088) — Prefetch `turnToken` bug causing possible cross-turn contamination. Open since 2026-03-31.
- [Issue #1089](https://github.com/netease-youdao/LobsterAI/issues/1089) — `CoworkRunner` reentrancy bug causing corrupted/duplicated messages. Open since 2026-03-31.
- [PR #1078](https://github.com/netease-youdao/LobsterAI/pull/1078) — Adds IM alert notifications for scheduled-task failures. No activity since March/April, still open.
- [PR #1079](https://github.com/netease-youdao/LobsterAI/pull/1079) — Cowork “current process” panel with tool execution and diff views. Still open.
- [PR #1081](https://github.com/netease-youdao/LobsterAI/pull/1081) — MCP i18n completion and dialog scrollbar fix. Still open.
- [PR #1087](https://github.com/netease-youdao/LobsterAI/pull/1087) — Fixes duplicate `continueSession()` error messages. Still open.
- [PR #1277](https://github.com/netease-youdao/LobsterAI/pull/1277) — Dependabot PR bumping Electron from `40.2.1` to `44.0.0` across the desktop project. Open since 2026-04-02; a major-version dependency update should be evaluated, tested, and either merged or closed explicitly.

The backlog pattern suggests the maintainers are actively shipping release-line PRs, but the older community-contributed PRs and serious concurrency issues have not yet received visible review outcomes.

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

# CoPaw Project Digest — 2026-09-04

*Data source: agentscope-ai/QwenPaw activity feed*

## 1. Today’s Overview

QwenPaw saw high activity in the last 24 hours: 27 issues were updated (19 open/active, 8 closed) and 36 PRs were updated (21 open, 15 merged/closed). No new releases were published during the window, but the project is close to or in v2.2.0 stable verification. Community discussion remains focused on the upcoming multi-tenant Hub, while bug reports center on security/governance, ReMe memory reliability, channel-specific failures, and desktop/mobile UX gaps. The PR queue is healthy, with UI/console improvements and backend hardening both moving forward.

## 2. Releases

None.

No new releases were recorded in the last 24 hours. A v2.2.0 stable installation verification issue was active/closed during this period, but no associated release changelog was included in the data.

## 3. Project Progress

The following PRs reached a closed/merged state in this update window:

- [PR #7524 — fix(console): separate free models from pro tab](https://github.com/agentscope-ai/QwenPaw/pull/7524)  
  Keeps free-tier models out of the PRO tab and limits the FREE tab to models marked `is_free === true`. Adds regression coverage for providers with mixed free/paid models.

- [PR #5399 — feat(providers): support custom model ordering within providers](https://github.com/agentscope-ai/QwenPaw/pull/5399)  
  Adds drag-and-drop / up-down reordering of models inside provider model management, persisted to the backend.

- [PR #5394 — feat(plugin-manager): mobile card layout and unified catalog cards](https://github.com/agentscope-ai/QwenPaw/pull/5394)  
  Fixes plugin manager overflow and clipped controls on narrow viewports.

- [PR #5363 — fix(console): improve mobile responsiveness of settings/agents page](https://github.com/agentscope-ai/QwenPaw/pull/5363)  
  Replaces the agents settings table with a card-based layout on mobile.

- [PR #5334 — feat(ui): allow switching agent in collapsed sidebar mode](https://github.com/agentscope-ai/QwenPaw/pull/5334)  
  Previously the collapsed sidebar showed only a static agent icon; this enables agent switching without expanding.

- [PR #7498 — fix(tools): return 404 when updating config for an unknown tool](https://github.com/agentscope-ai/QwenPaw/pull/7498)  
  Converts an internal `500` into a proper `404` when a tool config update targets a nonexistent tool.

- [PR #7525 — fix(governance): require approval for non-auto-denied critical findings](https://github.com/agentscope-ai/QwenPaw/pull/7525)  
  Fixes [Issue #7496](https://github.com/agentscope-ai/QwenPaw/issues/7496): CRITICAL findings were being instantly rejected even when the matched rule did not configure automatic denial; now such findings go through the expected approval flow.

- [PR #7080 — [Feature] Add optional PowerContext pluggable long-term memory backend](https://github.com/agentscope-ai/QwenPaw/pull/7080)  
  Adds `PowerContextMemoryManager` as a selectable long-term memory backend alongside existing ReMe implementations, registered via `@memory_registry.register("powercontext")`.

These closures show progress on both frontend usability and backend correctness, especially model management, governance behavior, and memory backend extensibility.

## 4. Community Hot Topics

Most active items by issue comments/reactions:

- [Issue #7318 — QwenPaw Hub, the multi-tenant edition, is coming in 2.2.0: what should we build next?](https://github.com/agentscope-ai/QwenPaw/issues/7318) — 17 comments, 3 👍  
  The most-discussed item. Community input references prior demand for multi-user access and admin-managed skills from [Issue #2324](https://github.com/agentscope-ai/QwenPaw/issues/2324). This is a clear roadmap-signal discussion.

- [Issue #7511 — [Bug]: QwenPaw2 security sandbox was breached / 安全沙箱被突破](https://github.com/agentscope-ai/QwenPaw/issues/7511) — 9 comments  
  Closed, but still a high-attention security report. Likely raised concern about sandbox strength and prompt-isolation guarantees.

- [Issue #7505 — qwenpaw accessing LAN LLM server repeatedly hits client disconnect and timeout](https://github.com/agentscope-ai/QwenPaw/issues/7505) — 7 comments  
  Pain point for users running local/LM Studio models over LAN. Reliability of streaming with local OpenAI-compatible endpoints is a recurring need.

- [Issue #4036 — Adding a model requires too many steps and clicks](https://github.com/agentscope-ai/QwenPaw/issues/4036) — 6 comments  
  A long-standing UX complaint about provider/model configuration flow.

- [Issue #7443 — It is easy for dangerous instructions to evade](https://github.com/agentscope-ai/QwenPaw/issues/7443) — 6 comments  
  Security/governance bypass report, closely related to the sandbox and critical-rule handling topics.

- [Issue #7469 — ReMe background embedding/indexing job fails](https://github.com/agentscope-ai/QwenPaw/issues/7469) — 5 comments  
  ReMe long-term memory reliability issue; background indexing fails silently with OpenAI-compatible embedding backends.

Underlying community needs: stronger security boundaries, better local/LAN model support, simplified setup, reliable long-term memory, and team/multi-user infrastructure.

## 5. Bugs & Stability

Ranked by severity:

### Security / governance

- [Issue #7511 — QwenPaw2 security sandbox was breached](https://github.com/agentscope-ai/QwenPaw/issues/7511) — **Critical**, closed. No dedicated fix PR was listed in the sample.
- [Issue #7443 — Dangerous instructions can evade QwenPaw safety](https://github.com/agentscope-ai/QwenPaw/issues/7443) — **Critical**, open. No visible fix PR yet.
- [Issue #7496 — CRITICAL rule rejected instead of entering required approval flow](https://github.com/agentscope-ai/QwenPaw/issues/7496) — **High**, closed. Fixed by [PR #7525](https://github.com/agentscope-ai/QwenPaw/pull/7525).

### Memory / reliability

- [Issue #7469 — ReMe background embedding/indexing job fails: `as_embedding:default accessed before start()`](https://github.com/agentscope-ai/QwenPaw/issues/7469) — **High**, open. ReMe memory indexing fails silently, so new memories may never be stored.
- [Issue #7534 — Feishu session queue consumer stays alive & stuck; same session becomes unresponsive to new messages](https://github.com/agentscope-ai/QwenPaw/issues/7534) — **High**, open. A single stuck high-priority card message can permanently block a DM session.
- [Issue #7476 — Cron task duplicated within `misfire_grace`, causing backup script to run twice](https://github.com/agentscope-ai/QwenPaw/issues/7476) — **Medium**, open.
- [Issue #7510 — `/memory/status` returns 500 on v2.2.0-beta.7 Desktop](https://github.com/agentscope-ai/QwenPaw/issues/7510) — **Medium**, open.

### Integration issues

- [Issue #7531 — OpenCode API now requires `x-opencode-session` header](https://github.com/agentscope-ai/QwenPaw/issues/7531) — **High**, open. OpenCode has warned requests missing the header may error starting 09/06.
- [Issue #7529 — Enabling Langfuse monitoring causes tool output to appear blank](https://github.com/agentscope-ai/QwenPaw/issues/7529) — **Medium**, open.
- [Issue #7516 — WeCom cannot send images represented as base64 data URLs](https://github.com/agentscope-ai/QwenPaw/issues/7516) — **Medium**, open.
- [Issue #7474 — Custom provider fails to load after `max_tokens` → `max_output_length` migration](https://github.com/agentscope-ai/QwenPaw/issues/7474) — **Medium**, closed.

### Desktop / console behavior

- [Issue #7545 — Desktop chat input missing right-click copy option, while web works](https://github.com/agentscope-ai/QwenPaw/issues/7545) — **Low**, closed.
- [Issue #7512 — Cannot switch conversation while another session is generating/outputting](https://github.com/agentscope-ai/QwenPaw/issues/7512) — **Medium**, closed.

Fix coverage is mixed: governance and HTTP-error handling were fixed, but the more serious sandbox bypass report, ReMe memory failure, and Feishu session stall still need maintainer follow-up.

## 6. Feature Requests & Roadmap Signals

Several feature requests stand out:

- [Issue #7318 — Multi-tenant Hub roadmap discussion](https://github.com/agentscope-ai/QwenPaw/issues/7318)  
  The clearest roadmap signal. Community is asking for team/multi-user support, admin-managed skills, and likely shared agent deployments.

- [Issue #7519 — Add mobile remote connection to QwenPaw Desktop](https://github.com/agentscope-ai/QwenPaw/issues/7519)  
  Users want to remotely control a running desktop instance from their phone, including viewing conversations, approving tool calls, and accessing workspace files.

- [Issue #7543 — Move online updates to background instead of blocking foreground UI](https://github.com/agentscope-ai/QwenPaw/issues/7543)  
  Simple but practical request from beta users.

- [Issue #7540 — Add config toggle to opt out of the hardcoded “About” identity line in `env_context`](https://github.com/agentscope-ai/QwenPaw/issues/7540)  
  Users with custom `SOUL.md` personas want control over identity injection.

- [Issue #7527 — Preserve agent persona and conversation style during native context compaction](https://github.com/agentscope-ai/QwenPaw/issues/7527)  
  Points to a broader concern around context compaction losing relationship/persona details.

- [Issue #7541 — Sessions should not be split/locked by channel](https://github.com/agentscope-ai/QwenPaw/issues/7541)  
  Architecture-level request: web console, desktop UI, and Telegram sessions should be unified because the channel is only an input transport.

- [Issue #7533 — Support interactive message buttons](https://github.com/agentscope-ai/QwenPaw/issues/7533)  
  Wants agent output to include clickable options/buttons that continue the conversation, plus custom channel support.

- [Issue #7535 — Matrix channel: Element-specific recovery-key verification and MSC2965 OIDC login](https://github.com/agentscope-ai/QwenPaw/issues/7535)  
  A more specialized integration request for Element/Matrix users.

- [Issue #1775 — Codex-like steer mode during agent execution](https://github.com/agentscope-ai/QwenPaw/issues/1775)  
  Older but still relevant: users want to inject guidance while an agent is running, not only before/after.

- [Issue #4036 — Simplify “add a model” workflow](https://github.com/agentscope-ai/QwenPaw/issues/4036)  
  Labeled `good first issue`, but still unsolved after several months.

The likely near-term roadmap already includes the multi-tenant Hub. Beyond that, the cluster of requests around session unification, agent identity preservation, and interactive controls suggests the next priorities may be conversation-system improvements and custom-agent runtime controls.

## 7. User Feedback Summary

Real user pain points visible in this data:

- **Power users are running real workloads**: cron backup jobs, Feishu/WeCom business messaging, LAN LLM servers, Langfuse tracing, and ReMe memory. Failures in these areas cause concrete disruption, not just inconvenience.
- **Trust and security anxiety is high**: The closed sandbox breach issue [\#7511](https://github.com/agentscope-ai/QwenPaw/issues/7511) and governance evasion report [\#7443](https://github.com/agentscope-ai/QwenPaw/issues/7443) show users are actively probing and evaluating QwenPaw’s security model.
- **A significant Chinese-speaking user base is reporting bugs and UX requests**, plus one Russian-language issue, indicating the project has broad non-English adoption.
- **Desktop/console parity is a repeated annoyance**: Missing right-click copy, blocked session switching, collapsed-sidebar limitations, and foreground updates are all “polish” issues that affect daily feel.
- **Remote/mobile access is an emerging expectation**: Both [\#7519](https://github.com/agentscope-ai/QwenPaw/issues/7519) and [\#7518](https://github.com/agentscope-ai/QwenPaw/issues/7518) show users want to leave a desktop agent running and still manage it from their phone.
- **Local-first and privacy-conscious users are especially sensitive to identity injection**: [\#7540](https://github.com/agentscope-ai/QwenPaw/issues/7540) and [\#7527](https://github.com/agentscope-ai/QwenPaw/issues/7527) reveal demand for preserving custom persona and not forcing a built-in “About” identity.

Overall, feedback skews toward advanced users pushing QwenPaw from a single-user assistant into a reliable, team-ready, remotely accessible agent platform.

## 8. Backlog Watch

Older or open items that may need maintainer attention:

- [Issue #1775 — Codex-like steer mode for in-progress agent correction](https://github.com/agentscope-ai/QwenPaw/issues/1775)  
  Open since 2026-03-18. Still seems unaddressed despite only 3 comments. This is a valuable core-agent UX feature.

- [Issue #4036 — Adding a model requires too many steps and clicks](https://github.com/agentscope-ai/QwenPaw/issues/4036)  
  Open since 2026-05-04 and labeled `good first issue`. It is an onboarding bottleneck and remains unresolved.

- [PR #6399 — feat: add reranker UI config panel to ReMeLightMemoryCard](https://github.com/agentscope-ai/QwenPaw/pull/6399)  
  Open since 2026-07-23 and still marked “Under Review.” The reranker UI complements a backend feature and appears to be waiting for maintainer review.

- [Issue #7531 — OpenCode API now requires `x-opencode-session` header](https://github.com/agentscope-ai/QwenPaw/issues/7531)  
  Open and time-sensitive: OpenCode may begin erroring on missing headers starting 09/06.

- [Issue #7443 — Dangerous instructions can evade QwenPaw safety](https://github.com/agentscope-ai/QwenPaw/issues/7443)  
  Open since 2026-08-31. Given the related security reports, this should be prioritized for investigation.

- [Issue #7469 — ReMe background embedding/indexing silently fails](https://github.com/agentscope-ai/QwenPaw/issues/7469)  
  Open since 2026-09-01. Memory reliability issues can severely undermine long-term agent usefulness.

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw Project Digest — 2026-09-04

## 1. Today's Overview

ZeroClaw is in a very active hardening-and-hardening-planning cycle, with **50 issues and 50 PRs updated in the last 24 hours**. Of the issues touched, 36 remain open and **14 moved to a closed state**; on the PR side **49 remain open and 1 closed** — a single merged/closed PR against `master`. No new releases were published in this window. The visible activity is dominated by security work (sandbox policy, verifiable-intent verification, tool permission schemas), maintainer-governed RFC batches, and broad refactoring (cron extraction, ACP transcript pagination, distribution tooling). The overall signal is a healthy, responsive project with heavy contributor throughput and a disciplined public decision-making process.

## 2. Releases

No releases published in this window.

## 3. Project Progress

**Merged/closed PR:** [PR #10539 — fix(runtime): stop advertising self-approval in tool schemas](https://github.com/zeroclaw-labs/zeroclaw/pull/10539). This closes a misleading attack surface where tool schemas exposed an `approved` argument that models could attempt to set, even though runtime plumbing (`call_prep`) already strips and overwrites model-supplied values at the approval gate.

**Closed issues (14, updated in the last 24h):** the closures cluster into clear completion themes:
- **Security fixes:** [operator denial now carries semantics instead of three bare words (#9654)](https://github.com/zeroclaw-labs/zeroclaw/issues/9654), [Docker runtime no longer nested inside a second Docker sandbox (#9231)](https://github.com/zeroclaw-labs/zeroclaw/issues/9231), [interactive approvals no longer accepted from arbitrary chat members on Telegram/Slack/Lark/Matrix (#9387)](https://github.com/zeroclaw-labs/zeroclaw/issues/9387), and [/health no longer reports never-connected channels as healthy (#9811)](https://github.com/zeroclaw-labs/zeroclaw/issues/9811).
- **Channel/integration fixes:** [Discord transcription manager is now bound to the active agent provider (#9905)](https://github.com/zeroclaw-labs/zeroclaw/issues/9905), [Matrix honors configured transcription providers (#10486)](https://github.com/zeroclaw-labs/zeroclaw/issues/10486), and the [Discord-assisted SOP triage mode (#8518)](https://github.com/zeroclaw-labs/zeroclaw/issues/8518) landed.
- **Runtime/observability fixes:** [ZeroCode no longer shows stale “Connected” after daemon exit (#10238)](https://github.com/zeroclaw-labs/zeroclaw/issues/10238), [vision-fallback error reporting corrected (#9983)](https://github.com/zeroclaw-labs/zeroclaw/issues/9983), [canonical JSONL session file handling (#9857)](https://github.com/zeroclaw-labs/zeroclaw/issues/9857), and [a `log`-crate bridge so dependency logs reach the tracing subscriber (#10202)](https://github.com/zeroclaw-labs/zeroclaw/issues/10202).
- **Feature/process items:** [Multi-session gateway web chat UI (#7543)](https://github.com/zeroclaw-labs/zeroclaw/issues/7543), [Anthropic `thinking.display` progress update support (#10529)](https://github.com/zeroclaw-labs/zeroclaw/issues/10529), and a [CI guard against “blame-collapse” PRs with no common ancestor (#9510)](https://github.com/zeroclaw-labs/zeroclaw/issues/9510).

**Major in-flight PRs (all open):** [PR #10610 implements the accepted shell V1 permission policy (RFC #7155 Phase 0+1)](https://github.com/zeroclaw-labs/zeroclaw/pull/10610); [PR #10197 checkpoints interrupted ACP turn progress](https://github.com/zeroclaw-labs/zeroclaw/pull/10197); [PR #10557 extracts cron into `zeroclaw-cron`](https://github.com/zeroclaw-labs/zeroclaw/pull/10557); [PR #10596 adds pagination for persisted ACP transcripts](https://github.com/zeroclaw-labs/zeroclaw/pull/10596); and [PR #10565 pins local ZeroCode sessions to the process working directory](https://github.com/zeroclaw-labs/zeroclaw/pull/10565).

## 4. Community Hot Topics

The most-discussed items are almost all security/governance heavyweights:

- [Issue #6996 — RFC: Granular sandbox policy / filesystem restrictions (23 comments)](https://github.com/zeroclaw-labs/zeroclaw/issues/6996) — the longest-running active design discussion. It targets the drift between application-layer path admission in `SecurityPolicy` and OS-level sandbox backends (Bubblewrap, Landlock, Seatbelt). Still tagged `needs-maintainer-review` with `risk:high` after ~14 weeks, indicating a difficult, consequential design decision. **Underlying need:** coherent, fine-grained filesystem confinement matched to agent risk profiles.
- [Issue #9328 — verifiable-intent evaluates constraints without verifying the credential chain (14 comments)](https://github.com/zeroclaw-labs/zeroclaw/issues/9328) — a correctness/security gap where L2 constraints come from the caller rather than from cryptographically verified chain values. Accepted and in-progress.
- [Issue #8692 — Tracker: Maintainer decision queue for RFCs and design issues (14 comments)](https://github.com/zeroclaw-labs/zeroclaw/issues/8692) — the community’s visible intake queue for RFCs, design votes, and release-policy questions. Signals that ZeroClaw’s governance is deliberately public.
- [Issue #10050 — RFC: Verbatim channel send over the gateway without an agent turn (13 comments)](https://github.com/zeroclaw-labs/zeroclaw/issues/10050) — accepted, `risk:high`. Users want first-class operator/direct channel messaging despite the gateway already exposing 47 API paths.
- [Issue #9975 — RFC: Web bundle/daemon compatibility for `web_dist_dir` (12 comments)](https://github.com/zeroclaw-labs/zeroclaw/issues/9975) — accepted capability-negotiation contract for web dashboard deployments.

On the PR side, comment counts were not populated in the export, but the strongest attention magnets by likely impact are [PR #10610](https://github.com/zeroclaw-labs/zeroclaw/pull/10610) (shell permission policy), [PR #10197](https://github.com/zeroclaw-labs/zeroclaw/pull/10197) (ACP interruption persistence) and [PR #9584](https://github.com/zeroclaw-labs/zeroclaw/pull/9584) (egress grant ceremony for plugin installs — carrying a maintainer scope-correction note). A single 👍 on the brand-new S1 bug [Issue #10603 (OpenCode missing `x-opencode-session`)](https://github.com/zeroclaw-labs/zeroclaw/issues/10603) already marks it as a fast-rising user concern.

## 5. Bugs & Stability

Ranked by severity:

1. **[Issue #10609 — S1: zerocode ignores its launch directory and forces agent workspace as cwd (new today)](https://github.com/zeroclaw-labs/zeroclaw/issues/10609)** — workflow-blocking for local TUI development. A fix is already in flight: [PR #10565 pins local Code sessions to process cwd](https://github.com/zeroclaw-labs/zeroclaw/pull/10565).
2. **[Issue #10603 — S1: OpenCode providers never send `x-opencode-session`, breaking Go models and risking account flags (new today)](https://github.com/zeroclaw-labs/zeroclaw/issues/10603)** — protocol-level incompatibility with the OpenCode relay. **No fix PR yet.**
3. **[Issue #9899 — P1 blocked: `cargo deny` failing on `RUSTSEC-2026-0247` (`bitmaps` via Matrix SDK dev-dependencies)](https://github.com/zeroclaw-labs/zeroclaw/issues/9899)** — security CI is red; blocked pending dependency triage.
4. **[Issue #9328 — verifiable-intent credential-chain verification gap](https://github.com/zeroclaw-labs/zeroclaw/issues/9328)** — accepted/in-progress but still exposing the runtime to caller-forged constraint evaluation.
5. **[Issue #10068 — S2: interactive agent session caps context at 32,000 tokens despite `max_context_tokens = 131072`](https://github.com/zeroclaw-labs/zeroclaw/issues/10068)** — degraded long-session behavior, still open without a visible fix PR.
6. **[PR #10539 merged/closed](https://github.com/zeroclaw-labs/zeroclaw/pull/10539)** resolves the self-approval schema issue, but note the related sibling issue space around denial semantics (#9654) is also now closed.

High-risk bugs closed in this window include the Docker double-sandbox (#9231, S1), the cross-channel interactive-approval vulnerability (#9387, P1), and the false-healthy `/health` reporting (#9811, P1) — signal that the security backlog is being actively worked off.

## 6. Feature Requests & Roadmap Signals

The current open RFCs and trackers paint a clear near-term roadmap:

- **Security policy v1:** [RFC #7155 implementation is landing via PR #10610](https://github.com/zeroclaw-labs/zeroclaw/pull/10610), and the long-running [granular filesystem sandbox RFC (#6996)](https://github.com/zeroclaw-labs/zeroclaw/issues/6996) is the most likely next security milestone once a maintainer decision is made.
- **Chat/UI completion:** multi-session web chat (#7543) closed, with follow-on polish already in flight — [/upload slash command (#10578)](https://github.com/zeroclaw-labs/zeroclaw/pull/10578), [any-file upload with RPC-parity markers (#10583)](https://github.com/zeroclaw-labs/zeroclaw/pull/10583), and [a 20 MiB image-size default aligned to provider ceilings (#10589)](https://github.com/zeroclaw-labs/zeroclaw/pull/10589).
- **Code-pane/ACP continuity:** expected to converge soon — [interrupted turn persistence (#10197)](https://github.com/zeroclaw-labs/zeroclaw/pull/10197), [ACP transcript pagination (#10596)](https://github.com/zeroclaw-labs/zeroclaw/pull/10596), [memory recall-date stamping (#10567)](https://github.com/zeroclaw-labs/zeroclaw/pull/10567), and the [ACP memory-continuity tracker (#10570)](https://github.com/zeroclaw-labs/zeroclaw/issues/10570).
- **Packaging/distribution standardization:** the new [canonical release-target registry (#10590)](https://github.com/zeroclaw-labs/zeroclaw/pull/10590) and [MCP bootstrap launcher (#10591)](https://github.com/zeroclaw-labs/zeroclaw/pull/10591) indicate a coming install/launch experience improvement.
- **Architecture cleanup:** [cron extraction into `zeroclaw-cron` (#10557)](https://github.com/zeroclaw-labs/zeroclaw/pull/10557) and the [holding-crate exception ADR (#10562)](https://github.com/zeroclaw-labs/zeroclaw/pull/10562) show deliberate debt reduction rather than feature-only velocity.

Prediction: the next release will likely bundle the shell V1 permission policy, ZeroCode/ACP persistence fixes, web upload UX, and at least the first distribution-registry tooling.

## 7. User Feedback Summary

Pain points expressed by users this cycle:

- **Silent configuration violation:** the 32k context cap despite a 131,072 setting (#10068) frustrates users running long interactive sessions that suddenly compact mid-work.
- **Workspace vs. launch-directory confusion:** launching `zerocode` locally is expected to “work where I am,” but it forces the agent workspace instead (#10609).
- **Broken provider integrations:** missing `x-opencode-session` headers (Go models, account-flag risk) are an S1 blocker for OpenCode-relay users (#10603); users also want the gateway to send verbatim messages without an agent turn (#10050).
- **Trust and clarity:** users were burned by misleading health checks (#9811), approving users on channels where any member could answer approvals (#9387), and models inventing causes when operators deny actions (#9654). All three are now closed.
- **Diagnostics quality:** fallback-to-non-vision models produced misleading error messages (#9983), now corrected.

Satisfaction signals: contributors are shipping a steady stream of small, well-scoped PRs (Twitch setup docs [#10581](https://github.com/zeroclaw-labs/zeroclaw/pull/10581), Todo-tracker dismissal [#10584](https://github.com/zeroclaw-labs/zeroclaw/pull/10584), long-thinking caching [#10595](https://github.com/zeroclaw-labs/zeroclaw/pull/10595)); maintainers are visibly reviewing with inline evidence and scope corrections (see the note on PR #9584); and the closed-issue count demonstrates that filed complaints convert to fixes quickly.

## 8. Backlog Watch

Items that need maintainer attention or have stalled despite priority:

- **[Issue #6996 — Granular sandbox policy RFC](https://github.com/zeroclaw-labs/zeroclaw/issues/6996)** — open since 2026-05-28, 23 comments, still `needs-maintainer-review` with `risk:high`. This is the single largest unanswered design decision.
- **[Issue #7108 — CI cached Rust builds / critical path improvement](https://github.com/zeroclaw-labs/zeroclaw/issues/7108)** — accepted since 2026-06-02 with no implementing PR; PR CI still takes 15–20 minutes on small changes.
- **[Issue #7685 — Test coverage and stale-test tracker across 13 shards](https://github.com/zeroclaw-labs/zeroclaw/issues/7685)** — open since June with only 1 comment; the large follow-up corpus has no visible executor.
- **[Issue #9899 — `RUSTSEC-2026-0247` bitmaps advisory waiver](https://github.com/zeroclaw-labs/zeroclaw/issues/9899)** — P1, `blocked` since 2026-08-10; security CI remains red while awaiting dependency resolution.
- **[Issue #9328 — VI constraint/credential-chain verification bug](https://github.com/zeroclaw-labs/zeroclaw/issues/9328)** — accepted and in-progress for over six weeks; high-risk security gap that should stay a priority.
- **[Issue #10603 — OpenCode `x-opencode-session` header bug](https://github.com/zeroclaw-labs/zeroclaw/issues/10603)** — new S1 with no fix PR yet; watch for a fast follow-up.

Overall: ZeroClaw appears well-governed and responsive, with security and architectural debt receiving visible attention. The main risk to watch is the accumulation of accepted-but-unimplemented trackers and a handful of P1/security items that remain blocked or in-progress for weeks.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*