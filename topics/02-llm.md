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

The **context window** is the maximum number of tokens the model can attend to at once — its working memory. Everything must fit: system prompt + conversation history + retrieved documents + the user's message + room for the response. Modern models range from ~8K to **200K–1M+** tokens.

- The model has **no memory** between calls. Each API call is stateless — you resend the whole context every time. "Memory" in chat apps is you re-sending history.
- Bigger context ≠ free. More tokens = **more cost and higher latency**, and quality can degrade.
- **Lost in the middle**: models attend best to the **start and end** of a long context; facts buried in the middle get missed. Put critical instructions/data at the edges.

> ⚠️ **Long context ≠ RAG replacement** — "Just stuff all the docs in the 1M window" fails on cost, latency, and the lost-in-the-middle effect at scale. Retrieval that sends only the **relevant** chunks is usually cheaper, faster, and more accurate. Know when each wins.

**◆ Lock it in**

- Context window = working memory; **everything** (system + history + docs + query + answer) shares it.
- APIs are **stateless** — you resend history each call; that's what "memory" is.
- Watch **lost-in-the-middle**: put key instructions/data at the start or end.

> 🧠 **Feynman drill** — A colleague says 'we have a 1M-token model, so we don't need retrieval anymore.' Give two reasons they're wrong.

**Retrieval check**

**Q: Where should you place the single most important instruction in a very long prompt?**

<details><summary>Show answer</summary>

**✓ At the beginning or the end, because models attend best to the edges (lost-in-the-middle).**

The lost-in-the-middle effect means edge positions get the most reliable attention. Critical instructions belong at the start or end of a long context.
</details>

---

## Temperature, sampling & determinism
_~6 min_

The model outputs a probability distribution; **sampling parameters** decide how you pick from it.

- **Temperature** (range is provider-dependent — OpenAI 0–2, Anthropic 0–1): scales randomness. **Low (~0–0.3)** = focused, deterministic-ish, best for extraction/classification/code. **High (~0.8+)** = creative, varied, best for brainstorming/writing.
- **Top-p (nucleus)**: sample only from the smallest set of tokens whose cumulative probability ≥ p. Another way to control diversity.
- **Max tokens**: caps the response length (and cost).
- **Stop sequences**: strings that force generation to halt.

> 💡 **Determinism reality check** — Even at temperature 0, LLMs are **not perfectly deterministic** in production (floating-point, batching, model updates). Never write code that assumes byte-identical outputs. Design for **validation and retries**, not exact reproducibility.

> 🎯 **Interview answer** — "For a data-extraction task I'd use temperature 0 to minimize variance, ask for structured JSON, validate it against a schema, and retry on failure." That sentence signals production maturity.

**◆ Lock it in**

- **Temperature low** for precision tasks (extraction, code); **high** for creative tasks.
- Top-p, max tokens, and stop sequences are your other sampling controls.
- Temperature 0 is **not** a guarantee of identical output — design for validation + retries.

> 🧠 **Feynman drill** — You're extracting invoice fields into JSON. What sampling settings do you choose and why?

**Retrieval check**

**Q: You need consistent JSON extraction from documents. Best setting?**

<details><summary>Show answer</summary>

**✓ Temperature ~0, request structured output, validate against a schema, retry on failure.**

Extraction wants low variance: temperature ~0, structured output, and schema validation with retries — because even temp 0 isn't perfectly deterministic.
</details>

---

## How models are made: pretraining, SFT, RLHF
_~6 min_

You won't train frontier models, but you must understand the **three stages** to reason about behavior, refusals, and fine-tuning.

1. **Pretraining** — predict the next token over a huge web-scale corpus. Produces a **base model**: knowledgeable but not helpful/aligned; it just autocompletes.
2. **Supervised fine-tuning (SFT / instruction tuning)** — train on curated (prompt → ideal response) pairs so the model **follows instructions** and answers rather than autocompletes.
3. **RLHF / preference tuning (RLHF, DPO, RLAIF)** — humans (or AI) rank responses; the model is optimized toward preferred behavior. This produces the helpful, harmless, honest **chat model** you actually call.

> 💡 **Why this matters for you** — Refusals, tone, and 'safety' behavior come from the alignment stage — you steer them with prompting, not retraining. **Knowledge** comes from pretraining (frozen at a cutoff date) — you add fresh/private knowledge with **RAG or tools**, not fine-tuning.

> ⚠️ **Knowledge cutoff** — The model's world knowledge stops at its **training cutoff**. For anything recent or private, it will confidently hallucinate. Ground it with retrieval or live tool calls.

**◆ Lock it in**

- Three stages: **pretraining** (knowledge) → **SFT** (instruction-following) → **RLHF/DPO** (alignment).
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

[◂ Back to the handbook](../README.md) · [▸ Practice this interactively](https://YOUR-DOMAIN/track/fde)
