# EF's zkEVM Security Roadmap

*Compiled from EF Blog posts (Jul 2025 – Dec 2025)*

## Overview

The Ethereum Foundation has published a clear 3-phase roadmap for shipping an L1 zkEVM. The story: *performance is solved, security is the new frontier*.

---

## Phase 1: Real-Time Proving Definition (July 2025)

**Blog post:** <https://blog.ethereum.org/2025/07/10/realtime-proving>
**Author:** Sophia Gold (EF)

### The Plan
The fastest and safest path: give validators the *option* to run ZK clients that verify multiple proofs (from different zkVMs) instead of re-executing. Same defense-in-depth as existing client diversity, applied to zkVMs.

### What's Needed from Protocol
- Some form of *pipelining* in Glamsterdam to allow more proving time
- Eventually: delayed execution for one-slot-delayed proof verification

### Adoption Path
1. Initially, few validators run ZK clients
2. Security demonstrated in production over time
3. When supermajority comfortable → increase gas limit
4. Once all validators verify proofs → same proofs usable for native rollups (EXECUTE precompile)

### Real-Time Proving Definition

| Parameter | Requirement |
|-----------|-------------|
| Latency | ≤10 seconds (12s slot - ~1.5s propagation) |
| Block coverage | ≥99% of mainnet blocks |
| Security | 128-bit target, 100-bit minimum initially |
| Proof size | ≤300 KiB |
| No recursive wrappers with trusted setups | |
| CAPEX | ≤$100,000 for on-prem hardware |
| Power | ≤10 kW (residential home proving) |

### Why These Numbers?
- **$100K CAPEX:** Matches ~$80K in stake needed to run a validator. Should come down over time.
- **10 kW:** Residential homes have 10 kW circuits (EV chargers, appliances). Enables "home proving" as final censorship resistance safeguard.
- **99% coverage:** Tail end + synthetic DoS vectors mitigated in future hard forks.

---

## Phase 2: Security Foundations (December 2025)

**Blog post:** <https://blog.ethereum.org/2025/12/18/zkevm-security-foundations>
**Author:** George Kadianakis (EF Cryptography Team)

### The Problem
Many STARK-based zkEVMs rely on *unproven mathematical conjectures* for their security claims. When conjectures get *disproven* (which happened in Nov 2025 — see zkSecurity's proximity conjecture post), actual security bits drop. What was advertised as 100 bits might be 80.

### Why 128 Bits Matters
- A soundness bug in an L1 zkEVM means accepting invalid state transitions as final
- An attacker who can forge a proof can mint tokens from nothing, rewrite state, steal funds
- For something securing hundreds of billions of dollars, the margin is not negotiable
- 128 bits is recommended by NIST and validated by real-world computational milestones

### Three Security Milestones

| Milestone | Deadline | Security | Proof Size | Additional |
|-----------|----------|----------|------------|------------|
| **M1: soundcalc integration** | End Feb 2026 | — | — | All circuits in soundcalc |
| **M2: Glamsterdam** | End May 2026 | 100-bit provable | ≤600 KiB | Recursion architecture description |
| **M3: H-star** | End 2026 | 128-bit provable | ≤300 KiB | Formal security argument for recursion |

### soundcalc
- **Repo:** <https://github.com/ethereum/soundcalc>
- **What it does:** Estimates zkVM security based on latest cryptographic bounds and proof system parameters
- **Living tool:** Integrates latest research and known attacks
- **Supports:** ZisK, Pico, OpenVM, OpenVM2, Airbender, SP1
- **Proof systems:** DEEP-ALI, Jagged PCS, SWIRL + WHIR
- **Security regimes:** UDR (Unique Decoding), JBR (Johnson Bound)

### soundcalc Results (Latest)

| zkVM | Security | Expected Proof Size | Proof System | Field |
|------|----------|-------------------|--------------|-------|
| **ZisK** | 128 bits (JBR) | 269 KiB | DEEP-ALI + FRI | Goldilocks³ |
| **SP1** | 100 bits (UDR) | 529 KiB | Jagged + FRI | KoalaBear⁴ |
| **OpenVM** | 100 bits (UDR) | 7,687 KiB | DEEP-ALI + FRI | BabyBear⁴ |
| **OpenVM2** | 100 bits (UDR) | TBD | SWIRL + WHIR | BabyBear⁴ |
| **Pico** | 53 bits (JBR) | 232 KiB | DEEP-ALI + FRI | KoalaBear⁴ |
| **Airbender** | 67 bits (JBR) | 1,836 KiB | DEEP-ALI + FRI | M31⁴ |

### Key Insight
Only *ZisK* currently meets the 128-bit security target. SP1 and OpenVM hit 100 bits. Pico and Airbender have significant work ahead.

### Techniques for Hitting Targets
- Compact PCS: WHIR, JaggedPCS
- Grinding (additional computation for security)
- Well-structured recursion topology

---

## Phase 3: Hegotá Hard Fork (Late 2026)

**Blog post:** <https://blog.ethereum.org/2025/12/22/hegota-timeline>

### Timeline
- Jan 8 – Feb 4, 2026: Headliner EIP proposals
- Feb 5 – Feb 26: Headliner discussion and finalization
- 30-day window after: Non-headliner proposals
- FOCIL already in "Considered for Inclusion" section
- Target activation: Late 2026

### What's Expected in Hegotá
- **FOCIL (EIP-7805):** Committee-based inclusion lists — already considered for inclusion
- **Possibly:** Delayed execution pipelining (for ZK proof verification time)
- **NOT:** Mandatory ZK verification (that's later)

---

## The Bigger Picture: Simplifying the L1

**Source:** Vitalik's "Simplifying the L1" (May 2025)

Vitalik's vision connects the zkEVM work to a broader simplification:

1. **EVM → RISC-V (or ZK-native VM):** Replace the EVM with a simpler VM that's also the VM ZK provers use. Potential 100x+ performance improvement. EVM becomes a smart contract running *on top of* RISC-V.

2. **One shared tree:** Migrate from hexary Merkle Patricia to binary tree → much better prover efficiency.

3. **One shared erasure code:** Same code for DAS, P2P broadcasting, and distributed history.

4. **One shared serialization:** SSZ across execution, consensus, and smart contract ABIs.

This is the *endgame*: Ethereum consensus becomes simple enough that even a smart high school student can understand it, like Bitcoin.

---

## Security vs Performance: Where We Are

```
         Performance (proving speed)
              ↑
   ZisK ●     │     ● SP1
              │
              │        ● Axiom (OpenVM)
              │
   ──────────┼───────────────→ Security (soundness bits)
              │
              │  ● Pico
              │     ● Airbender
   
   Target zone: top-right (fast + secure)
   Current leader: ZisK (128-bit security, top performer)
```

---
*Updated 2026-05-05 with soundcalc data, EF blog analysis, and Vitalik's simplification roadmap*
