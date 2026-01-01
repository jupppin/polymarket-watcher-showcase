# Polymarket Watcher

**AI-powered trading signals derived from prediction market data**

[Live App](https://polymarketwatcher.com) • [Terms of Service](https://polymarketwatcher.com/terms)

---

## Overview

Polymarket Watcher is a full-stack SaaS platform that uses AI to identify correlations between prediction market outcomes and US stock price movements. Users receive actionable trading signals with entry points, stop-loss/take-profit targets, and automated position monitoring.

**Status:** Live in production with paying customers

---

## How It Works

```
┌─────────────────────┐     ┌─────────────────────┐     ┌─────────────────────┐
│   Polymarket API    │────▶│   Claude AI Engine  │────▶│   Trading Signal    │
│  (Prediction Data)  │     │  (Correlation Logic)│     │  (Ticker, Direction)│
└─────────────────────┘     └─────────────────────┘     └─────────────────────┘
                                                                   │
                                                                   ▼
┌─────────────────────┐     ┌─────────────────────┐     ┌─────────────────────┐
│   Signal Closed     │◀────│  Position Monitor   │◀────│   User Executes     │
│   (P&L Recorded)    │     │  (Real-time Checks) │     │  (Own Brokerage)    │
└─────────────────────┘     └─────────────────────┘     └─────────────────────┘
```

1. **Scan** — AI analyzes active Polymarket predictions for stock correlations
2. **Signal** — User receives ticker, direction (long/short), confidence score, and reasoning
3. **Execute** — User places the trade in their own brokerage
4. **Monitor** — Background service tracks price and triggers exits based on user-defined rules

---

## Architecture

```
├── backend/                    # Python FastAPI
│   ├── app/
│   │   ├── clients/           # Polymarket, Yahoo Finance, Claude AI integrations
│   │   ├── models/            # SQLAlchemy ORM models
│   │   ├── routers/           # REST API endpoints
│   │   ├── services/          # Business logic (signals, monitoring, backtesting)
│   │   └── schemas/           # Pydantic request/response validation
│   └── alembic/               # Database migrations
│
├── frontend/                   # React + Vite
│   ├── src/
│   │   ├── components/        # Reusable UI components
│   │   ├── context/           # Global state management
│   │   ├── hooks/             # Custom React hooks (polling, etc.)
│   │   ├── pages/             # Route-level components
│   │   └── lib/               # API client layer
│   └── tailwind.config.js
│
└── infrastructure/            # Railway deployment configs
```

---

## Tech Stack

### Backend
- **FastAPI** — Async Python web framework
- **PostgreSQL** — Relational database
- **SQLAlchemy 2.0** — ORM with async support
- **Alembic** — Database migrations
- **Anthropic Claude API** — AI model for trade analysis
- **yfinance** — Real-time stock price data

### Frontend
- **React 18** — UI framework
- **Vite** — Build tooling
- **TailwindCSS** — Utility-first styling
- **React Router 6** — Client-side routing

### Infrastructure & Services
- **Railway** — Deployment platform
- **Clerk** — Authentication (JWT, OAuth)
- **Stripe** — Payment processing & webhooks

---

## Key Features

### AI Signal Generation
- Analyzes prediction market questions and probabilities
- Identifies correlated stock tickers (e.g., BTC predictions → MSTR, COIN)
- Provides confidence scores and natural language reasoning
- Two-stage validation: proposal + factual verification

### Position Monitoring
- Background thread monitors all active positions
- Configurable stop-loss, take-profit, and time limits
- Optional AI-assisted exit evaluation (detects thesis breakdown)
- Automatic position closure with outcome recording

### Risk Management
- User-configurable position sizes ($10 - $100k)
- Adjustable stop-loss (0.5% - 50%)
- Maximum gain targets (1% - 100%)
- Time-based exits (0.5 - 168 hours)
- Minimum confidence thresholds

### Backtesting Engine
- Historical analysis against closed Polymarket markets
- Multi-entry-point simulation
- Confidence tier analysis (high/medium/low)
- Aggregate statistics for strategy refinement

### Credit-Based Billing
- Stripe integration with webhook handling
- 1 credit = 1 complete trade cycle
- Multiple pricing tiers
- Transaction history and admin controls

---

## Technical Highlights

### Real-Time Position Tracking
The position monitor runs as a background thread within the FastAPI lifespan, checking all active signals every 60 seconds against current market prices. Exit conditions are evaluated in order of priority: stop-loss, max-gain, time-limit, then optional AI evaluation.

### AI Pipeline
Signal generation uses a three-stage AI pipeline:
1. **Proposal** — Analyzes market data and suggests trade
2. **Validation** — Factual verification of ticker and logic
3. **Exit Evaluation** — (Optional) Ongoing thesis assessment

### Market Hours Detection
The frontend intelligently detects NYSE trading hours and US market holidays, adjusting polling behavior to avoid unnecessary API calls when markets are closed.

### Duplicate Prevention
The system prevents users from opening multiple positions on the same ticker, avoiding overexposure to correlated trades.

---

## Screenshots

### Dashboard
*Active positions with real-time P&L, win rate statistics, and quick actions*

### Signal Generation
*AI-generated trade signal with confidence score, reasoning, and risk parameters*

### Markets View
*Live Polymarket prediction data with probability visualization*

### Settings
*Configurable risk parameters and trading preferences*

---

## Performance

- **Backtested win rate:** 71% (medium+ confidence signals)
- **Response time:** < 500ms API latency
- **Uptime:** 99.9% (Railway managed infrastructure)

---

## What I Learned

- Designing AI prompts for consistent, structured financial analysis
- Building real-time monitoring systems with background threads
- Implementing credit-based billing with Stripe webhooks
- Handling market data edge cases (holidays, pre/post market, stale prices)
- Balancing user configurability with sensible defaults

---

## Contact

**Email:** polymarketwatcher@gmail.com

---

*This is a showcase repository. The source code is private to protect proprietary AI logic.*
