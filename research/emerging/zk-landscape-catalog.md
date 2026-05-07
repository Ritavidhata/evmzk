# ZK Products & Projects Landscape Catalog

*Last updated: May 7, 2026*

A comprehensive catalog of zero-knowledge products, protocols, and infrastructure — organized by category. Inspired by GitHub trending repos and ecosystem tracking.

---

## zkVMs (Zero-Knowledge Virtual Machines)

| Project | Stars | Language | Proof System | Status | GitHub |
|---------|-------|----------|--------------|--------|--------|
| **SP1** (Succinct) | 1.7k | Rust | PLONKish (Plonky3) | Production | [succinctlabs/sp1](https://github.com/succinctlabs/sp1) |
| **RISC Zero** | 2.1k | Rust/C++ | zk-STARKs (RISC-V) | Production | [risc0/risc0](https://github.com/risc0/risc0) |
| **Nexus zkVM** | 2.6k | Rust | SNARK | Testnet | [nexus-xyz/nexus-zkvm](https://github.com/nexus-xyz/nexus-zkvm) |
| **Jolt** (a16z) | 988 | Rust | Lasso + Spark | Research/Prototype | [a16z/jolt](https://github.com/a16z/jolt) |
| **Eigen-zkVM** | 144 | Rust | STARK + PLONK | Prototype | [0xEigenLabs/eigen-zkvm](https://github.com/0xEigenLabs/eigen-zkvm) |
| **zkMIPS / Ziren** (ProjectZKM) | 133/115 | Rust | STARK+SNARK | Prototype | [ProjectZKM/Ziren](https://github.com/ProjectZKM/Ziren) |
| **OpenVM** (Axiom) | — | Rust | PLONKish | Production | [openvm-org/openvm](https://github.com/openvm-org/openvm) |
| **Pico** (Brevis) | — | Rust | PLONKish | Testnet | [brevis-network/pico](https://github.com/brevis-network/pico) |
| **Airbender** (Matter Labs) | — | Rust | PLONKish | Development | via [matter-labs](https://github.com/matter-labs) |
| **stwo-cairo** (StarkWare) | 275 | Rust | Circle STARKs | Production | [starkware-libs/stwo-cairo](https://github.com/starkware-libs/stwo-cairo) |
| **Scroll zkVM Prover** | 31 | Rust | Custom | Testnet | [scroll-tech/zkvm-prover](https://github.com/scroll-tech/zkvm-prover) |
| **DarkFi zkVM** | 1.3k | Rust | Halo2 | Development | [darkrenaissance/darkfi](https://github.com/darkrenaissance/darkfi) |

### Notable zkVM Infrastructure
- **eth-act/ere** (82★) — Unified zkVM Interface & Toolkit. Standardizes across SP1, RISC Zero, OpenVM. [eth-act/ere](https://github.com/eth-act/ere)
- **eth-act/zkevm-benchmark-workload** (42★) — zkVM benchmarking for Ethereum. [eth-act/zkevm-benchmark-workload](https://github.com/eth-act/zkevm-benchmark-workload)
- **a16z/zkvm-benchmarks** (64★) — Cross-zkVM benchmarks including Jolt. [a16z/zkvm-benchmarks](https://github.com/a16z/zkvm-benchmarks)
- **rkdud007/awesome-zkvm** (314★) — Curated list of all zkVMs. [rkdud007/awesome-zkvm](https://github.com/rkdud007/awesome-zkvm)
- **eth-act/zkevm-book** (83★) — The zkevm.fyi book source. [eth-act/zkevm-book](https://github.com/eth-act/zkevm-book)
- **risc0/risc0-lean4** (78★) — Formal verification of RISC Zero in Lean 4. [risc0/risc0-lean4](https://github.com/risc0/risc0-lean4)

---

## ZK Libraries & Frameworks

| Project | Stars | Language | Description | GitHub |
|---------|-------|----------|-------------|--------|
| **gnark** (Consensys) | 1.7k | Go | Fast zk-SNARK library, high-level circuit API | [Consensys/gnark](https://github.com/Consensys/gnark) |
| **gnark-crypto** | 596 | Go | Elliptic curve + pairing crypto for ZK systems | [Consensys/gnark-crypto](https://github.com/Consensys/gnark-crypto) |
| **Noir** | 1.3k | Rust | Domain-specific language for ZK proofs | [noir-lang/noir](https://github.com/noir-lang/noir) |
| **Leo** (Provable/Aleo) | 4.8k | Rust | Formally verified ZK programming language | [ProvableHQ/leo](https://github.com/ProvableHQ/leo) |
| **snarkOS** (Aleo) | 4.5k | Rust | Decentralized OS for ZK applications | [ProvableHQ/snarkOS](https://github.com/ProvableHQ/snarkOS) |
| **snarkVM** (Aleo) | 1.2k | Rust | zkVM for Decentralized Private Computations | [ProvableHQ/snarkVM](https://github.com/ProvableHQ/snarkVM) |
| **ZoKrates** | 1.9k | Rust | Toolbox for zkSNARKs on Ethereum | [Zokrates/ZoKrates](https://github.com/Zokrates/ZoKrates) |
| **arkworks/groth16** | 337 | Rust | Rust Groth16 zkSNARK implementation | [arkworks-rs/groth16](https://github.com/arkworks-rs/groth16) |
| **Semaphore** | 1.1k | TypeScript | ZK protocol for anonymous interactions | [semaphore-protocol/semaphore](https://github.com/semaphore-protocol/semaphore) |

---

## ZK-Rollups & L2 Networks

| Project | Type | Proof System | Status | Key Repos |
|---------|------|--------------|--------|-----------|
| **zkSync** (Matter Labs) | Type-4 zkEVM | PLONKish / Airbender | Mainnet | [matter-labs/zksync](https://github.com/matter-labs/zksync-era) |
| **Scroll** | Type-2 zkEVM | Custom zkVM | Mainnet | [scroll-tech](https://github.com/scroll-tech) |
| **Linea** (Consensys) | zkEVM | Custom | Mainnet | [Consensys/linea-monorepo](https://github.com/Consensys/linea-monorepo) |
| **Polygon zkEVM** | Type-2 zkEVM | zkSTARKs | Mainnet | [0xPolygonHermez](https://github.com/0xPolygonHermez) |
| **StarkNet** (StarkWare) | App-specific | STARKs (stwo-cairo) | Mainnet | [starkware-libs](https://github.com/starkware-libs) |
| **Kakarot** (kkrt-labs) | zkEVM-on-StarkNet | Cairo + STARKs | Testnet | [kkrt-labs/kakarot-ssj](https://github.com/kkrt-labs/kakarot-ssj) |
| **Sovereign SDK** | Rollup framework | Modular | Framework | [Sovereign-Labs/sovereign-sdk](https://github.com/Sovereign-Labs/sovereign-sdk) (471★) |
| **Alpen** | Bitcoin L2 ZK-rollup | Rust | Development | [alpenlabs/alpen](https://github.com/alpenlabs/alpen) (96★) |
| **Kroma** | Optimistic + ZK | Tachyon GPU | Mainnet | [kroma-network](https://github.com/kroma-network) |

---

## ZK Cross-Chain & Bridging

| Project | Stars | Description | GitHub |
|---------|-------|-------------|--------|
| **Union** (unionlabs) | 74.1k | Trust-minimized ZK bridging protocol, censorship-resistant | [unionlabs/union](https://github.com/unionlabs/union) |

---

## ZK Hardware Acceleration

| Project | Stars | Description | GitHub |
|---------|-------|-------------|--------|
| **Tachyon** (Kroma) | 7.7k | Modular ZK backend accelerated by GPU (CUDA) | [kroma-network/tachyon](https://github.com/kroma-network/tachyon) |

---

## ZK Privacy & Identity

| Project | Stars | Description | GitHub |
|---------|-------|-------------|--------|
| **Nym** | 1.7k | Mixnet privacy, anonymous credentials | [nymtech/nym](https://github.com/nymtech/nym) |
| **DarkFi** | 1.3k | Anonymous, uncensored, sovereign | [darkrenaissance/darkfi](https://github.com/darkrenaissance/darkfi) |
| **Semaphore** | 1.1k | ZK protocol for anonymous interactions | [semaphore-protocol/semaphore](https://github.com/semaphore-protocol/semaphore) |
| **Prism** | 134 | Trust-minimized key transparency via light clients | [deltadevsde/prism](https://github.com/deltadevsde/prism) |

---

## ZKML (Zero-Knowledge Machine Learning)

| Project | Stars | Description | GitHub |
|---------|-------|-------------|--------|
| **EZKL** | 1.2k | ZK inference engine for deep learning models | [zkonduit/ezkl](https://github.com/zkonduit/ezkl) |

---

## ZK End-to-End Encryption

| Project | Stars | Description | GitHub |
|---------|-------|-------------|--------|
| **Ente** | 26.3k | E2EE cloud for photos, auth, files | [ente-io/ente](https://github.com/ente-io/ente) |
| **NodeWarden** | 1.9k | Bitwarden-compatible on Cloudflare Workers | [shuaiplus/nodewarden](https://github.com/shuaiplus/nodewarden) |

---

## Educational Resources

| Project | Stars | Description | GitHub |
|---------|-------|-------------|--------|
| **WTF-zk** | 2.1k | Comprehensive ZK proofs tutorial (Chinese + English) | [WTFAcademy/WTF-zk](https://github.com/WTFAcademy/WTF-zk) |
| **MyZKP** | 51 | Building ZKP from scratch in Rust | [Koukyosyumei/MyZKP](https://github.com/Koukyosyumei/MyZKP) |
| **awesome-zkevm** | 470 | Curated zkEVM resources, libraries, tools | [LuozhuZhang/awesome-zkevm](https://github.com/LuozhuZhang/awesome-zkevm) |
| **awesome-zkvm** | 314 | Curated zkVM list | [rkdud007/awesome-zkvm](https://github.com/rkdud007/awesome-zkvm) |
| **zkVM Paradigm** (Lita) | — | 3-part educational series on zkVM design | [lita.foundation](https://www.lita.foundation/blog/zero-knowledge-paradigm-zkvm) |
| **zkevm.fyi** | — | Full zkEVM book | [eth-act/zkevm-book](https://github.com/eth-act/zkevm-book) |

---

## Ethereum-Specific ZK Infrastructure

| Project | Description | Link |
|---------|-------------|------|
| **Verified zkEVM** (EF) | Formal verification in Lean 4 | [Verified-zkEVM](https://github.com/Verified-zkEVM) |
| **soundcalc** (EF) | Security calculator for zkVM proving | [ethereum/soundcalc](https://github.com/ethereum/soundcalc) |
| **eth2030** | Ethereum execution client targeting 2030 roadmap (Go, 58 EIPs) | [jiayaoqijia/eth2030](https://github.com/jiayaoqijia/eth2030) (100★) |
| **zk-static-call** | Prove eth_call result via Halo2 | [zemse/zk-static-call](https://github.com/zemse/zk-static-call) |
| **Rust-ZK-Shadow-EVM** | revm inside RISC-V ZK-VM | [zacksfF/Rust-ZK-Shadow-EVM](https://github.com/zacksfF/Rust-ZK-Shadow-EVM) |
| **proof-of-exploit/cli** | Prove smart contract bug knowledge via zkEVM | [proof-of-exploit/cli](https://github.com/proof-of-exploit/cli) |

---

## Emerging & Worth Watching

| Project | Description | Why Notable |
|---------|-------------|-------------|
| **Ziren** (ProjectZKM) | zkVM on MIPS32 ISA — alternative to RISC-V approach | MIPS ISA may have different perf/security trade-offs |
| **eth-act/ere** | Unified interface across multiple zkVMs | Could become the "WebAssembly of ZK" |
| **Alpen** | First Bitcoin ZK-rollup | Bitcoin L2 ZK is emerging category |
| **Union** | 74K stars — trust-minimized ZK bridge | Massive community interest, cross-chain focus |
| **VEIL** (Succinct) | 3% overhead ZK compiler | Post-quantum path for SP1 |
| **eth2030** | Go execution client for 2030 roadmap | Signals where EF sees L1 going |

---

*Catalog auto-generated from GitHub topics scanning (zero-knowledge, zkvm, zkevm, zksnark, zk-rollup) on May 7, 2026. Star counts approximate.*
