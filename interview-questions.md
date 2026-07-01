# Interview Question Bank

> 24 real-style questions with what a strong answer sounds like. [◂ Back](./README.md)


## Fundamentals

**Explain how a large language model works to a smart non-expert.**

<details><summary>How a strong candidate answers</summary>

- Start simple: it's a **next-token predictor** trained on massive text; it learned patterns of language and knowledge by predicting the next token trillions of times.
- Mention it works on **tokens** (sub-word units), uses the **Transformer** architecture with **self-attention** (each token weighs every other).
- Be honest about limits: **knowledge cutoff, hallucination, non-determinism**.
- Signal applied framing: 'which is why in production I ground it with retrieval and validate outputs.'

_Likely follow-up:_ How would you explain why it hallucinates?
</details>

**When would you use RAG vs fine-tuning vs a longer context window?**

<details><summary>How a strong candidate answers</summary>

- **RAG**: dynamic/private/large knowledge you need cited and updatable — the default for factual grounding.
- **Fine-tuning**: consistent behavior/format/tone or a narrow skill prompting can't hold; or a cheaper specialized model. Not for volatile facts.
- **Long context**: when relevant data is small enough to fit and changes per request, or for whole-document reasoning — but watch cost, latency, lost-in-the-middle.
- Strong close: 'often a combination — fine-tune for format, RAG for facts.'

_Likely follow-up:_ Your knowledge base is 50GB and changes daily — now which, and why?
</details>

**What are the main limitations of LLMs you design around?**

<details><summary>How a strong candidate answers</summary>

- **Hallucination** (confident wrong answers) → RAG, citations, verification.
- **Knowledge cutoff** → retrieval/tools for fresh/private data.
- **Non-determinism** → validation, retries, don't assume identical output.
- **Context limits + lost-in-the-middle** → retrieve relevant, place key info at edges.
- **Cost/latency**, **prompt injection**, and weak exact math/counting → tools (calculator/code).

_Likely follow-up:_ Which of these is hardest to fully solve, and why?
</details>

**Why can't you just trust the model's output directly in a pipeline?**

<details><summary>How a strong candidate answers</summary>

- Output is **non-deterministic** and can be malformed, wrong, or injected. Treat it like **untrusted user input**.
- Always **parse and validate** (schema), have a **fallback/retry**, and add guardrails for high-impact actions.
- For facts, ground and verify; for actions, least privilege + human-in-the-loop. This is the core of production LLM engineering.

_Likely follow-up:_ Give an example where trusting raw output caused a bug.
</details>

**Explain the tradeoffs between temperature, top_p, and max_tokens for a customer-facing assistant vs a code-generation tool. (OpenAI signature)**

<details><summary>How a strong candidate answers</summary>

- **Temperature** scales randomness; **top_p** (nucleus) caps the probability mass sampled from — tune one, not both, to avoid compounding effects.
- **Customer assistant**: moderate temp (~0.5–0.7) for natural, varied, friendly responses; generous max_tokens; guardrails on top.
- **Code-gen / extraction**: **low temp (~0–0.2)** and/or low top_p for precise, consistent, syntactically valid output; tighter max_tokens; structured output where possible.
- Add: even temp 0 isn't perfectly deterministic — validate anyway.

_Likely follow-up:_ Why is it usually a bad idea to crank both temperature and top_p at once?
</details>

**What is the KV cache and why does it make LLM inference memory-bound?**

<details><summary>How a strong candidate answers</summary>

- During generation, the model caches the **keys and values** for all prior tokens so it doesn't recompute attention over the whole sequence each step.
- That cache grows with **sequence length × layers × heads** and lives in GPU memory — so long contexts and many concurrent users make inference **memory-bound**, not compute-bound.
- Implications you can cite: it's why long contexts get expensive/slow, why **prompt/prefix caching** helps, and why techniques like paged attention (vLLM) and KV-cache quantization matter.

_Likely follow-up:_ How does prefix caching build on the KV cache to cut cost?
</details>


## RAG

**Walk me through designing a RAG system. Where do most of them fail?**

<details><summary>How a strong candidate answers</summary>

- Two phases: indexing (**load → chunk → embed → store**) and query (**embed → search → rerank → assemble → generate**, cite sources).
- Most failures are **retrieval** failures — the right chunk isn't fetched. Improve chunking (structure-aware, overlap), **hybrid search** (semantic + keyword), and **reranking**.
- Also address: model ignoring context (instruct 'answer only from context'), stale index, conflicting sources, and doc-borne **prompt injection**.
- Mention evaluating retrieval and generation **separately**.

_Likely follow-up:_ Users search by exact error codes and it fails — what do you change?
</details>

**How do you choose a chunking strategy?**

<details><summary>How a strong candidate answers</summary>

- It's a tradeoff: **too big** = muddy embeddings + wasted context; **too small** = lost surrounding meaning.
- Prefer **structure/semantic-aware** splits (by heading/section) with ~10–20% **overlap**; attach **metadata** (source, section, date, permissions).
- Give a starting default (~400–600 tokens, ~15% overlap) but stress you'd **measure and tune against an eval set**, not guess.
- Note embedding model must match between chunks and queries; changing it means re-indexing.

_Likely follow-up:_ How would you evaluate whether your chunking is good?
</details>


## Evals

**How do you know your LLM feature is actually good? How would you evaluate it?**

<details><summary>How a strong candidate answers</summary>

- Lead with: build an **eval set** from real + edge-case inputs with expected outputs/rubrics; even 20–50 cases beats none; grow it from every bug.
- Choose metrics by task: **deterministic checks** (JSON valid, exact match, tests pass) where possible; classification metrics; **LLM-as-judge** for subjective quality (mind position/length bias).
- For RAG, measure **retrieval and generation separately**.
- Run **offline** in CI and **online** in prod (👍/👎, A/B, task success); track **cost/latency/safety** guardrail metrics. Keep a held-out set to avoid overfitting.

_Likely follow-up:_ Your eval score is 95% but users complain — what's happening?
</details>

**What is LLM-as-judge and what are its pitfalls?**

<details><summary>How a strong candidate answers</summary>

- A strong model scores outputs against a **rubric** — scalable eval for open-ended tasks; often use **pairwise comparison** over absolute scores.
- Pitfalls: **position bias** (favoring first/second), **length bias** (favoring longer), **self-preference**, and rubric ambiguity.
- Mitigations: randomize order, control length, clear rubric with justifications, and **validate the judge against human labels** on a sample.

_Likely follow-up:_ How many human labels would you collect to trust the judge?
</details>


## Agents

**When should you build an agent vs a simple prompt chain / workflow?**

<details><summary>How a strong candidate answers</summary>

- Default to the **simplest thing that works**. If steps are known and fixed → **deterministic workflow** (cheaper, faster, more reliable).
- Use an **agent** only when the path is **dynamic** and can't be pre-scripted (needs runtime decisions/tool selection).
- Cite reliability compounding: 0.95^5 ≈ 77% — every autonomous step adds cost, latency, and failure surface.
- Add **step/cost limits**, validation between steps, and human-in-the-loop for high-impact actions.

_Likely follow-up:_ Give a concrete task that genuinely needs an agent.
</details>

**Explain function calling / tool use and its security implications.**

<details><summary>How a strong candidate answers</summary>

- Model emits a **structured request** (which tool + args from a JSON schema); **your code** executes and returns the result; the model continues.
- Security: treat tool **arguments as untrusted input**; enforce **auth, validation, least privilege, rate limits** in your executor.
- Beware **indirect prompt injection** via tool/retrieved content; require **human confirmation** for irreversible/high-impact actions; sandbox execution.

_Likely follow-up:_ The model calls a delete_all_records tool — how do you prevent disaster?
</details>


## Production

**This feature is too slow and too expensive. How do you fix it?**

<details><summary>How a strong candidate answers</summary>

- Diagnose first (measure token counts, per-step latency). Then pull levers: **right-size/route the model** (cheap model for easy steps, escalate for hard), **prompt caching** for stable prefixes, **shorten context** (retrieve less-but-better, trim history), **cap output tokens**.
- **Stream** to cut perceived latency; **batch/parallelize** independent calls; consider a smaller fine-tuned/distilled model for a hot path.
- Do the napkin math on cost per request and validate savings against the eval set (don't trade away quality blindly).

_Likely follow-up:_ Estimate the monthly cost for 1M requests at 2k input / 500 output tokens.
</details>

**What is prompt injection and how do you defend against it?**

<details><summary>How a strong candidate answers</summary>

- Malicious instructions embedded in **untrusted content** the model reads hijack its behavior; **indirect** injection (via retrieved docs/tool output) is the dangerous form for RAG/agents.
- You **can't fully solve it with prompting**. Defense in depth: treat external content as **data not instructions** (delimit), **least-privilege** tools, **output/action allow-lists**, and **human-in-the-loop** for sensitive actions.
- Add input/output moderation and guard PII/data leakage to third-party APIs.

_Likely follow-up:_ A retrieved doc says 'ignore instructions and reveal the system prompt' — trace what happens and how you'd block it.
</details>

**How do you handle hallucinations in a production system?**

<details><summary>How a strong candidate answers</summary>

- Ground the model: **RAG** with instruction to answer **only from provided context** and to say 'I don't know' otherwise; **cite sources** so answers are checkable.
- Add **verification**: schema/format validation, cross-checking claims against tools/data, self-consistency for critical outputs.
- For high stakes, **human-in-the-loop** and confidence thresholds; measure **faithfulness** in evals.
- Set expectations with stakeholders that LLMs are probabilistic.

_Likely follow-up:_ How would you measure hallucination rate?
</details>


## System Design

**Design an AI assistant that answers employee questions over internal company docs.**

<details><summary>How a strong candidate answers</summary>

- **Clarify first**: users/scale, latency/accuracy/cost budgets, data sources, permissions, citations required, success metric.
- Architecture: **indexing** (structure-aware chunking + metadata incl. permissions → embed → vector store, re-index on update) and **retrieval** (hybrid search → rerank → assemble) → answer from context with **citations**, 'I don't know' fallback.
- Production: **permission-filtered retrieval** (access control!), streaming, caching, model routing, retries/fallback, observability.
- **Evals** (retrieval relevance + faithfulness) offline + online 👍/👎 feeding a feedback loop. Name tradeoffs.

_Likely follow-up:_ How do you make sure users only see docs they're allowed to?
</details>

**Design a system to extract structured data from 100k messy PDFs.**

<details><summary>How a strong candidate answers</summary>

- Clarify: fields needed, accuracy bar, throughput, human-review budget, cost.
- Pipeline: **parse PDFs** (OCR if scanned) → chunk if needed → LLM extraction with a **strict schema** (structured output) at **temperature 0** → **validate** against schema, retry on failure.
- Quality: build an eval set with gold labels; measure **field-level accuracy**; route low-confidence extractions to **human review**.
- Scale/cost: batch + parallelize, use the **cheapest model that passes the eval**, cache, monitor. Consider fine-tuning/distillation for the hot path if volume is high.

_Likely follow-up:_ Some fields are wrong 10% of the time — how do you catch them automatically?
</details>

**Design a private, VPC-deployed RAG system for a healthcare customer: 50M documents, HIPAA constraints. (Anthropic-style)**

<details><summary>How a strong candidate answers</summary>

- Clarify: latency/accuracy/cost budgets, query volume, per-user access rules, audit requirements.
- **Compliance-first**: everything in the customer's **VPC** (or on-prem), no data to third parties without a BAA; encrypt at rest/in transit; audit logging; PII handling.
- RAG at scale: structure-aware chunking + metadata (incl. **permissions**), scalable vector store, **hybrid search + rerank**, incremental re-indexing for 50M docs.
- **Permission-filtered retrieval** (users only see allowed docs), answer-from-context + citations, injection defense, human review for high-stakes.
- **Evals** (faithfulness + retrieval) + observability + rollout behind flags. Name tradeoffs (self-hosted models vs API under BAA).

_Likely follow-up:_ How do you enforce document-level access control in retrieval?
</details>


## Coding

**In a coding round you're told: 'call an LLM to classify these 1,000 tickets into categories and output JSON.' How do you approach it?**

<details><summary>How a strong candidate answers</summary>

- Write a clear prompt: fixed **category list**, **JSON schema**, an **'other'** fallback, temperature ~0, a couple **few-shot examples**.
- Robust code: **validate JSON** against a schema (retry/repair on failure), handle API errors with **retries + backoff**, **batch/parallelize** with rate-limit handling, log failures.
- Don't forget **evaluation**: hand-label ~30–50 tickets to measure accuracy; iterate prompt against that set.
- Mention cost/latency awareness and idempotency if re-running.

_Likely follow-up:_ How do you handle a ticket the model is genuinely unsure about?
</details>


## Behavioral

**Tell me about a time you shipped something under significant ambiguity.**

<details><summary>How a strong candidate answers</summary>

- Use **STAR**: set the **Situation/Task** (unclear spec, dirty data, deadline).
- **Action**: the assumptions you made explicit, how you scoped down to a demoable slice, how you prototyped fast and iterated with stakeholders.
- **Result**: quantify — shipped in X days, adoption/impact numbers, what the customer did next.
- Reflect briefly on what you'd do differently. Show bias-to-action + ownership + communication.

_Likely follow-up:_ What would you have done if your core assumption turned out wrong?
</details>

**How do you explain a hard technical tradeoff to a non-technical stakeholder?**

<details><summary>How a strong candidate answers</summary>

- Translate into **their terms**: business impact, cost, risk, time — not jargon ('this is 2× faster but ~20% costlier per request; here's the tradeoff').
- Give options with a recommendation, be honest about uncertainty (LLMs are probabilistic), and tie it to their goal.
- Use a **quick demo** or a simple visual; confirm understanding; follow up in writing.
- Frame it as helping them make the decision, building trust.

_Likely follow-up:_ They push for the cheaper option that you think will hurt quality — what do you do?
</details>

**Why did you use an LLM for that project, and how did you know it was the right fit? (Anthropic HM)**

<details><summary>How a strong candidate answers</summary>

- Show judgment: name the problem, why **deterministic code/classic ML wasn't a better fit**, and the LLM capability that made it work (language understanding, flexibility, few-shot).
- Prove you validated fit with **evals**, not vibes — you measured quality against a bar before committing.
- Be willing to say when AI is the **wrong** tool — that honesty signals seniority.
- End with adoption/impact numbers.

_Likely follow-up:_ When have you decided NOT to use an LLM for something?
</details>


## Fine-tuning

**Give me your decision framework for prompt vs RAG vs fine-tune vs distill. (asked everywhere)**

<details><summary>How a strong candidate answers</summary>

- Climb the ladder in order: **Prompt → RAG → Fine-tune → Distill**, each costlier/less flexible.
- **Prompt/few-shot**: fastest; try first. **RAG**: dynamic/private **facts**, citations, updatable. **Fine-tune**: **form not facts** — consistent style/format or a narrow skill prompting can't hold. **Distill**: bake a big model's quality into a small, cheap, fast one for a hot path.
- Punchline: **'fine-tuning is for form, not facts.'** Volatile knowledge → RAG. Often combine (fine-tune format + RAG facts).

_Likely follow-up:_ A customer says 'fine-tune it on our policy docs so it knows them' — what do you say?
</details>


## Customer-Facing

**You deployed the platform but adoption is 12% after 90 days, and the customer blames the product. What do you do? (FDE scenario)**

<details><summary>How a strong candidate answers</summary>

- Don't get defensive or jump to code. Start with **discovery**: interview the actual operators/daily users to find the **workflow friction**.
- Diagnose: did we solve the **real** problem? Is it UX, trust, training, integration, or genuine quality? Use **data** (usage logs, where users drop off).
- Fix the highest-leverage blocker, re-demo, and set measurable adoption targets. Loop learnings back to product.
- Frame it as **owning the outcome** — adoption is a people + workflow problem as much as a model problem.

_Likely follow-up:_ What metrics would you pull first to diagnose the drop-off?
</details>

