# CS401 — Operating Systems
## Complete Exam Solution Guide | R23 Syllabus | SEM-4 | JISCE 2024-25

> **How to use this guide:** Every question has a standard definition, a step-by-step solution, one or more worked examples, and memory tips. Read each section fully before moving to the next.

---

# ══════════════════════════════════════════
# PART A — MCQ SOLUTIONS (10 × 1 = 10)
# ══════════════════════════════════════════

---

## Q1. In paging, the page table is used to:
### ✅ Correct Answer: (c) Translate logical addresses to physical addresses

---

### 📖 Standard Definition

**Paging** is a non-contiguous memory management scheme in which:
- The **logical address space** of a process is divided into equal-sized blocks called **pages**.
- **Physical memory (RAM)** is divided into equal-sized blocks called **frames**.
- Page size = Frame size (always equal).
- The **Page Table** is a per-process data structure that stores the mapping: **page number → frame number**.

The CPU generates **logical (virtual) addresses**. The MMU (Memory Management Unit) hardware uses the page table to convert them into **physical addresses** that point to actual RAM locations.

---

### 🔢 Address Translation Formula

```
Logical Address  = (Page Number × Page Size) + Offset
Physical Address = (Frame Number × Page Size) + Offset

Where:
  Page Number  = Logical Address ÷ Page Size   (integer division)
  Offset       = Logical Address mod Page Size
  Frame Number = Page Table[Page Number]
```

---

### 📝 Worked Example

```
Given:
  Page Size       = 4 KB = 4096 bytes
  Logical Address = 8200

Step 1 — Extract Page Number and Offset:
  Page Number = 8200 ÷ 4096 = 2     (floor division)
  Offset      = 8200 mod 4096 = 8

Step 2 — Look up Page Table:
  Page Table[2] = Frame 5

Step 3 — Compute Physical Address:
  Physical Address = (5 × 4096) + 8 = 20480 + 8 = 20488
```

**Memory Map Visualization:**
```
Logical Memory          Page Table          Physical Memory
+----------+            +-------+-------+   +----------+
| Page 0   |  Page 0 → | Frame | Vld   |   | Frame 0  |
+----------+            +-------+-------+   +----------+
| Page 1   |  Page 1 → |   3   |  1    |   | Frame 1  |
+----------+            +-------+-------+   +----------+
| Page 2   |  Page 2 → |   5   |  1    |   | Frame 2  |
+----------+            +-------+-------+   +----------+
| Page 3   |  Page 3 → |   1   |  1    |   | Frame 3  |
+----------+                               +----------+
                                           | Frame 4  |
                                           +----------+
                                           | Frame 5  | ← Page 2 lives here
                                           +----------+
```

---

### ❌ Why Other Options Are Wrong

| Option | Why Incorrect |
|--------|--------------|
| (a) Map physical to logical | Page table is one-way: logical → physical only |
| (b) Allocate memory contiguously | Paging **avoids** contiguous allocation — that is its key advantage |
| (d) Allocate memory segments | Variable-size segments belong to **Segmentation**, not Paging |

---

### 💡 Memory Tip
> "**P**age table = **P**hysical address finder" — it always goes from **P**rocess space to **P**hysical space.

---

## Q2. Which disk scheduling algorithm has the least average seek time in most cases?
### ✅ Correct Answer: (b) SSTF (Shortest Seek Time First)

---

### 📖 Standard Definition

**SSTF (Shortest Seek Time First)** is a disk scheduling algorithm that always services the I/O request whose **cylinder number is closest to the current head position**, minimizing the mechanical movement (seek time) at every individual step.

It works like a greedy algorithm — locally optimal at each step — which generally yields the best overall average seek time compared to FCFS, but is **not globally optimal**.

---

### 📝 Worked Example

```
Current Head Position : 50
Request Queue         : 82, 170, 43, 140, 24, 16, 190

Step-by-step SSTF:
  Head = 50 → Nearest = 43  → Move = |50-43|  = 7
  Head = 43 → Nearest = 24  → Move = |43-24|  = 19
  Head = 24 → Nearest = 16  → Move = |24-16|  = 8
  Head = 16 → Nearest = 82  → Move = |16-82|  = 66
  Head = 82 → Nearest = 140 → Move = |82-140| = 58
  Head = 140→ Nearest = 170 → Move = |140-170|= 30
  Head = 170→ Nearest = 190 → Move = |170-190|= 20

  Total SSTF Movement = 7+19+8+66+58+30+20 = 208 cylinders

FCFS order (82→170→43→140→24→16→190):
  Total FCFS Movement = 32+127+127+116+8+174 = 642 cylinders
```

**SSTF saves 434 cylinders vs FCFS in this example.**

---

### ⚠️ Drawback: Starvation

If requests keep arriving near the current head, far-away requests **may never be served**. Example: head oscillates between cylinders 40–60, and a request at cylinder 190 waits indefinitely.

---

### 📊 Comparison with Other Algorithms

| Algorithm | Avg Seek Time | Starvation? | Use Case |
|-----------|--------------|-------------|----------|
| FCFS | Worst | No | Fairness needed |
| **SSTF** | **Best (generally)** | Yes | Performance priority |
| SCAN | Good | No | General purpose |
| C-SCAN | Good | No | Uniform wait time |
| LOOK | Good | No | Efficient SCAN variant |

---

## Q3. A time-sharing OS allows multiple users to:
### ✅ Correct Answer: (a) Share the CPU at the same time

---

### 📖 Standard Definition

A **Time-Sharing Operating System** is an extension of multiprogramming where CPU time is divided into small slices called **time quanta** (typically 10–100 ms). Each user/process gets a time quantum in rotation. From the user's perspective, the system appears dedicated to them because the switching is fast enough to be imperceptible.

**Key characteristics:**
- Multiple users work **simultaneously and interactively**
- CPU switches rapidly between users → illusion of parallelism
- Response time is short (sub-second)
- Also called **multitasking OS**

---

### 📝 Example

```
4 users (U1, U2, U3, U4), Time Quantum = 100 ms

Timeline:
  0ms   → U1 executes
  100ms → U2 executes
  200ms → U3 executes
  300ms → U4 executes
  400ms → U1 executes again
  ...

Max wait for U1 = 300ms (3 users × 100ms) → appears instantaneous to human
```

---

### Real-World Examples
- Unix/Linux servers with multiple SSH users logged in simultaneously
- University mainframes (historically)
- Modern desktop OS (Windows, macOS) — each app gets time slices

---

### ❌ Why Other Options Are Wrong

| Option | Why Incorrect |
|--------|--------------|
| (b) Access memory without restrictions | Memory protection is enforced; each process has isolated address space |
| (c) Run the same program simultaneously | Different users run different programs |
| (d) Use separate dedicated computers | They share ONE computer |

---

## Q4. What is the purpose of the exec() system call in process management?
### ✅ Correct Answer: (c) Replaces current process image with a new program

---

### 📖 Standard Definition

**`exec()`** is a family of Unix/Linux system calls that **completely replaces** the calling process's memory image — including its code (text), data, heap, and stack — with a new program loaded from disk. After a successful `exec()` call:
- The **PID remains the same** (same process, new program)
- The **old program's code is gone** — it is overwritten
- Execution begins at the `main()` of the new program
- If `exec()` succeeds, it **never returns** to the caller

---

### 📝 Worked Example (C Code)

```c
#include <unistd.h>
#include <stdio.h>

int main() {
    printf("Parent process, PID = %d\n", getpid());

    pid_t pid = fork();  // Create child process

    if (pid == 0) {
        // CHILD process
        printf("Child before exec, PID = %d\n", getpid());

        execl("/bin/ls", "ls", "-l", "/home", NULL);
        // ↑ Replace child's image with "ls" program
        // Everything below this line is NEVER executed if exec succeeds

        printf("This line is never printed!\n");
    } else {
        // PARENT process
        wait(NULL);  // Wait for child to finish
        printf("Parent: child is done.\n");
    }
    return 0;
}
```

**Execution flow:**
```
fork()          exec()           ls runs
Parent ──────────────────────────────────► continues
Child  ──────► [New Image: ls] ──────────► exits
               (PID unchanged)
```

---

### 📊 exec() Family Variants

| Function | Arguments | PATH search? |
|----------|-----------|-------------|
| `execl()` | List of args | No |
| `execv()` | Array of args | No |
| `execlp()` | List + filename | Yes |
| `execvp()` | Array + filename | Yes |
| `execle()` | List + environment | No |

---

### 📊 Process System Calls Comparison

| System Call | What It Does | Returns |
|-------------|-------------|---------|
| `fork()` | Creates a **copy** of current process (child) | 0 to child, child-PID to parent |
| `exec()` | **Replaces** current process with new program | Never returns on success |
| `wait()` | Parent **blocks** until child terminates | Child's PID |
| `exit()` | **Terminates** current process | Never returns |

---

## Q5. What does "thrashing" in memory management imply?
### ✅ Correct Answer: (b) High page fault rate due to frequent swapping

---

### 📖 Standard Definition

**Thrashing** is a pathological condition in virtual memory systems where a process (or the entire system) spends **more time swapping pages in and out of physical memory than actually executing instructions**.

It occurs when:
1. The **degree of multiprogramming** is too high
2. Each process has **too few frames** to hold its working set
3. Every page loaded causes another **needed** page to be evicted
4. The system enters an endless loop of page faults → swap out → swap in → page fault again

**Effect:** CPU utilization **collapses** toward 0% even as the disk is 100% busy.

---

### 📝 How Thrashing Develops (Step-by-Step)

```
Step 1: OS detects low CPU utilization
Step 2: OS loads MORE processes (increases multiprogramming)
Step 3: Each process now gets FEWER frames
Step 4: Page fault rate shoots UP
Step 5: Processes queue up waiting for pages from disk
Step 6: CPU has nothing to run → utilization DROPS further
Step 7: OS (mistakenly) loads even MORE processes → makes it worse
```

---

### 📊 CPU Utilization vs Degree of Multiprogramming

```
CPU Utilization (%)
100 |          ●●●
    |        ●     ●
 75 |      ●         ●
    |    ●             ●  ← Thrashing starts here
 50 |  ●                 ●
    |●                     ●●●●
  0 +─────────────────────────────→
    Low                         High
         Degree of Multiprogramming
```

The curve **rises then crashes** — the crash is thrashing.

---

### 🛠️ Solutions to Thrashing

| Solution | How It Helps |
|----------|-------------|
| **Working Set Model** | Only run processes whose entire working set fits in memory |
| **Page Fault Frequency (PFF)** | If PFF too high → give more frames; if too low → reduce frames |
| **Reduce multiprogramming** | Swap out entire processes to free frames for remaining ones |
| **Increase RAM** | More physical frames = less contention |

---

## Q6. In Round Robin, what happens when a process exceeds its time quantum?
### ✅ Correct Answer: (b) It is moved to the end of the ready queue

---

### 📖 Standard Definition

**Round Robin (RR)** is a **preemptive** CPU scheduling algorithm designed for time-sharing systems. Each process is given a fixed CPU time slice called the **time quantum** (or time slice). When a process's quantum expires:
- The process is **preempted** (forcibly removed from CPU)
- It is placed at the **tail (end) of the ready queue**
- The next process at the head of the ready queue gets the CPU

The scheduler cycles through all ready processes in a circular (round-robin) fashion, giving each a fair share of CPU time.

---

### 📝 Worked Example

```
Processes: P1 (BT=10), P2 (BT=4), P3 (BT=6) | Quantum = 4

Ready Queue at t=0: [P1, P2, P3]

Execution Timeline:
  t= 0– 4: P1 runs (used 4 of 10 ms), remaining = 6ms → moves to end
  t= 4– 8: P2 runs (used 4 of 4 ms), DONE ✓ at t=8
  t= 8–12: P3 runs (used 4 of 6 ms), remaining = 2ms → moves to end
  t=12–16: P1 runs (used 4 of 6 ms), remaining = 2ms → moves to end
  t=16–18: P3 runs (uses remaining 2ms), DONE ✓ at t=18
  t=18–20: P1 runs (uses remaining 2ms), DONE ✓ at t=20

Gantt Chart:
|──P1──|──P2──|──P3──|──P1──|P3|P1|
0      4      8     12     16 18 20
```

---

### 📊 Effect of Quantum Size

| Quantum Size | Behavior | Problem |
|-------------|----------|---------|
| Very small (1ms) | Very responsive | High context switch overhead |
| Very large (∞) | Degenerates into FCFS | Poor response time |
| **Optimal (~10-100ms)** | **Balance of fairness and efficiency** | — |

---

### ❌ Why Other Options Are Wrong

| Option | Why Incorrect |
|--------|--------------|
| (a) It is terminated immediately | Termination only happens at `exit()`, not quantum expiry |
| (c) It is assigned a new, larger quantum | Quantum is fixed in basic RR |
| (d) It executes until completion | That would be FCFS/non-preemptive, not RR |

---

## Q7. Virtual Memory is a part of:
### ✅ Correct Answer: (d) None (of the above)

> **Question options were:** a) Disk storage  b) Main memory  c) Secondary memory  d) None

---

### 📖 Standard Definition

**Virtual Memory** is a **memory management abstraction** — not a physical component. It creates the illusion that each process has access to a large, contiguous address space, regardless of the actual physical RAM available. Internally, it is implemented by using **disk storage** (swap partition / page file) as an extension of RAM, managed transparently by the OS and MMU.

**Why "None"?**
- It is **not** main memory — it may exceed physical RAM by many times
- It is **not** secondary memory — it's not a storage device you directly use
- It is **not** just disk storage — active pages still live in RAM
- It is an **abstraction layer** spanning both RAM and disk

---

### 📊 Virtual Memory Architecture

```
Process sees:
┌──────────────────────────────┐  ← 4 GB virtual address space
│  Stack (grows downward)      │
│  ↓                           │
│                              │
│  ↑                           │
│  Heap (grows upward)         │
│  BSS (uninit data)           │
│  Data segment (init data)    │
│  Text segment (code)         │
└──────────────────────────────┘

Reality:
  Active pages ──► RAM (fast)
  Inactive pages ──► Swap Space on Disk (slow)
  MMU translates virtual → physical transparently
```

---

### 📊 Virtual Address vs Physical Address

| Aspect | Virtual Address | Physical Address |
|--------|-----------------|-----------------|
| Seen by | Process / Programmer | CPU / Hardware |
| Typical size | 4 GB (32-bit), 256 TB (64-bit) | Actual installed RAM |
| Managed by | OS + MMU | Hardware |
| Can exceed RAM? | Yes | No (it IS RAM) |

---

## Q8. Main advantage of message passing in IPC?
### ✅ Correct Answer: (b) It simplifies process synchronization by using explicit messages

---

### 📖 Standard Definition

**Inter-Process Communication (IPC)** allows processes to share data and coordinate actions. **Message Passing** is one of the two primary IPC mechanisms where processes communicate by explicitly **sending and receiving discrete messages** through the OS kernel.

Unlike shared memory, no memory region is shared between processes — all data is **copied** through kernel buffers.

**Two primitives:**
- `send(destination, message)` — sender is blocked or buffered
- `receive(source, &message)` — receiver blocks until a message arrives

---

### 📊 Message Passing vs Shared Memory

| Feature | Message Passing | Shared Memory |
|---------|----------------|---------------|
| Data transfer | Via kernel (copy) | Direct read/write |
| Speed | Slower (kernel overhead) | Faster (no kernel after setup) |
| Synchronization | Automatic (send/receive) | Manual (mutex, semaphore) |
| Race conditions | None (messages are atomic) | Possible (needs locks) |
| Works across network | ✅ Yes (sockets, MPI) | ❌ No (local only) |
| Ease of use | Simple | Complex |

---

### 📝 Example: Producer-Consumer via Message Passing

```
Producer Process                    Consumer Process
─────────────────                   ─────────────────
produce_item(item)                  receive(producer, msg)
send(consumer, item) ──── msg ────► extract item from msg
                                    consume_item(item)

No shared memory needed!
No mutex or semaphore needed!
Synchronization is implicit — consumer blocks until msg arrives.
```

---

### ❌ Why Other Options Are Wrong

| Option | Why Incorrect |
|--------|--------------|
| (a) Allows direct memory sharing | That is Shared Memory, NOT Message Passing |
| (c) No kernel intervention | Message Passing requires kernel to route messages |
| (d) Eliminates scheduling | Scheduling is a separate concern, unrelated to IPC |

---

## Q9. Deadlock can be avoided by:
### ✅ Correct Answer: (d) All of the above

---

### 📖 Standard Definition

**Deadlock** is a situation in which a set of processes are **permanently blocked**, each waiting for a resource held by another process in the set. No process can proceed, and the system is stuck.

**Four necessary conditions (Coffman conditions)** — ALL must hold simultaneously:
1. **Mutual Exclusion** — Resource held by only one process at a time
2. **Hold and Wait** — Process holds at least one resource while waiting for more
3. **No Preemption** — Resources cannot be forcibly taken away
4. **Circular Wait** — P1 waits for P2, P2 waits for P3, ..., Pn waits for P1

---

### 📊 Deadlock Handling Strategies

| Strategy | Method | Description |
|----------|--------|-------------|
| **Prevention** | Eliminate one of 4 conditions | Ensure deadlock can never occur |
| **Avoidance** | Banker's Algorithm | Only allocate if system stays safe |
| **Detection & Recovery** | Resource Allocation Graph | Let deadlock occur, then fix it |
| **Ignorance** | Ostrich Algorithm | Reboot and hope (used by Windows/Linux sometimes) |

---

### 📊 Deadlock Avoidance Techniques (matching the question options)

| Option from Question | Technique Type | How It Works |
|---------------------|---------------|-------------|
| **Wait-Die** | Avoidance (timestamp-based) | Older process waits; younger process dies (aborts and retries later) |
| **Preemption** | Prevention (No Preemption) | Forcibly take a resource from a process holding it |
| **Resource Ordering** | Prevention (Circular Wait) | Assign global number to resources; always request in ascending order |

**Wait-Die vs Wound-Wait:**
```
Let Ti = timestamp (smaller = older)

Wait-Die:
  If Ti < Tj (requester is older): Wait for Tj to release
  If Ti > Tj (requester is younger): Die (abort), restart later

Wound-Wait:
  If Ti < Tj (requester is older): Wound Tj (preempt it)
  If Ti > Tj (requester is younger): Wait
```

**Resource Ordering Example:**
```
Resources: R1 (rank 1), R2 (rank 2), R3 (rank 3)

Rules:
  ✅ Process can request R1 then R2 then R3 (ascending)
  ❌ Process cannot hold R3 and request R1 (would cause circular wait)

This eliminates circular wait → deadlock impossible.
```

---

## Q10. The mechanism that brings a page into memory only when it is needed is called:
### ✅ Correct Answer: (b) Demand Paging

---

### 📖 Standard Definition

**Demand Paging** is a virtual memory strategy in which a page is loaded into physical memory (RAM) **only when it is accessed by the process** — not in advance. If a page is not in memory when accessed, a **page fault** occurs, and the OS fetches that specific page from disk (swap space).

This is a **lazy loading** approach: bring in only what is demanded, when it is demanded.

---

### 📝 Page Fault Handling — Step by Step

```
Process accesses address X
         │
         ▼
    Is page in RAM?
    /             \
  YES              NO → Page Fault Interrupt
   │                         │
   ▼                         ▼
Continue                Trap to OS (Page Fault Handler)
                              │
                              ▼
                    Find a free frame in RAM
                              │
                      (No free frame?)
                         → Run Page Replacement Algorithm
                         → Evict a victim page to disk
                              │
                              ▼
                    Load required page from disk → RAM
                              │
                              ▼
                    Update Page Table entry
                              │
                              ▼
                    Restart the faulting instruction
```

---

### 📊 Demand Paging vs Pre-paging

| Aspect | Demand Paging | Pre-paging |
|--------|--------------|------------|
| Pages loaded | Only when accessed | Predicted pages loaded in advance |
| RAM usage | Minimal | Higher (may load unused pages) |
| Startup speed | Slow (many initial faults) | Faster startup |
| Complexity | Simple | Complex prediction |

---

### ❌ Why Other Options Are Wrong

| Option | Why Incorrect |
|--------|--------------|
| (a) Segmentation | Divides process into logical segments; not about page-on-demand |
| (c) Fragmentation | A problem (wasted memory), not a loading mechanism |
| (d) Swapping | Moves ENTIRE process between RAM and disk; demand paging moves individual pages |

---

## Q11. In a batch OS, how are user programs executed?
### ✅ Correct Answer: (b) In a queue without user interaction

---

### 📖 Standard Definition

A **Batch Operating System** collects user jobs (programs + data + instructions) into **batches** and submits them to the CPU for execution without any direct user interaction. Jobs are queued and processed **sequentially or in order of priority**, and the user receives results only after the batch completes.

**Key characteristics:**
- User **does not interact** with the program during execution
- Similar jobs are grouped to minimize setup time (e.g., all FORTRAN jobs together)
- A special program called the **Job Control Language (JCL) monitor** manages job transitions

---

### 📝 Historical Example

```
1960s Workflow:
  User writes program on punch cards
       ↓
  Submits card deck to operator
       ↓
  Operator groups similar jobs into a batch
       ↓
  Batch loaded into computer → runs sequentially
       ↓
  Results printed → user collects printout hours/days later

Modern Equivalent:
  Payroll processing, bank statement generation, compiler runs on CI/CD pipelines
```

---

### 📊 Batch OS vs Time-Sharing OS vs Real-Time OS

| Feature | Batch OS | Time-Sharing OS | Real-Time OS |
|---------|----------|-----------------|--------------|
| User Interaction | None | Continuous | None / Limited |
| Response Time | Hours/Days | Seconds | Milliseconds/Microseconds |
| CPU Scheduling | Simple FCFS | Round Robin | Priority-based |
| Examples | Payroll, banking batch | Unix, Linux | Airplane autopilot, pacemaker |

---

## Q12. Which of the following is true about multithreading?
### ✅ Correct Answer: (a) Threads share the same memory space

---

### 📖 Standard Definition

A **Thread** (also called a lightweight process) is the **smallest unit of CPU execution** within a process. A single process can contain multiple threads, all of which:
- Share the process's **code segment, data segment, heap, and open file descriptors**
- Have their own **individual stack, registers, and program counter**

This sharing makes threads much faster to create and communicate than separate processes, but requires careful synchronization to avoid race conditions.

---

### 📊 What Threads Share vs What They Own

```
Process (Container)
╔══════════════════════════════════════════════════╗
║  Code Segment (shared by all threads)            ║
║  Data Segment (shared by all threads)            ║
║  Heap (shared by all threads)                    ║
║  Open Files & Sockets (shared)                   ║
╠══════════╦══════════════╦════════════════════════╣
║ Thread 1 ║   Thread 2   ║       Thread 3         ║
║ Stack    ║   Stack      ║       Stack             ║
║ Registers║   Registers  ║       Registers         ║
║ PC       ║   PC         ║       PC                ║
╚══════════╩══════════════╩════════════════════════╝
```

---

### 📊 Thread vs Process Comparison

| Feature | Thread | Process |
|---------|--------|---------|
| Creation time | Fast (microseconds) | Slow (milliseconds) |
| Memory | Shares with siblings | Own address space |
| Communication | Easy (shared memory) | Complex (IPC needed) |
| Context switch | Fast | Slow |
| Crash isolation | One crash can kill all | Independent |

---

### ❌ Why Other Options Are Wrong

| Option | Why Incorrect |
|--------|--------------|
| (b) Threads cannot run concurrently | On multi-core CPUs, threads run **truly in parallel** |
| (c) Threads are heavier than processes | Threads are **lighter** — "lightweight processes" |
| (d) Threads cannot communicate | They can communicate **directly** via shared memory |

---

---

# ══════════════════════════════════════════
# PART B — SHORT ANSWER SOLUTIONS (3 × 5 = 15)
# ══════════════════════════════════════════

---

## Q13. Page Table Size Calculation + Why Page Size is Always a Power of 2

### Given Information
```
Physical Memory         = 64 MB = 2^26 bytes
Virtual Address Space   = 32-bit = 2^32 bytes = 4 GB
Page Size               = 2 KB = 2^11 bytes
```

---

### 📖 Part 1: Page Table Size Calculation

#### Step 1 — Number of Virtual Pages (entries needed in page table)

```
Number of Pages = Virtual Address Space / Page Size
               = 2^32 / 2^11
               = 2^21
               = 2,097,152 pages

Each page needs ONE entry in the page table.
→ Page table must have 2^21 = ~2 million entries.
```

#### Step 2 — Size of Each Page Table Entry

```
Number of Physical Frames = Physical Memory / Page Size
                         = 2^26 / 2^11
                         = 2^15
                         = 32,768 frames

Bits needed to address all frames = log2(32768) = 15 bits

Round up to nearest byte boundary: 15 bits → 2 bytes (16 bits)
(The extra 1 bit is used for Valid/Invalid, Protection, Dirty, etc.)
```

#### Step 3 — Total Page Table Size

```
Page Table Size = Number of Entries × Bytes per Entry
               = 2^21 × 2 bytes
               = 2^22 bytes
               = 4 MB
```

### ✅ Answer: Page Table Size ≈ **4 MB**

---

### 📖 Part 2: Why Page Size is Always a Power of 2

#### Reason 1: Efficient Address Decomposition via Bit Shifting

With a page size of 2^k bytes, the CPU splits a logical address using **bit operations** — no division required:

```
Example: Page Size = 2^11 = 2048 bytes, 32-bit address

Logical Address:  [Bit 31 ──── Bit 11] [Bit 10 ─── Bit 0]
                   ↑                    ↑
                 Page Number (21 bits)  Offset (11 bits)

Hardware operation:
  Page Number = Address >> 11         (right-shift by 11 bits)
  Offset      = Address & 0x7FF       (AND with 0000...011111111111)

If page size = 1000 (not power of 2):
  Page Number = Address / 1000        (expensive integer division)
  Offset      = Address % 1000        (expensive modulo operation)
```

Bit operations execute in **1 CPU cycle**. Division takes **20-80 cycles**. For every memory access in the system, this matters enormously.

#### Reason 2: Perfect Alignment with Hardware

```
Disk sectors   = 512 bytes = 2^9     ✅ Aligns with pages
Cache lines    = 64 bytes  = 2^6     ✅ Aligns with pages
TLB entries    = power of 2 count   ✅ Aligns naturally
RAM modules    = powers of 2 sizes  ✅ No wasted bytes
```

All hardware memory components are powers of 2. Matching page size ensures zero internal waste in alignment.

#### Reason 3: Frame Size = Page Size (Symmetry)

Since pages and frames must be equal in size, if one is a power of 2, both are. This makes frame numbers directly usable as address prefixes.

---

### 📊 Summary

| Reason | Explanation |
|--------|-------------|
| Bit shift instead of division | Hardware speed: single cycle vs 20-80 cycles |
| Hardware alignment | Disk sectors, cache lines, RAM are all 2^n |
| Symmetry with frames | Pages and frames must be same size |

---

## Q14. Banker's Algorithm — What Does the "Need" Matrix Represent?

### 📖 Standard Definition of Banker's Algorithm

The **Banker's Algorithm** (proposed by Edsger Dijkstra, 1965) is a **deadlock avoidance** algorithm that simulates resource allocation and checks whether the resulting state is **safe** before granting any resource request.

The name comes from a bank analogy: A banker (OS) has limited cash (resources). Before giving a loan (resource), the banker checks: "Even if all approved borrowers (processes) demand their full approved limit simultaneously, can I satisfy everyone in some order?" If yes → grant; if no → make the customer wait.

---

### 📊 Data Structures in Banker's Algorithm

| Structure | Type | Meaning |
|-----------|------|---------|
| **n** | Integer | Number of processes |
| **m** | Integer | Number of resource types |
| **Max[n][m]** | Matrix | Maximum demand of each process for each resource type |
| **Allocation[n][m]** | Matrix | Resources currently allocated to each process |
| **Available[m]** | Vector | Number of free instances of each resource type |
| **Need[n][m]** | Matrix | **Remaining resources each process may still request** |

---

### 📖 Need Matrix — Definition

```
Need[i][j] = Max[i][j] − Allocation[i][j]
```

> **The Need matrix tells us: "How many more units of each resource type does process i need at most before it can complete?"**

If process Pi currently holds some resources (Allocation) and its maximum demand is known (Max), then Need = what it still might ask for. This is the **worst-case future demand**.

---

### 📝 Worked Example

Given 5 processes (P0–P4), 3 resource types (A, B, C):

```
         Max          Allocation        Need = Max - Alloc
      A    B    C    A    B    C       A    B    C
P0 [  7    5    3  ][  0    1    0  ] = [  7    4    3  ]
P1 [  3    2    2  ][  2    0    0  ] = [  1    2    2  ]
P2 [  9    0    2  ][  3    0    2  ] = [  6    0    0  ]
P3 [  2    2    2  ][  2    1    1  ] = [  0    1    1  ]
P4 [  4    3    3  ][  0    0    2  ] = [  4    3    1  ]

Available = [3, 3, 2]
```

**Calculation for P0:**
```
Need[P0][A] = Max[P0][A] - Allocation[P0][A] = 7 - 0 = 7
Need[P0][B] = Max[P0][B] - Allocation[P0][B] = 5 - 1 = 4
Need[P0][C] = Max[P0][C] - Allocation[P0][C] = 3 - 0 = 3
→ P0 may still request up to (7, 4, 3) more resources
```

---

### 📖 Safe State Check Using Need Matrix

The system is **safe** if there exists a **safe sequence** — an ordering of all processes such that each process can complete using Available resources (after earlier processes finish and release theirs).

**Safety Algorithm:**

```
Work = Available = [3, 3, 2]
Finish = [false, false, false, false, false]

1. Find i such that: Finish[i] = false AND Need[i] ≤ Work
   P1: Need=[1,2,2] ≤ [3,3,2] ✅
   Work = Work + Allocation[P1] = [3,3,2]+[2,0,0] = [5,3,2]
   Finish[P1] = true

2. Find next i:
   P3: Need=[0,1,1] ≤ [5,3,2] ✅
   Work = [5,3,2]+[2,1,1] = [7,4,3], Finish[P3]=true

3. P4: Need=[4,3,1] ≤ [7,4,3] ✅
   Work = [7,4,3]+[0,0,2] = [7,4,5], Finish[P4]=true

4. P0: Need=[7,4,3] ≤ [7,4,5] ✅
   Work = [7,4,5]+[0,1,0] = [7,5,5], Finish[P0]=true

5. P2: Need=[6,0,0] ≤ [7,5,5] ✅, Finish[P2]=true

Safe Sequence: P1 → P3 → P4 → P0 → P2 ✅ SAFE STATE
```

---

### 💡 Key Insight

The Need matrix is the **heart of Banker's Algorithm**. Without knowing what each process still might request, the OS cannot decide whether granting a resource is safe. The Need matrix provides exactly this "worst-case future demand" information.

---

## Q15. Page Faults — OPT and LRU (Instruction Reference String)

### Given
```
Reference String : 89, 101, 212, 342, 421, 198, 287, 521, 101, 232, 376, 432, 543
Page Size        : 100 instructions per page
Frames           : 4 (all initially empty)
```

---

### Step 1: Convert Instruction Numbers to Page Numbers

Page number = floor(instruction number / page size) = floor(address / 100)

```
89  → floor(89/100)  = 0
101 → floor(101/100) = 1
212 → floor(212/100) = 2
342 → floor(342/100) = 3
421 → floor(421/100) = 4
198 → floor(198/100) = 1
287 → floor(287/100) = 2
521 → floor(521/100) = 5
101 → floor(101/100) = 1
232 → floor(232/100) = 2
376 → floor(376/100) = 3
432 → floor(432/100) = 4
543 → floor(543/100) = 5
```

**Converted Page Reference String: 0, 1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5**

---

### OPT — Optimal Page Replacement

**Rule:** When a page fault occurs and all frames are full, replace the page that will **not be used for the longest time in the future** (or will never be used again).

**Future reference lookup:**
```
Position:  1  2  3  4  5  6  7  8  9 10 11 12 13
Page:       0  1  2  3  4  1  2  5  1  2  3  4  5
```

| Step | Page | Frames After | Fault? | Evicted | Reason |
|------|------|-------------|--------|---------|--------|
| 1 | 0 | [0, –, –, –] | ✅ Yes | — | Empty frame |
| 2 | 1 | [0, 1, –, –] | ✅ Yes | — | Empty frame |
| 3 | 2 | [0, 1, 2, –] | ✅ Yes | — | Empty frame |
| 4 | 3 | [0, 1, 2, 3] | ✅ Yes | — | Empty frame |
| 5 | 4 | [4, 1, 2, 3] | ✅ Yes | **0** | 0 never used again → furthest |
| 6 | 1 | [4, 1, 2, 3] | ❌ No | — | 1 is in frames |
| 7 | 2 | [4, 1, 2, 3] | ❌ No | — | 2 is in frames |
| 8 | 5 | [4, 1, 2, 5] | ✅ Yes | **3** | Next use: 4→step12, 1→step9, 2→step10, 3→step11. 3 is furthest |
| 9 | 1 | [4, 1, 2, 5] | ❌ No | — | 1 is in frames |
| 10 | 2 | [4, 1, 2, 5] | ❌ No | — | 2 is in frames |
| 11 | 3 | [4, 1, 3, 5] | ✅ Yes | **2** | Next use: 4→step12, 1→∞, 2→∞, 5→step13. 2 never used again |
| 12 | 4 | [4, 1, 3, 5] | ❌ No | — | 4 is in frames |
| 13 | 5 | [4, 1, 3, 5] | ❌ No | — | 5 is in frames |

### ✅ OPT Total Page Faults = **7**

---

### LRU — Least Recently Used Page Replacement

**Rule:** When a page fault occurs and frames are full, replace the page that was **used least recently** (farthest back in time).

| Step | Page | Frames After | Fault? | Evicted | LRU Reasoning |
|------|------|-------------|--------|---------|--------------|
| 1 | 0 | [0, –, –, –] | ✅ Yes | — | — |
| 2 | 1 | [0, 1, –, –] | ✅ Yes | — | — |
| 3 | 2 | [0, 1, 2, –] | ✅ Yes | — | — |
| 4 | 3 | [0, 1, 2, 3] | ✅ Yes | — | — |
| 5 | 4 | [4, 1, 2, 3] | ✅ Yes | **0** | Last used: 0→step1, 1→2, 2→3, 3→4. 0 is LRU |
| 6 | 1 | [4, 1, 2, 3] | ❌ No | — | 1 refreshed at step 6 |
| 7 | 2 | [4, 1, 2, 3] | ❌ No | — | 2 refreshed at step 7 |
| 8 | 5 | [4, 1, 2, 5] | ✅ Yes | **3** | Last used: 4→5, 1→6, 2→7, 3→4. 3 is LRU |
| 9 | 1 | [4, 1, 2, 5] | ❌ No | — | 1 refreshed at step 9 |
| 10 | 2 | [4, 1, 2, 5] | ❌ No | — | 2 refreshed at step 10 |
| 11 | 3 | [3, 1, 2, 5] | ✅ Yes | **4** | Last used: 4→5, 1→9, 2→10, 5→8. 4 is LRU |
| 12 | 4 | [3, 4, 2, 5] | ✅ Yes | **1** | Last used: 3→11, 1→9, 2→10, 5→8. 1 is LRU |
| 13 | 5 | [3, 4, 2, 5] | ❌ No | — | 5 is in frames |

### ✅ LRU Total Page Faults = **8**

---

### 📊 Final Summary

| Algorithm | Page Faults | Notes |
|-----------|------------|-------|
| OPT | **7** | Theoretical minimum — requires future knowledge |
| LRU | **8** | Practical approximation of OPT — good real-world performance |

> **OPT is always optimal** (Belady proved this). LRU is the best practical algorithm because it approximates OPT without needing future knowledge.

---

## Q16. Page Faults — LRU, FIFO, OPTIMAL Comparison (3 Frames)

### Given
```
Reference String : 1, 2, 3, 2, 4, 1, 3, 2, 4, 1
Frames           : 3 (initially all empty)
```

---

### Algorithm 1: FIFO (First In, First Out)

**Rule:** Replace the page that has been in memory the **longest** (was loaded earliest).

Track a queue of load order: oldest page = front of queue = victim.

| Ref | Frame 1 | Frame 2 | Frame 3 | Queue (oldest→newest) | Fault? | Evicted |
|-----|---------|---------|---------|----------------------|--------|---------|
| 1 | 1 | — | — | [1] | ✅ | — |
| 2 | 1 | 2 | — | [1,2] | ✅ | — |
| 3 | 1 | 2 | 3 | [1,2,3] | ✅ | — |
| 2 | 1 | 2 | 3 | [1,2,3] | ❌ | — |
| 4 | 4 | 2 | 3 | [2,3,4] | ✅ | **1** (oldest) |
| 1 | 4 | 1 | 3 | [3,4,1] | ✅ | **2** (oldest) |
| 3 | 4 | 1 | 3 | [3,4,1] | ❌ | — |
| 2 | 4 | 1 | 2 | [4,1,2] | ✅ | **3** (oldest) |
| 4 | 4 | 1 | 2 | [4,1,2] | ❌ | — |
| 1 | 4 | 1 | 2 | [4,1,2] | ❌ | — |

### ✅ FIFO Page Faults = **6**

---

### Algorithm 2: LRU (Least Recently Used)

**Rule:** Replace the page that was **used least recently** in time.

Track when each page was last used; evict the one with the earliest last-use time.

| Ref | Frames (set) | Last Used Times | Fault? | Evicted | Reason |
|-----|-------------|-----------------|--------|---------|--------|
| 1 | {1} | 1:t1 | ✅ | — | — |
| 2 | {1,2} | 1:t1, 2:t2 | ✅ | — | — |
| 3 | {1,2,3} | 1:t1, 2:t2, 3:t3 | ✅ | — | — |
| 2 | {1,2,3} | 1:t1, 2:t4, 3:t3 | ❌ | — | 2 refreshed |
| 4 | {2,3,4} | 2:t4, 3:t3, 4:t5 | ✅ | **1** | 1 last used t1 (LRU) |
| 1 | {2,1,4} | 2:t4, 1:t6, 4:t5 | ✅ | **3** | 3 last used t3 (LRU) |
| 3 | {1,4,3} | 1:t6, 4:t5, 3:t7 | ✅ | **2** | 2 last used t4 (LRU) |
| 2 | {1,4,2} | 1:t6, 4:t5, 2:t8 | ✅ | **3?** | Last: 1:t6, 4:t5, 3:t7 → 4 is LRU |

Let me redo cleanly with step numbers:

| Step | Ref | Frames | Fault? | Evicted | LRU = last used at step# |
|------|-----|--------|--------|---------|--------------------------|
| 1 | 1 | {1} | ✅ | — | — |
| 2 | 2 | {1,2} | ✅ | — | — |
| 3 | 3 | {1,2,3} | ✅ | — | — |
| 4 | 2 | {1,2,3} | ❌ | — | 2 refreshed |
| 5 | 4 | {2,3,4} | ✅ | **1** | 1:step1, 2:step4, 3:step3 → 1 is LRU |
| 6 | 1 | {2,1,4} | ✅ | **3** | 2:step4, 3:step3, 4:step5 → 3 is LRU |
| 7 | 3 | {1,3,4} | ✅ | **2** | 2:step4, 1:step6, 4:step5 → 2 is LRU |
| 8 | 2 | {1,3,2} | ✅ | **4** | 1:step6, 3:step7, 4:step5 → 4 is LRU |
| 9 | 4 | {3,2,4} | ✅ | **1** | 1:step6, 3:step7, 2:step8 → 1 is LRU |
| 10 | 1 | {2,4,1} | ✅ | **3** | 3:step7, 2:step8, 4:step9 → 3 is LRU |

### ✅ LRU Page Faults = **8**

*(Note: for this particular string, LRU performs worse than FIFO — this is valid; results depend on the specific reference string)*

---

### Algorithm 3: OPTIMAL (OPT)

**Rule:** Replace the page that will **not be used for the longest time in the future**.

| Step | Ref | Frames | Fault? | Evicted | Future analysis |
|------|-----|--------|--------|---------|-----------------|
| 1 | 1 | {1} | ✅ | — | — |
| 2 | 2 | {1,2} | ✅ | — | — |
| 3 | 3 | {1,2,3} | ✅ | — | — |
| 4 | 2 | {1,2,3} | ❌ | — | — |
| 5 | 4 | {1,4,3} | ✅ | **2** | 1→step6, 2→step8, 3→step7 → 2 is used furthest away |
| 6 | 1 | {1,4,3} | ❌ | — | — |
| 7 | 3 | {1,4,3} | ❌ | — | — |
| 8 | 2 | {1,4,2} | ✅ | **3** | 1→step10, 4→step9, 3→never again → evict 3 |
| 9 | 4 | {1,4,2} | ❌ | — | — |
| 10 | 1 | {1,4,2} | ❌ | — | — |

### ✅ OPT Page Faults = **5**

---

### 📊 Final Comparison

| Algorithm | Page Faults | Rank | Notes |
|-----------|------------|------|-------|
| **OPT** | **5** | 🥇 Best | Theoretical optimal; needs future knowledge |
| **FIFO** | **6** | 🥈 | Simple but can suffer Belady's Anomaly |
| **LRU** | **8** | 🥉 | Usually best practical; bad for this string |

> **Belady's Anomaly:** FIFO can get MORE page faults with MORE frames (paradox). OPT and LRU do not suffer from this.

---

## Q17. CPU Scheduling — FCFS, SJF, Priority, Round Robin

### Given

| Process | Burst Time (ms) | Priority |
|---------|-----------------|----------|
| P1 | 10 | 3 |
| P2 | 1 | 1 |
| P3 | 2 | 3 |
| P4 | 1 | 4 |
| P5 | 5 | 2 |

All processes arrive at time 0. **Lower priority number = Higher priority.**

---

### Key Formulas

```
Completion Time (CT)   = Time when process finishes execution
Turnaround Time (TAT)  = CT − Arrival Time = CT − 0 = CT
Waiting Time (WT)      = TAT − Burst Time
```

---

### Part (a): Gantt Charts

---

#### 1. FCFS — First Come First Served
**Rule:** Execute in order of arrival. No preemption.

Arrival order: P1, P2, P3, P4, P5

```
Gantt Chart:
┌──────────────────────┬──┬────┬──┬──────────┐
│          P1          │P2│ P3 │P4│    P5    │
└──────────────────────┴──┴────┴──┴──────────┘
0                     10 11   13 14          19
```

---

#### 2. SJF — Shortest Job First (Non-Preemptive)
**Rule:** Among all ready processes, run the one with the smallest burst time.

Order by burst time (all arrive at 0): P2(1), P4(1), P3(2), P5(5), P1(10)
Tie between P2 and P4 — break by arrival order (P2 first)

```
Gantt Chart:
┌──┬──┬────┬──────────┬──────────────────────┐
│P2│P4│ P3 │    P5    │          P1          │
└──┴──┴────┴──────────┴──────────────────────┘
0  1  2    4          9                     19
```

---

#### 3. Non-Preemptive Priority (Lower number = Higher priority)
**Rule:** Run the process with highest priority (smallest number) first.

Priority order: P2(1) → P5(2) → P1(3) = P3(3) → P4(4)
Tie between P1 and P3 (both priority 3): break by arrival order → P1 first

```
Gantt Chart:
┌──┬──────────┬──────────────────────┬────┬──┐
│P2│    P5    │          P1          │ P3 │P4│
└──┴──────────┴──────────────────────┴────┴──┘
0  1          6                     16   18 19
```

---

#### 4. Round Robin (Quantum = 1)
**Rule:** Each process runs for 1ms, then next in ready queue runs.

At t=0, queue: [P1, P2, P3, P4, P5]

```
Gantt Chart (Q=1):
┌──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┐
│P1│P2│P3│P4│P5│P1│P3│P5│P1│P5│P1│P5│P1│P1│P1│P1│P1│P1│P1│
└──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┴──┘
0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18  19
```

Completion Times: P2→2, P4→4, P3→7, P5→12, P1→19

---

### Part (b): Turnaround Time (TAT = CT − 0 = CT since all arrive at 0)

| Process | BT | FCFS CT | SJF CT | Priority CT | RR CT |
|---------|----|---------:|-------:|------------:|------:|
| P1 | 10 | 10 | 19 | 16 | 19 |
| P2 | 1 | 11 | 1 | 1 | 2 |
| P3 | 2 | 13 | 4 | 18 | 7 |
| P4 | 1 | 14 | 2 | 19 | 4 |
| P5 | 5 | 19 | 9 | 6 | 12 |
| **Avg TAT** | | **13.4** | **7.0** | **12.0** | **8.8** |

---

### Part (c): Waiting Time (WT = TAT − BT)

| Process | BT | FCFS WT | SJF WT | Priority WT | RR WT |
|---------|----|---------:|-------:|------------:|------:|
| P1 | 10 | 0 | 9 | 6 | 9 |
| P2 | 1 | 10 | 0 | 0 | 1 |
| P3 | 2 | 11 | 2 | 16 | 5 |
| P4 | 1 | 13 | 1 | 18 | 3 |
| P5 | 5 | 14 | 4 | 1 | 7 |
| **Avg WT** | | **9.6** | **3.2** | **8.2** | **5.0** |

---

### Part (d): Which Has Minimum Average Waiting Time?

### ✅ **SJF with Average Waiting Time = 3.2 ms**

**Why SJF minimizes average waiting time (proof by intuition):**
When all processes arrive at the same time, executing the shortest job first means longer jobs wait, but the accumulated wait of ALL processes is minimized. Running the shortest job first reduces the waiting of ALL subsequent processes.

---

### 📊 Algorithm Comparison Summary

| Algorithm | Avg TAT | Avg WT | Preemptive? | Starvation? | Best For |
|-----------|---------|--------|-------------|-------------|----------|
| FCFS | 13.4 | 9.6 | No | No | Simple batch systems |
| **SJF** | **7.0** | **3.2 ✅ MIN** | No | Yes | Batch with known BT |
| Priority | 12.0 | 8.2 | No | Yes | Real-time systems |
| RR | 8.8 | 5.0 | Yes | No | Time-sharing systems |

---

---

# ══════════════════════════════════════════
# PART C — LONG ANSWER SOLUTIONS (3 × 15 = 45)
# ══════════════════════════════════════════

---

## Q18/Q20 — Short Notes

---

### (a) Segmentation

#### 📖 Standard Definition

**Segmentation** is a memory management technique that divides a program into **variable-sized logical units called segments**, each representing a meaningful program unit such as the main function, a procedure, a data structure, the stack, or symbol tables.

Unlike paging (which is a physical division for hardware convenience), segmentation reflects the **programmer's logical view** of a program.

Each segment has:
- A **name** (or number)
- A **length** (limit)
- A **base address** (starting physical address)

These are stored in a **Segment Table**.

---

#### 🔢 Address Translation

```
Logical Address = (Segment Number s, Offset d)

Translation:
  1. Use s to index into Segment Table
  2. Check: d < Limit[s]  (if not → Segmentation Fault)
  3. Physical Address = Base[s] + d
```

```
Logical Address (s=1, d=200)
         │
         ▼
    Segment Table
  ┌────┬──────┬────────┐
  │ s  │ Base │ Limit  │
  ├────┼──────┼────────┤
  │ 0  │ 1000 │  400   │  ← Code segment
  │ 1  │ 2000 │  600   │  ← Data segment  ← look up s=1
  │ 2  │ 3000 │  200   │  ← Stack segment
  └────┴──────┴────────┘
         │
         ▼
  d=200 < Limit=600? YES ✅
  Physical Address = 2000 + 200 = 2200
```

---

#### 📝 Example — Program Segments

```
C Program Segments:
┌──────────────────────────────┐
│ Segment 0: main() code       │ size = 400 bytes
├──────────────────────────────┤
│ Segment 1: global variables  │ size = 600 bytes
├──────────────────────────────┤
│ Segment 2: stack             │ size = 200 bytes
├──────────────────────────────┤
│ Segment 3: sqrt() library    │ size = 300 bytes
└──────────────────────────────┘
```

Each segment can be placed anywhere in physical memory — they need NOT be contiguous with each other.

---

#### 📊 Advantages vs Disadvantages

| Advantages | Disadvantages |
|-----------|--------------|
| Matches programmer's logical view | External fragmentation (variable sizes) |
| Easy sharing of segments (e.g., shared library) | Compaction needed periodically |
| Protection per segment (read-only code) | Segment table overhead |
| No internal fragmentation | Complex allocation algorithms |

---

### (b) Page Table

#### 📖 Standard Definition

A **Page Table** is an OS-maintained per-process data structure that maps each **logical page number** to the corresponding **physical frame number** in RAM. It is the core data structure that enables virtual memory — allowing processes to use large virtual address spaces regardless of actual physical RAM.

---

#### 📊 Page Table Entry (PTE) Structure

```
Each entry in the page table:
┌──────────────────┬───┬───┬───┬──────────────────────┐
│  Frame Number    │ V │ D │ R │   Protection Bits    │
│  (physical addr) │   │   │   │  (R / W / X)         │
└──────────────────┴───┴───┴───┴──────────────────────┘
  V = Valid/Present bit   (1 = page is in RAM, 0 = not in RAM)
  D = Dirty bit           (1 = page was modified, must write back to disk)
  R = Reference bit       (1 = page was recently accessed)
```

---

#### 📊 Types of Page Tables

| Type | Structure | Advantage | Disadvantage |
|------|-----------|-----------|-------------|
| **Single-level** | One flat array | Simple | Huge for 32/64-bit spaces |
| **Two-level (hierarchical)** | Page directory + page tables | Sparse — only allocate used portions | Two memory accesses |
| **Inverted** | One entry per frame (not per page) | Fixed small size | Slow lookup (need hash) |
| **Hashed** | Hash page# → chain of PTEs | Fast for sparse spaces | Hash collision overhead |

---

#### 📝 Two-Level Page Table Example (32-bit, 4KB pages)

```
32-bit Logical Address:
 [Bits 31-22]  [Bits 21-12]  [Bits 11-0]
  Page Dir #    Page Tbl #     Offset
  (10 bits)     (10 bits)      (12 bits)

Translation:
  1. Page Dir # → index into Page Directory → pointer to Page Table
  2. Page Tbl # → index into Page Table     → Frame Number
  3. Physical Address = Frame Number concatenated with Offset
```

---

#### Role of TLB (Translation Lookaside Buffer)

Since every memory access needs a page table lookup (which is itself a memory access), a hardware cache called the **TLB** stores recent page→frame mappings.

```
CPU Address ──► TLB Hit? ──YES──► Physical Address  (1 cycle)
                  │
                  NO
                  │
                  ▼
            Page Table Lookup (1+ extra memory accesses)
                  │
                  ▼
            Update TLB ──► Physical Address
```

TLB hit rate is typically **95-99%**, making effective memory access time nearly as fast as single access.

---

### (c) Compaction

#### 📖 Standard Definition

**Compaction** is a memory management technique that **relocates all currently-loaded processes to one end of memory**, consolidating all scattered free holes into a single large contiguous free block. It is used to eliminate **external fragmentation**.

---

#### 📝 Before and After Compaction

```
BEFORE COMPACTION:                  AFTER COMPACTION:
┌─────────────┐                     ┌─────────────┐
│    OS       │ 100 KB              │    OS       │ 100 KB
├─────────────┤                     ├─────────────┤
│    P1       │ 200 KB              │    P1       │ 200 KB
├─────────────┤                     ├─────────────┤
│   FREE      │  50 KB              │    P2       │ 150 KB
├─────────────┤                     ├─────────────┤
│    P2       │ 150 KB              │    P3       │ 100 KB
├─────────────┤                     ├─────────────┤
│   FREE      │  30 KB              │   FREE      │ 180 KB ← consolidated
├─────────────┤                     └─────────────┘
│    P3       │ 100 KB
├─────────────┤
│   FREE      │ 100 KB
└─────────────┘

Total free: 50+30+100 = 180 KB (scattered)
After compaction: 180 KB in ONE block → usable!
```

---

#### 📊 Compaction Properties

| Property | Detail |
|----------|--------|
| Solves | External fragmentation |
| Cost | Very high — must copy all process data |
| Requires | Relocation support (base register must be updated) |
| When used | When allocation fails and enough total free space exists |
| Alternative | Paging or segmentation with paging (avoids need for compaction) |

---

### (d) Working Set Model

#### 📖 Standard Definition

The **Working Set Model** (proposed by Peter Denning, 1968) is a framework for understanding and managing a process's **locality of reference** — the observation that at any point in time, a process actively uses only a small subset of its pages.

**Working Set W(t, Δ):** The set of pages referenced by a process during the time interval **(t − Δ, t]**, where:
- **t** = current time
- **Δ** = working set window (a fixed number of page references or time units)

---

#### 📝 Example

```
Reference String: 2, 3, 2, 1, 5, 2, 1, 6, 2, 3, 7, 6, 3, 3, 1, ...
Window Δ = 5 page references

At t=10 (after reference #10):
  Last 5 references: 1, 6, 2, 3, 7
  Working Set W(10, 5) = {1, 2, 3, 6, 7} → needs 5 frames

At t=15 (after reference #15):
  Last 5 references: 6, 3, 3, 1, ? 
  Working Set W(15, 5) = {1, 3, 6} → only needs 3 frames
```

---

#### 📖 Working Set and Thrashing Prevention

The OS tracks the working set size (WSS) of each process. Total demand:

```
D = Σ WSSi (sum of working set sizes of all processes)

If D > total available frames → THRASHING IS IMMINENT
  → OS suspends one or more processes (swaps them out entirely)
  → Freed frames distributed to remaining processes
  → System recovers from thrashing

If D < total available frames → Safe to add more processes
```

---

#### 📊 Working Set vs Page Fault Frequency

| Method | How It Works | Advantage |
|--------|-------------|-----------|
| Working Set | Track recently used pages per process | Prevents thrashing proactively |
| PFF (Page Fault Frequency) | Monitor fault rate per process | Reactive — responds to observed faults |

---

### (e) Fragmentation

#### 📖 Standard Definition

**Fragmentation** refers to the **waste of memory** — space that exists but cannot be used for any purpose. It is a fundamental problem in memory management.

There are **two types**:

---

#### Type 1: Internal Fragmentation

**Definition:** Wasted space **inside an allocated memory block** — the allocated block is larger than what the process actually needs.

```
Example:
  Process requests : 13 KB
  System allocates : 16 KB (smallest available fixed block)
  Internal fragmentation = 16 − 13 = 3 KB wasted (inside the block)

  ┌────────────────────┐
  │  Process Data      │ ← 13 KB used
  │  (13 KB)           │
  ├────────────────────┤
  │  WASTED (3 KB)     │ ← Internal fragmentation
  └────────────────────┘
     Total block = 16 KB
```

**Occurs in:** Fixed-size partition allocation, Paging (last page of a process may not be full)

---

#### Type 2: External Fragmentation

**Definition:** Total free memory is sufficient, but it is **scattered in non-contiguous pieces** — no single contiguous block large enough exists.

```
Example:
  Memory layout:
  [OS:100KB][P1:200KB][FREE:50KB][P2:150KB][FREE:30KB][P3:100KB][FREE:100KB]
  
  Total free = 50+30+100 = 180 KB
  Process needs = 120 KB contiguous
  
  ALLOCATION FAILS even though 180 KB is free!
  → External fragmentation = 180 KB (technically available but unusable)
```

**Occurs in:** Variable-size partition allocation, Segmentation

---

#### 📊 Fragmentation Summary

| Type | Location | Occurs In | Solution |
|------|----------|-----------|---------|
| **Internal** | Inside allocated block | Paging, fixed partitions | Smaller page/block sizes |
| **External** | Between allocated blocks | Segmentation, variable partition | Compaction, Paging |

---

## Q18 (contd.) / Q19a — Memory Allocation: First Fit, Best Fit, Worst Fit

### Given (Q18 version)

```
Free Holes (in order) : 15K, 10K, 5K, 25K, 30K, 40K
Processes to allocate : 12K, 2K, 25K, 20K
```

---

### Algorithm 1: First Fit

**Rule:** Scan holes from the beginning; allocate to the **first hole that is large enough**.

```
Holes:    [15K] [10K] [5K] [25K] [30K] [40K]

Process 12K → First hole ≥ 12K is 15K → Allocate. Remaining: 3K
Holes:    [3K] [10K] [5K] [25K] [30K] [40K]

Process 2K → First hole ≥ 2K is 3K → Allocate. Remaining: 1K
Holes:    [1K] [10K] [5K] [25K] [30K] [40K]

Process 25K → Scan: 1K✗, 10K✗, 5K✗, 25K✓ → Allocate. Remaining: 0K
Holes:    [1K] [10K] [5K] [0K] [30K] [40K]

Process 20K → Scan: 1K✗, 10K✗, 5K✗, 0K✗, 30K✓ → Allocate. Remaining: 10K
Holes:    [1K] [10K] [5K] [0K] [10K] [40K]
```

| Process | Hole Used | Hole Size | Leftover (Internal Frag) |
|---------|-----------|-----------|--------------------------|
| 12K | 15K hole | 15K | **3K** |
| 2K | 3K hole (leftover) | 3K | **1K** |
| 25K | 25K hole | 25K | **0K** |
| 20K | 30K hole | 30K | **10K** |

**Internal Fragmentation (First Fit) = 3+1+0+10 = 14K**
**External Fragmentation = 1K + 10K + 5K + 10K + 40K → holes too small = depends on future needs**

---

### Algorithm 2: Best Fit

**Rule:** Scan all holes; allocate to the **smallest hole that is large enough** (minimize leftover).

```
Holes:    [15K] [10K] [5K] [25K] [30K] [40K]

Process 12K → Holes ≥12K: 15K, 25K, 30K, 40K → Smallest = 15K → Allocate. Rem: 3K
Process 2K  → Holes ≥2K: 3K, 10K, 25K, 30K, 40K → Smallest = 3K → Allocate. Rem: 1K
Process 25K → Holes ≥25K: 25K, 30K, 40K → Smallest = 25K → Allocate. Rem: 0K
Process 20K → Holes ≥20K: 30K, 40K → Smallest = 30K → Allocate. Rem: 10K
```

**Result is same as First Fit here** (same order of allocation).
**Internal Fragmentation (Best Fit) = 3+1+0+10 = 14K**

> Best Fit tends to leave many small leftover holes (which themselves become unusable external fragmentation).

---

### Algorithm 3: Worst Fit

**Rule:** Allocate to the **largest hole** (maximize leftover, hoping the large remainder is still useful).

```
Holes:    [15K] [10K] [5K] [25K] [30K] [40K]

Process 12K → Largest = 40K → Allocate. Rem: 28K
Holes:    [15K] [10K] [5K] [25K] [30K] [28K]

Process 2K  → Largest = 30K → Allocate. Rem: 28K
Holes:    [15K] [10K] [5K] [25K] [28K] [28K]

Process 25K → Largest = 28K → Allocate. Rem: 3K
Holes:    [15K] [10K] [5K] [25K] [3K] [28K]

Process 20K → Largest = 28K (last one) → Allocate. Rem: 8K
Holes:    [15K] [10K] [5K] [25K] [3K] [8K]
```

**Internal Fragmentation (Worst Fit) = 28+28+3+8 = 67K**

---

### 📊 Summary Table

| Algorithm | P12K | P2K | P25K | P20K | Internal Frag |
|-----------|------|-----|------|------|--------------|
| First Fit | 15K | 3K rem | 25K | 30K | **14K** |
| Best Fit | 15K | 3K rem | 25K | 30K | **14K** |
| Worst Fit | 40K | 30K | 28K(fr40) | 28K(fr30) | **67K** |

> In this case, **First Fit and Best Fit** are equally efficient. **Worst Fit** wastes the most memory.

---

## Q19a — Memory Partitions: First Fit, Best Fit, Worst Fit

### Given

```
Partitions (in order) : 100 KB, 500 KB, 200 KB, 300 KB, 600 KB
Processes             : 212 KB, 417 KB, 112 KB, 426 KB
```

---

### First Fit

Scan left to right; use the first partition that fits.

| Process | Scan Result | Partition Used | Leftover |
|---------|------------|----------------|----------|
| 212 KB | 100✗ → 500✓ | **500 KB** | 288 KB |
| 417 KB | 100✗ → 288✗ → 200✗ → 300✗ → 600✓ | **600 KB** | 174 KB |
| 112 KB | 100✗ → 288✓ | **288 KB** (leftover) | 176 KB |
| 426 KB | 100✗ → 176✗ → 200✗ → 300✗ → 174✗ → ✗ | **❌ Not allocated** | — |

---

### Best Fit

Find the **smallest partition ≥ process size**.

| Process | Eligible Partitions | Best (Smallest) | Leftover |
|---------|--------------------|-----------------|-|
| 212 KB | 500, 200✗(too small), 300, 600 | **300 KB** | 88 KB |
| 417 KB | 500, 600 | **500 KB** | 83 KB |
| 112 KB | 200, 83✗, 88✗, 100✗, 600 | **200 KB** | 88 KB |
| 426 KB | 600 | **600 KB** | 174 KB |

✅ **All 4 processes allocated under Best Fit!**

---

### Worst Fit

Find the **largest partition** each time.

| Process | Largest Partition | Used | Leftover |
|---------|--------------------|------|---------|
| 212 KB | 600 KB (largest) | **600 KB** | 388 KB |
| 417 KB | 500 KB (next largest) | **500 KB** | 83 KB |
| 112 KB | 388 KB (leftover from 600) | **388 KB** | 276 KB |
| 426 KB | 300✗, 276✗, 200✗, 83✗, 100✗ | **❌ Not allocated** | — |

---

### 📊 Comparison

| Algorithm | 212KB | 417KB | 112KB | 426KB | Efficiency |
|-----------|-------|-------|-------|-------|-----------|
| First Fit | 500KB | 600KB | 288KB rem | ❌ | 3/4 allocated |
| **Best Fit** | **300KB** | **500KB** | **200KB** | **600KB** | **✅ 4/4 — Best!** |
| Worst Fit | 600KB | 500KB | 388KB rem | ❌ | 3/4 allocated |

**✅ Best Fit makes the most efficient use of memory** — successfully allocates all 4 processes.

---

## Q19b — Boot Block and Bad Block

### Boot Block

#### 📖 Standard Definition

The **Boot Block** (also called the **Master Boot Record (MBR)** or **boot sector**) is a **reserved, fixed location on a disk** (typically the very first sector — cylinder 0, head 0, sector 1) that contains the **bootstrap loader** program.

**Purpose:** When the computer is powered on, the CPU has no OS loaded. The BIOS/UEFI firmware reads the boot block into RAM and executes it. The bootstrap loader then locates and loads the operating system kernel from the disk.

```
Power ON
    │
    ▼
ROM BIOS executes
    │
    ▼
BIOS reads Boot Block (first sector of disk) into RAM
    │
    ▼
Bootstrap loader executes
    │
    ▼
OS kernel located and loaded into RAM
    │
    ▼
OS takes control → normal operation begins
```

**Disk Layout:**
```
┌─────────────┬─────────────┬──────────────┬───────────────┬────────────┐
│  Boot Block │ Super Block │ Free Space   │  Inodes       │  Data      │
│  (Sector 0) │             │ Info         │  (metadata)   │  Blocks    │
└─────────────┴─────────────┴──────────────┴───────────────┴────────────┘
```

---

### Bad Block

#### 📖 Standard Definition

A **Bad Block** is a disk sector (or cluster) that is **permanently defective** and cannot reliably store data. Reading from or writing to a bad block produces errors or corrupted data.

**Two types:**
| Type | Description | When Discovered |
|------|-------------|----------------|
| **Hard Bad Block** | Physical surface defect — permanent | At manufacturing time |
| **Soft Bad Block** | Sector develops errors over time | During use (ECC failure) |

---

#### 🛠️ Bad Block Handling Methods

**Method 1: Sector Sparing (Forwarding)**
```
Disk controller maintains a pool of spare sectors.
When a bad block is detected:
  1. Mark it bad in a bad block list
  2. Remap all future accesses to that sector → a spare sector
  3. Transparent to OS — OS never sees the bad block

Bad Block B → Remapped to Spare S1
  All writes/reads to B automatically go to S1
```

**Method 2: Sector Slipping**
```
When a bad block B is detected:
  Shift all sectors AFTER B by one position.
  The sector that was at position B+1 is now at B.
  A spare at the end fills the gap.

Before: [B0][B1][BAD][B3][B4][B5]...[SPARE]
After:  [B0][B1][B3 ][B4][B5][B6]...[rearranged]
```

**Method 3: OS-Level Handling (Unix `fsck`)**
```
OS maintains a list of bad blocks in special file.
File system allocation routines skip bad blocks.
Tools like fsck, badblocks scan and mark them.
```

---

## Q19c — Disk Scheduling Algorithms

### Given
```
Request Queue : 23, 89, 132, 42, 187
Total Cylinders: 200 (numbered 0–199)
Initial Head Position: 100
```

---

### Algorithm 1: FCFS (First Come First Served)

**Rule:** Service requests in the order they arrive.

```
Path: 100 → 23 → 89 → 132 → 42 → 187

Movements:
  |100 - 23|  = 77
  |23  - 89|  = 66
  |89  - 132| = 43
  |132 - 42|  = 90
  |42  - 187| = 145

Total = 77 + 66 + 43 + 90 + 145 = 421 cylinders
```

---

### Algorithm 2: SSTF (Shortest Seek Time First)

**Rule:** At each step, service the request nearest to the current head.

```
Starting at 100, Queue: {23, 89, 132, 42, 187}

Step 1: Head=100, Distances: 23→77, 89→11, 132→32, 42→58, 187→87
        Nearest = 89 → Move 11
Step 2: Head=89,  Distances: 23→66, 132→43, 42→47, 187→98
        Nearest = 132 → Move 43
Step 3: Head=132, Distances: 23→109, 42→90, 187→55
        Nearest = 187 → Move 55
Step 4: Head=187, Distances: 23→164, 42→145
        Nearest = 42 → Move 145
Step 5: Head=42,  Distances: 23→19
        Move to 23 → 19

Path: 100 → 89 → 132 → 187 → 42 → 23
Total = 11 + 43 + 55 + 145 + 19 = 273 cylinders
```

---

### Algorithm 3: SCAN (Elevator Algorithm)

**Rule:** Head moves in one direction, services all requests it passes, reaches the **end of disk**, then reverses direction.

```
Head at 100, moving TOWARD HIGHER cylinders first.
Sorted requests: 23, 42, 89, 132, 187

Going UP (100 → 199):
  100 → 132 (service) → 187 (service) → 199 (end, reverse)
  
Going DOWN (199 → 0):
  199 → 89 (service) → 42 (service) → 23 (service)

Path: 100 → 132 → 187 → 199 → 89 → 42 → 23

Movements:
  |100-132| = 32
  |132-187| = 55
  |187-199| = 12
  |199-89|  = 110
  |89-42|   = 47
  |42-23|   = 19

Total = 32 + 55 + 12 + 110 + 47 + 19 = 275 cylinders
```

---

### Algorithm 4: C-SCAN (Circular SCAN)

**Rule:** Head moves in one direction only, services requests. When it reaches the end, it **immediately returns to the other end** (without servicing on the return trip) and sweeps again.

```
Head at 100, moving TOWARD HIGHER cylinders.

Going UP:  100 → 132 → 187 → 199
Return:    199 → 0  (jump — no service on return)
Going UP:  0 → 23 → 42 → 89

Path: 100 → 132 → 187 → 199 → [jump to 0] → 23 → 42 → 89

Movements:
  |100-132| = 32
  |132-187| = 55
  |187-199| = 12
  [199→0 jump = 199]   (counted as head movement)
  |0-23|    = 23
  |23-42|   = 19
  |42-89|   = 47

Total = 32 + 55 + 12 + 199 + 23 + 19 + 47 = 387 cylinders
```

C-SCAN provides more **uniform wait time** than SCAN (no requests near the outer edge wait for the full sweep return).

---

### Algorithm 5: LOOK (Optimized SCAN)

**Rule:** Like SCAN, but the head only goes **as far as the last request** in each direction (doesn't travel to the physical end of the disk).

```
Head at 100, sorted requests: 23, 42, 89, 132, 187
Moving UP: highest request = 187 (not 199!)

Going UP:  100 → 132 → 187 (reverse here, no need to go to 199)
Going DOWN: 187 → 89 → 42 → 23

Path: 100 → 132 → 187 → 89 → 42 → 23

Movements:
  |100-132| = 32
  |132-187| = 55
  |187-89|  = 98
  |89-42|   = 47
  |42-23|   = 19

Total = 32 + 55 + 98 + 47 + 19 = 251 cylinders
```

---

### 📊 Final Summary — All Algorithms

| Algorithm | Head Path | Total Movement | Notes |
|-----------|-----------|---------------|-------|
| FCFS | 100→23→89→132→42→187 | **421** | Worst |
| SSTF | 100→89→132→187→42→23 | **273** | Fast but starvation risk |
| SCAN | 100→132→187→199→89→42→23 | **275** | Good, uniform |
| C-SCAN | 100→132→187→199→0→23→42→89 | **387** | Most uniform wait |
| **LOOK** | **100→132→187→89→42→23** | **251** | **Best efficiency** |

---

## Q21a — Critical Section Problem: Basic Requirements

### 📖 Standard Definition

A **Critical Section** is a segment of code in which a process accesses **shared resources** (shared variables, files, hardware devices, etc.) that must not be accessed by more than one process at a time. If two or more processes execute their critical sections simultaneously, a **race condition** occurs, potentially corrupting the shared data.

**Structure of a process with a critical section:**
```c
do {
    /* ─── Entry Section ─── */
    // Request permission to enter CS
    // Block if another process is in CS

    /* ─── CRITICAL SECTION ─── */
    // Access/modify shared resources here

    /* ─── Exit Section ─── */
    // Signal that CS is being left
    // Allow waiting processes to enter

    /* ─── Remainder Section ─── */
    // Non-critical code

} while (true);
```

---

### Three Requirements for a Valid CS Solution

#### Requirement 1: Mutual Exclusion

> **Definition:** If process Pi is executing in its critical section, then NO other process can be executing in their critical section at the same time.

```
Timeline showing Mutual Exclusion:

P1: ──[ENTRY]──[███ CRITICAL SECTION ███]──[EXIT]──────────────
P2: ──────────────────────[BLOCKED]────────[ENTRY]──[██ CS ██]──

                          ↑                ↑
                     P2 wants to enter  P1 exits, P2 can now enter
```

This is the **core requirement**. Without it, race conditions corrupt shared data.

**Example of violation:**
```
Shared variable: count = 5
P1 reads count → gets 5
P2 reads count → gets 5
P1 increments → writes 6
P2 increments → writes 6  ← WRONG! Should be 7
```

---

#### Requirement 2: Progress

> **Definition:** If no process is executing in its critical section and some processes wish to enter, then only processes that are NOT in their remainder section can participate in the decision of which process will enter the critical section next. This selection cannot be postponed indefinitely.

**In simpler terms:** The system must always make progress. If the CS is free and processes want to enter, **someone must be allowed in** — the decision cannot be deferred forever.

**Example of violation (Progress violated):**
```
P1 and P2 alternate turns: P1 → P2 → P1 → P2 ...
P1 runs once and is done (in remainder section forever).
P2 wants to enter but waits forever for P1's "turn".
CS is free, but P2 can never enter → Progress violated.
```

---

#### Requirement 3: Bounded Waiting

> **Definition:** There exists a **bound on the number of times** other processes are allowed to enter their critical sections after a process has made a request to enter its critical section and before that request is granted.

**In simpler terms:** No process should wait forever to enter the CS. Every process must eventually get its turn — **starvation is forbidden**.

**Example:**
```
P1 makes request at t=10
P2 and P3 may each be allowed to enter the CS at most K times
before P1 is guaranteed entry.

If K = 2: P2 can go twice, P3 can go twice, then P1 MUST go.
```

---

### 📊 Summary of Requirements

| Requirement | What It Prevents | Key Word |
|-------------|-----------------|---------|
| Mutual Exclusion | Race conditions, data corruption | Safety |
| Progress | System deadlock / indefinite postponement | Liveness |
| Bounded Waiting | Starvation of individual processes | Fairness |

---

### Solutions and Whether They Satisfy All 3

| Solution | Mutual Excl. | Progress | Bounded Wait |
|----------|-------------|----------|-------------|
| Peterson's Algorithm | ✅ | ✅ | ✅ |
| Bakery Algorithm (n processes) | ✅ | ✅ | ✅ |
| Mutex Lock | ✅ | ✅ | ✅ (with fair queuing) |
| Semaphore (Binary) | ✅ | ✅ | ✅ (with fair queuing) |
| Simple flag variable | ✅ | ❌ | ❌ |
| Turn variable only | ❌ | ✅ | ✅ |

---

## Q21b — Deadlock Detection from Resource Allocation Graph

### Given
```
P1 → R1 → P2 → R2 → P3 → R3 → P1
```

### Step 1: Interpret the Notation

In a **Resource Allocation Graph (RAG)**:
- **P → R** means: Process P has **requested** resource R (P is waiting for R)
- **R → P** means: Resource R is **allocated** to process P (R is held by P)

Parsing the given chain:

| Edge | Type | Meaning |
|------|------|---------|
| P1 → R1 | Request edge | P1 is **waiting** for R1 |
| R1 → P2 | Assignment edge | R1 is **held** by P2 |
| P2 → R2 | Request edge | P2 is **waiting** for R2 |
| R2 → P3 | Assignment edge | R2 is **held** by P3 |
| P3 → R3 | Request edge | P3 is **waiting** for R3 |
| R3 → P1 | Assignment edge | R3 is **held** by P1 |

---

### Step 2: Draw the Wait-For Graph

Eliminate resources (only show which process waits for which process):

```
P1 waits for R1 → R1 held by P2 → P1 waits for P2
P2 waits for R2 → R2 held by P3 → P2 waits for P3
P3 waits for R3 → R3 held by P1 → P3 waits for P1

Wait-For Graph:
    P1 ──────► P2
    ▲            │
    │            ▼
    P3 ◄───── P3? 

Wait-For edges:
  P1 → P2 → P3 → P1   ← THIS IS A CYCLE!
```

```
RAG Visualization:

   P1 ──request──► R1 ──held by──► P2
   ▲                                 │
   │                               request
   │                                 │
   held by                           ▼
   │                                 R2
   R3                                │
   ▲                              held by
   │                                 │
   request                           ▼
   │                                P3 ─────────────────┘
   └────────────────────────────────
```

---

### Step 3: Deadlock Conclusion

A **cycle** in the RAG is:
- **Necessary** condition for deadlock (with multiple instances per resource type)
- **Sufficient** condition for deadlock when **each resource type has exactly ONE instance**

Since each of R1, R2, R3 appears to have **one instance** (the problem specifies one edge from each), the cycle is sufficient to confirm deadlock.

### ✅ Conclusion: **The system IS in deadlock**

P1, P2, and P3 are all permanently blocked:
- P1 waits for P2 to release R1
- P2 waits for P3 to release R2
- P3 waits for P1 to release R3
- **None can release; none can proceed**

---

## Q21c — Banker's Algorithm: Full System Snapshot

### Given Table

| Process | Allocation (A,B,C) | Max (A,B,C) | Available |
|---------|---------------------|-------------|-----------|
| P0 | 0  1  0 | 7  5  3 | A=3, B=3, C=2 |
| P1 | 2  0  0 | 3  2  2 | |
| P2 | 3  0  2 | 9  0  2 | |
| P3 | 2  1  1 | 4  2  2 | |
| P4 | 0  0  2 | 5  3  3 | |

---

### Part (a): Total Instances of Each Resource Type

**Formula:** Total = Sum of all Allocations + Available

```
Resource A:
  Allocated: 0+2+3+2+0 = 7
  Available: 3
  Total A = 7 + 3 = 10 instances

Resource B:
  Allocated: 1+0+0+1+0 = 2
  Available: 3
  Total B = 2 + 3 = 5 instances

Resource C:
  Allocated: 0+0+2+1+2 = 5
  Available: 2
  Total C = 5 + 2 = 7 instances
```

### ✅ Total Resources: **A = 10, B = 5, C = 7**

---

### Part (b): Need Matrix

**Formula:** Need[i] = Max[i] − Allocation[i] (computed per resource type)

```
P0: Need = [7-0, 5-1, 3-0] = [7, 4, 3]
P1: Need = [3-2, 2-0, 2-0] = [1, 2, 2]
P2: Need = [9-3, 0-0, 2-2] = [6, 0, 0]
P3: Need = [4-2, 2-1, 2-1] = [2, 1, 1]
P4: Need = [5-0, 3-0, 3-2] = [5, 3, 1]
```

| Process | Need A | Need B | Need C |
|---------|--------|--------|--------|
| P0 | **7** | **4** | **3** |
| P1 | **1** | **2** | **2** |
| P2 | **6** | **0** | **0** |
| P3 | **2** | **1** | **1** |
| P4 | **5** | **3** | **1** |

---

### Part (c): Is the System in a Safe State?

**Safety Algorithm:**

```
Work = Available = [3, 3, 2]
Finish = {P0:F, P1:F, P2:F, P3:F, P4:F}
Safe Sequence = []
```

**Iteration 1:** Find process with Need ≤ Work AND Finish=false
```
P0: Need=[7,4,3] ≤ [3,3,2]? NO (7>3)
P1: Need=[1,2,2] ≤ [3,3,2]? YES ✅
  → Run P1: Work = [3,3,2] + Allocation[P1]=[2,0,0] = [5,3,2]
  → Finish[P1] = true
  → Safe Sequence = [P1]
```

**Iteration 2:**
```
P0: Need=[7,4,3] ≤ [5,3,2]? NO (7>5, 4>3)
P2: Need=[6,0,0] ≤ [5,3,2]? NO (6>5)
P3: Need=[2,1,1] ≤ [5,3,2]? YES ✅
  → Run P3: Work = [5,3,2] + [2,1,1] = [7,4,3]
  → Finish[P3] = true
  → Safe Sequence = [P1, P3]
```

**Iteration 3:**
```
P0: Need=[7,4,3] ≤ [7,4,3]? YES ✅ (all equal counts)
P4: Need=[5,3,1] ≤ [7,4,3]? YES ✅ (P4 also fits!)
  → Run P4 first (either works; pick P4):
     Work = [7,4,3] + [0,0,2] = [7,4,5]
  → Finish[P4] = true
  → Safe Sequence = [P1, P3, P4]
```

**Iteration 4:**
```
P0: Need=[7,4,3] ≤ [7,4,5]? YES ✅
  → Run P0: Work = [7,4,5] + [0,1,0] = [7,5,5]
  → Finish[P0] = true
  → Safe Sequence = [P1, P3, P4, P0]
```

**Iteration 5:**
```
P2: Need=[6,0,0] ≤ [7,5,5]? YES ✅
  → Run P2: Finish[P2] = true
  → Safe Sequence = [P1, P3, P4, P0, P2]
```

All processes have Finish = true. ✅

### ✅ Safe Sequence: **P1 → P3 → P4 → P0 → P2**
### ✅ The system IS in a **Safe State**

---

## Q22a — Page Faults: OPT and LRU

### Given
```
Reference String : 2, 7, 4, 7, 6, 1, 7, 6, 1, 2, 7, 2
Frames           : 3
```

---

### OPT — Optimal Page Replacement

**Rule:** Evict the page whose **next use is furthest in the future** (or never used again).

**Pre-compute next use of each page at every step:**

```
Reference:   2   7   4   7   6   1   7   6   1   2   7   2
Position:    1   2   3   4   5   6   7   8   9  10  11  12

At step 5, frames contain {2,7,6}:
  Page 2 → next use at step 10
  Page 7 → next use at step 7
  Page 6 → next use at step 8
  Evict page 2 (used furthest away)
```

| Step | Ref | Frame 1 | Frame 2 | Frame 3 | Fault? | Evicted | Why |
|------|-----|---------|---------|---------|--------|---------|-----|
| 1 | 2 | **2** | — | — | ✅ | — | Empty |
| 2 | 7 | 2 | **7** | — | ✅ | — | Empty |
| 3 | 4 | 2 | 7 | **4** | ✅ | — | Empty |
| 4 | 7 | 2 | 7 | 4 | ❌ | — | 7 already in frame |
| 5 | 6 | 2 | 7 | **6** | ✅ | **4** | Next use: 2→10, 7→7, 4→never → evict 4 |
| 6 | 1 | **1** | 7 | 6 | ✅ | **2** | Next use: 2→10, 7→7, 6→8 → evict 2 (furthest) |
| 7 | 7 | 1 | 7 | 6 | ❌ | — | 7 in frame |
| 8 | 6 | 1 | 7 | 6 | ❌ | — | 6 in frame |
| 9 | 1 | 1 | 7 | 6 | ❌ | — | 1 in frame |
| 10 | 2 | 1 | 7 | **2** | ✅ | **6** | Next use: 1→never, 7→11, 6→never → evict 1 or 6 (tie; pick 6) |
| 11 | 7 | 1 | 7 | 2 | ❌ | — | 7 in frame |
| 12 | 2 | 1 | 7 | 2 | ❌ | — | 2 in frame |

### ✅ OPT Total Page Faults = **6** (steps 1,2,3,5,6,10)

---

### LRU — Least Recently Used

**Rule:** Evict the page whose **last access was earliest** (least recently used).

Track the last-used time of each page in frames.

| Step | Ref | Frame 1 | Frame 2 | Frame 3 | Fault? | Evicted | LRU Reasoning |
|------|-----|---------|---------|---------|--------|---------|---------------|
| 1 | 2 | **2**(t=1) | — | — | ✅ | — | Empty |
| 2 | 7 | 2(t=1) | **7**(t=2) | — | ✅ | — | Empty |
| 3 | 4 | 2(t=1) | 7(t=2) | **4**(t=3) | ✅ | — | Empty |
| 4 | 7 | 2(t=1) | **7**(t=4) | 4(t=3) | ❌ | — | 7 refreshed |
| 5 | 6 | 2(t=1) | 7(t=4) | **6**(t=5) | ✅ | **4** | Last used: 2:t1, 7:t4, 4:t3 → 2 is LRU... wait: 4 was last at t3, 2 at t1 → evict 2? No — 2 at t1 is LRU |

Let me redo step 5 carefully:
```
Frames before step 5: {2(last:t1), 7(last:t4), 4(last:t3)}
LRU = 2 (last used at t=1, earliest) → evict 2
```

| Step | Ref | Frames | Fault? | Evicted | LRU (last-used times) |
|------|-----|--------|--------|---------|----------------------|
| 1 | 2 | {2} | ✅ | — | 2:t1 |
| 2 | 7 | {2,7} | ✅ | — | 2:t1, 7:t2 |
| 3 | 4 | {2,7,4} | ✅ | — | 2:t1, 7:t2, 4:t3 |
| 4 | 7 | {2,7,4} | ❌ | — | 2:t1, 7:t4, 4:t3 |
| 5 | 6 | {6,7,4} | ✅ | **2** | 2:t1 is LRU. New: 6:t5, 7:t4, 4:t3 |
| 6 | 1 | {6,7,1} | ✅ | **4** | 4:t3 is LRU. New: 6:t5, 7:t4, 1:t6 |
| 7 | 7 | {6,7,1} | ❌ | — | 6:t5, 7:t7, 1:t6 |
| 8 | 6 | {6,7,1} | ❌ | — | 6:t8, 7:t7, 1:t6 |
| 9 | 1 | {6,7,1} | ❌ | — | 6:t8, 7:t7, 1:t9 |
| 10 | 2 | {6,7,2} | ✅ | **1?** | 6:t8, 7:t7, 1:t9 → 7:t7 is LRU → evict 7 |

Wait, 7:t7 < 6:t8 < 1:t9, so 7 is LRU at step 10:

| Step | Ref | Frames | Fault? | Evicted | Reasoning |
|------|-----|--------|--------|---------|-----------|
| 10 | 2 | {6,2,1} | ✅ | **7** | 7 last used t7 (LRU among 6:t8, 7:t7, 1:t9) |
| 11 | 7 | {6,2,7} | ✅ | **1** | 1 last used t9 (LRU among 6:t8, 2:t10, 1:t9)? No: 6:t8 < 1:t9 < 2:t10 → evict 6 |

Let me be precise:
At step 11, frames = {6(t8), 2(t10), 1(t9)}, accessing 7:
- LRU = 6 (last used t8, the earliest)
- Evict 6

| 11 | 7 | {7,2,1} | ✅ | **6** | 6:t8 is LRU (t8 < t9 < t10) |
| 12 | 2 | {7,2,1} | ❌ | — | 2 in frame (refreshed at t12) |

### ✅ LRU Total Page Faults = **7** (steps 1,2,3,5,6,10,11)

---

### 📊 Final Summary

| Algorithm | Page Faults | Comparison |
|-----------|------------|-----------|
| **OPT** | **6** | Theoretical minimum |
| **LRU** | **7** | Near-optimal in practice |

**Difference of only 1 page fault** — LRU closely approximates OPT.

---

## Q22b — Address Translation Mechanism of Paging (with Diagram)

### 📖 Standard Definition

When a process generates a **logical (virtual) address**, it cannot be used directly to access physical RAM. The **Memory Management Unit (MMU)** — a hardware component — intercepts every memory access and translates logical addresses to physical addresses using the **page table** stored in memory.

---

### Step 1: Structure of a Logical Address

```
For a system with:
  Virtual address space = 2^n bytes
  Page size = 2^p bytes

Logical Address (n bits total):
┌─────────────────────────┬──────────────────────┐
│     Page Number (p)     │    Page Offset (d)   │
│  (n-p bits) high bits   │   (p bits) low bits  │
└─────────────────────────┴──────────────────────┘

Example: 32-bit address, 4KB (2^12) pages:
  Bits [31:12] → Page Number  (20 bits → up to 1M pages)
  Bits [11:0]  → Offset       (12 bits → 0 to 4095)
```

---

### Step 2: Address Translation Diagram

```
CPU
 │
 │  generates Logical Address
 │  e.g., 0x00003004  →  Page#=3, Offset=4
 ▼
┌──────────────────────────────────────────────┐
│              MMU (Hardware)                  │
│                                              │
│   Logical Address = [Page# = 3 | Offset = 4]│
│                          │                  │
│                          │                  │
│                    Page Table                │
│               (in RAM, per process)         │
│         ┌─────┬──────────┬──────────┐       │
│         │ Pg# │  Frame#  │ Valid(V) │       │
│         ├─────┼──────────┼──────────┤       │
│         │  0  │    5     │    1     │       │
│         │  1  │    3     │    1     │       │
│         │  2  │    9     │    1     │       │
│         │  3  │    6     │    1     │◄──┐   │
│         │  4  │    —     │    0     │   │   │
│         └─────┴──────────┴──────────┘   │   │
│                    Page# = 3  ───────────┘  │
│                    Frame# = 6               │
│                          │                  │
│   Physical Address = Frame# concatenated    │
│                      with Offset            │
│                    = 6 × 4096 + 4           │
│                    = 24580 = 0x6004         │
└──────────────────────────────────────────────┘
 │
 ▼
Physical Memory (RAM)
  Frame 6, Byte 4 → data fetched
```

---

### Step 3: TLB (Translation Lookaside Buffer) Enhancement

Without optimization, every memory access requires **two** memory accesses:
1. Access page table (in RAM) to get frame number
2. Access actual data (in RAM) using physical address

The **TLB** is a **small, fast, associative cache** (typically 64–1024 entries) that stores recent page→frame mappings.

```
CPU Address Generation
         │
         ▼
   ┌─────────────┐
   │   TLB       │ ← Hardware cache (very fast, ~1 cycle)
   │  Lookup     │
   └─────────────┘
     /          \
   HIT           MISS
    │              │
    │         Page Table Lookup (main RAM, 50-200 cycles)
    │              │
    │         Update TLB with new entry
    │              │
    └──────────────┘
         │
         ▼
  Physical Address found
         │
         ▼
   Access main memory
```

**Effective Memory Access Time (EMAT) with TLB:**
```
EMAT = (hit_rate × TLB_time) + (miss_rate × (TLB_time + Page_Table_time + Memory_time))

Example:
  TLB access time     = 10 ns
  Memory access time  = 100 ns
  TLB hit rate        = 95% = 0.95

EMAT = 0.95 × (10 + 100) + 0.05 × (10 + 100 + 100)
     = 0.95 × 110 + 0.05 × 210
     = 104.5 + 10.5
     = 115 ns   (vs 210 ns without TLB — 45% faster)
```

---

### 📊 Summary of Paging Address Translation

| Step | Action | Where |
|------|--------|-------|
| 1 | CPU generates logical address (page#, offset) | CPU |
| 2 | Check TLB for page# → frame# mapping | TLB cache |
| 3 (hit) | Use frame# directly | — |
| 3 (miss) | Access page table in RAM → get frame# | Main RAM |
| 4 | Form physical address = frame# × page_size + offset | MMU |
| 5 | Access physical address in RAM | Main RAM |

---

---

*End of CS401 Operating Systems Complete Exam Solution Guide*

---

**Quick Reference — Formulas to Remember**

```
Physical Address  = Frame# × Page Size + Offset
Page Number       = Logical Address ÷ Page Size   (floor)
Offset            = Logical Address mod Page Size
Page Table Size   = (Virtual Space / Page Size) × Entry Size
Frames in RAM     = Physical Memory / Page Size

Turnaround Time   = Completion Time − Arrival Time
Waiting Time      = Turnaround Time − Burst Time
Response Time     = First CPU Start − Arrival Time

Need[i]           = Max[i] − Allocation[i]
Total Resources   = Sum(Allocation) + Available

Disk Head Movement = Sum of |current - next| for each step
```

---
*Prepared for R23 Syllabus | B.Tech CSE SEM-4 | JISCE | 2024-25*
*Last updated: June 2025*
