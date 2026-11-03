# Forward Deployed Engineer (FDE) Behavioral & Customer-Facing Interview

> The human half of the job: scoping ambiguity, customer communication, and nailing the behavioral round.

[◂ Back to the handbook](../README.md)

---

## The FDE mindset: ambiguity, speed, ownership
_~6 min_

The technical skills get you to the loop; the **FDE mindset** gets you the offer. Forward-deployed work means dropping into a customer's messy reality and delivering value fast. Interviewers screen for a specific temperament:

- **Bias to action**: you'd rather ship a rough working prototype this week than a perfect spec next month. Demos over decks.
- **Comfort with ambiguity**: the problem is under-defined and the data is dirty. You make reasonable assumptions, state them, and move.
- **Extreme ownership**: you own the outcome, not just your code. You chase down the blocker, talk to the customer, and don't wait to be told.
- **Customer empathy**: you obsess over the user's actual problem, not the shiny tech. You can say 'AI is the wrong tool here' when it is.
- **Pragmatism**: the simplest thing that works, shipped, beats the elegant thing in a branch.
- **Leave them capable**: the job ends with the customer able to run, evaluate and extend what you built — runbooks, the eval suite, training their team. Microsoft's FDE leadership calls this **capability transfer**.

> 🎯 **Show, don't claim** — Don't say 'I'm comfortable with ambiguity.' **Prove it with a story**: a time the spec was vague, what assumptions you made, how you got to a working thing fast, and the outcome. Stories beat adjectives.

**◆ Lock it in**

- FDE temperament: **bias to action, ambiguity tolerance, extreme ownership, customer empathy, pragmatism**.
- **Demos over decks**; the simplest shipped thing wins.
- Prove traits with **concrete stories**, not adjectives.

> 🧠 **Feynman drill** — Tell a 60-second story that proves you can turn an ambiguous ask into a shipped result.

**Retrieval check**

**Q: A customer gives you a vague, half-baked problem and messy data, on a tight timeline. The FDE move is:**

<details><summary>Show answer</summary>

**✓ Make reasonable assumptions (stated explicitly), build a rough working prototype fast, and iterate with the customer.**

FDEs thrive in ambiguity: assume, prototype, demo, iterate. Waiting for a perfect spec or over-designing up front is the opposite of forward-deployed.
</details>

---

## Communicating with customers & non-engineers
_~6 min_

Half the job is talking to people who aren't engineers: customer ops leads, executives, domain experts. This is a scored skill in FDE loops.

- **Translate**: explain technical tradeoffs in business terms ('this cuts response time in half but raises cost ~20%'), not jargon.
- **Manage expectations**: be honest about what AI can/can't do. Set realistic accuracy expectations and explain that LLMs are probabilistic, not perfect. Overpromising kills trust.
- **Discovery**: ask about the **workflow and the pain**, not the feature they think they want. Find the real problem worth solving.
- **Demo well**: show working software early; a live demo builds more confidence than any slide.
- **Handle pushback**: when a stakeholder is frustrated or skeptical, listen, acknowledge, show data/a fix, and follow up. Stay calm and credible.

> 🧠 **You are a translator** — Picture yourself standing between a brilliant, literal, sometimes-wrong model and a busy human who just wants their problem solved. Your value is translating in both directions — and being trusted by both.

**◆ Lock it in**

- Translate tradeoffs into **business terms**; manage expectations honestly (LLMs are probabilistic).
- Do **discovery** on the workflow/pain, not the requested feature; demo working software early.
- Handle skepticism with **listen → acknowledge → show → follow up**.

> 🧠 **Feynman drill** — An exec asks 'why can't the AI just be 100% accurate?' Answer in plain, trust-building language.

**Retrieval check**

**Q: A non-technical customer expects the AI to be right 100% of the time. Best response?**

<details><summary>Show answer</summary>

**✓ Set honest expectations: LLMs are probabilistic, explain how you measure and raise accuracy, add guardrails/human review for high-stakes cases, and define an acceptable bar together.**

Trust comes from honesty: explain probabilistic behavior, show your eval/guardrail plan, and agree on an acceptable accuracy bar. Overpromising perfection destroys credibility later.
</details>

---

## The customer-scenario round: a framework that wins
_~7 min_

FDE loops include an open-ended customer scenario: "A logistics firm wants an AI agent to reroute shipments; they have SAP data, weather APIs, and 400 warehouse managers on different systems — how do you approach it?" The trap is **jumping to a solution**. Winners run a repeatable 5-step framework out loud.

1. **Clarify scope & success criteria**: What's the real goal? What does 'working' mean numerically? What's out of scope? (Never skip this — it's the #1 differentiator.)
2. **Map stakeholders**: Who's the exec sponsor, the daily users, the blockers (IT, security, procurement)? Adoption dies without the operators.
3. **Identify data & constraints**: What data exists, how dirty, where does it live, what are the compliance/latency/cost limits?
4. **Propose an approach with explicit tradeoffs**: simplest thing that works first; name what you're trading (quality vs cost vs speed vs build-vs-buy).
5. **Surface failure modes unprompted**: where will this break (bad data, injection, edge cases)? How will you **measure** it and roll back? This is the move that signals seniority.

> ⚠️ **The adoption trap** — A classic curveball: "You deployed the platform but adoption is 12% after 90 days and the customer blames the product." The answer is **not** technical first — it's **discovery**: talk to the operators, find the workflow friction, check whether you solved the real problem, then fix with data. Adoption is a people+workflow problem as much as a model problem.

> 🎯 **Live-demo-fails curveball** — "Your demo throws errors on screen in front of the client's execs — what do you do?" Stay calm, acknowledge it honestly, narrate what's happening, pivot to what **does** work or walk the approach verbally, and commit to a fixed follow-up. Composure under failure **is** the test.

**◆ Lock it in**

- Run the 5-step scenario framework: **clarify → stakeholders → data/constraints → approach w/ tradeoffs → failure modes**.
- Never jump to a solution; **clarify success criteria first**.
- Low adoption = a **discovery/workflow** problem; demo failures test your **composure**, not your luck.

> 🧠 **Feynman drill** — Recite the 5-step customer-scenario framework and apply it to 'an insurer wants to automate claims triage.'

**Retrieval check**

**Q: In a customer-scenario round, after clarifying scope the strongest next move is to:**

<details><summary>Show answer</summary>

**✓ Map stakeholders and identify the data/constraints before proposing an approach with explicit tradeoffs and failure modes.**

The framework is clarify → stakeholders → data/constraints → approach+tradeoffs → failure modes. Jumping to a model/code or over-promising autonomy is the classic failure.
</details>

---

## The behavioral round: STAR stories that land
_~7 min_

Behavioral rounds are not filler — for FDE they're often decisive. Use the **STAR** structure: **Situation, Task, Action, Result**. Prepare 5–6 flexible stories you can map to many questions.

#### Stories every FDE candidate should have ready

- A time you **shipped under ambiguity** / with an unclear spec.
- A time you **dealt with a difficult stakeholder** or unhappy customer.
- A time you **learned a new technology fast** to get something done.
- A time you **made a hard tradeoff** (scope, quality vs speed) and why.
- A time something **failed** — what you learned and changed (show growth, own it).
- A time you **drove impact** end-to-end / took ownership beyond your lane.

> 💡 **Quantify the result** — Weak endings sink great stories. End with **numbers and outcome**: 'cut triage time 40%,' 'shipped in 9 days,' 'customer renewed.' Result is the R in STAR — never skip it.

> 🎯 **Also: know why THIS company** — Have a crisp, specific reason for the company and the role — reference their product, mission, or approach. And prepare sharp questions for them; curiosity about the actual work reads as fit.

**◆ Lock it in**

- Use **STAR**; prepare 5–6 flexible stories covering ambiguity, conflict, fast learning, tradeoffs, failure, ownership.
- End every story with a **quantified result**.
- Have a **specific 'why this company/role'** and strong questions to ask them.

> 🧠 **Feynman drill** — Pick one of your real experiences and tell it in full STAR form, ending with a number.

**Retrieval check**

**Q: Which behavioral answer is strongest for an FDE role?**

<details><summary>Show answer</summary>

**✓ A STAR story: vague customer ask (S/T), the assumptions and prototype you built fast (A), and 'shipped in 2 weeks, cut manual review 40%, customer expanded usage' (R).**

A concrete STAR story with a quantified result demonstrates the exact FDE traits. Adjectives, tangents, and 'no failures' all read as weak or unreflective.
</details>

---

[◂ Back to the handbook](../README.md) · [▸ Practice this interactively](https://deployprep.vercel.app/track/fde)
