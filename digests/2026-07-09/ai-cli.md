# AI CLI 工具社区动态日报 2026-07-09

> 生成时间: 2026-07-09 03:55 UTC | 覆盖工具: 9 个

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

# AI CLI 工具社区动态横向对比分析（2026-07-09）

## 1. 生态全景

2026 年中，AI CLI 工具已彻底从“命令行辅助”演变为可自主执行复杂任务的 Agent 平台。成本、可靠性和安全正取代单纯的功能扩充，成为社区最尖锐的话题。多 Agent 协作、持久化工作流、模型灵活路由等需求持续升温，Windows/Linux 兼容性与企业级部署适配仍存在显著痛点。整体呈现“功能探索期的热切”与“规模化落地前的阵痛”并存态势。

## 2. 各工具活跃度对比

| 工具 | 今日 Issues 数 | 今日 PR 数 | 新 Release | 社区活跃特征 |
|------|---------------|------------|------------|-------------|
| **Claude Code** | 10个高热度，多可达43💬 | 5个更新 | v2.1.205 | 成本焦虑、Cowork 稳定性问题集中爆发 |
| **OpenAI Codex** | 10+个（集中反馈工具命名 bug） | 10个活跃 | 0.144.0-alpha.2（Rust） | 0.143.0 回退导致大面积功能阻塞，内部可观测性改进密集 |
| **Gemini CLI** | 10个高关注度 | 10个（安全修复为主） | v0.50.0 / v0.51.0-preview.0 | 子代理挂起、安全漏洞修补成为焦点 |
| **GitHub Copilot CLI** | 10个 | 0 | 无 | 模型配置缺陷、会话恢复限制，交互低但问题集中 |
| **Kimi Code CLI** | 1个（仅活跃） | 0 | 无 | 极度平静，仅遗留企业 SSL 需求 |
| **OpenCode** | 10个（集中在安全/稳定性） | 10个（大量修复） | 无 | 安全争议与稳定性修复并行，未见新版本 |
| **Pi** | 43个（大量已关闭） | 8个（快速修复） | 无 | 高反馈率，快速关闭问题；多会话需求上升 |
| **Qwen Code** | 10个（含 RFC） | 10个（性能与架构优化） | v0.19.8 | 多工作区、文件上传回归，架构讨论热烈 |
| **DeepSeek TUI** | 10个（v0.8.68 元车道推动） | 10+个合并 | 无 | 高频率合并，推动 Agent 协议、跨平台、多提供商 |

注：Issue/PR 数量为日报中精选数，反映社区关注热点，不代表总变更量。

## 3. 共同关注的功能方向

- **Agent 自主性与多 Agent 协作**：Claude Code（分层 Opus 大脑 + Sonnet 工人）、Gemini CLI（子代理误报成功/挂起）、OpenCode（多 LLM 辩论）、Qwen Code（子代理推理循环、Advisor 角色）、DeepSeek TUI（AgentProfile 与子代理路由）均表明社区迫切期待可靠的长程自主任务与代理间协作。
- **成本管控与 Token 消耗可视化**：Claude Code 和 OpenAI Codex 均出现极端消耗疑义，Pi 引入缓存未命中追踪，Claude Code 还出现模型计费偏差；成本透明化成跨工具共识。
- **模型/提供商灵活路由**：Codex 有模型家族别名需求，Pi 期望临时模型更改，DeepSeek TUI 为子代理分配不同提供商，显示出用户希望在不同任务阶段调用最适宜的模型。
- **会话与记忆持久化**：Copilot CLI（/resume 限制）、Claude Code（删除会话命令）、OpenCode（工作区持久化）、Pi（多并发会话）等都强调会话管理、记忆连续性需求。
- **安全与沙箱**：Gemini CLI 密集修复 RCE 漏洞，OpenCode 出现 pip 自动安装争议，Copilot CLI 数据外泄误伤，Qwen Code 环境隔离，证明安全从“可选”变为“基线”。
- **跨平台稳定性**：Windows 虚拟化、NFS/内存文件系统、Wayland 兼容性等多个工具持续受困，平台适配仍是短板。

## 4. 差异化定位分析

| 工具 | 功能侧重 | 目标用户 | 技术路线差异 |
|------|---------|---------|------------|
| **Claude Code** | Cowork 协作、长期自主代理 | 需要跨 VM 执行的企业用户 | 与 Anthropic API 深度绑定，侧重于沙箱环境和上下文管理 |
| **OpenAI Codex** | 终端 + 桌面 + 远程场景统一 | 全平台开发者，尤其注重沙箱安全的用户 | 多端 (CLI/Desktop/Chrome) 统一执行服务器，强调可观测性 |
| **Gemini CLI** | 多技能与子代理、安全性加固 | Google 生态开发者 | 基于 Gemini 模型，注重工具注册与技能扩展，安全问题响应迅速 |
| **GitHub Copilot CLI** | 模型切换、会话管理 | GitHub 生态用户，希望与 Copilot 扩展一致的 CLI 体验 | 受限于 GitHub API 与设置体系，模型配置体验脆弱 |
| **Kimi Code CLI** | 基础交互、企业适配 | 国内企业环境用户 | 明显静默期，企业 SSL/TLS 问题悬而未决，生态尚处早期 |
| **OpenCode** | 多提供商集成、桌面端 + Zed 等 IDE 集成 | 偏好开源、自托管的开发者 | 插件化、支持多种模型后端，但受限于社区贡献的稳定性 |
| **Pi** | 多模型、多会话、压缩优化 | 对成本和模型灵活性敏感的个人开发者 | 高度可定制，支持会话元数据、缓存追踪，但 Agent 生命周期尚需强化 |
| **Qwen Code** | 多工作区、Web Shell 与自动化 | 企业级多租户场景、CI/CD 集成 | 依赖 Qwen 模型，强化服务端多工作区和 webhook 集成 |
| **DeepSeek TUI** | 终端 Agent 协议、多提供商与跨平台 | 偏好 TUI、控制力强的开发者 | 极致 TUI 效率，AgentProfile 协议驱动，支持 Android/Termux |

## 5. 社区热度与成熟度

- **最活跃**：DeepSeek TUI（超高 PR 合并频率，明确定义的里程碑）；Pi（43 issue 更新，闭环速度快）；OpenAI Codex（故障引发集中反应，内部投入明显）。
- **处于快速迭代但稳定性待提升**：Gemini CLI（安全补丁密集）、Claude Code（成本黑洞、Cowork 故障反映架构还不稳固），Qwen Code（正在架构升级推进多工作区）。
- **低活跃/观望期**：Kimi Code CLI 极少互动；GitHub Copilot CLI 今日无 PR，历史高赞需求未响应，社区热度不高。
- **成熟度信号**：Claude Code 和 Codex 已有大型企业用户基础，但仍被核心稳定性问题困扰；DeepSeek TUI 和 Pi 通过节奏极快的迭代体现早期活力。

## 6. 值得关注的趋势信号

1. **AI CLI 正从“辅助”走向“编排大脑”**：多个工具提出持久化状态、子代理递归、长期运行需求，预示将出现真正的自主开发 Agent，开发者需关注其可靠性与信任边界。
2. **成本失控将倒逼工具透明化**：大量用户报告短时间内配额耗尽，未来工具必须内置 token 消耗预估、异常检测和预算告警，这也成为选型关键指标。
3. **安全问题成成败门槛**：Gemini CLI 的 RCE、OpenCode 的 pip 自动安装等显示，在自动执行代码的环境下，安全漏洞影响比以往更致命，企业采购前必然严审。
4. **跨平台兼容性仍是痛点**：Windows 虚拟化、NFS 文件系统、Wayland 等系统环境持续出现特有 Bug，说明当前工具团队测试资源多集中于 macOS，开发者若在非主流环境使用需提前评估风险。
5. **多模型路由与本地模型需求上升**：社区期望一个工具基座能调度不同模型、支持本地部署（LM Studio、Ollama 等），未来数据隐私和延迟可控将成为核心竞争力。
6. **会话即资产**：会话管理、记忆持久化、跨设备恢复需求强烈，AI CLI 将更加类似长期运行的服务，而非一次性对话工具，架构设计需围绕会话生命周期。

---

*报告基于 2026-07-09 各工具社区动态日报，数据来源为对应 GitHub 公开仓库，仅反映当日窗口期内的社区活跃情况。*

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

## Claude Code Skills 社区热点报告
数据截止 2026-07-09，来源：`anthropics/skills` 官方仓库

### 1. 热门 Skills 排行
评论与关注度最高的功能型 Skills PR（状态均为 *Open*）：

- **技能评估/安全分析器套件** [#83](https://github.com/anthropics/skills/pull/83)  
  *功能*：`skill-quality-analyzer`（五维度质量评分）和 `skill-security-analyzer`。  
  *讨论热点*：社区对技能可审核性、安全性的需求强烈，希望用“元技能”自动化质检。  
  *状态*：Open

- **文档排版技能** [#514](https://github.com/anthropics/skills/pull/514)  
  *功能*：防止 AI 生成文档中的孤行、寡段、编号错乱等排版瑕疵。  
  *讨论热点*：专注于“用户很少要求但普遍存在”的痛点，具有广泛落地价值。  
  *状态*：Open

- **ODT 文档技能** [#486](https://github.com/anthropics/skills/pull/486)  
  *功能*：创建、填充、转换 OpenDocument 格式（.odt/.ods），支持 LibreOffice 生态。  
  *讨论热点*：对开源办公套件用户的填补空白型支持，引发多人关注兼容性。  
  *状态*：Open

- **前端设计技能优化** [#210](https://github.com/anthropics/skills/pull/210)  
  *功能*：重写 `frontend-design` 技能，提升指令清晰度与可执行性。  
  *讨论热点*：社区强调技能必须“单次对话可执行”，要求精炼 token 效率。  
  *状态*：Open

- **测试模式技能** [#723](https://github.com/anthropics/skills/pull/723)  
  *功能*：覆盖测试哲学、单元测试、React 组件测试的全栈测试指导。  
  *讨论热点*：将 AI 辅助编程延伸到测试实战，满足开发者对质量保障的诉求。  
  *状态*：Open

- **色彩专家技能** [#1302](https://github.com/anthropics/skills/pull/1302)  
  *功能*：提供色彩命名体系、色彩空间选用指南、可访问色阶生成等专业知识。  
  *讨论热点*：独立、自包含的领域知识技能，设计类工作流需求高。  
  *状态*：Open

- **自审计技能** [#1367](https://github.com/anthropics/skills/pull/1367)  
  *功能*：在交付前对 AI 输出进行机械验证与四维推理审计（按伤害严重度排序）。  
  *讨论热点*：将安全与可靠性内建到每次输出，契合社区对信任与质量的要求。  
  *状态*：Open

### 2. 社区需求趋势
从 Issues 中提炼的主要期待方向：

- **安全与信任边界**  
  社区强烈关注技能分发中的信任冒用问题（[#492](https://github.com/anthropics/skills/issues/492)，34 评论），要求区分官方与社区技能，防止 `anthropic/` 命名空间被滥用。

- **跨组织共享与协作**  
  企业用户希望通过链接或库实现组织内一键共享技能（[#228](https://github.com/anthropics/skills/issues/228)，14 评论），且出现将技能暴露为 MCP 接口的呼声（[#16](https://github.com/anthropics/skills/issues/16)）。

- **技能创建工具的稳定性与跨平台**  
  集中体现在评价与优化脚本的召回率 bug（[#556](https://github.com/anthropics/skills/issues/556)，12 评论；[#1169](https://github.com/anthropics/skills/issues/1169)），以及 Windows 兼容性问题（[#1061](https://github.com/anthropics/skills/issues/1061)）。社区要求工具链“开箱即用”。

- **高级智能与治理技能**  
  提议包括紧凑记忆符号（[#1329](https://github.com/anthropics/skills/issues/1329)，9 评论）和代理治理（[#412](https://github.com/anthropics/skills/issues/412)），希望 AI 拥有更可控的状态管理和安全策略。

- **已有技能去重与维护**  
  文档/示例技能安装导致重复（[#189](https://github.com/anthropics/skills/issues/189)，6 评论）、参考网站可用性（[#184](https://github.com/anthropics/skills/issues/184)）等问题也反映社区对生态稳定性的关注。

### 3. 高潜力待合并 Skills
以下 Open 状态的 PR 因解决痛点、讨论活跃，可能近期落地：

- **修复评估工具召回率（0% recall）**  
  [#1298](https://github.com/anthropics/skills/pull/1298)（关联 #556）、[#1323](https://github.com/anthropics/skills/pull/1323)（关联 #1169）— 多个独立复现报告，核心评价循环瘫痪，合并优先级极高。

- **Windows 平台兼容性修复**  
  [#1099](https://github.com/anthropics/skills/pull/1099)（管道读取崩溃）、[#1050](https://github.com/anthropics/skills/pull/1050)（子进程与编码）— 彻底解决 Windows 用户无法使用技能创建器的问题。

- **YAML 特殊字符验证与 UTF-8 崩溃修复**  
  [#539](https://github.com/anthropics/skills/pull/539)/[#361](https://github.com/anthropics/skills/pull/361)（描述字段误解析）、[#362](https://github.com/anthropics/skills/pull/362)（多字节字符引发 Panic）— 提升技能描述可靠性，防止静默失败。

- **DOCX/PDF 技能文档引用修复**  
  [#541](https://github.com/anthropics/skills/pull/541)（跟踪修订 ID 碰撞）、[#538](https://github.com/anthropics/skills/pull/538)（大小写文件引用）— 确保文档生成技能不会破坏现有文件。

- **待合并的功能技能**  
  `testing-patterns` [#723](https://github.com/anthropics/skills/pull/723) 与 `document-typography` [#514](https://github.com/anthropics/skills/pull/514) 评论活跃，覆盖面广，可能率先进入市场。

### 4. Skills 生态洞察
当前社区最集中的诉求是：**让技能创建与评估链条在跨平台环境下可信、稳定且安全，并以可审核、可共享的方式分发高质量技能。**

---

# Claude Code 社区动态日报 | 2026-07-09

## 1. 今日速览
今日社区焦点集中在 **token 成本失控与 Cowork 功能稳定性** ——多个高热度 issue 反映出 API 配额在几十分钟内耗尽、Cowork VM 文件同步失效等严重问题。同时，社区对**自主 Agent 长期运行、分层模型协同**的呼声达到新高，相关需求讨论量激增。

## 2. 版本发布
**v2.1.205** 已发布，主要修复：
- 增加自动模式规则：阻止恶意篡改会话转录文件
- 修复 `--json-schema` 在无效 schema 时静默输出非结构化内容的问题，并修复 `format` 关键字被拒绝的 bug
- 修正 Claude 工作中消息被静默截断的问题

## 3. 社区热点 Issues（Top 10）
1. **[#42249 极端 token 消耗：正常使用几分钟耗尽配额](https://github.com/anthropics/claude-code/issues/42249)**  
   43💬 17👍 · 描述：一次中等交互的会话在约 1 小时内耗尽每日配额，用户怀疑计费或上下文管理异常。高赞高评论，成本焦虑的代表。

2. **[#56913 让 Claude Code 真正自主运行：分层 Opus 大脑 + Sonnet 工人 + 持久状态](https://github.com/anthropics/claude-code/issues/56913)**  
   43💬 · 社区设想将 Claude Code 作为长期编排大脑，而非仅结对编程工具，要求支持分层代理和持久工作流。

3. **[#38993 Cowork FUSE 挂载返回截断/陈旧文件 (Windows)](https://github.com/anthropics/claude-code/issues/38993)**  
   41💬 28👍 · Cowork 环境中主机文件变更无法同步到 VM，严重影响协作准确性，👍数极高。

4. **[#54776 莫名高额 API 消耗：1-2 小时用光 100% 配额](https://github.com/anthropics/claude-code/issues/54776)**  
   33💬 12👍 · 20x 客户报告使用量突然从 10% 飙升到 100%，持续多日，指向计费或后台调用问题。

5. **[#74649 Cowork 在 Win11 Pro 上完全不可用（缺少 HCS 服务）](https://github.com/anthropics/claude-code/issues/74649)**  
   25💬 · 新 issue 热度迅速攀升，Windows 用户无法启动 Cowork 环境，缺少虚拟化组件。

6. **[#61993 子代理无法继续派生子代理](https://github.com/anthropics/claude-code/issues/61993)**  
   19💬 · 代理嵌套功能缺失，限制了复杂自主任务的分解，是自治 Agent 落地的重要障碍。

7. **[#67506 Fable 5 模型 token 消耗与描述不符](https://github.com/anthropics/claude-code/issues/67506)**  
   17💬 1👍 · 新模型计费疑似存在偏差，可能涉及平台侧计量错误。

8. **[#45178 Cowork 报“跨设备链接”错误 (Windows + OneDrive)](https://github.com/anthropics/claude-code/issues/45178)**  
   14💬 · 文件重命名操作在 OneDrive 路径下失败，Windows 用户又一致命兼容问题。

9. **[#26904 增加 /delete 命令以删除当前会话](https://github.com/anthropics/claude-code/issues/26904)**  
   9💬 51👍 · 高赞功能请求，用户急需会话管理能力，避免无用会话堆积。

10. **[#68354 工具调用时输出来路“call”/“court” token，XML 被当作文本打印](https://github.com/anthropics/claude-code/issues/68354)**  
    7💬 8👍 · Windows 本地及云端 Cowork 均出现该解析异常，影响代理操作可靠性。

## 4. 重要 PR 进展（过去 24h 更新）
1. **[#75938 修复 sweep 脚本：markStale 无法标记任何 issue](https://github.com/anthropics/claude-code/pull/75938)**  
   解决标签脚本因分页与跳过逻辑导致的失效问题。

2. **[#41447 开源 Claude Code（草案）](https://github.com/anthropics/claude-code/pull/41447)**  
   关联合并多个早期 issue，虽为早期提案，但长期受到关注。

3. **[#75541 修复 sweep：分页拉取事件并正确识别 unlabeled 以关闭过期 issue](https://github.com/anthropics/claude-code/pull/75541)**  
   完善过期 issue 自动关闭逻辑，防止误关。

4. **[#72014 新增 protect-mcp 插件：失败关闭型 Cedar 策略门禁 + 签名收据](https://github.com/anthropics/claude-code/pull/72014)**  
   提供插件化的工具调用安全管控，支持策略阻止及离线可验证签名。

5. **[#68673 修复分页中断条件：当页未满时停止，而非仅空页](https://github.com/anthropics/claude-code/pull/68673)**  
   防止遍历超出范围的页面，优化脚本效率。

## 5. 功能需求趋势
- **自治 Agent 与长期运行**：分层大脑、子代理递归、持久状态、后台任务管理（#56913, #61993, #75314）
- **成本可视化与控制**：消费异常诊断、配额预警、更精准的模型计费（#42249, #54776, #67506, #70459）
- **IDE/VS Code 深度集成**：fork 会话、worktree 并行、移动端一致性（#46451 已关闭, #69554, #75525）
- **Windows 稳定性**：Cowork 虚拟化故障、路径大小写、OneDrive 冲突（#38993, #74649, #45178, #75855）
- **会话管理**：删除命令、日期漂移修正、跨设备同步（#26904, #73800, #75525）

## 6. 开发者关注点
- **成本黑洞**：多个用户报告 token 消耗异常快速，怀疑后台死循环或缓存失效，需要透明化计费细节。
- **Cowork 生产就绪度不足**：Windows 虚拟化环境问题频出，文件同步错误导致可信度下降，是当前最迫切的平台质量瓶颈。
- **代理机制缺口**：无法嵌套创建子代理、后台任务不可取消、重复消息浪费 token，限制了复杂自动化场景。
- **非 macOS 平台体验**：Windows 和 Linux 在终端兼容性、虚拟化支持、MCP 连接器等方面仍有明显短板。

---

*日报生成基于 anthropics/claude-code 仓库公开数据，统计截至 2026-07-09 24h 内更新。*

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex 社区动态日报
**日期：2026年7月9日**

---

## 1. 今日速览
今日社区最突出的动态是 **Codex CLI 0.143.0 引入了一项工具命名重复的严重回退**，导致大量用户在执行 shell 命令或调用 `exec_command` 等内置工具时遭遇失败。多位开发者已在几个小时内集中报告此问题，短期内成为社区故障讨论的焦点。与此同时，Rust 通道发布了 `v0.144.0-alpha.2`，OpenAI 内部可观测性和性能优化的 PR 也密集推进，反映出团队对稳定性和可诊断性的持续投入。

---

## 2. 版本发布
- **rust-v0.144.0-alpha.2**  
  发布 0.144.0-alpha.2 版本，详情未在 Release Note 中公开，预计为 Rust 工具链的实验性更新。

- **rust-v0.144.0-alpha.1**  
  同系列上一个 alpha 版本，配合新一轮迭代。

> 注意：今日暂无面向终端用户的 CLI 或桌面端正式版本发布，当前最新 CLI 版本仍为 0.143.0，但该版本引发的工具调用问题正被集中追踪。

---

## 3. 社区热点 Issues（10 个重点关注）

### 🔥 #31697 / #31665 / #31639 — 0.143.0 工具名重复系列 bug
- **现象**：模型发起的工具调用出现 `exec_commandexec_command` 或 `shell_commandshell_command` 等重复命名，导致 CLI 或 app-server 拒绝执行，并报 “unsupported call”。
- **影响范围**：涵盖 Linux、macOS、Windows 多个平台，影响 `gpt-5.5` 等模型，涉及 CLI、桌面端及远程场景。
- **社区反应**：短时间内多个相似 issue 被创建，集中反馈该回退严重阻塞开发流程。
- 链接：[#31697](https://github.com/openai/codex/issues/31697) | [#31665](https://github.com/openai/codex/issues/31665) | [#31639](https://github.com/openai/codex/issues/31639)

### 🧠 #8745 — LSP 集成（自动检测与安装）
- **需求**：要求 Codex CLI 内置 Language Server Protocol 支持，自动检测并安装语言服务器，以提升代码分析与生成质量。
- **热度**：👍 407，评论 55，长期位居功能需求前列。
- **意义**：若能落地，将极大增强 CLI 的 IDE 级智能体验，缩小与编辑器扩展的差距。
- 链接：[#8745](https://github.com/openai/codex/issues/8745)

### 💾 #28224 — SQLite 反馈日志导致 SSD 过度写入（~640 TB/年）
- **问题**：Codex 的反馈日志机制曾以极高频率写入 SQLite，可耗尽消费级 SSD 寿命。
- **状态**：作者在 6 月 23 日更新称三个 PR 已合并，可避免 85% 的日志写入，并尝试关闭此 Issue，但 Issue 仍呈 Open 状态。
- **社区反响**：👍 427，评论 142，用户对数据写入量和硬件寿命极度敏感。
- 链接：[#28224](https://github.com/openai/codex/issues/28224)

### ⏳ #28969 — 禁用 60 秒自动解决问题提问的功能
- **需求**：用户希望拥有一个开关，可关闭 CLI 在提问时 60 秒后自动 “resolve”（视为解决）的行为，认为这会打断工作流。
- **认可**：👍 92，评论 13，多名开发者表达了对于交互控制的强烈意愿。
- 链接：[#28969](https://github.com/openai/codex/issues/28969)

### 🔄 #31520 — CLI 更新命令失败
- **现象**：通过官方安装脚本更新时，报错 “Could not find Codex package ...”，影响版本升级流程。
- **时间**：昨日创建，12 条评论，👍 24，说明更新基础设施存在断裂点。
- 链接：[#31520](https://github.com/openai/codex/issues/31520)

### 🖥️ #31611 — Amazon Linux 2023 上 `exec_command` 不支持
- **发现**：0.143.0 在 Amazon Linux 2023 环境中完全无法执行基本 shell 命令，退回 0.140.0 恢复正常。
- **特点**：平台特定故障，可能与系统库或权限环境有关。
- 链接：[#31611](https://github.com/openai/codex/issues/31611)

### 🪟 #31708 / #31511 — Windows 沙箱与权限错误
- **#31708**：桌面端在提升沙箱设置时，路径解析错误导致 Windows 原生错误对话框。
- **#31511**：受限权限配置下，`apply_patch`、`view_image` 误报“文件名过长”，实际路径远短于限制。
- 链接：[#31708](https://github.com/openai/codex/issues/31708) | [#31511](https://github.com/openai/codex/issues/31511)

### 🧩 #31706 — Chrome 插件丢失浏览器控制柄
- **现象**：Codex 桌面端的 Chrome 插件在成功列出标签页后，偶尔失去浏览器控制权，导致 `claimTab` 等操作失败。
- 链接：[#31706](https://github.com/openai/codex/issues/31706)

### 📉 #31682 / #31606 — 速率限制配额异常
- **问题**：用户反映 Pro/Plus 订阅的配额重置失效（重置次数消耗但未生效），或出现 “no-active-thread” 限制错误。
- 链接：[#31682](https://github.com/openai/codex/issues/31682) | [#31606](https://github.com/openai/codex/issues/31606)

### 📁 #19758 — 基于主题的记忆目录与 agent 自动写入
- **需求**：提出 topic-based 记忆架构，取代单一的 `memory_summary.md`，支持 agent 自主写入与 `/memory` 斜杠命令。
- **状态**：评论 8，功能构想影响 Agent 的长程记忆能力。
- 链接：[#19758](https://github.com/openai/codex/issues/19758)

---

## 4. 重要 PR 进展（10 个）

| PR | 内容 | 重要性 |
|----|------|--------|
| [#31712](https://github.com/openai/codex/pull/31712) | 自动信任来自符合条件的远程插件的钩子 | 降低手动信任负担，提升远程开发体验 |
| [#31690](https://github.com/openai/codex/pull/31690) | 为 exec-server 通知添加追踪 | 填补握手过程的观测空白，提升可诊断性 |
| [#31566](https://github.com/openai/codex/pull/31566) | 重用文件遍历清单以加速技能加载 | 高延迟连接下减少数百次远程请求，优化启动性能 |
| [#31687](https://github.com/openai/codex/pull/31687) | 标准化 exec-server RPC 请求 Span | 对齐 app-server 的可观测字段，统一遥测规范 |
| [#31707](https://github.com/openai/codex/pull/31707) | 跨越异步边界追踪 exec-server 工作 | 确保后台任务、流式 HTTP 等路径的追踪连续性 |
| [#31596](https://github.com/openai/codex/pull/31596) | 默认启用图像生成扩展 | 统一图像生成实现路径，移除旧有分支 |
| [#31254](https://github.com/openai/codex/pull/31254) | 速率限制后推荐其他安装选项 | 优化首次安装体验，减少 GitHub API 限制导致的失败 |
| [#31176](https://github.com/openai/codex/pull/31176) | 模型容量错误时自动重试 goal | 避免因瞬时容量问题中断自动化任务，减少人工干预 |
| [#30131](https://github.com/openai/codex/pull/30131) | 添加线程历史 SQLite 及初始迁移 | 为分页本地线程历史打下存储基础 |
| [#31694](https://github.com/openai/codex/pull/31694) | 允许通过后端依赖 ID 安装插件 | 拓宽插件安装渠道，支持未在推荐列表中但有效的后端插件 |

---

## 5. 功能需求趋势
从近期 Issue 中提炼出的三大功能方向：

- **CLI 工具稳定性与可配置性**：本周 0.143.0 引发的大面积工具调用失败，凸显了用户对 CLI 核心执行路径极高可靠性的要求；同时 “禁用自动 resolve”、“更新失败” 等问题长期存在。
- **IDE 级智能提升**：LSP 集成需求持续火爆（#8745），JetBrains 扩展功能落后的抱怨（#22648）也说明社区希望所有形态的 Codex 都能提供同等深度的代码理解和重构能力。
- **Agent 记忆与自主性**：基于主题的记忆目录（#19758）、长时运行自动化任务恢复（#23895）等需求，反映了用户对 Agent 持久化情境和自主学习记忆的进阶期待。
- **桌面端体验与跨平台一致性**：Windows 沙箱问题（#31708）、macOS 27 beta 无法提交任务（#31364）、自动化历史丢失（#23895）等表明，桌面应用在可靠性与平台适配方面仍有大量优化空间。

---

## 6. 开发者关注点
- **发布质量与回归控制**：0.143.0 的工具命名 bug 造成严重可用性问题，社区希望加强发布前对核心工具链的回归测试。
- **硬件资源友好性**：虽然 #28224 已有修复，但日志写入对 SSD 寿命的影响引发了开发者的广泛警觉，任何过度磁盘 I/O 都会成为关注的焦点。
- **远程与沙箱环境的流畅性**：从 `exec_command` 不支持、`apply_patch` 误报、到远程线程分组错误，远程开发链路中的各种摩擦点正在被逐一暴露并要求修复。
- **可观测性和自我诊断**：内部团队密集提交的 exec-server 追踪 PR 表明，增强分布式调用链的可视化是内部优先事项，这也将间接帮助开发者自行排查复杂问题。
- **配额与计费透明度**：重置配额失效、莫名其妙的线程限制，直指计费体系的透明度与可靠性，是目前 Pro/Plus 用户的一大痛点。

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI 社区动态日报 | 2026-07-09

## 今日速览
社区在稳定版 `v0.50.0` 发布后，快速推送了 `v0.51.0-preview.0` 预览版，修复了代理测试问题。同时，安全补丁集中出现，多个 PR 修复了远程代码执行（RCE）和路径遍历等高危漏洞，表明安全加固成为当前开发重点。Issue 方面，“通用代理挂起”与“子代理误报成功”等高赞 Bug 持续发酵，可靠性与智能调度仍是社区最大痛点。

---

## 版本发布
### ☁️ v0.50.0（正式版）
- **发布修复**：解决了 npm ci 脚本忽略问题，防止发布验证阶段二进制被遮蔽。
- **工具注册**：引入 `Feat/tool registry`，为工具发现与管理带来新基础。

### 🔧 v0.51.0-preview.0
- 包含 `v0.50.0-preview.1` 的变更日志。
- 修复了 `no_proxy` 相关测试，提升环境兼容性。

---

## 社区热点 Issues（精选 10 条）

1. **#21409 通用代理挂起 – 最高关注度**
   - 通用子代理在处理如“创建文件夹”等简单任务时无限挂起，已有 8 个点赞和 7 条持续讨论。手动禁止子代理成为临时缓解手段，对日常使用影响极大。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/issues/21409)

2. **#22323 子代理达 `MAX_TURNS` 后误报成功**
   - `codebase_investigator` 在达到最大轮次、未完成分析时仍返回 `GOAL` 成功状态，严重干扰自动化流水线判断。该问题评论数最高（10 条），揭示子代理退出语义的模糊性。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/issues/22323)

3. **#24353 组件级鲁棒评估体系（EPIC）**
   - 作为行为评估的延续，旨在构建组件级测试框架，覆盖 6 种 Gemini 模型，目前已生成 76 个测试，是质量保证的关键基础设施。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/issues/24353)

4. **#22745 评估 AST 感知的文件读取、搜索与映射**
   - 探讨利用语法树精准限定代码边界是否能减少无效轮次与令牌噪音，潜在提升代码库调查的准确性和效率，受到 7 条讨论重点关注。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/issues/22745)

5. **#21968 模型不主动使用自定义技能与子代理**
   - 尽管用户定义了 `gradle`、`git` 等技能，模型几乎从不自主调用，必须显式指令，削弱了扩展体系的实际价值。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/issues/21968)

6. **#26522 阻止 Auto Memory 对低信号会话无限重试**
   - 记忆提取代理会重复读取低价值会话，造成资源浪费和索引污染，5 条讨论呼吁加入信号判断逻辑以停止无效重试。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/issues/26522)

7. **#25166 Shell 命令完成后卡在“等待输入”**
   - 简单命令如 `ls` 执行毕后界面仍显示“Awaiting user input”并无限挂起，4 条评论外加 3 个点赞表明并非个案，严重影响交互流畅度。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/issues/25166)

8. **#21983 Wayland 下浏览器子代理失败**
   - 在 Wayland 环境中 `browser_agent` 报错终止，影响 Linux 桌面用户体验，急需做平台适配。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/issues/21983)

9. **#24246 工具数超过 128 时返回 400 错误**
   - 当注册工具数量膨胀至 128+ 时，API 直接拒绝请求，暴露出工具管理的上限与智能筛选机制的缺失。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/issues/24246)

10. **#22672 代理应主动阻止破坏性操作**
    - 模型在 Git 操作中可能滥用 `git reset --force` 等危险命令，社区希望加入安全护栏，避免不可逆数据丢失。
    - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/issues/22672)

---

## 重要 PR 进展

1. **#28232 [安全] 拆分评估工作流以修复供应链 RCE**
   - 将 `pull_request_target` 重构为 `pull_request` + `workflow_run`，防止 fork 代码泄漏 `GEMINI_API_KEY` 等高危凭据。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/pull/28232)

2. **#28319 [安全] 强制工作区信任检查，阻止 A2A Server 零点击 RCE**
   - 推迟环境加载到工作区验证之后，堵住非信任目录直接触发远程代码执行的严重漏洞。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/pull/28319)

3. **#28328 [修复] 精确判定 401 认证错误，避免误报**
   - 之前任何包含 `401` 子串的消息（如 `localhost:4012`）都会触发 OAuth 刷新流程，现改为仅匹配标准 401 状态码。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/pull/28328)

4. **#28327 [修复] 仅对 `file://` URL 进行百分号解码**
   - 解决文件名中含有合法百分号（如 `report%202026.txt`）被错误解码为空格的问题。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/pull/28327)

5. **#28164 [保护] 限制单次请求的递归推理轮次**
   - 为核心推理引擎增添 15 轮硬上限，可选配 `maxSessionTurns`，防止无限循环耗尽 CPU 和 API 配额。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/pull/28164)

6. **#28316 [修复] 任务取消后彻底终止执行循环**
   - 修复“幽灵执行”问题，避免取消后后台流继续运行，并顺带解决多线程竞争与内存泄漏。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/pull/28316)

7. **#28223 [修复] 绕过 LLM 校正，防止 JSON/IPYNB 文件损坏**
   - `write_file` 和 `replace` 在处理 Jupyter Notebook 及 JSON 时不再经过模型修正，避免引入破碎结构。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/pull/28223)

8. **#28126 [已合并] 多行编辑摘要追加省略号**
   - 改进 `EditToolInvocation` 展示，对隐藏内容增加 `...`，让用户一眼识别多行修改。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/pull/28126)

9. **#28309 [体验] 改善 CJK 文本换行与粗体语法渲染**
   - 修复中/日/韩等无空格语言的异常断行，并正确处理 `__bold__` 标记，提升终端可读性。
   - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/pull/28309)

10. **#28310 [小修] 移除 Antigravity 身份认证 URL 末尾多余的句点**
    - 避免因链接尾部标点导致登录提示中的地址无效，虽小但影响首次引导体验。
    - [🔗 查看详情](https://github.com/google-gemini/gemini-cli/pull/28310)

---

## 功能需求趋势

- **代理智能调度与可靠性**  
  大量 Issue 聚焦于子代理不主动启用、误报结束状态、挂起等问题，社区明显期待更稳健的决策框架和退出策略。
- **工作区安全强化**  
  从 RCE 漏洞修复到破坏性命令拦截，安全已超越“功能需求”，成为当前最紧迫的工程议题。
- **记忆系统成熟化**  
  Auto Memory 在索引清理、低信号过滤、脱敏和补丁校验方面连续爆发多个 Bug，其稳定性亟待提升。
- **工具系统的边界与智能裁剪**  
  工具数量激增导致 API 400 错误、临时脚本四处散落，需求指向动态工具注册、智能筛选和更规范的文件生成行为。
- **终端渲染与人机交互优化**  
  体现在 CJK 换行、调整大小时闪烁、等待输入假死等一系列 UI/UX 细粒度打磨。

---

## 开发者关注点

- **代理行为的透明性**：子代理轨迹难以追溯、错误报告缺少子代理上下文，阻碍开发者调试，`/chat share` 等特性呼声高。
- **弹性的会话与取消机制**：挂起、幽灵执行、无法真正取消任务等问题反复出现，开发者希望获得如熔断、强制终止等更强的控制力。
- **配置一致性**：浏览器代理忽略 `settings.json`、`no_proxy` 测试修复等现象表明配置分发与合并逻辑需要统一审计。
- **文件系统安全与副作用管理**：模型随意在项目各处生成临时脚本、危险 Git 操作等，破坏工作区整洁，需要文件生成沙箱或安全策略。
- **跨平台兼容性**：Wayland 下的浏览器代理失效、终端调整大小闪烁等提示跨 Linux 桌面环境的适配仍有薄弱环节。

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI 社区动态日报

**日期：2026-07-09**

---

## 今日速览

无新版本发布；社区讨论集中在模型配置缺陷与拦截修复上。两个新 issue 直指 `settings.json` 中 `model` 字段在启动时被忽略（#4067）和缺少模型家族别名（#4068），暴露了模型切换体验的脆弱性。历史 issue #618（自定义斜杠命令）虽已关闭，因获 99 个 👍 和 32 条评论，仍为近期最高热度功能请求。

---

## 版本发布

近 24 小时无新 Release。

---

## 社区热点 Issues（Top 10）

1. **#4067 ‑ `model` 字段在 `settings.json` 中不生效，启动回退到默认模型**  
   🔗 [github/copilot-cli Issue #4067](https://github.com/copilot-cli/issues/4067)  
   ⚠️ **重要性**：用户通过 `/model` 选择或手动在配置文件中指定的模型完全被忽略，会话始终使用 `claude-sonnet-5`。直接影响信任和模型切换工作流，多位用户已确认。  
   📊 社区反应：今日新创建，尚未形成大规模互动，但问题严重性高。

2. **#4068 ‑ 支持通过模型家族名（如 `opus`、`sonnet`）指定最新稳定版**  
   🔗 [github/copilot-cli Issue #4068](https://github.com/copilot-cli/issues/4068)  
   ⚠️ **重要性**：模型迭代快（Opus 已发 4.5~4.8 多个版本），手动 pin 版本导致频繁修改配置。社区期待像其他编码助手一样使用家族别名，自动追随最新版本。  
   📊 社区反应：今日提出，虽无评论但立即获得关注。

3. **#618（已关闭）‑ 支持从 `.github/prompts` 加载自定义斜杠命令**  
   🔗 [github/copilot-cli Issue #618](https://github.com/copilot-cli/issues/618)  
   ⚠️ **重要性**：类比 Claude Code 的自定义命令功能，可大幅提升团队工作流复用性与 CLI 扩展性。评论 32 条、99 👍，社区诉求强烈。已关闭但仍是最热遗产 issue。  
   📊 社区反应：高赞，期待官方给出后续计划。

4. **#970 ‑ macOS Gatekeeper 在每次 Homebrew 升级后阻止 Copilot 启动**  
   🔗 [github/copilot-cli Issue #970](https://github.com/copilot-cli/issues/970)  
   ⚠️ **重要性**：企业安全策略下的安装摩擦持续困扰 macOS 用户，每次更新都需手动到隐私设置中允许，影响开发效率。  
   📊 社区反应：21 👍，6 条讨论，长期未解决，反映安装体验槽点。

5. **#3158 及相关重复 issue（均已关闭）‑ 自动压缩后陷入 217 次“规划→压缩→重规划”无限循环，零代码生成**  
   🔗 代表性 issue：[github/copilot-cli Issue #3158](https://github.com/copilot-cli/issues/3158)  
   ⚠️ **重要性**：上下文自动压缩机制触发后，代理重复规划而非执行，完全耗尽会话 token。该问题被同一作者以轻微变体提交十余条，团队批量关闭，但核心 bug 可能仅被部分修复。  
   📊 社区反应：大量重复报告，虽已关闭，关注度极高，开发者对上下文管理稳定性抱有担忧。

6. **#4054 ‑ `/resume` 对所有非 Git 仓库会话失效**  
   🔗 [github/copilot-cli Issue #4054](https://github.com/copilot-cli/issues/4054)  
   ⚠️ **重要性**：在非 git 目录创建的会话存储 `repository = '/'`，恢复选择器中的 git 门控导致无法选取任何会话，使得 `/resume` 功能在该类场景下完全不可用。  
   📊 社区反应：新 issue，已有讨论和复现步骤。

7. **#1624 ‑ 旧版 CLI 安装包未清理，占用约 2GB 存储**  
   🔗 [github/copilot-cli Issue #1624](https://github.com/copilot-cli/issues/1624)  
   ⚠️ **重要性**：自动升级机制缺乏老旧版本回收，长期使用后严重浪费磁盘空间，对 CI 环境或瘦客户机不友好。  
   📊 社区反应：1 👍，1 评论，但反映了隐性的安装管理问题。

8. **#4039（已关闭）‑ 企业托管插件标记为已安装但文件从未同步到磁盘**  
   🔗 [github/copilot-cli Issue #4039](https://github.com/copilot-cli/issues/4039)  
   ⚠️ **重要性**：企业通过 `managed-settings.json` 分发的插件，显示已启用但实际未下载，插件功能形同虚设，威胁企业部署一致性。  
   📊 社区反应：已关闭，可能已修复，但企业用户需关注。

9. **#4053 ‑ Linux TUI 在 NFS/GPFS 家目录上启动时挂起于“Loading: N skills”**  
   🔗 [github/copilot-cli Issue #4053](https://github.com/copilot-cli/issues/4053)  
   ⚠️ **重要性**：在并行线程中调用 `which gh` 导致的 SIGCHLD 竞争条件，使 CLI 在网络文件系统上彻底卡死，影响 HPC 及共享集群用户。  
   📊 社区反应：技术细节扎实，便于定位。

10. **#4065 ‑ 数据外泄保护过于激进，拦截合法的 spec 内容**  
    🔗 [github/copilot-cli Issue #4065](https://github.com/copilot-cli/issues/4065)  
    ⚠️ **重要性**：包含环境变量注入示例（如 `Authorization: "Bearer ${env.AUTH_TOKEN}"`）的规范文件被误判为敏感信息并阻止，影响真实工作。  
    📊 社区反应：新 issue，代表了安全功能与可用性平衡的典型矛盾。

---

## 重要 PR 进展

过去 24 小时内无合并或更新的 Pull Request。

---

## 功能需求趋势

- **模型管理灵活性**：用户迫切需要别名/家族机制（#4068）和稳定的模型配置加载（#4067）。自动切换计划/执行模型（#2792）也持续受关注，反映对不同任务用不同模型的细粒度控制需求。
- **自定义与可扩展性**：自定义斜杠命令（#618）呼声极高，表明开发者希望 CLI 能深度适配团队规范，与 VS Code 扩展生态对齐。
- **会话与上下文可靠性**：自动压缩引发的规划循环（#3158 系列）和 `/resume` 限制（#4054）说明上下文记忆 / 会话恢复是核心体验的短板，社区期待更健壮的实现。
- **安装与存储治理**：macOS Gatekeeper（#970）与版本残留清理（#1624）构成安装生命周期管理的两大痛点，呼吁更顺滑的升级和清理策略。
- **企业特性健壮性**：托管插件同步失败（#4039）和 BYOK 在 ACP 模式下的认证问题（#4016）表明企业级功能仍需打磨，尤其是在非标准环境中的稳定性。

---

## 开发者关注点

1. **配置一致性**：`settings.json` 中的模型设置被忽略，导致用户失去对默认模型的控制感，引发信任危机。
2. **会话稳定性**：自动压缩后的无限循环、非 git 仓库下 `/resume` 不可用，破坏了长任务体验，是开发者反馈的高频痛点。
3. **安装体验摩擦**：macOS 安全拦截和旧版本占用空间，增加维护负担，尤其是自动化升级场景。
4. **安全策略过激**：外泄保护误伤合法的环境变量注入示例，要求提供更细粒度的白名单或禁用机制。
5. **性能与兼容性**：NFS/GPFS 下的 TUI 挂起问题表明 CLI 在共享文件系统环境的测试不足，需加强边缘环境适配。

---

*数据来源：github.com/github/copilot-cli Issues（截至 2026-07-09 UTC）*

</details>

<details>
<summary><strong>Kimi Code CLI</strong> — <a href="https://github.com/MoonshotAI/kimi-cli">MoonshotAI/kimi-cli</a></summary>

# Kimi Code CLI 社区动态日报

**日期：2026-07-09**

---

## 今日速览
今天社区极其平静，无新版本发布，无 PR 更新。唯一动态来自一条已开放近一个月的老 Issue (#2458)，由企业级用户提出“忽略 SSL 证书校验”的功能请求，该问题因终端安全软件导致登录无法完成，至今仍有评论关注。社区整体处于低活跃窗口期。

---

## 社区热点 Issues
> 过去 24 小时内仅 1 个 Issue 有更新，以下为该 Issue 的详情。

- **#2458 [enhancement] Add option to ignore ssl certificate**  
  作者 `dmorsin` 反馈，在企业受控环境中使用 Kimi CLI 登录时，PC 端杀毒软件通过中间人 (MITM) 方式接管 SSL 连接并替换证书，导致 CLI 因证书不匹配而终止。期望增加类似 `--insecure` 或环境变量选项，允许用户在明确知晓风险的前提下绕过 SSL 验证。目前 Issue 已开放 22 天，有 2 条评论（含提问与补充说明），无人点 👍，尚未被团队认领。这反映出部分 ToB 场景下安全软件与 CLI 工具的兼容需求。  
  **链接：** [MoonshotAI/kimi-cli#2458](https://github.com/MoonshotAI/kimi-cli/issues/2458)

---

## 重要 PR 进展
今日无新增或更新的 Pull Request。

---

## 功能需求趋势
从近期（结合历史上下文）社区反馈看，需求集中在以下方向：

- **企业环境适配**  
  如 #2458 所体现，用户希望 CLI 能在有 TLS 流量审查的办公网络中正常工作，需要更灵活的证书策略。
- **安全与信任控制**  
  允许高级用户自主决定是否跳过某些安全检查，增强工具在企业内网或开发测试环境中的可用性。
- **开发者体验**  
  虽然今日无新 Issues，但过往常见的 IDE 集成、会话管理、模型指定等需求持续构成讨论热点。

---

## 开发者关注点
- **企业网络兼容性**  
  大量开发者在使用公司配发的设备时遭遇代理或 MITM 证书干预，CLI 缺少忽略证书的选项已成为明确的阻塞性问题。
- **静默期下的期待**  
  社区并未因单日零发布而显得焦虑，但 #2458 的长时间无官方响应可能使企业用户转向其他支持 TLS 旁路的同类工具。

---

*数据来源：github.com/MoonshotAI/kimi-cli（基于截至 2026-07-09 的公开事件）*

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode 社区动态日报 — 2026-07-09

## 1. 今日速览
今天社区聚焦两大方向：一是大量历史和新增的 Bug 被集中修复，包括 Bun 崩溃、session 状态卡住、项目重复初始化等稳定性痛点；二是围绕权限安全、提供商生态与多人协作的讨论持续升温，尤其是 `pip` 自动安装引发的安全争议和 Z.AI 等新提供商的文档缺口获得较多关注。无新版本发布。

## 2. 版本发布
过去 24 小时内无新 Release。

## 3. 社区热点 Issues（Top 10）
1. **#22100 – OpenCode 为何以只读配置运行 pip3？**
   用户发现 OpenCode TUI 在只读权限配置下仍会尝试运行 `pip install`，认为这暴露了严重的安全漏洞，必须与操作系统用户同等信任。讨论激烈（11 评论），目前已完成修复。  
   [anomalyco/opencode#22100](https://github.com/anomalyco/opencode/issues/22100)

2. **#20045 – `edit` 权限路径格式不一致导致 agent 规则失效**
   `external_directory` 使用绝对路径正常工作，而 `edit` 权限因内部采用相对路径匹配，导致基于绝对路径的规则静默失败。影响所有自定义 agent 权限配置，10 条评论，2 个 👍。  
   [anomalyco/opencode#20045](https://github.com/anomalyco/opencode/issues/20045)

3. **#14465 – 工作区名称、图标等无法持久化**
   在完全关闭桌面应用后，工作区的自定义名称、颜色和图标都会丢失，只有当前会话中保留。该问题长期存在，8 条评论，社区呼声较高。  
   [anomalyco/opencode#14465](https://github.com/anomalyco/opencode/issues/14465)

4. **#24061 – OpenCode ACP 注册代理在 Zed 中加载卡死**
   通过 ACP 注册表安装的 OpenCode 代理在切换时无限加载，需手动重启才能恢复。影响 Zed 集成用户，7 条评论，1 个 👍。  
   [anomalyco/opencode#24061](https://github.com/anomalyco/opencode/issues/24061)

5. **#25984 – `setCacheKey` 向 Bedrock 代理发送错误字段**
   配置 `setCacheKey: true` 后，OpenCode 发送了 `promptCacheKey`（而非 `cache_control`），导致与 Bifrost/LiteLLM 等 AWS Bedrock 代理不兼容，削弱缓存命中率。5 条评论，影响性能优化。  
   [anomalyco/opencode#25984](https://github.com/anomalyco/opencode/issues/25984)

6. **#36010 – Z.AI 提供商文档严重缺失**
   用户尝试配置 Z.AI + GLM Coding Plan 时发现文档仅有基础 `/connect` 说明，缺少 MCP 设置、视觉路由、成本守护等关键章节，与其他提供商差距明显。今日新建，4 条评论。  
   [anomalyco/opencode#36010](https://github.com/anomalyco/opencode/issues/36010)

7. **#25131 – 桌面端切换服务器后因过时 session ID 崩溃**
   OpenCode Desktop 在不同远程服务器间切换时会恢复不存在的 session ID，引发 “Session not found” 崩溃。3 条评论，3 个 👍，对多服务器用户影响大。  
   [anomalyco/opencode#25131](https://github.com/anomalyco/opencode/issues/25131)

8. **#26498 – DeepSeek 工具调用可能生成非法 JSON 参数**
   部分 DeepSeek 模型在工具调用参数中输出 `null`、空串、字符串包裹的数组等格式，导致后续执行失败。此问题与更广泛的格式化讨论相关，4 条评论。  
   [anomalyco/opencode#26498](https://github.com/anomalyco/opencode/issues/26498)

9. **#17595 – 功能请求：任务子代理的运行时模型覆盖**
   当前子代理只能使用启动时预配的模型，无法由主代理动态切换。社区希望获得类似“子任务模型路由”的能力，3 条评论，3 个 👍。  
   [anomalyco/opencode#17595](https://github.com/anomalyco/opencode/issues/17595)

10. **#25766 – 功能请求：多 LLM 结构化团队辩论**
    用户基于其 fork（Conclave）提出内置多模型辩论机制，支持结构化提示、主持人轮换等。展示了社区对多 agent 协作范式的探索，4 条评论。  
    [anomalyco/opencode#25766](https://github.com/anomalyco/opencode/issues/25766)

## 4. 重要 PR 进展（Top 10）
1. **#35989 – 重构：移除 todo 工具**
   全面移除 V1/V2 中的 todo 工具、相关 schema、事件、权限、UI 渲染及文档，清理未维护功能。影响面广，今日刚开启。  
   [anomalyco/opencode#35989](https://github.com/anomalyco/opencode/pull/35989)

2. **#36014 – 升级 Bun 修复进程退出时 NAPI 崩溃**
   升级 Bun 至 Canary 版本以解决 Windows/macOS/Linux 全平台退出时的 NAPI 崩溃（#28046, #31563）。关键稳定性修复。  
   [anomalyco/opencode#36014](https://github.com/anomalyco/opencode/pull/36014)

3. **#8535 – 会话消息双向游标分页**
   为会话消息增加双向游标分页支持，覆盖服务端、App、TUI 等多端，同时关闭三个相关 Issue。该 PR 长期在开发中，今日更新。  
   [anomalyco/opencode#8535](https://github.com/anomalyco/opencode/pull/8535)

4. **#35311 – 修复：同一仓库的多份克隆被识别为不同项目**
   改变项目标识依据，使基于仓库 UUID 或相对路径识别项目，避免重复初始化和 session 混乱，一次性关闭 15 个相关 Issue。  
   [anomalyco/opencode#35311](https://github.com/anomalyco/opencode/pull/35311)

5. **#35777 – 修复：加载时刷新过期的 @latest npm 插件缓存**
   之前 `Npm.add` 在目录已存在时短路，导致 @latest 插件永远不更新。此 PR 解决了插件版本未及时更新的问题。  
   [anomalyco/opencode#35777](https://github.com/anomalyco/opencode/pull/35777)

6. **#35555 – 修复：桌面设置对话框显示滚动条**
   设置面板可滚动但完全隐藏了滚动条，导致键盘导航和鼠标操作无法感知下方内容。此 PR 恢复了滚动条可见性。  
   [anomalyco/opencode#35555](https://github.com/anomalyco/opencode/pull/35555)

7. **#36008 – 修复：恢复 Explore 代理的 shell 访问**
   内置 Explore 代理因 V2 操作重名而失去 shell 执行能力，此 PR 恢复了对应权限并增加了回归测试。  
   [anomalyco/opencode#36008](https://github.com/anomalyco/opencode/pull/36008)

8. **#36003 – 修复：模型目录刷新阻塞时降级使用缓存**
   远程模型目录获取阻塞启动时，将退回使用缓存/本地数据，防止启动无限等待。  
   [anomalyco/opencode#36003](https://github.com/anomalyco/opencode/pull/36003)

9. **#36002 – 修复：流关闭后 session 状态保持繁忙**
   修补在流式响应完成后 session 状态未及时转为 “空闲” 的竞态问题，改善 UI 反馈。  
   [anomalyco/opencode#36002](https://github.com/anomalyco/opencode/pull/36002)

10. **#35999 – 修复：分离活跃上下文令牌与累计使用量**
    上下文计量器错误地将缓存命中计入总量，导致大项目显示错误的可用上下文。此 PR 修正了统计数据展示。  
    [anomalyco/opencode#35999](https://github.com/anomalyco/opencode/pull/35999)

## 5. 功能需求趋势
- **安全与权限模型强化**：从 `pip` 自动安装到路径权限不一致，社区对代理级的安全控制要求更明确、更细粒度。
- **多模型协作与动态路由**：多 LLM 辩论、子代理运行时模型切换等需求反映出用户在尝试更复杂的 AI 工作流编排。
- **外部生态与 IDE 集成**：与 Zed、VS Code 的快捷键/ACP 集成问题频发，同时 Z.AI、Omniroute 等新提供商的接入需求增长。
- **桌面端体验打磨**：工作区持久化、滚动条可见性、服务器切换崩溃等问题说明用户对桌面应用的可靠性期望在提高。
- **跨仓库与大型项目管理**：多次出现的跨仓库 `/undo` 无效、多克隆识别错误等提示项目隔离和操作边界需更明确。

## 6. 开发者关注点
- **安全本能被触发**：OpenCode 在未明确授权的情况下尝试安装 Python 包，引起了开发者对工具“自治”边界的警惕。
- **配置文件混乱**：agent `.md` 的 frontmatter 被 body 覆盖、`AGENTS.md` 被忽视等现象，让开发者对指令系统的确定性产生担忧。
- **工具链兼容性**：DeepSeek 的 JSON 畸形、Bedrock 代理的缓存字段错误等导致集成成本增高，开发者期望对非 OpenAI 系模型有更强壮的支持。
- **状态丢失与崩溃**：session 无法恢复、退出状态码错误、Web UI “Failed to send prompt” 等问题频繁打断工作流，成为日常效率的主要瓶颈。
- **文档与上手门槛**：新提供商的文档缺失、MCP 配置无 UI 等问题，使高级功能的推广受阻。

---
> 以上内容基于 [OpenCode GitHub 仓库](https://github.com/anomalyco/opencode) 公开数据进行自动分析生成。

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/badlogic/pi-mono">badlogic/pi-mono</a></summary>

## Pi 社区动态日报 — 2026-07-09

### 今日速览
过去 24 小时 Pi 未发布新版本，但社区问题反馈与修复非常活跃，共更新 43 个 Issue 与 8 个 Pull Request。多项关键缺陷（如 DeepSeek V4 推理崩溃、OpenAI 压缩后 token 异常）在社区快速响应下已关闭；同时，“模型/思维级别修改默认仅影响当前会话”的功能请求获得大量关注（6 赞），成为近期最热门的长期方向之一。

---

### 版本发布
无。

---

### 社区热点 Issues（按重要性排序）

1. **#5263 [OPEN] 让会话内模型与思维级别变更为临时更改**
   - 社区希望 `/model` 等命令只影响当前会话，避免无意间修改全局默认值，拟在设置中显式提供“默认模型”入口。👍 6、评论 5，讨论热度高，代表众多用户的配置管理痛点。
   - 链接：https://github.com/earendil-works/pi/issues/5263

2. **#5700 [CLOSED] 支持多个并发 Agent 会话的 TUI 切换**
   - 请求允许同时维护多个后台 Agent 会话并在 TUI 中切换，而不是必须关闭当前会话。评论数最多（9），表明多任务工作流需求强烈，虽已关闭，但讨论仍值关注。
   - 链接：https://github.com/earendil-works/pi/issues/5700

3. **#5886 [OPEN] AgentSession 清理/续接与 assistant 尾部生命周期缺陷（元议题）**
   - 汇总了 Agent 在会话延续、终止后状态混乱的一类 bug，影响编码场景的可靠性。👍 2，由核心贡献者提出，对架构稳定性影响深远。
   - 链接：https://github.com/earendil-works/pi/issues/5886

4. **#6433 [CLOSED] DeepSeek V4 + 思维模式在 v0.80.3 中崩溃**
   - 开启高思维频度后，TUI 静默退出，`reasoning_content` 在工具调用历史回放时丢失。这是 v0.79.x 以来的回退，已快速修复，反映出多模型兼容测试仍需加强。
   - 链接：https://github.com/earendil-works/pi/issues/6433

5. **#6429 [CLOSED] 自动压缩后 OpenAI Responses 发送 `max_output_tokens=1` 导致请求失败**
   - 长时间会话压缩后向 GPT-5.5 发送无效参数，导致连续 400 错误。直接影响生产使用，关闭及时。
   - 链接：https://github.com/earendil-works/pi/issues/6429

6. **#6303 [OPEN] 指数退避重试未应用最大延迟上限**
   - 重试计算无限增长，第 7 次尝试可能等待约 4 分钟。`retry.provider.maxRetryDelayMs` 未生效，可能放大短暂故障的恢复时间。👍 1，建议纳入修复。
   - 链接：https://github.com/earendil-works/pi/issues/6303

7. **#6204 [CLOSED] `mimo-v2-omni` 在 MiMo Token Plan 三个提供商中为“幽灵”模型**
   - 模型清单列出但端点不提供服务，用户选择时收到 400 错误。影响使用新模型的用户，已通过端点修正关闭。
   - 链接：https://github.com/earendil-works/pi/issues/6204

8. **#6414 [CLOSED] streamProxy 丢弃 `thoughtSignature`，导致 Gemini 多轮工具调用 400 错误**
   - 代理转发场景下，Gemini 第二回合函数调用因缺少思考签名而失败，影响依赖代理的部署模式。
   - 链接：https://github.com/earendil-works/pi/issues/6414

9. **#6373 [CLOSED] 剪贴板图片无法发送给 LLM 且不支持远程推理**
   - 粘贴图片仅生成文件路径，某些模型不识别路径，且远程模型完全不可用。影响多模态交互，现已关闭。
   - 链接：https://github.com/earendil-works/pi/issues/6373

10. **#6250 [CLOSED] Bun 发布版下 Linux X11 图像粘贴静默失败**
    - 本机剪贴板绑定在打包后可执行文件中无法解析，影响 v0.80.3 的功能，已通过回退 xclip 与修复打包解决。
    - 链接：https://github.com/earendil-works/pi/issues/6250

---

### 重要 PR 进展（今日共 8 项）

1. **#6440 [CLOSED] 修复自定义编辑器组件在会话启动时未应用键位绑定的问题**
   - 确保从 `keybindings.json` 读取的自定义键位在初始加载扩展编辑器（如 powerline footer）时生效，无需 `/reload`。
   - 链接：https://github.com/earendil-works/pi/pull/6440

2. **#6437 [CLOSED] 更新 GitHub Copilot 扩展上下文窗口至 100 万 token**
   - 同步 GitHub 在 6 月 4 日公布的更大上下文能力，更新内置模型元数据。
   - 链接：https://github.com/earendil-works/pi/pull/6437

3. **#6436 [CLOSED] 隐藏 OpenAI Responses 推理摘要中的 `<!-- -->` 分隔符**
   - 仅从 TUI 渲染中剥离此分隔标识，保留原始签名的推理内容以支持重播，并增加回归测试。
   - 链接：https://github.com/earendil-works/pi/pull/6436

4. **#6427 [OPEN] 添加提示缓存未命中追踪**
   - 每回合比对缓存读取与提示 token，在缓存未命中时发出警告标注，帮助诊断开销与上下文切换影响。
   - 链接：https://github.com/earendil-works/pi/pull/6427

5. **#6430 [CLOSED] 修复 Fork 菜单允许重复选择导致多个 fork 的问题**
   - 在 fork 进程开始前关闭选择器，防止因扩展增加拆卸延迟而产生的重复会话。
   - 链接：https://github.com/earendil-works/pi/pull/6430

6. **#6418 [CLOSED] 修复 Bun 发布版原生剪贴板与 Linux X11 回退**
   - 将 `.node` 原生模块纳入打包，并在原生剪贴板不可用时自动回退到 xclip。
   - 链接：https://github.com/earendil-works/pi/pull/6418

7. **#6417 [CLOSED] 支持 JSONL 会话头中的不透明自定义元数据**
   - 允许在会话创建时携带 `metadata` 字段，并持久化至新 harness 模块的 v3 JSONL 格式中。
   - 链接：https://github.com/earendil-works/pi/pull/6417

8. **#6413 [CLOSED] 本地构建时显示 git 信息**
   - 在 `pi --version` 或相关界面中加入分支与提交信息，方便开发者追踪自建版本。
   - 链接：https://github.com/earendil-works/pi/pull/6413

---

### 功能需求趋势
- **会话生命周期管理**：多并发会话、临时模型设定、自定义会话元数据等请求持续升温，用户希望更灵活地控制工作上下文。
- **多模型与提供商兼容性**：新模型接入（Novita AI）、幽灵模型清理、上下文窗口更新（Copilot、DeepSeek）以及 OAuth 计费提示等，反映出社区对更快支持新 API 的渴望。
- **压缩与上下文逻辑优化**：自动压缩触发时机、大型压缩分块、模型切换预压缩等多项反馈，说明长对话可靠性与性能是重度用户的核心关切。
- **多模态与剪贴板**：图像粘贴、远程模型图像推理等功能需求浮现，依赖更好的桌面集成。
- **可观测性与缓存优化**：提示缓存命中追踪、重试上限修复等，显示专业开发者在降低 token 浪费与故障诊断方面的需求。

---

### 开发者关注点
- **会话稳定性**：Agent 会话在续接、fork、压缩后出现空推理、异常参数、会话状态丢失等问题多次报告，需要更健壮的生命周期保障。
- **重试与容错**：无上限退避、未分类的可重试网络错误（bun 套接字关闭），导致长时间阻塞或意外终止，影响自动化脚本运行。
- **跨构建与环境兼容性**：Bun 打包后剪贴板崩溃、read-only 配置文件引发 API 密钥读取失败等说明，开发者部署在受限环境或自构建时容易踩坑。
- **压缩算法健壮性**：大型会话在自动压缩时自身易失败（摘要请求过大），甚至留下未完成工作，暴露出压缩策略对极端长度的适应性不足。
- **模型切换副作用**：切换到小上下文模型后立即溢出、模型/思维更改影响全局默认等，反映出上下文感知与配置隔离需要进一步增强。

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

## Qwen Code 社区动态日报 | 2026-07-09

### 今日速览
社区今日围绕 **v0.19.8** 版本发布展开热烈讨论，重点集中在多工作区 RFC 与文件上传功能回归的诉求上。同时，多个关键 PR 正在推进，涉及性能优化、工作区隔离及 Web Shell 交互体验提升。

### 版本发布
- **v0.19.8** 发布，主要更新包括：
  - 文档更新：在渠道概览中新增企业微信（WeCom）。
  - 服务端特性：为 `qwen serve` 增加环境隔离与总许可控制，提升多租户场景安全性。

### 社区热点 Issues (10 个)
1. **#6378 - RFC: 单守护进程中支持多工作区**  
   由 doudouOUC 发起，19 条评论。提出在保持现有单工作区兼容的前提下，让一个 `qwen serve` 后台进程管理多个工作区，是社区呼声最高的架构改进。  
   [查看详情](https://github.com/QwenLM/qwen-code/issues/6378)

2. **#6560 - 恢复对话中直接上传 / 拖拽图片文档的功能**  
   用户反馈曾经支持的粘贴 / 拖拽上传图片功能丢失，目前只能通过 `read_file` 工具读取本地文件，严重影响使用体验。收获 17 条评论，团队正在讨论方案。  
   [查看详情](https://github.com/QwenLM/qwen-code/issues/6560)

3. **#6565 - 连接 Qwen Coder 时出现 Internal Error**  
   多名用户报告连接到服务时出现内部错误，涉及认证与网关问题，已标注 `need-information`，可能影响多个地区用户。  
   [查看详情](https://github.com/QwenLM/qwen-code/issues/6565)

4. **#6507 - 延时 IDE 连接成功仍显示过期失败状态**  
   IDE 启动超时后连接成功，但 UI 仍然展示之前的失败信息，造成误导。欢迎贡献者修复。  
   [查看详情](https://github.com/QwenLM/qwen-code/issues/6507)

5. **#6553 - Triage 动作静默丢弃 stderr 导致失败不可见**  
   CI 中的 `qwen-triage.yml` 只捕获 stdout，当代理静默失败时，步骤仍标记为成功，影响自动化工作流可靠性。  
   [查看详情](https://github.com/QwenLM/qwen-code/issues/6553)

6. **#6563 - MCP prompt 未声明 arguments 时用户输入被丢弃**  
   当 MCP 服务在 `prompts/list` 中未声明参数时，斜杠命令附带的参数会静默丢失，且无错误提示，属于易用性缺陷。  
   [查看详情](https://github.com/QwenLM/qwen-code/issues/6563)

7. **#6542 - 为复杂 Agent 任务添加只读 Advisor 反馈循环**  
   提议增加一个只读的“顾问”角色，可在任务停滞或完成前提供结构化建议，提升长任务成功率。  
   [查看详情](https://github.com/QwenLM/qwen-code/issues/6542)

8. **#6505 - 子代理推理循环可能无限重复相同工具调用**  
   尽管主代理有循环检测，但子代理仍会出现重复调用同一工具的情况，可能导致大量资源消耗。已关闭，但相关经验值得关注。  
   [查看详情](https://github.com/QwenLM/qwen-code/issues/6505)

9. **#6512 - 状态栏在子代理后台运行后显示快速模型名称**  
   交互式 CLI 状态栏在子代理用快速模型运行后，会错误显示快速模型而非主会话模型，误导用户。  
   [查看详情](https://github.com/QwenLM/qwen-code/issues/6512)

10. **#6554 - v0.19.8 每夜构建发布失败**  
    自动化发布流程因质量检查（prettier 格式化）失败而中断，社区已提交修复 PR #6555，暴露了 CI 门禁问题。  
    [查看详情](https://github.com/QwenLM/qwen-code/issues/6554)

### 重要 PR 进展 (10 个)
1. **#6506 - 优化大文本粘贴性能并增加进度指示器**  
   拦截大段粘贴控制序列，避免 readline 逐字符触发事件，将处理时间从 ~1.7 秒降至瞬时，显著提升 CLI 交互流畅度。  
   [查看详情](https://github.com/QwenLM/qwen-code/pull/6506)

2. **#6561 - 为 Web Shell 增加工作区 Goals 页面，并修复 daemon 恢复时丢失 /goal 的问题**  
   为 `/goal` 指令提供可视化面板，并解决守护进程模式下会话恢复时目标命令被静默丢失的缺陷。  
   [查看详情](https://github.com/QwenLM/qwen-code/pull/6561)

3. **#6556 - 限制 max_tokens 到上下文窗口，移除固定的输出预留**  
   自动压缩现在依据完整上下文窗口触发，每次请求向模型索取实际剩余窗口所能容纳的最大输出，避免因固定预留导致 token 浪费或请求失败。  
   [查看详情](https://github.com/QwenLM/qwen-code/pull/6556)

4. **#6557 - 跨守护进程重启持久化会话工件**  
   实现 V2 会话工件元数据持久化，使守护进程重启后可恢复之前的工件状态，增强长期任务可靠性。  
   [查看详情](https://github.com/QwenLM/qwen-code/pull/6557)

5. **#6495 - 支持 Webhook 触发的渠道任务**  
   允许外部 webhook 向 `qwen serve` 推送事件，由 Qwen 生成回复并通过渠道主动发送，扩展自动化触发场景。  
   [查看详情](https://github.com/QwenLM/qwen-code/pull/6495)

6. **#6567 - 添加工作区限定核心 REST 路由**  
   引入 `/workspaces/:workspace/...` 的正规 REST 契约，支持 URL 编码的绝对路径（含 Windows 路径），为多工作区管理提供 API 基础。  
   [查看详情](https://github.com/QwenLM/qwen-code/pull/6567)

7. **#6535 - 计划任务增加独立运行模式**  
   通过新增 `create_sub_session` 工具，为 cron 调度提供隔离的会话上下文，避免任务之间互相污染。  
   [查看详情](https://github.com/QwenLM/qwen-code/pull/6535)

8. **#6451 - 重写 Fleet View 以匹配 Claude Code 代理视图 UI**  
   按照 Claude Code 的设计模式重新实现多会话管理视图，提升视觉一致性和操作体验。  
   [查看详情](https://github.com/QwenLM/qwen-code/pull/6451)

9. **#6537 - Web Shell 用户消息中渲染 Composer 引用为芯片样式**  
   将内置的文件、扩展、MCP 引用在前端显示为紧凑的芯片，改善消息气泡的可读性。  
   [查看详情](https://github.com/QwenLM/qwen-code/pull/6537)

10. **#6566 - 检测 Triage 步骤的静默失败**  
    在 `qwen-triage.yml` 增加非空响应检查，当代理返回空结果时致使步骤失败，避免隐藏 API 错误。  
    [查看详情](https://github.com/QwenLM/qwen-code/pull/6566)

### 功能需求趋势
- **多工作区与隔离**：从 RFC #6378 到 PR #6567，社区迫切希望单个后台服务支持多项目独立工作区，以及与工作区关联的 REST API。
- **富媒体交互回归**：#6560 暴露出文件拖拽上传、粘贴截图等便捷功能消失，反映出用户对 CLI 与 Web Shell 中多媒体输入的强需求。
- **智能任务监督**：#6542 Advisor 模式、#6535 计划任务隔离运行，体现对长任务可控性、防跑偏机制的关注。
- **Agent 自身的运维能力**：#6553 Triage 静默失败、#6554 每夜构建质量门禁失败，倒逼项目需提升自动化工作流的可观测性。
- **第三方模型兼容**：#6519 Claude 4.8+ 废弃参数问题虽已解决，但表明需要持续跟进外部模型 API 变更。

### 开发者关注点
- **回归问题**：近期功能（如图片上传）丢失，导致日常使用受阻，团队反馈须加强回归测试。
- **状态同步不一致**：#6507 IDE 过期状态、#6512 错误模型名显示，多处 UI 状态与实际后台不符，信任度下降。
- **静默失败**：从 MCP 参数丢弃（#6563）到 Triage 吞掉错误输出（#6553），开发者期望所有失败路径均有明确反馈。
- **性能与资源**：#6506 大粘贴卡顿、#6505 死循环提示，提示需优化输入处理和子代理回路防护。
- **多模型与配置灵活性**：用户希望自定义 AutoMemory 超时、Channel DM 策略等，要求更深层次的配置能力。

</details>

<details>
<summary><strong>DeepSeek TUI</strong> — <a href="https://github.com/Hmbown/DeepSeek-TUI">Hmbown/DeepSeek-TUI</a></summary>

# DeepSeek TUI 社区动态日报 | 2026-07-09

---

## 1. 今日速览
v0.8.68 里程碑继续高速推进，Hmbown 密集合并了 10+ 个 PR，覆盖 Android Termux 构建、xAI 提供商集成、Fleet AgentProfile 路由重构、模型目录六视图和环境级工具沙箱等关键特性。社区侧反馈了中文场景下流式渲染卡顿问题，已引发讨论与初步排期。

---

## 2. 版本发布
无新 Release 发布（近 24 小时）。

---

## 3. 社区热点 Issues（Top 10）

| # | Issue | 重要性 & 社区反应 |
|---|-------|------------------|
| 1 | [#4092](https://github.com/Hmbown/CodeWhale/issues/4092) `v0.8.68 execution board: lane order, dependencies, and agent protocol (canonical packet)` | **55 条评论** – 整个 v0.8.68 的元车道入口，定义了 agent 协议与依赖关系，是当前所有里程碑工作的总控台。 |
| 2 | [#4270](https://github.com/Hmbown/CodeWhale/issues/4270) 流式文本显示太慢，模型输出完毕突然弹出一大块 | 今日新开，用户指出 DeepSeek V‑flash 等快模型在终端中打字速度明显滞后，严重影响交互体验。已获 2 评论讨论。 |
| 3 | [#4042](https://github.com/Hmbown/CodeWhale/issues/4042) Environment‑level tool sandboxing for sub‑agents (enforce `tool_restrictions`) | 已关闭，12 评论。确认了 `--disallowed-tools` 在不同上下文中的运行时强制机制，安全增强核心需求。 |
| 4 | [#3965](https://github.com/Hmbown/CodeWhale/issues/3965) Per‑sub‑agent provider assignment (explicit routing) + LM Studio support | 已关闭，7 评论。用户希望为每个子代理绑定不同提供商（含本地 LM Studio），催生了 Fleet 路由重构。 |
| 5 | [#4236](https://github.com/Hmbown/CodeWhale/issues/4236) Epic: official Termux / Android arm64 support | 2 评论，新增 Android 原生构建需求，附带 sandbox、secret‑store 行为澄清等子任务。 |
| 6 | [#4102](https://github.com/Hmbown/CodeWhale/issues/4102) Turn Inspector evidence cockpit | 2 评论，旨在将 LSP 修复循环、工具调用证据等整合到 Inspector 视图中，提升调试透明度。 |
| 7 | [#3875](https://github.com/Hmbown/CodeWhale/issues/3875) Guided multi‑step setup wizard for built‑in hosted providers in `/provider` | 2 评论，从多提供商配置需求中分离出的富向导 UI，降低首次配置门槛。 |
| 8 | [#3488](https://github.com/Hmbown/CodeWhale/issues/3488) Unicode, CJK, and terminal‑width rendering QA | 3 评论，长期存在的 CJK/宽字符渲染问题被纳入 v0.8.68 的专项 QA 车道。 |
| 9 | [#4109](https://github.com/Hmbown/CodeWhale/issues/4109) model catalog consolidation and live refresh | 5 评论，推动基于 Models.dev 的目录刷新与 TUI 内集成，取代手工维护的模型列表。 |
| 10 | [#4111](https://github.com/Hmbown/CodeWhale/issues/4111) make AgentProfile canonical for Fleet rosters | 4 评论，将 Fleet 从独立的载荷系统改造为标准的 AgentProfile 排班表，架构上的重要收敛。 |

---

## 4. 重要 PR 进展（Top 10）

| # | PR | 功能 / 修复 |
|---|----|------------|
| 1 | [#4269](https://github.com/Hmbown/CodeWhale/pull/4269) `ci(release): build Termux Android arm64 assets (#4240)` | 为 Release 流程添加 Android arm64 构建目标，生成 Termux 兼容二进制，已合并。 |
| 2 | [#4268](https://github.com/Hmbown/CodeWhale/pull/4268) `fix(provider): verify setup keys before saving (#3875)` | 在保存 API key 前通过 `/models` 端点验证，失败时返回具体错误并阻止无效密钥写入，已合并。 |
| 3 | [#4266](https://github.com/Hmbown/CodeWhale/pull/4266) `feat(provider): add xAI API‑key route (#4257)` | 添加 xAI/Grok 作为一等公民提供商，支持 API key 与所有环境变量集成，已合并。 |
| 4 | [#4265](https://github.com/Hmbown/CodeWhale/pull/4265) `fix(tui): polish setup and activity copy (#4112)` | 统一英文设置提示文案，完成 `Tasks` → `Activity` 的文本清理，已合并。 |
| 5 | [#4264](https://github.com/Hmbown/CodeWhale/pull/4264) `v0.8.68: cache command and regex hot paths` | 对命令组构建、正则搜索引入进程生命周期缓存和 LRU 缓存，提升热路径性能，已合并。 |
| 6 | [#4096](https://github.com/Hmbown/CodeWhale/pull/4096) `docs + feat: sub‑agent tool scoping plan and Phase 1 implementation (#4042)` | 子代理工具作用域文档 + 第一阶段的代码实现，为沙箱化提供了可落地方案，已合并。 |
| 7 | [#4263](https://github.com/Hmbown/CodeWhale/pull/4263) `v0.8.68 batch: Android updater, Termux docs, perf consts, sub‑agent tool sandbox` | 批量合并 Android 更新器、Termux 文档、性能常量提取和子代理沙箱，已合并。 |
| 8 | [#4252](https://github.com/Hmbown/CodeWhale/pull/4252) `feat(tui): six‑view model picker catalog browsing (#4115)` | 将 `/model` 切换为六种视图（已配置、目录、最近、编程、便宜、长上下文），已合并。 |
| 9 | [#4262](https://github.com/Hmbown/CodeWhale/pull/4262) `fix(fleet): route AgentProfile pins through custom providers` | 使 Fleet 配置文件能正确将子代理路由到自定义提供商（如 LM Studio），已合并。 |
| 10 | [#4253](https://github.com/Hmbown/CodeWhale/pull/4253) `fix(onboarding): localize dynamic welcome steps (#4044)` | 为新用户欢迎步骤添加动态本地化，根据实际 gates 渲染下一步引导，已合并。 |

---

## 5. 功能需求趋势

- **Agent 与子代理编排**：子代理工具沙箱化、AgentProfile 规范、Fleet roster 强化，多代理协作基础扎实。
- **多提供商路由**：xAI 集成、自定义提供商支持、每子代理分配手段，用户期望更灵活的模型选择。
- **模型目录智能化**：基于 Models.dev 的在线目录刷新、六视图浏览，摆脱手工静态配置。
- **跨平台支持**：Termux/Android arm64 原生构建、沙箱适配、自更新，拓展移动端使用场景。
- **性能与响应优化**：命令/正则缓存、渲染热路径优化、流式输出延迟修复需求（如 #4270）。
- **UX 打磨与本地化**：多步设置向导、Turn Inspector 整合、CJK 渲染专项、开机引导本地化。

---

## 6. 开发者关注点

1. **流式输出延迟**：中文用户抱怨终端打字跟不上模型速度，尤其在 DeepSeek V‑flash 等快模型上明显，需要优先优化流式渲染管线。  
2. **启动与交互流畅性**：Launch 时间优化（如 #3757）、TUI 帧率与输入热路径的持续打磨。  
3. **跨平台兼容性**：Android 沙箱限制、Termux 环境配置复杂度、updater 兼容等问题亟待文档化和测试。  
4. **提供商集成门槛**：用户期待更简单的密钥验证与设置向导，避免手动编辑配置文件。  
5. **多语言支持**：CJK 字符宽度估算错误、硬编码文案提取到 locales、动态步骤本地化仍需跟进。

> 数据来源：github.com/Hmbown/CodeWhale（DeepSeek TUI 项目）  
> 报告时间：2026-07-09

</details>

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*