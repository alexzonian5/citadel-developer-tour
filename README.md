# Citadel Developer Tour Pack (GitHub-Safe)

This is a curated, sanitized reading pack for a developer who needs to quickly understand what Citadel is, how it thinks, and how collaborator worlds work.

This pack is intentionally public-safe:
- internal mission names are removed
- collaborator identities are generalized
- local machine paths are scrubbed
- architecture is preserved while sensitive context is reduced

## What Citadel is

Citadel is a governed cognitive operating system.

Its core ideas are:
- mission-bound work
- routing before reasoning
- memory and truth on disk
- explicit authority boundaries
- collaborator worlds as membrane-bound lanes rather than generic chats
- receipts, admission rules, and live-state surfaces instead of hidden agent behavior

## Fastest reading order

### 1. Core control surfaces
Read these first:
- `LAWS.md`
- `STATE.md`
- `LIVE_COMMAND_BOARD.md`

What to notice:
- the system is governed by explicit law
- active work is attached to live missions
- there is a distinction between doctrine, current state, and active command surface

### 2. Routing and execution architecture
Then read:
- `routing/GOVERNOR_ALLOCATOR_SPEC_V1.md`
- `routing/MODEL_ROUTING.md`
- `routing/WORKFLOW_ADMISSION.md`
- `routing/NODE_CONTRACTS.md`

What to notice:
- every task is supposed to be routed, not just handled ad hoc
- routing prefers deterministic -> retrieval -> local -> premium
- workflows need admission, observability, boundedness, and killability before they go live
- the persistent coordination node governs, while heavier compute is labor only

### 3. Collaborator-world architecture
Then read:
- `collaborator-worlds/COLLABORATOR_WORLD_ARCHITECTURE_V1.md`
- `collaborator-worlds/COLLABORATOR_LANE_TEMPLATE_V1.md`
- `collaborator-worlds/COLLABORATOR_LANE_IDENTITY_ANCHOR_SCHEMA_V1.md`
- `collaborator-worlds/COLLABORATOR_ROUTING_AND_SESSION_MEMBRANE_PROTOCOL_V1.md`
- `collaborator-worlds/COLLABORATOR_WORLD_MINIMUM_VIABILITY_STANDARD_V1.md`

What to notice:
- collaborator worlds are not generic user threads
- each lane has identity, boundaries, escalation, and allowed artifact surfaces
- routing, delivery, session visibility, and world quality are treated as different layers
- each collaborator lane is meant to be a designed chamber, not just a transport route

### 4. One genericized world instance
Finally read:
- `examples/generic-collaborator/identity_anchor.md`
- `examples/generic-collaborator/COLLABORATOR_OWNER_PUBLISHER_MEMBRANE_V1.md`
- `examples/generic-collaborator/SHARED_DEMO_CELL_CHARTER_V1.md`

What to notice:
- the lane explicitly remembers who it is for
- outward collaborator artifacts go through a governed publishing membrane
- shared cells are bounded by front, tone, and escalation rules

## The key architectural idea

If you only remember one thing, remember this:

**Citadel is trying to make intelligence legible and governable.**

It does that by combining:
- explicit law
- live state surfaces
- routing contracts
- collaborator membranes
- bounded workflows
- authority separation between sovereign coordination and worker execution

## Important note

This pack is a comprehension pack, not a runtime-complete repo dump.
It is designed to let a serious developer sense the architecture quickly without exposing unrelated private context.

## Suggested questions for a developer
- Which parts are doctrine only, and which are runtime-enforced?
- What packet schema currently sits between routing and execution?
- How are receipts and state mutation enforced in practice?
- How are collaborator-lane permissions implemented today versus specified?
- Which parts of routing are already operational, and which remain design law?
