# Technical Specification: hate-sentry

## 1. Executive Summary
**hate-sentry** is an AI-native content moderation platform designed to identify implicit and nuanced forms of hate speech. Unlike traditional keyword-based filters, hate-sentry utilizes Large Language Model (LLM) inference with structured generation to provide high-accuracy detection, explainable reasoning, and continuous learning. The system is architected for scalability, leveraging Axentx's verified inference stack (SGLang) and vector database capabilities (pgvector) to handle large-scale content moderation workflows.

## 2. Architecture Overview
The system follows a request-response model with a feedback loop for continuous improvement.

### 2.1 High-Level Flow
1.  **Ingestion**: Content (text) is submitted via API or CLI.
2.  **Inference**: The content is sent to the **SGLang
