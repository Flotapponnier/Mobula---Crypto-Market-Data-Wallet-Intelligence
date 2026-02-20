# Comment fonctionne le Skill Mobula pour OpenClaw

## 🎯 Vue d'ensemble

Le skill Mobula pour OpenClaw permet à ton agent AI de communiquer avec l'API Mobula pour obtenir des données crypto en temps réel. Voici exactement comment ça marche.

---

## 🔐 1. Authentification

### Système d'authentification Mobula

L'API Mobula utilise un système simple d'API key dans le header :

```bash
Authorization: YOUR_API_KEY
```

**Comment obtenir une clé :**
1. Va sur [admin.mobula.fi](https://admin.mobula.fi)
2. Créé un compte (gratuit)
3. Génère une API key
4. Configure-la dans OpenClaw

**Rate Limits :**
- **Free tier :** 100 requests/minute
- **Pro tier :** Rate limits plus élevés

**Important :** Pas de clé API = tu peux utiliser `demo-api.mobula.io` pour tester, mais c'est limité.

---

## 🛠️ 2. Comment OpenClaw appelle l'API Mobula

### Le flow complet

```
User dit à OpenClaw : "What's the price of Bitcoin?"
                    ↓
OpenClaw lit le SKILL.md qui contient les instructions
                    ↓
L'agent LLM comprend qu'il doit utiliser Mobula
                    ↓
Il construit une requête HTTP vers l'API Mobula:
  GET https://api.mobula.io/api/1/market/data?asset=Bitcoin
  Header: Authorization: [TA_CLE_API]
                    ↓
API Mobula répond avec les données en JSON
                    ↓
L'agent parse le JSON et formatte la réponse pour toi
                    ↓
Tu reçois : "Bitcoin is $67,234 (↑2.1% 24h)"
```

### Le SKILL.md expliqué

Le fichier `SKILL.md` que j'ai créé contient :

#### a) **Frontmatter YAML** (metadata)
```yaml
---
name: mobula
displayName: Mobula Crypto Data
description: Real-time crypto data across 88+ blockchains
requiredEnvVars:
  - MOBULA_API_KEY
---
```

Ceci dit à OpenClaw :
- Le nom du skill
- Quelles variables d'environnement sont nécessaires
- Une description pour que l'agent sache quand utiliser ce skill

#### b) **Instructions en langage naturel**

Le reste du fichier est en Markdown normal, lisible par l'AI. Il contient :

**Les endpoints disponibles :**
```markdown
### 1. Market Data (`mobula_market_data`)

**Endpoint:** GET https://api.mobula.io/api/1/market/data

**Parameters:**
- asset (required): Token name, symbol, or contract address
- blockchain (optional): Chain name

**Returns:**
- Current price, volume, market cap, etc.

**Usage examples:**
- "What's the price of Bitcoin?"
- "Get data for contract 0x123... on Base"
```

**Comment les utiliser :**
L'agent lit ces instructions et comprend :
- Quel endpoint appeler pour quelle tâche
- Quels paramètres passer
- Comment interpréter la réponse

---

## 📡 3. Les endpoints Mobula réellement disponibles

D'après le code source de `mobula-api`, voici les endpoints principaux :

### ✅ Endpoints confirmés (dans le code)

| Endpoint | Path | Controller | Fonction |
|----------|------|------------|----------|
| Market Data | `/api/1/market/data` | `MarketDataController.ts` | Prix, volume, mcap d'un token |
| Portfolio | `/api/1/wallet/portfolio` | `PortfolioController.ts` | Holdings d'un wallet cross-chain |
| Multi Portfolio | `/api/1/wallet/multi-portfolio` | `PortfolioController.ts` | Plusieurs wallets à la fois |
| DeFi Positions | `/api/1/wallet/defi-positions` | `PortfolioController.ts` | Positions LP, farming |
| Wallet Transactions | `/api/1/wallet/transactions` | `WalletTransactionsController.ts` | Historique de transactions |
| Wallet Trades | `/api/1/wallet/trades` | `WalletTradesController.ts` | Trades/swaps d'un wallet |
| Market History | `/api/1/market/history` | `AssetHistoryController.ts` | Prix historiques |
| Market Multi Data | `/api/1/market/multi-data` | `MarketMultiDataController.ts` | Batch de plusieurs tokens |
| Market Pairs | `/api/1/market/pairs` | `PairsController.ts` | Pairs de trading |
| Market Trades (Pair) | `/api/1/market/trades/pair` | `MarketTradesPairController.ts` | Trades récents d'une pair |
| Search | `/api/1/search` | `SearchController.ts` | Recherche de tokens |
| Metadata | `/api/1/metadata` | `MetadataController.ts` | Info détaillées d'un token |
| NFT | `/api/1/wallet/nft` | `NFTController.ts` | NFTs d'un wallet |
| Wallet Labels | `/api/1/wallet/labels` | `WalletLabelsController.ts` | Labels d'un wallet (whale, etc.) |

### 📋 Format de réponse

Tous les endpoints renvoient du JSON :

**Exemple : `/api/1/market/data?asset=Bitcoin`**
```json
{
  "data": {
    "id": 1,
    "name": "Bitcoin",
    "symbol": "BTC",
    "price": 67234.56,
    "market_cap": 1320000000000,
    "volume": 28400000000,
    "price_change_24h": 2.1,
    "price_change_7d": 5.4,
    "liquidity": 150000000,
    "ath": 69000,
    "atl": 3200,
    "contracts": [
      {
        "address": "0x...",
        "blockchain": "ethereum",
        "decimals": 18
      }
    ]
  }
}
```

L'agent OpenClaw lit ce JSON, extrait les données pertinentes, et te les présente de façon lisible.

---

## 🤖 4. Comment l'agent OpenClaw utilise le skill

### Scénario 1 : Requête simple

**User :** "What's the price of Ethereum?"

**Agent :**
1. Lit `SKILL.md`
2. Voit la section "Market Data"
3. Comprend qu'il doit faire : `GET /api/1/market/data?asset=Ethereum`
4. Fait la requête avec le header `Authorization: [API_KEY]`
5. Reçoit les données
6. Répond : "Ethereum (ETH) is $3,456 (↑4.3% 24h)"

### Scénario 2 : Requête complexe

**User :** "Show me the portfolio for wallet 0x742d35Cc..."

**Agent :**
1. Identifie que c'est une requête de portfolio
2. Cherche dans `SKILL.md` la section "Wallet Portfolio"
3. Voit : `GET /api/1/wallet/portfolio?wallet=ADDRESS`
4. Fait la requête
5. Parse la réponse (liste de tokens + valeurs)
6. Formatte :
```
Portfolio for 0x742d...

Total Value: $26,430

Holdings:
• ETH: $11,200 (42%) - ↑3.2% 24h
• BRETT: $9,100 (34%) - ↑12.4% 24h
• DEGEN: $6,130 (23%) - ↓2.1% 24h
```

### Scénario 3 : Heartbeat proactif (monitoring 24/7)

**Setup (dans `~/openclaw/heartbeat/portfolio-guardian.md`) :**
```markdown
---
interval: 30  # minutes
enabled: true
---

# Portfolio Guardian

Every 30 minutes:
1. Check wallet 0x123... via mobula_wallet_portfolio
2. If any token drops >15% in 24h → alert on Telegram
3. If allocation >40% on one token → suggest rebalance
```

**Ce qui se passe :**
- Toutes les 30 minutes, l'agent exécute ce heartbeat
- Il appelle `/api/1/wallet/portfolio?wallet=0x123...`
- Compare avec les valeurs précédentes (stockées en mémoire)
- Si condition remplie → te ping sur Telegram

**Exemple d'alerte :**
```
⚠️ Portfolio Alert

BRETT is down -18% in the last 24 hours.

Details:
• Current price: $0.073 (was $0.089)
• Your holding: 112,329 BRETT
• Value: $8,200 (down from $10,000)
• Loss: -$1,800 (-18%)
```

---

## 🔧 5. Configuration technique

### Variables d'environnement

L'agent OpenClaw a besoin de :

```bash
MOBULA_API_KEY="ta_cle_api_ici"
```

Cette variable est lue par OpenClaw et injectée dans toutes les requêtes Mobula.

### Où configurer

**Option 1 : Shell config (permanent)**
```bash
echo 'export MOBULA_API_KEY="ta_cle"' >> ~/.zshrc
source ~/.zshrc
```

**Option 2 : `.env` file dans OpenClaw**
```
# ~/openclaw/.env
MOBULA_API_KEY=ta_cle
```

**Option 3 : Temporaire (session actuelle)**
```bash
export MOBULA_API_KEY="ta_cle"
```

---

## 🧠 6. L'intelligence du skill : pourquoi c'est puissant

### Ce qui rend ça différent d'un simple script

**Un script classique :**
```python
# Rigid, un seul use case
def get_btc_price():
    response = requests.get("https://api.mobula.io/api/1/market/data?asset=Bitcoin")
    return response.json()["data"]["price"]
```

**Avec OpenClaw + Skill Mobula :**
L'agent peut :
- **Interpréter des requêtes en langage naturel :** "Is Bitcoin pumping?" → traduit en appel API + analyse du price_change_24h
- **Combiner plusieurs endpoints :** "Compare BTC and ETH performance" → 2 appels API + comparaison
- **Contextualiser :** Pas juste "price is $67K" mais "Bitcoin is $67,234 (↑2.1% 24h), broke $67K resistance, grinding higher"
- **Être proactif :** Heartbeat pour monitorer sans que tu demandes

### Exemple avancé

**User :** "Should I buy this token?" (donne un contrat address)

**Agent :**
1. Appelle `/api/1/market/data` → prix, mcap, volume
2. Appelle `/api/1/market/history` → trend 7d et 30d
3. Appelle `/api/1/market/trades` → activité récente (accumulation vs distribution)
4. Appelle `/api/1/metadata` → info projet, socials
5. **Synthétise tout ça** :

```
TOKEN Analysis

Price: $0.042 (↓8% 24h, ↑156% 7d)
- Near 7d high, down from $0.051
- Still +400% from 30d low

Fundamentals:
- Mcap: $2.1M (small cap, high risk/reward)
- Liquidity: $280K (decent for size)
- Volume: $1.2M 24h (healthy)

On-chain:
- Recent trades show buying pressure (3:1 buy/sell ratio)
- 2 large buys ($50K+) in last 2h

Project:
- Contract verified ✓
- Active Twitter (12K followers)
- [Website link]

⚠️ Risk: Small cap, high volatility. Don't ape more than you can lose.
```

Aucun script classique ne peut faire ça. C'est l'AI qui **raisonne** avec les données.

---

## 🚨 7. Limites et contraintes

### Ce que le skill NE FAIT PAS

❌ **Exécuter des trades** : Le skill donne des données, pas d'exécution on-chain
❌ **WebSocket en continu** : Les requêtes sont ponctuelles (pas de stream permanent)
❌ **Prédire le futur** : Il analyse les données passées/actuelles, pas de prédiction

### Rate Limits à respecter

**Free tier :**
- 100 requests/minute
- Si heartbeat toutes les 30min pour 5 wallets = ~10 requests/heure = OK
- Mais si tu fais un scan de 100 tokens = tu vas dépasser

**Solution :** Utiliser les endpoints batch :
- `/api/1/market/multi-data` → 1 request pour 500 tokens au lieu de 500 requests

---

## 📝 8. Ce que tu peux demander à l'agent

### Questions simples
- "What's the price of Bitcoin?"
- "Show me BRETT's market cap"
- "Get portfolio for wallet 0x123..."

### Questions complexes
- "Compare performance of BTC, ETH, and SOL today"
- "Is this token near its ATH or ATL?"
- "What did this whale wallet buy in the last 24h?"

### Setup de monitoring
- "Monitor my wallet 0x123 and alert me if anything drops more than 15%"
- "Track these 3 whale wallets and tell me when they buy something"
- "Find tokens on Base under $5M mcap with volume surge"

### Analyse
- "Analyze this token: 0xContractAddress on Base"
- "Should I buy this token?" (donne contract)
- "What's the sentiment on this wallet's trades?"

---

## 🎉 Conclusion

**Le skill Mobula pour OpenClaw fonctionne comme un pont intelligent entre :**
- **L'humain** (toi) qui pose des questions en langage naturel
- **L'AI** (OpenClaw) qui comprend l'intention et sait quoi faire
- **L'API Mobula** qui fournit les données brutes
- **Le résultat** qui te revient sous forme analysée et actionnable

C'est pas juste "appeler une API", c'est avoir un **analyste crypto AI 24/7** qui :
1. Comprend ce que tu veux
2. Sait comment obtenir les données
3. Les analyse intelligemment
4. Te présente des insights
5. Peut agir de façon proactive

**Et tout ça sans que tu écrives une seule ligne de code.** 🚀
