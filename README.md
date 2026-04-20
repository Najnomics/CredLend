# CredLend

> Borrow more than you deposit — because on-chain identity changes everything.

---

## Overview

CredLend is a Sybil-resistant, identity-gated lending protocol on Conflux eSpace that unlocks undercollateralized credit lines for verified human users. By integrating World ID's Semaphore zero-knowledge proofs directly into the loan origination logic, CredLend creates a two-tier borrowing market: verified humans access credit lines up to 150% of their collateral value (undercollateralized), while unverified addresses are limited to standard overcollateralized borrowing (70% LTV). Real-time Pyth Network price feeds power all LTV calculations and liquidation triggers. dForce Unitus lending markets serve as the underlying capital deployment layer. All user interactions are gasless via Conflux's native Fee Sponsorship mechanism.

CredLend is the first human-gated, identity-aware lending market on Conflux eSpace — and the first DeFi protocol on Conflux to use World ID Semaphore proofs as a credit enhancement primitive rather than purely for access control.

> **Testnet note:** World ID's Semaphore verifier deployment on Conflux eSpace testnet is pending confirmation. The testnet demo uses a `MockSemaphoreVerifier.sol` that replicates the exact Semaphore interface, allowing full end-to-end testing. Mainnet deployment will swap in the live World ID verifier at the same address slot. This is standard practice for novel cross-protocol integrations on new chains.

---

## Hackathon

**Global Hackfest 2026** | 2026-03-23 – 2026-04-20

**Prize Target:** Main Award ($1,500)

---

## Team

- **Nosakhare Jesuorobo** — Lead Smart Contract Developer (GitHub: [@najnomics](https://github.com/najnomics), Discord: najnomics)
- Team Member 2 (GitHub: @username, Discord: username)
- Team Member 3 (GitHub: @username, Discord: username)
- Team Member 4 (GitHub: @username, Discord: username)
- Team Member 5 (GitHub: @username, Discord: username)

---

## Problem Statement

Every major DeFi lending protocol — dForce Unitus, Aave, Compound — requires borrowers to deposit more collateral than they borrow. A user who wants to borrow $1,000 USDT must first deposit $1,500 in crypto. This overcollateralization exists for one reason: protocols cannot distinguish between a single legitimate human user and a thousand Sybil wallets controlled by the same entity. In the absence of identity, the only safe assumption is that every wallet is anonymous and adversarial.

This creates three structural failures that CredLend solves:

**1. Real humans are excluded from productive credit.** Workers receiving AxCNH payroll, small business owners, and contractors who need short-term credit cannot access it without already holding more capital than they want to borrow. The people most likely to repay (verifiable humans with real economic stakes) are blocked by the same system that serves anonymous capital.

**2. Capital efficiency is systemically low.** For every $1,000 borrowed on any overcollateralized protocol today, $1,500+ in collateral sits idle. Across the Conflux eSpace DeFi ecosystem, this inefficiency limits the utility of dForce, AxCNH, and USDT0 for real-world financial use cases.

**3. Sybil attacks drain protocol incentives.** Liquidity mining programs and loyalty rewards are systematically exploited by operators running hundreds of wallets. Without identity, there is no mechanism to limit per-human participation.

On Conflux eSpace specifically, the problem is compounded by the ecosystem's ambition: Conflux's AxCNH PayFi stack targets real-world Asia corridor payments — but real-world borrowers cannot use a credit system that ignores their identity.

---

## Solution

CredLend implements a three-tier identity-gated lending system built on two key integrations: World ID Semaphore proofs for identity verification and dForce Unitus for the underlying capital market.

**Tier 0 — Unverified (standard DeFi).**
Any wallet without a World ID proof borrows at standard overcollateralized rates: 70% LTV, base interest rate + risk premium. Identical to existing dForce lending behavior — no regression for users who choose not to verify.

**Tier 1 — World ID Orb Verified.**
Users who complete World ID's Orb verification submit a ZK Semaphore proof to `WorldIDVerifier.sol`. The contract validates the proof against the World ID Semaphore verifier (or `MockSemaphoreVerifier` on testnet) without storing or revealing any biometric data. Verified users unlock: 120% LTV, reduced interest rate (base rate), and a maximum borrow limit of 2,000 USDT0 or AxCNH equivalent.

**Tier 2 — World ID Orb Verified + On-Chain Credit History.**
Users who have repaid at least 3 previous CredLend loans on time unlock the highest tier: 150% LTV, preferential interest rate (base rate minus discount), and a maximum borrow limit of 5,000 USDT0 or AxCNH equivalent. Credit history is tracked against the World ID nullifier — not the wallet address — so users cannot reset their history by switching wallets.

**dForce Unitus as the lending backend.**
`CredLendPool.sol` deposits all collateral and idle capital into dForce Unitus lending markets via `dForceAdapter.sol`. Protocol reserves earn lending yield while not actively borrowed, creating a compound interest effect that funds the gas sponsorship system over time. The dForce integration is ported directly from ConfluxMind's `dForceAdapter.sol`.

**Pyth Network for LTV calculations (Bounty 03 pattern).**
All collateral-to-loan ratios and liquidation triggers use real-time Pyth price feeds for USDT0/USD, AxCNH/CNH, and CFX/USD. The LTV calculation calls `IPyth.getPriceNoOlderThan()` on every borrow and liquidation check, ensuring prices reflect live market conditions rather than stale on-chain state.

---

## Go-to-Market Plan

### Target Users

**Primary — AxCNH payment corridor users:**
Workers receiving AxCNH payroll via FlowCNH or other Conflux-native payment protocols who need short-term credit between payment cycles. These are verified humans with predictable income patterns — the ideal undercollateralized borrower profile. Initial target: 200 Orb-verified World ID users active in the Conflux eSpace ecosystem within 60 days of mainnet.

**Secondary — DeFi capital efficiency seekers:**
Existing Conflux eSpace DeFi users who are World ID verified and want better LTV ratios than dForce Unitus offers on its own. CredLend's 150% LTV for verified users with repayment history represents a direct capital efficiency improvement of 2x over standard overcollateralized lending.

**Tertiary — Conflux ecosystem participants:**
Any World ID verified user participating in the Conflux ecosystem — hackathon participants, grant recipients, DAO contributors — who needs short-term liquidity against their AxCNH or USDT0 holdings.

### Distribution

**Phase 1 — Hackathon (now → April 20):**
Deploy on Conflux eSpace testnet with `MockSemaphoreVerifier.sol`, testnet AxCNH, USDT0, and Pyth testnet price feeds. Demonstrate all three borrowing tiers. Submit to Global Hackfest 2026.

**Phase 2 — Mainnet Launch (Month 1–2):**
- Confirm World ID Semaphore verifier deployment on Conflux eSpace mainnet
- Apply for Conflux Ecosystem Grants and World ID ecosystem grants
- Partnership outreach to FlowCNH for user cross-referral

**Phase 3 — Scale (Month 3–6):**
- Expand credit line limits as on-chain repayment history accumulates
- Cross-chain World ID proof relay via LayerZero
- Target $500K in total loans originated within 90 days of mainnet

### Key Metrics

| Metric | 30-Day Target | 90-Day Target |
|--------|--------------|---------------|
| Total loans originated (USD) | $50K | $500K |
| Unique verified borrowers | 100 | 1,000 |
| Average LTV (verified users) | 115% | 130% |
| Liquidation rate | < 5% | < 3% |
| dForce yield earned on reserves | $500 | $8,000 |
| Gas sponsored (USD equiv.) | $300 | $3,000 |

---

## Conflux Integration

- [ ] Core Space
- [x] **eSpace** — All contracts deployed on Conflux eSpace. Low gas costs make ZK proof verification on-chain economically viable for every borrow transaction (Groth16 Semaphore verification ~300K gas; negligible at Conflux gas prices).
- [ ] Cross-Space Bridge
- [x] **Gas Sponsorship** — `SponsorWhitelistControl` built-in (`0x0888000000000000000000000000000000000001`) sponsors all user transactions: `verifyWorldID()`, `deposit()`, `borrow()`, `repay()`, `withdraw()`. Users verify identity and access credit with zero CFX required. Managed by `CredLendSponsor.sol`, self-funded from dForce reserve yield.
- [x] **Built-in Contracts** — Uses `SponsorWhitelistControl` at `0x0888000000000000000000000000000000000001` for Fee Sponsorship management.
- [x] **Partner Integrations:**
  - **World ID (Worldcoin)** — Core identity layer. `WorldIDVerifier.sol` validates Semaphore ZK proofs; nullifier-based credit scoring persists across wallet changes. Testnet uses `MockSemaphoreVerifier.sol` with identical interface.
  - **Pyth Network** — Real-time price feeds for USDT0/USD, AxCNH/CNH, and CFX/USD in all LTV calculations and liquidation triggers. Direct implementation of the Bounty 03 (Pyth feeds) pattern. `IPyth.getPriceNoOlderThan()` called on every borrow and liquidation check.
  - **dForce Unitus** — Underlying lending market. `dForceAdapter.sol` (ported from ConfluxMind) supplies all idle collateral and protocol reserves to dForce Unitus; earns lending yield on every dollar not actively borrowed.

---

## Features

- **Three-tier identity-gated lending** — Unverified (70% LTV) · World ID verified (120% LTV) · Verified + repayment history (150% LTV); each tier with distinct rates and borrow limits
- **World ID Semaphore ZK verification** — On-chain proof validation via `WorldIDVerifier.sol`; no biometric data stored or transmitted; testnet-compatible via `MockSemaphoreVerifier.sol` (identical interface)
- **Nullifier-based credit history** — Repayment record tied to World ID nullifier, not wallet address; credit history persists across wallet changes
- **Pyth oracle LTV engine (Bounty 03)** — Real-time Pyth price feeds power all collateral valuations and liquidation triggers; `getPriceNoOlderThan()` on every state-changing action
- **dForce Unitus backend** — All collateral and reserves auto-supplied to dForce Unitus lending markets via `dForceAdapter.sol` ported from ConfluxMind; protocol earns yield on idle capital
- **Gasless all interactions** — Fee Sponsorship via `SponsorWhitelistControl` covers every user transaction; zero CFX required
- **AxCNH and USDT0 native** — Both borrow and collateral assets are Conflux's flagship stablecoins; no wrapped assets
- **Sybil-resistant liquidations** — Liquidators must hold a different World ID nullifier from the position owner; prevents self-liquidation exploit farming
- **Self-funding gas sponsorship** — dForce yield earned on reserves flows to `CredLendSponsor.sol`; the gasless UX is funded by the protocol's own capital efficiency, not a subsidy

---

## Technology Stack

- **Frontend:** React 18, Next.js 14, Wagmi v2, Viem, TailwindCSS, `@worldcoin/idkit` (World ID widget), `@pythnetwork/pyth-evm-js`
- **Backend:** Node.js 20, TypeScript — liquidation keeper using `@cfxdevkit/core` for RPC, Pyth feeds for price data
- **Blockchain:** Conflux eSpace (Chain ID: 1030 mainnet / 71 testnet)
- **Smart Contracts:** Solidity ^0.8.24, Foundry (forge, cast, anvil)
- **SDK Tooling:** `@cfxdevkit/core` (RPC clients, contract interactions), `@cfxdevkit/services` (Pyth feed helpers, Swappi DEX patterns), `@cfxdevkit/wallet` (session keys, transaction batching)
- **Identity:** World ID Semaphore — `@worldcoin/idkit` (frontend), `MockSemaphoreVerifier.sol` (testnet), `IWorldID.sol` interface (mainnet target)
- **Oracle:** Pyth Network — `IPyth.sol` on-chain, `@pythnetwork/pyth-evm-js` keeper-side
- **Protocol Integrations:** dForce Unitus (via `dForceAdapter.sol` from ConfluxMind), AxCNH ERC-20, USDT0 LayerZero OFT
- **Conflux-Specific:** `SponsorWhitelistControl` built-in, cfxdevkit template base, Conflux eSpace RPC
- **Testing:** Forge test suite, Conflux eSpace mainnet fork, mock World ID verifier, LTV fuzz tests, integration tests
- **DevOps:** GitHub Actions CI, Tenderly contract monitoring

---

## Setup Instructions

### Prerequisites

- Node.js v20+
- Foundry (`curl -L https://foundry.paradigm.xyz | bash && foundryup`)
- Git
- Conflux wallet — Fluent Wallet or MetaMask on Conflux eSpace testnet
- Testnet CFX from [Conflux eSpace faucet](https://efaucet.confluxnetwork.org/)
- Testnet USDT0 and AxCNH

### Installation

1. Clone the repository

   ```bash
   git clone https://github.com/najnomics/cred-lend
   cd cred-lend
   ```

2. Install Foundry dependencies

   ```bash
   forge install
   ```

3. Install frontend and cfxdevkit dependencies

   ```bash
   npm install
   ```

4. Configure environment

   ```bash
   cp .env.example .env
   ```

   Edit `.env`:

   ```env
   # Conflux eSpace
   CONFLUX_ESPACE_RPC=https://evm.confluxrpc.com
   CONFLUX_ESPACE_TESTNET_RPC=https://evmtestnet.confluxrpc.com
   CHAIN_ID=1030

   PRIVATE_KEY=your_private_key_here

   # Assets
   USDT0_ADDRESS=0x...
   AXCNH_ADDRESS=0x...

   # dForce Unitus
   DFORCE_UNITROLLER=0x...

   # World ID — mock on testnet
   USE_MOCK_VERIFIER=true
   WORLD_ID_APP_ID=app_credlend_conflux_v1
   WORLD_ID_ACTION=credlend_verify_v1

   # Pyth (Bounty 03)
   PYTH_CONTRACT=0x...
   PYTH_USDT0_USD_FEED=0x2b89b9dc8fdf9f34709a5b106b472f0f39bb6ca9ce04b0fd7f2e971688e2e53b
   PYTH_AXCNH_CNH_FEED=0x...
   PYTH_CFX_USD_FEED=0x...

   # Conflux built-ins
   SPONSOR_WHITELIST_CONTROL=0x0888000000000000000000000000000000000001

   # Lending parameters
   TIER0_MAX_LTV_BPS=7000
   TIER1_MAX_LTV_BPS=12000
   TIER2_MAX_LTV_BPS=15000
   LIQUIDATION_THRESHOLD_BPS=8500
   LIQUIDATION_BONUS_BPS=500
   MIN_REPAYMENTS_FOR_TIER2=3
   ```

5. Compile and deploy mock verifier (testnet)

   ```bash
   forge build
   forge script script/DeployMock.s.sol \
     --rpc-url $CONFLUX_ESPACE_TESTNET_RPC \
     --broadcast
   ```

6. Run the application

   ```bash
   npm run dev        # frontend
   npm run keeper     # liquidation keeper
   ```

### Testing

```bash
forge test -vvv
forge test --fork-url https://evm.confluxrpc.com -vvv
forge test --match-path test/fuzz/LTVFuzz.t.sol -vvv --fuzz-runs 10000
forge test --match-path test/integration/BorrowingTiers.t.sol -vvv
forge test --match-path test/Liquidation.t.sol -vvv
forge coverage --report lcov
```

---

## Usage

**Step 1 — Verify (testnet: instant mock grant)**
Open CredLend, connect wallet (no CFX needed). Click "Verify Identity". On testnet, `MockSemaphoreVerifier` instantly grants Tier 1 status — simulating World ID Orb verification for full demo flow. On mainnet: scan QR code with World ID app, complete Orb verification.

**Step 2 — Deposit and borrow**
Select USDT0 or AxCNH as collateral. Review credit line based on your tier and live Pyth prices. Enter borrow amount, confirm. One sponsored transaction — funds arrive instantly.

**Step 3 — Repay and build credit**
Repay to close position and increment on-chain credit score. After 3 on-time repayments, Tier 2 unlocks (150% LTV). Score tied to World ID nullifier — persists across wallets.

---

## Demo

- **Live Demo:** https://cred-lend.vercel.app *(testnet — mock World ID verifier)*
- **Demo Video:** [YouTube — CredLend walkthrough](https://youtube.com/watch?v=TBD)
- **Screenshots:** See `/demo/screenshots/` folder

---

## Architecture

```
User Interface
  @worldcoin/idkit widget → ZK proof (testnet: mock instant grant)
  React dashboard → borrow/repay/withdraw
  Pyth price display → live USDT0/AxCNH/CFX rates
           │ all txs sponsored (zero CFX)
           ▼
CredLendPool.sol (eSpace)
  ├── Tier Engine
  │     WorldIDVerifier / MockSemaphoreVerifier → LTV limit
  │     CreditHistory → Tier 2 upgrade if ≥3 repaid
  ├── Pyth LTV Engine (Bounty 03)
  │     IPyth.getPriceNoOlderThan() on every borrow + liquidation
  └── dForceAdapter.sol (ported from ConfluxMind)
        Idle collateral → dForce Unitus lending
        Yield → CredLendSponsor.sol → replenishes CFX sponsor balance
           │
  ┌────────┴────────┐
  │                 │
WorldIDVerifier    CreditHistory.sol
MockSemaphore      Tracks repayments per nullifier
(testnet)          Tier 2 unlock at ≥3 repaid

CredLendSponsor.sol
  SponsorWhitelistControl (0x0888...0001)
  Self-funded from dForce reserve yield

Off-Chain Keeper (Node.js + @cfxdevkit/core)
  Polls positions every 60s · Pyth prices
  Liquidates when LTV > 85%
  Sybil check: liquidator nullifier ≠ borrower nullifier

Tiers:
  Tier 0  No proof          → 70% LTV  · Standard rate
  Tier 1  World ID verified  → 120% LTV · Reduced rate · 2K limit
  Tier 2  Verified + history → 150% LTV · Best rate    · 5K limit
```

---

## Smart Contracts

### Testnet (Conflux eSpace Testnet — Chain ID: 71)

| Contract | Address |
|----------|---------|
| `CredLendPool` | `0x...` (to be deployed) |
| `MockSemaphoreVerifier` | `0x...` |
| `CreditHistory` | `0x...` |
| `PythPriceAdapter` | `0x...` |
| `dForceAdapter` | `0x...` |
| `CredLendSponsor` | `0x...` |

### Mainnet (Conflux eSpace — Chain ID: 1030)

| Contract | Address |
|----------|---------|
| `CredLendPool` | Post-hackathon, pending World ID eSpace deployment |
| `WorldIDVerifier` (live) | Pending World ID team confirmation |

*All testnet contracts verified on [ConfluxScan eSpace](https://evm.confluxscan.io)*

---

## Future Improvements

- **Live World ID Semaphore verifier** — Coordinate with World ID team for official Conflux eSpace mainnet deployment; `MockSemaphoreVerifier` swaps to live `IWorldID` at the same slot with zero contract logic changes
- **LayerZero cross-chain proof relay** — Allow World ID proofs from Ethereum to be relayed to Conflux eSpace; users who verified on other chains skip re-verification
- **FlowCNH stream collateral** — Accept FlowCNH `StreamNFT` ERC-721 positions as supplementary collateral; integrates the two protocols for compounding capital efficiency
- **Dynamic credit line growth** — Automatically increase Tier 2 limits as credit score grows beyond the 3-repayment threshold
- **Governance module** — On-chain voting for protocol parameters using a CredLend token distributed to early verified borrowers
- **Known Limitations:** World ID Semaphore verifier is mocked on testnet — mainnet requires World ID team coordination. Liquidation keeper is centralized in v1. Initial credit line caps are conservative (5,000 USDT0/AxCNH) until credit scoring has sufficient historical data.

---

## License

This project is licensed under the MIT License — see the [LICENSE](./LICENSE) file for details.

---

## Acknowledgments

- **Conflux Network** — For the Gas Sponsorship mechanism enabling zero-CFX borrower onboarding; for the eSpace infrastructure that makes ZK proof verification economically viable per transaction
- **World ID (Worldcoin)** — For the Semaphore ZK identity protocol making Sybil-resistant undercollateralized lending possible without storing user identity data on-chain
- **Pyth Network** — For the pull-based price oracle (Bounty 03 pattern) used in all LTV calculations; real-time manipulation-resistant pricing is a security requirement for any lending protocol
- **dForce** — For the Unitus lending market serving as CredLend's capital deployment backend; `dForceAdapter.sol` ported directly from ConfluxMind
- **cfxdevkit** — For `@cfxdevkit/core`, `@cfxdevkit/services`, and `@cfxdevkit/wallet` packages accelerating Conflux eSpace integration; RPC clients and contract patterns from the official toolkit
- **AnchorX** — For the AxCNH stablecoin serving as the primary borrow and collateral asset
- **Goldfinch / Maple Finance** — Conceptual inspiration for identity-gated undercollateralized lending; CredLend is an independent implementation using ZK proofs rather than off-chain KYC
- **OpenZeppelin** — ERC-20, AccessControl, ReentrancyGuard, and Pausable libraries
- **Foundry** — Solidity testing and deployment toolchain
- **Tenderly** — Contract monitoring and liquidation alerting on Conflux eSpace
