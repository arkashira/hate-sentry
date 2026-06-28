# dataflow.md

```markdown
# Hate‑Sentry System Dataflow

```

## 1. External Data Sources
- **Social Media APIs**  
  - Twitter, Reddit, Discord, Facebook (public posts, comments, threads)  
  - Rate‑limited, OAuth2 authenticated
- **User‑Generated Content (UGC) Feeds**  
  - Webhooks from partner platforms (e.g., Slack, Discord bots)
- **External Knowledge Bases**  
  - Hate‑speech lexicons (e.g., Hatebase, OpenAI Toxicity dataset)  
  - Contextual embeddings (e.g., BERT, RoBERTa) from HuggingFace
- **Feedback Loops**  
  - Moderation flags from platform admins  
  - Crowd‑source labeling via internal annotation tool

## 2. Ingestion Layer
| Component | Role | Auth Boundary |
|-----------|------|---------------|
| **API Gateway** | Exposes public endpoints for UGC ingestion, rate‑limit, and authentication. | OAuth2 / API keys |
| **Message Queue (Kafka / Pulsar)** | Buffers incoming text streams, decouples producers from processors. | TLS + ACL |
| **Webhook Listener** | Receives push events from partner platforms. | HMAC signatures |
| **Data Ingestor Service** | Normalizes payloads, enriches with metadata (user ID, timestamp, source). | Service‑to‑service IAM |

## 3. Processing / Transform Layer
| Component | Role | Auth Boundary |
|-----------|------|---------------|
| **Pre‑processing Service** | Tokenization, language detection, profanity filtering, de‑duplication. | Service‑to‑service IAM |
| **Feature Extraction Engine** | Generates embeddings (e.g., Sentence‑BERT), extracts n‑grams, sentiment scores. | Service‑to‑service IAM |
| **Inference Service (Model Server)** | Runs fine‑tuned transformer model for hate‑speech classification (implicit & nuanced). | Service‑to‑service IAM |
| **Post‑processing Service** | Threshold calibration, confidence scoring, risk‑tier assignment. | Service‑to‑service IAM |
| **Anomaly Detector** | Flags outliers (e.g., sudden spikes in hate content). | Service‑to‑service IAM |
| **Audit & Logging** | Records raw inputs, model outputs, decisions for compliance. | Service‑to‑service IAM |

## 4. Storage Tier
| Component | Role | Auth Boundary |
|-----------|------|---------------|
| **Raw Data Store (S3 / GCS)** | Immutable archive of ingested text, metadata. | Bucket policies, encryption at rest |
| **Feature Store (Redis / DynamoDB)** | Fast lookup of embeddings, cached inference results. | IAM roles, encryption |
| **Model Registry (MLflow / DVC)** | Versioned models, training artifacts. | IAM roles |
| **Analytics DB (PostgreSQL / BigQuery)** | Historical analysis, reporting, ML training data. | IAM roles, column‑level encryption |
| **Audit Log Store (Elasticsearch)** | Searchable logs for compliance audits. | IAM roles, TLS |

## 5. Query / Serving Layer
| Component | Role | Auth Boundary |
|-----------|------|---------------|
| **GraphQL / REST API Gateway** | Exposes moderation verdicts, confidence scores, and metadata to downstream services. | OAuth2 / JWT |
| **Cache Layer (Redis)** | Low‑latency serving of recent moderation results. | Service‑to‑service IAM |
| **Dashboard UI** | Visualizes moderation metrics, trend analysis, and manual review queue. | OAuth2 / SSO |
| **Webhook Callback Service** | Pushes moderation decisions back to partner platforms. | HMAC signatures |

## 6. Egress to User
| Component | Role | Auth Boundary |
|-----------|------|---------------|
| **Moderation Dashboard** | Platform admins view flagged content, override decisions. | OAuth2 / SSO |
| **API Clients** | External partners consume moderation verdicts via SDKs. | API keys, OAuth2 |
| **Alerting Service (PagerDuty / Slack)** | Real‑time alerts for high‑risk content or system anomalies. | Webhook secrets |
| **Export Service** | CSV/JSON exports for compliance reporting. | OAuth2 / SSO |

---

### ASCII Block Diagram

```
+------------------------+          +------------------------+
|  External Platforms    |          |  External Knowledge    |
|  (Twitter, Reddit, …)  |          |  Bases (Lexicons, …)   |
+-----------+------------+          +-----------+------------+
            |                                 |
            v                                 v
+------------------------+          +------------------------+
|   API Gateway / Webhook|          |  Data Ingestor Service |
|   (Auth: OAuth2)       |          |  (Auth: IAM)           |
+-----------+------------+          +-----------+------------+
            |                                 |
            v                                 v
+------------------------+          +------------------------+
|   Message Queue (Kafka)|          |  Feature Extraction    |
|   (TLS, ACL)           |          |  Engine (IAM)          |
+-----------+------------+          +-----------+------------+
            |                                 |
            v                                 v
+------------------------+          +------------------------+
|  Pre‑processing Service|          |  Inference Service     |
|  (IAM)                 |          |  (IAM)                 |
+-----------+------------+          +-----------+------------+
            |                                 |
            v                                 v
+------------------------+          +------------------------+
|  Post‑processing Service|          |  Audit & Logging       |
|  (IAM)                 |          |  (IAM)                 |
+-----------+------------+          +-----------+------------+
            |                                 |
            v                                 v
+------------------------+          +------------------------+
|  Raw Data Store (S3)   |          |  Feature Store (Redis) |
|  (Encryption)          |          |  (IAM)                 |
+-----------+------------+          +-----------+------------+
            |                                 |
            v                                 v
+------------------------+          +------------------------+
|  Query/Serving Layer   |          |  Analytics DB (PostgreSQL)|
|  (OAuth2, JWT)         |          |  (IAM, Encryption)      |
+-----------+------------+          +-----------+------------+
            |                                 |
            v                                 v
+------------------------+          +------------------------+
|  Moderation Dashboard  |          |  Alerting Service      |
|  (OAuth2, SSO)         |          |  (Webhook Secrets)     |
+------------------------+          +------------------------+
```

---

### Auth Boundaries Summary
- **External ↔ Ingestion**: OAuth2 / API keys, HMAC for webhooks.
- **Ingestion ↔ Processing**: Service‑to‑service IAM, TLS, ACLs.
- **Processing ↔ Storage**: IAM roles, encryption at rest, TLS.
- **Storage ↔ Query/Serving**: IAM, column‑level encryption, TLS.
- **Query/Serving ↔ Egress**: OAuth2 / JWT, API keys, SSO, webhook secrets.

---