# AI System Design Interview Questions & Framework

> The whiteboard round: design a production AI system end to end, with tradeoffs.

[◂ Back to the handbook](../README.md)

---

## A framework for any AI system design question
_~8 min_

'Design an AI assistant for X' is a staple round. Don't jump to a diagram — run a **structured framework** that shows product + engineering + AI judgment.

1. **Clarify requirements**: users, scale (QPS, docs), latency budget, accuracy bar, budget, data sources, offline/online, what 'good' means. **Always start here.**
2. **Define success + evals**: how you'll measure quality before you build. Signals matter more than architecture.
3. **High-level architecture**: ingestion/indexing → retrieval → orchestration (prompt/agent) → model → post-processing/validation → UI. Draw the data flow.
4. **The AI core**: prompt vs RAG vs agent vs fine-tune; model choice; context strategy; structured output.
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

[◂ Back to the handbook](../README.md) · [▸ Practice this interactively](https://deployprep.com/track/fde)
