# Open-weight / self-hosted models

Covers labs whose primary distribution is downloadable weights rather than a
vendor-hosted API. Several of the strongest current open-weight coding/agentic
models are actually Chinese (DeepSeek, Qwen, Kimi, GLM) — see
[`chinese-models.md`](chinese-models.md) for those; this file covers the
Western open-weight labs.

Because there's no single vendor API, cost/latency/effort controls depend on
**where you run it**: self-hosted, or via a third-party inference host
(e.g. Together AI, Fireworks, OpenRouter, Groq, AWS/Azure/GCP marketplaces).
Check the host's docs for pricing and any reasoning/effort parameter — the
model card only tells you what the weights themselves support.

## Model families

| Lab | Current line | Notes |
|---|---|---|
| **Meta Llama** | Llama 4 (incl. Llama 4 Scout) | Good for codebase Q&A, private-repo analysis, local/edge deployment, high-volume agentic pipelines where you want to avoid a vendor API entirely. |
| **Mistral AI** | Mistral Medium 3.5 (merged flagship — unifies former instruct/reasoning/coding lines), Mistral Small 4 (119B-total MoE, 6.5B active, Apache 2.0) | Mistral Small folds instruction-following, reasoning, vision, and agentic coding into one checkpoint. |
| **Mistral + All Hands AI** | Devstral (24B, 128K context, Apache 2.0) | Purpose-built for software-engineering agents, local codebase work, private repos. |

## When to reach for these vs. a hosted-API provider

- You need to keep code/data on infrastructure you control (compliance,
  air-gapped environments, IP-sensitive repos).
- You're running high-volume/low-latency pipelines where self-hosting or a
  cheap inference host beats per-token vendor pricing at scale.
- You want to fine-tune or otherwise modify the model itself.

Otherwise, a hosted-API provider (Claude, OpenAI, Gemini, or a Chinese lab's
own API) is usually less operational overhead for the same or better quality —
weigh that against the reasons above rather than defaulting to open weights.

## Sources

- [Best Open Source LLMs — Thunder Compute](https://www.thundercompute.com/blog/best-open-source-llms)
- [Open-Weights LLM Release History and Timeline](https://hidekazu-konishi.com/entry/open_weights_llm_release_history_and_timeline.html)

This list moves fast and is necessarily incomplete; check current leaderboards
(e.g. LM Arena, SWE-bench) and each lab's own release notes before committing
to a specific model for new work.
