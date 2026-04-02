<div align="center">

<img src="https://raw.githubusercontent.com/mppfinance/shadeswap-sdk/main/bannershade.png" width="100%" alt="ShadeSwap" />

# ShadeSwap

**Private swaps for humans and AI agents — powered by zero-knowledge proofs**

Swap in the Shade. No trace. No identity. No limits.

[![Website](https://img.shields.io/badge/WEBSITE-shadeswap.org-8B5CF6?style=for-the-badge&logo=safari&logoColor=white)](https://www.shadeswap.org)
[![Twitter](https://img.shields.io/badge/TWITTER-%40ShadeSwapAgent-8B5CF6?style=for-the-badge&logo=x&logoColor=white)](https://x.com/ShadeSwapAgent)
[![npm](https://img.shields.io/npm/v/shadeswap?style=for-the-badge&label=NPM&color=8B5CF6&logo=npm&logoColor=white)](https://www.npmjs.com/package/shadeswap)
[![Stars](https://img.shields.io/github/stars/mppfinance/shadeswap-sdk?style=for-the-badge&label=STARS&color=8B5CF6)](https://github.com/mppfinance/shadeswap-sdk)

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
// swapped privately in 280ms. no trace.
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

console.log(result.txHash)
console.log(result.amountOut)
```

### With approval gate

```ts
const client = new ShadeSwap({
  agentId: 'trading-agent-01',
  onApprovalRequired: async (req) => {
    console.log(`Agent wants to swap ${req.amount} ${req.from} to ${req.to}`)
    return true
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

### MCP integration

```ts
import { ShadeSwapMCP } from 'shadeswap'

const server = new ShadeSwapMCP({ agentId: process.env.AGENT_ID })
const tools = server.tools()
const result = await server.call('swap', { from: 'SOL', to: 'USDC', amount: 5 })
```

---

## Spending Rules

```ts
rules: {
  requireApproval: true,
  maxSlippage: 1,
  maxAmountUsd: 500,
  allowedTokens: ['USDC', 'SOL', 'JUP'],
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

---

## License

MIT © 2026 ShadeSwap
