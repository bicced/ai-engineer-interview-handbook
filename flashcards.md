# Rapid-Fire Flashcards

> 91 spaced-repetition cards. Cover the answer, recall, then check. [◂ Back](./README.md)


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

**What did the FDE market look like in late 2026?**
<details><summary>Answer</summary>

**7,300+ US postings Jan–Sep 2026** vs ~1,200 in all of 2025 (Lightcast); top posters are **consultancies and clouds**; OpenAI's title is **AI Deployment Engineer**; median advertised salary ~$195K, lab bases far higher.
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

The max tokens the model can attend to at once — ~1M on 2026 frontier models. Shared by **system prompt + history + retrieved docs + user message + response (+ thinking tokens)**. The model is stateless; server-side conversation state is storage, not memory.
</details>

**What is 'lost in the middle'?**
<details><summary>Answer</summary>

Models attend best to the **start and end** of a long context; info in the middle is often missed. Put critical instructions/data at the edges.
</details>

**Temperature: low vs high?**
<details><summary>Answer</summary>

**Low (0–0.3)** = focused → extraction, classification, code. **High (0.8+)** = varied → brainstorming. Temp 0 ≠ deterministic. **2026:** many frontier reasoning models reject or ignore temperature — control variance with structured outputs, prompts, effort and validation.
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

**What happened to sampling parameters on 2026 frontier models?**
<details><summary>Answer</summary>

Many reasoning models **reject or ignore** temperature/top-p (Claude Haiku 5.5 errors, GPT-6 Astra dropped them, Gemini 4 ignores them). Variance control moved to **structured outputs, prompts, reasoning effort, validation**; the knobs still matter on open-weight models.
</details>

**What is test-time compute / reasoning effort?**
<details><summary>Answer</summary>

Reasoning models spend **thinking tokens** before answering — more buys accuracy on hard tasks, at the cost of latency and output-token spend. You set an **effort level** (low → max) per request; o-series models and fixed thinking budgets are legacy.
</details>

**What is RLVR?**
<details><summary>Answer</summary>

**Reinforcement learning with verifiable rewards**: RL on tasks whose outcome can be checked (math answers, passing tests, completed tool tasks). It produced today's reasoning models and is how agentic skills get trained — including RL fine-tuning against graders.
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

Takes a broad candidate set (e.g. top-50) and **re-scores** each against the query (a cross-encoder reads query + chunk together), keeping the best few. 2026 rerankers also follow **instructions** and return **calibrated scores** you can threshold.
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

Open SDKs: **OpenAI Agents SDK, Claude Agent SDK, LangGraph, Microsoft Agent Framework** (absorbed AutoGen/Semantic Kernel), **Google ADK**, Pydantic AI. Managed harnesses: OpenAI Agents API, Anthropic Managed Agents, AWS AgentCore. MCP exposes **tools, resources, prompts** over stdio or Streamable HTTP — stateless since the 2026-07-28 spec.
</details>

**What changed in MCP's 2026-07-28 spec?**
<details><summary>Answer</summary>

**Stateless**: no initialize handshake or session id; state lives in explicit handles passed as tool args, so plain load balancing works. **input_required** round-trips for mid-call input; **Tasks** as an extension; roots/sampling/logging and HTTP+SSE deprecated; client registration via Client ID Metadata Documents.
</details>

**What is an Agent Skill?**
<details><summary>Answer</summary>

A folder with a `SKILL.md` (name, description, instructions) plus optional scripts/resources. The agent sees only the description until relevant, then loads it — **progressive disclosure**. Now an open standard under the Linux Foundation's Agentic AI Foundation.
</details>

**MCP vs A2A?**
<details><summary>Answer</summary>

**MCP** = agent ↔ tools and data. **A2A** = agent ↔ other agents across vendors/orgs (agent cards, tasks). Both Agentic AI Foundation projects. Deterministic API → MCP tool; someone else's autonomous agent → A2A.
</details>

**What is an agent harness — and the build-vs-rent question?**
<details><summary>Answer</summary>

Everything around the loop: **sandbox, context management (compaction, memory), permissions/approvals, durable sessions, tracing**. Rent (OpenAI Agents API, Anthropic Managed Agents, AgentCore) to ship fast — but check **ZDR/HIPAA eligibility, residency and lock-in** first.
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

Tracing/eval: **LangSmith, Braintrust, Langfuse (now ClickHouse), Arize Phoenix (now Dynatrace), Promptfoo (now OpenAI), W&B Weave**; OpenTelemetry GenAI conventions (still 'development' status). OpenAI's hosted Evals shut down Nov 2026. Track tokens, p50/p95/p99 latency, cost, retrieval hit rate and quality online.
</details>

**What is error analysis (open/axial coding)?**
<details><summary>Answer</summary>

Read ~100 **real traces**, note the **first upstream failure** per trace (open coding), cluster into a **counted failure taxonomy** (axial coding), then build one **binary judge per failure mode**. Evals are discovered, not invented.
</details>

**How do you validate an LLM judge in 2026?**
<details><summary>Answer</summary>

Like a classifier: ~100+ human-labeled examples, held-out split, report **TPR and TNR** (>90% target) — not raw % agreement. Prefer **binary pass/fail per failure mode** over 1–10 scores.
</details>

**How many traces for error analysis?**
<details><summary>Answer</summary>

~100 is a **floor**: keep reading until new traces stop adding failure categories (**saturation**). Hand-code the first ~30 yourself before LLM-assisted clustering; label 100–200 examples per judge (train/dev/test) to validate it.
</details>

**Why don't leaderboard scores transfer to your use case?**
<details><summary>Answer</summary>

Benchmarks saturate and churn (SWE-bench Verified was retired as a headline metric in 2026) and scores swing with the **harness**. Use them to shortlist; **your eval set on your data** picks the winner.
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

**Prompt-caching fine print in 2026?**
<details><summary>Answer</summary>

Cached reads are **~90–97% cheaper**, but some providers **bill cache writes** (OpenAI 1.25×–2.5× by TTL), caches expire, and any prefix change (reordered tools, a timestamp) busts them. Put stable content first; check cache diagnostics.
</details>

**How do you protect production from model churn?**
<details><summary>Answer</summary>

Pin model versions, track deprecation dates (2026 retired the o-series, Gemini 2.5 and older Claude models), and keep an **eval suite that makes migration a measured change**. Watch for automatic rerouting to a new model.
</details>

**Prefill vs decode?**
<details><summary>Answer</summary>

**Prefill** processes the prompt in parallel (compute-bound) → sets **TTFT**. **Decode** generates token by token reading the KV cache (memory-bound) → sets **tokens/sec**, and is why output costs ~5× input.
</details>

**2026 agent attack surfaces beyond prompt injection?**
<details><summary>Answer</summary>

**Repo/config files** (AGENTS.md, hooks, .env, MCP configs) as untrusted input; **supply chain** (a compromised LLM-gateway package stole keys); **allow-listed egress** used for exfiltration; **memory/compaction** poisoning; provider **tenant isolation** bugs.
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

**Hosted fine-tuning landscape, late 2026?**
<details><summary>Answer</summary>

**OpenAI is closing self-serve fine-tuning** (no new jobs after Jan 6, 2027); Anthropic doesn't offer it; **Google expanded** (Gemini SFT GA, RL tuning preview). Most applied FT = **open weights**: LoRA SFT + RL against graders (Tinker, Fireworks, Bedrock).
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

**What does the converged AI-assisted round look like?**
<details><summary>Answer</summary>

**An AI assistant + an unfamiliar multi-file repo**: fix a bug → build a feature → optimize (Meta), or comprehend and improve the repo (Google), or narrate the complexity of AI-written code (OpenAI's Codex screen). Graded on **verification**; 'crutch' use is marked down.
</details>

**Coding-agent fluency — what do interviewers look for?**
<details><summary>Answer</summary>

**Spec/tests first**, repo instructions (AGENTS.md), plan → small diffs → run → review every diff **including the tests**; parallel agents on isolated branches; and for customer rollouts: bounded workflows, a measured baseline, sandbox + review guardrails, enablement.
</details>


## Company Loop Guides

**Anthropic loop — official vs reported?**
<details><summary>Answer</summary>

**Official**: no AI unless explicitly permitted; postings ask for MCP servers, sub-agents, agent skills, eval frameworks. **Reported**: escalating-level coding, fictional-customer take-home, discovery-call role-play, eval-harness design round.
</details>

**OpenAI FDE / AI Deployment Engineer loop?**
<details><summary>Answer</summary>

**Varies by team**: take-home + recorded walkthrough, an **AI-enabled Codex screen** (narrate the AI's code complexity), or coding + system-design screens; later customer solution design. Know Responses/Agents APIs — Assistants and hosted Evals are gone.
</details>

**Sierra's loop (from its own post)?**
<details><summary>Answer</summary>

**System-design** phone screen (replaced no-AI coding) → onsite **Plan → 2-hr Build with any AI tools → Review** (demo, code review, path to production, how AI was used); a piloted round improving a draft PR with coding agents.
</details>

**Palantir's reported rounds?**
<details><summary>Answer</summary>

**Decomposition** (vague problem; process + user empathy), **Re-engineering** (bugs + red herrings in a big codebase), **Learning** (unfamiliar library on the spot), behavioral/mission fit. AI reportedly banned. Roles: FDSE vs Deployment Strategist.
</details>

