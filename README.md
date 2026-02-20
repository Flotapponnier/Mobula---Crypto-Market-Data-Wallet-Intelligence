# 🦞 Mobula Skill for OpenClaw

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-Skill-blue.svg)](https://openclaw.ai)

Transform your OpenClaw agent into a **24/7 crypto analyst** with real-time data from **88+ blockchains**.

---

## 🚀 What is This?

A skill for [OpenClaw](https://openclaw.ai) that gives your AI agent access to [Mobula's](https://mobula.io) comprehensive crypto data API. Your agent can now:

- 📊 Track any token's price, volume, and market cap across 88 chains
- 💼 Monitor any wallet's portfolio in real-time
- 🐋 Watch whale wallets and get alerted on significant moves
- 🔍 Autonomously discover tokens matching your criteria
- 📈 Analyze historical price data and trends
- ⚡ Get live trade feeds from all DEXs
- 🔔 Set up intelligent, contextual alerts (not just "price > X")

**The difference:** Unlike ChatGPT or Claude chat, OpenClaw runs 24/7 with a heartbeat system. Your agent proactively monitors, analyzes, and alerts you on Telegram/WhatsApp/Discord without you asking.

---

## ⚡ Quick Start

### 1. Install the Skill

**Option A: Via ClawHub (easiest)**
```
Tell your OpenClaw agent: "Install the Mobula skill from ClawHub"
```

**Option B: Via URL**
```
Tell your agent: "Install skill from https://raw.githubusercontent.com/Flotapponnier/Crypto-date-openclaw/main/SKILL.md"
```

**Option C: Manual**
```bash
cd ~/openclaw/skills/
mkdir mobula
cd mobula
curl -o SKILL.md https://raw.githubusercontent.com/Flotapponnier/Crypto-date-openclaw/main/SKILL.md
```

### 2. Get Your Mobula API Key

1. Go to [admin.mobula.fi](https://admin.mobula.fi)
2. Sign up (free tier available)
3. Copy your API key

### 3. Set the Environment Variable

```bash
export MOBULA_API_KEY="your_api_key_here"
```

Or add it to your shell config (`~/.zshrc`, `~/.bashrc`):
```bash
echo 'export MOBULA_API_KEY="your_key"' >> ~/.zshrc
source ~/.zshrc
```

### 4. Restart Your Agent

```bash
openclaw restart
```

### 5. Test It

Say to your agent:
> "What's the price of Bitcoin?"

> "Show me the portfolio for wallet 0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb"

> "Find tokens on Base under $5M market cap with high volume"

---

## 💡 What You Can Build

### 1. Portfolio Guardian

Your agent monitors your wallet 24/7 and alerts you proactively.

**Setup:**
> "Monitor my wallet 0x... and alert me on Telegram if:
> - Any token drops more than 15% in 24h
> - My allocation on one token exceeds 40% of my portfolio
> - Send me a daily summary at 9am"

**What happens:**
- Every 30 minutes (heartbeat), agent checks your wallet
- Calculates allocations, tracks price changes
- Alerts you only when conditions are met
- Keeps historical data to show trends

**Example alert:**
> ⚠️ Portfolio Alert
>
> Your ETH allocation is now **47%** (was 38% yesterday).
>
> Current holdings:
> - ETH: $12,340 (47%)
> - BRETT: $8,200 (31%) ↑12% 24h
> - DEGEN: $5,890 (22%) ↓3% 24h
>
> Consider rebalancing to reduce concentration risk.

---

### 2. Whale Tracker

Follow smart money and get alerted when they make moves.

**Setup:**
> "Watch these wallets:
> - 0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb
> - 0x8888887dF37c0A1e7C2A3b8f0c8b2D8e7c6B5A4D
> - 0x1111117dF37c0A1e7C2A3b8f0c8b2D8e7c6B5A4D
>
> Alert me if any of them buy or sell more than $50K of anything. If multiple whales buy the same token within 6 hours, that's priority - give me full analysis."

**What happens:**
- Agent checks these wallets every heartbeat (~30min)
- Detects new transactions
- When large trade found → analyzes the token they bought
- Cross-references with other tracked wallets
- Alerts with full context

**Example alert:**
> 🐋 Whale Alert - HIGH PRIORITY
>
> **Wallet 0x742d...** bought **$150,000 of BRETT** on Base (2 hours ago)
>
> Token Analysis:
> - Price: $0.089 (↑23% 24h)
> - Mcap: $2.3M | Liquidity: $340K
> - Volume: $1.2M (up 8x vs yesterday)
>
> **Cross-signal:** 2 other tracked whales also bought BRETT in the last 6h:
> - 0x888... bought $80K (4 hours ago)
> - 0x111... bought $120K (1 hour ago)
>
> ⚡ This could be a coordinated accumulation.

---

### 3. Token Scout (Autonomous Discovery)

Your agent finds new tokens for you based on your criteria - without you asking.

**Setup:**
> "Find me tokens on Base and Arbitrum every 6 hours with these criteria:
> - Market cap: under $5M
> - Liquidity: over $100K
> - Volume: up 50%+ in 24h
> - Contract: must be verified
>
> Send me the top 3 matches with analysis."

**What happens:**
- Every 6 hours, agent scans tokens across specified chains
- Filters by your criteria
- Analyzes each match (price trends, holder distribution, risk factors)
- Ranks by potential
- Sends you the best opportunities

**Example alert:**
> 🔍 Token Scout - 3 New Matches
>
> **1. BOOP** on Base
> - Price: $0.0042 (↑156% 24h, ↑340% 7d)
> - Mcap: $2.1M | Liquidity: $280K
> - Volume 24h: $890K (12x vs average)
> - Contract: Verified ✓
> - Chart: 7-day uptrend, broke resistance at $0.0035
> - Risk: Medium (good liquidity for size)
> - [Dexscreener](link) | [Contract](link)
>
> **2. ZORP** on Arbitrum
> - Price: $0.0089 (↑78% 24h)
> - Mcap: $3.4M | Liquidity: $450K
> - Volume: $1.1M
> - Contract: Verified ✓
> - Recent whale buy: $60K (3h ago)
> - [Links...]
>
> **3. [...]**

---

### 4. Smart Price Alerts

Not just "alert when price > X" - **contextual alerts** based on multiple conditions.

**Setup:**
> "Alert me if Bitcoin moves more than 5% in 1 hour, BUT only if trading volume is 2x above the 24h average. That means it's a significant move, not just noise."

**What happens:**
- Agent checks BTC price every heartbeat
- Compares to 1h ago
- Also checks current volume vs 24h average
- Only alerts if BOTH conditions met

This filters out low-volume wicks and only alerts on real, significant moves.

**Another example:**
> "Alert me if ETH breaks $4,000, but only if it's been consolidating between $3,800-$3,950 for at least 3 days before. That's a real breakout."

---

### 5. Multi-Wallet Portfolio Tracker

Track multiple wallets (yours, family, DAOs, etc.) in one dashboard.

**Setup:**
> "Track these wallets and send me a daily summary at 8am:
> - My main: 0x123...
> - My degen: 0x456...
> - My cold storage: 0x789...
>
> Show total value across all, best/worst performers, and any big moves."

**Example morning brief:**
> ☀️ Portfolio Summary - Feb 20, 2024
>
> **Total Value:** $47,320 (↑$2,140 / +4.7% 24h)
>
> **By Wallet:**
> - Main: $28,900 (+3.2%)
> - Degen: $15,200 (+12.4%) 🔥
> - Cold: $3,220 (-0.8%)
>
> **Top Performers:**
> 1. BRETT: +23% ($1,240 profit)
> 2. DEGEN: +15% ($890 profit)
>
> **Alerts:**
> - Your degen wallet is up 12% - mostly from BRETT pump
> - PEPE in main wallet down 8%, but still +45% overall
>
> Have a great day!

---

### 6. Arbitrage Spotter

Find price differences across chains.

**Setup:**
> "Check if USDC or USDT have significant price differences across Ethereum, Base, and Arbitrum. Alert me if the spread is >0.3% and arbitrage is profitable after gas."

**Example alert:**
> 💰 Arbitrage Opportunity
>
> **USDC** spread detected:
> - Arbitrum: $1.0035
> - Base: $0.9978
> - Spread: **0.57%**
>
> Estimated profit on $10K: ~$57
> Gas fees: ~$8 (Base) + $12 (Arbitrum)
> **Net profit: ~$37**
>
> Liquidity check: ✓ Sufficient on both chains

---

### 7. DeFi Position Monitor

Track your LP positions and lending positions.

**Setup:**
> "I have LP positions on Uniswap (pool WETH/USDC on Arbitrum). Monitor for:
> - Impermanent loss >8%
> - Pool liquidity drops >25%
> - APY changes significantly"

Your agent calculates IL based on current prices vs entry, monitors pool health.

---

### 8. Market Overview Bot

Get automated market briefings.

**Setup:**
> "Send me a market overview on Telegram every morning at 7am and evening at 7pm. Include BTC, ETH, SOL, and any token that moved >10% in top 100 by market cap."

**Example brief:**
> 📊 Crypto Market Brief - Feb 20, 7:00 AM
>
> **Majors:**
> - BTC: $67,234 (↑2.1% 24h) - grinding higher
> - ETH: $3,456 (↑4.3% 24h) - outperforming
> - SOL: $123.45 (↓1.2% 24h) - slight pullback
>
> **Big Movers (24h):**
> - PEPE: ↑23% (meme season?)
> - ARB: ↑15% (ecosystem growth)
> - AVAX: ↓12% (profit taking)
>
> **Volume Leaders:** PEPE, SHIB, WIF
>
> **Sentiment:** Cautiously bullish. ETH leading, memes pumping.

---

## 🎯 Use Case Templates

Ready-to-use heartbeat templates are in the [`examples/`](./examples) folder:

- [`heartbeat-portfolio-guardian.md`](./examples/heartbeat-portfolio-guardian.md) - Monitor your wallet 24/7
- [`heartbeat-whale-tracker.md`](./examples/heartbeat-whale-tracker.md) - Follow smart money
- [`heartbeat-token-scout.md`](./examples/heartbeat-token-scout.md) - Autonomous token discovery
- [`heartbeat-market-brief.md`](./examples/heartbeat-market-brief.md) - Daily market summaries

**How to use:**
1. Copy the template to `~/openclaw/heartbeat/`
2. Customize the parameters (wallet addresses, criteria, timing)
3. Your agent will automatically execute on the schedule

---

## 📚 Documentation

- **[SKILL.md](./SKILL.md)** - Full skill documentation (what the agent reads)
- **[SETUP.md](./docs/SETUP.md)** - Detailed setup guide
- **[EXAMPLES.md](./docs/EXAMPLES.md)** - More use case examples
- **[API_REFERENCE.md](./docs/API_REFERENCE.md)** - Endpoint reference
- **[TROUBLESHOOTING.md](./docs/TROUBLESHOOTING.md)** - Common issues & solutions

---

## 🔧 Supported Endpoints

| Endpoint | Purpose |
|----------|---------|
| `/market/data` | Price, volume, mcap for any token |
| `/wallet/portfolio` | Complete wallet portfolio across all chains |
| `/market/history` | Historical price data |
| `/market/trades` | Live trade feed (whale watching) |
| `/wallet/transactions` | Transaction history for any wallet |
| `/market/multi-data` | Batch endpoint (multiple tokens at once) |
| `/metadata` | Token info (website, socials, contract) |

**88+ blockchains supported** including:
Ethereum, Base, Arbitrum, Optimism, Polygon, BNB Chain, Avalanche, Solana, Fantom, Cronos, and [many more](https://docs.mobula.io/blockchains).

---

## 🎓 How It Works

1. **You give instructions** in natural language to your OpenClaw agent
2. **The agent reads SKILL.md** to understand what Mobula endpoints are available
3. **On heartbeat (~30min)**, the agent checks your configured monitoring tasks
4. **When conditions are met**, it fetches data from Mobula API
5. **Analyzes and alerts you** on your preferred channel (Telegram, WhatsApp, Discord, Slack, etc.)

The magic is in **persistent, proactive monitoring** - your agent works 24/7 without you needing to ask.

---

## 💎 Why This is Different from CoinGecko/CMC/DexScreener

| Feature | CoinGecko/CMC | DexScreener | Mobula + OpenClaw |
|---------|---------------|-------------|-------------------|
| Price tracking | ✅ | ✅ | ✅ |
| 24/7 monitoring | ❌ Manual | ❌ Basic alerts | ✅ AI agent |
| Contextual alerts | ❌ | ❌ | ✅ Smart conditions |
| Whale tracking | ❌ | ❌ | ✅ Real-time |
| Portfolio monitoring | Basic | ❌ | ✅ Cross-chain |
| Token discovery | ❌ Manual | ❌ Manual | ✅ Autonomous |
| Natural language | ❌ | ❌ | ✅ Full |
| Multi-wallet | ❌ | ❌ | ✅ |
| Custom logic | ❌ | ❌ | ✅ Unlimited |

**With Mobula + OpenClaw:** You have a personal crypto analyst that never sleeps, learns your preferences, and proactively finds opportunities.

---

## 🚦 Rate Limits

- **Free tier:** 100 requests/minute
- **Pro tier:** Higher limits + priority support

For heavy monitoring (multiple wallets, frequent checks), consider [upgrading your plan](https://admin.mobula.fi).

**Optimization tips:**
- Use `/market/multi-data` for batch queries (1 request for 500 tokens vs 500 requests)
- Cache metadata (project info doesn't change often)
- Set heartbeat intervals appropriately (30min is usually enough)

---

## 🤝 Contributing

Found a bug? Have a cool use case idea?

- Open an [Issue](https://github.com/Flotapponnier/Crypto-date-openclaw/issues)
- Submit a [Pull Request](https://github.com/Flotapponnier/Crypto-date-openclaw/pulls)
- Share your heartbeat templates with the community

---

## 📖 Resources

- **Mobula API Docs:** [docs.mobula.io](https://docs.mobula.io)
- **Get API Key:** [admin.mobula.fi](https://admin.mobula.fi)
- **OpenClaw Docs:** [openclaw.ai](https://openclaw.ai)
- **Supported Chains:** [docs.mobula.io/blockchains](https://docs.mobula.io/blockchains)

---

## 📄 License

MIT License - see [LICENSE](./LICENSE) for details.

---

## 🙏 Credits

- **Skill by:** [Mobula](https://mobula.io)
- **Built for:** [OpenClaw](https://openclaw.ai) by Peter Steinberger
- **Data coverage:** 88+ blockchains, 200M+ tokens

---

## ⚡ Get Started Now

1. Install the skill (see [Quick Start](#-quick-start))
2. Try a simple query: "What's the price of Bitcoin?"
3. Set up your first monitoring task: "Monitor my wallet 0x..."
4. Explore the [examples](./examples) for inspiration

**Transform your OpenClaw agent into a crypto powerhouse.** 🚀
