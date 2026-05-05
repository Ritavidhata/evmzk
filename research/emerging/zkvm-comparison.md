# zkVM Deep Dive: SP1 Hypercube, Pico, Airbender, OpenVM

*Compiled: 2026-05-05*

These are the four zkVMs competing to prove Ethereum L1 blocks in real-time. Here's how they work and what makes each different.

---

## 1. SP1 Hypercube (Succinct)

**Built by:** Succinct Labs
**GitHub:** https://github.com/succinctlabs/sp1
**Current version:** v6 (Hypercube) — previous was v5 (Turbo)
**License:** MIT + Apache 2.0
**Language:** Rust (programs compile to RISC-V RV64IM)

### What it is
SP1 is a general-purpose zkVM that proves correct execution of programs compiled for RISC-V. You write code in Rust (or any language that compiles to RISC-V), and SP1 generates a proof that the code ran correctly.

### Hypercube proof system (v6)
Hypercube is SP1's latest multilinear proof system. Key components:
- **Shard proofs:** Computation divided into shards of ~2²² RISC-V instructions each
- **LogUp GKR:** Lookup argument using the Grand Product Argument framework
- **Zerocheck:** Multilinear polynomial zero-testing protocol
- **Jagged PCS + Dense PCS:** Polynomial commitment schemes for different data shapes
- **Basefold:** Collision-resistant hash-based polynomial commitment
- **Global Memory Argument:** Proves memory consistency across shards (if shard A wrote value v to location i, shard B reads v from i)
- **Recursion:** Shard proofs aggregated and compressed into a single succinct proof

### Key differentiators
- **Most production-deployed zkVM:** Used by Optimism (Superchain ZK), Base ($7.4B in deposits), Google Quantum AI (elliptic curve research)
- **Prover Network:** Decentralized marketplace for proof generation (explorer.succinct.xyz)
- **VEIL integration:** Adding zero-knowledge to hash-based proofs with only ~3% prover overhead — path to post-quantum security (replacing Groth16 wrapper)
- **Precompiles:** Hardware-accelerated operations for common crypto (keccak, secp256k1, etc.)
- **PROVE token:** Ecosystem token for the prover network

### On Ethproofs
- Guest: revm (Rust Ethereum VM)
- Security Sprint M1: ✅
- Trusted by multiple production chains

---

## 2. Pico (Brevis)

**Built by:** Brevis Network
**GitHub:** https://github.com/brevis-network/pico
**Current version:** v1.3.0 (Feb 2026)
**License:** MIT + Apache 2.0
**Language:** Rust (RISC-V toolchain based on RISC Zero's work)

### What it is
Pico is an open-source zkVM built on the "Glue-and-Coprocessor" architecture (from Vitalik's vision). It combines the flexibility of a general-purpose zkVM with the efficiency of specialized circuits.

### Architecture
- **Glue-and-Coprocessor model:** Core zkVM handles general computation, while app-specific coprocessors handle domain-specific operations at high speed
- **Interchangeable proving backends:** Can switch between multiple backends:
  - STARK on KoalaBear (p = 2³¹ - 2²⁴ + 1)
  - STARK on BabyBear (p = 2³¹ - 2²⁷ + 1)
  - CircleSTARK on Mersenne31 (p = 2³¹ - 1)
- **Proverchain:** Full pipeline — RISCV → CONVERT (RECURSION) → COMBINE (RECURSION) → COMPRESS (RECURSION) → ONCHAIN (EVM)

### Key differentiators
- **Modular proving:** Pick the field and backend that best fits your use case
- **App-specific circuits:** Seamlessly integrate specialized coprocessors alongside the general zkVM
- **Built on Plonky3:** Proving backend extends Plonky3's modular design
- **Inspired by SP1, Valida, RISC Zero:** Chip design from SP1, cross-table lookups from Valida, Rust toolchain from RISC Zero
- **Audited by Sherlock:** Recommended for production use

### On Ethproofs
- Guest: revm
- Security Sprint M1: ✅
- Part of on-prem proving cohort

---

## 3. Airbender (Matter Labs)

**Built by:** Matter Labs (zkSync team)
**Guest execution:** zkSyncOS (NOT revm — custom execution environment)
**Status:** Proving on Ethproofs

### What it is
Airbender is Matter Labs' zkVM built specifically for proving zkSync OS execution. Unlike the other three zkVMs that use revm as their guest (running standard EVM), Airbender proves a *custom execution environment*.

### Why this matters
- **Not EVM-equivalent at the guest level:** zkSyncOS has its own VM with different opcodes, fee mechanics, and account abstraction built in
- **Type 2/3 zkEVM:** Not trying to be byte-for-byte compatible with Ethereum execution; optimized for ZK-friendliness instead
- **Native account abstraction:** zkSyncOS has AA as a first-class feature
- **Vertical integration:** Matter Labs controls both the execution layer (zkSync) and the proving layer (Airbender)

### Key differentiators
- **Only team NOT using revm:** All other Ethproofs provers run revm inside their zkVM
- **zkSync-native:** Purpose-built for the zkSync ecosystem
- **Custom ISA:** Optimized for ZK proving efficiency rather than EVM compatibility
- **Used in production:** Already proving blocks for zkSync Era (one of the largest ZK rollups)

### On Ethproofs
- Guest: zilkworm (custom)
- Both "ZKsync Airbender" and "Zilkworm Airbender ZKsync" configurations active
- Lowest cost per proof: $0.0032 (2-GPU setup on Vast.ai)
- Security Sprint M1: ✅

---

## 4. OpenVM (Axiom)

**Built by:** Axiom
**GitHub:** https://github.com/openvm-org/openvm
**Current version:** v1.5.0 (Feb 2026, production-ready)
**License:** MIT + Apache 2.0
**Language:** Rust

### What it is
OpenVM is a performant and modular zkVM framework with a unique "no-CPU" architecture. Instead of a traditional processor that dispatches instructions, OpenVM uses independent "chips" that each handle specific instruction types.

### Architecture — No-CPU Design
- **No central processor:** Unlike traditional VMs, there's no CPU dispatching instructions
- **Modular chips:** Each "chip" handles a specific instruction set (e.g., RV32IM for RISC-V, native field arithmetic)
- **Extensible ISA:** New custom instructions integrate directly into the VM without forking the core
- **Extensions currently available:**
  - RV32IM (RISC-V integer + multiplication)
  - Native field arithmetic extension
  - Custom chip extensions for domain-specific acceleration

### Key differentiators
- **Formally verified:** RV32IM extension formally verified in Lean by Nethermind Research
- **No-CPU architecture:** More modular than traditional zkVM designs — add chips without touching core
- **Native field arithmetic:** Direct access to field operations through Rust intrinsics (no emulated bignum)
- **On-chain verification:** Built-in support for unbounded program proving with EVM verification
- **Built on Plonky3:** STARK backend uses Plonky3's modular polynomial IOP
- **Audited:** External audits on Cantina + internal audit by Axiom team members

### On Ethproofs
- Guest: revm
- Hardware: 16x NVIDIA 5090 cluster ("Axiom 16x5090")
- Performance: 97.53% liveness, $0.0125/proof (most cost-efficient among top performers)
- 83% of eligible proof share on Ethproofs
- Security Sprint M1: ✅
- Declined on-prem grant but still actively proving

---

## Comparison Matrix

| Feature | SP1 Hypercube | Pico | Airbender | OpenVM |
|---------|---------------|------|-----------|--------|
| **Builder** | Succinct | Brevis | Matter Labs | Axiom |
| **Architecture** | RISC-V + shard proving | Glue + Coprocessor | Custom (zkSyncOS) | No-CPU modular chips |
| **Guest on Ethproofs** | revm | revm | zilkworm/zkSyncOS | revm |
| **Proof system** | Multilinear (Basefold, LogUp GKR) | STARK (Plonky3-based) | Custom STARK | STARK (Plonky3-based) |
| **Key innovation** | Hypercube sharding + VEIL | Interchangeable backends | Non-EVM native execution | No-CPU chip architecture |
| **EVM compatible** | ✅ (revm guest) | ✅ (revm guest) | ❌ (zkSyncOS) | ✅ (revm guest) |
| **Post-quantum path** | VEIL (hash-based ZK) | Hash-based STARKs | Hash-based | Hash-based STARKs |
| **Formally verified** | Partial | No | No | ✅ (Lean/Nethermind) |
| **Production chains** | Optimism, Base, others | Brevis ecosystem | zkSync Era | Axiom ecosystem |

---

## What This Means for zkEVM-Native Ethereum

For the delayed execution + same-slot proving pipeline:
- All four can generate proofs for Ethereum blocks
- Three use revm as guest → proving standard EVM execution
- Airbender proves a different execution environment → interesting for non-EVM paths
- SP1 has the most production traction and ecosystem
- OpenVM has the strongest formal verification story
- Pico has the most flexible backend architecture
- The race is far from settled — no one has hit the 99% <12s RTP grant threshold yet

---
*Updated as proving performance evolves on ethproofs.org*
