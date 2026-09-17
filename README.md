# ArcLimen

> **A typed decision runtime for crossing from fast computation into deliberate cognition.**

ArcLimen is an experimental systems project exploring a simple question:

**Can frequent agent-runtime judgments be handled cheaply, quickly, and transparently — escalating to generative models only when deeper cognition is actually useful?**

The project sits at the boundary between deterministic computation and probabilistic judgment. It is intended as a low-latency decision substrate around slower AI agents, not as another general-purpose agent framework.

## The idea

Modern agents often use a large language model for work that is much smaller than language generation:

- classify an event;
- decide whether something is relevant;
- detect uncertainty or conflict;
- choose a validator;
- select a model or execution path;
- decide whether to retrieve more context;
- decide whether a human or stronger model should be involved.

ArcLimen explores moving those repeated judgments into a typed fast path.

```text
event / observation
        │
        ▼
deterministic feature extraction
        │
        ▼
probabilistic decision layer
        │
        ├── remain local / deterministic
        ├── retrieve / verify / inspect
        ├── route to a cheap model
        └── escalate to deeper cognition
```

The generative model remains available for open-ended reasoning, synthesis, planning, and interpretation. It simply does not need to be the event loop.

## Why “ArcLimen”?

**Limen** is Latin for *threshold* and, in psychophysics, refers to a boundary at which a stimulus becomes perceptible or consequential.

**Arc** refers to the bounded path from stimulus through judgment to response — analogous to a reflex arc without implying that the system itself is a brain.

In ArcLimen, a **limen** is a decision boundary: the point at which available evidence justifies crossing into another mode of processing.

```text
signal ──► features ──► judgment ──► limen ──► route / escalate
```

## Intended architecture

ArcLimen is currently a research direction rather than a frozen architecture. The working shape is:

```text
┌─────────────────────────────────────────────────────┐
│                    ArcLimen                         │
│                                                     │
│  typed events                                       │
│      │                                              │
│      ▼                                              │
│  feature computation                               │
│  CPU reference ───── accelerated backend            │
│      │                                              │
│      ▼                                              │
│  typed decision questions                          │
│      │                                              │
│      ▼                                              │
│  probabilistic decision provider                   │
│      │                                              │
│      ▼                                              │
│  calibrated limen / routing policy                 │
│      │                                              │
│      ├── deterministic path                         │
│      ├── retrieval / verification                   │
│      ├── lightweight model                          │
│      └── deliberate model / human escalation        │
│                                                     │
│  every result recorded for replay and evaluation    │
└─────────────────────────────────────────────────────┘
```

Possible implementation ingredients include:

- **Rust** for the typed runtime and reference execution path;
- **CPU-first deterministic feature extraction** as the semantic baseline;
- **CUDA Rust** or other accelerator backends where batching or heavy transforms justify them;
- fast structured decision models such as **Jev**, behind a replaceable provider interface;
- deterministic and structured-output LLM baselines for comparison;
- **Nix** for reproducible runtime and experiment identity;
- recorded traces and replay for calibration, regression testing, and differential evaluation.

None of these providers or backends should define ArcLimen's semantics by themselves.

## Working vocabulary

| Term | Meaning |
| --- | --- |
| **Event** | A typed observation presented to the runtime. |
| **Feature** | Deterministically derived state used by decision providers. |
| **Question** | A versioned typed judgment the runtime wants evaluated. |
| **Decision** | A structured probabilistic result plus provider/runtime metadata. |
| **Limen** | A policy-defined threshold or boundary between processing modes. |
| **Arc** | One bounded traversal from event to disposition. |
| **Escalation** | A proposal to invoke a more expensive or deliberative path. |
| **Outcome** | Later evidence about what actually happened, used for evaluation and calibration. |

A future naming scheme might include concepts such as `ArcTrace`, `LimenProfile`, and `DecisionSet`, but the vocabulary is intentionally not frozen yet.

## Design principles

### Fast path, slow path

Cheap computation should handle cheap questions. Expensive generative cognition should be reserved for tasks that benefit from it.

### Typed uncertainty

Probabilities, confidence, abstention, routing reasons, and provider identity should be explicit data — not hidden in prose.

### Calibration over confidence theater

A model reporting `0.82` is not evidence that it is correct 82% of the time. ArcLimen should measure calibration against observed outcomes and maintain it per decision class.

### CPU semantics first

Accelerators should optimize a defined computation, not silently become the definition of that computation. Where practical, accelerated feature backends should be checked against a reference implementation.

### Replaceable intelligence

Jev, an LLM, a classical model, a rules engine, or a future local model should be interchangeable behind explicit decision interfaces.

### Replayable decisions

Given the same event corpus, runtime identity, schema, policy, and deterministic feature path, experiments should be reproducible enough to compare providers and policies meaningfully.

### Proposals are not authority

ArcLimen may recommend routes, escalation, verification, or intervention. It should not silently acquire authority to perform consequential actions.

## Relationship to the wider stack

ArcLimen is intended to remain a distinct primitive rather than collapsing into an agent harness, memory system, or authority layer.

```text
Endophasia    exposes and steers agent computation
Pallium       reasons and coordinates
ArcLimen      makes fast typed runtime judgments
Magpie        records governed epistemic history
Deadbolt      governs consequential authority
```

These boundaries are conceptual and subject to revision, but the separation matters:

- ArcLimen should not become canonical epistemic memory;
- ArcLimen should not become execution authority;
- ArcLimen should not become a general-purpose agent orchestrator;
- confidence from a decision model should not automatically become trusted evidence;
- a route proposal should remain distinguishable from permission to act.

## First useful experiment

The initial target is deliberately smaller than a “cognitive operating system.”

Feed ArcLimen recorded agent-runtime events such as:

```text
tool.finished
test.failed
git.changed
benchmark.recorded
peer.challenge.completed
context.compacted
```

Ask a small set of typed questions, for example:

```text
ShouldRetrieve?
ShouldVerify?
ShouldInterject?
NeedsHumanReview?
EvidenceConflict?
CheapModelSufficient?
ShouldEscalate?
```

Then compare several decision paths under the same trace corpus:

1. deterministic rules;
2. structured-output LLM classification;
3. a fast structured decision model;
4. the same decision model with deterministic derived features;
5. accelerated feature computation only where it demonstrates measurable value.

Useful measurements include calibration, latency, cost, selective accuracy, abstention, escalation rate, and downstream task quality.

The first version does **not** need to take actions.

```text
observe → compute → decide → record
```

That is enough to test the central hypothesis.

## Status

**Very early / exploratory.**

This repository currently exists to preserve the concept, terminology, experimental direction, and architectural boundaries before implementation begins. Nothing here should be treated as frozen doctrine or as evidence that a particular model, accelerator, or policy approach has already been validated.

---

**ArcLimen** — fast judgment at the threshold of deliberate cognition.
