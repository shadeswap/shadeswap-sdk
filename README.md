<div align="center">

# ShadeSwap

**Private swaps for humans and AI agents — powered by zero-knowledge proofs**

Swap in the Shade. No trace. No identity. No limits.

[![npm version](https://img.shields.io/npm/v/shadeswap?color=8B5CF6&label=npm)](https://www.npmjs.com/package/shadeswap)
[![License: MIT](https://img.shields.io/badge/license-MIT-8B5CF6)](LICENSE)
[![CI](https://github.com/ShadeSwapAgent/shadeswap-sdk/actions/workflows/ci.yml/badge.svg)](https://github.com/ShadeSwapAgent/shadeswap-sdk/actions)
[![Twitter](https://img.shields.io/twitter/follow/ShadeSwapAgent?style=social)](https://x.com/ShadeSwapAgent)

[Website](https://shadeswap.io) · [Docs](https://github.com/ShadeSwapAgent/shadeswap-sdk/tree/main/docs) · [NPM](https://www.npmjs.com/package/shadeswap)

</div>

---

## What is ShadeSwap?

ShadeSwap routes token swaps through a zero-knowledge privacy layer on Solana. Every trade is validated using ZK proofs — hiding wallet identity, transaction amounts, and routing paths.

Built for humans who want privacy. Built for AI agents that need to swap autonomously.

```ts
import { ShadeSwap } from 'shadeswap'

const swap = new ShadeSwap({ agentId: 'my-agent' })

const result = await swap.swap({
  from: 'SOL',
  to: 'USDC',
  amount: 10,
  privacy: 'zk',
  rules: {
    maxSlippage: 1,
    requireApproval: false,
  }
})
// → swapped privately in 280ms. no trace.
```

---

## Install

```bash
npm install shadeswap
```

---

## Features

| Feature | Description |
|---------|-------------|
| ZK Privacy | Every swap routed through zero-knowledge proof layer |
| Agent Native | Built for autonomous AI agents via MCP integration |
| Spending Rules | maxSlippage, requireApproval, token whitelist |
| Jupiter Routing | Best price routing across all Solana DEXes |
| MCP Server | Native tool for Claude, GPT, Cursor |
| No Identity | Wallet address never exposed on-chain |

---

## Usage

### Basic swap

```ts
import { ShadeSwap } from 'shadeswap'

const client = new ShadeSwap()

const result = await client.swap({
  from: 'SOL',
  to: 'USDC',
  amount: 5,
  privacy: 'zk',
})

console.log(result.txHash)    // 64-char hash
console.log(result.amountOut) // 5 USDC (minus fees)
```

### With approval gate

```ts
const client = new ShadeSwap({
  agentId: 'trading-agent-01',
  onApprovalRequired: async (req) => {
    console.log(`Agent wants to swap ${req.amount} ${req.from} → ${req.to}`)
    return true // or false to block
  }
})

const result = await client.swap({
  from: 'SOL',
  to: 'BONK',
  amount: 1,
  rules: {
    requireApproval: true,
    maxSlippage: 2,
    allowedTokens: ['USDC', 'BONK', 'JUP'],
  }
})
```

### Get quote

```ts
const quote = await client.quote('SOL', 'USDC', 10)
console.log(quote.estimatedOutput)
console.log(quote.priceImpact)
console.log(quote.route) // ['SOL', 'USDC']
```

### MCP integration (Claude / GPT / Cursor)

```ts
import { ShadeSwapMCP } from 'shadeswap'

const server = new ShadeSwapMCP({
  agentId: process.env.AGENT_ID,
})

// tools() returns MCP-compatible tool definitions
const tools = server.tools()

// call() executes the tool
const result = await server.call('swap', {
  from: 'SOL',
  to: 'USDC',
  amount: 5,
  privacy: 'zk',
})
```

**Claude Desktop config (`claude_desktop_config.json`):**
```json
{
  "mcpServers": {
    "shadeswap": {
      "command": "node",
      "args": ["dist/cli.js", "--mcp"],
      "env": {
        "AGENT_ID": "your-agent-id"
      }
    }
  }
}
```

---

## Spending Rules

```ts
rules: {
  requireApproval: true,     // gate every swap before execution
  maxSlippage: 1,            // block if slippage > 1%
  maxAmountUsd: 500,         // block swaps over $500
  allowedTokens: ['USDC', 'SOL', 'JUP'], // whitelist
}
```

---

## Roadmap

- [x] Core swap SDK
- [x] ZK privacy layer
- [x] MCP integration
- [x] Spending rules engine
- [ ] Multi-hop ZK routing (Q2 2026)
- [ ] Cross-chain swaps (Q3 2026)
- [ ] Agent portfolio management (Q3 2026)
- [ ] DAO governance (Q4 2026)

---

## License

MIT © 2026 ShadeSwap
