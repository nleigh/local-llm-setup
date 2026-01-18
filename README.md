# 🚀 Ultimate Local AI Coding Setup (RTX 5090 + 9950X3D) - Windows 11 64-bit

This guide is tuned for elite performance (120+ tokens/s) while maintaining a massive 128k context window for large projects. This setup is specifically optimized for Windows 11 64-bit systems.

## 1. System Requirements & Prep
* **Node.js**: Install the LTS version from [nodejs.org](https://nodejs.org/).
* **PowerShell Permissions**: Run as Admin and execute:
  `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`.
* **OpenCode CLI**: Run `npm install -g opencode-ai@latest`.

## 2. Directory Setup Example

Here's an example directory structure for the setup:

```powershell
PS C:\AI> ls


    Directory: C:\AI


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        18/01/2026     12:29                llama-server
d-----        18/01/2026     16:01                models
-a----        18/01/2026     18:10           3302 llama-server-command.md


PS C:\AI> ls .\models\


    Directory: C:\AI\models


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        18/01/2026     13:02    32483935392 Qwen3-Coder-30B-A3B-Instruct-Q8_0.gguf
-a----        18/01/2026     13:34    26340328608 Qwen3-Coder-30B-A3B-Instruct-UD-Q6_K_XL.gguf


PS C:\AI> ls .\llama-server\


    Directory: C:\AI\llama-server


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        18/01/2026     12:29                cudart-llama-bin-win-cuda-12.4-x64
d-----        18/01/2026     12:29                llama-b7770-bin-win-cuda-12.4-x64
-a----        26/03/2024     03:39      100033536 cublas64_12.dll
-a----        26/03/2024     03:39      473551360 cublasLt64_12.dll
-a----        15/03/2024     15:46         553984 cudart64_12.dll
-a----        18/01/2026     07:16         674304 ggml-base.dll
-a----        18/01/2026     07:16         999936 ggml-cpu-alderlake.dll
-a----        18/01/2026     07:16        1234944 ggml-cpu-cannonlake.dll
-a----        18/01/2026     07:16        1222656 ggml-cpu-cascadelake.dll
-a----        18/01/2026     07:16        1223168 ggml-cpu-cooperlake.dll
-a----        18/01/2026     07:16        1003520 ggml-cpu-haswell.dll
-a----        18/01/2026     07:16        1229312 ggml-cpu-icelake.dll
-a----        18/01/2026     07:16         908800 ggml-cpu-ivybridge.dll
-a----        18/01/2026     07:16         911872 ggml-cpu-piledriver.dll
-a----        18/01/2026     07:16         889856 ggml-cpu-sandybridge.dll
-a----        18/01/2026     07:16        1492992 ggml-cpu-sapphirerapids.dll
-a----        18/01/2026     07:16        1227776 ggml-cpu-skylakex.dll
-a----        18/01/2026     07:16         728064 ggml-cpu-sse42.dll
-a----        18/01/2026     07:16         719360 ggml-cpu-x64.dll
-a----        18/01/2026     07:16        1230336 ggml-cpu-zen4.dll
-a----        18/01/2026     07:16      445071360 ggml-cuda.dll
-a----        18/01/2026     07:16         142336 ggml-rpc.dll
-a----        18/01/2026     07:16          95744 ggml.dll
-a----        18/01/2026     11:16         634936 libomp140.x86_64.dll
-a----        18/01/2026     07:16        6532608 llama-batched-bench.exe
-a----        18/01/2026     07:16         510976 llama-bench.exe
-a----        18/01/2026     07:16        8103424 llama-cli.exe
-a----        18/01/2026     07:16        6557184 llama-completion.exe
-a----        18/01/2026     07:16        6519808 llama-fit-params.exe
-a----        18/01/2026     07:16          27648 llama-gemma3-cli.exe
-a----        18/01/2026     07:16          60416 llama-gguf-split.exe
-a----        18/01/2026     07:16        6616576 llama-imatrix.exe
-a----        18/01/2026     07:16          27648 llama-llava-cli.exe
-a----        18/01/2026     07:16          27648 llama-minicpmv-cli.exe
-a----        18/01/2026     07:16        6552576 llama-mtmd-cli.exe
-a----        18/01/2026     07:16        6656512 llama-perplexity.exe
-a----        18/01/2026     07:16         356864 llama-quantize.exe
-a----        18/01/2026     07:16          27648 llama-qwen2vl-cli.exe
-a----        18/01/2026     07:16        9760256 llama-server.exe
-a----        18/01/2026     07:16         296960 llama-tokenize.exe
-a----        18/01/2026     07:16        6637568 llama-tts.exe
-a----        18/01/2026     07:16        2665472 llama.dll
-a----        18/01/2026     07:16         849408 mtmd.dll
-a----        18/01/2026     07:16         111616 rpc-server.exe
```

## 2. The "Brain" (llama-server)
Use the **Qwen3-Coder-30B-A3B-Instruct-UD-Q6_K_XL** GGUF model.

You can download this model from Hugging Face: [unsloth/Qwen3-Coder-30B-A3B-Instruct-GGUF](https://huggingface.co/unsloth/Qwen3-Coder-30B-A3B-Instruct-GGUF)

### Alternative Model: Nemotron-3-Nano-30B-A3B

Another excellent option is the **Nemotron-3-Nano-30B-A3B-GGUF** model, which offers different advantages:

#### Why Q5_K_M is the "Goldilocks" Choice

**VRAM Safety**: 26.1 GB (Weights) + ~3.4 GB (128k Context at Q4) + ~1.5 GB (Windows) = 31 GB. This utilizes nearly 98% of your 31.5 GB usable VRAM, giving you the maximum possible intelligence without crashing.

**Architecture Benefits**: Nemotron-3-Nano uses a Mixture-of-Experts (MoE) design with only ~3.6B active parameters, meaning it will feel as fast as a small model while retaining 30B-level reasoning.

**Quality**: Moving from Q4 to Q5 significantly reduces "perplexity" (errors) in complex reasoning and coding tasks.

### Optimized Launch Command
```powershell
.\llama-server.exe -m "C:\AI\models\Qwen3-Coder-30B-A3B-Instruct-UD-Q6_K_XL.gguf" `
  --port 8080 `
  --gpu-layers 99 `
  --parallel 1 `
  --ctx-size 131072 `
  --cache-type-k q4_0 `
  --cache-type-v q4_0 `
  --batch-size 4096 `
  --ubatch-size 1024 `
  --threads 8 `
  --threads-batch 16 `
  --flash-attn on `
  --mlock `
  --jinja `
  --no-mmap
```
Why these settings?
--parallel 1: Prevents VRAM crashes by dedicating all memory to your active task.

--ctx-size 131072: Grants a 128k window, enough to read entire project structures.

--cache-type-k/v q4_0: Compresses memory usage by ~50% to fit the large context into your 32GB VRAM.

--threads 8: Optimizes for the Ryzen's single-die core performance during token generation.

## 3. Connecting OpenCode
Create %USERPROFILE%\.config\opencode\opencode.json (ensure no .txt extension).

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "local/qwen3-coder",
  "provider": {
    "local": {
      "npm": "@ai-sdk/openai-compatible",
      "options": {
        "baseURL": "http://127.0.0.1:8080/v1"
      },
      "models": {
        "qwen3-coder": {
          "name": "Qwen3 Coder (5090)",
          "tools": true
        }
      }
    }
  }
}
```

# 🛠️ Troubleshooting & Optimization Guide

### 1. Error: "Shared GPU Memory" is climbing / Speed is slow
* **Cause**: Your Dedicated VRAM is full, forcing Windows to use slow System RAM.
* **Fix**: Ensure `--parallel` is set to `1`. If it still happens, reduce `--ctx-size` to `65536`.
* **Windows Tweak**: Disable "Hardware-Accelerated GPU Scheduling" (HAGS) in Windows Graphics settings, as it can hog 1-2GB of VRAM.

### 2. Error: "request exceeds the available context size"
* **Cause**: The project files being read are larger than the current `--ctx-size`.
* **Fix**: Increase `--ctx-size` to `131072`. If you still hit it, use the `/clear` command in OpenCode to reset the conversation history.

### 3. Error: "npm is not recognized" or "Scripts Disabled"
* **Cause**: Node.js isn't installed or PowerShell's security policy is too strict.
* **Fix**: Re-run the `Set-ExecutionPolicy` command in an Admin PowerShell window and restart your terminal.

### 4. Problem: Model is "hallucinating" tool calls (e.g., `<function=...>` instead of doing it
* **Cause**: The server is likely missing the `--jinja` flag or the model isn't receiving the correct template.
* **Fix**: Ensure `--jinja` is in your launch command. If using the Desktop UI, ensure the provider is set to "OpenAI" with the local URL.

### 5. Problem: llama-server crashes immediately on launch
* **Cause**: Usually an attempt to allocate more VRAM than the 5090 has (32GB).
* **Fix**: Check the terminal logs for `failed to fit params`. Lower the `--ctx-size` or switch from `q8_0` cache to `q4_0` cache.