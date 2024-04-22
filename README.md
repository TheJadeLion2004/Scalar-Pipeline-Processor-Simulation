# Scalar-Pipeline-Processor-Simulation
This is a C++ code that simulates a Scalar Pipeline Processor at the software level. For any given assembly code in the specified format, this returns the number of various instructions that are executed and the various types of hazards encountered. It also returns the average CPI.

## Execution Steps:
We take 3 files as the input -  DCache.txt, ICache.txt, RF.txt

Then 

1)Enter the instructions for the processor in the ICache.txt file. Each instruction is spread across 2 lines - a byte for each line.

2)Enter the appropriate intial values in the DCache.txt and RF.txt files.

3)Run the main.cpp file, which will generate two files-

a)Output.txt : Contains stats for processor b)ODCache.txt : Contains final DCache config

## Description
- We consider  a  scalar  pipelined  processor  with  a  256B  instruction  cache  (I$)  and  a  256B  data  cache  (D$),  bothhaving a read port and a write port each, and both are direct-mapped caches.
- Assume that both instructionand data caches are perfect, which means there won’t be any cache misses in these caches.
- Assume the processorhas a register file (RF) with sixteen 8-bit registers named R0, ..., R15.
- The register R0 always stores the value‘0’.
- The register file has two read ports and a write port.
- Negative numbers are stored in 2’s complement form

### Instructions Supported:
Arithmetic
- ADD rd rs1 rs2      # Addition: rd←rs1 + rs2
- SUB rd rs1 rs2      # Subtraction: rd←rs1
- rs2–MUL rd rs1 rs2  # Multiplication: rd←rs1 * rs2
- INC r               # Increment: r←r + 1

Logical
- AND rd rs1 rs2      # Bit-wise AND: rd←rs1&rs2
- OR rd rs1 rs2       # Bit-wise OR: rd←rs1 | rs2
- –XOR rd rs1 rs2     # Bit-wise XOR: rd←rs1⊕rs2
- –NOT rd rs          # Bit-wise NOT: rd← ∼rs

Shift Instructions
- SLLI rd rs1 imm(4)  # Shift Logical Left Immediate: rd←rs1 << imm(4)
- SRLI rd rs1 imm(4)  # Shift Logical Right Immediate: rd←rs1 >> imm(4)

Memory Instructions
- LI rd imm(8)        # Load Immediate: rd←imm(8)
- LD rd rs1 imm(4)    # Load Memory: rd←[rs1 + imm(4)]
- ST rd rs1 imm(4)    # Store Memory: [rs1 + imm(4)]←rd

### Pipeline 
We consider a five-stage instruction pipeline: IF-ID-EX-MEM-WB. For store instruction, the WB stage remainsidle.  For ALU instructions, the MEM stage remains idle.  The processor is pipelined at the instruction level.The instructions are to be of a fixed length of 2 bytes each.

### Pipelining Hazards
We consider data hazards and control hazards
  1. RAW Hazards
     Consider the instruction sequence given below
        AND R1 R2 R3
        SUB R4 R1 R5
     The content of R1, produced by the ADD instruction, is required for the SUB instruction to proceed.
  2. Control Hazards
     Arise  from  pipelining  of  branches  and  other  instructions  that  change  the  Program  Counter  (PC).  For example, in a conditional Jump instruction, till the condition is evaluated, the new PC can take either the incremented PC value or the address accessed in that instruction.  To avoid this, we stall the pipelinefor two cycles. 
