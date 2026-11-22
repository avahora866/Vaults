---
tags:
  - Theory
---
Instructions are binary encoded commands that tell the CPU what to do. The binary encoded command is split into two parts:
1. Operation (Opcode): The action the CPU should take (e.g., add, subtract, load, store, jump).
2. Operands: The data the operation uses or where to find it (e.g., registers, memory addresses, or immediate values).

# Operation (Opcode)

# Operands

# Internet Set Architecture (ISA)
The ISA is the “language” of the CPU. It defines:
- Which instructions the CPU can execute.
- How operands are specified.
- How instructions are encoded in binary.

Common examples include x86, ARM, and RISC-V. The ISA allows programmers (or compilers) to write code without worrying about the physical transistors in the CPU.

# Instruction Cycle
The CPU executes instructions in a repeating process called the fetch-decode-execute cycle:

1. Fetch: The control unit reads the next instruction from memory, using the program counter to know the address. The instruction bits are transferred to the CPU via the data bus.
2. Decode: The CPU decodes the instruction. The opcode determines which operation to perform, and the operand bits indicate where the data is. Control signals are generated to direct the ALU, registers, and memory.
3. Execute: The ALU performs the operation (e.g., addition, logical comparison, memory access). Multiplexers and registers help route data to the correct place.
4. Store / Writeback: If the instruction produces a result (like an addition), it is written back to a register or memory.
5. Update Program Counter: The CPU increments or modifies the program counter to point to the next instruction.

# Types of Instructions
- Data Movement: Load or store data between memory and registers. Example: LOAD R1, [1000]
- Arithmetic / Logic: Add, subtract, AND, OR, XOR, shifts. Example: ADD R1, R2, R3
- Control Flow: Jump, branch, call, return. Example: JUMP 0x4000 or BEQ R1, R2, label
- Input / Output: Interact with devices like keyboards, screens, or network cards.
- Special Instructions: CPU-specific operations like cache control, atomic operations, or interrupts.

# How instructions relate to transistors
At the hardware level:
- Each instruction is just a pattern of bits.
- Transistors implement the logic gates that interpret these bits.
- For example, a set of AND/OR gates might decode the opcode 0001 as an “add” operation and activate the ALU circuitry.
- Multiplexers and registers route the operands to the ALU, which computes the result using adders and logic gates.

So an entire program is just a long sequence of binary instructions, each directing billions of transistors to switch in precise patterns to perform calculations, move data, or control flow.

# Example Instruction: ADD R1, R2 → R3

Suppose we have an instruction that adds the contents of register R1 and R2 and stores the result in R3. In machine code, this instruction is just a binary number, for example:

```
0001 0001 0010 0011 // Instruction is 2 bytes long
```

- The first few bits (0001) encode the opcode “ADD.”
- The next bits (0001 and 0010) specify the source registers R1 and R2.
- The final bits (0011) specify the destination register R3.

## Step 1: Fetch
1. The program counter (PC) holds the address of this instruction in memory.
2. The control unit sends this address to the memory via the address bus.
3. Memory returns the instruction as a sequence of bits on the data bus.
4. The CPU stores this instruction in the instruction register.
At this stage, each bit is stored in a flip-flop or memory cell built from transistors. Each transistor holds either a high voltage (1) or low voltage (0).

## Step 2: Decode
1. The control unit reads the opcode bits and determines this is an “ADD” instruction.
2. Control signals are generated to activate the ALU for addition.
3. Multiplexers select which registers’ outputs will be sent to the ALU. For this instruction, the multiplexers route the contents of R1 and R2 to the ALU inputs.

Inside the ALU:
- Each bit from the registers enters an adder circuit.
- Full-adders calculate the sum of each bit pair along with carry bits.
- Logic gates like XOR, AND, and OR determine sum and carry for each bit.

## Step 3: Execute
- The adder circuit inside the ALU computes the binary sum of R1 and R2.
- Each bit’s sum is stored in temporary lines, and any carry is propagated to the next higher bit using the full-adder logic.
- Transistors in the adder switch on/off to produce correct voltages representing 0 or 1 at each output line.
For example, adding 0011 (3) and 0101 (5) produces 1000 (8), bit by bit.

## Step 4: Writeback
- Once the ALU has the result, another multiplexer selects which register receives the output.
- In this case, the output is written back into R3.
- Flip-flops in R3 are set to hold the new bit values, representing the result 1000.

## Step 5: Update Program Counter
- The PC is incremented to point to the next instruction.
- The CPU is ready to repeat the fetch-decode-execute cycle for the next instruction.

## How All Circuits Work Together
- Registers hold operands and results.
- Multiplexers select which data to send to the ALU.
- Adder circuits compute the binary sum using full-adders and logic gates.
- Control unit interprets the instruction bits and coordinates signals.
- Clock synchronizes all operations so the transistors switch at the correct time.

This shows that what seems like a single high-level operation (“add two numbers”) is actually thousands of transistor switches and dozens of logic gates working together to move bits, compute sums, and store results.