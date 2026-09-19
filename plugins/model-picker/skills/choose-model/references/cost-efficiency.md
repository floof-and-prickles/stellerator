# Cost efficiency

Read this when cost/budget genuinely drives the decision. $/token at a given
effort level is only one of four factors — output-token verbosity, whether
you're paying by subscription or by the token, and prompt-cache hit rate can
each swing total cost as much as model or effort choice does.

## 1. Effort/reasoning scaling is steep and non-linear

Effort/reasoning-token multipliers are large on every provider measured so
far: GPT-5 at `high` effort uses ~23x the tokens (and cost) of `minimal`
effort, but most of that multiplier buys little — `medium`→`high` barely moves
the quality score compared to `minimal`→`medium` ([Artificial Analysis, GPT-5
benchmark analysis](https://artificialanalysis.ai/articles/gpt-5-benchmarks-and-analysis)).
Expect the same diminishing-returns shape elsewhere: the top one or two effort
levels on any given model usually buy little quality for a lot of cost — don't
reach for max effort by default.

For current cross-provider cost-vs-quality numbers, check [Artificial
Analysis's Intelligence Index and "Intelligence Index vs Cost per Task"
chart](https://artificialanalysis.ai/models) rather than trusting a pinned
table: $/task figures move constantly and vary a lot by benchmark
methodology, and searching for them surfaces a lot of SEO content quoting
inconsistent numbers for the same model — prefer a source that publishes its
methodology, and be skeptical of any single figure you can't trace back to
one.

## 2. Output-token efficiency is a separate axis from effort

A cheaper $/token model isn't cheaper overall if it's more verbose — cost per
*task* is what matters, not cost per token. Artificial Analysis tracks
weighted output tokens per benchmark task alongside its Intelligence Index and
has found only a **weak negative correlation between output-token volume and
success rate** — generating more text doesn't reliably mean better answers,
and verbosity varies a lot between models independent of how "smart" they are
(one small open model used ~2.8x the median token count on the same benchmark
suite for a below-median score). Where a model exposes a verbosity/length
control separate from reasoning effort (e.g. OpenAI's `verbosity` parameter,
or a max-output-tokens cap), tune it independently rather than assuming higher
effort or a bigger model implies more useful output.

## 3. Subscription vs metered API

Most providers relevant here sell both a flat-rate subscription (chat app or
CLI-integrated) and metered API billing, and coding-agent-specific
subscription "plans" are now their own product category (e.g. Claude Pro/Max,
ChatGPT Plus/Pro/Business with Codex included at the Pro tiers, Google AI
Pro/Ultra with higher Antigravity quotas, Zhipu's GLM Coding Plan, Moonshot's
Kimi plans). Rule of thumb: metered API tends to win when usage is low, when
cheap work can be routed to cheap models, or when building something serving
many users; a subscription tends to win for one person doing frequent
interactive/agentic work inside that provider's own tooling, since it turns a
highly variable metered bill into a fixed monthly cost. Check current pricing
pages before assuming a specific tier or quota — these change often and
faster than model lineups do.

## 4. Prompt caching materially changes the total

For agentic/coding workloads with large repeated context (system prompt, tool
definitions, codebase context resent every turn), cache hit rate often matters
more than model choice for total cost. As of this writing, Anthropic, OpenAI,
and Google all discount a prompt-cache **hit** to roughly **0.1x** the
standard input price (~90% off), with a smaller premium (~1.25x–2x standard
input) for the write that populates the cache. Some newer models get an even
steeper discount — Anthropic's own docs list Claude Fable 5.1/Mythos 5.1 cache
reads at 0.025x (~97.5% off) rather than the standard 0.1x — so check the
per-model rate rather than assuming the general figure. DeepSeek's cache
discount is steeper still across the board — cache hits have been reported
around **0.03x** standard input (~97% off). Structure prompts so the large, stable portion (system prompt,
tool/skill definitions, unchanging codebase context) comes first and stays
byte-identical across calls, and put the small, changing portion last, to
maximize cache hits — a session that breaks its own cache by reordering or
lightly editing the stable prefix on every turn can end up paying close to
uncached-input price throughout.

## Sources

- [Artificial Analysis — model comparisons](https://artificialanalysis.ai/models/comparisons)
- [Artificial Analysis — GPT-5 benchmarks and analysis](https://artificialanalysis.ai/articles/gpt-5-benchmarks-and-analysis)
- [Artificial Analysis — benchmarking methodology](https://artificialanalysis.ai/methodology)
- [Anthropic prompt caching docs](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- [OpenAI reasoning models guide (verbosity, cached input)](https://developers.openai.com/api/docs/guides/reasoning)
- [Subscription vs API cost calculator — AI Pricing Guru](https://www.aipricing.guru/subscription-vs-api/)
- [GLM Coding Plan pricing](https://codingplan.org/en/plans/glm)

Every dollar figure and discount rate here is a snapshot; pricing, quotas, and
plan names in this space change roughly monthly — verify against the current
docs above before making a budget decision on it.
