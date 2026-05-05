# zkEVM Ecosystem: The Complete Company Map

*Every company in the ZK proving ecosystem, analyzed — May 2026*

---

## Master Table

| # | Company | Category | zkVM/Product | Funding | Status | In L1 Race? |
|---|---------|----------|-------------|---------|--------|-------------|
| 1 | **ZisK (SilentSig)** | zkVM | ZisK (DEEP-ALI + FRI) | Unknown | 🟢 Active, leading | ✅ 128-bit |
| 2 | **Succinct Labs** | zkVM + Prover Network | SP1 Hypercube | $PROVE token, VC | 🟢 Active | ✅ 100-bit |
| 3 | **Axiom / Intrinsic** | zkVM + Proving API | OpenVM 2.0 | $20M Series A | 🟢 Active | ✅ 100-bit |
| 4 | **Brevis** | zkVM | Pico | VC-backed | 🟢 Active | ✅ 53-bit |
| 5 | **Matter Labs** | L2 + zkVM | Airbender | >$250M | 🟢 Active | ✅ 67-bit |
| 6 | **Aztec** | Privacy L2 | Noir + Barretenberg | $119M+ | 🟢 Alpha live | ❌ Privacy |
| 7 | **Linea (Consensys)** | L2 | custom SNARK/STARK | Consensys-funded | 🟢 Active L2 | ❌ L2 only |
| 8 | **Lagrange Labs** | Prover Network + zkML | ZK Prover Network | $13.2M Seed | 🟢 Active | ❌ Prover infra |
| 9 | **Aligned Layer** | Verification Layer | Agnostic (EigenLayer) | VC-backed | 🟢 Testnet | ❌ Verification |
| 10 | **Ingonyama** | ZK Hardware/Accel | ICICLE library | VC-backed | 🟢 Active | ❌ Tooling |
| 11 | **LambdaClass** | Ethereum Client + ZK | ethrex, ethlambda | Bootstrapped? | 🟢 Active | ❌ Execution client |
| 12 | **StarkWare** | L2 + Proof System | Cairo + Stwo | $270M+ | 🟢 Starknet live | ❌ L2 ecosystem |
| 13 | **Zama** | FHE Privacy | fhevm, TFHE-rs | $80M+ | 🟢 Mainnet live | ❌ FHE (different) |
| 14 | **Scroll** | L2 | Outsourced to Axiom | $80M+ | 🟢 L2 live | Via Axiom |
| 15 | **RISC Zero** | General zkVM | R0VM | VC-backed | 🟡 Pivoted | ❌ Enterprise now |
| 16 | **Miden (Polygon)** | Privacy L2 | Custom ZK | Polygon-incubated | 🟡 Building | ❌ Pre-mainnet |
| 17 | **Polygon zkEVM** | ~~L2~~ | ~~Custom~~ | ~~$250M+~~ | 🔴 **Sunset Jul 2026** | ❌ Dead |
| 18 | **Irreducible** | zkVM (binary fields) | Binius64 | $24M | 🔴 **Shut down** | ❌ Dead |

---

## New Deep Dives

---

### 6. Aztec — *Privacy L2, Alpha Live* 🟢

*What they are:*
- Privacy-first Ethereum L2 using client-side ZK proofs
- Founded by Zac Williamson and Joe Andrews (2017, one of the oldest ZK teams)
- $119M+ raised (a16z, A16z Crypto, Paradigm)
- $AZTEC token now trading

*Architecture:*
- **Noir** — Rust-like DSL for writing private smart contracts
- **Barretenberg** — UltraPlonk-based proof system (their custom SNARK)
- **PXE (Private Execution Environment)** — client-side proof generation
- **AVM (Aztec Virtual Machine)** — public execution (like EVM but for Aztec)
- **Account abstraction** — every account is a smart contract with 3 key pairs (nullifier, incoming viewing, outgoing viewing)

*How it works:*
1. User writes private logic in Noir, generates ZK proof on their device (PXE)
2. Proof submitted to sequencers who validate without seeing data
3. Public aspects executed in AVM
4. Proofs settled to Ethereum L1

*Production status:*
- Alpha Network launched March 31, 2026 — first L2 with full execution environment for private smart contracts
- Decentralized from day 1 (Ignition Chain went live Nov 2025 as coordination layer)
- Community-governed (Aztec Foundation, not Aztec Labs, operates the network)
- Apps live: AzGuard (wallet), Nemi (private DEX), human.tech (compliance bridge), Nyx

*⚠️ Security concerns:*
- **Critical vulnerability in Alpha v4** discovered March 17, 2026 — affects entire proving system
- Barretenberg vulnerability allowing incorrect proofs in mempool (fixed in v4.1.2)
- Another medium/critical vulnerability found by Consensys Diligence & TU Vienna
- Fix planned for v5 release (July 2026)
- Bug tracker being established for transparency

*Where they stand:*
- The most ambitious privacy L2. But Alpha is early and has known critical vulnerabilities.
- Not in the L1 proving race — they prove their own L2 blocks, not Ethereum
- Their innovation is the UTXO-based private state model + client-side proving
- Noir is becoming a popular ZK DSL beyond Aztec itself
- If they survive Alpha → Beta, they could be the "private Ethereum" that institutions want

---

### 7. Linea (Consensys) — *L2 With Custom Proving* 🟢

*What they are:*
- Consensys-built Ethereum L2 (MetaMask integration built-in)
- Custom SNARK/STARK proving system (not using SP1/OpenVM/etc.)
- $LINEA token launched with "ETH Genesis" distribution model (no insiders)
- 20% of fees burned to reduce ETH supply

*Architecture:*
- Uses a custom proving pipeline (details sparse, Consensys keeps it proprietary)
- "Small Fields" upgrade recently deployed
- Native MetaMask, Circle (USDC), Aave, Chainlink, Lido integrations

*Where they stand:*
- Strong institutional/commercial positioning (Mastercard partnership)
- Consensys backing means deep pockets and MetaMask distribution
- NOT in the L1 proving race — they prove their own L2
- Custom proving system is a risk (no third-party auditing like soundcalc)
- Their "ETH capital" narrative (burn fees, staking) is interesting for ETH maximalists

---

### 8. Lagrange Labs — *Prover Network + zkML* 🟢

*What they are:*
- Decentralized ZK Prover Network on EigenLayer
- $13.2M Seed (Founders Fund / Peter Thiel)
- $LA token with 20% expected APY staking
- 85+ operators on the network

*Products:*
1. **ZK Prover Network** — production-ready, supports any proof type (AI, rollups, apps, coprocessors)
2. **DeepProve (zkML)** — verifiable AI inference, "158x faster than leading zkML"
3. **ZK Coprocessor** — offchain verifiable compute for apps
4. **State Committees** — ZK light clients with unbounded node sets

*Where they stand:*
- They're infrastructure, not a zkVM. Think "AWS for ZK proofs"
- Matter Labs partnership: up to 75% of outsourced proofs directed to Lagrange
- Running on EigenLayer (restaking for economic security)
- Intel Startup Accelerator + NVIDIA partnership (AI angle)
- 3M+ AI inferences proven, 11M+ ZK proofs generated, 140K+ users
- The AI + ZK intersection is their differentiator (verifiable ML inference)

---

### 9. Aligned Layer — *ZK Verification Layer* 🟡

*What they are:*
- Verification layer for ZK proofs using EigenLayer
- Built by YetAnotherCo (github.com/yetanotherco/aligned_layer)
- Agnostic — verifies proofs from *any* proof system
- Currently on testnet

*How it works:*
- Instead of verifying each proof on Ethereum L1 (expensive), batch-verify on Aligned
- Uses EigenLayer operators for decentralized verification
- Aggregates proofs and settles a single commitment to Ethereum

*Where they stand:*
- Not a prover — they're a *verifier*. Complementary to prover networks.
- Could reduce on-chain verification costs for rollups significantly
- Still testnet — not production yet
- Competes with (or complements?) direct on-chain verification

---

### 10. Ingonyama — *ZK Hardware Acceleration* 🟢

*What they are:*
- High-speed cryptography library (ICICLE)
- Accelerates ZK proof generation across hardware: CPUs, GPUs, Apple Silicon, ZPU
- Not a zkVM — they accelerate *other people's* provers

*Products:*
- **ICICLE** — multi-platform cryptography library (MSM, NTT, etc.)
- **IMP1** — mobile-first proving framework (iOS/Android), 3x faster than RapidSnark

*Who uses them:*
- **Brevis** (Pico zkVM) — multi-GPU acceleration for Groth16
- **Fermah** — confidential proving delegation
- **Zerobase** — real-time ZK
- **Zircuit** — rollup proving
- **World Foundation** (Worldcoin) — blockchain infrastructure
- **Consensys** (Linea) — gnark Groth16 proving, "order of magnitude improvement"
- **Kroma** — 4x speedup in Circom proof generation
- **DolphinusLab** — ZKWASM proving

*Where they stand:*
- The "NVIDIA of ZK" — they don't prove things, they make proving faster
- Critical infrastructure — every major prover benefits from their work
- Grant program for researchers to implement papers using ICICLE
- Apple Silicon support is unique — enables client-side proving on Macs/iPhones

---

### 11. LambdaClass — *Ethereum Client + ZK Research* 🟢

*What they are:*
- Argentine engineering firm, deep in Ethereum infrastructure
- Building **ethrex** (Rust execution engine) and **ethlambda** (Lean Consensus client)
- Also building **lambdaworks** — cryptographic library for ZK

*Key projects:*
- **ethrex** — Rust Ethereum execution engine, achieved 20x block execution speedup through profiling-driven optimization
- **ethlambda** — post-quantum Ethereum consensus client using Lean Consensus (minimalist)
- **Ethrex L2** — "different approach to rollups: simplicity, minimalism, modularity"
- **libssz** — blazing fast, zkVM-friendly SSZ Rust library
- **Lean 4 formal verification** — exploring verified ZK systems

*Where they stand:*
- Not in the proving race directly — they're building the *execution layer* that gets proven
- ethrex could be the "clean" EVM execution engine that zkVMs prove
- Their Lean 4 work connects to the EF's Verified zkEVM initiative
- Post-quantum Ethereum client (ethlambda) is forward-looking
- If Ethereum transitions to ZK-verified execution, ethrex + a zkVM = the stack

---

## Ecosystem Map

```
                         ETHEREUM L1
                             │
                 ┌───────────┼───────────┐
                 │           │           │
            Verification  Settlement  Coordination
                 │           │           │
         ┌──────┴──────┐    │    ┌──────┴──────┐
         │Aligned Layer│    │    │  Ethproofs  │
         │(batch verif)│    │    │(performance │
         └──────┬──────┘    │    │ dashboard)  │
                │           │    └─────────────┘
        ┌───────┴───────┐   │
        │  Prover Infra │   │
        │  ┌──────────┐ │   │
        │  │ Lagrange │ │   │
        │  │(decentral│ │   │
        │  │  ized)   │ │   │
        │  └──────────┘ │   │
        │  ┌──────────┐ │   │
        │  │  Succinct│ │   │
        │  │(ProverNet│ │   │
        │  │+ GPU mkt)│ │   │
        │  └──────────┘ │   │
        │  ┌──────────┐ │   │
        │  │  Axiom   │ │   │
        │  │(Proving  │ │   │
        │  │   API)   │ │   │
        │  └──────────┘ │   │
        └───────┬───────┘   │
                │           │
    ┌───────────┼───────────┤
    │           │           │
  zkVMs     zkVMs      zkVMs
    │           │           │
┌───┴───┐  ┌───┴───┐  ┌───┴───┐
│ ZisK  │  │  SP1  │  │OpenVM │ ← L1 Proving Race
│128-bit│  │100-bit│  │100-bit│
└───┬───┘  └───┬───┘  └───┬───┘
    │          │           │
    │    ┌─────┴─────┐     │
    │    │           │     │
    │  Pico       Airbend  │
    │  (53-bit)   (67-bit) │
    │                      │
    └──────────────────────┘
                │
         Guest Programs
                │
    ┌───────────┼───────────┐
    │           │           │
  revm       revm       zilkworm
(ethrex)   (reth)    (zkSyncOS)
    │           │           │
    │           │           │
 LambdaClass  EF/Parity  Matter Labs


    PRIVACY LAYER (parallel to proving)
    ┌───────────┼───────────┐
    │           │           │
  Aztec       Zama        Miden
(Noir/BB)   (FHE)     (Polygon)
    │           │           │
 L2 privacy  Cross-chain  L2 privacy
             encryption


    ACCELERATION LAYER
    ┌───────────┐
    │ Ingonyama │ ← ICICLE: GPU/CPU/Accelerator
    │(hardware) │   library for MSM, NTT, etc.
    └───────────┘   Used by Brevis, Zircuit,
                    Worldcoin, Consensys, Kroma
```

---

## Key Insights

### 1. The Stack Is Specializing
No single company does everything anymore. The market has split into:
- **Execution clients** (ethrex, reth, revm) — what gets proven
- **zkVMs** (ZisK, SP1, OpenVM) — the proving engine
- **Prover networks** (Lagrange, Succinct) — decentralized proving infrastructure
- **Verification layers** (Aligned) — batch verification
- **Acceleration** (Ingonyama) — hardware optimization
- **Privacy** (Aztec, Zama, Miden) — data confidentiality

### 2. L2s Are Outsourcing Proving
Scroll → Axiom. Matter Labs → Lagrange (75% outsourced). The future is proving-as-a-service.

### 3. AI + ZK Is Real
Lagrange's DeepProve (158x faster zkML) and NVIDIA/Intel partnerships show ZK is expanding beyond blockchain into verifiable AI. 30+ AI projects integrated.

### 4. Post-Quantum Preparation Is Serious
- LambdaClass: ethlambda (post-quantum consensus client)
- ZisK: 128-bit security (hash-based, post-quantum)
- Succinct: VEIL integration (hash-based ZK)
- Zama: FHE is post-quantum by design
- Binius64 was post-quantum (orphaned after Irreducible shutdown)

### 5. Execution Clients Matter More Than You Think
LambdaClass's ethrex achieving 20x speedup shows that the *thing being proven* (EVM execution) is also being optimized. A faster execution client = less work for the zkVM = faster proving. The proving race depends as much on execution optimization as proof system innovation.

### 6. Alpha Products Are Dangerous
Aztec's critical vulnerability in v4 (discovered March 2026, affects entire proving system) is a reminder that ZK systems are still early. The vulnerability could lead to "theft of user funds." This is why the EF's soundcalc and formal verification efforts matter.

---

*Autonomous research — May 5, 2026*
