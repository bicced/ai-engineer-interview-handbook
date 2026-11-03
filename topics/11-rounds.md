# Round-by-Round Playbook — Decomposition, Take-Homes, AI-Assisted & Discovery Rounds

> The four rounds that decide 2026 loops: decomposition, take-homes, AI-assisted & debugging rounds, and the discovery call.

[◂ Back to the handbook](../README.md)

---

## The decomposition round (Palantir-style)
_~7 min_

The signature FDE round — invented at Palantir, now copied across the industry — hands you a **vague, often non-technical problem** ('design technology to help low-vision elderly people cook,' 'reduce a city's 911 response times') and watches how you think for 45–60 minutes. Candidate reports consistently call it the **biggest filter** in FDE loops, and the #1 rejection is the same everywhere: **jumping to a solution**.

#### The drill (narrate every step)

1. **Clarify + scope**: who is this for, what does success mean **numerically**, what's explicitly out of scope?
2. **Users & stakeholders**: who uses it daily, who sponsors it, who can block it? (The elderly-cooking question is scored on **user empathy**, not tech.)
3. **Map the data & constraints**: what data exists, how dirty is it, where does it live, what can't change?
4. **Decompose into subproblems** sequenced by **risk × value** — find the natural seams and say why you'd attack them in that order.
5. **Walking skeleton**: propose the thinnest end-to-end slice that proves value in week one — then iterate.
6. **Failure modes + measurement**: where will it break, and how will you know? Volunteer this unprompted.

- **Narrate continuously** — interviewers read silence as stuck. Think out loud even while you're structuring.
- Expect **injected constraints** mid-round ('now assume half the users have no smartphone'). Treat them as gifts: restate, adapt one component, keep moving.
- State **explicit assumptions with numbers** ('let's say 200 dispatchers, 5k calls/day') instead of waiting for permission.

> 💡 **Practice set — drill these cold** — Cut a city's 911 response times (call + traffic + ambulance-GPS data) · reroute shipments for a logistics firm (SAP + weather APIs + 400 warehouse managers on different systems) · unify fraud detection across three acquired banks' inconsistent systems · 'our ops team doesn't trust the dashboard' · tech to help low-vision elderly cook. Two minutes of structure before any solution, every time.

**◆ Lock it in**

- The decomposition drill: **clarify/scope → users & stakeholders → data & constraints → decompose by risk×value → walking skeleton → failure modes + measurement**.
- **Narrate continuously**; treat injected constraints as gifts; state numeric assumptions.
- Empathy for the **end user** is scored as heavily as technical structure.

> 🧠 **Feynman drill** — Take '911 response times' and run the full six-step decomposition out loud in three minutes.

**Retrieval check**

**Q: Ninety seconds into a decomposition round you already have a great architecture idea. What should you do?**

<details><summary>Show answer</summary>

**✓ Hold it: clarify users, success criteria, data and constraints first, decompose — then introduce your idea as one option with tradeoffs.**

Jumping to a solution is the #1 documented rejection in this round. The round scores your **process** — scoping, stakeholder empathy, decomposition, sequencing — not the cleverness of an early idea.
</details>

---

## Take-homes & work trials: evals first, then the video
_~7 min_

Most AI-native loops now gate on a **build artifact**: OpenAI teams variously use a take-home with a **recorded video walkthrough** (one account: ~5 hours of work over about a week); candidates report Anthropic's app from a **fictional customer brief**; Sierra runs a 2-hour onsite **build with any AI tools**; agent startups hand out build-an-agent-for-a-fictional-client briefs; and some companies run paid multi-hour **work trials** in their real codebase. Some hiring managers now skip puzzles entirely and review a **prototype you already built**. Typical calibration: **2–4 hours of actual work** inside a multi-day window — scope ruthlessly.

#### What graders actually score

- **It runs**, end to end, on their machine, first try. A modest working thing beats an ambitious broken one.
- **An eval harness** — even 15–30 labeled cases with a pass/fail script. Hiring managers say it plainly: the red flag is a candidate who **doesn't start with evals**.
- **Production hygiene**: error handling, retries/backoff, logging, graceful degradation, no hardcoded keys.
- **Judgment in writing**: a short README with tradeoffs, cost/latency notes, and 'what I'd do with another week.'
- **Handled ambiguity**: the brief is deliberately underspecified — state your assumptions the way you would to a customer.

#### The video walkthrough (a customer-demo proxy)

1. Demo the **working thing first** (60–90 seconds).
2. Architecture in one diagram — **why this shape**.
3. One tradeoff you made **consciously**, and what you gave up.
4. Your **evals**: what you measured and what it showed.
5. What you'd do next. Keep it **≤5 minutes**, energy up — they're imagining you in front of their customer.

> ⚠️ **Check the AI rules** — Some take-homes **expect** you to use AI tools; others (Anthropic's, notably) prohibit them unless explicitly permitted. The rule is stated in the brief — read it, follow it, and never hide usage: transcripts and style make it obvious.

**◆ Lock it in**

- Take-homes are graded on: **it runs · eval harness · production hygiene · written tradeoffs · stated assumptions**.
- **Start with evals** — its absence is the documented red flag.
- Video: **demo → architecture → one tradeoff → evals → next steps**, ≤5 minutes.

> 🧠 **Feynman drill** — You get a 3-hour take-home: 'build a support agent for our fictional client.' Sketch your hour-by-hour plan.

**Retrieval check**

**Q: You have 3 hours for a take-home agent build. Which allocation wins the round?**

<details><summary>Show answer</summary>

**✓ ~2h on a working core slice + a small eval harness, ~30m hardening (errors/retries/logging), ~30m README + video walkthrough.**

Graders score a **running** slice, an **eval harness**, production hygiene, and communicated judgment — not feature count, UI polish, or documents about unbuilt things.
</details>

---

## AI-assisted & debugging rounds
_~7 min_

LLMs made classic take-homes and LeetCode trivially solvable, so loops split two ways: rounds where you're **graded on driving AI well**, and rounds engineered so AI **can't help**. By late 2026 roughly half of companies allow AI in interviews in some form (CoderPad), and the dominant AI-assisted format has converged: **an AI assistant + an unfamiliar multi-file repo + a bug to fix, then a feature, then optimization**. Prep both modes — and always ask which one you're in.

#### AI-assisted rounds (Meta, Google, OpenAI's Codex screen, Sierra, LinkedIn, Shopify)

- Know the variants: Meta's round runs **fix a bug → build the feature → optimize**, scored on problem solving, code quality, **verification** and communication. Google's Gemini round is **code comprehension**: read, debug and improve a repo you didn't write ('AI fluency' — prompting, output validation, debugging). OpenAI's Codex screen asks you to **narrate the time/space complexity of what the AI wrote**.
- The graded artifact is **your judgment**: prompt with intent (state the plan before asking), **verify aloud** (run/test everything the AI produces), visibly **reject bad suggestions**, and own the architecture yourself.
- Assume the **AI chat transcript is reviewed** — some platforms hand it to the interviewer. 'Paste task, paste output' reads as unskilled.
- Some interview AIs are deliberately **weakened** to test whether you catch their mistakes. Treat every suggestion like a junior engineer's PR.
- **AI interviewers** are here too: platforms now run whole technical screens with an AI asking the follow-ups (HackerRank's Chakra went GA in Oct 2026). Same rules — narrate, verify, own the design.

#### No-AI rounds: debugging an unfamiliar codebase

- Format (Palantir's re-engineering round, Scale, many proctored screens): a multi-file agent/app plus a spec of **intended behavior**; find and fix several bugs, no AI tools, code must run. (Sierra's piloted version has you improve a colleague's draft PR **with** coding agents — check which mode you're in.)
- Method: read the spec first → trace **one request end-to-end** → diff intent vs. behavior → instrument (prints/logs) at the seam → fix, **run to prove it**, then hunt the next.
- Beware **red herrings** — ugly-but-working code is planted on purpose. Fix behavior, not style.

> 🎯 **The meta-skill** — Both formats test the same 2026 competency: **can you judge code** — yours, the AI's, or a stranger's — rather than merely produce it. Narrate your verification and you pass both.

**◆ Lock it in**

- AI-assisted rounds grade **how you drive AI**: intent-first prompts, verification aloud, visible rejection of bad output.
- Debugging rounds: **spec → trace one request → diff intent vs behavior → instrument → fix and prove**; ignore planted red herrings.
- Always **ask what tools are allowed** before every round.

> 🧠 **Feynman drill** — Explain how you'd behave differently in an AI-allowed coding round vs. a no-AI debugging round — and what's common to both.

**Retrieval check**

**Q: In an AI-assisted coding round, the assistant produces a plausible-looking function. The strongest move is:**

<details><summary>Show answer</summary>

**✓ Say what you expect it to do, run/test it, call out any flaw, and fix or re-prompt — all out loud.**

These rounds grade **verification and judgment**, not raw output. Blind pasting is the documented failure mode; narrated testing — and visibly catching a planted mistake — is exactly what scores.
</details>

---

## Coding-agent fluency: the new minimum qualification
_~7 min_

Late-2026 postings list 'using coding agents to develop code and tests to specification' as a **minimum** qualification, OpenAI hires AI Deployment Engineers specifically to roll Codex out at customers, and AI-assisted rounds grade how you drive an agent. Fluency means engineering discipline around the agent — not prompt tricks.

#### How senior engineers drive coding agents

- **Spec and tests first**: write (or have the agent draft, then review) the tests that define done; the agent iterates against them.
- **Repo instructions**: an `AGENTS.md` / `CLAUDE.md` with build commands, conventions and no-go zones; skills for repeated procedures.
- **Plan, then small diffs**: ask for a plan before code, implement in slices, run everything, read every diff like a junior's PR.
- **Parallelize safely**: several agents on isolated branches or worktrees for independent tasks; one owner merges.
- **Verify, don't trust**: check complexity and edge cases; watch for invented APIs and silently weakened tests.

#### Rolling coding agents out across a customer's org

- **Pick bounded, measurable workflows**: test generation, migrations, bug triage, dependency upgrades.
- **Baseline first**: cycle time, PR throughput, escaped defects — then measure the same after.
- **Guardrails**: sandboxed execution, no production secrets in the agent's environment, human review on merge, permission policies — and treat repo files and issue text as untrusted input (2026 incidents turned both into attack paths).
- **Enablement**: shared AGENTS.md conventions, skills for the org's procedures, office hours — capability transfer, not a one-off demo.

> ⚠️ **The quiet failure: weakened tests** — Agents under pressure to make tests pass sometimes edit the test instead of the code. Diff the tests too, and make 'don't modify tests without asking' an explicit rule.

> 🎯 **Say this** — "I write the acceptance tests first, give the agent the repo's conventions in AGENTS.md, have it propose a plan before touching code, and review every diff — including the tests — before anything merges."

**◆ Lock it in**

- Coding-agent fluency is now a **baseline qualification**: spec/tests first, repo instructions, plan → small diffs → verify.
- Rolling agents out at a customer = **bounded workflows, a measured baseline, sandbox + review guardrails, enablement**.
- Always **diff the tests** — weakened tests are the classic silent agent failure.

> 🧠 **Feynman drill** — A customer's VP of Engineering asks how you'd roll coding agents out to 200 developers safely. Give your 90-day plan.

**Retrieval check**

**Q: In an AI-assisted round, the agent's fix makes every test pass. What do you check first?**

<details><summary>Show answer</summary>

**✓ Whether it modified or weakened the tests — then the diff's logic, complexity and edge cases.**

Agents can 'pass' by editing tests. Verifying the tests are intact, then reviewing logic, complexity and edge cases, is exactly the verification skill these rounds grade.
</details>

---

## The discovery-call simulation
_~7 min_

A newer round — most often reported at Anthropic and agent startups — puts you in a **role-play discovery call**: the interviewer plays a VP at a fictional enterprise; you have 30–60 minutes. Candidate reports (not company-confirmed) call it one of the **strongest offer predictors** in the loop, and the failure mode is universal: **pitching instead of discovering**.

#### Run it like a real discovery call

1. **Open with their world, not your product**: 'walk me through how this works today' — map the workflow and where it hurts.
2. **Ask about past attempts**: 'what have you tried — including AI — and what happened?' Failed-deployment stories are gold.
3. **Quantify the pain**: how often, how expensive, who feels it?
4. **Find the immovables**: compliance, data residency, systems that can't change, security review, procurement.
5. **Identify the people**: daily users, exec sponsor, likely blockers (IT, security, legal).
6. **Define success together**: 'if this works, what number moves?' — and how do they judge quality today?
7. **Close crisply**: summarize what you heard, state what you'd validate first, propose a concrete next step.

- **Don't architect in the first half.** If pushed — 'so what would you build?' — earn it: 'a couple more questions so I don't design the wrong thing.'
- Aim to **listen more than you talk**; take visible notes; use their vocabulary, not yours.
- It's OK to **not know**: 'I'd want to see the data before promising that' is a senior answer in this round.

> 🎯 **Questions that score** — 'What does your team do today when the model gets it wrong?' · 'How would you evaluate whether this is working — what's the number?' · 'What broke in your last AI pilot?' · 'Who has to say yes for this to reach production?' Each one signals you've shipped into a real organization before.

**◆ Lock it in**

- **Discover, don't pitch**: today's workflow → past attempts → quantified pain → immovables → people → a success number → crisp close.
- Hold off on architecture; **listen more than you talk**; admit what you'd need to verify.
- Ask about **failed pilots and current eval criteria** — the highest-signal questions in the round.

> 🧠 **Feynman drill** — You're 5 minutes into a discovery call with a VP of Support at an insurer. Say your next four questions, in order, and why.

**Retrieval check**

**Q: Ten minutes into a discovery-call simulation, the 'VP' asks what you'd build. Best response?**

<details><summary>Show answer</summary>

**✓ Briefly acknowledge, then ask the remaining discovery questions (success metric, constraints, past attempts) before sketching a direction with stated assumptions.**

The round tests discovery discipline. You don't stonewall — you earn the design: acknowledge, close the key unknowns, then propose a direction explicitly tied to what you heard.
</details>

---

[◂ Back to the handbook](../README.md) · [▸ Practice this interactively](https://deployprep.vercel.app/track/fde)
