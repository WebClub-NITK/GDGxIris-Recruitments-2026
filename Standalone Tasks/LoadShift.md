## Task ID: LoadShift

#### `Agentic AI`, `Full Stack Web Development`, `Multi-Agent Systems`, `Routing`

Mentors: [Aditi Sinha](https://github.com/aditi0556) ([+91 9328392381](https://wa.me/919328392381))

Difficulty: `Medium-Hard`

### Description

Build **LoadShift**, a tiny delivery desk that reassigns work when a driver drops, an urgent order lands, or a road closes. The point is the **agent loop**, not a maps product.

Write **`LoadShift`** in your private-repo README. Submit using the recruitment README: **private** GitHub repo, mentors added as collaborators, **demo video required**. Do not make the repo public. Do not paste API keys, `.env` values, or model secrets into the README; name the variable (`GEMINI_API_KEY`) and keep the value in a gitignored `.env`.

**A solver-only script does not complete this task.** An evening of OR-Tools / Dijkstra with `handle(event)` and no model is a fail. All three agents below are required. Every assignment or reroute in the baseline run must go through an **LLM tool-calling** loop. A hardcoded if/else on the three events is a fail. Missing tool-call logs for `t=0`, `t=5`, `t=8`, and `t=12` is a fail.

Use the seed below as-is. Copy it into the repo (JSON is fine). Do not invent another map. Google Maps is **not** allowed on the required path.

**Time:** edge weight is travel time in minutes. `1` minute of travel = `1` sim minute. Events fire at `t=0` (first assign), `t=5`, `t=8`, and `t=12`. Between events, follow the current route. A stop is `delivered` if planned arrival ≤ event time; the driver is then at that node. Do not model mid-edge position: if the next leg would still be in progress, stay at the last finished node. If a deadline cannot be met on the remaining graph, mark the order `failed` and free the slot.

**Assignment rules:** an order has **at most one** assignee. Do not bump work already assigned to make room. Only **open-pool** `normal` orders wait behind `urgent` ones. `urgent` beats `normal`. Among the same priority, the earlier deadline wins.

Grade **invariants**, not the same driver–order pairing every run. A shorter feasible route is better than a longer one; there is no “globally optimal VRP” bar.

### Seed

Pickup for every order is `W`. Drivers `D1`, `D2`, `D3` start at `W`, capacity **2**, status `available`. All names and nodes are **fake**. Do not load real people, phones, or GPS.

**Nodes:** `W`, `A`, `B`, `C`, `D`, `E`

**Undirected edges** `(from, to, minutes)`:

`W-A 4`, `W-B 5`, `W-C 8`, `A-B 3`, `A-D 6`, `B-C 4`, `B-E 5`, `C-D 5`, `C-E 3`, `D-E 4`

**Orders at `t=0`:**

| id | drop | deadline | priority |
| --- | --- | --- | --- |
| `O1` | `A` | 15 | normal |
| `O2` | `B` | 16 | normal |
| `O3` | `C` | 22 | normal |
| `O4` | `D` | 24 | normal |
| `O5` | `E` | 26 | normal |
| `O6` | `D` | 28 | normal |

**`O7` arrives at `t=8`:** drop `C`, deadline `20`, priority `urgent`.

### Event contract

The world is this simulation only. Do not feed free-text notes to the model as “events.” Ignore any payload whose `id` / `type` is not in this allow-list. Tools may only read and write sim state (`get_state`, `assign`, `reassign`, `route`, `fail_order`). No shell, no outbound HTTP, no Maps client.

Replay the same `id` is a **no-op** (idempotent). Apply events in increasing `t`; if `t` ties, apply by `id` ascending. `driver_offline` before `order_arrive` when both would hit the same driver.

| id | t | type | payload |
| --- | --- | --- | --- |
| `e0` | 0 | `assign_initial` | seed orders `O1`–`O6` |
| `e1` | 5 | `driver_offline` | `driverId: D2` |
| `e2` | 8 | `order_arrive` | `O7` as above |
| `e3` | 12 | `edge_closed` | `edge: W-C` |

### Features to Implement

1. **Required agents**

   * **Orchestrator:** reads the allow-listed events, calls the other two agents as LLM tools, applies results to the sim.
   * **Assignment agent:** LLM-backed tool. Maps open orders to drivers using capacity, priority, and deadlines.
   * **Routing agent:** LLM chooses when to call it. It returns an ordered node list on the **current graph**. Implement routing **on this graph** (Dijkstra is enough). OR-Tools may sit behind that tool. If OR-Tools fails, quota-errors, or is missing, Dijkstra on the seed graph must still finish the run. Maps is not a fallback.

2. **Web dashboard**

   * One **dispatcher** page for this fake fleet. A table is enough; a map is not graded. No login, and no public “every driver sees every order” product: if you deploy, deploy this seed only, not real addresses.
   * Show each driver: current node, assigned orders, remaining capacity, status (`available`, `en route`, `offline`).
   * Show each order: status (`unassigned`, `assigned`, `delivered`, `failed`), drop node, deadline, priority, assignee (empty or exactly one driver).
   * After each event, the page updates without a full reload.

3. **Scripted events**

   Replay `e1`–`e3` (after `e0`) against the seed. The dashboard and the decision log must show them.

   * **`e1` / `t=5` — `D2` goes offline:** undelivered orders on `D2` return to the open pool (zero assignees). The orchestrator must run assignment + routing again. **Invariant:** `D2` is `offline`, holds no orders, and receives none while offline.
   * **`e2` / `t=8` — `O7` arrives:** assign open-pool `urgent` before open-pool `normal` if a feasible driver exists (`D1` or `D3` has spare capacity and some path from their current node meets the deadline). **Invariant:** `O7` is assigned to `D1` or `D3`, or `failed` with a reason. `D2` does not take it. No order has two assignees.
   * **`e3` / `t=12` — edge `W-C` closes:** remove `W-C` from the graph. Re-route remaining work. If a driver is on `W-C`, snap them to the node they left; that leg does not complete. **Invariant:** no remaining route uses `W-C`. Orders that cannot meet their deadline on the new graph are `failed` and the slot is freed.

4. **Decision log**

   * Persist a timeline: time, event `id`, tool calls (name, arguments, result), new assignments, failures.
   * **Invariant:** a replay of this seed fires `e0`–`e3` in order. A second delivery of `e1` (or any id) does not assign twice or drop the order. The log has tool calls for each event.

### Bonus Features (Optional)

*Implementing both of these features will make the task count as `Hard`*

1. **Traffic-aware recovery:** the routing agent receives a graph update (`W-C` removed, or weight set to infinity). Only permuting the old stop list does not count.
2. **Tool-call failure recovery:** if a tool call fails or returns invalid JSON, retry or fall back once, log the failure, and still finish the scripted run without crashing the orchestrator.

### Tips

- Get the seed, one agent, and the dashboard list views working before you add events.
- The grade is the loop and the invariants, not a GIS product.
- Intended model is **Gemini free tier** (AI Studio) or equivalent with tool calling. Pin the model name in the README. Put the key in `.env`, not in git.
- Dijkstra on this graph is the required router. OR-Tools is optional inside that tool. The orchestrator still has to choose when to call it.

### Useful Resources

- [Google AI Studio (Gemini free tier)](https://aistudio.google.com/)
- [Gemini function calling](https://ai.google.dev/gemini-api/docs/function-calling)
- [OpenAI function / tool calling](https://platform.openai.com/docs/guides/function-calling)
- [LangGraph](https://langchain-ai.github.io/langgraph/)
- [LangChain tool calling](https://python.langchain.com/docs/how_to/tool_calling/)
- [Google OR-Tools routing](https://developers.google.com/optimization/routing)
