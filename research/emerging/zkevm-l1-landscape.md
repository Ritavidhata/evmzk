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
- **Core idea:** Committee-based inclusion lists to preserve censorship resistance
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

### Verified zkEVM Project
- **Website:** https://verified-zkevm.org/
- **Contact:** verified-zkevm@ethereum.org
- **Purpose:** Apply formal verification (Lean 4) to zkEVMs for mathematical correctness guarantees
- **Key repos:**
  - ArkLib (⭐190) — Formally Verified Arguments of Knowledge in Lean
  - clean (⭐136) — Lean Circuit DSL
  - VCV-io (⭐91) — Formalized Cryptography Proofs
  - CompPoly (⭐40) — Computable Polynomial Model
  - evm-asm (⭐20) — EVM Assembly in Lean
  - iris-lean — Separation Logic Framework
  - rust-lean — Rust code verification in Lean 4
- **Significance:** Mathematical proofs of correctness > audits > testing. Critical before any zkVM touches L1 consensus.

---

## Proving Teams (Ethproofs On-Prem Cohort)

Five teams selected by the Ethereum Foundation for the $65K on-prem proving initiative (May 2026 launch), proving 1-in-10 L1 blocks on owned hardware:

### 1. ZisK
- **zkVM:** ZisK (custom)
- **Guest:** revm
- **Hardware:** 24 GPU clusters (Sevilla Cloud, Girona On-Prem)
- **Performance:** 98.31% blocks proven <12s (leading the RTP cohort)

### 2. Succinct
- **zkVM:** SP1 Hypercube
- **Guest:** revm
- **Notable:** Optimism chose SP1 for Superchain ZK, Google Quantum AI uses SP1, Base uses it for $7.4B deposits
- **Infrastructure:** Decentralized Prover Network
- **Token:** PROVE

### 3. Brevis
- **zkVM:** Pico
- **Guest:** revm
- **Notable:** Security Sprint M1 completed

### 4. Matter Labs
- **zkVM:** Airbender
- **Guest:** zkSyncOS (NOT revm — custom execution environment)
- **Notable:** Only team NOT using revm. Vertical integration of execution + proving

### 5. Axiom
- **zkVM:** OpenVM 2.0
- **Guest:** revm
- **Hardware:** 16x NVIDIA 5090 cluster
- **Performance:** 97.53% liveness, $0.0125/proof (most cost-efficient)
- **Notable:** OpenVM formally verified in Lean by Nethermind Research. Declined on-prem grant but still actively proving.

---

## Other Evaluated Provers on Ethproofs

### Zilkworm
- **zkVM:** ZKsync Airbender
- **Guest:** zilkworm (custom)
- **Hardware:** 2x 14700K + 5090x2 (Vast.ai)
- **Performance:** Lowest cost at $0.0032/proof

### zkDTVM — Evaluated, 0% eligibility so far

---

## zkVM Deep Dive

### SP1 Hypercube (Succinct)
- **Architecture:** RISC-V (RV64IM), shard-based proving (~2²² instructions/shard)
- **Proof system:** Multilinear — LogUp GKR, Basefold PCS, Zerocheck, Jagged/Dense PCS
- **Key innovations:** VEIL (hash-based ZK with 3% overhead → post-quantum path), decentralized prover network
- **Production use:** Optimism, Base, Google Quantum AI
- **GitHub:** https://github.com/succinctlabs/sp1

### Pico (Brevis)
- **Architecture:** Glue-and-Coprocessor model, modular chips
- **Proof system:** STARK on Plonky3, interchangeable backends (KoalaBear, BabyBear, Mersenne31)
- **Key innovations:** Flexible proving backends, app-specific coprocessors
- **GitHub:** https://github.com/brevis-network/pico

### Airbender (Matter Labs)
- **Architecture:** Custom VM (zkSyncOS), NOT EVM-equivalent
- **Key innovation:** Purpose-built for zkSync, native account abstraction, ZK-optimized ISA
- **Unique:** Only zkVM in the race proving a non-EVM execution environment
- **Cheapest prover:** $0.0032/proof

### OpenVM (Axiom)
- **Architecture:** No-CPU modular chips, extensible ISA
- **Proof system:** STARK on Plonky3
- **Key innovations:** Formally verified in Lean (Nethermind), native field arithmetic via Rust intrinsics
- **Production:** v1.5.0 (Feb 2026)
- **GitHub:** https://github.com/openvm-org/openvm

### Comparison Matrix

| Feature | SP1 Hypercube | Pico | Airbender | OpenVM |
|---------|---------------|------|-----------|--------|
| Builder | Succinct | Brevis | Matter Labs | Axiom |
| Architecture | RISC-V + sharding | Glue + Coprocessor | Custom (zkSyncOS) | No-CPU chips |
| Guest on Ethproofs | revm | revm | zilkworm/zkSyncOS | revm |
| EVM compatible | ✅ | ✅ | ❌ | ✅ |
| Formally verified | Partial | No | No | ✅ (Lean) |
| Production chains | Optimism, Base | Brevis ecosystem | zkSync Era | Axiom ecosystem |

---

## Infrastructure & Coordination

### Ethproofs (Ethereum Foundation)
- **Website:** https://ethproofs.org
- **Run by:** Fara Woolf, Will Corcoran (EF)
- **Purpose:** Track real-time proving performance, coordinate RTP grants ($300K total, 3 × $100K still unclaimed)
- **Updated RTP criteria (March 2026):** P99 ≤10s, ≥128-bit security, ≤$100K CAPEX, ≤10kW, ≤300KiB proof size
- **Phase:** Phase 2 of 5 (Security Sprint)
- **Next milestone:** Optional proving at Hegotá hard fork (late 2026)

### EF Cryptography Team
- Running zkVM Security Sprint (soundcalc integration)
- Evaluating zkVM implementations for L1 integration

### EF zkEVM Team
- Separate process for selecting zkVM implementations for L1
- Published Real-Time Proving guidelines (July 2025)
- Published zkEVM Security Foundations (December 2025)

---

## Performance Snapshot (May 2026)

- 301,452 total proofs across all evaluated provers
- 67% proved in ≤10s (target: ≥70%)
- ZisK leads at 98.31% <12s (still under 99% RTP threshold)
- Nobody has claimed the $100K grants yet

---

## Open Incentive Problems (from zkevm.fyi)

1. **Fallback Provers (HIGH PRIORITY):** 1-of-N trust assumption — always need ≥1 prover available. Multiplexing problem unsolved.
2. **Prover Killers (HIGH PRIORITY):** Blocks too hard to prove → liveness failure. "You build it, you prove it" proposal.
3. **Prover Markets (LOWER PRIORITY):** Open competitive market vs. fallback-only.
4. **Censorship Resistance:** FOCIL handles txs, blob censorship still unsolved.
5. **Offchain vs Onchain Proofs:** Design question still open.
6. **Network Throughput:** State growth limits with higher throughput unclear.

---

## Key Dependencies

```
EIP-7886 (Delayed Execution)
    └── Enables zkEVM proof verification one slot later
        └── Requires real-time provers (Ethproofs cohort)
        └── Requires VOPS (validator state reduction)
        └── Requires FOCIL (censorship resistance)
        └── Requires peerDAS (blob-native data distribution)
        └── Requires formal verification (Verified zkEVM)
        └── Requires incentive solutions (prover killers, fallback provers)
        └── Target: Hegotá hard fork (late 2026)
```

---

*This landscape is actively evolving. Ethproofs data refreshes continuously.*
