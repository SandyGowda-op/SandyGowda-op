<!-- ===================================================================
     Profile README for github.com/SandyGowda-op
     Replace every {{PLACEHOLDER}} before publishing (search for "{{").
     Everything here renders natively on GitHub except the typing banner
     and view counter, which are images from free public services.
     =================================================================== -->

<h1 align="center">Hi, I'm Sandesh R Gowda 👋</h1>

<p align="center">
  <a href="https://github.com/SandyGowda-op">
    <img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=600&size=20&duration=3000&pause=800&color=2F81F7&center=true&vCenter=true&width=640&lines=RTL+design+%26+verification;RISC-V+pipelines+%E2%80%A2+ML+accelerators;Dense+%2B+2%3A4-sparse+matrix+engines;Bugs+found+%E2%86%92+fixed+%E2%86%92+negative-controlled" alt="typing banner" />
  </a>
</p>

<p align="center">
  <a href="{{LINKEDIN_URL}}"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" /></a>
  <a href="mailto:{{EMAIL}}"><img src="https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white" /></a>
  <a href="{{RESUME_URL}}"><img src="https://img.shields.io/badge/Résumé-111111?style=for-the-badge&logo=readdotcv&logoColor=white" /></a>
  <img src="https://komarev.com/ghpvc/?username=SandyGowda-op&style=for-the-badge&color=2F81F7&label=profile+views" />
</p>

---

### 🧠 About me

- 🎓 B.E in EEE at **BMSCE** 2027
- 🔧 I build and verify digital hardware for machine learning: RISC-V CPUs, matrix accelerators, and the CPU↔accelerator interface between them
- 🔬 What I care about: proving a fix works by showing the test **fails without it** (negative controls)
- 🌱 Currently: turning NeuroCore into a small NPU: result write-back, K-tile accumulation, and a compiler that picks dense vs 2:4-sparse per tile
- 🎯 Interested in: RTL design / design verification internships
- 📍 BANGALORE, India

---

### 🚀 Featured: NeuroCore

> A RISC-V CPU driving a descriptor-based accelerator that runs **dense 8×8** and **2:4-sparse** matrix jobs.

<table>
<tr>
<td width="50%" valign="top">

**What it is**
- 5-stage RV32I pipeline + 4 custom-0 instructions
- Descriptor fetch → DMA → scratchpad → compute
- Dense and 2:4-sparse paths behind one interface
- Hardware trusts software's sparsity layout → tiny control logic

</td>
<td width="50%" valign="top">

**What I measured**
- **9** CPU–accelerator coupling hazards found & fixed
- **3** negative controls that fail without their fix
- **23 / 23** regression checks, **50 / 50** random seeds
- Sparse vs dense: **1.80×** faster compute, **1.35×** per job
- Dispatch control ≈ **1.3 %** of accelerator area (Nangate45)

</td>
</tr>
</table>

<p align="center">
  <a href="https://github.com/SandyGowda-op/neurocore"><img src="https://img.shields.io/badge/Open_NeuroCore-2F81F7?style=for-the-badge&logo=github&logoColor=white" /></a>
  <a href="https://github.com/SandyGowda-op/neurocore/actions"><img src="https://github.com/SandyGowda-op/neurocore/actions/workflows/regression.yml/badge.svg" /></a>
</p>

<details>
<summary><b>🔍 Click to see how the CPU talks to the accelerator</b></summary>

```mermaid
sequenceDiagram
    participant CPU as RISC-V CPU
    participant DM as Descriptor memory
    participant ACC as Accelerator
    CPU->>DM: SW ×8 (build descriptor)
    CPU->>ACC: FMAC_SETDESC idx
    CPU->>ACC: FMAC_START
    Note over CPU: stores stall while<br/>descriptor is fetched
    ACC->>DM: fetch 8 words
    ACC->>ACC: DMA → scratchpad → dense / 2:4 compute
    CPU->>ACC: FMAC_READ (stalls until done)
    ACC-->>CPU: result element (64-bit, two halves)
```

</details>

<details>
<summary><b>🐞 Click to see the hazards I found</b></summary>

| Hazard | Fix | Proven by |
|---|---|---|
| 2nd `START` while busy is silently dropped | stall `START` | T1, H7 |
| ALU / `FMAC_READ` result → `BEQ` reads stale value | 1-cycle stall | T2, T3 |
| Invalid descriptor hangs the accelerator | error path + status read | T4 |
| 64-bit result truncated to 32 bits | high-word read | T5 |
| Read right after `START` returns old job's data | hold read | H2 |
| Store data not forwarded | forward into store path | T6b + negative control |
| Distance-3 read-after-write misses write-back | register-file bypass | T7 + negative control |
| Store right after `START` corrupts the descriptor | store guard | T6c + negative control |
| Pipeline registers missing in synthesis | remove `ifndef SYNTHESIS` | synthesis review |

</details>

---

### 🗺️ My hardware journey

```mermaid
flowchart LR
    A["<b>riscv-ai-accelerator</b><br/>5-stage RV32I<br/>forwarding · hazards"] --> B["<b>tpu_research</b><br/>systolic-array GEMM<br/>CocoTB + NumPy"]
    B --> C["<b>NeuroCore</b><br/>CPU + dense / 2:4-sparse<br/>accelerator"]
    C --> D["<b>Next</b><br/>compiler · tiny NN<br/>on NeuroCore"]
    click A "https://github.com/SandyGowda-op/riscv-ai-accelerator"
    click B "https://github.com/SandyGowda-op/tpu_research"
    click C "https://github.com/SandyGowda-op/neurocore"
    style C fill:#2F81F7,color:#fff
    style D stroke-dasharray: 5 5
```

| Repo | One line | |
|---|---|---|
| [**neurocore**](https://github.com/SandyGowda-op/neurocore) | CPU + descriptor-driven dense / 2:4-sparse accelerator, hazard study | ![](https://img.shields.io/badge/SystemVerilog-flagship-2F81F7) |
| [riscv-ai-accelerator](https://github.com/SandyGowda-op/riscv-ai-accelerator) | 5-stage RV32I pipeline with a coprocessor interface | ![](https://img.shields.io/badge/Verilog-CPU-555) |
| [neurocore-accel](https://github.com/SandyGowda-op/neurocore-accel) | The accelerator on its own, with unit testbenches | ![](https://img.shields.io/badge/SystemVerilog-IP-555) |
| [tpu_research](https://github.com/SandyGowda-op/tpu_research) | Parameterised systolic-array GEMM study | ![](https://img.shields.io/badge/Verilog-research-555) |

---

### 🛠️ Toolbox

<p>
  <img src="https://img.shields.io/badge/SystemVerilog-1E3A8A?style=flat-square" />
  <img src="https://img.shields.io/badge/Verilog-1E3A8A?style=flat-square" />
  <img src="https://img.shields.io/badge/RISC--V-283272?style=flat-square&logo=riscv&logoColor=white" />
  <img src="https://img.shields.io/badge/Icarus_Verilog-444?style=flat-square" />
  <img src="https://img.shields.io/badge/Yosys-444?style=flat-square" />
  <img src="https://img.shields.io/badge/OpenSTA-444?style=flat-square" />
  <img src="https://img.shields.io/badge/CocoTB-444?style=flat-square" />
  <img src="https://img.shields.io/badge/Vivado-444?style=flat-square&logo=xilinx&logoColor=white" />
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white" />
  <img src="https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white" />
  <img src="https://img.shields.io/badge/Linux_/_WSL-FCC624?style=flat-square&logo=linux&logoColor=black" />
</p>

---

<details>
<summary><b>💬 Ask me about…</b></summary>

- why a branch resolved in ID needs a stall even with forwarding
- how a negative control proves a test can actually see a bug
- why a 1.80× compute speed-up becomes 1.35× end to end
- why one `always_comb` made every simulation compile take 2.5 minutes

</details>

<p align="center"><sub>Built with plain Markdown · last updated {{MONTH_YEAR}}</sub></p>
