# EIP-7886: Delayed Execution

## Metadata
- **Status:** Stagnant
- **Authors:** Francesco D'Amato (@fradamt), Toni Wahrstätter (@nerolation)
- **Created:** 2025-02-18
- **Category:** Standards Track: Core
- **Requires:** EIP-1559, EIP-2930, EIP-4844, EIP-7623, EIP-7702
- **Discussion:** https://ethereum-magicians.org/t/eip-7886-delayed-execution/22890

## Purpose
Separate block validation from execution. Validators can attest to blocks without executing transactions, unlocking async validation and higher throughput.

## Core Mechanism
- Execution outputs (receipts, blooms, requests) are *deferred* to the next block
- Block headers carry `parent_*` fields that reference the previous block's execution results
- `pre_state_root` replaces `state_root` — represents state *before* execution
- `parent_execution_reverted` flag handles reversion cases

## Key Technical Changes

### Header Changes
- `pre_state_root` — state root before execution (checked against parent's post-execution state)
- `parent_transactions_root`, `parent_receipt_root`, `parent_bloom`, `parent_requests_hash` — deferred outputs
- `parent_execution_reverted` — whether parent's execution was reverted

### Static Validation (No Execution Required)
- Verify parent execution outputs match chain-tracked values
- Verify pre_state_root matches current state
- Validate all tx signatures, fees, nonce ordering
- Check sender balances cover max possible gas fees + value
- EOA/delegation check for senders (EIP-7702)
- Gas accounting with EIP-7623 floor costs

### Pre-Charging Mechanism
- Max fees deducted from sender balances *before* execution
- Ensures transactions are economically valid without needing execution
- Compatible with EIP-1559 and EIP-4844 fee models

### Execution Reversion
- Block-level state snapshot before execution
- If declared gas_used != actual gas_used → entire block execution rolled back
- Block remains valid in chain, but no state changes applied
- `execution_reverted = True` propagated to next block
- Reverted blocks: gas_used treated as 0 for base fee calculation

## Security Model
- Economic incentives: proposers lose all execution rewards on incorrect gas declaration
- Attack cost of intentionally reverting blocks far exceeds benefits
- Transaction data still stored even on reverted blocks (DA requirement)

## Implications
- Decouples consensus from execution timing
- Enables potentially much higher block gas limits
- Prerequisite for zkEVM-based verification (validators verify proofs instead of re-executing)
- Hard fork required

## Related
- Combined with zkEVM proofs → same-slot or one-slot-delayed verification
- Combined with VOPS → validators can run with ~8.4 GiB state instead of 200+ GiB
- Combined with FOCIL → censorship resistance preserved in delayed execution world
- Combined with peerDAS → blob-native data distribution for execution payloads

---
*Analyzed 2026-05-05*
