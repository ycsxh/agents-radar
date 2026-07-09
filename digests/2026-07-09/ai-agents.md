# OpenClaw 生态日报 2026-07-09

> Issues: 500 | PRs: 500 | 覆盖项目: 13 个 | 生成时间: 2026-07-09 03:55 UTC

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

# OpenClaw 项目动态日报 · 2026-07-09

---

## 1. 今日速览

今日项目活跃度极高，过去 24 小时新增/活跃 Issue 457 条，关闭 43 条；Pull Request 活跃量同样达到 500 条，其中 109 条完成合并或关闭。无新版本发布，但大量修复正在密集着陆，特别是在 **文本截断安全**、**子代理可靠性**、**渠道信息泄露** 等长期难题上出现多个收敛性补丁。整体健康度：**活跃但有大量 P0/P1 问题积压，需持续关注稳定性与安全性债务**。

---

## 2. 版本发布

今日无新版本发布，也无 Release 更新。当前最新版本仍为 `2026.3.13` 或更早的快照。

---

## 3. 项目进展

今天合并/关闭的若干重要 PR 为项目带来了以下可见的稳定性与健壮性提升：

- **网关 & UI 文本截断安全巩固**  
  - [PR #102090](https://github.com/openclaw/openclaw/pull/102090) 修复了会话标题与预览的 UTF-16 截断，避免 emoji 等占 2 个代码单元的字符被截断后产生 U+FFFD 乱码。  
  - [PR #102225](https://github.com/openclaw/openclaw/pull/102225) 为 Control UI 的 `clampText`/`truncateText` 增加了代理对安全保护。  
  - [PR #102066](https://github.com/openclaw/openclaw/pull/102066) 为诊断工具 `doctor` 的错误信息截断加入了相同的 UTF-16 安全处理。

- **信道稳定性修复**  
  - [PR #89041](https://github.com/openclaw/openclaw/pull/89041) 关闭了 Discord 上 `ws` 库 8.21.0 引入的接收器限制，避免了高流量下连接错误关闭。  
  - [PR #101815](https://github.com/openclaw/openclaw/pull/101815) 修复了 Discord 视频附件带编码 URL 时无法正确分离标题的问题。

- **模型响应鲁棒性**  
  - [PR #102334](https://github.com/openclaw/openclaw/pull/102334) 使 OpenAI 的安全拒答信息（refusal）能够作为可见文本呈现给用户，而不是静默吞掉。

这些合并展示了维护团队对**底层健壮性（编码边界、协议容错）** 的高度关注，今天完成了多个小而精准的修复。

---

## 4. 社区热点

今日评论数量最多的 Issue 集中在**消息完整性、会话安全与多代理协调**三大领域：

- **#25592 - 工具调用之间的文本泄漏到消息通道**（35 评论）  
  🔗 [openclaw/openclaw#25592](https://github.com/openclaw/openclaw/issues/25592)  
  模型在工具调用之间生成的内部处理叙述、错误提示会直接泄露到 Slack/iMessage 等客户可见通道，暴露系统内情。社区反馈这属 **严重 UX 和安全问题**，要求框架级隔离。已有关联 PR，但尚未落地。

- **#44925 - 子代理完成结果静默丢失，无重试、无通知**（21 评论）  
  🔗 [openclaw/openclaw#44925](https://github.com/openclaw/openclaw/issues/44925)  
  多个子代理完成交付环节出现静默失败，导致父会话永远等待或丢失任务结果。用户运行 Telegram 论坛机器人时频繁复现，强烈要求增加状态机保护与超时自动重启。

- **#48788 - 集中式文件名编码工具**（18 评论）  
  🔗 [openclaw/openclaw#48788](https://github.com/openclaw/openclaw/issues/48788)  
  多语言文件名（中文、日文、韩文等）在飞书等渠道上出现乱码，需要统一的跨编码处理方案。社区认可架构级解法的价值，讨论涉及 GB18030、Shift-JIS 等编码支持。

- **#48003 - Steer 模式未在半路注入消息**（15 评论，👍3）  
  🔗 [openclaw/openclaw#48003](https://github.com/openclaw/openclaw/issues/48003)  
  用户启用 `messages.queue.mode: "steer"` 后，期望在工具调用边界实时插入指令，但实际消息被排队到回合结束。回归原因定位到 3 月份引入的 `KeyedAsyncQueue`，社区催更修复。

- **#45740 - gh-issues 技能未消毒注入子代理提示词**（14 评论）  
  🔗 [openclaw/openclaw#45740](https://github.com/openclaw/openclaw/issues/45740)  
  GitHub 问题体中包含的任意内容直接嵌入子代理提示，构成潜在提示注入风险。

---

## 5. Bug 与稳定性

今日重点报告的 Bug 按严重度排列，并标注现有修复 PR 情况：

### 🔴 P0 级（系统崩溃/数据丢失/安全）
- **[Bug] Live Docs 超前于发布版** ([#48920](https://github.com/openclaw/openclaw/issues/48920))  
  文档中的 `Heartbeat IsolatedSessions` 功能在 `2026.3.13` 中实际不可用，导致用户根据文档配置后崩溃。⛔ 无修复 PR，标签 `impact:ux-release-blocker`。

- **会话挂起与重复消息** ([#43661](https://github.com/openclaw/openclaw/issues/43661))  
  压缩超时后进入静默失败循环，每 10 分钟向用户重复发送同一消息，无恢复手段。⛔ 无修复 PR。

- **沙盒容器因 no-new-privileges 立即退出** ([#43996](https://github.com/openclaw/openclaw/issues/43996))  
  升级后 maker 沙箱无法执行任何命令，容器退出码 255。⛔ 无修复 PR。

- **cron 隔离任务跳过交付** ([#94846](https://github.com/openclaw/openclaw/issues/94846))  
  恢复的工具错误被误判为致命错误，导致最终助手输出未送达。⛔ 无修复 PR，标签 `impact:message-loss`。

### 🟠 P1 级（会话状态/消息丢失/认证）
- **Telegram 私信路由污染主会话** ([#41165](https://github.com/openclaw/openclaw/issues/41165))  
  即使有先前的修复 (#40519)，Telegram 私信仍会错误路由到 `agent:main:main`。🔗 有关联 PR。

- **Discord 泄露内部工具调用痕迹** ([#44905](https://github.com/openclaw/openclaw/issues/44905))  
  出现 `NO_REPLY`、`to=functions.memory_search` 等内部消息泄漏到频道。🛡️ 标签 `impact:security`，🔗 有关联 PR。

- **心跳路由到错误代理的会话** ([#99912](https://github.com/openclaw/openclaw/issues/99912))  
  `isolatedSession` 设置被忽略，心跳始终在默认代理会话中执行。⛔ 无修复 PR。

- **飞书：读图工具结果丢失媒体附件** ([#41744](https://github.com/openclaw/openclaw/issues/41744))  
  回复中图片在发送前丢失。🔗 有关联 PR，但有 `stale` 标记。

- **孤立的锁文件在网关重启时未清除** ([#49603](https://github.com/openclaw/openclaw/issues/49603))  
  PID 存活检查失效，导致持有锁的当前进程重启后锁文件残留。🔗 有关联 PR。

- **早期中止响应模板变量未填充** ([#45314](https://github.com/openclaw/openclaw/issues/45314))  
  用户中断时 `{model}` 等模板变量仍为占位符。

- **Cron 代理在 API 中断期间静默超时** ([#45494](https://github.com/openclaw/openclaw/issues/45494))  
  应快速失败却耗尽整个超时窗口，造成计划任务延迟。标签 `impact:auth-provider`。

- **Playwright 断言错误导致网关崩溃** ([#45224](https://github.com/openclaw/openclaw/issues/45224))  
  未捕获的 CDP 断言错误直接杀死网关进程。

- **A2A 会话重复消息** ([#39476](https://github.com/openclaw/openclaw/issues/39476))  
  代理 B 使用 `sessions_send` 回复代理 A 时造成消息重复。

- **Anthropic 原生路径重放 thinking 块签名错误** ([#94228](https://github.com/openclaw/openclaw/issues/94228))  
  长会话后永久返回 400 错误。

### 🟡 P2 级（体验/数据完整性）
- **内存管理混乱**（[#43747](https://github.com/openclaw/openclaw/issues/43747)）：不同用户的内存存储路径和方式不一致。
- **OPENCLAW_HOME 嵌套目录问题**（[#45765](https://github.com/openclaw/openclaw/issues/45765)）：环境变量设置 `~/.openclaw` 导致创建 `~/.openclaw/.openclaw`。
- **写工具无追加模式导致文件覆盖**（[#40001](https://github.com/openclaw/openclaw/issues/40001)）：cron 隔离会话覆盖共享文件，造成静默数据丢失。
- **成本仪表盘遗漏归档文件**（[#46252](https://github.com/openclaw/openclaw/issues/46252)）：`/new` 后重命名的归档 `.jsonl` 文件不计入统计，少算每日费用。
- **媒体生成成功但交付失败**（[#86034](https://github.com/openclaw/openclaw/issues/86034)）：用户看到的是生成失败表象，实际是交付环节断开。

当日大量 Bug 仍处于 `no-new-fix-pr` 状态，但部分（约 10 个）已有对应 PR 在途，稳定性改进正在推进但速度仍需提升。

---

## 6. 功能请求与路线图信号

用户提出的高价值功能需求中，以下几个方向出现了 PR 或明确的技术讨论，很可能被纳入后续版本：

- **私有网络访问控制** ([#39604](https://github.com/openclaw/openclaw/issues/39604))  
  为 `web_fetch` 增加 `allowPrivateNetwork` 开关，支持访问内网资源。👍 11 个点赞，🔥 高呼声。🔗 有关联 PR。

- **每代理成本预算强制执行** ([#42475](https://github.com/openclaw/openclaw/issues/42475))  
  在网关层实现每日/每月预算限制，广受自部署用户欢迎。标签 `impact:auth-provider`，已有关联开发讨论。

- **备份功能增加 `.gitignore` 风格排除模式** ([#40786](https://github.com/openclaw/openclaw/issues/40786))  
  避免备份 `node_modules`、凭证文件等。🔗 有关联 PR。

- **网关生命周期钩子（onSubagentComplete 等）** ([#43454](https://github.com/openclaw/openclaw/issues/43454))  
  希望提供事件驱动的自动化扩展点，替代轮询或提示词条件触发。

- **MathJax/LaTeX 支持** ([#42840](https://github.com/openclaw/openclaw/issues/42840))  
  Control UI 显示数学公式仍需纯文本，👍 9。

- **YAML 配置文件支持** ([#45758](https://github.com/openclaw/openclaw/issues/45758))  
  社区提议增加 YAML 格式配置以提升可读性。

- **反应式触发器（Telegram reaction 唤醒代理）** ([#47677](https://github.com/openclaw/openclaw/issues/47677))  
  希望将 Telegram 表情回复变为一级触发信号。

- **Webchat 内联按钮支持** ([#46656](https://github.com/openclaw/openclaw/issues/46656))  
  目前 Telegram 专有的按钮能力，要求扩展到 Control UI。

- **子代理流式输出到父会话** ([#47597](https://github.com/openclaw/openclaw/issues/47597))  
  请求 `streamTo="parent"` 支持 `runtime="subagent"`。

- **浏览器工具多项增强** ([#44431](https://github.com/openclaw/openclaw/issues/44431))  
  实际测试后提出 CSS 选择器支持、CDP 网络节流等 7 项改进。

上述功能中，私有网络、备份排除、预算控制已有活跃 PR，成为近期落地种子。

---

## 7. 用户反馈摘要

从 Issue 讨论中能提炼出用户群体的共同痛点和期望：

- **可靠性焦虑蔓延**  
  多个用户表达“代理突然不回应”“子任务结果丢失”“会话永久挂起”等体验，导致对生产使用的信任受挫。典型如 @IIIyban (#44925)：“子代理完成时静默失败，没有错误日志，也没有重试。”

- **信息泄漏成安全隐患**  
  #25592 和 #44905 暴露了内部推理文本出现在最终用户聊天中，社区认为这“极度危险”，要求立即添加内容过滤器。

- **配置和文档滞后带来挫败感**  
  #48920 “Live Docs 超前于实际版本” 让用户根据不存在的功能配置后出现崩溃，@Stoff81 反馈“文档中的功能不能用，严重误导”。

- **多代理编排满意度低**  
  @waliddafif (#43367) 在 CLI 下尝试并行编码时遇到“配置覆盖、会话锁失败、子进程脱离”等一系列问题，认为多代理“在实践中不可靠”。

- **编码与文化适配诉求强烈**  
  中文、日文、韩文用户在文件名和消息截断上频繁遇到乱码（#48788, #45765），虽然修复 PR 在途，但推进速度较慢。

- **对成本控制的呼声高**  
  @hkochar (#42475) 提出预算管控，@PabloDaVa (#46252) 指出成本面板少算花费，反映出个人及小团队用户对“防止意外账单”的迫切需求。

总体而言，用户社区对 OpenClaw 的潜力仍然充满热情（大量功能请求、👍 数量可观），但对当前的**稳定性纸面与实际表现之间的差距**显露出疲惫感。

---

## 8. 待处理积压

以下重要 Issue 长时间未获得有效响应或修复，存在变成 dead issue 的风险，提请维护者关注：

- **[stale] Feishu 读图工具丢失媒体** ([#41744](https://github.com/openclaw/openclaw/issues/41744)) — P1，有关联 PR 但自 3 月起积压，仍未落地。
- **[stale] 早期中止响应模板未填充** ([#45314](https://github.com/openclaw/openclaw/issues/45314)) — P2，关联 PR 已 open 长达数月。
- **[stale] 成本仪表盘遗漏 `/new` 归档文件** ([#46252](https://github.com/openclaw/openclaw/issues/46252)) — 影响所有频繁重置会话的用户。
- **OPENCLAW_HOME 嵌套问题** ([#45765](https://github.com/openclaw/openclaw/issues/45765)) — 中文用户持续遇到的配置路径错误，有 PR 但需要加快合入。
- **Cron 任务在 API 中断时静默超时** ([#45494](https://github.com/openclaw/openclaw/issues/45494)) — P1，影响定时任务可靠性，无 PR。
- **心跳路由到错误代理会话** ([#99912](https://github.com/openclaw/openclaw/issues/99912)) — 近期报告的 P1 问题，尚无 PR。

此外，多项 **P0/P1** 级 Bug 虽非 stale，但长期没有修复 PR（如 #43661、#43996、#94846），严重拖累项目健康度，建议提升排查优先级。

---

*本日报由 AI 智能体根据 GitHub 公开数据自动生成，仅供项目维护者与社区参考。*

---

## 横向生态对比

好的，以下是基于所提供动态摘要生成的横向对比分析报告。

---

### 个人 AI 助手与自主智能体开源生态横向对比报告

**报告日期：2026-07-09**

#### 1. 生态全景
2026年中，个人 AI 助手开源生态呈现出 **“一超多强、垂直深耕”** 的竞争格局。以 OpenClaw 为核心的少数项目凭借庞大的社区和代码基，正在构筑包含多代理、多通道、插件市场的综合平台，但同时也背负着沉重的稳定性和安全性债务。与此同时，NanoBot、CoPaw 等挑战者正以更快的迭代速度和更集中的功能（如 WebUI 安全、企业级审批）另辟赛道。整体趋势上，**从“单次对话”转向“长期运行、多步骤、可编排的自主智能体”** 已成为行业共识，进而放大了**上下文压缩/管理、子代理通信可靠性、与企业现有系统（IM、日历、监控）原生集成**这三大技术挑战。

#### 2. 各项目活跃度对比
下表汇总了各项目在 2026-07-09 的社区活跃度与健康度快照。

| 项目 | 新增/活跃 Issue | 活跃 PR | 新版本发布 | 健康度评估 |
| :--- | :--- | :--- | :--- | :--- |
| **OpenClaw** | 457 (关闭43) | 500 (合并109) | 无 | **活跃但积压严重**，P0/P1 稳定性与安全债务突出。 |
| **CoPaw** | 34 | 46 | **v2.0.0-beta.4** | **积极迭代中**，beta 版发布后稳定性修复为主旋律。 |
| **ZeroClaw** | 40 | 50 (合并少) | 无 | **极高活跃度，PR 重度积压**，社区参与度广，架构演进中。 |
| **NanoClaw** | 2 | 28 (合并4) | 无 | **高度活跃**，核心团队驱动，推进路线图级大功能。 |
| **NanoBot** | 21 | 23 | 无 | **维护节奏健康**，安全漏洞快速响应，体验持续改进。 |
| **Hermes Agent** | 50 | 50 (合并29) | 无 | **高频互动**，多平台稳定性修复成主轴，桌面端体验大幅改善。 |
| **IronClaw** | 13 | 50 (合并10) | 无 | **高活跃**，架构重构（NEA-25）进入白热化阶段。 |
| **LobsterAI** | ~1 | 15 (合并12) | 无 | **加速态势**，多 Agent 工作区隔离 bug 快速修复。 |
| **PicoClaw** | 2 | 4 | 无 | **中等活跃**，持续吸收社区贡献，通道扩展与修复进行中。 |
| **TinyClaw** | 0 | 1 (安全审计) | 无 | **低维护期**，仅关键安全加固。 |
| **Moltis** | 0 | 1 (Bug 修复) | 无 | **低活跃**，仅重要的稳定性补丁。 |
| **NullClaw** | 0 | 0 | 无 | **无活动**。 |
| **ZeptoClaw** | 0 | 0 | 无 | **无活动**。 |

#### 3. OpenClaw 在生态中的定位
OpenClaw 是该生态的**事实性标杆与供应链上游**。

-   **核心优势**：拥有最庞大的功能集（最多的通道、工具、模型提供商）、最活跃的用户社区和最多的衍生/致敬项目。其社区提出的议题（如 #25592 信息泄漏、#44925 子代理静默失败）往往成为其他项目预先规避的设计案例。
-   **技术路线差异**：与追求架构纯洁性的 ZeroClaw（插件化、WASM）或轻量级的 NanoClaw/NanoBot 不同，OpenClaw 更侧重于**通过庞大的单体代码库实现功能广度**。当前主要挑战是**偿还因高速扩张而累积的复杂性债务**（如文本截断、会话状态机漏洞），其发展重心在于“加固”而非“重构”。
-   **社区规模**：毫无疑问的第一梯队。单日 457 个活跃 Issue 的量级，远超其他任何项目（第二名 CoPaw/Hermes Agent 仅约 50 个），但其 Issue 关闭率较低，表明维护带宽与社区需求间存在巨大鸿沟。

#### 4. 共同关注的技术方向
多个项目不约而同地聚焦于解决相似问题，体现出行业的共识性挑战：

-   **子代理/多代理通信的可靠性**：
    -   **OpenClaw** (#44925)：子代理结果静默丢失，无重试，无通知。
    -   **NanoClaw** (#1702， #2985)：IPC 消息丢失及特定提供商下回复完全丢失。
    -   **ZeroClaw** (#4873)：Agent 调用被错误转发至 LLM，而非正确的子代理。
    -   **核心诉求**：需要框架内置原子性、重试机制及可见的状态监控，而非依赖不可靠的手动编排。

-   **上下文窗口膨胀与压缩**：
    -   **OpenClaw** (#43661, #94228)：压缩逻辑导致会话挂起、重复消息及 API 签名错误。
    -   **CoPaw** (#5171, #5860, #5848)：v2 beta 集中修复上下文压缩导致的信息丢失、对话循环及索引可读性。
    -   **Hermes Agent** (#39691, #48098)：社区呼吁集成外部工具输出压缩，并修复桌面端的“摘要状态”残留。
    -   **核心诉求**：需要一种对模型透明、对用户可预期的智能压缩策略，以支持长期运行的任务。

-   **未授权信息泄漏**：
    -   **OpenClaw** (#25592, #44905)：工具调用的内部推理文本直接泄漏至用户频道，被视为严重 UX 和安全问题。
    -   **NanoBot** (#954)：流式进度信息泄露 `exec()` 等内部工具调用。
    -   **核心诉求**：在自主代理、工具调用、推理链等复杂内部流程和最终用户可见信息之间，必须建立框架级的隔离栅栏。

-   **跨平台/多通道体验一致性**：
    -   **Projects**: OpenClaw, Hermes Agent, CoPaw, PicoClaw, ZeroClaw 均有相关议题。
    -   **核心诉求**：流式输出、富媒体（按钮、图像）、会话管理在不同平台（Telegram, Discord, 飞书, QQ）上的行为统一，以及跨界面（CLI, WebUI, 桌面端）的统一会话恢复能力。

-   **精细化成本与安全控制**：
    -   **核心诉求**：从 OpenClaw、NanoBot、Hermes Agent 到 ZeroClaw，普遍出现对**每代理/每任务预算、私有网络访问控制、操作审批流程（SOP）和工作区文件忽略机制**的需求，标志着用户正从“个人尝鲜”走向“小团队生产应用”。

#### 5. 差异化定位分析

| 项目 | 功能侧重 | 目标用户 | 技术架构关键词 |
| :--- | :--- | :--- | :--- |
| **OpenClaw** | **全能型综合平台**，覆盖最多通道与工具。 | 追求功能全、喜欢折腾的极客和开发者。 | 单体仓库，功能广度第一，积压重。 |
| **CoPaw** | **企业级与桌面体验**，重视审批流、分布式协作。 | 需要团队协作、工作流管理的专业用户。 | v2.0 公测，AgentScope 生态，上下文压缩。 |
| **ZeroClaw** | **安全、插件化、可扩展架构**。 | 对安全性、定制化和架构纯洁性要求高的开发者。 | Rust/WASM 插件，OCI 注册表，SOP 审批，.ignore 机制。 |
| **NanoBot** | **轻量级、高性能、易部署的个人助手**。 | 希望开箱即用、性能优异的个人及小型团队。 | 安全响应快，WebUI 功能持续增强。 |
| **Hermes Agent** | **跨平台桌面与推理模型深度集成**。 | 偏好桌面交互、使用本地模型的重度用户。 | 桌面 Shell 重构，贡献驱动，多平台稳定性。 |
| **NanoClaw** | **开发友好，CLI/DevOps 集成**。 | 将 AI 嵌入开发工作流的工程师。 | `ncl` CLI，定时任务控制平面，Agent 模板。 |
| **Moltis** | **特定生态（Rust/Caldav）日程智能助理**。 | 使用 Rust 技术栈、有日历集成需求的用户。 | 专注稳定与正确性的单点工具。 |
| **PicoClaw / TinyClaw** | **边缘计算与硬件安全**。 | 特定硬件（NanoKVM）及高安全需求用户。 | 边缘部署，发送方白名单。 |

#### 6. 社区热度与成熟度
项目可根据活跃度和成熟度分为三层：

-   **黄金成熟期 / 高活跃度**：**OpenClaw** 和 **CoPaw**。它们功能丰富，社区庞大，但都处在处理“高复杂度带来的意外行为”阶段。迭代重点在于稳定性和体验打磨，而非革命性新功能。
-   **健康成长期 / 高活跃度**：**ZeroClaw**、**NanoClaw**、**Hermes Agent**、**IronClaw**、**NanoBot**。这些项目社区活跃，维护节奏稳健，正快速实现架构愿景（如插件化、CLI 闭环、架构重构）或修补生态系统缺口。
-   **功能扩张期 / 中等活跃度**：**PicoClaw**、**LobsterAI**。它们聚焦于吸收社区贡献，扩展通道或进行特定问题的修复。

#### 7. 值得关注的趋势信号
对 AI 智能体开发者的关键启示：

-   **“未知的未知”是最大风险**：OpenCoaw P0/P1 级 Bug (如 #44925, #94846) 反映出，子代理的**静默失败和意外状态**是最致命的用户体验杀手。开发者必须为每个异步环节构建心跳、超时、重试和可观测的死信队列。
-   **安全边界前移**：从 NanoBot 的未认证 API 令牌漏洞到 ZeroClaw 呼声极高的 `.ignore` 文件，安全焦点正从“对外服务鉴权”转向“**约束 AI 代理本身的行为**”，防止其读取敏感文件或泄漏推理过程。
-   **上下文管理是核心竞争力**：“上下文压缩”已不再是高级功能，而是保证长期任务运行的唯一手段。CoPaw 和 Hermes 在此领域的密集修补表明，**谁能提供最稳健、用户无感知的记忆与压缩方案，谁就能留住重度用户。**
-   **生态民主化与碎片化并行**：OpenClaw 庞大的单体仓库催生了众多衍生/致敬项目，它们各自选择了更轻量、更安全、更专业的方向。开发者面临**“选全集还是选专精”的抉择**，需根据自身对功能广度、代码可控性和稳定性的偏好来决断。一体化方案（OpenClaw, CoPaw）提供便利，而垂直方案（ZeroClaw, NanoClaw）在各专长领域提供更佳体验。

---

## 同赛道项目详细报告

<details>
<summary><strong>NanoBot</strong> — <a href="https://github.com/HKUDS/nanobot">HKUDS/nanobot</a></summary>

# NanoBot 项目动态日报 – 2026-07-09

## 1. 今日速览
今日 NanoBot 社区保持高活跃度，单日产生 21 条 Issue 更新（新开/活跃 8 条，关闭 13 条）及 23 条 PR 更新（12 条待合并，11 条已合并/关闭）。多项安全漏洞被快速闭环，WebUI、执行器、配置管理等方向有多项改进落地，项目维护节奏健康。无新版本发布，但多个修复与功能增强已集成入主分支。

## 2. 版本发布
无新版本发布。

## 3. 项目进展
过去 24 小时内合并/关闭的重要 PR 推进了以下工作：

- **WebUI 安全修复与功能增强**
  - [#4849](https://github.com/HKUDS/nanobot/pull/4849) 为 WebUI 的 bootstrap API 令牌发放增加安全门控，将 WebSocket 令牌与 REST API 令牌分离，阻止未经认证的本地请求获取可操作 API 的令牌，直接修复 [#4825](https://github.com/HKUDS/nanobot/issues/4825) 系列安全漏洞。
  - [#4856](https://github.com/HKUDS/nanobot/pull/4856) 进一步修复并恢复了安全的 localhost bootstrap 流程，确保在未配置密钥时本地仍可安全登录，但远程访问被严格阻断。
  - [#4828](https://github.com/HKUDS/nanobot/pull/4828) 为 WebUI 加入基于统一 diff 视图的文件编辑进度展示，支持大文件折叠与用户偏好设置，提升开发体验。

- **配置与自动化**
  - [#4852](https://github.com/HKUDS/nanobot/pull/4852) 实现 `nanobot onboard --refresh` 非交互式配置刷新，解决自动化运维场景的痛点。
  - [#4850](https://github.com/HKUDS/nanobot/pull/4850) 优化文档搜索入口，新增多通道集成指南，降低新用户上手门槛。

- **工程基础**
  - [#4460](https://github.com/HKUDS/nanobot/pull/4460) 升级 Node 至 v24，保持运行环境现代性。
  - 长期积压的 Telegram 视觉支持 [PR #12](https://github.com/HKUDS/nanobot/pull/12) 也于今日合并，Telegram 频道现已能理解图像内容。

这些合并使项目在安全性、可用性和扩展性上均有实质进步。

## 4. 社区热点
- **[#912 支持任务专用模型配置](https://github.com/HKUDS/nanobot/issues/912)**（3 赞，5 评论）  
  用户希望为对话、工具调用、浏览器操控等不同任务类型配置不同模型，摆脱全局单模型限制。该需求获得社区认同，且已有多个支持不同任务类型的 PR 在途（如 cron 任务模型预设 [#4622](https://github.com/HKUDS/nanobot/pull/4622)），有望在后续版本落地。

- **[#4825 系列：未认证本地调用者可获取 WebUI API 令牌](https://github.com/HKUDS/nanobot/issues/4825)**（3 条评论，闭合）  
  安全研究员发现嵌入式 WebUI 在未配置令牌签发密钥时，任何本地进程都可获取 API 级 Bearer 令牌。该议题引发快速响应，24 小时内即有关联合并修复（PR #4849、#4856），展现了社区对安全的高度重视。

- **[#954 进度流式传输泄露内部工具调用](https://github.com/HKUDS/nanobot/issues/954)**（4 条评论，已关闭）  
  用户反馈升级 v0.1.4 后，`exec()`、`read_file()` 等内部工具调用会意外泄露到用户聊天界面，严重干扰使用体验。虽已关闭，但评论区透露出对流式功能稳定性的担忧，属于典型的回归问题。

- **[#937 执行工具产生过多幻觉](https://github.com/HKUDS/nanobot/issues/937)**（3 条评论，已关闭为 stale）  
  用户因频繁遇到 `exec` 工具幻觉而停止评估框架，强调 `exec` 是核心且关键工具。该问题虽已标记为 stale，但反映出部分用户对工具可靠性的根本性不满，需持续关注。

## 5. Bug 与稳定性
| 严重程度 | 问题描述 | 对应 Issue | 是否有修复 PR |
|---------|---------|------------|---------------|
| **严重** | 嵌入式 WebUI 在未配置密钥时向本地未认证调用者发放 API 令牌（安全漏洞） | [#4825](https://github.com/HKUDS/nanobot/issues/4825), [#4826](https://github.com/HKUDS/nanobot/issues/4826), [#4827](https://github.com/HKUDS/nanobot/issues/4827) | ✅ 已通过 [#4849](https://github.com/HKUDS/nanobot/pull/4849) 和 [#4856](https://github.com/HKUDS/nanobot/pull/4856) 修复 |
| **严重** | OpenAI 兼容聊天补全 API 接受未认证请求 | [#4078](https://github.com/HKUDS/nanobot/issues/4078) | ✅ 已关闭（可能已随修复一起解决） |
| **高** | Slack 依赖缺失 `aiohttp`，导致频道无法启用 | [#4829](https://github.com/HKUDS/nanobot/issues/4829) | ❌ 暂未发现直接 PR，需关注 |
| **中** | 进度流式传输泄露内部工具调用到用户界面 | [#954](https://github.com/HKUDS/nanobot/issues/954) | 已关闭，但修复状态不明确 |
| **中** | minimax-m2.7 模型通过 Ollama Cloud 在第二次请求时失败 | [#2450](https://github.com/HKUDS/nanobot/issues/2450) | 已关闭，可能因外部原因 |
| **低** | 媒体文件（Telegram/Discord）从未清理，磁盘无限增长 | [#896](https://github.com/HKUDS/nanobot/issues/896) | ❌ 开放中，标记 stale，需重视长期运行稳定性 |

## 6. 功能请求与路线图信号
结合开放 Issue 和已有 PR，以下功能最可能进入下一版本：

- **任务专用模型切换**（[#912](https://github.com/HKUDS/nanobot/issues/912)）：已有关联的设计讨论与 PR（如 cron 模型预设 [#4622](https://github.com/HKUDS/nanobot/pull/4622)），实现路径清晰，优先级较高。
- **子代理控制平面**（[#1006](https://github.com/HKUDS/nanobot/issues/1006)）：添加 `list`/`kill` 命令管理子代理，已有关注度，提升操作安全。
- **多租户网关**（[#936](https://github.com/HKUDS/nanobot/issues/936)）：一个网关实例管理多个代理，降低资源开销，已有初步讨论。
- **文件系统访问与技能持久化**（[#940](https://github.com/HKUDS/nanobot/issues/940)）：AI 代理写入隔离沙箱，无法创建真实技能或处理媒体文件，用户呼声高。
- **执行工具 RTK 改写器**（[#4854](https://github.com/HKUDS/nanobot/pull/4854)）：opt-in 的命令改写功能，提升 exec 工具的安全与灵活性。
- **零令牌路由钩子**（[#990](https://github.com/HKUDS/nanobot/issues/990)）：预处理器钩子绕过 LLM 处理特定消息（如日记），可节省 token，潜力大。
- **SimpleX Chat 通道支持**（[#240](https://github.com/HKUDS/nanobot/issues/240)）：去中心化即时通讯集成，满足隐私用户需求。

## 7. 用户反馈摘要
- **工具可靠性痛点**：多位用户对 `exec` 工具的幻觉问题表达沮丧，认为其“必不可少却不可靠”（[#937](https://github.com/HKUDS/nanobot/issues/937)），甚至中止评估。需要持续优化提示工程或约束策略。
- **意外行为的干扰**：升级后工具调用泄露至聊天界面，破坏交互体验（[#954](https://github.com/HKUDS/nanobot/issues/954)），用户反馈此类回归影响信任。
- **运维困境**：自动化部署场景下，无法非交互式刷新配置成为阻碍（[#4851](https://github.com/HKUDS/nanobot/issues/4851)），好在已有 PR 解决。用户对容器化镜像的构建灵活性也有诉求（[#4857](https://github.com/HKUDS/nanobot/pull/4857)）。
- **安全高度敏感**：社区对未认证即发放令牌的问题零容忍，快速报告与修复体现了良好的协作生态。
- **功能期待**：用户希望 NanoBot 能“成为团队一员”，支持 Slack 线程新会话机制（[#1010](https://github.com/HKUDS/nanobot/issues/1010)），以及多任务差异化模型配置，显示出生产环境下的精细化控制需求。

## 8. 待处理积压
以下重要 Issue 或 PR 长期未获响应，建议维护者关注：

- **长期开放的功能请求**
  - [#240](https://github.com/HKUDS/nanobot/issues/240) SimpleX Chat 通道支持（创建于 2026-02-07，2 赞）
  - [#931](https://github.com/HKUDS/nanobot/issues/931) 原生插件沙箱接口（2026-02-21 创建）
  - [#990](https://github.com/HKUDS/nanobot/issues/990) 零令牌路由预处理器钩子（2026-02-22 创建）
- **有待解决的稳定性问题**
  - [#896](https://github.com/HKUDS/nanobot/issues/896) 媒体文件无限增长导致磁盘耗尽（2026-02-20 创建，1 条评论）
  - [#940](https://github.com/HKUDS/nanobot/issues/940) AI 代理无法访问主机文件系统（2026-02-21 创建，2 条评论）
- **活跃但未合并的修复 PR**
  - [#4764](https://github.com/HKUDS/nanobot/pull/4764) MCP 重连取消作用域隔离修复（防止网关崩溃）
  - [#4816](https://github.com/HKUDS/nanobot/pull/4816) 工具执行异常捕获范围过宽修复
  - [#4840](https://github.com/HKUDS/nanobot/pull/4840) 僵尸进程收割修复

以上积压项如不及时处理，可能继续引发用户流失或安全/稳定性风险。

</details>

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

## Hermes Agent 项目日报 — 2026-07-09

### 1. 今日速览
今日项目整体活跃度较高，过去 24 小时产生 **50 条 Issue 更新**（其中 48 条为新增或活跃状态，2 条关闭）以及 **50 条 PR 更新**（21 条仍在开放，29 条已合并或关闭），体现出社区与维护者间持续的高频互动。无新版本发布，但大量稳定性修复和功能增强 PR 被合并，显著改善了桌面端、Gateway 及多平台适配的可靠性。桌面端 `0.17.0` 自动更新引发的启动崩溃（#61268）迅速得到处理并关闭，显示出问题响应速度良好。当前讨论焦点集中在**上下文压缩增强**、**跨界面会话可见性**、**Windows / 中文环境兼容性**以及**桌面用户体验优化**等方向。

---

### 2. 项目进展（今日合并/关闭的重要 PR）
今日共有 29 条 PR 完成合并或关闭，从可见列表中可提取以下关键推进：

- **修复 Windows 平台与 Gateway 稳定性缺陷**  
  - [#47055](https://github.com/NousResearch/hermes-agent/pull/47055) 合入三项修复：增加 `faulthandler`、处理 `NameResolutionError` 及 asyncio 传输层 OSError，直接解决 Windows 下网络波动引发的 Gateway 静默崩溃。  
  - [#60820](https://github.com/NousResearch/hermes-agent/pull/60820) 为 MCP 目录中的 `subprocess.run` 调用添加超时，防止 `npm install` 或 `git clone` 等操作因网络问题卡死整个进程。

- **Agent 与工具调用韧性提升**  
  - [#59043](https://github.com/NousResearch/hermes-agent/pull/59043) / [#57780](https://github.com/NousResearch/hermes-agent/pull/57780) 在上下文引用异步展开中增加 `return_exceptions=True`，避免单个引用失败导致所有并发展开崩溃。  
  - [#47038](https://github.com/NousResearch/hermes-agent/pull/47038) 修复视觉分析在使用 OpenRouter 提供商时因凭证丢失导致 401 认证失败的问题。

- **功能增强与开发者体验**  
  - [#47108](https://github.com/NousResearch/hermes-agent/pull/47108) 新增 `curator.locked_categories` 配置，允许按技能类别批量锁定，避免手动逐个 pin 技能的繁琐操作。  
  - [#47104](https://github.com/NousResearch/hermes-agent/pull/47104) 补全 `opencode-go` 提供商的模型目录（新增 minimax-m3、deepseek-v4-flash 等），提升模型选择器的完整性。  
  - [#45491](https://github.com/NousResearch/hermes-agent/pull/45491) 将仪表板 cron 端点中的配置文件扫描工作移出 FastAPI 事件循环，改善多 Profile 环境下的仪表板响应速度。  
  - [#61277](https://github.com/NousResearch/hermes-agent/pull/61277) 紧急修复桌面版 `npm run dev` 因 #57855 引入的 tsx 引导方式与 Electron 40 不兼容导致的开发环境崩溃。  
  - [#47043](https://github.com/NousResearch/hermes-agent/pull/47043) 引入对话式追问路由，用户回复机器人消息时会被正确排入 `queued_events`，而非覆盖当前待处理消息。  
  - [#47045](https://github.com/NousResearch/hermes-agent/pull/47045) 实现基于 `platform_registry` 的动态 PII 安全平台检测，方便插件作者声明平台安全性，无需修改会话核心代码。

这些合并/关闭共同推动了项目在多平台稳定性、工具链韧性和开发者友好度上的显著进步。

---

### 3. 社区热点（讨论最活跃的 Issues）
今日评论数突出、反应强烈的议题如下：

- **[#39691](https://github.com/NousResearch/hermes-agent/issues/39691) feat(compression): integrate headroom-ai for tool output compression**  
  评论区 9 条，👍 12。用户希望加入针对工具输出的高效压缩能力（如 headroom-ai），解决当前仅有的对话级压缩无法处理工具调用输出导致上下文膨胀的问题。呼声较高，反映出对长会话和多步工具调用场景下 Token 管理的迫切需求。

- **[#59224](https://github.com/NousResearch/hermes-agent/issues/59224) Classic CLI /resume listing hides Desktop sessions**  
  评论 8 条。用户 `louquillio` 指出经典 CLI 的 `/resume` 列表只显示 `source="cli"` 的会话，桌面端和 WebUI 创建的会话被隐藏，破坏了会话跨界面恢复的一致性。社区对统一会话管理入口的期待明显。

- **[#58646](https://github.com/NousResearch/hermes-agent/issues/58646) QQ bot adapter startup failure**  
  评论 7 条。QQ 机器人适配器因 `connect()` 方法意外收到 `is_reconnect` 参数而无法启动，影响国内 QQ 平台用户。该问题触及消息传递核心通道，需要尽快修复。

- **[#39534](https://github.com/NousResearch/hermes-agent/issues/39534) Desktop Chinese prompt cut off**  
  评论 7 条。Windows 桌面版 `v0.15.1` 出现中文输入在聊天窗口被截断的怪异现象，影响中文用户体验，且问题延续至今未彻底解决。

- **[#48098](https://github.com/NousResearch/hermes-agent/issues/48098) Desktop stale “Summarizing thread” status**  
  评论 6 条。桌面端在上下文压缩恢复后仍长时间显示“Summarizing thread”状态，造成模型仍在工作的误导，影响交互反馈体验。

- **[#5254](https://github.com/NousResearch/hermes-agent/issues/5254) Tool calls repeating when using LM-Studio**  
  评论 4 条。使用 LM-Studio 加载 Gemma4 等模型时，工具调用被碎片化为大量独立请求，与之前 OpenAI Codex 的已知行为类似，表明本地模型兼容性仍需打磨。

- **[#18241](https://github.com/NousResearch/hermes-agent/issues/18241) TUI — 按时间顺序显示思考块与工具调用**  
  评论 2 条，👍 4。使用推理模型时，思考块与工具调用实际是交叉进行的，但 TUI 按类型分组显示，丢失了时序信息。请求反映出高级用户对可调试性的深层需求。

---

### 4. Bug 与稳定性（按严重程度排列，标注修复状态）
| 严重程度 | Issue / PR | 描述 | 状态 |
|:---|:---|:---|:---|
| **P2 崩溃/启动失败** | [#61268](https://github.com/NousResearch/hermes-agent/issues/61268) 桌面更新后 `Composer is not available` 导致进程反复重启 | 已关闭，通过 [#61277](https://github.com/NousResearch/hermes-agent/pull/61277) 修复开发模式引导问题，正式版可能已热修 |
| **P2 启动阻塞** | [#61270](https://github.com/NousResearch/hermes-agent/issues/61270) Gateway 启动时若 Nous Portal OAuth 凭证耗尽，工具网关持续重试且不降级 | 开放，无修复 PR |
| **P2 委托路由失效** | [#61195](https://github.com/NousResearch/hermes-agent/issues/61195) `delegation.base_url` 在子代理实际 API 调用时仍走 OpenRouter 导致 401 | 有 PR [#61275](https://github.com/NousResearch/hermes-agent/pull/61275) 正在修复 |
| **P2 会话状态残留** | [#61220](https://github.com/NousResearch/hermes-agent/issues/61220) 会话过期清理未设置 `end_reason='session_reset'`，导致恢复时重打开过期会话并携带完整历史 | 开放，无修复 PR |
| **P2 消息通道故障** | [#58646](https://github.com/NousResearch/hermes-agent/issues/58646) QQ 机器人适配器连接报错 `unexpected keyword argument 'is_reconnect'` | 开放，无修复 PR |
| **P2 模型连接超时** | [#60715](https://github.com/NousResearch/hermes-agent/issues/60715) Nous 推理 API 完全不可达（curl 超时） | 开放，需基础设施侧排查 |
| **P2 桌面状态显示错误** | [#48098](https://github.com/NousResearch/hermes-agent/issues/48098) 桌面端卡在“Summarizing thread” | 开放，无修复 PR |
| **P2 本地推理适配** | [#5254](https://github.com/NousResearch/hermes-agent/issues/5254) LM-Studio 工具调用重复 | 开放，相关韧性修复已合入 [#59043](https://github.com/NousResearch/hermes-agent/pull/59043) 等，但根因可能未完全解决 |
| **P2 Session 管理** | [#45360](https://github.com/NousResearch/hermes-agent/issues/45360) Cron WebUI 运行显示仅含提示的会话，且平台报错 | 开放，无修复 PR |
| **P2 平台兼容性（WeCom）** | [#61211](https://github.com/NousResearch/hermes-agent/issues/61211) / [#61212](https://github.com/NousResearch/hermes-agent/issues/61212) 企业微信中文文件名因编码后超出 Windows `MAX_PATH` 导致上传失败 | 开放，重复报告 |
| **P3 显示/交互缺陷** | [#39534](https://github.com/NousResearch/hermes-agent/issues/39534) 中文输入截断；[#59224](https://github.com/NousResearch/hermes-agent/issues/59224) CLI 恢复列表隐藏桌面会话；[#35419](https://github.com/NousResearch/hermes-agent/issues/35419) 回退激活无提示；[#61207](https://github.com/NousResearch/hermes-agent/issues/61207) `/plan` 不写文件 | 均开放 |

当日合并的多个韧性修复 PR（超时、异常捕获、Windows 崩溃防护）已显著提升整体稳定性，但 API 可达性、委托路由以及多平台适配的痛点仍需优先关注。

---

### 5. 功能请求与路线图信号
下列新功能请求开放且讨论活跃，部分已有对应的 PR 或设计草案，可能被纳入后续版本：

- **工具输出压缩** [#39691](https://github.com/NousResearch/hermes-agent/issues/39691)：社区对集成 headroom-ai 的呼声很高，虽暂无关联 PR，但已列为 `type/feature` 且 P3，极可能成为下一阶段上下文优化的重点。
- **桌面 Shell 重构（贡献驱动架构）** [#60638](https://github.com/NousResearch/hermes-agent/pull/60638)（PR，开放中）：将桌面壳替换为基于布局树的贡献驱动平台，并建立插件 SDK。该 PR 体量庞大，一旦合入将彻底改变桌面扩展性，预计会联动解决 [#61193](https://github.com/NousResearch/hermes-agent/issues/61193)（无法查看完整终端命令）、[#61249](https://github.com/NousResearch/hermes-agent/issues/61249)（审批面板截断）等诸多体验问题。
- **看板插件** [#61173](https://github.com/NousResearch/hermes-agent/pull/61173)：作为桌面重构的首个 SDK 消费示范，若 #60638 合入，此看板功能将快速落地。
- **阿里云百炼提供商** [#58438](https://github.com/NousResearch/hermes-agent/pull/58438)：新增 `alibaba-cn` 提供商，服务中国用户，已完成开发待审核，路线图信号明确。
- **每 Cron 任务独立推理强度** [#23524](https://github.com/NousResearch/hermes-agent/issues/23524)：轻量级定时任务与高难度任务得以区分资源消耗，属于精细化调度需求。
- **桌面最小化到托盘** [#61246](https://github.com/NousResearch/hermes-agent/issues/61246)：大量 Windows 用户的典型诉求，提供了详细的实现建议。
- **TUI 时序显示** [#18241](https://github.com/NousResearch/hermes-agent/issues/18241)：与推理模型工具调用流程强相关，获得较多认可，但当前无 PR。
- **未读标记与系统通知** [#50718](https://github.com/NousResearch/hermes-agent/issues/50718)：会话完成、等待输入的状态提示匮乏，属于桌面体验基础增强。
- **自定义提供商 UI 配置** [#52807](https://github.com/NousResearch/hermes-agent/issues/52807)：降低用户配置门槛，避免手动编辑 YAML。
- **桌面宠物控制优化** [#61255](https://github.com/NousResearch/hermes-agent/issues/61255)：因已有底层支持，改进用户控制可见性有望较快实现。
- **OIDC 退出登录** [#61243](https://github.com/NousResearch/hermes-agent/issues/61243)：自助托管认证体系的标准化需求。

结合今日开放的众多 `type/feature` PR（如内存管理器异步隔离 [#60953](https://github.com/NousResearch/hermes-agent/pull/60953)、命令中心网桥 [#60994](https://github.com/NousResearch/hermes-agent/pull/60994)），项目正明显向 **插件化桌面、企业级调度、多模型多云适配** 方向演进。

---

### 6. 用户反馈摘要
真实用户场景与情绪从评论中提取如下：

- **跨界面一致性焦虑**：用户 `louquillio` 在 [#59224](https://github.com/NousResearch/hermes-agent/issues/59224) 中表示，习惯在 CLI 恢复所有会话，却发现桌面和 WebUI 创建的会话“消失”，破坏了“在任何地方都能继续工作”的认知。类似痛点也体现在 [#61191](https://github.com/NousResearch/hermes-agent/issues/61191)（本地缓存跨会话污染）。
- **中文与 Windows 用户挫折感**：`LiJT` 遇到中文输入突然截断（[#39534](https://github.com/NousResearch/hermes-agent/issues/39534)），`eason2026` 遭遇 Gateway OAuth 启动阻塞（[#61270](https://github.com/NousResearch/hermes-agent/issues/61270)），企业微信用户反馈文件上传因文件名编码失败（[#61211](https://github.com/NousResearch/hermes-agent/issues/61211)）。这些群体普遍表达了对非英文/非 POSIX 环境支持不足的不满。
- **桌面功能细节缺失**：用户 `klopyrev` 无法在桌面端看到完整终端命令（[#61193](https://github.com/NousResearch/hermes-agent/issues/61193)），`rudidev08` 抱怨审批栏仅显示单行，无法审查多行差异（[#61249](https://github.com/NousResearch/hermes-agent/issues/61249)），均体现了高级用户对透明度的强烈需求。
- **本地模型适配困难**：`micuentadecasa` 使用 LM-Studio 遭工具调用碎片化（[#5254](https://github.com/NousResearch/hermes-agent/issues/5254)），说明社区中本地/小模型用户群体不小，但 Hermes 在非大型 API 模式的工具呼叫稳定性仍有空白。
- **对快速修复的认可**：桌面 `0.17.0` 崩溃循环（[#61268](https://github.com/NousResearch/hermes-agent/issues/61268)）从报告到关闭用时极短，用户 `BLO523` 获得及时响应；多个并发修复 PR 的合入也使社区对维护速度表示肯定。

---

### 7. 待处理积压（提醒维护者关注）
以下 Issue 长期开放且涉及核心体验或稳定性，评论与赞同数表明影响较广，但尚未有明确修复计划：

- **[#5254](https://github.com/NousResearch/hermes-agent/issues/5254) — 工具调用重复（LM-Studio）**：自 4 月报告以来持续存在，与本地模型生态息息相关。
- **[#35419](https://github.com/NousResearch/hermes-agent/issues/35419) — 回退激活无用户提示**：5 月底提出，P2，影响消息通道用户对模型切换的感知。
- **[#39838](https://github.com/NousResearch/hermes-agent/issues/39838) — `notification_sources` 配置未被网关读取**：文档与代码行为不一致，6 月创建，属于跨 Profile 通知的基础缺陷。
- **[#18241](https://github.com/NousResearch/hermes-agent/issues/18241) — TUI 按时间顺序显示思考块**：5 月提出，赞成 4，对推理模型调试价值高，但尚未排期。
- **[#23524](https://github.com/NousResearch/hermes-agent/issues/23524) — 每 Cron 任务独立推理强度**：5 月提出，精细化调度需求，目前无 PR。
- **[#48098](https://github.com/NousResearch/hermes-agent/issues/48098) — 桌面端 stale 压缩状态**：6 月中报告，P2，影响桌面体验可靠性。
- **[#61220](https://github.com/NousResearch/hermes-agent/issues/61220) — 会话过期后重打开带历史**：今日新增但性质严重（状态泄漏），需尽快关注。

这些积压项若长期搁置，可能削弱多平台与高级用户群体的信任。建议结合当前桌面架构重构和韧性提升计划，批量排期处理。

</details>

<details>
<summary><strong>PicoClaw</strong> — <a href="https://github.com/sipeed/picoclaw">sipeed/picoclaw</a></summary>

# PicoClaw 项目动态日报（2026-07-09）

## 今日速览
今日项目整体活跃度中等。过去 24 小时内，社区提交了 2 个新/活跃 Issue，并推动了 4 个 PR 的处理（其中 3 个已关闭，1 个待合并），无新版本发布。重点变动集中在通道扩展、网关可靠性与视觉模型修复；一个关于路径越界提示的 PR 仍在等待评审。项目在多个集成方向上持续吸收社区贡献，但部分问题（如 NanoKVM 默认配置 Bug）尚未得到解决。

## 项目进展
过去一天有 3 个重要 PR 被合并/关闭，显著丰富了功能并修补了缺陷：

- **网关启动可靠性增强**（[#2278]）：当 loopback 绑定失败时，自动回退到通配符绑定并叠加 CIDR 允许列表，解决了特定环境下的启动障碍。
- **新增 Grafana Alertmanager 通道**（[#2251]）：实现了一个仅输入的 webhook 端点，可接收 Alertmanager 告警，并支持触发技能响应，扩展了可观测性集成场景。
- **修复视觉模型无法读取图片**（[#3234]）：`anthropic_messages` 提供方原先在构建请求体时丢弃了 `msg.Media`，导致视觉模型无法看到图片；本次修复将图片媒体嵌入用户消息，使得图像功能恢复正常。

上述合并/关闭使得项目在多通道支持与模型兼容性上又向前迈出一步。

## 社区热点
今日讨论量相对有限，但以下两则问题反映了用户的关键诉求：

- **#3195 [BUG] NanoKVM 上默认配置无法使用 OpenAI GPT**（2 条评论）  
  https://github.com/sipeed/picoclaw/issues/3195  
  用户报告在 NanoKVM 2.4.0 上配置 `gpt-5.4` 时，所有交互均返回错误。该问题自 6 月 30 日提出后持续更新，社区关注度虽不高，但直接影响新特性可用性，需要维护者尽快核实。

- **#3201 [Feature] 请求 QQ 频道支持流式输出**（1 条评论，已标记 `stale`）  
  https://github.com/sipeed/picoclaw/issues/3201  
  提议让 QQ 频道像 Telegram 一样实现 `StreamingCapable`，让用户看到逐 token 生成响应。此诉求反映了多通道体验一致性的期望，但已被标记为 `stale` 警告，若无人跟进可能被关闭。

## Bug 与稳定性
今日明确报告的缺陷仅有 1 个，影响中等：

- **OpenAI GPT 在 NanoKVM 上不可用** [#3195]  
  严重程度：中等。使用默认配置调用 GPT 模型时返回未明确错误，阻断了 NanoKVM 新用户的典型用例。截至目前尚无针对该 Bug 的修复 PR 提出，建议优先排查文档或配置兼容性问题。

## 功能请求与路线图信号
- **QQ 频道流式输出** [#3201] 是唯一被提出的功能需求。尽管已呈 `stale` 状态，但若社区表达持续兴趣，可能驱动将流式能力扩展到更多通道。目前并无直接相关的 PR 在途，需开发者或贡献者拾起此议题。

## 用户反馈摘要
- **NanoKVM 用户**（来自 [#3195]） 在使用新引入的 PicoClaw 特性时立刻受阻，反映出集成测试可能不足，用户对“开箱即用”体验有明显不满。建议维护团队在宣传新特性时同步提供经过验证的配置样例。
- **QQ 频道用户**（来自 [#3201]） 期待与主流通道同等的交互流畅度，侧面说明 PicoClaw 已在部分通道上提供了良好体验，但跨通道体验割裂可能制约部分用户群体的迁移意愿。

## 待处理积压
以下事项需引起维护者注意：

- **#3195 [BUG] NanoKVM 默认配置不工作** —— 开放近 10 天，有评论但无修复方案，影响新硬件合作生态。
- **#3226 [OPEN PR] 修复 `write_file` 工具的破坏性覆盖提示** —— 待合并已 4 天，解决了 agent 错误引导模型覆盖文件的问题（关联 #3150），对工具安全性提升有益，建议尽快评审。
- **#3201 [Feature] QQ 频道流式输出** —— 已 `stale`，若不希望关闭，需核心团队表明意向或由社区承接开发。

---
*数据来源：github.com/sipeed/picoclaw 过去 24 小时 Issue/PR 动态。*

</details>

<details>
<summary><strong>NanoClaw</strong> — <a href="https://github.com/qwibitai/nanoclaw">qwibitai/nanoclaw</a></summary>

# NanoClaw 项目动态日报

**日期：2026-07-09**  
**数据区间：过去 24 小时（截至 2026-07-08）**

---

## 今日速览
今日 NanoClaw 无新版本发布，仓库活跃度集中体现在 **PR 流水线上**：过去 24 小时共有 **28 个 PR 发生更新**，其中 **4 个已合并/关闭**，推动多项基础设施改进和稳定性修复。同时开放了 **2 个新 Issue**，分别报告了一个静默消息丢失的严重 Bug 和一个提升 Discord 体验的功能请求。整体来看，核心团队在持续推进计划中的大功能（如调度任务控制平面、agent 模板向导），社区也贡献了多个修复，项目保持较高的迭代速度，健康度良好。

---

## 项目进展
今日合并/关闭的重要 Pull Request 推进了以下工作：

- **自动化 CI 改进**  
  [PR #2978](https://github.com/nanocoai/nanoclaw/pull/2978)（ci: auto-label PRs from core team members）已关闭，为核心团队成员提交的 PR 自动添加 `core-team` 标签，简化了项目管理工作流。

- **CLI 体验升级**  
  [PR #2980](https://github.com/nanocoai/nanoclaw/pull/2980)（ncl CLI: verb-level args, deep help, server-rendered human view）合并，为 `ncl` 命令行工具带来了严格的参数校验、深层帮助信息以及服务端渲染的可读视图，是“定时任务”大功能系列的第一块积木。

- **IPC 稳定性修复**  
  [PR #1702](https://github.com/nanocoai/nanoclaw/pull/1702)（fix: break for-await loop on result to prevent IPC message loss）这个存在近三个月的长期修复终于关闭，解决了进程间通信中潜在的消息丢失问题，提升了 agent 运行时的可靠性。

- **其他内部改进**  
  第四个已关闭的 PR 未在公开列表详细展示，但根据合并记录判断，应是另一项工具或小特性改进。团队正按节奏消化积压并铺设后续功能基础。

---

## 社区热点
尽管今日 Issues 和 PRs 均**无公开评论**（评论数均为 0），以下事项因其功能影响或设计意图成为焦点：

- **⚠️ opencode provider 静默无回复 Bug**  
  [Issue #2985](https://github.com/nanocoai/nanoclaw/issues/2985) · 由 `fjnoyp` 提交。  
  描述：某些长 agent 回合下，opencode provider 完成回答却**没有任何消息被投递**，也无错误抛出，仿佛 bot 忽略了消息。答案卡在 `message.part.delta` 中未被读取。该问题在重度使用场景下可复现，对依赖 opencode 的生产环境冲击较大。

- **🗣️ Discord 线程自动重命名请求**  
  [Issue #2984](https://github.com/nanocoai/nanoclaw/issues/2984) · 由 `eagansilverpathmarketing` 提交。  
  目前 Discord 适配器为每个会话创建线程并使用默认日期时间名称，大量线程堆叠时极难辨认。提议让 agent 获得一个安全的 `rename_thread` 工具，将线程名改为会话主题摘要，改善 Discord 服务器的可扫描性。该需求如果实现，将显著提升多线程管理体验。

- **路线图级大功能：`ncl tasks` 控制平面**  
  [PR #2981](https://github.com/nanocoai/nanoclaw/pull/2981)（core-team）公开了定时任务的完整资源管理方案，包括创建、更新、运行、日志追加、隔离会话和运行历史等，是“定时任务列车”的第二部分，预示着 NanoClaw 即将拥有原生的任务调度能力。

---

## Bug 与稳定性
按严重程度排序的已报告问题：

| 严重程度 | 问题 | 状态 |
|---------|------|------|
| 🔴 高 | **opencode provider 消息静默丢失** – [Issue #2985](https://github.com/nanocoai/nanoclaw/issues/2985)  | 打开，暂未见直接修复 PR；可能与代理运行允许列表修正（[PR #2982](https://github.com/nanocoai/nanoclaw/pull/2982)）或 Codex 事件投递修复（[PR #2770](https://github.com/nanocoai/nanoclaw/pull/2770)）有关，需进一步关联 |
| 🟡 中 | **skill 组合时片段未按组过滤** – [PR #2921](https://github.com/nanocoai/nanoclaw/pull/2921) 已提供修复 | 待合并，修复了 `composeGroupClaudeMd` 把所有技能指令注入每个组的问题 |
| 🟡 中 | **Codex 图像事件未声明类型且投递失败** – [PR #2770](https://github.com/nanocoai/nanoclaw/pull/2770) 提供修复 | 开放中，预计合并后将解决 `file` 类型事件导致的构建错误和图像丢失 |
| 🟢 低 | **Discord 裸 URL 被错误包裹为掩码链接** – [PR #2979](https://github.com/nanocoai/nanoclaw/pull/2979) 修复了依赖版本问题 | 版本升级修复，风险较低 |

此外，已合并的 [PR #1702](https://github.com/nanocoai/nanoclaw/pull/1702) 根除了一个 IPC 消息丢失隐患，对整体稳定性是重要加固。

---

## 功能请求与路线图信号
从今日动态中可以识别出以下明确的路线图走向：

- **定时任务系统（Scheduled Tasks）**  
  [PR #2981](https://github.com/nanocoai/nanoclaw/pull/2981) 是核心团队规划的“定时任务列车”第二部分，后续还有更多 PR 待推进。该功能将成为 NanoClaw 的重要扩展，支撑自动化工作流。

- **Agent 模板与初始化向导**  
  [PR #2909](https://github.com/nanocoai/nanoclaw/pull/2909)（feat: template setup flow in the wizard and first-agent stamping）将大幅优化新用户的首次配置体验，提供“全新 agent”或“从模板创建”的选择，降低上手门槛。

- **按组能力开关**  
  [PR #2983](https://github.com/nanocoai/nanoclaw/pull/2983) 将 `agent-teams` 和 `workflow` 等高级能力默认关闭，并允许按组开启，为逐步释放复杂特性铺平道路。

- **PR Factory 自动化 Recipe**  
  [PR #2742](https://github.com/nanocoai/nanoclaw/pull/2742) 是一个社区贡献的完整方案，可实现 PR 自动审查、测试计划生成和 Slack 通知，若被采纳，将丰富生态并展示平台扩展能力。

- **Discord 线程重命名（来自 Issue）**  
  [Issue #2984](https://github.com/nanocoai/nanoclaw/issues/2984) 是一个用户声音强烈的小而美特性，可能与核心团队的产品方向契合，有望在未来进入路线图。

---

## 用户反馈摘要
- **opencode 用户不满**：在 [Issue #2985](https://github.com/nanocoai/nanoclaw/issues/2985) 中，用户明确表示该 Bug 导致“bot 看起来完全忽略了消息”，且 **无任何错误提示**，使得排查困难，**严重影响了交互信任度**。这是一个直接的一线使用痛点。
- **Discord 社区可用性诉求**：[Issue #2984](https://github.com/nanocoai/nanoclaw/issues/2984) 提出，日期命名的默认线程让活跃服务器“不可能扫视”，强烈希望 agent 有能力**自动生成有意义的线程名称**。这反映了将 NanoClaw 用于团队协作时，对信息组织效率的迫切需求。

由于没有公开评论，无法获取更多情感色彩。但 Issue 摘要清晰传达了“静默丢失”对信心的打击，以及“默认名称”造成的管理负担。

---

## 待处理积压
以下开放 PR 已存在较长时间，值得维护者关注：

- **[PR #2742](https://github.com/nanocoai/nanoclaw/pull/2742)** – “PR Factory” recipe，提供完整的 PR 审查自动化方案，自 6 月 11 日创建，已标注 `core-team`，但尚未合并。此功能价值显著，长期搁置可能影响贡献者积极性。
- **[PR #2770](https://github.com/nanocoai/nanoclaw/pull/2770)** – Codex image file 事件修复，6 月 14 日创建，解决构建问题和图像丢弃，亟需合入以避免与 `codex.ts` 上线后冲突。
- **[PR #2878](https://github.com/nanocoai/nanoclaw/pull/2878)** – 修复 Codex 在 OneCLI 已有过期凭据时无法重连，6 月 28 日提交，对使用 Codex 的用户是体验卡点。

此外，虽然今日新建的两个 Issue 尚未入积压，但 [Issue #2985](https://github.com/nanocoai/nanoclaw/issues/2985) 的严重性可能使其优先级快速上升，建议早日评估并关联修复。

---

> 数据来源：NanoClaw 仓库 ([nanocoai/nanoclaw](https://github.com/nanocoai/nanoclaw)) 公开事件。  
> 日报自动生成，如有误差请以实际仓库状态为准。

</details>

<details>
<summary><strong>NullClaw</strong> — <a href="https://github.com/nullclaw/nullclaw">nullclaw/nullclaw</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw 项目动态日报
**日期：2026-07-09**

## 1. 今日速览
过去 24 小时，项目活跃度极高：新增/活跃 Issue 13 个，关闭 5 个；Pull Request 方面，待合并 40 个，已合并/关闭 10 个。无新版本发布。总体来看，开发管道密集，大量功能特性与缺陷修复正在并行推进，标志着 Reborn 架构演进进入白热化阶段，但稳定性方面也暴露出一系列 P2 级用户流程阻断问题，需持续关注。

## 2. 版本发布
今日无新版本发布。

## 3. 项目进展
今日有 10 个 PR 被合并或关闭，结合关闭的 Issue，可观察到以下关键推进：
- **遗留 UI 缺陷清扫**：`#5768` (Projects 页 i18n 不完全)、`#5419` (无法重命名自动化)、`#3535` (时间戳错误)、`#5770` (工具权限选择器 UI) 被先后关闭，表明 WebUI v2 的国际化、自动化管理与基础 UI 正确性得到集中修复。
- **CI 稳定性恢复**：`#4108` (Nightly E2E 失败) 已关闭，相关的端到端测试回归得到处置。
- **架构大重构密集落地**：多个 XL/ L 级 PR（如 `#5839` 完成 manifest v2 切换、`#5842` 移除并行通道注册表、`#5854` 抽取胶水层 crate）为 NEA-25 大重构奠定基础；`#5857` 试图降低 API 容量延迟、`#5821` 实现助手文本流式传输等性能/体验 PR 也在推进。项目正向更统一、更高性能的 Reborn 架构快速迁移。

## 4. 社区热点
由于 Issue 整体评论量较低，本日热点主要集中在一组高关注度的缺陷与重构 PR 上：
- **通知消失 bug (`#5553`)**：批准通知闪现后消失，获 3 条评论，是今日讨论最多的 Issue。PR `#5681` 尝试修复，让活动线程批准通知保持可见，成为最受期待的修复之一。
- **Slack 集成缺陷 (`#5834`)**：用户无法通过 agent 断开 Slack，对应清理 PR `#5851` 旨在统一 Slack 清理逻辑，引起对扩展生命周期管理的关注。
- **技术债务清理系列 (`#5826`、`#5827`、`#5828`)**：连续三个 Issue 要求移除 v1 遗留测试、固件与引用，反映出社区对减少维护负担、厘清覆盖边界的强烈诉求。
- **大规模重构栈 (`NEA-25` 系列 PR)**：`#5839`、`#5842`、`#5848` 等 PR 代表零遗留策略，机器强制检查旧代码清除，体现了核心团队对架构纯洁性的追求。

## 5. Bug 与稳定性
本日报告的缺陷集中在自动化运行、UI 交互与扩展管理方面：
- **[P2] 通知/审批流异常**：批准通知不可靠消失 (`#5553`)，已有对应 PR `#5681`；例行运行操作按钮不可点击 (`#5837`)，日志与详情查看中断。
- **[P2] 运行期失败**：上下文压缩错误导致运行失败 (`#5838`)、例行运行系统性报 "No thread attached" (`#5836`)，后者导致 0% 成功率，影响定时任务。
- **[P2] 集成断开被拒绝**：Slack 断开请求被 agent 错误拒绝 (`#5834`)，`#5851` 可能修复。
- **[P3] 信息展示瑕疵**：“跳至最新”按钮在不必要时出现且位置过高 (`#5835`)。
- **[Bug/Enhancement] 附件限制**：WebChat 附件超过 10 个时静默丢弃，无错误提示 (`#5820`)。

## 6. 功能请求与路线图信号
- **管理员令牌管理完善**：`#5856` 指出缺少 API 令牌重新颁发功能，用户详情页残留无效按钮。这是管理员工作流的关键补齐项。
- **WebChat 附件能力提升**：`#5820` 要求提高文件数量上限并暴露溢出错误，已有实际工作流触碰限制，具备高优先级用户需求基础。
- **聊天终端快捷偏好**：PR `#5824` 新增“显示聊天终端快捷方式”偏好设置，说明终端视图的逐步产品化正在路上。
- **私人工具与技能安装**：PR `#5525` 和 `#5780` 推进非管理员用户的私有安装，将大幅扩展工具生态的可达性，有望进入下一版本。
- **性能与可观测性**：PR `#5857` 针对 API 容量预模型延迟，`#5858` 完成 Trace Commons 注册工具，显示团队对生产级可运维性的投入。
- **遗留清理路线**：`#5826` 等三个 Issue 以及 NEA-25 栈 PR `#5848` 的机器强制检查，标志着 v1 代码的正式退役路线已定，成为近期的技术债务清理重点。

## 7. 用户反馈摘要
来自 Issue 描述的真实痛点：
- **“批准通知闪烁后消失，无法继续自动化”** (`#5553`)：用户遇到需人工批准的网络能力调用时，通知忽现忽失，导致自动化交互断裂。
- **“自动化名字被截断，无法编辑”** (`#5419`)：自动生成名称过长且不可修改，影响管理效率。
- **“失败运行的日志按钮点不了”** (`#5837`)：使用者在诊断定时任务失败时无法直接打开运行详情，排查困难。
- **“我的定时 Slack 总结每次都失败，没线程”** (`#5836`)：ironclaw-issues-slack-summary 等内置例行程序 0% 成功率，用户完全无法依赖自动报告。
- **“断开 Slack 集成的请求被 agent 无视”** (`#5834`)：agent 给出无关回复，集成无法安全解除，影响连接生命周期信任。
- **“拖 10 个文件进去，有的静默丢了”** (`#5820`)：高级用户在工作流中多次遇到硬性上限，且丢失无提示，构成实际数据丢失感受。

## 8. 待处理积压
- **每日失败分类 Issue (`#5859`)**：当日新开的例行分类任务，标记了 44 个非通过的 pinchbench 失败，指出上游限流几乎让所有 LLM 调用返回错误，但 Issue 本身尚无开发活动，需关注是否触发基础设施或配额调整。
- **遗留清理 Issue 群 (`#5826`、`#5827`、`#5828`)** 均暂无对应 PR 链接，虽然近期大概率会被 NEA-25 工作覆盖，但仍建议明确指派，避免持续积压。
- **无回应的高严重度 Bug**：`#5836` (例行任务 "No thread attached")、`#5837` (操作按钮不可点击) 暂无明确关联修复 PR，若长时间搁置将直接损害自动化可靠性口碑。

</details>

<details>
<summary><strong>LobsterAI</strong> — <a href="https://github.com/netease-youdao/LobsterAI">netease-youdao/LobsterAI</a></summary>

# LobsterAI 项目动态日报
**日期：2026-07-09**

---

## 1. 今日速览
项目在过去 24 小时呈现明显加速态势：共处理 15 条 PR，其中 12 条被合并或关闭，3 条保持开放，同时关闭 1 个旧 Issue。社区涌现出关于多 Agent 配置同步的严重 Bug 反馈，但相应修复 PR 已在当天合并，展示了快速响应能力。整体来看，项目在稳定性和功能增强两端均有显著推进，健康度良好。

## 2. 版本发布
*今日无新版本发布。*

## 3. 项目进展
今日合并/关闭的重要 PR 推进了多个关键方向：

- **多 Agent 工作空间隔离**  
  [#2295](https://github.com/netease-youdao/LobsterAI/pull/2295) 修复 `USER.md` 按 Agent 工作区作用域引导，解决了编辑任一 Agent 的“关于你”页面会覆盖所有 Agent 用户配置的问题，直接回应用户 Bug 报告。

- **协作体验强化**  
  [#2300](https://github.com/netease-youdao/LobsterAI/pull/2300) 支持在协作引导队列中携带附件、拖拽文件、粘贴图片等载荷；  
  [#2296](https://github.com/netease-youdao/LobsterAI/pull/2296) 为协作权限提示添加最小化/恢复功能，提升长对话交互效率。

- **记忆与搜索优化**  
  [#2297](https://github.com/netease-youdao/LobsterAI/pull/2297) 默认应用本地 FTS 关键词搜索，即使在向量搜索关闭时也能保障记忆检索；  
  [#2301](https://github.com/netease-youdao/LobsterAI/pull/2301) 显式关闭 OpenClaw 的 memory dreaming，确保配置的一致性。

- **子代理与 IM 会话精确映射**  
  [#2299](https://github.com/netease-youdao/LobsterAI/pull/2299) 同步子代理子工具历史，避免孤儿工具调用记录；  
  [#2298](https://github.com/netease-youdao/LobsterAI/pull/2298) 按 Agent 作用域划分 IM 频道会话，防止不同代理的消息串扰。

- **积压清理**  
  关闭了一批长期积压的 PR，包括定时任务通知渠道修复 ([#1406](https://github.com/netease-youdao/LobsterAI/pull/1406))、时间选择器 UI 优化 ([#1404](https://github.com/netease-youdao/LobsterAI/pull/1404))、请求 ID 安全性修复 ([#1401](https://github.com/netease-youdao/LobsterAI/pull/1401)) 等，进一步提升了产品稳定性与体验细节。

## 4. 社区热点
- **多 Agent USER.md 覆盖 Bug**  
  [#2293](https://github.com/netease-youdao/LobsterAI/issues/2293) 刚开启即被维护者重视，用户反馈重启后所有 Agent 的 `USER.md` 均被 main agent 配置覆盖，导致无法为不同 Agent 设定独立需求。该问题是近期更新的回归，已获得针对性修复 PR #2295，并今日合并。

- **历史热点：4.1 版本网关崩溃**  
  [#1400](https://github.com/netease-youdao/LobsterAI/issues/1400) 虽今日被标记关闭，但评论数达 7 条，用户描述了升级后反复重启无法启动的严重故障，并留下联系方式求助，可见版本质量曾引起较强烈不满，该问题关闭值得留意是否确已解决。

另外，一个目录集成 PR [#2294](https://github.com/netease-youdao/LobsterAI/pull/2294) 添加了 TakoAPI 目录徽章，正在等待处理，反映社区对项目可见度的主动贡献。

## 5. Bug 与稳定性
按严重程度排列今日发现的 Bug 与修复状态：

- **严重：多 Agent USER.md 全局覆盖**  
  [#2293](https://github.com/netease-youdao/LobsterAI/issues/2293) - 修改任意 Agent 的“关于你”或 `USER.md` 会使所有 Agent 同步更改，重启后数据丢失。  
  ✅ **已有 Fix 且今日合并** [#2295](https://github.com/netease-youdao/LobsterAI/pull/2295)。

- **严重（已关闭）：4.1 版本网关无限重启**  
  [#1400](https://github.com/netease-youdao/LobsterAI/issues/1400) - 升级后网关启动失败进入无限循环，附带自定义 LLM 调用失败问题。今日关闭但未指明修复细节，需观察后续发布。

- **轻微：定时任务名称无重复校验**  
  [#1348](https://github.com/netease-youdao/LobsterAI/issues/1348) - 可创建同名定时任务，影响管理清晰度。尚未指派修复 PR，仍在积压。

## 6. 功能请求与路线图信号
- **定时任务增强 (Cron 自定义调度、Agent 选择器)**  
  开放 PR [#1347](https://github.com/netease-youdao/LobsterAI/pull/1347) 已包含可视化 Cron 构建器与表单优化，虽已停滞三个月，但从今日清理多项定时任务相关修复来看，该模块的活跃度仍在，可能被纳入后续版本规划。

- **技能管理界面**  
  [#1346](https://github.com/netease-youdao/LobsterAI/pull/1346) 持续等待评审，属于社区呼声较高的功能扩展，建议维护者给予关注。

- **文档生态对接**  
  [#2294](https://github.com/netease-youdao/LobsterAI/pull/2294) 添加的 TakoAPI 徽章可以提升项目的发现性，纯文档型 PR，合并成本低。

## 7. 用户反馈摘要
- **严重挫败感：升级即瘫痪**  
  从 [#1400](https://github.com/netease-youdao/LobsterAI/issues/1400) 的用语“彻底瘫痪了！”“反复重启，无限循环”可以看出，用户在生产环境中遭遇严重阻断，对版本质量产生不信任感，希望官方直接联系支援。

- **多 Agent 用户的刚需受挫**  
  [#2293](https://github.com/netease-youdao/LobsterAI/issues/2293) 用户指出：“只要改了一个 agent 设置里的‘关于你’页面内容…其他 agent 里也同步进行了修改”，这一行为完全破坏了多 Agent 场景的独立性，属于基本工作流中断。用户自行测试验证了问题的复现条件，反馈翔实。

- **正面行动反馈**  
  该 Bug 当天即修复闭环，体现出社区与维护者的高效联动，但需注意类似回归问题应通过自动化测试避免。

## 8. 待处理积压
以下重要 Issue/PR 长期未响应（超过 3 个月），建议维护者复审：

- **PR [#1346](https://github.com/netease-youdao/LobsterAI/pull/1346) 技能管理功能**  
  于 2026-04-02 提交，至今无评审或更新，技能管理是提升 Agent 能力的核心方向。

- **PR [#1347](https://github.com/netease-youdao/LobsterAI/pull/1347) 定时任务 Cron 调度等增强**  
  同样于 2026-04-02 提交，包含详细的 UX 优化与自定义调度，价值较高。

- **Issue [#1348](https://github.com/netease-youdao/LobsterAI/issues/1348) 定时任务名称重复校验**  
  该问题一直开放且未分配修复，虽严重程度不高，但容易引发用户体验困扰，可考虑作为 good first issue 处理。

- **PR [#2294](https://github.com/netease-youdao/LobsterAI/pull/2294) 目录徽章文档更新**  
  今日提交，低风险，建议快速合并以获得更多社区曝光。

</details>

<details>
<summary><strong>TinyClaw</strong> — <a href="https://github.com/TinyAGI/tinyagi">TinyAGI/tinyagi</a></summary>

# TinyClaw 项目动态日报
**日期：2026-07-09**  
**仓库：[TinyAGI/tinyagi](https://github.com/TinyAGI/tinyagi)**

---

## 1. 今日速览
过去24小时项目整体活跃度较低，无新 Issue 提出或产生讨论，亦无新版发布。唯一动态是一条安全加固相关的合并请求（PR #44）于昨日关闭，完成了历时近5个月的安全审计修复工作，为项目带来实质性的安全增强。社区整体平静，维护者可利用此时机关注积压事项。

---

## 2. 版本发布
本日无新版本发布。

---

## 3. 项目进展
- **安全加固与完整性修复（PR #44）✅ 已关闭**
  - [PR #44](https://github.com/TinyAGI/tinyagi/pull/44) 由 `coreyone` 提交，聚焦安全与代码审计，于2026-07-08关闭。
  - **主要变更**：
    - 在 Telegram、Discord、WhatsApp 及队列处理中强制启用发送方白名单（默认开启），未授权发送方将被拒绝。
    - 限制出站文件处理路径，防止路径遍历。
    - 加固 Bundle 更新与安装的完整性校验机制。
  - **价值**：该 PR 显著提升了 TinyClaw 在聊天入口、代理调用与文件处理层面的安全性，补上多通道滥用与供应链风险缺口，标志着项目安全基线的阶段性闭环。

---

## 4. 社区热点
本日无活跃讨论或高反响 Issue / PR。社区处于静默状态。

---

## 5. Bug 与稳定性
近24小时无新建 Bug 或稳定性报告。已关闭的 PR #44 本身是安全修复，非 Bug 反馈渠道。

---

## 6. 功能请求与路线图信号
无新增功能请求。对应 PR #44 中引入的发送方白名单等安全策略，虽为安全加固，但也暗含了“细粒度访问控制”这一架构能力的落地，可能为未来多租户或权限管理功能提供基础。当前无明确路线图变更信号。

---

## 7. 用户反馈摘要
本时段无用户直接反馈 Issue 或评论。从已关闭的 PR #44 内容看，团队自身正在主动弥补安全短板，推测此前可能存在安全审查驱动或外部审计推动，但无公开讨论痕迹。

---

## 8. 待处理积压
由于本日报仅覆盖近24小时数据，且未提供长期未关闭 Issue / PR 清单，暂无法评估积压状况。建议维护者后续关注以下几点：
- 核对各通道配置文件中白名单默认启用对现有用户是否产生**破坏性影响**（PR #44 未提及迁移引导）。
- 检查跨平台消息、文件发送的兼容性，确保路径限制未误伤正常功能。

---

**报告说明**：今日数据范围仅包含项目在所给时段内的公开活动。TinyClaw 当前处于低变动维护期，安全加固是近期主线。  
**数据来源**：GitHub 仓库 [TinyAGI/tinyagi](https://github.com/TinyAGI/tinyagi)

</details>

<details>
<summary><strong>Moltis</strong> — <a href="https://github.com/moltis-org/moltis">moltis-org/moltis</a></summary>

# Moltis 项目动态日报 — 2026-07-09

## 1. 今日速览
项目仓库今日整体处于低活跃状态，过去 24 小时无新 Issue 开启或关闭，无新版本发布。仅有一条来自社区的 Pull Request (#1145) 保持开放，聚焦于修复 CalDAV 日期解析中的潜在崩溃问题。整体健康度正常，但需关注该修复的合并进度以避免用户在生产环境中触发 Panic。

## 2. 版本发布
无新版本发布。

## 3. 项目进展
- **#1145** [OPEN] fix(caldav): avoid panic on non-ASCII datetime in normalise_datetime  
  作者 Osamaali313 针对 `crates/caldav/src/ical.rs` 中的 `normalise_datetime` 函数提出了一个防御性修复。该 PR 旨在消除当远程 CalDAV 服务器返回非 ASCII 日期时间值时，由于固定字节索引切片未充分校验而可能导致的 Panic。当前补丁尚未合并，一旦合入将显著提升日历同步的鲁棒性，是项目稳定性的重要一步。  
  → [moltis-org/moltis#1145](https://github.com/moltis-org/moltis/pull/1145)

## 4. 社区热点
今日无讨论热烈的 Issue 或 PR。唯一更新的 #1145 尚无评论与互动，社区焦点暂未形成。

## 5. Bug 与稳定性
- **严重程度：中** — `normalise_datetime` 在遇到非 ASCII 字符时发生 Panic  
  位置：`crates/caldav/src/ical.rs`  
  影响范围：任何从不符合 RFC 5545 日期格式的 CalDAV 服务器同步事件时均可能触发崩溃，导致守护进程异常退出。  
  修复进展：已有对应 Pull Request #1145 提交，等待项目维护者审阅合并。  
  → [相关 PR #1145](https://github.com/moltis-org/moltis/pull/1145)

## 6. 功能请求与路线图信号
今日无新增功能请求，亦无可纳入下一版本的明确信号。

## 7. 用户反馈摘要
虽然 Issue 区无直接反馈，但 PR #1145 的描述透露出真实使用痛点：有用户或开发者在实际对接远程 CalDAV 服务器时，遇到了因非预期日期格式导致的程序 Panic。该场景下崩溃将直接中断日历同步服务，用户对稳定性的需求极为迫切，是该修复提交的核心驱动力。

## 8. 待处理积压
今日无可识别的长期未响应 Issue 或 PR。当前唯一开放的 #1145 刚刚创建，尚未进入积压风险阶段，建议维护者及时审查。

</details>

<details>
<summary><strong>CoPaw</strong> — <a href="https://github.com/agentscope-ai/CoPaw">agentscope-ai/CoPaw</a></summary>

## 1. 今日速览  
过去 24 小时，CoPaw 社区活跃度极高：共产生 34 条 Issue 更新（新开/活跃 14 条，关闭 20 条）与 46 条 PR 更新（待合并 32 条，已合并/关闭 14 条）。随着 v2.0.0-beta.4 正式发布，稳定性修复与体验优化成为主旋律，核心滚动压缩逻辑得到强化，同时多个渠道扩展、安全加固和内存管理特性持续推进，项目整体处于积极迭代状态。

## 2. 版本发布  
**v2.0.0-beta.4** 今日发布  
- **变更内容**：  
  - 版本号晋升至 2.0.0b4，统一标记修复（PR #5837）。  
  - `scroll` 压缩模块引入活性回合保护、分级压力释放，并使召回失败状态更醒目（PR#? 截断，对应 Issue 描述为 `fix(scroll): protect the active turn, add graduated pressure relief, and make recall failures unmistakable`）。  
- **破坏性变更**：暂无报告。  
- **迁移注意事项**：升级至本版本后，建议检查长时间对话的上下文压缩行为，确保压缩索引中活动回合定位正常。

## 3. 项目进展  
今日已合并/关闭 14 个 PR，多项重要修复落地，显著提升稳定性与用户体验：  
- **[scoll] 回合锚定与索引可用性**：`fix(scroll): anchor the live turn with a seam banner in the eviction index` (#5871) 与 `fix(scroll): label un-headlined evicted spans in the eviction index` (#5848) 均已合并。前者在压缩索引中插入活动 banner，避免当前回合迷失于压缩历史；后者为无标题区间提供清晰标签，消除裸索引行，大幅改善压缩后上下文的可读性和模型理解度。  
- **[agents] 工具消息防丢**：`fix(agents): stop dropping self-paired tool messages during sanitation` (#5792) 合入，修复 AgentScope 2.0 自配消息在清洗过程中被误删的问题，保障工具链完整。  
- **[memory] 蒸馏工具插件**：虽然 `feat: add memory-distill tool plugin with title-diffing distillation engine` (#4171) 仍处于 Review 状态，但其持续推动记忆压缩能力；此外，`feat(memory): add reranker for search results on reme0.4` (#5692) 也在 Review，表明记忆检索精确度提升已进入集成阶段。  
- **[channels] 渠道扩展**：Azure Bot 频道 (#5849) 与 Zalo Bot 频道 (#5801) 均已进入代码审查，将进一步丰富即时通讯渠道生态。  
- **[console] 斜杠命令**：`feat(console, tui): expose system commands in slash autocomplete` (#5869) 已完成 Review，预计很快合入，提升所有终端界面的命令发现效率。  

已关闭的 Issue 中，多项高频反馈修复关闭，如 `v2.00b3 审批弹窗误弹出` (#5846)、`上下文压缩导致信息全丢失` (#5171)、`deepseek 思考过程中卡死` (#5328) 等，表明团队对社区反馈响应迅速。

## 4. 社区热点  
- 🔥 **飞书通道无回复** (`#5757`) • 13 评论  
  用户报告飞书通道首次消息可回复，续发则机器人无响应，Docker 与 AgentScope Platform 实例均复现。该问题悬而未决，引发飞书用户高度关注。  
- 🔥 **v2.0b3 审批弹窗误弹** (`#5846`) • 10 评论（Closed）  
  关闭模式下工具审批弹窗仍出现，阻塞自动任务流。已通过 v2.0.0-beta.4 修复，用户验证通过。  
- 🔥 **上下文压缩丢失信息** (`#5171`) • 9 评论（Closed）  
  人设文件 token 过大导致压缩后上下文归零，任务中断。团队已在最新版中处理阈值逻辑。  
- 🔥 **定时任务弹窗开关** (`#5797`) • 6 评论（Open）  
  用户强烈要求定时任务结果增加弹窗开关，反对“一刀切”关闭弹窗，希望按任务粒度配置。相关 PR 尚未出现，但讨论热度反映刚性需求。  
- 🔥 **Console 流式输出卡顿** (`#5725`) • 5 评论（Open）  
  用户对比 DeepSeek 网页版无卡顿，要求优化长时间流式输出的渲染性能，对云部署场景体验影响较大。

## 5. Bug 与稳定性  
以下按严重度排列今日活跃 Bug，部分已有对应修复 PR：  

| 严重度 | Issue | 描述 | 状态 | 
|--------|-------|------|------|
| 高 | `#5860` | 2.0 beta 频繁出现对话进度丢失和无限循环，上下文跳跃、重复输出。 | Open，暂无 PR |
| 高 | `#5872` | Docker 容器内 `browser_use` 因 dbus 连接失败导致 Chromium 退出，工具超时。 | Open，无 PR |
| 高 | `#5856` | 上下文压缩后 `tool_call` 结构丢失，引发 400 错误 / 消息数不匹配 (v1.1.12.post2)。 | Open，无 PR |
| 高 | `#5858` | 基础格式化器静默丢弃助手消息，破坏 `tool_use`/`tool_result` 配对 (v1.1.12.post2)。 | Open，无 PR |
| 中 | `#5379` | Python 安装后启动报 `Internal Server Error`，日志指向 `get_remote_addr`，影响 Windows 用户。 | Open，8 评论，无 PR |
| 中 | `#5259` | Windows 上向量索引无法持久化，必须保持“重建记忆索引”开启，重启后记忆搜索失效。 | Open，4 评论，无 PR |
| 中 | `#5863` | 编码会话中的图片文件显示二进制而非渲染图像。 | Open，3 评论，无 PR |
| 中 | `#5868` | Matrix 频道 token 认证失败，错误 `M_MISSING_TOKEN`，升级后出现。 | Open，已有 fix PR `#5873`（待审核） |
| 中 | `#5378` | 自定义模型后查询框被 Endpoint 覆盖，导致页面空白 (v1.1.12.post1)。 | Closed（已修复） |

## 6. 功能请求与路线图信号  
- **任务协作与提醒**：定时任务弹窗开关 (`#5797`) 呼声极高，用户希望按任务选择是否弹窗并设置停留时间；任务完成系统通知音 (`#5852`) 也获得共鸣。相关特性有望进入下一版规划。  
- **Agent 团队协作**：类似 WorkBuddy Expert Team 的多 Agent 协同能力 (`#5139`) 反复被提及，PR 中虽未直接对应，但 `feat(memory): add reranker` 等增强正在为复杂协作打下基础。  
- **桌面端体验**：最小化到系统托盘 (`#5312`)、附件拖拽上传 (`#5374`) 均已关闭，显著改善桌面使用体验。  
- **多语言渠道扩展**：Azure Bot 频道 (#5849) 和 Zalo 频道 (#5801) 的 PR 表明项目正在向全球即时通讯生态渗透。  
- **视觉模型降级**：`feat(agents): implement vision fallback for text-only models` (#5726) 正在审查，解决纯文本模型处理图片的需求，会受到多模态用户的欢迎。  
- **命令行与记忆**：系统命令自动补全 (#5869) 和记忆蒸馏 (#4171) 均已进入后期审核，预计很快合入。

## 7. 用户反馈摘要  
从 Issue 评论中提炼的真实用户声音：  
- **对飞书通道的挫败感**：`#5757` 下多位用户描述“发了消息，机器人已读不回”，严重影响日常工作流依赖。  
- **对上下文压缩“一刀切”的抱怨**：`#5171` 中用户指出人设文件稍大便导致全量压缩，质问“为何不保留关键信息或按条数保留”，希望压缩策略更智能。  
- **定时任务弹窗的博弈**：`#5797` 用户直言“有人反对就都关掉，千问不要因噎废食”，强调需要用户自主权，反映出对开发者代做决定的无奈。  
- **流式卡顿影响生产力**：`#5725` 中用户提供对比证据，要求优化渲染性能，侧面说明 CoPaw 正被用于长文本生成等重任务场景。  
- **v2.0 beta 用户的焦虑**：`#5860` 反馈对话丢失和无限循环，用户表示“完全不知道 agent 在回答什么”，对 beta 稳定性提出质疑，但多数仍愿意测试并报告。  
- **积极互动**：多位 contributor 提交 first-time-contributor PR（`#5751`, `#5801`, `#5792`, `#5869`），社区参与度健康。

## 8. 待处理积压  
以下重要 Issue/PR 长期未关闭，需项目维护者关注：  

- **`#5259` Windows 向量索引持久化失效**（17 天前，4 评论）。直接影响 Windows 用户记忆搜索功能，暂无修复 PR。  
- **`#5379` Python 安装后启动 Internal Server Error**（17 天前，8 评论）。影响面较广，至今未关闭，缺少明确解决方案。  
- **`#5797` 定时任务弹窗开关**（3 天前，6 评论）。强烈需求但无对应 PR 承接。  
- **`#5757` 飞书不回复**（6 天前，13 评论）。活跃度高，未安排修复。  
- **PR `#4171` 记忆蒸馏工具插件**（已开启 2 个月）。进展缓慢，虽有 Review 但未合并，可能阻塞记忆增强路线图。  
- **PR `#5692` memory reranker**（8 天前）。处于 Review 状态，需推动以提升记忆检索质量。

总体来看，CoPaw 社区在发布 beta 版后反馈密集，项目团队对稳定性问题响应及时，但桌面端持久化、飞书通道和上下文压缩策略等痛点仍需倾斜资源解决。建议下一迭代中优先攻克高频稳定性 Bug 并将社区强诉求纳入功能清单。

</details>

<details>
<summary><strong>ZeptoClaw</strong> — <a href="https://github.com/qhkm/zeptoclaw">qhkm/zeptoclaw</a></summary>

过去24小时无活动。

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目日报 – 2026-07-09

## 1. 今日速览
今日 ZeroClaw 仓库继续保持极高活跃度：过去 24 小时新增/活跃 Issue 40 条、关闭 10 条；PR 动态达 50 条，其中 45 条仍处于“待合并”状态。无新版本发布。整体呈现“群智涌现”模式 —— 社区围绕通道互操作性、安全可观测性、插件化架构展开集中讨论，同时多个高严重级 Bug 正在推进修复，项目健康度良好但合并积压明显。

## 2. 版本发布
无新版本发布，本日无相关更新。

## 3. 项目进展
过去 24 小时关闭了 10 个 Issue，其中包含多个长期存在的功能缺陷修复：
- #4873 修复了“飞书集成后默认调用 LLM 而非 Agent”的问题。
- #6173 解决 `model_switch` 工具切换不持久且 Gateway/UI 路径完全忽略切换的问题。
- #8334 修复 `skills install/list/remove` 在多 Agent 运行时指向错误目录的回归。
- #8553 使 Agent 能够将进程环境变量作为 `http_request` 的鉴权 secret 使用。
这些关闭项表明运行时稳定性与多 Agent 工作流得到了显著加固。

PR 方面虽仅有 5 条被合并/关闭，但仍有大量功能 PR 处于审查中，如 OpenAI 兼容端点 (#8486)、频道会话 TTL (#8139)、Agent 重命名 (#7954) 等，项目前进步伐紧凑，暂无破坏性变更。

## 4. 社区热点
### 讨论最热烈的 Issue
- [#5862] `[Bug]: zeroclaw does not know it can add cron` – 13 条评论。用户发现 Agent 声称无法设置定时任务，但实际上 CLI 已支持 `zeroclaw cron`，诉求是让模型感知工具边界、改进工具自举能力。
- [#8424] `RFC: .ignore 文件机制` – 7 条评论。社区强烈要求引入类似 `.gitignore` 的工作区文件保护机制，防止 AI 代理读取/修改敏感文件（如 `.env`、`config.yaml`），呼声极高且已关联风险标签 `risk:high`。
- [#6034] `单轮/多轮对话丢失 user message` – 7 条评论，严重程度 S1，用户连接自定义 Qwen 模型时出现请求错误，直接阻塞工作流，反映出 provider 兼容层存在参数处理缺陷。
- [#8226] `支持 per-agent 自定义环境变量` – 5 条评论，多租户场景刚需，已有 RFC 性质提案，并吸引到 `risk:high` 关注。

### 讨论热烈的 Pull Request
- [#8707] `operator-bind identities 免绑定码` 和 [#8486] `OpenAI 兼容端点` 均为结构级特性，评论数虽未见显示但基于复杂度和标签判断为社区高度关注对象。
- [#8880] `SOP 审批 broker 组与多数同意` 作为安全审批里程碑的一部分，与 [#8848] 一同堆叠，显示出企业对操作审批流程的严肃需求。

## 5. Bug 与稳定性
按严重等级排序：
- **S0 – 数据丢失 / 安全风险**
  - #6672: 当使用小米 MiMo 思考模式模型时，`reasoning_content` 在工具调用循环中未回传，导致 Agent 丢失推理上下文，标签 `risk:high`，尚无修复 PR。
  - #6558: 阿里云 DashScope provider 返回 405 错误，配置问题遗留，`needs-author-action` 待提供更多信息。

- **S1 – 工作流阻塞**
  - #6034: 多轮对话中 `user message` 随机丢失，自定义 API 报 400 错误，影响所有依赖此 provider 的流程，急需可靠复现步骤。
  - #6002: Telegram 频道下助手未被明确寻址，导致模型调用异常，虽有 `needs-author-action` 但已被接受。

- **S2 – 显著降级**
  - #8762: Anthropic provider 使用固定总超时，长任务（文档合成等）在约 120 秒时中断，虽无数据丢失但体验受损，已确认待改进。
  - #6517: 上下文溢出导致幻觉和主题漂移，`stale-candidate` 标签，需探究压缩或内存策略。
  - #8875: 配置加载器降级警告指向 `config migrate`，但未显示解析错误，给排障用户带来困惑。

已有对应修复 PR 的 Bug：
- #7828 `UTF-8 字符边界截断审计` – 由 PR #8873 提供全局安全加固。
- #5903 MCP 注册表跨心跳共享 – PR #8866 直接对应。

## 6. 功能请求与路线图信号
- **安全与隔离**：#8424 工作区 `.ignore` 机制，已获 RFC 标签，社区认可度极高，有望在下一版本落地。
- **多租户与环境分离**：#8226 per-agent 环境变量与 secrets，与 SOP 审批 (#8880) 相互配合，构筑企业级基座。
- **生态兼容性**：#8550 / #8603 OpenAI Chat Completions 适配器已有两大 PR (#8486 大规模实现，#8603 RFC 设计)，旨在打通 Open WebUI/LobeChat 等客户端，采纳概率极高。
- **插件化演进**：#8850 将可选通道/工具从编译特性迁移到 WASM 运行时插件，#7497 规划 OCI 注册表分发，表明项目正在向“可扩展发行”转型。
- **Tools 增强**：#8602 文件读取工具对标 Claude Code，成为新的增强追踪器；#7431 预转向意图抽取可提升路由体验。

这些信号与现有 PR 高度吻合，预计 v0.8.3 之后的某个里程碑会集中纳入。

## 7. 用户反馈摘要
- **挫败点**：用户无法让 ZeroClaw “意识到”自身可用的 cron 功能 (#5862)；自定义 provider 集成中频繁出现消息丢失和 400 错误 (#6034)；Android Termux 安装不支持 `aarch64` 导致用户受阻 (#7911)。
- **安全焦虑**：多位用户提出工作区敏感文件防护和 per-agent 密钥隔离，显示生产环境对零信任模型的急迫需求。
- **性能期待**：用户呼吁原生上下文压缩 (#7673) 和更好的超时控制 (#8762)，降低长对话与大数据量任务的中断概率。
- **积极信号**：社区对 RFC 讨论参与度极高，并自发贡献测试与 PR（如 #8132 用 Rust→Wasm 替换 React），显示出对项目架构方向的高度认可。

## 8. 待处理积压
- #5862（Agent cron 能力知晓问题）已开启两月余，带 `stale-candidate`，需维护者介入或关闭决定。
- #6034（消息丢失）急需可复现案例，`needs-author-action` 已保留多周。
- #6517（上下文溢出幻觉）及 #6558（provider 错误）均标记 `stale-candidate`，需敦促上报环境信息或结案。
- PR #8267、#8448 等小规模测试/硬件 PR 处于 `needs-author-action` 状态，建议合并或指导作者调整。

整体而言，ZeroClaw 社区在质量加固与架构演进两端同时并进，若能及时消化 PR 积压并处理停滞的故障单，项目有望在近期迎来一次稳健的功能更新。

</details>

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*