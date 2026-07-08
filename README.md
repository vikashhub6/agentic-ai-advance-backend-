# AgentForge

An agentic AI platform with a microservices backend — chat, coding assistance, image generation, document (PDF/PPT) generation, RAG-based document Q&A, and web-search-augmented answers, all orchestrated behind a single API gateway.

**Live Demo:** [agentic-ai-advance-backend-l44u.vercel.app](https://agentic-ai-advance-backend-l44u.vercel.app/)

---

## Overview

AgentForge is a full-stack, multi-agent AI application. Instead of a single monolithic backend, it is split into independent microservices (auth, chat, billing, and a dedicated agent-orchestration service) that sit behind an API gateway. The agent service uses **LangGraph** to route requests to specialized agents depending on the task — coding, search, image generation, document creation, or RAG-based PDF Q&A.

## Features

- 🔐 **Authentication** — Email/Password and Google sign-in via Firebase, with a dedicated auth microservice
- 💬 **Multi-agent chat** — Requests are routed to the right specialized agent based on intent
- 👨‍💻 **Coding agent** — Code generation and assistance (DeepSeek via OpenRouter)
- 🔎 **Search agent** — Web-search-augmented responses using Tavily
- 🖼️ **Image generation agent**
- 👁️ **Vision agent** — Image understanding (Gemini)
- 📄 **PDF & PPT generation agents** — Generates downloadable documents on request
- 📚 **RAG-based PDF Q&A** — Upload a PDF and ask questions grounded in its content (Qdrant vector store + Google embeddings)
- 💳 **Billing** — Subscription/payment handling via Razorpay
- ⚡ **Rate limiting & caching** — Redis-backed at the gateway layer

## Tech Stack

**Frontend**
- React 19 + Vite
- Redux Toolkit
- Tailwind CSS
- Firebase Auth
- Monaco Editor (in-app code editor)
- Framer Motion

**Backend (Microservices, Node.js + Express)**
| Service | Responsibility |
|---|---|
| `gateway` | Single entry point, routing, rate limiting, auth checks |
| `auth` | User authentication (Firebase Admin + MongoDB) |
| `chat` | Chat history & conversation persistence |
| `agent` | AI orchestration — LangGraph, all specialized agents |
| `billing` | Subscriptions & payments (Razorpay) |

**AI / ML**
- LangChain + LangGraph for agent orchestration
- Groq (Llama 3.3 70B) — chat, search, image agents
- Google Gemini 2.5 Flash — vision agent
- Google Gemini Embeddings — RAG embeddings
- OpenRouter (DeepSeek) — coding agent
- Qdrant — vector database for RAG
- Tavily — web search API

**Infrastructure**
- MongoDB — primary datastore
- Redis — caching & rate limiting
- AWS S3 — file storage
- Docker — containerization
- Deployed on Vercel (frontend) & Railway (backend)

## Architecture

```
                        ┌──────────────┐
                        │   Frontend   │
                        │  (React/Vite)│
                        └──────┬───────┘
                               │
                        ┌──────▼───────┐
                        │   Gateway    │
                        │ (rate-limit, │
                        │   routing)   │
                        └──────┬───────┘
              ┌────────┬───────┼───────┬────────┐
              │        │       │       │        │
         ┌────▼───┐┌───▼───┐┌──▼───┐┌──▼─────┐
         │  Auth  ││ Chat  ││Agent ││Billing │
         │Service ││Service││Service│Service │
         └────────┘└───────┘└──┬───┘└────────┘
                                │
                 ┌──────────────┼──────────────┐
                 │              │              │
            ┌────▼───┐   ┌──────▼─────┐  ┌─────▼────┐
            │ Groq /  │   │  Qdrant    │  │ Tavily /  │
            │ Gemini /│   │ (Vector DB)│  │  AWS S3   │
            │OpenRouter│  └────────────┘  └───────────┘
            └─────────┘
```

## Getting Started

### Prerequisites
- Node.js 18+
- MongoDB instance
- Redis instance
- API keys: Google (Gemini), Groq, Tavily, OpenRouter, Qdrant, AWS, Firebase, Razorpay

### Setup

1. **Clone the repo**
   ```bash
   git clone https://github.com/vikashhub6/agentic-ai-advance-backend-.git
   cd agentic-ai-advance-backend-
   ```

2. **Backend — install dependencies for each service**
   ```bash
   cd backend/gateway && npm install
   cd ../services/auth && npm install
   cd ../chat && npm install
   cd ../agent && npm install
   cd ../billing && npm install
   ```

3. **Configure environment variables**
   Each service has its own `.env` file (see `.env` in each service folder for required keys — MongoDB URL, Redis URL, Firebase, Groq, Google, Tavily, OpenRouter, Qdrant, AWS, Razorpay).

4. **Run Redis** (via Docker)
   ```bash
   cd backend && docker-compose up -d
   ```

5. **Start backend services** (each in its own terminal)
   ```bash
   npm run dev
   ```

6. **Frontend**
   ```bash
   cd frontend
   npm install
   npm run dev
   ```

## Roadmap

- [ ] Full backend deployment on Railway
- [ ] CI/CD pipeline
- [ ] Streaming responses for chat
- [ ] Usage analytics dashboard

## Author

**Vikash Kumar**
GitHub: [@vikashhub6](https://github.com/vikashhub6)
