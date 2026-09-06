# OpenClaw Ecosystem Digest 2026-09-06

> Issues: 500 | PRs: 500 | Projects covered: 13 | Generated: 2026-09-06 04:06 UTC

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

# OpenClaw Project Digest — 2026-09-06

## 1. Today's Overview

OpenClaw saw very high activity on 2026-09-06: 500 issues and 500 PRs were updated in the last 24 hours, with 123 issues closed and 224 PRs merged/closed. One new release, **v2026.9.2**, shipped with chat-responsiveness improvements targeting the frequently reported Gateway event-loop blocking problem. Maintainer-style cleanup and reliability PRs dominate the open PR queue, with heavy contribution from a single prolific maintainer (steipete) spanning process handling, update lifecycle, ACP delivery, and the Control UI. However, a cluster of stubborn **message-loss, duplicate-delivery, and session-state corruption** issues remain open at P1/P0, indicating that architectural reliability is the project's primary ongoing concern.

---

## 2. Releases

### v2026.9.2
- **Focus:** "Faster, more responsive chat" — keeps chat, dashboards, and session interactions responsive while long transcripts and disk usage are processed.
- **Includes:** direct dashboard lookup, reduced cold-load work, and durable history reads moved outside the Gateway event loop (refs: [#136862](https://github.com/openclaw/openclaw/issues/136862), [#138…](https://github.com/openclaw/openclaw/issues/138...)).
- **Breaking changes / migration notes:** None stated for users in the available release notes.
- **Context:** This release directly targets the class of P1/P2 issues where synchronous persistence and transcript maintenance block the Gateway ([#119720](https://github.com/openclaw/openclaw/issues/119720), [#53008](https://github.com/openclaw/openclaw/issues/53008), [#99910](https://github.com/openclaw/openclaw/issues/99910)).

---

## 3. Project Progress

224 PRs were merged/closed in the last 24 hours. From the visible top-PR set, the following merged/closed items advanced the codebase:

- [#139684](https://github.com/openclaw/openclaw/pull/139684) — refactor(android): reuse cron interval formatting (closed).
- [#139681](https://github.com/openclaw/openclaw/pull/139681) — improve(plugins): speed up repeated model catalog lookups (closed).
- [#139636](https://github.com/openclaw/openclaw/pull/139636) — refactor(auth): share legacy choice resolution across onboarding (closed).
- Closed issues include config-rollback feature request [#79164](https://github.com/openclaw/openclaw/issues/79164), MiniMax-M3 context display bug [#111630](https://github.com/openclaw/openclaw/issues/111630), and the Linux desktop AppImage crash [#136148](https://github.com/openclaw/openclaw/issues/136148).

**Featured themes in flight (open PRs):**

- **Plugin lifecycle without Gateway restarts** — [#135599](https://github.com/openclaw/openclaw/pull/135599) lets operators/agents install, enable, disable, and reload plugins live with rollback reporting.
- **Cloud sessions without a Gateway checkout** — [#138900](https://github.com/openclaw/openclaw/pull/138900) moves worktree preparation to the cloud/paired node while the Gateway retains ownership and credentials.
- **Update reliability** — [#139701](https://github.com/openclaw/openclaw/pull/139701) preserves restart outcomes vs. failed health checks; [#139704](https://github.com/openclaw/openclaw/pull/139704) adds explicit-consent failure reporting from Control UI.
- **Delivery / transcript integrity** — [#139394](https://github.com/openclaw/openclaw/pull/139394) recovers queued messages after adoption timeouts; [#139685](https://github.com/openclaw/openclaw/pull/139685) preserves queued ACP output on slow delivery; [#137381](https://github.com/openclaw/openclaw/pull/137381) keeps long transcript history available during `sessions_yield` cleanup.
- **Channel fixes** — [#139614](https://github.com/openclaw/openclaw/pull/139614) stops Feishu dead-stream log flooding with a non-streaming card fallback; [#135366](https://github.com/openclaw/openclaw/pull/135366) fixes Firecrawl self-hosted DNS misdiagnosis.
- **Maintainer hygiene** — large dead-code/indirection removal ([#139573](https://github.com/openclaw/openclaw/pull/139573)), daemon lifecycle test consolidation ([#139694](https://github.com/openclaw/openclaw/pull/139694)), and overlapping proxy deadline tests ([#139651](https://github.com/openclaw/openclaw/pull/139651)).

---

## 4. Community Hot Topics

Most-discussed issues (by comment count) reveal three underlying needs: **no message loss, no duplicate delivery, and no Gateway freezes**.

- [#69208](https://github.com/openclaw/openclaw/issues/69208) — *Umbrella: duplicate transcript, replay, and context assembly across channels* (14 comments; P1; open since April). A broad class of duplicate-message bugs spanning MSTeams, webchat, Telegram, and delivery-mirror paths.
- [#132762](https://github.com/openclaw/openclaw/issues/132762) — *Overflow retry can end "successfully" on a tool result without final delivery* (13 comments; P1). Users get no assistant response despite a `success` terminal state.
- [#53408](https://github.com/openclaw/openclaw/issues/53408) — *Write/exec tool parameters silently dropped after long conversations* (12 comments, 2👍; P2). High impact on real multi-step workflows.
- [#53763](https://github.com/openclaw/openclaw/issues/53763) — *Built-in headless browser for reliable web access* (12 comments; P3). Strong demand for removing the "fragile three-layer" dependency on user Chrome/third-party APIs.
- [#39476](https://github.com/openclaw/openclaw/issues/39476) — *A2A sessions_send loop causes duplicate messages* (12 comments; P1). Agent-to-agent responses double-post into the requester's channel.
- [#96975](https://github.com/openclaw/openclaw/issues/96975) — *Isolate subagent completion from parent context* (12 comments, 1👍; P2). Heavy subagent payloads pollute parent context.
- [#97616](https://github.com/openclaw/openclaw/issues/97616) — *Unreaped hook/tool child processes → zombie accumulation* (11 comments, 1👍; P1 regression).

The dominant need across hot topics is **transactional integrity of agent turns** — users repeatedly report silent drops, duplicates, and half-finished runs that "succeed" without ever delivering an answer.

---

## 5. Bugs & Stability

### 🔴 P0 / Release-blocking
- [#115642](https://github.com/openclaw/openclaw/issues/115642) — Billing cooldown outlives the actual provider outage on subscription auth; fixed ~5h `disabledUntil` window fails all requests. Needs probe-based recovery + shorter TTL (P0, diamond lobster, open).
- [#91931](https://github.com/openclaw/openclaw/issues/91931) — Preseeded SOUL.md/IDENTITY.md/USER.md cause OpenClaw to auto-complete bootstrap and **delete user-provided BOOTSTRAP.md before first run** (P0, data-loss).

### 🔴 High-severity P1, regressions and blockers
- [#124133](https://github.com/openclaw/openclaw/issues/124133) — **Beta blocker:** `openclaw-snowluma` channel plugin fails every QQ inbound dispatch (`formatInboundEnvelope is not a function`) after 2026.8.1-beta.2.
- [#97616](https://github.com/openclaw/openclaw/issues/97616) — Zombie hook/tool child processes accumulate, degrading runtime over time (regression).
- [#136183](https://github.com/openclaw/openclaw/issues/136183) — Command executor hangs spawning `ssh`; SIGTERM while waiting for server banner (regression in 2026.8.1, persists in 2026.8.2).
- [#91941](https://github.com/openclaw/openclaw/issues/91941) — Feishu streaming card full-content updates cause severe latency regression on long replies.
- [#110190](https://github.com/openclaw/openclaw/issues/110190) — Runtime context carrier positioned AFTER the user message causes model confusion and token waste.
- [#119720](https://github.com/openclaw/openclaw/issues/119720) — Synchronous persistence blocks the Gateway event loop at scale (targeted by v2026.9.2).
- [#99910](https://github.com/openclaw/openclaw/issues/99910) — Memory dreaming run pegs the Gateway event loop ~10 min until killed; short-term recall never persists.
- [#112259](https://github.com/openclaw/openclaw/issues/112259) — Visible inbound turn silently dropped: zero-payload dispatch has no retry/dead-letter.
- [#132765](https://github.com/openclaw/openclaw/issues/132765) — `agents_wait` ignores `timeoutSeconds`, dies after ~60s as a tool error instead of returning pending.
- [#114967](https://github.com/openclaw/openclaw/issues/114967) — Agent-driven live update left a `launchctl` keepalive force-restarting the Gateway every ~2 minutes.
- [#90098](https://github.com/openclaw/openclaw/issues/90098) — Large Control-UI attachments overflow browser/Gateway stack (`RangeError: Maximum call stack`).
- [#89430](https://github.com/openclaw/openclaw/issues/89430) — Google Chat can't deliver images/files under app authentication (media upload → 403).

### 🟡 Notable P2
- [#84110](https://github.com/openclaw/openclaw/issues/84110) — Codex app-server rewrites prompt on tool-call continuation turns, busting prompt cache (93% → 47%).
- [#87212](https://github.com/openclaw/openclaw/issues/87212) — Internal system envelope footer echoed verbatim into Telegram outbound as the "answer."
- [#44134](https://github.com/openclaw/openclaw/issues/44134) — Frequent tool-schema reloading triggered a **Google Antigravity account ban** (false-positive anti-abuse detection).

**Matching fix PRs exist for several of these:** queued-message recovery ([#139394](https://github.com/openclaw/openclaw/pull/139394) → closes #139341), ACP output loss ([#139685](https://github.com/openclaw/openclaw/pull/139685) → closes #139680), Feishu stream flooding ([#139614](https://github.com/openclaw/openclaw/pull/139614) → closes #139443), and `sessions_yield` transcript unavailability ([#137381](https://github.com/openclaw/openclaw/pull/137381)).

---

## 6. Feature Requests & Roadmap Signals

Strong signals for near-term features:

- **Headless built-in browser** ([#53763](https://github.com/openclaw/openclaw/issues/53763)) — bundled Chromium as a first-class tool for JS-rendered and login-required pages; 12 comments, P3 but persistently discussed.
- **/models test-fallback command** ([#6599](https://github.com/openclaw/openclaw/issues/6599)) — verify fallback chains without waiting for real provider failure.
- **Reduce tool-schema token overhead** ([#14785](https://github.com/openclaw/openclaw/issues/14785)) — ~3,500 tokens/session fixed tax; large provider cost and context win.
- **Subagent context isolation** ([#96975](https://github.com/openclaw/openclaw/issues/96975)) — return only status + child session link by default.
- **Per-turn send budget for the `message` tool** ([#119992](https://github.com/openclaw/openclaw/issues/119992)) — stop within-turn duplicate-answer storms.
- **Strict failure policy for context engines** ([#116716](https://github.com/openclaw/openclaw/issues/116716)) — opt out of silent legacy-engine fallback.
- **Dynamic allowlist identity resolution** ([#58057](https://github.com/openclaw/openclaw/issues/58057)) — `dmPolicy: dynamic` for multi-user deployments without config reloads.
- **Progress-draft narration labels** ([#132781](https://github.com/openclaw/openclaw/issues/132781)) and **message-list pagination** ([#71452](https://github.com/openclaw/openclaw/issues/71452)) — smaller UX/quality-of-life asks.
- **Plugin hot management** ([PR #135599](https://github.com/openclaw/openclaw/pull/135599)) and **cloud sessions without Gateway checkout** ([PR #138900](https://github.com/openclaw/openclaw/pull/138900)) are already in PR form and likely candidates for the next minor release.

---

## 7. User Feedback Summary

Real pain points expressed across issues this cycle:

- **Silent message loss and duplicate deliveries** are the most emotionally charged complaints (e.g., [#69208](https://github.com/openclaw/openclaw/issues/69208), [#39476](https://github.com/openclaw/openclaw/issues/39476), [#87212](https://github.com/openclaw/openclaw/issues/87212) — where a system envelope footer is echoed to the user as the AI's answer).
- **Bot unresponsiveness for 10+ minutes** due to memory compaction, memory dreaming, or persistence blocking the processing lane ([#53008](https://github.com/openclaw/openclaw/issues/53008), [#99910](https://github.com/openclaw/openclaw/issues/99910)) — unacceptable for production Telegram/Feishu deployments.
- **Long-conversation degradation**: tool parameters silently dropped after ~15 turns ([#53408](https://github.com/openclaw/openclaw/issues/53408)); tool schemas reloading so often that a Google account was banned ([#44134](https://github.com/openclaw/openclaw/issues/44134)).
- **Provider/auth resilience gaps**: billing cooldowns outliving outages ([#115642](https://github.com/openclaw/openclaw/issues/115642)); Claude CLI session-limit errors failing to trigger fallback chains ([#118793](https://github.com/openclaw/openclaw/issues/118793)); model override inheritance confusion in WebChat ([#86174](https://github.com/openclaw/openclaw/issues/86174)).
- **Deployment friction**: SSH sandboxes not staging media ([#112160](https://github.com/openclaw/openclaw/issues/112160)); multi-user allowlists requiring hardcoded IDs ([#58057](https://github.com/openclaw/openclaw/issues/58057)); TUI scroll-jump disruptiveness ([#44130](https://github.com/openclaw/openclaw/issues/44130), 3👍).
- Satisfaction signals are indirect but real: closed issues this cycle (AppImage crash #136148, MiniMax context display #111630) show the team is responsive; heavy maintainer PR throughput indicates an actively healthy core.

---

## 8. Backlog Watch

Long-standing, high-importance items that still need maintainer attention:

- [#69208](https://github.com/openclaw/openclaw/issues/69208) — Umbrella for duplicate transcript/replay bugs; open since **2026-04-20**, P1, 14 comments, no fix PR linked. This should be a top triage target.
- [#53408](https://github.com/openclaw/openclaw/issues/53408) — Write/exec param drop after long conversations; open since **2026-03-24**, 12 comments, 2👍, still unaddressed.
- [#53763](https://github.com/openclaw/openclaw/issues/53763) — Headless browser feature; open since March, 12 comments, still awaiting product decision (`needs-product-decision`).
- [#39476](https://github.com/openclaw/openclaw/issues/39476) — A2A `sessions_send` duplicate messages; P1, open since March, 12 comments, has linked open PR but not resolved.
- [#97616](https://github.com/openclaw/openclaw/issues/97616) — Zombie child-process leak; P1 regression, open since June, 11 comments, 1👍, flagged `needs-info`.
- [#110190](https://github.com/openclaw/openclaw/issues/110190) — Runtime context-carrier placement causing model confusion; P1, 10 comments, `needs-product-decision`.
- [#72015](https://github.com/openclaw/openclaw/issues/72015) — `active-memory` blocking replies + QMD boot overload on multi-agent gateways; P1, 10 comments, 2👍.
- [#90098](https://github.com/openclaw/openclaw/issues/90098) — Stack-safe large attachments; P1, 7 comments, 2👍, linked PR open — needs push to merge.
- [#119720](https://github.com/openclaw/openclaw/issues/119720) — Gateway event-loop blocking; P1, 10 comments — v2026.9.2 claims partial relief, but the issue notes the remaining Gateway-thread persistence work is **not** closed.

Several of these carry `clawsweeper:needs-maintainer-review` / `needs-product-decision` labels, indicating the triage bot has surfaced them but human maintainer decisions are the bottleneck.

---

## Cross-Ecosystem Comparison

# Cross-Project Comparison — Personal AI Assistant & Agent OSS Ecosystem
**Digest date: 2026-09-06 · 13 tracked projects**

---

## 1. Ecosystem Overview

The personal AI assistant open-source space is maturing from "chatbot wrappers" into **stateful, multi-channel agent infrastructure**. This snapshot shows a polarized ecosystem: OpenClaw remains the clear reference core and traffic center, while a long tail of specialized variants — ZeroClaw, ZeptoClaw, NanoBot, CoPaw/QwenPaw, Hermes Agent — iterate on narrower problems such as security boundaries, Rust-native agent runtimes, desktop/gateway durability, and China-market channel support. Two projects shipped releases in the window (OpenClaw v2026.9.2, ZeroClaw v0.8.5), but several others show **maintainer-review bottlenecks**, merge-conflict backlogs, and P1 reliability issues that have gone unmerged for weeks or months. The dominant community demand across projects is no longer raw model capability but **operational trust**: no silent message loss, no duplicate delivery, no dead agents, and no unbounded context/memory growth.

---

## 2. Activity Comparison

Figures = items **updated in the last 24h** (closures/merges in parentheses). Health score is a directional 0–10 judgment blending issue/PR closure rate, open P0/P1 load, release cadence, backlog age, and maintainer responsiveness observed in this digest.

| Project | Issues updated (closed) | PRs updated (merged/closed) | Release this window | Health score |
|---|---|---|---|---|
| **OpenClaw** | 500 (123) | 500 (224) | v2026.9.2 | 7.6 |
| **Hermes Agent** | 50 (2) | 50 (2) | — | 5.8 |
| **ZeroClaw** | 42 (8) | 50 (6) | v0.8.5 | 8.2 |
| **NanoBot** | 1 (0) | 25 (7) | — | 7.0 |
| **ZeptoClaw** | 14 (4) | 8 (3) | — | 7.8 |
| **CoPaw / QwenPaw** | 11 (3) | 7 (0) | — | 6.3 |
| **PicoClaw** | 2 (1) | 3 (3) | — | 5.5 |
| **NanoClaw** | 0 (0) | 3 (0) | — | 6.2 |
| **LobsterAI** | 0 (0) | 2 (0) | — | 4.3 |
| **IronClaw** | 1 (0) | 1 (0) | — | 6.8 |
| NullClaw | 0 | 0 | — | Dormant |
| TinyClaw | 0 | 0 | — | Dormant |
| Moltis | 0 | 0 | — | Dormant |

**Notable interpretations:**
- **OpenClaw** closes ~25% of touched issues and ~45% of touched PRs daily — massive throughput, but 2 P0s and a cluster of P1 message-integrity bugs remain.
- **ZeroClaw** delivered the largest release (454 commits, 73 contributors) but its open queue is dominated by architecture RFCs and large security PR stacks awaiting maintainer review.
- **Hermes Agent** has the worst closure ratio among active projects (4% issues, 4% PRs) despite 100 touched items, plus a 163-comment watchdog issue.
- **NanoBot's** biggest risk is queue hygiene: 7 merges, but many open PRs (including P1 security fixes) carry `conflict` labels.
- **LobsterAI** has a clean issue tracker but two substantial PRs stranded for ~160 days — the clearest stalled-review signal in the ecosystem.

---

## 3. OpenClaw's Position

**Advantages vs. peers:**
- **Order-of-magnitude community scale.** OpenClaw's daily activity (500 issues, 500 PRs) is ~10× the next-busiest projects (Hermes, ZeroClaw at ~50 each). If issue volume is a proxy for installed base, it is the only project here with mainstream-level community gravity.
- **Release cadence with targeted impact.** v2026.9.2 ships fixes for its most-reported pain class (Gateway event-loop blocking during persistence/transcript maintenance), showing a fast issue→release loop that most peers lack.
- **Breadth of surface coverage.** No other project matches its channel/plugin matrix (Telegram, MSTeams, Feishu, Google Chat, QQ via plugins), Control UI/dashboard, cloud/paired worktree sessions, plugin hot-management ([#135599](https://github.com/openclaw/openclaw/pull/135599)), and ACP delivery path. LobsterAI's explicit dependency on OpenClaw's `McpBridgeServer` indicates OpenClaw already functions as an embedded runtime for sibling products.
- **Deep maintainer bench.** A single prolific maintainer (steipete) plus heavy contributor throughput is unusual in this digest; most peers show 1–3 active maintainers.

**Technical approach differences:**
- OpenClaw concentrates logic in a Gateway event loop with synchronous persistence — currently its biggest architectural liability. The v2026.9.2 fix moves durable history reads out of the event loop but explicitly does **not** close the remaining persistence-blocking work ([#119720](https://github.com/openclaw/openclaw/issues/119720)).
- ZeptoClaw and ZeroClaw are Rust-based and "protocolize" boundaries earlier: fail-closed config modes, environment scrubbing, WASM plugin sandboxes, CI security gates. OpenClaw's equivalent security work is comparatively reactive (e.g., zombie subprocesses, credential handling).
- Hermes Agent differentiates with desktop-launched but headless-capable gateway operation; OpenClaw approaches the same need via cloud/paired sessions and a Control UI.
- NanoBot is the only project doing a deliberate MessageBus/event-delivery architecture refactor ([#5670](https://github.com/HKUDS/nanobot/pull/5670)) — converging on the same queue/outbox semantics OpenClaw needs.

**Community size comparison:** OpenClaw's 24h digest alone contains more merged/closed PRs (224) than most peers will process in a quarter. ZeroClaw's 73-contributor release is the closest qualitative signal of a large contributor community, but its per-day issue/PR flow is still ~10× smaller.

---

## 4. Shared Technical Focus Areas

Requirements emerging independently across multiple projects:

1. **Transactional turn integrity — no silent drops, no duplicates.** The strongest cross-project signal. OpenClaw umbrella [#69208](https://github.com/openclaw/openclaw/issues/69208) (duplicate transcripts across channels), A2A duplicate loop [#39476](https://github.com/openclaw/openclaw/issues/39476), and silent inbound drop [#112259](https://github.com/openclaw/openclaw/issues/112259); NanoBot discarded-session revival [#5589](https://github.com/HKUDS/nanobot/pull/5589) and outbound-dispatcher death [#5457](https://github.com/HKUDS/nanobot/pull/5457); CoPaw/QwenPaw rejecting mid-turn messages with HTTP 409 instead of queueing [#7559](https://github.com/agentscope-ai/QwenPaw/issues/7559); ZeroClaw RFC [#9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487) proposing runtime-owned session semantics. **Implication:** agent frameworks need outbox/dead-letter semantics, idempotent channel delivery, and queue-while-running behavior as core primitives.

2. **Provider resilience and real failover.** NanoBot's newest bug ([#5674](https://github.com/HKUDS/nanobot/issues/5674)) shows a provider timeout being interpreted as model output, killing the agent; fix PR [#5675](https://github.com/HKUDS/nanobot/pull/5675) restores fallback after runner deadlines. OpenClaw has parallel gaps: "success" terminal state without final delivery ([#132762](https://github.com/openclaw/openclaw/issues/132762)) and fallback chains not triggering on session-limit errors ([#118793](https://github.com/openclaw/openclaw/issues/118793)). CoPaw adds hardcoded context-size failure ([#7576](https://github.com/agentscope-ai/QwenPaw/issues/7576)) and provider-config migration breakage ([#7474](https://github.com/agentscope-ai/QwenPaw/issues/7474)). **Implication:** timeouts must be classified as provider errors, not model output; fallback decisions need to be testable without waiting for real outages (see OpenClaw `/models test-fallback` request [#6599](https://github.com/openclaw/openclaw/issues/6599)).

3. **Per-session context isolation and memory bounds.** OpenClaw wants subagent context isolation ([#96975](https://github.com/openclaw/openclaw/issues/96975)) and per-turn message-tool budgets ([#119992](https://github.com/openclaw/openclaw/issues/119992)); NanoBot's dream-memory files have no size cap ([#5630](https://github.com/HKUDS/nanobot/pull/5630)); ZeptoClaw wants durable cross-session memory ([#666](https://github.com/qhkm/zeptoclaw/issues/666)); CoPaw users report agents "forgetting" workspace paths and overwriting the wrong directory ([#7571](https://github.com/agentscope-ai/QwenPaw/issues/7571)). **Implication:** session-scoped state (memory, MCP config, model routing) must be isolated by default; subagents should return lightweight summaries, not full context dumps.

4. **Subprocess and credential security.** ZeptoClaw closed two P0s this window: environment-variable leakage into plugin/MCP subprocesses and invalid `agent_mode` escalating to Autonomous ([#660](https://github.com/qhkm/zeptoclaw/issues/660), [#659](https://github.com/qhkm/zeptoclaw/issues/659)). OpenClaw has a P1 zombie-process regression ([#97616](https://github.com/openclaw/openclaw/issues/97616)); ZeroClaw is fighting macOS Seatbelt sandbox policy divergence ([#10536](https://github.com/zeroclaw-labs/zeroclaw/issues/10536)) and shipped a WASM plugin sandbox ([#5230](https://github.com/zeroclaw-labs/zeroclaw/pull/5230)). **Implication:** process spawning must scrub inherited env, kill/reap process trees on timeout, and treat sandbox policy as an enforcement layer, not documentation.

5. **Headless durability and service lifecycle.** Hermes users want bot group chats to keep running after the desktop app closes ([#97681](https://github.com/NousResearch/hermes-agent/issues/97681)); the project is adding macOS LaunchAgent lifecycle management ([#104022](https://github.com/NousResearch/hermes-agent/pull/104022)). OpenClaw is moving worktree preparation to cloud/paired nodes ([#138900](https://github.com/openclaw/openclaw/pull/138900)) and preserving restart outcomes in update flows ([#139701](https://github.com/openclaw/openclaw/pull/139701)). ZeroClaw wants direct gateway channel-send without an agent turn ([#10050](https://github.com/zeroclaw-labs/zeroclaw/issues/10050)). **Implication:** the desktop app is becoming a control plane; the agent runtime must survive as a managed daemon/service with supervised auto-update.

6. **Token and prompt-cache economics.** OpenClaw documents a ~3,500-token/session tool-schema tax ([#14785](https://github.com/openclaw/openclaw/issues/14785)) and prompt-cache busting on tool-call continuation turns ([#84110](https://github.com/openclaw/openclaw/issues/84110)); ZeptoClaw proposes a byte-stable prompt envelope ([#661](https://github.com/qhkm/zeptoclaw/issues/661)); Hermes wants compactable skills catalogs ([#72200](https://github.com/NousResearch/hermes-agent/pull/72200)). **Implication:** prompt construction order and schema loading are now cost/performance levers, not just correctness details.

---

## 5. Differentiation Analysis

| Project | Primary target | Distinctive focus | Architectural signature |
|---|---|---|---|
| **OpenClaw** | General users, power users, cloud/control-plane operators | Broadest channel/plugin breadth; live plugin lifecycle; cloud sessions | Gateway event loop + in-process channels/providers; shifting persistence off the event loop |
| **ZeroClaw** | Security-conscious operators, contributor-driven platform | Sandbox policy, WASM plugins, RFC-governed architecture, cron/session revamp | Rust-based; heavy RFC/tracker governance; release-scale contributor community |
| **Hermes Agent** | Desktop + headless gateway users (Windows/macOS) | Update/install reliability; group-chat survival; delegate/profile routing | Desktop app + launchd-managed gateway; SQLite state DB with pluggable-SessionDB demand |
| **NanoBot** | Lightweight WebUI/Desktop assistant users | Small footprint, heartbeat cost controls, event delivery correctness | MessageBus event refactor; WebUI + Python + Desktop coexistence |
| **ZeptoClaw** | Security-first/self-hosted deployments | Credential isolation, audit-chain persistence, fail-closed config | Rust + cargo-deny/Clippy gates; maintainer-driven architecture reviews |
| **CoPaw / QwenPaw** | Feishu/China-market, enterprise team deployments | Per-session MCP, skills v2, Advisor Mode, Hub multi-tenant 2.2.0 | Python backend (`_coordinator.py`, `lark_oapi`); web cowork UI; image-gen skills |
| **LobsterAI** | Web "cowork" session users | Per-session MCP toggles; chat UI performance | OpenClaw `McpBridgeServer` integration; 2,100-line `CoworkSessionDetail` frontend monolith |
| **PicoClaw** | IRC-centric/lightweight users | Long-message IRCv3 reassembly; turn-interruption semantics | Small, triage-driven maintenance; batch fix-train merges |
| **NanoClaw** | Lightweight channel-focused installs | Linux Signal reliability, setup hygiene | Small test/CI cleanup PRs; no feature pipeline |
| **IronClaw** | Benchmark/sandbox users (near.ai) | Embedded Pi sandbox as startup default | Stacked PRs around native-loop sandbox spike (#7908 → #8075) |
| NullClaw / TinyClaw / Moltis | — | — | Dormant |

The naming pattern and shared issue taxonomy suggest these projects form an **OpenClaw-adjacent family** with common lineage, but each has diverged substantially: ZeptoClaw/ZeroClaw toward memory-safe, security-hardened runtimes; CoPaw/LobsterAI toward OpenClaw-powered product surfaces; Hermes toward desktop-orchestrated fleet operation.

---

## 6. Community Momentum & Maturity

**Tier 1 — Hyperactive / shipping:** **OpenClaw** (v2026.9.2, 224 PR closures) and **ZeroClaw** (v0.8.5, 454 commits, 73 contributors) are the only projects in a true release-and-iterate rhythm. Both have enough contributor supply to sustain high throughput, and both are bottlenecked by architectural debt — OpenClaw on message integrity, ZeroClaw on RFC/maintainer decision throughput.

**Tier 2 — Steady but constrained:** **Hermes Agent** iterates on a large surface but closes very little; its `needs-decision` pile is the constraint. **NanoBot** has healthy contributor responsiveness (bug→fix PR within a day) but is losing velocity to a `conflict`-labeled PR backlog that includes two P1 security/state bugs and feature PRs waiting since June. **CoPaw/QwenPaw** triages well (3 issues closed) but merged zero PRs in-window and holds a "Ready for Merge" PR. **ZeptoClaw** is execution-strong but entirely maintainer-driven — no external community signal.

**Tier 3 — Maintenance / quiet:** **PicoClaw** completed a long-delayed fix-consolidation merge (PRs from March finally closed in September), but quickly stale-closed a 15-day-old feature proposal — a sign of deliberate scope control, not necessarily health. **NanoClaw** has three small scoped fixes awaiting review. **IronClaw** is stable with one low-risk XL PR deliberately sequenced behind a base PR. **LobsterAI** is the weakest: zero issue traffic and two `[stale]` PRs open ~160 days, indicating a review bottleneck on its core frontend component.

**Tier 4 — Dormant:** NullClaw, TinyClaw, Moltis registered no activity.

---

## 7. Trend Signals

1. **Agent reliability is now the product.** Across OpenClaw, NanoBot, and CoPaw, the most emotionally charged bugs are silent drops, duplicate deliveries, and "success" states that never deliver an answer. **For developers:** design session logs and channel delivery as an event-sourced, idempotent outbox from day one — not as an afterthought.

2. **Provider failover must distinguish infrastructure errors from model output.** NanoBot's NIM-timeout bug is a canonical failure mode: the agent consumed an error string as an assistant reply and stopped. **For developers:** error classification, health probes, and testable fallback chains are table stakes for multi-provider agents.

3. **Desktop apps are becoming control planes, not runtimes.** Hermes group chats surviving Desktop shutdown, OpenClaw cloud sessions, ZeroClaw direct gateway sends — all point to headless, daemonized agents as the expected deployment model. **For developers:** build service-lifecycle management (install/start/stop/health-gated update) early; desktop UIs should attach to a remote/long-lived runtime.

4. **Security is moving into the architecture layer.** Env-var scrubbing, fail-closed agent modes, WASM plugin sandboxes, and OS-backend sandbox parity are being treated as P0s — not hardening wishlist items. **For developers:** assume model-controlled subprocesses are hostile; scope credentials, cgroups/process trees, and filesystem access explicitly.

5. **Context and token budgets are a UX and cost surface.** Users notice when tool schemas are re-sent, when prompt cache is busted, or when subagent payloads pollute parent context. **For developers:** cache-stable prompt ordering, schema compression, and per-turn message budgets are now competitive differentiators.

6. **Multi-tenant / managed deployments are the next frontier.** CoPaw's QwenPaw Hub 2.2.0 roadmap thread (23 comments), OpenClaw's cloud/Control-UI direction, and ZeroClaw's dynamic-allowlist request all point to team-scale deployment demand. **For developers:** session isolation, admin-managed skills, and per-tenant configuration will matter more than single-user features in the next 12 months.

7. **Maintainer-review bandwidth is the ecosystem's scarcest resource.** Stale PRs (LobsterAI ~160 days, NanoBot conflict-labeled P1s, ZeroClaw's 10+ PR security stack) and high-comment RFCs awaiting decisions are the binding constraint — not contributor output. **For developers and investors:** projects with explicit decision queues, rebase hygiene, and dedicated review capacity will outperform those that merely accumulate issues and RFCs.

---

## Peer Project Reports

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

# NanoBot Project Digest — 2026-09-06

## 1. Today's Overview

NanoBot showed solid, sustained activity over the last 24 hours: 25 pull requests were updated (18 open, 7 merged/closed) and 1 new issue was filed, with no new releases. The one new bug report, [#5674](https://github.com/HKUDS/nanobot/issues/5674), describes an Nvidia NIM timeout being misinterpreted as model output, which stops the agent entirely — a fix PR ([#5675](https://github.com/HKUDS/nanobot/pull/5675)) was opened within a day, showing strong contributor responsiveness. Closed PRs visible in the data ([#5670](https://github.com/HKUDS/nanobot/pull/5670), [#5671](https://github.com/HKUDS/nanobot/pull/5671), [#5672](https://github.com/HKUDS/nanobot/pull/5672)) point to ongoing internal refactoring of event delivery, WebUI dev-mode behavior, and test cleanup. However, a large number of open PRs carry `conflict` labels — some dating back to June — suggesting that merge-conflict resolution and maintainer review bandwidth, not contributor output, are the current bottleneck. Overall project health is good: fix activity outpaces new bug reports, and the WebUI/provider/session areas are receiving concentrated attention.

## 2. Releases

**No new releases** were published in this window. The "Latest Releases" section is empty, so there are no changelog entries, breaking changes, or migration notes to report.

## 3. Project Progress

The data overview reports 7 merged/closed PRs in the last 24h; 3 of them are visible in the top-20 listing:

- **[#5670 — refactor(events): unify scoped runtime notifications across clients](https://github.com/HKUDS/nanobot/pull/5670)** *(closed)* — Replaces separate mechanisms with MessageBus for both awaited local event subscriptions and queued channel delivery of text/typed events, introducing a thin operation-scoped sender. Context compaction was migrated end-to-end in one cohesive PR while keeping wire payloads and persisted session formats compatible.
- **[#5671 — fix(cli): skip WebUI bundle check in dev mode](https://github.com/HKUDS/nanobot/pull/5671)** *(closed)* — `nanobot webui --dev` no longer warns about a stale/missing production bundle when Vite is serving the current source tree; production behavior is preserved.
- **[#5672 — test: remove obsolete nonexistence checks](https://github.com/HKUDS/nanobot/pull/5672)** *(closed)* — Deletes tests that only assert retired or never-exposed symbols/routes/wording are absent, while keeping observable-behavior, security, protocol-omission, and lazy-loading coverage (including an email-validation no-write regression check with byte comparison).

The remaining 4 closed PRs fall outside the top-20 list by comment count and are not itemized in the dataset.

## 4. Community Hot Topics

Comment/reaction tallies were not included in the dataset for most items (issue [#5674](https://github.com/HKUDS/nanobot/issues/5674) shows 0 comments/0 reactions), so the following is inferred from recency, priority tags, cross-links, and clustered activity:

- **[#5674 — Agent stops working on Nvidia NIM timeout](https://github.com/HKUDS/nanobot/issues/5674)** — The only open issue and the clearest user-reported pain point. The user notes the agent "stops working because nanobot thinks [the error] is the model output." Underlying need: provider errors must be distinguishable from model-generated text, and provider failures should trigger recovery rather than dead-stop the agent.
- **[#5675 — fix(providers): allow model failover after runner deadlines](https://github.com/HKUDS/nanobot/pull/5675)** — Direct response to #5674; identifies a deeper regression: a hanging primary model exhausts the runner deadline before `FallbackProvider` can react, so configured healthy fallbacks are never attempted. This is the issue to watch today.
- **[#5676 — feat(cli): add attach-only Desktop target selection](https://github.com/HKUDS/nanobot/pull/5676)** — Freshest feature PR. Addresses users who run both Desktop and Python installs side-by-side and want per-invocation target selection for bare `nanobot` / `nanobot webui`, with fallback handling for missing/stale/busy/untrusted Desktop states.
- **chengyongru's WebUI/event cluster** ([#5670](https://github.com/HKUDS/nanobot/pull/5670), [#5671](https://github.com/HKUDS/nanobot/pull/5671), [#5672](https://github.com/HKUDS/nanobot/pull/5672), [#5673](https://github.com/HKUDS/nanobot/pull/5673)) — Repeated high-velocity updates in one area signal an ongoing architectural push around runtime events, scoped notifications, and remote WebUI capabilities.
- **Stalled-but-watched features** — Heartbeat configuration PRs ([#4549](https://github.com/HKUDS/nanobot/pull/4549), [#4551](https://github.com/HKUDS/nanobot/pull/4551)) continue to receive update traffic after months, implying user demand for cheaper heartbeat models and shared-session control.

## 5. Bugs & Stability

Bugs active in the last 24h, ranked by severity:

1. **High — Agent hard-stop on provider timeout (newly reported)** — [#5674](https://github.com/HKUDS/nanobot/issues/5674): Nvidia NIM timeout errors ("timed out after 300s/600s") are treated as model output, killing the agent. Fix exists: [#5675](https://github.com/HKUDS/nanobot/pull/5675) (open) restores failover after runner deadlines.
2. **High (P1, security) — Session key path traversal** — [#5633](https://github.com/HKUDS/nanobot/pull/5633): untrusted session IDs like `../../etc/passwd` become file paths before persistence, potentially addressing files outside the sessions directory (fixes #5564). Fix PR open since Sep 2 but carries a `conflict` label.
3. **High (P1, performance/stability) — Session persistence on event loop** — [#5580](https://github.com/HKUDS/nanobot/pull/5580): slow storage or file-lock contention blocks the event loop and stalls unrelated conversations. Fix open since Aug 28.
4. **High (P1) — Discarded sessions can "revive"** — [#5589](https://github.com/HKUDS/nanobot/pull/5589): queued/deferred messages can still publish during task cleanup after a session is discarded. Fix PR open since Aug 28, also `conflict`-flagged.
5. **Medium (P2, regression) — Remote WebUI project path handling** — [#5673](https://github.com/HKUDS/nanobot/pull/5673): remote users cannot enter server-side absolute paths, and the UI invokes the client machine's native file picker instead of honoring gateway picker capabilities. Fix PR created 2026-09-05.
6. **Medium (P2, regression) — Dream memory files unbounded** — [#5630](https://github.com/HKUDS/nanobot/pull/5630): PR #5622 removed the only size cap on SOUL.md/USER.md/MEMORY.md; files can now grow without limit and are injected into every request. Fix open since Sep 2.
7. **Medium (P2, memory growth) — Unbounded idle summary cache** — [#5664](https://github.com/HKUDS/nanobot/pull/5664): summaries of abandoned sessions remain cached indefinitely; fix bounds the `AutoCompact._summaries` dictionary.
8. **Medium (P2) — MCP OAuth tokens not refreshed** — [#5573](https://github.com/HKUDS/nanobot/pull/5573): expired tokens fail until restart; fix persists absolute expiry/issuer metadata and refreshes proactively or after a 401.
9. **Medium (P2) — Outbound dispatcher dies on one bad message** — [#5457](https://github.com/HKUDS/nanobot/pull/5457): an error while processing one outbound message stops `ChannelManager._dispatch_outbound` until process restart.
10. **Low (P2, UX/observability) — Model retry status invisible** — [#5504](https://github.com/HKUDS/nanobot/pull/5504): retry lifecycle events are not surfaced to WebSocket/TUI/WebUI clients; fix publishes sanitized retry events with relative countdown timers.

A fix PR exists for every item above — a positive health signal — but several are blocked by merge conflicts rather than technical disagreement.

## 6. Feature Requests & Roadmap Signals

- **Heartbeat cost/context controls** — [#4549](https://github.com/HKUDS/nanobot/pull/4549) (`model_override` for a cheaper heartbeat model) and [#4551](https://github.com/HKUDS/nanobot/pull/4551) (`isolated_session` opt-out for shared-context heartbeats) have been implemented and awaiting merge since June 26. Both are labeled `conflict`; when resolved, they are strong candidates for the next release.
- **Signed direct-delivery webhook** — [#5652](https://github.com/HKUDS/nanobot/pull/5652): authenticated webhook that pushes final notification text straight to the outbound message bus, bypassing the agent loop/model calls. Targets CI, monitoring, and billing integrations; could plausibly ship next.
- **Attach-only Desktop target selection** — [#5676](https://github.com/HKUDS/nanobot/pull/5676): fresh, small-footprint CLI feature; likely to move fast if review slots open up.
- **Per-spawn model presets** — [#5561](https://github.com/HKUDS/nanobot/pull/5561): resolves #4231 with a `spawnPresets` allowlist interface; an alternative to #4291 (referenced). Still open since Aug 27.
- **MCP Apps result metadata** — [#5386](https://github.com/HKUDS/nanobot/pull/5386): preserves structured MCP tool results separately from model-facing text so app-only tools stay out of the model registry. Open since Aug 13.

**Prediction**: The heartbeat pair (#4549/#4551) and the remote-WebUI fix (#5673) are the most "release-ready" if conflicts can be cleared. #5676 and #5652 are likely next-version features; #5561 will depend on how quickly #4291's review discussion converges.

## 7. User Feedback Summary

- **Provider resilience is the top direct pain point**: the [#5674](https://github.com/HKUDS/nanobot/issues/5674) reporter describes a fully stopped agent after an Nvidia NIM timeout — evidence that users expect timeouts to be handled as provider errors, not model output, and expect a healthy fallback model to take over.
- **Silent/confusing failure modes**: Several fix PRs address issues where NanoBot fails quietly or confusingly — outbound messages silently stop (#5457), discarded sessions resurrect (#5589), retry state gives no UI feedback (#5504). These indicate users value observability and predictable lifecycle semantics.
- **Deployment topology pain**: PRs like [#5676](https://github.com/HKUDS/nanobot/pull/5676) (Desktop + Python co-existence) and [#5673](https://github.com/HKUDS/nanobot/pull/5673) (remote WebUI absolute paths) show real users running mixed installs and accessing NanoBot from remote clients, where local-vs-server file pickers are a genuine usability trap.
- **Positive signal**: the gap between bug report (#5674) and fix PR (#5675) was under a day, which should reassure users that reported regressions are triaged quickly.

No explicit satisfaction/dissatisfaction ratings were available in the dataset; the above is inferred directly from issue/PR content and timestamps.

## 8. Backlog Watch

Items needing maintainer attention, roughly by urgency:

- **[#4549](https://github.com/HKUDS/nanobot/pull/4549) & [#4551](https://github.com/HKUDS/nanobot/pull/4551)** *(open since Jun 26, `conflict`)* — Fully implemented heartbeat features blocked for over two months. They need a rebase/conflict pass and a maintainer decision; this is the oldest actionable work in the queue.
- **[#5633](https://github.com/HKUDS/nanobot/pull/5633)** *(open since Sep 2, P1 security, `conflict`)* — Session path-traversal fix. Security-labeled P1 PRs should not sit behind merge conflicts; prioritize review.
- **[#5589](https://github.com/HKUDS/nanobot/pull/5589)** *(open since Aug 28, P1, `conflict`)* — Discarded-session revival fix; high severity and aging.
- **[#5580](https://github.com/HKUDS/nanobot/pull/5580)** *(open since Aug 28, P1)* — Event-loop-blocking session persistence fix; no visible `conflict` label but still unmerged.
- **[#5386](https://github.com/HKUDS/nanobot/pull/5386)** *(open since Aug 13, `conflict`)* — MCP Apps metadata preservation; large but isolated feature.
- **[#5471](https://github.com/HKUDS/nanobot/pull/5471)** *(open since Aug 21, `conflict`)* — SDK `ephemeral=True` runs currently mutate session state despite documented behavior; a correctness fix for SDK users.
- **[#5457](https://github.com/HKUDS/nanobot/pull/5457)** *(open since Aug 20, `conflict`)* — Outbound message-delivery resilience.
- **[#5561](https://github.com/HKUDS/nanobot/pull/5561)** *(open since Aug 27, `conflict`)* — Per-spawn model presets; resolution is blocked on an alternative-implementation discussion.

**Overall observation**: more than half of the open PRs updated in the last 24h carry a `conflict` label. The project's main risk is not feature velocity but queue hygiene — a coordinated rebase pass and prioritization of P1/security items would meaningfully improve merge throughput and reduce the age of the backlog.

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent Project Digest — 2026-09-06

## 1. Today's Overview

Hermes Agent saw a high volume of issue and PR activity over the last 24 hours: **50 issues updated** (48 open / 2 closed) and **50 PRs updated** (48 open / 2 closed/merged), with **0 new releases**. The dominant themes are update/install reliability — including multiple P1 bugs around `hermes update`, Discord gateway behavior, and Windows update paths — and growing multi-agent orchestration demand, such as group-chat continuity, per-task profile routing, and pluggable session storage. Project health is mixed: there is clear ongoing contribution and maintenance movement, but several important issues have remained open for weeks or months with `needs-decision` labels, and a stale docs-index watchdog issue has accumulated 163 comments without resolution.

## 2. Releases

No new releases were published in the 2026-09-06 window. There are no release notes, breaking-change notes, or migration steps to report.

## 3. Project Progress

- Aggregate data shows **2 PRs moved to closed/merged** in the last 24 hours, but those PRs are not included in the top-20 sample, so no merged feature/fix details are available.
- One long-standing issue was closed with label `implemented-on-main`:
  - [#45876](https://github.com/NousResearch/hermes-agent/issues/45876) — `web_search` fell through to DDGS timeouts in cron sessions despite AnySearch being configured. This was a meaningful fix for mainland-China users and is now closed as implemented on `main`.
- [#104012](https://github.com/NousResearch/hermes-agent/issues/104012) was closed as withdrawn — it was submitted by an AI assistant without account-owner approval.

Notable open PRs actively updated in the window:

- [#104022](https://github.com/NousResearch/hermes-agent/pull/104022) — Adds first-class macOS LaunchAgent lifecycle management for the dashboard, implementing feature request [#44106](https://github.com/NousResearch/hermes-agent/issues/44106).
- [#103965](https://github.com/NousResearch/hermes-agent/pull/103965) — Adds per-task Hermes profile routing for `delegate_task` subagents, allowing different models, hosts, memory stores, and state DBs per task.
- [#103582](https://github.com/NousResearch/hermes-agent/pull/103582) — Fixes launchd-managed gateways being falsely reported as offline.
- [#101420](https://github.com/NousResearch/hermes-agent/pull/101420) — Adds an E2E cross-OS install/update matrix across Windows, macOS, and Linux.
- Several small connection-leak fixes were opened/updated by one contributor: [#104037](https://github.com/NousResearch/hermes-agent/pull/104037), [#104035](https://github.com/NousResearch/hermes-agent/pull/104035), [#104034](https://github.com/NousResearch/hermes-agent/pull/104034), [#104033](https://github.com/NousResearch/hermes-agent/pull/104033), and [#104024](https://github.com/NousResearch/hermes-agent/pull/104024).

## 4. Community Hot Topics

The most-commented and most-reacted items reveal ongoing user interest in making Hermes work as durable infrastructure rather than only an interactive desktop tool.

- [#66616](https://github.com/NousResearch/hermes-agent/issues/66616) — **Skills index is stale or degraded** · 163 comments
  Automated freshness probe reports the Skills Hub index is 29.8h old against a 26h limit. This is the single most active issue in the tracker. The sustained comment count suggests either the underlying docs CI/deploy pipeline is repeatedly failing or the watchdog itself is generating noise. Both need maintainer attention.

- [#97681](https://github.com/NousResearch/hermes-agent/issues/97681) — **Bot Group Chats should keep working after Desktop closes** · 23 comments
  Users want groups of Hermes bots to continue operating after the Desktop app goes away. This is a strong signal that gateway-owned, headless bot operation is a core user need.

- [#23717](https://github.com/NousResearch/hermes-agent/issues/23717) — **RFC: Pluggable SessionDB Provider — PostgreSQL, MySQL, and Beyond** · 20 comments, 8 👍
  Community interest in replacing the shared SQLite `state.db` with server-grade databases. Upvoted as a roadmap-level architecture improvement.

- [#26058](https://github.com/NousResearch/hermes-agent/issues/26058) — **Discord `auto_thread` disabled for `free_response_channels`** · 10 comments, 5 👍
  P1 regression where a previous fix broke legitimate thread auto-creation for free-response channels. Needs a maintainer decision and eventual fix.

- [#98022](https://github.com/NousResearch/hermes-agent/issues/98022) — **`hermes update` catch-up fleet restart re-fires forever** · 10 comments
  A stale interrupted receipt causes the update fleet-restart logic to trigger on every run, even when the install is current.

## 5. Bugs & Stability

Ranked by severity:

- **P1 — `hermes update` restart loop on stale receipt**  
  [#98022](https://github.com/NousResearch/hermes-agent/issues/98022)  
  The catch-up fleet restart from #95294 re-fires continuously when `update_receipts/latest.json` is a stale interrupted receipt. Users can end up in an endless fleet restart cycle. No fix PR appears in today’s sample.

- **P1 — Discord free-response channels cannot use `auto_thread`**  
  [#26058](https://github.com/NousResearch/hermes-agent/issues/26058)  
  A regression caused channels in `free_response_channels` to skip auto-thread creation entirely. The issue is marked `needs-decision`, so current behavior may require policy clarification, but the user-visible break remains P1.

- **P2 — `hermes update` creates root-owned files**  
  [#102193](https://github.com/NousResearch/hermes-agent/issues/102193) and related [#91212](https://github.com/NousResearch/hermes-agent/issues/91212)  
  Repeated user reports that update processes create root-owned files under `~/.hermes/` on non-root installs, which then breaks future updates and installs.

- **P2 — Windows update paths are fragile**  
  [#97394](https://github.com/NousResearch/hermes-agent/issues/97394) — Desktop watchdog cancels healthy Windows updates because `--gateway` mode never creates `logs/update.log`.  
  [#90495](https://github.com/NousResearch/hermes-agent/issues/90495) — ZIP fallback deletes the packaged Desktop app and `web_dist`, and also forgets Desktop was installed.  
  Both directly impact Windows users and can leave the app in an unrecoverable state.

- **P2 — Cron jobs silently lose the `web` toolset**  
  [#82912](https://github.com/NousResearch/hermes-agent/issues/82912)  
  Jobs configured with `enabled_toolsets: ["web", "file"]` only see `file` tools. The same job with only `["web"]` works correctly.

- **Newly reported / updated P2 auth and security bugs**  
  - [#103978](https://github.com/NousResearch/hermes-agent/issues/103978) — Claude Code OAuth auto-discovery refreshes a single-use token, logging the user out of Claude CLI and conflicting with Anthropic Consumer ToS. Requested opt-out.
  - [#103989](https://github.com/NousResearch/hermes-agent/issues/103989) — `hermes auth add openai-codex --type oauth` prints `Added` but never persists the credential.
  - [#103974](https://github.com/NousResearch/hermes-agent/issues/103974) — Kanban worker identity is based on env presence, so grandchild `hermes chat`/worker shells can become full workers. Proposal includes `HERMES_KANBAN_OWNER_PID`, scrub-by-default, and CLI parity.

- **Desktop-side behavior bugs**  
  - [#103900](https://github.com/NousResearch/hermes-agent/issues/103900) — Pinned sessions are local-only and do not set the canonical `pinned` flag.
  - [#103985](https://github.com/NousResearch/hermes-agent/issues/103985) — “Hide from sidebar” in worktree lanes is a silent no-op when the worktree still exists on disk.
  - [#103893](https://github.com/NousResearch/hermes-agent/issues/103893) — Group-chat hold directive misclassifies German filler words like “halt” as stop commands.

## 6. Feature Requests & Roadmap Signals

- **Storage pluggability is the clearest roadmap signal**  
  [#23717](https://github.com/NousResearch/hermes-agent/issues/23717) — RFC for PostgreSQL/MySQL SessionDB providers has 8 👍 and 20 comments. It directly targets the “hot-update death spiral” with SQLite during `git pull`/`hermes update`.

- **Multi-agent orchestration is trending heavily**  
  - [#103965](https://github.com/NousResearch/hermes-agent/pull/103965) adds per-task profile routing for subagents.
  - [#103748](https://github.com/NousResearch/hermes-agent/issues/103748) requests an official API to deliver messages into a live Hermes session.
  - [#97681](https://github.com/NousResearch/hermes-agent/issues/97681) needs group chats to survive Desktop shutdown.

- **Plan mode may return to the roadmap**  
  [#80994](https://github.com/NousResearch/hermes-agent/issues/80994) — A read-only exploration phase before file edits is still marked `needs-decision`.

- **macOS lifecycle management appears close to shipping**  
  [#44106](https://github.com/NousResearch/hermes-agent/issues/44106) is now directly addressed by open PR [#104022](https://github.com/NousResearch/hermes-agent/pull/104022), which adds `hermes dashboard service install|start|stop|restart|status|uninstall`.

Likely near-term release candidates: gateway infrastructure reliability, delegate profile routing, macOS LaunchAgent support, and update/install regression fixes.

## 7. User Feedback Summary

- **Recurring update/package complaints** are the loudest user pain point. The same reporter cluster has filed multiple issues about:
  - Root-owned files created by update ([#102193](https://github.com/NousResearch/hermes-agent/issues/102193))
  - Outdated Python packages, including `certifi` ([#83673](https://github.com/NousResearch/hermes-agent/issues/83673))
  - Broken `hermes doctor --fix` behavior around npm vulnerabilities ([#94375](https://github.com/NousResearch/hermes-agent/issues/94375))
  - Slow and unhelpful `hermes update` output ([#102540](https://github.com/NousResearch/hermes-agent/issues/102540))

- **Desktop/session state divergence** frustrates users: pinning in Desktop does not affect native Hermes sessions ([#103900](https://github.com/NousResearch/hermes-agent/issues/103900)), and hiding a worktree lane is silently undone by live rescanning ([#103985](https://github.com/NousResearch/hermes-agent/issues/103985)).

- **Auth flows that report success but do not persist** are a trust issue: [#103989](https://github.com/NousResearch/hermes-agent/issues/103989) shows “Added” being printed even though the credential never reaches `auth.json`.

- **Regional/China users are being listened to**: [#45876](https://github.com/NousResearch/hermes-agent/issues/45876) was closed as implemented on `main`, which should fix cron `web_search` fallback to DDGS when AnySearch is configured.

## 8. Backlog Watch

These issues are important, long-running, or high-noise and would benefit from maintainer action:

- [#66616](https://github.com/NousResearch/hermes-agent/issues/66616) — Skills-index freshness watchdog has been active since 2026-07-18 and now has 163 comments. Needs either a fix to the docs index pipeline or a quieter alerting process.
- [#23717](https://github.com/NousResearch/hermes-agent/issues/23717) — Pluggable SessionDB RFC has 8 👍 and 20 comments but remains `needs-decision`.
- [#26058](https://github.com/NousResearch/hermes-agent/issues/26058) — P1 Discord regression has been open since May 15 and still needs a decision or fix.
- [#102193](https://github.com/NousResearch/hermes-agent/issues/102193) — User states root-owned file creation during update has been reported “multiple times” over several months.
- [#80994](https://github.com/NousResearch/hermes-agent/issues/80994) — Plan mode feature request remains in `needs-decision` limbo since early August.

Several older, substantial PRs also remain open and were updated in the last 24 hours, suggesting review may be blocked or slow:

- [#51953](https://github.com/NousResearch/hermes-agent/pull/51953) — Fix Copilot reasoning-effort resolution from live catalog.
- [#54193](https://github.com/NousResearch/hermes-agent/pull/54193), [#54229](https://github.com/NousResearch/hermes-agent/pull/54229), [#64270](https://github.com/NousResearch/hermes-agent/pull/64270) — Mattermost thread-follow, DM session continuity, and thread-history seeding improvements.
- [#72200](https://github.com/NousResearch/hermes-agent/pull/72200) — Add `agent.skills_catalog_mode` to compact large skills catalogs.
- [#75679](https://github.com/NousResearch/hermes-agent/pull/75679) — Normalize memory provider aliases across doctor, runtime, and dashboard.
- [#80600](https://github.com/NousResearch/hermes-agent/pull/80600) — Preserve WhatsApp GIF playback container metadata.

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

# PicoClaw Project Digest — 2026-09-06

*(Section 2 — Releases — omitted: no new releases were published in this window.)*

## 1. Today's Overview

PicoClaw is in a **maintenance / backlog-cleanup phase**: in the last 24 hours, 2 issues were updated (1 still open, 1 closed as stale) and 3 fix-batch PRs were closed, while no new releases or version tags were published. The three closed PRs by **xuwei-xy** ([#1559](https://github.com/sipeed/picoclaw/pull/1559), [#1545](https://github.com/sipeed/picoclaw/pull/1545), [#1555](https://github.com/sipeed/picoclaw/pull/1555)) are consolidations of **13 earlier fix PRs**, suggesting an effort to drain a long-pending fix queue — though these PRs were created on 2026-03-14 and only resolved on 2026-09-05, a ~6-month gap that signals a chronic PR backlog. Community attention is concentrated in a single feature thread, [#3287 — Better support for long IRCv3 messages](https://github.com/sipeed/picoclaw/issues/3287) (10 comments). One competing feature proposal, an opt-in "after-turn" steering mode ([#3342](https://github.com/sipeed/picoclaw/issues/3342)), was closed as **stale** after only 15 days and minimal traction. Overall health: responsive triage, quiet feature pipeline, and fix-consolidation churn rather than new feature delivery.

## 3. Project Progress

All three PRs updated today were closed, and all are fix-consolidation merges. Given the data, no new user-facing features advanced; progress is on the **stability/merge-hygiene front**:

- **[PR #1559 — Closed — "fix: merge PR #1327 #1319 #1318 #1313"](https://github.com/sipeed/picoclaw/pull/1559)** — Consolidates fixes originating in PRs [#1327](https://github.com/sipeed/picoclaw/pull/1327), [#1319](https://github.com/sipeed/picoclaw/pull/1319), [#1318](https://github.com/sipeed/picoclaw/pull/1318), and [#1313](https://github.com/sipeed/picoclaw/pull/1313). Created 2026-03-14, closed 2026-09-05.
- **[PR #1545 — Closed — "fix: merge PR #1500 #1490 #1488 #1487 #1485"](https://github.com/sipeed/picoclaw/pull/1545)** — Consolidates fix PRs [#1500](https://github.com/sipeed/picoclaw/pull/1500), [#1490](https://github.com/sipeed/picoclaw/pull/1490), [#1488](https://github.com/sipeed/picoclaw/pull/1488), [#1487](https://github.com/sipeed/picoclaw/pull/1487), and [#1485](https://github.com/sipeed/picoclaw/pull/1485). Same ~6-month open lifecycle.
- **[PR #1555 — Closed — "fix: merge PR #1390 #1389 #1383 #1381"](https://github.com/sipeed/picoclaw/pull/1555)** — Consolidates fix PRs [#1390](https://github.com/sipeed/picoclaw/pull/1390), [#1389](https://github.com/sipeed/picoclaw/pull/1389), [#1383](https://github.com/sipeed/picoclaw/pull/1383), and [#1381](https://github.com/sipeed/picoclaw/pull/1381).

These are batch "merge train" PRs; the exact bug fixes they carried are not itemized in the available data, so the associated underlying fix PRs should be verified as merged or still open.

## 4. Community Hot Topics

The only issue with significant engagement is a clear hotspot; the rest of the activity was quiet:

- **[#3287 [Open] [Feature] "Better support long messages in IRC"](https://github.com/sipeed/picoclaw/issues/3287) — 10 comments** — Author **superuser-does** describes a real usability gap: IRC limits messages to ~512 bytes and newlines delimit messages, so long messages are split by IRC clients into fragments. The request is that PicoClaw recognize these fragments as **one cohesive message** instead of scattering or misinterpreting them. With 10 comments and no maintainer resolution since 2026-07-22, this is the community's most active discussion and a probable roadmap candidate.
- **[#3342 [Closed/stale] "Opt-in 'after-turn' steering mode"](https://github.com/sipeed/picoclaw/issues/3342) — 2 comments** — Low engagement, now closed; more detail below.

No PRs drew discussion comments.

## 5. Bugs & Stability

No new bug reports, crashes, or regressions were filed in the last 24h. Bug-related activity was limited to the three batch fix-PR closures. The titled "fix:" PRs (`#1559`, `#1545`, `#1555`) aggregate fixes from 13 earlier PRs and represent the true stability work being absorbed into the codebase, though the specific defects they address are not enumerated in the available metadata.

One behavioral friction point, arguably adjacent to stability, is documented in the now-closed [#3342](https://github.com/sipeed/picoclaw/issues/3342): when a user sends a second message mid-turn, current steering behavior **skips remaining tool calls** of the running task and injects the new message as a course correction. Users may experience lost/incomplete work in that flow, but the proposal to change it was closed as stale, meaning it is **not currently a maintainer priority** and no fix PR is associated with it. Ranked severity: low-to-moderate; no crashes or regressions reported.

## 6. Feature Requests & Roadmap Signals

- **[#3287 — Long-message IRC support](https://github.com/sipeed/picoclaw/issues/3287)** — Open, 10 comments, created 2026-07-22. This is the strongest roadmap signal today. Because PicoClaw itself appears IRC-connected and IRCv3 fragmentation is a predictable interoperability issue, **this is a good candidate for an upcoming release**. A likely implementation would involve reassembling IRCv3 message fragments (or correctly splitting outbound messages at the 512-byte boundary) while preserving the conversation context.
- **[#3342 — Opt-in "after-turn" steering mode](https://github.com/sipeed/picoclaw/issues/3342)** — Requested that busy-session user messages be **queued** rather than interrupting and skipping the running turn's remaining tool calls (opt-in). Closed as **[stale]** after only 2 comments and 0 👍, which is a signal maintainers are **not planning this in the near term**. It may resurface if more users hit the interruption behavior.

Predictive take: #3287-type IRC/chat robustness work is more likely in the next minor release than steering-mode changes; the project appears to favor fixing existing protocol behavior over adding new turn-management configuration.

## 7. User Feedback Summary

- **IRCv3 fragmentation is a concrete pain point.** Users sending long messages over IRC see them split at client/protocol boundaries; they expect the assistant to treat a split message as a single utterance. This indicates PicoClaw's IRC integration needs protocol-aware framing, not just line-based parsing.
- **Interruption semantics surprise users.** The #3342 discussion (now stale) reveals that a queued user message mid-turn aborts the remaining tool calls of task #1, effectively discarding in-flight work. This is framed as an intentional steering feature by the design docs, but the requester explicitly wanted an opt-in queue/"after-turn" alternative.
- **Engagement is narrow.** With only one issue attracting meaningful comments and zero 👍 across all updated items, community energy is currently low. The maintainers' stale-closure of #3342 suggests they are consciously deciding where to spend attention.

## 8. Backlog Watch

- **[#3287 — Long IRCv3 messages](https://github.com/sipeed/picoclaw/issues/3287)** — ⚠️ Open since **2026-07-22** (~6 weeks) with 10 comments and no maintainer resolution. This is the single most attention-worthy item: it is actively discussed, unanswered, and affects real IRC usage.
- **Underlying fix PRs referenced by today's merges** — The batch PRs (`#1559`, `#1545`, `#1555`) reference "open PRs" `#1327`, `#1319`, `#1318`, `#1313`, `#1500`, `#1490`, `#1488`, `#1487`, `#1485`, `#1390`, `#1389`, `#1383`, `#1381`. Since the batch merges were created in March but only closed in September, verifiers should confirm each referenced PR was actually merged and closed — otherwise the true fixes may still be sitting unresolved.
- **[#3342 was closed as stale](https://github.com/sipeed/picoclaw/issues/3342)** — created 2026-08-21, closed 2026-09-05, only 2 comments. Worth monitoring only if the "skipped tool calls" interruption complaint recurs among users.

**Data source:** [github.com/sipeed/picoclaw](https://github.com/sipeed/picoclaw) activity window ending 2026-09-05/06.

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw Project Digest — 2026-09-06

## 1. Today's Overview

NanoClaw is in a low-activity maintenance phase: no Issues were updated in the last 24 hours and no new releases were published. Three pull requests were updated and remain open, all submitted by external contributors. There are no merged or closed PRs today, so no completed changes landed in the repository. The active PRs focus on concrete quality-of-life fixes: a Signal dependency bug, test-suite cleanup, and stale provider documentation. Overall, the project appears quiet but still receiving community-driven patches.

## 2. Releases

No new NanoClaw releases were published in this period, so there are no changelog entries, breaking changes, or migration notes to report.

## 3. Project Progress

No PRs were merged or closed in the last 24 hours, so no changes officially advanced into the codebase. However, three open PRs represent ongoing proposed improvements:

- [#3725 — fix(setup): pin Linux signal-cli to 0.14.7](https://github.com/nanocoai/nanoclaw/pull/3725)  
  Fixes the Linux Signal installer so new installs no longer use `signal-cli` 0.14.3, which can hang forever when messaging a contact without an existing session.

- [#3710 — test: remove the temp directories the suite leaves behind](https://github.com/nanocoai/nanoclaw/pull/3710)  
  Cleans up roughly 355 OS temp directories left behind per full `pnpm test` run, preventing accumulation on long-lived dev and CI machines.

- [#3724 — Update retired model id in the add-opencode Anthropic example](https://github.com/nanocoai/nanoclaw/pull/3724)  
  Replaces the retired `anthropic/claude-sonnet-4-20250514` model id in the `add-opencode` skill with `anthropic/claude-sonnet-5`.

## 4. Community Hot Topics

There were no Issues updated in the last 24 hours, and the three active PRs have no recorded comments or reactions, so no thread qualifies as highly discussed. Still, these PRs reveal community concerns:

- [#3725](https://github.com/nanocoai/nanoclaw/pull/3725) targets `area/channels` and `area/setup-installation`, showing user need for reliable Linux Signal setup rather than a default version known to hang.
- [#3710](https://github.com/nanocoai/nanoclaw/pull/3710) touches setup, containers, CLI, and skills test infrastructure, reflecting maintainers/contributors who run tests often and care about dev-box hygiene.
- [#3724](https://github.com/nanocoai/nanoclaw/pull/3724) updates a provider example in a skill, indicating a need to keep externally referenced model ids current.

## 5. Bugs & Stability

No new bug reports were filed as Issues. Open PRs describe the following stability problems:

1. **High — `signal-cli` 0.14.3 can hang indefinitely**  
   Linux setup installs `signal-cli` 0.14.3, which can hang forever when sending to a contact with no existing session.  
   Fix PR: [#3725](https://github.com/nanocoai/nanoclaw/pull/3725), pinning to 0.14.7.

2. **Medium — Test suite leaves ~355 temp dirs per run**  
   `pnpm test` does not clean up after itself; on long-lived dev machines or tmpfs-backed `/tmp`, storage can accumulate until reboot or cleanup.  
   Fix PR: [#3710](https://github.com/nanocoai/nanoclaw/pull/3710).

3. **Low/Medium — Example uses retired Anthropic model id**  
   The `add-opencode` skill example references a model retired on 15 June 2026, which could cause failures for users following the example.  
   Fix PR: [#3724](https://github.com/nanocoai/nanoclaw/pull/3724).

## 6. Feature Requests & Roadmap Signals

No explicit feature-request Issues were reported. The open PRs do not introduce new user-facing features, but they signal maintenance priorities for upcoming releases:

- Keeping channel setup scripts pinned to reliable dependency versions.
- Improving test infrastructure cleanup for contributor and CI experience.
- Updating skill/provider examples as external APIs retire model versions.

Given the small, maintenance-focused set of changes, the next NanoClaw version may be a patch release incorporating these fixes. No larger roadmap signals are visible in the current data.

## 7. User Feedback Summary

Direct user feedback is limited because no Issues or comment/reaction activity are available in this window. Indirect signals from PR summaries include:

- Linux users or maintainers encountered or anticipated a real failure mode with `signal-cli` 0.14.3, particularly around first-time messaging sessions.
- Developers running the full test suite are dissatisfied with leftover temp files piling up, especially on tmpfs-backed systems.
- Users of the `add-opencode` skill could receive outdated Anthropic model guidance, indicating minor frustration with stale provider defaults.

These are practical, reproducible pain points rather than speculative requests.

## 8. Backlog Watch

No long-unanswered Issues are currently listed, and no backlog items stood out for maintainer response. The open PRs are worth triaging:

- [#3710](https://github.com/nanocoai/nanoclaw/pull/3710) has been open since 2026-09-03 — the oldest item in the active set.
- [#3725](https://github.com/nanocoai/nanoclaw/pull/3725) and [#3724](https://github.com/nanocoai/nanoclaw/pull/3724) are recent and should not yet be considered stale.

All three are small, well-scoped fixes and are candidates for review and merge in the next maintainer pass.

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw Project Digest — 2026-09-06

## 1. Today's Overview

Activity in the last 24 hours was light but signal-rich: one open bug issue and one open feature PR were updated, with no new releases, merges, or closures. The open issue (#8074) documents a narrow but user-facing messaging bug in the shared-channel connection flow, while the open PR (#8075) pushes forward a sandbox default-boot change explicitly requested for benchmark use. Both items were created/updated between Sep 4–5, indicating a steady, deliberate cadence rather than a burst of churn. Overall project health looks stable: no crashes, regressions, or breaking changes were reported in this window, and the one open PR carries a "low risk" label.

## 2. Releases

No new releases were published in this window.

## 3. Project Progress

No PRs were merged or closed in the last 24 hours. The only progress signal is continued work on:

- **[PR #8075 — feat: make the embedded Pi sandbox loop the startup default](https://github.com/nearai/ironclaw/pull/8075)** *(open, XL, low risk, sandbox + docs scope, core contributor)* — Adds a pinned Bun/Pi agent-core worker to the sandbox image and makes the embedded Pi sandbox loop the default for fresh startup, explicitly requested for benchmark use. The default boot profile is now the hosted-sandbox variant.
  - ⚠️ This PR is **stacked on #7908** (base: `feat/7903-native-loop-sandbox-spike`) and must not be merged before the base PR lands.

## 4. Community Hot Topics

Engagement is minimal this window, with zero reactions and one comment across all surfaced items:

- **[Issue #8074 — Paired user's rejected action in a not-connected shared channel gets the pairing notice copy instead of channel-not-connected copy](https://github.com/nearai/ironclaw/issues/8074)** — 1 comment. The discussion is early-stage, but the underlying need is clear: users expect contextual error copy that matches their *actual* authentication/connection state rather than generic fallback text. For paired users, a "connect your account in the IronClaw web app" nudge is not just unhelpful—it's actively confusing.

No other issue or PR accumulated meaningful discussion; community activity this period is effectively dormant.

## 5. Bugs & Stability

One open bug was updated in the last 24 hours:

- **[#8074 — Wrong notice copy for paired users in a not-connected shared channel](https://github.com/nearai/ironclaw/issues/8074)** *(open, created Sep 4, updated Sep 5)*
  - **Severity: Low–Medium.** This is a UX/messaging defect, not a crash, data-loss, or security issue. A **paired** user acting in a shared channel that is **not connected** for the installation receives the manifest's `connect_required` notice — copy written for the *unpaired-actor* case, telling them to "connect your account in the IronClaw web app" — instead of messaging that the channel itself is not connected.
  - **Impact:** Misleading guidance could send already-paired users on a needless trip to the web app and obscure the real remediation (connecting the channel/installation). It may also indicate a broader class of copy-selection logic that keys off the wrong actor state.
  - **Fix status:** No associated fix PR yet; the issue has only 1 comment.

## 6. Feature Requests & Roadmap Signals

The clearest roadmap signal this period is **[PR #8075](https://github.com/nearai/ironclaw/pull/8075)**, which makes the embedded Pi sandbox loop the startup default "as explicitly requested for benchmark use." This suggests:

- Benchmarking is a driving use case for the embedded sandbox, and the team is prioritizing a frictionless default boot experience for that workflow.
- Expect the "hosted Pi sandbox as default" behavior, plus accompanying sandbox/docs updates, in a coming release — likely once the base PR #7908 merges.
- The active `feat/7903-native-loop-sandbox-spike` branch signals that native loop-sandbox work is still an in-flight initiative.

No new user-submitted feature requests appeared in this window.

## 7. User Feedback Summary

The only direct user signal is the bug report behind [#8074](https://github.com/nearai/ironclaw/issues/8074), which highlights a real pain point in the shared-channel experience:

- **Pain point:** Already-authenticated (paired) users are being told to connect their account when the actual problem is that the *channel/installation* isn't connected. The messaging fails to distinguish between two very different failure states, causing confusion and misdirected troubleshooting.
- **Expectation:** Users want error/notice copy that adapts to their actual state — paired vs. unpaired — and that names the real blocker ("channel is not connected") rather than a generic fallback.
- **Satisfaction impression:** No explicit praise or complaints beyond this issue; the low issue/PR volume and stable, mergeable-feature pipeline suggest a reasonably healthy project with no acute community distress.

## 8. Backlog Watch

No items in this window qualify as long-unanswered in the strict sense:

- **[Issue #8074](https://github.com/nearai/ironclaw/issues/8074)** was updated Sep 5 (within the last 24h), so it is freshly attended.
- **[PR #8075](https://github.com/nearai/ironclaw/pull/8075)** is open but intentionally blocked by its dependency on base PR #7908 — this is an explicit sequencing constraint, not neglect.

**Monitor:** PR #8075's dependency chain (#7908 / `feat/7903-native-loop-sandbox-spike`) is the main thing to watch — if the base PR stalls, #8075 (and the benchmark-default roadmap item) stalls with it.

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# LobsterAI Project Digest — 2026-09-06

Data source: [github.com/netease-youdao/LobsterAI](https://github.com/netease-youdao/LobsterAI)

## 1. Today's Overview

LobsterAI is in a low-activity maintenance window as of 2026-09-06: **zero issues** were opened or updated, **no releases** shipped, and **no PRs were merged or closed** in the last 24 hours. The only tracked movement is that two long-open, `[stale]`-flagged pull requests ([#1069](https://github.com/netease-youdao/LobsterAI/pull/1069), [#1070](https://github.com/netease-youdao/LobsterAI/pull/1070)) registered their most recent updates on 2026-09-05, still in review limbo. Both were created on 2026-03-30, meaning they have now been open for roughly five months awaiting maintainer decision. Activity is concentrated entirely in the cowork/agent-session feature area, and the project's health signal is one of **review bottleneck rather than user-reported churn**: the issue tracker is clear, but substantive pull requests are not being processed through to merge or closure.

## 2. Releases

No new releases (none published in this window).

## 3. Project Progress

No features or fixes advanced into the mainline in the last 24 hours — both PRs updated remain **open** and unmerged. Their proposed work, still pending review, includes:

- **[#1069 — Refactor: split `CoworkSessionDetail` monolith to improve maintainability and rendering performance](https://github.com/netease-youdao/LobsterAI/pull/1069)** (author: stone333) — Decomposes the 2,100+ line core chat-page component into dedicated files (`CoworkSessionDetail.types.ts` plus type/UI/hook separation), aiming to eliminate unnecessary re-renders of unrelated historical messages during streaming output and to make pure functions independently testable.
- **[#1070 — feat(cowork): support per-session MCP switch control](https://github.com/netease-youdao/LobsterAI/pull/1070)** (author: vdorchan) — Adds an MCP control button in the session input toolbar with per-server toggles, persisting each session's MCP configuration to the DB, and enforces those switches at the OpenClaw `McpBridgeServer` request-interception layer.

While the 24-hour window closed no PRs, the September 5 activity on both suggests they have not been silently abandoned by their authors; however, no merge progress was recorded.

## 4. Community Hot Topics

With zero open issues and no comment/reaction data surfaced for either PR, the discussion surface is limited to the two open pull requests:

- [PR #1069](https://github.com/netease-youdao/LobsterAI/pull/1069) and [PR #1070](https://github.com/netease-youdao/LobsterAI/pull/1070) are effectively the entire active community/output surface. No comment counts were reported, indicating long-running PRs with little conversational engagement.

**Underlying needs analysis:**
- **#1069** reflects internal developer pain: `CoworkSessionDetail.tsx` has become a maintenance liability (2,100+ mixed lines of components, inline pure functions, and custom hooks), and streaming performance is degraded by top-level state updates triggering re-renders of unrelated history. The subtext is a need for **code-health investment** in the core conversational UI.
- **#1070** signals a genuine user-facing capability gap: MCP servers are currently global-only, forcing all sessions to share one configuration. The requested per-session toggles with DB persistence map to real workflows where different sessions serve different tasks/toolsets.

## 5. Bugs & Stability

**No new bugs, crashes, or regressions were reported in the last 24 hours** — the open/active and closed issue counts both sit at 0. Notably, the performance concern raised in [#1069](https://github.com/netease-youdao/LobsterAI/pull/1069) ("streaming output causes unnecessary re-renders of unrelated history") is a potential stability/UX degradation vector, but it is framed as an architectural refactor rather than an urgent reported defect. No hotfix PRs exist for any outstanding runtime issues. From a severity standpoint, the project is currently at its cleanest bug-report state, though this may reflect reduced end-user activity as much as genuine stability.

## 6. Feature Requests & Roadmap Signals

The clearest forward-looking signal is **[PR #1070](https://github.com/netease-youdao/LobsterAI/pull/1070), per-session MCP control** — a complete, implementable feature (UI Popover, server toggle list, DB persistence, engine-level enforcement). If merged, this would be a strong candidate for the next release's headline capability, enabling OpenClaw-based setups to scope tool access per conversation.

Secondary signals:

- **Per-session state isolation in general**: both PRs orbit the same theme — decoupling shared state/config (rendering state in #1069, MCP config in #1070) so that sessions behave independently.
- **Component/test architecture** (from [#1069](https://github.com/netease-youdao/LobsterAI/pull/1069)): the push toward types, Hooks, and UI as separate testable units suggests a planned increase in test coverage and developer velocity on the chat/cowork surface.
- The persistence requirement in #1070 indicates a **cross-restart session memory** expectation in the product roadmap.

Prediction: per-session MCP configuration is the feature most likely to land in a future minor version, contingent on the maintainer resolving the current PR backlog.

## 7. User Feedback Summary

Direct user feedback data is sparse (0 issue reports, no PR comment counts available), so the most reliable signals come from the stated motivations of the two PR authors:

- **Pain point — monolithic UI code**: developers and maintainers find the 2,100+ line `CoworkSessionDetail.tsx` difficult to navigate and maintain; inline logic cannot be independently tested.
- **Pain point — rendering inefficiency**: during streaming output, unrelated historical messages re-render because of top-level state churn, implying inflated CPU/compositor load in long conversations.
- **Pain point — rigid global configuration**: MCP servers can only be enabled/disabled globally; users cannot tailor tools per session, forcing awkward workflow compromises for multi-purpose assistant usage.
- No satisfaction metrics, bug reports, or user complaints were recorded in this window; the absence of issue traffic either indicates quiet stability or reduced active feedback.

## 8. Backlog Watch

Both pull requests in this digest qualify as at-risk backlog items and warrant immediate maintainer attention:

1. **[#1069 — CoworkSessionDetail refactor](https://github.com/netease-youdao/LobsterAI/pull/1069)** — Open **since 2026-03-30 (~160 days)**, flagged `[stale]`. Touches the most critical component in the chat UI; the longer it sits, the greater the conflict/bit-rot risk with ongoing cowork-feature development.
2. **[#1070 — per-session MCP toggle](https://github.com/netease-youdao/LobsterAI/pull/1070)** — Open **since 2026-03-30 (~160 days)**, flagged `[stale]`. This is a user-facing feature with architectural impact (OpenClaw `McpBridgeServer` changes) that cannot proceed without maintainer review.

With an empty issue tracker, the project's real backlog is these two review-blocked PRs. Maintainers should either assign reviewers, request concrete changes, or explicitly close/reject them — the current `[stale]` holding pattern is the single largest project-health risk visible in this data, as both branches will continue to diverge from the mainline.

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

# CoPaw / QwenPaw Project Digest — 2026-09-06

> Data window: last 24h · Issue/PR links point to `agentscope-ai/QwenPaw`.

## 1. Today's Overview

QwenPaw saw a moderately active 24h period: 11 issues were updated, with 8 open/active and 3 closed, while 7 PRs were updated and none were merged or closed. No release was published. The main community gravity is the roadmap discussion around QwenPaw Hub multi-tenant 2.2.0, while fast-moving bug fixes from first-time contributors target two notable reliability issues: console/HTTP 409 queue behavior and swallowed tool-call exception stacks. Overall project health is stable but mixed: issue triage is moving, three bug reports were closed, and contributor onboarding appears healthy, but maintainer merge throughput was flat and several user-facing reliability threads remain open.

## 2. Releases

No new versions or releases were published in the last 24h. There are no changelog, breaking-change, or migration notes to report for this window.

## 3. Project Progress

No PRs were merged or closed in the observed 24h window. This means no completed code integration landed today.

However, several active PRs advanced:

- [#7547](https://github.com/agentscope-ai/QwenPaw/pull/7547) — fix for stuck session queue consumers, likely related to Feishu channel reliability.
- [#7546](https://github.com/agentscope-ai/QwenPaw/pull/7546) — lazy-load unused builtin channel modules to avoid heavy startup costs such as `lark_oapi`.
- [#7577](https://github.com/agentscope-ai/QwenPaw/pull/7577) — makes follow-up messages queue correctly when a chat task is already running; directly addresses the 409 issue in [#7559](https://github.com/agentscope-ai/QwenPaw/issues/7559).
- [#7578](https://github.com/agentscope-ai/QwenPaw/pull/7578) — adds `logger.exception` in `_coordinator.py` `_drain()` so tool-chain failures are not silently swallowed; addresses [#7572](https://github.com/agentscope-ai/QwenPaw/issues/7572).
- [#7509](https://github.com/agentscope-ai/QwenPaw/pull/7509) — “Make Skill v2” workflow is marked Ready for Merge.
- [#7569](https://github.com/agentscope-ai/QwenPaw/pull/7569) — new Advisor Mode pairing a stronger advisor model with a worker model.
- [#6874](https://github.com/agentscope-ai/QwenPaw/pull/6874) — configurable MCP tool-call timeout, still under review.

Three issues were closed: [#7474](https://github.com/agentscope-ai/QwenPaw/issues/7474), [#7574](https://github.com/agentscope-ai/QwenPaw/issues/7574), and [#7575](https://github.com/agentscope-ai/QwenPaw/issues/7575), indicating progress on custom-provider and image-generation skill bugs, even without a merged PR in this particular window.

## 4. Community Hot Topics

- [#7318 — QwenPaw Hub multi-tenant edition: what should we build next?](https://github.com/agentscope-ai/QwenPaw/issues/7318)  
  **23 comments · 3 👍**  
  This is by far the hottest issue. It reflects strong community demand for team/multi-user usage, admin-managed skills, and moving beyond the single-person assistant model. The thread is both a roadmap request and a signal that large groups/companies want centrally managed QwenPaw deployments.

- [#7559 — HTTP 409 while task is running: new message should be queued](https://github.com/agentscope-ai/QwenPaw/issues/7559)  
  **5 comments**  
  Users expect a non-blocking interaction model: during a long-running task, sending a new message or file should enqueue, not return “A task is already running…”. The underlying need is better session concurrency and interactive control.

- [#7474 — Custom provider fails to load after `ModelInfo.max_tokens` migration](https://github.com/agentscope-ai/QwenPaw/issues/7474)  
  **5 comments · Closed**  
  This release/compat migration caused real breakage for users with manually configured custom providers. The discussion signals that configuration migrations need backward-compatibility paths and clearer error messages.

## 5. Bugs & Stability

| Severity | Bug / Issue | Impact | Status / Fix |
|---|---|---|---|
| High | [#7572 — `_coordinator.py` `_drain()` swallows exception stacks](https://github.com/agentscope-ai/QwenPaw/issues/7572) | Tool-handler failures are logged nowhere, making production debugging very difficult. | Open. Fix PR [#7578](https://github.com/agentscope-ai/QwenPaw/pull/7578) exists. |
| High | [#7576 — RetryChatModel hardcoded 32768 context_size causes CONTEXT_UNFIT](https://github.com/agentscope-ai/QwenPaw/issues/7576) | Models with context usage above 31130 tokens fail with CONTEXT_UNFIT across v2.1.0–v2.2.0. | Open. No fix PR in this window. |
| High | [#7559 — New messages during running task return 409 instead of queueing](https://github.com/agentscope-ai/QwenPaw/issues/7559) | Users cannot send follow-ups or files mid-task, breaking expected agent interaction. | Open. Fix PR [#7577](https://github.com/agentscope-ai/QwenPaw/pull/7577) exists. |
| Medium | [#7571 — Agent repeatedly forgets rules and workspace paths](https://github.com/agentscope-ai/QwenPaw/issues/7571) | User reports model forgetting development/deployment paths, leading to wrong-path development and destructive overwrites. | Open. No fix PR. Needs deeper triage of memory/instruction persistence. |
| Medium | [#7474 — Custom provider broken by `max_tokens` → `max_output_length` migration](https://github.com/agentscope-ai/QwenPaw/issues/7474) | Existing custom provider configs no longer load. | Closed. |
| Medium | [#7574 — img-gen `openai_images.py` omits `model` field](https://github.com/agentscope-ai/QwenPaw/issues/7574) | Image generation falls back or fails with HTTP 503/missing model. | Closed. |
| Medium | [#7575 — img-gen `edit()` always sends `response_format`](https://github.com/agentscope-ai/QwenPaw/issues/7575) | HTTP 400 on `gpt-image-2` edit endpoint. | Closed. |

## 6. Feature Requests & Roadmap Signals

The clearest roadmap signal is the announced **QwenPaw Hub multi-tenant edition in 2.2.0**, with community input requested in [#7318](https://github.com/agentscope-ai/QwenPaw/issues/7318).

Other notable feature signals:

- [#7557 — Version & dependency metadata for skills / `skill_pool`](https://github.com/agentscope-ai/QwenPaw/issues/7557)  
  A fleet operator managing 9 agents requests skill versioning, workspace tracking, and dependency metadata.

- [#7573 — “Edit last message” and “Rewind” controls in Web UI](https://github.com/agentscope-ai/QwenPaw/issues/7573)  
  Users want to correct prompts or roll back sessions without restarting.

- [#7570 — Feishu streaming card: auto-collapse thinking process after output](https://github.com/agentscope-ai/QwenPaw/issues/7570)  
  Long “thinking process” cards push final answers too far down; user verified a local JSON 2.0 collapsible-panel workaround.

Likely next-version candidates: small UX fixes such as [#7573](https://github.com/agentscope-ai/QwenPaw/issues/7573) and [#7570](https://github.com/agentscope-ai/QwenPaw/issues/7570) could land in a 2.2.x patch, while [#7557](https://github.com/agentscope-ai/QwenPaw/issues/7557) fits the fleet/Hub direction of 2.2.0. Open feature PRs such as [Advisor Mode #7569](https://github.com/agentscope-ai/QwenPaw/pull/7569), [Make Skill v2 #7509](https://github.com/agentscope-ai/QwenPaw/pull/7509), and [MCP timeout #6874](https://github.com/agentscope-ai/QwenPaw/pull/6874) are likely continued roadmap content.

## 7. User Feedback Summary

User sentiment is engaged but pressured by reliability and migration friction.

- **Team demand is high**: users repeatedly asked for multi-user support and admin-managed skills, leading to QwenPaw Hub ([#7318](https://github.com/agentscope-ai/QwenPaw/issues/7318)).
- **Queueing expectations are clear**: during task execution, users expect follow-up messages to be queued, not rejected with 409 ([#7559](https://github.com/agentscope-ai/QwenPaw/issues/7559)).
- **Memory/path discipline is a pain point**: one user reported repeated agent “forgetting,” causing development in the wrong path and dangerous overwrites via deploy scripts ([#7571](https://github.com/agentscope-ai/QwenPaw/issues/7571)).
- **Config migration caused real-world frustration**: custom provider setups broke after the `ModelInfo.max_tokens` migration ([#7474](https://github.com/agentscope-ai/QwenPaw/issues/7474)).
- **Observability concerns**: users and operators cannot debug failures because exception stacks are intentionally swallowed by the tool coordinator ([#7572](https://github.com/agentscope-ai/QwenPaw/issues/7572)).
- **Positive signal**: one user reported that their Feishu auto-collapse patch worked steadily in local testing ([#7570](https://github.com/agentscope-ai/QwenPaw/issues/7570)).

## 8. Backlog Watch

- [#6874 — Under Review: configurable MCP tool-call timeout](https://github.com/agentscope-ai/QwenPaw/pull/6874)  
  Open since **2026-08-10** and still under review nearly a month later. This PR improves MCP HTTP/SSE timeout behavior and deserves maintainer decision/merge attention.

- [#7509 — Ready for Merge: Make Skill v2](https://github.com/agentscope-ai/QwenPaw/pull/7509)  
  Marked Ready for Merge since early September but still open. If it is genuinely ready, it should be reviewed/merged promptly or moved back to needs-work.

- [#7318 — QwenPaw Hub roadmap discussion](https://github.com/agentscope-ai/QwenPaw/issues/7318)  
  With 23 comments and only 3 👍, this is a high-maintenance roadmap thread. Maintainers should close the loop on which Hub capabilities will actually be built.

- [#7571 — Agent instruction/path memory failures](https://github.com/agentscope-ai/QwenPaw/issues/7571)  
  Not strictly “old,” but serious and not tied to a fix PR. It needs maintainer triage to determine whether this is an agent behavior bug, a planner issue, or an intended memory limitation.

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

# ZeptoClaw Project Digest — 2026-09-06

## 1. Today's Overview

ZeptoClaw shows a high-activity, security-focused development day. 14 issues and 8 PRs were updated in the reported window; 4 issues were closed and 3 PRs were closed/merged, all centered on security remediation and CI restoration. The two P0 security findings from the latest architecture review — environment scrubbing and invalid `agent_mode` fallback — now have closing fixes. No new releases were published. Ten open P2-high issues describe a broader roadmap focused on durability, extension architecture, pipeline migration, and prompt-cache optimization, suggesting maintainers are moving from security hardening toward structural refactors.

## 2. Releases

No releases were published during this period.

## 3. Project Progress

The following closed/merged PRs represent the day’s main feature and security progress:

- [qhkm/zeptoclaw PR #672](https://github.com/qhkm/zeptoclaw/pull/672) — **Closed, security fix for P0 #660**: scrubs inherited environment variables at plugin and MCP spawn sites, closing the remaining credential-leak paths for agent-controlled subprocesses.
- [qhkm/zeptoclaw PR #671](https://github.com/qhkm/zeptoclaw/pull/671) — **Closed, security fix for P0 #659**: invalid/unknown `agent_mode` now fails closed to `Assistant` instead of escalating to `Autonomous`; permission escalation via config typo is no longer possible.
- [qhkm/zeptoclaw PR #645](https://github.com/qhkm/zeptoclaw/pull/645) — **Closed, runtime fix**: subprocess environments are scrubbed, and timed-out process trees are terminated and reaped; resolves [Issue #644](https://github.com/qhkm/zeptoclaw/issues/644).
- [qhkm/zeptoclaw Issue #646](https://github.com/qhkm/zeptoclaw/issues/646) — **Closed, CI fix**: restored Clippy and cargo-deny checks on the current toolchain, addressing five new Clippy warnings and vulnerable `quick-xml 0.39.2` / `lopdf 0.40.0` dependencies.

Overall, the project advanced primarily on **credential isolation**, **subprocess lifecycle correctness**, and **CI baseline enforcement**.

## 4. Community Hot Topics

Comment activity was low overall, but two issues drew the only comments:

- [qhkm/zeptoclaw Issue #646 — CI restore for Clippy and cargo-deny](https://github.com/qhkm/zeptoclaw/issues/646) — 3 comments. This was the most active discussion. The underlying need is **CI trustworthiness**: the fix for runtime subprocesses exposed pre-existing toolchain failures, and maintainers need lint and dependency-license/security gates back on the main branch before further dependency work.
- [qhkm/zeptoclaw Issue #644 — Subprocess environment scrubbing / process-tree cleanup](https://github.com/qhkm/zeptoclaw/issues/644) — 1 comment. This reflects the core user-facing concern that model-authored shell commands and spawned tools should **not inherit unrelated credentials**, and that timeouts should not orphan process trees.

No reaction data was reported. All issues in this window are maintainer-authored rather than external community-submitted, so “hot topics” here indicate maintainer priority rather than user-volume signals.

## 5. Bugs & Stability

The most serious stability/security issues updated today were:

1. **P0 security — environment variable leakage into plugin/MCP subprocesses**  
   [Issue #660](https://github.com/qhkm/zeptoclaw/issues/660) — child `Command` spawn sites did not scrub inherited credentials.  
   **Fix:** [PR #672](https://github.com/qhkm/zeptoclaw/pull/672).

2. **P0 security — invalid `agent_mode` falls back to Autonomous**  
   [Issue #659](https://github.com/qhkm/zeptoclaw/issues/659) — a typo or unknown config value would grant maximum permissions.  
   **Fix:** [PR #671](https://github.com/qhkm/zeptoclaw/pull/671).

3. **P1 safety — subprocess env leaks and orphaned process trees on timeout**  
   [Issue #644](https://github.com/qhkm/zeptoclaw/issues/644) — runtime shell commands inherited the full ZeptoClaw environment, and timeouts did not consistently terminate/reap process trees.  
   **Fix:** [PR #645](https://github.com/qhkm/zeptoclaw/pull/645).

4. **P1 CI baseline — Clippy warnings and cargo-deny failures**  
   [Issue #646](https://github.com/qhkm/zeptoclaw/issues/646) — Rust 1.97.1 produced five new Clippy warnings; `quick-xml 0.39.2` and `lopdf 0.40.0` were rejected as vulnerable.  
   **Status:** Closed.

No open crash-level regression was reported today. The P0/P1 items all have associated fixes that are now closed/merged.

## 6. Feature Requests & Roadmap Signals

There were no external feature-request issues in this window, but the open issue set forms a clear roadmap:

- [Issue #670 — Config source opacity](https://github.com/qhkm/zeptoclaw/issues/670): users need an “effective value came from here” view, schema-backed get/set, and clearer env-var deprecation.
- [Issue #669 — Persistent audit-chain segments](https://github.com/qhkm/zeptoclaw/issues/669): the current audit chain is tamper-evident only within one process lifetime.
- [Issue #666 — Durable cross-session memory](https://github.com/qhkm/zeptoclaw/issues/666): memory retrieval is intentionally bounded, but recall and writes need to survive restarts.
- [Issue #665 — Cron Job v2](https://github.com/qhkm/zeptoclaw/issues/665): completion acknowledgments, run ledgers, and operational control will make scheduled jobs observable.
- [Issue #664 — Delegated-agent capability inheritance](https://github.com/qhkm/zeptoclaw/issues/664): child agents must never exceed parent policy permissions.
- [Issue #667 — Footprint Ladder / extension metadata](https://github.com/qhkm/zeptoclaw/issues/667): reduce binary-size and compiler-time liability from the central tool registry.
- [Issue #668 — Hermetic seam-level integration tests](https://github.com/qhkm/zeptoclaw/issues/668): real-path, no-credential tests at subsystem seams.
- [Issue #663 — Finish Agent Pipeline migration](https://github.com/qhkm/zeptoclaw/issues/663): production still runs the legacy 5,227-line `AgentLoop`.
- [Issue #662 — Complete channel-plugin protocol](https://github.com/qhkm/zeptoclaw/issues/662): plugin channels are currently an outbound-only, fire-and-forget command sink.
- [Issue #661 — Byte-stable Prompt Envelope](https://github.com/qhkm/zeptoclaw/issues/661): rebuild volatile prompt content to improve prompt-cache economics.

The next release will likely include the just-closed security fixes. Among roadmap items, the smaller P2-high items most plausible for an early follow-up are config-source opacity ([#670](https://github.com/qhkm/zeptoclaw/issues/670)), durable memory behavior ([#666](https://github.com/qhkm/zeptoclaw/issues/666)), and Cron Job v2 ([#665](https://github.com/qhkm/zeptoclaw/issues/665)). The larger “L” items — pipeline migration [#663](https://github.com/qhkm/zeptoclaw/issues/663), channel protocol [#662](https://github.com/qhkm/zeptoclaw/issues/662), and prompt envelope [#661](https://github.com/qhkm/zeptoclaw/issues/661) — will likely require longer design and migration phases.

## 7. User Feedback Summary

No direct external user feedback or reactions were recorded in this data window. The active issues are maintainer-authored, mostly driven by the 2026-09-06 architecture review rather than inbound community bug reports. The implicit user pain points reflected in the issue text include:

- Lack of confidence that subprocesses cannot access unrelated credentials.
- Missing visibility into which configuration source produced an effective value.
- Audit evidence that does not survive restarts.
- Memory that is not durable across sessions.
- Channel and cron behaviors that are operationally incomplete.

Satisfaction cannot be directly measured from this dataset. The positive project-health signal is **speed of response**: all P0/P1 security items updated in this window were closed, and every critical issue has a corresponding fix PR.

## 8. Backlog Watch

The clearest backlog item is the group of Dependabot dependency PRs opened on **2026-06-03** and still open after three months:

- [qhkm/zeptoclaw PR #627 — serde_json 1.0.149 → 1.0.150](https://github.com/qhkm/zeptoclaw/pull/627)
- [qhkm/zeptoclaw PR #625 — rpassword 7.4.0 → 7.5.2](https://github.com/qhkm/zeptoclaw/pull/625)
- [qhkm/zeptoclaw PR #623 — tokio 1.52.1 → 1.52.3](https://github.com/qhkm/zeptoclaw/pull/623)
- [qhkm/zeptoclaw PR #620 — scraper 0.26.0 → 0.27.0](https://github.com/qhkm/zeptoclaw/pull/620)
- [qhkm/zeptoclaw PR #617 — tower-http 0.6.10 → 0.6.11](https://github.com/qhkm/zeptoclaw/pull/617)

These are low-to-moderate-risk dependency updates, but they have remained open for roughly three months. With the Clippy and cargo-deny baseline restored in [Issue #646](https://github.com/qhkm/zeptoclaw/issues/646), this is a good moment for maintainers to re-run CI on these PRs and either merge them or close them with explicit reasons. The ten open roadmap issues are active and tracked, so they do not currently appear abandoned or need maintainer attention as backlog.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw Project Digest — 2026-09-06

## 1. Today's Overview

ZeroClaw is in a high-velocity release and stabilization window. In the last 24 hours, 42 issues and 50 PRs were updated, with 8 issues and 6 PRs moving to closed/merged state. The project shipped **v0.8.5**, a substantial release spanning **454 commits from 73 contributors**, focused on connectivity, security hardening, and operator experience. Open activity remains dominated by architecture RFCs, security/sandbox work, and large feature PRs waiting on maintainer review or author follow-up. Overall project health looks strong, though the volume of high-risk open PRs suggests a growing maintainer-review bottleneck.

---

## 2. Releases

### v0.8.5

- **Source:** [ZeroClaw releases — v0.8.5](https://github.com/zeroclaw-labs/zeroclaw/releases/tag/v0.8.5)
- **Scope:** Security, connectivity, and operator-experience release.
- **Scale:** 454 commits, 73 contributors.
- **Highlights introduced:**
  - **ZeroRelay** and **ZeroRouter**
  - Expanded live chat and provider capabilities
  - Hardened plugin, sandbox, webhook, credential, and file boundaries
- **Breaking changes / migration notes:** No explicit breaking-change or migration details were included in the release-note excerpt provided. Since v0.8.5 includes router/relay architecture additions, deployments using gateway or provider settings should review the full release notes.

---

## 3. Project Progress

Several tracked items moved to closed/merged state in the last 24 hours. Among the visible PRs:

- [#5230 — feat(plugins): add WASM plugin system with security sandbox](https://github.com/zeroclaw-labs/zeroclaw/pull/5230)  
  A major feature PR closed after a long lifecycle, adding an extension mechanism for custom tools without forking the core codebase.
- [#10064 — fix(channels/telegram): self-destruct approval cards after an operator tap](https://github.com/zeroclaw-labs/zeroclaw/pull/10064)  
  Improves operator flow by resolving pending Telegram approvals and dismissing inline spinner UI after button taps.
- [#10435 — fix(providers): preserve model context when anchoring Gemini requests](https://github.com/zeroclaw-labs/zeroclaw/pull/10435)  
  Fixes provider request shaping for Gemini so model context is not lost during anchoring.
- [#10005 — fix(channels): base channel health on the channel, not on listener liveness](https://github.com/zeroclaw-labs/zeroclaw/pull/10005)  
  Corrects health reporting so a channel is not marked healthy merely because its listener started.
- [#10350 — ci(tests): measure affected Windows tests on pull requests](https://github.com/zeroclaw-labs/zeroclaw/pull/10350)  
  Adds advisory Windows test measurement to collect selection/duration/cache evidence; deliberately not yet part of the required CI gate.

Closed issues in the same window include:

- [#7911 — install.sh selects a generic Linux binary on Android/Termux](https://github.com/zeroclaw-labs/zeroclaw/issues/7911)
- [#7910 — add Windows runtime test coverage for self-update paths](https://github.com/zeroclaw-labs/zeroclaw/issues/7910)
- [#9593 — make TaskRecord the single lifecycle owner for background delegation](https://github.com/zeroclaw-labs/zeroclaw/issues/9593)
- [#10048 — validate Rust 1.98.0 local-CI, demo, and release/cross-platform lanes](https://github.com/zeroclaw-labs/zeroclaw/issues/10048)
- [#10045 — persisted image markers can retain temporary source paths](https://github.com/zeroclaw-labs/zeroclaw/issues/10045)
- [#10282 — hardware probe feature does not reach tool implementations](https://github.com/zeroclaw-labs/zeroclaw/issues/10282)

These closures show progress across platform installation, Windows updates, delegation lifecycle, CI tooling, media handling, and hardware probing.

---

## 4. Community Hot Topics

The most active discussion items in the last 24 hours are dominated by architecture RFCs with heavy revision and review cycles. No reaction-count data was available, so activity is ranked by comment count.

- [#9487 — RFC: Runtime-owned conversation sessions and transport surface adapters](https://github.com/zeroclaw-labs/zeroclaw/issues/9487) — **33 comments**  
  Revision 5 supersedes the Revision 4 vote snapshot; maintainers are expected to open a new discussion window before voting. High engagement suggests this is a pivotal runtime/session architecture decision.

- [#9488 — RFC: Unified file and attachment architecture for conversation surfaces](https://github.com/zeroclaw-labs/zeroclaw/issues/9488) — **26 comments**  
  A companion architecture RFC to #9487, currently at Revision 10. The repeated revisions indicate a complex, materially debated design.

- [#6808 — RFC: Work Lanes, Board Automation, and Label Cleanup](https://github.com/zeroclaw-labs/zeroclaw/issues/6808) — **24 comments**  
  Ratified governance tracker still in rollout; has been running since May. Community interest centers on routing maintainer work more efficiently.

- [#6996 — RFC: Granular sandbox policy — filesystem restrictions](https://github.com/zeroclaw-labs/zeroclaw/issues/6996) — **24 comments**  
  High-risk security RFC needing maintainer review. Aligns application path admission with OS sandbox backends.

- [#8692 — Tracker: Maintainer decision queue for RFCs and design issues](https://github.com/zeroclaw-labs/zeroclaw/issues/8692) — **15 comments**  
  It is the central issue-level queue for pending RFC/design decisions. Its continued visibility suggests the maintainer team is wrestling with volume.

- [#10050 — RFC: Verbatim channel send over the gateway, without an agent turn](https://github.com/zeroclaw-labs/zeroclaw/issues/10050) — **14 comments**  
  Requests a new gateway route for direct outbound channel delivery; reflects operator demand for finer-grained gateway control.

- [#7822 — RFC: WASM plugin lifecycle observer subscriptions](https://github.com/zeroclaw-labs/zeroclaw/issues/7822) — **13 comments**  
  Maintainer-takeover revision folded prior discussion into the reserved `PluginCapability::Observer`.

- [#9975 — RFC: define Web bundle/daemon compatibility for web_dist_dir](https://github.com/zeroclaw-labs/zeroclaw/issues/9975) — **13 comments**  
  Focuses on central capability negotiation for web dashboard deployments.

**Underlying needs:** The most active threads are not bug reports but attempts to settle the architectural direction before more features are built. The common thread is a desire for cleaner runtime ownership, unified session/file semantics, safer sandbox policies, and more structured governance.

---

## 5. Bugs & Stability

No critical S0-level crash was visible in the update set. The following active bugs were discussed or updated, ranked by severity:

### S1 — Workflow blocked

- [#10536 — macOS Seatbelt ignores configured allowed_roots for shell commands](https://github.com/zeroclaw-labs/zeroclaw/issues/10536)  
  Severity S1. Shell commands still receive `Operation not permitted` even when `allowed_roots` are configured. The app-level policy recognizes the roots, but the macOS sandbox backend does not. Marked `status:in-progress`, priority P1.

### S2 — Degraded behavior

- [#10625 — Internal `[media attachment]` placeholder is delivered to users when a non-vision model is in use](https://github.com/zeroclaw-labs/zeroclaw/issues/10625)  
  Text-only models cause the chat history to show a literal `[media attachment]` placeholder. This is an upstream degrade-path issue in channel/provider behavior.

- [#10626 — TTS synthesizes text verbatim: Markdown and emoji are spoken aloud](https://github.com/zeroclaw-labs/zeroclaw/issues/10626)  
  On some self-hosted deployments, spoken replies read raw markup and emoji names aloud. Needs a sanitization/filter stage before TTS synthesis.

- [#10532 — degraded-config remediation can invoke a different binary than the running daemon](https://github.com/zeroclaw-labs/zeroclaw/issues/10532)  
  If `zeroclaw` on `PATH` does not match the launched daemon, the degraded-config warning recommends running the wrong binary for `config migrate`.

- [#10533 — model_routing_config rejects custom.* provider slots — tool validation diverges from config schema](https://github.com/zeroclaw-labs/zeroclaw/issues/10533)  
  Priority P1; tool validation rejects valid dotted provider references such as `custom.truefoundry`. Already marked `status:in-progress`.

- [#10534 — bounded delegates silently strip the delegate tool, contradicting delegation_policy/max_delegation_depth config](https://github.com/zeroclaw-labs/zeroclaw/issues/10534)  
  Bounded-mode delegates always lose the `delegate` tool, regardless of policy configuration.

### S3 — Minor

- [#10585 — new log sink regression races migration tests under the default parallel runner](https://github.com/zeroclaw-labs/zeroclaw/issues/10585)  
  Tracing lock contention between new sink tests and migration tests; a test-isolation issue rather than end-user impact.

### Closed bug issues

- [#10045 — persisted image markers can retain temporary source paths](https://github.com/zeroclaw-labs/zeroclaw/issues/10045)
- [#10282 — hardware probe feature does not reach tool implementations](https://github.com/zeroclaw-labs/zeroclaw/issues/10282)
- [#7911 — install.sh selects generic Linux binary on Android/Termux](https://github.com/zeroclaw-labs/zeroclaw/issues/7911)

These were closed in the last 24 hours. No direct fix PR was visible in the top-PR list for all of them, so closure may also include triage/duplicate handling.

---

## 6. Feature Requests & Roadmap Signals

Active feature signals point toward **operator UX, security enrollment, provider compatibility, and cron/system surfaces**:

- [#10641 — Web: Per-field cron schedule input](https://github.com/zeroclaw-labs/zeroclaw/issues/10641)  
  New feature request opened 2026-09-05. Users want more than a raw cron text field in the Add/Edit Cron Job modal: per-field input, client-side validation, and human-readable confirmation are requested. This is a strong candidate for a future web-focused patch.

- [#10530 — Pass Anthropic extended-thinking params through OpenAI-compatible providers](https://github.com/zeroclaw-labs/zeroclaw/issues/10530)  
  Deployment need: users reaching Claude via LiteLLM/TrueFoundry/proxies with `wire_api = "chat_completions"` currently lose extended thinking. A gateway passthrough feature is likely if maintainers prioritize compatible provider parity.

- [#10549 — RFC: Simplify RFC voting by removing mandatory discussion windows](https://github.com/zeroclaw-labs/zeroclaw/issues/10549)  
  Process improvement request. Reflects community frustration with slow RFC cycles and material revisions replacing snapshots.

- [#10050 — RFC: Verbatim channel send over the gateway](https://github.com/zeroclaw-labs/zeroclaw/issues/10050)  
  Gateway route request for direct caller-supplied messages on configured channels.

- [#10339 — Tracker: Implement accepted shell V1 approval policy (#7155)](https://github.com/zeroclaw-labs/zeroclaw/issues/10339)  
  Implementation tracker for the accepted shell command-policy contract. This is a likely near-term feature once review bandwidth opens.

- [#10321 — feat(security): browser PKCE and cross-surface enrollment API](https://github.com/zeroclaw-labs/zeroclaw/pull/10321)  
  Large open PR that would add browser PKCE enrollment. It is part of a security-focused PR stack and is a strong roadmap signal for standalone-user authentication.

- [#9997 — feat(channels/telegram): add secure model picker](https://github.com/zeroclaw-labs/zeroclaw/pull/9997)  
  Would add a provider-grouped Telegram inline model picker for bare `/model`.

- [#10356 — feat(tools): add AnySearch web search provider](https://github.com/zeroclaw-labs/zeroclaw/pull/10356)  
  Blocked/do-not-merge, but signals ongoing demand for opt-in web search provider diversity.

**Predicted near-term next-version scope:** Because v0.8.5 has shipped, the next minor release is likely to combine stabilization follow-ups with accepted sandbox/delegation fixes and possibly the first RFC implementation batches from trackers such as #10339.

---

## 7. User Feedback Summary

Users and contributors consistently report friction around **configuration truth, sandbox behavior, channel/provider boundaries, and text/voice output quality**.

Key pain points visible in the last 24 hours:

- **macOS sandbox configuration mismatch:** Users configure `allowed_roots` and still get blocked shell behavior (#10536).
- **Approval and delegation surprises:** Bounded delegates silently lose their `delegate` tool even when policy allows delegation (#10534). Prior related work on supervised shell approval is still blocked/open in #10241.
- **Media handling confusion:** Non-vision models produce literal `[media attachment]` text (#10625), and persisted image markers may point at temp paths (#10045, now closed).
- **TTS quality:** Markdown and emoji are read aloud, creating a poor voice output experience (#10626).
- **Installation roulette on Android/Termux:** A closed issue (#7911) documents incorrect binary selection on non-standard Linux environments.
- **Cron UI is too raw:** The current raw text cron field is unwelcoming; users want validation and clearer scheduling UX (#10641).
- **Provider compat gaps:** OpenAI-compatible gateways to Anthropic models do not surface extended-thinking parameters (#10530), and `custom.*` provider slots are rejected by tool validation (#10533).

No explicit satisfaction surveys or user sentiment scores were in the data. However, the number of high-effort RFC revisions and the sheer volume of contributor-created PRs suggests an engaged, technically sophisticated community.

---

## 8. Backlog Watch

Several important items remain open for extended periods and may need maintainer attention:

### Long-running / high-comment RFCs and trackers

- [#6808 — RFC: Work Lanes, Board Automation, and Label Cleanup](https://github.com/zeroclaw-labs/zeroclaw/issues/6808)  
  Open since May, 24 comments, accepted/rollout tracker. Still needs execution follow-through.
- [#6996 — RFC: Granular sandbox policy — filesystem restrictions](https://github.com/zeroclaw-labs/zeroclaw/issues/6996)  
  Open since May, needs maintainer review, high risk.
- [#8692 — Tracker: Maintainer decision queue for RFCs and design issues](https://github.com/zeroclaw-labs/zeroclaw/issues/8692)  
  Open since July. It is itself the queue that needs processing.
- [#9487 and #9488](https://github.com/zeroclaw-labs/zeroclaw/issues/9487)  
  Open since late July, both with 25+ comments and high risk. Maintainers still need to record new discussion windows after the latest revisions.
- [#10076 — RFC: Composable WASM plugin runtime architecture](https://github.com/zeroclaw-labs/zeroclaw/issues/10076)  
  Needs maintainer review; important for aligning WASM plugin direction.
- [#10526 — RFC: Append-only session event history and deterministic replay](https://github.com/zeroclaw-labs/zeroclaw/issues/10526)  
  Recently referenced as the exclusive authority for session-history vocabulary; needs review attention.

### Old / stalled PRs needing author or maintainer action

- [#9320 — fix(cron): bound agent job runs with a wall-clock timeout that releases the lock](https://github.com/zeroclaw-labs/zeroclaw/pull/9320)  
  Open since July 23, needs author action, high risk, size XL. Cron timeouts and lock release are important stability work.
- [#8966 — feat(agent): carry live provider identity on usage events and resolve context window](https://github.com/zeroclaw-labs/zeroclaw/pull/8966)  
  Open since July 11, needs author action, high risk, size XL.
- [#10321 / #10275 / #10274 / #10270 / #10268 / #10265 / #10263 / #10259 / #10255 / #10248 — stacked security refactor](https://github.com/zeroclaw-labs/zeroclaw/pull/10321)  
  A very large, security-critical PR stack has been open since late August. It is marked `needs-maintainer-review` and `distinguished contributor`; the dependency chain makes it a review-heavy queue.
- [#10241 — fix(channels): restore supervised shell approval routing](https://github.com/zeroclaw-labs/zeroclaw/pull/10241)  
  Blocked, high risk; relevant to the S1/delegation stability concerns.
- [#9997 — Telegram secure model picker](https://github.com/zeroclaw-labs/zeroclaw/pull/9997) and [#10356 — AnySearch provider](https://github.com/zeroclaw-labs/zeroclaw/pull/10356)  
  Both remain blocked/do-not-merge.

The overall backlog is not one of neglected bug reports, but of **large architectural and security PRs** moving more slowly than the rate of new RFCs and feature suggestions. The v0.8.5 release should ease short-term release pressure and may allow maintainers to clear several of these high-risk items next.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*