# 🔮 Madame Satoshi's Bitcoin Oracle

A Lightning-native Bitcoin tarot bot for Telegram. Pay 21 sats, draw 3 cards, receive your Bitcoin fortune — seeded by the Bitcoin blockchain.

> **Status: Work in progress — actively under development**

## Contents
- [What It Does](#what-it-does)
- [How It Works](#how-it-works)
- [Prize Pool](#prize-pool)
- [Provably Fair](#provably-fair)
- [Tech Stack](#tech-stack)
- [Self-Sovereign Stack](#self-sovereign-stack)
- [Setup](#setup)
- [Environment Variables](#environment-variables)
- [Admin Commands](#admin-commands)
- [Roadmap](#roadmap)
- [Known Issues](#known-issues)
- [License](#license)

## What It Does

Madame Satoshi's Bitcoin Oracle is a Telegram bot that combines tarot card reading with a Bitcoin Lightning prize pool. Each draw costs 21 sats, deducted from your in-bot balance. Three cards are drawn from the 22 Major Arcana and interpreted as Past, Present, and Future — with a Bitcoin-themed fortune reading.

Every draw is seeded by the latest Bitcoin block hash, making it provably fair and verifiable on-chain.

## How It Works

- Deposit sats via Lightning invoice
- Draw 3 tarot cards for 21 sats
- Receive an animated card reveal + Bitcoin fortune
- Win sats from the prize pool on matching combinations
- Withdraw your balance anytime via LNURL

## Prize Pool

| Tier | Cards | Odds | Prize |
|------|-------|------|-------|
| 🏆 JACKPOT | The Sun + The World + The Magician (any order) | 6 in 9,240 | 100% of pool |
| 🥇 MAJOR WIN | Any 3 cards of the same suit | 66 in 9,240 | ~35% of pool |
| 🥈 MINOR WIN | Any 2 cards of the same suit | 666 in 9,240 | ~15% of pool |

Card order matters — draws use permutations (P(22,3) = 9,240), not combinations.

## Provably Fair

Each draw is seeded using:
```
SHA256(blockHash + userId + timestamp)
```

Where `blockHash` is fetched from a local Bitcoin node via the Mempool API at the moment of each draw.

Users can run `/verify` in the bot to retrieve:
- Bitcoin block height and full block hash
- The seed used for their draw
- Cards drawn and timestamp
- Direct link to the block on mempool.space

## Tech Stack

- **Node.js** — bot runtime
- **node-telegram-bot-api** — Telegram integration
- **LNbits** — Lightning wallet management
- **LND** — Lightning Network node
- **sharp + ffmpeg** — animated card reveal generation
- **Bitcoin Core + Mempool** — block data and Lightning routing
- **Docker** — Lightning stack containerization

## Self-Sovereign Stack

Runs entirely on self-hosted infrastructure:
- Bitcoin Core (full node)
- LND (Lightning node)
- LNbits (wallet API)
- Mempool (block explorer + API)
- Telegram bot (systemd service)

No third-party Lightning custody. No KYC. No ads.

## Setup

1. Clone the repo
2. Copy `.env.example` to `.env` and fill in your values
3. Install dependencies: `npm install`
4. Ensure LNbits and Mempool are running and accessible
5. Start the bot: `node bot.js` or use the included systemd service

## Environment Variables

See `.env.example` for required configuration:
- Telegram bot token (from @BotFather)
- LNbits URL and wallet API keys
- Mempool API URL
- Admin Telegram user ID
- Draw cost in sats

## Admin Commands

The bot includes admin-only commands accessible via Telegram:
- `/msstats` — full dashboard
- `/msuser <id>` — user lookup
- `/mslogs` — recent draw history
- `/msbroadcast <msg>` — message all users
- `/msdrain` — wallet management
- `/msblock` — current block info
- `/mshelp` — command list

## Roadmap

- [ ] Website with live jackpot display
- [ ] Nostr integration with zap support
- [ ] Wallet Connect / NWC
- [ ] Sponsor spots on website
- [ ] Full open source verification tooling

## License

GPL v3 — see LICENSE file.

## Try It

[@BitcoinTarotBot](https://t.me/bitcointarotbot) on Telegram

---

*Stack sats. Trust the cards. Have fun.* ⚡

## Known Issues
- npm audit reports vulnerabilities in node-telegram-bot-api's 
  deprecated `request` dependency. These have no available fix 
  upstream. A future version will migrate to a maintained library.