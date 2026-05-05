# evmzk

🔬 Emerging EVM & ZK Technology Research

## Quick Start

**New to the space?** Read [`GUIDE.md`](GUIDE.md) first — it explains the entire zkEVM-native Ethereum vision with diagrams, a reading order, and a glossary.

## Structure

```
research/
├── eips/                                    # EIP analyses
│   ├── eip-7886-delayed-execution.md        # Async block validation
│   └── eip-7805-focil.md                    # Censorship resistance via inclusion lists
├── protocols/                               # Protocol & tool deep dives
│   ├── candidate-slot-architecture.md       # Post-zkEVM slot pipeline design
│   ├── vops-partial-statelessness.md        # 8.4 GiB validator state
│   └── soundcalc-explained.md               # EF's zkVM security calculator
├── emerging/                                # Cutting-edge developments
│   ├── zkevm-l1-landscape.md               # Full teams, projects & proving landscape
│   ├── ef-zkevm-security-roadmap.md         # EF's 3-phase plan (performance → security)
│   ├── incentive-problems.md                # Open economic problems for zkEVM adoption
│   └── eigen-zkvm.md                        # Eigen-zkVM (0xEigenLabs)
├── papers/                                  # Research paper analyses
│   ├── zkvm-paradigm-lita.md               # Lita's zkVM educational series
│   ├── veil-zk-compiler.md                  # VEIL: 3% overhead ZK (post-quantum path)
│   └── simplifying-l1-vitalik.md            # EVM → RISC-V endgame vision
└── resources/
    └── index.md                             # All links, repos, dashboards, papers
```

## Research Focus

*Priority 1 — zkEVM-Native Ethereum:*
- EIP-7886 delayed execution + real-time zkEVM proving
- Candidate slot architecture with FOCIL + VOPS + peerDAS
- Proving teams (SP1, OpenVM, Pico, Airbender, ZisK) on Ethproofs

*Priority 2 — Security & Formal Verification:*
- soundcalc: measuring actual zkVM security (soundness in bits)
- EF's 3-milestone roadmap (M1: soundcalc → M2: 100-bit → M3: 128-bit)
- Verified zkEVM project (Lean 4 formal proofs)
- Only ZisK currently hits 128-bit security target

*Priority 3 — Cryptographic Advances:*
- VEIL: lightweight ZK for hash-based proofs (3% prover overhead)
- WHIR, JaggedPCS: next-gen polynomial commitment schemes
- Proximity gap breakthroughs and their impact on security claims

*Priority 4 — The Endgame:*
- Vitalik's simplification roadmap: EVM → RISC-V
- Binary tree replacing hexary Merkle Patricia
- Protocol as simple as Bitcoin

## Key Insight

```
   Performance:  Ethproofs (who's fastest?)
   Security:     soundcalc (who's mathematically provable?)
   Economics:    Incentives (who gets paid, how, what breaks?)
   Endgame:      Simplification (can a student understand it?)
                 ← All four required for L1 integration →
```

## Security Snapshot (May 2026)

| zkVM | Security | Proof Size | Target Met? |
|------|:--------:|:----------:|:-----------:|
| ZisK | 128 bit | 269 KiB | ✅ M3 |
| SP1 | 100 bit | 529 KiB | ✅ M2 |
| OpenVM | 100 bit | 7.7 MiB | ⚠️ Too large |
| Pico | 53 bit | 232 KiB | ❌ |
| Airbender | 67 bit | 1.8 MiB | ❌ |

## Timeline

- ✅ Fusaka (2025) — PeerDAS shipped
- 🔄 Glamsterdam (2026) — ePBS, block access lists, ZK pipelining
- 🔄 Hegotá (late 2026) — FOCIL, optional ZK verification
- 🎯 End 2026 — 128-bit provable security target (H-star)

---

*Dive deep into the future of EVM and ZK.*
