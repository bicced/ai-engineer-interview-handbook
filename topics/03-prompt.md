# Prompt Engineering Interview Questions & Answers

> Turn vague asks into reliable model behavior: structure, examples, chain-of-thought, structured output.

[◂ Back to the handbook](../README.md)

---

## Anatomy of a production prompt
_~7 min_

Amateurs write a sentence. Professionals write a **structured prompt** with clear roles and sections. A strong production prompt usually has:

- **Role / system prompt**: who the model is and its high-level goal + constraints.
- **Task instructions**: precise, unambiguous steps. Tell it what to do **and what not to do**.
- **Context**: retrieved documents, user data, examples — clearly delimited.
- **Output format**: exact shape (JSON schema, sections). Show, don't just tell.
- **Guardrails**: how to handle unknowns ("If the answer isn't in the context, say 'I don't know'").

```
SYSTEM: You are a support-ticket classifier for an e-commerce company.
Classify each ticket into exactly one category and rate urgency.

Rules:
- Use ONLY these categories: [billing, shipping, returns, technical, other]
- If unsure, use "other". Never invent a category.
- Output valid JSON only, no prose.

Output schema:
{ "category": string, "urgency": "low"|"medium"|"high", "reason": string }

Ticket: """{ticket_text}"""
```

> 💡 **Delimiters matter** — Wrap user/injected content in clear delimiters (triple quotes, XML tags like <document>). It reduces confusion and is your first line of defense against **prompt injection**.

**◆ Lock it in**

- Structure a prompt into **role, instructions, context, output format, guardrails**.
- State the **negative space**: what NOT to do, and how to handle unknowns.
- Delimit injected content clearly — clarity + injection defense.

> 🧠 **Feynman drill** — List the five sections of a robust production prompt from memory.

**Retrieval check**

**Q: Which addition most improves reliability of a classification prompt?**

<details><summary>Show answer</summary>

**✓ Constraining to a fixed category list, specifying an exact JSON schema, and defining a fallback for uncertainty.**

Reliability comes from constraints: fixed label set, explicit output schema, and an uncertainty fallback. Politeness and brevity don't drive correctness; high temperature hurts consistency.
</details>

---

## Zero-shot, few-shot & in-context learning
_~6 min_

**In-context learning** is the model's ability to learn a task from examples in the prompt — no training required.

- **Zero-shot**: just instructions, no examples. Fine for simple, well-known tasks.
- **Few-shot**: include 2–8 input→output **examples**. Dramatically improves format adherence and edge-case handling. The single highest-leverage technique after clear instructions.
- Pick examples that are **diverse** and cover the tricky cases (including a 'none/other' example). Keep formatting identical to what you want back.

> 💡 **Few-shot = teaching by demonstration** — If you can't precisely describe the rule, **show** it. Two good examples of the exact output you want often beat three paragraphs of instructions.

> ⚠️ **Watch the cost** — Every few-shot example is tokens on **every** call — cost and latency. If examples get large or numerous, that's a signal to consider fine-tuning or retrieval of examples (dynamic few-shot).

**◆ Lock it in**

- **Few-shot examples** are the highest-leverage lever after clear instructions.
- Choose **diverse** examples covering edge cases and the 'none' case; match the exact output format.
- Examples cost tokens on every call — a lot of them hints at fine-tuning/dynamic selection.

> 🧠 **Feynman drill** — When would you switch from few-shot prompting to fine-tuning? Give the tradeoff.

**Retrieval check**

**Q: Your extraction prompt gets the format wrong ~20% of the time with zero-shot. Cheapest first fix?**

<details><summary>Show answer</summary>

**✓ Add a few diverse worked examples (few-shot) showing the exact desired output, including a tricky case.**

Few-shot examples are the fast, cheap fix for format adherence. Reach for fine-tuning only when prompting plateaus or example tokens get expensive.
</details>

---

## Chain-of-thought & reasoning control
_~6 min_

**Chain-of-thought (CoT)** prompting asks the model to reason step by step before answering. For multi-step problems (math, logic, complex extraction), it substantially improves accuracy because the model uses generated tokens as a scratchpad.

- Simple trigger: "Think step by step, then give the final answer."
- For clean output, have it reason in a hidden/section then emit a final structured answer — or use a model with built-in reasoning.
- **Reasoning models** (e.g. o-series, extended-thinking modes) do this internally — you often **don't** need manual CoT and shouldn't force verbosity.

> ⚠️ **Don't parse the reasoning** — If you show your reasoning and your final answer together, **extract the final answer explicitly** (e.g. after a marker or in a JSON field). Don't regex the whole blob. And more reasoning = more tokens = more cost/latency — use it where it pays off.

> 🎯 **Nuance that impresses** — "For arithmetic-heavy or multi-hop tasks I'd enable step-by-step reasoning, but for a reasoning model I'd rely on its internal thinking and just constrain the final output — no need to pay for verbose CoT I then have to parse."

**◆ Lock it in**

- CoT ("think step by step") boosts multi-step accuracy by giving the model a scratchpad.
- Separate reasoning from the **final structured answer**; extract the answer explicitly.
- Reasoning models think internally — don't force manual CoT; mind the token cost.

> 🧠 **Feynman drill** — Why does asking a model to 'think step by step' improve accuracy on a math word problem?

**Retrieval check**

**Q: For a multi-step logic task, chain-of-thought helps mainly because:**

<details><summary>Show answer</summary>

**✓ The generated reasoning tokens act as a scratchpad, letting the model work through intermediate steps before committing to an answer.**

CoT gives the model intermediate working space (tokens) to decompose the problem, improving multi-step accuracy. It doesn't change model size, temperature, or add retrieval.
</details>

---

## Structured output & making prompts robust
_~6 min_

Production systems need **machine-parseable** output. Free-form text is a liability. Techniques, best to okay:

- **Structured output / JSON mode / tool schemas**: constrain the model to a schema at the API level so it **can't** emit invalid JSON. Prefer this when available.
- **Ask for JSON + validate + retry**: instruct the schema, then parse; on failure, send the error back and retry once.
- **Prefill / response prefix**: start the assistant's turn with { to force JSON, or with a known prefix to steer format.

> 💡 **Always validate** — Treat model output like untrusted user input: parse it, validate against a schema (e.g. Pydantic/zod), and have a fallback path. A pipeline that assumes the model 'just returns JSON' will break in production.

#### Iterating on prompts like an engineer

- Keep prompts in **version control**, not hardcoded strings scattered in code.
- Change **one thing at a time** and measure against an eval set — never 'it feels better'.
- Build a small set of **failure cases** and re-run them on every prompt change (regression testing).

**◆ Lock it in**

- Prefer **structured output / schema-constrained** generation; else JSON + validate + retry.
- Treat model output as untrusted: **parse, validate, fallback**.
- Engineer prompts: version control, change one variable, test against an eval + regression set.

> 🧠 **Feynman drill** — Describe how you'd guarantee a downstream service always receives valid JSON from an LLM.

**Retrieval check**

**Q: The most robust way to ensure valid structured output is to:**

<details><summary>Show answer</summary>

**✓ Use schema-constrained/structured output where available, and otherwise parse-validate-retry against a schema.**

Schema-constrained generation is strongest; when unavailable, validate against a schema and retry. Regex/hoping/temperature don't guarantee well-formed, valid output.
</details>

---

[◂ Back to the handbook](../README.md) · [▸ Practice this interactively](https://deployprep.com/track/fde)
