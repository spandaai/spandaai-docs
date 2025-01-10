# Spanda Platform Demo

## Overview: Spanda.AI Platform & Architecture

### Three-Layer Stack in v0.5
- **Platform Layer**
  - Core Services
    - Kafka & Zookeeper for messaging and coordination
    - MySQL or other relational DB for structured data storage
    - Redis for caching and real-time data processing
    - Prometheus for system monitoring and metrics collection
    - Ollama for foundational model serving
  - Agent Frameworks (New in v0.5)
    - LangGraph to enable agent-based orchestration across different LLMs.

- **Domain Layer**
  - EdTech Subdomain
    - Dissertation Analysis: Fine-tuned foundation models for academic text analysis.
  - HRTech / SportsTech (Planned Expansions)

- **App Layer**
  - Dissertation Analysis App: User-facing application combining EdTech domain models and AI services.

## v0.5 Single-Node Demo Highlights
- **Deployment:** Single-node on Mac/PC, CPU/GPU.
- **Functionality:** Dissertation analysis with plagiarism checks, rubric alignment, summaries, and recommendations.
- **Agent-Based Workflows:** Integrates LangGraph for LLM coordination.

## Roadmap to v1.0: The Spanda Fabric
- **Fabric Concept:** Multi-node, globally distributed architecture.
- **Benefits:** High availability, resource optimization, unified experience.

