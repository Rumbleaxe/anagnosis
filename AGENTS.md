# AGENTS.md

## Project

Anagnosis is a research-driven OCR and document reconstruction system focused on degraded technical documents.

The project targets:
- scanned standards
- engineering specifications
- legal documents
- photocopies
- historical technical material
- multilingual degraded text

The primary goal is not merely text extraction, but reliable semantic reconstruction for downstream retrieval and analysis systems.

Anagnosis is designed around reproducibility, modular experimentation, and practical robustness under real-world document conditions.

---

# Core Principles

## 1. OCR is not the final objective

The system exists to preserve meaning and structure, not only maximize raw OCR scores.

Downstream usability matters:
- retrieval quality
- specification extraction
- semantic fidelity
- document structure preservation
- reference integrity

Character-perfect OCR with broken semantics is considered failure.

---

## 2. Real-world documents first

The system is optimized for:
- skewed scans
- noisy photocopies
- faded print
- mixed languages
- low DPI
- annotations
- tables
- typewriter-era artifacts
- compression artifacts

Development should prioritize robustness over benchmark vanity.

---

## 3. Modular pipeline architecture

Every stage must remain independently replaceable.

Typical stages:
1. ingestion
2. preprocessing
3. segmentation
4. OCR
5. semantic reconstruction
6. structure extraction
7. indexing/export

No component should tightly couple to a specific OCR engine or model.

---

## 4. Research and engineering coexist

The repository serves both:
- production-grade tooling
- reproducible scientific experimentation

Code must therefore prioritize:
- determinism
- reproducibility
- traceability
- measurable outputs

Avoid opaque heuristics without explicit justification.

---

# Engineering Guidelines

## Repository philosophy

Prefer:
- small composable modules
- explicit interfaces
- readable code
- deterministic behavior
- typed boundaries where possible

Avoid:
- giant utility files
- hidden global state
- premature abstractions
- framework-heavy architecture
- magic configuration

---

## Performance philosophy

The project explicitly acknowledges the OCR tradeoff triangle:
- quality
- speed
- cost

No single operating point dominates all use cases.

The architecture should therefore support:
- local lightweight inference
- hybrid pipelines
- ensemble OCR
- optional expensive passes for uncertain regions

Performance decisions must always be measurable.

---

## Error handling

Silent OCR failures are unacceptable.

Every pipeline stage should:
- expose confidence metrics
- surface uncertainty
- preserve provenance
- emit diagnostics where possible

Low-confidence extraction is preferable to fabricated certainty.

---

# Directory Expectations

Suggested structure:

```text
/anagnosis
    /core
    /pipeline
    /ocr
    /preprocessing
    /reconstruction
    /layout
    /evaluation
    /datasets
    /experiments
    /cli
    /api
    /tests
```


This structure may evolve, but separation of concerns must remain clear.

---

# OCR Philosophy

## Ensemble-first mindset

No OCR engine is universally optimal.

The system should support:

* engine comparison
* confidence aggregation
* selective fallback
* region-specific OCR strategies

The architecture must make experimentation easy.

---

## Domain-aware reconstruction

Semantic repair is a first-class concern.

Examples:

* technical notation correction
* unit normalization
* standards references
* multilingual consistency
* table reconstruction
* symbol disambiguation

The reconstruction layer is part of the research contribution, not postprocessing glue.

---

# Evaluation Principles

The project values downstream utility over isolated OCR metrics.

Important evaluation dimensions include:

* character error rate
* word error rate
* retrieval accuracy
* extraction precision
* structural fidelity
* latency
* robustness under degradation

All experiments should be reproducible.

---

# Coding Standards

## General

* Prefer explicitness over cleverness.
* Prefer pure functions where practical.
* Keep side effects isolated.
* Avoid unnecessary dependencies.
* Write code that survives six months of forgetting.

---

## Naming

Names should reflect:

* intent
* domain meaning
* pipeline responsibility

Avoid vague names like:

* helper
* utils
* temp
* manager

---

## Comments

Comments should explain:

* why
* assumptions
* tradeoffs

Avoid narrating obvious code behavior.

---

# Testing Expectations

Testing is mandatory for:

* preprocessing logic
* reconstruction logic
* parsers
* normalization
* layout analysis
* evaluation metrics

Where possible:

* use real degraded samples
* preserve regression fixtures
* test pathological cases

Perfectly clean documents are insufficient validation.

---

# Research Expectations

Experimental code is allowed.

However:

* experiments must remain isolated
* findings should be reproducible
* benchmark contamination must be avoided
* temporary research code should not silently become production code

Research credibility matters.

---

# Design Philosophy

Anagnosis is not designed to chase benchmark theater.

The project exists to solve difficult document understanding problems under imperfect real-world conditions.

The system should remain:

* inspectable
* extensible
* reproducible
* domain-adaptive
* scientifically grounded

Robustness is preferred over spectacle.
