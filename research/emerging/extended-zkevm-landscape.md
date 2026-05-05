# Extended zkEVM Landscape: Where Everyone Stands

*Comprehensive status of every major ZK company — May 2026*

---

## The Quick Answer

| Company | Status | In L1 Proving Race? | What Happened |
|---------|--------|---------------------|---------------|
| ZisK | 🟢 Active | *Leader* (128-bit) | Ex-Polygon team, now independent (SilentSig) |
| Succinct (SP1) | 🟢 Active | Yes (100-bit) | Commercial leader, VEIL integration |
| Axiom (OpenVM) | 🟢 Active | Yes (100-bit) | Fastest proving, $20M raised |
| Brevis (Pico) | 🟢 Active | Yes (53-bit) | Modular architecture, needs security upgrade |
| Matter Labs (Airbender) | 🟢 Active | Yes (67-bit) | Largest team, weakest zkVM security |
| **Polygon zkEVM** | 🔴 **SUNSET** | No | **Shut down July 1, 2026** |
| **RISC Zero** | 🟡 Pivoted | No | Quiet — no Ethereum proving activity |
| **Zama** | 🟢 Active (different market) | No | FHE, not ZK proving. Different problem. |
| **Scroll** | 🟢 Active (L2) | Via Axiom | Proving outsourced to Axiom/OpenVM |
| **Miden** | 🟡 Building | No | Polygon incubation, privacy-focused L2 |
| **Starknet/StarkWare** | 🟢 Active (L2) | No | Cairo/Stwo, L2 ecosystem play |
| **Irreducible (Binius)** | 🔴 **SHUT DOWN** | No | Closed Nov 2025, binary fields orphaned |

---

## Deep Dives on Companies You Asked About

---

### 1. Polygon zkEVM — *Sunset July 1, 2026* 🔴

This is the biggest finding. **Polygon zkEVM Mainnet Beta is dead.**

*What happened:*
- Polygon announced in June 2025 that zkEVM Mainnet Beta would sunset in 12 months
- On July 1, 2026, the sequencer was shut down
- An exit snapshot was taken of all wallet balances
- Funds held in EOAs were migrated to Ethereum L1, claimable at `ui.agglayer.dev` until December 31, 2027
- Funds locked in DeFi protocols, multisigs, or bridges are **not recoverable**

*Why it happened:*
- Polygon zkEVM struggled with reliability issues throughout 2024-2025
- Multiple sequencer downtime incidents
- TVL never reached competitive levels vs. zkSync Era or Optimism
- Polygon pivoted to an "Open Money Stack" strategy focused on payments (Visa, Meta partnerships)
- The ZK technology was absorbed into Polygon CDK and Agglayer

*What survives:*
- **Plonky3** — the proof system originally built for Polygon zkEVM — continues as an independent project (github.com/Plonky3/Plonky3). It's the foundation for SP1, OpenVM, and Pico. This is Polygon's most lasting ZK contribution.
- **Polygon CDK** — toolkit for building ZK-powered chains using the Agglayer
- **ZisK** — the former Polygon zkEVM team spun out independently (see below)
- **Miden** — Polygon-incubated privacy L2 (still building, different architecture)

*Polygon's current strategy:*
- "Open Money Stack" — enterprise payments infrastructure
- Recent partnerships: Visa stablecoin settlements, Meta USDC creator payouts, Modern Treasury
- Private payments went live May 4, 2026
- Cross-chain interop via Polygon Trails
- Polygon is no longer a ZK proving company — they're a payments company that uses ZK

*The critical lesson:* Building a zkEVM is brutally hard. Even with $250M+ in funding and a 200+ person team, the product didn't survive. The technology outlived the product (Plonky3 → powers competitors now).

---

### 2. RISC Zero — *Pivoted/Reduced Activity* 🟡

*What they are:*
- RISC-V based zkVM using zk-STARKs
- One of the *original* general-purpose zkVMs (along with Cairo)
- Based in Seattle, VC-backed
- Open source: github.com/risc0/risc0 (actively maintained)
- Bonsai proving service

*Where they stand:*
- **Not active on Ethproofs** — the core L1 proving coordination platform
- Blog is sparse — no recent major announcements visible
- GitHub repo is maintained but no major zkEVM/Ethereum proving pushes
- The website now leads with "verifiable computation for software supply chains" and "compliance" — broader than blockchain

*What happened:*
- RISC Zero was an early leader in the zkVM space (2022-2024)
- They competed with SP1 for the "general-purpose RISC-V zkVM" market
- As SP1 pulled ahead on Ethereum proving performance and commercial traction, RISC Zero appears to have pivoted toward enterprise use cases (supply chain verification, compliance)
- Still technically strong but no longer competing in the Ethereum L1 proving race

*Technical assets:*
- Solid RISC-V zkVM implementation
- Bonsai (remote proving service)
- Groth16 wrapping for on-chain verification
- SDK for Rust, C, C++

*Assessment:* Still alive, still building, but *not in the Ethereum proving conversation anymore*. The L1 proving race has moved to SP1, OpenVM, and ZisK.

---

### 3. Zama — *Different Market Entirely* 🟢

*What they are:*
- Fully Homomorphic Encryption (FHE) company, *not* a zkVM/zkEVM company
- Based in Paris, founded by Pascal Paillier and Rand Hindi
- Technology: TFHE (Torres Fully Homomorphic Encryption)
- They do use ZK proofs — but only as one component, not the core product

*What FHE does (vs ZK):*
- ZK proves: "I computed this correctly without showing you the inputs"
- FHE enables: "Compute on encrypted data without decrypting it"
- Zama's pitch: "HTTPZ" — encrypt all data by default, like HTTPS encrypted web traffic

*Their stack:*
- `fhevm` — Solidity library for encrypted smart contracts (write `euint64` types)
- `TFHE-rs` — Rust FHE library
- `Concrete` — Python FHE compiler
- `Concrete ML` — Machine learning on encrypted data
- `$ZAMA` token launched

*Production status:*
- Ethereum mainnet: live
- Other EVM chains: H1 2026
- Solana: H2 2026 (planned)
- T-REX Ledger partnership (confidential finance)
- FHE State OS for enterprise

*How FHE + ZK + MPC work together in Zama:*
- FHE handles computation on encrypted data
- ZK ensures user inputs were encrypted correctly (lightweight proofs)
- MPC decentralizes the decryption key (no single party can decrypt)

*Why they're NOT in the zkEVM race:*
- They don't prove execution correctness — they encrypt the data
- Their goal is privacy, not validity
- FHE is post-quantum by design (no elliptic curves)
- Performance target: 100+ tx/s with GPU, thousands with FPGA/ASIC

*Assessment:* Zama is a *complement* to the zkEVM ecosystem, not a competitor. If zkEVMs prove blocks correctly, Zama makes the transaction data inside those blocks private. They're doing well — live on mainnet, token launched, clear roadmap. But it's a different problem space.

---

### 4. Scroll — *L2 With Outsourced Proving* 🟢

*What they are:*
- zkEVM Layer 2 for Ethereum (Type 1 zkEVM)
- One of the original "big three" zkEVM L2s (with Polygon zkEVM and zkSync Era)
- `chain.scroll.io` is their blockchain site; `scroll.io` is now a *completely different consumer fintech app* (same brand, different product)

*What happened:*
- Scroll was one of the first to build a production zkEVM for L2 use
- They partnered with Axiom to use OpenVM for proving
- Axiom has been finalizing Scroll mainnet blocks since April 2025
- This partnership helped Scroll reach Stage 1 rollup status

*Where they stand now:*
- Scroll's proving is handled by Axiom/OpenVM — they don't run their own prover
- Still operating as an L2
- Focused on EVM equivalence and developer experience
- Not directly in the L1 proving race (Axiom represents them)

*The key insight:* Scroll demonstrates a likely future pattern — *separation of rollup operation from proving*. A rollup doesn't need to build its own zkVM. It can outsource proving to a specialist (Axiom) and focus on its L2 business.

---

### 5. Miden — *Polygon-Incubated Privacy L2* 🟡

*What they are:*
- Polygon-incubated project
- Privacy-focused blockchain using ZK architecture
- Custom VM (not EVM-compatible)
- Local/private transaction execution

*Key features:*
- Programmable privacy (public or private transactions)
- Post-quantum secure by design
- Local execution (client-side proving)
- Use cases: private payroll, multisig, treasury management, prediction markets, neobanking

*Where they stand:*
- Still building — not yet mainnet
- Different architecture from all other projects listed
- Execution traces, recursive proofs, local execution model
- More similar to Aztec than to SP1/OpenVM

*Assessment:* Interesting architecture but unproven. Client-side proving is a different paradigm from the Ethproofs L1 proving race. Still early.

---

### 6. ZisK — *The Dark Horse Revealed* 🟢

*Major finding:* ZisK is the former Polygon zkEVM team.

From their website:
> "ZisK began as an experimental project at Polygon Labs, where a team of developers explored ways to make zero-knowledge proof generation faster, cheaper, and more accessible."
> "The team decided in June 2025 to continue the project independently and founded SilentSig Switzerland GmbH."

*What this means:*
- The people who built Polygon zkEVM left and started their own company
- They took the ZK expertise but not the Polygon brand
- SilentSig Switzerland GmbH now stewards ZisK as open-source
- Code is at `github.com/0xPolygonHermez/` (the original Polygon zkEVM repo org)

*Why they lead:*
- Only zkVM with 128-bit provable security
- 269 KiB proofs
- 98.31% blocks proven in <12 seconds
- 415,000+ blocks proven on Ethproofs
- Multiple clusters: cloud and on-prem

*The irony:* Polygon shut down zkEVM, but the team that built it went on to create the best zkVM in the market. ZisK is what Polygon zkEVM could have been if it had been given the right strategic direction.

---

### 7. Starknet / StarkWare — *L2 Ecosystem Play* 🟢

*What they are:*
- Cairo-based zkVM (custom ISA, not RISC-V or EVM)
- Stwo proving backend
- One of the best-funded ZK companies ($270M+ raised)
- Starknet L2 is live in production

*Where they stand:*
- Not in the L1 Ethereum proving race
- Proving their own L2 (Starknet) blocks, not Ethereum mainnet
- Cairo is a domain-specific language — requires developers to learn it
- Strong academic roots (Eli Ben-Sasson, co-inventor of STARKs)

*Why they're not proving L1:*
- Their architecture is purpose-built for Starknet
- Cairo → STARK pipeline doesn't naturally map to EVM execution
- They'd need to write an EVM executor in Cairo (possible but not their focus)

*Assessment:* StarkWare has the deepest ZK expertise in the industry. If they wanted to enter the L1 proving race, they could. But their business model is Starknet as an L2 ecosystem, and that's working well for them.

---

## The Bigger Picture: Market Segmentation

The ZK market has fragmented into distinct categories:

### 1. L1 Ethereum Proving (The Race)
*Who:* ZisK, Succinct, Axiom, Brevis, Matter Labs
*Prize:* Becoming consensus infrastructure for Ethereum
*Metrics:* Security (bits), proof size (KiB), latency (seconds), cost ($/proof)

### 2. L2 Rollup Proving (Commercial)
*Who:* Scroll (via Axiom), zkSync Era (Matter Labs), Starknet (StarkWare), Optimism (via Succinct)
*Prize:* Securing billions in L2 TVL
*Metrics:* Cost/tx, developer experience, EVM compatibility

### 3. Privacy / Confidentiality
*Who:* Zama (FHE), Miden (client-side ZK), Aztec
*Prize:* Making blockchain transactions private
*Metrics:* TPS on encrypted data, programmability, compliance

### 4. Verifiable Computation (Enterprise)
*Who:* RISC Zero, Succinct (partially)
*Prize:* Software supply chain verification, compliance, trust
*Metrics:* Generality, ease of integration, cost

### 5. ZK Infrastructure / Tooling
*Who:* Plonky3 (proof system library), Ethproofs (coordination), soundcalc (security auditing), ERE (unified zkVM interface)
*Prize:* Becoming the standard layer everyone builds on
*Metrics:* Adoption, performance, correctness

---

## What's Dead / Dying

| Project | What Happened | Lesson |
|---------|--------------|--------|
| Polygon zkEVM | Sunset July 2026 | Product-market fit matters more than tech |
| Irreducible (Binius) | Shut down Nov 2025 | Being 5x faster doesn't matter if you can't sustain a business |
| RISC Zero (in Ethereum) | Pivoted away | General zkVM market consolidates to 1-2 winners |
| Scroll (as prover) | Outsourced to Axiom | Rollups don't need to build their own zkVM |

---

## Updated Repo

Pushed to: `research/emerging/extended-zkevm-landscape.md`

---

*Autonomous research — May 5, 2026*
