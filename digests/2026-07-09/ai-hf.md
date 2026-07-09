# Hugging Face 热门模型日报 2026-07-09

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-07-09 03:55 UTC

---

## 📊 Hugging Face 热门模型日报  
**日期：2026-07-09**  
**数据范围：周点赞数排名 Top 30**

---

### 今日速览
本周 Hugging Face 榜单呈现“基座派生、量化为王”的格局。**Qwen 3.5/3.6 系列**以压倒性数量衍生出大量 GGUF 量化版、无审查版及多模态变体，占据近半榜单。**NVIDIA、腾讯、百度**等大厂集中发布多模态与专用模型，涵盖通用定位、OCR 与表格学习。**DeepSeek-V4** 与 **Gemma 4** 的社区微调与 Agent 版本热度持续攀升，本地化部署生态围绕 llama.cpp 与 MLX 高速运转。

---

### 🔥 热门模型

#### 🧠 语言模型（LLM、对话模型、指令微调）
- **[tencent/Hy3](https://huggingface.co/tencent/Hy3)**  
  作者 tencent · 👍 567 · ⬇️ 121  
  腾讯混元系列最新文本生成模型，采用 Hy3 架构，刚发布即引发关注。
- **[zai-org/GLM-5.2](https://huggingface.co/zai-org/GLM-5.2)**  
  作者 zai-org · 👍 3,672 · ⬇️ 281,584  
  智谱 GLM 最新 MoE 对话模型，支持多轮聊天，点赞量全榜最高，中文社区重点关注。
- **[AliesTaha/fable-traces](https://huggingface.co/AliesTaha/fable-traces)**  
  作者 AliesTaha · 👍 188 · ⬇️ 3,886  
  基于 Qwen3 的指令微调模型，专注于故事与对话风格，社区创意微调代表。
- **[mistralai/Leanstral-1.5-119B-A6B](https://huggingface.co/mistralai/Leanstral-1.5-119B-A6B)**  
  作者 mistralai · 👍 168 · ⬇️ 157  
  Mistral 推出的超大型混合专家模型，极新发布，暂未标注具体任务。
- **[meituan-longcat/LongCat-2.0](https://huggingface.co/meituan-longcat/LongCat-2.0)**  
  作者 meituan-longcat · 👍 153 · ⬇️ 385  
  美团长文本对话模型 LongCat 第二代，主打长上下文理解与闲聊。
- **[deepseek-ai/DeepSeek-V4-Pro-DSpark](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-DSpark)**  
  作者 deepseek-ai · 👍 439 · ⬇️ 15,538  
  DeepSeek V4 系列专业版，面向数据科学推理，延续 DeepSeek 快速迭代势头。

#### 🎨 多模态与生成（图像、视频、音频、文本到 X）
- **[baidu/Unlimited-OCR](https://huggingface.co/baidu/Unlimited-OCR)**  
  作者 baidu · 👍 1,876 · ⬇️ 1,084,945  
  百度推出的无限长度 OCR 模型，可从图像中提取长文本，下载量破百万。
- **[InternScience/Agents-A1](https://huggingface.co/InternScience/Agents-A1)**  
  作者 InternScience · 👍 401 · ⬇️ 14,723  
  多模态 Agent 模型，结合视觉与文本生成，可执行工具调用，MoE 架构。
- **[bottlecapai/ThinkingCap-Qwen3.6-27B](https://huggingface.co/bottlecapai/ThinkingCap-Qwen3.6-27B)**  
  作者 bottlecapai · 👍 144 · ⬇️ 46  
  基于 Qwen3.6 的多模态思考模型，旨在增强图像理解与推理链。
- **[nvidia/LocateAnything-3B](https://huggingface.co/nvidia/LocateAnything-3B)**  
  作者 nvidia · 👍 2,668 · ⬇️ 1,424,958  
  NVIDIA 的通用视觉定位模型，仅 3B 参数实现开放词汇检测，实用性极强。
- **[empero-ai/Qwythos-9B-Claude-Mythos-5-1M](https://huggingface.co/empero-ai/Qwythos-9B-Claude-Mythos-5-1M)**  
  作者 empero-ai · 👍 737 · ⬇️ 152,516  
  结合 Qwen3.5 与 Claude 风格的混合多模态微调，支持 1M 上下文。
- **[krea/Krea-2-Turbo](https://huggingface.co/krea/Krea-2-Turbo)**  
  作者 krea · 👍 557 · ⬇️ 123,729  
  Krea 2 的 Turbo 版文本到图像模型，快速生成高质量图片。
- **[deepreinforce-ai/Ornith-1.0-9B](https://huggingface.co/deepreinforce-ai/Ornith-1.0-9B)**  
  作者 deepreinforce-ai · 👍 411 · ⬇️ 136,037  
  基于 Qwen3.5 的多模态微调模型，兼顾文本与图像输入，MIT 许可。
- **[nvidia/Nemotron-Labs-Audex-30B-A3B](https://huggingface.co/nvidia/Nemotron-Labs-Audex-30B-A3B)**  
  作者 nvidia · 👍 70 · ⬇️ 0  
  NVIDIA 的音频-文本生成模型，探索语音交互新范式，刚上架暂未产生下载。

#### 🔧 专用模型（代码、数学、医疗、嵌入）
- **[google/tabfm-1.0.0-pytorch](https://huggingface.co/google/tabfm-1.0.0-pytorch)**  
  作者 google · 👍 314 · ⬇️ 9,458  
  Google 的表格基础模型，零样本完成分类与回归，开辟表格学习新时代。
- **[poolside/Laguna-XS-2.1](https://huggingface.co/poolside/Laguna-XS-2.1)**  
  作者 poolside · 👍 80 · ⬇️ 3,385  
  Poolside 的轻量级代码生成模型 Laguna 系列，专为编程辅助设计。

#### 📦 微调与量化（社区微调、GGUF、AWQ）
- **[empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF](https://huggingface.co/empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF)**  
  作者 empero-ai · 👍 1,865 · ⬇️ 1,683,711  
  前述 Mythos 混合模型的 GGUF 量化版，适合本地部署与推理，下载火爆。
- **[deepreinforce-ai/Ornith-1.0-35B-GGUF](https://huggingface.co/deepreinforce-ai/Ornith-1.0-35B-GGUF)**  
  作者 deepreinforce-ai · 👍 803 · ⬇️ 502,663  
  Ornith 35B 的 GGUF 格式，大参数多模态模型本地化首选。
- **[yuxinlu1/gemma-4-12B-agentic-fable5-composer2.5-v2-3.5x-tau2-GGUF](https://huggingface.co/yuxinlu1/gemma-4-12B-agentic-fable5-composer2.5-v2-3.5x-tau2-GGUF)**  
  作者 yuxinlu1 · 👍 1,102 · ⬇️ 384,383  
  Gemma 4 的 Agent 微调版本，集成编程与终端能力，GGUF 方便消费级硬件运行。
- **[HauhauCS/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive](https://huggingface.co/HauhauCS/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive)**  
  作者 HauhauCS · 👍 2,577 · ⬇️ 2,823,988  
  无审查、多模态 MoE 的 Qwen3.6 量化版，下载量断层领先，极受社区欢迎。
- **[froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates)**  
  作者 froggeric · 👍 783 · ⬇️ 0  
  修复 Qwen 系列对话模板的 MLX/Jinja 工具，改善多模型兼容性。
- **[nvidia/Qwen3.6-27B-NVFP4](https://huggingface.co/nvidia/Qwen3.6-27B-NVFP4)**  
  作者 nvidia · 👍 326 · ⬇️ 538,687  
  NVIDIA 官方发布的 Qwen3.6 4 位浮点量化模型，采用 NVFP4 新格式。
- **[conradlocke/krea2-identity-edit](https://huggingface.co/conradlocke/krea2-identity-edit)**  
  作者 conradlocke · 👍 102 · ⬇️ 0  
  Krea-2 的风格保持 & 身份编辑 LoRA，适配 ComfyUI。
- **[eric-venti-seeds/Sun-Direction-Lora-Flux2Klein9B](https://huggingface.co/eric-venti-seeds/Sun-Direction-Lora-Flux2Klein9B)**  
  作者 eric-venti-seeds · 👍 110 · ⬇️ 0  
  Flux2Klein 的光照方向控制 LoRA，用于图像到图像的光影调整。
- **[unsloth/DeepSeek-V4-Flash-GGUF](https://huggingface.co/unsloth/DeepSeek-V4-Flash-GGUF)**  
  作者 unsloth · 👍 99 · ⬇️ 47  
  DeepSeek-V4 的 GGUF 量化版，由 Unsloth 优化，刚刚发布。
- **[unsloth/Qwen3.6-27B-MTP-GGUF](https://huggingface.co/unsloth/Qwen3.6-27B-MTP-GGUF)**  
  作者 unsloth · 👍 1,016 · ⬇️ 2,842,118  
  Unsloth 量化的 Qwen3.6 多模态模型，下载量近 300 万，社区部署首选。
- **[deepreinforce-ai/Ornith-1.0-9B-GGUF](https://huggingface.co/deepreinforce-ai/Ornith-1.0-9B-GGUF)**  
  作者 deepreinforce-ai · 👍 462 · ⬇️ 454,944  
  Ornith 9B 的 GGUF 量化，轻量多模态模型，下载活跃。
- **[yuxinlu1/gemma-4-12B-coder-fable5-composer2.5-v1-GGUF](https://huggingface.co/yuxinlu1/gemma-4-12B-coder-fable5-composer2.5-v1-GGUF)**  
  作者 yuxinlu1 · 👍 2,655 · ⬇️ 674,977  
  Gemma 4 编程增强版的 GGUF，专注于代码与推理，点赞数极高。
- **[InternScience/Agents-A1-Q4_K_M-GGUF](https://huggingface.co/InternScience/Agents-A1-Q4_K_M-GGUF)**  
  作者 InternScience · 👍 85 · ⬇️ 11,226  
  Agents-A1 视觉 Agent 的 GGUF 量化，适合端侧多模态任务。
- **[Patil/Krea-2-depth-controlnet](https://huggingface.co/Patil/Krea-2-depth-controlnet)**  
  作者 Patil · 👍 74 · ⬇️ 0  
  基于 Krea-2 的深度 ControlNet，可精细控制生成图像的几何结构。

---

### 🌍 生态信号
**Qwen 家族（3.5/3.6）** 已成为社区二次开发的核心基座，通过 GGUF、无审查微调、MTP 量化以及多模态扩展，形成了庞大的派生模型网络，单个变体下载量已突破 280 万。**DeepSeek-V4** 和 **Gemma 4** 紧随其后，在编程智能与 Agent 微调上分别占据一席之地。所有热门模型均为**开源权重**，闭源趋势几无踪迹，本地化推理需求空前高涨。量化方式中，**GGUF 格式依旧统治边缘侧**，而 NVIDIA 的 **NVFP4** 开始崭露头角，可能带来硬件级推理加速。同时，围绕 Krea、Flux 等图像模型的 **LoRA/ControlNet 微调**持续活跃，多模态 Agent 的量化也成为新热点。

---

### 🔍 值得探索
1. **[nvidia/LocateAnything-3B](https://huggingface.co/nvidia/LocateAnything-3B)**  
   仅 3B 参数即具备强大的开放词汇视觉定位能力，下载量超 140 万，非常适合需要轻量级检测与定位的终端场景，实用性居本周之冠。
2. **[zai-org/GLM-5.2](https://huggingface.co/zai-org/GLM-5.2)**  
   本周全榜点赞最高，由智谱推出的 MoE 对话模型，显著的中文社区热度反映出其中文理解与对话能力的突破，值得第一时间评估。
3. **[google/tabfm-1.0.0-pytorch](https://huggingface.co/google/tabfm-1.0.0-pytorch)**  
   谷歌推出的首个表格基础模型，零样本即可处理分类与回归，为表格数据学习打开了全新范式，对于数据科学家和自动化流程极具潜力。

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*