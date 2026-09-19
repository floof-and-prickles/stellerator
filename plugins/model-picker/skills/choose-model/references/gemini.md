# Google Gemini models

## Model tiers

| Tier | Model | Example use cases |
|---|---|---|
| Fastest/cheapest | **gemini-3.1-flash-lite** | Workhorse for cost-efficiency and high-volume tasks |
| Balanced | **gemini-3-flash-preview** | "Pro-level intelligence at the speed/pricing of Flash"; everyday agentic work |
| Complex/agentic | **gemini-3.1-pro-preview** | Complex tasks needing broad world knowledge, advanced reasoning, 1M-token context |
| Highest capability | **Gemini 3 Deep Think** | High-compute reasoning configuration for the hardest problems |

Model IDs currently carry a `-preview` suffix; expect that to drop once each
promotes to general availability — check the current model list before pinning
an ID in code.

## Thinking parameter

Gemini 3.x models use `thinking_level` (not the older `thinking_budget`):

| Value | Notes |
|---|---|
| `minimal` | "No thinking" for most queries — only available on Flash-tier models (e.g. `gemini-3-flash-preview`, `gemini-3.1-flash-lite`) |
| `low` | Minimizes latency and cost |
| `medium` | Balanced thinking for most tasks |
| `high` | Maximizes reasoning depth |

Default varies by model (e.g. `high` for `gemini-3.1-pro-preview`, `medium` for
Flash-tier models) — confirm in the docs rather than assuming.

Gemini 2.5-series models predate `thinking_level` and instead use
`thinking_budget`: an explicit token count (`0` disables thinking where
supported, `-1` enables dynamic/automatic budgeting).

## Sources

- [Gemini 3 developer guide — Google AI for Developers](https://ai.google.dev/gemini-api/docs/gemini-3)
- [Gemini thinking — Google AI for Developers](https://ai.google.dev/gemini-api/docs/generate-content/thinking)
- [Release notes — Gemini API](https://ai.google.dev/gemini-api/docs/changelog)
- [Google releases three new Gemini models — TechCrunch](https://techcrunch.com/2026/07/21/google-releases-three-new-gemini-models-but-no-3-5-pro/)
