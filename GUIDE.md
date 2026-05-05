# Understanding This Repo

## Who is this for?
Developers, engineers, and technical researchers who want to understand *where Ethereum is heading* with zero-knowledge proofs — specifically, how zkEVMs will transform L1 consensus.

## What's the big picture?

Ethereum today has a bottleneck: *every validator must re-execute every transaction*. This limits throughput because execution must finish within a 12-second slot.

The solution: let a single prover execute the block and generate a *cryptographic proof* that the execution was correct. Validators verify the proof (fast) instead of re-executing (slow). This unlocks much higher throughput without centralizing the validator set.

## How does it all fit together?

```
┌─────────────────────────────────────────────────────────┐
│                ZKEVM-NATIVE ETHEREUM L1                  │
│                                                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │  EIP-7886    │  │  EIP-7805    │  │    VOPS      │  │
│  │  Delayed     │  │  FOCIL       │  │  Partial     │  │
│  │  Execution   │  │  Inclusion   │  │  Stateless-  │  │
│  │              │  │  Lists       │  │  ness        │  │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘  │
│         │                 │                  │          │
│         ▼                 ▼                  ▼          │
│  Validators attest   16 includers force-   Validators   │
│  without executing   include txs from      only need    │
│  (proof comes later) public mempool        8.4 GiB      │
│                                              state      │
│         │                 │                  │          │
│         └────────┬────────┴──────────────────┘          │
│                  ▼                                      │
│  ┌──────────────────────────────────────────────┐       │
│  │        Candidate Slot Architecture            │       │
│  │        (The 3-slot pipeline)                  │       │
│  │                                               │       │
│  │  Slot N-1: Build payload → upload to blobs    │       │
│  │  Slot N:   Propose header → prover generates  │       │
│  │            zkEVM proof (~9 seconds)            │       │
│  │  Slot N+1: Validators verify proof            │       │
│  └──────────────────────┬───────────────────────┘       │
│                          ▼                              │
│  ┌──────────────────────────────────────────────┐       │
│  │           WHO BUILDS THE PROOFS?              │       │
│  │                                               │       │
│  │  SP1 (Succinct)  │  100-bit, 529 KiB proofs  │       │
│  │  OpenVM (Axiom)  │  100-bit, but 7.7 MiB!    │       │
│  │  Pico (Brevis)   │  53-bit, needs work       │       │
│  │  Airbender (ML)  │  67-bit, non-EVM          │       │
│  │  ZisK            │  128-bit, 269 KiB ⭐       │       │
│  └──────────────────────┬───────────────────────┘       │
│                          ▼                              │
│  ┌──────────────────────────────────────────────┐       │
│  │         HOW DO WE KNOW IT'S SAFE?             │       │
│  │                                               │       │
│  │  soundcalc (EF tool) measures actual security │       │
│  │  Verified zkEVM (Lean 4) proves correctness  │       │
│  │  3 milestones: M1→M2→M3 (100→128 bits)       │       │
│  └──────────────────────┬───────────────────────┘       │
│                          ▼                              │
│  ┌──────────────────────────────────────────────┐       │
│  │          WHAT STILL NEEDS SOLVING?            │       │
│  │                                               │       │
│  │  • Fallback provers (always need ≥1 prover)   │       │
│  │  • Prover killers (blocks too hard to prove)  │       │
│  │  • Blob censorship resistance                │       │
│  │  • Only ZisK hits 128-bit security today     │       │
│  └──────────────────────────────────────────────┘       │
│                                                         │
│  ┌──────────────────────────────────────────────┐       │
│  │          THE ENDGAME (Vitalik's Vision)       │       │
│  │                                               │       │
│  │  EVM → RISC-V → ZK-native                    │       │
│  │  Hexary Patricia → Binary tree                │       │
│  │  Protocol as simple as Bitcoin                │       │
│  └──────────────────────────────────────────────┘       │
│                                                         │
│  Timeline: Fusaka ✅ → Glamsterdam → Hegotá → H-star   │
└─────────────────────────────────────────────────────────┘
```

## Reading Order

**Start here if you're new to the space:**
1. `papers/zkvm-paradigm-lita.md` — What are zkVMs, SNARKs vs STARKs, evaluation framework
2. `resources/index.md` — All links in one place

**Understand the protocol design:**
3. `eips/eip-7886-delayed-execution.md` — The foundation: decouple validation from execution
4. `eips/eip-7805-focil.md` — Censorship resistance in a builder-dominated world
5. `protocols/candidate-slot-architecture.md` — How all the pieces fit into a 3-slot pipeline
6. `protocols/vops-partial-statelessness.md` — How validators survive with 8.4 GiB of state

**Understand the security story:**
7. `emerging/ef-zkevm-security-roadmap.md` — EF's 3-phase plan: performance → security → deployment
8. `protocols/soundcalc-explained.md` — The EF's tool for measuring actual zkVM security
9. `papers/veil-zk-compiler.md` — Succinct's ZK compiler (3% overhead, post-quantum path)

**Understand the proving layer:**
10. `emerging/zkevm-l1-landscape.md` — Who is building what, performance data, formal verification
11. `emerging/eigen-zkvm.md` — A specific zkVM deep dive

**Understand the endgame:**
12. `papers/simplifying-l1-vitalik.md` — Vitalik's vision: EVM → RISC-V, simple like Bitcoin
13. `emerging/incentive-problems.md` — The 6 open economic problems blocking adoption

## Key Terms

| Term | What it means |
|------|---------------|
| zkVM | Zero-Knowledge Virtual Machine — proves program execution is correct without re-running it |
| zkEVM | zkVM specialized for Ethereum (proves EVM execution) |
| SNARK | Succinct Non-interactive ARgument of Knowledge — small proofs, often need trusted setup |
| STARK | Scalable Transparent ARgument of Knowledge — no trusted setup, larger proofs |
| revm | Rust Ethereum Virtual Machine — the EVM implementation most provers run inside their zkVM |
| Prover | The entity that executes the block and generates the cryptographic proof |
| Verifier | The entity that checks the proof (much faster than re-execution) |
| Delayed execution | Validators attest before execution finishes; results verified next slot |
| Inclusion list | Set of transactions that validators force the builder to include |
| VOPS | Validity-Only Partial Statelessness — validators store minimal state |
| peerDAS | Peer-to-peer Data Availability Sampling — distributes block data via blobs |
| Soundness | A proof system is "sound" if it never accepts false statements |
| Formal verification | Mathematical proof that code is correct (stronger than testing or auditing) |
| Lean 4 | A programming language / proof assistant used by the Verified zkEVM project |
| soundcalc | The EF's tool for calculating soundness (security level) of proof systems |
| VEIL | Succinct's lightweight ZK compiler — adds ZK to hash-based proofs with only 3% overhead |
| UDR | Unique Decoding Regime — conservative security analysis for proof systems |
| JBR | Johnson Bound Regime — more aggressive security analysis (higher claimed bits) |
| FRI | Fast Reed-Solomon IOP of Proximity — the core polynomial commitment scheme for STARKs |
| WHIR | A newer, more compact polynomial commitment scheme |
| Jagged PCS | A technique for efficient polynomial commitments with sparse structures |
| Hegotá | The upcoming Ethereum hard fork (late 2026), FOCIL likely included |
| H-star | The target for 128-bit provable security (end 2026) |
| RISC-V | A simple, open instruction set — candidate to replace EVM long-term |
| ePBS | Enshrined Proposer-Builder Separation — part of Glamsterdam |
| Gasper | Ethereum's current consensus protocol (complex, to be replaced by 3-slot finality) |

## Security Snapshot (soundcalc, May 2026)

| zkVM | Security Bits | Proof Size | Status |
|------|:------------:|:----------:|--------|
| ZisK | 128 | 269 KiB | ✅ Meets M3 target |
| SP1 | 100 | 529 KiB | ✅ Meets M2 target |
| OpenVM | 100 | 7,687 KiB | ⚠️ Security OK, proof too large |
| OpenVM2 | 100 | TBD | 🔄 SWIRL+WHIR may fix size |
| Airbender | 67 | 1,836 KiB | ❌ Below target |
| Pico | 53 | 232 KiB | ❌ Below target |

## Timeline

| When | What |
|------|------|
| ✅ Fusaka (2025) | PeerDAS, 10x data space for L2 |
| 🔄 Glamsterdam (2026) | Block-level Access Lists, ePBS, pipelining for ZK |
| 🔄 Hegotá (late 2026) | FOCIL, possibly optional ZK verification |
| 🔄 End 2026 (H-star) | 128-bit provable security target |
| 🔮 Future | EVM → RISC-V, binary tree, mandatory ZK verification |
