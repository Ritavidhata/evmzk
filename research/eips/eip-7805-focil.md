# EIP-7805: FOCIL (Fork-choice enforced Inclusion Lists)

## Metadata
- **Status:** Draft
- **Authors:** Thomas Thiery, Francesco D'Amato, Julian Ma, Barnabé Monnot, Terence Tsao, Jacob Kaufmann, Jihoon Song
- **Created:** 2024-11-01
- **Category:** Standards Track: Core
- **Discussion:** https://ethereum-magicians.org/t/eip-7805-committee-based-fork-choice-enforced-inclusion-lists-focil/21578
- **Affiliation:** Ethereum Foundation, Offchain Labs

## Purpose
Preserve Ethereum's censorship resistance in a builder-dominated world. Allow a committee of validators to force-include transactions in every block.

## The Problem
Block building has been auctioned to specialized builders. A few sophisticated entities dominate block production → censorship resistance deteriorates. Validators need a mechanism to impose constraints on builders.

## How FOCIL Works

### Roles & Timeline

**IL Committee Members (16 validators per slot):**
- Slot N, t=0–8s: Build ILs from public mempool, broadcast over P2P
- If no block by t=7s, call `get_head()` and build IL for local head

**Validators:**
- t=0–9s: Receive and store ILs, record equivocation evidence
- t=9s (freeze): Stop storing new ILs, continue forwarding
- After next slot t=4s: Ignore all ILs from previous committee

**Builder:**
- t=0–11s: Receive ILs from P2P, cache valid ones
- t=11s: Freeze IL view, update execution payload with IL transactions

**Proposer:**
- Slot N+1, t=0s: Broadcast block with IL-satisfying payload

**Attesters:**
- Slot N+1, t=4s: Verify IL satisfaction before voting
- Check: are all IL transactions present, OR are missing ones invalid/block full?

### Fork-Choice Enforcement
Attesters *only vote for blocks that satisfy IL conditions*. Blocks that fail → not voted on → cannot be canonical. This is enforced at the fork-choice level, making it unbypassable.

### Execution Layer Check
After executing all payload transactions, for each IL transaction T:
1. Is T already in the block? → skip
2. Does block have enough gas for T? → skip if not
3. Validate T against post-execution state (nonce + balance)
4. If T is valid and not included → return `INCLUSION_LIST_UNSATISFIED`

Block is valid but CL will NOT attest to it.

### Engine API Changes
- New: `engine_getInclusionListV1` endpoint
- Modified: `engine_forkchoiceUpdated` — payloadAttributes extended with IL
- Modified: `engine_newPayload` — includes IL transactions parameter

## Key Design Properties

| Property | Description |
|----------|-------------|
| Committee-based | 16 validators per slot, not single proposer — reduces bribery surface |
| Fork-choice enforced | Built into consensus, cannot be bypassed |
| Same-slot | IL constraints from slot N apply to block N+1 (not N+2 like forward ILs) |
| Conditional inclusion | Missing IL txs are OK if invalid or block is full |
| Anywhere-in-block | IL txs can go anywhere in the block, not just at the end |
| No explicit rewards | Relies on 1-of-N altruistic honesty assumption |

## Consensus Specs
- Beacon chain: https://github.com/ethereum/consensus-specs/blob/e678deb772fe83edd1ea54cb6d2c1e4b1e45cec6/specs/_features/eip7805/beacon-chain.md
- Fork choice: https://github.com/ethereum/consensus-specs/blob/e678deb772fe83edd1ea54cb6d2c1e4b1e45cec6/specs/_features/eip7805/fork-choice.md
- P2P: https://github.com/ethereum/consensus-specs/blob/e678deb772fe83edd1ea54cb6d2c1e4b1e45cec6/specs/_features/eip7805/p2p-interface.md
- Validator guide: https://github.com/ethereum/consensus-specs/blob/e678deb772fe83edd1ea54cb6d2c1e4b1e45cec6/specs/_features/eip7805/validator.md

## Security Considerations

### IL Equivocation
- Allow max 2 ILs per committee member (P2P rule)
- If 2 different ILs detected from same member → ignore all their ILs
- Worst case: IL gossip bandwidth doubles

### Builder Connectivity
Builder must receive ILs by t=9s freeze → must be well-connected to committee members. Sufficient buffer needed between freeze and block broadcast.

### Payload Construction Complexity
Naive approach: O(n²) validity checks for n IL transactions. Efficient approach: track nonce/balance of IL-involved EOAs during payload construction → real-time validity tracking with minimal overhead.

## Relationship to zkEVM-Native Architecture
In the candidate slot architecture (HackMD spec):
- 16 includers enforce censorship resistance via FOCIL
- IL bitmap (16 bits) in block header indicates which includers' ILs were received
- Provers verify IL compliance as part of the zkEVM proof
- Delayed execution + FOCIL ensures censorship resistance even when validators don't re-execute

## Related EIPs
- EIP-7547: Forward inclusion lists (predecessor, 1-slot delay)
- EIP-7886: Delayed execution (complementary)
- Requires hard fork

---
*Analyzed 2026-05-05*
