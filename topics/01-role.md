# Applied AI Engineer & Forward Deployed Engineer — The Role & Interview Loop

> What the job actually is, how it differs from ML/research, and how the interview loop works.

[◂ Back to the handbook](../README.md)

---

## What an Applied AI / Forward Deployed Engineer is
_~7 min_

An **Applied AI Engineer (AAE)** builds products and features **on top of** foundation models. A **Forward Deployed Engineer (FDE)** — increasingly searched as **forward deployed AI engineer** — does the same thing but **embedded with a customer**: flying to their office (or Zooming into their mess), understanding their workflow, and shipping a working AI solution against real, dirty data on a deadline. The term was coined at **Palantir**; OpenAI (which titles it **AI Deployment Engineer**), Anthropic, the cloud providers, the big consultancies and a wave of AI startups now hire it heavily.

The unifying idea: you are the person who turns a general-purpose model into **something that solves a specific person's problem in production**. You are not training frontier models. You are the bridge between the model's raw capability and a customer's outcome.

#### The three things every version of this role blends

- **Real software engineering** — you ship code: APIs, data pipelines, evals, glue. Python is the lingua franca; TypeScript/React for anything customer-facing.
- **Applied LLM craft** — prompting, RAG, agents, evaluation, cost/latency tuning. You know what models can and can't do and design around the limits.
- **Customer / product instinct** — you scope ambiguous problems, talk to non-engineers, demo, and iterate. FDE weights this heaviest; pure AAE weights engineering heaviest.

> 💡 **The one-liner** — Research engineers make the model smarter. **You make the model useful.** Interviewers are testing whether you can take a fuzzy business problem and a fallible model and reliably close the gap between them.

#### Where the role came from — and why it exploded

- **Palantir** invented it: **Deltas** (embedded engineers who ship against one customer's mess) paired with **Echoes** (domain strategists who find the problem worth solving). Palantir's own framing: product engineering = 'one capability, many customers'; FDE = **'one customer, many capabilities.'**
- The best one-liner in circulation (Bob McGrew, ex-Palantir/OpenAI): FDEs **'build the rough gravel road'** where the product needs to go; product engineers pave it into a highway for the next ten customers. FDE is **product discovery**, not consulting.
- Why every AI company now hires it: the great majority of enterprise AI pilots fail on **integration, not model quality** — someone has to close the last mile inside the customer's messy reality. That's you.
- **2026: it went mainstream.** US FDE postings hit **7,300+ in Jan–Sep 2026** vs ~1,200 in all of 2025 (Lightcast). AWS committed $1B to an FDE org, OpenAI launched a deployment company, Microsoft runs FDE as multidisciplinary pods — and the biggest posters are now **consultancies and clouds** (Deloitte, Accenture, Google, KPMG), often for one vendor's stack.

> 🎯 **Great 'Why FDE?' material** — Recruiter screens filter hard on motivation, and generic answers get downgraded. Use the history: "I want the **product-discovery** version of engineering — embed with one customer, ship the gravel road fast, and feed what I learn back into the product." That's the role described in the industry's own language.

**◆ Lock it in**

- AAE/FDE = ship products **on top of** foundation models, not train them.
- The role blends **SWE + applied LLM craft + customer/product sense**.
- FDE = **product discovery**: ship the 'gravel road' with one customer; product paves it for the rest.
- FDE leans customer-facing and on-site; AAE leans product engineering — same core skills.
- 2026: FDE went mainstream — **clouds and consultancies** now post most roles; OpenAI calls it **AI Deployment Engineer**.

> 🧠 **Feynman drill** — In 20 seconds, explain to a non-technical friend how a Forward Deployed Engineer is different from a data scientist or an ML researcher.

**Retrieval check**

**Q: Which best captures the core of the Applied AI / FDE role?**

<details><summary>Show answer</summary>

**✓ Building useful products on top of foundation models and closing the gap to a real user's problem.**

You build **on top of** models. Training frontier models is the research/pretraining org; infra is the systems org. Your job is applied: model capability → shipped user value.
</details>

---

## How it differs from ML Engineer, Research & Solutions roles
_~6 min_

Interviewers often probe whether you understand the boundaries of the role. Know the neighbors:

- **Research Engineer / Research Scientist** — improves the model itself (architecture, training, RLHF). Deep ML theory, PyTorch at scale. You are **not** this.
- **ML Engineer (traditional)** — trains and serves bespoke models (ranking, fraud, recsys). Feature pipelines, model training, MLOps. Overlaps on eval/serving, but you mostly **call** a pretrained LLM rather than train one.
- **Solutions Architect / Sales Engineer** — pre-sales, reference architectures, does less hands-on shipping. FDE is more hands-on and **owns delivery** of working software.
- **Software Engineer (product)** — closest cousin. The AAE/FDE is a product SWE who is **fluent in LLM behavior, evals, and non-determinism**.

> 🎯 **Say this in the interview** — "I sit between the model and the customer. I don't train frontier models — I take one, wrap it in retrieval, tools, evals and guardrails, and ship something reliable against the customer's actual data." Clean role clarity signals seniority.

**◆ Lock it in**

- Research = improve the model. ML Eng = train bespoke models. **You = apply pretrained models to ship product.**
- FDE **owns delivery** — more hands-on than a solutions architect or sales engineer.
- Your differentiator vs a normal SWE: fluency in **non-determinism, evals, and LLM failure modes**.

> 🧠 **Feynman drill** — In four sentences, place the AAE/FDE role against its neighbors: research engineer, ML engineer, solutions architect, product SWE.

**Retrieval check**

**Q: A hiring manager asks how your role differs from a Research Engineer. Best answer?**

<details><summary>Show answer</summary>

**✓ "They improve the model itself; I apply an existing model — retrieval, tools, evals, guardrails — to ship a reliable product."**

Crisp boundary: research improves the model; you apply it and own the surrounding system. Claiming you train frontier models (A/C) is wrong for the role; (D) undersells the SWE depth expected.
</details>

---

## A day in the life & what companies actually want
_~6 min_

Across OpenAI, Anthropic, Palantir, Sierra, the clouds and countless startups, the day-to-day rhymes:

- Scope a fuzzy problem with a customer or PM ("can AI triage our support tickets?").
- Prototype fast — a prompt + a bit of retrieval + a script — and demo within days.
- Build an **eval set** from real examples so you can tell if changes help.
- Harden it: handle failure cases, add guardrails, cut latency and cost, add observability.
- Iterate with the customer, then hand off or scale it.

#### What every job posting is really screening for

- **Speed & pragmatism** — can you get to a working demo fast, not a perfect design doc.
- **Ambiguity tolerance** — the spec is vague and the data is dirty. You thrive anyway.
- **LLM system fluency** — RAG, agents, evals, cost/latency are second nature.
- **Communication** — you can explain tradeoffs to a CTO and a non-technical ops lead in the same meeting.
- **Ownership** — you carry a problem from "vague ask" to "in production" without hand-holding.
- **Agent-era stack** — late-2026 postings name **agent harnesses, MCP servers, sub-agents and skills**, evals, observability, identity/governance — and rolling out coding agents (Codex, Claude Code) across a customer's engineering org. Some list fluency with coding agents as a **minimum** qualification.
- **Capability transfer** — you leave the customer able to run, evaluate and extend what you built. Microsoft's FDE leadership names this explicitly.

> ⚠️ **Common rejection reason** — Candidates who can discuss transformers in the abstract but freeze on "the model keeps hallucinating account numbers in production — what do you actually do?" The role rewards **applied judgment** over theory.

**◆ Lock it in**

- Loop: **scope → prototype → eval → harden → iterate**, on customer-real data.
- Screened for: speed, ambiguity tolerance, LLM system fluency, communication, ownership — plus the **agent-era stack** (harnesses, MCP, skills, evals) and **capability transfer**.
- Applied judgment beats textbook theory in this role.

> 🧠 **Feynman drill** — Describe the five stages of an FDE's typical project loop from memory, in order.

**Retrieval check**

**Q: Which trait is MOST distinctive of a strong FDE (vs a generic backend engineer)?**

<details><summary>Show answer</summary>

**✓ Comfort taking a vague ask and dirty data to a working, evaluated demo quickly.**

The defining trait is turning ambiguity + messy real data into a fast, **evaluated** working solution. The others are useful but not the differentiator.
</details>

---

## The interview loop — what to expect
_~7 min_

The loop varies by company but almost always has these stages. Prepare for each:

1. **Recruiter / hiring-manager screen** — your story, why AI, why this company. Have a crisp "here's an AI thing I shipped" narrative.
2. **Practical coding** — usually **not** abstract leetcode. Expect: call an LLM API, parse/transform data, build a small RAG or tool-calling loop, or debug a broken prompt/pipeline. Python, often in a shared editor. Increasingly the format is **an AI assistant + an unfamiliar multi-file repo + bugs to fix** (Meta, Google, OpenAI's Codex screen).
3. **LLM / applied AI deep-dive** — RAG vs fine-tuning, how to evaluate, how to stop hallucinations, prompt design, agents. Concept + judgment.
4. **AI system design** — "design an AI assistant for X." Ingestion, retrieval, orchestration, evals, cost/latency, guardrails, feedback loops — and, increasingly, **agentic** design: stuck agents, human-in-the-loop, which harness to run on.
5. **Take-home or live build** (common at startups) — build a small working AI app; judged on it working, code quality, and evals.
6. **Behavioral / customer-facing** — ambiguity, dealing with an unhappy stakeholder, prioritization, communicating to non-engineers. FDE roles weight this heavily.

> 💡 **Company flavors (late 2026)** — **Palantir** (invented the role): a **Decomposition** round, a **Re-engineering** round (real bugs plus red herrings in a large codebase) and a **Learning** round; candidates consistently report AI tools are banned. **OpenAI**: **varies by team** — a take-home with a recorded walkthrough, an **AI-enabled coding screen in a Codex environment**, or coding + system-design screens, then customer solution design. **Anthropic**: official policy is **no AI unless they say so**; postings name **MCP servers, sub-agents and agent skills**; candidates report a fictional-customer take-home, a discovery-call role-play and an eval-design round. **Sierra** (its own Apr-2026 post): a **system-design phone screen**, then **Plan → 2-hr Build with any AI tools → Review**. **Google**: a Gemini-assisted **code-comprehension** round. The [Company Loop Guides](./12-companies.md) has the full, sourced breakdown — and the [Round-by-Round Playbook](./11-rounds.md) drills each round.

> ⚠️ **Late-2026 reality — AI in the room** — AI-assisted rounds are now mainstream — Meta's AI-enabled onsite, Google's Gemini round, OpenAI's Codex screen, Sierra, LinkedIn, Shopify — graded on how well you **drive and verify** AI output (Meta: candidates get 'marked down if you use it as a crutch'). Roughly **half of companies allow AI in interviews** in some form and about a third ban it (CoderPad 2026); **AI interviewers** now run real technical screens (HackerRank's Chakra went GA in Oct 2026); and many employers moved skills tests to **proctored or in-person** formats. Never assume: **ask what's allowed before every round**, and prep both modes.

**◆ Lock it in**

- Coding rounds are **practical** (API calls, RAG, data wrangling) — and increasingly **AI-assisted in an unfamiliar repo**.
- Expect an **applied-LLM deep dive** and an **AI system design** round.
- FDE loops weight **behavioral / customer-facing** heavily — prepare STAR stories.

> 🧠 **Feynman drill** — Recite the six standard stages of an Applied AI / FDE loop from memory, with one sentence on how you'd prep each.

**Retrieval check**

**Q: You're prepping for the coding round of an Applied AI Engineer loop. Best use of time?**

<details><summary>Show answer</summary>

**✓ Practicing calling an LLM API, building a small RAG loop, and parsing/validating model output in Python.**

These rounds are practical and LLM-flavored: API calls, retrieval, tool loops, and robust output handling. Some fundamentals help, but hard DP/CUDA is rarely the bar for this role.
</details>

---

## The 2026 FDE market: titles, employers, salary & offers
_~6 min_

The forward-deployed market changed shape in 2026. Knowing it lets you target the right employers, decode titles, and negotiate from facts instead of folklore.

#### Who's hiring — and what they call it

- **Volume**: 7,300+ US FDE postings in Jan–Sep 2026, vs ~1,200 in all of 2025 and under 200 in 2023 (Lightcast, via CNBC). Roles **open at one time** are far fewer — ~1,300 across ~550 companies in Oct 2026. Don't confuse cumulative postings with live roles.
- **Employers**: the top posters are **consultancies and clouds** — Deloitte, Accenture, Google, KPMG — alongside the labs (OpenAI, Anthropic), AWS's new $1B FDE org, Microsoft, Databricks, and agent startups (Sierra, Decagon, Harvey).
- **Titles**: Forward Deployed Engineer · **AI Deployment Engineer** (OpenAI — with Codex, ChatGPT-plugins, startup and digital-native tracks) · Frontier Agents Engineer (Scale) · Applied AI Engineer · Solutions / Deployed Engineer — plus vendor-specific roles ('Anthropic FDE', 'Google Cloud FDE') at consultancies, some requiring security clearance.
- **What postings now list**: agent harnesses, MCP servers, sub-agents and skills, evals, observability, identity/governance, cost/latency — and rolling coding agents out across a customer's engineering org. Expect 25–50% travel at many labs.

#### Salary & offers — facts vs folklore

- **Market median** advertised FDE salary: ~**$195K** (Lightcast, up ~40% vs 2023). Lab bases run far higher — Anthropic posts US FDE base around **$280–320K** (London £225–255K). Treat $500K+ total-comp figures as self-reported unless you see the offer letter.
- **Equity is getting more liquid**: Anthropic filed to go public in Oct 2026, and OpenAI reportedly moved new grants to RSUs. Ask about vesting, lockups, refresh policy and tax treatment — not just the headline number.
- **Negotiation**: 'lab X never negotiates' is secondhand folklore — ask your recruiter directly. Level, competing offers and sign-on are the usual levers; negotiate total comp on the written offer.
- **Credentials**: vendor programs are appearing — e.g. Anthropic's Claude Frontier Academy (nomination-based, graded deployment practical, 'Claude Frontier Deployed Engineer' badge). Consultancy postings may start asking for them; they don't replace shipped work.

> 💡 **Target the employer type, not just the logo** — A **lab** FDE does product discovery for one platform; a **consultancy** FDE ships one vendor's stack across many clients; a **cloud** FDE pulls workloads onto that cloud. Same core skills — different 'why us' story, travel load and comp shape. Say which one you want and why.

> ⚠️ **Delivery role or discovery role?** — Many 2026 'FDE' postings at consultancies are implementation roles on a single vendor's stack. Read the posting — and ask in the screen — whether you'll be feeding a product roadmap or billing delivery hours. Both are fine; mismatched expectations aren't.

**◆ Lock it in**

- FDE went mainstream in 2026: **7,300+ US postings Jan–Sep** — most from consultancies and clouds, not only labs.
- Decode titles: **AI Deployment Engineer** (OpenAI), Frontier Agents Engineer (Scale), vendor-specific FDE roles at consultancies.
- Salary: ~**$195K** market median advertised; lab bases far higher; verify equity liquidity, vesting and negotiation norms yourself.

> 🧠 **Feynman drill** — In 60 seconds, explain how an FDE role differs at a lab, a cloud provider and a consultancy — and which you'd pick and why.

**Retrieval check**

**Q: A friend says 'there are 7,300 open FDE jobs right now.' What's the precise correction?**

<details><summary>Show answer</summary>

**✓ 7,300+ is cumulative US postings for Jan–Sep 2026 (Lightcast); roles open at any one time were far fewer (~1,300 in Oct 2026).**

Cumulative postings over a period ≠ open roles at a moment. Precision with numbers — and citing source and date — is the same habit interviewers probe in eval discussions.
</details>

---

## How to actually win the loop (+ the capstone)
_~6 min_

Across every company, the same handful of behaviors separate offers from rejections. Internalize these — they're the meta-strategy this whole course is built to deliver.

1. **Lead with evals.** The single most-tested signal is: "**How do you know your AI system is actually working?**" Never answer "I tested it manually." Talk golden sets, LLM-as-judge (validated against humans), and regression gates. Module 6 makes this automatic.
2. **Start with the customer, not the tech.** In any open-ended scenario, **clarify before you architect**. The #1 failure mode is jumping straight to a vector DB. Ask about users, data, constraints, and success first.
3. **Know the decision tree cold:** **Prompt → RAG → Fine-tune → Distill**. "Fine-tuning is for form, not facts." This exact question appears at OpenAI, Anthropic, and Mistral.
4. **Speak in production primitives:** token cost, latency budgets, prompt-cache economics, reasoning effort, model routing, guardrails, observability — not just "call the model."
5. **Own it.** Say "I will," bring a 30/60/90-day mindset, and have a "deployment that failed" story ready.

> 🎯 **The capstone that proves everything** — The best single prep artifact (recommended across every source): build **one end-to-end project** — a RAG or agent app on **real data**, with chunking, embeddings, **hybrid search + rerank**, an **eval harness (golden set + LLM-as-judge)**, guardrails, tracing, and a short **video walkthrough**. It's evidence for nearly every interview module at once. Plus prepare **4–6 STAR stories**. Bonus: some hiring managers now skip puzzles and review a **prototype you already built** — the capstone doubles as that portfolio.

> 💡 **How to use this trainer** — Work the modules in order, answer every retrieval check, and let the **flashcards** resurface concepts on schedule. When your **readiness meter hits ~100%** and you can pass the **mock interview** cold, you're loop-ready. Then go build the capstone.

**◆ Lock it in**

- The two universal winners: **lead with evals** and **start with the customer, not the tech**.
- Memorize the **Prompt → RAG → Fine-tune → Distill** tree and speak in **production primitives**.
- Build **one end-to-end evaluated capstone** + **4–6 STAR stories** — it covers almost every round.

> 🧠 **Feynman drill** — Recite the two universal interview winners and the four-step adaptation decision tree from memory.

**Retrieval check**

**Q: An interviewer asks an open-ended 'design an AI feature for our customer' question. Your very first move should be:**

<details><summary>Show answer</summary>

**✓ Clarify the customer's users, data, constraints, and how success is measured — before proposing any architecture.**

Starting with the customer/requirements (not the tech) is the universal winning move and the top failure mode when skipped. Clarify first; architecture second.
</details>

---

[◂ Back to the handbook](../README.md) · [▸ Practice this interactively](https://deployprep.vercel.app/track/fde)
