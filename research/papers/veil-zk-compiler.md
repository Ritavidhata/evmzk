# VEIL: Lightweight Zero-Knowledge for Hash-Based Proof Systems

## Source
- **Paper:** <https://eprint.iacr.org/2026/683>
- **Authors:** Rahul Dalal, Tamir Hemo, Eugene Rabinovich, Ron Rothblum (Succinct)
- **Published:** April 2026 (revised April 13, 2026)
- **Category:** Cryptographic Protocols

## What Problem Does It Solve?
Adding zero-knowledge to hash-based proof systems is expensive. You either:
1. Compose with an expensive ZK proof system that proves the hash operations are correct, OR
2. Make tightly coupled modifications to every component of the base protocol

Both approaches add significant complexity and performance overhead.

## What VEIL Does
A *lightweight, non-intrusive compiler* that adds ZK to any hash-based multilinear proof system without the drawbacks above.

**Key insight:** Decouple the protocol's algebraic interactions from the cryptographic hashing. Apply a ZK wrapper *only* to the algebraic components. Leave the hashing alone.

## Performance
Measured over a 31-bit base prime field, trace of 2²⁹ field elements:

| Metric | Overhead vs non-ZK |
|--------|-------------------|
| Prover time | ~3% |
| Verifier time | ~22% |
| Proof size | ~12% |

**That's remarkable.** Previous approaches to adding ZK had overheads of 2-10x. VEIL gets it down to single-digit percentages.

## Why This Matters for Ethereum

1. **Post-quantum path:** VEIL is plausibly post-quantum (hash-based). If Ethereum needs to transition away from elliptic curve assumptions, VEIL enables ZK without pairing-based systems.

2. **SP1 integration:** Succinct builds SP1 Hypercube. VEIL is their research. Expect this to be integrated into SP1's proof system, potentially making SP1 both ZK and post-quantum with minimal overhead.

3. **Privacy applications:** Until now, privacy on Ethereum (Tornado Cash successors, private transactions) required specialized ZK circuits. VEIL makes *any* hash-based proof system privacy-capable.

4. **L1 implications:** If validators verify proofs instead of re-executing, ZK properties mean validators can verify without learning transaction contents → potential privacy at the protocol level.

## Technical Approach
```
Standard hash-based proof system:
  Prover ↔ Verifier (algebraic + hash interactions)

With VEIL:
  Prover ↔ Verifier
    ├── Algebraic components → ZK wrapper (minimal overhead)
    └── Hash components → unchanged (no overhead)
```

The result is "a simple, plausibly post-quantum, protocol that achieves a minimal prover overhead of (1+o(1))" while maintaining the architectural integrity of the base proof system.

## Relationship to Other Work
- **SP1 Hypercube:** VEIL is from the Succinct team, same builders as SP1
- **Basefold PCS:** SP1 uses Basefold; VEIL is compatible with any hash-based multilinear system
- **VEIL + EIP-7886:** ZK proofs + delayed execution could enable private state transitions at L1

## Status
- Paper published April 2026
- Proof-of-concept implementation exists
- Integration into SP1 likely but not announced
