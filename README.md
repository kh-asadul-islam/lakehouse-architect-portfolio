# Lakehouse Architect Portfolio

A public, end-to-end portfolio of data engineering, dimensional modelling, data architecture and generative AI work on the Databricks Lakehouse, built entirely on **open public datasets**.

**Author:** Asadul Islam, Principal Data Modeller, Brisbane, Australia
**Focus:** Data Architect and AI roles
**Status:** In progress (started October 2026). The status table below shows what is complete.

---

## Purpose

This repository demonstrates, with working artefacts rather than claims, how I design and build modern data platforms:

- engineering a governed medallion pipeline on Databricks (Delta Lake, Unity Catalog, Lakeflow Declarative Pipelines);
- applying Kimball dimensional modelling (conformed dimensions, SCD Type 2, a governed semantic layer);
- documenting architecture the way an architect does (C4 diagrams, non-functional requirements, Architecture Decision Records, cost model, migration roadmap);
- building a retrieval-augmented generation (RAG) and Genie-based analytics application with evaluation and AI governance documentation.

## Ground rules

- **Public data only.** Every dataset is public and used under a verified open licence, recorded in [`DATASETS.md`](DATASETS.md) with its source URL and licence.
- **No employer material.** No code, schemas, data, names or designs from any current or former employer appear in this repository.
- **Reproducible.** Each project has a README explaining how to run it in a Databricks Free Edition workspace.
- **Decisions are written down.** Significant design choices are recorded as ADRs in `docs/adr/`.

## Projects

| # | Project | What it shows | Target date | Status |
|---|---------|---------------|-------------|--------|
| M1 | [Medallion pipeline](m1-medallion-pipeline/) | Auto Loader ingestion, Bronze/Silver/Gold, data-quality expectations, quarantine, scheduled job | 25 Oct 2026 | Planned |
| M2 | [Dimensional model and semantic layer](m2-dimensional-model/) | Kimball star schema, SCD Type 2, fact table at a declared grain, tests, metric view, Genie space | 8 Nov 2026 | Planned |
| M3 | [Reference architecture](m3-reference-architecture/) | C4 diagrams, NFRs, ADRs, governance and privacy, security, Fabric alternative, cost model, migration roadmap | 20 Nov 2026 | Planned |
| M4 | [RAG and Genie application](m4-genai-rag/) | Vector Search, retrieval chain/agent, evaluation set, guardrails, model card, AI risk assessment | 29 Nov 2026 | Planned |

## Repository structure

```
lakehouse-architect-portfolio/
├── README.md
├── LICENSE
├── DATASETS.md                 # source, URL and licence for every dataset
├── docs/
│   └── adr/                    # Architecture Decision Records (ADR-001 ...)
├── m1-medallion-pipeline/
│   ├── README.md
│   ├── notebooks/
│   ├── pipelines/
│   ├── docs/                   # diagrams, screenshots
│   └── tests/
├── m2-dimensional-model/
├── m3-reference-architecture/
└── m4-genai-rag/
```

## Technology

Databricks Free Edition (serverless), Delta Lake, Unity Catalog, Lakeflow Declarative Pipelines and Jobs, Auto Loader, PySpark and SQL, MLflow, Mosaic AI Vector Search, Genie, GitHub. Diagrams are drawn in draw.io or Mermaid.

## How to use this repository

1. Create a free [Databricks Free Edition](https://www.databricks.com/learn/free-edition) workspace.
2. Add this repository as a Git folder (Workspace, Create, Git folder).
3. Open the README of the project you want to run and follow its steps.

Free Edition limits (serverless compute only, one active pipeline per type, usage quotas) shape some design choices. Where a limit forced a trade-off, the project README says so.

## Roadmap

- [ ] M1 Medallion pipeline
- [ ] M2 Dimensional model and semantic layer
- [ ] M3 Reference architecture
- [ ] M4 RAG and Genie application
- [ ] Later: Terraform/Bicep infrastructure examples, TOGAF-style capability map, Azure landing-zone design

## Licence

Code and documentation in this repository are released under the MIT Licence (see [`LICENSE`](LICENSE)). Datasets remain under their original licences, listed in `DATASETS.md`.

## Contact

Asadul Islam: connect on LinkedIn (add your profile link here).
