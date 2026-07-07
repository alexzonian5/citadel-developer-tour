# Packet Schema Example

This page shows the kind of object Citadel tries to converge work into.

The exact runtime schemas in the wider system may vary by lane, but this example captures the architectural intent:

- tasks should be bounded
- tasks should be routable
- tasks should point at artifacts
- tasks should declare allowed tools and models
- tasks should have stop conditions
- tasks should return receipts

## Why packets matter

A packet is the bridge between:
- vague operator intent
- routing logic
- execution lanes
- receipts and state mutation

Without packets, work tends to collapse back into chat improvisation.

With packets, work becomes:
- inspectable
- portable
- replayable
- easier to delegate
- easier to validate

## Minimal example

```yaml
packet_id: PKT-EXAMPLE-0001
issued_at: 2026-07-07T07:00:00Z
issued_by: coordination_node
mission: collaborator_support_front
artifact_target: collaborator_brief_v1
job_type: bounded_synthesis

intent:
  summary: Prepare a collaborator-facing brief for a specific front.
  success_condition: Brief is bounded, clear, and ready for governed outward publication.

input_refs:
  - type: state_surface
    ref: STATE.md
  - type: law_surface
    ref: LAWS.md
  - type: collaborator_lane
    ref: identity_anchor
  - type: project_context
    ref: front-specific-notes

output:
  path: artifacts/collaborator-brief-v1.md
  format: markdown
  visibility: collaborator_shared_candidate

routing:
  preferred_lane: local_or_worker
  escalate_if:
    - ambiguity_is_high
    - sovereign_judgment_required
    - collaborator_boundary_is_unclear

allowed_tools:
  - retrieval
  - local_model
  - bounded_worker

allowed_models:
  - reasoning_local
  - fast_local

forbidden:
  - direct_state_mutation
  - doctrine_rewrite
  - unbounded_premium_recursion

validators:
  - structure_validator
  - collaborator_boundary_validator
  - artifact_target_validator

budget:
  runtime_limit_minutes: 20
  retry_limit: 1
  premium_calls_allowed: 0

stop_conditions:
  - artifact_completed
  - ambiguity_threshold_exceeded
  - validator_failure
  - missing_required_context

return_requirements:
  - artifact
  - run_summary
  - provisional_receipt
  - escalation_note_if_blocked
```

## What each section is doing

### Identity fields
- `packet_id`
- `issued_at`
- `issued_by`

These make the work traceable.

### Mission and artifact fields
- `mission`
- `artifact_target`
- `job_type`

These prevent vague labor with no shipping target.

### Intent block
This says what the work is for and what success means.

### Input refs
These tell the lane what truth surfaces or context it is allowed to use.

### Output block
This says where the result should go and what visibility class it belongs to.

### Routing block
This captures the intended lane and the conditions that force escalation.

### Allowed / forbidden block
This is a practical membrane.
It limits what the worker or lane is allowed to do.

### Validators
These define what has to pass before the output is treated as healthy.

### Budget and stop conditions
These are anti-drift controls.
They keep a worker from becoming an ambient loop.

### Return requirements
These ensure that completion means more than “the model said it’s done.”

## How this fits the architecture

Packets are where several Citadel ideas meet:

### 1. Routing before reasoning
The packet gives the routing layer something explicit to score and bind.

### 2. Membranes before undifferentiated access
Allowed tools, forbidden actions, and visibility classes limit drift.

### 3. Artifacts before vague completion
The packet points at a real output target.

### 4. Receipts before hidden state
The packet expects a traceable return package.

## Important note

This example should not be mistaken for a claim that one single universal schema is already the only runtime object in the wider system.

It is better understood as:
- a representative example
- an architectural compression
- the kind of object Citadel is trying to standardize around

## If you are evaluating the system

The right developer questions here are:
- how close is the real runtime to this packet shape?
- which fields are already enforced mechanically?
- which fields are still enforced by operator discipline?
- what is the smallest live example of a packet going end-to-end?

## Short mental model

A packet is a governed work object.

It turns:
- “please help with this”

into:
- “here is the bounded task, allowed lane, target artifact, validation logic, and return contract.”
