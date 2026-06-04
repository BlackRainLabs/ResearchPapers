# 🧠 MoE-LLM-Research Repository

![MoE Banner](https://img.shields.io/badge/Mixture_of_Experts-Research-blue?style=for-the-badge&logo=ai)
![VRAM Badge](https://img.shields.io/badge/Ultra_Low_VRAM-3GB%2B-critical?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**Pushing the boundaries of Mixture-of-Experts (MoE) LLMs on consumer & legacy hardware.**

Welcome to the **MoE-LLM-Research** repository by BlackRainLabs. This collection documents cutting-edge experiments, guides, and optimizations for running massive MoE models on extremely limited VRAM hardware.

---

## ✨ Project Highlights

- **Ultra-low VRAM MoE techniques** for 3GB–6GB GPUs
- Practical guides for **GTX 1060 3GB**, RTX 20xx/30xx series, and older cards
- TurboQuant + llama.cpp optimizations
- Real-world performance benchmarks
- Community-driven hardware exploration

**Goal**: Make frontier-level AI accessible to everyone — even on decade-old hardware.

---

## 📁 Repository Contents

| File | Description | Focus |
|------|-------------|-------|
| **[MoE-OLDHW.md](./MoE-OLDHW.md)** | Original research guide | General MoE on old hardware |
| **[MoE-OLDHW-02.md](./MoE-OLDHW-02.md)** | **GTX 1060 3GB Edition** | Ultra-optimized for 3GB VRAM + i7-7700 |
| **README.md** | This overview document | Repository navigation |

---

## 🚀 Featured Guide: MoE-OLDHW-02 (GTX 1060 3GB)

**The star of this repo** — A complete, battle-tested setup for running a **35B+ parameter MoE model** on just **3GB VRAM**.

### Key Achievements
- **Model**: Qwen3.6-35B-A3B-Uncensored (Q4_K_P)
- **Hardware**: GTX 1060 3GB + i7-7700 + 32GB RAM
- **Context**: 16K tokens
- **Speed**: Up to **20 tokens/second**
- **Stability**: Excellent with TurboQuant optimizations

[📖 Open MoE-OLDHW-02.md](./MoE-OLDHW-02.md)

---

## 🛠 Key Techniques Covered

- Heavy CPU expert offloading (`--n-cpu-moe`)
- TurboQuant KV cache (`turbo2` / `turbo3`)
- Conservative GPU layer loading (`-ngl`)
- Memory locking and no-mmap strategies
- Optimized batching and context settings

---

## 📊 Performance Gallery

**GTX 1060 3GB Results**:
- **8–20 tokens/sec** at 16K context
- VRAM usage consistently **under 2.9GB**
- Stable long-context conversations

---

## 🔗 Useful Links

- [Original MoE-OLDHW Research](./MoE-OLDHW.md)
- [llama.cpp TurboQuant Fork](https://github.com/BlackRainLabs/llama.cpp-turboquant) *(recommended)*
- [Qwen3.6-35B-A3B Model](https://huggingface.co/)

---

## 🤝 Contributing

Found a better flag combination? Achieved higher speeds on different hardware?  
Feel free to open issues or pull requests!

**Community focus**: Share your results with old GPUs!

---

## 📜 License

MIT License — Feel free to use, modify, and share.

---

**Made with ❤️ by Black Rain Labs for the local LLM & retro hardware community**

*Last updated: June, 4, 2026*  
**BlackRainLabs** — Democratizing AI
