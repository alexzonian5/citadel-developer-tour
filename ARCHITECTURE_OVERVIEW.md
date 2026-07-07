# Citadel Architecture Overview

This is the shortest architecture sketch of the system shown in this repo.

## Core idea

Citadel is not presented here as a single agent.
It is a governed operating system for intelligence work.

Its main architectural moves are:
- law before freeform autonomy
- routing before reasoning
- disk-backed state before hidden context
- membranes before undifferentiated access
- receipts before vague completion

## Simple map

```mermaid
flowchart TD
    A[Operator Input] --> B[Governor / Allocator]
    B --> C[Retrieval]
    B --> D[Deterministic Workflows]
    B --> E[Local Models]
    B --> F[Premium Cognition]
    B --> G[Coder / Worker Lanes]

    C --> H[Receipts + State Surfaces]
    D --> H
    E --> H
    F --> H
    G --> H

    H --> I[Live Command Surface]
    H --> J[Mission State]
    H --> K[Collaborator Worlds]
```

## Authority split

```mermaid
flowchart LR
    A[Node A: Coordination Node] -->|issues packets| B[Node B: Heavy Inference Node]
    B -->|returns artifacts, logs, provisional receipts| A

    A1[Law] --> A
    A2[State] --> A
    A3[Routing] --> A
    A4[Memory] --> A

    B1[Local inference] --> B
    B2[Batch cognition] --> B
    B3[Evaluator swarms] --> B
```

Node A governs.
Node B executes.

## Collaborator-world map

```mermaid
flowchart TD
    A[Collaborator] --> B[Lane Identity Anchor]
    B --> C[Allowed Artifacts + Boundaries]
    C --> D[Session Membrane]
    D --> E[Bounded Shared Cell]
    E --> F[Governed Publication / Receipts]
```

A collaborator world is meant to be a designed chamber, not a generic thread.

## Read this next
- `README.md`
- `routing/GOVERNOR_ALLOCATOR_SPEC_V1.md`
- `routing/MODEL_ROUTING.md`
- `collaborator-worlds/COLLABORATOR_WORLD_ARCHITECTURE_V1.md`
