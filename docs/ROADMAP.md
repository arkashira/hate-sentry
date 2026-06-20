# ROADMAP.md – hate‑sentry

> **Goal:** Deliver a production‑ready AI‑driven hate‑speech detection service that can be embedded in SaaS platforms, social apps, and enterprise communication tools.  
> **Target launch:** Q4 2026 (MVP).  
> **Success metric for launch:** ≥ 92 % F1‑score on a validated multi‑domain hate‑speech benchmark *and* a self‑service API that can process **≥ 10 k requests/min** with < 200 ms latency (99 th percentile).

---

## 📌 MVP – “Launch‑Ready Core”

| # | Milestone | Description | Owner | Due | **MVP‑Critical** |
|---|-----------|-------------|-------|-----|-------------------|
| 1 | **Data Pipeline & Baseline Corpus** | • Ingest the 22 M `auto` pairs + 7 M `instr‑resp` pairs. <br>• Curate a balanced hate‑speech benchmark (5 k labeled examples across 10 domains). | Data Lead | 2026‑07‑15 | ✅ |
| 2 | **Model Selection & Training** | • Fine‑tune a vLLM‑compatible LLM (e.g., Llama‑3‑8B) on the curated dataset. <br>• Implement prompt‑engineering for implicit hate detection. | ML Engineer | 2026‑08‑01 | ✅ |
| 3 | **API Service (REST + gRPC)** | • Deploy model behind vLLM inference server. <br>• Expose `/detect` endpoint (text → `{label, confidence, rationale}`). <br>• Rate‑limit & auth (API‑key). | Backend Lead | 2026‑08‑20 | ✅ |
| 4 | **Explainability Layer** | • Use SGLang structured generation to return token‑level attribution (highlighted spans). | ML Engineer | 2026‑08‑28 | ✅ |
| 5 | **CI/CD & Automated Tests** | • Unit, integration, and performance tests (≥ 95 % pass). <br>• GitHub Actions pipeline that builds Docker image and runs load test (10 k rps). | DevOps | 2026‑09‑05 | ✅ |
| 6 | **Documentation & SDKs** | • Markdown API docs + OpenAPI spec. <br>• Python & JavaScript client libraries (pip & npm). | Docs/SDK Lead | 2026‑09‑12 | ✅ |
| 7 | **Beta Release & Validation** | • Private beta with 3 anchor customers (social app, forum, enterprise chat). <br>• Collect false‑positive/negative logs, iterate on thresholds. | PM | 2026‑09‑30 | ✅ |
| 8 | **Production Hardening** | • Autoscaling on Kubernetes (horizontal pod autoscaler). <br>• Monitoring (Prometheus + Grafana) + alerting for latency & error‑rate. | Site Reliability Engineer | 2026‑10‑10 | ✅ |
| 9 | **Go‑Live** | Public launch page, pricing tier (free‑tier 5 k/month, paid tier unlimited). | PM/Marketing | 2026‑10‑15 | ✅ |

> **MVP‑Critical** items are bolded in the table. All must be completed before the public launch.

---

## 🚀 Version 1 – “Enterprise‑Ready Expansion”

| Theme | Target Quarter | Key Features |
|-------|----------------|--------------|
| **A. Multi‑Modal Support** | Q1 2027 | • Add image‑based hate‑speech detection (OCR + text model). <br>• Video frame sampling pipeline (audio‑transcript → text detection). |
| **B. Policy Engine & Rules** | Q1 2027 | • Rule‑based overrides (e.g., whitelist/blacklist). <br>• Policy templates per industry (gaming, finance, education). |
| **C. Bulk & Streaming APIs** | Q2 2027 | • `/batch-detect` (up to 10 k messages per request). <br>• WebSocket streaming endpoint for real‑time moderation. |
| **D. Model Registry & A/B Testing** | Q2 2027 | • Ability to roll out newer fine‑tuned checkpoints without downtime. <br>• Built‑in A/B test dashboard for customers. |
| **E. Compliance & Data Residency** | Q3 2027 | • Deployable on‑prem Docker image. <br>• EU/US/APAC data‑region clusters (GDPR‑ready). |

*All V1 features are **shippable** within a single sprint cycle (2 weeks) once the underlying infra is in place.*

---

## 🚀 Version 2 – “Platform & Ecosystem”

| Theme | Target Quarter | Key Features |
|-------|----------------|--------------|
| **F. Community Model Marketplace** | Q4 2027 | • Allow customers to upload custom fine‑tuned models. <br>• Marketplace UI for rating & sharing. |
| **G. Active Learning Loop** | Q4 2027 | • UI for moderators to label false positives/negatives. <br>• Automated nightly re‑training using collected feedback. |
| **H. Federated Inference** | Q1 2028 | • Edge‑device inference (WebAssembly) for latency‑critical apps. |
| **I. Integration Hub** | Q1 2028 | • Pre‑built connectors for Slack, Discord, Microsoft Teams, and major CMS platforms. |
| **J. SLA & Enterprise Dashboard** | Q2 2028 | • Customizable dashboards (throughput, latency, abuse trends). <br>• 99.9 % uptime SLA with dedicated support. |

---

## 📅 Milestone Timeline (High‑Level)

| Date | Milestone |
|------|-----------|
| **2026‑07‑15** | Data pipeline ready |
| **2026‑08‑01** | Base model fine‑tuned |
| **2026‑09‑15** | Beta customers onboard |
| **2026‑10‑15** | Public MVP launch |
| **2027‑01‑30** | Multi‑modal & policy engine GA |
| **2027‑04‑15** | Bulk & streaming APIs GA |
| **2027‑07‑01** | Compliance & on‑prem release |
| **2027‑12‑01** | Model marketplace & active learning beta |
| **2028‑03‑15** | Federated inference & integration hub GA |
| **2028‑06‑01** | Enterprise SLA dashboard launch |

---

## 📌 How We Track Progress

| Tool | Purpose |
|------|---------|
| **GitHub Projects (Roadmap view)** | Visual sprint planning, issue linking to milestones |
| **OKR Dashboard** | Quarterly objectives (e.g., “Achieve 92 % F1 on benchmark”) |
| **Prometheus + Grafana** | Real‑time service health, latency, error‑rate |
| **Customer Success Metrics** | Adoption rate, churn, support tickets per 1 k requests |
| **Post‑Launch Review** | 2‑week retrospective after each major release |

---

*Prepared by the hate‑sentry product & engineering leadership team.*
