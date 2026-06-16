# CS403 - Operating Systems: Exam Solutions
### JISCE | UG | CSE | R21 | EVEN | SEM-4 | 2023-24
> **Full Marks: 70 | Time: 3 Hours**
> All solutions include standard definitions, detailed explanations, and examples for exam preparation.

---

## GROUP A — Multiple Choice Questions (10×1 = 10)

---

### Q1.i) Which of the following is **NOT** a function of an Operating System?

**Answer: c) Keyboard input handling**

**Explanation:**

An Operating System (OS) is system software that manages hardware and software resources. Its core functions are:

| Function | Description |
|---|---|
| Process Management | Creating, scheduling, terminating processes |
| Memory Management | Allocating and deallocating RAM |
| File System Management | Creating, reading, writing, deleting files |
| I/O Management | Managing I/O devices via device drivers |
| Security & Protection | Access control, authentication |

**Keyboard input handling** is managed by the **device driver** (part of the I/O subsystem), not directly by the OS kernel as a core function. It is an application-level or driver-level concern, not a fundamental OS responsibility.

> **Example:** Windows OS manages memory allocation for Chrome browser (Memory Management), but keyboard key detection is done by the keyboard driver → passed to the application.

---

### Q1.ii) The default remedy of starvation is:

**Answer: a) Ageing**

**Definition:**

> **Starvation** is a condition where a process waits indefinitely because higher-priority processes keep getting CPU time, leaving low-priority processes never executed.

> **Ageing** is the technique of gradually increasing the priority of a process the longer it waits in the ready queue.

**Why not others?**

| Option | Why Incorrect |
|---|---|
| Critical Section | A region of code for mutual exclusion; unrelated to starvation remedy |
| Mutual Exclusion | A condition for deadlock prevention, not starvation remedy |
| All of these | Only Ageing directly addresses starvation |

**Example:**
```
Process P1 (priority = 1) → waiting since t=0
Process P2, P3... always arrive with priority = 10 → always run first

Without Ageing: P1 starves forever
With Ageing:    At t=50, P1's priority bumped to 5
                At t=100, P1's priority bumped to 10 → finally runs
```

---

### Q1.iii) Which scheduling algorithm allocates CPU for a **fixed time**, then reassigns to another process?

**Answer: a) Round Robin**

**Definition:**

> **Round Robin (RR) Scheduling** is a preemptive CPU scheduling algorithm where each process is assigned a fixed time quantum (time slice). After the quantum expires, the process is preempted and added to the end of the ready queue.

**Key Characteristics:**
- Time Quantum (Q) is fixed (e.g., 4ms, 10ms)
- Circular queue structure
- Best for time-sharing systems
- Fair — no starvation

**Example (Q = 2ms):**
```
Processes: P1(5ms), P2(3ms), P3(1ms)

Gantt Chart:
| P1 | P2 | P3 | P1 | P2 | P1 |
0    2    4    5    7    8   10
```

**Why not others?**

| Algorithm | Behavior |
|---|---|
| SJF | Runs shortest job first; not fixed time |
| Priority Scheduling | Runs highest priority; not fixed time |
| FCFS | Runs in arrival order; non-preemptive |

---

### Q1.iv) Which system call is used to **create a new process**?

**Answer: a) fork()**

**Definition:**

> **fork()** is a system call in Unix/Linux that creates a new process (child process) by duplicating the calling process (parent process). The child is an exact copy of the parent.

**System Call Comparison:**

| System Call | Purpose |
|---|---|
| `fork()` | Creates a new child process (duplicate of parent) |
| `exec()` | Replaces the current process image with a new program |
| `wait()` | Makes parent wait until child process finishes |
| `exit()` | Terminates the current process |

**Example (C-like pseudocode):**
```c
pid = fork();

if (pid == 0) {
    // Child process code
    printf("I am child");
} else {
    // Parent process code
    printf("I am parent, child PID = %d", pid);
    wait();  // Parent waits for child to finish
}
```

> After `fork()`, both parent and child execute the next instruction. They differ only in the return value of `fork()`.

---

### Q1.v) When **External Fragmentation** exists:

**Answer: a) Enough total memory exists to satisfy a request but it is NOT contiguous**

**Definition:**

> **External Fragmentation** occurs when there is enough total free memory to satisfy a request, but the free memory is scattered in small, non-contiguous blocks rather than one large block.

**Visual Example:**
```
Memory State:
[USED 10MB][FREE 4MB][USED 8MB][FREE 3MB][USED 5MB][FREE 5MB]

Total Free = 4 + 3 + 5 = 12MB
Request = 10MB continuous block → FAILS due to external fragmentation
```

**Contrast:**

| Type | Definition | Cause |
|---|---|---|
| External Fragmentation | Free memory is scattered | Variable-size allocation |
| Internal Fragmentation | Allocated memory is larger than needed | Fixed-size partitions |

**Solutions to External Fragmentation:**
- Compaction (shuffle memory to merge free blocks)
- Paging (use non-contiguous allocation)
- Segmentation with paging

---

### Q1.xii) The size of a page is typically:

**Answer: b) Power of 2**

**Definition:**

> **Page size** in memory management is always a power of 2 (e.g., 512B, 1KB, 2KB, 4KB, 8KB) to simplify address translation.

**Why Power of 2?**

Given a logical address, it splits into:
```
Logical Address = Page Number | Page Offset

If page size = 2^n bytes:
  → Last n bits = Offset
  → Remaining bits = Page Number

Example: Page size = 4KB = 2^12 bytes
Logical Address (32-bit): 
  Bits [31:12] = Page Number (20 bits)
  Bits [11:0]  = Offset (12 bits)
```

This bitwise split is **extremely fast** (hardware-efficient), which is why page sizes are always powers of 2.

> Typical page sizes: 4KB (most OS), 2MB or 1GB (huge pages in Linux)

---

## GROUP B — Short Answer Questions (3×5 = 15)

---

### Q2.a) Define Thrashing (2 marks)

**Standard Definition:**

> **Thrashing** is a condition in operating systems where a process spends more time **swapping pages in and out** of memory (paging) than actually **executing** useful instructions. It occurs when the system has too many processes and not enough physical memory frames.

**When does Thrashing occur?**

Thrashing happens when the degree of multiprogramming is **too high** — processes don't have enough frames to hold their active working set, so they continuously page fault.

**Working Set Model:**

> The **Working Set** of a process is the set of pages it actively uses in a recent time window (Δ). If the OS cannot provide enough frames to hold the working set of all processes, thrashing begins.

**Diagram:**
```
CPU Utilization
      |         *
      |       *   *
      |     *       *
      |   *           *
      | *               * (THRASHING zone)
      |___________________
         Degree of Multiprogramming

Optimal point: Peak CPU utilization
Beyond peak: Adding more processes → thrashing → CPU drops
```

**Effects of Thrashing:**
- CPU utilization drops drastically
- System appears frozen or very slow
- High disk I/O activity (constant paging)

**Solutions:**
1. Reduce degree of multiprogramming (swap out some processes)
2. Use Working Set Model to allocate sufficient frames
3. Use Page Fault Frequency (PFF) algorithm
4. Increase physical RAM

**Example:**
```
System has 10 frames total.
5 processes each need 4 frames to run = 20 frames needed.
Only 10 frames available → pages constantly swapped out and in.
Every process page faults immediately → THRASHING.
```

---

### Q2.b) TLB Effective Memory Access Time Calculation (3 marks)

**Given:**
- TLB Hit Ratio (h) = 90% = 0.90
- TLB Access Time = 15 ns
- Main Memory Access Time (m) = 100 ns

**Formula:**

> **Effective Memory Access Time (EMAT)**
>
> EMAT = h × (TLB time + Memory time) + (1 - h) × (TLB time + 2 × Memory time)

**Why 2 × Memory?**

- **TLB Hit:** TLB gives frame number → 1 memory access (for data)
- **TLB Miss:** Must access page table in memory (1st access) + access data (2nd access) = 2 memory accesses

**Calculation:**
```
EMAT = 0.90 × (15 + 100) + 0.10 × (15 + 100 + 100)

     = 0.90 × 115 + 0.10 × 215

     = 103.5 + 21.5

     = 125 nanoseconds
```

**Answer: EMAT = 125 nanoseconds**

**Intuition:**
```
Without TLB: Every access = 100 + 100 = 200 ns (page table in memory + data)
With TLB:    EMAT = 125 ns → ~37.5% speedup
```

---

## GROUP B — Q3: SRTN (Shortest Remaining Time Next) Scheduling

**Given Processes:**

| Process | Arrival Time | Burst Time |
|---|---|---|
| P1 | 2 | 1 |
| P2 | 1 | 5 |
| P3 | 4 | 1 |
| P4 | 0 | 6 |
| P5 | 2 | 3 |

**Definition:**

> **SRTN (Shortest Remaining Time Next)** — also called Preemptive SJF — always runs the process with the **least remaining burst time**. If a new process arrives with a shorter remaining time than the current process, the CPU is preempted.

---

### Q3.a) Gantt Chart for SRTN (2 marks)

**Step-by-step trace:**

```
t=0: Only P4 available (AT=0). Run P4. P4 remaining = 6
t=1: P2 arrives (BT=5). P4 remaining=5, P2 remaining=5 → Tie: continue P4. P4 remaining=4
t=2: P1 (BT=1), P5 (BT=3) arrive. 
     Remaining: P4=4, P2=5, P1=1, P5=3 → P1 is shortest → PREEMPT P4, Run P1
t=3: P1 finishes (BT=1 done).
     Remaining: P4=4, P2=5, P5=3 → P5 shortest → Run P5
t=4: P3 arrives (BT=1). 
     Remaining: P4=4, P2=5, P5=2, P3=1 → P3 shortest → PREEMPT P5, Run P3
t=5: P3 finishes.
     Remaining: P4=4, P2=5, P5=2 → P5 shortest → Run P5
t=7: P5 finishes.
     Remaining: P4=4, P2=5 → P4 shortest → Run P4
t=11: P4 finishes.
     Remaining: P2=5 → Run P2
t=16: P2 finishes. DONE.
```

**Gantt Chart:**
```
| P4 | P4 | P1 | P5 | P3 | P5 | P5 | P4 | P4 | P4 | P4 | P2 | P2 | P2 | P2 | P2 |
0    1    2    3    4    5    6    7    8    9   10   11   12   13   14   15   16
```

Compact form:
```
| P4  | P1 | P5 | P3 | P5  | P4      | P2      |
0     2    3    4    5    7          11         16
```

---

### Q3.b) Average Waiting Time and Turnaround Time (3 marks)

**Definitions:**

> **Turnaround Time (TAT)** = Completion Time − Arrival Time
>
> **Waiting Time (WT)** = Turnaround Time − Burst Time

**Completion Times from Gantt Chart:**

| Process | Arrival | Burst | Completion | TAT = CT − AT | WT = TAT − BT |
|---|---|---|---|---|---|
| P1 | 2 | 1 | 3 | 3 − 2 = **1** | 1 − 1 = **0** |
| P2 | 1 | 5 | 16 | 16 − 1 = **15** | 15 − 5 = **10** |
| P3 | 4 | 1 | 5 | 5 − 4 = **1** | 1 − 1 = **0** |
| P4 | 0 | 6 | 11 | 11 − 0 = **11** | 11 − 6 = **5** |
| P5 | 2 | 3 | 7 | 7 − 2 = **5** | 5 − 3 = **2** |

**Calculations:**
```
Average TAT = (1 + 15 + 1 + 11 + 5) / 5 = 33 / 5 = 6.6 ms

Average WT  = (0 + 10 + 0 + 5 + 2) / 5 = 17 / 5 = 3.4 ms
```

> **Average Turnaround Time = 6.6 ms**
> **Average Waiting Time = 3.4 ms**

---

## GROUP B — Q4: Three Requirements to Solve the Critical Section Problem (5 marks)

**Definition:**

> The **Critical Section** is a segment of code where a process accesses shared resources (e.g., shared variables, files, memory). To prevent race conditions, access must be controlled.

**Three Requirements (Must memorize all three):**

---

#### 1. Mutual Exclusion

> **Definition:** If process Pi is executing in its critical section, no other process can be executing in their critical section simultaneously.

```
At any time:
✅ P1 in CS, P2 outside CS → OK
❌ P1 in CS, P2 in CS → NOT ALLOWED
```

**Example:** Two ATM withdrawals from the same account simultaneously without mutual exclusion can lead to incorrect balance.

---

#### 2. Progress

> **Definition:** If no process is in the critical section and some processes wish to enter, the selection of which process enters next cannot be postponed indefinitely. Only processes NOT in their remainder section can participate in the decision.

- Ensures the system **makes progress** — a decision is always made
- Prevents **deadlock** at the entry to the critical section

**Example:**
```
P1 wants to enter CS, CS is empty.
Progress rule: P1 must be allowed in — cannot wait forever when CS is free.
```

---

#### 3. Bounded Waiting

> **Definition:** There must be a bound (limit) on the number of times other processes can enter the critical section after a process has requested to enter and before that request is granted.

- Prevents **starvation** — no process waits forever to enter CS
- Every requesting process eventually gets CPU time in the CS

**Example:**
```
If 5 processes want CS, each process must enter CS within
at most 4 turns of the other processes (bounded by n-1).
```

---

**Summary Table:**

| Requirement | Prevents | Description |
|---|---|---|
| Mutual Exclusion | Race Condition | Only one process in CS at a time |
| Progress | Deadlock | CS must be given to a waiting process when free |
| Bounded Waiting | Starvation | Limit on how long a process waits |

**Classical Solutions implementing all 3:**
- Peterson's Solution (software)
- Test-and-Set / Compare-and-Swap (hardware)
- Semaphores (OS-level)
- Monitors (high-level language construct)

---

## GROUP B — Q5.a) Page Table Size Calculation (3 marks)

**Given:**
- Physical Memory = 64 MB = 2^26 bytes
- Virtual Address Space = 32-bit = 2^32 bytes
- Page Size = 2 KB = 2^11 bytes

**Formula:**

```
Number of Pages (entries in page table) = Virtual Address Space / Page Size
                                        = 2^32 / 2^11
                                        = 2^21 entries

Each entry stores a frame number.
Number of Frames = Physical Memory / Page Size
                 = 2^26 / 2^11
                 = 2^15 frames

Bits needed per entry = 15 bits ≈ 2 bytes (rounded up)

Page Table Size = Number of entries × Size per entry
               = 2^21 × 2 bytes
               = 2^22 bytes
               = 4 MB
```

> **Approximate size of the Page Table = 4 MB**

---

### Q5.b) Why is Page Size Always a Power of 2? (2 marks)

**Answer:**

Page size is always a power of 2 for **efficient address translation**:

```
Logical Address (32-bit example, page size = 4KB = 2^12):

  | Page Number (20 bits) | Page Offset (12 bits) |
    Bits 31-12              Bits 11-0

Extraction:
  Page Number = Logical Address >> 12   (right shift)
  Offset      = Logical Address & 0xFFF (bitwise AND mask)
```

**Benefits:**
1. **Speed:** Bit shifting and masking are single-cycle operations
2. **No Division:** If page size were not power of 2, we'd need expensive division
3. **Hardware Simplicity:** MMU (Memory Management Unit) circuits are simpler
4. **Alignment:** Memory naturally aligns with powers of 2

> **Example:** Page size = 4KB = 2^12. Address = 0x12345.
> Page No = 0x12345 >> 12 = 0x12 = 18
> Offset  = 0x12345 & 0xFFF = 0x345 = 837

---

## GROUP B — Q6: Page Replacement — FIFO and LRU (5 marks)

**Given:**
- Reference String: 101, 212, 342, 421, 198, 287, 521, 101, 232, 376, 432, 543
- Each page = 100 instructions (pages accessed: 101→page 1, 212→page 2, etc.)
- Number of Frames = 4
- Initially all frames empty

**Convert to Page Numbers (divide by 100, take floor):**
```
101 → Page 1
212 → Page 2
342 → Page 3
421 → Page 4
198 → Page 1
287 → Page 2
521 → Page 5
101 → Page 1
232 → Page 2
376 → Page 3
432 → Page 4
543 → Page 5
```

**Reference String (pages):** 1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5

---

### FIFO Page Replacement

**Definition:**

> **FIFO (First-In First-Out)** replaces the page that has been in memory the longest. A queue is maintained; the oldest page is evicted.

| Step | Page | Frame 1 | Frame 2 | Frame 3 | Frame 4 | Fault? |
|---|---|---|---|---|---|---|
| 1 | 1 | **1** | - | - | - | ✅ Miss |
| 2 | 2 | 1 | **2** | - | - | ✅ Miss |
| 3 | 3 | 1 | 2 | **3** | - | ✅ Miss |
| 4 | 4 | 1 | 2 | 3 | **4** | ✅ Miss |
| 5 | 1 | 1 | 2 | 3 | 4 | ❌ Hit |
| 6 | 2 | 1 | 2 | 3 | 4 | ❌ Hit |
| 7 | 5 | **5** | 2 | 3 | 4 | ✅ Miss (evict 1, oldest) |
| 8 | 1 | 5 | **1** | 3 | 4 | ✅ Miss (evict 2, oldest) |
| 9 | 2 | 5 | 1 | **2** | 4 | ✅ Miss (evict 3, oldest) |
| 10 | 3 | 5 | 1 | 2 | **3** | ✅ Miss (evict 4, oldest) |
| 11 | 4 | **4** | 1 | 2 | 3 | ✅ Miss (evict 5, oldest) |
| 12 | 5 | 4 | **5** | 2 | 3 | ✅ Miss (evict 1, oldest) |

> **FIFO Page Faults = 10**

---

### LRU Page Replacement

**Definition:**

> **LRU (Least Recently Used)** replaces the page that has not been used for the longest period of time. It uses past access history to predict future use.

| Step | Page | Frames (contents) | Fault? | Evicted |
|---|---|---|---|---|
| 1 | 1 | {1} | ✅ Miss | — |
| 2 | 2 | {1,2} | ✅ Miss | — |
| 3 | 3 | {1,2,3} | ✅ Miss | — |
| 4 | 4 | {1,2,3,4} | ✅ Miss | — |
| 5 | 1 | {2,3,4,1} | ❌ Hit | — (1 now most recent) |
| 6 | 2 | {3,4,1,2} | ❌ Hit | — (2 now most recent) |
| 7 | 5 | {4,1,2,5} | ✅ Miss | 3 (LRU) |
| 8 | 1 | {4,2,5,1} | ❌ Hit | — (1 now most recent) |
| 9 | 2 | {4,5,1,2} | ❌ Hit | — |
| 10 | 3 | {5,1,2,3} | ✅ Miss | 4 (LRU) |
| 11 | 4 | {1,2,3,4} | ✅ Miss | 5 (LRU) |
| 12 | 5 | {2,3,4,5} | ✅ Miss | 1 (LRU) |

> **LRU Page Faults = 8**

---

### Comparison Summary

| Algorithm | Page Faults | Notes |
|---|---|---|
| FIFO | **10** | Simple but can suffer Belady's Anomaly |
| LRU | **8** | Better performance; no Belady's Anomaly |

> **LRU performs better** because it uses access history intelligently, while FIFO ignores recency of use.

**Belady's Anomaly:** FIFO can sometimes produce more page faults with more frames. LRU does not suffer from this.

---

## Quick Revision Summary

| Topic | Key Point |
|---|---|
| OS Functions | Process, Memory, File, I/O management (NOT raw keyboard handling) |
| Starvation Remedy | **Ageing** — gradually increase waiting process priority |
| Round Robin | Fixed **time quantum**; circular queue; preemptive |
| fork() | Creates new process in Unix/Linux |
| External Fragmentation | Enough free memory but **not contiguous** |
| Page Size | Always **power of 2** for fast bitwise address splitting |
| Thrashing | More paging than execution; fix = reduce multiprogramming |
| EMAT with TLB | h×(TLB+Mem) + (1-h)×(TLB+2×Mem) |
| SRTN | Preemptive SJF; always run process with least remaining time |
| Critical Section | 3 rules: **Mutual Exclusion, Progress, Bounded Waiting** |
| Page Table Size | (Virtual Space / Page Size) × bytes per entry |
| FIFO | Evict **oldest** loaded page |
| LRU | Evict **least recently used** page |

---
*Prepared for CS403 Operating Systems — Semester Exam 2023-24*
