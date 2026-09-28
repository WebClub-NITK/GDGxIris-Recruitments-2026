## Task ID: ChainEscrow

`Mentor: Ashlesh Prabhu (+91 7676130360)`

#### `Solidity/Web3`

## Difficulty: `Medium+`

## Overview

Your task is to build **ChainEscrow**, a decentralized milestone-based escrow dApp for freelance/gig payments. It doesn't have to look polished — focus on getting the fund-custody logic and state transitions airtight, since that's the actual point of the task.

The idea: a **client** hires a **freelancer** for a job and locks payment into a smart contract upfront. The job is split into **milestones**. The freelancer gets paid milestone-by-milestone only after the client approves each one. If client and freelancer disagree, a neutral **arbiter** address can step in and resolve it. Nobody — not even the client — should be able to pull funds out of the contract except through these defined paths.

This task is meant to introduce you to the fundamentals of dApp development: **smart contract state machines, access control, fund custody & withdrawal safety, wallet integration, and syncing frontend UI to on-chain state.**

You can build on any chain you like (Ethereum testnet, Polygon Amoy, Solana devnet, etc.) — EVM chains with Solidity are recommended since the tooling/resources are richest.

## Required Features

You're welcome to add more and customize however you want, but the following is the recommended priority order.

### 1. Roles & Job Setup

- Anyone can call `createJob()` as a **client**, specifying the freelancer's wallet address and depositing the total payment (ETH or an ERC-20 token) into the contract at creation time.
- Job must be split into **2 or more milestones**, each with its own payout amount (amounts must sum to the total deposit — validate this on-chain, don't trust the frontend).
- Each job gets a unique ID; store client, freelancer, arbiter, milestone list, and job status on-chain.

`Bonus: Support ERC-20 token payments in addition to native ETH, allow the client to choose the arbiter per-job instead of a fixed global arbiter.`

### 2. Milestone State Machine

Each milestone must move through a strict set of states — don't let the frontend "fake" a transition that isn't valid on-chain:

- `Pending` → freelancer hasn't submitted work yet.
- `Submitted` → freelancer calls `submitMilestone()`, marking it ready for review.
- `Approved` → client calls `approveMilestone()`; this immediately transfers that milestone's funds to the freelancer.
- `Disputed` → either client or freelancer calls `raiseDispute()` on a submitted milestone instead of approving it.
- `Resolved` → arbiter calls `resolveDispute()`, deciding how the milestone's locked funds are split between client and freelancer.

Enforce all transitions with `require()` checks (e.g. you cannot approve a `Pending` milestone, you cannot resolve a dispute that wasn't raised, only the client can approve, only the freelancer can submit).

`Bonus: Add a refund path — if the freelancer never submits a milestone before a deadline, allow the client to reclaim those funds.`

### 3. Fund Safety (this is what will actually be graded closely)

- Use the **checks-effects-interactions** pattern or `nonReentrant` guards on every function that moves funds.
- No milestone's funds should ever be withdrawable twice.
- No address should be able to drain funds belonging to a job it's not part of.
- Write and include **unit tests** that specifically try to break this: double-approve, double-resolve, approve-before-submit, non-client trying to approve, etc.

`Bonus: Formal test coverage report, or a short writeup of the attack vectors you tested against.`

### 4. Dispute Resolution

- Arbiter address (set at job creation or globally, your choice) can call `resolveDispute(jobId, milestoneId, clientShare)` to split a disputed milestone's funds between client and freelancer in any ratio (including 100/0).
- Only the designated arbiter can call this — enforce with access control (`onlyArbiter` modifier or similar).
- Emit an event on resolution so the frontend can reflect the outcome.

`Bonus: Multi-arbiter voting instead of a single trusted arbiter.`

### 5. Smart Contract Deployment & Verification

- Deploy to a public testnet (Sepolia, Amoy, Solana devnet, etc.).
- **Verify the contract source** on the relevant block explorer (Etherscan, Polygonscan, Solscan) so anyone can read the deployed code.
- Contract should be well-commented — explain *why* a check exists, not just what it does.

### 6. Frontend Application

- Wallet connect (MetaMask / WalletConnect for EVM, Phantom for Solana).
- The UI should adapt based on which role the connected wallet has for a given job — a client sees "Approve"/"Raise Dispute" buttons, a freelancer sees "Submit Milestone," an arbiter sees a dispute-resolution panel. Don't show actions a connected wallet isn't allowed to take.
- **Create Job** flow: pick freelancer address, set milestone amounts, deposit funds.
- **Job Detail** view: shows all milestones, their current state, and the relevant action button for the connected wallet.
- **Dispute Panel** (arbiter-only): shows disputed milestones across jobs and a form to resolve them.
- All state shown in the UI should be **read live from the chain**, not cached/guessed client-side — after any action, the UI should refetch and reflect the true on-chain state.

`Bonus: Transaction status toasts (pending/confirmed/failed), a job history/activity log pulled from contract events instead of just current state.`

### 7. UI / UX

- Clear indication of milestone status (pending/submitted/approved/disputed/resolved) — a simple status badge is enough, doesn't need to be fancy.
- Basic error handling: wrong network, insufficient funds, wallet not connected, transaction rejected.

## Deliverables

We will need the following to be present in your repository:

### 1. Source Code

Complete project — smart contract(s) with a proper testing setup (Hardhat/Foundry for EVM, Anchor for Solana), and the frontend app. Include a `.gitignore` appropriate for your stack (don't commit `node_modules`, private keys, `.env` files, etc.).

### 2. Tests

Unit tests for the contract covering both the happy path (full job lifecycle) and the failure cases listed under Fund Safety above.

### 3. Documentation

Your `README.md` must include:

- A project overview and explanation of the milestone/dispute flow.
- Setup instructions — how to install, run tests, deploy, and run the frontend locally.
- The deployed contract address(es) and a link to the **verified** contract on the block explorer.
- Screenshots or a short screen recording of the app in use (job creation → submit → approve → a dispute being resolved).

### 4. Live Deployment

A live, deployed link to the frontend (Vercel/Netlify), connected to your deployed testnet contract.

### 5. Demo Video

A short (2-5 minute) walkthrough covering: creating a job, submitting a milestone, approving it (funds released), and raising + resolving a dispute.

## Resources

**EVM-Based (Ethereum, Polygon, Avalanche, etc.)**

- [Solidity Documentation](https://docs.soliditylang.org/)
- [Hardhat Development Environment](https://hardhat.org/)
- [Foundry](https://book.getfoundry.sh/)
- [OpenZeppelin Contracts — ReentrancyGuard, AccessControl](https://docs.openzeppelin.com/contracts/4.x/)
- [Ethers.js Library](https://docs.ethers.org/) / [wagmi](https://wagmi.sh/)
- [Solidity by Example — Escrow patterns](https://solidity-by-example.org/)
- [Consensys Smart Contract Security Best Practices](https://consensys.github.io/smart-contract-best-practices/)

**Solana**

- [Solana Docs](https://docs.solana.com/)
- [Anchor Framework](https://www.anchor-lang.com/) — PDAs are a natural fit for escrow-style state

**General**

- [ReactJS](https://react.dev/)
- [CryptoZombies](https://cryptozombies.io/) (good Solidity primer if you're new to this)
