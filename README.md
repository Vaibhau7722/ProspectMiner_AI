# ⛏ ProspectMiner AI — Lead Mining SaaS

AI-powered business lead scraping and enrichment engine.

## 🏗️ Stack

| Layer | Tech |
|-------|------|
| Backend | Node.js, Express, MongoDB, BullMQ, Redis |
| Scraper | Puppeteer + Stealth Plugin |
| AI | OpenAI GPT-4o-mini |
| Frontend | React, Vite, TailwindCSS, Recharts |
| Queue | BullMQ + Redis |
| Payments | Stripe |

---

## 🚀 Quick Start (Local Dev)

### Prerequisites
- Node.js 18+
- MongoDB (local or Atlas)
- Redis (local or Upstash)
- OpenAI API Key

### 1. Backend Setup

```bash
cd backend
cp .env.example .env
# Fill in .env with your MongoDB URI, Redis config, OpenAI API key
npm install
npm run dev         # API server on :5000
```

### 2. Start Workers (separate terminal)

```bash
cd backend
npm run dev:worker  # BullMQ workers
```

### 3. Frontend Setup

```bash
cd frontend
npm install
npm run dev         # React app on :3000
```

Open http://localhost:3000 — register an account and start scraping!

---

## 🐳 Docker (Full Stack)

```bash
# Copy env file
cp backend/.env.example backend/.env
# Edit backend/.env with your API keys

docker-compose up --build
```

- Frontend: http://localhost:3000
- API: http://localhost:5000
- MongoDB: localhost:27017
- Redis: localhost:6379

---

## 📋 How It Works

1. **Search** → User enters "Dentists in Chicago"
2. **Scrape** → Puppeteer scrapes Google Maps listings
3. **Queue** → Each lead goes into BullMQ enrichment queue
4. **Enrich** → Website content fetched + sent to OpenAI
5. **Score** → AI scores each lead High/Medium/Low
6. **Store** → Lead saved to MongoDB, 1 credit deducted
7. **Export** → Download as CSV or Excel

---

## 🔑 Environment Variables

```env
MONGODB_URI=mongodb://localhost:27017/prospectminer
REDIS_HOST=localhost
REDIS_PORT=6379
JWT_SECRET=your-secret-32-chars-minimum
OPENAI_API_KEY=sk-...
STRIPE_SECRET_KEY=sk_test_...   # optional
STRIPE_WEBHOOK_SECRET=whsec_... # optional
```

---

## 📁 Project Structure

```
prospectminer/
├── backend/
│   ├── src/
│   │   ├── config/         # DB, Redis, OpenAI, Logger
│   │   ├── controllers/    # Route handlers
│   │   ├── middleware/     # Auth, credits, errors
│   │   ├── models/         # Mongoose schemas
│   │   ├── queues/         # BullMQ queue definitions
│   │   ├── routes/         # Express routes
│   │   ├── services/       # Scraper, AI enrich, score, export
│   │   ├── utils/          # Helpers
│   │   └── workers/        # BullMQ job processors
│   └── server.js
├── frontend/
│   └── src/
│       ├── context/        # Auth context
│       ├── pages/          # Dashboard, Search, Leads, Analytics, Credits
│       ├── components/     # Table, Progress, Filters, Badges
│       └── services/       # Axios API client
└── docker-compose.yml
```

---

## 💳 Credit System

- New accounts get **10 free credits**
- 1 credit = 1 successfully enriched lead
- Credits are deducted atomically via MongoDB transactions
- Failed enrichments do NOT cost credits
- Top up via Stripe (configure STRIPE_SECRET_KEY)

---

## 🔒 Security

- JWT authentication (7-day tokens)
- bcrypt password hashing (12 rounds)
- Helmet security headers
- CORS configured per origin
- Rate limiting: 200 req/15min per IP
- Input validation on all routes

---

## 📈 Scaling

- Scale API horizontally (stateless)
- Scale workers independently via Docker replicas
- MongoDB Atlas with read replicas for heavy reads
- Redis Cluster for queue high availability
