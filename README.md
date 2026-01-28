# 🎯 Veiled Markets

<div align="center">

<img src="./logo-veiled-markets.png" alt="Veiled Markets Logo" width="200"/>

### **Predict Freely. Bet Privately.**

*The first privacy-preserving prediction market built on Aleo blockchain*

[![Aleo](https://img.shields.io/badge/Built%20on-Aleo-00D4AA?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48Y2lyY2xlIGN4PSIxMiIgY3k9IjEyIiByPSIxMCIgZmlsbD0iIzAwRDRBQSIvPjwvc3ZnPg==)](https://aleo.org)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](./LICENSE)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-3178C6?style=for-the-badge&logo=typescript)](https://typescriptlang.org)
[![React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react)](https://react.dev)

[Live Demo](https://veiled-markets.vercel.app) · [Documentation](./docs) · [Smart Contracts](./contracts) · [Blockchain Explorer](https://testnet.explorer.provable.com/program/veiled_markets.aleo)

</div>

---

## 🌟 Overview

**Veiled Markets** revolutionizes prediction markets by leveraging Aleo's zero-knowledge architecture to provide complete privacy for market participants. Unlike traditional prediction markets where all bets are visible on-chain, Veiled Markets ensures:

- 🔒 **Private Betting** — Your bet amount and position remain encrypted
- 🛡️ **MEV Protection** — No front-running or sandwich attacks possible
- 🎭 **Anonymous Participation** — Express your true beliefs without social pressure
- 📊 **Fair Markets** — Parimutuel pool system with transparent odds
- ✅ **Verifiable Outcomes** — All markets verifiable on-chain with transaction links
- 🔗 **Real Blockchain Data** — Live markets fetched directly from Aleo testnet

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         VEILED MARKETS PROTOCOL                          │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│   ┌──────────────┐     ┌──────────────┐     ┌──────────────┐           │
│   │   Frontend   │────▶│ Indexer API  │────▶│ Leo Contracts │           │
│   │   (React)    │     │ (TypeScript) │     │   (Aleo VM)   │           │
│   └──────────────┘     └──────────────┘     └──────────────┘           │
│          │                     │                     │                   │
│          │                     ▼                     ▼                   │
│          │            ┌──────────────┐    ┌──────────────┐              │
│          │            │ Market Index │    │ Market State │              │
│          │            │    (JSON)    │    │  (On-chain)  │              │
│          │            └──────────────┘    └──────────────┘              │
│          │                                        │                      │
│          └────────────────────────────────────────┴──────────────────┐  │
│                                                                       ▼  │
│                                                            ┌───────────┐ │
│                                                            │ User Bets │ │
│                                                            │ (Private) │ │
│                                                            └───────────┘ │
│                                                                          │
│   PUBLIC DATA:                        PRIVATE DATA:                      │
│   • Market question                   • Individual bet amounts           │
│   • Total pool size                   • User positions (Yes/No)          │
│   • Parimutuel odds                   • User identities                  │
│   • Resolution deadline               • Winning claims                   │
│   • Transaction IDs (verifiable)      • Bet history                      │
│   • Block heights                                                        │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

### Key Components

1. **Smart Contracts** (`contracts/src/main.leo`)
   - Parimutuel pool system for fair odds
   - Market creation, betting, resolution, and claiming
   - 2% total fees (1% protocol + 1% creator)

2. **Blockchain Indexer** (`backend/src/indexer.ts`)
   - Scans Aleo blockchain for market creation transactions
   - Auto-generates market registry (no hardcoded IDs)
   - Scalable solution for production deployment

3. **Frontend Application** (`frontend/src/`)
   - Real-time market data from blockchain
   - Wallet integration (Puzzle, Leo)
   - On-chain verification links for transparency

4. **SDK** (`sdk/src/`)
   - TypeScript client for contract interaction
   - Transaction building and signing
   - Type-safe API

## 🚀 Quick Start

### Prerequisites

- [Aleo SDK](https://developer.aleo.org/getting_started) (v1.0+)
- [Node.js](https://nodejs.org) (v18+)
- [pnpm](https://pnpm.io) (v8+)
- [Leo](https://developer.aleo.org/leo) (latest)

### Installation

```bash
# Clone the repository
git clone https://github.com/mdlog/veiled-markets.git
cd veiled-markets

# Install dependencies
pnpm install

# Setup environment variables
cp frontend/.env.example frontend/.env
# Edit frontend/.env with your configuration

# Build smart contracts
cd contracts
leo build

# Deploy contracts (optional - already deployed on testnet)
leo execute create_market "10001field" "1u8" "14107191u64" "14124471u64" --broadcast

# Start frontend development server
cd ../frontend
pnpm dev
```

### Deployed Contract

The contract is already deployed on Aleo Testnet:
- **Program ID**: `veiled_markets.aleo`
- **Deployment TX**: [at1j2f9r4mdls0n6k55nnscdckhuz7uyqfkuhj9kmer2v2hs6z0u5zsm8xf90](https://testnet.explorer.provable.com/transaction/at1j2f9r4mdls0n6k55nnscdckhuz7uyqfkuhj9kmer2v2hs6z0u5zsm8xf90)
- **Network**: Testnet
- **Explorer**: [View on Explorer](https://testnet.explorer.provable.com/program/veiled_markets.aleo)

### Live Markets

8 real markets are currently active across all categories:
1. **Politics**: Will Trump complete his full presidential term through 2028?
2. **Sports**: Will Lionel Messi win the 2026 FIFA World Cup with Argentina?
3. **Crypto**: Will Bitcoin reach $150,000 by end of Q1 2026?
4. **Entertainment**: Will Avatar 3 gross over $2 billion worldwide in 2026?
5. **Tech**: Will Apple release AR glasses (Apple Vision Pro 2) in 2026?
6. **Economics**: Will global inflation drop below 3% average by end of 2026?
7. **Science**: Will NASA Artemis III successfully land humans on Moon in 2026?

All markets are verifiable on-chain with transaction links!

## 📁 Project Structure

```
veiled-markets/
├── contracts/              # Leo smart contracts
│   ├── src/
│   │   └── main.leo       # Core prediction market logic (Parimutuel)
│   ├── build/             # Compiled Aleo instructions
│   ├── create-markets.sh  # Script to create markets via CLI
│   └── program.json       # Contract configuration
├── frontend/              # React frontend application
│   ├── src/
│   │   ├── components/    # UI components (MarketCard, MarketRow, etc.)
│   │   ├── lib/           # Utilities & SDK integration
│   │   │   ├── aleo-client.ts      # Blockchain client
│   │   │   ├── market-store.ts     # Real market data store
│   │   │   ├── wallet.ts           # Wallet adapters
│   │   │   └── store.ts            # Global state
│   │   ├── pages/         # Page components (Dashboard, MarketDetail)
│   │   └── styles/        # Global styles & theme
│   └── public/            # Static assets
├── backend/               # Blockchain indexer service
│   ├── src/
│   │   ├── indexer.ts     # Market indexer logic
│   │   ├── index-markets.ts # CLI script
│   │   └── config.ts      # Configuration
│   └── package.json
├── sdk/                   # TypeScript SDK
│   ├── src/
│   │   ├── client.ts      # Aleo client wrapper
│   │   ├── types.ts       # Type definitions
│   │   └── utils.ts       # Helper functions
│   └── package.json
├── scripts/               # Utility scripts
│   └── index-markets.sh   # Run blockchain indexer
├── docs/                  # Documentation
│   ├── ARCHITECTURE.md
│   ├── AMM_IMPLEMENTATION.md
│   ├── ON_CHAIN_VERIFICATION.md
│   ├── COPYABLE_MARKET_ID.md
│   └── PRIVACY_ANALYSIS.md
├── INDEXER_GUIDE.md       # Indexer service guide
└── README.md
```

## 🎯 Features

### For Market Creators
- ✅ Create binary (Yes/No) prediction markets
- ✅ Set betting and resolution deadlines (block height-based)
- ✅ Earn 1% creator fee from market activity
- ✅ Markets verifiable on blockchain explorer
- ✅ Support for 7 categories: Politics, Sports, Crypto, Entertainment, Tech, Economics, Science

### For Participants
- ✅ Place private bets without revealing position or amount
- ✅ View real-time parimutuel odds (fair pricing)
- ✅ See potential payout multipliers before betting
- ✅ Claim winnings privately after market resolution
- ✅ Copy market IDs with one click
- ✅ Verify markets on-chain via transaction links

### For Everyone
- ✅ Transparent market rules with private participation
- ✅ Parimutuel pool system (no AMM, no liquidity providers needed)
- ✅ 2% total fees (1% protocol + 1% creator)
- ✅ No counterparty risk — funds secured by smart contract
- ✅ Real-time data fetched from Aleo blockchain
- ✅ Auto-refresh every 30 seconds
- ✅ Block height-based time calculations

### New Features (Latest Update)

#### 🔗 On-Chain Verification
Every market card displays a "Verify On-Chain" button that links to the creation transaction on Aleo blockchain explorer. This proves markets are genuinely hosted on-chain, not mock data.

#### 📋 Copyable Market IDs
Market IDs are displayed in truncated format (e.g., `2226266059...41034862field`) with a copy button for easy sharing while keeping the UI clean.

#### 🔄 Blockchain Indexer
Automated service that scans the blockchain for market creation transactions, eliminating the need for hardcoded market IDs. Scalable solution for production deployment.

#### 📊 Real Market Data
All 8 markets fetch live data from Aleo testnet:
- Real pool sizes
- Actual bet counts
- Live block heights
- Accurate time remaining

## 🔐 Privacy Model

| Data Type | Visibility | Description |
|-----------|------------|-------------|
| Market Question | 🌍 Public | Everyone can see what's being predicted |
| Total Pool Size | 🌍 Public | Aggregate betting volume is visible |
| Parimutuel Odds | 🌍 Public | Fair odds based on pool distribution |
| Transaction IDs | 🌍 Public | Verifiable on blockchain explorer |
| Block Heights | 🌍 Public | Transparent deadline tracking |
| Your Bet Amount | 🔒 Private | Only you know how much you bet |
| Your Position | 🔒 Private | Only you know if you bet Yes/No |
| Your Identity | 🔒 Private | Bets are not linked to your address |
| Winnings Claim | 🔒 Private | Claim without revealing how much you won |

### Parimutuel Pool System

Unlike AMM-based prediction markets, Veiled Markets uses a **parimutuel pool system**:

- All bets go into YES and NO pools
- Winners split the total pool proportionally
- No liquidity providers needed
- Fair odds determined by market participants
- Formula: `Payout = (Total Pool / Winning Pool) × (1 - Fees)`

**Example:**
- YES pool: 1,000 ALEO
- NO pool: 500 ALEO
- Total: 1,500 ALEO
- If YES wins: Each 1 ALEO bet on YES returns `(1,500 / 1,000) × 0.98 = 1.47 ALEO`
- Potential multiplier: **1.47x**

## 🛠️ Technology Stack

### Smart Contracts
- **Leo** - Aleo's ZK programming language
- **Aleo VM** - Zero-knowledge virtual machine
- **Parimutuel Logic** - Fair odds calculation

### Frontend
- **React 18** - UI framework
- **TypeScript** - Type safety
- **Vite** - Build tool
- **Tailwind CSS** - Styling
- **Framer Motion** - Animations
- **Zustand** - State management
- **React Router** - Navigation

### Backend
- **TypeScript** - Indexer service
- **Node.js** - Runtime
- **Aleo SDK** - Blockchain interaction

### Infrastructure
- **Vercel** - Frontend hosting
- **Aleo Testnet** - Blockchain network
- **GitHub Actions** - CI/CD (optional)

## 💼 Supported Wallets

| Wallet | Status | Features | Notes |
|--------|--------|----------|-------|
| 🧩 **Puzzle Wallet** | ✅ Fully Supported | Full integration, balance display | Recommended |
| 🦁 **Leo Wallet** | ⚠️ Limited | Basic integration | SDK compatibility issues |
| 🎮 **Demo Mode** | ✅ Available | Test without real wallet | For development |

### Wallet Features
- ✅ **Real-time balance updates** (public + private credits)
- ✅ **Transaction signing** via wallet extension
- ✅ **Network switching** (testnet/mainnet)
- ✅ **Address display** with copy functionality
- ⚠️ **Record decryption** (limited by wallet SDK)

### Known Issues
- Leo Wallet returns generic errors due to SDK incompatibility
- Puzzle Wallet requires specific network names: `AleoTestnet`, `AleoMainnet`
- Private balance reading may be limited by wallet capabilities

See [WALLET_TROUBLESHOOTING.md](./WALLET_TROUBLESHOOTING.md) for detailed solutions.

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

The indexer generates `frontend/public/markets-index.json`:

```json
{
  "lastUpdated": "2026-01-28T10:30:00.000Z",
  "totalMarkets": 8,
  "marketIds": ["...", "..."],
  "markets": [
    {
      "marketId": "...",
      "transactionId": "at1...",
      "questionHash": "10001field",
      "category": 1,
      "deadline": "14107191u64",
      "blockHeight": 14067123
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

### Creating Markets via CLI

```bash
cd contracts

# Create a single market
leo execute create_market \
  "10001field" \      # question_hash
  "1u8" \             # category (1=Politics)
  "14107191u64" \     # betting_deadline (block height)
  "14124471u64" \     # resolution_deadline (block height)
  --broadcast

# Create multiple markets
./create-markets.sh
```

### Testing

```bash
# Frontend tests
cd frontend
pnpm test

# Contract tests
cd contracts
leo test

# E2E tests (if available)
pnpm test:e2e
```

### Environment Variables

Create `frontend/.env`:

```env
VITE_NETWORK=testnet
VITE_PROGRAM_ID=veiled_markets.aleo
VITE_ALEO_RPC_URL=https://api.explorer.provable.com/v1/testnet
VITE_EXPLORER_URL=https://testnet.explorer.provable.com
```

## 📚 Documentation

- [Architecture Overview](./docs/ARCHITECTURE.md)
- [Parimutuel System](./docs/PARIMUTUEL_SYSTEM.md)
- [Privacy Analysis](./docs/PRIVACY_ANALYSIS.md)
- [On-Chain Verification](./docs/ON_CHAIN_VERIFICATION.md)
- [Copyable Market ID](./docs/COPYABLE_MARKET_ID.md)
- [Indexer Guide](./INDEXER_GUIDE.md)
- [Wallet Troubleshooting](./WALLET_TROUBLESHOOTING.md)
- [Real Data Integration](./REAL_DATA_INTEGRATION.md)

## 📜 License

MIT License — see [LICENSE](./LICENSE) for details.

## 🙏 Acknowledgments

- **Aleo Team** - For the amazing zero-knowledge blockchain platform
- **Leo Language** - For making ZK programming accessible
- **Community** - For feedback and contributions

## 🤝 Contributing

Contributions are welcome! Please read our [Contributing Guide](./CONTRIBUTING.md) for details.

### Development Workflow

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 🐛 Bug Reports

Found a bug? Please open an issue with:
- Description of the bug
- Steps to reproduce
- Expected vs actual behavior
- Screenshots (if applicable)
- Environment details (OS, browser, wallet)

## 💬 Community & Support

- **GitHub Issues** - Bug reports and feature requests
- **GitHub Discussions** - General questions and ideas
- **Documentation** - Check the [docs](./docs) folder

---

<div align="center">

**Built with 💜 for the Aleo Ecosystem**

[Live Demo](https://veiled-markets.vercel.app) · [Blockchain Explorer](https://testnet.explorer.provable.com/program/veiled_markets.aleo) · [GitHub](https://github.com/mdlog/veiled-markets)

**Contract Address**: `veiled_markets.aleo` on Aleo Testnet

</div>

