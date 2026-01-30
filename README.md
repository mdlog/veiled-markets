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
- [pnpm](https://pnpm.io) (v8.15.9+)
- [Leo](https://developer.aleo.org/leo) (latest)
- Aleo wallet extension ([Puzzle Wallet](https://puzzle.online) or [Leo Wallet](https://leo.app))

### Installation

```bash
# Clone the repository
git clone https://github.com/mdlog/veiled-markets.git
cd veiled-markets

# Install dependencies (uses pnpm workspaces)
pnpm install

# Setup environment variables
cp frontend/.env.example frontend/.env
# Edit frontend/.env with your configuration

# Build smart contracts (optional - already deployed)
cd contracts
leo build

# Start frontend development server
cd ../frontend
pnpm dev

# Open browser at http://localhost:5173
```

### Quick Deploy to Vercel

The project is configured for one-click deployment to Vercel:

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Or deploy to production
vercel --prod
```

The `vercel.json` is configured to:
- Use npm for installation (compatible with Vercel)
- Build from the frontend directory
- Output to `dist` folder
- Use Vite framework

### Deployed Contract

The contract is already deployed on Aleo Testnet:
- **Program ID**: `veiled_markets.aleo`
- **Network**: Testnet
- **Explorer**: [View on Explorer](https://testnet.explorer.provable.com/program/veiled_markets.aleo)
- **RPC Endpoint**: `https://api.explorer.provable.com/v1/testnet`

### Live Application

- **Production URL**: [https://veiled-markets.vercel.app](https://veiled-markets.vercel.app)
- **Status**: ✅ Live on Vercel
- **Auto-deploy**: Enabled on `main` branch push
- **Framework**: Vite + React 18 + TypeScript

### Live Markets

9 real markets are currently active across all categories:
1. **Crypto**: Will Bitcoin reach $100,000 by end of Q1 2026?
2. **Politics**: Will Trump win the 2024 US Presidential Election?
3. **Sports**: Will Lakers win NBA Championship 2026?
4. **Crypto**: Will Ethereum reach $5,000 by March 2026?
5. **Entertainment**: Will Taylor Swift release a new album in 2026?
6. **Tech**: Will Apple release AR glasses in 2026?
7. **Economics**: Will US Fed cut interest rates in Q1 2026?
8. **Science**: Will SpaceX land on Mars by 2030?
9. **Crypto**: Will Ethereum reach $10,000 by end of Q2 2026? ⭐ NEW

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
│   │   │   ├── question-mapping.ts # Question hash to text mapping
│   │   │   ├── wallet.ts           # Wallet adapters
│   │   │   └── store.ts            # Global state
│   │   ├── pages/         # Page components (Dashboard, MarketDetail)
│   │   └── styles/        # Global styles & theme
│   └── public/            # Static assets
│       └── markets-index.json # Indexed markets from blockchain
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
│   ├── index-markets.sh   # Run blockchain indexer
│   └── generate-question-hash.js # Generate SHA-256 hashes for questions
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

#### 🎯 Dynamic Question Mapping System
All market questions are now stored in localStorage and managed dynamically through the question-mapping system. No more hardcoded questions in the codebase!

- Questions mapped by SHA-256 hash (converted to Aleo decimal format)
- Automatic initialization on app startup
- Easy to add new markets without code changes
- Fallback to hash preview if question not found

#### 🔗 On-Chain Verification
Every market card displays a "Verify On-Chain" button that links to the creation transaction on Aleo blockchain explorer. This proves markets are genuinely hosted on-chain, not mock data.

#### 📋 Copyable Market IDs
Market IDs are displayed in truncated format (e.g., `2226266059...41034862field`) with a copy button for easy sharing while keeping the UI clean.

#### 🔄 Blockchain Indexer
Automated service that scans the blockchain for market creation transactions, eliminating the need for hardcoded market IDs. Scalable solution for production deployment.

#### 📊 Real Market Data
All 9 markets fetch live data from Aleo testnet:
- Real pool sizes
- Actual bet counts
- Live block heights
- Accurate time remaining
- Dynamic question text from localStorage

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
- **Parimutuel Logic** - Fair odds calculation system

### Frontend
- **React 18** - Modern UI framework with hooks
- **TypeScript 5.3** - Type-safe development
- **Vite 5** - Lightning-fast build tool
- **Tailwind CSS 3.4** - Utility-first styling
- **Framer Motion 11** - Smooth animations
- **Zustand 4.4** - Lightweight state management
- **React Router 6** - Client-side routing
- **Radix UI** - Accessible component primitives
- **Lucide React** - Beautiful icon library

### Blockchain Integration
- **@provablehq/sdk 0.6** - Aleo SDK for blockchain interaction
- **Aleo Wallet Adaptors 0.3** - Multi-wallet support
- **@puzzlehq/sdk 1.0** - Puzzle Wallet integration

### Backend/Indexer
- **TypeScript** - Type-safe indexer service
- **Node.js 18+** - Runtime environment
- **Aleo SDK** - Blockchain data fetching

### Infrastructure
- **Vercel** - Frontend hosting with auto-deploy
- **Aleo Testnet** - Blockchain network
- **GitHub** - Version control & CI/CD
- **pnpm 8.15** - Fast, disk-efficient package manager

### Development Tools
- **ESLint** - Code linting
- **PostCSS** - CSS processing
- **Autoprefixer** - CSS vendor prefixes

## 💼 Supported Wallets

| Wallet | Status | Features | Notes |
|--------|--------|----------|-------|
| 🧩 **Puzzle Wallet** | ✅ Fully Supported | Full integration, balance display, transaction signing | **Recommended** - Best compatibility |
| 🦁 **Leo Wallet** | ✅ Supported | Full integration, transaction signing | Works with latest SDK |

### Wallet Features
- ✅ **Real-time balance updates** (public + private credits)
- ✅ **Transaction signing** via wallet extension
- ✅ **Network switching** (testnet/mainnet)
- ✅ **Address display** with copy functionality
- ✅ **Secure connection** with wallet encryption
- ✅ **Auto-reconnect** on page refresh

### Setup Instructions

1. **Install Wallet Extension**
   - Puzzle Wallet: [puzzle.online](https://puzzle.online)
   - Leo Wallet: [leo.app](https://leo.app)

2. **Create/Import Account**
   - Generate new account or import existing
   - Switch to **Testnet** network

3. **Get Test Credits**
   - Visit [Aleo Faucet](https://faucet.aleo.org)
   - Request testnet credits to your address

4. **Connect to Veiled Markets**
   - Click "Connect Wallet" button
   - Select your wallet
   - Approve connection request

### Important Notes
- ⚠️ Demo Mode has been removed - real wallet required
- ⚠️ Puzzle Wallet requires network names: `AleoTestnet`, `AleoMainnet`
- ⚠️ Private balance reading may be limited by wallet capabilities
- ⚠️ Always verify you're on **Testnet** before transactions

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

### Project Structure

The project uses **pnpm workspaces** for monorepo management:

```
veiled-markets/
├── frontend/          # React application (main workspace)
├── backend/           # Blockchain indexer service
├── sdk/              # TypeScript SDK (workspace package)
├── contracts/        # Leo smart contracts
└── scripts/          # Utility scripts
```

### Development Commands

```bash
# Start frontend dev server
pnpm dev

# Build frontend for production
pnpm build

# Build all workspaces (SDK + contracts + frontend)
pnpm build:all

# Run tests
pnpm test              # SDK tests only
pnpm test:all          # SDK + contract tests

# Lint code
pnpm lint

# Clean build artifacts
pnpm clean
```

### Creating Markets via CLI

```bash
cd contracts

# 1. Generate question hash first
node ../scripts/generate-question-hash.js "Will Ethereum reach $10,000 by end of Q2 2026?"
# Output: 350929565016816493992297964402345071115472527106339097957348390879136520853field

# 2. Create market on blockchain
leo execute create_market \
  "350929565016816493992297964402345071115472527106339097957348390879136520853field" \
  "3u8" \              # category: 0=Politics, 1=Sports, 2=Entertainment, 3=Crypto, 4=Tech, 5=Economics, 6=Science
  "14107320u64" \      # betting_deadline (block height)
  "14124600u64" \      # resolution_deadline (block height)
  --broadcast

# 3. Run indexer to discover new market
cd ../backend
npm run index

# 4. Add question mapping (frontend/src/lib/question-mapping.ts)
# See "Adding Question Mapping" section below
```

### Batch Market Creation

```bash
cd contracts

# Create multiple markets at once
./create-markets.sh

# Or use the interactive script
./create-new-market.sh
```

### Adding Question Mapping

After creating a market, add the question mapping to `frontend/src/lib/question-mapping.ts`:

```typescript
export function initializeQuestionMappings(): void {
    const mappings: Record<string, string> = {
        // ... existing mappings ...
        
        // Your new market
        '350929565016816493992297964402345071115472527106339097957348390879136520853field':
            'Will Ethereum reach $10,000 by end of Q2 2026?',
    };
    // ...
}
```

The question will automatically appear in the dashboard after refresh!

### Testing

```bash
# Frontend tests (if available)
cd frontend
pnpm test

# Contract tests
cd contracts
leo test

# SDK tests
cd sdk
pnpm test

# Run all tests
pnpm test:all
```

### Deployment

#### Deploy to Vercel

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy to preview
vercel

# Deploy to production
vercel --prod
```

#### Deploy Smart Contracts

```bash
cd contracts

# Build contract
leo build

# Deploy to testnet
leo deploy --network testnet --broadcast

# Deploy to mainnet (requires mainnet credits)
leo deploy --network mainnet --broadcast
```

### Vercel Configuration

The project includes `vercel.json` with optimized settings:

```json
{
  "installCommand": "npm install --include=dev",
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "framework": "vite"
}
```

**Key Points:**
- Uses npm (not pnpm) for Vercel compatibility
- Builds from frontend directory
- Outputs to `dist` folder
- Auto-deploys on push to `main` branch

### Environment Variables

Create `frontend/.env` from the example:

```bash
cp frontend/.env.example frontend/.env
```

Key configuration options:

```env
# Network Configuration
VITE_NETWORK=testnet                                    # testnet | mainnet
VITE_ALEO_RPC_URL=https://api.explorer.provable.com/v1/testnet
VITE_EXPLORER_URL=https://testnet.explorer.provable.com

# Program Configuration
VITE_PROGRAM_ID=veiled_markets.aleo                     # Deployed contract
VITE_CREDITS_PROGRAM_ID=credits.aleo                    # Aleo credits program

# Wallet Configuration
VITE_ENABLE_DEMO_MODE=false                             # Demo mode disabled
VITE_DEFAULT_WALLET=puzzle                              # puzzle | leo

# Feature Flags
VITE_ENABLE_CREATE_MARKET=true                          # Allow market creation
VITE_ENABLE_BETTING=true                                # Allow betting
VITE_SHOW_TESTNET_BANNER=true                           # Show testnet warning
VITE_DEBUG=false                                        # Debug logging

# App Metadata
VITE_APP_NAME=Veiled Markets
VITE_APP_DESCRIPTION=Privacy-Preserving Prediction Markets on Aleo
VITE_APP_URL=https://veiled-markets.vercel.app
```

**⚠️ Security Notes:**
- Never commit `.env` files to git
- Never use real private keys in `VITE_DEV_*` variables
- Development keys are exposed to browser - only for local testing
- Use environment variables in Vercel dashboard for production

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

