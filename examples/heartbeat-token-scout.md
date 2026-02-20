---
name: Token Scout
description: Autonomously discover tokens matching your criteria
interval: 360
enabled: true
---

# Token Scout - Autonomous Token Discovery

This heartbeat task autonomously finds new tokens matching your criteria without you needing to ask.

## Configuration

**Scan interval:** Every **6 hours** (360 minutes)

**Search criteria:**
- **Chains:** Base, Arbitrum
- **Market cap:** < $5M (small cap opportunities)
- **Liquidity:** > $100K (minimum for safe entry/exit)
- **Volume change 24h:** > +50% (momentum/interest)
- **Contract:** Must be verified (reduce rug risk)

**Results:** Send top **3 matches** per scan

**Alert channel:** Telegram

---

## What This Does

Every **6 hours**, your agent will:

1. **Scan tokens** across specified chains (Base, Arbitrum)
2. **Filter by criteria** (mcap, liquidity, volume surge)
3. **For each match:**
   - Get 7-day price history (trend analysis)
   - Check metadata (contract verified? social links?)
   - Calculate risk score
   - Check if previously flagged (avoid duplicates)
4. **Rank by potential** (volume surge + fundamentals)
5. **Send top 3 with detailed analysis**

This is like having a junior analyst working 24/7 to find opportunities.

---

## Example Alerts

### Token Scout Report
```
🔍 Token Scout - 3 New Discoveries
Scan completed: Feb 20, 2024 at 14:00

Chains: Base, Arbitrum
Criteria: <$5M mcap, >$100K liq, volume up >50% 24h

━━━━━━━━━━━━━━━━━━━━━━━━━━

1. BOOP on Base ⭐⭐⭐

Current Metrics:
• Price: $0.0042 (↑156% 24h, ↑340% 7d)
• Market Cap: $2.1M
• Liquidity: $280K
• Volume 24h: $890K (12x vs average!)

Chart Analysis:
• 7-day trend: Strong uptrend
• Broke resistance at $0.0035 (now support)
• Not near ATH - room to run

Fundamentals:
• Contract: Verified ✅
• Holders: 1,240 (growing)
• Liquidity locked: Yes (90 days)

Risk Score: 6/10 (Medium)
• Good liquidity for mcap (13%)
• Contract verified ✅
• Recent volume spike (potential FOMO)

Why interesting:
• Volume exploded (12x) - something is happening
• Broke key level and holding
• Liquidity is decent for safe entry/exit

[Dexscreener] [Contract] [Twitter]

━━━━━━━━━━━━━━━━━━━━━━━━━━

2. ZORP on Arbitrum ⭐⭐

Current Metrics:
• Price: $0.0089 (↑78% 24h, ↑120% 7d)
• Market Cap: $3.4M
• Liquidity: $450K
• Volume 24h: $1.1M

Chart Analysis:
• Consolidating after pump
• Potential bull flag forming
• Volume still elevated

Fundamentals:
• Contract: Verified ✅
• Holders: 890
• Recent whale buy: $60K (3h ago)

Risk Score: 5/10 (Medium-Low)

Why interesting:
• Strong liquidity (13% of mcap)
• Whale just bought (smart money signal?)
• Consolidation could lead to breakout

[Dexscreener] [Contract]

━━━━━━━━━━━━━━━━━━━━━━━━━━

3. FLOKI2.0 on Base ⭐

Current Metrics:
• Price: $0.0156 (↑52% 24h)
• Market Cap: $4.8M
• Liquidity: $120K
• Volume: $680K

Chart Analysis:
• Sharp pump, now cooling off
• Low volume compared to pump

Fundamentals:
• Contract: Verified ✅
• Holders: 2,100

Risk Score: 7/10 (Medium-High)
• Liquidity is low relative to mcap (2.5%)
• Could be harder to exit on dump

Why interesting:
• Memecoin narrative (FLOKI derivative)
• High holder count for mcap
• Volume surge but needs confirmation

⚠️ Higher risk due to low liquidity ratio

[Dexscreener] [Contract]

━━━━━━━━━━━━━━━━━━━━━━━━━━

Next scan in 6 hours.
```

---

## Instructions for the Agent

### Every 6 Hours:

1. **Prepare search:**
   ```
   Chains to scan: ["base", "arbitrum"]

   Criteria:
   - market_cap < 5000000 (5M)
   - liquidity > 100000 (100K)
   - volume_change_24h > 50 (percent)
   - contract_verified = true
   ```

2. **Execute search:**

   Since Mobula doesn't have a "search" endpoint, use this strategy:

   Option A: Sample trending tokens
   ```
   - Query trending/high volume tokens on each chain
   - Filter by criteria
   ```

   Option B: User provides token list
   ```
   - User maintains a watchlist of tokens to check
   - Agent filters watchlist by criteria each scan
   ```

   Option C: Use external source + Mobula validation
   ```
   - Scrape trending from Dexscreener API (if available)
   - Validate each with Mobula for accurate data
   ```

3. **For each potential match:**

   a. **Validate criteria:**
   ```
   Use mobula_market_data:
   - Check market_cap < $5M
   - Check liquidity > $100K
   - Check volume_change_24h > +50%
   ```

   b. **Get historical data:**
   ```
   Use mobula_market_history:
   - Last 7 days
   - Analyze trend (is it breaking out? consolidating? dumping?)
   - Identify support/resistance levels
   ```

   c. **Get metadata:**
   ```
   Use mobula_metadata:
   - Contract verified?
   - Website, Twitter, Telegram links
   - Description
   ```

   d. **Check for duplicates:**
   ```
   Compare with previously flagged tokens (stored in memory):
   - If token was already sent in last 7 days → skip
   - This prevents repetitive alerts on same token
   ```

   e. **Calculate risk score (1-10):**
   ```
   Factors:
   - Liquidity/Mcap ratio (higher = lower risk)
     - >10% = low risk (+0 points)
     - 5-10% = medium (+2)
     - <5% = high (+4)

   - Contract verified? (not verified = +3 risk)

   - Volume surge magnitude
     - 50-100% = normal (+0)
     - 100-500% = FOMO risk (+2)
     - >500% = extreme FOMO (+4)

   - Holder count (if available)
     - >1000 = distributed (+0)
     - <500 = concentrated (+2)

   Final score: 10 = highest risk, 1 = lowest risk
   ```

   f. **Assign stars:**
   ```
   ⭐⭐⭐ = Low risk (1-4), strong fundamentals
   ⭐⭐ = Medium risk (5-7), decent fundamentals
   ⭐ = Higher risk (8-10), speculative
   ```

4. **Rank matches:**
   ```
   Sort by:
   1. Volume surge magnitude (bigger = more interest)
   2. Risk score (lower = better)
   3. Liquidity/Mcap ratio (higher = safer)
   ```

5. **Send top 3:**
   ```
   Format each with:
   - Token name, chain, star rating
   - Current metrics (price, mcap, liq, volume)
   - Chart analysis (trend, levels)
   - Fundamentals (contract, holders, locks)
   - Risk score + explanation
   - Why interesting (narrative)
   - Links (Dexscreener, contract, socials)
   ```

6. **Update memory:**
   ```
   Store tokens sent:
   - token_address + date_sent
   - Don't re-send for 7 days

   Also store:
   - Number of matches found
   - Scan timestamp
   ```

---

## Customization Guide

### Change Scan Frequency
Edit `interval` in frontmatter:
- `360` = 6 hours (default, balanced)
- `240` = 4 hours (more frequent, more findings)
- `720` = 12 hours (less frequent, fewer alerts)

### Change Chains
Edit `Chains` in configuration:
- Add: "ethereum", "polygon", "optimism", "solana", etc.
- Remove: Remove Base or Arbitrum if not interested

### Adjust Market Cap Range
- **Lower mcap:** `< $3M` = micro caps (higher risk/reward)
- **Higher mcap:** `< $10M` = more established (lower risk)

### Adjust Liquidity Minimum
- **Higher liq:** `> $250K` = safer entry/exit
- **Lower liq:** `> $50K` = more risky (could get trapped)

### Adjust Volume Surge Threshold
- **Higher threshold:** `> +100%` = only extreme surges (stronger signal, fewer results)
- **Lower threshold:** `> +25%` = more results (more noise)

### Change Number of Results
Edit `Results` in configuration:
- Top 1 = only best match
- Top 5 = more options per scan

### Add More Filters
You can add criteria like:
- **Price range:** Only tokens between $0.001 - $0.01
- **Holder count:** Must have >500 holders
- **Age:** Must be launched more than 7 days ago (avoid instant rugs)
- **Liquidity locked:** Must have locked LP

---

## Advanced Features

### Pattern Recognition
```
Track tokens found over time:
- "This is the 3rd Base memecoin with similar volume pattern this week"
- "BRETT pattern detected: low mcap, sudden volume, consolidation, then pump"
```

### Sentiment Analysis
```
For each token found:
- Check Twitter mentions (via web search)
- Check Telegram member count
- "This token has 5K Twitter mentions in last 24h - viral potential"
```

### Backtesting
```
Track performance of tokens you flagged:
- "Of last 20 tokens Scout found, 12 pumped further (+5% avg), 8 dumped"
- "Scout's win rate: 60%"
- This helps refine criteria
```

### Auto-Watchlist
```
Tokens found but not in top 3:
- Add to watchlist
- Monitor for another surge
- "ZORP was in your watchlist. It just surged again (+80%)"
```

### Whale Cross-Reference
```
Combine with Whale Tracker:
- Check if any tracked whales hold the token found
- "TOKEN was just bought by WHALE_2 (from your tracker) 2h ago"
- This validates the signal
```

---

## Finding Tokens to Scan

Since Mobula API doesn't have a built-in "search" endpoint, here are strategies:

### Strategy 1: User-Provided Watchlist
You maintain a list of tokens to monitor (100-200 tokens), agent filters by criteria each scan.

### Strategy 2: Trending Tokens
Use external data (Dexscreener trending, CoinGecko new listings, etc.), then validate with Mobula.

### Strategy 3: Blockchain Scanning
Agent could theoretically scan DEX events for new pools, then check each with Mobula (advanced).

### Strategy 4: Community Sourced
Agent monitors Telegram/Discord channels for token mentions, validates with Mobula.

**Recommendation for MVP:** Start with Strategy 1 (watchlist). You give the agent a list of 100-200 tokens across Base/Arbitrum, it filters by criteria every 6h.

---

## Troubleshooting

**No matches found?**
- Criteria might be too strict (try lowering volume threshold to +25%)
- Check that chains have active tokens
- Verify API key is working

**Too many results?**
- Increase criteria strictness (mcap < $3M, volume > +100%)
- Increase liquidity minimum ($200K+)

**Same tokens showing up repeatedly?**
- Ensure duplicate detection is working (check memory storage)
- Increase duplicate window (14 days instead of 7)

**Want higher quality signals?**
- Add more filters (holder count, liquidity locked, age)
- Cross-reference with whale tracker
- Only alert on verified contracts with active socials

---

## Setup Checklist

- [ ] Set your preferred chains (Base, Arbitrum, others?)
- [ ] Adjust criteria (mcap, liquidity, volume thresholds)
- [ ] Set scan interval (6h recommended to start)
- [ ] Choose number of results per scan (3 is good)
- [ ] Set alert channel (Telegram, etc.)
- [ ] Decide on token source strategy (watchlist, trending, etc.)
- [ ] Copy this file to `~/openclaw/heartbeat/`
- [ ] Restart OpenClaw agent
- [ ] Wait for first scan (6h) or test manually

---

**Your agent is now your 24/7 token discovery engine.** 🔍💎
