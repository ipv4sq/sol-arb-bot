# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Solana DEX arbitrage bot that performs circular arbitrage between WSOL and USDC using Jupiter v6 API. The bot is a proof-of-concept implementation that demonstrates MEV-protected arbitrage using Jito bundles.

## Development Commands

### Running the Bot
```bash
npx esrun src/index.ts
```

Note: This project uses `esrun` to execute TypeScript directly without a build step. There are no build, test, or lint commands configured.

### Missing Development Setup
The project lacks:
- TypeScript configuration (tsconfig.json)
- Build scripts
- Test framework
- Linting configuration

When implementing these, follow common Solana/TypeScript project patterns.

## Architecture

### Core Structure
- **Single-file implementation**: All logic resides in `src/index.ts`
- **Main loop**: Continuously monitors arbitrage opportunities with 200ms intervals
- **Transaction flow**: Compute units → Setup → Swap → Jito tip

### Key Dependencies
The project uses:
- `@solana/web3.js` for blockchain interaction
- `@solana/spl-token` for token operations
- Jupiter v6 API (self-hosted) for swap quotes
- Jito Block Engine for MEV protection

### Important Configuration
- RPC endpoint: `https://mainnet-ams.chainbuff.com`
- Jupiter API: `http://127.0.0.1:8080` (must be self-hosted with `--allow-circular-arbitrage`)
- Trading pair: WSOL/USDC only
- Trade amount: 0.01 WSOL
- Profit threshold: 3000 lamports

### Missing Dependencies
The code imports these packages that aren't in package.json:
- `dotenv`
- `bs58`

## Critical Implementation Notes

1. **Incomplete Program**: The bot references a custom Solana program for profit guarantees that doesn't exist. The `saveBalanceInstruction` function is a placeholder.

2. **Arbitrage Logic**: 
   - Performs WSOL → USDC → WSOL circular trades
   - Uses Address Lookup Tables (ALT) for transaction compression
   - Implements dynamic compute unit pricing

3. **Jito Integration**:
   - Tips 50% of calculated profit to Jito
   - Uses specific Jito tip accounts for different regions
   - Submits transactions as bundles for MEV protection

## Environment Setup

Create a `.env` file with:
```
SECRET_KEY=<base58_private_key>
```

## Known Limitations

- Only supports WSOL/USDC pair
- Hardcoded configuration values
- No error recovery mechanisms
- Limited to circular arbitrage strategy
- Requires self-hosted Jupiter v6 instance