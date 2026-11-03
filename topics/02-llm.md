# LLM Interview Questions & Answers — Foundations (Tokens, Embeddings, Transformers)

> Tokens, embeddings, context windows, sampling, training stages — the mental model you must have.

[◂ Back to the handbook](../README.md)

---

## What an LLM actually does: next-token prediction
_~7 min_

A large language model is, mechanically, a **next-token predictor**. Given a sequence of tokens, it outputs a probability distribution over the next token. That's it. Everything else — reasoning, coding, translation — is emergent behavior from doing this extremely well at scale over trillions of tokens.

#### Tokens, not words

Models don't see characters or words; they see **tokens** — sub-word chunks from a fixed vocabulary (~100k–256k for modern models). "tokenization" might be 2 tokens; a rare word or code symbol might be several. Rule of thumb: **~4 characters ≈ 1 token**, and **100 tokens ≈ 75 words** in English (so ~1.3 tokens per word — English text is a bit more than one token per word).

> 💡 **Why tokens matter in practice** — Cost and context limits are measured in **tokens**, not words. Tokenization explains classic failures: models struggle to count letters in a word ("how many r's in strawberry") because they never see letters — they see tokens.

#### The Transformer, in one breath

The architecture is the **Transformer** (Vaswani et al., 2017). Its key mechanism is **self-attention**: every token can look at every other token in the context and weigh how relevant each is. Stack many attention + feed-forward layers, train to predict the next token, and you get an LLM. You don't need to derive attention math for this role — but you must know **what** it does and **why** it's expensive (attention cost grows with the square of sequence length).

**◆ Lock it in**

- LLMs predict the **next token** from a probability distribution; capability is emergent from scale.
- Models operate on **tokens** (~4 chars each), not words or letters — this drives cost, limits, and quirks.
- The engine is the **Transformer**; **self-attention** lets every token weigh every other; cost scales ~quadratically with length.

> 🧠 **Feynman drill** — Explain why an LLM might fail to count the letters in a word, using the concept of tokens.

**Retrieval check**

**Q: Fundamentally, what is a language model computing at each step?**

<details><summary>Show answer</summary>

**✓ A probability distribution over the next token given the preceding tokens.**

At each step it outputs a distribution over the next token. Sampling from that distribution (repeatedly) produces text. It's not a lookup or a guaranteed-correct oracle — hence hallucinations.
</details>

---

## Embeddings & vector space
_~6 min_

An **embedding** is a vector (a list of numbers, e.g. 1536-dim) that represents the **meaning** of a piece of text. Texts with similar meaning land close together in this high-dimensional space; unrelated texts land far apart. "dog" and "puppy" are near; "dog" and "tax law" are far.

- You compute embeddings with an **embedding model** (a separate, cheaper model from the chat LLM).
- Similarity is measured with **cosine similarity** (angle between vectors) — 1 = identical direction, 0 = unrelated.
- Embeddings are the foundation of **semantic search** and **RAG**: embed a query, find the nearest document chunks by vector distance.

> 🧠 **Analogy** — Think of embeddings as GPS coordinates for meaning. Every sentence gets a location; "nearby" means "similar in meaning." Retrieval = "find the documents closest to where my question lands."

> 🎯 **Interview trap** — Don't confuse embeddings (meaning vectors, used for search/RAG) with **fine-tuning** (changing the model's weights). And embeddings are **not** the same as tokens — tokens are input units; embeddings are learned vector representations.

**◆ Lock it in**

- An embedding = a vector encoding **meaning**; similar meaning → nearby vectors.
- **Cosine similarity** measures closeness; it powers semantic search and RAG.
- Embedding models are separate and cheaper than chat models.

> 🧠 **Feynman drill** — Explain how you'd find the most relevant paragraph to a user's question using embeddings — no code.

**Retrieval check**

**Q: Two sentences have an embedding cosine similarity of 0.92. What does that imply?**

<details><summary>Show answer</summary>

**✓ They are semantically very similar in meaning.**

High cosine similarity ≈ close in meaning-space, i.e. semantically similar — even if the wording differs. It says nothing about identical grammar/tokens or fine-tuning.
</details>

---

## Context windows & why they rule your design
_~6 min_

The **context window** is the maximum number of tokens the model can attend to at once — its working memory. Everything must fit: system prompt + conversation history + retrieved documents + the user's message + room for the response (and, on reasoning models, the thinking tokens). In late 2026 **~1M tokens is the default** on frontier models (Claude 5.5 family, GPT-6, Gemini 4) and on leading open-weight models — but bigger windows didn't repeal the tradeoffs below.

- The **model** has no memory between calls — every response is computed from the tokens in the window. Many APIs now keep **server-side conversation state** for you (OpenAI Conversations, Gemini's Interactions API, Anthropic message threads), but that's **storage, not memory**: the full context is still processed (and billed, minus cache discounts) each turn, and stored state has **data-retention** implications.
- Bigger context ≠ free. More tokens = **more cost and higher latency**, and quality can degrade. Some providers add a **long-context surcharge** too (OpenAI doubles input price above 272K tokens).
- **Lost in the middle**: models attend best to the **start and end** of a long context; facts buried in the middle get missed. Put critical instructions/data at the edges.

> ⚠️ **Long context ≠ RAG replacement** — "Just stuff all the docs in the 1M window" fails on cost, latency, and the lost-in-the-middle effect at scale. Retrieval that sends only the **relevant** chunks is usually cheaper, faster, and more accurate. Know when each wins.

> 💡 **Name the discipline: context engineering** — Managing what's in the window is now a named craft — **context engineering** — and prompt engineering is framed as a subset of it. The four verbs: **Write, Select, Compress, Isolate**. Vocabulary interviewers recognize: **attention budget** (every token spends it), **context rot** (quality degrades as the window fills — measured across models), **compaction** (summarize/prune stale turns), **just-in-time retrieval** (fetch when needed, don't preload), **progressive disclosure** (load tool defs/docs on demand). Using this vocabulary instantly dates your knowledge to 2026. In 2026 parts of it became **API features** — server-side **compaction**, cache-preserving tool changes — so know both the concept and the managed version.

**◆ Lock it in**

- Context window = working memory; **everything** (system + history + docs + query + answer) shares it.
- The **model** is stateless; server-side conversation state is **storage** — the whole context is still processed and billed each turn.
- Watch **lost-in-the-middle**: put key instructions/data at the start or end.

> 🧠 **Feynman drill** — A colleague says 'we have a 1M-token model, so we don't need retrieval anymore.' Give two reasons they're wrong.

**Retrieval check**

**Q: Where should you place the single most important instruction in a very long prompt?**

<details><summary>Show answer</summary>

**✓ At the beginning or the end, because models attend best to the edges (lost-in-the-middle).**

The lost-in-the-middle effect means edge positions get the most reliable attention. Critical instructions belong at the start or end of a long context.
</details>

---

## Sampling, determinism & the knobs that disappeared
_~6 min_

The model outputs a probability distribution over the next token; **sampling** decides how you pick from it. You must know the classic knobs — and that on 2026's frontier reasoning models, most of them are gone.

- **Temperature**: scales randomness. **Low (~0–0.3)** = focused, best for extraction/classification/code. **High (~0.8+)** = varied, best for brainstorming. The range is provider-dependent (historically OpenAI 0–2, Anthropic 0–1).
- **Top-p (nucleus)**: sample only from the smallest set of tokens whose cumulative probability ≥ p. Tune temperature **or** top-p, not both.
- **Max output tokens**: caps length and cost — on reasoning models it must leave room for the thinking tokens too.
- **Stop sequences**: strings that force generation to halt.

> ⚠️ **2026 reality: the knobs are disappearing** — Frontier reasoning models increasingly **reject or ignore sampling parameters**: Claude Haiku 5.5 returns an error if you send them, GPT-6 Astra dropped them, and Gemini deprecated them (Gemini 4 ignores them). They still matter on many open-weight and older models — but 'just set temperature 0' is no longer a universal answer.

#### How you control variance now

- **Structured outputs**: constrain the shape at the API so format can't drift.
- **Tight prompts + examples**: fixed label sets, explicit fallbacks, few-shot demonstrations.
- **Reasoning effort**: the dial that replaced most sampling tuning — set per request (see 'Reasoning models, effort & test-time compute').
- **Validate + retry**: schema and business-rule checks on every output; retry or escalate on failure.
- **Sample k and aggregate** when it pays: majority vote / self-consistency for high-stakes labels — and measure run-to-run variance in your evals.

> 💡 **Determinism reality check** — Even where temperature 0 exists, LLMs are **not perfectly deterministic** in production (floating-point, batching, model updates). Never write code that assumes byte-identical outputs. Design for **validation and retries**, not exact reproducibility.

> 🎯 **Interview answer** — "For extraction I'd use structured outputs, validate fields against the schema and business rules, and retry or route to a human on failure. If the model exposes temperature I'd set it low — but on most 2026 frontier models the real levers are the schema, the prompt, and reasoning effort." That shows you know both the concept and the current APIs.

**◆ Lock it in**

- Know the classic knobs — **temperature, top-p, max tokens, stop sequences**: still interview vocabulary, still live on open-weight models.
- Frontier reasoning models increasingly **reject or ignore** sampling params; variance control moved to **structured outputs, prompts, reasoning effort, validation**.
- Temperature 0 was never a determinism guarantee — design for **validation + retries**.

> 🧠 **Feynman drill** — You're extracting invoice fields into JSON on a 2026 frontier model that rejects the temperature parameter. How do you get consistent output?

**Retrieval check**

**Q: You need consistent JSON extraction, and the model you're on rejects sampling parameters. Best approach?**

<details><summary>Show answer</summary>

**✓ Use structured outputs, validate against the schema and business rules, and retry or escalate on failure.**

Format consistency comes from schema-constrained output; correctness from validation with a retry/escalation path. Byte-identical outputs aren't guaranteed even at temperature 0, and prompting for determinism does nothing.
</details>

---

## How models are made: pretraining, SFT, RLHF & RLVR
_~6 min_

You won't train frontier models, but you must understand **how they're made** to reason about behavior, refusals, reasoning, and fine-tuning.

1. **Pretraining** — predict the next token over a huge web-scale corpus. Produces a **base model**: knowledgeable but not helpful/aligned; it just autocompletes.
2. **Supervised fine-tuning (SFT / instruction tuning)** — train on curated (prompt → ideal response) pairs so the model **follows instructions** and answers rather than autocompletes.
3. **RLHF / preference tuning (RLHF, DPO, RLAIF)** — humans (or AI) rank responses; the model is optimized toward preferred behavior. This produces the helpful, harmless, honest **chat model** you actually call.
4. **RL with verifiable rewards (RLVR)** — the 2025–26 frontier: reinforcement learning on tasks whose outcome can be **checked** (a math answer, tests passing, a tool-use task completed). This is what produced **reasoning models** that think before answering, and it's increasingly how labs — and some applied teams — teach agentic skills.

> 💡 **Why this matters for you** — Refusals, tone, and 'safety' behavior come from the alignment stage — you steer them with prompting, not retraining. **Knowledge** comes from pretraining (frozen at a cutoff date) — you add fresh/private knowledge with **RAG or tools**, not fine-tuning.

> ⚠️ **Knowledge cutoff** — The model's world knowledge stops at its **training cutoff**. For anything recent or private, it will confidently hallucinate. Ground it with retrieval or live tool calls.

**◆ Lock it in**

- Stages: **pretraining** (knowledge) → **SFT** (instruction-following) → **RLHF/DPO** (alignment) → **RLVR** (reasoning & agentic skill).
- The base model just autocompletes; alignment makes it a helpful assistant.
- Add **fresh/private knowledge** via RAG/tools, not fine-tuning; steer behavior via prompting.

> 🧠 **Feynman drill** — Explain the difference between what pretraining gives a model and what RLHF gives it.

**Retrieval check**

**Q: A model confidently makes up facts about your company's internal policy. The best fix is usually:**

<details><summary>Show answer</summary>

**✓ Ground it with retrieval (RAG) or a tool that fetches the real policy, and instruct it to only answer from provided context.**

Private/fresh knowledge isn't in pretraining. Retrieval or tools supply the facts; instructing 'answer only from context' curbs hallucination. Retraining/fine-tuning on web data won't add your private policy.
</details>

---

## Reasoning models, effort & test-time compute
_~7 min_

Every 2026 frontier model is a **reasoning model**: before answering it generates **thinking tokens** — a private scratchpad — and spending more of them buys accuracy on hard problems. This is **test-time compute**: trading latency and tokens at inference for quality, instead of reaching for a bigger model.

#### Reasoning became a parameter, not a model

- The separate reasoning models (OpenAI's o-series) were **retired in 2026**; reasoning is now a setting on general models.
- You dial it with an **effort** level (low → medium → high → max on Claude and GPT-6; `thinking_level` on Gemini). Fixed token budgets (`budget_tokens`, numeric thinking budgets) are legacy — Gemini 4 rejects them.
- Some models can't switch thinking off at all (e.g. Claude Opus 5.5) — **low effort** is your latency lever, not 'disable reasoning'.
- Thinking tokens are **billed as output** and add latency before the first visible token.

#### Choosing effort like an engineer

- **Low**: classification, extraction, routing, simple rewrites — latency-sensitive, high-volume paths.
- **Medium / high**: multi-step analysis, coding, agentic tool use where a wrong step is costly.
- **Max**: rare, hard, high-value problems — and only when your evals show it beats high.
- Decide with **evals, not vibes**: run the eval set at two effort levels and compare quality vs cost vs p95 latency.

> ⚠️ **Don't pay for thinking twice** — Telling a reasoning model to 'think step by step' and then parsing its verbose output is double-spending. Let it reason internally and constrain only the **final** answer with structured output.

> 🎯 **Say this** — "I treat reasoning effort as a per-route cost dial: low for the high-volume classification path, high for the agent's planning step — and I pick each level from an eval run that compares quality, cost and p95 latency."

**◆ Lock it in**

- Reasoning models spend **thinking tokens** (test-time compute) to buy accuracy — billed as output, adding latency.
- Reasoning is now an **effort parameter** on general models; the o-series and fixed thinking budgets are legacy.
- Pick effort **per route, from evals**: low for high-volume simple paths, high for costly multi-step decisions.

> 🧠 **Feynman drill** — Explain test-time compute to a PM, and how you'd decide which effort level each feature uses.

**Retrieval check**

**Q: A high-volume ticket-classification route on a reasoning model is too slow. Best first move?**

<details><summary>Show answer</summary>

**✓ Drop that route to low reasoning effort, confirm on the eval set that accuracy holds, and compare p95 latency and cost.**

Effort is the latency/cost dial on 2026 reasoning models: lower it for simple high-volume work and verify with evals that quality holds. Fixed thinking budgets are legacy (some models reject them), CoT prompting adds tokens, and more effort means more latency.
</details>

---

[◂ Back to the handbook](../README.md) · [▸ Practice this interactively](https://deployprep.vercel.app/track/fde)
