# Rapid-Fire Flashcards

> 70 spaced-repetition cards. Cover the answer, recall, then check. [◂ Back](./README.md)


## The Role: Applied AI & Forward Deployed Engineer

**One-line definition of the AAE/FDE role?**
<details><summary>Answer</summary>

You take a general-purpose model and make it **useful** — ship products on top of foundation models against a real customer's problem. You don't train frontier models.
</details>

**AAE/FDE vs Research Engineer?**
<details><summary>Answer</summary>

Research improves the **model itself**. You **apply** an existing model — retrieval, tools, evals, guardrails — and own delivery of a reliable product.
</details>

**What are practical coding rounds for this role like?**
<details><summary>Answer</summary>

**Not abstract leetcode**: call an LLM API, build a small RAG or tool-calling loop, parse/validate output, wrangle data — usually in Python.
</details>

**The #1 and #2 universal interview signals for AAE/FDE?**
<details><summary>Answer</summary>

1) **Evals** — 'how do you know it works?' (golden set, LLM-as-judge, regression gates). 2) **Start with the customer, not the tech** — clarify before architecting.
</details>

**Why did the FDE role explode? (great 'why FDE' material)**
<details><summary>Answer</summary>

Most enterprise AI pilots fail on **integration, not models** — someone must close the last mile. FDE = **product discovery**: 'build the gravel road' with one customer; product paves the highway. Postings grew ~8× YoY.
</details>


## LLM Foundations

**What does an LLM fundamentally compute at each step?**
<details><summary>Answer</summary>

A probability distribution over the **next token** given the preceding tokens. Sampling from it repeatedly produces text.
</details>

**Rule of thumb: tokens ↔ words / chars?**
<details><summary>Answer</summary>

**100 tokens ≈ 75 words** (so ~1.3 tokens/word), and **~4 characters ≈ 1 token** in English. Cost & context limits are counted in tokens.
</details>

**What is an embedding?**
<details><summary>Answer</summary>

A **vector** representing the **meaning** of text. Similar meaning → nearby vectors. Measured by **cosine similarity**. Basis of semantic search & RAG.
</details>

**What is the context window and what shares it?**
<details><summary>Answer</summary>

The max tokens the model can attend to at once. Shared by **system prompt + history + retrieved docs + user message + response**. APIs are stateless.
</details>

**What is 'lost in the middle'?**
<details><summary>Answer</summary>

Models attend best to the **start and end** of a long context; info in the middle is often missed. Put critical instructions/data at the edges.
</details>

**Temperature: low vs high?**
<details><summary>Answer</summary>

**Low (0–0.3)** = focused/deterministic-ish → extraction, classification, code. **High (0.8+)** = creative/varied → brainstorming, writing. Temp 0 ≠ perfectly deterministic.
</details>

**The three model training stages?**
<details><summary>Answer</summary>

**Pretraining** (next-token on web = knowledge), **SFT** (instruction-following), **RLHF/DPO** (alignment/preference). Knowledge is frozen at a cutoff.
</details>

**Self-attention: what are Q, K, V?**
<details><summary>Answer</summary>

Each token makes a **Query**; it's matched against every token's **Key** to get attention weights; those weight each token's **Value**. **Multi-head** = several attention patterns in parallel.
</details>

**Why are many frontier models Mixture-of-Experts (MoE)?**
<details><summary>Answer</summary>

MoE routes each token to a few **expert** sub-networks, so total parameters are huge but only a fraction (**active params**) run per token — big capacity at lower inference compute. Tradeoff: routing complexity and memory to hold all experts.
</details>

**What is context engineering?**
<details><summary>Answer</summary>

The umbrella discipline (prompting is a subset): curate everything in the window via **Write / Select / Compress / Isolate**. Vocab: **attention budget, context rot, compaction, just-in-time retrieval, progressive disclosure**.
</details>


## Prompt Engineering

**Five sections of a robust production prompt?**
<details><summary>Answer</summary>

**Role, task instructions, context (delimited), output format, guardrails** (how to handle unknowns). State what NOT to do.
</details>

**Zero-shot vs few-shot?**
<details><summary>Answer</summary>

Zero-shot = instructions only. **Few-shot** = include 2–8 input→output examples; biggest lever after clear instructions for format & edge cases.
</details>

**Why does chain-of-thought improve accuracy?**
<details><summary>Answer</summary>

The generated reasoning tokens act as a **scratchpad**, letting the model work through intermediate steps before committing to a multi-step answer.
</details>

**Most robust way to get valid structured output?**
<details><summary>Answer</summary>

**Schema-constrained / structured output** at the API level. Otherwise: instruct schema → parse → **validate → retry**. Treat output as untrusted.
</details>


## Retrieval-Augmented Generation

**What is RAG and what 3 problems does it solve?**
<details><summary>Answer</summary>

Retrieve relevant data at query time into the prompt. Solves **freshness, private knowledge, hallucination/attribution (citations)**.
</details>

**RAG vs fine-tuning?**
<details><summary>Answer</summary>

**RAG** = add knowledge/facts at runtime (cheap to update, citable). **Fine-tuning** = change behavior/format/style/skill. Facts→RAG; behavior→fine-tune. Often both.
</details>

**The RAG pipeline (both phases)?**
<details><summary>Answer</summary>

Indexing: **load → chunk → embed → store**. Query: **embed query → search → (rerank) → assemble → generate**. Same embedding model for both.
</details>

**Most RAG failures are failures of…?**
<details><summary>Answer</summary>

**Retrieval**, not generation. If the right chunk isn't retrieved, the model can't answer. Fix chunking, hybrid search, reranking first.
</details>

**What is hybrid search and why use it?**
<details><summary>Answer</summary>

Combine **semantic (vector)** + **keyword/BM25** search. Vectors catch meaning; keywords catch exact terms, IDs, SKUs, rare jargon vectors miss.
</details>

**What does a reranker do?**
<details><summary>Answer</summary>

Takes a broad candidate set (e.g. top-50) and **re-scores** each against the query with a **cross-encoder**, keeping the best few. Big precision win.
</details>

**How do you extract from 100k scanned/messy PDFs in 2026?**
<details><summary>Answer</summary>

Increasingly send pages straight to a **vision-capable model** (skip brittle OCR-then-text), with a strict schema at temp 0, validation, and human review of low-confidence extractions. This is **multimodal** document AI.
</details>

**Is classic RAG dead in 2026?**
<details><summary>Answer</summary>

No — it **moved up the stack**. Retrieval is now a **tool inside the agent loop** (agentic RAG); agent-driven grep/keyword search wins on navigable corpora like code (Claude Code dropped its vector index); classic pipelines still win for **large, fresh, permissioned** corpora.
</details>


## Agents & Tool Use

**How does tool use / function calling work?**
<details><summary>Answer</summary>

Model emits a **structured request** to call a described function; **your code** executes it and returns the result; model continues. Model never runs code itself.
</details>

**What is the ReAct loop?**
<details><summary>Answer</summary>

**Thought → Action (tool) → Observation (result)**, repeat until goal met. An agent = LLM in a loop using tools, with hard step/cost limits.
</details>

**Workflow vs agent — when to use which?**
<details><summary>Answer</summary>

Known, fixed steps → **deterministic workflow** (cheaper, faster, reliable). Dynamic, unpredictable path → **agent**. Prefer the simplest that works.
</details>

**Why does reliability 'compound down' in agents?**
<details><summary>Answer</summary>

Per-step reliability multiplies: 95% over 5 steps = 0.95^5 ≈ **77%**. More autonomous steps → more compounding error. Minimize steps, add checks.
</details>

**What is MCP?**
<details><summary>Answer</summary>

**Model Context Protocol** — an open standard for connecting models to tools/data via a uniform interface. 'USB-C for tools/context.'
</details>

**Name some agent frameworks (and MCP primitives).**
<details><summary>Answer</summary>

Frameworks: **LangGraph, OpenAI Agents SDK, Claude Agent SDK, CrewAI, AutoGen, LlamaIndex**. MCP exposes **tools, resources, and prompts** over stdio or streamable-HTTP transports.
</details>


## Evaluation & Quality

**Why are evals the #1 applied-AI skill?**
<details><summary>Answer</summary>

LLM output is non-deterministic & open-ended — you can't eyeball quality. Evals make it **measurable**, catch regressions, and enable fast iteration.
</details>

**For RAG, evaluate what separately?**
<details><summary>Answer</summary>

**Retrieval** (context relevance — did we fetch the right chunks?) and **generation** (faithfulness/groundedness — is the answer supported by context?).
</details>

**LLM-as-judge: what and what biases?**
<details><summary>Answer</summary>

Strong model scores outputs against a rubric. Biases: **position** (first/second), **length** (longer), **self-preference**. Randomize order; validate vs humans.
</details>

**Offline vs online evaluation?**
<details><summary>Answer</summary>

**Offline**: run eval set in CI pre-deploy (catch regressions). **Online**: prod signals — 👍/👎, A/B tests, task success, sampled human review.
</details>

**Name some LLM eval / observability tooling.**
<details><summary>Answer</summary>

Tracing/observability: **LangSmith, Langfuse, Braintrust, Arize Phoenix, Weave**, OpenTelemetry GenAI. Track token usage, latency P50/P95/P99, RAG hit rate, cost, and quality online. Naming these is cheap interview signal.
</details>

**What is error analysis (open/axial coding)?**
<details><summary>Answer</summary>

Read ~100 **real traces**, note the **first upstream failure** per trace (open coding), cluster into a **counted failure taxonomy** (axial coding), then build one **binary judge per failure mode**. Evals are discovered, not invented.
</details>

**How do you validate an LLM judge in 2026?**
<details><summary>Answer</summary>

Like a classifier: ~100+ human-labeled examples, held-out split, report **TPR and TNR** (>90% target) — not raw % agreement. Prefer **binary pass/fail per failure mode** over 1–10 scores.
</details>


## Production: Latency, Cost, Reliability & Safety

**Biggest levers to cut LLM cost/latency?**
<details><summary>Answer</summary>

**Right-size/route the model, prompt caching, shorten context, cap output, stream, batch**. Output tokens usually cost more than input.
</details>

**Why does streaming matter?**
<details><summary>Answer</summary>

It slashes **perceived** latency — users see tokens immediately even if total time is unchanged. Huge UX win for chat.
</details>

**Reliability essentials for LLM APIs?**
<details><summary>Answer</summary>

**Timeouts, retries w/ exponential backoff, fallbacks (model/provider), output validation, idempotency** for side effects, graceful degradation to human.
</details>

**What is prompt injection (incl. indirect)?**
<details><summary>Answer</summary>

Malicious instructions in **untrusted content** (page/doc/tool result) hijack the model. **Indirect** = via retrieved/tool content (dangerous for RAG/agents).
</details>

**Defenses against prompt injection?**
<details><summary>Answer</summary>

Layered: treat external content as **untrusted data**, **least-privilege tools**, **output constraints/allow-lists**, **human-in-the-loop** for high-impact actions. Can't fully prompt your way out.
</details>

**What is prefix caching and why does it matter?**
<details><summary>Answer</summary>

Cache the processed **KV state of a stable prompt prefix** (system prompt, docs) so repeat calls skip recomputing it. Big cost/latency cuts (Anthropic ~90% cost / ~85% latency on cached prefixes).
</details>

**Name three latency levers for LLM serving.**
<details><summary>Answer</summary>

**Continuous batching + PagedAttention** (vLLM), **speculative decoding** (a draft model raises **decode throughput / lowers inter-token latency** — not TTFT, which is prefill-bound), and **model routing** by complexity. Plus prefix caching + streaming.
</details>

**What is the 'lethal trifecta' in agent security?**
<details><summary>Answer</summary>

Willison's frame: exfiltration risk needs all three — **access to private data + exposure to untrusted content + ability to communicate externally**. Cut any one leg (e.g. no outbound action without human approval) and the attack can't complete.
</details>

**OWASP Agentic Top 10 & Meta's Rule of Two?**
<details><summary>Answer</summary>

OWASP's 2026 list for **agentic apps**: goal hijack, tool misuse, memory/context poisoning, rogue agents… **Rule of Two**: an unsupervised agent holds at most **two** of {private data, untrusted content, external comms}.
</details>


## Fine-Tuning & Model Adaptation

**The adaptation ladder (in order)?**
<details><summary>Answer</summary>

**Prompt → few-shot → RAG/tools → fine-tune**. Each rung costlier/less flexible. Climb in order; fine-tune last.
</details>

**What does fine-tuning NOT reliably do?**
<details><summary>Answer</summary>

**Add fresh/volatile knowledge** — that's RAG's job. Fine-tuning is for **behavior, format, tone, narrow skills**, and cheaper/faster specialized models.
</details>

**What is LoRA / PEFT?**
<details><summary>Answer</summary>

Parameter-efficient fine-tuning: train small **adapter** matrices instead of all weights. Cheaper, faster, swappable per task. Modern default.
</details>

**What dominates fine-tuning success?**
<details><summary>Answer</summary>

**Data quality** > quantity. A few hundred clean, consistent, representative examples beat thousands of noisy ones. Beware catastrophic forgetting.
</details>

**The full adaptation decision tree?**
<details><summary>Answer</summary>

**Prompt → RAG → Fine-tune → Distill.** 'Fine-tuning is for **form, not facts**.' Facts→RAG; consistent format/skill→fine-tune; cheap fast hot-path→distill.
</details>

**What is QLoRA?**
<details><summary>Answer</summary>

LoRA **adapters over a frozen 4-bit quantized base** model. Slashes memory so you can fine-tune a ~7B model on a single small GPU. PEFT's efficiency, one step further.
</details>

**Where does fine-tuning actually happen in 2026?**
<details><summary>Answer</summary>

**Open-weight models** (LoRA SFT, and RFT/agent-RL on hosted platforms). Self-serve FT of frontier proprietary models is disappearing (OpenAI sunsetting; Anthropic never offered it). **Context-engineer first**; fine-tune for form/latency/cost — never facts.
</details>


## System Design for AI Applications

**First move in any AI system design question?**
<details><summary>Answer</summary>

**Clarify requirements**: users, scale (QPS/docs), latency/accuracy/cost budgets, data sources, and how 'good' is measured. Never architect unscoped.
</details>

**Two moves that win the design round?**
<details><summary>Answer</summary>

1) **Clarify first.** 2) Volunteer **evals + the feedback loop** (and name tradeoffs) unprompted — signals applied maturity.
</details>

**The reusable FDE design template?**
<details><summary>Answer</summary>

**RAG for knowledge + tools for actions + routing for cost + guardrails/human-in-loop for safety + evals for quality**, with a feedback flywheel.
</details>


## Forward-Deployed Craft & Behavioral

**Core FDE temperament traits?**
<details><summary>Answer</summary>

**Bias to action, ambiguity tolerance, extreme ownership, customer empathy, pragmatism.** Demos over decks; simplest shipped thing wins.
</details>

**STAR + how to end a behavioral story?**
<details><summary>Answer</summary>

**Situation, Task, Action, Result.** Always end with a **quantified result** ('shipped in 9 days,' 'cut triage 40%'). Prep 5–6 flexible stories.
</details>

**Customer expects 100% AI accuracy — what do you do?**
<details><summary>Answer</summary>

Set honest expectations (LLMs are **probabilistic**), show your eval/guardrail plan, add human review for high-stakes, and agree on an acceptable bar together.
</details>

**The 5-step customer-scenario framework?**
<details><summary>Answer</summary>

**Clarify scope/success → map stakeholders → identify data/constraints → propose approach with tradeoffs → surface failure modes.** Never jump straight to a solution.
</details>


## Round-by-Round Playbook

**The decomposition-round drill?**
<details><summary>Answer</summary>

**Clarify/scope → users & stakeholders → data & constraints → decompose by risk×value → walking skeleton → failure modes + measurement.** Narrate constantly — silence reads as stuck.
</details>

**What is a 'walking skeleton'?**
<details><summary>Answer</summary>

The **thinnest end-to-end slice** that proves value in week one — ship that first, then iterate. The decomposition round's favorite proposal shape.
</details>

**What do take-home graders actually score?**
<details><summary>Answer</summary>

**It runs** · an **eval harness** (start with evals — its absence is the red flag) · production hygiene (retries, logging, no hardcoded keys) · a tradeoffs README with cost/latency notes · explicitly stated assumptions.
</details>

**Structure of a take-home video walkthrough?**
<details><summary>Answer</summary>

≤5 min: **demo the working thing first → architecture (why this shape) → one conscious tradeoff → your evals and what they showed → what you'd do next.** It's a customer-demo proxy — energy up.
</details>

**How do you behave in an AI-assisted coding round?**
<details><summary>Answer</summary>

Prompt with **intent**, **verify aloud** (run/test everything), visibly **reject bad suggestions**, own the architecture. Assume the AI transcript is reviewed — the graded skill is **judging** code, not producing it.
</details>

**Method for the debugging-an-unfamiliar-codebase round?**
<details><summary>Answer</summary>

Read the intended-behavior spec → trace **one request end-to-end** → diff intent vs behavior → instrument at the seam → fix and **run to prove it**. Beware planted red herrings — fix behavior, not style.
</details>

**The #1 rule of the discovery-call simulation?**
<details><summary>Answer</summary>

**Discover, don't pitch.** Map today's workflow, past (failed) attempts, quantified pain, immovable constraints, the people, and a success number — then summarize and propose a next step. Architecture waits.
</details>

