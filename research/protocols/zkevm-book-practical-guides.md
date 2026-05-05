# zkEVM Book: Practical Guides — How zkVMs Actually Get Used

## Source
- **Book:** https://zkevm.fyi (eth-act/zkevm-book)
- **Section:** Part III — Practical Guides
- **Pages analyzed:**
  1. Getting Started / Overview
  2. Guest Program Compilation
  3. Benchmarking
  4. Clients (under construction)
  5. Process (under construction)

---

## 1. ERE: The Unified zkVM Interface

**Repo:** https://github.com/eth-act/ere

ERE (Execute-RE-prove?) is a *unified Rust API* that works across multiple zkVM backends. Think of it as the "JDBC for zkVMs" — write once, prove on any backend.

### What ERE Provides
- **Single API** for: compile → execute → prove → verify
- **Switch zkVMs** with minimal code changes
- **Two modes:** Local SDK installation (fast) or Docker-only (simple)

### Developer Workflow
```
Write guest program (Rust)
    → Use ERE library
    → Generate prover/verifier keys
    → Execute guest program
    → Generate proof
    → Verify proof
```

### Example (SP1 backend via ERE)
```rust
use ere_sp1::{EreSP1, RV32_IM_SUCCINCT_ZKVM_ELF};
use zkvm_interface::{Compiler, Input, ProverResourceType, zkVM};

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let guest_directory = std::path::Path::new("workspace/guest");

    // Compile guest program
    let compiler = RV32_IM_SUCCINCT_ZKVM_ELF;
    let program = compiler.compile(guest_directory)?;

    // Create zkVM instance (CPU proving)
    let zkvm = EreSP1::new(program, ProverResourceType::Cpu);

    // Prepare inputs
    let mut io = Input::new();
    io.write(42u32);

    // Execute, prove, verify
    let _report = zkvm.execute(&io)?;
    let (proof, _report) = zkvm.prove(&io)?;
    zkvm.verify(&proof)?;

    Ok(())
}
```

### For Client Developers
The killer use case: proving Ethereum block execution *statelessly*. An EL client (like reth) becomes the "guest program" running inside the zkVM. The client receives a block + witness (all necessary state data) as inputs, executes the block, and the zkVM generates a proof that the post-state root is correct.

Example stateless validation guest program (SP1):
```rust
sp1_zkvm::entrypoint!(main);

pub fn main() {
    let input = sp1_zkvm::io::read::<StatelessInput>();
    let genesis = Genesis { config: input.chain_config.clone(), ..Default::default() };
    let chain_spec: Arc<ChainSpec> = Arc::new(genesis.into());
    let evm_config = EthEvmConfig::new(chain_spec.clone());

    stateless_validation_with_trie::<SparseState, _, _>(
        input.block,
        input.witness,
        chain_spec,
        evm_config,
    ).unwrap();
}
```

**Key tool:** https://github.com/eth-act/zkevm-benchmark-workload — EF's higher-level tool that uses ERE to benchmark EL client + zkVM combinations.

---

## 2. Guest Program Compilation

### What Is a Guest Program?
The *application code* whose execution is being proven. In the Ethereum context, the guest program is an EL client doing stateless block validation.

### Why RISC-V?
Most zkVMs use RISC-V as the guest ISA:
- **Simple:** Small base instruction set → efficient verifiable circuits
- **Compiler support:** rustc and clang both target RISC-V
- **Modular:** Optional extensions (M for multiply, A for atomics) — zkVMs support only what they need
- **Open standard:** No licensing, permissionless innovation

### The Forked Compiler Problem
Almost every zkVM project forks the Rust compiler with custom:
- zkVM-specific compilation targets
- Custom std library functions (panic handlers, allocators, I/O)
- zkVM-specific "OS" ABI functions

**This means binaries are NOT portable across zkVMs.** A program compiled for SP1 won't run on OpenVM or Pico.

### Why Binaries Differ Between zkVMs
- **Program I/O:** How guest sends/receives data from host
- **System calls & precompiles:** keccak, ECC, other accelerated operations
- **Entrypoint:** Address of `_start` function and setup procedures
- **Memory layout:** Organization of code, stack, data segments
- **Termination:** How to signal success or panic

### The Path to a Common Binary Format
Benefits would be massive: portability, shared audits, standard toolchain, cross-zkVM testing, reduced maintenance.

**How to get there without forked compilers:**
1. Select standard RISC-V target (e.g., `rv32ima`) — Tier 2 support in stock Rust
2. Use `#![no_std]` — disable standard library
3. Define `_start` entrypoint manually
4. Initialize stack/global pointers explicitly
5. Implement panic handler and memory allocator
6. Use linker script for memory layout
7. Implement I/O per target zkVM ABI

Only one zkVM has upstreamed their target to stock Rust, and it's still Tier 3 (not auto-built/tested).

### Precompiles
RISC-V is inefficient for cryptographic operations (keccak, elliptic curves). zkVMs provide *precompiles* — dedicated circuits for these operations. Guest programs invoke them via `ecall` or custom instructions. These are critical for performance.

---

## 3. Benchmarking

### Why Benchmark zkVMs?
You can't evaluate zkVM performance with napkin math — the devil is in the engineering details:
- Implementation optimizations matter more than theory
- Each zkVM moves fast, need to catch regressions
- Different EL clients as guest programs may perform differently
- Need to understand both average cases AND worst cases (prover killers)

### What's Being Benchmarked?
Two factors determine block proving performance:
1. **The zkVM** (proving infrastructure)
2. **The EL client** (guest program — reth, erigon, etc.)

Both dimensions matter.

### Fixture Design
Fixtures = test blocks to benchmark against. Two categories:

**Average cases:** Mainnet blocks → everyday performance

**Worst cases (prover killers):**
- Blocks using all gas on a single expensive opcode
- DoS vectors (unchunkified bytecode access)
- Precompile-heavy blocks (modexp, elliptic curve ops)
- Storage-heavy blocks (SSTORE/SLOAD with cold access)

### Workload Dimensions
```
┌─────────────────────────────────────────────────┐
│  Block Execution Workload                        │
│                                                   │
│  Non-EVM Execution        EVM Execution           │
│  ├── Sender recovery      ├── Pure compute opcodes│
│  ├── Withdrawal processing│   (ADD, MUL, MULMOD)  │
│  ├── System contracts     ├── Storage opcodes     │
│  └── Other non-EVM tasks  │   (SLOAD, SSTORE)     │
│                           ├── Precompiles          │
│                           │   (ecrecover, modexp)  │
│                           └── Bytecode access      │
│                              (code loading/verify) │
└─────────────────────────────────────────────────┘
```

### Tools
- **ere:** https://github.com/eth-act/ere — unified API
- **zkevm-benchmark-workload:** https://github.com/eth-act/zkevm-benchmark-workload — benchmark runner
- **execution-spec-tests (benchmarks):** https://github.com/ethereum/execution-spec-tests/tree/main/tests/benchmark — standardized test fixtures
- **Benchmark results:** https://github.com/eth-act/zkevm-benchmark-runs

### Benchmarking Strategy
Run real mainnet blocks + crafted worst-case blocks through all EL client + zkVM combinations. When targeting a block using all gas on one opcode, the pre/post block overhead is amortized. Subtracting empty-block cost gives opcode-specific proving cost.

---

## 4. Clients (Under Construction)
Not yet written. Will cover:
- What needs to be integrated in EL clients
- Proposed architecture and standardization
- Which client parts are affected
- Reference implementations and specs

## 5. Process (Under Construction)
Not yet written. Will cover:
- Standardization effort and process
- Communication between zkVM vendors, client teams, and EF
- Specs, reference implementations, tooling

---

## Key Takeaway
The zkEVM Book's practical guides reveal the *developer infrastructure* being built around zkVMs:
- **ERE** unifies the zkVM interface (like JDBC for databases)
- **Guest compilation** is standardizing around RISC-V but binaries remain zkVM-specific
- **Benchmarking** is systematic: real mainnet + worst-case fixtures, all zkVM × EL client combinations
- **Client integration** and **standardization process** are being actively defined

This is the "plumbing" that makes the theoretical zkEVM vision practical.
