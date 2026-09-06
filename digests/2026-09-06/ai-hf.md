# Hugging Face 热门模型日报 2026-09-06

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-09-06 04:06 UTC

---

# Hugging Face 热门模型日报（2026-09-06）

## 今日速览

今日榜单最显眼的信号是“新版本前夜”：Qwen3.8-27B 以 14,044 周点赞领跑热度，并带动官方 GGUF、社区量化、Uncensored/abliterated 衍生版形成完整生态链。DeepSeek-V4-Flash-Vision-Exp、Qwen3.8-Flash-Next、GLM-5.3-Flash 等“Flash / Next / Exp”命名密集出现，说明头部厂商正用开源权重做多模态能力的快速迭代。视频生成热度同样走高，MiniMax-H3、LTX-2.5 与 FastVideo 的 4-step 预览模型均进入热门榜。与此同时，GPT-2、BERT、MiniLM、CLIP 等老牌基础模型仍保持千万级下载，表明“小模型 + 嵌入”依然是生产级落地的高频选择。

## 热门模型

### 🧠 语言模型（LLM、对话、基础文本生成）

| 模型 | 作者 | 周赞 | 下载 | 一句话说明 |
|---|---|---:|---:|---|
| [XHToken/Spark-X2.5-4B](https://huggingface.co/XHToken/Spark-X2.5-4B) | XHToken | 551 | 4,755 | 4B 级轻量文本生成模型，面向可本地部署的 LLM 场景。 |
| [zai-org/GLM-5.3](https://huggingface.co/zai-org/GLM-5.3) | zai-org | 1,720 | 370,417 | GLM-5.3 主版本，采用 MoE 稀疏架构，主打强化文本对话能力。 |
| [openai-community/gpt2](https://huggingface.co/openai-community/gpt2) | openai-community | 3,702 | 14,739,982 | 经典 GPT-2 模型，长期作为教学、基线与轻量生成测试对象。 |
| [google-bert/bert-base-uncased](https://huggingface.co/google-bert/bert-base-uncased) | google-bert | 2,986 | 56,175,564 | 经典 BERT 编码器，仍是文本分类、检索和蒸馏任务的重要底座。 |
| [distilbert/distilbert-base-uncased](https://huggingface.co/distilbert/distilbert-base-uncased) | distilbert | 1,156 | 7,101,423 | BERT 的轻量蒸馏版本，适合低延迟文本理解与嵌入场景。 |
| [IFM/K2-Horizon-MoVA-36B-A4B](https://huggingface.co/IFM/K2-Horizon-MoVA-36B-A4B) | IFM | 175 | 1,333 | 36B 总参数、4B 激活的 MoE 模型，代表高效稀疏推理方向。 |
| [tencent/Hy4-preview](https://huggingface.co/tencent/Hy4-preview) | tencent | 443 | 6,195 | 腾讯混云 Hy4 预览版文本生成模型，延续 Hunyuan 系列迭代。 |

### 🎨 多模态与生成（图像、视频、音频、文本到X）

| 模型 | 作者 | 周赞 | 下载 | 一句话说明 |
|---|---|---:|---:|---|
| [deepseek-ai/DeepSeek-V4-Flash-Vision-Exp](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-Vision-Exp) | deepseek-ai | 684 | 184,542 | DeepSeek V4 的 Flash 视觉实验版，主打图文输入到文本的快速多模态理解。 |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 14,044 | 6,024,467 | 本周热度最高模型之一，27B 规模多模态对话模型，支持图像与文本输入。 |
| [Qwen/Qwen3.8-Flash-Next](https://huggingface.co/Qwen/Qwen3.8-Flash-Next) | Qwen | 4,918 | 401,327 | 标记为 qwen4_exp 的 Flash 快速迭代版，探索下一代多模态对话架构。 |
| [zai-org/GLM-5.3-Flash](https://huggingface.co/zai-org/GLM-5.3-Flash) | zai-org | 2,080 | 727,610 | GLM-5.3 的 Flash 多模态版，下载量超过同族主模型，适合轻量化图文对话。 |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 2,894 | 1,484,329 | 支持图像/文本/视频到视频的新一代扩散视频生成模型。 |
| [BreezeBlue/Breeze-TTS-2](https://huggingface.co/BreezeBlue/Breeze-TTS-2) | BreezeBlue | 449 | 5,962 | 新出现的高质量文本转语音模型，尚处热度早期。 |
| [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | MiniMaxAI | 4,938 | 5,057,414 | MiniMax H3 视频生成模型，同时支持文本到视频与图像到视频。 |
| [OpenVDN/vdn-minimax-h3](https://huggingface.co/OpenVDN/vdn-minimax-h3) | OpenVDN | 191 | 0 | 基于 MiniMax-H3 的社区微调文本到视频版本，刚发布仍在观望期。 |
| [FastVideo/FastVideo-FastH3-4-step-Preview-v1-VSA-DataFree](https://huggingface.co/FastVideo/FastVideo-FastH3-4-step-Preview-v1-VSA-DataFree) | FastVideo | 279 | 22,851 | 主打 4 步采样的 H3 视频生成预览版，研究高效视频推理路径。 |

### 🔧 专用模型（时序、嵌入、语音、图文检索）

| 模型 | 作者 | 周赞 | 下载 | 一句话说明 |
|---|---|---:|---:|---|
| [google/timesfm-3.0-pytorch](https://huggingface.co/google/timesfm-3.0-pytorch) | google | 460 | 123,025 | Google 时序预测基础模型，面向金融、销量、传感器等时间序列场景。 |
| [sentence-transformers/all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) | sentence-transformers | 5,560 | 255,006,933 | 最流行的轻量句向量模型之一，广泛用于语义搜索与 RAG。 |
| [facebook/mms-300m](https://huggingface.co/facebook/mms-300m) | facebook | 263 | 12,961 | 基于 wav2vec2 的多语种语音预训练模型，覆盖多语种语音理解任务。 |
| [openai/clip-vit-base-patch32](https://huggingface.co/openai/clip-vit-base-patch32) | openai | 1,210 | 20,755,211 | 经典 CLIP 图文对比模型，支持零样本图像分类与图文检索。 |

### 📦 微调与量化（社区微调 / GGUF / 去拒答）

| 模型 | 作者 | 周赞 | 下载 | 一句话说明 |
|---|---|---:|---:|---|
| [ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF](https://huggingface.co/ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF) | ISTA-DASLab | 418 | 297,493 | Qwen3.8-27B 的 GSQ/RCO 混合精度 GGUF 量化版，降低本地部署门槛。 |
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 3,548 | 10,157,510 | Unsloth 官方 GGUF 量化版，下载量超千万，是本地运行 Qwen3.8-27B 的主要格式。 |
| [DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF) | DavidAU | 227 | 174,405 | 面向编码与本地对话的社区魔改 GGUF，集合 TURBO、MTP、Uncensored 等社区偏好。 |
| [unsloth/Qwen3.8-Flash-Next-GGUF](https://huggingface.co/unsloth/Qwen3.8-Flash-Next-GGUF) | unsloth | 800 | 780,823 | Qwen3.8-Flash-Next 的 GGUF 量化版，便于在低资源设备上体验新架构。 |
| [HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF) | HauhauCS | 966 | 1,527,627 | 带 MTP 的社区去审查微调，下载量已超 150 万。 |
| [OBLITERATUS/Qwen3.8-27B-OBLITERATED](https://huggingface.co/OBLITERATUS/Qwen3.8-27B-OBLITERATED) | OBLITERATUS | 1,096 | 968,936 | 通过 abliteration 去除安全拒答的版本，同时提供 MLX、GGUF 与 safetensors。 |
| [Jackrong/Qwopus3.8-27B-Flash-GGUF](https://huggingface.co/Jackrong/Qwopus3.8-27B-Flash-GGUF) | Jackrong | 120 | 10,680 | 社区重打包的 27B Flash 视觉模型 GGUF，方便 llama.cpp 推理。 |
| [JonathanColetti/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/JonathanColetti/Qwen3.8-27B-Uncensored-GGUF) | JonathanColetti | 988 | 2,453,361 | 下载量最高的 Uncensored GGUF 版本之一，主打本地化去审查体验。 |
| [orcarouter/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-GGUF) | orcarouter | 729 | 283,774 | Qwen3.8-27B 的另一条 Uncensored GGUF 分支，社区关注度快速上升。 |
| [orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF) | orcarouter | 240 | 106,845 | Qwen3.8-Flash-Next 的 Uncensored GGUF 版，延续新架构的本地化生态。 |

## 生态信号

Qwen3.8 是本轮最强势的模型家族：原版之外，Unsloth 官方量化、学术量化、社区 Uncensored / abliterated 衍生版层层扩散，形成“开源权重 → GGUF → 本地部署”的完整链条。头部厂商越来越多地使用“Flash / Next / Exp / Preview”等实验代号发布开源权重，而不是直接推出闭源 API，说明开源权重已成为大模型灰度迭代的主渠道。技术上，MoE 与 MTP 特征正在增强渗透：GLM-5.3 采用 MoE 结构，K2-Horizon 达到 36B 总参数 / 4B 激活，多个 Qwen3.8 GGUF 也加入 MTP 支持，体现“更强 + 更低推理成本”的社区共识。此外，MiniMax-H3 打开的下载基础后，FastVideo 等团队开始用 4-step、DataFree 等方案降低视频生成成本。整体来看，中文大模型与开源量化生态正在深度耦合。

## 值得探索

- [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B)：本周生态核心。它既是官方讨论焦点，也催生了大量量化、Uncensored 和微调衍生版，适合作为多模态应用基座进行系统评测。
- [deepseek-ai/DeepSeek-V4-Flash-Vision-Exp](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-Vision-Exp)：处于榜单显眼位置的 V4 实验性视觉模型。其“Flash + Exp”定位可能代表 DeepSeek 对多模态低延迟路线的下一步判断，值得实测。
- [google/timesfm-3.0-pytorch](https://huggingface.co/google/timesfm-3.0-pytorch)：非 LLM 领域的时序预测基础模型，进入热门榜说明企业时间序列场景仍被高度关注，适合该赛道的团队快速验证。

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*