# Hugging Face Trending Models Digest 2026-09-05

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-09-05 03:59 UTC

---

# Hugging Face Trending Models Digest — 2026-09-05

## Today's Highlights

The Sep 5 trending board is strongly defined by Qwen: **Qwen3.8-27B** and **Qwen3.8-Flash-Next** dominate both base-model interest and the community fine-tune/quantization ecosystem. Video generation is also in a major moment, with **MiniMax-H3** and **Lightricks LTX-2.5** generating millions of downloads and early derivatives already appearing. In the LLM arena, **GLM-5.3** and **GLM-5.3-Flash** give zai-org a strong one-two punch, while DeepSeek, Tencent, and IFM contribute new frontier/experimental checkpoints. Notably, the specialized-model section is led by time-series forecasting, sentence embeddings, and classic BERT/CLIP baselines rather than code or math models. Finally, the long tail of abliterated/uncensored GGUF variants centered on Qwen3.8 shows that open-weight models continue to fuel a fast-moving community-adaptation economy.

---

## Trending Models

### 🧠 Language Models

- **[GLM-5.3](https://huggingface.co/zai-org/GLM-5.3)** — zai-org · 1,706 likes · 303,534 downloads  
  Z.ai’s next-generation open-weight text-generation LLM, tagged as a MoE/DSA model and already serving as the anchor for Flash and FP8 derivatives.

- **[Spark-X2.5-4B](https://huggingface.co/XHToken/Spark-X2.5-4B)** — XHToken · 481 likes · 3,524 downloads  
  A compact 4B causal LLM from the Spark family, gaining attention as a lightweight alternative to the larger frontier models in this chart.

- **[Hy4-preview](https://huggingface.co/tencent/Hy4-preview)** — tencent · 437 likes · 5,684 downloads  
  Tencent’s Hunyuan-family text-generation preview, tagged `hy_v4`, signaling a new LLM iteration under the Hy4 name.

- **[K2-Horizon-MoVA-36B-A4B](https://huggingface.co/IFM/K2-Horizon-MoVA-36B-A4B)** — IFM · 156 likes · 433 downloads  
  A 36B-total / 4B-active MoE text-generation model, notable for its highly sparse, efficient inference profile.

- **[gpt2](https://huggingface.co/openai-community/gpt2)** — openai-community · 3,661 likes · 14,607,268 downloads  
  The classic open causal LM remains a durable ecosystem baseline; its continued popularity reflects ongoing evaluation, library testing, and small-scale fine-tuning use.

---

### 🎨 Multimodal & Generation

- **[DeepSeek-V4-Flash-Vision-Exp](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-Vision-Exp)** — deepseek-ai · 608 likes · 133,024 downloads  
  DeepSeek’s experimental V4 Flash vision-language model, an important signal for fast “Flash”-style multimodal LLMs.

- **[Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B)** — Qwen · 13,962 likes · 5,739,341 downloads  
  Qwen’s flagship 27B multimodal chat model and the base for the largest GGUF/fine-tune wave on this list.

- **[Qwen3.8-Flash-Next](https://huggingface.co/Qwen/Qwen3.8-Flash-Next)** — Qwen · 4,880 likes · 351,374 downloads  
  Qwen’s experimental “Flash-Next” image-text-to-text model, tagged `qwen4_exp`, making it one of the most interesting architectural previews this week.

- **[GLM-5.3-Flash](https://huggingface.co/zai-org/GLM-5.3-Flash)** — zai-org · 2,053 likes · 654,957 downloads  
  The fast, multimodal companion to GLM-5.3, extending the GLM line into image-text-to-text chat with strong community uptake.

- **[LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5)** — Lightricks · 2,795 likes · 1,399,511 downloads  
  A multi-format generation model supporting image-to-video, text-to-video, and video-to-video use cases.

- **[MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3)** — MiniMaxAI · 4,912 likes · 5,118,457 downloads  
  A highly popular text/image-to-video model that has become the center of gravity for community video fine-tunes.

- **[Breeze-TTS-2](https://huggingface.co/BreezeBlue/Breeze-TTS-2)** — BreezeBlue · 434 likes · 5,388 downloads  
  A newer text-to-speech model from BreezeBlue, standing out as the main dedicated TTS release in the trending set.

---

### 🔧 Specialized Models

- **[timesfm-3.0-pytorch](https://huggingface.co/google/timesfm-3.0-pytorch)** — google · 432 likes · 105,304 downloads  
  Google’s time-series forecasting foundation model; its presence highlights growing interest in specialized predictive models beyond language.

- **[all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2)** — sentence-transformers · 5,519 likes · 253,789,790 downloads  
  One of the most-used sentence embedding models ever, consistently present because of its central role in RAG and semantic-search pipelines.

- **[mms-300m](https://huggingface.co/facebook/mms-300m)** — facebook · 237 likes · 12,823 downloads  
  Facebook’s 300M multilingual speech model based on wav2vec2, offering a strong open-weight baseline for speech processing.

- **[bert-base-uncased](https://huggingface.co/google-bert/bert-base-uncased)** — google-bert · 2,951 likes · 58,675,189 downloads  
  The classic BERT encoder remains an essential transfer-learning and evaluation baseline for NLP practitioners.

- **[distilbert-base-uncased](https://huggingface.co/distilbert/distilbert-base-uncased)** — distilbert · 1,133 likes · 7,067,963 downloads  
  A distilled version of BERT for efficient NLP tasks, showing that lightweight encoders remain relevant in production.

- **[clip-vit-base-patch32](https://huggingface.co/openai/clip-vit-base-patch32)** — openai · 1,185 likes · 20,569,141 downloads  
  The standard open CLIP model for zero-shot image classification and vision-language embedding tasks.

---

### 📦 Fine-tunes & Quantizations

- **[Qwen3.8-27B-GSQ-RCO-GGUF](https://huggingface.co/ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF)** — ISTA-DASLab · 315 likes · 206,575 downloads  
  Academic mixed-precision GGUF quantization using GSQ-RCO, an important testbed for more efficient Qwen3.8 deployment.

- **[Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF)** — unsloth · 3,514 likes · 9,951,693 downloads  
  The default unsloth GGUF release for Qwen3.8-27B, and one of the most-downloaded entries on the entire chart.

- **[Qwen3.8-Flash-Next-GGUF](https://huggingface.co/unsloth/Qwen3.8-Flash-Next-GGUF)** — unsloth · 788 likes · 702,251 downloads  
  Unsloth’s GGUF quantization for the experimental Qwen3.8-Flash-Next model, enabling fast local inference soon after release.

- **[Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF)** — HauhauCS · 949 likes · 1,463,966 downloads  
  A community Qwen3.8-27B GGUF with an “aggressive” style and MTP support, representative of the uncensored fine-tune trend.

- **[Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF)** — DavidAU · 184 likes · 95,226 downloads  
  A long-tail community merge/fine-tune of Qwen3.8-27B aimed at uncensored coding and role-play-style use cases.

- **[Qwen3.8-27B-OBLITERATED](https://huggingface.co/OBLITERATUS/Qwen3.8-27B-OBLITERATED)** — OBLITERATUS · 1,090 likes · 928,393 downloads  
  An abliterated Qwen3.8-27B checkpoint offered in MLX, safetensors, and GGUF formats.

- **[GLM-5.3-Flash-Uncensored-FP8](https://huggingface.co/orcarouter/GLM-5.3-Flash-Uncensored-FP8)** — orcarouter · 183 likes · 7,782 downloads  
  An FP8, abliterated/uncensored variant of GLM-5.3-Flash, showing that the uncensored-derivative pattern extends beyond Qwen.

- **[Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-GGUF)** — orcarouter · 717 likes · 276,706 downloads  
  Another popular abliterated Qwen3.8 GGUF release, optimized for local “uncensored” conversational use.

- **[Qwen3.8-Flash-Next-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF)** — orcarouter · 232 likes · 97,994 downloads  
  The Flash-Next counterpart in orcarouter’s abliterated GGUF family, bringing the uncensored treatment to Qwen’s experimental model.

- **[Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/JonathanColetti/Qwen3.8-27B-Uncensored-GGUF)** — JonathanColetti · 971 likes · 2,395,758 downloads  
  One of the most-downloaded uncensored Qwen3.8 GGUF variants, with llama.cpp and MTP support.

- **[FastVideo-FastH3-4-step-Preview-v1-VSA-DataFree](https://huggingface.co/FastVideo/FastVideo-FastH3-4-step-Preview-v1-VSA-DataFree)** — FastVideo · 277 likes · 0 downloads  
  A preview text-to-video model focused on 4-step, data-free acceleration of the FastH3 video-generation approach.

- **[vdn-minimax-h3](https://huggingface.co/OpenVDN/vdn-minimax-h3)** — OpenVDN · 175 likes · 0 downloads  
  A community text-to-video fine-tune of MiniMax-H3, currently showing zero downloads but signaling early third-party video-model adaptation.

---

## Ecosystem Signal

The dominant signal is that **open-weight Qwen3.8 has become an ecosystem in itself**. The official 27B model and Flash-Next variant generated immediate GGUF releases from unsloth, research quantizations from ISTA-DASLab, and a wave of abliterated/uncensored community checkpoints from orcarouter, OBLITERATUS, HauhauCS, JonathanColetti, and DavidAU. GLM-5.3 is following the same playbook: base release, Flash derivative, then community FP8/abliterated spins.

Video generation is a second major center of gravity. **MiniMax-H3** is the highest-download video model on the list, while FastVideo and OpenVDN previews indicate that people are already exploring distillation, fine-tuning, and custom pipelines around it. Lightricks LTX-2.5 also remains highly sticky for multi-format video generation.

Classic models — MiniLM, BERT, DistilBERT, GPT-2, CLIP — continue to show enormous cumulative usage and steady interest, reinforcing that embeddings and baseline encoders are still core infrastructure. Overall, the ecosystem is increasingly open-weight, quantization-first, and community-fine-tune-driven.

---

## Worth Exploring

1. **[Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B)**  
   The center of gravity for this week’s ecosystem: a powerful multimodal chat model whose base weights power dozens of quantized and fine-tuned derivatives. Studying it helps understand both the frontier model quality and the broader Qwen ecosystem.

2. **[MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3)**  
   The most important video-generation model in the current trend cycle, given its massive download count and the early appearance of FastVideo and OpenVDN derivatives. It is worth testing for practical text/image-to-video workflows.

3. **[google/timesfm-3.0-pytorch](https://huggingface.co/google/timesfm-3.0-pytorch)**  
   A useful reminder that not everything on the frontier is an LLM. Time-series forecasting is a high-value specialized task, and TimesFM is a compact, production-relevant PyTorch model for numeric/predictive applications.

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*