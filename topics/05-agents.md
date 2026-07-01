# AI Agents & Tool-Use Interview Questions (Function Calling, ReAct, MCP)

> Give models the ability to act: function calling, agent loops, planning, multi-agent, MCP, and failure control.

[◂ Back to the handbook](../README.md)

---

## Tool use / function calling — models that act
_~7 min_

A raw LLM only produces text. **Tool use** (a.k.a. function calling) lets it **request actions**: you describe available functions (name, description, parameter schema); the model, instead of answering, emits a structured request to call one; **your code** executes it and returns the result; the model continues with that result.

1. You define tools with a JSON schema: `get_weather(city: str)`, `search_orders(user_id: str)`, etc.
2. The model decides **whether** and **which** tool to call and with **what arguments**.
3. Your runtime executes the real function and returns the output to the model.
4. The model incorporates the result and either answers or calls another tool.

> 💡 **The model never runs code** — The LLM only **asks** to call a tool — **your** code runs it. This is where you enforce auth, validation, rate limits, and safety. Treat tool arguments as **untrusted input**.

> 🎯 **Why this matters** — Tool use is how LLMs overcome their limits: no live data → give it a search tool; bad at math → give it a calculator; can't act → give it an API. 'What can't the model do, and what tool fixes it?' is core FDE thinking.

**◆ Lock it in**

- Tool use = model emits a **structured request**; your code executes and returns the result.
- The model chooses the tool + arguments; **you** own execution, auth, and validation.
- Tools patch model weaknesses: live data, math, actions, private systems.

> 🧠 **Feynman drill** — Walk through what happens, step by step, when a model answers 'what's the weather in Tokyo?' using a weather tool.

**Retrieval check**

**Q: In function calling, who actually executes the function?**

<details><summary>Show answer</summary>

**✓ Your application code executes it; the model only emits a structured request to call it.**

The model only produces a structured call request. Your runtime executes it — which is exactly where you enforce authorization, validation, and safety.
</details>

---

## The agent loop: ReAct, planning & memory
_~7 min_

An **agent** is an LLM in a loop that can use tools to pursue a goal over multiple steps, deciding its own next action. The canonical pattern is **ReAct** (Reason + Act):

1. **Thought**: the model reasons about what to do next.
2. **Action**: it calls a tool.
3. **Observation**: it gets the tool's result.
4. Repeat until the goal is met, then produce a final answer.

- **Planning**: for complex tasks, have the agent draft a plan / decompose into sub-tasks first, then execute — improves reliability.
- **Memory**: short-term = the running context; long-term = external store (vector DB / scratchpad) the agent reads and writes across steps.
- **Termination**: you must cap steps/iterations and cost — agents can loop forever or thrash.

> 🧠 **Mental model** — An agent is an intern with a to-do list and a phone: it thinks, makes a call (tool), reads the reply, updates its plan, and repeats — until the task is done or it hits the limits you set.

**◆ Lock it in**

- Agent = **LLM in a loop** using tools toward a goal; **ReAct** = Thought → Action → Observation, repeat.
- Add **planning** for complex tasks and **memory** (short-term context + long-term store).
- Always enforce **step, time, and cost limits** — agents can loop.

> 🧠 **Feynman drill** — Explain the ReAct loop and why an agent needs a hard stop condition.

**Retrieval check**

**Q: What does the 'Observation' step in a ReAct agent loop refer to?**

<details><summary>Show answer</summary>

**✓ The result returned from executing a tool/action, fed back into the model's context.**

Observation = the tool's output returned to the model, which then reasons about the next step. It's the feedback that makes the loop work.
</details>

---

## Multi-agent, MCP & when NOT to build an agent
_~6 min_

Agents are powerful and **over-used**. Senior judgment is knowing when a simpler design wins.

- **Multi-agent systems**: specialized agents (planner, researcher, coder, critic) collaborate. More capable for complex work, but more latency, cost, and failure surface. Use only when a single agent genuinely can't cope.
- **MCP (Model Context Protocol)**: an open standard for connecting models to tools/data sources through a uniform interface — think 'USB-C for tools.' Increasingly expected knowledge.
- **Workflow vs agent**: if the steps are known and fixed, a **deterministic workflow** (hardcoded chain of LLM calls) is cheaper, faster, and more reliable than an autonomous agent. Use an agent only when the path is **dynamic** and can't be pre-scripted.

> 🎯 **The line that signals maturity** — "I'd start with the simplest thing that works — often a fixed workflow, not an agent. I add agency only where the task truly needs dynamic decision-making, because every added loop is added latency, cost, and a new way to fail."

> ⚠️ **Reliability compounds down** — If each step is 95% reliable, a 5-step agent is only ~77% reliable end-to-end (0.95^5). More autonomous steps = more compounding error. Constrain scope, add checks, and prefer fewer steps.

**◆ Lock it in**

- Prefer a **deterministic workflow** when the path is known; reserve **agents** for dynamic tasks.
- Multi-agent adds capability but multiplies cost/latency/failure — use sparingly.
- **MCP** = standard interface for tools/data ('USB-C for tools'). Reliability **compounds down** across steps.

> 🧠 **Feynman drill** — Give an example task that needs a real agent, and one that should just be a fixed workflow. Justify each.

**Retrieval check**

**Q: A task has fixed, known steps every time (extract → validate → summarize → email). Best design?**

<details><summary>Show answer</summary>

**✓ A deterministic workflow (a fixed chain of LLM calls) — simpler, cheaper, more reliable.**

Known, fixed steps = deterministic workflow. Autonomous agents add cost, latency, and compounding failure with no benefit when the path never varies.
</details>

---

[◂ Back to the handbook](../README.md) · [▸ Practice this interactively](https://YOUR-DOMAIN/track/fde)
