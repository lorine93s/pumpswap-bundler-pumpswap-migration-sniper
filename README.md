# PumpSwap Bundler — Migration Sniper & Automation Suite

Automate trading on PumpSwap with a high‑performance toolkit: migration sniping, bundling, copy trading, optimized swaps, LP creation, and volume orchestration. Built for Solana traders and memecoin hunters who need speed, reliability, and precise control.

## Key Features

- **Migration Sniper**: Detects tokens migrating from pump.fun to PumpSwap and buys instantly with configurable slippage, priority fees, and spend limits.
- **Bundler**: Executes multiple actions in a single atomic transaction (buy + add LP + set sell order) to reduce fees and race conditions.
- **Copy Trading**: Mirrors selected wallets with filters for token age, liquidity, and risk thresholds.
- **Swap Engine**: Fast, optimized route selection for swaps on PumpSwap with custom slippage and retry strategies.
- **Liquidity Tools**: Create and manage LPs with sane defaults and safety checks.
- **Volume Bot**: Generate controlled volume patterns for liquidity bootstrapping and signal strategies.

## Tech Stack

- **Blockchain**: Solana
- **Smart Contracts**: Rust, Anchor (optional, for auxiliary programs)
- **Backend/Bot**: Node.js, TypeScript
- **Networking**: Solana RPC and gRPC data feeds

---

## Support
- Telegram: `@lorine93s`
- Open an issue on GitHub

---

## Getting Started

### Prerequisites

- Node.js 18+ and npm/yarn/pnpm
- A funded Solana wallet (preferably a dedicated hot wallet)
- Access to a reliable Solana RPC endpoint (paid recommended)

### Install

```bash
git clone https://github.com/<your-org>/pumpswap-bundler.git
cd pumpswap-bundler
npm install # or: yarn install / pnpm install
```

### Configure Environment

Create a `.env` file at the project root:

```bash
# RPC
SOLANA_RPC_URL=https://<your-rpc-endpoint>
SOLANA_WS_URL=wss://<your-ws-endpoint> # optional; will derive if omitted

# Wallet & Keys
WALLET_SECRET_KEY=<base58_or_json_array_secret>
WALLET_ADDRESS=<public_key_optional_if_derivable>

# Trading
DEFAULT_SLIPPAGE_BPS=150             # 1.50%
PRIORITY_FEE_MICROLAMPORTS=5000      # tune based on network congestion
MAX_SPEND_SOL=0.5                     # safety limit per buy

# Strategy Toggles
ENABLE_MIGRATION_SNIPER=true
ENABLE_COPY_TRADER=false
ENABLE_VOLUME_BOT=false

# Copy‑Trading
FOLLOWED_WALLETS=wallet1,wallet2
MIN_LIQUIDITY_SOL=5
MAX_TOKEN_AGE_MIN=120

# Volume Bot
VOLUME_TARGET_MCAP_USD=200000
VOLUME_TXS_PER_MIN=6
```

Environment notes:

- Use distinct wallets for development, testing, and production.
- Prefer non-custodial secrets management (e.g., OS keychain, KMS, Vault). `.env` is for local only.

---

### Usage

Common scripts (adjust names to your `package.json`):

```bash
# Start the migration sniper
npm run start:sniper

# Start the bundler (multi‑action executor)
npm run start:bundler

# Run the swap engine with CLI args
npm run start:swap -- \
  --mint-in <MINT_IN> \
  --mint-out <MINT_OUT> \
  --amount 0.2 \
  --slippage-bps 100

# Create LP for a token pair
npm run start:lp -- --token <MINT> --sol-amount 1 --token-amount 10000

# Start copy trading
npm run start:copy
```

Example minimal `package.json` scripts:

```json
{
  "scripts": {
    "build": "tsc -p .",
    "start:sniper": "ts-node src/runners/migrationSniper.ts",
    "start:bundler": "ts-node src/runners/bundler.ts",
    "start:swap": "ts-node src/runners/swap.ts",
    "start:lp": "ts-node src/runners/liquidity.ts",
    "start:copy": "ts-node src/runners/copyTrader.ts"
  }
}
```

---

### Architecture

- **Detectors**: Listen to PumpSwap indexers, gRPC feeds, and program logs to identify migrations and liquidity events.
- **Executors**: Compose and submit Solana transactions with deterministic compute budgets and priority fees.
- **Strategies**: Pluggable modules for sniping, copy trading, bundling, and volume shaping.
- **Risk & Safety**: Guardrails for max spend, slippage, token filters, liquidity thresholds, and age limits.

Folder hints (subject to your codebase):

- `src/detectors/` — on‑chain and indexer listeners (migrations, listings, LP changes)
- `src/executors/` — transaction building and submission
- `src/strategies/` — sniper, bundler, copy trading, volume
- `src/runners/` — CLI entrypoints for each capability
- `src/utils/` — RPC, math, pricing, token utils

---

### Security & Risk

- Trading bots carry financial risk. Only use funds you can afford to lose.
- Always test with devnet or small amounts on mainnet first.
- Never commit private keys. Consider using a dedicated, limited‑fund wallet.
- Prefer rate‑limited, private RPC endpoints to avoid frontrunning and throttling.

---

### Roadmap

- Adaptive slippage and priority fee based on live network conditions
- Better route discovery and JIT price impact modeling for swaps
- Enhanced copy‑trading heuristics and wallet scoring
- Optional GUI dashboard for monitoring and kill‑switch

---

### Contributing

Contributions are welcome! Please open an issue to discuss significant changes before submitting a PR.

