# 🚀 MoE-OLDHW: Running Massive Mixture-of-Experts Models on Old & Low-VRAM Hardware

![VRAM Badge](https://img.shields.io/badge/VRAM-6GB%2B-brightgreen?style=flat-square) 
![Context Badge](https://img.shields.io/badge/Context-256K-blue?style=flat-square) 
![Speed Badge](https://img.shields.io/badge/Speed-15%E2%80%9330%20t%2Fs-orange?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

**A Complete Research Paper & Hands-On Guide**  
**Based on Codacus' "Running a 35B AI Model on 6GB VRAM, FAST" (May 2026)**

**Focus Model**: Qwen3.6-35B-A3B (≈35B total parameters, **only ~3B active** per token)  
**Target Hardware**: GTX 1060 6GB + 32GB RAM (and other older GPUs)

---

## ✨ Why MoE Changes Everything

Traditional dense models waste VRAM by activating **all** parameters.  
**Mixture-of-Experts (MoE)** only activates a small subset of experts per token → massive efficiency gains on old hardware.

> **Real-World Result**: 15–30 tokens/second at 64K–256K context on a **GTX 1060 6GB** 🔥

---

## 📋 Table of Contents
- [Introduction](#introduction)
- [Hardware Requirements](#hardware-requirements)
- [Model Download](#model-download)
- [Setup & Best Commands](#setup--best-commands)
- [Performance Breakdown](#performance-breakdown)
- [Advanced Tips & Troubleshooting](#advanced-tips--troubleshooting)
- [Conclusion](#conclusion)

---

## 📖 Introduction

This guide compiles techniques from the referenced video and llama.cpp community best practices to run powerful MoE models locally on budget or legacy hardware.

**Key Innovation**: Intelligent offloading of MoE experts to CPU/RAM while keeping critical layers on GPU.

---

## 🛠 Hardware Requirements

| Component       | Minimum          | Recommended       | Notes |
|-----------------|------------------|-------------------|-------|
| GPU             | 6GB VRAM         | 8GB+ VRAM         | GTX 1060 / RTX 2060 work great |
| System RAM      | 16GB             | 32GB+             | For expert offloading |
| CPU             | 6+ cores         | 8+ cores          | Handles CPU experts |
| Storage         | SSD (20GB free)  | NVMe SSD          | Fast model loading |

---

## 📥 Model Download

**Recommended Quant**: `Qwen_Qwen3.6-35B-A3B-Q4_K_M.gguf` from bartowski (imatrix)

- Hugging Face: [bartowski/Qwen_Qwen3.6-35B-A3B-GGUF](https://huggingface.co/bartowski/Qwen_Qwen3.6-35B-A3B-GGUF)
- Size on disk: ~20–22 GB

---

## 🔥 Setup & Best Commands

### 1. Build llama.cpp (with TurboQuant support recommended)

```bash
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
make -j LLAMA_CUDA=1  # or your backend
```

### 2. Ultimate Launch Command

```bash
./llama-server \
  -m models/Qwen_Qwen3.6-35B-A3B-Q4_K_M.gguf \
  -c 131072 \
  --jinja \
  -ngl 99 \
  --n-cpu-moe 32 \           # ← Most important flag!
  --flash-attn \
  --no-mmap \
  --mlock \
  --cache-type-k q4_0 \
  --cache-type-v q4_0 \
  -b 512 \
  -ub 256 \
  -t 12 \
  --host 0.0.0.0 --port 8080
```

### Pro Flags Explained

| Flag                | Effect                                      | Why It Matters |
|---------------------|---------------------------------------------|----------------|
| `--n-cpu-moe 32`    | Offloads MoE experts to CPU                | Biggest speed boost |
| `-ngl 99`           | Max GPU layers                              | Best of both worlds |
| `--no-mmap + --mlock` | Keeps model in RAM                       | Prevents disk thrashing |
| TurboQuant KV       | Extreme context compression                 | 256K on 6GB VRAM |

---

## 📊 Performance Breakdown

| Configuration              | Tokens/sec | Context   | VRAM Used |
|---------------------------|------------|-----------|-----------|
| Default                   | ~3 t/s     | 8K        | ~5.8GB    |
| + `--n-cpu-moe`           | 10–12 t/s  | 32K       | ~5.2GB    |
| Full Optimizations        | **17–30 t/s** | **256K** | ~5.8GB    |

**Tested on**: GTX 1060 6GB + i7 + 32GB RAM

---

## ⚡ Advanced Tips & Troubleshooting

**💡 Pro Tips**:
- Use `--n-cpu-moe` aggressively (test 16–48)
- Try TurboQuant forks for even longer context
- Monitor with `nvidia-smi` and `htop`

**Common Issues**:
- **Slow loading** → Use `--no-mmap`
- **Out of memory** → Lower `--n-cpu-moe` or quant level
- **Poor quality** → Try Q5_K_M instead of Q4

---

## 🎯 Conclusion

With MoE + llama.cpp optimizations, **frontier-level AI is now accessible on decade-old hardware**. No cloud bills. Full privacy. Maximum fun.

**Happy inferencing!** 🚀

---

**Made with ❤️ for the local LLM community**  
*Last updated: June 2026*
