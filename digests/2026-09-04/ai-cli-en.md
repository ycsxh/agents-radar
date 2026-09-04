# AI CLI Tools Community Digest 2026-09-04

> Generated: 2026-09-04 04:02 UTC | Tools covered: 9

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

# Cross-Tool Comparison Report — AI CLI Developer Tools (2026-09-04)

*Scope: Claude Code, OpenAI Codex, Gemini CLI, GitHub Copilot CLI, Kimi Code CLI, OpenCode, Pi, Qwen Code, and DeepSeek TUI/Codewhale. All metrics are from each project's community digest for 2026-09-04.*

---

## 1. Ecosystem Overview

The AI CLI space is consolidating from "chat with a model in a terminal" into a broader agent platform market, where extensibility (hooks/plugins), context/cost transparency, and subagent orchestration are the new battlegrounds. The field is split between model-house CLIs (Claude Code, Codex, Gemini, Copilot, Qwen, Kimi) and model-neutral, community-driven clients (OpenCode, Pi, DeepSeek TUI/Codewhale), with the latter increasingly serving as integration testbeds for the former's workflows. Release cadence is high overall — Claude Code, Codex, Copilot, Qwen, and Gemini all shipped within the window — but meaningful feature parity gaps remain, most visibly in protocol completeness (ACP/MCP), Windows support, and honest agent-state reporting. Across all nine trackers, the same pain clusters recur daily: false "success" terminations, cache/context budget failures, guardrail bypasses or false positives, and Windows/WSL platform breakage.

---

## 2. Activity Comparison

| Tool | Issue Traffic (24h) | PR Traffic (24h) | Releases (24h) | Momentum Call |
|---|---|---|---|---|
| **Claude Code** | **Very High** — ~10 profiled hot issues; top bug at 76 comments/167 👍; new hooks RFC hit 64 comments in a day | **Moderate** — 5 PRs active, 2 addressing security-guard correctness | 1 stable (v2.1.260: `/diff`, cache-miss diagnostics) | Responsive, feature-rich |
| **OpenAI Codex** | **Very High** — 10 profiled; Windows/WSL and quota-reset clusters at 12–30 comments | **Very High** — 13 PRs incl. managed worktrees, attachment store, remote-exec trusted headers | 2 stable hotfixes + 3 alphas | Fastest daily shipper |
| **Gemini CLI** | **Very High** — 50 issues updated/24h; 10 profiled incl. 4 P1 reliability bugs | **Very High** — 41 updated/24h; 10 profiled (security, sandbox, SSE fixes) | 1 nightly | High velocity, P1 backlog unresolved |
| **GitHub Copilot CLI** | **Moderate** — 10 profiled; enterprise-policy and long-session themes dominate; highest ask at 13 👍 | **None** — 0 PRs updated | 2 stable (v1.0.83-4/-5) | Steady, governance-focused |
| **Kimi Code CLI** | **Low** — 7 issues updated, 6 closed; only 1 open (ACP auth gate) | **Low** — 1 PR (dynamic token budget) | None | Consolidating/housekeeping |
| **OpenCode** | **High** — 10 profiled; model reliability (Gemini edits, GLM cache) and workflow demand up to 39 comments | **High** — 10 profiled incl. desktop plugin manager, background shell, browser automation (~3 merged) | None | Strong community-driven iteration |
| **Pi** | **Moderate–High** — 10 profiled; streaming-perf and context-bloat reports; many closed/triaged | **Moderate–High** — ~11 profiled, mostly closed/merged (musl builds, exit-code fix, Meta provider) | None | Quality/architecture-focused |
| **Qwen Code** | **High** — 10 profiled incl. 2 P1 shell-security findings and content-leak regressions | **High** — 10 profiled incl. thinking-leak fixes, serve observability, CI timeout repair | 1 stable (v0.23.0) | Security-first, fast |
| **DeepSeek TUI / Codewhale** | **Low** — 4 issues updated (2 real ACP gaps, 1 spam) | **Low–Moderate** — 8 updated, 4 closed/merged (UX slices, theme consolidation) | None | Slower; refactor/branch-cleanup phase |

*Note: Where a digest states an aggregate count (Gemini 50 issues/41 PRs; Kimi 7 issues/1 PR; DeepSeek 4 issues/8 PRs; Copilot 0 PRs), it is used verbatim. Other rows are relative assessments from profiled items.*

---

## 3. Shared Feature Directions

Several requirements are appearing simultaneously across multiple communities:

- **Hooks, events, and first-class plugin APIs.** Claude Code's Function Hooks RFC (#91870, side-effect-tracked `$` params + `next()` continuation) is the highest-velocity new proposal. Kimi asks for lifecycle/notification hooks (#1313); Qwen wants pluggable output middleware for reasoning/content rewriting (#10872); OpenCode keeps requesting more plugin surfaces (`chat.message` blocking/cancellation #30434, before/after prompt hooks #47087) and is hardening them with permission assertions (#46530); Pi is building partial mid-conversation system-prompt updates for extensions (#8998); Copilot has agent-plugin discovery gaps (#4655, #4708). The ecosystem is converging on hooks as the primary long-term extensibility contract.

- **Truthful agent-state and cancellation semantics.** Gemini subagents report `GOAL`/`success` after hitting `MAX_TURNS` (#22323); Codex background turns are marked complete while the JSONL stream continues (#38972); Pi maps OOM-killed commands to exit code 0 (#8992/#8994) and has a model stuck in a self-aware dead loop (#9104); Qwen's todo plan freezes while subagents advance (#10953); Copilot silently drops allow-all mode after idle (#4696); Kimi ESC does not reliably abort subagents (#1315). Users are demanding trustworthy "why did the agent stop" semantics, not just faster generation.

- **Context/cache economics and cost governance.** Claude Code ships cache-miss diagnostics in `/cost` and faces complaints about chained `-p --resume` never hitting cache (#91971) and a ~900-line `CLAUDE.md` resent every tool round-trip (#91880). OpenCode sees GLM-5.1 prompt-cache reads randomly drop to 0 (#31348) and requests OpenRouter service-tier support (#28566). Pi struggles with context budgets that ignore output-token reservation (#8061) and duplicated thinking signatures bloating sessions to 4.5 MB (#9097). Copilot wants configurable Auto-mode model pools (#4218, 13 👍). Kimi is removing hardcoded `max_tokens` in favor of dynamic completion budgets (#2332).

- **Managed git worktree isolation.** Codex added experimental managed worktrees to `codex exec` (#42652); OpenCode has a `--worktree` flag request (#35471), worktree-per-task direction (#47202), and a bug where non-git projects use `/` as worktree (#24694); Claude Code has worktree-session memory inconsistencies (#81833). Per-session isolated checkouts are becoming a baseline safety pattern.

- **ACP/MCP protocol completeness.** DeepSeek TUI cannot expose session config or enumerate/resume sessions over ACP (#5863, #5864); Kimi's 1.17+ ACP auth gate blocks custom providers that don't use Kimi accounts (#2633); Qwen's ACP NDJSON channel tears down the whole conversation on queue saturation (#10162); Copilot hit an MCP handshake regression after modern `server/discover` (#4525) and has OAuth token-reuse failures (#4695); Gemini is hardening MCP OAuth with RFC 9207 issuer identification (#29117). Editor interop quality is now a first-class requirement.

- **Windows/WSL platform parity.** Every tool has a Windows-specific gap: Claude always-on-top desktop (#85891) and crash-orphaned Job Objects (#53247); Codex project operations fail after switching to WSL (#41290); Gemini Windows sandbox allows silent `git diff --output=` file truncation (#29184); Copilot PowerShell ConstrainedLanguage errors on every command (#4683); OpenCode WSL install syntax error (#29210) and desktop config-wipe loop (#35419); Qwen Windows IME low-contrast candidates (#9666); Pi CRLF edit-tool failures (#355). No vendor has solved this platform cluster yet.

---

## 4. Differentiation Analysis

- **Model-house CLIs differ on deployment philosophy.** Anthropic (**Claude Code**) is the most community-heavily invested, using an explicit RFC process for hooks and shipping UX features aimed at cost visibility (/cost, /diff). **OpenAI Codex** is engineering- and infrastructure-led — thread managers, attachment stores, remote-exec WebSockets with trusted headers — and treats the CLI as a runtime rather than a chat surface. **Gemini CLI** stands out for security hardening and nightly release discipline, with P1s concentrated on subagent semantics; it is also the most "model-fast" (3.8-flash promotion on day one). **Copilot CLI** differentiates on enterprise governance: remote-session policy, marketplace blocking, AppLocker/WDAC posture, Windows 11 taskbar presence — but showed zero PR activity, indicating a slower, more controlled development loop.

- **Independent clients differentiate on extensibility, neutrality, and workflow parity.** **OpenCode** is the most aggressive at absorbing Claude-Code-style paradigm changes (dynamic workflows #29059, teams #17994, background shell #47187) while serving a genuinely multi-model load (Gemini, GLM, DeepSeek). **Pi** is the most technically niche: focused on TUI rendering performance, streaming pathology, and extension internals — closer to a platform substrate than a product. **DeepSeek TUI/Codewhale** is the least differentiated here: most activity is branch cleanup, theme consolidation, and incomplete ACP bridging to other tools' ecosystems.

- **Target users and trust models diverge.** Copilot, Claude, and Codex target professional/enterprise developers with managed policies and cost controls; Qwen and Kimi serve users closely bound to their model families and local deployment (LM Studio, Token Plan ASR — its biggest local-model gaps); OpenCode, Pi, and DeepSeek target the BYO-provider, terminal-native power user who values neutrality over vendor integration. A notable pattern: vendor CLIs are all struggling with desktop-app state (Codex pets/recents, Claude topmost windows, Copilot taskbar), while terminal-native tools are building browser automation and remote-daemon capabilities instead.

- **Security posture differs meaningfully.** Qwen is actively patching content leaks (thinking tags, tool-result scaffolding) and bash allow-rule bypasses; Gemini is validating git args and checkpoint paths before execution; OpenCode is adding plugin permission assertions; Pi is fixing silent binary corruption and signal-kill misreporting; Codex and Claude are fighting false-positive guardrails that block legitimate work (#32597, #91650). The differing maturity of permission/sandbox models is visible in each tracker's bug mix.

---

## 5. Community Momentum & Maturity

- **Claude Code** shows the deepest community engagement — a 76-comment/167 👍 desktop bug, a 64-comment hooks RFC in under a day, and a GitLab request at 131 👍 since Nov 2025. It is the most mature in terms of issue-driven roadmap shaping, though several long-running Windows defects remain unsolved.
- **OpenAI Codex** has the fastest observed release cadence (2 stable hotfixes + 3 alphas + 13 PRs in 24h) and is iterating more aggressively than its tracker can absorb; issue clusters on WSL/rate-limit resets persist across months.
- **Gemini CLI** has the highest raw churn (50 issues and 41 PRs updated in a day) but velocity is not yet translating into resolution: subagent false-success, generalist hangs, and shell "Waiting input" wedges remain open P1s. High signal, high backlog.
- **OpenCode** is the most responsive independent project, merging several sizable features (desktop plugin manager, background shell, request-route classification) without a release — a sign of a healthy, continuous main branch. Demand for multi-agent teams and dynamic workflows is strong (22 👍/17 comments on #29059 alone).
- **Qwen Code** is iterating at parity with vendor tools and maintains unusually good security hygiene in its PR queue (two leak fixes closed same-day), plus recognizable investment in CI and review-bot automation.
- **Copilot CLI** ships reliably but processes little public PR contribution; its momentum is steady-state rather than explosive, with long-lived feature requests (#232, #4218) indicating a slower roadmap.
- **Pi** has a moderate but engaged niche community, with many issues closed/triaged quickly — an indication of proactive maintainers and a tightly-scoped architecture.
- **Kimi CLI** and **DeepSeek TUI/Codewhale** are the least active: Kimi spent the window closing early-March tickets; DeepSeek's PRs are dominated by re-lands and migrations. Both remain viable but are not currently pace-setters.

---

## 6. Trend Signals

1. **False-success reporting is eroding trust in agent autonomy.** The recurring "GOAL/success despite MAX_TURNS/dead-loop/stream-divergence" pattern across Gemini, Pi, Codex, and Qwen is the most serious reliability signal in this dataset. Expect run-state APIs, explicit termination-reason surfaces, and "interrupted ≠ completed" semantics to become purchasing criteria.

2. **Context/cache cost is becoming a product surface, not a background detail.** Claude's `/cost` diagnostics, Copilot's model-pool constraints, OpenCode's service-tier requests, Pi's budget-accounting bugs, and Kimi's dynamic completion budgets all point one direction: heavy CLI users now watch token economics in real time, and tools that make cache behavior visible (or eliminate waste) will win professional loyalty.

3. **Hooks/plugins are where the ecosystem "platform war" is being fought.** Claude is driving a formal hooks architecture; Copilot is building agent-plugin governance; OpenCode is shipping permission assertions and tool namespaces; Pi and Qwen are working on dynamic system-prompt and output-middleware extension points; Kimi and Gemini are filling in surrounding primitives (skills lifecycle, MCP OAuth hardening). For developers deciding which tool to build on, extensibility-surface maturity — not raw model quality — is becoming the lock-in variable.

4. **Worktree isolation is the new sandbox.** Codex shipping managed worktrees to `exec`, OpenCode's worktree-per-task direction, and Claude's worktree-related memory defects all indicate the industry converging on "one session = one isolated git worktree" as the standard mutation boundary. Sandboxing is moving from a permission prompt to an execution topology.

5. **Client-server architectures are displacing the monolith CLI.** Qwen's daemon/web-shell with browser-granted local directory access, Codex's remote-exec WebSockets and enrollment, Copilot's remote sessions, and the ACP/editor bridge work in DeepSeek and Kimi all show terminals evolving into remote-first agent backends. The CLI is becoming a thin client over a daemonized agent runtime.

6. **Windows is the unresolved greenfield.** Virtually every tool has a daily-driver Windows defect — desktop topmost windows, WSL state desync, IME contrast, AppLocker noise, sandbox path holes. No vendor has demonstrated systemic Windows/WSL reliability. For a tool vendor, this is still an open competitive opportunity; for an engineering team, it is a persistent cost center requiring compensating local workflows.

7. **Security precision determines production readiness.** One day's digests contain Bash allow-rule bypasses via environment assignments (Qwen), silent git file truncation (Gemini), non-enforced edit permissions (OpenCode), a false-positive block on defensive security work (Codex), and spurious permission prompts that erode guardrail trust (Claude). Teams adopting these tools for production repos should evaluate not just model accuracy but the permission system's false-positive/false-negative balance.

8. **Memory/hygiene is the next differentiator after context economics.** Claude's inconsistent `MEMORY.md` in worktrees, Gemini sending transcript content to extraction models before redaction, and Pi's session bloat from duplicated reasoning payloads show that "agent memory" is still pre-productized. The tool that makes memory deterministic, redacted-before-send, and observably bounded will have a durable advantage.

**Reference value for developers.** When choosing an AI CLI for daily work: prefer tools with explicit, inspectable termination semantics and cache/cost diagnostics; check whether the extension surface is first-class (hooks, ACP/MCP, plugin permissions) rather than bolted on; assume worktree isolation is a feature you should standardize on regardless of tool; and if your organization includes Windows users, budget compensating workflow investment until the platform cluster is fixed.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills Community Highlights Report — 2026-09-04

*Source: github.com/anthropics/skills (official Claude Code Skills repository). PR ranking below reflects the dataset's comment-activity order (top 8 of 50 PRs). All listed PRs are currently [OPEN] — none have reached "merged" status as of the snapshot.*

---

## 1. Top Skills Ranking

### #1 — #1298: skill-creator eval harness repair (`run_eval.py` always reports 0% recall)
- **Functionality:** Fixes the skill-creator evaluation pipeline (`run_eval.py`, and downstream `run_loop.py` / `improve_description.py`, which consume its signal) so skill descriptions stop being "optimized against noise." Changes install the eval artifact as a real skill, and address Windows stream reading, trigger detection, and parallel-worker behavior.
- **Discussion highlights:** Directly addresses issue #556 (12 comments, 7 👍, 10+ independent reproductions). The same Windows eval failure also spawned PRs #1099 and #1050 — three separate fix attempts from different authors, signaling real frustration with headless `claude -p` evaluation reliability.
- **Status:** Open | Last updated 2026-06-23 | [anthropics/skills PR #1298](https://github.com/anthropics/skills/pull/1298)

### #2 — #514: document-typography skill (typographic quality control)
- **Functionality:** Proposes a new skill that prevents orphan word wraps (1–6 words spilling to the next line), widow paragraphs (section headers stranded at page bottom), and numbering misalignment in AI-generated documents.
- **Discussion highlights:** Positions itself as a universal quality gate for *every* document Claude generates — a broad, horizontal use case that explains its high comment volume. One of the clearest "documentation quality" skill directions in the repo.
- **Status:** Open | Created 2026-03-04 | [anthropics/skills PR #514](https://github.com/anthropics/skills/pull/514)

### #3 — #1615: scnet-hpc skill (HPC cluster operations)
- **Functionality:** Adds a skill for operating SCNet HPC clusters via profile-based SSH and Slurm workflows: profile-specific connection, partition/memory/module/accelerator guidance, Slurm job generation, cluster discovery, and compute-node operations.
- **Discussion highlights:** Represents the community's push beyond general-purpose coding into specialized scientific-computing infrastructure. Recent activity (updated 2026-08-24) keeps it in active review.
- **Status:** Open | Created 2026-08-20 | [anthropics/skills PR #1615](https://github.com/anthropics/skills/pull/1615)

### #4 — #538: fix(pdf) — case-sensitive file references in SKILL.md
- **Functionality:** Small but high-impact repair: corrects 8 case mismatches (`REFERENCE.md` → `reference.md`, `FORMS.md` → `forms.md`) so the PDF skill works on case-sensitive filesystems.
- **Discussion highlights:** Notable because a one-line-class bug fix attracted a top-5 comment thread — evidence that cross-platform correctness of existing skills is a major community pain point.
- **Status:** Open | Last updated 2026-04-29 | [anthropics/skills PR #538](https://github.com/anthropics/skills/pull/538)

### #5 — #486: ODT skill (OpenDocument text creation, template filling, ODT→HTML)
- **Functionality:** Adds an `odt` skill for creating, filling, reading, and converting OpenDocument files (.odt, .ods), with triggers for "ODT", "ODS", "ODF", "OpenDocument", "LibreOffice document", and open-format/ISO-standard requests.
- **Discussion highlights:** A concrete document-format gap alongside existing PDF/DOCX skills. The long review window (created 2026-03-01, updated through 2026-04-14) suggests scope discussions around format coverage.
- **Status:** Open | Last updated 2026-04-14 | [anthropics/skills PR #486](https://github.com/anthropics/skills/pull/486)

### #6 — #210: frontend-design skill clarity and actionability revision
- **Functionality:** Revises the existing frontend-design skill so every instruction is executable within a single conversation, removing vague guidance in favor of specific, behavior-steering instructions.
- **Discussion highlights:** A "skill-quality" rather than "new capability" PR. Its high comment ranking signals community demand for *better-written* skills, not just more skills.
- **Status:** Open | Last updated 2026-03-07 | [anthropics/skills PR #210](https://github.com/anthropics/skills/pull/210)

### #7 — #83: skill-quality-analyzer and skill-security-analyzer (meta-skills)
- **Functionality:** Adds two meta-skills to the example marketplace: a **skill-quality-analyzer** (evaluates structure, documentation, examples, and resources across five weighted dimensions) and a **skill-security-analyzer** (security review of skill content).
- **Discussion highlights:** One of the earliest proposals still drawing attention (created 2025-11-06). Directly anticipates the trust-boundary concerns raised in issue #492.
- **Status:** Open | Last updated 2026-01-07 | [anthropics/skills PR #83](https://github.com/anthropics/skills/pull/83)

### #8 — #541: fix(docx) — prevent tracked-change `w:id` collisions with existing bookmarks
- **Functionality:** Fixes document corruption caused when DOCX skills add tracked changes to files that already contain bookmarks. Root cause: `w:id` is a shared ID space across bookmarks, tracked changes, comments, and move ranges, and SKILL.md examples used hardcoded low IDs that collide.
- **Discussion highlights:** A second high-comment infrastructure fix (same author as #538), reinforcing that file-format skill reliability — not just new-format coverage — is a top community concern.
- **Status:** Open | Last updated 2026-04-16 | [anthropics/skills PR #541](https://github.com/anthropics/skills/pull/541)

---

## 2. Community Demand Trends

### 🔐 Security, trust boundaries, and enterprise-safe distribution (highest signal)
Issue **#492** (43 comments — the most-commented issue by a wide margin) warns that community skills distributed under the `anthropic/` namespace impersonate official Anthropic skills, enabling trust-boundary abuse when users grant elevated permissions. Issue **#1175** adds a related concern about writing SharePoint Online access-control logic into SKILL.md. The community is asking for a distribution model that preserves the official/community trust boundary.

### 🔁 Skill lifecycle management for teams (sharing, duplication, storage reliability)
Issue **#228** (16 comments, 8 👍) requests org-wide skill sharing in Claude.ai — currently users must hand-transfer .skill files and manually upload them. Issue **#189** reports identical duplicate skills when both `document-skills` and `example-skills` plugins are installed, bloating the context window. Issue **#62** (10 comments) reports skills silently disappearing after file renames. Together: the community wants sync, deduplication, and storage reliability.

### 🧪 Honest, reproducible skill evaluation
Issue **#556** (12 comments, 7 👍 — the highest 👍 count in the issue set) documents that `run_eval.py` never triggers a skill in headless `claude -p` mode, reporting `precision=100% recall=0%` across all queries. Issue **#1390** shows the same failure mode in mcp-builder's evaluation harness, and issue **#202** argues skill-creator itself violates best practices. There is heavy demand for trustworthy evaluation tooling before writing new skills.

### 🧠 Agent memory, governance, and reasoning quality gates
Issue **#1329** (9 comments) proposes a **compact-memory** skill (symbolic notation for compact agent state), issue **#1385** proposes a three-gate reasoning quality pipeline (calibration → adversarial review → delivery verification), and issue **#412** proposed **agent-governance** safety patterns. A distinct cluster around long-running agents: state efficiency, self-audit, and safety.

### ⚡ Context-window efficiency
Issue **#1487** (4 comments) reports the `claude-api` skill eagerly injecting ~156k tokens in a single tool call, exhausting the context window. Issue **#1175** similarly couples SharePoint security with context-window limits. Skills that are lean, lazy-loading, or reference-based are an emerging requirement — not a nice-to-have.

### 🔌 Platform reach and protocol questions (persistent background demand)
Issue **#29** asks how to use Skills with AWS Bedrock; issue **#16** proposes exposing Skills as MCPs. Older issues with steady comment counts, indicating integration-surface questions remain unresolved.

---

## 3. High-Potential Pending Skills

*Because the top comment-ranked PR list skews toward bug fixes, these open submissions are the most likely *new-skill* landings based on community value, author momentum, and recency of activity.*

### #723 — testing-patterns skill (test generation & strategy)
Comprehensive testing skill covering the Testing Trophy model, unit-test patterns, React Testing Library, and what *not* to test. High general value; stalled since 2026-04-21 but one of the most broadly applicable pending skills. | [PR #723](https://github.com/anthropics/skills/pull/723)

### #568 — ServiceNow platform skill (enterprise workflow automation)
Broad ServiceNow assistant covering ITSM, ITOM, ITAM/SAM, FSM, HRSD, CSM, SPM, Vulnerability Response, SecOps, CSDM, and IntegrationHub. Actively updated as recently as 2026-08-12, signaling sustained maintainer interest. | [PR #568](https://github.com/anthropics/skills/pull/568)

### #1628 — Hivemind (zero-cost multi-agent orchestration)
Lets Claude Code delegate mechanical work to headless opencode workers on free models while Claude remains the sole planner/reviewer/merger — a direct response to the "expensive context is the scarce resource" problem. Recent activity (2026-08-24). | [PR #1628](https://github.com/anthropics/skills/pull/1628)

### #1607 — claude-api skill: mark retired model IDs
Updates `skills/claude-api/shared/models.md` to correctly classify four retired models (fixes #1603). The most recently touched PR in the set (2026-09-01), so it is the most likely candidate to land next. | [PR #1607](https://github.com/anthropics/skills/pull/1607)

### #1367 — self-audit skill (verification + reasoning quality gate, v1.3.0)
Audits AI output before delivery: mechanical file verification first, then a four-dimension reasoning audit in damage-severity order. Declared universal across projects, tech stacks, and models; pairs with issue #1385. | [PR #1367](https://github.com/anthropics/skills/pull/1367)

### #525 — pyxel skill (retro game development)
Skill wrapping pyxel-mcp for the Pyxel retro game engine (write → run_and_capture → inspect → iterate). Authored by Kitao, who maintains Pyxel itself — strong author credibility for a niche creative skill. | [PR #525](https://github.com/anthropics/skills/pull/525)

---

## 4. Skills Ecosystem Insight

Across both PRs and Issues, the community's most concentrated demand is not for any single functional skill but for **trust and reliability infrastructure around Skills itself** — secure distribution under the official namespace, reproducible headless evaluation, and context-window-efficient packaging — with document-format/quality skills (typography, ODT, DOCX/PDF correctness) as the fastest-growing functional cluster.

---

# Claude Code Community Digest — 2026-09-04

## Today's Highlights

Claude Code v2.1.260 ships a fullscreen diff panel (`/diff`) and prompt-cache-miss diagnostics in `/cost` — a direct response to the community's ongoing cost/context-window complaints. Meanwhile, the tracker's energy is concentrated in two places: a fast-rising Function Hooks extensibility RFC (#91870, 64 comments in under a day) and the long-running Windows "always-on-top" desktop bug (#85891, 76 comments, 167 👍). The rest of the week's signal is dominated by Windows platform grievances, auto-memory inconsistencies, and caching failures.

## Releases

### v2.1.260
- **Diff panel in fullscreen mode**: Opens beside the conversation and shows uncommitted changes live as Claude edits; toggle with `/diff`
- **Prompt-cache miss diagnostics**: `/cost` now suggests likely causes for cache misses (e.g., tool definitions or system prompt changed, idle past the TTL)

Note: Multiple regression reports in the tracker flag 2.1.257–2.1.259 specifically (e.g., #91650), so this release arrives amid active Windows/tooling cleanup.

## Hot Issues

- **[#85891 — Claude Desktop (Windows 11) stays always-on-top, no setting to disable](https://github.com/anthropics/claude-code/issues/85891)** — 76 comments, 167 👍. The most-engaged open bug; duplicates (#88093) keep arriving, and reporters note it's the Windows counterpart of #66516. Broad demand for a topmost-window toggle.
- **[#91870 — Function Hooks: "make plugins 10x more powerful"](https://github.com/anthropics/claude-code/issues/91870)** — 64 comments in ~24 hours. Proposes deep, safe composability via side-effect-tracked `$` parameters and an Express/Koa-style `next` continuation model. Filed explicitly for community feedback; the velocity signals strong appetite for a first-class hooks architecture.
- **[#53247 — Windows launch failure: orphaned Silo/Job Object after crash, only logoff/reboot recovers](https://github.com/anthropics/claude-code/issues/53247)** — 55 comments; open since April. HRESULT 0x80070020 with AppModel-Runtime EventID 215/208. Anecdotal reports indicate multiple affected users with no software recovery path.
- **[#12346 — GitLab Integration (repository connection, MRs, mobile access)](https://github.com/anthropics/claude-code/issues/12346)** — 52 comments, 131 👍 since Nov 2025. Persistent, high-demand ecosystem gap next to the existing GitHub integration.
- **[#91650 — Bash cd-compound-read guard prompts on absolute cd targets whenever a Read() deny rule exists (Windows Git Bash)](https://github.com/anthropics/claude-code/issues/91650)** — 9 comments, 52 👍. Regression on 2.1.257–2.1.259; deny rules for files like `.env` cause spurious prompts on plain `cd`. Security-guard false positive with high community agreement.
- **[#81833 — Auto-memory inconsistently loaded in git-worktree sessions](https://github.com/anthropics/claude-code/issues/81833)** — 12 comments. Same repo, same day: some worktree sessions get the origin project's full `MEMORY.md`, others none. Memory reliability is becoming a top trust issue.
- **[#38698 — Per-agent model provider routing](https://github.com/anthropics/claude-code/issues/38698)** — 11 comments, 43 👍. Wants local Ollama for subagents while the orchestrator stays on Anthropic; current `ANTHROPIC_BASE_URL` is session-wide and `model` only accepts sonnet/opus/haiku.
- **[#91971 — Prompt cache never hits across chained `-p --resume` calls](https://github.com/anthropics/claude-code/issues/91971)** — New today. Static prefixes cache correctly, but per-turn conversation content is never promoted, even at minimum config. Directly relevant to the new `/cost` diagnostics.
- **[#78569 — Auto-memory pointer edits deterministically rejected by read-before-write gate](https://github.com/anthropics/claude-code/issues/78569)** — 6 comments. The system prompt directs an immediate `MEMORY.md` pointer edit, but the guardrail blocks it every time — an instruction-vs-safety deadlock.
- **[#91880 — Excessive context re-sending: ~900-line CLAUDE.md re-sent every tool round-trip](https://github.com/anthropics/claude-code/issues/91880)** — 3 comments. Also covers "file changed on disk" reminders re-printing 150+ lines after python/sed edits. Cost and context-window waste that the new cache-miss tooling should help expose.

## Key PR Progress

Only 5 PRs were active in the last 24h; all are listed below.

- **[#87079 — fix(security-guidance): make `**` glob patterns match zero-depth paths](https://github.com/anthropics/claude-code/pull/87079)** — High-impact security fix. Delegation to `fnmatch` means `**/*.ts` requires a literal `/`, silently excluding top-level files from `security-patterns.json` rules — a silent non-coverage failure mode exactly where coverage matters most.
- **[#89404 — validate-agent.sh: don't abort at the first warning and stop false-flagging valid agents](https://github.com/anthropics/claude-code/pull/89404)** — Fixes public issue #83803. Three `set -euo pipefail` interactions (including `((count++))` returning exit status 1) caused plugin-dev's validator to reject its own agent files.
- **[#66416 — fix(plugin-dev): validator scripts abort on first finding due to `set -e`](https://github.com/anthropics/claude-code/pull/66416)** — Overlapping fix for the same class of bug across `validate-agent.sh`, `hook-linter.sh`, and `validate-hook-schema.sh`. Both #66416 and #89404 remain open; maintainers should consolidate.
- **[#79150 — docs: align code-review README with the current validation-based command](https://github.com/anthropics/claude-code/pull/79150)** — The README documents a git blame/history agent and a 0–100 confidence threshold that no longer exist, and tells users to edit a config line that isn't in the codebase.
- **[#91894 — Update /frontend-design SKILL.md](https://github.com/anthropics/claude-code/pull/91894)** — Closed content update to the frontend-design skill.

## Feature Request Trends

- **First-class hooks/extensibility** (#91870): Side-effect-tracked, safely composable function hooks — the highest-velocity new proposal this week.
- **GitLab integration** (#12346): Repo connection, merge requests, and mobile access; 131 👍 and climbing since Nov 2025.
- **Per-agent model/provider routing** (#38698, plus closed #73654 requesting sub-agent model exposure in the status line): Users want heterogeneous model topologies — local models for subagents, frontier models for orchestration.
- **Multi-profile accounts** (#91770): Separate history/memory/config within one account, with profile-only sign-in for shared client machines.
- **Desktop window controls** (#85891/#88093): At minimum, a setting to disable always-on-top behavior on Windows.

## Developer Pain Points

- **Windows desktop instability cluster**: Always-on-top windows (#85891, #88093), crash-orphaned Job Objects requiring reboot (#53247), an auto-updater that blocks the main event loop (#88072), and fully masked computer-use screenshots even for granted apps (#88937, #91079 — two independent reports).
- **Prompt-cache and context economics**: Chained `-p --resume` sessions never accumulate cache (#91971); multi-hundred-line CLAUDE.md files re-sent on every tool round-trip (#91880). Users are watching token costs closely.
- **Auto-memory unreliability**: Worktree sessions inconsistently receive memory (#81833), and the safety gate can block the very edits the system prompt demands (#78569).
- **Over-aggressive permission guardrails**: Spurious cd prompts under deny rules on Windows (#91650), permission/credential prompts rendered in invisible windows so they lapse as "denied" (#91969, #83959), and a C compiler error being classified as a security threat (#91977). False positives erode trust in the permission model.
- **macOS compatibility regressions**: dyld `_DNSServiceGetAddrInfoEx` symbol crash on macOS 12.7.6 (#91550) and side chat (Cmd+;) going silent after the first reply (#91975).
- **Silent failures**: VS Code chat links to binary files do nothing with no error (#81227); `SendUserFile` claims delivery while rendering nothing usable in the terminal (#88889).

---

*Data source: [github.com/anthropics/claude-code](https://github.com/anthropics/claude-code), retrieved 2026-09-04.*

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-09-04

## Today's Highlights

The stable 0.153 line shipped two hotfix releases (0.153.1, 0.153.2) centered on making GPT-6-Astra configurable via API and correcting its Fast-tier description, while 0.154.0 entered alpha. On the engineering side, the team merged a large batch of TUI hardening fixes and added an experimental managed-worktrees feature to `codex exec`. Community attention remains concentrated on Windows/WSL project-state bugs and rate-limit reset failures, with several high-traffic issues still open.

## Releases

- **[rust-v0.153.1](https://github.com/openai/codex/releases/tag/rust-v0.153.1)** — Added support for configuring GPT-6-Astra through the API without changing the default model or showing it in the model picker ([PR #42605](https://github.com/openai/codex/pull/42605)). [Full changelog](https://github.com/openai/codex/compare/rust-v0.153.0...rust-v0.153.1)
- **[rust-v0.153.2](https://github.com/openai/codex/releases/tag/rust-v0.153.2)** — Corrected the GPT-6-Astra Fast tier description to “2x speed, increased usage” (displayed text only; no request behavior change) ([PR #42632](https://github.com/openai/codex/pull/42632)). [Full changelog](https://github.com/openai/codex/compare/rust-v0.153.1...rust-v0.153.2)
- **rust-v0.154.0-alpha.1/.2/.3** — Pre-release alphas published without changelog notes.

## Hot Issues

1. **[#41290 — Project creation/removal fails after switching Agent Environment to WSL](https://github.com/openai/codex/issues/41290)** (30 comments, 21 👍) — Windows desktop users on the 26.825 build hit project-operation failures the moment the agent environment is switched to WSL. One of the most-upvoted active Windows regressions.
2. **[#25779 — Desktop meta-bug: unbounded session/turn state causes freezes, context bloat, lost active-turn control](https://github.com/openai/codex/issues/25779)** (17 comments) — A long-running tracker aggregating desktop app freezes and session-state degradation; still unresolved after three months.
3. **[#39989 — Windows desktop keeps deleted ChatGPT conversations in Recents](https://github.com/openai/codex/issues/39989)** (16 comments) — Already-deleted conversations persist in the sidebar even after a full restart of the packaged app.
4. **[#31601 — Usage limit reset failed and quota is gone](https://github.com/openai/codex/issues/31601)** (13 comments, 5 👍) — A Pro CLI user reports a failed reset that consumed reset inventory and left the quota exhausted; part of a wider reset-credits reliability pattern.
5. **[#39121 — Historical local projects disappear after update while tasks remain intact](https://github.com/openai/codex/issues/39121)** (12 comments) — Projects vanish from the Windows desktop UI after updates across four app versions, although associated tasks survive.
6. **[#31995 — Long conversations show only recent turns after update](https://github.com/openai/codex/issues/31995)** (7 comments) — Full rollout files exist locally and app-server paginates correctly, but the conversation UI only renders recent turns.
7. **[#32597 — Codex Security validation blocked on defensive review of personal repository](https://github.com/openai/codex/issues/32597)** (6 comments, 3 👍) — A suspected false-positive cybersecurity block prevents legitimate defensive work on a personal repo; raises guardrail-precision concerns.
8. **[#38972 — Background turn reported completed/interrupted while JSONL continues without final_answer/task_complete](https://github.com/openai/codex/issues/38972)** (6 comments) — App-server completion state diverges from the underlying rollout data for threads started via `thread/start` + `turn/start`.
9. **[#37928 — “Usage limit resets” fails to load, hiding banked reset inventory](https://github.com/openai/codex/issues/37928)** (4 comments, 12 👍) — High community agreement despite low comment count: the reset-credits UI is unreliable, and users cannot see how many resets they have banked.
10. **[Windows desktop pet click-through and drag breakage](https://github.com/openai/codex/issues/41535)** (with related [#42190](https://github.com/openai/codex/issues/42190), [#42061](https://github.com/openai/codex/issues/42061)) — After moving or resizing the pet, its interactive hit area detaches from the visible position and clicks pass through to underlying windows. Three separate reports in under a week indicate a reproducible overlay bug cluster.

## Key PR Progress

1. **[PR #42652 — Add managed worktrees to `codex exec`](https://github.com/openai/codex/pull/42652)** — New experimental `worktrees` feature and shared `--worktree` flag; each enabled session runs in a managed Git worktree bound as the working directory.
2. **[PR #42668 — Cancel remote control enrollment on stdio shutdown](https://github.com/openai/codex/pull/42668)** — Prevents a pending remote-control enrollment from blocking app-server exit after stdio EOF, which was leaking thread-writer resources.
3. **[PR #42634 — Add an injectable attachment store to ThreadManager](https://github.com/openai/codex/pull/42634)** — New `codex-attachment-store` crate with storage-neutral attachment metadata and an async persistence interface; inline implementation preserves attachment bytes as media-typed blobs.
4. **[PR #42606 — Support trusted headers for remote exec WebSockets](https://github.com/openai/codex/pull/42606)** — Adds `RemoteEnvironmentOptions` so embedding hosts can attach trusted HTTP headers to remote exec-server handshakes, preserved across session reconnects with redaction.
5. **[PR #42667 — Tailor TUI cyber refusal notices to Daybreak eligibility](https://github.com/openai/codex/pull/42667)** — Prefetches ChatGPT account eligibility, shows an activation link when Daybreak is available, and gives Astra-specific messaging for unsupported models.
6. **[PR #42641 — Restore the inline TUI after full-screen overlays](https://github.com/openai/codex/pull/42641)** — Invalidates the restored inline viewport after leaving alternate-screen overlays to avoid stale cells and scrolled-out history.
7. **[PR #42640 — Harden TUI parsing of assistant markup](https://github.com/openai/codex/pull/42640)** — Shared parser for assistant directives handling quoted attributes, embedded braces, escaped quotes, and malformed input consistently across Git-action receipts and code comments.
8. **[PR #42650 — Render assistant file citations as local links](https://github.com/openai/codex/pull/42650)** — Converts `codex-file-citation` directives into local-file links, preserving Unicode, Windows separators, and location suffixes.
9. **[PR #42639 — Warn when saved model defaults are overridden](https://github.com/openai/codex/pull/42639)** — Surfaces a warning when higher-priority config layers silently override saved model, reasoning-effort, or service-tier defaults.
10. **[PR #42623 — Bound Noise handshakes by the exec-server initialization timeout](https://github.com/openai/codex/pull/42623)** — Waits for the authenticated Noise handshake before sending JSON-RPC `initialize` and shares the configured init timeout across both phases, preventing unbounded hangs.

Also notable: **[PR #42631](https://github.com/openai/codex/pull/42631)** adds GStreamer runtime initialization to the voice host, **[PR #42619](https://github.com/openai/codex/pull/42619)** adds GPT-6-Astra to Amazon Bedrock catalogs, and **[PR #42624](https://github.com/openai/codex/pull/42624)** centralizes prompt image detail modes.

## Feature Request Trends

Explicit feature requests are rare in this window, but the signals from issues and merged PRs point to several recurring directions:

- **Commit attribution / Aider parity** — The long-dormant [request #938](https://github.com/openai/codex/issues/938) to add `(codex)` / `codex <model>` co-author trailers to commit messages resurfaced with 14 👍. Users clearly want conventional Git attribution for agent-authored work.
- **Managed execution isolation** — The experimental worktree support in [PR #42652](https://github.com/openai/codex/pull/42652) aligns with repeated community calls for safer per-session checkouts rather than mutating the user’s working tree.
- **Reliable quota lifecycle** — Multiple reset failures ([#31601](https://github.com/openai/codex/issues/31601), [#37928](https://github.com/openai/codex/issues/37928), [#35116](https://github.com/openai/codex/issues/35116)) suggest users want self-serve, dependable usage-reset management with visible banked inventory.
- **Conversation/session lifecycle controls** — Ghost conversations, truncation of long histories, and disappearing projects ([#39989](https://github.com/openai/codex/issues/39989), [#31995](https://github.com/openai/codex/issues/31995), [#39121](https://github.com/openai/codex/issues/39121)) point to demand for explicit archive/restore and stronger persistence guarantees.

## Developer Pain Points

- **Rate-limit reset flow is fragile.** Users report lost quota after failed resets, blocked reset buttons due to 429s, and hidden banked-reset inventory ([#31601](https://github.com/openai/codex/issues/31601), [#37934](https://github.com/openai/codex/issues/37934), [#37928](https://github.com/openai/codex/issues/37928), [#35116](https://github.com/openai/codex/issues/35116)). Several mention hesitating to upgrade to Pro because of reset inconsistencies.
- **Windows/WSL state management breaks down.** Switching the agent environment to WSL breaks project creation/removal ([#41290](https://github.com/openai/codex/issues/41290)); historical projects disappear after updates ([#39121](https://github.com/openai/codex/issues/39121)); Schannel certificate-validation failures trap users in a “Reconnecting…” loop ([#41275](https://github.com/openai/codex/issues/41275)).
- **Session/history desynchronization.** Deleted conversations stick around ([#39989](https://github.com/openai/codex/issues/39989), [#41987](https://github.com/openai/codex/issues/41987)), long threads render truncated despite intact JSONL ([#31995](https://github.com/openai/codex/issues/31995)), and background turns can be reported complete while still streaming ([#38972](https://github.com/openai/codex/issues/38972)).
- **Desktop-pet overlay hit-testing regressions.** Multiple Windows reports of click-through pets and hit areas detached from visuals after move/resize ([#41535](https://github.com/openai/codex/issues/41535), [#42190](https://github.com/openai/codex/issues/42190), [#42061](https://github.com/openai/codex/issues/42061)).
- **CLI subagent configuration inconsistencies.** `codex exec --ephemeral` fails to spawn custom subagents with “no thread with id” ([#41474](https://github.com/openai/codex/issues/41474)), and custom-agent `service_tier = "fast"` is ignored when the parent runs Standard — a regression from 0.152.1 ([#42612](https://github.com/openai/codex/issues/42612)).
- **Computer-Use accessibility scans crash third-party Qt apps.** Enabling macOS Accessibility for Computer Use triggers SIGSEGV-type crashes in Qt Creator 20 and NVIDIA Nsight Systems ([#41374](https://github.com/openai/codex/issues/41374), [#42666](https://github.com/openai/codex/issues/42666)); Windows builds also fail to launch UI when `cua_node` staging cannot copy `node_repl.exe` ([#42501](https://github.com/openai/codex/issues/42501)).

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-09-04

## 1. Today's Highlights
Agent reliability remains the dominant theme: maintainers re-triaged long-running P1 issues where subagents report `GOAL` success after hitting `MAX_TURNS` and where shell commands hang at "Waiting input." On the security front, the latest nightly hardens the MCP OAuth flow with RFC 9207 issuer identification, while open PRs close Windows sandbox bypasses and a checkpoint path-traversal hole. Community demand is also clearly focused on getting the newest flash models into the picker — the top-voted open issue this cycle.

## 2. Releases
- **v0.60.0-nightly.20260904.g87a9c71d5** — New nightly. Enforces RFC 9207 issuer identification in the MCP OAuth flow ([PR #29117](https://github.com/google-gemini/gemini-cli/pull/29117)), plus the routine release version bump. Otherwise quiet in the last 24 hours.

## 3. Hot Issues
Chosen from 50 issues updated in the last 24 hours.

- **[#22323](https://github.com/google-gemini/gemini-cli/issues/22323) — Subagent recovery after MAX_TURNS is reported as GOAL success (P1, 13 comments)**: `codebase_investigator` returns `status: "success"` / `Termination Reason: "GOAL"` even when it hit the turn limit before any analysis. Misleading termination semantics hide real interruptions and undermine trust in agent output.
- **[#29164](https://github.com/google-gemini/gemini-cli/issues/29164) — 3.6 and 3.7 flash still not available in the model picker (P1, 12 👍)**: Highest-reaction issue this cycle. Users expect the latest flash models selectable immediately; PR #29172 (below) appears to be the direct fix in flight.
- **[#21409](https://github.com/google-gemini/gemini-cli/issues/21409) — Generalist agent hangs (P1, 8 👍)**: Simple tasks like folder creation hang indefinitely when deferred to the generalist agent; users report waiting up to an hour. Explicitly disabling subagent deferral works around it, pointing at the dispatch path.
- **[#25166](https://github.com/google-gemini/gemini-cli/issues/25166) — Shell command stuck with "Waiting input" after command completes (P1)**: Even trivial CLI commands finish but leave the session wedged in an "awaiting input" state. Recurring and high-friction for shell-centric workflows.
- **[#19873](https://github.com/google-gemini/gemini-cli/issues/19873) — Zero-Dependency OS Sandboxing & Post-Execution Intent Routing (P2, 9 comments)**: Enhancement proposal to let Gemini 3's native bash affinity run safely via OS-level sandboxing and intent routing after command execution. Signals the desired direction for secure shell autonomy.
- **[#29197](https://github.com/google-gemini/gemini-cli/issues/29197) — TOML command interpolation stuck in infinite permission loop (new today)**: A `.toml` command template with two `!{}` interpolations re-requests permission for the first command after the second is approved. Fresh day-0 bug with an easy repro.
- **[#21983](https://github.com/google-gemini/gemini-cli/issues/21983) — Browser subagent fails in Wayland (P1)**: Browser agent terminates with `GOAL` without useful output under Wayland. Long-standing platform gap now tagged `need-retesting`.
- **[#26525](https://github.com/google-gemini/gemini-cli/issues/26525) — Add deterministic redaction and reduce Auto Memory logging (P2, security)**: Auto Memory sends transcript content to the extraction model before prompt-based redaction occurs, meaning secrets already enter model context. Requests deterministic pre-send redaction and quieter logging.
- **[#22745](https://github.com/google-gemini/gemini-cli/issues/22745) — AST-aware file reads, search, and mapping (P2 epic, 7 comments)**: Epic investigating AST-aware tooling to read exact method bounds in one call, reduce token noise from misaligned reads, and improve codebase navigation/reasoning.
- **[#21968](https://github.com/google-gemini/gemini-cli/issues/21968) — Gemini does not use skills and sub-agents enough**: Anecdotal but recurring: custom skills (e.g., `gradle`, `git`) and sub-agents are ignored unless explicitly requested, even for closely related tasks — limiting the value of user-defined agents.

## 4. Key PR Progress
Chosen from 41 PRs updated in the last 24 hours.

- **[#29184](https://github.com/google-gemini/gemini-cli/pull/29184) — Validate git args in Windows sandbox**: Blocks silent `git diff --output=<path>` file truncation. Windows treats all `git status|log|diff|show|branch` as read-only without confirmation; this closes a real data-loss vector.
- **[#29172](https://github.com/google-gemini/gemini-cli/pull/29172) — Add gemini-3.8-flash as default flash model**: Registers 3.5-flash-lite through 3.8-flash as selectable models and promotes 3.8-flash to default — directly addressing model-picker complaints like #29164.
- **[#29192](https://github.com/google-gemini/gemini-cli/pull/29192) — Contain legacy raw tag path inside checkpoints directory (P1, security)**: `/chat delete <tag>` with `../` in the tag could delete files outside the checkpoints dir via the backward-compat raw-tag fallback. Now confined.
- **[#29195](https://github.com/google-gemini/gemini-cli/pull/29195) — Degrade non-array history instead of crashing resume**: Checkpoint files with valid JSON but non-array `history` crashed `/resume`; now degrades to an empty checkpoint like other unparseable files.
- **[#28930](https://github.com/google-gemini/gemini-cli/pull/28930) — Drop unsafe `diff.external` override (closed)**: The empty-string `diff.external` override from #28792 didn't disable external diffs as intended and created an unsafe git environment; removed. Fixes #28928.
- **[#28938](https://github.com/google-gemini/gemini-cli/pull/28938) — Keep GIT_CONFIG_* environment triplets internally consistent (closed)**: Prevents redaction from removing one half of a numbered `GIT_CONFIG_*` key/value pair, which made the environment unparsable, and stops `ShellExecutionService` from restoring sensitive git config after sanitization.
- **[#29106](https://github.com/google-gemini/gemini-cli/pull/29106) — Flush final SSE event on EOF**: The SSE parser dropped the last buffered event when a stream ended without a trailing blank line, silently losing `finishReason`/usage metadata on truncated connections or non-conformant proxies.
- **[#29110](https://github.com/google-gemini/gemini-cli/pull/29110) — Route `read_file` content through FileSystemService**: `read_file` bypassed the injected `FileSystemService`, breaking ACP clients advertising `fs.readTextFile` support. Now consistent with `write_file` and `replace`.
- **[#29158](https://github.com/google-gemini/gemini-cli/pull/29158) — Sanitize hardcoded Google CrUX API key in chrome-devtools-mcp (closed)**: Removes an exposed CrUX API key from the compiled `chrome-devtools-mcp` bundle and copied third-party assets before distribution.
- **[#28939](https://github.com/google-gemini/gemini-cli/pull/28939) — Avoid persisting interrupted response placeholder (closed)**: After an interrupted tool-response turn, the synthetic "[The previous response was interrupted…]" text could persist and be repeated by the model in later turns. Fixes #28927.

## 5. Feature Request Trends
- **Faster model availability**: Users expect new flash models (3.6/3.7/3.8) in the picker immediately upon release (#29164); PR #29172 is the corresponding enablement.
- **Secure-by-default execution**: Strong pull toward OS-level sandboxing for the model's native shell/bash workflows (#19873), deterministic secret redaction *before* content enters model context (#26525), and preventing destructive git/DB commands (#22672, #29184).
- **Precision tooling to cut tokens and turns**: AST-aware file reads, search, and codebase mapping are being actively scoped as epics (#22745, #22746), with native file tools for task tracking also under experiment (#21000).
- **Better agent self-awareness and utilization**: Users want the CLI to proactively use custom skills/sub-agents (#21968) and to accurately know its own flags, hotkeys, and self-execution mechanics (#21432).
- **Sub-execution observability**: Subagent trajectories and internal context should be visible/shareable via `/chat share` and included in `/bug` reports (#22598, #21763).
- **Smarter memory hygiene**: Auto Memory should quarantine invalid inbox patches, stop retrying low-signal sessions indefinitely, and reduce logging of skill contents (#26522, #26523, #26525).

## 6. Developer Pain Points
- **False success and invisible hangs**: Subagents report `GOAL`/`success` after hitting `MAX_TURNS` (#22323), generalist agents hang indefinitely (#21409), and shell commands stay stuck at "Waiting input" after finishing (#25166). Users resort to instructing the model to never defer to subagents as a workaround.
- **Config and environment drift**: Browser agent ignores `settings.json` overrides like `maxTurns` (#22267); symlinked agent files under `~/.gemini/agents/` aren't recognized (#20079); redacted git environments can become unparsable (#28938).
- **Security foot-guns**: Silent Windows sandbox commands that can truncate files (#29184), hardcoded API keys shipped in bundles (#29158), checkpoint path traversal (#29192), and secrets sent to extraction models before redaction (#26525).
- **Tool and process sprawl**: 400 errors once tool counts grow large (#24246), models scattering temporary edit scripts across directories and dirtying commits (#23571), and hangs at interactive prompts like Vite scaffolding (#22465).
- **Reliability around interruption/state**: Interrupted responses persist as synthetic text the model repeats (#28939), checkpoints crash on malformed history (#29195), and TOML command templates loop on permission requests (#29197).

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

## Today’s Highlights

Two incremental releases shipped in the last 24 hours: **v1.0.83-4** brings MCP OAuth/Client ID Metadata Document support and faster large-session resume behavior, while **v1.0.83-5** adds Windows 11 taskbar session presence plus stronger macOS/Linux sandbox network isolation. The issue tracker shows clear focus areas — MCP/plugin interoperability, long-session stability, and enterprise policy constraints — but no pull requests were updated in the same window.

## Releases

- **v1.0.83-5** — [GitHub Release](https://github.com/github/copilot-cli/releases/tag/v1.0.83-5)  
  - Added Windows 11 taskbar presence for running Copilot sessions, with live hover status cards.  
  - Improved sandbox network isolation on macOS/Linux: sandboxed commands can no longer reach services running on the host machine. On macOS this also affects commands that start their own server on `127.0.0.1`, so local test-suite behavior may change.

- **v1.0.83-4** — [GitHub Release](https://github.com/github/copilot-cli/releases/tag/v1.0.83-4)  
  - Added Client ID Metadata Document (CIMD) support for MCP OAuth sign-in.  
  - The CLI now starts without showing the interrupted-session restore prompt by default.  
  - Resuming large sessions now keeps the input prompt responsive sooner.  
  - Fixed an issue where sandboxed file tools were not reading the same developer tooling configuration as the normal path.

## Hot Issues

1. **MCP handshake regression after modern `server/discover`** — [#4525](https://github.com/github/copilot-cli/issues/4525)  
   Open `area:mcp`. Copilot CLI sends a legacy `initialize` after a successful modern `server/discover`, causing MCP initialization failures with Python MCP SDK 2.0 stdio servers. Six comments and 3 👍 — important for the current MCP ecosystem transition.

2. **Enterprise remote sessions disabled warning** — [#3442](https://github.com/github/copilot-cli/issues/3442)  
   Closed but heavily discussed. Users on v1.0.51 see “Remote sessions are not enabled” even when org settings appear correct. With 10 👍, this remains a notable enterprise rollout pain point.

3. **Compaction fails with empty model responses** — [#2861](https://github.com/github/copilot-cli/issues/2861)  
   Open. Manual and automatic `/compact` fail repeatedly with empty model responses, even on Claude Opus 4.6. Five comments and 4 👍 — a direct threat to long-session context reliability.

4. **MCP OAuth tokens not reused across sessions** — [#4695](https://github.com/github/copilot-cli/issues/4695)  
   Open. HTTP-based MCP servers using OAuth/PKCE receive duplicate cache-key entries, forcing users to re-authenticate more often than expected. Five comments.

5. **Global `--system-prompt` parameter request** — [#232](https://github.com/github/copilot-cli/issues/232)  
   Open. Long-running feature request: users want system-level instructions outside repo-specific instruction files. Four comments and 10 👍; still unresolved after nearly a year.

6. **Agent Plugins 1.0 custom agents not discovered** — [#4655](https://github.com/github/copilot-cli/issues/4655)  
   Open. Custom agents placed under `com.github.copilot/agents` inside Agent Plugins are not discovered, blocking reusable agent distribution. Three comments.

7. **PowerShell ConstrainedLanguage error on every command** — [#4683](https://github.com/github/copilot-cli/issues/4683)  
   Open. Under AppLocker/WDAC ConstrainedLanguage mode, every shell command emits a spurious `$host.SetShouldExit()` error. Two comments; significant for managed Windows environments.

8. **OOM crash on long `--resume` sessions** — [#4699](https://github.com/github/copilot-cli/issues/4699)  
   Open. Long resumed sessions repeatedly crash at the V8 4 GiB heap cap, and Node diagnostic dumps are written into the user’s current working directory. Two 👍.

9. **Configurable model pool for Auto mode** — [#4218](https://github.com/github/copilot-cli/issues/4218)  
   Open feature request. Users want to restrict which models Auto mode can select. The 13 👍 — the highest reaction count in this window — shows strong desire for predictable cost/behavior.

10. **`--yolo`/allow-all mode resets after long idle periods** — [#4696](https://github.com/github/copilot-cli/issues/4696)  
    Open. After roughly eight hours of inactivity, allow-all permissions are silently dropped even though the session is still active.

## Key PR Progress

None. GitHub data shows **0 pull requests updated in the last 24 hours**, so there are no PRs to summarize.

## Feature Request Trends

- **Model and provider control** — Users want to constrain Auto mode’s model pool ([#4218](https://github.com/github/copilot-cli/issues/4218)) and assign different provider endpoints per agent rather than process-wide ([#4703](https://github.com/github/copilot-cli/issues/4703)).
- **Session management quality** — `/resume` and `/session` should support filtering by current working directory ([#4704](https://github.com/github/copilot-cli/issues/4704)). The new v1.0.83-4 behavior, which removes the interrupted-session restore prompt by default, partially addresses session startup friction.
- **Global/systems-level prompt support** — The long-running request for a first-class `--system-prompt` parameter remains open ([#232](https://github.com/github/copilot-cli/issues/232)).
- **Plugin and marketplace governance** — Enterprise users want the ability to block/hide built-in Copilot plugin marketplaces ([#4715](https://github.com/github/copilot-cli/issues/4715)), while plugin authors are blocked by custom-agent discovery gaps ([#4655](https://github.com/github/copilot-cli/issues/4655)) and skill visibility issues for subagents ([#4708](https://github.com/github/copilot-cli/issues/4708)).

## Developer Pain Points

- **Long-session instability is the biggest recurring theme.** Developers report heap OOM crashes ([#4699](https://github.com/github/copilot-cli/issues/4699)), runaway `copilot-file-search` threads consuming CPU and disk while idle ([#4710](https://github.com/github/copilot-cli/issues/4710)), compaction failures ([#2861](https://github.com/github/copilot-cli/issues/2861)), slow resume with no loading UI ([#4714](https://github.com/github/copilot-cli/issues/4714)), and extension startup failures on very large session histories ([#4670](https://github.com/github/copilot-cli/issues/4670), [#4717](https://github.com/github/copilot-cli/issues/4717)).
- **Enterprise and managed environments face frequent policy blockers.** PowerShell ConstrainedLanguage errors ([#4683](https://github.com/github/copilot-cli/issues/4683)), unclear remote-session enablement ([#3442](https://github.com/github/copilot-cli/issues/3442)), `telemetry.headers` breaking OpenTelemetry export ([#4669](https://github.com/github/copilot-cli/issues/4669)), and inability to block built-in marketplaces ([#4715](https://github.com/github/copilot-cli/issues/4715)) all hurt enterprise adoption.
- **MCP/plugin integration still consumes significant developer time.** Recurring protocol handshake and auth issues include legacy `initialize` after `server/discover` ([#4525](https://github.com/github/copilot-cli/issues/4525)), unreliable OAuth token reuse ([#4695](https://github.com/github/copilot-cli/issues/4695)), custom agent discovery ([#4655](https://github.com/github/copilot-cli/issues/4655)), and subagents lacking access to installed skills ([#4708](https://github.com/github/copilot-cli/issues/4708)).
- **Permission-mode transparency is also a concern.** Allow-all mode silently resets after inactivity ([#4696](https://github.com/github/copilot-cli/issues/4696)), permission-gate previews truncate Windows paths ([#4701](https://github.com/github/copilot-cli/issues/4701)), and queued prompts occasionally get stuck after the session becomes idle ([#4705](https://github.com/github/copilot-cli/issues/4705)).

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI Community Digest — 2026-09-04

## Today’s Highlights
Most issue activity this cycle is housekeeping: six long-running bugs and feature requests were closed or updated, including several from early March. The standout open item is **#2633**, which reports that the 1.17+ ACP auth gate now breaks custom providers that don’t use a Kimi account. On the code side, the only active PR updates Kimi provider token-budget handling to use dynamic completion budgets instead of a hardcoded `max_tokens`.

## Releases
No new releases were published in the last 24 hours.

## Hot Issues
Only seven issues had activity in the last 24 hours; all are listed below, since the standard “10 noteworthy” threshold exceeds the available dataset. All are closed except **#2633**.

- **[#2633 [OPEN] ACP auth gate (1.17+) blocks custom providers that don't need a Kimi account](https://github.com/MoonshotAI/kimi-cli/issues/2633)**  
  Reports that since 1.17.0, the ACP server unconditionally requires a persisted Kimi OAuth token for `session/new`, `session/load`, `session/resume`, and `session/prompt`. This is a significant compatibility concern for developers using third-party/self-hosted backends through ACP. No comments yet.

- **[#290 [CLOSED] Use openrouter with custom model returns 401](https://github.com/MoonshotAI/kimi-cli/issues/290)**  
  Old bug from Kimi CLI 0.54 involving `openai/gpt-5.1-codex` on the `kimi-for-coding` platform. Now closed, but it highlights recurring friction with BYO/custom-provider auth. Three comments.

- **[#1316 [CLOSED] MCP timeout causes kimi-cli to be unavailable](https://github.com/MoonshotAI/kimi-cli/issues/1316)**  
  A single MCP connection failure takes down the whole CLI process. This is a critical reliability issue for MCP-heavy workflows; now closed, likely with a fix? Only one comment.

- **[#1320 [CLOSED] Smart arrow key navigation for multiline input](https://github.com/MoonshotAI/kimi-cli/issues/1320)**  
  Asks for cursor-aware Up/Down behavior in multiline inputs rather than always triggering history navigation. A common UX pain point in terminal-based editors. No comments.

- **[#1319 [CLOSED] Add methods for local skills operation management](https://github.com/MoonshotAI/kimi-cli/issues/1319)**  
  Requests CLI commands like `skills list` / `skills rm` and a unified storage layout for locally created skills. Reflects a broader desire for first-class skills lifecycle management outside of `/skill` commands. No comments.

- **[#1315 [CLOSED] Subagents keep running after hitting ESC](https://github.com/MoonshotAI/kimi-cli/issues/1315)**  
  On Windows, pressing ESC does not reliably abort subagents. Cancellation reliability is important because users rely on ESC as an emergency stop during long agent runs. No comments.

- **[#1313 [CLOSED] Feature Request: Add Hooks System for Notifications and Lifecycle Events](https://github.com/MoonshotAI/kimi-cli/issues/1313)**  
  Would allow notifications when an agent task starts, blocks, completes, or needs attention. Received 3 👍, the strongest positive community reaction in this batch.

## Key PR Progress
Only one pull request was updated in the last 24 hours.

- **[#2332 fix(kimi): clamp completion budget dynamically](https://github.com/MoonshotAI/kimi-cli/pull/2332)**  
  Removes the provider-level hardcoded `max_tokens = 32000` and instead computes a per-request `max_completion_tokens` that fits the current context window. This should prevent out-of-bounds completion-size errors and better respect Kimi models’ context limits. No explicit community comments.

## Feature Request Trends
Distilling requests from recent issues:

- **Hooks/event-driven lifecycle notifications** — Users want OS notifications or external callbacks when long-running agent tasks need attention (#1313).
- **Local skills management** — There is an ongoing need for dedicated commands to list, version, inspect, and remove user-defined skills (#1319).
- **Smarter interactive terminal editing** — Multiline input navigation should respect cursor position, not blindly use Up/Down for history (#1320).
- **Provider/auth flexibility** — Users repeatedly want to use custom/OpenRouter models without forcing a Kimi account/OAuth path (#290, #2633).
- **Robust subprocess/agent cancellation** — ESC should stop all subagents; MCP failures should not make the whole CLI unusable (#1315, #1316).

## Developer Pain Points
- **MCP failures are fatal** — A single unreachable MCP server can terminate kimi-cli instantly (#1316).
- **Forced Kimi auth in newer versions** — ACP now blocks otherwise valid custom providers that don’t rely on Kimi accounts (#2633).
- **Cancellation is unreliable** — Subagents can keep running after ESC, especially on Windows (#1315).
- **Hardcoded token budgets cause breakdowns** — Fixed `max_tokens` values need to become context-window-aware to avoid request failures (#2332).
- **Lack of local skill discoverability/control** — Developers cannot easily see or delete their own skills without leaving the CLI and hunting through inconsistent storage directories (#1319).

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-09-04

**Data source:** github.com/anomalyco/opencode

## 1. Today's Highlights

No new releases landed in the past 24 hours, but the project saw a heavy maintenance and feature burst: a wave of desktop UI polish PRs, two reliability fixes for the TUI/server event stream (clean exit during startup probes, reconnect backoff), and continued expansion of the browser-automation and plugin-permission surfaces. Community attention remains split between long-standing reliability complaints—Gemini edit-tool string matching (#266) and GLM prompt-cache drops (#31348)—and a strong appetite for Claude-Code-style workflow automation and multi-agent orchestration features.

## 2. Releases

No new releases in the last 24 hours.

## 3. Hot Issues

- **[#266 — Gemini doesn't handle edit tool very well](https://github.com/anomalyco/opencode/issues/266)** — 39 comments, 17 👍. The longest-running open complaint: Gemini frequently fails with `oldString not found in file`, even with exact whitespace/line-break matches. Author suggests whitespace normalization as a possible fix. Still open after 15 months, indicating a stubborn model/tool integration gap.

- **[#29059 — [FEATURE]: Add Dynamic workflows for repeatable multi-step automation](https://github.com/anomalyco/opencode/issues/29059)** — 17 comments, 22 👍 (highest 👍 in the batch). Users want project-local, repeatable multi-step workflows in the style of the new Claude Code feature. Closed recently, presumably after design discussion or implementation elsewhere.

- **[#17994 — [FEATURE]: Support for multi-agent orchestration in isolated workspaces](https://github.com/anomalyco/opencode/issues/17994)** — 24 comments. Requests a built-in "team" of coding agents running in isolated workspaces, similar to existing external tools. Closed on 2026-09-04 after six months of discussion; watch for an announcement.

- **[#31348 — GLM-5.1 prompt cache randomly drops to 0 on opencode-go](https://github.com/anomalyco/opencode/issues/31348)** — 7 comments, 7 👍. Long-running workflows see cache reads randomly drop to zero with GLM-5.1 while DeepSeek V4 Flash stays stable—causing unexpected cost spikes. A provider-specific reliability issue with real budget impact.

- **[#47184 — "It constantly goes into repetitive loops"](https://github.com/anomalyco/opencode/issues/47184)** — newest quality regression report (created 2026-09-04). Vague on details/version, but the sentiment echoes a common complaint pattern; likely to attract maintainer requests for a repro.

- **[#33677 — `edit` permission not enforced for `edit`/`write` tool calls](https://github.com/anomalyco/opencode/issues/33677)** — 3 comments. The `permission.edit` config setting never triggers an "ask" prompt, while `bash` and `webfetch` permissions work correctly. Security-relevant; tool-call permissions are a core trust boundary.

- **[#35419 — Desktop fails to bootstrap on unrecognized config keys; cleanup wipes valid config](https://github.com/anomalyco/opencode/issues/35419)** — 2 comments. After upgrade to v1.17.13 on Windows, an unknown config key triggers a `ConfigInvalidError` loop and the model dropdown empties; recovery from backups fails, risking configuration loss. Data-loss-adjacent bug, high severity for desktop users.

- **[#29210 — Install Fails with Syntax Error on WSL](https://github.com/anomalyco/opencode/issues/29210)** — 6 comments. `npm i -g opencode-ai` breaks in `postinstall.mjs:25` during platform detection. Onboarding friction for WSL users; a recurring install-script fragility.

- **[#34117 — v1.17.11 tag: Nix build fails with stale bun.lock](https://github.com/anomalyco/opencode/issues/34117)** — 4 comments, 4 👍. Released tags fail `bun install --frozen-lockfile` because `bun.lock` is stale relative to `package.json`. Reproducible-build breakage that affects Nix users; related duplicates #34235 show it's systemic.

- **[#28566 — OpenRouter Service Tiers Support for Reducing Model Cost](https://github.com/anomalyco/opencode/issues/28566)** — 7 comments. Requests config support for OpenRouter's `service_tier` parameter. Part of a broader community theme: cost control and prompt-cache reliability.

## 4. Key PR Progress

- **[#47180 — Desktop plugin manager: browse, install, and manage plugins from settings](https://github.com/anomalyco/opencode/pull/47180)** — Merged. Adds a Plugins tab merging three catalogs (official docs ecosystem, `awesome-opencode`, opencode.cafe) with npm metadata. A significant desktop UX expansion.

- **[#47187 — Add `run_in_background` to shell tool with auto notification](https://github.com/anomalyco/opencode/pull/47187)** — Merged. First-class background execution for long-running commands (dev servers, watch/test suites) with output capture and completion notification, replacing `nohup ... &` hacks.

- **[#46548 — feat(ai): add tool namespaces](https://github.com/anomalyco/opencode/pull/46548)** — Open. Introduces recursive, provider-neutral `ToolEntry`/`ToolNamespace` definitions so large tool trees can be normalized, deduplicated, and budgeted without collapsing sibling namespaces. Foundational architecture work for scaling tool ecosystems.

- **[#47204 — Back off reconnects when the stream never connects](https://github.com/anomalyco/opencode/pull/47204)** — Open (fixes #47062). Replaces fixed one-second retry with backoff for event-stream clients, addressing unauthenticated browser sessions and dead streams hammering the server.

- **[#46726 — TUI exits cleanly when startup probes cannot reach the server](https://github.com/anomalyco/opencode/pull/46726)** — Open (fixes #36688). Handles the cold-boot/electing-server case where TUI startup location probes fail; prevents hang-or-crash behavior after updates.

- **[#47208 — Show server-known projects in project lists](https://github.com/anomalyco/opencode/pull/47208)** — Open (closes #43072). Fixes project list in the home panel/session sidebar being fed only by local persistence, ignoring server-known projects.

- **[#47160 — Classify GitHub Copilot requests on every route](https://github.com/anomalyco/opencode/pull/47160)** — Merged. Bug fix: Copilot's `X-Interaction-Type` header was only set on the native Claude route, not the AI SDK route used for GPT/Copilot models, so title/compaction requests were misclassified.

- **[#46530 — Plugin permission assertions](https://github.com/anomalyco/opencode/pull/46530)** — Open. Adds `ctx.permission.assert()` for Effect/Promise plugins, canonical-URL checks before tab operations, and server-file/external-directory checks before uploads. Closes a plugin sandboxing gap.

- **[#44838 — Browser tabs and Chromium diagnostics in desktop](https://github.com/anomalyco/opencode/pull/44838)** — Open. Multi-tab open/focus/close, shared ownership between user controls and agent tools, cross-origin frame inspection, and snapshots from the Review pane. Large agentic-browser surface.

- **[#46531 — Public-API browser plugin](https://github.com/anomalyco/opencode/pull/46531)** — Open. Companion to #44838: 44 namespaced Code Mode methods (tabs, interaction, snapshots, files, diagnostics, profiling, audits) via a pure RPC entrypoint. Together these two PRs define the future browser-automation API.

## 5. Feature Request Trends

- **Multi-agent orchestration and task-level model control.** Desire for "teams" of agents in isolated workspaces (#17994) plus per-subagent model selection in the Task tool (#26925). A session-move suggestion into task worktrees also landed (#47202), signaling maintainers are leaning into worktree-per-task flows.
- **Repeatable workflow automation.** Project-local dynamic workflows for multi-step automation (#29059) mirroring Claude Code's newest feature. Likely the next major automation paradigm for coding agents.
- **Plugin API expansion.** Recurring asks for more hooks and surfaces: blocking/cancelling user messages in `chat.message` (#30434), exposing v2 session APIs (#35443), and before/after prompt hooks for TUI plugins (#47087). Permission assertions (#46530) show the maintainers are hardening the same boundary.
- **Cost control.** OpenRouter service tiers (#28566), prompt-cache stability for GLM (#31348), and cheaper sub-agents via Task model overrides (#26925) all point to cost as a top-of-mind concern for heavy users.
- **Git worktree isolation ergonomics.** A `--worktree`/`-w` CLI flag request (#35471) plus the bug where non-git projects break permissions by using `/` as worktree (#24694) indicate worktrees are core infrastructure but need UX polish.

## 6. Developer Pain Points

- **Tool-call reliability with specific models.** Gemini edit failures with exact-match `oldString` errors (#266) and GLM-5.1 prompt-cache drops (#31348) show model-specific integration bugs that users can't work around via config.
- **Build and install friction.** Stale-lockfile failures on released Nix tags (#34117, #34235), WSL postinstall syntax errors (#29210), and untrusted Homebrew tap warnings (#32072) repeatedly interrupt adoption and reproducible builds.
- **Desktop app resilience.** Renderer crashes on deleted workspace files (#35493), silent failures when project volumes are ejected (#35438), tool calls terminated without diagnostics (#35485), and config-wipe loops (#35419) paint a picture of a desktop client that fails without actionable error messages.
- **Permission enforcement gaps.** The `edit` permission silently not triggering "ask" prompts (#33677) undermines trust in the permission system, especially for users running untrusted agents.
- **Connectivity and responsiveness.** API timeouts (#35483) and a completely unresponsive TUI for first-run users (#35474) are blocking experiences that surface quickly in community channels.
- **Quality regressions in recent versions.** The unverified "repetitive loops" complaint (#47184) and the sibling-race follow-up (#35399) suggest users are sensitive to agent-loop and concurrency regressions after updates.

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi Community Digest — 2026-09-04

## Today's Highlights
No new release shipped in the last 24 hours; activity focused on bug triage and a wave of contributor fixes across TUI polish, Linux packaging, and docs. Reliability reports dominate the tracker: context-budget overflow recovery fails mid-session ([#8061](https://github.com/earendil-works/pi/issues/8061)), streaming renders pathologically ([#8822](https://github.com/earendil-works/pi/issues/8822), [#9062](https://github.com/earendil-works/pi/issues/9062)), and sessions can bloat past context limits from duplicated reasoning payloads ([#9097](https://github.com/earendil-works/pi/issues/9097)). A joke "rewrite pi in Rust" PR ([#9106](https://github.com/earendil-works/pi/pull/9106)) was closed quickly, as was its approval-request issue ([#9107](https://github.com/earendil-works/pi/issues/9107)).

## Releases
None in the last 24 hours.

## Hot Issues
- **[#8845 — Branch summarization deterministically fails: hardcoded `maxTokens: 2048`](https://github.com/earendil-works/pi/issues/8845)** *(closed, 14 comments)* — `/tree` branch summarization always fails on large branches, hitting the token cap and producing incomplete summaries. The hardcoded limit ignores model or branch size; community is pushing to make it configurable or model-aware.
- **[#5023 — Terminal scrolls to beginning without reason](https://github.com/earendil-works/pi/issues/5023)** *(closed, 18 comments)* — Long-running sessions randomly jump to session start and fast-scroll to the buffer end while the model is working. Highest-comment issue in the window; no deterministic repro has been found.
- **[#8061 — Context budget ignores maxTokens output reservation](https://github.com/earendil-works/pi/issues/8061)** *(open, 2 👍)* — A provider rejected a request at only 78% input context because the output-token reservation wasn't accounted for; the automatic compact-and-retry recovery then failed for the same reason. Highlights a correctness gap in budget calculation.
- **[#9097 — DeepSeek/OpenRouter thinking turns persist redundant `thinkingSignature`](https://github.com/earendil-works/pi/issues/9097)** *(closed)* — Every streamed thinking block also stores the full reasoning signature field, bloating a one-lane session to 4.5 MB and hitting provider context limits on every resume.
- **[#9105 — `processFileArguments()` corrupts binary attachments via forced UTF-8 decode](https://github.com/earendil-works/pi/issues/9105)** *(closed)* — Both `@file` mentions and the Read tool silently corrupt binary files by lossy-decoding them as text before image/text handling. High severity for anyone passing non-text assets to agents.
- **[#9104 — Agent gets stuck repeating the same response indefinitely](https://github.com/earendil-works/pi/issues/9104)** *(closed)* — Model entered a degenerate loop, repeating "OK I'm in a dead loop…" many times without termination or recovery, even while explicitly self-identifying the dead loop.
- **[#8822 — Streaming output falls behind: per-delta full O(n²) markdown re-render](https://github.com/earendil-works/pi/issues/8822)** *(open)* — With ~25 SSE chunks/s, every delta triggers a full markdown re-render on the synchronous event path, so the TUI visually lags the model output.
- **[#9062 — Tool-call argument parsing becomes quadratic with fragmented deltas](https://github.com/earendil-works/pi/issues/9062)** *(closed)* — `processResponsesStream()` re-parses the full accumulated JSON buffer on every delta instead of incrementally, causing O(N²) work.
- **[#9094 — Reasoning-markup literals silently stripped/mangled from tool I/O](https://github.com/earendil-works/pi/issues/9094)** *(closed)* — Literal `<think>` open/close tags are byte-level corrupted in both directions: written file contents/tool arguments and returned tool results.
- **[#9079 — Plugin auth-file keys ignored; only `/login` store is checked](https://github.com/earendil-works/pi/issues/9079)** *(closed)* — Provider plugins that store API keys in their own auth file report "No API key found" unless `/login` duplicated the key into the core store, breaking `pi -ne` extension-only setups.

## Key PR Progress
- **[#8998 — System prompt refactor (draft by mitsuhiko)](https://github.com/earendil-works/pi/pull/8998)** *(open)* — Large architectural change enabling partial, mid-conversation system prompt updates for extensions, avoiding full-session resets when tools/system context change dynamically.
- **[#9096 — Add Meta provider with Muse subscription OAuth](https://github.com/earendil-works/pi/pull/9096)** *(open, resolves #7543)* — New provider with an unusual daily identity-token re-minting flow instead of rolling refresh tokens; streaming is currently bursty/fake on medium outputs.
- **[#8734 — Support top-level `instructions` for OpenAI Responses-compatible providers](https://github.com/earendil-works/pi/pull/8734)** *(open, closes #8388)* — Adds a `systemPromptFormat` compatibility option, moving the dynamic system prompt to top-level `instructions` without duplicating it in `input`.
- **[#8994 — Map signal-killed processes to non-zero exit codes](https://github.com/earendil-works/pi/pull/8994)** *(closed, fixes #8992)* — `waitForChildProcess` resolved `null` on signal termination, and `exec` mapped it to `code ?? 0`, making OOM-killed bash tool calls look successful.
- **[#9070 — Download statically linked musl fd/ripgrep builds on Linux](https://github.com/earendil-works/pi/pull/9070)** *(closed, fixes #9033)* — glibc-linked binaries previously broke find/grep tools on NixOS and Alpine; switches to musl release assets.
- **[#9087 — Fail fast when a dynamic model's API has no matching implementation](https://github.com/earendil-works/pi/pull/9087)** *(closed)* — `openrouter/anthropic/*` requests previously surfaced a giant HTML 404 page as the error; now produces a clear model/API mismatch message.
- **[#9081 — Let `registerProvider` API key read plugin auth files](https://github.com/earendil-works/pi/pull/9081)** *(closed, closes #9079)* — `apiKey` can now be a function resolved at request time, letting plugins pull keys from their own auth files.
- **[#9084 — Update source checkouts via rebase](https://github.com/earendil-works/pi/pull/9084)** *(closed)* — `pi update` now handles source-checkout installs with `git pull --rebase` + `npm ci` instead of falling through to generic package-manager messaging.
- **[#9080 — TUI jump-to-latest control](https://github.com/earendil-works/pi/pull/9080)** *(closed)* — Adds a jump-to-latest control in alt-screen mode, building on earlier new-message-indicator work.
- **[#9077 — Document running Pi in Docker Sandboxes](https://github.com/earendil-works/pi/pull/9077)** *(closed, closes #8788)* — Adds a "Docker Sandboxes" section to `containerization.md` plus a row in the "Choose a pattern" comparison table.

## Feature Request Trends
- **Model catalog freshness and correctness** — Requests to add `gemini-3.8-flash` ([#9076](https://github.com/earendil-works/pi/issues/9076)), ship a Meta provider ([#9096](https://github.com/earendil-works/pi/pull/9096)), and exclude/curate models like Grok Build 0.1 ([#9093](https://github.com/earendil-works/pi/pull/9093)) show the community treats built-in catalogs as a correctness guarantee.
- **TUI viewport and scrolling ergonomics** — Fullscreen-mode scroll speed ([#9052](https://github.com/earendil-works/pi/issues/9052)), jump-to-latest controls ([#9080](https://github.com/earendil-works/pi/pull/9080)), OSC 8 clickable tool paths ([#5168](https://github.com/earendil-works/pi/issues/5168)), Ghostty link support ([#4839](https://github.com/earendil-works/pi/issues/4839)), and extension-exposed viewport primitives ([#4861](https://github.com/earendil-works/pi/issues/4861)).
- **Deeper extension API surface** — Partial system-prompt updates ([#8998](https://github.com/earendil-works/pi/pull/8998)), package namespaces for skills/templates ([#8834](https://github.com/earendil-works/pi/issues/8834)), a `prepareSubagentArguments` hook ([#9072](https://github.com/earendil-works/pi/issues/9072)), and true override of built-in tools by extension-registered names ([#9071](https://github.com/earendil-works/pi/issues/9071)).
- **Context health observability** — Exposing prompt disposition (`handled`/`queued`/`started`) in RPC responses ([#9098](https://github.com/earendil-works/pi/issues/9098)) and surfacing the full undocumented scope of `PI_OFFLINE` ([#8684](https://github.com/earendil-works/pi/issues/8684)).

## Developer Pain Points
- **Streaming performance pathology** — Three independent reports of quadratic/blocking behavior: per-delta full markdown re-renders ([#8822](https://github.com/earendil-works/pi/issues/8822)), re-parsing the whole accumulated JSON buffer per tool-call delta ([#9062](https://github.com/earendil-works/pi/issues/9062)), and 3× slower wheel scrolling in fullscreen mode ([#9052](https://github.com/earendil-works/pi/issues/9052)).
- **Silent incorrect statuses** — Signal-terminated commands reported as exit code 0 ([#8882](https://github.com/earendil-works/pi/issues/8882), fixed by [#8994](https://github.com/earendil-works/pi/pull/8994)), sessions silently falling back to another provider's default model ([#8810](https://github.com/earendil-works/pi/issues/8810)), and `PI_OFFLINE` disabling far more than documented ([#8684](https://github.com/earendil-works/pi/issues/8684)).
- **Context accounting and session bloat** — Missing output-token reservations ([#8061](https://github.com/earendil-works/pi/issues/8061)), hardcoded summarization caps ([#8845](https://github.com/earendil-works/pi/issues/8845)), and duplicated `thinkingSignature` payloads ([#9097](https://github.com/earendil-works/pi/issues/9097)) all push real sessions into unrecoverable provider limit errors.
- **Silent data corruption** — Forced UTF-8 decoding of binary attachments ([#9105](https://github.com/earendil-works/pi/issues/9105)) and byte-level mangling of reasoning-markup literals through the tool layer ([#9094](https://github.com/earendil-works/pi/issues/9094)).
- **Environment-specific breakages** — glibc-dynamic fd/ripgrep downloads breaking NixOS/Alpine ([#9070](https://github.com/earendil-works/pi/pull/9070)), CRLF edit-tool failures on Windows ([#355](https://github.com/earendil-works/pi/issues/355)), and tool calls with no execution timeout hanging runs indefinitely ([#8857](https://github.com/earendil-works/pi/issues/8857)).

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-09-04

## 1. Today’s Highlights

Qwen Code shipped **v0.23.0** with no breaking changes, adding git state hints to the branch picker. On the issue side, the community is converging around content-leak fixes, with two PRs landing that close the latest `thinking`/tool-result scaffolding leaks. Developer attention remains split between terminal UI rendering modernization and P1 security findings around Bash allow-rule bypasses and CI/CVE failures.

## 2. Releases

- **v0.23.0** — https://github.com/QwenLM/qwen-code/releases/tag/v0.23.0  
  The only feature noted in the changelog: the branch picker now displays git state hints such as `↓3 · origin/main` or `Up to date` beside Update Project, Commit, and Push. No known breaking changes.

## 3. Hot Issues

1. [**#8662** — Migrate TUI rendering layer from ink to OpenTUI (tracking)](https://github.com/QwenLM/qwen-code/issues/8662)  
   The long-running tracking issue for replacing the heavily patched ink + React 19 renderer with OpenTUI. It has the highest engagement today at 28 comments, reflecting how much terminal flicker/rendering pain the current architecture causes.

2. [**#10065** — LM Studio 0.4.21: “failed to parse grammar” even with no MCP servers and tools.core=[]](https://github.com/QwenLM/qwen-code/issues/10065)  
   Local-model users fail against LM Studio’s OpenAI-compatible endpoint with zero tools configured. A P2 blocker for anyone running Qwen Code against LM Studio locally.

3. [**#10162** — Degrade gracefully when the ACP NDJSON channel queue saturates](https://github.com/QwenLM/qwen-code/issues/10162)  
   `qwen serve` currently tears down the whole channel when the decoded NDJSON queue guard fires. The production behavior is too aggressive; discussion favors bounded degradation over fail-closed teardown.

4. [**#10908** — CI test time is bound by module import cost, not scheduling](https://github.com/QwenLM/qwen-code/issues/10908)  
   A telling data point: the CLI workspace spent 2223s collecting tests vs. 1372s running them. CI optimization is shifting from scheduling to reducing import/collection overhead.

5. [**#10953** — Todo plan state goes stale while work is delegated to subagents](https://github.com/QwenLM/qwen-code/issues/10953)  
   A persisted Todo plan was frozen for 55m44s while subagents advanced through four plan nodes. The active-todo reminder never fires because plan state is not refreshed during subagent handoff.

6. [**#10791** — Balanced content-only `<thinking>` blocks still leak to user-visible output](https://github.com/QwenLM/qwen-code/issues/10791)  
   Previous defenses only caught unclosed thinking tags. Properly balanced `<thinking>...</thinking>` emitted on content-only turns still reaches the user. This is a direct content-hygiene regression.

7. [**#9666** — Terminal IME candidate box low contrast when typing Chinese on Windows](https://github.com/QwenLM/qwen-code/issues/9666)  
   Chinese input on Windows/PowerShell is nearly unusable because candidate text has very low contrast. A longstanding UI pain point, especially for CJK users.

8. [**#10932** — Voice dictation cannot use Token Plan ASR](https://github.com/QwenLM/qwen-code/issues/10932)  
   The voice pipeline hardcodes old model IDs and rejects `qwen-audio-3.0-asr-flash`, so Token Plan ASR is unavailable even though mic capture works. The fix is a model-ID allowlist update.

9. [**#10850** — CI: Dependency CVE audit fails repo-wide](https://github.com/QwenLM/qwen-code/issues/10850)  
   New advisories for `fast-uri` / `qs` / `uuid` broke `npm audit --omit=dev` on main: 4 vulnerabilities, including one high-severity issue. This is blocking repo-wide CI since 2026-09-02.

10. [**#10197** — Security: static loader environment assignments can bypass concrete Bash allow rules](https://github.com/QwenLM/qwen-code/issues/10197)  
    A P1 shell-security finding: stripping leading environment assignments before matching a saved `Bash(...)` allow rule can let attacker-controlled assignments change runtime semantics and execute extra code. Related bypass #10192 covers command substitution in the same position.

## 4. Key PR Progress

- [**#10982** — fix(core): demote balanced content-only thinking blocks to thought parts](https://github.com/QwenLM/qwen-code/pull/10982)  
  Directly closes #10791 by extending thinking-tag leak defense to balanced leading `<thinking>...</thinking>` blocks, routing them to thought parts instead of user-visible content.

- [**#10992** — fix(core): catch tool-result scaffolding and system-reminder echo leaks](https://github.com/QwenLM/qwen-code/pull/10992)  
  Addresses #10797’s two uncovered leak shapes: fake/echoed tool-result XML in content and system-reminder text reaching user-visible output.

- [**#10938** — feat(web-shell): make Session Workflow dependencies navigable and quiet its chrome](https://github.com/QwenLM/qwen-code/pull/10938)  
  Improves the Session Workflow plan DAG: navigation, dependency inspection, and a cleaner presentation with the step — not its status — leading each node.

- [**#10954** — feat(serve): expose the background agents the supervisor is running](https://github.com/QwenLM/qwen-code/pull/10954)  
  Adds `GET /background-agents` to `qwen serve`, returning the supervisor’s active sessions, names, states, and current activity. Improves daemon observability.

- [**#10962** — feat(web-shell): bridge a browser-granted local directory into a session](https://github.com/QwenLM/qwen-code/pull/10962)  
  When the daemon runs on a cloud box/container, this lets the browser user grant access to a local directory, making the remote agent see files the daemon otherwise could not reach.

- [**#10915** — ci: give every workspace the shared-pool test timeout](https://github.com/QwenLM/qwen-code/pull/10915)  
  Raises fifteen workspaces from vitest’s 5000 ms default and adds a parity sweep so new workspaces cannot silently inherit the too-low timeout.

- [**#10347** — feat(core): auto-retry transient network errors (EOF) where Ctrl+Y is unavailable](https://github.com/QwenLM/qwen-code/pull/10347)  
  Treats wrapped low-level EOF/network failures as retryable transport errors instead of fail-fast client errors, enabling bounded auto-retry in channels and daemon contexts.

- [**#10421** — fix(review): screen content filters before the probe tree’s restore too](https://github.com/QwenLM/qwen-code/pull/10421)  
  Review process hardening: prevents local git config content filters from executing during scratch-tree restore, closing another command-execution gap in review probes.

- [**#10940** — fix(cli): repair the live slash gate fallout on main](https://github.com/QwenLM/qwen-code/pull/10940)  
  Fixes two regressions introduced by the live slash submission gate: adds the missing action on the mock `memory` command and restores the missing `slashCommand` handling in unit tests.

- [**#9940** — fix(review): reply carried findings into their thread, resolve fixed ones](https://github.com/QwenLM/qwen-code/pull/9940)  
  Improves the review bot’s thread hygiene: re-posted findings now become replies in the original thread, and fixed findings are resolved on the PR instead of being re-opened.

## 5. Feature Request Trends

- **Output middleware and content rewriting**  
  The clearest new direction is a public pluggable middleware API for transforming reasoning/thinking output before it reaches clients ([#10872](https://github.com/QwenLM/qwen-code/issues/10872)). Work on balanced thinking leaks ([#10791](https://github.com/QwenLM/qwen-code/issues/10791)) and scaffolding echoes ([#10797](https://github.com/QwenLM/qwen-code/issues/10797)) shows users want stronger, extensible control over what the model emits.

- **Remote-first daemon and web-shell workflow**  
  Multiple requests point to making `qwen serve` viable for cloud/remote setups: standalone sessions without a workspace ([#8908](https://github.com/QwenLM/qwen-code/issues/8908)), browser-granted local directory access ([#10962](https://github.com/QwenLM/qwen-code/pull/10962)), shell/monitor output in the web UI ([#10906](https://github.com/QwenLM/qwen-code/pull/10906)), and propagating prompt authority into the VS Code companion ([#10989](https://github.com/QwenLM/qwen-code/issues/10989)).

- **Terminal UX modernization**  
  The OpenTUI migration ([#8662](https://github.com/QwenLM/qwen-code/issues/8662)) is the umbrella request for replacing the current ink renderer. Adjacent asks include fixing Windows IME contrast ([#9666](https://github.com/QwenLM/qwen-code/issues/9666)) and making CLI configuration process-scoped via `--config-dir` ([#10984](https://github.com/QwenLM/qwen-code/issues/10984)).

- **CI/review automation reliability**  
  There is steady demand for faster and more reliable CI: module-import-bound test collection ([#10908](https://github.com/QwenLM/qwen-code/issues/10908)), uniformly raised vitest timeouts ([#10915](https://github.com/QwenLM/qwen-code/pull/10915)), and automated follow-up tracking for review findings ([#10977](https://github.com/QwenLM/qwen-code/issues/10977), [#10922](https://github.com/QwenLM/qwen-code/issues/10922)).

## 6. Developer Pain Points

- **Internal model output leaking to users**  
  Thinking tags, tool-result scaffolding, and system reminders keep surfacing in user-visible output ([#10791](https://github.com/QwenLM/qwen-code/issues/10791), [#10797](https://github.com/QwenLM/qwen-code/issues/10797)). Sanitizers keep having to cover new shapes after release, which erodes trust in content-only/hybrid-thinking modes.

- **Shell security rules are still bypassable**  
  Multiple P1 findings show saved `Bash(...)` allow rules can be bypassed through environment assignment semantics ([#10197](https://github.com/QwenLM/qwen-code/issues/10197), [#10192](https://github.com/QwenLM/qwen-code/issues/10192)), and command-execution config keys remain an open entrance set ([#10561](https://github.com/QwenLM/qwen-code/issues/10561)). This is the most safety-critical recurring theme.

- **CI and dependency health churn**  
  Release runs are bottlenecked on Python/TS module import time ([#10908](https://github.com/QwenLM/qwen-code/issues/10908)), the ECS runner fleet can go stale ([#10911](https://github.com/QwenLM/qwen-code/issues/10911)), and dependency CVE audits break repo-wide overnight ([#10850](https://github.com/QwenLM/qwen-code/issues/10850)).

- **Local model integration friction**  
  LM Studio users hit “failed to parse grammar” even with no MCP/tools ([#10065](https://github.com/QwenLM/qwen-code/issues/10065)), Token Plan ASR is rejected by hardcoded IDs ([#10932](https://github.com/QwenLM/qwen-code/issues/10932)), and Token Plan setup documentation is still missing ([#10620](https://github.com/QwenLM/qwen-code/issues/10620)).

- **Agent/subagent state desynchronization**  
  Todo plan state freezes during delegated subagent work ([#10953](https://github.com/QwenLM/qwen-code/issues/10953)), daemon channel queues can tear down an entire conversation ([#10162](https://github.com/QwenLM/qwen-code/issues/10162)), and web-shell loading indicators still lose sync with the daemon prompt authority ([#9645](https://github.com/QwenLM/qwen-code/issues/9645), [#10989](https://github.com/QwenLM/qwen-code/issues/10989)).

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

## DeepSeek-TUI / Codewhale Community Digest — 2026-09-04

Data source: `Hmbown/DeepSeek-TUI`; linked issues/PRs resolve to `Hmbown/Codewhale`.

### Today’s Highlights

Activity was dominated by merging Codewhale 0.9.12 UX slices and feature-branch re-lands onto `main`: PRs #5862, #5858, #5843, and #5833 closed, while FEAT-020 plugin command shapes are being re-landed in #5865. No release was published in the last 24 hours. The clearest open problem is incomplete ACP support: editor clients still cannot expose session configuration or enumerate/resume existing sessions via `serve --acp`.

---

### Releases

No releases published in the last 24 hours.

---

### Hot Issues

Only four issues were updated in this window; all are included.

- **[#5316 — EPIC-005: CodeWhale TUI Crate Decomposition (Umbrella)](https://github.com/Hmbown/Codewhale/issues/5316)**  
  Author: `aboimpinto` · 21 comments · Open  
  This is the umbrella epic for the CodeWhale TUI crate decomposition. It tracks sub-epics, feature slices, and related PRs, including FEAT-020 command-shape re-landing and memory work. The age plus high comment count makes it the main structural refactor coordination point for the codebase.

- **[#5863 — [enhancement] ACP Function Enhancement](https://github.com/Hmbown/Codewhale/issues/5863)**  
  Author: `Lujc0523` · 2 comments · Open  
  `serve --acp` does not expose session config options such as modes, models, or `configOptions`. This blocks editor clients from displaying or changing the working mode through ACP. Likely a common friction point for anyone building ACP-connected editor UIs.

- **[#5864 — `serve --acp` does not implement ACP `session/list` / `session/load`](https://github.com/Hmbown/Codewhale/issues/5864)**  
  Author: `senka9h` · 1 comment · Open  
  Companion issue to #5863: ACP clients cannot enumerate prior Codewhale sessions or resume them. This makes session persistence and handoff impossible for editor integrations, reinforcing the need for fuller ACP lifecycle support.

- **[#5866 — Key Ophthalmology CPT & ICD-10 Updates for 2026](https://github.com/Hmbown/Codewhale/issues/5866)**  
  Author: `medicalbilling-usa` · 1 comment · Open  
  Appears to be unrelated medical-billing link spam rather than a product issue. It likely needs moderation or closure as off-topic.

---

### Key PR Progress

Eight PRs were updated in this window; all are included.

- **[#5869 — fix(shell): preserve task origin in job snapshots](https://github.com/Hmbown/Codewhale/pull/5869)** · Open  
  Author: `zhuowp`  
  Fixes missing stable origin identifiers for background shell job snapshots. Without this, host-side heuristics can mismatch job updates and incorrectly project an earlier job’s error output onto a newer tool card.

- **[#5868 — feat: send `x-opencode-session` header for OpenCode Go/Zen providers](https://github.com/Hmbown/Codewhale/pull/5868)** · Open  
  Author: `huangxianzhan`  
  Adds the `x-opencode-session` header expected by OpenCode Go. This should improve prompt caching and traffic attribution; it also addresses UA handling for Codewhale clients.

- **[#5867 — feat(config): add `[reasoning_only]` section for retry count and custom…](https://github.com/Hmbown/Codewhale/pull/5867)** · Open  
  Author: `Gabriel-Degret`  
  Makes reasoning-only retry behavior configurable. Previously, the engine silently retried exactly twice when a reasoning model returned hidden thinking with no answer or tool call. This PR exposes that behavior via a `[reasoning_only]` config section.

- **[#5865 — refactor(tui): re-land FEAT-020 plugin command shapes on `main`](https://github.com/Hmbown/Codewhale/pull/5865)** · Open  
  Author: `aboimpinto`  
  Re-lands the original FEAT-020 implementation from #5657 onto current `main`. The underlying work remains tracked by umbrella #5316, which stays open for remaining command-decomposition work.

- **[#5833 — feat(memory): FEAT-019 memory capability, memory facet, and typed outcomes](https://github.com/Hmbown/Codewhale/pull/5833)** · Closed  
  Author: `Hmbown`  
  Closes #5609. Re-lands the FEAT-019 memory slice, adding a `CommandCapabilities::MEMORY` bit, `CommandMemoryContext` facet, and TUI memory adapter with typed outcomes for search, remember, get, export, reindex, and delete.

- **[#5858 — tui: collapse `ocean_treatment` into `ThemeId::Underwater`](https://github.com/Hmbown/Codewhale/pull/5858)** · Closed  
  Author: `Hmbown`  
  Consolidates the short-lived ocean theme split into a single underwater theme. Includes locale strings, mark assets, config migration, OceanRamp theme keys, and repaint/routing updates.

- **[#5862 — Codewhale 0.9.12: Fleet-only UX](https://github.com/Hmbown/Codewhale/pull/5862)** · Closed  
  Author: `Hmbown`  
  Integrates 10 UX slices for the 0.9.12 release: workbar rename, startup experience, underwater theme default, provider/settings/logo updates, unified hover contract, role UI, and retro theme polish.

- **[#5843 — tui: align typed config and schema with live value spaces](https://github.com/Hmbown/Codewhale/pull/5843)** · Closed  
  Author: `Hmbown`  
  Low-risk config cleanup: typed themes now carry custom themes, orphaned locale keys were removed, and typed config/schema definitions were realigned with actual runtime values.

---

### Feature Request Trends

- **ACP protocol completeness**  
  Multiple issues request more complete ACP support: exposing session modes/models/config options (#5863) and implementing session enumeration/loading (#5864). The direction is clear: editor clients need full conversational control, not just message streaming.

- **Runtime configurability**  
  PR #5867 reflects a desire to replace hardcoded engine constants with user-facing configuration sections. Expect more requests for policy knobs around retries, timeouts, and reasoning fallbacks.

- **Provider integration polish**  
  PR #5868 highlights demand for deeper provider compatibility, including provider-specific session headers and caching optimizations.

- **Architecture and command decomposition**  
  EPIC #5316 remains the long-running driver for crate decomposition and command-shape refactoring. Workbar/theme/memory slices continue to flow into this umbrella as the TUI matures.

---

### Developer Pain Points

- **ACP gaps block editor workflows**  
  The sharpest recurring pain is that `serve --acp` is not yet a complete bridge for editor clients: they cannot switch working modes, set config options, or resume prior sessions.

- **Session/job identity ambiguity**  
  Shell background jobs lack stable origin identifiers, forcing clients to reconcile by command text. That can misattribute errors from older jobs onto the currently displayed tool card.

- **Silent and non-configurable reasoning retries**  
  Hardcoded retry behavior when reasoning models return only hidden thinking is frustrating and difficult to tune without source changes.

- **Feature-branch churn**  
  Several PRs are re-lands or consistency fixes after previous work landed on integration branches. This signals maintainers are actively cleaning up branch drift and aligning config/schema/theme state across `main`.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*