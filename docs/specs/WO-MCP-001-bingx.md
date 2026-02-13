# WO-MCP-001: BingX MCP Server - ACTIVE

**Status:** 🟢 IN PROGRESS  
**Assigned:** CC (Claude Code)  
**Mode:** BRRR MODE (autonomous, no approval needed)  
**Started:** 2026-02-13  
**Target:** 2026-02-20 (7 days)  

---

## 🎯 MISSION

Build the FIRST dedicated BingX MCP server. Ship to Apify within 7 days.

**Zero competition = first mover advantage. JUST BUILD IT.**

---

## 📋 BRRR MODE RULES

1. **NO ASKING PERMISSION** - just build
2. **NO WAITING FOR APPROVAL** - commit and push
3. **DECISIONS = YOURS** - pick the best option and go
4. **STUCK > 30 MIN?** - leave TODO comment, move on
5. **DONE > PERFECT** - ship fast, iterate later

---

## ✅ CHECKLIST (in order)

### Phase 1: Setup (Day 1)
- [ ] Initialize TypeScript project in `packages/bingx-mcp/`
- [ ] Set up Apify Actor structure (`apify.json`, `INPUT_SCHEMA.json`)
- [ ] Create shared utilities in `packages/shared/`
- [ ] Basic MCP server skeleton with health check

### Phase 2: Market Data - FREE tier (Day 2-3)
- [ ] `get_ticker` - current price
- [ ] `get_orderbook` - order book depth  
- [ ] `get_klines` - OHLCV candlesticks
- [ ] `get_trades` - recent trades
- [ ] `get_funding_rate` - funding rate
- [ ] `list_symbols` - available pairs

### Phase 3: Account Data - FREE tier (Day 3-4)
- [ ] `get_balance` - account balances
- [ ] `get_positions` - open positions
- [ ] `get_open_orders` - active orders
- [ ] `get_order_history` - past orders

### Phase 4: Trading - PAID tier (Day 4-5)
- [ ] `place_order` - create order (with confirmation!)
- [ ] `cancel_order` - cancel order
- [ ] `close_position` - close position
- [ ] Apify monetization events (€0.005/trade)

### Phase 5: Polish & Deploy (Day 6-7)
- [ ] Rate limiting implementation
- [ ] Error handling for all BingX error codes
- [ ] MCP Inspector testing
- [ ] README with setup instructions
- [ ] Deploy to Apify
- [ ] List on directories (Smithery, PulseMCP, mcp.so, Glama)

---

## 🔧 TECHNICAL DECISIONS (CC decides)

| Decision | Options | CC picks |
|----------|---------|----------|
| HTTP client | fetch / axios / got | ? |
| Rate limiter | custom / bottleneck / p-limit | ? |
| Testing | vitest / jest / none initially | ? |
| Logging | console / pino / winston | ? |

**Rule:** Pick one, document in code, move on.

---

## 📁 FILE STRUCTURE TO CREATE

```
packages/bingx-mcp/
├── src/
│   ├── index.ts           # MCP server entry
│   ├── tools/
│   │   ├── market.ts      # Tier 1: Market data
│   │   ├── account.ts     # Tier 2: Account data
│   │   └── trading.ts     # Tier 3-4: Trading
│   ├── api/
│   │   ├── client.ts      # BingX REST client
│   │   ├── auth.ts        # HMAC-SHA256 signing
│   │   └── types.ts       # TypeScript types
│   └── utils/
│       ├── rate-limiter.ts
│       └── errors.ts
├── apify.json
├── INPUT_SCHEMA.json
├── package.json
├── tsconfig.json
└── README.md

packages/shared/
├── src/
│   ├── index.ts
│   ├── confirmation.ts    # Human-in-the-loop helper
│   └── monetization.ts    # Apify billing helper
└── package.json
```

---

## 🔗 RESOURCES

- **BingX API Docs:** https://bingx-api.github.io/docs/
- **MCP TypeScript SDK:** https://github.com/modelcontextprotocol/typescript-sdk
- **Apify Actor Guide:** https://docs.apify.com/platform/actors
- **MCP Inspector:** `npx @modelcontextprotocol/inspector`

---

## 📊 PROGRESS LOG

| Date | What got done |
|------|---------------|
| 2026-02-13 | WO created, repo setup |
| ... | CC fills this in |

---

## 🚨 BLOCKERS

_None yet. CC adds here if stuck._

---

**REMEMBER: BRRR MODE = NO PERMISSION NEEDED. JUST SHIP IT.** 🖨️💰
