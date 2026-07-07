# Azure AIOps Bootcamp — 10-Day Syllabus
**Format:** 2 hours/day | **Style:** Hands-on lab-first, minimal theory (10–15 min max per day) | **Audience:** Intermediate cloud/DevOps engineers

> Curated from the 16-module Azure Cloud + AIOps + ML source curriculum. Selected and re-sequenced into a tight 10-day arc: setup → identity/network → compute/storage AIOps → observability/self-healing → IaC → cost intelligence → security hardening. An optional capstone module follows separately for those who want to integrate everything into one workload.

---

## Day 1 — Cloud Foundations & Your First AIOps Pipeline
**Theory (10 min):** ARM as the control plane · shared responsibility · the AIOps loop (Observe → Detect → Decide → Act)

**Hands-on Lab (110 min):**
- Set up Azure free account, resource group, budget/cost alert
- Install Azure CLI, Python 3.11, Ollama; pull Mistral 7B locally
- Write a Python script: Azure CLI output → local LLM → plain-English resource summary

**Outcome:** A working zero-cost AI-assisted CLI summarizer.

---

## Day 2 — Identity, RBAC & Access Anomaly Detection
**Theory (10 min):** RBAC scope inheritance · Managed Identity vs Service Principal

**Hands-on Lab (110 min):**
- Create a least-privilege custom RBAC role; assign to a Service Principal
- Configure Managed Identity on a Function to read Key Vault secrets
- Export Entra sign-in logs via Graph API → pandas
- Run Isolation Forest (scikit-learn) to flag anomalous logins

**Outcome:** A Python pipeline that flags suspicious sign-ins with anomaly scores.

---

## Day 3 — Networking & ML-Based Traffic Anomaly Detection
**Theory (10 min):** Hub-spoke topology · NSG rule evaluation order · Private Endpoint vs Service Endpoint

**Hands-on Lab (110 min):**
- Build a hub-spoke VNet with 2 spokes + peering, tiered NSG rules
- Deploy storage with Private Endpoint; verify public access is blocked
- Enable NSG Flow Logs → parse in Python → train Isolation Forest to flag unusual traffic

**Outcome:** A flow-log anomaly detector catching lateral movement/port-scan patterns.

---

## Day 4 — Compute & Predictive Autoscaling
**Theory (10 min):** Reactive vs predictive scaling · VMSS vs App Service vs Container Apps decision framework

**Hands-on Lab (110 min):**
- Deploy VMSS with baseline CPU-based reactive autoscale
- Deploy a containerized workload to Container Apps with HTTP scaling
- Export 30 days of CPU metrics (azure-monitor-query); fit an ARIMA model
- Use the forecast to pre-emptively adjust VMSS instance count via CLI

**Outcome:** A forecast-driven autoscaler that beats reactive scaling lag.

---

## Day 5 — Storage Lifecycle Intelligence
**Theory (10 min):** Hot/Cool/Cold/Archive trade-offs · lifecycle policies

**Hands-on Lab (110 min):**
- Create storage account with hot/warm/archive containers; simulate access patterns
- Export blob access logs → extract features (last accessed, frequency, size)
- Train a Decision Tree classifier to predict optimal tier
- Apply tier changes via Storage SDK; deploy Blob static site + CDN, measure latency

**Outcome:** A data-driven storage tier optimizer replacing manual lifecycle rules.

---

## Day 6 — Observability & Self-Healing Monitoring
**Theory (10 min):** Metrics vs Logs vs Traces · dynamic thresholds and their limits

**Hands-on Lab (110 min):**
- Enable Application Insights + Log Analytics workspace on a deployed app
- Write 3 KQL queries: failed requests, p95 latency, error spikes
- Export CPU/memory metrics → Isolation Forest running continuously
- On anomaly: publish to Event Grid → Automation Runbook auto-restarts the service

**Outcome:** A complete self-healing loop: detect → event → auto-remediate.

---

## Day 7 — AI Incident Triage Agent
**Theory (10 min):** Queue vs Topic vs Event Stream · the agentic Observe→Reason→Act pattern

**Hands-on Lab (110 min):**
- Wire Azure Monitor alert → Event Grid → Function → Service Bus queue
- A second Function dequeues and calls local Ollama (Llama 3) with a structured prompt
- LLM returns IGNORE / INVESTIGATE / ESCALATE with reasoning
- Route decision to priority/dead-letter queue; expose via API Management with rate limiting

**Outcome:** A working LLM-driven alert triage agent — zero paid AI APIs.

---

## Day 8 — AI-Assisted Infrastructure as Code
**Theory (10 min):** Declarative IaC value · policy-as-code concept

**Hands-on Lab (110 min):**
- Write a Bicep module for a 3-tier workload (VNet + App Service + Storage + Key Vault)
- Build a GitHub Actions pipeline: validate → plan → security-scan (Checkov) → deploy
- Run Ollama with CodeLlama locally to scaffold a Terraform module from a description
- Pass generated Terraform through Checkov; have the LLM explain flagged issues

**Outcome:** A CI/CD pipeline where AI drafts and reviews infrastructure code.

---

## Day 9 — FinOps & AI Cost Intelligence
**Theory (10 min):** Reserved Instances vs Savings Plans vs Spot · FinOps maturity (Crawl/Walk/Run)

**Hands-on Lab (110 min):**
- Export daily cost data via Python; engineer features (day-of-week, resource type, tag score)
- Train a Random Forest classifier: normal / overspend / idle-resource / tier-mismatch
- Use local Ollama to generate a two-sentence explanation per anomaly
- Implement autoscale schedule rules to cut instance count outside business hours; compare costs

**Outcome:** A daily cost-intelligence digest with plain-English anomaly explanations.

---

## Day 10 — AI Security Hardening Advisor
**Theory (10 min):** Key Vault object types (secrets/keys/certs) · Defender for Cloud Secure Score · encryption at rest/in transit basics

**Hands-on Lab (110 min):**
- Integrate Key Vault with an App Service via Managed Identity — no secrets in code/config
- Enable Defender for Cloud; run a baseline assessment on your resource group
- Export security recommendations JSON via Python (azure-mgmt-security SDK)
- Use a sentence-transformer model to embed and cluster similar findings
- Use local Ollama to generate a plain-English remediation summary per cluster
- Restrict a storage account with Private Endpoint + Firewall; re-run assessment and compare Secure Score before/after

**Outcome:** A pipeline that turns a raw 40-item security findings list into a 5-item prioritized action plan.

---

---

## Optional Capstone Module (Post-Bootcamp, not counted in the 10 days)
**Suggested duration:** one extended 3–4 hour session, or split across 2 evenings

### Capstone — Intelligent Cloud Workload
**Goal:** Tie the AIOps loop (RAG + agentic decisioning) across a full stack into one deployable workload.

- Deploy a scaled-down **Intelligent Order Processing Platform** via IaC only:
  - Container Apps API (FastAPI) + Blob-hosted static frontend
  - Service Bus for order events, Key Vault via Managed Identity, Private Endpoints
  - Isolation Forest on order metrics → anomaly event → triage agent (reuse the Day 7 agent)
- Run the triage agent against 3 simulated alert scenarios and validate responses
- Present the architecture: services chosen, 2 key tradeoffs justified, cost estimate, top 3 failure modes

**Outcome:** A working end-to-end intelligent workload connecting identity, network, compute, storage, observability, and an AI decision layer — presented with architect-level tradeoff reasoning.

---

## Stack Used (all free-tier / local, no paid AI APIs)
Azure CLI · Python 3.11 · azure-mgmt-* / azure-monitor-query SDKs · pandas · scikit-learn (Isolation Forest, Random Forest, Decision Tree) · statsmodels (ARIMA) · Ollama (Llama 3, Mistral 7B, CodeLlama) · Bicep/Terraform · GitHub Actions · Checkov

**Estimated total lab cost per student:** under $15 across all 10 days.

## Topics Deliberately Left Out (available as follow-on modules)
Governance/Policy-as-code compliance NLP, Disaster Recovery + Chaos Studio failure prediction, full RAG/vector-search runbook hub, and dedicated architect interview-prep whiteboarding — trimmed to fit the 10-day / 2-hr format without diluting hands-on time.
