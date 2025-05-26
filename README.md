#PIPELINED PROCESSOR

*Company name*: CODTECH IT SOLUTIONS

*NAME*: KANNAJOSYULA HARSHA VARDHAN

*INTERN ID*: CT06DN42

*DOMAIN*: VLSI

*DURATION*: 6 WEEKS

*MENTOR*: NEELA SANTOSH

***TASK-3***

# 4-Stage Pipelined RISC-V Processor

## Overview

This project implements a 4-stage pipelined RISC-V processor that demonstrates fundamental concepts of instruction pipelining, memory hierarchy, and processor design. The implementation is optimized for educational use and FPGA deployment using Xilinx Vivado 2023.3.

### Key Features

✅ 4-stage pipeline architecture (IF → ID → EX → WB)  
✅ Harvard architecture with separate instruction/data memories  
✅ Support for essential RISC-V instructions  
✅ Comprehensive testbench with automated verification  
✅ FPGA-ready Verilog implementation  
✅ Educational-focused design with clear documentation  

---

## Architecture

### Pipeline Stages

| Stage | Name | Function |
|-------|------|----------|
| IF    | Instruction Fetch | Fetch instructions from memory using PC |
| ID    | Instruction Decode | Decode instructions and read registers |
| EX    | Execute | Perform ALU operations and memory calculations |
| WB    | Writeback | Write results back to register file |

### Core Components

🔧 **Processor Core** (`pipelined_processor`):
- 32-bit Program Counter (PC)
- Pipeline registers for inter-stage data flow
- 32 × 32-bit register file (r0-r31)
- ALU with arithmetic and logical operations
- Control unit for instruction decoding

💾 **Memory Subsystem**:
- Instruction Memory: 1024 × 32-bit words
- Data Memory: 1024 × 32-bit words with test data
- Word-addressed memory access

---

## Supported Instructions

### R-Type Instructions (Register-Register)

```assembly
ADD  rd, rs1, rs2    # rd = rs1 + rs2
SUB  rd, rs1, rs2    # rd = rs1 - rs2
AND  rd, rs1, rs2    # rd = rs1 & rs2
OR   rd, rs1, rs2    # rd = rs1 | rs2
XOR  rd, rs1, rs2    # rd = rs1 ^ rs2
```

### I-Type Instructions (Immediate)

```assembly
LOAD rd, imm(rs1)    # rd = memory[rs1 + imm]
```

---

## Pipeline Stages Diagram

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│     IF      │    │     ID      │    │     EX      │    │     WB      │
│ Fetch Inst  │────│ Decode &    │────│ ALU & Mem   │────│ Write Back  │
│ Update PC   │    │ Read Regs   │    │ Address     │    │ to RegFile  │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
       │                  │                  │                  │
   IF/ID Reg          ID/EX Reg          EX/WB Reg        Register File
```

---

## Memory Organization

### Instruction Memory Layout

| Address | Instruction | Assembly | Description |
|---------|-------------|----------|-------------|
| 0x00    | 0x00000033  | ADD r1, r0, r0 | r1 = 0 + 0 |
| 0x04    | 0x00002103  | LOAD r2, 0(r0) | r2 = mem[0] |
| 0x08    | 0x002081B3  | ADD r3, r1, r2 | r3 = r1 + r2 |
| 0x0C    | 0x40109233  | SUB r4, r3, r1 | r4 = r3 - r1 |
| 0x10    | 0x0021F2B3  | AND r5, r3, r2 | r5 = r3 & r2 |

### Data Memory Initialization

| Address | Value      | Description              |
|---------|------------|--------------------------|
| 0x00    | 0x12345678 | Test data for LOAD       |
| 0x04    | 0xABCDEF00 | Additional test data     |
| 0x08    | 0x87654321 | Additional test data     |
| 0x0C    | 0xDEADBEEF | Additional test data     |

---

## Sample Program

```assembly
# Sample RISC-V Assembly Program
ADD  r1, r0, r0      # r1 = 0 (r0 is hardwired to 0)
LOAD r2, 0(r0)       # r2 = memory[0] = 0x12345678
ADD  r3, r1, r2      # r3 = 0 + 0x12345678 = 0x12345678
SUB  r4, r3, r1      # r4 = 0x12345678 - 0 = 0x12345678
AND  r5, r3, r2      # r5 = 0x12345678 & 0x12345678 = 0x12345678
```

### Expected Results

```
r1 = 0x00000000
r2 = 0x12345678
r3 = 0x12345678
r4 = 0x12345678
r5 = 0x12345678
```

---

## Getting Started

### Prerequisites

- Xilinx Vivado 2023.3 or compatible version
- Verilog simulator (ModelSim, Vivado Simulator, etc.)
- Basic knowledge of digital design and computer architecture

### Quick Start

```bash
# Clone the repository:
git clone https://github.com/yourusername/pipelined-risc-v-processor.git
cd pipelined-risc-v-processor
```

```bash
# Compile and simulate:
xvlog pipelined_processor.v
xelab tb_pipelined_processor -debug typical
xsim tb_pipelined_processor -gui
```

### View results

- Check console output for test results
- Open `pipelined_processor.vcd` for waveform analysis

---

## Synthesis for FPGA

```tcl
# Vivado TCL commands
create_project pipelined_processor ./project -part xc7a35tcpg236-1
add_files {pipelined_processor.v}
launch_runs synth_1
wait_on_run synth_1
```

---

## Simulation Results

### Sample Console Output

```
=== Starting Pipelined Processor Test ===
Reset released at time: 25
Cycle 1: PC=00000000 | IF/ID_Inst=00000033 | WB: r1=00000000 (en=1)
Cycle 2: PC=00000004 | IF/ID_Inst=00002103 | WB: r2=12345678 (en=1)
...
=== Verifying Results ===
PASS: r1 (ADD r0, r0) - Expected: 00000000, Got: 00000000
PASS: r2 (LOAD from mem[0]) - Expected: 12345678, Got: 12345678
...
*** ALL TESTS PASSED ***
```

### Performance Metrics

- Pipeline Latency: 4 clock cycles
- Throughput: 1 instruction per clock cycle (after pipeline fill)
- Clock Frequency: Up to 100 MHz on Artix-7 FPGA

---

## File Structure

```
📦 pipelined-risc-v-processor/
├── 📄 pipelined_processor.v      # Main processor implementation
├── 📄 tb_pipelined_processor.v   # Comprehensive testbench
├── 📄 README.md                  # This file
├── 📄 LICENSE                    # License information
└── 📁 docs/                      # Additional documentation
    ├── 📄 architecture.md        # Detailed architecture description
    ├── 📄 instruction_set.md     # Instruction set reference
    └── 📄 simulation_guide.md    # Simulation tutorial
```

---

## Educational Value

This project is ideal for learning:

🎓 **Computer Architecture Concepts**
- Pipeline design and implementation
- Instruction set architecture (ISA)
- Memory hierarchy and organization
- Datapath and control unit design

💻 **Digital Design Skills**
- Verilog HDL programming
- Testbench development and verification
- FPGA implementation techniques
- Timing analysis and optimization

🔬 **Verification Methodology**
- Self-checking testbenches
- Automated testing procedures
- Waveform analysis and debugging
- Performance measurement techniques

---

## Contributing

We welcome contributions! Please feel free to:

🐛 Report bugs - Open an issue with detailed description  
💡 Suggest features - Propose new instructions or optimizations  
📖 Improve documentation - Help make the code more accessible  
🔧 Submit pull requests - Follow our coding standards  

### Development Guidelines

- Follow Verilog coding standards
- Include comprehensive test cases
- Update documentation for new features
- Ensure FPGA synthesis compatibility
