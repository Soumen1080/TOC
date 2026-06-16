# Operating Systems — Complete Semester Preparation Guide
## Detailed Solutions: 5-Mark & 15-Mark Questions

---

# MODULE 1 — Introduction to Operating Systems

---

## Q1 (5 Marks): Explain the main functionalities of an Operating System with suitable examples.

### Definition
> **Operating System (OS):** A system software that acts as an intermediary between computer hardware and the user. It manages hardware resources and provides an environment in which programs can run efficiently and safely.

### Main Functionalities

#### 1. Process Management
The OS creates, schedules, and terminates processes. It decides which process gets the CPU and for how long.
- **Example:** When you open Chrome and Notepad simultaneously, the OS manages both as separate processes, allocating CPU time to each.

#### 2. Memory Management
The OS allocates and deallocates memory to processes, ensuring no two processes use the same memory space.
- **Example:** When a program crashes, the OS reclaims its memory so other programs are unaffected.

#### 3. File System Management
The OS organizes files into directories, controls access, and handles reading/writing operations.
- **Example:** Windows NTFS and Linux ext4 are file systems managed by their respective OSes.

#### 4. Device (I/O) Management
The OS uses device drivers to communicate with hardware like printers, keyboards, and hard drives.
- **Example:** When you print a document, the OS sends data to the printer driver, which communicates with the hardware.

#### 5. Security and Protection
The OS prevents unauthorized access to resources using user accounts, permissions, and access control lists.
- **Example:** In Linux, a file owned by root cannot be modified by a regular user unless permissions are granted.

#### 6. User Interface
The OS provides either a Command Line Interface (CLI) or Graphical User Interface (GUI) for user interaction.
- **Example:** Linux terminal (CLI), Windows Desktop (GUI).

#### 7. Networking
Modern OSes manage network connections, protocols, and data sharing between computers.
- **Example:** The OS handles TCP/IP communication when you browse the internet.

---

## Q2 (5 Marks): Compare Batch OS and Time-Sharing OS.

### Batch Operating System

> **Definition:** A type of OS where jobs (programs) are collected in groups (batches) and executed one after another without user interaction.

**Characteristics:**
- Jobs are submitted in bulk
- No direct user interaction during execution
- CPU stays busy as long as there are jobs in the batch
- **Example:** IBM's early mainframe OS (OS/360), payroll processing systems

### Time-Sharing Operating System

> **Definition:** A type of OS that allows multiple users to share the CPU simultaneously by rapidly switching between tasks using a concept called "time slices" or "quantum."

**Characteristics:**
- Multiple users interact simultaneously
- Each user gets a small time slice
- Response time is very fast (milliseconds)
- **Example:** UNIX, Linux multi-user systems

### Comparison Table

| Feature | Batch OS | Time-Sharing OS |
|---|---|---|
| User Interaction | None during execution | Continuous interaction |
| Response Time | Minutes to hours | Milliseconds to seconds |
| Multiple Users | No | Yes |
| CPU Utilization | High | High |
| Example Use | Payroll, billing | Web servers, university systems |
| Complexity | Simple | Complex |
| Cost | Low | High |

---

## Q3 (5 Marks): Explain Protection and Security Mechanisms in Operating System.

### Definitions
> **Protection:** Mechanisms that control access of processes and users to resources defined by the OS.
> **Security:** Defence of the system against internal and external attacks (e.g., viruses, unauthorized access).

### Protection Mechanisms

#### 1. Access Control
Each resource (file, memory, device) has an Access Control List (ACL) that specifies which users/processes can perform which operations (read, write, execute).
- **Example:** In Linux, `chmod 644 file.txt` gives read-write to owner, read-only to others.

#### 2. Domain of Protection
The OS defines protection domains — a set of (object, access-rights) pairs. A process operates within a domain.
- **Example:** User mode vs Kernel mode — processes in user mode cannot directly access hardware.

#### 3. Dual-Mode Operation
- **User Mode:** Restricted — processes cannot directly execute privileged instructions.
- **Kernel Mode:** Unrestricted — OS kernel runs with full hardware access.
- **Example:** A user process calls `read()` → triggers a system call → switches to kernel mode → OS reads disk → switches back.

#### 4. Memory Protection
Base and limit registers define the legal address range for each process. Any access outside this range triggers an interrupt.

### Security Mechanisms

#### 1. Authentication
Verifying the identity of users via passwords, biometrics, or tokens.

#### 2. Encryption
Data is encrypted so even if intercepted, it cannot be read without a key.

#### 3. Firewalls and Antivirus
The OS or associated software monitors traffic and files for threats.

#### 4. Auditing / Logging
The OS logs all security-relevant events for later analysis.

---

## Q4 (15 Marks — Module 1, Part a): Evolution and Types of Operating Systems

### Evolution of Operating Systems

#### 1. Serial Processing (1940s–50s)
- No OS; programmers directly interacted with hardware.
- Programs were loaded manually using punch cards and switches.
- Problems: Wasted setup time, no multitasking.

#### 2. Batch Systems (1950s–60s)
- Jobs collected and run in batches without user interaction.
- Operators handled job submission.
- Problem: CPU idle while waiting for I/O.

#### 3. Multiprogrammed Systems (1960s)
- Multiple jobs loaded into memory simultaneously.
- When one job waits for I/O, CPU switches to another.
- Increased CPU utilization significantly.

#### 4. Time-Sharing Systems (1970s)
- Multiple users interact simultaneously through terminals.
- Each user gets a small CPU time slice.
- **Example:** UNIX (1969), C language developed alongside.

#### 5. Personal Computer OS (1980s)
- Single-user OSes like MS-DOS, early Mac OS.
- GUI introduced — made computers accessible to non-technical users.

#### 6. Modern OS (1990s–Present)
- Multitasking, networking, GUI, security — all combined.
- **Examples:** Windows NT, Linux kernel, macOS, Android, iOS.

### Types of Operating Systems

| Type | Description | Example |
|---|---|---|
| **Batch OS** | Processes jobs in batches without user interaction | IBM OS/360 |
| **Time-Sharing OS** | Multiple users share CPU in time slices | UNIX |
| **Distributed OS** | Multiple computers appear as a single system | Amoeba, Plan 9 |
| **Real-Time OS** | Guarantees response within strict time limits | VxWorks, FreeRTOS |
| **Embedded OS** | Runs on embedded hardware with limited resources | Android (phones), RTOS in washing machines |
| **Network OS** | Provides network services to clients | Novell NetWare |
| **Multiprocessor OS** | Manages multiple CPUs | Windows Server, Linux SMP |

---

## Q5 (15 Marks — Module 1, Part b): OS Structure + Protection & Security

### OS Structural Overview

The OS can be designed using different structural approaches:

#### 1. Monolithic Structure (Simple)
All OS functions are combined into one large kernel.
- Fast but difficult to modify/debug.
- **Example:** Early UNIX, MS-DOS.

```
[User Programs]
      ↓
[Single-Layer OS Kernel]
      ↓
[Hardware]
```

#### 2. Layered Approach
OS divided into layers; each layer uses functions only from the layer below it.
```
Layer 5 — User Programs
Layer 4 — I/O Management
Layer 3 — CPU Scheduling
Layer 2 — Memory Management
Layer 1 — Hardware Abstraction
Layer 0 — Hardware
```

#### 3. Microkernel
Only essential functions (IPC, basic scheduling) in the kernel. Everything else (file system, device drivers) runs in user space.
- More secure and easier to maintain.
- **Example:** Mach, QNX, minix.

#### 4. Modular (Modern Approach)
Core kernel with dynamically loadable modules.
- **Example:** Linux kernel — can load/unload drivers as modules.

---

# MODULE 2 — Process Management

---

## Q6 (5 Marks): Explain different states of a process with a neat diagram.

### Definition
> **Process:** A program in execution. It is an active entity, unlike a program which is a passive set of instructions stored on disk.

### Process States

#### 1. New
The process is being created (program loaded from disk into memory).

#### 2. Ready
The process is in memory, waiting to be assigned to the CPU. All necessary resources are available except the CPU.

#### 3. Running
The process is currently being executed by the CPU.

#### 4. Waiting (Blocked)
The process is waiting for some event (like I/O completion) before it can continue.

#### 5. Terminated
The process has finished execution. OS reclaims its resources.

### Process State Diagram

```
              admitted
  [New] ──────────────────→ [Ready]
                               ↑  ↑
                    interrupt  |  | scheduler dispatch
                               |  |
                            [Running] ──→ [Terminated]
                               |
                   I/O or event wait
                               ↓
                           [Waiting]
                               |
                   I/O or event completion
                               ↓
                            [Ready]
```

### Transitions Explained

| Transition | Cause |
|---|---|
| New → Ready | Process creation complete |
| Ready → Running | CPU scheduler selects this process |
| Running → Ready | Time quantum expires (preemption) |
| Running → Waiting | Process requests I/O or waits for event |
| Waiting → Ready | I/O completed or event occurred |
| Running → Terminated | Process finishes execution |

---

## Q7 (5 Marks): Differentiate between User-Level Threads and Kernel-Level Threads.

### Thread Definition
> **Thread:** The smallest unit of CPU utilization within a process. A process can have multiple threads sharing the same code, data, and resources but each with its own program counter, registers, and stack.

### User-Level Threads (ULT)

- Managed entirely by a user-level thread library (e.g., POSIX Pthreads)
- Kernel is unaware of their existence
- Thread switching does not require kernel intervention → fast
- **Problem:** If one thread makes a blocking system call, the entire process blocks

### Kernel-Level Threads (KLT)

- Managed directly by the OS kernel
- Kernel creates, schedules, and manages each thread
- **Advantage:** If one thread blocks, other threads in the same process can continue
- **Disadvantage:** Thread switching involves kernel mode switch → slower

### Comparison Table

| Feature | User-Level Threads | Kernel-Level Threads |
|---|---|---|
| Management | User thread library | OS Kernel |
| Speed | Faster (no kernel call) | Slower (kernel call needed) |
| Blocking | Blocks entire process | Only one thread blocks |
| Portability | Portable across OS | OS-specific |
| Example | POSIX Pthreads (in user space) | Windows threads, Linux pthreads |
| Awareness | Kernel unaware | Kernel aware |

### Thread Models

1. **Many-to-One:** Many user threads → one kernel thread. Simple but no parallelism.
2. **One-to-One:** Each user thread → one kernel thread. True parallelism; used in Linux/Windows.
3. **Many-to-Many:** Many user threads → many (but ≤) kernel threads. Best of both worlds.

---

## Q8 (5 Marks): Explain FCFS and SJF Scheduling Algorithms.

### CPU Scheduling Definition
> **CPU Scheduling:** The process of determining which process in the ready queue will be allocated the CPU next.

---

### FCFS — First Come First Served

> **Definition:** Processes are executed in the order they arrive in the ready queue. It is non-preemptive.

**Example:** P1 (BT=5), P2 (BT=3), P3 (BT=8) — all arrive at time 0.

```
Gantt Chart:
| P1 | P2 |    P3    |
0    5    8          16
```

- WT: P1=0, P2=5, P3=8 → Average WT = (0+5+8)/3 = **4.33 ms**

**Advantages:**
- Simple to implement
- No starvation

**Disadvantages:**
- **Convoy Effect:** Short processes wait behind long ones
- Not optimal average waiting time

---

### SJF — Shortest Job First

> **Definition:** The process with the shortest burst time is executed next. Can be preemptive (SRTF) or non-preemptive.

**Example:** P1(BT=6), P2(BT=8), P3(BT=7), P4(BT=3) — all arrive at t=0.

Order of execution: P4(3), P1(6), P3(7), P2(8)

```
Gantt Chart:
| P4 |  P1  |   P3   |    P2    |
0    3      9       16          24
```

- WT: P4=0, P1=3, P3=9, P2=16 → Average WT = (0+3+9+16)/4 = **7 ms**

**Advantages:**
- Minimum average waiting time (provably optimal for non-preemptive)

**Disadvantages:**
- Cannot know burst time in advance (estimation needed)
- Long processes may starve

---

## Q9 (5 Marks Numerical): FCFS — P1(AT=0,BT=5), P2(AT=1,BT=3), P3(AT=2,BT=8), P4(AT=3,BT=6)

### Solution

FCFS is non-preemptive; processes run in order of arrival.

**Gantt Chart:**
```
| P1  |  P2  |        P3        |    P4    |
0     5      8                  16         22
```

**Step-by-step:**
- P1 starts at 0, ends at 5
- P2 starts at 5, ends at 8
- P3 starts at 8, ends at 16
- P4 starts at 16, ends at 22

**Calculations:**

| Process | AT | BT | CT | TAT = CT−AT | WT = TAT−BT |
|---|---|---|---|---|---|
| P1 | 0 | 5 | 5 | 5 | 0 |
| P2 | 1 | 3 | 8 | 7 | 4 |
| P3 | 2 | 8 | 16 | 14 | 6 |
| P4 | 3 | 6 | 22 | 19 | 13 |

- **Average Waiting Time = (0 + 4 + 6 + 13) / 4 = 23/4 = 5.75 ms**
- **Average Turnaround Time = (5 + 7 + 14 + 19) / 4 = 45/4 = 11.25 ms**

---

## Q10 (5 Marks Numerical): Round Robin with TQ=3 — P1=10ms, P2=5ms, P3=8ms (all AT=0)

### Solution

Time Quantum = 3 ms

**Execution Sequence:**

| Time | Running | Remaining: P1 | P2 | P3 |
|---|---|---|---|---|
| 0–3 | P1 | 7 | 5 | 8 |
| 3–6 | P2 | 7 | 2 | 8 |
| 6–9 | P3 | 7 | 2 | 5 |
| 9–12 | P1 | 4 | 2 | 5 |
| 12–14 | P2 | 4 | 0 ✓ | 5 |
| 14–17 | P3 | 4 | — | 2 |
| 17–20 | P1 | 1 | — | 2 |
| 20–22 | P3 | 1 | — | 0 ✓ |
| 22–23 | P1 | 0 ✓ | — | — |

**Gantt Chart:**
```
|P1|P2|P3|P1|P2|P3|P1|P3|P1|
0  3  6  9 12 14 17 20 22 23
```

| Process | AT | BT | CT | TAT | WT |
|---|---|---|---|---|---|
| P1 | 0 | 10 | 23 | 23 | 13 |
| P2 | 0 | 5 | 14 | 14 | 9 |
| P3 | 0 | 8 | 22 | 22 | 14 |

- **Average WT = (13+9+14)/3 = 36/3 = 12 ms**
- **Average TAT = (23+14+22)/3 = 59/3 = 19.67 ms**

---

## Q11 (15 Marks — Module 2): Process States + PCB + FCFS Numerical

### Part a (7M): Process States and PCB

*(See Q6 above for Process States diagram)*

### Process Control Block (PCB)

> **Definition:** A data structure maintained by the OS for every process. It contains all the information needed to manage a process.

#### PCB Contents:

| Field | Description |
|---|---|
| Process ID (PID) | Unique identifier |
| Process State | New, Ready, Running, Waiting, Terminated |
| Program Counter | Address of next instruction to execute |
| CPU Registers | Contents of all CPU registers (saved on context switch) |
| CPU Scheduling Info | Priority, scheduling queue pointers |
| Memory Management Info | Base/limit registers, page tables |
| Accounting Info | CPU time used, time limits |
| I/O Status Info | List of open files, I/O devices assigned |

### Context Switching
When the OS switches CPU from one process to another, it saves the current process's PCB and loads the next process's PCB. This is called a **context switch**.

---

### Part b (8M): FCFS Numerical — P1(AT=0,BT=4), P2(AT=1,BT=3), P3(AT=2,BT=5), P4(AT=3,BT=2)

**Gantt Chart:**
```
| P1 |P2 |   P3  |P4|
0    4   7       12  14
```

| Process | AT | BT | CT | TAT=CT−AT | WT=TAT−BT |
|---|---|---|---|---|---|
| P1 | 0 | 4 | 4 | 4 | 0 |
| P2 | 1 | 3 | 7 | 6 | 3 |
| P3 | 2 | 5 | 12 | 10 | 5 |
| P4 | 3 | 2 | 14 | 11 | 9 |

- **Average WT = (0+3+5+9)/4 = 17/4 = 4.25 ms**
- **Average TAT = (4+6+10+11)/4 = 31/4 = 7.75 ms**

---

## Q12 (15 Marks — Module 2): IPC + SJF Numerical

### Part a (7M): Inter-Process Communication using Message Passing

> **IPC (Inter-Process Communication):** Mechanisms that allow processes to communicate and synchronize their actions.

### Message Passing

In message passing, processes communicate by sending and receiving messages without sharing memory.

#### Operations:
- `send(message)` — sends a message to destination
- `receive(message)` — receives a message from source

#### Direct Communication:
Processes name each other explicitly.
```
send(P2, message)   // P1 sends to P2
receive(P1, message) // P2 receives from P1
```

#### Indirect Communication (Mailboxes / Ports):
Messages are sent to and received from mailboxes (logical channels).
```
send(mailbox_A, message)
receive(mailbox_A, message)
```

#### Synchronous vs Asynchronous:
| Type | Blocking Send | Blocking Receive |
|---|---|---|
| **Synchronous** | Sender waits until receiver gets the message | Receiver waits until a message arrives |
| **Asynchronous** | Sender sends and continues | Receiver gets message or null |

**Real-World Example:** Pipes in UNIX — `ls | grep .txt` sends output of `ls` as a message to `grep`.

---

### Part b (8M): SJF Numerical — P1=6ms, P2=8ms, P3=7ms, P4=3ms (all AT=0)

Order by burst time: P4(3) → P1(6) → P3(7) → P2(8)

**Gantt Chart:**
```
|P4 |   P1   |    P3    |     P2     |
0   3        9         16           24
```

| Process | AT | BT | CT | TAT=CT−AT | WT=TAT−BT |
|---|---|---|---|---|---|
| P4 | 0 | 3 | 3 | 3 | 0 |
| P1 | 0 | 6 | 9 | 9 | 3 |
| P3 | 0 | 7 | 16 | 16 | 9 |
| P2 | 0 | 8 | 24 | 24 | 16 |

- **Average WT = (0+3+9+16)/4 = 28/4 = 7 ms**
- **Average TAT = (3+9+16+24)/4 = 52/4 = 13 ms**

---

## Q13 (15 Marks — Module 2): Threads + Round Robin Numerical

### Part a (7M): Benefits of Threads and Thread Models

#### Benefits of Threads:

1. **Responsiveness:** A multi-threaded program remains responsive even if one thread is blocked (e.g., browser loads page while allowing user input).

2. **Resource Sharing:** Threads share memory and resources of the process, making communication easier than between separate processes.

3. **Economy:** Creating a thread is much cheaper than creating a new process (less memory, less time).

4. **Scalability:** Threads can run in parallel on multi-processor systems, improving performance.

5. **Faster Context Switch:** Switching between threads is faster than between processes.

#### Thread Models (Many-to-One, One-to-One, Many-to-Many): *(See Q7 above)*

---

### Part b (8M): Round Robin TQ=3 — P1=10ms, P2=4ms, P3=5ms, P4=8ms (all AT=0)

**Execution Sequence:**

| Slot | Process | Time | P1 Rem | P2 Rem | P3 Rem | P4 Rem |
|---|---|---|---|---|---|---|
| 1 | P1 | 0–3 | 7 | 4 | 5 | 8 |
| 2 | P2 | 3–6 | 7 | 1 | 5 | 8 |
| 3 | P3 | 6–9 | 7 | 1 | 2 | 8 |
| 4 | P4 | 9–12 | 7 | 1 | 2 | 5 |
| 5 | P1 | 12–15 | 4 | 1 | 2 | 5 |
| 6 | P2 | 15–16 | 4 | 0✓ | 2 | 5 |
| 7 | P3 | 16–18 | 4 | — | 0✓ | 5 |
| 8 | P4 | 18–21 | 4 | — | — | 2 |
| 9 | P1 | 21–24 | 1 | — | — | 2 |
| 10 | P4 | 24–26 | 1 | — | — | 0✓ |
| 11 | P1 | 26–27 | 0✓ | — | — | — |

**Gantt Chart:**
```
|P1|P2|P3|P4|P1|P2|P3|P4|P1|P4|P1|
0  3  6  9 12 15 16 18 21 24 26 27
```

| Process | BT | CT | TAT | WT |
|---|---|---|---|---|
| P1 | 10 | 27 | 27 | 17 |
| P2 | 4 | 16 | 16 | 12 |
| P3 | 5 | 18 | 18 | 13 |
| P4 | 8 | 26 | 26 | 18 |

- **Average WT = (17+12+13+18)/4 = 60/4 = 15 ms**
- **Average TAT = (27+16+18+26)/4 = 87/4 = 21.75 ms**

---

## Q14 (15 Marks — Module 2): Preemptive vs Non-Preemptive + Priority Scheduling

### Part a (7M): Preemptive vs Non-Preemptive Scheduling

#### Non-Preemptive Scheduling
> **Definition:** Once a process starts executing, it cannot be stopped until it either completes or voluntarily gives up the CPU (e.g., I/O wait).

- Simple to implement
- Better for batch systems
- **Examples:** FCFS, SJF (non-preemptive), Priority (non-preemptive)

#### Preemptive Scheduling
> **Definition:** The OS can forcibly take away the CPU from a running process and assign it to another higher-priority or shorter process.

- Better for interactive systems
- More complex (need to handle context switching)
- **Examples:** Round Robin, SRTF, Priority (preemptive)

| Feature | Non-Preemptive | Preemptive |
|---|---|---|
| CPU Takeover | Not possible mid-execution | Possible |
| Context Switch | Less frequent | More frequent |
| Response Time | Poorer | Better |
| Overhead | Less | More |
| Starvation | Less likely | Possible |
| Best for | Batch systems | Interactive systems |

---

### Part b (8M): Priority Scheduling Numerical

**Processes:** P1(BT=10, Pri=3), P2(BT=1, Pri=1), P3(BT=2, Pri=4), P4(BT=1, Pri=5), P5(BT=5, Pri=2)
*(Lower number = Higher priority; all arrive at t=0)*

Priority Order (highest first): P2(1) → P5(2) → P1(3) → P3(4) → P4(5)

**Gantt Chart:**
```
|P2|   P5   |      P1      |P3|P4|
0  1        6             16 18 19
```

| Process | BT | Priority | CT | TAT | WT |
|---|---|---|---|---|---|
| P2 | 1 | 1 | 1 | 1 | 0 |
| P5 | 5 | 2 | 6 | 6 | 1 |
| P1 | 10 | 3 | 16 | 16 | 6 |
| P3 | 2 | 4 | 18 | 18 | 16 |
| P4 | 1 | 5 | 19 | 19 | 18 |

- **Average WT = (0+1+6+16+18)/5 = 41/5 = 8.2 ms**

---

## Additional Numericals (Module 2)

### SJF — P1=24ms, P2=3ms, P3=3ms (all AT=0)

Order: P2(3) → P3(3) → P1(24)

```
|P2|P3|        P1        |
0  3  6                  30
```

| Process | BT | CT | TAT | WT |
|---|---|---|---|---|
| P2 | 3 | 3 | 3 | 0 |
| P3 | 3 | 6 | 6 | 3 |
| P1 | 24 | 30 | 30 | 6 |

- **Average WT = (0+3+6)/3 = 3 ms**
- **Average TAT = (3+6+30)/3 = 13 ms**

---

### SRTF — P1(AT=0,BT=8), P2(AT=1,BT=4), P3(AT=2,BT=9), P4(AT=3,BT=5)

SRTF = Preemptive SJF. At each time unit, check if new arrival has shorter remaining time.

| Time | Remaining: P1 | P2 | P3 | P4 | Running |
|---|---|---|---|---|---|
| 0 | 8 | — | — | — | P1 |
| 1 | 7 | 4 | — | — | P2 (4<7) |
| 2 | 7 | 3 | 9 | — | P2 (3 shortest) |
| 3 | 7 | 2 | 9 | 5 | P2 (2 shortest) |
| 4 | 7 | 1 | 9 | 5 | P2 (1 shortest) |
| 5 | 7 | 0✓ | 9 | 5 | P4 (5 shortest) |
| 6–9 | 7 | — | 9 | 4→1→0✓ | P4 finishes at 10 |

Wait — let me do this carefully:

**Gantt Chart:** P1(0–1), P2(1–5), P4(5–10), P1(10–17), P3(17–26)

| Process | AT | BT | CT | TAT | WT |
|---|---|---|---|---|---|
| P1 | 0 | 8 | 17 | 17 | 9 |
| P2 | 1 | 4 | 5 | 4 | 0 |
| P3 | 2 | 9 | 26 | 24 | 15 |
| P4 | 3 | 5 | 10 | 7 | 2 |

- **Average WT = (9+0+15+2)/4 = 26/4 = 6.5 ms**
- **Average TAT = (17+4+24+7)/4 = 52/4 = 13 ms**

---

### RR TQ=4 — P1=6ms, P2=8ms, P3=7ms, P4=3ms (all AT=0)

**Execution:**

| Slot | Process | Time | P1 | P2 | P3 | P4 |
|---|---|---|---|---|---|---|
| 1 | P1 | 0–4 | 2 | 8 | 7 | 3 |
| 2 | P2 | 4–8 | 2 | 4 | 7 | 3 |
| 3 | P3 | 8–12 | 2 | 4 | 3 | 3 |
| 4 | P4 | 12–15 | 2 | 4 | 3 | 0✓ |
| 5 | P1 | 15–17 | 0✓ | 4 | 3 | — |
| 6 | P2 | 17–21 | — | 0✓ | 3 | — |
| 7 | P3 | 21–24 | — | — | 0✓ | — |

**Gantt Chart:**
```
|  P1  |  P2  |  P3  |P4|P1|  P2  |P3|
0      4      8     12 15 17     21  24
```

| Process | BT | CT | TAT | WT |
|---|---|---|---|---|
| P1 | 6 | 17 | 17 | 11 |
| P2 | 8 | 21 | 21 | 13 |
| P3 | 7 | 24 | 24 | 17 |
| P4 | 3 | 15 | 15 | 12 |

- **Average TAT = (17+21+24+15)/4 = 77/4 = 19.25 ms**
- **Average WT = (11+13+17+12)/4 = 53/4 = 13.25 ms**
# Operating Systems — Semester Preparation Guide
## Part 2: Modules 3, 4 & 5

---

# MODULE 3 — Process Synchronization & Deadlocks

---

## Q15 (5 Marks): Critical Section Problem and Two Synchronization Solutions

### Critical Section — Definition
> **Critical Section:** A segment of code in which a process accesses shared resources (shared variables, files, I/O devices). Only ONE process should be in its critical section at a time to prevent race conditions.

### Structure of a Process (Critical Section Framework)

```
do {
    [Entry Section]      ← Request permission to enter CS
    
    [Critical Section]   ← Access shared resource
    
    [Exit Section]       ← Release permission
    
    [Remainder Section]  ← Other code
} while (true);
```

### Requirements for a Valid Solution

1. **Mutual Exclusion:** Only one process can be in its critical section at a time.
2. **Progress:** If no process is in the CS and some want to enter, selection cannot be postponed indefinitely.
3. **Bounded Waiting:** There must be a limit on how many times other processes can enter CS before a waiting process is allowed in (no starvation).

### Solution 1: Peterson's Algorithm (Two-Process Solution)

```c
// Shared variables
int turn;           // whose turn it is
bool flag[2];       // flag[i] = true means Pi wants to enter CS

// Process Pi's code:
flag[i] = true;
turn = j;           // Give turn to the other process
while (flag[j] && turn == j); // Busy wait

// --- CRITICAL SECTION ---

flag[i] = false;    // Exit section

// --- REMAINDER SECTION ---
```

**Why it works:** Satisfies all three conditions — mutual exclusion, progress, bounded waiting.

---

### Solution 2: Semaphores

> **Semaphore:** An integer variable accessed only through two atomic operations — `wait()` (P) and `signal()` (V).

```
wait(S):            signal(S):
  while S <= 0        S = S + 1
    ; // busy wait
  S = S - 1
```

**Using Binary Semaphore for Critical Section:**
```
semaphore mutex = 1;

Process Pi:
  wait(mutex);
  // CRITICAL SECTION
  signal(mutex);
  // REMAINDER SECTION
```

---

## Q16 (5 Marks): Semaphores and Producer-Consumer Problem

### Types of Semaphores

| Type | Value Range | Use |
|---|---|---|
| **Binary Semaphore** (Mutex) | 0 or 1 | Mutual exclusion |
| **Counting Semaphore** | Any non-negative integer | Resource counting |

### Producer-Consumer (Bounded Buffer) Problem

> **Problem Statement:** A producer process creates data and puts it in a shared buffer. A consumer process takes data from the buffer. The buffer has a fixed size N. Producer should not produce when buffer is full; consumer should not consume when buffer is empty.

### Solution using Semaphores

```
// Shared variables:
semaphore mutex = 1;    // mutual exclusion for buffer access
semaphore empty = N;    // number of empty slots (initially N)
semaphore full  = 0;    // number of full slots (initially 0)
int buffer[N];
int in = 0, out = 0;

// PRODUCER:
do {
    produce item;
    
    wait(empty);        // Wait if no empty slots
    wait(mutex);        // Lock buffer
    
    buffer[in] = item;
    in = (in + 1) % N;
    
    signal(mutex);      // Unlock buffer
    signal(full);       // Increment full count
} while(true);

// CONSUMER:
do {
    wait(full);         // Wait if no items to consume
    wait(mutex);        // Lock buffer
    
    item = buffer[out];
    out = (out + 1) % N;
    
    signal(mutex);      // Unlock buffer
    signal(empty);      // Increment empty count
    
    consume item;
} while(true);
```

**Example:** Consider N=3 (buffer size 3):
- Initially: empty=3, full=0, mutex=1
- Producer produces 3 items: empty→0, full→3
- Producer tries to produce 4th: `wait(empty)` blocks → producer waits ✓
- Consumer consumes 1 item: empty→1, full→2 → producer unblocks ✓

---

## Q17 (5 Marks): Deadlock — Definition, Conditions, and Banker's Algorithm

### Deadlock Definition
> **Deadlock:** A situation where a set of processes are blocked, each waiting for a resource held by another process in the set, and none can proceed.

**Classic Example — Four Conditions (Coffman Conditions):**

1. **Mutual Exclusion:** At least one resource is non-sharable (only one process can use it at a time). E.g., a printer.

2. **Hold and Wait:** A process holds at least one resource and is waiting to acquire additional resources held by others.

3. **No Preemption:** Resources cannot be forcibly taken from a process; they must be released voluntarily.

4. **Circular Wait:** There exists a set of processes {P1, P2, ..., Pn} such that P1 waits for P2, P2 waits for P3, ..., Pn waits for P1.

> **All four conditions must hold simultaneously for deadlock to occur.**

### Resource Allocation Graph (RAG)
- **Circle** = Process, **Rectangle** = Resource type
- **Request edge:** P → R (process requesting resource)
- **Assignment edge:** R → P (resource assigned to process)
- **Deadlock exists if there is a cycle in the RAG (for single-instance resources)**

---

## Q18 (5 Marks): Deadlock Prevention and Avoidance

### Deadlock Prevention
Ensure at least one of the four necessary conditions never holds.

| Condition to Break | Method |
|---|---|
| Mutual Exclusion | Make resources sharable (not always possible) |
| Hold and Wait | Process must request all resources at once before starting |
| No Preemption | If a process can't get a resource, release all currently held resources |
| Circular Wait | Impose total ordering on resource types; request in increasing order |

### Deadlock Avoidance — Banker's Algorithm

> **Banker's Algorithm:** A deadlock avoidance algorithm that works by simulating resource allocation to determine if the system will remain in a "safe state."

> **Safe State:** A state in which there exists a safe sequence — an order in which all processes can complete.

**Data Structures needed:**
- **Available[m]:** Number of available instances of each resource type
- **Max[n][m]:** Maximum demand of each process
- **Allocation[n][m]:** Current allocation
- **Need[n][m] = Max - Allocation:** Remaining need

**Safety Algorithm:**
```
1. Work = Available
   Finish[i] = false for all i

2. Find i such that:
   Finish[i] == false AND Need[i] <= Work
   If no such i, go to step 4

3. Work = Work + Allocation[i]
   Finish[i] = true
   Go to step 2

4. If Finish[i] == true for all i → SAFE STATE
   Otherwise → UNSAFE STATE
```

---

## Q19 (5 Marks Numerical): Banker's Algorithm — Safe/Unsafe State

**Given:**
- 5 Processes (P0–P4), 3 Resource Types (A, B, C)
- Available: A=3, B=3, C=2

| Process | Allocation A,B,C | Max A,B,C | Need = Max − Alloc |
|---|---|---|---|
| P0 | 0,1,0 | 7,5,3 | 7,4,3 |
| P1 | 2,0,0 | 3,2,2 | 1,2,2 |
| P2 | 3,0,2 | 9,0,2 | 6,0,0 |
| P3 | 2,1,1 | 2,2,2 | 0,1,1 |
| P4 | 0,0,2 | 4,3,3 | 4,3,1 |

**Step 1:** Work = [3,3,2], all Finish = false

**Step 2:** Find process whose Need ≤ Work
- P0: Need=[7,4,3] ≤ [3,3,2]? **No**
- P1: Need=[1,2,2] ≤ [3,3,2]? **Yes** → Execute P1
  - Work = [3,3,2] + [2,0,0] = [5,3,2], Finish[P1]=true

**Step 3:** Continue with Work=[5,3,2]
- P3: Need=[0,1,1] ≤ [5,3,2]? **Yes** → Execute P3
  - Work = [5,3,2] + [2,1,1] = [7,4,3], Finish[P3]=true

**Step 4:** Work=[7,4,3]
- P4: Need=[4,3,1] ≤ [7,4,3]? **Yes** → Execute P4
  - Work = [7,4,3] + [0,0,2] = [7,4,5], Finish[P4]=true

**Step 5:** Work=[7,4,5]
- P0: Need=[7,4,3] ≤ [7,4,5]? **Yes** → Execute P0
  - Work = [7,4,5] + [0,1,0] = [7,5,5], Finish[P0]=true

**Step 6:** Work=[7,5,5]
- P2: Need=[6,0,0] ≤ [7,5,5]? **Yes** → Execute P2

✅ **Safe Sequence: P1 → P3 → P4 → P0 → P2**
**The system is in a SAFE STATE.**

---

## Q20 (15 Marks — Module 3): Critical Section + Producer-Consumer + Deadlock

### Part a (7M): Critical Section Problem
*(See Q15 above for full details)*

### Part b (8M): Producer-Consumer using Semaphores
*(See Q16 above for full details)*

---

## Q21 (15 Marks — Module 3): Semaphores/Monitors + Dining Philosophers

### Part a (7M): Semaphores and Monitors

#### Monitors
> **Monitor:** A high-level synchronization construct that encapsulates shared data, operations on that data, and synchronization — ensuring only one process executes inside the monitor at a time.

```
monitor MonitorName {
    // Shared variables
    condition x, y;   // condition variables
    
    procedure P1(...) { ... }
    procedure P2(...) { ... }
    
    initialization code { ... }
}
```

**Condition Variables — two operations:**
- `x.wait()` — suspends the calling process
- `x.signal()` — resumes one suspended process

**Advantages over semaphores:**
- Easier to reason about correctness
- Mutual exclusion is automatic
- Less prone to programming errors

---

### Dining Philosophers Problem

> **Problem:** Five philosophers sit at a round table. Each needs two chopsticks (left and right) to eat. Chopsticks are shared between adjacent philosophers.

**Risk:** If all philosophers pick up their left chopstick simultaneously → **deadlock** (each waiting for right chopstick).

#### Solution Using Semaphores (Asymmetric):
- Even-numbered philosophers pick left first, then right
- Odd-numbered philosophers pick right first, then left
- This breaks circular wait → no deadlock

#### Solution Using Monitor:

```
monitor DiningPhilosophers {
    enum {THINKING, HUNGRY, EATING} state[5];
    condition self[5];
    
    void pickup(int i) {
        state[i] = HUNGRY;
        test(i);
        if (state[i] != EATING) self[i].wait();
    }
    
    void putdown(int i) {
        state[i] = THINKING;
        test((i+4) % 5);   // test left neighbor
        test((i+1) % 5);   // test right neighbor
    }
    
    void test(int i) {
        if (state[(i+4)%5] != EATING &&
            state[i] == HUNGRY &&
            state[(i+1)%5] != EATING) {
            state[i] = EATING;
            self[i].signal();
        }
    }
}
```

---

## Q22 (15 Marks — Module 3): Deadlock Characterization + Banker's Algorithm

### Part a (7M): Deadlock Characterization
*(See Q17–Q18 above)*

### Part b (8M): Banker's Algorithm Numerical
*(See Q19 above)*

---

## Q23 (15 Marks — Module 3): Methods of Handling Deadlock + Detection & Recovery

### Part a (7M): Methods for Handling Deadlocks

Three approaches exist:

#### 1. Deadlock Prevention
Ensure at least one of the four Coffman conditions never holds (see Q18).

#### 2. Deadlock Avoidance
Use an algorithm (Banker's) to ensure the system never enters an unsafe state by carefully granting resources.

#### 3. Deadlock Detection and Recovery
Allow deadlocks to occur, detect them, and then recover.

#### 4. Ostrich Algorithm (Ignore)
Simply ignore deadlocks — valid for systems where deadlocks are rare (e.g., Linux, Windows leave this to the user).

---

### Part b (8M): Deadlock Detection and Recovery

#### Detection Algorithm (Multi-Instance Resources):
Similar to Banker's safety algorithm, but using the **Allocation** and **Request** matrices.

```
Work = Available
Finish[i] = (Allocation[i] == 0) for each i

Find i: Finish[i]==false AND Request[i] <= Work
  → Work = Work + Allocation[i], Finish[i] = true, repeat

If any Finish[i] == false → those processes are DEADLOCKED
```

#### Recovery Techniques:

**1. Process Termination:**
- Option A: Abort all deadlocked processes (costly — lose all work)
- Option B: Abort one process at a time until deadlock broken (less costly)

**Selection criteria for victim:** Lowest priority, shortest CPU time used, fewest resources held.

**2. Resource Preemption:**
- Select a victim process
- Rollback: Return the process to some safe state and restart from there
- Starvation concern: Same process may always be chosen as victim → use count of rollbacks as part of cost factor

---

# MODULE 4 — Memory Management

---

## Q24 (5 Marks): Paging and Segmentation

### Paging

> **Paging:** A memory management scheme that eliminates external fragmentation by dividing physical memory into fixed-size blocks called **frames** and logical memory into blocks of the same size called **pages**.

#### Key Concepts:
- **Page Size** = **Frame Size** (chosen as power of 2, e.g., 1KB, 4KB)
- **Logical address** = [Page Number | Page Offset]
- **Physical address** = [Frame Number | Page Offset] (offset unchanged)
- **Page Table:** Maps page numbers to frame numbers

#### Address Translation:
```
Logical address: d bits total
  → Page number p = d / page_size (higher bits)
  → Page offset d = d mod page_size (lower bits)

Physical address:
  → Frame f = PageTable[p]
  → Physical address = f * page_size + d
```

**Example:** Page size = 1KB = 1024 bytes
Logical address 3100:
- Page number = 3100 / 1024 = 3 (integer division)
- Offset = 3100 mod 1024 = 3100 − 3×1024 = 3100 − 3072 = 28
- Page Table: Page 3 → Frame 7
- Physical address = 7 × 1024 + 28 = 7168 + 28 = **7196**

**Advantage:** No external fragmentation
**Disadvantage:** Internal fragmentation (last page may not be fully used)

---

### Segmentation

> **Segmentation:** A memory management scheme that divides logical memory into variable-sized units called **segments**, each representing a logical unit (code, stack, data, heap).

**Logical address** = [Segment Number | Offset]

**Segment Table:**
| Segment | Base (Physical start) | Limit (size) |
|---|---|---|
| 0 (code) | 1400 | 1000 |
| 1 (stack) | 6300 | 400 |

**Address translation:**
1. Check if offset < limit (else addressing error)
2. Physical address = base + offset

**Advantage:** Matches programmer's view of memory (logical units)
**Disadvantage:** External fragmentation

---

### Comparison: Paging vs Segmentation

| Feature | Paging | Segmentation |
|---|---|---|
| Division | Fixed-size pages | Variable-size segments |
| External Fragmentation | None | Yes |
| Internal Fragmentation | Yes (last page) | None |
| Logical View | Not visible to programmer | Matches programmer's view |
| Table Used | Page table | Segment table |

---

## Q25 (5 Marks): Virtual Memory and Demand Paging

### Virtual Memory

> **Virtual Memory:** A technique that allows execution of processes that may not be completely loaded in physical memory. The logical address space of a process can be larger than the physical address space.

**Benefits:**
- Programs larger than physical memory can run
- More processes can run simultaneously
- Less I/O needed to load/swap programs

### Demand Paging

> **Demand Paging:** Pages are loaded into memory only when they are demanded (accessed), not in advance.

#### Page Fault:
When a process accesses a page not in memory → **page fault** interrupt occurs.

**Page Fault Handling Steps:**
1. Check internal page table → invalid reference? → abort process
2. Find a free frame in memory
3. Load the required page from disk into the frame
4. Update page table
5. Restart the instruction that caused the fault

#### Performance:
```
Effective Access Time (EAT) = (1 − p) × memory_access + p × page_fault_service_time

Where p = page fault rate (0 ≤ p ≤ 1)
```

**Example:** Memory access = 200ns, Page fault service = 8ms, p = 0.001
EAT = (0.999)(200) + (0.001)(8,000,000) = 199.8 + 8000 ≈ **8200 ns**

---

## Q26 (5 Marks): Thrashing and Working Set Model

### Thrashing

> **Thrashing:** A situation where a process spends more time paging (swapping pages in and out) than executing, causing extremely low CPU utilization.

**Cause:** A process has too few frames allocated → frequent page faults → OS brings in many pages → other processes lose frames → they page fault too → CPU utilization drops → OS thinks it needs to increase multiprogramming → makes it worse.

```
CPU Utilization
     ↑
     |      ←peak
     |     /\
     |    /  \  ← thrashing zone
     |   /    \____________
     |__/
     +-------------------------→ Degree of Multiprogramming
```

### Working Set Model

> **Working Set:** The set of pages a process is actively using in a given time window Δ.

**Working Set Window (Δ):** A fixed number of page references to look back.

**Principle:** Allocate enough frames for each process's working set. If total working set demand > physical frames → suspend some processes.

**Benefits:** Prevents thrashing while maximizing CPU utilization.

---

## Q27 (5 Marks Numerical): FIFO Page Replacement

### Page Replacement — Definition
> When a page fault occurs and no free frame is available, the OS must choose a page to **replace**. The algorithm for choosing the victim page is the **page replacement algorithm**.

### FIFO — First In First Out

> **Definition:** Replace the page that has been in memory the longest (oldest page).

**Reference String:** 7, 0, 1, 2, 0, 3, 0, 4 | **3 frames**

| Ref | Frame 1 | Frame 2 | Frame 3 | Page Fault? |
|---|---|---|---|---|
| 7 | **7** | — | — | ✅ YES |
| 0 | 7 | **0** | — | ✅ YES |
| 1 | 7 | 0 | **1** | ✅ YES |
| 2 | **2** | 0 | 1 | ✅ YES (replace 7, oldest) |
| 0 | 2 | 0 | 1 | ❌ NO (0 in frame) |
| 3 | 2 | **3** | 1 | ✅ YES (replace 0, oldest) |
| 0 | 2 | 3 | **0** | ✅ YES (replace 1, oldest) |
| 4 | **4** | 3 | 0 | ✅ YES (replace 2, oldest) |

**Total Page Faults = 6**

---

## Q28 (15 Marks — Module 4): Paging + Virtual Memory + FIFO Numerical

### Part a (7M): Virtual Memory and Demand Paging
*(See Q25 above)*

### Part b (8M): FIFO — 7,0,1,2,0,3,0,4,2,3 with 3 frames

| Ref | Frame 1 | Frame 2 | Frame 3 | Fault? |
|---|---|---|---|---|
| 7 | **7** | — | — | ✅ |
| 0 | 7 | **0** | — | ✅ |
| 1 | 7 | 0 | **1** | ✅ |
| 2 | **2** | 0 | 1 | ✅ (evict 7) |
| 0 | 2 | 0 | 1 | ❌ |
| 3 | 2 | **3** | 1 | ✅ (evict 0) |
| 0 | 2 | 3 | **0** | ✅ (evict 1) |
| 4 | **4** | 3 | 0 | ✅ (evict 2) |
| 2 | 4 | **2** | 0 | ✅ (evict 3) |
| 3 | 4 | 2 | **3** | ✅ (evict 0) |

**Total Page Faults = 9**

---

## Q29 (15 Marks — Module 4): Thrashing + LRU Numerical

### Part a (7M): Thrashing and Working Set Model
*(See Q26 above)*

### Part b (8M): LRU Page Replacement

### LRU — Least Recently Used

> **Definition:** Replace the page that has not been used for the longest time (looking backward in time).

**Reference String:** 1,2,3,4,1,2,5,1,2,3,4,5 | **3 frames**

*For LRU, keep track of when each page was last used. Replace the page used least recently.*

| Ref | Frame Contents | Evicted | Fault? |
|---|---|---|---|
| 1 | {1} | — | ✅ |
| 2 | {1,2} | — | ✅ |
| 3 | {1,2,3} | — | ✅ |
| 4 | {4,2,3} | 1 (LRU) | ✅ |
| 1 | {4,1,3} | 2 (LRU) | ✅ |
| 2 | {4,1,2} | 3 (LRU) | ✅ |
| 5 | {5,1,2} | 4 (LRU) | ✅ |
| 1 | {5,1,2} | — | ❌ (1 present) |
| 2 | {5,1,2} | — | ❌ (2 present) |
| 3 | {3,1,2} | 5 (LRU) | ✅ |
| 4 | {3,4,2} | 1 (LRU) | ✅ |
| 5 | {3,4,5} | 2 (LRU) | ✅ |

**Total Page Faults = 10**

---

## Q30 (Additional Numerical): Optimal Page Replacement

### Optimal Algorithm

> **Definition:** Replace the page that will NOT be used for the longest time in the future. This is theoretically optimal but cannot be implemented in practice (requires future knowledge). Used as a benchmark.

**Reference String:** 7,0,1,2,0,3,0,4,2,3 | **3 frames**

| Ref | Frames | Evicted | Next Use of Each | Fault? |
|---|---|---|---|---|
| 7 | {7} | — | — | ✅ |
| 0 | {7,0} | — | — | ✅ |
| 1 | {7,0,1} | — | — | ✅ |
| 2 | {2,0,1} | 7 (7 not used again) | 7→∞, 0→4, 1→∞ | ✅ |
| 0 | {2,0,1} | — | 0 present | ❌ |
| 3 | {2,0,3} | 1 (1 not used again) | 2→8, 0→6, 1→∞ | ✅ |
| 0 | {2,0,3} | — | 0 present | ❌ |
| 4 | {4,0,3} | 2 (next use of 2 is at pos 8; 3 at pos 9; 0 at... wait) | → evict 2 or 3? | ✅ |
| 2 | {4,2,3} | 0 (0 not used again) | 4→∞, 2→last, 3→9 | ✅ |
| 3 | {4,2,3} | — | 3 present | ❌ |

**Total Page Faults = 6**

---

## Q31 (Paging Address Translation): Logical Address 3100

**Given:** Page size = 1024 bytes
Page Table: Page 0→Frame 3, Page 1→Frame 6, Page 2→Frame 1, Page 3→Frame 7
**Logical Address = 3100**

**Step 1: Find Page Number and Offset**
```
Page Number (p) = floor(3100 / 1024) = floor(3.027...) = 3
Page Offset (d) = 3100 mod 1024 = 3100 − (3 × 1024) = 3100 − 3072 = 28
```

**Step 2: Look up Page Table**
```
Page 3 → Frame 7
```

**Step 3: Calculate Physical Address**
```
Physical Address = Frame Number × Page Size + Offset
                 = 7 × 1024 + 28
                 = 7168 + 28
                 = 7196
```

✅ **Physical Address = 7196**

---

# MODULE 5 — File Systems & Disk Scheduling

---

## Q32 (5 Marks): Disk Scheduling Algorithms

### Disk Structure
- **Tracks:** Concentric circles on a disk
- **Sectors:** Divisions of a track
- **Cylinders:** Set of tracks at same position across all platters
- **Seek Time:** Time to move head to correct track
- **Rotational Latency:** Time for correct sector to rotate under head
- **Transfer Time:** Time to read/write data

### Disk Scheduling Algorithms

#### 1. FCFS (First Come First Served)
Service requests in the order they arrive. Simple but may cause excessive head movement.

#### 2. SSTF (Shortest Seek Time First)
Service the request closest to the current head position. Reduces seek time but may cause **starvation** of far requests.

#### 3. SCAN (Elevator Algorithm)
Head moves in one direction, servicing all requests along the way. When it reaches the end, it reverses direction.

#### 4. C-SCAN (Circular SCAN)
Head moves in one direction only. When it reaches one end, it jumps back to the beginning (without servicing on the way back) and moves in the same direction again.

#### 5. LOOK
Like SCAN but head only goes as far as the last request in each direction, then reverses.

---

## Q33 (5 Marks): File Allocation Methods

### 1. Contiguous Allocation

> **Definition:** Each file occupies a set of contiguous blocks on disk.

- **Advantages:** Simple, fast sequential access, easy random access
- **Disadvantages:** External fragmentation, difficult to grow files
- **Example:** Old MS-DOS file system

### 2. Linked Allocation

> **Definition:** Each file is a linked list of disk blocks. Each block contains a pointer to the next block.

- **Advantages:** No external fragmentation, files can grow easily
- **Disadvantages:** No random access, pointer overhead, pointer corruption loses the rest of the file

### 3. Indexed Allocation

> **Definition:** Each file has an index block containing all pointers to its data blocks.

- **Advantages:** Supports direct access, no external fragmentation
- **Disadvantages:** Index block overhead; small files waste space in index block

### File Allocation Comparison

| Method | Sequential Access | Random Access | External Fragmentation |
|---|---|---|---|
| Contiguous | Excellent | Excellent | Yes |
| Linked | Good | Poor | No |
| Indexed | Good | Good | No |

---

## Q34 (5 Marks): DMA — Direct Memory Access

### Definition
> **DMA (Direct Memory Access):** A feature that allows certain hardware subsystems to access main memory independently of the CPU. The CPU initiates the transfer, then the DMA controller handles it, freeing the CPU for other work.

### How DMA Works:
1. CPU initiates DMA transfer: sets source, destination, size in DMA controller registers
2. DMA controller takes over the bus and transfers data directly between memory and I/O device
3. When transfer is complete, DMA controller interrupts the CPU
4. CPU resumes its work

**Example:** Reading a large file from disk:
- Without DMA: CPU must copy each byte from disk buffer to memory (CPU-bound, slow)
- With DMA: CPU sets up DMA, continues other work; DMA copies data; interrupts CPU when done

**Benefit:** Significantly improves system throughput for large data transfers.

---

## Q35 (15 Marks — Module 5): Disk Scheduling + File Allocation + FCFS/SSTF Numerical

### Part a (7M): Disk Scheduling Algorithms
*(See Q32 above)*

### Part b (8M): FCFS and SSTF — Requests: 98,183,37,122,14,124,65,67 | Head: 53

#### FCFS Solution:

Service in order: 53→98→183→37→122→14→124→65→67

| Movement | Distance |
|---|---|
| 53→98 | 45 |
| 98→183 | 85 |
| 183→37 | 146 |
| 37→122 | 85 |
| 122→14 | 108 |
| 14→124 | 110 |
| 124→65 | 59 |
| 65→67 | 2 |

**Total FCFS Head Movement = 45+85+146+85+108+110+59+2 = 640 cylinders**

---

#### SSTF Solution:

At each step, choose the request nearest to current head.

| Step | Current | Requests Remaining | Nearest | Distance |
|---|---|---|---|---|
| Start | 53 | 98,183,37,122,14,124,65,67 | 65 (dist=12) | 12 |
| 1 | 65 | 98,183,37,122,14,124,67 | 67 (dist=2) | 2 |
| 2 | 67 | 98,183,37,122,14,124 | 98 (dist=31) | 31 |
| 3 | 98 | 183,37,122,14,124 | 122 (dist=24) | 24 |
| 4 | 122 | 183,37,14,124 | 124 (dist=2) | 2 |
| 5 | 124 | 183,37,14 | 183 (dist=59) | 59 |
| 6 | 183 | 37,14 | 37 (dist=146) | 146 |
| 7 | 37 | 14 | 14 (dist=23) | 23 |

**Total SSTF Head Movement = 12+2+31+24+2+59+146+23 = 299 cylinders**

> SSTF (299) is significantly better than FCFS (640) in this case.

---

## Q36 (15 Marks — Module 5): File Allocation + SCAN Numerical

### Part a (7M): File Allocation Methods and Free-Space Management

#### Free-Space Management Techniques:

**1. Bit Vector (Bitmap):**
- One bit per block: 0 = free, 1 = allocated
- Example: `011011010...` → blocks 0, 3, 6 are free
- Easy to find contiguous free blocks; requires extra space

**2. Linked List:**
- Free blocks linked together
- No wasted space; hard to find contiguous blocks

**3. Grouping:**
- First free block stores addresses of n free blocks
- Last of those n blocks stores next group address

**4. Counting:**
- Store (first free block, count of consecutive free blocks)
- Efficient for contiguous allocation

---

### Part b (8M): SCAN Disk Scheduling

**Request Queue:** 176,79,34,60,92,11,41,114 | **Head = 50, moving toward higher cylinders**

**Sorted requests:** 11, 34, 41, 60, 79, 92, 114, 176

SCAN: Head moves toward 176, services: 60, 79, 92, 114, 176
Then reverses, services: 41, 34, 11

**Service Order:** 50 → 60 → 79 → 92 → 114 → 176 → 41 → 34 → 11

| Movement | Distance |
|---|---|
| 50→60 | 10 |
| 60→79 | 19 |
| 79→92 | 13 |
| 92→114 | 22 |
| 114→176 | 62 |
| 176→41 | 135 |
| 41→34 | 7 |
| 34→11 | 23 |

**Total SCAN Head Movement = 10+19+13+22+62+135+7+23 = 291 cylinders**

---

## Q37 (Additional Numerical): C-SCAN Disk Scheduling

**Request Queue:** 176,79,34,60,92,11,41,114 | **Head=50, Disk size=200**

**Sorted:** 11,34,41,60,79,92,114,176

C-SCAN moves toward high end: 60→79→92→114→176→199(end)→0→11→34→41

| Movement | Distance |
|---|---|
| 50→60 | 10 |
| 60→79 | 19 |
| 79→92 | 13 |
| 92→114 | 22 |
| 114→176 | 62 |
| 176→199 | 23 |
| 199→0 (wrap) | 199 |
| 0→11 | 11 |
| 11→34 | 23 |
| 34→41 | 7 |

**Total C-SCAN Head Movement = 10+19+13+22+62+23+199+11+23+7 = 389 cylinders**

---

## Q38 (Additional Numerical): SCAN for Requests 95,180,34,119,11,123,62,64 | Head=50

**Sorted:** 11,34,62,64,95,119,123,180

SCAN (moving toward high): 50 → 62 → 64 → 95 → 119 → 123 → 180 → 34 → 11

| Movement | Distance |
|---|---|
| 50→62 | 12 |
| 62→64 | 2 |
| 64→95 | 31 |
| 95→119 | 24 |
| 119→123 | 4 |
| 123→180 | 57 |
| 180→34 | 146 |
| 34→11 | 23 |

**Total Head Movement = 12+2+31+24+4+57+146+23 = 299 cylinders**

---

# QUICK REVISION SUMMARY

## Key Formulas

| Formula | Expression |
|---|---|
| TAT (Turnaround Time) | Completion Time − Arrival Time |
| WT (Waiting Time) | TAT − Burst Time |
| Average WT | Sum of all WT / Number of processes |
| Page Number | Logical Address ÷ Page Size (integer division) |
| Page Offset | Logical Address mod Page Size |
| Physical Address | Frame Number × Page Size + Offset |
| Need (Banker's) | Max − Allocation |

## Key Algorithms — When to Use

| Scenario | Best Algorithm |
|---|---|
| Batch system, simple | FCFS |
| Minimize average WT (non-preemptive) | SJF |
| Minimize average WT (preemptive) | SRTF |
| Interactive, fairness | Round Robin |
| Important jobs first | Priority Scheduling |
| Benchmark (unrealizable) page replacement | Optimal |
| Approximation of Optimal | LRU |
| Simple page replacement | FIFO |
| Disk: minimal seek | SSTF |
| Disk: no starvation | SCAN / LOOK |
| Disk: uniform wait | C-SCAN / C-LOOK |

## Semaphore Operations Summary

```
wait(S)   ≡   P(S)   ≡   S--   (if S<0, block)
signal(S) ≡   V(S)   ≡   S++   (wake one blocked process)
```

## Deadlock — Quick Check

All **four** conditions must hold: **M**utual exclusion + **H**old & wait + **N**o preemption + **C**ircular wait → "**MH-NC**"

---

*End of Operating Systems — Complete Semester Preparation Guide*
