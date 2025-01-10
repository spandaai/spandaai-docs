Below is a detailed writeup highlighting the Spanda.AI Platform (including recent component additions and planned agent framework integrations) and how our 0.5 demo will showcase a single-node deployment of the EdTech domain’s Dissertation Analysis application. We also include a look ahead to v1.0, where we introduce a multi-node “fabric” architecture spanning hybrid/local/cloud deployments across regions like the US and APAC.



1. Overview: Spanda.AI Platform & Architecture

1.1 Three-Layer Stack in v0.5

Platform Layer

Core Services

Kafka & Zookeeper for messaging and coordination

MySQL or other relational DB for structured data storage

Redis for caching and real-time data processing

Prometheus (and DockerProm suite) for system monitoring and metrics collection

Ollama (or additional model serving runtimes) for foundational model serving

Agent Frameworks (New in v0.5)

LangGraph (planned integration) to enable agent-based orchestration across different LLMs, plugging in domain logic and chaining advanced prompts.

In the v0.5 release, the Platform Layer is designed to be deployable on a single node (Mac or PC, CPU or GPU) using docker-compose.yml, while still reflecting the production-grade microservice approach.

Domain Layer

EdTech Subdomain

Dissertation Analysis: Fine-tuned foundation models (e.g., GPT-based, Ollama’s local LLM, or Hugging Face Transformers) specifically targeting academic text analysis, feedback generation, and rubric alignment.

HRTech / SportsTech (Planned Expansions)

Already present as folders in the domain layer “starter kit,” but not the focus of the 0.5 demo.

Why Domain Layer Matters

Encapsulates domain-specific logic, enabling new verticals to be added without rearchitecting the Platform Layer.

App Layer

Dissertation Analysis App

This is the user-facing application that ties together EdTech domain models, the platform’s AI services, and a front-end for instructors/students.

Showcases how Spanda.AI can streamline dissertation feedback loops, automate rubric-based grading suggestions, and provide advanced text summarization features.



2. v0.5 Single-Node Demo Highlights

2.1 Single-Node Deployment (CPU or GPU)

Local Box / Minimal Footprint

Everything—Kafka, MySQL, Redis, the agent framework (LangGraph), and the tuned Dissertation Analysis model—runs in Docker containers on a single Mac or PC.

This design demonstrates quick PoC capability and low-friction setup, ideal for pilot evaluations or on-prem data governance.

GPU or CPU Flexibility

If a developer or data scientist has a GPU-enabled workstation, they can get accelerated inference for large foundation models.

Alternatively, a CPU-only environment is automatically supported (albeit with lower throughput or slightly increased latencies).

2.2 Domain-Specific Functionality: Dissertation Analysis

Fine-Tuned Models

The EdTech domain includes custom-trained or fine-tuned LLMs specialized in academic text analysis.

Key Features

Plagiarism/Similarity Checks: Tagging repeated sections or references.

Rubric Alignment: Mapping paragraphs to grading categories.

Summaries & Recommendations: Streamlined insights for instructors and TAs.

2.3 Agent-Based Workflows (LangGraph Integration)

LangGraph

In 0.5, we integrate a lightweight agent layer that can coordinate multiple LLM calls or chain them in a pipeline.

Example: The Dissertation Analysis App can use LangGraph to break down student essays, feed them into the domain model, and orchestrate a summarization + rubric evaluation workflow.



Future Extensibility

The same approach will support other domain flows (e.g., HRTech for resume screening, SportsTech for analytics).



3. Roadmap to v1.0 and the Spanda Fabric

3.1 The Fabric Concept

From Single Node to Distributed

v1.0 will introduce a “fabric” approach, where each node (CPU or GPU, on-prem or cloud) can join a global Spanda Fabric.

This allows dynamic scaling—some nodes might be GPU-heavy for model training, while others are CPU-optimized for inference or data ingestion.

Hybrid & multi-Region

We’ll support hybrid deployments (mix of on-prem and cloud) and distributed clusters (e.g., US-based nodes + APAC-based nodes).

The platform automatically coordinates messaging (via Kafka/Zookeeper), data (via MySQL/Redis replication or other distributed DB options), and model serving (through orchestrators like KServe or Ollama replicas).

3.2 Fabric Benefits

High Availability & Lower Latency

Users in APAC can connect to local nodes for real-time tasks, while US nodes can handle heavier training jobs.

Resource Optimization

Deploy GPU-heavy tasks where GPU nodes exist, while offloading lighter workloads to CPU nodes—cost-effective and scalable.

Unified App Experience

The top-level application (e.g., Dissertation App) remains coherent and accessible, but under the hood, it is orchestrating multiple nodes in a single “fabric” environment.



4. Putting It All Together

4.1 Value Proposition in v0.5

Immediate Hands-On: Show that the entire stack—Platform + Domain + Dissertation App—can run on a single machine.

Agent Framework: Demonstrate how LangGraph can orchestrate advanced text analysis and summarization across different LLMs.

EdTech ROI: Highlight instant gains in Dissertation Analysis—shorter grading cycles, deeper feedback, better student outcomes.

4.2 Future-Proof with v1.0 Fabric

Multi-Node Scalability: On-prem, cloud, or hybrid architecture for enterprise-level deployments.

Geographically Distributed: US and APAC (or other regional) nodes to minimize latency and comply with data localization laws.

Expanded Domains: The same platform composition approach will unlock new applications in HRTech (e.g., candidate screening) and SportsTech (e.g., performance analytics).



5. Demo Call-to-Action

Request a 0.5 Demo: Spin up containers locally and see the Dissertation Analysis use case end-to-end.

Discuss Roadmap: Learn how we’ll evolve to a fabric-based, multi-node architecture in 1.0 for large-scale or global use cases.

Extend Domain Layer: Explore how new domain logic or fine-tuned models (HRTech, SportsTech, etc.) integrate seamlessly under the same platform.



Final Note

With v0.5, Spanda.AI underscores the agility of a single-node deployment—simple, CPU/GPU-friendly, locally or in the cloud—while paving the way for v1.0’s fabric model to handle enterprise-scale, globally distributed AI workloads. By integrating the LangGraph agent framework and domain-tuned models, we deliver both immediate business impact (in EdTech’s dissertation analysis) and a clear path to multi-domain, multi-node expansion in the near future.



