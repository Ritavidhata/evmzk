# Candidate Slot Architecture: Post-zkEVM Ethereum Design

## Source
- **URL:** https://hackmd.io/@oK3in1lRQ7-pt7b3j8nQxg/Hk9KBHsGgg
- **Title:** Making it all work: Candidate slot structure in a post-zkEVM world

## Overview
Concrete architecture combining EIP-7886 + zkEVM proofs + VOPS + peerDAS + FOCIL into a working pipeline. Validators verify proofs instead of re-executing, reducing state storage from 200+ GiB to 8.4 GiB.

## Three-Slot Pipeline

### Slot N-1
- **Includers (16):** Build inclusion lists from public mempool, broadcast over P2P
- **Builder:** Collects ILs, builds block N payload, uploads to blobs (execution payload + stateDiff + excluded IL indices)
- **Proposer (slot N-1):** Broadcasts block N-1 header + blobs + proof for block N-2
- **Prover:** Downloads blobs for N-1, generates zkEVM proof
- **Attesters:** Static validation of N-1 at t=4s, full validation of N-2 via proof

### Slot N
- **Proposer:** Broadcasts block N header + blobs + proof for block N-1
- **Prover:** Generates proof for block N
- **Attesters:** Static validation of N at t=4s, full validation of N-1 via proof
- **VOPS nodes:** Update quadruplet table if proof valid

## What Goes in Blobs
1. **Execution payload** — all transactions
2. **StateDiff** — post-execution values for all modified accounts/storage
3. **Excluded IL transaction indices** — which IL txs were validly excluded

## What Goes in Headers
- **Pre-state root** — state root after executing previous block
- **IL bitmap** (16 bits) — which includers' ILs the builder received
- **KZG commitments** — commitments to all blob data

## Roles

### Includers (16 per slot)
- Randomly selected from validator set
- Build ILs from public mempool (0–9s)
- Can be VOPS nodes
- If no block by 8s, call get_head() and build IL for that head

### Builder
- Collects ILs from CL P2P (0–11s)
- Freezes IL view at 11s
- Inserts includable IL txs into payload
- Uploads: execution payload + stateDiff + excluded IL indices to blobs

### Proposer
- Broadcasts header + blobs + proof for previous block
- Monitors for proofs of own block (0–9s)
- Freezes proof view at 9s

### zk-Prover
- DAS to reconstruct blobs (0–1s)
- Generate proof covering:
  - State transition validity + stateDiff correctness
  - IL compliance (included ILs verified, excluded txs verified invalid/full)
- Broadcast proof at 9s

### Attesters
- Static validation of current block at t=4s (header, KZG, IL bitmap)
- Full validation of previous block via proof
- No-op handling: missing/invalid proof → no state changes

### VOPS Nodes
- Verify proof, perform DAS for stateDiff
- Update: address (20B) + nonce (8B) + balance (12B) + codeFlag (1 bit)
- Prune stale mempool transactions
- Total storage: ~8.4 GiB

## Resource Requirements

| Role | Disk | Bandwidth | CPU |
|------|------|-----------|-----|
| VOPS node | 8.4 GiB quadruplet table | DAS for stateDiff only | 1 SNARK verification/block |
| Builder/Prover | Full state (unchanged) | Full DAS for all blobs | Heavy proving (GPUs) |

## Open Questions
- **Data availability:** Missing blobs → no-op blocks → no execution rewards for producer
- **Historical sync:** Can't replay execution after blob pruning window. Interaction with EIP-4444?
- **Prover markets:** Builders need reliable provers. Market mechanisms (bonds, reputation) expected to emerge.
- **Prover-builder communication:** Direct connections? All peerDAS subnets? Dedicated channel?

---
*Analyzed 2026-05-05*
