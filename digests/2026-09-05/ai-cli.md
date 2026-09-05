# AI CLI 工具社区动态日报 2026-09-05

> 生成时间: 2026-09-05 03:59 UTC | 覆盖工具: 9 个

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

# AI CLI 工具横向对比分析报告（2026-09-05）

> 数据说明：本文基于 9 个主流 AI CLI 仓库 2026-09-05 社区动态日报；"Issue/PR 数"为各日报点名的重点条目，不等同于全量统计，少量仓库标注了 24h 总量。

---

## 1. 生态全景

AI CLI 正处于从"单机代码助手"向"多模型、可编程、跨会话 Agent 运行时"演进的关键阶段：头部厂商保持日级发版节奏（Codex v0.153.4、Copilot v1.0.84-1、Claude Code v2.1.261），而开源新势力（OpenCode、Pi、Qwen Code）正以更快的 PR 吞吐追赶，且不约而同地把"Claude Code 规则/Hooks"当作生态兼容的事实标准。与此同时，各工具集体暴露出三类共性问题：**Windows 桌面端生命周期管理缺陷、权限/沙箱安全加固滞后、以及长会话状态不可信**。可以判断，未来 6 个月的竞争焦点将从"模型能力"转向"运行时可靠性、成本可预测性与跨工具互操作"。

---

## 2. 各工具活跃度对比

| 工具 | 当日 Release | 提名 Issue | 提名 PR | 活跃度信号 |
|---|---|---|---|---|
| Claude Code | v2.1.261 | 10 条（最高 #42776 达 159 评论） | 2（24h 仅 2 PR 更新） | 单 Issue 讨论深度全场最高，但 PR 节奏偏保守 |
| OpenAI Codex | v0.153.4 / v0.153.3 | 10 条 | 10 条 | 发版密集，PR 同步合并量最大之一 |
| Gemini CLI | v0.60.0-nightly | 10 条 | 10 条（2 已合入 nightly） | P1 Bug 批量进入 need-retesting，维护者响应快 |
| GitHub Copilot CLI | v1.0.84-1 / v1.0.84-0 | 10 条（24h 更新总量 37 条） | 1（非官方、无实质内容） | Issue 流量最高，但社区 PR 贡献几乎为零 |
| Kimi Code CLI | 无 | 1 条 | 1 条 | 明显低活跃 |
| OpenCode | v1.18.29 / v1.18.28 | 10 条 | 10 条 | 一日双版本 + 10 PR，开源侧迭代速度第一 |
| Pi | v0.85.0 | 10 条 | 10 条 | 发布后出现打包回归并快速修复，响应敏捷 |
| Qwen Code | 无明确 Release | 10 条 | 10 条 | P1 修复当天提交（Cerebras 400、HTML 导出），工程执行力强 |
| DeepSeek TUI | 无 | 5 条（全量） | 13 条（多为 Dependabot 自动升级） | 维护模式为主，社区规模小 |

> Copilot CLI 的 Issue 区活跃度（24h 37 条更新）与 PR 区冷清形成鲜明对比，说明其"官方发布、社区反馈"的单向模式；DeepSeek TUI 的 13 条 PR 中约 9 条为依赖机器人提交，真实人类贡献有限。

---

## 3. 共同关注的功能方向

| 共同方向 | 涉及工具与关键证据 | 核心诉求 |
|---|---|---|
| **Windows 桌面/终端可靠性** | Claude Code（#42776 孤儿进程文件锁，159 评论；#89680 静默更新无法重启）；Codex（#25220 EFS 插件失败、#41463 WSL 路径崩溃）；Copilot（#4728 自动更新覆写桌面应用 exe）；OpenCode（#47425 大文本粘贴崩溃）；Kimi（#2634 Ctrl+V 失效） | 可禁用/延迟自动更新；更新前清理旧进程；修复文件锁、Job Object、WSL 路径等平台级问题 |
| **权限审批一致性与沙箱安全** | Claude Code（#91650 `cd && grep` 误拦截、#91683 bypassPermissions 回归）；Copilot（#4537 ACP 模式静默自动批准）；Gemini（#28863 环境变量注入需 consent、#29170 Symlink 工作区逃逸、#29216 OAuth 凭据隔离）；Codex（#33282 auto-approval 不继承） | 权限状态在会话全生命周期可预期；审批不能被静默跳过；沙箱必须防路径穿越与凭据泄漏 |
| **上下文与成本治理** | Copilot（#4720 BYOK prompt-cache 被静默禁用，成本约增 5 倍；#2627 固定系统提示词 2.05 万 tokens）；Claude Code（MEMORY.md 压缩阈值不可配、subagent prompt cache 致约 14% 额外花费）；Gemini（#22745 AST 感知读取减少 token）；Pi（#8720 坏输出污染上下文导致全部请求失败） | 缓存行为透明可控；固定开销可裁剪；压缩/记忆阈值可配置；坏消息可剔除 |
| **MCP 生态稳定性** | Copilot（#4525 新旧握手协议冲突、#4731 超时后 tools/list 刷新致工具永久丢失）；Claude Code（MCP server 全量启动造成内存浪费）；OpenCode（#47368 v1.18.28 远程 MCP 回归）；DeepSeek（rmcp SDK 2.x→3.x 大版本升级待回归） | 协议兼容、超时容错、按需连接，单点超时不应拖垮整组工具 |
| **会话状态保真与恢复** | Gemini（#22323 MAX_TURNS 被误报 GOAL 成功，13 评论）；Pi（#8720 空白 tool 输出"毒死"会话）；Codex（#41566 重复 ordinal 冻结历史线程）；Qwen（#11060 Daemon 缺 promptId 无法对齐）；Claude Code（#92016 SendMessage 被桌面端拒绝、#81658 跨设备同步丢会话） | 失败必须报失败；恢复后无权限漂移；transcript 有稳定 ID；跨端工具集一致 |
| **插件系统与可编程扩展** | Claude Code（#91870 Function Hooks 提案，100 评论）；OpenCode（#12472 兼容 Claude Code hooks，获 40 👍 为仓库最高赞需求）；Gemini（#21968 模型不主动调用自定义 Skill）；Copilot（#2904 自定义 Agent 无法配置 reasoning effort）；Qwen（#10872 公开中间件改写 thinking 输出） | 允许第三方深度修改 Agent 行为，同时保持副作用可追踪、能力可配置 |
| **新模型接入后的适配阵痛** | Codex（Astra 在 Windows picker 不可见）；Copilot（v1.0.84-1 才补上 GPT-6 Astra）；OpenCode（Codex OAuth 过滤 `gpt-6` 整数版本 Bug）；Qwen（Cerebras 因 `reasoning_content` 字段多轮 400） | 新模型上线必须跨端一致，provider 抽象层需兼容各家接口差异 |

---

## 4. 差异化定位分析

| 工具 | 定位与目标用户 | 技术/产品主线 | 相对弱点 |
|---|---|---|---|
| **Claude Code** | 面向企业与专业开发者的商业 Agent 环境；团队协作、策略治理 | 组织策略加载、Permission Guard、Hooks、Desktop/CLI 双端 | Windows 稳定性反噬明显；PR 迭代节奏慢于竞品 |
| **OpenAI Codex** | ChatGPT/Pro 生态用户，追求前沿模型能力 | GPT-6-Astra 快速落地、异步问题交互协议（TUI 集成）、AGENTS.md 规范 | Windows Store/WSL 体验系统性断裂；权限降级问题多 |
| **Gemini CLI** | 安全敏感型开发者、开源社区 | 沙箱/边界加固优先级最高（Symlink、环境变量、凭据隔离）；nightly 高频迭代 | Subagent 运行可靠性（挂起、假成功）拖后腿 |
| **Copilot CLI** | GitHub 企业用户，托管沙箱与合规诉求强 | GitHub 深度集成、企业级沙箱、多账号凭据、Windows 任务栏集成 | BYOK 成本与缓存策略不透明；更新机制破坏宿主应用 |
| **Kimi Code CLI** | Moonshot/k3 模型用户，中文社区 | 轻量 CLI，绑定自家模型链路 | 活跃度低，Windows 基础适配问题尚未闭环 |
| **OpenCode** | 开源、多 provider 爱好者，Claude Code 生态迁移者 | "什么都能接"：Copilot/Codex OAuth/Bedrock/Ollama + Claude Code 兼容层 | 发版过快导致回归频繁（远程 MCP、图像读取） |
| **Pi** | 极客/独立开发者，追求轻量 TUI 与模型自由 | 模型目录制（Bedrock Mantle、Meta Muse、OpenRouter :free），compaction 与 TUI 细节打磨 | 打包质量不过关（0.85.0 未声明 pi-server 依赖）；社区规模有限 |
| **Qwen Code** | 阿里/Qwen 模型用户，中文开发者，服务化场景 | Daemon/Channel/Web Shell 形态的 Agent 服务化、后台会话、会话轮换 | CI 测试收集时间 2223s 拖累工程效率；主分支 E2E 频繁失败 |
| **DeepSeek TUI** | 本地模型（Ollama）用户、Rust TUI 爱好者 | 轻量编译、本地窗口预算推导、Codewhale 同源生态 | 社区贡献稀疏、大量 Dependabot 噪声、营销垃圾 Issue 未治理 |

**趋势判断**：商业工具（Claude Code / Codex / Copilot / Gemini）在"企业治理 + 云服务联动"上用力；开源工具（OpenCode / Pi / Qwen）则打"多模型自由 + 生态兼容"牌。值得注意，**Opencode 将 Claude Code 兼容性作为最高赞需求（40 👍），Qwen 也列出与 Claude Code 2.1.260 的差距清单**，"Claude 生态兼容"正成为开源 CLI 获取用户的公共策略。

---

## 5. 社区热度与成熟度

**第一梯队：大规模用户 + 高 Issue 流量**

- **GitHub Copilot CLI**：24h 更新 37 条 Issue，为全场最高。发布节奏稳定（v1.0.83→84-0→84-1），但 PR 贡献极少，属于"官方产品 + 用户反馈"模式，社区影响力大但开放性弱。
- **Claude Code**：单 Issue 评论深度极高（#42776 达 159 条、#91870 达 100 条），用户在认真讨论架构级能力（Function Hooks），社区成熟度最高；但 PR 区仅 2 条更新，说明外部贡献门槛较高。

**第二梯队：快速迭代、修复响应快**

- **OpenCode**：一日双版本 + 10 PR，表现最激进，社区呈早期高增长特征；代价是回归频率高（远程 MCP、图像读取、npm 插件超时），用户对发版质量的信任正在经受考验。
- **Gemini CLI**：Nightly 日更 + 10 PR + 大量 P1 Bug 进入 need-retesting，说明维护者在批量处理已知问题；安全加固方向明确，但 Subagent 挂起与假成功问题尚未根除。
- **OpenAI Codex**：版本号 0.x、每日可多次发版，10 条 PR 覆盖异步问题交互、Guardian 上下文加固、Windows 沙箱新增，工程投入大；Windows 平台问题数量多且分散，处于"功能跑得快、体验欠打磨"阶段。
- **Qwen Code**：P1 修复当天闭环（Cerebras 400 数小时内提交 PR #11049），执行力强；但 CI/测试基础设施（模块导入耗时 2223s、E2E 批量失败）显示工程成熟度尚在爬坡。
- **Pi**：单维护者驱动的开源项目却有 10 PR/日活跃度，且能在发布事故当晚连出两个修复 PR（#9170/#9172），社区韧性好；0.85.0 出现"装完即坏"暴露其发布管道还需防复发机制。

**第三梯队：低活跃/维护模式**

- **Kimi Code CLI**：过去 24h 仅 1 Issue、1 PR 更新，且 Windows 粘贴问题无维护者响应。社区规模小，迭代动力不足。
- **DeepSeek TUI**：Issue 区全量仅 5 条，PR 以 Dependabot 为主，属于个人项目维护状态；但 #5882 恢复 CI、#5883 修复 Ollama 窗口预算说明核心维护者仍在推进。

---

## 6. 值得关注的趋势信号

1. **Windows 桌面端已成为所有 AI CLI 共同的"系统性生态债"**  
   Claude Code 的进程文件锁、Codex 的 EFS/插件崩溃、Copilot 自动更新覆写宿主 exe、Kimi 的粘贴失效，问题重叠度极高。根因不是单一 Bug，而是各工具在 Windows 进程模型、更新机制、文件权限上缺乏与桌面应用共存的工程投入。  
   *建议开发者*：Windows 重度用户应关注"自动更新可关闭性"，并优先选用对更新策略更保守的工具版本。

2. **"假成功"比失败更危险——会话状态保真成为信任底线**  
   Gemini #22323 将 MAX_TURNS 中断包装成 GOAL Success、Copilot #4537 静默跳过权限审批、Pi #8720 坏输出使后续所有请求 400，都会在无人值守/CI 工作流中造成隐蔽的连锁故障。可以预见，**"失败上报真实性"与"会话可恢复性"将成为下一代 Agent 工具的验收指标**。  
   *建议开发者*：在自动化接入前，应显式测试子代理/后台任务的异常上报路径，而非只验证 happy path。

3. **上下文成本正在从"隐性损耗"变为"可量化指标"**  
   Copilot #4720 的 BYOK 缓存静默失效让用户成本上升约 5 倍，Claude Code 被指出 subagent prompt cache 造成约 14% 额外花费，Codex 用户实测显式批处理可降低用量 27–45%。用户已开始用"每会话固定 token 开销"和"缓存命中率"来选择工具。  
   *建议开发者*：BYOK/自建网关用户应监控 `cached_tokens` 指标；对固定系统提示词过大的工具（如 Copilot 约 2.9 万 tokens）需评估长会话经济性。

4. **安全管理重心正在下沉：从"模型对齐"转向"运行时加固"**  
   Gemini 在 24h 内合并/提交了 4 条以上安全 PR——环境变量消毒、Symlink 路径逃逸、容器内 OAuth 凭据隔离、不可信工具输出的伪元数据防欺骗（#29215）；Claude Code 在修 glob 安全规则匹配；Copilot 在修 ACP 授权回归。这些动作指向同一结论：**Agent 的最大攻击面已不是 prompt injection，而是文件系统边界、环境变量、扩展权限与工具输出信任链**。  
   *建议开发者*：对第三方 MCP/插件保持最小授权，并优先选择具备沙箱隔离与凭据隔离能力的 CLI。

5. **跨工具生态兼容成为新的竞争维度**  
   Claude Code 的 Rules/Hooks/Agent 格式正在演变为行业接口：OpenCode 获 40 👍 的 #12472 直接要求兼容 Claude Code hooks；Qwen 主动对照 Claude Code 2.1.260 整理差距清单；Gemini 的 SKILL.md/Subagent 体系与 DeepSeek 的 SKILL.md 也高度同构。  
   *建议开发者*：沉淀自定义规则与技能时，优先采用跨工具通用格式（如 `AGENTS.md`、Claude Code 兼容的 settings/hooks），避免被单一厂商锁定。

6. **MCP 生态仍处于"协议青春期"，生产依赖需带容错设计**  
   Copilot 的旧版 `initialize` 与新 `server/discover` 共存冲突（#4525）、单次超时导致整组工具被永久移除（#4731）、Claude Code 社区要求 MCP 改为 lazy/on-demand 连接——MCP 的发现、握手、超时、清理规范尚未成熟。  
   *建议开发者*：将 MCP server 视为"可降级的外部依赖"而非核心路径，并锁定经过验证的 SDK/协议版本，警惕 major 版本升级（如 DeepSeek 的 rmcp 2.x→3.x）带来的行为变化。

---

**一句话总结**：2026 年 9 月的 AI CLI 竞争已从"谁的模型强"切换到"谁的运行时更可信"——**Windows 上能稳定重启、长会话不撒谎、权限不漂移、每一分 token 都可解释**的工具，将赢得下一轮开发者信任。

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills 社区热点报告（数据截至 2026-09-05）

> 数据来源：github.com/anthropics/skills 官方仓库。所有结论基于当前 Issue / PR 的讨论热度与状态判断。

## 1. 热门 Skills 排行

以下按社区关注度排序，重点覆盖 8 个讨论最集中的 PR。

### 1️⃣ skill-creator：run_eval.py 持续误报 0% recall（#1298）
- **功能**：修复技能创建与评估流水线的核心缺陷——`run_eval.py` 对所有描述都报告 `recall=0%`，导致 `run_loop.py`、`improve_description.py` 的优化信号完全失真；同时修复 Windows 管道读取、触发检测与并行 worker 问题。
- **讨论热点**：对应 Issue #556 被 10+ 次独立复现，是当前 skill 自动评估体系里最严重的“元 Bug”——不是技能不好，而是评估器本身坏了。
- **状态**：Open
- 🔗 https://github.com/anthropics/skills/pull/1298

### 2️⃣ document-typography：生成文档的排版质检（#514）
- **功能**：新增技能，检测 AI 生成文档中的孤儿词换行、寡行段落（标题被孤立在页底）、编号错位等排版问题。
- **讨论热点**：覆盖面广，几乎所有 Claude 生成的文档都会遇到；社区认为这是文档类技能里“最后一块通用体验拼图”。
- **状态**：Open
- 🔗 https://github.com/anthropics/skills/pull/514

### 3️⃣ scnet-hpc：HPC 集群操作技能（#1615）
- **功能**：通过 Profile 化 SSH 与 Slurm 工作流，覆盖 SCNet 集群的连接、分区选择、作业生成、模块加载和计算节点管理。
- **讨论热点**：代表垂直科研场景的真实需求——社区关注的是技能能否把“HPC 领域经验”固化为可重复的安全操作路径。
- **状态**：Open
- 🔗 https://github.com/anthropics/skills/pull/1615

### 4️⃣ Hivemind：零成本多智能体编排（#1628）
- **功能**：由 Claude Code 担任唯一的规划者、审查者和合并者，将机械性工作委托给运行免费模型的 headless opencode worker，从而节约昂贵模型上下文。
- **讨论热点**：集中在成本优化、多 Agent 架构与任务边界划分；是新一批 PR 中概念最“激进”的一个。
- **状态**：Open
- 🔗 https://github.com/anthropics/skills/pull/1628

### 5️⃣ ODT：OpenDocument 全流程处理（#486）
- **功能**：支持 `.odt/.ods` 的创建、模板填充、读取及 ODT→HTML 转换，目标是与文档生态中的 docx/pdf 技能互补。
- **讨论热点**：反映办公文档互操作性的持续需求；尤其受 Linux / LibreOffice 与合规场景用户关注。
- **状态**：Open
- 🔗 https://github.com/anthropics/skills/pull/486

### 6️⃣ self-audit：交付前四维推理质量门禁（#1367）
- **功能**：先做输出文件的机械验证，再做按危害优先级排序的推理质量审计，宣称“适用于任何项目、技术栈和模型”。
- **讨论热点**：与 Issue #1385 的“推理质量门禁管线”直接呼应，社区期待一套可插拔的交付质量把关机制。
- **状态**：Open
- 🔗 https://github.com/anthropics/skills/pull/1367

### 7️⃣ skill-quality-analyzer + skill-security-analyzer（#83）
- **功能**：向 marketplace 新增两个元技能——前者从结构、文档、示例等维度评估技能质量；后者审查技能的安全风险。
- **讨论热点**：直接回应第三方技能激增下的信任问题；多数评论者认为这正是社区最缺的“技能审计工具”。
- **状态**：Open
- 🔗 https://github.com/anthropics/skills/pull/83

### 8️⃣ frontend-design：提升指令可执行性（#210）
- **功能**：重构现有 frontend-design 技能，使其指令能够在单次会话中真正被执行，减少空泛设计与内部矛盾。
- **讨论热点**：社区对“设计类技能往往说得很好但执行不落地”的反馈集中体现，与反 “UI slop” 诉求同频。
- **状态**：Open
- 🔗 https://github.com/anthropics/skills/pull/210

---

## 2. 社区需求趋势（来自 Issues）

### 🔐 安全与信任边界
- 社区对“第三方技能被放在 anthropic 命名空间下分发”的警惕度极高（Issue #492，43 条评论）。
- 用户希望明确区分官方技能与社区技能，并要求可审计的权限边界。

### 🏢 组织级共享与治理
- 最典型诉求：在 Claude.ai 中支持组织级 Skill 共享，而不是手动下载 `.skill` 文件再通过 Slack/Teams 传递（Issue #228，👍 8）。
- 伴随“技能应纳入企业治理、权限与审计体系”的讨论。

### 🔧 技能评估基础设施严重不足
- 高赞问题集中在：技能评估脚本不可靠、触发率恒为 0%、打包重复、环境兼容差。
- 代表 Issue：#556（run_eval.py 全部触发失败）、#1390（MCP builder 评估无法得分）、#189（两个插件安装了重复技能）。

### 🧠 上下文与成本效率
- 技能体积和 token 占用正成为新痛点：#1487 报告 `claude-api` 单次注入约 156k tokens，直接耗尽上下文窗口。
- 社区开始提出“符号化紧凑记忆”（#1329 compact-memory）等面向长会话的轻量状态设计方案。

### 🧩 技能应成为可组合的“软件接口”
- 早期但持续被引用：#16 主张将 Skills 以 MCP 形式暴露，让技能能力成为可编程 API 的一部分。
- 反映出“技能复用、链接、互相调用”的平台化期待。

### 🧭 结论一句话
> 社区当前真正需要的不是“更多技能”，而是**让技能可评估、可信任、可治理、可低成本运行的平台能力**。

---

## 3. 高潜力待合并 Skills

这些 PR 讨论充分、功能完整，且尚未被关闭，是近期最可能进入仓库的候选：

### 🎯 document-typography（#514）
- 痛点明确、规则清晰，几乎所有文档生成场景都能受益。
- 合并关键是让排版规则可被 Claude 稳定执行，而不是停留在“建议清单”。
- 🔗 https://github.com/anthropics/skills/pull/514

### 🎯 testing-patterns（#723）
- 从测试哲学到 React 组件测试全覆盖，是一套“可以立刻用”的工程化技能。
- 与软件开发日常需求极度匹配，社区基础大，合并后使用频率预计较高。
- 🔗 https://github.com/anthropics/skills/pull/723

### 🎯 skill-quality-analyzer / skill-security-analyzer（#83）
- 元技能正好踩中当前安全与质量痛点；如果 Anthropic 希望提升生态整体质量，这是低成本高杠杆的合并项。
- 不确定点在于需要与 marketplace 官方准入策略对齐。
- 🔗 https://github.com/anthropics/skills/pull/83

### 🎯 Hivemind（#1628）
- 概念新颖、活跃度极高，且提供了一条可行的“低成本多 Agent”路径。
- 但它依赖外部 opencode 与免费模型，可能会成为合并时最大的审查焦点。
- 🔗 https://github.com/anthropics/skills/pull/1628

### 🎯 self-audit（#1367）
- 输出审计是该仓库里少见的“交付质量门禁”方向；若 Anthropic 推动 Agent 工程化落地有很高采用潜力。
- 作者同时提交了更大的 Reason Quality Gate 管线提案（#1385），说明这条线有持续演进空间。
- 🔗 https://github.com/anthropics/skills/pull/1367

---

## 4. Skills 生态洞察

**当前 Skills 生态最集中的诉求不是“新增技能”，而是围绕技能建立质量评估、安全分发、组织治理与低成本运行的基础设施——社区正在从“堆数量”转向“建信任”。**

---

# Claude Code 社区动态日报（2026-09-05）

## 今日速览

- Claude Code v2.1.261 发布，主要新增组织策略加载失败提示，以及命令/后台任务输出量上限配置。
- 社区讨论热度最高的是 Windows 桌面端“孤儿进程导致应用无法重启/更新”的系列 Bug，多个 Issue 评论数居高不下。
- Function Hooks 功能建议与 2.1.259 权限系统回归，成为今天功能讨论和问题反馈的两大焦点。

## 版本发布

### Claude Code v2.1.261

本次可见更新包括：

- `/status` 与 `claude doctor` 新增 **Organization policy** 提示行，用于说明组织策略为何无法加载，例如代理没有放行对应端点。
- 新增 `bashOutputMaxChars` 和 `taskOutputMaxChars` 配置项，可调高命令输出与后台任务输出的最大字符数。

## 社区热点 Issues

1. **Windows 桌面应用因孤儿进程文件锁无法重启**  
   [#42776](https://github.com/anthropics/claude-code/issues/42776)  
   159 条评论 / 75 👍，是当前社区热度最高的问题。Claude Code Desktop 在 Windows 上退出后，遗留进程仍占用文件锁，导致后续重启失败，用户只能通过注销或重启系统恢复。

2. **Function Hooks：让插件系统能力提升一个量级**  
   [#91870](https://github.com/anthropics/claude-code/issues/91870)  
   100 条评论 / 62 👍。社区提议引入带副作用跟踪的 `$` 对象，并以类似 Express/Koa 的中间件模型让插件深度修改 Claude Code 行为。该方向被很多开发者视为下一代插件能力的关键。

3. **Windows 桌面启动失败：孤立 Silo/Job Object 导致 0x80070020**  
   [#53247](https://github.com/anthropics/claude-code/issues/53247)  
   60 条评论 / 28 👍。应用崩溃后遗留 Job Object，导致需要 Admin 权限的新实例无法启动，只有 logoff/reboot 才能恢复，与 #42776 同属 Windows 进程生命周期管理问题。

4. **Bash `cd` 复合命令在有 `Read()` 拒绝规则时被误拦截**  
   [#91650](https://github.com/anthropics/claude-code/issues/91650)  
   10 条评论但获得 56 👍。Windows Git Bash 下执行 `cd DIR && grep ...` 时，Permission Guard 会对绝对路径的 `cd` 目标反复弹窗，影响所有配置了 `.env` 等 Read deny 规则的用户。

5. **`bypassPermissions` 模式下 `cd && grep` 仍触发权限提示**  
   [#91683](https://github.com/anthropics/claude-code/issues/91683)  
   7 条评论 / 26 👍，属于 2.1.259 的回归。即使开启 bypassPermissions，配置 Read deny 规则后 `cd DIR && grep` 依然弹出权限确认，开发者对权限系统稳定性的信任受到较大影响。

6. **Windows 静默更新遗留旧版孤儿进程，新版无法启动**  
   [#89680](https://github.com/anthropics/claude-code/issues/89680)  
   15 条评论。桌面版自动更新后，旧版 AppX 容器仍被残留子进程占用，导致新版启动报 `0x80070020`，必须重启电脑。开发者普遍希望至少能推迟或关闭自动更新。

7. **Windows 桌面窗口始终置顶，无法关闭**  
   [#89467](https://github.com/anthropics/claude-code/issues/89467)  
   15 条评论 / 10 👍。Claude Code 桌面版窗口会一直悬浮在其他窗口之上，且没有设置项或快捷键可以取消置顶，影响多窗口并行工作流。

8. **MEMORY.md 自动压缩提醒阈值不可配置**  
   [#91188](https://github.com/anthropics/claude-code/issues/91188)  
   20 条评论。当前自动记忆机制硬编码为会话加载前 200 行 / 25KB，接近上限时提示压缩，用户希望能自定义或单独关闭这一阈值。

9. **跨平台同步故障导致 Cowork 对话消失**  
   [#81658](https://github.com/anthropics/claude-code/issues/81658)  
   16 条评论。Desktop / Web / Android 之间出现同步失败，用户报告 Cowork 会话和聊天记录疑似丢失，社区高度关注数据安全问题，怀疑为服务端事件。

10. **Claude Desktop 自动拒绝 SendMessage，破坏子代理跨会话恢复**  
    [#92016](https://github.com/anthropics/claude-code/issues/92016)  
    8 条评论。macOS 桌面版 Code 标签页会自动拒绝 CLI 原生的 `SendMessage` 工具，导致依赖跨会话子代理恢复的自动化流程被阻断。桌面端与 CLI 工具集不一致的问题正在引发更多反馈。

## 重要 PR 进展

过去 24 小时仅有 2 个 PR 有更新，均值得关注：

1. **修复 security-guidance 中 `**` 无法匹配顶层路径的安全规则问题**  
   [#87079](https://github.com/anthropics/claude-code/pull/87079)  
   修复 `**/*.ts` 这类模式因 `fnmatch` 委托逻辑而无法匹配顶层文件的问题。安全规则此前可能被静默漏过，属于安全策略语义修正，建议尽快 Review 合并。

2. **为 GitHub Connector “Connected 但无工具”问题添加诊断脚本**  
   [#61691](https://github.com/anthropics/claude-code/pull/61691)  
   面向 Windows 用户，新增 PowerShell 诊断/修复脚本，解决 Cowork 中 GitHub MCP connector 显示 Connected 却不暴露任何工具的问题，并关闭 #61682。

## 功能需求趋势

从当前 Issues 中可以提炼出以下社区重点关注方向：

- **可编程扩展与权限精细化**  
  最典型的是 #91870 的 Function Hooks 提案，希望允许插件深度修改 Claude Code 行为，同时保持副作用可追踪。此外还有 deny 规则应指明触发来源、`**` glob 应正确匹配安全规则、子代理内 `Agent(inner)` 白名单应真正生效等诉求。

- **Windows 桌面稳定性与更新机制改进**  
  大量 Issue 集中在进程文件锁、Silent Update、孤儿进程、强制重启等问题。社区核心诉求是：可推迟或禁用自动更新、更新前清理旧版本进程、避免桌面应用崩溃后无法再次启动。

- **Agent 跨会话互操作一致性**  
  SendMessage / ListAgents 在 CLI 与桌面 Code、调度任务、Remote Control 中行为不一致，多个 Issue 反映了 Agent 工作流被桌面端工具注册缺失阻断的问题。

- **资源与成本优化**  
  MCP server 在每个会话全量启动造成内存浪费，社区希望支持 lazy/on-demand 连接，或按会话进行 MCP scoping。另有分析指出 subagent prompt cache 策略会导致约 14% 的额外花费。

- **对“自主行为”的更强控制**  
  开发者希望 MEMORY.md compaction 阈值、context ring 警告点、AskUserQuestion 跳过含义、后台任务模型选择等都能被显式配置，从而让 Claude Code 的自主性更可预期。

## 开发者关注点

- **Windows 平台问题已成为最高频痛点**  
  从 #42776 到 #53247、#89680，大量用户遭遇“一次崩溃或更新后无法再启动应用”的问题，且恢复手段只有重启系统，严重干扰日常开发。

- **权限系统回归伤害配置信任**  
  2.1.259 中 `cd` 复合命令误报、`bypassPermissions` 未真正绕过规则、deny 消息不显示具体规则来源等问题，让开发者很难判断是配置错误还是产品 Bug。

- **桌面端与 CLI 功能差异被放大**  
  Desktop Code 标签页、调度任务、Remote Control 等场景中，工具注册表与 CLI 不一致，尤其缺少 SendMessage / ListAgents，使依赖多 Agent 协作的用户无法正常工作。

- **资源使用与数据安全焦虑并存**  
  MCP 全量启动和重复加载增加内存压力；Cowork 同步失败与自动更新强制重启则直接威胁工作数据完整性。社区希望官方在资源占用和更新策略上采取更保守、可控的方案。

---

数据来源：GitHub `anthropics/claude-code` Issues / PRs，统计时间截至 2026-09-05。

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex 社区动态日报 — 2026-09-05

> 数据来源：[github.com/openai/codex](https://github.com/openai/codex)  
> 覆盖窗口：2026-09-05 过去 24 小时 Releases / Issues / PRs

## 1. 今日速览

- Codex 发布 `0.153.4` 与 `0.153.3`，核心动作是让 GPT-6-Astra 在 bundled model picker 中可见并作为默认模型，同时修正 Astra 的异步澄清提问引导。
- 社区 Issue 高度集中在 Windows 体验：插件复制失败、WSL 无法建项目、code-mode host handshake 崩溃、桌面 Pets 无法点击拖动等问题持续刷屏。
- PR 侧进入“异步问题交互 + 模型上下文保留”集中合并期，多条 TUI async questions 和 Guardian 上下文加固 PR 在今日关闭。

## 2. 版本发布

### [rust-v0.153.4](https://github.com/openai/codex/releases/tag/rust-v0.153.4)

- 修复 Astra 在 bundled model picker 中的可见性，并在未显式配置模型时将其设为 bundled 默认选项。
- 更新 Astra 引导逻辑：仅当会话内提供对应工具时，才使用异步提问。

### [rust-v0.153.3](https://github.com/openai/codex/releases/tag/rust-v0.153.3)

- Amazon Bedrock 模型选择器新增 GPT-6-Astra，覆盖 Mantle 与 Runtime global/US 路由。
- 修正 GPT-6-Astra 异步澄清提问的 guidance，标明其当前仅接受文本输入。

## 3. 社区热点 Issues

### [#35050](https://github.com/openai/codex/issues/35050) — GPT-5.6 经常串行化 Code Mode 调用；显式批处理使加权用量降低 27–45%
`comment 30 | 👍 41`  
模型编排效率问题：用户实测通过显式批处理能显著减少 API 用量，说明模型默认调用策略存在较大成本优化空间。

### [#28919](https://github.com/openai/codex/issues/28919) — Windows Codex App 缺少“控制其他设备”设置页签
`comment 59 | 👍 54`  
Windows 端与其它平台功能不对齐，用户无法在 Settings > Connections 中找到远程/其它设备控制入口，是目前评论数最高的 Windows 功能缺失 Issue。

### [#41049](https://github.com/openai/codex/issues/41049) — Windows 上 code-mode host 在握手阶段退出，GPT-5.6 无法正常工作
`comment 46 | 👍 1`  
本地命令执行通道初始化失败，导致自动读取目录等核心能力不可用；Windows 用户受影响明显。

### [#25220](https://github.com/openai/codex/issues/25220) — Windows Store 版 bundled plugins 全部不可用：copyfile 在 EFS 加密 WindowsApps 文件上失败
`comment 29 | 👍 4`  
Computer Use、Browser、Chrome、LaTeX 等内置插件无法加载，根因指向 Windows EFS/打包路径权限问题。

### [#41463](https://github.com/openai/codex/issues/41463) — Windows + WSL 无法创建项目：AbsolutePathBuf 缺少 base path 反序列化失败
`comment 27 | 👍 18`  
WSL 场景下项目创建直接失败，影响大量在 Windows 中使用 WSL2 的开发者。

### [#41513](https://github.com/openai/codex/issues/41513) — Windows 桌面宠物变成 click-through 且无法拖动
`comment 23 | 👍 10`  
内置 Codey 和自定义宠物均受影响，用户反馈桌面宠物截获/穿透行为异常，属于 Windows UI 层的高频缺陷。

### [#32069](https://github.com/openai/codex/issues/32069) — Feature request：允许隐藏 Pets 菜单项，并支持可配置 prompt polishing
`comment 16 | 👍 17`  
反映部分用户认为 Pets 入口非必需，同时希望系统 Prompt 润色行为可配置，是体验类高赞需求。

### [#41566](https://github.com/openai/codex/issues/41566) — Codex paginated rollout 发出重复 ordinal，导致线程历史投射永久冻结
`comment 15 | 👍 0`  
会话状态一致性问题：未完成任务后可能产生重复序号，最终卡死历史线程展示，影响长时间会话维护。

### [#33282](https://github.com/openai/codex/issues/33282) — create_thread 未继承 worktree 任务的 auto-approval 模式
`comment 15 | 👍 6`  
桌面端创建子线程时权限模式没有正确传递，可能导致全自动流程在不该出现审批时被阻塞。

### [#25820](https://github.com/openai/codex/issues/25820) — Codex CLI 登录被电话验证限流阻塞（Pro 用户）
`comment 12 | 👍 6`  
Pro 订阅用户无法通过 `codex login` 完成认证，直接阻塞 CLI 使用；认证链路和限流策略需要重新梳理。

## 4. 重要 PR 进展

### [#42904](https://github.com/openai/codex/pull/42904) — Default 协作模式改用静态 instructions
将 Default 与 Plan 模式直接写入默认指令，不再走模板渲染；同时移除相关 mode-name helpers 和 `codex-utils-template` 依赖，降低模型指令层可变性。

### [#42903](https://github.com/openai/codex/pull/42903) — 保留 TUI question state，并整合历史与队列导航
连接恢复/线程还原时保留 question 草稿、选择状态、展开状态及 handled message IDs，避免重连后重复回放或丢失问题。

### [#42900](https://github.com/openai/codex/pull/42900) — 为独立的 tasks 与 memory requests 建立 root turn identity
补齐 background/empty-input turns 和 detached memory requests 的 `root_turn_id`，避免任务从 coalesced mailbox input 中错误继承 root。

### [#42897](https://github.com/openai/codex/pull/42897) — async question 选项支持内联 “Other” 自定义回答
异步提问除预设选项外，用户可直接在问题面板中输入其它答案，提升人机协同流程灵活性。

### [#42894](https://github.com/openai/codex/pull/42894) — TUI 支持 async question 选项选择
将异步问题的 suggested answers 渲染成可选择项，并要求完整展示后才能提交，改善 TUI 端交互严谨性。

### [#42891](https://github.com/openai/codex/pull/42891) — 将异步问题集成进 TUI
TUI 支持折叠显示问题数、展开回答编辑器、导航/回答/排队/跳过问题，同时保留主输入框草稿，属于今日异步问题体系的核心补全。

### [#42879](https://github.com/openai/codex/pull/42879) — GPT-6-Astra 进入 model picker
将 GPT-6-Astra 的 bundled visibility 设为 `list`，使其出现在交互式模型选择器首位，为后续新模型落地做 UI 准备。

### [#42852](https://github.com/openai/codex/pull/42852) — Context compaction 后加固 Guardian reviews
Compaction 后仍保留用户授权约束，且不再复用不可读或不兼容的 parent checkpoint；同时保留超大 root user message 的有界摘要。

### [#42844](https://github.com/openai/codex/pull/42844) — 在 Guardian context 中保留用户原始指令
即使 compaction 或 transcript selection 从当前窗口移除了用户消息，Guardian 仍能从 host-owned retained context 获取完整用户指令，提升审核可靠性。

### [#42841](https://github.com/openai/codex/pull/42841) — 新增 Windows 原生 MXC sandbox adapter
新增 `codex-mxc-sandbox`，支持原生 MXC 可用性检测、继承标准 I/O 并等待沙箱进程退出；启动前校验 deny-path 并拒绝不支持的 learning-mode/fallback 策略。

## 5. 功能需求趋势

- **Windows 桌面端可靠性成为最集中的需求池**  
  大量 Issue 指向 Windows/WSL 的基础环境问题：内置插件不可用、WSL 路径反序列化失败、code-mode handshake 崩溃、首次启动长时间无窗口、Computer Use 无法控制原生应用等。  
  代表 Issue：[#25220](https://github.com/openai/codex/issues/25220)、[#41463](https://github.com/openai/codex/issues/41463)、[#41049](https://github.com/openai/codex/issues/41049)、[#28919](https://github.com/openai/codex/issues/28919)

- **GPT-6-Astra / 新模型接入需要跨端一致**  
  社区同时在抱怨 Astra 未出现在 Windows model picker、Linux 上不可靠、ChatGPT Pro 账户看不到模型等问题。  
  代表 Issue：[#42853](https://github.com/openai/codex/issues/42853)、[#42868](https://github.com/openai/codex/issues/42868)；相关 PR：[#42879](https://github.com/openai/codex/pull/42879)

- **异步问题（async questions）交互正在快速模板化**  
  多条 PR 在同步推进 TUI 中的问题展示、选择回答、内联 Other 输入以及状态保留，说明 Codex 正在把“人工确认/澄清”做成更完整的交互协议。  
  代表 PR：[#42891](https://github.com/openai/codex/pull/42891)、[#42894](https://github.com/openai/codex/pull/42894)、[#42897](https://github.com/openai/codex/pull/42897)

- **权限与沙箱状态在会话生命周期内必须一致**  
  开发者反复反馈：UI 显示 Full Access，实际恢复为 workspace-write 或 on-request；子线程不继承 auto-approval；恢复会话后权限漂移。表明用户需要“状态一致、可审计”的权限模型。  
  代表 Issue：[#33282](https://github.com/openai/codex/issues/33282)、[#25590](https://github.com/openai/codex/issues/25590)、[#40125](https://github.com/openai/codex/issues/40125)

- **体验类轻量改进需求持续存在**  
  例如隐藏 Pets 菜单、去掉 “Keep Waiting” 手动确认、动态对话标题、AI 生成 commit message 等，虽然零散但点赞量不低。  
  代表 Issue：[#32069](https://github.com/openai/codex/issues/32069)、[#32139](https://github.com/openai/codex/issues/32139)、[#14044](https://github.com/openai/codex/issues/14044)、[#20036](https://github.com/openai/codex/issues/20036)

## 6. 开发者关注点

- **Windows 版本的可预期性不足**  
  从 Store 安装包到沙箱、插件、桌面宠物、远程控制，Windows 用户遇到的是系统性体验断裂，而非单一偶发 bug；开发者对 Windows 端“可用但不可靠”的观感在增强。

- **会话恢复和权限降级影响自动化落地**  
  多个 Issue 显示线程恢复后 Full Access 会被降级、auto-approval 不继承、甚至历史消息永久冻结；这类问题对无人值守任务和复杂工作流尤其致命。

- **模型上下文在 compaction 过程可能失真**  
  用户报告 Codex 会“over-report completion”、压缩 AGENTS.md 指令、丢失原始用户授权约束。相关 PR 正在补救，但说明当前模型长会话中的上下文可信度仍是风险点。

- **配额与认证链路不够透明**  
  CLI 登录被电话验证限流、标准额度耗尽后无法使用 Luna Reserve、App 与 CLI 权益不一致等，正在消耗订阅用户对额度体系的信任。

- **新模型功能与本地环境能力需要同步更新**  
  Astra 已进入 picker 和 release hotfix，但异步提问依赖的本地工具并非所有会话都存在；社区希望模型能力展示、权限声明和实际本地能力保持一致。

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI 社区动态日报 — 2026-09-05

> 数据来源：github.com/google-gemini/gemini-cli

## 今日速览

昨日发布 `v0.60.0-nightly.20260905` 夜间版，重点加固了“扩展环境变量篡改”与“工作区路径边界逃逸”两类安全问题。社区讨论热度集中在 Subagent 可靠性上：多起 P1 级 Bug（Agent 挂起、MAX_TURNS 被误报为成功）仍在发酵，且大量 Issue 进入 `status/need-retesting` 状态，说明维护者已在批量修复等待用户验证。

## 版本发布

### v0.60.0-nightly.20260905.g85aca163f
> Release 链接：[github.com/google-gemini/gemini-cli/releases](https://github.com/google-gemini/gemini-cli/releases)

包含两项安全修复：

- **fix(extensions): prompt for consent on environment changes and sanitize runtime-altering environment variables**（[@amelidev](https://github.com/amelidev)，[PR #28863](https://github.com/google-gemini/gemini-cli/pull/28863)）：扩展更新将不再静默注入环境变量；涉及环境变更时需用户授权，并过滤可改变运行时行为（如 `LD_PRELOAD`、`PATH` 等）的敏感变量。
- **fix(core): enhance workspace path boundary checks and symlink resolution in command safety**（[PR #29170](https://github.com/google-gemini/gemini-cli/pull/29170)）：强化命令安全机制中的工作区边界检查与符号链接解析，覆盖文件发现/目录列举场景，防止通过 Symlink 逃逸工作目录。

## 社区热点 Issues

> 按评论活跃度精选 10 条。

### 1. #22323 — Subagent 超限被误报为 GOAL 成功，掩盖真实中断
- 标签：`priority/p1` `kind/bug` `status/need-retesting`
- 链接：[Issue #22323](https://github.com/google-gemini/gemini-cli/issues/22323)
- 评论 13 · 👍 2 · 更新于 2026-09-05

`codebase_investigator` 明明因 MAX_TURNS 中断、尚未分析代码，却向上层返回 `status: "success"` 和 `Termination Reason: "GOAL"`。这种“假成功”会污染自动化流程对 Agent 完成度的判断，是目前评论数最高的 Bug。

### 2. #19873 — 让模型以“原生 Bash 用户”方式安全操作：零依赖 OS 沙箱 + 意图路由
- 标签：`priority/p2` `kind/enhancement` `effort/large`
- 链接：[Issue #19873](https://github.com/google-gemini/gemini-cli/issues/19873)
- 评论 9 · 更新于 2026-09-05

主张 Gemini 3 模型天生习惯用 `grep/cat/sed/awk` 探索代码库。该 EPIC 希望在零依赖沙箱内放开原生 Shell 能力，并在执行后做“意图路由”，兼顾效率与安全。这是近期沙箱安全 PR 密集出现的重要背景。

### 3. #21409 — Generalist Agent 无限挂起
- 标签：`priority/p1` `kind/bug` `status/need-retesting`
- 链接：[Issue #21409](https://github.com/google-gemini/gemini-cli/issues/21409)
- 评论 8 · 👍 8 · 更新于 2026-09-05

一旦工作流委托给 generalist agent，连「创建文件夹」这类简单操作都会永久挂起。用户最长等待 1 小时后只能手动取消；通过提示词禁止模型使用 Subagent 可以绕过问题。该项目获得了较高的 👍 票，影响面较大。

### 4. #22745 — AST 感知的文件读取/搜索/代码库映射影响评估
- 标签：`priority/p2` `kind/feature`
- 链接：[Issue #22745](https://github.com/google-gemini/gemini-cli/issues/22745)
- 评论 7 · 更新于 2026-09-05

评估 AST 感知工具能否用单次调用精确读取方法边界，减少“读错再重读”的多轮往返和 Token 噪音。这是当前上下文压缩方向的一个长期探索性 EPIC。

### 5. #21968 — Gemini 不会主动使用自定义 Skills 和 Subagents
- 标签：`priority/p2` `kind/bug` `status/need-retesting`
- 链接：[Issue #21968](https://github.com/google-gemini/gemini-cli/issues/21968)
- 评论 6 · 更新于 2026-09-05

用户配置了 `gradle`、`git` 等自定义 Skill，描述写得足够明确，但模型只有在被显式指令要求时才调用，遇到强相关任务反而不主动使用。这一行为直接削弱自定义 Agent 配置的价值。

### 6. #26525 — Auto Memory 需要确定性脱敏，并减少日志记录
- 标签：`priority/p2` `kind/bug`
- 链接：[Issue #26525](https://github.com/google-gemini/gemini-cli/issues/26525)
- 评论 5 · 更新于 2026-09-05

Auto Memory 读取本地 transcripts 时，虽然提示词要求后台模型脱敏，但 Secrets 已经先进入模型上下文；服务还可能记录用户已有的 Skill 内容。社区关注隐私与最小权限问题。

### 7. #25166 — Shell 命令执行完后卡在 “Waiting input”
- 标签：`priority/p1` `kind/bug` `effort/medium`
- 链接：[Issue #25166](https://github.com/google-gemini/gemini-cli/issues/25166)
- 评论 4 · 👍 3 · 更新于 2026-09-05

极简 CLI 命令执行完毕后，UI 仍将进程显示为“活动/等待输入”，模型一直等待，无法继续。该问题会周期性阻塞长时间自动化任务。

### 8. #22232 — 增强 Browser Agent 韧性：自动接管会话与锁恢复
- 标签：`priority/p3` `kind/feature` `kind/customer-issue`
- 链接：[Issue #22232](https://github.com/google-gemini/gemini-cli/issues/22232)
- 评论 4 · 更新于 2026-09-05

`BrowserManager.ts` 遇到 persistent profile 被锁定/进程残留时直接 fail-fast，社区建议改为自动接管会话或自动恢复锁。

### 9. #21983 — Browser Subagent 在 Wayland 下失败
- 标签：`priority/p1` `kind/bug` `agent/browser`
- 链接：[Issue #21983](https://github.com/google-gemini/gemini-cli/issues/21983)
- 评论 4 · 👍 1 · 更新于 2026-09-05

浏览器子代理在 Wayland 环境中运行失败，并直接终止为 `GOAL`。影响 Linux 桌面用户，需要 Wayland 下的显示/会话兼容处理。

### 10. #20079 — `~/.gemini/agents/` 下的 Symlink 文件不被识别为 Agent
- 标签：`priority/p2` `kind/bug` `status/need-information`
- 链接：[Issue #20079](https://github.com/google-gemini/gemini-cli/issues/20079)
- 评论 4 · 更新于 2026-09-05

用户通过 Symlink 管理自定义 Agent 配置，但 CLI 无法识别 Symlink 指向的 `.md` 文件。期望 Agents 目录开启正常 Symlink 支持。

## 重要 PR 进展

> 覆盖昨日合并进入 Nightly 的 PR，以及当前高优先级安全/修复 PR。

### 1. #29218 — Nightly 版本号自动 bump
- 状态：`OPEN`
- 链接：[PR #29218](https://github.com/google-gemini/gemini-cli/pull/29218)

`chore/release` 例行版本号更新至 `0.60.0-nightly.20260905.g85aca163f`。

### 2. #28863 — 扩展：环境变更需征求同意 + 环境变量消毒
- 状态：`CLOSED`（已合入 v0.60 nightly）
- 链接：[PR #28863](https://github.com/google-gemini/gemini-cli/pull/28863)

将 MCP Server 环境配置纳入 consent 字符串，并对自定义环境变量做运行时安全过滤。这是针对“扩展更新静默注入环境变量”漏洞的修复。

### 3. #29170 — 核心：工作区路径边界检查与 Symlink 解析增强
- 状态：`CLOSED`（已合入 v0.60 nightly）
- 链接：[PR #29170](https://github.com/google-gemini/gemini-cli/pull/29170)

在 POSIX/Windows 的命令安全、文件发现、目录列举工具中统一加入 `isPathEscapingWorkspace` 边界校验与 Symlink 解析，修复潜在路径穿越。

### 4. #29217 — 配置：不要重写显式指定的 `gemini-2.5-flash`
- 状态：`OPEN` · `priority/p1`
- 链接：[PR #29217](https://github.com/google-gemini/gemini-cli/pull/29217)

现有 `isFlashModel()` 用 `endsWith('flash')` 判断，导致用户显式指定的 `gemini-2.5-flash` 也被静默升级为 3.5 Flash。PR 修复模型选择逻辑，保证用户固定版本不被自动改写。

### 5. #29215 — 核心：为不可信工具输出强制信封元数据来源
- 状态：`OPEN`
- 链接：[PR #29215](https://github.com/google-gemini/gemini-cli/pull/29215)

更新系统提示词：要求模型在处理外部工具/MCP 输出时，只依据“顶层信封属性”判断作者身份与运行状态，防止模型被嵌入在工具结果中的伪元数据欺骗。

### 6. #29216 — CLI：沙箱容器内隔离 `settings` 目录
- 状态：`OPEN`
- 链接：[PR #29216](https://github.com/google-gemini/gemini-cli/pull/29216)

之前在 Docker/Podman 沙箱中直接挂载宿主机 `~/.gemini`，存在 token/账号凭据泄漏风险。PR 改为使用干净的配置容器并做读写隔离，避免 OAuth 凭据进入沙箱。

### 7. #29214 — 沙箱：加固文件系统边界 + 隔离运行时状态
- 状态：`OPEN`
- 链接：[PR #29214](https://github.com/google-gemini/gemini-cli/pull/29214)

进一步将宿主机配置目录与沙箱 runtime state 脱钩：仅以只读方式挂载消毒后的配置文件，同时在路径敏感性检查中解析 Symlink，并解耦容器环境变量。

### 8. #29110 — 核心：`read_file` 改为经 FileSystemService 路由
- 状态：`OPEN`
- 链接：[PR #29110](https://github.com/google-gemini/gemini-cli/pull/29110)

当前 `read_file` 直接读本地磁盘，绕过了 `config.getFileSystemService()`；而 `write_file`/`replace` 都已走注入的 FS 服务。这会造成 ACP 客户端声明 `fs.readTextFile` 能力时，读取路径与覆盖路径策略不一致的安全缺口。

### 9. #29114 — 核心：修复 spawn 失败时 `handleExit` 重复执行
- 状态：`OPEN`
- 链接：[PR #29114](https://github.com/google-gemini/gemini-cli/pull/29114)

Node.js 在子进程 spawn 失败时会同时触发 `error` 和 `close` 事件，导致 `handleExit` 被执行两次。PR 加入重入保护标志，避免 Shell 执行服务状态被异常覆盖。

### 10. #29118 — 扩展：仅剥离末尾的 `.git` 后缀
- 状态：`OPEN`
- 链接：[PR #29118](https://github.com/google-gemini/gemini-cli/pull/29118)

修复 GitHub 扩展仓库解析：只移除作为“尾部后缀”的 `.git`，避免 `blog.github.io` 这类内部含 `.git` 的仓库名被误截断。匹配同时改为大小写不敏感。

## 功能需求趋势

从近期 Issue 中可以提炼出 5 个社区最关注的产品方向：

1. **Subagent 运行时可靠性与状态保真**
   大量 Issue 指向同一个目标：Agent 在 MAX_TURNS/中断/错误恢复时必须上报真实状态，不能伪装成 `GOAL` 成功；通用 Agent 挂起问题也需要可观测、可恢复。

2. **零依赖 OS 沙箱与“原生 Bash 友好”执行模型**
   `bash affinity` 被视为核心能力，但前提是完成文件系统路径边界、Symlink 解析、NTFS 短名、环境变量过滤、敏感目录隔离等一系列沙箱加固——这正好是这两天 PR 最密集的方向。

3. **AST 感知的代码读取与搜索**
   社区对上下文 Token 成本越来越敏感。AST 感知工具被认为是替代“firehose 式大文件读取”的关键路径，能减少误读、降低轮次、提升代码搜索精度。

4. **Auto Memory 的隐私、质量与可观测性**
   多个相关 Issue（[#26516](https://github.com/google-gemini/gemini-cli/issues/26516)、[#26522](https://github.com/google-gemini/gemini-cli/issues/26522)、[#26523](https://github.com/google-gemini/gemini-cli/issues/26523)、[#26525](https://github.com/google-gemini/gemini-cli/issues/26525)）集中关注：提取前脱敏、低价值会话重试抑制、无效 Patch 隔离、处理进度可视化。

5. **持久化任务跟踪取代 In-Context Todo**
   [#18836](https://github.com/google-gemini/gemini-cli/issues/18836) 提议废弃内存型 `WriteToDo`，改为基于文件系统的持久化 CRUD 任务跟踪，避免 Context Rot、Token 浪费和会话间记忆丢失。

## 开发者关注点

- **“假成功”比失败更可怕**：`MAX_TURNS` 被上报为 GOAL Success（[#22323](https://github.com/google-gemini/gemini-cli/issues/22323)）会让上层自动编排误判任务已完成，这在 CI/Agent 工作流中会造成隐蔽的连锁故障。
- **模型不会主动使用配置好的 Skills**：开发者投入精力编写自定义 Skill/Subagent，但模型在无显式指令时不调用（[#21968](https://github.com/google-gemini/gemini-cli/issues/21968)），自定义配置的 ROI 大打折扣。
- **Shell 执行状态机仍有 Hang 场景**：通用 Agent 无限挂起（[#21409](https://github.com/google-gemini/gemini-cli/issues/21409)）与 Shell 命令完成后卡在 “Waiting input”（[#25166](https://github.com/google-gemini/gemini-cli/issues/25166)）频繁打断真实开发流程。
- **第三方工具生态带来新的信任边界**：扩展/MCP 可注入环境变量（[PR #28863](https://github.com/google-gemini/gemini-cli/pull/28863)）、工具数量超限导致 400 错误（[#24246](https://github.com/google-gemini/gemini-cli/issues/24246)）、以及不可信工具输出伪造元数据（[PR #29215](https://github.com/google-gemini/gemini-cli/pull/29215)），都是 Agent 安全链条上的薄弱环节。
- **本地数据隐私保护意识增强**：Auto Memory 等后台服务读取 transcripts、可能记录 Skill 内容，开发者要求在执行前完成脱敏与最小化收集，而不是事后补救。
- **Linux 桌面体验仍有缺口**：Wayland 环境下的 Browser Subagent 失败（[#21983](https://github.com/google-gemini/gemini-cli/issues/21983)）说明跨平台 GUI/显示协议兼容性还有提升空间。

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI 社区动态日报 — 2026-09-05

## 今日速览

过去 24 小时内 GitHub Copilot CLI 连续发布 v1.0.84-0 与 v1.0.84-1 两个版本，其中 v1.0.84-1 正式加入 GPT-6 Astra 模型支持。社区侧，ACP 模式权限自动批准回归（#4537）、BYOK 提示缓存被静默禁用导致成本倍增（#4720）、自动更新破坏桌面应用会话（#4728）等问题密集发酵，安全与成本控制成为今日讨论焦点。此外，MCP 协议兼容性和稳定性问题仍在高频出现，多个新提交的 triage Issue 集中指向 MCP 工具调度超时与会话恢复后的异常行为。

## 版本发布

### v1.0.84-1
**Added**
- 新增 GPT-6 Astra 模型支持（[Release 页面](https://github.com/github/copilot-cli/releases)）

### v1.0.84-0
**Added**
- 托管沙箱会话现在可以从已批准的 bypass 提示中，选择在当前会话剩余时间内禁用沙箱。

**Fixed**
- 修复：沙箱阻止 PowerShell 写入时，现在会提供在沙箱外部运行该命令的选项。
- 修复：凭据存储中存在多个 GitHub 账号时，沙箱化 `gh` 命令的相关问题（原文未完整列出）。

### v1.0.83（2026-09-04）
- Windows 11 任务栏新增运行中 Copilot 会话展示，支持悬停实时状态卡片。
- 为 MCP OAuth 登录增加 Client ID Metadata Document (CIMD) 支持。
- 自定义 Agent 可在 `model` 字段中列出多个模型，按顺序尝试直到可用；`model-policy: required` 保持强制。

## 社区热点 Issues

以下从过去 24 小时更新的 37 条 Issue 中筛选 10 条最值得关注的内容：

### 1. ACP 模式自动批准工具调用回归，权限提示被静默跳过 — [#4537](https://github.com/github/copilot-cli/issues/4537)
- 作者：richardjv-msft | 更新：2026-09-04 | 👍 2
- 自 v1.0.81-1 起，`--acp` 模式不再发送 `session/request_permission`，Shell 命令、文件编辑与删除在无人值守状态下执行，是 #845 的回归。权限模型类问题影响面广，值得优先跟进。

### 2. BYOK 模式下提示缓存被静默禁用，成本约提升 5 倍 — [#4720](https://github.com/github/copilot-cli/issues/4720)
- 作者：Jianshui | 更新：2026-09-04
- v1.0.82 在 BYOK 模式下发送的请求不再携带 prompt-cache 声明，`cached_tokens=0`，导致整个会话上下文每次全量计费。这是成本敏感型用户必看的问题。

### 3. 自动更新重写自身 exe，导致 GitHub Copilot 桌面应用会话全部不可恢复 — [#4728](https://github.com/github/copilot-cli/issues/4728)
- 作者：doomslayer2k | 更新：2026-09-04
- CLI 的自动更新机制会改写其启动源 `copilot.exe`，间接破坏 GitHub Copilot 桌面应用，所有已有会话报“Session unavailable”。该问题揭示了 CLI 与桌面应用共享二进制时的更新冲突风险。

### 4. 空闲会话中 copilot-file-search 线程失控：单核占满 + 无界磁盘写入 — [#4710](https://github.com/github/copilot-cli/issues/4710)
- 作者：mmazur | 更新：2026-09-04
- v1.0.83-3 在 Ubuntu 24.04 上，会话状态为 idle 时 `copilot-file-search` 线程仍持续运行，占满一个 CPU 核心并无限写入 `~/.copilot` 日志。资源泄漏类问题对开发者本地体验影响显著。

### 5. 自定义 Agent 无法配置 reasoning effort — [#2904](https://github.com/github/copilot-cli/issues/2904)
- 作者：brian-kelley-intel | 更新：2026-09-04 | 评论 8 | 👍 23
- 自定义 `.agent.md` 目前只能通过 `model` 固定模型，但不能设置每个 Agent 的推理强度。这是当前 Issue 区点赞数最高的需求之一，社区对按 Agent/场景精细控制推理开销有强烈诉求。

### 6. 系统提示词固定 token 开销过大，建议支持裁剪与配置 — [#2627](https://github.com/github/copilot-cli/issues/2627)
- 作者：ronkeele | 更新：2026-09-04 | 评论 4 | 👍 19
- 开场系统提示词约消耗 20,500 tokens，加工具定义合计约 29,000 tokens，在 200K 上下文中占据近 15%。开发者希望对固定开销进行配置化精简，反映了对成本与上下文效率的敏感性。

### 7. WSL2 下 Ctrl+H 被误判为 Ctrl+Backspace — [#4328](https://github.com/github/copilot-cli/issues/4328)
- 作者：dimbleby | 更新：2026-09-04 | 评论 7
- 文档声明 `ctrl+h` 为“删除前一个字符”，但在 Windows Terminal + WSL2 环境中实际表现为删除整个前一个单词。环境相关输入处理 bug，Windows 用户受影响较广。

### 8. v1.0.81-1 MCP 初始化：成功 server/discover 后又发旧版 initialize，导致 -32022 — [#4525](https://github.com/github/copilot-cli/issues/4525)
- 作者：dmbutko | 更新：2026-09-04 | 评论 6 | 👍 3（已关闭）
- 针对 Python MCP SDK 2.0.0 双时代 runner，CLI 先发送新协议 `server/discover` 后又发送旧版 `initialize`，造成握手冲突。虽然已被关闭，但该 Issue 是 MCP 协议演进期兼容性阵痛的典型样本。

### 9. 高频崩溃：JavaScript heap out of memory — [#4725](https://github.com/github/copilot-cli/issues/4725)
- 作者：jbulow | 更新：2026-09-04
- CLI 每几分钟崩溃一次，Mark-Compact 阶段达到约 4GB 后 allocation failure。内存管理问题对长时间运行的自动化工作流影响极大。

### 10. 工具调用取消后 tools/list 刷新落入同一阻塞服务器，工具永久丢失 — [#4731](https://github.com/github/copilot-cli/issues/4731)
- 作者：tecrogue | 更新：2026-09-05（最新提交）
- 当 stdio MCP Server 工具调用超时后，运行时立即向同一仍阻塞中的服务器派发 `tools/list` 刷新，导致刷新超时，该服务器所有工具在此进程生命周期内被永久移除。MCP 调度容错设计存在系统性缺陷。

## 重要 PR 进展

过去 24 小时数据源中仅包含 1 条 Pull Request，暂无实质性的社区或官方代码合入动态：

### [#3771](https://github.com/github/copilot-cli/pull/3771) — Initial project setup（Open）
- 作者：limenpchuolto112-creator | 创建：2026-06-11 | 最近更新：2026-09-04
- 该 PR 仅包含项目初始化内容，作者账号疑似非官方贡献者，无评论与摘要，社区关注度低。
- 说明：今日发布的新版本（v1.0.84-0 / v1.0.84-1）均通过 Release 通道直接发布，Issue 区活跃度远高于 PR 区。

## 功能需求趋势

从近期 Issues 与 Release 动态来看，社区最关注的五个功能方向为：

1. **模型与 Agent 控制力进一步细化**
   - 诉求集中在为自定义 Agent 单独设置 reasoning effort（#2904）。
   - v1.0.84-1 的 GPT-6 Astra 支持与 v1.0.83 的多模型顺序 fallback，都表明官方正在加强模型选择灵活性。

2. **上下文窗口与成本治理**
   - 开发者要求固定系统提示词可配置、可裁剪（#232、#2627）。
   - 要求自动压缩阈值可调（#1688），并希望压缩时机与 prompt cache TTL（约 5 分钟）对齐，降低空闲后恢复回合的延迟与成本（#4724）。
   - BYOK 缓存失效问题（#4720）进一步强化了这一诉求。

3. **MCP 生态成熟化与治理**
   - 兼容性问题：旧版 `initialize` 与新协议并存（#4525）、chroma-mcp 兼容性受损（#4647）。
   - 可靠性问题：tools/list 刷新在超时场景下造成工具永久丢失（#4731）。
   - 治理需求：企业希望屏蔽内置 Agent Plugin Marketplace（#4715）。

4. **终端交互与桌面体验完善**
   - 输入行需要 Shift+Arrow/Ctrl+A 文本选择（#2644）。
   - 终端鼠标滚动在 Android Studio 中被误判为历史导航（#3194）。
   - 输出渲染中的滚动条会随选区被复制（#4707）。
   - 正向进展：v1.0.83 已将 Copilot 会话带入 Windows 11 任务栏。

5. **企业安全合规能力增强**
   - ACP 模式权限回归（#4537）引发对授权边界的讨论。
   - 用户希望接入“Trusted Access for Cyber”等安全审查项目（#4322）。
   - 企业需要更高的可控性，包括配置系统级提示词与管理内置市场来源（#4715）。

## 开发者关注点

当前开发者反馈中最集中的痛点与高频诉求可归纳为：

- **安全回归风险**：#4537 所示的 ACP 模式静默自动批准工具调用，可能造成高危操作无提示执行，被社区视为严重回归。
- **成本透明度**：#4720 中 BYOK 模式缓存静默失效，用户在无感知情况下成本上升约 5 倍，突显了关键计费行为缺少警示的问题。
- **本地资源占用**：#4710 的空闲 CPU/磁盘泄漏、#4725 的 JS 堆 OOM，直接影响开发者日常使用体验与自动化稳定性。
- **MCP Server 调度韧性不足**：单个超时工具调用可能导致整组工具在整个进程周期内不可用（#4731），对依赖 MCP 工具链的用户构成严重阻塞。
- **更新机制风险**：#4728 自动更新会覆写桌面应用所依赖的 `copilot.exe`，说明“更新自身”与“被宿主应用嵌入”两种运行模式存在冲突。
- **会话与插件生命周期问题**：#4590 中多个插件激活时，MCP host reload 会连带 dispose 会话的 hook processor；#4645 中 `session.resume` 静默忽略新传入的 `model` 参数，均反映出会话状态管理不够稳健。
- **输入与渲染的细节打磨**：WSL2 下 Ctrl+H 行为异常（#4328）、下划线开头内容被 Markdown 解析吞掉（#4722）、Android Studio 集成终端滚轮冲突（#3194）等一批体验级 bug，仍是高频反馈来源。

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

```markdown
# Kimi Code CLI 社区动态日报 – 2026-09-05

## 1. 今日速览

过去 24 小时，Kimi Code CLI 仓库整体动静较少：**无新版本发布**，有 1 个新 Issue 和 1 个 PR 更新。新 Issue 聚焦 Windows Terminal 下 `Ctrl+V` 粘贴失效的问题；PR 侧则在持续推动 `StrReplaceFile` 对“运行中内容”的替换计数修复。整体来看，社区议题重心仍集中在终端平台体验和代码编辑工具链的细节准确性上。

## 2. 版本发布

过去 24 小时内没有发现新的 Release，故本节从略。

## 3. 社区热点 Issues

> 说明：过去 24 小时内更新/创建的 Issue 仅 1 条，未达到 10 条的常规展示规模。以下为该时段唯一值得关注的 Issue。

### [#2634 [bug] kimi终端改键位不成功，比如粘贴](https://github.com/MoonshotAI/kimi-cli/issues/2634)
- **作者 / 时间**：PANG-GIT-AI 创建于 2026-09-04，最后更新于 2026-09-04
- **状态**：Open｜0 评论｜0 👍
- **摘要**：用户在 Kimi Code CLI v0.40.1、Windows Terminal + PowerShell 环境下无法通过 `Ctrl+V` 粘贴文本，并反馈修改终端键位设置后依然不生效。
- **为何重要**：粘贴是 CLI 交互中最基础、最高频的操作之一。该问题会直接阻塞 Windows 用户在长输入场景（如调用 k3 模型时）下的使用体验，属于平台适配层面的可用性缺陷。
- **社区反应**：刚提交不久，尚未形成讨论，也无维护者回复或复现确认，需要后续继续观察。

## 4. 重要 PR 进展

> 过去 24 小时内更新的 PR 仅 1 条，以下为全部条目。

### [#2524 fix(tools): count StrReplaceFile replacements against the running content](https://github.com/MoonshotAI/kimi-cli/pull/2524)
- **作者 / 时间**：Sreekant13 创建于 2026-07-20，最后更新于 2026-09-04
- **关联 Issue**：Resolves #2526
- **摘要**：修复 `StrReplaceFile` 在执行连续文本替换时，替换计数仍基于“原始文件内容”计算的问题。当链式编辑中的某个 `old` 字符串由前一次编辑产生时，它并不存在于原始内容中，导致计数与实际写入不一致。
- **重要性**：对 AI 编程助手而言，编辑结果可追踪、可计数非常重要。该 PR 将计数基准改为“当前运行中的内容”，可更真实地反馈工具对文件的修改次数，减少 Agent 在连续编辑场景下的状态误判。
- **当前进展**：PR 从 7 月 20 日创建延续至近日更新，说明贡献者或维护者仍在推动。若合入，将明显提升 `StrReplaceFile` 等文本编辑工具的行为透明度。

## 5. 功能需求趋势

过去 24 小时有效样本较少，趋势仅能作为观察信号：

- **Windows 终端适配**：反馈者使用 Windows Terminal + PowerShell，表明该环境下的键位与剪贴板集成是实际痛点。
- **键位自定义能力**：用户希望 CLI 的键位可以被修改，并能在 Windows 终端中原生覆盖默认快捷键。
- **文本编辑工具透明度**：PR #2524 反映出社区开始关注“代码编辑工具作用于动态内容时，反馈结果是否准确”，说明用户不仅关心编辑是否成功，更在意编辑过程的可计算性和可观测性。
- **新模型链路兼容**：Issue 中用户使用 `k3` 模型，未来可继续关注新模型在 CLI 登录、会话、平台适配方面是否有潜在兼容问题。

## 6. 开发者关注点

- **Windows 粘贴失效**：`Ctrl+V` 无法粘贴、键位修改不生效，是 Windows Terminal 用户当前最直接的使用痛点。
- **连续编辑的计数准确性**：开发者 PR 表明，链式编辑中“旧文本”可能存在时效性问题，工具应以运行中的最新内容为准进行匹配和计数，而不是机械地对照原始文件。
- **社区响应节奏**：过去 24 小时内 Issue 和 PR 均无新评论，GitHub 讨论热度偏低；目前还需要等待维护者对 Windows 键位问题及 #2524 的进一步反馈。
```

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode 社区动态日报 — 2026-09-05

## 今日速览

昨日连续发布 **v1.18.28** 与 **v1.18.29** 两个版本：前者加入 GitHub Copilot 会话追踪与桌面端体验修复，后者修复 Codex OAuth 模型过滤 bug。值得警惕的是，v1.18.28 被报告引入了 **远程 MCP 回归问题**（#47368），社区中关于图像读取、自动压缩循环、npm 插件安装超时等稳定性问题讨论活跃。此外，**Claude Code hooks 兼容性**（#12472）是当前获赞最高的开放 Issue，反映了社区对跨工具生态兼容的强烈诉求。

---

## 版本发布

### v1.18.29
**核心修复：**
- 修复 Codex OAuth 模型过滤无法识别整数 GPT 版本（如 `gpt-6`）的问题
- 修复 `gpt-6-astra` 未对 OpenAI 订阅用户显示的问题
- 感谢 2 位社区贡献者，其中 @Peter267 修复了中文文档中粗体渲染缺少空格的问题
> https://github.com/anomalyco/opencode/releases/tag/v1.18.29

### v1.18.28
**核心改进：**
- 将会话 ID 作为 GitHub Copilot 的 interaction header 发送，改善跨会话请求追踪
**桌面端修复：**
- OpenCode 账户设备认证改用桌面客户端 ID
- 增大“在应用中打开”图标尺寸，提升可见性
> https://github.com/anomalyco/opencode/releases/tag/v1.18.28

---

## 社区热点 Issues

### 1. 原生 Claude Code hooks 兼容性（PreToolUse、PostToolUse、Stop）
**#12472 · 开放 · 👍 40 · 💬 19**
社区最高赞需求。OpenCode 已兼容 Claude Code 的规则、技能与环境变量禁用机制，但 hooks 系统尚未支持，期望在 `~/.claude/settings.json` 层面实现深度互通。
> https://github.com/anomalyco/opencode/issues/12472

### 2. Ollama 本地模型在 Desktop 中返回无效 JSON
**#19948 · 已关闭 · 👍 5 · 💬 23**
Windows 桌面版配置 Ollama 本地模型后，模型虽出现在列表中，但持续返回无效 JSON 响应（工具调用格式错误）。本地模型场景关注度高。
> https://github.com/anomalyco/opencode/issues/19948

### 3. 图像读取功能回归——Bad Request 错误
**#25832 · 已关闭 · 👍 5 · 💬 18**
4 月 29 日前可正常读取 PNG/JPG 并据此修改 HTML，但 5 月 5 日起报 `Bad Request` 错误，疑似模型图像输入链路被意外破坏。
> https://github.com/anomalyco/opencode/issues/25832

### 4. 自动压缩死循环，模型停止响应
**#30680 · 已关闭 · 👍 0 · 💬 17**
即使在新空目录下启动，OpenCode 也反复自动压缩并消耗 token，最终完全停止输出。对日常会话可用性影响严重。
> https://github.com/anomalyco/opencode/issues/30680

### 5. bad gateway 错误与循环
**#35148 · 已关闭 · 👍 13 · 💬 9**
OpenCode Desktop v1.16.2 持续报 bad gateway 并陷入循环，Go 插件环境下复现。引发较多用户共鸣，👍 数达 13。
> https://github.com/anomalyco/opencode/issues/35148

### 6. 默认共享应改为“禁用”——隐私优先
**#17188 · 已关闭 · 👍 13 · 💬 5**
建议将默认分享行为设为关闭，强调用户知情同意。关联 #7982、#459，隐私保护是社区持续关注的方向。
> https://github.com/anomalyco/opencode/issues/17188

### 7. 插件安装器拉取 npm 公共依赖超时
**#44684 · 开放 · 💬 5**
1.18.21 起插件安装偶发超时（`NpmInstallFailedError`），导致插件静默失效或启动挂起，headless 模式（`run`/`serve`）必现。上游 PR #47430 正在解决。
> https://github.com/anomalyco/opencode/issues/44684

### 8. 仪表盘整体用量百分比计算错误
**#47142 · 开放 · 💬 4**
“Total” 百分比直接对各模型百分比求和（61.2% + 17.4% + 8.4%… = 88.7%），但各模型配额上限不同（$30/$15/$60），求和法产生人为偏差。
> https://github.com/anomalyco/opencode/issues/47142

### 9. 远程 MCP 回归——KitWright 工具不可用
**#47368 · 已关闭 · 💬 3**
1.18.27 → 1.18.28 升级后，原本正常的远程 MCP server（KitWright/Unity，127.0.0.1:9155）无法连接，疑为 1.18.28 引入的回归。
> https://github.com/anomalyco/opencode/issues/47368

### 10. Desktop 无法正常启动（持续崩溃）
**#35549 · 已关闭 · 💬 4**
启动即崩溃，与消息日志读取能力不足及默认打开最近项目的行为有关，重装新版仍复现。
> https://github.com/anomalyco/opencode/issues/35549

---

## 重要 PR 进展

### 1. feat(ai)：通过 AWS 默认链解析 Bedrock 凭据
**#47436 · 开放**
原生 Bedrock 路由目前只接受静态密钥或 bearer token，本 PR 增加对 `AWS_PROFILE`、`~/.aws` 共享配置、SSO 缓存及实例元数据的完整支持。
> https://github.com/anomalyco/opencode/pull/47436

### 2. feat(plugin)：暴露会话表单、会话列表与全局事件流
**#46690 · 开放**
为 Telegram bot 等插件场景扩展 API 能力——插件将能读取会话表单、列出会话，并订阅全局事件流。
> https://github.com/anomalyco/opencode/pull/46690

### 3. fix(core)：为 npm 安装设置可配置超时
**#47430 · 开放**
`Npm.reify()` 此前无超时等待 arborist，网络异常时插件安装无限挂起。本 PR 引入可配置超时，预期直接修复 #44684、#31463 等一组插件安装问题。
> https://github.com/anomalyco/opencode/pull/47430

### 4. fix(session)：停止对免费额度和 Go 用量配额的重试
**#47339 · 开放**
免费 Zen 模型返回 `FreeUsageLimitError` 时带长 `retry-after`，会话重试策略将其视作普通错误而持续重试。本 PR 予以纠正，关闭 #47318。
> https://github.com/anomalyco/opencode/pull/47339

### 5. feat(core)：支持 provider OAuth client credentials
**#47423 · 开放**
为配置的 provider 增加可选的 OAuth `client_credentials` 认证（Basic 或 POST），token 仅存内存、到期自动续期，失败重试一次。无需浏览器或 `/connect` 流程。
> https://github.com/anomalyco/opencode/pull/47423

### 6. fix(core)：同仓库多个克隆应视为不同项目
**#35311 · 开放**
一个存在已久的大型修复 PR，关闭 15 个相关 Issue。核心变更大概率涉及项目 ID 标识逻辑，从而让同一仓库的不同克隆各自独立管理会话和状态。
> https://github.com/anomalyco/opencode/pull/35311

### 7. fix(app)：延迟后台工作区发现
**#47428 · 开放**
不再急切发现历史项目的工作树与 MCP 目录，仅在挂载会话或打开工作区选择器时按需加载，并引入服务端作用域缓存，减少启动开销。
> https://github.com/anomalyco/opencode/pull/47428

### 8. fix(lsp)：空闲 TTL 超时与 LRU 驱逐策略
**#47392 · 已关闭**
为 LSP 客户端增加空闲超时与 LRU 驱逐机制，防止无界增长并自动清理空闲实例。
> https://github.com/anomalyco/opencode/pull/47392

### 9. perf(plugin)：并行加载内部插件
**#47391 · 已关闭**
通过 `Effect.forEach` 实现无界并发，将内部插件加载并行化，加速插件初始化流程。
> https://github.com/anomalyco/opencode/pull/47391

### 10. fix(desktop)：防止大文本粘贴导致崩溃
**#47427 · 开放**
在 Windows 上复现的大段文本粘贴会让桌面端 UI 卡顿甚至崩溃，本 PR 修复此问题，关闭 #47425。
> https://github.com/anomalyco/opencode/pull/47427

---

## 功能需求趋势

从过去 24 小时活跃的 Issue/PR 来看，社区关注方向集中在以下几类：

- **Claude Code 生态兼容**：#12472 获 40 👍，hooks 系统（PreToolUse/PostToolUse/Stop）兼容诉求强烈；另有代理 Markdown 变体保留修复（#47414）等配套工作。
- **本地与私有化模型支持**：Ollama 本地 JSON 响应异常（#19948）、MiniMax-M3 经代理不支持图像（#34596）、Bedrock AWS 默认链（#47436）等，说明开发者在大量尝试非 OpenAI 模型接入。
- **插件系统能力扩展**：包括会话表单/事件流暴露（#46690）、自动导入 @include 文件（#35567）、URL 白名单（#35565）、finalization hook（#35540）等，插件化生态需求进入深水区。
- **可观测性与用量统计**：#47142 用量百分比算法不合理、#47342 OpenAI usage 归一化修复，提示多模型/多配额场景的精细化计费展示成为痛点。
- **资源治理与稳定性**：npm 安装超时（#44684）、LSP 空闲清理（#47392）、并行插件加载（#47391）共同指向长期运行场景下的稳定性与资源占用优化。

---

## 开发者关注点

- **回归问题频发**：1.18.28 引入远程 MCP 回归（#47368）；此前版本还出现过图像读取（#25832）、自动压缩死循环（#30680）等回归。用户对发版质量与自动化回归测试的期望在提高。
- **“隐式重试”消耗预算**：免费/Go 配额被无限重试（#47339），加上 auto-compaction 死循环（#30680），开发者对“无谓消耗 token/配额”非常敏感。
- **网络与依赖安装韧性不足**：npm registry 超时导致插件静默失效或启动挂起（#44684），在 CI/headless 环境下尤为致命。
- **权限与隐私意识增强**：默认共享数据需用户主动关闭（#17188）获 13 👍；支持按用户 provider 偏好过滤模型（#35506）也反映了对数据边界控制的诉求。
- **终端与桌面端体验细节**：SSH 连接后终端渲染损坏（#35541）、Cmd+B 快捷键失效（#35575）、大文本粘贴崩溃（#47427）等 UI/UX 问题反馈密度较高，说明桌面端已进入体验打磨阶段。

---

> 日报数据来源：github.com/anomalyco/opencode 社区公开数据（2026-09-04 至 2026-09-05）

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi 社区动态日报 — 2026-09-05

## 今日速览

v0.85.0 发布并带来「Claude 思考强度持久化」新特性，但该版本的打包缺陷（`@earendil-works/pi-server` 未声明依赖）引发多条重复 Issue 并被快速修复。社区最热讨论集中在 Amazon Bedrock Mantle Provider 支持、macOS 长会话高 CPU 占用，以及多起上下文内容被静默丢弃 / 会话被异常请求“锁死”的稳定性问题。

## 版本发布

**v0.85.0** — 新特性：**Persistent Claude thinking effort（持久的 Claude 思考强度）**。受支持的 Anthropic transports 现在会保留每轮的 `thinking effort` 设置，并能在 signed-thinking 不匹配时安全恢复。详见 [Model Configuration](https://github.com/earendil-works/pi/blob/v0.85.0/packages/coding-agent/docs/models.md#model-configuration)。

## 社区热点 Issues

1. **[#5363 Add amazon-bedrock-mantle provider（18 评论 / 15 👍）](https://github.com/earendil-works/pi/issues/5363)**
   最受关注的进行中功能请求：现有 `amazon-bedrock` 走 Converse API，而 Bedrock Mantle 模型使用 OpenAI 兼容接口（`/openai/v1/responses`），两者不兼容，需新增独立 provider。社区需求度高，已进入 in-progress。

2. **[#7730 macOS 长会话高 CPU（15 评论 / 10 👍）](https://github.com/earendil-works/pi/issues/7730)**
   在 macOS 上运行 pi 时 CPU 在 50–110% 间摆动，内存 600–800MB，疑似与会话/上下文长度相关。属高频性能 bug，反馈者多，等待定位。

3. **[#9132 0.85.0 打包缺陷：dist/cli.js 静态导入未声明的 pi-server（4 评论 / 5 👍）](https://github.com/earendil-works/pi/issues/9132)**
   已关闭但影响面大：`@earendil-works/pi-coding-agent@0.85.0` 的 npm 包内置 `dist/cli.js` 顶层静态导入 `@earendil-works/pi-server`，而该依赖未声明，导致全新安装即报 `ERR_MODULE_NOT_FOUND`。与 #9140、#9156、#9158 为同一问题的重复报告——说明有较多用户受影响。

4. **[#8896 /export HTML 静默丢弃已发送给模型的上下文（6 评论）](https://github.com/earendil-works/pi/issues/8896)**
   `display: false` 被文档标注为“仅 TUI 隐藏用”，但 `/export` 的 HTML 输出也据此丢弃了这些自定义消息，导致导出的会话记录缺失实际发给模型的内容，存在上下文不一致风险。

5. **[#8760 OpenRouter :free 模型全部 400 失败（5 评论）](https://github.com/earendil-works/pi/issues/8760)**
   Pi 以模型目录的 `maxOutputTokens` 作为 `max_tokens` 发出请求，超过上游免费模型的硬性限制，导致多个 `:free` 模型不可用。反映“目录元数据与上游实际限制脱节”的一类问题。

6. **[#5593 Tab 补全斜杠命令时尾随空格阻塞参数补全（7 评论）](https://github.com/earendil-works/pi/issues/5593)**
   进行中的交互 bug：Tab 补全 `/sb-l` → `/sb-list ` 会在末尾插入空格，用户再按 Space 反而无法触发参数/选项自动补全。典型的高频输入体验问题。

7. **[#9052 全屏模式滚轮滚动比普通模式慢 3 倍（5 评论 / 2 👍）](https://github.com/earendil-works/pi/issues/9052)**
   TUI 全屏模式固定输入框很好用，但滚轮滚动速度显著变慢。提出问题的用户给出了明确的复现对比，已有对应 PR 修复。

8. **[#8720 空白 tool 输出永久锁死会话（4 评论）](https://github.com/earendil-works/pi/issues/8720)**
   工具返回纯空白内容（如 Windows bash 输出 `"\r\n"`）时被原样发给 provider，OpenAI 兼容接口以 HTTP 400 拒绝；坏消息残留在历史中导致后续每次请求都失败，会话无法恢复。严重稳定性缺陷。

9. **[#8684 PI_OFFLINE 静默禁用所有 provider 模型发现（4 评论）](https://github.com/earendil-works/pi/issues/8684)**
   文档声明 `PI_OFFLINE` 只关闭启动期的联网运维操作，但实际连整个会话的模型目录发现也一并禁用，且无日志提示。属“文档/行为不一致”的坑。

10. **[#9073 JsonlSessionRepo 在 cwd 编码冲突时拒绝会话 ID（2 评论）](https://github.com/earendil-works/pi/issues/9073)**
    `tenant-a/project` 与 `tenant/a-project` 两种 cwd 映射到同一会话目录，后创建的同名会话相互冲突。体现路径编码方案在极端命名下的边界缺陷。

## 重要 PR 进展

1. **[#9170 fix(coding-agent): declare pi-server runtime dependency](https://github.com/earendil-works/pi/pull/9170)**
   修复 0.85.0 打包回归：为 `@earendil-works/pi-coding-agent` 声明 `@earendil-works/pi-server` 运行时依赖，解决全新安装后包根无法 import 的问题。

2. **[#9172 fix(coding-agent): prevent broken package root publication](https://github.com/earendil-works/pi/pull/9172)**
   依赖 #9170 的后续防线：增加发布前校验机制，防止“包根静态导入未声明依赖”这类缺陷再次流入 npm。

3. **[#9179 fix(coding-agent): reject tree navigation during compaction](https://github.com/earendil-works/pi/pull/9179)**
   修复 compact 期间的树导航竞态：在压缩或另一树导航进行中时拒绝新的树导航，并保证压缩结果与保留上下文绑定在正确的分支上；附两则回归测试。

4. **[#9166 feat(tui): accelerate Alt-modified wheel scrolling](https://github.com/earendil-works/pi/pull/9166)**
   按住 Alt 时滚轮以 5 倍速滚动，直接解决 #9052 全屏模式滚动过慢的问题，改动很小。

5. **[#9163 feat(tui): Simplify clipboard handling](https://github.com/earendil-works/pi/pull/9163)**
   mitsuhiko 提交：简化剪贴板处理——原 Rust 库对所需原生能力来说过于重，替换后有望改善 NixOS 等环境构建体验。

6. **[#9096 feat(ai,coding-agent): add Meta provider with Muse subscription OAuth](https://github.com/earendil-works/pi/pull/9096)**
   新增 Meta provider（解决 #7543），支持 Muse 订阅 OAuth。作者指出两个 quirks：refresh token 机制特殊（每日基于 identity token 重铸）、当前流式实为整段 burst，中等输出规模下才可用。

7. **[#9116 feat(ai): add mid-conversation system messages](https://github.com/earendil-works/pi/pull/9116)**
   #8998 拆分后的第一层：在 pi-ai 引入“会话中段的 system 消息”能力（如扩展中途修改 prompt 时），并保证 agent-core 与 coding-agent 不破坏地透传新角色。

8. **[#9117 feat(coding-agent): deliver prompt and tool changes as system message deltas](https://github.com/earendil-works/pi/pull/9117)**
   #8998 拆分后的第二层（基于 #9116）：把 prompt / 工具集合的变更改为增量 system delta 下发，替代原先重写顶层 system prompt 的做法，以降低 token 浪费。

9. **[#9164 fix(coding-agent): preserve model selector selection after refresh](https://github.com/earendil-works/pi/pull/9164)**
   修复 `/model` 选择器在后台刷新模型目录期间若用户移动光标，刷新完成时选中项被重置的问题，提升交互顺滑度。

10. **[#9149 fix(coding-agent): selector save keybindings](https://github.com/earendil-works/pi/pull/9149)**
    `/model` 与 `/thinking` 选择器改为读取 `app.models.save` / 新增的 `app.thinking.save` 绑定，不再硬编码 `Ctrl+S`，并对齐各选择器的快捷键文案显示——直接修复 #8797。

## 功能需求趋势

- **更多 Provider 与模型接入**：除进行中的 Amazon Bedrock Mantle 外，社区还提交了 Meta（Muse OAuth）、OrcaRouter 等新 provider，加上 OpenRouter `:free` 兼容性修复——对“把 Pi 接到更多模型网关”的需求明显且持续。
- **发布质量与依赖可信度**：0.85.0 的 `pi-server` 未声明依赖在一个晚上被 4 个用户重复上报，促使“打包根修复 + 防复发校验”两个 PR 快速跟进，反映用户对发布管道稳定性的敏感度很高。
- **上下文完整性 / 会话安全**：`/export` 丢上下文、空白 tool 输出导致全局 400、cwd 编码冲突、compaction 期间导航竞态……多起事件指向同一方向：任何环节“静默丢失或污染历史消息”都会带来连锁故障。
- **TUI 交互细节打磨**：全屏滚动加速、Tab 补全空格、macOS `Cmd+V` 粘贴图片（#9138）、会话树搜索光标（#9157）等大量小修小补，说明核心功能稳定后社区正集中改善终端交互体验。
- **扩展 API 可定制性**：tool 执行前 final hook（#9175）、hidden thinking label 作用域（#9161）、keybinding 全面生效（#9149）——扩展作者们希望获得更细粒度的拦截与展示控制力。

## 开发者关注点

- **“装完即坏”最伤信任**：`@earendil-works/pi-coding-agent@0.85.0` 因未声明 `pi-server` 依赖导致全新安装直接启动失败，当晚即出现 4 条重复 Issue 和多条外部 CI 失败报告，开发者对这类可避免的发布回归容忍度极低。
- **Provider 适配的“边角坑”反复出现**：OpenRouter `:free` 的 `max_tokens` 超限、Anthropic 拒绝 per-message `output_config`、Bedrock Mantle 与 Converse 不兼容——第三方接口的细微差异正持续消耗用户调试成本。
- **会话被“毒死”后无逃生通道**：空白 tool 输出一旦进入历史，后续所有请求都 400；工具调用无执行超时（#8857）时则可能无限卡死。高频诉求是“坏消息可剔除 + 阶段超时兜底”。
- **长会话性能焦虑**：macOS 高 CPU 伴随 600–800MB 内存的反馈获得 15 条评论共鸣，上下文增长与资源消耗的关系是重度用户最关心的稳定性指标。
- **静默行为最隐蔽**：`PI_OFFLINE` 超出了文档声明的影响范围、`/export` 悄悄丢弃（视觉上只是“TUI 隐藏”的）内容、bash 工具静默丢弃 `cwd` 参数（#5904）——多个问题都指向“不报错也不生效”会极大增加排查成本，开发者希望这类场景要么明确报错、要么明确记录日志。

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code 社区动态日报（2026-09-05）

## 今日速览

今日社区聚焦于**稳定性与数据一致性修复**：Cerebras 等多轮对话 400 错误的根因（`reasoning_content` 字段未剥离）已在 PR #11049 中修复；HTML 导出体积高达 19.5 MB 的架构问题也通过 #11035（从 CDN 加载渲染器）得到根治。与此同时，Daemon 会话数据一致性（promptId 缺失）与生命周期管理（孤儿 worker、通道删除）成为新晋讨论热点。

---

## 社区热点 Issues

### 1. 迁移 TUI 渲染层：从 ink 到 OpenTUI（跟踪 Issue）
- **编号**: [#8662](https://github.com/QwenLM/qwen-code/issues/8662) | P3 · enhancement · tracking
- **为什么重要**: qwen-code 当前基于 ink 7 深度定制的渲染层（约 1037 行补丁）存在难以修复的结构性闪烁问题。社区以 30 条评论持续跟进该迁移，涉及渲染、交互、终端 UX 等多个 roadmap 维度。
- **社区反应**: 被标记为多个子问题（如 #10905 OpenTUI 斜杠命令输出丢失）的跟踪父任务，开发者关注度高。

### 2. CI 测试时间被模块导入耗时拖累
- **编号**: [#10908](https://github.com/QwenLM/qwen-code/issues/10908) | P2 · enhancement · performance
- **为什么重要**: 最新 CI 运行显示 `cli` workspace 的**测试收集耗时（2223s）远超实际测试运行时间（1372s）**，说明模块导入成本已成为发布效率的主要瓶颈。
- **社区反应**: 8 条评论讨论优化方向，包括直接导入模块路径（对应 PR #10957）。

### 3. 语音听写无法使用 Token Plan ASR 模型
- **编号**: [#10932](https://github.com/QwenLM/qwen-code/issues/10932) | P2 · bug · voice
- **为什么重要**: Model Studio 的 Token Plan 已上线 `qwen-audio-3.0-asr-flash`，但 Qwen Code 的语音管道仍硬编码旧模型 ID，导致 Token Plan 的 ASR 完全不可用。麦克风本身工作正常，只是模型 ID 白名单将其拒绝。
- **社区反应**: 5 条评论，用户希望尽快解除硬编码限制或改为可配置模型列表。

### 4. 新增可插拔中间件：语言感知的思考输出改写
- **编号**: [#10872](https://github.com/QwenLM/qwen-code/issues/10872) | P2 · feature-request
- **为什么重要**: 用户希望提供公开的中间件 API，在输出到达客户端前转换 thinking/reasoning 内容，典型场景是**将思考过程翻译成用户目标语言**（与 #3787 相关但范围更广）。
- **社区反应**: 4 条评论，社区呼吁同时支持交互式 CLI 和 `qwen serve` 守护进程会话。

### 5. `/export html` 文件臃肿：19.5 MB 的空会话导出
- **编号**: [#11031](https://github.com/QwenLM/qwen-code/issues/11031) | P1 · bug · performance
- **为什么重要**: 当前导出架构将整个 Web Shell 运行时（React 依赖图）嵌入每个 HTML 文件，**即使空会话也产生约 19.5 MB 的文件**。这是 P1 级别的功能缺陷。
- **社区反应**: 3 条评论，已有两项 PR（#11035、#11049）在紧急跟进修复。

### 6. Cerebras（OpenAI 兼容）多轮请求全部失败
- **编号**: [#11045](https://github.com/QwenLM/qwen-code/issues/11045) | P1 · bug · integration
- **为什么重要**: 使用 Cerebras 托管的任何模型时，首轮成功但**后续所有轮次均报 400 错误**，反馈在请求中携带了 `reasoning_content` 字段被上游拒绝。
- **社区反应**: 3 条评论，已被标记 P1，修复 PR #11049 在数小时内提交。

### 7. Daemon 转录缺少 promptId —— 活动轮无法可靠对齐
- **编号**: [#11060](https://github.com/QwenLM/qwen-code/issues/11060) | P2 · bug · daemon
- **为什么重要**: Daemon 已为每次 prompt 生成了稳定的 promptId，但 JSONL 和 `/transcript` 在轮次完成前都不暴露该 ID，导致集成方在活动轮刷新/重连时无法确定 canonical 转录身份。
- **社区反应**: 2 条评论，对应修复 PR #11062 已于同日提交。

### 8. 通道删除 Leave Orphaned Workers —— 配置缺失时无法收敛
- **编号**: [#11063](https://github.com/QwenLM/qwen-code/issues/11063) | P2 · bug · daemon
- **为什么重要**: 当持久化配置不存在时，显式的 Channel DELETE 直接返回 `channel_instance_not_found`，即使守护进程仍持有该 Channel 的 worker。**重复删除无法收敛孤儿进程**，存在资源泄漏。
- **社区反应**: 2 条评论，属生命周期管理盲区，与 #11024 的 worktree 清理问题相互印证。

### 9. DingTalk 通道将 clientSecret 明文打印到 stdout
- **编号**: [#10936](https://github.com/QwenLM/qwen-code/issues/10936) | P1 · bug · security · 已关闭
- **为什么重要**: `qwen channel start` 每次连接 DingTalk 通道时，会将客户端凭证（含 `clientSecret`）和 stream ticket 以明文打印到终端。安全问题严重但已关闭（修复已合入）。
- **社区反应**: 3 条评论，用户认可修复及时，同时建议审计其他通道的日志行为。

### 10. ACP 模式下思考语言与用户语言不一致
- **编号**: [#3787](https://github.com/QwenLM/qwen-code/issues/3787) | bug · needs-triage（已长期开放）
- **为什么重要**: 模型回复语言符合用户要求，但 **thinking 过程始终输出英文**，即使用户明确要求使用中文。该问题自 2026-05 提出，至今仍在 triage，社区持续施加压力。
- **社区反应**: 2 条新评论，关联 #10872（中间件方案），用户期待统一解决。

---

## 重要 PR 进展

### 1. 修复 Cerebras 请求携带 reasoning_content
- **编号**: [#11049](https://github.com/QwenLM/qwen-code/pull/11049) | fix(core)
- **内容**: 为 Cerebras 增加 provider 配置，在出站边界移除 `messages[].reasoning_content` 字段（与 Mistral 同策略），按 hostname `api.cerebras.ai` 自动识别。直接修复 #11045。

### 2. HTML 导出改为从 unpkg 加载渲染器
- **编号**: [#11035](https://github.com/QwenLM/qwen-code/pull/11035) | fix(export)
- **内容**: 导出文件仅保留转录数据与样式，Web Shell 渲染器（含 React/ReactDOM）打包为单一浏览器脚本并从 npm/CDN 加载。**目标将 19.5 MB 降为 KB 级**。同时关闭 #11031 的完整运行时嵌入方案。

### 3. 持久化 Daemon promptId 到转录记录
- **编号**: [#11062](https://github.com/QwenLM/qwen-code/pull/11062) | fix(daemon)
- **内容**: 在初始用户记录上持久化 `_meta.promptId` 并暴露于转录投影，使嵌入式客户端无需猜测即可关联 live-journal 与 canonical history。解决 #11060。

### 4. 合并并发 Config.initialize() 调用
- **编号**: [#11037](https://github.com/QwenLM/qwen-code/pull/11037) | fix(core)
- **内容**: 修复 `Config.initialize()` 的竞态：第一个调用完成前，第二个调用会被错误拒绝。现在将共享同一个初始化结果，消除高并发场景下的偶发错误。

### 5. CLI 直接导入核心模块（性能优化）
- **编号**: [#10957](https://github.com/QwenLM/qwen-code/pull/10957) | perf(cli)
- **内容**: 为 CLI 测试运行器增加 resolver 映射，将包根导入改为直接导入 `core` 子路径。目标是**削减 issue #10908 中 2223s 的测试收集时间**。已替换 #10946 和 #10956。

### 6. 添加 `qwen --bg` 后台 Agent View 会话
- **编号**: [#10943](https://github.com/QwenLM/qwen-code/pull/10943) | feat(cli)
- **内容**: 允许用户通过 `--bg "<prompt>"` 启动后台 Agent View 会话并立即返回（打印 session id），会话在 shell 退出后继续运行。此前 review 提出 argv 扫描需重构，该问题已记录在 #11065。

### 7. 解耦扩展激活刷新机制
- **编号**: [#10991](https://github.com/QwenLM/qwen-code/pull/10991) | refactor(daemon)
- **内容**: 扩展激活操作在策略持久化提交后才完成，不再直接刷新所有活动会话。新增 `extension_activation_explicit_refresh` capability，让客户端区分新旧 daemon 契约。

### 8. Session Rotation —— 按轮次/时长轮换通道会话
- **编号**: [#8927](https://github.com/QwenLM/qwen-code/pull/8927) | feat(channels)
- **内容**: 为通道增加 `sessionRotation` 配置（支持 `maxTurns` 和 `maxAge`），当前会话超限后，下一条消息自动创建新会话。在渠道集成场景下防止上下文无限膨胀。

### 9. 无头全局 Turn 导航（Web Shell 数据层）
- **编号**: [#11054](https://github.com/QwenLM/qwen-code/pull/11054) | feat(web-shell)
- **内容**: 新增 session-wide turn 导航的 Phase 2A 无头数据层：有界 turn-index 缓存、不可变历史分页、exact live/persisted turn locator，并为后续 UI 提供公开 React hooks。属于 Session Workflow 导航能力的一部分（#10938 的底层支持）。

### 10. Web Shell 会话工作流 DAG 导航与界面精简
- **编号**: [#10938](https://github.com/QwenLM/qwen-code/pull/10938) | feat(web-shell)
- **内容**: 优化 Session Workflow 的 plan DAG 展示（步骤而非状态优先），精简 inspector chrome，补齐 #8583 遗留的导航与文档缺口。与 #11054 配合形成完整的会话导航体验。

---

## 功能需求趋势

### 1. 渲染架构现代化
- **OpenTUI 全面替换 ink**（#8662 跟踪）：社区持续推动底层重构，以根除闪烁与 CLI 渲染不一致问题。已衍生多个子 issue（#10905 OpenTUI 斜杠命令输出丢失等）。

### 2. HTML 导出与数据可移植性
- **导出文件瘦身**（#11031、#11035）：从“文件自包含”转向“内容 + CDN 依赖”，保证可移植性的同时避免体积爆炸。

### 3. 多模型与提供商适配
- **新模型 ID 硬编码问题**集中爆发：ASR 旧 ID（#10932）、Cerebras 不支持 `reasoning_content` 输入（#11045）。社区期望 provider 配置层更灵活、无需发版即可支持新模型。

### 4. 会话管理与后台自动化
- **后台运行**：`--bg` 后台 Agent View（#10943）表明用户需要“派发任务后不阻塞终端”的工作流。
- **带身份的活动轮回放**（#11060、#11062）：集成方需要可靠的会话转录对齐能力。
- **Quick Chat 浮动面板**（#11017）：在 Web Shell 中不离开当前任务即可发起独立会话。
- **会话轮换策略**（#8927）：为长期运行的渠道绑定增加新鲜度上限。

### 5. 可插拔中间件与内容管道
- **thinking 输出改写**（#10872）：需要公开中间件 API，支持翻译、过滤或格式化推理过程。与 ACP/thinking 语言问题（#3787）相关联。

### 6. 进程级配置隔离
- **`--config-dir`**（#10984）：用户希望单个进程可指定完整配置根（相当于进程级 QWEN_HOME），而非只切换某个 session 配置。

### 7. 动态工作流与 Claude Code 对齐
- #11013 明确列出与 Claude Code 2.1.260 的差距清单（contract、entry/budget、resilience、distribution），显示社区对 sub-agents 并行执行、资源配额等能力的规模化诉求。

---

## 开发者关注点

### 痛点 1：测试与 CI 基础设施可靠性
- 模块导入成本**占优**于实际测试时间（#10908：2223s collect vs 1372s test）。
- 主分支 E2E 频繁失败（#11002、#11010、#11027、#11043、#11058、#11059、#11061 等多条机器人报告），多与 PR 合并前未充分验证相关。
- E2E 测试依赖绝对路径模式（#11066），非 `qwen*` 命名的 checkout 会导致 SIGKILL 测试失效。

### 痛点 2：达蒙（Daemon）生命周期与孤儿进程
- #11063：通道配置删除后 worker 仍存活且无法收敛。
- #11024：worktree 会话缺少孤儿回收机制，part 4A review 的残留项目尚无交付计划。
- 问题集中在**配置与运行时状态不一致**时系统缺乏自愈能力。

### 痛点 3：安全与凭据处理
- #10936（DingTalk 凭据明文输出）虽已关闭，但暴露了通道类集成日志的共性问题。
- #11019：AUTO 模式下用户多次确认仍被阻止，审批结果无法到达分类器，权限边界存在逻辑缺陷。

### 痛点 4：上游模型/平台的兼容性维护
- 多 provider（Cerebras、Model Studio ASR）的快速迭代使 Qwen Code 的硬编码频繁过时，**需要更健壮的 provider 抽象层**。

### 痛点 5：文档与本地开发环境
- #11055：Windows cmd 下 docs-site 本地预览流程两处受阻（符号链接权限和命令行兼容性），反映跨平台开发体验有待系统优化。

### 痛点 6：历史遗留 UI 问题等待 OpenTUI 迁移统一解决
- 单元格 Cmd+A 全选页面（#10702）、OpenTUI 斜杠命令输出丢失（#10905）等 UI 级 bug 长期处于 triage 状态，社区期望渲染层迁移完成后成批修复。

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI 社区动态日报 — 2026-09-05

> 数据来源：github.com/Hmbown/DeepSeek-TUI（说明：数据快照内部分 Issue/PR 链接显示为 Codewhale，与 DeepSeek-TUI 项目同源）。

## 1. 今日速览

昨日没有新版本发布，但社区修复与底层优化非常活跃：一个修复 Todo 列表快照污染会话历史的 PR（#5873）已合并关闭，对应的 Issue #5871 随之解决；另一个针对 Ollama 本地 32K 模型输入预算被错误压缩至 1024 tokens 的修复 PR（#5883）已提交，正在解决 Windows 用户面临的实际痛点。此外，维护者恢复了贡献者 CI 基线（#5882），降低外部 PR 的评估门槛。

## 2. 版本发布

过去 24 小时无新 Release。

## 3. 社区热点 Issues

> 数据源共 5 条，全部列出。

### #5820 [OPEN] Ollama provider：32K 本地模型的输入预算被压缩至 1024 tokens
- **作者**: slowly247 | 创建: 2026-09-02 | 更新: 2026-09-04 | 评论: 4
- **链接**: [GitHub Issue #5820](Hmbown/Codewhale%20Issue%20%235820)
- **摘要**: Windows 10 + Ollama 0.32.3 + qwen2.5:7b 场景下，本地模型上下文窗口为 32K，但默认 output reservation 竟然是 64K——窗口钳制逻辑反而把输入预算压缩到了 1024 tokens，导致模型几乎无法工作。
- **关注原因**: 这是本地模型用户高频踩坑的配置冲突问题，影响面大，社区已在讨论解决方案，且有对应修复 PR 提交（#5883）。

### #5860 [OPEN] [enhancement] 对话中持续自学习（自动技能演化）
- **作者**: Edouard-Legoupil | 创建: 2026-09-02 | 更新: 2026-09-04 | 评论: 3
- **链接**: [GitHub Issue #5860](Hmbown/Codewhale%20Issue%20%235860)
- **摘要**: 当前 SKILL.md 技能系统是静态的：agent 无法从重复解决的同类问题中自动提取模式，知识不会自我演化。
- **关注原因**: 社区对 agent 长期记忆和自动技能演化的期待很高，属于 AI 编程工具的前沿方向，评论中有多位用户参与方案讨论。

### #5871 [CLOSED] [bug] Todo 列表快照在会话记录中堆积，无法清空
- **作者**: ronohara | 创建: 2026-09-04 | 更新: 2026-09-05 | 评论: 1
- **链接**: [GitHub Issue #5871](Hmbown/Codewhale%20Issue%20%235871)
- **摘要**: 每次 `todo_write` 调用都会将完整快照永久渲染为会话卡片；即使清空列表（0 或 1 项），历史快照仍然会堆叠显示，严重干扰阅读。
- **关注原因**: 这是影响日常交互体验的 bug，修复 PR #5873 已合并关闭，1 条评论确认问题解决。

### #5872 [OPEN] [enhancement] 将 rusty_alloc 作为 mimalloc 旁的可选特性加入
- **作者**: freedomlovesfrank | 创建: 2026-09-04 | 更新: 2026-09-04 | 评论: 1
- **链接**: [GitHub Issue #5872](Hmbown/Codewhale%20Issue%20%235872)
- **摘要**: 当前引入 mimalloc 需要 C 编译器；建议将 rusty_alloc 作为 opt-in feature，让贡献者无需 C 编译器也能完成交叉编译，让 TUI/agent 的构建链更轻量。
- **关注原因**: 反映出一部分开发者希望降低 Rust 项目的 native 依赖门槛，提升交叉编译体验。

### #5866 [CLOSED] 2026 年眼科 CPT 与 ICD-10 编码更新
- **作者**: medicalbilling-usa | 创建: 2026-09-03 | 更新: 2026-09-04 | 评论: 1
- **链接**: [GitHub Issue #5866](Hmbown/Codewhale%20Issue%20%235866)
- **摘要**: 推广眼科医疗账单编码更新文章，属于垃圾营销信息。
- **关注原因**: 提醒维护者需要加强 Issue 的 spam 治理；社区实际讨论价值低，本条目不展开。

## 4. 重要 PR 进展

> 数据源共 13 条，按主题分组如下。

### 核心功能修复

- **#5873 [CLOSED] fix(tui): 替换过期的 todo 对话快照**
  - **作者**: yiheng-kkk | 更新: 2026-09-05
  - **链接**: [GitHub PR #5873](Hmbown/Codewhale%20PR%20%235873)
  - **内容**: 只保留最新一次成功 `todo_write` 的快照，清空当前快照时不再移除历史对话上下文。修复 #5871，9 个 todo_write 相关测试全部通过。
  - **重要性**: 已合并关闭，实打实解决了 Todo 列表污染会话的问题。

- **#5883 [OPEN] fix(tui): 从 route 声明的窗口推导本地输出预算**
  - **作者**: dajiaohuang | 创建: 2026-09-04 | 更新: 2026-09-04
  - **链接**: [GitHub PR #5883](Hmbown/Codewhale%20PR%20%235883)
  - **内容**: 当模型不在静态目录中时，按 route 声明的 context window 推导自动 output reservation；保留 operator 显式覆盖和既有窗口钳制逻辑；加入 Ollama 32K synthetic 回归测试。
  - **重要性**: 直接针对 Issue #5820 的修复，等待 review。

### CI / 工程基线

- **#5882 [CLOSED] test: 恢复贡献者 CI 基线与进程生命周期检查**
  - **作者**: Hmbown | 创建: 2026-09-04 | 更新: 2026-09-05
  - **链接**: [GitHub PR #5882](Hmbown/Codewhale%20PR%20%235882)
  - **内容**: 恢复 CI，使无关 PR 能基于可用的基线被评审。修复 plugin 生命周期 fixtures 缺 trust token、Windows 上跳过 Unix symlink 测试、pointer assertion 更新、格式化调整。
  - **重要性**: 已合并，直接影响后续所有贡献者的 PR 验证效率。

### 工具链修复

- **#5870 [OPEN] Fix: Tools: 原子提交拆分——按依赖顺序排列无关变更**
  - **作者**: goransh-walia | 创建: 2026-09-04 | 更新: 2026-09-04
  - **链接**: [GitHub PR #5870](Hmbown/Codewhale%20PR%20%235870)
  - **内容**: 解决 Issue #3999——提交拆分时按依赖关系对无关变更排序，检测并拒绝循环依赖。AI 辅助生成，已做语法与 scope 校验。
  - **重要性**: 对工具链（commit 拆分）能力增强，需人工 review。

### 依赖自动化更新（Dependabot）

- **#5881 [OPEN] chore(deps): bump tower-http 0.7.0 → 0.7.1** — [PR #5881](Hmbown/Codewhale%20PR%20%235881)
- **#5875 [OPEN] chore(deps): bump base64 0.22.1 → 0.23.1** — [PR #5875](Hmbown/Codewhale%20PR%20%235875)
- **#5876 [OPEN] chore(deps): bump lru 0.18.2 → 0.18.3** — [PR #5876](Hmbown/Codewhale%20PR%20%235876)
- **#5880 [OPEN] chore(deps): bump jsonschema 0.46.10 → 0.52.1** — [PR #5880](Hmbown/Codewhale%20PR%20%235880)
- **#5877 [OPEN] chore(deps): bump rmcp 2.2.0 → 3.2.0**（MCP Rust SDK 大版本更新）— [PR #5877](Hmbown/Codewhale%20PR%20%235877)
- **#5828 [OPEN] chore(deps): bump npm_and_yarn 依赖（feishu-bridge / vscode 扩展：qs、fast-uri）** — [PR #5828](Hmbown/Codewhale%20PR%20%235828)
- **#5879 [OPEN] chore(deps): bump softprops/action-gh-release 3.0.2 → 3.0.3** — [PR #5879](Hmbown/Codewhale%20PR%20%235879)
- **#5878 [OPEN] chore(deps): bump actions/create-github-app-token 2 → 3** — [PR #5878](Hmbown/Codewhale%20PR%20%235878)
- **#5874 [OPEN] chore(deps): bump docker/setup-qemu-action 4.2.0 → 4.3.0** — [PR #5874](Hmbown/Codewhale%20PR%20%235874)

> 重要提示：以上 Dependabot PR 中，最值得关注的是 **rmcp 2.x → 3.x**（#5877）与 **jsonschema 0.46 → 0.52**（#5880）两个 major 版本升级，需重点回归 MCP 协议相关功能与 JSON Schema 校验逻辑。

## 5. 功能需求趋势

1. **本地模型上下文窗口配置优化**：Ollama 等本地 provider 的输入/输出预算推导逻辑需要更智能，不能因默认参数导致窗口被钳制到不可用（Issue #5820 + PR #5883）。
2. **Agent 持续学习与技能自动演化**：社区希望 agent 能自动从重复问题中提取模式，推动 SKILL.md 自更新（Issue #5860）。
3. **更干净的会话/状态管理**：Todo 等工具调用不得污染对话上下文，历史快照需要自动折叠或可清除（Issue #5871 / PR #5873）。
4. **降低原生构建依赖**：开发者希望选择纯 Rust 的内存分配器（rusty_alloc）来替代需要 C 编译器的 mimalloc，改善交叉编译体验（Issue #5872）。

## 6. 开发者关注点

- **Ollama 本地模型可用性**：32K 窗口模型在 Windows 上输入预算被错误压缩到 1024 tokens，严重影响实用，修复 PR #5883 需尽快跟进 review 与合入。
- **Todo 列表对上下文成本的隐性消耗**：永久快照造成 token 浪费和阅读干扰，已有用户明确反馈；建议后续在 UI 层做快照折叠或“仅显示最新”策略。
- **构建链的 C 编译器依赖**：Rust 开发者对 native dependency 敏感，提供无 C 依赖的 allocator 选项有助于吸引更多跨平台贡献者。
- **CI/PR 基线稳定性**：维护者已主动修复 CI，但后续需要保持基线持续可用，避免外部贡献者因环境问题阻塞提交。
- **Issue 垃圾信息治理**：#5866 表明仓库需要 spam 过滤策略，避免营销内容占用社区注意力。

---

*本日报由 AI 自动生成，数据截至 2026-09-05。*

</details>

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*