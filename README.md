# evmzk

🔬 Emerging EVM & ZK Technology Research

## Structure

```
research/
├── eips/                                    # EIP analyses
│   ├── eip-7886-delayed-execution.md        # Async block validation
│   └── eip-7805-focil.md                    # Censorship resistance via inclusion lists
├── protocols/                               # Protocol deep dives
│   ├── candidate-slot-architecture.md       # Post-zkEVM slot pipeline design
│   └── vops-partial-statelessness.md        # 8.4 GiB validator state
├── emerging/                                # Cutting-edge developments
│   ├── zkevm-l1-landscape.md               # Full teams, projects & proving landscape
│   ├── eigen-zkvm.md                        # Eigen-zkVM (0xEigenLabs)
│   └── incentive-problems.md                # Open economic problems for zkEVM adoption
├── papers/                                  # Research paper analyses
│   └── zkvm-paradigm-lita.md               # Lita's zkVM educational series
└── resources/
    └── index.md                             # All links, repos, dashboards, publications
```

## Research Focus

*Priority 1 — zkEVM-Native Ethereum:*
- EIP-7886 delayed execution + real-time zkEVM proving
- Candidate slot architecture with FOCIL + VOPS + peerDAS
- Proving teams (SP1, OpenVM, Pico, Airbender, ZisK) on Ethproofs

*Priority 2 — Formal Verification:*
- Verified zkEVM project (Lean 4 formal proofs)
- soundcalc security sprint for prover evaluation

*Priority 3 — Open Problems:*
- Fallback provers and multiplexing
- Prover killers ("you build it, you prove it")
- Blob censorship resistance
- Prover market design

## Key Insight

```
Performance:  Ethproofs (who's fastest?)
Security:     Verified zkEVM (who's mathematically correct?)
Economics:    Incentives (who gets paid, how, and what breaks?)
              ← All three required for L1 integration →
```

Target: Hegotá hard fork (late 2026) — optional proving activation.

---

*Dive deep into the future of EVM and ZK.*
