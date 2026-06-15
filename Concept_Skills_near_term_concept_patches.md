# Concept Skills: A Near-Term Path Toward Shared Machine Abstractions

**Tagline:** `SKILL.md` for reusable concepts, not just reusable tasks.

## Summary

Agent skills are emerging as a practical way to give AI agents reusable capabilities: instructions, workflows, examples, scripts, resources, and constraints that can be loaded when relevant.

This memo proposes a nearby extension:

> **Concept Skills** are small, testable, on-demand packets that teach an agent a reusable abstraction or cognitive lens.

A normal skill says:

> Here is how to do a task.

A Concept Skill says:

> Here is a way to recognize, model, predict, and act on a recurring pattern in the world.

This is a more buildable near-term version of a deeper idea: compact concept patches for foundation models. Instead of waiting for neural-native ConceptIR, sparse-autoencoder feature bindings, or tiny composable LoRA stacks, we can start with a disciplined `SKILL.md`-like format that lets agents retrieve and use new abstractions through a fast memory layer.

The core claim:

> A useful abstraction can be packaged as a skill-like object with triggers, examples, counterexamples, prediction rules, intervention policies, and evals. Agents can load these concept packets on demand, use them as reasoning operators, refine them over time, share them over a network, and eventually distill the most valuable ones into more efficient internal forms.

---

## Motivation

Humans do not only learn facts and procedures. We also learn **abstractions**.

Examples:

- technical debt
- opportunity cost
- regulatory capture
- product-market fit
- OODA loop
- coordination problem
- Goodhart’s law
- learned helplessness
- attention economy
- self-fulfilling prophecy

These concepts are not merely definitions. They are reusable cognitive tools. Once you understand one, it changes what you notice.

For example, **technical debt** lets someone see messy engineering decisions through a financial metaphor:

- shortcuts create future cost
- some debt is strategic
- interest compounds
- refactoring is repayment
- too much debt slows future work

The phrase is small. The model it activates is large.

AI agents need something similar: a way to acquire and share compact cognitive tools, not just documents, prompts, or task instructions.

---

## Concept Skills versus ordinary agent skills

### Ordinary agent skill

An ordinary skill is usually procedural.

It answers:

- What task is this for?
- When should the agent use it?
- What steps should the agent follow?
- What tools or scripts are available?
- What examples show correct usage?

Example:

```text
Skill: Create a spreadsheet
Use when the user asks for a spreadsheet artifact.
Instructions:
  - Use openpyxl.
  - Apply formatting.
  - Save as .xlsx.
  - Return a sandbox link.
```

### Concept Skill

A Concept Skill is interpretive and predictive.

It answers:

- What pattern does this abstraction detect?
- What does it compress?
- What does it predict?
- What interventions does it suggest?
- When should it *not* be used?
- What are examples and counterexamples?
- How does it compose with other concepts?

Example:

```text
Concept Skill: Map-Lag Failure
Use when a system is being controlled with an outdated representation of reality.
Detect:
  - stale metrics
  - obsolete categories
  - delayed feedback
  - leadership/frontline disconnect
Predict:
  - symptom fixes decay
  - adding process may worsen lag
  - actors close to reality seem irrational to leadership
Intervene:
  - update ontology first
  - shorten feedback loops
  - move decision rights closer to live information
```

The agent is not just following a workflow. It is applying a lens.

---

## Why this matters

The current agent stack has several memory/adaptation mechanisms:

```text
prompting
RAG
tool use
agent skills
long-term memory
LoRA/adapters
fine-tuning
activation steering
```

But there is a missing practical layer:

```text
shared reusable abstractions
```

RAG can retrieve a document about a concept, but the agent may treat it as text rather than as a cognitive operator.

A LoRA can change model behavior, but it is opaque and often entangled.

A prompt can define a concept, but it is temporary and context-heavy.

A Concept Skill would be:

- compact
- human-readable
- testable
- composable
- versioned
- shareable
- loaded on demand
- usable by many agents
- eventually distillable into stronger internal forms

This is the practical bridge between today’s skill files and tomorrow’s neural concept patches.

---

## The core design

A Concept Skill should be a structured packet.

Minimum useful form:

```yaml
name: map-lag-failure
kind: concept-skill
version: 0.1.0

summary: >
  A failure mode where a system is being controlled using a representation
  that updates more slowly than the system itself changes.

use_when:
  - diagnosing organizations
  - analyzing software architecture drift
  - evaluating institutions or regulations
  - understanding stale metrics
  - analyzing AI memory or world-model staleness

do_not_use_when:
  - the main problem is bad incentives rather than stale representation
  - the system is random/noisy rather than misrepresented
  - the controller has no meaningful control channel
  - the representation is current but resources are insufficient

triggers:
  positive:
    - stale metrics
    - old categories
    - delayed feedback
    - dashboards disagree with frontline reality
    - architecture reflects an obsolete product
    - people optimize a proxy from a previous phase
  negative:
    - fraud
    - random failure
    - pure incompetence
    - insufficient resources
    - no decision authority

core_model:
  controller: the actor or system making decisions
  representation: the map, metric, model, category, dashboard, or story being used
  territory: the actual changing system
  lag: how far the representation trails reality
  intervention: action chosen using the stale representation

reasoning_steps:
  - identify the controller
  - identify the representation being used
  - identify how the territory has changed
  - estimate the lag between representation and reality
  - check whether interventions are optimizing the stale representation
  - predict where symptom-level fixes will decay
  - propose map-update interventions before optimization

predictions:
  - fixes aimed at symptoms will decay quickly
  - better dashboards will fail if they preserve the wrong ontology
  - actors closest to reality may appear irrational to leadership
  - adding process can worsen the problem if it encodes the old map

interventions:
  - update the ontology before optimizing
  - shorten feedback loops
  - move decisions closer to live information
  - delete metrics that encode the old world
  - create new categories that match current reality

examples:
  - company optimizing activation metrics from an obsolete product phase
  - codebase architecture reflecting a product that no longer exists
  - regulation based on old technology categories
  - person making life decisions from an outdated self-image

counterexamples:
  - workers know reality but incentives reward lying
  - system state is correctly represented but resources are inadequate
  - chaotic environment where no stable map is possible
  - leadership has accurate data but refuses to act

composition:
  combines_with:
    coordination_debt: organizational slowdown caused by misaligned teams
    proxy_corrosion: metrics becoming targets and losing meaning
    ontology_fracture: different groups using incompatible categories
  conflicts_with:
    pure_resource_constraint
    random_failure

evals:
  - classify_examples
  - classify_counterexamples
  - generate_predictions
  - suggest_interventions
  - detect_overuse
```

---

## Important distinction: skill, concept, and concept skill

A **skill** is mostly procedural.

> How do I do this?

A **concept** is mostly representational.

> What kind of thing am I looking at?

A **Concept Skill** is operationalized representation.

> When I see this pattern, how should I model it, what should I predict, and what should I do?

This is why Concept Skills are more useful than glossary entries. A glossary defines terms. A Concept Skill changes behavior.

---

## How agents would use Concept Skills

A runtime could use Concept Skills in stages.

### 1. Index only the metadata

The system keeps a lightweight index of all available concept skills:

```yaml
name: map-lag-failure
summary: stale map controlling changed territory
triggers:
  - stale metrics
  - delayed feedback
  - obsolete categories
domains:
  - organizations
  - software
  - regulation
  - AI memory
```

This avoids loading full skills into context.

### 2. Retrieve candidates

When the agent sees a situation, it retrieves candidate skills.

User says:

> Our team keeps adding process, but everyone still seems confused. Leadership keeps optimizing old metrics that no longer match what users care about.

Candidate skills:

- map-lag-failure
- coordination-debt
- proxy-corrosion
- ontology-fracture

### 3. Run fit checks

Before using a Concept Skill, the agent checks:

```text
Does this apply?
What evidence supports it?
What evidence would disconfirm it?
Is there a simpler explanation?
Would this concept be overreach?
```

This is crucial. Without fit checks, Concept Skills become buzzword generators.

### 4. Load the full skill only if useful

If the fit is good, the agent loads the full Concept Skill.

Then it applies:

- core model
- reasoning steps
- predictions
- interventions
- counterexample checks

### 5. Compose several skills

Real situations often need multiple abstractions.

Example:

```text
organizational slowdown =
  map-lag-failure
  + coordination-debt
  + ontology-fracture
```

The runtime should not blindly merge everything. It should ask what each concept contributes.

### 6. Produce an answer or plan

The output should not be jargon-heavy. The agent should translate the concept into useful human language.

Bad:

> This is clearly map-lag failure plus proxy corrosion plus ontology fracture.

Good:

> The deeper problem may be that the company is steering with an old map. The metrics and categories were built for an earlier product phase, so adding process just makes people more efficient at optimizing the wrong model.

### 7. Record outcome

After use, the agent records whether the concept helped.

Signals:

- user accepted/rejected diagnosis
- prediction succeeded/failed
- intervention worked/failed
- concept was overused
- concept needed revision

This turns Concept Skills into a learning substrate.

---

## Concept Skill network

The larger vision is a fast network of skills and concept skills.

```text
local agent
  -> local skill cache
  -> private org skill registry
  -> public/p2p skill network
  -> reputation/eval layer
  -> distillation layer
```

Agents could:

- discover new skills
- fork skills
- edit skills
- publish improved versions
- run evals
- vote/reputation-weight skills
- merge related skills
- deprecate broken skills
- specialize general skills for domains

This starts to look like a package manager for cognition.

Not NPM for code.

NPM for reusable mental tools.

---

## P2P skill sharing

A decentralized or semi-decentralized Concept Skill network could work like this:

### Skill identity

Each skill has a content hash.

```text
concept://sha256/...
```

### Metadata index

Agents share small metadata records first:

```yaml
name: map-lag-failure
kind: concept-skill
summary: stale representation controlling changed reality
domains:
  - organizations
  - software
  - governance
tokens_initial: 300
tokens_full: 2500
eval_score: 0.82
downloads: 18412
forks: 93
```

### Lazy loading

The full skill is loaded only when needed.

### Trust and provenance

Each skill has:

- author
- signing key
- version history
- parent/fork lineage
- eval results
- known failure modes
- compatibility notes
- user/org policy flags

### Local mutation

Agents can adapt skills locally.

Example:

```text
map-lag-failure
  -> map-lag-failure-for-software-architecture
  -> map-lag-failure-for-call-center-ops
  -> map-lag-failure-for-regulation
```

### Reputation

Reputation should not be based only on popularity. It should include eval results and downstream usefulness.

Possible scores:

- applicability precision
- overuse rate
- prediction accuracy
- intervention success
- explanation usefulness
- conflict rate with other skills

---

## The slop problem

A public skill network will immediately fill with garbage unless quality controls are built in.

Failure modes:

- buzzword concepts
- overbroad abstractions
- hidden prompt injection
- duplicated skills
- adversarial skills
- brand/political manipulation
- false expertise
- untested workflows
- cargo-cult “AI slop” skill packs
- concepts that sound deep but predict nothing

A Concept Skill should not be trusted just because it sounds good.

It should have evals.

---

## Concept Skill evals

Every Concept Skill should include tests.

### Example classification

Given cases, identify whether the concept applies.

```yaml
eval: classify_examples
cases:
  - input: >
      A company keeps optimizing a dashboard metric created before a major product pivot.
      Frontline workers say the metric no longer reflects user value.
    expected: applies

  - input: >
      A company has accurate live metrics, but executives ignore them because bonuses
      reward short-term revenue.
    expected: does_not_apply
    reason: incentive failure, not map-lag failure
```

### Prediction eval

Given a situation, predict likely outcomes.

```yaml
eval: generate_predictions
input: >
  Leadership adds weekly reporting requirements but keeps the same obsolete metrics.
expected_contains:
  - added process may increase lag
  - reports may improve apparent control but not real alignment
  - frontline frustration may increase
```

### Intervention eval

Given a case, suggest useful actions.

```yaml
eval: suggest_interventions
input: >
  Product teams disagree about what the core user action means after a business model shift.
expected_contains:
  - update product ontology
  - redefine metrics
  - align language across teams
```

### Overuse eval

Make sure the agent does not apply the concept everywhere.

```yaml
eval: detect_overuse
input: >
  The system is accurately represented, but there are not enough engineers to fix it.
expected: do_not_apply
```

---

## Concept composition

A major advantage of Concept Skills is that they can compose.

Example:

```text
map-lag-failure:
  steering with an outdated representation

proxy-corrosion:
  metric loses meaning because people optimize it

coordination-debt:
  accumulated misalignment that slows future work

ontology-fracture:
  different groups use incompatible categories
```

A situation may involve all four.

But composition needs discipline.

### Bad composition

```text
This company has map-lag failure, proxy corrosion, coordination debt,
ontology fracture, narrative overfit, semantic bottleneck, and goal collapse.
```

That is just jargon soup.

### Good composition

```text
The core issue is map-lag failure: leadership is using an old representation
of the business. That has created proxy corrosion because the old metrics
are now targets detached from real user value. The fix should start with
updating the shared product ontology, not adding more process.
```

Composition should identify:

- primary lens
- secondary lenses
- causal order
- conflicts
- interventions

---

## Concept lifecycle

A Concept Skill can mature over time.

### Stage 1: Draft concept

A human or agent writes a rough concept.

```text
This pattern seems common. Here is a proposed abstraction.
```

### Stage 2: Local use

The concept is used privately in one agent or organization.

### Stage 3: Evals

Examples, counterexamples, and tests are added.

### Stage 4: Public release

The skill is published to a registry.

### Stage 5: Forks and specialization

Other users adapt it to specific domains.

### Stage 6: Distillation

If it proves valuable, the concept may be distilled into:

- a better prompt policy
- a local memory pattern
- an activation steering vector
- a ReFT-style intervention
- a LoRA/adapter
- a specialized tool
- a model update

### Stage 7: Deprecation or merge

If it becomes redundant or misleading, it is deprecated or merged into a better concept.

---

## File layout

A practical Concept Skill package might look like this:

```text
map-lag-failure/
  SKILL.md
  concept.yaml
  examples.yaml
  counterexamples.yaml
  evals.yaml
  README.md
  provenance.json
  versions/
    0.1.0.yaml
    0.2.0.yaml
```

### SKILL.md

Human-readable instructions for agents.

### concept.yaml

Structured concept representation.

### examples.yaml

Canonical examples.

### counterexamples.yaml

Boundary cases.

### evals.yaml

Tests for use, misuse, predictions, and interventions.

### provenance.json

Authorship, signing, lineage, eval history, and compatibility.

---

## Example SKILL.md

```markdown
# Map-Lag Failure

Use this Concept Skill when diagnosing a system that appears to be controlled
using an outdated representation of a changed reality.

## Quick trigger

The system is being steered with an old map of a changed territory.

## Use when

- Metrics no longer match user value or operational reality.
- Categories were created for an earlier phase of the system.
- Leadership and frontline actors disagree because they see different realities.
- A codebase architecture reflects an obsolete product concept.
- Regulation or policy uses outdated categories.

## Do not use when

- The representation is accurate but incentives are bad.
- The main problem is lack of resources.
- The system is random/noisy rather than misrepresented.
- Decision-makers have accurate feedback but refuse to act.

## Reasoning procedure

1. Identify the controller.
2. Identify the representation being used.
3. Identify how the territory has changed.
4. Estimate the lag between map and territory.
5. Check whether current interventions optimize the stale map.
6. Predict what symptom-level fixes will decay.
7. Recommend representation updates before optimization.

## Output style

Do not overuse the phrase “map-lag failure.” Explain it in plain language.
Prefer: “The team may be steering with an old map.”
```

---

## Relationship to ConceptIR

Concept Skills are the near-term version.

```text
Concept Skill:
  human-readable
  markdown/yaml
  external memory
  RAG/skill runtime
  easy to build now

ConceptIR:
  machine-native
  typed graph + latent bindings
  activation/adapter execution
  eventually distillable
  harder to build
```

A Concept Skill can later compile into ConceptIR.

Possible pipeline:

```text
SKILL.md / concept.yaml
  -> structured concept graph
  -> examples/counterexamples
  -> eval suite
  -> latent feature search
  -> activation steering policy
  -> adapter/LoRA distillation
```

This lets the ecosystem start simple without waiting for fully interpretable model internals.

---

## Relationship to RAG

Concept Skills are a special kind of RAG object.

Normal RAG retrieves content.

Concept Skill RAG retrieves a reasoning operator.

Normal RAG says:

> Here is information that may be relevant.

Concept Skill RAG says:

> Here is a lens. Use it only if it fits. If it fits, apply this model, predictions, counterexamples, and interventions.

This is “instant RAG” with stronger structure and lower overhead.

---

## Relationship to LoRA

A Concept Skill is not a LoRA.

But a successful Concept Skill could eventually become one.

The pathway:

```text
explicit concept skill
  -> repeated successful use
  -> training data generated from applications
  -> adapter/LoRA distilled behavior
  -> smaller runtime overhead
```

So the agent ecosystem can have fast explicit learning first and slower neural consolidation later.

This is similar to humans:

```text
read definition
  -> practice using concept
  -> build intuitions
  -> internalize
  -> use automatically
```

---

## Relationship to agent memory

Concept Skills should integrate with memory.

Memory stores:

- facts
- user preferences
- past events
- procedures
- domain knowledge
- concepts

A Concept Skill is a memory object that changes interpretation.

A useful memory system should distinguish:

```text
fact memory:
  The user uses Qwen 3.6 27B.

procedure memory:
  When creating spreadsheets, use openpyxl.

concept memory:
  Map-lag failure means a stale representation is steering changed reality.

policy memory:
  Prefer concise answers unless detail is needed.
```

Concept memory deserves its own format.

---

## Fast path implementation

A very practical prototype could be built now.

### Components

1. **Concept Skill registry**
   - local folder or Git repo
   - each skill is a directory
   - metadata indexed into embeddings

2. **Retriever**
   - semantic search over summaries/triggers
   - negative trigger check
   - domain filter
   - recency/version filter

3. **Fit checker**
   - asks whether candidate concept applies
   - compares against counterexamples
   - returns score and rationale

4. **Skill loader**
   - loads compact version first
   - loads full version only if score is high

5. **Reasoning wrapper**
   - applies selected concept skill
   - forces predictions/counterexamples/interventions
   - prevents jargon soup

6. **Evaluator**
   - logs outcome
   - runs built-in evals
   - records success/failure

7. **Mutator**
   - suggests edits after repeated failures
   - forks skill for special domains

### Minimal architecture

```text
user/task
  -> retrieve candidate concept skills
  -> fit-check against triggers/counterexamples
  -> load best skill(s)
  -> apply reasoning procedure
  -> answer user
  -> log outcome
  -> update skill scores
```

---

## Pseudocode

```python
def use_concept_skills(task, context):
    candidates = retrieve_skill_metadata(task, context)

    scored = []
    for skill in candidates:
        fit = check_fit(skill.metadata, task, context)
        if fit.score > 0.65:
            scored.append((skill, fit))

    selected = choose_non_redundant_skills(scored, max_skills=3)

    loaded = [load_full_skill(skill) for skill, fit in selected]

    reasoning_context = build_reasoning_context(
        task=task,
        concept_skills=loaded,
        require_counterexample_checks=True,
        require_predictions=True,
        require_interventions=True,
        avoid_jargon=True,
    )

    answer = model.generate(reasoning_context)

    log_skill_usage(task, selected, answer)

    return answer
```

---

## Security and governance

A skill network becomes a supply-chain surface.

Threats:

- prompt injection inside skill files
- malicious tool instructions
- subtle ideological steering
- reputation gaming
- skill squatting
- poisoned evals
- dependency confusion
- hidden exfiltration instructions
- adversarial triggers that cause over-selection

Needed defenses:

- signed skill packages
- permission scopes
- static analysis
- sandboxing
- eval transparency
- trust roots
- local allowlists
- dependency pinning
- provenance history
- human-readable diffs
- restricted tool access per skill

Concept Skills may be safer than procedural skills because they often do not require tool execution. But they can still steer reasoning, so they need governance.

---

## Why agents might write Concept Skills themselves

One of the most interesting possibilities is self-authoring.

An agent repeatedly notices a pattern:

```text
In many failed projects, the team was not just poorly coordinated.
They were using incompatible definitions of the core object.
```

It proposes a concept:

```text
ontology fracture
```

Then it writes:

- definition
- triggers
- counterexamples
- predictions
- interventions
- examples
- evals

Then it uses the concept.

If useful, it refines it.

If not, it deletes or merges it.

This is a primitive form of abstraction invention.

Not neural-native yet. But operationally similar.

---

## Example: Ontology Fracture

```yaml
name: ontology-fracture
kind: concept-skill
version: 0.1.0

summary: >
  A coordination failure where different actors use incompatible categories
  or definitions for what appears to be the same domain.

use_when:
  - teams keep agreeing verbally but implementing different things
  - arguments recur because key terms have different meanings
  - product, engineering, sales, and users classify the same object differently
  - documentation exists but does not resolve confusion

do_not_use_when:
  - everyone shares definitions but incentives conflict
  - the issue is lack of information rather than incompatible categories
  - disagreement is about values, not definitions

triggers:
  positive:
    - same word means different things
    - repeated re-explaining
    - handoff failures
    - data model mismatch
    - teams think they agree but outputs diverge
  negative:
    - explicit value conflict
    - resource shortage
    - simple missing documentation

core_model:
  shared_label: the word or category people think they share
  local_meanings: different meanings attached by each actor/group
  translation_cost: cost of converting between meanings
  fracture_line: where incompatible ontologies meet

predictions:
  - documentation will help only if it resolves category boundaries
  - adding process may increase translation overhead
  - bugs will appear at handoff boundaries
  - meetings will produce apparent agreement but divergent implementation

interventions:
  - define canonical ontology
  - create examples and non-examples
  - rename overloaded concepts
  - update data models to match real distinctions
  - assign ownership for boundary definitions
```

---

## Example: Proxy Corrosion

```yaml
name: proxy-corrosion
kind: concept-skill
version: 0.1.0

summary: >
  A failure mode where a metric or proxy loses connection to the value it
  was meant to represent because actors optimize the proxy directly.

use_when:
  - metrics improve while real outcomes degrade
  - people game measurements
  - a target becomes detached from the underlying goal
  - dashboards show success but users/customers/operators disagree

do_not_use_when:
  - the metric was never intended to represent the value
  - real outcomes are improving along with the metric
  - data collection is simply broken

triggers:
  positive:
    - metric gaming
    - vanity metrics
    - dashboard success, real-world failure
    - incentives tied to proxy
    - Goodhart effects
  negative:
    - no optimization pressure
    - measurement bug only
    - low data quality without gaming

core_model:
  true_goal: the value the system actually cares about
  proxy: measurable substitute for the true goal
  optimization_pressure: incentive to improve the proxy
  corrosion: degradation of proxy-goal correlation

predictions:
  - proxy improvement will diverge from true outcome improvement
  - actors will discover cheaper ways to move the metric
  - metric dashboards will become less trustworthy over time
  - adding rewards to the proxy may accelerate corrosion

interventions:
  - rotate metrics
  - audit proxy-goal correlation
  - include qualitative reality checks
  - reward bundles of metrics rather than one target
  - reduce direct optimization pressure on the proxy
```

---

## A Concept Skill should be boring

This is important.

The format should discourage grandiose naming and encourage usefulness.

Bad Concept Skill:

```text
Transcendent Recursive Ontological Collapse
```

Good Concept Skill:

```text
metric drift
unclear owner
handoff mismatch
stale map
proxy corrosion
```

The best concepts are often simple names for real recurring patterns.

A Concept Skill should earn its place by improving predictions and interventions.

---

## Design principles

### 1. Compact first

Load metadata first. Load full instructions only when needed.

### 2. Counterexamples are mandatory

Every concept is dangerous without boundaries.

### 3. Predictions matter

A concept that predicts nothing is probably just vocabulary.

### 4. Interventions matter

A concept should provide handles for action.

### 5. Avoid jargon in user-facing output

The agent can use the concept internally, then explain plainly.

### 6. Concepts should compose, but not too freely

Require causal order and primary/secondary lens selection.

### 7. Skills should be evaluated, not merely liked

Popularity is not truth.

### 8. Local trust beats global hype

Agents should prefer locally validated skills over viral skills.

### 9. Concepts can be temporary

Not every abstraction deserves permanence.

### 10. Distill later

Start explicit. Internalize only after repeated success.

---

## Why this could be a serious infrastructure layer

If agents become common, they will need shared ways to improve.

They can share:

- tools
- prompts
- workflows
- code
- memories
- evals
- skills

But the highest-leverage thing they can share may be:

```text
ways of seeing
```

A good abstraction lets an agent notice a pattern earlier, reason about it better, and choose better interventions.

That is more valuable than a single fact or workflow.

Concept Skills are a possible format for sharing “ways of seeing” in a practical, testable, networked way.

---

## One-line pitch

**Concept Skills are skill-file-like packets for reusable abstractions: compact, testable cognitive lenses that agents can retrieve, apply, compose, refine, share, and eventually distill.**

---

## Suggested repo structure

```text
concept-skills/
  README.md
  schema/
    concept_skill.schema.json
  skills/
    map-lag-failure/
      SKILL.md
      concept.yaml
      examples.yaml
      counterexamples.yaml
      evals.yaml
      provenance.json
    ontology-fracture/
      SKILL.md
      concept.yaml
      examples.yaml
      counterexamples.yaml
      evals.yaml
      provenance.json
    proxy-corrosion/
      SKILL.md
      concept.yaml
      examples.yaml
      counterexamples.yaml
      evals.yaml
      provenance.json
  runtime/
    retriever.py
    fit_checker.py
    composer.py
    evaluator.py
  docs/
    design-principles.md
    security.md
    composition.md
```

---

## Minimal schema sketch

```json
{
  "name": "string",
  "kind": "concept-skill",
  "version": "semver",
  "summary": "string",
  "domains": ["string"],
  "use_when": ["string"],
  "do_not_use_when": ["string"],
  "triggers": {
    "positive": ["string"],
    "negative": ["string"]
  },
  "core_model": {
    "variable_name": "description"
  },
  "reasoning_steps": ["string"],
  "predictions": ["string"],
  "interventions": ["string"],
  "examples": ["string"],
  "counterexamples": ["string"],
  "composition": {
    "combines_with": {
      "concept_name": "relationship"
    },
    "conflicts_with": ["concept_name"]
  },
  "evals": ["eval_id"]
}
```

---

## Closing thought

The deep version of this idea is machine-native concept patches: compact abstractions bound directly into model latents, steering policies, adapters, or weights.

But the deployable version can start much simpler.

A Concept Skill is just a disciplined artifact:

```text
a name
a pattern
a boundary
examples
counterexamples
predictions
interventions
evals
```

Loaded on demand through a fast skill/RAG layer, this could already make agents more consistent, more teachable, and better at accumulating reusable abstractions.

It is not a replacement for better models.

It is a way for models to build and share better mental tools.
