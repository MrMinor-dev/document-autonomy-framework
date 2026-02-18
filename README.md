# Document Autonomy System

**An agentic document management framework that improved doc quality scores from 45→88/100 with 36/36 gate passes — built so AI agents can find, trust, and act on documentation without human help.**

---

## The Problem

Documentation written for humans fails agents. A human reads a doc and fills in ambiguity from context — they know what "the main workflow" means, they understand implied references, they infer whether a doc is current or stale. An AI agent reads the same doc and either hallucinates the gaps, retrieves the wrong document, or stalls waiting for clarification.

The problem compounds at scale. When a system has 20+ authoritative documents with overlapping scope, agents can't determine which is the source of truth, which overrides which, or whether a doc they found is still valid. The result is either over-retrieval (load everything, waste tokens) or under-retrieval (miss the right doc, act on stale information).

The standard answer — "just keep docs updated" — misses the structural issue. A doc can be perfectly current and still fail an agent because it uses ambiguous section headers, implicit cross-references, or prose that chunks poorly in semantic search. Agentic readability is a distinct design requirement, not a side effect of being well-written for humans.

---

## Architecture

The system has three layers: standards, registry, and validation.

```
┌──────────────────────────────────────────────────────────┐
│                  AGENTIC DOC STANDARDS                   │
│                                                          │
│  Semantic headers (headers = concepts, not topics)       │
│  YAML frontmatter (name, description, version, status)   │
│  Unambiguous cross-references (file path, not name)      │
│  Chunking optimization (≤500 tokens per logical unit)    │
│  SSOT declaration (is this THE source, or a consumer?)   │
└──────────────────────┬───────────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────────┐
│                    SSOT REGISTRY                         │
│               (SSOT-REGISTRY-MASTER.md)                  │
│                                                          │
│  20+ authoritative documents tracked                     │
│  Cascade dependencies mapped (A → B → C)                │
│  Override rules explicit (which doc wins on conflict)    │
│  Status per doc: active / deprecated / draft             │
└──────────────────────┬───────────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────────┐
│                9-GATE QUALITY VALIDATION                 │
│                                                          │
│  Gate 1: YAML frontmatter present and complete           │
│  Gate 2: SSOT status declared                            │
│  Gate 3: Section headers are concept-named               │
│  Gate 4: Cross-references use file paths                 │
│  Gate 5: No ambiguous pronouns (it/this/that)            │
│  Gate 6: Chunking boundaries at logical breaks           │
│  Gate 7: Version and last-updated present                │
│  Gate 8: Registry entry exists                           │
│  Gate 9: Cascade dependencies documented                 │
└──────────────────────────────────────────────────────────┘
```

**Naming convention:** `-MASTER.md` = authoritative source. Any doc without this suffix is a consumer or reference — it may cite the MASTER but cannot override it. Agents learn this pattern once and apply it universally.

---

## Key Insight

**Doc quality is measurable — and the score is consistent across doc sizes.**

The 9-gate system was validated across 4 documents of very different sizes and complexity. All 4 scored exactly 88/100 after optimization. A small test document and a 14x-larger production document reached the same score because the gates test *structural properties*, not content volume.

This matters because it means doc quality is auditable without reading the content. An agent (or a human auditor) can score any document in under 2 minutes by checking gate presence — no judgment calls required. 45→88 isn't a fuzzy improvement; it's specific gates that changed.

The second insight: **cascade dependencies are the missing layer in most doc systems.** If Document A references Document B, and B is updated, A may now be wrong — but nothing flags it. The SSOT Registry tracks these dependencies explicitly so a change to one document surfaces all downstream documents that need review. This is the difference between a doc system that stays current and one that slowly drifts into inconsistency.

---

## Results

- **Doc quality scores: 45→88/100** across all optimized documents
- **36/36 gate passes** — 9 gates × 4 documents, zero rework required
- **20+ documents** tracked in SSOT Registry with cascade dependencies
- **Consistent scoring across document sizes** — small test doc and 14x-larger production doc scored identically, validating the structural approach
- **`doc-management-skill v2.0`** packages this as a reusable capability with agentic execution standards
- **Applied across 350+ sessions** — every document in the MRMINOR knowledge base conforms to these standards

---

## Built With

- **`doc-management-skill v2.0`** — skill module packaging the full workflow (create → validate → register → index)
- **Supabase pgvector** — semantic index that benefits directly from chunking optimization
- **SSOT Registry** — living document tracking 20+ authoritative sources with dependency graph
- **MRMINOR HAIOS/AOS** — production environment where these standards were developed and validated
- **350+ sessions** of agentic operations providing the signal for what breaks and what works

---

## Related Work

This framework is part of a larger [Human-AI Operating System (HAIOS)](https://mrminor-dev.github.io) — a production infrastructure for human-AI collaboration built across 350+ sessions.

Other components:
- [ai-skills-framework](https://github.com/MrMinor-dev/ai-skills-framework) — the capability module system that uses these doc standards
- [semantic-search-framework](https://github.com/MrMinor-dev/semantic-search-framework) — the retrieval layer that benefits from chunking optimization
- [quality-assurance-framework](https://github.com/MrMinor-dev/quality-assurance-framework) — 60-point audit system built using the same validation-gate approach

---

## Author

**Jordan Waxman** — AI Systems & Operations  
14 years operations leadership + 350 sessions building human-AI infrastructure  

[Portfolio](https://mrminor-dev.github.io) · [GitHub](https://github.com/MrMinor-dev) · [Email](mailto:waxmanj@mac.com)

---

*Part of the MRMINOR HAIOS/AOS production infrastructure. Every number verified from production systems.*
