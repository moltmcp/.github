# MoltMCP

**Autonomous AI Agent Payments on the Machine Payments Protocol (MCP)**
$MCP CA: `0x15f5CB7b2Acbe170101f55B20A490C6884E334e0`

---

## Overview

MoltMCP is an integration layer that enables AI agents to execute autonomous transactions using [Stripe's Machine Payments Protocol (MCP)](https://stripe.com/blog/machine-payments-protocol), with all transactions transparently settled on-chain via the **Base** blockchain.

By bridging natural language agent instructions with protocol-compliant stablecoin payments, MoltMCP transforms AI agents into first-class economic actors capable of discovering, negotiating, and paying for services without human intervention.

### Key Features

- **MCP Protocol Compliant** — Full support for Stripe's Machine Payments Protocol
- **Base-Native Settlement** — Sub-400ms finality, <$0.001 transaction fees
- **OpenClaw Integration** — First-class support for OpenClaw AI agents
- **Transparent & Auditable** — All transactions verifiable on-chain via Base
- **Developer-First** — Ship autonomous payments in minutes, not weeks

---

## Repositories

### Core Infrastructure

#### [`moltmcp-sdk`](https://github.com/jackyixuan/moltmcp-sdk)

TypeScript SDK for integrating MoltMCP payments into AI agents and applications.

**Key Features:**
- Intent-based payment API
- Automatic MCP negotiation
- Base wallet integration
- Configurable spending policies

**Quick Start:**

```ts
import { MoltMCPClient } from '@moltmcp/sdk'

const mcp = new MoltMCPClient({
  baseRpc: 'https://mainnet.base.org',
  wallet: await loadAgentWallet(),
  policies: {
    maxPerTransaction: 100_000, // 100 USDC
    dailyLimit: 1_000_000       // 1000 USDC
  }
})

const result = await mcp.payForResource({
  url: 'https://api.weather.com/premium/forecast',
  parameters: { location: 'SF', days: 7 }
})
```

---

#### [`moltmcp-contracts`](https://github.com/jackyixuan/moltmcp-contracts)

EVM smart contracts for payment escrow and settlement on Base.

**Key Features:**
- Payment intent escrow with timeouts
- Proof-of-delivery verification
- Merchant registry and verification
- Dispute resolution mechanism

**Contract Functions:**
- `initializePaymentIntent` — Create escrow and lock funds
- `finalizePaymentIntent` — Verify delivery and release funds
- `cancelPaymentIntent` — Refund after timeout
- `disputePaymentIntent` — Initiate dispute resolution

---

#### [`moltmcp-openclaw`](https://github.com/jackyixuan/moltmcp-openclaw)

Python integration for OpenClaw AI agents.

**Quick Start:**

```python
from moltmcp import MoltMCPClient

client = MoltMCPClient(
    base_rpc='https://mainnet.base.org',
    wallet=agent_wallet,
    policies={
        'max_per_transaction': 100_000,
        'daily_limit': 1_000_000
    }
)

result = await client.pay_for_resource(
    url='https://api.weather.com/premium/forecast',
    parameters={'location': 'SF', 'days': 7}
)

print(f"Payment finalized: {result.signature}")
```

---

## Getting Started

### Prerequisites

- **Node.js** 18+ (for TypeScript SDK)
- **Python** 3.9+ (for OpenClaw integration)
- **USDC** on Base mainnet (for transactions)

### Installation

#### TypeScript SDK

```bash
npm install @moltmcp/sdk
# or
yarn add @moltmcp/sdk
```

#### Python Provider

```bash
pip install moltmcp-openclaw
```

#### Smart Contracts (Base)

```bash
# Deploy via Foundry
forge install
forge build
forge deploy --network base
```

---

## Documentation

### Core Concepts

- **[Machine Payments Protocol](https://github.com/jackyixuan/moltmcp-docs/blob/main/concepts/mpp-protocol.md)** — Understanding MPP and HTTP 402
- **[Base Settlement](https://github.com/jackyixuan/moltmcp-docs/blob/main/concepts/base-settlement.md)** — How on-chain escrow works
- **[Agent Integration](https://github.com/jackyixuan/moltmcp-docs/blob/main/concepts/agent-integration.md)** — Autonomous payment workflows

### Integration Guides

- **[TypeScript SDK Guide](https://github.com/jackyixuan/moltmcp-docs/blob/main/guides/typescript-sdk.md)**
- **[OpenClaw Setup](https://github.com/jackyixuan/moltmcp-docs/blob/main/guides/openclaw-setup.md)**
- **[Merchant Integration](https://github.com/jackyixuan/moltmcp-docs/blob/main/guides/merchant-integration.md)**
- **[Security Best Practices](https://github.com/jackyixuan/moltmcp-docs/blob/main/guides/security.md)**

---

## Examples

### Basic Payment

```ts
import { MoltMCPClient } from '@moltmcp/sdk'

const client = new MoltMCPClient({
  baseRpc: process.env.BASE_RPC_URL,
  wallet: loadWalletFromEnv()
})

const payment = await client.executePayment({
  amount: 5.00,
  merchant: '0xMerchantAddress',
  token: 'USDC',
  memo: 'payment-id-12345'
})

console.log(`Payment successful: ${payment.hash}`)
```

### Service Provider (Merchant)

```ts
import { verifyPayment } from '@moltmcp/sdk'

app.get('/api/premium/data', async (req, res) => {
  const paymentCredential = req.headers['mpp-payment-credential']

  if (!paymentCredential) {
    return res.status(402).json({
      amount: "10.00",
      currency: "USD",
      payment_methods: [{
        type: "crypto",
        blockchain: "base",
        token: "USDC",
        recipient: process.env.MERCHANT_WALLET
      }]
    })
  }

  const payment = await verifyPayment(paymentCredential)

  if (payment.verified) {
    return res.json({ data: getPremiumData(), receipt: generateReceipt(payment) })
  }

  return res.status(402).json({ error: 'Invalid payment' })
})
```

---

## Performance

### Settlement Latency

| Percentile | Latency |
|------------|---------|
| p50 | <300ms |
| p95 | <500ms |
| p99 | <1000ms |

### Transaction Costs

| Fee | Amount |
|-----|--------|
| Base network fee | <$0.001 |
| MoltMCP service fee | 0.5% (capped at $5) |

### Reliability

- **SDK uptime:** 99.9%+
- **Transaction success rate:** 99.5%+

---

## Security

If you discover a security vulnerability, please email **security@moltmcp.dev**. Do not open a public issue.

---

## License

All MoltMCP repositories are licensed under the **MIT License** unless otherwise specified.

---

## Contact

- **Website:** [moltmcp.dev](https://moltmcp.dev)
- **Twitter/X:** [@moltmcp](https://twitter.com/molt_mcp)
- **GitHub:** [@jackyixuan](https://github.com/jackyixuan)
