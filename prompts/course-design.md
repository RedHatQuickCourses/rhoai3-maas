# Red Hat AI Training: Models-as-a-Service (MaaS)

## COURSE GOAL

To equip architects, engineers, and administrators with the knowledge and skills required to design, configure, deploy, and monitor a robust Models as a Service (MaaS) solution using Red Hat OpenShift AI 3.4. By the end of this training, participants will understand the strict boundaries around serving, resource management, access, and lifecycle. You will learn to govern a scalable MaaS ecosystem, transforming isolated GPUs into a fairly scheduled, multi-tenant shared service.

## TARGET AUDIENCE

- **Platform Engineers**: Responsible for designing and deploying model serving environments, enabling MaaS, defining service tiers, and deploying governed models.
- **Developers**: Responsible for generating API keys, connecting applications to OpenAI-compatible REST endpoints, and integrating code assistants.
- **DevOps / SREs**: Focused on observability, diagnostics, operational scale, tracking consumption metrics, cost allocation, and utilizing agentic AI for cluster operations.

## PREREQUISITES

This course assumes that you have the following experience:

* Familiarity with Kubernetes and Red Hat OpenShift Container Platform.
* Basic understanding of machine learning concepts and Red Hat OpenShift AI.
* Basic networking, routing, and understanding of Kubernetes operators.
* Familiarity with REST APIs and Prometheus/Grafana observability.

## COURSE DESIGN

| Learning Objective / Section Description | Session Type |
|------------------------------------------|--------------|
| **LEARNING OBJECTIVE #1: Design a MaaS Architecture** | |
| Understand the "GPU Silo" crisis where expensive, limited hardware is trapped within individual teams, leading to simultaneous idle capacity and project bottlenecks. Explain how MaaS acts as a centralized governance layer sitting directly between users and the model serving infrastructure to maximize ROI of scarce hardware. | Presentation |
| Map the architectural layers, specifically understanding the roles of KServe, Kuadrant, and the Gateway API to establish strict serving boundaries. Introduce the "Foundational Accelerator Enablement" layer at the hardware level, detailing the NVIDIA GPU Operator, including drivers, DCGM, the container toolkit, MIG, and the manager. This section builds Diagram Layer 1: GPU Foundation. | Presentation |
| Test recall of architectural layers and components through a knowledge check quiz covering the GPU Silo problem, MaaS governance benefits, and the foundational GPU operator stack. | Quiz |
| **LEARNING OBJECTIVE #2: Configure the MaaS Platform and Ecosystem** | |
| Install Node Feature Discovery (NFD) for hybrid cloud GPU recognition. Deploy Cert-Manager, Kueue, and the Leader Worker Set to manage workload queuing across the foundational GPU layer. Understand the ecosystem operators that enable MaaS functionality. | Presentation |
| Update the `modelsAsService` component in the DataScienceCluster CR to `Managed` to trigger the instantiation of MaaS features. Configure the OdhDashboardConfig to enable Gen AI Studio and MaaS interface. Install and enable the Llama Stack Operator required for dashboard access. | Lab |
| Integrate with Kubernetes RBAC to map users to service tiers based on OpenShift groups. Configure access controls and rate limits for default tiers like Level 0 (Free), Level 1 (Premium), and Level 2 (Enterprise). Demonstrate how to create custom service tiers beyond the defaults. | Lab |
| **LEARNING OBJECTIVE #3: Formulate a Model Selection Strategy** | |
| Evaluate models based on architecture: understand the architectural differences and performance impacts of choosing dense versus sparse models. Learn how model architecture affects GPU memory consumption and inference speed. | Presentation |
| Assess models based on capabilities: evaluate models based on reasoning vs. non-reasoning capabilities. Understand how different capability profiles impact workload suitability and computational requirements. | Presentation |
| Test understanding of model selection criteria through a knowledge check covering context length limitations, tool calling considerations, and choosing the right model type for specific workloads. | Quiz |
| **LEARNING OBJECTIVE #4: Deploy Models for MaaS** | |
| Understand why llm-d is required for MaaS deployments to optimize disaggregated serving and prefix cache aware routing. Review how vLLM manages GPU memory via page attention. This section builds Diagram Layer 2: LLM Inference (llm-d + vLLM on top of GPU foundation). | Presentation |
| Deploy a model (using the NVIDIA-Nemotron-3-Nano-4B-FP8 as the standard lab model) transitioning from single-node vLLM to the distributed llm-d runtime. Select "Publish as MaaS endpoint" to expose the model. Learn how to properly instrument model endpoints and enforce access controls. | Lab |
| Manage model versions and integrate the model serving deployment into existing GitOps pipelines. Demonstrate version management, rollback capabilities, and declarative deployment patterns for production environments. | Lab |
| **LEARNING OBJECTIVE #5: Track Metrics and Usage** | |
| Understand how OpenShift AI exposes centralized metrics via User Workload Monitoring. This section builds Diagram Layer 3: Observability - connect the NVIDIA DCGM Exporter to the platform metrics to demonstrate how hardware data flows up to the observability plane. Discuss how this data enables cross-department chargeback models. | Presentation |
| Access Grafana to build queries tracking rate limit hits, token counts, request success, and latency-focused metrics like Time To First Token (TTFT) and Time Per Output Token (TPOT). Create custom dashboards for monitoring MaaS performance and consumption patterns. | Lab |
| **LEARNING OBJECTIVE #6: Consume MaaS Endpoints and Agentic Integrations** | |
| Navigate the "Models as a service" tab in the OpenShift AI dashboard to generate single-view API keys. Explain the OpenAI-compatible API and standardized error codes like 401 (unauthorized), 403 (forbidden), and 429 (rate limited). Understand the developer experience for consuming MaaS endpoints. | Presentation |
| Use curl and Python to interact with the `/v1/chat/completions` endpoint using Bearer token authentication. Read response headers to identify remaining tokens and experiment with setting max tokens. Test API key authentication and rate limiting behavior. | Lab |
| Connect the published Nemotron model to OpenShift DevSpaces acting as an IDE with a code assistant, utilizing standard REST API calls and the generated MaaS endpoint. This section completes the Final Capstone Diagram showing the full MaaS ecosystem with developer integration. | Lab |
