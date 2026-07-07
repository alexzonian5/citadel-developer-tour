# Citadel Operating Laws

> These laws govern the entire ecology. Every organ, every provider, every session.

---

## Primary Laws

**1. No system may expand unless it proves artifact throughput.**
Infrastructure, tools, organs, and protocols justify their existence through artifacts shipped. If it doesn't contribute to a deliverable, it gets frozen.

**2. No provider may own Citadel's memory, commands, or operating grammar.**
Providers implement the grammar. They do not define it. All state lives on disk in provider-independent formats.

**3. Every action must attach to a mission.**
Unattached work is drift. If there's no mission for the work, either create one or don't do the work.

**4. Every mission must attach to an artifact.**
A mission without a target artifact is a project without an exit. Define what ships.

**5. Revenue-closest missions take priority.**
When choosing what to work on, the mission with the shortest path to revenue wins. Infrastructure missions serve revenue missions, not the other way around.

---

## Anti-Chaos Rules

Carried forward from Citadel Delegation Engine V1:

**6. One owner per artifact.** No parallel writes without a merge checkpoint.

**7. No silent execution.** Every completed task produces a receipt. No action without writeback.

**8. No scope creep in delegation.** Bounded tasks only. Expanded scope surfaces as a blocker, not unilateral action.

**9. No state held in transient context.** Important state writes to disk. Context resets. Disk does not.

**10. Citadel arbitrates conflicts.** Alex resolves. Organs and providers do not override each other.

---

## Governor Rules

**11. Kill drift.** If an organ or mission has produced no artifact progress in 7 days, flag it. In 14 days, freeze it.

**12. Refuse infra creep.** New tools, services, and organs require justification against an active mission. "Could be useful" is not justification.

**13. Protect focus.** Maximum 3 active missions at any time. Additional missions queue or freeze.

**14. Enforce ship bias.** When in doubt between perfecting and shipping, ship. Refine after delivery, not before.

---

## Delegation Grammar

Carried forward and refined from Delegation Engine V1:

| Decision type | Who decides |
|---|---|
| Strategic direction, company shape | Alex (never delegates) |
| What to build next | Alex (never delegates) |
| What to delegate and to whom | Alex (never delegates) |
| How to execute a bounded task | Organ / provider (delegated) |
| What artifact format to use | Defined by mission packet |
| When something is done | Receipt confirms, Alex reviews |

---

## The Cockpit Law

The cockpit must answer these five questions at all times:

1. What is being built now?
2. What artifact is closest to shipping?
3. What is blocked?
4. What should happen next?
5. Which organ is being used, and why?

If the cockpit cannot answer these, it is broken. Fix the cockpit before doing more work.
