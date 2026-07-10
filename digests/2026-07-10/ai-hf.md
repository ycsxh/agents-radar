# Hugging Face 热门模型日报 2026-07-10

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-07-10 03:56 UTC

---

## Hugging Face 热门模型日报  
**日期：2026-07-10**

---

### 今日速览
本周 Hugging Face 热门榜呈现“基础模型亮剑、多模态齐飞、社区量化爆发”的态势。**智谱 GLM-5.2** 以 3.7k 点赞夺魁，验证中文对话模型的持续高需求；**NVIDIA LocateAnything-3B** 与 **百度 Unlimited-OCR** 分别以视觉定位和全场景文字识别闯入多模态赛道，双双突破百万下载。Qwen 家族演化出“Qwen3.5/3.6”两个分支，未审查微调版 **HauhauCS/Qwen3.6-35B-A3B-Uncensored** 下载量高达 270 万，边缘社区的热情可见一斑。此外，DeepSeek V4、Gemma 4 编程微调以及 Krea-2 图像生态开始接力，显示模型迭代进入“一超多强”的新周期。

---

### 热门模型

#### 🧠 语言模型
- [zai-org/GLM-5.2](https://huggingface.co/zai-org/GLM-5.2)  
  作者：zai-org | 👍 3,732 | ⬇️ 362,300  
  基于 GLM MoE 架构的新一代对话模型，支持超长文本和多轮对话，本周点赞最高。

- [tencent/Hy3](https://huggingface.co/tencent/Hy3)  
  作者：tencent | 👍 617 | ⬇️ 5,572  
  腾讯混元大模型第三代，专注文本生成，标记为 `hy_v3`、`hunyuan`。

- [deepseek-ai/DeepSeek-V4-Pro-DSpark](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-DSpark)  
  作者：deepseek-ai | 👍 459 | ⬇️ 29,230  
  DeepSeek V4 的专业增强版，推理与编程能力再次跃升，附带 ArXiv 论文参考。

- [meituan-longcat/LongCat-2.0](https://huggingface.co/meituan-longcat/LongCat-2.0)  
  作者：meituan-longcat | 👍 166 | ⬇️ 1,107  
  美团长猫对话模型第二代，强调超长上下文与连贯性。

- [mistralai/Leanstral-1.5-119B-A6B](https://huggingface.co/mistralai/Leanstral-1.5-119B-A6B)  
  作者：mistralai | 👍 179 | ⬇️ 258  
  Mistral 精简版 119B 大模型，配合 vLLM 高效推理，MIT 协议。

#### 🎨 多模态与生成
- [nvidia/LocateAnything-3B](https://huggingface.co/nvidia/LocateAnything-3B)  
  作者：nvidia | 👍 2,688 | ⬇️ 1,447,244  
  3B 参数的通用物体检测与指代表达理解模型，在定位任务中小体积大能力。

- [baidu/Unlimited-OCR](https://huggingface.co/baidu/Unlimited-OCR)  
  作者：baidu | 👍 1,904 | ⬇️ 1,246,042  
  全能光学字符识别模型，覆盖各类场景，可作为特征提取器使用。

- [InternScience/Agents-A1](https://huggingface.co/InternScience/Agents-A1)  
  作者：InternScience | 👍 438 | ⬇️ 23,112  
  基于 Qwen3.5-MoE 的多模态智能体，可处理图文交错输入，打上 `image-text-to-text` 标签。

- [nvidia/Nemotron-Labs-Audex-30B-A3B](https://huggingface.co/nvidia/Nemotron-Labs-Audex-30B-A3B)  
  作者：nvidia | 👍 84 | ⬇️ 436  
  30B 参数的音频文本理解模型，专攻听觉内容生成与分析。

- [krea/Krea-2-Turbo](https://huggingface.co/krea/Krea-2-Turbo)  
  作者：krea | 👍 570 | ⬇️ 157,302  
  Krea-2 文生图模型的加速版本，基于扩散架构。

- [open-gigaai/Giga-World-1](https://huggingface.co/open-gigaai/Giga-World-1)  
  作者：open-gigaai | 👍 98 | ⬇️ 0  
  世界模型，或用于生成交互式 3D/视频场景（Diffusers 格式）。

- [OpenMOSS-Team/MOSS-Transcribe-Diarize](https://huggingface.co/OpenMOSS-Team/MOSS-Transcribe-Diarize)  
  作者：OpenMOSS-Team | 👍 75 | ⬇️ 2,537  
  MOSS 系列音频转写与说话人分离模型，填补开源语音管线。

#### 🔧 专用模型
- [google/tabfm-1.0.0-pytorch](https://huggingface.co/google/tabfm-1.0.0-pytorch)  
  作者：google | 👍 333 | ⬇️ 16,374  
  表格基座模型，零样本完成表格分类与回归，领域专用。

- [froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates)  
  作者：froggeric | 👍 822 | ⬇️ 0  
  修复 Qwen 系列聊天模板的 Jinja 配置，确保多轮对话格式正确，属于工具类发布。

#### 📦 微调与量化
- [HauhauCS/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive](https://huggingface.co/HauhauCS/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive)  
  作者：HauhauCS | 👍 2,599 | ⬇️ 2,716,428  
  未审查版 Qwen3.6 MoE 多模态 GGUF，风格激进，下载量登顶。

- [yuxinlu1/gemma-4-12B-coder-fable5-composer2.5-v1-GGUF](https://huggingface.co/yuxinlu1/gemma-4-12B-coder-fable5-composer2.5-v1-GGUF)  
  作者：yuxinlu1 | 👍 2,671 | ⬇️ 703,735  
  Gemma 4 编程微调 GGUF，专注代码与推理。

- [empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF](https://huggingface.co/empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF)  
  作者：empero-ai | 👍 1,938 | ⬇️ 1,875,602  
  基于 Qwen3.5 的神话故事多模态生成模型 GGUF 版，社区二创热度高。

- [unsloth/Qwen3.6-27B-MTP-GGUF](https://huggingface.co/unsloth/Qwen3.6-27B-MTP-GGUF)  
  作者：unsloth | 👍 1,025 | ⬇️ 2,894,918  
  Qwen3.6 的多 token 预测量化版，支持图像输入，下载量惊人。

- [nvidia/Qwen3.6-27B-NVFP4](https://huggingface.co/nvidia/Qwen3.6-27B-NVFP4)  
  作者：nvidia | 👍 333 | ⬇️ 748,054  
  NVIDIA 低精度 NVFP4 量化 Qwen3.6，适配 GPU Tensor Core 高效推理。

- [deepreinforce-ai/Ornith-1.0-35B-GGUF](https://huggingface.co/deepreinforce-ai/Ornith-1.0-35B-GGUF)  
  作者：deepreinforce-ai | 👍 821 | ⬇️ 957,721  
  35B 文本模型的 GGUF 版本，MIT 协议，兼容常见推理端点。

- [yuxinlu1/gemma-4-12B-agentic-fable5-composer2.5-v2-3.5x-tau2-GGUF](https://huggingface.co/yuxinlu1/gemma-4-12B-agentic-fable5-composer2.5-v2-3.5x-tau2-GGUF)  
  作者：yuxinlu1 | 👍 1,119 | ⬇️ 418,171  
  Gemma 4 智能体微调 GGUF，面向终端任务和工具调用。

- [unsloth/DeepSeek-V4-Flash-GGUF](https://huggingface.co/unsloth/DeepSeek-V4-Flash-GGUF)  
  作者：unsloth | 👍 112 | ⬇️ 22,953  
  DeepSeek V4 Flash 的社区 GGUF 量化，方便本地部署。

- [AliesTaha/fable-traces](https://huggingface.co/AliesTaha/fable-traces)  
  作者：AliesTaha | 👍 197 | ⬇️ 4,647  
  Qwen3 指令微调，专注寓言故事创作。

- [bottlecapai/ThinkingCap-Qwen3.6-27B](https://huggingface.co/bottlecapai/ThinkingCap-Qwen3.6-27B)  
  作者：bottlecapai | 👍 189 | ⬇️ 2,189  
  “思考帽”版 Qwen3.6，针对图像文本推理场景微调。

- [GnLOLot/MiniCPM5-1B-Claude-Opus-Fable5-Thinking-GGUF](https://huggingface.co/GnLOLot/MiniCPM5-1B-Claude-Opus-Fable5-Thinking-GGUF)  
  作者：GnLOLot | 👍 143 | ⬇️ 2,239  
  极小的 MiniCPM5 思维链微调 GGUF，探索边缘侧推理。

- [conradlocke/krea2-identity-edit](https://huggingface.co/conradlocke/krea2-identity-edit)（LoRA）  
  作者：conradlocke | 👍 139 | ⬇️ 0  
  用于 Krea-2 图像身份编辑的 LoRA，可搭配 ComfyUI。

- [eric-venti-seeds/Sun-Direction-Lora-Flux2Klein9B](https://huggingface.co/eric-venti-seeds/Sun-Direction-Lora-Flux2Klein9B)（LoRA）  
  作者：eric-venti-seeds | 👍 124 | ⬇️ 0  
  控制光线方向的 Flux2Klein 9B LoRA，用于图像风格化。

- [Patil/Krea-2-depth-controlnet](https://huggingface.co/Patil/Krea-2-depth-controlnet)（ControlNet）  
  作者：Patil | 👍 83 | ⬇️ 0  
  Krea-2 深度控制网络，增强生成图像的结构一致性。

---

### 生态信号
**Qwen 家族占据半壁江山，DeepSeek 与 Gemma 追赶**：Qwen3.5/3.6 基础模型及其微调、量化变体合计超过 10 个上榜，从官方 NVFP4 量化到未审查社区版，显示出极强派生能力。**开源全栈化趋势明显**：所有上榜模型权重完全开放，结合 GGUF、NVFP4、LoRA 等工具链，开发者已能构建从训练到低资源推理的完整闭环。**量化和微调成为社区刚需**：超过一半的模型为二次分发，GGUF 格式尤其突出；unsloth 和 NVIDIA 的低精度方案（NVFP4）正推动硬件亲和性。此外，**图像生成生态初现**：Krea-2 的 Turbo 版、ControlNet、LoRA 相继出现，类似 Stable Diffusion 早期的社区协作模式正在重演。

---

### 值得探索
1. **[zai-org/GLM-5.2](https://huggingface.co/zai-org/GLM-5.2)**  
   本周点赞王，智谱新一代 MoE 对话模型，或为目前开源中文能力天花板，值得评估其长文本和推理能力。

2. **[nvidia/LocateAnything-3B](https://huggingface.co/nvidia/LocateAnything-3B)**  
   仅 3B 参数即实现强大的视觉定位与指代表达，适合端侧和实时应用，开源后立刻收获百万下载，扩展了多模态小模型的边界。

3. **[unsloth/Qwen3.6-27B-MTP-GGUF](https://huggingface.co/unsloth/Qwen3.6-27B-MTP-GGUF)**  
   近 290 万下载的量化模型，多 token 预测可加速生成，结合 GGUF 的本地部署友好性，是尝试 Qwen3.6 最便捷的入口。

---
*本日报由 [agents-radar](https://github.com/ycsxh/agents-radar) 自动生成。*