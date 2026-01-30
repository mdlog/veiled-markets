# 🎯 Veiled Markets

<div align="center">

<img src="./logo-veiled-markets.png" alt="Veiled Markets Logo" width="200"/>

### **Predict Freely. Bet Privately.**

Privacy-preserving prediction market built on Aleo blockchain

[![Live Demo](https://img.shields.io/badge/Live-Demo-00D4AA?style=for-the-badge)](https://veiled-markets.vercel.app)
[![Aleo](https://img.shields.io/badge/Aleo-Testnet-00D4AA?style=for-the-badge)](https://testnet.explorer.provable.com/program/veiled_markets.aleo)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](./LICENSE)

</div>

---

## 🌟 What is Veiled Markets?

A prediction market platform where you can bet on future events with complete privacy. Built on Aleo's zero-knowledge blockchain:

- 🔒 **Private Bets** — Your position and amount stay encrypted
- 📊 **Fair Odds** — Parimutuel pool system (no AMM needed)
- ✅ **Verifiable** — All markets proven on-chain
- 🔗 **Real Data** — Live from Aleo testnet

## 🏗️ Architecture

```
Frontend (React + TypeScript)
    ↓
Aleo Wallet (Puzzle/Leo)
    ↓
Smart Contract (veiled_markets.aleo)
    ↓
Aleo Blockchain (Testnet)
```

### Key Components

- **Smart Contract** (`contracts/src/main.leo`) - Parimutuel betting logic with 2% fees
- **Frontend** (`frontend/src/`) - React app with wallet integration
- **Indexer** (`backend/src/`) - Scans blockchain for markets
- **SDK** (`sdk/src/`) - TypeScript client library

## 🚀 Quick Start

```bash
# Clone and install
git clone https://github.com/mdlog/veiled-markets.git
cd veiled-markets
pnpm install

# Setup environment
cp frontend/.env.example frontend/.env

# Start development
pnpm dev
# Open http://localhost:5173
```

**Live App:** [veiled-markets.vercel.app](https://veiled-markets.vercel.app)

**Contract:** `veiled_markets.aleo` on [Aleo Testnet](https://testnet.explorer.provable.com/program/veiled_markets.aleo)

## 📁 Project Structure

```
veiled-markets/
├── frontend/          # React app
│   ├── src/
│   │   ├── components/    # UI components
│   │   ├── lib/           # Utils & SDK
│   │   ├── pages/         # Routes
│   │   └── styles/        # CSS
│   └── public/            # Static files
├── contracts/         # Leo smart contracts
│   └── src/main.leo   # Core logic
├── backend/           # Blockchain indexer
├── sdk/              # TypeScript SDK
└── docs/             # Documentation
```

## 🎯 Features

### Privacy Model

| Data | Visibility |
|------|-----------|
| Market question, total pool, odds | 🌍 Public |
| Your bet amount & position | 🔒 Private |
| Your identity & winnings | 🔒 Private |

### Parimutuel System

Winners split the total pool proportionally:

```
Payout = (Total Pool / Winning Pool) × (1 - 2% fees)
```

**Example:** If YES pool = 1,000 ALEO and NO pool = 500 ALEO, and YES wins:
- Each 1 ALEO bet returns: `(1,500 / 1,000) × 0.98 = 1.47 ALEO`
- Multiplier: **1.47x**

## 🛠️ Tech Stack

- **Frontend:** React 18, TypeScript, Vite, Tailwind CSS
- **Blockchain:** Leo (Aleo), Aleo SDK
- **State:** Zustand
- **Wallet:** Puzzle/Leo wallet adaptors
- **Hosting:** Vercel

## 💼 Wallet Setup

**Supported:** Puzzle Wallet (recommended) | Leo Wallet

1. Install wallet extension from [puzzle.online](https://puzzle.online) or [leo.app](https://leo.app)
2. Switch to **Testnet** network
3. Get test credits from [Aleo Faucet](https://faucet.aleo.org)
4. Connect wallet in app

See [WALLET_TROUBLESHOOTING.md](./WALLET_TROUBLESHOOTING.md) for issues.

## 🔄 Blockchain Indexer

The indexer service automatically scans the Aleo blockchain for market creation transactions, eliminating the need for hardcoded market IDs.

### Running the Indexer

```bash
# Install backend dependencies
cd backend
npm install

# Run indexer
npm run index

# Or use the helper script
cd ..
./scripts/index-markets.sh
```

### Output

The indexer generates `backend/public/markets-index.json` and copies it to `frontend/public/markets-index.json`:

```json
{
  "lastUpdated": "2026-01-28T15:40:51.456Z",
  "totalMarkets": 9,
  "marketIds": ["...", "..."],
  "markets": [
    {
      "marketId": "3582024152336217571382682973364798990155453514672503623063651091171230848724field",
      "transactionId": "at1crl3gd6ukawwrslf3r5vqttg7a8hll84fj2klqtmtwdafntspg9sgcgw2a",
      "creator": "aleo10tm5ektsr5v7kdc5phs8pha42vrkhe2rlxfl2v979wunhzx07vpqnqplv8",
      "questionHash": "350929565016816493992297964402345071115472527106339097957348390879136520853field",
      "category": 3,
      "deadline": "14107320u64",
      "resolutionDeadline": "14124600u64",
      "createdAt": 1769614851455,
      "blockHeight": 14067000
    }
  ]
}
```

### Benefits

- ✅ **No hardcoded IDs** - Markets discovered automatically
- ✅ **Scalable** - Handles thousands of markets
- ✅ **Production-ready** - Can be run as cron job
- ✅ **Verifiable** - All data from blockchain

See [INDEXER_GUIDE.md](./INDEXER_GUIDE.md) for detailed documentation.

## 🧪 Development

### Commands

```bash
pnpm dev              # Start dev server
pnpm build            # Build for production
pnpm test             # Run tests
```

### Create a Market

```bash
# 1. Generate question hash
node scripts/generate-question-hash.js "Your question?"

# 2. Create on blockchain
cd contracts
leo execute create_market \
  "HASH_field" \
  "3u8" \              # category (0-6)
  "14107320u64" \      # betting deadline
  "14124600u64" \      # resolution deadline
  --broadcast

# 3. Run indexer
cd ../backend
npm run index
```

### Environment Variables

Copy `frontend/.env.example` to `frontend/.env`:

```env
VITE_NETWORK=testnet
VITE_PROGRAM_ID=veiled_markets.aleo
VITE_ALEO_RPC_URL=https://api.explorer.provable.com/v1/testnet
```

## 📚 Documentation

- [Architecture Overview](./docs/ARCHITECTURE.md)
- [Parimutuel System](./docs/PARIMUTUEL_SYSTEM.md)
- [Privacy Analysis](./docs/PRIVACY_ANALYSIS.md)
- [On-Chain Verification](./docs/ON_CHAIN_VERIFICATION.md)
- [Copyable Market ID](./docs/COPYABLE_MARKET_ID.md)
- [Indexer Guide](./INDEXER_GUIDE.md)
- [Create Market Guide](./CREATE_MARKET_GUIDE.md)
- [Wallet Troubleshooting](./WALLET_TROUBLESHOOTING.md)
- [Real Data Integration](./REAL_DATA_INTEGRATION.md)

## 📚 Documentation

- [CREATE_MARKET_GUIDE.md](./CREATE_MARKET_GUIDE.md) - How to create markets
- [INDEXER_GUIDE.md](./INDEXER_GUIDE.md) - Blockchain indexer setup
- [WALLET_TROUBLESHOOTING.md](./WALLET_TROUBLESHOOTING.md) - Wallet issues
- [REAL_DATA_INTEGRATION.md](./REAL_DATA_INTEGRATION.md) - Data integration
- [docs/](./docs) - Architecture & privacy analysis

## 🤝 Contributing

1. Fork the repo
2. Create feature branch (`git checkout -b feature/name`)
3. Commit changes (`git commit -m 'Add feature'`)
4. Push to branch (`git push origin feature/name`)
5. Open Pull Request

## 📜 License

MIT License - see [LICENSE](./LICENSE)

---

<div align="center">

**Built with 💜 on Aleo**

[Live Demo](https://veiled-markets.vercel.app) · [Contract](https://testnet.explorer.provable.com/program/veiled_markets.aleo) · [GitHub](https://github.com/mdlog/veiled-markets)

</div>

