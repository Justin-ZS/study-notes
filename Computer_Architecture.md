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
    * How many and size of each register?
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

## [MIPS](https://course.ece.cmu.edu/~ece447/s15/lib/exe/fetch.php?media=mips_reference_data.pdf)
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
        
## Microarchitecture Basic
1. Process Instructions: `AS -> AS'`
    * **AS**: Architectural state before processing
    * process instruction
    * **AS'**: Architectural state after processing
1. ISA specifies abstractly what AS' should be, given an instruction as AS
1. Microarchitecture implements how AS is transformed to AS’
1. Single-Cycle Machine
    * Each instruction takes a single clock cycle
    * `CPI`(cycles per instruction) is 1
    * the slowest instruction determines cycle time
    * Clock cycle time = long
1. Instruction Processing phases
    * Fetch
    * Decode
    * Evaluate Address
    * Fetch Operands
    * Execute
    * Store Result
1. An instruction processing engine consists by:
    * **Datapath**: hardware elements that deal with and transform data signal
    * **Control logic**: hardware elements that determine control signals
1. Control signals controls two things:
    * How the datapath should process the data
    * How to generate the control signal for the next clock cycle
1. Design Principles
    * **Critical path**: decrease the maximum logic delay
    * **Common case**: spend time and resources on where it matters most
    * **Balanced**: balance all components
    * **Keep it simple**
1. Multi-Cycle Machine
    * Each instruction takes as many clock cycles as it needs to take
    * `CPI` = different for each instruction
    * Clock cycle time = short
1. Microprogrammed Control Structure
    * **Microinstruction**: control signals
    * **Control store**: store all microinstructions
    * **Microsequencer**: determines the address of next microinstruction
1. Advantages of Microprogrammed Control
    * a simple design to do powerful computation by sequencer
    * enables easy extensibility of the ISA
    * enables update of machine behavior
    
## Pipeline
1. Idea: Increases instruction processing throughput
    * Divide the instruction processing cycle into distinct“stages” of processing
    * Ensure there are enough hardware resources to process one instruction in each stage
    * Process a different instruction in each stage
1. Four independent ADDs
    * Multi-cycle: 4 cycles per instruction
        ```js
        [
            [F,D,E,W,_,_,_,_,_,_,_,_,_,_,_,_],
            [_,_,_,_,F,D,E,W,_,_,_,_,_,_,_,_],
            [_,_,_,_,_,_,_,_,F,D,E,W,_,_,_,_],
            [_,_,_,_,_,_,_,_,_,_,_,_,F,D,E,W],
        ]
        ```
    * Pipelined: 4 cycles per 4 instructions
        ```js
        [
            [F,D,E,W,_,_,_],
            [_,F,D,E,W,_,_],
            [_,_,F,D,E,W,_],
            [_,_,_,F,D,E,W],
        ]
        ```
1. Ideal Pipeline and Not Ideal
    * Repetition of identical operations
        * actually, different instructions don't need the same stages
        * external fragmentation (some pipe stages idle for some instructions)
    * Repetition of independent operations
        * actually, different pipeline stages don't have the same latency
        * internal fragmentation (some stages are too fast but all take the same clock cycle time)
    * Uniformly partitionable suboperations
        * actually, instructions are not independent of each other
        * pipeline stalls (pipeline is not always moving)
1. Pipeline Stalls and solutions
    * Resource contention
        * duplicate the resource or increase its throughput
        * stall one of the contending stages
    * Dependences
        * Data Dependences
            * Flow Dependence: read after write
                * Detect and wait unit value is available.
                * Predict the value, execute speculatively, and verify
                * Detect and forward data to dependent instruction.
                * Detect and eliminate the dependence at software level
            * Output Dependence: write after write
            * Anti dependence: write after read
        * Control Dependences: know the address of the next instruction 
            * Stall until the next fetch address get known
            * Branch prediction: guess the next fetch address
            * Predicted execution: exec all branches with condition, some will become no-ops
            * Fine-grained Multi-threading: all instructions in pipeline come from different threads
    * Long-latency operations
1. Dependence Detection
    * Scoreboard
        * Add a valid bit for each register
        * An instruction that is writing to it resets the bit
        * An instruction in Decode stage checks if all its registers are valid
    * Combinational dependence check logic
        * if the instruction in later stages is  supposed to write any source register of the decoded instructions
## Branch Prediction
1. Predicates 3 things at fetch stage:
    * Whether the fetched instruction is a branch
    * Branch direction
    * Branch target address (if taken)
1. Branch Target Buffer (BTB) caches target addresses
    * Provides target address computed last time
    * If BTB provides a address, it must be a branch
1. Branch direction?
    * Static
        * Always not-taken: no BTB, no direction prediction, low accuracy
        * Always taken: no direction prediction, better accuracy (loops are usually taken)
        * Loop taken, others not-taken
        * Profile-based: compiler determines and provide meta-data
        * Program-based: analysis program heuristic
    * Dynamic: based on run-time information
        * Last time predictor: good for big loop, predictor changes too quickly
        * Two-bit Counter predictor: strongly taken, weakly taken, strongly !taken, weakly !taken


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
    * How many and size of each register?
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

## [MIPS](https://course.ece.cmu.edu/~ece447/s15/lib/exe/fetch.php?media=mips_reference_data.pdf)
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
        
## Microarchitecture Basic
1. Process Instructions: `AS -> AS'`
    * **AS**: Architectural state before processing
    * process instruction
    * **AS'**: Architectural state after processing
1. ISA specifies abstractly what AS' should be, given an instruction as AS
1. Microarchitecture implements how AS is transformed to AS’
1. Single-Cycle Machine
    * Each instruction takes a single clock cycle
    * `CPI`(cycles per instruction) is 1
    * the slowest instruction determines cycle time
    * Clock cycle time = long
1. Instruction Processing phases
    * Fetch
    * Decode
    * Evaluate Address
    * Fetch Operands
    * Execute
    * Store Result
1. An instruction processing engine consists by:
    * **Datapath**: hardware elements that deal with and transform data signal
    * **Control logic**: hardware elements that determine control signals
1. Control signals controls two things:
    * How the datapath should process the data
    * How to generate the control signal for the next clock cycle
1. Design Principles
    * **Critical path**: decrease the maximum logic delay
    * **Common case**: spend time and resources on where it matters most
    * **Balanced**: balance all components
    * **Keep it simple**
1. Multi-Cycle Machine
    * Each instruction takes as many clock cycles as it needs to take
    * `CPI` = different for each instruction
    * Clock cycle time = short
1. Microprogrammed Control Structure
    * **Microinstruction**: control signals
    * **Control store**: store all microinstructions
    * **Microsequencer**: determines the address of next microinstruction
1. Advantages of Microprogrammed Control
    * a simple design to do powerful computation by sequencer
    * enables easy extensibility of the ISA
    * enables update of machine behavior
    
## Pipeline
1. Idea: Increases instruction processing throughput
    * Divide the instruction processing cycle into distinct“stages” of processing
    * Ensure there are enough hardware resources to process one instruction in each stage
    * Process a different instruction in each stage
1. Four independent ADDs
    * Multi-cycle: 4 cycles per instruction
        ```js
        [
            [F,D,E,W,_,_,_,_,_,_,_,_,_,_,_,_],
            [_,_,_,_,F,D,E,W,_,_,_,_,_,_,_,_],
            [_,_,_,_,_,_,_,_,F,D,E,W,_,_,_,_],
            [_,_,_,_,_,_,_,_,_,_,_,_,F,D,E,W],
        ]
        ```
    * Pipelined: 4 cycles per 4 instructions
        ```js
        [
            [F,D,E,W,_,_,_],
            [_,F,D,E,W,_,_],
            [_,_,F,D,E,W,_],
            [_,_,_,F,D,E,W],
        ]
        ```
1. Ideal Pipeline and Not Ideal
    * Repetition of identical operations
        * actually, different instructions don't need the same stages
        * external fragmentation (some pipe stages idle for some instructions)
    * Repetition of independent operations
        * actually, different pipeline stages don't have the same latency
        * internal fragmentation (some stages are too fast but all take the same clock cycle time)
    * Uniformly partitionable suboperations
        * actually, instructions are not independent of each other
        * pipeline stalls (pipeline is not always moving)
1. Pipeline Stalls and solutions
    * Resource contention
        * duplicate the resource or increase its throughput
        * stall one of the contending stages
    * Dependences
        * Data Dependences
            * Flow Dependence: read after write
                * Detect and wait unit value is available.
                * Predict the value, execute speculatively, and verify
                * Detect and forward data to dependent instruction.
                * Detect and eliminate the dependence at software level
            * Output Dependence: write after write
            * Anti dependence: write after read
        * Control Dependences: know the address of the next instruction 
            * Stall until the next fetch address get known
            * Branch prediction: guess the next fetch address
            * Predicted execution: exec all branches with condition, some will become no-ops
            * Fine-grained Multi-threading: all instructions in pipeline come from different threads
    * Long-latency operations
1. Dependence Detection
    * Scoreboard
        * Add a valid bit for each register
        * An instruction that is writing to it resets the bit
        * An instruction in Decode stage checks if all its registers are valid
    * Combinational dependence check logic
        * if the instruction in later stages is  supposed to write any source register of the decoded instructions
## Branch Prediction
1. Predicates 3 things at fetch stage:
    * Whether the fetched instruction is a branch
    * Branch direction
    * Branch target address (if taken)
1. Branch Target Buffer (BTB) caches target addresses
    * Provides target address computed last time
    * If BTB provides a address, it must be a branch
1. Branch direction?
    * Static
        * Always not-taken: no BTB, no direction prediction, low accuracy
        * Always taken: no direction prediction, better accuracy (loops are usually taken)
        * Loop taken, others not-taken
        * Profile-based: compiler determines and provide meta-data
        * Program-based: analysis program heuristic
    * Dynamic: based on run-time information
        * Last time predictor: good for big loop, predictor changes too quickly
        * Two-bit Counter predictor: strongly taken, weakly taken, strongly !taken, weakly !taken

