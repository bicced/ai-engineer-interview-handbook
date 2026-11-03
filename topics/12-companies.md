# FDE Interview Process by Company — Anthropic, OpenAI, Palantir, Big Tech & Startups

> How the FDE / Applied AI loop actually runs at Anthropic, OpenAI, Palantir, the clouds and agent startups — official vs reported, with sources.

[◂ Back to the handbook](../README.md)

---

## Anthropic: Applied AI & FDE interview process
_~7 min_

Anthropic hires Applied AI engineers, Forward Deployed Engineers and solutions architects to deploy Claude with enterprises. Separate what's **official** from what's **reported** — prep sites copy each other, and the official parts are the ones you can't get wrong.

#### Official

- **AI policy**: no AI assistance on take-homes or in live interviews **unless Anthropic explicitly says otherwise** (its candidate AI-guidance page).
- **What postings ask for** (e.g. a London FDE posting, Sep 2026): building **MCP servers, sub-agents and agent skills**, evaluation frameworks, 4+ years in customer-facing roles, 25–50% travel. Posted US FDE base: roughly $280–320K.
- **Ecosystem**: the Claude Frontier Academy (Oct 2026) trains partner and consultancy engineers with a graded deployment practical — a signal of what 'good deployment work' means to them.

#### Reported by candidates (unverified)

- Practical coding: one problem in **escalating levels**.
- A take-home built from a **fictional customer brief**.
- A **discovery-call role-play** with an interviewer playing a customer executive.
- A design round centered on **eval harnesses**.
- Practices vary by team — one Anthropic engineering manager publicly dropped coding interviews for his team in favor of reviewing prototypes (Oct 2026).

#### How to prep

- Practice building **without AI**, under time pressure — the policy is real.
- Have a shipped **MCP server + skill + eval harness** you can walk through in five minutes.
- Drill discovery (Round-by-Round Playbook) and eval-harness design (Evals module).
- Know the platform vocabulary — effort levels, compaction, skills, Managed Agents — and when you would **not** use a managed product (retention, compliance).

> 💡 **Sources & confidence** — Official: Anthropic candidate AI guidance (unchanged since Jul 2025); Anthropic FDE posting (Sep 2026); Claude Frontier Academy announcement (Oct 2026). Reported: candidate write-ups on prep sites — low confidence. Team-level practice: The Register (Oct 28, 2026).

**◆ Lock it in**

- Official: **no AI unless explicitly permitted**; postings name **MCP servers, sub-agents, skills, evals**.
- Reported: escalating-level coding, fictional-customer take-home, discovery role-play, eval-harness design.
- Prep: build without AI, bring a shipped MCP/skill/eval artifact, drill discovery and eval design.

> 🧠 **Feynman drill** — Explain why you'd prep differently for Anthropic's take-home than for Sierra's onsite build.

**Retrieval check**

**Q: Anthropic's take-home brief doesn't mention AI tools. What do you do?**

<details><summary>Show answer</summary>

**✓ Assume no AI assistance unless explicitly permitted, and ask the recruiter if unsure.**

Anthropic's published guidance: no AI on take-homes or live rounds unless they say otherwise. When unsure, ask — hidden AI use is both a policy breach and usually obvious.
</details>

---

## OpenAI: FDE & AI Deployment Engineer interview process
_~7 min_

OpenAI hires **Forward Deployed Engineers** and **AI Deployment Engineers** — the latter with tracks for Codex, ChatGPT plugins/ecosystem, startups and digital natives. Its deployment organization grew fast in 2026 (a dedicated deployment company plus an acquired consultancy), and the loop is **not uniform**: it varies by team.

#### Reported loop shapes (candidate reports, 2026)

- A **take-home** with about a week to submit, plus a **recorded video walkthrough** — one account describes ~5 hours of build on OpenAI's APIs.
- An **AI-enabled coding screen in a Codex environment**: the assistant writes code and you **narrate its time and memory complexity**, test it and improve it.
- Alternatively, back-to-back coding and system-design screens, or a deep dive on a project you led; some teams ask for a design doc with no code.
- Later rounds: customer solution design and a defense of your build — rate limits, retries, 'how do you know it works?'

#### How to prep

- Practice driving a coding agent **while explaining** what it produced — complexity, edge cases, failure modes.
- Know the current platform: the **Responses API** (the Assistants API shut down in Aug 2026), Agents SDK and the managed Agents API, structured outputs, prompt caching (cache writes are now billed), and reasoning effort. The hosted Evals product shut down in Nov 2026 — bring your own eval harness.
- For AI Deployment Engineer roles, prepare a story about **rolling a coding agent out** to a team: workflow choice, baseline metrics, guardrails, enablement.
- Record yourself doing a five-minute walkthrough: demo → architecture → one tradeoff → evals → next steps.

> 💡 **Sources & confidence** — OpenAI careers postings for AI Deployment Engineer tracks (2026); OpenAI's 'What FDE means at OpenAI' post (Oct 22, 2026); CNBC on OpenAI's deployment company (Oct 15, 2026); OpenAI API deprecations page. Loop details: candidate reports via interview-prep sites (2–3 reports) — medium-low confidence.

**◆ Lock it in**

- Titles: **FDE** and **AI Deployment Engineer** (Codex, plugins, startups, digital-native tracks).
- The loop **varies by team**: take-home + video, an **AI-enabled Codex screen**, or coding + design screens.
- Prep: narrate AI-written code, know Responses/Agents APIs (Assistants and hosted Evals are gone), bring a coding-agent rollout story.

> 🧠 **Feynman drill** — Narrate, as if in OpenAI's Codex screen, how you'd review an assistant-written function you didn't write.

**Retrieval check**

**Q: In an AI-enabled coding screen, the assistant writes a working solution in seconds. What earns the credit?**

<details><summary>Show answer</summary>

**✓ Explaining its time/space complexity and edge cases, testing it, and improving it out loud.**

These screens grade verification and judgment: complexity, edge cases, tests and improvements you can articulate. Speed alone, refusing the tool, or parroting its explanation all miss the point.
</details>

---

## Palantir: FDSE & Deployment Strategist interview process
_~6 min_

Palantir invented the role and runs two flavors: the **Forward Deployed Software Engineer (FDSE)** — historically 'Delta', the embedded builder — and the **Deployment Strategist** — historically 'Echo', who finds the problem worth solving. Palantir publishes no public rubric; everything below is from consistent candidate reports.

#### The reported rounds

- **Decomposition**: a vague, often non-technical problem ('help low-vision elderly people cook', 'cut 911 response times'), scored on process, user empathy and sequencing — the biggest filter.
- **Re-engineering**: find real bugs — and ignore planted red herrings — in a large unfamiliar codebase.
- **Learning**: build something with a library or tool you've never used, on the spot.
- Behavioral and 'why Palantir' — including a considered view on its government and defense work.
- **AI tools**: candidates consistently report they're not allowed.

#### How to prep

- Drill decomposition out loud (Round-by-Round Playbook) until 'clarify → users → data → decompose → walking skeleton → measure' is reflex.
- Practice reading and debugging large codebases **without** AI help.
- Practice learning a new library from its docs in 30 minutes and shipping something small.
- Prepare an honest, specific answer on mission fit — vague enthusiasm reads poorly.

> 💡 **Sources & confidence** — Candidate reports and interview guides (Jul–Aug 2026), consistent with earlier years; no official Palantir description. Round structure: medium confidence. AI ban: reported, not published policy.

**◆ Lock it in**

- Two roles: **FDSE** (build) and **Deployment Strategist** (find the problem).
- Reported rounds: **decomposition, re-engineering, learning**, behavioral — AI reportedly banned.
- Prep: decomposition drills, no-AI codebase reading, learn-a-library sprints, a real mission-fit answer.

> 🧠 **Feynman drill** — Run the decomposition drill on 'reduce food waste in a hospital cafeteria' in two minutes, out loud.

**Retrieval check**

**Q: In Palantir's learning round you're handed an unfamiliar library. Best approach?**

<details><summary>Show answer</summary>

**✓ Skim the quick-start, get the smallest end-to-end example running, then extend it — narrating as you go.**

The round tests how fast you learn by doing: get a minimal example running, then iterate, narrating your reasoning. Exhaustive reading or avoiding the tool defeats the purpose.
</details>

---

## Google, Microsoft, AWS, Databricks & the consultancies
_~7 min_

In 2026 the big platforms and consultancies became the largest FDE employers. Their loops blend standard big-company interviewing with deep platform knowledge.

#### Google & Google DeepMind

- A spokesperson said in Oct 2026 that **AI fluency is a requirement for all software-engineering jobs**; a Gemini-assisted **code-comprehension** round (read, debug and improve an unfamiliar repo) is expanding from its pilot.
- Google Cloud FDE postings ask for building **MCP servers** and fluency in **tokens/sec and cost-per-request**; a DeepMind applied-AI posting lists 'using coding agents to develop code and tests to specification' as a minimum qualification.
- One FDE screen report: agentic system design (stuck agents, human-in-the-loop), then a small coding task that grew into a repo-wide migration discussion.

#### Microsoft

- Microsoft's head of Forward Deployed Engineering describes the unit as a **multidisciplinary pod** — engineers, domain experts, a TPM, security and governance — and lists multi-agent systems, agent harnesses, MCP, evals, observability, identity, cost/latency and **capability transfer** as the core skills.

#### AWS & Databricks

- AWS committed $1B to a new FDE organization (Jun 2026), starting with hundreds of engineers; its leadership calls the right candidates 'unicorns' — strong engineers who are also customer-ready. Expect Bedrock / AgentCore depth.
- Databricks formalized an FDE organization in 2026 — expect data-platform depth (pipelines, governance) alongside agents.

#### Consultancies

- Deloitte, Accenture and KPMG are among the **largest FDE posters**, often for a single vendor's stack ('Anthropic FDE', 'Google Cloud FDE'), sometimes requiring security clearance.
- Accenture and Google Cloud are reskilling engineers into a 1,000-person FDE workforce in 30–90 days — loops weight client delivery, communication and vendor certifications.

> 🎯 **Prep for platform roles** — Go deep on **that vendor's agent stack** (harness, model lineup, data and compliance terms), bring a delivery story with numbers, and expect classic big-company coding and behavioral rounds alongside the AI ones.

> 💡 **Sources & confidence** — Business Insider via syndication (Oct 20, 2026) on Google's AI-fluency requirement and interview changes; Google/DeepMind/Adobe postings (Oct 2026); Microsoft 'Command Line' post by its FDE CVP (Oct 28, 2026); Reuters (Jun 30, 2026) and Business Insider (Oct 4, 2026) on AWS; Databricks blog (Jun 2026); CNBC/Lightcast (Oct 15, 2026). Single FDE screen report from Blind — low confidence.

**◆ Lock it in**

- Google: **AI fluency required** for SWE roles; a Gemini **code-comprehension** round; FDE postings want MCP servers and tokens/sec math.
- Microsoft runs FDE as **pods**; AWS built a $1B FDE org; Databricks formalized one.
- Consultancies post the most FDE roles — usually **vendor-specific**; prep that vendor's stack and delivery stories.

> 🧠 **Feynman drill** — Explain how you'd tailor your prep for a 'Google Cloud FDE' role at a consultancy versus an FDE role at Google itself.

**Retrieval check**

**Q: You're interviewing for a vendor-specific FDE role at a large consultancy. What's most worth adding to your prep?**

<details><summary>Show answer</summary>

**✓ Depth in that vendor's agent stack plus client-delivery stories with numbers (and its certifications, if the posting lists them).**

Vendor-specific consultancy roles hire for delivering one platform to many clients: platform depth, delivery evidence and (sometimes) certifications. Pure algorithm grinding or ignoring the technical bar both miss.
</details>

---

## Agent startups: Sierra, Harvey, Glean, Decagon & co.
_~7 min_

Agent-native startups run the most experimental loops — and Sierra is unusual in publishing its own.

#### Sierra (official, Apr 2026)

- Phone screen: **system design** — it replaced the earlier no-AI coding screen.
- Onsite: **Plan → 2-hour Build with any AI tools → Review** — a demo, code review, the path to production, and how you used AI.
- A piloted round: improve a colleague's draft PR **iterating with coding agents**; the AI rules are still being tuned.
- Sierra maintains **τ-bench** (2.0 added voice and multilingual tasks) — know pass^k and simulated-user evals before you walk in.

#### Harvey (reported)

- An Aug 2026 report for an AI Agent role: a spreadsheet-style screen, a project deep dive, and an **'improve an eval metric'** question; another 2026 report mentions an AI-assisted onsite.
- Legal domain: expect rubric-graded evals and domain-expert review — Harvey has described RL post-training of an open-weight model on simulated legal tasks graded against attorney rubrics.

#### Glean, Decagon, Scale, Cohere (reported)

- **Glean**: still reports algorithm rounds plus system design — enterprise search means permission-aware retrieval depth.
- **Decagon, ElevenLabs**: practical builds and customer case studies; voice-agent depth helps.
- **Scale** titles some roles 'Frontier Agents Engineer (FDE)'; **Cohere** candidates describe coding as 'not LeetCode'.

> 🎯 **Prep for agent startups** — Get fast at **building with AI tools and explaining the result**, practice agentic system design, and have one domain eval story — how you defined 'correct' with experts and measured it.

> 💡 **Sources & confidence** — Sierra engineering blog, 'The AI-native interview' (Apr 22, 2026) — high confidence. Harvey, Glean, Cohere, Decagon, Scale: individual candidate reports (Feb–Sep 2026) and prep sites — low confidence. Harvey post-training: industry press (Aug 2026), self-reported results.

**◆ Lock it in**

- **Sierra** (official): system-design screen → Plan / 2-hr Build with any AI / Review; a piloted PR-with-agents round.
- **Harvey**: practical screens + 'improve an eval metric'; **Glean**: still DSA + system design (reported).
- Prep: build-with-AI fluency, agentic design, and a domain eval story.

> 🧠 **Feynman drill** — Contrast Sierra's onsite with Palantir's re-engineering round: what's each really testing?

**Retrieval check**

**Q: Per Sierra's own description, what replaced its no-AI coding phone screen?**

<details><summary>Show answer</summary>

**✓ A system-design screen.**

Sierra's Apr-2026 post says the phone screen is now system design; AI-assisted building moved to the onsite Plan → Build → Review.
</details>

---

[◂ Back to the handbook](../README.md) · [▸ Practice this interactively](https://deployprep.vercel.app/track/fde)
