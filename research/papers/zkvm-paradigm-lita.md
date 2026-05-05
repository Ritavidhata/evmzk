# Educational Resources: zkVM Paradigm

## Source
- **URL:** https://www.lita.foundation/blog/zero-knowledge-paradigm-zkvm
- **Author:** Lita Foundation (builders of Valida zkVM)
- **Series:** 3-part educational series

## Part 1: What is zkVM? (covered here)

### ZKP Fundamentals

#### zkSNARKs
- Requires trusted or non-trusted setup
- Small proof sizes, easy to verify
- Used by: Jolt (a16z), zkSync, Scroll, Linea

#### zkSTARKs
- No trusted setup
- Transparent — publicly verifiable randomness
- Highly scalable for large witnesses
- Larger proofs, harder to verify
- Used by: Starknet, RISC Zero, SP1, Valida
- Key insight: All STARKs are SNARKs, but not all SNARKs are STARKs

### zkVM Process Flow
1. **Compiler** → machine code (governed by ISA choice)
2. **VM executes** → generates execution trace
3. **Prover maps trace** → polynomials + constraints
4. **Polynomial Commitment Scheme (PCS)** → "fingerprint" the constraints
5. **PIOP** → prover persuades verifier via polynomial queries
6. **Fiat-Shamir** → makes interaction non-interactive
7. **Evaluation/opening proof** → proves polynomial evaluations correct
8. **Verifier checks** → accepts or rejects

### zkVM vs zkEVM
- **zkVM** = general-purpose, any blockchain/application
- **zkEVM** = specialized for Ethereum, EVM-compatible
- Both use ZKPs for privacy + scalability

### Evaluation Framework (Lita's Proposal)

**Baseline (reliability):**
- **Correctness:** Soundness + Completeness + Zero Knowledge
- **Security:** Measured in bits (128-bit target for Ethproofs)
- **Trust assumptions:** No trusted setup > 1-of-N > honest majority

**Performance:**
- **Efficiency:** Core-time, energy, capital cost
- **Speed:** Latency of proof generation
- **Succinctness:** Proof size and verification cost

Lita's key argument: Most comparisons focus only on speed, but efficiency (cost) often matters more for commercial applications. Correctness and security should be baseline prerequisites.

### Full Series
- Part 2: https://www.lita.foundation/blog/a-zero-knowledge-paradigm-part-2--exploring-zk-vm-design-trade-offs
- Part 3: https://www.lita.foundation/blog/custom-instruction-set-architecture-achieving-ultimate-efficiency-in-zk-proving

---
*Analyzed 2026-05-05*
