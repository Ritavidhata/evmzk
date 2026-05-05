# Incentive Problems for zkEVM-Native Ethereum

## Source
- **URL:** https://zkevm.fyi/trees/external/incentives.html
- **From:** Ethereum zkEVM Book (eth-act)

## Overview
Robust incentive structures are required before zkEVM proofs can become L1 consensus infrastructure. The engineering is ahead of the economics.

---

## 1. Fallback Provers (HIGH PRIORITY — Bottleneck)

**The Problem:** 1-out-of-N trust assumption must be credible — at all times, ≥1 prover must be willing and able to prove blocks. If nobody can prove, Ethereum loses liveness.

**Three requirements for credibility:**
1. Proving costs low enough that many people could run a prover
2. Provers can be spun up quickly (local, distributed, rented GPUs)
3. **New provers can find builders** ← the hard part (multiplexing)

**Status:** Bottleneck for zkEVM adoption. In-protocol multiplexing system not yet designed.

---

## 2. Prover Killers (HIGH PRIORITY — Bottleneck)

**The Problem:** Blocks that are excessively hard to prove due to transaction volume/type. If proofs are required for validation and a block can't be proven → liveness failure.

**Two solutions being explored (not mutually exclusive):**
1. **"You build it, you prove it"** — assign proving responsibility to block builder. Builders won't create blocks they can't prove. (ETHResearch: "Prover Killers Killer")
2. **Proving gas costs** — assign proving costs to individual transactions based on how expensive they are to prove

**Status:** Solution space better understood than fallback provers, but not yet specified.

---

## 3. Prover Markets (LOWER PRIORITY)

**The Question:** Should Ethereum have an open competitive market for proofs in normal operation (not just fallback)?

**Sub-questions:**
- How important is an active prover market vs. just fallback provers?
- What would an in-protocol prover market look like?

**Status:** Not a bottleneck. Ethereum can maintain liveness with fallback provers alone. But competitive markets could lower costs and reduce barriers.

---

## 4. Censorship Resistance

**The Problem:** zkEVMs leverage prover/validator asymmetry. More computation for building/proving means not every validator can build locally. Censorship resistance can't come from local block building.

**Solutions in progress:**
- **FOCIL (EIP-7805):** Handles regular transaction censorship via inclusion lists
- **Blob censorship resistance:** Still unsolved. Francesco D'Amato exploring blob mempool tickets. Mike and Julian have a concrete proposal for blob mempool future.

**Why it matters:** In the candidate slot architecture, ALL execution data lives in blobs via peerDAS. Blob censorship = data censorship.

---

## 5. Offchain vs Onchain Proofs

**The Question:** Should proofs live on-chain or off-chain?

- Off-chain: cheaper storage, more verifier flexibility
- On-chain: more transparent, easier to audit
- Vitalik introduced the concept. Justin Drake explored "native rollups" with L1 execution.

**Status:** Open design question.

---

## 6. Network Throughput

**The Question:** What are the actual throughput limits when proving is the bottleneck instead of re-execution?

**Considerations:**
- Some parties still need to execute blocks (to get current state)
- Compute and disk access limits for block execution
- State growth limits with potentially much higher throughput
- What should the new gas limits be?

**Status:** Unclear. Needs more research.

---

## Priority Summary

| Problem | Priority | Status |
|---------|----------|--------|
| Fallback Provers | 🔴 Bottleneck | Multiplexing unsolved |
| Prover Killers | 🔴 Bottleneck | Solution space understood |
| Censorship Resistance | 🟡 Important | FOCIL for txs, blob CR open |
| Prover Markets | 🟢 Lower | Not blocking adoption |
| Offchain/Onchain Proofs | 🟢 Design | Open question |
| Network Throughput | 🟡 Important | Needs research |

---
*Analyzed 2026-05-05*
