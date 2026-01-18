# Event-Driven Streaming Data Pipeline (Databricks Lakehouse)

## Overview
This project demonstrates the design and implementation of an end-to-end **event-driven data engineering pipeline** on Databricks. The pipeline ingests incremental data from a public external API and processes it using a **Bronze–Silver–Gold Lakehouse architecture**, focusing on correctness, data quality, and analytics readiness rather than raw scale.

The project is designed to mirror **production Databricks patterns under serverless and resource-constrained environments**.

---

## Problem Statement
External data sources often expose:
- Incremental and unpredictable data arrival
- Non-standard schemas and timestamp formats
- Schema evolution over time
- Late-arriving and duplicate events

This project addresses these challenges by building a resilient ingestion and transformation pipeline that ensures **exactly-once processing**, **data quality enforcement**, and **safe reprocessing/backfills**.

---

## Dataset
The pipeline ingests incremental Near-Earth Object (NEO) close-approach data from a **public NASA API**, exposed as JSON responses and processed as event-level records.

---

## Architecture Overview

**High-level flow:**

External API  
→ Event File Generation  
→ Databricks Autoloader (Bronze)  
→ Data Cleaning & Deduplication (Silver)  
→ Analytics-Ready Modeling (Gold)  
→ Power BI Consumption

The pipeline follows the Medallion Architecture pattern:
- **Bronze**: Raw, immutable events
- **Silver**: Trusted, deduplicated, validated data
- **Gold**: Enriched, BI-ready fact table

---

## Tech Stack

| Layer | Tools |
|-----|------|
| Ingestion | Databricks Autoloader |
| Storage | Delta Lake |
| Processing | Apache Spark (PySpark) |
| Governance | Unity Catalog, Volumes |
| Streaming | Spark Structured Streaming |
| Analytics | Power BI |

---

## Pipeline Implementation

### 1. Bronze Layer – Streaming Ingestion
- Converted batch API responses into incremental event files to simulate streaming behavior.
- Ingested events using **Databricks Autoloader** with checkpointing and schema tracking.
- Enabled schema evolution awareness to safely handle future non-breaking schema changes.
- Stored raw events as an immutable Delta table to support replay and backfills.

---

### 2. Silver Layer – Data Quality & Trust
- Standardized non-ISO timestamp formats and enforced data type consistency.
- Removed invalid records and deduplicated events using business keys.
- Implemented a **lightweight data quality framework** to track dropped and duplicate records per run.
- Designed the Silver layer to support partial and full backfills.

---

### 3. Gold Layer – Analytics-Ready Modeling
- Built a Gold event fact table enriched with:
  - Surrogate keys
  - Temporal dimensions (date, year, month)
  - Deterministic business classifications (risk, size, velocity bands)
- Partitioned Gold data using time-based columns to optimize BI query performance.
- Implemented incremental upsert logic to support scalable refreshes.

---

## Visualization
A lightweight Power BI dashboard was built on top of the Gold table to validate analytics readiness and query performance.

---

## Key Learnings
- Designing streaming pipelines under serverless constraints
- Applying exactly-once ingestion semantics with Autoloader
- Enforcing data quality without heavy frameworks
- Modeling Gold tables for BI consumption
- Supporting reprocessing and late-arriving data

---

## Future Improvements
- Add automated data quality alerts
- Introduce job orchestration
- Extend the pipeline to additional external data sources
