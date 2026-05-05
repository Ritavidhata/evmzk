# VOPS: Validity-Only Partial Statelessness

## Source
- **URL:** https://ethresear.ch/t/a-pragmatic-path-towards-validity-only-partial-statelessness-vops/22236

## Concept
Validators only need to maintain a minimal account table instead of full state. With zkEVM proofs guaranteeing execution correctness, validators don't need to re-execute — they only need enough state to validate the public mempool.

## The Quadruplet Table
VOPS nodes maintain only:
- `address` (20 bytes)
- `nonce` (8 bytes)
- `balance` (12 bytes)
- `codeFlag` (1 bit)

Total storage: ~8.4 GiB (vs 200+ GiB for full state)

## How It Works
1. Block proposed with execution payload + stateDiff in blobs
2. zkEVM proof proves state transition correctness
3. VOPS nodes verify proof
4. If valid, perform DAS to get stateDiff
5. Update quadruplet table with modified accounts
6. Prune stale mempool transactions

## Why This Matters
- Makes running a validator *dramatically* cheaper
- 8.4 GiB fits on consumer hardware easily
- Key enabler for delayed execution (EIP-7886) — validators don't need full state to validate
- Preserves censorship resistance via mempool validation

## Trade-offs
- Cannot serve full RPC requests (no contract storage, no code)
- Cannot re-execute blocks independently
- Relies on zkEVM proof correctness (hence the "validity-only" name)
- Still needs stateDiff from DAS — network bandwidth dependency

## Relationship to Other Proposals
- **EIP-7886:** Delayed execution requires validators to not re-execute → VOPS provides the minimal state needed
- **Candidate Slot Architecture:** VOPS nodes are the default validator in the post-zkEVM world
- **Verified zkEVM:** If the zkVM has a soundness bug, VOPS nodes accept invalid state → formal verification is critical

---
*Analyzed 2026-05-05*
