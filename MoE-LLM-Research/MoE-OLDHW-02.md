# 🚀 MoE-OLDHW-02: Ultra Low VRAM MoE on GTX 1060 3GB

![VRAM Badge](https://img.shields.io/badge/VRAM-3GB-critical?style=flat-square) 
![Context Badge](https://img.shields.io/badge/Context-16K-blue?style=flat-square) 
![Speed Badge](https://img.shields.io/badge/Speed-20+t%2Fs-orange?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

**A Complete Research Guide & Hands-On Setup**  
**Ultra Low VRAM Edition for GTX 1060 3GB**

**Focus Model**: Qwen3.6-35B-A3B (≈35B total parameters, **only ~3B active** per token)  
**Target Hardware**: **GTX 1060 3GB** + 32GB+ System RAM

---

## ✨ Why MoE Makes 3GB VRAM Possible

Traditional dense models are impossible on 3GB VRAM.  
**Mixture-of-Experts (MoE)** only activates a tiny fraction of parameters per token, allowing massive models to run on ancient hardware when combined with aggressive offloading and **TurboQuant**.

> **Real-World Result**: Stable ~20 tokens/second at 16K context on a **GTX 1060 3GB** 🔥

---

## 📋 Table of Contents

- [Introduction](#introduction)
- [Hardware Requirements](#hardware-requirements)
- [Model Download](#model-download)
- [Important: TurboQuant Build Required](#important-turboquant-build-required)
- [Setup & Best Commands](#setup--best-commands)
- [Performance Breakdown](#performance-breakdown)
- [Advanced Tips & Troubleshooting](#advanced-tips--troubleshooting)
- [Conclusion](#conclusion)

---

## 📖 Introduction

This is the **second iteration** of the MoE-OLDHW research series, specifically optimized for **extremely constrained 3GB VRAM** cards like the GTX 1060 3GB. 

It builds upon the original guide but pushes the limits further with heavier CPU offloading, reduced GPU layers, specialized KV cache types, and **TurboQuant** optimizations.

---

## 🛠 Hardware Requirements

| Component       | Minimum          | Recommended       | Notes |
|-----------------|------------------|-------------------|-------|
| GPU             | **3GB VRAM**     | 4GB+ VRAM         | GTX 1060 3GB tested |
| System RAM      | 24GB             | **32GB+**         | Critical for MoE experts |
| CPU             | 6+ cores         | 8+ cores          | Heavy CPU inference load |
| Storage         | SSD (25GB free)  | NVMe SSD          | Faster loading |

---

## 📥 Model Download

**Recommended Quant**: `Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive-Q4_K_P.gguf`

- Use a highly optimized Q4_K_P variant for best quality/speed balance on 3GB cards.

---

## ❗ Important: TurboQuant Build Required

This specificc setup **requires a TurboQuant fork** of llama.cpp for the specialized KV cache types (`turbo2` / `turbo3`) and enhanced MoE handling.

It's also good to make the build specific to your GPU - CUDA

---

## 🔥 Setup & Best Commands

### Ultimate Launch Script (GTX 1060 3GB)

```bash
#!/bin/bash
MODEL_PATH="$HOME/llama_cpp/models/Qwen3.6-35B-A3B-Uncensored-HauhauCS-Aggressive-Q4_K_P.gguf"
SERVER_BIN="$HOME/llama_cpp/llama-cpp-turboquant/build/bin/llama-server"

if [ ! -f "$MODEL_PATH" ]; then
   echo "ERROR: Model file not found at $MODEL_PATH"
   exit 1
fi

echo "========================================================="
echo " ULTRA LOW VRAM - MAX STABILITY (GTX 1060 3GB)"
echo "========================================================="

export TURBO_AUTO_ASYMMETRIC=1

$SERVER_BIN \
  -m "$MODEL_PATH" \
  --host 0.0.0.0 \
  --port 8080 \
  -ngl 60 \
  --n-cpu-moe 99 \
  -c 16384 \
  --parallel 1 \
  --batch-size 1024 \
  --ubatch-size 512 \
  --threads 6 \
  --cont-batching \
  --flash-attn off \
  --no-mmap \
  --mlock \
  --no-warmup \
  --cache-ram 0 \
  --cache-type-k turbo3 \
  --cache-type-v turbo2 \
  --jinja \
  --chat-template-file "$HOME/llama_cpp/Hermes-Template.jinja" \
  --fit off
```

### Key Flags Explained

| Flag                        | Value     | Why It Matters on 3GB |
|----------------------------|-----------|-----------------------|
| `-ngl`                     | 60        | Conservative GPU layers |
| `--n-cpu-moe`              | 99        | Maximum expert offload to CPU |
| `--cache-type-k/v`         | turbo3/2  | TurboQuant compression |
| `--cache-ram 0`            | 0         | No RAM cache overhead |
| `--flash-attn off`         | off       | Stability on old CUDA |
| `--no-mmap + --mlock`      | -         | Keep everything in RAM |

---

## 📊 Performance Breakdown

| Configuration              | Tokens/sec | Context   | VRAM Used |
|---------------------------|------------|-----------|-----------|
| Baseline (no offload)     | ~2–3 t/s   | 4K        | ~2.9GB    |
| Moderate CPU MoE          | 6–9 t/s    | 8K        | ~2.7GB    |
| **Full Ultra Settings**   | **~20 t/s** | **16K**   | **~2.8GB** |

**Tested on**: GTX 1060 3GB + i7-7700 + 32GB RAM

---

## ⚡ Advanced Tips & Troubleshooting

**Pro Tips**:
- Start with lower `--n-cpu-moe` (e.g. 48) and increase until stable
- Monitor GPU/CPU usage with `nvidia-smi` and `htop`
- Use `--threads` equal to your physical cores minus 2
- Try different TurboQuant cache types if you get instability

**Common Issues**:
- **CUDA OOM** → Lower `-ngl` to 40–50
- **Very slow generation** → Increase `--n-cpu-moe`
- **Model fails to load** → Ensure `--no-mmap` and enough system RAM
- **Poor coherence** → Try a different Q4_K variant

---

## 🎯 Conclusion

Running a **35B+ MoE model on a 3GB GTX 1060** was considered impossible just months ago. With MoE architecture, aggressive offloading, and TurboQuant optimizations, frontier-level local AI is now accessible on truly ancient hardware.

**Privacy, zero cost, and maximum tinkering fun achieved.**

**Happy inferencing!** 🚀

---

**Made with ❤️ by Black Rain Labs for the local LLM community**  
*Last updated: June, 4, 2026*  
**MoE-OLDHW-02** — GTX 1060 3GB Edition
