# Hugging Face 热门模型日报 2026-07-09

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-07-08 17:22 UTC

---

好的，这是为你准备的《Hugging Face 热门模型日报》。

---

### **Hugging Face 热门模型日报 | 2026年7月9日**

#### **今日速览**

今日Hugging Face生态异常活跃，两大趋势尤为突出：一是以**GLM-5.2**和**DeepSeek-V4-Pro-DSpark**为代表的国产大模型备受瞩目，社区热度极高；二是**Qwen3.6**系列及其量化版（GGUF）成为绝对主流，从7B到35B多个规模版本霸榜。此外，NVIDIA发布了多款专用模型，覆盖**细粒度定位**和**低精度推理**，显示出硬件厂商在应用层工具上的布局。社区微调与量化活动依然火爆，**Ornith**和**Qwythos**等创意模型也获得了大量关注。

---

#### **热门模型**

##### 🧠 **语言模型 (LLM、对话模型、指令微调)**

*   **[zai-org/GLM-5.2](https://huggingface.co/zai-org/GLM-5.2)** by zai-org | 点赞: 3,648 | 下载: 281K
    *   周榜冠军，智谱AI最新旗舰级MoE语言模型，以超高点赞数展现了其在社区中的巨大影响力。
*   **[deepseek-ai/DeepSeek-V4-Pro-DSpark](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-DSpark)** by deepseek-ai | 点赞: 433 | 下载: 15K
    *   深度求索的最新MoE模型，带有“DSpark”特性，代表了国产大模型在顶尖推理能力上的最新探索。
*   **[tencent/Hy3](https://huggingface.co/tencent/Hy3)** by tencent | 点赞: 548 | 下载: 121
    *   腾讯发布的Hunyuan系列第三代模型，尽管目前下载量不高，但高点赞数预示了其商业级文本生成质量。
*   **[meituan-longcat/LongCat-2.0](https://huggingface.co/meituan-longcat/LongCat-2.0)** by meituan-longcat | 点赞: 148 | 下载: 385
    *   美团的对话模型新版本，专注长上下文处理，体现了行业用户对特定场景模型的需求。
*   **[poolside/Laguna-XS-2.1](https://huggingface.co/poolside/Laguna-XS-2.1)** by poolside | 点赞: 75 | 下载: 3K
    *   专注软件工程场景的“极精”模型（XS），2.1版本进一步优化了代码生成与理解能力。

##### 🎨 **多模态与生成 (图像、视频、音频、文本到X)**

*   **[nvidia/LocateAnything-3B](https://huggingface.co/nvidia/LocateAnything-3B)** by nvidia | 点赞: 2,665 | 下载: 1.4M
    *   NVIDIA推出的视觉定位模型，能以极低的参数量（3B）实现“指哪打哪”的精准交互，下载量惊人。
*   **[baidu/Unlimited-OCR](https://huggingface.co/baidu/Unlimited-OCR)** by baidu | 点赞: 1,861 | 下载: 1M
    *   百度发布的无限制OCR模型，支持复杂场景下的文字识别，以优质的开源表现迅速获得社区认可。
*   **[krea/Krea-2-Turbo](https://huggingface.co/krea/Krea-2-Turbo)** by krea | 点赞: 552 | 下载: 123K
    *   知名创意工具Krea的最新图像生成模型，速度更快，其在线平台的成功带动了开源模型的热度。
*   **[HauhauCS/Qwen3.6-35B-A3B-Uncensored-...](https://huggingface.co/HauhauCS/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive)** by HauhauCS | 点赞: 2,569 | 下载: 2.8M
    *   基于Qwen3.6的“无审查”社区微调版，下载量极高，显示了社区对特定风格和自由度模型的需求。
*   **[bottlecapai/ThinkingCap-Qwen3.6-27B](https://huggingface.co/bottlecapai/ThinkingCap-Qwen3.6-27B)** by bottlecapai | 点赞: 135 | 下载: 46
    *   “思考帽”模型，旨在增强Qwen3.6的推理链能力，体现了对模型思考过程优化的探索。

##### 🔧 **专用模型 (代码、数学、医疗、嵌入)**

*   **[nvidia/LocateAnything-3B](https://huggingface.co/nvidia/LocateAnything-3B)** by nvidia | 点赞: 2,665 | 下载: 1.4M
    *   **（注：此模型也在此分类）** 作为视觉专用模型，在物体定位任务上表现出色，是NVIDIA在机器人视觉和工业检测领域的布局。
*   **[google/tabfm-1.0.0-pytorch](https://huggingface.co/google/tabfm-1.0.0-pytorch)** by google | 点赞: 305 | 下载: 9K
    *   Google的表格数据基础模型，支持零样本分类与回归，为结构化数据分析带来了新范式。

##### 📦 **微调与量化 (社区微调、GGUF、AWQ)**

*   **[empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF](https://huggingface.co/empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF)** by empero-ai | 点赞: 1,829 | 下载: 1.6M
    *   社区基于Qwen3.5微调的创意模型，GGUF版本下载量巨大，是本地部署和创意写作的首选。
*   **[HauhauCS/Qwen3.6-35B-A3B-Uncensored-...-GGUF](https://huggingface.co/HauhauCS/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive)** by HauhauCS | 点赞: 2,569 | 下载: 2.8M
    *   **（注：此模型也在此分类）** 展示了最大规模的Qwen3.6未经审查量化模型，体现了社区对“释放模型全部潜能”的追求。
*   **[deepreinforce-ai/Ornith-1.0-35B-GGUF](https://huggingface.co/deepreinforce-ai/Ornith-1.0-35B-GGUF)** by deepreinforce-ai | 点赞: 798 | 下载: 502K
    *   35B参数大模型的GGUF版本，下载量超过原始模型，表明用户对高性能、可本地高效部署的量化模型有强烈渴求。
*   **[unsloth/Qwen3.6-27B-MTP-GGUF](https://huggingface.co/unsloth/Qwen3.6-27B-MTP-GGUF)** by unsloth | 点赞: 1,003 | 下载: 2.8M
    *   知名量化工具Unsloth推出的Qwen3.6多令牌预测（MTP）版量化模型，集高效推理与先进架构于一体。
*   **[InternScience/Agents-A1-Q4_K_M-GGUF](https://huggingface.co/InternScience/Agents-A1-Q4_K_M-GGUF)** by InternScience | 点赞: 82 | 下载: 11K
    *   为智能体Agent设计的MoE模型量化版，标志着量化活动正从通用模型向Agent专用模型延伸。

---

#### **生态信号**

*   **模型家族势头**：**Qwen3.6**（和3.5）家族无疑是本周的“生态之王”。从基础模型到各类微调、量化版本全面开花，下载量和点赞数均遥遥领先。**DeepSeek-V4**和**GLM-5.2**则代表了另一梯队国产模型的顶尖实力。**Gemma-4**系列在编码和Agent化方向上的社区微调版（如fable5）也表现强劲。
*   **开源 vs 闭源**：榜单全部为开源模型，社区对开放权重的认可度极高。特别是NVIDIA、Google、腾讯、百度等大型科技公司的积极参与，表明开源生态已从学术驱动转向工业界主导。
*   **量化与微调活动**：**GGUF**几乎成为本地部署的标准格式，Unsloth和NVIDIA的ModelOpt是背后的主要推手。社区微调集中在两个方向：一是**模型能力增强**（如Ornith），二是**特定风格或去除约束**（如HauhauCS的Uncensored版）。这反映出用户群正在快速分化，对模型质量和自由度都有极致要求。

---

#### **值得探索**

1.  **[nvidia/LocateAnything-3B](https://huggingface.co/nvidia/LocateAnything-3B)**: 强烈推荐。仅3B参数实现了强大的视觉定位能力，是机器人、自动驾驶和图像编辑领域的“瑞士军刀”。其高下载量和高点赞数证明了其实用性与受欢迎程度。
2.  **[google/tabfm-1.0.0-pytorch](https://huggingface.co/google/tabfm-1.0.0-pytorch)**: 值得研究。它将Transformer的革命带到了传统表格数据领域，可能改变金融、风控等行业的建模方式，具有很高的研究和应用价值。
3.  **[unsloth/Qwen3.6-27B-MTP-GGUF](https://huggingface.co/unsloth/Qwen3.6-27B-MTP-GGUF)**: 值得尝试。对于开发者和AI爱好者，这是体验最新一代高性能模型的最优解。它集成了Qwen3.6的先进架构、Unsloth的顶尖量化技术和多令牌预测（MTP）带来的推理加速，是本地部署的性能标杆。

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*