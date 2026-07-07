# MODEL_ROUTING.md

## Purpose
Define how Citadel chooses between deterministic logic, retrieval, local models, and premium models.

This is a routing contract, not a philosophical note.

---

## 1. Routing priority

Citadel must route work in this order:

1. **deterministic execution**
2. **retrieval / existing memory**
3. **local model execution**
4. **premium model execution**

Default rule:
Never use a more expensive or less governable layer if a lower layer can do the job adequately.

---

## 2. Definitions

### Deterministic execution
Non-model execution such as:
- scripts
- SQL
- rules
- parsers
- validators
- ranking logic
- graph traversal
- API transforms
- schedulers
- state transitions

### Retrieval
Using existing memory or indexed knowledge before generation:
- mission packets
- `STATE.md`
- `COCKPIT.md`
- `LAWS.md`
- receipts
- project-local files
- QMD or equivalent semantic retrieval

### Local models
Low-cost or zero-marginal-cost models, usually on Ollama or equivalent.
Use for:
- extraction
- classification
- compression
- summarization
- lightweight synthesis
- batch enrichment
- evaluator assistance when quality tolerance permits

### Premium models
Paid frontier cognition.
Use only where economic justification is clear.

---

## 3. Routing laws

### Law 1, deterministic first
If the task can be completed reliably with rules, code, SQL, or direct file edits, do not call a model.

### Law 2, retrieval before generation
Before answering or generating, check whether the needed truth already exists in canonical state or indexed memory.

### Law 3, local before premium
If a local model can produce acceptable output quality for the task class, prefer local.

### Law 4, premium by explicit justification
Premium usage must attach to one of the allowed premium cases below.

### Law 5, no autonomous premium loops without budget
No recurring or background workflow may use premium models unless it has:
- explicit approval
- budget boundary
- stop condition
- receipt path
- recent proof of value

### Law 6, premium silence is safer than premium drift
If routing confidence is low, fail closed or escalate instead of silently defaulting to premium.

---

## 4. Allowed premium cases

Premium models are allowed for:
- strategic synthesis where output quality materially changes business value
- ambiguity resolution where lower layers are insufficient
- difficult coding or architectural reasoning
- executive-grade refinement of high-value artifacts
- collaborator-facing outputs where failure cost is high
- deep comparative judgment across complex evidence

Premium models are not justified for:
- routine extraction
- routine tagging
- routine classification
- cron-style keepalive cognition
- heartbeat chatter
- background summarization with no clear artifact target
- “maybe useful” autonomous thinking

---

## 5. Task-class routing table

| Task class | Default route | Premium allowed? |
|---|---|---|
| health checks | deterministic | no |
| state updates | deterministic + retrieval | no |
| file classification | deterministic or local | rarely |
| extraction from known formats | deterministic or local | no |
| queue triage | deterministic or local | rarely |
| lightweight summaries | local | rarely |
| semantic compression | local | sometimes |
| retrieval-augmented memo | local first | sometimes |
| collaborator briefing | retrieval + local first | yes |
| executive intelligence brief | retrieval + premium if justified | yes |
| difficult code review / architecture | local first if simple, premium if hard | yes |
| long autonomous background loop | deterministic or local only by default | only by explicit approval |

---

## 6. Premium gating requirements

Every premium call path should eventually expose:
- mission served
- artifact target
- caller identity
- reason premium was necessary
- budget class
- logging / receipt path

For recurring premium workflows, all of these are mandatory.

If any are missing, the workflow should not run autonomously.

---

## 7. Autonomous routing rules

### Background jobs
Background jobs should default to:
- deterministic
- retrieval-backed
- local model if needed

### Heartbeats
Heartbeats must not become premium cognition pumps.
They are for:
- brief checks
- lightweight orientation
- useful motion
- escalation only when something real changed

### Failover
Premium failover must not silently recurse.
If a premium path errors from quota, auth, or policy:
- stop
- surface the problem
- do not keep retrying blindly

---

## 8. Current Citadel posture, May 2026

The current routing posture should be treated as:
- premium autonomy clamped
- deterministic and retrieval preferred
- local-target autonomy desired but not yet fully proved in OpenClaw model listing
- premium allowed only when directly invoked for justified high-value work

That means the current operating default is conservative by design.

---

## 9. Retrieval law details

Before generation, Citadel should preferentially load from:
1. live truth surfaces
2. mission packets
3. receipts and recent state files
4. project-local state surfaces
5. semantic retrieval index
6. only then, generation layers

If retrieval already answers the question, stop there.

---

## 10. Model admission standard

A model is not “live” in the routing layer merely because it exists.
A model becomes routing-eligible when it is:
- reachable
- listed cleanly by the runtime
- tested on its intended task class
- observed for output quality
- attached to a receipt or benchmark trail

This matters especially for local fallback paths.

---

## 11. Escalation rules

Escalate from lower layer to higher layer only when one of these is true:
- lower layer lacks required capability
- lower layer fails validation
- ambiguity remains after retrieval
- artifact stakes justify higher cognition cost
- user explicitly requests premium quality or premium tool use

Escalation should be visible, not silent.

---

## 12. Bottom line

Citadel should feel intelligent because it routes well, remembers well, and validates well.
Not because it spends premium tokens by default.
