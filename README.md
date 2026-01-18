# 🚀 Ultimate Local AI Coding Setup (RTX 5090 + 9950X3D) - Windows 11 64-bit

This guide is tuned for elite performance (120+ tokens/s) while maintaining a massive 128k context window for large projects. This setup is specifically optimized for Windows 11 64-bit systems.

## 1. System Requirements & Prep
* **Node.js**: Install the LTS version from [nodejs.org](https://nodejs.org/).
* **PowerShell Permissions**: Run as Admin and execute:
  `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`.
* **OpenCode CLI**: Run `npm install -g opencode-ai@latest`.

## 2. The "Brain" (llama-server)
Use the **Qwen3-Coder-30B-A3B-Instruct-UD-Q6_K_XL** GGUF model.

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