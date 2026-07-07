# Reality Status: What Is Real vs What Is Doctrine

This page exists to answer the most important developer question quickly:

**Which parts of Citadel are already real, and which parts are still architecture law, design intent, or partially implemented substrate?**

## Short answer

Citadel is **not** just a concept repo.
But it is also **not** a fully runtime-enforced operating system yet.

The honest state is:
- some parts are real and operational
- some parts are live but only partially enforced
- some parts are doctrine that exists to guide a cleaner future implementation

## 1. Already real

These ideas are materially real in the broader Citadel system that this pack was extracted from:

### A. Disk-backed law and state surfaces
There are real files used as live control surfaces for:
- law
- current state
- command surface
- receipts
- mission truth

This is not merely a prompt style.
A major design principle is that important truth should survive outside chat context.

### B. Curated routing doctrine exists as actual operating substrate
Routing is not treated as a vague aspiration.
There are real routing artifacts specifying:
- task scoring
- lane selection
- deterministic vs retrieval vs local vs premium preference
- workflow admission rules
- node authority split

Some of this is enforced socially and procedurally today, and some of it is only partially mechanized.
But it is real enough to shape actual operations.

### C. Local-first / premium-containment direction is real
A major operating push has been to reduce uncontrolled premium-model usage and prefer:
- deterministic execution
- retrieval
- local models
- explicit premium gating

This is not decorative philosophy.
It emerged from real cleanup pressure and cost/governance constraints.

### D. Collaborator worlds are real as a design pattern
Citadel has already produced collaborator-lane artifacts with:
- identity anchors
- boundaries
- world-specific membranes
- governed outward publication logic
- bounded shared-cell rules

That means collaborator worlds are not only an imagined future feature.
The design pattern has already been instantiated.

## 2. Partially real / partially enforced

These are the most important in-between areas.

### A. Governor / allocator behavior
The routing layer is conceptually central and has strong doctrine around it.
In practice, some routing behavior has been materially exercised, but the full always-on universal allocator is better understood as:
- partially operational,
- partially procedural,
- partially still being hardened.

So the governor is real as an architectural center, but not every pathway should be assumed to be automatically enforced by one perfect runtime brain.

### B. Workflow admission and killability
The system clearly cares about:
- admission rules
- observability
- receipts
- freeze criteria
- kill switches

Some workflows have been actively classified, frozen, or bounded in real cleanup work.
But a reader should not assume every workflow in the wider system already conforms perfectly.
The doctrine exists partly because real drift had to be corrected.

### C. Node authority separation
The rule that the coordination node governs and heavier compute remains labor is a serious architectural law.
That law is meaningful and grounded.
But a reader should understand it as a governing contract for system growth, not as evidence that the full multi-node production organism is already mature.

### D. Collaborator membranes vs runtime permission systems
The collaborator-world architecture is real and thoughtful.
But some of the membranes are implemented through:
- file structure
- operator discipline
- route design
- explicit lane artifacts

rather than a complete hardened product-grade permission system.

## 3. Mostly doctrine or forward contract

These are best understood as serious design commitments rather than fully completed runtime substrate.

### A. Complete universal routing enforcement
The repo shows what the routing system should do.
It should not be read as proof that every single task across the whole organism is already automatically scored and bound perfectly.

### B. Full collaborator-world productization
The collaborator-world model is strong and already instantiated in examples.
But a reader should not mistake that for a polished commercial multi-tenant product.
It is closer to a governed internal architecture and operating pattern.

### C. Fully mature node ecology
The authority/labor split is clearly specified.
But the repo should not be read as evidence that the heavy-node ecosystem is fully operationalized at scale.
It is a governed direction with real preparatory law.

## 4. Best way to interpret this repo

The correct interpretation is:

- this is **not** empty theory
- this is **not** a finished platform product
- this **is** a serious architecture slice extracted from a real evolving system
- the strongest value here is the explicitness of the contracts, boundaries, and routing logic

In other words:

Citadel is interesting not because it claims magic autonomy, but because it tries to make intelligence work:
- legible
- inspectable
- governable
- membrane-bound
- stateful outside transient chat

## 5. Good developer reading posture

A good developer should ask:
- which of these contracts are already backed by runtime machinery?
- which are operator discipline encoded as files?
- where are the current packet schemas and receipt paths?
- what is the smallest end-to-end proof that demonstrates the architecture in action?

That is the right lens.

## Bottom line

Citadel is best understood as a **real system with real architectural organs, mixed enforcement maturity, and unusually explicit governing doctrine**.

Its most unusual move is not “having agents.”
Its most unusual move is trying to make intelligence infrastructure structurally governable.
