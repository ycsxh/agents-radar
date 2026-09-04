# AI CLI 工具社区动态日报 2026-09-04

> 生成时间: 2026-09-04 04:02 UTC | 覆盖工具: 9 个

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

# AI CLI 工具横向对比分析报告（2026-09-04）

> 分析范围：Claude Code、OpenAI Codex、Gemini CLI、GitHub Copilot CLI、Kimi Code CLI、OpenCode、Pi、Qwen Code、DeepSeek TUI（Codewhale）。以下口径均为各仓库 2026-09-04 滚动 24 小时公开数据。

## 1. 生态全景

AI CLI 正在完成从「单次会话式编码助手」向「可编程 Agent 平台」的过渡：Subagent 控制、Hooks、后台任务、Worktree、远程会话、插件市场已成为跨工具的活跃议题。各模型厂商同步加快模型发布频率，GPT-6-Astra、Gemini 3.8-flash 等新模型的一夜更新和入库请求，说明「模型选择及时性」已成为新的竞争力矩阵。与此同时，长会话恢复、上下文压缩、缓存命中与 Windows/WSL 兼容性等规模化使用后才暴露的可靠性问题集中爆发，社区对稳定性与成本可预测性的不满明显大于对功能数量的需求。MCP/ACP 等互联协议正成为新的标准战场，但协议版本协商、OAuth、故障隔离等细节远未成熟。

## 2. 各工具活跃度对比

| 工具 | 版本 / Release（24h） | Issues（本期覆盖） | PR（本期覆盖） | 代表性热点 |
|---|---|---|---|---|
| **Claude Code** | v2.1.260（正式版） | 10 个热点议题 | 5 个更新（4 Open / 1 Closed） | Windows 窗口置顶 #85891（167👍/76评论）；GitLab 集成 131👍 |
| **OpenAI Codex** | v0.153.1、v0.153.2 + 3 个 alpha | 10 个热点议题 | 10 个推进中 PR | WSL 项目管理失效 #41290（30 评论）；Aider 式 co-author #938（14👍） |
| **Gemini CLI** | Nightly v0.60.0-20260904 | 10 个热点议题 | 10 个重要 PR | Subagent 误报成功 #22323（P1）；模型选择器缺新模型 #29164（12👍） |
| **GitHub Copilot CLI** | v1.0.83-4、v1.0.83-5 | 10 个热点议题 | 0 条合并/更新 | Auto 模型池不可配置 #4218（13👍）；企业远程会话误伤 #3442（10👍） |
| **Kimi Code CLI** | 无 | 7 条动态（1 Open / 6 Closed） | 1 个关闭 | ACP 强制 Kimi OAuth 阻碍自定义 Provider #2633 |
| **OpenCode** | 无 | 10 个热度议题 | 10 个重点 PR | Gemini edit 兼容性 #266（39 评论）；动态工作流诉求 #29059（22👍） |
| **Pi** | 无 | 10 个热度议题 | 10 个重点 PR | 终端乱滚动 #5023（18 评论）；大分支摘要 token 上限 #8845（14 评论） |
| **Qwen Code** | v0.23.0（正式版） | 10 个热点议题 | 10 个重要 PR | TUI 迁移 OpenTUI #8662（28 评论）；Dependency CVE 审计失败 #10850（P1） |
| **DeepSeek TUI（Codewhale）** | 无（0.9.12 准备中） | 4 条活跃 Issues | 8 个 PR（4 Open / 4 Closed） | ACP 缺少 session/list、session/config 能力 #5863/#5864 |

> 注：各仓库维护者对「热点」筛选口径不同，上表反映的是每个仓库日报覆盖的动态范围，适合横向看方向而非精确归一化比较。

## 3. 共同关注的功能方向

| 方向 | 涉及工具 | 社区具体诉求 |
|---|---|---|
| **多模型路由与可配置性** | Claude Code、Codex、Gemini、Copilot、OpenCode、Pi、Kimi | Subagent 级模型路由、可配置 Auto 模型池、本地/云端模型混跑、新模型当天入库、自定义 Provider 不被账号体系绑定 |
| **长会话稳定性与状态可恢复** | Codex、Copilot、Claude Code、Gemini、Qwen、Pi | 超大会话 OOM、残留“幽灵会话”、checkpoint 损坏、cache 不命中、Todo 冻结、恢复后 UI 卡死；要求 session 具备可靠序列化、可追溯、可恢复 |
| **沙箱权限的可预期性** | Claude Code、Codex、Gemini、Copilot、Qwen、Pi | “确认/允许”规则既不能被环境变量绕过，也不能误伤正常 cd、git diff、防御性代码审查；安全机制需要可解释、可豁免、可审计 |
| **MCP/ACP 协议成熟度** | Copilot、Gemini、Kimi、DeepSeek、Claude Code | MCP OAuth / CIMD、协议版本协商失败、单 MCP 故障拖垮全局、ACP 会话枚举/恢复能力缺失；协议互联正从 demo 走向生产级要求 |
| **插件与异步 Agent 能力** | Claude Code、OpenCode、Gemini、Copilot、Qwen | Function Hooks、插件管理器、后台 shell、worktree 隔离、浏览器自动化；单一终端已不够，需要事件、通知、后台任务和可编程入口 |
| **Windows / WSL 一等公民体验** | Claude Code、Codex、Copilot、Gemini、Qwen、OpenCode、Pi | Windows 桌面端置顶与崩溃、ConstrainedLanguage 报错、git 参数注入、NTFS 短文件名绕过、中文 IME 对比度过低、WSL 安装失败等高频问题 |

## 4. 差异化定位分析

- **Claude Code**：目前功能最厚、社区讨论最深的「可组合代码 Agent」。Diff 面板、`/cost` 诊断、Git 工作流等特性完整，新议题已进入 Function Hooks / 深度插件化设计阶段，定位偏重度专业用户和复杂代码库。
- **OpenAI Codex**：与 ChatGPT 账户、GPT-6-Astra 模型发布节奏绑定最紧。近期 PR 集中在 Remote Exec、托管 Worktree、可信请求头，目标是成为「可嵌入团队协作流的多端 Agent 平台」，但 Windows/配额稳定性拖了后腿。
- **Gemini CLI**：Google 模型与跨云路线为主，同时探索 browser_agent、AST 感知代码读取等前沿方向。社区对 Subagent 可靠性和 checkpoint 安全投入了大量关注，说明核心 Agent 状态机仍需加固。
- **GitHub Copilot CLI**：最强调企业合规与组织策略。版本迭代由官方热修复驱动，社区 PR 几乎为零；重点在于进入已有 GitHub 企业治理体系，而非成为通用可插拔平台。
- **Kimi Code CLI**：仍处于早期平台化转折点。社区的集中诉求是“允许自定义 Provider、不要强制 Kimi 账号”，但当前 ACP 认证门槛反而在收口。它需要尽快回答自己是「Kimi 模型前端」还是「通用 Agent 网关」。
- **OpenCode**：最接近「本地优先 + 多 Agent 编排」的开源挑战者。桌面插件管理器、浏览器自动化、后台 shell、子代理模型参数等 PR 同时推进，定位是集成开发环境式的 Agent Runtime。
- **Pi**：带有明显系统工具气质的 TUI。重视上下文预算、provider 目录、静态链接、信号退出码等底层工程质量，社区反馈偏向性能、数据完整性和可自托管场景。
- **Qwen Code**：国内模型生态与 Web/远程开发结合最紧密。CI 效率、依赖安全、中文 IME、输出净化、Token Plan 新套餐同步等议题显示出其服务化与规模化意识较强。
- **DeepSeek TUI（Codewhale）**：走「兼容现有生态协议」的快速适配路线，例如补充 OpenCode 的 `x-opencode-session` header，但 ACP 的 session/config 能力仍显早期。适合作为观察协议兼容性落地速度的样本。

## 5. 社区热度与成熟度

从议题热度和维护者响应看，可大致分为三个梯队：

- **高热成熟区：Claude Code、OpenAI Codex、GitHub Copilot CLI**。Claude Code 社区最有「产品设计参与感」，单个 issue 可获 167👍 与 76 条评论；Codex 每日版本节奏最密；Copilot 则以企业用户的强反馈和零星高赞需求为主，但社区 PR 贡献基本为空。
- **密集开发迭代区：Gemini CLI、Qwen Code、OpenCode、Pi**。Gemini 的功能讨论与 P1 安全 PR 都很密集，但多个 P1 issue 存活 6 个月未解决，呈现「开发速度快、历史债也重」的状态；Qwen 的 issue 分诊和 PR 对应效率极高，快速修复标签明显；OpenCode 的 PR 数量大、功能扩张激进，但桌面端稳定性问题也密集；Pi 维护者对小 bug 的关闭速度很快。
- **成长期：Kimi Code CLI、DeepSeek TUI（Codewhale）**。每日动态数量明显较少，仍是「少量关键 issue 卡住体验」的阶段，社区整体声音不足以形成规模压力，但诉求方向已初步平台化。

综合看，产品成熟度与代码成熟度并不完全一致：Claude Code 和 Copilot 功能完备但 Windows 历史问题长期悬置；Gemini 设计想法前沿但核心 Subagent 可靠性仍未根本修复。对技术选型者而言，不能只看功能清单，还应把「长会话恢复、缓存命中、Windows/WSL 实机验证」作为采购测试项。

## 6. 值得关注的趋势信号

1. **模型路由能力正成为下一代选型标准。** 新模型发布越来越快，社区不再接受“等官方升级 model picker”。工具是否支持 per-agent 模型指定、自定义 provider、本地模型与云端模型混跑，将直接影响成本控制能力。

2. **可靠性债务已经超过功能需求，成为社区最大公约数。** 从 Codex 的“已删除会话无法清除”、Copilot 的 4GiB 堆 OOM、Claude Code 的缓存未命中、到 Gemini 的 checkpoint 崩溃，核心都指向会话数据模型不够强健。评估时应当把 `--resume`、worktree 会话、compaction、崩溃恢复作为一等公民测试。

3. **Windows / WSL 不再是“边缘平台”。** 几乎所有主流工具在 Windows 桌面、WSL、企业受限语言模式、中文 IME 上都存在成串问题。若你的团队以 Windows 为主，任何 PoC 都必须单独覆盖该平台，否则工具在 paper 上的能力会显著缩水。

4. **安全沙箱正在进入「既要防绕过、又要防误伤」的第二阶段。** 多个仓库同时出现环境变量绕过、路径穿越、git 参数注入等安全问题，也出现 cd 误触发、安全审查误报等信任损耗问题。安全机制如果不可解释、不可豁免，对开发者日常体验的伤害不亚于漏洞本身。

5. **MCP/ACP 将决定工具生态位，但目前互操作成熟度不足。** OAuth 令牌复用、协议版本协商、单节点故障隔离、ACP 会话恢复都是高频摩擦点。选择工具时应关注它对 MCP 的故障隔离能力和 OAuth 支持，而不是仅看“支持 MCP”的标语。

6. **Agent 工作流会从“人盯终端”走向异步协作。** Hooks、后台执行、Agent 完成通知、浏览器自动化、worktree 多任务并行是当前 PR 最集中的领域。开发者可提前布局支持事件通知、CLI 可编程化和多 Agent 状态可观测的工具链，以匹配下一阶段工作方式。

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills 社区热点报告

**数据截至 2026-09-04 · 数据源：GitHub anthropics/skills**  
说明：PR 侧热度按仓库「评论数排序」呈现；快照所示 Top PR 均为 Open 状态。

---

## 一、热门 Skills 排行（Top 8 PR）

### 1. skill-creator 评估链路修复 — #1298
- **功能**：修复 `run_eval.py` 对所有 skill 描述一律报 `recall=0%` 的致命缺陷（#556，10+ 次独立复现），并将评估产物安装为真实 skill，同时修复 Windows 流读取、触发检测与并行 worker 问题。该修复直接决定 `run_loop.py` / `improve_description.py` 优化闭环是否有效。
- **社区热点**：skill-creator 是社区的「元技能」，其评估器失效意味着所有 skill 描述优化都在“对着噪声调参”——讨论热度居全部 PR 之首，是典型的阻断级问题。
- **状态**：Open（评审中）
- https://github.com/anthropics/skills/pull/1298

### 2. document-typography（文档排版质检）— #514
- **功能**：新增排版质量检查技能，覆盖 AI 生成文档三类高频问题：孤儿词换行（1–6 个词溢出到下一行）、孤行段落/标题滞留页底、编号错位。
- **社区热点**：讨论聚焦其泛用性——“几乎每份 Claude 生成的文档都受影响”，属于低门槛、高普适价值的 Skill。
- **状态**：Open（评审中）
- https://github.com/anthropics/skills/pull/514

### 3. scnet-hpc（HPC 集群运维）— #1615
- **功能**：新增面向 SCNet HPC 集群的运维 Skill，基于 profile 化 SSH 与 Slurm 工作流，覆盖集群发现、分区/内存/模块/加速器选型、作业生成与 profile 刷新。
- **社区热点**：科学计算用户对「垂直领域、可复用的集群操作封装」兴趣浓厚，创建于 2026-08-20，属近期活跃 PR。
- **状态**：Open（评审中）
- https://github.com/anthropics/skills/pull/1615

### 4. ODT/OpenDocument 多维文档技能 — #486
- **功能**：新增 `odt` Skill，支持 OpenDocument（.odt/.ods）的创建、模板填充、读取及 **ODT → HTML** 解析转换，覆盖 LibreOffice/ODF/ISO 标准文档场景。
- **社区热点**：与 PDF/DOCX 技能形成互补，社区关注点集中在模板变量映射与格式保真这类实现细节。
- **状态**：Open（评审中）
- https://github.com/anthropics/skills/pull/486

### 5. Hivemind（零成本多智能体编排）— #1628
- **功能**：新增 Hivemind Skill，让 Claude Code 将机械性工作委托给运行在免费模型上的 headless opencode workers，Claude Code 仅保留规划、审查与合并职责。核心理念：**昂贵模型的上下文是稀缺资源，而非其智能**。
- **社区热点**：低成本扩展上下文的思路引起广泛讨论，与「上下文成本控制」这一社区痛点直接相关。
- **状态**：Open（评审中）
- https://github.com/anthropics/skills/pull/1628

### 6. self-audit（交付前输出审计门禁）— #1367
- **功能**：新增自带 v1.3.0 的自动化审计 Skill：先做机械性的“输出文件是否真实存在”校验，再按危害优先级执行四维度推理质量审计；不绑定技术栈。
- **社区热点**：属于“质量门禁管线”系列提案（对应 Issue #1385），社区对把审计做成可复用标准件有持续需求。
- **状态**：Open（评审中）
- https://github.com/anthropics/skills/pull/1367

### 7. testing-patterns（全栈测试模式库）— #723
- **功能**：新增综合性测试技能，覆盖 Testing Trophy 测试哲学、单元测试 AAA 模式、命名规范、React Testing Library 组件测试及边界用例等完整测试栈。
- **社区热点**：开发工作流刚需，讨论围绕“测试什么 vs 不测试什么”的决策规则如何被 Claude 稳定执行。
- **状态**：Open（评审中）
- https://github.com/anthropics/skills/pull/723

### 8. ServiceNow 平台技能（企业级宽覆盖）— #568
- **功能**：新增 `servicenow` Skill，定位为平台级助理而非狭义的脚本助手，覆盖 ITSM、ITOM、ITAM/SAM、FSM、HRSD/CSM、SPM、漏洞响应、安全事件响应、CSDM 与 IntegrationHub。
- **社区热点**：企业自动化场景需求明确，作者持续迭代至 2026-08-12；讨论关注单一 Skill 的体量与可维护性边界。
- **状态**：Open（评审中）
- https://github.com/anthropics/skills/pull/568

> 其他值得注意：#210 frontend-design 可执行性改进、#525 pyxel 复古游戏开发（作者即 Pyxel/pyxel-mcp 引擎作者）、#83 skill-quality-analyzer / skill-security-analyzer 双元技能——后者与下述安全议题直接呼应。

---

## 二、社区需求趋势（来自 Issues）

1. **安全与信任边界治理（最热议题）**  
   Issue #492（43 条评论）指出：社区技能被分发在 `anthropic/` 命名空间下，冒充官方技能，形成“用户误授高权限”的信任边界漏洞；同方向的 #412 提出 agent-governance 安全模式技能，#1175 关注 SharePoint 场景的权限与上下文安全。  
   → 社区强烈期待 **Skill 安全审计、来源可信验证与治理类技能**。  
   https://github.com/anthropics/skills/issues/492

2. **企业级 Skill 分发与共享基础设施**  
   Issue #228（16 条评论，8 👍）要求支持组织内 Skill 直链/共享库，替代“下载 .skill 文件 → Slack 传输 → 手动上传”的低效流程。  
   https://github.com/anthropics/skills/issues/228

3. **元技能（skill-creator）评估可靠性与 Windows 兼容**  
   Issue #556（12 条评论，7 👍）“claude -p 从不触发 skills”，叠加 #1099 / #1050 的 Windows 管道崩溃，说明社区大量用户正在深度使用 skill-creator 做自动化优化，**对“可评估、可复现”的闭环有刚性需求**。  
   https://github.com/anthropics/skills/issues/556

4. **上下文窗口体积控制**  
   Issue #1487 报告 `claude-api` Skill 单次调用注入约 **15.6 万 token**、直接打爆上下文；#189（9 👍）报告 `document-skills` 与 `example-skills` 插件安装重复内容导致上下文浪费。  
   → Skill 的 **轻量化与按需加载** 正成为核心质量指标。  
   https://github.com/anthropics/skills/issues/1487

5. **新方向提案集中于“智能体自身能力”**：  
   长生命周期记忆压缩（#1329 compact-memory，符号化状态表示）、推理质量门禁管线（#1385）、将 Skill 暴露为 MCP 接口（#16）、Bedrock 平台可用性（#29）。说明社区不再只满足于“技能教 Claude 做 X”，而开始要求 **Skill 参与智能体的记忆、治理与互操作架构**。  
   https://github.com/anthropics/skills/issues/1329

---

## 三、高潜力待合并 PR（评审活跃、近期可能落地）

| PR | Skill | 落地潜力分析 |
|---|---|---|
| #1298 | skill-creator 评估修复 | 直接解除 #556 阻断问题，属“修复型”PR，合并优先级最高 |
| #538 / #541 / #539 | pdf / docx / skill-creator 修复 | 同一作者 Lubrsy706 的三组小改动、根因清晰（大小写敏感、OOXML w:id 冲突、YAML 引号校验），典型可快速合入的健壮性修复 |
| #514 | document-typography | 跨文档类型的通用排版质检，场景普适、无平台绑定 |
| #723 | testing-patterns | 开发工作流刚需，覆盖面广、结构完整 |
| #525 | pyxel 游戏开发 | 作者即引擎生态维护者（kitao），与 pyxel-mcp 天然配套，7 月仍在更新 |
| #486 | ODT 文档技能 | 补齐 LibreOffice/ISO 文档处理空白，需求稳定 |

参考链接：  
https://github.com/anthropics/skills/pull/1298 · https://github.com/anthropics/skills/pull/538 · https://github.com/anthropics/skills/pull/541 · https://github.com/anthropics/skills/pull/539 · https://github.com/anthropics/skills/pull/514 · https://github.com/anthropics/skills/pull/723 · https://github.com/anthropics/skills/pull/525 · https://github.com/anthropics/skills/pull/486

> 注：截至数据快照日，上述 PR 均处于 Open 状态，尚无一条被官方合并——社区贡献的“落地周期”本身也是后续值得关注的数据点。

---

## 四、Skills 生态洞察（一句话）

**社区当前最集中的诉求，已从“增加更多新技能”转向“让 Skill 可评测、可信、轻量”——热度最高的议题几乎全部围绕 skill-creator 评估器失效（0% recall）、`anthropic/` 命名空间信任滥用、以及单次注入 15.6 万 token 的上下文爆炸等生态基建问题，标志着 Claude Code Skills 正从“内容扩充期”进入“工程质量与治理期”。**

---

# Claude Code 社区动态日报

**日期：2026-09-04**  
**数据来源：github.com/anthropics/claude-code**

---

## 1. 今日速览

昨日发布 v2.1.260，新增全屏模式下的 Diff 面板与 `/diff` 指令，并增强 `/cost` 的缓存未命中原因诊断。社区侧，Windows 桌面版「窗口强制置顶」问题持续发酵（主 Issue 获 167 👍、76 条评论）；功能讨论则聚焦于 Function Hooks（插件能力跃升）与 GitLab 集成（131 👍）两大方向。

---

## 2. 版本发布

### v2.1.260
- **新增 Diff 面板**：全屏模式下，对话旁会打开一个 Diff 面板，实时展示 Claude 编辑的未提交更改，可通过 `/diff` 指令切换。
- **改进 `/cost` 诊断**：针对 prompt-cache 未命中场景，现在会显示可能原因（例如工具定义或系统提示词变更、空闲超过 TTL 等）。

---

## 3. 社区热点 Issues（Top 10）

### 1. Claude Desktop (Windows 11) 窗口始终置顶，无设置可关闭
- **Issue**: [#85891](https://github.com/anthropics/claude-code/issues/85891)
- 作者：kylealty-boop | 更新：2026-09-04 | 评论：76 | 👍：167 | 状态：OPEN
- **要点**：Windows 11 上 Claude Desktop 窗口强制置顶。这是 #66516（macOS 端）的 Windows 对应问题。同类型重复 Issue #88093（评论 17，👍 37）也被并入关注。
- **值得关注**：社区共鸣极强，167 个 👍 表明在 Windows 桌面用户中影响广泛，但目前无任何设置项可关闭。

---

### 2. Function Hooks：让插件能力提升 10 倍
- **Issue**: [#91870](https://github.com/anthropics/claude-code/issues/91870)
- 作者：poteat | 创建：2026-09-03 | 评论：64 | 👍：35 | 状态：OPEN
- **要点**：提出「Function Hooks」概念：通过参数化 `$` 对象 + 副作用追踪来安全地深度修改 Claude Code，并采用类 Express/Koa 的 `next` 链式注册模型实现组合。
- **值得关注**：64 条评论说明社区对插件扩展机制有极高的参与热情，属于设计导向型大讨论。

---

### 3. GitLab 集成（仓库连接、MR、移动端访问）
- **Issue**: [#12346](https://github.com/anthropics/claude-code/issues/12346)
- 作者：rogi-sh | 创建：2025-11-25 | 评论：52 | 👍：131 | 状态：OPEN
- **要点**：请求新增 GitLab 集成能力，覆盖 Repository Connection、Merge Requests 与移动端访问。
- **值得关注**：长期悬而未决的高赞功能请求（131 👍），明确反映 GitHub-only 的现状对 GitLab 用户构成使用门槛。

---

### 4. Claude Desktop Windows 启动失败——孤儿 Job Object 导致只能注销/重启恢复
- **Issue**: [#53247](https://github.com/anthropics/claude-code/issues/53247)
- 作者：rnpacheco25-sudo | 创建：2026-04-25 | 评论：55 | 👍：25 | 状态：OPEN
- **要点**：应用崩溃后遗留 Silo / Job Object，重启应用时出现 HRESULT 0x80070020（AppModel-Runtime EventID 215/208），只能注销或重启系统。
- **值得关注**：直接影响 Windows 用户日常使用，生命周期已横跨 4 个月仍未解决，社区关注度持续上升。

---

### 5. Per-agent 模型提供商路由（如本地 Ollama 用于子代理）
- **Issue**: [#38698](https://github.com/anthropics/claude-code/issues/38698)
- 作者：berthelius | 创建：2026-03-25 | 评论：11 | 👍：43 | 状态：OPEN
- **要点**：目前 `ANTHROPIC_BASE_URL` 是会话级配置，子代理的 `model` 参数只能选 sonnet/opus/haiku，无法在同一会话内将不同子代理路由到不同模型提供商。
- **值得关注**：43 👍 表明本地模型生态用户对混合路由（敏感任务走本地、复杂任务走云端）的需求明确。

---

### 6. Bash cd 复合读取守卫在绝对路径上误触发
- **Issue**: [#91650](https://github.com/anthropics/claude-code/issues/91650)
- 作者：railapex | 创建：2026-09-02 | 评论：9 | 👍：52 | 状态：OPEN
- **要点**：Windows Git Bash 环境（2.1.257~2.1.259），只要存在任意 `Read()` 拒绝规则，`cd` 到绝对路径就会触发权限询问。
- **值得关注**：52 👍 在较短时间内获得，属权限系统回归/误判类问题，对 Windows 用户的自动化工作流影响很大。

---

### 7. Prompt 缓存在链式 `-p --resume` 调用中从未命中
- **Issue**: [#91971](https://github.com/anthropics/claude-code/issues/91971)
- 作者：thebeals | 创建：2026-09-04 | 评论：2 | 👍：0 | 状态：OPEN
- **要点**：系统提示词/工具定义的静态前缀缓存正常，但每次对话轮次内容写完后无法进入可复用缓存，导致链式调用成本线性增长。
- **值得关注**：这是新版 `/cost` 诊断推出后暴露出的深层缓存问题，对高频 `-p` 自动化用户直接影响费用与延迟。

---

### 8. VS Code 扩展：点击聊天内的二进制文件链接静默失败
- **Issue**: [#81227](https://github.com/anthropics/claude-code/issues/81227)
- 作者：Oleh-Matiash | 创建：2026-07-25 | 评论：3 | 👍：6 | 状态：OPEN
- **要点**：聊天面板中点击 PNG/JPG/PDF 等链接时，扩展调用 `showTextDocument()` 并因二进制文件被拒绝，且 rejection 未被捕获，导致“点了没反应”。
- **值得关注**：IDE 集成细节缺陷，属于高频操作路径上的体验问题，修复成本应该不高，但一直未被处理。

---

### 9. Auto-memory 在 git-worktree 会话中加载不一致
- **Issue**: [#81833](https://github.com/anthropics/claude-code/issues/81833)
- 作者：bunahu | 创建：2026-07-28 | 评论：12 | 👍：0 | 状态：OPEN
- **要点**：`<repo>/.claude/worktrees/<name>` 下启动的会话中，有时完整加载项目 MEMORY.md 索引，有时完全没有任何记忆内容，表现随机。
- **值得关注**：Auto-memory 是 Claude Code 的核心记忆机制，该 inconsistency 会直接导致代理行为不一致，评论中有复现信息（has repro）。

---

### 10. Claude in Chrome：权限/凭据确认提示渲染在用户不可见的窗口中
- **Issue**: [#91969](https://github.com/anthropics/claude-code/issues/91969)
- 作者：anandman | 创建：2026-09-04 | 评论：1 | 👍：0 | 状态：OPEN
- **要点**：navigate/凭据请求的授权确认弹窗出现在用户看不到的窗口/画布中，用户无法点击，最终超时并报 “Permission denied by user” / transport_error。
- **值得关注**：属于设计缺陷而非简单的权限逻辑错误——提示需要展示在正确的前台窗口。同类问题在 #83959 也因提示/权限状态不同而被单独报告。

---

## 4. 重要 PR 进展

> 过去 24 小时内更新的 PR 仅 5 条，均为社区贡献，其中 4 条处于 Open 状态，1 条已关闭。

### 1. fix(security-guidance): 让 `**` glob 模式匹配零深度路径
- **PR**: [#87079](https://github.com/anthropics/claude-code/pull/87079)
- 作者：anishsamant | 更新：2026-09-04 | 状态：OPEN
- **要点**：当前安全规则匹配委托给 fnmatch，而 fnmatch 中裸 `*` 已可跨越 `/`，导致 `**/*.ts` 反而要求显式 `/`，会把顶层文件从 security-patterns.json 检查中静默排除。由于是安全规则，失败模式为「静默不拦截」。
- **价值**：修复一个可能造成安全规则失效的静默缺陷，应优先合入。

---

### 2. docs: 对齐 code-review README 与实际命令行为
- **PR**: [#79150](https://github.com/anthropics/claude-code/pull/79150)
- 作者：Codeturion | 更新：2026-09-03 | 状态：OPEN
- **要点**：README 描述的流水线（git blame/history agent、0-100 置信度、80 分阈值、可编辑阈值行）与命令实际实现已不一致，文档还在引导用户修改一个不存在的配置。
- **价值**：消除文档与实现脱节带来的误导，属于开发者体验改进。

---

### 3. validate-agent.sh：不要在第一个 warning 时中断，且停止误报合法 agent
- **PR**: [#89404](https://github.com/anthropics/claude-code/pull/89404)
- 作者：bcherny | 更新：2026-09-03 | 状态：OPEN
- **要点**：修复 plugin-dev 的 `validate-agent.sh` 在 `set -euo pipefail` 下遇到 `((count++))` 返回退出码 1 导致提前中止的问题，并消除对 plugin-dev 自身 agent 文件的误报。
- **价值**：解决公共 Issue #83803，直接提升插件开发工具的可用性。

---

### 4. fix(plugin-dev): 修复 validator 脚本因 `set -e` 在首个 finding 处中止
- **PR**: [#66416](https://github.com/anthropics/claude-code/pull/66416)
- 作者：wellkilo | 更新：2026-09-03 | 状态：OPEN
- **要点**：针对 `validate-agent.sh`、`hook-linter.sh`、`validate-hook-schema.sh` 三个脚本在遇到首个错误即整体中止的问题，建议移除 `set -e` 的退出影响。
- **价值**：与 #89404 目标一致、实现不同。两个 PR 并存说明该问题社区关注度高，但方案尚未收敛。

---

### 5. Update /frontend-design SKILL.md
- **PR**: [#91894](https://github.com/anthropics/claude-code/pull/91894)
- 作者：ant-kurt | 创建：2026-09-03 | 更新：2026-09-03 | 状态：CLOSED
- **要点**：对内置 frontend-design 技能文档进行更新。该 PR 已在 24 小时内被关闭，未合并，原因未在数据中展现。

---

## 5. 功能需求趋势

从近 24 小时活跃的 Issue 与 PR 中，可提炼出以下社区关注方向：

1. **多平台/互操作性扩展**：GitLab 集成（#12346，131 👍）长期为最高赞功能请求之一；另出现「单账户内多 Profiles」（#91770）需求——分离历史、记忆与配置。
2. **更灵活的模型路由**：按子代理维度配置不同模型/提供商（#38698），呼应本地模型（Ollama 等）与云端模型混合使用的诉求。
3. **插件/钩子体系深度化**：Function Hooks 大讨论（#91870）预示着社区不满足于现有 hook 表面，希望以安全、可组合的方式深度修改 Claude Code 行为。
4. **缓存与成本工程**：v2.1.260 新增缓存未命中原因后，社区立即跟进深挖（#91971），说明在高频/批量调用场景下，缓存命中率是用户的核心成本痛点。
5. **已有功能的细粒度暴露**：例如在 `subagentStatusLine` 中暴露子代理所用模型（#73654，已关闭——可能已实现或被拒，但反映用户对可观测性的需求）。

---

## 6. 开发者关注点

1. **Windows 桌面版稳定性欠佳**：置顶窗口不可配置（#85891）、崩溃后无法重启（#53247）、截图被恶意遮罩（#91079、#88937）、自动更新器阻塞主进程（#88072）。Windows 用户在多条高频问题中反复出现，形成明显的平台短板。
2. **权限系统误判/行为不一致**：Bash 的 cd 误触发读取确认（#91650）、C 编译器被执行操作误判为安全威胁（#91977）、浏览器权限确认出现在不可见窗口（#91969）。权限系统在边界条件下的「误伤」比「漏放」更容易引发反感。
3. **Auto-memory / 上下文管理可靠性**：git-worktree 中记忆加载随机失败（#81833）、MEMORY.md 更新被读写门控拒绝（#78569）、大 CLAUDE.md 导致上下文反复重发（#91880）。记忆与上下文是代理一致性的地基，此类问题对信心影响极大。
4. **文档与实现脱节**：code-review README 描述的功能已不存在（PR #79150）、plugin-dev 自带校验脚本无法通过自身校验（PR #89404、#66416）。工具链的自举失败会放大用户对整体质量的怀疑。
5. **小细节体验积累不满**：VS Code 中点击二进制链接静默失败（#81227）、终端中 SendUserFile 报“已送达”但用户看不到文件（#88889）。这些单个看来很小的问题，叠加起来会显著拉低日常使用评价。

---

*本日报由 AI 技术分析师整理，基于 anthropics/claude-code 仓库公开数据，仅供社区参考。*

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex 社区动态日报 — 2026-09-04

> 数据来源：github.com/openai/codex

## 1. 今日速览

今日 Codex 发布线动作频繁，v0.153.1 与 v0.153.2 连续热修复，新增 GPT-6-Astra 模型 API 支持，并修正了其 Fast tier 文案。社区方面，Windows 桌面端已删除会话残留、WSL 项目创建失败以及配额重置失效是最热的三大 Bug 焦点；同时大量工作流相关 PR（托管 worktree、子代理、远程控制）指向 Codex 在团队协作与自动化方向的功能深化。

## 2. 版本发布

| 版本 | 类型 | 内容 |
|------|------|------|
| **rust-v0.153.2** | Patch | 修正 GPT-6-Astra Fast 层描述文字，将“1.5x speed”改为“2x speed, increased usage”，仅影响展示文案。相关 PR：[#42632](https://github.com/openai/codex/pull/42632) |
| **rust-v0.153.1** | Patch | 新增 GPT-6-Astra API 配置支持，模型可通过 API 使用但不显示在模型选择器中（隐藏项）。相关 PR：[#42605](https://github.com/openai/codex/pull/42605) |
| **rust-v0.154.0-alpha.1 / .2 / .3** | Alpha | 面向下一阶段的连续三个预发布测试版本，尚无正式 changelog。 |

## 3. 社区热点 Issues

> 以下 10 个 Issue 反映了当前社区最集中的反馈声音。

### 🔥 Windows + WSL 项目目录管理失效
**#41290** · [openai/codex Issue #41290](https://github.com/openai/codex/issues/41290) · 评论 30 · 👍 21
将 Agent Environment 切换至 WSL 后，项目创建与删除功能失效。曾为 Pro 用户、Windows 平台。该问题评论数居首且获赞达 21 次，是 Windows 用户群的核心阻塞问题。

### 🔥 Codex Desktop 元 Bug：会话状态失控
**#25779** · [openai/codex Issue #25779](https://github.com/openai/codex/issues/25779) · 评论 17 · 👍 8
Codex Desktop 的会话/轮次状态增长无界，引发 UI 冻结、上下文膨胀失控。该 Issue 已存活 3 个月并持续更新，开发者普遍认同这是跨版本存在的架构性隐患。

### 🔥 Windows 已删对话在 Recents 中残留
**#39989** · [openai/codex Issue #39989](https://github.com/openai/codex/issues/39989) · 评论 16
Windows 桌面版在完全重启后，Recents 列表依旧显示已被删除的 ChatGPT 对话，无法清除。

### 🔥 Usage 配额重置失败且配额归零
**#31601** · [openai/codex Issue #31601](https://github.com/openai/codex/issues/31601) · 评论 13 · 👍 5
v0.143.0 CLI 用户报告使用额度重置失败且原配额消失。获 👍 5，说明并非个例。

### 🔥 历史本地项目在更新后丢失
**#39121** · [openai/codex Issue #39121](https://github.com/openai/codex/issues/39121) · 评论 12
Windows 桌面版 26.810.7004.0 更新后，历史本地项目从侧栏消失，但任务记录仍存在。已跨多个版本复现。

### 🔥 使用额度重置入口加载失败
**#37928** · [openai/codex Issue #37928](https://github.com/openai/codex/issues/37928) · 评论 4 · 👍 12
“Usage limit resets” 页面无法加载，用户无法查看已累积的充值额度库存。获 👍 12，在 rate-limit 系问题中关注度最高。

### 🔥 网络安全验证误伤个人仓库
**#32597** · [openai/codex Issue #32597](https://github.com/openai/codex/issues/32597) · 评论 6 · 👍 3
Codex 安全审查将个人仓库的“防御性代码审查”误判为网络安全攻击行为并阻断执行，属于安全策略误报。

### 🔥 429 阻塞重置 Credits UI
**#37934** · [openai/codex Issue #37934](https://github.com/openai/codex/issues/37934) · 评论 5 · 👍 4
重置 Credits API 返回 429，导致桌端与 Web 端均无法进入 reset 界面。Pro 用户受影响。

### 🔥 Windows Schannel 证书错误导致无限重连
**#41275** · [openai/codex Issue #41275](https://github.com/openai/codex/issues/41275) · 评论 4 · 👍 3
Windows 企业网络环境下，Schannel 证书校验失败，Codex Desktop 陷入 “Reconnecting” 死循环。企业/公共部门用户的网络兼容性问题。

### 📌 提交信息中标记 Codex 为 co-author
**#938** · [openai/codex Issue #938](https://github.com/openai/codex/issues/938) · 评论 3 · 👍 14
一年以上历史需求：希望 Codex 在默认提交信息中增加 `(codex)` 作者标记，遵循 [Aider 的规范](https://github.com/Aider-AI/aider)。获 👍 14，为功能请求类中社区支持度最高的一条。

## 4. 重要 PR 进展

> 以下 10 个 PR 代表了未来功能方向与近期重点修复。

### 🔧 托管 Worktree 支持 `codex exec`
**#42652** · [openai/codex PR #42652](https://github.com/openai/codex/pull/42652)
实验性 `worktrees` 功能：新 fork 的 `codex exec` 会话可在 Git 托管 worktree 中运行，以隔离多任务并行代码变更。对团队协作与多分支开发场景意义重大。

### 🔧 GPT-6-Astra 正式加入内置模型目录
**#42607** · [openai/codex PR #42607](https://github.com/openai/codex/pull/42607)
新增 `gpt-6-astra` 模型定义，包含推理分级、工具能力、上下文限制与审查策略等完整配置。

### 🔧 GPT-6-Astra 回移植至 0.153 分支
**#42605** · [openai/codex PR #42605](https://github.com/openai/codex/pull/42605)
将 `gpt-6-astra` （隐藏、API-only）模型目录回移植到 0.153 行，作为 0.153.1 热修复发布——对应今日 v0.153.1。

### 🔧 取消 stdio shutdown 时的远程控制注册
**#42668** · [openai/codex PR #42668](https://github.com/openai/codex/pull/42668)
当 stdio EOF 后，挂起的远端控制注册会阻止 app-server 退出并持有线程写入器等资源。通过独立 child shutdown token 解决退出卡死。

### 🔧 远程控制注册期间增强 TUI 提示
**#42667** · [openai/codex PR #42667](https://github.com/openai/codex/pull/42667)
根据 ChatGPT 账户 Daybreak 资格预取结果，提示是否启用、为何不适用等定制化引导文案。这也是对 #37934 等 rate-limit 问题的配套 UX 改进。

### 🔧 Assistant 文件引用渲染为本地链接
**#42650** · [openai/codex PR #42650](https://github.com/openai/codex/pull/42650)
在 TUI 中将 `codex-file-citation` 指令渲染为可点击的本地链接，支持 Unicode、Windows 分隔符等边界字符，强化桌面端交互体验。

### 🔧 全屏覆盖层退出后恢复内联 TUI
**#42641** · [openai/codex PR #42641](https://github.com/openai/codex/pull/42641)
离开 alternate-screen 后刷新内联视口，清除残留的覆盖层内容并修复历史滚动位置错乱。

### 🔧 强化 TUI 对 Assistant 标记的解析
**#42640** · [openai/codex PR #42640](https://github.com/openai/codex/pull/42640)
引入统一解析器处理助手指令（含引号缺陷、花括号、转义引号等场景），使 Git 操作回执与代码注释解析行为一致。

### 🔧 可信请求头支持远程 Exec WebSocket
**#42606** · [openai/codex PR #42606](https://github.com/openai/codex/pull/42606)
嵌入方（宿主 App）可通过 webSocket 握手附加可信 HTTP 头，并支持会话重连后保留、脱敏。为 `codex exec` 远程化提供安全通道基础。

### 🔧 GPT-6-Astra 接入 Amazon Bedrock
**#42619** · [openai/codex PR #42619](https://github.com/openai/codex/pull/42619)
将 `openai.gpt-6-astra` 及跨区域变体加入 Amazon Bedrock 目录，覆盖更广的云端部署场景。

## 5. 功能需求趋势

综合今日 Issue 与 PR，社区最受关注的功能方向如下：

| 趋势关键词 | 热度信号 | 说明 |
|-----------|---------|------|
| **GPT-6-Astra 等新模型支持** | 3+ Releases / 3 PR | 0.153.x 全部围绕隐藏的 API-only Astra 支持铺开，说明 OpenAI 内部对新一代模型的接入正进入大规模灰度阶段 |
| **Git 工作流集成（worktree、co-author）** | #938（14👍）、#42652 | Aider 式提交 co-author 标记为长期呼声；接管 Git 工作树的 `--worktree` 实验性支持将进一步缩小与 Aider / Cursor 的差距 |
| **Windows / WSL 稳定性** | #41290（30评论） | WSL 环境下的项目生命周期管理是 Windows 开发者使用 Codex 的最大痛点 |
| **Rate limit / 配额重置体验** | #31601、#37928、#37934、#42346、#42660 | 重置 API 损坏 + UI 加载失败 + 配额恢复逻辑不透明，波及面从 Free 到 Pro 用户均存在 |
| **桌面端会话状态一致性** | #25779、#39989、#39121、#31995 | “幽灵会话”、本地历史丢失、长对话加载不全是桌面应用的共性问题 |
| **Remote/Agent 协作与子代理控制** | #42606、#42652、#42623、#42668 | 子代理行为、远程环境握手与超时控制是 Codex 进一步嵌入企业自动化流程的铺垫 |
| **Computer Use 与辅助功能** | #41374、#42666、#42501 | macOS 辅助功能扫描导致 Qt 应用中 NVIDIA 工具崩溃，说明 Computer Use 相对完整，但兼容性仍需打磨 |

## 6. 开发者关注点

**痛点一：Windows 平台体验显著落后于 macOS**
- WSL 切换后项目目录无法创建/删除（#41290）
- 已删除的对话残留在 Recents（#39989）+ 历史本地项目丢了但任务还在（#39121）+ ghost 对话删除不掉（#41987）
- 企业网络陷入无限 “Reconnecting” 死循环（#41275）
- 桌面宠物交互区漂移、点击穿透问题被多次报告（#41535、#42190、#42061）

**痛点二：Rate Limit 恢复机制不稳定，直接消耗用户信任**
- 多用户反馈已充值额度无法在 UI 中显示（#37928）
- Weekly 配额重置逻辑不一致，且本地无活动却耗尽配额（#42660）
- 重置请求本身触发 429 导致界面不可用（#37934），部分用户因此质疑是否应该升级 Pro

**痛点三：安全与合规策略的误伤现象**
- 防御性代码审查被识别成网络安全攻击（#32597），说明安全机制在“真实漏洞扫描”与“正常代码审计”之间的边界仍需调优

**痛点四：上下文/Chat 状态管理复杂度已经浮现**
- 进程端认为 turn 已完成，而 JSONL 底层仍在写入（#38972）
- rollback 后 fork 侧边聊天出现 rollout ordinal 错乱（#42027）
- 长会话仅显示最近轮次、历史被吞（#31995）
- 上述问题互相交织，社区期望 Codex 会话模型升级为有序、可靠、可恢复的状态流

**痛点五：新增模型与 daybreak 特性处于灰度期**
- 虽然 GPT-6-Astra 支持已落地，但用户对“模型选择器中没有、文档缺失、文案显示错误”的困惑将持续存在，需加强可视化与资格提示（#42639、#42667）

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI 社区动态日报 — 2026-09-04

## 今日速览

今日最值得关注的是 **#29172 PR 为 CLI 新增了 gemini-3.8-flash 默认模型支持**，有望解决社区长期抱怨的模型选择器缺少新模型问题。安全方面动作密集：昨日发布的 nightly 修复了 MCP OAuth 流程中的 RFC 9207 问题，多起 PR 针对 Windows 沙箱 git 参数注入、检查点目录穿越、硬编码 API 密钥泄露等安全隐患展开修补。此外，子代理可靠性仍是社区最大痛点（#22323 子代理被误报为成功、#21409 通用代理挂起），以及与 Auto Memory 相关的多项安全和质量问题在持续发酵。

## 版本发布

**v0.60.0-nightly.20260904.g87a9c71d5** 于今日发布，主要内容：

- **fix(core)**: 在 MCP OAuth 流程中强制执行 RFC 9207 issuer identification（由 @jvargassanchez-dot 提交，PR #29117）
- chore(release): 版本号自动提升

## 社区热点 Issues（10 个）

### 1. #22323 Subagent recovery after MAX_TURNS is reported as GOAL success, hiding interruption
**优先级 P1 | 13 条评论 | 2 👍**
`codebase_investigator` 子代理已报告触发最大轮次限制，却仍以 `status: "success"` 和 `Termination Reason: "GOAL"` 上报成功，导致用户无法察觉分析实际上被中断。此问题直接损害用户对代理任务结果可靠性的信任，P1 且持续 6 个月仍未解决，评论区期待根本性修复。

### 2. #29164 3.6 and 3.7 flash still not available in the model picker
**P1 | 6 条评论 | 12 👍**
新版模型未出现在模型选择器中，12 个 👍 说明影响面广（目前获赞最多）。模型支持滞后直接影响用户体验，与今日 PR #29172（新增 3.8-flash 注册）形成呼应——需要更高效的模型发布管道。

### 3. #21409 Generalist agent hangs
**P1 | 8 条评论 | 8 👍**
当 CLI 转交任务给 generalist agent 时无限挂起（用户表示曾等待长达 1 小时），创建文件夹等简单操作也会触发。已有 8 👍 + 8 条评论，说明大量用户被此问题卡住，尤其影响日常使用。

### 4. #25166 Shell command execution gets stuck with "Waiting input" after command completes
**P1 | 4 条评论 | 3 👍**
极简单的 shell 命令执行完毕后，CLI 仍显示命令活跃并卡在"等待用户输入"。命令实际早已完成，疑似终端状态管理 bug，可能会导致自动化流程中断。

### 5. #26525 Add deterministic redaction and reduce Auto Memory logging
**P2 | 5 条评论**
Auto Memory 会将读取本地 transcripts 的消息发送给提取 agent、并由 prompt 指示模型在内容进入上下文后才做脱敏；且服务可能记录含敏感信息的 skill 内容。数据安全原则要求在内容进入模型上下文**前**就确定性脱敏，而非依赖模型事后判断。

### 6. #22745 Assess the impact of AST-aware file reads, search, and mapping
**P2 | 7 条评论**
EPIC 级别议题：探索 AST 感知工具在代码库读取/搜索/映射中的价值（精确读取方法边界、减少 token 噪声、提升 agent 路径导航效率）。代表社区对 agent 代码理解质量的深层次提升方向。

### 7. #21968 Gemini does not use skills and sub-agents enough
**P2 | 6 条评论**
有用户指出 Gemini 在拥有"gradle""git"等自定义 skills 的情况下仍不会自主调用，只有在明确指示时才使用——大幅削弱了自定义扩展的实际价值。对应 workstream-rollup 正在跟进 agent 编排策略改进。

### 8. #22232 Enhance browser_agent resilience: Automatic session takeover and lock recovery
**P3 | 4 条评论**
`browser_agent` 的 fail-fast 策略在遇到锁定的浏览器 profile 时过于脆弱。当使用 persistent session 但遭遇已有实例或孤儿进程时直接失败退出，期望能自动接管会话或恢复锁，属于浏览器 Agent 生产可用的关键稳定性提升。

### 9. #22672 Agent should stop/discourage destructive behavior
**P2 | 3 条评论 | 1 👍**
涉及复杂 git 操作、分支管理等场景时，模型可能使用 `git reset` 或 `--force` 等破坏性命令，即使存在更安全的替代方案；维护 DB 等资源时需要 agent 理解风险并主动劝阻。安全护栏类需求热度持续上升。

### 10. #20079 ~/.gemini/agents/filename.md is not recognized as an agent if filename.md is a symlink
**P2 | 4 条评论**
如果 `~/.gemini/agents/` 下代理文件是 symlink，CLI 会识别失败而无法作为 subagent 使用。影响复用 dotfiles 管理自定义 agent 工作流的用户。

## 重要 PR 进展（10 个）

### 1. #29172 feat(core): add support for gemini-3.8-flash as default flash model
**[OPEN] size/l**
注册 `gemini-3.5-flash-lite`、`3.6-flash`、`3.7-flash`、`3.8-flash` 为可选模型，并把 3.8-flash 设为默认 flash 模型。直接回应了 #29164 等模型选择器缺失问题，是今日关注度最高的新功能 PR。

### 2. #29184 fix(core): validate git args in Windows sandbox to block silent `git diff --output`
**[OPEN] area/security, size/m**
Windows 上 `git status | log | diff | show | branch` 均被当作只读命令一概不弹确认框，导致 `git diff --output=<path>` 可在默认模式下静默截断/覆写任意文件。通过在沙箱内严格校验 git 参数来修复，是一个真实可利用的 Windows 安全漏洞修复。

### 3. #29192 fix(checkpoint): contain legacy raw tag path inside checkpoints directory
**[OPEN] P1, area/security, size/m**
`/chat delete <tag>` 在 legacy raw-tag 兼容分支直接用用户输入拼 `path.join`，`../` 可穿越离开 checkpoints 目录删除任意文件。对检查点文件路径做了边界约束，P1 安全问题，需尽快合入。

### 4. #29195 fix(checkpoint): degrade non-array history instead of crashing resume
**[OPEN] P2, size/s**
checkpoint 文件 JSON 合法但 `history` 非数组时 `/resume` 直接崩出 `TypeError`；现在与 unparseable 文件走同路径：降级为空 checkpoint。提升会话恢复健壮性。

### 5. #28930 fix(core): drop unsafe `diff.external` override (#28928)
**[CLOSED] P1, size/m**
此前为禁用外部 diff 工具而加入 `['diff.external', '']`，但 git 不会把空值当成"禁用"，反而会把它作为可执行路径去解析，引发安全问题。**已关闭（被合入或废弃）**，消除了这一环境变量错误配置。

### 6. #28938 fix(core): keep GIT_CONFIG_* environment triplets internally consistent
**[CLOSED] P1, size/l**
修掉脱敏 `GIT_CONFIG_*` 环境变量时若只去掉编号键值对中的一半，会让 git 无法解析；同时防止 ShellExecutionService 在消毒后恢复敏感 git 配置。**已关闭**。

### 7. #29106 fix(core): flush final SSE event on EOF without trailing blank line
**[OPEN] size/m**
SSE parser 在流结束但无空行（如连接截断或代理不合规）时静默丢弃最后一个缓冲事件，可能导致 `finishReason` 和 usage 元数据丢失且无任何报错。修复后可在 EOF 时正确冲刷，提高遥测完整性。

### 8. #29110 fix(core): route read_file content through FileSystemService
**[OPEN] area/agent, size/l**
`read_file` 绕过注入的 `FileSystemService` 直读本地磁盘，而 `write_file`/`replace` 已统一走该服务。这会导致 ACP 客户端声称支持 `fs: readTextFile` 但实际拦截不到 `read_file` 的 I/O 不一致问题，使 `read_file` 行为与 `write_file` 对齐。

### 9. #29115 fix(config): enforce strict permission and ownership checks on system-wide configuration paths
**[OPEN] size/l**
对 Windows 和 POSIX 上的系统级配置文件在加载前先做文件所有权与 ACL 验证（Windows 走 PowerShell ACL 检查），强化配置加载链路的抗篡改能力。

### 10. #29185 test(integration): deflake run_shell_command and file-system-interactive tests
**[OPEN] size/s**
耗时较长的 E2E 集成测试（`run_shell_command.test.ts` 与 `file-system-interactive.test.ts`）偶发失败，处理得不够规范。PR 针对不可靠测试做了修复，属于可观测性与 CI 稳定性改进，利于后续迭代提速。

## 功能需求趋势

从近 24 小时的 Issues 看，社区关注方向集中在以下五个方面：

1. **新模型支持与"默认模型"及时性**：模型选择器看不到 3.6/3.7/3.8 flash（#29164），侧面印证官方模型发布与 CLI 模型注册之间有延迟，用户对新模型永远"先人一步"的期待值很高。
2. **子代理可靠性**：多个 P1/P2 聚焦 subagent 的失败模式——误报成功（#22323）、无限挂起（#21409）、与用户自定义扩展（skills/subagents）自驱力不足（#21968）。方向：不只要修 bug，还要让 agent 更主动、更聪明地使用自身工具链。
3. **安全意识显著提升**：#26525（Auto Memory 脱敏发生在模型上下文之后）、#26523/#26522（无效 patch 静默跳过与低信号 session 无限重试）揭示 Auto Memory 的质量与安全缺陷；gundermanc、SandyTao520 两人贡献了追踪型 issue。沙箱安全（#29184、#29192、#29115、#29116）等 PR 形成围攻态势。
4. **AST 感知的代码库理解工具**：#22745/#22746 组成的 EPIC 意图用 AST 感知工具做 method-bound 精确读取、语义检索和 codebase mapping，以解决 agent 读文件的 token 浪费与"阅读噪声"问题。可能重塑 `codebase_investigator` 的整体工作方式。
5. **Windows 与本地化体验专项修复**：#20079（symlink 代理）、#21983（Wayland 下 browser_agent 失败）、#29184（Windows git 沙箱）、#29116（NTFS 8.3 短文件名绕过），形成对"非 Linux/macOS 默认环境"体验的系统性补强。

## 开发者关注点（痛点高频汇总）

- **"等新模型"**：模型选择器更新滞后，开发者期望发布当天即可选用最新 flash 模型。#29164（12 👍）与该 PR #29172 的直接对应关系值得继续追踪。
- **"卡住不动"成为最高频故障**：generalist agent 无限 hang（#21409）与 shell 命令"拿不到退出信号"（#25166）是最影响体验的两类问题，会让开发者直接放弃当前会话。大量 P1 + 多评论高赞，释放出对 agent 状态机的强不满信号。
- **"不知道子代理里发生了什么"**：bugreport/chat share 缺少 subagent 轨迹（#21763、#22598），同时子代理的终止原因可以"谎报"（#22323），这对依赖 agent 做自动化重构的开发者是致命的信任危机。
- **Windows 与特殊环境长期"二等公民"**：从 sandbox 到 git diff 参数、NTFS 8.3 短名绕过与 symlink 处理，不同平台的行为不一致正在消耗社区大量排障精力。
- **对"安全默认值"的持续诉求**：Agent 应主动阻止 `reset --force` / `rm -rf` 类破坏性操作（#22672），配置文件与 API key 的暴露面要最小化，Auto Memory 的理想形态是"确定性脱敏 + 无效数据透明处理"（#26516 汇总了大量相关 item）。

---
*本日报数据来源于 google-gemini/gemini-cli 仓库公开 Issues/PRs，按过去 24 小时更新活跃度排序。*

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI 社区动态日报 — 2026-09-04

## 1. 今日速览

Copilot CLI 过去 24 小时发布两个热更新：v1.0.83-4 新增 MCP OAuth CIMD 支持，v1.0.83-5 则强化了 macOS/Linux 沙箱隔离并新增 Windows 11 任务栏实时会话卡片。社区侧，MCP 生态兼容性、OAuth 令牌复用、超长会话恢复稳定性是当前讨论最集中的议题，另有若干新提交的稳定性与权限策略相关 Issue 值得关注。

## 2. 版本发布

### v1.0.83-5
- **新增**：Windows 11 任务栏支持显示进行中的 Copilot 会话，并通过悬停卡片提供实时状态。
- **改进**：macOS/Linux 上沙箱命令无法再访问本机服务（macOS 也阻断命令自身启动在 127.0.0.1 的服务器，避免测试套件冲突）。

### v1.0.83-4
- **新增**：为 MCP OAuth 登录流程添加 Client ID Metadata Document（CIMD）支持。
- **改进**：CLI 启动时默认不再弹出中断会话恢复提示；恢复大型会话时输入框响应更快。
- **修复**：沙箱文件工具现在读取与开发者工具一致的配置路径，消除行为不一致。

## 3. 社区热点 Issues

以下为过去 24 小时更新最频繁、讨论度最高或点赞较多的问题，按关注度挑选 10 条：

### #4525 — MCP 协议版本不一致导致 `initialize` 失败
- **作者**：dmbutko ｜ 评论 6 ｜ 👍 3
- **链接**：https://github.com/github/copilot-cli/issues/4525
- **要点**：CLI 已用现代的 `server/discover` 探测成功（协议版本 2026-07-28），随后却仍发送旧版 `initialize`，导致 Python MCP SDK 2.0.0 服务器返回 -32022。
- **为何重要**：MCP 双协议过渡期兼容问题，直接影响 stdio 类型 MCP 服务器用户，修复优先级高。

### #3442 — 企业远程会话被组织策略禁用（v1.0.51）
- **作者**：aarondglover ｜ 评论 6 ｜ 👍 10
- **链接**：https://github.com/github/copilot-cli/issues/3442
- **要点**：用户无论是否显式开启远程会话，CLI 都提示需联系管理员启用；判断逻辑疑似误伤非托管用户。
- **为何重要**：企业用户受阻塞、获赞多，至今未关闭说明仍在影响一批组织用户。

### #2861 — Compaction 持续收到模型空响应
- **作者**：ronkeele ｜ 评论 5 ｜ 👍 4
- **链接**：https://github.com/github/copilot-cli/issues/2861
- **要点**：Opus 4.6 上手动 `/compact` 连续三次返回空响应，重试无效；用户称只在短会话（<30 轮）中出现。
- **为何重要**：上下文压缩是长会话核心能力，此类问题会造成会话不可恢复中断。

### #4695 — MCP OAuth 令牌跨会话复用失效
- **作者**：DaveHolden2025 ｜ 评论 5
- **链接**：https://github.com/github/copilot-cli/issues/4695
- **要点**：HTTP 类型 MCP 服务器（PKCE、public client）的令牌缓存键不稳定，CLI 反复重新走 OAuth 流程；用户判断缓存键哈希计算有误。
- **为何重要**：与 v1.0.83-4 新增的 CIMD 支持同属 MCP OAuth 优化方向，属于热门领域常见摩擦点。

### #232 — 请求增加 --system-prompt 参数
- **作者**：DevelopmentCats ｜ 评论 4 ｜ 👍 10
- **链接**：https://github.com/github/copilot-cli/issues/232
- **要点**：除仓库内 REPO 级 instruction 文件外，社区希望能在 CLI 启动时直接注入系统级指令。
- **为何重要**：历史与点赞数较高，开发者对“全局可控的系统提示”需求长期存在。

### #4218 — Auto 模式模型池不可配置
- **作者**：ecmusick ｜ 评论 1 ｜ 👍 13
- **链接**：https://github.com/github/copilot-cli/issues/4218
- **要点**：Auto 模式会在套餐/组织策略允许的全部模型中挑选，用户无法限定范围，导致成本与行为不可预期。
- **为何重要**：本期所有 Issue 中点赞最高，反映企业对模型选择权与控制力的明确诉求。

### #4699 — 长会话 --resume 后 V8 堆内存溢出崩溃
- **作者**：pedoch ｜ 评论 1 ｜ 👍 2
- **链接**：https://github.com/github/copilot-cli/issues/4699
- **要点**：1.0.82 在长会话中反复达到 4GiB 堆上限后崩溃；Node 诊断报告直接写入用户当前工作目录。
- **为何重要**：大模型会话历史增长带来的内存问题成为稳定性的主要威胁，且“崩溃转储污染用户目录”这一影响被多人关注。

### #4683 — PowerShell ConstrainedLanguage 模式引发杂散报错
- **作者**：Lerri-Cofannos ｜ 评论 2
- **链接**：https://github.com/github/copilot-cli/issues/4683
- **要点**：CLI 追加的退出码探针调用 `$host.SetShouldExit()` 在 ConstrainedLanguage 模式下被策略拦截，导致每个命令都打印一段错误块。
- **为何重要**：受 AppLocker/WDAC 管控的企业 Windows 环境使用体验受影响严重。

### #4655 — Agent Plugins 1.0 自定义 agent 未被发现
- **作者**：mcollier ｜ 评论 3
- **链接**：https://github.com/github/copilot-cli/issues/4655
- **要点**：插件已包含 skills、MCP servers 与 copilot 专属自定义 agents，但 `com.github.copilot/agents` 目录下自定义组件未被识别。
- **为何重要**：插件生态是 Copilot CLI 能力扩展的关键路径，兼容性问题会拖慢采用速度。

### #4717 — 超大会话历史导致扩展启动失败
- **作者**：MattPD ｜ 评论 0（当天新提交）
- **链接**：https://github.com/github/copilot-cli/issues/4717
- **要点**：序列化事件历史超过 V8 最大字符串长度后，扩展 `joinSession()` 调用失败，整个会话扩展能力不可用。
- **为何重要**：与会话体量、内存上限相关的边界问题集中出现，代表规模化会话场景已成为核心质量瓶颈。

## 4. 重要 PR 进展

过去 24 小时内没有合并或更新的 Pull Requests（共 0 条）。当前社区提交集中在 Issue 反馈与版本热修复上，值得留意的是 #4699、#4714、#4717 等大会话稳定性问题的修复预计会在未来数日的 patch release 中跟进。

## 5. 功能需求趋势

从本期所有 Issues 中可提炼出五条高价值需求方向：

1. **模型编排灵活性**
   - 代表：#4218（Auto 模型池配置）、#4703（per-agent provider / 双端点）、#232（系统提示词参数）
   - 社区希望从“CLI 替我做主”向“CLI 听我指挥”演进，核心诉求是显式控制 —— 限定 Auto 模型范围、为不同 agent 指定不同模型端点、注入全局系统指令。

2. **MCP 生态稳定性与无缝体验**
   - 代表：#4525（协议协商失败）、#4695（OAuth 令牌复用）、#4655（插件内 agent 发现）
   - MCP 已进入生产实用阶段，社区注意力从“能否连上”转向“能否稳定、静默地连上”，尤其在 OAuth 和混合协议版本场景下。

3. **长会话/大会话的健壮性**
   - 代表：#4699（OOM）、#4717（扩展加载失败）、#4714（恢复无 UI/极慢）、#4670（扩展崩溃后挂死）、#2861（compaction 空响应）
   - 4GiB 堆上限、V8 字符串长度限制、序列化体积不断暴露设计边界；这是当前最集中的质量短板方向。

4. **Windows 与企业策略合规体验**
   - 代表：#4683（ConstrainedLanguage 报错）、#4702（路径分隔符去重失效）、#4701（长路径截断）、#3442（远程会话管控误判）
   - 企业安全策略覆盖下的 Copilot CLI 仍存在较多“策略与工具冲突”的边角问题，高频出现说明 Windows + 企业管控是最具挑战的组合场景。

5. **资源占用与后台行为可观测性**
   - 代表：#4710（copilot-file-search 空转 CPU）、#4699（崩溃转储写入 cwd）、#4696（allow-all 权限静默重置）
   - 用户希望 CLI 空闲时不要占用 CPU/磁盘，且任何状态变化（如权限丢失）都应有明确的可观测提示。

## 6. 开发者关注点

**稳定性压倒一切**
目前单日内已有 10+ 条 Issue 指向会话恢复、崩溃、挂起和内存问题，说明开发者对 Copilot CLI 的核心场景——数小时连续使用、跨多次会话恢复——有着不容妥协的稳定性预期。

**自动化安全机制需要“可解释、可退出”**
沙箱限制、权限重置和受限语言模式是好事，但当它们静默改动行为、或在每个命令后追加报错时，开发者会产生强烈挫败感。反馈建议集中在：更清晰的提示、可配置的豁免路径、避免误报。

**企业对可控性的诉求升高**
从模型池配置（#4218）、插件市场屏蔽（#4715）、远程会话管控（#3442）到遥测头拦截（#4669）——企业管理员已不满足于“功能可用”，而是要求 Copilot CLI 可纳入自身安全与合规体系，并获得相应的开关能力。

**Windows 平台体验仍是薄弱项**
路径转义、ConstrainedLanguage、长路径截断、任务栏集成（新版本已回应）等分层问题密集出现，反映出 Windows 受支持度已逐渐成为团队采用率的实际门槛。

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI 社区动态日报（2026-09-04）

## 1. 今日速览

今日无新版本发布。过去 24 小时更新窗口内共有 7 个 Issue 和 1 个 PR 发生变化，社区讨论重心集中在**MCP 稳定性、子代理任务中断、本地 Skills 管理**，以及最新的 **v1.17+ ACP 认证门禁阻碍自定义 Provider** 问题。其中 #2633 是当前唯一仍然处于打开状态的高影响 Issue，值得关注。

## 2. 版本发布

今日无新版本发布。

## 3. 社区热点 Issues

> 说明：过去 24 小时更新窗口内共有 7 条 Issue，以下覆盖全部动态，确保无遗漏。

### 🔺 #2633 [OPEN] ACP 认证门禁阻碍无需 Kimi 账号的自定义 Provider

- **作者**: billc8128 | 创建: 2026-09-03 | 更新: 2026-09-03
- **链接**: [MoonshotAI/kimi-cli Issue #2633](https://github.com/MoonshotAI/kimi-cli/issues/2633)

**要点**：自 v1.17.0 起，ACP 服务器的 `session/new`、`session/load`、`session/resume`、`session/prompt` 全部强制要求持久化的 Kimi 账号 OAuth Token。对于使用第三方自定义 Provider 的用户，这相当于引入了一道**不必要的认证门槛**。

**关注理由**：这是当前唯一 Open 的新 Issue，直指 ACP 协议在开放生态中的策略问题。如果团队或企业用户仅想通过 ACP 对接内部模型网关，会被强制要求登录 Kimi 账号，影响面较大。

---

### ✅ #1316 [CLOSED] MCP 超时导致 kimi-cli 整体不可用

- **作者**: Caius1L | 创建: 2026-03-03 | 更新: 2026-09-04
- **链接**: [MoonshotAI/kimi-cli Issue #1316](https://github.com/MoonshotAI/kimi-cli/issues/1316)

**要点**：当某个 MCP Server 连接不上时，会导致整个 kimi-cli 会话被直接中断；即使配置了多个 MCP，单一节点故障也会拖垮全部 CLI。

**关注理由**：该 Issue 在 9 月 4 日被更新并关闭，大概率已通过新版本修复或给出规避方案。MCP 故障隔离能力对重度使用 MCP 的开发者体验至关重要，值得验证是否真正解决了根因。

---

### ✅ #1315 [CLOSED] 按 ESC 后 Subagent 仍然继续运行

- **作者**: chriswingler | 创建: 2026-03-03 | 更新: 2026-09-04
- **链接**: [MoonshotAI/kimi-cli Issue #1315](https://github.com/MoonshotAI/kimi-cli/issues/1315)

**要点**：用户按下 ESC 中断主任务后，后台 Task/Subagent 进程没有被同步取消，继续消耗资源或产生副作用。

**关注理由**：任务取消是终端 AI Agent 的基本安全能力之一。如果主任务结束但子代理仍运行，会显著增加用户对 Agent 失控的担忧。该 Issue 关闭说明团队已对取消机制进行了处理。

---

### ✅ #1320 [CLOSED] 多行输入模式下智能化方向键导航

- **作者**: imbecile-gulu | 创建: 2026-03-03 | 更新: 2026-09-04
- **链接**: [MoonshotAI/kimi-cli Issue #1320](https://github.com/MoonshotAI/kimi-cli/issues/1320)

**要点**：当输入多行文本时，Up/Down 方向键始终用于浏览历史命令，无法在光标位于多行文本中间时进行上下行移动。

**关注理由**：这属于编辑器细节体验问题。随着 CLI 中多行输入与复杂指令越来越常见，方向键行为需要更智能地根据光标位置决定“历史浏览”还是“光标导航”。

---

### ✅ #1319 [CLOSED] 缺少本地 Skills 操作与管理方法

- **作者**: Mocuishler | 创建: 2026-03-03 | 更新: 2026-09-04
- **链接**: [MoonshotAI/kimi-cli Issue #1319](https://github.com/MoonshotAI/kimi-cli/issues/1319)

**要点**：目前内置了 `/skill:kimi-cli-help` 和 `/skill:skill-creator`，但对用户自建 Skill 缺少类似 `/mcp` 的集中管理能力，如查看版本、触发词、删除 Skill 等。此外 Skill 的存储目录不统一。

**关注理由**：Skill 是 Agent 能力的核心扩展单元。社区希望以更一致的方式对本地 Skill 做全生命周期管理。这个需求预计会推动后续 CLI 增加 `skills list`、`skills rm` 之类的子命令。

---

### ✅ #1313 [CLOSED] 增加 Hooks 系统，用于通知与生命周期事件

- **作者**: AungMyoKyaw | 创建: 2026-03-03 | 更新: 2026-09-04 | 👍: 3
- **链接**: [MoonshotAI/kimi-cli Issue #1313](https://github.com/MoonshotAI/kimi-cli/issues/1313)

**要点**：当 Agent 执行长任务（分析大型代码库、构建、测试等）时，用户往往会切换到其他窗口，缺少通知或回调机制来提醒“Agent 需要人工介入”。

**关注理由**：Hooks 系统代表着从“实时盯着终端”到“异步协作”的关键能力。该 Issue 获得 3 个 👍，说明对长时间后台任务、CI 联动、桌面通知有明确诉求。

---

### ✅ #290 [CLOSED] OpenRouter 自定义模型返回 401

- **作者**: Iwoooooods | 创建: 2025-11-14 | 更新: 2026-09-03 | 评论: 3
- **链接**: [MoonshotAI/kimi-cli Issue #290](https://github.com/MoonshotAI/kimi-cli/issues/290)

**要点**：使用 v0.54 通过 OpenRouter 调用 `openai/gpt-5.1-codex` 时返回 401。该问题横跨时间很长，最终在本次窗口关闭。

**关注理由**：老 Issue 的关闭说明自定义模型认证相关问题可能已经解决或演进；但它与 #2633 共同指向一个长期痛点：**Kimi CLI 对第三方模型/网关的接入依然不够顺畅**。

## 4. 重要 PR 进展

> 过去 24 小时更新窗口内仅有 1 个 PR。

### ✅ #2332 [CLOSED] feat: 动态钳制 Completion Budget

- **作者**: wbxl2000 | 创建: 2026-05-20 | 更新: 2026-09-03
- **链接**: [MoonshotAI/kimi-cli PR #2332](https://github.com/MoonshotAI/kimi-cli/pull/2332)

**改动摘要**：
- 移除 Kimi provider 路径中硬编码的 `max_tokens = 32000`；
- 改为根据当前上下文窗口动态计算 `max_completion_tokens`；
- 优先使用 Kimi 的 `max_completion_tokens` 请求参数。

**关注理由**：
1. 硬编码 32K 上限在长上下文场景下容易造成浪费或请求失败；
2. 动态预算可以根据实际对话长度和模型上下文窗口更合理地分配 token；
3. 该 PR 关闭后，后续版本更有可能兼容不同上下文窗口的模型配置，对依赖长上下文任务的开发者是正向更新。

## 5. 功能需求趋势

从近期活跃 Issue 中，可以看出社区对 Kimi Code CLI 的需求正从“基础可用”走向“平台化与工程化”，核心趋势包括：

- **自定义 Provider 与开放认证**  
  #2633 和 #290 表明，越来越多用户希望将 Kimi CLI 作为通用 Agent 前端，接入自己的模型网关、OpenRouter、企业内部模型，而不是被绑定在 Kimi 账号体系内。

- **本地 Skill 全生命周期管理**  
  #1319 提出需要类似 `skills list`、`skills rm` 的管理命令，说明 Skill 生态正在壮大，用户要求一致的存储规范和集中管理入口。

- **事件驱动与自动化**  
  #1313 要求 Hooks 系统，用于任务完成、人工介入、失败回调等场景。参考 Claude Code 等竞品，事件系统已成为复杂 Agent 工具的必要组件。

- **任务中断与资源回收**  
  #1315 反映出，用户不仅需要生成任务，也需要可靠地取消任务，尤其是 Subagent/Task 的级联终止。

- **编辑器交互精细化**  
  #1320 的多行输入导航问题看起来小，但直接影响长篇指令粘贴和复杂 prompt 编辑体验，属于“高频低敏”的编辑器核心体验优化。

## 6. 开发者关注点

综合本次窗口中的开发者反馈，高频痛点集中在以下方面：

1. **容错与故障隔离**：单个 MCP Server 超时导致 CLI 整体崩溃，开发者希望 Agent 具备更强的降级能力，而非“一损俱损”。
2. **可靠的任务取消**：ESC 后 Subagent 仍在运行，说明中断信号没有完整贯穿 Agent 执行树。对于长时间自动化任务，这会带来资源浪费和意外副作用风险。
3. **认证系统不应成为第三方模型墙**：v1.17+ ACP 要求强制 Kimi OAuth，让只想用自定义 provider 的用户无法绕过。该问题被评为当前最需要关注的开放问题。
4. **本地可管理性**：Skill 的数量一旦增加，缺少统一的“查看/删除/触发词管理”入口会严重降低可用性。
5. **后台感知与通知**：用户期望 CLI 能主动通过 hooks 通知任务状态，而不是依赖开发者手动盯屏，这表明 Kimi CLI 正出现在更多“非阻塞式”工作流中。

> 总体来看，社区对 Kimi Code CLI 的功能丰富度和稳定性有较高期待，目前正在经历从“单体对话工具”向“可编程、可扩展、可自动化 Agent 平台”过渡的关键阶段。建议关注 #2633 的官方回复，以及后续版本中关于 Hooks 系统与 Skill 管理能力的落地情况。

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode 社区动态日报（2026-09-04）

## 今日速览

过去 24 小时内 OpenCode 无新版本发布，社区主要精力集中在功能迭代与质量修复上。长期悬而未决的 #266「Gemini 编辑工具兼容性差」持续发酵，以 39 条评论成为今日讨论热度最高议题；多智能体编排、动态工作流、模型成本控制依旧是功能需求主旋律。PR 侧则呈现出明显的功能扩张态势：shell 后台执行、桌面插件管理器、浏览器自动化插件等一批重量级 PR 正在推进。

---

## 社区热点 Issues（10 个）

### 1. Gemini 无法很好处理 edit 工具——长期高热问题
**#266** | 创建 2025-06-20 | 更新 2026-09-04 | 讨论 39 条 | 👍 17
[查看 Issue](https://github.com/anomalyco/opencode/issues/266)

社区长期讨论的焦点问题：Gemini 模型在使用 `edit` 工具时频繁报错 `oldString not found in file`，且对空白符和换行差异极为敏感。用户建议引入空白归一化处理来缓解该问题。这是目前 Issue 区评论数最高、生命周期最长的话题之一，说明多模型兼容性仍是核心痛点。

### 2. 多智能体编排与隔离工作区支持
**#17994** | 状态 CLOSED | 讨论 24 条 | 👍 2
[查看 Issue](https://github.com/anomalyco/opencode/issues/17994)

请求内置运行「agent 团队」并在隔离工作区中协同编码的能力。24 条评论反映出社区对 OpenCode 从单智能体走向多智能体协同的强烈期待，该方向也是本期多个 Issue 的共同主线。

### 3. 仿 Claude Code 的动态工作流功能
**#29059** | 状态 CLOSED | 讨论 17 条 | 👍 22
[查看 Issue](https://github.com/anomalyco/opencode/issues/29059)

获得 22 个 👍，是本期点赞数最高的需求类 Issue。用户希望引入项目级动态工作流，支持可复用的多步骤自动化，直接对标 Claude Code 新功能。需求聚焦在「本地化」「可复用」「多步骤」三个关键词上。

### 4. GLM-5.1 prompt cache 随机归零导致成本飙升
**#31348** | 状态 CLOSED | 讨论 7 条 | 👍 7
[查看 Issue](https://github.com/anomalyco/opencode/issues/31348)

在长会话 agent 工作流中，GLM-5.1 的 prompt cache 读取会随机降至 0，而 DeepSeek V4 Flash 表现稳定。对依赖缓存控制成本的用户来说，这是极具杀伤力的可靠性问题，7 个 👍 说明不少用户同样受到影响。

### 5. Task 工具应支持子代理模型参数
**#26925** | 状态 CLOSED | 讨论 3 条 | 👍 3
[查看 Issue](https://github.com/anomalyco/opencode/issues/26925)

当 OpenCode 作为编排者启动多个子代理时，`subagent_type` 只能选 `general` 或 `explore`，无法按任务指定模型，导致成本无法精细控制。与 #17994 形成互补：多智能体编排不仅要有，还要能灵活配置。

### 6. 非 git 项目将 "/" 作为 worktree，破坏权限路径解析
**#24694** | 状态 CLOSED | 讨论 6 条 | 👍 3
[查看 Issue](https://github.com/anomalyco/opencode/issues/24694)

在非 git 目录下运行时，`Project.fromDirectory` 将 worktree 设为 "/"，导致权限路径解析错乱。暴露出 OpenCode 对非 git 工作目录的适配短板，可能引发越权或误判问题，属于基础能力缺陷。

### 7. WSL 下安装失败：postinstall 语法错误
**#29210** | 状态 CLOSED | 讨论 6 条
[查看 Issue](https://github.com/anomalyco/opencode/issues/29210)

`sudo npm i -g opencode-ai@latest` 在 WSL 环境中触发 postinstall.mjs 语法错误，导致安装直接失败。安装体验问题会显著拉高用户上手门槛，6 条评论说明 WSL 用户群体对该问题较为敏感。

### 8. 桌面版删除工作区文件后渲染器崩溃
**#35493** | 状态 CLOSED | 讨论 3 条
[查看 Issue](https://github.com/anomalyco/opencode/issues/35493)

Windows x64 桌面版 v1.17.13 在时间线引用已删除文件时，渲染进程持续崩溃，报错 `Cannot read properties of undefined (reading '_tag')`。桌面端稳定性问题在本期多个 Issue 中反复出现，已成为用户抱怨的重灾区。

### 9. Nix 构建因 bun.lock 过期失败
**#34117** | 状态 CLOSED | 讨论 4 条 | 👍 4
[查看 Issue](https://github.com/anomalyco/opencode/issues/34117)

`nix build github:anomalyco/opencode/v1.17.11#default` 因 `bun install --frozen-lockfile` 检测到 lockfile 过期而失败。该问题与 #34235 重复报告，影响 Nix 用户从源码构建，暴露出发布流程中 lockfile 同步缺失。

### 10. 中国用户付费后无法使用，要求退款
**#47205** | 状态 OPEN | 创建 2026-09-04 | 标记 [needs:compliance]
[查看 Issue](https://github.com/anomalyco/opencode/issues/47205)

今日新增的特殊 Issue：用户表示在 zen 充值后因中国地区限制无法使用，要求退款。带有 `[needs:compliance]` 标签，属于合规/客户服务范畴。虽非技术问题，但涉及付费可用性与区域政策，值得官方关注处理。

---

## 重要 PR 进展（10 个）

### 1. shell 工具支持后台运行 + 自动通知
**#47187** | 状态 CLOSED
[查看 PR](https://github.com/anomalyco/opencode/pull/47187)

为 shell 工具新增 `run_in_background` 能力。此前长时间运行的命令（dev server、watch、测试等）会阻塞整个 turn，开发者只能借助 `nohup ... &`，但会丢失输出。该 PR 为长任务提供了一等公民的后台执行体验，完成后自动通知，属于工作流效率的重要改进。

### 2. 桌面端插件管理器
**#47180** | 状态 CLOSED
[查看 PR](https://github.com/anomalyco/opencode/pull/47180)

在设置对话框新增 Plugins 标签页，可浏览、安装、管理插件。插件目录整合了官方文档生态页、`awesome-opencode`（10k stars）与 opencode.cafe 市场，并补充 npm 元数据。这将大幅降低桌面用户的插件发现与安装成本。

### 3. AI 工具命名空间
**#46548** | 状态 OPEN
[查看 PR](https://github.com/anomalyco/opencode/pull/46548)

引入递归的、provider 无关的 `ToolEntry` / `ToolNamespace` 定义体系，支持工具树的归一化、去重与预算控制，并为 OpenAI Responses 路由做原生命名空间降级。属于 AI 工具系统的架构级重构。

### 4. 桌面端浏览器标签页与 Chromium 诊断
**#44838** | 状态 OPEN
[查看 PR](https://github.com/anomalyco/opencode/pull/44838)

为桌面端引入多浏览器标签页的打开、聚焦、关闭能力，并在 Review 面板与 agent 工具之间共享标签所有权。同时加入跨域 frame 检查、快照等 Chromium 诊断工具，显著扩展浏览器自动化边界。

### 5. 公共 API 浏览器插件
**#46531** | 状态 OPEN
[查看 PR](https://github.com/anomalyco/opencode/pull/46531)

新增 `@opencode-ai/plugin-browser`，提供 44 个命名空间的 Code Mode 方法，覆盖标签页、交互、快照、文件、诊断、性能分析和审计。实现与合约分离，客户端只依赖纯 RPC 入口，设计较干净。

### 6. 修复 Copilot 请求分类覆盖所有路由
**#47160** | 状态 CLOSED
[查看 PR](https://github.com/anomalyco/opencode/pull/47160)

修复了 Copilot 插件只在本机 AI 路由上设置 `X-Interaction-Type` 头的问题。此前 GPT 等 Copilot 模型走 AI SDK 路由时会绕过中间件，导致标题与压缩等元信息分类错误。

### 7. TUI 启动探测失败时干净退出
**#46726** | 状态 OPEN | 标记 [needs:issue]
[查看 PR](https://github.com/anomalyco/opencode/pull/46726)

当后台服务器处于冷启动或选举阶段时，TUI 的启动探测可能失败。此前行为可能造成卡死或不可预期状态，该 PR 让 TUI 在探测失败时干净退出并给出反馈，提升恶劣条件下的终端体验。

### 8. 事件流客户端重连退避
**#47204** | 状态 OPEN | 标记 [needs:issue]
[查看 PR](https://github.com/anomalyco/opencode/pull/47204)

修复事件流客户端固定 1 秒重连间隔的问题。未认证浏览器会话等场景下，流从未建立时会陷入无意义的快速重连，浪费资源。该 PR 为重连引入退避策略。

### 9. 移除模拟点击的人为延迟
**#47199** | 状态 OPEN
[查看 PR](https://github.com/anomalyco/opencode/pull/47199)

OpenTUI 的测试助手在模拟鼠标点击时默认加入 10ms 级延迟，每 100 次点击会产生约 3 秒额外等待。该 PR 传入 `{ delayMs: 0 }` 移除人工节流，对大规模 UI 模拟与 agent 测试场景有明显性能收益。

### 10. Task 工作树会话迁移建议
**#47202** | 状态 CLOSED
[查看 PR](https://github.com/anomalyco/opencode/pull/47202)

以内置插件提示工程的方式，指导模型在开启 `task` 相关多会话场景时，将会话移入 task worktree。虽只是 prompt 层面的改动，但针对的是多 agent 并发下会话隔离的实际需求，成本低而收益直接。

---

## 功能需求趋势

综合今日所有 Issue，社区最关注的功能方向可归纳为以下六类：

- **多智能体编排与子代理控制**：#17994（隔离工作区多智能体）、#26925（Task 工具子代理模型参数）共同指向一个需求——OpenCode 需要从单 agent 走向可编排、可独立配置的 agent 集群。
- **动态工作流与任务自动化**：#29059 以 22 个 👍 成为最强信号。社区希望获得类似 Claude Code 的项目级动态工作流能力，让多步骤自动化可以沉淀、复用。
- **成本控制与模型路由**：OpenRouter `service_tier` 支持（#28566）、子代理 model 参数（#26925）与 GLM-5.1 prompt cache 异常（#31348）都表明，用户在规模化使用后开始重点关注意外的成本波动与细粒度费用控制。
- **插件 API 深度扩展**：会话 API 暴露（#35443）、`chat.message` 钩子支持阻断/取消（#30434）、TUI 输入前后钩子（#47087）——插件开发者正从「能跑通」走向「可管控、可干预」。
- **浏览器自动化与桌面端能力闭环**：桌面浏览器标签管理（#44838）、公共浏览器插件 API（#46531）、插件权限断言（#46530）共同构建了从桌面 UI 到 agent 工具的完整浏览器操作链路。
- **Worktree 与工作区体验完善**：包括 git worktree CLI flag（#35471）、非 git 目录路径修复（#24694）、Task worktree 会话隔离建议（#47202）。

## 开发者关注点

从今日 Issue 反馈中，高频痛点集中在以下几个方向：

- **模型工具兼容性**：#266 显示 Gemini 在 `edit` 工具上成功率不佳，模型对精确匹配要求过高，用户期望更鲁棒的差异容忍度与空白归一化。
- **成本不可预测**：GLM-5.1 prompt cache 随机归零是「钱的问题」，对长会话开发者伤害极大；而缺少子代理模型参数也让成本优化无从下手。
- **权限系统形同虚设**：#33677 指出 `permission.edit` 设置即便配置了也不会触发确认提示，属于敏感的安全权限缺陷。
- **桌面端稳定性堪忧**：渲染器崩溃（#35493）、工具调用被静默终止（#35485）、配置识别失败导致下拉框为空（#35419），桌面端在 v1.17.13 上暴露的问题较为密集。
- **安装与构建链路脆弱**：WSL 安装语法错误（#29210）、Nix frozen lockfile 失败（#34117）、Homebrew tap 信任警告（#32072），多平台交付一致性仍需加强。
- **回归担忧**：#47184 反映「最新版本变差、反复进入相同循环」，虽然信息不足，但用户体验下降的抱怨值得官方排查对应模型或编排逻辑变更。
- **非 git 工作流支持不足**：#24694 暴露了非 git 目录下的路径解析错乱，对于不使用 git 或处于嵌套目录的用户是硬伤。
- **区域付费可用性**：#47205 提醒官方在处理付费与区域策略时需更谨慎，避免用户付款后无法使用而进入退款流程。

---

*本日报由 AI 技术分析师整理，基于 GitHub 公开数据生成。内容不构成官方立场，仅供参考。*

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

# Pi 社区动态日报（2026-09-04）

## 今日速览

过去 24 小时无新版本发布。社区热点集中在上下文/Token 预算管理、TUI 流式渲染性能、以及扩展注册 provider 的可靠性问题；新增的二进制附件损坏、agent 死循环等 Bug 已被快速关闭。PR 侧则以静态二进制兼容修复、信号退出码修正、模型目录更新和 TUI 交互增强为主。

## 社区热点 Issues

1. **终端无故滚动到会话顶部又快速回到底部** [#5023](https://github.com/earendil-works/pi/issues/5023) `[CLOSED]`  
   18 条评论 / 3 👍。模型生成过程中随机发生，用户侧无操作触发；影响阅读和操作连续性，目前根因未明。

2. **分支摘要固定 maxTokens=2048，大分支必然失败** [#8845](https://github.com/earendil-works/pi/issues/8845) `[CLOSED]`  
   14 条评论。`generateBranchSummary` 硬编码 2048 token 上限，导致大仓库 `/tree` Summarize 确定性报错，应随分支规模或上下文动态调整。

3. **Context budget 未预留输出 token 空间，compact 重试依旧失败** [#8061](https://github.com/earendil-works/pi/issues/8061) `[OPEN] [inprogress]`  
   6 条评论 / 2 👍。在 1,048,576 token 窗口下输入仅占 78% 仍被 provider 拒绝；自动压缩后重试因同样原因失败，对超长会话不友好。

4. **扩展注册的 provider 间歇性忽略 defaultProvider/defaultModel** [#8810](https://github.com/earendil-works/pi/issues/8810) `[OPEN]`  
   3 条评论。新会话启动时随机回退到其他 provider 的默认模型，疑似扩展注册与配置解析存在竞态条件。

5. **二进制附件被强制 UTF-8 解码导致损坏** [#9105](https://github.com/earendil-works/pi/issues/9105) `[CLOSED]`  
   今日新提交。`processFileArguments()` 在 `@file` 语法和 Read 工具中误判二进制内容，会造成数据损坏，值得警惕。

6. **Agent 循环缺少工具调用执行超时** [#8857](https://github.com/earendil-works/pi/issues/8857) `[CLOSED]`  
   2 条评论。bash 等工具若阻塞（如等待数据库连接），整个 run 将无限挂起；LLM 流超时和 bash 单工具超时均不覆盖该阶段。

7. **流式输出 UI 落后于模型，每 delta 触发 O(N²) 全量 Markdown 重渲染** [#8822](https://github.com/earendil-works/pi/issues/8822) `[OPEN]`  
   2 条评论。同步事件路径上逐 delta 重渲染导致界面明显滞后，25 SSE chunk/s 即出现可见卡顿。

8. **请求在 Containerization 文档中增加 Docker Sandbox 章节** [#8788](https://github.com/earendil-works/pi/issues/8788) `[OPEN]`  
   4 条评论。用户希望得到完整的隔离运行方案说明，文档侧需求明确。

9. **gemini-3.8-flash 未收录进 Google/Vertex model catalog** [#9076](https://github.com/earendil-works/pi/issues/9076) `[CLOSED]`  
   3 条评论。新模型发布后内置目录未同步，影响使用 Google 系模型的用户。

10. **PI_OFFLINE 静默禁用全部 provider model discovery，与文档范围不符** [#8684](https://github.com/earendil-works/pi/issues/8684) `[OPEN]`  
    3 条评论。文档只声称关闭启动期联网检查，实际行为影响整个会话的模型目录发现。

## 重要 PR 进展

1. **系统提示重构：支持扩展部分更新** [#8998](https://github.com/earendil-works/pi/pull/8998) `[OPEN]`  
   较大架构改动，允许 coding-agent 在会话中动态发送系统提示更新，减少全量替换带来的上下文干扰。

2. **OpenAI Responses-compatible provider 支持顶层 instructions** [#8734](https://github.com/earendil-works/pi/pull/8734) `[OPEN]`  
   新增 `systemPromptFormat` 选项，将动态系统提示移到 top-level `instructions`，避免与 `input` 重复。

3. **新增 Meta provider，支持 Muse 订阅 OAuth** [#9096](https://github.com/earendil-works/pi/pull/9096) `[OPEN]`  
   接入 Meta 模型；PR 指出其刷新 token 机制特殊、当前流式输出接近“伪流式”。

4. **Linux 下改用静态 musl 版 fd/ripgrep** [#9070](https://github.com/earendil-works/pi/pull/9070) `[CLOSED]`  
   修复 NixOS/Alpine 等系统上因 glibc 动态链接缺失导致 find/grep 工具不可用的问题。

5. **信号杀死子进程映射为非零退出码** [#8994](https://github.com/earendil-works/pi/pull/8994) `[CLOSED]`  
   修复 OOM killer 或信号终止 bash 工具时被误判为执行成功的问题，提升 agent 对失败命令的感知能力。

6. **动态模型 api 无匹配实现时快速失败** [#9087](https://github.com/earendil-works/pi/pull/9087) `[CLOSED]`  
   改善 `openrouter/anthropic/*` 等模型返回巨型 HTML 404 页面的错误体验，改为明确报错。

7. **TUI 新增“跳转到最新”控件** [#9080](https://github.com/earendil-works/pi/pull/9080) `[CLOSED]`  
   配合新消息指示器，用户可一键回到会话最新输出位置。

8. **修复 JPEG 非 EXIF APP1 段扫描逻辑** [#8616](https://github.com/earendil-works/pi/pull/8616) `[CLOSED]`  
   处理 XMP 位于 EXIF 之前的 JPEG，避免图片预处理转换失败。

9. **registerProvider 的 apiKey 支持函数形式** [#9081](https://github.com/earendil-works/pi/pull/9081) `[CLOSED]`  
   允许插件在请求时从自己的 auth file 读取密钥，不再强制依赖 `/login` 核心存储。

10. **文档：containerization.md 增加 Docker Sandboxes 章节** [#9077](https://github.com/earendil-works/pi/pull/9077) `[CLOSED]`  
    响应 #8788，补全 Docker 隔离运行模式说明及选择表。

## 功能需求趋势

- **上下文与 Token 预算管理是最大痛点**：涉及输出 token 预留、分支摘要固定上限、重复 thinkingSignature 导致会话体积膨胀等问题（[#8061](https://github.com/earendil-works/pi/issues/8061)、[#8845](https://github.com/earendil-works/pi/issues/8845)、[#9097](https://github.com/earendil-works/pi/issues/9097)）。
- **模型目录与新版模型接入滞后**：Gemini 3.8 Flash 缺失、Grok Build 0.1 清理、Meta provider 支持等均指向需要更快的 catalog 更新机制（[#9076](https://github.com/earendil-works/pi/issues/9076)、[#9096](https://github.com/earendil-works/pi/pull/9096)、[#9093](https://github.com/earendil-works/pi/pull/9093)、[#9016](https://github.com/earendil-works/pi/issues/9016)）。
- **TUI 渲染与交互体验持续被关注**：流式输出 O(N²) 重渲染、OSC 8 超链接支持、通用 viewport 原语、全屏模式滚动速度等（[#8822](https://github.com/earendil-works/pi/issues/8822)、[#5168](https://github.com/earendil-works/pi/issues/5168)、[#4861](https://github.com/earendil-works/pi/issues/4861)、[#9052](https://github.com/earendil-works/pi/issues/9052)）。
- **扩展生态需要补齐边界能力**：注册 provider 的默认选择优先级、extension 覆盖内置工具、包级命名空间、插件 auth 读取等问题集中出现（[#8810](https://github.com/earendil-works/pi/issues/8810)、[#9071](https://github.com/earendil-works/pi/issues/9071)、[#8834](https://github.com/earendil-works/pi/issues/8834)、[#9079](https://github.com/earendil-works/pi/issues/9079)）。
- **可靠性/自恢复机制不足**：工具调用无超时、信号退出误报成功、agent 陷入死循环重复同一句话等，都是真实使用中“卡死”场景（[#8857](https://github.com/earendil-works/pi/issues/8857)、[#8882](https://github.com/earendil-works/pi/issues/8882)、[#9104](https://github.com/earendil-works/pi/issues/9104)）。

## 开发者关注点

- 多数人遇到了“模型输出被 token cap 拒绝，但上下文压缩逻辑却没有预留输出空间”的矛盾，说明预算控制需要同时考虑输入与输出。
- 本地或扩展接入的 provider（Ollama、OpenRouter、llama.cpp 等）配置兼容性问题较多，尤其是默认模型选择和 error message 可读性。
- 对长时间运行的 agent 任务而言，工具调用缺少全局超时是高风险点；信号被杀却显示成功会严重误导后续 agent 决策。
- 二进制附件被强制转码、`think` 标签被从 tool I/O 中剥离等“底层数据被悄悄改写”的问题，让开发者担心数据完整性。
- 整体来看，维护者响应速度快，许多新提交的 Bug 在 24 小时内被关闭或标记；社区期待这些修复尽快进入下一个正式 release。

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code 社区动态日报 — 2026-09-04

## 今日速览

今日发布 v0.23.0（无破坏性变更），分支选择器新增 git 状态提示。社区讨论最热的是 TUI 渲染层向 OpenTUI 迁移的跟踪 Issue（#8662，28 条评论），以及 thinking/脚手架内容泄漏两类 P2 缺陷（#10791/#10797）——两个修复 PR 均已出现。安全侧，Dependency CVE 审计失败（#10850，P1）与 Bash 允许规则绕过报告（#10197/#10192）持续发酵；CI 导入耗时问题（#10908）也引发了开发效能讨论。

## 版本发布

- **[v0.23.0](https://github.com/QwenLM/qwen-code/releases)**：无已知破坏性变更。已公布改进：分支选择器（Branch Picker）在 Update Project、Commit、Push 等入口旁显示 git 状态提示，如 `↓3 · origin/main`、`Up to date`。完整变更列表见 Release 页。

## 社区热点 Issues

1. **[#8662](https://github.com/QwenLM/qwen-code/issues/8662)（28 评论 · P3 · Tracking）— 将 TUI 渲染层从 ink 迁移至 OpenTUI**
   现有 ink 7 + React 19 之上积累了约 1037 行补丁与自定义 Virtual Viewport，闪烁等结构性问题在 ink 内难以根治。该跟踪 Issue 代表 Terminal UX 路线图的核心架构方向，评论热度全站最高，值得关注迁移进展与回归验证。

2. **[#10065](https://github.com/QwenLM/qwen-code/issues/10065)（8 评论 · P2 · Bug）— LM Studio 0.4.21 请求报 “failed to parse grammar”**
   用户未配置 MCP 与 tools.core=[]，仍无法通过 OpenAI 兼容 API 调用本地模型。反映本地/第三方模型网关的接入稳定性是高频痛点，已标记 ready-for-human。

3. **[#10908](https://github.com/QwenLM/qwen-code/issues/10908)（5 评论 · P2）— CI 时间受模块 import 成本拖累**
   某次 release 运行中 `cli` workspace 的 collect 耗时 2223s、tests 仅 1372s；core 为 546s vs 251s。CI 效率问题直接拖慢发版节奏，对应修复 PR #10915 已提交。

4. **[#10953](https://github.com/QwenLM/qwen-code/issues/10953)（4 评论 · P2 · Bug）— 子代理工作期间 Todo Plan 状态冻结**
   真实工作已推进 4 个节点，但持久化 Todo 在 55m44s 内无更新，active-todo 提醒不触发。暴露长任务/子代理场景下的会话状态同步缺口，与 #10247 Agent Team 质量追踪相关。

5. **[#10791](https://github.com/QwenLM/qwen-code/issues/10791)（4 评论 · P2 · welcome-pr）— 平衡的 content-only `<thinking>` 块仍泄漏到用户输出**
   当前防御仅覆盖未闭合的 thinking 标签；hybrid-thinking 模型把完整 thinking 以字面标签输出到 content 时仍会泄漏。PR #10982 已提出修复方案，建议参与测试验证。

6. **[#9666](https://github.com/QwenLM/qwen-code/issues/9666)（4 评论 · P2 · welcome-pr · Windows）— Windows 终端中文 IME 候选框对比度极低**
   候选字被半透明方框包围、几乎无法辨认，严重影响中文输入效率。属于高频中文用户的核心体验问题，等待贡献者实现。

7. **[#10932](https://github.com/QwenLM/qwen-code/issues/10932)（4 评论 · P2 · ready-for-human）— 语音听写无法使用 Token Plan ASR 模型**
   Model Studio Token Plan 已提供 `qwen-audio-3.0-asr-flash` 系列，但 voice pipeline 仍硬编码旧模型 ID，导致语音转录无法使用 Token Plan。提示音与录音正常，纯属模型 ID 白名单过期。

8. **[#10797](https://github.com/QwenLM/qwen-code/issues/10797)（3 评论 · P2 · welcome-pr）— tool-result 脚手架/system-reminder 标签回显到用户输出**
   模型在 content 中输出伪造的 tool-result XML 块与系统提醒。与 #10791 同属输出净化盲区，PR #10992 已覆盖 Shape A/B，等待合入。

9. **[#10850](https://github.com/QwenLM/qwen-code/issues/10850)（3 评论 · P1 · Security）— Dependency CVE 审计全仓库失败（main lockfile）**
   fast-uri/qs/uuid 新增 4 个漏洞（1 low、2 moderate、1 high），导致 `npm audit --omit=dev` 在 2026-09-02 起全仓库失败。P1 级别安全门禁问题，直接影响 CI 与发布流程。

10. **[#10197](https://github.com/QwenLM/qwen-code/issues/10197)（3 评论 · P1 · Security）— 静态 loader 环境赋值可绕过 Bash allow 规则**
    保存的具体 `Bash(...)` 允许规则在被去除前置环境赋值后仍可匹配，但这些赋值会改变允许程序的运行时语义并产生额外代码执行（同作者还提交了 #10192 命令替换变体）。需加强 shell 工具的安全执行判定。

## 重要 PR 进展

1. **[#10982](https://github.com/QwenLM/qwen-code/pull/10982)（now-ing）— 修复 #10791：平衡的 content-only thinking 块降级为 thought parts**
   扩展 converter 防御，新增 `hasLeadingBalancedThinkingBlock()` 检测，将泄漏的 thinking 内容转为内部 thought 而不再进入用户可见 content。

2. **[#10992](https://github.com/QwenLM/qwen-code/pull/10992)（now-ing）— 修复 #10797：捕获 tool-result 脚手架与 system-reminder 回显泄漏**
   针对“伪造 tool-result XML 块”和“system-reminder 回显”两种新泄漏形状，在现有 sanitizer 基础上补上 Shapes A/B 的拦截逻辑，方向与 issue triage 结论一致。

3. **[#10986](https://github.com/QwenLM/qwen-code/pull/10986)（wenshao · 已关闭）— OpenTUI slash 提交判定改为实时读 editor buffer**
   Enter 键提交时不再依赖渲染 effect 发布的状态，而是直接读取编辑器 buffer，消除了按键后“慢一拍”导致误判的缺陷。

4. **[#10906](https://github.com/QwenLM/qwen-code/pull/10906)（BZ-D）— Web Shell 任务详情展示 Shell 与 Monitor 输出**
   Monitor stdout/stderr 与现有 Shell capture 一起持久化，daemon 暴露 live-session-owner 作用域的 sanitized tail 端点，补全任务输出可观测性。

5. **[#10938](https://github.com/QwenLM/qwen-code/pull/10938)（yiliang114）— Session Workflow 依赖可导航 + 界面收敛**
   Plan DAG 以步骤而非状态为核心进行展示，依赖关系可导航跳转，并对 inspector chrome 做了整体设计收敛，目标对齐 #8583 遗留问题。

6. **[#10962](https://github.com/QwenLM/qwen-code/pull/10962)（wenshao）— Web Shell 将浏览器本地目录桥接为会话文件**
   当 daemon 运行在云盒子/容器/共享主机时，允许用户在浏览器中把本机目录授权给 agent，弥补 daemon 文件系统与开发者本地的隔离。

7. **[#10954](https://github.com/QwenLM/qwen-code/pull/10954)（yiliang114）— `qwen serve` 暴露后台 Agent 列表**
   新增 `GET /background-agents` 端点，返回 Agent View supervisor 正在运行的后台会话、名称与当前状态（state、need…），提升多代理场景的可观测性。

8. **[#10915](https://github.com/QwenLM/qwen-code/pull/10915)（yiliang114）— CI：为所有 workspace 统一共享池测试超时**
   将 vitest 默认 5s 上限提升为共享 ECS 池的超时配置，覆盖其余 15 个 workspace，并加入“防呆”扫描，防止新 workspace 悄悄退回默认超时。

9. **[#10817](https://github.com/QwenLM/qwen-code/pull/10817)（qqqys）— Channels 支持 `messagePrefix` 消息前缀过滤**
   可选配置后，用户消息须以精确前缀开始（可先跳过完整 @mention），并带非空 payload；匹配成功后前缀被剥离再进入 agent。默认关闭，不破坏现行为。

10. **[#10347](https://github.com/QwenLM/qwen-code/pull/10347)（qwen-code-dev-bot · needs-human）— 将 EOF 类网络错误识别为可重试传输错误**
    把实际是底层网络失败的 4xx（如 `400 network error ... EOF`、对端中断）从 fail-fast 改为走 bounded auto-retry，弥补 Ctrl+Y 不可用场景下的自动恢复能力。

## 功能需求趋势

- **终端/交互基础设施重构**：#8662（ink→OpenTUI）、#9666（IME 对比度）与 #9305/#10986 等表明，Terminal UX 架构与中文输入体验是社区最集中的基础方向。
- **模型输出内容净化**：#10791、#10797 已快速催生修复 PR，另有 #10872 提议为 thinking/reasoning 输出增加可插拔中间件（如翻译），预计输出治理还会继续演进。
- **安全与供应链加固**：#10192/#10197/#10561（Bash allow 绕过）、#10850（CVE 审计）显示 shell 规则可信度与依赖漏洞已成为安全类最高优先级话题。
- **第三方模型服务与本地模型兼容**：#10065（LM Studio）、#10932（Token Plan ASR）表明社区希望 Qwen Code 能更平滑地接入新模型服务、新认证套餐与本地网关。
- **会话/多代理状态可视化**：#10953（todo stale）、#10954（background-agent API）、#10989（web-shell 提示权威轮询）共同指向“长时间后台任务需对用户保持透明”。
- **Web Shell / 远程开发扩展**：#10938、#10906、#10962 等 PR 显示 web-shell 正从对话界面扩展为远程文件与任务运维入口。
- **配置灵活性与会话模型**：#10984（per-process `--config-dir`）、#8908（standalone sessions）、#10817（channel prefix）、#8927（sessionRotation）持续扩大 CLI/daemon 的可配置边界。

## 开发者关注点

- **本地模型链路兼容性脆弱**：LM Studio 在无 MCP、无 tools 条件下仍报 grammar 解析错误，用户期待 OpenAI 兼容层更稳健及报错信息可诊断。
- **Shell 安全规则可信度**：P1 报告指出 allow 规则可被环境赋值或命令替换绕过，开发者对工具的“确认/允许”判定机制安全边界高度敏感。
- **长任务状态与提示不同步**：子代理推进时 todo 冻结、Web Shell 侧栏 spinner 丢失，用户在长时间运行中难以判断任务真实状态，期望以 daemon 权威状态为准而非本地猜测。
- **Windows 中文输入体验差**：IME 候选框对比度过低直接影响日常可用性，相关 issue 已标 `welcome-pr`，社区期待贡献者接手。
- **模型输出“标签残留”影响观感**：thinking/tool-result 标签进入用户可见内容，即使不影响功能也严重干扰阅读，两个修复 PR 需要更多测试确认。
- **CI 基础成本拖累发布**：模块 import（collect 2223s）耗时高于实际测试（1372s），开发者希望测试基础设施与超时策略更贴近真实耗时。
- **文档/配置希望与新套餐同步**：Token Plan 的 endpoint、env key、settings.json 示例需要文档补齐（#10620），且需要支持进程级独立配置目录以隔离不同项目环境（#10984）。

—— 数据来源：github.com/QwenLM/qwen-code · 日报生成时间：2026-09-04

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI 社区动态日报 — 2026-09-04

> 注：本次抓取的数据源中 GitHub 链接统一指向 Hmbown/Codewhale 仓库（与所给数据原样一致）。

## 1. 今日速览

今日社区焦点集中在 `serve --acp` 的会话能力缺口上——两个新 Issue 指出 ACP 客户端无法枚举/恢复会话、也无法读取或切换 session 配置，是编辑器对接 Codewhale 的明显短板。代码侧则有多项新 PR 落地，涉及 shell 任务来源追踪、OpenCode Go/Zen provider 的 `x-opencode-session` 请求头、以及可配置的 reasoning-only 重试逻辑。此外，0.9.12 的 Fleet-only UX 大合入 PR 已于昨天关闭，主题折叠等 TUI 改动进入稳定阶段。

- 4 个活跃 Issues（2 个涉及 ACP 会话能力缺陷，1 个总领 EPIC，1 个为外部垃圾广告）
- 8 个 PR 在过去 24 小时有更新，其中 4 个已 CLOSED，4 个仍 OPEN
- 无新版本 Release 发布

## 2. 版本发布

过去 24 小时没有新的 Release 发布。上一个里程碑 PR（#5862）表明 0.9.12 正在准备中，包含大量 Fleet-only UX 改进。

## 3. 社区热点 Issues

当日活跃 Issue 共 4 条，全部列出（数据不足 10 条）：

### #5316 EPIC-005：CodeWhale TUI Crate Decomposition（Umbrella）
- **状态**: OPEN | 创建: 2026-08-10 | 更新: 2026-09-03 | 评论: 21
- **链接**: [#5316](https://github.com/Hmbown/Codewhale/issues/5316)
- **重要性**: 这是 TUI 命令分解工作的总领跟踪 Issue，所有子 EPIC 与 FEAT 完成时均需向此报告，同时所有相关 PR 也需在此登记。评论数高达 21 条，说明它是整个重构工作的中枢，新的 FEAT 子任务仍会持续跟进。近期 `#5865`（FEAT-020 plugin command shapes）也需要挂靠到该 EPIC 下。

### #5863 [enhancement] ACP 功能增强：`serve --acp` 未暴露 session 配置选项
- **状态**: OPEN | 创建/更新: 2026-09-03 | 评论: 2
- **链接**: [#5863](https://github.com/Hmbown/Codewhale/issues/5863)
- **重要性**: `serve --acp` 不暴露 session config 选项（modes / models / configOptions），导致编辑器客户端无法显示或修改当前工作模式。环境信息表明该 Issue 来自 `codewhale` npm wrapper 0.9.11（Linux x64），是一个非常具体的集成层缺口。由于 comment 已有 2 条且是同一天创建，开发者可能很快跟进。这直接影响了 ACP 客户端作为前端 IDE 的用户体验。

### #5864 `serve --acp` 未实现 ACP `session/list` / `session/load`
- **状态**: OPEN | 创建/更新: 2026-09-03 | 评论: 1
- **链接**: [#5864](https://github.com/Hmbown/Codewhale/issues/5864)
- **重要性**: ACP 客户端无法枚举或恢复已有 Codewhale 会话（环境为 Windows x64 + codewhale 0.9.11）。对于一个以"继续对话"为核心场景的 CLI/Agent UI 工具而言，缺少 session 持久化恢复能力会迫使客户端每轮都从新会话开始。与 #5863 互为补充，说明 ACP 协议的服务端实现仍在早期阶段。

### #5866 Key Ophthalmology CPT & ICD-10 Updates for 2026（垃圾信息）
- **状态**: OPEN | 创建/更新: 2026-09-03 | 评论: 1
- **链接**: [#5866](https://github.com/Hmbown/Codewhale/issues/5866)
- **重要性**: 该 Issue 内容为眼科医疗编码广告，与 Codewhale 项目无关，是典型的垃圾 Issue（SPAM）。此类噪音会分散维护者注意力，社区评论已有 1 条，管理员可能需要将其关闭或标记为 SPAM。

## 4. 重要 PR 进展

过去 24 小时有更新的 PR 共 8 条，全部列出：

### #5869 fix(shell): preserve task origin in job snapshots（OPEN）
- **作者**: zhuowp | 更新: 2026-09-04 | 评论: 0 | 👍: 0
- **链接**: [#5869](https://github.com/Hmbown/Codewhale/pull/5869)
- **功能**: 修复后台 shell job 快照与完成事件缺少稳定 origin 标识符的问题。此前同一 session 存在多个 job 时，host 只能靠命令文本等启发式方法匹配更新结果，可能将较早 job 的 error 输出错误地投射到较新的 tool card 上。该修复有助于在多会话、多 job 场景下保持 UI 语义准确。

### #5868 feat: send x-opencode-session header for OpenCode Go/Zen providers（OPEN）
- **作者**: huangxianzhan | 创建/更新: 2026-09-04 | 评论: 0
- **链接**: [#5868](https://github.com/Hmbown/Codewhale/pull/5868)
- **功能**: OpenCode Go 要求客户端发送稳定的 `x-opencode-session` header 以优化 prompt caching 和会话流量归属。Codewhale 请求此前缺失该 header，导致 UA（Mozilla/5.0 compatible codewhale/...）被误分类。是对第三方 provider 兼容性的直接改进。

### #5867 feat(config): add [reasoning_only] section for retry count and custom（OPEN）
- **作者**: Gabriel-Degret | 创建: 2026-09-03 | 更新: 2026-09-04 | 评论: 0
- **链接**: [#5867](https://github.com/Hmbown/Codewhale/pull/5867)
- **功能**: 新增 `[reasoning_only]` 配置节，使用户可以配置 reasoning-only 重试次数。此前重试次数（`MAX_REASONING_ONLY_REPROMPTS = 2`）是硬编码的：当 reasoning 模型只返回内部思考而未给出最终答案或工具调用时，引擎静默重试固定两次。该 PR 解决了一个特定的推理模型行为问题，值得关注。

### #5865 refactor(tui): re-land FEAT-020 plugin command shapes on main（OPEN）
- **作者**: aboimpinto | 创建/更新: 2026-09-03 | 评论: 0
- **链接**: [#5865](https://github.com/Hmbown/Codewhale/pull/5865)
- **功能**: 将 FEAT-020（plugin 命令形状改造）重新合入 `main` 分支。原实现 PR #5657 被合并到了 `codex/v0912-integration-20260...` 分支上，需要在 main 上重做。这属于 EPIC-005 crate 分解中的一部分。

### #5833 feat(memory): FEAT-019 memory capability, memory facet, and typed outcomes（CLOSED）
- **作者**: Hmbown | 创建: 2026-09-02 | 更新: 2026-09-03
- **链接**: [#5833](https://github.com/Hmbown/Codewhale/pull/5833)
- **功能**: 关闭 #5609。重新落 FEAT-019 memory 命令部分：
  - 新增 `CommandCapabilities::MEMORY` capability 位和 `CommandMemoryContext` facet
  - TUI memory adapter 支持 typed outcomes（search / remember / get / export / reindex / delete）
  - 将 `/n...` 相关命令转换到新架构
  - 属于 EPIC-005 框架下的分能力构建。

### #5858 tui: collapse ocean_treatment into ThemeId::Underwater（CLOSED）
- **作者**: Hmbown | 创建: 2026-09-02 | 更新: 2026-09-03
- **链接**: [#5858](https://github.com/Hmbown/Codewhale/pull/5858)
- **功能**: 将 `ocean_treatment` 折叠进 `ThemeId::Underwater`，共 11 个 commits，涉及 locale 字符串、mark assets、核心折叠逻辑（deepsea alias、单一 picker 列表、只读配置迁移、OceanRamp keys）、命令/engine 路由、ocean+picker+widget 重绘、`context_percent` 管道等。属于 shell UX 清理工作的一部分。

### #5862 Codewhale 0.9.12: Fleet-only UX（CLOSED）
- **作者**: Hmbown | 创建/更新: 2026-09-03
- **链接**: [#5862](https://github.com/Hmbown/Codewhale/pull/5862)
- **功能**: 集成了 10 个 UX 相关 slice，为 0.9.12 release 准备：
  - hover 契约（统一 hovered_row_style band）
  - workbar 重命名（sidebar/rail → workbar，底部默认，`/workbar` + aliases）
  - settings 分组与重组
  - 以及其他 startup、underwater 默认主题、provider、logo、hover、roles、retro theme 等改动。
  - 这是 0.9.12 的用户可见改动汇总，对社区升级判断很有价值。

### #5843 tui: align typed config and schema with the live value spaces（CLOSED）
- **作者**: Hmbown | 创建: 2026-09-02 | 更新: 2026-09-03
- **链接**: [#5843](https://github.com/Hmbown/Codewhale/pull/5843)
- **功能**: 3 commits，同步 typed config/schema 与实际运行时值空间：typed theme 现在携带 custom themes、删除孤儿 locale keys、对齐 typed config/schema。标注为 Low Risk，但它是配置正确性的基础清理工作。

## 5. 功能需求趋势

综合上述 Issues 与 PR，社区当前关注的功能方向集中在以下几条主线：

1. **ACP 协议服务端能力补全（最突出）**
   - #5863 与 #5864 均指向 `serve --acp` 的会话能力缺失。ACP 客户端（主要是编辑器/IDE 场景）当前既不能读取/切换工作模式与模型配置，也不能列出/加载历史会话。这说明 ACP 集成场景的重要性在上升，预计后续会有更多协议对齐工作。

2. **会话生命周期与可恢复性**
   - 除了 ACP `session/list` / `session/load` 的需求外，#5869 关注的是 shell job 快照的会话内追踪问题，本质上也是"多个任务在同一 session 中共存"时的状态一致性问题。

3. **第三方模型 Provider 兼容性适配**
   - #5868 为 OpenCode Go/Zen 生态加入 `x-opencode-session` header 支持。此类改动表明社区用户正在大量把 Codewhale 接入不同模型网关/代理产品，头部厂商兼容性成为新功能的常态化考量。

4. **可配置化与去硬编码**
   - #5867 将 `MAX_REASONING_ONLY_REPROMPTS` 这类内部常量开放为配置项。反映用户在真实使用中遇到模型返回空思考或循环时，希望用配置而非改代码来改变引擎行为。

5. **TUI 工作台架构重构收尾（EPIC-005）**
   - #5865、#5833、#5316 等显示了 CodeWhale TUI 正在向"capability + facet + typed outcome"范式收敛，命令由完整实现逐步分解为可插拔的命令单元。这是最大尺度的内部重构，但社区对后续插件化的预期也在增强。

6. **主题与 UI 定制持续迭代**
   - #5858 和 #5862 展示了主题合并、workbar 重命名、hover 样式统一等外观优化高频推进；未来在默认主题简化与自定义主题承载能力上还会有进一步动作。

## 6. 开发者关注点

从 Issues 与 PR 的反馈中，可以提炼出以下高频痛点和开发者的关注：

1. **ACP 客户端体验被会话能力卡脖子**
   - 多个 issue 同时指向：`session/config` 查看能力缺失、`session/list` 无法使用。开发者需要在客户端侧做大量 workaround，或被迫为每次工具调用开新会话，浪费上下文的 prompt cache。

2. **跨会话任务归属不清晰、易串扰**
   - #5869 提到此前只能靠 command text 来"猜"后台 job 的更新属于哪个任务，且会产生错误输出串到新 tool card 上的现象。这说明多任务并行在 TUI 上已成为实际工作负载，job 的稳定 ID 是刚需。

3. **UA 识别与 Provider 流量归因问题**
   - Codewhale 自带的 UA 被第三方服务判定为浏览器爬虫（"Mozilla/5.0 compatible codewhale/..."），导致请求分类异常。开发者需要更友好、更易于 provider 识别的请求头。

4. **硬编码的重试策略不可调**
   - reasoning-only 模型静默重试两次可能既浪费时间，也可能在不需要重试时拖慢响应；用户期望该参数可配置。

5. **分支管理与 re-land 的额外开销**
   - FEAT-020 原本应合并到 main，但被错误地合入了特性分支 `codex/v0912-integration-20260823`，导致必须在 main 上重新提交（#5865）。这类问题会增加合入噪音，开发者希望主干分支集成纪律更强。

6. **垃圾 Issue 开始出现**
   - #5866 是典型的非技术类 SEO 推广垃圾信息。对维护者而言，这意味着需要引入更好的 Issue 筛选与模板约束机制，避免社区噪音稀释真实问题反馈。

</details>

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*