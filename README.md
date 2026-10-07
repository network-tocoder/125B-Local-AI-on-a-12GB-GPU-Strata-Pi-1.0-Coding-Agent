# 125B Local AI on a 12GB GPU: Strata + Pi 1.0 Coding Agent

▶️ Video: *(link after upload)*
📺 Previous: [125B coding agent on a 16GB GPU](https://github.com/network-tocoder/Free-Local-AI-Coding-Agent-125B-on-16GB-GPU)

A 125B mixture-of-experts model (**Swift 1.5**, a fine-tune of Qwen3.8-Flash-Next, IQ3_XXS) running on a **12GB GPU** with the free **Strata** engine, driven by the **Pi 1.0** coding agent through 4 real coding levels. **$0 per token, fully local.**

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
