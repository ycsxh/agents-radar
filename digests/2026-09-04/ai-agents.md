# OpenClaw 生态日报 2026-09-04

> Issues: 500 | PRs: 500 | 覆盖项目: 13 个 | 生成时间: 2026-09-04 04:02 UTC

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

# OpenClaw 项目动态日报 — 2026-09-04

---

## 1. 今日速览

过去 24 小时，OpenClaw 仓库保持 **极高活跃度**：共产生 500 条 Issue 更新（新开/活跃 339 条，关闭 161 条）与 500 条 PR 更新（待合并 415 条，已合并/关闭 85 条），并发版 `v2026.9.1`。值得关注的是，新版本发布后迅速出现多条 **Windows 平台 Gateway 启动失败** 的 P0 级回归报告（#137813），同时 DeepSeek 缓存命中率、SQLite 存储无限增长等老问题仍在持续发酵。项目整体呈现"高频迭代 + 新版本引入新回归 + 存储/进程稳定类问题积压"的健康度分化态势——功能推进节奏快，但稳定性债务值得警惕。

---

## 2. 版本发布

### v2026.9.1（9月4日发布）

**主要亮点：**

- **对话中渲染图表**：Mermaid 代码块现在可在 Control UI 以及原生 macOS、iOS、Android 应用中渲染为图表，支持放大预览，移动端失败时提供重试。（#134913, #135746, #135470, #135342）
- **安装到对话的体验优化**（release notes 截断，后续更新待查）

**破坏性变更/迁移注意事项：**

- ⚠️ **Windows 用户升级后 Gateway 可能无法启动**：2026.9.1 重新生成了 `~/.openclaw/gateway.cmd` 并新增 `--task-supervisor` 参数。已有用户报告该进程**静默退出（exit 0）、子进程从未生成**，导致 Scheduled Task 方式运行的 Gateway 完全不可用（见 #137813）。由于 Service 由 `gateway.vbs` 启动该 `.cmd`，此问题影响所有 Windows Scheduled Task 部署。建议 Windows 用户在官方修复前**暂缓升级**。
- ⚠️ 有用户报告升级后 `codex` 插件出现 `dist/extensions/codex missing node_modules` 报错（#135970，当日已修复关闭）。

---

## 3. 项目进展

今日已合并/关闭的 PR 与对应 Issue 反映了项目在 **多个方向同步推进**：

- **Codex 插件安装缺陷修复**：[#135970](https://github.com/openclaw/openclaw/issues/135970)（P1）—— Managed Codex app-server 二进制缺失导致所有 openclaw 工具调用失败。已修复关闭，对 Codex 重度用户是重要恢复。
- **OAuth MCP 服务器在 claude-cli runtime 上不可用**：[#134307](https://github.com/openclaw/openclaw/issues/134307)（已关闭）—— `auth: "oauth"` 类型 MCP 服务器此前在 claude-cli runtime 会话中完全不出现。
- **Doctor 维护流程修复**：
  - [#134938](https://github.com/openclaw/openclaw/issues/134938)（P1）：`doctor --fix` 在 legacy exec-approvals gate 上死锁，阻塞多项核心迁移 —— 已修复。
  - [#137377](https://github.com/openclaw/openclaw/issues/137377)（P1）：Windows 上 `doctor --fix` 终局重启必失败，Scheduled Task 被遗留为禁用态 —— 已修复。
- **Discord/Codex 运行时**：`message` 工具作为终态工具导致进度更新静默终结回合的问题 [#106961](https://github.com/openclaw/openclaw/issues/106961) 已修复——对 Discord 上使用 Codex runtime 的用户体验有直接改善。
- **配置备份可靠性**：[#106581](https://github.com/openclaw/openclaw/issues/106581)（P2）—— 单文件读取错误导致备份快照永远阻塞，现已修复。

此外，**35 条新 PR 等待维护者查看**，其中一批标注为 `ready for maintainer look`，信号积极。综合来看，今日项目推进主要围绕**安装可靠性、Codex runtime 稳定性与工具链修复**展开，涉及面广但以缺陷修复为主，暂无重大新功能合入。

---

## 4. 社区热点

今日热帖集中在 **数据存储膨胀、子进程泄漏与 DeepSeek 缓存效率** 三方面——反映真实生产用户最关心的「长期运行稳定性」问题：

| Issue | 标题 | 评论/点赞 | 核心诉求 |
|---|---|---|---|
| [#94518](https://github.com/openclaw/openclaw/issues/94518)（已关闭） | DeepSeek 6.x 升级后缓存命中率 <10%，边界感知缓存破坏前缀匹配 | 💬 11 / 👍 10 | 升级后 API 成本升高，社区最关注的话题之一 |
| [#114612](https://github.com/openclaw/openclaw/issues/114612)（开放） | memory-core SQLite 无限增长：`memory_index_chunks` + `memory_embedding_cache` 无保留策略 | 💬 11 / 👍 0 | 生产实例磁盘即将被填满，需尽快介入 |
| [#97616](https://github.com/openclaw/openclaw/issues/97616)（开放） | hook/tool 子进程泄漏，僵尸进程堆积 | 💬 10 | 长时间运行导致性能退化 |
| [#96007](https://github.com/openclaw/openclaw/issues/96007)（开放） | Discord 多段回复中出现错误文本时后续内容被丢弃 | 💬 9 | 消息丢失类问题，触及渠道可靠性 |

**信号解读**：社区最不满的是**存储无上限增长**与**进程泄漏**这两颗"定时炸弹"——它们不导致立即崩溃，但会随着运行时间推移逐步侵蚀系统可用性。DeepSeek 缓存问题获 10 👍 说明 **API 成本控制**对用户有极高敏感度。P2 且长期未修的 [SQLite 膨胀问题 #114612](https://github.com/openclaw/openclaw/issues/114612) 应引起维护者重视。

---

## 5. Bug 与稳定性

按严重度整理如下（🔥= 已有修复 PR / fix-shape-clear）：

| 严重度 | Issue | 描述 | 状态 |
|---|---|---|---|
| **P0 🔥** | [#137813](https://github.com/openclaw/openclaw/issues/137813) | **2026.9.1 Windows Gateway 完全无法启动**：新增 `--task-supervisor` 参数静默退出 | 开放，无修复 PR |
| **P0** | [#136203](https://github.com/openclaw/openclaw/issues/136203) | Windows de-DE 2026.8.2 升级后 Doctor 维护被阻塞 | 开放，fix-shape-clear |
| **P0** | [#126821](https://github.com/openclaw/openclaw/issues/126821) | SQLite 损坏在纯净重建的 DB 上 **15–24 小时内复发**（WSL2），含 "paralyzed gateway" 模式 | 开放 |
| **P0 🔥** | [#123327](https://github.com/openclaw/openclaw/issues/123327) | 共享状态 WAL checkpoint 将索引页复制到 SQLite page 1（树莓派 ext4 上两次损坏） | 开放 |
| **P0** | [#107694](https://github.com/openclaw/openclaw/issues/107694) | Gateway 启动因严格 migration warning 守卫而失败（P0） | ✅ 已关闭 |
| **P1 🔥** | [#137710](https://github.com/openclaw/openclaw/issues/137710) | Codex 子进程完成后不唤醒 sessions_yield 父进程 | 开放，fix-shape-clear |
| **P1 🔥** | [#127239](https://github.com/openclaw/openclaw/issues/127239) | deepseek-v4-flash 上下文窗口静默回退为 200k 硬编码，忽略 1M 真实值 | 开放，fix-shape-clear |
| **P1** | [#123799](https://github.com/openclaw/openclaw/issues/123799) | Codex compact 404 生产影响，需要安全升级/回滚指引 | 开放 |
| **P1** | [#126906](https://github.com/openclaw/openclaw/issues/126906) | 拒绝 write 工具后**静默禁用 memory 持久化**，agent 仍报告成功 | 开放 |
| **P1** | [#97616](https://github.com/openclaw/openclaw/issues/97616) | hook/tool 子进程泄漏 → 僵尸堆积（长期运行） | 开放 |
| **P1 🔥** | [#118185](https://github.com/openclaw/openclaw/issues/118185) | 同一 agent turn 被写入 transcript **两次**且内容不一致 | 开放，有链接 PR |
| **P1** | [#127148](https://github.com/openclaw/openclaw/issues/127148) | Codex sessions.compact 获取第二个 app-server，触发 active-writer 冲突 | 开放 |
| **P1** | [#136183](https://github.com/openclaw/openclaw/issues/136183) | Command executor 执行 ssh 时挂起（2026.8.1 回归，2026.8.2 仍存在） | 开放 |
| **P1** | [#135347](https://github.com/openclaw/openclaw/issues/135347) | 强制 memory reindex 将共享 agent DB 膨胀到 35GB 并阻止 Gateway 启动 | 开放 |
| **P2 🔥** | [#125640](https://github.com/openclaw/openclaw/issues/125640) | memory index 在 item-count 批量限制下仍失败（千帆/火山）—— 旧修复未生效 | 开放 |

**稳定性判断**：当日新报告的 P0/P1 集中于 **Windows 升级路径**（#137813）与**SQLite/存储损坏**两类；前者影响面大、属新版本回归，预计会快速修复；后者已是反复出现的系统性隐患，涉及 WAL 检查点、共享状态、DB 膨胀等多个侧面，项目组需考虑统一的存储层加固方案。好消息是多个 P0/P1 已有明确修复方向（fix-shape-clear / linked-pr-open）。

---

## 6. 功能请求与路线图信号

| 请求 | 描述 | 信号 |
|---|---|---|
| [#72741](https://github.com/openclaw/openclaw/issues/72741) | 外部安全/护栏检查的标准接口，便于第三方系统接入 agent 动作审计 | P2、enhancement、security；呼应企业采用需求 |
| [#127208](https://github.com/openclaw/openclaw/issues/127208) | 新增一次性 `/followup <message>` 命令，不改变会话队列模式 | P2 enhancement，与现有 /queue 互补 |
| [#126781](https://github.com/openclaw/openclaw/issues/126781) | 让 `/loop` 与 Automations 启动持久 Lobster workflow，返回稳定 flow ID | 对自动化能力有进阶需求，已有讨论 |
| [#137872](https://github.com/openclaw/openclaw/issues/137872) | 让 policy-bound prompt hooks 能枚举当前回合的授权工具名 | 新提交，方向明确 |
| [#132781](https://github.com/openclaw/openclaw/issues/132781) | 无 narration 时，用最新 commentary 作为进度草稿标签 | P3 enhancement，小体验改进 |
| [#116716](https://github.com/openclaw/openclaw/issues/116716) | context engine 支持可选的 strict failure 策略 | 涉及部署可用性与可预期性取舍 |

**路线图判断**：结合在途 PR，以下方向有较大概率进入下一版本：

- **故障域隔离 / provider resilience**：[#137882](https://github.com/openclaw/openclaw/pull/137882)（翻译模型选择保持私有）与 [#137853](https://github.com/openclaw/openclaw/pull/137853)（fallback summary 保留个人 cooldown）——关注 provider 故障场景下的安全边界。
- **会话生命周期管理增强**：PR #137863（Slack 原生会话控制路由）与 [#137789](https://github.com/openclaw/openclaw/pull/137789)（分离父子会话 UI 活动）均在推进中，会话系统正在精细化。
- **内存/存储可观测性**：PR [#137876](https://github.com/openclaw/openclaw/pull/137876) 将暴露 memory 存储用量指标并引导安全磁盘恢复——直接呼应 #114612 等热帖，预计该方向会持续投入。

---

## 7. 用户反馈摘要

- **DeepSeek 缓存命中率是成本敏感用户的核心痛**（#94518, 👍10）：升级 6.x 后命中率从高位跌至 10% 以下，"每 API 调用都在烧钱"成为最直接体感，用户对缓存策略变更（boundary-aware caching）的兼容性表示强烈不满。该帖虽已关闭，但值得自查是否在后续版本真正修复。
- **升级 = 冒险**：Windows 用户 #137813 反馈升级 2026.9.1 后服务完全起不来，"我唯一做的就是 npm 更新"；#137377 反馈 doctor 修复流程本身成为阻塞点——用户对升级路径的平滑度有明显不满。
- **存储无限增长引发生产焦虑**：多个用户报告 agent DB 以不可控速度膨胀（#114612 无保留策略、#135347 达 35GB、#123327 数据库损坏），且"使用 `memory reset` 后文件仍占分配空间"——用户对存储管理的透明度和工具支持有明确需求。
- **静默失败比报错更让用户不安**：写入被 deny 后 agent 仍报告成功（#126906）、model 上下文窗口静默回退（#127239）、进程"消失了但没退出"（#137813）——多起 Issue 共同显示用户对**无声的错误吞没**的容忍度极低。
- **正面反馈**：已关闭的 #106961（Discord 消息工具终结回合）得到 3 👍；#134307（OAuth MCP）关闭速度获得认可；社区对 **Doctor 修复**的跟进速度（#134938、#137377 均快速闭环）应予以肯定。

---

## 8. 待处理积压

以下为长期未解决、但影响面较大的问题，提醒维护者优先关注：

| Issue | 创建时间 | 描述 | 备注 |
|---|---|---|---|
| [#86119](https://github.com/openclaw/openclaw/issues/86119) | 2026-05-24 | 孤儿 node server.js worker 进程持续堆积 | **已超 3 个月**，P1 crash-loop，仍开放 |
| [#44910](https://github.com/openclaw/openclaw/issues/44910) | 2026-03-13 | OpenAI Codex 错误泄漏到用户 Telegram 聊天 | 虽已关闭，但评论止于 9-04，需确认修复覆盖所有路径 |
| [#110190](https://github.com/openclaw/openclaw/issues/110190) | 2026-07-17 | runtime context carrier 置于用户消息之后导致模型混淆与 token 浪费 | P1，累积 9 评论，截至 9-03 无维护者明确方案 |
| [#126874](https://github.com/openclaw/openclaw/issues/126874) | 2026-08-20 | Windows CI 仅运行 10,979 个测试文件中的 66 个，"跳过即通过" | P2，平台质量风险高，无维护者响应记录 |
| [#114612](https://github.com/openclaw/openclaw/issues/114612) | 2026-07-27 | memory SQLite 无限增长 | P2 但生产影响持续扩大，社区关注度高（💬11），建议提升优先级 |
| [#127176](https://github.com/openclaw/openclaw/issues/127176) | 2026-08-21 | Windows CLI 与 Node Host 交替审批设备元数据 | P1 session-state，已有 linked PR，但 5 天无更新 |

---

*本报告基于 2026-09-04 GitHub 公开数据自动生成，数据客观呈现，不代表项目官方立场。*

---

## 横向生态对比

# 个人 AI 助手 / 自主智能体开源生态横向对比报告
**数据窗口：2026-09-03 → 2026-09-04 ｜ 覆盖 10 个有动态项目 + 4 个静默项目**

---

## 1. 生态全景

当前生态呈“**一超多强、分层演进**”格局：OpenClaw 以单日 500 Issue/500 PR 的流量远超同类，是事实上的生态核心参照；Hermes Agent、ZeroClaw、CoPaw 构成第二梯队，IronClaw、LobsterAI、NanoBot、PicoClaw、NanoClaw 则以更克制的节奏运行。各项目不约而同进入“**稳定性偿还期**”——Windows 升级回归、SQLite/记忆存储无界增长、静默失败是三份日报中的最高频词；同时外部安全研究者开始对 Agent 沙箱做对抗性测试，**安全与权限治理在多个项目中上升为主线**。模型成本敏感度的提升使 prompt cache 命中率与上下文预算成为新的性能战场。整体判断：生态正从“功能军备竞赛”转向“**可长期运行、可治理、可被信任**”的工程化阶段，技术选型者应把稳定性基线置于功能数量之前。

---

## 2. 各项目活跃度对比

| 项目 | Issue 动态 | PR 动态 | Release | 阶段与健康度 |
|---|---|---|---|---|
| **OpenClaw** | 500（新开/活跃 339，关闭 161） | 500（待合并 415，合并/关闭 85） | **v2026.9.1** | 吞吐量断层第一，但新版引入 Windows Gateway P0 回归；SQLite 损坏/存储膨胀等稳定性债务积压 |
| **Hermes Agent** | 50（49 活跃，1 关闭） | 50（47 待合并，3 合并/关闭） | 无 | “问题集中暴露、修复同步推进”，cron/流式等当日修复；但 Issue 关闭率过低、同类 Bug 重复上报明显 |
| **ZeroClaw** | 50（36 活跃，14 关闭） | 50（49 待合并，1 合并/关闭） | 无 | 缺陷生命周期管理优秀（14 条关闭），安全/权限为主线；PR 合入瓶颈突出 |
| **CoPaw** | 27（19 活跃，8 关闭） | 36（21 待合并，15 合并/关闭） | 无（2.2.x 验证已闭环） | 安全治理 + 移动端体验双线推进，健康度中上 |
| **IronClaw** | 11（8 活跃，3 关闭） | 18（8 待合并，10 合并/关闭） | 无 | 主干 24h 内由红转绿、TypeScript 技术债清理收官，典型质量巩固型 |
| **LobsterAI** | 1 新开 + 4 旧激活 + 2 自动关闭 | 15（10 合并/关闭） | 冲刺 2026.9.4（暂无正式版） | 发布收尾活跃；但 5 个 PR/3 个 Issue 积压超 150 天 |
| **NanoBot** | 4（3 活跃，1 关闭） | 25（11 待合并，14 合并/关闭） | 无 | Bug→PR 闭环速度小时级，健康；0.3.0 Current Time 回归尚无修复 |
| **NanoClaw** | 4 新开 | 23（20 待合并，3 合并/关闭） | 无 | 提交活跃但合入明显滞后；7 条 Provider 契约重构 PR 超一周未合并 |
| **PicoClaw** | 6（5 活跃，1 关闭） | 8（7 待合并，1 合并/关闭） | 无 | 维护期；两个已自测的修复 PR（Slack/WebUI）滞留至 stale |
| NullClaw / TinyClaw / Moltis / ZeptoClaw | 无活动 | 无活动 | 无 | 24h 完全静默，处于停摆/休眠状态，依赖方应关注 |

> 横向对照：OpenClaw 单日 Issue 流量是第二梯队（Hermes/ZeroClaw 各 50）的 10 倍；OpenClaw 单日 PR 关闭/合并量（85）约为其余主要项目合计（约 29）的 3 倍。

---

## 3. OpenClaw 在生态中的定位

**生态角色：核心参照与“上游引擎”。** 多个生态项目以 OpenClaw 为基线（如 LobsterAI 明确将 `openclaw.version` 锁定为 v2026.3.2 内嵌），其路线选择会直接向下游传导。

**相对优势：**
- **全端覆盖**：Control UI + 原生 macOS/iOS/Android + 桌面端 + 插件体系（Codex/MCP/OAuth/多模型）一应俱全，是生态中“平台完整度”最高的项目；
- **迭代速度**：单日合入/关闭 85 条 PR、161 条 Issue，修复响应快（codex 插件损坏、Doctor 死锁均当日闭环）；
- **生态辐射力**：Issue 中反复出现的 DeepSeek 缓存、SQLite 膨胀、子进程泄漏等话题，实际定义了整个品类的问题清单。

**技术路线差异：**
- 与 **ZeroClaw（Rust，RFC 驱动的安全契约优先）**、**IronClaw（Rust，subagent 审批链 + 上下文预算）** 相比，OpenClaw 更偏向 **Node 生态 + 本地 Gateway + SQLite memory-core 的“功能广度优先”路线**，通过高频发版换取演进速度；
- 风险在于稳定性债务：2026.9.1 的 Windows Gateway 静默退出（#137813）、SQLite 损坏在纯净重建后 15-24 小时复发（#126821）均属**架构级隐患**，而非单点 Bug。415 条待合并 PR 也意味着大量变更同时在途，回归面被放大。

**给技术决策者的结论**：追求功能覆盖与生态兼容性，OpenClaw 仍是首选参照；但若用于生产，需将 Windows 升级路径、存储增长、进程生命周期纳入验收清单，并对新版本采取“延迟一个补丁周期”策略。

---

## 4. 共同关注的技术方向

| 方向 | 具体诉求（涉及项目） |
|---|---|
| **上下文/缓存成本治理** | DeepSeek 6.x 缓存命中率跌破 10%（OpenClaw #94518）；CLI 不持久化 sidecar 致每轮首个请求 cache 塌缩（Hermes #102194）；Prompt budget 未计入非 transcript 材料（IronClaw #8053/#8057）；稳定路由键保持 prompt cache 亲和性（NanoBot #5632）；32k 上下文硬编码与预算裁剪黑盒（ZeroClaw #10068/#10597） |
| **记忆/存储生命周期管理** | memory-core SQLite 无限增长、35GB 膨胀阻塞启动、WAL checkpoint 损坏（OpenClaw #114612/#135347/#123327）；ReMe 记忆后台任务静默失败（CoPaw #7469）；SQLite 并发 busy_timeout 顺序（NanoClaw #3708）；多 Profile 状态串扰（Hermes #102526）；ACP 会话持久化与分页（ZeroClaw #10197/#10596） |
| **静默失败治理与可观测性** | 写入被拒后仍报告成功（OpenClaw #126906）；hooks“解析正确、doctor 通过但永不触发”（Hermes #69825/#102504）；locale 并发竞态无声丢失（NanoBot #5644）；MCP 错误被压平成单一 token（IronClaw #8009）；`/health` 假健康（ZeroClaw #9811，已修复）。用户对“无声错误吞没”的容忍度已接近零 |
| **安全从 Prompt 走向执行审批门禁** | CRITICAL 指令审批逻辑缺陷、沙箱突破 PoC、危险指令逃逸（CoPaw #7496/#7511/#7443）；统一权限策略 RFC #7155 落地实现、工具 schema 不再暴露 self-approval、可验证意图凭据链（ZeroClaw #10610/#10539/#9328）；OS keychain 默认加密存储（Hermes #99490）；外部护栏标准接口（OpenClaw #72741） |
| **Windows 与升级平滑度** | 2026.9.1 Windows Gateway 静默退出（OpenClaw #137813）；Windows 原生路径搜索、Studio Bridge 超时（Hermes）；Windows 安装器 DPI/控制台窗口（LobsterAI #2605/#2606）；非 UTF-8 代码页契约检查（CoPaw #7267）；升级后行为回归（NanoBot #5645、Hermes #102486） |
| **单机助手向团队/多租户演进** | QwenPaw Hub 多租户方向征集获 17 评论，诉求集中在多用户访问与管理员可管理技能（CoPaw #7318/#2324）；per-agent-group 投递模式（NanoClaw #3713）；subagent 被阻塞时进入 owner inbox 而非静默消失（IronClaw #8046）；跨渠道统一会话（CoPaw #7541） |

---

## 5. 差异化定位分析

| 项目 | 功能侧重 | 目标用户 | 技术架构关键差异 |
|---|---|---|---|
| **OpenClaw** | 全端个人 Agent 平台：Control UI/移动端/桌面 + 插件生态 + 自动任务 | 开发者到 Prosumer 全谱系 | Node 生态 + 本地 Gateway + SQLite memory-core；以“功能广度 + 高频迭代”取胜 |
| **Hermes Agent** | 多形态入口（CLI/TUI/desktop/gateway/plugins）+ Profile 会话路由 + cron/systemd 自动化 | 开发者、私有部署、自动化重度用户 | “serve 命令”服务化 + 多 Profile 状态隔离；桌面 Bot Mode 问题面较大（约 80 个开放问题） |
| **ZeroClaw** | 安全/权限优先的 Agent Runtime：统一工具权限、可验证意图、沙箱策略 | 安全敏感开发者、自托管 | Rust + crates 模块化 + RFC 驱动治理；ACP（Agent Client Protocol）会话层投入明显 |
| **IronClaw** | 自主 Coding-Agent 编排：subagent 审批链、上下文预算、沙箱边界 | AI 应用研发团队 | Rust；CI ratchet 文化（禁 `@ts-nocheck` 增量回归）；主干健康度纪律最强 |
| **CoPaw** | 多租户 Hub + 企业 IM（飞书/企微/Matrix）+ 治理规则 | 团队/企业 | 2.2.0 多租户路线；安全沙箱正被外部研究者系统性测试 |
| **LobsterAI** | 中文桌面优先体验 + 应用内浏览器/MCP + IM 机器人 | 中文 C 端用户 | Electron 客户端，内嵌 OpenClaw 上游；Windows 安装器与合规锁版是特色痛点 |
| **NanoBot** | 轻量级 WebUI/TUI 交互打磨、上下文用量可视化 | Python 开发者、个人用户 | Python；Bug 闭环速度快，界面层与通道层（Matrix/Signal）迭代活跃 |
| **PicoClaw** | 多 IM 网关 + 低资源/边缘部署（ARM/RKLLM） | 嵌入式/个人开发者 | Go 生态；与上游依赖（botgo/resty）的兼容性治理是当前重点 |
| **NanoClaw** | Agent 群组 + 定时任务 + 会话投递契约 | 群运营者/高级用户 | 频道适配层与 group contract 设计；Provider 契约重构系列形成合并瓶颈 |

> 共性说明：几乎所有项目都在做同一件事——**把 Agent 从“单轮聊天”改造为“可调度、可审批、可审计、跨渠道一致”的长生命周期服务**，差异仅在切入路径（消费端/开发者端/安全端/团队端）。

---

## 6. 社区热度与成熟度

**活跃度分层：**
- **爆发层**：OpenClaw（500/500 每日更新），一骑绝尘；
- **高活跃层**：Hermes Agent（50/50）、ZeroClaw（50/50）、CoPaw（27/36）——处于“功能交付与问题暴露并行”的高强度期；
- **中活跃层**：IronClaw（11/18）、LobsterAI（15 PR）、NanoClaw（23 PR）、PicoClaw（6/8）、NanoBot（4/25）——以质量收敛、维护与发布收尾为主；
- **静默层**：NullClaw/TinyClaw/Moltis/ZeptoClaw——24h 无任何活动，若持续将退出有效竞争。

**质量巩固 vs 快速迭代：**
- **快速迭代阶段**：OpenClaw（功能推进最快，但新版本引入回归的频率同步上升）；Hermes Agent（问题集中暴露与修复同步，triage 能力待加强）；NanoClaw（提交活跃，但合入节奏未跟上）；
- **质量巩固阶段**：IronClaw（主干绿色纪律 + TS 技术债清零 + CI ratchet 防复发）；LobsterAI（2026.8.31 发布收尾、Windows 安装器修复）；CoPaw（2.2.x 稳定性修复 + 移动端补齐）；NanoBot（Bug 平均修复周期 1-10 天，当日新 Bug 当日开 PR）；PicoClaw（维护期，但修复 PR 滞留过久有贡献者流失风险）；ZeroClaw（缺陷闭环效率高，但 49 条 PR 待合并是明显瓶颈）。

**流程成熟度信号：**
- 工程文化标杆：IronClaw（主干变红即优先修复并解除全队阻塞）、ZeroClaw（RFC/accepted/needs-repro 标签体系 + 决策队列 #8692 + 已接受 RFC 实现索引 #10330）；
- 治理压力信号：Hermes 社区自建约 80 问题的桌面 Bot Mode umbrella tracker（#94726）；ZeroClaw 社区创建“维护者决策队列”以推动 RFC 透明化。这说明头部项目的**维护者带宽已成为社区感知的稀缺资源**。

---

## 7. 值得关注的趋势信号

**① “上下文经济”成为一等公民，而不仅是性能优化**
OpenClaw 的 DeepSeek 缓存投诉获得高赞、Hermes 用户精确测量每轮首请求 cache 塌缩、IronClaw 将 budget/cache key 列为主线 PR、NanoBot 为缓存亲和性改造路由键——**用户正在按 API 成本监控 Agent 行为**。参考价值：智能体框架应默认暴露 token/缓存命中/裁剪事件三类指标，支持缓存亲和路由，并消灭硬编码上下文窗口。

**② Agent 记忆从“功能特性”转向“数据治理”**
SQLite 无限增长、35GB 膨胀阻塞启动、静默失败且无保留策略、重置后空间不释放——多项目同时踩中同一类坑。参考价值：**存储配额、TTL/保留策略、可回收的 reset、损坏自愈**应作为记忆子系统的基础需求而非事后补丁。

**③ 安全重心从 Prompt 层下沉到运行时强制执行**
CoPaw 的沙箱突破与指令逃逸、ZeroClaw 的“工具 schema 不应暴露 self-approval”与可验证意图、IronClaw 的 subagent 审批门禁——行业共识正在形成：**安全不能依赖模型“自觉”，必须由运行时门禁、审批链与密码学验证兜底**。第三方安全研究者已开始系统性对抗测试，项目方需建立公开的安全响应路径。

**④ Windows 与“升级平滑度”成为新的质量基线**
OpenClaw 新版本 Windows Gateway 静默退出并影响所有 Scheduled Task 部署、LobsterAI 集中修 Windows 安装器、Hermes 修 Windows 原生路径、CoPaw 修非 UTF-8 代码页问题。同时多项目出现“升级后行为变化”回归（NanoBot 0.3.0 丢失 Current Time、OpenClaw 升级后插件失效）。参考价值：**Windows 真实 CI（而非跳过）与升级演练应成为 Agent 项目的发布门禁**。

**⑤ “静默失败”是信任杀手，可观测性即竞争力**
hooks 配置正确但不触发、写入被拒绝仍报告成功、locale 竞态无声丢失、错误被压平成单一 token——最损害用户信任的不是报错，而是“无声的错误吞没”。ZeroClaw 修复 `/health` 假健康、Hermes 修复 deny 语义丢失，说明头部项目已开始反向投资**状态真实性**。开发者选型时可主动检查：该框架的错误路径是否保留根因上下文？健康检查是否反映真实连接状态？

**⑥ 单机个人助手向“团队基础设施”演进，多租户与跨渠道会话成为分水岭**
CoPaw 的多租户 Hub 方向、NanoClaw 的 per-group 投递契约、Hermes 的跨 Profile 会话隔离、IronClaw 的 subagent 任务上浮到 owner inbox——共同指向一个未来：**Agent 不再只服务一个人，而是服务一个群组、一个团队、一个组织的共享工作流**。率先补齐多用户权限模型与跨渠道统一会话的项目，将在下一阶段获得企业级市场的先手。

---

## 同赛道项目详细报告

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

# NanoBot 项目动态日报（2026-09-04）

## 今日速览

过去 24 小时 NanoBot 保持高活跃度：**4 条 Issue 更新**（3 条新开/活跃、1 条关闭），**25 条 PR 更新**，其中 **14 条已合并/关闭、11 条仍待合并**，无新版本发布。合并主要集中在 WebUI 状态管理、Matrix/Signal 通道可靠性与 Provider 错误处理，其中困扰用户多日的 **Gateway 重启后 WebUI 卡死问题（#5512）** 已由 PR #5514 修复关闭。与此同时，今日新报的 **WebUI locale 并发加载丢数据（#5644）** 在几小时内即获得对应修复 PR，说明维护者对社区反馈响应速度较快。整体属于"高产出、状态健康"的迭代节奏。

## 项目进展

今日有约 14 条 PR 被合并/关闭，覆盖 WebUI、通道层、Provider、SDK 与 Agent 工具链：

- **WebUI 稳定性（重点）**
  - [`PR #5514`](https://github.com/HKUDS/nanobot/pull/5514) 修复 Gateway 重连后流式状态残留导致的 WebUI 永久 spinning，关闭 [`Issue #5512`](https://github.com/HKUDS/nanobot/issues/5512)。`useNanobotStream` 此前未订阅 `onRunStatus` 重置事件，现已补上。
  - [`PR #5650`](https://github.com/HKUDS/nanobot/pull/5650) 保留 Hero 页面所选模型预设，避免创建首条消息时乐观会话丢失模型选择。
  - [`PR #5646`](https://github.com/HKUDS/nanobot/pull/5646) 语言选择器只显示母语名称，改善国际化可用性。

- **通道层可靠性**
  - [`PR #5637`](https://github.com/HKUDS/nanobot/pull/5637) Matrix 流式发送失败不再被吞掉，可走 channel manager 重试策略。
  - [`PR #5472`](https://github.com/HKUDS/nanobot/pull/5472) Signal 入站白名单支持 `*` 通配符。
  - [`PR #5385`](https://github.com/HKUDS/nanobot/pull/5385) 补全 Matrix Element SAS 设备验证流程。
  - [`PR #5334`](https://github.com/HKUDS/nanobot/pull/5334) 修复长消息按换行切分时缩进丢失问题。

- **Provider 与 SDK**
  - [`PR #5413`](https://github.com/HKUDS/nanobot/pull/5413) Provider 抛异常时也能触发 fallback 策略，而非直接逃逸。
  - [`PR #5632`](https://github.com/HKUDS/nanobot/pull/5632) Codex 会话路由键采用稳定 SHA-256 派生值，保持 prompt cache 亲和性（性能优化）。
  - [`PR #5635`](https://github.com/HKUDS/nanobot/pull/5635) SDK 流关闭时不再丢弃排队中的未读事件。

- **Agent/工具链**
  - [`PR #5515`](https://github.com/HKUDS/nanobot/pull/5515) 后台会话超时投递任务的失败不再被静默丢弃。
  - [`PR #5629`](https://github.com/HKUDS/nanobot/pull/5629) `format_tool_hints()` 对普通（非路径/非命令）参数也执行 `max_length` 截断。

整体判断：项目今日在"多通道稳定性"与"WebUI 交互细节"两个方向明显前进，且大量修复自带回归测试。

## 社区热点

按评论数与关联讨论热度，今日最受关注的条目为：

- [`Issue #5644`](https://github.com/HKUDS/nanobot/issues/5644)：Channel locale 并发加载导致语言包（如 `en`）丢失。虽是启动期竞态，但影响面是"用户界面语言随机消失"，且 [`PR #5651`](https://github.com/HKUDS/nanobot/pull/5651) 当日即开出并明确 `Closes #5644`，形成完整的 bug→fix 闭环，是今日社区协作效率的典型代表。
- [`Issue #5512`](https://github.com/HKUDS/nanobot/issues/5512)：Gateway 重启后 WebUI 永久 spinner，用户视角是"整个对话无法继续"，诉求是前端必须感知后端 run 状态重置。该问题于 8/24 提出，9/3 由 PR #5514 关闭，响应周期约 10 天。
- [`PR #5650`](https://github.com/HKUDS/nanobot/pull/5650) 与 [`PR #5646`](https://github.com/HKUDS/nanobot/pull/5646) 均来自同一贡献者且当日合并，反映 WebUI 细节体验正在被密集打磨。

## Bug 与稳定性

按严重程度排序：

1. **高：[`Issue #5645`](https://github.com/HKUDS/nanobot/issues/5645)（0.3.0 回归）** — `nanobot-ai` 从 0.2.2 升级到 0.3.0 后，`ContextBuilder.build_messages()` 不再自动注入 Current Time 运行时上下文。影响 Agent 对时间相关问题的判断，且与 timezone 文档描述不符。**尚未见对应修复 PR**，建议优先排查。
2. **高：[`Issue #5644`](https://github.com/HKUDS/nanobot/issues/5644)（启动期竞态）** — 两个 locale 并发加载时，后完成的写回会覆盖先完成的 Map 注册，导致 `en` 等语言在启动时静默丢失。**已有修复 PR [`#5651`](https://github.com/HKUDS/nanobot/pull/5651) 待合并**。
3. **中：[`Issue #5647`](https://github.com/HKUDS/nanobot/issues/5647)（标题生成边界条件）** — 前端 envelope 缺少 `webui` 标记时 `unifiedSession` 模式下会话标题不生成，是 PR #5528 修复后的遗留分支。**对应 PR [`#5648`](https://github.com/HKUDS/nanobot/pull/5648) 已开出**。
4. **已解决：[`Issue #5512`](https://github.com/HKUDS/nanobot/issues/5512)** — WebUI spinning 问题已由 `PR #5514` 关闭，修复方式是消费客户端 `onRunStatus` 重置事件。

## 功能请求与路线图信号

今日无新开纯 feature 类 Issue，但以下开放 PR 反映了明确的功能方向：

- [`PR #5620`](https://github.com/HKUDS/nanobot/pull/5620)（cron 可配置投递目标 + 批量归档）：将 cron 结果投递目标与 session 所有权解耦，并引入"归档"生命周期。属于任务调度能力增强，9/1 创建、9/4 仍在更新，说明作者在积极迭代，**大概率进入下个版本**。
- [`PR #5649`](https://github.com/HKUDS/nanobot/pull/5649)（WebUI 上下文复用可视化）：把 token 用量从消息中移入 composer popover，并用堆叠条展示每次请求的上下文占比与复用率。指向"可观测 Agent 上下文消耗"的产品方向。
- [`PR #5504`](https://github.com/HKUDS/nanobot/pull/5504)（UI 展示模型重试状态）：将重试倒计时与尝试进度实时渲染到 TUI/WebUI，已积压 11 天，疑与 NAN-34 内部任务绑定，需关注其推进。
- [`PR #5641`](https://github.com/HKUDS/nanobot/pull/5641)（iOS PWA 交互修复）与 [`PR #5639`](https://github.com/HKUDS/nanobot/pull/5639)（OpenTUI 升级、会话标签稳定化）提示移动端与 TUI 体验也在并行优化。
- [`PR #5446`](https://github.com/HKUDS/nanobot/pull/5446)（Codex OAuth 令牌持久化到 Nanobot 数据目录）：涉及安全与多端一致性，属于基础设施完善类需求。

## 用户反馈摘要

从今日 Issue/PR 描述可提炼以下真实用户声音：

- **升级阵痛（desku24，#5645）**：0.2.2→0.3.0 后同样的调用代码行为变化，Current Time 上下文默认缺失。用户实际关切是"升级后 Agent 可能不知道当前时间"，属于值得重视的兼容性回归。
- **重启恢复体验（yrxeva，#5512）**：Gateway 重启后前端永久"转圈"，用户被迫强刷或重新开始对话。修复后团队应补充类似断电/重启场景的端到端测试，防止复发。
- **静默丢数据的惊讶感（top777，#5644）**：locale 并发加载导致 UI 语言偶发缺失，且无任何报错，用户难以自行排查。该案例说明"竞态导致的静默丢失"比显式报错更损害信任。
- **边界条件敏感（zpljd258，#5647）**：开发者用户对 `unifiedSession` 模式下 `target_session_key` 与前端 envelope 缺口的反馈非常细致，说明已有用户深度使用会话投影能力并愿意回报边缘问题。

## 待处理积压

- [`PR #5446`](https://github.com/HKUDS/nanobot/pull/5446)：Codex OAuth 令牌持久化，自 8/19 开启（16 天），带 `conflict` 标签，可能已产生冲突需要 rebase。建议维护者优先处理或明确说明阻塞点。
- [`PR #5504`](https://github.com/HKUDS/nanobot/pull/5504)：模型重试状态 UI，8/24 起已等待 11 天，功能跨 TUI/WebUI/WebSocket，改动面大，建议安排 reviewer 推进。
- [`PR #5620`](https://github.com/HKUDS/nanobot/pull/5620)：cron 批量归档功能，9/1 提出，作者 9/4 仍在提交，需要维护者反馈设计是否合意。
- 新开待审 PR 提醒：[#5651](https://github.com/HKUDS/nanobot/pull/5651)（locale 修复）、[#5648](https://github.com/HKUDS/nanobot/pull/5648)（标题修复）、[#5649](https://github.com/HKUDS/nanobot/pull/5649)（上下文可视化）均为 2 天内新开，等待首轮 review。

> 总评：24 小时内合入 14 条 PR，Bug 平均修复周期约 1-10 天，今日新增 3 个 Bug 中 2 个已有对应修复 PR，项目健康度良好。唯一需要警惕的是 **0.3.0 的 Current Time 上下文回归（#5645）** 尚无修复方案，建议尽快定位。

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent 项目动态日报（2026-09-04）

> 数据源：[NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) ｜ 统计窗口：过去 24 小时（2026-09-03 → 2026-09-04）

## 1. 今日速览

过去 24 小时项目保持极高活跃度：50 条 Issue 更新（49 条新开/活跃，1 条关闭）与 50 条 PR 更新（47 条待合并，3 条合并/关闭），无新版本发布。今日 Issue 呈现明显的"同类 Bug 重复上报"特征，三大集中区为：**桌面端 serve 命令不加载 shell hooks/插件 hooks**、**auxiliary vision 自定义 provider 鉴权 401**、**桌面端 composer 拖拽误触发**——说明这些问题在真实用户侧影响面已经铺开，且多个 P1/P0 问题（如 [#102194](https://github.com/NousResearch/hermes-agent/issues/102194)、[#102574](https://github.com/NousResearch/hermes-agent/issues/102574)）尚无修复 PR。正面信号是 cron/systemd 兼容、流式回答持久化、后台任务路由等问题当天即有修复 PR（[#102655](https://github.com/NousResearch/hermes-agent/pull/102655)、[#102646](https://github.com/NousResearch/hermes-agent/pull/102646)、[#102647](https://github.com/NousResearch/hermes-agent/pull/102647)），项目处于"问题集中暴露、修复同步推进"的快速迭代期。

## 2. 项目进展（今日合并/关闭）

今日共 3 条 PR 合并/关闭、1 条 Issue 关闭（展示列表中可见部分）：

**已关闭 PR**
- [PR #77157｜fix(search): zero-match probes fall back to grep; native paths for rg on Windows](https://github.com/NousResearch/hermes-agent/pull/77157)（closed）——修复原生 Windows 下 `search_files` 的路径边界问题：零匹配时自动回退 grep，同时保留 MSYS 安全路径处理。该 PR 自 8 月 2 日开启，经历约一个月后于今日关闭，Windows 平台搜索工具链得到实质完善。

**已关闭 Issue**
- [Issue #15779｜Bug: /model switch to named custom provider ignores custom_providers model context_length](https://github.com/NousResearch/hermes-agent/issues/15779)（closed）——4 月 25 日创建、积压超过 4 个月的"`/model` 切换命名自定义 provider 时忽略 context_length"问题今日关闭。配套的 compressor 惰性路径修复 PR [#102645](https://github.com/NousResearch/hermes-agent/pull/102645)（honor per-model custom-provider context_length）也于今日提交，说明自定义 provider 上下文窗口配置这一整条问题链正在收口。

**今日新开、指向明确修复的 PR（体现项目推进方向）**
- [PR #102655](https://github.com/NousResearch/hermes-agent/pull/102655)：修复 systemd 249 拒绝 `OOMPolicy=kill` 导致 cron 调度全部失败（对应 P1 [#102486](https://github.com/NousResearch/hermes-agent/issues/102486)）
- [PR #102646](https://github.com/NousResearch/hermes-agent/pull/102646)：防止 sentinel-only 流式内容被当作最终回答持久化
- [PR #102647](https://github.com/NousResearch/hermes-agent/pull/102647)：multiplex_profiles 下后台任务输出路由到所属 profile 的 bot
- [PR #102649](https://github.com/NousResearch/hermes-agent/pull/102649)：delegation.fallback_providers 正确作用于 child agent（修复 #65038）
- [PR #102650](https://github.com/NousResearch/hermes-agent/pull/102650)：新增 owner-only 的 `hermes sessions reset-store` 恢复命令

## 3. 社区热点

**讨论最热**
- [Issue #96692｜[Spec]: Unified slash-command registry and execution contract across every Hermes surface（11 评论）](https://github.com/NousResearch/hermes-agent/issues/96692)——规格型讨论，目标是让 CLI/gateway/TUI/plugins/desktop 共用同一套版本化斜杠命令目录、解析器与调用/结果契约。标记为 `needs-decision`，属于跨所有产品面的架构级议题，社区参与度高，短期内会持续发酵。
- [Issue #69825｜serve command never registers shell hooks（7 评论）](https://github.com/NousResearch/hermes-agent/issues/69825)——shell hooks 配置"解析正确、doctor 通过、但实际永不触发"的诡异问题。今日出现同族升级报告 [#102504](https://github.com/NousResearch/hermes-agent/issues/102504)（P1）与插件版 [#102592](https://github.com/NousResearch/hermes-agent/issues/102592)，说明影响的不只是单用户，而是整个桌面端后端路径。
- [Issue #94726｜Desktop Bot Mode 开放 Bug 跟踪器（6 评论，👍1）](https://github.com/NousResearch/hermes-agent/issues/94726)——由 teknium1 发起的 umbrella tracker，汇总约 80 个开放问题并按类别分组，是社区在帮助维护者建立桌面 Bot Mode 问题地图。

**重复上报形成热点簇**
- 视觉 provider 401 簇：[#100858](https://github.com/NousResearch/hermes-agent/issues/100858)（6 评论）+ [#76602](https://github.com/NousResearch/hermes-agent/issues/76602)（4 评论）：`auxiliary.vision` 配置 `custom:<name>` + `base_url` 后，请求把 API key 降级为 `no-key-required`，两个独立用户在一个月内先后踩中，且 [#76602](https://github.com/NousResearch/hermes-agent/issues/76602) 已给出抓包级根因分析，修复 PR [#67055](https://github.com/NousResearch/hermes-agent/pull/67055) 已悬挂 7 周未合并，社区耐心在被消耗。
- Desktop composer 拖拽簇：[#70422](https://github.com/NousResearch/hermes-agent/issues/70422)（5 评论，👍1）+ [#101318](https://github.com/NousResearch/hermes-agent/issues/101318)（4 评论）：选中文本/滚动时误抓取悬浮窗成为高频抱怨，且无关闭选项。

**潜在诉求分析**：今日热点本质上都指向同一件事——**用户对"配置/契约是否真实生效"的信任危机**：hooks 被静默跳过、API key 被静默替换、composer 被静默拖出。这类"配置面与实际运行面不一致"的问题比显式报错更损伤信任，建议维护者优先处理钩子类（hook/plugin）问题。

## 4. Bug 与稳定性

按严重程度列出今日活跃 Bug，并标注是否有修复 PR：

**P0**
- [Issue #102194｜CLI 路径从不持久化 api_content sidecar → 每轮首次 API 调用丢失 prompt cache（P0，2 评论）](https://github.com/NousResearch/hermes-agent/issues/102194)——每轮对话首个请求 `cache_read` 塌缩到 header-only 前缀，后续请求却 94–100% 命中。直接影响用户成本与延迟，**尚无修复 PR**。
- [Issue #93817｜Reasoning Blocks OFF 仍输出全部思维链与工具调用（P0 用户声称 / P3 标记 / duplicate）](https://github.com/NousResearch/hermes-agent/issues/93817)——若属实涉及推理隐私泄漏，但目前被标记为 P3 + duplicate，存在**用户感知严重度与维护者 triage 不一致**，建议复查。

**P1**
- [Issue #102574｜共享 PeriodicScheduler 单线程内联执行，任一回调阻塞将使全部安全定时器停摆（P1，新开）](https://github.com/NousResearch/hermes-agent/issues/102574)——turn-liveness、lease 刷新、child heartbeat 全部依赖该调度器，**尚无修复 PR**。
- [Issue #102486｜systemd 249 拒绝 `OOMPolicy=kill` → cron worker 调度全部 fail-closed（P1，4 评论）](https://github.com/NousResearch/hermes-agent/issues/102486)——post-v0.21.0 回归，**修复 PR [#102655](https://github.com/NousResearch/hermes-agent/pull/102655) 当日已提交**，响应迅速。
- [Issue #102526｜Desktop 启动时 HERMES_HOME 覆盖竞态 → 默认 bot 打开另一个 profile 的聊天与 state.db（P1）](https://github.com/NousResearch/hermes-agent/issues/102526)——多 profile 状态串扰，用户点击 Hermes 行却进入 `treasure-valley-pm` 的 996 条消息会话，**尚无修复 PR**。
- [Issue #102504｜`serve` 命令跳过 _prepare_agent_startup → config.yaml shell hooks 永不注册（P1，[#69825](https://github.com/NousResearch/hermes-agent/issues/69825) duplicate）](https://github.com/NousResearch/hermes-agent/issues/102504)——所有桌面端会话的外发/破坏性命令护栏静默缺失，**尚无修复 PR**。

**P2（精选）**
- [Issue #102592｜插件注册的 hooks（pre_llm_call 等）在 serve/dashboard 永不触发（新开，2 评论）](https://github.com/NousResearch/hermes-agent/issues/102592)——与 #69825/#102504 同源，为插件体系版。
- [Issue #100858 / #76602｜vision custom provider 鉴权 401 簇](https://github.com/NousResearch/hermes-agent/issues/100858)——修复 PR [#67055](https://github.com/NousResearch/hermes-agent/pull/67055) 已存在但滞留。
- [Issue #97296｜macOS 27 上 Popen(start_new_session=True) fork 线程化 gateway → Network.framework atfork SIGSEGV（5 评论）](https://github.com/NousResearch/hermes-agent/issues/97296)
- [Issue #70422 / #101318｜Desktop composer 拖拽误弹出（5 + 4 评论）](https://github.com/NousResearch/hermes-agent/issues/70422)
- [Issue #101091｜Desktop 接受不匹配的 provider/model/base_url 组合并注入错误 provider 组](https://github.com/NousResearch/hermes-agent/issues/101091)
- [Issue #100870｜Docker 后端 brace group 重写器在 `}` 后漏分隔符，远程 code kernel 启动失败](https://github.com/NousResearch/hermes-agent/issues/100870)
- [Issue #100855｜两条 browser_exec 路径不设 AGENT_BROWSER_SOCKET_DIR，孤儿守护进程对 reaper 不可见，wedge 47 小时](https://github.com/NousResearch/hermes-agent/issues/100855)
- [Issue #98645｜Desktop clarify 卡片空白渲染，因无内容可点超时 10 分钟（P2 回归）](https://github.com/NousResearch/hermes-agent/issues/98645)
- [Issue #100381 / #100315｜codex 相关压缩触发与推理事件授权异常（压缩抖动/无限期保留）](https://github.com/NousResearch/hermes-agent/issues/100381)
- [Issue #77409｜桌面 UI 测试在 NODE_ENV=production 下 React.act undefined，约 957 个失败](https://github.com/NousResearch/hermes-agent/issues/77409)

**Windows 平台（标注 invalid）**
- [Issue #102642](https://github.com/NousResearch/hermes-agent/issues/102642) 与 [Issue #102057](https://github.com/NousResearch/hermes-agent/issues/102057)——Studio 组聊/冷启动触发 Agent Bridge 连接超时（WinError 10060 / ETIMEDOUT，1s 超时 0 重试）。已带 `invalid` 标记，维护者可能判断为环境/非产品缺陷，但作为平台真实体感问题仍建议给出结论性说明或关闭理由，避免用户困惑。

## 5. 功能请求与路线图信号

**架构级**
- [Issue #96692｜统一斜杠命令注册表与执行契约（11 评论，needs-decision）](https://github.com/NousResearch/hermes-agent/issues/96692)——横跨 CLI/gateway/TUI/plugins/desktop 的命令体系大一统规格，若被采纳将是跨版本的大工程，属于中期路线图信号。

**桌面端**
- [Issue #77952｜切换 Profile 后恢复该 Profile 上次选中的会话（4 评论）](https://github.com/NousResearch/hermes-agent/issues/77952)
- [Issue #91329｜Bot Mode 群组设置中直接管理成员](https://github.com/NousResearch/hermes-agent/issues/91329)
- [Issue #102597｜All-profiles 会话列表显示每行所属 Profile 标记（新开）](https://github.com/NousResearch/hermes-agent/issues/102597)

**CLI / 配置**
- [Issue #102582｜`hermes moa configure` 暴露 per-slot reasoning effort（新开）](https://github.com/NousResearch/hermes-agent/issues/102582)——schema 已支持但交互配置缺失，补齐成本低，较可能进入下个版本。

**i18n（新信号）**
- [Issue #102643｜斜杠命令 description 的 i18n 支持（中文请求，含 `str | dict[str,str]` 具体方案）](https://github.com/NousResearch/hermes-agent/issues/102643)——来自中文母语用户的完整方案提议，反映非英语用户群体增长，值得纳入多语言规划。

**结合已有 PR 判断**：桌面安全默认值方向会被加强，[PR #99490](https://github.com/NousResearch/hermes-agent/pull/99490)（secret storage 默认走 OS keychain 加密）已在队列中；本地数据自救能力（[PR #102650](https://github.com/NousResearch/hermes-agent/pull/102650) 的 store reset）与[自更新依赖 PR #102648](https://github.com/NousResearch/hermes-agent/pull/102648)（Tirith 二进制自动维护）也具备合入条件。多个桌面小功能请求（#77952、#101318 的 disable 选项）实现成本低，若能随下个桌面版本一起释放，可有效缓解当前 Desktop 密集的 UX 抱怨。

## 6. 用户反馈摘要

从今日 Issue 评论与描述中提炼的真实用户体感：

- **护栏"静默不存在"最令用户不安**：#69825 用户详细描述了"配置被正确解析、allowlist、hooks doctor/test 全部通过——但真实聊天中从不触发"；P1 duplicate [#102504](https://github.com/NousResearch/hermes-agent/issues/102504) 用户明确写道"outbound-send guards、destructive-command guards、tenant guards 全部静默缺席"。这是安全功能在用户不知情状态下失效，比直接报错危险得多。
- **Profile 状态混串造成身份困惑**：#102526 用户反映"每次点击 Hermes（默认）行，打开的却是另一个 bot 的聊天——同名、同会话、996 条消息的 Bot Chat"，严重动摇用户对本地数据隔离的信任。
- **高频 UX 干扰**：#70422 用户称 composer 误拖拽"constantly happens in normal use"，选中文本都会被中断；#101318 抱怨 16px 的抓取环"太容易误触"，并强调"没有任何关闭方式"。
- **付费/成本敏感信号**：P0 [#102194](https://github.com/NousResearch/hermes-agent/issues/102194) 用户详细测量了"每轮首调用 cache 塌缩、同轮后续调用 94–100% 命中"的差异，说明用户正在密切监控 prompt cache 成本，这类隐藏性能缺陷会直接影响留存。
- **多语言社区活跃**：PR [#102654](https://github.com/NousResearch/hermes-agent/pull/102654) 修复 TTS 静默截断长回复（作者使用西班牙语撰写 PR 描述）；Issue [#102643](https://github.com/NousResearch/hermes-agent/issues/102643) 为中文 i18n 提议。项目已拥有实际的西语/中文贡献与反馈群体，界面 i18n 需求开始浮出水面。
- **Windows 中文环境用户**：kwingen 在 Studio 组聊中贴出中文系统原文的 `WinError 10060` 错误，表示"切回单聊后可能消失"，这类间歇性桥接问题很影响首次体验。

## 7. 待处理积压

以下为长期未解决或今日新增但需要维护者明确响应的条目：

- **[Issue #69825｜serve 不注册 shell hooks（7 周未解决，已产生 P1 duplicate）](https://github.com/NousResearch/hermes-agent/issues/69825)**——7 月 23 日创建，已有 7 条评论并衍生 #102504/#102592 同族报告，是当前信任危机最大源头，强烈建议本周内 triage 并指派。
- **[PR #67055｜fix(vision): preserve named provider transport（7 周悬挂）](https://github.com/NousResearch/hermes-agent/pull/67055)**——直接对应两个 401 用户报告（#76602/#100858），长期不合并导致同一问题反复上报，属于"有修复方案却积压"的典型。
- **[PR #31003｜fix(tools): reject redacted-secret placeholders（5 月 23 日开启，超 3 个月）](https://github.com/NousResearch/hermes-agent/pull/31003)**——安全边界类修复，涉及 write_file/patch 对脱敏占位符的防护，不宜继续拖延。
- **[Issue #97296｜macOS 27 kanban dispatcher SIGSEGV（开启 7 天，P3）](https://github.com/NousResearch/hermes-agent/issues/97296)**——涉及 fork 线程化 gateway 的底层风险，macOS 27 beta 用户已被阻塞，建议至少确认是否在 beta 范围内。
- **[PR #99490｜desktop secret storage 默认安全化（已开启 4 天）](https://github.com/NousResearch/hermes-agent/pull/99490)** 与 **[PR #98913｜key_cmd argv 加固（已开启 5 天）](https://github.com/NousResearch/hermes-agent/pull/98913)**——两项安全 PR 均涉及 trusted-operator 边界与默认安全策略，建议安全负责人尽快 review，避免与 #98831/#98910 的安全报告继续积压成簇。
- **[Issue #15779 的前置遗留](#)**——旧 Bug 虽已关闭，但相关实现分散在 `/model` 切换、compressor 惰性路径等多个入口（[#102645](https://github.com/NousResearch/hermes-agent/pull/102645) 仍在队列），建议在合并 #102645 时做一次全链路 context_length 一致性回归，防止问题在 gateway/desktop 路径复发。
- **[Issue #70422/#101318｜composer 拖拽 disable 选项（7 月至今重复 2 次）](https://github.com/NousResearch/hermes-agent/issues/70422)**——非高难度问题但用户情绪累积明显，建议在下个桌面版本中给出一劳永逸的设置项。

> **项目健康度小结**：今日吞吐量强劲（50+50 更新），Issue 关闭速度仍偏慢（仅 1 条），P0/P1 中至少 4 条尚无对应 PR；重复上报与"有修复 PR 但不合入"（如 #67055）是当前社区摩擦的主要来源。若能加快安全/兼容性类 PR 的合入节奏，并优先消除 hooks 静默失效簇，项目的版本信任度将有明显回升。

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

# PicoClaw 项目动态日报 — 2026-09-04

项目: [sipeed/picoclaw](https://github.com/sipeed/picoclaw)
数据窗口: 2026-09-03 至 2026-09-04（GitHub 活动时间线）

## 1. 今日速览

过去 24 小时项目活跃度中等偏高：共 6 条 Issue 更新（5 条开放/活跃、1 条关闭），8 条 PR 更新（7 条待合并、1 条已合并/关闭），无新版本发布。当前 PR 流主要由 Dependabot 自动依赖升级驱动（8 条中 5 条），但同时有 2 个用户侧 bug 修复（Web UI 卡顿、Slack 媒体上传）处于待合并状态，说明维护者注意力正从功能开发转向稳定性收敛。社区方面，QQ 频道 401 鉴权失败出现重复报告，且新 Issue #3365 给出了较深的根因分析，需尽快统一受理。总体判断：项目处于健康的维护期，但若干用户可感知缺陷的修复 PR 停留时间偏长，合入节奏需要加快。

## 2. 版本发布

今日无新版本 Release（最新版本仍为 0.3.1 / nightly 构建），故本节从略。

## 3. 项目进展

今日合并/关闭类变更较少，主要成果如下：

- **[PR #3329（已关闭）——fix(line): 对无效的 webhook_host / webhook_port 给出警告而不是默认填充](https://github.com/sipeed/picoclaw/pull/3329)**
  修复 #3328。此前 `line.settings.webhook_host` / `webhook_port` 虽然可以被声明、默认化并绑定环境变量，但实际上没有任何代码读取它们；LINE 通道的 Webhook 实际被挂载在共享网关 HTTP 服务上。该 PR 移除了会造成误导的默认值注入逻辑，改为惰性配置时显式告警，避免用户误以为这两个配置项生效。这是对多通道配置体系可维护性的一次有效清理。
- **[Issue #3339（已关闭）——Antigravity 返回通用 429 的问题已被处理](https://github.com/sipeed/picoclaw/issues/3339)**
  该 Issue 报告：Google Antigravity 的 OAuth 认证与模型发现均正常，但所有生成请求均返回 `429 RESOURCE_EXHAUSTED`。此问题已在过去 24 小时内关闭，解决方式可回看 Issue 评论区。

其余 7 条开放 PR 中，除 5 条依赖升级外，以下两个实质性修复值得关注（详见下节）：

- [PR #3340 — fix(slack): 在媒体上传参数中设置 FileSize](https://github.com/sipeed/picoclaw/pull/3340)
- [PR #3347 — fix: 解决 Web UI 聊天记录变长后的输入卡顿](https://github.com/sipeed/picoclaw/pull/3347)

综合来看，项目今日主要的代码进展是关闭了一个 LINE 通道配置隐患；Slack 与 Web UI 的两个用户侧修复仍停留在“待合并”状态，尚未进入主分支。

## 4. 社区热点

**[Issue #3281 — Web UI 聊天输入在历史记录变长后非常卡顿](https://github.com/sipeed/picoclaw/issues/3281)**
- 评论数: 9 | 👍: 2 | 状态: OPEN（stale）
- 这是当前社区反馈最集中的问题。用户反馈在单会话历史较长时，输入框输入出现严重延迟，已影响实际使用。该 Issue 自 7 月 21 日创建至今仍开放，但社区贡献者 iMilnb 已提交 [PR #3347](https://github.com/sipeed/picoclaw/pull/3347) 并声称已在桌面和移动端浏览器完成验证。背后诉求本质上是 PicoClaw Web 前端在长文本渲染/状态更新上的性能优化，很大概率会被纳入下一版本。

**[Issue #3349 与 #3365 — QQ 频道网关 401 鉴权失败](https://github.com/sipeed/picoclaw/issues/3365)**
- #3349（创建于 2026-08-30，3 条评论）：Docker 版和 Linux x86 版均无法使用 QQ 频道，Gateway 日志报 `401 code:11241`，提示“请求头 Authorization 参数格式错误”。
- #3365（今日新开，0 条评论）：作者进一步将根因缩小到 `botgo v0.2.1 + resty >= v2.17` 的兼容性问题，并给出了完整的复现环境（Orange Pi 3B / aarch64 / nightly build）。
- 两个 Issue 指向同一故障，且新 Issue 已给出较高价值的依赖层分析，预计会成为近期通道稳定性修复的输入。

此外，[Issue #3338（Slack 媒体上传失败）](https://github.com/sipeed/picoclaw/issues/3338) 也有 3 条评论，用户对根因的定位非常清晰，同样值得维护者快速跟进。

## 5. Bug 与稳定性

按严重程度排序：

| 严重程度 | Bug 摘要 | 对应 Issue | 修复 PR |
|---|---|---|---|
| **高** | **Slack 无法发送图片等媒体内容**：`SendMedia` 构造 `slack.UploadFileParameters` 时未设置 `FileSize`，SDK 在发起网络请求前即以 `file size cannot be 0` 拒绝全部上传 | [#3338](https://github.com/sipeed/picoclaw/issues/3338) | ✅ [#3340](https://github.com/sipeed/picoclaw/pull/3340) 待合并 |
| **高** | **QQ 频道连接完全不可用**：Gateway 获取 WebSocket 信息时返回 401 鉴权错误，Docker 与 Linux x86 版本均复现 | [#3349](https://github.com/sipeed/picoclaw/issues/3349)、[#3365](https://github.com/sipeed/picoclaw/issues/3365) | ❌ 暂无，需在依赖层面处理 botgo/resty 兼容性 |
| **中高** | **Web UI 输入框在长会话历史下严重卡顿**，核心交互受影响 | [#3281](https://github.com/sipeed/picoclaw/issues/3281) | ✅ [#3347](https://github.com/sipeed/picoclaw/pull/3347) 待合并 |
| 中 | Google Antigravity 生成请求统一返回 429（认证与模型发现均正常） | [#3339](https://github.com/sipeed/picoclaw/issues/3339) | ✅ 已关闭/处理 |
| 中 | RKLLM 模型在 ARM 开发板上回复内容异常（截图可见，细节待补充） | [#3346](https://github.com/sipeed/picoclaw/issues/3346) | ❌ 暂无回应，仅 1 条评论 |
| 低 | LINE 通道 `webhook_host` / `webhook_port` 配置项实际上不生效，易误导用户 | 已由 [#3329](https://github.com/sipeed/picoclaw/pull/3329) 承接 | ✅ 已关闭 |

> 注：#3340 与 #3347 两个修复 PR 均已存在且具备用户验证基础，目前仍处于 open 状态，建议维护者优先评审合入。

## 6. 功能请求与路线图信号

过去 24 小时没有出现典型的 feature request 类型 Issue，但从 Bug 报告和 PR 中可以提取以下路线图信号：

- **通道层依赖治理将成为近期重点**：#3365 将 QQ 频道故障定位到 `botgo v0.2.1` 与 `resty >= v2.17` 的版本兼容性问题。项目可能需要固定/升级 botgo 版本，或在 CI 中增加对间接依赖变更的回归测试，避免上游小版本升级破坏通道 SDK 契约。
- **Web UI 长会话性能优化预计进入下一迭代**：#3281 的持续热度与 #3347 的修复提交表明，前端渲染/输入性能是一个真实且影响面较大的体验短板。合入 #3347 后，建议同步补充针对长历史会话的性能回归测试用例。
- **Slack 媒体能力的补全**：#3338 反映当前 Slack 通道的媒体上传路径并不完整（缺少 `FileSize` 预声明）。这提示多通道能力矩阵中，Slack 的富媒体支持仍需按 Slack API 规范做一次系统性核对。

整体来看，项目短期路线图会更偏向“多通道稳定性修复 + 依赖版本治理”，而非新功能扩张。

## 7. 用户反馈摘要

- **Web UI 长对话场景体验差**：用户反馈在单个 session 中积累较多聊天历史后，“输入框操作非常卡顿”（#3281）。这属于长时间运行后的渐变式性能劣化，对重度用户和客服类使用场景影响明显。
- **Slack 媒体上传失败且没有明确报错路径**：用户指出问题不在网络或权限，而是代码构造上传参数时就漏掉了必要字段，导致所有图片/媒体上传在发起前即被 SDK 拒绝（#3338）。该反馈带有清晰的代码级诊断，开发者体验诉求是“请让错误信息更早、更明确地暴露”。
- **QQ 通道配置门槛高，跨环境一致性失败**：用户在 Docker 与 Linux x86 两种部署形态下得到相同的 401 错误（#3349），说明问题与部署环境无关，而是通道接入/鉴权实现层面的缺陷。用户同时希望文档或日志能更明确地指出 Authorization 头应如何构造。
- **部分用户具备较高的代码追溯能力**：#3365 作者直接定位到 `botgo` + `resty` 的依赖组合问题；#3338 作者也直接指出了缺失的 `FileSize` 字段。这类用户反馈质量很高，维护者若能及时回应，可以有效提升社区信任度。
- RKLLM 用户反馈相对模糊（仅有截图），信息完整度不足，需要维护者主动引导补充环境与复现步骤（#3346）。

## 8. 待处理积压

以下 Issue/PR 已持续较长时间或处于 stale 状态，建议维护者优先处理：

- **[Issue #3281 — Web UI 长历史卡顿（已 stale，7/21 创建）](https://github.com/sipeed/picoclaw/issues/3281)**
  已有贡献者提交修复 PR #3347 且经过作者自测，但 PR 至今未合入。长期悬置可能导致重复提交或贡献者流失，建议尽快评审。

- **[PR #3340 — Slack 媒体上传 FileSize 修复（8/17 创建，已 stale）](https://github.com/sipeed/picoclaw/pull/3340)**
  与 Issue #3338 对应的修复已躺了近三周，属于明确的 bug fix，合入成本低。建议维护者尽快 review 或给出修改意见。

- **[Issue #3346 — RKLLM 在 ARM 上回复异常（8/27 创建，仅 1 条评论）](https://github.com/sipeed/picoclaw/issues/3346)**
  目前没有任何维护者回应。即便是“需要补充更多信息”的回复也有助于降低用户等待焦虑。

- **[Issue #3349 / #3365 — QQ 频道 401 问题（8/30 与 9/4 创建）](https://github.com/sipeed/picoclaw/issues/3365)**
  两个 Issue 指向同一故障，目前无修复 PR 也无维护者确认。建议至少将 #3365 标记为与 #3349 重复/关联，并给出 tentative 的版本处理计划，避免社区重复排查。

---

**报告小结**：PicoClaw 今天没有发布新版本，核心进展是 LINE 通道配置误导问题被清理、Antigravity 429 问题闭环；但 Slack 媒体上传与 Web UI 卡顿两个修复 PR 仍未合入，QQ 频道的 401 问题也急需官方介入。项目整体处于“功能稳定、bug 集中收敛”阶段，社区反馈质量较高，若能加快 PR 合入节奏，项目健康度将明显提升。

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw 项目动态日报 — 2026-09-04

## 1. 今日速览

过去 24 小时 NanoClaw 保持较高的活跃度：共产生 4 条新 Issue（全部来自外部贡献者）与 23 条 PR 状态更新，其中 20 条 PR 仍在等待合并，仅 3 条完成合并/关闭，**合并吞吐明显跟不上提交速度**。新增 Issue 集中在 CLI 路径处理、SQLite 测试并发安全、定时任务重排与扩展点请求四个方面，质量较高且问题定位清晰；新增 PR 覆盖 WhatsApp 适配器、入站内容懒加载、SQLite 连接稳定性与临时目录清理，均为针对性修复。今日无新版本发布。值得注意的信号是：由核心团队主导的 Provider 契约重构系列（#3581-#3592 等）已积压一周以上仍未被合并，正在形成事实上的合并瓶颈。

---

## 2. 版本发布

今日无新版本 Release。

---

## 3. 项目进展

今日可见 2 条 PR 与 1 条 Issue 进入关闭状态：

| 条目 | 类型 | 说明 |
|---|---|---|
| [#3126 fix(agent-runner): never deliver silence, never deliver \<internal\> thinking](https://github.com/nanocoai/nanoclaw/pull/3126) | PR（CLOSED） | 核心 Agent Runner 修复：杜绝向用户交付静默消息与内部思考内容，属于 7 月 24 日发起、8 月下旬以来持续跟进的长期 PR，对消息投递正确性有直接改进 |
| [#3461 chore(deps): bump all @chat-adapter/\* + chat 4.29.0 → 4.38.1](https://github.com/nanocoai/nanoclaw/pull/3461) | PR（CLOSED） | 全量升级 9 个频道适配器依赖包，与 #3460 的 chat 主版本升级配套，避免各频道 skill 在新核心上因适配器版本不一致而失效 |
| [#3426 send_card docs promise callback buttons that the bridge drops](https://github.com/nanocoai/nanoclaw/issues/3426) | Issue（CLOSED） | 低优先级 bug 关闭（8/21 创建），表明该问题已有归属处理 |

与此同时，今日有 4 条新 PR 首次进入队列（#3710、#3711、#3712、#3713），说明贡献者仍在积极提交，但合入节奏未能跟上，积压 PR 已增长至 **20 条待合并**，项目整体处于「提交活跃、合并滞后」的状态。

---

## 4. 社区热点

数据中唯一有明确评论记录的条目为：

- **[#3706 ncl groups config add-mount silently produces a broken double-nested path](https://github.com/nanocoai/nanoclaw/issues/3706)** — 作者 DawoudIO 指出 CLI 的 `--container` 参数文档称其为 "a container path"，用户自然输入绝对路径 `/workspace/shared-repos` 后，命令却静默构造出双重嵌套的坏路径。这一条获得了 1 条评论，是当前唯一被讨论的 Issue。背后诉求是 **CLI 参数校验应更严谨**——要么显式拒绝绝对路径，要么在文档与实现中明确约束，而不是静默接受并产生错误结果。

值得注意的另外两个结构性热点：

- **[#3711 + #3712 关联 PR 组合](https://github.com/nanocoai/nanoclaw/pull/3711)**（mmv 提交）：前者在 router 层引入入站内容懒解析/懒下载，后者依赖前者修复 WhatsApp 文档标题读取与媒体多余下载问题。两个 PR 相互依赖、同日提交，说明频道适配器层存在跨消息类型的通用设计缺口——**每条入站消息无论是否会被 Agent 接收，都必须提前完成昂贵的网络下载**。

- **Provider 契约重构系列**（zvi-fried 提交）：#3581、#3584、#3585、#3586、#3588、#3591、#3592 等 7 个相互关联的 PR 已积压多日，涉及 runtime/setup/host/provider 指令渲染等多个契约层，加上 #3355/#3356 的 Cursor 支持。这是当前最大的一组路线图级改动，但长期未被维护者合入或给出反馈，可能成为社区关注的隐忧。

---

## 5. Bug 与稳定性

今日报告的 Bug 按严重程度排列如下：

| 严重度 | Issue/PR | 描述 | 修复状态 |
|---|---|---|---|
| 中 | [#3706 路径构造错误](https://github.com/nanocoai/nanoclaw/issues/3706) | `ncl groups config add-mount --container <绝对路径>` 会静默产生双重嵌套的容器路径（如把 `/workspace/shared-repos` 变成容器内错误路径），用户难以及时察觉 | ⚠️ 无 PR |
| 中 | [#3705 定时任务不重排](https://github.com/nanocoai/nanoclaw/issues/3705) | `ncl tasks update --recurrence <new-cron>` 不会基于新 cron 表达式重算 `process_after`，从周更改为日更后任务仍按旧的周调度触发 | ⚠️ 无 PR |
| 低（开发体验） | [#3709 测试固定临时路径](https://github.com/nanocoai/nanoclaw/issues/3709) | SQLite 相关测试在固定 `/tmp` 路径下创建 fixture 数据库，并发运行多个 vitest 进程时相互删除数据库，导致开发者在多 worktree/CI 并行场景下测试不稳定 | ⚠️ 无 PR，但已有相关 PR 改善测试残留 |
| 低（测试卫生） | [#3710 测试残留临时目录](https://github.com/nanocoai/nanoclaw/pull/3710) | 完整测试套件每次运行遗留约 355 个临时目录，长期占用 tmpfs/CI 磁盘，只等 30 天 systemd 清理 | ✅ 已有 PR 待合入 |

此外，有 4 条与稳定性直接相关的 PR 今日处于开放状态，均已有明确修复方案：

- **[#3708 busy_timeout 与 journal_mode 执行顺序](https://github.com/nanocoai/nanoclaw/pull/3708)**：`journal_mode` 需要排他锁，若先执行而 `busy_timeout` 未设置，并发下会直接抛 SQLITE_BUSY。将 `busy_timeout` 前置，是对 SQLite 并发场景的务实修复。
- **[#3462 防止消息重复投递](https://github.com/nanocoai/nanoclaw/pull/3462)**：为 mid-turn block door 已投递的内容增加发送防护，修复双投递 bug 类（关联 #2404）。
- **[#3440 Docker 驱动修复](https://github.com/nanocoai/nanoclaw/pull/3440)**：解决 SELinux 阻止挂载、group-writable rw 挂载失败及 stray NUL byte 三个容器运行时问题。
- **[#3712 WhatsApp 媒体处理](https://github.com/nanocoai/nanoclaw/pull/3712)**：读取文档 caption 并停止下载无人需要的媒体，依赖 #3711 合入后才能完全生效。

---

## 6. 功能请求与路线图信号

今日新出现的功能/能力请求与路线图信号包括：

- **[#3704 受保护的 session-assembly hook](https://github.com/nanocoai/nanoclaw/issues/3704)**（davekim917）：fork 维护者请求在 `SqliteAgentMailbox` 上增加一个 protected 的 `session-assembly` 扩展钩子，以便子类在不替换 compose 组合根的前提下携带 fork 专有的表/列/触发器。当前该 fork 只能通过 `compose.ts` 唯一插槽整体替换实现。这是一个典型的**扩展点诉求**，与 [#3707 registerAdmissionGate poll-loop seam](https://github.com/nanocoai/nanoclaw/pull/3707) 属于同一模式——外部维护者需要更细粒度的接缝来扩展核心容器。

- **[#3713 记录 per-agent-group 投递模式](https://github.com/nanocoai/nanoclaw/pull/3713)**（glifocat）：允许为每个 agent group 记录其使用的投递契约（envelope 消息 vs. outbound 工具），解决不支持 `<message to>` envelope 约定的模型/提供商无法纳入现有组管理的问题。目前只落库列与管道，尚未有消费者，属于前置铺路改动。

- **入站内容延迟物化**（[#3711](https://github.com/nanocoai/nanoclaw/pull/3711)）：将昂贵的内容加载从「所有入站消息」推迟到「Agent 将真正接收的消息」，减少不必要的网络下载与带宽消耗。不仅修 bug，更是**频道适配器层的基础架构改进**，未来所有新适配器都会受益。

- **Provider 契约体系与 Cursor 支持**：#3581-#3592 系列把 provider 定义从硬编码/自由文本逐步收编为可验证的声明式契约，并同步新增 Cursor Agent SDK 支持（#3356 + #3355）。该方向带有清晰的平台化意图，但多日未获合入反馈，需维护者明确推进节奏。

---

## 7. 用户反馈摘要

从今日 Issue 中可以提炼出以下真实用户痛点：

- **CLI 文档与实现不一致会直接坑害用户**（#3706）：维护者认为 `--container` 参数「按直觉输入绝对路径即可」，但实现端静默产出坏路径且无任何报错。用户 DawoudIO 的表述 "the natural thing to type given every other con…" 说明这不是个例误操作，而是**交互设计缺陷**——此类命令对绝对/相对路径应显式归一化并给出警告。

- **行为 bug 正在侵蚀信任**（#3705）：用户更新任务频率后，系统仍按旧时间表执行。DawoudIO 精确指出 `process_after` 未重算的根因，但在真实使用中，这类问题最危险的地方是**用户会误以为新调度已生效**，属于静默逻辑错误。

- **调试成本转嫁给使用者**（#3426，已关闭）：Agent 能发送带按钮的卡片，但 bridge 静默丢弃所有无 URL 的按钮动作，Agent 只能根据 `fallbackText` 猜测原因并向用户解释「平台不支持按钮」。用户会错误地责怪平台。这暴露了**能力协商机制缺失**——平台能力应当在工具调用前就已知，而不是在消息送达后被丢弃。

- **测试基础设施影响真实开发**（#3709）：多个 worktree 或 CI 并行运行 vitest 会互相删除数据库，davekim917 的 fork 工作流直接受影响。这虽然不是一个终端用户功能 bug，但对贡献者友好度有明显负面影响。

---

## 8. 待处理积压

以下为需要维护者关注的长周期未响应/未合入条目：

| 条目 | 创建时间 | 积压天数 | 重要性 |
|---|---|---|---|
| [#2003 语音转录 V2（container-side, sovereign by default）](https://github.com/nanocoai/nanoclaw/pull/2003) | 2026-04-25 | **132 天** | 高。PR 本身已按维护者意见重构，将实现移入 agent container，且明确 closes #1879。长期悬置可能打击贡献者积极性 |
| [#3355 / #3356 Cursor Agent 支持（skill + SDK payload）](https://github.com/nanocoai/nanoclaw/pull/3356) | 2026-08-19 | 16 天 | 高。新 provider 的完整实现，与 #3581-#3592 的 provider 契约重构直接相关，建议按依赖关系安排评审，否则两者将持续互相阻塞 |
| [#3440 Docker driver 修复（SELinux/挂载权限/NUL 字节）](https://github.com/nanocoai/nanoclaw/pull/3440) | 2026-08-22 | 13 天 | 高。影响容器场景的可用性，且今日（9-04）有更新，说明作者仍在跟进，合入窗口应尽快给出 |
| [#3462 send_message 防重复投递修复](https://github.com/nanocoai/nanoclaw/pull/3462) | 2026-08-23 | 12 天 | 中高。关联 #2404 同类双投递 bug，属消息正确性问题 |
| zvi-fried Provider 契约重构系列（#3581 #3584 #3585 #3586 #3588 #3591 #3592） | 2026-08-27 | 8 天 | 高。7 个相互关联的 PR 已全部超过一周未合入，虽每日有更新，但长时间不合并会带来与主线持续冲突的合并成本 |

> **维护者提醒**：当前最突出的健康度风险不在 bug 数量，而在 **20 条 PR 的合并等待时间持续拉长**，且其中包含一个 132 天的语音转录 V2 和一个 16 天的完整 Cursor 提供方支持。考虑到 Provider 契约重构系列（7 条 PR）之间存在强依赖关系，建议安排专项评审 session 一次性消化，避免逐条处理带来的上下文切换成本。

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw 项目动态日报 — 2026-09-04

> 数据窗口：2026-09-03 → 2026-09-04（GitHub 活动）

## 今日速览

过去 24 小时项目活跃度很高：11 条 Issue 更新（8 条活跃、3 条关闭），18 条 PR 更新（8 条待合并、10 条合入/关闭），未发布新版本。主干稳定性先受损后快速修复：#8038 合入引入的两处测试失败，由 [#8055](https://github.com/nearai/ironclaw/pull/8055) 与 [#8058](https://github.com/nearai/ironclaw/pull/8058) 在 24 小时内修复合入，主干恢复绿色，解除对全部悬置 PR 的阻塞。WebUI TypeScript 抑制清理系列（#8037-#8040）收官，对应追踪 Issue [#8033](https://github.com/nearai/ironclaw/issues/8033)、[#8035](https://github.com/nearai/ironclaw/issues/8035)、[#8036](https://github.com/nearai/ironclaw/issues/8036) 一并关闭。开发主线集中在 LLM 上下文预算/缓存（[#8053](https://github.com/nearai/ironclaw/pull/8053)、[#8062](https://github.com/nearai/ironclaw/pull/8062)、[#8044](https://github.com/nearai/ironclaw/pull/8044)）以及 subagent R3 审批链路（[#8046](https://github.com/nearai/ironclaw/pull/8046) 已合入、[#8061](https://github.com/nearai/ironclaw/pull/8061) 待审）。整体判断：项目处于高强度的核心迭代期，合并质量与主干健康度良好。

## 项目进展

### 主干稳定性：由红转绿

- [#8055](https://github.com/nearai/ironclaw/pull/8055)：修正 WebUI 资产测试仍引用旧路径的问题，恢复 `sidebar_trace_credits_card_assets_are_embedded`。该 PR 标题明确表示“main is currently red and this unblocks every open PR”。
- [#8058](https://github.com/nearai/ironclaw/pull/8058)：修复新 api-boundary 测试中使用已退役 `web-push` 拼写导致的架构门禁失败，改为项目当前实际使用的 `web-app` 扩展 ID。

两个问题均源于同一前端提交（#8038 的 `666ebcbf0`）。回归虽由 #8038 引入，但在窗口内即完成修复闭环。

### WebUI TypeScript 技术债收官

四组 PR 在窗口内全部合入/关闭，是过去一天最大的技术债清理项：

- [#8037](https://github.com/nearai/ironclaw/pull/8037)：移除 40 个冗余 `@ts-nocheck`，新增受检的 legacy 抑制基线与 CI ratchet，防止新增 `@ts-nocheck`/`@ts-ignore`。
- [#8038](https://github.com/nearai/ironclaw/pull/8038)：为前端 API 边界补上类型化结果与运行时解码器，在构造请求 URL 前拒绝缺失的 thread/run/gate 标识符。
- [#8039](https://github.com/nearai/ironclaw/pull/8039)：为 64 个生产组件、hooks、页面及局部 helper 移除 `@ts-nocheck`。
- [#8040](https://github.com/nearai/ironclaw/pull/8040)：移除全部 94 处测试端 `@ts-nocheck`，集中处理不可避免的动态 VM 边界。

这组改动让 WebUI v2 从生产代码到测试基础设施的 TypeScript 覆盖进入“ratchet”模式，后续新增抑制会被 CI 拦下。

### Subagent R3 继续落地

- [#8046](https://github.com/nearai/ironclaw/pull/8046)（R3 slice 3a）已合入：子 agent 子任务在自身 approval/credential gate 上被阻塞时，会进入 owner 的 inbox，而不是在父进程中静默不可见。

这补齐了子代理“卡住时无人知晓”的关键缺口。R3 后续 slice 3b（子任务并发上限与子门卡片回放验证）由 [#8061](https://github.com/nearai/ironclaw/pull/8061) 承接，目前待审。

### 性能与工具可靠性

- [#8043](https://github.com/nearai/ironclaw/pull/8043)：loop-host 将流式文本更新改为合并发送，避免每个 provider delta 都对全文重新消毒与克隆，消除了 O(N·k) 的重复开销。
- [#7984](https://github.com/nearai/ironclaw/pull/7984)：`tool_search` 回复按模型 first-look envelope 裁剪。此前 10 条命中序列化约 16,066 B，到达模型端时却被裁成 857 B，整个 `results` 数组被替换为单个 `omitted` 标记，工具结果形同虚设。

### CI 基建

- [#8060](https://github.com/nearai/ironclaw/pull/8060)：为三个 `crates/` 全树架构扫描二进制补足超时余量。此前实测单次扫描约 176.8 s，而 CI profile 的硬超时是 60 s × 3 = 180 s，余量仅 3.2 s，随时可能误杀。

## 社区热点

过去 24 小时的公开讨论密度并不高，绝大多数 PR 评论为 0，真正形成讨论的 Issue 集中在两个：

- [#7903](https://github.com/nearai/ironclaw/issues/7903)（2 条评论，`[risk: high]`）：探讨在可信宿主内核之后为每个用户放置持久化沙箱执行器的可能性。作者指出当前 Reborn 架构虽然保留了清晰的权限边界，但“每个新 CLI 都需要在 host 与 sandbox 间做命令管道”，扩展成本高。这是围绕 agent/sandbox 边界的一次重要架构决策 spike，已开放 9 天，值得持续跟踪。

- [#8009](https://github.com/nearai/ironclaw/issues/8009)（1 条评论）：MCP egress 错误被折叠成单一 `response_error` token，底层原因与字节计数全部丢失，导致 hosted-MCP 发现失败无法诊断。开发者关注的是可观测性而非单纯报错。

## Bug 与稳定性

按严重程度排序：

1. **主干 CI 红灯（高 · 已修复）**：#8038 合入后引发的两类测试失败已由 [#8055](https://github.com/nearai/ironclaw/pull/8055) 与 [#8058](https://github.com/nearai/ironclaw/pull/8058) 修复，所有悬置 PR 解除阻塞。
2. **Responses cancel API 无法成功（高 · 修复待合入）**：[#8059](https://github.com/nearai/ironclaw/pull/8059) 指出 `POST /api/v1/responses/{id}/cancel` 在 in-progress 和 completed 状态下都返回 `400 invalid_request`，且 run 会继续执行。根因是硬编码的 cancel reason 不符合 `parse_cancel_reason` 的期望格式。PR 已开出，等待合入。
3. **Host-API 工具预览切片可致 panic（中高 · 修复待合入）**：[#8056](https://github.com/nearai/ironclaw/pull/8056) 修复了畸形嵌入式工具结果文本在“先出现 closing delimiter、后出现 opening delimiter”时触发 panic 的问题，将无检查切片改为 checked lookup，并保留原有 fail-closed 全文脱敏回退。
4. **MCP egress 错误无法诊断（中 · 未修复）**：[#8009](https://github.com/nearai/ironclaw/issues/8009) 将 `RuntimeHttpEgressError` 全部拍平成稳定 reason code，调用方只能拿到 `response_error`。目前无对应 fix PR。
5. **WebUI 命令结果卡持续收缩（低 · 未修复）**：[#8066](https://github.com/nearai/ironclaw/issues/8066) 报告执行 `/model` 等命令多次后，旧结果卡会缩成只剩边框的水平线，属于 flex 布局中被压缩而非保持内容高度的问题。
6. **失败分类数据（信息）**：[#8052](https://github.com/nearai/ironclaw/issues/8052) 例行发布 2026-09-03 失败分类。officeqa 的 63 个 non-pass 全部被判定为 deepseek-v4-flash 在 OCR 版 Treasury Bulletins 上的真实模型质量问题，没有基础设施或框架缺陷。

## 功能请求与路线图信号

- **LLM 上下文预算与提示缓存是本阶段明确主线**：已开 PR [#8053](https://github.com/nearai/ironclaw/pull/8053)（从模型广告窗口按 90% 推导 prompt context budget）、[#8062](https://github.com/nearai/ironclaw/pull/8062)（为 OpenAI Responses/Chat Completions 路径发送稳定、域隔离的伪匿名 conversation cache key）以及 [#8044](https://github.com/nearai/ironclaw/pull/8044)（Claude 新家族缓存放行策略由 allowlist 改为 denylist）。三者合在一起，指向“精确测量并复用模型上下文”的下一阶段能力。

- [#8057](https://github.com/nearai/ironclaw/issues/8057) 指出了上述方向的缺口：prompt budget 目前只计算 transcript，identity、skill/memory 片段、channel context、tool schema 等非 transcript 材料只是在 transcript allowance 之上叠加，并不抵扣预算。该 Issue 与 #8053 应配套设计，否则即使 #8053 合入，实际请求仍可能超出预算。

- **Slash 命令菜单/结果卡形成明确 UX 批次**：同一提交者在一天内开出 [#8063](https://github.com/nearai/ironclaw/issues/8063)（菜单滚动保持 active command 可见）、[#8064](https://github.com/nearai/ironclaw/issues/8064)（结果卡支持 dismiss）、[#8065](https://github.com/nearai/ironclaw/issues/8065)（命令元数据对齐）、[#8066](https://github.com/nearai/ironclaw/issues/8066)（防止结果卡收缩）。这些看起来是一个完整的 WebUI 聊天体验改进集合，尚无对应 PR，可能由同一提交者后续实现。

- **Reborn/Subagent 架构仍在演进**：#8046 已合入的 inbox gate 链路与 [#8061](https://github.com/nearai/ironclaw/pull/8061) 的并发子代理上限，加上高风险决策 spike [#7903](https://github.com/nearai/ironclaw/issues/7903)，说明 agent 执行边界与沙箱模型仍是 IronClaw 最核心的架构讨论方向。

## 用户反馈摘要

以下内容摘自 Issue 正文与评论，反映的是一线开发者遇到的实际问题：

- **MCP 诊断链断裂（[#8009](https://github.com/nearai/ironclaw/issues/8009)）**：“hosted-MCP discovery failure reaches the caller as the single token `response_error` with nothing behind it。”集成方只能看到错误代号，无法判断是网络、协议、鉴权还是超时问题。

- **沙箱架构扩展成本（[#7903](https://github.com/nearai/ironclaw/issues/7903)）**：作者认同“agent loop 保留在 host、仅 shell 进沙箱”的权限边界，但明确表达“every new CLI requires host-to-sandbox command plumbing”的维护负担。这是驱动持久化沙箱执行器探索的直接诉求。

- **命令结果卡缺少关闭操作（[#8064](https://github.com/nearai/ironclaw/issues/8064)）**：“The card looks like a temporary panel, but it has no close or dismiss action.” 用户反复执行 slash command 后，结果卡只能累积并占用会话空间。

- **结果卡不可读（[#8066](https://github.com/nearai/ironclaw/issues/8066)）**：多次执行命令后，“previously rendered command result cards shrink until only their borders remain visible as horizontal lines”，本质上等同于界面元素丢失。

- **模型质量基线记录（[#8052](https://github.com/nearai/ironclaw/issues/8052)）**：officeqa 上的 63 个 non-pass 全部归因于模型对 OCR 后 Treasury Bulletins 的理解质量，而非框架缺陷。这对基准语义是一个正向信号：IronClaw 的测试基础设施没有把基础设施问题误报成模型问题。

## 待处理积压

按需要维护侧关注的程度排序：

1. **[#7903](https://github.com/nearai/ironclaw/issues/7903)（risk: high，开放 9 天）**：持久化 per-user 沙箱执行器的决策 spike 始终只有 2 条评论，尚无 PR 或明确结论。作为带 `risk: high` 标签的架构议题，建议安排一次集中评审。

2. **[#8009](https://github.com/nearai/ironclaw/issues/8009)（开放 4 天）**：MCP 错误信息被压平的问题直接影响 hosted-MCP 生态的可诊断性，目前无 fix PR。

3. **[#7988](https://github.com/nearai/ironclaw/pull/7988)**：来自 `ironclaw-ci[bot]` 的 codebase knowledge graph 自动刷新 PR，自 8 月 29 日开放至今已 7 天，无评论、无合并。建议维护者按常规节奏 review，避免 nightly 产物持续积压。

4. **LLM 上下文预算/缓存 PR 组（[#8053](https://github.com/nearai/ironclaw/pull/8053)、[#8062](https://github.com/nearai/ironclaw/pull/8062)、[#8044](https://github.com/nearai/ironclaw/pull/8044)）**：三者均为 XL 尺寸且代码路径相关，其中 #8044 与 #8062 在 OpenAI Responses 缓存 key 上存在功能交集。建议先合入 #8053 的成本模型，再系统评审两条缓存 key 实现，避免返工。

5. **小修复等待队列（[#8059](https://github.com/nearai/ironclaw/pull/8059)、[#8056](https://github.com/nearai/ironclaw/pull/8056)、[#8054](https://github.com/nearai/ironclaw/pull/8054)）**：分别覆盖 cancel API、panic 修复和 Telegram 首次配对体验。其中 #8054 来自 experienced 贡献者而非 core 成员，建议优先 review，避免外部贡献等待过久。

6. **Slash 命令菜单/结果卡 UX 组（[#8063](https://github.com/nearai/ironclaw/issues/8063)-[#8066](https://github.com/nearai/ironclaw/issues/8066)）**：刚开出且暂无 PR 认领。鉴于它们是同一提交者集中报告的一批改进，建议尽快 triage，决定纳入本迭代还是后续迭代。

当前没有超过两周无人更新的僵尸 Issue；积压以近一周新增的 XL 级 PR 和 UX 改进请求为主，整体健康度可控。

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# LobsterAI 项目动态日报 · 2026-09-04

## 1. 今日速览

过去 24 小时处于 2026.8.31 发布窗口后的密集收尾期，PR 活动量很高：15 条 PR 更新中有 10 条已合并/关闭，且多为发布补丁与体验修复。Issue 侧有 2 个历史问题被自动关闭，4 个旧 Issue 被生命周期机制重新激活；真正新增的需求只有 1 个，即针对 MCP Apps 桌面渲染的 [Issue #2601](https://github.com/netease-youdao/LobsterAI/issues/2601)。合并内容集中在安装/更新流程、Windows 安装器、OpenClaw/MCP 架构瘦身、IM/语音体验等方向，整体节奏显示 `2026.9.4` 版本线已在推进中。值得关注的是，仍有 5 个 PR 与若干高价值 Issue 自 3 月底滞留至今，项目健康度受长期积压拖累。

---

## 3. 项目进展

### 2026.8.31 Release 收尾
- [PR #2600](https://github.com/netease-youdao/LobsterAI/pull/2600) 合并，正式准备 `2026.8.31` 发布。主要包括：引导式首次运行体验、Library 浏览加速、客户端支持分享模型生成视频、登录与配额文案梳理、Windows 安装程序恢复能力增强。

### 应用内浏览器能力回归（瞄准 2026.9.4）
- [PR #2602](https://github.com/netease-youdao/LobsterAI/pull/2602) 合并，覆盖 renderer/main/openclaw/cowork/artifacts 多个模块。恢复可交互的 in-app Agent Browser、browser MCP bridge、持久化浏览器 profile、加密凭据保存、审批制 Agent 自动填充、手动凭据采集与设置管理。这是浏览器模块的一次整体性恢复，而非单点修复。

### 更新流程与退出提示优化
- [PR #2609](https://github.com/netease-youdao/LobsterAI/pull/2609) 合并：当 Agent turn 或定时任务仍在运行时发起安装，会先弹确认框，避免静默打断；移除下载中途取消操作，改为“静默下载完成后通知”；同时在 Cmd+Q、应用菜单、Dock、托盘退出时增加二次确认，降低误操作率。

### Windows 安装器与打包修复
- [PR #2605](https://github.com/netease-youdao/LobsterAI/pull/2605)：将 Windows 安装器声明为 DPI-aware，修复图标模糊问题。
- [PR #2606](https://github.com/netease-youdao/LobsterAI/pull/2606)：辅助进程启动时不再弹出控制台窗口。

### OpenClaw 架构精简与体积优化
- [PR #2607](https://github.com/netease-youdao/LobsterAI/pull/2607)：停止 peer install，避免插件包体积膨胀。
- [PR #2608](https://github.com/netease-youdao/LobsterAI/pull/2608)：dsh 不再注册为 MCP server，也不再把编码任务委派给 LobsterAI；移除 `dshCodeMcpServer` / `dshSessionClient`、`McpRuntime` resolution hook，并停止因 dsh feature toggle 而重新同步 OpenClaw 配置。

### IM / 语音体验打磨
- [PR #2599](https://github.com/netease-youdao/LobsterAI/pull/2599)：多实例 bot 卡片限制为双列响应式布局；空“添加 bot”卡片保持紧凑，内容垂直居中。
- [PR #2604](https://github.com/netease-youdao/LobsterAI/pull/2604)：当日 ASR 配额耗尽后，语音输入按钮进入稳定的置灰态，但仍可点击以打开配额提示；补充了对应测试。
- [PR #2603](https://github.com/netease-youdao/LobsterAI/pull/2603)：中文语音配额耗尽文案更新为“免费试用订阅”相关措辞，并改为紧凑的时间格式，文案更清晰。

总体来看，今日合入集中在“发布收尾、可交互浏览器恢复、高频 UI 细节优化”三条线上，项目正为下一版本线铺平道路；没有出现大规模新功能合入，属稳定性冲刺型的一天。

---

## 4. 社区热点

- [Issue #2601](https://github.com/netease-youdao/LobsterAI/issues/2601)：**在桌面客户端渲染 MCP Apps / Prefab UI** —— 这是今日唯一新开的 Issue，也是需求侧最重要的信号。用户希望支持 `io.modelcontextprotocol/ui` 扩展，使 PrefectHQ Prefab、FastMCP 等 MCP server 返回的 `ui://` 交互式 HTML UI 能直接在 LobsterAI 桌面端渲染，而不是只显示 `text/html` 原始结果。诉求本质是“MCP 生态正在从工具调用走向 UI 原生嵌入”，对桌面 Agent 客户端的架构方向有参考价值。

- [Issue #1556](https://github.com/netease-youdao/LobsterAI/issues/1556)：虽然是 stale 自动关闭，但它是近期评论数最多（3 条）的 Issue 之一。用户反馈 `LobsterAI-IM机器人配置指南.md` 链接返回 404。对文档型机器人配置页来说，404 会直接影响 IM 集成上手体验，需确认是否真的已修复。

---

## 5. Bug 与稳定性

### 高严重度 —— 仍未处理
- [Issue #1089](https://github.com/netease-youdao/LobsterAI/issues/1089)：`CoworkRunner.startSession()/continueSession()` 缺少 per-session 重入保护。IPC 层 fire-and-forget（`.catch()` 不 `await`）调用时，若用户连续发消息或 IM 网关批量投递，同一 sessionId 可能有两个异步执行同时进入事件流迭代，并发修改共享 `ActiveSession` 状态，导致**流式消息损坏和消息重复**。报告自 3 月 31 日至今未关闭，也未见关联修复 PR，建议优先处理。
- [Issue #1088](https://github.com/netease-youdao/LobsterAI/issues/1088)：`prefetchChannelUserMessages` 是异步“发后即忘”操作，恢复时只按 `sessionId` 取当前活跃 turn，不校验该 turn 是否为发起 prefetch 的原始 turn。若 Turn A 的 prefetch 在 Turn B 开始后才恢复，会把 pendingUser 消息错误写入 Turn B，造成**跨轮次状态污染**。同样缺少关联 fix PR。

### 中严重度 —— 已关闭但修复存疑
- [Issue #1556](https://github.com/netease-youdao/LobsterAI/issues/1556)：IM 机器人配置指南 404。Issue 被 stale 机制关闭，但所有 PR 列表中未看到对应“docs 修复”的合入记录，需要维护者确认文档链接是否已实际修好，避免“关掉了问题，但问题还在”。

### 低严重度 —— 已有修复合入
- [PR #2605](https://github.com/netease-youdao/LobsterAI/pull/2605)：Windows 安装器图标模糊问题（DPI）已修复。
- [PR #2606](https://github.com/netease-youdao/LobsterAI/pull/2606)：Windows helper 进程弹出控制台窗口问题已修复。
- [PR #2604](https://github.com/netease-youdao/LobsterAI/pull/2604)：语音配额耗尽状态不直观的问题已修复。
- [PR #2603](https://github.com/netease-youdao/LobsterAI/pull/2603)：配额耗尽文案中英混杂的问题已修复。

---

## 6. 功能请求与路线图信号

- [Issue #2601](https://github.com/netease-youdao/LobsterAI/issues/2601)：**MCP Apps / Prefab UI 渲染支持**。该 Issue 与 [PR #2602](https://github.com/netease-youdao/LobsterAI/pull/2602) 恢复的 in-app Browser + browser MCP bridge 方向高度相关——一旦桌面端 Web 容器能力稳定，渲染 MCP 返回的交互式 HTML 在技术上已有可承接基础。建议维护者在路线图中显式评估“MCP UI 渲染策略”，这是 2026 年 MCP 生态的重要演进方向。

- [Issue #1552](https://github.com/netease-youdao/LobsterAI/issues/1552)：AI 产物 Markdown 预览与文件卡片支持。该请求描述了非常具体的使用痛点：Agent 用 Write 工具生成 Markdown/HTML 文件后，用户只能把全文 Read 到聊天流中，或手动切文件管理器。期望是 Write 完成后直接在会话中显示带图标、路径、类型标签的文件卡片，并支持预览。该 Issue 已被 stale 关闭，但产品侧价值明显，建议在 Artifacts 相关迭代中复用。

---

## 7. 用户反馈摘要

- **文档可访问性问题**：[Issue #1556](https://github.com/netease-youdao/LobsterAI/issues/1556) 用户直接贴出 404 截图，说明公开文档与站点实际路径不一致，对 IM 机器人配置的新用户构成实际阻碍。
- **生成内容的消费效率**：[Issue #1552](https://github.com/netease-youdao/LobsterAI/issues/1552) 中用户给出的场景是“写作/文档生成”。用户明确表示：把全文读到聊天里占用大量对话空间，手动切到文件管理器又打断流程，希望用 FileCard + 预览解决。
- **合规驱动的更新压力**：[Issue #1082](https://github.com/netease-youdao/LobsterAI/issues/1082) 用户提到 `openclaw.version` 停留在 `v2026.3.2`，并援引国家互联网应急中心对“更新到最新版本”的要求，反映出政府/合规场景下用户对组件版本滞后有明显焦虑。这不只是技术问题，而是企业采购与合规层面的信号。
- **MCP 生态的前沿需求**：[Issue #2601](https://github.com/netease-youdao/LobsterAI/issues/2601) 用户提出的不是单纯的“加载网页”，而是让外部 MCP server 通过 `ui://` 资源直接驱动客户端 UI。如果 LobsterAI 想要承接更多企业级 MCP 生态流量，这很可能会成为桌面端差异化能力。

---

## 8. 待处理积压

### 超 150 天未合并的 PR（5 个）
- [PR #1277](https://github.com/netease-youdao/LobsterAI/pull/1277)：`electron` 40.2.1 → 44.0.0 依赖升级，创建于 4 月 2 日。长时间未合并会积累大量冲突，且对安全性和稳定性影响较大，建议排期处理。
- [PR #1078](https://github.com/netease-youdao/LobsterAI/pull/1078)：定时任务失败时向 IM 推送告警通知。当前成功有通知、失败无通知，行为不对称。这是错误可观测性的有效补充。
- [PR #1079](https://github.com/netease-youdao/LobsterAI/pull/1079)：Cowork 会话页新增「当前进程」右侧面板，展示工具执行记录与 Write/Edit diff 红绿高亮。属于提升 Agent 可解释性的直接贡献。
- [PR #1081](https://github.com/netease-youdao/LobsterAI/pull/1081)：MCP 工具同步提示国际化和编辑弹窗滚动条圆角溢出修复。小而明确的 UI/UX 修复。
- [PR #1087](https://github.com/netease-youdao/LobsterAI/pull/1087)：修复 `continueSession()` 失败时展示两条重复错误消息的问题。与 [Issue #1089](https://github.com/netease-youdao/LobsterAI/issues/1089) 同属 Cowork 会话健壮性问题，建议一并 review。

### 超 150 天未关闭的 Issue（3 个）
- [Issue #1088](https://github.com/netease-youdao/LobsterAI/issues/1088)：Prefetch 异步回调跨轮次污染，建议作为 P1 处理。
- [Issue #1089](https://github.com/netease-youdao/LobsterAI/issues/1089)：CoworkRunner 无重入保护导致消息损坏/重复，建议作为 P1 处理。
- [Issue #1082](https://github.com/netease-youdao/LobsterAI/issues/1082)：OpenClaw 版本停留在 2026.3.2 且存在合规压力。无论确认为“有意锁定”还是“计划升级”，都值得给用户一个官方回应。

### 其他提醒
- [Issue #1556](https://github.com/netease-youdao/LobsterAI/issues/1556) 已被 stale bot 关闭，但 PR 历史中未见对应修复，建议确认文档 404 是否真实修复，不要因自动关闭造成问题静默沉底。

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

# CoPaw 开源项目动态日报 · 2026-09-04

> 数据窗口：过去 24 小时 | 数据源：github.com/agentscope-ai/CoPaw

## 1. 今日速览

过去 24 小时项目整体处于**高活跃度**状态：Issue 更新 27 条（新开/活跃 19，关闭 8），PR 更新 36 条（待合并 21，已合并/关闭 15），无新版本 Release。安全与治理是今日主线：CRITICAL 指令审批逻辑缺陷由 [PR #7525](https://github.com/agentscope-ai/CoPaw/pull/7525) 修复并关闭；但安全研究者连续提交的沙箱突破与危险指令逃逸类问题（[#7511](https://github.com/agentscope-ai/CoPaw/issues/7511)、[#7443](https://github.com/agentscope-ai/CoPaw/issues/7443)）仍是社区关注焦点。此外，v2.2.0 Stable 的安装验证任务 [Issue #7515](https://github.com/agentscope-ai/CoPaw/issues/7515) 已关闭，一批移动端/UI 优化 PR 集中合入，说明项目正处于 **2.2.x 稳定性修复与体验打磨并行**的阶段。

| 指标 | 24 小时数值 |
|---|---:|
| Issue 新开/活跃 | 19 |
| Issue 关闭 | 8 |
| PR 待合并 | 21 |
| PR 已合并/关闭 | 15 |
| 新版本发布 | 0 |

## 2. 版本发布

今日无新版本发布。上一版本 v2.2.0 Stable 的安装验证任务 [Issue #7515](https://github.com/agentscope-ai/CoPaw/issues/7515) 于窗口内关闭。

## 3. 项目进展

以下 PR 于本统计窗口内被置为 Closed（按平台统计口径含已合并）：

- **[PR #7525 — fix(governance): require approval for non-auto-denied critical findings](https://github.com/agentscope-ai/CoPaw/pull/7525)**  
  修复 [Issue #7496](https://github.com/agentscope-ai/CoPaw/issues/7496)：CRITICAL 类型规则此前会被直接拒绝执行，与安全 UI 中「需人工审批」的行为描述不符。现在仅在规则显式配置为自动拒绝时才直接阻断，否则进入审批流程。这是治理/安全链路的重要修正。

- **[PR #7498 — fix(tools): return 404 when updating config for an unknown tool](https://github.com/agentscope-ai/CoPaw/pull/7498)**  
  将未知工具配置更新从 HTTP 500 改为语义正确的 404，提升 API 错误可诊断性。

- **[PR #7524 — fix(console): separate free models from pro tab](https://github.com/agentscope-ai/CoPaw/pull/7524)**  
  控制台模型选择器不再把免费模型混入 PRO 标签页，并补充了「同一提供商同时含免费/付费模型」的回归测试。

- **[PR #5399 — feat(providers): support custom model ordering within providers](https://github.com/agentscope-ai/CoPaw/pull/5399)**  
  支持在提供商内通过拖拽或上下按钮自定义模型排序，结果持久化到后端。

- **[PR #5394 / #5363 / #5334 — 移动端与窄屏体验批量合入](https://github.com/agentscope-ai/CoPaw/pull/5394)**  
  Plugin Manager 卡片布局、Settings/Agents 移动端响应式改造、折叠侧边栏下可切换 Agent。三者共同补齐了 Console 在手机/窄视口下的核心操作路径。

- **[PR #7080 — feat: PowerContext pluggable long-term memory backend](https://github.com/agentscope-ai/CoPaw/pull/7080)**  
  由 first-time-contributor 提交，新增可选 PowerContext 长期记忆后端，与 ReMeLightMemoryManager 形成可替换的 peer 实现。

- **[PR #7267 — fix(channels): make contract checks portable and complete](https://github.com/agentscope-ai/CoPaw/pull/7267)**  
  修复非 UTF-8 代码页 Windows 环境下渠道契约检查误报/漏报问题，并确保所有内置渠道都有可运行的契约测试。

整体看，**今日合入内容覆盖了安全治理、模型管理、移动端 UI、长期记忆与渠道测试基础设施**，2.2.0 发布后的后续迭代方向已比较清晰。

## 4. 社区热点

- **[Issue #7318 — 「QwenPaw Hub 多租户版 2.2.0 应该做什么？」](https://github.com/agentscope-ai/CoPaw/issues/7318)**  
  🔥 17 条评论，3 👍，是今日讨论热度最高的 Issue。官方在 2.2.0 多租户 Hub 发布前公开征询方向，社区呼声集中在**多用户访问与管理员可管理的技能体系**（引用了 [#2324](https://github.com/agentscope-ai/CoPaw/issues/2324)）。释放的信号：用户正推动 QwenPaw 从「个人 AI 助手」走向「团队共享基础设施」。

- **[Issue #7511 — 安全沙箱被突破（已关闭）](https://github.com/agentscope-ai/CoPaw/issues/7511)**  
  9 条评论。安全研究者通过知乎公开了 QwenPaw2 安全沙箱的 PoC。Issue 虽已关闭，但窗口内未看到与之直接对应的 fix PR 或安全公告，建议维护者补充后续处理说明。

- **[Issue #7505 — 局域网 LLM Server 频繁 client disconnect 致超时](https://github.com/agentscope-ai/CoPaw/issues/7505)**  
  7 条评论。用户通过局域网访问 LM Studio + Qwen3.8 时频繁触发客户端断开、重试直至超时。反映出**本地/局域网推理部署的流式连接稳定性**仍有改进空间。

- **[Issue #7541 — 俄语社区发起架构质疑：会话是否应按渠道隔离？](https://github.com/agentscope-ai/CoPaw/issues/7541)**  
  用户认为「渠道只是传输层，不应按 Web/Desktop/Telegram 等渠道切分和锁定会话」，希望跨渠道看到统一会话。这是一个值得产品团队关注的架构级反馈。

## 5. Bug 与稳定性

按严重程度排列：

### 🔴 严重 / 安全相关

- **[Issue #7443 — 危险指令容易逃逸安全规则（开放）](https://github.com/agentscope-ai/CoPaw/issues/7443)**  
  6 条评论，提交者与 #7511 为同一安全研究者。目前未在 PR 窗口内看到对应修复。

- **[Issue #7511 — 安全沙箱被突破（已关闭）](https://github.com/agentscope-ai/CoPaw/issues/7511)**  
  见社区热点。严重度高，建议关注是否有后续 security advisory。

- **[Issue #7496 — CRITICAL 规则被直接拒绝而非触发审批（已关闭）](https://github.com/agentscope-ai/CoPaw/issues/7496)**  
  ✅ 已由 [PR #7525](https://github.com/agentscope-ai/CoPaw/pull/7525) 修复。

### 🟠 中高 / 功能异常或静默失败

- **[Issue #7469 — ReMe 后台 embedding/indexing 任务静默失败（开放）](https://github.com/agentscope-ai/CoPaw/issues/7469)**  
  `as_embedding:default accessed before start()`，新记忆不会被写入且仅记日志。存在**记忆数据丢失风险**，目前无对应 fix PR。

- **[Issue #7534 — 飞书会话 consumer 卡死、会话静默无响应（开放）](https://github.com/agentscope-ai/CoPaw/issues/7534)**  
  高优先级卡片消息处理后单 session consumer 不再拉取下一条消息，后续普通消息无法新建 consumer。影响 DM 会话可用性，需排查队列消费生命周期。

- **[Issue #7476 — cron 任务在 misfire_grace 窗口内重复调度（开放）](https://github.com/agentscope-ai/CoPaw/issues/7476)**  
  每日 06:30 备份任务在 17–48 秒间隔内执行两次，产生重复备份文件。

### 🟡 中低 / 兼容性与体验

- **[Issue #7474 — 自定义 Provider 加载失败（已关闭）](https://github.com/agentscope-ai/CoPaw/issues/7474)**  
  由 PR #7337 的 `ModelInfo.max_tokens` 迁移引发，属升级回归。

- **[Issue #7531 — OpenCode API 将于 09/06 强制要求 `x-opencode-session` header（开放）](https://github.com/agentscope-ai/CoPaw/issues/7531)**  
  ⚠️ 时间敏感：若未适配，09/06 起请求可能报错。

- **[Issue #7510 — Reme /memory/status 在 2.2.0-beta.7 Desktop 返回 500（开放）](https://github.com/agentscope-ai/CoPaw/issues/7510)**

- **[Issue #7505 — 局域网 LLM 频繁 client disconnect（开放）](https://github.com/agentscope-ai/CoPaw/issues/7505)**

- **[Issue #7529 — 启用 Langfuse 后工具输出为空/空白（开放）](https://github.com/agentscope-ai/CoPaw/issues/7529)**  
  Langfuse trace 中 Tool Observation 的 `output` 字段为空，影响可观测性。

- **[Issue #7516 — 企业微信无法发送 base64 data URL 图片（开放）](https://github.com/agentscope-ai/CoPaw/issues/7516)**

- **[Issue #7545 — 桌面端聊天框右键无「复制」选项（已关闭）](https://github.com/agentscope-ai/CoPaw/issues/7545)**  
  桌面端与 Web 行为不一致，属小型体验回归。

## 6. 功能请求与路线图信号

- **团队/多租户版本向（最强路线图信号）**：  
  [Issue #7318](https://github.com/agentscope-ai/CoPaw/issues/7318) 与 [Issue #2324](https://github.com/agentscope-ai/CoPaw/issues/2324) 显示社区需要 multi-user access 与 admin-managed skills，2.2.0 Hub 多租户版预计将沿此方向落地。

- **移动/远程访问**：  
  [Issue #7519 — 手机远程连接桌面端](https://github.com/agentscope-ai/CoPaw/issues/7519) 与 [Issue #7518 — 远程 WebUI 首屏加载慢（已关闭）](https://github.com/agentscope-ai/CoPaw/issues/7518) 形成组合诉求：用户希望出门在外用手机继续操作桌面端 Agent。配合今日合入的 #5394/#5363/#5334，移动端投入显然在加大。

- **更新体验**：  
  [Issue #7543 — 在线更新改为后台更新](https://github.com/agentscope-ai/CoPaw/issues/7543)：当前前台更新导致更新期间应用不可用，用户明确希望「后台下载、完成后再提醒安装」。

- **交互范式扩展**：  
  [Issue #7533 — 消息按钮支持](https://github.com/agentscope-ai/CoPaw/issues/7533)、[Issue #7527 — 原生 context compaction 保留 agent persona](https://github.com/agentscope-ai/CoPaw/issues/7527)、[Issue #7540 — 允许关闭硬编码 identity line](https://github.com/agentscope-ai/CoPaw/issues/7540)。

- **渠道兼容**：  
  [Issue #7535 — Matrix 频道适配 Element 恢复密钥与 MSC2965 登录](https://github.com/agentscope-ai/CoPaw/issues/7535)。

- **长期待回应需求**：  
  [Issue #1775 — codex 风格 steer mode（3 月创建）](https://github.com/agentscope-ai/CoPaw/issues/1775) 与 [Issue #4036 — 添加模型步骤过多（5 月创建，good first issue）](https://github.com/agentscope-ai/CoPaw/issues/4036) 均持续被用户提起，建议排期或标记为社区任务。

## 7. 用户反馈摘要

- **本地/局域网部署是真实高频场景，但连接稳定性不足**：  
  [Issue #7505](https://github.com/agentscope-ai/CoPaw/issues/7505) 用户在 LM Studio + Qwen3.8 环境下反复遇到 client disconnect，最终超时失败；这类问题对本地模型用户影响直接。

- **安全研究者正在对 QwenPaw2 做系统性对抗测试**：  
  同一用户提交了 [沙箱突破 #7511](https://github.com/agentscope-ai/CoPaw/issues/7511) 和 [危险指令逃逸 #7443](https://github.com/agentscope-ai/CoPaw/issues/7443)，并同步发布分析文章。外部安全审计强度在上升，建议项目方加强安全响应可见性。

- **桌面端/Web 一致性仍是一项基础体验诉求**：  
  [Issue #7545](https://github.com/agentscope-ai/CoPaw/issues/7545) 与 [Issue #7512](https://github.com/agentscope-ai/CoPaw/issues/7512) 都涉及桌面端控制台操作体验不一致，好在均已关闭。

- **团队级用户开始表达多用户诉求**：  
  [#7318](https://github.com/agentscope-ai/CoPaw/issues/7318) 的评论气氛说明很多用户不再满足于单机个人助手，而是希望以团队身份共享会话、技能与模型配置。

- **国际社区参与度上升**：  
  [Issue #7541](https://github.com/agentscope-ai/CoPaw/issues/7541) 为俄语提交，开始从架构层面质疑「按渠道隔离会话」的设计；说明社区讨论已超出单纯 bug 报告，进入设计争论阶段，是项目走向成熟期的信号。

## 8. 待处理积压

- **[Issue #4036 — Adding a model requires too many steps and clicks](https://github.com/agentscope-ai/CoPaw/issues/4036)**  
  创建于 5 月 4 日，至今仍开放。虽然标记为 good first issue，但配置路径过长直接影响新用户上手，建议产品侧重新设计模型添加流程。

- **[Issue #1775 — 类似 codex 的消息附加/steer mode](https://github.com/agentscope-ai/CoPaw/issues/1775)**  
  创建于 3 月 18 日，已积压近半年。社区仍有讨论热度，是 Agent 交互范式的重要增强，值得纳入 roadmap 评估。

- **[PR #6399 — feat: add reranker UI config panel to ReMeLightMemoryCard](https://github.com/agentscope-ai/CoPaw/pull/6399)**  
  Under Review，7 月 23 日创建，已停留约 43 天。若 reranker 后端功能已就绪，建议尽快推进 review。

- **[PR #7401 — fix(acp): prevent Windows ACP agent stalls during workspace bootstrap](https://github.com/agentscope-ai/CoPaw/pull/7401)**  
  Under Review，Windows 下 Agent 启动可被 plugin bootstrap 卡住数分钟，修复 PR 已等待约 6 天。

- **已就绪等待维护者 review 的 PR**：  
  [PR #7504 — MCP per-tool whitelist enforcement on agent runtime](https://github.com/agentscope-ai/CoPaw/pull/7504) 与 [PR #7539 — move managed Chromium install off startup critical path](https://github.com/agentscope-ai/CoPaw/pull/7539) 均标记为 `ready-for-human-review`，分别对应 MCP 安全与桌面端启动提速，建议优先处理。

---

**整体评估**：CoPaw 今日贡献者活跃度与合入效率均处于健康水平，安全治理和移动端体验都有实质推进。主要风险集中在三处：一是安全漏洞（#7443、#7511）的透明跟进；二是 ReMe 记忆静默失败与飞书会话卡死等稳定性问题；三是 21 条待合并 PR 中仍有多条关键修复等待 review。若保持当前合入节奏，2.2.x 的稳定性与体验将有望在下一迭代中显著改善。

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目动态日报 — 2026-09-04

> 数据窗口：2026-09-03 至 2026-09-04（信息来源：GitHub Issues/PR 元数据）

---

## 1. 今日速览

ZeroClaw 在 2026-09-04 维持高活跃度：过去 24 小时共更新 50 条 Issues（36 条活跃 / 14 条关闭）和 50 条 PR（49 条待合并 / 1 条合并或关闭），无新版本发布。安全与权限治理是今日主线——两项 P1 级新缺陷（#10609 ZeroCode 工作目录错误、#10603 OpenCode 会话头缺失）被报告，同时 RFC #7155 的 shell V1 权限策略以大型 PR #10610 的形式正式进入实现阶段。值得关注的是，团队在关闭历史缺陷（如 #9654 操作员拒绝语义丢失、#9811 假健康状态）上效率较高（14 条关闭），但待合并 PR 积压达 49 条（主要集中于 ACP 会话持久化、cron 模块抽取、插件 egress 等大型重构），合并吞吐成为当前流程瓶颈。项目整体健康度良好：社区讨论集中于 RFC 评审与架构协调，缺陷生命周期管理有序（严重度标注、no-stale、accepted 等标签体系运转有效），但发布节奏的停滞和中型以上 PR 的长时间滞留值得关注。

---

## 2. 版本发布

**无新 Release。** 上一次发布窗口已停滞一段时间，考虑到 49 条待合并 PR 中包含多项已完成评审的功能（ACP 持久化分页、cron 抽取、权限策略），建议维护者评估近期是否安排一次合并与发布窗口。

---

## 3. 项目进展

过去 24 小时合并/关闭的 PR 数量极少（1 条），但已完成的关键闭合与新增实现信号如下：

### 3.1 合并/关闭的 PR

| PR | 标题 | 状态 | 意义 |
|---|---|---|---|
| [#10539](https://github.com/zeroclaw-labs/zeroclaw/issues/10539) | fix(runtime): stop advertising self-approval in tool schemas | 已关闭 | 修复安全/UX 问题：`shell`、`schedule`、`cron_add`、`cron_update`、`cron_run` 等工具的 schema 中不再暴露 `approved` 参数（由 `call_prep` 在审批门禁后覆写）。该 PR 直接影响 #9654（操作员拒绝语义丢失）所述代码路径，使审批状态不再可被模型通过参数自声明。 |

### 3.2 新进入实现阶段的关键工作

- **PR [#10610](https://github.com/zeroclaw-labs/zeroclaw/pull/10610)（今日新增，NiuBlibing）**：以 5 个单关注点提交实现已接受的 RFC #7155 Phase 0+1（统一工具权限策略与分级审批），覆盖 shell 权限策略运行时、审批门禁等。这是当前项目中最具路线图意义的实现 PR。
- **PR [#10565](https://github.com/zeroclaw-labs/zeroclaw/pull/10565)（tidux）**：修复 ZeroCode 本地 Code 会话未固定到进程 cwd 的问题——直指今日新报告缺陷 #10609。

### 3.3 关联闭合的缺陷

以下缺陷在过去 24 小时关闭，可视为项目清理积压的积极信号：

- [#9654](https://github.com/zeroclaw-labs/zeroclaw/issues/9654)（操作员拒绝被模型编造原因——P1，已关闭）
- [#9811](https://github.com/zeroclaw-labs/zeroclaw/issues/9811)（/health 误报未连接通道健康——P1，已关闭）
- [#9231](https://github.com/zeroclaw-labs/zeroclaw/issues/9231)（Docker 运行时嵌套在第二个 Docker 沙箱内——P1，已关闭）
- [#9387](https://github.com/zeroclaw-labs/zeroclaw/issues/9387)（Telegram/Slack/Lark/Matrix 互动审批可被任意成员应答——P1，已关闭）
- [#9983](https://github.com/zeroclaw-labs/zeroclaw/issues/9983)、[#10238](https://github.com/zeroclaw-labs/zeroclaw/issues/10238)、[#9905](https://github.com/zeroclaw-labs/zeroclaw/issues/9905)、[#10486](https://github.com/zeroclaw-labs/zeroclaw/issues/10486) 等亦关闭。

> **项目前进评估**：虽然合并数量有限，但 14 条 Issues 的关闭表明多个 P1/P2 安全与功能性缺陷已完成修复验证。项目整体处于“大 PR 待合入、问题清理加速”的阶段。阻塞合并的因素值得关注（见第 8 节）。

---

## 4. 社区热点

今日讨论最集中的议题反映了社区对**安全策略、架构演进与维护流程透明度**的高关注度。

### 4.1 高评论议题 Top 5

| # | Issue | 评论数 | 主题 | 诉求分析 |
|---|---|---|---|---|
| 1 | [#6996](https://github.com/zeroclaw-labs/zeroclaw/issues/6996) RFC: Granular sandbox policy - filesystem restrictions | 23 | 应用层路径准入（工具/SecurityPolicy）与 OS 沙箱后端（Bubblewrap/Landlock/Seatbelt）策略漂移问题 | 已讨论约 3 个月的重量级 RFC（标记 `needs-maintainer-review`）。社区核心诉求：Agent 风险画像应同时约束应用层与 OS 层，需要一个统一的文件系统策略模型以停止两层各自演进。风险等级 high，仍在等待最终决策。 |
| 2 | [#9328](https://github.com/zeroclaw-labs/zeroclaw/issues/9328) verifiable-intent 评估约束缺少凭据链验证 | 14 | `vi_verify` 的 `evaluate_constraints` 直接信任调用者传入的 fulfillment 对象 | 安全审计发现的深层设计缺陷：约束检查必须在密码学验证后的值上执行。标记 `in-progress`、`accepted`，正在修复中，对应参考实现 VI chain verifier 工作流。 |
| 3 | [#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692) Tracker: Maintainer decision queue for RFCs and design issues | 14 | 维护者决策队列跟踪器：RFC、设计议题、发布政策问题的准入/拒绝/延期 | 社区的流程管理需求：希望所有待维护者裁决的 RFC 有公开可见的排队队列。该 tracker 已被 #10330（Accepted RFC 实现索引）引用，说明决策流程正朝两级体系演进（决策前 #8692 → 决策后 #10330）。 |
| 4 | [#10050](https://github.com/zeroclaw-labs/zeroclaw/issues/10050) RFC: Verbatim channel send over gateway | 13 | 网关现有 47 个 `/api/*` 路径中，没有一条可以不经 Agent turn 将调用者消息原样发送到已配置频道 | 对基础能力的缺失提出 RFC：外部系统需要直接向频道推送消息（如通知机器人场景）而无需触发完整 Agent 循环。安全审查关注点：原样消息发送意味着网关成为频道写入代理，需要 rate limiting 与来源校验。已被接受（accepted）并需要 follow-up。 |
| 5 | [#9975](https://github.com/zeroclaw-labs/zeroclaw/issues/9975) RFC: define Web bundle/daemon compatibility for web_dist_dir | 12 | Web 仪表盘与守护进程的 `web_dist_dir` 部署兼容契约 | 用户遇到前端 bundle 与 daemon 版本不匹配的问题。社区推动“集中能力协商（capability negotiation）”作为兼容性保证机制。 |

### 4.2 值得注意的讨论动态

- **RFC #7155（shell 权限策略）今日出现实现 PR #10610**，代码社区对权限治理的关注从讨论转向落地，预期将影响后续所有工具类 RFC 的实现模板。
- **#8692 与 #10330 的同时活跃**表明社区不希望 RFC 止步于“accepted”，对实现落地保持追踪。

---

## 5. Bug 与稳定性

过去 24 小时报告的缺陷按严重程度排列如下。

### 5.1 S1 — 工作流阻塞

| Issue | 标题 | 状态 | 修复 PR |
|---|---|---|---|
| [#10609](https://github.com/zeroclaw-labs/zeroclaw/issues/10609) | ZeroCode 忽略启动目录，强制以 Agent workspace 作为 cwd | 新开（09-04） | 已有 [PR #10565](https://github.com/zeroclaw-labs/zeroclaw/pull/10565)：将本地 Code 会话固定到进程 cwd，含全新会话与重启会话。 |
| [#10603](https://github.com/zeroclaw-labs/zeroclaw/issues/10603) | OpenCode providers 从不发送 `x-opencode-session` 头 | 新开（09-03） | 暂无。影响面：Go 模型（Gemini code tools）可能因此中断，且有账号被标记风险。作者指出 `opencode` 族默认 URL 配置在 `crates/zeroclaw-providers/src/factory.rs`。 |

### 5.2 S2 — 行为降级

| Issue | 标题 | 状态 | 说明 |
|---|---|---|---|
| [#10068](https://github.com/zeroclaw-labs/zeroclaw/issues/10068) | 交互式 Agent 会话上下文被硬限制在 32,000 tokens，忽略 `max_context_tokens = 131072` | 待复现（`r:needs-repro`） | 用户报告 `zeroclaw agent --agent` 会话固定按 32k 压缩。需要复现者提供配置与运行环境以排查硬编码上限来源。 |
| [#10603](https://github.com/zeroclaw-labs/zeroclaw/issues/10603)（S1，见上） | — | — | — |

### 5.3 已关闭缺陷（稳定性改善）

以下 P1/P2 缺陷在过去 24 小时关闭，确认修复：

- [#9811](https://github.com/zeroclaw-labs/zeroclaw/issues/9811)：`/health` 对从未成功连接的 Telegram 通道误报 `healthy`——修复后健康检查将反映真实连接状态。
- [#9654](https://github.com/zeroclaw-labs/zeroclaw/issues/9654)：CLI 或频道的操作员拒绝（Deny）以三词无语义文本到达模型，导致模型编造失败原因——语义保留问题已修复。
- [#9231](https://github.com/zeroclaw-labs/zeroclaw/issues/9231)：Docker runtime 命令被嵌套在第二个 Docker 沙箱中执行——已解决，S1 级工作流恢复。
- [#10486](https://github.com/zeroclaw-labs/zeroclaw/issues/10486)：Matrix 通道忽略了 `[providers.transcription.*]` 配置与 `transcription_provider`——修复后与 Discord 侧 #9905（也已关闭）保持了一致的通道转录配置模型。
- [#10202](https://github.com/zeroclaw-labs/zeroclaw/issues/10202)：log 依赖（如 whatsapp-rust）的记录因缺少 log↔tracing 桥而全部丢失——已修复。

### 5.4 稳定性观察

- 零崩溃/回归类缺陷报告，多数缺陷集中在通道集成（Matrix、Discord、Telegram）与运行时上下文管理。
- 过去 24 小时新开重点缺陷仅 2 条（#10609、#10603），均为 S1，且其中 1 条已获得修复 PR。

---

## 6. 功能请求与路线图信号

### 6.1 已进入实现（存在对应 PR）

| 功能/RFC | 追踪 | 实现 PR | 状态 |
|---|---|---|---|
| RFC #7155：统一工具权限策略与分级审批（shell V1） | — | [#10610](https://github.com/zeroclaw-labs/zeroclaw/pull/10610)（今日提交） | 合入 master 待审 |
| ZeroCode 本地会话按进程 cwd 固定 | 对应缺陷 [#10609](https://github.com/zeroclaw-labs/zeroclaw/issues/10609) | [#10565](https://github.com/zeroclaw-labs/zeroclaw/pull/10565) | 合入 master 待审 |
| ACP 中断会话进度落盘与恢复 | 缺陷相关（#10197） | [#10197](https://github.com/zeroclaw-labs/zeroclaw/pull/10197) | 合入 master 待审（XL） |
| ACP 持久化会话分页 | — | [#10596](https://github.com/zeroclaw-labs/zeroclaw/pull/10596) | 合入 master 待审（XL） |
| 文件附件 RPC 图片标记基于 provider 可加载契约 | — | [#10582](https://github.com/zeroclaw-labs/zeroclaw/pull/10582) | 合入 master 待审 |
| 网关任意文件上传 + RPC 文档标记 | 依赖 #10544 | [#10583](https://github.com/zeroclaw-labs/zeroclaw/pull/10583) | Stacked，待审 |
| cron 模块抽取为独立 crate | 关联 #10546 | [#10557](https://github.com/zeroclaw-labs/zeroclaw/pull/10557) | 合入 master 待审（XL） |
| session-scoped prompt attachments 执行批次 | 追踪 [#10405](https://github.com/zeroclaw-labs/zeroclaw/issues/10405) | — | 规划中 |

### 6.2 已接受但仍需实现的 RFC（路线图信号）

| RFC/Issue | 内容 | 当前状态 |
|---|---|---|
| [#10050](https://github.com/zeroclaw-labs/zeroclaw/issues/10050) | 网关原样频道消息发送（不经 Agent turn） | accepted，待实现 |
| [#9975](https://github.com/zeroclaw-labs/zeroclaw/issues/9975) | Web bundle/daemon 兼容契约（能力协商） | accepted，待实现 |
| [#10222](https://github.com/zeroclaw-labs/zeroclaw/issues/10222) | 交互式 Agent 单工具回合可选功能 | accepted，待实现 |
| 追踪器 [#10406](https://github.com/zeroclaw-labs/zeroclaw/issues/10406) | Gemini speech-to-speech broker 通道实现批次 | accepted，待实现 |
| 追踪器 [#10405](https://github.com/zeroclaw-labs/zeroclaw/issues/10405) | session-scoped prompt attachments（#9998 执行批次） | accepted，待实现 |

### 6.3 本周新增的用户功能建议（信号较新）

- **相关 PR [#10597](https://github.com/zeroclaw-labs/zeroclaw/pull/10597)**：记录模型上报的上下文用量与预算裁剪事件，提升可观测性——这很可能回应了 #10068（32k 上下文硬上限）等上下文管理问题。
- **PR [#10595](https://github.com/zeroclaw-labs/zeroclaw/pull/10595)**：ZeroCode 对长思考输出（thinking）的换行行做缓存包装，修复长响应时的渲染性能问题。
- **PR [#10584](https://github.com/zeroclaw-labs/zeroclaw/pull/10584)**：ZeroCode Todo 面板的关闭按钮与显隐记忆，降低普通用户的使用成本。

> **路线图信号汇总**：项目下一阶段的发力方向是 (1) 工具与文件的安全契约统一（RFC #7155 → 各工具权限收敛）；(2) ACP/Code 会话的可靠持久化与分页；(3) Web/gateway 能力补全（上传、原样发送、能力协商）；(4) cron 等原生模块的架构抽离，减少 holding crate 体积。

---

## 7. 用户反馈摘要

从今日活跃的 Issues/PRs 中可提炼出以下真实用户声音。

### 7.1 痛点反馈

| 反馈来源 | 用户痛点 | 出现场景 |
|---|---|---|
| [#10609](https://github.com/zeroclaw-labs/zeroclaw/issues/10609)（singlerider） | “我在项目目录里启动了 zerocode，但它却把每个会话都放到 `<install>/agents/<alias>/workspace/` 下”——**本地开发工作流受阻**（S1），用户期望“在哪个目录启动就在哪个目录工作”，与 VS Code/类似工具心智模型一致。 | 本地启动 zerocode → 打开 Chat/Code，所有文件操作发生在 agent workspace |
| [#10603](https://github.com/zeroclaw-labs/zeroclaw/issues/10603)（JordanTheJet） | “从未发送 `x-opencode-session` 头”——使用 OpenCode relay 且对接 Go 系模型的用户**工作流完全不可用**，且存在账号被上游标记的风险。 | OpenCode provider 接入 Google Gemini code tools |
| [#10068](https://github.com/zeroclaw-labs/zeroclaw/issues/10068)（icemann521） | “会话显示 15,538 / 32,000，配置了 131,072 也无效”——交互式 Agent 会话被**硬编码 32k 上限截断**，s2。 | `zeroclaw agent --agent` 交互会话，大上下文任务 |
| [#10567](https://github.com/zeroclaw-labs/zeroclaw/pull/10567)（wromansky） | 记忆召回条目没有日期，“几天前召回的内容和现在的文本无法区分”——**记忆体的时间上下文缺失**，用户在长会话中无法判断信息新旧。 | 多日连续使用 Agent 后，记忆混淆 |
| [#10597](https://github.com/zeroclaw-labs/zeroclaw/pull/10597)（wromansky） | 预算裁剪是黑盒，用户看不见哪些轮次的上下文被压缩了。 | 长会话调试“为什么模型忘了前面内容” |

### 7.2 满意信号与社区气质

- **安全审计质量被社区认可**：#9328、#9387 等深度审计类 issue 的并发出现，显示核心用户来自安全工程背景，且其发现被维护者快速接受（accepted + in-progress）。
- 评论区的 RFC 围绕**reference implementation 对照**展开（如 #9328 对照 VI 参考实现），说明核心用户高度工程化、严谨，这对项目质量是把双刃剑——高质量输入 + 高维护负担。
- 多个 P1 缺陷（#9811、#9654、#9231、#9387）在过去 24 小时内完成关闭，用户可见的 bug→fix→close 周期较短，修复体验良好。

### 7.3 对维护流程的反馈（来自行为而非文字）

- #8692（维护者决策队列）与 #10330（已接受 RFC 索引）的创建不是孤立诉求——多个 RFC 处于 accepted 后长期无实现的状态，社区因此尝试建立公开跟踪机制来推动决策透明化。这暗示了**对维护者带宽的隐性压力信号**。

---

## 8. 待处理积压

### 8.1 长期未完成的重要 RFC/决策

| Issue | 内容 | 创建时间 | 风险 | 积压原因分析 |
|---|---|---|---|---|
| [#6996](https://github.com/zeroclaw-labs/zeroclaw/issues/6996) | RFC: Granular sandbox policy（文件系统限制） | 2026-05-28（距今约 99 天） | high | **最高评论数 RFC（23），标记 needs-maintainer-review + in-progress**。两层策略漂移涉及架构决策，可能需协调多 crate 改动。持续积压将增加后续实现与现有沙箱后端的漂移成本。 |
| [#9899](https://github.com/zeroclaw-labs/zeroclaw/issues/9899) | RUSTSEC-2026-0247（bitmaps）豁免弃用追踪 | 2026-08-10 | high（P1） | 状态 blocked——依赖图经由 `imbl` → Matrix SDK dev-dependencies 引入，需上游处理或替换。长期阻塞会导致新增 Rust 依赖受限。 |
| [#7108](https://github.com/zeroclaw-labs/zeroclaw/issues/7108) | CI Rust 构建缓存改进 | 2026-06-02 | high | 评审/实现停滞近 3 个月——15-20 分钟的 PR CI 时长直接影响每次合并吞吐，与当前 49 条 PR 积压存在关联。 |

### 8.2 跟踪器已识别的积压（维持者侧）

- [#7685](https://github.com/zeroclaw-labs/zeroclaw/issues/7685)：13 个测试分片的测试覆盖清理追踪——1 条评论，代表一项早已接受但推进缓慢的大范围质量债。
- [#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692)：RFC 决策队列——该队列自身的 14 条评论说明有多项 RFC 等待裁决。

### 8.3 PR 队列风险提示

49 条待合并 PR 中，数条为重量级（XL）且互相存在 stack 依赖：

- [#10596](https://github.com/zeroclaw-labs/zeroclaw/pull/10596)（ACP 分页，XL）、[#10557](https://github.com/zeroclaw-labs/zeroclaw/pull/10557)（cron 抽取，XL）、[#9584](https://github.com/zeroclaw-labs/zeroclaw/pull/9584)（egress 授权仪式，XL）、[#10584](https://github.com/zeroclaw-labs/zeroclaw/pull/10584)（ZeroCode Todo 持久化，XL）、[#10197](https://github.com/zeroclaw-labs/zeroclaw/pull/10197)（ACP 中断恢复，XL）与 [#10595](https://github.com/zeroclaw-labs/zeroclaw/pull/10595)（ZeroCode 长输出缓存，XL）。
- Stack 链：`#10590 → #10591`；`#10583 → #10544`；`#10595 → #9317`。若底层 PR 长期未合并，上层变更将持续冲突。

**维护者建议**：优先处理 (1) #6996 的最终裁决；(2) 合并推进已完成的 RFC #7155 实现 [#10610](https://github.com/zeroclaw-labs/zeroclaw/pull/10610)； (3) 对 P1 依赖漏洞 #9899 给出上游替代方案或显式接受风险；(4) 审视 CI（#7108）与合并队列的吞吐瓶颈。

---

*本报告由 ZeroClaw 项目数据分析生成，数据截至 2026-09-04。所有链接指向对应 GitHub Issue/PR 页面。*

</details>

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*