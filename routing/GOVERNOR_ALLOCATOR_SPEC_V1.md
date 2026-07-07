# GOVERNOR_ALLOCATOR_SPEC_V1

**Status:** Active — governs task routing across all Citadel organs as of 2026-04-19  
**Rank:** Tier B — Operational doctrine; supersedes CITADEL_LANE_OFFLOAD_DOCTRINE_V1 routing heuristics in practice

---

## 1. Purpose

The Governor-Allocator is the single routing brain that sits between operator intake and lane dispatch. Every task entering Citadel passes through it. It scores the task against five dimensions, applies a decision matrix, binds the task to one canonical lane, emits a scope packet, and waits for a receipt before allowing state mutation.

Without a Governor-Allocator, routing decisions are made implicitly — by whoever handles the task. That produces sovereign cognition wasted on work a worker could do, n8n bypassed because it's easier to ask Claude, and no audit trail linking tasks to their actual cost.

The Governor-Allocator makes routing explicit, cheap by default, and auditable.

---

## 2. Position in System

```
Operator (Alex)
    │ command envelope
    ▼
Governor-Allocator          ← this document governs this layer
    │ scoring + lane binding
    ├──────────────────────────────────────────────────────────
    │ Retrieval Gate (QMD / MEMORY.md)         [near-zero cost]
    │ Deterministic Lane (n8n / cron / scripts) [very low cost]
    │ Worker Loop (cognition loop / workers)    [low cost]
    │ Coder Lane (Claude Code / worktrees)      [medium cost]
    │ Premium Cognition (Perplexity)            [medium cost]
    │ Local Inference (qwen3:14b / qwen2.5:7b) [low cost]
    │ Sovereign (OpenClaw Sonnet/Opus)          [high cost]
    ▼
Receipt surface (task_router.jsonl / packet runtime)
    │
    ▼
Sovereign review (before any state mutation)
```

The existing `citadel_task_router.py` handles **local inference lane dispatch only** — it is a subordinate component. The Governor-Allocator is the layer above it that decides whether local inference is even the right lane.

---

## 3. The Five Routing Dimensions

Each incoming task is scored against five dimensions. Scores are integers. The decision matrix in §4 reads these scores.

### D1 — Sovereignty Score (0–3)

Does this task require constitutional authority, approval power, or shared-state write access?

| Score | Meaning |
|-------|---------|
| 0 | Pure data operation, no judgment required |
| 1 | Judgment needed but bounded; no state mutation; no doctrine effect |
| 2 | State-adjacent: classification with downstream consequence, or output could be promoted to memory |
| 3 | **Force sovereign**: doctrine creation, approval decision, memory writeback, architectural choice, operator-facing synthesis, proof promotion |

Sovereignty 3 is a hard stop. No other score overrides it.

### D2 — Recurrence Score (0–2)

Is this task likely to recur on a predictable schedule or trigger pattern?

| Score | Meaning |
|-------|---------|
| 0 | One-off; no known recurrence |
| 1 | Likely to recur; not yet scripted |
| 2 | Already recurring, or trivially scriptable into a deterministic cell |

High recurrence + low sovereignty = strongest signal to route n8n.

### D3 — Build Score (0–2)

Does this task require creating or editing files, implementing features, or producing executable artifacts?

| Score | Meaning |
|-------|---------|
| 0 | No file changes; output is prose, data, or plan |
| 1 | Minor file edits or stub creation |
| 2 | Significant implementation: scripts, workflows, features, refactors |

Build 2 + Sovereignty < 2 = coder lane.

### D4 — External Data Score (0–2)

Does this task require live web synthesis or multi-source external research?

| Score | Meaning |
|-------|---------|
| 0 | No external data; answer exists in workspace or local models can handle |
| 1 | External data helpful but not required for acceptable quality |
| 2 | Live web synthesis required; task crosses domains or requires regulatory/competitive data not in workspace |

External 2 + Sovereignty < 2 = Perplexity lane.

### D5 — Compute Weight (0–3)

How much reasoning depth does this task require?

| Score | Meaning |
|-------|---------|
| 0 | Near-zero: lookup, format, label, count |
| 1 | Light: summarize, extract, classify, transform |
| 2 | Medium: multi-step analysis, synthesis, structured planning |
| 3 | Heavy: architecture, strategy, novel judgment — premium or sovereign required |

---

## 4. Decision Matrix

Rules are evaluated **top to bottom, first match wins**. All subsequent rules are skipped once a lane is bound.

| Priority | Condition | Lane | Rationale |
|----------|-----------|------|-----------|
| 1 | D1 ≥ 3 | **Sovereign** | Constitutional hard stop — no override |
| 2 | D1 ≥ 2 AND task not covered by standing doctrine | **Sovereign** | State-adjacent decisions need operator-aligned judgment |
| 3 | Retrieval gate hits (answer in QMD / MEMORY.md) | **Retrieval** | Retrieve before regenerate — near-zero cost |
| 4 | D2 ≥ 2 AND D1 ≤ 1 AND D3 ≤ 1 | **n8n (Deterministic)** | Recurrence with no sovereignty or build = automate it |
| 5 | D2 = 1 AND D1 = 0 AND D5 ≤ 1 | **n8n (Deterministic)** | Likely-recurring, simple = compress into deterministic cell |
| 6 | D3 ≥ 2 AND D1 ≤ 1 | **Coder Lane** | Significant implementation; no sovereignty gate |
| 7 | D4 ≥ 2 AND D1 ≤ 1 | **Perplexity** | Live web synthesis needed; not sovereign |
| 8 | D5 ≥ 2 AND multi-step AND D1 ≤ 1 | **Worker Loop** | Multi-node bounded execution; no live operator needed |
| 9 | D5 = 1 AND D1 ≤ 1 | **Local Inference (fast_local)** | Light transform; cheap |
| 10 | D5 = 2 AND D1 ≤ 1 (single-step) | **Local Inference (reasoning_local)** | Medium reasoning; no multi-node |
| 11 | D5 = 3 AND D1 ≤ 1 | **Perplexity** | Heavy reasoning without sovereignty = premium cognition |
| 12 | Default | **Local Inference (fast_local)** | Safest cheap default |

### Retrieval Gate (Rule 3 detail)

Before scoring D1–D5, the governor ALWAYS checks:
1. Is answer already in `MEMORY.md` or project memory files?
2. Can QMD (`citadel_qmd.sh`) return a high-confidence hit?

If yes: return the retrieval result. Do not invoke any model. Log as `lane=retrieval`.

This gate is not optional. Skipping it is architectural debt.

---

## 5. Lane Definitions and Scope Contracts

Each lane has a scope contract. Crossing the contract boundary requires sovereign review.

### Lane A — Retrieval (QMD / MEMORY.md)

**Organs:** `projects/Citadel/scripts/citadel_qmd.sh`, MEMORY.md, project memory files  
**Cost tier:** near-zero  
**Permitted outputs:** Retrieved text verbatim or minimally reformatted  
**Forbidden:** Generating new content, modifying state, synthesizing across multiple sources with judgment  
**Receipt:** `lane=retrieval, source=<file/index>, confidence=<score>`

### Lane B — Deterministic (n8n / scripts / cron)

**Organs:** `citadel-n8n` Docker container (port 5678), shell scripts, cron entries  
**Cost tier:** very-low  
**Entry point:** `POST http://localhost:5678/webhook/citadel-worker-command-surface`  
**Permitted outputs:** Scheduled artifact creation, webhook responses, file routing, health checks, notification dispatch, brief generation from template  
**Forbidden:** LLM calls inside deterministic cells unless output is bounded and verified downstream; doctrine decisions; approval actions  
**Receipt:** `lane=deterministic, workflow_id=<id>, trigger=<schedule|webhook|manual>`

### Lane C — Worker Loop (Citadel cognition loop / workers)

**Organs:** `citadel_cognition_loop.sh`, `intake_triage_worker.py`, `citadel_brief_gen.py`, `citadel_qwen3_research_worker.py`  
**Cost tier:** low  
**When to use:** Multi-step bounded tasks with defined input/output contracts, tasks requiring checkpointing, background processing that doesn't need live operator presence  
**Permitted outputs:** Briefs, research digests, triage outputs, structured analysis artifacts  
**Forbidden:** Doctrine changes, approval decisions, operator-facing synthesis, memory writebacks (these must return to sovereign for review)  
**State schema:** Follows CITADEL_EXECUTION_GRAPH_LAW_V1 — graph_id, run_id, phase, checkpoint  
**Checkpoint path:** `.tmp/citadel_graphs/<graph_id>/<run_id>/checkpoint.json`  
**Receipt:** `lane=worker_loop, graph_id=<id>, run_id=<id>, nodes_completed=<n>`

### Lane D — Coder Lane (Claude Code / worktrees)

**Organs:** Claude Code CLI, `EnterWorktree` / `ExitWorktree` tool, Claude Code subagents  
**Cost tier:** medium  
**When to use:** Script implementation, workflow creation, feature building from spec, refactoring, test writing  
**Input contract:** Task must arrive as a scoped spec packet (see §6). Coder lane does not make architectural decisions — it implements what sovereign has scoped.  
**Permitted outputs:** Code files, scripts, workflow configs, test suites, implementation artifacts  
**Forbidden:** Doctrine changes, lane additions, approval decisions, memory writebacks beyond task-scoped artifacts  
**Receipt:** `lane=coder, files_changed=<list>, tests_passed=<bool>, verification=<method>`

### Lane E — Premium Cognition (Perplexity)

**Organs:** `projects/Citadel/scripts/citadel_perplexity_fast_lane.py`  
**Cost tier:** medium (Perplexity credits = finite)  
**When to use:** Multi-source web research, competitive landscape, regulatory fact-finding, high-ambiguity strategic synthesis, architectural judgment requiring external data  
**Permitted outputs:** Research receipts, synthesis artifacts, competitive notes, external fact digests  
**Forbidden:** Writing doctrine, making approval decisions, holding state, promoting output to canon without sovereign review  
**Receipt:** `lane=perplexity, query=<text>, sources_cited=<n>, output_type=<research_receipt|synthesis>`

**Credits NOT worth spending on:**
- Repeated known answers (check QMD first)
- Local follow-up questions (route to qwen3:14b)
- Template-driven brief assembly (route to n8n)
- Routine ops checks (route to deterministic)

### Lane F — Local Inference (qwen models)

**Organs:** Ollama at `http://127.0.0.1:11434`, `citadel_task_router.py`  
**Cost tier:** very-low to low  

| Sub-lane | Model | Tasks |
|----------|-------|-------|
| fast_local | qwen2.5:7b-instruct | summarize, extract, classify, format, transform, label |
| reasoning_local | qwen3:14b | research synthesis, code gen, multi-step analysis |
| embed | nomic-embed-text | embedding generation, similarity queries |

**Permitted outputs:** Prose transformations, local analysis, structured extractions  
**Forbidden:** Sovereign judgment, doctrine creation, approval decisions  
**Receipt:** `lane=local_inference, model=<name>, elapsed_s=<n>, tokens=<n>`

### Lane G — Sovereign (OpenClaw Sonnet/Opus)

**Organs:** Active Claude Code / OpenClaw session with operator context  
**Cost tier:** high  
**Reserved for:**
- Doctrine creation or revision
- Approval decisions (AUTOMATION_BOUNDARIES.md gate)
- Escalation judgment
- Architectural changes (lane additions, scope expansions)
- Memory writeback (MEMORY.md, project memory)
- Operator-facing synthesis
- Proof promotion (elevating lane output to proved capability)
- Checkpoint/state management decisions

**Sovereign time is capital.** If a task could have been cheaper but wasn't, that's architectural debt.

---

## 6. Scope Packet Structure

Every task dispatched to a non-retrieval lane MUST carry a scope packet. The packet travels with the task and is closed by the receipt.

```json
{
  "packet_id": "pkt_<lane>_<yyyymmdd>_<slug>",
  "lane": "<A|B|C|D|E|F|G>",
  "task_summary": "<one sentence describing the task>",
  "scope_boundary": "<what work is in scope>",
  "allowed_outputs": ["<output type 1>", "<output type 2>"],
  "forbidden_actions": ["<forbidden 1>", "<forbidden 2>"],
  "proof_requirement": "<how success is verified>",
  "receipt_target": "sovereign_review | log_only | state_update",
  "sovereignty_score": 0,
  "recurrence_score": 0,
  "build_score": 0,
  "external_data_score": 0,
  "compute_weight": 0,
  "governor_decision_rule": "<rule number from §4 that fired>",
  "created_at": "<iso8601>",
  "operator_authority": "<advise_only|draft_ask|apply_safe|execute_bounded>"
}
```

Packets for lanes C and D MUST also include:
```json
{
  "graph_id": "<for worker loop>",
  "interrupt_nodes": ["<approval_gate>"],
  "checkpoint_path": ".tmp/citadel_graphs/<graph_id>/<run_id>/checkpoint.json"
}
```

---

## 7. Receipt Protocol

No lane output may mutate shared state without a receipt being evaluated by sovereign (or logged for near-zero-cost operations).

**Receipt structure:**
```json
{
  "receipt_id": "rcpt_<packet_id>_<ts>",
  "packet_id": "<original packet id>",
  "lane": "<lane letter>",
  "status": "done | failed | partial | escalated",
  "actor": "<citadel_task_router | n8n_workflow_id | worker_loop | coder | perplexity>",
  "summary": "<one sentence of what was produced>",
  "payload": {},
  "elapsed_s": 0,
  "state_mutation_attempted": false,
  "escalation_required": false,
  "escalation_reason": null
}
```

Receipt evaluation rules:
- `status=done AND state_mutation_attempted=false`: log only, no sovereign action required
- `status=done AND state_mutation_attempted=true`: sovereign must review before applying
- `status=escalated`: sovereign must respond; packet paused
- `status=failed`: sovereign notified; checkpoint preserved for resume

---

## 8. Routing Cost Model

Track actual lane usage to measure system maturity. A maturing Citadel gets cheaper per task over time.

| Lane | Cost Basis | Maturity Signal |
|------|-----------|-----------------|
| Retrieval | ~$0 | High hit rate = strong memory discipline |
| Deterministic | ~$0 + infra | High recurrence ratio = workflows compressing labor |
| Worker Loop | Ollama only | Long-running tasks off sovereign = leverage gained |
| Coder Lane | Claude Code API | Bounded spec packets = implementation without architectural drag |
| Local Inference | Ollama only | D5≤2 tasks not reaching sovereign = good classification |
| Perplexity | Credits | Narrow use = external research is targeted, not reflexive |
| Sovereign | High | Declining share of total tasks = system maturity |

**Target ratio (mature system):**  
Retrieval 30% | Deterministic 25% | Local Inference 25% | Worker/Coder 10% | Premium/Sovereign 10%

**Current state estimate (2026-04-19):** Sovereign and Local Inference taking ~70%+ of load. Retrieval gate not consistently applied. n8n under-utilized for recurring work.

---

## 9. Anti-Patterns This Governor Prevents

| Anti-pattern | How governor prevents it |
|---|---|
| Asking sovereign to summarize text | D5=1 classification routes to fast_local |
| Running Perplexity for questions already in QMD | Retrieval gate fires before scoring |
| Building the same n8n trigger from scratch each session | D2≥2 routes to deterministic lane |
| Coder lane making architectural decisions | Scope packet forbids doctrine/approval actions |
| Worker loop writing to MEMORY.md directly | Receipt protocol requires sovereign review for state mutation |
| Sovereign session handling multi-step background research | D5≥2 + multi-step routes to worker loop |
| Premium cognition on local follow-up Qs | D4=0 prevents Perplexity binding |
| Tasks never reaching deterministic because scripting feels slower | Recurrence score ≥1 triggers n8n evaluation |

---

## 10. Integration with Existing Components

### `citadel_task_router.py`

This script is the **local inference dispatcher** — it handles D5 ≤ 2 tasks that the governor has already bound to Lane F. The Governor-Allocator is upstream of it.

Concretely:
1. Governor-Allocator scores task, applies matrix, fires rule
2. If rule → Local Inference: call `citadel_task_router.py` with the task string
3. Router's output (classification + model call) becomes the lane receipt
4. If rule → any other lane: task_router.py is not invoked

The task_router's `classify_with_confidence()` logic is a valid fast-path for D5 classification when governor needs a quick cost estimate. Call it in dry-run mode to score compute weight without actually running the model.

### `citadel_task_router.py` classification → Governor D-scores

| Router class | D5 (Compute Weight) |
|---|---|
| embed | 0 |
| fast_local | 1 |
| reasoning_local | 2 |
| sovereign | 3 (ignore — governor handles sovereignty separately via D1) |

### n8n Worker Surfaces

Deterministic lane packets should be delivered via:
- `POST http://localhost:5678/webhook/citadel-worker-command-surface`

Receipt arrives via:
- `POST http://localhost:5678/webhook/citadel-worker-delivery-receipt`

### Worker Loop Entry Points

- `citadel_cognition_loop.sh` — for triage → digest → brief → writeback → drain flows
- `citadel_qwen3_research_worker.py` — for bounded qwen3 research jobs with output contracts
- `intake_triage_worker.py` — for intake normalization before loop entry

### Perplexity

- Wrapper: `projects/Citadel/scripts/citadel_perplexity_fast_lane.py`
- Doctrine: `projects/Citadel/knowledge/perplexity/PERPLEXITY_FAST_LANE_V1.md`
- All Perplexity outputs are research receipts, not doctrine. Must be reviewed by sovereign before promotion.

### QMD (Retrieval Gate)

- Wrapper: `projects/Citadel/scripts/citadel_qmd.sh`
- Index: `citadel` (corpus root = workspace)
- MCP endpoint: `http://localhost:8182/mcp`
- Call before any LLM routing. Log cache hit rate.

---

## 11. Governor-Allocator Invocation Protocol

When receiving a task, the governor proceeds in this exact order:

```
Step 1: NORMALIZE
   - Extract: task text, operator authority level, deadline, stated output format
   - If task is ambiguous, attempt classification anyway and flag confidence=low

Step 2: RETRIEVAL GATE
   - Query QMD for high-confidence hit (threshold: similarity > 0.85)
   - Check MEMORY.md for matching named context
   - If hit: return retrieval result, log lane=retrieval, STOP

Step 3: SCORE
   - Assign D1 (Sovereignty), D2 (Recurrence), D3 (Build), D4 (External), D5 (Compute)
   - Flag any score uncertainty for sovereign audit trail

Step 4: APPLY MATRIX
   - Evaluate rules 1–12 in order
   - Bind to first matching lane
   - Record: rule_fired, lane_bound

Step 5: EMIT SCOPE PACKET
   - Construct packet per §6 schema
   - Include all D-scores, rule_fired, operator_authority

Step 6: DISPATCH
   - Route packet to bound lane organ
   - Log dispatch: ts, packet_id, lane, cost_tier

Step 7: AWAIT RECEIPT
   - Do not mutate state until receipt arrives
   - On timeout (>5 min lane C/D, >2 min lane B/F, >30 min lane E): escalate to sovereign
   - On receipt: evaluate status, decide whether sovereign review required

Step 8: CLOSE OR ESCALATE
   - If status=done + no state mutation: log and close
   - If status=done + state mutation pending: surface to sovereign for review
   - If status=escalated or failed: notify operator; preserve checkpoint
```

---

## 12. Governing Constraints (Inherited from Doctrine Spine)

This spec operates under:
- **CITADEL_CANON_COMPACT.md** — approval law, mode boundary, default constraints
- **AUTOMATION_BOUNDARIES.md** — what may be automatic vs what must return to Alex
- **CITADEL_LANE_OFFLOAD_DOCTRINE_V1.md** — the seven lane classes and offload rules
- **CITADEL_EXECUTION_GRAPH_LAW_V1.md** — state schema, checkpoint, interrupt nodes
- **COST_AWARE_COGNITION_ROUTING_V1.md** — premium cognition as capital, not oxygen

This spec **supersedes** the informal routing heuristics in `CITADEL_LANE_OFFLOAD_DOCTRINE_V1` (§6 Governance Flow) by making the decision matrix explicit and auditable. The lane definitions in that document remain valid and are inherited here without change.

---

## 13. Evolution Path

This spec is v1. It governs by scoring + matrix. Future versions may:
- Replace keyword-based D5 scoring with embedding-based semantic classification (requires embeddings work to be unblocked)
- Add D6 (Latency sensitivity) as a tiebreaker for lanes of equal cost
- Build a lightweight governor script (`citadel_governor.py`) that implements Steps 1–8 programmatically
- Wire governor output into `citadel_packet_runtime.py` for full receipt integration
- Add cost accounting (tokens/credits per lane) to the log surface for maturity tracking

None of these require spec revision to implement — they are additive. The matrix and D-scores remain stable.

---

## 14. Quick Reference Card

```
GOVERNOR DECISION QUICK REF

D1 ≥ 3?           → SOVEREIGN (hard stop)
D1 ≥ 2, novel?    → SOVEREIGN
QMD hit?          → RETRIEVAL (near-zero)
D2 ≥ 2, D1 ≤ 1?  → n8n DETERMINISTIC
D3 ≥ 2, D1 ≤ 1?  → CODER LANE
D4 ≥ 2, D1 ≤ 1?  → PERPLEXITY
D5 ≥ 2, multi?    → WORKER LOOP
D5 = 1?           → fast_local (qwen2.5)
D5 = 2, single?   → reasoning_local (qwen3)
D5 = 3, D1 ≤ 1?  → PERPLEXITY
Default           → fast_local

COST ORDER (cheapest to most expensive):
retrieval → deterministic → local_inference → worker_loop → coder → perplexity → sovereign
```
