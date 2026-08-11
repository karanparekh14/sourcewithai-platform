# SourceWithAI, AI-powered B2B sourcing platform

> **Live at [sourcewithai.com](https://sourcewithai.com)** · Private commercial repository, architecture shared here for portfolio purposes

SourceWithAI is a production B2B sourcing platform. Buyers describe what they need in plain language, and the system works out what they actually mean, searches multiple supplier networks, filters aggressively, and returns a short list of products it can justify.

The interesting engineering problem is not the chat interface. It is that supplier marketplace search APIs return large volumes of loosely relevant results, so the platform is built to **treat those APIs as raw data sources rather than as search engines**, and to do the ranking, filtering and judgement itself.

---

## Architecture

Three services behind Nginx, split by what each one is good at.

```
                          Nginx (TLS, reverse proxy)
                                     |
        +----------------------------+----------------------------+
        |                                                         |
   Next.js 16 / React 19                                Express API (Node)
   51 app routes                    <---- REST / WS ---->  50 route files
   178 components                                          47 API domains
                                                           294 handlers
                                                           Socket.io
                                                                 |
                                                        Python FastAPI
                                                        AI orchestration
                                                                 |
        +----------------------+----------------------+----------+
        |                      |                      |
    MongoDB                  Redis              Elasticsearch
    35 models          cache, pub/sub, sessions   product search
```

**Why three services.** The AI layer is Python native, the transactional and real-time layer is better in Node, and separating them means the AI service can be redeployed or restarted without touching checkout.

---

## The search pipeline

The core of the product. A naive implementation sends the user's words to a supplier API and shows what comes back, which produces poor results because those APIs match on keywords rather than intent.

Instead, search runs through a staged pipeline with a deliberate division of labour across models, each chosen for what it is good at rather than for being the strongest available.

| Stage | Role | Why this model class |
|---|---|---|
| **Intent reasoning** | Understand the request, detect ambiguity, list possible meanings, score confidence | Fast and cheap, excellent at classification. Returns strict JSON and is instructed never to guess |
| **Routing and category selection** | Decide search strategy, pick category codes, exclude irrelevant categories | Very fast and deterministic. Used as a classifier, not a creative model |
| **Judgement and reranking** | Reject irrelevant products, rank the rest, score fit, explain the choice | Best buyer-style reasoning. Exactly **one call per query**, and it does no searching or routing |
| **Response synthesis** | Explain the result set back to the user | Cheap and fast, no reasoning burden |

**Dual-reality retrieval.** Rather than committing to one interpretation of an ambiguous request, the pipeline forks intent into two readings, one category-dominant and one attribute-dominant, runs both, then intersects them. Agreement between the two paths becomes a confidence signal. A deterministic eligibility filter runs before any model judgement, so obviously wrong products are eliminated by rules rather than by tokens. A final quality gate requires a minimum number of products above a confidence threshold before results are returned at all.

The design principle behind it: **eliminate wrong products first rather than trying to understand everything perfectly.**

---

## Retrieval and ranking

- **Elasticsearch** for product search, with AI-expanded queries so synonyms, variants and related terms are folded in before the query runs
- **Vector embeddings** for semantic matching where the buyer describes an outcome rather than naming a product
- **Hybrid ranking**, because lexical matching still wins on exact product names and part numbers while embeddings win on description
- **Object detection** (DETR via transformers.js) for image search, so a buyer can upload a photo and have the product identified before searching
- **Redis** caching for embeddings, hot queries and warmed result sets
- Cross-network deduplication and unified ranking, since the same physical product appears under different titles, currencies and minimum order conventions depending on the source

**Supplier networks integrated:** OTAPI fronting the Alibaba ecosystem (Taobao, 1688, Tmall), Alibaba 1688 direct for wholesale and Fenxiao, and SAGE for the US promotional products market. Each has a different schema, currency handling and minimum order convention, so normalising them into one catalogue was a larger job than the retrieval layer itself.

---

## Supplier intelligence

Beyond search, the platform maintains a view of the supplier side.

- Premium vendor scoring from reviews, certifications and order history
- Supplier snapshots on a schedule, with change alerts
- Competitor and cross-network price comparison
- Minimum-order-quantity splitting across suppliers
- Trending keyword and market signal tracking
- Configurable product boost rules for merchandising

---

## Commerce and B2B workflow

- Conversational sourcing sessions with multi-turn refinement and persisted context
- Request for quote workflow with admin approval and expiry handling
- Cart and checkout for both guest and authenticated buyers, with product configuration and customisation
- Stripe payments with fraud risk scoring and webhook handling
- Wholesale and direct-supplier order flows
- Order tracking, reviews, support tickets, bundles, coupons and a points system
- Product mockup generation for client previews, and automated deck rendering
- Automatic translation of supplier product data
- Notion sync for orders, quotes and trending data
- Admin dashboard with analytics, user management and configuration

---

## Stack

| Layer | Technologies |
|---|---|
| **Frontend** | Next.js 16, React 19, Tailwind, Framer Motion, GSAP, Three.js, Spline, Radix UI, TipTap, Recharts, SWR |
| **API** | Express, Socket.io, Mongoose, JWT and OAuth, Helmet, rate limiting, request validation, Winston, Swagger |
| **AI services** | Python FastAPI, multi-provider orchestration across OpenAI, Anthropic and Google model families |
| **Search** | Elasticsearch, vector embeddings, transformers.js for on-device vision |
| **Data** | MongoDB, Redis, S3-compatible object storage |
| **Media** | Sharp, HEIC conversion, Fabric.js, Puppeteer for server-side rendering |
| **Payments** | Stripe |
| **Ops** | Nginx, PM2, scheduled jobs, health checks for Elasticsearch and Redis |

---

## Scale

| | |
|---|---|
| API route files | 50 |
| Mounted API domains | 47 |
| Route handlers | 294 |
| Backend services | 63 |
| Data models | 35 |
| Frontend routes | 51 |
| React components | 178 |

---

## My role

I own the data and AI layer of the platform and work alongside the engineering team on the rest of it. That covers:

- The search pipeline end to end: intent classification, retrieval strategy, the multi-model orchestration, ranking and the quality gate
- Elasticsearch and vector retrieval, embeddings, and the caching layer that keeps query cost and latency down
- Evaluation and production monitoring, including a labelled evaluation set built from real buyer queries, retrieval scoring with Ragas alongside precision, recall and F1, and per-request latency and token-cost tracking
- The conversational sourcing engine and the agent workflows behind it
- Supplier intelligence: scoring, snapshots, alerting and price comparison
- The analytics layer, reporting pipelines and dashboards the business runs on
- Access control and audit logging on customer-facing endpoints

---

## Status

**In production**, serving real B2B customers.

> Private commercial repository. Source is not public. Architecture and technical approach are documented here for portfolio purposes; a code walkthrough is available on request during interviews.
