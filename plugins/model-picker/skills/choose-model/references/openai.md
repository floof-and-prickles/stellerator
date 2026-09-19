# OpenAI models

## Model tiers

| Tier | Model | Example use cases |
|---|---|---|
| Fastest/cheapest | **gpt-5.6-luna** | High-volume/real-time work, cost-sensitive deployments, subagent/exploration tasks |
| Balanced | **gpt-5.6-terra** | Everyday agentic work, well-scoped single/few-file changes, cost/latency-optimizing after an accuracy target is met |
| Complex/agentic | **gpt-5.6-sol** | Complex professional tasks, prior-generation flagship-grade reasoning |
| Highest capability | **gpt-6-astra** | Establishing an accuracy ceiling, hardest end-to-end coding/reasoning/computer-use work |

Older families (`gpt-5.5`, `gpt-5.4` and its `-mini`/`-nano`/`-pro` variants, `gpt-5`
and its `-mini`/`-nano`/`-pro` variants, the `o1`/`o3`/`o4-mini` reasoning-only
series, `gpt-4.1`, `gpt-4o`) still work but are prior-generation; only reach for
them for compatibility with an existing integration, not for new work.

## Reasoning effort parameter

`reasoning.effort` (Responses API) / `reasoning_effort` (Chat Completions).
Accepted values and the default vary by model generation — check the model's own
docs page before relying on a value:

| Level | Typical use case |
|---|---|
| `none` / `minimal` | Fastest, lowest reasoning-token usage; latency-critical tasks (not supported on every model — e.g. flagship reasoning models may reject `none`) |
| `low` | Lightweight tasks that still benefit from brief reasoning |
| `medium` (commonly the default) | Balanced quality/reliability/performance starting point |
| `high` | Complex reasoning, difficult coding problems |
| `xhigh` / `max` | Longest, most thorough reasoning; not available on every model |

## Verbosity parameter

`text.verbosity` (or `verbosity`): `low` / `medium` / `high`. Independent of
reasoning effort — it scales the length/depth of the final answer (not the
reasoning) without changing the prompt. Use `low` for compact answers, `high` when
the response needs richer explanation or fuller context.

## Sources

- [Models — OpenAI API docs](https://developers.openai.com/api/docs/models)
- [Model selection guide — OpenAI API docs](https://developers.openai.com/api/docs/guides/model-selection)
- [Reasoning models — OpenAI API docs](https://developers.openai.com/api/docs/guides/reasoning)
- [Pricing — OpenAI API docs](https://developers.openai.com/api/docs/pricing)
- [Introducing GPT-5 for developers — OpenAI](https://openai.com/index/introducing-gpt-5-for-developers/)
