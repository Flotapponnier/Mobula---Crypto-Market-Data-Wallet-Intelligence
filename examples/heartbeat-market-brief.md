---
name: Market Brief
description: Daily crypto market summaries delivered to you automatically
interval: 1440
enabled: true
---

# Market Brief - Automated Crypto Market Summaries

Get a comprehensive crypto market overview delivered to you automatically every morning.

## Configuration

**Frequency:** Once per day (every 1440 minutes = 24 hours)

**Delivery time:** **8:00 AM** (local time)

**Assets to track:**
- **Majors:** BTC, ETH, SOL, BNB
- **Your holdings:** (optional - add your portfolio tokens)
- **Top movers:** Any token in top 100 by mcap that moved >10% in 24h

**Alert channel:** Telegram

**Include:**
- Price changes (1h, 24h, 7d)
- Volume analysis
- Big movers (>10% change)
- Your portfolio summary (if wallet configured)
- Sentiment summary

---

## What This Does

Every morning at **8:00 AM**, your agent will:

1. **Check major assets** (BTC, ETH, SOL, BNB) for price changes and volume
2. **Identify big movers** (tokens with >10% change in 24h)
3. **Analyze volume leaders** (highest 24h volume)
4. **Check your portfolio** (if wallet configured) for performance
5. **Compile sentiment** (bullish, bearish, neutral based on data)
6. **Send formatted brief** via Telegram

It's like having a personal morning briefing from a crypto analyst.

---

## Example Morning Brief

```
☀️ Crypto Market Brief
February 20, 2024 | 8:00 AM

━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 MAJOR ASSETS
━━━━━━━━━━━━━━━━━━━━━━━━━━

BTC: $67,234
• 1h: +0.8% | 24h: +2.1% | 7d: +5.4%
• Volume: $28.4B (↑15% vs avg)
• Status: Grinding higher, broke $67K resistance

ETH: $3,456
• 1h: +1.2% | 24h: +4.3% | 7d: +8.1%
• Volume: $14.2B (↑22% vs avg)
• Status: Outperforming BTC, bullish momentum

SOL: $123.45
• 1h: -0.3% | 24h: -1.2% | 7d: +3.8%
• Volume: $2.8B
• Status: Slight pullback after 7d run

BNB: $389.12
• 1h: +0.5% | 24h: +1.8% | 7d: +2.3%
• Volume: $1.2B
• Status: Steady, lagging majors

━━━━━━━━━━━━━━━━━━━━━━━━━━
🚀 BIG MOVERS (24h)
━━━━━━━━━━━━━━━━━━━━━━━━━━

Top Gainers:
1. PEPE: +23.4% | $0.00000189 | Vol: $890M
   → Meme season heating up

2. ARB: +15.2% | $1.89 | Vol: $450M
   → Ecosystem growth, airdrop rumors

3. WIF: +18.7% | $2.34 | Vol: $320M
   → Dog meme rally

Top Losers:
1. AVAX: -12.3% | $45.67 | Vol: $340M
   → Profit taking after rally

2. MATIC: -8.9% | $1.12 | Vol: $280M
   → General weakness

3. LINK: -6.4% | $18.90 | Vol: $420M
   → No specific catalyst

━━━━━━━━━━━━━━━━━━━━━━━━━━
💼 YOUR PORTFOLIO
━━━━━━━━━━━━━━━━━━━━━━━━━━

Total Value: $26,430 (↑$1,240 / +4.9% 24h)

Holdings:
• ETH: $11,200 (42%) - ↑3.2% | +$350
• BRETT: $9,100 (34%) - ↑12.4% | +$1,010 🔥
• DEGEN: $6,130 (23%) - ↓2.1% | -$130

Best performer: BRETT (+12.4%)
Allocation: Balanced ✅

━━━━━━━━━━━━━━━━━━━━━━━━━━
📈 MARKET SENTIMENT
━━━━━━━━━━━━━━━━━━━━━━━━━━

Overall: 🟢 Cautiously Bullish

Signals:
✅ BTC holding above $67K (key level)
✅ ETH outperforming (altseason signal)
✅ Volume is up across majors
⚠️ Memes pumping (late-cycle indicator?)

Watch:
• BTC $68K resistance (break = continuation)
• ETH $3,500 (psychological level)
• SOL support at $120

━━━━━━━━━━━━━━━━━━━━━━━━━━
🗓️ TODAY'S EVENTS
━━━━━━━━━━━━━━━━━━━━━━━━━━

• 10:00 AM: Fed meeting minutes release
• 2:00 PM: Base ecosystem AMA
• Evening: ETH Denver continues (watch for announcements)

━━━━━━━━━━━━━━━━━━━━━━━━━━

Have a great day! 🚀

Next brief: Tomorrow at 8:00 AM
```

---

## Instructions for the Agent

### Once Per Day at 8:00 AM:

1. **Check current time:**
   ```
   If current_time ≈ 08:00 (±15 min):
     Proceed with brief
   Else:
     Skip and wait for next heartbeat
   ```

2. **Fetch major assets:**
   ```
   Use mobula_market_multi with assets: "Bitcoin,Ethereum,Solana,BNB"

   For each:
   - Current price
   - Price changes: 1h, 24h, 7d
   - Volume 24h
   - Compare volume to historical average (if available)
   ```

3. **Identify big movers:**
   ```
   Strategy depends on data availability:

   Option A: If you have a watchlist (top 100 tokens)
   - Use mobula_market_multi for all
   - Filter: abs(price_change_24h) > 10%
   - Sort by magnitude
   - Take top 3 gainers, top 3 losers

   Option B: Manual/curated list
   - User provides list of tokens to check
   - Filter and sort same as above

   For each big mover:
   - Token, price, 24h change, volume
   - Brief explanation if obvious (e.g., "airdrop announced")
   ```

4. **Fetch user portfolio (if configured):**
   ```
   If user has set wallet address in USER.md or config:

   Use mobula_wallet_portfolio with wallet address

   Calculate:
   - Total portfolio value
   - 24h change (compare to stored previous value from yesterday)
   - Allocation percentages
   - Best/worst performers in portfolio
   ```

5. **Analyze volume:**
   ```
   Identify volume leaders from big movers

   Context:
   - High volume + price up = strong move
   - High volume + price down = capitulation or distribution
   ```

6. **Determine market sentiment:**
   ```
   Analyze:
   - Are majors (BTC, ETH) up or down?
   - Is volume increasing or decreasing?
   - Are more tokens pumping or dumping?
   - Are memecoins rallying? (late cycle indicator)

   Classify:
   - 🟢 Bullish: BTC/ETH up, volume up, more pumps than dumps
   - 🟡 Neutral: Mixed signals
   - 🔴 Bearish: BTC/ETH down, volume down, more dumps
   - 🟠 Cautiously Bullish/Bearish: Signals are mixed but leaning one way
   ```

7. **Note key levels to watch:**
   ```
   For BTC and ETH:
   - Resistance levels (round numbers, recent highs)
   - Support levels (recent lows, psychological levels)

   Example:
   - BTC: Watch $68K resistance, $65K support
   - ETH: Watch $3,500 resistance, $3,300 support
   ```

8. **Add today's events (optional):**
   ```
   If user has provided calendar events or you're aware of major events:
   - Fed meetings
   - Major crypto conferences (ETH Denver, Consensus, etc.)
   - Token unlocks
   - Airdrops

   Include with times.
   ```

9. **Format and send:**
   ```
   Use template format (see example above)

   Send via Telegram with:
   - Clear sections (use ━ separators)
   - Emojis for readability (☀️, 📊, 🚀, 💼, 📈)
   - Concise but informative
   - Actionable insights
   ```

10. **Update memory:**
    ```
    Store for tomorrow's comparison:
    - Today's portfolio value
    - Today's BTC/ETH prices
    - Timestamp of this brief
    ```

---

## Customization Guide

### Change Delivery Time
Edit **Delivery time** in configuration:
- `8:00 AM` = Morning brief before market open
- `7:00 PM` = Evening summary after trading day
- Both: Set interval to 720 (12h) and send at 8 AM and 8 PM

### Change Frequency
Edit `interval` in frontmatter:
- `1440` = Once per day (default)
- `720` = Twice per day (morning + evening)
- `10080` = Once per week (Sunday evening summary)

### Add/Remove Assets
Edit **Assets to track**:
- Add more majors: "Cardano", "Polygon", "Avalanche"
- Add your holdings: "BRETT", "DEGEN", "PEPE"
- Remove any you don't care about

### Add Portfolio Tracking
Set your wallet address:
```
Portfolio wallet: 0xYourWalletAddressHere
```
Agent will include your portfolio performance in every brief.

### Change Alert Channel
Replace Telegram with WhatsApp, Discord, email, Slack, etc.

### Customize Sections
You can add/remove sections:
- **Add:** DeFi TVL summary, NFT floor prices, gas prices
- **Remove:** Events section if not useful

---

## Advanced Features

### Multi-Chain Summary
```
Instead of just tokens, summarize by chain:

"Base: 12 tokens up, 5 down. Avg change: +4.2%"
"Arbitrum: 8 up, 9 down. Avg: -1.1%"
"ETH Mainnet: 15 up, 10 down. Avg: +2.8%"

Insight: "Base is outperforming today."
```

### Historical Comparison
```
Compare today to:
- Yesterday: "Market is up 2.3% vs yesterday"
- Last week: "BTC is now 8% higher than this time last week"
- Last month: "Portfolio is up 23% since last month"
```

### Trend Analysis
```
Track trends over time:
- "BTC has been up 5 days in a row (longest streak this month)"
- "ETH is forming higher lows (bullish pattern)"
- "Memecoins have rallied 3 days straight (caution: overheated?)"
```

### Personalized Insights
```
Based on user's portfolio and behavior:
- "Your BRETT holding is up 45% this week. Consider taking some profit?"
- "ETH is near resistance. You might want to DCA if it breaks $3,500."
- "Your portfolio allocation has shifted to 50% BRETT due to price. Rebalance?"
```

### News Integration (if available)
```
Use web search to fetch top crypto news:
- "Top story: SEC approves ETH ETF (bullish for ETH)"
- "FUD: Exchange hack reported (market pullback expected)"
```

---

## Combining with Other Heartbeats

**Portfolio Guardian + Market Brief:**
- Morning brief shows overall market
- Portfolio guardian monitors throughout the day
- You get both macro and micro views

**Whale Tracker + Market Brief:**
- Brief shows market is pumping
- Whale tracker shows which whales are buying
- Combined: "Market pumping + whales accumulating = strong signal"

**Token Scout + Market Brief:**
- Brief shows memecoins rallying
- Scout finds new memecoin opportunities
- "Memecoin season detected, here are 3 new ones to watch"

---

## Troubleshooting

**Brief not sending at 8 AM?**
- Check timezone settings on your system
- Verify heartbeat interval is set to 1440 (24h)
- Check agent logs for errors around 8 AM

**Want brief at different times?**
- Edit delivery time in config
- For multiple times per day, reduce interval (e.g., 720 for 2x daily)

**Too much info / too long?**
- Remove sections you don't need (events, volume leaders, etc.)
- Limit big movers to top 1 gainer/loser instead of 3

**Not enough info?**
- Add more sections (DeFi, NFTs, gas prices)
- Include more tokens in big movers (top 5 instead of 3)
- Add news/sentiment from Twitter

---

## Setup Checklist

- [ ] Set your preferred delivery time (8 AM default)
- [ ] Choose frequency (daily, twice daily, weekly)
- [ ] List assets you want to track (BTC, ETH, SOL, BNB + others)
- [ ] Optionally add your wallet for portfolio tracking
- [ ] Set alert channel (Telegram, etc.)
- [ ] Copy this file to `~/openclaw/heartbeat/`
- [ ] Restart OpenClaw agent
- [ ] Wait for first brief (next morning at 8 AM)

---

**Wake up to crypto insights every morning.** ☀️📊
