# Hugging Face Trending Models Digest 2026-07-10

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-07-10 03:56 UTC

---

**Hugging Face Trending Models Digest – 2026-07-10**

---

### 1. Today’s Highlights
Community quantization continues to dominate, with GGUF variants of Qwen3.6 effortlessly pulling millions of downloads and proving that local, resource-efficient multimodal inference is the new normal. DeepSeek V4 enters the spotlight through Pro and Flash versions, while new LLMs from Tencent (Hy3) and Zhipu (GLM-5.2) underscore the momentum of Chinese MoE architectures. Krea‑2’s text‑to‑image ecosystem expands rapidly with Turbo, LoRAs and ControlNets, mirroring earlier Flux‑era creativity. Nvidia adds a strong signal with NVFP4 quantization and specialised vision (LocateAnything) and audio (Audex) models, reinforcing the move toward efficient, task‑specific open‑weight releases.

---

### 2. Trending Models

#### 🧠 Language Models
- **[tencent/Hy3](https://huggingface.co/tencent/Hy3)**  
  *Tencent · 617 likes · 5,572 downloads*  
  A fresh Hunyuan V3 text‑generation LLM, attracting early interest as a new open‑weight contender from Tencent.

- **[zai-org/GLM-5.2](https://huggingface.co/zai-org/GLM-5.2)**  
  *zai-org · 3,732 likes · 362,300 downloads*  
  Chat‑oriented MoE‑DSA model that combines conversational fluency with a dense‑sparse architecture, driving rapid adoption.

- **[mistralai/Leanstral-1.5-119B-A6B](https://huggingface.co/mistralai/Leanstral-1.5-119B-A6B)**  
  *mistralai · 179 likes · 258 downloads*  
  Mistral’s latest high‑capacity MoE model (119B total, 6B active) designed for efficient inference with vLLM, marking a follow‑up to the Leanstral line.

- **[meituan-longcat/LongCat-2.0](https://huggingface.co/meituan-longcat/LongCat-2.0)**  
  *meituan-longcat · 166 likes · 1,107 downloads*  
  A conversational text‑generation model from Meituan, pushing into the long‑context Chinese LLM space.

- **[deepseek-ai/DeepSeek-V4-Pro-DSpark](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-DSpark)**  
  *deepseek-ai · 459 likes · 29,230 downloads*  
  The “Pro” variant of DeepSeek V4, building on the popular series with enhanced reasoning and text‑generation capabilities.

#### 🎨 Multimodal & Generation
- **[InternScience/Agents-A1](https://huggingface.co/InternScience/Agents-A1)**  
  *InternScience · 438 likes · 23,112 downloads*  
  An image‑text‑to‑text MoE model based on Qwen3.5‑MoE, optimised for agentic tasks with vision input.

- **[bottlecapai/ThinkingCap-Qwen3.6-27B](https://huggingface.co/bottlecapai/ThinkingCap-Qwen3.6-27B)**  
  *bottlecapai · 189 likes · 2,189 downloads*  
  Multimodal Qwen3.6‑based model with thinking traces, blending vision understanding with extended reasoning.

- **[nvidia/LocateAnything-3B](https://huggingface.co/nvidia/LocateAnything-3B)**  
  *nvidia · 2,688 likes · 1,447,244 downloads*  
  A small but highly capable vision‑language model for visual grounding and object localisation, trending for its utility and efficiency.

- **[empero-ai/Qwythos-9B-Claude-Mythos-5-1M](https://huggingface.co/empero-ai/Qwythos-9B-Claude-Mythos-5-1M)**  
  *empero-ai · 749 likes · 179,378 downloads*  
  A community merge of Qwen3.5 with Claude‑style reasoning, offered as a multimodal (image‑text‑to‑text) model, driving interest in “mythos” fusions.

- **[krea/Krea-2-Turbo](https://huggingface.co/krea/Krea-2-Turbo)**  
  *krea · 570 likes · 157,302 downloads*  
  Turbo‑charged version of the Krea‑2 text‑to‑image model, delivering faster generation for creative workflows.

- **[conradlocke/krea2-identity-edit](https://huggingface.co/conradlocke/krea2-identity-edit)**  
  *conradlocke · 139 likes · 0 downloads*  
  LoRA adapter for identity‑aware image editing on Krea‑2, popular within the ComfyUI ecosystem.

- **[eric-venti-seeds/Sun-Direction-Lora-Flux2Klein9B](https://huggingface.co/eric-venti-seeds/Sun-Direction-Lora-Flux2Klein9B)**  
  *eric-venti-seeds · 124 likes · 0 downloads*  
  An image‑to‑image LoRA for controlling sun/light direction with Flux‑based models, a niche but highly shared creative tool.

- **[Patil/Krea-2-depth-controlnet](https://huggingface.co/Patil/Krea-2-depth-controlnet)**  
  *Patil · 83 likes · 0 downloads*  
  Depth ControlNet for Krea‑2, enabling structure‑aware image generation with flow‑matching.

- **[open-gigaai/Giga-World-1](https://huggingface.co/open-gigaai/Giga-World-1)**  
  *open-gigaai · 98 likes · 0 downloads*  
  A diffusers‑based model likely focused on world‑consistent video generation, stirring curiosity with its Apache‑2.0 license.

- **[nvidia/Nemotron-Labs-Audex-30B-A3B](https://huggingface.co/nvidia/Nemotron-Labs-Audex-30B-A3B)**  
  *nvidia · 84 likes · 436 downloads*  
  An audio‑language Nemotron model for speech understanding and generation, representing Nvidia’s push into voice‑enabled LLMs.

#### 🔧 Specialized Models
- **[google/tabfm-1.0.0-pytorch](https://huggingface.co/google/tabfm-1.0.0-pytorch)**  
  *google · 333 likes · 16,374 downloads*  
  A tabular foundation model for zero‑shot classification and regression, opening new paths for structured‑data tasks.

- **[baidu/Unlimited-OCR](https://huggingface.co/baidu/Unlimited-OCR)**  
  *baidu · 1,904 likes · 1,246,042 downloads*  
  High‑capacity OCR model that extracts text from diverse image inputs, earning substantial downloads from document‑processing workflows.

- **[OpenMOSS-Team/MOSS-Transcribe-Diarize](https://huggingface.co/OpenMOSS-Team/MOSS-Transcribe-Diarize)**  
  *OpenMOSS-Team · 75 likes · 2,537 downloads*  
  A dedicated audio‑text‑to‑text model for transcription and speaker diarization, filling a niche in open‑source speech pipelines.

#### 📦 Fine‑tunes & Quantizations
- **[empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF](https://huggingface.co/empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF)**  
  *empero-ai · 1,938 likes · 1,875,602 downloads*  
  Quantized GGUF of the popular Qwythos merge, massively downloaded for local Qwen3.5‑based multimodal reasoning.

- **[AliesTaha/fable-traces](https://huggingface.co/AliesTaha/fable-traces)**  
  *AliesTaha · 197 likes · 4,647 downloads*  
  Instruct fine‑tune on Qwen3 with “fable” style traces, catering to narrative and role‑play use cases.

- **[HauhauCS/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive](https://huggingface.co/HauhauCS/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive)**  
  *HauhauCS · 2,599 likes · 2,716,428 downloads*  
  Uncensored GGUF MoE‑vision model with record downloads, reflecting the persistent demand for unfiltered local assistants.

- **[deepreinforce-ai/Ornith-1.0-35B-GGUF](https://huggingface.co/deepreinforce-ai/Ornith-1.0-35B-GGUF)**  
  *deepreinforce-ai · 821 likes · 957,721 downloads*  
  GGUF quant of the Ornith model, MIT‑licensed and suitable for production endpoints, driving high adoption.

- **[yuxinlu1/gemma-4-12B-agentic-fable5-composer2.5-v2-3.5x-tau2-GGUF](https://huggingface.co/yuxinlu1/gemma-4-12B-agentic-fable5-composer2.5-v2-3.5x-tau2-GGUF)**  
  *yuxinlu1 · 1,119 likes · 418,171 downloads*  
  A coding‑agent fine‑tune of Gemma 4 in GGUF, combining Fable5 and Composer recipes for terminal‑based coding tasks.

- **[GnLOLot/MiniCPM5-1B-Claude-Opus-Fable5-Thinking-GGUF](https://huggingface.co/GnLOLot/MiniCPM5-1B-Claude-Opus-Fable5-Thinking-GGUF)**  
  *GnLOLot · 143 likes · 2,239 downloads*  
  Lightweight 1B‑parameter thinking model quantized, enabling chain‑of‑thought on edge devices with MiniCPM5.

- **[unsloth/DeepSeek-V4-Flash-GGUF](https://huggingface.co/unsloth/DeepSeek-V4-Flash-GGUF)**  
  *unsloth · 112 likes · 22,953 downloads*  
  Unsloth’s fast‑GGUF release of DeepSeek‑V4‑Flash, accelerating local use of the Flash variant.

- **[nvidia/Qwen3.6-27B-NVFP4](https://huggingface.co/nvidia/Qwen3.6-27B-NVFP4)**  
  *nvidia · 333 likes · 748,054 downloads*  
  Nvidia’s NVFP4‑quantized Qwen3.6, optimized for NVIDIA hardware with ModelOpt, gaining traction rapidly.

- **[nvidia/NVIDIA-Nemotron-Labs-3-Puzzle-75B-A9B-NVFP4](https://huggingface.co/nvidia/NVIDIA-Nemotron-Labs-3-Puzzle-75B-A9B-NVFP4)**  
  *nvidia · 88 likes · 16,959 downloads*  
  NVFP4 quant of a 75B Nemotron “Puzzle” variant, appealing for high‑efficiency deployment on supported GPUs.

- **[unsloth/Qwen3.6-27B-MTP-GGUF](https://huggingface.co/unsloth/Qwen3.6-27B-MTP-GGUF)**  
  *unsloth · 1,025 likes · 2,894,918 downloads*  
  The highest‑downloaded model this week: a GGUF multimodal Qwen3.6 with multi‑turn prompt support, tuned for real‑world local use.

- **[yuxinlu1/gemma-4-12B-coder-fable5-composer2.5-v1-GGUF](https://huggingface.co/yuxinlu1/gemma-4-12B-coder-fable5-composer2.5-v1-GGUF)**  
  *yuxinlu1 · 2,671 likes · 703,735 downloads*  
  A coding‑specialized GGUF of Gemma 4, widely liked and downloaded for code generation and reasoning tasks.

---

### 3. Ecosystem Signal
Qwen3.5/3.6 has become the backbone of the community’s multimodal fine‑tuning and quantization activity, driving more than 5 million weekly downloads across its GGUF variants alone. DeepSeek V4 and Gemma 4 follow suit, with a clear trend toward MoE architectures that balance quality and inference cost—both eagerly adopted in GGUF and NVFP4 formats. Nvidia’s NVFP4 emerges as a hardware‑specific alternative to GGUF, while unsloth maintains its position as the go‑to provider for well‑optimized GGUF builds.  

On the image side, Krea‑2 solidifies its role as a new open‑weight diffusion hub with turbo, LoRAs, and ControlNets, reminiscent of Flux’s early creative explosion. Outside LLMs, task‑specific models thrive: Baidu’s Unlimited‑OCR and Google’s TabFM indicate growing demand for domain‑specialized foundation models, while MOSS‑Transcribe‑Diarize extends open‑source speech‑processing capability. The ongoing popularity of “uncensored” merges and aggressive instruction tuning demonstrates a strong, persistent appetite for unrestricted, user‑controlled assistants, even as capabilities become more powerful.

---

### 4. Worth Exploring
- **[deepseek-ai/DeepSeek-V4-Pro-DSpark](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-DSpark)**  
  Likely the most advanced open‑weight text model this week; ideal for benchmarking LLM reasoning and exploring the DeepSeek V4 family’s latest improvements.

- **[nvidia/LocateAnything-3B](https://huggingface.co/nvidia/LocateAnything-3B)**  
  A small, efficient vision‑language model that excels at spatial understanding—perfect for prototyping visual‑grounding applications without heavy hardware constraints.

- **[unsloth/Qwen3.6-27B-MTP-GGUF](https://huggingface.co/unsloth/Qwen3.6-27B-MTP-GGUF)**  
  The week’s most downloaded model; an excellent on‑ramp for local multimodal chat with reasonable compute, representative of the quality and accessibility driving the GGUF ecosystem.

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*