# Hugging Face 热门模型日报 2026-09-05

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-09-05 03:59 UTC

---

# Hugging Face 热门模型日报

**日期：** 2026-09-05  
**数据来源：** Hugging Face Hub 热门模型周榜（共 30 个）  

---

## 1. 今日速览

- **Qwen3.8 家族强势霸榜**：旗舰多模态模型 Qwen3.8-27B 以单周 13,962 赞断层领先，连带 Flash-Next、近 10 个 GGUF 量化与 Uncensored 衍生版本集体入榜，成为本周 HF 生态绝对中心。
- **视频生成迎来爆发期**：MiniMax-H3（4,912 赞）与 Lightricks LTX-2.5（2,795 赞）双双成为现象级作品，且社区已快速出现 4 步蒸馏与微调版本。
- **国内大模型厂商集中亮相**：DeepSeek-V4 视觉实验版、智谱 GLM-5.3 系列、腾讯混元 Hy4-preview、讯飞星火 Spark-X2.5-4B 等本周均有代表模型上榜。
- **经典基础模型依旧坚挺**：all-MiniLM-L6-v2、BERT、CLIP、GPT-2 等老面孔保持千万/亿级下载量，检索、向量化与基线测试需求依然旺盛。

---

## 2. 热门模型

### 🧠 语言模型（LLM / 对话 / 指令微调）

- [**openai-community/gpt2**](https://huggingface.co/openai-community/gpt2) — 作者：openai-community · 👍 3,661 · ⬇️ 14,607,268  
  经典开源文本生成模型，作为教学、基线对比与兼容性测试的默认选项持续被高频调用。

- [**zai-org/GLM-5.3**](https://huggingface.co/zai-org/GLM-5.3) — 作者：zai-org · 👍 1,706 · ⬇️ 303,534  
  智谱新一代 MoE 文本旗舰（标签 `glm_moe_dsa`），是 GLM-5.3-Flash 多模态版的纯文本基座。

- [**XHToken/Spark-X2.5-4B**](https://huggingface.co/XHToken/Spark-X2.5-4B) — 作者：XHToken · 👍 481 · ⬇️ 3,524  
  星火 Spark 系列开源 4B 轻量语言模型，主打低成本本地部署与私有化文本生成。

- [**tencent/Hy4-preview**](https://huggingface.co/tencent/Hy4-preview) — 作者：tencent · 👍 437 · ⬇️ 5,684  
  腾讯混元 Hunyuan 新一代 Hy4 预览版（标签 `hy_v4`），处于早期公开阶段，值得持续跟踪。

- [**IFM/K2-Horizon-MoVA-36B-A4B**](https://huggingface.co/IFM/K2-Horizon-MoVA-36B-A4B) — 作者：IFM · 👍 156 · ⬇️ 433  
  总参 36B / 激活 4B 的稀疏结构文本生成模型，K2-Horizon 架构路线的新面孔，热度尚在积累。

### 🎨 多模态与生成（图像 / 视频 / 音频 / 文本到 X）

- [**Qwen/Qwen3.8-27B**](https://huggingface.co/Qwen/Qwen3.8-27B) — 作者：Qwen · 👍 13,962 · ⬇️ 5,739,341  
  Qwen3.8 旗舰多模态对话模型，支持图像与文本理解，是本周 HF 流量中心，也是大量 GGUF/微调版本的源头。

- [**MiniMaxAI/MiniMax-H3**](https://huggingface.co/MiniMaxAI/MiniMax-H3) — 作者：MiniMaxAI · 👍 4,912 · ⬇️ 5,118,457  
  MiniMax 新一代视频生成大模型，支持文生视频与图生视频，单周下载超 500 万，现象级新作。

- [**Qwen/Qwen3.8-Flash-Next**](https://huggingface.co/Qwen/Qwen3.8-Flash-Next) — 作者：Qwen · 👍 4,880 · ⬇️ 351,374  
  Qwen 新架构实验标签（`qwen4_exp`）下的多模态对话模型，定位更高效的 Flash 级体验。

- [**Lightricks/LTX-2.5**](https://huggingface.co/Lightricks/LTX-2.5) — 作者：Lightricks · 👍 2,795 · ⬇️ 1,399,511  
  Lightricks 新一代扩散式视频生成模型，支持图像/文本/视频到视频等多输入模态。

- [**zai-org/GLM-5.3-Flash**](https://huggingface.co/zai-org/GLM-5.3-Flash) — 作者：zai-org · 👍 2,053 · ⬇️ 654,957  
  智谱 GLM-5.3 的多模态 Flash 版本（`glm5_next`），兼顾视觉理解与响应效率。

- [**deepseek-ai/DeepSeek-V4-Flash-Vision-Exp**](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-Vision-Exp) — 作者：deepseek-ai · 👍 608 · ⬇️ 133,024  
  DeepSeek-V4 系列的视觉语言实验版，Flash 轻量定位，关注度稳步上升。

- [**BreezeBlue/Breeze-TTS-2**](https://huggingface.co/BreezeBlue/Breeze-TTS-2) — 作者：BreezeBlue · 👍 434 · ⬇️ 5,388  
  新一代 Breeze 语音合成（TTS）模型，是本周音频生成赛道的小热门。

- [**FastVideo/FastVideo-FastH3-4-step-Preview-v1-VSA-DataFree**](https://huggingface.co/FastVideo/FastVideo-FastH3-4-step-Preview-v1-VSA-DataFree) — 作者：FastVideo · 👍 277 · ⬇️ 0  
  MiniMax-H3 的高速 4 步采样蒸馏预览版，主打 DataFree 数据无关蒸馏，刚发布、下载仍为 0，值得抢先测试。

### 🔧 专用模型（检索 / 嵌入 / 语音 / 时序 / 经典基础模型）

- [**sentence-transformers/all-MiniLM-L6-v2**](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) — 作者：sentence-transformers · 👍 5,519 · ⬇️ 253,789,790  
  通用句子嵌入标准件，语义检索与向量库基础设施，历史下载超 2.5 亿。

- [**google-bert/bert-base-uncased**](https://huggingface.co/google-bert/bert-base-uncased) — 作者：google-bert · 👍 2,951 · ⬇️ 58,675,189  
  BERT 基础编码器，MLM 表征与迁移学习基线，常青树级模型。

- [**openai/clip-vit-base-patch32**](https://huggingface.co/openai/clip-vit-base-patch32) — 作者：openai · 👍 1,185 · ⬇️ 20,569,141  
  图像-文本对齐模型 CLIP 的经典版本，零样本图像分类与多模态检索的常用骨干。

- [**distilbert/distilbert-base-uncased**](https://huggingface.co/distilbert/distilbert-base-uncased) — 作者：distilbert · 👍 1,133 · ⬇️ 7,067,963  
  BERT 的蒸馏轻量版，兼顾速度与效果，适合快速原型与资源受限场景。

- [**google/timesfm-3.0-pytorch**](https://huggingface.co/google/timesfm-3.0-pytorch) — 作者：google · 👍 432 · ⬇️ 105,304  
  时序预测基础模型 TimesFM 3.0 PyTorch 版，面向时间序列预测这一垂直赛道。

- [**facebook/mms-300m**](https://huggingface.co/facebook/mms-300m) — 作者：facebook · 👍 237 · ⬇️ 12,823  
  Meta MMS 系列 300M 语音预训练模型（wav2vec2），覆盖多语种语音表示与 ASR 迁移。

### 📦 微调与量化（社区微调 / GGUF / FP8 / Abliterated）

- [**unsloth/Qwen3.8-27B-GGUF**](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) — 作者：unsloth · 👍 3,514 · ⬇️ 9,951,693  
  Qwen3.8-27B 最主流的社区 GGUF 量化包，下载近千万，是本地部署首选。

- [**OBLITERATUS/Qwen3.8-27B-OBLITERATED**](https://huggingface.co/OBLITERATUS/Qwen3.8-27B-OBLITERATED) — 作者：OBLITERATUS · 👍 1,090 · ⬇️ 928,393  
  对 Qwen3.8-27B 进行“abliterated”去审查改造，提供 MLX / Safetensors / GGUF 多格式。

- [**JonathanColetti/Qwen3.8-27B-Uncensored-GGUF**](https://huggingface.co/JonathanColetti/Qwen3.8-27B-Uncensored-GGUF) — 作者：JonathanColetti · 👍 971 · ⬇️ 2,395,758  
  面向 llama.cpp 的 Qwen3.8-27B Uncensored GGUF 版，支持 MTP，本地运行热度很高。

- [**HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF**](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF) — 作者：HauhauCS · 👍 949 · ⬇️ 1,463,966  
  激进风格调校 + MTP 支持的 Qwen3.8-27B Uncensored GGUF 版，体现社区个性化微调偏好。

- [**unsloth/Qwen3.8-Flash-Next-GGUF**](https://huggingface.co/unsloth/Qwen3.8-Flash-Next-GGUF) — 作者：unsloth · 👍 788 · ⬇️ 702,251  
  Qwen3.8-Flash-Next 的官方社区量化系列，让轻量多模态模型更易被本地调用。

- [**orcarouter/Qwen3.8-27B-Uncensored-GGUF**](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-GGUF) — 作者：orcarouter · 👍 717 · ⬇️ 276,706  
  主打免审查对话体验的 Qwen3.8-27B GGUF 版本，下载量稳定增长。

- [**ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF**](https://huggingface.co/ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF) — 作者：ISTA-DASLab · 👍 315 · ⬇️ 206,575  
  DASLab 出品的科研向量化版本，采用 GSQ-RCO 混合精度量化路线，适合关注压缩质量的玩家。

- [**orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF**](https://huggingface.co/orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF) — 作者：orcarouter · 👍 232 · ⬇️ 97,994  
  Qwen3.8-Flash-Next 的 Uncensored GGUF 衍生版，延续“轻量 + 去审查”社区配方。

- [**DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF**](https://huggingface.co/DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF) — 作者：DavidAU · 👍 184 · ⬇️ 95,226  
  社区“缝合怪”式密集魔改版本，融合 Uncensored、Coder、MTP 等标签，代表个人化二创的长尾生态。

- [**orcarouter/GLM-5.3-Flash-Uncensored-FP8**](https://huggingface.co/orcarouter/GLM-5.3-Flash-Uncensored-FP8) — 作者：orcarouter · 👍 183 · ⬇️ 7,782  
  GLM-5.3-Flash 的 FP8 量化 + 去审查版本，是榜单中少见的 FP8 精度多模态衍生模型。

- [**OpenVDN/vdn-minimax-h3**](https://huggingface.co/OpenVDN/vdn-minimax-h3) — 作者：OpenVDN · 👍 175 · ⬇️ 0  
  MiniMax-H3 的开源社区微调视频模型，刚发布暂无下载，是观察视频模型微调浪潮的窗口。

---

## 3. 生态信号

Qwen3.8 是绝对主角：旗舰与 Flash-Next 双线并发，GGUF / Uncensored 衍生版本超十个，几乎包揽微调与量化榜单。去审查（abliterated / Uncensored）已成规模化供给，对齐策略正变为社区用户的“可选项”。视频生成则在复刻 LLM 的生态打法：MiniMax-H3 首发周内便出现 4 步蒸馏与社区微调。此外，上榜开源权重集中于 Qwen、GLM、DeepSeek、混元、星火等中国厂商，闭源模型未进入趋势榜，开源权重继续主导草根开发者生态。

---

## 4. 值得探索

1. [**Qwen/Qwen3.8-27B**](https://huggingface.co/Qwen/Qwen3.8-27B)  
   本周最值得先跑通的基座模型：自身是多模态旗舰，周边还有 Unsloth GGUF、Abliterated 等完整生态，适合从对话、视觉理解到本地部署的全链路研究。

2. [**MiniMaxAI/MiniMax-H3**](https://huggingface.co/MiniMaxAI/MiniMax-H3)  
   视频生成顶流，单周 500 万+ 下载，社区已出现 FastVideo 4 步蒸馏与 OpenVDN 微调，推荐实际生成视频对比其画质与可控性。

3. [**google/timesfm-3.0-pytorch**](https://huggingface.co/google/timesfm-3.0-pytorch)  
   在 LLM 与视频热潮中显得独特的时序预测基础模型，赛道竞争少、泛化价值高，适合金融、能源、运维等时序场景团队深入研究。

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*