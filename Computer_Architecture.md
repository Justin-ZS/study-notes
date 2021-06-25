# Computer Architecture

## Resources
1. [BiliBili Videos](https://www.bilibili.com/video/BV1PT4y1M7gM)

## Goals
1. Understand how a computing platform (processor + memory + interconnect) works 
1. Understand how decisions made in hardware affect the software/programmer as well as hardware designer
1. Think critically (in solving problems)
1. Think broadly across the levels of transformation
1. Understand how to analyze and make tradeoffs in design

## Introduce and Basic
1. The Power of Abstraction
    * Levels of transformation create abstractions
    * Abstraction improves productivity
1. Crossing the Abstraction Layers
    * Understand how a lower(*processor*) level works and how decisions made in it affect the higher level.
    * Breaking the abstraction layers and knowing what is underneath enables you to solve problems
    * Able to be comfortable in making design and optimization decisions that cross the boundaries of different layers and system components
    * Cooperation between multiple components and layers can enable more effective solutions and systems
1. Examples
    * Unexpected Slowdowns in Multi-Core: “*Memory performance attacks*“
    * DRAM Refresh
    * Row Hammer

## Fundamental Concepts and ISA
1. Computer Architecture 
    1. Problem
    1. Algorithm
    1. Program/Language
    1. Runtime System(VM, OS, MM)
    1. ISA (Architecture)
    1. Microarchitecture
    1. Logic
    1. Circuits
    1. Electrons
1. What is A Computer?
    1. Computation
    1. Communication
    1. Storage (memory)
1. The Von Neumann Model  
An instruction is fetched and executed in control flow order 
    1. Stored program
        * Instructions stored in a linear memory array
        * Memory is unified between instructions and data
    1. Sequential instruction processing
        * One instruction processed (fetched, executed, and completed) at a time
        * Program counter (instruction pointer) identifies the current instr
        * Program counter is advanced sequentially (except for control transfer instructions)
1. ISA-level Tradeoff: **Instruction Pointer**
    * Yes: Control-driven, sequential execution
        1. An instruction is executed when the IP points to it
      1. IP automatically changes sequentially (except for control flow instructions)
    * No: Data-driven, parallel execution
      1. An instruction is executed when all its operand values are available (data flow)
1. ISA vs. Microarchitecture
    1. ISA
        * Agreed upon interface between software and hardware
        * What the software writer needs to know to write and debug system/user programs 
    1. Microarchitecture(*uarch*)
        * Specific implementation of an ISA
        * Not visible to the software
    1. Microprocessor
        * ISA, uarch, circuits
        * Architecture = ISA + microarchitecture

## ISA Tradeoffs
### ISA = Instruction + Data types + Registers + Memory
1. Instruction
    * **opcode**: what the instruction does
    * **operands**: who it is to do it to
1. Data types
    * **Low-level**: Integer, floating point, character, binary, decimal
    * **High-level**: Doubly linked list, queue, string, bit vector, stack
1. Registers
    * **Why?**: A recently produced/accessed value is likely to be used more than once
    * How many and size of each register ?
1. Memory organization
    * **Address space**: How many uniquely identifiable locations in memory
    * **Addressability**: How much data does each uniquely identifiable location store
        1. How do you add 2 32-bit numbers with only byte addressability?
        1. Big endian vs. little endian

### Instruction Tradeoff  
1. The number of operands an instruction operates on
    1. **0-address**: stack machine (op, push A, pop A)
    1. **1-address**: accumulator machine (op ACC, ld A, st A)
    1. **2-address**: 2-operand machine (op S,D; one is both source and dest)
    1. **3-address**: 3-operand machine (op S1,S2,D; source and dest separate)
1. Addressing modes: how to obtain the operands
    1. **Absolute**: value as address. `LW rt, 10000`
    1. **Register Indirect**: GPR[rbase] as address. `LW rt, (rbase)`
    1. **Memory Indirect**: M[GPR[rbase]] as address. `LW rt ((rbase))`
    1. More modes
        * Better support programming constructs
        * Harder to design
        * Too many choice for the compiler
1. Instruction length
    1. **Fixed length**: same length of all instructions 
        * Easier to decode in hardware and concurrently
        * Wasted bites
        * Harder to extend
    1. **Variable length**: different length of instructions determined by opcode
        * Compact encoding
        * Harder to decode concurrently
1. Uniform Decode
    1. **Uniform decode**: same bits in instructions have the same meaning
        * Opcode and operand are always in the same location
        * Easier to decode, simpler hardware
        * Enable parallelism
        * Restrict format and wastes space
    1. **Non-uniform decode**
        * More compact and powerful format
        * More complex decode logic 

### Data Type Tradeoffs
1. Data types coupled tightly to the semantic level, or complexity of instructions
    1. **CISC**: Closer to high-level language -> Small semantic gap
    1. **RISC**: Closer to hardware control signals -> Large semantic gap

### Resister Tradeoffs
1. Numbers of Registers
    * Affects
        * Number of bits for encoding resister address
        * Number of values kept
        * Size, access time, power consumption(uarch)
    * Large number of registers:
        * Better allocation
        * Larger instruction size
        * Larger register file size

### RISC VS CISC
1. RISC
    * Simple instructions
    * Fixed length
    * Uniform decode
    * Few addressing mode
1. CISC
    * Complex instructions
    * Variable length
    * Non-uniform decode
    * Many addressing modes

## MIPS
### State
1. Program Counter(PC): memory address of current instruction
1. Memory: 32-bit address(4GB)
1. General Purpose Register File(GPRF): r0 ... r31
### Endian
1. Big Endian: pointer -> byte0(MSB) - byte3(LSB)
1. Little Endian: pointer -> byte0(LSB) - byte3(MSB)
### Instruction Formats
1. 3 formats
    1. R-type: 3 register operands
    1. I-type: 2 register operands and 16-bit immediate
    1. J-type: 26-bit immediate operand
1. Decoding
    1. 4 bytes per instruction
    1. 4-byte aligned
### Instructions
1. register-register signed addition
    1. ADD rd_reg rs_reg rt_reg
    1. R-type: `[0, rs, rt, rd, 0, ADD]`
    1. Semantics
        * GPR[rd] <- GPR[rs] + GPR[rt]
        * PC <- PC + 4
1. regi-immediate signed additions
    1. ADDI rt_reg rs_reg immediate_16
    1. I-type: `[ADDI, rs, rt, immediate]`
    1. Semantics
        * GPR[rt] <- GPR[rs] + signed-extend(immediate)
        * PC <- PC + 4
1. load 4-byte word
    1. LW rt_reg offset_16 (base_reg)
    1. I-type: `[LW, base, rt, offset]`
    1. Semantics
        * address <- signed-extend(offset) + GPR[base]
        * GPR[rt] <- MEMORY[address]
        * PC <- PC + 4
1. store 4-byte word
    1. SW rt_reg offset_16 (base_reg)
    1. I-type: `[SW, base, rt, offset]`
    1. Semantics
        * address <- signed-extend(offset) + GPR[base]
        * MEMORY[address] <- GPR[rt]
        * PC <- PC + 4
1. branch if equal
    1. BEQ rs_reg rt_reg immediate_16
    1. I-type: `[BEQ, rs, rt, immediate]`
    1. Semantics
        * target = PC + signed-extend(immediate) * 4
        * if GPR[rs] == GPR[rt], then PC <- target
        * else PC <- PC + 4
1. jump
    1. J immediate_26
    1. J-type: `[J, immediate]`
    1. Semantics
        * target = PC[31:28]*2^28 | zero-extend(immediate) * 4
        * PC <- target
### Caller and Callee Saved Registers
1. Callee-Saved Registers
    * Caller: The values of these registers should not change when you return to me
    * Callee: I promise to save the old values to memory first and restore them before I return to you
1. Caller-Saved Registers
    * Caller: I already saved the values from these registers
    * Callee: Don’t count on them staying the same values after I am done
        