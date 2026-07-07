# COLLABORATOR_LANE_IDENTITY_ANCHOR_SCHEMA_V1

## Status
Active schema.

## Purpose
Make collaborator-lane identity anchoring concrete, fast to load, and hard to forget.

## Required file
Each collaborator lane should include:
- `meta/identity_anchor.md`

## Minimum contents
### 1. Active human
State the actual person this lane is for.

### 2. Core rule
State explicitly:
- this lane is for that person
- this lane is not generic Alex-core
- Alex is sovereign/escalation context, not the default active identity

### 3. Load first
List the first files that should be loaded before relying on generic context.
At minimum:
- `meta/identity_anchor.md`
- `meta/authority_and_boundary.md`
- `meta/allowed_artifacts_manifest.md`
- the lane's main session behavior file

### 4. Interpretation rule
State that replies in the lane should anchor to the collaborator first rather than defaulting to Alex.

## Standard
The identity anchor should be:
- small
- explicit
- first-pull
- non-poetic
- durable under pressure

## Failure condition
A collaborator lane with only a transport route and no identity anchor is incomplete.

## Final sentence
A route is not enough. The lane must remember who it is for.
