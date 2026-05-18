# 🛒 StellarCart — Conversational Agentic Storefront on Stellar

> **Enabling merchants and content creators to accept digital payments seamlessly through AI-powered conversational commerce, built on the Stellar network.**

[![Stellar](https://img.shields.io/badge/Built%20on-Stellar-black?logo=stellar)](https://stellar.org)
[![Soroban](https://img.shields.io/badge/Smart%20Contracts-Soroban-blueviolet)](https://soroban.stellar.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Status: Active Development](https://img.shields.io/badge/Status-Active%20Development-brightgreen)]()

---

## 🌍 Overview

StellarCart is a conversational, AI-agent-powered storefront that allows merchants, freelancers, and content creators to set up and run fully functional digital storefronts through natural language — no technical setup, no complex dashboards. Customers can browse, pay, and receive receipts through a chat interface, while merchants manage inventory, pricing, subscriptions, and payouts all via conversation.

Built natively on **Stellar**, StellarCart leverages the network's low-fee, fast-settlement infrastructure and **Soroban smart contracts** to handle escrow, subscriptions, streaming payments, and multi-currency checkout.

---

## 🎯 Problem Statement

- Over **300 million** small creators and micro-merchants globally lack access to simple, instant digital payment tools
- Existing storefront solutions require technical know-how, costly integrations, and impose high fees (2–5%)
- Most Web3 payment tools are wallet-focused but lack the full merchant experience: inventory, invoicing, subscriptions, tipping, and analytics
- Language and UX barriers prevent adoption in emerging markets like West Africa, Southeast Asia, and Latin America

---

## ✨ Core Features

### 🤖 Conversational AI Commerce Agent
- Merchants create and manage their entire store through natural language (text or voice)
- Customers can discover, ask questions about products, and pay — all through a chat interface
- Powered by an embedded LLM agent with real-time Stellar state awareness
- Multi-language support: English, French, Spanish, Yoruba, Swahili, and more

### 💳 Seamless Stellar Payments
- Accept **XLM**, **USDC**, **EURC**, and any Stellar-issued asset
- Sub-cent transaction fees (<0.00001 XLM per op)
- Real-time settlement — no waiting 3–5 business days
- Automatic currency conversion via **Stellar DEX** at point of checkout

### 📦 Smart Contract Escrow (Soroban)
- Milestone-based payment release for service providers and freelancers
- Dispute resolution with time-locked escrow and arbitrator support
- Auto-refund logic for digital goods not delivered within SLA

### 🔁 Streaming Payments & Subscriptions
- Native subscription billing using Stellar's recurring payment primitives
- Content creators can monetize newsletters, courses, and communities with weekly/monthly streams
- Pay-per-minute streaming for live sessions, consultations, and premium content

### 🧾 On-Chain Invoicing
- Generate Stellar-anchored invoices with unique payment memos
- Invoice status tracked entirely on-chain — no backend required
- QR code and shareable payment links for offline-to-online commerce

### 📊 Agent-Powered Analytics Dashboard
- Ask the agent: *"How much did I earn last week in USDC?"* or *"Who are my top 5 buyers?"*
- Revenue charts, conversion rates, and payout history via natural language queries
- Export reports as CSV or PDF

### 🔌 Agentic Integrations
- Connect to WhatsApp, Telegram, Discord, or any messaging platform as a storefront
- Webhook support for physical PoS integration
- Federated identity via **Stellar Federation** — use `merchant*stellarcart.io` as a payment address

### 🌐 Token-Gated Storefronts
- Gate products or content behind ownership of specific Stellar-issued tokens or NFTs (Soroban SEP-0011)
- Community memberships, early-access drops, and loyalty programs via on-chain token gating

---

## 🏗️ Architecture

```
┌──────────────────────────────────────────────────────┐
│                  StellarCart Platform                │
│                                                      │
│  ┌──────────────┐     ┌──────────────────────────┐  │
│  │  Chat UI /   │────▶│  AI Commerce Agent       │  │
│  │  WhatsApp /  │     │  (LLM + Stellar SDK)     │  │
│  │  Telegram    │◀────│  Intent → TX Builder     │  │
│  └──────────────┘     └────────────┬─────────────┘  │
│                                    │                  │
│               ┌────────────────────▼──────────────┐  │
│               │        Stellar Network            │  │
│               │  ┌──────────┐  ┌───────────────┐ │  │
│               │  │ Soroban  │  │  Stellar DEX  │ │  │
│               │  │Contracts │  │  (AMM / CLMM) │ │  │
│               │  │ Escrow   │  │  FX at POS    │ │  │
│               │  │ Subs     │  └───────────────┘ │  │
│               │  └──────────┘                    │  │
│               │  ┌──────────────────────────┐    │  │
│               │  │  Anchors (USDC, EURC)    │    │  │
│               │  │  MoneyGram / Circle      │    │  │
│               │  └──────────────────────────┘    │  │
│               └──────────────────────────────────┘  │
└──────────────────────────────────────────────────────┘
```

---

## 🔧 Tech Stack

| Layer | Technology |
|---|---|
| Blockchain | Stellar (Mainnet + Testnet) |
| Smart Contracts | Soroban (Rust) |
| AI Agent Framework | LangChain / Vercel AI SDK |
| LLM Backend | Claude claude-sonnet-4-20250514 / GPT-4o (configurable) |
| Frontend | Next.js 14 + Tailwind CSS |
| Wallet Integration | Freighter, xBull, Lobstr, WalletConnect |
| Payments Protocol | SEP-0006, SEP-0024, SEP-0031 (Stellar Anchor Standards) |
| Asset Issuance | SEP-0011 (Token) + Soroban NFT standard |
| Federation | Stellar Federation Protocol |
| Storage | IPFS (product images, metadata) |
| Database | Supabase (off-chain merchant state) |
| Notifications | Push Protocol + Email (Resend) |

---

## 🚀 Getting Started

### Prerequisites

- Node.js >= 18
- Rust (for Soroban contract compilation)
- Stellar CLI (`cargo install --locked stellar-cli`)
- A Stellar testnet account ([Stellar Laboratory](https://laboratory.stellar.org))

### Installation

```bash
# Clone the repo
git clone https://github.com/your-org/stellarcart
cd stellarcart

# Install dependencies
npm install

# Configure environment
cp .env.example .env.local
# Fill in: STELLAR_NETWORK, ANCHOR_URL, OPENAI_API_KEY, SUPABASE_URL
```

### Deploy Soroban Contracts

```bash
# Build contracts
cd contracts/
cargo build --target wasm32-unknown-unknown --release

# Deploy escrow contract to testnet
stellar contract deploy \
  --wasm target/wasm32-unknown-unknown/release/escrow.wasm \
  --source YOUR_SECRET_KEY \
  --network testnet

# Deploy subscription contract
stellar contract deploy \
  --wasm target/wasm32-unknown-unknown/release/subscription.wasm \
  --source YOUR_SECRET_KEY \
  --network testnet
```

### Run Locally

```bash
npm run dev
# App available at http://localhost:3000
```

---

## 📁 Project Structure

```
stellarcart/
├── app/                    # Next.js App Router
│   ├── storefront/         # Public-facing chat storefront
│   ├── dashboard/          # Merchant management UI
│   └── api/                # API routes (agent, webhooks)
├── contracts/              # Soroban smart contracts (Rust)
│   ├── escrow/             # Milestone escrow contract
│   ├── subscription/       # Recurring billing contract
│   └── token-gate/         # Token-gated access contract
├── agent/                  # AI commerce agent logic
│   ├── tools/              # Stellar SDK tool bindings
│   ├── prompts/            # System prompts per merchant type
│   └── memory/             # Conversation + state memory
├── lib/
│   ├── stellar/            # Stellar SDK helpers
│   ├── anchors/            # SEP-0024 anchor integrations
│   └── federation/         # Stellar Federation lookups
├── public/
└── docs/                   # Architecture docs
```

---

## 🛤️ Roadmap

### Phase 1 — MVP (Q3 2026)
- [x] Conversational storefront (single merchant)
- [x] XLM + USDC payments
- [x] Basic Soroban escrow contract
- [ ] Freighter wallet integration
- [ ] Invoice generation with QR code
- [ ] WhatsApp channel integration

### Phase 2 — Growth (Q4 2026)
- [ ] Multi-merchant marketplace mode
- [ ] Subscription / streaming payments
- [ ] Stellar DEX auto-conversion at checkout
- [ ] Token-gated storefronts
- [ ] Analytics agent queries
- [ ] Telegram + Discord integrations

### Phase 3 — Scale (Q1 2027)
- [ ] Physical PoS webhook bridge
- [ ] Agent marketplace (pre-built merchant personas)
- [ ] Multi-language LLM support (Yoruba, Swahili, French)
- [ ] MoneyGram / Circle anchor integration for cash-in/out
- [ ] Mobile SDK (React Native)
- [ ] Merchant DAO for platform governance

---

## 💡 Use Cases

| User Type | Use Case |
|---|---|
| Freelancer | Accept project payments with milestone escrow |
| Music Artist | Sell beats & samples, tip streaming |
| Educator | Subscription-based course access, token-gated content |
| Street Vendor | QR code checkout for physical goods |
| SaaS Founder | Recurring billing in USDC via Stellar anchors |
| NGO / Creator | Community donations with on-chain transparency |

---

## 🔒 Security

- All payment flows are non-custodial — StellarCart never holds user funds
- Soroban contracts are open source and auditable
- Escrow dispute resolution is time-locked with on-chain arbitrator multisig
- API keys are never stored on-chain; agent context is scoped per session

---

## 🤝 Contributing

We welcome contributions! Please read [CONTRIBUTING.md](CONTRIBUTING.md) and open issues or PRs.

Key areas where contributions are welcome:
- New Soroban contract modules (loyalty points, auctions, bundles)
- Language support for the agent
- Additional wallet integrations
- SEP protocol implementations

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 🔗 Links

- [Stellar Developer Docs](https://developers.stellar.org)
- [Soroban Smart Contracts](https://soroban.stellar.org)
- [Stellar SEP Standards](https://github.com/stellar/stellar-protocol/tree/master/ecosystem)

---

*Built with ❤️ for the global creator economy on Stellar.*
