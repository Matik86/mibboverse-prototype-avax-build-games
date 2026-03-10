![Base](docs\images\main_header.png)

> This folder contains the smart contracts that power the AI Agent Ecosystem.

📖 **[Read the Core Architecture Documentation](docs/ARCHITECTURE.md)** for deep dives into protocol design, workflow schemas, and ERC-8004/x402 integration.

## Deployed Contracts

All contracts are deployed on **Avalanche Fuji Testnet**

| Address  | Name | Contracts Overview |
| ------------- | ------------- | ------------- |
|  [0xA8A6Ac2C54caa755C0B91930AC6e80a726C5B5E8](https://testnet.snowtrace.io/address/0xA8A6Ac2C54caa755C0B91930AC6e80a726C5B5E8) | AgentTreasury | ERC-8004 Custodian & Meta-Tx Manager |
|  [0xFc2aF8B306fa27f5FD8739B7B1D713DAe263bAFe](https://testnet.snowtrace.io/address/0xFc2aF8B306fa27f5FD8739B7B1D713DAe263bAFe) | AgentRegistry | Agent Lifecycle Orchestrator & Beneficial Ownership |
|  [0xc5e7Ba73e9d3Fb1B80037F56d03f707Af0ebAF32](https://testnet.snowtrace.io/address/0xc5e7Ba73e9d3Fb1B80037F56d03f707Af0ebAF32) | AgentPass | NFT-based access control & membership logic |
|  [0x8004A818BFB912233c491871b3d84c89A494BD9e](https://testnet.snowtrace.io/address/0x8004A818BFB912233c491871b3d84c89A494BD9e) | ERC-8004 IdentityRegistry | Agent Identity Registry Proxy |

## Structure 

```
onchain-core/
├── 📂 contracts/             # Core Smart Contracts (Solidity)
│   ├── 📂 interfaces/        # System types and Interface definitions (IAgent...)
│   ├── 📂 mocs/              # Mock contracts for testing and proxy implementations
│   ├── AgentPass.sol         # NFT-based access control & membership logic
│   ├── AgentRegistry.sol     # Agent Lifecycle Orchestrator & Beneficial Ownership
│   └── AgentTreasury.sol     # ERC-8004 Custodian & Meta-Tx Manager
│  
├── 📂 docs/                  # Project Documentation
│   └── ARCHITECTURE.md       # Deep dive into protocol design & security
│  
├── 📂 ignition/modules/      # Hardhat Ignition Deployment Framework
│   ├── AgentEcosystem.ts     # Orchestrates the multi-contract deployment sequence
│   └── TestToken.ts          # Deploys ERC20 mocks for staging environments
│          
├── 📂 scripts/               # Maintenance & Interaction Tools
│    └── interaction.ts       # Post-deployment lifecycle simulation script
│
└── 📂 test/                  # Comprehensive Test Suite (TypeScript)
    ├── AgentPass.test.ts     # Unit: Tests subscription tiers, fee burning, and access gating logic
    ├── AgentRegistry.test.ts # Unit: Tests minting-to-treasury routing and beneficial owner tracking
    ├── AgentTreasury.test.ts # Unit: Tests EIP-712 signature recovery and custodial NFT safety
    ├── integration.test.ts   # E2E: Validates cross-contract interactions (Registry → Treasury → Pass)
    └── helpers.ts            # Test Framework: Fixtures, EIP-712 hashing, and Viem assertions
```

### Tech stack snapshot

- Solidity contracts compiled and verified with Hardhat + TypeScript.
- Viem + `node:test` helpers for the interaction script & integration scenarios.
- Ignition modules for seeded deployments (`AgentEcosystem`, `TestToken`).

## 🚀 Quick start

> To run this project locally, you need **Node.js** and **Hardhat** installed.

1. Change to the onchain-core-avax directory:
   ```bash
   cd onchain-core-avax
   ```

2. Copy the environment template and fill in the secrets.
   ```bash
   cp .env.example .env  # or `copy` on Windows
   ```

3. Install dependencies.
   ```bash
   npm install
   ```

4. (Optional) Install Hardhat globally if you haven't yet:

   ```bash
   npm install --save-dev hardhat
   ```

5. Compile the contracts.
   ```bash
   npx hardhat compile
   ```

6. Configure environment variables for deployment:

   ```env
   PRIVATE_KEY="your_private_key"
   FUJI_RPC_URL="fuji_rpc_url"
   ```

7. Run Full Lifecycle Simulation:
   Run the main script to see the ecosystem in action (deployment, agent registration, and access purchase):

   ```bash
   npx hardhat run scripts/interaction.ts --network avalancheFuji
   ```
   > *For details on what this script does, see the section* [Full Lifecycle Simulation](#full-lifecycle-simulation--e2e-workflow)

## 🧪 Tests (Optional)

To run all tests, execute this command:

```env
npx hardhat test
```

## ✨ Ignition deployments (Optional)

1. Seed the AgentEcosystem stack (registry, treasury, pass) on a chosen network.
   ```bash
   npx hardhat ignition deploy ignition/modules/AgentEcosystem.ts --network avalancheFuji --verify
   ```
2. Deploy the TestToken contract (ERC20 faucet) on a chosen network.
   ```bash
   npx hardhat ignition deploy ignition/modules/TestToken.ts --network avalancheFuji --verify
   ```
   > *Note:* This step is optional. You can use the existing contract 
   > already deployed on the Avalanche Fuji network at: `0xEf4386e91d63A97070D349C3B5Bb302b847E221f`


## 🔄 Full Lifecycle Simulation | E2E Workflow

This is the primary script for dev to verify the entire ecosystem. It automates the full journey of an AI Agent — from onchain identity creation to monetization and usage.

```bash
   npx hardhat run scripts/interaction.ts --network avalancheFuji
```

### What this script does:
- **Auto-Deployment:** If the ecosystem is not yet deployed, the script automatically deploys the `AgentRegistry`, `AgentTreasury`, and `AgentPass` contracts.
- **Agent Registration:** Mints a new Agent NFT via ERC-8004 and routes it to the Treasury.
- **Cryptographic Binding:** Signs and executes an EIP-712 typed data message to securely link your wallet to the Agent.
- **Monetization Setup:** Configures business logic including subscription fees, durations, and API request limits.
- **Access Purchase:** Simulates the user flow by approving tokens and purchasing an access pass.
- **Usage Tracking:** Simulates a relayer reporting off-chain consumption to the blockchain.


## 🏛️ Architecture & Agentic Economy

Mibboverse is powered by integration of **ERC-8004** and **x402** protocols, designed to transform AI agents into sovereign economic entities.

### Key Innovations:
* **User-Centric Identity (ERC-8004):** We utilize a **Custodial Treasury** to bind agents permanently to their creators. This ensures reputation transparency — agents cannot be sold or transferred, making their history a verifiable extension of the user.
* **Verified Access (x402):** A high-velocity monetization layer where users acquire **Soulbound AgentPasses** using agent-specific tokens (**$AGENT**), while the $MIBBO ecosystem token ensures protocol stability.
* **Hybrid Onchain/Offchain Tracking:** Our Backend Relayer securely records session usage onchain, providing a seamless user experience with cryptographic integrity.

### 🔒 Security & Grant Roadmap
As a **Security-First** project, our immediate milestones following the grant acquisition are:
1.  **Professional Audit:** Full security audit of `AgentRegistry`, `AgentTreasury`, and `AgentPass` core contracts.
2.  **$AGENT Fee Hooks:** Implementation of secure liquidity pool hooks to allow agent owners to capture value from trading activity.
3.  **Transparent Economy:** Ensuring every burn and fee-split is mathematically verifiable and resistant to manipulation.

> [!TIP]
> **Dive Deeper:** For technical diagrams, contract breakdowns, and the full lifecycle of an agent, read our [**Core Architecture Documentation**](docs/ARCHITECTURE.md).
