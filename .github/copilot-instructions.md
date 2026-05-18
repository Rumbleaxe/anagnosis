# Copilot Instructions

## Project

Anagnosis is a research-driven OCR and document reconstruction system for degraded technical documents (scanned standards, engineering specs, legal docs, photocopies, historical material, multilingual text). The goal is **semantic reconstruction** for downstream retrieval and analysis — not raw OCR accuracy alone. Character-perfect output with broken semantics is considered failure.

## Architecture

The system follows a modular, stage-based pipeline:

```
ingestion → preprocessing → segmentation → OCR → semantic reconstruction → structure extraction → indexing/export
```

Each stage is independently replaceable. No component should tightly couple to a specific OCR engine or model. The architecture must support ensemble OCR (multiple engines, confidence aggregation, selective fallback, region-specific strategies).

Expected module layout:

```
/core          # shared primitives and interfaces
/pipeline      # stage orchestration
/ocr           # engine adapters and ensemble logic
/preprocessing # image normalization, deskew, denoising
/reconstruction # semantic repair, normalization, table reconstruction
/layout        # page segmentation, region classification
/evaluation    # metrics and benchmarking
/datasets      # data loading and fixture management
/experiments   # isolated research/experimental code
/cli           # command-line entrypoints
/api           # programmatic API surface
/tests         # test suite
```

## Key Conventions

### Pipeline design
- Every stage must expose **confidence metrics**, surface uncertainty, and preserve provenance. Silent failures are unacceptable.
- Low-confidence extraction is preferable to fabricated certainty.
- No hidden global state; all configuration must be explicit.

### Naming
- Names must reflect domain meaning and pipeline responsibility.
- Avoid: `helper`, `utils`, `temp`, `manager`.

### Code style
- Prefer pure functions; isolate side effects.
- Prefer explicitness over cleverness.
- Avoid unnecessary dependencies and premature abstractions.
- Typed boundaries where possible.
- Comments explain *why*, assumptions, and tradeoffs — not what the code obviously does.

### Testing
- Tests are mandatory for: preprocessing logic, reconstruction logic, parsers, normalization, layout analysis, and evaluation metrics.
- Use real degraded document samples where possible; preserve regression fixtures.
- Test pathological cases — clean documents are insufficient validation.

### Research vs. production code
- Experimental code lives under `/experiments` and must remain isolated.
- Research code must not silently migrate into production modules.
- All experiments must be reproducible; avoid benchmark contamination.

### Performance tradeoffs
- The system explicitly acknowledges the OCR tradeoff triangle: quality ↔ speed ↔ cost.
- Performance decisions must be measurable. No opaque heuristics without explicit justification.
- Architecture must support: local lightweight inference, hybrid pipelines, ensemble OCR, optional expensive passes for uncertain regions.

### Evaluation
- Downstream utility (retrieval accuracy, extraction precision, structural fidelity) takes precedence over isolated OCR metrics like CER/WER.
- All benchmarks must be reproducible.
