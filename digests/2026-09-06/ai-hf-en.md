# Hugging Face Trending Models Digest 2026-09-06

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-09-06 04:06 UTC

---

# Hugging Face Trending Models Digest — 2026-09-06

## 1. Today’s Highlights

The Qwen3.8 family is the clear center of gravity this week: **Qwen/Qwen3.8-27B** leads with 14K likes and over 6M downloads, while roughly a third of the trending list consists of its GGUF, abliterated, and community-uncensored variants. Frontier labs are shipping compact multimodal “Flash” models in parallel — DeepSeek-V4-Flash-Vision-Exp, Qwen3.8-Flash-Next, and GLM-5.3-Flash — indicating that fast, vision-capable chat models are a default form factor. On the generation side, video is the strongest momentum story: **MiniMax-H3**, **Lightricks/LTX-2.5**, and the FastVideo few-step preview all chart high. Meanwhile, the evergreen infrastructure stack (MiniLM, BERT, CLIP, GPT-2) keeps accruing massive download volumes, and specialized/vertical releases such as TimesFM-3.0 and Breeze-TTS-2 show broadening platform adoption.

## 2. Trending Models

### 🧠 Language Models

- [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) — Author: Qwen | Likes: 14,044 | Downloads: 6,024,467 — Qwen’s flagship open-weight multimodal LLM release, serving as the base model powering the largest ecosystem of GGUF/fine-tuned derivatives in this snapshot.

- [zai-org/GLM-5.3](https://huggingface.co/zai-org/GLM-5.3) — Author: zai-org | Likes: 1,720 | Downloads: 370,417 — The top conversational text-generation release from the GLM 5.x generation, trending on the strength of its MoE architecture and strong post-training.

- [IFM/K2-Horizon-MoVA-36B-A4B](https://huggingface.co/IFM/K2-Horizon-MoVA-36B-A4B) — Author: IFM | Likes: 175 | Downloads: 1,333 — A sparse MoE LLM (36B total, 4B active) from IFM in the K2-Horizon lineup, drawing attention as an efficiency-first frontier-family release.

- [tencent/Hy4-preview](https://huggingface.co/tencent/Hy4-preview) — Author: tencent | Likes: 443 | Downloads: 6,195 — Tencent’s preview of the Hunyuan v4 text-generation generation, interesting to users tracking proprietary-lab open-weight experiments.

- [XHToken/Spark-X2.5-4B](https://huggingface.co/XHToken/Spark-X2.5-4B) — Author: XHToken | Likes: 551 | Downloads: 4,755 — A compact 4B-class Spark 2.5 open LLM, notable as a lightweight option for local and efficient text-generation use cases.

- [openai-community/gpt2](https://huggingface.co/openai-community/gpt2) — Author: openai-community | Likes: 3,702 | Downloads: 14,739,982 — The evergreen GPT-2 small-language-model baseline, still ubiquitous in RLHF research, evaluation, and tooling.

### 🎨 Multimodal & Generation

- [deepseek-ai/DeepSeek-V4-Flash-Vision-Exp](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-Vision-Exp) — Author: deepseek-ai | Likes: 684 | Downloads: 184,542 — DeepSeek’s experimental Flash-scale V4 vision-language model, generating strong interest as a preview of the V4 family’s multimodal direction.

- [Qwen/Qwen3.8-Flash-Next](https://huggingface.co/Qwen/Qwen3.8-Flash-Next) — Author: Qwen | Likes: 4,918 | Downloads: 401,327 — A fast, next-generation Qwen3.8 multimodal chat variant tagged as an experiment for the Qwen4 era, trending alongside the 27B flagship.

- [zai-org/GLM-5.3-Flash](https://huggingface.co/zai-org/GLM-5.3-Flash) — Author: zai-org | Likes: 2,080 | Downloads: 727,610 — GLM’s high-throughput image-text-to-text “Flash” companion to GLM-5.3, popular for multimodal conversational applications.

- [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) — Author: MiniMaxAI | Likes: 4,938 | Downloads: 5,057,414 — A leading open-weights text-to-video and image-to-video generation model, with huge download momentum in the open video space.

- [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) — Author: Lightricks | Likes: 2,894 | Downloads: 1,484,329 — A diffusion-based image/video generation suite supporting video-to-video and text-to-video workflows, widely used for creative editing.

- [FastVideo/FastVideo-FastH3-4-step-Preview-v1-VSA-DataFree](https://huggingface.co/FastVideo/FastVideo-FastH3-4-step-Preview-v1-VSA-DataFree) — Author: FastVideo | Likes: 279 | Downloads: 22,851 — A 4-step/distilled video-generation preview built in the FastVideo ecosystem, signaling growing demand for low-latency inference.

- [BreezeBlue/Breeze-TTS-2](https://huggingface.co/BreezeBlue/Breeze-TTS-2) — Author: BreezeBlue | Likes: 449 | Downloads: 5,962 — A next-generation open text-to-speech model with transformer-based conditioning; trending quickly despite modest download volume.

- [facebook/mms-300m](https://huggingface.co/facebook/mms-300m) — Author: facebook | Likes: 263 | Downloads: 12,961 — Meta’s 300M-parameter multilingual speech foundation model, gaining renewed attention as audio/multimodal tooling expands.

### 🔧 Specialized Models

- [google/timesfm-3.0-pytorch](https://huggingface.co/google/timesfm-3.0-pytorch) — Author: google | Likes: 460 | Downloads: 123,025 — Google’s pretrained time-series forecasting foundation model, increasingly popular as a zero-shot forecasting baseline.

- [sentence-transformers/all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) — Author: sentence-transformers | Likes: 5,560 | Downloads: 255,006,933 — The go-to compact sentence-embedding model; still the most-downloaded model on the Hub and the default retrieval backbone for RAG pipelines.

- [google-bert/bert-base-uncased](https://huggingface.co/google-bert/bert-base-uncased) — Author: google-bert | Likes: 2,986 | Downloads: 56,175,564 — The canonical masked-language-model encoder, remaining a fixture for NLP benchmarks, embedding infrastructure, and fine-tuning studies.

- [distilbert/distilbert-base-uncased](https://huggingface.co/distilbert/distilbert-base-uncased) — Author: distilbert | Likes: 1,156 | Downloads: 7,101,423 — The distilled BERT variant of choice for lightweight NLP and embedding workloads, retaining a steady community following.

- [openai/clip-vit-base-patch32](https://huggingface.co/openai/clip-vit-base-patch32) — Author: openai | Likes: 1,210 | Downloads: 20,755,211 — OpenAI’s foundational image-text contrastive model, widely used for zero-shot image classification and multimodal retrieval.

### 📦 Fine-Tunes & Quantizations

- [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) — Author: unsloth | Likes: 3,548 | Downloads: 10,157,510 — Unsloth’s high-efficiency GGUF quantization of Qwen3.8-27B; the standout distribution channel for local Qwen inference.

- [OBLITERATUS/Qwen3.8-27B-OBLITERATED](https://huggingface.co/OBLITERATUS/Qwen3.8-27B-OBLITERATED) — Author: OBLITERATUS | Likes: 1,096 | Downloads: 968,936 — An abliterated/uncensored treatment of Qwen3.8-27B offered in MLX, GGUF, and safetensors, highlighting ongoing community demand for reduced-refusal models.

- [JonathanColetti/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/JonathanColetti/Qwen3.8-27B-Uncensored-GGUF) — Author: JonathanColetti | Likes: 988 | Downloads: 2,453,361 — A popular llama.cpp/GGUF uncensored variant with MTP support, one of the most-downloaded Qwen3.8 community releases.

- [HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF](https://huggingface.co/HauhauCS/Qwen3.8-27B-Uncensored-HauhauCS-Aggressive-MTP-GGUF) — Author: HauhauCS | Likes: 966 | Downloads: 1,527,627 — An “aggressive” uncensored multimodal GGUF build of Qwen3.8-27B, demonstrating the active role of persona-style fine-tunes in the open ecosystem.

- [unsloth/Qwen3.8-Flash-Next-GGUF](https://huggingface.co/unsloth/Qwen3.8-Flash-Next-GGUF) — Author: unsloth | Likes: 800 | Downloads: 780,823 — Unsloth’s quantized distribution of Qwen3.8-Flash-Next, making the fast multimodal chat model easy to run locally.

- [orcarouter/Qwen3.8-27B-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-27B-Uncensored-GGUF) — Author: orcarouter | Likes: 729 | Downloads: 283,774 — Another major uncensored/abliterated Qwen3.8 GGUF release, representative of the crowded “refusal-free Qwen” fine-tune niche.

- [ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF](https://huggingface.co/ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF) — Author: ISTA-DASLab | Likes: 418 | Downloads: 297,493 — A research-grade mixed-precision quantization of Qwen3.8-27B using GSQ + RCO, notable for pushing compression quality beyond uniform-bit GGUF.

- [orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF) — Author: orcarouter | Likes: 240 | Downloads: 106,845 — The Flash-Next counterpart to the uncensored Qwen3.8 GGUF wave, broadening the abliterated ecosystem beyond the 27B model.

- [DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF) — Author: DavidAU | Likes: 227 | Downloads: 174,405 — A large community merge/GGUF emphasizing uncensored and coding-oriented behavior; part of the Qwen3.8 fine-tune explosion.

- [Jackrong/Qwopus3.8-27B-Flash-GGUF](https://huggingface.co/Jackrong/Qwopus3.8-27B-Flash-GGUF) — Author: Jackrong | Likes: 120 | Downloads: 10,680 — An alternative llama.cpp-ready GGUF build of Qwen3.8 with vision tooling, illustrating the diversity of third-party quantizations.

- [OpenVDN/vdn-minimax-h3](https://huggingface.co/OpenVDN/vdn-minimax-h3) — Author: OpenVDN | Likes: 191 | Downloads: 0 — A community fine-tune/adaptation of MiniMax-H3 aimed at text-to-video use cases, still early in adoption but representative of derivative video-model work.

## 3. Ecosystem Signal

The strongest ecosystem signal this week is **Qwen becoming the new platform layer** for open-weight experimentation. Of 30 trending models, more than 14 are Qwen3.8 releases: the base model, first-party GGUF distributions from unsloth, and community uncensored/abliterated fine-tunes. This mirrors the earlier Llama ecosystem playbook: an open strong base, rapid quantization, and then a long tail of persona and refusal-reduced builds.

Open weights are clearly winning the trending chart, with Qwen, DeepSeek, zai-org/GLM, MiniMax, and Tencent all contributing frontier-class models. Notably, many base models are now natively multimodal (image-text-to-text), so vision is no longer an optional add-on. Video generation is also maturing rapidly: MiniMax-H3 attracted 5M downloads, while few-step and distilled video variants point to an inference-cost race.

Finally, quantization itself has become a research surface: ISTA-DASLab’s GSQ-RCO mixed-precision GGUF shows that the community is moving beyond generic quantizations toward higher-quality compression. Meanwhile, classic embedding and baseline models continue to dominate cumulative downloads — MiniLM alone sits above 255M — reminding us that RAG and classical fine-tuning workloads remain the true backbone of Hub usage.

## 4. Worth Exploring

- [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) — The anchor of this entire ecosystem wave; worth studying to understand what a modern open-weight multimodal base model enables before community fine-tuning, quantization, and abliteration layers are applied.

- [MiniMaxAI/MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) — One of the fastest-adopted open video-generation releases on the Hub (5M+ downloads), making it a key reference point for text-to-video and image-to-video quality in the open-weight space.

- [ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF](https://huggingface.co/ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF) — A compelling technical artifact for anyone interested in mixed-precision and quantization research, showing how GSQ-style mixed-precision techniques can push GGUF efficiency beyond symmetric bit-widths.

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*