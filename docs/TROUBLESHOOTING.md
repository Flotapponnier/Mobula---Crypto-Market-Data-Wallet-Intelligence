# Troubleshooting Guide

Common issues and solutions for the Mobula OpenClaw skill.

---

## Table of Contents

1. [Installation Issues](#installation-issues)
2. [API Authentication Issues](#api-authentication-issues)
3. [Agent Not Using the Skill](#agent-not-using-the-skill)
4. [Rate Limiting](#rate-limiting)
5. [Heartbeat Issues](#heartbeat-issues)
6. [Data Quality Issues](#data-quality-issues)
7. [Performance Issues](#performance-issues)

---

## Installation Issues

### Skill file not found

**Symptom:** Agent says "I don't have access to Mobula" or doesn't recognize crypto queries.

**Solution:**

1. Verify skill file exists:
   ```bash
   ls ~/openclaw/skills/mobula/SKILL.md
   ```

2. If missing, reinstall:
   ```bash
   cd ~/openclaw/skills/
   mkdir -p mobula
   cd mobula
   curl -o SKILL.md https://raw.githubusercontent.com/Flotapponnier/Crypto-date-openclaw/main/SKILL.md
   ```

3. Restart agent:
   ```bash
   openclaw restart
   ```

---

### Skill installed but agent doesn't recognize it

**Symptom:** File exists but agent still doesn't use it.

**Solution:**

1. Check file format (must be valid Markdown with YAML frontmatter):
   ```bash
   head -20 ~/openclaw/skills/mobula/SKILL.md
   # Should show YAML frontmatter between --- markers
   ```

2. Check file permissions:
   ```bash
   chmod 644 ~/openclaw/skills/mobula/SKILL.md
   ```

3. Check agent logs for parsing errors:
   ```bash
   openclaw logs | grep -i "mobula\|skill"
   ```

4. Force reload:
   ```bash
   openclaw reload-skills
   ```

---

## API Authentication Issues

### 401 Unauthorized Error

**Symptom:** Agent says "API key not authorized" or requests fail with 401.

**Cause:** API key is invalid, expired, or not set correctly.

**Solution:**

1. Verify API key is set:
   ```bash
   echo $MOBULA_API_KEY
   ```

   If empty, set it:
   ```bash
   export MOBULA_API_KEY="your_actual_key_here"
   ```

2. Verify the key is valid:
   - Log in to [admin.mobula.fi](https://admin.mobula.fi)
   - Check API Keys section
   - Regenerate if needed

3. Add to shell config permanently:
   ```bash
   echo 'export MOBULA_API_KEY="your_key"' >> ~/.zshrc
   source ~/.zshrc
   ```

4. Restart agent:
   ```bash
   openclaw restart
   ```

---

### 403 Forbidden Error

**Symptom:** Requests fail with 403.

**Cause:** API key doesn't have permission for this endpoint, or you're on a restricted plan.

**Solution:**

1. Check your plan at [admin.mobula.fi](https://admin.mobula.fi)
2. Verify the endpoint is available on your plan
3. Contact Mobula support if you believe this is an error

---

### API key visible in logs

**Symptom:** Worried about API key security.

**Solution:**

- OpenClaw should automatically redact API keys from logs
- Don't commit `.zshrc` or `.bashrc` to git
- Don't share logs publicly without checking for keys
- Rotate your key if it was exposed:
  1. Generate new key at [admin.mobula.fi](https://admin.mobula.fi)
  2. Update environment variable
  3. Delete old key

---

## Agent Not Using the Skill

### Agent gives generic responses instead of using Mobula

**Symptom:** You ask "What's the price of Bitcoin?" and agent says "I don't have real-time data" instead of using Mobula.

**Cause:** Agent doesn't understand when to use the skill.

**Solution:**

1. Be more explicit:
   ```
   Use Mobula to check the price of Bitcoin
   ```

2. Check skill description in SKILL.md (frontmatter):
   - Should have clear `description` field
   - Should have relevant `tags` (crypto, blockchain, etc.)

3. Retrain agent on skill usage:
   ```
   You have access to Mobula crypto data. Use it when I ask about token prices, wallets, or crypto markets.
   ```

4. Check agent's understanding:
   ```
   What skills do you have access to?
   ```

   Should list Mobula skill.

---

### Agent tries to use skill but fails

**Symptom:** Agent says "I'll check Mobula" but then returns an error.

**Cause:** Usually API key or network issue.

**Solution:**

1. Check agent logs:
   ```bash
   openclaw logs --tail 50
   ```

   Look for error messages related to Mobula API calls.

2. Test API manually:
   ```bash
   curl -H "Authorization: $MOBULA_API_KEY" \
     "https://api.mobula.io/api/1/market/data?asset=Bitcoin"
   ```

   Should return Bitcoin data. If not, API key or network issue.

3. Check network connectivity:
   ```bash
   ping api.mobula.io
   ```

4. Verify endpoint URL in SKILL.md is correct (should be `https://api.mobula.io/api/1/...`)

---

## Rate Limiting

### 429 Too Many Requests

**Symptom:** Agent says "rate limit exceeded" or requests fail with 429.

**Cause:** Exceeded your plan's rate limit (free tier: 100 requests/minute).

**Solution:**

**Immediate fix:**
- Wait 60 seconds, requests will work again

**Long-term fixes:**

1. **Reduce heartbeat frequency:**
   - Change from 30min to 60min intervals
   - Edit heartbeat files, change `interval: 30` to `interval: 60`

2. **Use batch endpoints:**
   - Instead of calling `/market/data` 10 times, use `/market/multi-data` once
   - Modify skill instructions or ask agent to batch requests

3. **Monitor fewer assets:**
   - Reduce the number of wallets, tokens, or whales you're tracking

4. **Upgrade your plan:**
   - Go to [admin.mobula.fi](https://admin.mobula.fi)
   - Upgrade for higher rate limits

5. **Cache aggressively:**
   - Store data in agent memory, reuse for 5-10 minutes before re-fetching
   - Metadata (token info, socials) rarely changes - fetch once, cache long-term

---

### Soft rate limiting (slower responses)

**Symptom:** Requests work but are slow.

**Cause:** Approaching rate limits, API is throttling.

**Solution:**
- Same as 429 above
- Optimize request patterns (fewer calls, larger batches)

---

## Heartbeat Issues

### Heartbeat not running

**Symptom:** Expected alerts or summaries aren't coming.

**Cause:** Heartbeat file not configured correctly, or agent isn't processing it.

**Solution:**

1. Verify heartbeat file location:
   ```bash
   ls ~/openclaw/heartbeat/
   ```

   Your .md files should be here.

2. Check frontmatter:
   ```yaml
   ---
   enabled: true
   interval: 30
   ---
   ```

   `enabled` must be `true`.

3. Check agent logs at expected run times:
   ```bash
   openclaw logs --follow
   ```

   Watch for heartbeat execution messages.

4. Ask agent:
   ```
   What heartbeat tasks are currently active?
   ```

5. Manually trigger (for testing):
   ```
   Run the portfolio guardian task now
   ```

---

### Heartbeat running but not alerting

**Symptom:** Heartbeat executes (you see it in logs) but no Telegram/WhatsApp alerts.

**Cause:** Messaging channel not configured, or conditions aren't met.

**Solution:**

1. **Verify messaging is configured:**
   - Check OpenClaw settings
   - Test manually: Ask agent to "Send me a test message on Telegram"

2. **Check conditions:**
   - Maybe thresholds aren't being met
   - Lower thresholds temporarily to test (e.g., alert if >5% change instead of >15%)

3. **Check channel setting in heartbeat file:**
   - Should specify "Telegram", "WhatsApp", etc.
   - Match exactly with your configured channel names

4. **Check agent memory:**
   - Agent might have stored "already alerted" state
   - Clear memory or wait for new condition

---

### Wrong heartbeat interval

**Symptom:** Heartbeat runs too often or not often enough.

**Cause:** `interval` in frontmatter is wrong.

**Solution:**

Edit the heartbeat file:
```yaml
---
interval: 30  # in minutes
---
```

- 30 = every 30 minutes
- 60 = every hour
- 1440 = once per day

Restart agent after changing.

---

## Data Quality Issues

### Token not found

**Symptom:** Agent says "Token not found" or "I couldn't find that token."

**Cause:** Token name is ambiguous, misspelled, or very new/unlisted.

**Solution:**

1. **Use contract address instead:**
   ```
   Get data for contract 0x532f27101965dd16442e59d40670faf5ebb142e4
   ```

2. **Specify the chain:**
   ```
   Get BRETT price on Base
   ```

3. **Check spelling:**
   - "Etherium" → "Ethereum"
   - "Bitcon" → "Bitcoin"

4. **Check if token exists:**
   - Search on [Dexscreener](https://dexscreener.com)
   - Get contract address from there

---

### Wallet shows zero balance but I know it has funds

**Symptom:** Portfolio query returns empty or zero.

**Cause:** Wrong chain, wrong address, or wallet hasn't been indexed yet.

**Solution:**

1. **Verify address:**
   - Check for typos
   - Make sure it's the correct address (0x... format, 42 characters)

2. **Specify chain:**
   ```
   Get portfolio for wallet 0x123... on Base
   ```

3. **Check on block explorer:**
   - Ethereum: etherscan.io
   - Base: basescan.org
   - Arbitrum: arbiscan.io

   Verify the wallet has funds there.

4. **Wait for indexing:**
   - Very new wallets might take a few minutes to index
   - Try again in 5-10 minutes

---

### Prices seem wrong or outdated

**Symptom:** Price shown is different from Dexscreener or other sources.

**Cause:** Different data sources, or cached data.

**Solution:**

1. **Check timestamp:**
   - Mobula data should be recent (within 1-2 minutes)
   - If older, might be cached

2. **Compare sources:**
   - Dexscreener, CoinGecko, CoinMarketCap might aggregate differently
   - Mobula aggregates across DEXs - price is average or weighted

3. **Specify exchange/pool:**
   - For very volatile tokens, price varies by DEX
   - "Get BRETT price on Aerodrome pool on Base"

4. **Report if consistently wrong:**
   - Open issue on [Mobula's GitHub](https://github.com/MobulaIO) or contact support

---

## Performance Issues

### Slow responses

**Symptom:** Agent takes 10+ seconds to respond to crypto queries.

**Cause:** API latency, network issues, or agent processing overhead.

**Solution:**

1. **Check API response time:**
   ```bash
   time curl -H "Authorization: $MOBULA_API_KEY" \
     "https://api.mobula.io/api/1/market/data?asset=Bitcoin"
   ```

   Should be <1 second. If slower, network or API issue.

2. **Reduce query complexity:**
   - Instead of "analyze this wallet and all tokens", break into steps
   - Agent can parallelize some requests

3. **Use cache:**
   - For repeated queries (e.g., BTC price), cache for 1-2 minutes

4. **Check agent load:**
   - Are other skills running heavy tasks?
   - Restart agent to clear any hung processes

---

### Agent crashes when using Mobula skill

**Symptom:** Agent stops responding or crashes after Mobula query.

**Cause:** Likely a bug in skill handling or agent, or very large API response.

**Solution:**

1. **Check agent logs:**
   ```bash
   openclaw logs --tail 100
   ```

   Look for error stack traces.

2. **Report the bug:**
   - [OpenClaw issues](https://github.com/openclaw/openclaw/issues) if agent crash
   - [Mobula skill issues](https://github.com/Flotapponnier/Crypto-date-openclaw/issues) if skill-specific

3. **Workaround:**
   - Simplify queries (ask for one token at a time instead of batch)
   - Reduce data returned (limit history to 7 days instead of 30)

---

## Still Need Help?

If your issue isn't listed here:

1. **Check agent logs:**
   ```bash
   openclaw logs --tail 100 > logs.txt
   ```

   Review for error messages.

2. **Open a GitHub issue:**
   - [Mobula Skill Issues](https://github.com/Flotapponnier/Crypto-date-openclaw/issues)
   - Include: error message, steps to reproduce, agent version, skill version

3. **Ask in community:**
   - OpenClaw Discord
   - Mobula Discord

4. **Contact support:**
   - Mobula API issues: support@mobula.io
   - OpenClaw issues: their support channel

---

**Most issues are quick fixes. Don't give up!** 💪
