# The zkEVM Market: Companies, Technology, and What's Emerging

*Comprehensive landscape analysis — May 2026*

---

## Market Overview

The zkVM/zkEVM market in 2026 is defined by a single race: *who will prove Ethereum L1 blocks in real-time, securely enough to become consensus infrastructure?*

**Market size signals:**
- Ethereum Foundation has committed *at least $365K* in direct grants ($300K RTP + $65K on-prem)
- No venture capital is directly funding L1 proving — this is infrastructure play
- L2 proving market is already commercial (Scroll, Base, OP Superchain)

**Key dynamics:**
- 7 zkVMs and 15 prover teams active on Ethproofs
- Average proving latency dropped from 16 min 44s → ~60s in 2025 (5× improvement)
- Average proving cost dropped from $1.69 → $0.0376 per block (45× improvement)
- ~200,000 Ethereum blocks proven on Ethproofs in its first year
- L1 gas limits expected to rise ~3× in 2026 (EIP-7938)

---

## The Companies

### Tier 1: Ethproofs Proving Cohort (L1 Focus)

#### 1. Succinct Labs (SP1 Hypercube)
- **zkVM:** SP1 (RISC-V, Plonky3-based)
- **Funding:** Token ($PROVE), VC-backed (Paradigm among investors)
- **Headcount:** ~40-60 (est.)
- **Revenue model:** Decentralized Prover Network, enterprise contracts
- **Production use:**
  - Optimism Superchain ZK (selected as proving partner)
  - Base — proving $7.4B in deposits
  - Google Quantum AI uses SP1 for research
  - ZCAM — cryptographic camera for content authenticity
- **Security (soundcalc):** 100 bits (UDR), 529 KiB proofs
- **Innovation:** VEIL compiler (3% overhead ZK, post-quantum path), Jagged PCS
- **Ethproofs:** Active, 100-bit security
- **On-prem cohort:** Yes (accepted)
- **Status:** The *commercial leader*. Most production traction. VEIL paper gives them a post-quantum narrative.

#### 2. Axiom / Intrinsic Technologies (OpenVM)
- **zkVM:** OpenVM 2.0 (modular chips, Plonky3-based)
- **Funding:** $20M Series A (Paradigm, Standard Crypto) — Jan 2024
- **Headcount:** ~20-30 (est.)
- **Revenue model:** Axiom Proving API (commercial proving service), enterprise
- **Production use:**
  - Scroll mainnet (finalizing since April 2025, enabling Stage 1 rollup)
  - Lighter EVM (EVM-equivalent rollup powered by OpenVM 2.0)
- **Security (soundcalc):** 100 bits (UDR), but 7,687 KiB proofs (OpenVM 1.5)
- **Innovation:** OpenVM 2.0 Beta with SWIRL+WHIR (sub-300 KiB proofs, fastest on Ethproofs as of April 2026), formal verification of RV32IM in Lean by Nethermind Research
- **Ethproofs:** Active, fastest zkVM (OpenVM 2.0)
- **On-prem cohort:** Declined (not participating in $65K on-prem program)
- **Status:** Technically sophisticated, production-proven with Scroll. OpenVM 2.0 leapfrogs on speed. Strong formal verification story. Declining on-prem suggests they're focused on their commercial proving API instead.

#### 3. Brevis (Pico)
- **zkVM:** Pico (glue + coprocessor model, Plonky3-based)
- **Funding:** VC-backed (specifics not public)
- **Revenue model:** Brevis ecosystem, proving services
- **Security (soundcalc):** 53 bits (JBR), 232 KiB proofs
- **Innovation:** Modular "chip" architecture with interchangeable backends
- **Ethproofs:** Active, Security Sprint M1 completed
- **On-prem cohort:** Yes (accepted)
- **Status:** Smallest proofs (232 KiB) but lowest security (53 bits). Needs significant proof system upgrades. Modular architecture is technically interesting but market traction unclear.

#### 4. Matter Labs (Airbender)
- **zkVM:** Airbender (custom VM, zkSyncOS)
- **Funding:** Massive VC backing (>$250M total raised)
- **Headcount:** 200+ (largest team in the space)
- **Revenue model:** zkSync Era L2, zkSync OS ecosystem
- **Security (soundcalc):** 67 bits (JBR), 1,836 KiB proofs
- **Ethproofs:** Active, cheapest prover ($0.0032/proof via Zilkworm)
- **On-prem cohort:** Yes (accepted)
- **Unique:** Only team NOT using revm as guest. Proving zkSyncOS, not EVM. This is either a strategic advantage (vertical integration) or a liability (not proving Ethereum's execution environment).
- **Status:** Well-funded but their zkVM is the *weakest on security*. Proving a non-EVM execution environment creates questions about L1 integration path.

#### 5. ZisK
- **zkVM:** ZisK (custom, DEEP-ALI + FRI)
- **Funding:** Unknown (no public VC rounds)
- **Headcount:** Small team
- **Security (soundcalc):** 128 bits (JBR), 269 KiB proofs ← *THE LEADER*
- **Ethproofs:** Top performer: 98.31% blocks <12s across 415K+ proofs
- **On-prem cohort:** Yes (accepted)
- **Clusters:** ZisK Cluster, ZisK Sevilla Cloud, ZisK Girona On-Prem, ZkCloud Eth Prover v3/v4 (ZisK-based)
- **Status:** The *dark horse*. Unknown funding, small team, but *only zkVM hitting 128-bit security target with compact proofs*. Leads on both security AND performance. The team to watch.

### Tier 2: Active But Not in On-Prem Cohort

#### 6. RISC Zero (R0VM)
- **zkVM:** RISC-V based, Bonsol
- **Funding:** VC-backed
- **Status:** Early leader in the zkVM space but appears to have pivoted focus away from L1 Ethereum proving. Active in other ecosystems.

#### 7. Polygon (zkEVM)
- **zkVM:** Custom, tied to Solidity
- **Funding:** Well-funded ($MATIC ecosystem)
- **Status:** L2 focused. Not active in L1 proving race.

#### 8. Starknet / StarkWare (Stwo)
- **zkVM:** Cairo-based, custom ISA
- **Funding:** One of the best-funded ZK companies
- **Status:** L2 ecosystem play. Stwo is their proving backend. Not directly competing for L1 Ethereum proving but could pivot.

#### 9. Irreducible (Binius64) ⚠️ SHUTTING DOWN
- **zkVM:** Binius64 (binary tower fields, CPU-optimized circuits)
- **Funding:** $24M Series A
- **Status:** "Irreducible is Shutting Down" — announced Nov 12, 2025. Binius64 showed ~5x CPU performance vs SP1/R0VM on GPUs for signature aggregation. Revolutionary approach (binary fields instead of prime fields). But company is closing. Technology may be acquired or open-sourced.
- **Legacy:** Proved that custom circuits over binary fields can outperform general-purpose zkVMs on GPUs. This insight may influence future designs.

---

## Technology Landscape

### The Stack (Bottom to Top)
```
┌─────────────────────────────────────────────────┐
│  APPLICATION LAYER                               │
│  Ethereum L1 proving, L2 rollups, privacy,       │
│  content authenticity, identity credentials       │
├─────────────────────────────────────────────────┤
│  PROVING INFRASTRUCTURE                          │
│  Ethproofs (coordination), Decentralized Prover   │
│  Networks (Succinct), Axiom Proving API           │
├─────────────────────────────────────────────────┤
│  GUEST PROGRAMS (what's being proven)            │
│  revm (reth), zilkworm, zkSyncOS                 │
├─────────────────────────────────────────────────┤
│  zkVM LAYER                                      │
│  SP1, OpenVM, Pico, ZisK, Airbender              │
├─────────────────────────────────────────────────┤
│  PROOF SYSTEM (arithmetization + cryptography)   │
│  DEEP-ALI, SWIRL, FRI, WHIR, Jagged PCS,         │
│  Sumcheck, LogUp-GKR, VEIL                       │
├─────────────────────────────────────────────────┤
│  FIELD ARITHMETIC                                │
│  Goldilocks (64-bit), KoalaBear (32-bit),         │
│  BabyBear (32-bit), Mersenne31 (31-bit),         │
│  Binary towers (Binius)                          │
├─────────────────────────────────────────────────┤
│  HARDWARE                                        │
│  NVIDIA RTX 5090, L40S, 6000 Ada │ CPU (x86, ARM)│
│  FPGA (exploration) │ TPU (speculative)          │
└─────────────────────────────────────────────────┘
```

### Technology Battlegrounds

**1. Prime Fields vs Binary Fields**
- *Prime fields* (Goldilocks, BabyBear, KoalaBear): The mainstream. All 5 on-prem teams use these.
- *Binary fields* (Binius): Revolutionary but unproven at scale. Irreducible's shutdown leaves this direction without a champion.
- *Why it matters:* Binary field arithmetic is native to CPUs (no modular reduction). If someone makes it work for full zkEVM proving, it could be 5-10x faster.

**2. General-Purpose zkVM vs Custom Circuits**
- *GP zkVMs* (SP1, OpenVM, Pico, ZisK): Run any RISC-V program. Flexible but carry interpreter overhead.
- *Custom circuits* (Binius64): Direct circuit description. Faster but harder to develop.
- *Middle ground:* SP1/OpenVM precompiles (accelerated circuits for specific ops within a GP zkVM).

**3. FRI vs WHIR**
- *FRI:* The dominant PCS. Mature, well-understood. Used by all 5 Ethproofs zkVMs.
- *WHIR:* Next generation. 360μs verification (vs milliseconds for FRI). Used by OpenVM 2.0.
- *Why it matters:* WHIR could dramatically reduce proof sizes. If OpenVM 2.0 delivers on its beta numbers, it may force other zkVMs to switch.

**4. Security Regimes: Provability vs Conjectures**
- Nov 2025: Foundational proximity conjectures *mathematically disproven*
- Teams relying on CBR (Combined Bound Regime) had their security claims invalidated
- soundcalc now the standard measurement tool
- Only ZisK has 128-bit provable security today
- The market is shifting from "trust our security claims" to "prove it with soundcalc"

---

## What's Emerging (May 2026)

### 1. On-Prem Proving (May 2026 Launch)
5 teams deploying 2-GPU rigs in their own facilities. Proving 1-in-10 L1 blocks. First operational data expected June 2026. This is the *bridge* between cloud demos and decentralized proving.

### 2. VEIL Integration into SP1
Succinct's VEIL paper (May 2026 blog post) is being integrated into SP1. Replaces Groth16 wrapper with hash-based ZK → SP1 becomes fully post-quantum. Prototype already in SP1 repo.

### 3. OpenVM 2.0 Beta (April 2026)
"Fastest zkVM on Ethproofs" per Axiom. Sub-300 KiB proofs, 100-bit security, SWIRL+WHIR proof system. Lighter EVM partnership shows commercial traction.

### 4. Hegotá Hard Fork (Late 2026)
- FOCIL likely included (censorship resistance)
- Optional proving may be activated
- On-prem cohort generates operational evidence through Hegotá

### 5. soundcalc Security Milestones
- M1 (Feb 2026): ✅ All 5 on-prem teams completed
- M2 (End May 2026): 100-bit provable security + ≤600 KiB proofs
- M3 (End 2026): 128-bit provable security + ≤300 KiB proofs

### 6. Post-Quantum Preparations
- Google Quantum AI paper (April 2026): quantum threat "decades closer than expected"
- VEIL gives SP1 a post-quantum path
- Binius64 was post-quantum by design but Irreducible shut down
- Vitalik's long-term vision: hash-based everything

### 7. Gas Limit Increases
- EIP-7938 proposes ~3× gas limit increase in 2026
- More gas → harder to prove → more pressure on zkVM performance
- This is why raw speed still matters even as security takes center stage

---

## Market Dynamics

### Who Makes Money?
| Company | Revenue Source | Amount (est.) |
|---------|---------------|---------------|
| Succinct | Prover Network fees, enterprise contracts | Significant (Base, OP partnerships) |
| Axiom | Axiom Proving API (SaaS) | Growing (Scroll, Lighter) |
| Matter Labs | zkSync Era sequencer fees | Largest revenue |
| ZisK | Grants + unclear commercial model | Minimal |
| Brevis | Brevis ecosystem | Early stage |

### The $300K Question
Three $100K RTP grants remain unclaimed. Updated criteria (March 2026):
- P99 ≤10s sustained for 1 calendar month
- ≥128-bit provable security (soundcalc)
- ≤$100K CAPEX, ≤10kW power
- ≤300 KiB proofs, fully open source

ZisK leads at 98.31% <12s but hasn't hit 99% for a full month. Nobody has claimed.

### Funding Landscape
- **EF direct:** $365K+ in grants (RTP + on-prem)
- **EF indirect:** soundcalc development, Verified zkEVM, cryptography team, zkEVM team salaries
- **VC:** Succinct, Axiom, Matter Labs all well-funded
- **Token:** Succinct ($PROVE), zkSync ($ZK) — market-driven proving

### Competitive Moats
| Moat | Who Has It |
|------|-----------|
| Security (128-bit) | ZisK only |
| Production traction | Succinct (OP, Base, Google) |
| Formal verification | Axiom (OpenVM in Lean), Verified zkEVM project |
| Cost efficiency | Axiom ($0.0125/proof), Zilkworm ($0.0032/proof) |
| Largest ecosystem | Matter Labs (zkSync) |
| Fastest proving | Axiom (OpenVM 2.0 Beta) |

---

## Risks and Unknowns

1. **Gas limit increases** could make current performance insufficient → requires constant optimization
2. **Post-quantum timeline** uncertain → hash-based systems are ready, but migration is disruptive
3. **ZisK's anonymity** — who are they? No public team, no VC rounds, no token. 128-bit security leader but opaque.
4. **Binary fields** remain unproven at zkEVM scale — Binius64 showed promise but company shut down
5. **Matter Labs' non-EVM path** — Airbender proves zkSyncOS, not Ethereum. Unclear if this is viable for L1.
6. **Client diversity for zkVMs** — EF wants multiple independent zkVMs (like they have multiple EL clients). If only 2-3 are viable, centralization risk.
7. **On-prem may not work** — real-world DevOps, power, thermal, networking constraints could reveal problems

---

## Resources
- **Ethproofs:** https://ethproofs.org
- **Ethproofs 2025 Review:** https://ethproofs.org/blog/ethproofs-2025-review-2026-roadmap
- **On-Prem Initiative:** https://ethproofs.org/blog/the-ethproofs-on-prem-proving-initiative-updated-real-time-proving-grants
- **Succinct blog:** https://blog.succinct.xyz
- **Axiom:** https://axiom.xyz
- **Irreducible (archived):** https://www.irreducible.com
- **soundcalc:** https://github.com/ethereum/soundcalc
- **Benchmark runs:** https://github.com/eth-act/zkevm-benchmark-runs

---
*Autonomous research — May 5, 2026*
