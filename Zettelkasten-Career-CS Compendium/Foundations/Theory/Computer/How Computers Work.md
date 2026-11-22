---
tags:
  - Theory
---
Computers are complex machine and so this note will cover the most basic of information regarding the computer starting from the lowest layer.

At the most fundamental level computers are basically billions of transistors.

# Transistor
A transisitor is a small electronic switch that is either on (current flows) or off (no current). Physically they represent 1's and 0's. 

# Binary or Base 2
Base 2 or binary is a type of number system. There are various number systems, the one we are most familiar with is base 10 or decimal.
Base 10 is a way of representing numbers using 10 digits 0 -> 9. Each position in a number has a place value that is the power of 10, depending on its position from the right.
| Number | Place values (powers of 10) |
| ------ | --------------------------- |
| 345    | 3×10² + 4×10¹ + 5×10⁰       |
| 3      | 3x10⁰                       |

We just need to replace the place value of 10 with 2 to get the Base 2 or Binary number system
| Binary | Place values (powers of 2) |
| ------ | -------------------------- |
| 1011   | 1×2³ + 0×2² + 1×2¹ + 1×2⁰  |

---
Multiple transistors can be used to represent numbers through the binary number system.
A single bianry digit represnets a bit whilst 8 bits represent a byte. A byte can represent 256 values through the binary number systsem 

# Gates
By wiring bits together we can create lgoci gates for example the AND gate ouputs a 1 only if both inputs are 1 whilst the OR gate outputs 1 if either inputs are 1. With a few basic gates (AND, OR, NOT) you can construct more complex circuits that perform arithamtic, like adders that sum bianry numbers, or multiplexers that select between inputs. These small building blocks are the foundation of the CPU.

Complex circuits in the CPU:
- **Adders**: Perform binary addition. Includes half-adders (two bits) and full-adders (two bits plus carry-in), used in ALUs for arithmetic operations.
- **Multiplexers (MUX)**: Select one input from several and forward it to the output based on control signals. Used to route data between registers, ALU results, and memory.
- **Flip-Flops / Latches**: Store single bits of data, forming the basis of registers.
- **Registers**: Groups of flip-flops storing multi-bit values for fast access by the CPU.
- **Shifters / Barrel Shifters**: Move bits left or right, used in bitwise operations, multiplication, and division.
- **Counters**: Increment or decrement binary numbers, used for program counters, timers, and loop control.
- **ALU (Arithmetic Logic Unit)**: Combines adders, logic gates (AND, OR, XOR, NOT), and other circuits to perform arithmetic and logical operations.
- **Decoders / Encoders**:
    - **Decoders**: Convert binary input into a single active output, e.g., for memory address selection.
    - **Encoders**: Compress multiple active inputs into a smaller binary code.
- **Comparators**: Compare two binary numbers and output equal, greater-than, or less-than signals.
- **Memory Cells (RAM / Cache)**: Arrays of flip-flops (SRAM) or transistor-capacitor pairs (DRAM) storing bits.
- **Bus Controllers**: Manage data flow along shared communication lines in the CPU or between CPU and memory.
- **Clocked Sequential Circuits**: Combine flip-flops, gates, and counters to synchronize operations with the CPU clock.

# CPU
The CPU or the Central Processing Unit is essentially a sophisticate arrangement of these transistor-based circuits. Its core components are:
- Arithmetic Logic Unit (ALU): performs basic operations such as addition, subtraction, comparisons, and logical tests, all using binary arithmetic built from transistors
- Registers: tiny, extremely fast storage locations inside the CPU, each holding a small number of bits that the ALU operates on directly
- Control unit: orchestrates everything, fetching instructions from memory, decoding them into signals that control the ALU and other parts, and ensuring the correct sequence of operations.

# Instructions
An instruction is a binary encoded command that tells the CPU to perform a specific operation. Instructions are the smallest unit of a program the CPU can understand. Each instruction usually specifies:
1. Operation (Opcode): The action the CPU should take (e.g., add, subtract, load, store, jump).
2. Operands: The data the operation uses or where to find it (e.g., registers, memory addresses, or immediate values).
For example, a simple instruction might be “add the contents of register A to register B and store the result in register C.” In machine code, this could be a 32-bit binary pattern, where some bits encode the opcode and others encode which registers to use.

# Memory
Memory, such as RAM, stores both data and instructions. At its lowest level, RAM is built from arrays of transistors and capacitors. Each memory cell holds a bit. When the CPU needs data or instructions, it sends an address to memory, and the corresponding cells output their stored bits onto a bus, which is just a set of parallel wires that carry binary values.

# Operating System (OS)
The OS sits one layer above the CPU and hardware and acts as a manager, translating between raw hardware capabilities and the programs you run.

## Abstracting the hardware
A CPU can only understand binary instructions, directly operating on bits in registers and memory. Without an OS, every program would have to:

- Know the exact memory addresses of all hardware resources.
- Directly manage I/O devices like the screen, keyboard, or network.
- Handle timing, multitasking, and access conflicts manually.

The OS abstracts all this, providing a consistent interface for programs to interact with the hardware without worrying about the transistor level.

## Key roles of the OS
1. Process Management:
    - The OS allows multiple programs (processes) to run seemingly at the same time.
    - It schedules which instructions the CPU executes and when, switching rapidly between processes.
    - Each process gets its own virtual memory space so that programs don’t interfere with each other.

2. Memory Management:
    - The OS manages RAM allocation.
    - It maps virtual addresses used by programs to physical addresses in memory.
    - Ensures programs only access memory they are allowed to, preventing crashes.

3. Device Management:
    - The OS provides drivers that translate generic instructions like “write to file” into hardware-specific operations.
    - This avoids programs needing to know exactly how to send electrical signals to the disk or screen.

4. File System Management:
    - Provides a structured way to store and retrieve data on disks or SSDs.
    - Hides the complexity of physical sectors, blocks, and disk controllers.

5. System Calls / API:
    - Programs interact with the OS through system calls — predefined instructions like read(), write(), open().
    - The OS then translates these calls into the necessary CPU instructions and hardware signals.

## How it Interacts With CPU and Instructions
When a program executes an instruction like “add these numbers” or “read from a file”:
- CPU executes the machine code instructions in registers and ALU.
- If the instruction requires OS services (like accessing memory outside the program or I/O), it triggers a system call, often via a CPU interrupt.
- The OS takes control, performs the requested operation safely, and returns control to the program.

Essentially, the OS is software that orchestrates how instructions are executed, manages resources, and provides a safe, consistent environment for programs. Without it, every program would need to directly manipulate transistors, registers, memory addresses, and hardware devices — which would be nearly impossible for complex tasks.
