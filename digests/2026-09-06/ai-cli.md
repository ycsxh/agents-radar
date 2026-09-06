# AI CLI 工具社区动态日报 2026-09-06

> 生成时间: 2026-09-06 04:06 UTC | 覆盖工具: 9 个

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

## 横向对比

# AI CLI 工具横向对比分析报告（2026-09-06）

> 本报告基于 2026-09-06 各主流 AI CLI 项目社区日报整理。表中 Issue/PR 数量为日报“显著样本”，并非 GitHub 全量统计。

## 1. 生态全景

当前 AI CLI 工具正从“单点对话式编程助手”向“多端协同、可插拔、可观测的 Agent 运行平台”演进。各项目今日均围绕 Agent/Subagent 生命周期、MCP 可靠性、上下文管理与跨端一致性进行高密度迭代；Windows 桌面端稳定性、远程/移动端协同、以及“工具假成功/假等待”等状态真实性问题已成为跨工具共性痛点。新模型发布（GPT-6 Astra、GLM-5.x、Sonnet-5 等）正在对客户端路由、配额计费和模型可见性形成连锁压力，同时 Hooks、ACP、A2A 等事件/互操作协议开始成为下一阶段竞争焦点。

## 2. 各工具活跃度对比

| 工具 | 显著 Issues（日报样本） | 显著 PR（日报样本） | Release 情况 |
|---|---:|---:|---|
| Claude Code | 10 | 3 | v2.1.263（维护版本） |
| OpenAI Codex | 10 | 10 | 无 |
| Gemini CLI | 10 | 10 | v0.60.0-nightly.20260906 |
| GitHub Copilot CLI | 10 | 0 | 无 |
| Kimi Code CLI | 4 | 1 | 无 |
| OpenCode | 10 | 10 | 无 |
| Pi | 10 | 10 | v0.85.1 |
| Qwen Code | 10 | 10 | 3 个版本（2 nightly + 1 preview） |
| CodeWhale（原 DeepSeek TUI） | 10 | 10 | v0.9.12 |

快速排名：OpenAI Codex、Gemini CLI、Qwen Code、OpenCode、Pi、CodeWhale 均出现“高 Issue 样本 + 多 PR/Release”态势，属于高活跃贡献阶段；Claude Code 以 issue 深度讨论和稳定版本为主，属于成熟期社区；GitHub Copilot CLI 当日无 PR/Release，但 10 个高影响 Issue 集中在升级回归与远程会话，明显处于平台整合期；Kimi Code CLI 动态最少，处于低活跃维护状态。

## 3. 共同关注的功能方向

### 3.1 Agent/Subagent 生命周期与终止语义
- **Gemini CLI**：Subagent 达到 MAX_TURNS 被误报为 `GOAL success`，generalist agent 无限挂起。
- **GitHub Copilot CLI**：`agentStop` 事件在子 agent 回合误触发，导致 `/review` 永不结束。
- **Claude Code**：子代理派发时继承父级 system prompt 和全部工具，与子代理定义不一致。
- **CodeWhale**：Fleet 中被取消/暂停的 Agent 永久占用写权限，形成并行写锁死锁。
- **OpenCode**：WebChat Agent 出现“自己提问、自己回答”的幻觉式执行。

### 3.2 MCP 生态稳定性
- **Claude Code**：HTTP MCP 显示已连接但工具调用报 “No such tool available”。
- **GitHub Copilot CLI**：一次工具调用超时即可导致 `tools/list` 刷新失败，该 server 工具被永久移除。
- **OpenAI Codex**：MCP OAuth 动态客户端注册未携带 scopes，远程 MCP Server 无法登录。
- **Gemini CLI**：MCP prompt 文本被 JSON 编码，破坏引号与换行。
- **Qwen Code**：持久化的 mcp_config 在桌面端重启后不自动加载。

### 3.3 跨端/远程会话一致性与恢复
- **Claude Code**：跨机器同步 `~/.claude/` 配置、Remote Control session 挂错会话。
- **OpenAI Codex**：Remote-SSH 重连后旧 app-server 持有 writer 锁；分页历史丢失。
- **GitHub Copilot CLI**：远程 SSH 复制“成功”但 macOS 剪贴板为空；Mobile 会话 UI 无法渲染。
- **OpenCode**：TUI 与 Desktop 会话列表不同步；升级后历史会话被隐藏。
- **Qwen Code**：后台 shell 输出在 session runtime 回收后静默丢失，导致会话卡死。

### 3.4 状态可观测性与“真实反馈”
- **Gemini CLI**：Shell 命令结束后仍显示 “Waiting input”。
- **GitHub Copilot CLI**：工具调用前正文被误判为 reasoning，折叠成 “Thought for Ns”，用户不可见。
- **CodeWhale**：Windows computer-use 在 PowerShell 实际未执行时上报 `{action_sent: true}`。
- **Claude Code**：Bash 工具静默截断约 8KB 命令并误报为 quoting error。

### 3.5 成本、配额与上下文控制
- **OpenAI Codex**：WebSocket 断线重连持续消耗已购 Credits；空闲时段每周限额从 45% 掉到 0%。
- **OpenCode**：订阅月度用量按模型百分比加总得到 100%，超额用户被错误锁死。
- **Claude Code**：希望可配置 Fable 分类器 fallback 的目标模型与 effort，以控制成本。
- **Pi**：Ollama 本地模型受 output reservation 影响，input budget 坍缩到 1024 tokens。
- **Gemini CLI**：可用工具超过 128/400 个时直接 400，需动态裁剪工具集合。

## 4. 差异化定位分析

| 工具 | 核心定位 | 技术路线特征 |
|---|---|---|
| **Claude Code** | Anthropic 全栈编码 Agent | 版本成熟，社区深度讨论插件/Hook 组合模型；向“Agent 开发平台”演进，Plugin 与 Agent 组合能力最强。 |
| **OpenAI Codex** | OpenAI 模型体系原生客户端 | 与 GPT-6 Astra、订阅配额、WebRTC 语音深度绑定；侧重 TUI + 桌面 + 实时会话基础能力。 |
| **Gemini CLI** | Google Gemini 生态与本地隐私 | 以 nightly 快速推进；持续兼容 Claude Code Hooks，加入 A2A 服务端与 Auto Memory 隐私治理。 |
| **GitHub Copilot CLI** | GitHub 工作流与桌面一体 | 与桌面应用、Remote、Mobile 深度绑定；升级/自动更新与桌面运行时耦合紧密，问题集中在集成层回归。 |
| **Kimi Code CLI** | Moonshot 模型 + IDE 面板 | 社区规模小，当前集中在 VS Code 渲染、Zed ACP 等 IDE 集成修复。 |
| **OpenCode** | 开源多 Provider Agent 网关 | 强调 Desktop/TUI/Web 数据统一、SQLite 并发可靠性、连接自愈；适合自托管与异构模型环境。 |
| **Pi** | 多 Provider 实验性 Agent runtime | 快速接入 GPT-6 Astra、Meta/Muse、Ollama；扩展 API 与系统提示增量更新是差异化点，但 npm 发布管线仍不稳定。 |
| **Qwen Code** | Web Shell/worktree 深度工作流 | 在会话工作流可视化、turn 级导航、外部 ACP Agent 委托方面投入最大；适合长会话、多 worktree、CI 集成场景。 |
| **CodeWhale** | 本地优先 + 多 Agent 并行自动化 | 原 DeepSeek TUI → CodeWhale 品牌迁移中；差异化在 Fleet 并行、computer-use、Windows/本地模型支持。 |

## 5. 社区热度与成熟度

- **高活跃/快速迭代**：OpenAI Codex、Gemini CLI、Qwen Code、OpenCode、Pi、CodeWhale。
  - 共性特征：Release/PR 密度高，新模型与远程/桌面功能同步推进；但同时暴露较多 Windows 与后端稳定性问题。
- **成熟平台期**：Claude Code。
  - 版本稳定在 2.1.x，更多是功能提案（如 #91870 Function Hooks 112 评论）与深层次插件能力讨论；Bug 集中在 Windows 桌面端，修复周期偏长。
- **平台整合期**：GitHub Copilot CLI。
  - 当日无 PR/Release，但 10 个高影响 Issue 全部指向“自动更新回归”“worktree 丢失”“远程会话不可用”，说明产品在快速扩张后正承受集成层复杂度代价。
- **低活跃/维护期**：Kimi Code CLI。
  - 24 小时内仅 4 个 Issue 更新（3 个关闭）、1 个 PR 延续修复；整体社区声音较小。

## 6. 值得关注的趋势信号

1. **模型发布正从“能力竞赛”转为“工程链路压力测试”**  
   GPT-6 Astra 发布后，OpenAI Codex、Copilot CLI、Pi、Qwen 等社区同时出现模型路由错误、客户端模型列表不同步、WebSocket 重连扣费等问题。说明新模型上线不仅是 API 更新，还挑战所有上游客户端的模型矩阵、配额系统和计费状态机。技术决策者应关注工具对“模型版本/能力漂移”的隔离能力。

2. **“状态真实性”正在成为 Agent 工具的信任基线**  
   MAX_TURNS 误报成功、Shell 假等待、PowerShell 假执行、正文被折叠为 reasoning、导出静默丢上下文——多个项目社区都在为“误报状态”付出高信任成本。工具若不能区分 `已请求/执行中/成功/失败/超时/已终止`，将很难承载严肃自动化任务。

3. **MCP 正在从“能连”进入“可信”阶段**  
   “Connected 但工具不可用”“一次超时导致整个 server 工具永久移除”“tools/list 被阻塞”等反馈，说明 MCP 需要 per-server 超时隔离、取消传播、状态回退机制；生态问题已从协议可用性转向分布式可靠性。

4. **多 Agent 互操作协议开始成为生态分水岭**  
   Claude Code 社区推动 Function Hooks，Gemini CLI 做 Claude Code Hooks 迁移兼容，Qwen Code 通过 ACP 将 subagent turn 委托给 Claude Code，Pi 提供 extension context 增量注入。可预见 ACP/Hooks/A2A 类开放协议能力，将成为企业选型与工具链可组合性的关键判断点。

5. **Windows/远程/多端体验已成为用户留存短板，也在成为新机会**  
   Claude Code、Codex、Copilot CLI、OpenCode、Pi、CodeWhale 均有大量 Windows 桌面端或 Remote/移动端 issue。能在这些场景提供稳定“断线重连、会话恢复、输入/锁同步”的工具，将构成明确的差异化优势。

6. **发布与升级机制开始被纳入技术选型评估**  
   Pi 连续两次出现 npm 包未声明依赖、Copilot CLI 自动更新改写自身二进制、Qwen Code release pipeline 重复执行、CodeWhale 18 个 crate 上传后才失败。社区越来越在意“先验证、再发布、可回滚”的工程能力；对工具厂商而言，升级路径的可靠性正成为品牌信任的一部分。

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills 社区热点报告（数据截至 2026-09-06）

## 0. 数据说明
- 样本为仓库按评论数排序的 Top 50 PR 与 Top 15 Issue；本次数据中 PR 评论数字段未返回，排名结果保留原始排序（即条目前后顺序代表评论热度）。
- 样本内所有 PR 状态均为 `OPEN`，未观察到已合并条目。

---

## 1. 热门 Skills / PR 排行（Top 8）

| # | PR | 功能 | 社区讨论焦点 | 状态 |
|---|----|------|-------------|------|
| 1 | [PR #1298](https://github.com/anthropics/skills/pull/1298) | 修复 skill-creator 的 `run_eval.py` 永远报 0% recall：将评估产物装为真实 skill、修复 Windows 流读取/触发检测/并行 workers | 直接对应 [Issue #556](https://github.com/anthropics/skills/issues/556)（10+ 次独立复现）；标题级 bug 使 skill 描述优化循环“在噪音上优化”，评论热度全仓库最高 | OPEN |
| 2 | [PR #514](https://github.com/anthropics/skills/pull/514) | 新增 document-typography：生成文档排版质检（孤儿词换行、标题寡行悬底、编号错位） | 被评论为“Claude 生成的每份文档都受影响、但用户极少主动要求”，通用性极强的基础质量 skill | OPEN |
| 3 | [PR #1615](https://github.com/anthropics/skills/pull/1615) | 新增 scnet-hpc：基于 profile 的 SCNet HPC 集群 SSH/Slurm 操作（分区、内存、module、加速器、作业生成） | 场景垂直但痛点明确，社区关注 HPC 场景下 Claude 的作业调度能力 | OPEN |
| 4 | [PR #538](https://github.com/anthropics/skills/pull/538) | 修复 pdf skill：`REFERENCE.md`/`FORMS.md` 大小写引用不一致，在大小写敏感文件系统上引用破裂 | 反映文档类 skill 的可移植性（macOS/Linux 实机可用性）是社区高频验证点 | OPEN |
| 5 | [PR #486](https://github.com/anthropics/skills/pull/486) | 新增 odt：ODT/ODS 创建、模板填充、ODT→HTML 解析 | 补全办公文档格式拼图（docx/pdf/pptx 之外），触发词覆盖 LibreOffice、ODF、ISO 标准格式 | OPEN |
| 6 | [PR #210](https://github.com/anthropics/skills/pull/210) | 改进 frontend-design：保证每条指令可在单次会话内被执行、可观测可验收 | 呼应“SKILL.md 是给模型的操作手册而非给人看的文档”的社区共识（关联 [Issue #202](https://github.com/anthropics/skills/issues/202)） | OPEN |
| 7 | [PR #541](https://github.com/anthropics/skills/pull/541) | 修复 docx：tracked change 的 `w:id` 与既有书签共享 ID 空间导致文档损坏 | 深入 OOXML 规范的兼容性修复；说明高质量文档 skill 需要处理真实世界的 ID 冲突 | OPEN |
| 8 | [PR #525](https://github.com/anthropics/skills/pull/525) | 新增 pyxel：基于 pyxel-mcp 的复古/像素游戏开发（write → run_and_capture → inspect 迭代循环） | “Skill + MCP + 迭代闭环”的典型组合，社区关注它与 MCP 生态的搭配范式 | OPEN |

---

## 2. 社区需求趋势（来自 Issues）

**安全与信任边界（最强声量）**
- [Issue #492](https://github.com/anthropics/skills/issues/492)：43 条评论，指出社区 skill 在 `anthropic/` 命名空间下分发、冒充官方，形成信任边界攻击面。这是当前生态**第一优先级**的治理诉求。

**企业级分享与分发**
- [Issue #228](https://github.com/anthropics/skills/issues/228)：👍 8，要求组织级 Skill 共享库/直链分享，取代“下载文件→IM 发送→手动上传”的原始流程。

**评估与质量基建**
- [Issue #556](https://github.com/anthropics/skills/issues/556)：👍 7，`run_eval.py` 对全部查询 0% 触发，skill 描述优化失效。
- [Issue #189](https://github.com/anthropics/skills/issues/189)：👍 9，document-skills 与 example-skills 安装重复内容，白白占用上下文窗口。

**上下文经济性**
- [Issue #1487](https://github.com/anthropics/skills/issues/1487)：claude-api skill 单次注入 ~156k tokens，直接耗尽上下文窗口——社区对 skill 的“体积敏感度”开始出现。

**新 Skill 方向提案**
- [Issue #1329](https://github.com/anthropics/skills/issues/1329)：compact-memory，符号化紧凑记忆，降低长运行 agent 的自述开销。
- [Issue #412](https://github.com/anthropics/skills/issues/412)：agent-governance，agent 系统的安全治理模式。
- [Issue #1385](https://github.com/anthropics/skills/issues/1385)：三段式 Reasoning Quality Gate 流水线。
- [Issue #16](https://github.com/anthropics/skills/issues/16)：将 Skills 暴露为 MCPs，统一工具接口；[Issue #29](https://github.com/anthropics/skills/issues/29)：Bedrock 平台可用性。

**趋势小结**：社区需求正从“更多领域 skill”转向**治理与工程化**——安全命名空间、企业内共享、可评估、去重、低上下文开销、Agent 长期运行的自管理能力。

---

## 3. 高潜力待合并 Skills（活跃但未合并的 PR）

> 全部样本均为 OPEN；以下按**近期更新时间 + 需求呼应度**筛选，最可能近期落地。

| PR | Skill / 变更 | 高潜力理由 |
|----|--------------|-----------|
| [PR #1627](https://github.com/anthropics/skills/pull/1627) | buffer-api：Buffer GraphQL 社媒排程 Agent Skill（Claude/Cursor/Codex 等通用） | 更新至 09-05，样本内最活跃；跨 agent 可移植设计符合“agent skill 标准化”趋势 |
| [PR #1298](https://github.com/anthropics/skills/pull/1298) | skill-creator eval 0% recall 修复 | 评论热度第 1；对应 Issue #556 的合并修复，是评估链路的“止血”补丁 |
| [PR #1628](https://github.com/anthropics/skills/pull/1628) | Hivemind：零成本多 agent 编排（Claude 规划/审查 + 免费模型 headless 执行） | 回应“昂贵模型上下文才是稀缺资源”的成本痛点，8 月底仍活跃 |
| [PR #568](https://github.com/anthropics/skills/pull/568) | servicenow：覆盖 ITSM/ITOM/ITAM/SecOps/FSM/SPM/CSDM/IntegrationHub 的宽幅平台 skill | 更新至 08-12，企业平台覆盖面大，一旦合入直接补全大型企业场景 |
| [PR #723](https://github.com/anthropics/skills/pull/723) | testing-patterns：Testing Trophy、单元/React 组件测试、测试哲学 | 测试生成是 agent 编码高频刚需，社区长期呼吁的通用测试 skill |
| [PR #1367](https://github.com/anthropics/skills/pull/1367) | self-audit：机械文件校验 + 四维推理质量门禁 | 与 Issue #1385 提案同源，属于“输出交付前的质量门禁”新品类 |
| [PR #525](https://github.com/anthropics/skills/pull/525) | pyxel：复古像素游戏开发 | 将真实 MCP server（pyxel-mcp）封装为 skill 的完整范例，迭代到 07-15 |

---

## 4. Skills 生态洞察（一句话）

社区现阶段最集中的诉求，不是继续增加领域型 Skill 的数量，而是把 Skills 从“个人脚本”升级为**可信（防冒充/防越权）、可评估（run_eval 不再虚报）、可分发（组织级共享/去重）、低成本（控制注入 token）**的工程化生态——即先修好 Skill 的生产与分发基础设施，再谈内容繁荣。

---

# Claude Code 社区动态日报 — 2026-09-06

## 今日速览

过去 24 小时内，Claude Code 发布了 v2.1.263 维护版本，内容为 Bug 修复与可靠性改进。社区讨论热度最高的功能提案是 [#91870 Function Hooks](https://github.com/anthropics/claude-code/issues/91870)，已达 112 条评论、72 👍；与此同时，Windows 桌面端的稳定性、窗口管理与 OAuth 认证类问题仍持续占据 Issue 榜。PR 侧动态较少，主要集中在 agent 配置与校验工具链的修复。

## 版本发布

- **v2.1.263**：官方更新说明仅包含 “Bug fixes and reliability improvements”，属于小型稳定性迭代，建议在方便时升级。[查看 Release 页面](https://github.com/anthropics/claude-code/releases)

## 社区热点 Issues（节选 10 个）

1. [**#91870 Function Hooks — make plugins 10x more powerful**](https://github.com/anthropics/claude-code/issues/91870)  
   `OPEN` ｜ 112 评论 / 72 👍  
   社区当前最热的功能增强提案。作者提出通过“函数钩子 + 副作用跟踪 + 参数化 `$` 对象”让插件可以深度修改 Claude Code，同时用类似 Express/Koa 的注册顺序 `next` 模型保证安全组合。设计讨论激烈，值得持续关注。

2. [**#53247 Claude Desktop 启动失败：崩溃后遗留 Silo/Job Object**](https://github.com/anthropics/claude-code/issues/53247)  
   `OPEN` ｜ 66 评论  
   Windows 上 Claude Desktop 崩溃后无法再次启动，报 HRESULT 0x80070020，只能通过注销或重启系统恢复。该 Issue 已存在数月，开发者普遍期待官方尽快定位。

3. **窗口置顶问题：从“invalid”到 WS_EX_TOPMOST 定位**  
   [#87895 Windows](https://github.com/anthropics/claude-code/issues/87895) 曾被标记 invalid 但仍获 72 👍；同类型 macOS 报告 [#66516](https://github.com/anthropics/claude-code/issues/66516) 也有 31 条评论。最新 [#92337](https://github.com/anthropics/claude-code/issues/92337) 则进一步定位到主窗口间歇性获得 `WS_EX_TOPMOST`，并与 `LocalSessions.setFocusedSession` 相关，影响 Alt+Tab 正常切换。

4. [**#85111 Bash 工具静默截断约 8KB 命令并误报为 quoting error**](https://github.com/anthropics/claude-code/issues/85111)  
   `OPEN` ｜ 5 评论  
   对重度使用 Bash 工具做自动化操作的开发者影响很大：命令并非引号问题，而是上游被静默截断，错误信息具有误导性，会明显增加排查成本。

5. [**#86875 HTTP 传输 MCP 工具不可达，但 /mcp 显示已连接**](https://github.com/anthropics/claude-code/issues/86875)  
   `OPEN` ｜ 4 评论  
   添加 HTTP MCP Server 后，`/mcp` 显示 connected 并列出工具，但实际调用却报 “No such tool available”，重启后依然存在。这类“表面连接、实际不可用”的问题正在削弱开发者对 MCP 生态的信任。

6. [**#74311 允许配置 Fable 5 分类器 fallback 的目标模型与 effort**](https://github.com/anthropics/claude-code/issues/74311)  
   `OPEN` ｜ 3 评论 / 6 👍  
   当前当 Fable 5 安全分类器拦截请求后，Claude Code 会把会话固定到默认 Opus 模型。社区希望能在 Anthropic API 和第三方 API 上自定义 fallback 模型及 effort，以更好控制成本与行为。

7. [**#90688 Windows/VS Code：睡眠唤醒后 OAuth refresh token 被拒**](https://github.com/anthropics/claude-code/issues/90688)  
   `OPEN` ｜ 1 评论  
   自 2.1.247 起，每次睡眠唤醒后 VS Code 扩展的 OAuth 刷新都会收到 HTTP 400，导致用户每天都要重新 `/login`。对日常使用是明显的“持续性摩擦”问题。

8. [**#91750 Windows MSIX 默认抢占 .docx/.pdf/.csv 文件关联**](https://github.com/anthropics/claude-code/issues/91750)  
   `OPEN` ｜ 3 评论 / 2 👍  
   Claude Desktop MSIX 安装后会自动注册为常用文档格式的默认打开方式，且未提供 opt-out。开发者普遍认为这是对系统默认关联的越权行为。

9. [**#89679 Windows x64“先冻结再消失”崩溃，且无任何日志痕迹**](https://github.com/anthropics/claude-code/issues/89679)  
   `OPEN` ｜ 5 评论  
   用户报告可复现的 “freeze-then-vanish” 崩溃，但 Windows 标准日志、事件查看器等渠道均无相关内容。此类无痕崩溃对问题归因和修复都是很大挑战。

10. [**#72123 Read Out Loud / Play voice 播放中途退化**](https://github.com/anthropics/claude-code/issues/72123)  
    `OPEN` ｜ 8 评论  
   朗读或语音播放中途会出现声音变小、变速、变声直至消失，桌面端语音功能体验受损，已有多个用户确认存在相同症状。

## 重要 PR 进展

过去 24 小时公开 PR 更新共 3 条，均为开放状态，集中于 agent 配置与技能仓库质量修复：

1. [**PR #87077 fix(pr-review-toolkit): repair invalid YAML frontmatter in all agents**](https://github.com/anthropics/claude-code/pull/87077)  
   修复 agent 描述中使用未加引号的多行标量文本，导致 YAML 被解析为嵌套 mapping、agent 加载后 frontmatter 为空的问题。

2. [**PR #87079 fix(security-guidance): make \*\* glob patterns match zero-depth paths**](https://github.com/anthropics/claude-code/pull/87079)  
   修复安全规则中 `**/*.ts` 无法匹配顶层文件的问题。原实现对 `**` 的语义与文档不一致，可能造成安全规则静默漏检。

3. [**PR #89404 validate-agent.sh: don't abort at the first warning**](https://github.com/anthropics/claude-code/pull/89404)  
   修复 plugin-dev 的 `validate-agent.sh` 在 `set -euo pipefail` 下因 `((x++))` 返回码导致首次 warning 即退出的问题，并减少了误报。关联 issue [#83803](https://github.com/anthropics/claude-code/issues/83803)。

## 功能需求趋势

从近期 Issue 中可以提炼出几个较明确的方向：

- **插件/Agent 能力深化与一致性**：除 Function Hooks 外，[#92427](https://github.com/anthropics/claude-code/issues/92427) 也在要求 CLI 与桌面端解析一致的插件身份；PR 侧同样在强化 agent YAML/glob/校验可靠性。
- **模型行为更可控**：社区希望针对安全分类器 fallback 的模型与 effort 可配置（[#74311](https://github.com/anthropics/claude-code/issues/74311)），也希望模型能直接修复自己发现的问题而非转交给其他模型（[#92428](https://github.com/anthropics/claude-code/issues/92428)）。
- **多端状态同步与一致性**：例如跨机器同步 `~/.claude/` 配置（[#66303](https://github.com/anthropics/claude-code/issues/66303)）、Remote Control 新建 session 错误挂到最近会话（[#91991](https://github.com/anthropics/claude-code/issues/91991)）、定时任务状态误标记（[#92429](https://github.com/anthropics/claude-code/issues/92429)）等，说明开发者希望 CLI、桌面、Web 和远程控制之间保持统一状态。
- **桌面端对系统层集成的克制**：文件关联不应自动抢占（[#91750](https://github.com/anthropics/claude-code/issues/91750)）、窗口不应随意改变 Z-order（[#92337](https://github.com/anthropics/claude-code/issues/92337)）、Updater 不应误判 MSIX 来源并阻塞启动（[#92432](https://github.com/anthropics/claude-code/issues/92432)）。

## 开发者关注点

- **Windows 桌面端是当前最集中的“痛点区”**：启动失败（[#53247](https://github.com/anthropics/claude-code/issues/53247)）、无痕崩溃（[#89679](https://github.com/anthropics/claude-code/issues/89679)）、窗口置顶（[#92337](https://github.com/anthropics/claude-code/issues/92337)）、OAuth 每日失效（[#90688](https://github.com/anthropics/claude-code/issues/90688)）等多个问题已经交叉出现，且修复周期长。
- **CLI/Agent 工具自描述不一致**：Bash 工具截断后误报 quoting error（[#85111](https://github.com/anthropics/claude-code/issues/85111)）、ListAgents 推荐调用不存在的 SendMessage（[#92134](https://github.com/anthropics/claude-code/issues/92134)）、Auto mode 在 Windows 上仍提示使用 Bash（[#92407](https://github.com/anthropics/claude-code/issues/92407)），都指向工具面描述与实际运行时行为存在偏差。
- **MCP 可观测性不足**：[#86875](https://github.com/anthropics/claude-code/issues/86875) 显示“已连接但工具不可用”，[#91898](https://github.com/anthropics/claude-code/issues/91898) 则批评超时信息误导并提到应用日志缺失。开发者需要更真实的状态上报与诊断日志。
- **Agent/子代理行为与定义不一致**：[#92426](https://github.com/anthropics/claude-code/issues/92426) 指出派发到子代理时会继承派发方的 system prompt 和全部工具，相当于忽略子代理定义，这对多 Agent 工作流影响很大。

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex 社区动态日报 — 2026-09-06

## 1. 今日速览

过去 24 小时官方无新版本发布，但社区讨论与合并活动依旧密集。GPT-6 Astra 上线后的客户端可见性、WebSocket 重连扣费问题成为新焦点；Windows 桌面端 Bug 占比明显提升。PR 侧则以语音/实时通信基础设施、Windows 原生构建支持，以及 TUI 会话管理增强为主，大量自动化 PR 已完成合入。

## 2. 社区热点 Issues（Top 10）

### 1. [#35746] CLI 分页历史会丢失有效 rollout 记录并复用序号
版本 `0.146.0-alpha.10.1` 中发现，分页读取历史时 `RolloutLine` 解码不一致，导致已有会话状态无法正确还原，且同类问题在 `rust-v0.146.0-alpha.14` 中仍未修复。这是本周评论数最高的 Issue（39 条），直接关系到长会话可靠性与数据完整性。  
https://github.com/openai/codex/issues/35746

### 2. [#42853] Windows 桌面端 GPT-6 Astra 未出现在模型选择器中
使用 ChatGPT Pro 账号，但 Windows App（捆绑 CLI 0.153.1）中无法选择 GPT-6 Astra。此类“新模型已发布但客户端目录未同步”的问题会直接影响新功能落地体验，目前已积累 10 条评论。  
https://github.com/openai/codex/issues/42853

### 3. [#16579] Windows 默认会话 Shell 可通过 config 配置
目前 Windows 端默认使用 PowerShell，对 Git Bash 等用户不友好。Issue 已获得 45 个 👍，是目前社区呼声最高的功能请求之一，且作者已准备初步实现补丁，希望官方支持通过配置项指定默认 Shell。  
https://github.com/openai/codex/issues/16579

### 4. [#43045] GPT-6 Astra WebSocket 重连循环持续消耗已购 Credits
在 CLI 0.153.4 中，WebSocket 断线后反复重连，期间持续消耗用户已购买 Credits，直到回退到 HTTPS 才停止。这属于直接影响用户成本的计费级 Bug，需要优先排查重连状态机与用量上报逻辑。  
https://github.com/openai/codex/issues/43045

### 5. [#20503] MCP OAuth 动态客户端注册未携带 scopes
`codex mcp login --scopes ...` 发出的注册请求缺少 scope 字段，导致 Fastmail 等要求 scope 的远程 MCP Server 无法登录。该问题影响 MCP 生态互操作性，已获得 11 个 👍。  
https://github.com/openai/codex/issues/20503

### 6. [#42765] 未运行任何会话，每周限额从 45% 掉到 0%
Pro 用户在完全空闲时段内，剩余额度从 45% 直接归零。此类配额计数异常会严重损害用户信任，社区关注度高，怀疑是服务端限额计算或会话级联重置逻辑出错。  
https://github.com/openai/codex/issues/42765

### 7. [#41922] 上下文压缩后聊天不可用，内部校验错误频出
Windows App 在长对话触发 Context Compaction 后，后续消息无法正常发送，并输出内部 validation 错误。上下文压缩本是长会话核心能力，压缩后状态不可恢复属于高优先级 Bug。  
https://github.com/openai/codex/issues/41922

### 8. [#41849] Remote-SSH 重连后残留 app-server 占用 writer 锁
VS Code Remote-SSH 断线重连后，旧 app-server 仍持有线程写入锁，新会话被 “This is open in another app” 阻塞。该问题直接影响远程开发工作流，属于多会话锁管理缺陷。  
https://github.com/openai/codex/issues/41849

### 9. [#42501] Windows App 因 cua_node 无法复制 node_repl.exe 启动失败
更新到 `26.901.1978.0` 后，App 启动进程可见但主窗口无法出现，根源是 cua_node 运行时暂存阶段拷贝失败。作为启动级阻断 Bug，影响面较大。  
https://github.com/openai/codex/issues/42501

### 10. [#43131] Astra Light 在合法 Bug 分类任务中反复触发 cyber_policy
同一授权任务在 01:35 UTC 前已连续记录 5 次 `cyber_policy` 失败，说明安全策略可能对特定模型存在过度拦截或误判，影响自动化 Agent 完成授权安全类工作。  
https://github.com/openai/codex/issues/43131

## 3. 重要 PR 进展（Top 10）

### 1. [#43147] 会话启动时按模型能力 Gate 实验性上下文
修复了实验性上下文仅检查 Provider 和账号资格、不检查模型支持的问题；同时修正了子会话从父会话继承 Token Budget 激活状态的行为。  
https://github.com/openai/codex/pull/43147

### 2. [#43120] TUI 会话命令支持受管 Worktree 创建
新增 `/worktree` 命令，并让 `/new` 与 `/fork` 可选择在当前 checkout 或新建 worktree 中开启会话，适合需要并行分支任务的开发者。  
https://github.com/openai/codex/pull/43120

### 3. [#43113] 通过 App Server 持久化 Subagent 与 Memory 选项
TUI 中启用 Subagent / Memory 的选项现在会写入服务端配置，而不再只影响当前线程，避免新会话丢失用户偏好。  
https://github.com/openai/codex/pull/43113

### 4. [#43110] 在 Conversation History 中记录 Reasoning Effort 变更
新增默认关闭的 `reasoning_effort_override` 特性，在会话尚无 effort 配置时追加受信任的 `configuration_update` 记录，便于追踪模型推理强度切换。  
https://github.com/openai/codex/pull/43110

### 5. [#43104] Guardian 线程上下文迁移至 `guardianv2` 配置
将 `features.guardian_thread_context` 调整为 `features.guardianv2.thread_context`，统一同步与异步 Guardian 的线程级上下文管理。  
https://github.com/openai/codex/pull/43104

### 6. [#43097] 新增 Helper 支持的实时 WebRTC Session API
引入 `RealtimeWebrtcSession` 及可克隆句柄，负责启动、Answer 协商、音频控制、电平表与错误上报，为实时语音会话提供统一 API 层。  
https://github.com/openai/codex/pull/43097

### 7. [#43090] voice-host 发送处理后的麦克风音频
此前 voice-host 虽采集音频但未发送给对端；本次打通采集 → 处理后音频 → RTP 发送的主链路，并保留静音边界与陈旧音频限制。  
https://github.com/openai/codex/pull/43090

### 8. [#43079] voice helper 增加可选本地音频设备支持
扩展 Helper 协议，新增 `openDevices` 与 `setAudioControls`，在运行时初始化和传输协商完成后打开默认麦克风与扬声器，默认处于静音/抑制状态。  
https://github.com/openai/codex/pull/43079

### 9. [#43100] 为 voice-host 增加有界 Incoming Opus RTP 处理
对上游 Opus RTP 强制加入 64 packets / 2 MiB 上限及单包 64 KiB 限制，避免无界队列增长，提升实时音频链路的稳定性与安全性。  
https://github.com/openai/codex/pull/43100

### 10. [#43144] 增加 Windows MSVC Bazel 构建目标
为原生语音库新增 x64 与 ARM64 的 MSVC Bazel 目标，覆盖构建、运行准备与链接步骤，是 Windows 端语音能力构建基础设施的关键一步。  
https://github.com/openai/codex/pull/43144

## 4. 功能需求趋势

从近期 Issue 和 PR 中可以提炼出以下社区重点关注方向：

- **新模型支持与客户端一致性**：GPT-6 Astra 已开始推送，但用户报告了 Windows 模型选择器缺失、WebSocket 连接扣费等问题。社区期望新模型在 CLI / Desktop / Mobile 各端同步可见且行为一致。
- **会话历史与长任务可靠性**：分页历史记录丢失、context compaction 后会话不可用等问题频发，表明长会话、断线恢复和历史一致性已成为核心关注点。
- **配额与速率限制模型优化**：用户不满足于 5 小时滚动上限，提出“持续消耗每周配额”的可选模式；同时对“无操作却掉额度”的计数错误非常敏感。
- **TUI 与本地开发工作流增强**：包括可配置 Windows 默认 Shell、TUI 中显示持续运行的 Subagent 状态、受管 Worktree 支持等，均指向更强大的本地终端体验。
- **Remote / 移动端协同控制**：Remote-SSH、桌面端与移动端 Remote 控制的稳定性仍是高频诉求，尤其关注分布式锁清理、连接断线恢复与跨端任务同步。

## 5. 开发者关注点

- **会话恢复仍是脆弱环节**：多个 Issue 指向同一类问题——分页、续跑、压缩后回放旧状态或复用 rollout 序号。开发者希望 Codex 在断线、重连或压缩后，能严格基于持久化 JSONL 重建正确视图。
- **Windows 平台质量问题突出**：启动失败、cua_node 暂存异常、模型缺失、插件选择器卡死、graphite.exe 崩溃等大量集中在 Windows App，开发者明显感觉 Windows 端修复优先级落后于 macOS/Linux。
- **配额与计费透明度不足**：无操作期间额度下降、WebSocket 循环扣费、5 小时上限打断工作流等问题，让用户难以預測和控制成本，需要更清晰的用量日志与配额状态解释。
- **远程开发流程存在锁与残留进程隐患**：Remote-SSH / Remote Control 场景下，旧连接未释放的 app-server 或 writer 会阻塞新建会话，开发者需要手动清理，期望服务端能自动检测并回收陈旧会话。

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI 社区动态日报 — 2026-09-06

> 数据来源：github.com/google-gemini/gemini-cli  
> 覆盖范围：过去 24 小时更新的 Releases、Issues、Pull Requests

## 1. 今日速览

- 发布 `v0.60.0-nightly.20260906.g85aca163f`，延续每日夜间版节奏。
- Issue 侧热度集中在 **Subagent/Agent 可靠性** 与 **可观测性**：MAX_TURNS 被误报为成功、generalist agent 挂起、Shell 执行完成后卡在 “Waiting input” 等 P1 问题仍在流动。
- PR 侧围绕 **Claude Code hooks 迁移正确性**、**MCP 文本编码**、**Git 仓库内认证崩溃** 以及 **显式指定模型被自动改写** 等方向推进修复。

## 2. 版本发布

- **v0.60.0-nightly.20260906.g85aca163f**  
  常规 nightly 版本发布，尚未披露面向用户的功能增量。  
  [查看完整 Changelog](https://github.com/google-gemini/gemini-cli/compare/v0.60.0-nightly.20260905.g85aca163f...v0.60.0-nightly.20260906.g85aca163f)

## 3. 社区热点 Issues

以下按关注度挑选 10 个值得关注的 Issue：

### 1. Subagent 达到 MAX_TURNS 后被误报为 GOAL success
[Issue #22323](https://github.com/google-gemini/gemini-cli/issues/22323)

`codebase_investigator` 等 Subagent 已经撞上最大轮次上限，但最终状态仍显示为 `success` / `GOAL`。这会让用户误以为分析已完成，属于**高影响的可观测性误导**。13 条评论、2 👍，且停留在 `need-retesting` 状态。

### 2. Generalist agent 无限挂起
[Issue #21409](https://github.com/google-gemini/gemini-cli/issues/21409)

即使创建文件夹这种简单操作，一旦模型 defer 到 generalist agent 就会卡住，用户最长等待 1 小时无结果。绕过方式是提示模型不要使用 subagent——这说明问题很可能出在 subagent 调度或生命周期上。8 条评论、8 👍，社区呼声很高。

### 3. Shell 命令完成后仍卡在 “Waiting input”
[Issue #25166](https://github.com/google-gemini/gemini-cli/issues/25166)

极其简单的 CLI 命令执行完后，Gemini CLI 仍显示 shell 活跃并等待用户输入。属于高频交互路径上的 P1 体验缺陷，会让自动化流程看起来像“假死”。

### 4. Gemini 不会主动使用自定义 skills 和 subagents
[Issue #21968](https://github.com/google-gemini/gemini-cli/issues/21968)

用户配置了 `gradle`、`git` 等 skills，但模型只有被明确要求时才会使用，相关任务中不会自主调用。这直接影响 Agents/Skills 生态的实际价值。

### 5. `~/.gemini/agents/` 下 symlink 不被识别为 agent
[Issue #20079](https://github.com/google-gemini/gemini-cli/issues/20079)

用户希望用 symlink 管理 `filename.md` 形式的自定义 agent，但当前发现逻辑会直接跳过 symlink。对于用 dotfiles 管理配置的开发者来说非常关键。

### 6. Browser subagent 在 Wayland 下失败
[Issue #21983](https://github.com/google-gemini/gemini-cli/issues/21983)

浏览器类 Subagent 在 Wayland 会话中启动失败，影响 Linux 桌面用户。当前标记为 P1 / `need-retesting`，说明复现或修复验证尚未完全收敛。

### 7. Auto Memory 在内容进入模型上下文前无法确定性脱敏
[Issue #26525](https://github.com/google-gemini/gemini-cli/issues/26525)

Auto Memory 会读取本地 transcripts 并发送给后台 extraction 模型；提示词要求脱敏，但实际上内容已经先进入模型上下文，服务还可能记录已存在的 skills 内容。这属于**隐私与安全设计缺陷**，社区关注度高。

### 8. 可用工具超过 128 个时触发 400 错误
[Issue #24246](https://github.com/google-gemini/gemini-cli/issues/24246)

当启用的工具数量过大（标题提到 128，正文观察到超过 400）时，请求直接 400。开发者期望客户端能更智能地裁剪工具范围，而不是简单把全部工具塞进上下文。

### 9. 模型经常在随机目录创建临时脚本
[Issue #23571](https://github.com/google-gemini/gemini-cli/issues/23571)

限制 shell 执行后，模型倾向在多个目录散落 `edit` 脚本，导致 commit 前清理成本高。核心诉求是**限制模型只能在工作区内可预期位置写入临时文件**。

### 10. Agent 应避免或劝阻破坏性命令
[Issue #22672](https://github.com/google-gemini/gemini-cli/issues/22672)

复杂 Git 操作、分支管理、数据库资源维护中，模型偶尔会使用 `git reset`、`--force` 等破坏性命令。社区希望 Gemini CLI 在策略层更强地阻止这类操作，并主动推荐安全替代方案。

## 4. 重要 PR 进展

### 1. MCP prompt 文本不再 JSON 编码
[PR #29205](https://github.com/google-gemini/gemini-cli/pull/29205)

`McpPromptLoader` 此前会 JSON 编码 MCP prompt 响应，破坏引号和换行。此 PR 改为原样提交文本，并增加回归测试。

### 2. Claude Code hooks 迁移把 timeout 单位从秒改成毫秒
[PR #29125](https://github.com/google-gemini/gemini-cli/pull/29125)

Claude Code 的 hook timeout 单位是秒，Gemini CLI 的 hook runner 按毫秒读取。迁移时直接拷贝数值导致 60 会被当成 60ms。修复 `#29122`，对使用 `gemini hooks migrate` 的用户非常重要。

### 3. Git 仓库内认证崩溃修复
[PR #29163](https://github.com/google-gemini/gemini-cli/pull/29163)

在 macOS Seatbelt 或受限权限环境下，Gemini CLI 启动时读取 Git 分支名可能触发 crash。此 PR 阻止在 Git 仓库内启动认证流程时崩溃。

### 4. 修正 SubagentStop 事件 key
[PR #29124](https://github.com/google-gemini/gemini-cli/pull/29124)

Claude Code 事件拼写是 `SubagentStop`，而迁移映射写成 `SubAgentStop`，导致 hook 被静默丢弃。修复 `#29123`，提升跨工具迁移忠实度。

### 5. A2A Server：先挂载 express.json 再挂 A2A SDK 路由
[PR #29126](https://github.com/google-gemini/gemini-cli/pull/29126)

此前 `express.json()` 在 `setupRoutes` 之后挂载，导致 `POST /` 收到 `req.body = undefined`，JSON-RPC 解析失败。修复 `#29073`。

### 6. Checkpoint 恢复时遇到非数组 history 不再崩溃
[PR #29195](https://github.com/google-gemini/gemini-cli/pull/29195)

`loadCheckpoint` 之前只处理 unparseable JSON；若 JSON 合法但 `history` 不是数组，`/resume` 会直接抛 `TypeError`。现在会优雅降级为空 checkpoint。

### 7. 避免在 state updater 内部继续调度 state 更新
[PR #29211](https://github.com/google-gemini/gemini-cli/pull/29211)

`useInputHistoryStore.addInput()` 在 setState updater 内部嵌套调用其他 setState，导致 React 状态链路异常。PR 改为清晰拆分更新时机，修复可能的输入历史 UI 竞态。

### 8. 不自动改写显式指定的 `gemini-2.5-flash`
[PR #29217](https://github.com/google-gemini/gemini-cli/pull/29217)

当某后端已经启用 3.5 Flash GA 时，`isFlashModel()` 会把显式指定的 `gemini-2.5-flash` 也改写成 3.5 Flash，导致没有 3.5 权限的环境请求失败。

### 9. 另一个防止改写 pinned flash 模型的修复
[PR #29222](https://github.com/google-gemini/gemini-cli/pull/29222)

与 #29217 类似，也是解决 `--model gemini-2.5-flash` 被静默改写的问题。两个 PR 思路接近，后续可能合并或二选一。

### 10. Nightly 版本发布自动化
[PR #29223](https://github.com/google-gemini/gemini-cli/pull/29223)

自动版本 bump 至 `0.60.0-nightly.20260906.g85aca163f`，对应今日发布流程。

## 5. 功能需求趋势

- **Subagent 可靠性、终止语义与超时恢复**：社区大量 P1/P2 Issue 集中在 Subagent 误报成功、无限挂起、被忽略配置等。后续会重点强化 Subagent 的终止原因透出、全局超时与恢复策略。
- **Auto Memory 与隐私安全**：多个 Issue 指向 Auto Memory 的脱敏时机、无效 patch 处理、低信号 session 无限重试。用户越来越在意本地 transcript 是否在模型上下文出现之前就被安全过滤。
- **Browser Agent 的健壮性**：Wayland 失败、settings.json 覆盖被忽略、持久化 profile 锁死等问题，说明浏览器自动化仍是薄弱环节。
- **Agent “会自主使用工具”**：Skills/Subagents 不被自动触发是一个明显产品缺口；symlink 支持、任务轨迹共享、self-awareness 是配套诉求。
- **AST-aware 与上下文效率**：有 EPIC 在调研 AST 感知的文件读取、搜索与 codebase mapping，目标是减少大文件读入带来的 token 洪泛。
- **工具数量膨胀与动态裁剪**：当可用工具超过 128/400 时出现请求失败，社区希望客户端能按当前任务动态收敛工具集。
- **终端 UX 与渲染稳定性**：resize flicker、scrollback 被清空、命令结束后错误等待输入等，是命令行用户最直接的体验痛。

## 6. 开发者关注点

- **“假成功”和“假等待”最伤信任**：MAX_TURNS 被报为 GOAL success、Shell 结束后仍显示 “Waiting input”，这类不可信状态反馈必须优先修正。
- **Agent 自主性矛盾突出**：一面是 Subagent/Skill 不会被自动使用，另一面是 generalist agent 一旦被调用就卡死。开发者希望模型更聪明地判断何时该用子代理，并且用完能稳定交还控制权。
- **Git 与 shell 操作安全边界需要更强约束**：开发者不希望看到 `git reset`、`--force`，也不希望模型在任意目录生成脚本；需要更严格的 sandbox 与写入路径约束。
- **浏览器自动化是 Linux 重灾区**：Wayland、浏览器 profile 锁、配置覆盖失效，已经形成一组连续反馈。
- **隐私期待前置而非事后**：Auto Memory 的“先送进模型再让模型脱敏”模式不被接受，社区期望本地过滤先于外发。
- **Claude Code 迁移不能“形似而神不似”**：timeout 单位错误、事件名大小写错误等细节会让用户从其他 CLI 迁移时踩坑，需要更系统的兼容层测试。

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI 社区动态日报 — 2026-09-06

## 今日速览

- 今日无新版本发布，也无 PR 更新；社区动态集中在 Issue 区。
- 最需要警惕的是两个升级/自动更新相关回归：#4734 让所有 worktree 项目会话不可用，#4728 显示自动更新可能改写当前 `copilot.exe`，进而破坏 GitHub Copilot 桌面应用。
- 此外，MCP 工具刷新、插件 agentStop 事件、以及“正文被折叠为 reasoning”等稳定性与显示问题也在持续发酵。

## 版本发布

过去 24 小时无新版本发布。

## 社区热点 Issues

### 1. 升级到 desktop 2.98.0 / runtime 1.1.15 后，全部项目会话报 “Worktree missing”
- **Issue**: [#4734](https://github.com/github/copilot-cli/issues/4734)
- **为什么重要**：升级后所有 worktree-backed 项目会话——无论新建还是已有——都会提示 “Worktree missing”，需要用户从会话菜单手动重建 worktree。影响面大，属于升级直接引入的回归。
- **社区反应**：Issue 刚进入 triage，暂无评论与 👍，但严重度较高。

### 2. 自动更新改写了运行时 `copilot.exe`，导致桌面应用会话全部失效
- **Issue**: [#4728](https://github.com/github/copilot-cli/issues/4728)
- **为什么重要**：在终端运行 `copilot` 时，自动更新会直接改写它自身被启动的 `copilot.exe`，进而破坏 GitHub Copilot 桌面应用内置的 CLI。之后所有现有会话都会报 “Session unavailable”，且应用内没有任何提示指向 CLI 问题。
- **社区反应**：0 评论，仍是新 Issue，但属于高危安装/更新路径缺陷。

### 3. 希望允许用户在消息执行前取消/移除已入队消息
- **Issue**: [#1857](https://github.com/github/copilot-cli/issues/1857)
- **为什么重要**：当 agent 忙或执行 `/compact` 时，用户通过 `Ctrl+Q` / `Ctrl+Enter` 入队的消息无法取消。一旦排入队列就只能等待按顺序执行，缺少“反悔”机制。
- **社区反应**：目前有 **11 条评论、28 👍**，是今日样本中长期高关注、用户诉求明确的功能请求。

### 4. Linux 上频繁出现 JavaScript heap out of memory
- **Issue**: [#4725](https://github.com/github/copilot-cli/issues/4725)
- **为什么重要**：CLI 每几分钟就会因 Mark-Compact 阶段堆内存不足而崩溃，内存涨到约 3.9GB 后触发 allocation failure。对需要长时间运行的 Linux 用户来说，这个问题会直接导致会话中断。
- **社区反应**：1 条评论，暂无 👍；目前仍标记为 triage，需要更多复现信息。

### 5. Windows 25H2 最新构建不支持 `--sandbox`
- **Issue**: [#4652](https://github.com/github/copilot-cli/issues/4652)
- **为什么重要**：使用 `copilot --experimental --sandbox` 时提示 “Sandboxing is enabled but is not supported on this host”，导致沙箱相关 shell 命令与服务无法执行。Windows 25H2 用户会整体失去沙箱能力。
- **社区反应**：2 条评论，暂无 👍；问题已更新但仍开放。

### 6. 远程会话 UI 无法在 GitHub Mobile Android 端渲染
- **Issue**: [#3498](https://github.com/github/copilot-cli/issues/3498)
- **为什么重要**：远程 CLI 会话数据已能通过 WebSocket 到达 Android 端，推送通知也能工作，但 UI 只显示静态状态文本，无法展示真实会话内容。这会直接影响移动端“随手继续任务”的体验。
- **社区反应**：1 条评论、3 👍；Issue 从 5 月开放至今，说明移动端远程会话仍未达到可用状态。

### 7. 远程 SSH 会话提示 “Copied”，但 macOS 剪贴板为空
- **Issue**: [#4551](https://github.com/github/copilot-cli/issues/4551)
- **为什么重要**：在 macOS Terminal 中通过 SSH 操作远程 Linux 机器时，CLI 会报告复制成功，但实际内容没有进入 macOS 本地剪贴板。对依赖复制代码/命令的用户属于高频痛点。
- **社区反应**：1 条评论、1 👍，属于跨平台远程剪贴板缺陷。

### 8. `agentStop` 在子 agent 回合触发，导致 `/review` 永不结束
- **Issue**: [#3894](https://github.com/github/copilot-cli/issues/3894)
- **为什么重要**：用户注册了自定义 `agentStop` hook，用于自动改进 instructions 文件。但该事件在子 agent 回合也会触发，造成 `/review` 流程被错误打断且始终无法返回。这是插件/Agent 生命周期事件边界问题。
- **社区反应**：1 条评论、1 👍；最近更新于 9 月 6 日，仍在开放。

### 9. 被取消的 MCP 工具调用会连带拖垮后续 `tools/list` 刷新
- **Issue**: [#4731](https://github.com/github/copilot-cli/issues/4731)
- **为什么重要**：当一个 stdio MCP server 的工具调用触发客户端超时后，运行时会立刻向“同一个刚被放弃的 server”发送 `tools/list` 刷新请求。该 server 没空响应，刷新超时后会导致该 server 的所有工具在进程生命周期内被移除。一次取消/超时就能造成永久性工具丢失。
- **社区反应**：0 评论，新 Issue；属于 MCP 集成中较严重的状态一致性 bug。

### 10. 工具调用前的正式文本被误判为 reasoning，折叠成 “Thought for Ns”
- **Issue**: [#4735](https://github.com/github/copilot-cli/issues/4735)
- **为什么重要**：当模型在同一轮中输出较长 reasoning 块、多段用户可见文本，再调用工具时，CLI 会把用户可见正文错误折叠到 “Thought for Ns” 区域，用户最终看不到这些内容。对追求可观测性和透明度的 Agent 工作流影响很大。
- **社区反应**：0 评论，仍处于 triage。

## 重要 PR 进展

过去 24 小时无 PR 创建或更新，因此本次没有 PR 进展可列。

## 功能需求趋势

从 Issue 整体来看，社区关注的功能方向主要有以下几点：

- **队列与输入控制**：除了取消入队消息（[#1857](https://github.com/github/copilot-cli/issues/1857)），还有用户希望 `Ctrl+E` 能先接受内联补全而不是直接跳到行尾（[#4736](https://github.com/github/copilot-cli/issues/4736)）。说明终端用户正在把 Copilot CLI 当作“可交互的 agent terminal”来使用，要求它遵循常见终端编辑习惯。
- **自动更新可靠性**：多个 Issue 指向自动更新/桌面应用集成问题，尤其是更新后 worktree 丢失（[#4734](https://github.com/github/copilot-cli/issues/4734)）和更新改写自身二进制（[#4728](https://github.com/github/copilot-cli/issues/4728)）。社区需要更安全的自更新机制。
- **模型与上下文成本控制**：[#4724](https://github.com/github/copilot-cli/issues/4724) 提出按 prompt cache TTL 做空闲自动 compact，而不仅依赖 token 阈值。这与长期会话的延迟和成本直接相关。
- **远程会话一致性**：GitHub Mobile 渲染、SSH 剪贴板、worktree 恢复等问题都集中反映“远程/跨端会话仍不稳定”。
- **MCP 生态稳定性**：`tools/list` 被超时工具调用阻塞（[#4731](https://github.com/github/copilot-cli/issues/4731)）、research agent 调用不存在的 `github/get_me` 工具（[#4729](https://github.com/github/copilot-cli/issues/4729)）、Canvas `open_canvas` 参数被 JSON-RPC 序列化破坏（[#4721](https://github.com/github/copilot-cli/issues/4721)）表明 MCP 需要更强的超时隔离、取消处理和参数校验。

## 开发者关注点

- **升级/自动更新已成为稳定性风险源**：多个严重问题都出现在桌面应用自动更新后，且没有自动恢复或降级路径。开发者希望自动更新能避免覆盖正在使用的 CLI 二进制，并在迁移失败时保留可恢复的 session/worktree。
- **Agent 输出可见性需要重新审视**：用户可见正文被折叠为 reasoning（[#4735](https://github.com/github/copilot-cli/issues/4735)）会直接降低对 agent 的信任感，这已成为新的 UI/UX 缺陷类别。
- **MCP 超时与取消语义是高频痛点**：一次取消/超时不该让后续 `tools/list` 永久失败；工具注册也不该出现 agent prompt 与 MCP server 实际能力不匹配的问题。
- **平台支持回退明显**：Windows 25H2 沙箱不可用、Linux 端 OOM 崩溃，说明平台兼容性和资源占用仍需持续投入。
- **插件与 Agent 生命周期事件需要更清晰**：`agentStop` 不应在子 agent 回合触发，自定义 agent 下的 `--interactive` 启动 prompt 也不应被静默丢弃。社区对可编程插件生态的期待正在上升。

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI 社区动态日报（2026-09-06）

> 数据快照：github.com/MoonshotAI/kimi-cli  
> 覆盖窗口：过去 24 小时

## 今日速览

- 过去 24 小时无新版本 Release。
- 4 个 Issue 更新中，3 个为历史遗留问题（Windows Zed 集成、Linux 认证失败、Shell 提示符回归）并已关闭；仅 1 个新开放 Issue，指向 VS Code 扩展的渲染层丢字问题。
- PR 方面仅 #2513 今日有活跃更新，继续推进 Moonshot API 工具调用参数的双重编码解码修复。

## 社区热点 Issues

当前窗口内仅有 4 个 Issue 更新，数量较少，以下全量列出。

### #2635 [OPEN] VS Code extension: streamed chat text drops individual characters at render/copy layer

- 链接：[MoonshotAI/kimi-cli Issue #2635](https://github.com/MoonshotAI/kimi-cli/issues/2635)
- 作者：@TserenTserenov | 创建：2026-09-05 | 更新：2026-09-05 | 评论：0 | 👍：0
- 状态：OPEN
- 重要性：这是近期新报告的 VS Code 扩展前端缺陷。会话日志显示模型输出完整，问题发生在渲染或复制面板这一层，直接影响开发者在 GUI 中读取和复用生成内容。作者已用 wire log 验证模型侧没有丢数据，定位范围清晰，修复成本应可控，但也说明扩展渲染链路缺少足够的字符级完整性校验。

### #1350 [CLOSED] 频繁出现 Authorization failed, please check your login status

- 链接：[MoonshotAI/kimi-cli Issue #1350](https://github.com/MoonshotAI/kimi-cli/issues/1350)
- 作者：@dapeng1162 | 创建：2026-03-05 | 更新：2026-09-06 | 评论：0 | 👍：0
- 状态：CLOSED
- 重要性：该问题影响 Debian 12 下使用 `/login` 与 `kimi-for-coding` 模型的场景，反复出现登录态失效提示，说明 CLI 的本地凭证刷新或鉴权状态判断存在兼容性隐患。今日被关闭，但 Issue 中没有补充评论，建议关注后续版本是否有对应修复，并在 Linux 下做回归验证。

### #1349 [CLOSED] shell prompt no longer shows cwd/git branch; request configurable display

- 链接：[MoonshotAI/kimi-cli Issue #1349](https://github.com/MoonshotAI/kimi-cli/issues/1349)
- 作者：@Sirfetch-d | 创建：2026-03-05 | 更新：2026-09-06 | 评论：0 | 👍：0
- 状态：CLOSED
- 重要性：交互式编程场景中，用户需要快速确认当前目录和 git 分支，而新版 prompt 只显示 `✨ / 💫 / $` 这类装饰符号，属于明显的交互回归。用户诉求是让 prompt 组件可配置。今日关闭，但未见相关配置项说明，若官方决定移除这类上下文信息，建议在文档中明确给出替代方案。

### #1284 [CLOSED] Does not launch in Zed IDE ACP panel in Windows

- 链接：[MoonshotAI/kimi-cli Issue #1284](https://github.com/MoonshotAI/kimi-cli/issues/1284)
- 作者：@prashanth057 | 创建：2026-02-27 | 更新：2026-09-06 | 评论：1 | 👍：0
- 状态：CLOSED
- 重要性：Windows 用户无法在 Zed IDE 的 ACP 面板中启动 kimi-cli v1.14.0。Zed 的 ACP（Agent Client Protocol）是第三方 IDE 接入 CLI 的重要路径，这个兼容性问题关系到 IDE 集成生态的完善度。Issue 有一条评论，今日关闭，推测有 workaround 或已在后续版本修复。

## 重要 PR 进展

当前窗口内仅有 1 个 PR 更新。

### #2513 [OPEN] fix(kosong): recursively decode double-encoded tool-call arguments

- 链接：[MoonshotAI/kimi-cli PR #2513](https://github.com/MoonshotAI/kimi-cli/pull/2513)
- 作者：@nitishagar | 创建：2026-07-19 | 更新：2026-09-06 | 评论：暂无数据 | 👍：0
- 状态：OPEN
- 修复内容：Moonshot API 返回的 `function.arguments` 中，嵌套的数组/对象值可能以 JSON 字符串形式二次编码。此前代码只做一次 `json.loads`，导致 `todos` 这类字段仍然为字符串，最终触发 Pydantic 校验错误 `Input should be a valid list`。该 PR 新增共享 `decode_tool_arguments` 工具，对参数做递归解码。
- 价值评估：这是一个典型的真实环境兼容性修复。如果 Moonshot API 并不保证内层字段的序列化形态，那么所有依赖工具调用的上层逻辑都可能踩中该问题。下沉为共享解码函数是正确做法，能避免不同模块各自打补丁。

## 功能需求趋势

由于本次采样窗口 Issue 数量有限，趋势基于现有样本归纳，仅供参考：

1. **IDE 集成稳定性**：Zed ACP 启动问题和 VS Code 渲染丢字，说明社区对编辑器接入体验非常敏感，希望 CLI 不只是终端内可用，还要在 Zed、VS Code 等 GUI 环境中保持同等完整度。
2. **Shell 交互上下文可视化**：有用户明确希望 prompt 重新支持 `cwd` / git branch 展示，并且能自定义。这类需求指向“CLI 应默认帮用户感知自己在哪个仓库、哪个目录工作”。
3. **登录态可靠性**：Linux 环境下反复弹 `Authorization failed`，说明 `/login` 之后的凭证维护流程仍有改进空间。

## 开发者关注点

- **认证体验是最基础的信任指标**：一旦出现间歇性 `Authorization failed` 且无明确恢复指引，会直接影响用户对 CLI 的信任感。
- **UI 渲染的“可靠性”优先于“丰富性”**：VS Code 扩展丢字虽然是边缘情况，但用户用 wire log 对比验证，说明社区已经开始用协议级手段追溯前后端问题，官方对这类报告需要更及时的反馈。
- **IDE 支持广度是增长关键**：Windows + Zed 场景失败，叠加 VS Code 面板问题，暴露出多端适配的测试盲区。
- **用户希望回归“信息完整”的默认交互**：prompt 里的目录和 git 分支不是装饰性内容，而是防止开发者误操作的上下文锚点。

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode 社区动态日报 — 2026-09-06

> 数据来源：github.com/anomalyco/opencode | 覆盖范围：过去 24 小时

## 1. 今日速览

今日无新版本发布。开发侧的主线是**网络连接韧性与稳定性修复**：多个 PR 集中解决桌面/TUI 客户端事件流重连、请求超时、目录同步风暴等问题；同时有人提交了对 SQLite 锁竞争（`SQLITE_BUSY`）的重试修复。社区侧最值得关注的是两个新开放的高影响 bug：**OpenCode Go 订阅被错误计费锁死（#47547）** 和**多实例共享数据库写入冲突（#47566）**。此外，近期 1.17.x 升级后“会话历史丢失/隐藏”的反馈仍是高频痛点。

## 2. 版本发布

过去 24 小时无新的 Release。

## 3. 社区热点 Issues

以下挑选 10 个当前关注度最高或影响面最大的 Issue：

### #47547 [OPEN] — OpenCode Go 订阅被“Monthly Usage 100%”错误锁死
- **内容**：订阅报告 5 小时用量 0%、每周用量 0%，月度总用量却按各模型用量百分比加总计算（DeepSeek V4 Flash 47.8% + Pro 34.7% + …）得到了 100%，而非按实际美元（$60 限额）计算。用户未超额却被封禁，且要等约 15 天才重置。
- **重要性**：直接影响付费订阅用户，属于计费算法缺陷；今日新开，应重点关注。
- 链接：https://github.com/anomalyco/opencode/issues/47547

### #47566 [OPEN] — 多 opencode 进程共享数据库触发 SQLITE_BUSY
- **内容**：多个进程共用同一数据目录时，单条写入超过 `busy_timeout`（5s）即报 `database is locked`，错误显示为 “Failed to execute statement”。今天的 PR #47567 已尝试修复。
- **重要性**：影响自动化、并行任务、同机多项目共用 session 存储的高级用法。
- 链接：https://github.com/anomalyco/opencode/issues/47566

### #42627 [OPEN] — Windows 远程 serve 无法在 macOS Desktop 客户端中正常加载项目/会话
- **内容**：macOS Desktop 能连上 Windows 上的 `opencode serve`，底层 API 正常，但 GUI 无法读取/使用远程项目与历史会话。
- **重要性**：跨平台远程开发场景下的可见阻塞，说明两端数据模型或 UI 状态同步仍有差异。
- 链接：https://github.com/anomalyco/opencode/issues/42627

### #32747 [OPEN] — `@` 文件提及不包含启动后新建的文件
- **内容**：opencode 启动后新创建的文件不会进入 `@` 文件选择器，重启后消失的文件才会出现；代码线索指向 TUI 的 `@` 搜索索引状态过期。
- **反应**：15 条评论、13 个 👍，是当前 bug 类反馈中呼声最高的问题之一。
- 链接：https://github.com/anomalyco/opencode/issues/32747

### #35009 [CLOSED] — 升级 1.17.11 → 1.17.13 后资源占用显著升高
- **内容**：会话过程中 RSS 约 1GB、虚拟内存 75GB、CPU 22% 波动；已随版本修复关闭。
- **启示**：说明社区对 1.17.x 的运行时内存与 CPU 开销很敏感。
- 链接：https://github.com/anomalyco/opencode/issues/35009

### #35690 & #35750 [CLOSED] — 升级后旧会话历史消失/被隐藏
- **内容**：多个用户反馈升级 1.17.14 后 Session picker 只显示少量会话，旧数据仍在 `opencode.db` 中但不再展示；后者定位到数据库中新增 `path` 列在迁移时未回填，导致旧会话被过滤。
- **重要性**：升级迁移路径直接破坏用户数据可见性，是续 #29071 后的又一数据完整性事件。
- 链接：https://github.com/anomalyco/opencode/issues/35690 、 https://github.com/anomalyco/opencode/issues/35750

### #35741 [CLOSED] — WebChat 代理模式幻觉：AI 自己提问、自己回答
- **内容**：在 agent/build 模式下，模型有时向用户抛出澄清问题后，不等待用户输入，直接编造“用户选了方案 1”并继续执行。
- **重要性**：对自动化代理可靠性影响极大，容易产生误导性操作。
- 链接：https://github.com/anomalyco/opencode/issues/35741

### #35784 [CLOSED] — GLM-5.2 陷入文件读取死循环
- **内容**：Agent 反复读取文件同一片段，`read file loop [limit=40]`，难以稳定复现，但已在 GLM-5.2 / opencode-go 下出现两次。
- **启示**：大上下文 / 文件读取类循环问题需要更好的步进检测与熔断机制。
- 链接：https://github.com/anomalyco/opencode/issues/35784

### #34030 [CLOSED] — 无法调用 GitHub Copilot Enterprise 中企业自建的第三方模型
- **内容**：企业向 Copilot 平台添加了非 Copilot 自有模型，OpenCode 接入 Copilot 后读不到这些模型。
- **重要性**：企业定制模型 + 第三方 MCP 插件是目前高频诉求，开放性和兼容性仍不足。
- 链接：https://github.com/anomalyco/opencode/issues/34030

### #29071 [CLOSED] — Desktop 创建的会话在 CLI TUI 中不可见
- **内容**：Desktop（Electron）中创建的会话不会出现在 TUI 的 Switch session 列表中；反之 TUI 创建的会话在 Desktop 可见；`opencode export` 可导出全部数据。
- **反应**：4 条评论、2 个 👍，与统一桌面/TUI 存储目录的诉求（#35703）同源。
- 链接：https://github.com/anomalyco/opencode/issues/29071

---

## 4. 重要 PR 进展

挑选 10 个值得关注的新 PR，覆盖客户端稳定性、SQLite 并发、云凭证发现与 UI 修复：

### #47567 [OPEN] — SQLite 语句在锁超时后进行重试
- 修复 #47566：多个 opencode 进程共享同一数据库时，SQLite 拿不到写锁不再直接 `orDie`，而是按重试策略等待/重试，避免整条 prompt 直接失败。
- 链接：https://github.com/anomalyco/opencode/pull/47567

### #47571 [OPEN] — 检测停滞事件流并强制 resync
- 解决移动端/桌面端锁屏恢复后连接假死的问题：页面挂起时 socket 已死但 `reader.read()` 不报错，连接状态仍为 connected。该 PR 引入前台恢复时的停滞检测与重新同步。
- 链接：https://github.com/anomalyco/opencode/pull/47571

### #47572 [OPEN] — 为服务器无响应的请求增加超时机制
- 单个死亡 socket 可让 `fetch` 等数分钟，占满每 server 4 个 in-flight 并发槽，导致后续 API 全被阻塞。该 PR 为请求增加主动超时释放队列。
- 链接：https://github.com/anomalyco/opencode/pull/47572

### #47573 [OPEN] — 连接恢复后刷新 queued inputs
- 修复重连后 `session.pending` 未加载的问题，确保断线期间用户排队的输入不会丢失。
- 链接：https://github.com/anomalyco/opencode/pull/47573

### #47565 [OPEN] — 重连后放缓目录 re-sync 节奏
- 原先重连会对每个 active directory 同时执行排队刷新与立即同步；连接约 20 个目录时会瞬间产生大量请求。该 PR 统一走 pacing 队列。
- 链接：https://github.com/anomalyco/opencode/pull/47565

### #47561 [CLOSED] — 合并 MCP 目录的 refetch 事件风暴
- Desktop 网络日志显示 7 秒内触发 192 次 `GET /api/mcp`：每个 MCP server 的 `mcp.status.changed` 都会触发一次 invalidate + sync。该 PR 将突发事件合并且抖动处理，属于明显的前端性能优化。
- 链接：https://github.com/anomalyco/opencode/pull/47561

### #47560 [CLOSED] — 保留 Desktop 本地服务 CORS 头，启用预检缓存
- 此前每条 Desktop API 请求前都跟着一个 `OPTIONS` 预检：310 秒日志中有 759 个真实请求 + 759 个 OPTIONS。修复后 Chromium 可命中预检缓存，API 往返次数几乎减半。
- 链接：https://github.com/anomalyco/opencode/pull/47560

### #47548 [CLOSED] — 在 Provider 插件中自动发现 Bedrock 凭证
- 配合 #47436 为原生 Bedrock 提供 AWS 默认凭证链支持：EC2 instance role（IMDS）场景下不再需要手动设置 `AWS_ACCESS_KEY_ID` 即可自动加载 amazon-bedrock provider。
- 链接：https://github.com/anomalyco/opencode/pull/47548

### #47555 [OPEN] — 修复 `opencode --continue` 获取占位 session ID
- `--continue` 会以 `sessionID: "dummy"` 做占位路由，导致 session 路由提前向后端请求一个非法 ID（server 400）。该 PR 阻止这次无效 fetch。
- 链接：https://github.com/anomalyco/opencode/pull/47555

### #47564 [OPEN] — 避免慢速 git 读取占满请求队列
- 请求队列只有 4 个槽位，而 `GET /api/vcs` 的 p50 高达 1789ms、max 2468ms；低速 git/worktree 查询会把后续正常 API 全部堵死。该 PR 调整请求优先级或队列调度。
- 链接：https://github.com/anomalyco/opencode/pull/47564

---

## 5. 功能需求趋势

从近 24 小时更新的 50 个 Issue 及 PR 中，可提炼以下社区关注方向：

1. **桌面端 / TUI / Web 三者数据统一**
   代表：#35703（合并 desktop 与 TUI 目录）、#29071（会话列表不同步）、#35690 / #35750（升级后会话不显示）。
   社区期望所有端共享同一份 session、plugin、agent 配置，并保证升级迁移不破坏历史数据。

2. **会话生命周期与后台任务能力**
   代表：#28695（session 生命周期 hooks）、#35728（`background.extend()` 无法向活动会话注入消息）。
   开发者希望插件能监听 session 启停事件，并在 agent 运行时异步注入上下文，实现更复杂的自动化工作流。

3. **大文件 / 大 diff 下的性能与内存保护**
   代表：#31916（TUI 卡在 Preparing to write…）、#32046（Renderer 计算大 diff 崩溃）、#35009（1.17.x RAM/CPU 激增）。
   这已成为 1.17 系最集中的性能投诉方向。

4. **企业级与自定义模型接入**
   代表：#34030（Enterprise Copilot 第三方模型）、#35798（EC2 instance role 的 Bedrock 自动加载）、#35732（自定义 OpenAI-compatible provider 下 GPT-5.5 工具调用路径不清晰）。
   开发者希望除标准云账号外，还能通过 OIDC/IMDS/企业代理等多种方式使用自建或内网模型。

5. **客户端网络连接韧性**
   今天多笔 PR（#47571、#47572、#47573、#47565）都表明：**事件流断线恢复、请求超时与队列防阻塞**正在成为桌面/Web 的稳定性重点。

---

## 6. 开发者关注点

高频反馈 / 痛点汇总：

- **升级迁移容易“吞”历史会话**：1.17.14 前后多个版本出现旧 session 不可见、黑屏、高磁盘占用问题（#35690、#35750、#35717）。用户对“升级是否需要备份、能否回滚”尤为关心。
- **Windows 桌面端资源与渲染问题突出**：大 diff 计算卡死（#32046）、大文件写入挂起（#31916）、1GB RAM/高 CPU（#35009），且部分问题在 Windows 上比 WSL 更明显（#35611）。
- **@ 文件索引状态会过期**：启动后新建文件无法被搜索/提及（#32747），说明索引生命周期需要与文件系统事件绑定，而非仅在启动时建立一次。
- **SQLite 单写者限制影响多实例协同**：任何超过 5s busy_timeout 的写竞争都会直接中断会话（#47566），需要重试、队列或独立 server 模式。
- **模型行为不稳定会连带产生成本/任务异常**：如 GLM-5.2 反复读文件（#35784）、DeepSeek 返回 DSML/XML 风格工具调用（#34676）以及 WebChat Agent 幻觉式自问自答（#35741），都会影响自动化可靠性与费用控制。
- **插件版本缓存策略不直观**：`plugin@latest --force` 仍使用旧缓存，只有指定版本号才会重新下载（#35742），在插件迭代调试时常造成困惑。

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi 社区动态日报 2026-09-06

## 1. 今日速览

Pi 发布 v0.85.1，重点新增 GPT-6 Astra 接入。但 0.85.x 的 npm 打包/依赖声明问题成为社区新焦点：`#9132` 之后又有 `#9218` 报告全局安装子代理运行失败。Windows 相关体验仍是最大讨论热点，同时多组 PR 正在修复扩展运行期稳定性，并推进系统提示增量更新机制。

---

## 2. 版本发布

### v0.85.1
[GitHub Releases](https://github.com/earendil-works/pi/releases)

- 新特性：**GPT-6 Astra** 可通过 OpenAI API Keys 和 OpenAI Codex 订阅使用。
- 更多 Provider 配置参考：[providers.md](https://github.com/earendil-works/pi/blob/v0.85.1/packages/coding-agent/docs/providers.md#api-keys)

注意：v0.85.1 仍被报告存在全局安装后子代理运行失败问题，详见 issue `#9218`。

---

## 3. 社区热点 Issues

以下按关注度/重要度精选 10 条：

### #7547 [Windows] 你在 Windows 上怎么用 Pi？遇到哪些问题？
**状态：OPEN · 52 评论**  
当前评论数最高的 issue。Windows 上有太多运行方式，官方需要社区反馈来聚焦 bug 修复、文档和开箱体验。  
[https://github.com/earendil-works/pi/issues/7547](https://github.com/earendil-works/pi/issues/7547)

### #9132 0.85.0 发布包中 dist/cli.js 静态导入未声明的 @earendil-works/pi-server
**状态：CLOSED · 5 评论 · 5 👍**  
导致 0.85.0 的 npm 包无法正常 import 包根，属于发布质量事故，也直接推动了 `#9170/#9172` 修复。  
[https://github.com/earendil-works/pi/issues/9132](https://github.com/earendil-works/pi/issues/9132)

### #9218 npm install -g pi-coding-agent@0.85.1 后子代理运行失败
**状态：CLOSED · 1 评论**  
报告 0.85.1 全局安装后仍缺少 `@earendil-works/pi-server` 和 `@earendil-works/pi-client` 声明，导致 pi-subagents 子会话无法运行。发布依赖列表仍需补强。  
[https://github.com/earendil-works/pi/issues/9218](https://github.com/earendil-works/pi/issues/9218)

### #5023 [bug] 终端无故滚动到会话开始位置
**状态：CLOSED · 19 评论**  
模型输出过程中终端会随机跳到会话开头并快速滚回底部，影响长会话阅读。属于反馈较多的 TUI 稳定性问题。  
[https://github.com/earendil-works/pi/issues/5023](https://github.com/earendil-works/pi/issues/5023)

### #6300 [bug] Windows 下每次按键输入行都会重绘
**状态：OPEN · 8 评论**  
在 Windows Terminal / cmd.exe 下，每个字符都出现在新行，严重干扰基本输入。Windows TUI 输入渲染仍是明显短板。  
[https://github.com/earendil-works/pi/issues/6300](https://github.com/earendil-works/pi/issues/6300)

### #8896 /export HTML 静默丢弃发给模型的上下文
**状态：OPEN · 8 评论**  
`display: false` 自定义消息本应仅控制 TUI 显示，但 `/export` HTML 导出时把它们一并丢弃，导致导出记录与会话真实上下文不一致。  
[https://github.com/earendil-works/pi/issues/8896](https://github.com/earendil-works/pi/issues/8896)

### #8684 PI_OFFLINE 静默禁用所有 Provider 模型发现
**状态：OPEN · 5 评论**  
文档说 `PI_OFFLINE` 只关闭启动时的网络检查，实际却禁用了整个会话的模型目录/目录发现。该行为需要文档化或修正。  
[https://github.com/earendil-works/pi/issues/8684](https://github.com/earendil-works/pi/issues/8684)

### #9212 Sonnet-5 经网关调用：13% edit 工具参数被截断
**状态：CLOSED · 3 评论**  
`anthropic/claude-sonnet-5` 通过 Vercel AI Gateway 调用时，部分 edit 参数被截断成 `edits: [{}]`，导致 schema 校验失败。模型路由/输出可靠性仍是高风险区。  
[https://github.com/earendil-works/pi/issues/9212](https://github.com/earendil-works/pi/issues/9212)

### #9209 Copilot GPT-6 Astra 被路由到不支持的 Chat Completions 端点
**状态：CLOSED · 3 评论**  
Pi 将 `github-copilot/gpt-6-astra` 路由到 `/chat/completions`，Copilot 明确拒绝该模型访问此端点。新模型接入的同时需要修正路由规则。  
[https://github.com/earendil-works/pi/issues/9209](https://github.com/earendil-works/pi/issues/9209)

### #9216 Ollama qwen3.8:27b 流式 “terminated” 错误 + 自动压缩不再重复触发
**状态：CLOSED · 2 评论**  
0.84.x 到 0.85.x 的回归问题，本地模型在高强度工具调用场景下反复断流，且自动压缩只触发一次，之后不再工作。  
[https://github.com/earendil-works/pi/issues/9216](https://github.com/earendil-works/pi/issues/9216)

---

## 4. 重要 PR 进展

### #9170 fix(coding-agent): 声明 pi-server 运行时依赖
**CLOSED**  
解决 0.85.0 包根无法 import 的问题，是本次发布事故的直接修复。  
[https://github.com/earendil-works/pi/pull/9170](https://github.com/earendil-works/pi/pull/9170)

### #9172 fix(coding-agent): 防止损坏的包根再次发布
**CLOSED**  
在发布流程上增加保护，避免同类“静态导入未声明依赖”的缺陷再次进入 npm 包。  
[https://github.com/earendil-works/pi/pull/9172](https://github.com/earendil-works/pi/pull/9172)

### #9222 fix(coding-agent): 活动会话操作期间拒绝 reload
**OPEN**  
修复 RPC 模式下工具运行中执行 reload 导致 runner 失效、错误结果回传模型的问题。  
[https://github.com/earendil-works/pi/pull/9222](https://github.com/earendil-works/pi/pull/9222)

### #9182 fix(coding-agent): 跳过失效扩展 runner 的会话事件
**CLOSED**  
围绕 `/new`、退出与 reload 竞争的场景，避免二次 teardown 访问已销毁 runner。  
[https://github.com/earendil-works/pi/pull/9182](https://github.com/earendil-works/pi/pull/9182)

### #9219 fix(coding-agent): 保留宿主 UI 原型方法与 Proxy traps
**CLOSED**  
原先 object spread 会丢失宿主 `ExtensionUIContext` 的原型方法，破坏嵌入方提供的 UI context，该 PR 改为更安全包装。  
[https://github.com/earendil-works/pi/pull/9219](https://github.com/earendil-works/pi/pull/9219)

### #9214 Invoke skills and prompt templates mid-sentence
**CLOSED**  
带来技能/模板中句调用能力，用户无需把调用放在输入行开头，即可在消息中间展开技能或 prompt template。  
[https://github.com/earendil-works/pi/pull/9214](https://github.com/earendil-works/pi/pull/9214)

### #9116 feat(ai): 增加会话中途系统消息
**OPEN**  
为会话中动态插入系统消息提供底层能力，属 `#9117` 的上游基础层。  
[https://github.com/earendil-works/pi/pull/9116](https://github.com/earendil-works/pi/pull/9116)

### #9117 feat(coding-agent): 用系统消息增量交付 prompt 与工具变更
**OPEN**  
不再每次重写顶层 system prompt，而是以 system message delta 方式同步工具/提示变化，降低 token 开销与上下文扰动。  
[https://github.com/earendil-works/pi/pull/9117](https://github.com/earendil-works/pi/pull/9117)

### #9096 feat(ai,coding-agent): 增加 Meta Provider 与 Muse 订阅 OAuth
**OPEN**  
新增 Meta Provider，支持 Muse 订阅 OAuth。该 Provider 的 token 刷新机制比较特殊：每天从 identity token 重新铸造 API token。  
[https://github.com/earendil-works/pi/pull/9096](https://github.com/earendil-works/pi/pull/9096)

### #8734 feat(ai): 为 OpenAI Responses 兼容 Provider 支持顶层 instructions
**OPEN**  
增加 `openai-responses` 的 `systemPromptFormat` 选项，可将动态系统提示放入顶层 `instructions`，避免重复写入 `input`。  
[https://github.com/earendil-works/pi/pull/8734](https://github.com/earendil-works/pi/pull/8734)

---

## 5. 功能需求趋势

从全部 issue/PR 看，社区当前最关注的方向包括：

- **新模型与 Provider 接入**：GPT-6 Astra（#9209）、OpenAI 异步工具调用（#9113）、Requesty 原生 Provider（#5473）、Meta/Muse（#9096）、LLM Gateway（#7610）。
- **会话上下文与压缩机制**：OpenAI stateful continuation / server-side compaction（#7317、#6676）、system message delta（#9116/#9117）、压缩逻辑清理（#6451）。
- **技能/Template 易用性**：中句调用（#8457/#9214）、opt-in 包 namespace 统一技能与 prompt template 命名（#8834）。
- **TUI 与终端体验**：Windows 支持收集（#7547）、输入重绘（#6300）、IME 候选框错位（#5200）、菜单快捷键统一（#9199）、全屏滚动指示（#7970）。
- **扩展 API 深化**：向扩展暴露底层 `ModelRuntime`（#8791）、保留宿主 UI context 原型（#9219）、reload 安全性（#9222）。

---

## 6. 开发者关注点

- **npm 发布/安装质量仍是当前最大痛点**：`#9132` 和 `#9218` 连续指向“发布包内静态导入未声明依赖”，导致新装包在 import 或子代理场景直接失败，开发者期待更严格的发布自检。
- **Windows/TUI 输入体验急需改善**：Windows 运行方式征集、输入行重绘、IME 候选框错位、终端随机滚动等反馈高频出现，Windows 用户群体现已形成集中反馈渠道。
- **模型/网关路由与计费细节容易踩坑**：Copilot GPT-6 Astra 错误路由、Sonnet-5 edit 截断、Vercel gateway routing 配置失效、cache TTL 计费偏差等，都说明多网关兼容层仍需补齐。
- **环境变量语义需要文档化**：`PI_OFFLINE` 的实际影响范围超出文档描述，开发者希望环境变量边界更明确。
- **本地模型在 0.85.x 存在回归风险**：Ollama 断流、自动压缩失效等问题，提示本地小模型与长会话场景需要继续回归验证。

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code 社区动态日报（2026-09-06）

## 1. 今日速览

昨日共发布 3 个版本（含 nightlies 与 preview），核心围绕 Web Shell 工作流可视化的增强。Issue 与 PR 更新量保持高位，值得关注的是：多条 release pipeline 在 9 月 5 日出现失败记录，社区对 CI/CD 稳定性的反馈集中且行动迅速（多条 tracking issue 已开启并进入 autofix 流程）；Web 导出功能体积问题引发连续多个高优 issue 讨论，团队已通过“按需加载 / 瘦身 entry”方向推进优化。

## 2. 版本发布

昨日发布 3 个版本，内容高度聚焦于 Web Shell 的 Session Workflow（动态工作流）可视化与管理，具体包括：

### v0.23.0-nightly.20260905.0c945a6136
- **feat(web-shell):** 新增动态工作流运行的可视化与管理能力（PR #10594）
- **perf(web-shell):** 优化会话工作流项目派生逻辑

### v0.23.1-preview.0
- 与 nightly 版本内容基本一致，侧重于 Web Shell 工作流改进的预览集成。

### v0.23.0-nightly.20260905.e3d26283e6
- 功能与性能改进同样基于 PR #10594。

**说明：** 三个版本均于同日发布，但 Release 工作流出现多次失败记录（quality / integration_docker job），对应的自动 tracking issue（#11098、#11114、#11122、#11138）已在处理中，值得关注失败根因是否与新增工作流可视化功能有关。📎 [所有 Releases](https://github.com/QwenLM/qwen-code/releases)

## 3. 社区热点 Issues（10 条）

### 🐛 高优先级 Bug 与性能问题

1. **[#11031 [P1] export HTML 文件体积过大：每个 HTML 都内嵌整个 Web Shell 运行时](https://github.com/QwenLM/qwen-code/issues/11031)** ⭐
   即使空会话导出的 HTML 也高达 19.5 MB，因构建产物将 WebShellTranscript 可达的完整浏览器依赖图全部内嵌。当前 #11038 已通过瘦身 entry 优化，但根治需进一步按需加载。5 条评论，属于 roadmap/export-data 核心事项。

2. **[#11091 [P2] mermaid（~6 MB）仍被扁平化打进导出 transcript 渲染器中](https://github.com/QwenLM/qwen-code/issues/11091)**
   #9812 合并后导出文件已改为外链 unpkg（SHA-384 SRI 锁定），但 mermaid 依然被静态打包进渲染器，导致体积持续膨胀。6 条评论，社区讨论热度最高。

3. **[#11119 [P1] serve: 后台 shell 输出与唤醒通知在 session runtime 回收后静默丢失](https://github.com/QwenLM/qwen-code/issues/11119)**
   在 `qwen serve` 的 Web Shell 会话中，由 `run_shell_command` 启动的轮询类后台任务（如 CI 监听）在启动它的 turn 结束之后，其后续输出与唤醒通知被静默丢弃，进而导致会话卡死。涉及 background-automation roadmap，需要设计层面的讨论（need-discussion）。

4. **[#10989 [P2] daemon prompt 状态仅在 sidebar 挂载处被轮询，导致 VS Code 中 #9487 加载指示器修复失效](https://github.com/QwenLM/qwen-code/issues/10989)**
   长任务中途 sidebar spinner 掉落的修复（#9487）在 VS Code companion 场景中不起作用，因为 `useWorkspaceSessionLiv...` 只在 sidebar 挂载时才拉取 daemon 的 `hasActivePrompt`。说明前端修复需考虑多挂载点/多种 UI 宿主。

5. **[#10865 [P2] session workflow 投影每次渲染被派生 3 次](https://github.com/QwenLM/qwen-code/issues/10865)**
   `SessionWorkflowCockpit.tsx` 等表面每次渲染重复构建同一投影索引，产生无谓 CPU 开销。已标记 ready-for-agent，可直接领取。

### ⚙️ 基础设施与 CI/CD

6. **[#10879 [P1] hk4 被固定为 release host 但仍带共享 label，release 与 PR CI 互相抢占资源](https://github.com/QwenLM/qwen-code/issues/10879)**
   `release.yml` 将 8 个验证 job 固定到 `ecs-qwen-hk4-host` 专属 label，但该 host 同时仍挂有共享 `ecs-qwen` label，导致 release 验证与 PR CI 在物理机 CPU 上互相竞争。状态为 ready-for-human。

7. **[#10892 [P2] vi.waitFor 默认 1s 超时在全仓库 2047 个调用点生效，测试易脆](https://github.com/QwenLM/qwen-code/issues/10892)**
   `vi.waitFor` 硬编码 1000ms 且无全局配置覆盖，在 CI 压力下会导致偶发失败。涉及全仓库测试基建的配置设计改进。

8. **[#11109 [P2] release.yml 重复执行同一次运行已完成的工作，消耗 20 分钟却未验证任何内容](https://github.com/QwenLM/qwen-code/issues/11109)**
   昨日有 2 次 release 运行超时（33957952281、33963757913）。release 流水线中存在明显可优化的重复步骤，社区通过自动 issue 直接指出问题所在。

### 🔌 IDE 与集成体验

9. **[#11141 [P2] 作为 ACP 在 IntelliJ IDEA 26.1.1 中无法展示/回答任何问题](https://github.com/QwenLM/qwen-code/issues/11141)**
   中文用户报告：通过 ACP 集成到 IntelliJ IDEA 26.1.1（Windows x64）后，提问无响应。客户端信息：Qwen Code 0.23.0、Node.js v24.14.1、模型 glm-5.3。3 条评论，IDE 集成方向的高频反馈类型。

10. **[#7771 [Bug] 持久化的 mcp_config 未在桌面端启动时加载到主进程 MCP 代理](https://github.com/QwenLM/qwen-code/issues/7771)**
    用户配置的 MCP server 在重启 Qwen Desktop 后无法自动生效，IPC 调用因主进程侧缺失配置而失败。已存在相应修复 PR #11145，本 issue 处于 need-retesting 状态。5 条评论，持续关注度高。

## 4. 重要 PR 进展（10 条）

### ✨ 功能与架构

1. **[#11086 feat(serve): 将扩展（extension）隔离到 workspace 运行时中](https://github.com/QwenLM/qwen-code/pull/11086)** ⭐
   使全局扩展目录按 workspace 运行时生效，同步更新扩展管理、composer 添加菜单及 `@` 引用。进一步提升多 workspace 隔离性与安全性。

2. **[#11003 feat: 将 subagent turn 委托给外部 ACP agent（率先支持 Claude Code）](https://github.com/QwenLM/qwen-code/pull/11003)** ⭐
   允许 subagent 定义声明 `executor` 块（外部命令），通过 ACP 协议驱动外部编码 agent 完成 turn，并将全过程结果重新发布为当前 session 的事件流。多 agent 协作生态的重要一步。

3. **[#10938 feat(web-shell): 让 Session Workflow 依赖项可导航并收敛其视觉噪音](https://github.com/QwenLM/qwen-code/pull/10938)**
   补齐 #8583 后在导航、形状与文档方面的缺口，并重设计了 plan DAG 与 inspector 区域的视觉层次。

4. **[#11054 / #11053 feat(web-shell): 全局 turn 导航 Phase 2 数据层（headless + 客户端）](https://github.com/QwenLM/qwen-code/pull/11054)**
   - #11054：服务端数据层——引入有界 turn 元数据与历史 transcript 缓存、精确的 live/persisted turn 定位器、prompt 对账与公开 React hooks。
   - #11053：客户端数据层——provider 记录每次 fetch 返回的 transcript 窗口切片，将未加载区间视为显式值域，为后续虚拟滚动导航做铺垫。
   
   两项配合实现 Phase 2 的完整数据链路。

5. **[#11015 feat(channels): 实现 named-session worktree reset（Part 4B）](https://github.com/QwenLM/qwen-code/pull/11015)**
   使 `/clear`、`/new`、`/reset` 可作用于选定的 worktree 隔离任务，保留 daemon 验证过的 worktree 与分支，同时清空会话上下文。

6. **[#9466 refactor: 将 rewind 映射锚定到稳定 prompt identity](https://github.com/QwenLM/qwen-code/pull/9466)**
   解决 session resume（含 headless `-p --rewind`）等场景中因 turn 序号重排导致的 rewind 错位，提升长会话与并发的健壮性。

### 🔧 修复与稳定性

7. **[#11145 fix(serve): 启动时在 ACP preheat 后加载持久化 MCP 配置](https://github.com/QwenLM/qwen-code/pull/11145)**
   针对 issue #7771 的修复：daemon 启动流程中显式调用 `reconcileMcpConfiguration()`，不再等待用户手动 reload。

8. **[#11144 fix(cli): 在 tool-result 写入完成后才允许 live transcript 读取](https://github.com/QwenLM/qwen-code/pull/11144)**
   修复 issue #9704：在 ACP agent 路径中对磁盘上的 transcript 读取设置屏障，避免并发加载时读到半写入的 tool turn；同时将 restore 清理统一收敛到 `finalizeDanglingForRestore` helper。

9. **[#11134 fix(ci): macOS E2E shard 偶发整体死亡时增加受预算控制的单次重试](https://github.com/QwenLM/qwen-code/pull/11134)**
   为 macOS E2E 增加与 Linux sandbox:none leg 相同的重试机制，缓解全绿状态下的 shard 进程突然退出问题。

10. **[#11103 ci: vitest worker RPC 单独失败时不再将 Test job 标红](https://github.com/QwenLM/qwen-code/pull/11103)**
    新增 infra-flake 分类脚本：若仅因 vitest worker IPC 断连导致失败，则输出 warning 而非失败，减少 CI 噪音。

## 5. 功能需求趋势

- **Web Shell / 会话管理仍是绝对主线**：工作流可视化、turn 级导航、sessionRotation、worktree 生命周期、prompt identity 重构等多条 PR/issue 并行推进，说明项目正将重心放在 Web 端深度会话体验的重构上。
- **导出功能瘦身与数据可移植性**：export HTML 体积问题（#11031、#11091、#11100、#11142）连续出现，社区对“分享/归档产物应轻量、独立、不含运行时”的诉求强烈，方向为按需加载、外链资源 + SRI、以及剥离 daemon SDK 依赖。
- **外部代理（ACP）协作生态**：从 ACP delegate（#11003）到扩展系统 workspace 作用域（#11086），再到 IDE 中 ACP 集成问题反馈（#11141），Qwen Code 正强化“作为 agent 中枢指挥外部工具/模型”的定位。
- **CI/CD 可靠性工程**：4 条 release 失败 tracking issue + release.yml 重复步骤 + hk4 host 资源竞争 + vi.waitFor 超时，围绕“回归验证可信度”的基础设施治理密集推进。
- **MCP 配置体验闭环**：#7771 修复 PR（#11145）已提交，说明“配置一次、重启仍在”是用户基础预期。

## 6. 开发者关注点

- **构建与发布稳定性成为社区情绪焦点**：多条 “Release Failed” 自动 issue 在 24 小时内连续生成，且有两条指向同一失败运行（33963757913 触发 #11114、#11122 两次记录），说明 release pipeline 的重试/去重逻辑让开发者感到困惑。建议关注 #11109 中对重复步骤的剖析，并检查 autofix/upsert 中间件的幂等性。
- **性能问题集中在“导出产物体积”与“会话恢复风暴”两点**：
  - 前端导出文件体积（19.5 MB 起步）直接暴露构建依赖图治理不足；
  - serve daemon 在重连时的全量历史回放（#10780）在 10^5–10^6 token 会话上会打爆有界 NDJSON transport，进而波及其他无关会话。此类问题已在 dogfooding 中被真实触发，修复优先级高。
- **测试基建的脆弱性开始产生“狼来了”效应**：vi.waitFor 1s 超时、vitest worker RPC 失败、macOS E2E shard 整体死亡——三类 flaky test 问题同时被讨论，开发者对“CI 标红信息量低”明确表达了不满（甚至专门发 issue 要求分类 infra 与真实失败）。
- **第三方模型接入路径值得关注**：在 #11141 中，用户使用 glm-5.3 模型且 LSP disabled，界面完全无法展示回答。尽管问题可能源于 ACP 通道，但模型名称的非 Qwen 化说明用户正在通过 Qwen Code 连接异构模型网关。兼容性反馈将是后续社区声量的重要组成部分。

---
*日报生成时间：2026-09-06 · 数据范围：2026-09-05 ~ 2026-09-06（GitHub API 快照）*

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI 社区动态日报 — 2026-09-06

> 数据来源：`github.com/Hmbown/DeepSeek-TUI`  
> 注意：项目正处于品牌迁移期，产品已更名为 **CodeWhale（Shannon Labs）**，Issue/PR 链接均指向 `Hmbown/Codewhale`。

---

## 1. 今日速览

- **v0.9.12 正式定调品牌迁移**：发布说明确认产品公开名称为 **Codewhale**，旧 npm 包 `deepseek-tui` 弃用，v0.8.x/旧 `deepseek` 用户需转向 `codewhale` 命令与包。  
- **Windows/computer-use 可靠性问题集中爆发**：多个 issue 指向“PowerShell 执行失败却上报成功”“CRLF 被静默改写”“鼠标按下事件丢失”等问题，并已有对应修复 PR 跟进。  
- **社区对上下文管理与 MCP 状态可视化诉求强烈**：上下文压力警告、Ollama token 预算坍缩、MCP “N connecting” 长时间不消退是讨论最集中的稳定性话题。

---

## 2. 版本发布

### v0.9.12

- **Codewhale** 正式作为 Shannon Labs 公开发布产品命名；`codewhale` 命令、npm 包、release 资产名称统一保持小写技术标识。
- 旧 `deepseek-tui` npm 包弃用，不再接受新发布。
- v0.8.x 时代的 `deepseek`/`d` 用户需要迁移到新命令/包；发布说明在迁移指引处截断，建议直接查看 Release 原文。
- 发布后暴露了一个发布门禁问题：18 个 crate 上传成功后才因 `codewhale-tui` 校验失败，目前已通过 #5893 补充“先完整验证再首次上传”的流程。

> 相关链接：[v0.9.12 Release](https://github.com/Hmbown/Codewhale/releases) | [Issue #5892](https://github.com/Hmbown/Codewhale/issues/5892) | [PR #5893](https://github.com/Hmbown/Codewhale/pull/5893)

---

## 3. 社区热点 Issues（10 个）

### 1. [#5620] 上下文压力警告是瞬时的，Agent 不会主动响应  
- **标签**：bug / context / compaction  
- **重要性**：警告出现一次后消失，Agent 不会主动触发压缩或降级。开发者认为这属于“静默的上下文劣化”，尤其危险。  
- **社区反应**：12 条评论，持续讨论中。  
- 链接：https://github.com/Hmbown/Codewhale/issues/5620

### 2. [#5820] Ollama 本地模型输入预算坍缩到 1024 tokens（已关闭）  
- **标签**：bug / Ollama provider  
- **重要性**：默认 output reservation 64K 会把 32K 窗口模型的实际 input 预算夹到 1024 tokens，对本地模型用户影响极大。  
- **社区反应**：5 条评论，已关闭，但被多个 Ollama 用户复现确认。  
- 链接：https://github.com/Hmbown/Codewhale/issues/5820

### 3. [#5909] `write_file` 覆盖 CRLF 文件时静默转成 LF（今日新增）  
- **标签**：bug / 文件编辑  
- **重要性**：`edit_file` 会保留原有行尾风格，而 `write_file` 不会，导致同一文件在不同工具下表现不一致，Windows 用户容易产生无意义 diff。  
- **社区反应**：提交当天即出现对应修复 PR #5911。  
- 链接：https://github.com/Hmbown/Codewhale/issues/5909

### 4. [#5908] Windows computer-use 在 PowerShell 失败时仍上报成功（今日新增）  
- **标签**：bug / computer-use / win32  
- **重要性**：后端 `{action_sent: true}` 是假的——PowerShell 实际没执行；同时 `left_mouse_down` 会丢失按下事件。自动化可靠性归零。  
- **社区反应**：已由 #5903 修复并合入，作者 EvanProgramming 继续推动测试重构 PR #5912。  
- 链接：https://github.com/Hmbown/Codewhale/issues/5908

### 5. [#5906] Fleet 中已取消/暂停的 Agent 永久占用写权限（今日新增）  
- **标签**：bug / Fleet / write claims  
- **重要性**：子 Agent 被取消后仍保持 write claim，导致同一路径后续写任务全部被拒。这是并行 Agent 机制的隐含死锁问题。  
- **社区反应**：暂无评论，可能处于早期复现阶段，但影响面较大。  
- 链接：https://github.com/Hmbown/Codewhale/issues/5906

### 6. [#5904] Web fetch：JS 渲染的 200 页面无法抽取内容（今日新增）  
- **标签**：bug / web fetch  
- **重要性**：同一 URL 是否失败取决于缓存状态，Agent 会误判为“网站不可抓取”，且无重试或浏览器升级策略。  
- **社区反应**：暂无评论，是 Explore/scout Agent 的间接稳定性隐患。  
- 链接：https://github.com/Hmbown/Codewhale/issues/5904

### 7. [#5887] MCP 启动时长时间卡在 “20 connecting”  
- **标签**：bug / MCP / UX  
- **重要性**：使用方无法判断是“仍在连接”“某个 server 卡死”还是“计数已过期”。创始人 dogfood 时反馈的问题。  
- **社区反应**：已有 PR #5897 改为逐 server 更新连接状态。  
- 链接：https://github.com/Hmbown/Codewhale/issues/5887

### 8. [#5888] Fleet 菜单选项过多，需要精简  
- **标签**：UX / enhancement  
- **重要性**：创始人认为“way too many things”，当前菜单让用户难以识别下一步动作。  
- **社区反应**：已由 PR #5905 将 `/fleet` 使用行从 14 个动词压缩到 5 个。  
- 链接：https://github.com/Hmbown/Codewhale/issues/5888

### 9. [#5901] `/theme` 选择器没有列出自定义主题  
- **标签**：enhancement / TUI  
- **重要性**：`~/.codewhale/themes/<name>.json` 可通过 `custom:<name>` 使用，但无法在选择器中发现，自定义主题用户找不到入口。  
- **社区反应**：已由 PR #5907 实现。  
- 链接：https://github.com/Hmbown/Codewhale/issues/5901

### 10. [#2323] 中文输入法适配不完整  
- **标签**：bug / 中文输入 / IME  
- **重要性**：中文输入时拼音候选区无法隐藏、在配置/危险操作窗口中会串到斜杠命令输入区；中文社区长期痛点，且有一个 👍 支持。  
- **社区反应**：3 条评论，更新于 2026-09-05，仍有中文用户关注。  
- 链接：https://github.com/Hmbown/Codewhale/issues/2323

---

## 4. 重要 PR 进展（10 个）

### 1. [#5897] fix(mcp)：每个 MCP server 连接后即时更新启动进度  
- 原先会缓存所有连接结果，直到最慢批次结束才更新 UI，导致 “20 connecting” 假象。  
- 改为连接完毕一个、更新一个，避免工具池等待。  
- 链接：https://github.com/Hmbown/Codewhale/pull/5897

### 2. [#5911] fix(tools)：`write_file` 保留已有文件的 CRLF 行尾风格  
- 修复 #5909，让覆盖写入与 `edit_file` 行为一致。  
- 影响 `WriteFileTool::execute` 等两条写入路径。  
- 链接：https://github.com/Hmbown/Codewhale/pull/5911

### 3. [#5903] fix(computer-use)：win32 后端如实上报 PowerShell 失败  
- 修复 Windows 后端误报 `{action_sent: true}` 的问题。  
- 关闭 #5896；已合入主分支。后续 #5910/#5912 将在此基础上补充可注入 runner 与宿主无关测试。  
- 链接：https://github.com/Hmbown/Codewhale/pull/5903

### 4. [#5905] feat(tui)：精简 Fleet 菜单主界面  
- 关闭 #5888。  
- `/fleet` 即时使用的动词从 14 个降到 5 个：`members | setup | teams | workers | help`，其余收进 `/fleet help` 的分组文档。  
- 链接：https://github.com/Hmbown/Codewhale/pull/5905

### 5. [#5907] feat(tui)：`/theme` 选择器中列出自定义主题 overlay  
- 自动发现 `$CODEWHALE_HOME/themes/` 下合法用户主题。  
- 在编译主题之后追加自定义行，并保留 `custom:<name>` 选择器状态。  
- 链接：https://github.com/Hmbown/Codewhale/pull/5907

### 6. [#5899] fix(version)：发布版 Cargo 包不再显示 `(dev)` 标记  
- crates.io 安装的 `codewhale 0.9.12` 之前会显示为 `codewhale 0.9.12 (dev)`，看起来像本地未发布构建。  
- 修复后 stamped 包显示正式版本号；源代码 checkout 仍保留 `(dev)`。  
- 链接：https://github.com/Hmbown/Codewhale/pull/5899

### 7. [#5902] refactor(tui)：会话生命周期切片统一改用 command shapes  
- FEAT-023 的推进项，涉及 `/branch`、`/compact`、`/fork`、`/load`、`/new`、`/purge`、`/save`、`/sessions`、`/tree`。  
- 有助于去掉平台相关命令解析逻辑，让 TUI 核心更可移植。  
- 链接：https://github.com/Hmbown/Codewhale/pull/5902

### 8. [#5900] fix：shell 执行指引与实际解释器对齐  
- 当底层选择 PowerShell 执行时，模型侧仍能看到 Bash 风格的 lowercase bash tool 指引，导致模型按错误语法生成命令。  
- 改为从 ShellDispatcher 动态推导给模型的工具说明。  
- 链接：https://github.com/Hmbown/Codewhale/pull/5900

### 9. [#5868] feat：OpenCode Go/Zen provider 增加 `x-opencode-session` 请求头  
- 方便 OpenCode 服务端做 prompt caching 与流量归因。  
- 来自 huangxianzhan 的贡献，已合入。  
- 链接：https://github.com/Hmbown/Codewhale/pull/5868

### 10. [#5895] fix(computer-use)：HarmonyOS 文件读取清理只删除自己创建的临时目录  
- 修复一个危险清理 bug：`hdcExec().readFile()` 先前会递归删除 `os.tmpdir()` 父目录，可能误删系统中其它临时文件。  
- 失败传输也会留下部分下载文件，一并处理。  
- 链接：https://github.com/Hmbown/Codewhale/pull/5895

> 另有两类依赖更新 PR：GitHub Actions 依赖升级（QEMU、action-gh-release、create-github-app-token）以及 Rust 依赖升级（lru、tower-http），可作为例行维护关注。[查看全部 PR](https://github.com/Hmbown/Codewhale/pulls)

---

## 5. 功能需求趋势

从近期 Issue/PR 看，社区最关注的功能方向集中为：

1. **上下文管理与长会话稳定性**  
   - 代表：#5620（压缩预警主动化）、#5820（token 预算边界）、#5849（模型实时目录解析）。  
   - 线索：用户希望 Codewhale 在上下文紧张时主动降级，而不是“提示一次就消失”。

2. **Windows 平台与本地自动化完善**  
   - 代表：#5908、#5909、#5898、#2323。  
   - 线索：win32 computer-use、PowerShell 交互、CRLF 处理、Windows 并发 idle 测试，都是 Windows 真实用户的高频环境。

3. **编辑器/IDE 互操作（ACP 深度集成）**  
   - 代表：#5863、#5864（ACP 缺少 session/list、config 暴露）。  
   - 线索：editor clients 不只希望“运行”，还希望切换 mode/model、枚举历史 session。

4. **TUI 视觉与操作密度优化**  
   - 代表：#5888（Fleet 菜单）、#5901（自定义主题）、#5887（MCP 状态可见性）。  
   - 线索：功能增多后，UI 从“功能全”转向“找得到、看得懂”。

5. **多模态/本地语音输入**  
   - 代表：#5846。  
   - 线索：要求端侧 STT 默认、API key 兜底、单键快捷键与电平动画，属于新交互入口的探索。

6. **本地化与中文社区支持**  
   - 代表：#5482（文档中文 EPIC）、#2323（中文输入法）。  
   - 线索：中文用户占比上升，官方已把“docs 全部中文化”列为 EPIC。

---

## 6. 开发者关注点

### 痛点 1：工具“假成功”比失败更可怕
- Windows computer-use 后端在 PowerShell 未执行时返回 `{action_sent: true}`。  
- `write_file` 把 CRLF 静默改成 LF，git 历史里会多出“幽灵 diff”。  
- 开发者期待：工具调用要么真实执行，要么返回失败原因；状态上报不能靠“猜”。

### 痛点 2：后台任务状态不透明
- MCP 启动期 “20 connecting” 长时间不变，用户无法区分“缓慢”和“卡死”。  
- Fleet 中已取消 Agent 的写锁不释放，后续写任务只看到 “write-scope contention”，缺少因果链。  
- 核心诉求：中间状态要有进度、要有推出的路径，错误要能回溯到具体 Agent/资源。

### 痛点 3：文件与资源清理的安全边界
- HarmonyOS 读文件后会递归删除临时目录，险些误删系统临时文件。  
- web fetch 依赖缓存状态，清理/缓存逻辑不一致会直接改变 Agent 行为。  
- 开发者共识：清理逻辑必须限定在自己创建的资源内，不能对共享目录做递归操作。

### 痛点 4：发布流程需要“先全量验证，再上传”
- v0.9.12 发布时 18 个 crates 已上传，`codewhale-tui` 却因缺少插件文件校验失败。  
- 新安装的 Cargo 包 `0.9.12 (dev)` 还会误导用户判断版本来源。  
- 涉及：#5892 / #5893 / #5891 / #5899。发布质量门禁正在被重点补强。

---

> 里程碑跟踪可关注：[Issue #5573](https://github.com/Hmbown/Codewhale/issues/5573)（v0.9.12 收尾与 0.9.13 排序）、[EPIC-005 #5316](https://github.com/Hmbown/Codewhale/issues/5316)（TUI crate 架构拆分）。

</details>

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*