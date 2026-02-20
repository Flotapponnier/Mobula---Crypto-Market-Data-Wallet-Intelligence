# Detailed Setup Guide

Complete step-by-step guide to install and configure the Mobula skill for OpenClaw.

---

## Prerequisites

Before you begin, make sure you have:

1. **OpenClaw installed and running**
   - If not: Follow the [OpenClaw installation guide](https://openclaw.ai)
   - Verify: Run `openclaw --version`

2. **Node.js 22+**
   - Check: `node --version`
   - If needed: [Download Node.js](https://nodejs.org)

3. **A messaging app configured** (Telegram, WhatsApp, Discord, etc.)
   - OpenClaw uses these for alerts
   - Configure during `openclaw onboard` if you haven't already

---

## Step 1: Get Your Mobula API Key

### 1.1 Sign Up

1. Go to [mobula.io](https://mobula.io)
2. Click "Sign Up" or "Get Started"
3. Complete registration (email + password)
4. Verify your email

### 1.2 Get API Key

1. Log in to [mobula.io](https://mobula.io)
2. Navigate to "API Keys" (usually in dashboard or settings)
3. Click "Create New API Key" or copy your existing key
4. **Save this key** - you'll need it in the next step

### 1.3 Free vs Paid Plans

**Free Tier:**
- 100 requests/minute
- All endpoints available
- Great for getting started

**Paid Tiers:**
- Higher rate limits
- Priority support
- Better for intensive monitoring (multiple wallets, frequent checks)

For most users, free tier is sufficient. Upgrade later if needed.

---

## Step 2: Install the Skill

Choose one of the three methods below:

### Method A: Via ClawHub (Easiest)

1. Open your messaging app connected to OpenClaw (e.g., Telegram)
2. Send this message to your agent:
   ```
   Install the Mobula skill from ClawHub
   ```
3. Your agent will find and install it automatically
4. You'll see a confirmation message

**Note:** This assumes the skill is published on ClawHub. If not available yet, use Method B or C.

---

### Method B: Via URL (Quick)

1. Send this message to your agent:
   ```
   Install skill from https://raw.githubusercontent.com/Flotapponnier/Crypto-date-openclaw/main/SKILL.md
   ```
2. Agent downloads and installs the skill
3. Confirmation message appears

---

### Method C: Manual (Full Control)

1. Open terminal and navigate to OpenClaw skills directory:
   ```bash
   cd ~/openclaw/skills/
   ```

2. Create directory for Mobula skill:
   ```bash
   mkdir mobula
   cd mobula
   ```

3. Download the skill file:
   ```bash
   curl -o SKILL.md https://raw.githubusercontent.com/Flotapponnier/Crypto-date-openclaw/main/SKILL.md
   ```

4. Verify download:
   ```bash
   ls -la
   # You should see SKILL.md
   ```

5. Restart your agent (see Step 4)

---

## Step 3: Configure API Key

You need to set the `MOBULA_API_KEY` environment variable so your agent can authenticate with Mobula's API.

### Option A: Temporary (Current Session Only)

```bash
export MOBULA_API_KEY="your_api_key_here"
```

**Note:** This only lasts until you close the terminal. Use Option B for permanent setup.

---

### Option B: Permanent (Recommended)

Add to your shell configuration file so it persists across sessions.

**For zsh (macOS default, modern Linux):**

```bash
echo 'export MOBULA_API_KEY="your_api_key_here"' >> ~/.zshrc
source ~/.zshrc
```

**For bash (older macOS, many Linux distros):**

```bash
echo 'export MOBULA_API_KEY="your_api_key_here"' >> ~/.bashrc
source ~/.bashrc
```

**For fish:**

```bash
echo 'set -x MOBULA_API_KEY "your_api_key_here"' >> ~/.config/fish/config.fish
source ~/.config/fish/config.fish
```

---

### Verify API Key is Set

```bash
echo $MOBULA_API_KEY
# Should print your API key
```

If it prints nothing, the variable isn't set. Double-check the steps above.

---

## Step 4: Restart Your Agent

For the skill and API key to take effect:

```bash
openclaw restart
```

Or if using the daemon:

```bash
openclaw stop
openclaw start
```

Wait ~10 seconds for the agent to fully restart.

---

## Step 5: Verify Installation

### 5.1 Check Skill is Loaded

Ask your agent (via Telegram, WhatsApp, etc.):

```
List all installed skills
```

You should see "Mobula Crypto Data" or similar in the list.

---

### 5.2 Test Basic Query

Ask your agent:

```
What's the price of Bitcoin?
```

**Expected response:**
```
Bitcoin (BTC) is currently $67,234 (↑2.1% 24h).

Market data:
- Market cap: $1.32T
- Volume 24h: $28.4B
- 7d change: +5.4%

Bitcoin is grinding higher, recently broke $67K resistance.
```

If you get this (or similar), **installation successful!** ✅

---

### 5.3 Test Wallet Query

Ask your agent:

```
Show the portfolio for wallet 0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb
```

**Expected response:**
Should show all tokens held in that wallet across all chains, with values and allocations.

If it works, you're all set! 🎉

---

## Step 6: Set Up Monitoring (Optional but Recommended)

Now that basic queries work, set up proactive monitoring using heartbeat templates.

### 6.1 Portfolio Guardian

1. Download the template:
   ```bash
   cd ~/openclaw/heartbeat/
   curl -o portfolio-guardian.md https://raw.githubusercontent.com/Flotapponnier/Crypto-date-openclaw/main/examples/heartbeat-portfolio-guardian.md
   ```

2. Edit the file:
   ```bash
   nano portfolio-guardian.md
   # or use your preferred editor
   ```

3. Replace `YOUR_WALLET_ADDRESS_HERE` with your actual wallet address

4. Save and exit

5. Restart agent:
   ```bash
   openclaw restart
   ```

6. Your agent will now monitor your portfolio every 30 minutes! 🛡️

---

### 6.2 Other Templates

Repeat the process for other templates you want:

**Whale Tracker:**
```bash
curl -o whale-tracker.md https://raw.githubusercontent.com/Flotapponnier/Crypto-date-openclaw/main/examples/heartbeat-whale-tracker.md
```

**Token Scout:**
```bash
curl -o token-scout.md https://raw.githubusercontent.com/Flotapponnier/Crypto-date-openclaw/main/examples/heartbeat-token-scout.md
```

**Market Brief:**
```bash
curl -o market-brief.md https://raw.githubusercontent.com/Flotapponnier/Crypto-date-openclaw/main/examples/heartbeat-market-brief.md
```

Edit each to customize for your needs.

---

## Troubleshooting

### "API key not found" or 401 errors

**Cause:** The `MOBULA_API_KEY` environment variable isn't set or isn't accessible to the agent.

**Solution:**
1. Verify the variable is set: `echo $MOBULA_API_KEY`
2. If empty, repeat Step 3
3. Make sure you restarted the agent AFTER setting the variable
4. If using systemd or a daemon manager, you may need to set the variable in the service config

---

### "Skill not found" or agent doesn't use Mobula

**Cause:** Skill file isn't in the right location or agent hasn't detected it.

**Solution:**
1. Check skill file exists:
   ```bash
   ls ~/openclaw/skills/mobula/SKILL.md
   ```
2. If missing, repeat Step 2 (install the skill)
3. Restart agent
4. Try asking explicitly: "Use the Mobula skill to check Bitcoin price"

---

### Rate limit errors (429)

**Cause:** You're exceeding the free tier limit (100 requests/minute).

**Solution:**
- Reduce heartbeat frequency (e.g., 60min instead of 30min)
- Monitor fewer wallets/tokens
- Use batch endpoints (`/market/multi-data`) instead of individual calls
- Consider upgrading your Mobula plan at [mobula.io](https://mobula.io)

---

### Agent gives generic responses, doesn't use Mobula

**Cause:** Agent might not understand when to use the skill, or skill description isn't clear.

**Solution:**
- Be explicit: "Use Mobula to check..."
- Check agent logs for errors related to the skill
- Verify skill frontmatter has proper tags and description
- Update to latest skill version

---

### Heartbeat tasks not running

**Cause:** Heartbeat file not in correct location, or interval is too long.

**Solution:**
1. Verify location:
   ```bash
   ls ~/openclaw/heartbeat/
   # Should see your .md files
   ```
2. Check `enabled: true` in frontmatter
3. Check logs around expected run times
4. Try a shorter interval (10min) to test
5. Ask agent: "What heartbeat tasks are active?"

---

## Advanced Configuration

### Multiple API Keys (for different projects)

If you want different keys for different agents:

1. Set environment variable in agent-specific config
2. Or use `.env` files per agent workspace
3. Or pass as parameter when starting agent (depends on OpenClaw's config system)

---

### Custom Skill Modifications

You can edit `SKILL.md` to:
- Add custom instructions
- Change default behaviors
- Add domain-specific logic

After editing:
1. Save the file
2. Restart agent
3. Changes take effect

---

### Webhook Integration

For advanced users: Set up webhooks to trigger actions based on Mobula data.

Example: When portfolio drops 10%, trigger a webhook to your trading bot.

(Implementation depends on OpenClaw's webhook system)

---

## Next Steps

Now that everything is set up:

1. **Explore examples:** Check [EXAMPLES.md](./EXAMPLES.md) for more use case ideas
2. **Customize heartbeats:** Edit templates to match your needs
3. **Join the community:** Share your setups, get help, contribute
4. **Monitor and iterate:** See what works, adjust thresholds and intervals

---

## Getting Help

**Skill Issues:**
- [GitHub Issues](https://github.com/Flotapponnier/Crypto-date-openclaw/issues)

**Mobula API Issues:**
- [Mobula Docs](https://docs.mobula.io)
- [Mobula Discord](https://discord.gg/mobula) (check their site for link)

**OpenClaw Issues:**
- [OpenClaw Docs](https://openclaw.ai)
- [OpenClaw Discord](https://discord.gg/openclaw) (check their site)

---

**You're all set! Enjoy your 24/7 crypto intelligence agent.** 🚀
