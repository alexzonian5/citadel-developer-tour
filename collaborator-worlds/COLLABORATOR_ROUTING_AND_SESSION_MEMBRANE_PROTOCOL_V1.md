# Collaborator Routing and Session Membrane Protocol V1

## Status
Active protocol, sanitized public version.

## Purpose
Formalize how Citadel routes into collaborator worlds and interprets session visibility so collaborator lanes become durable operational districts rather than fragile transcript accidents.

## Core principle
A collaborator lane is not “broken” just because one attempted access path failed.

Always distinguish:
1. can messages be delivered,
2. does a stored or live session exist,
3. is that session visible to the current tool path,
4. is the membrane policy-constrained,
5. is the collaborator world itself useful and alive.

## Identity-anchor rule
Before using a collaborator route, first anchor the lane to the actual person.

Minimum invariant:
1. who the person is,
2. that the lane is for that person,
3. that the lane is not generic core context unless explicitly escalated,
4. what collaborator packet, directory, or world-shaping files should be loaded first.

If that anchor is missing, the route is under-specified even if delivery works.

## Required artifact
Each collaborator lane should carry a small identity-anchor file at `meta/identity_anchor.md`.
That file is the minimum fast-load surface for who the lane is for, what it is not, and what files should load first.

## Canonical routing layers

### Layer 1, Provider or account routing
This is the raw messaging path.
Question answered here:
- can a message actually be delivered to the person?

### Layer 2, Session existence
Question answered here:
- is there a stored or active world/history/session for this collaborator?

### Layer 3, Session visibility or tool reachability
Question answered here:
- can the current runtime access that session through the active routing surface?

Failure here does not mean the collaborator lane is unreal.
It usually means visibility policy, membrane constraint, or routing-tool mismatch.

### Layer 4, Collaborator world quality
Question answered here:
- once routed correctly, is the lane actually useful, alive, and compounding?

## Permanent diagnostic rule
When collaborator routing feels broken, diagnose in this order:
1. provider routing,
2. session existence,
3. visibility or tool reachability,
4. collaborator-world quality.

Do not skip ahead and declare the lane dead just because one path failed.

## Canonical routing modes

### Mode A, Canonical direct routing
Use when the correct account and target are known and direct delivery is the most reliable path.

### Mode B, Session routing
Use when a collaborator session is known, live, and visible, and the lane is functioning as a true world rather than only a raw message target.

### Mode C, Policy-constrained routing
Use this concept when a session exists and delivery works, but cross-session access is constrained by visibility or sandbox policy.

Interpretation:
- the lane is real
- the road is gated
- the problem is membrane or policy, not collaborator reality

## Operating rule for all collaborator lanes
On entry, first resolve:
- which human this lane belongs to,
- what the lane is for,
- what files or packets define the lane,
- whether the core operator is relevant only as sovereign or escalation context rather than default active identity.

Do not let collaborator routes collapse back into generic core context.

## Membrane doctrine
A collaborator membrane should do three things well:
1. preserve lane identity,
2. preserve routing durability,
3. preserve adaptive shaping from live behavior.

A good membrane is:
- legible
- durable
- respectful
- strategically useful
