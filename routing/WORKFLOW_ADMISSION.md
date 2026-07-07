# WORKFLOW_ADMISSION.md

## Purpose
Define the minimum conditions a workflow must satisfy before it is allowed to run as a live Citadel workflow.

This is the anti-drift gate.

---

## 1. Admission law

No workflow becomes live because it is interesting, technically possible, or emotionally compelling.

A workflow becomes live only when it proves:
- mission attachment
- artifact relevance
- boundedness
- observability
- validator coverage
- killability
- economic justification

If it cannot prove these, it stays draft, sandboxed, manual, or frozen.

---

## 2. Workflow states

Every workflow must be in one of these states:
- `draft`
- `sandbox`
- `manual`
- `candidate-live`
- `live`
- `flagged`
- `frozen`
- `archived`

### Meanings
- **draft**: concept exists, not run
- **sandbox**: test runs allowed, no authority writeback
- **manual**: useful but human-triggered only
- **candidate-live**: passed initial proof, awaiting final admission
- **live**: approved for routine use
- **flagged**: still live but showing drift, failures, or low value
- **frozen**: disabled, preserved for possible later revival
- **archived**: retained as history/support, not expected to return soon

---

## 3. Minimum admission checklist

A workflow may not become `live` unless all of these are defined.

### A. Mission attachment
- which active mission it serves
- why that mission needs this workflow
- what artifact class it moves forward

### B. Trigger clarity
- manual, schedule, event, or queue trigger
- exact schedule if recurring
- no hidden or ambiguous trigger paths

### C. Inputs and outputs
- input schema or expected sources
- output path
- output format
- writeback target

### D. Owner and escalation
- owner
- reviewer if needed
- escalation target when blocked or ambiguous

### E. Validators
- input validator
- structure validator
- consistency or source validator as appropriate
- governance validator

### F. Boundedness
- stop condition
- timeout or runtime budget
- retry policy
- scope boundary

### G. Observability
- health-check method
- log path
- receipt path
- recent-run visibility

### H. Killability
- explicit disable path
- service or schedule label if autonomous
- freeze criteria

### I. Economics
- why this workflow is worth being live
- cost class: deterministic, local, premium
- reason premium is justified if used

---

## 4. Workflow record template

Each candidate workflow should eventually expose something like:

```yaml
name: workflow name
state: draft | sandbox | manual | candidate-live | live | flagged | frozen | archived
mission: mission name
artifact_target: explicit artifact or artifact class
owner: human or organ
trigger:
  type: manual | schedule | queue | event
  detail: exact trigger
inputs:
  - refs or schema
outputs:
  path: explicit path or surface
  format: file / json / webhook / patch / report
validators:
  - input
  - structure
  - source
  - consistency
  - governance
budget:
  runtime_limit_minutes: n
  retry_limit: n
  model_class: deterministic | local | premium
receipt_path: explicit path
health_check: exact probe
kill_switch: exact disable method
freeze_criteria:
  - explicit conditions
```

---

## 5. Live workflow requirements by class

### Manual workflow
Must have:
- mission
- artifact target
- inputs and outputs
- owner
- receipt path

### Scheduled workflow
Must have all manual requirements plus:
- exact schedule
- stop condition
- health check
- kill switch
- freeze criteria
- bounded retry policy

### Queue or event workflow
Must have all manual requirements plus:
- queue source or event source
- dedupe / idempotency handling if relevant
- failure surfacing path
- backlog handling rule

### Premium workflow
Must have all above plus:
- explicit premium justification
- spend boundary
- escalation if premium fails
- no silent recursive retry behavior

---

## 6. Proof requirements before going live

A workflow should generally prove all of the following before `live` status:

1. **one successful end-to-end run**
2. **one useful artifact or meaningful state improvement**
3. **one receipt**
4. **clear logs**
5. **clear disable path**
6. **no unresolved contradiction with current law or mission state**

If these are absent, it should remain `candidate-live` or below.

---

## 7. Freeze rules

A live workflow should be frozen when any of these become true:
- no artifact progress in 7 days
- still no meaningful value by 14 days
- repeated failures without corrective action
- stale assumptions about service state
- hidden premium spend
- unclear owner
- unclear kill switch
- drift from active missions
- duplicate surface exists with no justified distinction

The May 2026 substrate cleanup is a canonical example of why this rule exists.

---

## 8. Re-activation rules

A frozen workflow may return only if it is re-admitted.

Re-activation requires:
- current mission attachment
- current artifact target
- fresh health check
- fresh validator review
- confirmation that the prior failure mode is understood
- updated receipt after first restored run

Frozen is not the same as deleted.
But frozen is also not “quietly still live.”

---

## 9. Forbidden live workflow patterns

Do not admit workflows that are primarily:
- ambient “thinking” with no artifact target
- premium token pumps
- recursive retries without budget
- parallel duplicate surfaces with no contract distinction
- stale writeback loops that mutate truth surfaces without current proof
- prestige infrastructure with no throughput gain

These are anti-patterns, not sophistication.

---

## 10. Current policy implications for Citadel

Given the current cleanup phase:
- new workflows should default to `manual` or `sandbox`
- scheduled workflows should be rare
- premium scheduled workflows should be effectively disallowed unless explicitly approved
- any workflow affecting `STATE.md`, mission packets, or live command surfaces must face a high admission bar

---

## 11. First restoration order after cleanup

When bringing frozen workflows back, prefer this order:
1. workflows that clearly support an active mission artifact
2. workflows with deterministic cores and simple health checks
3. workflows with existing receipts and recent proof
4. local-model workflows with bounded outputs
5. premium workflows only last

---

## 12. Bottom line

A workflow is live only if Citadel can explain:
- why it exists
- what it ships
- what stops it
- how to observe it
- how to kill it

If Citadel cannot answer those, the workflow is not ready for live status.
