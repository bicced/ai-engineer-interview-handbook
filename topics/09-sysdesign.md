# AI System Design Interview Questions & Framework

> The whiteboard round: design a production AI system end to end, with tradeoffs.

[◂ Back to the handbook](../README.md)

---

## A framework for any AI system design question
_~8 min_

'Design an AI assistant for X' is a staple round. Don't jump to a diagram — run a **structured framework** that shows product + engineering + AI judgment.

1. **Clarify requirements**: users, scale (QPS, docs), latency budget, accuracy bar, budget, data sources, **data-retention / residency rules**, offline/online, what 'good' means. **Always start here.**
2. **Define success + evals**: how you'll measure quality before you build. Signals matter more than architecture.
3. **High-level architecture**: ingestion/indexing → retrieval → orchestration (prompt/agent) → model → post-processing/validation → UI. Draw the data flow.
4. **The AI core**: prompt vs RAG vs agent vs fine-tune; model choice (**including access tier and retention terms**); context strategy; structured output; **build vs rent** the agent harness.
5. **Production concerns**: latency (streaming, caching, routing), cost, reliability (retries/fallbacks), security (injection, PII), observability.
6. **Feedback loop**: capture user signals + logs → grow eval set → improve. Close the loop.
7. **Tradeoffs**: state them out loud (build vs buy, quality vs cost vs latency). Seniority = naming tradeoffs, not reciting components.

> 🎯 **Two moves that win this round** — 1) **Start by clarifying** — never architect a system you haven't scoped. 2) **Bring up evals and the feedback loop unprompted** — most candidates forget, and it instantly signals applied maturity.

**◆ Lock it in**

- Run the framework: **clarify → evals → architecture → AI core → production → feedback loop → tradeoffs**.
- **Clarify first**; quantify scale, latency, accuracy, and cost budgets.
- Volunteer **evals + feedback loop + explicit tradeoffs** to signal seniority.

> 🧠 **Feynman drill** — Recite the 7-step AI system design framework from memory.

**Retrieval check**

**Q: You're asked to 'design an AI assistant that answers questions over a company's documents.' Your first move should be:**

<details><summary>Show answer</summary>

**✓ Clarify requirements — users, scale, latency/accuracy/cost budgets, data sources, and how success is measured.**

Always scope first. Requirements (scale, latency, accuracy, cost, data, success metric) drive every downstream decision. Jumping to components signals junior instincts.
</details>

---

## Worked example: a customer-support AI assistant
_~8 min_

Let's design a support assistant that answers customer questions from a company's help docs + can take actions (check order status, issue refunds). This blends RAG + agents + production concerns.

#### Clarify (assume answers)

- 10k help articles + order DB; ~5 QPS peak; <3s first-token latency; must **cite sources**; refunds need a guardrail; multilingual later.

#### Architecture

1. **Indexing**: chunk help articles (structure-aware, ~500 tokens, metadata: product, section, permissions) → embed → vector store. Re-index on doc updates.
2. **Retrieval**: hybrid search (semantic + keyword for error codes/SKUs) → rerank top-k → assemble context.
3. **Orchestration**: router classifies intent → simple FAQ = RAG answer; action requests (order status/refund) = **tool calls** with schemas.
4. **Generation**: answer only from retrieved context, cite chunks, 'I don't know → escalate to human' fallback.
5. **Guardrails**: refunds above a threshold require **human approval** (human-in-the-loop); treat retrieved/user text as untrusted (injection defense); PII handling.
6. **Production**: stream tokens; cache stable system prompt; small model for routing, big model for hard answers; retries/fallback provider; log everything.
7. **Evals + feedback**: eval set of real tickets (retrieval relevance + answer faithfulness + action correctness); thumbs up/down and resolution rate online; failures feed back into the eval set and retrieval tuning.

> 💡 **Notice the pattern** — Almost every FDE design is some mix of: **RAG for knowledge + tools for actions + routing for cost + guardrails for safety + evals for quality**. Master this template and you can design most of these on the spot.

**◆ Lock it in**

- Support assistant = **RAG (cite) + tools (actions) + routing (cost) + human-in-the-loop (safety) + evals (quality)**.
- Gate high-impact actions (refunds) behind **human approval**; treat all external text as untrusted.
- Close with **online signals feeding the eval set** — the improvement flywheel.

> 🧠 **Feynman drill** — Design, out loud in 2 minutes, an AI assistant for triaging IT support tickets. Hit every framework step.

**Retrieval check**

**Q: In the support-assistant design, why gate refunds behind human approval?**

<details><summary>Show answer</summary>

**✓ High-impact/irreversible actions need human-in-the-loop safety, and it limits damage from errors or prompt injection.**

Refunds are high-impact and irreversible. Human-in-the-loop is the standard guardrail — it contains model mistakes and injection attacks on the action path.
</details>

---

## Worked example: an agent that triages insurance claims
_~8 min_

Agentic design now opens many screens: 'how would you build an agent that…, and what happens when it gets stuck?' Prompt: **'Design an agent that triages incoming insurance claims — reads the claim and attachments, checks the policy, requests missing information, and approves low-value claims or routes the rest to an adjuster.'**

#### 1. Clarify & assumptions

- ~5K claims/day, most under $2K; attachments are PDFs and photos; PII and regulated data; anything over $2K needs an adjuster.
- Goal: handle ~60% end to end with **zero wrongful denials** — so the agent may only **approve or escalate**, never deny.

#### 2. Success & evals

- Primary: decision/routing accuracy against adjuster labels on a held-out set, and **pass^k** on replayed claims (consistency is what the customer feels).
- Guardrail metrics: cost per claim, p95 time-to-decision, escalation rate, and a 'cited a clause that isn't in the policy' rate from a validated judge.

#### 3. Workflow where you can, agent where you must

- **Deterministic intake**: parse/OCR → extract fields with structured outputs → validate against business rules.
- **Agent only for the judgment step**: read the policy, decide what's missing, request it, draft the decision with cited clauses.
- Tools as **MCP servers**: get_policy, get_claim_history, request_documents (templated message only), route_to_adjuster, approve_claim (amount-capped, idempotency key).

#### 4. Harness, safety & stuck agents

- **Harness**: regulated data and retention rules → check ZDR/compliance eligibility; likely an open SDK in the insurer's VPC.
- **Rule of Two**: the agent reads untrusted attachments and private policy data, so it gets **no free-form outbound channel** — customer contact goes through a templated tool.
- **The cap lives in the tool**: approve_claim rejects anything over $2K server-side; prompts are not controls.
- **Stuck-agent handling**: step and spend budgets, loop detection (same tool + same args), and a timeout that escalates to a human with a summary of what was tried.

#### 5. Production

- Low reasoning effort for extraction, higher for the decision step; prompt-cache the policy-rules prefix.
- Trace every step and tool call (the OpenTelemetry GenAI conventions are still in 'development' status — pick a backend you can export from).
- Pin model versions; re-run the eval suite before any migration; online, sample decisions for adjuster review and feed disagreements back into the dataset.

> 🎯 **The close** — "I kept the agent's autonomy to the one step that needs judgment, enforced every limit inside the tools, and I'd call it production-ready once pass^k on replayed claims clears the bar and the wrongful-denial test stays at zero."

**◆ Lock it in**

- **Workflow for the predictable parts, agent for the judgment step** — and expose tools as capped, idempotent MCP servers.
- Enforce limits **in the tools**, apply the **Rule of Two**, and design stuck-agent handling: budgets, loop detection, escalation.
- Evaluate with **pass^k on replayed cases** plus validated judges; trace everything; eval-gate model migrations.

> 🧠 **Feynman drill** — Design, out loud in three minutes, an agent that handles employee IT access requests. Cover autonomy, tool limits and what happens when it gets stuck.

**Retrieval check**

**Q: Where should the claims agent's $2,000 auto-approval limit be enforced?**

<details><summary>Show answer</summary>

**✓ Inside the approve_claim tool on the server, with a human approval path above the limit.**

Limits on high-impact actions belong in code the model can't talk its way past — the tool itself — with a human path above the cap. Prompts and training data are guidance, not controls; evals detect problems but don't prevent them.
</details>

---

[◂ Back to the handbook](../README.md) · [▸ Practice this interactively](https://deployprep.vercel.app/track/fde)
