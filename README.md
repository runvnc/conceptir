# Ambient Agent Skills: Fast Shared Snippets for Agent Networks

## Summary

Ambient Agent Skills are lightweight `skill://...` references that agents can casually create, cite, retrieve, and refine during normal work.

Unlike formal installed skills, these do not need to be packages, directories, scripts, or slow setup procedures. They can be short reusable snippets that describe a task pattern, concept, warning, communication convention, project rule, or local preference.

The main change is simple:

> Agents should routinely share small reusable skill/concept references as they communicate, and agent runtimes should retrieve those references instantly before completion requests.

This creates a fast shared memory layer for agent networks.

---

## Core idea

Current agent skills are often treated like installed packages:

```text
create skill -> install skill -> load skill -> use skill
```

Ambient Agent Skills are more like shared working references:

```text
notice reusable pattern
  -> write small skill snippet
  -> publish to fast snippet store
  -> cite skill://reference in normal communication
  -> other agents retrieve it automatically
  -> refine or fork through repeated use
```

The goal is not to build a perfect ontology. The goal is to make reusable knowledge cheap enough that agents create and share it constantly.

---

## What can an ambient skill describe?

An ambient skill may describe:

- a reusable procedure
- a concept or diagnostic lens
- a warning or failure mode
- a project-specific rule
- a communication convention
- a user preference
- a tool-use pattern
- a domain assumption
- an evaluation/checking habit

The important thing is not whether it contains code. The important thing is whether retrieving it changes future behavior in a useful way.

---

## Why not just use RAG?

Normal RAG retrieves information.

Ambient skills retrieve reusable operational snippets.

A normal RAG result says:

> Here is something relevant.

An ambient skill says:

> Here is a pattern other agents already know how to apply.

That can be a procedure, but it can also be a concept. For example, agents might refer to `skill://stale-map-diagnosis` when a team appears to be optimizing old metrics after reality has changed.

---

## Why not just use formal installed skills?

Formal skills are still useful for larger workflows, scripts, and tool-heavy tasks.

Ambient skills are lighter. They should be easy to create casually, easy to reference in conversation, and easy to retrieve with no install step.

An ambient skill that becomes important can later graduate into a formal skill package.

```text
ambient snippet
  -> frequently reused snippet
  -> structured skill
  -> formal installed skill
  -> possible adapter/model distillation later
```

---

## Example: a more realistic ambient skill

```yaml
id: skill://stale-map-diagnosis
title: Stale Map Diagnosis
kind: concept
scope: shared
version: 0.1.0
tags:
  - systems
  - organizations
  - software
  - metrics
  - diagnosis

summary: >
  Use this when a person, team, organization, or software system appears to be
  making decisions from an outdated representation of reality.

body: >
  A stale-map problem happens when the "map" being used to steer a system no
  longer matches the territory. The map might be a dashboard, product ontology,
  architecture diagram, legal category, user persona, self-image, roadmap, or
  management narrative. The giveaway is that people keep adding effort, process,
  or optimization, but the interventions decay because they are aimed at the old
  model rather than the current system.

  When applying this skill, identify three things: the controller, the map, and
  the changed territory. Then ask what decisions are being made from the stale
  map and what would change if the map were updated first. Do not overuse this
  diagnosis when the real problem is bad incentives, insufficient resources,
  random failure, or accurate information being ignored. Prefer plain language
  in user-facing output: "The team may be steering with an old map" is usually
  better than naming the concept.

example_uses:
  - A company keeps optimizing an activation metric created before a product pivot.
  - A codebase architecture reflects a product model the company no longer uses.
  - A regulation treats a new technology as if it fit an old category.
  - A user keeps making plans based on an outdated self-image.

counterexamples:
  - Leaders have accurate data but incentives reward ignoring it.
  - The map is current, but the team lacks money or staff.
  - The system is chaotic enough that no stable map would help.
```

This is long enough to be useful, but still small enough to retrieve cheaply.

---

## Normal agent communication

Agents should be able to include skill references naturally:

```text
This looks like skill://stale-map-diagnosis. The team is adding process,
but the process is still pointed at old metrics.
```

A receiving agent can automatically expand `skill://stale-map-diagnosis` before its next completion request.

This makes skill references into shared vocabulary. The reference is short, but it points to a richer reusable object.

---

## Runtime flow

A minimal runtime loop:

```text
incoming message/task
  -> extract explicit skill:// references
  -> retrieve those snippets
  -> semantic search for a few additional relevant snippets
  -> pack selected snippets into context
  -> generate response/action
  -> optionally save a new snippet
  -> log which snippets helped
```

The retrieval must be fast and automatic. Agents should not need a manual installation phase.

---

## Shared snippet store

The snippet store can be local, organizational, public, or peer-to-peer.

A practical hierarchy:

```text
local cache
  -> project/team store
  -> organization store
  -> trusted peers
  -> public/global index
```

Each snippet should have a stable reference, such as:

```text
skill://stale-map-diagnosis
skill://project/cyberpad/hosting-assumption
skill://user/runvnc/markdown-handoff-preference
skill://public/proxy-corrosion
```

Exact references should always win. Semantic retrieval can add likely relevant snippets.

---

## Creation policy

Agents should not save every thought. A snippet is worth saving when it is:

- reusable
- short enough to retrieve cheaply
- likely to recur
- useful to other agents
- not already covered by a better snippet
- safe to share at the chosen scope

Scopes might include:

```text
private
conversation
project
organization
trusted-network
public
```

---

## Trust and quality

A shared snippet network can turn into slop or become a prompt-injection surface.

Basic protections:

- signed snippets
- scope controls
- allowlists/blocklists
- provenance and version history
- human review for high-trust scopes
- safe defaults for public snippets
- no automatic tool permissions from public snippets
- usage logging and helpfulness scores

Popularity alone should not determine trust. The best snippets should be clear, reusable, behavior-changing, and easy to ignore when irrelevant.

---

## Minimal data model

```json
{
  "id": "skill://stale-map-diagnosis",
  "title": "Stale Map Diagnosis",
  "kind": "concept",
  "scope": "shared",
  "summary": "Use when decisions are being made from an outdated representation of reality.",
  "body": "A longer reusable explanation, with examples and counterexamples.",
  "tags": ["systems", "diagnosis", "metrics"],
  "version": "0.1.0",
  "author": "agent://example",
  "related": ["skill://proxy-corrosion"]
}
```

---

## Main behavioral change

The important part is not the file format.

The important part is the norm:

> Agents routinely create, cite, retrieve, and refine lightweight skill/concept snippets as part of ordinary work.

Instead of each agent re-deriving or re-explaining every useful pattern, agents can say:

```text
This is probably skill://stale-map-diagnosis with some skill://proxy-corrosion.
```

and the receiving agent can instantly load the relevant patterns.

---

## Short pitch

Ambient Agent Skills are lightweight `skill://...` snippets that agents casually create, cite, retrieve, and refine during normal work. They can describe procedures, concepts, warnings, preferences, or local rules. Unlike installed skills, they require no slow setup. They live in a fast shared snippet store and are retrieved automatically before completion requests, making reusable patterns part of ordinary agent communication.
