# 125B Local AI on a 12GB GPU: Strata + Pi 1.0 Coding Agent

![GPU](https://img.shields.io/badge/Tested%20on-RTX%203080%20Ti%2012GB-76b900?style=for-the-badge&logo=nvidia&logoColor=white)
![Model](https://img.shields.io/badge/Model-Qwen3.8--Flash--Next%20125B%20MoE-06b6d4?style=for-the-badge)
![Engine](https://img.shields.io/badge/Engine-Strata-f59e0b?style=for-the-badge)
![Cost](https://img.shields.io/badge/Cost-Free-brightgreen?style=for-the-badge)
![Cloud](https://img.shields.io/badge/Cloud-Not%20Required-red?style=for-the-badge)
![API](https://img.shields.io/badge/API-OpenAI%20Compatible-black?style=for-the-badge)

## 📺 The 125B-on-One-GPU Series

Run a **125B AI model locally** on a single consumer GPU with the free **Strata** engine. Every part is a real, hands-on test with configs and results.

| Part | Video | GPU | What you'll learn | Code |
|:---:|:---:|:---:|---|:---:|
| **1** | [![Part 1](https://img.youtube.com/vi/S5drxdKSE1s/mqdefault.jpg)](https://www.youtube.com/watch?v=S5drxdKSE1s)<br>[![Watch Now](https://img.shields.io/badge/YouTube-Watch%20Now-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/watch?v=S5drxdKSE1s) | **RTX 3090**<br>24GB | Run a 125B model on one GPU: Strata + Qwen 3.8 Flash-Next setup and first benchmarks | [![Repo](https://img.shields.io/badge/GitHub-Part%201-181717?style=for-the-badge&logo=github)](https://github.com/network-tocoder/Run-a-125B-AI-Model-on-One-GPU-Strata-Qwen3.8-Flash-Next) |
| **2** | [![Part 2](https://img.youtube.com/vi/QBPbvMaHkJc/mqdefault.jpg)](https://www.youtube.com/watch?v=QBPbvMaHkJc)<br>[![Watch Now](https://img.shields.io/badge/YouTube-Watch%20Now-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/watch?v=QBPbvMaHkJc) | **RTX 4060 Ti**<br>16GB | A free local AI coding agent: Strata + opencode through 4 coding levels and a racing-game boss | [![Repo](https://img.shields.io/badge/GitHub-Part%202-181717?style=for-the-badge&logo=github)](https://github.com/network-tocoder/free-local-ai-coding-agent-125b-on-16gb-gpu-strata-opencode) |
| **3** | [![Part 3](https://img.youtube.com/vi/c1DXLFLMRkk/mqdefault.jpg)](https://www.youtube.com/watch?v=c1DXLFLMRkk)<br>[![Watch Now](https://img.shields.io/badge/YouTube-Watch%20Now-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/watch?v=c1DXLFLMRkk) | **RTX 3080 Ti**<br>12GB | A new way to run Qwen 125B on 12GB V RAM: full setup, 75 tok/s writing, 2,035 tok/s reading, Pi 1.0 coding agent | [![Repo](https://img.shields.io/badge/GitHub-Part%203-181717?style=for-the-badge&logo=github)](https://github.com/network-tocoder/125B-Local-AI-on-a-12GB-GPU-Strata-Pi-1.0-Coding-Agent) |

### 🧭 Where should I start?
- **New to Strata?** Start with **[Part 1](https://www.youtube.com/watch?v=S5drxdKSE1s)** for the setup and the basics.
- **Want a local coding agent?** Jump to **[Part 2](https://www.youtube.com/watch?v=QBPbvMaHkJc)** (opencode).
- **Have only a 12GB GPU?** Go straight to **[Part 3](https://www.youtube.com/watch?v=c1DXLFLMRkk)** (Pi 1.0).

[![Subscribe](https://img.shields.io/badge/Subscribe-NetworkCoder-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/@NetworkCoder?sub_confirmation=1)

⭐ If this helped, **star the repo** so more people can find it.
## Hardware
| Part | Spec |
|---|---|
| GPU | RTX 3080 Ti 12GB |
| CPU | AMD Ryzen 9 5950X (16 cores) |
| RAM | 64 GB |
| Disk | NVMe SSD |

## How it fits
- ~40 GB of experts loaded into system RAM
- **3,161 hot experts (5.2 GB)** cached on the GPU
- CPU computes the expert misses
- Loaded: **11.6 / 12 GB VRAM**, **47 / 62 GB RAM**, 0 swap

## Speed
| Test | Result |
|---|---|
| Writing (800 tokens) | **74.7 tok/s** (MTP: 81% of drafted tokens accepted) |
| Reading / prefill (17,220 tokens) | **2,035 tok/s** (8.5 s) |
| Auto-tune best | 64.6 tok/s (`--pcie-frac 0.20 --spec-min-p 0.70`) |
| Context | 64K |
| Start-up | ~7 min first start, 3–4 min after (64GB RAM can't keep 40 GB cached) |

> A 3060 12GB has much lower memory bandwidth than a 3080 Ti, so expect lower speeds there.

## Results (Pi 1.0 + Swift 1.5 125B)
| Level | Task | Tests |
|---|---|---|
| 1 | Build a task manager app | 21/21 ✅ |
| 2 | Fix a bug + add due dates | 30/30 ✅ |
| 3 | Export/import + Docker + README | 38/38 ✅ |
| Boss | Single-file racing game | ✅ built + self-tested ("Apex Circuit") |

Notes:
- **Level 2:** during a smoke test the agent deleted the app's data file, noticed in its own reasoning, restored the tasks and disclosed it in the summary.
- **Boss:** an extra screenshot check got stuck in the Windows shell (heredoc) and had to be stopped; the game was already done.

Full data: [`results.csv`](results.csv)

## Setup
1. Install Strata and run its setup (Swift 1.5 → IQ3_XXS → 64K context → 8-bit KV):
   ```
   git clone https://github.com/Niko1221/Strata.git
   cd Strata && ./setup.sh --port 8082
   ```
   Let it auto-tune once; later starts use `./run-swift-iq3_xxs.sh`.
2. Install Pi 1.0:
   - Windows: `powershell -c "irm https://pi.dev/install.ps1 | iex"`
   - macOS/Linux: `curl -fsSL https://pi.dev/install.sh | sh`
3. Copy [`config/models.json`](config/models.json) to `~/.pi/agent/models.json`
4. Run in your project folder:
   ```
   pi --model strata/swift-125b
   ```

## RAM guide (from the Strata README)
| RAM | Build |
|---|---|
| 64 GB | IQ3_XXS (this video) |
| 48 GB | 2-bit builds (IQ2_XS / Q2_0) |
| 32 GB | Coder build |
