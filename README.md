🎬 Automated GenAI Video Orchestration Pipeline (v13)

A low-code/code-hybrid architecture for end-to-end video production, utilizing n8n, ComfyUI, and FFmpeg.
📋 Project Overview

This repository contains the orchestration logic for a fully automated financial commentary video pipeline. It transforms raw text/ideas into 1440p (2K) narrative videos without human intervention in the loop.

The Product Goal: Reduce video production cycle time from 40 hours (manual) to <4 hours (automated) while eliminating SaaS subscription dependencies by utilizing local hardware.

🏗️ Architecture & Logic

The pipeline is managed by n8n (workflow provided in YT-Finance-Workflow-V13.json) which acts as the control plane, orchestrating distinct microservices:

1. Ingestion & Narrative Construction

    Transcript Extraction: Ingests raw YouTube URLs via Supadata.

    LLM Processing (DeepSeek-V3):

        Sanitization: Cleans raw transcripts (removes "fluff", preserves financial data).

        Scripting: Re-writes content into a specific "Financial Realist" persona.

        Title/Metadata: Generates high-CTR titles and SEO tags via structured output parsers.

2. Semantic Scene Segmentation (Custom JS)

Unlike standard fixed-time splitting, this workflow utilizes a custom JavaScript Algorithm (visible in Code in JavaScript node) to parse script density:

    Dynamic Pacing: Calculates scene duration based on sentence structure and semantic breaks.

    SRT Mapping: Aligns audio timestamps with visual scene changes to ensure "cuts" happen on beat.

3. Visual Generation (ComfyUI + NVIDIA Optimization)

The workflow dispatches prompt payloads to a local ComfyUI instance:

    Model: Custom diffusion workflow utilizing Wan 2.1 / Flux with LoRA adapters for style transfer ("Impasto Oil Painting").

    Optimization: Implements SageAttention and Triton kernels to maximize inference speed on consumer hardware (RTX 4090).

    VRAM Management: Utilizes aggressive quantization and latent caching to render 2K resolution images.

4. Post-Production (FFmpeg Automation)

The pipeline executes raw shell commands via Docker to handle video assembly:

    Dynamic Ken Burns Effect: Uses mathematical expressions in FFmpeg filters (120+(240*(1-t/Duration))) to create random pan/zoom movement      on static images.

    Rendering: Encodes final output to 1440p (2560x1440) at 25fps using libx264.

🛠️ Tech Stack & Nodes

    Orchestrator: n8n (Self-Hosted)

    Database: Baserow (State management for scenes, prompts, and render status)

    Generative Models:

        Text: DeepSeek-V3 / Llama (via OpenRouter/Ollama)

        Visual: ComfyUI (Local API)

        Audio: OpenAI Whisper (Timestamping) + F5-TTS

    Infrastructure: Docker, NVIDIA CUDA 12.x
