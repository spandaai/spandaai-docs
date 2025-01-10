let’s summarize what the Dissertation Analysis solution does today, then map each of its major components to the appropriate layer in the Spanda Platform (Platform Layer, Domain Layer, Solutions/App Layer). Finally, we’ll show how the 0.5 “platformed” version of the Dissertation Analysis integrates into a single-node deployment, paving the way for the future multi-node fabric in v1.0.



1. High-Level Purpose of Dissertation Analysis

The Dissertation Analysis repository is an end-to-end prototype (or standalone solution) for:

Ingesting Academic Documents (theses, dissertations, reports)

Text Preprocessing & Chunking (splitting long documents into sections/chapters)

AI-Driven Analysis

Semantic Understanding: Summaries, key points, or concept extraction

Rubric Alignment / Feedback: Mapping textual segments to rubric criteria and providing automated or semi-automated feedback

Plagiarism / Similarity Checks: (If configured) Identifying suspiciously similar text or references

Reporting & Grading Suggestions

Generating targeted feedback for instructors or TAs

Possibly outputting a grade “draft” or recommended rubric scores

(Optional) Web or CLI Interface

Depending on the user’s environment, some combination of Python scripts, notebooks, or minimal front-end might be present to upload a dissertation file and see the analysis results

In short, it addresses the academic workflow of evaluating a dissertation’s structure, originality, and alignment with standardized requirements or rubrics.



2. Current Repo Structure & Components

Although the exact folder layout may differ, the Dissertation Analysis repo typically includes (or is likely to include) components such as:

Data Ingestion & Processing

Scripts to read PDF/Word documents (e.g., ingest.py, parser.py, etc.)

Utilities for chunking large text into smaller sections, extracting metadata, or cleaning up references.

LLM Integration / Model Scripts

Code that calls out to large language models (e.g., via Hugging Face Transformers, GPT-based endpoints, or local LLM runners like Ollama).

Fine-tuned model weights or references to external model hosts.

Rubric & Feedback Logic

A set of rules or python modules that compare a chunk of text against specific rubric criteria (organization, clarity, novelty, referencing, etc.).

Possibly a “scoring” or “ranking” script that compiles final feedback.

User Interface / CLI

A minimal web UI (Flask, FastAPI, or Streamlit) or a Jupyter notebook that demonstrates how a user might upload a dissertation, run an analysis, and view results.

Alternatively, a command-line approach with a main.py script that processes an input file and prints out suggestions or a grading breakdown.

Config & Dependencies

Requirements files, environment configs, or partial Dockerfiles that define how everything runs.

(Optional) Database Adapters

Some repos might store results or logs in a local SQLite/MySQL.

Others might have a simple “no-database” approach with everything in memory.

Note: Over time, these pieces will be migrated into dedicated microservices or modules so they integrate nicely into the Spanda.AI multi-layer ecosystem.



3. Mapping to the Spanda.AI Platform Layers

3.1 Platform Layer

Model Serving Infrastructure: If the Dissertation Analysis is using local LLM inference (e.g., Ollama or any container-based serving mechanism), that “inference runtime” belongs to the Platform Layer.

Data & Message Brokers: Any references to Kafka, MySQL, Redis, or Prometheus monitoring in the repo will ultimately be “lifted” and placed in the Platform. For instance:

A container that provides MySQL for storing user data or feedback results.

Redis for quick caching of chunked text or partial inference results.

Kafka for processing large volumes of text in an asynchronous pipeline.

Prometheus or “DockProm” for capturing usage metrics and system performance.

Rationale: The “foundation” for compute, data management, and logging/monitoring is a Platform responsibility.

3.2 Domain Layer (EdTech)

Dissertation-Specific Logic: The Python scripts or classes that define how to chunk, parse, and interpret dissertations.

Rubric / Grading Modules: Code that compares text sections to domain rubrics.

Model Fine-Tuning: If you’ve built or integrated custom language model weights specifically tuned for academic or dissertation text, that belongs in the EdTech Domain subfolder.

Agent Orchestration (LangGraph): In 0.5, if we add an agent workflow that’s specifically curated for dissertation tasks (chain-of-thought logic or multi-step parsing for academic text), it would primarily be in the Domain Layer, although the agent framework’s libraries (LangGraph) get installed in the Platform. The “domain flow” or prompt templates, however, live in EdTech code.

Rationale: Domain Layer = all the specialized business or sector-specific knowledge. In EdTech’s case, it’s the dissertation analysis methodology and the academic rubrics themselves.

3.3 Solutions Layer (“Dissertation Analysis App”)

Presentation & APIs: Any front-end code, UI dashboards, or REST endpoints that present final grading suggestions to a user (instructor, TA, admin).

Integration Adapters: If the solution needs to plug into an LMS (Learning Management System), or to push results to a school’s administrative platform, that bridging code sits in the Solutions Layer.

Reporting & Export: PDF or HTML generation for a final “feedback” report that an instructor might share with a student.

Rationale: This top layer is all about end-user experiences, integration points, and “last-mile” delivery of the domain’s AI capabilities.



4. The 0.5 “Platformed” Version of Dissertation Analysis

With Spanda.AI v0.5, we’re taking the functionality from the standalone Dissertation Analysis repo and containerizing it into a single-node (Mac or PC, CPU or GPU) setup. Here’s how that works in practice:

docker-compose.yml (Platform)

The base docker-compose.yml references containers like Kafka, MySQL, Redis, Prometheus, etc.

Also includes a container for Ollama or other local model servers.

Optionally includes a container for the agent framework (LangGraph) if it’s integrated in 0.5.

EdTech Domain Container

A container image that packages up the domain logic for dissertation analysis: chunking scripts, rubric logic, any specialized LLM-fine-tuning code.

This container depends on the Platform containers (for data storage, logs, GPU runtime if available).

Dissertation Analysis App Container (Solutions Layer)

A minimal web or API-based front-end that an instructor or user sees.

Connects to the domain container (via internal Docker network) to run the analysis pipeline on uploaded files.

Single-Machine Advantage

Everything runs on your local workstation or server using docker-compose up.

Great for demonstrations, PoCs, and small-scale usage.

Supports GPU acceleration if you have a compatible GPU. If not, everything just runs on CPU.

Agent Flows

If LangGraph is included:

The dissertation text can be orchestrated through multiple LLM calls (e.g., summarization → rubric check → feedback generation).

This pipeline is still domain-specific but leverages the shared Platform capabilities (logging, concurrency, HPC resources, etc.).



5. Future Transition to the v1.0 “Fabric” Deployment

While v0.5 is single-node, the v1.0 fabric approach will enable:

Distributed Nodes: Multiple CPU/GPU nodes—local, cloud, or hybrid—joined into a single orchestrated environment.

Regional Deployment: US-based nodes for one part of the institution, APAC-based nodes for another, all coordinating seamlessly.

Scalable Dissertation Analysis: The same domain container can auto-scale horizontally if thousands of dissertations need parallel analysis.

Cross-Domain Expansion: HRTech or SportsTech containers can join the same fabric, each specialized in different tasks but sharing the underlying Platform.



6. Final Takeaways

Today’s Repo:

A self-contained application that ingests academic papers, runs AI-based analysis, and outputs structured feedback.

Bundles domain logic, partial data/ML code, and UI or demonstration interfaces into one place.

Where Each Piece Lands:

Platform Layer: Kafka, Zookeeper, MySQL, Redis, Prometheus, Ollama (model serving), plus agent libraries (LangGraph) at a system level.

Domain Layer: EdTech’s specialized chunking, rubrics, LLM fine-tuning, and academic heuristics.

Solutions Layer: The final “Dissertation App” front-end, APIs, and integration points.

0.5 “Platformed”:

Single-node, Docker Compose–based deployment that merges these pieces into containers.

Demonstrates GPU or CPU readiness, local box usage, and how an agent workflow can orchestrate multiple LLM calls for dissertations.

Looking Ahead to 1.0:

A fabric-based architecture that scales horizontally or globally, orchestrating multi-region nodes for advanced performance, resilience, and multi-domain synergy.

By migrating the key elements of the Dissertation-Analysis repo into their rightful places in the Spanda.AI Platform, Domain, and Solutions layers, we ensure that the EdTech offering (dissertation analysis) becomes more modular, scalable, and easier to integrate with future domain expansions—ultimately delivering a robust, enterprise-ready AI ecosystem.

