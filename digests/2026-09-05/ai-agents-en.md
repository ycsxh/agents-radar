# OpenClaw Ecosystem Digest 2026-09-05

> Issues: 500 | PRs: 500 | Projects covered: 13 | Generated: 2026-09-05 03:59 UTC

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

# OpenClaw Project Digest — 2026-09-05

## 1. Today's Overview

OpenClaw is in a high-activity maintenance and hardening cycle: 500 issues and 500 PRs were updated in the last 24 hours (395 issues open / 105 closed; 348 PRs open / 152 merged or closed), with **no new release published** in the window. Issue volume is dominated by stability follow-ups around the Codex harness, gateway event-loop blocking, channel-specific message delivery, and memory/context management. Triage velocity is healthy — several long-running P0/P1 issues closed today — but a meaningful share of top issues remain parked in `clawsweeper:needs-maintainer-review` / `needs-product-decision` states, suggesting product decisions are the bottleneck more than engineering capacity. Maintainers are visibly active: a large cluster of new PRs today (many by maintainer `steipete`) targets gateway teardown, WebSocket handling, Control UI/macOS consolidation, and small perf fixes. Community-reported versions in issues indicate the current line is around **2026.9.1**, with users actively testing beta.7 and reporting regressions across 2026.7.x → 2026.9.x.

## 2. Releases

**No new releases in the last 24 hours.** No release notes, breaking changes, or migration notes to report. The project appears to be between release trains; PRs labeled next-version candidates (e.g., model-list unification, memory hardening) are still in review.

## 3. Project Progress

**152 PRs were merged/closed in the window.** The visible sample confirms one same-day close:

- [PR #138776 — fix(ci): stabilize Control UI stream and recovery tests](https://github.com/openclaw/openclaw/pull/138776) — [CLOSED] Browser test fixtures were flaky under timer clamping; merged same-day, a good sign for release-verification velocity.

Open PRs actively advancing fixes and features (all updated today):

**Stability / core gateway**
- [PR #138818 — fix(gateway): stop repeated teardown after WebSocket failures](https://github.com/openclaw/openclaw/pull/138818) — Stops cascading teardown/error spam per queued frame after a socket disconnect.
- [PR #130706 — fix: prevent Gateway stalls with multiple workspaces](https://github.com/openclaw/openclaw/pull/130706) — P1, removes repeated plugin discovery, bounds session-summary hydration, closes #130324.
- [PR #138830 — fix(transcripts): preserve capture identity across startup retries](https://github.com/openclaw/openclaw/pull/138830) — Prevents permanent "stuck" transcript auto-start after a failed provider startup.
- [PR #138832 — perf(plugins): avoid repeated installed record scans](https://github.com/openclaw/openclaw/pull/138832) — Removes quadratic scanning of the installed-plugin inventory.

**Memory subsystem**
- [PR #138798 — fix(memory): bound overlap carry so chunks stay within the char budget](https://github.com/openclaw/openclaw/pull/138798) — Fixes #138722; stops 400 context-length errors on files with very long lines.
- [PR #138391 — fix(memory): split embedding batches on provider row-cap errors](https://github.com/openclaw/openclaw/pull/138391) — Turns one oversized embedding request from a full rebuild abort into batch retries.

**Models & providers**
- [PR #136257 — feat(models): direct model lists and provider login across surfaces](https://github.com/openclaw/openclaw/pull/136257) — Headline cross-cutting feature: unifies Gateway `models.list`, CLI `models list`, chat `/models`, and Control UI catalog/auth state; P1, in maintainer review with telegram-e2e proof.
- [PR #127992 — fix(openai): restore cache-TTL context pruning](https://github.com/openclaw/openclaw/pull/127992) — Closes #95840; reintroduces idle-gap tool-result pruning for direct OpenAI models.
- [PR #125861 — feat(web-search): keyless Tavily capability](https://github.com/openclaw/openclaw/pull/125861) — Lowers onboarding friction when no API key is set.

**Channels & platforms**
- [PR #117174 — fix(whatsapp): render self LID mentions as agent identity](https://github.com/openclaw/openclaw/pull/117174)
- [PR #125718 — fix(matrix): bound poll relation pagination](https://github.com/openclaw/openclaw/pull/125718) — Memory/exhaustion guard against faulty/malicious homeservers.
- [PR #103928 — fix(feishu): prevent long streaming replies from draining stale updates](https://github.com/openclaw/openclaw/pull/103928) — Closes #91941.
- [PR #138808 — fix(signal): keep prototype-named container styles unstyled](https://github.com/openclaw/openclaw/pull/138808) — Fixes a `constructor` prototype-lookup bug wrapping text in native Function source.

**Agent-core / tools**
- [PR #115933 — fix: agent turn dies silently while waiting for an exec approval only someone else can grant](https://github.com/openclaw/openclaw/pull/115933) — P1; routes approval decisions by real sender, not channel type.
- [PR #122846 — agent-core: add per-response tool-call block cap (`maxCallsPerBlock`)](https://github.com/openclaw/openclaw/pull/122846) — Protects CLI loopback correlation from >4–5 tool-call bursts returning `outcome: "unknown"`.
- [PR #138820 — fix(codex): reject stale control commands after session rollover](https://github.com/openclaw/openclaw/pull/138820)

**Apps / UI / DX**
- [PR #138092 — refactor(macos): move app settings into the Dashboard, keep Connection native](https://github.com/openclaw/openclaw/pull/138092) — Completes the "one UI owner for settings" doctrine.
- [PR #138666 — fix(control-ui): measure context usage against the effective token budget](https://github.com/openclaw/openclaw/pull/138666) — Closes #138590; fixes misleading context meter.
- [PR #138826 — fix(wizard): preserve typed enum values in plugin config](https://github.com/openclaw/openclaw/pull/138826)
- [PR #138757 — fix(ui): acknowledge successful remote config open](https://github.com/openclaw/openclaw/pull/138757)

## 4. Community Hot Topics

Most-commented issues in the window (links to active discussion):

- [Issue #91009 — Codex PreToolUse native hook relay spawns CPU-bound `openclaw-hooks` processes and stalls gateway RPC](https://github.com/openclaw/openclaw/issues/91009) — **21 comments, P0, open ~3 months.** The single most-discussed item. Users report 100%+ CPU per hook process and gateway RPC stalls on the Codex integration; still waiting on maintainer review/product decision plus a live repro. Underlying need: a definitive fix or supported workaround for Codex hook-relay CPU spinning.
- [Issue #48003 — Steer mode does not inject messages mid-turn for main sessions](https://github.com/openclaw/openclaw/issues/48003) — **20 comments, P1, 👍4.** Root cause narrowed to a `KeyedAsyncQueue` commit; linked PR is open, but a product decision is pending. Underlying need: interactive real-time steering of agent turns, not queued-until-done behavior.
- [Issue #104721 — All tool results return "(see attached image)" literal string instead of actual output](https://github.com/openclaw/openclaw/issues/104721) — **17 comments, P0, [CLOSED] today.** A scary regression where file-read/tool data was replaced by a placeholder string; closure in-window will be welcomed by users.
- [Issue #87307 — Matrix thread replies sent as normal replies on 2026.5.22; /status and /model silent](https://github.com/openclaw/openclaw/issues/87307) — **15 comments, P1, [CLOSED].** Channel parity + silent-command failure.
- [Issue #115908 — Session transcript projection reconcile can livelock under sustained writes, blocking the main thread](https://github.com/openclaw/openclaw/issues/115908) — **15 comments, P1, open.** Synchronous rebuild cycle stalls the event loop for tens of seconds, killing all transports.
- [Issue #53628 — `${XDG_CONFIG_HOME}` not processed when installing a skill](https://github.com/openclaw/openclaw/issues/53628) — **14 comments, P2.** Docker + env-var expansion friction in ClawHub skill installs.
- [Issue #43367 — Multi-agent orchestration is unstable: concurrent config overwrites, session-lock failures, detached child work](https://github.com/openclaw/openclaw/issues/43367) — **14 comments, P1, open since March.**
- [Issue #108435 — Update to 2026.7.1: gateway fails to start with systemd/Ollama/manual launch](https://github.com/openclaw/openclaw/issues/108435) — **14 comments, P0, 👍3.**

## 5. Bugs & Stability

Ranked by severity, with fix-PR status where known:

**P0**
- [Issue #91009 — Codex hook relay spawns CPU-bound processes; gateway RPC stalls](https://github.com/openclaw/openclaw/issues/91009) — **OPEN, ~3 months.** No new fix PR; blocked on maintainer/product decision + live repro.
- [Issue #108435 — Gateway fails to start on 2026.7.1 ("gateway did not start on 127.0…")](https://github.com/openclaw/openclaw/issues/108435) — **OPEN.** Regression across systemd/Ollama/manual launch; needs-info state.
- [Issue #70903 — Persistent file-based provider cooldown blocks users for hours after billing recovery](https://github.com/openclaw/openclaw/issues/70903) — **OPEN, stale, since April.** `disabledUntil` timestamps persist across restarts and keep extending; no fix PR.
- [Issue #104721 — Tool results replaced by literal "(see attached image)"](https://github.com/openclaw/openclaw/issues/104721) — **CLOSED in-window** (message-loss / release-blocker impact).

**P1 highlights**
- [Issue #115908 — Transcript projection reconcile livelock blocks the main thread](https://github.com/openclaw/openclaw/issues/115908) — **OPEN**, no linked fix PR yet.
- [Issue #43367 — Multi-agent orchestration: concurrent config writes, session-lock failures](https://github.com/openclaw/openclaw/issues/43367) — **OPEN since March**; linked PR open + needs-info.
- [Issue #97616 — Unreaped hook/tool child processes cause zombie accumulation and runtime degradation](https://github.com/openclaw/openclaw/issues/97616) — **OPEN**; related root-cause family to #91009.
- [Issue #112259 — Visible inbound channel turn silently dropped with no retry/dead-letter](https://github.com/openclaw/openclaw/issues/112259) — **OPEN**.
- [Issue #48003 — Steer mode doesn't inject mid-turn](https://github.com/openclaw/openclaw/issues/48003) — **OPEN**; linked PR open.
- [Issue #131150 — Slack DMs silently dropped after gateway restart (19-account socket mode)](https://github.com/openclaw/openclaw/issues/131150) — **OPEN**.
- [Issue #119720 — Synchronous agent persistence/transcript maintenance block the gateway event loop at scale](https://github.com/openclaw/openclaw/issues/119720) — **OPEN**.
- [Issue #86215 — Codex OAuth refresh failures wedge agents for hours](https://github.com/openclaw/openclaw/issues/86215) — **CLOSED in-window.**
- [Issue #107449 — cron tool JSON Schema incompatible with llama.cpp parser (`pattern: "\S"`)](https://github.com/openclaw/openclaw/issues/107449) — **CLOSED in-window**, 👍4.

**Confirmed issue→fix-PR pairs in flight:**
- #95840 (OpenAI cache-TTL pruning never fires) → [PR #127992](https://github.com/openclaw/openclaw/pull/127992)
- #138722 (memory chunk overflow) → [PR #138798](https://github.com/openclaw/openclaw/pull/138798)
- #138590 (Control UI context meter wrong budget) → [PR #138666](https://github.com/openclaw/openclaw/pull/138666)
- #91941 (Feishu streaming drains stale updates) → [PR #103928](https://github.com/openclaw/openclaw/pull/103928)
- #130324 (gateway stalls) → [PR #130706](https://github.com/openclaw/openclaw/pull/130706)

**Cluster analysis:** Three bug families dominate severity: **(a) Codex/hook process lifecycle** (#91009, #97616, plus closed #86215/#107814/#84393); **(b) silent message loss / wrong delivery** (#112259, #131150, #90944, #135704, closed #69008/#87212); **(c) synchronous main-thread work stalling the gateway** (#115908, #119720, #114234).

## 6. Feature Requests & Roadmap Signals

Notable user-requested features in the backlog:

- [Issue #28300 — Theme Customization System: preset themes + custom theme studio](https://github.com/openclaw/openclaw/issues/28300) — **👍5, highest-reacted feature** (P3): Control UI polish demand is real.
- [Issue #53763 — Built-in headless browser for reliable web access](https://github.com/openclaw/openclaw/issues/53763) — 12 comments, P3; may stay parked since the project is investing in the existing browser extension instead (see [PR #135648](https://github.com/openclaw/openclaw/pull/135648)).
- [Issue #16670 — Onboarding Wizard should make Memory/Embedding setup mandatory](https://github.com/openclaw/openclaw/issues/16670) — recurring "memory is the killer feature nobody gets configured" complaint.
- [Issue #6757 — Agent-triggered context compaction (self-compact tool)](https://github.com/openclaw/openclaw/issues/6757) — filed autonomously by an OpenClaw agent ("Wyatt"); interesting dogfooding signal.
- [Issue #51441 — Expose resolved backend model in `session_status`/agent runtime](https://github.com/openclaw/openclaw/issues/51441) — important for LiteLLM/proxy users.
- [Issue #45501 — `session.resetPrompt`: configurable session startup message](https://github.com/openclaw/openclaw/issues/45501)
- [Issue #55249 — Session labels/nicknames for easier identification](https://github.com/openclaw/openclaw/issues/55249)
- [Issue #42276 — Reasoning stream with overwrite-able progress lines](https://github.com/openclaw/openclaw/issues/42276)
- [Issue #87362 — Emit task-flow lifecycle hook events for plugin observability](https://github.com/openclaw/openclaw/issues/87362)
- [Issue #41366 — Durable natural-language rule learning + multi-mention reply semantics](https://github.com/openclaw/openclaw/issues/41366)
- [Issue #88032 — Telegram quote/reply context as a first-class durable inbound contract](https://github.com/openclaw/openclaw/issues/88032)

**Next-version prediction:** The strongest near-term candidates are items already in maintainer review with proof attached: the cross-surface model-list/provider-login overhaul ([PR #136257](https://github.com/openclaw/openclaw/pull/136257)), gateway/WebSocket teardown fixes ([PR #138818](https://github.com/openclaw/openclaw/pull/138818)), memory-index robustness ([PR #138798](https://github.com/openclaw/openclaw/pull/138798), [PR #138391](https://github.com/openclaw/openclaw/pull/138391)), and OpenAI cache-TTL pruning restoration ([PR #127992](https://github.com/openclaw/openclaw/pull/127992)). User-facing P3 backlog items (themes, session labels, headless browser) are unlikely to ship in the next minor release.

## 7. User Feedback Summary

Real pain points expressed across the last 24 hours:

- **Silent message loss is the most emotionally charged complaint.** Users repeatedly describe DMs/replies "silently dropped," "never delivered," "no error," or replies going to the wrong session — e.g., [Slack DMs after restart](https://github.com/openclaw/openclaw/issues/131150), [zero-payload dispatch](https://github.com/openclaw/openclaw/issues/112259), [iMessage echo-cache bypass](https://github.com/openclaw/openclaw/issues/135704), and [stale subagent completion delivered into replaced lifecycle](https://github.com/openclaw/openclaw/issues/118018). Users want retries, dead-lettering, and user-visible failures rather than quiet drops.
- **Codex-integration fatigue:** CPU spins, zombie processes, OAuth wedging, and silent coding-base-prompt injection into operational agents (see [#91009](https://github.com/openclaw/openclaw/issues/91009), [#97616](https://github.com/openclaw/openclaw/issues/97616), [#86215](https://github.com/openclaw/openclaw/issues/86215), [#84393](https://github.com/openclaw/openclaw/issues/84393)) make the flagship Codex harness feel unreliable for production.
- **Multi-agent orchestration trust deficit:** concurrent `agents add` overwrites, session-lock failures, and detached child work ([#43367](https://github.com/openclaw/openclaw/issues/43367)) block team workflows; a 6-month-old P1 with a pending linked PR is frustrating.
- **Billing/provider friction:** users who top up credit remain blocked by a persisted provider cooldown ([#70903](https://github.com/openclaw/openclaw/issues/70903)); OAuth inheritance breaks for child agents ([#98702](https://github.com/openclaw/openclaw/issues/98702)).
- **Configuration surprises:** `${XDG_CONFIG_HOME}` unexpanded during skill install ([#53628](https://github.com/openclaw/openclaw/issues/53628)); memory not configured because onboarding omits it ([#16670](https://github.com/openclaw/openclaw/issues/16670)); cron JSON schema breaking llama.cpp ([#107449](https://github.com/openclaw/openclaw/issues/107449)).
- **Satisfaction signals:** users are engaging deeply (repros, logs, bisection across versions such as #138272), and several high-severity regressions were closed within the day — response to the P0 "(see attached image)" placeholder issue and Matrix regression was comparatively fast. UI demands (theme system, 👍5 on #28300) show a community that cares about the Control UI as a product surface.

## 8. Backlog Watch

Long-running, important items needing maintainer attention:

| Issue | Age / Status | Why it matters |
|---|---|---|
| [#70903 — Persistent provider cooldown after billing recovery](https://github.com/openclaw/openclaw/issues/70903) | **P0, open since Apr 24, stale** | Users blocked for hours after paying; no fix PR, needs product decision. |
| [#91009 — Codex hook relay CPU-bound processes](https://github.com/openclaw/openclaw/issues/91009) | **P0, open since Jun 6, 21 comments** | Highest-discussed issue; `no-new-fix-pr`, `needs-maintainer-review`, `needs-product-decision`, `needs-live-repro`. |
| [#108435 — Gateway fails to start on 2026.7.1](https://github.com/openclaw/openclaw/issues/108435) | **P0, open since Jul 15** | Blocks upgrades for affected users; parked in `needs-info`. |
| [#43367 — Multi-agent orchestration instability](https://github.com/openclaw/openclaw/issues/43367) | **P1, open since Mar 11** | 6-month-old concurrency/data-loss cluster with linked PR still open. |
| [#48003 — Steer mode mid-turn injection](https://github.com/openclaw/openclaw/issues/48003) | **P1, open since Mar 16, 20 comments** | Needs product decision despite clear root cause + fix shape. |
| [#119720 — Synchronous persistence blocks gateway event loop](https://github.com/openclaw/openclaw/issues/119720) | **P1, open since Aug 5** | Scale-relevant architecture debt; partially fixed by #133925/#134062. |
| [#115908 — Transcript projection livelock](https://github.com/openclaw/openclaw/issues/115908) | **P1, open since Jul 29** | Event-loop stall with no linked PR in view. |
| [#112259 — Silent inbound message drop](https://github.com/openclaw/openclaw/issues/112259) | **P1, open since Jul 21** | Missing retry/dead-letter/user-visible failure — trust issue. |
| [#71689 — Tasks registry restore fails on malformed SQLite](https://github.com/openclaw/openclaw/issues/71689) | **P1, open since Apr 25** | Startup-blocking corruption path; no new fix PR. |
| [#6757 — Agent-triggered context compaction](https://github.com/openclaw/openclaw/issues/6757) | **P2/P3, open since Feb 2** | Agent-filed request; still awaiting maintainer product decision. |

**Overall health assessment:** OpenClaw shows strong engineering throughput — a 152-PR merged/closed day, fast CI-fix turnaround, and maintainer-led refactors across gateway, memory, and macOS/Control UI. The main risk to project health is not code velocity but **decision latency**: several P0/P1 items have clear root causes and even linked PRs, yet remain parked behind `needs-product-decision` / `needs-maintainer-review`, while users continue to hit Codex-hook CPU spins, silent message drops, and event-loop stalls in production.

---

## Cross-Ecosystem Comparison

# Cross-Project Ecosystem Comparison — 2026-09-05

*Scope: 13 projects from the personal AI assistant / agent open-source ecosystem. All data from 2026-09-05 community digest summaries.*

---

## 1. Ecosystem Overview

The open-source personal AI assistant landscape is consolidating into three broad categories: gateway-first omnichannel agent cores (OpenClaw, ZeroClaw, IronClaw, CoPaw), lightweight chat/WebUI front ends (NanoBot, PicoClaw, NanoClaw, Moltis), and desktop/team-oriented agent products (Hermes Agent, LobsterAI). Across all categories, the engineering emphasis has shifted sharply from model wiring to production reliability — event-loop stalls, silent message drops, process lifecycle bugs, and context/memory management dominate issue queues. A second common thread is provider-contract churn: several projects independently hit the same OpenCode session-header deadline and OpenAI/Anthropic prompt-cache semantic changes on the same day. The ecosystem is also bifurcating by deployment philosophy, with one cluster optimizing for local-first/self-hosted control and another for multi-user, multi-agent fleet and team governance.

---

## 2. Activity Comparison

*Issues/PRs = items updated in the 24-hour window. Health score is editorial (1–10), weighing release cadence, fix throughput, maintainer responsiveness, and unresolved severity.*

| Project | Issues updated | PRs updated | Merged/closed | Releases | Health |
|---|---|---|---|---|---|
| **OpenClaw** | 500 | 500 | 152 PRs / 105 issues | None (users on 2026.9.1; beta.7 in field) | 7.5 |
| **LobsterAI** | 1 | 33 | 28 PRs / 0 issues | **2** — 2026.9.4, 2026.9.3 | 8.5 |
| **IronClaw** | 3 | 12 | 3 PRs / 2 issues | None | 8.0 |
| **NanoBot** | 5 | 28 | 7 PRs / 3 issues | None | 7.5 |
| **ZeroClaw** | 34 | 50 | 6 PRs / 10 issues | None (v0.8.5 bump open) | 7.5 |
| **CoPaw** | 23 | 26 | ~6 PRs / 8 issues | None (2.2.0 Hub in beta churn) | 7.0 |
| **Hermes Agent** | 50 | 50 | 3 PRs / 3 issues | None (v0.21.0 referenced) | 6.5 |
| **NanoClaw** | 2 | 18 | 3 PRs / 0 issues | None | 5.0 |
| **PicoClaw** | 3 | 22 | 20 PR closures — mostly `[stale]` | None | 4.0 |
| **Moltis** | 1 | 1 | 0 | None | 4.0 |
| **NullClaw** | 1 | 0 | 0 | None | 3.5 |
| **TinyClaw** | 0 | 0 | 0 | None | 2.0 |
| **ZeptoClaw** | 0 | 0 | 0 | None | 2.0 |

Notable: **OpenClaw merged/closed 152 PRs in a single day — more than the total PR activity of any other project in the window.** LobsterAI and IronClaw show the healthiest balance of throughput and responsiveness; PicoClaw and NanoClaw carry unresolved high-impact defects despite active or recent PR churn.

---

## 3. OpenClaw's Position

**Advantages:**

- **Scale and velocity.** 500 issues/500 PRs updated per day; 152 PRs merged/closed; 105 issues closed. No peer is within an order of magnitude. Issue triage velocity is healthy, with multiple P0/P1 closures in-window.
- **Breadth of channel and product surface.** WhatsApp, Matrix, Feishu, Signal, Slack, Telegram, iMessage, plus Control UI, macOS app, CLI, gateway, plugin (ClawHub), and Codex-harness integration. This is the widest integrated surface in the ecosystem.
- **Vertical productization.** OpenClaw is the only project with a first-class memory subsystem (chunked embeddings, batch caps, transcript projections), a unified model/provider catalog across surfaces, and a "one UI owner" consolidation doctrine — signs of a maturing platform rather than a single-surface tool.
- **Maintainer-driven refactoring.** Same-day CI-fix turnaround and large maintainer-authored PR clusters (gateway teardown, WebSocket handling, macOS/Control UI) indicate strong core-team capacity.

**Technical approach differences:**

- Multi-workspace, gateway-centric async event loop, distinct from Hermes' Desktop-bound profile pool, NanoClaw's per-session container model, or ZeroClaw/IronClaw's Rust CLI/TUI-first architecture.
- Positioned as the "core reference" monorepo — the project others fork, port, or pattern-match against (visible in the "Claw" naming lineage across PicoClaw, NanoClaw, TinyClaw, ZeptoClaw, IronClaw).

**Community size comparison:**

- OpenClaw's single-day PR merge count (152) exceeds the *total PR activity* of all but the largest peers; its issue volume is roughly 10× the sum of several peers combined.
- Its top thread has only 21 comments vs. Hermes' 157 and CoPaw's 22 — engagement is broad and issue-distributed rather than concentrated in mega-threads, consistent with a large, diversified user base.
- **Weakness:** decision latency. Several P0/P1 items sit behind `needs-product-decision`/`needs-maintainer-review` with clear root causes and even open PRs, making governance the constraint rather than engineering capacity.

---

## 4. Shared Technical Focus Areas

**Event-loop and process-lifecycle stability** — OpenClaw (#115908, #119720, #91009 Codex CPU spin, #97616 zombie children), Hermes (#103375 pool-starved bot tiles), CoPaw (#7534 Feishu consumer permanently stuck). *Need: eliminate sync main-thread work; reap child processes; supervise hook/approval workloads outside the hot path.*

**Memory/context as an archival system** — OpenClaw (chunk overlap bounds #138722, embedding batch caps #138391); NanoClaw (OOM from unbounded full PreCompact rewrites #3716); NanoBot (`/compact`, `compaction_id`, lifecycle events #5656); ZeroClaw (history-trim token accounting #9713); Hermes (spurious-compaction defer baseline #103397); Moltis (persistent reasoning-effort defaults #1259). *Need: bounded, incremental, auditable compaction with rotation/caps/IDs, not full rewrites.*

**Provider session and prompt-cache contract churn** — The **same `x-opencode-session` header deadline was independently filed against NanoBot (#5661) and ZeroClaw (#10603)** on the same day. Concurrently: OpenClaw restores OpenAI cache-TTL pruning (#127992); IronClaw sends stable conversation cache keys on OpenAI paths (#8062); ZeroClaw requests Anthropic `cache_control` passthrough (#10619); CoPaw hits Volcengine Ark partial-turn rejection (#7549); PicoClaw's provider-compat PRs (xAI #2260, Copilot stdio #2240, strict-mode #1683) were closed stale. *Need: versioned session headers, idempotent cache keys, and transparent cache-control passthrough as first-class provider contracts.*

**Multi-agent and fleet reliability** — OpenClaw (#43367 orchestration instability), IronClaw (stranded subagent delivery sweep #8067, concurrent-children cap #8061), Hermes (group chats outliving Desktop #97681), CoPaw (9-agent fleet operator #7557/#7558), NanoClaw (A2A identity preservation #3718, failure reporting #3719). *Need: subagent delivery guarantees, identity/command-boundary preservation, and team-level governance.*

**Channel-connect and identity semantics** — ZeroClaw (empty WhatsApp `allowed_groups` = permit-none #9397), IronClaw (pairing vs. connectivity copy #8074/#8054/#8073), OpenClaw (WhatsApp self-LID mentions #117174), PicoClaw (Slack race #2089, Feishu mention detection #2091), NanoBot (Feishu one-streaming-card-per-conversation #5567). *Need: clear, context-correct channel state; one logical message per user turn.*

**Default-deny security posture** — ZeroClaw (WhatsApp security closure #9348/#9397), PicoClaw (empty-`allow_from` hardening #2088, exec preflight #2298), NanoClaw (mount-bypass fix #3680), OpenClaw (config/trust issues), CoPaw (MCP whitelist enforcement #7504). *Need: least-privilege defaults, administratively explicit allows, and clear blame-free error copy.*

**Skills/plugins and MCP onboarding** — OpenClaw (ClawHub, XDG expansion #53628); CoPaw (workspace-scoped Skill preload #7183); Hermes (skills-index watchdog #66616); LobsterAI (per-Skill npm audit caps #2616); NanoClaw (opt-in/operator-policy skill installs #3720/#3721); PicoClaw (community MCP setup guides #3368/#3367). *Need: manageable installation, explicit policy, and discoverable configuration.*

---

## 5. Differentiation Analysis

| Project | Architecture | Distinctive strategy | Target user |
|---|---|---|---|
| **OpenClaw** | TypeScript monorepo, multi-workspace gateway | Broadest channel integration + memory subsystem + unified UI surface; de-facto reference platform | Power users running always-on personal agents across all messaging surfaces |
| **Hermes Agent** | Desktop client + local profile pool + SSH remote | Desktop-first with group-chat bot-mode that outlives the app | Desktop power users with multi-profile/fleet setups |
| **ZeroClaw** | Rust workspace (23 crates), crates.io distribution | Release-grade engineering, RFC-governed, TUI (ZeroCode), strict security semantics | DevOps-leaning users who want a compiled, auditable agent |
| **CoPaw** | Python/Qwen ecosystem | Multi-tenant Hub (2.2.0), Chinese-language community, LAN/local models, Creator media plugin, batch/cost scheduling | Chinese-speaking teams and creators running agent fleets |
| **LobsterAI** | Electron desktop | Polish-first consumer desktop assistant with in-app browser, cowork features, subscription/publishing lifecycle | Mainstream/consumer desktop users |
| **IronClaw** | Rust agent loop + gateway | Social-channel-first, subagent reliability program (R3/R4) | Telegram/social bot operators |
| **NanoBot** | Go, lightweight | Fast feature delivery, WebUI/TUI observability (speed/context meters), provider breadth | Hobbyist-to-SMB personal assistant users |
| **PicoClaw** | Go CLI | Broad provider compatibility; MCP documentation momentum | CLI users self-hosting across many LLM backends |
| **NanoClaw** | Session-container runtime | Provider-contract consolidation (Codex/OpenCode/Cursor) with strict session boundaries | Developers standardizing on coding-agent CLIs |
| **NullClaw** | Zig, minimal | Minimalism and self-hosted control | Niche self-hosters |
| **Moltis** | External-agent CLI streaming | Interop with `agy` CLI/OAuth workflows | Users standardizing on non-OpenAI agent CLIs |
| **TinyClaw / ZeptoClaw** | Dormant | — | — |

Architectural split worth noting: **monorepo gateway** (OpenClaw) vs. **Desktop-supervised profiles** (Hermes) vs. **container-per-session runtime** (NanoClaw) vs. **compiled Rust core** (ZeroClaw/IronClaw). CoPaw is the clearest counterweight to the English-centric ecosystem: a large Chinese-speaking community with enterprise/team feature demands (PostgreSQL backend, fleet management, off-peak batch scheduling).

---

## 6. Community Momentum & Maturity

**Tier A — Fast, healthy iteration**
- **LobsterAI** — shipped two releases in 24 hours; 28 PRs merged; clearly the best release cadence in the window.
- **IronClaw** — low-risk, experienced-contributor PRs landing promptly; focused on a coherent Telegram-copy + subagent-reliability program.
- **NanoBot** — short feature-to-merge cycles (WebUI speed display filed and closed same window); proactive cache-bounding work.

**Tier B — High velocity with accumulating risk**
- **OpenClaw** — exceptional throughput, but P0 backlog items (Codex hook CPU spin, cooldown persistence) remain parked for months behind product decisions.
- **CoPaw** — strong community engagement and feature demand, but "fake stop" task-state confusion (#7567), channel deadlocks (#7534), and LAN stream failures (#7505) remain open without fix PRs.
- **ZeroClaw** — large active PR/issue surface and imminent v0.8.5, yet many `risk:high`/`do-not-merge` PRs are blocked awaiting maintainer bandwidth.

**Tier C — Stabilizing / stretched**
- **Hermes Agent** — substantial bot/group-chat feature train, but a state.db corruption P1 (#103339), SSH 401 cluster, and Desktop display bugs signal an over-extended pipeline.

**Tier D — Maintainer-constrained**
- **PicoClaw** — most at risk: a batch cleanup closed ~13 substantive provider/channel fix PRs as `[stale]`, including a severe MCP-hang fix (#3337) and security hardening (#2298). Contributor satisfaction is visibly at stake.
- **NanoClaw** — active refactor but a production OOM crash loop (#3716) is unresolved; provider-contract PRs are aging.
- **Moltis / NullClaw** — single-thread maintenance; awaiting triage on their only open items.

**Tier E — Dormant**
- **TinyClaw, ZeptoClaw** — no observable activity.

Maturity signal: ZeroClaw and CoPaw are developing formal RFC and stabilization-tracker processes (#10330, #9459, #9487), indicating projects reaching governance complexity. OpenClaw's `needs-product-decision` bottleneck suggests the same stage, but without a formalized decision mechanism yet.

---

## 7. Trend Signals

**1. "Fail loud" is the new reliability bar.** Silent message drops, cron jobs that never run (#10593/#10594 ZeroClaw), self-stopping tasks without notice (#6921 CoPaw), and quiet subagent failures dominate user complaints across OpenClaw, Hermes, CoPaw, and ZeroClaw. Users are explicitly demanding retries, dead-lettering, run history, and user-visible errors. → *Value for developers: build delivery receipts and failure records into agent loops and channel adapters by default.*

**2. Provider session/cache contracts are now a hard dependency risk.** Two projects independently hit the same OpenCode `x-opencode-session` deadline. Long-lived agent loops increasingly depend on provider-side prompt caching for cost viability (OpenClaw, IronClaw, ZeroClaw are all actively working cache semantics). → *Value: treat provider session IDs and cache keys as first-class state, persisted across turns and restarts, and abstracted behind a versioned provider contract.*

**3. Context compaction is becoming a log-structured system.** OpenClaw is bounding chunk overlap; NanoClaw hit an OOM from unbounded PreCompact rewrites; NanoBot is adding `compaction_id` lifecycle events and auditability; ZeroClaw is accounting tokens on history-trim; Hermes is suppressing spurious compaction. → *Value: implement compaction as an append/rotate operation with IDs, caps, and observability — not a full serialization rewrite.*

**4. Local-first and keyless operation is a growing adoption lever.** Keyless Tavily (OpenClaw), no-API-key MCP guides (PicoClaw), self-hosted Firecrawl (NullClaw), LAN model servers (CoPaw), and Hailo-Ollama edge acceleration (ZeroClaw) all point the same direction: users want agents that work with local or self-hosted infrastructure and no mandatory SaaS keys. → *Value: abstract every external dependency behind configurable endpoints and optional-key paths.*

**5. Agents are becoming fleet infrastructure, not single-user toys.** CoPaw operators run nine agents and want relational storage and off-peak batch scheduling; Hermes users run 20 profiles; OpenClaw has multi-agent orchestration as a long-running P1; IronClaw is building subagent delivery sweeps; CoPaw's 2.2.0 Hub is explicitly multi-tenant. → *Value: design for supervised concurrency — child-process caps, session locking, stranded-work recovery, and governance from day one.*

**6. Security is moving from optional hardening to default-deny policy.** ZeroClaw's empty-`allowed_groups` = permit-none semantics, PicoClaw's empty-`allow_from` audit, NanoClaw's mount-allowlist enforcement, and CoPaw's MCP whitelist enforcement all show a converging default: if not explicitly allowed, it is denied. → *Value: invert access-control defaults and make administrator configuration gaps visible with clear error copy.*

**7. Desktop apps are becoming supervisors, not runtimes.** Hermes' group-chat continuity work (#97681), LobsterAI's release cadence, and OpenClaw's macOS/Control UI consolidation all separate the agent process from the UI process. Users increasingly treat bots as gateway daemons that must survive app closure, network drops, and machine reboots. → *Value: decouple agent lifecycle from UI lifecycle and test the headless/background path as a first-class deployment mode.*

---

## Peer Project Reports

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

## NanoBot Project Digest — 2026-09-05

### 1. Today’s Overview

NanoBot is in a high-activity maintenance and feature-development phase. As of 2026-09-05, 28 PRs were updated in the last 24 hours, with 7 merged/closed and 21 still open. Issue churn was also positive: 3 of the 5 recently touched issues were closed. The most prominent workstream is stability hardening — PRs landed or advanced today around TUI/WebUI fixes, WebUI token/speed display, and bounding in-memory caches. No new releases were published during this window.

### 2. Releases

No new releases in the last 24 hours.

### 3. Project Progress

Visible closed/merged PRs from the last 24 hours:

- **#5639** — [fix: stabilize session labels, TUI streaming, and pairing prompts](https://github.com/HKUDS/nanobot/pull/5639)  
  Improves TUI rendering, particularly streamed code fences, and session label behavior.

- **#5660** — [feat(webui): show model generation speed in context usage popover](https://github.com/HKUDS/nanobot/pull/5660)  
  Closes the WebUI feature request [#5631](https://github.com/HKUDS/nanobot/issues/5631) by adding tokens-per-second speed display alongside existing context/token info.

- **#5657** — [refactor(webui): extract outbound wire encoding](https://github.com/HKUDS/nanobot/pull/5657)  
  Refactors WebSocket encoding into a shared `send_payload` primitive and tightens turn persistence behavior.

Also closed this window were issues [#5644](https://github.com/HKUDS/nanobot/issues/5644) and [#5645](https://github.com/HKUDS/nanobot/issues/5645), plus feature request [#5631](https://github.com/HKUDS/nanobot/issues/5631).

### 4. Community Hot Topics

The most discussed item currently is:

- **#5567** — [Feishu channel should integrate multi-turn replies into a single streaming card message](https://github.com/HKUDS/nanobot/issues/5567)  
  **4 comments** | Open  
  Users report that a single user request can produce many separate Feishu messages: progress messages, tool calls, and the final reply. The underlying need is clear: channel UX should preserve a one-user-message → one-agent-message mental model, especially on chat platforms with visible message separation.

Other notable but lower-discussion topics:

- **#5631** — [Show context & model speed in WebUI](https://github.com/HKUDS/nanobot/issues/5631) — closed and implemented in PR #5660.
- **#5661** — [OpenCode session header support required after 2026-09-06](https://github.com/HKUDS/nanobot/issues/5661) — not yet commented, but time-sensitive and tied to PR [#5662](https://github.com/HKUDS/nanobot/pull/5662).
- **#5666** — [Add aimlapi.com as an OpenAI-compatible provider](https://github.com/HKUDS/nanobot/pull/5666) — an external contributor/provider partnership proposal, likely to attract ecosystem attention.

### 5. Bugs & Stability

Ranked by severity and impact:

1. **High — Runtime context regression in nanobot-ai 0.3.0**  
   [#5645](https://github.com/HKUDS/nanobot/issues/5645) — `Current Time` runtime context is absent by default in 0.3.0, despite existing timezone documentation. This is a backward-incompatible behavior change compared with 0.2.2. The issue is closed, but no direct fix PR is visible in today’s set.

2. **High / Time-critical — OpenCode requests may fail after 2026-09-06**  
   [#5661](https://github.com/HKUDS/nanobot/issues/5661) — Request without the `x-opencode-session` header will lose prompt-cache optimization and may start erroring. Fix PR [#5662](https://github.com/HKUDS/nanobot/pull/5662) is open and should be prioritized for the next patch.

3. **Medium — Concurrent WebUI locale loading can drop a locale**  
   [#5644](https://github.com/HKUDS/nanobot/issues/5644) — `loadChannelLocale()` has a startup race that can lose a locale when two locales load concurrently. Closed, though no fix PR is visible in this batch.

4. **Medium — WebUI session titles not generated when envelope omits `webui: true`**  
   Referenced as [#5647](https://github.com/HKUDS/nanobot/issues/5647), with two open fix PRs: [#5648](https://github.com/HKUDS/nanobot/pull/5648) and [#5658](https://github.com/HKUDS/nanobot/pull/5658). This is an ongoing regression area for WebUI workflows.

5. **Stability hardening PRs now pending/merged**  
   Multiple PRs target unbounded memory growth in long-running agents:
   - [#5664](https://github.com/HKUDS/nanobot/pull/5664) — Bound the idle-session summary cache.
   - [#5665](https://github.com/HKUDS/nanobot/pull/5665) — Bound retained MCP browser OAuth flows.
   - [#5663](https://github.com/HKUDS/nanobot/pull/5663) — Bound the Mattermost thread context cache.

These reflect a good proactive reliability posture.

### 6. Feature Requests & Roadmap Signals

The following signals suggest where the project is heading:

- **Provider ecosystem expansion**  
  [#5666](https://github.com/HKUDS/nanobot/pull/5666) adds aimlapi.com as an OpenAI-compatible gateway provider. Combined with [#5662](https://github.com/HKUDS/nanobot/pull/5662), provider compatibility is an active roadmap area.

- **Context compaction transparency**  
  [#5656](https://github.com/HKUDS/nanobot/pull/5656) adds `/compact`, structured `context_compaction` lifecycle events, and a stable `compaction_id`. This points toward more user-visible and auditable context management.

- **EPHEMERAL runtime-context blocks**  
  [#5659](https://github.com/HKUDS/nanobot/pull/5659) adds an opt-out flag for runtime-context blocks, allowing session-constant info to be attached without replaying on every turn — likely a reaction to request-size and context-conservation complaints.

- **Expanded filesystem tools**  
  [#5626](https://github.com/HKUDS/nanobot/pull/5626) adds first-class `copy_file` and `move_file` tools, reducing multi-step model workarounds.

- **Channel UX polish**  
  [#5567](https://github.com/HKUDS/nanobot/issues/5567) is still open and is the clearest user-facing channel improvement request. It may be picked up if Feishu is a strategic channel.

Likely next-version candidates: the OpenCode session header fix ([#5662](https://github.com/HKUDS/nanobot/pull/5662)), bounded cache fixes ([#5663](https://github.com/HKUDS/nanobot/pull/5663), [#5664](https://github.com/HKUDS/nanobot/pull/5664), [#5665](https://github.com/HKUDS/nanobot/pull/5665)), and possibly the merged/closed WebUI speed feature already in progress.

### 7. User Feedback Summary

Real user pain points visible in this window:

- **Channel message flooding is confusing** — Feishu users want all agent progress and final output consolidated into a single streaming card ([#5567](https://github.com/HKUDS/nanobot/issues/5567)).
- **WebUI observability is important** — Users want visible model generation speed, context usage, and token information directly in the main UI ([#5631](https://github.com/HKUDS/nanobot/issues/5631)). That request was delivered quickly via [#5660](https://github.com/HKUDS/nanobot/pull/5660).
- **Upgrades can silently change behavior** — A user reported that upgrading to 0.3.0 removed the default `Current Time` runtime context block, causing a regression in prompt content ([#5645](https://github.com/HKUDS/nanobot/issues/5645)).
- **Developers want cost and context control** — Long-standing heartbeat PRs ([#4549](https://github.com/HKUDS/nanobot/pull/4549), [#4551](https://github.com/HKUDS/nanobot/pull/4551)) indicate real demand for isolated sessions and cheaper heartbeat model overrides.
- **Built-in productivity tools matter** — Contributors request basic FS operations like copy/move rather than forcing agents to chain read/write calls ([#5626](https://github.com/HKUDS/nanobot/pull/5626)).

Overall, contributors seem engaged and maintainers are responsive, but some open PRs and older regressions are waiting for release attention.

### 8. Backlog Watch

Several long-open PRs need maintainer merge decisions or continued review:

- **Heartbeat feature pair — open since 2026-06-26**  
  - [#4551](https://github.com/HKUDS/nanobot/pull/4551): Add `isolated_session` config for shared-session heartbeat support.  
  - [#4549](https://github.com/HKUDS/nanobot/pull/4549): Add `model_override` for cheaper heartbeat model execution.

- **Memory integrity fix — open since 2026-08-13**  
  [#5379](https://github.com/HKUDS/nanobot/pull/5379) — Preserve full memory consolidation input to avoid losing raw-fallback data.

- **Background task reliability — open since 2026-08-18**  
  [#5431](https://github.com/HKUDS/nanobot/pull/5431) — Report background task failures properly instead of silently discarding exceptions.

- **WebUI token-usage clarity — open since 2026-08-22**  
  [#5490](https://github.com/HKUDS/nanobot/pull/5490) — Clarify aggregate turn token usage; tagged as a regression fix and has a `conflict` label.

- **Model retry visibility — open since 2026-08-24**  
  [#5504](https://github.com/HKUDS/nanobot/pull/5504) — Surface model retry status in TUI and WebUI while keeping ordinary chat channels clean.

- **Codex Langfuse tracing — open since 2026-08-24**  
  [#5520](https://github.com/HKUDS/nanobot/pull/5520) — Adds observability tracing for Codex provider requests.

These items are not necessarily stale, but the long lead times — especially for the June-era heartbeat PRs — suggest they need maintainer attention, conflict resolution, or explicit closure.

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent Project Digest — 2026-09-05

## 1. Today's Overview

Hermes Agent is in a high-activity phase: 50 issues and 50 PRs were updated in the last 24 hours, with 47 issues and 47 PRs remaining open/active and 3 closed/merged in each category. No new releases were published. Activity is dominated by P1/P2 reliability defects: Desktop reasoning-block settings being ignored, SSH remote-mode 401/session-token failures, SQLite live-WAL state.db corruption from second writers, and Windows terminal hangs. The project also has a substantial bot/group-chat feature train in flight, indicating a roadmap focus on keeping bots operational independently of the Desktop client.

## 2. Releases

No new Hermes Agent releases were published on 2026-09-05. Several issues reference `main` and v0.21.0 behavior, but no formal release, changelog, or migration notes are included in today’s data.

## 3. Project Progress

No prominent merged feature was visible in the top-20 PR sample. Three PRs moved to merged/closed status overall; the one visible closed PR is:

- [#103394 fix(cli): propagate non-zero exit codes on cron and webhook subcommand failures](https://github.com/NousResearch/hermes-agent/pull/103394) — closed, marked as a duplicate.

Open PRs actively advancing today include important fixes and features:

- [#98307 feat(bot-mode): complete Group Chat continuity, control, and files](https://github.com/NousResearch/hermes-agent/pull/98307) — the largest feature PR in review for bot/group-chat continuity.
- [#98073 feat(bot-mode): control Group Chats from messaging](https://github.com/NousResearch/hermes-agent/pull/98073) — companion PR for remote/messaging control of group chats.
- [#103399 fix(desktop): prevent background tile reconcile from starving pool slots](https://github.com/NousResearch/hermes-agent/pull/103399) — targets a 20-profile local backend pool bug.
- [#103402 fix(tools): prevent terminal probe hang and kill orphaned processes on Windows](https://github.com/NousResearch/hermes-agent/pull/103402)
- [#103400 fix(desktop): disable QuickEdit so Select mode cannot block update handoff](https://github.com/NousResearch/hermes-agent/pull/103400)
- [#103405 fix(terminal): stop session snapshot from executing orphaned function bodies](https://github.com/NousResearch/hermes-agent/pull/103405)
- [#103369 fix(cli): resume one-shot sessions](https://github.com/NousResearch/hermes-agent/pull/103369)
- [#103397 fix(agent): record pre-override rough estimate as defer baseline to prevent spurious compaction](https://github.com/NousResearch/hermes-agent/pull/103397)

## 4. Community Hot Topics

- [#66616 [skills-index-watchdog] Skills index is stale or degraded](https://github.com/NousResearch/hermes-agent/issues/66616) — **157 comments**, the most active thread by far. Although this is an automated freshness watchdog issue, it has stayed open since July 18 and signals that the docs/skills-index generation pipeline remains unreliable.

- [#97681 Bot Group Chats should keep working after Desktop closes](https://github.com/NousResearch/hermes-agent/issues/97681) — **23 comments**, and strongly tied to the large open PRs [#98307](https://github.com/NousResearch/hermes-agent/pull/98307) and [#98073](https://github.com/NousResearch/hermes-agent/pull/98073). Users clearly want bots to be first-class gateway citizens rather than dependent on a Desktop session.

- [#18715 Support remote Hermes agent with local tool execution](https://github.com/NousResearch/hermes-agent/issues/18715) — **18 comments and 29 👍 reactions**. This is one of the clearest user-demand signals in the issue tracker: users want remote orchestration with local tool execution.

- [#49664 Desktop: display.show_reasoning toggle has no effect](https://github.com/NousResearch/hermes-agent/issues/49664) — **6 comments**, but duplicated by [#93817](https://github.com/NousResearch/hermes-agent/issues/93817) and related to [#85110](https://github.com/NousResearch/hermes-agent/issues/85110). This is a highly visible Desktop UX bug.

PR comment counts were not reliably available in the top-20 PR sample, but the above issue threads represent the main community pressure points.

## 5. Bugs & Stability

### Highest severity — P1 active issues

- [#103339 Second writer via `doctor --fix` / `repair_state_db_schema` corrupts live-WAL state.db](https://github.com/NousResearch/hermes-agent/issues/103339) — multi-profile host suffered **7 state.db corruptions in 4 days**. The reporter proposes a lazy flock single-writer gate. No fix PR is visible yet; this is a serious correctness/session-state issue.

- [#103054 Dashboard serves a stale session token after `--ssh-session-token-file`](https://github.com/NousResearch/hermes-agent/issues/103054), [#103366 Desktop SSH Isolated refreshProfiles 401s](https://github.com/NousResearch/hermes-agent/issues/103366), and [#103313 Desktop SSH remote mode 401s every sensitive API call](https://github.com/NousResearch/hermes-agent/issues/103313) (closed as duplicate) — SSH remote mode is broadly failing with stale session tokens. The duplicate cluster indicates a real multi-user regression.

- [#102486 Restart-safe cron worker dispatch fails closed on systemd 249 — `OOMPolicy=kill` rejected](https://github.com/NousResearch/hermes-agent/issues/102486) — post-v0.21.0 regression breaking gateway cron workers on common systemd versions.

- [#98022 `hermes update` fleet restart re-fires when `update_receipts/latest.json` is stale](https://github.com/NousResearch/hermes-agent/issues/98022) — update loop can restart fleets forever even when already current.

- Desktop reasoning-block display issue cluster: [#49664](https://github.com/NousResearch/hermes-agent/issues/49664), [#93817](https://github.com/NousResearch/hermes-agent/issues/93817), and [#85110](https://github.com/NousResearch/hermes-agent/issues/85110). Users report that `display.show_reasoning: false` is ignored and the Desktop still dumps reasoning and tool calls into transcripts.

### Other notable bugs

- [#103375 Bot tiles auto-reconnect in an infinite loop, starving the local backend pool](https://github.com/NousResearch/hermes-agent/issues/103375) — an active fix PR exists: [#103399](https://github.com/NousResearch/hermes-agent/pull/103399).
- [#103398 Windows: `terminal` tool hangs for minutes on trivial commands](https://github.com/NousResearch/hermes-agent/issues/103398) — fix PR in review: [#103402](https://github.com/NousResearch/hermes-agent/pull/103402).
- [#103303 Kanban: `decompose_triage_task` lets scratch siblings inherit root `workspace_path`](https://github.com/NousResearch/hermes-agent/issues/103303) — concurrent workers share one directory.
- [#103401 Failed to switch profile: backend start timed out waiting for a free slot](https://github.com/NousResearch/hermes-agent/issues/103401) — profile-switching instability with five profiles.
- [#103287 `/steer` confirms “queued” but silently strands text when no run is active](https://github.com/NousResearch/hermes-agent/issues/103287)
- [#96418 Loopback bind disables WS keepalive ping, leaking a PTY child per dead client](https://github.com/NousResearch/hermes-agent/issues/96418)
- [#52382 “Unknown toolsets: messaging” warning on every start after toolset removal](https://github.com/NousResearch/hermes-agent/issues/52382)

## 6. Feature Requests & Roadmap Signals

The clearest roadmap signal is **bot/group-chat continuity**. Issue [#97681](https://github.com/NousResearch/hermes-agent/issues/97681) plus open PRs [#98307](https://github.com/NousResearch/hermes-agent/pull/98307) and [#98073](https://github.com/NousResearch/hermes-agent/pull/98073) suggest Hermes is moving toward a gateway-owned architecture where bots continue running, exchanging messages/files, and can be controlled remotely even after Desktop closes.

Other feature signals:

- [#18715 Support remote Hermes agent with local tool execution](https://github.com/NousResearch/hermes-agent/issues/18715) — the most broadly endorsed open feature issue, with 29 👍. This may be a candidate for the next architecture push.
- [#103368 Antigravity/Gemini ACP support](https://github.com/NousResearch/hermes-agent/issues/103368) — filed today; users expect official `antigravity` profile support.
- [#100428 Let `browser_exec` choose headed mode per local session](https://github.com/NousResearch/hermes-agent/issues/100428)
- [#100944 Kanban: deny worker create/link per profile while retaining lifecycle tools](https://github.com/NousResearch/hermes-agent/issues/100944)
- [#103404 PR: `pre_tool_call` serve directive — supply a tool result without executing](https://github.com/NousResearch/hermes-agent/pull/103404) — a useful plugin-system extension if merged.

Most likely next-version content: the Group Chat field build already being implemented in PRs, plus targeted fixes for Desktop tile/backend pool starvation, Windows terminal hangs, and SSH session-token handling.

## 7. User Feedback Summary

Real user pain points in this window:

- **Desktop transparency controls are not working.** Users describe Hermes Desktop as unusable when “Reasoning Blocks” are off but reasoning/tool-call traces still flood the transcript. See [#85110](https://github.com/NousResearch/hermes-agent/issues/85110), [#93817](https://github.com/NousResearch/hermes-agent/issues/93817), and [#49664](https://github.com/NousResearch/hermes-agent/issues/49664).
- **SSH remote mode has an authentication regression.** Users report “SSH connection failed” even when OpenSSH succeeds, or empty profiles rails caused by repeated 401s. See [#103366](https://github.com/NousResearch/hermes-agent/issues/103366) and [#103054](https://github.com/NousResearch/hermes-agent/issues/103054).
- **Power users are running many profiles and hitting local process-pool limits.** See [#103375](https://github.com/NousResearch/hermes-agent/issues/103375) and [#103401](https://github.com/NousResearch/hermes-agent/issues/103401).
- **Update/install friction continues.** Windows QuickEdit can stall Desktop updates ([#103400 PR](https://github.com/NousResearch/hermes-agent/pull/103400)), stale update receipts cause endless fleet restarts ([#98022](https://github.com/NousResearch/hermes-agent/issues/98022)), and stale config entries cause warnings on every start ([#52382](https://github.com/NousResearch/hermes-agent/issues/52382)).
- **Users want a hybrid deployment model.** High interest in keeping remote agents connected to local machine tooling ([#18715](https://github.com/NousResearch/hermes-agent/issues/18715)), and in group chats that outlive the Desktop client ([#97681](https://github.com/NousResearch/hermes-agent/issues/97681)).

## 8. Backlog Watch

Items that have been open a long time and still carry clear user value:

- [#66616 Skills index is stale or degraded](https://github.com/NousResearch/hermes-agent/issues/66616) — open since July 18 with 157 comments; needs pipeline owner attention.
- [#18715 Support remote Hermes agent with local tool execution](https://github.com/NousResearch/hermes-agent/issues/18715) — open since May 2 with 29 👍; architectural and high-demand.
- [#49664 Desktop `display.show_reasoning` toggle has no effect](https://github.com/NousResearch/hermes-agent/issues/49664) — open since June 20, P1, with open duplicates.
- [#24740 Honcho session titles override sessionStrategy setting](https://github.com/NousResearch/hermes-agent/issues/24740) — open since May 13; touches session/memory behavior.
- [#45562 Desktop should preserve per-session chat scroll/read position](https://github.com/NousResearch/hermes-agent/issues/45562) — open since June 13; a UX feature that has not advanced.

Older PRs still needing review or a decision:

- [#22982 fix(gateway): route model command inline payloads](https://github.com/NousResearch/hermes-agent/pull/22982) — open since May 10.
- [#44551 test(tui): cover slash worker protocol and startup](https://github.com/NousResearch/hermes-agent/pull/44551) — open since June 12.
- [#78495 fix(discord): accept relay-lane kwargs in native rename_thread](https://github.com/NousResearch/hermes-agent/pull/78495) — open since August 4.
- [#87573 fix(plugins): clearer bare-name install error when community index is unreachable](https://github.com/NousResearch/hermes-agent/pull/87573) — open since August 16.
- [#88465 fix(config): decode JSON-string-typed providers at load time](https://github.com/NousResearch/hermes-agent/pull/88465) — open since August 17.

Overall, Hermes Agent is showing a healthy but stretched issue/PR pipeline. The biggest risk areas today are Desktop session/display reliability, SSH remote-mode authentication, and local backend/profile process lifecycle management on multi-profile systems.

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

# PicoClaw Project Digest — 2026-09-05

**Source:** github.com/sipeed/picoclaw

---

## 1. Today's Overview

PicoClaw is in a moderate-activity maintenance window: no releases landed in the last 24 hours, 3 issues are active (all open), and 22 PRs changed state — 20 of which are now closed/merged. The dominant event is a large backlog cleanup in which a batch of `[stale]` PRs from March–August (mostly Go provider/channel fixes and enhancements) was closed without visible maintenance action. Meanwhile, community contribution has shifted toward documentation: the two remaining open PRs are both MCP setup guides (Parallel Search, Pilot Protocol). Open issues reflect real user friction areas: Web UI performance with long histories, IRC long-message handling, and provider extensibility. Overall project health is stable but shows a slow-maintainer risk pattern, with several valuable bug-fix PRs possibly needing to be re-landed.

---

## 2. Releases

No new releases reported. Latest release data: none.

---

## 3. Project Progress

20 PRs were closed/merged in the last 24 hours. They fall into two categories:

**Active/possibly merged closures (no `[stale]` marker):**
- **#1541** — Aggregate fix merging media temp dir handling, channel DoS hardening, and DeepWiki badge updates ([PR #1541](https://github.com/sipeed/picoclaw/pull/1541))
- **#2016** — Improved context-overflow error detection/classification for Anthropic, ZhipuAI, GLM ([PR #2016](https://github.com/sipeed/picoclaw/pull/2016))
- **#2014** — Agent token estimation now includes SystemParts; added reasoning guards ([PR #2014](https://github.com/sipeed/picoclaw/pull/2014))
- **#1855** — Fixed `isNumeric` rejecting negative Telegram group/channel IDs ([PR #1855](https://github.com/sipeed/picoclaw/pull/1855))
- **#1860** — Azure AI Foundry host recognition enabling prompt caching and native search ([PR #1860](https://github.com/sipeed/picoclaw/pull/1860))
- **#2088** — Security audit/hardening for open-by-default bots with empty `allow_from` lists ([PR #2088](https://github.com/sipeed/picoclaw/pull/2088))
- **#2092** — Telegram duplicate-message fix on streaming edit timeouts ([PR #2092](https://github.com/sipeed/picoclaw/pull/2092))

**Closed as `[stale]` (likely not merged — see Backlog Watch):**
- **#3337** — MCP connection failure hung agent loop; was the most recent substantive fix attempted ([PR #3337](https://github.com/sipeed/picoclaw/pull/3337))
- **#1683** — OpenAI strict-mode compatibility for third-party providers ([PR #1683](https://github.com/sipeed/picoclaw/pull/1683))
- **#1854** — Occurrence-aware `tool_call_id` sanitization for Anthropic/Cerebras ([PR #1854](https://github.com/sipeed/picoclaw/pull/1854))
- **#1858** — `thinking`/`reasoning` fallback for Ollama/DeepSeek-R1 outputs ([PR #1858](https://github.com/sipeed/picoclaw/pull/1858))
- **#2090 / #2089 / #2091** — Telegram streaming drafts/routing, Slack mention race condition, Feishu group mention detection ([PR #2090](https://github.com/sipeed/picoclaw/pull/2090), [PR #2089](https://github.com/sipeed/picoclaw/pull/2089), [PR #2091](https://github.com/sipeed/picoclaw/pull/2091))
- **#2240** — GitHub Copilot stdio transport support ([PR #2240](https://github.com/sipeed/picoclaw/pull/2240))
- **#2260** — xAI provider compatibility ([PR #2260](https://github.com/sipeed/picoclaw/pull/2260))
- **#2298** — Fail-closed exec script preflight hardening ([PR #2298](https://github.com/sipeed/picoclaw/pull/2298))
- **#2522** — Streaming usage support in `openai_compat` for OpenAI/Azure ([PR #2522](https://github.com/sipeed/picoclaw/pull/2522))

**Docs advances (open):**
- **#3368** — New copy-paste setup example for Parallel Search MCP in the CLI guide ([PR #3368](https://github.com/sipeed/picoclaw/pull/3368))
- **#3367** — Pilot Protocol MCP setup example with health-check command ([PR #3367](https://github.com/sipeed/picoclaw/pull/3367))

---

## 4. Community Hot Topics

- **[Issue #3287 — IRC long-message support](https://github.com/sipeed/picoclaw/issues/3287)** (10 comments)
  Request that PicoClaw treat IRCv3 messages split by the 512-byte limit / newline boundaries as single cohesive messages. Long-running since July 22; no maintainer response visible. Underlying need: correct IRC message semantics for bots in real-world clients.

- **[Issue #3281 — Web UI chat input laggy with long history](https://github.com/sipeed/picoclaw/issues/3281)** (9 comments, 👍 2)
  Users on v0.3.1 / Go 1.25.11 report severe input lag as session history grows. The most-endorsed open bug; it points to frontend rendering inefficiency (likely O(n) re-renders over message count).

- **[PR #3368 — Parallel Search MCP docs](https://github.com/sipeed/picoclaw/pull/3368)** and **[PR #3367 — Pilot MCP docs](https://github.com/sipeed/picoclaw/pull/3367)**
  Both freshly opened by external contributors. Signal strong community demand for turnkey MCP setup paths — particularly search/extraction without API keys and self-hostable "no API key" assistants.

- **[Issue #3366 — OpenAI-compatible providers](https://github.com/sipeed/picoclaw/issues/3366)** (0 comments)
  New, unanswered. Note: the codebase already has an `openai_compat` provider layer (see PRs #1683, #2522), so this likely reflects a configuration/discoverability gap around custom base URLs for self-hosted routers, not a greenfield feature.

---

## 5. Bugs & Stability

Ranked by severity/impact:

1. **Web UI lag with long histories (High visibility, unfixed)** — [#3281](https://github.com/sipeed/picoclaw/issues/3281)
   Reported July 21 on v0.3.1; still open with 2 👍 and 9 comments. No associated fix PR exists. This degrades the core UX for anyone using the Web channel beyond short sessions.

2. **MCP server failure hangs the agent loop (High impact, fix lost)** — [#3337](https://github.com/sipeed/picoclaw/pull/3337)
   Fix PR existed: when `ensureMCPInitialized` errors (unreachable/broken MCP server), `AgentLoop.Run` propagated the error and the chat interface stopped replying entirely. PR was closed as `[stale]`; the underlying bug therefore remains unmitigated in trunk.

3. **Duplicate tool_call_id 400 errors on strict providers (Medium-High)** — fix in closed-as-stale [#1854](https://github.com/sipeed/picoclaw/pull/1854)
   Anthropic/Cerebras reject reused `tool_call_id` values; occurrence-aware sanitization was implemented but not merged.

4. **Telegram/Slack/Feishu channel defects (Medium)** — fixes in stale-closed PRs [#2089](https://github.com/sipeed/picoclaw/pull/2089), [#2090](https://github.com/sipeed/picoclaw/pull/2090), [#2091](https://github.com/sipeed/picoclaw/pull/2091), [#2092](https://github.com/sipeed/picoclaw/pull/2092)
   Duplicate Slack responses on @mention race, Telegram streaming drafts/forum misrouting, Feishu false-negative @mentions, and Telegram duplicate messages on edit timeout were all fixed in PRs that may not have landed.

5. **Reasoning-model output loss on Ollama (Medium)** — fix in stale-closed [#1858](https://github.com/sipeed/picoclaw/pull/1858)
   DeepSeek-R1-style outputs could be dropped when `content` is empty.

---

## 6. Feature Requests & Roadmap Signals

- **Custom OpenAI-compatible provider endpoints** — [#3366](https://github.com/sipeed/picoclaw/issues/3366)
  User wants "OpenAI Compatible" as a first-class custom provider to point at self-hosted routers (e.g., "9Router"). Since the compatibility layer already exists internally, likely shipped soon as user-facing configuration + docs.

- **IRC long-message cohesion** — [#3287](https://github.com/sipeed/picoclaw/issues/3287)
  Has been open for ~6 weeks with active discussion; IRCv3 continues to be used by real deployments. A candidate for a near-term channel-layer improvement.

- **MCP ecosystem momentum** — incoming docs PRs for Parallel Search and Pilot Protocol (both no-API-key or self-hostable options) suggest the maintainers' MCP CLI guide is becoming a de-facto onboarding surface; expect more third-party MCP examples and possibly MCP config UX work.

- **Provider expansion signals from stale closures** — xAI support (#2260) and GitHub Copilot stdio (#2240) were feature-complete and tested. If maintainers resurrect them, next releases could broaden the provider matrix meaningfully.

---

## 7. User Feedback Summary

- **Pain point — Web UI performance:** Typing degrades as history grows ([#3281](https://github.com/sipeed/picoclaw/issues/3281)); users on latest release (0.3.1) are affected and the issue has 2 👍 — the clearest dissatisfaction signal currently.
- **Pain point — IRC semantics:** Clients split >512-byte messages, and PicoClaw misinterprets chunks as new distinct messages, breaking conversation flow ([#3287](https://github.com/sipeed/picoclaw/issues/3287)).
- **Desire for self-hosted/private setups:** Requests center on OpenAI-compatible routers ([#3366](https://github.com/sipeed/picoclaw/issues/3366)) and no-API-key MCP services (Parallel, Pilot) — users want optionality and data control.
- **Contributor engagement is high but satisfaction is at risk:** One contributor (badgerbees) authored the vast majority of the substantial provider/channel fixes closed as stale; silent closure without merge decisions may discourage future deep contributions.

---

## 8. Backlog Watch

- **[Issue #3287 — IRC long messages](https://github.com/sipeed/picoclaw/issues/3287)** — Open since 2026-07-22, 10 comments, no maintainer response. Needs triage or an explicit roadmap answer.
- **[Issue #3281 — Laggy Web UI](https://github.com/sipeed/picoclaw/issues/3281)** — Open since 2026-07-21, 9 comments, no linked fix or milestone. Most upvoted open bug.
- **[Issue #3366 — OpenAI-compatible providers](https://github.com/sipeed/picoclaw/issues/3366)** — Reported 2026-09-04; unanswered. Since the feature partially exists, a clarification of supported configuration is warranted.
- **[PR #3337 — MCP failure hang fix](https://github.com/sipeed/picoclaw/pull/3337)** — Closed as stale on 2026-09-04 after being open since Aug 14. This is a correctness fix for a severe availability bug and should be revived or re-implemented.
- **13 `[stale]` PRs closed in the cleanup wave** — Includes provider/channel bug fixes (#1854, #1858, #2089, #2090, #2091), security hardening (#2298), and completed features (#2240, #2260). Maintainers should audit each for re-application; otherwise these fixes are effectively lost from the mainline.
- **[PR #1541](https://github.com/sipeed/picoclaw/pull/1541)** — Aggregate fix PR from March updated 2026-09-05; unclear whether merged or closed-superseded. Status should be clarified for traceability of its three component fixes (media tempdir, DoS hardening, DeepWiki badge).

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw Project Digest — 2026-09-05

## Today’s Overview
NanoClaw is in an active maintenance-and-refactor cycle: 18 PRs were updated in the last 24 hours, 2 open issues are actively tracked, and 3 PRs moved to closed/merged status. No new releases were published. The dominant workstream is provider contract consolidation, with additional focus on skill installation hardening, agent-to-agent messaging, and mount security. The open issues are both operational/configuration concerns, one of which reports a production OOM crash loop and needs immediate attention.

## Releases
No new releases are recorded for this window. Since there are no release artifacts or changelog entries, there are no breaking-change or migration notes to report.

## Project Progress
The three PRs that were closed/merged in the last 24 hours are:

- [#2403](https://github.com/nanocoai/nanoclaw/pull/2403) — **CI/release hardening**: replaces the old `bump-version` flow with an explicit Release workflow and adds a concurrency guard.
- [#2231](https://github.com/nanocoai/nanoclaw/pull/2231) — **Chat SDK bridge**: adds a `sendAsRaw` flag to bypass adapter Markdown round-trips.
- [#2232](https://github.com/nanocoai/nanoclaw/pull/2232) — **Chat SDK bridge**: falls back to URL fetch for adapters that do not expose `fetchData`.

The PRs are older additions, but their closure clears long-standing bridge and release-process work. Broader active progress continues in the provider-contract refactor series, especially around Codex, OpenCode, Cursor, and provider instruction rendering.

## Community Hot Topics
The most active item by comment count is:

- [#3716](https://github.com/nanocoai/nanoclaw/issues/3716) — **PreCompact conversation-archive writes a full, unbounded rewrite per firing**, reported as the actual cause of a production OOM crash loop. With 2 comments, this is the clearest signal of maintainer/user attention today. The underlying need is conversation-archive lifecycle management: rotation, caps, cleanup, or incremental writes instead of full re-serialization.

Also notable:

- [#3714](https://github.com/nanocoai/nanoclaw/issues/3714) — **Operator env overrides never reach the session container**, preventing operators from setting documented knobs such as `CLAUDE_CODE_AUTO_COMPACT_*` without patching. It is a direct follow-up to #1820 and an unresolved operator-experience gap.

## Bugs & Stability
Ranked by severity:

1. **Critical — Production OOM crash loop**  
   [#3716](https://github.com/nanocoai/nanoclaw/issues/3716)  
   Every `PreCompact` firing writes a brand-new full conversation-history file into the conversations directory with no rotation, cap, or cleanup, causing unbounded disk/memory growth. No dedicated fix PR is visible in the current data, making this the top stability risk.

2. **High — Documented operator env overrides silently do nothing**  
   [#3714](https://github.com/nanocoai/nanoclaw/issues/3714)  
   Auto-compact window and transcript-rotation overrides are never forwarded from the host into the session container. Operators cannot use the documented configuration surface without patching code.

3. **Active hardening PRs with fix intent**  
   - [#3717](https://github.com/nanocoai/nanoclaw/pull/3717) — escapes embedded payloads inside composed prompt blocks, preventing payloads from closing-and-forging surrounding structures.  
   - [#3718](https://github.com/nanocoai/nanoclaw/pull/3718) — preserves verified agent identity and command boundaries in A2A messages, preventing legitimate requests from being refused.  
   - [#3719](https://github.com/nanocoai/nanoclaw/pull/3719) — reports A2A communication failures back to the originating chat.  
   - [#3680](https://github.com/nanocoai/nanoclaw/pull/3680) — closes an allowlisted-extra mount bypass in `validateSpec`.

## Feature Requests & Roadmap Signals
There are few new issue-tracked feature requests, but the PR activity strongly signals the near-term roadmap:

- **Provider contract completion** continues across [#3586](https://github.com/nanocoai/nanoclaw/pull/3586), [#3588](https://github.com/nanocoai/nanoclaw/pull/3588), [#3584](https://github.com/nanocoai/nanoclaw/pull/3584), [#3591](https://github.com/nanocoai/nanoclaw/pull/3591), and [#3722](https://github.com/nanocoai/nanoclaw/pull/3722). These are likely to land as part of the next provider/runtime release.
- **Capability installation skills** are being added and hardened: [#3720](https://github.com/nanocoai/nanoclaw/pull/3720) opt-in source installation, [#3721](https://github.com/nanocoai/nanoclaw/pull/3721) explicit install with operator policy, [#3715](https://github.com/nanocoai/nanoclaw/pull/3715) Zapier MCP access, and [#3355](https://github.com/nanocoai/nanoclaw/pull/3355) `/add-cursor` install skill.
- **New provider support** is still in-flux, primarily Cursor Agent SDK via [#3356](https://github.com/nanocoai/nanoclaw/pull/3356).
- **Core-owned agent-group speed inference property** is proposed in [#3592](https://github.com/nanocoai/nanoclaw/pull/3592), suggesting future CLI/config support for per-group `speed` tiers alongside `model` and `effort`.

The likely next-version themes are provider contract consolidation, more explicit skill-based installations, and stronger operator-controlled defaults.

## User Feedback Summary
User-reported pain points are concentrated around operational safety and configuration delivery:

- A production user reports an OOM crash loop caused by unbounded `PreCompact` archive rewriting ([#3716](https://github.com/nanocoai/nanoclaw/issues/3716)). This is the strongest dissatisfaction signal in the dataset.
- Operators report documented env overrides are unusable because they never reach session containers ([#3714](https://github.com/nanocoai/nanoclaw/issues/3714)).
- A2A communication feedback fixes ([#3718](https://github.com/nanocoai/nanoclaw/pull/3718), [#3719](https://github.com/nanocoai/nanoclaw/pull/3719)) suggest users have experienced refused legitimate agent requests and confusing delivery failures.

No direct satisfaction metrics or positive user testimonials are present in this dataset.

## Backlog Watch
Several important provider-contract PRs have been open for more than a week and may need maintainer attention or merge decisions:

- [#3356](https://github.com/nanocoai/nanoclaw/pull/3356) — Cursor Agent SDK payload, opened Aug 19.
- [#3355](https://github.com/nanocoai/nanoclaw/pull/3355) — `/add-cursor` install skill, opened Aug 19.
- [#3584](https://github.com/nanocoai/nanoclaw/pull/3584) — Codex provider contract implementation, opened Aug 27.
- [#3586](https://github.com/nanocoai/nanoclaw/pull/3586) — Provider setup/install verifier contract, opened Aug 27.
- [#3588](https://github.com/nanocoai/nanoclaw/pull/3588) — OpenCode provider contract implementation, opened Aug 27.
- [#3592](https://github.com/nanocoai/nanoclaw/pull/3592) — Core-owned speed inference property, opened Aug 28.
- [#3680](https://github.com/nanocoai/nanoclaw/pull/3680) — Security fix for `validateSpec` mount bypass, opened Aug 30.

These are all labeled `core-team` / `follows-guidelines` and appear to be waiting on review or merge sequencing rather than being abandoned. The mount-security fix, in particular, has been waiting since Aug 30 and deserves prompt attention.

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

# NullClaw Project Digest — 2026-09-05

## 1. Today's Overview
NullClaw had a very quiet day on 2026-09-05: one existing enhancement issue was updated, while no pull requests were active or merged and no new release was published. The only issue receiving attention is a feature request around making the Firecrawl web search provider work with self-hosted instances. There were no new bug reports, regressions, or code changes in the last 24 hours. The project appears to be in a low-activity maintenance phase, with community focus currently centered on configuration flexibility rather than urgent fixes.

## 2. Releases
No new releases were published during the reporting window. No changelogs, breaking changes, or migration notes are therefore available.

## 3. Project Progress
- **Merged/Closed PRs:** 0  
- **Open PRs:** 0  

No feature work, bug fixes, or refactoring advanced through pull requests today. The project had no observable commit/PR progress in the last 24 hours.

## 4. Community Hot Topics
The only actively updated issue is the current focal point of community activity:

- [Issue #993: [enhancement] feat: make Firecrawl search endpoint configurable for self-hosted instances](https://github.com/nullclaw/nullclaw/issues/993)  
  - Author: `Crymfox`  
  - Created: 2026-08-24 | Updated: 2026-09-04  
  - Comments: 1 | Reactions: 0  

This request points out that the Firecrawl provider in `src/tools/web_search_providers/firecrawl.zig` has a hardcoded API endpoint (`https://api.firecrawl.dev/v1/search`), which prevents users with self-hosted Firecrawl instances from using `search_provider: "firecrawl"`. Underlying community need: support for self-hosted/private search infrastructure and reducing dependency on Firecrawl's public API.

## 5. Bugs & Stability
No bug reports, crashes, or stability regressions were reported in the last 24 hours.  

The open issue is not a crash-level bug, but it is a configuration limitation that makes the built-in Firecrawl provider unusable for self-hosted deployments. No fix PR currently exists for this issue.

## 6. Feature Requests & Roadmap Signals
The main roadmap signal today is:

- [Issue #993: Make Firecrawl search endpoint configurable](https://github.com/nullclaw/nullclaw/issues/993)  

If accepted, this feature would likely expose the Firecrawl endpoint through configuration/environment variables instead of hardcoding it in `firecrawl.zig`. Because the issue remains open with no linked PR, it may not land in the immediate next release. However, it is the only active issue in the project and clearly points to growing self-hosting/user-controlled infrastructure requirements.

## 7. User Feedback Summary
The limited observed feedback comes from one user request:

- Users with self-hosted Firecrawl instances want to keep using NullClaw's native Firecrawl search provider.
- The current hardcoded endpoint forces those users to fork/customize code or use a different provider.
- The request is constructive and feature-oriented, meaning the user values NullClaw's built-in integration but needs deployment flexibility.
- No strongly negative feedback or satisfaction complaints were observed in the last 24 hours.

Because the sample size is only one issue, broader sentiment cannot be reliably inferred.

## 8. Backlog Watch
- [Issue #993: Firecrawl endpoint configurability](https://github.com/nullclaw/nullclaw/issues/993) has been open since 2026-08-24 and remains unresolved after roughly 12 days. It is the only issue receiving recent updates, and no linked PR or assignee is visible in the provided data. Maintainer attention would help clarify whether this feature is planned, needs design input, or should be marked as accepted/declined.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw Project Digest — 2026-09-05

## 1. Today's Overview

IronClaw showed steady, high-volume activity over the past 24 hours: 3 issues were updated (2 closed, 1 open), 12 pull requests were touched (3 closed, 9 open), and no release was cut. The closed issue/PR pairs indicate that Telegram onboarding bugs found earlier are now landing fixes, while the 9 open PRs reveal active workstreams in WebUI slash-command polish, subagent lifecycle hardening, and Telegram channel features. Consensus risk labels on open PRs are “low,” and almost all changes come from core/experienced contributors, suggesting healthy, controlled momentum. Overall the project looks stable: intake of bug reports is modest, fixes are shipping promptly, and no release-blocking or critical regressions were reported today.

## 2. Releases

No new releases were published in this window. The latest-release list for IronClaw is currently empty, so this digest covers merged code, surfaced bugs, and in-flight pull requests only.

## 3. Project Progress

Three PRs were closed (and are assessed as landed) in the last 24 hours:

- **[#8054 – fix(assistant): check pairing before command admission so first contact gets the connect notice](https://github.com/nearai/ironclaw/pull/8054)** *(M, risk: low, experienced contributor)* — Fixes the Telegram onboarding bug where an unpaired user’s bare `/start` received the “Available commands” inventory instead of the pairing/connect notice. Closes issue [#7956](https://github.com/nearai/ironclaw/issues/7956).
- **[#8073 – fix(device-link): say “not configured by administrator” instead of blaming the user's account](https://github.com/nearai/ironclaw/pull/8073)** *(M, risk: low, experienced contributor)* — Replaces the misleading “Something went wrong while linking” copy with a clear “not configured by administrator” message when `telegram_api_id`/`telegram_api_hash` are missing. Closes issue [#7955](https://github.com/nearai/ironclaw/issues/7955).
- **[#8062 – fix(llm): send conversation cache keys on OpenAI request paths](https://github.com/nearai/ironclaw/pull/8062)** *(XL, risk: low, core contributor)* — Derives a stable, domain-separated pseudonymous prompt-cache key per conversation at the loop-host gateway and sends it on supported OpenAI Responses and OpenAI-compatible Chat Completions requests, preserved across turns and tool-loop iterations.

**Work in progress (9 open PRs):** Telegram Bot API command-menu registration ([#8072](https://github.com/nearai/ironclaw/pull/8072), L); subagent stranded-delivery boot/periodic sweep ([#8067](https://github.com/nearai/ironclaw/pull/8067), XL) and concurrent-children cap ([#8061](https://github.com/nearai/ironclaw/pull/8061), M); a four-part WebUI slash-command UX series from italic-jinxin ([#8071](https://github.com/nearai/ironclaw/pull/8071), [#8070](https://github.com/nearai/ironclaw/pull/8070), [#8069](https://github.com/nearai/ironclaw/pull/8069), [#8068](https://github.com/nearai/ironclaw/pull/8068)); a Responses-cancel repair ([#8059](https://github.com/nearai/ironclaw/pull/8059)); and the nightly codebase knowledge-graph refresh ([#7988](https://github.com/nearai/ironclaw/pull/7988)).

## 4. Community Hot Topics

All issues and PRs in this snapshot have **0 comments and 0 👍 reactions**, so there is no discussion-driven signal to rank. The most substantive items by developer attention and potential user impact are:

- **[#8074 – Paired user's rejected action in a not-connected shared channel gets the pairing notice copy instead of channel-not-connected copy](https://github.com/nearai/ironclaw/issues/8074)** — The newest open bug; it shows that channel-connectivity copy is still being refined beyond the first-contact and device-linking cases.
- **[#8072 – feat(telegram): register the Bot API command menu at activation](https://github.com/nearai/ironclaw/pull/8072)** — Active large feature (L) that will surface declared commands (`/model`, `/status`, `/new`, `/stop`, `/interrupt`) in Telegram’s chat menu.
- **[#8067 – feat(subagent): boot/periodic sweep for stranded background deliveries (R4)](https://github.com/nearai/ironclaw/pull/8067)** — Large reliability work closing a healing-trigger gap for results written into parent threads that never run again.

Underlying need across all three: users should never be stuck with unclear or contextually wrong messaging about Telegram pairing/connection state, and background subagent deliveries should not be lost even in edge-case thread lifetimes.

## 5. Bugs & Stability

No crashes or critical regressions were reported. All bug activity concerns user-facing messaging clarity and one functional API-cancel defect.

- **Moderate – functional bug (fix pending review): [#8059 – fix(responses): send cancel reason the product surface accepts](https://github.com/nearai/ironclaw/pull/8059)** *(XS, risk: low, new contributor)* — `POST /api/v1/responses/{id}/cancel` currently returns `400 invalid_request` for both in-progress and completed runs, so cancellation can never succeed and runs keep going. The fix PR has been open since September 3 and needs maintainer review/merge.
- **Low-to-moderate – open issue bug: [#8074 – Paired user's rejected action in a not-connected shared channel gets pairing copy instead of channel-not-connected copy](https://github.com/nearai/ironclaw/issues/8074)** *(open, no fix PR linked yet)* — A paired user acting in an unconnected shared channel receives `connect_required` copy written for the unpaired case. Creates confusion about whether the *account* or the *channel* needs connection. Likely to gain a fix PR soon given the two related fixes already landed today.
- **Resolved in this window** — [#7956](https://github.com/nearai/ironclaw/issues/7956) (unpaired `/start` shows command inventory, fixed by [#8054](https://github.com/nearai/ironclaw/pull/8054)) and [#7955](https://github.com/nearai/ironclaw/issues/7955) (generic “Something went wrong” when admin has not configured api_id/api_hash, fixed by [#8073](https://github.com/nearai/ironclaw/pull/8073)).

## 6. Feature Requests & Roadmap Signals

No new feature-request issues were filed in the last 24 hours, but open PRs reveal the active roadmap:

- **Telegram channel polish** — [#8072](https://github.com/nearai/ironclaw/pull/8072) registers the Bot API command menu at activation and clears it on deactivation. Given the concurrent Telegram copy fixes from the same contributor, this looks likely to land in the next release.
- **Subagent reliability program (R3/R4)** — [#8067](https://github.com/nearai/ironclaw/pull/8067) adds the long-missing boot pass for stranded background deliveries plus counters and end-to-end revival tests; [#8061](https://github.com/nearai/ironclaw/pull/8061) caps concurrent children and verifies the child-gate approval card replay. Both are large (M/XL) core changes and appear to be continuations of a planned multi-slice effort.
- **WebUI slash-command UX batch** — A series of four small fixes ([#8071](https://github.com/nearai/ironclaw/pull/8071), [#8070](https://github.com/nearai/ironclaw/pull/8070), [#8069](https://github.com/nearai/ironclaw/pull/8069), [#8068](https://github.com/nearai/ironclaw/pull/8068)) targets card height preservation, metadata alignment, dismiss actions, and keeping the active slash command visible during keyboard/pointer navigation. These are low-risk XS/M size and look like a candidate batch for an imminent minor release.
- **Nightly codebase memory refresh** — [#7988](https://github.com/nearai/ironclaw/pull/7988) refreshes the committed knowledge-graph bootstrap snapshot via CI; this is infrastructure hygiene rather than a user-visible feature.

## 7. User Feedback Summary

There were no direct user comments or reactions in this window, so feedback is inferred from bug reports and the fixes they produced:

- **First-contact Telegram confusion (resolved):** Unpaired users tapping Start saw a command inventory instead of connection/pairing instructions ([#7956](https://github.com/nearai/ironclaw/issues/7956)). The fix redirects first contact to the connect notice, directly addressing a confusing cold-start experience.
- **Admin misconfiguration blamed on users (resolved):** When Telegram personal-account linking was not configured by the administrator, users were told “Something went wrong while linking” ([#7955](https://github.com/nearai/ironclaw/issues/7955)). The fix now correctly frames the issue as an administrator configuration gap, reducing user-side blame and support noise.
- **Channel-vs-account connection state still confusing (open):** [#8074](https://github.com/nearai/ironclaw/issues/8074) reports that copy written for unpaired accounts leaks into the paired-user/not-connected-channel case. This suggests remaining dissatisfaction with how clearly IronClaw communicates different connection states.

Overall, user pain points cluster around Telegram linking and connect-state messaging; all reported issues are copy/clarity defects rather than crashes or data-loss complaints.

## 8. Backlog Watch

Items that are aging or appear to need maintainer attention:

- **[#7988 – chore(agents): refresh codebase knowledge graph](https://github.com/nearai/ironclaw/pull/7988)** *(XS, CI)* — Open since August 29 (~7 days); generated by the nightly workflow and awaiting human review/merge. Low urgency but should be regularly merged to keep the committed bootstrap snapshot fresh.
- **[#8059 – fix(responses): send cancel reason the product surface accepts](https://github.com/nearai/ironclaw/pull/8059)** *(XS, new contributor)* — Open since September 3; addresses an API cancel path that can never succeed in any state. A functional defect of this kind deserves prompt maintainer review to avoid lingering broken behavior in the Responses surface.
- **[#8067 – feat(subagent): boot/periodic sweep for stranded background deliveries (R4)](https://github.com/nearai/ironclaw/pull/8067)** *(XL)* and **[#8061 – feat(subagent): concurrent-children cap](https://github.com/nearai/ironclaw/pull/8061)** *(M)* — Both have been open since September 3–4. They are large, multi-part core changes with substantial validation scope; they are not stale, but given their size they may need time to converge and should be watched to prevent bit-rot.
- **[#8074 – open channel-connectivity copy bug](https://github.com/nearai/ironclaw/issues/8074)** — Filed September 4 with no linked fix PR yet. Given that two near-identical Telegram copy bugs were fixed within the same 24-hour window, this one should get a dedicated fix soon.

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# LobsterAI Project Digest — 2026-09-05

## 1. Today's Overview

LobsterAI had a release-heavy 24 hours: two new releases (`2026.9.4`, `2026.9.3`) were published, and 28 of the 33 recently touched PRs were merged/closed, leaving 5 PRs still open. The merged `Release/2026.9.4` branch (#2618) consolidated work across browser, update flow, publishing, and stability areas. Only 1 issue was updated, and it is a stale, high-severity SQLite reliability report (#1071) that remains open without a visible fix PR. Overall, project throughput is strong and release cadence is healthy, but the unresolved storage-layer bug remains a production-risk signal.

## 2. Releases

### [LobsterAI 2026.9.4](https://github.com/netease-youdao/LobsterAI/releases)
Visible release notes:
- **feat(browser): restore interactive in-app browser** by @btc69m979y-dotcom — [#2602](https://github.com/netease-youdao/LobsterAI/pull/2602)
- **feat(update): confirm before install and quitting the app** by @fisherdaddy — [#2609](https://github.com/netease-youdao/LobsterAI/pull/2609)
- **feat(publishi...** — release note text truncated in source

No breaking changes or migration notes were included in the available data.

### [LobsterAI 2026.9.3](https://github.com/netease-youdao/LobsterAI/releases)
Visible release notes:
- **feat(cowork): show login prompt before unauthenticated chat** by @liuzhq1986 — [#2573](https://github.com/netease-youdao/LobsterAI/pull/2573)
- **feat(browser): add interactive in-app browser** by @btc69m979y-dotcom — [#2574](https://github.com/netease-youdao/LobsterAI/pull/2574)
- **feat(onboarding)...** — release note text truncated in source

No migration guidance was provided in the release data.

## 3. Project Progress

Merged/closed PRs in the last 24 hours show concentrated progress in browser UX, login/auth flows, publishing/subscription, and general Electron quality-of-life fixes.

### Release & Publishing
- **Release/2026.9.4 merged** — [#2618](https://github.com/netease-youdao/LobsterAI/pull/2618)
- **feat(publishing): 完善订阅恢复引导与资源状态同步** — improved subscription recovery guidance, artifact/library/site restore entries, and analytics — [#2613](https://github.com/netease-youdao/LobsterAI/pull/2613)
- **fix(config): 修正测试模式服务端 API 地址** — corrected test-mode server API endpoint — [#2614](https://github.com/netease-youdao/LobsterAI/pull/2614)

### Browser & Electron
- **fix(browser): support Unicode Windows install paths** — [#2615](https://github.com/netease-youdao/LobsterAI/pull/2615)
- **feat(electron): add edit context menu for text inputs** — cut/copy/paste/select-all support — [#2503](https://github.com/netease-youdao/LobsterAI/pull/2503)
- **fix(electron): preserve message selection for context menu** — [#2521](https://github.com/netease-youdao/LobsterAI/pull/2521)

### Cowork, Login & Analytics
- **feat(cowork): show login prompt before unauthenticated chat** — [#2573](https://github.com/netease-youdao/LobsterAI/pull/2573)
- **fix(cowork): preserve model display during login refresh** — [#2612](https://github.com/netease-youdao/LobsterAI/pull/2612)
- **fix(analytics): track chat login CTA clicks** — [#2596](https://github.com/netease-youdao/LobsterAI/pull/2596)

### UI & Renderer Polish
- **fix(skills): portal upgrade progress overlay** — [#2501](https://github.com/netease-youdao/LobsterAI/pull/2501)
- **fix(i18n): refine voice quota exhausted copy** — [#2603](https://github.com/netease-youdao/LobsterAI/pull/2603)
- **fix(im): improve bot card layout** — [#2599](https://github.com/netease-youdao/LobsterAI/pull/2599)
- **fix(plugins): keep install modal usable with long errors** — [#2520](https://github.com/netease-youdao/LobsterAI/pull/2520)
- **fix(sidebar): fade out login promo tip** — [#2532](https://github.com/netease-youdao/LobsterAI/pull/2532)
- Miscellaneous visual/guide fixes: [#2598](https://github.com/netease-youdao/LobsterAI/pull/2598), [#2523](https://github.com/netease-youdao/LobsterAI/pull/2523), [#2571](https://github.com/netease-youdao/LobsterAI/pull/2571), [#2567](https://github.com/netease-youdao/LobsterAI/pull/2567)

### CI / Build
- **fix(ci): bound skill audit duration** — caps npm audit per Skill at 90 seconds and prevents unbounded CI hangs — [#2616](https://github.com/netease-youdao/LobsterAI/pull/2616)

## 4. Community Hot Topics

The only issue with visible discussion is:

- **[#1071 [OPEN/stale] SQLite storage layer has three data integrity/reliability defects**](https://github.com/netease-youdao/LobsterAI/issues/1071)  
  Created 2026-03-30, updated 2026-09-04, 1 comment, 0 reactions.  
  The report describes:
  1. `ON DELETE CASCADE` not working, causing orphan message accumulation.
  2. Non-atomic `save()` writes, risking corruption on crashes.
  3. `storeInitPromise` timing out and causing permanent storage failure.

Underlying need: users/auditors want guaranteed data integrity in the SQLite storage layer — foreign keys must be reliably enforced, writes need atomicity/fault tolerance, and initialization failure should not permanently brick the store.

No PRs in the recent activity set had comment counts above 0, so issue #1071 is the clearest community attention signal today.

## 5. Bugs & Stability

Ranked by severity:

1. **High — SQLite durability and integrity risks (open, unresolved)**  
   [#1071](https://github.com/netease-youdao/LobsterAI/issues/1071)  
   Three production-impacting flaws: broken cascade deletion, non-atomic `save()`, and permanent failure after `storeInitPromise` timeout. No fix PR is visible in the latest activity.

2. **Medium — Windows Unicode install path handling (fixed)**  
   [#2615](https://github.com/netease-youdao/LobsterAI/pull/2615)  
   Generated Windows browser MCP launcher could fail on non-ASCII paths. Closed by switching launcher to UTF-8 and improving internal browser error reporting.

3. **Medium — Unbounded CI audit duration (fixed)**  
   [#2616](https://github.com/netease-youdao/LobsterAI/pull/2616)  
   npm audit could hang; fixed by enforcing a 90-second cap per Skill while preserving stderr and non-blocking behavior.

4. **Low/UX — Model display temporarily disappears during login refresh (fixed)**  
   [#2612](https://github.com/netease-youdao/LobsterAI/pull/2612)  
   Now keeps the selected server model visible during authenticated metadata refresh without allowing stale models to run.

5. **Open browser fixes in review**  
   [#2617](https://github.com/netease-youdao/LobsterAI/pull/2617) — improves in-app login feedback dismissibility, credential settings behavior, and tab strip controls.

## 6. Feature Requests & Roadmap Signals

Several recently merged or open PRs reveal near-term roadmap direction:

- **Interactive in-app browser is a major focus**  
  Restored/merged in [#2602](https://github.com/netease-youdao/LobsterAI/pull/2602) and [#2574](https://github.com/netease-youdao/LobsterAI/pull/2574), with further UX improvements in open PR [#2617](https://github.com/netease-youdao/LobsterAI/pull/2617). Likely appears in the next `2026.9.x` release.

- **Login/auth friction before chat is being actively addressed**  
  [#2573](https://github.com/netease-youdao/LobsterAI/pull/2573) shows a welcome/login modal for unauthenticated chat submission; [#2596](https://github.com/netease-youdao/LobsterAI/pull/2596) adds analytics for those CTA clicks. Expect continued auth-flow refinement.

- **Subscription recovery and resource restoration is expanding**  
  [#2613](https://github.com/netease-youdao/LobsterAI/pull/2613) adds recovery entries and synchronization across Artifact, library, and site detail pages. This suggests deeper monetization/subscription lifecycle work in progress.

- **Update experience hardening**  
  [#2609](https://github.com/netease-youdao/LobsterAI/pull/2609) adds confirmation before installing and quitting — a small but meaningful reliability/QoL feature.

## 7. User Feedback Summary

Direct user comments are limited in the provided data, but signals include:

- **Data-integrity anxiety from storage-layer audit** — Issue [#1071](https://github.com/netease-youdao/LobsterAI/issues/1071) reads like upstream audit/bug-report feedback: users want crash-safe persistence, correct cascade cleanup, and recoverable initialization, not just UI features.
- **Login friction is a known pain point** — The unauthenticated chat prompt PR [#2573](https://github.com/netease-youdao/LobsterAI/pull/2573) and model-display fix [#2612](https://github.com/netease-youdao/LobsterAI/pull/2612) address confusion around auth state.
- **In-app browser demand is high** — Multiple browser-related PRs across two releases plus a follow-up open PR indicate active use cases for embedded web browsing.
- **Satisfaction appears decent on maintainer responsiveness** — Older PRs dating back to August 17 (e.g. [#2503](https://github.com/netease-youdao/LobsterAI/pull/2503), [#2501](https://github.com/netease-youdao/LobsterAI/pull/2501)) were merged this release cycle, showing backlog is being cleared even if not immediately.

## 8. Backlog Watch

- **[#1071 — SQLite storage layer defects (open since 2026-03-30)](https://github.com/netease-youdao/LobsterAI/issues/1071)**  
  This is the most important backlog item. It is labeled `[stale]`, but its content describes high-severity production data risks. It was updated 2026-09-04 but still has no linked fix PR. Maintainer attention is needed: either reproduce/fix or provide an explicit triage response.

- **[#2617 — fix(browser): improve in-app login and tab controls (open)](https://github.com/netease-youdao/LobsterAI/pull/2617)**  
  The only open PR visible in the top-20 list. It is a natural continuation of the newly shipped in-app browser feature and should be reviewed/merged for the next patch release.

- **Older PRs merged in this release cycle**  
  PRs like [#2503](https://github.com/netease-youdao/LobsterAI/pull/2503), [#2501](https://github.com/netease-youdao/LobsterAI/pull/2501), and [#2520](https://github.com/netease-youdao/LobsterAI/pull/2520) were open for around two weeks before closing on 2026-09-04. This suggests occasional release-cycle lag rather than maintainer neglect, but future batches would benefit from faster review windows.

</details>

<details>
<summary><strong>TinyClaw</strong> — <a href="https://github.com/TinyAGI/tinyagi">TinyAGI/tinyagi</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>Moltis</strong> — <a href="https://github.com/moltis-org/moltis">moltis-org/moltis</a></summary>

# Moltis Project Digest — 2026-09-05

## Today's Overview
Activity is low but focused: one open issue and one open PR were updated in the last 24 hours, with no new releases or merged PRs. The only issue is a user-facing enhancement request around persistent reasoning-effort defaults, signaling interest in session ergonomics. The only PR advances Moltis’s external-agent compatibility by adding direct AGY streaming support. No bug reports, crashes, or regressions were submitted today. Both open items are still awaiting maintainer triage or review.

## Releases
No releases were published in this period.

## Project Progress
- No PRs were merged or closed today.
- **PR #1258 — feat(external-agents): add direct AGY streaming** ([link](https://github.com/moltis-org/moltis/pull/1258))  
  This open PR by GTanger adds a first-class streaming transport for the official `agy` CLI. It reuses AGY’s existing Google OAuth session, avoiding the need for Gemini CLI or an API key, and translates AGY’s versioned `stream-json` output into Moltis text, reasoning, notice, tool, sub-agent, usage, and resumable-session streams. This would be a meaningful advancement for external agent interoperability, but it is not merged yet.

## Community Hot Topics
- **Issue #1259 — [Feature]: Configurable default reasoning/thinking level (persist across sessions)** ([link](https://github.com/moltis-org/moltis/issues/1259))  
  The only open issue updated in the window. It currently has zero comments or reactions, but it is the primary community signal today. The underlying need is straightforward: users want a persistent, configurable default reasoning/thinking level so they don’t have to manually set or re-select it in every session. 

- **PR #1258 — direct AGY streaming** ([link](https://github.com/moltis-org/moltis/pull/1258))  
  No maintainer/community comments are recorded yet. The underlying need is broader: users want Moltis to work seamlessly with more external agent CLIs/OAuth workflows rather than requiring API keys or vendor-specific setup.

## Bugs & Stability
No bugs, crashes, regressions, or stability issues were reported in the last 24 hours.

## Feature Requests & Roadmap Signals
- **Issue #1259** ([link](https://github.com/moltis-org/moltis/issues/1259)) requests a configurable default reasoning/thinking level that persists across sessions. This suggests the project is mature enough that users are now optimizing repeated workflows and consistency. 
- A likely roadmap direction is a configuration or profile-level setting for reasoning effort / thinking level, possibly complemented by the reasoning-stream support being introduced in PR #1258. Given that AGY streaming already parses reasoning output, a persistent reasoning-level control could plausibly land in a near-future version.

## User Feedback Summary
The only explicit user request today comes from Issue #1259. The pain point is configuration friction: users do not want to re-enter or re-configure their preferred reasoning/thinking level for each session. The request indicates real usage across multiple sessions and a desire for repeatable, consistent AI assistant behavior. There are no complaints or negative satisfaction signals in this window; the issue author confirmed they searched for existing enhancement requests first.

## Backlog Watch
No long-stale backlog items surfaced in this data snapshot.  
However, both open items need maintainer attention:
- **Issue #1259** is brand new and awaits triage: [moltis-org/moltis#1259](https://github.com/moltis-org/moltis/issues/1259)
- **PR #1258**, opened/updated within the last day, likely needs a review decision: [moltis-org/moltis#1258](https://github.com/moltis-org/moltis/pull/1258)

</details>

<details>
<summary><strong>CoPaw</strong> — <a href="https://github.com/agentscope-ai/CoPaw">agentscope-ai/CoPaw</a></summary>

# CoPaw Project Digest — 2026-09-05

*Note: Item links reference the `agentscope-ai/QwenPaw` repository as provided in the source data.*

---

## 1. Today's Overview

Activity is high but stabilizing: 23 issues and 26 PRs were touched in the last 24 hours, with no new releases cut. The project is between release trains — the community is anticipating **2.2.0 (QwenPaw Hub, multi-tenant)** while the maintainers are converging on 2.2.x stabilization work, including MCP whitelist enforcement ([#7504](https://github.com/agentscope-ai/QwenPaw/pull/7504)), loop-mode propagation fixes ([#7560](https://github.com/agentscope-ai/QwenPaw/pull/7560)), and workspace-scoped Skill preload ([#7183](https://github.com/agentscope-ai/QwenPaw/pull/7183)). The user base is heavily Chinese-speaking, and the dominant themes are **task-control reliability** (stop/409/loop-mode confusion), **LAN/local model server connectivity**, and **team/enterprise deployment** capabilities. A large batch of feature requests landed on 2026-09-04, suggesting a well-engaged but demanding power-user community. Overall health signal is positive: bugs are being closed at a healthy rate (8 issues closed, ~6 PRs merged/closed), but several high-severity runtime issues (task interruption semantics, channel consumer stalls) remain open without fix PRs.

---

## 2. Releases

**None.** No new releases were published in the last 24 hours. The most recent public markers are `2.2.0` (reported by users), `2.2.0-beta.7` (Windows Desktop), and `2.2.1b1` — indicating active beta/channel churn around the upcoming **2.2.0 Hub (multi-tenant)** release.

---

## 3. Project Progress

Six PRs were merged/closed in the last 24 hours (three visible in the top-20 sample):

- **[#7183 — feat(skills): add workspace-scoped preload configuration](https://github.com/agentscope-ai/QwenPaw/pull/7183)** *(closed)* — Implements the opt-in, workspace-scoped Skill `preload` policy (vs. `on_demand`) proposed in issue [#7182](https://github.com/agentscope-ai/QwenPaw/issues/7182). Reduces first-turn tool-call discovery overhead for workspaces built around a core Skill.
- **[#7504 — fix(mcp): enforce per-tool whitelist on the agent runtime path](https://github.com/agentscope-ai/QwenPaw/pull/7504)** *(merged/closed)* — Fixes [#7470](https://github.com/agentscope-ai/QwenPaw/issues/7470): `card.config.tools` was display-only after the 2.0 Driver rewrite, so disabled MCP tools remained callable by the agent. Now each MCP capability is tagged `enabled` from the whitelist.
- **[#7560 — fix(console): preserve selected loop mode query](https://github.com/agentscope-ai/QwenPaw/pull/7560)** *(closed)* — Resolves [#7552](https://github.com/agentscope-ai/QwenPaw/issues/7552) and [#7555](https://github.com/agentscope-ai/QwenPaw/issues/7555): the Goal/Mission selection from the composer menu now actually reaches the backend and is not silently reset to "Default."

Correspondingly, 8 issues were closed, including the validation/fix of the above ([#7470](https://github.com/agentscope-ai/QwenPaw/issues/7470), [#7182](https://github.com/agentscope-ai/QwenPaw/issues/7182), [#7552](https://github.com/agentscope-ai/QwenPaw/issues/7552), [#7555](https://github.com/agentscope-ai/QwenPaw/issues/7555)) plus bug cleanups [#6921](https://github.com/agentscope-ai/QwenPaw/issues/6921), [#7510](https://github.com/agentscope-ai/QwenPaw/issues/7510), [#7023](https://github.com/agentscope-ai/QwenPaw/issues/7023), and [#7567](https://github.com/agentscope-ai/QwenPaw/issues/7567).

---

## 4. Community Hot Topics

- **[#7318 — QwenPaw Hub multi-tenant edition coming in 2.2.0: what should we build next?](https://github.com/agentscope-ai/QwenPaw/issues/7318)** — *22 comments, 3 👍 (open question/discussion)*  
  The single most active thread. Rooted in long-standing requests ([#2324](https://github.com/agentscope-ai/QwenPaw/issues/2324) multi-user access, admin-managed skills). Underlying need: **teams want to run QwenPaw centrally with governance**, not as per-user installs.

- **[#7505 — LAN LLM server: frequent `client disconnect` → retries → timeout](https://github.com/agentscope-ai/QwenPaw/issues/7505)** — *12 comments (open)*  
  Users running LM Studio / local Qwen models on LAN report stream instability. Underlying need: **robust streaming/backoff for local & semi-reliable network model endpoints** — the backbone of the local-first use case.

- **[#6921 — Task silently stops after planning message ("Now 2.1, 3.1, 3.2…"), requires user to say "continue"](https://github.com/agentscope-ai/QwenPaw/issues/6921)** — *12 comments, closed as need-info*  
  The most emotionally charged bug class this week: the model plans next steps, stops without any visible notice, and the user must nudge it. The community interprets this as a **reliability defect in multi-step task execution**, not a model quirk — it repeatedly erodes trust in autonomous operation.

- **[#7559 — 409 error when sending a message while a task is running](https://github.com/agentscope-ai/QwenPaw/issues/7559)** — *4 comments (open, created 2026-09-04)*  
  User expectation: new messages during execution should queue, not error. Related to task-state UX confusion (see also #7567 and #7555, now closed).

Other notable threads: [#7182](https://github.com/agentscope-ai/QwenPaw/issues/7182) (4 comments, closed → implemented), [#7550](https://github.com/agentscope-ai/QwenPaw/issues/7550) (Docker image losing `codex` CLI after image updates), [#7534](https://github.com/agentscope-ai/QwenPaw/issues/7534) (Feishu session silently stuck), [#7541](https://github.com/agentscope-ai/QwenPaw/issues/7541) (Russian-language architectural critique: sessions should not be split by channel).

---

## 5. Bugs & Stability

Ranked by severity:

| Severity | Issue | Description | Fix status |
|---|---|---|---|
| **Critical** | [#7567](https://github.com/agentscope-ai/QwenPaw/issues/7567) | After pressing Stop, UI shows stopped but the task **keeps executing**; corrected follow-up command then hits 409. | Closed as "Close-and-review-later" — **not actually fixed**; needs real follow-up |
| **Critical** | [#7534](https://github.com/agentscope-ai/QwenPaw/issues/7534) | Feishu DM session: queue consumer permanently stuck after a high-priority card message; session is **silently unresponsive** and new messages cannot spawn a consumer. | No fix PR |
| **High** | [#7559](https://github.com/agentscope-ai/QwenPaw/issues/7559) | Sending a message/file mid-task triggers 409 instead of queueing. | No fix PR (partial relation to #7560) |
| **High** | [#7505](https://github.com/agentscope-ai/QwenPaw/issues/7505) | LAN LLM server stream fails with repeated client-disconnect retries and eventual timeout. | No fix PR |
| **High** | [#7549](https://github.com/agentscope-ai/QwenPaw/issues/7549) | Volcengine Ark Responses API rejects requests whose `input` ends with an assistant text turn (`400 MissingParameter: partial`) — breaks a major Chinese cloud provider. | No fix PR |
| **Medium** | [#7367](https://github.com/agentscope-ai/QwenPaw/issues/7367) | Startup takes 30–45 s even with console-only channel: `_load_builtin_channels()` unconditionally imports all 18 channel SDKs (`lark_oapi` alone ≈ 18.5 s). | No fix PR |
| **Medium** | [#7554](https://github.com/agentscope-ai/QwenPaw/issues/7554) | Windows: shell tool child processes inherit console stdin; `stdin`-reading commands hang the shared cmd console and Ctrl+C cannot kill them. | No fix PR |
| **Medium** | [#7548](https://github.com/agentscope-ai/QwenPaw/issues/7548) | Right-side navigation history for a conversation is lost after switching sessions/restarting, although content remains in `history.db`. | No fix PR |
| **Closed/fixed** | [#7470](https://github.com/agentscope-ai/QwenPaw/issues/7470) | MCP per-tool whitelist not enforced | Fixed by [#7504](https://github.com/agentscope-ai/QwenPaw/pull/7504) |
| **Closed/fixed** | [#7552](https://github.com/agentscope-ai/QwenPaw/issues/7552), [#7555](https://github.com/agentscope-ai/QwenPaw/issues/7555) | Loop mode not reaching / reverting in UI | Fixed by [#7560](https://github.com/agentscope-ai/QwenPaw/pull/7560) |
| **Closed** | [#6921](https://github.com/agentscope-ai/QwenPaw/issues/6921), [#7510](https://github.com/agentscope-ai/QwenPaw/issues/7510), [#7023](https://github.com/agentscope-ai/QwenPaw/issues/7023) | Task self-stop w/o notice; `/memory/status` 500; 60 s Playwright install on startup | Closed (need-info / resolved) |

**Open fix PRs in flight:** [#7401](https://github.com/agentscope-ai/QwenPaw/pull/7401) (Windows ACP agent stalls during workspace bootstrap), [#7211](https://github.com/agentscope-ai/QwenPaw/pull/7211) (prevent injected context persisting as user history), [#7497](https://github.com/agentscope-ai/QwenPaw/pull/7497) (deny sensitive paths even in governance `OFF` mode), [#7564](https://github.com/agentscope-ai/QwenPaw/pull/7564) (fire `workspace_created` hooks after `/daemon restart`).

---

## 6. Feature Requests & Roadmap Signals

Strong near-term roadmap signals:

- **Multi-tenant Hub (2.2.0)** — [#7318](https://github.com/agentscope-ai/QwenPaw/issues/7318): confirmed direction; discussion now collects priorities (admin-managed skills, multi-user access, governance).
- **Native mobile experience** — [#7378 (DO NOT MERGE draft)](https://github.com/agentscope-ai/QwenPaw/pull/7378): Expo/React Native client for Android/iOS backed by existing services. Being socialized, not yet mergeable.
- **Fleet/enterprise management** (issues by `laob9444`, who runs 9 agents):
  - [#7558](https://github.com/agentscope-ai/QwenPaw/issues/7558) — Pluggable relational storage (PostgreSQL/MySQL) for WAL-sensitive HA deploys (Swarm/K8s).
  - [#7556](https://github.com/agentscope-ai/QwenPaw/issues/7556) — Driver-level fallback chain for MCP drivers when policy denies.
  - [#7557](https://github.com/agentscope-ai/QwenPaw/issues/7557) — Version & dependency metadata for skills (`skill_pool`).
- **Cost optimization** — [#7568](https://github.com/agentscope-ai/QwenPaw/issues/7568): off-peak/batch scheduling to exploit provider off-hours discounts (e.g., DeepSeek 00:30–08:30). A novel signal: users now treat QwenPaw as a **scheduled batch job runner**.
- **Docker ergonomics** — [#7550](https://github.com/agentscope-ai/QwenPaw/issues/7550): preinstall/one-click `codex` CLI inside the official image since image updates wipe user-installed CLIs.

Feature PRs in review/progress: [#7486](https://github.com/agentscope-ai/QwenPaw/pull/7486) (Creator 1.1.2 — notification bus, multi-timeline A/B compare, T2V/I2V/S2V scheduling, media prompts), [#6960](https://github.com/agentscope-ai/QwenPaw/pull/6960) (PawPort import from Codex/Qoder), [#7538](https://github.com/agentscope-ai/QwenPaw/pull/7538) (unified runtime env management), [#7565](https://github.com/agentscope-ai/QwenPaw/pull/7565) (clean plugin unload + rollback-safe hot reload), [#7561](https://github.com/agentscope-ai/QwenPaw/pull/7561) (breaking refactor of memory lifecycle), [#7502](https://github.com/agentscope-ai/QwenPaw/pull/7502) (console sidebar/settings redesign).

**Prediction:** 2.2.0 will land with the Hub multi-tenant milestone; 2.2.1/2.3 is likely to absorb mobile preview, plugin hot-reload safety, and the skill versioning/storage backend requests if reviewers move on the open PRs.

---

## 7. User Feedback Summary

**Satisfaction drivers:** Users are enthusiastically building on QwenPaw — the Creator 1.1.x plugin series ([#7486](https://github.com/agentscope-ai/QwenPaw/pull/7486)) and multi-agent fleet deployments ("we hit this with 9 agents," [#7557](https://github.com/agentscope-ai/QwenPaw/issues/7557)) show production adoption beyond personal assistants.

**Core pain points (recurring):**

1. **"Fake stop" / state confusion** — the UI and backend disagree about whether a task is running ([#7567](https://github.com/agentscope-ai/QwenPaw/issues/7567)); the composer's loop-mode indicator lies about the active mode ([#7555](https://github.com/agentscope-ai/QwenPaw/issues/7555)); mid-task messages cause raw 409s ([#7559](https://github.com/agentscope-ai/QwenPaw/issues/7559)). Users want **queue semantics and honest state**, especially during long-running jobs.
2. **Premature task termination without notice** ([#6921](https://github.com/agentscope-ai/QwenPaw/issues/6921)) — described as the assistant "stopping after planning and waiting to be told continue." Frequently reported in Chinese; a **trust-eroding** behavior for the autonomous-agent promise.
3. **LAN/local model flakiness** ([#7505](https://github.com/agentscope-ai/QwenPaw/issues/7505)) — client-disconnect retry storms against LM Studio; the local-first audience needs graceful reconnects and idempotent retry.
4. **Slow startup from over-eager channel loading** ([#7367](https://github.com/agentscope-ai/QwenPaw/issues/7367)) — one user measured `lark_oapi` import alone at 18.5 s; console-only users pay the cost of all 18 channels.
5. **Channel-silent deadlocks** ([#7534](https://github.com/agentscope-ai/QwenPaw/issues/7534)) — Feishu sessions going unresponsive with no traceback is especially damaging for a chat-integrated assistant.
6. **Multi-channel session philosophy** ([#7541](https://github.com/agentscope-ai/QwenPaw/issues/7541), Russian) — one user argues sessions should be **channel-agnostic**; users expect one continuous conversation across web/desktop/Telegram, not per-channel silos. This aligns architecturally with the coming Hub.

---

## 8. Backlog Watch

Items needing maintainer attention:

- **[#6381 — perf(drivers): avoid blocking on stale capabilities](https://github.com/agentscope-ai/QwenPaw/pull/6381)** — Open since **2026-07-23** (~6 weeks), no merge. Addresses recurring request-time Driver discovery latency; directly affects perceived responsiveness.
- **[#6874 — feat(mcp): add configurable tool call timeout](https://github.com/agentscope-ai/QwenPaw/pull/6874)** — Open since 2026-08-10, "Under Review" for nearly a month. Relevant to the MCP hang/deny complaints in #7534/#7556.
- **[#6960 — feat(pawport): third-party import flow (Codex/Qoder)](https://github.com/agentscope-ai/QwenPaw/pull/6960)** — Open since 2026-08-13; large surface, no visible review progress.
- **[#7211 — fix(runtime): prevent injected context from persisting](https://github.com/agentscope-ai/QwenPaw/pull/7211)** — First-time contributor, labeled *ready-for-human-review*, open since 2026-08-21. Risk of contributor attrition if not reviewed soon.
- **[#7401 — fix(acp): prevent Windows ACP agent stalls during workspace bootstrap](https://github.com/agentscope-ai/QwenPaw/pull/7401)** — Under Review since 2026-08-29; Windows ACP hangs of "minutes" are severe for automation users.
- **[#7367 — Startup slowness from unconditional channel imports](https://github.com/agentscope-ai/QwenPaw/issues/7367)** — Open since 2026-08-28, only 2 comments, no maintainer response; clear performance win available.
- **[#7534 — Feishu session consumer stuck](https://github.com/agentscope-ai/QwenPaw/issues/7534)** and **[#7549 — Volcengine Ark rejection](https://github.com/agentscope-ai/QwenPaw/issues/7549)** — Senior-bug territory (channel deadlock, provider incompatibility) with **no linked fix PR**.
- **Process note:** PR [#7566](https://github.com/agentscope-ai/QwenPaw/pull/7566) was submitted with a placeholder body ("Describe what this PR does and why"); maintainers should request completion before review.

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

No activity in the last 24 hours.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw Project Digest — 2026-09-05

## 1. Today's Overview

In the 24‑hour window ending 2026‑09‑05, ZeroClaw recorded 34 updated issues (24 open, 10 closed) and 50 updated PRs (44 open, 6 merged/closed), with no new tagged release. Activity remains high, with a clear concentration on security hardening and v0.8.5 release plumbing: WhatsApp Web authorization was locked down, Bedrock cachePoint configuration was resolved, terminal‑response classification is moving in a fix PR, and a coordinated version bump to v0.8.5 was opened. The open list contains several large, heavily labeled PRs in review or blocked states, suggesting maintainer bandwidth is a limiting factor. Overall project health looks good, but risk‑high items still dominate the open queue.

## 2. Releases

No new releases in the last 24 hours. The closest near‑term release signal is PR #10632, opened today, which bumps the workspace from 0.8.4 → 0.8.5 across 23 crates and generated packaging surfaces, indicating a v0.8.5 release is imminent rather than already shipped.

## 3. Project Progress

Closed/merged PRs visible in the top‑20 set:

- [#10158](https://github.com/zeroclaw-labs/zeroclaw/pull/10158) — `feat(release): publish the workspace to crates.io`. Marks the coordinated 23‑crate set publishable (including `zerorelay`, `zeroclaw-relay-proto`, `zeroclaw-tls`) while keeping maintainer tools/fixtures private. This is a major distribution milestone.
- [#10153](https://github.com/zeroclaw-labs/zeroclaw/pull/10153) — `feat(whatsapp-web): port to whatsapp-rust 0.7.0`. Replaces six git‑pinned WhatsApp dependencies with crates.io releases, unblocking `zeroclaw-channels` publication.
- [#10587](https://github.com/zeroclaw-labs/zeroclaw/pull/10587) — dependency group bump (49 updates in the `rust-all` group).

Closed issues in the window, reflecting delivered fixes or accepted dispositions:

- WhatsApp Web security pair: [#9348](https://github.com/zeroclaw-labs/zeroclaw/issues/9348) (agent replying to every DM/group under `mode = business`) and [#9397](https://github.com/zeroclaw-labs/zeroclaw/issues/9397) (RFC: empty `allowed_groups` now means permit‑none).
- [#8720](https://github.com/zeroclaw-labs/zeroclaw/issues/8720) — Bedrock Nova 2 Lite `cachePoint` can be disabled via config.
- [#10357](https://github.com/zeroclaw-labs/zeroclaw/issues/10357) — tool execution error body no longer discarded.
- [#10223](https://github.com/zeroclaw-labs/zeroclaw/issues/10223) — ZeroCode Ctrl+C/reconnect input drop.
- [#10390](https://github.com/zeroclaw-labs/zeroclaw/issues/10390) — inactive Chat pane navigation block.
- [#8650](https://github.com/zeroclaw-labs/zeroclaw/issues/8650) — log path surfaced in diagnostics.
- [#9171](https://github.com/zeroclaw-labs/zeroclaw/issues/9171) — ZeroCode modifier semantics.
- [#9529](https://github.com/zeroclaw-labs/zeroclaw/issues/9529) — TodoWrite tracker close control.
- [#10571](https://github.com/zeroclaw-labs/zeroclaw/issues/10571) — Twitch section in Social Channels docs.

## 4. Community Hot Topics

Most‑commented issues (links to each; PR comment metadata was not populated for the PR list):

- [#9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487) — RFC: Runtime‑owned conversation sessions and transport adapters (32 comments). Still open and labeled `needs-maintainer-review`. Revision 5 replaced a prior vote snapshot, signaling an active but contested architecture decision.
- [#6909](https://github.com/zeroclaw-labs/zeroclaw/issues/6909) — RFC: Computer‑use / desktop screen interaction (16 comments). Accepted but not visibly implemented; maintainers clarified approval boundaries.
- [#9397](https://github.com/zeroclaw-labs/zeroclaw/issues/9397) — RFC: empty WhatsApp Web `allowed_groups` = permit‑none (14 comments). Closed after sponsor review.
- [#10050](https://github.com/zeroclaw-labs/zeroclaw/issues/10050) — RFC: verbatim channel send over gateway without an agent turn (13 comments). Accepted with follow‑up status.
- [#8720](https://github.com/zeroclaw-labs/zeroclaw/issues/8720) — Bedrock Nova 2 Lite caching error (10 comments). Closed with config‑based solution.
- [#10603](https://github.com/zeroclaw-labs/zeroclaw/issues/10603) — the only item with a 👍 reaction; provider behavior hitting OpenCode users.

Underlying needs: power users are pushing toward stricter security defaults (WhatsApp), more explicit provider/runtime control (sessions, verbatim sends, credential rotation), and safer private/self‑hosted deployments. The conversation around #9487 reflects growing complexity: the RFC process itself is being exercised heavily.

## 5. Bugs & Stability

Bugs in the 24‑hour active set, ranked by reported severity:

S1 — workflow blocked:

- [#10609](https://github.com/zeroclaw-labs/zeroclaw/issues/10609) — `zerocode` ignores its launch directory, always forcing the agent workspace as cwd. Filed 2026‑09‑04, in progress.
- [#10603](https://github.com/zeroclaw-labs/zeroclaw/issues/10603) — OpenCode relay requests never send `x-opencode-session`; breaks Go models and risks account flags. In progress.
- [#10593](https://github.com/zeroclaw-labs/zeroclaw/issues/10593) — `backup.schedule_cron` silently schedules nothing when no agent claims `__builtin_backup`.
- [#9421](https://github.com/zeroclaw-labs/zeroclaw/issues/9421) — incomplete terminal responses reported as successful. **Fix PR exists:** [#9447](https://github.com/zeroclaw-labs/zeroclaw/pull/9447) open, requires author action.
- [#9882](https://github.com/zeroclaw-labs/zeroclaw/issues/9882) — image markers bypass content validation on the `run_model_query` direct‑dispatch seam. Accepted as high risk.

S2 — degraded behavior:

- [#10626](https://github.com/zeroclaw-labs/zeroclaw/issues/10626) — TTS synthesizes Markdown and emoji verbatim.
- [#10625](https://github.com/zeroclaw-labs/zeroclaw/issues/10625) — literal `[media attachment]` placeholder delivered to users when a non‑vision model is in use.
- [#10594](https://github.com/zeroclaw-labs/zeroclaw/issues/10594) — cron silent non‑execution leaves no run history.

S3 — minor:

- [#10585](https://github.com/zeroclaw-labs/zeroclaw/issues/10585) — new log‑sink regression races migration tests under the default parallel runner.

Other open bugs updated in the same window include #10603's companion provider concerns and #10579 (broken docs links across the Reference section). Fix PRs in flight for adjacent security issues: [#10337](https://github.com/zeroclaw-labs/zeroclaw/pull/10337) (honor allowed roots for git tools) and [#10241](https://github.com/zeroclaw-labs/zeroclaw/pull/10241) (restore supervised shell approval routing).

## 6. Feature Requests & Roadmap Signals

- Release readiness: [#10632](https://github.com/zeroclaw-labs/zeroclaw/pull/10632) — version bump to v0.8.5 opened today, pinning the final signed translation snapshot. Expect v0.8.5 to follow shortly.
- [#10619](https://github.com/zeroclaw-labs/zeroclaw/issues/10619) — Anthropic prompt‑cache passthrough for OpenAI‑compatible providers (`cache_control` through translating gateways). P1, in progress; likely candidate for v0.8.5 if review completes, otherwise v0.9.0.
- [#10588](https://github.com/zeroclaw-labs/zeroclaw/issues/10588) — raise default `multimodal.max_image_size_mb` from 5 to 20 and document the ceiling.
- [#10580](https://github.com/zeroclaw-labs/zeroclaw/issues/10580) — repo‑wide docs internal‑link validation in CI.
- [#10579](https://github.com/zeroclaw-labs/zeroclaw/issues/10579) — missing Reference CLI/Config pages, still linked from ~39 places.
- Accepted RFCs awaiting implementation or follow‑up: #6909 (desktop computer‑use), #10050 (verbatim channel send).
- Process/roadmap tracking: [#10330](https://github.com/zeroclaw-labs/zeroclaw/issues/10330) (accepted RFC implementation index) and [#9459](https://github.com/zeroclaw-labs/zeroclaw/issues/9459) (v0.8.5 finite stabilization tracker, still open past its nominal Aug 30 line).

Prediction: the next release will carry the crates.io publishing milestone, WhatsApp security semantics, the 0.7.0 whatsapp‑rust port, and provider credential/terminal‑response fixes; larger RFCs (#9487, #10050 feature surface) are more likely targeted at v0.9.0.

## 7. User Feedback Summary

Pain points actively voiced in the window:

- Configuration that looks locked down but behaves wide open (#9348) — resolved by policy change in #9397; this was the strongest negative signal and is now closed.
- Provider‑specific friction: Bedrock Nova 2 Lite cachePoint errors (closed #8720), OpenCode session header missing (#10603), lack of Anthropic cache passthrough on compatible providers (#10619).
- Silent failures are a recurring complaint — cron jobs that don't run without records (#10593, #10594) and terminal‑response success reporting for incomplete replies (#9421).
- ZeroCode TUI annoyances: launch‑directory/cwd behavior (#10609), loss of Ctrl+C during reconnect (#10223, closed), navigation blocked by inactive Chat pane (#10390, closed), missing TodoWrite close button (#9529, closed), and modifier‑key semantics (#9171, closed). The number of ZeroCode UX bugs raised and fixed in recent weeks shows active dogfooding.
- New user‑visible content bugs surfaced today: TTS speaking Markdown/emoji (#10626) and internal media placeholders reaching end users (#10625).

Satisfaction signals: contributor‑authored fixes are merging (WhatsApp, config diagnostics, docs tasks), the maintainers resolved several S1 items, and trusted contributors (e.g., vrurg, IftekharUddin, Audacity88) continue to author large, security‑sensitive PRs.

## 8. Backlog Watch

Items needing maintainer attention or unblocking:

- [#6909](https://github.com/zeroclaw-labs/zeroclaw/issues/6909) — Accepted RFC for desktop computer‑use, open since 2026‑05‑25 (≈103 days) with no visible implementation; needs a follow‑up owner or explicit deferral note.
- [#9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487) — RFC with 32 comments and `needs-maintainer-review`; Revision 5 creates a new discussion window but the review label remains.
- [#9109](https://github.com/zeroclaw-labs/zeroclaw/pull/9109) — Native Hailo‑Ollama provider support, open since 2026‑07‑17 (≈50 days), labeled `do-not-merge`, `risk:high`, `size:XL`.
- [#9419](https://github.com/zeroclaw-labs/zeroclaw/pull/9419) — Credential rotation after rate limits, open since 2026‑07‑26, `do-not-merge` and `needs-maintainer-review`.
- [#9713](https://github.com/zeroclaw-labs/zeroclaw/pull/9713) — Token accounting on history‑trim events; `status:blocked` since early August despite principal‑contributor authorship.
- [#10241](https://github.com/zeroclaw-labs/zeroclaw/pull/10241) — Restore supervised shell approval routing across six channels; blocked since 2026‑08‑22, `risk:high`, touches security‑critical approval flows.
- [#9002](https://github.com/zeroclaw-labs/zeroclaw/pull/9002) — Gateway agent‑turn lifetime after viewer disconnect; open since 2026‑07‑11 with `needs-author-action`.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*