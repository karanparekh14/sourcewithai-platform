# SourceWithAI - AI-Powered B2B Product Sourcing Platform

> **Live at [sourcewithai.com](https://sourcewithai.com)** | Private repository - code available upon request

SourceWithAI is a full-stack AI-powered e-commerce platform that connects businesses with global suppliers through conversational search, multi-platform aggregation, and intelligent product matching.

---

## Architecture

```
+-------------------------------------------------------------+
|                        NGINX                                |
|              SSL / Reverse Proxy / Load Balancing            |
+------------------------+------------------------------------+
|                        |                                    |
|   sourcewithai.com     |     apiv2.sourcewithai.com         |
|         :3000          |              :3003                  |
|                        |                                    |
|  +--------------+      |      +------------------+          |
|  |  Next.js 16  |      |      |  Express.js API  |          |
|  |  React 19    |<-----+----->|  40+ Services    |          |
|  |  App Router  |  REST| WS   |  32 Route Files  |          |
|  |  Tailwind    |      |      |  Socket.io       |          |
|  +--------------+      |      +--------+---------+          |
|                        |               |                    |
|                        |      +--------v---------+          |
|                        |      |  FastAPI (Py)    |          |
|                        |      |  AI Orchestrator |          |
|                        |      |       :8000      |          |
|                        |      +------------------+          |
+------------------------+------------------------------------+
                                    |
                 +------------------+------------------+
                 |                  |                  |
          +------v------+   +------v------+   +------v------+
          |  MongoDB    |   |   Redis     |   |Elasticsearch|
          |  16 Models  |   |  Cache/PubSub|   | AI-Enhanced |
          +-------------+   +-------------+   |   Search    |
                                              +-------------+
```

---

## Tech Stack

| Layer | Technologies |
|-------|-------------|
| **Frontend** | Next.js 16, React 19, Tailwind CSS 4, Framer Motion, Three.js, Spline 3D, Radix UI |
| **Backend** | Express.js, Socket.io (WebSocket), Node.js 18+, PM2 |
| **AI Service** | Python FastAPI, OpenAI GPT-4o-mini, Anthropic Claude, Google Gemini |
| **Search** | Elasticsearch (AI-enhanced queries), Vector embeddings (Transformers.js, all-MiniLM-L6-v2), RAG |
| **Database** | MongoDB (Mongoose ODM, 16 models), Redis (caching + pub/sub) |
| **Payments** | Stripe (payment intents, webhooks, fraud detection) |
| **Storage** | AWS S3, Cloudinary CDN, Vultr Object Storage |
| **Infra** | Vultr VPS, Nginx reverse proxy, PM2 process management, dual-domain setup |

---

## Core Features

### Conversational AI Sourcing
Multi-phase dialogue system that understands natural language product requests and guides users from intent to purchase.

```
User: "I need 500 custom printed tote bags for a conference"
  |
  v
Intent Classification (GPT-4o-mini)
  |
  v
Clarification: budget range? material preference? print method?
  |
  v
Multi-Platform Search (OTAPI + 1688 + SAGE)
  |
  v
AI-Ranked Results with Match Explanations
```

**Phases:** Greeting > Product Detection > Clarification > Search > Confirmation

### Multi-Platform Product Aggregation
- **OTAPI** - Alibaba ecosystem (Taobao, 1688, Tmall)
- **Alibaba 1688 Direct** - Wholesale/Fenxiao platform
- **SAGE** - US promotional products marketplace
- Unified search across all platforms with deduplication
- Real-time inventory and pricing

### AI-Powered Search
- **Semantic search** with vector embeddings (all-MiniLM-L6-v2)
- **Elasticsearch** with AI query expansion (synonyms, variants, related terms)
- **Image-based search** - upload a photo, find matching products
- **Object detection** - identify products in images automatically
- **RAG pipeline** - retrieval-augmented generation for contextual results

### Vendor Intelligence
- Premium vendor scoring algorithm (reviews, certifications, order history)
- Vendor type classification (Trading / Manufacturing / Workshop)
- Verified merchant badges
- Cross-platform vendor comparison

### E-Commerce Workflow
- Shopping cart with product configurations (size, color, material, customization)
- Guest checkout and authenticated checkout flows
- **RFQ (Request for Quote)** - B2B quotation system with admin approval workflow
- Stripe payment processing with fraud detection
- Order tracking and management

### Additional Capabilities
- **Product mockup generation** - AI-generated preview with logo/artwork placement
- **Chinese <> English translation** - automatic product translation
- **Market signals** - trending products and keyword analytics
- **Price intelligence** - cross-supplier price comparison
- **Blog/content system** - SEO-optimized content management
- **Bundle & coupon system** - promotional pricing tools

---

## API Surface

**50+ REST endpoints** across 10 domains:

| Domain | Endpoints | Description |
|--------|-----------|-------------|
| `/api/search` | 15+ | Multi-platform search, image search, categories, vendor lookup |
| `/api/chat` | 6 | Conversational AI with session management |
| `/api/rfq` | 8 | B2B quotation workflow (guest + authenticated) |
| `/api/orders` | 8 | Order management (guest + authenticated) |
| `/api/cart` | 4 | Shopping cart operations |
| `/api/payments` | 4 | Stripe payment processing + webhooks |
| `/api/auth` | 7 | JWT auth, Google OAuth, email verification |
| `/api/ai-search` | 4 | AI orchestration, intent detection, property extraction |
| `/api/1688` | 4 | Alibaba 1688 direct integration |
| `/api/admin` | 5+ | Dashboard analytics, user management |

---

## Database Schema

**16 Mongoose models** covering the full e-commerce lifecycle:

- **User** - Auth, roles (user/admin/super_admin), address book, company info, preferences
- **Product** - Multi-platform catalog with specs, pricing tiers, inventory tracking
- **Order** - Full order lifecycle with guest support, payment status, shipping tracking
- **RFQ** - B2B quote requests with admin approval workflow and auto-expiry
- **Cart** - Session-based cart with product configurations and customization
- **Payment** - Stripe integration with fraud risk scoring
- **ChatHistory / Conversation / ConversationSession** - Multi-turn dialogue persistence
- **Review, Blog, Bundle, Coupon** - Engagement and marketing features

---

## AI Models Used

| Model | Purpose |
|-------|---------|
| **GPT-4o-mini** | Conversation responses, intent classification, keyword generation, match explanations |
| **Claude 3 Haiku** | Query enhancement, synonym expansion for Elasticsearch |
| **Gemini 2.5** | Fallback query enhancement |
| **all-MiniLM-L6-v2** | Vector embeddings for semantic search (Transformers.js) |

---

## My Role

**CTO / Lead Developer** - Designed and built the entire platform from scratch:

- Architected the 3-service system (Next.js frontend + Express API + FastAPI AI service)
- Built the multi-phase conversational AI sourcing engine
- Integrated 3 supplier platforms (OTAPI, 1688, SAGE) with unified search
- Implemented vector search (RAG), Elasticsearch, and multi-LLM orchestration
- Built the full e-commerce workflow (cart > checkout > payment > order tracking)
- Designed the RFQ system for B2B quotation management
- Deployed and maintain production infrastructure (Nginx, PM2, VPS)

---

## Status

**Production** - Live and serving real B2B customers at [sourcewithai.com](https://sourcewithai.com)

---

> *This is a private commercial project. The repository contains proprietary code and is not publicly available. Architecture details and technical approach are shared here for portfolio purposes. Code review available upon request during interviews.*
