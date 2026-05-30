# Financial-Intelligence Platform — autonomous business intelligence for lenders

A production B2B financial-intelligence platform for banks and commercial lenders. A loan officer submits a business name and address; the platform autonomously assembles a single, scored, ~17-section intelligence report on that business — who owns it, how it performs, how it sits in its local market, what loans are already filed against it, and which banking products it fits.

→ **[The product + what it does](https://bostondevelopment.github.io/financial-site/)**
→ **[Engineering deep-dive](https://bostondevelopment.github.io/financial-site/engineering.html)** — async Celery pipeline, PostGIS geospatial engine, ~10 data integrations, containerized infrastructure
→ **[AI](https://bostondevelopment.github.io/financial-site/ai.html)** — constrained multi-provider LLM orchestration, structured output, a compositional prompt system, and a token-level cost + audit ledger

---

## About this repo

This repository hosts the public-facing case-study site for a B2B financial-intelligence platform I built core systems for — a product overview, an engineering deep-dive, and an AI write-up. The platform itself (a Django / DRF backend) lives in a separate, private repository. Company names and identifying details have been removed; the system is described on its own technical merits.

## About the platform

A loan officer can't answer "is this business expanding, over-leveraged, and when should I call them?" from any single data feed. The platform fuses four hard modalities onto one report — regulatory filings, behavioral foot traffic, official geostatistics, and unstructured web — then scores the result deterministically and recommends the banking products the business is a fit for. The AI explains; deterministic math decides, so every grade and recommendation is auditable and defensible to a credit committee.

## What I built

I stood up the platform's foundation and owned its hardest systems:

- **Foundation.** Created the application from the ground up — the initial Django app, its settings and service-layer structure, and the containerized infrastructure around it: the Dockerfile for the full GIS toolchain and the Compose stack wiring web, workers, Redis, and a PostGIS database.
- **The async pipeline.** A multi-tier Celery fan-out/fan-in canvas — an outer chord over candidate businesses, per-candidate chains, and an immutable group→score barrier — with at-least-once delivery, bounded retries, and idempotent row-locked writes.
- **The geospatial engine.** A trade-area catchment pipeline that synthesizes polygons from mobility data (DBSCAN outlier clustering → alphashape concave hull → periodic B-spline smoothing → PostGIS), plus an erf-scaled seasonality score from 52-week footfall.
- **Entity resolution under concurrency.** Normalization + fuzzy matching across ~10 sources, resolved under `select_for_update` + `get_or_create`, so concurrent workers converge on one canonical company or person.
- **FinOps + lifecycle.** Token-level model-cost tracking into a polymorphic usage log with an archival audit trail, and a report-expiration / subscription-refresh subsystem that auto-regenerates stale reports.

Plus the strict Ruff ruleset and pytest harness, production observability (a Slack error handler with cloud-logging deep links, statsd metrics, request-ID propagation), and later carrying the codebase through a Django 5.1 / Python 3.12 upgrade.

The **[engineering page](https://bostondevelopment.github.io/financial-site/engineering.html)** and **[AI page](https://bostondevelopment.github.io/financial-site/ai.html)** walk through all of it.

---

## Author

Built by **Michael Finneran** — Boston, MA
[linkedin.com/in/michaelfinneran](https://linkedin.com/in/michaelfinneran) · [bostondevelopmentco@gmail.com](mailto:bostondevelopmentco@gmail.com)
