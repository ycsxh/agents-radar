# OpenClaw 生态日报 2026-09-06

> Issues: 500 | PRs: 500 | 覆盖项目: 13 个 | 生成时间: 2026-09-06 04:06 UTC

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

# OpenClaw 项目动态日报 — 2026-09-06

## 1. 今日速览

OpenClaw 项目今日保持高活跃度：过去 24 小时新增/更新 500 条 Issues（123 条关闭）和 500 条 PR（224 条合并/关闭），发布 v2026.9.2 版本。本次发布的 v2026.9.2 重点在于响应性能与 Gateway 稳定性优化，旨在减少聊天与仪表盘交互的阻塞问题。值得关注的是，在长期未决的 Issue 中，`clawsweeper:needs-product-decision`（等待产品决策）、`clawsweeper:needs-maintainer-review`（等待维护者审阅）与 `clawsweeper:no-new-fix-pr`（无新增修复 PR）标签以极高频率出现，标示大量高价值问题等待维护者介入，这已成为制约项目问题清理效率的主要瓶颈。同时，多条 P1/P0 级 Bug 与 `impact:crash-loop`、`impact:message-loss` 相关问题仍在积压，需警惕模型审核响应延迟加剧核心稳定性的风险。


## 2. 版本发布

### v2026.9.2
- **发布日期**: 2026-09-06
- **发布说明摘要**:
  - **Highlights**: Faster, more responsive chat — keep chat, dashboards, and session interactions responsive while long transcripts and disk usage are processed, with direct dashboard lookup, less cold-load work, and durable history reads outside the Gateway event loop. (#136862, #138...)
- **链接**: https://github.com/openclaw/openclaw/releases

该版本的核心方向是优化 Gateway 事件循环压力，针对长会话/大磁盘数据场景下 UI 卡顿问题进行了关键改进。建议用户在升级后关注长会话与仪表盘操作响应速度的改善情况。当前版本发布说明为截断状态，建议访问 Release 页面获取完整变更列表。


## 3. 项目进展

今日合并/关闭了 224 条 PR，以下为值得关注的代表性条目。同时注意，另有 276 条 PR 仍处于待合并状态，合并队列压力较大：

### 已关闭 PR
- **#139684**: refactor(android): reuse cron interval formatting — Android 端自动化间隔标签维护成本降低（代码复用，无用户侧行为变化）。
  - 链接: https://github.com/openclaw/openclaw/pull/139684
- **#139681**: improve(plugins): speed up repeated model catalog lookups — 模型目录的重复查询性能优化（每次查找都重复构建条件集），与 #139649 相关。
  - 链接: https://github.com/openclaw/openclaw/pull/139681
- **#139636**: refactor(auth): share legacy choice resolution across onboarding — 重构 onboarding 的认证选项解析逻辑，行为保留、仅清理重复代码。
  - 链接: https://github.com/openclaw/openclaw/pull/139636

### 待合并重要 PR（信号参考）
- **#139394 [P0]**: fix(channels): recover queued messages after timeouts — 修复消息在 "busy turn" 后因五分钟超时被丢弃的问题（关闭 #139341）。目前已具备初步 proof，处于待合入状态。
  - 链接: https://github.com/openclaw/openclaw/pull/139394
- **#139685 [P1]**: fix(acp): preserve queued output and cancel failed deliveries — 修复 ACP 输出丢失，当完成信号超过慢速投递时不会丢弃已累积输出。
  - 链接: https://github.com/openclaw/openclaw/pull/139685
- **#139614 [P1]**: fix(feishu): stop dead-stream log flooding + non-streaming card fallback — 飞书流式卡片在死流后不再刷屏报错，并增加非流式卡片兜底（关闭 #139443）。
  - 链接: https://github.com/openclaw/openclaw/pull/139614

从 PR 活跃度与问题的分布可以看出，插件/渠道层稳定性（飞书、Telegram、Google Chat、WebChat）与模型/工具执行链路仍是当前社区贡献的重点区域。若 276 条待合并 PR 能得到及时审核，预计下一阶段版本将有显著的功能密度提升。


## 4. 社区热点

### 最热 Issue 分析

**#69208 — Umbrella: duplicate transcript, replay, and context assembly across channels（14 评论，P1）**
- 链接: https://github.com/openclaw/openclaw/issues/69208
- 摘要：这是一个跨渠道（MSTeams、webchat、Telegram 等）的持久性 Bug 总集，横跨多个子系统，集中在 duplicate transcript 和重复消息上。已被标记 `clawsweeper:needs-product-decision`。
- 社区诉求：多渠道重复消息问题长期存在且影响范围广，社区期待一个统一的根因级修复而非逐渠道打补丁。

**#132762 — overflow retry can end successfully on a tool result without final delivery（13 评论，P1）**
- 链接: https://github.com/openclaw/openclaw/issues/132762
- 摘要：多阶段文档任务中 overflow-retry 成功结束但最终没有 assistant 回复，用户只拿到一个 `toolResult` 作为末尾，发消息丢失。
- 社区诉求：对 retry 语义的正确性提出明确期待，重试成功不等于最终交付完成。

**#53408 — Write/exec tool parameters silently dropped after long conversations（12 评论，P2）**
- 链接: https://github.com/openclaw/openclaw/issues/53408
- 摘要：长对话（15+ turns）后 `write` 与 `exec` 工具参数被静默丢弃，导致工具调用无参数返回。获得 2 👍。
- 社区诉求：工具调用的参数完整性不应随上下文长度而劣化。该问题是长会话可靠性隐患，用户希望获得明确的临时规避手段。

**#53763 — [Feature]: Built-in headless browser for reliable web access without external dependencies（12 评论，P3）**
- 链接: https://github.com/openclaw/openclaw/issues/53763
- 摘要：建议打包无头 Chromium 到 OpenClaw 作为内置工具，不依赖用户本地 Chrome。
- 社区诉求：Agent 网页访问路径应开箱即用，尤其对需 JS 渲染和登录的页面，当前过度依赖脆弱的第三方桥接。

**#39476 — A2A sessions_send: target agent can call sessions_send back, causing duplicate messages（12 评论，P1）**
- 链接: https://github.com/openclaw/openclaw/issues/39476
- 摘要：Agent 间通信中回拨导致重复消息。已有 `clawsweeper:linked-pr-open` 标记，意味着修复 PR 在途。
- 社区诉求：Agent 间消息传递机制需要端到端的消息去重协议，而不是仅靠人工约束工具使用方式。

**#97616 — OpenClaw leaks unreaped hook/tool child processes（11 评论，P1）**
- 链接: https://github.com/openclaw/openclaw/issues/97616
- 摘要：进程泄漏导致僵尸进程积累与运行时降级，影响生产部署。标有 `impact:crash-loop`。
- 社区诉求：进程生命周期管理需要更严格的可观测性和兜底回收机制，而不仅依赖外部 supervisor。

此外，**#91931 [P0]**（preseeded 的 SOUL/IDENTITY/USER.md 导致 BOOTSTRAP.md 被删除）、**#115642 [P0]**（订阅模式冷却时间过长）、**#136148 [P0]**（Linux 桌面 App 崩溃）等 P0 问题也在今日产生了活跃反馈，表明发布版本中的关键回归对用户影响较大。

### 今日 PR 侧热区
新提交的 PR 集中在 **steipete** 的"大规模清理/稳定性"系列（#139573、#139701、#139705、#139421、#139657），以及 **gaoanze888** 的 Feishu 稳定性修复（#139614），显示社区正在主动对架构复杂度做减法。


## 5. Bug 与稳定性

### 传播范围广且尚未修复的高风险问题

| 问题 | 严重度 | 状态 | 摘要 | 链接 |
|---|---|---|---|---|
| #110190 Runtime context carrier 位置导致模型困惑与推理 token 浪费 | P1 | 无新增 fix PR | ~15K 字符的 context carrier 被放在用户消息后，导致模型严重困惑 | [Issue](https://github.com/openclaw/openclaw/issues/110190) |
| #119720 同步持久化/transcript 维护阻塞 Gateway 事件循环 | P1 | 无新增 fix PR | 群体规模扩大后主线程阻塞，影响整个 Gateway | [Issue](https://github.com/openclaw/openclaw/issues/119720) |
| #97616 Hook/tool 子进程未回收（zombie 积累） | P1 | 无新增 fix PR | 进程泄漏导致运行时降级，用户报告生产受影响 | [Issue](https://github.com/openclaw/openclaw/issues/97616) |
| #114967 agent 触发的 live update 导致 launchctl 强制重启循环 | P1 | 无新增 fix PR | 每 2 分钟一次强制 Gateway 重启 | [Issue](https://github.com/openclaw/openclaw/issues/114967) |
| #99910 Memory dreaming 占满 Gateway 主线程约 10 分钟 | P1 | 无新增 fix PR | 记忆系统无输出、CLI/RPC 无响应、渠道断开 | [Issue](https://github.com/openclaw/openclaw/issues/99910) |
| #136182 作为 bug/regression 报告的 ssh banner 挂起 | P1 | 无新增 fix PR | 命令执行器在 ssh 启动时无限等待 SIGTERM | [Issue](https://github.com/openclaw/openclaw/issues/136183) |
| #132762 overflow retry 最后交付丢失 | P1 | 无新增 fix PR | 用户只见 toolResult 无后续 assistant 消息 | [Issue](https://github.com/openclaw/openclaw/issues/132762) |
| #84110 Codex prompt 重写破坏缓存 | P2 | 无新增 fix PR | 缓存命中率从 93% → 47%，推理成本上升 | [Issue](https://github.com/openclaw/openclaw/issues/84110) |
| #44134 Google Antigravity 因 schema 频繁刷新被误封 | P2 | 无新增 fix PR | 触发 false-positive abuse 检测并封号 | [Issue](https://github.com/openclaw/openclaw/issues/44134) |

### 关键趋势
- **Gateway 主线程阻塞是核心主题**：多个 P1 问题（#119720、#99910、#53008、#72015）指向单一瓶颈 —— 大量的同步操作在 Gateway 事件循环中串行执行。v2026.9.2 已开始针对此问题优化。
- **message-loss 高发**：大量 P1 Issue 包含 `impact:message-loss` 标签。这类 Bug（#132762、#112259、#119992、#39476、#132765、#89430）直接影响用户对 AI 助手的信任感。
- **session-state 与 crash-loop 持续堆积**：会话状态被污染、进程崩溃循环等问题，在自动化/多方 Agent 协作频繁的场景中尤为严重。已有 PR 在途的有 #39476（linked-pr-open）。


## 6. 功能请求与路线图信号

### 高讨论度/高优先级功能诉求

- **#53763** — 内置 Headless 浏览器（P3, 12 评论）；[链接](https://github.com/openclaw/openclaw/issues/53763)
- **#96975** — 默认隔离 Subagent 完成内容，避免污染父上下文（P2, 12 评论）；[链接](https://github.com/openclaw/openclaw/issues/96975)
- **#14785** — 降低工具 schema token 开销，预估每次会话平均可节省约 3,500 token（P2, 10 评论）；[链接](https://github.com/openclaw/openclaw/issues/14785)
- **#6599** — 增加 `/models test-fallback` 命令验证 fallback 链路（P3, 11 评论）；[链接](https://github.com/openclaw/openclaw/issues/6599)
- **#58057** — 动态身份解析 allowlists（P2，与多用户部署场景相关）；[链接](https://github.com/openclaw/openclaw/issues/58057)

### 合并 PR 后可能进入下一版本的功能线索

- **#135599** — "feat: manage and reload plugins without restarting the Gateway"。这是一个超大型（XL）PR，涉及 CLI/RPC/Control UI/Agent 操作侧的全量插件热管理，可能成为下一版本的功能亮点；[链接](https://github.com/openclaw/openclaw/pull/135599)
- **#138900** — "feat: start cloud sessions without a Gateway checkout"，使 Repository cloud sessions 不再需要 Gateway 克隆/工作树，属于云部署链路的重要架构简化；该 PR 标有 `merge-risk` 警告且目前状态为"needs proof"；[链接](https://github.com/openclaw/openclaw/pull/138900)
- **#116716** — 为 context engine 增加可选的 strict failure policy（避免静默降级到 legacy 引擎）；[链接](https://github.com/openclaw/openclaw/issues/116716)

### 综合判断
社区对"降低隐形成本"（token 开销、内存占用、进程泄漏）和"架构简化"（插件热管理、远程/session 生命周期解耦）的诉求正变得非常明确。这些方向大概率在 2026.9.x 与 2026.10.x 版本中占据主导。


## 7. 用户反馈摘要

> 以下观点均提炼自上述 Issues 评论中的真实用户声音。

- **长会话稳定性是最大痛点**：多个用户在 #53408 和 #110190 中报告长对话后行为异常。用户原话中的核心诉求："问题不在于工具无法工作，而是它静默地丢失参数且没有报错——这会让人对 Agent 产生不信任。" #53408 收获 2 👍，侧面印证了影响面。

- **被"静默吞消息"困扰**：#112259 用户描述了一个场景——消息被接受但不运行 Agent、"无人处理也不报错"。这种 0 反馈失败模式让用户感到"对话直接掉入黑洞"。该情绪与 #132762、#119992 高度共振。

- **对 Gateway 卡死的挫败感强**：#53008（memory compaction 阻塞 10 分钟）与 #99910（memory dream 锁死 10 分钟）的评论中，用户明确表示在自托管/生产场景下不可接受。一名用户指出需要一个 watchdog 来"从外部杀死进程"——外部看门狗才恢复了可用性。

- **对产品决策缓慢不满**：#69242（exec Linux 偶发 SIGKILL）、#136183（ssh 挂起回归）等 Bug 已打开多日但未见 fix PR。用户在 #136183 中确认该回归是 "regression in 2026.8.1, persists in 2026.8.2"，带有明显的"一周内发布两个版本仍未修复"的沮丧感。

- **对子代理内容污染的关切上升**：#96975 与 #78055 收到的评论与 👍 说明使用 subagent 做并行的用户数量正在增加，当 subagent 的完整输出被注入 parent 上下文时会导致 token 膨胀与决策混乱。


## 8. 待处理积压

### 长期未决且值得维护者优先关注

以下问题大多创建超过一个月、带有 `clawsweeper:no-new-fix-pr` 标识、且包含 `clawsweeper:needs-maintainer-review` 或 `clawsweeper:needs-product-decision` 标签。这意味着它们已完成分类、等待决策/审核，但因维护者资源有限或需要产品定义，一直未能向修复 PR 推进。

| 高优先级（P0/P1） | 创建 | 标签 | 描述 | 链接 |
|---|---|---|---|---|
| #115642 | 2026-07-29 | P0, needs-product-decision | Billing cooldown 5h 过长且无法自动恢复，用户被锁在订阅之外 | [Issue](https://github.com/openclaw/openclaw/issues/115642) |
| #91931 | 2026-06-10 | P0, linked-pr-open | Preseeded SOUL/IDENTITY 导致 BOOTSTRAP.md 在首次运行前被自动删除 | [Issue](https://github.com/openclaw/openclaw/issues/91931) |
| #119720 | 2026-08-05 | P1, needs-maintainer-review | Gateway 事件循环阻塞问题（虽然已有部分修复落地，但主问题未关闭） | [Issue](https://github.com/openclaw/openclaw/issues/119720) |
| #110190 | 2026-07-17 | P1, needs-product-decision | Runtime context carrier 位置策略 | [Issue](https://github.com/openclaw/openclaw/issues/110190) |
| #78055 | 2026-05-05 | P1, needs-product-decision | Subagent 陈旧输出投递问题 | [Issue](https://github.com/openclaw/openclaw/issues/78055) |
| #72015 | 2026-04-26 | P1, needs-maintainer-review | active-memory 阻塞回复 + QMD 启动过载 | [Issue](https://github.com/openclaw/openclaw/issues/72015) |
| #99910 | 2026-07-04 | P1, needs-maintainer-review | Memory dreaming 占满 Gateway 事件循环 | [Issue](https://github.com/openclaw/openclaw/issues/99910) |
| #90098 | 2026-06-04 | P1, linked-pr-open | Control UI 大 PDF 导致栈溢出 | [Issue](https://github.com/openclaw/openclaw/issues/90098) |
| #114967 | 2026-07-28 | P1, needs-maintainer-review | launchctl 强制重启循环 | [Issue](https://github.com/openclaw/openclaw/issues/114967) |
| #124133 | 2026-08-15 | P1, beta-blocker, needs-live-repro | snowluma 插件 `formatInboundEnvelope is not a function` | [Issue](https://github.com/openclaw/openclaw/issues/124133) |
| #136183 | 2026-09-02 | P1, needs-maintainer-review | ssh 命令执行器 SIGTERM 挂起 | [Issue](https://github.com/openclaw/openclaw/issues/136183) |

| 长期未决（P2+/旧 Issue） | 创建 | 标签 | 描述 | 链接 |
|---|---|---|---|---|
| #53408 | 2026-03-24 | P2 | Write/exec 工具参数长会话后丢失 | [Issue](https://github.com/openclaw/openclaw/issues/53408) |
| #53763 | 2026-03-24 | P3, needs-product-decision | 内置 headless 浏览器 | [Issue](https://github.com/openclaw/openclaw/issues/53763) |
| #6599 | 2026-02-01 | P3, needs-product-decision | `/models test-fallback` 命令 | [Issue](https://github.com/openclaw/openclaw/issues/6599) |
| #14785 | 2026-02-12 | P2, needs-product-decision | 降低 tool schema token 开销 | [Issue](https://github.com/openclaw/openclaw/issues/14785) |
| #58057 | 2026-03-31 | P2, needs-product-decision | 动态 allowlist 身份解析 | [Issue](https://github.com/openclaw/openclaw/issues/58057) |
| #44130 | 2026-03-12 | P2 | TUI 滚动跳动问题仍未解决 | [Issue](https://github.com/openclaw/openclaw/issues/44130) |
| #69208 | 2026-04-20 | P1, Umbrella | 跨渠道重复 transcript/重放问题总集 | [Issue](https://github.com/openclaw/openclaw/issues/69208) |

| 长期未合入 PR 示例 | 创建 | 状态 | 描述 | 链接 |
|---|---|---|---|---|
| #132491 | 2026-08-29 | 👀 ready for maintainer look | visitor-access 插件外部 JSON 响应边界限制，等待审核 | [PR](https://github.com/openclaw/openclaw/pull/132491) |
| #102182 | 2026-07-08 | 📣 needs proof | Control UI 流式增量输出，提交至今近两个月未有 proof | [PR](https://github.com/openclaw/openclaw/pull/102182) |
| #120248 | 2026-08-07 | ⏳ waiting on author | Amazon Bedrock 流式工具参数解析性能优化，等待作者回复 | [PR](https://github.com/openclaw/openclaw/pull/120248) |
| #137381 | 2026-09-03 | 👀 ready for maintainer look | sessions_yield 大事务历史清理导致短时不可用修复，暂未合入 | [PR](https://github.com/openclaw/openclaw/pull/137381) |

---

**总结**：OpenClaw 项目保持高速迭代态势（每日约 500 条 Issue/PR 动态），但项目产出受限于维护者 review 队列。v2026.9.2 标志对 Gateway 性能和体验优化的正式发力，然而大量 P0/P1 修复仍处于排队中，历史 Issue 中「等待产品决策/等待维护者审阅」的积压数量持续攀升，可能成为下一阶段的首要瓶颈。建议维护团队优先关注：a) #69208 Umbrella 级别问题的统一修复策略；b) 高优先级 message-loss 及 crash-loop 类问题的批量清理；c) 订阅类/会话类 P0 的快速响应。

---

## 横向生态对比

# 个人 AI 助手 / 自主智能体开源生态横向对比报告

**统计窗口**：2026-09-05 ~ 2026-09-06  
**数据口径**：基于各仓库 GitHub Issues / PRs / Releases 动态快照

---

## 1. 生态全景

当前个人 AI 助手开源生态已从“模型能力展示”阶段切入“**7×24 小时自托管服务的工程化**”阶段：头部项目 OpenClaw 单日动态超过千条（500 条 Issue + 500 条 PR）并保持例行发版，ZeroClaw、Hermes Agent、NanoBot、CoPaw 等也在各自细分方向快速推进。但全生态共同面临的已不是“模型是否聪明”，而是基础设施级可靠性：Gateway/事件循环被同步任务阻塞、消息“被吞”或重复、子进程泄漏、长会话后工具参数静默丢失、凭据与配置文件安全边界不清晰。这些问题在多个项目中高度同构，说明 Agent 框架正处于当年 Web 框架经历过的“工程化补课”阶段。长尾端还有大量以 “Claw” 命名、面向特定场景的轻量实现，但多数活跃度低、维护带宽不足，生态呈“**一超数强 + 长尾孵化**”的格局。

---

## 2. 各项目活跃度对比

| 项目 | Issue 24h | PR 24h | Release | 健康度评估 |
|---|---|---|---|---|
| **OpenClaw** | 500 更新（123 关闭） | 500 更新（224 合并/关闭；276 待合） | **v2026.9.2** | 极高活跃、例行发布；瓶颈在维护者评审与产品决策积压，P0/P1 消息丢失类问题堆积 |
| **ZeroClaw** | 42 更新（8 关闭） | 50 更新（6 合并/关闭；44 待合） | **v0.8.5** | 高活跃；454 commits / 73 贡献者，RFC 架构讨论密集，但大型 PR 合并偏慢 |
| **Hermes Agent** | 50 更新（2 关闭） | 50 更新（2 合并/关闭） | — | 高提交、修复冲刺；更新链路反复回归 + 自动化 Issue 噪音是主要减分项 |
| **NanoBot** | 1 新 Issue | 25 更新（7 合并/关闭；18 待合） | — | 中高活跃；新 Bug 响应快（#5674→#5675 当日闭环），但 PR conflict 积压明显 |
| **CoPaw** | 11 更新（3 关闭） | 7 更新（0 合并） | — | 中高活跃；外部新贡献者跨模块提交，0 合入意味着评审是唯一瓶颈 |
| **ZeptoClaw** | 14 更新（4 关闭） | 8 更新（3 合并/关闭） | — | 中高活跃；安全修复同日闭环效率高，但依赖机器人 PR 滞留 3 个月+ |
| **NanoClaw** | 0 | 3 待合 | — | 低活跃但有实质待合改进；合并节奏偏慢 |
| **PicoClaw** | 0 | 3 个批量 PR 关闭 | — | 存量维护；PR 处置周期长达约 175 天，响应能力弱 |
| **IronClaw** | 1 新 Issue | 1 个 XL PR 待合 | — | 低活跃但路线图清晰（#7908 → #8075 依赖链） |
| **LobsterAI** | 0 | 0（2 个 stale PR 开放 5 个月+） | — | 维护静默期；stale PR 需要明确去留 |
| **NullClaw / TinyClaw / Moltis** | 无 | 无 | — | 休眠（24 小时无活动） |

**总体判断**：OpenClaw 的 Issue/PR 体量约为第二梯队（ZeroClaw、Hermes）的 10 倍，构成生态绝对核心。ZeroClaw、Hermes Agent、NanoBot、CoPaw、ZeptoClaw 处于“有持续社区输入但仍未跑通稳定合并节奏”的阶段；其余项目更多是场景化、个人维护或孵化状态。

---

## 3. OpenClaw 在生态中的定位

**社区规模与影响力：无可争议的中心参照。** 单日 224 个 PR 合并/关闭，超过其他所有活跃项目合并量总和；v2026.9.2 已形成稳定的“优化 Gateway → 发版 → 收集反馈”节奏。其 Issue 池覆盖渠道、插件、模型目录、认证重构、memory/context 等全栈话题，这种“广度本身”就是生态价值：几乎每个细分问题都能在 OpenClaw 找到原型案例，因此其他 Claw 家族项目普遍会以 OpenClaw 的问题清单作为自身架构设计的对照输入。

**技术路线差异：中心化 Gateway + 事件循环。** OpenClaw 当前的核心矛盾是历史持久化、memory 维护等同步任务对 Gateway 事件循环的压力（#119720、#99910），v2026.9.2 正是将 durable history reads 移出事件循环、减少冷启动加载。对比同类：

- **Hermes Agent** 更强调桌面端 + 后台 Gateway 的进程分离、launchd 生命周期管理和安装/更新链路一致性；
- **NanoBot** 走轻量 MessageBus + 事件驱动路线，把 session 持久化移到独立 IO 适配器；
- **ZeroClaw** 则通过 RFC 推动“运行时拥有会话 / 事件溯源重放”的彻底架构重构；
- **ZeptoClaw** 以 Rust + 安全默认值作为差异化，直接对子进程环境变量和非法配置做 fail-closed。

也就是说，**OpenClaw 的优势在于“先遇到问题、先形成平台级解法”**，但其高度中心化的事件循环也让单点瓶颈更突出。v2026.9.2 的发版方向说明团队已意识到这一点。

**潜在风险**：大量 P0/P1 修复（#139394、#139685、#139614）和跨渠道 Umbrella Issue（#69208）仍卡在 review 队列，276 个待合入 PR 和 500 个每日 Issue 会让社区贡献者的等待成本持续上升。若产品决策/维护者评审速度跟不上提交速度，OpenClaw 的“生态基准”地位会被 ZeroClaw、Hermes 这类治理更轻快的项目分流。

---

## 4. 多项目共同关注的技术方向

### ① 事件循环卸载与持久化异步化
- **OpenClaw**：#119720（同步持久化阻塞 Gateway）、#99910（memory dreaming 锁死主线程），v2026.9.2 已开始修复；
- **NanoBot**：#5580 将 session 加载/保存/checkpoint 移出事件循环；
- **Hermes Agent**：#23717（可插拔 SessionDB，回应 SQLite 写入导致的热更新“死亡螺旋”）；
- **ZeroClaw**：#9487（Runtime-owned conversation sessions）、#10526（append-only session event history + deterministic replay）。

**共同诉求**：Agent 运行时不能再让历史读写、记忆维护、状态压缩占用交互主路径；状态层必须异步化、可插拔、可重放。

### ② 消息交付契约：不静默丢、不重复、必须有最终回复
- **OpenClaw**：#132762（overflow-retry 成功却以 toolResult 结尾，用户收不到最终 assistant 消息）、#69208（跨渠道重复 transcript/replay Umbrella）；
- **NanoBot**：#5674（NIM 超时文本被当作 model output，Agent 永久停摆）、#5589（被丢弃的 session 可能“复活”并向全局总线发消息）；
- **CoPaw**：#7559（任务运行中提交新消息返回 409，用户预期是进入消息队列而非拒绝）；
- **PicoClaw**：#3287（IRC 自动拆分的长消息需要语义重组，而不是当作多条独立 prompt）。

**共同诉求**：消息系统必须定义明确的“端到端交付”语义——要么排队、要么明确失败，不能无声消失，也不能让工具结果代替最终回复。

### ③ 进程生命周期与安全默认值
- **OpenClaw**：#97616（hook/tool 子进程未回收，僵尸进程积累）；
- **ZeptoClaw**：#659（非法 `agent_mode` 回退到 Autonomous，权限升级隐患）、#660（子进程环境变量未清理）——均为 P0 且已关闭；
- **Hermes Agent**：#102193（重复出现的 root-owned 文件）、#104022（macOS LaunchAgent 生命周期管理）；
- **NanoBot**：#5633（session key 路径穿越）；
- **ZeroClaw**：#10536（macOS Seatbelt 忽略 `allowed_roots`）。

**共同诉求**：Agent 生成代码/命令/子进程时，应默认继承最小环境、fail-closed、超时回收进程树；配置解析错误时向低权限方向降级，而不是向高权限方向回退。

### ④ Token / 上下文成本控制与 prompt-cache 友好性
- **OpenClaw**：#110190（约 15K 字符 context carrier 位置导致模型困惑）、#14785（工具 schema 平均每会话可省约 3,500 token）、#84110（Codex prompt 重写致缓存命中率 93%→47%）；
- **ZeptoClaw**：#661（Byte-stable Prompt Envelope，系统提示词每轮重建对 prompt cache 极不友好，定位为最大架构级性能缺口）；
- **NanoBot**：#5386（MCP Apps 的富元数据不应进入模型上下文）；
- **Hermes Agent**：#72200（聊天侧 skills catalog 压缩）；
- **OpenClaw** #96975 / **Hermes** 相关讨论：#96975（subagent 完整输出污染 parent 上下文）。

**共同诉求**：把“成本”作为一等架构约束——system prompt 要字节级稳定以命中缓存，工具定义要精简，子代理/中间结果要默认隔离。

### ⑤ 渠道层从“可用”到“得体”：多语言、多端状态一致性
- **OpenClaw**：#139614（飞书死流日志刷屏 + 非流式卡片兜底）；
- **Hermes Agent**：#103893（德语填充词 “halt” 误触发群聊暂停——多语言回归）、#103900（Desktop pin 不写回 canonical session-store）；
- **CoPaw**：#7570（飞书长思维链卡片需自动折叠）、#7547（飞书队列消费者卡死）；
- **NanoClaw**：#3725（signal-cli 固定到 0.14.3 导致发给新联系人永久挂起）；
- **ZeroClaw**：#10625（非视觉模型直接向用户展示 `[media attachment]` 字面量）。

**共同诉求**：渠道适配器不只是“收发消息”，还要处理死流、消息拆分/重组、富文本降级、本地化语义和 UI 状态与 canonical state 的一致性。

---

## 5. 差异化定位分析

| 项目 | 目标用户与场景 | 关键架构 / 功能差异 |
|---|---|---|
| **OpenClaw** | 全场景个人 AI 助手；跨渠道（Telegram/飞书/WebChat 等）重度用户与 Agent 应用开发者 | 中心化 Gateway 事件循环 + channel 适配器 + memory/dreaming + 上下文引擎 + ACP/A2A 跨 Agent 协议；生态广度最大 |
| **Hermes Agent** | 自托管用户、桌面端 + 后台常驻 Bot、多账号/群聊场景 | Desktop App + Gateway/runner 进程分离，重视安装/更新链路（ZIP、launchd、E2E 测试矩阵）、可插拔 SessionDB 方向 |
| **ZeroClaw** | 团队 / 企业级部署、对安全与架构演进敏感的用户 | RFC 驱动的治理文化；v0.8.5 引入 ZeroRelay/ZeroRouter、WASM 插件系统、PKCE；向事件溯源、运行时自有会话演进 |
| **NanoBot** | 开发者用户，构建自定义工作流、CI/监控/计费通知等 | 轻量 MessageBus 架构 + CLI/WebUI/SDK；重视 session 安全、模型 failover、MCP Apps 元数据隔离 |
| **CoPaw** | 飞书/console 工作区用户，中文企业场景 | Workspace + Skill v2 + 多租户 Hub 方向；针对飞书流式卡片、队列消费做深度优化 |
| **ZeptoClaw** | Rust / 安全合规偏好者；单维护者主导的审计型项目 | Rust 实现，cargo-deny/Clippy 纳入 CI；环境变量擦除、非法配置 fail-closed、超时进程树回收 |
| **IronClaw** | 基准测试 / 沙箱场景（NEAR 生态） | 嵌入式 Pi 沙箱循环作为启动默认，强调开箱即用的可复现运行环境 |
| **PicoClaw** | IRC 深度用户、轻量部署 | 面向 IRC 512 字节行限制的协议适配；需长消息语义重组 |
| **NanoClaw** | Signal 渠道个人用户 | 聚焦 signal-cli 安装可靠性、skill 示例正确性 |
| **LobsterAI** | 桌面端 CoWork 场景（网易有道） | 2100+ 行单文件组件重构；per-session MCP 开关控制 |

**关键判断**：这些项目虽然共享“Claw”命名和大量相似 Issue 类型，但目标用户与技术路径已明显分化——有的做“个人助理全家桶”（OpenClaw），有的做“团队级安全平台”（ZeroClaw），有的做“开发者嵌入式框架”（NanoBot），有的做“企业 IM 工作台”（CoPaw），有的做“IRL/IRC 极简场景”（PicoClaw）。对技术决策者而言，选型时应先确认自己需要的是**渠道广度、团队治理、协议抽象、安全默认值，还是部署轻量性**，而不是单纯比较 GitHub Star 数。

---

## 6. 社区热度与成熟度分层

### Tier 1：快速迭代期（提交密集、版本持续输出、但 review 积压）

- **OpenClaw**：单日 ~1000 动态，v2026.9.2 发布；问题标签体系发达（clawsweeper 系列），但维护者队列是硬瓶颈。
- **ZeroClaw**：v0.8.5 发布（454 commits / 73 贡献者），RFC 讨论质量高；问题是“决策重、大 PR 合并慢”（#9487、#9488 反复修订）。
- **Hermes Agent**：Issue/PR 各 50 条左右，修复提交密集；但更新链路回归、依赖陈旧与自动化噪音（#66616 163 条评论）说明“发布工程成熟度”与“迭代速度”尚未匹配。

### Tier 2：质量巩固期（Bug 闭环快、外部贡献开始进入、但尚未形成稳定发版节奏）

- **NanoBot**：NIM Bug（#5674）当日即出现修复 PR #5675；但大量 PR 带 conflict 标记，需要集中解决。
- **CoPaw**：外部新贡献者（kabishou11）覆盖渠道/console/工具链多个模块，Issue 与 PR 形成良好映射（#7559↔#7577、#7572↔#7578）；但当日 0 合入，需警惕新贡献者流失。
- **ZeptoClaw**：P0 安全修复同日关闭，响应纪律好；但整体仍属“主导者驱动”，外部生态尚未形成。

### Tier 3：孵化 / 维护期（有明确方向，但社区输入弱、响应周期以周/月计）

- **IronClaw**（核心团队推进 sandbox 默认化）、**NanoClaw**（3 个低风险 PR 等合入）、**PicoClaw**（175 天关闭批量 PR）、**LobsterAI**（2 个 5 个月 stale PR）。

### Tier 4：休眠

- **NullClaw / TinyClaw / Moltis**：24 小时无任何动态。

**成熟度信号**：真正区分“快速迭代”与“健康迭代”的不是 PR 提交数，而是 **Issue→PR→Merge 的闭环周期**。OpenClaw 与 Hermes 的瓶颈都出现在闭环末端的维护者评审；ZeptoClaw 的高效说明“小团队 + 明确安全优先级 + 自动化检查”仍是一种可复制的健康形态。

---

## 7. 值得关注的趋势信号

### ① “最后一次消息必须是 agent 的最终回复”正在成为正确性原语
OpenClaw #132762 与 NanoBot #5674 共同揭示：当前 Agent 框架把“工具执行成功”误当作“任务交付成功”，导致用户只看到 toolResult 或超时文本，收不到最终 assistant 消息。**对开发者的启示**：在设计编排层时，应把“最终交付消息”作为独立于工具调用的协议状态；retry/failover 只能发生在最终交付之前，不能替代最终交付。

### ② Agent 正在从“函数调用框架”变成“需要运维的长期服务”
僵尸进程（OpenClaw #97616）、事件循环卡死（OpenClaw #99910、NanoBot #5580）、launchd 看门狗误杀（Hermes #97394）、macOS Seatbelt 限制（ZeptoClaw / ZeroClaw）——这些已不是传统分布式系统的专属问题，而是单机 Agent 的日常运维命题。**对开发者的启示**：Agent 产品上线前就应配套 watchdog、进程组超时回收、健康检查与启动配置管理，而不是事后补。

### ③ 成本压力已从“模型选型”下沉到“系统提示词与缓存架构”
ZeptoClaw #661 将“系统提示词字节级稳定”列为最大架构性能缺口，OpenClaw #84110 展示了一次 prompt 重写如何让缓存命中率从 93% 掉到 47%。**对开发者的启示**：工具 schema、context carrier、身份文件的位置与稳定性，会直接决定长会话的推理成本和延迟；应像管理 API 兼容性一样管理 prompt 缓存友好性。

### ④ 安全默认值进入“fail-closed”时代
ZeptoClaw #659（非法 `agent_mode` 不再回退到最高权限）、NanoBot #5633（session key 路径穿越）、Hermes #103989（OAuth 凭据假成功）共同指向一个原则：**配置不确定时向低权限失败，凭据未落盘时明确报错，而不是静默成功**。“假成功”对用户信任的伤害比显式报错更大。

### ⑤ 社区治理正在成为生态健康的“最后一公里”
OpenClaw 每日 500 条 Issue、Hermes 自动化探针刷出 163 条评论噪音、NanoBot 大量 conflict PR、ZeroClaw 提出简化 RFC 投票（#10549）——所有这些都在说同一件事：**Agent 开源项目的增长瓶颈不再是代码，而是 issue triage、PR 冲突解决和决策记录**。**对开发者的启示**：如果要在该生态中建立项目，早期就要配置标签机器人、明确的 review SLA、定期 conflict 清理和公开 roadmap，否则社区贡献者会因等待而流失。

---

**结论**：个人 AI 助手开源生态正处于“功能过剩、可靠性不足”的转折点。OpenClaw 以绝对体量维持生态中心地位，但其维护者评审队列已成为系统性风险；ZeroClaw、Hermes、NanoBot、CoPaw、ZeptoClaw 等第二梯队正在事件循环卸载、消息交付语义、进程安全、Token 成本与渠道体验等方向快速补课。对技术决策者，建议将**“长会话稳定性 + 消息交付闭环 + 安全默认值 + 维护者响应速度”**作为项目选型与投入判断的首要指标。

---

## 同赛道项目详细报告

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

# NanoBot 项目动态日报

**日期**：2026-09-06  
**数据窗口**：2026-09-05 ~ 2026-09-06  
**数据来源**：HKUDS/nanobot GitHub Issues / PR / Releases

---

## 1. 今日速览

过去 24 小时 NanoBot 项目整体活跃度较高：新增 1 个 Bug Issue，PR 更新达 25 条，其中 7 条已关闭/合并，另有 18 条处于待合并状态；但今日无新版本发布。值得关注的是，新报告的问题 #5674（Nvidia NIM 错误导致 Agent 停止工作）在当日即出现了针对性的修复 PR #5675，体现了较快的社区响应速度。与此同时，PR 队列中长期积压且带有 conflict 标记的 PR 仍占相当比例，合并效率值得关注。整体看，项目正处于功能与修复密集提交、但审查合并带宽相对吃紧的阶段。

---

## 2. 版本发布

无新版本发布。

---

## 3. 项目进展

今日共有 7 个 PR 处于“已合并/关闭”状态。结合可查到的关闭 PR 列表，主要体现在以下方面：

- **WebUI 开发模式修复**（#5671，已关闭）：修复 `nanobot webui --dev` 模式下仍会执行生产 bundle 新鲜度检查而误报警告的问题，改善本地开发体验。  
  https://github.com/HKUDS/nanobot/pull/5671

- **测试清理**（#5672，已关闭）：移除仅断言“已删除或不存在的符号、字段、路由”的过时测试，同时保留可观察行为与安全相关回归覆盖，提升测试套件质量。  
  https://github.com/HKUDS/nanobot/pull/5672

- **事件通知系统重构**（#5670，已关闭）：尝试统一使用 MessageBus 处理本地事件订阅与通道消息投递，并将上下文压缩（context compaction）迁移到统一机制，保持线上 wire payload 兼容。该 PR 较为庞大，最终被关闭，可能需拆分后重新提交。  
  https://github.com/HKUDS/nanobot/pull/5670

由于大量功能类 PR 仍处于待合并状态，今日项目整体“向前推进”主要体现在工程质量维护与开发工具链修复，而非面向用户的新功能落地。

---

## 4. 社区热点

本次窗口内数据未提供评论数，以下结合 PR/Issue 内容与关联关系分析讨论热度与社区诉求：

- **#5675 “allow model failover after runner deadlines”**（OPEN，疑似 #5674 的修复 PR）：由 be-student 提交，直接关联今日唯一的新 Issue #5674。该 PR 定位到根因是“主模型挂起耗尽 runner deadline，导致 `FallbackProvider` 还未看到超时，runner 就取消了整条链路”，并给出了复现说明。Bug + 快速修复的组合通常构成社区讨论集中的焦点。  
  https://github.com/HKUDS/nanobot/pull/5675

- **#5676 “add attach-only Desktop target selection”**（OPEN）：同样是今日新提交的 PR，面向 CLI 交互体验，解决 Desktop 与 Python 安装相互独立时的目标选择问题，体现了开发者用户对多环境共存的真实需求。  
  https://github.com/HKUDS/nanobot/pull/5676

- **#5652 “add signed direct delivery webhook”**（OPEN）：安全增强型功能，允许可信系统绕过 Agent 循环直接把通知发送到消息总线，适用 CI、监控、计费等场景，涉及文档、测试、安全多维改动，社区关注度可能持续升高。  
  https://github.com/HKUDS/nanobot/pull/5652

整体来看，社区关注点集中在“模型调用异常恢复/容错”与“部署环境适配与安全集成”两个方向。

---

## 5. Bug 与稳定性

### 严重程度：高

- **#5674 [OPEN] Agent 遇 Nvidia NIM 特定错误后停止工作**  
  当 Nvidia NIM provider 返回 “LLM returned error: Error calling LLM: timed out after 300s/600s” 时，NanoBot 会把超时信息误当作 model output，导致 Agent 无法继续运行。  
  → 已有疑似修复 PR #5675（allow model failover after runner deadlines），通过允许模型在 runner 超时后触发 failover 来解决。  
  https://github.com/HKUDS/nanobot/issues/5674  
  https://github.com/HKUDS/nanobot/pull/5675

### 严重程度：中

- **#5589 [OPEN] 被丢弃的会话可能“复活”并继续向全局消息总线发布消息**（fix 已提交）  
  会话丢弃时，pending queue 或 deferred automation queue 中尚未处理的消息可能在任务清理期间被发布到全局总线。修复 PR 仍待合并。  
  https://github.com/HKUDS/nanobot/pull/5589

- **#5580 [OPEN] Session 持久化阻塞事件循环**（fix 已提交）  
  慢速 session 存储或文件锁竞争会阻塞事件循环，进而拖慢无关会话与运行时事件。PR 将 session 加载/保存/checkpoint 通过 `nanobot.session.io` 适配器移出事件循环。  
  https://github.com/HKUDS/nanobot/pull/5580

- **#5471 [OPEN] Ephemeral SDK run 未真正保持“不持久化会话状态”**（fix 已提交）  
  文档声明 `run(ephemeral=True)` 不持久化 turn 也不压缩会话历史，但实际实现未兑现。  
  https://github.com/HKUDS/nanobot/pull/5471

### 严重程度：待确认

- **#5633 [OPEN] Session 键存在路径穿越风险**（fix 已提交，标记 p1）  
  Session key 会直接转为文件路径，恶意 session id 可能寻址到 sessions 目录之外。修复 PR 增加了 `validate_session_key()`。  
  https://github.com/HKUDS/nanobot/pull/5633

---

## 6. 功能请求与路线图信号

以下功能类 PR 当前虽大多未合并，但集中反映了产品路线图可能的方向：

- **CLI 多目标选择增强**（#5676）：让 `nanobot` 与 `nanobot webui` 在 Desktop 与 Python 安装并存时，按调用指定运行目标，并保留 `commands:app` 启动器。近期提交，社区关注度高，较可能进入后续版本。  
  https://github.com/HKUDS/nanobot/pull/5676

- **签名直投 Webhook**（#5652）：基于签名的 webhook，让 CI、监控、计费等可信系统直接把通知文本投递到消息总线，不经过 Agent 循环，适合确定性通知场景。  
  https://github.com/HKUDS/nanobot/pull/5652

- **Heartbeat 模型可配置化**（#4549、#4551）：允许为 heartbeat 指定更廉价的模型（`modelOverride`），并支持选择是否在共享会话中执行（`isolatedSession`）。两个 PR 均由 dajiaohuang 于 6 月 26 日提交，距今已超两个月，仍带 conflict 标记，需要维护者推动合并或明确方向。  
  https://github.com/HKUDS/nanobot/pull/4549  
  https://github.com/HKUDS/nanobot/pull/4551

- **Per-spawn 模型预设 allowlist**（#5561）：针对 #4231 的替代实现，为 spawn 功能引入 `spawnPresets` 白名单以限定可选模型。类似设计已在社区讨论中积累一定共识。  
  https://github.com/HKUDS/nanobot/pull/5561

- **MCP Apps 结果元数据保留**（#5386）：将 MCP Apps 工具的结构化结果与面向模型的文本分离，避免无关数据进入模型上下文，同时保留富元数据。  
  https://github.com/HKUDS/nanobot/pull/5386

以上功能若进入主线，将显著增强 NanoBot 在部署灵活性、外部系统集成、低成本运行与 MCP 生态方面的能力。

---

## 7. 用户反馈摘要

由于当前窗口评论数据有限，反馈主要从 Issue 与 PR 摘要中的真实场景提炼：

- **Nvidia NIM 用户遭遇服务不可恢复中断**（#5674）：用户报告 NIM 返回 300s/600s 超时后，NanoBot 会将其视为模型输出，导致 Agent 永久停摆。这是典型的云模型 API 不稳定场景下的容错诉求——用户期望看到 failover 而非静默崩溃。  
  https://github.com/HKUDS/nanobot/issues/5674

- **多安装环境下的 CLI 心智负担**（#5676）：用户在同时装有 Desktop 版和 Python 版的环境中，调用 `nanobot` 时缺少清晰的目标选择机制，容易跑错运行环境。  
  https://github.com/HKUDS/nanobot/pull/5676

- **远程 WebUI 使用限制**（#5673）：远程用户无法通过 WebUI 选择服务器上的任意项目路径，且原生文件夹选择器会在远程会话中错误地打开客户端本地文件浏览器。反馈指向“远程开发 + 本地 gateway”这一使用场景的体验缺口。  
  https://github.com/HKUDS/nanobot/pull/5673

- **Dream 文件体积失控隐患**（#5630）：此前的修复移除了对 SOUL.md / USER.md / MEMORY.md 的唯一大小上限，导致文件可无限增长并被注入每次请求。用户侧体现为对长期运行成本与上下文膨胀的担忧。  
  https://github.com/HKUDS/nanobot/pull/5630

---

## 8. 待处理积压

以下重要 PR 长期未合并，建议维护者重点关注：

- **#4549 feat(heartbeat) model_override**：自 2026-06-26 起悬置超过 70 天，仍标记 conflict。  
  https://github.com/HKUDS/nanobot/pull/4549

- **#4551 feat(heartbeat) isolated_session**：与 #4549 同作者、同日期、同类目，同样 conflict，等待超过 70 天。  
  https://github.com/HKUDS/nanobot/pull/4551

- **#5386 feat(mcp) 保留 MCP Apps 结果元数据**：2026-08-13 提交，标记 conflict，与近期 MCP 生态演进直接相关。  
  https://github.com/HKUDS/nanobot/pull/5386

- **#5457 fix(channels) dispatcher 异常边界**：2026-08-20 提交，带 conflict，修复可能导致所有 outbound 消息停发的严重问题。  
  https://github.com/HKUDS/nanobot/pull/5457

- **#5471 fix(sdk) ephemeral run 会话状态保留**：提交于 2026-08-21，当前存在 conflict，影响 SDK 行为一致性。  
  https://github.com/HKUDS/nanobot/pull/5471

- **#5561 feat(spawn) per-spawn 模型预设**：2026-08-27 提交，标记 conflict，涉及 #4231 功能需求，社区等待时间已不短。  
  https://github.com/HKUDS/nanobot/pull/5561

- **#5630 fix(agent) Dream 文件大小护栏**：2026-09-02 提交，带 conflict，若长期不合并可能导致 Dream 功能内存/上下文失控。  
  https://github.com/HKUDS/nanobot/pull/5630

- **#5664 fix(agent) 限制 idle summary 缓存**：2026-09-04 提交，已标记 conflict，涉及无限增长的内存缓存问题。  
  https://github.com/HKUDS/nanobot/pull/5664

该批 PR 已出现的共同特点是 **conflict 标记较多**。建议维护者安排一次集中 rebase/conflict 解决，或明确逐条给出结论（merge/close/要求修改），以降低社区贡献者的等待成本。

---

## 结语

从今日数据看，NanoBot 项目社区贡献活跃、覆盖面广（CLI、WebUI、MCP、Session 安全、模型容错均有提交），但 PR 合并积压与 conflict 问题正逐渐成为项目健康度的主要制约因素。建议项目维护团队在下一阶段优先处理：(1) #5674 相关修复合并，尽快解决 Nvidia NIM 容错问题；(2) 针对一批长期 conflict PR 做集中处理，恢复贡献者信心。

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent 项目动态日报 — 2026-09-06

**数据快照**：过去 24 小时 Issue 更新 50 条（新增/活跃 48，关闭 2）；PR 更新 50 条（待合并 48，合并/关闭 2）；新版本发布 0 个。

## 1. 今日速览

项目处于**发布间期的密集修复冲刺**：单日新提交约 9 个 PR，其中 6 个为连接/socket 泄漏类“卫生型”修复（如 [#104033](https://github.com/NousResearch/hermes-agent/pull/104033)、[#104034](https://github.com/NousResearch/hermes-agent/pull/104034)、[#104035](https://github.com/NousResearch/hermes-agent/pull/104035)），呈现集中审计后的批量修补特征。Issue 侧新增约 10 条，集中于认证凭据持久化（[#103989](https://github.com/NousResearch/hermes-agent/issues/103989)、[#103978](https://github.com/NousResearch/hermes-agent/issues/103978)）、桌面端与会话状态一致性（[#103900](https://github.com/NousResearch/hermes-agent/issues/103900)、[#103985](https://github.com/NousResearch/hermes-agent/issues/103985)）与更新链路安全（[#102193](https://github.com/NousResearch/hermes-agent/issues/102193)）。热度最高的 [#66616](https://github.com/NousResearch/hermes-agent/issues/66616)（163 条评论）为自动化探针噪音，但其暴露的 Skills 索引降级问题已持续约 7 周未根治。项目整体活跃度**高**，但**安装/更新链路反复回归**与**自动化 Issue 噪音**是当前最明显的健康度减分项。

## 2. 版本发布

今日无新版本发布。

## 3. 项目进展

- **已确认落地修复**：[#45876](https://github.com/NousResearch/hermes-agent/issues/45876)（cron 会话中 `web_search` 配置了 AnySearch 仍回退到 DDGS 并超时）今日关闭，标记 `sweeper:implemented-on-main`，即修复已合入 main。
- 另有 2 个 PR 在过去 24 小时被合并/关闭，但快照仅展示评论量前 20 的 PR（均为 OPEN），具体编号无法从当前数据确认。
- 两个被关闭的 Issue 中，[#104012](https://github.com/NousResearch/hermes-agent/issues/104012) 为 AI 助手未经账号所有者授权提交后主动撤回，不计入有效进展。
- **合并等待仍是瓶颈**：一批功能 PR 已停留 1~2.5 个月未合入（见“待处理积压”），说明 review 吞吐量低于提交速度。

## 4. 社区热点

| 排名 | Issue | 评论数 | 核心主题 |
|---|---|---|---|
| 1 | [#66616](https://github.com/NousResearch/hermes-agent/issues/66616) | 163 | Skills 索引 freshness 探针持续 `degraded`（index 29.8h 旧，阈值 26h） |
| 2 | [#97681](https://github.com/NousResearch/hermes-agent/issues/97681) | 23 | Bot 群聊应在 Desktop 关闭后继续运行（gateway 基础已合入 main） |
| 3 | [#23717](https://github.com/NousResearch/hermes-agent/issues/23717) | 20 | RFC：可插拔 SessionDB（PostgreSQL/MySQL），👍 8 |
| 4 | [#26058](https://github.com/NousResearch/hermes-agent/issues/26058) | 10 | Discord `auto_thread` 与 `free_response_channels` 互斥破坏合法用例，👍 5 |
| 5 | [#98022](https://github.com/NousResearch/hermes-agent/issues/98022) | 10 | `hermes update` catch-up fleet restart 因陈旧 receipt 无限循环 |

**诉求分析**：

- **#66616 的 163 条评论中大量为自动化回写噪音**，真实信号是该探针自 7 月 18 日以来反复失败，指向 `.github/workflows/skills-index.yml`（cron 6/18 UTC）与 `deploy-site.yml` 之间索引重建链路存在未闭环故障——建议维护者优先修复 CI 而非继续让 bot 刷屏。
- **#23717（5 月 11 日开启，悬置近 4 个月）** 是社区对“热更新时共享 `state.db` SQLite 写入导致死亡螺旋”的架构级回应，8 👍 表明自托管重度用户对 PostgreSQL/MySQL 后端有明确需求，RFC 已成型，等待 `needs-decision` 标签被解除。
- **#97681 与 #98022 共同指向“长生命周期运行”可靠性**：前者要求 gateway 群聊能力不依赖桌面端进程，后者要求更新机制在异常状态下不进入无限重启。两者是同一批重度用户诉求的两面。

## 5. Bug 与稳定性

### P1（严重）

- [#26058](https://github.com/NousResearch/hermes-agent/issues/26058)（P1, `needs-decision`）：提交 `d557544` 使 `free_response_channels` 中的频道完全跳过 auto-thread 创建，即使显式配置了 `auto_thread`。讨论 10 条，尚无修复 PR。
- [#98022](https://github.com/NousResearch/hermes-agent/issues/98022)（P1）：`update_receipts/latest.json` 残留中断 receipt 时，catalog 更新后的 catch-up fleet restart 每次运行都会触发，即使版本已最新。关联父 bug [#95294](https://github.com/NousResearch/hermes-agent/issues/95294)。

### P2（高）

- [#103989](https://github.com/NousResearch/hermes-agent/issues/103989)（新，安全边界）：`hermes auth add openai-codex --type oauth` 完成 OAuth 并打印 “Added”，凭据实际未写入 `auth.json`——**假成功**比报错更危险。
- [#103978](https://github.com/NousResearch/hermes-agent/issues/103978)（新，安全边界）：Claude Code OAuth 自动发现会在 token 过期时刷新，而该 refresh token 是单次使用的，导致**用户 Claude CLI 被登出**，且违反 Anthropic 消费者 ToS；用户请求增加 opt-out。
- [#102193](https://github.com/NousResearch/hermes-agent/issues/102193)（新，`needs-repro`）：用户明确表示 “**已连续数月多次报告**”，`hermes update` 仍在 `~/.hermes/` 下制造 root-owned 文件（同源问题另见 [#91212](https://github.com/NousResearch/hermes-agent/issues/91212) 的 `.gateway-planned-stop.json`）。这是更新链路最顽固的稳定性/安全回归。
- [#97394](https://github.com/NousResearch/hermes-agent/issues/97394)：Windows Desktop 更新看门狗只认 `logs/update.log` 增长，而 `--gateway` 模式从不创建该文件，导致**健康更新被误杀**。
- [#90495](https://github.com/NousResearch/hermes-agent/issues/90495)：ZIP 回退路径删除 Desktop 应用与 `web_dist`，且安装器“遗忘”Desktop 曾安装，后续更新永不重建。
- [#90782](https://github.com/NousResearch/hermes-agent/issues/90782)：终端环境快照泄漏 `HERMES_DELEGATED_CHILD_CONTEXT=1`，污染父会话的 kanban CLI。
- [#82912](https://github.com/NousResearch/hermes-agent/issues/82912)：cron 任务配置 `enabled_toolsets: ["web","file"]` 时**整个 web 工具集被静默丢弃**，单独配置 `["web"]` 却正常——属配置解析顺序 bug。
- [#103900](https://github.com/NousResearch/hermes-agent/issues/103900)（9 月 5 日新报）：Desktop 侧边栏 pin 会话仅写本地状态，不置 canonical session-store `pinned` 标志，native Hermes 看不到。

### P3 与长尾稳定性

- [#103974](https://github.com/NousResearch/hermes-agent/issues/103974)（安全）：Kanban worker 身份由环境变量存在性决定，子进程/孙进程可“升级”为完整 worker，提案引入 `HERMES_KANBAN_OWNER_PID` + 默认 scrub。
- [#103985](https://github.com/NousResearch/hermes-agent/issues/103985)：Desktop “Hide from sidebar” 对 worktree lane 是静默 no-op，存活 worktree 会立刻令其复活。
- [#103893](https://github.com/NousResearch/hermes-agent/issues/103893)：群聊 hold 指令分类器用 `/\b(stop|halt|pause)\b/i` 匹配全文，**德语填充词 “halt” 会误触发暂停**——非英语用户的多语言回归案例。
- [#104005](https://github.com/NousResearch/hermes-agent/issues/104005)（新，duplicate）：holographic 实体抽取正则从缩略语和句首词铸造出垃圾实体。

### 已有修复 PR 在途

- [#104036](https://github.com/NousResearch/hermes-agent/pull/104036)：Bedrock 过期 AWS 凭据在签名阶段失败被误分类为 “unknown”，改为归为 auth 错误。
- [#103582](https://github.com/NousResearch/hermes-agent/pull/103582)：launchd 托管的 gateway 因 PID 文件缺失被误报 offline。
- 连接泄漏批量修复：[#104037](https://github.com/NousResearch/hermes-agent/pull/104037)（xAI）、[#104035](https://github.com/NousResearch/hermes-agent/pull/104035)（OpenAI）、[#104034](https://github.com/NousResearch/hermes-agent/pull/104034)（Krea 轮询）、[#104033](https://github.com/NousResearch/hermes-agent/pull/104033)（evals）、[#104024](https://github.com/NousResearch/hermes-agent/pull/104024)（google-workspace revoke）。

## 6. 功能请求与路线图信号

- **macOS LaunchAgent 生命周期管理**：[#44106](https://github.com/NousResearch/hermes-agent/issues/44106) 今日迎来实现 PR [#104022](https://github.com/NousResearch/hermes-agent/pull/104022)（`hermes dashboard service <install|start|stop|...>`），是下一版本**最可能合入的新功能**。
- **Bot 群聊与桌面端解耦**：[#97681](https://github.com/NousResearch/hermes-agent/issues/97681) 的 gateway 权威、同 gateway runner、跨 gateway 传输已上 main，剩余工作是将地基接到 Desktop UI，属路线图上的“生产化收尾”。
- **delegate_task 按任务路由 profile**：PR [#103965](https://github.com/NousResearch/hermes-agent/pull/103965) 新增子代理按 Hermes profile 分配 model/host/memory/state.db——印证多 persona（如 tigger/piglet 多机异构）是真实用户场景，可能进入下版。
- **可插拔 SessionDB**：[#23717](https://github.com/NousResearch/hermes-agent/issues/23717) 是路线图上最大的架构候选，但 4 个月未做决策，建议维护者明确表态或委托 POC。
- **指向下一版本的小型特性**：向 live session 投递消息的官方入口（[#103748](https://github.com/NousResearch/hermes-agent/issues/103748)）、只读 Plan mode（[#80994](https://github.com/NousResearch/hermes-agent/issues/80994)）、聊天面 skills catalog 压缩开关（PR [#72200](https://github.com/NousResearch/hermes-agent/pull/72200)）、[#101420](https://github.com/NousResearch/hermes-agent/pull/101420) 三平台安装/更新 E2E 测试矩阵（工程质量信号）。

## 7. 用户反馈摘要

- **最集中的不满：依赖卫生与更新链路**。用户 `eabase` 今日在多条 Issue 中表达挫败：`~/.hermes/` root-owned 文件“已报告多次仍复现”（[#102193](https://github.com/NousResearch/hermes-agent/issues/102193)）；venv 中 ~112 个包超半数陈旧含 certifi（[#83673](https://github.com/NousResearch/hermes-agent/issues/83673)）；直指 “devs 在发布前不运行 npm-check / npm outdated”（[#102563](https://github.com/NousResearch/hermes-agent/issues/102563)）；update 异常耗时 6 分钟且日志无帮助（[#102540](https://github.com/NousResearch/hermes-agent/issues/102540)）。**信任成本在累积**。
- **桌面端与 native 状态分叉**：[#103900](https://github.com/NousResearch/hermes-agent/issues/103900) 用户观察到 pin 在 Desktop 侧“看似成功”但 native Hermes 不认——UI 层假成功比显式报错更伤体验。
- **AI 代发 Issue 引发反感**：[#104012](https://github.com/NousResearch/hermes-agent/issues/104012) 明确声明 “由 AI 助手未经账号所有者批准提交，请忽略”，说明社区已出现 AI 代提 Issue 的撤回先例，建议为自动化投稿加审批门禁。对比之下，[#104005](https://github.com/NousResearch/hermes-agent/issues/104005) 是 Hermes 在本地发现自身 holographic 插件 bug 后建议用户上报——dogfooding 的正面案例。
- **德语用户的本地化痛点**：[#103893](https://github.com/NousResearch/hermes-agent/issues/103893) 给出具体版本 commit（`9dd6634`）与触发路径，是高质量的多语言回归报告。

## 8. 待处理积压（提请维护者关注）

| 类型 | 编号 | 搁置时长 | 状态 |
|---|---|---|---|
| RFC / 架构决策 | [#23717](https://github.com/NousResearch/hermes-agent/issues/23717) | 118 天（5/11） | `needs-decision`，8 👍，等待明确结论 |
| P1 Bug + 决策 | [#26058](https://github.com/NousResearch/hermes-agent/issues/26058) | 114 天（5/15） | P1 与 `needs-decision` 并存，长期无人拍板 |
| 自动化噪音 | [#66616](https://github.com/NousResearch/hermes-agent/issues/66616) | 50 天（7/18） | Skills 索引 degraded 未根治；建议修 CI + 给 bot 降噪 |
| Issue 无人认领 | [#43847](https://github.com/NousResearch/hermes-agent/issues/43847) | 87 天（6/11） | P2 重构（进程 kill-safety 泛化），仅 1 评论、无关联 PR |
| PR 积压 | [#51953](https://github.com/NousResearch/hermes-agent/pull/51953) | 74 天（6/24） | Copilot reasoning 修复，等待合并 |
| PR 积压（Mattermost 系列） | [#54193](https://github.com/NousResearch/hermes-agent/pull/54193)、[#54229](https://github.com/NousResearch/hermes-agent/pull/54229)、[#64270](https://github.com/NousResearch/hermes-agent/pull/64270) | 54~70 天 | 同作者同平台三连 PR，功能相互依赖，建议捆绑评审 |
| PR 积压（8 月批） | [#80583](https://github.com/NousResearch/hermes-agent/pull/80583)、[#80594](https://github.com/NousResearch/hermes-agent/pull/80594)、[#80600](https://github.com/NousResearch/hermes-agent/pull/80600)、[#75679](https://github.com/NousResearch/hermes-agent/pull/75679) | 31~37 天 | MCP 超时、tool_call 校验、WhatsApp GIF、memory provider 别名 |

---

**总体评估**：项目提交活跃度高，社区反馈渠道畅通；但**更新链路稳定性、依赖陈旧度与 PR 合并吞吐**是三大健康度短板。建议下个版本优先合入 macOS LaunchAgent（#104022）、连接泄漏批量修复与 Mattermost 积压系列，并针对 root-owned 文件问题做一次根因专项治理，以回应高频用户投诉。

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

# PicoClaw 项目动态日报 — 2026-09-06

## 1. 今日速览

过去 24 小时，PicoClaw 仓库无新版本发布，无新开 Issue/PR，整体以存量维护为主。3 个创建于 2026-03-14 的批量修复汇总 PR 于 2026-09-05 关闭，仓库当前待合并 PR 归零。Issue 侧，高讨论量的功能请求 #3287 仍在活跃更新，功能请求 #3342 则被标为 stale 后关闭。整体活跃度中等偏下，社区讨论节奏正常，但项目对 issue/PR 的响应周期明显偏长，跨度为 2 周至约 175 天，维护带宽是当前项目健康度的主要约束。

## 2. 版本发布

无新版本发布，暂无 release 更新内容、破坏性变更或迁移注意事项可记录。

## 3. 项目进展

今日被关闭的 3 个 PR 均为长期存在的批量集成型 PR，目的是将此前多个 open 的修复 PR 收拢到同一分支中：

- [PR #1559](https://github.com/sipeed/picoclaw/pull/1559) — `fix: merge PR #1327 #1319 #1318 #1313`，整合 4 个 PR 的修改，2026-03-14 创建，2026-09-05 关闭。
- [PR #1545](https://github.com/sipeed/picoclaw/pull/1545) — `fix: merge PR #1500 #1490 #1488 #1487 #1485`，整合 5 个 PR 的修改，2026-03-14 创建，2026-09-05 关闭。
- [PR #1555](https://github.com/sipeed/picoclaw/pull/1555) — `fix: merge PR #1390 #1389 #1383 #1381`，整合 4 个 PR 的修改，2026-03-14 创建，2026-09-05 关闭。

三个 PR 合计涉及 13 个被引用的修复 PR。但需要指出，GitHub 数据显示这三个 PR 状态为 **CLOSED** 而非 **MERGED**，且 PR 描述中未披露具体修复内容，因此本次关闭更适合理解为“长期集成请求的收尾与清理”，不一定代表这些修复已经随新版本合入主线。PR 队列层面，仓库的待合并数量清零，队列整洁度有所提升。

## 4. 社区热点

[Issue #3287](https://github.com/sipeed/picoclaw/issues/3287) 是今日讨论最活跃的条目，开放中，已有 10 条评论，更新于 2026-09-05。该 Issue 的核心使用场景是 IRC 通道：IRC 默认有约 512 字节的行长度限制，客户端发送超长内容时会自动将其拆分为多条消息，而 PicoClaw 目前倾向于把拆分后的每一个片段当作独立消息处理。这会导致一条完整的长输入被割裂成多条独立 prompt，破坏语义连续性。10 条评论说明该需求在 IRC 用户群体中有实际共鸣，且已存在一个多月仍未进入实现阶段。

[Issue #3342](https://github.com/sipeed/picoclaw/issues/3342) 以较低热度结束，评论仅 2 条，已被标为 stale 并关闭。该 Issue 主张增加可选的 “after-turn” 模式，把忙碌会话中的消息排队而不是中断当前执行，但社区讨论热度不足以让其继续停留在开放队列中。

## 5. Bug 与稳定性

今日没有新增的 Bug、崩溃或严重回归问题报告。3 个关闭的 PR 虽然以 `fix` 开头且涉及多个历史修复的汇总，但由于缺少 commit 级细节，无法从公开数据确认是否包含稳定性方面的改动。Issue 队列中也未发现用户报告新回归。当前稳定性风险较低。

## 6. 功能请求与路线图信号

目前开放队列中最明确的功能请求是 [Issue #3287](https://github.com/sipeed/picoclaw/issues/3287)：**IRC 长消息的语义重组**。该需求贴近 PicoClaw 作为 IRC/聊天 agent 的实际使用体验，若实施成本可控，很有可能进入下一版本的产品与协议适配范围。

[Issue #3342](https://github.com/sipeed/picoclaw/issues/3342) 提出的 “after-turn” 消息排队模式虽然已被 stale 关闭，但它仍是一个有价值的产品思路，尤其适合希望长任务不被中途消息打断的 power user。若未来 roadmap 将长时运行任务作为重点，该需求有可能被重新开启或吸收进新的 steering 设计中。

## 7. 用户反馈摘要

从今日活跃的 Issue 中可以提炼出两类真实用户诉求：

- **IRC 长文本场景下的上下文割裂问题**：用户希望 PicoClaw 理解，经 IRC 自动拆分并分多条发送的长消息应被视为一条完整消息。当前行为会让 agent 在接收长 prompt 时“看到”多段彼此割裂的内容，影响回答质量。来源：[Issue #3287](https://github.com/sipeed/picoclaw/issues/3287)

- **执行过程中被多余消息打断的困扰**：当用户发送第一条消息后，agent 仍在执行工具调用，此时用户发第二条消息，当前设计会直接跳过第一条消息剩余的 tool calls，并将其视为 “mid-task course correction”。部分用户不接受这种默认的中断式交互，希望拥有可选的排队机制。来源：[Issue #3342](https://github.com/sipeed/picoclaw/issues/3342)

## 8. 待处理积压

- **[Issue #3287](https://github.com/sipeed/picoclaw/issues/3287)（高关注度）**：已开放约 46 天，最近一次更新在 2026-09-05，评论数为 10，但仍无关联 PR，也没有维护者给出明确的 roadmap 回复。这是当前最需要维护者回应的公开 Issue。
- **PR 处置周期过长（流程信号）**：今日关闭的 3 个 PR 从创建到关闭耗时约 175 天，即便最终以清理收尾，也反映出仓库对长期开放 PR 缺少及时决策。若这些 PR 所包含的修复确有意义，建议维护者在后续 release 中通过 commit 记录明确其去向。

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw 项目动态日报 | 2026-09-06

## 1. 今日速览

- 过去 24 小时 Issue 无新增、无关闭，Release 无更新，项目的问题反馈通道较为平静。
- 共有 3 条 PR 处于待合并状态并在统计窗口内有过更新，无 PR 被合并或关闭。
- 3 条 PR 均不属于新功能，而是聚焦在依赖版本修复、测试环境清理和文档示例修正。
- 项目当前整体状态可概括为：**社区贡献活跃，维护者合并节奏偏慢**，后续需观察维护者是否在近 48 小时内处理这 3 条待审 PR。

## 2. 版本发布

本期无新版本发布（最新 Releases 为空）。

## 3. 项目进展

今日合并/关闭 PR 数为 0，主分支没有因 PR 合入产生新的代码变更。  
但从待合并 PR 的内容看，目前有 3 项改进正在排队，一旦合入将小幅提升项目的稳定性和工程质量：

- [PR #3725](https://github.com/nanocoai/nanoclaw/pull/3725) — 将 Linux 版 `signal-cli` 安装版本从 `0.14.3` 固定升级到 `0.14.7`，避免向无现有 session 的联系人发消息时永久挂起。属于直接修复用户可感知的通道稳定性问题。
- [PR #3710](https://github.com/nanocoai/nanoclaw/pull/3710) — 修复 `pnpm test` 在 OS 临时目录残留约 355 个目录的问题。对长期运行的开发机和 CI runner 是实质性的工程效能改进。
- [PR #3724](https://github.com/nanocoai/nanoclaw/pull/3724) — 更新 `add-opencode` skill 中已退役的 Anthropic 模型 ID，避免用户照抄文档示例时报错。

整体来看，项目主线尚未因今日 PR 产生实际推进，但上述改动若获合并，将主要在**安装可靠性、测试卫生和文档正确性**三个方向带来增量改善。

## 4. 社区热点

从现有数据看，本期 Issue 和 PR 均未出现高评论、高反应讨论。3 条 PR 的评论与点赞数据均为空，严格意义上没有传统社区热点。

若按用户实际影响面判断，值得关注的是 [PR #3725](https://github.com/nanocoai/nanoclaw/pull/3725)，它反映的诉求是：**官方安装脚本不应默认携带已知的严重缺陷版本**。`signal-cli 0.14.3` 在发送消息给无现有 session 的联系人时可能无限期挂起，这对 Linux 新用户是很差的首次体验。此类问题也容易在 Issue 区引发集中反馈，建议维护者优先处理。

## 5. Bug 与稳定性

本期没有新增 Issue 形式的 Bug 报告，但 3 条 PR 中有 2 条与稳定性直接相关，1 条属于过期文档信息。

| 严重程度 | 问题描述 | 修复状态 |
| --- | --- | --- |
| 高 | Linux 新装脚本把 `signal-cli` 固定在 `0.14.3`，该版本向无现有 session 的联系人发消息时可能永久挂起 | [PR #3725](https://github.com/nanocoai/nanoclaw/pull/3725) 待合并 |
| 中 | 完整运行 `pnpm test` 会在 OS 临时目录留下约 355 个目录，重启或 30 天系统清理前不会被回收；`/tmp` 为 tmpfs 的机器受影响更明显 | [PR #3710](https://github.com/nanocoai/nanoclaw/pull/3710) 待合并 |
| 低 | `add-opencode` skill 文档中的 Anthropic 模型 ID `claude-sonnet-4-20250514` 已于 2026-06-15 退役，照抄示例会导致调用失败 | [PR #3724](https://github.com/nanocoai/nanoclaw/pull/3724) 待合并 |

目前没有发现已确认的崩溃或回归类问题；上述 PR 均为主动修复，而非事后补救。

## 6. 功能请求与路线图信号

本期没有用户提交的新功能请求，但从 3 条 PR 中可以读出一些路线图信号：

- 项目当前重心似乎正从快速迭代转向**稳定性与工程质量收尾**：修复安装默认版本、清理测试残留、更新过期文档，都是典型的新版本发布前的整理动作。
- [PR #3725](https://github.com/nanocoai/nanoclaw/pull/3725) 提示安装脚本中硬编码版本号存在长期维护风险。未来可能值得考虑引入“可配置版本号 + 校验和检查”机制，避免同类问题再次出现。
- [PR #3724](https://github.com/nanocoai/nanoclaw/pull/3724) 显示 skill 示例中直接硬编码了第三方模型 ID。若该 skill 面向广泛应用，更稳妥的做法是改为可配置项或从提供方动态获取模型列表。

## 7. 用户反馈摘要

本期没有 Issue 更新，也没有可用的 PR 评论语料，因此无法提供直接引用形式的用户反馈。

从 PR 描述中可以间接看出以下用户痛点：

- Linux 用户通过 Signal 渠道向无 session 的联系人发送消息时，可能遭遇永久性挂起，现有 workaround 只能手动升级 `signal-cli`。
- 使用 `pnpm test` 的开发者或 CI 维护者长期受到临时目录膨胀困扰，尤其 `/tmp` 为 tmpfs 的环境可能面临磁盘容量压力。
- 参考 `add-opencode` skill 配置 Anthropic 的用户会发现官方示例给出的模型 ID 已退役 3 个月，按文档操作会得到无效配置。

这些反馈共同指向一个方向：**文档与安装脚本的默认状态需要和上游保持同步**。

## 8. 待处理积压

本期数据中没有长期无人响应的陈旧 Issue，但 3 条 PR 均已处于待合并状态，建议维护者尽快安排 review：

- [PR #3710](https://github.com/nanocoai/nanoclaw/pull/3710) — 自 2026-09-03 创建至今已等待约 3 天，是当前最久未合并的 PR。如果仓库启用了 stale bot，需留意其进入自动过期流程。  
- [PR #3725](https://github.com/nanocoai/nanoclaw/pull/3725) — 创建于 2026-09-05，等待 1 天。因其修复的是用户可感知的挂起问题，建议优先处理。  
- [PR #3724](https://github.com/nanocoai/nanoclaw/pull/3724) — 创建于 2026-09-05，变更简单、风险低，可随同下一批 PR 快速合入。

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw 项目日报 — 2026-09-06

数据窗口：2026-09-05 至 2026-09-06（近 24 小时）

---

## 1. 今日速览

过去 24 小时项目整体活跃度较低但方向明确：新开 1 个 Bug Issue（#8074），1 个大型功能 PR 进入待合并队列（#8075）。无新版本发布，无 PR 合并/关闭，说明当前处于功能开发与审阅的间歇期，而非交付高峰期。值得关注的是 #8075 是一个 XL 体量、低风险的沙箱默认行为变更，且依赖尚未合并的 #7908，意味着近期有一条明确的功能管线正在持续推进。项目健康度稳定，社区反馈集中在共享频道连接状态下的提示文案准确性上，属于细节体验类问题。

---

## 2. 版本发布

今日无新版本发布。

---

## 3. 项目进展

今日没有 PR 被合并或关闭，项目代码基线无变化。

在途的重要进展是 **#8075** — `feat: make the embedded Pi sandbox loop the startup default`。该 PR 将嵌入式 Pi 沙箱循环设为默认启动方式，为基准测试场景提供开箱即用的环境。PR 体量为 XL，scope 涉及 sandbox 与 docs，风险评估为 low，且由 core 成员提交，说明核心团队正在推进沙箱默认体验的架构性调整。

需要注意的是，该 PR 明确标注 **Stacked on #7908**（基础分支为 `feat/7903-native-loop-sandbox-spike`），在 #7908 合并前不应合入。因此当前进展实际是功能链路上的中间站点，后续合并节奏取决于 #7908 的审阅进度。

🔗 [PR #8075](https://github.com/nearai/ironclaw/pull/8075)

---

## 4. 社区热点

今日唯一有评论互动的 Issue 是 **#8074**，评论数 1 条。该 Issue 描述了一个较为细致的 UX/Bug 场景：

> Paired 用户在「未连接」的共享频道中执行操作被拒绝时，收到的是为 unpaired 用户场景撰写的 `connect_required` 提示文案（引导用户在 IronClaw Web 应用中连接账户），而不是真正应展示的「该频道尚未连接」的提示。

这一反馈反映出：多用户协作/共享频道场景下，不同身份状态（paired vs unpaired）与频道连接状态（connected vs not-connected）的组合正在产生用户可感知的文案错配。虽然评论数不高，但该 Issue 语义清晰、复现路径完整，指向的是产品文案逻辑分支设计问题，容易在真实多用户工作流中造成困惑。

🔗 [Issue #8074](https://github.com/nearai/ironclaw/issues/8074)

---

## 5. Bug 与稳定性

今日报告 1 个 Bug，无崩溃或严重回归类问题。

| 严重程度 | Issue | 描述 | 目前状态 |
|---------|-------|------|----------|
| 中 | [#8074](https://github.com/nearai/ironclaw/issues/8074) | Paired 用户在未连接的共享频道执行操作被拒绝时，收到 unpaired 场景的连接引导文案，与实际原因不符 | Open，暂无关联 fix PR |

该问题不涉及数据安全或核心功能瘫痪，但会影响多用户共享频道场景下的产品可理解性，属于中等优先级的体验修复项。目前无已提交的 fix PR，建议维护者将此 Issue 与文案模板的分支逻辑结合起来排查。

---

## 6. 功能请求与路线图信号

今日没有明确的社区新功能请求。路线图信号主要来自核心团队的 PR #8075：将 Pi sandbox loop 设为启动默认值，并明确提及是为基准测试（benchmark use）场景服务。结合其依赖的 #7908（`feat/7903-native-loop-sandbox-spike`，spike 性质），可以判断 IronClaw 团队正在将「原生循环沙箱」从实验性 spike 转化为默认产品行为，下一版本很可能包含以下变化：

- 新的默认启动配置文件切换为 `hosted-…`（基于 Pi/agent-core worker）；
- 沙箱镜像内预置 pinned Bun 运行时与 agent-core worker；
- 相关 sandbox 和 docs 的同步更新。

🔗 [PR #8075](https://github.com/nearai/ironclaw/pull/8075)

---

## 7. 用户反馈摘要

今日唯一用户反馈来自 Issue #8074 的描述与评论。提炼如下：

- **使用场景**：用户在 IronClaw 中以 paired 身份加入共享频道，但该频道未完成安装连接。
- **痛点**：被拒绝操作时看到的提示文案是关于「连接你的账户 in IronClaw Web app」的引导，而非「该频道尚未连接」的实际原因。用户需要自行排查才能理解真正的问题，增加了试错成本。
- **潜在期望**：提示文案应依据发起操作的 actor 身份（paired/unpaired）与目标频道的连接状态进行正确的分支判断，两种场景使用各自对应的措辞。

该反馈暴露的是状态矩阵下的文案覆盖不完整，而非功能逻辑本身的缺陷，表明功能已跑通但边界场景的润色仍在进行中。

---

## 8. 待处理积压

- **#8075（Open，待合并）** — 依赖的 #7908 目前也在开放状态。两个 PR 构成一条依赖链，若 #7908 审阅周期过长，#8075 的沙箱默认启动改动将延迟进入主干。建议维护者关注 #7908 的审阅状态，避免 XL PR 长期停留造成合并冲突风险。

  🔗 [PR #8075](https://github.com/nearai/ironclaw/pull/8075)

- **今日 Bug #8074 暂无 fix PR** — 虽非紧急，但建议在下一轮文案 overhaul 或状态逻辑重构时一并纳入，避免拖着成为遗留问题。

  🔗 [Issue #8074](https://github.com/nearai/ironclaw/issues/8074)

---

**总结**：IronClaw 今日整体处于「开发进行中、交付暂缓」的状态。核心动向集中在 Pi 沙箱默认化这一架构决策上，社区反馈则暴露了共享频道提示文案状态分支不完善的问题。建议观察 #7908 → #8075 的合并进度，视其为项目短期内的主要里程碑。

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# LobsterAI 项目动态日报 — 2026-09-06

> 数据来源：github.com/netease-youdao/LobsterAI | 统计窗口：2026-09-05 至 2026-09-06

---

## 1. 今日速览

过去 24 小时项目活跃度较低：无新 Issue、无已关闭 Issue、无新版本发布，亦无 PR 被合并。当前有 2 个 Pull Request 处于开放状态，均于 2026-03-30 创建、2026-09-05 被更新，且已带 `[stale]` 标记——表明二者已进入长期未合并的待办区间。整体判断：项目今日处于「维护静默期」，社区讨论热度平淡，但两个悬置 PR 分别指向**代码架构治理**与**MCP 粒度控制**两个实质性方向，值得维护者关注。

---

## 2. 版本发布

过去 24 小时内无新版本 Release。无更新内容、破坏性变更或迁移说明可报告。

---

## 3. 项目进展

今日没有 PR 被合并或关闭，因此没有可直接归因的功能进展。但从在途 PR 的内容来看，项目有两个值得关注的演进方向：

- [#1069 重构：拆分 CoworkSessionDetail 单文件](https://github.com/netease-youdao/LobsterAI/pull/1069)：针对核心对话组件 `CoworkSessionDetail.tsx`（2100+ 行）进行文件拆分，将类型定义、纯函数、Hook 与 UI 组件解耦。若被合并，将显著降低该模块的维护成本，并改善流式输出场景下的重渲染性能。
- [#1070 feat(cowork): 支持 per-session MCP 开关控制](https://github.com/netease-youdao/LobsterAI/pull/1070)：将会话级 MCP server 的独立启停能力引入 OpenClaw 桌面端，扩展了 Agent 工具的按场景配置灵活性。

两项工作均停留在 PR 阶段（Open，带 stale 标记），未进入主干。项目在代码质量重构与 MCP 功能增强上均有储备，但合并节奏偏慢。

---

## 4. 社区热点

今日无新增 Issue、无活跃评论（两个 PR 的评论数据均为 undefined），未出现高讨论量话题。

在有限的信号中，两个 stale PR 本身可以被视为社区关注点的间接反映：

- [#1069](https://github.com/netease-youdao/LobsterAI/pull/1069) 指向**核心组件规模失控**（2100+ 行单文件）——这是长期迭代后常见的技术债问题，社区贡献者已自发提出拆分方案。
- [#1070](https://github.com/netease-youdao/LobsterAI/pull/1070) 回应了 **MCP 配置粒度不足**的使用痛点，若该议题在用户群中讨论度上升，可能推动其进入下一步迭代。

---

## 5. Bug 与稳定性

过去 24 小时内无新报告的 Bug、崩溃或回归 Issue，也没有相关的修复 PR。项目今日无可见的稳定性风险事件。

需要留意的是，#1069 的 PR 描述中提到 **“流式输出时，无关联历史消息因顶层状态更新而触发不必要重渲染”**——这是一个已定位的性能/渲染问题，但尚无对应 Issue 跟踪，也未被单独修复。该问题目前仅通过重构 PR 间接覆盖，处于悬置状态。

---

## 6. 功能请求与路线图信号

今日没有新的功能请求 Issue。但两个开放 PR 提供了清晰的路线图信号：

- **per-session MCP 开关**（[#1070](https://github.com/netease-youdao/LobsterAI/pull/1070)）：目前 MCP server 仅支持全局启用/禁用，所有会话共享配置。该 PR 提案将会话级 toggle 与持久化引入，并计划在 `McpBridgeServer` 层做请求拦截。这是一个明确的功能增强方向，具备较高的用户可见价值，有较大概率被纳入下一版本规划（若维护者结束 stale 状态并推进代码评审）。
- **组件级架构治理**（[#1069](https://github.com/netease-youdao/LobsterAI/pull/1069)）：虽为重构而非新功能，但反映的路线图信号是——项目开始关注**核心模块的可维护性与渲染性能**，为后续迭代铺平道路。

---

## 7. 用户反馈摘要

今日没有新的 Issue 评论或用户反馈可供提炼。从两个 PR 的提案动机中可以间接读取真实用户侧的痛点：

- **会话上下文隔离需求**（来自 #1070）：不同会话场景（如开发调试 vs. 日常问答）可能需要不同的 MCP 工具集合，当前全局共享机制无法覆盖精细化工作流。
- **对话页面响应性能敏感**（来自 #1069）：流式输出场景下 UI 存在不必要的重渲染，用户对交互流畅度有较高预期。

以上均为推理信号，非直接用户原声；今日无直接评论内容可用于满意度分析。

---

## 8. 待处理积压

以下两个 PR 均创建于 2026-03-30，持续开放超过 5 个月，且近期被标记为 `[stale]`（更新时间为 2026-09-05），需要维护者介入处理：

| PR | 标题 | 创建时间 | 最后更新 | 状态 | 建议 |
|---|---|---|---|---|---|
| [#1069](https://github.com/netease-youdao/LobsterAI/pull/1069) | 重构：拆分 CoworkSessionDetail 单文件，提升可维护性与渲染性能 | 2026-03-30 | 2026-09-05 | OPEN / stale | 安排 code review，决定合并或关闭；如暂不处理应明确回复作者预期 |
| [#1070](https://github.com/netease-youdao/LobsterAI/pull/1070) | feat(cowork): 支持 per-session MCP 开关控制 | 2026-03-30 | 2026-09-05 | OPEN / stale | 该功能与路线图相关度高，建议纳入版本规划或给出明确取舍结论 |

两个 PR 长时间滞留会降低外部贡献者积极性，也增加了与主干冲突的概率。建议维护者尽快进行一次 triage，明确去留。

---

**结论**：LobsterAI 今日无重大动态，项目处于低活跃维护期。短期关注点应放在两个 5 个月以上的 stale PR 上——它们是项目技术债清理与 MCP 功能演进的重要候选，需要维护者给出明确回应，以保持社区贡献通道的健康度。

</details>

<details>
<summary><strong>TinyClaw</strong> — <a href="https://github.com/TinyAGI/tinyagi">TinyAGI/tinyagi</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>Moltis</strong> — <a href="https://github.com/moltis-org/moltis">moltis-org/moltis</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>CoPaw</strong> — <a href="https://github.com/agentscope-ai/CoPaw">agentscope-ai/CoPaw</a></summary>

# CoPaw 项目动态日报（2026-09-06）

> 数据源：agentscope-ai/QwenPaw（CoPaw）。以下链接按原始仓库保留。

## 今日速览

过去 24 小时共产生 11 条 Issue 更新与 7 条 PR 更新：Issue 端 8 开 3 关，PR 端 7 条全部待合并，合并/关闭数为 0。本期无新版本 Release，项目重心集中在 v2.2.x 的缺陷修复与社区路线讨论上。活跃度整体较高，Bug 反馈与修复 PR 形成了一定闭环，例如 #7559 对应 #7577、#7572 对应 #7578；但 0 合并也意味着维护者评审速度正成为当前唯一瓶颈。社区热度上，[#7318（多租户 Hub 2.2.0 路线咨询）](https://github.com/agentscope-ai/QwenPaw/issues/7318) 以 23 条评论领先；稳定性方面，[#7576（上下文窗口硬编码 32768）](https://github.com/agentscope-ai/QwenPaw/issues/7576) 影响面最广。

## 项目进展

今日没有 PR 从待合并状态转为 merged/closed，因此严格意义上的主线代码“零合入”。但这 24 小时的 PR 管线有明显填充：

- [#7577](https://github.com/agentscope-ai/QwenPaw/pull/7577)：console 会话任务运行中不再直接 409，而是将后续消息入队，直接回应 [#7559](https://github.com/agentscope-ai/QwenPaw/issues/7559)。
- [#7578](https://github.com/agentscope-ai/QwenPaw/pull/7578)：为 `_coordinator.py _drain()` 补上 `logger.exception()`，解决工具链异常栈被吞的问题，对应 [#7572](https://github.com/agentscope-ai/QwenPaw/issues/7572)。
- [#7547](https://github.com/agentscope-ai/QwenPaw/pull/7547)：修复飞书会话队列消费者卡死后永久占住队列的问题。
- [#7546](https://github.com/agentscope-ai/QwenPaw/pull/7546)：内置渠道模块改为懒加载，避免 console-only 工作区为 `lark_oapi` 等重型 SDK 付出数十秒启动成本。
- [#7509](https://github.com/agentscope-ai/QwenPaw/pull/7509)：Make Skill v2，已标记 **Ready for Merge**，等待合入已 4 天。

值得注意的是，[#7547](https://github.com/agentscope-ai/QwenPaw/pull/7547)、[#7546](https://github.com/agentscope-ai/QwenPaw/pull/7546)、[#7577](https://github.com/agentscope-ai/QwenPaw/pull/7577)、[#7578](https://github.com/agentscope-ai/QwenPaw/pull/7578) 全部来自新贡献者 `kabishou11`，且覆盖渠道、console、工具调用三个不同模块，说明外部贡献者已具备跨模块修复能力。若这批 PR 在未来数日集中合入，项目稳定性会获得一次明显跃升。

## 社区热点

- **[QwenPaw Hub 多租户版 2.2.0 路线讨论](https://github.com/agentscope-ai/QwenPaw/issues/7318)（Issue #7318，评论 23，👍 3）**  
  这是当前社区最热的讨论帖。QwenPaw 最初定位个人 AI 助手，但社区反复要求“为团队运行”的能力。Hub 是官方对此的第一波响应。讨论中关联了 #2324 中关于多用户访问和管理员管理技能的历史诉求。该 Issue 的后续回复将直接成为 2.3/3.0 路线图输入。

- **[任务执行中提交消息触发 409 的讨论](https://github.com/agentscope-ai/QwenPaw/issues/7559)（Issue #7559，评论 5）**  
  用户认为“新消息放入队列”才是合理语义，而不是直接报 `A task is already running...`。这一反馈很快获得 PR [#7577](https://github.com/agentscope-ai/QwenPaw/pull/7577) 响应，是当日“用户体验反馈→修复提案”闭环最快的一例。

## Bug 与稳定性

### 关键（Critical）

| 事项 | 说明 | 状态 |
|---|---|---|
| [#7576](https://github.com/agentscope-ai/QwenPaw/issues/7576) | `RetryChatModel.__init__` 硬编码 `context_size=32768` 后备值，导致所有模型超过约 31130 tokens 即触发 `CONTEXT_UNFIT`。v2.1.0 至 v2.2.0 均受影响。 | **无 fix PR**，建议优先处理 |

### 高（High）

| 事项 | 说明 | 状态 |
|---|---|---|
| [#7559](https://github.com/agentscope-ai/QwenPaw/issues/7559) | 任务执行中通过对话框提交文件/文字返回 409，用户预期应进入消息队列。 | 已有候选修复 [#7577](https://github.com/agentscope-ai/QwenPaw/pull/7577) |
| [#7474](https://github.com/agentscope-ai/QwenPaw/issues/7474) | PR #7337 引入 `ModelInfo.max_tokens` → `max_output_length` 迁移后，Windows 下自定义 provider 配置文件无法加载，启动即报错。 | 已关闭，需确认修复归属哪个版本，并补回归测试 |
| [#7572](https://github.com/agentscope-ai/QwenPaw/issues/7572) | 工具派发层 `_drain()` 用 `except Exception` 吞掉完整异常栈，日志零痕迹，线上排障困难。 | 已有候选修复 [#7578](https://github.com/agentscope-ai/QwenPaw/pull/7578) |

### 中（Medium）

| 事项 | 说明 | 状态 |
|---|---|---|
| [#7574](https://github.com/agentscope-ai/QwenPaw/issues/7574) | `img-gen` skill 的 `openai_images.py` 请求体缺少 `model` 字段，导致 HTTP 503 并回退到 `dall-e-2`。 | 已关闭，修复来源待确认 |
| [#7575](https://github.com/agentscope-ai/QwenPaw/issues/7575) | `edit()` 无条件携带 `response_format`，在 `gpt-image-2` edit 端点触发 HTTP 400。 | 已关闭，修复来源待确认 |
| [#7571](https://github.com/agentscope-ai/QwenPaw/issues/7571) | Agent“遗忘”用户指定的输出目录约束，反复在 A/B/C 多路径生成文件，最终导致未开发的 A 路径代码覆盖了 C 运行时目录，发生一次实质性的源码覆盖事故。 | 开放中，无对应修复 PR |

此外，[#7547](https://github.com/agentscope-ai/QwenPaw/pull/7547) 本身是一条稳定性修复：飞书卡住的消费者会使后续消息被错误丢弃为“already running”，建议尽快并入主分支。

## 功能请求与路线图信号

- **多租户 Hub 与团队管理能力**（[#7318](https://github.com/agentscope-ai/QwenPaw/issues/7318)）：2.2.0 将上线多租户 Hub，社区希望继续看到多用户访问、管理员技能审批、团队级共享能力，预计会成为下一阶段最明确的产品方向。

- **技能版本化与依赖元数据**（[#7557](https://github.com/agentscope-ai/QwenPaw/issues/7557)）：来自 9 个 agent 工作区运维场景。技能目前是裸目录（`SKILL.md` + 文件），无法判断各个 workspace 持有哪个版本，也无法安全更新。该需求和 #7571 的“路径漂移”问题呼应，方向上是把 Skill 从“文件夹”升级为“可版本化产物”。

- **Web UI 会话重塑能力**（[#7573](https://github.com/agentscope-ai/QwenPaw/issues/7573)）：用户希望增加“编辑最后一条消息”和“Rewind”按钮，使对话可修正、可回滚，不必重启整个会话。

- **飞书思考卡片自动折叠**（[#7570](https://github.com/agentscope-ai/QwenPaw/issues/7570)）：用户已用 JSON 2.0 `collapsible_panel` 实现流式期间展开、结束后自动折叠，并验证稳定运行。这是典型的“用户已完成原型，等待官方收编”的增强请求。

- **Advisor Mode**（[#7569](https://github.com/agentscope-ai/QwenPaw/pull/7569)）：PR 提出“更强模型担任 advisor、便宜模型担任 worker”的双模型 loop mode，显示社区正试图在单次会话内做成本/质量分层。

- **审批驱动的技能创建流程**（[#7509](https://github.com/agentscope-ai/QwenPaw/pull/7509)）：Make Skill v2 引入 plan → approval → draft → publish，将技能开发正式化为可管理和审批的流程，符合团队/多租户方向。

## 用户反馈摘要

- **对消息异步化的预期很强**（[#7559](https://github.com/agentscope-ai/QwenPaw/issues/7559)）：用户发现任务运行中提交新消息被 409 拒绝时，第一反应是“不是应该放队列吗”。说明产品语义在用户心智中已经是队列驱动，而非“busy -> reject”。

- **对 Agent 遵守目录约束失去信任**（[#7571](https://github.com/agentscope-ai/QwenPaw/issues/7571)）：用户反复强调“只在 B 目录生成 TODO 文件”，但两天后 A/B/C 三处仍然都出现；“让它只在 A 开发”却最终跑到 C 开发，并触发脚本自动部署覆盖了未开发的 A 代码。这类场景会严重削弱用户对 agent 执行可靠性的信心。

- **对模型上下文硬编码不满**（[#7576](https://github.com/agentscope-ai/QwenPaw/issues/7576)）：用户逐个模型排查才发现 `RetryChatModel` 把所有模型上下文强制按 32768 截断。社区期望 Provider 层应尊重模型自身上限，而不是写死。

- **长思维链占屏影响实际使用**（[#7570](https://github.com/agentscope-ai/QwenPaw/issues/7570)）：飞书流式卡片整体可用（用户认可 #3001 的成果），但 GLM-5.x 等强制思考模型的思考文本过长，输出结束后仍整屏占据，把最终回复挤到很远处。用户已本地实现“自动折叠”并稳定运行，属于满意基础上的体验优化建议。

- **自定义 Provider 升级被“硬断”**（[#7474](https://github.com/agentscope-ai/QwenPaw/issues/7474)）：合并 PR #7337 后旧的 `max_tokens` 字段被直接拒绝，自定义提供商文件无法加载，用户没有平滑迁移提示。升级体验上的破坏性变更需要更清晰的 warning 与 legacy 兼容策略。

## 待处理积压

以下条目需要维护者重点关注：

- **[PR #6874 · MCP tool_call_timeout 可配置超时](https://github.com/agentscope-ai/QwenPaw/pull/6874)**：自 8 月 10 日创建，至今已在 **Under Review** 状态停留近一个月，9 月 5 日仍有更新。建议尽快给出结论或合入计划。

- **[PR #7509 · Make Skill v2](https://github.com/agentscope-ai/QwenPaw/pull/7509)**：9 月 2 日已标记 **Ready for Merge**，仍未合入。该 PR 若持续积压，会与后续社区提交产生冲突成本。

- **[Issue #7318 · Hub 多租户版路线问题](https://github.com/agentscope-ai/QwenPaw/issues/7318)**：已有 23 条评论，社区投入了大量讨论。建议官方在近期输出一份阶段性整理或路线图回复，否则讨论动能会自然流失。

- **4 个 first-time-contributor PR 等待 review**：[#7547](https://github.com/agentscope-ai/QwenPaw/pull/7547)、[#7546](https://github.com/agentscope-ai/QwenPaw/pull/7546)、[#7577](https://github.com/agentscope-ai/QwenPaw/pull/7577)、[#7578](https://github.com/agentscope-ai/QwenPaw/pull/7578)。这四位新贡献者的首次提交如果长期不被处理，会对后续外部贡献形成负向激励。

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

# ZeptoClaw 项目动态日报 — 2026-09-06

## 1. 今日速览

过去 24 小时内 ZeptoClaw 共有 14 条 Issue 更新（10 条活跃、4 条关闭）和 8 条 PR 更新（3 条合并/关闭、5 条仍待合并）。项目今日的核心动作是集中消除了 3 个安全/稳定性的高优先级问题：子进程环境变量泄露、无效 `agent_mode` 回退至最高权限、以及超时子进程树未回收，三组对应修复 PR 均在今日内关闭，安全响应非常迅速。与此同时，维护者 qhkm 此轮基于架构评审生成了一批（#661–#670）P2-high 功能/重构议题，尚未进入排期，项目正处于“安全加固收尾、中期架构工作待启动”的过渡阶段。整体活跃度中等偏高，但 Issues 和 PR 的互动评论较少，目前仍是典型的主导者驱动型开发节奏。

## 2. 版本发布

过去 24 小时无新版本发布，`Releases` 列表为空，暂无开源下载、破坏性变更或迁移注意事项可同步。

## 3. 项目进展

今日有 3 个 PR 被合并/关闭，均围绕安全加固，且全部是“修复与对应 Issue 同日关闭”的高效闭环：

- [PR #672: fix(security): scrub inherited env in plugin/MCP spawn sites (P0 #660)](https://github.com/qhkm/zeptoclaw/pull/672) — 修复插件与 MCP server 子进程继承宿主完整环境的问题，补齐了最后三个未做环境清理的子进程派生点，与运行时层修复形成完整闭环。关联 Issue #660 已关闭。
- [PR #671: fix(security): fail closed on invalid agent_mode (P0 #659)](https://github.com/qhkm/zeptoclaw/pull/671) — 将非法/拼写错误的 `agent_mode` 从回退到 `Autonomous` 改为回退到 `Assistant` 并告警，杜绝未知配置值导致权限升级的隐患。关联 Issue #659 已关闭。
- [PR #645: fix(runtime): scrub subprocess secrets and reap timed-out process trees](https://github.com/qhkm/zeptoclaw/pull/645) — 运行时子进程不再继承 ZeptoClaw 的完整环境变量，同时处理超时场景下的进程树回收问题。该 PR 自 2026-07-23 起经历了较长时间评审，今日正式关闭，并连带促使 #644/#646 两个 P1 问题被识别和关闭。

向前的整体判断：两个 P0 安全发现（#659、#660）在同一天内完成修复并关闭，说明项目对安全评审的跟进效率高；从 #645 的合并时间跨度看，跨模块的运行时改造仍需要较为谨慎的评审周期。

## 4. 社区热点

今日讨论热度本身并不高，评论集中在与安全/CI 相关的关闭议题上：

- [Issue #646: [CLOSED] chore(ci): restore Clippy and cargo-deny checks on current toolchain](https://github.com/qhkm/zeptoclaw/issues/646) — 评论数 3，是今日最活跃的 Issue。该问题由 #645 暴露：Rust 1.97.1 对 channel/provider/binary-plugin 既有代码报出 5 个新 Clippy 警告，同时 `cargo-deny` 检出 `quick-xml 0.39.2` 和 `lopdf 0.40.0` 的已知漏洞版本。社区/维护者的诉求很明确：希望 CI 安全检查回到绿线，同时尽快升级或替换含漏洞的依赖。
- [Issue #644: [CLOSED] bug(safety): scrub subprocess environments and terminate process trees on timeout](https://github.com/qhkm/zeptoclaw/issues/644) — 1 条评论，核心痛点是“模型生成的 shell 命令可能拿到与任务无关的生产者密钥”。

总体来看，热点集中在安全与工程健康度方面，功能性话题讨论较少；这与项目当前处于安全整改期的阶段特征一致。

## 5. Bug 与稳定性

今日关闭的问题按严重程度排列如下：

| 严重程度 | Issue | 状态 | 是否已有 Fix PR |
|---|---|---|---|
| P0 安全 | [#660 [CLOSED] Centralize child-process env scrubbing across all spawn sites](https://github.com/qhkm/zeptoclaw/issues/660) | 已关闭 | 是，[#672](https://github.com/qhkm/zeptoclaw/pull/672) |
| P0 安全 | [#659 [CLOSED] Fail closed on invalid agent_mode — never fall back to Autonomous](https://github.com/qhkm/zeptoclaw/issues/659) | 已关闭 | 是，[#671](https://github.com/qhkm/zeptoclaw/pull/671) |
| P1 安全 | [#644 [CLOSED] bug(safety): scrub subprocess environments and terminate process trees on timeout](https://github.com/qhkm/zeptoclaw/issues/644) | 已关闭 | 是，[#645](https://github.com/qhkm/zeptoclaw/pull/645) |
| P1 依赖/CI | [#646 [CLOSED] chore(ci): restore Clippy and cargo-deny checks on current toolchain](https://github.com/qhkm/zeptoclaw/issues/646) | 已关闭 | 无直接对应 PR，建议维护者说明修复方式 |

值得关注的是 #646：虽然状态已关闭，但数据中没有看到对应的 fix PR，而它涉及的 `quick-xml 0.39.2` 与 `lopdf 0.40.0` 漏洞版本若不升级，`cargo-deny` 会持续报错，后续依赖 PR 的合入可能仍受影响。今日没有新增的崩溃或回归类 Bug 报告。

## 6. 功能请求与路线图信号

今日没有来自外部用户的新功能请求。值得注意的信号是一批由维护者 qhkm 发起的、具有明确路线图指向的开放议题（#661–#670），均为 P2-high，多数引用了 2026-09-06 的 Hermes 对比评审报告。这些议题揭示了比“功能新增”更重要的架构演进方向：

- [#665 [RFC] Cron Job v2 — completion ack, run ledger, operational control](https://github.com/qhkm/zeptoclaw/issues/665) — 计划的“成功”语义目前只是“任务被调度执行”，缺少执行结果的确认与运行台账；若纳入 v2，将显著提升定时任务的可靠性。
- [#667 Footprint Ladder + registry-owned extension metadata (Extension Host v2)](https://github.com/qhkm/zeptoclaw/issues/667) — 解决可扩展性增长导致的二进制体积和注册中心负担，是面向“扩展生态”的重构。
- [#661 [RFC][L][perf] Byte-stable Prompt Envelope contract (system prompt is prompt-cache hostile)](https://github.com/qhkm/zeptoclaw/issues/661) — 系统提示词每轮重建且混入时间、动态记忆内容，对 prompt cache 极不友好；此问题被标记为最大的架构级性能缺口。
- [#662 [L][channels] Complete the channel-plugin protocol](https://github.com/qhkm/zeptoclaw/issues/662) — 插件 channel 目前是“只出不进、发后即忘”的适配器，与 README 宣称的能力存在差距。
- [#666 [M][memory] Durable cross-session recall and transactional memory writes](https://github.com/qhkm/zeptoclaw/issues/666)、[#670 [S] Config source opacity](https://github.com/qhkm/zeptoclaw/issues/670) — 分别指向记忆系统的跨会话持久化与配置项来源透明度。

这些议题若被纳入排期，回答的是“架构如何支撑下一个阶段”，优先于单个用户功能请求。

## 7. 用户反馈摘要

今日 Issue/PR 评论中暂未看到第三方用户的声音，评论互动基本集中在维护者 qhkm 发起的审计与 CI 类问题上。能够提炼出的真实痛点和诉求主要来自维护者视角对系统现状的陈述：

- 安全隐患实际存在：子进程（含插件、MCP server）会继承完整环境变量，存在将 API Key、数据库 URL 等生产机密暴露给模型生成的命令的现实风险（[#644](https://github.com/qhkm/zeptoclaw/issues/644)、[#660](https://github.com/qhkm/zeptoclaw/issues/660)）。
- 配置安全性设计有反直觉行为：未知的 `agent_mode` 字符串此前会回退到 `Autonomous` 最大权限，用户一次拼写错误即可获得超出预期的权限（[#659](https://github.com/qhkm/zeptoclaw/issues/659)）。该问题已修复。
- 生产环境与代码注释不一致：注释宣称“未来将切换到 CoreLoop”，但生产环境仍然运行 5,227 行的 `AgentLoop`（[#663](https://github.com/qhkm/zeptoclaw/issues/663)），维护者认为这是当前最主要的架构错配。

整体上，今日反馈来源清一色来自项目所有者，尚缺乏外部用户的独立声音；可理解为当前项目仍以内部架构驱动为主，但公开的审计文档和议题为外部贡献者预留了很好的讨论入口。

## 8. 待处理积压

- **依赖机器人 PR 已滞留超 3 个月**：以下 5 个 Dependabot PR 均创建于 2026-06-03，且今天仍显示“待合并”，建议维护者尽快处理，避免依赖陈旧：
  - [PR #627: chore(deps): bump serde_json from 1.0.149 to 1.0.150](https://github.com/qhkm/zeptoclaw/pull/627)
  - [PR #625: chore(deps): bump rpassword from 7.4.0 to 7.5.2](https://github.com/qhkm/zeptoclaw/pull/625)
  - [PR #623: chore(deps): bump tokio from 1.52.1 to 1.52.3](https://github.com/qhkm/zeptoclaw/pull/623)
  - [PR #620: chore(deps): bump scraper from 0.26.0 to 0.27.0](https://github.com/qhkm/zeptoclaw/pull/620)
  - [PR #617: chore(deps): bump tower-http from 0.6.10 to 0.6.11](https://github.com/qhkm/zeptoclaw/pull/617)

- **新一批架构评审议题尚无排期响应**：#661–#670 共 10 个议题自 2026-09-05 创建后，除少数几个昨日被标记关闭外，其余在当前数据中均没有评论或指派更新。其中 [#662](https://github.com/qhkm/zeptoclaw/issues/662)（channel-plugin 协议）、[#663](https://github.com/qhkm/zeptoclaw/issues/663)（Agent Pipeline 迁移）被标为 L 级大工程，建议维护者尽快评估优先级并把它们纳入公开路线图，否则会持续形成“知道该做什么但没有排期”的积压压力。

- **Issue #646 的修复方式待说明**：[#646](https://github.com/qhkm/zeptoclaw/issues/646) 虽然已关闭，但依赖漏洞（quick-xml、lopdf）和 Clippy 警告没有直接对映到可见的修复 PR。在 `cargo-deny` 的检查下，相关依赖的升级 PR 后续很可能会继续受阻，建议维护者对关闭方式做一次注释说明。

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目日报 — 2026-09-06

## 1. 今日速览

ZeroClaw 在过去 24 小时保持高强度迭代：共产生 **42 条 Issue 更新**（新开/活跃 34 条、关闭 8 条）与 **50 条 PR 更新**（44 条待合并、6 条已合并/关闭），并正式发布 **v0.8.5**。Issue 侧仍以 RFC/架构设计讨论为主导，多个大型 RFC（如 #9487、#9488）进入第 5、第 10 次修订，说明项目正处于架构升级的关键期；PR 侧则覆盖了 Telegram 审批流、Windows CI 测量、Gemini provider 修复等多个方向。值得关注的是，PR 队列仍积压大量 `needs-author-action`（等待作者回复）与 `needs-maintainer-review`（等待维护者评审）的高风险大 PR，维护者评审带宽可能是当前主要瓶颈。整体来看，项目在版本发布节奏、社区讨论活跃度与提交密度上均表现健康，但大 PR 的合并周期偏长是一个需要警惕的信号。

---

## 2. 版本发布

### v0.8.5 — security, connectivity, and operator-experience release
**链接**: https://github.com/zeroclaw-labs/zeroclaw/releases/tag/v0.8.5

- **规模**: 454 commits，73 位贡献者
- **核心新特性**:
  - 引入 **ZeroRelay** 与 **ZeroRouter** 两个新组件，推测与网关连接与路由能力相关（数据中未给出完整 Release Notes，此处基于简介推断）
  - 扩展 live chat 与 provider 能力
  - 对 plugin、sandbox、webhook、credential 与 file 边界进行了安全加固
- **注意事项**: 数据源未提供 Breaking Changes 与迁移指南详情。鉴于该版本涉及安全边界加固，建议升级前查看完整 Release Notes 确认配置项是否变动。当前 Issue 队列中 #10048（Rust 1.98.0 本地 CI 验证）已关闭，说明发布前的 CI 验证已就绪；但 #9459（v0.8.5 稳定化跟踪）仍处于 Open 状态，可能包含少量发布后收尾工作。

---

## 3. 项目进展

今日 6 条 PR 被合并/关闭，值得关注的已合入工作包括：

| PR | 内容 | 状态说明 |
|---|---|---|
| [#10435](https://github.com/zeroclaw-labs/zeroclaw/pull/10435) | **fix(providers): preserve model context when anchoring Gemini requests** — 修复 Gemini provider 在处理请求时丢失模型上下文的问题 | 已关闭（合入） |
| [#10064](https://github.com/zeroclaw-labs/zeroclaw/pull/10064) | **fix(channels/telegram): self-destruct approval cards after an operator tap** — Telegram 审批卡片在操作后自动销毁，修复 pending approval 状态残留 | 已关闭（合入） |
| [#10350](https://github.com/zeroclaw-labs/zeroclaw/pull/10350) | **ci(tests): measure affected Windows tests on pull requests** — 在 PR 上增加 Windows 测试耗时的测量型任务（advisory，不阻塞 CI gate） | 已关闭 |
| [#10005](https://github.com/zeroclaw-labs/zeroclaw/pull/10005) | **fix(channels): base channel health on the channel, not on listener liveness** — `/health` 现在基于真实连接状态而非 listener 存活状态报告 channel 健康 | 已关闭（合入） |
| [#5230](https://github.com/zeroclaw-labs/zeroclaw/pull/5230) | **feat(plugins): add WASM plugin system with security sandbox** — 自 4 月开启的巨型 PR（WASM 插件系统）今日关闭，具体合入状态待确认 | 已关闭 |

同时有多条与代码推进直接关联的 Issue 被关闭：#9593（**TaskRecord 作为后台委派的单一生命周期 owner** 的重构）、#7910（Windows self-update 测试覆盖）、#10282（hardware probe feature 传播）、#10045（Persisted image marker 临时路径问题）、#10048（Rust 1.98.0 CI 验证）、#7911（Termux 安装选择器）等，说明这些方向的代码修复已完成并合入。

**整体判断**: 项目今日在渠道健康检查、Telegram 审批体验、Windows 测试基建与 Gemini provider 修复四个方向有实质推进；WASM 插件系统巨型 PR 的关闭（无论合入或拒绝）意味着这一长期悬而未决的架构议题可能迎来阶段性结论。

---

## 4. 社区热点

今日讨论热度最高的议题集中在 **RFC 架构设计**与**治理流程**两个方向：

### 4.1 高讨论量 RFC（长期悬而未决）

- **[#9487 — RFC: Runtime-owned conversation sessions and transport surface adapters](https://github.com/zeroclaw-labs/zeroclaw/issues/9487)**（33 条评论）
  由 NiuBlibing 提出，历经 5 次修订。该 RFC 主张将对话会话从信道层解耦为运行时自有资源，并引入"传输面适配器"。第 5 次修订完全替换了 Rev 4 的投票快照，意味着此前针对 Rev 4 的投票作废，需要重新进入讨论窗口。这反映出该项目对**核心架构变更**的决策非常审慎。
- **[#9488 — RFC: Unified file and attachment architecture for conversation surfaces](https://github.com/zeroclaw-labs/zeroclaw/issues/9488)**（26 条评论）
  与 #9487 同源（均为 NiuBlibing 提交），目前处于 Rev 10。该 RFC 试图统一所有对话表面的文件与附件处理架构。值得注意的是，该 RFC 与今日关闭的 #10045（image marker 临时路径问题）有直接关联，说明基础设施重构正在被实际 Bug 驱动。

### 4.2 治理流程与维护者关注

- **[#6808 — RFC: Work Lanes, Board Automation, and Label Cleanup](https://github.com/zeroclaw-labs/zeroclaw/issues/6808)**（24 条评论，state: Ratified / rollout in progress）
  已批准、处于实施中的治理类 RFC。讨论围绕让工作路由更自动化、减少维护者手动管理开销。
- **[#10549 — RFC: Simplify RFC voting](https://github.com/zeroclaw-labs/zeroclaw/issues/10549)**（2 条评论，2026-09-02 创建）
  新提出的治理改进提案：取消强制讨论等待窗口（普通 RFC 48 小时/特殊 72 小时），并让 REVISE 立即停止当前快照。该提案若通过，将大幅加快项目 RFC 流转速度。

### 4.3 高关注度 PR

- **[#10321 — feat(security): browser PKCE and the cross-surface enrollment API](https://github.com/zeroclaw-labs/zeroclaw/pull/10321)** — 安全领域的大规模堆叠式 PR（依赖 #10275 → #10274 → #10270 等一串前置），采用 browser PKCE 与跨端注册 API。
- **[#10241 — fix(channels): restore supervised shell approval routing](https://github.com/zeroclaw-labs/zeroclaw/pull/10241)** — 修复渠道驱动监督式 shell 调用的审批路径，被标记为 `blocked` 状态，说明有外部依赖或决策阻塞。

**诉求分析**: 高讨论量 Issue 背后的核心诉求有两类：(1) **架构层面对会话与文件模型做统一化、运行时化的重构**，以解决当前多信道/多工具拼接带来的复杂度；(2) **降低维护者的治理负担**，通过简化投票流程、自动化 Label 管理来加速决策。这反映了项目在规模扩大后对流程效率的内在需求。

---

## 5. Bug 与稳定性

按严重程度排列今日新增/活跃的 Bug（标注是否已有 Fix PR）：

### S1 — 工作流阻塞

- **[#10536 — [Bug]: macOS Seatbelt ignores configured allowed_roots for shell commands](https://github.com/zeroclaw-labs/zeroclaw/issues/10536)**（2026-09-02 创建，P1，in-progress）
  影响 macOS 用户：即使风险配置文件设置了 `allowed_roots`，shell 命令仍收到 `Operation not permitted`。S1 严重度 + 已标记 in-progress，暂无对应 PR。

### S2 — 功能降级

- **[#10533 — [Bug]: model_routing_config rejects custom.* (and other valid) provider slots](https://github.com/zeroclaw-labs/zeroclaw/issues/10533)**（P1，in-progress）
  配置工具校验器与真实配置 schema 不一致，`custom.*` 等合法 provider 槽被拒绝。
- **[#10534 — [Bug]: bounded delegates silently strip the delegate tool](https://github.com/zeroclaw-labs/zeroclaw/issues/10534)**（P2）
  bounded 模式的 delegate 不管配置如何总是被去掉 `delegate` 工具，与 `delegation_policy/max_delegation_depth` 配置矛盾。
- **[#10532 — [Bug]: degraded-config remediation can invoke a different binary](https://github.com/zeroclaw-labs/zeroclaw/issues/10532)**（P2）
  降级配置提示建议运行的 `zeroclaw config migrate` 可能与当前实际运行 daemon 的二进制不同。
- **[#10625 — Internal `[media attachment]` placeholder is delivered to users when a non-vision model is in use](https://github.com/zeroclaw-labs/zeroclaw/issues/10625)**（P2，2026-09-04 创建）
  text-only 模型场景下，历史消息中的媒体标记降级为字面量 `[media attachment]` 并直接展示给用户。
- **[#10626 — TTS synthesizes text verbatim: Markdown and emoji are spoken aloud](https://github.com/zeroclaw-labs/zeroclaw/issues/10626)**（P2，2026-09-04 创建）
  TTS 直接朗读 Markdown 语法与 emoji 名称，缺少预处理净化。

### S3 — 轻微问题

- **[#10585 — [Bug]: new log sink regression races migration tests under the default parallel runner](https://github.com/zeroclaw-labs/zeroclaw/issues/10585)**（P2）
  新增 log sink（#10203 引入）在默认并行测试下与 migration tests 发生锁竞争，导致偶发失败。

### 今日合入的 Bug 修复

- [#10435](https://github.com/zeroclaw-labs/zeroclaw/pull/10435) 修复 Gemini 请求上下文锚定问题
- [#10064](https://github.com/zeroclaw-labs/zeroclaw/pull/10064) 修复 Telegram 审批卡片残留
- [#10005](https://github.com/zeroclaw-labs/zeroclaw/pull/10005) 修复 channel 健康检查误报

**总体判断**: 今日新出现 Bug 数量不多（约 5–6 个），但多个 S2/S1 级别的安全/沙箱相关 Bug 处于已确认但尚在修复中的状态。#10536（macOS Seatbelt）、#10533（配置校验不一致）等均带有 `status:in-progress` 标签，应优先跟进。

---

## 6. 功能请求与路线图信号

### 6.1 今日新增功能请求

- **[#10641 — [Feature] [Web]: Per-field cron schedule input](https://github.com/zeroclaw-labs/zeroclaw/issues/10641)**（2026-09-05 创建，help wanted，P2）
  Web 端 Cron 弹窗的 Schedule 字段目前为自由文本输入（`web/src/pages/Cron.tsx`），用户要求改为分字段输入（如分别填分/时/日/月/周）并提供校验与人类可读确认。这表明用户对易用性的要求在提升，属于低成本高收益的 UI 改进。

### 6.2 与路线图关联的新信号

- **[#10530 — [Feature]: Pass Anthropic extended-thinking params through OpenAI-compatible providers](https://github.com/zeroclaw-labs/zeroclaw/issues/10530)**（2026-09-02 创建）
  通过 OpenAI-compatible 网关（wire_api="chat_completions"）访问 Claude 模型的用户希望能透传 extended-thinking 参数。这与此前 #10063 的"Anthropic-backed compatible gateways"方向一致，说明 ZeroClaw 的多 provider 兼容层正在成为真实用户的关键路径。未看到对应 Fix PR。
- **[#10526 — RFC: Append-only session event history, deterministic state replay, and derived agent streams](https://github.com/zeroclaw-labs/zeroclaw/issues/10526)**（2026-09-01 创建）
  计划将可变消息持久化改为 append-only 事件历史 + 确定性状态重放。该 RFC 已被 #10076（WASM 插件运行时）声明为独家权威，说明项目正在向"事件溯源"架构演进。

### 6.3 可能被纳入下一版本的功能

- **[#10050 — RFC: Verbatim channel send over the gateway, without an agent turn](https://github.com/zeroclaw-labs/zeroclaw/issues/10050)**（已 accepted）
  网关目前有 47 个 `/api/*` 路径，但没有一个能直接在指定 channel 上发送调用方原样消息（不经过 agent turn）。该 RFC 已获接受，属于明确的路线图内功能。
- **[#10222 — RFC: Opt-in single-tool provider rounds for interactive agents](https://github.com/zeroclaw-labs/zeroclaw/issues/10222)**（已 accepted）
  在工具批次之间归还控制权给模型，而不是一次收集全部工具调用后统一执行。

**判断**: Web UI 易用性、多 provider 兼容层能力补齐、"事件溯源"架构转型是当前三大方向信号。若 #10549（简化 RFC 投票）通过，上述 RFC 的落地速度有望加快。

---

## 7. 用户反馈摘要

以下从 Issue 中提炼真实用户的声音：

- **Termux/Android 用户安装受阻（#7911，已关闭）**
  > "我试图在 Android/Termux 上安装 ZeroClaw——无论是预编译二进制还是本地编译，都会选中错误的 unknown linux aarch64 二进制。"
  
  *反馈：该问题已标记为关闭，修复已合入。*

- **macOS 用户被沙箱策略卡死（#10536，S1）**
  > "即使配置了 `allowed_roots`，shell 命令仍收到 `Operation not permitted`。"
  
  *反馈：S1 级别阻塞性 Bug，用户在 macOS 上无法按预期执行 shell 工具，目前仍 in-progress。*

- **TTS 体验粗糙（#10626，S2）**
  > "自托管部署中，一段回复会朗读出 `**bold**` 这样的 Markdown 标记并逐字念出 emoji 名称。"
  
  *反馈：用户期望 TTS 输出更自然，涉及渠道文本预处理。*

- **非视觉模型的降级路径不优雅（#10625，S2）**
  > "当文本模型遇到历史消息中无法识别的媒体标记，用户会直接看到字面量 `[media attachment]`。"
  
  *反馈：暴露了模型能力协商与消息降级策略之间的衔接不足。*

- **社区对 RFC 流程的普遍不满（#10549）**
  > "在某些情况下，计时器并不会带来更多 review，只是拖慢了所有人的节奏。"
  
  *反馈：来自项目内部活跃贡献者对治理流程的吐槽，说明 RFC 讨论-投票机制需要轻量化。*

---

## 8. 待处理积压

以下为长时间未解决或今日仍未获处理的高价值 Issue/PR：

### 8.1 长期未决的核心 RFC（已超过 40 天）

- **[#9487](https://github.com/zeroclaw-labs/zeroclaw/issues/9487)**（2026-07-28 创建，33 评论，需维护者评审）— 会话运行时架构 RFC，已重大修订 5 次，进入第 5 修订讨论窗口。
- **[#9488](https://github.com/zeroclaw-labs/zeroclaw/issues/9488)**（2026-07-28 创建，26 评论，需维护者评审）— 统一文件/附件架构 RFC，已修订 10 次。
- **[#6996](https://github.com/zeroclaw-labs/zeroclaw/issues/6996)**（2026-05-28 创建，24 评论，需维护者评审）— 文件系统沙箱策略的 granular sandbox RFC，悬置已超过 3 个月。

### 8.2 维护者决策队列

- **[#8692 — Maintainer decision queue for RFCs and design issues](https://github.com/zeroclaw-labs/zeroclaw/issues/8692)**（15 评论）
  这是项目专门用于跟踪待维护者决策的队列，建议关注此 Issue 以了解哪些 RFC 在等最终决定。

### 8.3 需要作者行动的 PR（可能阻塞合并）

以下 PR 被标记为 `needs-author-action`，已有一段时间未推进：
- [#10391](https://github.com/zeroclaw-labs/zeroclaw/issues/10391)（delegate 文件系统工具修复，8 月 26 日最后更新）
- [#8966](https://github.com/zeroclaw-labs/zeroclaw/issues/8966)（provider identity 透传，7 月 11 日创建，超大 PR）
- [#10485](https://github.com/zeroclaw-labs/zeroclaw/issues/10485)（zerocode clipboard 清理，8 月 30 日创建）
- [#9320](https://github.com/zeroclaw-labs/zeroclaw/issues/9320)（cron 任务 wall-clock 超时，7 月 23 日创建）

### 8.4 需要注意的长期开放 PR

- **[#5230 — feat(plugins): add WASM plugin system with security sandbox](https://github.com/zeroclaw-labs/zeroclaw/pull/5230)**（2026-04-02 创建至今，跨度 5 个月）今日状态变为 CLOSED，但需要确认是合入还是关闭。若未合入，意味着 WASM 插件生态仍无落地方案。

### 8.5 被阻塞且高风险

- [#10241](https://github.com/zeroclaw-labs/zeroclaw/pull/10241)（supervised shell approval routing，`blocked`）
- [#10356](https://github.com/zeroclaw-labs/zeroclaw/pull/10356)（AnySearch web search provider，`blocked` + `do-not-merge`）
- [#9997](https://github.com/zeroclaw-labs/zeroclaw/issues/9997)（Telegram 安全 model picker，`blocked` + `do-not-merge`）

---

**数据说明**: 本报告基于 2026-09-06 提供的 GitHub 数据快照生成，Issue/PR 的"今日更新"以数据中的 `更新日期` 为 2026-09-05 至 2026-09-06 为准。部分最新 Release（v0.8.5）的详细变更日志未覆盖完全，建议直接查看 Release 页面获取完整信息。

</details>

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*