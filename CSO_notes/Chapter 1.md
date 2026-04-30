
### Introduction

- Some appplications
    - Computers in automobiles: reduce pollution, improve fuel efficiency via engine controls and increase saftery by preventing dangerous skids
    - Cell phones
    - Human genome project: mapping and analyzing human DNA sequences
    - World wide web
    - Searche ngines

- Classes of computing applications
    - Desktop computers : PC; good performance for single users at low cost and exceute third party software
    - Servers: A computer used for running larger programs for multiple users, usually simultaneously, and typically accessed only via a network, carry large workloads, greater expandibility of both computing and input/output capacity, greater emphasis on depandability
        - Supercomputers: class of computers with the highest performance and cost, configured as servers, large memory and storage, used for high end scientific and engineering calculations.
            - eg: weather forecasting, oil exploration, protein structure determination
        - Intenet datacenters: eg-google, eBay →not supercomputers
    - Embedded computers
        - Largest class of computers, low cost, lower tolerance for failure, minimize cost and power. Stringent limitations on both.
        - A computer inside another device used for running one predetermined application or collection of software
        - Eg: microprocessors, computers in a cell phone

- PMD (Personal mobile device)
    - Small wireless devices that connect to the interenet.
    - Rely on batteries for power, software is installed by downloading apps.
    - eg: smartphone, tablet

- Cloud computing
    - A large collection of servers that provide services over the internet. Some providers rent out a number of servers as a utility.
    - These large datacenters are called Warehouse Scale Computers (WSCs)

- Software as a service
    - Delivers software and data as a service over the Internet, usually via a thin program such as a browser taht runs on local client devices

- Multicore microprocessor
    - A microprocessor containing many processors (cores) in a single integrated circuit

- Memory → most important constrain tfor computers
- Lesser memory required → faster programs

---

### Section 1.2

There are 8 great ideas in computer architecture. They are:

1. Design for Moore’s Law
    - Integrated circuit resources (e.g: transistors) double every 18-24 months. This resulted in a prediction of such growth in IC Capacity made by Gordon Moore, one of the founders of Intel
2. Use abstraction to simplify design
3. Make the common case fast (in an algorithm)
4. Performance via parallelism
    - Performance by parallelism is the optimization technique of breaking large, complex computational tasks into smaller, independent sub-tasks, processing them simultaneously across multiple processors, CPU cores, or computing nodes
5. Performance via pipeling
6. Perfoamance via prediction
    - When the recovery from a misprediction is not too expensive and the accuracy of the prediction is high, then it is m=better to make a guess and start working than waiting
7. Hierarchy of memories
8. Dependability via redundancy

---

### Section 1.3

- System software
    - Provides ervices that are commonly useful like operating systems, compilers, loaders and assemblers.
    - An operating system: Supervising program that manage steh resources of the computer for the benefit of the programs that run on the computer.
        - Interfaces between a user’s program and the hardware.
        - Handles basic input/output operations
        - Allocates storage and memory
        - Provides protected sharing of the computer among multiple applications using it simultaneously
    - Compilers:
        - translates high - level language statements to assembly language statements.

- Binary digit - bit
- Instruction - a command that a computer system understands and obeys
- Assembler - a program that translates a symbolic version of instructions to the binary version
- The symbolic representation of machine instructions = assembly language
- The binary representation of machine instructions = machine language

- A high level prgramming language
    - A portable language like C,C++,Java or Visual Basic that is composed of words and algebraic notation that can be translated into assembly language by a compiler
    - Benefits = easier to learn and use, since more like english
    - They allow languages to be designed according to their intended use:
        - Fortran - scientific computation
        - Cobol - buisness data
        - Lisp - symbol manipulation
    - They improve programmer produuctivity
    - They allow it to be independent of the computer on which they are developed.

---

### Section 1.4

- wireless netwroks provide both input and output

- Input device
    - A mechanism through which the computer is fed information, such as a keyboard.

- Output device
    - A mechanism that conveys the result of a computation to a user, such as a display, or to another computer.

- BIg picture of a computer
    - 5 components
    - input, output, memory, datapath and control, last 2 are sometimes combined and called processor

- Graphics display
    - LIquid crystal display (LCD) →controls teh transmission of light
    - A display technology using a thin layer of liquid polymers that can be used to transmit or block light according to whether a charge is applied.
    - Active matrix display : A liquid crystal display using a transistor to control the transmission of light at each individual pixel.
    - ratser resfresh buffer or frame buffer

- Touchscreen
    - Capacitivie sensing
    - people are electrical conductors, if an insulator like glass is covered with a transparent conductor, touching distorts the electrostatic field of the screen, which results in a change in capacitance.
    - multiple touches simultaneously allowed

- **Integerated circuit** — Also called a chip. A device combining dozens to millions of transistors.
- **CPU(central processor unit)** - Also called processor. The active part of the computer, which contains the datapath and control and which adds numbers, tests numbers, signals I/O devices to activate, and so on.
- **Datapath** : The component of the processor that performs arithmetic operations.
- **Control** : The component of the processor that commands the datapath, memory, and I/O devices according to the instructions of the program.
- **Memory** : The storage area in which programs are kept when they are running and that contains the data needed by the running programs.
- **DRAM (dynamic random access memory)** : Memory built as an integrated circuit; it provides random access to any location. Access times are 50 nanoseconds and cost per gigabyte in 2012 was $5 to $10.
- **Cache :** A small, fast memory that acts as a buffer for a slower, larger memory.
- **SRAM (static random access memory) :** Also memory built as an integrated circuit, but faster and less dense than DRAM, more expensive than DRAM.
- **Instruction set architecture :** Also called architecture...
- **ABI (Application bInary Interface) :** The user portion of the instruction set plus the operating system interfaces...

---

SRAM vs DRAM

| Feature | **SRAM (Static RAM)** | **DRAM (Dynamic RAM)** |
|--------|----------------------|------------------------|
| Full form | Static Random Access Memory | Dynamic Random Access Memory |
| Storage element | Flip-flops (typically 6 transistors per bit) | Capacitor + transistor (1T1C) |
| Data retention | **No refresh needed** | **Needs periodic refresh** |
| Speed | **Very fast** | Slower than SRAM |
| Access time | Low (few CPU cycles) | Higher (tens to hundreds of cycles) |
| Density | Low (fewer bits per chip) | High (more bits per chip) |
| Cost per bit | **Very expensive** | **Cheap** |
| Power consumption | Lower when idle, higher per access | Lower per bit, but refresh consumes power |
| Refresh overhead | None | Mandatory refresh circuitry |
| Complexity | Simple to use (no refresh logic) | More complex (refresh control required) |
| Typical size | Small (KBs to few MBs) | Large (GBs) |
| Used as | Cache memory (L1, L2, L3) | Main memory (RAM) |
| Volatility | Volatile | Volatile |
| Example location | Inside CPU / close to CPU | DIMM slots on motherboard |

---

# Memory heirarchy

| Level | Memory Type | Location | Speed | Capacity | Cost/bit |
|------|-------------|----------|-------|----------|----------|
| 1 | Registers | Inside CPU | Fastest | Few bytes | Highest |
| 2 | L1 Cache (SRAM) | Inside CPU | Very fast | KBs | Very high |
| 3 | L2 Cache (SRAM) | Inside / near CPU | Very fast | 100s KB – MB | Very high |
| 4 | L3 Cache (SRAM) | On-chip (shared) | Fast | MBs | High |
| 5 | Main Memory (DRAM) | Motherboard | Moderate | GBs | Medium |
| 6 | Secondary Storage (SSD/HDD) | External to CPU | Slow | TBs | Low |
| 7 | Tertiary / Backup Storage | External | Very slow | Very large | Lowest |

---

**NETWORKS**

- **Local Area Network:** A network designed to carry data within a geographically confined area, typically within a single building.
- **Wide Area Network :** A network extended over hundreds of kilometers that can span a continent.

---

### Section 1.6 : Performance

- **Response time:** Also called execution time...
- **Throughput:** Also called bandwidth...
- To maximise performace, minimise repsonse time

- system performance → referes to elapsed time
- CPU performance → refers to user CPU time

- **CPU time:** is the time the CPU spends computing for some task...

- **Clock cycle :** Also called tick, clock tick...

---

### Later sections

**PITFALLS AND FALLACIES**

**Amdahl’s Law**

A rule stating that the performance enhancement possible with a given improvement is limited by the amount that the improved feature is used. It is a quantitative version of the law of diminishing returns.

f → fraction improved