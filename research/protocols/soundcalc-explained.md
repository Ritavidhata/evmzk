# soundcalc: Universal zkEVM Soundness Calculator

## Source
- **GitHub:** <https://github.com/ethereum/soundcalc>
- **Maintainer:** Ethereum Foundation (Cryptography Team)
- **Language:** Python (81.3%), TeX (17.2%)

## What It Does
A universal calculator that estimates the *concrete soundness* (security level in bits) of hash-based zkEVM proof systems. Answers questions like:
- "What if OpenVM moves from BabyBear⁴ to Goldilocks³?"
- "What if ZisK moves from Johnson Bound to Unique Decoding Regime?"

## Why It Exists
zkEVM teams claim security levels (e.g., "128-bit security"), but these claims depend on:
1. Unproven mathematical conjectures (some recently disproven)
2. Specific parameter choices (field size, code rate, number of queries)
3. Security regime assumptions (UDR vs JBR)

soundcalc provides a *common, transparent* way to evaluate and compare actual security.

## Supported zkVMs
- ZisK, Pico, OpenVM, OpenVM2, Airbender, SP1

## Supported Proof Systems
- DEEP-ALI + FRI
- Jagged PCS + FRI
- SWIRL + WHIR

## Security Regimes Explained

### UDR (Unique Decoding Regime)
- More conservative, provable bounds
- θ ≤ (1-ρ)/2 where ρ is code rate
- Generally lower security bits for same parameters

### JBR (Johnson Bound Regime)
- More aggressive, relies on Johnson bound
- (1-ρ)/2 < θ < √ρ
- Generally higher security bits, but relies on stronger assumptions

**Important:** θ is not an input to prover/verifier code. Both regimes apply to the *same* zkEVM instance. The difference is only in how you analyze security.

## Latest Results Summary

| zkVM | Best Security | Proof Size (Expected) | Proof System | Field |
|------|--------------|----------------------|--------------|-------|
| ZisK | **128 bits** (JBR) | 269 KiB | DEEP-ALI + FRI | Goldilocks³ |
| SP1 | 100 bits (UDR) | 529 KiB | Jagged + FRI | KoalaBear⁴ |
| OpenVM | 100 bits (UDR) | 7,687 KiB | DEEP-ALI + FRI | BabyBear⁴ |
| OpenVM2 | 100 bits (UDR) | TBD | SWIRL + WHIR | BabyBear⁴ |
| Airbender | 67 bits (JBR) | 1,836 KiB | DEEP-ALI + FRI | M31⁴ |
| Pico | 53 bits (JBR) | 232 KiB | DEEP-ALI + FRI | KoalaBear⁴ |

## Critical Observations

1. **Only ZisK hits 128-bit security** — and it does so in the JBR regime (stronger assumptions). It also has the second-smallest proof size at 269 KiB.

2. **OpenVM has a proof size problem** — 7,687 KiB is 26x larger than ZisK's proof. This may not propagate reliably on Ethereum's P2P network. OpenVM2 with SWIRL+WHIR is expected to improve this dramatically.

3. **Pico and Airbender are far from target** — 53 and 67 bits respectively. Need significant parameter changes or proof system upgrades to reach even 100 bits.

4. **SP1 is well-positioned** — 100 bits with relatively compact 529 KiB proofs. Jagged PCS is a newer technique that helps both security and size.

## Recent Cryptographic Developments (Nov 2025)
- Foundational proximity conjectures were *mathematically disproven* (BCHKS25, DG25, CS25)
- soundcalc integrated the improved UDR and JBR bounds
- CBR (Combined Bound Regime) was *removed* following DG25 and CS25 results
- This is why "advertised security" may not match "actual security" — the math changed

## How to Use
```bash
python3 -m soundcalc
```
Generates/updates reports in `reports/` directory.

## Relationship to EF Milestones
- **M1 (Feb 2026):** All participating zkVMs must have circuits in soundcalc
- **M2 (May 2026):** soundcalc must report ≥100-bit provable security
- **M3 (End 2026):** soundcalc must report ≥128-bit provable security
