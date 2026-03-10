![Base](header.png)

## 🌀Mibboverse

A Collaborative Ecosystem for AI-Powered Alpha.

We are building a platform where everyone can earn from their Web3 experience and knowledge using AI.

> **Mibboverse Prototype** — a monorepo exploring the intersection of AI agents and onchain economies.
> Agents register identities onchain, monetize via **x402** micropayments, and are accessible through a local UI demo.

## 📖 Documentation:
- **[Project overview](https://mibboverse.gitbook.io/mibbopaper)** — Core concept, vision, and general project description.
- **[Core Architecture](onchain-core-avax/docs/ARCHITECTURE.md)** — protocol design, ERC-8004/x402 integration, and contract lifecycle.


> [!IMPORTANT]
> ### 🧪 Prototype Limitations
> For ease of testing in this early prototype, **onchain access gating (AgentPass) and complex settlement logic are not yet fully integrated into the backend API**. 
> The UI currently demonstrates the **x402 micropayment flow** (client-side signing and header injection), while the smart contracts (`onchain-core-avax`) simulate the full lifecycle independently via Hardhat scripts. End-to-end integration is a work in progress.

## What's Inside

```
mibboverse-prototype/
├── 📂 demo/                # Vite + React UI — interact with agents via x402
└── 📂 onchain-core-avax/   # Hardhat project — smart contracts & deployment
```


| Module | Stack | Purpose |
|---|---|---|
| `demo` | React · Vite · Tailwind · x402 | Local UI to browse and call agent APIs via x402 micropayments |
| `onchain-core-avax` | Solidity · Hardhat · TypeScript · Viem | Agent identity (ERC-8004), access control (AgentPass), treasury (ERC-8004 custodian) |

## Architecture Overview

Mibboverse transforms AI agents into **sovereign economic entities** using two core protocols:

- **ERC-8004** — binds each agent permanently to its creator via a Custodial Treasury. Agents cannot be transferred, making their on-chain history a verifiable reputation.
- **x402** — a pay-per-use monetization layer. Every agent API call is a micropayment signed by the user's wallet — no API keys, no subscriptions.

```
User Wallet
    │
    ▼
[demo UI]  ──x402 signed request──▶  [Agent x402 API]
                                            │
                                     validates payment
                                            │
                                     returns response
                                            │
                              [onchain-core-avax contracts]
                         AgentRegistry · AgentTreasury · AgentPass
```

## Deployed Contracts (Avalanche Fuji Testnet)

| Address  | Name | Contracts Overview |
| ------------- | ------------- | ------------- |
|  [0xA8A6Ac2C54caa755C0B91930AC6e80a726C5B5E8](https://testnet.snowtrace.io/address/0xA8A6Ac2C54caa755C0B91930AC6e80a726C5B5E8) | AgentTreasury | ERC-8004 Custodian & Meta-Tx Manager |
|  [0xFc2aF8B306fa27f5FD8739B7B1D713DAe263bAFe](https://testnet.snowtrace.io/address/0xFc2aF8B306fa27f5FD8739B7B1D713DAe263bAFe) | AgentRegistry | Agent Lifecycle Orchestrator & Beneficial Ownership |
|  [0xc5e7Ba73e9d3Fb1B80037F56d03f707Af0ebAF32](https://testnet.snowtrace.io/address/0xc5e7Ba73e9d3Fb1B80037F56d03f707Af0ebAF32) | AgentPass | NFT-based access control & membership logic |
|  [0x8004A818BFB912233c491871b3d84c89A494BD9e](https://testnet.snowtrace.io/address/0x8004A818BFB912233c491871b3d84c89A494BD9e) | ERC-8004 IdentityRegistry | Agent Identity Registry Proxy |

## 🚀 Quick start

1. Clone the repository:
    ```bash
    git clone https://github.com/Matik86/mibboverse-prototype-avax-build-games.git
    cd mibboverse-prototype
    ```

2. Run the Demo UI:
    ```bash
    cd demo
    npm install
    npm run dev
    ```

3.  Run the Onchain Core:
    ```bash
    cd onchain-core-avax
    cp .env.example .env   # fill in PRIVATE_KEY and FUJI_RPC_URL
    npm install
    npx hardhat compile
    npx hardhat run scripts/interaction.ts --network avalancheFuji
    ```

Full setup details in each module's **README**:
- [onchain-core-avax/README.md](onchain-core-avax/README.md)
- [demo/README.md](demo/README.md)

## 🏛️ Agentic Economy

| Concept | Implementation |
| ------------- | ------------- |
| Agent Identity | 	ERC-8004 soul-bound NFT, custodied in `AgentTreasury` |
| Monetization | x402 micropayment per API call, settled onchain & pass purchase |
| Access Control | `AgentPass` — soulbound pass purchased with $AGENT token |
| Usage Tracking | Backend relayer writes session data onchain |
| Protocol Stability | $MIBBO ecosystem token for fee splits and burns |

> [!TIP]
> **Dive Deeper:** For technical diagrams, contract breakdowns, and the full lifecycle of an agent, read our [**Core Architecture Documentation**](onchain-core-avax/docs/ARCHITECTURE.md).
