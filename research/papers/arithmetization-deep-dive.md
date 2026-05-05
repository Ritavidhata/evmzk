# Arithmetization: The Mathematical Engine Behind zkVMs

*The most important concept most people skip over.*

## What Is Arithmetization?

Arithmetization is the process of converting a computational problem ("did this program execute correctly?") into an algebraic problem ("does this polynomial satisfy these constraints?"). It's the *bridge* between code and cryptography.

Without arithmetization, you can't build a zero-knowledge proof. Every zkVM — SP1, OpenVM, Pico, ZisK, Airbender — relies on arithmetization to function.

## The Big Picture

```
Program (Rust/C/Solidity)
    │
    ▼
Compiler → Machine Code (RISC-V, custom ISA)
    │
    ▼
VM Execution → Execution Trace (matrix of values at each step)
    │
    ▼
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    ARITHMETIZATION (this document)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    │
    ├── 1. Encode trace as polynomials over finite fields
    ├── 2. Express correctness as polynomial constraints
    ├── 3. Commit to polynomials (PCS)
    └── 4. Prove constraints hold (IOP)
    │
    ▼
Proof System (FRI, WHIR, etc.) → Cryptographic Proof
    │
    ▼
Verifier checks proof in milliseconds
```

## The Two Pillars of Arithmetization

### Pillar 1: Constraint Systems

A constraint system is a set of polynomial equations that *must* hold if the execution was correct. Think of it as a mathematical checklist.

#### R1CS (Rank-1 Constraint System)
- **Used by:** Groth16, Circom-based systems (Tornado Cash, eigen-zkvm)
- **Structure:** Each constraint is of the form: `(A · w) * (B · w) = (C · w)` where w is the witness vector
- **Strengths:**
  - Can be auto-generated from higher-level circuit descriptions (Circom, Noir)
  - Well-understood, massive ecosystem of tools
- **Weaknesses:**
  - Constraint specification often much larger than the witness
  - Requires pre-processing (trusted setup for Groth16, or "sparse matrix commitments" for Spartan)
  - Not naturally uniform — hard to exploit repetition

#### AIR (Algebraic Intermediate Representation)
- **Used by:** Starknet, RISC Zero, Winterfell, Valida, most STARK-based systems
- **Structure:** Polynomial constraints over an *execution trace* (a matrix where each row = one VM step)
- **Types:**
  - **Boundary constraints:** "The first row of this column equals X"
  - **Transition constraints:** "If row i has value A, then row i+1 has value f(A)"
  - **Consistency constraints:** "Column 1 equals column 2 squared"
- **Strengths:**
  - Uniform representation → verifier works directly with succinct format
  - No pre-processing needed
  - Naturally maps to VM execution (rows = clock cycles)
- **Weaknesses:**
  - Requires careful manual design
  - Less suitable for computations without repetitive structure

#### PLONKish (Plonk-like constraints)
- **Used by:** Plonky2, Plonky3, Halo2, Pico, OpenVM
- **Structure:** A hybrid — partially uniform, with non-uniform "copy constraints" and "selectors"
- **Strengths:**
  - More expressive than pure AIR
  - Simpler pre-processing than R1CS
  - Can incorporate specialized IOPs (lookup arguments, etc.)
  - The dominant approach for modern zkVMs
- **Weaknesses:**
  - Pre-processing still required (though universal, not circuit-specific)
  - More complex than pure AIR

#### Comparison

| Constraint System | Pre-processing | Expressiveness | Used by |
|---|---|---|---|
| R1CS | Heavy (circuit-specific) | General | Groth16, Circom, eigen-zkvm |
| AIR | None | Repetitive computations | Starknet, RISC Zero, Valida |
| PLONKish | Light (universal) | General + specialized | Plonky3, Halo2, Pico, OpenVM |

### Pillar 2: Finite Fields

A *field* is an algebraic structure where you can add, subtract, multiply, and divide (except by zero). A *finite field* has only finitely many values. All arithmetic in a zkVM happens over finite fields.

**Why it matters:** The choice of field determines:
- How fast arithmetic operations are
- How much security you get
- How large proofs are
- Whether you need extension fields

#### Common Fields in zkVMs

| Field | Prime Size | zkVMs Using It | Notes |
|-------|-----------|----------------|-------|
| **Goldilocks** | ~64-bit | ZisK, Valida | Large enough for security, small enough for fast ops |
| **BabyBear** | ~32-bit | OpenVM, Polygon zkEVM | Fast on 32-bit hardware, needs extension field for security |
| **KoalaBear** | ~32-bit | SP1, Pico | Optimized variant of BabyBear |
| **Mersenne31** | ~31-bit | Airbender | Special structure enables faster arithmetic |
| **BN254 scalar** | ~254-bit | Groth16 systems | Native elliptic curve compatibility |
| **Binary (GF(2))** | 1-bit | Binius | The radical approach |

#### The Extension Field Problem

Small fields (32-bit) are fast but don't provide enough security on their own. Solution: use *extension fields* — stack multiple small field elements to create a larger field.

Example: BabyBear⁴ means stacking 4 BabyBear elements → effectively ~128 bits of space. This is where the `⁴` and `³` notation comes from in soundcalc results.

#### Binary Fields (Binius Approach)

Binius (now Binius64, by Irreducible) takes a completely different approach: work over *towers of binary fields* (GF(2), GF(2²), GF(2⁴), ...).

**Why this is interesting:**
- Binary operations are *native* to CPUs — no modular arithmetic needed
- Data is represented as bits, not big integers
- Potentially the most efficient approach for real hardware

**Status:** Original Binius repo archived Sep 2025, succeeded by Binius64. Not yet used in any Ethproofs zkVM, but represents a potential future direction.

## How Arithmetization Connects to the Proof System

```
┌─────────────────────────────────────────────────┐
│               ARITHMETIZATION                     │
│                                                   │
│  Constraint System + Finite Field                 │
│  (R1CS, AIR, or PLONKish) over (Goldilocks,      │
│   BabyBear, KoalaBear, Mersenne31, etc.)          │
│                                                   │
│  Output: Polynomials + Constraints                │
└──────────────────┬──────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────┐
│               PROOF SYSTEM                        │
│                                                   │
│  ┌─────────────────┐  ┌─────────────────────┐   │
│  │  IOP (Interactive│  │  PCS (Polynomial     │   │
│  │  Oracle Proof)   │  │  Commitment Scheme)  │   │
│  │                  │  │                       │   │
│  │  Prover sends    │  │  Prover "commits" to  │   │
│  │  polynomials,    │  │  polynomials, later   │   │
│  │  verifier queries│  │  proves evaluations   │   │
│  │  at random points│  │  are correct          │   │
│  │                  │  │                       │   │
│  │  Examples:       │  │  Examples:            │   │
│  │  • DEEP-ALI      │  │  • FRI (most common)  │   │
│  │  • Sumcheck      │  │  • WHIR (newer, fast) │   │
│  │  • Zerocheck     │  │  • Basefold           │   │
│  │  • LogUp-GKR     │  │  • KZG (elliptic)     │   │
│  └─────────────────┘  └─────────────────────┘   │
│                                                   │
│  Made non-interactive via Fiat-Shamir heuristic   │
│  (replace verifier randomness with hash of        │
│   transcript so far)                              │
└──────────────────┬──────────────────────────────┘
                   │
                   ▼
              Cryptographic Proof
              (a few hundred KiB)
```

## Key Cryptographic Building Blocks

### Sumcheck Protocol
The workhorse of multilinear IOPs. Proves that Σ f(x₁,...,xₖ) = claimed_value over the boolean hypercube. Used by SP1, OpenVM, and most modern zkVMs.

### DEEP-ALI (Domain Extension for Eliminating Pretenders)
An IOP technique that asks the prover to evaluate trace polynomials at a random point *outside* the original domain. This dramatically reduces soundness error per query. Used by most FRI-based zkVMs (ZisK, Pico, OpenVM, Airbender).

### FRI (Fast Reed-Solomon IOP of Proximity)
The most widely used PCS in the zkVM space. Proves that a committed polynomial is "close to" a low-degree polynomial. Works by recursively halving the degree. Used by ZisK, SP1, Pico, OpenVM, Airbender.

### WHIR (Reed-Solomon Proximity Testing with Super-Fast Verification)
A *next-generation* replacement for FRI. Key innovation: verification in ~360 microseconds (vs milliseconds for FRI). Used by OpenVM2.
- **Paper:** https://eprint.iacr.org/2024/1586
- **Authors:** Gal Arnon, Alessandro Chiesa, Giacomo Fenzi, Eylon Yogev
- **Benchmarks:** degree 2²², 100-bit security → commit+open in 1.2s, 63 KiB proof, 360μs verification

### Jagged PCS
Handles sparse polynomial structures efficiently. Used by SP1. Allows committing to polynomials that have "jagged" degree bounds (different columns have different maximum degrees). More efficient than committing to all columns at the maximum degree.

### Lookup Arguments (LogUp, LogUp-GKR)
Prove that values in one column appear in a predefined table — without enumerating all possibilities. Critical for efficiently proving:
- Range checks ("is this value between 0 and 2³²?")
- Instruction decoding ("is this opcode valid?")
- Memory access ("was this memory value previously written?")

LogUp-GKR combines LogUp with GKR protocol for even more efficient lookups.

## Arithmetization Choices by zkVM

| zkVM | Constraint System | Field | PCS | IOP | Lookup |
|------|:-----------------:|:-----:|:---:|:---:|:------:|
| **SP1** | PLONKish | KoalaBear⁴ | FRI + Jagged | Sumcheck | LogUp-GKR |
| **OpenVM** | PLONKish | BabyBear⁴ | FRI | DEEP-ALI | LogUp |
| **OpenVM2** | PLONKish | BabyBear⁴ | WHIR | SWIRL | ? |
| **Pico** | PLONKish | KoalaBear⁴ | FRI | DEEP-ALI | LogUp |
| **ZisK** | AIR/PLONKish | Goldilocks³ | FRI | DEEP-ALI | ? |
| **Airbender** | Custom | M31⁴ | FRI | DEEP-ALI | ? |
| **eigen-zkvm** | R1CS (Circom+PIL) | BN254 | Groth16/PLONK | — | — |
| **Valida** | AIR | Goldilocks | FRI | DEEP-ALI | — |
| **Binius** | Custom | Binary towers | Brakedown | — | — |

## The Fundamental Trade-off

```
              Constraint complexity
              ↑
              │     R1CS (Groth16)
              │     • Small proofs
              │     • Trusted setup
              │     • Heavy pre-processing
              │
              │         PLONKish (Plonky3)
              │         • Balanced
              │         • Universal setup
              │         • Most modern zkVMs
              │
              │              AIR (Starknet)
              │              • No pre-processing
              │              • Larger proofs
              │              • Best for uniform computations
              │
              └────────────────────────────→ Expressiveness
```

*More expressive constraint systems tend to be more complex. The art is finding the sweet spot for your use case.*

## Why Arithmetization Matters for Ethereum's L1 zkEVM

1. **Security:** The constraint system determines how many "bits of security" you get. A poorly designed arithmetization can leak information or allow forged proofs. soundcalc measures this.

2. **Proof size:** PLONKish + FRI produces proofs of 200-800 KiB. PLONKish + WHIR produces proofs of ~60 KiB. OpenVM's current 7.7 MiB proofs are because its DEEP-ALI + FRI combination isn't optimized yet.

3. **Proving speed:** Field choice matters enormously. 32-bit fields (KoalaBear, BabyBear) are 2-4x faster for CPU arithmetic than 64-bit fields (Goldilocks). But 64-bit fields may offer better security per query.

4. **The H-star target:** Getting to 128-bit security with ≤300 KiB proofs requires *careful arithmetization choices*. The field, constraint system, PCS, and IOP must all work together. This is why OpenVM2 is switching to SWIRL+WHIR and ZisK uses Goldilocks³.

## Resources
- **Lita Part 2 (best overview):** https://www.lita.foundation/blog/a-zero-knowledge-paradigm-part-2--exploring-zk-vm-design-trade-offs
- **WHIR paper:** https://eprint.iacr.org/2024/1586
- **VEIL paper:** https://eprint.iacr.org/2026/683
- **Plonky3 toolkit:** https://github.com/Plonky3/Plonky3
- **Binius64:** https://github.com/IrreducibleOSS/binius64
- **soundcalc:** https://github.com/ethereum/soundcalc
- **JaggedPCS paper:** https://eprint.iacr.org/2025/917
- **LogUp paper:** https://eprint.iacr.org/2022/1530
- **LogUp-GKR paper:** https://eprint.iacr.org/2023/1284

---
*The mathematics that makes "trust but verify" into "verify, don't trust."*
