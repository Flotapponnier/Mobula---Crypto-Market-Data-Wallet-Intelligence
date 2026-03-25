# Legacy Mobula Skill

⚠️ **This is the original monolithic skill (v1.0.1)**

## Deprecated

This skill has been split into 3 focused skills for better maintainability and usability:

1. **[mobula-prices](../mobula-prices/)** - Real-time prices & market data
2. **[mobula-wallet](../mobula-wallet/)** - Portfolio tracking & wallet analytics
3. **[mobula-alerts](../mobula-alerts/)** - 24/7 monitoring & smart alerts

## Migration Guide

**If you're using the legacy skill:**

### For price checking only
→ Switch to **mobula-prices**
```
Install mobula-prices from ClawHub
```

### For wallet tracking only
→ Switch to **mobula-wallet**
```
Install mobula-wallet from ClawHub
```

### For monitoring/alerts
→ Install **all 3 skills**
```
Install mobula-prices, mobula-wallet, and mobula-alerts from ClawHub
```

## Why Split?

**Original problem:** 7 endpoints in one skill
- Too broad scope
- Hard to maintain
- Confusing for new users
- Not following ClawHub best practices

**Solution:** Modular skills with precise scope
- Install only what you need
- Easier to understand
- Follows pattern of top skills (database-query, RootData)
- Better discoverability on ClawHub

## Changelog

**v1.0.1** (2024-02-20): Security & clarity improvements
- Added comprehensive Privacy & Security section
- Clarified API key requirement
- Enhanced authentication documentation
- Added best practices for API key management
- Explicit warnings about wallet address privacy

**v1.0.0** (2024-02-20): Initial release
- 7 core endpoints
- Heartbeat monitoring patterns
- Smart alert examples
- Comprehensive documentation

---

## Support

For the new modular skills:
- **GitHub:** https://github.com/Flotapponnier/Crypto-date-openclaw
- **Issues:** https://github.com/Flotapponnier/Crypto-date-openclaw/issues

This legacy folder is kept for reference only.
