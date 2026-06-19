# CS401 — Operating Systems
## Complete Exam Solution Guide (R23 Syllabus, SEM-4)
> JISCE | B.Tech CSE | 2024-25

---

# PART A — MCQ Solutions (10 × 1 = 10)

---

## Q1. In paging, the page table is used to:
**✅ Answer: (c) Translate logical addresses to physical addresses**

### Definition
**Paging** is a memory management technique where the logical address space of a process is divided into fixed-size units called **pages**, and physical memory is divided into blocks of the same size called **frames**. The **Page Table** stores the mapping from each page number to its corresponding frame number.

### Address Translation Formula
```
Physical Address = Frame Number × Page Size + Offset
Logical Address  = Page Number × Page Size + Offset
```

### Example
- Page size = 4 KB = 4096 bytes
- Logical Address = 8200
  - Page Number = 8200 / 4096 = **2** (page index)
  - Offset      = 8200 % 4096 = **8**
- Page Table says: Page 2 → Frame 5
- Physical Address = 5 × 4096 + 8 = **20488**

### Why other options are wrong
| Option | Why Wrong |
|--------|-----------|
| (a) Map physical to logical | Opposite direction — page table goes logical → physical |
| (b) Allocate memory contiguously | Paging specifically avoids contiguous allocation |
| (d) Allocate memory segments | That's Segmentation, not Paging |

---

## Q2. Which disk scheduling algorithm has the least average seek time in most cases?
**✅ Answer: (b) SSTF (Shortest Seek Time First)**

### Definition
**SSTF** services the disk I/O request that is **closest to the current head position**, minimizing the immediate seek time at each step.

### Example
- Current head = 50
- Queue = [82, 170, 43, 140, 24, 16, 190]
- SSTF order: 43 → 24 → 16 → 82 → 140 → 170 → 190
- Total movement = 7+19+8+66+58+30+20 = **208 cylinders**
- FCFS would give **642 cylinders** for the same queue

### Why SSTF beats FCFS
FCFS processes in arrival order ignoring distance; SSTF always picks the nearest, reducing wasted movement.

### Drawback of SSTF
Can cause **starvation** — requests far from current head may never get served if closer requests keep arriving.

---

## Q3. A time-sharing OS allows multiple users to:
**✅ Answer: (a) Share the CPU at the same time**

### Definition
A **Time-Sharing Operating System** uses CPU scheduling and multiprogramming to give each user a small time slice (**quantum**) of CPU time. Users get the illusion of having a dedicated machine.

### Example
- 4 users, time quantum = 100ms
- CPU cycles through U1 → U2 → U3 → U4 → U1...
- Each user gets CPU response within ~400ms — appears instantaneous

### Real-world examples
Unix, Linux, Windows (multi-user server edition)

---

## Q4. What is the purpose of exec() system call?
**✅ Answer: (c) Replaces current process image with a new program**

### Definition
`exec()` is a family of system calls in Unix/Linux that **replaces the calling process's memory image** (code, data, heap, stack) with a new program. The PID remains the same but the program changes completely.

### Key exec() variants
| Call | Description |
|------|-------------|
| `execl()` | Takes list of arguments |
| `execv()` | Takes array of arguments |
| `execp()` | Searches PATH for the program |

### Example (C code)
```c
pid_t pid = fork();
if (pid == 0) {
    // Child process
    execl("/bin/ls", "ls", "-l", NULL);  // Replace child with "ls" program
    // If exec succeeds, this line is NEVER reached
}
```

### Comparison of Process System Calls
| Call | Action |
|------|--------|
| `fork()` | Creates new child process (copy of parent) |
| `exec()` | Replaces current process image |
| `wait()` | Parent waits for child to finish |
| `exit()` | Terminates the current process |

---

## Q5. What does "thrashing" in memory management imply?
**✅ Answer: (b) High page fault rate due to frequent swapping**

### Definition
**Thrashing** occurs when a process spends more time **swapping pages in and out of memory** than executing instructions. It happens when the system has too many processes competing for too few frames — every time a page is loaded, another needed page is evicted.

### How Thrashing Occurs
1. OS increases degree of multiprogramming
2. Each process gets fewer frames
3. Page fault rate rises sharply
4. CPU spends most time in I/O (paging), not execution
5. CPU utilization **drops** even though processes are "running"

### Graph Insight
```
CPU Utilization
    |        *
    |      *   \
    |    *       \  ← Thrashing begins here
    |  *           \
    |*               \
    +---------------------> Degree of Multiprogramming
```

### Solution: Working Set Model
Track the **working set** (recently used pages) of each process and only allow as many processes as can have their working sets in memory.

---

## Q6. In Round Robin, what happens when a process exceeds its time quantum?
**✅ Answer: (b) It is moved to the end of the ready queue**

### Definition
**Round Robin (RR)** is a CPU scheduling algorithm where each process is assigned a fixed time unit (**quantum**). When a process's quantum expires, it is **preempted** and placed at the **back of the ready queue**, giving the next process a turn.

### Example
- Processes: P1(BT=10), P2(BT=4), P3(BT=6), Quantum=4
- Execution: P1(0-4) → P2(4-8) → P3(8-12) → P1(12-16) → P3(16-18) → P1(18-20)
- P2 finishes in one quantum; P1 and P3 loop until completion

### Key Properties
- **Fair**: No process waits longer than (n-1) × quantum
- **Preemptive**: Processes can be interrupted
- **Best for time-sharing systems**

---

## Q7. Virtual Memory is a part of:
**✅ Answer: (d) None (of the options given) — it's an abstraction using disk storage**

> Note: The question's options are a) Disk storage b) Main memory c) Secondary memory d) None.

### Definition
**Virtual Memory** is a memory management technique that gives processes the illusion of having a large, contiguous address space, even if physical RAM is limited. It is implemented using **disk storage** (swap space/page file) as an extension of RAM.

### Key Concept
Virtual memory is **not physically** main memory or secondary memory — it is an **abstraction** that spans both. Pages reside in RAM when active, and in swap space (disk) when not.

### Virtual vs Physical Address
| Aspect | Virtual Address | Physical Address |
|--------|-----------------|-----------------|
| Seen by | Process | Hardware (CPU/MMU) |
| Size | Large (e.g., 4 GB per process) | Actual RAM size |
| Managed by | OS + MMU | Hardware |

---

## Q8. Main advantage of message passing in IPC?
**✅ Answer: (b) It simplifies process synchronization by using explicit messages**

### Definition
**Message Passing** is an IPC mechanism where processes communicate by explicitly **sending and receiving messages** via the OS kernel. No shared memory is involved.

### Advantages
- No shared memory → **no race conditions**
- Works across **distributed systems** (network)
- **Simpler synchronization** — send/receive are atomic

### Comparison: Message Passing vs Shared Memory
| Feature | Message Passing | Shared Memory |
|---------|----------------|---------------|
| Speed | Slower (kernel overhead) | Faster (no kernel after setup) |
| Ease of sync | Easy (explicit msgs) | Hard (need mutexes/semaphores) |
| Works across network | Yes | No |
| Risk of race conditions | Low | High |

### Example
```
Process A (Producer)         Process B (Consumer)
send(B, "data ready") ──────► receive(A, msg)
                              process(msg)
```

---

## Q9. Deadlock can be avoided by:
**✅ Answer: (d) All of the above**

### Definition
**Deadlock** is a situation where a set of processes are each waiting for a resource held by another, resulting in permanent blocking.

### Deadlock Avoidance Strategies
| Strategy | How It Works |
|----------|-------------|
| **Wait-Die** | Older process waits; younger process dies (rolls back) |
| **Preemption** | Forcibly take resources from a process |
| **Resource Ordering** | Assign global order to resource types; always request in that order |

### Wait-Die vs Wound-Wait
- **Wait-Die**: Old waits, young dies → non-preemptive for older processes
- **Wound-Wait**: Old wounds (preempts) young; young waits

### Banker's Algorithm (classic avoidance)
Check if a resource allocation leaves system in a **safe state** before granting it.

---

## Q10. The mechanism that brings a page into memory only when needed is called:
**✅ Answer: (b) Demand Paging**

### Definition
**Demand Paging** is a lazy-loading strategy where pages are loaded into memory **only when they are accessed (demanded)**, rather than loading the entire process upfront.

### How It Works
1. Process starts; **no pages** loaded initially (or minimal set)
2. CPU accesses a page → page not in RAM → **Page Fault** occurs
3. OS traps to page fault handler
4. Required page loaded from disk into a free frame
5. Page table updated; instruction restarted

### Benefits
- Saves memory (only active pages loaded)
- Faster process startup
- Supports virtual memory

### Page Fault Handling Steps
```
Access Page → [In RAM?]
                 ↓ No        ↓ Yes
           Page Fault     Continue
                 ↓
        Find free frame
                 ↓
        Load page from disk
                 ↓
        Update page table
                 ↓
        Restart instruction
```

---

## Q11. In batch OS, how are user programs executed?
**✅ Answer: (b) In a queue without user interaction**

### Definition
A **Batch Operating System** collects similar jobs into **batches** and processes them sequentially without user interaction. Users submit jobs (via punch cards historically, or job scripts), and results are collected later.

### Characteristics
- No user-program interaction during execution
- Jobs grouped by resource requirements
- High throughput for repetitive tasks
- Examples: Payroll processing, bank statement generation

### Batch vs Time-Sharing
| Feature | Batch OS | Time-Sharing OS |
|---------|----------|-----------------|
| User interaction | None | Interactive |
| Response time | Hours | Seconds |
| Job execution | Sequential batches | Time slices |

---

## Q12. Which is true about multithreading?
**✅ Answer: (a) Threads share the same memory space**

### Definition
A **Thread** is the smallest unit of CPU execution within a process. Multiple threads in the same process share: code segment, data segment, heap, and open files. Each thread has its own: stack, registers, and program counter.

### What Threads Share vs Own
| Shared (all threads) | Per-Thread (private) |
|---------------------|---------------------|
| Code (text) segment | Stack |
| Data segment | Registers |
| Heap | Program counter |
| Open file handles | Thread ID |

### Why Other Options Are Wrong
- **(b) Threads cannot run concurrently** → FALSE: on multi-core CPUs, threads run truly in parallel
- **(c) Threads are heavier than processes** → FALSE: threads are "lightweight processes"
- **(d) Threads cannot communicate** → FALSE: they share memory, so communication is trivial

---

# PART B — Short Answer Solutions (3 × 5 = 15)

---

## Q13. Page Table Size Calculation + Why Page Size is Power of 2

### Given
- Physical Memory = 64 MB = 2²⁶ bytes
- Virtual Address Space = 32-bit → 2³² bytes = 4 GB
- Page Size = 2 KB = 2¹¹ bytes

### Step-by-Step Solution

**Step 1: Calculate number of pages (virtual)**
```
Number of Pages = Virtual Address Space / Page Size
                = 2^32 / 2^11
                = 2^21 = 2,097,152 pages
```

**Step 2: Calculate bits per page table entry**
```
Number of Physical Frames = Physical Memory / Page Size
                          = 2^26 / 2^11
                          = 2^15 = 32,768 frames

Bits needed per entry = 15 bits (to address 32,768 frames)
→ Round up to 2 bytes (16 bits) per entry for alignment
```

**Step 3: Calculate page table size**
```
Page Table Size = Number of Pages × Size per Entry
               = 2^21 × 2 bytes
               = 2^22 bytes
               = 4 MB
```

### ✅ Page Table Size ≈ 4 MB

---

### Why Page Size is Always a Power of 2

**Reason 1: Efficient Address Splitting**

With power-of-2 page size, the logical address is split by simple bit extraction:
```
Logical Address (32-bit), Page Size = 2^11:
  Bits [31:11] → Page Number (21 bits)
  Bits [10:0]  → Offset     (11 bits)
```
No division needed — just bit masking (extremely fast in hardware).

**Reason 2: Hardware Simplicity (MMU Design)**
```
Page Number = Logical Address >> 11     (right shift)
Offset      = Logical Address & 0x7FF   (bitwise AND)
```
If page size were not a power of 2 (say, 1000 bytes), you'd need expensive division circuitry.

**Reason 3: Alignment with Cache Lines**
Cache lines and disk sectors are also powers of 2 (512B, 4KB), so pages align perfectly with them.

---

## Q14. Banker's Algorithm — What Does the "Need" Matrix Represent?

### Definition
The **Banker's Algorithm** (by Dijkstra) is a deadlock avoidance algorithm. It treats the OS like a banker who only grants loans (resources) if the system can remain in a **safe state**.

### Need Matrix Definition
```
Need[i][j] = Max[i][j] - Allocation[i][j]
```

The **Need matrix** represents the **maximum additional resources** that each process may still request before completing.

### Components of Banker's Algorithm
| Matrix/Vector | Meaning |
|---------------|---------|
| **Max** | Maximum resources each process may ever need |
| **Allocation** | Resources currently allocated to each process |
| **Available** | Resources currently free in the system |
| **Need** | Max − Allocation (what each process still needs) |

### Example

Suppose 3 resource types: A, B, C

| Process | Max (A,B,C) | Allocation (A,B,C) | Need (A,B,C) |
|---------|-------------|---------------------|---------------|
| P0 | 7 5 3 | 0 1 0 | **7 4 3** |
| P1 | 3 2 2 | 2 0 0 | **1 2 2** |
| P2 | 9 0 2 | 3 0 2 | **6 0 0** |
| P3 | 2 2 2 | 2 1 1 | **0 1 1** |
| P4 | 4 3 3 | 0 0 2 | **4 3 1** |

Available = [3, 3, 2]

### Safe State Check
The system is in a **safe state** if there exists a **safe sequence** — an order in which all processes can complete using available resources.

**Check P1 (Need=[1,2,2] ≤ Available=[3,3,2]):** ✅ → Run P1, release its allocation
**New Available** = [3,3,2] + [2,0,0] = [5,3,2]
...continue until all processes complete.

Safe Sequence: **P1 → P3 → P4 → P0 → P2**

### Key Insight
The Need matrix tells the OS: "Before granting this process more resources, verify that even in the worst case (it requests all of `Need`), the system can still find a safe sequence."

---

## Q15. Page Faults — OPT and LRU (Instruction Reference String)

### Given
```
Reference String: 89, 101, 212, 342, 421, 198, 287, 521, 101, 232, 376, 432, 543
Page Size: 100 instructions (so map each to a "page")
Frames = 4 (initially empty)
```

### Convert to Page Numbers (divide each by 100, floor)
```
89→0, 101→1, 212→2, 342→3, 421→4, 198→1, 287→2, 521→5, 101→1, 232→2, 376→3, 432→4, 543→5
```
**Page reference string: 0, 1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5**

---

### OPT (Optimal Page Replacement)
Replace the page that will **not be used for the longest time** in the future.

| Step | Page | Frame State | Fault? | Replaced |
|------|------|-------------|--------|----------|
| 1 | 0 | [0, -, -, -] | ✅ Yes | — |
| 2 | 1 | [0, 1, -, -] | ✅ Yes | — |
| 3 | 2 | [0, 1, 2, -] | ✅ Yes | — |
| 4 | 3 | [0, 1, 2, 3] | ✅ Yes | — |
| 5 | 4 | [4, 1, 2, 3] | ✅ Yes | 0 (never used again) |
| 6 | 1 | [4, 1, 2, 3] | ❌ No | — |
| 7 | 2 | [4, 1, 2, 3] | ❌ No | — |
| 8 | 5 | [4, 1, 2, 5] | ✅ Yes | 3 (next use furthest: step 11) |
| 9 | 1 | [4, 1, 2, 5] | ❌ No | — |
| 10 | 2 | [4, 1, 2, 5] | ❌ No | — |
| 11 | 3 | [4, 1, 3, 5] | ✅ Yes | 2 (not used after this) |
| 12 | 4 | [4, 1, 3, 5] | ❌ No | — |
| 13 | 5 | [4, 1, 3, 5] | ❌ No | — |

**OPT Page Faults = 7**

---

### LRU (Least Recently Used)
Replace the page that was **used least recently**.

| Step | Page | Frame State | Fault? | Replaced |
|------|------|-------------|--------|----------|
| 1 | 0 | [0, -, -, -] | ✅ Yes | — |
| 2 | 1 | [0, 1, -, -] | ✅ Yes | — |
| 3 | 2 | [0, 1, 2, -] | ✅ Yes | — |
| 4 | 3 | [0, 1, 2, 3] | ✅ Yes | — |
| 5 | 4 | [4, 1, 2, 3] | ✅ Yes | 0 (LRU) |
| 6 | 1 | [4, 1, 2, 3] | ❌ No | — |
| 7 | 2 | [4, 1, 2, 3] | ❌ No | — |
| 8 | 5 | [4, 1, 2, 5] | ✅ Yes | 3 (LRU: last used at step 4) |
| 9 | 1 | [4, 1, 2, 5] | ❌ No | — |
| 10 | 2 | [4, 1, 2, 5] | ❌ No | — |
| 11 | 3 | [3, 1, 2, 5] | ✅ Yes | 4 (LRU: last used at step 5) |
| 12 | 4 | [3, 4, 2, 5] | ✅ Yes | 1 (LRU: last used at step 9) |
| 13 | 5 | [3, 4, 2, 5] | ❌ No | — |

**LRU Page Faults = 8**

### Summary
| Algorithm | Page Faults |
|-----------|-------------|
| OPT | **7** |
| LRU | **8** |

OPT is always optimal (theoretical minimum); LRU approximates it well in practice.

---

## Q16. Page Faults — LRU, FIFO, OPTIMAL (3 Frames)

### Given
```
Reference String: 1, 2, 3, 2, 4, 1, 3, 2, 4, 1
Frames = 3 (initially empty)
```

### FIFO (First In First Out)
Replace the page that was **loaded first**.

| Ref | Frames | Fault? | Evicted |
|-----|--------|--------|---------|
| 1 | [1, -, -] | ✅ | — |
| 2 | [1, 2, -] | ✅ | — |
| 3 | [1, 2, 3] | ✅ | — |
| 2 | [1, 2, 3] | ❌ | — |
| 4 | [4, 2, 3] | ✅ | 1 |
| 1 | [4, 1, 3] | ✅ | 2 |
| 3 | [4, 1, 3] | ❌ | — |
| 2 | [4, 1, 2] | ✅ | 3 |
| 4 | [4, 1, 2] | ❌ | — |
| 1 | [4, 1, 2] | ❌ | — |

**FIFO = 6 page faults**

---

### LRU (Least Recently Used)

| Ref | Frames | Fault? | Evicted |
|-----|--------|--------|---------|
| 1 | [1, -, -] | ✅ | — |
| 2 | [1, 2, -] | ✅ | — |
| 3 | [1, 2, 3] | ✅ | — |
| 2 | [1, 2, 3] | ❌ | — |
| 4 | [4, 2, 3] | ✅ | 1 (LRU) |
| 1 | [4, 2, 1] | ✅ | 3 (LRU) |
| 3 | [3, 2, 1] | ✅ | 4 (LRU) |
| 2 | [3, 2, 1] | ❌ | — |
| 4 | [3, 4, 1] | ✅ | 2 (LRU) |
| 1 | [3, 4, 1] | ❌ | — |

**LRU = 7 page faults**

---

### OPTIMAL (OPT)
Replace the page **not needed for longest time in future**.

| Ref | Frames | Fault? | Evicted | Reason |
|-----|--------|--------|---------|--------|
| 1 | [1, -, -] | ✅ | — | — |
| 2 | [1, 2, -] | ✅ | — | — |
| 3 | [1, 2, 3] | ✅ | — | — |
| 2 | [1, 2, 3] | ❌ | — | — |
| 4 | [1, 4, 3] | ✅ | 2 | 2 next at step 8, 1 at step 6, 3 at step 7 → evict 2 (furthest) |
| 1 | [1, 4, 3] | ❌ | — | — |
| 3 | [1, 4, 3] | ❌ | — | — |
| 2 | [1, 4, 2] | ✅ | 3 | 3 never used again → evict 3 |
| 4 | [1, 4, 2] | ❌ | — | — |
| 1 | [1, 4, 2] | ❌ | — | — |

**OPT = 5 page faults**

---

### Final Comparison
| Algorithm | Page Faults |
|-----------|-------------|
| **OPTIMAL** | **5** (minimum possible) |
| **FIFO** | **6** |
| **LRU** | **7** |

> OPT < FIFO < LRU for this particular string. Note: LRU typically performs better than FIFO in general, but results vary per reference string.

---

## Q17. CPU Scheduling — FCFS, SJF, Priority, RR

### Given
| Process | Burst Time | Priority |
|---------|-----------|----------|
| P1 | 10 | 3 |
| P2 | 1 | 1 |
| P3 | 2 | 3 |
| P4 | 1 | 4 |
| P5 | 5 | 2 |

All arrive at time 0. Lower priority number = higher priority.

---

### a) Gantt Charts

**FCFS** (order of arrival: P1, P2, P3, P4, P5)
```
| P1       | P2 | P3 | P4 | P5   |
0          10  11   13  14      19
```

**SJF** (Shortest Job First, non-preemptive; order: P2, P4, P3, P5, P1)
```
| P2 | P4 | P3 | P5   | P1        |
0    1    2    4      9          19
```

**Non-Preemptive Priority** (lower number = higher priority: P2→P5→P1→P3→P4)
```
| P2 | P5   | P1        | P3 | P4 |
0    1       6         16   18  19
```

**Round Robin** (quantum = 1, arrival order: P1,P2,P3,P4,P5)
```
|P1|P2|P3|P4|P5|P1|P3|P5|P1|P5|P1|P5|P1|P1|P1|P1|P1|P1|P1|
0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18  19
```
(P2 done at t=2, P4 done at t=4, P3 done at t=7, P5 done at t=12, P1 done at t=19)

---

### b) Turnaround Time = Completion Time − Arrival Time

| Process | FCFS | SJF | Priority | RR |
|---------|------|-----|----------|-----|
| P1 | 10 | 19 | 16 | 19 |
| P2 | 11 | 1 | 1 | 2 |
| P3 | 13 | 4 | 18 | 7 |
| P4 | 14 | 2 | 19 | 4 |
| P5 | 19 | 9 | 6 | 12 |
| **Avg** | **13.4** | **7.0** | **12.0** | **8.8** |

---

### c) Waiting Time = Turnaround Time − Burst Time

| Process | FCFS | SJF | Priority | RR |
|---------|------|-----|----------|-----|
| P1 | 0 | 9 | 6 | 9 |
| P2 | 10 | 0 | 0 | 1 |
| P3 | 11 | 2 | 16 | 5 |
| P4 | 13 | 1 | 18 | 3 |
| P5 | 14 | 4 | 1 | 7 |
| **Avg** | **9.6** | **3.2** | **8.2** | **5.0** |

---

### d) Algorithm with Minimal Average Waiting Time

**✅ SJF has the minimum average waiting time = 3.2 ms**

SJF is provably optimal for minimizing average waiting time in a non-preemptive, single-CPU environment when all jobs arrive simultaneously.

---

# PART C — Long Answer Solutions (3 × 15 = 45)

---

## Q18a. Segmentation

### Definition
**Segmentation** is a memory management technique where a program is divided into **logical units called segments** (e.g., code, stack, heap, data). Each segment has a **variable size** and a **base + limit** entry in the **Segment Table**.

### Address Translation
```
Logical Address = (Segment Number, Offset)
Physical Address = Base[Segment Number] + Offset
```
Condition: Offset < Limit[Segment Number] (else → segmentation fault)

### Segment Table Example
| Segment | Base | Limit |
|---------|------|-------|
| 0 (code) | 1000 | 400 |
| 1 (data) | 2000 | 600 |
| 2 (stack) | 3000 | 200 |

Logical address (1, 53) → Physical = 2000 + 53 = **2053**

### Advantages
- Logical view matches programmer's view
- Easy to share segments between processes
- No internal fragmentation

### Disadvantages
- **External fragmentation** (variable-size segments)
- Requires compaction periodically

---

## Q18b. Page Table

### Definition
A **Page Table** is a data structure maintained by the OS (one per process) that stores the mapping from **page numbers** (logical) to **frame numbers** (physical).

### Structure of a Page Table Entry (PTE)
```
| Frame Number | Valid/Invalid | Dirty | Reference | Protection |
```

| Bit | Meaning |
|-----|---------|
| Valid | Page is in memory |
| Dirty | Page was modified (needs write-back) |
| Reference | Page was recently accessed |
| Protection | R/W/X permissions |

### Types of Page Tables
| Type | Description |
|------|-------------|
| Single-level | Simple array, huge for large address spaces |
| Multi-level | Hierarchical, saves space (only allocate used portions) |
| Inverted | One entry per frame (not per page), saves space for large virtual spaces |
| Hashed | Hash of page number → linked list of entries |

### Two-Level Page Table (32-bit, 4KB pages)
```
Virtual Address: [10 bits | 10 bits | 12 bits]
                  Page Dir  Page Tbl  Offset
```
Only allocate 2nd-level tables for pages actually used.

---

## Q18c. Compaction

### Definition
**Compaction** is a memory management technique that **shuffles memory contents** to consolidate all free memory into one large contiguous block, eliminating external fragmentation.

### Before and After Compaction
```
BEFORE:                         AFTER:
+--------+                      +--------+
| OS     |                      | OS     |
+--------+                      +--------+
| P1     |                      | P1     |
+--------+                      +--------+
| FREE   | 50KB                 | P2     |
+--------+                      +--------+
| P2     |                      | P3     |
+--------+                      +--------+
| FREE   | 30KB                 | FREE   | 80KB (consolidated)
+--------+                      +--------+
| P3     |
+--------+
```

### Key Points
- Solves **external fragmentation**
- Very **expensive** (must move all process data and update pointers/page tables)
- Requires **relocation support** in hardware
- Used in: garbage collected runtimes (JVM, .NET), some OS compactors

---

## Q18d. Working Set Model

### Definition
The **Working Set Model** (proposed by Peter Denning) tracks the set of pages a process has referenced in the **last Δ (delta) time units**, called the **Working Set Window**.

```
WS(t, Δ) = {pages referenced in time interval (t-Δ, t]}
```

### Purpose
- Prevents **thrashing** by ensuring each process has enough frames for its working set
- Only run a process if its entire working set fits in available memory

### Example
Reference string: 1, 2, 3, 2, 1, 5, 6, 2, 3, 1

At t=10, Δ=5: recently referenced = {6, 2, 3, 1} at steps 6-10

Working Set = **{1, 2, 3, 6}** → need 4 frames

### Working Set vs Page Fault Rate
```
Page Fault Rate
     |
High |***
     |   ***
Low  |      ***------  ← Working set fits → stable
     +---------------------> Frames Allocated
```

---

## Q18e. Fragmentation

### Definition
**Fragmentation** refers to wasted memory — space that exists but cannot be used efficiently.

### Two Types

#### Internal Fragmentation
Wasted space **inside** an allocated block.
```
Process requests 13 KB, system allocates 16 KB block
→ Internal fragmentation = 16 - 13 = 3 KB wasted
```
Occurs in: **fixed-size allocation, paging**

#### External Fragmentation
Wasted space **outside** allocated blocks — enough total free space exists, but it's not contiguous.
```
Free: 10KB + 5KB + 8KB = 23KB free
Process needs: 20KB contiguous
→ External fragmentation: request FAILS despite 23KB being free
```
Occurs in: **variable-size allocation, segmentation**

---

## Q18 — Memory Allocation: First Fit, Best Fit, Worst Fit

### Given
- Holes: 15K, 10K, 5K, 25K, 30K, 40K
- Processes: 12K, 2K, 25K, 20K

### First Fit
Allocate to the **first hole that is large enough**.

| Process | Allocated To | Remaining |
|---------|-------------|-----------|
| 12K | 15K hole | 3K left |
| 2K | 3K hole (remaining) | 1K left |
| 25K | 25K hole | 0K left |
| 20K | 30K hole | 10K left |

Internal Fragmentation = 3K (unused in 15K hole after 12K) + 0 + 0 + 10K = **13K**
External Fragmentation = 10K + 5K = **15K** (holes too small to use: 1K, 10K, 5K)

### Best Fit
Allocate to the **smallest hole that fits** (minimizes wasted space in that hole).

| Process | Allocated To | Remaining |
|---------|-------------|-----------|
| 12K | 15K hole (best fit) | 3K left |
| 2K | 3K hole | 1K left |
| 25K | 25K hole | 0K left |
| 20K | 30K hole | 10K left |

Internal Fragmentation = **3K + 0 + 10K = 13K**
External Fragmentation = **1K + 5K + 10K = 16K** (tiny leftovers)

### Worst Fit
Allocate to the **largest hole** (leaves largest possible remainder).

| Process | Allocated To | Remaining |
|---------|-------------|-----------|
| 12K | 40K hole | 28K left |
| 2K | 30K hole | 28K left |
| 25K | 28K (from 40K) | 3K left |
| 20K | 28K (from 30K) | 8K left |

Internal Fragmentation = 0 (uses largest, leftovers are still large)
External Fragmentation = 3K + 8K + 15K + 10K + 5K = **41K**

### Summary Table
| Algorithm | Internal Frag | External Frag |
|-----------|--------------|--------------|
| First Fit | 13K | 15K |
| Best Fit | 13K | 16K |
| Worst Fit | 0K | 41K |

---

## Q19a. Memory Partitions: First Fit, Best Fit, Worst Fit

### Given
- Partitions (in order): 100 KB, 500 KB, 200 KB, 300 KB, 600 KB
- Processes: 212 KB, 417 KB, 112 KB, 426 KB

### First Fit
| Process | Partition Chosen | Leftover |
|---------|-----------------|----------|
| 212 KB | 500 KB | 288 KB |
| 417 KB | 288 KB? No → 600 KB | 183 KB |
| 112 KB | 288 KB | 176 KB |
| 426 KB | Cannot fit (300KB, 176KB, 183KB all < 426KB) | ❌ Not allocated |

### Best Fit
| Process | Partition Chosen | Leftover |
|---------|-----------------|----------|
| 212 KB | 300 KB (smallest fitting) | 88 KB |
| 417 KB | 500 KB (smallest fitting) | 83 KB |
| 112 KB | 200 KB (smallest fitting) | 88 KB |
| 426 KB | 600 KB (only one left ≥ 426) | 174 KB |

**✅ Best Fit allocates all 4 processes**

### Worst Fit
| Process | Partition Chosen | Leftover |
|---------|-----------------|----------|
| 212 KB | 600 KB (largest) | 388 KB |
| 417 KB | 500 KB (largest remaining) | 83 KB |
| 112 KB | 388 KB (from 600KB remainder) | 276 KB |
| 426 KB | Cannot fit (300KB, 276KB, 83KB, 200KB < 426KB) | ❌ Not allocated |

### Most Efficient
**Best Fit** — successfully allocated all 4 processes with minimum wasted space.

---

## Q19b. Boot Block and Bad Block

### Boot Block
The **boot block** (also called the Master Boot Record or boot sector) is a **reserved, fixed location on disk** (usually first sector) containing the **bootstrap loader** program.

**Function:**
1. CPU powers on → ROM BIOS executes
2. BIOS loads boot block from disk into RAM
3. Boot block locates and loads the OS kernel
4. OS takes control

```
Disk Structure:
[Boot Block][Super Block][Free Space Map][Inodes][Data Blocks...]
```

### Bad Block
A **bad block** is a disk sector that is **permanently damaged and cannot reliably store data**.

**Two types:**
| Type | Description |
|------|-------------|
| Hard bad block | Physical damage; identified at manufacturing |
| Soft bad block | Appears during use due to wear |

**Handling Methods:**
- **Sector sparing (forwarding)**: Replace bad block with a spare sector; disk controller remaps transparently
- **Sector slipping**: Shift all sectors after the bad one by one position
- **OS-level**: OS marks block as unusable in the free block list (e.g., Unix `fsck`)

---

## Q19c. Disk Scheduling Algorithms

### Given
- Queue: 23, 89, 132, 42, 187
- Cylinders: 0–199
- Head starts at: 100

---

### FCFS (First Come First Served)
Order: 100 → 23 → 89 → 132 → 42 → 187

```
Movement: |100-23| + |23-89| + |89-132| + |132-42| + |42-187|
        = 77 + 66 + 43 + 90 + 145 = 421 cylinders
```

---

### SSTF (Shortest Seek Time First)
At each step, go to the nearest request.

```
100 → 89 → 42 → 23 → 132 → 187
     (11) (47) (19) (109) (55)
Total = 11 + 47 + 19 + 109 + 55 = 241 cylinders
```

---

### SCAN (Elevator Algorithm)
Head moves in one direction, services requests, then reverses at the end.

Assume moving toward higher cylinders first.

Sort requests: 23, 42, 89, 132, 187
Head at 100, moving UP:

```
100 → 132 → 187 → 199 → 89 → 42 → 23
     (32)  (55)  (12)  (110)(47)(19)
Total = 32 + 55 + 12 + 110 + 47 + 19 = 275 cylinders
```

---

### C-SCAN (Circular SCAN)
Head moves in one direction only; after reaching end, jumps back to beginning (without servicing on return).

```
100 → 132 → 187 → 199 → [jump to 0] → 23 → 42 → 89
     (32)  (55)  (12)   (199)        (23)(19)(47)
Total movement = 32 + 55 + 12 + 199 + 23 + 19 + 47 = 387 cylinders
```

---

### LOOK
Like SCAN but head only goes as far as the last/first request (doesn't go to disk end).

Moving UP: 100 → 132 → 187 then reverse → 89 → 42 → 23

```
100 → 132 → 187 → 89 → 42 → 23
     (32)  (55) (98)(47)(19)
Total = 32 + 55 + 98 + 47 + 19 = 251 cylinders
```

### Summary Table
| Algorithm | Total Head Movement |
|-----------|-------------------|
| FCFS | 421 |
| SSTF | 241 |
| SCAN | 275 |
| C-SCAN | 387 |
| LOOK | **251** |

---

## Q21a. Critical Section Problem — Requirements

### Definition
The **Critical Section** is a portion of code where a process accesses shared resources (shared memory, files, etc.). Only one process should be in its critical section at a time.

### Three Requirements

#### 1. Mutual Exclusion
If process Pi is executing in its critical section, **no other process** can be executing in their critical section.

```
Time →
P1:  [Entry] ████ CS ████ [Exit] ...........
P2:  ......... [Entry waits] ██ CS ██ [Exit]
```
P2 is blocked until P1 exits.

#### 2. Progress
If no process is in the critical section and some processes wish to enter, the selection of who enters **cannot be postponed indefinitely**. Processes not in their remainder section cannot participate in the decision.

(Avoids the case where no one enters even though someone wants to)

#### 3. Bounded Waiting
There must be a **bound on how many times** other processes can enter their critical section after a process has requested entry and before that request is granted.

(Prevents **starvation** — a process eventually gets to enter)

### Structure of a Process with Critical Section
```c
do {
    // Entry Section (request permission)
    entry_section();
    
    // Critical Section
    // Access shared resources here
    
    // Exit Section (release)
    exit_section();
    
    // Remainder Section
    remainder_section();
} while (true);
```

### Solutions
| Solution | Type | Satisfies All 3? |
|----------|------|-----------------|
| Peterson's Algorithm | Software (2 processes) | ✅ Yes |
| Mutex Lock | Hardware/OS | ✅ Yes |
| Semaphore | OS primitive | ✅ Yes |
| Bakery Algorithm | Software (n processes) | ✅ Yes |

---

## Q21b. Deadlock Detection from Resource Allocation Graph

### Given
P1 → R1 → P2 → R2 → P3 → R3 → P1

### Interpretation
- **P → R**: Process P **requests** resource R (P is waiting for R)
- **R → P**: Resource R **is allocated** to process P

So:
- P1 requests R1; R1 is allocated to P2
- P2 requests R2; R2 is allocated to P3
- P3 requests R3; R3 is allocated to P1

### Wait-For Graph
```
P1 → P2 → P3 → P1
```
This forms a **cycle**: P1 waits for P2, P2 waits for P3, P3 waits for P1.

### ✅ Conclusion: System IS in Deadlock

A cycle in the Resource Allocation Graph is a **necessary condition** for deadlock. When each resource type has exactly **one instance**, a cycle is also **sufficient** to confirm deadlock.

All three processes (P1, P2, P3) are permanently blocked — none can proceed.

---

## Q21c. Banker's Algorithm — System Snapshot

### Given Table
| Process | Allocation (A,B,C) | Max (A,B,C) | Available: A=3, B=3, C=2 |
|---------|---------------------|-------------|--------------------------|
| P0 | 0 1 0 | 7 5 3 | — |
| P1 | 2 0 0 | 3 2 2 | — |
| P2 | 3 0 2 | 9 0 2 | — |
| P3 | 2 1 1 | 4 2 2 | — |
| P4 | 0 0 2 | 5 3 3 | — |

### a) Total Instances of Each Resource Type
```
Total = Allocation[all] + Available

A: (0+2+3+2+0) + 3 = 7 + 3 = 10 instances
B: (1+0+0+1+0) + 3 = 2 + 3 = 5 instances
C: (0+0+2+1+2) + 2 = 5 + 2 = 7 instances
```

**Total Resources: A=10, B=5, C=7**

### b) Need Matrix
```
Need[i] = Max[i] - Allocation[i]
```
| Process | Need (A,B,C) |
|---------|-------------|
| P0 | 7-0, 5-1, 3-0 = **7 4 3** |
| P1 | 3-2, 2-0, 2-0 = **1 2 2** |
| P2 | 9-3, 0-0, 2-2 = **6 0 0** |
| P3 | 4-2, 2-1, 2-1 = **2 1 1** |
| P4 | 5-0, 3-0, 3-2 = **5 3 1** |

### c) Safe State Check

Available = [3, 3, 2]

**Step 1: Find process with Need ≤ Available**
- P1: Need=[1,2,2] ≤ [3,3,2] ✅ → Run P1
- Available = [3,3,2] + [2,0,0] = **[5,3,2]**

**Step 2:**
- P3: Need=[2,1,1] ≤ [5,3,2] ✅ → Run P3
- Available = [5,3,2] + [2,1,1] = **[7,4,3]**

**Step 3:**
- P4: Need=[5,3,1] ≤ [7,4,3] ✅ → Run P4
- Available = [7,4,3] + [0,0,2] = **[7,4,5]**

**Step 4:**
- P0: Need=[7,4,3] ≤ [7,4,5] ✅ → Run P0
- Available = [7,4,5] + [0,1,0] = **[7,5,5]**

**Step 5:**
- P2: Need=[6,0,0] ≤ [7,5,5] ✅ → Run P2

**Safe Sequence: P1 → P3 → P4 → P0 → P2**

**✅ System IS in a Safe State**

---

## Q22a. Page Faults — OPT and LRU

### Given
```
Reference String: 2, 7, 4, 7, 6, 1, 7, 6, 1, 2, 7, 2
Frames = 3 (assumed; common default if unspecified)
```

### OPT (Optimal Page Replacement)

| Ref | Frames | Fault? | Evicted | Next use of each page |
|-----|--------|--------|---------|-----------------------|
| 2 | [2,-,-] | ✅ | — | — |
| 7 | [2,7,-] | ✅ | — | — |
| 4 | [2,7,4] | ✅ | — | — |
| 7 | [2,7,4] | ❌ | — | — |
| 6 | [2,6,4] | ✅ | 7→step6,2→step9,4→never | Evict 4 (not used again) |
| 1 | [2,6,1] | ✅ | 6→step7,2→step9,4 gone | Evict... 6 next at 7, 2 next at 9, 1 next at 8 → Evict 2 (furthest) |

Wait — let me redo carefully:

| Step | Ref | Frames State | Fault? | Evicted (reason) |
|------|-----|--------------|--------|-----------------|
| 1 | 2 | [2, -, -] | ✅ | — |
| 2 | 7 | [2, 7, -] | ✅ | — |
| 3 | 4 | [2, 7, 4] | ✅ | — |
| 4 | 7 | [2, 7, 4] | ❌ | — |
| 5 | 6 | [2, 7, 6] | ✅ | 4 (not used again) |
| 6 | 1 | [1, 7, 6] | ✅ | 2 (next at step 10, furthest among 7→step7, 6→step8) |
| 7 | 7 | [1, 7, 6] | ❌ | — |
| 8 | 6 | [1, 7, 6] | ❌ | — |
| 9 | 1 | [1, 7, 6] | ❌ | — |
| 10 | 2 | [1, 7, 2] | ✅ | 6 (not used after step 10) |
| 11 | 7 | [1, 7, 2] | ❌ | — |
| 12 | 2 | [1, 7, 2] | ❌ | — |

**OPT Page Faults = 6**

---

### LRU (Least Recently Used)

| Step | Ref | Frames State | Fault? | Evicted (LRU page) |
|------|-----|--------------|--------|--------------------|
| 1 | 2 | [2, -, -] | ✅ | — |
| 2 | 7 | [2, 7, -] | ✅ | — |
| 3 | 4 | [2, 7, 4] | ✅ | — |
| 4 | 7 | [2, 7, 4] | ❌ | — |
| 5 | 6 | [6, 7, 4] | ✅ | 2 (last used step 1, LRU) |
| 6 | 1 | [6, 7, 1] | ✅ | 4 (last used step 3, LRU) |
| 7 | 7 | [6, 7, 1] | ❌ | — |
| 8 | 6 | [6, 7, 1] | ❌ | — |
| 9 | 1 | [6, 7, 1] | ❌ | — |
| 10 | 2 | [2, 7, 1] | ✅ | 6 (last used step 8, LRU vs 7 step 7 vs 1 step 9) |
| 11 | 7 | [2, 7, 1] | ❌ | — |
| 12 | 2 | [2, 7, 1] | ❌ | — |

**LRU Page Faults = 6**

### Summary
| Algorithm | Page Faults |
|-----------|-------------|
| OPT | **6** |
| LRU | **6** |

---

## Q22b. Address Translation in Paging — Diagram

### Definition
When a process generates a **logical (virtual) address**, the **Memory Management Unit (MMU)** hardware translates it to a **physical address** using the page table.

### Logical Address Structure
```
+-----------------+------------------+
|   Page Number   |     Offset       |
|   (p bits)      |     (d bits)     |
+-----------------+------------------+
```
- **Page Number (p)**: Index into the page table
- **Offset (d)**: Byte offset within the page/frame

### Address Translation Mechanism
```
CPU generates
Logical Address
        │
        ▼
┌───────────────────┐
│   Logical Address  │
│  [Page No | Offset]│
└───────────────────┘
        │
        │  Split Address
        ▼
┌──────────────────────────────┐
│         Page Table            │
│  Page 0 → Frame 3            │
│  Page 1 → Frame 7            │
│  Page 2 → Frame 1   ◄─── Page Number indexes here
│  Page 3 → Frame 9            │
└──────────────────────────────┘
        │
        │  Frame Number found
        ▼
Physical Address = Frame Number × Page Size + Offset
```

### Concrete Example
- Page Size = 4 KB = 4096 bytes
- Logical Address = 12292
  - Page Number = 12292 / 4096 = **3**
  - Offset = 12292 % 4096 = **4** (i.e., 12292 - 3×4096)
- Page Table: Page 3 → Frame 6
- Physical Address = 6 × 4096 + 4 = **24580**

### Role of TLB (Translation Lookaside Buffer)
The **TLB** is a **hardware cache** for the page table. On each memory access:
```
CPU → Check TLB → [Hit]  → Physical Address directly
               → [Miss] → Page Table lookup → update TLB → Physical Address
```
TLB dramatically speeds up address translation (99%+ hit rates typical).

---

*End of CS401 Operating Systems Complete Solution Guide*
*Prepared for R23 Syllabus, SEM-4 | JISCE | 2024-25*
