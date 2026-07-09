# Hugging Face Trending Models Digest 2026-07-09

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-07-09 03:55 UTC

---

### Today’s Highlights  
The Hugging Face Hub is buzzing with fresh open-weight LLMs and a wave of community quantization. Tencent’s **Hy3**, Zhipu’s **GLM-5.2**, Mistral’s **Leanstral-1.5-119B-A6B**, and DeepSeek’s **V4-Pro-DSpark** each debuted powerful MoE-based text generators. Multimodal momentum is equally strong: Baidu’s **Unlimited-OCR** and NVIDIA’s **LocateAnything-3B** bring production‑grade vision‑language capabilities, while Krea‑2 derivatives and LoRA fine‑tunes fuel the image‑generation front. The GGUF ecosystem remains incredibly active, with dozens of quantized versions of Qwen3.5/3.6, Gemma 4, and DeepSeek V4 models dominating downloads – a clear signal that local, edge‑deployable inference is a top community priority.

---

### Trending Models

#### 🧠 Language Models (LLMs)
- **[tencent/Hy3](https://huggingface.co/tencent/Hy3)**  
  *tencent* · 567 likes · 121 downloads  
  A new Hunyuan-series text-generation LLM that marks Tencent’s latest open‑weight entry, drawing attention as a potential GPT‑class contender.

- **[zai-org/GLM-5.2](https://huggingface.co/zai-org/GLM-5.2)**  
  *zai-org* · 3,672 likes · 281,584 downloads  
  A conversational MoE (Mixture of Experts) model from Zhipu AI with DSA architecture; trending for its strong multi‑turn dialogue and massive early interest.

- **[mistralai/Leanstral-1.5-119B-A6B](https://huggingface.co/mistralai/Leanstral-1.5-119B-A6B)**  
  *mistralai* · 168 likes · 157 downloads  
  Mistral’s latest 119B‑parameter MoE model (6B active), licensed Apache 2.0, signaling a continued push toward efficient large‑scale open models.

- **[meituan-longcat/LongCat-2.0](https://huggingface.co/meituan-longcat/LongCat-2.0)**  
  *meituan-longcat* · 153 likes · 385 downloads  
  Meituan’s conversational LongCat-2.0 model, marking the company’s expanding presence in open‑source LLMs.

- **[poolside/Laguna-XS-2.1](https://huggingface.co/poolside/Laguna-XS-2.1)**  
  *poolside* · 80 likes · 3,385 downloads  
  A compact text-generation model from Poolside’s Laguna family, aimed at code-oriented tasks and fast local inference.

- **[deepseek-ai/DeepSeek-V4-Pro-DSpark](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-DSpark)**  
  *deepseek-ai* · 439 likes · 15,538 downloads  
  DeepSeek V4’s Pro-DSpark variant, extending the V4 series with enhanced reasoning; referenced in a research preprint (arXiv:2606.19348).

- **[nvidia/Nemotron-Labs-Audex-30B-A3B](https://huggingface.co/nvidia/Nemotron-Labs-Audex-30B-A3B)**  
  *nvidia* · 70 likes · 0 downloads  
  A 30B‑parameter Nemotron‑Labs model from NVIDIA, likely targeting advanced text‑based audio/codec tasks (just released, zero downloads yet).

#### 🎨 Multimodal & Generation
- **[baidu/Unlimited-OCR](https://huggingface.co/baidu/Unlimited-OCR)**  
  *baidu* · 1,876 likes · 1,084,945 downloads  
  A high‑accuracy OCR (optical character recognition) model from Baidu with image‑to‑text pipeline, trending for its strong out‑of‑the‑box extraction.

- **[InternScience/Agents-A1](https://huggingface.co/InternScience/Agents-A1)**  
  *InternScience* · 401 likes · 14,723 downloads  
  A Qwen3.5‑based MoE vision‑language model designed for agentic tasks; attracts interest for its multimodal agentic capabilities.

- **[nvidia/LocateAnything-3B](https://huggingface.co/nvidia/LocateAnything-3B)**  
  *nvidia* · 2,668 likes · 1,424,958 downloads  
  An open‑vocabulary object localization model with image‑to‑text output; trending for its flexibility and large download volume.

- **[krea/Krea-2-Turbo](https://huggingface.co/krea/Krea-2-Turbo)**  
  *krea* · 557 likes · 123,729 downloads  
  The accelerated version of Krea’s text‑to‑image model, delivering fast high‑quality generation for creatives.

- **[conradlocke/krea2-identity-edit](https://huggingface.co/conradlocke/krea2-identity-edit)**  
  *conradlocke* · 102 likes · 0 downloads  
  A LoRA‑based identity‑editing plugin for Krea‑2, enabling consistent character editing in ComfyUI workflows.

- **[eric-venti-seeds/Sun-Direction-Lora-Flux2Klein9B](https://huggingface.co/eric-venti-seeds/Sun-Direction-Lora-Flux2Klein9B)**  
  *eric-venti-seeds* · 110 likes · 0 downloads  
  An image‑to‑image LoRA for controlling sunlight direction in Flux2Klein, popular for lighting‑aware scene generation.

- **[Patil/Krea-2-depth-controlnet](https://huggingface.co/Patil/Krea-2-depth-controlnet)**  
  *Patil* · 74 likes · 0 downloads  
  A depth‑aware ControlNet for Krea‑2, enabling image‑to‑image generation with spatial depth guidance using flow matching.

#### 🔧 Specialized Models
- **[google/tabfm-1.0.0-pytorch](https://huggingface.co/google/tabfm-1.0.0-pytorch)**  
  *google* · 314 likes · 9,458 downloads  
  A foundation model for tabular data (classification & regression) from Google, bringing zero‑shot capabilities to structured data tasks.

- **[froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates)**  
  *froggeric* · 783 likes · 0 downloads  
  A set of corrected Jinja chat templates for Qwen3.5/3.6 models; not a model but a community fix that’s highly appreciated for proper prompt formatting.

#### 📦 Fine-tunes & Quantizations
- **[empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF](https://huggingface.co/empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF)**  
  *empero-ai* · 1,865 likes · 1,683,711 downloads  
  GGUF‑quantized version of a Qwen3.5‑based reasoning fine‑tune; massive downloads driven by local inference enthusiasts.

- **[HauhauCS/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive](https://huggingface.co/HauhauCS/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive)**  
  *HauhauCS* · 2,577 likes · 2,823,988 downloads  
  A GGUF‑quantized, uncensored MoE vision‑language model; trending for its unfiltered outputs and enormous download count.

- **[yuxinlu1/gemma-4-12B-coder-fable5-composer2.5-v1-GGUF](https://huggingface.co/yuxinlu1/gemma-4-12B-coder-fable5-composer2.5-v1-GGUF)**  
  *yuxinlu1* · 2,655 likes · 674,977 downloads  
  GGUF quantization of a Gemma‑4 coding fine‑tune; highly popular among developers for local code generation and reasoning.

- **[yuxinlu1/gemma-4-12B-agentic-fable5-composer2.5-v2-3.5x-tau2-GGUF](https://huggingface.co/yuxinlu1/gemma-4-12B-agentic-fable5-composer2.5-v2-3.5x-tau2-GGUF)**  
  *yuxinlu1* · 1,102 likes · 384,383 downloads  
  Another Gemma‑4 GGUF, this time tuned for agentic/terminal tasks, reflecting growing demand for AI‑driven command‑line agents.

- **[unsloth/Qwen3.6-27B-MTP-GGUF](https://huggingface.co/unsloth/Qwen3.6-27B-MTP-GGUF)**  
  *unsloth* · 1,016 likes · 2,842,118 downloads  
  Unsloth’s GGUF of Qwen3.6‑27B with MTP (Multi‑Token Prediction), a go‑to for fast, accurate local text inference.

- **[unsloth/DeepSeek-V4-Flash-GGUF](https://huggingface.co/unsloth/DeepSeek-V4-Flash-GGUF)**  
  *unsloth* · 99 likes · 47 downloads  
  Flash‑optimized GGUF of DeepSeek V4; early adoption with low downloads but notable interest for speed‑focused deployments.

- **[deepreinforce-ai/Ornith-1.0-35B-GGUF](https://huggingface.co/deepreinforce-ai/Ornith-1.0-35B-GGUF)** & **[Ornith-1.0-9B-GGUF](https://huggingface.co/deepreinforce-ai/Ornith-1.0-9B-GGUF)**  
  *deepreinforce-ai* · 803/462 likes · 502k/454k downloads  
  Two GGUF quantizations of the Ornith fine‑tune, offering MIT‑licensed local LLMs at different scales.

- **[empero-ai/Qwythos-9B-Claude-Mythos-5-1M](https://huggingface.co/empero-ai/Qwythos-9B-Claude-Mythos-5-1M)** (safetensors) · 737 likes, 152k downloads – the original reasoning fine‑tune before quantization.
- **[AliesTaha/fable-traces](https://huggingface.co/AliesTaha/fable-traces)** · 188 likes, 3.8k downloads – a Qwen3 instruct fine‑tune with storytelling traces.
- **[bottlecapai/ThinkingCap-Qwen3.6-27B](https://huggingface.co/bottlecapai/ThinkingCap-Qwen3.6-27B)** · 144 likes, 46 downloads – vision‑language fine‑tune adding “thinking cap” behaviour.
- **[deepreinforce-ai/Ornith-1.0-9B](https://huggingface.co/deepreinforce-ai/Ornith-1.0-9B)** · 411 likes, 136k downloads – MIT‑licensed base Ornith fine‑tune (image‑text‑to‑text capable).
- **[nvidia/Qwen3.6-27B-NVFP4](https://huggingface.co/nvidia/Qwen3.6-27B-NVFP4)** · 326 likes, 538k downloads – NVIDIA‑quantized Qwen3.6‑27B in FP4 precision for efficient deployment.
- **[InternScience/Agents-A1-Q4_K_M-GGUF](https://huggingface.co/InternScience/Agents-A1-Q4_K_M-GGUF)** · 85 likes, 11k downloads – GGUF of the Agents-A1 VLM for on‑device agentic use.

---

### Ecosystem Signal  
The Hugging Face landscape in July 2026 is dominated by **Qwen3.5/3.6 and Gemma 4** lineages, with DeepSeek V4 also gaining rapid community traction. Nearly every popular foundation model is immediately mirrored by multiple GGUF quantizations—often within days—driven by unsloth, yuxinlu1, and deepreinforce-ai, making local LLM inference more accessible than ever. The Qwen family, in particular, sees extensive fine‑tuning for uncensored, agentic, coding, and “aggressive” variants, reflecting user demand for customised, unfiltered experiences.  

Open‑weight releases remain fiercely competitive: Tencent, Zhipu, Mistral, Meituan, and NVIDIA all publish fresh models, ensuring a diverse, multi‑stakeholder ecosystem. Proprietary gaps are narrower, as open models now routinely match or surpass earlier closed models in reasoning and multimodal tasks. The surge of tabular (TabFM) and OCR models (Unlimited-OCR) signals that foundation models are branching into structured data and document understanding, beyond conventional text and images. Quantization itself is no longer just for inference—NVFP4 and MTP optimisations from NVIDIA and unsloth hint at training‑time acceleration and co‑design of model architectures with compression. In summary, the ecosystem is shifting from “one big model” to a mesh of specialised, quantised, and fine‑tuned open‑weight components that can be composed locally.

### Worth Exploring  
- **[zai-org/GLM-5.2](https://huggingface.co/zai-org/GLM-5.2)** — With 3,672 likes in a short span, this MoE‑based conversational model from Zhipu AI is a top‑tier open‑source alternative for interactive agents. Its DSA architecture and strong multi‑turn performance make it a compelling candidate for chat and voice‑assistant applications.

- **[nvidia/LocateAnything-3B](https://huggingface.co/nvidia/LocateAnything-3B)** — An open‑vocabulary object localisation model that packs state‑of‑the‑art detection into just 3B parameters. Its 1.4 million downloads show it’s already a go‑to for visual grounding, robotics, and document understanding pipelines.

- **[google/tabfm-1.0.0-pytorch](https://huggingface.co/google/tabfm-1.0.0-pytorch)** — A pioneering foundation model for tabular data that zero‑shot classifies and regresses across varied structured datasets. It opens a new frontier for automating data science tasks and integrating structured data into multimodal AI stacks.

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*