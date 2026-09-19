---
name: choose-model
description: Decide which model and effort/reasoning level to use for a given task, API call, subagent, or workflow step, across providers (Claude/Anthropic, OpenAI, Google Gemini, Chinese labs like DeepSeek/Qwen/Kimi/GLM, open-weight models like Llama/Mistral). Use when picking a model, tuning effort/reasoning_effort/thinking_level/thinking_budget/verbosity, routing a delegated subagent or issue to a model class, or triaging by "which model class it needs". Triggers on terms like "which model should I use", "model selection", "effort level", "reasoning effort", "thinking budget", "sonnet vs opus", "gpt-5 vs o-series", "deepseek vs qwen", "should I self-host", "route to a subagent".
---

# Choosing a model and effort level

Two independent knobs decide how a task gets handled: **which model** runs it, and
**how much effort/reasoning** it spends. Tune effort first — it's cheaper to change
and doesn't require re-justifying a tier switch — and only escalate the model when
effort alone can't close the gap. This holds across providers; only the model names
and the effort parameter's shape differ.

## 1. Identify the provider

Use whichever provider the task/project already targets. If genuinely unclear, ask
rather than guessing — don't infer it from the model names below, they go stale.
Note that self-hosting an open-weight model is itself a choice with real
tradeoffs (see [`references/open-weight.md`](references/open-weight.md#when-to-reach-for-these-vs-a-hosted-api-provider))
— don't default to it just because it's available.

## 2. Pick a starting model tier

Every provider in scope here exposes roughly this ladder. Read the matching
reference file for **exact current model names** — don't rely on memory, names and
tiers change across releases:

| Tier | Example use cases |
|---|---|
| Fastest/cheapest, still strong reasoning | High-volume/real-time work, cost-sensitive deployments, subagent/exploration tasks |
| Balanced speed + capability | Code generation, data analysis, agentic tool use, well-scoped single/few-file changes |
| Complex agentic/reasoning work | Multi-hour autonomous coding, large-scale refactors, complex systems engineering, design/architecture judgment calls |
| Highest available capability | Long-horizon agent sessions, multistep deep research, sustained reasoning/tool orchestration |

- **Claude/Anthropic:** read [`references/anthropic.md`](references/anthropic.md)
- **OpenAI:** read [`references/openai.md`](references/openai.md)
- **Google Gemini:** read [`references/gemini.md`](references/gemini.md)
- **Chinese labs (DeepSeek, Qwen, Kimi/Moonshot, GLM/Zhipu):** read [`references/chinese-models.md`](references/chinese-models.md)
- **Open-weight/self-hosted (Llama, Mistral, Devstral):** read [`references/open-weight.md`](references/open-weight.md)

Read only the file(s) for the provider(s) actually in play — that's what keeps
this skill cheap regardless of how many providers it ends up covering.

Two validated starting strategies (pick one, don't mix per-task):

- **Efficiency-first** (most tasks): start at the two cheapest tiers, evaluate,
  upgrade only when you hit a measured capability gap.
- **Capability-first** (complex reasoning, scientific/mathematical work,
  high-autonomy agentic work): start at the third tier, optimize prompts and
  effort, and only move to the top tier if it still falls short at max effort.

**Escalation rule:** upgrade the model when the current model had full context,
clearly tried, and still produced a confidently wrong answer — not speculatively
from a task description alone. Downgrade when work turns out to be routine,
without waiting for a quality regression to prove it's safe.

**Delegation rule (subagents, workflow steps, routing an issue to a model class):**
route mechanical, single-file, or grep-level work to the cheapest model that
handles it reliably; reserve the top two tiers for steps that require multi-file
reasoning, ambiguity, concurrency/ordering reasoning, platform-API semantics, or a
design-vs-spec judgment call. Keep final synthesis/ranking on the top model in the
pipeline.

## 3. Pick an effort/reasoning level

Every provider here exposes a knob that trades intelligence for latency/cost
**within one model** — tune this before switching models. Exact parameter name and
accepted values vary by provider/model (see the reference files); the shape is
consistent: lower effort means fewer/terser tool calls or reasoning tokens and less
exploration, higher effort means more verification and more thorough
cross-referencing.

Practical guidance:

- Start at the model's documented default effort, and move up or down based on
  evals — don't guess.
- Increase **effort** if the model skipped files, avoided verification, or
  stopped short of a complete answer.
- Increase the **model** tier if it confidently produced a wrong answer despite
  having full context and adequate effort.
- Hold effort constant within a single cached conversation/session where possible
  — changing it mid-conversation can invalidate prompt caching.

## 4. Combined decision flow

1. Default to the cheapest tier in the efficiency-first strategy, at that model's
   default effort.
2. If quality falls short, raise effort one level before changing model.
3. If effort alone doesn't close the gap and the failure involves cross-file
   reasoning, ambiguity, or design judgment, move up one model tier.
4. Reserve the top tier for tasks that are long-horizon by nature — don't
   pre-classify a task into it from its title/description alone.
5. Re-run this evaluation per task/step rather than fixing one model class for an
   entire project — a multi-step pipeline (e.g. review → verify, or triage → fix)
   usually mixes tiers, and may even mix providers.

These are living docs; when in doubt, re-check the current docs linked from each
reference file rather than trusting names/numbers pinned there, since model
lineups and effort defaults change across releases.
