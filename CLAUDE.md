# CLAUDE.md - BRRR MCP Servers

## 🎯 MISSION

Build and monetize MCP (Model Context Protocol) servers for cryptocurrency and trading exchanges. Target: €5,000/month revenue within 6 months.

**Primary Platform:** Apify (80% revenue share)
**Tech Stack:** TypeScript, MCP SDK, Apify Actor framework

---

## 📁 PROJECT STRUCTURE

```
brrr-mcp-servers/
├── packages/
│   ├── shared/                 # Shared utilities for ALL servers
│   │   ├── src/
│   │   │   ├── auth/           # API key handling (pass-through model)
│   │   │   ├── rate-limiting/  # Exchange-specific rate limits
│   │   │   ├── confirmation/   # Human-in-the-loop for trades
│   │   │   ├── apify/          # Monetization event helpers
│   │   │   └── types/          # Shared TypeScript types
│   │   └── package.json
│   ├── bingx-mcp/              # BingX Exchange Server (Priority 1)
│   ├── mexc-mcp/               # MEXC Exchange Server (Priority 2)
│   ├── kraken-mcp/             # Kraken Exchange Server (Priority 3)
│   └── [future-servers]/
├── docs/
│   ├── specs/                  # Server specifications (WO-MCP-XXX)
│   ├── handoffs/               # Session handoff files
│   └── research/               # Market research, API docs
├── scripts/                    # Build, deploy, test scripts
└── .github/workflows/          # CI/CD for Apify deployment
```

---

## ⚡ CRITICAL RULES

### 1. SECURITY IS NON-NEGOTIABLE

```typescript
// ✅ CORRECT: Pass-through API key model
const apiKey = input.apiKey; // User provides per-session
// Use immediately, never persist

// ❌ FORBIDDEN: Never store credentials
await storage.set('apiKey', key); // NEVER DO THIS!
```

- **NEVER** persist API keys to any storage
- **NEVER** log API keys or secrets
- **NEVER** return API keys in tool responses
- **ALWAYS** use environment variables for server-level secrets only

### 2. HUMAN-IN-THE-LOOP FOR ALL TRADES

```typescript
// ALL trading tools MUST have confirmation
server.tool(
  'place_order',
  { /* params */ },
  {
    annotations: {
      readOnlyHint: false,        // This modifies state
      destructiveHint: true,      // Requires confirmation
      idempotentHint: false,
      openWorldHint: false
    }
  },
  async (params) => { /* ... */ }
);
```

### 3. RATE LIMITING IS MANDATORY

| Exchange | Order Limit | Request Limit | Ban Threshold |
|----------|-------------|---------------|---------------|
| BingX    | 100/10s     | 500/min       | HTTP 429 |
| MEXC     | 200/10s     | 1000/min      | Auto-ban |
| Kraken   | 60/min      | 15/sec        | Lockout |

### 4. NO INVESTMENT ADVICE

```typescript
// ✅ CORRECT: Provide data and execution
"Current BTC price: $45,000. Order placed successfully."

// ❌ FORBIDDEN: Never recommend trades
"You should buy BTC now because..."  // LEGAL RISK!
```

---

## 🔧 DEVELOPMENT WORKFLOW

### Starting New Server

```bash
# 1. Create from template
cd packages/
apify create [exchange]-mcp --template ts-mcp-proxy

# 2. Install shared utilities
npm install @brrr/shared

# 3. Test with MCP Inspector
npx @modelcontextprotocol/inspector

# 4. Deploy to Apify
apify push
```

### Tool Categories (implement in order)

**Tier 1 - Market Data (FREE):**
- `get_ticker`, `get_orderbook`, `get_ohlcv`, `get_trades`

**Tier 2 - Account (FREE):**
- `get_balance`, `get_positions`, `get_orders`

**Tier 3 - Trading (PAID - €0.005/event):**
- `place_order`, `cancel_order`, `modify_order`

**Tier 4 - Advanced (PAID - €0.01/event):**
- `place_batch_orders`, `set_leverage`

---

## 💰 APIFY MONETIZATION

### Pricing Strategy

| Tool Type | Price | Rationale |
|-----------|-------|----------|
| Market data | FREE | Acquisition funnel |
| Account info | FREE | User stickiness |
| Basic trades | €0.005 | Core revenue |
| Advanced | €0.01 | Premium value |

---

## 🔗 RESOURCES

- [MCP TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk)
- [Apify MCP Guide](https://blog.apify.com/build-and-deploy-mcp-servers-typescript/)
- [MCP Inspector](https://modelcontextprotocol.io/docs/tools/inspector)

### Exchange API Docs
- [BingX API](https://bingx-api.github.io/docs/)
- [MEXC API](https://mexcdevelop.github.io/apidocs/)
- [Kraken API](https://docs.kraken.com/api/)

---

## 👥 TEAM

- **Risto** - Boss, strategy, final decisions
- **Kuldar** - Investor partner
- **CC (Claude Code)** - Development
- **Claudia** - Architecture, planning

---

## ⚠️ LEGAL DISCLAIMER

All MCP servers are TOOLS providing data access and trade execution. They do NOT provide investment advice. Users are responsible for their own trading decisions.
