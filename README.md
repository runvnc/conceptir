# ConceptIR: Compact Concept Patches for Foundation Models

**Tagline:** Source code for new abstractions.

## Summary

Foundation models can be adapted through prompting, retrieval, parameter-efficient fine-tuning, activation steering, and model editing. But none of these gives us a clean way to define, share, compose, test, and eventually internalize *new abstractions*.

A human can learn a phrase like **technical debt** and, over time, build a reusable mental model around it: short-term engineering shortcuts, future maintenance burden, compounding cost, strategic versus reckless borrowing, repayment through refactoring, and so on.

The phrase itself is tiny. The useful abstraction is large.

This memo proposes **ConceptIR**: an intermediate representation for compact, composable “concept patches” that could let AI systems define and reuse new abstractions more efficiently than prompts, RAG documents, opaque LoRA weights, or full fine-tuning.

The core idea:

```text
ConceptIR =
  typed symbolic graph
  + latent feature bindings
  + examples/counterexamples
  + activation conditions
  + prediction rules
  + intervention policy
  + composition constraints
  + natural language export
  + optional adapter/LoRA distillation
```

A ConceptIR object would not merely describe a concept. It would install a reusable **cognitive operator**: something the model can use to notice patterns, compress situations, make predictions, suggest interventions, and explain the abstraction to humans.

---

## Motivation

Current foundation-model adaptation methods each solve part of the problem.

### Prompting

Prompting is cheap and flexible, but temporary. It can describe a concept, but the model may not reliably internalize it as a reusable reasoning primitive.

### RAG and memory

Retrieval can provide documents, examples, and definitions, but retrieved text usually remains external. It is not the same as installing a new abstraction into the model’s active ontology.

### LoRA and PEFT

LoRA freezes the base model and injects trainable low-rank adaptation matrices, dramatically reducing trainable parameters compared with full fine-tuning. This makes it possible to steer model behavior with relatively small parameter updates.

But LoRAs are usually opaque and entangled. A LoRA may encode a style, task, skill, or domain, but it is not usually a clean semantic concept. It is more like patching a compiled binary than editing source code.

### Activation steering and representation editing

Activation steering and representation fine-tuning operate closer to the model’s live internal state. They suggest that useful behavioral changes can be achieved by intervening on hidden representations rather than only changing weights.

This feels closer to a “concept lens,” but current methods are still mostly vector-level interventions. They do not yet provide a structured format for defining and composing new abstractions.

### Sparse autoencoders and interpretable features

Sparse autoencoders suggest that model activations can be decomposed into more interpretable features. In principle, this could reveal a latent alphabet of concepts: delayed feedback, proxy gaming, stale representations, social threat, authority mismatch, and so on.

If we can discover and address these features, then a new abstraction could be represented as a compact graph over existing latent primitives rather than as a large blob of weight changes.

---

## The central analogy

Current LoRA:

> A behavioral tattoo on the model.

ConceptIR:

> A new mental tool installed into a cognitive operating system.

Or, more technically:

Current LoRA is like patching a compiled binary.  
ConceptIR is like adding a new library function with types, tests, examples, and runtime hooks.

---

## Example: Map-Lag Failure

A proposed abstraction:

> **Map-lag failure**: a failure mode where a system is being controlled using a representation that updates more slowly than the system itself changes.

This abstraction applies to many domains:

- A company optimizing old metrics after the market has shifted.
- A legal system regulating a technology using obsolete categories.
- A person acting from an outdated self-image.
- A codebase whose architecture reflects an old product concept.
- An AI system relying on stale world knowledge.
- A bureaucracy using reports that no longer match operational reality.

The phrase “map-lag failure” is small. But the useful concept has structure.

```yaml
id: map_lag_failure
kind: concept_patch
type_signature: ControlLoop[Controller, Representation, WorldState] -> FailureMode

summary: >
  A system is being controlled using a representation that updates
  more slowly than the system being controlled.

primitive_refs:
  - control_loop
  - stale_representation
  - delayed_feedback
  - environment_volatility
  - proxy_optimization
  - ontology_mismatch

activation_conditions:
  - decision makers rely on old categories, metrics, maps, or narratives
  - the environment has changed faster than the representation
  - feedback is delayed, filtered, or distorted
  - interventions optimize the old representation rather than current reality

counterexamples:
  - the representation is current but incentives are bad
  - the system is mostly random rather than misrepresented
  - the controller has no meaningful control channel
  - the failure is caused by resource scarcity rather than map lag

predictions:
  - symptom-level fixes will decay quickly
  - dashboards may fail unless the underlying ontology is updated
  - frontline actors may appear irrational to leadership, and vice versa
  - adding process can increase lag if it preserves the old categories

interventions:
  - update the ontology before optimizing
  - shorten feedback loops
  - move decision rights closer to live information
  - delete metrics that encode the old world
  - create new measurements aligned with current system state

human_export:
  short_label: map-lag failure
  one_sentence: >
    The system is being steered with an old map of a changed territory.
```

This is not just a definition. It is an operator.

Given a situation, the model can ask:

- Does this pattern apply?
- What evidence would confirm it?
- What evidence would disconfirm it?
- What does it predict?
- What interventions does it suggest?
- How should it be explained to a human?

---

## What a ConceptIR object should contain

A useful concept patch should have several parts.

### 1. Name and human handle

A compact label that humans and models can refer to.

Examples:

- technical debt
- map-lag failure
- coordination debt
- proxy corrosion
- ontology fracture
- semantic bottleneck
- authority/information inversion

The name is not the concept. The name is the handle.

### 2. Type signature

A rough description of where the abstraction applies.

```text
ControlLoop[Controller, Representation, WorldState] -> FailureMode
MultiAgentSystem[Agents, Incentives, SharedModel] -> CoordinationCost
Codebase[Architecture, ProductOntology, ChangeRate] -> MaintenanceRisk
```

This matters because concepts should compose safely. A concept about incentive systems should not be blindly applied to every emotional, technical, or physical system.

### 3. Primitive references

Pointers to existing latent or symbolic primitives.

For example:

```text
map_lag_failure =
  control_loop
  + stale_representation
  + delayed_feedback
  + environment_volatility
  + intervention_decay
```

The goal is semantic compression. The concept patch can be small because the base model already knows the primitives.

### 4. Boundary conditions

Where the abstraction applies and where it does not.

This is essential. Without boundary conditions, a new abstraction becomes a buzzword.

### 5. Examples and counterexamples

Examples ground the concept. Counterexamples prevent overuse.

A mature ConceptIR should include both.

### 6. Prediction rules

A real abstraction should predict something.

If the abstraction does not help forecast what happens next, it may be vocabulary rather than cognition.

### 7. Intervention policy

The concept should suggest useful actions.

An abstraction becomes powerful when it provides handles on reality.

### 8. Latent feature bindings

A ConceptIR object should optionally bind to model-internal features, such as:

- sparse-autoencoder feature IDs
- activation steering vectors
- representation intervention points
- router/expert preferences
- retrieval memories
- tool calls or simulation hooks

This is where the abstraction becomes operational inside a model.

### 9. Composition constraints

Concepts should declare how they combine with other concepts.

Example:

```text
coordination_debt + map_lag_failure -> organizational_slowdown
proxy_corrosion + authority_information_inversion -> management_blindness
ontology_fracture + semantic_bottleneck -> product_confusion
```

Composition is only useful if it avoids destructive interference.

### 10. Test suite

A concept should be tested.

Possible tests:

- classify examples versus counterexamples
- generate predictions for historical cases
- propose interventions and compare outcomes
- explain the concept at multiple levels
- detect misuse of the concept
- compose with adjacent concepts without contradiction

---

## Why this could be much smaller than a LoRA

A LoRA may be tens or hundreds of megabytes because it is an opaque low-rank update to a large, entangled parameter space.

But a concept patch could be tiny if it mostly references existing primitives.

Human language already works this way. The phrase **technical debt** is small because the listener already understands debt, code, maintenance, shortcuts, time, and compounding cost.

Similarly, a 10 KB ConceptIR object could be enough to define a new abstraction if the base model already has the necessary substrate.

The concept patch does not contain all the knowledge. It binds existing knowledge into a reusable operator.

---

## Relation to existing work

### LoRA and PEFT

LoRA shows that low-rank parameter updates can adapt large models efficiently while keeping the base model frozen. It is a practical proof that small patches can redirect model behavior.

But LoRA patches are usually opaque, entangled, and difficult to compose semantically.

Relevant work:

- Hu et al., “LoRA: Low-Rank Adaptation of Large Language Models”
- LoRA surveys and PEFT literature
- AdapterFusion
- LoraHub
- TIES-Merging and other model/adapter merging methods

### Text-to-LoRA and hypernetworks

Text-to-LoRA generates LoRA adapters from natural-language task descriptions. This is especially relevant because it suggests a compiler-like path from description to model adaptation.

A future ConceptIR compiler might generate:

- LoRA weights
- ReFT interventions
- activation steering vectors
- retrieval structures
- routing policies
- test cases

Text-to-LoRA is close to “generate a patch from a description,” but the generated patch is still mostly opaque.

### ReFT / LoReFT

Representation Finetuning proposes learning interventions on hidden representations of a frozen model. This is highly relevant because a concept patch may not need to alter weights. It may act as a runtime intervention on the model’s internal reasoning state.

ConceptIR could use representation interventions as one execution backend.

### Activation steering

Activation steering methods suggest that model behavior can be changed by adding or modifying directions in activation space.

A ConceptIR object could include steering policies such as:

```text
When analyzing organizations:
  activate stale-representation and delayed-feedback features
  suppress simple-incompetence explanations unless evidence supports them
  search for ontology-update interventions
```

### Sparse autoencoders and monosemantic features

Sparse autoencoders may provide the latent alphabet needed for compact concept patches. If interpretable features can be discovered and controlled, then new abstractions can be represented as structured compositions of those features.

This is one of the most important research paths for ConceptIR.

### Concept bottleneck models

Concept bottleneck models force models to reason through explicit intermediate concepts. They are usually task-specific, but the architectural instinct is relevant: make concepts visible, editable, and testable.

ConceptIR would generalize this idea from fixed classifier concepts to open-ended abstractions.

### Vector symbolic architectures / hyperdimensional computing

Vector symbolic architectures combine distributed representations with symbolic composition operations such as binding and superposition. These ideas may be useful for representing structured concepts in a neural-compatible format.

ConceptIR could borrow from this tradition: typed roles, fillers, binding operations, cleanup memory, and compositional vector structures.

### Model editing

Methods like ROME and MEMIT suggest that some factual associations can be edited directly in model weights. However, facts are not the same as abstractions.

ConceptIR is more about installing reusable cognitive operators than editing isolated factual claims.

### Agent memory and graph memory

External memory systems, especially graph-based agent memories, may be the easiest near-term implementation path.

A first version of ConceptIR does not need to be deeply embedded in the model’s weights. It could live as an external structured memory object that an agent retrieves, applies, tests, and gradually distills.

---

## A likely architecture

A practical ConceptIR system could have five layers.

### Layer 1: Discover latent primitives

Use sparse autoencoders, probing, clustering, and representation analysis to identify reusable latent features.

Examples:

```text
delayed_feedback
stale_representation
proxy_metric_gaming
authority_information_mismatch
social_status_threat
intervention_decay
```

### Layer 2: Define concepts as typed graphs

Represent new abstractions as graphs over primitives.

```text
map_lag_failure =
  control_loop
  + stale_representation
  + delayed_feedback
  + environment_volatility
  + failed_intervention_decay
```

### Layer 3: Bind concept nodes to neural controls

Each node can bind to:

- sparse-autoencoder features
- activation steering vectors
- ReFT intervention points
- retrieval examples
- negative examples
- simulation hooks
- tool-use policies

### Layer 4: Apply as a cognitive lens

At runtime, the model can use the concept to:

- detect a pattern
- compress a situation
- generate predictions
- suggest interventions
- check counterexamples
- compose with adjacent concepts
- explain itself to humans

### Layer 5: Consolidate if useful

If a concept proves useful across many tasks, it can be distilled into:

- a LoRA
- an adapter
- a router expert
- a memory policy
- a fine-tuned model update
- a specialized reasoning tool

This mirrors human learning: first a concept is awkward and explicit, then it becomes fluent and internalized.

---

## Prototype roadmap

### Phase 1: Pure symbolic ConceptIR

Create a YAML/JSON schema for concepts.

Build examples:

- technical debt
- map-lag failure
- proxy corrosion
- coordination debt
- ontology fracture

Use an LLM agent to retrieve and apply these objects during reasoning.

Success metric:

- Does the model use the abstraction more consistently than with a plain prose definition?

### Phase 2: Concept tests

Add test suites:

- examples versus counterexamples
- prediction tasks
- misuse detection
- intervention selection
- multi-resolution explanation

Success metric:

- Does the concept improve reasoning without overfitting or becoming a buzzword?

### Phase 3: Feature binding

Bind concept nodes to discovered SAE features or activation steering directions.

Success metric:

- Can the concept be activated more reliably and compactly than with prompt text alone?

### Phase 4: Runtime intervention

Use ReFT-like or activation-steering methods to apply ConceptIR objects during inference.

Success metric:

- Can the model reason through the concept without stuffing the full definition into context?

### Phase 5: Distillation

Distill repeatedly useful concepts into adapters or LoRAs.

Success metric:

- Can a concept move from external memory to efficient internal behavior?

---

## Open questions

1. Can latent features be made stable enough across models, versions, and contexts to serve as reusable concept references?

2. How should concepts be typed so they compose safely?

3. How do we prevent concept overuse, where a useful abstraction becomes a universal explanation?

4. Can concepts be compressed into very small patches without losing boundary conditions and counterexamples?

5. What is the right execution backend: prompt, retrieval, steering, ReFT, LoRA, router expert, or some hybrid?

6. Can a model invent a new ConceptIR object, test it, refine it, and decide whether to keep it?

7. Can two models share a ConceptIR object and use it similarly?

8. What does it mean for a model to “understand” a new abstraction rather than merely repeat its definition?

---

## Why this matters

The next jump in AI cognition may not come only from larger models. It may come from better ways to create, share, compose, and internalize abstractions.

Humans are powerful partly because we invent reusable concepts:

- technical debt
- opportunity cost
- coordination problem
- regulatory capture
- OODA loop
- attention economy
- product-market fit
- self-fulfilling prophecy

These concepts are compact mental tools. They let us see patterns that were previously invisible.

Foundation models need something similar, but native to their architecture.

A mature ConceptIR would let models build new abstractions on the fly, test them, communicate them compactly, and eventually distill them into more fluent internal cognition.

That would be a step toward AI systems that do not merely retrieve and remix human concepts, but invent and share new ones.

---

## References and starting points

- Hu et al., **LoRA: Low-Rank Adaptation of Large Language Models**  
  https://arxiv.org/abs/2106.09685

- Charakorn et al., **Text-to-LoRA: Instant Transformer Adaptation**  
  https://arxiv.org/abs/2506.06105  
  https://sakana.ai/text-to-lora/

- Wu et al., **ReFT: Representation Finetuning for Language Models**  
  https://arxiv.org/abs/2404.03592

- Anthropic, **Decomposing Language Models With Dictionary Learning**  
  https://transformer-circuits.pub/2023/monosemantic-features

- Anthropic, **Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet**  
  https://transformer-circuits.pub/2024/scaling-monosemanticity/

- Geiger et al. and related work on representation interventions and causal abstraction  
  https://arxiv.org/abs/2404.03592

- Koh et al., **Concept Bottleneck Models**  
  https://arxiv.org/abs/2007.04612

- Meng et al., **Locating and Editing Factual Associations in GPT** / ROME  
  https://arxiv.org/abs/2202.05262

- Yadav et al., **TIES-Merging: Resolving Interference When Merging Models**  
  https://arxiv.org/abs/2306.01708

- Chronopoulou et al., **AdapterSoup / Adapter and LoRA composition literature**  
  https://arxiv.org/abs/2302.07027

- Su et al., **LoraHub: Efficient Cross-Task Generalization via Dynamic LoRA Composition**  
  https://arxiv.org/abs/2307.13269

- Gayler, Plate, Kanerva, and later surveys on **Vector Symbolic Architectures / Hyperdimensional Computing**  
  https://redwood.berkeley.edu/wp-content/uploads/2022/11/2022_CSUR_survey_HDCVSA_part_1.pdf

---

## Minimal repo structure

```text
concept-ir/
  README.md
  schema/
    concept_ir.schema.json
  examples/
    map_lag_failure.concept.yaml
    technical_debt.concept.yaml
    proxy_corrosion.concept.yaml
  tests/
    map_lag_failure.eval.yaml
  experiments/
    prompt_only/
    rag_memory/
    activation_steering/
    sae_binding/
    lora_distill/
```

---

## One-line pitch

**ConceptIR is a proposal for compact, composable concept patches: a way for foundation models to define, share, test, apply, and eventually internalize new abstractions.**
