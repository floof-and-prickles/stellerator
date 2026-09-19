---
name: choose-claude-model
description: Decide which Claude model (Haiku/Sonnet/Opus/Fable) and which effort level (low/medium/high/xhigh/max) to use for a given task, or which model to route a delegated subagent/issue to. Use when picking a model for an API call, a Claude Code subagent's `model` field, a workflow step, or when triaging/classifying a task or issue by "which model class it needs". Triggers on terms like "which model should I use", "model selection", "effort level", "sonnet vs opus", "route to a subagent".
---

# Choosing a Claude model and effort level

Two independent knobs decide how a task gets handled: **which model** runs it, and
**how much effort** that model spends. Reach for effort first — it is cheaper to
tune and does not require re-justifying a tier change — and only escalate the
model when effort alone can't close the gap.

## 1. Pick a starting model

| Need | Model | Example use cases |
|---|---|---|
| Lowest latency/price, still strong reasoning | **Haiku 4.5** | High-volume/real-time work, cost-sensitive deployments, subagent/exploration tasks |
| Speed + capability for everyday work | **Sonnet 5** | Code generation, data analysis, agentic tool use, well-scoped single/few-file changes |
| Complex agentic coding, enterprise work | **Opus 5** | Multi-hour autonomous coding, large-scale refactors, complex systems engineering, design/architecture judgment calls |
| Highest available capability | **Fable 5.1** | Agent sessions running hours, multistep deep research, sustained reasoning/tool orchestration across many steps |

Two validated starting strategies (pick one, don't mix per-task):

- **Efficiency-first** (most tasks): start with Haiku or Sonnet, evaluate, upgrade
  only when you hit a measured capability gap.
- **Capability-first** (complex reasoning, scientific/mathematical work, high-autonomy
  agentic work): start with Opus, optimize prompts and effort, and only move to Fable
  if Opus still falls short at `xhigh`/`max` effort.

**Escalation rule:** upgrade the model when the current model had full context,
clearly tried, and still produced a confidently wrong answer — not speculatively
from a task description alone. Downgrade when work turns out to be routine,
without waiting for a quality regression to prove it's safe.

**Delegation rule (subagents, workflow steps, routing an issue to a model class):**
route mechanical, single-file, or grep-level work to the cheapest model that
handles it reliably (Haiku/Sonnet); reserve Opus/Fable for steps that require
multi-file reasoning, ambiguity, concurrency/ordering reasoning, platform-API
semantics, or a design-vs-spec judgment call. Keep final synthesis/ranking on the
top model in the pipeline.

## 2. Pick an effort level

Effort (`low` / `medium` / `high` / `xhigh` / `max`) trades intelligence for
latency and cost **within one model** — tune this before switching models.
It affects every output token: text, tool calls, and thinking. Lower effort
means fewer, terser tool calls and less exploration; higher effort means more
verification and more thorough cross-referencing of results.

| Level | Typical use case |
|---|---|
| `low` | Short, scoped, latency-sensitive tasks; the right default for subagents |
| `medium` | Balanced agentic work trading some capability for speed/cost |
| `high` (default) | Complex reasoning, difficult coding problems, agentic tasks |
| `xhigh` | Long-running agentic/coding work (30+ minutes), demanding tool orchestration |
| `max` | Frontier-difficulty problems only — usually adds cost for little quality gain elsewhere |

Practical guidance:

- Start at each model's default (`high`), and move up or down based on evals —
  don't guess.
- Increase **effort** if the model skipped files, avoided verification, or
  stopped short of a complete answer.
- Increase the **model** if the model confidently produced a wrong answer despite
  having full context and adequate effort.
- Hold effort constant within a single cached conversation/session where possible
  — changing it mid-conversation can invalidate prompt caching (some models
  support a per-message effort change that preserves the cache; check current
  model docs before relying on that).

## 3. Combined decision flow

1. Default to the cheapest model in the efficiency-first strategy, at that
   model's default effort.
2. If quality falls short, raise effort one level before changing model.
3. If effort alone doesn't close the gap and the failure involves cross-file
   reasoning, ambiguity, or design judgment, move up one model tier.
4. Reserve the top tier (Fable) for tasks that are long-horizon by nature —
   don't pre-classify a task into it from its title/description alone.
5. Re-run this evaluation per task/step rather than fixing one model class for
   an entire project — a multi-step pipeline (e.g. review → verify, or
   triage → fix) usually mixes tiers.

## Sources

- [Choosing the right model — Claude Platform Docs](https://platform.claude.com/docs/en/about-claude/models/choosing-a-model)
- [Effort parameter — Claude Platform Docs](https://platform.claude.com/docs/en/build-with-claude/effort)
- [Claude model and effort level in Claude Code — Anthropic blog](https://claude.com/blog/claude-model-and-effort-level-in-claude-code)
- [Selecting a model for a subagent — Claude Code Docs](https://code.claude.com/docs/en/sub-agents)
- [Introducing Claude Fable 5 and Mythos 5 — Anthropic](https://www.anthropic.com/news/claude-fable-5-mythos-5)

These are living docs; when in doubt, re-check the current pages above rather
than trusting numbers pinned in this file, since model names, tiers, and effort
defaults change across releases.
