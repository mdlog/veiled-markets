# Veiled Markets

<div align="center">

<img src="./logo-veiled-markets.png" alt="Veiled Markets Logo" width="200"/>

### **Predict Freely. Bet Privately.**

Privacy-preserving prediction market + turbo markets on Aleo blockchain

[![Live Demo](https://img.shields.io/badge/Live-Demo-00D4AA?style=for-the-badge)](https://veiledmarkets.xyz)
[![Aleo](https://img.shields.io/badge/Aleo-Testnet-00D4AA?style=for-the-badge)](https://testnet.explorer.provable.com/program/veiled_markets_v37.aleo)
[![npm](https://img.shields.io/badge/SDK-npm-CB3837?style=for-the-badge)](https://www.npmjs.com/package/@veiled-markets/sdk)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](./LICENSE)

</div>

---

## Overview

Veiled Markets is a dual-mode prediction market protocol on Aleo:

1. **Standard Markets** — Long-duration prediction markets (hours to months) with FPMM AMM, multi-voter quorum resolution, and tiered governance escalation. Supports ALEO, USDCX, and USAD tokens.

2. **Turbo Markets** — Short-duration (5-minute) UP/DOWN crypto price markets resolved automatically by Pyth Network oracle. Shared vault architecture — operator deposits ALEO once, all markets draw from a single pool. Rolling chain: each market's closing price becomes the next market's baseline.

All trading is **fully private** via Aleo zero-knowledge proofs.

## Deployed Contracts

| Contract | Program ID | Transitions | Purpose |
|----------|-----------|-------------|---------|
| **Main** | [`veiled_markets_v37.aleo`](https://testnet.explorer.provable.com/program/veiled_markets_v37.aleo) | 25/31 | ALEO standard markets, multi-voter resolution, multisig treasury |
| **USDCX** | [`veiled_markets_usdcx_v7.aleo`](https://testnet.explorer.provable.com/program/veiled_markets_usdcx_v7.aleo) | 25/31 | USDCX stablecoin markets |
| **USAD** | [`veiled_markets_usad_v14.aleo`](https://testnet.explorer.provable.com/program/veiled_markets_usad_v14.aleo) | 25/31 | USAD stablecoin markets |
| **Governance** | [`veiled_governance_v6.aleo`](https://testnet.explorer.provable.com/program/veiled_governance_v6.aleo) | 31/31 | Proposals, tiered escalation, committee panel, cross-program resolve |
| **Turbo** | [`veiled_turbo_v8.aleo`](https://testnet.explorer.provable.com/program/veiled_turbo_v8.aleo) | 10/31 | 5-min UP/DOWN markets, shared vault, Pyth-resolved, 10 crypto symbols |
| **Parlay** | [`veiled_parlay_v3.aleo`](https://testnet.explorer.provable.com/program/veiled_parlay_v3.aleo) | — | Multi-leg parlay betting |

**Turbo deployment date:** 2026-04-10 (v8 shared vault architecture)
**Standard deployment date:** 2026-04-08 (post-audit hardening)

## Turbo Markets

5-minute UP/DOWN crypto price prediction markets powered by Pyth Network oracle with automatic resolution and rolling chain architecture.

### How it works

```
T+0:00  Backend creates BTC market (baseline = $72,150)
        ├─ No per-market LP needed — shared vault backs all payouts
        ├─ Users bet UP or DOWN with private ALEO credits
        └─ Live Pyth price chart streams in browser

T+5:00  Deadline reached → chart freezes → "RESOLVING..."
        ├─ Backend snapshots Pyth closing price ($72,180)
        ├─ Submits resolve_turbo_market tx to Aleo
        └─ Contract: $72,180 > $72,150 → UP wins

T+5:45  Resolved → "↑ UP" result splash (5 seconds)
        ├─ Winners can claim from Portfolio page
        └─ Closing price ($72,180) becomes next market's baseline

T+6:00  New market auto-created (baseline = $72,180)
        └─ Continuous rolling chain — no operator intervention
```

### Supported symbols (10)

BTC, ETH, SOL, DOGE, XRP, BNB, ADA, AVAX, LINK, DOT — all with Pyth Hermes real-time feeds.

### Shared vault architecture (v8)

| Aspect | Standard Markets (v37) | Turbo Markets (v8) |
|--------|----------------------|-------------------|
| Liquidity | Per-market FPMM pool | **Shared vault** — deposit once |
| Create cost | ~10+ ALEO per market | **~1 ALEO gas only** |
| Pool model | Constant product AMM | **Parimutuel** (1:1 shares) |
| Resolution | Multi-voter quorum (24h) | **Pyth oracle (~1 min)** |
| Duration | Hours → months | **5 minutes** |
| Token | ALEO / USDCX / USAD | **ALEO** (multi-token planned) |
| Privacy | Full private trading | **Full private trading** |

### Turbo transitions

| Transition | Purpose |
|-----------|---------|
| `deposit_vault` / `deposit_vault_public` | Operator funds shared vault |
| `withdraw_vault` | Operator reclaims from vault |
| `create_turbo_market` | Create 5-min market (no LP needed) |
| `buy_up_down` | Private bet with credits record |
| `resolve_turbo_market` | Oracle resolves with Pyth closing price |
| `emergency_cancel` | Permissionless refund if oracle offline |
| `claim_winnings` | Winner claims from vault (private payout) |
| `claim_refund` | Refund after cancel (private payout) |
| `withdraw_fees` | Operator withdraws protocol fees |

### Trust model

- **Data source:** Pyth Network (90+ publishers, publicly verifiable)
- **Authorization:** Caller-based — only `ORACLE_OPERATOR` wallet can create/resolve
- **On-chain enforcement:** Window (60 blocks), range, confidence (0.5%), max-move (50%) sanity checks
- **Escape hatch:** `emergency_cancel` permissionless after grace period → full refund
- **Audit trail:** All on-chain, cross-checkable against Pyth Hermes historical data

See [contracts-turbo-v1/THREAT_MODEL.md](contracts-turbo-v1/THREAT_MODEL.md) for the full 23-threat catalog.

### Running turbo markets

```bash
# 1. Start operator backend (creates + resolves markets automatically)
cd backend
set -a && source ../.env && set +a
TURBO_SYMBOLS=BTC npx tsx src/pyth-oracle.ts --serve --auto-create

# 2. Frontend (turbo markets appear in dashboard hero + /turbo/btc page)
cd frontend
npm run dev
# Open http://localhost:3000/dashboard or http://localhost:3000/turbo/btc
```

## SDK

Published on npm: [`@veiled-markets/sdk@0.5.4`](https://www.npmjs.com/package/@veiled-markets/sdk)

```bash
npm install @veiled-markets/sdk @provablehq/sdk
```

Full TypeScript SDK for building on the Veiled Markets protocol:

- **6 typed clients** — `VeiledMarketsClient`, `TurboClient`, `VeiledGovernanceClient`, `ParlayClient`, `IndexerClient`, `PythHermesClient`
- **3 wallet adapters** — Shield, Puzzle, Leo (browser) + auto-detection
- **Node.js executor** — `NodeExecutor` wraps `snarkos developer execute` for backend bots
- **Off-chain quoting** — exact mirrors of on-chain `*_fin` math (FPMM, parimutuel, fees, LP, disputes)
- **191 unit tests** — math, executor, indexer, parlay, turbo, governance, wallets
- **Dual ESM + CJS** — works in Vite, Next.js, esbuild, and legacy Node CommonJS

Quick example:

```ts
import { createTurboClient, quoteBuyUpDown } from '@veiled-markets/sdk'

const turbo = createTurboClient({ network: 'testnet' })
const market = await turbo.getCurrentMarket('BTC')
const quote = quoteBuyUpDown({ pool: await turbo.getPool(market.market_id), side: 'UP', amountMicro: 1_000_000n })
```

See [sdk/README.md](sdk/README.md) for full API docs, 6 usage examples, and configuration.

### Telegram bot (SDK integration test)

Live bot: [@veiledmarkets_bot](https://t.me/veiledmarkets_bot)

The [`bot-test/`](bot-test/) folder contains a multi-user Telegram bot that exercises the SDK end-to-end — both Turbo and FPMM markets. Auto-generates encrypted wallets per user, no setup required for end users.

```bash
cd bot-test && npm install && npx tsx smoke.ts  # 24/24 SDK checks pass
cp .env.example .env                             # fill TELEGRAM_BOT_TOKEN + WALLET_ENCRYPTION_KEY
npx tsx bot.ts                                   # start bot
```

**Turbo commands:** `/price BTC`, `/market BTC`, `/quote BTC UP 1`, `/bet BTC UP 0.5`, `/watch BTC`
**FPMM commands:** `/markets`, `/marketinfo <ID>`, `/buy <ID> <1-4> <AMT>`, `/redeem <ID>`
**Wallet commands:** `/wallet`, `/fund <AMT>`, `/mybets`, `/result`, `/claim`

Features:
- Multi-user: auto-generates encrypted Aleo wallet per Telegram user (AES-256-GCM)
- Auto-capture: share records + change records saved automatically after each bet
- Both market types: Turbo (5-min UP/DOWN) and FPMM (prediction markets) in one bot

See [bot-test/README.md](bot-test/README.md) for full setup instructions.

## Standard Markets

Long-duration prediction markets with FPMM AMM, multi-voter quorum resolution, tri-token support, and tiered governance escalation. Full details below.

### Key features
- **Fully Private Trading** — All buy/sell/redeem use private records
- **Tri-Token Support** — ALEO, USDCX, USAD stablecoins
- **Multi-Outcome Markets** — 2-, 3-, and 4-outcome with custom labels
- **FPMM AMM** — Constant product invariant (0.5% protocol + 0.5% creator + 1% LP = 2% total)
- **Multi-Voter Quorum Resolution** — Min 3 voters with ALEO bond, dispute window, slashing
- **Tiered Dispute Escalation** — Committee (tier 2) → community (tier 3) → cross-program governance
- **Multi-Sig Treasury** — 2-of-3 or 3-of-3 multisig (configurable threshold) for protocol fund withdrawals

### Market resolution flow

```
Market Deadline → close_market → CLOSED
        ↓
Anyone: vote_outcome(outcome, 1 ALEO bond)  →  PENDING_RESOLUTION
        ↓  (min 3 voters, ~3.2h voting window)
Anyone: finalize_votes()  →  PENDING_FINALIZATION
        ↓
Dispute window (~3.2h)
        │
        ├─── No dispute → confirm_resolution() → RESOLVED
        │    Winners: bond back + share of loser bonds
        │
        └─── Dispute filed (3× bond) → STATUS_DISPUTED
                      ↓
              Tier 2 (Committee) or Tier 3 (Community)
                      ↓
              governance_resolve_* → apply_governance_resolution
                      ↓
              STATUS_RESOLVED (claims unlocked)
```

## Architecture

```
┌────────────────────┐     ┌──────────────────┐     ┌────────────────────────────────┐
│   Frontend         │────▶│  Aleo Wallets    │────▶│   Aleo Testnet                  │
│   React 18 / Vite  │     │  Shield · Puzzle │     │                                │
│   TypeScript       │     │  Leo · Fox       │     │  veiled_markets_v37.aleo        │
│   Tailwind CSS     │     └──────────────────┘     │  veiled_markets_usdcx_v7.aleo   │
│                    │                              │  veiled_markets_usad_v14.aleo    │
│  Pages:            │     ┌──────────────────┐     │  veiled_governance_v6.aleo       │
│  - Dashboard       │────▶│  Pyth Hermes SSE │     │  veiled_turbo_v8.aleo           │
│  - /turbo/:symbol  │     │  (10 crypto feeds)│     │  veiled_parlay_v3.aleo          │
│  - MarketDetail    │     └──────────────────┘     │                                │
│  - Portfolio       │                              │  Dependencies:                  │
│  - Governance      │     ┌──────────────────┐     │  ├─ credits.aleo               │
│  - Create Market   │────▶│  Supabase        │     │  ├─ test_usdcx_stablecoin      │
│  - Verify Turbo    │     │  (encrypted)     │     │  └─ test_usad_stablecoin       │
│                    │     └──────────────────┘     │                                │
│                    │                              └────────────────────────────────┘
│                    │     ┌──────────────────┐
│                    │     │  Backend Services │
│                    │────▶│  - pyth-oracle.ts │  ← Turbo rolling chain + Pyth SSE
│                    │     │  - indexer.ts     │  ← Market discovery
│                    │     │  - dispute-idx.ts │  ← Dispute scanner
│                    │     │  - gov-indexer.ts │  ← Governance scanner
│                    │     └──────────────────┘
└────────────────────┘
┌────────────────────┐     ┌──────────────────┐
│ @veiled-markets/sdk│────▶│  npm registry    │  ← v0.5.4 published
│ 6 clients, 191 tests     │  npmjs.com       │
├────────────────────┤     └──────────────────┘
│ bot-test/          │
│ Telegram bot       │────▶ SDK integration test via chat commands
└────────────────────┘
```

## Project Structure

```
veiled-markets/
├── contracts-v37/              # ALEO standard market (25 transitions)
├── contracts-usdcx-v7/         # USDCX market (25 transitions)
├── contracts-usad-v14/         # USAD market (25 transitions)
├── contracts-governance-v6/    # Governance (31 transitions)
├── contracts-turbo-v1/         # Turbo markets source (deployed as v8)
│   ├── src/main.leo            # ~23 KB, 10 transitions, shared vault (veiled_turbo_v8.aleo)
│   ├── tests/                  # 16 logic tests
│   ├── THREAT_MODEL.md         # 23 threats + testnet validation log
│   ├── MAINNET_MIGRATION.md    # Block-time retuning guide
│   └── SHADOW_RUN.md           # 24h shadow run runbook
├── contracts-parlay/           # Parlay multi-leg betting
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── TurboMarketPanel.tsx   # Live price chart + buy UP/DOWN
│   │   │   ├── TurboRollingView.tsx   # Rolling market state machine
│   │   │   ├── DashboardHero.tsx      # Hero section (turbo + standard)
│   │   │   └── ClaimWinningsModal.tsx  # Claim for both standard + turbo
│   │   ├── lib/
│   │   │   ├── turbo-client.ts        # Turbo tx builders + RPC helpers
│   │   │   ├── aleo-client.ts         # Standard market client
│   │   │   └── store.ts              # Zustand stores
│   │   └── pages/
│   │       ├── Dashboard.tsx          # Hero with turbo + market grid
│   │       ├── TurboDetail.tsx        # /turbo/:symbol rolling view
│   │       ├── Turbo.tsx              # /turbo index
│   │       ├── VerifyTurbo.tsx        # /verify/turbo/:id
│   │       └── MyBets.tsx             # Portfolio (standard + turbo bets)
│   └── public/
├── backend/
│   └── src/
│       ├── pyth-oracle.ts        # Pyth SSE + rolling chain + vault + HTTP
│       ├── indexer.ts            # Standard market discovery
│       ├── dispute-indexer.ts    # Dispute scanner
│       └── governance-indexer.ts # Governance scanner
├── sdk/                        # TypeScript SDK (published: @veiled-markets/sdk@0.5.4)
│   ├── src/
│   │   ├── client.ts              # VeiledMarketsClient (FPMM markets)
│   │   ├── turbo-client.ts        # TurboClient (5-min UP/DOWN)
│   │   ├── governance-client.ts   # VeiledGovernanceClient (proposals)
│   │   ├── parlay.ts + parlay-client.ts  # Parlay math + client
│   │   ├── indexer.ts             # Supabase off-chain query layer
│   │   ├── pyth-client.ts         # Pyth Hermes verification
│   │   ├── executor.ts            # NodeExecutor (snarkos wrapper)
│   │   ├── wallets/               # Shield, Puzzle, Leo adapters
│   │   └── __tests__/             # 191 unit tests (8 files)
│   ├── examples/                  # turbo-bot, market-dashboard, governance-monitor
│   ├── dist/                      # ESM + CJS + .d.ts (built by tsup)
│   └── package.json               # v0.5.4, npm published
├── bot-test/                   # Telegram bot (multi-user, SDK integration)
│   ├── bot.ts                     # Turbo + FPMM commands, multi-user wallets
│   ├── wallets.ts                 # Per-user wallet manager (AES-256-GCM encrypted)
│   ├── records.ts                 # Credits record scanner + auto-capture
│   ├── smoke.ts                   # 24-check SDK install verification
│   └── README.md                  # Step-by-step setup guide
├── supabase/
│   ├── create_turbo_audit_table.sql  # Turbo oracle audit trail
│   └── *.sql                         # Standard market schemas
└── docs/
```

## Privacy Model

| Token | Buy Method | Privacy |
|-------|-----------|---------|
| **ALEO** | `credits.aleo/transfer_private_to_public` | All inputs encrypted |
| **USDCX** | `test_usdcx_stablecoin.aleo/transfer_private_to_public` | Token + MerkleProof encrypted |
| **USAD** | `test_usad_stablecoin.aleo/transfer_private_to_public` | Token + MerkleProof encrypted |
| **Turbo** | `credits.aleo/transfer_private_to_public` | Private bet, private payout |

## Testing

```bash
# Standard market tests (15)
cd contracts-v37 && leo test

# Governance tests (28)
cd contracts-governance-v6 && leo test --no-local

# Turbo market tests (16)
cd contracts-turbo-v1 && leo test

# SDK tests (191)
cd sdk && npm test

# SDK smoke test (24 checks — verifies npm install works)
cd bot-test && npx tsx smoke.ts

# Frontend type check
cd frontend && npx tsc --noEmit

# Backend smoke test
cd backend && DRY_RUN=1 TURBO_SYMBOLS=BTC npx tsx src/pyth-oracle.ts --auto-create
```

**Total: 250 tests** (15 standard + 28 governance + 16 turbo + 191 SDK)

All contract tests use `assert_eq(actual, expected)` with exact computed values — not stubs. SDK tests mirror on-chain `*_fin` math to guarantee off-chain quote = on-chain settlement.

## Quick Start

```bash
git clone https://github.com/mdlog/veiled-markets.git
cd veiled-markets

# Frontend
cd frontend && npm install --legacy-peer-deps && cp .env.example .env
# Edit .env with your keys
npm run dev

# Turbo backend (separate terminal)
cd backend && npm install
set -a && source ../.env && set +a
TURBO_SYMBOLS=BTC npx tsx src/pyth-oracle.ts --serve --auto-create
```

**Wallet:** Install [Shield Wallet](https://shieldwallet.io/), switch to Testnet, get credits from [Aleo Faucet](https://faucet.aleo.org).

## Environment Variables

See [`frontend/.env.example`](frontend/.env.example) for the full list. Core values:

```env
# Network
VITE_NETWORK=testnet
VITE_ALEO_RPC_URL=https://api.explorer.provable.com/v1/testnet
VITE_EXPLORER_URL=https://testnet.explorer.provable.com
VITE_ALEO_SECONDS_PER_BLOCK=4

# Standard market contracts (program IDs)
VITE_PROGRAM_ID=veiled_markets_v37.aleo                 # ALEO market
VITE_USDCX_MARKET_PROGRAM_ID=veiled_markets_usdcx_v7.aleo
VITE_USAD_PROGRAM_ID=veiled_markets_usad_v14.aleo
VITE_GOVERNANCE_PROGRAM_ID=veiled_governance_v6.aleo
VITE_PARLAY_PROGRAM_ID=veiled_parlay_v3.aleo

# Token programs (used for private transfers)
VITE_CREDITS_PROGRAM_ID=credits.aleo
VITE_USDCX_PROGRAM_ID=test_usdcx_stablecoin.aleo
# USAD token program is built into the market contract

# Turbo markets
VITE_TURBO_PROGRAM_ID=veiled_turbo_v8.aleo
# URL of the Pyth oracle backend (pyth-oracle.ts) — frontend subscribes to
# SSE price stream and polls /chain/symbol from here. Point this at your
# local backend in dev; production deploys a public endpoint.
VITE_TURBO_ORACLE_URL=http://localhost:4090

# Persistence & off-chain data
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
VITE_PINATA_JWT=your-pinata-jwt
# VITE_PINATA_GATEWAY=https://gateway.pinata.cloud

# Feature flags & UX
VITE_DEFAULT_WALLET=puzzle              # puzzle | leo | fox | shield | soter
VITE_ENABLE_DEMO_MODE=true              # mock wallet for testing without real creds
VITE_ENABLE_CREATE_MARKET=true
VITE_ENABLE_BETTING=true
VITE_SHOW_TESTNET_BANNER=true
VITE_DEBUG=false
```

Backend (`backend/pyth-oracle.ts`) additionally reads: `OPERATOR_ADDRESS`, `OPERATOR_PRIVATE_KEY`, `TURBO_SYMBOLS` (comma-separated subset to enable), `DRY_RUN`, `ALEO_SECONDS_PER_BLOCK`, and `SUPABASE_URL` / `SUPABASE_ANON_KEY` for audit logging.

## Turbo Testnet Validation History

The turbo contract went through 8 iterative deploys to fix bugs discovered during live testnet testing:

| Version | Key Fix |
|---------|---------|
| v1 | Initial deploy — `baseline_block == current` impossible |
| v2 | Baseline lag tolerance (30 blocks) |
| v3 | Resolution window widened (10 → 60 blocks) |
| v4 | Branch-free abs diff (ternary underflow fix) |
| v5 | Private transfers (credits record in/out) |
| v6 | `withdraw_liquidity` for LP recovery |
| v7 | Shared vault + 10 symbols + claim refund validation |
| **v8** | **`deposit_vault_public` + all bug fixes consolidated** |

See [contracts-turbo-v1/THREAT_MODEL.md](contracts-turbo-v1/THREAT_MODEL.md) for the full threat catalog and testnet validation log.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Contracts** | Leo 4.0, snarkVM, snarkOS |
| **Frontend** | React 18, TypeScript, Vite 5, Tailwind CSS, Framer Motion |
| **SDK** | [`@veiled-markets/sdk`](https://www.npmjs.com/package/@veiled-markets/sdk) — TypeScript, tsup (ESM+CJS), vitest (191 tests) |
| **State** | Zustand |
| **Charts** | Recharts + Canvas (turbo live chart) |
| **Price Oracle** | Pyth Network Hermes SSE (turbo markets) |
| **Wallet** | ProvableHQ Aleo Wallet Adapter (Shield, Puzzle, Leo, Fox, Soter) |
| **Persistence** | Supabase (AES-256-GCM) + localStorage |
| **Metadata** | IPFS via Pinata |
| **Hosting** | Vercel (frontend), npm (SDK) |

## Release History

| Version | Status | Highlights |
|---------|--------|-----------|
| **SDK v0.5.4** (active) | Published 2026-04-13 | 6 clients, 191 tests, struct parsing fix, outcomeLabels in enriched markets |
| **Telegram bot** (active) | Updated 2026-04-13 | Multi-user wallets (AES-256-GCM), Turbo + FPMM markets, auto-capture records, `/fund`, `/claim` |
| **Turbo v8** (active) | Deployed 2026-04-10 | Shared vault, 10 symbols, private transfers, parimutuel model, rolling chain, Pyth oracle |
| **v37 / v7 / v14 / v6** (active) | Deployed 2026-04-08 | Post-audit hardening: Bug A/B/C/D fixes + `assert_disputed` guard |
| **v36 / v6 / v13 / v5** (legacy) | Replaced 2026-04-08 | First v6 dispute architecture |

## Contributing

1. Fork the repo
2. Create feature branch (`git checkout -b feature/name`)
3. Commit changes and open Pull Request

## License

MIT License — see [LICENSE](./LICENSE)

---

<div align="center">

**Built on Aleo**

[Live Demo](https://veiledmarkets.xyz) · [Telegram Bot](https://t.me/veiledmarkets_bot) · [SDK on npm](https://www.npmjs.com/package/@veiled-markets/sdk) · [Standard Markets](https://testnet.explorer.provable.com/program/veiled_markets_v37.aleo) · [Turbo Markets](https://testnet.explorer.provable.com/program/veiled_turbo_v8.aleo) · [Governance](https://testnet.explorer.provable.com/program/veiled_governance_v6.aleo)

</div>
