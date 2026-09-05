# OpenClaw 生态日报 2026-09-05

> Issues: 500 | PRs: 500 | 覆盖项目: 13 个 | 生成时间: 2026-09-05 03:59 UTC

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

## OpenClaw 项目深度报告

# OpenClaw 项目动态日报 — 2026-09-05

## 今日速览

过去24小时项目活跃度极高：共产生 500 条 Issue 更新（新开/活跃 395 条，关闭 105 条）及 500 条 PR 更新（待合并 348 条，合并/关闭 152 条），显示社区反馈与维护节奏均在高位运转。但今日新版本发布为 0，且高严重度问题（P0/P1）仍密集，多个长期积压的稳定性议题（crash-loop、消息丢失、会话状态异常）仍在推进中值得关注。没有新版本释出，意味着大量已合并的修复要等到下一批次统一发布，短期内用户侧的痛感可能持续。整体来看，项目处于**高活跃度、高积压、稳定版本待发布**的密集攻坚窗口。


## 版本发布

今日无新版本发布（最新 Releases 为空）。


## 项目进展

今日无显式标注为已合并的 PR 数据。以下为观察到的关键进展信号：

- **高讨论度 Bug 闭环**：#104721（所有工具结果返回字面量 "(see attached image)" 占位符而非实际内容）从 OPEN 状态转为 CLOSED，这是一个影响面极广的 P0 回归问题，涉及会话状态与消息丢失，关闭意味着该严重问题已有解决方案。
- **Matrix 线程回复回归获修复**：#87307 标记为 CLOSED，该问题涉及 Matrix 线程回复被发送为普通回复、`/status` 与 `/model` 命令静默失效，修复将改善 Matrix 渠道的用户体验。
- **Codex OAuth 刷新故障问题关闭**：#86215 从 OPEN 转为 CLOSED，该问题会导致 agent 在 OAuth 令牌失效后长时间无告警地卡在重试循环中，修复后将显著提升 Codex 继承认证的可靠性。
- **cron 工具 JSON Schema 兼容性修复**：#107449 已关闭，解决了 cron 工具 schema 中 `pattern: "\S"` 与 llama.cpp 工具解析器不兼容的回归问题。
- **开发管线活跃**：#122846（per-response tool-call block cap）等大型 PR 仍处于开放状态，覆盖范围横跨几乎所有 channel 与 extension，说明 agent-core 层的防御性改进仍在持续推进，但目前未合并意味着这些改进尚未对用户生效。

整体评价：今日项目向前推进主要体现为**多个长期 P0/P1 问题的关闭**，而非新功能落地。修复主要集中于消息丢失、认证故障与渠道兼容性，属于"还旧账"式的稳定性收敛。


## 社区热点

今日讨论最活跃的议题集中在核心运行时稳定性与消息丢失，用户诉求高度一致：**“我的消息/任务为什么被静默丢弃或卡死？”**

### 1. #91009 — Codex PreToolUse hook 进程 CPU 飙升与 gateway RPC 停滞（21 条评论）
**状态**: OPEN | **标签**: P0, crash-loop, platinum hermit
**链接**: https://github.com/openclaw/openclaw/issues/91009

`@openclaw/codex` 集成在工具调用前会 spawn 多个 `openclaw-hooks` relay 进程，每个进程消耗 100%+ CPU，导致 gateway RPC 停滞。该问题自 2026-06-06 创建至今已近三个月仍未关闭，评论数 21 条居首，说明影响范围大、复现路径复杂，且修复 PR 尚未到位。

### 2. #48003 — Steer 模式无法在 turn 中途注入消息（20 条评论）
**状态**: OPEN | **标签**: P1, session-state, diamond lobster
**链接**: https://github.com/openclaw/openclaw/issues/48003

`messages.queue.mode: "steer"` 配置下，用户消息要等到当前 turn 完成后才会被处理，而非在工具边界处注入。这是交互体验的核心缺陷，已持续近 6 个月，社区持续关注。

### 3. #104721 — 所有工具结果返回字面占位符（17 条评论）✅ 已关闭
**链接**: https://github.com/openclaw/openclaw/issues/104721

“文件读取返回字面字符串 ‘(see attached image)’ 而非文件内容” — 用户在描述中直接用了 "completely broken"。虽然今日已关闭，但其 P0 属性与 message-loss 影响说明这曾是一个急需热修的严重回归。

### 4. #115908 — 会话 transcript 投影在持续写入下 livelock（15 条评论）
**状态**: OPEN | **标签**: P1, crash-loop, diamond lobster
**链接**: https://github.com/openclaw/openclaw/issues/115908

同步重建路径占用 Node 主线程导致事件循环阻塞数十秒。与 #119720（同步持久化阻塞 Gateway 事件循环）形成呼应，指向**核心架构层面同步 I/O 过多**的系统性问题。社区对此类问题的讨论热度持续不减。

**社区热点分析**：今日讨论热度最高的议题并非新功能，而是集中在**运行时可靠性**上：CPU 飙高、事件循环阻塞、消息不交付。用户的核心诉求是“基础能力先稳下来”。


## Bug 与稳定性

按严重程度排列今日值得关注的 Bug（含活跃与今日关闭的）：

### P0 — 严重

| Issue | 标题 | 状态 | Fix PR |
|-------|------|------|--------|
| [#91009](https://github.com/openclaw/openclaw/issues/91009) | Codex PreToolUse hook 进程 CPU 100%+，gateway RPC 停滞 | OPEN（已 3 个月） | ❌ 无 |
| [#104721](https://github.com/openclaw/openclaw/issues/104721) | 所有工具结果返回 "(see attached image)" 字面量而非实际内容 | ✅ CLOSED | — |
| [#108435](https://github.com/openclaw/openclaw/issues/108435) | 升级到 2026.7.1 后 gateway 无法启动（systemd/ollama/手动均失败） | OPEN | ❌ 无 |
| [#70903](https://github.com/openclaw/openclaw/issues/70903) | 基于文件的 provider 冷却在用户充值后仍持续阻止请求数小时 | OPEN（已 4 个月+） | ❌ 无 |

### P1 — 高

| Issue | 标题 | 状态 | Fix PR |
|-------|------|------|--------|
| [#131150](https://github.com/openclaw/openclaw/issues/131150) | Slack DM 在 gateway 重启后被静默丢弃（19 账号 socket mode） | OPEN | ❌ 无 |
| [#112259](https://github.com/openclaw/openclaw/issues/112259) | 可见入站消息被静默丢弃：零 payload dispatch 无重试/死信 | OPEN | ❌ 无 |
| [#97616](https://github.com/openclaw/openclaw/issues/97616) | Hook/tool 子进程泄漏导致僵尸进程累积 | OPEN | ❌ 无 |
| [#90944](https://github.com/openclaw/openclaw/issues/90944) | sessions_yield 恢复回复已记录但未投递 | OPEN | ❌ 无 |
| [#129314](https://github.com/openclaw/openclaw/issues/129314) | 内部运行时上下文消息偶尔作为独立可见 turn 发出 | OPEN | ❌ 无 |
| [#87307](https://github.com/openclaw/openclaw/issues/87307) | Matrix 线程回复被当作普通回复；/status、/model 静默 | ✅ CLOSED | — |
| [#86215](https://github.com/openclaw/openclaw/issues/86215) | Codex OAuth 刷新失败可致 agent 卡死数小时 | ✅ CLOSED | — |
| [#107449](https://github.com/openclaw/openclaw/issues/107449) | cron 工具 JSON Schema 与 llama.cpp 解析器不兼容 | ✅ CLOSED | — |

### 值得关注的新增问题

- **#138272**（2026-09-04 新建，OPEN，7 评论）— **Android Talk 实时语音在任务型 turn 中持续掉线**："no live response owner" 错误，已在 2026.7.1-2 → 2026.8.2 → 2026.9.1 三个版本上复现。移动端实时语音是重要场景，此问题虽然新开但标记为 P1，需要密切关注。
- **#135704**（2026-09-02，OPEN）— iMessage 反射消息绕过 echo cache，导致自己的出站消息被当作新入站文本。消息环路问题在渠道接入层仍然存在。

**Bug 与稳定性趋势判断**：今日关闭的 105 个 Issue 中包含多个 P0/P1 级长期问题，是积极的信号。但大量 P0/P1 仍然缺少 fix PR，尤其是 #91009 与 #108435 这类影响 gateway 可用性的严重问题，三到四个月未能解决，说明 **gateway 核心链路的复杂性与维护瓶颈**值得关注。


## 功能请求与路线图信号

今日无合并 PR，因此没有新功能正式落地。但以下开放 PR 与功能请求信号较强，可能被纳入下一版本：

### 开放 PR 中功能指向明确的

- **#136257**（feat: direct model lists and provider login across surfaces）— 将模型列表与 provider 登录统一到 Gateway models.list/CLI/Control UI，消除平行实现。标志着下一版本可能在模型管理 UX 上有较大改善。
- **#122846**（per-response tool-call block cap）— agent-core 层防御性改进：对单次响应的工具调用数量设置上限，防止 CLI loopback 缓冲区溢出导致所有工具结果 `unknown`。
- **#125861**（feat: Tavily keyless capability）— 允许 Tavily 无 API key 使用（有限速），降低 web search 的入门门槛。
- **#109337**（feat: featured plugin icons）— 为第一方精选插件补充图标元数据，属于 UI 打磨类增强。

### 值得关注的功能请求

- **#53763**（12 评论）— 内置 headless 浏览器以摆脱对外部 Chrome/第三方 API 的依赖。评论数较高说明 web 访问可靠性是用户的真实痛点。
- **#16670**（8 评论，P2）— 要求 onboarding wizard 将 Memory/Embedding 配置设为强制步骤。2 月创建至今已近 7 个月，用户仍在持续表达诉求。
- **#6757**（8 评论，P2）— Agent 自主触发上下文压缩（self-compact tool）。创建于 2 月初，长期未获维护者明确回应。
- **#28300**（7 评论，👍 5）— Control UI 主题定制系统（预置主题 + 自定义主题工作室）。👍 数较高，社区呼声较强。

整体来看，**功能请求积压明显**：大量 P2/P3 功能请求停留在 `needs-product-decision` / `needs-maintainer-review` 状态长达数月，维护者带宽受限是当前项目的主要瓶颈之一。


## 用户反馈摘要

从今日 Issues 评论中提炼出的真实用户声音：

### 痛点 1：消息静默丢失是最大的信任杀手
> #112259："A visible inbound channel message can be accepted by the channel turn kernel and then silently discarded... no agent run is ever created, nothing is persisted."

> #90944："user gets child raw summary, not parent reply" — 用户收到的是子代理的原始摘要，而非父会话应有的回复。

**用户期望**：即便是失败，也要有明确、可见的错误提示；静默丢弃会让用户对系统完全失去信任。

### 痛点 2：升级带来的回归让用户疲惫
> #108435："gateway doesn't start with systemd / ollama / manual launch"（升级 2026.7.1 后）

> #104721："This is completely broken — the actual data is being replaced with a placeholder string, not just displayed wrong."

**用户期望**：稳定版（stable）升级不应携带如此严重的回归；对 regression 的自动化测试覆盖需要加强。

### 痛点 3：多账号/大规模部署的边缘情况频繁触发
> #131150（19 个 Slack 账号、socket mode、频繁重启）：所有账号的 DM 在 gateway 重启后被静默丢弃。

**用户期望**：多账号部署场景下，身份与路由逻辑需要更健壮的恢复机制。

### 痛点 4：授权/认证失败恢复路径粗糙
> #70903：即使充值恢复，provider cooldown 仍然阻止请求数小时。

> #86215：OAuth 刷新失败导致 agent 在一个坏掉的认证通道上重试数小时，不轮换、不告警。

**用户期望**：认证失败后应快速失败并轮换，而不是长时间"安静地卡住"。

### 正面声音
- 👍 数较高的 #28300（主题系统，5 👍）与 #104721（3 👍）说明用户对 UI/UX 改进有持续热情；用户愿意为美化与控制力投票。


## 待处理积压

以下为长期未响应/未解决的重要 Issue，值得维护者重点关注：

| Issue | 创建时间 | 持续时间 | 严重度 | 标签 | 备注 |
|-------|---------|---------|--------|------|------|
| [#6757](https://github.com/openclaw/openclaw/issues/6757) | 2026-02-02 | ~7 个月 | P2 | needs-maintainer-review, needs-product-decision | Agent 自主压缩上下文的请求长期无维护者回应 |
| [#16670](https://github.com/openclaw/openclaw/issues/16670) | 2026-02-15 | ~7 个月 | P2 | needs-maintainer-review, needs-product-decision | Onboarding 缺少 Memory/Embedding 配置 |
| [#91009](https://github.com/openclaw/openclaw/issues/91009) | 2026-06-06 | ~3 个月 | **P0** | needs-maintainer-review, needs-product-decision, needs-live-repro | Codex hook 进程 CPU 飙高，**无 fix PR** |
| [#70903](https://github.com/openclaw/openclaw/issues/70903) | 2026-04-24 | ~4.5 个月 | **P0** | needs-product-decision, source-repro | Provider 冷却持久化阻塞用户，**无 fix PR** |
| [#108435](https://github.com/openclaw/openclaw/issues/108435) | 2026-07-15 | ~2 个月 | **P0** | needs-info | Gateway 无法启动，**无 fix PR** |
| [#48003](https://github.com/openclaw/openclaw/issues/48003) | 2026-03-16 | ~6 个月 | P1 | needs-maintainer-review, needs-product-decision | Steer 模式不注入消息，核心交互缺陷 |
| [#53763](https://github.com/openclaw/openclaw/issues/53763) | 2026-03-24 | ~5.5 个月 | P3 | needs-maintainer-review, needs-product-decision | 内置 headless 浏览器，12 条评论 0 赞，讨论度高但无维护者跟进 |

### 积压分析

**健康度隐忧**：
- 500 条 Issue 更新中 395 条为新开/活跃，这个体量对维护者团队是极大的压力。
- 多个 P0 级问题（#91009、#70903、#108435）挂起数月且无 fix PR，说明**高严重度 bug 的修复周期过长**。
- 大量 P2/P3 功能请求齐刷刷带着 `needs-maintainer-review` + `needs-product-decision` 标签，表明维护者的评审带宽严重受限，积压已是系统性现象。

**积极信号**：
- 今日关闭了 105 个 Issue（含多个 P0/P1），说明积压虽然在增长，但清理工作也在同步推进。
- 152 个 PR 被合并/关闭，尽管今日展示的 PR 多为新提交，长期来看 PR 处理管线仍然通畅。

**建议关注**：
1. P0 级 #91009（Codex hook CPU 飙高）是唯一一个已持续三个月以上的 P0 crash-loop 类问题，应优先协调资源攻坚。
2. PR #127992 直接修复 P1 Issue #95840（OpenAI 模型 cache-ttl pruning 不生效），值得在下一版本中合入。
3. 新开的 #138272（Android Talk 掉线）已跨三个版本复现，移动端语音为增长赛道，建议尽早介入。

---

*本日报基于 2026-09-05 的 GitHub 公开数据自动生成。所有链接指向 openclaw/openclaw 仓库。*

---

## 横向生态对比

# 个人 AI 助手开源生态横向对比分析报告

**报告日期：2026-09-05**
**数据窗口：过去 24 小时**


## 1. 生态全景

当前个人 AI 助手/自主智能体开源生态正处于**高活跃度与深度攻坚并存的密集迭代期**：头部项目（OpenClaw、ZeroClaw、Hermes Agent、CoPaw）单日合计产生超过 600 条 Issue 与 660 条 PR 更新，但同时普遍面临"新版本发布停滞"与"P0/P1 级稳定性缺陷修复周期过长"的双重压力。各项目不约而同地将研发重心从"新功能开拓"转向**运行时可靠性收敛**——消息静默丢失、认证失败恢复粗糙、任务生命周期状态不一致成为跨项目的高频痛点。与此同时，渠道层（Telegram、飞书、Slack、Matrix、IRC）的体验精细化与连接鲁棒性正在成为差异化竞争的新战场，而多租户/企业化部署（Hub 多租户、统一会话、PostgreSQL 支持）与外部 Agent 生态互操作（OpenCode、Codex、ACP）则代表了下一阶段的架构演进方向。整体来看，生态正处于"从可用走向可信、从单机走向多 Agent 协同"的质变前夜。

## 2. 各项目活跃度对比

| 项目 | Issues 更新 | PR 更新 | 新版本 | PR 合并/关闭 | 活跃度 | 健康度评估 |
|------|------------|---------|--------|-------------|--------|-----------|
| **OpenClaw** | 500（新开/活跃 395，关闭 105） | 500（待合并 348，合并/关闭 152） | 0 | 152 | 🔥 极高 | ⚠️ 高积压；多个长期 P0 关闭是积极信号，但 P0 无 fix PR 问题仍存 |
| **ZeroClaw** | 34（新开/活跃 24，关闭 10） | 50（待合并 44，合并/关闭 6） | 0 | 6 | 🔥 高 | ✅ 较健康；v0.8.5 发布在即，RFC 密集讨论，但通道兼容层问题仍在暴露 |
| **Hermes Agent** | 50（新开/活跃 47，关闭 3） | 50（待合并 47，合并/关闭 3） | 0 | 3 | 🔥 高 | ⚠️ Review 吞吐是瓶颈；P1 重复报告多（SSH 401、推理显示），修复供给充足但合入慢 |
| **CoPaw** | 23（新开/活跃 15，关闭 8） | 26（待合并 20，合并/关闭 6） | 0 | 6 | 🔥 高 | ✅ 总体良好；MCP 白名单安全修复落地，但任务停止状态不一致问题待解 |
| **LobsterAI** | 1（更新 1） | ~33（合并/关闭 28，待合并 5） | **2**（2026.9.4 / 2026.9.3） | 28 | 🟢 高（内部驱动） | ✅ 版本节奏快；但社区反馈信号弱，存储层严重缺陷积压 159 天 |
| **NanoBot** | 5（新开/活跃 2，关闭 3） | 28（待合并 21，合并/关闭 7） | 0 | 7 | 🟢 中高 | ✅ 较健康；WebUI 可观测性与内存治理双主线推进，heartbeat PR 积压 70+ 天 |
| **PicoClaw** | 3（新开 1，关闭/更新 2） | 22（新开 2，关闭/合并 20） | 0 | 20（大量为 stale 清理） | 🟡 中 | ⚠️ Stale 机器人批量关闭高质量 PR，维护者带宽不足是核心风险 |
| **IronClaw** | 3（新开 0，关闭 2） | 12（待合并 9，合并 3） | 0 | 3 | 🟢 中高 | ✅ 健康；Bug 约 8 天内闭环，Telegram UX 系统整治中 |
| **NanoClaw** | 2（全部新开） | 18（15 待合并，3 关闭） | 0 | 3 | 🟢 中高（写入侧） | ✅ 开发活跃；但合并速度慢，安全类 PR 等待超 6 天 |
| **Moltis** | 1（新开） | 1（新开待审查） | 0 | 0 | 🔵 低 | ✅ 无压力；核心关注 PR #1258 review 推进 |
| **NullClaw** | 1（更新） | 0 | 0 | 0 | 🔵 极低 | ✅ 无恶化但响应慢，仅 1 个积压 12 天的 enhancement |
| **TinyClaw** | — | — | — | — | ⚫ 无活动 | — |
| **ZeptoClaw** | — | — | — | — | ⚫ 无活动 | — |

## 3. OpenClaw 在生态中的定位

**社区规模与 issue 量级断层领先。** OpenClaw 单日 500 条 Issue + 500 条 PR 更新的体量，是第二梯队（ZeroClaw、Hermes Agent）的 10 倍以上，表明其用户基数与社区参与度在生态中处于绝对头部。其高讨论度问题（如 #91009 的 21 条评论）能在单个 Issue 上聚集相当于小型项目全天的互动量。

**技术路线：全渠道、全模型、重运行时。** 相较于 NanoBot/IronClaw 聚焦 WebUI 与单一渠道体验，OpenClaw 的技术版图横跨 Codex/Matrix/Slack/Android Talk/iMessage 等几乎所有主流渠道与模型后端，体系复杂度最高。这也决定了其 bug 面最广——crash-loop、消息丢失、会话状态异常等"大规模网关综合征"是其他小体量项目较少遭遇的。

**当前处境：从快速扩张转向稳定性还债。** 与 LobsterAI（2 个 Release/日）和 IronClaw（bug 一周内闭环）相比，OpenClaw 今日 0 Release、348 条 PR 待合并、多个 P0 积压 3-4 个月，反映出维护者带宽与社区产出量之间存在显著剪刀差。但另一方面，今日关闭多个 P0/P1 也说明其问题清理机制仍在运转。OpenClaw 在生态中的角色更像是**"基础设施层"**——它的架构决策与踩坑经验（如同步 I/O 过多、静默丢弃、认证恢复）对其他项目具有风向标意义。当 OpenClaw 修复某个 gateway 级问题时，往往意味着整个品类共同面对的深层架构挑战有了参考答案。

## 4. 共同关注的技术方向

| 技术方向 | 涉及项目 | 具体诉求 |
|---------|---------|---------|
| **消息静默丢弃 / 会话状态不可信** | OpenClaw（#112259、#90944、#131150）；CoPaw（#7559/#7567 停止后实际仍在执行）；Hermes Agent（SSH 401 三连报）；ZeroClaw（#10593 cron 静默不生效）；NanoClaw（#3716 OOM） | 用户对"系统安静地做错事/不做事"的容忍度降至最低。失败必须有明确可见的错误路径，UI 状态必须与后端真实状态严格一致 |
| **认证/OAuth 失败恢复路径粗糙** | OpenClaw（#86215 OAuth 刷新卡死、#70903 provider 冷却不解除）；Hermes Agent（#103366 SSH 401 陈旧 token）；NanoBot（#5661 OpenCode header 强制）；ZeroClaw（#10603 OpenCode 缺 session 头） | 凭据失效后应快速失败并轮换/告警，而非长时间卡死在重试循环中；外部生态的认证协议变更需要快速跟随 |
| **cron/后台任务"看似开启实则失灵"** | OpenClaw（#107449 cron schema）；ZeroClaw（#10593/#10594 backup cron 静默失败、无日志）；Hermes Agent（#98022 陈旧 receipt 触发无限 restart） | 定时/后台任务的执行状态需要可观测：成功/失败/未执行都应有记录，配置不能"静默失效" |
| **WebUI/桌面端可观测性与体验打磨** | NanoBot（#5631 context/token/速度可视化，已落地）；Hermes Agent（#49664 推理过程无法隐藏）；PicoClaw（#3281 输入框长会话卡顿）；IronClaw（WebUI 斜杠命令系列 PR）；LobsterAI（文本编辑菜单、登录引导弹窗）；CoPaw（Loop 模式 UI 一致性） | 用户需要感知模型"是否在认真工作"（token/速度/上下文占用），同时对 UI 状态一致性要求极高 |
| **渠道消息聚合与流式体验** | NanoBot（#5567 飞书多消息轰炸）；PicoClaw（#3287 IRC 长消息拆分）；OpenClaw（#87307 Matrix 线程回复）；LobsterAI（#2617 浏览器登录交互）；CoPaw（#7541 会话不应按渠道分裂） | 在 IM 场景下，用户期望"发一条消息 → 收到一条结构化回复"，而非多条碎片消息；渠道只是传输层，会话应跨渠道统一 |
| **多 Agent/多账号/多租户部署的可靠性** | CoPaw（#7318 Hub 多租户、#7558 PostgreSQL）；Hermes Agent（#103375 多 profile 后端池耗尽、#103339 state.db 损坏）；OpenClaw（#131150 19 个 Slack 账号 DM 丢失）；ZeroClaw（#9487 RFC 会话所有权） | 从单用户单会话走向多用户多 Agent 规模化部署时，会话隔离、状态一致、存储可靠性成为刚需 |
| **MCP 权限与安全硬化** | CoPaw（#7504 per-tool 白名单强制执行）；NanoClaw（#3680 mount 绕过修复）；PicoClaw（#3337 MCP 失败致 Agent 挂起）；LobsterAI（#2615 浏览器 MCP Windows 路径） | MCP 正在成为事实标准，但权限边界、失败降级、进程隔离的安全实践仍在补课 |
| **Windows/桌面端兼容性** | Hermes Agent（#103398 terminal 挂起、#103400 QuickEdit 卡死）；CoPaw（#7554 Shell stdin 挂起）；LobsterAI（#2615 Unicode 路径 MCP 失败） | Windows 用户的边缘情况频繁触发，桌面端跨平台测试覆盖普遍不足 |

## 5. 差异化定位分析

| 项目 | 功能侧重 | 目标用户 | 技术架构特征 |
|------|---------|---------|-------------|
| **OpenClaw** | 全渠道、全模型的消息网关 + Agent 运行时 | 高阶自托管用户/开发者，需要多平台统一入口 | 多 channel adapter + 统一 gateway；Node.js 主线程事件循环；积压大量架构级改造（per-response tool-call cap 等） |
| **Hermes Agent** | 本地优先的桌面助手 + SSH 远程 + Group Chat | 技术型个人用户，macOS/Windows 桌面端 | "Desktop + 多 profile gateway"模式；Bot 磁贴/池机制；群聊跨网关运输（进行中） |
| **CoPaw** | 多 Agent 编排 + 长任务执行 + Driver 生态 | 从个人到团队的进阶用户，偏 Agent 工作流自动化 | 2.x 多 Driver 架构；Skill/workspace 体系；Hub 多租户（进行中）；火山引擎/国产模型适配较好 |
| **NanoBot** | 轻量级多 IM 网关 + WebUI | 个人开发者，快速部署 IM 机器人 | Rust 实现，单项二进制部署，OpenAI-compatible provider 连接；WebUI 演进活跃 |
| **ZeroClaw** | 多通道消息 + 桌面端 + 发布治理 | 自托管社区，关注渠道广度与桌面使用 | Rust workspace 多 crate 架构；crates.io 发布管道刚打通；RFC 驱动的架构演进 |
| **LobsterAI** | 桌面工作台 + 内嵌浏览器 + 协作/订阅商业化 | 中文用户为主的 C 端产品型用户 | Electron 桌面应用 + 内嵌浏览器（MCP 连接）；订阅恢复/登录引导等商业链路完整 |
| **PicoClaw** | 轻量频道机器人（Telegram/IRC 等） | 个人/社区自托管轻量使用 | Go 实现，偏轻量级、无重依赖；维护者带宽紧缺 |
| **IronClaw** | Telegram + WebUI + 子代理可靠性 | Telegram 重度用户 | Rust 实现，产品化程度高；Telegram MTProto + Bot API 双路支持；子代理（subagent）设计债持续偿还 |
| **NanoClaw** | 本地开发 + Chat SDK + 多 Agent 通信 | 开发者/工程师，Embedded Agent 场景 | 多重语言的 SDK 桥接（Chat SDK Bridge）；容器化运行沙箱；operator 配置覆盖机制 |
| **NullClaw** | 极轻量 Agent 框架 | Zig 生态开发者 | Zig 实现，依赖极少；活动极低，维护风险 |

## 6. 社区热度与成熟度分层

**第一梯队：核心基础设施攻坚期（OpenClaw）**
体量最大、问题最复杂。当前处于"高活跃、高积压、稳定版本待发布"的密集攻坚窗口。值得关注的是今日无新 Release，但大量合并修复等待统一发布——这意味着下一版本将是重要的稳定性里程碑。

**第二梯队：高速迭代期（ZeroClaw、Hermes Agent、CoPaw、LobsterAI）**
- **ZeroClaw**：v0.8.5 即将发布（版本号已 bump），RFC 驱动架构演进，处于"收尾发布 + 远期规划"并行阶段。
- **Hermes Agent**：开发响应快（1 天 8 个针对性修复 PR），但合入吞吐（3/50）是瓶颈；社区 P1 重复报告多，急需一次集中发布止血。
- **CoPaw**：2.2.x 稳定迭代 + Hub 多租户方向明确。任务执行状态一致性与局域网连接鲁棒性是当前短板，但整体方向清晰。
- **LobsterAI**：版本节奏最快（2 Release/日），处于功能密集交付期（内嵌浏览器/订阅恢复/登录引导）。开发是主驱动，社区声音偏弱。

**第三梯队：质量巩固期（NanoBot、IronClaw、NanoClaw、PicoClaw）**
- **NanoBot**：维护节奏健康、双主线清晰，但 heartbeat 功能 PR 积压 70+ 天。
- **IronClaw**：Bug 闭环速度快、Backlog 健康。今日关闭的 Issue 多为 1 周内报告的问题，显示"报告→修复→发布"链路运转顺畅。
- **NanoClaw**：开发侧供给充沛（架构级重构 + 安全修复），但合入速度需要跟上，否则主分支与 PR 分支的分叉会持续加大。
- **PicoClaw**：社区贡献活跃但被 stale 机器人批量清理高质量 PR，维护者带宽不足是核心风险，属于"贡献者热情与维护能力不匹配"的状态。

**第四梯队：低活跃/停滞期（Moltis、NullClaw、TinyClaw、ZeptoClaw）**
处于观察期或维护低谷。Moltis 与 NullClaw 仍有零星需求提交但无代码合入，长期来看存在维护者失联风险；TinyClaw/ZeptoClaw 今日完全无活动。

## 7. 值得关注的趋势信号

**1. "静默失效"成为用户信任的头号杀手。** 跨项目最高频的抱怨不是"功能缺失"而是"系统安静地做错事"：消息被丢弃无日志（OpenClaw #112259）、cron 配置不生效无提醒（ZeroClaw #10593）、停止按钮失效但 UI 显示已停止（CoPaw #7567）、OAuth 卡死数小时无告警（OpenClaw #86215）。**对开发者而言，在设计 Agent 系统时，显式失败路径与可观测性（logging/事件追踪）应作为与功能同等级的一等公民来设计**——一次静默的数据丢失对用户信任的伤害，远超十个功能缺失。从技术选型角度，优先采用有界队列、死信机制、结构化日志（trace id 贯穿全链路）和定期健康巡检，可以避免大量此类问题。

**2. 多 Agent / Group Chat 从概念走向真实负载——会话所有权与状态一致性成为核心架构挑战。** CoPaw 的 Hub 多租户 RFC（22 评论）、ZeroClaw 的 Runtime-owned session RFC（#9487，第 5 版修订）、Hermes Agent 的 Bot Group Chat (#97681) 三条线索同时涌现，指向同一个方向：**当多个 Agent 共享运行时、跨渠道协作时，"会话归谁所有、状态如何同步、消息如何路由"成为顶层架构问题**。开发者应关注这些 RFC 的讨论结论——它们实际上在定义下一代 Agent 运行时的会话契约。

**3. 外部工具生态的互操作协议正在成为新的兼容层负担。** 多个项目同时遭遇与 OpenCode、Codex、Cursor 等外部 Agent CLI 的认证/header/schema 兼容问题（NanoBot #5661、ZeroClaw #10603、OpenClaw #86215/#107449、NanoClaw provider 契约系列）。**当 Agent 生态走向互联时，协议标准化（session header、OAuth 流转、工具 schema）成为隐形成本**——建议新项目在起步阶段即对齐主流协议规范（如 Anthropic MCP、OpenCode session 约定），避免后期为每个外部工具打补丁。

**4. 本地/自托管部署的存储可靠性被提升到 P0 级别关注。** Hermes Agent 的 state.db 二次写者损坏（#103339，4 天 7 次损坏）、LobsterAI 的 SQLite CASCADE 失效（#1071，159 天未解决）、CoPaw 请求 PostgreSQL/MySQL 支持（#7558），共同指向 SQLite 在长生命周期/多进程/网络文件系统场景下的固有缺陷。**当 Agent 从"玩具"走向"生产工具"，嵌入式数据库的边界开始显现**，可插拔存储后端可能成为头部项目的标配能力。

**5. "过程可观测性"成为 WebUI/桌面端新的用户刚需。** NanoBot 落地了 tokens/s 与 context 可视化的诉求（#5631 → #5660），Hermes Agent 用户持续抗议无法隐藏推理过程（#49664），CoPaw 用户要求产物独立展示（#7553）。这说明用户不再满足于只看最终回复，而是需要实时感知模型的工作状态——这既是信任构建手段，也是上下文管理的实用工具。

**6. 降级路径的质量正在被审视。** 当非视觉模型收到图片、TTS 读到 Markdown/emoji、MCP 未初始化时，系统应如何优雅降级而非报错或透传占位符（ZeroClaw #10626/#10625、PicoClaw #3337、Hermes Agent #9476 关联）？这反映了用户对 Agent 系统"鲁棒性下限"的预期正在提高——**真正的生产级 Agent 不仅要处理理想路径，还要优雅处理所有"边缘但真实发生"的输入**。开发者在构建工具/渠道适配层时，建议为每一种降级路径设计显式的、用户可理解的输出，而不是简单透传底层错误或占位符。

---

## 同赛道项目详细报告

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

# NanoBot 开源项目日报

**日期：2026-09-05**  
**数据窗口：过去 24 小时**

---

## 1. 今日速览

过去 24 小时项目活跃度较高，但主力集中在 **PR 侧**：共有 28 条 PR 更新，其中 7 条已合并/关闭、21 条仍待处理；Issues 侧相对平稳，5 条更新中 3 条已关闭、2 条新开。本次无新版本 Release。值得关注的是，当前贡献明显呈现 **"WebUI 可观测性 + 内存边界治理"** 双主线：HaisamAbbas 等贡献者持续推进 WebUI 会话标题、token 统计与模型速度展示，Shizoqua 则集中提交了 3 个针对缓存/会话 Registry 无界增长的内存安全修复 PR。社区侧，飞书渠道消息轰炸问题（#5567）仍是最热讨论点，另有 OpenCode 官方渠道 9 月 6 日强制 header 的截止压力（#5661 → #5662）。项目整体维护节奏健康，但仍有 2 个 6 月提交的 heartbeat 特性 PR（#4549、#4551）长期滞留，值得关注。

---

## 2. 版本发布

无新版本发布。

---

## 3. 项目进展

过去 24 小时有 7 条 PR 被合并/关闭，列表中可见的 3 条均质量较高：

- **[#5639](https://github.com/HKUDS/nanobot/pull/5639)（已关闭）：修复 session 标签稳定性、TUI 流式输出与配对提示。**  
  升级 OpenTUI 0.5.3 → 0.5.10，解决了流式 fenced code 在回复完成后不可见的问题；同时将项目级 session 句柄与标题居中，不改变顶层话题布局。属于对 TUI 终端体验的一次收尾打磨。

- **[#5660](https://github.com/HKUDS/nanobot/pull/5660)（已关闭）：在 WebUI context 用量弹层中展示模型生成速度。**  
  直接落地 #5631 的诉求：在已有每轮 context/token 用量基础上，新增 **tokens/s** 展示。后端此前已上报所需数据，因此该改动主要在前端完成，推进了 WebUI 的可观测性。

- **[#5657](https://github.com/HKUDS/nanobot/pull/5657)（已关闭）：重构 WebUI 出站 wire 编码。**  
  将 `recovery_state` 与 `turn_end` 负载编码从 `WebSocketChannel` 抽离，统一为带显式持久化策略的 `send_payload` 原语。虽然没有直接面向用户的功能增量，但为后续更多生命周期事件（如 context_compaction、重试状态）的稳定推送打下了架构基础。

Issues 侧，3 个问题被关闭，包括 WebUI 上下文/模型速度展示增强（#5631）、0.3.0 的 Current Time runtime context 回归（#5645）以及 WebUI locale 并发丢失 bug（#5644）。整体来看，**问题闭合速度略大于新开速度**，且原有问题正在被快速吸收进 PR。

---

## 4. 社区热点

- **[#5567 飞书渠道整合流式卡片消息（OPEN，4 评论，热度最高）](https://github.com/HKUDS/nanobot/issues/5567)**  
  作者 yrxeva 于 8/27 提出，至今仍是最多讨论的 issue。用户核心诉求是：飞书渠道在 agent 处理一条用户消息时会先后输出流式增量、工具调用提示、最终回复等多条独立消息，导致会话混乱。评论区已围绕 `send_delta()` 与 `send()` 两种发送路径展开讨论。该 issue 目前**尚无关联 PR**，是渠道体验中一个待啃的硬骨头。

- **[#5661 OpenCode Zen/Go 强制要求 x-opencode-session header（OPEN，0 评论）](https://github.com/HKUDS/nanobot/issues/5661)**  
  该 issue 虽无评论，但引用官方公告并带有明确时限（2026-09-06 后可能报错），紧迫性强。社区响应迅速：GUTYL 同日提交了 PR #5662，将 `x-opencode-session` 头附加到 OpenAI-compatible provider 的请求中，并关闭了 #5661。

- **[#5631 WebUI 展示上下文与模型速度（CLOSED，2 评论）](https://github.com/HKUDS/nanobot/issues/5631)**  
  该 issue 在关闭前获得了社区积极回应。用户明确表示希望"在回答结束后或输入框附近"看到模型速度与上下文信息，并点名深挖类似 DeepSeek harness 的展示。此类对"过程可观测性"的诉求正在成为 WebUI 的迭代方向。

---

## 5. Bug 与稳定性

过去 24 小时涉及的 Bug/回归问题按影响面排序如下：

| 严重程度 | 问题 | 状态 | 说明 |
|---|---|---|---|
| 高 | **[#5645](https://github.com/HKUDS/nanobot/issues/5645)：0.3.0 默认不再注入 Current Time runtime context** | 已关闭 | 0.2.2→0.3.0 升级后，同一 `ContextBuilder.build_messages()` 调用，未传入 runtime blocks 时不再自动附加时间上下文。对依赖时间感知的 agent 场景影响较大，属行为回归。 |
| 中 | **[#5644](https://github.com/HKUDS/nanobot/issues/5644)：WebUI locale registry 并发加载丢失 locale** | 已关闭 | 启动时两个 locale 并发执行 `loadChannelLocale()`，`translations` 的 Map 在当时立即创建，后写入者会互相覆盖（如 `en`）。影响多语言加载稳定性。 |
| 中 | **内存无界增长修复（3 个待合并 PR）** | 均 OPEN | Shizoqua 连续提交 [#5665](https://github.com/HKUDS/nanobot/pull/5665)（MCP OAuth flow 注册表）、[#5664](https://github.com/HKUDS/nanobot/pull/5664)（idle-session summary 缓存）、[#5663](https://github.com/HKUDS/nanobot/pull/5663)（Mattermost thread context 集合），均是为无容量上限的进程级数据结构添加 bound/eviction。风险不高，但属于长期运行网关的内存隐患。 |
| 低 | **WebUI session 标题生成缺陷（#5647）** | 相关修复 PR 待合并 | [#5648](https://github.com/HKUDS/nanobot/pull/5648) 与 [#5658](https://github.com/HKUDS/nanobot/pull/5658) 均引用 #5647。问题涉及 PR #5528 引入 `target_session_key` 后，session 元数据/WebUI envelope 判断不当导致标题未生成。两个修复方案思路略有差异，需维护者对比取舍。 |

另外，[#5639](https://github.com/HKUDS/nanobot/pull/5639)（已合并）修复了 TUI 中流式输出完成后 fenced code 不可见的问题，属于已修复项。

---

## 6. 功能请求与路线图信号

- **飞书渠道消息流式合一（#5567）**：呼声最高，但暂无 PR 认领。改动会涉及工具阶段消息是改用流式通道还是聚合延迟发送，属渠道层架构调整，**预计需要较长时间**，可能不会在下一个 minor 版本出现。

- **OpenCode session header（#5661 → [#5662](https://github.com/HKUDS/nanobot/pull/5662)）**：外部强制时间线驱动，PR 已提交且标为 `priority: p1`，**大概率进入下一补丁版本**。注意该 PR 目前仅覆盖 OpenAI-compatible provider，Codex 等使用 raw HTTP 的 provider 可能需要单独跟进。

- **Ephemeral runtime-context blocks（[#5659](https://github.com/HKUDS/nanobot/pull/5659)，引用 #5586）**：HaisamAbbas 为 `RuntimeContextBlock` 增加 `ephemeral` opt-out 标记，使运行时上下文可仅附加到当前请求、不持久化回放。这是一个生命周期语义的设计补全，契合 #5645 暴露的 runtime context 行为回归讨论，预计维护者会优先 review。

- **新增 copy_file / move_file 文件工具（[#5626](https://github.com/HKUDS/nanobot/pull/5626)）**：目前 filesystem 工具只有 read/write/edit/list，模型复制文件只能退化为"读→写"链，移动则更麻烦。该 PR 补足基础工具原语，属于低风险高实用性的增强。

- **WebUI 会话上下文/速度可视化（#5631）**：已完成，随 #5660 合入。可以视作**已被纳入路线图并落地**的示例。

- **Heartbeat 共享会话与模型覆盖（#4551、#4549）**：这两个 PR 已滞留两个多月（见下方积压列表），但在路线图层面仍代表一类需求——用户希望 heartbeat 能复用目标 chat session 上下文，并使用独立/更便宜的 model 执行。

---

## 7. 用户反馈摘要

从 Issues 及 PR 的上下文描述中，可以提炼出以下真实用户声音：

- **"用户发一条消息 → agent 回复一条消息"是渠道体验的基本预期。** （#5567）  
  飞书用户对工具调用进度消息与最终回复拆分展示感到困扰，认为多消息轰炸严重干扰对话流。这说明当前「渠道即时消息」模型在 IM 场景下需要向 **聚合/流式卡片** 演进。

- **"在回答结束后或者输入框附近加上这些信息"**（#5631）  
  用户希望像 DeepSeek harness 一样直观看到 token 速度与上下文占用。这不仅是"好看"，而是用户需要感知模型是否在"认真工作"、上下文是否接近上限。该诉求已被 #5660 响应。

- **升级到 0.3.0 后 Current Time 上下文消失**（#5645）  
  用户精确对比了 0.2.2 与 0.3.0 的 `build_messages()` 输出，反映出用户对 runtime context 默认行为的稳定性有明确预期。虽然该 issue 已关闭，但建议维护者在 release notes 中将"runtime context 默认注入策略变化"列为破坏性变更，避免其他用户升级后困惑。

- **并发加载导致 locale 丢失让翻译"随缘"**（#5644）  
  用户在启动时发现 `en` locale 偶发缺失，说明 WebUI 国际化模块存在启动竞态条件。该问题已关闭，但从描述看属于比较典型的异步初始陷阱，对多语言渠道插件的扩展性构成隐患。

---

## 8. 待处理积压

以下 PR/Issue 滞留时间较长或存在合并冲突，建议维护者优先关注：

| 项目 | 创建/最后更新 | 标签 | 风险与建议 |
|---|---|---|---|
| [#4551](https://github.com/HKUDS/nanobot/pull/4551)：heartbeat `isolated_session` 配置 | 2026-06-26（至今积压 70+ 天） | feature | 允许 heartbeat 在目标 chat session 中共享执行。长期无人 review，存在过期风险 |
| [#4549](https://github.com/HKUDS/nanobot/pull/4549)：heartbeat `model_override` 配置 | 2026-06-26（积压 70+ 天） | feature | 与 #4551 同作者的配套需求，互为依赖，建议一并处理 |
| [#5379](https://github.com/HKUDS/nanobot/pull/5379)：preserve full consolidation input | 2026-08-13 | bug/fix | 涉及 memory consolidation 的输入保真，与数据可靠性直接相关，建议优先排查 |
| [#5431](https://github.com/HKUDS/nanobot/pull/5431)：report background task failure | 2026-08-18 | fix | 后台任务失败静默问题；异常日志缺失会导致排障困难 |
| [#5490](https://github.com/HKUDS/nanobot/pull/5490)：clarify aggregate turn token usage | 2026-08-22 | regression/fix，带 conflict 标签 | 已标记冲突，需要 rebase |
| [#5504](https://github.com/HKUDS/nanobot/pull/5504)：surface model retry status（NAN-34） | 2026-08-24 | fix/feature | 将 model retry 状态推送到 WebSocket，有一定 UI 改动量 |
| [#5520](https://github.com/HKUDS/nanobot/pull/5520)：langfuse tracing for codex | 2026-08-24 | feature | Codex provider 的可观测性空白，值得纳入下一步稳定性规划 |

整体来看，PR 队列约有 21 条处于待合并状态，大量来自活跃贡献者的改动已经超过一周未获得 review。建议维护者在下一个 release 前集中处理 **heartbeat 两件套**（#4549/#4551）与 **Shizoqua 的内存 bound 三连**（#5663/#5664/#5665），以降低积压分化风险。

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent 项目动态日报（2026-09-05）

## 1. 今日速览

过去 24 小时 Hermes Agent 仓库保持**高活跃度**：共更新 Issues 50 条（其中 47 条新开/活跃，3 条关闭），PR 50 条（其中 47 条仍在待合并状态，仅 3 条合并/关闭），全天无新版本 Release。Issue 侧最显著的特征是**多组 P1/P2 问题出现重复报告**，尤其是 Desktop SSH 远程模式的 401 会话令牌回归（#103313、#103054、#103366）、推理过程显示开关失效（#49664、#93817），以及本地后端池被“Bot 磁贴”耗尽（#103375、#103401）——这些重复报告说明用户正在真实环境中持续触达同一批未修复缺陷。PR 侧虽无大规模合入，但 9 月 5 日一天内密集提交了至少 8 个针对性强的高质量修复候选（Windows terminal、Windows 更新卡死、Bot 池饥饿、备份排除临时节点、误压缩等），修复供给充沛；不过 50 条 PR 更新中仅 3 条合并/关闭，显示 **review/合入吞吐可能成为下一个瓶颈**。另有 #66616 自动化巡检问题已持续 49 天、累计 157 条评论，属于项目健康度亮红灯的信号，详见下文积压分析。

## 2. 版本发布

过去 24 小时无新版本发布，最新 Releases 为空。Issue 中多次提及 v0.21.0 为最近可识别版本（如 #102486 标注 “post-v0.21.0”），目前 main 分支处于 v0.21.0 之后的迭代期；多个提交指向的修复预计将随下一版本（推测为 v0.22.0）发布。

## 3. 项目进展

过去 24 小时的 3 个合并/关闭 PR 未出现在高热度列表中，无法从现有数据确认具体内容。以下列出的均为今日新提交、仍在 review 流程中的候选补丁，展示当前 main 分支的演进方向：

**稳定性修复候选（9 月 5 日提交）**

- [PR #103402](https://github.com/NousResearch/hermes-agent/pull/103402)：修复 Windows 上 `terminal` 工具 bash 启动探测死锁，并清理 kill 后仍存活的子进程——直接对应 Issue #103398
- [PR #103399](https://github.com/NousResearch/hermes-agent/pull/103399)：阻止后台 Bot 磁贴 reconcile 在 20+ profile 场景下耗尽本地后端池——直接对应 Issue #103375
- [PR #103400](https://github.com/NousResearch/hermes-agent/pull/103400)：Windows 桌面端禁用 QuickEdit 模式，避免更新流程因 Select 模式卡死
- [PR #103397](https://github.com/NousResearch/hermes-agent/pull/103397)：修复 API 请求组装时 `_anchored_pressure` 覆盖导致的误压缩（spurious compaction）
- [PR #103396](https://github.com/NousResearch/hermes-agent/pull/103396)：全量备份时通过 `lstat` 排除 symlink/socket/FIFO 等非普通节点，确保备份不包含临时文件
- [PR #103405](https://github.com/NousResearch/hermes-agent/pull/103405)：修复 `init_session()` 构建 shell 快照时 `declare -f | grep` 逐行过滤导致孤儿函数体被执行的问题

**新功能候选**

- [PR #103404](https://github.com/NousResearch/hermes-agent/pull/103404)：为 `pre_tool_call` 插件钩子新增 `serve` 指令，允许插件**不执行工具而直接提供工具结果**（以 `status="cached"` 返回）——这是对插件系统能力边界的扩展
- [PR #103395](https://github.com/NousResearch/hermes-agent/pull/103395)：新增 `hermes doctor --quick` 快速诊断模式，跳过 npm audit 和 provider API 连通性检测，方便 boot hooks 和 CI

**大型功能 PR 持续推进（未合并）**

- [PR #98307](https://github.com/NousResearch/hermes-agent/pull/98307) 与 [PR #98073](https://github.com/NousResearch/hermes-agent/pull/98073)：由 @dokterdok 提交的 Group Chat 完整生产化实现（对应 Issue #97681），覆盖 Desktop 关闭后的 Bot 间消息/文件交换、手机端远程查看/控制/审批等场景。两个 PR 今日均有 push/更新，仍在 review 中

整体来看，main 分支正在积累一批“面向真实环境”的修复——Windows 平台、多 profile 主机、SSH 远程模式是当前主要战场。虽然合入速度一般，但开发侧响应已较为充分。

## 4. 社区热点

**#66616 | 自动化巡检异常引发 157 条评论（并非用户热度的健康信号）**

- 链接：[Issue #66616](https://github.com/NousResearch/hermes-agent/issues/66616)
- 标签：type/bug · tool/skills · P3 · sweeper:risk-automation
- 分析：这是一个由 @nousbot-eng 自动创建的 freshness probe 失败通知：skills-index 已过期 29.8 小时（上限 26 小时）。截至今日已存活 **49 天**，累计 157 条评论，是所有 Issue 中评论数最高的。问题根源指向 `.github/workflows/skills-index.yml` 与 `deploy-site.yml` 未能及时重建 `/docs/api/skills-index.json`，导致 /docs/skills 依赖的索引持续 stale/degraded。该 Issue 的评论数极高并非来自真实用户讨论，而是**自动化流程反复追加失败状态**——这本身比功能 Bug 更值得警惕：它说明持续集成/发布管道已长时间异常，且未获得有效介入。

**#97681 | Bot Group Chat 的连续性诉求（23 条评论）**

- 链接：[Issue #97681](https://github.com/NousResearch/hermes-agent/issues/97681)
- 分析：用户要求 Hermes 将 Bot（来自笔记本、homelab、VPS）放入同一 Group Chat 后，即使 Desktop 客户端关闭，Bot 之间仍能继续协作；用户随后可在手机端查看进度、取回文件、执行 Stop/Retry/审批。今日评论数依然活跃，且配套 PR #98307/#98073 正在推进。这反映出**从“本地桌面助手”走向“持续在线的多 Agent 协作网络”**是用户对 Hermes 的明确期待。

**#18715 | 远程 Agent + 本地工具执行（18 条评论，29 👍，高赞功能请求）**

- 链接：[Issue #18715](https://github.com/NousResearch/hermes-agent/issues/18715)
- 分析：用户希望 Machine A（本地）通过 Machine B（远程 Hermes）调用模型/记忆/技能，但工具执行留在本地。该请求已开放 4 个月，获得 29 个 👍，是近期功能请求中社区认可度最高的一条。其背后诉求是**数据/环境主权与算力分离**——远程跑模型，本地跑工具。该能力与 Group Chat 跨网关 transport 有一定架构交集，值得路线图层面统筹考虑。

**桌面端“无法隐藏推理过程”引发多用户高分贝反馈（#49664、#93817、#85110）**

- 链接：[Issue #49664](https://github.com/NousResearch/hermes-agent/issues/49664)（P1）、[Issue #93817](https://github.com/NousResearch/hermes-agent/issues/93817)（P1，duplicate）、[Issue #85110](https://github.com/NousResearch/hermes-agent/issues/85110)（P2）
- 分析：多名用户（@Baibaibin、@networthexplained 等）报告同一个问题：`display.show_reasoning: false` 写入 config 后，桌面端仍会完整展示模型思考 token、推理块和每次工具调用。用户将其描述为 “P0 / makes Hermes Desktop unusable”“productivity-blocking”。目前 #49664 已定位到根因——**渲染代码从未读取该配置**，但尚未看到对应修复 PR。

## 5. Bug 与稳定性

以下按严重程度排列今日出现的 Bug/回归，并标注是否有修复 PR。

### P1 — 高优问题

**SSH 远程模式 401 会话令牌回归（3 份重复报告，根因未修复）**

- [Issue #103054](https://github.com/NousResearch/hermes-agent/issues/103054)（OPEN，P1，duplicate）
- [Issue #103366](https://github.com/NousResearch/hermes-agent/issues/103366)（OPEN，P1，duplicate，9 月 5 日新提交）
- [Issue #103313](https://github.com/NousResearch/hermes-agent/issues/103313)（CLOSED，P1，duplicate）
- 现象：Desktop 通过 SSH 连接远程 Hermes 后，`/api/health` 和 `/api/status` 正常，但所有敏感 API（`/api/profiles` 等）返回 401。根因指向 `mount_spa` 向页面注入的 `window.__HERMES_SESSION_TOKEN__` 为**陈旧会话令牌**（回归自 `5f1feb5344`）。9 月 5 日仍有新报告提交，说明根因尚未修复，急需维护者统一处理。

**`state.db` 被第二写者破坏（多 profile 主机，4 天 7 次损坏）**

- 链接：[Issue #103339](https://github.com/NousResearch/hermes-agent/issues/103339)
- 现象：在 default + 2 profile gateways 的 multi-profile 主机上，`doctor --fix` / `repair_state_db_schema` / hosted_rooms 会绕过 WAL 单写者保护，造成 live state.db 损坏（SQLite 3.53.1 / Linux ext4），4 天内 7 次损坏。提交者已提出 lazy flock 单写者门的设计建议。**无对应 fix PR**。

**systemd 249 拒绝 `OOMPolicy=kill`，cron worker dispatch 全部失败**

- 链接：[Issue #102486](https://github.com/NousResearch/hermes-agent/issues/102486)
- 现象：升级到 main（post-v0.21.0）后，systemd 249 环境下每个 gateway cron worker dispatch 都因 `OOMPolicy=kill` 被识别为未知配置而 fail-closed。**无对应 fix PR**，属于版本兼容性回归。

**`hermes update` 因陈旧 receipt 无限触发 fleet restart**

- 链接：[Issue #98022](https://github.com/NousResearch/hermes-agent/issues/98022)（P1）
- 现象：当 `update_receipts/latest.json` 是中断的陈旧 receipt 时，`hermes update` 每次运行都会重新触发 fleet restart，即使当前版本已最新。这是对 #95294 修复的边界条件补漏。**无对应 fix PR**。

**桌面端本地 Bot 池被无限重连耗尽（#103375，已有修复 PR）**

- 链接：[Issue #103375](https://github.com/NousResearch/hermes-agent/issues/103375)（P2）
- 现象：20+ profiles 环境下，Bot 磁贴（包括已隐藏的）不断触发 `reconcileTileTranscripts → ensureBackend → spawnPoolBackend`，10 秒 touch 循环导致本地后端池 slot 永不释放。**已有修复 PR #103399 待 review**。

**推理块显示开关失效（#49664 / #93817，无修复 PR）**

- 链接：[Issue #49664](https://github.com/NousResearch/hermes-agent/issues/49664)（P1）、[Issue #93817](https://github.com/NousResearch/hermes-agent/issues/93817)（P1）
- 现象：设置 `display.show_reasoning: false` 后桌面端仍渲染完整思考过程与工具调用。渲染层从未读取配置。已有根因分析，但**无 fix PR**。多用户将其视为桌面端最高优先级问题。

### P2 — 中优先问题

**Windows terminal 工具长时间挂起（已有修复 PR）**

- 链接：[Issue #103398](https://github.com/NousResearch/hermes-agent/issues/103398)、修复 [PR #103402](https://github.com/NousResearch/hermes-agent/pull/103402)
- 现象：ACP 模式下 `terminal` 工具在 Windows 上连 `pwd` 都会挂到 executor 超时。Git for Windows 的 bash 包装器启动探测死锁，且探测子进程在 `subprocess.run` kill 后依然存活。修复 PR 已提交。

**Loopback bind 禁用 WS keepalive，导致 PTY 子进程泄漏**

- 链接：[Issue #96418](https://github.com/NousResearch/hermes-agent/issues/96418)
- 现象：loopback bind 情况下 WS keepalive ping 被禁用，反向代理后的死客户端无法被探知，每个死连接泄漏一个 PTY 子进程。属于源码级缺陷报告。

**UI 内安装 pip 包无效**

- 链接：[Issue #100610](https://github.com/NousResearch/hermes-agent/issues/100610)
- 现象：Podman/quadlet 容器中从 UI 安装 pip 包（如 ddgs）失败，`_pip_install` 与容器环境不兼容。

### P3 — 低优/体验问题

- [Issue #103303](https://github.com/NousResearch/hermes-agent/issues/103303)：Kanban `decompose_triage_task` 让 scratch 子任务继承 root 的 workspace_path，并发 worker 共享同一目录
- [Issue #102619](https://github.com/NousResearch/hermes-agent/issues/102619)：Qwen3.8 27B 在 128GB M5 Max 上被误标 “Too big for this machine”，模型大小估算未考虑统一内存架构
- [Issue #101311](https://github.com/NousResearch/hermes-agent/issues/101311)：macOS 桌面端输出文本块仍出现水平滚动条（用户要求仅垂直滚动、长行自动换行）
- [Issue #103364](https://github.com/NousResearch/hermes-agent/issues/103364)：plugin-guard 对 `obra/superpowers` 的 Markdown 解释性文本误报 `dangerous` 结论
- [Issue #24740](https://github.com/NousResearch/hermes-agent/issues/24740)：Honcho 自动重命名会话标题间歇性覆盖 `sessionStrategy` 设置
- [Issue #103287](https://github.com/NousResearch/hermes-agent/issues/103287)：`/steer` 在无活跃 run 时确认 “queued” 但文本永远滞留于 `_pending_steer`

## 6. 功能请求与路线图信号

**很可能进入下一版本的能力（已有实现/PR 支撑）**

- **插件供应工具结果能力**：PR #103404 为 `pre_tool_call` 新增 `serve` 指令，让插件无需执行工具即可返回结果。这为插件生态的 mock、缓存、权限拦截提供了新的核心原语，属于低成本、高杠杆的插件系统扩展
- **`hermes doctor --quick`**：PR #103395 为 boot hooks/CI 提供跳过网络检查的快速诊断路径，属于开发者体验类改进，合入概率较高
- **Group Chat 手机端控制与端到端文件交换**：PR #98307 + #98073 构成完整的 Group Chat 生产化实现。虽然体量大、仍在 review，但与其关联的 Issue #97681 是当前功能方向的主线之一

**具有路线图价值但尚无 PR 的需求**

- [Issue #18715](https://github.com/NousResearch/hermes-agent/issues/18715)（远程 Agent + 本地工具执行）——29 👍，开放 4 个月，社区需求信号最强
- [Issue #100944](https://github.com/NousResearch/hermes-agent/issues/100944)（Kanban worker 按 profile 区分创建/链接权限，保留生命周期工具）——细粒度的多租户/权限治理需求
- [Issue #100428](https://github.com/NousResearch/hermes-agent/issues/100428)（`browser_exec` 按本地 session 选择 headed/headless 模式）——当前工具 schema 缺少可见性控制参数
- [Issue #103368](https://github.com/NousResearch/hermes-agent/issues/103368)（Antigravity/Gemini ACP 支持）——用户指出 Antigravity 订阅已进入官方文档和 ACP Registry，但权限/身份流程尚未完整落地
- [Issue #45562](https://github.com/NousResearch/hermes-agent/issues/45562)（桌面端保留每会话阅读位置）——面向长对话的导航体验改进

## 7. 用户反馈摘要

- **“桌面端不可用”是最强烈的反感来源**：多名用户（@networthexplained、@Baibaibin）在 #49664、#93817 和 #85110 中把“无法隐藏推理过程与工具调用”称为 P0，直指“answer-only transcript”这一基础需求被打破，UI 被完整 agent trace 刷屏。
- **SSH 远程模式的挫败感正在累积**：@LordMelkor、@aimen08、@SZWzz 在 9 月 4 日至 9 月 5 日连续提交 401 bug 报告，现象高度一致：SSH 已成功、`HERMES_BACKEND_READY` 已打印，但桌面端 profiles rail 为空并反复 `refreshProfiles failed after 3 attempt(s)`。用户能区分“连接失败”和“令牌不匹配”，说明故障定位门槛已被用户自己跨过。
- **数据损坏最刺痛专业用户**：@RChina 在 #103339 中给出了详尽的现场分析（4 天 7 次 state.db 损坏、SQLite 3.53.1、ext4），并提出了 lazy flock 修复建议。这是典型的高级自托管用户——他们信任 Hermes 到将其用于多 profile 生产环境，因此对 WAL 写者竞争这类基础数据安全问题容忍度极低。
- **Windows 用户持续边缘化**：#103398（terminal 挂起）、#103400（QuickEdit 导致更新卡死）、#100610（容器内 UI pip 安装失败）均来自 Windows/容器路径，反映桌面端跨平台测试覆盖不足。
- **Mac 本地模型用户感到被误判**：@LAMBODOG 在 #102619 中质疑“Qwen3.8 27B 在 128GB M5 Max 上被标记 Too big”，说明硬件适配逻辑需要更智能地识别 unified memory 架构。
- **社区对“配置残留”感到烦躁**：#52382 中 @kungunier 对每次启动都出现 `Warning: Unknown toolsets: messaging` 表示不满——PR #47856 删除了 messaging 工具集，但没有做配置迁移，用户在每次启动时被动承受噪音。

## 8. 待处理积压

以下条目长期未获有效解决或正在形成积压，提请维护者关注：

- **[Issue #66616](https://github.com/NousResearch/hermes-agent/issues/66616)（自动化巡检，P3，存活 49 天，157 条评论）**：skills-index pipeline 已持续 degraded 近 50 天。这不是功能 bug，而是 CI/CD 管道本身不健康，会逐步侵蚀文档站和技能发现的可信度，建议优先排查 workflows 失败原因
- **[Issue #18715](https://github.com/NousResearch/hermes-agent/issues/18715)（远程 Agent + 本地工具执行，P2，存活约 126 天，29 👍）**：高赞功能请求长期无 roadmap 回应，建议至少给出架构可行性反馈
- **[Issue #52382](https://github.com/NousResearch/hermes-agent/issues/52382)（messaging 工具集配置残留警告，P2，存活约 72 天）**：低成本、高感知度的修复——只需在配置加载时自动 prune 未知工具集名称
- **SSH 401 令牌问题三连报（#103054、#103313、#103366）**：9 月 5 日仍有人新报，根因已指向 `mount_spa` 注入陈旧 token。建议由一名维护者统一收口，避免继续以 duplicate 标签关闭而实际不修复
- **[Issue #102486](https://github.com/NousResearch/hermes-agent/issues/102486)（systemd 249 cron dispatch 全面失败，P1）**：影响面大（所有 systemd 249 用户的 cron worker），9 月 3 日提交至今无 fix PR
- **[PR #22982](https://github.com/NousResearch/hermes-agent/pull/22982)（修复 /model 命令内联负载路由，开放约 118 天）**：长期滞留的 P2 gateway 行为修复 PR，至今未合并
- **[PR #44551](https://github.com/NousResearch/hermes-agent/pull/44551)（TUI slash worker 协议测试，开放约 85 天）**：测试类 PR 长期无人 review，会削弱贡献者持续投入的意愿
- **[Issue #103339](https://github.com/NousResearch/hermes-agent/issues/103339)（state.db 二次写者损坏，P1）**：提交于 9 月 5 日但问题表述非常完整（含现场、机制、修复建议），数据安全类问题建议尽快指派维护者验证 lazy flock 方案

---

**健康度小结**：今日社区生产力和问题报告量均处于高位，但 Bug 关闭速度（3/50）远低于新报告速度（47/50），P1 问题中目前只有 #103375 和 #103398 出现了对应修复 PR，其余仍悬空。项目正处于“开发响应快、合入吞吐慢”的阶段——若 review 瓶颈不能缓解，同一批 P1 问题很可能在下一次日报中继续以 duplicate 形式出现。

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

# PicoClaw 项目动态日报 · 2026-09-05

## 1. 今日速览

过去 24 小时项目整体活跃度中等：Issues 侧有 3 条更新，其中 1 条为新提交的 Feature 请求（[#3366](https://github.com/sipeed/picoclaw/issues/3366)）；PR 侧有 22 条更新，包含 2 条新开的文档类 PR 和 20 条已关闭/合并记录，但大量关闭条目来自 stale 机器人对历史积压 PR 的批量清理，而非真正的新功能合入。唯一可明确观察到的代码主干推进是一条整理型 PR [#1541](https://github.com/sipeed/picoclaw/pull/1541) 的关闭，将媒体临时目录、频道 DoS 加固与 DeepWiki 徽标三个子 PR 带入主线。今日没有新版本 Release。

## 2. 版本发布

无。

## 3. 项目进展

- **[#1541](https://github.com/sipeed/picoclaw/pull/1541) 合并/关闭，是今日最明确的代码主干进展**。该 PR 属于集成型修复分支，集中合入了三个此前悬置的子 PR：
  - [#1536](https://github.com/sipeed/picoclaw/pull/1536)：将 PicoClaw 媒体临时目录集中到 `pkg/media/tempdir.go`，并扩展了 agent 的 `read_file`/`list_dir` 等工具对媒体临时目录的读取权限；
  - [#1535](https://github.com/sipeed/picoclaw/pull/1535)：频道层面的 DoS 加固；
  - [#1531](https://github.com/sipeed/picoclaw/pull/1531)：项目文档添加 DeepWiki 徽标。

- **两条新的待合并 PR 均为 MCP 生态文档补充**，说明项目正在强化 MCP 接入的开发者体验：
  - [#3368](https://github.com/sipeed/picoclaw/pull/3368)：新增 Parallel Search MCP 配置示例，用户可在无 Parallel 账号/API Key 情况下为 PicoClaw 增加网页搜索与页面提取能力；
  - [#3367](https://github.com/sipeed/picoclaw/pull/3367)：新增 Pilot Protocol MCP 接入说明，包含健康检查命令，且明确不破坏已有配置、无需 API Key。

- 值得关注的是，在列出的 20 条已关闭/合并 PR 中，**超过一半的标题带 `[stale]` 标记**，例如 [#3337](https://github.com/sipeed/picoclaw/pull/3337)、[#1683](https://github.com/sipeed/picoclaw/pull/1683)、[#1854](https://github.com/sipeed/picoclaw/pull/1854) 等。它们大多是 2026 年 3 至 5 月提交的增强或修复，于昨日被批量关闭。这意味着 24 小时内的“PR 关闭”并不等于“功能合入”，核心代码的实际增量有限。

## 4. 社区热点

- **[Issue #3287：IRC 长消息支持](https://github.com/sipeed/picoclaw/issues/3287)** —— 10 条评论，是当前评论数最高的事务。IRC 默认限制单条消息 512 字节，客户端会自动拆分超长消息，但 PicoClaw 会将拆分后的片段当作多条独立消息处理。社区用户希望 PicoClaw 能识别并重组 IRCv3 长消息。该 Issue 目前已被标记为 `[stale]`，但仍处于开放状态。

- **[Issue #3281：Web UI 输入框在长历史会话中严重卡顿](https://github.com/sipeed/picoclaw/issues/3281)** —— 9 条评论、2 个 👍，是当前最受关注的 Bug。用户报告在会话历史较长时，Web UI 的输入框响应变得非常迟钝，影响日常对话使用。该问题自 2026-07-21 提交至今已超过 6 周，仍无明确修复 PR 关联。

- **[Issue #3366：支持自定义 OpenAI Compatible Providers](https://github.com/sipeed/picoclaw/issues/3366)** —— 新提交的功能请求，暂无评论，但代表了自托管网关用户对 provider 扩展性的持续需求。用户希望在现有 OpenAI provider 之外，添加“OpenAI Compatible”自定义选项，以便接入 9Router 之类的自托管路由。

## 5. Bug 与稳定性

按严重程度排序：

| 严重程度 | 问题 | 说明 |
| --- | --- | --- |
| **高** | [Web UI 输入框长会话卡顿 #3281](https://github.com/sipeed/picoclaw/issues/3281) | 直接影响高频 Web 用户的核心交互体验。复现路径明确：在单一 session 中积累较多历史后，输入框即出现明显延迟。暂无关联 fix PR。 |
| **中** | [MCP server 连接失败会导致 Agent 循环挂起 #3337](https://github.com/sipeed/picoclaw/pull/3337) | 该 PR 描述了 PicoClaw 在 `ensureMCPInitialized` 返回错误后，`AgentLoop.Run` 直接退出、聊天界面停止响应的严重问题，并已给出修复。但 PR 在昨日被 stale 机制关闭，修复尚未进入主干。 |
| **中** | [默认开放机器人缺少安全审计 #2088](https://github.com/sipeed/picoclaw/pull/2088) | 当 `allow_from` 为空时机器人默认处于“允许任何人消息”的 permissive 状态，存在用户无意暴露私有 agent 的风险。该安全加固 PR 已于 09-04 被关闭。 |
| **中** | [Telegram 流式消息存在重复/错投问题 #2092](https://github.com/sipeed/picoclaw/pull/2092) [#2090](https://github.com/sipeed/picoclaw/pull/2090) | 涉及流式编辑超时后发送冗余第二条消息，以及 Forum/Topics 场景下回复目标错误。两条修复 PR 均已被关闭，未确认合入。 |
| **低至中** | [Slack @mention 双重处理与 session 碎片化 #2089](https://github.com/sipeed/picoclaw/pull/2089)、[飞书群 @机器人漏检 #2091](https://github.com/sipeed/picoclaw/pull/2091) | 均为频道集成中“事件重复/检测失败”类稳定性问题，PR 已关闭，无合入记录。 |
| **信息性** | [Telegram 群组负数 ID 被误判为非数字 #1855](https://github.com/sipeed/picoclaw/pull/1855) | 会导致群组 ID 被错误解析为平台名，影响路由与权限判断。 |

此外，一批 3 至 4 月提交的 OpenAI-compatible provider 相关修复与增强也已随 stale 清理而关闭，包括 [tool_call_id 去重 #1854](https://github.com/sipeed/picoclaw/pull/1854)、[上下文溢出检测改进 #2016](https://github.com/sipeed/picoclaw/pull/2016)、[SystemParts token 估算修正 #2014](https://github.com/sipeed/picoclaw/pull/2014) 等。**如果这些修复未被合入，相关 bug 很可能仍存在于当前主干中**，建议维护者按优先级重新评估并恢复其中重要的 PR。

## 6. 功能请求与路线图信号

- **自定义 OpenAI-compatible Provider（新请求：[#3366](https://github.com/sipeed/picoclaw/issues/3366)）**：用户希望新增“OpenAI Compatible”自定义 provider，以接入自托管路由器。结合此前大量围绕 `openai_compat` 的 PR（[#1683](https://github.com/sipeed/picoclaw/pull/1683)、[#1858](https://github.com/sipeed/picoclaw/pull/1858)、[#1860](https://github.com/sipeed/picoclaw/pull/1860)、[#2260](https://github.com/sipeed/picoclaw/pull/2260)、[#2522](https://github.com/sipeed/picoclaw/pull/2522)），可以判断 **“OpenAI 兼容层”是社区贡献密度最高的方向之一**。未来版本若将兼容层从“固定第三方 provider 列表”扩展为“用户自定义 baseURL/model 配置”，将能覆盖大量自托管用户需求。

- **IRC 长消息整体识别（[#3287](https://github.com/sipeed/picoclaw/issues/3287)）**：本质上属于 IRC 通道对长文本/多行消息的支持缺口。考虑到该 issue 已被 stale 标记，若近期无维护者介入，IRC 方向的长消息支持可能会滞后。

- **MCP 搜索与应用生态（[#3368](https://github.com/sipeed/picoclaw/pull/3368)、[#3367](https://github.com/sipeed/picoclaw/pull/3367)）**：两天内连续出现两个 MCP 接入配置 PR，分别指向网页搜索/Pilot Protocol，且均强调“无需 API Key”。这说明来自社区的外部工具方正在主动为 PicoClaw 补齐联网检索与第三方协议接入能力，未来版本 MCP 相关示例和默认 server 配置可能会持续增加。

## 7. 用户反馈摘要

- **Web UI 用户在长会话场景下体验受损**：[#3281](https://github.com/sipeed/picoclaw/issues/3281) 的用户运行 PicoClaw 0.3.1，在单一 session 中历史消息较多后，输入框开始明显卡顿。这是当前用户体验痛点最集中、复现步骤最清晰的 Issue 之一，且长时间没有对应修复，容易造成 Web 端用户流失。

- **IRC 用户需要 PicoClaw 理解“被拆分的合法长消息”**：[#3287](https://github.com/sipeed/picoclaw/issues/3287) 的提出者希望 PicoClaw 能把 IRCv3 中因 512 字节限制而被客户端切分的消息视为一条完整消息，而不是多条独立消息。对依赖 IRC 作为日常办公/社区入口的用户来说，这直接决定了机器人能否正确接收长命令或长文本。

- **自托管用户希望接入自有路由层**：[#3366](https://github.com/sipeed/picoclaw/issues/3366) 提出的“自定义 OpenAI Compatible provider”不是新概念，而是将现有 OpenAI 适配逻辑暴露为可配置入口。这类用户通常已经自建了模型网关，不愿意为每个后端模型分别维护一套 PicoClaw provider 配置。

## 8. 待处理积压

- **[Issue #3281：Web UI 输入卡顿](https://github.com/sipeed/picoclaw/issues/3281)**：创建于 2026-07-21，已有 9 条评论和 2 个 👍，目前被标记为 `[stale]`。这是一个用户明确复现、影响日常 Web UI 使用的 Bug，却迟迟没有对应 fix PR，建议维护者优先处理。

- **[Issue #3287：IRC 长消息支持](https://github.com/sipeed/picoclaw/issues/3287)**：创建于 2026-07-22，共 10 条评论，同样已被 `[stale]` 标记但仍开放。IRC 频道作为部分社区的主要入口，目前长消息处理能力缺失，长期搁置会限制该通道的实际可用性。

- **[PR #3337：修复 MCP 失败导致 Agent 挂起](https://github.com/sipeed/picoclaw/pull/3337)**：该 PR 提交于 2026-08-14，修复的是一个会导致聊天界面完全停止响应的严重问题，但在不足一个月后即被 stale 机制关闭。建议维护者检查该修复是否仍适用，并考虑在近期恢复合并。

- **大批量 stale 关闭的核心修复 PR**：贡献者 badgerbees 在 3 至 4 月集中提交了大量跨 provider/channel/agent 的修复与增强 PR，包括 [OpenAI strict mode 兼容 #1683](https://github.com/sipeed/picoclaw/pull/1683)、[上下文溢出检测 #2016](https://github.com/sipeed/picoclaw/pull/2016)、[Telegram 流式重复消息 #2092](https://github.com/sipeed/picoclaw/pull/2092)、[GitHub Copilot stdio 支持 #2240](https://github.com/sipeed/picoclaw/pull/2240) 等。这些 PR 已在 09-04 被批量关闭，但其中很多并非“已合入”状态。**如果这些修复仍然有效，建议维护者主动与贡献者沟通恢复 PR 或重新实现**，避免出现“修复已存在但被机器人清理、Bug 仍留在主干”的局面。

整体来看，PicoClaw 社区侧的贡献意愿仍然活跃，但维护者 review/merge 带宽不足已成为当前项目健康度的主要风险：大量高质量的修复 PR 被 stale 机制清理，而核心 Web UI 体验问题已超过 6 周没有推进。建议下一阶段优先恢复高价值 PR、回应 [#3281](https://github.com/sipeed/picoclaw/issues/3281) 的 Web UI 性能问题，并为 OpenAI-compatible 自定义 provider 给出明确的路线图回应。

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw 项目动态日报 · 2026-09-05

> 数据窗口：过去 24 小时（2026-09-04 → 2026-09-05）
> 链接基于数据快照中的 Issue/PR 编号，指向 `https://github.com/qwibitai/nanoclaw`

## 1. 今日速览

- 过去 24 小时 **Issues 更新 2 条**：全部新开，关闭 0 条；**PR 更新 18 条**：15 条仍待合并，3 条关闭；**无新版本发布**。
- **开发侧活跃度：高**。18 条 PR 更新与 7 条新 PR 集中在同一天涌现，说明当前正处于密集的功能/重构提交期。
- 提交热点集中在 **provider 契约统一、技能安装安全硬化、A2A 通信可靠性** 三个方向，属于结构性架构升级，而非零散小补丁。
- **Issue 侧净化速度偏慢**：2 条新 Issue 均为生产/配置暴露出的问题，其中一条直接导致生产 OOM 崩溃，值得优先回应。
- 当前状态可概括为：**写入速度远大于合并速度，主分支之外积压了大量待评审 PR**。

## 2. 版本发布

今日无新 Release、预发布或补丁版本。

## 3. 项目进展

过去 24 小时有 3 条 PR 被关闭，其中 2 条为长达数月的存量 backlog，1 条为发布流程改造：

- [ci: replace bump-version with explicit Release workflow + concurrency guard (#2403)](https://github.com/qwibitai/nanoclaw/pull/2403) —— 由 glifocat 提交，5 月创建、今日关闭。意味着仓库发布流程从 `bump-version` 脚本迁移到显式 Release workflow，并加入并发保护；这为后续自动化发版和防止重复发布/标签冲突铺平了道路。
- [fix(chat-sdk-bridge): fall back to URL fetch for adapters without fetchData (#2232)](https://github.com/qwibitai/nanoclaw/pull/2232) —— 清理了 5 月以来 Chat SDK 桥接层的一个适配器兼容性问题。
- [feat(chat-sdk-bridge): add sendAsRaw flag to bypass adapter Markdown round-trip (#2231)](https://github.com/qwibitai/nanoclaw/pull/2231) —— 为 Chat SDK 桥接增加 `sendAsRaw` 能力，避免消息经过适配器 Markdown 往返导致内容被改写。

此外，15 条 PR 仍处于 OPEN 状态。尤其值得关注的是 zvi-fried 提交的 **provider 契约重构系列**（#3584、#3586、#3588、#3591、#3592 等），它们已形成一条清晰的架构主线：让 OpenCode、Codex、Cursor 等 provider 遵循统一的核心契约。该系列仍未合入主分支，是当前最大的待落地区块。

## 4. 社区热点

从可见评论数据看，今日讨论最集中的是：

- [PreCompact conversation-archive writes an unbounded, full-rewrite file per firing — real cause of a production OOM crash loop (#3716)](https://github.com/qwibitai/nanoclaw/issues/3716)

这是过去 24 小时内唯一带评论（2 条）的 Issue。反馈者 DawoudIO 定位到一个非常具体的生产事故：`PreCompact` hook 每次触发都会将完整会话历史重新序列化，并写入一个**无轮换、无上限、无清理**的归档文件，最终导致 OOM 崩溃循环。2 条评论说明该问题已引发初步讨论，且反馈者给出了明确根因，社区诉求是希望归档机制从“无界写文件”改为有界、可轮换或可清理的行为。

另一个值得注意的 Issue 是：

- [Operator env overrides never reach the session container (#3714)](https://github.com/qwibitai/nanoclaw/issues/3714)

虽然目前 0 评论、0 👍，但它是 #1820 的 follow-up，指向一个长期未闭合的配置链路缺陷，属于典型的“小众人数、大影响”问题。

由于快照未提供 PR 评论数统计，PR 侧的“热度”更多体现在提交/更新频率上：zvi-fried 的 provider 契约系列形成了明显的 PR 讨论/审查热点区，9 月 4 日一天即新增/更新多条相关 PR（#3720、#3721、#3722 等）。

## 5. Bug 与稳定性

按严重程度排列：

### 高严重度

- [PreCompact conversation-archive 全量重写导致生产 OOM 崩溃循环 (#3716)](https://github.com/qwibitai/nanoclaw/issues/3716)
  - 现象：每次 `PreCompact` 触发都会在 `/workspace/agent/conversations/` 写入一个全新文件，内容是完整对话历史的重新序列化，且完全不清理旧文件。
  - 影响：在生产环境中已造成实际 OOM 崩溃循环。
  - 修复状态：**暂无对应 Fix PR**，需维护者尽快确认预期行为并给出归档策略。

### 中严重度

- [Operator env overrides 无法进入会话容器 (#3714)](https://github.com/qwibitai/nanoclaw/issues/3714)
  - 现象：auto-compact window、transcript rotation 等多个“操作员覆盖变量”虽有文档和源码注释，但从未从宿主机转发到会话容器。
  - 影响：部署者无法不修改代码/镜像就调整这些行为；属于#1820 问题的延续，说明该问题仍未闭合。
  - 修复状态：**无对应 Fix PR**。

### 已提交修复、但尚未合入的安全/稳定性风险

以下 PR 已在过去一段时间提交，但在它们合入之前，相关风险在主分支上仍然存在：

- [fix(mount-security): close allowlisted-extra mount bypass in validateSpec (#3680)](https://github.com/qwibitai/nanoclaw/pull/3680) —— 容器挂载验证绕过修复，8/30 创建，已等待 6 天。
- [fix(agent-runner): escape payloads embedded in composed prompt blocks (#3717)](https://github.com/qwibitai/nanoclaw/pull/3717) —— 防止嵌入 payload 关闭外层 prompt block 并伪造结构，属提示注入类加固。
- [fix(skills): require explicit installation and respect operator policy (#3721)](https://github.com/qwibitai/nanoclaw/pull/3721) —— 收紧能力安装路径，禁止绕过部署拒绝策略。
- [feat(skills): add opt-in source installation with guarded recovery (#3720)](https://github.com/qwibitai/nanoclaw/pull/3720) —— 源安装默认关闭，且不允许通过 agent approval 开启，属于带安全边界的技能安装能力。

## 6. 功能请求与路线图信号

今日没有出现全新的大型功能请愿；新 Issue 偏向稳定性与配置缺陷。但结合待合并 PR，可以识别出以下路线图信号：

- **会话归档/压缩文件的可管理性**：#3716 虽然以 Bug 形式报告，但其本质需求是让归档写入具备轮换、大小上限或清理策略。这是生产运维的强信号，可能催生后续配置项。
- **Operator 可覆盖配置链路**：#3714 指向容器环境变量透传缺失。下一个版本如要修复，需要把宿主机 operator 配置与 session container 启动参数打通。
- **Provider 契约统一**：#3584、#3586、#3588、#3591、#3722 等 PR 正在将 Codex、OpenCode、Cursor 的 provider 行为收编到 core-owned 契约中，避免各 provider 重复解释核心语义。
- **技能安装的显式化与安全化**：#3720、#3721、#3355、#3715 四者叠加，暗示未来技能/工具安装会从“脚本拷贝”演进为“CLI 管理 + 显式审批 + 策略约束”的正式机制。
- **A2A 通信可靠性**：#3718、#3719 分别解决 agent 身份验证与失败反馈问题，属于多智能体通信链路的稳定性补齐。

## 7. 用户反馈摘要

基于两条新 Issue 的可见标题与摘要，用户反馈呈现明显的“生产部署视角”：

- [Issue #3716](https://github.com/qwibitai/nanoclaw/issues/3716) 的反馈者具备较强的根因分析能力，直接指出 OOM 的“真实原因”是 `PreCompact` 的无界全量归档行为，而不是笼统地报告“内存溢出”。其潜台词是：归档逻辑缺少容量边界设计，当前实现不适合长时间运行的生产环境。
- [Issue #3714](https://github.com/qwibitai/nanoclaw/issues/3714) 的反馈者明确提到“文档中标注为 operator overrides，但除非打补丁否则无法设置”。这是一种典型的 **文档承诺与实际行为不一致** 带来的挫败感；且由于引用 #1820，说明用户已经等待该问题修复相当久，最终选择再次开 Issue 升级可见性。

整体来看，用户并没有在今日提出“我想要某个新功能”，而是在集中表达对 **可运维性、可配置性、可预测性** 的诉求。

## 8. 待处理积压

需要维护者关注的存量项：

- [Issue #3714](https://github.com/qwibitai/nanoclaw/issues/3714) —— 新开但 0 评论、0 👍、无 label、无 assignee。作为 #1820 的 follow-up，若无人回应极可能再次沉底。
- [feat(providers): add Cursor Agent SDK payload (#3356)](https://github.com/qwibitai/nanoclaw/pull/3356) 与 [feat(skills): add /add-cursor provider install skill (#3355)](https://github.com/qwibitai/nanoclaw/pull/3355) —— 8/19 创建，已开放 17 天，Cursor provider 相关功能迟迟未合入。
- [refactor(providers) 系列](https://github.com/qwibitai/nanoclaw/pull/3584) —— #3584、#3586、#3588、#3591、#3592 均为 8/27 前后创建的大型重构，彼此关联、更新活跃但无一合入。建议维护者规划合并批次，避免分支长时间分叉。
- [fix(mount-security): close allowlisted-extra mount bypass in validateSpec (#3680)](https://github.com/qwibitai/nanoclaw/pull/3680) —— 安全修复已等待 6 天，建议在任何下次 Release 前优先合入。
- 另需追溯：[Issue #1820](https://github.com/qwibitai/nanoclaw/issues/1820) 被 #3714 再次引用，说明其修复状态仍未满足用户预期，建议维护者同步检查。

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

# NullClaw 项目日报（2026-09-05）

## 1. 今日速览

过去 24 小时项目活跃度处于低位：仅 1 条 Issue 更新，没有任何 PR 合并/关闭，也没有新版本发布。唯一动态是 [#993](https://github.com/nullclaw/nullclaw/issues/993)（Firecrawl 搜索端点可配置化请求）在 9 月 4 日获得更新，但状态仍为 OPEN。项目代码合并流为零，长期来看需关注是否有维护者持续跟进；目前无 Bug、崩溃或回归类报告，健康度未见明显恶化，但社区需求响应速度偏慢。

## 2. 版本发布

本期无新版本发布（最新 Releases 为空）。

## 3. 项目进展

过去 24 小时无 PR 被合并或关闭，项目在功能开发和 Bug 修复层面没有代码进展。唯一值得关注的是 Issue [#993](https://github.com/nullclaw/nullclaw/issues/993) 仍在活跃讨论中，说明自托管场景下的配置能力需求尚未得到代码层面响应。

## 4. 社区热点

**[#993 [enhancement] feat: make Firecrawl search endpoint configurable for self-hosted instances](https://github.com/nullclaw/nullclaw/issues/993)**（作者：Crymfox，创建于 2026-08-24，更新于 2026-09-04，评论：1）

该 Issue 是过去 24 小时唯一的讨论热点。核心诉求是 `src/tools/web_search_providers/firecrawl.zig` 中硬编码的 API 端点：`const endpoint = "https://api.firecrawl.dev/v1/search";` 导致自托管 Firecrawl 实例无法通过原生 `search_provider: "firecrawl"` 使用。背后反映的是自托管用户对服务地址可配置化、避免锁定官方 SaaS 端点的普遍需求，可能涉及数据隐私、内网部署或合规要求。

## 5. Bug 与稳定性

今日无新 Bug、崩溃或回归报告。待观察项仅为 [#993](https://github.com/nullclaw/nullclaw/issues/993) 这类配置缺陷，它不算运行时崩溃，但会导致自托管环境完全不可用，属于功能性阻塞问题，建议后续作为优先级较高的增强处理。

## 6. 功能请求与路线图信号

- **[#993](https://github.com/nullclaw/nullclaw/issues/993) 请求将 Firecrawl 端点从硬编码改为可配置**（例如通过环境变量或配置文件）。从代码改动角度看，处理方式是将 `const endpoint = "..."` 替换为可注入的配置项，属于低风险、低破坏性的修改，适合纳入下一个 minor/patch 版本。该 Issue 已开放 12 天，若维护者认可方向，建议尽快给出方案或实现。

## 7. 用户反馈摘要

来自 Issue [#993](https://github.com/nullclaw/nullclaw/issues/993) 的用户描述，作者 Crymfox 明确指出硬编码端点是采用 `search_provider: "firecrawl"` 自托管部署的主要障碍。该用户的使用场景是自托管 Firecrawl 实例，痛点非常具体：原生 provider 无法指向自建服务，必须绕过或修改源码。目前该 Issue 评论数为 1，评论的具体内容不在本次数据快照范围内，但仅从描述即可判断用户希望配置方式与 Docker/self-host 部署模式对齐。

## 8. 待处理积压

- **[#993](https://github.com/nullclaw/nullclaw/issues/993)（OPEN，创建于 2026-08-24，更新于 2026-09-04）**：Firecrawl 端点可配置化请求，已积压 12 天。虽然 9 月 4 日仍有更新，但尚未有维护者回复、关闭或关联 PR。结合项目当日无 PR 合并的情况，此需求可能在短期内无法得到满足，建议维护者优先关注。

---
*报告生成时间：2026-09-05。数据来源：NullClaw GitHub 仓库公开数据。*

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw 项目动态日报 — 2026-09-05

## 1. 今日速览

过去 24 小时 IronClaw 整体处在**中高活跃度**状态，且项目健康度良好：3 条 Issue 中有 2 条闭环（#7956、#7955 均已在今日关闭），12 条 PR 中有 3 条被合并、9 条待合并，没有新版本发布。今日工作重心清晰集中在两个方向：**Telegram 渠道配对/连接引导文案修复**，以及 **WebUI 斜杠命令体验打磨**。两条存在近一周的 Telegram 连接 Bug（#7956、#7955）均随对应 fix PR 的合并而关闭，表明 Issue 从报告到修复的闭环链路运转顺畅。当前 9 条待合并 PR 中大部分为 WebUI 交互改进与 Telegram 功能增强，说明项目正在从核心协议稳定性向渠道体验层持续投入。

---

## 2. 版本发布

过去 24 小时没有检测到新版本 Release，故本节从略。

---

## 3. 项目进展

今日共有 3 条 PR 被合并/关闭，均为修复性变更，且有两条直接解决了长期存在的渠道 Bug：

**已合并 PR**

- **[#8073] fix(device-link): 管理员未配置时不将责任归咎于用户账号**（size: M，merged 2026-09-05）  
  修复当管理员未配置 `telegram_api_id`/`telegram_api_hash` 时，个人账号绑定流程出现误导性的 “Something went wrong while linking” 报错。现在会明确提示用户“功能未由管理员配置”，而非让用户误以为是自己账号的问题。该 PR 直接关闭了 Issue #7955。  
  https://github.com/nearai/ironclaw/pull/8073

- **[#8054] fix(assistant): 在命令准入前先检查配对状态**（size: M，merged 2026-09-05）  
  这是对 Telegram 首次交互的重要修复：未配对用户点击 Start 发送裸 `/start` 时，原来会直接得到命令清单而非配对/连接引导。根因是产品工作流在配对查找之前就执行了命令准入。该 PR 修复后，首次联系用户会得到正确的连接指引。对应关闭 Issue #7956。  
  https://github.com/nearai/ironclaw/pull/8054

- **[#8062] fix(llm): OpenAI 请求路径发送会话缓存键**（size: XL，merged 2026-09-04）  
  在 loop-host 网关层为每个会话生成稳定的、域隔离的假名化 prompt-cache key，并在所有受支持的 OpenAI Responses 及 OpenAI 兼容 Chat Completions 请求路径上传递该 key。这会带来跨轮次、跨工具循环的缓存命中提升，对成本和延迟都是正向改进。  
  https://github.com/nearai/ironclaw/pull/8062

**整体评估**：项目今日完成了 2 个 Telegram 渠道关键 UX Bug 的闭环，同时落地了 1 个跨模型的缓存基础设施改进。加上 9 条待合并 PR 的持续输入，项目正在以“渠道体验修复 + WebUI 交互打磨 + 会话基础设施增强”三条线并行推进。

---

## 4. 社区热点

今日没有出现评论数极高的热点讨论 Thread——所有 3 条活跃 Issue 的评论数均为 0，说明社区讨论并未集中爆发，项目的协作更多体现在快速的 Bug 报告 → PR 修复链路中。

从项目流动看，**最值得关注的是 Issue #8074**：

- **[#8074] [OPEN] 已配对用户在“未连接”共享频道操作时收到的是配对引导文案**  
  https://github.com/nearai/ironclaw/issues/8074

该 Issue 由同一作者（thisisjoshford）在 9 月 4 日提出，紧跟着 #7956/#7955 这两条 Telegram 引导文案 Bug 的修复而出现。它描述的是同类问题的**第三个边界场景**：用户已经完成配对，但共享频道对安装方而言未连接。此时系统错误地向用户展示了面向“未配对用户”的 `connect_required` 文案，而非“频道未连接”的说明。这反映出维护者在系统性地收敛 Telegram 连接状态提示体系，并且社区/维护者对这类误导性文案的错误容忍度很低。这不太像社区抱怨，更像是维护团队内部驱动的体验一致性整治。

此外值得留意的是，WebUI 一批来自 italic-jinxin 的 PR（#8068/#8069/#8070/#8071）在同一天集中提交，说明内部正在对斜杠命令 UI 做一轮整体打磨，虽然没有产生大量公开讨论，但产品迭代意图非常明确。

---

## 5. Bug 与稳定性

今日共有 3 条活跃 Bug Issue，按严重程度排序如下：

| 严重程度 | Issue | 状态 | 描述 | 对应修复 |
|---|---|---|---|---|
| 中 | [#8074] 已配对用户在不连接的共享频道收到配对文案 | **OPEN**，创建于 09-04 | 用户已配对但频道未连接时，收到的是针对“未配对用户”的引导文案，与实际场景不符，易产生困惑 | 暂无直接 fix PR。建议维护者将 #8073 的修复思路推广到该场景 |
| 中 | [#7956] 未配对用户首次 /start 收到命令清单 | **CLOSED**，09-05 关闭 | Telegram bot 首次打开、未携带配对码时，应显示连接/配对提示，却显示了命令清单，会让首次接触用户找不到入口 | ✅ 已由 PR #8054 修复 |
| 低 | [#7955] 管理员未配置 api_id/api_hash 时提示 “Something went wrong” | **CLOSED**，09-05 关闭 | 个人账号连接因管理员侧配置缺失失败时，报错文案把问题归咎于用户账号，造成误导 | ✅ 已由 PR #8073 修复 |

**稳定性评估**：#7956 与 #7955 两条 Bug 都在报告后约 8 天内完成修复并闭环，反应速度良好。#8074 与这两条同源，属于同一文案体系中的剩余场景，风险不高，但在 Telegram 连接提示彻底统一前，渠道侧的用户引导仍可能继续暴露类似边界场景。建议后续做一次完整的“配对 × 频道连接”状态矩阵文案审查。

---

## 6. 功能请求与路线图信号

今日没有来自社区的新功能请求 Issue，但 PR 队列里有几条功能向变更值得关注，它们比 Bug 修复更能反映 IronClaw 的路线图方向：

- **[#8072] feat(telegram): 激活时注册 Bot API 命令菜单**（size: L，OPEN）  
  通过 `setMyCommands` 将频道声明的命令（`/model`、`/status`、`/new`、`/stop`、`/interrupt`）注册到 Telegram 聊天菜单按钮，并在停用时用 `deleteMyCommands` 做 best-effort 清理。这是对 Telegram 客户端可发现性的直接提升，预计会在下一个版本合入。  
  https://github.com/nearai/ironclaw/pull/8072

- **[#8067] feat(subagent): 后台投递的 boot/periodic sweep、计数器和 e2e 恢复（R4）**（size: XL，OPEN）  
  补上了此前缺失的启动期全面扫描机制。背景子代理投递此前有两种恢复触发：结算时投递、父线程下次启动 run 时清扫。但仍覆盖不到“父线程永远不会再次运行”的边界。该 PR 为这类数据持久写入的结果增加 boot 扫描和周期清扫。这是子代理可靠性债的第四期还债，说明上下文工程（subagent）仍是重点投入方向。  
  https://github.com/nearai/ironclaw/pull/8067

- **[#8061] feat(subagent): 并发子代理上限（R2 debt）+ 验证子门卡重放（R3 3b）**（size: M，OPEN）  
  在无生产变更的情况下，验证了通过 URL 打开被阻塞子代理线程时审批卡片确实能正常展示；同时补上并发子代理数量上限这一设计债。  
  https://github.com/nearai/ironclaw/pull/8061

结合 #8062（prompt 缓存 key）等合并来看，**IronClaw 近期的路线图信号包括：Telegram 渠道完善、子代理可靠性补全、LLM 请求基础设施优化，以及 WebUI 交互体验细节打磨**，而非新的垂直功能大特性。

---

## 7. 用户反馈摘要

今日所有 Issue/PR 的评论区均无直接文本评论（Comments: 0），故这里基于 Issue 描述及修复链路，提炼背后的真实用户场景与痛点：

1. **Telegram 新用户首次触发 bot 时感到“找不到入口”**（#7956）  
   用户在 Telegram 中点击 Start，期待看到“如何连接/配对”的引导，结果看到的是 `/interrupt`、`/model` 等面向已配对用户的命令清单。对第一次接触 bot 的潜在用户来说，这造成了明显的认知断层——不知道该做什么、也不知道这个 bot 能用来干嘛。

2. **配置错误被误报为用户自身账号问题时，用户会归因错误**（#7955）  
   当管理员没有配置 Telegram MTProto API 凭据时，普通用户走完设置流程却看到 “Something went wrong while linking your account”——用户会反复检查自己的账号、重新尝试绑定，而实际上问题完全在服务端部署配置。这类错误归因文案对用户极具挫败感，且浪费支持资源。

3. **配对状态与频道连接状态是两个独立维度，用户容易混淆**（#8074）  
   已配对用户不代表所属的共享频道已对安装方连接。当用户在频道内操作被拒时，如果看到的是“请先在 IronClaw Web 应用中连接你的账号”这类文案，会误以为自己的配对状态失效，而真正出问题的是频道连接。这提示用户心智模型需要更精确的渠道状态提示。

**整体评价**：目前用户侧反馈集中在 Telegram 渠道的引导文案精确度上，不涉及核心功能缺失或性能抱怨。项目对这些反馈的响应和修复速度迅速（约 1 周内闭环），用户体验改善预期明显。

---

## 8. 待处理积压

当前有 1 条 OPEN Bug 和 9 条 OPEN PR 处于待处理状态，整体积压压力较小。需要特别关注的条目如下：

**未关闭 Issue：**

- **[#8074] 配对用户在未连接共享频道时收到错误文案**（OPEN，创建 09-04，0 评论）  
  https://github.com/nearai/ironclaw/issues/8074  
  与今日已修复的两条 Bug（#7956、#7955）同属一个文案体系，建议维护者在后续迭代中一并处理，避免同类问题反复出现。

**等待合并的 PR：**

- **[#7988] chore(agents): 刷新代码库知识图谱**（OPEN，创建于 08-29，已停留 7 天）  
  由 CI bot 自动生成的代码库记忆快照刷新 PR。这类 PR 通常只需常规 review 后合并，已等待超一周，建议维护者顺手处理。  
  https://github.com/nearai/ironclaw/pull/7988

- **[#8072] feat(telegram): 注册 Bot API 命令菜单**（OPEN，size: L，等待 review）  
  功能完整、风险标注为 low，但改动 scope 涉及 docs 和 dependencies，可能需要更仔细的审查。  
  https://github.com/nearai/ironclaw/pull/8072

- **[#8059] fix(responses): 发送产品界面接受的取消原因**（OPEN，创建 09-03）  
  该 PR 修复 `POST /api/v1/responses/{id}/cancel` 在所有状态下都返回 400 的问题。cancel 接口完全不可用是偏严重的功能缺陷，建议优先 review。  
  https://github.com/nearai/ironclaw/pull/8059

**队列整体评估**：9 条 OPEN PR 中 5 条来自 italic-jinxin 的 WebUI 改进、3 条来自 henrypark133 的子代理/LLM 基础设施优化，产出集中、scope 清晰，目前没有发现被长期搁置的“僵尸 PR”或无人响应的求助 Issue。项目 backlog 较为健康。

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# LobsterAI 项目动态日报 — 2026-09-05

> 数据来源：[github.com/netease-youdao/LobsterAI](https://github.com/netease-youdao/LobsterAI) | 统计周期：2026-09-04 至 2026-09-05


## 1. 今日速览

过去 24 小时项目处于高强度的版本收尾与持续迭代状态：共合并/关闭 28 条 PR，发布 2 个新版本（2026.9.4 / 2026.9.3），核心功能（内嵌浏览器、订阅恢复、协作登录引导）均有显著推进。Issues 侧仅 1 条更新，且为历史遗留的 SQLite 存储层严重缺陷（#1071），社区外部反馈信号较弱，项目整体以内部开发驱动为主。活跃度评定：**高**——Release 节奏快，PR 合并密度大，但公开 Issue 讨论/用户反馈量偏低，需留意社区声音的收集。

- 版本发布：2 个（2026.9.4、2026.9.3）
- PR 合并/关闭 28 条，待合并 5 条
- Issues：1 条更新（open，stale）
- 当前待处理严重 Issue：1 条（存储层数据完整性缺陷，暂无 fix PR）


## 2. 版本发布

### LobsterAI 2026.9.4（2026-09-04 发布）

**更新亮点：**
- feat(browser): 恢复可交互的 in-app 浏览器（[PR #2602](https://github.com/netease-youdao/LobsterAI/pull/2602)）— 9 月 3 日版本引入的交互式浏览器出现回退，该版本重新合入
- feat(update): 安装前增加确认步骤，退出 App 前增加确认提示（[PR #2609](https://github.com/netease-youdao/LobsterAI/pull/2609)）— 优化升级流程的用户可控性

**破坏性变更/迁移注意：** Release Note 截断，未明确标注破坏性变更。但涉及 update 流程行为变化（新增确认步骤），企业用户需注意静默升级场景可能受影响；in-app 浏览器二次合入，Windows Unicode 路径问题由 [PR #2615](https://github.com/netease-youdao/LobsterAI/pull/2615) 单独修复，建议 Windows 用户重点验证。

### LobsterAI 2026.9.3（2026-09-03 发布）

**更新亮点：**
- feat(cowork): 未认证用户在无自定义模型配置时发起聊天，先展示登录引导弹窗（[PR #2573](https://github.com/netease-youdao/LobsterAI/pull/2573)）
- feat(browser): 新增可交互的 in-app 浏览器（[PR #2574](https://github.com/netease-youdao/LobsterAI/pull/2574)）

**破坏性变更/迁移注意：** 该版本 in-app 浏览器在 Windows 非 ASCII 安装路径下存在缺陷，已由 2026.9.4 中 [PR #2615](https://github.com/netease-youdao/LobsterAI/pull/2615) 修复。仍在使用 2026.9.3 的 Windows 用户建议尽快升级至 2026.9.4。


## 3. 项目进展

今日合并/关闭的 28 条 PR 标志着 2026.9.4 版本的完整落地（[Release/2026.9.4 PR #2618](https://github.com/netease-youdao/LobsterAI/pull/2618) 已关闭）。按主题归类如下：

### 🧭 内嵌浏览器（主要看点）
- 合入 in-app 浏览器的功能恢复（[PR #2602](https://github.com/netease-youdao/LobsterAI/pull/2602)）
- 修复 Windows Unicode 安装路径下浏览器 MCP 启动失败的问题（[PR #2615](https://github.com/netease-youdao/LobsterAI/pull/2615)）
- 修复 CI：技能审计时长受限（[PR #2616](https://github.com/netease-youdao/LobsterAI/pull/2616)）
- 改进 in-app 登录反馈与标签页交互（[PR #2617](https://github.com/netease-youdao/LobsterAI/pull/2617)，待合并）

### 💳 订阅与商业化链路
- 完善 Artifact/资料库/站点详情的订阅恢复入口，区分自动恢复与订阅后重新部署两种路径；补充恢复策略埋点（[PR #2613](https://github.com/netease-youdao/LobsterAI/pull/2613)）

### 🤝 协作与登录体验
- 未认证用户发起聊天前的登录引导弹窗（[PR #2573](https://github.com/netease-youdao/LobsterAI/pull/2573)）
- 登录刷新期间保留已选模型展示，但不允许旧模型继续运行（[PR #2612](https://github.com/netease-youdao/LobsterAI/pull/2612)）
- 聊天登录 CTA 点击归因到 onboarding 分析事件（[PR #2596](https://github.com/netease-youdao/LobsterAI/pull/2596)）
- 侧边栏免费 token 提示 5 秒自动淡出，并清理认证状态变化时的计时器（[PR #2532](https://github.com/netease-youdao/LobsterAI/pull/2532)）

### 🛠 客户端稳定性与体验修复
- 主窗口文本输入框支持 Cut/Copy/Paste/Select All 编辑菜单（[PR #2503](https://github.com/netease-youdao/LobsterAI/pull/2503)）
- 技能升级进度浮层通过 document.body 渲染，覆盖完整应用外壳（[PR #2501](https://github.com/netease-youdao/LobsterAI/pull/2501)）
- 插件安装弹窗在长错误信息下仍可使用，弹窗内独立滚动 + 增加关闭按钮（[PR #2520](https://github.com/netease-youdao/LobsterAI/pull/2520)）
- 上下文菜单打开时保留选中文本的右键编辑菜单支持（[PR #2521](https://github.com/netease-youdao/LobsterAI/pull/2521)）
- 多实例机器人卡片限制为两列响应式布局（[PR #2599](https://github.com/netease-youdao/LobsterAI/pull/2599)）
- 中文语音配额耗尽文案更新为新的 free-trial 订阅表述（[PR #2603](https://github.com/netease-youdao/LobsterAI/pull/2603)）

> 整体判断：这一个发布周期内，项目完成了“浏览器能力 + 登录引导/订阅转化闭环 + 安装与升级确认机制”三条主线的集中交付，产品形态向具备内嵌浏览器的协同工作台方向演进。


## 4. 社区热点

### Issue #1071 — SQLite 存储层数据完整性缺陷
- 链接：[LobsterAI Issue #1071](https://github.com/netease-youdao/LobsterAI/issues/1071)
- 创建于 2026-03-30，`[stale]`，但 2026-09-04 仍有评论更新

该 Issue 是今日唯一的 Issue 动态，也是项目当前讨论价值最高的一条。核心质疑聚焦在 `src/main/sqliteStore.ts` 与 `coworkStore.ts` 的存储可靠性，涉及三个层面：

1. **CASCADE 删除级联失效**：`cowork_messages` 表外键约束在 SQLite 默认配置下不生效，会话删除后孤儿消息持续累积；
2. **save() 非原子写入**：崩溃可能导致存储文件损坏；
3. **storeInitPromise 永久故障**：初始化超时后无自愈机制，功能永久不可用。

> **热点分析**：这不是使用体验类反馈，而是来自对代码库有深入理解的用户（MaoQianTu）的底层架构审计结果。开发者社区的关注点在于 SQLite 这一核心存储基础设施的可靠性——三个缺陷叠加可能造成**数据丢失 + 存储膨胀 + 永久性功能锁死**的组合风险。此类审计型 Issue 虽讨论热度不高，但技术含金量和潜在影响大，值得维护者优先回应。

### Pull Request 侧
PR 评论数据均为 `undefined`（未展示），无单条高讨论量 PR。但从合并密度看，[@liuzhq1986](https://github.com/liuzhq1986)（约 15 条）与 [@btc69m979y-dotcom](https://github.com/btc69m979y-dotcom)（约 6 条）是今日的核心贡献者，集中在 UI/UX 细节打磨与浏览器能力建设上。


## 5. Bug 与稳定性

按严重程度排列：

| 严重度 | 问题 | 状态 | Fix PR |
|---|---|---|---|
| **严重** | SQLite 存储层：会话删除后孤儿消息无限累积（CASCADE 失效）；save() 非原子写崩溃损坏风险；初始化超时后永久故障（[#1071](https://github.com/netease-youdao/LobsterAI/issues/1071)） | Open，长时间未响应 | 无，待认领 |
| 中等 | 长错误信息导致插件安装弹窗按钮不可见，安装流程可能卡死（[PR #2520](https://github.com/netease-youdao/LobsterAI/pull/2520)） | 已修复合入 | [PR #2520](https://github.com/netease-youdao/LobsterAI/pull/2520) |
| 中等 | 技能升级进度浮层未覆盖完整 app shell，iOS/Android 端显示不一致（[PR #2501](https://github.com/netease-youdao/LobsterAI/pull/2501)） | 已修复合入 | [PR #2501](https://github.com/netease-youdao/LobsterAI/pull/2501) |
| 中等 | 登录刷新期间已选模型展示短暂消失（[PR #2612](https://github.com/netease-youdao/LobsterAI/pull/2612)） | 已修复合入 | [PR #2612](https://github.com/netease-youdao/LobsterAI/pull/2612) |
| 较低 | Windows 非 Unicode 路径下 in-app 浏览器 MCP 启动失败（[PR #2615](https://github.com/netease-youdao/LobsterAI/pull/2615)） | 已修复合入 | [PR #2615](https://github.com/netease-youdao/LobsterAI/pull/2615) |
| 较低 | 聊天输入等文本控件缺少编辑菜单（剪贴板操作） | 已修复合入 | [PR #2503](https://github.com/netease-youdao/LobsterAI/pull/2503) |
| 较低 | 机器人卡片在 IM 多实例场景下布局溢出/空卡片不紧凑（[PR #2599](https://github.com/netease-youdao/LobsterAI/pull/2599)） | 已修复合入 | [PR #2599](https://github.com/netease-youdao/LobsterAI/pull/2599) |

> 观察：今日合并的修复类 PR 大多是对历史遗留 UI/交互问题的清理（8 月创建的 PR 在今日集中合入），而 #1071 这类存储底层架构问题仍悬而未决。项目在“表面体验修复”上节奏很快，但“深度可靠性债务”的偿还未见启动。


## 6. 功能请求与路线图信号

今日 Issue 无新功能请求提交。但从 PR 合入趋势上，可以识别出如下产品方向信号：

| 方向 | 信号 | 状态推断 |
|---|---|---|
| **内嵌浏览器持续投入** | 9.3 新增 → 9.4 恢复（[#2602](https://github.com/netease-youdao/LobsterAI/pull/2602)）→ 待合并改进登录与标签控件（[#2617](https://github.com/netease-youdao/LobsterAI/pull/2617)） | **活跃开发中**，登录态管理、标签栏交互是当前迭代重点，预计进入 2026.9.5 |
| **订阅恢复与状态同步** | Artifact/资料库/站点详情增加“订阅恢复”入口；区分“自动恢复”与“订阅后重新部署”（[#2613](https://github.com/netease-youdao/LobsterAI/pull/2613)） | 商业化闭环打磨明显，订阅到期/恢复场景的产品细节被重视 |
| **用户引导与 onboarding** | 登录前引导弹窗（[#2573](https://github.com/netease-youdao/LobsterAI/pull/2573)）、登录 CTA 埋点（[#2596](https://github.com/netease-youdao/LobsterAI/pull/2596)）、promo 提示自动淡出（[#2532](https://github.com/netease-youdao/LobsterAI/pull/2532)） | 新用户激活路径的转化率优化正在进行 |
| **升级/安装流程干预** | 安装前确认 + 退出前确认（[#2609](https://github.com/netease-youdao/LobsterAI/pull/2609)） | 提升用户对自动更新的掌控感，降低误更新/被打断的负面体验 |


## 7. 用户反馈摘要

- **社区参与度偏低**：过去 24 小时仅有 1 条 Issue 更新、0 个 Issue 获 👍，PR 评论数据缺失。公开社区反馈渠道活跃度不足，需要警惕“开发团队自转”而用户声音缺席的风险。
- **中文用户体验细节在收敛**：多个合入 PR 显示中文环境下语音配额文案、free-trial 订阅措辞、测试环境 API 地址切换（[PR #2614](https://github.com/netease-youdao/LobsterAI/pull/2614)）等 i18n/本地化细节在被持续修正，说明中文用户体验仍存在零散问题但处于快速修复通道中。
- **来自 #1071 的用户诉求本质**：用户 MaoQianTu 在评论/Issue 中并非抱怨使用体验，而是以代码审计的方式提出存储层将导致“生产环境数据丢失或功能永久故障”。此类反馈代表了**高级用户/企业客户对数据安全的核心关注**——即使 UI 打磨得再好，存储层可靠性问题会摧毁信任根基。


## 8. 待处理积压

### 🔴 高优先级提醒
| 项目 | 详情 | 链接 | 积压时长 |
|---|---|---|---|
| **Issue #1071**：SQLite 存储层三个数据完整性/可靠性缺陷 | Open / `[stale]`，最新评论 2026-09-04，无 fix PR | [查看 Issue](https://github.com/netease-youdao/LobsterAI/issues/1071) | 自 2026-03-30 至今约 **159 天** |

该 Issue 已于 3 月提出，被机器人标记为 stale 后仍收到用户评论——说明用户认为该问题未得到应有重视。现在距离开启已超过 5 个月，建议维护者：

1. 明确回应三个缺陷的复现评估结论；
2. 给出修复计划或说明为何在现有架构下风险可接受；
3. 若确认修复，建立跟踪链接并将 Issue 从 stale 状态移除。

### 🌕 待合并 PR（共 5 条）
可确认的 1 条为 [PR #2617 — fix(browser): improve in-app login and tab controls](https://github.com/netease-youdao/LobsterAI/pull/2617)，属于浏览器体验的增量改进。其余 4 条未在列表头部展示，维护者需关注是否有等待时长过久、可能冲突的 PR 积压。

### ⚪ 其他长期未关闭但今日合入的 PR
- [PR #2503](https://github.com/netease-youdao/LobsterAI/pull/2503)（8-17 创建 → 9-04 关闭，历时 18 天）
- [PR #2501](https://github.com/netease-youdao/LobsterAI/pull/2501)（8-17 创建 → 9-04 关闭，历时 18 天）
- [PR #2520](https://github.com/netease-youdao/LobsterAI/pull/2520)（8-24 创建 → 9-04 关闭，历时 11 天）
### ⚪ 未发现新待处理的 feature request 积压（今日无新开 Issue）


### 健康度总结

项目处于**高速迭代的健康节奏**中：版本发布及时（2 个/24h）、PR 流动顺畅（28 合入 vs 5 待合并）、修复覆盖 UI/构建/存储/国际化等多个层面。当前最大的健康度风险项是 **Issue #1071 的存储层可靠性债务**（150+ 天无主）与**社区参与度偏低**。建议在版本节奏维持的同时，抽出资源回应底层审计类反馈，避免核心基础设施风险长期悬置。

---
*本报告基于 GitHub 公开数据自动生成，部分 Release Note 与 PR 描述存在截断，完整信息请参阅原始链接。*

</details>

<details>
<summary><strong>TinyClaw</strong> — <a href="https://github.com/TinyAGI/tinyagi">TinyAGI/tinyagi</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>Moltis</strong> — <a href="https://github.com/moltis-org/moltis">moltis-org/moltis</a></summary>

# Moltis 项目动态日报 — 2026-09-05

## 1. 今日速览

过去 24 小时 Moltis 项目活跃度偏低，共发生 2 条更新：1 条新功能增强 Issue、1 条待审查 PR，无新版本发布，也无 PR 被合并或关闭。Issue #1259 提出跨会话持久化默认推理强度级别的配置能力，PR #1258 则为外部 agent（agy CLI）增加直接流式传输支持。整体来看，项目仍处于稳步迭代状态，功能讨论集中在外部 agent 集成与推理参数体验优化两个方向。

## 2. 版本发布

今日无新版本 Release。

## 3. 项目进展

今日无 PR 被合并或关闭。

值得关注的候选变更为 PR #1258（待审查中）：

- **PR #1258** — [feat(external-agents): add direct AGY streaming](https://github.com/moltis-org/moltis/pull/1258)  
  作者为 GTanger。该 PR 为官方 `agy` CLI 增加了头等流式传输支持，核心思路是复用 agy 已有的 Google OAuth 会话，无需 Gemini CLI 或 API key 即可对接。同时将 AGY 的版本化 `stream-json` 输出翻译为 Moltis 的文本、推理、通知、工具调用、子 agent、用量统计及可恢复会话等事件类型。  
  目前该 PR 尚未获得 review，若顺利合入，将进一步完善 Moltis 对多种外部 agent 生态的兼容能力。

## 4. 社区热点

今日未有评论数或反应数较高的讨论热点。新增的 Issue #1259 与 PR #1258 均处于初始提交/待审状态，评论数为 0，社区讨论热度不高。

| 条目 | 链接 | 评论数 | 状态 |
|------|------|--------|------|
| #1259 [Feature] 可配置默认推理等级 | [Issue #1259](https://github.com/moltis-org/moltis/issues/1259) | 0 | 新开 |
| #1258 feat: add direct AGY streaming | [PR #1258](https://github.com/moltis-org/moltis/pull/1258) | 0 | 待审查 |

## 5. Bug 与稳定性

今日无 Bug、崩溃或回归类 Issue 报告。未发现需要紧急关注或标注严重程度的问题。

## 6. 功能请求与路线图信号

- **[Issue #1259](https://github.com/moltis-org/moltis/issues/1259)** — [enhancement] Configurable default reasoning/thinking level（可配置的默认推理/思考等级，并跨会话持久化）  
  作者 Scentedtiger 在预检清单中确认已搜索过现有 enhancement 请求且未发现重复提案，说明该需求存在真实缺口。该请求背后暗示用户希望在多个会话中保持一致的推理等级偏好，而不是每次重新调整，属于体验优化类需求。  
  考虑到 PR #1258 正在推进更多外部 agent（如 agy）的接入，不同 agent 的推理模式与强度差异可能促使项目对“默认推理等级”的会话级配置做统一规划。该请求有一定合理性，或可作为下一迭代的候选特性。

## 7. 用户反馈摘要

今日所有 Issue/PR 均无评论，未能从评论中提炼出具体使用场景或满意度反馈。仅能从 Issue #1259 本身推断：部分用户已在使用过程中积累了对推理等级个性化配置的明确诉求，且该用户具备较强的 issue 检索习惯，属于较高质量的 feature request。

## 8. 待处理积压

今日无长期未响应的高优先级 Issue 或 PR 暴露。

一个需要维护者关注的时间点是 **PR #1258** 自 2026-09-04 创建以来，已过去 1 天仍处于待审查状态，且涉及多个模块（外部 agent 流式传输、OAuth 复用、事件格式翻译），建议尽快安排 review，以降低后续合并冲突风险并加速外部 agent 集成进度。链接：[PR #1258](https://github.com/moltis-org/moltis/pull/1258)

---

**总结**：Moltis 今日整体活跃度不高，项目健康度良好，无稳定性问题或积压压力。当前核心关注点应落在 PR #1258 的评审推进与 Issue #1259 的路线图讨论上。

</details>

<details>
<summary><strong>CoPaw</strong> — <a href="https://github.com/agentscope-ai/CoPaw">agentscope-ai/CoPaw</a></summary>

# CoPaw 项目动态日报 — 2026-09-05

> 数据窗口：2026-09-04 ~ 2026-09-05 | 数据来源：agentscope-ai/QwenPaw GitHub

## 1. 今日速览

过去 24 小时项目活跃度较高：Issues 更新 23 条（新开/活跃 15，关闭 8），PR 更新 26 条（20 条待合并，6 条已合并/关闭），无新版本发布。整体处于 **2.2.x 稳定迭代期**：权限安全修复（MCP per-tool 白名单落地）、Console/UI 状态一致性修复、多租户 Hub 方向讨论是今日主线。值得警惕的是，**任务执行控制类 Bug（停止失效、409 冲突）在 2.2 版本上被同一用户连续复现**，说明 Agent 任务生命周期管理仍是当前最突出的稳定性短板。项目健康度总体良好，但"执行状态可信度"和"局域网 LLM 连接鲁棒性"需要维护者在下个 patch 版本优先回应。

## 2. 版本发布

过去 24 小时无新版本 Release。

## 3. 项目进展

过去 24 小时关闭的 PR 中，有 3 条可直接对应到具体功能/修复的落地，是项目向 2.2.1/后续版本迈进的关键节点：

| PR | 内容 | 关联 Issue | 意义 |
|---|---|---|---|
| [#7504](https://github.com/agentscope-ai/QwenPaw/pull/7504) | **fix(mcp): enforce per-tool whitelist on the agent runtime path** | [#7470](https://github.com/agentscope-ai/QwenPaw/issues/7470)（已关闭） | 修复了 MCP per-tool 白名单仅在 Console 和外部 harness 路径生效、却未在 Agent 运行时主路径执行的权限绕过问题。这是 2.0 Driver 重写后一个较关键的安全/合规缺口，本次关闭意味着 Agent 工具调用将真正遵守 `card.config.tools` 白名单约束。 |
| [#7183](https://github.com/agentscope-ai/QwenPaw/pull/7183) | **feat(skills): add workspace-scoped preload configuration** | [#7182](https://github.com/agentscope-ai/QwenPaw/issues/7182)（已关闭） | 为 Skill 引入 workspace 级 `on_demand` / `preload` 策略，使以某个 Skill 为核心构建的 workspace 无需在每次新对话中重新发现/加载工具，减少首轮工具调用开销。属于 Agent 工程效率优化。 |
| [#7560](https://github.com/agentscope-ai/QwenPaw/pull/7560) | **fix(console): preserve selected loop mode query** | [#7552](https://github.com/agentscope-ai/QwenPaw/issues/7552)、[#7555](https://github.com/agentscope-ai/QwenPaw/issues/7555)（均已关闭） | 修复了 Composer 菜单中选定的 Loop 模式无法到达后端、页面切换后模式显示被重置的问题。通过 `beforeSubmit` 将 query 带回 SDK，保证队列提交与 UI 状态一致。 |

综合来看，本次窗口内项目在 **MCP 权限执行链完整性、Skill 装配效率、长任务 Loop 模式前后端一致性**三个方向都有实质收敛。其中 MCP 白名单修复是一次值得强调的安全正确性补强。

另有一条大型 PR [#7486](https://github.com/agentscope-ai/QwenPaw/pull/7486)（Creator 1.1.2）仍在开放状态，涵盖 runtime notification bus、async delegation、multi-timeline A/B compare、T2V/I2V/S2V 调度等大量功能，是 fork 内工作的整体上游同步，体量较大，预计需要较长时间 Review。

## 4. 社区热点

### 4.1 [#7318 QwenPaw Hub 多租户版路线图讨论](https://github.com/agentscope-ai/QwenPaw/issues/7318) — 22 条评论，3 👍

维护者 rayrayraykk 发起的征集帖，宣布 **QwenPaw Hub 多租户版将在 2.2.0 推出**，并向社区征集"下一步应该构建什么"。该帖更新于 9 月 4 日仍保持活跃，且援引了 [#2324 多用户访问与管理员托管 Skills](https://github.com/agentscope-ai/QwenPaw/issues/2324) 等历史诉求。

结合同期多个新 Issue 来看，**社区对"个人助手团队化/多租户化"的需求非常集中且持续**：
- [#7541](https://github.com/agentscope-ai/QwenPaw/issues/7541)（俄语）批评当前按渠道（web console/desktop/telegram）拆分 Session 的架构，认为渠道只是传输层，用户应看到统一会话；
- laob9444 连续提交的 [#7558](https://github.com/agentscope-ai/QwenPaw/issues/7558)（PostgreSQL/MySQL 存储）、[#7557](https://github.com/agentscope-ai/QwenPaw/issues/7557)（Skill 版本与依赖元数据）都面向多 Agent 团队运维场景。

这明确指向：**Hub 多租户不是单一功能，而是一组"多用户、多 Agent、可运维"的企业化能力组合**。建议维护者在 Hub 2.2.0 路线图讨论中主动关联这些 Issue，避免社区诉求分散。

### 4.2 [#7505 局域网 LLM Server 频繁 client disconnect 导致重试超时](https://github.com/agentscope-ai/QwenPaw/issues/7505) — 12 条评论

用户通过局域网访问 LM Studio Server（qwen3.8 flash next q3）时，QwenPaw 频繁出现 `client disconnect`，反复重试后最终超时失败。12 条评论说明不少用户也在局域网/个人 PC 推理场景下遇到类似的 SSE 连接稳定性问题。

### 4.3 [#6921 多步骤任务"规划后无提示停止"](https://github.com/agentscope-ai/QwenPaw/issues/6921) — 12 条评论（已于本窗口关闭）

该 Issue 虽来自 2.1beta2，但 12 条评论 + 多步骤任务自我停止的场景与当前 2.2 上 rerbin 连续提交的 [#7559](https://github.com/agentscope-ai/QwenPaw/issues/7559)、[#7567](https://github.com/agentscope-ai/QwenPaw/issues/7567) 高度同源。其关闭是"旧版本问题收敛"，但 2.2 上同类症状仍在复现，说明**任务执行中断/恢复/停止一致性问题尚未被彻底解决**，这在社区中已形成信任损耗。

## 5. Bug 与稳定性

按严重程度排列：

### 高严重度

| Issue | 描述 | 状态 | 是否有修复 PR |
|---|---|---|---|
| [#7559](https://github.com/agentscope-ai/QwenPaw/issues/7559) | 任务执行中通过对话框新发消息/提交文件触发 **409 "A task is already running"**。用户质疑：新消息应进入队列，而非直接报错 | OPEN | ❌ 无对应修复 PR |
| [#7567](https://github.com/agentscope-ai/QwenPaw/issues/7567) | 点击停止后 UI 显示已停止，实际上任务仍在后台执行，导致用户发送修正指令时遇到 409 | CLOSED（label: `Close-and-review-later`） | ⚠️ 关闭但未见对应修复 PR，建议作为 2.2.x 回归项持续跟踪 |
| [#7534](https://github.com/agentscope-ai/QwenPaw/issues/7534) | 飞书私聊会话的 queue consumer 长驻卡死，后续新消息无法创建新 consumer，会话"静默无响应" | OPEN | ❌ 无 |
| [#7554](https://github.com/agentscope-ai/QwenPaw/issues/7554) | Windows 下 Shell 工具子进程继承控制台 stdin；命令读取 stdin 时会挂起共享 cmd 控制台，Ctrl+C 无法杀死（缺乏 `CREATE_NEW_PROCESS_GROUP`） | OPEN | ❌ 无 |

### 中严重度

| Issue | 描述 | 状态 | 是否有修复 PR |
|---|---|---|---|
| [#7505](https://github.com/agentscope-ai/QwenPaw/issues/7505) | 局域网 LLM Server（LM Studio）SSE 频繁 disconnect → 重试 → 最终超时 | OPEN | ❌ 无 |
| [#7549](https://github.com/agentscope-ai/QwenPaw/issues/7549) | 请求 input 以 assistant 文本 turn 结尾时，Volcengine Ark Responses API 返回 400 `MissingParameter: partial` | OPEN | ❌ 无 |
| [#7548](https://github.com/agentscope-ai/QwenPaw/issues/7548) | 对话切换/重启后右侧快速导航记录丢失，早期消息在 UI 不可见，但数据完整存在于 history.db，疑似上下文截断导致显示错误 | OPEN | ❌ 无 |
| [#7367](https://github.com/agentscope-ai/QwenPaw/issues/7367) | 仅启用 console 渠道时启动仍需 30-45 秒；`get_channel_registry()` 无条件 import 全部 18 个渠道模块，其中 lark_oapi 单包约耗时 18.5 秒 | OPEN | ❌ 无 |

### 低严重度 / 已收敛

| Issue | 描述 | 状态 | 备注 |
|---|---|---|---|
| [#7552](https://github.com/agentscope-ai/QwenPaw/issues/7552) | Composer 菜单选择 Goal/Mission Loop 模式后，后端收到的是裸消息，仍按默认 Loop 运行 | CLOSED | [#7560](https://github.com/agentscope-ai/QwenPaw/pull/7560) 已修复 |
| [#7555](https://github.com/agentscope-ai/QwenPaw/issues/7555) | Loop 模式 UI 显示在切换页面后被重置为"默认" | CLOSED | [#7560](https://github.com/agentscope-ai/QwenPaw/pull/7560) 已修复 |
| [#7510](https://github.com/agentscope-ai/QwenPaw/issues/7510) | 2.2.0-beta.7 Desktop 的 `/memory/status` 返回 500 | CLOSED | 已关闭 |
| [#7023](https://github.com/agentscope-ai/QwenPaw/issues/7023) | Desktop 启动时同步执行 Playwright Chromium 安装，阻塞就绪路径约 60s，且无跳过/懒加载选项 | CLOSED | 已关闭 |
| [#7470](https://github.com/agentscope-ai/QwenPaw/issues/7470) | MCP per-tool 白名单未在 Agent 运行时路径生效 | CLOSED | [#7504](https://github.com/agentscope-ai/QwenPaw/pull/7504) 已修复 |

**综合判断**：2.2 上最需要立即跟进的是 **#7559 + #7567 组合**——"停止按钮不可信 → 用户误发新消息 → 409 报错"形成了连贯的负面体验闭环。`Close-and-review-later` 的标签意味着维护者已知晓，但社区需要看到一个真正解决任务取消竞态条件的修复 PR。

## 6. 功能请求与路线图信号

### 6.1 可能进入下一版本的高价值请求

| Issue | 请求 | 信号强度 | 判断 |
|---|---|---|---|
| [#7568](https://github.com/agentscope-ai/QwenPaw/issues/7568) | **Off-peak 闲时任务调度**：将大批量/长耗时任务挂起，在模型厂商低谷折扣时段（如 DeepSeek 00:30-08:30 半价）自动执行，或对接 Batch API | ⭐⭐⭐ | 符合 AI Agent 成本优化大趋势，且与 Qwen 生态的模型接入天然契合，预计会被认真评估 |
| [#7558](https://github.com/agentscope-ai/QwenPaw/issues/7558) | **可插拔关系型存储后端（PostgreSQL/MySQL）**，解决 SQLite WAL 在网络文件系统（Docker Swarm/K8s）上的不可用问题 | ⭐⭐⭐ | 与 Hub 多租户/企业部署方向强相关，属 Hub 2.2.0 的基础设施前置需求 |
| [#7557](https://github.com/agentscope-ai/QwenPaw/issues/7557) | **Skill 版本与依赖元数据**：9 个 Agent 的 workspace 各自复制 SKILL.md，无法追踪版本与更新 | ⭐⭐ | 团队化使用场景的必然需求，建议与 Hub 的 Skills 管理一并设计 |
| [#7556](https://github.com/agentscope-ai/QwenPaw/issues/7556) | **MCP Driver 多级 fallback 链**：policy 缺失时默认 `deny` 导致所有工具静默失败，需可配置 fallback | ⭐⭐⭐ | 这是一个**默认策略过严导致可用性受损**的设计问题，建议尽快调整默认行为 |
| [#7553](https://github.com/agentscope-ai/QwenPaw/issues/7553) | 产物输出折叠在已完成步骤内、位置靠后，希望展示在对话时间戳上方的独立产物区 | ⭐⭐⭐ | UI 体验类改进，改造成本低、用户感知强 |
| [#7550](https://github.com/agentscope-ai/QwenPaw/issues/7550) | Docker 镜像升级后第三方 agent（codex CLI）配置丢失，建议预装或提供一键安装 | ⭐⭐ | 镜像生命周期管理问题，可通过 volume 持久化 + 文档缓解 |

### 6.2 路线图信号

- **[#7318](https://github.com/agentscope-ai/QwenPaw/issues/7318) 已明确 2.2.0 Hub 多租户是官方方向**。结合 [#7541](https://github.com/agentscope-ai/QwenPaw/issues/7541)（会话不应按渠道分裂）与 [#7502](https://github.com/agentscope-ai/QwenPaw/pull/7502)（Console 侧边栏与设置体验重构），可以看出下一阶段产品重心是 **"从个人单会话体验 → 组织级多会话/多用户管理"的整体 UX 升级**。
- PR [#7538](https://github.com/agentscope-ai/QwenPaw/pull/7538)（统一运行时环境变量管理）与 [#7561](https://github.com/agentscope-ai/QwenPaw/pull/7561)（自动记忆生命周期 breaking refactor）都处于开放状态，预示着核心层正在经历一轮 **"配置与状态管理契约收敛"**，可能为 2.3 或 3.0 铺路。
- PR [#7378](https://github.com/agentscope-ai/QwenPaw/pull/7378)（Expo/React Native 原生移动端）仍标有 `DO NOT MERGE`，处于早期探索阶段，但值得社区关注。

## 7. 用户反馈摘要

### 7.1 真实痛点：任务执行"黑箱感"仍是头号问题

rerbin 在过去一个月内连续提交了 [#6921](https://github.com/agentscope-ai/QwenPaw/issues/6921)（任务自停）、[#7559](https://github.com/agentscope-ai/QwenPaw/issues/7559)（执行中发消息 409）、[#7567](https://github.com/agentscope-ai/QwenPaw/issues/7567)（停止后仍在执行）、[#7555](https://github.com/agentscope-ai/QwenPaw/issues/7555)（Loop 模式显示错乱），几乎覆盖了长任务执行体验的每个环节：

> "我绝对确认对话框下方的执行中的 □ 消失了、变成了正常的 ↑ …… 但我刷新页面时发现他依然在执行、事实上还在执行有误的指令。" —— [#7567](https://github.com/agentscope-ai/QwenPaw/issues/7567)

这说明用户对任务状态的信任度已经受到影响。**UI 上的停止/执行状态必须与后端真实 executor 状态严格一致**，否则用户不敢放心让 Agent 执行长任务。

### 7.2 局域网/本地模型用户的连接稳定性诉求

[#7505](https://github.com/agentscope-ai/QwenPaw/issues/7505) 的用户在 LAN + LM Studio + 量化小模型环境下使用，针对的是 `client disconnect` 后的重试策略。这类用户是 QwenPaw 本地优先场景的核心人群，建议 provider 层对 SSE 半开连接、空闲断开、短暂网络抖动有更智能的重试/退避策略。

### 7.3 架构层面的用户质疑

xguest 用俄语在 [#7541](https://github.com/agentscope-ai/QwenPaw/issues/7541) 中提出了一个尖锐的架构批评：**Session 被渠道（web/desktop/telegram）分割并被渠道阻塞，而渠道本质只是输入传输层**。这与 [#7318](https://github.com/agentscope-ai/QwenPaw/issues/7318) 的多租户讨论形成共振——当用户同时使用 Console、Desktop、飞书/Telegram 时，他们期望看到同一个会话、同一段上下文。

### 7.4 满意点 / 正向信号

- 社区对 Hub 多租户的期待度高（#7318 有 22 条评论且 👍 为正），说明产品方向与用户诉求吻合；
- 贡献者生态活跃，过去 24 小时有至少 3 个 `first-time-contributor` PR（[#7551](https://github.com/agentscope-ai/QwenPaw/pull/7551)、[#7564](https://github.com/agentscope-ai/QwenPaw/pull/7564)），新 Contributor 门槛较低，是项目社区健康度的正向指标；
- 维护者对 Issue 的响应和关闭速度较好，24 小时内关闭了 8 条 Issue，多数进入了修复或明确收敛状态。

## 8. 待处理积压

以下为长时间未合并/未响应的重点项，建议维护者关注：

### 长期未合并 PR

| PR | 创建时间 | 等待天数 | 说明 |
|---|---|---|---|
| [#6381 perf(drivers): avoid blocking on stale capabilities](https://github.com/agentscope-ai/QwenPaw/pull/6381) | 2026-07-23 | **44 天** | 优化 AgentBuilder 路径 Driver 发现延迟：TTL 过期后先返回快照、后台刷新。属于性能改进，与近期多个 MCP/驱动相关 Issue（#7367、#7505）有叠加效应 |
| [#6874 feat(mcp): add configurable tool call timeout](https://github.com/agentscope-ai/QwenPaw/pull/6874) | 2026-08-10 | 26 天 | 为 MCP 工具调用增加可配置超时，默认 300s。与 #7505 局域网断连重试场景直接相关 |
| [#6960 feat(pawport): third-party agent import flow](https://github.com/agentscope-ai/QwenPaw/pull/6960) | 2026-08-13 | 23 天 | 从 Codex/Qoder 等三方 Agent 导入配置/Skills/历史记录，是扩大用户迁移入口的重要功能 |
| [#7211 fix(runtime): prevent injected context from persisting](https://github.com/agentscope-ai/QwenPaw/pull/7211) | 2026-08-21 | 15 天 | 阻止 `HookContext.inject_context()` 注入的 request-local 上下文被持久化为可见用户聊天记录。涉及对话数据正确性，建议优先 Review |
| [#7401 fix(acp): prevent Windows ACP agent stalls during bootstrap](https://github.com/agentscope-ai/QwenPaw/pull/7401) | 2026-08-29 | 7 天 | Windows 上 ACP agent 在 workspace 初始化期间因同步插件加载导致事件循环冻结数分钟 |

### 值得跟踪的开放 Issue

- **[#7367](https://github.com/agentscope-ai/QwenPaw/issues/7367)（启动 30-45s 性能问题）**：已开放 8 天，仅 2 条评论。根因定位很清晰（无条件 import 18 个渠道模块），期望维护者给出 lazy-load 方案回应。
- **[#7549](https://github.com/agentscope-ai/QwenPaw/issues/7549)（Volcengine Ark API 400）**：Responses API 兼容性问题，影响中国区用户使用火山方舟，建议补齐 provider 集成测试。
- **[#7318](https://github.com/agentscope-ai/QwenPaw/issues/7318)（Hub 2.2.0 路线图讨论帖）**：已 22 条评论但尚无维护者对诉求的归类/回应，建议尽快整理成正式 Roadmap 或 Discussion 置顶帖，避免社区重复提出同类建议。

> **日报总结**：过去 24 小时没有新版本发布，但 MCP 白名单强制执行、Skill workspace 级 preload、Loop 模式一致性三项修复的关闭，使 2.2.x 在权限与基础体验上更扎实。项目最需要立即处理的是任务执行状态机的信任问题（#7559/#7567），以及多租户 Hub 上线前的基础设施讨论（#7318/#7558/#7557）的系统化收敛。

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目动态日报 — 2026-09-05

## 1. 今日速览

过去 24 小时 ZeroClaw 社区活跃度处于**高位**：共产生 34 条 Issue 更新（含 24 条新开/活跃、10 条关闭）与 50 条 PR 更新（44 条待合并、6 条已合并/关闭）。最受关注的是两条 RFC（#9487、#6909）仍在持续修订讨论，显示项目正处于重要的架构决策密集期。**没有新版本发布**，但已有 PR #10632 将工作区版本从 v0.8.4 提升至 v0.8.5，且昨日有 crates.io 发布（#10158）与 WhatsApp 移植（#10153）两个重量级 PR 合并，表明 v0.8.5 发布周期正在收尾。值得警惕的是，新报告的 #10603（OpenCode 缺少会话头）和 #10626/#10625（TTS 与媒体降级路径）反映出通道与提供商兼容层仍然存在稳定性短板。

---

## 2. 版本发布

今日无新版本发布。

**相关动态**：PR #10632（chore(release): bump version to v0.8.5）已将 23 个 crate 的版本从 0.8.4 提升至 0.8.5，并同步更新了所有安装器、容器、Nix、Tauri、工作流示例与稳定文档。该 PR 目前处于待合并状态，预示着 v0.8.5 版本即将正式发布。

🔗 https://github.com/zeroclaw-labs/zeroclaw/pull/10632

---

## 3. 项目进展

过去 24 小时有 6 条 PR 被合并或关闭，其中最值得关注的三条为：

### 3.1 crates.io 发布管道打通（#10158，已合并）
将协调的 23-crate 发布集合明确标记为可发布，同时保持维护者工具、fixtures 和桌面端私有。将 zerorelay、zeroclaw-relay-proto、zeroclaw-tls 纳入同一依赖闭包。这意味着 ZeroClaw 的 crates.io 生态发布工作正式落地。

🔗 https://github.com/zeroclaw-labs/zeroclaw/pull/10158

### 3.2 WhatsApp Web 依赖移植至 crates.io 0.7.0（#10153，已关闭/合并）
移除了六个 git 固定引用的 WhatsApp Web 依赖，改用 crates.io 的 0.7.0 版本，为 zeroclaw-channels 的发布扫清障碍。protobuf 字段已完成向 buffa MessageField 的迁移，单消息事件改为有序批次。这是 #10158 发布工作的前置依赖。

🔗 https://github.com/zeroclaw-labs/zeroclaw/pull/10153

### 3.3 依赖批量更新（#10587，已合并/关闭）
rust-all 依赖组完成 49 个 crates 的批量升级（clap 4.6.1→4.6.6、tokio 1.52.3 等），属常规依赖维护。

🔗 https://github.com/zeroclaw-labs/zeroclaw/pull/10587

**另有多个高优先级 Issue 被关闭**，包括安全相关的 #9397（WhatsApp 空 allowed_groups 视为 permit-none）、#9348（WhatsApp 业务模式回复所有群聊）以及 #10357（工具执行错误详情丢失）、#10223（ZeroCode 重连期间阻塞输入）等。

---

## 4. 社区热点

### 4.1 #9487 [RFC] Runtime-owned conversation sessions and transport surface adapters（评论 32 条）
这是过去 24 小时评论数量最高的 Issue。修订至第 5 版，是第 4 版投票快照的重大替代方案。维护者需要重新开启讨论窗口和快照后再进行投票。该 RFC 涉及运行时会话所有权和传输层适配器，是影响架构方向的重要决策。

🔗 https://github.com/zeroclaw-labs/zeroclaw/issues/9487

### 4.2 #6909 [RFC] Computer-use support for desktop screen interaction and input control（评论 16 条）
一份长期运行的 RFC（创建于 5 月 25 日），8 月 24 日维护者接手修订并做了安全澄清。社区对桌面端屏幕交互和输入控制的功能诉求强烈。此 RFC 已获得 accepted 状态，可能会在后续版本进入实现。

🔗 https://github.com/zeroclaw-labs/zeroclaw/issues/6909

### 4.3 #10050 [RFC] Verbatim channel send over the gateway, without an agent turn（评论 13 条）
建议新增一条网关路由，允许调用者通过运行中的守护进程直接发送消息，不经过 Agent 处理。当前网关有 47 个 /api/* 路径，但没有一个支持这种直发模式。背后是用户对灵活集成/自动化通道的需求。

🔗 https://github.com/zeroclaw-labs/zeroclaw/issues/10050

### 4.4 讨论热点分析
目前的社区讨论集中在 **RFC 提案与安全边界确定**两个方向。高讨论量集中在会话机制、桌面交互、通道直发等架构级能力演进上，说明社区活跃成员多是有明确集成需求的开发者。相较而言，具体 bug 类 Issue 的评论量普遍较少（多为 1-3 条），主要依赖维护者驱动推进。

---

## 5. Bug 与稳定性

过去 24 小时新报告的 Bug 按严重程度排序如下：

### 🔴 S1 - 工作流阻塞/安全风险

| Issue | 标题 | 状态 | 修复 PR |
|-------|------|------|---------|
| #10603 | OpenCode providers 从不发送 x-opencode-session 头，导致 Go 模型异常与账户风险 | OPEN（优先级 P1，2 评论） | 暂无 |
| #10593 | backup.schedule_cron 静默不生效（无 Agent 认领 __builtin_backup） | OPEN（P1，1 评论） | 暂无 |
| #9421 | 不完整的终端响应可能被报告为成功（Anthropic 提供商） | OPEN（P1，3 评论） | PR #9447（待合并） |
| #10357 | 工具执行错误路径丢弃详细错误体，Agent 只看到 "HTTP 400" | CLOSED（P1） | — |
| #10223 | ZeroCode 在活跃 turn 期间重连会丢弃 Ctrl+C 并阻塞输入 | CLOSED（P1） | — |

### 🟠 S2 - 功能降级

| Issue | 标题 | 状态 |
|-------|------|------|
| #10626 | TTS 逐字朗读 Markdown 和 emoji | OPEN（0 评论） |
| #10625 | 非视觉模型的 [media attachment] 占位符被转发给用户 | OPEN（0 评论） |
| #10594 | cron 不执行作业时无任何记录，静默不可见 | OPEN（P1，1 评论） |
| #10390 | 进入不活跃 Chat 面板阻塞 ZeroCode 导航 | CLOSED（P2） |

### 🟡 S3 - 次要问题

| Issue | 标题 | 状态 |
|-------|------|------|
| #10585 | 新日志接收器回归与迁移测试在默认并行运行器下竞争 | OPEN（P2） |

**关键观察**：
- 今日关闭的 #10357 与 #10223 均为 P1 工作流阻塞问题，说明维护者正在持续清剿高优先级 bug。
- #9421 已有对应修复 PR #9447 在等待合并，该 PR 对"不完整终端响应"进行分类修复，是 runtime 层的重要正确性改进。
- 仍无修复 PR 的 P1 问题集中在：OpenCode 提供商兼容层缺陷（#10603）、cron 静默失败（#10593、#10594）以及图像标记绕过验证的遗留安全缺陷（#9882）。

---

## 6. 功能请求与路线图信号

### 6.1 可能进入 v0.8.5 的功能
- **版本发布就绪**：PR #10632 已执行版本号提升至 v0.8.5、PR #10158 打通 crates.io 发布，二者是 v0.8.5 发货的核心前置。
- **Anthropic 提示缓存透传**（#10619）：要求 OpenAI 兼容提供商支持 Anthropic cache_control 透传。P1，已有实现方向（现硬编码为 prompt_caching: false），可能作为 v0.8.5 的功能增强纳入。

🔗 https://github.com/zeroclaw-labs/zeroclaw/issues/10619

### 6.2 路线图中的架构演进信号
- **#9487 RFC**（会话所有权与传输适配器）修订至第 5 版，是未来 runtime 架构的核心基石。
- **#6909 RFC**（桌面端 Computer-use）已 accepted，桌面自动化能力预计会进入远期规划。
- **#10330 Accepted RFC 实现索引**：追踪已接受 RFC 到实现状态的 tracker，方便社区跟踪架构演进全貌。

🔗 https://github.com/zeroclaw-labs/zeroclaw/issues/10330

### 6.3 新提出的功能请求
- **Docs 链接门禁**增强：#10580 建议链接检查从"仅新增链接"扩展为"全仓库内部链接"。
- **Docs Reference 页面补全**：#10579 报告两个 Reference 页面缺失但仍在 ToC 中，被 39 处引用。虽然标记为 docs、risk:low，但 39 处悬挂链接影响用户体验。

---

## 7. 用户反馈摘要

### 7.1 配置静默失败是最令人沮丧的问题
#10593 反馈者 JordanTheJet 指出，backup.schedule_cron 在无 Agent 认领 __builtin_backup 时**静默地不执行任何备份**，且几乎无日志线索。配置文件"看起来已开启但实际未生效"的问题，在 #9348（已关闭）中也有类似模式——配置看起来锁定了但实际完全开放。这类"静默失效"比显式报错对用户的信任伤害更大。

🔗 https://github.com/zeroclaw-labs/zeroclaw/issues/10593

### 7.2 文档维护滞后于代码
#10579 与 #10571（今日已关闭）同时指向文档建设滞后问题。#10579 指出 Reference CLI 和 Config 页面缺失，但仍在目录中且被 35+ 页面链接。用户在设计文档时对照的是 39 个悬挂链接而不是有用的参考页。

🔗 https://github.com/zeroclaw-labs/zeroclaw/issues/10579

### 7.3 本地工具行为与预期不符
#10609 反馈（singlerider）：本地启动 zerocode 会忽略启动目录并强制使用 Agent 的 workspace 作为 cwd。这影响了使用 zerocode 作为通用编辑器的场景——用户期望在哪个目录启动就在哪里工作。2 条评论，P1 级别，维护者正在关注。

🔗 https://github.com/zeroclaw-labs/zeroclaw/issues/10609

### 7.4 多媒体体验在真实部署中暴露粗糙
#10626（sebkraemer）反馈 TTS 会朗读 Markdown 标记和 emoji 名称；#10625 反馈非视觉模型时 `[media attachment]` 占位符会直接透传给终端用户。这两个问题均来自自托管部署实测，暴露了降级路径不够优雅的问题。

🔗 https://github.com/zeroclaw-labs/zeroclaw/issues/10626

---

## 8. 待处理积压

### 8.1 长时间未合并且被标记 do-not-merge 的大型 PR
| PR | 标题 | 创建时间 | 标记 | 说明 |
|----|------|---------|------|------|
| #9109 | feat(providers): add native Hailo-Ollama support | 07-17 | do-not-merge, risk:high, size:XL | 已活跃近两个月仍未合并 |
| #9419 | fix(providers): rotate live credentials after rate limits | 07-26 | do-not-merge, needs-maintainer-review, risk:high, size:XL | 凭据安全修复，等待维护者审阅 |
| #10491 | fix(plugins): read the machine's trust store for plugin HTTPS | 08-30 | do-not-merge, needs-author-action, stacked, risk:high | 堆叠 PR，等待作者行动 |

🔗 https://github.com/zeroclaw-labs/zeroclaw/pull/9109
🔗 https://github.com/zeroclaw-labs/zeroclaw/pull/9419
🔗 https://github.com/zeroclaw-labs/zeroclaw/pull/10491

### 8.2 等待作者行动的活跃 PR
| PR | 标题 | 说明 |
|----|------|------|
| #9447 | fix(anthropic): classify incomplete terminal responses | 对 #9421 的修复，size:XL，待作者回应 |
| #10407 | feat(sessions): add persistent session prompt attachments | RFC 级功能，等待作者补充 |
| #9002 | fix(gateway): keep agent turns alive after viewer disconnect | 运行 5 周，等待作者行动 |
| #10337 | fix(tools): honor allowed roots for git operations | Git 安全修复，等待作者行动 |

### 8.3 值得关注的历史遗留 PR
- #9713（feat(runtime): expose token accounting on history-trim events，blocked + do-not-merge）于 08-03 创建后被阻塞超过一个月，涉及 token 统计的可观测性改进。

### 8.4 维护者行动建议
- **高优先级**：#9487 RFC 第 5 版需要维护者开启新的讨论窗口与投票快照，该决策将影响 runtime 会话架构走向。
- **安全修复推进**：#9419（凭据轮换）、#10337（git 工具根目录限制）、#10491（HTTPS 信任库）均为 risk:high 安全类 PR，其中两个已等待近两个月，建议尽快安排 review。
- **P1 无主 Bug**：#10603（x-opencode-session 缺失）、#10593/#10594（cron 静默失败）暂无修复 PR，建议尽快认领。

---

*本日报基于 GitHub 公开数据自动生成，时间为 2026-09-05。数据源：github.com/zeroclaw-labs/zeroclaw*

</details>

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*