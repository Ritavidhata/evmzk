# Eigen-zkVM (0xEigenLabs)

## Source
- **GitHub:** https://github.com/0xEigenLabs/eigen-zkvm
- **Latest Release:** v0.0.5 (Dec 2023)
- **Stars:** 144
- **License:** Apache 2.0

## What It Is
A Rust-based zkVM built on a layered proof system (STARK → SNARK pipeline) with RISC-V ISA support. Goal: no trusted setup, constant on-chain proof size, low gas cost.

## Architecture — Layered Proof System
```
STARK (arithmetic) → Recursion → PLONK/Groth16 (solidity verifier)
```
1. Generate STARK proofs (no trusted setup)
2. Aggregate/recursively compose STARK proofs
3. Wrap in SNARK (Groth16/PLONK) for constant-size on-chain verification
4. Auto-generate Solidity verifier

## Tech Stack
- **ISA:** RISC-V
- **Frontend:** Circom + PIL (circuit definition), Rust (guest programs)
- **Proof composition:** STARK aggregation + STARK-on-STARK recursion + SNARK-on-STARK wrapping
- **GPU acceleration:** Supported but *closed source*
- **Elliptic curves:** BN128, BLS12-381
- **zkit CLI:** Universal tool for STARK, PLONK, Groth16 operations

## Key Components
- `zkvm/` — RISC-V based zkVM
- `starky/` — STARK proof generation
- `stark-circuits/` — Circuits for proof recursion
- `groth16/` — Groth16 wrapper for on-chain
- `recursion/` + `recursion-gnark/` — Recursive proof composition
- `dsl_compile/` — Domain-specific language compilation
- `algebraic-gpu/` — GPU-accelerated algebra

## Assessment
- Solid engineering but hasn't kept pace with the Ethproofs cohort
- Layered STARK→SNARK approach is proven but older than newer multilinear approaches (SP1 Hypercube, OpenVM)
- GPU code is closed source (others like SP1, RISC Zero have open GPU acceleration)
- Latest release Dec 2023 — development appears to have slowed
- Significantly smaller community than SP1 or OpenVM
- Not active on Ethproofs proving leaderboard

---
*Analyzed 2026-05-05*
