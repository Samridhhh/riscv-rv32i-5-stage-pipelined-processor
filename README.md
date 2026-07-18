# RISC-V RV32I 5-Stage Pipelined Processor

A Verilog implementation of a **32-bit RISC-V RV32I 5-stage pipelined processor** featuring hazard detection, data forwarding, branch prediction, and separate instruction/data memories. The processor is designed following the standard IF–ID–EX–MEM–WB pipeline architecture and emphasizes modular RTL design.


Project File Link https://drive.google.com/file/d/1wbpdVeCOtrTf-Kd_ullscAjK7d-vLxt8/view?usp=sharing

 
---

## Features

- RV32I Base Integer Instruction Set
- 5-Stage Pipeline
  - Instruction Fetch (IF)
  - Instruction Decode (ID)
  - Execute (EX)
  - Memory Access (MEM)
  - Write Back (WB)
- Hazard Detection Unit
- Data Forwarding Unit
- Load-Use Hazard Handling
- Dynamic Branch Prediction (2-bit Saturating Counter)
- Pipeline Registers
  - IF/ID
  - ID/EX
  - EX/MEM
  - MEM/WB
- Separate Instruction and Data Memories (Harvard Architecture)
- Modular RTL Design
- Fully synthesizable Verilog

---

## Supported Instruction Set (RV32I)

### R-Type
- ADD
- SUB
- SLL
- SLT
- SLTU
- XOR
- SRL
- SRA
- OR
- AND

### I-Type
- ADDI
- SLTI
- SLTIU
- XORI
- ORI
- ANDI
- SLLI
- SRLI
- SRAI

### Load Instructions
- LB
- LH
- LW
- LBU
- LHU

### Store Instructions
- SB
- SH
- SW

### Branch Instructions
- BEQ
- BNE
- BLT
- BGE
- BLTU
- BGEU

### Jump Instructions
- JAL
- JALR

### Upper Immediate Instructions
- LUI
- AUIPC

---

## Processor Architecture

```
          +----------------------+
          |      PC Register      |
          +----------+-----------+
                     |
                     v
          +----------------------+
          | Instruction Memory   |
          +----------+-----------+
                     |
                  IF/ID
                     |
                     v
      +------------------------------+
      | Control Unit                 |
      | Register File                |
      | Immediate Generator          |
      | Hazard Detection Unit        |
      +--------------+---------------+
                     |
                  ID/EX
                     |
                     v
          +----------------------+
          | ALU                  |
          | Forwarding Unit      |
          | Branch Logic         |
          +----------+-----------+
                     |
                  EX/MEM
                     |
                     v
          +----------------------+
          | Data Memory          |
          +----------+-----------+
                     |
                 MEM/WB
                     |
                     v
          +----------------------+
          | Write Back MUX       |
          +----------+-----------+
                     |
                     v
               Register File
```

---

## Hazard Handling

### Data Hazards

Implemented using:

- EX/MEM Forwarding
- MEM/WB Forwarding
- Load-Use Hazard Detection
- Pipeline Stall
- Execute Stage Flush

### Structural Hazards

Eliminated using separate Instruction Memory and Data Memory.

### Control Hazards

Resolved using:

- Dynamic Branch Prediction
- 2-bit Saturating Counter
- Pipeline Flush
- Correct PC Redirection

---

## Branch Prediction

The processor uses a **2-bit saturating counter predictor**.

States:

```
00 : Strongly Not Taken
01 : Weakly Not Taken
10 : Weakly Taken
11 : Strongly Taken
```

Prediction is updated after the actual branch outcome is known.

---

## Project Structure

```
.
├── Top Module
├── PC
├── Instruction Memory
├── Register File
├── Control Unit
├── Immediate Generator
├── ALU
├── ALU Decoder
├── Branch Decision Unit
├── Data Memory
├── Hazard Control Unit
├── IF_ID Register
├── ID_EX Register
├── EX_MEM Register
├── MEM_WB Register
├── Multiplexers
└── Testbench
```

---

## Pipeline Registers

| Register | Purpose |
|----------|---------|
| IF/ID | Stores fetched instruction and PC |
| ID/EX | Stores decoded operands and control signals |
| EX/MEM | Stores ALU result and memory control signals |
| MEM/WB | Stores memory/ALU result for write-back |

---

## Forwarding Logic

Priority:

```
EX/MEM
   ↓
MEM/WB
   ↓
Register File
```

The latest available value is always forwarded to the Execute stage to minimize stalls.

---

## Load-Use Hazard

Example:

```
LW   x5,0(x1)
ADD  x6,x5,x2
```

Since load data becomes available after the Memory stage, the processor inserts:

- 1 Pipeline Bubble
- Stall Fetch
- Stall Decode
- Flush Execute

---

## Tools Used

- Verilog HDL
- Xilinx Vivado / ModelSim (Simulation)
- GTKWave (Waveform Analysis)

---

## Future Improvements

- Instruction Cache
- Data Cache
- Static Branch Prediction
- Branch Target Buffer (BTB)
- Return Address Stack
- RISC-V M Extension
- CSR Support
- Exception & Interrupt Handling
- AXI/AHB Bus Interface

---

## Learning Outcomes

This project demonstrates understanding of:

- Computer Architecture
- RTL Design
- Pipeline Design
- Hazard Detection
- Data Forwarding
- Branch Prediction
- Verilog HDL
- Digital System Design
- RISC-V ISA

---

## Author

**Samridh Shukla**

Electronics & Instrumentation Engineering  
National Institute of Technology Rourkela

---

## License

This project is intended for educational and learning purposes.
