# AI CLI Tools Community Digest 2026-09-05

> Generated: 2026-09-05 03:59 UTC | Tools covered: 9

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

# Cross-Tool Comparison Report: AI CLI Developer Tools — 2026-09-05

## 1. Ecosystem Overview

The AI CLI coding-agent market is visibly shifting from “best model” competition to platform-level competition: session lifecycle, plugin extensibility, permission truthfulness, and Windows reliability. Ten releases crossed the nine tracked repos in the 24-hour digest window, most of them regression fixes or ecosystem integrations (GitHub taskbar cards, Bedrock/GPT-6-Astra model pickers, OAuth fixes) rather than new model capabilities. Across all communities, the hottest threads concern desktop stability, MCP breakage, permission/sandbox mismatches, forced auto-updates, and token/cost accounting. A new consensus is emerging: extensibility via hooks/event streams and honest agent state reporting are the next competitive moats. The strongest community signals reward tooling that is auditable, restorable, and predictable — not merely fast.

## 2. Activity Comparison

*Counts represent issues/PRs surfaced as significant in the 24h community digests — not raw repository totals.*

| Tool | Notable Issues (24h) | Notable PRs (24h) | Releases (24h) | Dominant theme |
|---|---|---|---|---|
| Claude Code | 10 | 2 | v2.1.261 | Windows launch/auto-update debt; function-hooks demand spikes |
| OpenAI Codex | 10+ | 10 | v0.153.3, v0.153.4 | Fast iteration vs. Windows desktop fragility |
| Gemini CLI | 10 (4 labeled p1) | 10 | v0.60.0-nightly | Sandbox/security hardening; false-success agent reporting |
| GitHub Copilot CLI | 10 | 1* | v1.0.83, v1.0.84-0, v1.0.84-1 | Release-regression detection (MCP, caching cost, permissions) |
| Kimi Code CLI | 1 | 1 | none | Quiet window; Windows Terminal paste defect |
| OpenCode | 10 (8 resolved) | 10 | v1.18.28, v1.18.29 | Cross-ecosystem compatibility + fast regression triage |
| Pi | 10 (4 = duplicates of one packaging blocker) | 13 | v0.85.0 | npm tarball broken; provider/gateway breadth |
| Qwen Code | 10 | 10 | none | Session/transcript identity; OpenTUI migration & CI cost |
| DeepSeek TUI | 4 (+1 promotional spam) | 10 | none | Local-model context-budget bug; housekeeping |

\* Copilot CLI shows minimal OSS PR motion; development likely flows through internal pipelines rather than the public repo.

**Velocity read:** Codex, Copilot CLI, and OpenCode are shipping at near-daily/hotfix cadence. Gemini CLI operates on a nightly security-hardening track. Claude Code releases deliberately and fields the most heavily commented issues. Kimi Code and DeepSeek TUI had effectively dormant windows.

## 3. Shared Feature Directions

### 3.1 Plugin/hook/event extensibility — “hooks are the new API”
- **Claude Code:** Function Hooks proposal (#91870) — intercept/modify tool calls mid-execution with `next()` composition; 62 👍 in two days, the fastest-climbing feature request.
- **OpenCode:** Native Claude Code hooks compatibility (#12472, 40 👍) is the top open feature; plugin authors also want session/global event streams and child-session visibility (PR #46690, #29175).
- **Pi:** Final pre-execution hook for permission-style extensions (#9175); system-message deltas so plugins can change tools mid-session (PRs #9116/#9117).
- **Gemini CLI:** Users want the agent to proactively use installed skills/subagents (#21968) and share subagent trajectories (#22598).
- **Qwen Code:** Public middleware hooks to rewrite reasoning output before emission (#10872); parity with Claude Code dynamic workflows (#11013).

### 3.2 Context-budget and cost engineering
- **Copilot CLI:** Fixed ~20.5K-token system prompt (#2627); configurable auto-compaction threshold (#1688); BYOK silently losing prompt caching → ~5× cost (#4720).
- **Claude Code:** Configurable/suppressible MEMORY.md compaction threshold (#91188); restore early context-ring warnings (#91385).
- **Codex:** GPT-5.6 serializing independent Code Mode calls costs users 27–45% extra weighted usage (#35050).
- **Gemini CLI:** AST-aware reads to reduce token noise (#22745); 400 errors when >128 tools are advertised (#24246).
- **OpenCode:** Auto-compaction loops draining tokens even in empty folders (#30680).
- **DeepSeek TUI / Pi:** Local-model budget clamping (Ollama 32K models reduced to 1,024-token input, #5820); OpenRouter `:free` models rejected due to over-limit `max_tokens` (#8760).

### 3.3 Permission-model truthfulness
The pattern repeats across nearly every tool: what the UI/config promises diverges from what the sandbox enforces.

- **Codex:** UI shows “Full Access,” sandbox silently enforces `workspace-write` after resume (#25590); child worktrees don’t inherit auto-approval (#33282); Full Access children downgraded to managed approval (#40125).
- **Claude Code:** `bypassPermissions` mode still prompts on `cd DIR && grep` when a `Read()` deny rule exists (#91683); denial messages don’t name the rule/settings file that fired (#87153).
- **Copilot CLI:** ACP mode auto-approves tool calls again (#4537) — regression of a previously fixed permission-safety issue.
- **Gemini CLI:** Hardening sandbox boundaries, isolating credentials, and requiring consent for extension-driven env changes (PRs #29214/#29216/#28863; shipped in nightly).

### 3.4 MCP lifecycle and interoperability
- **Claude Code:** Lazy/on-demand MCP connections and per-session scoping to stop RAM/startup bloat (#63251, #82952).
- **Copilot CLI:** Version-to-version MCP breakage (#4525, #4647); stuck `tools/list` refresh permanently strips tools (#4731).
- **OpenCode:** Remote MCP connectivity regression introduced in v1.18.28 (#47368) and fixed within the same digest window.
- **DeepSeek TUI:** Major `rmcp` SDK bump (2.2.0 → 3.2.0) requiring breaking-change review (#5877).
- **Gemini CLI:** Defending against prompt injection via MCP/external tool payloads by enforcing envelope-metadata provenance (#29215).

### 3.5 Windows as a first-class requirement
Windows-specific issues dominate across 8 of 9 repos:

- **Claude Code:** Launch failures from orphaned process locks (#42776, 159 comments; #53247); forced auto-update restarts (#92246).
- **Codex:** Handshake crash breaking code mode (#41049); EFS-blocked bundled plugins (#25220); WSL project creation failures (#41463); pets overlay hit-testing cluster.
- **Copilot CLI:** Auto-updater rewriting the desktop app’s bundled `copilot.exe` (#4728); WSL2 `ctrl+h` misparse (#4328).
- **Kimi Code CLI:** Ctrl+V paste fails in Windows Terminal + PowerShell (#2634).
- **Gemini CLI:** NTFS 8.3 short-name path-bypass fix (PR #29116).
- **OpenCode:** Desktop paste crashes; local-model JSON errors on Windows.

## 4. Differentiation Analysis

**Ecosystem-native agents.** Claude Code targets organizations standardizing on configuration-driven, policy-aware autonomous coding: deepest permission/settings surface, memory files, org-policy diagnostics, and now a serious function-hooks roadmap. Its weakness is desktop lifecycle reliability on Windows. OpenAI Codex pushes the “always-on desktop agent running in multiple sandboxes” model: background tasks, remote control, session resumption, and a native Windows MXC sandbox adapter in progress. It ships fastest but breaks Windows flows most visibly. GitHub Copilot CLI differentiates through GitHub-native identity: `.agent.md` custom agents with ordered model fallbacks and `model-policy: required`, MCP OAuth via Client ID Metadata Documents, sandboxed `gh`, and Windows 11 live session cards. For teams already living in GitHub/Copilot, it is the most administratively coherent option; its public OSS PR signal, however, is thin.

**Cloud/lab-aligned CLIs.** Gemini CLI is taking a security-engineer’s path: OS-level sandboxing, settings-directory isolation, symlink/NTFS boundary hardening, and consent before extension-driven environment changes. It is the most defensively architected tool in the set, at some cost to feature breadth. Qwen Code is oriented toward Alibaba’s Model Studio ecosystem and persistent daemon/web-shell/channel workflows (e.g., DingTalk), with strong session-identity and transcript-integrity work — a fit for enterprise internal automation rather than the Western OSS aesthetic. Kimi Code remains minimal and undifferentiated so far; its one issue this window (Windows Terminal paste) shows it is still chasing basic terminal parity.

**Independent / OSS layer.** OpenCode’s strategy is explicit neutrality and compatibility: it consumes Claude Code hooks (pending), Codex OAuth, and GitHub Copilot session headers, plus a growing plugin API. It is the most credible “drop-in replacement across vendors” candidate, and its fast issue-closing record shows responsive triage — though its own rapid releases occasionally introduce regressions. Pi is a craft-driven, provider-broad terminal product: obsessive UX details, broad model/gateway catalog support, and extension-API growth, but its small release pipeline just shipped a broken npm tarball. DeepSeek TUI serves the Rust/local-model niche: minimal dependencies, Ollama focus, low community volume.

## 5. Community Momentum & Maturity

**Tier 1 — largest, most engaged communities:**
- **Claude Code** has the strongest engagement depth: top issue at 159 comments/75 👍, and the function-hooks proposal hit 62 👍 in two days. Its release cadence is measured; but the Windows/auto-update issue cluster shows accumulated platform debt.
- **Codex** shows the most frenetic engineering momentum: 2 patch releases and ~10 notable PRs in 24h (async TUI questions, Astra rollout, sandbox work). Community threads on Windows defects are long (46–59 comments) but young.
- **Copilot CLI** ships daily and draws steady regression reports. Its community is broad by distribution but shallow in OSS participation; issues here are fast, concrete, and cost-focused.

**Tier 2 — rapidly iterating, purposeful communities:**
- **Gemini CLI** runs a disciplined nightly/security-hardening process and closes meaningful p1 bugs promptly; issue volume is smaller but well-labeled.
- **OpenCode** is highly responsive (8 of 10 surfaced issues already resolved in the window) and has real feature-request gravity (Claude hooks compatibility at 40 👍). The risk is release hygiene: three regressions in recent point releases.
- **Qwen Code** generates substantial PR volume without a release — its OpenTUI migration is absorbing engineering capacity, and CI collection time (2,223s) is a recognized internal bottleneck.

**Tier 3 — niche/small communities:**
- **Pi** has a passionate, UX-focused following and strong PR discipline (fix PRs appeared the same day as the packaging regression), but the duplicate-issue cluster shows a small user base with no release-testing safety net.
- **Kimi Code** and **DeepSeek TUI** had quiet windows; both are better assessed over longer timeframes. DeepSeek TUI’s PR list is dominated by routine dependency bumps.

## 6. Trend Signals

1. **Agent infrastructure, not model IQ, is now the differentiator.** Issues about completion honesty (Gemini subagents reporting `GOAL` on `MAX_TURNS`, #22323), session resumption across CLI/desktop boundaries (Claude #92016), and orphaned session workers (Qwen #11063) outrank model-quality complaints. Buyers should evaluate how a tool reports success and restores state, not just how it codes.

2. **Token/cost accounting is becoming a purchase criterion.** Developers are quantifying serialized calls (+27–45% waste on Codex), missing prompt-cache declarations (~5× cost on Copilot BYOK), and fixed system-prompt overhead (20.5K tokens). Expect demand for per-session cost dashboards, cache-TTL-aware compaction, and batching controls to grow.

3. **Permission-state trust is fragile everywhere.** When the UI says “Full Access” and the sandbox disagrees, or `bypassPermissions` still prompts, developers lose trust in the entire automation layer. The tools that ship a coherent permission model across CLI, desktop, threads, and subagents will win the enterprise automation segment.

4. **Windows is the battleground for adoption.** Every major tool has an open Windows-reliability debt: launch locks, EFS-blocked plugins, handshake crashes, NTFS short-name bypasses, broken paste. For tool vendors, Windows-native sandbox adapters and update orchestration are now table stakes.

5. **Auto-update is a trust-breaking liability.** Forced restarts (Claude #92246), self-updaters corrupting the hosting app’s binary (Copilot #4728), and broken npm tarballs (Pi #9132) all destroy session continuity. Deferral, opt-out, rollback, and staged rollout are becoming hard requirements for professional use.

6. **MCP remains the standard — but the ecosystem is still immature.** Every client-facing repo reports MCP breakage: SDK churn, discovery regressions, missing tools after refresh, and startup RAM bloat. Lazy per-session MCP lifecycle management is an open product opportunity across the entire category.

7. **Local-model and budget-model demand is real but underserved.** Ollama context clamping, OpenRouter free-tier rejections, and local-plugin rough edges show a growing segment of users who want control of cost and data locality. Tools that deliver reliable context-window negotiation for local models will find an uncontested niche.

**Bottom line for developers and decision-makers:** choose tools that are honest about permissions and completions, expose token/cost behavior, treat Windows as a first-class platform, and provide controlled update/extension lifecycles. In the current landscape, Claude Code offers the deepest configuration and community gravity; Codex contributes the fastest iteration; OpenCode is the strongest neutrality play; and Gemini CLI is setting the security bar the rest will have to match.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills Community Highlights — 2026-09-05

*Source ordering follows GitHub comment activity. All listed PRs are currently open unless otherwise noted.*

---

## 1. Top Skills Ranking

These are the most-discussed Skills-related PRs in the repository, reflecting both new Skills and critical fixes to existing Skill tooling.

- **#1298 — Fix `skill-creator` evaluation pipeline** · [PR #1298](https://github.com/anthropics/skills/pull/1298)  
  **Functionality:** Fixes `run_eval.py` so skill descriptions are not incorrectly scored as 0% recall. Includes Windows subprocess fixes, trigger detection fixes, and parallel-worker handling.  
  **Discussion highlights:** The eval harness was reporting `recall=0%` for every description, making downstream `run_loop.py` and `improve_description.py` optimize against meaningless noise.  
  **Status:** Open.

- **#514 — Add `document-typography` skill** · [PR #514](https://github.com/anthropics/skills/pull/514)  
  **Functionality:** Typographic quality control for AI-generated documents, targeting orphaned words, stranded section headers, and numbering misalignment.  
  **Discussion highlights:** Positions typography QA as a broadly reusable document-quality layer for Claude-generated output.  
  **Status:** Open.

- **#1615 — Add `scnet-hpc` skill** · [PR #1615](https://github.com/anthropics/skills/pull/1615)  
  **Functionality:** Operates SCNet HPC clusters through profile-based SSH/Slurm workflows, including job generation, cluster discovery, and compute-node guidance.  
  **Discussion highlights:** HPC workflows remain an underserved automation area in the Skills ecosystem.  
  **Status:** Open.

- **#538 — Fix case-sensitive references in `pdf` skill** · [PR #538](https://github.com/anthropics/skills/pull/538)  
  **Functionality:** Corrects eight `REFERENCE.md`/`FORMS.md` → `reference.md`/`forms.md` filename mismatches in the PDF skill docs.  
  **Discussion highlights:** Breaks SKILL.md resolution on case-sensitive filesystems; simple but highly visible portability fix.  
  **Status:** Open.

- **#486 — Add `odt` skill** · [PR #486](https://github.com/anthropics/skills/pull/486)  
  **Functionality:** Create, fill, read, and convert OpenDocument files (`.odt`, `.ods`, `.odf`), including template filling and ODT-to-HTML parsing.  
  **Discussion highlights:** Extends document-format coverage beyond DOCX/PDF into LibreOffice-compatible formats.  
  **Status:** Open.

- **#210 — Improve `frontend-design` skill clarity** · [PR #210](https://github.com/anthropics/skills/pull/210)  
  **Functionality:** Revises the frontend-design skill so its instructions are more actionable, internally coherent, and executable within a single conversation.  
  **Discussion highlights:** Focused on reducing vague guidance and making the skill steer Claude behavior concretely.  
  **Status:** Open.

- **#83 — Add `skill-quality-analyzer` and `skill-security-analyzer`** · [PR #83](https://github.com/anthropics/skills/pull/83)  
  **Functionality:** Adds two meta-skills: a quality analyzer covering structure/docs/examples/resources, and a security analyzer for evaluating Skill trustworthiness.  
  **Discussion highlights:** Directly responds to community concerns about Skill quality and supply-chain security.  
  **Status:** Open.

- **#541 — Fix DOCX tracked-change `w:id` collisions** · [PR #541](https://github.com/anthropics/skills/pull/541)  
  **Functionality:** Prevents document corruption when DOCX skills add tracked changes to files that already contain bookmarks/comments.  
  **Discussion highlights:** Fixes a subtle OOXML shared-ID-space bug that could corrupt real-world Word documents.  
  **Status:** Open.

---

## 2. Community Demand Trends

Issue activity points to strong demand for ecosystem reliability and governance, not only new content Skills.

- **Security and trust boundaries** — The most active issue, [Issue #492](https://github.com/anthropics/skills/issues/492) with 43 comments, concerns community Skills being distributed under the `anthropic/` namespace and impersonating official Skills. There is clear demand for security review, official distribution controls, and trust signaling.
- **Enterprise sharing and distribution** — [Issue #228](https://github.com/anthropics/skills/issues/228) asks for org-wide Skill libraries and shared installation links rather than manual file transfer. This signals a platform/distribution gap for team adoption.
- **Skill-creator reliability** — [Issue #556](https://github.com/anthropics/skills/issues/556) reports the evaluator never triggers Skills, and [Issue #202](https://github.com/anthropics/skills/issues/202) calls for skill-creator to be rewritten around actionable operational guidance. Demand: trustworthy authoring/evaluation tooling.
- **Context-window economy** — [Issue #1487](https://github.com/anthropics/skills/issues/1487) reports a `claude-api` Skill injecting ~156k tokens; [Issue #189](https://github.com/anthropics/skills/issues/189) reports duplicate plugin content inflating context. The community is actively pushing for leaner Skills.
- **Agent memory and governance** — The clearest new-Skill proposals with traction are [compact-memory](https://github.com/anthropics/skills/issues/1329), [agent-governance safety patterns](https://github.com/anthropics/skills/issues/412), and [reasoning quality-gate pipelines](https://github.com/anthropics/skills/issues/1385).
- **Platform interoperability** — [Issue #16](https://github.com/anthropics/skills/issues/16) asks to expose Skills as MCPs, and [Issue #29](https://github.com/anthropics/skills/issues/29) requests Bedrock support. Skills-as-protocol remains a recurring architectural request.

---

## 3. High-Potential Pending Skills

These active, open PRs add new Skills or substantial upgrades and appear likely to land next based on community attention.

- **#514 — `document-typography`** · [PR #514](https://github.com/anthropics/skills/pull/514)  
  Broadly applicable document-quality Skill for typographic issues in generated documents.

- **#486 — `odt` skill** · [PR #486](https://github.com/anthropics/skills/pull/486)  
  Adds OpenDocument creation, template filling, and conversion workflows.

- **#83 — Skill quality/security analyzers** · [PR #83](https://github.com/anthropics/skills/pull/83)  
  Meta-skills for evaluating Skill quality and security; aligned with the community’s strongest trust-related concerns.

- **#723 — `testing-patterns` skill** · [PR #723](https://github.com/anthropics/skills/pull/723)  
  Comprehensive testing guidance covering Testing Trophy philosophy, unit tests, React Testing Library, and broader test strategy.

- **#568 — `servicenow` platform skill** · [PR #568](https://github.com/anthropics/skills/pull/568)  
  Broad ServiceNow assistant coverage across ITSM, ITOM, SAM, SecOps, SPM, and IntegrationHub.

- **#525 — `pyxel` retro game development skill** · [PR #525](https://github.com/anthropics/skills/pull/525)  
  Workflow for building retro/pixel-art 8-bit games with Pyxel and `pyxel-mcp`.

- **#1628 — `hivemind` multi-agent orchestration skill** · [PR #1628](https://github.com/anthropics/skills/pull/1628)  
  Lets Claude Code delegate mechanical work to cheap headless workers while retaining planning/review/merge control.

- **#1367 — `self-audit` skill** · [PR #1367](https://github.com/anthropics/skills/pull/1367)  
  Mechanical verification plus four-dimension reasoning audit before delivering AI output.

---

## 4. Skills Ecosystem Insight

The community’s most concentrated Skills-level demand is not for any single domain but for **ecosystem reliability**: secure distribution, trustworthy evaluation, context-window efficiency, and quality/audit gates for Skills themselves.

---

# Claude Code Community Digest — 2026-09-05

## 1. Today's Highlights

Windows desktop reliability remains the dominant community theme this week: two long-running launch-failure bugs ([#42776](https://github.com/anthropics/claude-code/issues/42776), [#53247](https://github.com/anthropics/claude-code/issues/53247)) keep accumulating comments, and a fresh report of nine forced auto-update restarts in nine days ([#92246](https://github.com/anthropics/claude-code/issues/92246)) is sharpening calls for opt-out controls. On the positive side, v2.1.261 ships org-policy diagnostics plus new output-capture caps, and the Function Hooks proposal ([#91870](https://github.com/anthropics/claude-code/issues/91870)) has become the most-liked feature discussion in weeks — a strong signal that power users want deeper plugin extensibility.

## 2. Releases

**v2.1.261** ([release](https://github.com/anthropics/claude-code/releases))
- `/status` and `claude doctor` now include an "Organization policy" line explaining why the org policy could not be loaded (e.g., a proxy not forwarding the endpoint).
- Added `bashOutputMaxChars` and `taskOutputMaxChars` settings to raise how much command and background task output is captured.

## 3. Hot Issues

- [#42776 — Claude Code Desktop fails to relaunch on Windows due to orphaned process file lock](https://github.com/anthropics/claude-code/issues/42776) — Open since April, this is the single most-commented issue on the tracker (159 comments, 75 👍). Every Windows update cycle risks relaunch failure until the orphaned process is cleared, making it the clearest symbol of the platform's reliability debt.

- [#91870 — Function Hooks: make plugins 10x more powerful](https://github.com/anthropics/claude-code/issues/91870) — A proposal to let plugins intercept and modify Claude Code mid-execution using side-effect-tracked hooks and an Express/Koa-style `next()` composition model. 100 comments and 62 👍 in two days indicate this is the feature direction the community most wants — and it appears maintainers are actively soliciting design feedback.

- [#53247 — Claude Desktop fails to launch on Windows — orphaned Silo/Job Object after crash](https://github.com/anthropics/claude-code/issues/53247) — Related to #42776, with 60 comments. Users hit `HRESULT 0x80070020` (AppModel-Runtime EventID 215/208) and only logoff or reboot recovers, confirming the Windows launch bug is broader than a single process lock.

- [#91650 — Bash cd-compound-read guard prompts on absolute cd targets whenever a Read() deny rule exists](https://github.com/anthropics/claude-code/issues/91650) — A regression in 2.1.257–2.1.259 on Windows Git Bash. Only 10 comments but 56 👍 — a very high reaction-to-comment ratio — showing how disruptive permissions false positives are to daily workflows.

- [#81658 — Cross-platform sync failure causing Cowork conversations and chats to disappear](https://github.com/anthropics/claude-code/issues/81658) — 16 comments on a suspected server-side incident where Desktop/Web/Android sync drops conversations entirely. Data-loss reports escalate quickly in developer trust, even if root cause turns out to be backend-side.

- [#91683 — bypassPermissions mode now prompts on `cd DIR && grep …` when a Read() deny rule is configured](https://github.com/anthropics/claude-code/issues/91683) — Regression in 2.1.259 (Windows/macOS) with 26 👍. Users explicitly in bypass mode are getting prompted anyway — a correctness issue that erodes confidence in the permission model.

- [#89467 — Windows: app window is always-on-top with no way to disable it](https://github.com/anthropics/claude-code/issues/89467) — 15 comments, 10 👍. A minor but persistent ergonomic bug; on multi-monitor setups the desktop app constantly occludes other windows.

- [#91188 — Make the auto-memory MEMORY.md compaction reminder threshold configurable](https://github.com/anthropics/claude-code/issues/91188) — 20 comments on the hardcoded 200-line/25KB threshold that triggers compaction reminders. Users managing large memory files want control or suppression of the nag.

- [#92016 — Claude Desktop (Code tab) auto-denies CLI-native SendMessage, breaking subagent resumption](https://github.com/anthropics/claude-code/issues/92016) — macOS desktop v1.46388.1 silently denies `SendMessage` in every Code session, so CLI-started subagents can't be resumed from the desktop. Highlights the growing gap between CLI and desktop tool registries.

- [#92246 — Windows desktop app self-updates and restarts over a running session — nine forced restarts in nine days](https://github.com/anthropics/claude-code/issues/92246) — Filed today; only one comment so far, but the report is damning: no prompt, no deferral, no opt-out, and nine interrupted sessions in nine days on one machine. Expect this to become a rallying point for update-policy complaints.

## 4. Key PR Progress

Only two PRs were updated in the 24-hour window; both remain open.

- [#87079 — fix(security-guidance): make ** glob patterns match zero-depth paths](https://github.com/anthropics/claude-code/pull/87079) — Important security fix: `_glob_match` delegates to `fnmatch`, where `**/*.ts` requires a literal `/` and silently misses top-level files, causing `security-patterns.json` rules to not apply where the docstring promises "** matches any depth". Silent non-coverage of security rules makes this worth watching.

- [#61691 — Add diagnostic script for GitHub connector showing 'Connected' but no tools](https://github.com/anthropics/claude-code/pull/61691) — Adds a PowerShell diagnostic/repair script for Windows users hit by the recurring Cowork bug where the GitHub MCP connector reports `Connected` yet exposes zero tools (closes #61682). References a long issue chain (#28695, #41658, #5758x), suggesting a persistent connector problem on Windows.

## 5. Feature Request Trends

- **Deeper plugin extensibility via function hooks** ([#91870](https://github.com/anthropics/claude-code/issues/91870)) — intercept and modify tool calls mid-execution with side-effect tracking; the highest-velocity feature request this week.
- **MCP server lifecycle management** ([#63251](https://github.com/anthropics/claude-code/issues/63251), [#82952](https://github.com/anthropics/claude-code/issues/82952)) — Lazy/on-demand MCP connections and per-session scoping to eliminate the RAM and startup cost of booting every configured server in every session.
- **Memory and context-window configurability** ([#91188](https://github.com/anthropics/claude-code/issues/91188), [#91385](https://github.com/anthropics/claude-code/issues/91385)) — Make `MEMORY.md` compaction thresholds configurable/suppressible, and restore the context ring's early yellow-band warning before the prompt limit.
- **Permission transparency and granularity** ([#87153](https://github.com/anthropics/claude-code/issues/87153), [#92259](https://github.com/anthropics/claude-code/issues/92259)) — Denial messages should name the exact permission rule and settings file that fired; nested subagent definitions should be able to restrict which subagent types they can spawn.
- **Workflow ergonomics** ([#70610](https://github.com/anthropics/claude-code/issues/70610), [#87723](https://github.com/anthropics/claude-code/issues/87723)) — Model selection when spawning background task chips, and Cowork project chats ordered by last activity rather than creation date.

## 6. Developer Pain Points

- **Windows desktop update/relaunch instability** — A cluster of issues ([#42776](https://github.com/anthropics/claude-code/issues/42776), [#53247](https://github.com/anthropics/claude-code/issues/53247), [#89680](https://github.com/anthropics/claude-code/issues/89680)) shows orphaned processes and AppX container locks routinely forcing reboots; 0x80070020 failures are effectively a Windows-side rite of passage by now.
- **Forced auto-updates killing running work** ([#92246](https://github.com/anthropics/claude-code/issues/92246), [#89680](https://github.com/anthropics/claude-code/issues/89680)) — No prompt, deferral, or opt-out; nine forced restarts in nine days on one machine is the starkest data point yet.
- **CLI/desktop agent-tool parity regressions** ([#92016](https://github.com/anthropics/claude-code/issues/92016), [#92249](https://github.com/anthropics/claude-code/issues/92249)) — `SendMessage`/`ListAgents` are missing or auto-denied in desktop-started sessions, breaking subagent resumption and Remote Control workflows.
- **Permission engine regressions** ([#91650](https://github.com/anthropics/claude-code/issues/91650), [#91683](https://github.com/anthropics/claude-code/issues/91683)) — Git Bash `cd` compound commands trigger prompts even under `bypassPermissions` when any `Read()` deny rule exists; opaque denial messages ([#87153](https://github.com/anthropics/claude-code/issues/87153)) make these hard to debug.
- **MCP process and memory bloat** ([#82952](https://github.com/anthropics/claude-code/issues/82952), [#63251](https://github.com/anthropics/claude-code/issues/63251)) — Every session boots the full user-scope MCP server set up front; machines running multiple sessions report serious, avoidable RAM pressure.
- **Cowork sync and Remote Control fragility** ([#81658](https://github.com/anthropics/claude-code/issues/81658), [#90243](https://github.com/anthropics/claude-code/issues/90243), [#91991](https://github.com/anthropics/claude-code/issues/91991)) — Disappearing chats, stale pairings that truncate reachability scans, and "New session" attaching to the most recent session instead of creating one.

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-09-05

## 1. Today's Highlights

Codex shipped two rapid-fire patch releases within 24 hours — v0.153.3 added **GPT-6-Astra** to the Amazon Bedrock model picker, and v0.153.4 made Astra visible and the **bundled default model** in the desktop picker when no model is explicitly configured. Windows desktop stability dominates community discussion again, with top threads covering a missing Remote-control settings tab (#28919, 59 comments) and a code-mode handshake crash that breaks GPT-5.6 sessions (#41049, 46 comments). On the engineering side, a large wave of merged PRs brings full **asynchronous question support to the TUI** — selectable answers, an editable "Other" option, and question-state persistence across reconnects.

## 2. Releases

**Codex 0.153.4 ([rust-v0.153.4](https://github.com/openai/codex/releases/tag/rust-v0.153.4))**

- Fixed Astra's visibility in the bundled model picker, making it the bundled default when no model is explicitly configured ([#42874](https://github.com/openai/codex/pull/42874)).
- Updated Astra's guidance to use asynchronous clarification questions only when the tool is actually available in the session ([#42878](https://github.com/openai/codex/pull/42878)).

**Codex 0.153.3 ([rust-v0.153.3](https://github.com/openai/codex/releases/tag/rust-v0.153.3))**

- Added GPT-6-Astra to the Amazon Bedrock model picker for Mantle and Runtime global/US routes ([#42805](https://github.com/openai/codex/pull/42805)).
- Corrected GPT-6-Astra's guidance for asynchronous clarification questions to reference the supported tool and to recognize that it accepts text only ([#42809](https://github.com/openai/codex/pull/42809)).

## 3. Hot Issues

**Windows desktop reliability**

- **[Windows Codex app missing "Remote control other devices" tab in Settings → Connections](https://github.com/openai/codex/issues/28919)** — *59 comments · 54 👍*: The longest-running and most-voted issue this week. Windows users cannot access the remote/device-control surface that exists elsewhere in the app; still open after nearly three months.
- **[code-mode host exited during handshake; GPT-5.6 not working properly](https://github.com/openai/codex/issues/41049)** — *46 comments*: A Windows 10 report (with more reproductions in-thread) where the local command-execution channel crashes during handshake, breaking automatic directory reading and tool calls in code mode.
- **[Bundled plugins unavailable on Windows — copyfile fails on EFS-encrypted WindowsApps](https://github.com/openai/codex/issues/25220)** — *29 comments*: Computer Use, Browser, Chrome, and LaTeX plugins all fail to activate on Windows machines where Microsoft Store installs are EFS-encrypted. Since May, this remains a hard blocker for plugin marketplace users.
- **[Windows + WSL cannot create projects — AbsolutePathBuf deserialized without a base path](https://github.com/openai/codex/issues/41463)** — *27 comments · 18 👍*: Project creation fails under WSL2 because path deserialization drops the mount base; a high-signal issue for WSL-based developers.

**Model behavior and permission confusion**

- **[GPT-5.6 serializes independent Code Mode calls; explicit batching reduced weighted usage by 27–45%](https://github.com/openai/codex/issues/35050)** — *30 comments · 41 👍*: A community-driven cost/performance analysis showing that the model's serialization of independent calls materially increases token usage. Strong demand for app-level batching or model-level parallelism.
- **[Codex Desktop resumes thread with workspace-write sandbox despite UI showing Full Access](https://github.com/openai/codex/issues/25590)** — *10 comments*: A trust-critical mismatch: the UI reports Full Access, but after resuming a thread the sandbox silently enforces `workspace-write` / `on-request` permission.
- **[create_thread does not inherit auto-approval mode for worktree tasks](https://github.com/openai/codex/issues/33282)** — *15 comments*: Child worktree tasks spawned by Codex Desktop don't inherit the parent's auto-approval mode, breaking automation flows that depend on consistent permissions.

**Windows "pets" overlay regression cluster**

- **[Windows floating pets become click-through and cannot be dragged](https://github.com/openai/codex/issues/41513)** — *23 comments · 10 👍*: Built-in and custom pets render but no longer accept mouse input. Related reports [#41960](https://github.com/openai/codex/issues/41960) (15 comments), [#41596](https://github.com/openai/codex/issues/41596), and [#42661](https://github.com/openai/codex/issues/42661) show a systemic Windows overlay hit-testing regression rather than an isolated bug.

**CLI / rate-limit friction**

- **[Remove manual approval of "Keep Waiting"](https://github.com/openai/codex/issues/32139)** — *11 comments · 22 👍*: Users want the CLI to auto-accept extended wait periods instead of requiring an extra approval keystroke; the issue has the strongest 👍-to-comment ratio this week.
- **[Codex CLI login blocked by phone verification rate limit](https://github.com/openai/codex/issues/25820)** — *12 comments*: A Pro subscriber cannot authenticate the CLI at all because phone verification is rate-limited — a frustrating onboarding dead-end for paid users on shared networks.

## 4. Key PR Progress

**Async question support in the TUI**

- **[Preserve TUI question state and integrate history and queue navigation](https://github.com/openai/codex/pull/42903)** — Preserves question drafts, selections, expanded state, and handled-message IDs across reconnect/restore; avoids replaying buffered live questions during session refresh.
- **[Add inline "Other" answers to async question choices](https://github.com/openai/codex/pull/42897)** — Adds an editable "Other" option to suggested-choice panes so users can type a custom answer without leaving the question UI.
- **[Support selectable answers for asynchronous TUI questions](https://github.com/openai/codex/pull/42894)** — Renders numbered, wrapped choice lists and requires full visibility before submission.
- **[Integrate asynchronous questions into the TUI](https://github.com/openai/codex/pull/42891)** — The core integration: collapsed question count, expandable answer editor, and navigate/queue/skip controls while preserving the composer draft.

**Astra model rollout**

- **[List GPT-6-Astra in the model picker](https://github.com/openai/codex/pull/42879)** — Sets Astra's bundled visibility to `list` so it appears first in the interactive model picker; complements the hotfix releases above.

**Session integrity and safety**

- **[Establish root turn identity for independent tasks and memory requests](https://github.com/openai/codex/pull/42900)** — Fixes missing `root_turn_id` on background/empty-input turns and detached memory requests so independent tasks can no longer adopt a root from coalesced mailbox input.
- **[Harden Guardian reviews after context compaction](https://github.com/openai/codex/pull/42852)** — Prevents Guardian from losing user authorization constraints or reusing an unreadable parent checkpoint after compaction; retains bounded excerpts of oversized root user messages.

**Platform and performance**

- **[Add a native Windows MXC sandbox adapter](https://github.com/openai/codex/pull/42841)** — New `codex-mxc-sandbox` crate with native MXC detection, standard I/O inheritance, and deny-path support verification — a substantive step toward Windows sandbox parity.
- **[Avoid redundant filesystem sandbox path resolution](https://github.com/openai/codex/pull/42870)** — Removes synchronous probing of unrelated permission roots and repeated alias resolution on executor threads, reducing sandbox setup overhead.

**DX polish**

- **[Preserve Markdown formatting when copying TUI responses](https://github.com/openai/codex/pull/42847)** — Writes rendered HTML alongside original Markdown to the native clipboard so rich-text destinations keep headings, tables, code, and formatting.

## 5. Feature Request Trends

- **Fewer modal interruptions, more autonomous continuation**: Users consistently request that Codex stops requiring manual acknowledgment — auto-accept "Keep Waiting" time ([\#32139](https://github.com/openai/codex/issues/32139)) and automatically resume automation after a rate-limit window expires ([\#12503](https://github.com/openai/codex/issues/12503)).
- **Optional "lifestyle" UI that stays out of the way**: The simultaneous spread of pets-related bugs has amplified requests to hide the Pets menu item entirely and to make prompt-polishing behavior configurable ([\#32069](https://github.com/openai/codex/issues/32069)).
- **Model-driven session metadata**: Community interest in exposing conversation renaming as a model-usable capability so titles track evolving work dynamically ([\#14044](https://github.com/openai/codex/issues/14044)).
- **Native Git workflow integration**: Repeated asks for AI-generated commit messages that carry conversation/chat context, rather than relying on third-party extensions ([\#20036](https://github.com/openai/codex/issues/20036)).

## 6. Developer Pain Points

- **Windows is the weak link**: Users consistently hit broader and more severe issues on Windows: GPT-5.6 handshake crashes ([\#41049](https://github.com/openai/codex/issues/41049)), 15-minute first-launch hangs extracting the bundled `cua_node` runtime ([\#41170](https://github.com/openai/codex/issues/41170)), WSL project creation failures ([\#41463](https://github.com/openai/codex/issues/41463)), EFS-blocked bundled plugins ([\#25220](https://github.com/openai/codex/issues/25220)), and the pets overlay regression cluster.
- **Sandbox permission state is unpredictable**: The app's displayed permission mode disagrees with the actual sandbox after resume ([\#25590](https://github.com/openai/codex/issues/25590)), worktree children don't inherit auto-approval ([\#33282](https://github.com/openai/codex/issues/33282)), and Full Access children are intermittently downgraded to managed approval ([\#40125](https://github.com/openai/codex/issues/40125)). Developers can't trust what the UI tells them about a session's real capabilities.
- **Auth and quota dead-ends**: Phone-verification rate limits can fully block CLI login for paid subscribers ([\#25820](https://github.com/openai/codex/issues/25820)), and the CLI reportedly cannot consume Luna Reserve entitlements once standard usage is exhausted even when the same account has them in the app ([\#40939](https://github.com/openai/codex/issues/40939)).
- **Cost-inefficient model behavior**: GPT-5.6 serializing independent Code Mode calls costs users 27–45% extra weighted usage unless they hand-batch explicitly ([\#35050](https://github.com/openai/codex/issues/35050)). Separately, users report the model over-reporting completion and compressing `AGENTS.md` instructions despite explicit project rules ([\#31177](https://github.com/openai/codex/issues/31177)) — a reliability red flag for long-running autonomous work.

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-09-05

## 1. Today's Highlights

The v0.60.0 nightly release lands two security-oriented fixes: extension-triggered environment changes now require explicit user consent (with runtime-altering variables sanitized), and workspace path boundary checks were hardened against symlink escapes. In parallel, a wave of open PRs is converging on sandbox hardening — isolating settings directories, enforcing config-file provenance, and blocking NTFS short-name path bypasses — signaling that filesystem isolation is the project's current focal point.

## 2. Releases

**v0.60.0-nightly.20260905.g85aca163f** ([release](https://github.com/google-gemini/gemini-cli/releases/tag/v0.60.0-nightly.20260905.g85aca163f))
- **fix(extensions):** Prompt for consent on environment changes and sanitize runtime-altering environment variables ([PR #28863](https://github.com/google-gemini/gemini-cli/pull/28863)) — extension updates can no longer silently inject env vars into spawned MCP server processes.
- **fix(core):** Enhance workspace path boundary checks and symlink resolution in command safety and file discovery ([PR #29170](https://github.com/google-gemini/gemini-cli/pull/29170)) — closes symlink-based workspace escape vectors on POSIX and Windows.

## 3. Hot Issues

1. **[#22323](https://github.com/google-gemini/gemini-cli/issues/22323) — Subagent recovery after MAX_TURNS reported as GOAL success (p1, 13 comments).** A `codebase_investigator` subagent that hits its turn limit reports `status: "success"` and `Termination Reason: "GOAL"`, masking the interruption. Misleading termination reporting undermines trust in agent results — this is exactly the kind of bug that silently corrupts downstream workflows.

2. **[#21409](https://github.com/google-gemini/gemini-cli/issues/21409) — Generalist agent hangs (p1, 8 comments, 8 👍).** When Gemini CLI defers to the generalist agent, it hangs indefinitely — even simple folder creation stalls for up to an hour. High community upvote count suggests wide impact; a workaround exists (disable subagent delegation), but that defeats the agent architecture.

3. **[#25166](https://github.com/google-gemini/gemini-cli/issues/25166) — Shell execution stuck at "Waiting input" after command completes (p1, 4 comments, 3 👍).** Simple CLI commands finish but remain shown as active, with the session hanging. Recurring enough to be p1; terminal-state management remains fragile.

4. **[#19873](https://github.com/google-gemini/gemini-cli/issues/19873) — Leverage model's bash affinity via zero-dependency OS sandboxing (p2, 9 comments).** Proposes letting Gemini 3 models use native POSIX toolchains (`grep`, `sed`, `awk`) through OS-level sandboxing plus post-execution intent routing, rather than restricting them to bespoke tools. Bridges the gap between model strengths and safe execution.

5. **[#22745](https://github.com/google-gemini/gemini-cli/issues/22745) — AST-aware file reads, search, and mapping (p2, 7 comments).** Epic investigating whether AST-aware tools can reduce token noise, read exact method bounds in one call, and improve codebase navigation. A direct response to context-bloat pain.

6. **[#21968](https://github.com/google-gemini/gemini-cli/issues/21968) — Gemini does not use skills and sub-agents enough (p2, 6 comments).** Users report the model ignores custom skills (e.g., "gradle", "git") unless explicitly instructed. The discovery/selection heuristics for installed skills appear far too conservative.

7. **[#26525](https://github.com/google-gemini/gemini-cli/issues/26525) — Auto Memory: deterministic redaction and reduced logging (p2, 5 comments).** Memory's background extractor sends transcript content to the model *before* redaction instructions run, and can log existing skill content. A privacy/security concern: secrets may enter model context before the "redact this" prompt takes effect.

8. **[#21983](https://github.com/google-gemini/gemini-cli/issues/21983) — Browser subagent fails on Wayland (p1, 4 comments).** Browser agent terminates with "GOAL" but no useful output under Wayland sessions. Another p1 agent bug that ships a misleading success signal.

9. **[#24246](https://github.com/google-gemini/gemini-cli/issues/24246) — 400 error with >128 tools (p2, 3 comments).** The CLI hits API 400 errors when tool counts grow large; users expect smarter scoping of enabled tools to fit context windows.

10. **[#22672](https://github.com/google-gemini/gemini-cli/issues/22672) — Agent should stop/discourage destructive behavior (p2, 3 comments).** Complex git operations and DB maintenance occasionally trigger `git reset`/`--force` when safer alternatives exist. Points to missing hazard-awareness in the system prompt for high-risk commands.

## 4. Key PR Progress

1. **[#29215](https://github.com/google-gemini/gemini-cli/pull/29215) — Enforce envelope metadata provenance for untrusted tool outputs.** Updates the core system prompt so author identity and operational status derive only from verified top-level envelope properties, defending against prompt injection via MCP/external tool payloads.

2. **[#29216](https://github.com/google-gemini/gemini-cli/pull/29216) — Isolate settings directory in sandbox containers.** Previously mounted host `~/.gemini` directly into Docker/Podman containers, exposing OAuth tokens and credentials. Replaces host mounts with sanitized, read-only config files.

3. **[#29214](https://github.com/google-gemini/gemini-cli/pull/29214) — Harden filesystem boundaries and isolate runtime state.** Resolves symlinks during path sensitivity checks, decouples container environment from host, and sanitizes config mounting — complements #29216 in the same sandbox-hardening push.

4. **[#29116](https://github.com/google-gemini/gemini-cli/pull/29116) — Mitigate NTFS 8.3 short-name (SFN) path bypasses.** Handles Windows short names (`git~1`, `env~1`, `node_m~1`) in path normalization and `AllowedPathChecker`, closing a traversal/blocklist bypass unique to NTFS.

5. **[#29110](https://github.com/google-gemini/gemini-cli/pull/29110) — Route `read_file` content through FileSystemService.** `read_file` bypassed the injected `FileSystemService` (unlike `write_file`/`replace`), breaking ACP clients that advertise remote `fs` capabilities. Alignment fix for the filesystem abstraction layer.

6. **[#29114](https://github.com/google-gemini/gemini-cli/pull/29114) — Prevent duplicate `handleExit` execution on spawn failure.** Node's `child_process` fires both `error` and `close` on spawn failure; a re-entrancy guard stops double-handling that could corrupt shell session state.

7. **[#29217](https://github.com/google-gemini/gemini-cli/pull/29217) — Don't rewrite explicit `gemini-2.5-flash` model selection.** `isFlashModel()` used a broad `endsWith('flash')` check, silently upgrading an explicitly pinned `--model gemini-2.5-flash` to 3.5 Flash once GA. Honors explicit user pinning.

8. **[#29118](https://github.com/google-gemini/gemini-cli/pull/29118) — Only strip trailing `.git` suffix in extension repo parsing.** Repos like `blog.github.io` were previously mangled because `.git` removal wasn't anchored to the end of the name. Small parsing correctness fix for extension installs.

9. **[#28942](https://github.com/google-gemini/gemini-cli/pull/28942) — Strict boolean parsing for `DEBUG` env var in sandbox launcher.** Used JavaScript string truthiness, so `DEBUG=false` and `DEBUG=0` enabled debug mode. Fixes three observable bugs tied to inverted debug behavior.

10. **[#28863](https://github.com/google-gemini/gemini-cli/pull/28863) — Consent for extension env changes + sanitization (merged into today's nightly).** Incorporates MCP server environment configs into consent strings and sanitizes custom env vars that could alter runtime behavior. First half of the release's security one-two punch.

Also notable: the **[PR-generation evaluation suite PRs #28948–#28953](https://github.com/google-gemini/gemini-cli/pull/28948)** (eval harness, LLM diff judge, diff visualizer, deployment pipeline) were closed after a nudge — appears the caretaker bot is consolidating or superseding this internal benchmarking work.

## 5. Feature Request Trends

- **AST-aware code tooling** ([#22745](https://github.com/google-gemini/gemini-cli/issues/22745), [#22746](https://github.com/google-gemini/gemini-cli/issues/22746)): demand for surgical, token-frugal file reads and codebase mapping via AST tools like `tilth`/`glyph`.
- **OS-level sandboxing & native tool use** ([#19873](https://github.com/google-gemini/gemini-cli/issues/19873)): let the model exercise its bash training inside a zero-dependency sandbox instead of constraining it to custom tools.
- **Persistent, file-based task tracking** ([#18836](https://github.com/google-gemini/gemini-cli/issues/18836), [#21000](https://github.com/google-gemini/gemini-cli/issues/21000)): replacing in-context `WriteToDo` with CRUD-backed files to fight context rot.
- **Memory system transparency & hygiene** ([#26525](https://github.com/google-gemini/gemini-cli/issues/26525), [#26523](https://github.com/google-gemini/gemini-cli/issues/26523), [#26522](https://github.com/google-gemini/gemini-cli/issues/26522)): deterministic redaction before model context, quarantine of invalid patches, and no infinite retries on low-signal sessions.
- **Deeper subagent observability & autonomy** ([#22598](https://github.com/google-gemini/gemini-cli/issues/22598), [#21968](https://github.com/google-gemini/gemini-cli/issues/21968)): shareable subagent trajectories via `/chat share` and proactive use of user-defined skills/subagents.
- **Browser agent resilience** ([#22232](https://github.com/google-gemini/gemini-cli/issues/22232)): automatic session takeover and lock recovery instead of fail-fast on locked profiles.

## 6. Developer Pain Points

- **False success signals:** Subagents reporting `GOAL`/`success` when actually interrupted ([#22323](https://github.com/google-gemini/gemini-cli/issues/22323)), or browser agents exiting "successfully" with no work done ([#21983](https://github.com/google-gemini/gemini-cli/issues/21983)) — erodes confidence in agent output.
- **Hangs and stalls:** Generalist agent hangs ([#21409](https://github.com/google-gemini/gemini-cli/issues/21409)) and shell commands stuck at "Waiting input" ([#25166](https://github.com/google-gemini/gemini-cli/issues/25166)) waste developer time and force manual intervention.
- **Context/token bloat:** Large file reads "firehose" the context window, and the CLI 400-errors when tool count exceeds backend limits ([#24246](https://github.com/google-gemini/gemini-cli/issues/24246)) — both point to weak context budgeting.
- **Secret exposure risk:** Auto Memory sends content to the model before redaction ([#26525](https://github.com/google-gemini/gemini-cli/issues/26525)) and sandbox containers mount raw host credentials ([#29216](https://github.com/google-gemini/gemini-cli/pull/29216)); security-conscious users are closely watching both.
- **Workspace pollution:** Models scatter temp edit scripts across directories ([#23571](https://github.com/google-gemini/gemini-cli/issues/23571)) and occasionally issue destructive git commands ([#22672](https://github.com/google-gemini/gemini-cli/issues/22672)), forcing cleanup before commits.

---
*Data compiled from the [google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli) repository on 2026-09-05.*

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI Community Digest — 2026-09-05

## Today's Highlights
Three releases shipped in the past 24 hours, headlined by GPT-6 Astra support and Windows 11 taskbar session cards. Community attention is split between per-agent configuration demands (reasoning effort, model fallbacks) and a wave of regressions: MCP tool-discovery breakage, BYOK prompt caching being silently disabled (~5x cost), and an auto-update path that corrupts the desktop app's bundled CLI binary.

## Releases
- **v1.0.84-1** — Adds support for GPT-6 Astra.
- **v1.0.84-0** — Managed sandbox sessions can now be disabled for the rest of the session from an approved bypass prompt. Fixes: PowerShell file writes blocked by the sandbox now offer to run outside the sandbox; fixes sandboxed `gh` behavior when multiple GitHub accounts exist in the credential store.
- **v1.0.83** (2026-09-04) — Running Copilot sessions now surface in the Windows 11 taskbar with live hover status cards. Adds Client ID Metadata Document (CIMD) support for MCP OAuth sign-in. Custom agents can list several models in `model`, tried in order until one is available, with `model-policy: required` to enforce the constraint.

See the [releases page](https://github.com/github/copilot-cli/releases) for details.

## Hot Issues
1. **Per-custom-agent reasoning effort** ([#2904](https://github.com/github/copilot-cli/issues/2904)) — `.agent.md` files can pin `model` but cannot set reasoning effort; only the global `--effort` flag exists. 8 comments, 23 👍 — the strongest community signal in this digest for finer-grained agent configuration.
2. **~20.5K-token fixed system prompt** ([#2627](https://github.com/github/copilot-cli/issues/2627)) — Prompt plus tool guidelines consume ~29K tokens before any user content. Users request slimming or configurability. 19 👍.
3. **Add a `--system-prompt` parameter** ([#232](https://github.com/github/copilot-cli/issues/232)) — Long-running request (since Oct 2025) for system-level instructions without repo-local files. 10 👍.
4. **BYOK silently disables prompt caching** ([#4720](https://github.com/github/copilot-cli/issues/4720)) — v1.0.82 BYOK requests carry no cache declaration; `cached_tokens=0` every turn, re-creating cache each turn → roughly 5x cost. High-impact cost regression for BYOK users.
5. **Auto-update rewrites the `copilot.exe` it was launched from** ([#4728](https://github.com/github/copilot-cli/issues/4728)) — Running standalone `copilot` can corrupt the GitHub Copilot desktop app's bundled CLI, breaking session resume with "Session unavailable." A serious packaging/trust issue.
6. **ACP mode auto-approves tool calls again** ([#4537](https://github.com/github/copilot-cli/issues/4537)) — Since 1.0.81-1, `--acp` sends no `session/request_permission`; shell/file operations execute unattended. Regression of #845 — a permission-safety concern.
7. **Ctrl+H misparsed under WSL2** ([#4328](https://github.com/github/copilot-cli/issues/4328)) — `WT_SESSION` leaking from Windows Terminal makes `ctrl+h` delete whole words instead of one character. 7 comments, clear repro.
8. **v1.0.81 broke chroma-mcp compatibility** ([#4647](https://github.com/github/copilot-cli/issues/4647)) — MCP stdio config working on 1.0.80 fails on 1.0.81; still in triage with no maintainer response yet.
9. **Runaway `copilot-file-search` thread** ([#4710](https://github.com/github/copilot-cli/issues/4710)) — An idle `--yolo` session spawns a file-search thread pinning a CPU core and writing unbounded diagnostic logs under `~/.copilot`.
10. **Configurable auto-compaction threshold** ([#1688](https://github.com/github/copilot-cli/issues/1688)) — On large-context models like Opus 4.6, latency degrades sharply at 45–60% context usage, well before built-in compaction triggers.

Also closed this cycle: **MCP legacy `initialize` after modern `server/discover`** ([#4525](https://github.com/github/copilot-cli/issues/4525)) — a -32022 regression against the Python MCP SDK 2.0.0 dual-era runner, now resolved.

## Key PR Progress
Only one PR appears in the 24-hour update window, and the overall snapshot shows little substantive open PR motion.

- [#3771](https://github.com/github/copilot-cli/pull/3771) — "Initial project setup" by limenpchuolto112-creator (open since June 11, last updated Sept 4). No description; appears to be repository scaffolding rather than a feature change.

Note: PR/merge activity is under-represented in this data window; no in-flight feature or fix PRs with meaningful descriptions were captured.

## Feature Request Trends
- **Per-agent intelligence controls** — Custom-agent reasoning effort (#2904), multi-model fallback ordering (shipped in v1.0.83), and `model-policy` show demand for agents as independently tunable units rather than inheriting global defaults.
- **System prompt configurability** — Both a user-supplied global system prompt (#232) and trimming the built-in prompt's token footprint (#2627) target the same issue: fixed instructions are a large, non-negotiable context cost at session start.
- **Context/cache-aware session lifecycle** — Auto-compaction thresholds (#1688) and compact-on-idle aligned to prompt-cache TTL (#4724) reflect demand for context management driven by cost and latency, not just raw token counts.
- **Terminal input UX polish** — Standard GUI text-selection shortcuts (#2644), mouse-scroll behavior in embedded terminals (#3194), and an option to suppress the scrollbar (#4707) are recurring TUI ergonomics requests.
- **Enterprise governance** — Blocking built-in plugin marketplaces (#4715) and linking to the "Trusted Access for Cyber" program (#4322) show enterprise adoption requirements surfacing.

## Developer Pain Points
- **MCP ecosystem churn** — Version-to-version MCP breakage (#4525, #4647) plus a stuck-server `tools/list` refresh that permanently strips tools (#4731). Each release risks breaking MCP servers, and failure paths give users little diagnostic signal.
- **Cost regressions discovered after upgrade** — Fixed 20.5K-token overhead (#2627) and BYOK caching disabled in 1.0.82 (#4720) both translate directly into spend; users are repeatedly catching cost regressions post-release.
- **Permission-model regressions** — ACP auto-approval slipping again (#4537) continues a pattern (#845) that erodes trust in non-interactive/automation modes.
- **Terminal-dependent input handling** — WSL2 key misparse (#4328), Android Studio mouse-wheel history cycling (#3194) — terminal detection/translation remains fragile across environments.
- **Session and extension lifecycle fragility** — Extension reconnects dispose hook processors (#4590); `session.resume` silently ignores a requested `model` (#4645); OTel spans lose input messages after window reload (#4726).
- **Update and packaging hazards** — The auto-update corrupting the desktop app's bundled binary (#4728) is this batch's most severe trust breaker: a standalone process rewriting a hosting application's files.

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI Community Digest — 2026-09-05

## 1. Today's Highlights
No releases landed in the last 24h, and the community window was very quiet: one bug report surfaced around Ctrl+V paste failing on Windows Terminal + PowerShell (v0.40.1), while PR #2524 — a fix for `StrReplaceFile` replacement counting — got updated after roughly six weeks, suggesting it may be approaching merge. Overall, the actionable signals are narrowly focused on Windows terminal compatibility and correct file-edit accounting.

## 2. Releases
No new releases in the last 24h; the latest known version remains **0.40.1**. *(Omitted as no release activity exists in the digest window.)*

## 3. Hot Issues
*Only 1 Issue met the 24h updated-criteria window; listing it below.*

### #2634 — [bug] kimi终端改键位不成功，比如粘贴
**Author:** PANG-GIT-AI | **Created:** 2026-09-04 | **Comments:** 0 | **👍:** 0
**Link:** [MoonshotAI/kimi-cli Issue #2634](https://github.com/MoonshotAI/kimi-cli/issues/2634)

- **Reported problem:** Running Kimi Code CLI 0.40.1 under **Windows Terminal + PowerShell**, the standard **Ctrl+V paste shortcut does not work**. The reporter also mentions their terminal-level key binding changes don't take effect.
- **Why it matters:** Windows Terminal + PowerShell is a mainstream Windows dev setup. Inability to paste via Ctrl+V breaks the standard keyboard workflow and forces users to rely on context menus or other workarounds mid-AGI session.
- **Community reaction:** None yet — the issue is fresh (0 comments, 0 reactions). Expect follow-up reports from other Windows users if the problem proves widespread.

## 4. Key PR Progress
*One PR was updated in the digest window.*

### #2524 — fix(tools): count StrReplaceFile replacements against the running content
**Author:** Sreekant13 | **Created:** 2026-07-20 | **Updated:** 2026-09-04 | **Resolves #2526**
**Link:** [MoonshotAI/kimi-cli PR #2524](https://github.com/MoonshotAI/kimi-cli/pull/2524)

- **What it does:** `StrReplaceFile` applies edits sequentially, but its reported replacement count was calculated against the *original* file content. When a chained edit's `old` string is produced by an earlier edit, that string never existed in the original file — so the replacement went uncounted. This PR makes the count track the running/current content after each edit.
- **Why it matters:** Agentic coding loops that rely on iterative `str_replace` operations can abort or misreport when "no match found" is returned based on stale original-content counts. The fact that this fix has been open since July and was just touched again is a positive signal toward final review.

## 5. Feature Request Trends
**Note:** With only 1 Issue in the 24h window, there is no statistically robust basis for trend extraction. The available signal points to:

- **Customizable terminal key bindings inside Kimi Code CLI** — specifically, users want an in-CLI mechanism to make standard shortcuts like `Ctrl+V` paste work consistently, rather than requiring OS/terminal-level remapping.

## 6. Developer Pain Points

- **Windows Terminal + PowerShell clipboard/keybinding gap:** `Ctrl+V` paste reportedly fails inside the CLI session, and terminal settings for key bindings don't seem to take effect — a recurring rough edge for Windows devs since Ctrl+C/V handling differs from Unix terminals.
- **File-edit reporting accuracy:** The underlying issue behind PR #2524 (#2526) highlights that `StrReplaceFile`'s replacement counts can be misleading for multi-step edits, forcing tooling authors to distrust reported counts and add extra verification.

*Digest compiled from public MoonshotAI/kimi-cli GitHub data for 2026-09-05.*

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-09-05

## Today's Highlights

Two hotfix releases shipped in the last 24 hours: v1.18.28 added GitHub Copilot session-ID tracking and desktop auth fixes, while v1.18.29 resolved Codex OAuth model filtering so GPT-class integer versions (e.g., `gpt-6`, `gpt-6-astra`) appear correctly for OpenAI subscribers. Meanwhile, the community spotlight remains on Claude Code hooks compatibility ([#12472](https://github.com/anomalyco/opencode/issues/12472), 40 👍) — still the highest-demand compatibility gap — and two regressions in recent point releases (plugin installer timeouts in 1.18.21, remote MCP breakage in 1.18.28) are drawing active triage.

## Releases

- **[v1.18.29](https://github.com/anomalyco/opencode/releases/tag/v1.18.29)** — Core bugfixes: Codex OAuth model filtering now recognizes integer GPT versions like `gpt-6`; fixes `gpt-6-astra` not appearing for OpenAI subscription users. Includes docs(zh) bold-rendering fix from community contributor @Peter267.
- **[v1.18.28](https://github.com/anomalyco/opencode/releases/tag/v1.18.28)** — Core: sends session ID as GitHub Copilot's interaction header to improve request tracking across a session. Desktop: uses the desktop client ID during OpenCode account device authentication; larger open-in-app icon for visibility.

## Hot Issues

1. **[#12472 — Native Claude Code hooks compatibility (OPEN)](https://github.com/anomalyco/opencode/issues/12472)** — Requests `PreToolUse`, `PostToolUse`, and `Stop` hooks from `~/.claude/settings.js`. The most-upvoted open feature (40 👍, 19 comments); rules/skills already work, hooks are the remaining gap for migrating Claude Code setups.
2. **[#19948 — Ollama local model integration (CLOSED)](https://github.com/anomalyco/opencode/issues/19948)** — Local Ollama models appear in OpenCode Desktop but return invalid JSON responses for tool/“skill” arguments. 23 comments; highlights ongoing rough edges for local-model users on Windows.
3. **[#25832 — Opencode cannot read images anymore (CLOSED)](https://github.com/anomalyco/opencode/issues/25832)** — Regression where image input fails with a bad-request error ("does not support image input"). 18 comments; a high-severity regression for multimodal workflows.
4. **[#30680 — Auto-compaction loop and token drain (CLOSED)](https://github.com/anomalyco/opencode/issues/30680)** — Repeatedly auto-compacts and consumes tokens even in empty folders, eventually stopping responses entirely. 17 comments; severe for paid-API users.
5. **[#44684 — Plugin installer times out fetching npm deps (OPEN)](https://github.com/anomalyco/opencode/issues/44684)** — 1.18.21 regression: plugins from private registries (Verdaccio via SSH tunnel) hang or die with `NpmInstallFailedError`, and headless runs hang. 5 comments; ties directly to PR #47430.
6. **[#47368 — Remote MCP regression in 1.18.28 (CLOSED)](https://github.com/anomalyco/opencode/issues/47368)** — KitWright/Unity remote MCP server on `127.0.0.1:9155` stopped connecting after the 1.18.27→1.18.28 update. A freshly filed, fast-closed regression — warns that desktop/MCP refactors need closer release scrutiny.
7. **[#35148 — Bad gateway error loop (CLOSED)](https://github.com/anomalyco/opencode/issues/35148)** — Persistent bad-gateway errors and looping, no specific repro steps. 13 👍; broad impact suggests gateway/proxy reliability concerns for Go plugins.
8. **[#17188 — Default sharing to "disabled": privacy by default (CLOSED)](https://github.com/anomalyco/opencode/issues/17188)** — Asks that sharing/telemetry default to off, citing informed-consent concerns (relates to #7982, #459). 13 👍; a recurring privacy theme even though closed.
9. **[#16678 — "Failed to run the query 'CREATE TABLE project'" prevents startup (CLOSED)](https://github.com/anomalyco/opencode/issues/16678)** — Mid-migration quit leaves the app unable to start; user suggests transactional migration or blocking `/exit`. 5 comments; worth watching for storage-layer robustness.
10. **[#29175 — Plugin-created child sessions invisible in parent UI (CLOSED)](https://github.com/anomalyco/opencode/issues/29175)** — Sessions created via `session.create(parentID)` stream events but never appear because the TUI only discovers native `task` tool subagents. 4 comments; important for plugin authors building agent orchestration.

## Key PR Progress

1. **[#35311 — Multiple clones of same repo are different projects (OPEN)](https://github.com/anomalyco/opencode/pull/35311)** — Rewrites project identity so clones of the same repository share session/project state. Closes **14+ issues** (#17940, #29869, #42040, etc.); one of the largest long-running fixes in flight.
2. **[#47430 — Bound npm installs with configurable timeout (OPEN)](https://github.com/anomalyco/opencode/pull/47430)** — Addresses `Npm.reify()` hanging forever (no timeout), targeting #31463 and #44684; backport of the v2 fix to the v1 line.
3. **[#47436 — Resolve Bedrock credentials via AWS default chain (OPEN)](https://github.com/anomalyco/opencode/pull/47436)** — Native Bedrock routes currently accept only static keys or bearer tokens; this adds `AWS_PROFILE`, `~/.aws` shared config, SSO cache, web identity, and instance/container metadata support.
4. **[#47339 — Stop retrying free and Go usage quotas (OPEN)](https://github.com/anomalyco/opencode/pull/47339)** — Fixes session retry policy treating `FreeUsageLimitError`'s daily `retry-after` as a normal transient failure, preventing wasted retries (closes #47318).
5. **[#47428 — Defer background workspace discovery (OPEN)](https://github.com/anomalyco/opencode/pull/47428)** — Stops eager worktree/MCP catalog discovery for historical projects; defers to mounted sessions or workspace pickers with server-scoped caching for faster startup.
6. **[#46690 — Expose session forms, session list, and global event stream via plugin API (OPEN)](https://github.com/anomalyco/opencode/pull/46690)** — New plugin capabilities requested for a Telegram bot plugin; broadens OpenCode as a platform.
7. **[#47342 — OpenAI usage normalization and tier threshold config (CLOSED)](https://github.com/anomalyco/opencode/pull/47342)** — `input_tokens` from OpenAI already includes cached/cache-write tokens; `normalizeUsage` now subtracts both (clamped) so each category is priced once, plus configurable `cost200K` tier threshold.
8. **[#47423 — Support provider OAuth client credentials (OPEN)](https://github.com/anomalyco/opencode/pull/47423)** — Opt-in `client_credentials` flow for configured providers with in-memory token caching, renewal on expiry, and 401/`invalid_token` retry — no browser, `/connect`, or refresh token needed (refs #24084).
9. **[#47431 — Rename `/variants` to `/reasoning`, keep alias (OPEN)](https://github.com/anomalyco/opencode/pull/47431)** — Makes `/reasoning` the canonical slash command for the model-variant picker while retaining `/variants` as an alias (closes #47432).
10. **[#47388 — Reload local plugin dependency graphs (CLOSED)](https://github.com/anomalyco/opencode/pull/47388)** — Editing a local CLI plugin's imported helper previously left stale cached UI; this reloads the full dependency graph so new helper exports work in dev.

## Feature Request Trends

- **Claude Code hooks compatibility** ([#12472](https://github.com/anomalyco/opencode/issues/12472), 40 👍) remains the single most-requested migration block for ex-Claude Code users: `PreToolUse`, `PostToolUse`, and `Stop` lifecycle hooks.
- **Privacy by default**: [#17188](https://github.com/anomalyco/opencode/issues/17188) (13 👍) asks that sharing default to disabled; related threads (#7982, #459) show accumulated demand for consent-first defaults.
- **Richer plugin hook surface**: repeated requests for lifecycle hooks — finalization hooks for main/subagent sessions ([#35540](https://github.com/anomalyco/opencode/issues/35540)), footer text event interception ([#35561](https://github.com/anomalyco/opencode/issues/35561)), and session/global event exposure (PR #46690). The plugin ecosystem is clearly outgrowing the current API.
- **Safety controls for agentic fetch/search**: URL whitelisting for webfetching, websearching, and codesearching ([#35565](https://github.com/anomalyco/opencode/issues/35565)); OpenRouter model list filtering by user privacy/guardrail preferences ([#35506](https://github.com/anomalyco/opencode/issues/35506)).
- **More built-in LSPs and harness conveniences**: CircleCI YAML language server ([#25735](https://github.com/anomalyco/opencode/issues/25735)); auto-importing `@included` files by the harness ([#35567](https://github.com/anomalyco/opencode/issues/35567)).

## Developer Pain Points

- **Release regressions are the top frustration**: image input broken between 1.18.x releases ([#25832](https://github.com/anomalyco/opencode/issues/25832)), plugin installer hangs introduced in 1.18.21 ([#44684](https://github.com/anomalyco/opencode/issues/44684)), and remote MCP connectivity broken in 1.18.28 ([#47368](https://github.com/anomalyco/opencode/issues/47368)) — each was a working feature in the prior version.
- **Cost/session runaway**: auto-compaction loops consuming tokens ([#30680](https://github.com/anomalyco/opencode/issues/30680)) and misleading aggregated usage percentages on the dashboard ([#47142](https://github.com/anomalyco/opencode/issues/47142)) erode trust in quota accounting.
- **Custom provider friction**: `"attachment": true` having no effect ([#33542](https://github.com/anomalyco/opencode/issues/33542)), MiniMax-M3 via proxy rejecting images ([#34596](https://github.com/anomalyco/opencode/issues/34596)), and Google AI Studio rejecting `strength_areas`/`skills` fields ([#35606](https://github.com/anomalyco/opencode/issues/35606)) point to inconsistent model capability detection across providers.
- **Terminal/TUI robustness**: corrupted terminal after SSH attempts ([#35541](https://github.com/anomalyco/opencode/issues/35541)), `[Reconciler] Unknown component type: spinner` crash ([#35562](https://github.com/anomalyco/opencode/issues/35562)), and desktop crashes on large pastes (PR #47427) show UI resilience gaps on Windows/macOS.
- **Startup/configuration fragility**: broken SQLite state after interrupted migration bricking the app ([#16678](https://github.com/anomalyco/opencode/issues/16678)) and bad-gateway error loops ([#35148](https://github.com/anomalyco/opencode/issues/35148)) remain recurring "can't launch" report classes.

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi Community Digest — 2026-09-05

## Today's Highlights
The 0.85.0 release shipped persistent Claude thinking-effort handling, but the day was dominated by a packaging regression: multiple reports ([#9132](https://github.com/earendil-works/pi/issues/9132), [#9140](https://github.com/earendil-works/pi/issues/9140), [#9156](https://github.com/earendil-works/pi/issues/9156), [#9158](https://github.com/earendil-works/pi/issues/9158)) confirmed that the published npm tarball statically imports the undeclared `@earendil-works/pi-server`, breaking fresh installs. On the feature front, there's strong momentum around restoring session cursor/selection state (tree navigation, compaction, model refresh) and a new wave of provider integrations (Bedrock Mantle, Meta/Muse, OrcaRouter).

---

## Releases
- **v0.85.0** ([release](https://github.com/earendil-works/pi/releases/tag/v0.85.0)): New **Persistent Claude thinking effort** feature. Anthropic-supported transports now preserve per-turn effort and safely recover from signed-thinking mismatches. See the [Model Configuration docs](https://github.com/earendil-works/pi/blob/v0.85.0/packages/coding-agent/docs/models.md#model-configuration).

---

## Hot Issues
1. **[#5363 — Add amazon-bedrock-mantle provider for OpenAI-compatible models](https://github.com/earendil-works/pi/issues/5363)** (18 comments, 15 👍) — The existing `amazon-bedrock` provider only speaks Converse; Mantle models expose an OpenAI-compatible endpoint and need a separate provider. Heavily upvoted; currently in progress.

2. **[#7730 — High CPU usage on Mac OS with long session](https://github.com/earendil-works/pi/issues/7730)** (15 comments, 10 👍) — CPU swings 50–110% with 600–800MB memory; anecdotal correlation with session/context length. A badly needed performance fix for daily TUI users on macOS.

3. **[#9132 — 0.85.0: published dist/cli.js statically imports @earendil-works/pi-server, which is not a declared dependency](https://github.com/earendil-works/pi/issues/9132)** (4 comments, 5 👍) — The release-blocker packaging bug: a fresh `npm install` cannot import the package root. Several duplicates filed within hours ([#9140](https://github.com/earendil-works/pi/issues/9140), [#9156](https://github.com/earendil-works/pi/issues/9156), [#9158](https://github.com/earendil-works/pi/issues/9158)), indicating broad CI/user breakage.

4. **[#8896 — /export HTML silently drops context that was sent to the model](https://github.com/earendil-works/pi/issues/8896)** (6 comments) — Custom messages with `display: false` are stripped from exports even though they were part of the model context — a fidelity trap for anyone auditing or sharing sessions.

5. **[#8760 — OpenRouter `:free` models fail with 400; Pi sends `max_tokens` above provider limit](https://github.com/earendil-works/pi/issues/8760)** (5 comments) — Pi uses the catalog's `maxOutputTokens` value, exceeding the upstream free-tier hard cap. Impacts multiple `:free` models and blocks budget-conscious users.

6. **[#9052 — Fullscreen mode's wheel scrolling is 3x slower than regular mode](https://github.com/earendil-works/pi/issues/9052)** (5 comments, 2 👍) — Users who switch to fullscreen for the fixed input box get sluggish scroll; a quality-of-life regression in the pinned-input UX.

7. **[#9073 — JsonlSessionRepo rejects cwd-scoped IDs when directory encodings collide](https://github.com/earendil-works/pi/issues/9073)** (2 comments) — Paths like `<root>/tenant-a/project` and `<root>/tenant/a-project` map to the same encoded session directory, causing session ID collisions and creation failures.

8. **[#8720 — Whitespace-only tool output permanently bricks the session (HTTP 400)](https://github.com/earendil-works/pi/issues/8720)** (4 comments) — Bash on Windows emitting `"\r\n"` produces empty tool content that OpenAI-compatible providers reject; the poisoned message stays in history, making every subsequent request fail.

9. **[#8684 — PI_OFFLINE silently disables all provider model discovery](https://github.com/earendil-works/pi/issues/8684)** (4 comments) — Documented as limiting startup housekeeping, but in practice it disables the entire model catalog — undocumented scope that surprises offline-only setups.

10. **[#8857 — Agent loop has no tool call execution timeout](https://github.com/earendil-works/pi/issues/8857)** (3 comments) — Stream timeouts and the optional `bash` timeout don't cover a hung tool call (e.g. `psql` waiting on a DB connection), leaving runs stuck indefinitely. Closed as no-action, but a recurring frustration.

---

## Key PR Progress
1. **[#9170 — fix(coding-agent): declare pi-server runtime dependency](https://github.com/earendil-works/pi/pull/9170)** (OPEN) — Direct fix for the 0.85.0 missing-dependency breakage; adds `@earendil-works/pi-server` to declared deps so importing the package root works on fresh installs.

2. **[#9172 — fix(coding-agent): prevent broken package root publication](https://github.com/earendil-works/pi/pull/9172)** (OPEN, depends on #9170) — Follow-up hardening to stop this class of packaging defect from shipping again.

3. **[#9116 — feat(ai): add mid-conversation system messages](https://github.com/earendil-works/pi/pull/9116)** (OPEN) — First layer of an architecture change enabling extensions or tool-loadout changes to be delivered as delta system messages instead of rewriting the top-level system prompt mid-session.

4. **[#9117 — feat(coding-agent): deliver prompt and tool changes as system message deltas](https://github.com/earendil-works/pi/pull/9117)** (OPEN) — Second layer stacking the coding-agent integration on #9116; eliminates redundant full system-prompt rewrites on every request.

5. **[#9096 — feat(ai,coding-agent): add Meta provider with Muse subscription OAuth](https://github.com/earendil-works/pi/pull/9096)** (OPEN) — New provider resolving #7543. Notes unusual auth (daily re-minting from identity token) and effectively non-streaming output, but scoped and quite self-contained.

6. **[#9135 — feat(ai): add OrcaRouter as a first-class provider with live catalog model discovery](https://github.com/earendil-works/pi/pull/9135)** (CLOSED) — OpenAI-compatible gateway (adaptive routing, failover) added as a built-in provider — part of the wider trend toward aggregator/gateway integrations.

7. **[#9179 — fix(coding-agent): reject tree navigation during compaction](https://github.com/earendil-works/pi/pull/9179)** (OPEN) — Race fix: disallows tree navigation while compaction is active and preserves prepared context on the original branch; pairs with [#9155](https://github.com/earendil-works/pi/pull/9155), which rejects prompts during tree navigation.

8. **[#9166 — feat(tui): accelerate Alt-modified wheel scrolling](https://github.com/earendil-works/pi/pull/9166)** (OPEN) — Alt+wheel now scrolls 5x faster, directly closing the fullscreen-scroll complaint in #9052.

9. **[#9163 — feat(tui): Simplify clipboard handling](https://github.com/earendil-works/pi/pull/9163)** (OPEN, mitsuhiko) — Replaces the heavyweight Rust clipboard library with a thinner native approach to enable NixOS and other builds; notable for unblocking broader platform support.

10. **[#9138 — feat(coding-agent): use Cmd+V for clipboard image paste on macOS](https://github.com/earendil-works/pi/pull/9138)** (CLOSED) — Binds image paste to `super+v` on darwin (with `ctrl+v` fallback), fixing a macOS platform-convention gap.

Also noteworthy: **[#9137 — Nix flake](https://github.com/earendil-works/pi/pull/9137)** (WIP, mitsuhiko), **[#9131 — Durable Object SQLite session backend](https://github.com/earendil-works/pi/pull/9131)**, and **[#9149 — Selector save keybindings](https://github.com/earendil-works/pi/pull/9149)** (respects `app.models.save` / new `app.thinking.save` instead of hardcoded `Ctrl+S`).

---

## Feature Request Trends
- **Provider breadth and gateway support** — The largest cluster. Requests for Bedrock Mantle ([#5363](https://github.com/earendil-works/pi/issues/5363)), Meta/Muse (PR #9096), OrcaRouter (PR #9135), and fixes for OpenRouter `:free` tiers ([#8760](https://github.com/earendil-works/pi/issues/8760)) show users actively routing Pi through many backends and expecting first-class catalog/token handling everywhere.
- **Session state preservation** — Multiple asks around making session navigation robust: pinning in the resume picker ([#9139](https://github.com/earendil-works/pi/issues/9139)), cursor preservation during model refresh (PR #9164), and rejecting navigation during compaction/background work (PR #9179).
- **Extension API growth** — Extensions want finer-grained control: a final pre-execution hook for permission-style extensions ([#9175](https://github.com/earendil-works/pi/issues/9175)), scoping hidden-thinking labels to the active streaming block ([#9161](https://github.com/earendil-works/pi/issues/9161)), and honoring rebound keybindings in selectors ([#8797](https://github.com/earendil-works/pi/issues/8797)).
- **Distributability** — Packaging and platform portability is an emerging theme: dependency-free pi-ai for browser bundles ([#9128](https://github.com/earendil-works/pi/issues/9128)), Nix flake (PR #9137), Docker sandbox docs (PR #9077), simplified clipboard for NixOS (PR #9163).

---

## Developer Pain Points
- **Release packaging regressions** — The recurring `@earendil-works/pi-server` undeclared-dependency bug (4 separate issues in 24h: [#9132](https://github.com/earendil-works/pi/issues/9132), [#9140](https://github.com/earendil-works/pi/issues/9140), [#9156](https://github.com/earendil-works/pi/issues/9156), [#9158](https://github.com/earendil-works/pi/issues/9158)) is the top frustration: users and downstream CIs can't install or run 0.85.0 at all.
- **Provider edge-case rejections** — A recurring pattern where Pi sends provider-incompatible payloads (whitespace-only tool content [#8720](https://github.com/earendil-works/pi/issues/8720), over-limit `max_tokens` [#8760](https://github.com/earendil-works/pi/issues/8760), per-message `output_config` on OpenRouter/Claude Opus 5 [#9165](https://github.com/earendil-works/pi/issues/9165), dropped root `anyOf` schemas [#9134](https://github.com/earendil-works/pi/issues/9134)). Because the bad message stays in history, these aren't transient — they brick sessions.
- **State corruption / threading hazards** — Session-scoped IDs colliding from encoded paths ([#9073](https://github.com/earendil-works/pi/issues/9073)), concurrent extension dialogs hanging ([#6978](https://github.com/earendil-works/pi/issues/6978)), and navigation/compaction races suggest the session and dialog state machines need more rigorous serialization.
- **Silent data loss** — `/export` dropping `display:false` context ([#8896](https://github.com/earendil-works/pi/issues/8896)) and the bash tool silently discarding a model-supplied `cwd` ([#5904](https://github.com/earendil-works/pi/issues/5904)) erode trust: users and models can't tell when their intent was dropped.
- **Inconsistent CLI semantics** — Exit code 0 vs 1 for the same model error depending on `--mode json` vs `--mode text` ([#9089](https://github.com/earendil-works/pi/issues/9089)) breaks scripting assumptions.
- **Undocumented environment behavior** — `PI_OFFLINE` disabling model discovery beyond its documented scope ([#8684](https://github.com/earendil-works/pi/issues/8684)) surprises users with silent capability loss; the community wants docs and behavior aligned.

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-09-05

## Today's Highlights
Maintainers are converging on **session lifecycle and transcript identity**: new PRs persist daemon `promptId` for live replay ([#11062](https://github.com/QwenLM/qwen-code/pull/11062)) and add headless turn navigation ([#11054](https://github.com/QwenLM/qwen-code/pull/11054)), while issues flag orphaned channel workers and unoverridable approvals in AUTO mode. On the ecosystem side, an OpenTUI fix for the **Cerebras multi-turn 400** error is in review ([#11049](https://github.com/QwenLM/qwen-code/pull/11049)), CI performance and repeated auto-filed E2E failures remain the hottest operational topics. No new release was published in the last 24 hours.

## Releases
No new releases in the last 24 hours.

## Hot Issues

- [#8662](https://github.com/QwenLM/qwen-code/issues/8662) — **Migrate TUI rendering layer from ink to OpenTUI (tracking)** · 30 comments. The community's most-discussed thread: ink 7 + React 19 with a ~1037-line patch causes flicker and structural rendering problems that justify a tracked migration, with a dedicated parity backlog (U-28 etc.).
- [#10908](https://github.com/QwenLM/qwen-code/issues/10908) — **CI time bound by module import cost, not scheduling** · 8 comments. Highlights a striking bottleneck: `cli` spent 2223s in `collect` vs 1372s in `tests`. Directly motivates the perf refactor in PR [#10957](https://github.com/QwenLM/qwen-code/pull/10957).
- [#10932](https://github.com/QwenLM/qwen-code/issues/10932) — **Voice dictation cannot use Token Plan ASR** · 5 comments. Voice pipeline hardcodes legacy model IDs, so `qwen-audio-3.0-asr-flash` is rejected even though mic capture works — a pure allowlist regression against Model Studio's Token Plan.
- [#10872](https://github.com/QwenLM/qwen-code/issues/10872) — **Pluggable middleware for language-aware rewriting of thinking output** · 4 comments. Requests a public middleware API (CLI + `qwen serve`) to translate reasoning text before emission, echoing long-standing complaint [#3787](https://github.com/QwenLM/qwen-code/issues/3787) that thinking is always in English.
- [#11031](https://github.com/QwenLM/qwen-code/issues/11031) — **Export embeds full Web Shell runtime in every HTML file** · 3 comments. `/export html` yields ~19.5 MB files even for empty sessions because React/Web Shell are copied per export; PR [#11035](https://github.com/QwenLM/qwen-code/pull/11035) targets this.
- [#11045](https://github.com/QwenLM/qwen-code/issues/11045) — **Cerebras: every multi-turn request fails with 400 (no body)** · 3 comments. First turn succeeds, then `reasoning_content` is sent back upstream and rejected; a provider-boundary strip like Mistral's is proposed.
- [#10936](https://github.com/QwenLM/qwen-code/issues/10936) — **DingTalk channel prints clientSecret and stream ticket to stdout** · 3 comments. A P1 security leak: full SDK config objects and `res.data` lines are logged in plaintext on every connect. Closed.
- [#11019](https://github.com/QwenLM/qwen-code/issues/11019) — **AUTO mode user approvals never reach the classifier** · 2 comments. Three affirmative user answers were ignored and tool calls remained blocked; approval mode also silently reverts to AUTO on session rebuild — a serious blocker for API-driven hosts.
- [#11063](https://github.com/QwenLM/qwen-code/issues/11063) — **Channel DELETE leaves owned workers running when config is missing** · 2 comments. Explicit delete returns `channel_instance_not_found` once persisted config is absent, even though the daemon still owns the worker — the orphan cannot be converged by repeating DELETE.
- [#11060](https://github.com/QwenLM/qwen-code/issues/11060) — **Active transcript omits the Daemon promptId** · 2 comments. While a turn is still running, transcript projections lack the `promptId` that `liveJournal` events carry, breaking canonical reconciliation for integrations that refresh mid-turn; fixed by [#11062](https://github.com/QwenLM/qwen-code/pull/11062).

## Key PR Progress

- [#11049](https://github.com/QwenLM/qwen-code/pull/11049) — **fix(core): strip reasoning_content from Cerebras requests**. Adds a Cerebras provider that removes the non-standard field at the outbound boundary (matching Mistral handling) and detects the provider by hostname.
- [#11035](https://github.com/QwenLM/qwen-code/pull/11035) — **fix(export): load transcript renderer from unpkg**. Exported HTML now carries only data/styles/bootstrap; the full React renderer ships once in the npm package, attacking the 19.5 MB export bloat.
- [#11037](https://github.com/QwenLM/qwen-code/pull/11037) — **fix(core): coalesce concurrent Config.initialize() calls**. Fixes a race where a second caller during in-flight init gets a spurious "already initialized" error instead of the real result.
- [#10943](https://github.com/QwenLM/qwen-code/pull/10943) — **feat(cli): start a background Agent View session with `--bg`**. Spawns a session that outlives the launching shell and prints the session id, enabling headless background automation.
- [#11046](https://github.com/QwenLM/qwen-code/pull/11046) — **fix(cli): wait for the startup chat before an OpenTUI turn sends**. Early keystrokes were silently dropped because the composer accepted input before `Chat not initialized` resolved.
- [#11062](https://github.com/QwenLM/qwen-code/pull/11062) — **fix(daemon): persist prompt identity for active transcript replay**. Exposes trusted `promptId` as `_meta` on the user record before turn settlement, unblocking live-journal correlation (fixes [#11060](https://github.com/QwenLM/qwen-code/issues/11060)).
- [#10957](https://github.com/QwenLM/qwen-code/pull/10957) — **perf(cli): import core modules directly instead of package root**. Migrates to targeted resolver imports to cut the module-import overhead dominating CI (addresses [#10908](https://github.com/QwenLM/qwen-code/issues/10908)).
- [#10991](https://github.com/QwenLM/qwen-code/pull/10991) — **refactor(daemon): decouple extension activation refresh**. Activation completes after durable commit; a new capability flag lets clients distinguish old vs new daemon contracts.
- [#10938](https://github.com/QwenLM/qwen-code/pull/10938) — **feat(web-shell): make Session Workflow dependencies navigable and quiet its chrome**. Redesigns the plan DAG to lead with step meaning, and cleans up inspector chrome after the earlier workflow surface (#8583).
- [#11054](https://github.com/QwenLM/qwen-code/pull/11054) — **feat(web-shell): add headless global turn navigation**. Phase 2A: bounded turn-index cache, historical page ranges, and provisional prompt reconciliation as a data layer for later UI phases.

## Feature Request Trends

- **Session lifecycle governance.** Strong demand for bounded/owned sessions: worktree-session cleanup and orphan reaping ([#11024](https://github.com/QwenLM/qwen-code/issues/11024)), channel `sessionRotation` ([#8927](https://github.com/QwenLM/qwen-code/pull/8927)), and mode/session-state persistence across rebuilds ([#11019](https://github.com/QwenLM/qwen-code/issues/11019)).
- **Web Shell as a full product surface.** Requests lean toward richer standalone UI: independent Quick Chat panel ([#11017](https://github.com/QwenLM/qwen-code/issues/11017)), navigable plan graphs ([#10938](https://github.com/QwenLM/qwen-code/pull/10938)), scheduled-task model/group routing ([#10885](https://github.com/QwenLM/qwen-code/pull/10885)).
- **Pluggable, middleware-style extension points.** Users want public hooks for transforming thinking output ([#10872](https://github.com/QwenLM/qwen-code/issues/10872)), parity with Claude Code dynamic workflows ([#11013](https://github.com/QwenLM/qwen-code/issues/11013)), and workspace-scoped Skills runtimes ([#10697](https://github.com/QwenLM/qwen-code/pull/10697)).
- **Portable configuration & distribution.** Per-process `--config-dir` ([#10984](https://github.com/QwenLM/qwen-code/issues/10984)) and a fresh `@qwen-code/sdk` release carrying merged memory/prompt-cache fixes ([#11022](https://github.com/QwenLM/qwen-code/issues/11022)).
- **Provider/model compatibility hygiene.** Recurring theme: hardcoded model IDs and vendor quirks break integrations — ASR family IDs ([#10932](https://github.com/QwenLM/qwen-code/issues/10932)) and `reasoning_content` leakage ([#11045](https://github.com/QwenLM/qwen-code/issues/11045)).

## Developer Pain Points

- **Main-branch CI keeps failing before tests report.** Multiple auto-filed failures ([#11027](https://github.com/QwenLM/qwen-code/issues/11027), [#11043](https://github.com/QwenLM/qwen-code/issues/11043), [#11061](https://github.com/QwenLM/qwen-code/issues/11061)) show OpenTUI interactive and macOS shards breaking intermittently — a sign the new renderer's test harness is still stabilizing.
- **CI time dominated by module imports, not tests.** 2223s of collection vs 1372s of execution ([#10908](https://github.com/QwenLM/qwen-code/issues/10908)) makes every release run painfully slow; the direct-import fix ([#10957](https://github.com/QwenLM/qwen-code/pull/10957)) is eagerly anticipated.
- **Credential leakage in logs.** DingTalk connect dumping `clientSecret` and stream tickets to stdout ([#10936](https://github.com/QwenLM/qwen-code/issues/10936)) undermines trust for channel users; expect lint/redaction follow-ups.
- **Test fragility from environment assumptions.** The SIGKILL test's `pgrep` pattern only matches checkouts whose path contains "qwen" ([#11066](https://github.com/QwenLM/qwen-code/issues/11066)), and interactive PTY cleanup doesn't wait for child exit ([#11001](https://github.com/QwenLM/qwen-code/pull/11001)).
- **Windows and local docs workflows lag.** `npm run link` requires Developer Mode/admin due to symlinks ([#11055](https://github.com/QwenLM/qwen-code/issues/11055)) — minor, but friction for new contributors on standard Windows shells.


</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI Community Digest — 2026-09-05

Issue/PR links below use the `Hmbown/Codewhale` repository path, matching the activity payload.

## 1. Today's Highlights

No release was cut in the last 24 hours. Development activity is focused on two notable bug fixes: removing stale to-do-list transcript cards from the TUI (#5873) and correcting the input-context collapse seen with local Ollama models (#5883). Maintainers also restored contributor CI so outside PRs can be validated again (#5882).

## 2. Releases

No new releases in the last 24 hours; no release notes to summarize.

## 3. Hot Issues

Only five issues were updated in the last 24h, so the full set is listed below.

- [**#5820 — Ollama provider: input budget collapses to 1024 tokens on 32K local models**](https://github.com/Hmbown/Codewhale/issues/5820)  
  A high-impact bug for local-model users: when no static catalogue row exists, the automatic output reservation can exceed the model's entire context window and clamp input to 1024 tokens. The thread has 4 comments and may be addressed directly by PR #5883.

- [**#5860 — [enhancement] Continuous Self-Learning from Dialog**](https://github.com/Hmbown/Codewhale/issues/5860)  
  Users want the agent to automatically detect repeated problem-solving patterns and evolve its `SKILL.md` knowledge over time instead of relying only on manually written skills. 3 comments; active design discussion.

- [**#5871 — [bug] To-do list history clutters the transcript**](https://github.com/Hmbown/Codewhale/issues/5871)  
  Every `todo_write` snapshot remains as a permanent card, creating a “push-down history” users cannot clear without losing context. Now closed; PR #5873 is the associated fix.

- [**#5872 — [enhancement] Add rusty_alloc as an opt-in feature next to mimalloc**](https://github.com/Hmbown/Codewhale/issues/5872)  
  Suggests a pure-Rust allocator path so contributors no longer need a C compiler/build script for the allocation backend. Relevant to TUI/agent build portability.

- [**#5866 — Key Ophthalmology CPT & ICD-10 Updates for 2026**](https://github.com/Hmbown/Codewhale/issues/5866)  
  Clearly off-topic promotional content from `medicalbilling-usa`. Included only for completeness; not a genuine developer issue.

## 4. Key PR Progress

- [**#5873 — fix(tui): replace stale todo transcript snapshots**](https://github.com/Hmbown/Codewhale/pull/5873)  
  Keeps only the newest successful `todo_write` snapshot visible and hides empty current snapshots without discarding stored conversation context. Fixes #5871; 9 TUI tests reported passing.

- [**#5883 — fix(tui): derive local output budget from route window**](https://github.com/Hmbown/Codewhale/pull/5883)  
  Targets the Ollama context-collapse issue. Derives output reservation from the route's declared context window when no static model row exists, while preserving explicit operator overrides and existing clamping behavior.

- [**#5882 — test: restore contributor CI baseline**](https://github.com/Hmbown/Codewhale/pull/5882)  
  Restores CI so unrelated PRs can be evaluated against a working baseline. Fixes trust-token fixtures, Windows symlink tests, pointer assertion formatting, and formatter/doc-comment mismatches.

- [**#5870 — Fix: Tools: atomic commit splitting — order unrelated changes by dependency**](https://github.com/Hmbown/Codewhale/pull/5870)  
  Implements dependency ordering for atomic commit splitting and rejects cyclic dependencies. Addresses issue #3999; generated with AI assistance and manually scoped by the author.

- [**#5881 — chore(deps): bump tower-http from 0.7.0 to 0.7.1**](https://github.com/Hmbown/Codewhale/pull/5881)  
  Routine dependency update.

- [**#5875 — chore(deps): bump base64 from 0.22.1 to 0.23.1**](https://github.com/Hmbown/Codewhale/pull/5875)  
  Routine Rust dependency update; needs a release-note review before merge.

- [**#5876 — chore(deps): bump lru from 0.18.2 to 0.18.3**](https://github.com/Hmbown/Codewhale/pull/5876)  
  Minor caching-library update, low risk.

- [**#5880 — chore(deps): bump jsonschema from 0.46.10 to 0.52.1**](https://github.com/Hmbown/Codewhale/pull/5880)  
  Large version jump across jsonschema releases; likely worthwhile but should be reviewed for behavior changes.

- [**#5877 — chore(deps): bump rmcp from 2.2.0 to 3.2.0**](https://github.com/Hmbown/Codewhale/pull/5877)  
  Major update to the MCP Rust SDK. Watch for breaking API/type changes before merge.

- [**#5828 — chore(deps): bump npm_and_yarn group across 2 directories**](https://github.com/Hmbown/Codewhale/pull/5828)  
  Updates `qs` in the Feishu bridge and both `qs` and `fast-uri` in the VS Code extension directories.

## 5. Feature Request Trends

- **Conversation-aware skill evolution:** Issue #5860 signals demand for self-improving skills — recognizing repeated problem types automatically and extracting reusable knowledge into `SKILL.md`.

- **Cleaner transcript/state management:** The todo-list clutter bug (#5871) reflects a broader desire for ephemeral UI snapshots that do not permanently pollute the conversation transcript or hidden context.

- **Better local-model context handling:** Users expect context-window enforcement to respect actual model capacity rather than applying a generic/overly large output reservation (#5820).

- **Portable build tooling:** Issue #5872 points toward reducing C/build-script requirements by offering a pure-Rust allocator option like `rusty_alloc`.

## 6. Developer Pain Points

- **Silent context-budget collapse:** Ollama users with 32K models see input budgets shrink to 1024 tokens due to a 64K default output reservation, making local models nearly unusable until manually diagnosed.
- **Transcript pollution from tool-state cards:** Permanent `todo_write` snapshots stack into an unmanageable history, and clearing the to-do list does not remove earlier visual clutter.
- **Manual knowledge maintenance burden:** Users find maintaining static `SKILL.md` files tedious and want automatic pattern extraction from real usage.
- **C toolchain friction:** Pulling in `mimalloc` forces contributors to have a C compiler/build setup; a pure-Rust path would lower contribution barriers.
- **Contribution CI instability:** PR #5882 shows the contributor CI baseline had regressed badly enough that unrelated PRs could not be evaluated reliably.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*