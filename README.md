# SourceWithAI â€” AI-Powered B2B Product Sourcing Platform

> **Live at [sourcewithai.com](https://sourcewithai.com)** | Private repository â€” code available upon request

SourceWithAI is a full-stack AI-powered e-commerce platform that connects businesses with global suppliers through conversational search, multi-platform aggregation, and intelligent product matching.

---

## Architecture

```
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚                        NGINX                                â”‚
â”‚              SSL / Reverse Proxy / Load Balancing            â”‚
â”œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¤
â”‚                        â”‚                                    â”‚
â”‚   sourcewithai.com     â”‚     apiv2.sourcewithai.com         â”‚
â”‚         :3000          â”‚              :3003                  â”‚
â”‚                        â”‚                                    â”‚
â”‚  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”      â”‚      â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”          â”‚
â”‚  â”‚  Next.js 16  â”‚      â”‚      â”‚  Express.js API  â”‚          â”‚
â”‚  â”‚  React 19    â”‚â—„â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â–ºâ”‚  40+ Services    â”‚          â”‚
â”‚  â”‚  App Router  â”‚  RESTâ”‚ WS   â”‚  32 Route Files  â”‚          â”‚
â”‚  â”‚  Tailwind    â”‚      â”‚      â”‚  Socket.io       â”‚          â”‚
â”‚  â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜      â”‚      â””â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜          â”‚
â”‚                        â”‚               â”‚                    â”‚
â”‚                        â”‚      â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â–¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”          â”‚
â”‚                        â”‚      â”‚  FastAPI (Py)    â”‚          â”‚
â”‚                        â”‚      â”‚  AI Orchestrator â”‚          â”‚
â”‚                        â”‚      â”‚       :8000      â”‚          â”‚
â”‚                        â”‚      â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜          â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                    â”‚
                 â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
                 â”‚                  â”‚                  â”‚
          â”Œâ”€â”€â”€â”€â”€â”€â–¼â”€â”€â”€â”€â”€â”€â”   â”Œâ”€â”€â”€â”€â”€â”€â–¼â”€â”€â”€â”€â”€â”€â”   â”Œâ”€â”€â”€â”€â”€â”€â–¼â”€â”€â”€â”€â”€â”€â”
          â”‚  MongoDB    â”‚   â”‚   Redis     â”‚   â”‚Elasticsearchâ”‚
          â”‚  16 Models  â”‚   â”‚  Cache/PubSubâ”‚   â”‚ AI-Enhanced â”‚
          â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜   â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜   â”‚   Search    â”‚
                                              â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
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
  â†“
Intent Classification (GPT-4o-mini)
  â†“
Clarification: budget range? material preference? print method?
  â†“
Multi-Platform Search (OTAPI + 1688 + SAGE)
  â†“
AI-Ranked Results with Match Explanations
```

**Phases:** Greeting â†’ Product Detection â†’ Clarification â†’ Search â†’ Confirmation

### Multi-Platform Product Aggregation
- **OTAPI** â€” Alibaba ecosystem (Taobao, 1688, Tmall)
- **Alibaba 1688 Direct** â€” Wholesale/Fenxiao platform
- **SAGE** â€” US promotional products marketplace
- Unified search across all platforms with deduplication
- Real-time inventory and pricing

### AI-Powered Search
- **Semantic search** with vector embeddings (all-MiniLM-L6-v2)
- **Elasticsearch** with AI query expansion (synonyms, variants, related terms)
- **Image-based search** â€” upload a photo, find matching products
- **Object detection** â€” identify products in images automatically
- **RAG pipeline** â€” retrieval-augmented generation for contextual results

### Vendor Intelligence
- Premium vendor scoring algorithm (reviews, certifications, order history)
- Vendor type classification (Trading / Manufacturing / Workshop)
- Verified merchant badges
- Cross-platform vendor comparison

### E-Commerce Workflow
- Shopping cart with product configurations (size, color, material, customization)
- Guest checkout and authenticated checkout flows
- **RFQ (Request for Quote)** â€” B2B quotation system with admin approval workflow
- Stripe payment processing with fraud detection
- Order tracking and management

### Additional Capabilities
- **Product mockup generation** â€” AI-generated preview with logo/artwork placement
- **Chinese â†” English translation** â€” automatic product translation
- **Market signals** â€” trending products and keyword analytics
- **Price intelligence** â€” cross-supplier price comparison
- **Blog/content system** â€” SEO-optimized content management
- **Bundle & coupon system** â€” promotional pricing tools

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

- **User** â€” Auth, roles (user/admin/super_admin), address book, company info, preferences
- **Product** â€” Multi-platform catalog with specs, pricing tiers, inventory tracking
- **Order** â€” Full order lifecycle with guest support, payment status, shipping tracking
- **RFQ** â€” B2B quote requests with admin approval workflow and auto-expiry
- **Cart** â€” Session-based cart with product configurations and customization
- **Payment** â€” Stripe integration with fraud risk scoring
- **ChatHistory / Conversation / ConversationSession** â€” Multi-turn dialogue persistence
- **Review, Blog, Bundle, Coupon** â€” Engagement and marketing features

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

**CTO / Lead Developer** â€” Designed and built the entire platform from scratch:

- Architected the 3-service system (Next.js frontend + Express API + FastAPI AI service)
- Built the multi-phase conversational AI sourcing engine
- Integrated 3 supplier platforms (OTAPI, 1688, SAGE) with unified search
- Implemented vector search (RAG), Elasticsearch, and multi-LLM orchestration
- Built the full e-commerce workflow (cart â†’ checkout â†’ payment â†’ order tracking)
- Designed the RFQ system for B2B quotation management
- Deployed and maintain production infrastructure (Nginx, PM2, VPS)

---

## Status

**Production** â€” Live and serving real B2B customers at [sourcewithai.com](https://sourcewithai.com)

---

> *This is a private commercial project. The repository contains proprietary code and is not publicly available. Architecture details and technical approach are shared here for portfolio purposes. Code review available upon request during interviews.*
