---
name: Portfolio Guardian
description: Monitor your crypto wallet 24/7 with smart alerts and daily summaries
interval: 30
enabled: true
---

# Portfolio Guardian - 24/7 Wallet Monitoring

This heartbeat task monitors your crypto portfolio continuously and alerts you on important changes.

## Configuration

**Wallet to monitor:** `YOUR_WALLET_ADDRESS_HERE`
*(Replace with your actual wallet address, e.g., 0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb)*

**Alert thresholds:**
- Maximum allocation per token: **40%** (rebalance suggestion if exceeded)
- Price drop alert: **-15%** in 24h (significant loss alert)
- Total portfolio change: **±10%** since last check (major move notification)

**Daily summary time:** **9:00 AM** (local time)

**Alert channel:** Telegram *(change to WhatsApp, Discord, or Slack if preferred)*

---

## What This Does

Every **30 minutes**, your agent will:

1. **Fetch your portfolio** via Mobula's `/wallet/portfolio` endpoint
2. **Calculate key metrics:**
   - Total portfolio value (USD)
   - Allocation percentage for each token
   - 24h price change for each holding
   - Total portfolio change since last check
3. **Check alert conditions:**
   - Is any single token >40% of total portfolio? → Concentration risk warning
   - Did any token drop >15% in 24h? → Loss alert with context
   - Did total portfolio value change >10%? → Major move notification
4. **Store data in memory** for historical tracking
5. **At 9am daily:** Compile and send a comprehensive summary

---

## Example Alerts

### Concentration Risk Alert
```
⚠️ Portfolio Alert - Concentration Risk

Your ETH allocation is now 47% of your total portfolio (was 38% yesterday).

Current allocation:
• ETH: $12,340 (47%)
• BRETT: $8,200 (31%)
• DEGEN: $5,890 (22%)

Recommendation: Consider rebalancing to reduce single-asset risk.
Target: Keep individual holdings under 40%.
```

### Price Drop Alert
```
📉 Portfolio Alert - Significant Drop

BRETT is down -18% in the last 24 hours.

Details:
• Current price: $0.073 (was $0.089)
• Your holding: 112,329 BRETT
• Value: $8,200 (down from $10,000)
• Loss: -$1,800 (-18%)

24h context: Token is down across the market, volume up 3x.
Consider: This could be a dip buying opportunity or a trend reversal.
```

### Major Portfolio Move
```
🚀 Portfolio Alert - Big Move!

Your total portfolio is up +12.4% since last check (30 min ago).

Total value: $28,450 (was $25,320)
Gain: +$3,130

Top movers:
• BRETT: +23% ($2,100 gain)
• DEGEN: +8% ($620 gain)
• ETH: +2% ($240 gain)

This looks like a BRETT pump leading the portfolio.
```

---

## Daily Summary Example

```
☀️ Daily Portfolio Summary - February 20, 2024

Total Value: $26,430 (↑$1,240 / +4.9% 24h)

Holdings Breakdown:
1. ETH: $11,200 (42%) - ↑3.2% 24h | +$350
2. BRETT: $9,100 (34%) - ↑12.4% 24h | +$1,010
3. DEGEN: $6,130 (23%) - ↓2.1% 24h | -$130

Performance:
• Best: BRETT +12.4% (+$1,010)
• Worst: DEGEN -2.1% (-$130)
• Net 24h: +$1,240 (+4.9%)

Allocation Status: ✓ Well balanced

Activity:
• No new trades detected
• All holdings stable

Notes:
• BRETT continues strong uptrend (up 67% over 7d)
• ETH allocation is healthy at 42%
• Portfolio diversification is good

Have a great day! 🚀
```

---

## Instructions for the Agent

### On Every Heartbeat (30 min):

1. **Fetch portfolio data:**
   ```
   Use mobula_wallet_portfolio with wallet: YOUR_WALLET_ADDRESS_HERE
   ```

2. **Calculate allocations:**
   ```
   For each token:
   - allocation_pct = (token_value_usd / total_portfolio_value) * 100
   ```

3. **Check concentration risk:**
   ```
   If any token has allocation_pct > 40%:
   - Send concentration risk alert via Telegram
   - Include current allocations
   - Suggest rebalancing
   ```

4. **Check 24h price drops:**
   ```
   For each token:
   - If price_change_24h < -15%:
     - Send price drop alert via Telegram
     - Include: current price, previous price, loss amount, market context
     - Use mobula_market_data for additional context (volume, market-wide movement)
   ```

5. **Check total portfolio change:**
   ```
   Compare current total_value with stored previous_value (from memory):
   - If abs(change_pct) > 10%:
     - Send major move alert
     - Include: total change, top movers, reason for change
   ```

6. **Update memory:**
   ```
   Store in SOUL.md or memory:
   - current_portfolio_value
   - timestamp
   - allocations_snapshot

   This allows tracking changes over time.
   ```

7. **Check time for daily summary:**
   ```
   If current_time is approximately 9:00 AM local:
   - Compile daily summary (see format above)
   - Include: 24h performance, allocation status, best/worst performers
   - Send via Telegram
   ```

---

## Customization Guide

### Change Alert Thresholds
Edit these values above:
- `Maximum allocation per token`: Default 40%, increase for more concentration tolerance
- `Price drop alert`: Default -15%, adjust based on your risk tolerance
- `Total portfolio change`: Default ±10%, set higher for less frequent alerts

### Change Summary Time
Edit `Daily summary time` to your preferred morning time (e.g., 8:00 AM, 10:00 AM)

### Change Alert Channel
Replace "Telegram" with your preferred messaging app:
- WhatsApp
- Discord
- Slack
- Signal
- iMessage

### Add More Wallets
To monitor multiple wallets, duplicate this heartbeat file and change the wallet address in each.

Or modify the task to loop through multiple addresses:
```
Wallets to monitor:
- Main wallet: 0x123...
- Degen wallet: 0x456...
- Cold storage: 0x789...

Check all three and send combined summary.
```

---

## Advanced Features

### Track Historical Performance
```
Every heartbeat, store:
- Timestamp
- Total portfolio value
- Top holdings

Over time, this builds a history you can analyze:
- "Show me my portfolio performance over the last 30 days"
- "What was my best day this month?"
- "Calculate my 7-day average daily return"
```

### Smart Rebalancing Suggestions
```
If concentration risk detected:
- Calculate how much to sell/buy to reach target allocation (e.g., 33% each)
- "To rebalance to 33% each, sell $X of ETH and buy $Y of DEGEN"
```

### Profit/Loss Tracking
```
If you tell the agent your entry prices:
- Store in USER.md: "Bought 100K BRETT at $0.05 on Jan 15"
- Agent can calculate unrealized PnL
- "Your BRETT position is up $4,000 (+80%) from entry"
```

### Tax Loss Harvesting Alerts
```
If a token is down significantly:
- "DEGEN is down 25% from your entry. Consider tax loss harvesting before year end."
```

---

## Troubleshooting

**Agent not sending alerts?**
- Check that `enabled: true` in the frontmatter
- Verify your wallet address is correct
- Ensure MOBULA_API_KEY is set
- Check agent logs for errors

**Too many alerts?**
- Increase thresholds (e.g., 20% drop instead of 15%)
- Increase heartbeat interval (e.g., 60 min instead of 30)

**Want more frequent updates?**
- Decrease interval to 15 or 10 minutes
- Note: This increases API usage

**Daily summary not sending?**
- Verify the time format matches your local timezone
- Check agent logs around summary time

---

## Setup Checklist

- [ ] Replace `YOUR_WALLET_ADDRESS_HERE` with your actual wallet
- [ ] Adjust alert thresholds to your preferences
- [ ] Set your preferred daily summary time
- [ ] Choose your alert channel (Telegram, WhatsApp, etc.)
- [ ] Copy this file to `~/openclaw/heartbeat/`
- [ ] Restart your OpenClaw agent
- [ ] Test: Wait 30 minutes and check if agent is monitoring
- [ ] Test: Say "Check my portfolio" to agent manually

---

**Once set up, your agent will be your 24/7 portfolio guardian.** 🛡️
