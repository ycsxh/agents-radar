# Hugging Face 热门模型日报 2026-09-04

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-09-04 04:02 UTC

---

# Hugging Face 热门模型日报 · 2026-09-04

## 今日速览

- Qwen 与 GLM 之争成为今日主线：`Qwen3.8-27B` 以 1.38 万周点赞和 525 万下载量稳居人气王，多模态轻量版本也大量上榜。
- 视频生成进入白热化：MiniMax-H3、Lightricks LTX-2.5 下载量均超百万，同时出现 FastVideo 4-step 这类加速推理的新模型。
- 量化与去审查微调生态“刷屏”：unsloth、orcarouter 等团队围绕 Qwen/GLM 发布了至少 10 个 GGUF/FP8/abliterated 版本，本地部署门槛持续降低。
- 头部开源权重完全压制闭源模型：本周榜单几乎全部为可下载权重，Qwen、GLM、DeepSeek、混元形成“中国开源军团”集体登榜。
- 经典模型仍是不可替代底座：all-MiniLM、BERT、CLIP、GPT-2 等老牌权重的下载量继续以千万级稳定增长。

## 热门模型

### 🧠 语言模型（LLM、对话、指令模型）

- [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) — 作者：Qwen | 点赞：13,843 | 下载：5,254,882 — 小尺寸但综合能力极强的开源多模态 LLM，直接拿下本周最高关注度。
- [Qwen/Qwen3.8-Flash-Next](https://huggingface.co/Qwen/Qwen3.8-Flash-Next) — 作者：Qwen | 点赞：4,816 | 下载：263,287 — Qwen 新 Flash 实验版，主打低资源下实现文本+图像对话。
- [openai-community/gpt2](https://huggingface.co/openai-community/gpt2) — 作者：openai-community | 点赞：3,606 | 下载：14,071,683 — 经典开源文本生成模型，仍被大量用作基线、教学和轻量部署。
- [zai-org/GLM-5.3-Flash](https://huggingface.co/zai-org/GLM-5.3-Flash) — 作者：zai-org | 点赞：2,021 | 下载：517,902 — 智谱 GLM-5.3 的轻量多模态版本，兼顾文本与图像理解能力。
- [zai-org/GLM-5.3](https://huggingface.co/zai-org/GLM-5.3) — 作者：zai-org | 点赞：1,619 | 下载：151,021 — 采用 MoE + DSA 架构的新一代 GLM 旗舰模型，代表智谱当前最强开源语言底座。
- [deepseek-ai/DeepSeek-V4-Flash-Vision-Exp](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-Vision-Exp) — 作者：deepseek-ai | 点赞：557 | 下载：54,571 — DeepSeek V4 的视觉快速实验版，延续高性价比推理路线。
- [tencent/Hy4-preview](https://huggingface.co/tencent/Hy4-preview) — 作者：tencent | 点赞：417 | 下载：4,449 — 腾讯混元 Hy4 预览模型，显示混元系列在对话基础模型上的迭代提速。
- [pipecat-ai/phonellm-alpha-1](https://huggingface.co/pipecat-ai/phonellm-alpha-1) — 作者：pipecat-ai | 点赞：208 | 下载：11,526 — 面向电话语音场景的 Alpha 版 LLM，基于 Nemotron-H 架构改造。
- [XHToken/Spark-X2.5-4B](https://huggingface.co/XHToken/Spark-X2.5-4B) — 作者：XHToken | 点赞：168 | 下载：1,514 — 4B 规模 Spark 系列轻量 LLM，适合资源受限场景快速部署。

### 🎨 多模态与生成（图像、视频、音频、文本到 X）

- [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) — 作者：MiniMaxAI | 点赞：4,866 | 下载：5,092,067 — 支持图文联合输入的视频生成模型，下载量破 500 万，是本周视频类第一明星。
- [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) — 作者：Lightricks | 点赞：2,702 | 下载：1,293,463 — 全能视频生成模型，覆盖文生视频、图生视频、视频生视频等多任务。
- [BreezeBlue/Breeze-TTS-2](https://huggingface.co/BreezeBlue/Breeze-TTS-2) — 作者：BreezeBlue | 点赞：398 | 下载：3,861 — 新一代文本转语音模型，以自然度和轻量化获得音频社区关注。
- [FastVideo/FastVideo-FastH3-4-step-Preview-v1-VSA-DataFree](https://huggingface.co/FastVideo/FastVideo-FastH3-4-step-Preview-v1-VSA-DataFree) — 作者：FastVideo | 点赞：256 | 下载：0 — 主推“4 步生成”的视频模型 Preview，面向极速视频推理优化。
- [OpenVDN/vdn-minimax-h3](https://huggingface.co/OpenVDN/vdn-minimax-h3) — 作者：OpenVDN | 点赞：142 | 下载：0 — MiniMax-H3 的社区微调/蒸馏视频版本，刚发布即进入热门榜。

### 🔧 专用模型（嵌入、时序、语音、经典表示学习）

- [sentence-transformers/all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) — 作者：sentence-transformers | 点赞：5,461 | 下载：246,135,287 — 最常用的通用句向量模型，语义检索/相似度应用的基础设施级存在。
- [google-bert/bert-base-uncased](https://huggingface.co/google-bert/bert-base-uncased) — 作者：google-bert | 点赞：2,910 | 下载：58,556,227 — NLP 经典双向编码器，仍是无数任务的默认 baselines 和嵌入工具。
- [openai/clip-vit-base-patch32](https://huggingface.co/openai/clip-vit-base-patch32) — 作者：openai | 点赞：1,136 | 下载：19,936,700 — 经典的图文对比模型，广泛用于零样本图像分类和图文检索。
- [distilbert/distilbert-base-uncased](https://huggingface.co/distilbert/distilbert-base-uncased) — 作者：distilbert | 点赞：1,092 | 下载：6,761,868 — BERT 的轻量蒸馏版，兼顾推理速度与语义表示能力。
- [google/timesfm-3.0-pytorch](https://huggingface.co/google/timesfm-3.0-pytorch) — 作者：google | 点赞：370 | 下载：46,862 — 面向时间序列预测的预训练 PyTorch 模型，适合金融/运维/销量预测等场景。
- [facebook/mms-300m](https://huggingface.co/facebook/mms-300m) — 作者：facebook | 点赞：181 | 下载：12,386 — Meta 多语种语音自监督模型，覆盖大量语种的语音编码与识别任务。

### 📦 微调与量化（社区微调、GGUF、FP8）

- [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) — 作者：unsloth | 点赞：3,449 | 下载：9,553,042 — Qwen3.8-27B 的 GGUF 量化版，下载接近千万，是本地部署 Qwen 的事实标准入口。
- [OBLITERATUS/Qwen3.8-27B-OBLITERATED](https://huggingface.co/OBLITERATUS/Qwen3.8-27B-OBLITERATED) — 作者：OBLITERATUS | 点赞：1,060 | 下载：848,781 — 对 Qwen3.8-27B 做“abliterated”去对齐处理，并提供 MLX/Safetensors/GGUF 多格式。
- [HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF) — 作者：HauhauCS | 点赞：900 | 下载：1,336,061 — 带“Aggressive MTP”的社区去审查 GGUF 版，下载量已破百万。
- [unsloth/Qwen3.8-Flash-Next-GGUF](https://huggingface.co/unsloth/Qwen3.8-Flash-Next-GGUF) — 作者：unsloth | 点赞：764 | 下载：535,984 — Qwen3.8-Flash-Next 的 GGUF 量化版本，为轻量多模态模型提供本地运行支持。
- [orcarouter/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-GGUF) — 作者：orcarouter | 点赞：687 | 下载：262,325 — orcarouter 出品的 Qwen3.8-27B Uncensored GGUF，主打“去审查 + 可量贩”的本地部署。
- [unsloth/GLM-5.3-Flash-GGUF](https://huggingface.co/unsloth/GLM-5.3-Flash-GGUF) — 作者：unsloth | 点赞：349 | 下载：75,195 — GLM-5.3-Flash 的 GGUF 量化版，推动新一代 GLM 进入 Ollama 等本地生态。
- [ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF](https://huggingface.co/ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF) — 作者：ISTA-DASLab | 点赞：250 | 下载：100,110 — 学术实验室采用 GSQ/RCO 混合精度量化策略，探索更高效的低损压缩方案。
- [orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF) — 作者：orcarouter | 点赞：212 | 下载：85,105 — Qwen3.8-Flash-Next 的 Uncensored GGUF 版本，兼顾多模态轻量与本地化。
- [orcarouter/GLM-5.3-Flash-Uncensored-FP8](https://huggingface.co/orcarouter/GLM-5.3-Flash-Uncensored-FP8) — 作者：orcarouter | 点赞：165 | 下载：4,477 — GLM-5.3-Flash 的去审查 FP8 版本，适合在商用级 GPU 上低显存推理。
- [DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF) — 作者：DavidAU | 点赞：140 | 下载：39,646 — 长命名社区融合微调版 GGUF，混合多项社区自定义能力。

## 生态信号

Qwen、GLM、DeepSeek、混元本周密集上榜，且全部采用开源权重；头部实验室正以“小尺寸 + 多模态 + 高吞吐”争夺开发者也入口，Qwen3.8-27B 成为最大枢纽模型。视频生成权重首次形成批量热门，MiniMax-H3、LTX-2.5 下载量巨大，说明生成类模型已从文生图向文生视频/图生视频迁移。社区二次创作高度集中在 GGUF 量化和 abliterated/uncensored 微调，unsloth、orcarouter 成为关键中间层，大幅拉低了端侧部署门槛。与此同时，all-MiniLM、BERT、CLIP 等经典模型下载量持续领跑，形成“前沿模型吸引流量、经典模型稳定跑量”的双层生态。

## 值得探索

- [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B)：本周热度与下载双冠，是具有代表性的新一代小尺寸开源多模态 LLM，适合作为微调和部署基座。
- [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3)：下载破 500 万的视频生成模型，生态正围绕它形成微调/蒸馏分支，值得开发者跟进。
- [ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF](https://huggingface.co/ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF)：少见的学术型混合精度量化模型，对研究低比特压缩和高效率部署具有参考价值。

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*