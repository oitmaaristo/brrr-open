# WO-MCP-001: BingX MCP Server

**Status:** Planned  
**Priority:** 1 (Highest)  
**Estimated Time:** 5-7 days  
**Competition:** NONE (first to market!)  

---

## 🎯 Objective

Build the FIRST dedicated BingX MCP server. Zero competition exists - only generic CCXT wrapper covers BingX indirectly.

---

## 📋 Requirements

### Tier 1: Market Data Tools (FREE)

| Tool | Description | Endpoint |
|------|-------------|----------|
| `get_ticker` | Current price for symbol | GET /openApi/swap/v2/quote/ticker |
| `get_orderbook` | Order book depth | GET /openApi/swap/v2/quote/depth |
| `get_klines` | OHLCV candlesticks | GET /openApi/swap/v2/quote/klines |
| `get_trades` | Recent trades | GET /openApi/swap/v2/quote/trades |
| `get_funding_rate` | Current funding rate | GET /openApi/swap/v2/quote/fundingRate |
| `list_symbols` | Available trading pairs | GET /openApi/swap/v2/quote/contracts |

### Tier 2: Account Tools (FREE)

| Tool | Description | Endpoint |
|------|-------------|----------|
| `get_balance` | Account balance | GET /openApi/swap/v2/user/balance |
| `get_positions` | Open positions | GET /openApi/swap/v2/user/positions |
| `get_open_orders` | Active orders | GET /openApi/swap/v2/trade/openOrders |
| `get_order_history` | Past orders | GET /openApi/swap/v2/trade/allOrders |
| `get_trade_history` | Executed trades | GET /openApi/swap/v2/trade/allFillOrders |

### Tier 3: Trading Tools (PAID - €0.005/action)

| Tool | Description | Endpoint | Confirmation |
|------|-------------|----------|-------------|
| `place_order` | Create new order | POST /openApi/swap/v2/trade/order | ✅ REQUIRED |
| `cancel_order` | Cancel order | DELETE /openApi/swap/v2/trade/order | ✅ REQUIRED |
| `cancel_all_orders` | Cancel all orders | DELETE /openApi/swap/v2/trade/allOpenOrders | ✅ REQUIRED |
| `close_position` | Close position | POST /openApi/swap/v2/trade/closePosition | ✅ REQUIRED |

### Tier 4: Advanced Tools (PAID - €0.01/action)

| Tool | Description | Endpoint | Confirmation |
|------|-------------|----------|-------------|
| `set_leverage` | Change leverage | POST /openApi/swap/v2/trade/leverage | ✅ REQUIRED |
| `set_margin_mode` | Isolated/Cross | POST /openApi/swap/v2/trade/marginType | ✅ REQUIRED |
| `place_batch_orders` | Multiple orders | POST /openApi/swap/v2/trade/batchOrders | ✅ REQUIRED |

---

## 🔧 Technical Specs

### Authentication

```typescript
interface BingXAuth {
  apiKey: string;      // User provides
  secretKey: string;   // User provides  
  testnet?: boolean;   // Default: false
}

// Signature generation
function sign(params: Record<string, any>, secret: string): string {
  const queryString = Object.entries(params)
    .sort(([a], [b]) => a.localeCompare(b))
    .map(([k, v]) => `${k}=${v}`)
    .join('&');
  return crypto.createHmac('sha256', secret)
    .update(queryString)
    .digest('hex');
}
```

### Rate Limits

- **Orders:** 100 requests per 10 seconds
- **Market Data:** 500 requests per minute
- **Weight System:** Each endpoint has weight, total <= 2400/min

### Error Handling

```typescript
const BINGX_ERRORS: Record<number, string> = {
  100001: 'Signature verification failed',
  100500: 'Internal server error',
  80001: 'Request too frequent',
  80012: 'Insufficient balance',
  80014: 'Order does not exist',
};
```

---

## 📁 File Structure

```
packages/bingx-mcp/
├── src/
│   ├── index.ts           # Main entry, MCP server setup
│   ├── tools/
│   │   ├── market.ts      # Market data tools
│   │   ├── account.ts     # Account tools
│   │   ├── trading.ts     # Trading tools (paid)
│   │   └── advanced.ts    # Advanced tools (paid)
│   ├── api/
│   │   ├── client.ts      # BingX API client
│   │   ├── auth.ts        # Authentication
│   │   └── types.ts       # API response types
│   └── utils/
│       ├── rate-limiter.ts
│       └── errors.ts
├── apify.json             # Apify Actor config
├── INPUT_SCHEMA.json      # Actor input schema
├── package.json
├── tsconfig.json
└── README.md
```

---

## ✅ Acceptance Criteria

1. [ ] All Tier 1 tools implemented and tested
2. [ ] All Tier 2 tools implemented and tested
3. [ ] All Tier 3 tools with human confirmation
4. [ ] Rate limiting prevents API bans
5. [ ] Error messages are user-friendly
6. [ ] Works with MCP Inspector
7. [ ] Deployed to Apify with monetization
8. [ ] Listed on 4+ MCP directories
9. [ ] README with clear setup instructions
10. [ ] No security vulnerabilities

---

## 🚀 Marketing

**Tagline:** "The first and only dedicated BingX MCP server - purpose-built, not a CCXT wrapper"

**Launch Channels:**
- Reddit: r/BingX, r/algotrading, r/CryptoCurrency
- Twitter/X: Tag @BingXOfficial
- Apify Store
- MCP Directories (Smithery.ai, PulseMCP, mcp.so, Glama.ai)

---

## 📊 Success Metrics

| Metric | Week 1 | Month 1 | Month 3 |
|--------|--------|---------|--------|
| Installs | 50 | 200 | 500 |
| Active users | 10 | 50 | 150 |
| Paid events | 100 | 1000 | 5000 |
| Revenue | €5 | €50 | €250 |
