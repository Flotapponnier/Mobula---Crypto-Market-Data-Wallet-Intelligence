---
name: Whale Tracker
description: Monitor smart money wallets and get alerted on significant moves
interval: 30
enabled: true
---

# Whale Tracker - Follow Smart Money

This heartbeat task monitors whale wallets 24/7 and alerts you when they make significant moves.

## Configuration

**Wallets to track:**
```
WHALE_1: 0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb
WHALE_2: 0x8888887dF37c0A1e7C2A3b8f0c8b2D8e7c6B5A4D
WHALE_3: 0x1111117dF37c0A1e7C2A3b8f0c8b2D8e7c6B5A4D
```
*(Replace these with actual whale wallets you want to monitor)*

**Alert thresholds:**
- **Significant trade:** Transactions >$50,000 USD
- **Priority signal:** 2+ whales buy the same token within 6 hours
- **Watch threshold:** Any transaction >$10,000 (logged, not alerted)

**Alert channel:** Telegram

**Cross-reference window:** 6 hours (to detect coordinated moves)

---

## What This Does

Every **30 minutes**, your agent will:

1. **Check recent transactions** for all tracked wallets
2. **Detect new trades** since last check (compare timestamps)
3. **For significant trades (>$50K):**
   - Get token details (price, mcap, liquidity, volume)
   - Check recent trades on that token (accumulation pattern?)
   - Cross-reference with other tracked wallets (did others buy it too?)
   - Analyze risk/opportunity
4. **Send tiered alerts:**
   - 🐋 **Standard alert:** Single whale made a big move
   - 🚨 **Priority alert:** Multiple whales bought the same token (coordinated signal)
5. **Daily summary** of all whale activity

---

## Example Alerts

### Standard Whale Alert
```
🐋 Whale Activity Detected

Wallet: 0x742d...0bEb (WHALE_1)
Action: BOUGHT $150,000 of BRETT
Time: 2 hours ago
Chain: Base

Token Analysis:
• Price: $0.089 (↑23% 24h, ↑67% 7d)
• Market Cap: $2.3M
• Liquidity: $340K (decent for size)
• Volume 24h: $1.2M (up 8x vs average)

Recent trades:
• 3:1 buy/sell ratio last 1h
• 1 other large buy ($80K) detected 30min ago

Assessment: Token is trending up with strong volume. Whale likely accumulating before further move.

[Dexscreener Link] [Etherscan Link]
```

### Priority Alert (Multiple Whales)
```
🚨 HIGH PRIORITY - Coordinated Whale Activity

Multiple tracked whales are buying the SAME token:

Token: BRETT on Base

Whale Activity (last 6h):
1. WHALE_1 (0x742d...): $150K bought (2h ago)
2. WHALE_2 (0x8888...): $80K bought (4h ago)
3. WHALE_3 (0x1111...): $120K bought (1h ago)

Total tracked whale volume: $350K in 6 hours

Token Analysis:
• Price: $0.089 (↑23% 24h)
• Mcap: $2.3M (small cap - high potential)
• Liquidity: $340K
• Volume: $1.2M (massive spike)

Pattern: This looks like coordinated accumulation. 3 whales don't buy the same small-cap token randomly.

Risk: Small cap = high volatility
Opportunity: Early on a potential whale-backed pump

⚡ This is an actionable signal.

[Dexscreener] [Contract]
```

### Daily Summary
```
📊 Whale Activity Summary - Feb 20, 2024

Total tracked whales: 3
Total transactions: 12
Significant moves (>$50K): 4

Top Activity:
• WHALE_1: 5 transactions, $420K total volume
  - Bought: BRETT ($150K), ETH ($120K)
  - Sold: PEPE ($150K - profit taking)

• WHALE_2: 4 transactions, $180K volume
  - Accumulating: ETH (3 buys)

• WHALE_3: 3 transactions, $290K volume
  - Bought: BRETT ($120K)
  - Bought: DEGEN ($170K)

Common holdings:
• ETH (all 3 whales accumulating)
• BRETT (2 whales bought in last 24h) ⚡

Patterns:
• All whales are bullish ETH
• BRETT seeing coordinated buys (watch this)
• PEPE profit-taking by WHALE_1

Stay sharp! 🐋
```

---

## Instructions for the Agent

### On Every Heartbeat (30 min):

1. **For each tracked wallet:**
   ```
   Use mobula_wallet_transactions with:
   - wallet: WHALE_ADDRESS
   - from: last_check_timestamp (stored in memory)
   - to: current_timestamp
   ```

2. **Detect new transactions:**
   ```
   Compare fetched transactions with stored last_transaction_hash (from memory).
   New transactions = those not seen before.
   ```

3. **For each new transaction:**

   **If transaction value >$10K (watch threshold):**
   - Log it in memory for daily summary

   **If transaction value >$50K (alert threshold):**

   a. **Get token details:**
   ```
   Use mobula_market_data for the token involved:
   - Current price
   - Market cap
   - Liquidity
   - Volume 24h (compare to average)
   - Price change 24h, 7d
   ```

   b. **Check recent trades:**
   ```
   Use mobula_market_trades for the token:
   - Last 50 trades
   - Calculate buy/sell ratio
   - Identify other large buys
   ```

   c. **Cross-reference with other whales:**
   ```
   Check stored whale activity (from memory):
   - Did any other tracked wallet buy this token in last 6 hours?
   - If yes: count how many whales bought it
   - Calculate total whale volume on this token
   ```

   d. **Determine alert type:**
   ```
   If 2+ whales bought same token within 6h:
     → Send PRIORITY ALERT
   Else if single whale >$50K:
     → Send STANDARD ALERT
   ```

4. **Send alert with full context:**
   ```
   Include:
   - Whale identifier (wallet address + nickname if set)
   - Action (bought/sold), amount, token
   - Token analysis (price, mcap, volume, trend)
   - Recent trade patterns
   - Cross-reference results
   - Assessment/interpretation
   - Links (Dexscreener, Etherscan/block explorer)
   ```

5. **Update memory:**
   ```
   Store:
   - last_check_timestamp = current_timestamp
   - last_transaction_hashes for each wallet
   - All whale activity for daily summary (rolling 24h)
   - Tokens bought by multiple whales (with timestamps)
   ```

6. **Daily summary (once per day, e.g., 8pm):**
   ```
   Compile all whale activity from last 24h:
   - Total transactions per whale
   - Total volume per whale
   - Most bought tokens
   - Patterns (e.g., "all whales buying ETH")
   - Send summary via Telegram
   ```

---

## Customization Guide

### Add/Remove Whales
Edit the wallets list above. To add:
```
WHALE_4: 0xYourWhaleAddressHere
```

Give them nicknames for easier tracking:
```
WHALE_1 (Ansem): 0x742d...
WHALE_2 (Tetranode): 0x8888...
```

### Adjust Thresholds
- **Significant trade:** Default $50K. Increase for higher signal, decrease for more alerts
- **Watch threshold:** Default $10K for logging only
- **Priority signal:** Default 2+ whales. Increase to 3+ for stricter signals
- **Cross-reference window:** Default 6 hours. Adjust based on your trading style

### Change Alert Channel
Replace Telegram with WhatsApp, Discord, Slack, etc.

### Add More Analysis
You can enhance with:
- **Holder distribution:** Check if token has whale concentration
- **Contract verification:** Only alert on verified contracts
- **Liquidity locked:** Check if LP is locked (rug pull risk)
- **Historical performance:** Did this whale make profitable trades before?

---

## Advanced Features

### Whale Performance Tracking
```
Track each whale's trades over time:
- Entry price vs current price
- Win rate (% of trades that are profitable)
- Average hold time
- Best/worst trades

"WHALE_1 has 73% win rate. When they buy, it pumps 68% of the time within 48h."
```

### Smart Filtering
```
Only alert if:
- Whale buys a token AND it's near 30d low (buying the dip)
- Whale buys AND volume is spiking (momentum play)
- Whale buys AND token just broke resistance (breakout)
```

### Copy Trading Assistant
```
When priority alert fires:
- Calculate: "To copy this trade proportionally, you'd buy $X of TOKEN"
- Show entry price
- Suggest stop loss based on liquidity
```

### Exit Signals
```
Track when whales SELL:
- "WHALE_1 just sold 50% of BRETT holdings. Consider taking profit."
```

---

## Finding Whale Wallets

**Where to find good wallets to track:**

1. **Nansen Wallet Labels:** Search for "Smart Money" wallets
2. **Etherscan/Arbiscan Token Holders:** Look at top holders of successful tokens
3. **Twitter/CT:** Follow whale watching accounts, they often share addresses
4. **Dexscreener:** Check "Top Traders" on trending tokens
5. **On-chain data sites:** Arkham, Debank, Zerion

**Tips for choosing wallets:**
- Look for consistent profitability (not just lucky on one trade)
- Prefer wallets with public track records
- Diversify: track whales from different ecosystems (Base, Arbi, ETH mainnet)
- Start with 3-5 wallets, add more as you learn

---

## Troubleshooting

**No alerts coming through?**
- Verify wallet addresses are correct
- Check that wallets are active (have recent transactions)
- Lower the threshold temporarily to test ($10K instead of $50K)

**Too many alerts?**
- Increase significant trade threshold ($100K instead of $50K)
- Filter by chain (only Base, for example)
- Only alert on buys (ignore sells)

**Want real-time (not 30min delay)?**
- Decrease heartbeat interval to 10-15min
- Note: This increases API calls

**Missed a move?**
- Check daily summary for logged activity
- Reduce heartbeat interval for faster detection

---

## Setup Checklist

- [ ] Replace whale wallet addresses with actual addresses
- [ ] Give whales nicknames (optional but helpful)
- [ ] Set your alert thresholds ($50K default, adjust as needed)
- [ ] Choose alert channel (Telegram, etc.)
- [ ] Copy this file to `~/openclaw/heartbeat/`
- [ ] Restart OpenClaw agent
- [ ] Test: Check that whales are being monitored (ask agent "Show recent whale activity")
- [ ] Wait for first alert (or test with lower threshold)

---

**Your agent is now tracking smart money 24/7.** 🐋💰
