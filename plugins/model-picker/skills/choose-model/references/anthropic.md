# Claude / Anthropic models

## Model tiers

| Tier | Model | Example use cases |
|---|---|---|
| Fastest/cheapest | **Haiku 4.5** | High-volume/real-time work, cost-sensitive deployments, subagent/exploration tasks |
| Balanced | **Sonnet 5** | Code generation, data analysis, agentic tool use, well-scoped single/few-file changes |
| Complex/agentic | **Opus 5** | Multi-hour autonomous coding, large-scale refactors, complex systems engineering, design/architecture judgment calls |
| Highest capability | **Fable 5.1** | Agent sessions running hours, multistep deep research, sustained reasoning/tool orchestration across many steps |

## Effort parameter

Values: `low` / `medium` / `high` (default) / `xhigh` / `max`. Affects every
output token — text, tool calls, and thinking.

| Level | Typical use case |
|---|---|
| `low` | Short, scoped, latency-sensitive tasks; the right default for subagents |
| `medium` | Balanced agentic work trading some capability for speed/cost |
| `high` (default) | Complex reasoning, difficult coding problems, agentic tasks |
| `xhigh` | Long-running agentic/coding work (30+ minutes), demanding tool orchestration |
| `max` | Frontier-difficulty problems only — usually adds cost for little quality gain elsewhere |

Some models support a per-message effort change that preserves prompt caching;
check current model docs before relying on that.

## Sources

- [Choosing the right model — Claude Platform Docs](https://platform.claude.com/docs/en/about-claude/models/choosing-a-model)
- [Effort parameter — Claude Platform Docs](https://platform.claude.com/docs/en/build-with-claude/effort)
- [Claude model and effort level in Claude Code — Anthropic blog](https://claude.com/blog/claude-model-and-effort-level-in-claude-code)
- [Selecting a model for a subagent — Claude Code Docs](https://code.claude.com/docs/en/sub-agents)
- [Introducing Claude Fable 5 and Mythos 5 — Anthropic](https://www.anthropic.com/news/claude-fable-5-mythos-5)
