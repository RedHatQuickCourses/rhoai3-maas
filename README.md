# Models as a Service (MaaS) on Red Hat OpenShift AI 3.x

**From Ticket-Based Blockers to API-First Utility**

> **The Problem:** Data scientists blocked by IT tickets; expensive GPUs sitting idle; "Shadow AI" creating security and compliance risk.  
> **The Solution:** A centralized MaaS platform using vLLM (inference), Gateway API (routing), and Kuadrant (governance) to share resources dynamically and secure endpoints.

This repository contains a complete **course-in-a-box** for **Models as a Service (MaaS)** on Red Hat OpenShift AI 3.x. It guides learners from business value and strategy through taxonomy, architecture, hands-on deployment, and troubleshooting.

---

## Prerequisites

* **Cluster:** OpenShift 4.x with Red Hat OpenShift AI 3.x installed (operator channel `fast-3.x` or equivalent)
* **Access:** Permissions to install/configure the OpenShift AI Operator, KServe, and Dashboard (e.g., cluster-admin for operator install)
* **CLI:** `oc` installed and authenticated; `curl` for testing the inference API
* **Hardware:** For production-style labs, at least one GPU node (NVIDIA) and the NVIDIA GPU Operator

---

## Quick Start: Deploy a MaaS-Style Endpoint

### Step 1: Install OpenShift AI Operator

From the OpenShift Console (**Administrator** → **Operators** → **OperatorHub**):

1. Search for **Red Hat OpenShift AI** (or **OpenShift AI**).
2. Install with **Channel:** `fast-3.x`.
3. Wait for the operator to show **Succeeded**.

### Step 2: Enable KServe and Dashboard

Ensure the Data Science Cluster (or equivalent) has **KServe** and **Dashboard** enabled so you can create InferenceServices and use the UI.

### Step 3: Create Project and LLMInferenceService

```bash
oc new-project my-maas-lab
```

Create an `LLMInferenceService` (save as `llm-inference-service.yaml`):

```yaml
apiVersion: serving.kserve.io/v1alpha1
kind: LLMInferenceService
metadata:
  name: maas-demo
  namespace: my-maas-lab
  annotations:
    opendatahub.io/model-type: generative
spec:
  replicas: 1
  model:
    uri: "hf://Qwen/Qwen3-0.6B"
    name: "Qwen3-0.6B"
  template:
    spec:
      containers:
        - name: kserve-container
          resources:
            limits:
              nvidia.com/gpu: "1"
            requests:
              nvidia.com/gpu: "1"
          env:
            - name: VLLM_ADDITIONAL_ARGS
              value: "--max-model-len=4096"
```

```bash
oc apply -f llm-inference-service.yaml
oc get llminferenceservice -n my-maas-lab -w
```

### Step 4: Test with curl

Get the inference URL from the Dashboard (**Model Serving** → your service) or:

```bash
oc get inferenceservice maas-demo -n my-maas-lab -o jsonpath='{.status.url}'
```

Then:

```bash
export INFERENCE_URL="<your-url>"
curl -k -X POST "$INFERENCE_URL/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -d '{"model":"Qwen3-0.6B","messages":[{"role":"user","content":"Say hello."}],"max_tokens":50}'
```

---

## Course Structure (Antora)

| Part | Content |
|------|--------|
| **Introduction & Value** | Problem (tickets, idle GPUs, Shadow AI); MaaS definition; three pillars (Efficiency, Security, Simplicity) |
| **Strategy Guide** | Path A (single model) vs Path B (MaaS with vLLM + llm-d); when to use which |
| **Taxonomy** | MaaS, vLLM, Gateway API, Kuadrant, llm-d, KEDA, TTFT |
| **Architecture Deep Dive** | Stack (hardware → vLLM → Gateway → auth); scaling (KEDA); observability (TTFT) |
| **Hands-On Lab** | Install operator, enable KServe/Dashboard, deploy LLMInferenceService, test with curl |
| **Troubleshooting** | Pending pods, model load failures, route/gateway issues, TTFT, rate limiting |
| **Quiz** | Knowledge check (4 questions) |

---

## Build the Full Course

**Docker:**

```bash
docker run -u $(id -u) -v $PWD:/antora:Z --rm -t antora/antora antora-playbook.yml
# open build/site/index.html
```

**NPM:**

```bash
npm install
npx antora antora-playbook.yml
# open build/site/index.html
```

---

## Repository Structure

```
/
├── modules/
│   ├── ROOT/pages/index.adoc      # Home (includes chapter1)
│   └── chapter1/pages/
│       ├── index.adoc             # Foundations (Introduction & Value)
│       ├── section1.adoc          # Strategy (Well-Lit Paths)
│       ├── section2.adoc          # Taxonomy
│       ├── section3.adoc          # Architecture Deep Dive
│       ├── section4.adoc          # Hands-On Lab
│       ├── section5.adoc          # Troubleshooting
│       └── quiz.adoc              # Knowledge Check
├── antora.yml
├── antora-playbook.yml
└── README.md
```

---

## Additional Resources

* **Red Hat OpenShift AI:** [Red Hat Documentation](https://access.redhat.com/documentation/en-us/red_hat_openshift_ai/)
* **KServe / LLMInferenceService:** See OpenShift AI 3.x documentation for current CRD and fields.
