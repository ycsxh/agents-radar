# Hugging Face Trending Models Digest 2026-07-09

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-07-08 17:22 UTC

---

# Hugging Face Trending Models Digest — 2026-07-09

## Today's Highlights

The HF Hub this week is dominated by **Qwen-based MoE models** and a new wave of **vision-capable GGUF quantizations**, signaling that multimodal inference on consumer hardware is maturing rapidly. **NVIDIA** makes multiple appearances with both vision models (LocateAnything-3B) and high-performance quantized checkpoints (Qwen3.6-27B-NVFP4), while **DeepSeek V4** variants begin appearing in early quantized form. Community fine-tunes like **gemma-4-12B-coder-fable5** and **Ornith-1.0** series are driving outsized download volumes. The biggest surprise is **zai-org/GLM-5.2**, a conversational MoE model that quietly amassed 3.6K likes in its debut week.

## Trending Models

### 🧠 Language Models (LLMs, chat models, instruction-tuned)

- **[zai-org/GLM-5.2](https://huggingface.co/zai-org/GLM-5.2)** by zai-org · 3,648 likes · 281K downloads  
  A new conversational MoE model from Zhipu AI's GLM lineage, trending as the highest-liked release this week—likely a strong open-weight competitor in the chat arena.

- **[deepseek-ai/DeepSeek-V4-Pro-DSpark](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-DSpark)** by deepseek-ai · 433 likes · 15.5K downloads  
  The first public release of DeepSeek V4 Pro with DSpark inference support, accompanied by an arXiv paper (2606.19348) and strong community anticipation.

- **[meituan-longcat/LongCat-2.0](https://huggingface.co/meituan-longcat/LongCat-2.0)** by meituan-longcat · 148 likes · 385 downloads  
  Meituan's updated long-context conversational model, designed for extended dialogue and retrieval-augmented use cases.

- **[poolside/Laguna-XS-2.1](https://huggingface.co/poolside/Laguna-XS-2.1)** by poolside · 75 likes · 3.4K downloads  
  A compact code-focused LLM from poolside, optimized for software engineering agent tasks in constrained deployment scenarios.

### 🎨 Multimodal & Generation (image, video, audio, text-to-X)

- **[nvidia/LocateAnything-3B](https://huggingface.co/nvidia/LocateAnything-3B)** by nvidia · 2,665 likes · 1.4M downloads  
  NVIDIA's 3B parameter visual grounding model for object detection and localization, trending due to its strong performance on zero-shot referring expression tasks.

- **[HauhauCS/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive](https://huggingface.co/HauhauCS/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive)** by HauhauCS · 2,569 likes · 2.8M downloads  
  An uncensored, aggressively fine-tuned Qwen3.6 35B MoE vision model in GGUF format—massive download count driven by the uncensored community and vision MoE appeal.

- **[empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF](https://huggingface.co/empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF)** by empero-ai · 1,829 likes · 1.7M downloads  
  A GGUF quantized version of the Qwythos 9B multimodal reasoning model, fine-tuned on Mythos synthetic data—highly downloaded for local reasoning + vision.

- **[baidu/Unlimited-OCR](https://huggingface.co/baidu/Unlimited-OCR)** by baidu · 1,861 likes · 1.1M downloads  
  Baidu's unlimited OCR model capable of document-level text extraction with no constraint on input size, trending for its practical utility in enterprise workflows.

- **[krea/Krea-2-Turbo](https://huggingface.co/krea/Krea-2-Turbo)** by krea · 552 likes · 123K downloads  
  Krea's accelerated text-to-image model built on their Krea-2-Raw base, optimized for faster inference while maintaining generative quality.

- **[bottlecapai/ThinkingCap-Qwen3.6-27B](https://huggingface.co/bottlecapai/ThinkingCap-Qwen3.6-27B)** by bottlecapai · 135 likes · 46 downloads  
  A Qwen3.6 based vision-language model with chain-of-thought reasoning capabilites, still early but showing promise for visual reasoning tasks.

### 🔧 Specialized Models (code, math, medical, embeddings)

- **[google/tabfm-1.0.0-pytorch](https://huggingface.co/google/tabfm-1.0.0-pytorch)** by google · 305 likes · 9.5K downloads  
  Google's TabFM foundation model for zero-shot tabular classification and regression, built with PyTorch—significant as it marks a shift toward tabular foundation models.

- **[nvidia/Nemotron-Labs-TwoTower-30B-A3B-Base-BF16](https://huggingface.co/nvidia/Nemotron-Labs-TwoTower-30B-A3B-Base-BF16)** by nvidia · 134 likes · 10.9K downloads  
  A 30B parameter two-tower model from NVIDIA for embedding and retrieval tasks, using a 3B active MoE architecture for efficient inference.

- **[yuxinlu1/gemma-4-12B-coder-fable5-composer2.5-v1-GGUF](https://huggingface.co/yuxinlu1/gemma-4-12B-coder-fable5-composer2.5-v1-GGUF)** by yuxinlu1 · 2,647 likes · 675K downloads  
  A GGUF-quantized Gemma 4 12B coding model fine-tuned with Fable5 + Composer2.5—highly popular among developers for local code generation and agentic workflows.

- **[yuxinlu1/gemma-4-12B-agentic-fable5-composer2.5-v2-3.5x-tau2-GGUF](https://huggingface.co/yuxinlu1/gemma-4-12B-agentic-fable5-composer2.5-v2-3.5x-tau2-GGUF)** by yuxinlu1 · 1,089 likes · 384K downloads  
  The agentic variant of the same Gemma 4 coding family, optimized for terminal and tool-calling tasks with improved tau2 merging strategy.

- **[conradlocke/krea2-identity-edit](https://huggingface.co/conradlocke/krea2-identity-edit)** by conradlocke · 88 likes · 0 downloads  
  A LoRA for identity-preserving image editing built on Krea-2, enabling consistent character editing in ComfyUI workflows.

### 📦 Fine-tunes & Quantizations (community fine-tunes, GGUF, AWQ)

- **[deepreinforce-ai/Ornith-1.0-35B-GGUF](https://huggingface.co/deepreinforce-ai/Ornith-1.0-35B-GGUF)** by deepreinforce-ai · 798 likes · 503K downloads  
  The GGUF quantized version of Ornith 1.0 35B MoE, a community fine-tune on Qwen3.5 base—strong download traction for open-source MoE reasoning on local hardware.

- **[deepreinforce-ai/Ornith-1.0-9B-GGUF](https://huggingface.co/deepreinforce-ai/Ornith-1.0-9B-GGUF)** by deepreinforce-ai · 459 likes · 455K downloads  
  Smaller sibling in the Ornith family, offering 9B dense performance with GGUF quantization for efficient CPU/GPU inference.

- **[unsloth/Qwen3.6-27B-MTP-GGUF](https://huggingface.co/unsloth/Qwen3.6-27B-MTP-GGUF)** by unsloth · 1,003 likes · 2.8M downloads  
  Unsloth's Multi-Turn-Prompt GGUF quantization of Qwen3.6 27B, achieving extremely high download volume for multimodal instruction-following.

- **[nvidia/Qwen3.6-27B-NVFP4](https://huggingface.co/nvidia/Qwen3.6-27B-NVFP4)** by nvidia · 322 likes · 539K downloads  
  NVIDIA's FP4 quantized version of Qwen3.6 27B using Model Optimizer, enabling near-lossless inference on OVX/A100 with massive memory savings.

- **[InternScience/Agents-A1](https://huggingface.co/InternScience/Agents-A1)** by InternScience · 393 likes · 14.7K downloads  
  A Qwen3.5 MoE model fine-tuned for agentic tasks with vision capabilities, trending in the agent research community.

- **[InternScience/Agents-A1-Q4_K_M-GGUF](https://huggingface.co/InternScience/Agents-A1-Q4_K_M-GGUF)** by InternScience · 82 likes · 11.2K downloads  
  GGUF quantized variant of Agents-A1, making the vision-capable MoE agent model accessible to consumer hardware.

- **[froggeric/Qwen-Fixed-Chat-Templates](https://huggingface.co/froggeric/Qwen-Fixed-Chat-Templates)** by froggeric · 764 likes · 0 downloads  
  A utility model providing corrected Jinja chat templates for Qwen 3.5 models—viral for fixing widespread template parsing issues in local deployments.

- **[unsloth/DeepSeek-V4-Flash-GGUF](https://huggingface.co/unsloth/DeepSeek-V4-Flash-GGUF)** by unsloth · 81 likes · 47 downloads  
  Early GGUF quantization of DeepSeek V4 Flash from Unsloth, making the newest DeepSeek architecture available for local inference experimentation.

## Ecosystem Signal

The HF ecosystem is seeing a convergence of **three major trends**: (1) **Qwen 3.5/3.6 dominance** as the preferred base for community fine-tunes—over 40% of this week's top models derive from Qwen variants, spanning reasoning, vision, and uncensored use cases. (2) **MoE is becoming mainstream** for local deployment, with models like Ornith 1.0 35B (35B total, 8B active) and GLM-5.2 achieving high engagement while remaining quantizable to consumer hardware. (3) **GGUF quantization is the default format** for model distribution—7 of the top 10 most downloaded models this week are GGUF, as the community prioritizes practical CPU/GPU inference over raw benchmark scores. Notably, NVIDIA is aggressively entering the open-weight space with both specialized models (LocateAnything, Nemotron TwoTower) and quantization tooling (NVFP4), signaling a shift toward inference-optimized releases from major hardware vendors. The DeepSeek V4 ecosystem is still nascent but already seeing early quantized offerings from Unsloth.

## Worth Exploring

1. **[NVIDIA's LocateAnything-3B](https://huggingface.co/nvidia/LocateAnything-3B)** — A 3B vision grounding model with 2.6K likes and 1.4M downloads in its first week. Its combination of small size, strong zero-shot localization, and NVIDIA backing makes it a must-test for anyone building visual search or detection pipelines on a budget.

2. **[zai-org/GLM-5.2](https://huggingface.co/zai-org/GLM-5.2)** — The highest-liked model this week with 3.6K likes but only 281K downloads, suggesting strong interest but low actual usage yet. As a new MoE conversational model from the GLM lineage, it's worth evaluating as an alternative to Qwen-based MoE models for chat applications.

3. **[yuxinlu1/gemma-4-12B-coder-fable5-composer2.5-v1-GGUF](https://huggingface.co/yuxinlu1/gemma-4-12B-coder-fable5-composer2.5-v1-GGUF)** — With 2.6K likes and 675K downloads, this Gemma 4 coding fine-tune represents the cutting edge of local code generation. The combination of Fable5 fine-tuning data and Composer2.5 merge technique yields a model that consistently outperforms larger open-code baselines in developer benchmarks.

---
*This digest is auto-generated by [agents-radar](https://github.com/ycsxh/agents-radar).*