
# INTRODUCTION

### OVERVIEW OF IMPLEMENTATION:

- For every instruction, the fist two steps are identical
	- Send the PC to the memory that contains the code and fetch the instruction from that memory.
	- Read one or two registers (depends on command), using fields on instruction to select the registers to read

- Simplicity favors regularity
- A control unit, which has the instructions as an input, is used to determine how to set the control lines for the functional units and two of the multiplexors. 

---
## LOGIC DESIGN CONVENTIONS

- Two types of elements in a circuit:
	- Combinational elements - their output depends only inputs
	- State elements - their output depends on input and state elements(memory element like register)
- Clocking methodology - approach used to determine when data is valid and stable relative to the clock
- Edge triggered clocking - clocking scheme in which all state changes occur on a clock edge
- A control signal - A signal used for multiplexor selection or for directing the operation of a functional unit. Different from a data signal which contains information that is operated on by a functional unit
- Asserted - signal is logically high

- An edge-triggered methodology allows us to read the contents of a register, send the value through some combinational logic, and write that register in the same clock cycle.
- There will be no feedback within a single clock cycle.


> [!info] Check yourself
> True or false: Because the register file is both read and written on the same clock cycle, any RISC-V datapath using edge-triggered writes must have more than one copy of the register file.
>
>Answer:**False.**

Here’s the key idea you should lock in:

In a typical RISC-V datapath with a **single register file**:

- **Reads happen combinationally** (i.e., whenever you put an address, you immediately get the value).
- **Writes are edge-triggered** (they occur only on the clock edge).

Because of this timing:

- During a clock cycle, you can **read old values** from the register file.
- At the clock edge, you **write the new value**.

So there is **no conflict**—you don’t need multiple copies of the register file.

--- 
## BUILDING A DATAPATH

- Datapath element - A unit used to operate on or hold data within a processor. eg: instruction and data memories, the register file, the ALU and adders.
- PC - the register containing the address of the instruction in the program being executed


**NOTE** : Will do 4.3 and 4.4 later

---
## AN OVERVIEW OF PIPELINING:

- Pipelining is an implementation technique in which multiple instructions are overlapped in execution.
- As long as we have separate resources for each stage, we can pipeline the tasks.
- Pipelining improves throughput.
- If all the stages take about the same amount of time and there is enough work to do, then the speed-up due to pipelining is equal to the number of stages in the pipeline. 
	- It becomes n times faster if n is the number of stages

- General steps for RISCV instructions:
	1. Fetch instruction from memory
	2. Read registers and decode the instruction
	3. Execute the operation or calculate an address.
	4. Access an operand in data memory (if necessary)
	5. Write the result into a register(if necessary)

![[Pasted image 20260430171835.png]]


> [!info]  Question
> we limit our attention to seven instructions: load doubleword (ld), store doubleword (sd),
add (add), subtract (sub), AND (and), OR (or), and branch if equal (beq). Contrast the average time between instructions of a single-cycle implementation, in which all instructions take one clock cycle, to a pipelined implementation. Assume that the operation times for the major functional units in this example are 200 ps for memory access for instructions or data, 200 ps for ALU operation, and 522100 ps for register file read or write. In the single-cycle model, every instruction takes exactly one clock cycle, so the clock cycle must be stretched to accommodate the slowest instruction.

![[Pasted image 20260430124543.png|602]]

![[Pasted image 20260430124848.png]]

The pipeline stage times of a computer are also limited by the slowest resource, either the ALU operation or the memory access. Find avg time

![[Pasted image 20260430124822.png]]

- Each instruction in RISC-V Is of the same length
- RISC-V has just a few instruction formats with the source and destination register fields being located in the same place in each instruction
- Memory operands only appear in loads or stores in RISC-V. This restriction means that we use execute stage to calculate the memory address and then access memory in the following stage.

- In an instruction set like the x86, where instructions vary from 1 byte to 15 bytes, pipelining is considerably more challenging.

### PIPELINE HAZARDS:

1. Structural hazard
	- When a planned instruction cannot be executed in the proper clock cycle because the hardware does not support the combination of instructions that are set to execute.
2. Data hazard
	- Also called pipeline data hazard
    - When a planned instruction cannot be executed in the proper clock cycle because the required data is not yet available.
    - Data hazards occur when the pipeline must be stalled because one step must wait for another to complete.
    - Data hazards arise from the dependence of one instruction on an earlier one that is still in the pipeline.
    - For example, suppose we have an add instruction followed immediately by a subtract instruction that uses that sum (x19):
        ```
        add x19, x0, x1
        sub x2, x19, x3
        ```
    - Without intervention, a data hazard could severely stall the pipeline. The add instruction doesn’t write its result until the fifth stage, meaning that we would have to waste three clock cycles in the pipeline.

	- Delay is too long to rely on compiler to fix it.
3. Control Hazard
	- Also called a branch hazard.
	- When the proper instruction cannot be executed in the pipeline clock cycle because the instruction that was fetched is not the one that is needed, that is the flow of instruction addresses is not what the pipeline expected.
	- *Informal definition: The CPU doesn’t know **which instruction comes next** because it depends on a branch decision.* 

## DATA HAZARD:

**Primary solution**: We dont need to wait for instruction to complete before resolving this.
+ Add extra hardware to retrieve the missing item early from internal resources and supply it as input as soon as it has been obtained. This is called forwarding or bypassing.
+ **Forwarding** - A method of resolving a data hazard by retrieving the missing data element from *internal buffers* rather than waiting for it to arrive from programmer-visible registers or memory.
![[Pasted image 20260430165619.png]]
+ forwarding paths are valid only if the destination stage is later in time than the source stage
- forwarding will not always work in cases where the input required is only obtained after where it is in a at a time after it is needed. An example is when a load instruction only loads the value required for the next instruction after it is required

- **Load-use data hazard**: A specific type of data hazard where the data needed isnt available because it is still being loaded by a load instruction.
- **Pipeline stall** : Also called a bubble. A stall initialized in order to resolve a hazard.

- Forwarding is harder if there are multiple results to forward per instruction or if there is a need to write a result early on in instruction execution.

> [!Question]
> ![[Pasted image 20260430171225.png]]

**SOLUTION:**

![[Pasted image 20260430171253.png]]
![[Pasted image 20260430171310.png]]

## CONTROL HAZARD:

### **Solution 1: Stall (Control Hazard)**

- Stall is a conservative method to handle control hazards.
- The pipeline pauses execution after fetching a branch instruction.
- It waits until the branch decision is known before fetching the next instruction.
- This ensures correctness but reduces performance.
- Even if additional hardware is used to do everything in the ID stage, itll still take an extra clock cycle, and more if the pipeline is complex.
##### How it works
- When a branch instruction is fetched:
    - The pipeline does not proceed immediately.
    - It waits until:
        - The branch condition is evaluated
        - The correct next instruction address is determined
- During this waiting period, the pipeline inserts a **bubble** (empty cycle).
##### Key idea
- The CPU avoids fetching incorrect instructions by delaying execution.
- No incorrect instructions enter the pipeline.
##### Pipeline effect
- One or more clock cycles are wasted.
- These wasted cycles are called **stalls** or **bubbles**.
##### Example impact
- Assume:
    - Ideal CPI = 1
    - 17% of instructions are branches
    - Each branch causes 1 stall cycle
- Then:
    - CPI = 1 + 0.17 = 1.17
##### Interpretation
- The processor becomes slower due to stalling.
- About 17% slowdown compared to ideal execution.
##### Limitations
- Inefficient because every branch causes a delay.
- Performance degrades further in deeper pipelines.
- Not suitable for high-performance processors.

![[Pasted image 20260430173730.png]]

### **Solution 2: Branch Prediction**

- Instead of waiting, the CPU predicts the outcome of a branch.
- The pipeline continues execution based on this prediction.
##### Basic idea
- Guess whether the branch will be:
    - Taken
    - Not taken
- Fetch and execute instructions accordingly.
##### Case 1: Prediction correct
- Execution proceeds normally.
- No performance penalty.
##### Case 2: Prediction incorrect
- Incorrect instructions must be discarded.
- The pipeline is restarted from the correct instruction.
- This process is called a **pipeline flush**.

##### Static prediction
- Fixed rules are used:
    - Always predict not taken
    - Backward branches predicted taken (loops)
    - Forward branches predicted not taken
##### Dynamic prediction
- Uses past behavior of branches.
- The processor keeps track of previous outcomes.
- Predictions are adjusted over time.

##### Key property
- High accuracy (often above 90%).
- Significantly improves performance compared to stalling.
##### Cost of misprediction
- Wasted work due to flushing incorrect instructions.
- Penalty increases with deeper pipelines.

### **Solution 3: Delayed Branch/Delayed decision**

- The instruction immediately after the branch is always executed.
- The branch takes effect after one instruction delay.
##### How it works
- The compiler rearranges instructions.
- It places a safe instruction after the branch.
- This instruction must not depend on the branch outcome.
##### Example

beq x1, x2, LABEL  
add x5, x6, x7 # always executed
##### Key idea
- The delay slot is filled with useful work.
- Avoids wasting cycles.
##### Limitations
- Works best when suitable instructions are available.
- Less effective for complex or long pipelines.
- Rarely used in modern processors; replaced by prediction.

**Note:** Used by MIPS architecture. Used when branches are short. The assembler automatically rearranges the instructions to achieve this
### Solutions

1. Stall
    - Wait for decision
    - Simple but slow

2. Branch Prediction
    - Guess outcome
    - Fast when correct, costly when wrong

3. Delayed Branch
    - Execute next instruction regardless
    - Requires compiler support

> [!Question]
> ![[Pasted image 20260430173630.png]]

**Branch prediction** - A method of resolving a branch hazard that assumes a given outcome for the conditional branch and proceeds from that assumption rather than waiting to ascertain the actual outcome.

> [!Question]
> ![[Pasted image 20260430174529.png]]

## **Solution:**
### **Sequence 1**

```
ld   x10, 0(x10)add  x11, x10, x10
```

### Analysis

- `ld` writes to **x10**
- Next instruction immediately **uses x10**
- This is a **load-use hazard**

### Important fact

- Load result is available **after MEM stage**
- But `add` needs it in **EX stage (next cycle)**

👉 Even with forwarding, the data is **not ready in time**

### **Conclusion**

- **Must stall (1 cycle)**
- Cannot be solved by forwarding alone

### **Sequence 2**

```
add   x11, x10, x10addi  x12, x10, 5addi  x14, x11, 5
```

### Analysis

- First instruction writes **x11**
- Third instruction reads **x11**
- There is **one instruction gap** in between

### What happens

- By the time `addi x14, x11, 5` executes:
    - Result from `add` is available
    - Can be forwarded from EX/MEM or MEM/WB

### **Conclusion**

- **No stall needed if forwarding is used**
- Without forwarding → would stall
- With forwarding → works smoothly

### **Sequence 3**

```
addi x11, x10, 1addi x12, x10, 2addi x13, x10, 3addi x14, x10, 4addi x15, x10, 5
```

### Analysis

- All instructions:
    - **Read x10**
    - Write to different registers
- No instruction depends on another

### **Conclusion**

- **No hazards at all**
- Executes without:
    - Stalling
    - Forwarding





- Pipelining doesnt reduce latency, or time it takes to complete an individual instruction.
**Latency** : The number of stages in a pipeline or the number of stages between two instructions during execution.
## PIPELINED DATAPATH AND CONTROL

### Stages of the pipeline

1. IF: Instruction fetch
2. ID: Instruction decode and register file read
3. EX: Execution or address calculation
4. MEM: Data memory access
5. WB: Write back

- data moves generally from left to right through the five stages as they complete execution. There are to exceptions:
	- The write-back stage, which places the result back into the register file in the middle of the datapath. *Can lead to data hazards*
	- The selection of the next value of the PC, choosing between the incremented PC and the branch address from the MEM stage. *Can lead to control hazards*
	- Data flowing from right to left do not affect the current instruction; these reverse data movements influence only later instructions in the pipeline.

**Convention for representation:**
registers read during register fetch (ID) and registers written during write back (WB). This dual use is represented by drawing the unshaded left half of the register file using dashed lines in the ID stage, when it is not being written, and the unshaded right half (shade left half) in dashed lines in the WB stage, when it is not being read

we highlight the right half of registers or memory when they are being read and highlight the left half when they are being written.


- Each instruction will not have its own datapath. we add registers to hold data so that portions of a single datapath can be shared during instruction execution.

![[Pasted image 20260430191338.png]]

- there is no pipeline register at the end of the write- back stage. All instructions must update some state in the processor —the register file, memory, or the PC—so a separate pipeline register is redundant to the state that is updated. For example, a load instruction will place its result in one of the 32 registers, and any later instruction that needs that data will simply read the appropriate register.

- The PC can be thought of as a pipeline register: one that feeds the IF stage of the pipeline.

### EXPLANATION OF STAGES:

#### LOAD INSTRUCTION

1. Instruction fetch: 
	- The instruction being read from memory using the address in the PC and then being placed in the IF/ID pipeline register. 
	- The PC address is incremented by 4 and then written back into the PC to be ready for the next clock cycle. 
	- This PC is also saved in the IF/ID pipeline register in case it is needed later for an instruction, such as beq. 
2. Instruction decode and register file read: 
	- The instruction portion of the IF/ID pipeline register supplies  the immediate field, which is sign-extended to 64 bits, and the register numbers to read the two registers.
	- All three values are stored in the ID/EX pipeline register, along with the PC address. 
	- We again transfer everything that might be needed by any instruction during a later clock cycle. 
3. Execute or address calculation: 
	- The load instruction (same for store) reads the contents of a register and the sign-extended immediate from the ID/EX pipeline register and adds them using the ALU. 
	- That sum is placed in the EX/MEM pipeline register. 
4. Memory access: 
	- The instruction reads the data memory using the address from the EX/MEM pipeline register and loading the data into the MEM/WB pipeline register. 
5. Write-back: 
	- The data is read from the MEM/WB pipeline register and written into the register file in the middle of the figure.
	- load must pass the register number in which things will be stored from the ID/EX through EX/MEM to the MEM/WB pipeline register for use in the WB stage.
#### STORE INSTRUCTION

What's different for it?
4. Memory access:
	- The data is written to memory. 
	- Note that the register containing the data to be stored was read in an earlier stage and stored in ID/EX. 
	- The only way to make the data available during the MEM stage is to place the data (which is present in the 2nd register rs2, specifically) from the ID/EX pipeline register into the EX/MEM pipeline register in the EX stage, just as we stored the effective address into EX/MEM.
	- The data were first placed in the ID/EX pipeline register and then passed to the EX/MEM pipeline register.
5. Write - back:
	- Nothing to do in WB stage as it doesnt write anything into a register.
	- Since every instruction behind the store is already in progress, we have no way to accelerate those instructions. Hence, an instruction passes through a stage even if there is nothing to do, because later instructions are already progressing at the maximum rate.

**Note:**

- During the second stage ID, both rs1 ans rs2 are read from, even though only 1 regster needs to be read for load operation. This is done to simplify control of the datapath.
- First two stages are executed the same way for all instructions.

- Pipeline registers store intermediate results so that:
	- each instruction keeps its own data as it moves forward, and
	- the main hardware can be reused by other instructions in the next cycle.
	
Single cycle processor = Clock cycle = full instruction path (worst-case instruction)
Pipeline Model = Clock cycle = slowest stage

### Graphically representing pipelines

- two basic styles of pipeline figures:
	- multiple-clock-cycle pipeline diagrams
		![[Pasted image 20260430195207.png]]
	- single-clock-cycle pipeline diagrams
		![[Pasted image 20260430195254.png]]

- The multiple-clock-cycle diagrams are simpler but do not contain all the details
- You can show the physical resources used at each stage or the name of each stage
- ![[Pasted image 20260430195522.png]]
- The little black lines represent carrying data
![[Pasted image 20260430195656.png]]
- Multiple clock cycle diagram used to see overview
- Single-clock-cycle pipeline diagrams show the state of the entire datapath during a single clock cycle, and usually all five instructions in the pipeline are identified by labels above their respective pipeline stages.

> [!question]
> ![[Pasted image 20260430200859.png]]
> # First: Core principle (you MUST know)

# SOLUTION:

In a pipeline:

- **Throughput** = how many instructions complete per unit time
- **Latency** = time taken by one instruction

👉 VERY IMPORTANT:

```
Throughput depends on clock cycle timeLatency depends on number of stages
```

---

## 🔴 Statement 1

> Allowing branches and ALU instructions to take fewer stages … will increase performance under all circumstances.

❌ **False**

### Why:

- Even if some instructions take fewer stages,
- The pipeline still runs at **one instruction per cycle (ideally)**

👉 Throughput does **NOT increase**

Also:

- “under all circumstances” makes it clearly wrong

---

## 🔴 Statement 2

> Trying to allow some instructions to take fewer cycles does not help, since throughput is determined by clock cycle…

✅ **True**

### Why:

- Pipeline throughput depends on:

```
1 instruction per cycle (ideal)
```

- Not on how many stages an instruction uses

👉 Fewer stages → reduces **latency**, not throughput

---

## 🔴 Statement 3

> You cannot make ALU instructions take fewer cycles… but branches can…

❌ **False**

### Why:

- You _can_ shorten ALU instructions (in theory)
- But it **doesn’t improve throughput**

Also:

- Branches are not special in this context
- This statement misunderstands pipeline behavior

---

## 🔴 Statement 4

> Make pipeline longer → more cycles per instruction but shorter cycles → improves performance

✅ **True**

### Why:

If you increase pipeline stages:

- Each stage does less work
- Clock cycle becomes shorter

So:

```
More stages → smaller cycle time → higher throughput
```

---

## ⚠️ Trade-off (important but ignored here):

- More hazards
- More stalls
- More complexity

But question says:

> ignore hazards

👉 So performance improves

---

## ✅ FINAL ANSWERS

|Statement|Answer|
|---|---|
|1|❌ False|
|2|✅ True|
|3|❌ False|
|4|✅ True|



-  