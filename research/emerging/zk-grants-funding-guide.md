# ZK / zkEVM Grant & Funding Guide

*Every active grant, bounty, and funding program in the ZK proving ecosystem — May 2026*

---

## Tier 1: Major Grants (>$50K)

### 1. Ethproofs Real-Time Proving Grants — *$300,000 total (3 × $100K)*

**Status:** 🟢 Open — all three prizes unclaimed as of March 2026

**Who:** Ethereum Foundation (Ethproofs team)

**What:** First team to sustain real-time proving of Ethereum L1 blocks wins $100K. Three prizes available, first-come first-served.

**Updated criteria (March 2026):**

| Criterion | Requirement |
|-----------|------------|
| Latency | P99 ≤ 10 seconds per block |
| Security | ≥ 128-bit provable (soundcalc) |
| On-prem CAPEX | ≤ $100K USD |
| Power | ≤ 10kW |
| Proof size | ≤ 300 KiB, no trusted setups |
| Open source | Fully open source |
| Measurement | Sustained live proving on Ethproofs for 1 calendar month |

**Payout:** $50K after 1 month at threshold, $50K after 5 additional months

**Current leaderboard (closest):**
- ZisK Cluster: 98.31% <12s (needs 99% at P99 ≤10s)
- ZkCloud Eth Prover v4: 94.26%
- OpenVM-Multi-GPU: 88.36%

**Apply:** Contact @fbwoolf or @corcoranwill on Telegram, or through <https://ethproofs.org>

**Why it matters:** This is the gold standard. Hitting this means your prover is production-ready for Ethereum L1.

---

### 2. Ethproofs On-Prem Proving Initiative — *$65,000 ($13K × 5 teams)*

**Status:** 🟡 Active cohort selected (May 2026), expanding later

**Who:** Ethereum Foundation (Ethproofs team)

**What:** Hardware grants for on-prem proving rigs. Each team gets $13K for a 2-GPU setup (2× NVIDIA RTX 5090, AMD EPYC 9354P, 256GB DDR5 ECC RAM).

**Current cohort:**
1. ZisK (revm guest)
2. Succinct / SP1 Hypercube (revm guest)
3. Brevis / Pico (revm guest)
4. Matter Labs / Airbender (zkSyncOS guest)
5. ~~Axiom / OpenVM~~ (declined)

**Requirements:**
- On-prem deployment only (cloud = clawback)
- Prove 1-in-10 Ethereum L1 blocks
- Quarterly operational reports
- Run through Hegotá activation (late 2026)

**Selection criteria for future expansion:**
1. Completed zkVM Security Sprint Milestone 1 with EF Crypto team
2. Active proof submissions on Ethproofs
3. Participated in Ethproofs Day demo/panel
4. Willingness to commit to on-prem deployment + reporting

**Apply:** Reach out to @fbwoolf or @corcoranwill on Telegram

---

### 3. EF Ecosystem Support Program (ESP) Grants — *Ongoing, varies*

**Status:** 🟢 Always open

**Who:** Ethereum Foundation ESP

**What:** Non-dilutive grants for infrastructure, research, tooling, and community development. 1,091+ projects funded.

**Relevant recently funded (2026 Q2):**
- *Ethproofs On-Prem Multi-GPU Prover Initiative — Matter Labs* (ZK Proofs)
- *The recursive extraction problem* (Cryptography)
- *Bivariate Polynomials — CompPoly Phase 2* (Crypto tooling)
- *Verified zkEVM ArkLib Day at ZKProof 8* (zkEVM verification)
- *Noir to LLZK compiler* (Security tooling)
- *Efficient Single-Server PIR for Ethereum Indexers* (Cryptography)
- *Internship programs* — Cryptography, Protocol, Poseidon, STEEL, PandaOps

**Funding range:** Varies widely — from a few thousand for research to six figures for infrastructure

**How to apply:**
1. Browse Wishlist/RFP items at <https://esp.ethereum.foundation/applicants>
2. Submit application with methodology, timeline, deliverables
3. Interview + review with EF team
4. Milestone-based payments

**Apply:** <https://esp.ethereum.foundation/applicants>

---

### 4. Ethereum Bug Bounty Program — *Up to $1,000,000 per finding*

**Status:** 🟢 Always open

**Who:** Ethereum Foundation

**What:** Find bugs in Ethereum clients, specs, compilers, and crypto primitives.

**Relevant scope for ZK:**
- Client bugs (execution + consensus clients)
- Specification bugs (safety/finality-breaking, DoS vectors)
- Cryptographic primitive bugs
- Dependency bugs (C-KZG-4844, Go-KZG-4844)

**Payouts:**
- Low: up to $2,000
- Medium: up to $10,000
- High: up to $50,000
- Critical: up to $1,000,000

**Submit:** <https://forms.gle/Gnh4gzGh66Yc3V7G8>

**zkEVM relevance:** As L1 zkEVM integration progresses, soundness bugs in proof systems will be critical. A forged-proof vulnerability in a zkVM targeting L1 could qualify for the highest bounties.

---

## Tier 2: Medium Grants ($10K–$50K)

### 5. Ingonyama Research Grants — *$100,000 pool*

**Status:** 🟢 Open (rolling)

**Who:** Ingonyama (ZK hardware acceleration)

**What:** Pick a research paper with reported benchmarks, re-implement the algorithm using ICICLE, and receive grants proportional to the improvement.

**Structure:**
- $1K for ~1x improvement, scaling up to $10K+ for 10x improvement
- Special bonus for algorithms using ICICLE sumcheck
- Paid in USDC

**What they provide:**
- Hardware access (GPUs)
- Developer support from ICICLE team
- Research collaboration
- Social promotion of your work

**Apply:** Fill out form at <https://forms.monday.com/forms/d0ed9699146d61e3b5a649b56ba2c663>

**Good for:** Researchers who want to implement ZK papers with hardware acceleration. Direct path from academic research to production-grade code.

---

### 6. Optimism Grants — *Ongoing*

**Status:** 🟢 Active

**Who:** Optimism Foundation

**What:** Builder grants for apps, tooling, and infrastructure on the Superchain.

**Relevant for:** ZK tooling, OP Stack proving infrastructure, cross-chain bridges

**Apply:** <https://atlas.optimism.io/>

---

### 7. Base Builder Grants — *1-5 ETH*

**Status:** 🟢 Active

**Who:** Base (Coinbase L2)

**What:** Small grants for builders on Base.

**Relevant for:** ZK applications, proving tooling for Base

**Apply:** <https://paragraph.com/@grants.base.eth/calling-based-builders>

---

### 8. Arbitrum Foundation Grants — *Ongoing*

**Status:** 🟢 Active

**Who:** Arbitrum Foundation

**What:** Grants for DApps and infrastructure on Arbitrum. 300+ projects supported.

**Relevant for:** ZK bridges, proving tooling, Orbi/Sré infrastructure

**Apply:** <https://arbitrum.foundation/grants>

---

### 9. Uniswap Foundation Grants — *Ongoing*

**Status:** 🟢 Active

**Who:** Uniswap Foundation

**What:** Programs for Unichain developer community. Novel DeFi mechanisms focus.

**Relevant for:** ZK-powered DeFi, private trading, MEV protection

**Apply:** <https://www.uniswapfoundation.org/build>

---

### 10. Polygon Grants — *Ongoing*

**Status:** 🟢 Active

**Who:** Polygon

**What:** Community grants for builders and teams committed to Polygon ecosystem.

**Note:** Polygon zkEVM was sunset July 2026, but CDK (Chain Development Kit) and AggLayer still active.

**Apply:** <https://polygon.technology/grants>

---

## Tier 3: Community & Education Grants

### 11. PSE (Privacy & Scaling Explorations) Programs — *Education + Grants*

**Status:** 🟢 Ongoing

**Who:** Ethereum Foundation PSE team

**Programs:**
- **Core Program** — 8-week intro to ZK, FHE, MPC for university students (Buenos Aires, Seoul, Tokyo, Taipei)
- **Acceleration Program** — Grants for alumni to deepen ZK/FHE/MPC research through open tasks

**Relevant for:** Learning ZK from scratch, then getting funded to contribute

**Apply:** <https://pse.dev/programs> and <https://github.com/privacy-scaling-explorations/acceleration-program>

---

### 12. Gitcoin Grants 24 (GG24) — *Quadratic Funding*

**Status:** 🟢 Active (rounds ongoing)

**Who:** Gitcoin community

**What:** Quadratic funding rounds where community donations are amplified by matching pools.

**Relevant domains:**
- **Developer Tooling & Infrastructure** — Deep Funding + QF (most relevant for ZK tooling)
- **Privacy** — Directly funds privacy solutions
- **Public Goods R&D** — Research and open-source tools

**How it works:**
1. Create project profile on Giveth
2. Apply to relevant domain round
3. Mobilize community support (many small donations > few large ones)
4. Matching pool amplifies community signals

**Apply:** <https://grants.gitcoin.co> → Apply to relevant domain

---

## Tier 4: Company-Specific Programs

### 13. Lagrange Labs — *Staking + Operator Programs*

**Who:** Lagrange Labs

**What:**
- **$LA token staking** — ~20% APY for operating on the prover network
- **Operator program** — Run proving infrastructure on EigenLayer
- 85+ operators currently active

**Relevant for:** Running decentralized proving infrastructure

**Apply:** <https://stake.lagrange.dev/>

---

### 14. Succinct Prover Network — *GPU Marketplace + $PROVE Token*

**Who:** Succinct Labs

**What:**
- **$PROVE token** — Stake to participate in decentralized proving
- **GPU marketplace** — Rent/provide GPU compute for proof generation
- Proving SP1, OpenVM, and other zkVM workloads

**Relevant for:** Running provers, earning fees from proof generation

---

## Special: EF Security Milestones (Not Grants, But Critical Path)

The EF has set three security milestones that all zkVM teams targeting L1 must hit:

| Milestone | Deadline | Requirements |
|-----------|----------|-------------|
| **M1: soundcalc integration** | End of Feb 2026 (passed) | Integrate proof system + circuits with soundcalc |
| **M2: Glamsterdam** | End of May 2026 | 100-bit provable security, proof ≤ 600 KiB, recursion architecture described |
| **M3: H-star** | End of 2026 | 128-bit provable security, proof ≤ 300 KiB, formal security argument |

Teams that hit these milestones are positioned for L1 integration. This isn't a grant, but it's the gate to the biggest opportunity in the space.

---

## Quick Decision Guide

**Building a zkVM / prover?**
→ Ethproofs RTP Grants ($100K), EF ESP Grants, On-Prem Initiative

**Doing ZK research?**
→ Ingonyama Grants ($100K pool), EF ESP Research grants

**Found a bug in a proof system?**
→ EF Bug Bounty (up to $1M), or the specific zkVM team's bounty

**Building ZK tooling / infrastructure?**
→ Gitcoin GG24 (Dev Tooling domain), EF ESP, Optimism/Base grants

**Want to learn ZK?**
→ PSE Core Program (free education), then Acceleration Program (grants)

**Want to run proving hardware?**
→ Lagrange operator program, Succinct Prover Network, Ethproofs On-Prem

**Building a ZK-powered app?**
→ Arbitrum/Base/Optimism grants, Gitcoin GG24, EF ESP

---

*Compiled May 5, 2026 — verify deadlines before applying*
