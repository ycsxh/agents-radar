# AI CLI 工具社区动态日报 2026-07-10

> 生成时间: 2026-07-10 03:56 UTC | 覆盖工具: 9 个

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

# AI CLI 工具横向对比分析报告
**日期：2026-07-10**

---

## 1. 生态全景
当前 AI CLI 工具生态已进入 **“多智能体协作 + 深度标准化”** 并行发展的阶段。各工具在模型扩展（GPT-5.6、Claude Fable 5、Grok 等）、多账户管理、安全与权限控制上密集迭代，同时 **`AGENTS.md` 统一规范**引发跨社区的高度共鸣，标志着行业从各自为战向互操作性演进。稳定性问题（TUI 假死、子代理误报、认证循环）依然是开发者最大痛点，反映出快速功能扩张与核心体验打磨之间的张力。整体上，**“命令行即平台”** 的理念愈发明确，工具正从单次问答向自定义工作流、子代理编排和持久化会话管理深度进化。

---

## 2. 各工具活跃度对比

| 工具 | 今日 Issues 情况 | 今日 PR 情况 | 新版本发布 |
|------|-------------------|--------------|------------|
| **Claude Code** | Top 10 热点 Issues（含 6235 爆款 335💬） | 3 个社区 PR（文档修正） | v2.1.206（目录建议、`/doctor`、自动 git push） |
| **OpenAI Codex** | 10 个热点 Issues，含紧急故障 #31906 | 10 个重要 PR（安全隔离、认证、分页历史等） | v0.144.1 紧急修复；v0.144.0 新增额度重置等 |
| **Gemini CLI** | 10 个热点 Issues（子代理、认证循环等） | 10 个重要 PR（安全、性能、`AGENTS.md` 支持） | v0.52.0-nightly（修复思考泄漏） |
| **GitHub Copilot CLI** | 10 个 Issues（TUI 假死回归严重） | 0 个 PR | v1.0.70 正式版 + v1.0.70-0 预发布 |
| **Kimi Code CLI** | 仅 2 个活跃 Issues（SSL 证书、速率限制） | 3 个 PR（`CLAUDE.md` 兼容、异常修复） | 无 |
| **OpenCode** | 10 个热点 Issues（V2 架构、桌面交互） | 10 个 PR（工具接纳、shell 协调、V2 规格） | v1.17.16 ~ v1.17.18 三个补丁版 |
| **Pi** | 10 个热点 Issues（严格工具、思考层级、上下文） | 10 个 PR（模型窗口修正、OAuth、缓存追踪） | v0.80.6（`max` 推理层级） |
| **Qwen Code** | 10 个热点 Issues（多工作区、粘贴失效、安全） | 10 个 PR（多工作区守护进程、ACP 传输等） | v0.19.8-nightly（修复代理死循环） |
| **DeepSeek TUI** | 10 个热点 Issues（编排架构、TUI 性能） | 10 个 PR（v0.8.68 收尾、性能、xAI/Termux） | 无（v0.8.68 即将发布） |

*注：多数工具仅列出社区精选的 Top 10 Issues/PR，实际总量可能更高；Copilot CLI 今日无 PR 合并。*

---

## 3. 共同关注的功能方向

### 3.1 统一代理规范文件 `AGENTS.md`
- **Claude Code**：Issue #6235 爆火（335💬，4344👍），社区要求以 `AGENTS.md` 替代 `CLAUDE.md`，实现跨工具代理上下文共享。
- **Gemini CLI**：PR #28240 直接将 `AGENTS.md` 加入默认上下文文件，实现开箱即用。
- **Kimi Code CLI**：PR #2487 支持同时加载 `CLAUDE.md` 与 `AGENTS.md`，降低迁移成本。
- **趋势**：行业正在自发形成事实标准，`AGENTS.md` 有望成为多代理工具链的通用上下文描述文件。

### 3.2 多智能体/子代理的稳定性与透明度
- **Claude Code**：子代理权限继承缺失（#73633）、Agent 名称静默切换 teammate（#71723）。
- **OpenAI Codex**：GPT-5.6 Sol 多智能体 V2 默认隐藏子代理控制（#31814），引发黑盒恐慌。
- **Gemini CLI**：子代理达到 `MAX_TURNS` 后误报成功（#22323）、通用代理频繁卡死（#21409）。
- **OpenCode**：V2 架构大量讨论工具链接纳、会话簿记修复。
- **DeepSeek TUI**：Fleet/Workflow/Lane/Runtime 四层编排架构定义，欲从根本上解决子代理协调问题。
- **共识**：所有支持子代理的工具都面临“失控”与“不透明”问题，社区呼吁可观测性、权限继承和自动恢复机制。

### 3.3 模型灵活配置与成本控制
- **GitHub Copilot CLI**：请求自动切换模型以便规划/执行分离（#2792）、可配置系统提示以节省 ~20K token（#2627）。
- **OpenAI Codex**：额度重置失败浪费付费资源（#31606）。
- **Pi**：支持 `max` 推理层级（v0.80.6）、指数退避无上限引发浪费（#6303）。
- **OpenCode**：希望“纯问答模式”以节省 token（#1573）。
- **趋势**：开发者对 token 消耗和费用透明度的要求日益精细，从模型选择到推理深度均为可控项。

### 3.4 IDE/桌面集成与跨平台体验
- **Claude Code**：VS Code 拖放失效（#25128）、远程控制不支持斜杠命令（#28379）。
- **OpenAI Codex**：Windows 缺失控制其他设备选项卡（#28919）。
- **GitHub Copilot CLI**：TUI 在 WSL2/Windows Terminal 频繁假死（#4069，#4077）。
- **Qwen Code**：macOS 下粘贴图片失效、Windows 控制台乱码。
- **OpenCode**：桌面端快捷键冲突、归档恢复等交互打磨。
- **共识**：终端与 IDE/桌面端的融合体验已成为用户首要诉求，跨平台一致性与稳定性亟待提升。

### 3.5 安全与权限模型强化
- **OpenAI Codex**：PR 阻止工作负载凭据泄露至子进程（#32008），引入令牌交换。
- **Gemini CLI**：A2A-Server 强制工作区信任以阻止 RCE（#28319），修复 hook 对话框泄露（#28346）。
- **Qwen Code**：子进程继承敏感环境变量导致凭证泄露（#6601）。
- **DeepSeek TUI**：环境级工具沙盒、角色门控等安全设计。
- **趋势**：从被动修补漏洞转向主动设计安全边界（凭据擦除、沙箱、工作区信任），安全成为 CLI 工具的基础设施。

---

## 4. 差异化定位分析

| 工具 | 功能侧重 | 目标用户 | 技术路线/独特优势 |
|------|----------|----------|-------------------|
| **Claude Code** | 高级推理与代码生成，v2.1 强化 CLI 交互 | 重度依赖 Anthropic 模型的开发者 | `CLAUDE.md` 深度集成，`/doctor` 等自检指令，向 GUI 渗透 |
| **OpenAI Codex** | 多模型、MCP 集成、安全认证 | 企业用户、CI/自动化场景 | 多智能体 V2 架构，Rust 实现的高性能，工作负载身份认证 |
| **Gemini CLI** | 记忆系统、子代理、安全打磨 | Google 生态用户，注重记忆与自动化 | `AGENTS.md` 原生支持，Auto Memory 与健壮性评估，安全先行 |
| **GitHub Copilot CLI** | TUI 体验、插件生态、企业策略 | GitHub 生态开发者，偏好一体化体验 | 插件作用域（项目级），Gists 管理，GPT-5.6 支持，企业策略集成 |
| **Kimi Code CLI** | 跨工具兼容、企业环境适配 | 国内开发者、企业受控环境 | 兼容 `CLAUDE.md`/`AGENTS.md`，SSL 证书忽略（企业代理），小团队精耕 |
| **OpenCode** | 桌面端与 TUI 双模，V2 架构重构 | 偏好桌面 GUI 的开发者，多模型切换者 | 密集修复周，桌面端交互细腻，会话分叉与分页历史，多提供商 |
| **Pi** | SDK 化、严格工具/结构化输出 | 扩展开发者、多模型整合者 | `max` 推理层级，消息锚定动态工具加载，ADK 提供商整合，xAI OAuth |
| **Qwen Code** | 多工作区守护进程、终端优化 | 通义千问模型用户，多项目管理 | 守护进程多工作区架构，ACP 传输，安全漏洞主动预防 |
| **DeepSeek TUI** | 四层编排架构（Fleet/Workflow/Lane） | 高级自动化用户、偏好 Rust 性能 | 独创工作流引擎，Termux/Android 支持，性能优化（parking_lot），宪法约束 |

**关键差异总结**：
- **编排深度**：DeepSeek TUI > Claude Code / Gemini CLI（前者自建工作流引擎，后者偏重单代理+工具调用）。
- **安全成熟度**：OpenAI Codex / Gemini CLI > 其他（工作负载身份、RCE 修复、沙箱设计）。
- **GUI/桌面化**：OpenCode > GitHub Copilot CLI / Claude Code（桌面端优先，交互打磨最多）。
- **标准化推动**：Gemini CLI 落地 `AGENTS.md` 最积极；Claude Code 社区呼声最高但官方尚未行动。
- **模型多样性**：Pi > OpenAI Codex > OpenCode（Pi 同时接入 GPT-5.6、Claude、xAI、Bedrock 等，切换最灵活）。

---

## 5. 社区热度与成熟度

### 高活跃度、快速迭代阵营
- **OpenAI Codex**：今日 10 个重要 PR + 紧急修复发布，安全认证、分页历史等深层特性持续落地，Rust 重构中的工具性能领先。
- **Pi**：今日 10 PR 合并/进行中，从模型窗口修正到 OAuth 登录，响应神速，SDK 生态扩展性强。
- **Qwen Code**：33 个 Issues 更新，50 个 PR 更新（虽仅列 10），多工作区守护进程架构快速推进，CI 安全守卫新颖。
- **DeepSeek TUI**：虽未发布新版，但 v0.8.68 里程碑收尾汇集大量 PR，编排架构落地带动高频协作。

### 成熟稳定、问题聚焦阵营
- **Claude Code**：Issue #6235 单条超 335 评论，核心诉求强烈，但开发节奏相对克制，PR 较少（仅文档修正 3 个）。
- **GitHub Copilot CLI**：TUI 回归问题严重，但新功能（GPT-5.6、插件 pin 等）正常推进，企业策略类 Issue 长期。
- **Kimi Code CLI**：活跃度较低，仅 2 个 Issue、3 个 PR，处于细水长流的改进期。

### 特色社区驱动型
- **OpenCode**：桌面端体验贴反馈极多，从归档恢复到快捷键冲突，以 UX 细节致胜。
- **Gemini CLI**：安全修复占比高（A2A RCE、hook 泄露），社区对记忆系统和子代理行为要求严苛。

---

## 6. 值得关注的趋势信号

### 6.1 互操作性标准化势不可挡
`AGENTS.md` 正从 Claude Code 社区的“呼声”变为 Gemini CLI 的“默认加载”和 Kimi Code 的“兼容读取”。**未来 AI 工具链的入口不再是某个模型，而是一份跨工具的代理规范文件**。开发者应尽早采用该规范，为多工具协作做好准备。

### 6.2 从“对话”到“编排”的架构升级
DeepSeek TUI 四层编排、OpenAI Codex 多智能体 V2、Pi 动态工具加载均指向：**单次问答已无法满足复杂开发任务，模型需要调度子代理、管理工作流**。这要求工具提供可观测的编排面板、失败重试与权限继承机制——目前普遍缺失，是下一阶段竞争焦点。

### 6.3 安全从“被动修复”转向“主动设计”
凭据擦除、工作负载身份、沙箱、环境变量隔离等安全特性密集出现在 PR 中（Codex、Gemini、Qwen、DeepSeek）。**安全将成为 CLI 工具的企业准入门槛**，没有隐私保护和安全护栏的工具将难以进入生产环境。

### 6.4 桌面/GUI 化不可逆，但稳定性堪忧
Copilot CLI 的 TUI 假死、OpenCode 的快捷键冲突、Claude Code 的 VS Code 回归，暴露出**将 CLI 功能迁移至 GUI 时的工程复杂度**。趋势明显是“命令行启动，图形界面交互”，但现阶段跨平台稳定性差，开发者应谨慎评估生产环境中 GUI 模式的可靠性。

### 6.5 成本可视化与推理控制成为刚需
额度重置浪费、token 缓存未命中追踪、推理层级选择（`max`/`low`）、模型规划/执行角色分离等需求，说明**开发者希望像管理云计算资源一样管理 AI token 消耗**。未来领先工具将提供多层次成本仪表盘和自动路由策略。

### 6.6 多提供商与模型不可知论
Pi 的一站式多提供商（OpenAI、Anthropic、xAI、Bedrock）、DeepSeek TUI 快速集成 xAI 和 GPT-5.6、Codex 的 Copilot 模型切换，表明**锁定单一模型厂商的风险正在被社区规避**。工具的长期竞争力将取决于对异构模型后端的适配速度和透明配置能力。

---

**结论**：2026 年中，AI CLI 工具已进入深水区。**标准化、编排化、安全化和多模型化**是四大不可逆趋势。对技术决策者而言，选择工具需综合考量社区活跃度（OpenAI Codex、Pi、Qwen 迭代快）、安全成熟度（Gemini CLI、Codex 领先）、编排能力（DeepSeek TUI 探索前沿）以及桌面交互稳定性（OpenCode 用心但不稳）。建议关注 `AGENTS.md` 的落地，并尽早参与社区反馈以影响路线图。

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills 社区热点报告

数据截止 2026‑07‑10，聚焦技术讨论最活跃的 PR 和 Issues。

---

## 1. 热门 Skills 排行（PR 关注度 Top 7）

| # | PR | 功能概述 | 讨论热点 | 状态 |
|---|-----|---------|----------|------|
| 1 | [#1298](https://github.com/anthropics/skills/pull/1298) | 修复 **skill‑creator 评估器** `run_eval.py` 始终报告 0% 召回率的问题；安装评估产物为真实 Skill；修复 Windows 流读取、触发检测、并行 worker | 直接解决[#556](https://github.com/anthropics/skills/issues/556)（12 评论），该 bug 导致描述优化回路完全失效，社区多人复现 | OPEN |
| 2 | [#1323](https://github.com/anthropics/skills/pull/1323) | 修复 `run_eval.py` 无法检测真实 Skill 名、遇到首个非 Skill 工具即退出的问题，同样导致 **recall 永远为 0%** | 与 #1298 同类，从另一角度补齐触发检测逻辑，讨论集中在彻底修复评估管道 | OPEN |
| 3 | [#1099](https://github.com/anthropics/skills/pull/1099) | 修复 `run_eval.py` 在 **Windows** 上因管道读取异常导致所有查询被标记为“未触发” | 评论区与 [#1061](https://github.com/anthropics/skills/issues/1061)（Windows 兼容性）联动，确认多编码/子进程问题 | OPEN |
| 4 | [#1050](https://github.com/anthropics/skills/pull/1050) | 修复 **Windows 子进程 `PATHEXT` 与编码**问题（`claude.cmd` 无法找到、`cp1252` 错误） | 单行改动但覆盖多个平台障碍，与 #1099 共同构成 Windows 可用性基线 | OPEN |
| 5 | [#514](https://github.com/anthropics/skills/pull/514) | 新增 **文档排版 Skill**，预防 AI 生成文档的孤行、寡段、编号错位等问题 | 讨论焦点：排版规则应自动注入所有生成文档，触发条件需足够广泛；非功能需求 Skill 的价值得到认可 | OPEN |
| 6 | [#1367](https://github.com/anthropics/skills/pull/1367) | 新增 **self‑audit Skill**：交付前的机械验证 + 四维推理质量门，通用型检查 | 评论区关注“审计顺序是否合理”、“是否可跨模型复用”，体现了对输出质量自动化把关的强烈需求 | OPEN |
| 7 | [#486](https://github.com/anthropics/skills/pull/486) | 新增 **ODT（OpenDocument）Skill**，支持模板填充、格式转换、生成 | 争论点：ODT 与现有 DOCX Skill 的重叠度、是否应统一文档 Skill；开放文档格式支持呼声高 | OPEN |

> 其他高价值 PR：`skill‑quality‑analyzer / skill‑security‑analyzer` ([#83](https://github.com/anthropics/skills/pull/83))、`testing‑patterns` ([#723](https://github.com/anthropics/skills/pull/723))、`color‑expert` ([#1302](https://github.com/anthropics/skills/pull/1302)) 等也获得关注，但尚未形成集中讨论。

---

## 2. 社区需求趋势（来自 Issues 的高热话题）

按评论数与点赞数梳理，最迫切的方向为：

*   **安全与信任边界** – Issue [#492](https://github.com/anthropics/skills/issues/492)（34 💬）直指社区 Skill 套用 `anthropic/` 命名空间带来的信任滥用风险，要求建立官方标识与验证机制。
*   **组织级协作** – Issue [#228](https://github.com/anthropics/skills/issues/228)（14 💬，7 👍）强烈呼吁 **组织内 Skill 共享**，避免手动收发 `.skill` 文件。
*   **开发工具体验（skill‑creator 修复）** – Issue [#556](https://github.com/anthropics/skills/issues/556)（12 💬，7 👍）等揭示描述评估/优化回路完全中断，Windows 兼容性问题集中爆发，说明社区正深度使用 skill‑creator，亟需稳定。
*   **Agent 长时记忆与状态压缩** – Issue [#1329](https://github.com/anthropics/skills/issues/1329)（9 💬）提出 `compact‑memory` Skill，用符号化记号节省上下文，反映对长时间运行 Agent 的成本优化需求。
*   **Skill 自身质量标准与治理** – 已关闭的 [#202](https://github.com/anthropics/skills/issues/202)（8 💬，1 👍）和 [#412](https://github.com/anthropics/skills/issues/412)（6 💬）建议 Skill 应偏指令性而非教程式，并要求为 Agent 系统添加治理/安全 Skill。
*   **Skill 分发与基础设施** – [#189](https://github.com/anthropics/skills/issues/189)（6 💬，9 👍）指出 `document‑skills` 与 `example‑skills` 重复安装，[#16](https://github.com/anthropics/skills/issues/16)（4 💬）提议 Skill 包装为 MCP 工具。

趋势总结：从 **Skill 能做什么** 转向 **Skill 如何安全、高效地分发与运行**，并特别关注开发闭环（创建→评估→优化→共享）的可用性。

---

## 3. 高潜力待合并 Skills

以下 PR 讨论活跃且有明确需求驱动，预计近期可能被合并或推动：

1.  **skill‑creator 评估修复三件套** – [#1298](https://github.com/anthropics/skills/pull/1298)、[#1323](https://github.com/anthropics/skills/pull/1323)、[#1099](https://github.com/anthropics/skills/pull/1099)（解决 0% 召回率和 Windows 问题），是恢复 skill‑creator 核心功能的关键修复。
2.  **Windows 兼容性补丁** – [#1050](https://github.com/anthropics/skills/pull/1050) 与 [#362](https://github.com/anthropics/skills/pull/362)（UTF‑8 字节级校验），降低 Windows 用户参与门槛。
3.  **skill‑creator 增强** – [#1261](https://github.com/anthropics/skills/pull/1261)（隔离评估命令避免污染项目）、[#539](https://github.com/anthropics/skills/pull/539)（YAML 特殊字符警告）、[#361](https://github.com/anthropics/skills/pull/361)（未引号 YAML 检测），提升开发者体验。
4.  **文档排版 Skill** – [#514](https://github.com/anthropics/skills/pull/514)，填补 AI 生成文档的“最后 1%”质量空白。
5.  **self‑audit Skill** – [#1367](https://github.com/anthropics/skills/pull/1367)，将交付前审计内化为 Skill，符合质量自动化趋势。
6.  **ODT Skill** – [#486](https://github.com/anthropics/skills/pull/486)，开放文档用户呼声高，可能通过模板引擎方式与现有 DOCX 技能共存。

---

## 4. Skills 生态洞察

**社区最集中的诉求是：将 Skills 从“能用”推进到“可靠、安全、可度量”的开发基础设施阶段 —— 确保评估回路完好、跨平台健壮、分发可信，并让 AI 输出自带排版与审计等质量闸门。**

---

# Claude Code 社区动态日报 | 2026-07-10

## 今日速览
今日社区焦点无疑是 **v2.1.206 版本发布**，带来了目录路径建议、`/doctor` 检查新指令以及自动允许 `git push` 等实用改进。与此同时，关于统一代理规范文件 `AGENTS.md` 的讨论持续升温，该特性请求已累积 **335 条评论和 4344 个点赞**，反映社区对跨工具互操作性的强烈渴望。多账户管理、模型可用性及 IDE 集成问题也占据大量讨论，表现出开发者对工作流一体化的迫切需求。

## 版本发布
**v2.1.206** 已于今日发布，主要更新如下：
- `/cd` 指令现在支持目录路径建议，与 `/add-dir` 行为保持一致。
- 新增 `/doctor` 检查，可建议对已检入的 `CLAUDE.md` 进行裁剪，移除 Claude 可从代码库中自行推导的内容。
- `/commit-push-pr` 现在自动允许 `git push` 到仓库默认的远程配置。

## 社区热点 Issues (Top 10)
1. **#6235 支持 AGENTS.md 统一规范**  
   `[enhancement, area:core]` | 🔥 335 评论 · 4344 👍  
   社区强烈要求支持行业正逐步标准化的 `AGENTS.md`，以替代仅限 Claude Code 的 `CLAUDE.md`，方便多人协作中不同编码代理共享上下文。  
   [查看详情](https://github.com/anthropics/claude-code/issues/6235)

2. **#5088 支付后账户被禁用**  
   `[bug, area:cost]` | 180 评论 · 59 👍  
   用户反映购买 Max 5x 计划后账户立刻不可用，涉及计费与认证系统稳定性问题。  
   [查看详情](https://github.com/anthropics/claude-code/issues/5088)

3. **#18435 多账户管理功能**  
   `[enhancement, area:auth]` | 126 评论 · 643 👍  
   桌面应用缺少多 Claude 账户切换功能，用户需频繁登出重登，希望引入轻松切换的账户档案。  
   [查看详情](https://github.com/anthropics/claude-code/issues/18435)

4. **#73365 Fable 5 advisor 始终 “unavailable”**  
   `[bug, platform:windows]` | 48 评论 · 92 👍  
   使用 Fable 5 模型时 Advisor 工具不可用，影响 Windows 用户咨询体验，与模型选择逻辑相关。  
   [查看详情](https://github.com/anthropics/claude-code/issues/73365)

5. **#20131 多账户档案支持**  
   `[enhancement, area:auth]` | 37 评论 · 96 👍  
   类似 #18435，强调 API 计费账户与订阅账户的快速切换，避免重新认证。  
   [查看详情](https://github.com/anthropics/claude-code/issues/20131)

6. **#25128 VS Code 扩展拖放失效**  
   `[bug, platform:macos]` | 23 评论 · 43 👍  
   VS Code 面板中拖放功能失效，而 CLI 端正常，系 v2.1.6 引入的回归问题。  
   [查看详情](https://github.com/anthropics/claude-code/issues/25128)

7. **#67609 Advisor 超 100K tokens 返回 “unavailable”**  
   `[bug, platform:macos]` | 16 评论 · 34 👍  
   Fable 5 下会话超过约 10 万 token 时 Advisor 服务中断，影响长时间任务。  
   [查看详情](https://github.com/anthropics/claude-code/issues/67609)

8. **#28379 远程控制 UI 不支持斜杠命令**  
   `[enhancement, area:claude-code-web]` | 11 评论 · 51 👍  
   通过 `/remote-control` 继续会话时，斜杠命令被当作普通文本发送，移动端体验打折。  
   [查看详情](https://github.com/anthropics/claude-code/issues/28379)

9. **#71723 Agent name 参数静默切换为 teammate 协议**  
   `[bug, area:agents]` | 6 评论 · 1 👍  
   当会话存在团队配置时，带 `name` 的 Agent 调用会误入 teammate 路径，导致后台代理结果丢失。  
   [查看详情](https://github.com/anthropics/claude-code/issues/71723)

10. **#73633 工作流子代理未继承权限允许规则**  
    `[bug, area:agents]` | 5 评论 · 5 👍  
    多代理工作流中子代理不继承项目 `settings.local.json` 的允许权限，导致每次工具调用都弹出权限询问。  
    [查看详情](https://github.com/anthropics/claude-code/issues/73633)

## 重要 PR 进展
今日仅有 3 个社区 PR 更新，均来自同一作者，专注于文档和示例修正：
- **#76029** 修正插件开发文档中 `.mcp.json` 示例格式，从嵌套 `mcpServers` 改为平铺结构，以符合插件实际部署方式。  
  [链接](https://github.com/anthropics/claude-code/pull/76029)
- **#76028** 修复插件开发 README 中过时的市场名称，统一为正确的安装指令格式。  
  [链接](https://github.com/anthropics/claude-code/pull/76028)
- **#76023** 修复加载上下文示例中 Github Actions 目录检测错误，将文件测试 `-f` 改为目录测试 `-d`，确保 CI 环境变量正确设置。  
  [链接](https://github.com/anthropics/claude-code/pull/76023)

## 功能需求趋势
从过去 24 小时更新的 Issues 中可提炼出以下主要趋势：
1. **互操作性与标准化**：大量支持要求兼容 `AGENTS.md`，减少厂商锁定，促进工具链协作。
2. **多账户/多档案管理**：多个高票特性请求集中在桌面应用内便捷切换 Claude 账户（个人/工作/API 账单等）。
3. **IDE 与桌面集成**：VS Code 扩展的功能完善（拖放修复）、桌面应用 UI 体验优化（近期文件夹管理、会话组拖拽排序等）。
4. **远程与移动体验**：远程控制界面支持斜杠命令，使手机端也能像 CLI 一样掌控会话。
5. **工作流与代理稳定性**：多代理工作流中权限继承、错误重试机制、子代理结果传递等问题频繁出现，提示自动化工作流需要更健壮的运行环境。

## 开发者关注点
- **模型可用性**：Fable 5、Opus 4.8 等新模型在 CLI、IDE 扩展中的调用成功率与错误处理仍需改进，尤其 Advisor 工具对会话长度敏感。
- **成本与控制**：计费异常、技能运行消耗监控（如 Ultrareview 崩溃仍消耗配额）直接关系开发者信任。
- **权限与安全透明**：App 请求 macOS TCC 敏感目录权限的触发逻辑模糊，引发隐私担忧。
- **跨平台一致性**：Windows 拖放、Linux 崩溃（如 v2.1.206 Bun 基线安全）、Cowork 功能在 Linux 上的 KVM 探测限制等，暴露了多平台体验差距。
- **CLI 到 GUI 的过渡落差**：报告结果在桌面应用中以原始 JSON 呈现，而非格式化面板，表明交互体验需统一。

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

## OpenAI Codex 社区动态日报  
**日期：2026-07-10**

---

### 1. 今日速览
今日 Codex CLI 紧急修复版 `v0.144.1` 上线，解决了 `v0.144.0` 中 Homebrew 安装缺失关键主机二进制导致的全面命令失败问题。同时，GPT-5.6 Sol 模型多智能体 V2 架构引发大量争议——默认隐藏子代理控制并频繁出现 `collaboration.spawn_agent` 保留字报错，暴露出模型路线图的透明度与可控性风险。社区还高度关注额度重置浪费、会话历史不全等稳定性痛点。

---

### 2. 版本发布
- **rust-v0.144.1**：紧急修复独立安装因 GitHub 压缩/重排序元数据失败的问题，确保 macOS 包安装后正确暴露 `codex-code-mode-host`，并优化了伴侣主机不可用时的降级行为。
- **rust-v0.144.0**：新增额度重置明细（显示类型/有效期，允许选择兑换）、添加 `writes` 应用许可模式（允许声明只读操作，写入时提示），并支持 MCP 工具申请交互式认证。

*近期还有数个 alpha 预发布版（0.145.0-alpha.1/2 等），暂不推荐生产使用。*

---

### 3. 社区热点 Issues（10 条）
1. **#31906 CLI 0.144.0 缺少 `codex-code-mode-host`**  
   [openai/codex#31906](https://github.com/openai/codex/issues/31906)  
   Homebrew cask 安装后每一条命令都报“failed to spawn code-mode host”，影响所有用户。👍36 💬9，已成今日最高关注故障，修复版已发布。

2. **#29532 macOS 日志仍刷屏 SQLite TRACE 消息**  
   [openai/codex#29532](https://github.com/openai/codex/issues/29532)  
   升级至 0.142.0 后 `logs_2.sqlite` 仍在持续高频写入，社区认为只是部分修复，性能影响明显。👍8 💬35，该问题已纠缠近三周。

3. **#31814 GPT-5.6 Sol 多智能体 V2 默认隐藏子代理控制**  
   [openai/codex#31814](https://github.com/openai/codex/issues/31814)  
   模型元数据自动选择 MultiAgent V2 并强制隐藏派生子代理的元数据，用户无法掌控路由策略，引发对模型透明度的担忧。👍11 💬11。

4. **#31606 额度重置失败且浪费一次重置机会**  
   [openai/codex#31606](https://github.com/openai/codex/issues/31606)  
   用户重置用量后额度未恢复，计数器仍下降，Pro 用户付费资源直接损失。👍13 💬13，高赞高评论，信任危机。

5. **#11747 请求增加 `/add-dir` 会话中动态添加目录**  
   [openai/codex#11747](https://github.com/openai/codex/issues/11747)  
   CLI 仅支持启动参数 `--add-dir`，无法中途扩展工作目录，强迫中断会话。👍35 💬11，长期功能请求，呼声极高。

6. **#31664 推理摘要渲染出 `<!-- -->` 占位符**  
   [openai/codex#31664](https://github.com/openai/codex/issues/31664)  
   在 TUI 和 JSON 输出中直接显示 HTML 注释，破坏可读性。👍15 💬4，虽评论少但赞同集中，表明广泛影响。

7. **#28919 Windows 端缺失“控制其他设备”选项卡**  
   [openai/codex#28919](https://github.com/openai/codex/issues/28919)  
   Windows 桌面应用无法作为远程控制中枢，与 macOS 体验不对等。👍16 💬8，跨平台一致性议题。

8. **#13009 Spark 模型拒绝 `reasoning.summary` 参数**  
   [openai/codex#13009](https://github.com/openai/codex/issues/13009)  
   使用 `gpt-5.3-codex-spark` 时提示 `unsupported_parameter`，模型能力与文档脱节。👍4 💬15，历史遗留但至今未解决。

9. **#8342 暴露 MCP Server Prompts 为斜杠命令**  
   [openai/codex#8342](https://github.com/openai/codex/issues/8342)  
   对标 Claude Code，希望将 MCP 服务器提示模板转化为 `/` 命令，方便调用。👍22 💬6，开发者体验重要方向。

10. **#19425 自定义 stdio MCP 工具未暴露给桌面线程**  
    [openai/codex#19425](https://github.com/openai/codex/issues/19425)  
    桌面端可发现 MCP 工具列表，但实际对话中不可用，疑为回归或暴露层 bug。👍2 💬13，严重影响自定义集成可用性。

---

### 4. 重要 PR 进展（10 条）
1. **#32008 阻止工作负载身份凭据泄露至子进程**  
   [openai/codex#32008](https://github.com/openai/codex/pull/32008)  
   将工作负载身份选择器/断言视为进程私有凭据，从 shell、钩子、MCP 子环境中擦除，增强安全隔离。

2. **#32009 添加工作负载身份令牌交换客户端**  
   [openai/codex#32009](https://github.com/openai/codex/pull/32009)  
   实现 RFC 7523 令牌交换，支持环境变量或轮转文件注入断言，包含重试与缓存，为无密钥认证铺路。

3. **#32010 将工作负载身份集成至 Codex 认证生命周期**  
   [openai/codex#32010](https://github.com/openai/codex/pull/32010)  
   新增分层配置与环境变量，整合进外部认证刷新流程，赋予显式凭据优先权，为 CI/自动化场景打基础。

4. **#31688 保留 WebSocket TBT 指标分数精度**  
   [openai/codex#31688](https://github.com/openai/codex/pull/31688)  
   将 Responses WebSocket 的等待时间 (TBT) 从毫秒取整改为 `f64` 记录，并保持 TUI 显示兼容，提升性能观测精度。

5. **#31891 提取可复用的反向 JSONL 扫描器**  
   [openai/codex#31891](https://github.com/openai/codex/pull/31891)  
   从会话索引中抽取出 `ReverseJsonlScanner`，支持块式反向读取、容错尾部无换行 JSON，为分页历史等场景提供基础组件。

6. **#30131 添加分页线程历史数据库**  
   [openai/codex#30131](https://github.com/openai/codex/pull/30131)  
   创建 `thread_history` SQLite 库及 `thread_turns`/`thread_items` 表，为未来分页加载长对话历史预备存储层。

7. **#31731 增加线程历史分叉基点支持**  
   [openai/codex#31731](https://github.com/openai/codex/pull/31731)  
   在分页线程历史上构建引用冻结范围的 fork 关系，仅 schema 变动，为对话分支管理做准备。

8. **#31655 将工作区根目录移至执行环境概念中**  
   [openai/codex#31655](https://github.com/openai/codex/pull/31655)  
   把 `workspace roots` 从全局会话状态迁移到 `Environment`，避免当前工作目录与沙箱根路径不一致，强化远程执行语义。

9. **#31482 将插件命令迁移至技能系统**  
   [openai/codex#31482](https://github.com/openai/codex/pull/31482)  
   把 `plugins/commands` 转换为 `skills`，解决依赖循环，并在安装原子阶段生成技能文件，使插件架构更统一。

10. **#32000 权限请求路径以 URI 形式传递**  
    [openai/codex#32000](https://github.com/openai/codex/pull/32000)  
    将请求权限的文件路径标准化为 `PathUri`，保留执行器端原生路径标识，避免主机本地路径转换错误。

---

### 5. 功能需求趋势
从 Issues 中提炼出五大社区核心诉求方向：

- **会话与工作流增强**：动态添加目录 (`#11747`)、全窗口聊天模式 (`#31956`)、更灵活的分支/分页历史控制，用户希望减少中断、提高长对话管理能力。
- **MCP 集成深化**：支持自动将 MCP Prompts 转化为斜杠命令 (`#8342`)、修复桌面端工具暴露缺陷 (`#19425`)，社区期待像 Claude Code 一样无缝调用外部工具。
- **多智能体透明化**：`#31814` 和 `#31864` 暴露出新 Sol 模型强制隐藏子代理行为，开发者要求提供开关、公开路由逻辑，并对模型选择可控。
- **额度与计费控制**：重置失败浪费 (`#31606`)、防止自动使用购买积分 (`#28382`)，用户呼吁更透明的额度管理和手动审批机制。
- **跨平台体验对齐**：Windows 与控制其他设备 (`#28919`)、macOS 日志性能 (`#29532`) 及安装缺失工具 (`#31906`) 显示桌面端与 CLI 在稳定性、功能一致性上仍存在差距。

---

### 6. 开发者关注点
- **稳定性滑坡**：`v0.144.0` 缺失 `codex-code-mode-host` 导致 CLI 整体瘫痪，是近期最严重的发布事故；多个问题（日志刷屏、应用闪退、计划模式回归）指向 QA 与回滚机制需加强。
- **模型行为不透明**：GPT‑5.6 Sol 的多智能体 V2 默认隐藏元数据、使用保留 `spawn_agent` 函数报错，开发者感觉被“黑盒”，影响调试与信任。
- **额度管理信任危机**：重置失败直接损失付费额度，社区要求更可靠的重置机制与更清晰的用量展示。
- **长会话处理缺陷**：分页历史仅显示近期轮次 (`#31995`)、推理占位符污染输出 (`#31664`)，以及工具调用中丢失连接 (`#14824`)，严重损害生产效率。
- **跨设备/跨端一致性**：远程控制、MCP 工具暴露、Windows 功能落后等持续存在，企业多设备用户尤其不满。
- **开发者呼吁“可逆性”**：强调配置开关（如 MultiAgent 行为、自动使用积分）、以及更完善的升级前检查，以减少意外变更带来的影响。

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI 社区动态日报 （2026-07-10）

## 1. 今日速览
今日 Gemini CLI 发布了 `v0.52.0-nightly` 版本，重点修复了历史对话中的“思考内容泄漏”问题，并移除了临时 CI 配置文件对工作区上下文的干扰。社区方面，子代理误报成功、Windows 认证死循环等高优先级缺陷持续发酵，同时多项安全与稳定性的重要修复已合并。

## 2. 版本发布
**v0.52.0-nightly.20260710.ga4c91ce19**
- **修复思考内容泄漏**：擦除历史轮次中的模型思维链，避免敏感中间过程暴露给后续上下文。  
- **重构工作区上下文**：自动排除临时的 CI 配置文件（如 `.github/workflows` 等），减少噪音 Token 消耗。  
详见：[Release v0.52.0-nightly](https://github.com/google-gemini/gemini-cli/releases/tag/v0.52.0-nightly.20260710.ga4c91ce19)

## 3. 社区热点 Issues （Top 10）
1. **子代理达到 `MAX_TURNS` 后误报成功**  
   [#22323](https://github.com/google-gemini/gemini-cli/issues/22323) | 🔥10 评论 · 👍2  
   `codebase_investigator` 在未完成分析时因达到轮次上限而中断，却返回 `status: "success"` 和 `GOAL` 终止原因，严重干扰自动化流程信任。

2. **通用代理（Generalist Agent）频繁卡死**  
   [#21409](https://github.com/google-gemini/gemini-cli/issues/21409) | 🔥7 评论 · 👍8  
   一旦模型将任务委托给通用子代理，进程便无限挂起，即便简单到创建文件夹也会卡住，用户被迫指导模型不要使用子代理。

3. **Windows 下 OAuth 无限认证循环**  
   [#28341](https://github.com/google-gemini/gemini-cli/issues/28341) | 🆕4 评论 · 👍1  
   昨日新增，多个 Windows 用户反馈 CLI 反复进入 OAuth 流程，降级到更早版本也无法解决，完全无法使用。

4. **Shell 命令执行完成后仍卡在“等待输入”**  
   [#25166](https://github.com/google-gemini/gemini-cli/issues/25166) | 4 评论 · 👍3  
   简单 CLI 命令已正常退出，但 Gemini CLI 界面仍显示“Awaiting user input”并挂起，严重影响交互流畅度。

5. **模型不使用自定义技能和子代理**  
   [#21968](https://github.com/google-gemini/gemini-cli/issues/21968) | 6 评论  
   即使技能描述高度匹配当前任务，模型也几乎不会自动触发，必须显式指令才能利用已有能力，降低了自动化程度。

6. **Auto Memory 持续重试低信号会话**  
   [#26522](https://github.com/google-gemini/gemini-cli/issues/26522) | 5 评论  
   记忆提取后台无休止地重试那些被判定为“低信息量”的会话，既浪费资源又让未处理标记累积，影响记忆系统整体有效性。

7. **浏览器子代理在 Wayland 环境下失败**  
   [#21983](https://github.com/google-gemini/gemini-cli/issues/21983) | 4 评论 · 👍1  
   Linux 下 Wayland 会话中，浏览器代理不可用，报错并立即终止，限制了该功能在主流桌面环境的覆盖。

8. **浏览器代理忽略 `settings.json` 配置覆盖**  
   [#22267](https://github.com/google-gemini/gemini-cli/issues/22267) | 3 评论  
   用户在全局或项目级配置中针对浏览器代理设置的 `maxTurns` 等参数被完全忽略，AgentRegistry 读取后并未实际应用。

9. **代理应主动阻止破坏性操作**  
   [#22672](https://github.com/google-gemini/gemini-cli/issues/22672) | 3 评论 · 👍1  
   在执行复杂 Git 操作或数据库维护时，模型偶尔会使用 `git reset --force` 等危险命令，缺乏安全护栏和劝阻机制。

10. **组件级健壮性评估体系 EPIC**  
    [#24353](https://github.com/google-gemini/gemini-cli/issues/24353) | 7 评论  
    追踪 76+ 行为评估用例的扩展，目标是建立覆盖更多模型和场景的组件级 CI 评测，对保障迭代质量至关重要。

## 4. 重要 PR 进展
- **修复启动时同步探测编辑器导致缓慢**  
  [#28144](https://github.com/google-gemini/gemini-cli/pull/28144) (已合并)  
  将编辑器可用性检查改为延迟加载，避免 `execSync` 阻塞启动，尤其在 Windows 下提升明显。

- **会话重置后忽略过时的 `update_topic` 调用**  
  [#28153](https://github.com/google-gemini/gemini-cli/pull/28153) (已合并)  
  防止 `/clear` 重置会话后，残留的主题更新工具调用覆盖到新会话，造成标题错乱。

- **技能资源列表遵守 `.gitignore` 与 `.geminiignore`**  
  [#28149](https://github.com/google-gemini/gemini-cli/pull/28149) (已合并)  
  激活技能时显示的文件夹结构现在会过滤忽略文件，避免暴露不应提交的资源。

- **MCP 资源按服务器隔离，消除跨服务器混淆**  
  [#28143](https://github.com/google-gemini/gemini-cli/pull/28143) (已合并)  
  修复了两个 MCP 服务器暴露相同 URI 时，`read_mcp_resource` 可能返回错误服务器内容的严重缺陷。

- **修补 shell 命令依赖的安全公告**  
  [#28140](https://github.com/google-gemini/gemini-cli/pull/28140) (已合并)  
  升级 `shell-quote` 和 `simple-git` 以覆盖公开安全漏洞，预防潜在风险。

- **绕过 LLM 修正，修复 JSON/IPYNB 文件损坏**  
  [#28223](https://github.com/google-gemini/gemini-cli/pull/28223) (已合并)  
  对 `.json` 和 `.ipynb` 文件，`write_file` 和 `replace` 不再交由模型二次修正，直接写入，避免结构破坏。

- **支持 `AGENTS.md` 开箱即用**  
  [#28240](https://github.com/google-gemini/gemini-cli/pull/28240) (开放中)  
  将 `AGENTS.md` 加入默认上下文文件列表，无需手动配置即可被自动加载，对齐社区标准。

- **修复可运行 Hook 的信任对话框泄露**  
  [#28346](https://github.com/google-gemini/gemini-cli/pull/28346) (开放中)  
  安全修复：停止将扁平且无效的钩子条目当作可执行命令报告，并添加项目级命令钩子警告，防止权限误导。

- **A2A-Server 强制工作区信任以阻止 RCE**  
  [#28319](https://github.com/google-gemini/gemini-cli/pull/28319) (开放中)  
  重构启动流程与环境加载，修复零点击远程代码执行漏洞和环境污染风险，安全等级极高。

- **修复因模糊“上一次用户意图”标签导致的回答偏离**  
  [#28343](https://github.com/google-gemini/gemini-cli/pull/28343) (已合并)  
  在截断历史时使用更明确的意图标签，解决了模型继续回答旧问题而忽略最新提示的严重偏差。

## 5. 功能需求趋势
- **代理可靠性与可观测性**：社区强烈要求解决子代理误报成功、agent 卡死、shell 执行停滞等问题，并期待子代理轨迹可查看、共享（#22598）。  
- **记忆系统健壮化**：Auto Memory 被视为半成品，需要停止无效重试、隔离损坏补丁、增强自动脱敏和日志管控（#26522, #26523, #26525）。  
- **安全与破坏性行为防护**：希望内置破坏性命令预警、强化权限对话框、修复远程执行漏洞（#22672, #28346, #28319）。  
- **代码理解与工具质量**：探索 AST 感知文件读取（#22745），增强 Jupyter/JSON 文件处理，使用原生文件工具维护任务列表（#21000）。  
- **技能与子代理激活率**：要求模型更主动地发现并使用自定义技能和子代理，减少必须显式指令的情况（#21968, #28240 的 `AGENTS.md` 支持）。  
- **跨平台体验一致性**：Wayland 浏览器代理支持、终端大小调整性能（#21924）、认证流程在 Windows 下的稳定性（#28341）。

## 6. 开发者关注点
- **认证流程脆弱**：Windows 上反复进入 OAuth 循环，阻碍日常使用，需根治。  
- **执行过程不透明且易卡死**：从 shell 命令到子代理，大量“挂起-等待”现象消耗开发者耐心，“看起来完成了，但就是卡住”。  
- **配置与权限控制意外失效**：`settings.json` 被忽略、子代理在不期望时启动，开发者感到对行为失去控制。  
- **记忆系统干扰大于帮助**：后台无休止重试、日志泄露风险、无效补丁静默丢弃，让不少用户对该功能产生质疑。  
- **安全改进呼声强烈**：从 hook 泄露到 A2A RCE，社区期望更严格的上游安全审查与默认安全策略。

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI 社区动态日报

**日期**：2026-07-10  
**数据范围**：过去 24 小时

---

## 1. 今日速览
今日社区主要聚焦 v1.0.70 正式版与 v1.0.70-0 预发布版的新功能，但**多个 TUI 假死/黑屏回归问题**引发强烈关注，尤其集中在 WSL2 及 Windows Terminal 环境。同时，社区对模型配置灵活性、企业策略等问题持续讨论，插件项目级作用域需求已获官方关闭，表明功能可能已落地或有单独计划。

---

## 2. 版本发布
**v1.0.70**（稳定版）于 7 月 9 日发布：
- 新增 **GPT-5.6 模型**支持
- `web_fetch` 工具现已支持强制 HTTPS 代理环境
- 改进错误提示：MCP / 技能命令失败时显示统一 `Error` 前缀；`--agent` 加载畸形自定义代理时展示真实解析错误
- Gists 标签页增加隐藏/搜索功能
- 被取代的子代理运行视为已取消

**v1.0.70-0**（预发布版）额外包含：
- 插件支持通过 `sha` 字段固定到精确 commit
- 新增 `--sandbox` / `--no-sandbox` 标志，可临时覆盖沙箱设置
- 新增 `/refine` 命令，用于重写/优化内容

---

## 3. 社区热点 Issues（Top 10）

| # | 议题 | 热度 | 核心关注点 |
|---|------|------|------------|
| 1 | [#4069](https://github.com/github/copilot-cli/issues/4069) TUI 假死：屏幕清空，输入无响应，Ctrl+C 无效（WSL2） | 👍7 💬7 | **v1.0.70-0 严重回归**，终端直接僵死，影响基本使用 |
| 2 | [#4077](https://github.com/github/copilot-cli/issues/4077) TUI 黑屏挂起（Windows Terminal），可 --resume 恢复 | 👍2 💬2 | 与 #4069 同为稳定性危机，内容未丢失但交互瘫痪 |
| 3 | [#970](https://github.com/github/copilot-cli/issues/970) macOS Gatekeeper 每次升级后均阻止 Copilot 运行 | 👍21 💬7 | 企业安全策略下的长期安装痛点 |
| 4 | [#1595](https://github.com/github/copilot-cli/issues/1595) 企业策略偶发阻止模型拉取，`/models` 返回“access denied” | 👍10 💬28 | 企业用模型管理混乱，影响面广 |
| 5 | [#2792](https://github.com/github/copilot-cli/issues/2792) 请求自动切换模型：一个用于规划，另一个用于执行 | 👍14 💬4 | 社区对成本与性能平衡的强烈需求 |
| 6 | [#2627](https://github.com/github/copilot-cli/issues/2627) 希望可配置系统提示，减少固定 token 开销（~20K tokens） | 👍18 💬3 | 大上下文模型下的成本控制需求突出 |
| 7 | [#1665](https://github.com/github/copilot-cli/issues/1665) 插件支持项目/仓库级作用域（已关闭） | 👍18 💬13 | 虽已关闭，但说明该特性可能已进入开发或拆分，社区关注度高 |
| 8 | [#4071](https://github.com/github/copilot-cli/issues/4071) 会话选择器仅显示当前会话（回归） | 👍0 💬0 | 新版本试验特性导致 `/resume` 列表失效，影响工作流连贯性 |
| 9 | [#4067](https://github.com/github/copilot-cli/issues/4067) `settings.json` 中设置的模型在启动时不生效 | 👍0 💬0 | 配置可靠性问题，直接影响用户自定义模型 |
| 10 | [#107](https://github.com/github/copilot-cli/issues/107) Alpine Linux 下工具调用导致段错误 | 👍4 💬15 | 长期未解决，容器/CI 环境中使用受阻 |

---

## 4. 重要 PR 进展
**过去 24 小时内无合并或新提交的 Pull Request。**

---

## 5. 功能需求趋势
- **模型配置灵活化**：自动切换模型（规划 vs 执行）、指定模型系列（如 `opus`）、修复模型设置在启动时不生效、为 `/fleet` 子代理设置默认模型。
- **插件与扩展生态**：插件作用域（项目/仓库级，虽关闭但热度高）、研究代理可配置 MCP 工具、支持 BYOK 自定义 HTTP 头。
- **上下文与成本控制**：可配置系统提示减少 token 浪费、MCP 服务器过多时自动压缩检测与告警。
- **企业适配**：代理支持（`web_fetch` 已修）、“始终允许”新模型给旧订阅用户、改进企业策略错误反馈。
- **会话与可靠性**：会话恢复可靠性、防止 `git clean -fd` 永久删除未跟踪文件。

---

## 6. 开发者关注点
1. **TUI 稳定性**：`1.0.70-0` 引入的终端假死、黑屏问题成为当前最大痛点，影响 WSL2/Windows Terminal 用户。
2. **配置即生效**：模型设置不生效、实验性特性导致会话列表回归，破坏用户自定义工作流。
3. **安装与平台兼容性**：macOS Gatekeeper 反复拦截、Alpine Linux 段错误，使 CI/企业部署受阻。
4. **模型管理不透明**：企业策略报错不明确、新模型对旧订阅用户不可见、无法查看扩展上下文定价。
5. **会话数据安全**：意外 `git clean` 造成数据丢失、会话查找不一致，开发者对信任感不足。

---

> 数据来源：[github/copilot-cli](https://github.com/github/copilot-cli)  
> 自动生成时间：2026-07-10

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI 社区动态日报

**日期：2026-07-10**

---

## 今日速览

今日无新版本发布，社区焦点集中在两个持续关注的 Issue 上：企业环境中 SSL 证书拦截导致的登录失败（#2458）以及 TPD 速率限制计算偏差（#2318）。同时有三个 PR 获得了更新，其中支持加载 `CLAUDE.md` 配置文件（#2487）有望改善跨工具用户的体验。

---

## 版本发布

过去24小时无新版本发布。

---

## 社区热点 Issues

由于近期无新增 Issue，以下列出最近更新且仍在开放的 Issue：

1. **[enhancement] Add option to ignore ssl certificate**  
   [#2458](https://github.com/MoonshotAI/kimi-cli/issues/2458)  
   由 dmorsin 提出，发布于 2026-06-17，最近更新于 2026-07-09（5 条评论，👍 0）。  
   **要点**：在企业环境中，防病毒软件通过中间人（MITM）方式注入自己的 SSL 证书，导致 Kimi CLI 登录时证书验证失败。用户希望增加忽略 SSL 证书验证的选项（例如 `--insecure` 标志），以兼容受管控的设备。该问题代表了一批企业用户的实际痛点，但需谨慎实现以避免安全风险。

2. **[bug] request reached organization TPD rate limit, current: 1505241**  
   [#2318](https://github.com/MoonshotAI/kimi-cli/issues/2318)  
   由 globalvideos272-lab 提出，发布于 2026-05-18，最近更新于 2026-07-09（1 条评论，👍 1）。  
   **要点**：用户报告使用 kimi 2.6 模型时触发了组织 TPD（Tokens Per Day？）速率限制，错误显示的当前值高达 1,505,241，疑似计算或显示异常。该问题影响高用量用户，可能导致服务中断，需要官方确认限流逻辑是否正确。

---

## 重要 PR 进展

以下为过去24小时内更新且仍开放的 Pull Request：

1. **feat(agent): support loading CLAUDE.md alongside AGENTS.md**  
   [#2487](https://github.com/MoonshotAI/kimi-cli/pull/2487)  
   nankingjing 于 2026-07-09 创建并更新，关联 Issue [#2401](https://github.com/MoonshotAI/kimi-cli/issues/2401)。  
   **内容**：在 `load_agents_md()` 中增加对 `CLAUDE.md` 和 `.claude/CLAUDE.md` 的自动发现，使具有 Claude Code 配置的项目无需额外适配即可被 Kimi CLI 使用。这一改动可大幅降低迁移成本，促进工具互操作性。

2. **fix(web): handle BrokenPipeError in SessionProcess.send_message**  
   [#2324](https://github.com/MoonshotAI/kimi-cli/pull/2324)  
   Ricardo-M-L 于 2026-05-19 创建，2026-07-09 更新。  
   **内容**：修复 `SessionProcess.send_message` 在子进程意外退出时可能触发 `BrokenPipeError` 的隐患，通过添加对管道状态的检查提升稳定性，避免异常崩溃。

3. **fix(string): strip newlines in shorten_middle before the length check**  
   [#2449](https://github.com/MoonshotAI/kimi-cli/pull/2449)  
   Ricardo-M-L 于 2026-06-13 创建，2026-07-09 更新。  
   **内容**：修正 `shorten_middle` 函数在处理带换行符的短文本时，因提前返回而未清理换行符的问题，确保工具调用关键参数能以单行方式显示，改善输出可读性。

---

## 功能需求趋势

从当前开放的 Issue 中提炼的社区需求方向：

- **企业环境兼容性**：支持忽略 SSL 证书或配置自定义 CA 证书，解决企业代理/防病毒软件造成的连接问题。
- **速率限制透明化**：完善 TPD 等用量限制的提示信息，避免误解和异常中断。
- **工具链互操作**：兼容其他 AI 编程工具的配置文件（如 `CLAUDE.md`），降低多工具使用成本。
- **稳定性增强**：改进网络连接与进程通信的异常处理，减少非预期崩溃。

---

## 开发者关注点

- **SSL 验证绕过需求**：企业管理员和受限设备用户强烈需要一个简单开关来跳过证书校验，但社区尚未形成统一的安全方案，平衡易用性与安全性是关键挑战。
- **速率限制异常**：高并发或高用量场景下，错误提示的数值显得极端不合理，开发者需确认是显示 Bug 还是限流策略配置错误，以避免用户误解和投诉。
- **跨工具配置兼容**：随着 Claude Code 等工具用户迁移至 Kimi CLI，自动读取现有配置文件可以成为吸引开发者的低成本改进，PR #2487 正回应了这一趋势。
- **CLI 健壮性**：多个 PR 聚焦于进程通信异常处理和字符串格式化细节，表明开发者正在持续打磨命令行工具的可靠性，这有助于提升专业用户的使用信心。

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode 社区动态日报 — 2026-07-10

## 今日速览
过去 24 小时，OpenCode 密集发布了三个补丁版本，主要解决了模型定价崩溃、Meta 模型推理兼容性及桌面端交互细节。社区围绕 V2 架构展开了大量技术讨论，归档恢复、权限切换与 token 节省等需求持续获得高票关注，同时多个与 V2 工具链、会话分叉相关的技术 issue 正在推进中。核心团队通过多项 PR 对会话簿记、工具接纳流程和 TUI shell 协调进行了重构，稳定性进一步提升。

## 版本发布
今天发布了三个小版本，主要聚焦核心稳定性和桌面体验：
- **v1.17.18**：修复了 GitHub Copilot 返回零计费批次模型时导致的崩溃和错误定价，并为 Meta Muse Spark 模型添加了专属系统提示。
- **v1.17.17**：改进了 Meta 模型的推理变体和提供商请求处理；修复了桌面端模型选择器标签字体被裁剪的问题，并新增了可关闭的选项卡介绍弹窗及帮助入口更新。
- **v1.17.16**：为 Grok 模型开放了推理强度变体，优化了 xAI 的提示缓存路由和 PDF 文件支持；桌面端为项目首页添加了“打开所在文件夹”操作，并在编写器中新增了文件、命令等快捷菜单。

## 社区热点 Issues（10 则）

1. **#12393 如何撤销桌面端会话归档（已关闭，👍27）**  
   [链接](https://github.com/anomalyco/opencode/issues/12393)  
   用户误操作归档后找不到恢复入口，暴露出存档管理 UX 缺口。高赞数反映出大量桌面用户对会话生命周期管理的基本诉求。

2. **#1573 添加“纯问答模式”以节省 token（已关闭，👍2）**  
   [链接](https://github.com/anomalyco/opencode/issues/1573)  
   即使是简单问候也会消耗上万 token，社区呼吁提供一种不激活工具和代理的轻量对话模式，体现对成本控制的强烈需求。

3. **#10815 macOS 快捷键导致无确认会话关闭与数据丢失（已关闭，👍4）**  
   [链接](https://github.com/anomalyco/opencode/issues/10815)  
   `Cmd-Shift-Delete` 被用于强制关闭会话，与系统级文本编辑快捷键冲突，极易造成工作丢失，敏感性高。

4. **#21578 恢复会话级“自动接受权限”开关（已关闭，👍4）**  
   [链接](https://github.com/anomalyco/opencode/issues/21578)  
   权限控制从会话 UI 移至全局设置后，有用户希望回到以前每次会话快速切换的便捷方式，反映操作灵活性的需求。

5. **#34387 V2 支持在提示中使用 @ 标记文件和文件夹（开放中）**  
   [链接](https://github.com/anomalyco/opencode/issues/34387)  
   V2 核心体验补全，允许通过 `@` 引用文件目录，这是原版基本功能向新架构迁移的关键一步。

6. **#35013 Fable/Zen 模型请求过大时绕过自动压缩（开放中）**  
   [链接](https://github.com/anomalyco/opencode/issues/35013)  
   在长会话中，字节数触及限制前不会触发压缩，可能静默失败，影响 Claude Fable 等模型的稳定使用。

7. **#31064 GitHub Copilot gpt-5.5 上下文窗口信息不一致（开放中，👍1）**  
   [链接](https://github.com/anomalyco/opencode/issues/31064)  
   同一模型在 TUI 与本地模型缓存中显示差异巨大的上下文上限，开发者难以准确评估 token 预算。

8. **#36199 上游返回零 token 用量时会话停滞（今日新增，开放中）**  
   [链接](https://github.com/anomalyco/opencode/issues/36199)  
   当提供商返回有效响应但 `usage` 全为零时，OpenCode 会中途卡死，属于边界情况兼容性缺陷。

9. **#23041 希望在单次会话中授权多个目录（开放中，👍2）**  
   [链接](https://github.com/anomalyco/opencode/issues/23041)  
   开发者常需跨项目工作，限制单一工作目录不利于多仓库协作，是提升灵活性的常见需求。

10. **#34430 实现 V2 会话分叉 API（开放中）**  
    [链接](https://github.com/anomalyco/opencode/issues/34430)  
    会话分叉（从特定消息创建分支）是探索性编程和工作流复用的关键，V2 规划中的基础能力。

## 重要 PR 进展（10 则）

1. **#36180 简化核心工具接纳流程**  
   [链接](https://github.com/anomalyco/opencode/pull/36180)  
   将工具引入简化为 `materialize(permissions?)`，清理冗余的模型维度与测试术语，提升核心代码可维护性。

2. **#36200 简化会话运行器状态簿记**  
   [链接](https://github.com/anomalyco/opencode/pull/36200)  
   使用片段成员作为工具输入完成的唯一来源，移除重复的请求和证据跟踪，让会话生命周期更清晰。

3. **#36177 保留已接纳的工具生成信息**  
   [链接](https://github.com/anomalyco/opencode/pull/36177)  
   确保工具调用始终绑定到模型所见的确切注册版本，消除因插件重载导致的 `aborted` 错误，增强并发安全性。

4. **#36184 修复 TUI 重连后 shell 状态协调**  
   [链接](https://github.com/anomalyco/opencode/pull/36184)  
   保留 shell 缓存的工作目录，并在服务重启后刷新非默认 shell 的计数，解决了终端视图的状态漂移问题。

5. **#36182 包裹会话创建状态更新以消除 UI 闪烁**  
   [链接](https://github.com/anomalyco/opencode/pull/36182)  
   用 SolidJS `startTransition` 批量提交新建会话后的多个状态变更，避免中间态导致的界面抖动。

6. **#36038 保持草稿重装载时的模型选择**  
   [链接](https://github.com/anomalyco/opencode/pull/36038)  
   将临时模型选择限定到标签页，防止切换提供者或路由时丢失当前草稿的模型配置。

7. **#36179 为每个提示创建根跨度以隔离 OTEL 跟踪**  
   [链接](https://github.com/anomalyco/opencode/pull/36179)  
   修复了 `OTEL_EXPORTER_OTLP_ENDPOINT` 启用时所有提示被合并成一个巨大 trace 的问题，实现一次提示一个跟踪。

8. **#36073 桌面端视觉改进**  
   [链接](https://github.com/anomalyco/opencode/pull/36073)  
   统一终端与审查面板背景色，修复审查面板字体错误，并为终端标签页添加了切换动画，提升视觉一致性。

9. **#36172 增加时间线初始消息预加载数量**  
   [链接](https://github.com/anomalyco/opencode/pull/36172)  
   将首次渲染获取的消息数从 2 条提升至 20 条，缓解加载历史时的视觉闪白，改善浏览流畅度。

10. **#36186 整合 V2 技术规格文档**  
    [链接](https://github.com/anomalyco/opencode/pull/36186)  
    为 V2 建立了权威的交叉模块契约索引，清理了过时的 API、配置和待办草稿，便于贡献者快速对齐。

## 功能需求趋势
- **V2 基础能力补全**：`@` 文件引用、会话分叉、统一文件监控和模型上下文钩子等议题集中出现，表明社区希望 V2 尽快达到与 V1 对等的日常生产效率。
- **成本与 token 管控强化**：从“纯问答模式”到零 token 响应处理、准确的上下文限制显示，开发者对用量透明度和节省 token 的机制要求愈发精细。
- **桌面端交互细节打磨**：归档恢复、选项卡最大化、双击聚焦等改进请求持续涌现，反映出桌面用户渴望更符合操作系统习惯的流畅体验。
- **权限与多工作目录支持**：多项议题要求更灵活的目录授权和会话级权限控制，以适应跨仓库和协作场景。

## 开发者关注点
- **数据安全与操作防呆**：无确认的会话关闭和快捷键冲突是容易造成工作丢失的痛点，需要在交互层面增加屏障。
- **V2 迁移过程中的稳定性**：自动压缩绕过、预运行错误静默、工具接纳争用等问题，影响了 V2 体验成熟度，核心贡献者正在密集修复。
- **进程资源泄漏**：ripgrep Worker 的 tmp 文件堆积、MCP 子进程残留等虽已关闭，但提醒社区需持续关注长时间运行的资源管理。
- **UI 反馈不足**：思考动画无进度、历史导航卡住、图片粘贴静默失败等，削弱了用户对操作完成预期的信心。

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

### 1. 今日速览
最新版本 v0.80.6 正式支持 GPT-5.6 与 Claude 新模型的 `max` 最高推理层级，社区对此功能呼声极高（👍16）。同时，围绕“严格工具/语法约束”（#6306）和“会话内模型变更应默认临时生效”（#5263）的讨论持续火热，多个关键 bug（思考块丢失、缓存统计、模型切换重试）已被快速修复合并。

### 2. 版本发布
- **v0.80.6**：引入 `max` 推理层级（`--thinking max`），原生支持 GPT-5.6 系列及 adaptive Claude 模型，覆盖 CLI、SDK、RPC 与模型选择器。自定义主题可定义 `thinkingMax`。[查看详情](https://github.com/earendil-works/pi/releases/tag/v0.80.6)
- v0.80.5 为常规迭代，未附详细说明。

### 3. 社区热点 Issues（10 个）

1. **#6306 [Open] 支持严格工具 / 语法约束**  
   22 评论 · 作者 mitsuhiko  
   探讨如何在 SDK 中表达“自由形式”与“严格”工具，涉及 LLM 语法感知探测，与 OpenAI SDK 的 LARK/ Rust 正则方案相关。这是结构化输出方向的核心需求。  
   [链接](https://github.com/earendil-works/pi/issues/6306)

2. **#5263 [Open] 会话内模型与思考层级变更应默认临时生效**  
   6 评论 · 👍6  
   希望 /model 与 /thinking 等指令只影响当前会话，全局默认值通过设置界面统一管理，避免误改导致意外消耗。  
   [链接](https://github.com/earendil-works/pi/issues/5263)

3. **#6206 [Open] 上下文窗口限制导致无法人为设置较小的 maxTokens**  
   7 评论 · 作者 DanielThomas  
   由于将 max_tokens 强钳至报告上下文窗口，用户无法主动限制输出长度以节省成本，与 maxTokens 语义区分不足。  
   [链接](https://github.com/earendil-works/pi/issues/6206)

4. **#6376 [Closed] 新版 Claude 模型思考块被错误剥离**  
   5 评论 · 已修复  
   fable 5、sonnet 5、opus 4.7/4.8 等模型因 Anthropic API 在无文字思考时不返回 thinking 内容，导致 Pi 误判为需要清除，影响推理链连贯性。  
   [链接](https://github.com/earendil-works/pi/issues/6376)

5. **#6097 [Open] 支持 `max` 思考层级**  
   2 评论 · 👍16  
   在 v0.80.6 中被实现，但 Issue 作为需求记录仍保留。用户极力希望引入 OpenAI GPT-5.6 Sol 的第六级推理深度。  
   [链接](https://github.com/earendil-works/pi/issues/6097)

6. **#5886 [Open] AgentSession 结束/继续与助手尾部消息生命周期 bug**  
   5 评论 · 👍2 · 作者 mitsuhiko  
   一组关于运行后逻辑在非就绪状态下尝试继续 agent 的元 bug，影响扩展和编码代理稳定性。  
   [链接](https://github.com/earendil-works/pi/issues/5886)

7. **#6303 [Open] 指数退避无上限，尽管存在 retry.provider.maxRetryDelayMs**  
   3 评论 · 👍1  
   实际退避计算缺少 maxDelayMs 约束，导致第 7 次重试等待约 4 分钟，严重影响交互体验。  
   [链接](https://github.com/earendil-works/pi/issues/6303)

8. **#2023 [Closed] 添加 pi.runWhenIdle() 以在 agent 完全空闲后调度工作**  
   14 评论 · 👍5  
   已关闭，但反映了扩展开发者对“agent 稳定后执行清理/重新加载”能力的急切需求，后续可能通过 #6363 等事件系统解决。  
   [链接](https://github.com/earendil-works/pi/issues/2023)

9. **#4973 [Closed] 提示模板多行参数被合并为单行**  
   4 评论 · 👍3  
   `$@` 传递用户输入时换行被压缩成空格，破坏代码块等多行内容，影响代码生成与复杂提示。  
   [链接](https://github.com/earendil-works/pi/issues/4973)

10. **#4147 [Closed] 使 agent.state.tools 的变更对运行中的 agent 循环可见**  
    4 评论 · 👍1  
    推动工具数组就地更新并传入循环上下文，解决扩展动态注册工具后 agent 无法立即使用的问题。  
    [链接](https://github.com/earendil-works/pi/issues/4147)

### 4. 重要 PR 进展（10 个）

1. **#6471 [Closed] 修正 GPT-5.6 Codex 上下文窗口**  
   将 Sol/Terra/Luna 的窗口从 272k 更正为 372k，对齐 OpenAI Codex 上游元数据。  
   [链接](https://github.com/earendil-works/pi/pull/6471)

2. **#6457 [Closed] 修复 Anthropic 空思考文本时仍发送 thinking 块**  
   确保新 Claude 模型在无文字思考时保留 thinking 块，修复 #6376。  
   [链接](https://github.com/earendil-works/pi/pull/6457)

3. **#6463 [Closed] 模型切换时取消进行中的自动重试**  
   防止 `/model` 后旧模型的退避重试继续运行，消除交互混乱。  
   [链接](https://github.com/earendil-works/pi/pull/6463)

4. **#6470 [Closed] 支持 shellPath 中的 `~` 展开**  
   允许在 shell 配置中使用波浪线路径，如 `~/.local/bin/agent-shell-wrapper`，提升跨环境一致性。  
   [链接](https://github.com/earendil-works/pi/pull/6470)

5. **#6460 [Closed] 添加 xAI Grok SuperGrok OAuth 登录**  
   新增基于设备码的 OAuth 提供商，保留原有 API Key 路径，方便订阅用户。  
   [链接](https://github.com/earendil-works/pi/pull/6460)

6. **#6449 [Closed] 将 ResourceExhausted 加入可重试错误**  
   增加对资源耗尽错误的自动重试，提高 API 调用韧性。  
   [链接](https://github.com/earendil-works/pi/pull/6449)

7. **#6427 [Closed] 添加提示缓存未命中追踪**  
   在对话轮次中检测并警告缓存未命中，标注空闲超时或模型切换原因，提升成本可见性。  
   [链接](https://github.com/earendil-works/pi/pull/6427)

8. **#6467 [Closed] 修复 git 包依赖缺失问题，优化 pnpm 安装**  
   解决 git 包 checkout 后 node_modules 不全导致的加载失败，尤其惠及 pnpm 用户。  
   [链接](https://github.com/earendil-works/pi/pull/6467)

9. **#6474 [Open] 概念验证：消息锚定动态工具加载**  
   允许在多轮对话中途引入新工具，而非必须在初始请求列表中全部声明，适用于 Anthropic 等后端。  
   [链接](https://github.com/earendil-works/pi/pull/6474)

10. **#6216 [Open] 添加 Amazon Bedrock Mantle OpenAI Responses 提供商**  
    基于 OpenAI Bedrock Provider，接入 AWS Bedrock Mantle 服务，扩展部署选项。  
    [链接](https://github.com/earendil-works/pi/pull/6216)

### 5. 功能需求趋势
- **高级推理与多模型支持**：从 `max` 思考层级的快速落地可见，社区对 GPT-5.6、Claude 新模型及推理深度的需求强烈，并希望目录和缓存统计能同步跟上。
- **结构化输出与严格工具**：开发者希望在 SDK 中规范自由/严格工具，支持语法约束，这是构建确定性代理的关键缺口。
- **会话与状态管理优化**：模型与思考切换应为临时行为、会话快速加载（Fast Sessions）、避免上下文窗口强制限制等，反映出对控制精度和低打扰的追求。
- **扩展与生命周期事件**：`runWhenIdle`、`agent_idle`、工具动态注入等需求，表明扩展系统需要更细腻的钩子和状态同步。
- **成本与性能可见性**：缓存未命中追踪、重试退避上限、token 预算修正等，显示用户对资源消耗和响应可靠性的监控需求上升。

### 6. 开发者关注点
- **模型响应处理**：思考块渲染异常、空 HTML 注释显示、多行模板被压扁等问题直接影响输出质量与可读性。
- **Token 与成本控制**：上下文窗口限制过死、压缩后输出预算计算过小、缓存写入统计错误，开发者希望有更精确的计量和更灵活的配置。
- **工具链健壮性**：扩展加载路径不一致、模型 ID 含括号无法选择、全局包与开发版冲突等，说明安装和配置体验仍需打磨。
- **交互细节**：模型切换时未取消重试、Escape 键卡在 Working 状态、Ctrl-P 期望复用命令行习惯等，影响日常使用流畅度。
- **新模型接入速度**：在 GPT-5.6 发布后，社区快速提交目录、上下文窗口纠正和登录支持，体现出对多厂商模型及时跟进的高期望。

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code 社区动态日报

**日期：2026 年 7 月 10 日**

---

## 1. 今日速览

今日 Qwen Code 发布了新的 nightly 构建 `v0.19.8`，紧急修复了子代理工具调用死循环的问题。社区围绕**多工作区守护进程**（daemon multi-workspace）的 RFC 讨论持续活跃，相关实现已进入 PR 审核阶段；同时，macOS 下粘贴图片失效、Windows 控制台乱码等跨平台体验问题引起广泛反馈，表明终端交互打磨仍是当前社区关注的重点。

---

## 2. 版本发布

**v0.19.8-nightly.20260710.205430235**  
- 修复了子代理（subagent）工具调用死循环，避免重复 Llama 工具调用导致的会话卡死（#6543）。  
- 增强了会话历史链断裂检测与标记能力（`fix(session): detect and mark broken history chains`）。  
- 新增 `cua-driver-rs` 预编译二进制 v0.7.1，支持相对坐标，已预置在 `packages/cua-driver` 下，覆盖 macOS（经公证与签名）、Linux、Windows。  
  *详见 [Release v0.19.8-nightly](https://github.com/QwenLM/qwen-code/releases/tag/v0.19.8-nightly.20260710.205430235)*

---

## 3. 社区热点 Issues（10 条）

| # | 编号 | 标题 | 热度 | 摘要 |
|---|------|------|------|------|
| 1 | [#6378](https://github.com/QwenLM/qwen-code/issues/6378) | RFC: 支持单个 `qwen serve` 守护进程管理多个工作区 | 20 💬 | 建议在保持现有单工作区客户端兼容的前提下，允许一个守护进程同时服务多个工作区。评论中深入探讨了架构设计、会话隔离与性能影响，是多工作区能力的核心设计蓝本。 |
| 2 | [#6560](https://github.com/QwenLM/qwen-code/issues/6560) | 希望恢复对话中直接上传、拖拽图片和文档的功能 | 18 💬 | 用户反映 CLI 界面无法像之前版本那样通过 Ctrl+V 粘贴图片或拖拽文件，被迫使用 `read_file` 工具绕行，严重破坏交互流畅度。大量用户附议，要求恢复原有体验。 |
| 3 | [#6581](https://github.com/QwenLM/qwen-code/issues/6581) | JetBrains ACP 代理未收到用户提示，仅收到启动上下文 | 8 💬 | 在 IntelliJ IDEA 中使用本地 Ollama 模型的 Qwen Code 作为 AI 助手时，ACP 代理无法转发用户提示，仅返回系统引导信息。该问题直接影响 IDE 集成的可用性。 |
| 4 | [#6565](https://github.com/QwenLM/qwen-code/issues/6565) | 连接到 Qwen Coder 时出现 “Internal Error” | 7 💬 | 多位用户报告认证或连接时出现内部错误，截图显示界面提示 “糟糕！连接到 Qwen Coder 时出现问题”。社区正协助排查是否与 API 网关或本地网络有关。 |
| 5 | [#3696](https://github.com/QwenLM/qwen-code/issues/3696) | 全面热重载系统：技能、扩展、MCP 和配置 | 5 💬 | 要求实现运行时热重载，无需重启会话即可更新技能、扩展、MCP 服务器等。已部分实施，剩余工作被跟踪，是提升开发效率的关键需求。 |
| 6 | [#6600](https://github.com/QwenLM/qwen-code/issues/6600) | v0.19.8 `--debug` 打印日志路径但调试日志文件从未创建 | 4 💬 | 携带 `--debug` 标志启动时，CLI 显示日志路径，但磁盘上从未实际创建文件，亦未创建 `latest` 符号链接。影响调试和问题反馈。 |
| 7 | [#6629](https://github.com/QwenLM/qwen-code/issues/6629) | Cron 解析器在单值表达式（如 `5/15`）中丢弃步进 | 3 💬 | 五字段 cron 解析器错误地将 `5/15` 当作单个值 `5` 处理，导致步进功能失效。虽语法已被接受，但行为不符合预期。 |
| 8 | [#6595](https://github.com/QwenLM/qwen-code/issues/6595) | qwen3.7-max 模型可能泄露 `<analysis>`/`<summary>` 标签并停止后续操作 | 3 💬 | 在长上下文或复杂工具调用中，模型可能直接将内部协议标签输出为助手文本，导致代码将输出视为普通回复，从而跳过后续动作。影响模型生成质量和工具链可靠性。 |
| 9 | [#6614](https://github.com/QwenLM/qwen-code/issues/6614) | Glob 工具在超大目录扫描时可能 OOM，输出截断前内存溢出 | 2 💬 | 子代理执行 `glob` 搜索整个 Qwen Code 仓库时，Node 进程因堆内存耗尽而崩溃，即使输出结果会被截断，但扫描阶段已超出限制。急需内存安全的遍历策略。 |
| 10 | [#6601](https://github.com/QwenLM/qwen-code/issues/6601) | Shell 子进程继承敏感环境变量导致凭证泄露 | 2 💬 | 子进程通过 `process.env` 继承完整的守护进程环境，暴露 `QWEN_SERVER_TOKEN`、API 密钥等敏感信息，只需执行 `printenv` 即可泄露。属于 P1 安全漏洞。 |

---

## 4. 重要 PR 进展（10 个）

| # | 编号 | 标题 | 状态 | 亮点 |
|---|------|------|------|------|
| 1 | [#6635](https://github.com/QwenLM/qwen-code/pull/6635) | feat(cli): 按工作区分组守护进程通道工作进程（多工作区阶段 4b） | 开放 | 允许 `qwen serve` 守护进程为每个绑定的工作区托管通道工作进程，实现非主工作区的通道也能运行，是多工作区架构的关键里程碑。 |
| 2 | [#6621](https://github.com/QwenLM/qwen-code/pull/6621) | feat(cli): 工作区合格的 ACP 传输（守护进程多工作区阶段 4） | 开放 | 添加了针对每个工作区的 ACP 端点，当守护进程绑定多个工作区时，客户端可通过特定端点连接到对应工作区。 |
| 3 | [#6599](https://github.com/QwenLM/qwen-code/pull/6599) | ci: 添加可疑评论附件守卫 | 开放 | 新增 GitHub Actions 审核守卫，自动删除非可信用户发布的带有高风险附件（压缩包、安装程序、脚本等）的评论，增强社区安全。 |
| 4 | [#6637](https://github.com/QwenLM/qwen-code/pull/6637) | test(core): 稳定文件历史淘汰测试 | 开放 | 为 I/O 密集型的文件历史快照淘汰测试增加超时时间，修复自托管 Linux 运行器中因性能不足导致的失败问题。 |
| 5 | [#6619](https://github.com/QwenLM/qwen-code/pull/6619) | feat(scheduled-tasks): 为独立运行添加前置条件闸门 | 开放 | 定时任务现在可以携带前置条件，仅在评估通过后才会在新子会话中执行提示词，提高了自动化调度的灵活性与安全性。 |
| 6 | [#6625](https://github.com/QwenLM/qwen-code/pull/6625) | feat(web-shell): 新会话的工作区选择器（守护进程多工作区阶段 4） | 开放 | 在 Web Shell 侧边栏增加工作区下拉选择器，用户可在多工作区守护进程中为新建聊天选择目标工作区。 |
| 7 | [#6633](https://github.com/QwenLM/qwen-code/pull/6633) | fix(web-shell): 对齐分屏查看的聊天交互 | 开放 | 修复分屏模式下聊天面板的交互缺陷，使其支持后续建议、立即发送提示词以及排队输入，与普通单面板行为一致。 |
| 8 | [#6626](https://github.com/QwenLM/qwen-code/pull/6626) | feat(web-shell): 改进 Markdown 表格可读性 | 开放 | 在 Web Shell 中增加表格密度切换、长文本展开/折叠、斑马条纹、工具提示等显示控制功能，显著提升密集数据表格的可读性。 |
| 9 | [#6638](https://github.com/QwenLM/qwen-code/pull/6638) | feat(cli): 工作区合格的扩展 REST 接口（守护进程多工作区） | 开放 | 为多工作区守护进程增加每个工作区独立的扩展管理 REST API，弥补了之前仅主工作区可管理扩展的短板。 |
| 10 | [#6019](https://github.com/QwenLM/qwen-code/pull/6019) | feat(cli): 添加 `/model --compaction` 用于配置聊天压缩模型 | 开放 | 允许用户指定专用的模型用于聊天压缩（自动压缩），增强了长对话管理能力，受到希望控制压缩质量和成本的开发者的期待。 |

---

## 5. 功能需求趋势

从近期 Issues 和 PR 讨论中可提炼出以下几个高热度方向：

- **多工作区与守护进程架构**：Issue #6378 及其关联的一系列 PR（如 #6621、#6635、#6638）反映出社区对单个守护进程管理多个项目的强需求，推动该功能快速迭代。
- **终端/跨平台交互体验**：大量反馈聚焦 macOS 粘贴图片失效 (#6590)、Windows 控制台乱码 (#6214)、拖拽上传失效 (#6560) 等平台兼容性问题，用户期望 CLI 的交互达到现代 IDE 水平。
- **IDE 集成与代理稳定性**：JetBrains ACP 代理故障 (#6581)、延期 IDE 启动显示过期错误 (#6507) 等问题表明，将 Qwen Code 无缝嵌入开发环境仍是关键挑战。
- **安全与可靠性增强**：从凭证泄露 (#6601)、敏感环境变量清理，到可疑评论过滤 (#6599)，安全防护正在从被动修补转向主动预防。
- **观察性与调试工具**：热重载 (#3696)、子代理执行可见性 (#6569)、`--debug` 日志修复 (#6600) 等需求凸显用户对工具链可观测性的重视，期望更透明的运行状态和更高效的排错手段。

---

## 6. 开发者关注点

- **平台一致性问题频发**：macOS 独立安装包缺失原生模块导致图片粘贴失效、Windows 非 UTF-8 控制台输出乱码、Alt+V 无法粘贴截图等现象表明，跨平台打包和终端适配仍需系统化测试与修复。
- **模型输出与工具循环稳定性**：`qwen3.7-max` 标签泄露 (#6595)、子代理死循环修复 (#6543)、PDF 读取死循环 (#6586) 等问题反复提醒开发者，模型与工具链的交互边界需要更严格的保护和异常恢复机制。
- **性能与资源控制**：Glob 工具 OOM (#6614)、调试日志文件不创建 (#6600)、文件历史测试不稳定 (#6637) 等暴露了大规模资源操作下的健壮性不足，性能调优和内存管理成为持续集成中的高频痛点。
- **认证与连接诊断困难**：Internal Error 错误 (#6565) 和多用户订阅无法使用 (#6477) 等，反映认证链路缺少详细的错误码和自助诊断工具，社区期望更透明的故障定位手段。
- **功能回归导致的体验断层**：之前可用的图片上传、拖拽功能突然失效，表明发布流程中需增强端到端回归测试，尤其是交互式 CLI 场景的覆盖。

---

> 数据统计：过去 24 小时共更新 33 个 Issue，50 个 Pull Request，社区活跃度保持高位。

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI 社区动态日报（2026-07-10）

## 今日速览
v0.8.68 里程碑进入收尾冲刺，核心编排模型（Fleet/Workflow/Lane/Runtime）的讨论与实现并行推进，多项长期存在的性能与 UX 痛点被集中修复。同时，Termux/Android 官方支持与 xAI（Grok）集成两大用户呼声强烈的功能已通过 PR 落地，CI 安全审计与加速也已完成。

## 版本发布
无新版本发布，但 v0.8.68 发布准备 PR 已合并，即将推出。

## 社区热点 Issues
1. [#4092](https://github.com/Hmbown/CodeWhale/issues/4092) **v0.8.68 执行看板：Lane 顺序、依赖与代理协议** — 作为里程碑的唯一入口看板，定义了每个 issue 的 Lane 归属，评论数高达 58，是当前协作的核心跟踪器。
2. [#4032](https://github.com/Hmbown/CodeWhale/issues/4032) **Codewhale 未遵循“宪法”约束** — 用户反映代理在已有专有脚本时仍自行编写临时脚本，社区热议代理指令遵循性问题，评论 30 条。
3. [#4042](https://github.com/Hmbown/CodeWhale/issues/4042) **[CLOSED] 环境级工具沙盒** — 为子代理执行上下文强制实施工具限制的方案，已关闭，但设计思路将直接影响 Fleet 角色安全模型。
4. [#4014](https://github.com/Hmbown/CodeWhale/issues/4014) **高并发下 TUI 卡顿与内存压力** — 当 30+ 子代理并行时，终端渲染严重延迟，是性能改进的重点驱动力。
5. [#4178](https://github.com/Hmbown/CodeWhale/issues/4178) **Stopship 工作流作为 Fleet Lane 的狗粮** — 实战验证 Fleet/Workflow 模型，推动 Lane 在真实 stopship 场景中落地。
6. [#4257](https://github.com/Hmbown/CodeWhale/issues/4257) **[CLOSED] 添加 xAI (Grok) 一线提供商** — 结束了仅能通过 OpenAI 兼容路径使用 Grok 的历史，OAuth 和 API Key 双路径已实现。
7. [#4175](https://github.com/Hmbown/CodeWhale/issues/4175) **架构定型追踪：Fleet/Workflow/Lane/Runtime** — 定义编排词汇与职责分离，避免概念混淆，是后续所有工作流实现的基础。
8. [#4236](https://github.com/Hmbown/CodeWhale/issues/4236) **Termux / Android arm64 官方支持史诗** — 响应社区长期请求，旨在让 CLI/TUI 原生运行于 Termux，已有构建修复 PR。
9. [#4308](https://github.com/Hmbown/CodeWhale/issues/4308) **MCP 发现容错与工具描述截断** — 部分 MCP 服务仅实现 tools/list 却阻塞 resources/list，导致连接失败；工具列表显示过长，影响可用性。
10. [#4095](https://github.com/Hmbown/CodeWhale/issues/4095) **TUI 默认展示过于杂乱** — 社区反馈默认 UI 信息过载，要求将紧凑模式作为标准体验。

## 重要 PR 进展
1. [#4327](https://github.com/Hmbown/CodeWhale/pull/4327) **v0.8.68 发布准备** — 统一版本号、更新文档与安装脚本，为即将发布的里程碑版本完成最终拼图。
2. [#3902](https://github.com/Hmbown/CodeWhale/pull/3902) **TUI 五大渲染/输入热路径修复** — 一次修复任务栏重复计算、文件树同步阻塞等 5 个性能问题，显著降低 UI 卡顿。
3. [#4243](https://github.com/Hmbown/CodeWhale/pull/4243) **runtime_threads 使用 parking_lot::Mutex** — 将热锁点从 std::sync 迁移到 parking_lot，提升子代理并发管理效率。
4. [#4310](https://github.com/Hmbown/CodeWhale/pull/4310) **CI 关键路径优化** — 移除每合并一次即重构建 nightly 的浪费，PR 迭代周期大幅缩短。
5. [#4314](https://github.com/Hmbown/CodeWhale/pull/4314) **xAI 设备码 OAuth 入口** — 完成 `codewhale auth xai-device` 与 TUI 引导，Grok 集成完整可用。
6. [#4315](https://github.com/Hmbown/CodeWhale/pull/4315) **修复 Termux 构建与 rustls 崩溃** — 使 Android arm64 目标可编译启动，解决 rquickjs 绑定与 JNI 恐慌问题，正式支持 Termux。
7. [#4025](https://github.com/Hmbown/CodeWhale/pull/4025) **CI 轻量分类，跳过无改动平台** — 仅脚本变更不再触发全平台测试，节省 20 分钟以上 CI 资源。
8. [#4313](https://github.com/Hmbown/CodeWhale/pull/4313) **宪法重平衡** — 回复精简的行为指导，在自主性与约束间取得更好的提示工程平衡。
9. [#4311](https://github.com/Hmbown/CodeWhale/pull/4311) **新增 GPT-5.6 与 Muse Spark 路由** — 扩展最新模型家族支持，为开发者提供更多模型选择。
10. [#4328](https://github.com/Hmbown/CodeWhale/pull/4328) **依赖安全升级** — 修复 crossbeam-epoch、lopdf 等 CVE，保障供应链安全。

## 功能需求趋势
- **编排与工作流引擎**：Fleet/Workflow/Lane/Runtime 四层架构成为社区当前最关注的方向，致力于将手动 tmux/worktree 管理变成一等公民。
- **多模型与多 provider 扩展**：xAI、GPT-5.6、Muse Spark 等新模型快速集成，以及 Termux 等新平台支持，反映用户对异构环境与前沿模型的迫切需求。
- **性能与资源优化**：TUI 高并发下的渲染卡顿、subagents.v1.json 无限膨胀等问题频繁被提及，表明大规模代理协作场景已进入日常使用。
- **开发者体验与 UX 重构**：默认 TUI 过载、MCP 服务容错弱、宪法遵循性不足等反馈密集，指向“开箱即用”的体验仍需打磨。
- **安全与隔离**：工具沙盒、worktree 隔离、角色门控等需求在多个 issue 中交叉出现，安全性正从被动修复转向主动设计。

## 开发者关注点
- **TUI 性能与内存**：长时间运行或高并发代理时，终端响应变慢、内存持续增长，已成为高频反馈点。
- **代理行为一致性**：代理无视用户提供的脚本或突破约束，导致项目规范被打破，信任感降低。
- **MCP 生态的脆弱性**：第三方 MCP 服务的非标准实现常导致连接失败或 UI 刷屏，迫切需要容错和展示优化。
- **配置与 provider 设置门槛**：虽然 setup wizard 有所改善，但多 provider 配置、API key 管理仍是新手卡点。
- **大规模 Session 的状态管理**：subagents.v1.json 无界增长、未知子代理标签等问题，暴露出长期运行会话的状态清理与身份元数据缺失。

</details>

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*