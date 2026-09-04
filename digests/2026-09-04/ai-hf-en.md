# Hugging Face Trending Models Digest 2026-09-04

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-09-04 04:02 UTC

---

# Hugging Face Trending Models Digest — 2026-09-04

## 1. Today's Highlights

This week’s Hugging Face leaderboard is dominated by the **Qwen3.8** ecosystem: the official multimodal Qwen/Qwen3.8-27B is the most-liked model of the week with **13,843 weekly likes**, while the Qwen3.8-27B GGUF quantization is its most-downloaded derivative with **9.55M downloads**. Large image-text models — GLM-5.3-Flash, Qwen3.8-Flash-Next, DeepSeek-V4-Flash-Vision-Exp — signal that the frontier is shifting to multimodal chat as the default release format. Video generation also surged: MiniMax-H3 crossed 5M downloads and Lightricks LTX-2.5 continues to draw thousands of weekly likes, with a new FastVideo 4-step preview aiming at faster inference. Community activity is heavily focused on quantization and “uncensored”/abliterated rewrites of frontier open-weights models, suggesting strong grassroots demand for local, steerable LLMs. Meanwhile, classic production stalwarts such as sentence-transformers/all-MiniLM-L6-v2, BERT-base, and GPT-2 remain remarkably durable, appearing beside their much younger frontier counterparts.

## 2. Trending Models

### 🧠 Language Models (LLMs, chat models, instruction-tuned)

- **[zai-org/GLM-5.3](https://huggingface.co/zai-org/GLM-5.3)** — Author: zai-org · Likes: 1,619 · Downloads: 151,021  
  The flagship GLM MoE language model release of the week, drawing attention as the top pure text-generation LLM in this trend list.

- **[tencent/Hy4-preview](https://huggingface.co/tencent/Hy4-preview)** — Author: tencent · Likes: 417 · Downloads: 4,449  
  Tencent’s preview of its Hunyuan fourth-generation LLM, noteworthy as a major Chinese vendor open-weights release.

- **[openai-community/gpt2](https://huggingface.co/openai-community/gpt2)** — Author: openai-community · Likes: 3,606 · Downloads: 14,071,683  
  The classic GPT-2 base model remains a near-permanent trending item due to continued downstream fine-tuning and educational use.

- **[XHToken/Spark-X2.5-4B](https://huggingface.co/XHToken/Spark-X2.5-4B)** — Author: XHToken · Likes: 168 · Downloads: 1,514  
  A 4B-parameter Spark LLM aimed at efficient deployment, trending as a compact chat/text-generation option.

### 🎨 Multimodal & Generation (image, video, audio, text-to-X)

- **[Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B)** — Author: Qwen · Likes: 13,843 · Downloads: 5,254,882  
  The week’s most-liked release: Qwen’s 27B multimodal image-text model, the foundation of an enormous quantization and fine-tuning ecosystem.

- **[Qwen/Qwen3.8-Flash-Next](https://huggingface.co/Qwen/Qwen3.8-Flash-Next)** — Author: Qwen · Likes: 4,816 · Downloads: 263,287  
  A faster/experimental Qwen3.8 “Flash-Next” variant for conversational image-text-to-text tasks, popular for high-throughput multimodal serving.

- **[zai-org/GLM-5.3-Flash](https://huggingface.co/zai-org/GLM-5.3-Flash)** — Author: zai-org · Likes: 2,021 · Downloads: 517,902  
  The multimodal, vision-language Flash variant of GLM-5.3, quickly adopted for conversational image-text applications.

- **[deepseek-ai/DeepSeek-V4-Flash-Vision-Exp](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-Vision-Exp)** — Author: deepseek-ai · Likes: 557 · Downloads: 54,571  
  DeepSeek’s experimental Flash-tier vision-language model, signaling DeepSeek's entry into compact multimodal chat.

- **[Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5)** — Author: Lightricks · Likes: 2,702 · Downloads: 1,293,463  
  A versatile image-to-video / text-to-video / video-to-video diffusion model, trending strongly for creative video generation workflows.

- **[MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3)** — Author: MiniMaxAI · Likes: 4,866 · Downloads: 5,092,067  
  One of the most downloaded video models on the Hub this week, a text-to-video / image-to-video diffusion model with major community traction.

- **[FastVideo/FastVideo-FastH3-4-step-Preview-v1-VSA-DataFree](https://huggingface.co/FastVideo/FastVideo-FastH3-4-step-Preview-v1-VSA-DataFree)** — Author: FastVideo · Likes: 256 · Downloads: 0  
  A “data-free” 4-step fast inference preview built for MiniMax-H3-class video generation, trending as an efficiency experiment for low-cost video synthesis.

- **[BreezeBlue/Breeze-TTS-2](https://huggingface.co/BreezeBlue/Breeze-TTS-2)** — Author: BreezeBlue · Likes: 398 · Downloads: 3,861  
  A new text-to-speech generation model from BreezeBlue, trending among TTS adopters exploring lightweight speech synthesis.

### 🔧 Specialized Models (code, math, medical, embeddings, forecasting, speech)

- **[google/timesfm-3.0-pytorch](https://huggingface.co/google/timesfm-3.0-pytorch)** — Author: google · Likes: 370 · Downloads: 46,862  
  Google’s third-generation time-series forecasting foundation model in PyTorch, a growing reference point for domain-specific forecasting.

- **[sentence-transformers/all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2)** — Author: sentence-transformers · Likes: 5,461 · Downloads: 246,135,287  
  The most-downloaded embedding model on the Hub, continually trending due to its use as the default sentence-embedding workhorse.

- **[google-bert/bert-base-uncased](https://huggingface.co/google-bert/bert-base-uncased)** — Author: google-bert · Likes: 2,910 · Downloads: 58,556,227  
  The foundational Masked Language Model, still central to NLP pipelines, fine-tuning, and academic baselines.

- **[distilbert/distilbert-base-uncased](https://huggingface.co/distilbert/distilbert-base-uncased)** — Author: distilbert · Likes: 1,092 · Downloads: 6,761,868  
  DistilBERT’s distilled encoder remains a compact, efficient favorite for lightweight classification and retrieval.

- **[pipecat-ai/phonellm-alpha-1](https://huggingface.co/pipecat-ai/phonellm-alpha-1)** — Author: pipecat-ai · Likes: 208 · Downloads: 11,526  
  An alpha phone-domain LLM from Pipecat, an early exploration of models built for telephony/real-time voice use cases.

- **[facebook/mms-300m](https://huggingface.co/facebook/mms-300m)** — Author: facebook · Likes: 181 · Downloads: 12,386  
  Facebook’s Massively Multilingual Speech (wav2vec2) model for thousands of languages, relevant for cross-lingual speech research.

- **[openai/clip-vit-base-patch32](https://huggingface.co/openai/clip-vit-base-patch32)** — Author: openai · Likes: 1,136 · Downloads: 19,936,700  
  OpenAI’s standard CLIP vision-language model, heavily used for zero-shot image classification and image/text retrieval.

### 📦 Fine-tunes & Quantizations (community fine-tunes, GGUF, FP8, AWQ)

- **[unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF)** — Author: unsloth · Likes: 3,449 · Downloads: 9,553,042  
  Unsloth’s official GGUF quantization of Qwen3.8-27B, the most-downloaded model this week and the preferred local-execution entry point.

- **[unsloth/Qwen3.8-Flash-Next-GGUF](https://huggingface.co/unsloth/Qwen3.8-Flash-Next-GGUF)** — Author: unsloth · Likes: 764 · Downloads: 535,984  
  GGUF quantized version of Qwen3.8-Flash-Next, bringing a fast multimodal model to local and edge hardware.

- **[unsloth/GLM-5.3-Flash-GGUF](https://huggingface.co/unsloth/GLM-5.3-Flash-GGUF)** — Author: unsloth · Likes: 349 · Downloads: 75,195  
  Unsloth’s GGUF release of GLM-5.3-Flash, making zai-org’s multimodal chat model easily runnable offline.

- **[ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF](https://huggingface.co/ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF)** — Author: ISTA-DASLab · Likes: 250 · Downloads: 100,110  
  Research-grade mixed-precision GSQ-RCO quantization of Qwen3.8-27B, trending for demonstrating compression beyond standard GGUF.

- **[orcarouter/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-GGUF)** — Author: orcarouter · Likes: 687 · Downloads: 262,325  
  An abliterated/uncensored GGUF of Qwen3.8-27B, attracting users seeking reduced-refusal local chat models.

- **[orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF)** — Author: orcarouter · Likes: 212 · Downloads: 85,105  
  The uncensored/abliterated GGUF companion of Qwen3.8-Flash-Next for multimodal local chat.

- **[orcarouter/GLM-5.3-Flash-Uncensored-FP8](https://huggingface.co/orcarouter/GLM-5.3-Flash-Uncensored-FP8)** — Author: orcarouter · Likes: 165 · Downloads: 4,477  
  FP8-precision, abliterated GLM-5.3-Flash variant, a “less restricted” multimodal model with efficient precision.

- **[HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF)** — Author: HauhauCS · Likes: 900 · Downloads: 1,336,061  
  A community GGUF emphasizing aggressive uncensored chat behavior and multi-token prediction for Qwen3.8-27B.

- **[OBLITERATUS/Qwen3.8-27B-OBLITERATED](https://huggingface.co/OBLITERATUS/Qwen3.8-27B-OBLITERATED)** — Author: OBLITERATUS · Likes: 1,060 · Downloads: 848,781  
  The widely shared “abliterated” rewrite of Qwen3.8-27B, distributed in GGUF, MLX, and safetensors formats for local use.

- **[DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF)** — Author: DavidAU · Likes: 140 · Downloads: 39,646  
  A community “mega-merge” fine-tune adding uncensored and coding-tuned flavors to Qwen3.8-27B in GGUF form.

- **[OpenVDN/vdn-minimax-h3](https://huggingface.co/OpenVDN/vdn-minimax-h3)** — Author: OpenVDN · Likes: 142 · Downloads: 0  
  A fresh community fine-tune on MiniMax-H3 for text-to-video, showing early experimentation with the new video base model.

## 3. Ecosystem Signal

This week’s chart shows a quiet structural shift: **multimodal “chat” models are now the frontier default**. Qwen3.8-27B, GLM-5.3-Flash, Qwen3.8-Flash-Next, and DeepSeek-V4-Flash-Vision-Exp all ship via image-text-to-text pipelines, meaning open-weight leaders are being designed as vision-language assistants, not text-only LLMs. The Qwen3.8 family is the unmistakable center of gravity — not only the official release, but a dense layer of GGUF quantizations, uncensored/abliterated rewrites, and heavily merged fine-tunes. This suggests that much of the current ecosystem value is concentrated in community serving infrastructure and model “personality” modifications rather than new pre-training runs. Video generation has also hit an inflection point: MiniMax-H3 reached millions of downloads while FastVideo and LTX-2.5 push toward fewer inference steps and cheaper video. At the same time, small production embedders and classic encoders like all-MiniLM-L6-v2, BERT, DistilBERT, and CLIP persist at extraordinary cumulative download counts, indicating that most real-world applications still run on old, efficient encoder models rather than frontier-size LLMs. Finally, the rapid appearance of official “Flash” tiers from Qwen, GLM, and DeepSeek points to a maturing open-weight market organized around speed, size, and serving cost.

## 4. Worth Exploring

- **[Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B)** — Worth studying both as a multimodal frontier release and as the base model for the week’s largest quantization and fine-tuning ecosystem. Opening its model card provides a useful snapshot of how modern open-weight release strategy works: unified multimodal inputs, commercial license, and a rich derivative ecosystem.

- **[ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF](https://huggingface.co/ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF)** — A research-driven GGUF using GSQ and row-column optimization instead of standard group-wise quantization. It is valuable for practitioners who want to push 27B-class quality and speed on constrained hardware, and for researchers tracking quantization research in real time.

- **[FastVideo/FastVideo-FastH3-4-step-Preview-v1-VSA-DataFree](https://huggingface.co/FastVideo/FastVideo-FastH3-4-step-Preview-v1-VSA-DataFree)** — Video generation is still the least “industrialized” frontier area. This 4-step, data-free preview is an early attempt to cut video inference cost drastically, making it a compelling artifact to study if you are tracking diffusion acceleration techniques or thinking about building on MiniMax-H3.

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*