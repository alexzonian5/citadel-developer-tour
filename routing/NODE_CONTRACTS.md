# NODE_CONTRACTS.md

## Purpose
Define the authority boundary between the persistent coordination node and the heavy inference node.

This file is a system contract, not a live command board.

---

## 1. Node roles

### Node A, Persistent Coordination Node
Current substrate: Mac mini

Node A is the **authority node**.

It owns:
- mission state
- truth surfaces
- workflow admission
- routing policy
- scheduling policy
- memory writeback
- receipts
- organ classification
- kill / freeze decisions
- premium-model governance

Node A may:
- create and update task packets
- decide whether work runs at all
- assign work to Node B
- accept or reject returned work
- freeze autonomous systems
- update canonical filesystem state

Node A must remain:
- always-on
- lean
- legible
- observable
- governable

### Node B, Heavy Inference Node
Current planned substrate: Ryzen 9 + RTX 3090 + 128GB RAM

Node B is the **labor node**.

It exists to perform bounded heavy work such as:
- large local inference
- long-running research batches
- evaluator swarms
- embedding jobs
- multimodal processing
- coding bursts when explicitly assigned
- overnight compute-heavy tasks

Node B does **not** own:
- mission state
- system law
- truth surfaces
- workflow admission
- routing policy
- strategic memory
- receipts as final truth

---

## 2. Core authority law

Node A governs.
Node B executes.

If there is ever a conflict between:
- Node A state and Node B state
- Node A routing and Node B local judgment
- Node A receipts and Node B claimed completion

Node A wins.

---

## 3. Allowed operations by node

### Node A allowed
- run core services
- host OpenClaw gateway
- host Citadel MCP
- maintain PostgreSQL and retrieval surfaces
- schedule approved workflows
- store canonical mission files
- issue heavy compute packets
- validate returned work
- write final receipts

### Node B allowed
- receive bounded jobs
- run local models
- generate outputs inside assigned work directories
- emit logs and provisional receipts
- return artifacts, metrics, and errors to Node A
- remain idle when no packet is assigned

### Node B forbidden
- editing `STATE.md`, `COCKPIT.md`, `LAWS.md`, or mission packets directly
- changing routing policy
- changing scheduler policy
- self-starting new recurring loops
- self-promoting into authority or governance
- mutating live command surfaces
- initiating premium API use unless explicitly packet-authorized
- creating new active missions

---

## 4. Work packet contract

All Node B work must arrive as a packet from Node A.

Minimum packet fields:

```yaml
packet_id: unique id
issued_at: ISO-8601 timestamp
issued_by: node_a
mission: active mission name
artifact_target: explicit artifact or output target
job_type: inference | coding | embedding | evaluation | multimodal | batch_research
input_refs:
  - files, queries, URLs, or dataset refs
output_path: bounded write location
allowed_tools:
  - explicit tools or runtimes
allowed_models:
  - explicit local or premium models
budget:
  wall_clock_minutes: n
  max_model_calls: n
  max_tokens_or_compute_class: bounded value
validators:
  - required checks before return
stop_conditions:
  - completion
  - budget exceeded
  - ambiguity threshold exceeded
  - missing dependency
return_requirements:
  - artifact
  - run log
  - provisional receipt
  - error summary if failed
```

No packet, no work.

---

## 5. Return contract

Node B returns work to Node A with:
- produced artifact or patch
- run log
- model/runtime summary
- validator results
- provisional status: `ok | partial | failed | blocked`
- reason for escalation if not `ok`

Node A decides whether the return becomes:
- accepted artifact
- needs review
- retry
- rejected
- frozen follow-up

Only Node A writes final truth and final receipt.

---

## 6. Receipt law

Node B may emit provisional receipts.
Node A emits canonical receipts.

Canonical receipt must include:
- mission
- artifact
- organ / node used
- actions performed
- timestamp
- acceptance or rejection outcome

If Node B claims completion without a return package, completion does not count.

---

## 7. Scheduling law

Node A may schedule recurring work.
Node B may not create recurring schedules for itself.

Any recurring Node B usage must specify:
- mission served
- artifact class served
- schedule
- budget
- stop condition
- freeze criteria
- health check
- owner

If any of these are missing, the schedule should not go live.

---

## 8. Network and trust law

Node B should be treated as a trusted compute worker, not a sovereign peer.

Practical implications:
- expose only the minimum required interfaces
- prefer job pull or explicit dispatch over ambient autonomy
- keep credentials scoped narrowly
- avoid granting Node B direct write authority over canonical state
- do not expose premium credentials by default

---

## 9. Failure handling

If Node B becomes:
- unavailable
- slow
- contradictory
- noisy
- unbounded
- opaque

Node A must be able to:
- stop assigning work
- kill the current run
- quarantine returned artifacts
- preserve logs
- continue operating without Node B

Node A must degrade gracefully without Node B.

Node B is additive labor, not a single point of control.

---

## 10. Admission gate before connecting the 3090

The 3090 node may only enter the system when all are true:
1. premium leakage is contained
2. core scheduled loops are classified
3. `STATE.md` obeys max-three mission law
4. core health checks are reliable
5. this node contract is accepted
6. first packet schema is defined
7. receipt path from Node B back to Node A is defined

If these are not true, Node B remains offline from Citadel authority.

---

## 11. First implementation target

The first live Node B task should be narrow.

Recommended first packet type:
- bounded overnight research batch
  or
- bounded local evaluator swarm on a known artifact

Do not start with open-ended autonomy.

---

## 12. Bottom line

Node A is the governor.
Node B is the worker.

The system gets stronger only if heavier compute enters as disciplined labor under Node A law.
