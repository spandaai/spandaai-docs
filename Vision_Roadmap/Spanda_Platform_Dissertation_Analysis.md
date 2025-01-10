# Spanda Platform: Dissertation Analysis

## High-Level Purpose
- Ingest academic documents for structure, originality, rubric alignment, and feedback generation.
- Features:
  - Document ingestion/parsing.
  - AI-driven analysis: Semantic understanding, rubric alignment, plagiarism checks.
  - Reporting and grading suggestions.

## Mapping to Spanda Platform Layers
- **Platform Layer:** Model serving, data/message brokers, monitoring.
- **Domain Layer:** Dissertation-specific logic, rubric modules, fine-tuning.
- **Solutions Layer:** Presentation APIs, reporting/export tools.

## v0.5 “Platformed” Dissertation Analysis
- Single-node deployment via Docker Compose.
- Modular containers for Platform, Domain, and App layers.
- GPU/CPU support for local inference.

