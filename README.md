<div align="center">
  <h1 align="center">awesome zkVM</h1>

A curated list of zkVM, zero-knowledge virtual machine.

  <p align="center">
    <a href="https://github.com/sindresorhus/awesome">
      <img alt="awesome list badge" src="https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg">
    </a>
    <a href="https://github.com/rkdud007/awesome-zkvm/graphs/contributors">
      <img alt="GitHub contributors" src="https://img.shields.io/github/contributors/rkdud007/awesome-zkvm">
    </a>
    <a href="http://makeapullrequest.com">
      <img alt="pull requests welcome badge" src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat">
    </a>
  </p>

</div>

[Contributions](./CONTRIBUTING.md) and suggestions are always welcome; open issues or pull requests with any changes you want to be made.

# contents

- [contents](#contents)
  - [projects](#projects)
  - [bench](#bench)
  - [Independent/third-party Benchmarks](#independentthird-party-benchmarks)
  - [papers](#papers)
    - [Cairo](#cairo)
    - [Ceno](#ceno)
    - [Jolt](#jolt)
    - [SP1](#sp1)
    - [Risc Zero](#risc-zero)
    - [EDEN](#eden)
  - [resources](#resources)
  - [tutorials / educational zkVM](#tutorials--educational-zkvm)
  - [related tooling](#related-tooling)
  - [related awesome lists](#related-awesome-lists)

## projects

> [!NOTE]  
> Maintained by [@0xpiapark](https://x.com/0xpiapark) and [@alexanderlhicks](https://x.com/alexanderlhicks). Some details may be outdated; feel free to open an issue or PR. For discussions on fair tracking methods, see the [open issues](https://github.com/rkdud007/awesome-zkvm/issues).

- ISA ([Instruction Set Architecture](https://en.wikipedia.org/wiki/Instruction_set_architecture)): The fundamental “language” of the VM, defining all its basic operations and how they interact with data.
- Continuations (aka Chunking/Sharding): A technique to break oversized computations — too big for a single run — into smaller segments that get proven individually and recursively (proof of segment `n+1` includes the proving of segment `n`'s verifier). How self-contained the proving of each segment is (no commitment passed from one segment to the other) and how efficiently parallelizatable it is (minimal inter-thread comm) differs from one implementation to another.
- Precompiles (Built-ins, Chiplets, Accelerate etc): Specialized, pre-built functions for complex tasks (like cryptography) that boost efficiency and reduce proof overhead.
- Proving Frontend: The programming language the programmer expresses their business logic in, which then get compiled down into the VM’s supported ISA for constrained execution.
- GPU: Indicates whether proving on GPU is supported (based on publicly exposed Metal/CUDA code)
- Optimizations: Ingredients in the proof system that can optimize the size and complexity of the constraints overall. 
- Backends: The (Polynomial) Interactive Oracle Proof (P)IOP and polynomial commitment scheme(s) (PCS) used for the (typically non-interactive) prover-verifier checks. 
- Verifiers: Programs that can do the (typically non-interactive) verification given a proof and public inputs. 

|                               zkVM                                |         ISA          |   Continuations    |  Parallelizable Proving  |  Precompiles   |      GPU       |          Frontend                   |            Arithmetization          |                                   Optimizations                              |                          Backends                                         |        Verifiers    |                               zkVM                                |
| :---------------------------------------------------------------: | :------------------: | :----------------: | :----------------------: | :------------: | :------------: | :---------------------------------: |:-----------------------------------:|:----------------------------------------------------------------------------:|:-------------------------------------------------------------------------:|:-------------------:|:-----------------------------------------------------------------:|
|         [cairo](https://github.com/lambdaclass/cairo-vm)          |        Cairo         |        :x:         |                :x:       | :white_check_mark: |                |                Cairo                |               `AIR`                 |                                                                              |                          FRI                                              |                     |         [cairo](https://github.com/lambdaclass/cairo-vm)          |
|            [ceno](https://github.com/scroll-tech/ceno)            |        RISC-V        |        :x:         |                :x:       |         :x:         |                |                Rust                 |               `GKR`                 | `Lookup` `Sumcheck`                                                          |                          Brakedown                                        |      Rust           |            [ceno](https://github.com/scroll-tech/ceno)            |
|      [eigen zkvm](https://github.com/0xEigenLabs/eigen-zkvm)      |        RISC-V        | :white_check_mark: |       :white_check_mark: | :white_check_mark: | :white_check_mark: |                Circom, PIL          |               `eAIR`                |                                                                              |                          FRI, Groth16                                     |    Solidity         |      [eigen zkvm](https://github.com/0xEigenLabs/eigen-zkvm)      |
|               [jolt](https://github.com/a16z/jolt)                |        RISC-V        |        :x:         |                :x:            |         :x:         |                |                Rust                 |               `R1CS`                | `Lookup` `Sumcheck` `Offline Mem Check`                                      |                          Spartan                                          |      WASM           |                [jolt](https://github.com/a16z/jolt)               |
|        [miden](https://github.com/0xPolygonMiden/miden-vm)        | MASM(Miden Assembly) |        :x:         |                :x:            | :white_check_mark: | :white_check_mark: |             Rust, Wasm              |               `AIR` (winterfell)    | `Lookup`                                                                     |                          Winterfell                                       |      Rust           |        [miden](https://github.com/0xPolygonMiden/miden-vm)        |
|          [mozak vm](https://github.com/0xmozak/mozak-vm)          |        RISC-V        |        :x:         |                :x:            |         :x:         |                |                Rust                 |               `AIR` (Starky)        | `Lookup`                                                                     |                          FRI                                              |      Rust           |          [mozak vm](https://github.com/0xmozak/mozak-vm)          |
|         [nexus](https://github.com/nexus-xyz/nexus-zkvm)          |        RISC-V        | :white_check_mark: |       :white_check_mark:     | :white_check_mark: |                |                Rust                 | Folded Accumulated Relaxed R1CS     | `Accumulated Folding`                                                        |                   Spartan + {Zeromorph, PSE-Halo2 (KZG)}                  |      Rust           |         [nexus](https://github.com/nexus-xyz/nexus-zkvm)          |
| [o1vm](https://github.com/o1-labs/proof-systems/tree/master/o1vm) |         MIPS         |        :x:         |                :x:            |         :x:         |                |                 Go                  |              Plonkish               | `Lookup`                                                                     |                          IPA                                              |      Rust           | [o1vm](https://github.com/o1-labs/proof-systems/tree/master/o1vm) |
|              [olavm](https://github.com/Sin7Y/olavm)              |     Ola Assembly     |        :x:         |                :x:            | :white_check_mark: |                |            Ola Assembly             |           `AIR` (plonky2)           | `Lookup`                                                                     |                          FRI                                              |      Rust           |              [olavm](https://github.com/Sin7Y/olavm)              |
|          [powdrVM](https://github.com/powdr-labs/powdr)           |        RISC-V        | :white_check_mark: |       :white_check_mark:     | :white_check_mark: |                |                ASM assembly         |        `AIR`-ish (PIL, plonky3)     |            -                                                                 | PSE-Halo2 (KZG), Plonky3, FRI([eSTARK](https://eprint.iacr.org/2023/474)) | Solidity (auto-gen) |           [powdrVM](https://github.com/powdr-labs/powdr)          |
|              [risc0](https://github.com/risc0/risc0)              |        RISC-V        | :white_check_mark: |       :white_check_mark:     | :white_check_mark: | :white_check_mark: |                Rust                 |             `PLONK`                 | `Plookup`                                                                    | [DEEP-FRI & ALI](https://eprint.iacr.org/2021/582.pdf) | Rust, Solidity   |     Rust, Solidity  | [risc0](https://github.com/risc0/risc0)                           |
|            [sp1](https://github.com/succinctlabs/sp1)             |        RISC-V        | :white_check_mark: |       :white_check_mark:     | :white_check_mark: | :white_check_mark: |                Rust                 |              `AIR` (plonky3)        |  `Lookup`                                                                    |                       FRI                                                 | Rust, Solidity      |            [sp1](https://github.com/succinctlabs/sp1)             |
|       [sphinx](https://github.com/argumentcomputer/sphinx)        |        RISC-V        | :white_check_mark: |       :white_check_mark:     | :white_check_mark: |                |                Rust, Lurk           |    `AIR` (core) `PLONK` (wrap)      | `Lookup`                                                                     |                       FRI                                                 |       Rust          |       [sphinx](https://github.com/argumentcomputer/sphinx)        |
|        [triton vm](https://github.com/TritonVM/triton-vm)         |   Triton Assembly    |        :x:         |                :x:            |         :x:         |                |           Triton Assembly           |                 `AIR`               | `Lookup` [`Contiguity`](https://triton-vm.org/spec/memory-consistency.html)  |                       FRI                                                 |       Rust          |         [triton vm](https://github.com/TritonVM/triton-vm)        |
|          [valida](https://github.com/lita-xyz/valida-releases)    |        Valida        |        :x:         |                :x:            |         :x:         |                |               Rust, C               |            `AIR` (plonky3)          |                                                                              |                       FRI                                                 |        ?            |          [valida](https://github.com/lita-xyz/valida-releases)    | 
|          [zisk](https://github.com/0xPolygonHermez/zisk)          |        RISC-V        |        :x:         |                :x:            |         :x:         |                |                 PIL                 |            ?                        |         ?                                                                    |         ?                                                                 |        ?            |          [zisk](https://github.com/0xPolygonHermez/zisk)          |
|               [zkm](https://github.com/zkMIPS/zkm)                |         MIPS         | :white_check_mark: |       :white_check_mark:     | :white_check_mark: |                |              Rust, Go               |          `AIR` (plonky2)            | `Lookup`                                                                     |                       FRI                                                 |       Rust          |               [zkm](https://github.com/zkMIPS/zkm)                |
|         [zkWasm](https://github.com/DelphinusLab/zkWasm)          |         Wasm         | :white_check_mark: |       :white_check_mark:     | :white_check_mark: |                | C, C++, rust, etc (wasm compilable) |          `PLONK`                    | -                                                                            |                       IPA?                                                |       Rust          |         [zkWasm](https://github.com/DelphinusLab/zkWasm)          |

## bench

- [benchmarks (lita)](https://lita.gitbook.io/lita-documentation/architecture/benchmarks) | [code](https://github.com/lita-xyz/benchmarks)
- [benchmark (risc0)](https://reports.risczero.com/benchmarks/Linux-cpu) | [code](https://github.com/risc0/risc0/tree/main/benchmarks)
- zkvm-benchmarks (a16z) | [code](https://github.com/a16z/zkvm-benchmarks)
- zkvm perf (succinct) | [code](https://github.com/succinctlabs/zkvm-perf)

## Independent/third-party Benchmarks
- Benchmarking of π<sup>2</sup> ZK Metamath checkers | [code](https://github.com/Pi-Squared-Inc/zk-benchmark), including [results](https://github.com/Pi-Squared-Inc/zk-benchmark?tab=readme-ov-file#our-results)
- definitive guide to zkVMs | [article](http://mirror.xyz/stackrlabs.eth/jEBSBZtKEiMiTrRIGMCxN7n6r7al-vi25lmrnD610W4)
- Lurk 0.5 Benchmarks | [article](https://argument.xyz/blog/perf-2024/)
- benchmark of zkVMs and proving schemes | [code](https://github.com/babybear-labs/benchmark)

## papers

### Cairo

- [Cairo – a Turing-complete STARK-friendly CPU architecture](https://eprint.iacr.org/2021/1063.pdf)
- [A Verified Algebraic Representation of Cairo Program Execution](https://dl.acm.org/doi/pdf/10.1145/3497775.3503675)
- [A Proof-Producing Compiler for Blockchain Applications](https://drops.dagstuhl.de/storage/00lipics/lipics-vol268-itp2023/LIPIcs.ITP.2023.7/LIPIcs.ITP.2023.7.pdf)

### Ceno

- [Ceno: Non-uniform, Segment and Parallel Zero-knowledge Virtual Machine](https://eprint.iacr.org/2024/387.pdf)

### Jolt

- [Jolt: SNARKs for Virtual Machines via Lookups](https://eprint.iacr.org/2023/1217.pdf)

### SP1

- [SP1 V4 Turbo: Memory Argument via Elliptic Curve based Multiset Hashing](https://github.com/succinctlabs/sp1/blob/5c8a50e08b48d22b88471f39f9cc45947ca3bf5c/book/static/SP1_Turbo_Memory_Argument.pdf)
- [Succint Network: Prove the World's software](https://www.provewith.us/)

### Risc Zero

- [RISC Zero zkVM: Scalable, Transparent Arguments of RISC-V Integrity](https://dev.risczero.com/proof-system-in-detail.pdf)

### EDEN

- [EDEN](https://eprint.iacr.org/2023/1021.pdf)

## resources

- [a zero-knowledge paradigm series](https://www.lita.foundation/blog/zero-knowledge-paradigm-zkvm)
- [cairo – a turing-complete stark-friendly cpu architecture - shahar papini](https://www.youtube.com/watch?v=vVgHL5vpJxY&t=33s)
- [lasso + jolt playlist](https://youtube.com/playlist?list=PLjQ9HCQMu_8xjOEM_vh5p26ODtr-mmGxO&si=Uega8IMg_J8kNaa8)
- [new paradigm in ethereum l2 scaling: multi-proving and zk-vms](https://www.mikkoikola.com/blog/2023/12/11/new-paradigm-in-ethereum-l2-scaling-multi-proving-and-zk-vms)
- [the nexus v1.0 zkvm - daniel marin (nexus)](https://www.youtube.com/watch?v=UtzFOwQp8n4)
- [understanding jolt: clarifications and reflections](https://a16zcrypto.com/posts/article/understanding-jolt-clarifications-and-reflections/)
- [zk whiteboard sessions – module seven: zero knowledge virtual machines (zkvm) with grjte](https://www.youtube.com/watch?v=GRFPGJW0hic)
- [zk10: analysis of zkvm designs - wei dai & terry chung](https://www.youtube.com/watch?v=tWJZX-WmbeY&t=325s)
- [zk11: o1vm: building a real-world zkvm for mips - danny willems](https://www.youtube.com/watch?v=HDH2KXRAxAc)
- [zk12: memory checking in ivc-based zkvm - jens groth](https://www.youtube.com/watch?v=kzSYNFh4uQ0&list=PLothk45x3HC9Oz4f3e9-OoYUEytfHWCl5)
- [zk7: miden vm: a stark-friendly vm for blockchains - bobbin threadbare – polygon](https://www.youtube.com/watch?v=81UAaiIgIYA&t=803s)
- [zeroing into zkvm](https://taiko.mirror.xyz/e_5GeGGFJIrOxqvXOfzY6HmWcRjCjRyG0NQF1zbNpNQ)
- [zkvm design walkthrough with max and daniel](https://www.youtube.com/watch?v=aobrJ-zTcAU)
- [Verification of zkWasm in Coq](https://github.com/CertiKProject/zkwasm-fv)
- [zk11: polynomial acceleration for stark vms](https://www.youtube.com/watch?v=R07ina4k7hg)
- [what does risc v have to do with risc zero's zkvm](https://www.youtube.com/watch?v=11DIflEwx50)
- [risc zero architecture presentation @ stanford](https://www.youtube.com/watch?v=RtGk6967PC4)
- [continuations: scaling in zkvm](https://www.youtube.com/watch?v=h1qWnf-M5lo)
- [Getting the bugs out of SNARKs: The road ahead](https://a16zcrypto.com/posts/article/getting-bugs-out-of-snarks/)
- [~tacryt-socryp on Zorp, the Nock zkVM | Reassembly23](https://www.youtube.com/watch?v=zD45V6GAD00)
- [Automatic Circuit Acceleration of Guest Programs](https://www.powdr.org/blog/auto-acc-circuits)
- [Introducing Twist and Shout](https://a16zcrypto.com/posts/article/introducing-twist-and-shout/)

## tutorials / educational zkVM

- [brainfuck tutorial](https://neptune.cash/learn/brainfuck-tutorial/)
- [chip0](https://github.com/shuklaayush/chip0)
- [continuous read only memory constraints an implementation using lambdaworks](https://blog.lambdaclass.com/continuous-read-only-memory-constraints-an-implementation-using-lambdaworks/)
- [fri from scratch](https://blog.lambdaclass.com/how-to-code-fri-from-scratch/)
- [stark by hand](https://dev.risczero.com/proof-system/stark-by-hand)
- [stark brainfuck](https://aszepieniec.github.io/stark-brainfuck/)
- [stark 101](https://starkware.co/stark-101/)
- [stwo brainfuck](https://github.com/kkrt-labs/stwo-brainfuck)
- [useless zkvm](https://github.com/armanthepythonguy/Useless-ZKVM)
- [Anatomy of a STARK](https://aszepieniec.github.io/stark-anatomy/)
  
## related tooling

- [zkRust](https://github.com/yetanotherco/zkRust)
- [any zkVM](https://github.com/MatteoMer/any-zkvm)

## related awesome lists

- [awesome risc0](https://github.com/inversebrah/awesome-risc0)
- [awesome sp1](https://github.com/gakonst/awesome-sp1)
- [awesome starknet #cryptography-and-maths](https://github.com/keep-starknet-strange/awesome-starknet?tab=readme-ov-file#cryptography-and-maths)
- [awesome zkp](https://github.com/matter-labs/awesome-zero-knowledge-proofs)
- [awesome miden](https://github.com/phklive/awesome-miden)
- [awesome plonky3](https://github.com/Plonky3/awesome-plonky3)
- [awesome stwo](https://github.com/keep-starknet-strange/awesome-stwo)
