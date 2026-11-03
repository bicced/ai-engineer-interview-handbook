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

- **Multi-agent systems**: the shape that survived is an **orchestrator that owns the context plus sub-agents used as context firewalls** (isolated research or review that returns a compressed summary), with a **single writer** for actions. The labs now ship this as a platform feature — but it still costs several times the tokens (Anthropic measured **~15× a chat's tokens**, about **4× a single agent's**), so use it only when work genuinely parallelizes or would flood one context.
- **MCP (Model Context Protocol)**: the open standard for connecting agents to tools and data — 'USB-C for tools.' Governed by the Linux Foundation's **Agentic AI Foundation** (alongside A2A and Agent Skills); its July-2026 spec went **stateless**, so servers scale like ordinary web services. Expected knowledge — FDE postings list 'MCP servers' as a deliverable (next lesson goes hands-on).
- **Workflow vs agent**: if the steps are known and fixed, a **deterministic workflow** (hardcoded chain of LLM calls) is cheaper, faster, and more reliable than an autonomous agent. Use an agent only when the path is **dynamic** and can't be pre-scripted.

> 🎯 **The line that signals maturity** — "I'd start with the simplest thing that works — often a fixed workflow, not an agent. I add agency only where the task truly needs dynamic decision-making, because every added loop is added latency, cost, and a new way to fail."

> ⚠️ **Reliability compounds down** — If each step is 95% reliable, a 5-step agent is only ~77% reliable end-to-end (0.95^5). More autonomous steps = more compounding error. Constrain scope, add checks, and prefer fewer steps.

**◆ Lock it in**

- Prefer a **deterministic workflow** when the path is known; reserve **agents** for dynamic tasks.
- Multi-agent = **orchestrator + sub-agents as context firewalls**, one writer — ~4× a single agent's tokens, so justify it.
- **MCP** = the standard tool/data interface (Linux Foundation AAIF; stateless since the 2026-07-28 spec). Reliability **compounds down** across steps.

> 🧠 **Feynman drill** — Give an example task that needs a real agent, and one that should just be a fixed workflow. Justify each.

**Retrieval check**

**Q: A task has fixed, known steps every time (extract → validate → summarize → email). Best design?**

<details><summary>Show answer</summary>

**✓ A deterministic workflow (a fixed chain of LLM calls) — simpler, cheaper, more reliable.**

Known, fixed steps = deterministic workflow. Autonomous agents add cost, latency, and compounding failure with no benefit when the path never varies.
</details>

---

## Building MCP servers & skills (hands-on)
_~8 min_

FDE postings now list 'MCP servers', 'add an MCP tool, create a skill' and 'typed tools' as deliverables — expect to build one live or in a take-home. The core is small.

```
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("claims")

@mcp.tool()
def get_policy(policy_id: str) -> dict:
    """Fetch a policy's coverage, limits and deductible by id.
    Use before deciding anything about a claim. Amounts are USD."""
    return policies.lookup(policy_id)        # least privilege: read-only

@mcp.resource("claims://rules/auto-approval")
def approval_rules() -> str:
    """The current auto-approval rules, as text."""
    return load_rules()

if __name__ == "__main__":
    mcp.run(transport="streamable-http")   # "stdio" for a local tool
```

- Typed signatures + docstrings become the **schema + description** the model sees — write them like prompts: when to use it, when not to, units, formats.
- **Transports**: `stdio` for local tools; **Streamable HTTP** for remote or multi-user servers (the old HTTP+SSE transport is deprecated).
- **Stateless since the 2026-07-28 spec**: no initialize handshake or session id — every request carries its own version and capabilities, and any state lives in **explicit handles** your tools return and receive as arguments. Plain round-robin load balancing just works. Don't share one transport object across clients — responses can reach the wrong one.
- **Auth**: OAuth for remote servers, with clients identified by **Client ID Metadata Documents** (dynamic client registration is deprecated). Enforce permissions in the tool body — never in the description.
- **Long-running work**: the **Tasks** extension (start, then poll for the result); user input mid-call comes back as an `input_required` round-trip instead of a held-open stream. Roots, sampling and logging are deprecated.

#### Skills: packaging the customer's procedures

A **skill** is a folder with a `SKILL.md` — a name, a one-line description, then instructions — plus optional scripts and reference files. The agent sees only the description until the skill is relevant, then loads the rest (**progressive disclosure**), so one agent can carry hundreds of procedures. Introduced by Anthropic, now an open standard under the Linux Foundation's Agentic AI Foundation.

```
---
name: claims-intake
description: Use when a customer submits or asks about a new claim. Validates required
  fields and files it via the claims tools. Never quotes a payout amount.
---
# Claims intake
1. Collect: policy number, incident date, loss type, photos.
2. Call get_policy; stop and escalate if the policy is lapsed.
3. Run scripts/validate_claim.py on the draft; fix every error it reports.
4. File with file_claim. Confirm the claim id back to the customer.
```

#### MCP vs A2A

MCP connects an agent to **tools and data**; **A2A** connects an agent to **other agents** across vendors and organizations (each advertises an agent card; work moves as tasks). Both are now Agentic AI Foundation projects. Rule of thumb: if the other side is a deterministic API, wrap it as an MCP tool; if it's someone else's autonomous agent, use A2A.

> 🎯 **The whiteboard answer** — "Declare a server, register typed tools with real docstrings, run Streamable HTTP behind OAuth, keep it stateless so it scales horizontally, enforce least privilege inside each tool, log every call — and package the customer's procedure that uses those tools as a skill."

> ⚠️ **Tool sprawl and tool poisoning** — Fifty tools with overlapping descriptions wreck tool selection and burn context — consolidate, and use tool search / deferred loading. Treat third-party server metadata and results as untrusted (tool poisoning): pin and review servers like dependencies.

**◆ Lock it in**

- An MCP server = typed tools + docstrings (the model's schema) over **stdio** or **Streamable HTTP**; auth and least privilege live in the tool body.
- Since 2026-07-28 MCP is **stateless**: no session handshake, state in explicit handles, plain load balancing; long jobs via the **Tasks** extension.
- **Skills** = SKILL.md procedures loaded on demand; **MCP** = agent↔tools, **A2A** = agent↔agent.

> 🧠 **Feynman drill** — Explain to a customer's platform team how you'd expose their claims API to agents — MCP server, skill, or both — and why.

**Retrieval check**

**Q: Your MCP server runs behind a load balancer on the 2026-07-28 spec. Where should per-conversation state live?**

<details><summary>Show answer</summary>

**✓ In explicit state handles your tools return and receive as arguments (backed by your own store if needed).**

The 2026-07-28 spec removed sessions and the initialize handshake; state is carried in explicit handles passed as tool arguments, which is what lets plain round-robin load balancing work. Descriptions aren't state, and stateful work is fully supported.
</details>

---

## Harnesses & long-running agents: build vs rent
_~8 min_

A model in a loop isn't a product. The **harness** is everything around the loop: tool execution, a sandbox, context management, permissions and approvals, sessions that survive restarts, and tracing. In 2026 the harness — not the model — is where most forward-deployed agent engineering happens, and postings name it directly.

#### What a production harness provides

- **Sandboxed execution**: code and shell run in isolation with scoped, short-lived credentials and egress rules.
- **Context management**: server-side **compaction** of old turns, file-based progress notes and memory for long tasks, and tool search so tool definitions load on demand.
- **Programmatic tool calling ('code mode')**: the model writes code that calls tools inside the sandbox, so only results — not every intermediate payload — enter the context. OpenAI and Anthropic both ship it natively.
- **Permissions & approvals**: allow-lists, human approval for high-impact actions, spend budgets.
- **Durable sessions**: long-running and background agents (hours to days) that resume after failures.

#### Build vs rent

Every major vendor now rents you a harness: OpenAI's **Agents API** (the Codex harness as a service), Anthropic's **Managed Agents**, Google's Agent Runtime, AWS **Bedrock AgentCore**. Three questions decide it:

- **Data handling**: managed harnesses keep state server-side, and several are **not eligible for zero data retention or HIPAA**, or are region-restricted. For a bank or hospital that can settle it on day one.
- **Control & portability**: your own loop (or an open SDK — OpenAI Agents SDK, Claude Agent SDK, LangGraph, Microsoft Agent Framework, Google ADK) stays model-portable and debuggable; a managed harness ships faster but couples you to one vendor.
- **Operations**: rented harnesses give you sandboxes, compaction, sessions and approvals on day one — weeks of work to build well.

> 🎯 **The answer that lands** — "I'd prototype on a managed harness to prove value fast — after checking retention, residency and compliance eligibility. If the customer needs zero data retention or multi-model portability, I'd run an open SDK in their VPC and keep the tools as MCP servers, so either path works."

> ⚠️ **Long-running agents fail quietly** — The longer the run, the more you need durable progress notes, step and spend budgets, a resumable checkpoint — and monitoring of **what survives compaction** (summaries can drop constraints, and have been shown to carry injected instructions). 'It ran for 18 hours' isn't a success metric; the verified end state is.

**◆ Lock it in**

- The **harness** — sandbox, context management, permissions, sessions, tracing — is where agent engineering happens.
- **Code mode** and server-side **compaction** are now native platform features; know what they do to context and cost.
- **Build vs rent**: managed harnesses ship fast, but check **ZDR/HIPAA eligibility, residency and lock-in** first.

> 🧠 **Feynman drill** — A hospital wants a document-processing agent next month. Walk through whether you'd use a managed agent harness or run your own — and what you'd check first.

**Retrieval check**

**Q: A healthcare customer needs zero data retention. You're choosing between a vendor's managed agent harness and an open agent SDK in their VPC. What decides it first?**

<details><summary>Show answer</summary>

**✓ Whether the managed harness is eligible for zero data retention / HIPAA in their region — several aren't — before speed or features.**

Data-handling eligibility is a hard constraint: several 2026 managed agent products keep server-side state and aren't ZDR/HIPAA eligible. Check it first, then weigh speed vs portability. Stars and retired benchmarks don't decide compliance, and 'never' is an overcorrection.
</details>

---

[◂ Back to the handbook](../README.md) · [▸ Practice this interactively](https://deployprep.vercel.app/track/fde)
