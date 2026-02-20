# Mobula - Crypto Market Data & Wallet Intelligence

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-Skill-blue.svg)](https://openclaw.ai)

Connect your OpenClaw agent to the fastest multi-chain crypto data aggregator. Track portfolios, monitor whales, discover tokens autonomously, and get intelligent alerts across 88+ blockchains in real-time.

**What is Mobula?** The fastest and most accurate crypto data aggregator. Aggregates DEX and CEX data across 88+ chains with WebSocket support, sub-second latency, and oracle-grade pricing trusted by Chainlink, Supra, and API3. Powers 200M+ token coverage with real-time updates.

**What you can build:** 24/7 portfolio guardians that alert on concentration risks. Whale trackers that detect coordinated accumulation. Token scouts that autonomously discover gems matching your criteria. Smart alerts with multi-condition logic (price + volume + on-chain activity). All running proactively via OpenClaw's heartbeat system.

**Why this matters:** Unlike ChatGPT/Claude chat, your OpenClaw agent runs continuously. It monitors, analyzes, and alerts you on Telegram/WhatsApp/Discord without prompting. Set conditions once, let it work 24/7.

---

## Quick Start

### 1. Install the Skill

**Option A: Via ClawHub (recommended)**
```
Tell your agent: "Install the Mobula skill from ClawHub"
```

**Option B: Via URL**
```
Tell your agent: "Install skill from https://raw.githubusercontent.com/Flotapponnier/Crypto-date-openclaw/main/SKILL.md"
```

**Option C: Manual**
```bash
cd ~/openclaw/skills/
mkdir mobula && cd mobula
curl -o SKILL.md https://raw.githubusercontent.com/Flotapponnier/Crypto-date-openclaw/main/SKILL.md
```

### 2. Get API Key

1. Go to [mobula.io](https://mobula.io)
2. Sign up (free tier: 100 requests/min)
3. Copy API key from dashboard

### 3. Configure Environment

```bash
export MOBULA_API_KEY="your_api_key_here"
```

Make it permanent:
```bash
echo 'export MOBULA_API_KEY="your_key"' >> ~/.zshrc
source ~/.zshrc
```

### 4. Restart Agent

```bash
openclaw restart
```

### 5. Test

Ask your agent:
> "What's the price of Bitcoin?"

> "Show portfolio for wallet 0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb"

---

## Use Cases

### Portfolio Guardian
Monitor your wallet 24/7, get alerts on drops or concentration risks.

> "Monitor my wallet 0x... and alert me on Telegram if any token drops more than 15% or my allocation exceeds 40% on one asset. Daily summary at 9am."

**Example alert:**
```
Portfolio Alert

Your ETH allocation is now 47% (was 38% yesterday).
Consider rebalancing to reduce concentration risk.

Current holdings:
- ETH: $12,340 (47%)
- BRETT: $8,200 (31%) up 12% 24h
- DEGEN: $5,890 (22%) down 3% 24h
```

---

### Whale Tracker
Follow smart money, get alerted when whales move.

> "Watch wallets 0xabc, 0xdef, 0x123. Alert if any buy/sell more than $50K. If multiple whales buy the same token within 6h, priority alert with full analysis."

**Example alert:**
```
Whale Alert - HIGH PRIORITY

Wallet 0x742d... bought $150K of BRETT on Base (2h ago)

Token: BRETT | Price: $0.089 (up 23% 24h)
Mcap: $2.3M | Volume: $1.2M (8x normal)

Cross-signal: 2 other tracked whales also bought BRETT:
- 0x888... bought $80K (4h ago)
- 0x111... bought $120K (1h ago)

Possible coordinated accumulation.
```

---

### Token Scout (Autonomous Discovery)
Agent finds new tokens matching your criteria every X hours.

> "Find tokens on Base/Arbitrum every 6h: mcap under $5M, liquidity over $100K, volume up 50%+ 24h, verified contract. Send top 3 with analysis."

**Example alert:**
```
Token Scout - 3 New Matches

1. BOOP on Base
   - Price: $0.0042 (up 156% 24h, up 340% 7d)
   - Mcap: $2.1M | Liquidity: $280K
   - Volume: $890K (12x average)
   - Contract: Verified
   - Risk: Medium

2. ZORP on Arbitrum
   - Price: $0.0089 (up 78% 24h)
   - Mcap: $3.4M | Liquidity: $450K
   - Whale buy: $60K (3h ago)

3. [...]
```

---

### Smart Price Alerts
Contextual alerts based on multiple conditions (not just "price > X").

> "Alert if BTC moves more than 5% in 1h, BUT only if volume is 2x above 24h average. Real moves, not noise."

> "Alert if ETH breaks $4K after consolidating $3,800-$3,950 for 3+ days. That's a real breakout."

---

### Market Brief
Automated morning/evening market overviews.

> "Send market overview at 7am and 7pm on Telegram. Include BTC, ETH, SOL, and any top 100 token that moved more than 10%."

**Example brief:**
```
Crypto Market Brief - Feb 20, 7:00 AM

Majors:
- BTC: $67,234 (up 2.1% 24h)
- ETH: $3,456 (up 4.3% 24h)
- SOL: $123.45 (down 1.2% 24h)

Big Movers (24h):
- PEPE: up 23%
- ARB: up 15%
- AVAX: down 12%

Sentiment: Cautiously bullish. ETH leading, memes pumping.
```

---

## Templates

Ready-to-use heartbeat templates in [`examples/`](./examples):

- **[`heartbeat-portfolio-guardian.md`](./examples/heartbeat-portfolio-guardian.md)** - Monitor your wallet 24/7
- **[`heartbeat-whale-tracker.md`](./examples/heartbeat-whale-tracker.md)** - Follow smart money
- **[`heartbeat-token-scout.md`](./examples/heartbeat-token-scout.md)** - Autonomous token discovery
- **[`heartbeat-market-brief.md`](./examples/heartbeat-market-brief.md)** - Daily market summaries

**How to use:**
1. Copy template to `~/openclaw/heartbeat/`
2. Edit wallet addresses, criteria, timing
3. Agent executes automatically on schedule

---

## Documentation

- **[SKILL.md](./SKILL.md)** - Core skill file (what your agent reads)
- **[docs/SETUP.md](./docs/SETUP.md)** - Detailed installation guide
- **[docs/TROUBLESHOOTING.md](./docs/TROUBLESHOOTING.md)** - Common issues and fixes

---

## Supported Endpoints

| Endpoint | Purpose |
|----------|---------|
| `/market/data` | Price, volume, mcap for any token |
| `/wallet/portfolio` | Complete wallet holdings across all chains |
| `/market/history` | Historical price data |
| `/market/trades` | Live trade feed (whale watching) |
| `/wallet/transactions` | Transaction history |
| `/market/multi-data` | Batch queries (up to 500 tokens) |
| `/metadata` | Token info (website, socials, contract) |

**88+ blockchains:** Ethereum, Base, Arbitrum, Optimism, Polygon, BNB, Avalanche, Solana, and [more](https://docs.mobula.io/blockchains).

---

## How It Works

1. You give instructions in natural language to your agent
2. Agent reads SKILL.md to learn Mobula endpoints
3. On heartbeat (~30min intervals), agent checks monitoring tasks
4. When conditions met, fetches from Mobula API
5. Analyzes and alerts you on Telegram/WhatsApp/Discord/Slack

**The advantage:** Persistent, proactive monitoring. Your agent works 24/7 without prompting.

---

## Comparison

| Feature | CoinGecko/CMC | DexScreener | Mobula + OpenClaw |
|---------|---------------|-------------|-------------------|
| Price tracking | Yes | Yes | Yes |
| 24/7 AI monitoring | No | No | Yes |
| Contextual alerts | No | No | Yes |
| Whale tracking | No | No | Yes |
| Cross-chain portfolio | Basic | No | Yes |
| Autonomous discovery | No | No | Yes |
| Natural language | No | No | Yes |
| Custom logic | No | No | Yes |

---

## Rate Limits

- **Free tier:** 100 requests/minute (sufficient for most users)
- **Pro tier:** Higher limits at [mobula.io](https://mobula.io)

**Optimization tips:**
- Use `/market/multi-data` for batch queries (1 request for 500 tokens vs 500 separate)
- Cache metadata (rarely changes)
- Set heartbeat to 30-60min intervals

---

## Troubleshooting

**"API key not found" / 401 errors**
- Check: `echo $MOBULA_API_KEY`
- Set it: `export MOBULA_API_KEY="your_key"`
- Restart agent: `openclaw restart`

**Agent doesn't use the skill**
- Verify: `ls ~/openclaw/skills/mobula/SKILL.md`
- Be explicit: "Use Mobula to check Bitcoin price"
- Check logs: `openclaw logs | grep -i mobula`

**Rate limit (429) errors**
- Reduce heartbeat frequency (60min instead of 30min)
- Use batch endpoints
- Consider upgrading plan

See full guide: **[docs/TROUBLESHOOTING.md](./docs/TROUBLESHOOTING.md)**

---

## Feedback & Roadmap

This is v1.0 - we want your feedback.

**Tell us what you need:**
- [Report bugs](https://github.com/Flotapponnier/Crypto-date-openclaw/issues)
- [Request features](https://github.com/Flotapponnier/Crypto-date-openclaw/issues/new?template=feature_request.md)
- [Submit PRs](https://github.com/Flotapponnier/Crypto-date-openclaw/pulls)
- Share your heartbeat templates

**If this skill adds value, we're ready to ship more:**
- WebSocket streaming for instant alerts (no 30min heartbeat delay)
- NFT portfolio tracking and floor price monitoring
- DeFi position tracking (LP, lending, staking across protocols)
- On-chain contract verification and security scoring
- Custom webhook integrations
- Advanced pattern detection (smart money clusters, narrative tracking)

Let us know what matters most. [Open an issue](https://github.com/Flotapponnier/Crypto-date-openclaw/issues/new?template=feature_request.md) or reach out on [Mobula Discord](https://discord.gg/mobula).

---

## Resources

- **Mobula Website:** [mobula.io](https://mobula.io)
- **API Documentation:** [docs.mobula.io](https://docs.mobula.io)
- **OpenClaw Docs:** [openclaw.ai](https://openclaw.ai)
- **Supported Chains:** [docs.mobula.io/blockchains](https://docs.mobula.io/blockchains)

---

## License

MIT License - see [LICENSE](./LICENSE)

---

## Credits

- **Skill by:** [Mobula](https://mobula.io)
- **Built for:** [OpenClaw](https://openclaw.ai)
- **Coverage:** 88+ blockchains, 200M+ tokens

---

## Get Started

1. [Install the skill](#quick-start)
2. Try: "What's the price of Bitcoin?"
3. Set up monitoring: "Monitor my wallet 0x..."
4. Explore [templates](./examples)

Build your 24/7 crypto intelligence agent.
