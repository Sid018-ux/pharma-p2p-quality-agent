# Autonomous Pharma P2P Quality & Vendor CoA Ingestion Agent

An enterprise-grade document intelligence and ERP reconciliation engine built with **Gemini Multimodal APIs**, **Google AI Studio**, and **BigQuery/Stitch data pipelines**. Designed for pharmaceutical Procure-to-Pay (P2P) receiving docks to eliminate manual Certificate of Analysis (CoA) inspection bottlenecks.

---

## System Architecture

```text
[Vendor Shipment: Scanned CoA PDF / Challan]
                   │
                   ▼
       [Google AI Studio / Vertex AI]
          Gemini Multimodal Agent
   (Extracts: Purity %, Moisture %, pH, Batch #)
                   │
                   ▼ (Strict Structured JSON)
      [Enterprise Integration Layer]
                   │
         ┌─────────┴─────────┐
         ▼                   ▼
[Stitch Data Pipeline]   [SAP S/4HANA OData Mock]
Replicates ERP PO Specs  Reconciles Extracted Values
into BigQuery Warehouses Against Contract Release Limits
         │                   │
         └─────────┬─────────┘
                   ▼
     [Deterministic Decision Gate]
         ├── PASS ──> SAP BAPI: Post Goods Receipt (MIGO 101)
         └── FAIL ──> SAP BAPI: Material Movement 322 (Quarantine Hold)
                   │
                   ▼
     [BigQuery Audit Log & Power BI Control Tower]
     (Tracks OTIF, Batch Hold Latency, and QA Compliance)
