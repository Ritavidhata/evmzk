# zkEVM-Native Ethereum L1: Teams, Projects & Landscape

*Compiled: 2026-05-05*

## Overview

The zkEVM-native Ethereum vision combines several proposals (EIP-7886, FOCIL, VOPS, peerDAS) with real-time zkEVM proving. Here's who is building what.

---

## Protocol Design & Research

### EIP-7886: Delayed Execution
- **Authors:** Francesco D'Amato (@fradamt), Toni Wahrstätter (@nerolation)
- **Status:** Stagnant (as of May 2026)
- **Core idea:** Separate block validation from execution, enable async attestation
- **Affiliation:** Ethereum Foundation (EL research)

### EIP-7805: FOCIL (Fork-choice enforced Inclusion Lists)
- **Authors:** Thomas Thiery, Francesco D'Amato, Julian Ma, Barnabé Monnot, Terence Tsao, Jacob Kaufmann, Jihoon Song
- **Status:** Draft
- **Core idea:** Committee-based inclusion lists to preserve censorship resistance in a builder-dominated world
- **Affiliation:** Ethereum Foundation, Offchain Labs

### VOPS (Validity-Only Partial Statelessness)
- **Published on:** ETHResearch
- **Core idea:** Validators only maintain account quadruplets (address, nonce, balance, codeFlag) — ~8.4 GiB instead of 200+ GiB
- **Significance:** Critical for making delayed execution practical for validators

### Candidate Slot Architecture (HackMD Spec)
- **Combines:** EIP-7886 + zkEVM proofs + VOPS + peerDAS + FOCIL
- **Defines:** 3-slot pipeline with concrete timing (includers, builders, proposers, provers, attesters)

### zkevm.fyi Book (eth-act/zkevm-book)
- **Organization:** eth-act (Ethereum ACT)
- **GitHub:** https://github.com/eth-act/zkevm-book
- **Purpose:** Canonical educational resource for zkEVM integration into Ethereum L1
- **Structure:** Part I (using zkEVMs), Part II (crypto primitives), Part III (audience-specific), Part IV (live status)

---

## Proving Teams (Ethproofs On-Prem Cohort)

Five teams selected by the Ethereum Foundation for the $65K on-prem proving initiative (May 2026 launch), proving 1-in-10 L1 blocks on owned hardware:

### 1. ZisK
- **zkVM:** ZisK (custom)
- **Guest:** revm
- **Hardware:** 24 GPU clusters (Sevilla Cloud, Girona On-Prem)
- **Performance:** 98.31% blocks proven <12s (leading the RTP cohort)
- **Notable:** Currently the top-performing prover on Ethproofs

### 2. Succinct
- **zkVM:** SP1 (Hypercube)
- **Guest:** revm
- **Notable:** Major ecosystem player — Optimism chose SP1 for ZK proofs on the Superchain, Google Quantum AI uses SP1 for elliptic curve research
- **Infrastructure:** Decentralized Prover Network (explorer.succinct.xyz)
- **Token:** PROVE

### 3. Brevis
- **zkVM:** Pico
- **Guest:** revm
- **Notable:** Security Sprint M1 completed

### 4. Matter Labs
- **zkVM:** Airbender (zkSync native)
- **Guest:** zkSyncOS (not revm — custom execution environment)
- **Notable:** The only team NOT using revm as guest. Building their own execution + proving stack
- **Context:** Matter Labs operates zkSync Era, one of the largest ZK rollups

### 5. Axiom
- **zkVM:** OpenVM 2.0
- **Guest:** revm
- **Hardware:** 16x NVIDIA 5090 cluster
- **Performance:** 97.53% liveness, $0.0125/proof (most cost-efficient)
- **Notable:** Declined on-prem grant participation but still actively proving. OpenVM is open-source, formally verified in Lean by Nethermind Research
- **OpenVM:** Modular no-CPU zkVM architecture, v1.5.0 production-ready (Feb 2026)

---

## Other Evaluated Provers on Ethproofs

### Zilkworm
- **zkVM:** ZKsync Airbender
- **Guest:** zilkworm (custom)
- **Hardware:** 2x 14700K + 5090x2 (Vast.ai)
- **Performance:** 78.90% liveness, lowest cost at $0.0032/proof

### zkDTVM
- Evaluated but 0% eligibility so far

---

## Infrastructure & Coordination

### Ethproofs (Ethereum Foundation)
- **Website:** https://ethproofs.org
- **Run by:** Fara Woolf, Will Corcoran (EF)
- **Purpose:** Track real-time proving performance, coordinate RTP grants ($300K total, 3 x $100K prizes still unclaimed)
- **Updated RTP criteria (March 2026):** P99 ≤10s, ≥128-bit security (soundcalc), ≤$100K CAPEX, ≤10kW power, ≤300KiB proof size
- **Phase:** Phase 2 of 5 (Security Sprint)
- **Next milestone:** Optional proving at Hegotá hard fork (late 2026)

### Ethereum Foundation Cryptography Team
- Running the zkVM Security Sprint (soundcalc integration, formal verification)
- Evaluating zkVM implementations for potential L1 integration (separate from Ethproofs)

### Ethereum Foundation zkEVM Team
- Own process for selecting zkVM implementations for L1 integration
- Published Real-Time Proving guidelines (July 2025)
- Published zkEVM Security Foundations (December 2025)

---

## Current Performance Snapshot

From Ethproofs live data:
- **301,452 total proofs generated** across all evaluated provers
- **67.0% proved in ≤10 seconds** (target: ≥70%)
- **6.19% "stunned"** (slightly over 10s)
- **3.49% "paralyzed"** (significantly over)
- **23.4% offline**
- **91.5% eligible rate** among submitted proofs
- Best performer: ZisK Cluster at 98.31% <12s (still under 99% RTP grant threshold)

---

## Key Relationships & Dependencies

```
EIP-7886 (Delayed Execution)
    └── Enables zkEVM proof verification one slot later
        └── Requires real-time provers (Ethproofs cohort)
        └── Requires VOPS (validator state reduction)
        └── Requires FOCIL (censorship resistance)
        └── Requires peerDAS (blob-native data distribution)
        └── Target: Hegotá hard fork (late 2026)
```

---

*This landscape is actively evolving. Ethproofs data refreshes continuously.*
