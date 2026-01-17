# 🎬 Automated GenAI Video Orchestration Pipeline (v14)

**A low-code/code-hybrid architecture for end-to-end video production, utilizing n8n, ComfyUI, and FFmpeg.**

![n8n](https://img.shields.io/badge/Orchestration-n8n-FF655A) ![ComfyUI](https://img.shields.io/badge/Generative-ComfyUI-blue) ![NVIDIA](https://img.shields.io/badge/Hardware-RTX%204090-76B900) ![FFmpeg](https://img.shields.io/badge/Render-FFmpeg-green)

## 📋 Project Overview
This repository contains the orchestration logic for a **fully automated financial commentary video pipeline**. It transforms raw text/ideas into 1440p (2K) narrative videos without human intervention in the loop.

**The Product Goal:** Reduce video production cycle time from **40 hours** (manual) to **<4 hours** (automated) while eliminating SaaS subscription dependencies by utilizing local hardware.

## 🏗️ Architecture & Logic

The pipeline is managed by **n8n** (the control plane), which orchestrates distinct microservices for Image, Video, and Audio generation.

### 1. Orchestration (The Brain)
* **File:** `YT-Finance Workflow V14.json`
* **Function:** This is the master controller.
    * **Ingestion:** Grabs raw transcripts via Supadata/YouTube.
    * **Logic:** Uses DeepSeek-V3 to rewrite scripts and a custom JavaScript algorithm to segment the script into semantic scenes (5-10s duration).
    * **Assembly:** Triggers FFmpeg commands to stitch the final assets.

### 2. Visual Generation (The Engine)
* **File:** `02-Z Image Turbo txt2img bf16 1920x1080px FullHD_V3(1).json`
    * **Function:** High-speed Text-to-Image generation. Uses **Z-IMAGE/Turbo** models with **SageAttention** to generate the initial "Impasto Oil Painting" style frames at 1080p/2K.
* **File:** `Wan_Video_21_InfiniteTalk.json`
    * **Function:** Image-to-Video conversion using **Wan 2.1**. It takes the static frames and animates them based on the script's emotional tone.

### 3. Audio Synthesis (The Voice)
* **File:** `TTS - Single-Speaker.json`
    * **Function:** Dedicated text-to-speech workflow utilizing **InfiniteTalk**. It generates character-specific voiceovers and aligns them with the SRT timestamps.

## 🛠️ Tech Stack & Optimization

* **Orchestrator:** n8n (Self-Hosted, Docker)
* **Generative Backends:** ComfyUI (Local API)
* **Hardware:** NVIDIA RTX 4090 (24GB VRAM)
    * *Optimization:* Implemented **Triton kernels** and **SageAttention** to reduce latency by ~35%.
    * *Quantization:* GGUF/EXL2 strategies used to fit 14B+ parameter models alongside video rendering buffers.
* **Post-Production:** FFmpeg (Dynamic Zoom/Pan filters: `crop=w=2560:h=1440...`)

## 📺 Demo / Final Output

See the pipeline in action. This video was generated entirely using the logic in this repository (Script by DeepSeek, Visuals by Wan 2.1/ComfyUI, Voice by InfiniteTalk).

https://www.youtube.com/@Financial_History_and_Markets

*Click the link above to watch on YouTube.*

## 📂 Repository File Manifest

```text
├── YT-Finance Workflow V13 - FFMPEG & 1440P(1).json   # MAIN: n8n Orchestrator Logic
├── 02-Z Image Turbo txt2img...json                    # ComfyUI: Text-to-Image Generator
├── Wan_Video_21_InfiniteTalk.json                     # ComfyUI: Video & Lip-Sync Generator
├── TTS - Single-Speaker.json                          # ComfyUI: Audio/Speech Synthesis
├── LICENSE                                            # MIT License
└── README.md                                          # Documentation


