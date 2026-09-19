# Chinese frontier models

The major Chinese labs ship at a very fast cadence (multiple point releases a
quarter) and — unlike Claude/OpenAI/Gemini — mostly publish **open weights**
alongside a hosted API, so the same model can be called through the vendor's
own API or self-hosted/rented from a third-party inference host (pricing and
latency then depend on the host, not just the model). If you plan to self-host
rather than call a vendor API, also see
[`open-weight.md`](open-weight.md).

## Model families

| Lab | Current flagship line | Notes |
|---|---|---|
| **DeepSeek** | `deepseek-v4-pro` (1.6T total / 49B active params), `deepseek-v4-flash` (284B total / 13B active) | MoE, 1M-token context, dual Thinking/Non-Thinking mode. `deepseek-chat`/`deepseek-reasoner` are retired — use the V4 model IDs. |
| **Alibaba Qwen** | Qwen3.6 family — `Qwen3.6-35B-A3B` for repo-scale agentic coding, dense `Qwen3.6-27B` for single-GPU self-hosting, `Qwen3-Coder-Next` (80B total / 3B active MoE) for local dev | Apache 2.0. Hosted via Alibaba Cloud Model Studio (DashScope); weights on Hugging Face/ModelScope. |
| **Moonshot AI (Kimi)** | Kimi K2 line, currently around `Kimi-K2.6` (~1T total / 32B active MoE) | Modified MIT License. Strong on SWE-bench-style agentic/tool-use benchmarks. API at platform.moonshot.ai is OpenAI/Anthropic-SDK compatible. |
| **Zhipu / Z.ai (GLM)** | GLM-5.x line, e.g. `GLM-5.2` (744B total / 40B active MoE, 1M context) | License varies by release (MIT or Apache 2.0 on most, but not universal — check each model card). API at z.ai (international) or open.bigmodel.cn (China). |

These version numbers move fast — check each lab's own model list before
assuming a specific point release is still current or still the recommended
default.

## Reasoning/thinking controls

There's no single standardized parameter across these labs (unlike the
effort/reasoning_effort/thinking_level convergence among Claude, OpenAI, and
Gemini). Each exposes its own thinking toggle, usually binary or a small set
of modes rather than a graduated scale:

- **DeepSeek:** `extra_body={"thinking": {"type": "enabled"}}` on an
  OpenAI-SDK-style call (some deployments also accept a `reasoning_effort`
  string); response reasoning comes back in `reasoning_content`.
- **Qwen / DashScope:** `enable_thinking` (boolean) to turn reasoning on/off
  per request; hybrid Qwen3.5+ models default it **on**. Some deployments also
  expose `thinking_budget` / `reasoning_effort`.
- **Kimi / GLM:** check the current API docs — these labs iterate this surface
  frequently and it isn't consistently documented across releases.

## Sources

- [DeepSeek V4 Preview Release — DeepSeek API Docs](https://api-docs.deepseek.com/news/news260424/)
- [Thinking Mode — DeepSeek API Docs](https://api-docs.deepseek.com/guides/thinking_mode/)
- [deepseek-ai/DeepSeek-V4-Pro — Hugging Face](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro)
- [Qwen3-Coder-Next — Qwen official blog](https://qwen.ai/blog?id=qwen3-coder-next)
- [Use deep thinking models via API — Alibaba Cloud Model Studio](https://www.alibabacloud.com/help/en/model-studio/deep-thinking)
- [Alibaba unveils Qwen3.5 — CNBC](https://www.cnbc.com/2026/02/17/china-alibaba-qwen-ai-agent-latest-model.html)
- [Kimi K2: Open Agentic Intelligence — Moonshot AI](https://moonshotai.github.io/Kimi-K2/)
- [moonshotai/Kimi-K2.6 — Hugging Face](https://huggingface.co/moonshotai/Kimi-K2.6)
- [GLM (AI) — Wikipedia](https://en.wikipedia.org/wiki/GLM_(AI))
- [Z.ai — Wikipedia](https://en.wikipedia.org/wiki/Z.ai)

These move faster than the other reference files here; treat the specific
model IDs and parameter names above as a starting point to verify, not as
settled fact.
