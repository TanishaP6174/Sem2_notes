
# INTRODUCTION

**Principle of locality** : States that programs access a relatively small portion of their address space at any instant of time.

2 types:
1. Temporal locality: if an item is referenced, it will tend to be referenced again soon.
2. Spacial locality: if an item is referenced, items whose addresses are close by will tend to be referenced soon.

**Memory Hierarchy** : A structure that uses multiple levels of memories; as the distance from the processor increases, the size of the memories and the access time both increase.
- Faster, more expensive memory is close to the processor.

![[Pasted image 20260501021149.png|454]]

- Flash memory has replaced disks in PMDs
- Data is similarly hierarchical.
- A memory hierarchy can consist of multiple levels, but data are copied between only two adjacent levels at a time.
Block/line - The minimum unit of information that can be either present or not present in a cache

If the data requested by the processor appear in some block in the upper level, this is called a **hit**.
If the data are not found in the upper level, the request is called a **miss**.
The lower level in the hierarchy is then accessed to retrieve the block containing the requested data.

The hit rate, or hit ratio, is the fraction of memory accesses found in the upper level; it is often used as a measure of the performance of the memory hierarchy.

The miss rate = 1- hit rate

**Hit time** : The time required to access a level of the memory hierarchy, including the time needed to determine whether the access is a hit or a miss.

**Miss penalty** : The time required to fetch a block into a level of the memory hierarchy from the lower level, including the time to access the block, transmit it from one level to the other, insert it in the level that experienced the miss, and then pass the block to the requestor.

	Memory hierarchies take advantage of temporal locality by
	keeping more recently accessed data items closer to the processor.
	Memory hierarchies take advantage of spatial locality by moving
	blocks consisting of multiple contiguous words in memory to
	upper levels of the hierarchy.

In most systems, the memory is a true hierarchy, meaning that data cannot be present in level i unless they are also present in level i + 1.

>[!Question]
> ![[Pasted image 20260501022406.png]]

# Solution:

## Statement 1
Memory hierarchies take advantage of temporal locality.

**Answer: True**

- Temporal locality means recently accessed data is likely to be accessed again soon.
- Cache memory is designed to store recently used data.
- This is one of the core principles behind memory hierarchy.

---

## Statement 2
On a read, the value returned depends on which blocks are in the cache.

**Answer: False**

- The value returned must always be the **correct value from memory**.
- Cache only affects **where the data comes from (fast or slow)**, not **what the data is**.
- Even on a cache miss, the correct value is fetched from lower levels.

---

## Statement 3
Most of the cost of the memory hierarchy is at the highest level.

**Answer: True**

- Highest level = cache (especially L1)
- Cache memory is:
  - Very fast
  - Built using expensive technology (SRAM)
- Lower levels (like DRAM, disk) are much cheaper per bit

---

## Statement 4
Most of the capacity of the memory hierarchy is at the lowest level.

**Answer: True**

- Lowest level = main memory / disk
- These have:
  - Very large storage capacity
  - Lower cost per bit
- Cache is small, memory is large, disk is massive

---

## Final Answer
**Statements that are true: 1, 3, and 4**


---
# Memory technologies:

There are 4 main technologies used in memory heirarchies:

## 1. SRAM

- Static random access memory, levels closer to the processor use it, more costly per bit, faster
- More costly than DRAM as its more area per bit of memory.
- Integrated circuits that are memory arrays with a single access port that can either provide read/write
- SRAMs dont need to refresh and so the access time is very close to the cycle time.
## 2. DRAM

- substantially slower than SRAM, cheaper
- have larger capacity for same amount of silicon than SRAM
- implements main memory
- dynamic random access memory - the vlue A s
- Because DRAMs use only one transistor per bit of storage, they are much denser and cheaper per bit than SRAM.
- - The value kept in a cell is stored as a charge in a capacitor.
- A singe transistor is then used to access this stored charge.
- As DRAMs store the charge on a capacitor, it cannot be kept indefinitely and must periodically be refreshed. That's why its called dynamic.
- To refresh the cell, we merely read its contents and write it back
- DRAMs use a two-level decoding structure, and this allows us to refresh an entire row (which shares a word line) with a read cycle followed immediately by a write cycle.

Modern DRAMs are organized in banks, typically four for DDR3. Each bank consists of a series of rows. Sending a PRE (precharge) command opens or closes a bank. A row address is sent with an Act (activate), which causes the row to transfer to a buffer. When the row is in the buffer, it can be transferred by successive column addresses at whatever the width of the DRAM

## 3. Flash memory

- This non-volatile memory is the secondary memory in Personal Mobile Devices.

## 4. Magnetic Disk

- Largest and slowest level in the hierarchy

- 


