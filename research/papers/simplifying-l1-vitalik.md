# Vitalik's "Simplifying the L1" — The Endgame Vision

## Source
- **URL:** <https://vitalik.eth.limo/general/2025/05/03/simplel1.html>
- **Published:** May 3, 2025
- **Author:** Vitalik Buterin

## Core Argument
Ethereum should be *as simple as Bitcoin*. Protocol simplicity is upstream of resilience, just like decentralization. The current complexity is a liability.

## Why Simplicity Matters
- More people can participate in protocol research, development, governance
- Lower barrier to creating new clients, provers, developer tools
- Reduced long-term maintenance costs
- Fewer catastrophic bugs in specification and implementation
- Reduced social attack surface (fewer places for special interests)

## Three Simplification Vectors

### 1. Consensus Layer (Beam Chain / 3-Slot Finality)
- Remove separate slots/epochs, committee shuffling, sync committees
- 3-slot finality implementation: ~200 lines of code (vs Gasper's complexity)
- STARK-based signature aggregation → anyone can be an aggregator
- Simpler validator entry/exit/withdrawal mechanisms

### 2. Execution Layer (EVM → RISC-V)
The radical proposal: *replace the EVM with RISC-V* (or the VM that ZK provers use).

**Why:**
- EVM is a 256-bit VM optimized for cryptography that's becoming irrelevant
- Precompiles optimized for single use cases barely being used
- RISC-V spec is "absurdly simple" compared to EVM
- Potential **100x+ performance improvement** in ZK proving (no interpreter overhead)
- All EOF benefits (code sections, static analysis, larger code sizes) come free

**Migration Path (4 steps):**
1. New precompiles written with canonical onchain RISC-V implementation
2. RISC-V as option alongside EVM — contracts in either can interact
3. Replace all precompiles (except EC ops and KECCAK) with RISC-V
4. EVM interpreter in RISC-V goes onchain → existing EVM contracts run through it

**Result:** Ethereum consensus only "natively" understands RISC-V. EVM becomes an encapsulated complexity (like Apple's Rosetta translation layer).

### 3. Shared Protocol Components
**One erasure code** for: DAS, P2P broadcasting, distributed history storage
→ Minimizes code, enables data reuse across use cases

**One serialization format (SSZ)** for: execution layer, consensus layer, smart contract ABI
→ Easy to decode, already used in consensus, similar to existing ABI

**One tree structure** for: execution + consensus state
→ Binary tree replacing hexary Merkle Patricia → massive prover efficiency gain

## The Complexity Model

```
┌─────────────────────────────────────────┐
│  GREEN: Consensus-critical code         │  ← MINIMIZE THIS
│  (must be correct for network to work)  │
├─────────────────────────────────────────┤
│  YELLOW: Important but not consensus    │  ← Encapsulated complexity
│  (block building, mempool, analysis)    │     (OK to be complex)
├─────────────────────────────────────────┤
│  ORANGE: Historical processing          │  ← Can be dropped by new clients
│  (old EVM, deprecated features)         │     and zkEVMs
└─────────────────────────────────────────┘
```

**Key insight:** Code in the green zone is *systemic complexity* — bugs here affect the entire network. Code in yellow/orange is *encapsulated complexity* — bugs are contained. The goal is to move as much as possible from green to yellow/orange.

## Connection to zkEVM Work
- EVM → RISC-V transition *depends on* having working ZK provers (they already implement RISC-V)
- Binary tree migration improves proving performance (removes hexary Merkle overhead)
- Shared serialization simplifies proof generation (one format everywhere)
- The zkVM proving race directly enables this simplification

## Explicit Target
Follow tinygrad's lead: set an explicit **max line of code target** for Ethereum's consensus-critical specification. Make it close to as simple as Bitcoin's.

---
*Analyzed 2026-05-05*
