# Computer Networks — Complete Descriptive Solutions
### Semester Preparation Guide (15 Marks Each)

---

# Q1. Network Architecture and Protocol Design

## (a) OSI Reference Model — 6 Marks

### Definition
The **OSI (Open Systems Interconnection) Reference Model** is a conceptual framework developed by ISO (International Organization for Standardization) in 1984 that standardizes the functions of a communication system into **seven distinct layers**. Each layer provides specific services to the layer above it and receives services from the layer below it.

> **Standard Definition:** *"The OSI model is a layered, abstract description for communication and computer network protocol design, published as ISO/IEC 7498-1."*

---

### The 7 Layers of OSI Model

```
┌──────────────────────────────────────────────┐
│  7. Application Layer                        │
├──────────────────────────────────────────────┤
│  6. Presentation Layer                       │
├──────────────────────────────────────────────┤
│  5. Session Layer                            │
├──────────────────────────────────────────────┤
│  4. Transport Layer                          │
├──────────────────────────────────────────────┤
│  3. Network Layer                            │
├──────────────────────────────────────────────┤
│  2. Data Link Layer                          │
├──────────────────────────────────────────────┤
│  1. Physical Layer                           │
└──────────────────────────────────────────────┘
```

#### Layer 1 — Physical Layer
- **Function:** Transmits raw bits over a physical medium.
- **Responsibilities:** Bit synchronization, bit rate control, physical topologies, transmission mode (simplex/half-duplex/full-duplex).
- **Example:** Ethernet cables, fiber optics, radio waves.
- **Devices:** Hub, Repeater, Modem.

#### Layer 2 — Data Link Layer
- **Function:** Provides node-to-node data transfer and handles error detection/correction from the physical layer.
- **Responsibilities:** Framing, MAC addressing, error detection (CRC), flow control (CSMA/CD).
- **Example:** Ethernet (IEEE 802.3), Wi-Fi (IEEE 802.11).
- **Devices:** Switch, Bridge.

#### Layer 3 — Network Layer
- **Function:** Responsible for logical addressing and routing of packets across networks.
- **Responsibilities:** IP addressing, routing, fragmentation & reassembly.
- **Example:** IP (IPv4/IPv6), ICMP, routers.
- **Devices:** Router.

#### Layer 4 — Transport Layer
- **Function:** Provides end-to-end communication, reliability, and flow control.
- **Responsibilities:** Segmentation, error recovery, connection management (TCP), connectionless service (UDP).
- **Example:** TCP (reliable), UDP (unreliable but fast).

#### Layer 5 — Session Layer
- **Function:** Manages and controls dialogues (sessions) between two computers.
- **Responsibilities:** Session establishment, maintenance, and termination; synchronization.
- **Example:** NetBIOS, RPC (Remote Procedure Call), PPTP.

#### Layer 6 — Presentation Layer
- **Function:** Translates data between the application layer and network format.
- **Responsibilities:** Data translation, compression, encryption/decryption.
- **Example:** SSL/TLS encryption, JPEG, MPEG, ASCII to EBCDIC translation.

#### Layer 7 — Application Layer
- **Function:** Provides network services directly to end-user applications.
- **Responsibilities:** File transfer, email, web browsing, DNS.
- **Example:** HTTP, FTP, SMTP, DNS, Telnet.

---

## (b) OSI vs TCP/IP Model — 5 Marks

### TCP/IP Model (4-Layer Model)

```
┌─────────────────────────────────────────────────────────────────────┐
│           OSI Model (7 Layers)      │    TCP/IP Model (4 Layers)   │
├─────────────────────────────────────┼──────────────────────────────┤
│  7. Application Layer               │                              │
│  6. Presentation Layer              │  4. Application Layer        │
│  5. Session Layer                   │                              │
├─────────────────────────────────────┼──────────────────────────────┤
│  4. Transport Layer                 │  3. Transport Layer          │
├─────────────────────────────────────┼──────────────────────────────┤
│  3. Network Layer                   │  2. Internet Layer           │
├─────────────────────────────────────┼──────────────────────────────┤
│  2. Data Link Layer                 │                              │
│  1. Physical Layer                  │  1. Network Access Layer     │
└─────────────────────────────────────┴──────────────────────────────┘
```

### Comparison Table

| Feature | OSI Model | TCP/IP Model |
|---|---|---|
| Full Form | Open Systems Interconnection | Transmission Control Protocol/Internet Protocol |
| Layers | 7 | 4 |
| Developed By | ISO | DARPA (U.S. Department of Defense) |
| Approach | Generic, protocol-independent | Protocol-specific |
| Usage | Reference model (theoretical) | Practical implementation |
| Transport Layer | Supports both connection-oriented and connectionless | Both TCP (connection) and UDP (connectionless) |
| Application Layer | 3 separate layers (Application, Presentation, Session) | Single Application Layer |
| Reliability | Less reliable (model only) | More reliable (based on actual protocols) |

---

## (c) Advantages and Limitations of Layered Architecture — 4 Marks

### Advantages
1. **Modularity:** Each layer can be developed and modified independently. E.g., changing the physical medium doesn't affect the application layer.
2. **Standardization:** Vendors can build interoperable products by following layer standards.
3. **Ease of Troubleshooting:** Problems can be isolated to specific layers. E.g., a network connectivity issue can be diagnosed layer by layer.
4. **Reusability:** Protocols at one layer can be reused with different protocols at adjacent layers. E.g., TCP works over both IPv4 and IPv6.

### Limitations
1. **Overhead:** Each layer adds its own header/trailer (encapsulation overhead), consuming bandwidth.
2. **Rigidity:** Strict separation may prevent optimization across layers. E.g., a video application cannot directly control the physical layer for QoS.
3. **Duplication:** Error handling exists in multiple layers (Data Link AND Transport), causing redundancy.
4. **Real-World Mismatch:** Real protocols often don't map cleanly. OSI's session and presentation layers are rarely implemented separately.

---

# Q2. Wired and Wireless Communication Systems

## (a) Guided and Unguided Transmission Media — 5 Marks

### Definition
**Transmission media** is the physical path through which data is transmitted from sender to receiver. It is broadly classified into:

### Guided (Wired) Media
Signal is confined to a physical conductor.

#### 1. Twisted Pair Cable
- **Structure:** Two insulated copper wires twisted together.
- **Types:** UTP (Unshielded) and STP (Shielded).
- **Bandwidth:** Up to 100 Mbps for Cat-5e, 10 Gbps for Cat-6a.
- **Example:** Telephone lines, Ethernet LAN.
- **Limitation:** Susceptible to interference over long distances.

#### 2. Coaxial Cable
- **Structure:** Central copper conductor, insulating layer, metallic shield, plastic cover.
- **Bandwidth:** Up to 1 Gbps.
- **Example:** Cable TV, broadband Internet.
- **Advantage:** Better noise immunity than twisted pair.

#### 3. Optical Fiber
- **Structure:** Glass/plastic core surrounded by cladding; transmits light pulses.
- **Types:** Single-mode (long distance) and Multi-mode (short distance).
- **Bandwidth:** Terabits per second.
- **Example:** Internet backbone, FTTH (Fiber to Home).
- **Advantage:** Immune to electromagnetic interference, very high bandwidth.

### Unguided (Wireless) Media
Signal propagates through the atmosphere or space.

#### 1. Radio Waves (3 kHz – 1 GHz)
- Omnidirectional, can penetrate walls.
- **Example:** AM/FM radio, Wi-Fi (2.4 GHz), Bluetooth.

#### 2. Microwaves (1 GHz – 300 GHz)
- Line-of-sight, directional antennas.
- **Example:** Cellular networks (4G/5G), satellite TV.

#### 3. Infrared (300 GHz – 400 THz)
- Short range, cannot penetrate walls.
- **Example:** TV remotes, IrDA communication.

---

## (b) Wired vs Wireless Networks — 5 Marks

| Parameter | Wired Network | Wireless Network |
|---|---|---|
| **Reliability** | High – consistent signal quality | Moderate – affected by interference, distance |
| **Security** | High – physically contained, hard to intercept | Lower – signals can be intercepted (needs WPA3, encryption) |
| **Mobility** | None – fixed physical connection | High – devices can move freely |
| **Bandwidth** | Very high (10 Gbps+ with fiber) | Lower (Wi-Fi 6E: up to 9.6 Gbps, typical 100–600 Mbps) |
| **Cost** | High installation cost (cables, conduits) | Lower installation, higher device cost |
| **Setup** | Complex, time-consuming cabling | Quick and flexible |
| **Latency** | Very low (~1ms) | Higher (~5–50ms) |
| **Example** | Ethernet LAN in offices | Wi-Fi in cafes, homes, hospitals |

---

## (c) Smart City Communication Infrastructure Design — 5 Marks

### Scenario
Design connectivity for: **Traffic lights, Surveillance cameras, IoT sensors**

### Proposed Infrastructure

```
                ┌─────────────────────┐
                │   Central Control   │
                │      Center         │
                └─────────┬───────────┘
                          │ Fiber Optic Backbone
          ┌───────────────┼───────────────┐
          │               │               │
   ┌──────┴──────┐  ┌─────┴──────┐  ┌────┴───────┐
   │ Traffic Mgmt│  │Surveillance│  │ IoT Sensors│
   │  (4G/5G)   │  │(Fiber/LTE) │  │(LoRaWAN/NB-│
   └─────────────┘  └────────────┘  │    IoT)    │
                                     └────────────┘
```

### Design Choices and Justification

| Component | Technology | Justification |
|---|---|---|
| **Traffic Lights** | 4G/5G Cellular + Fiber to key junctions | Real-time control needs low latency (<20ms). 5G provides ultra-reliable low-latency communication (URLLC). |
| **Surveillance Cameras** | Fiber Optic (primary) + 4G LTE (backup) | HD/4K video generates 4–8 Mbps per camera. Fiber provides the bandwidth. LTE as fallback. |
| **IoT Sensors** (air quality, parking, waste) | LoRaWAN / NB-IoT | Low data rate (bytes/minute), long battery life, wide coverage – ideal for sensor networks. |
| **Network Management** | SD-WAN with central NOC | Centralized monitoring, dynamic traffic routing, fault tolerance. |
| **Security** | VPN tunnels, AES-256 encryption, IDS/IPS | Protects against cyber attacks on critical infrastructure. |

**Justification Summary:** A hybrid approach using fiber for high-bandwidth needs, 5G for latency-critical control, and LPWAN (LoRaWAN/NB-IoT) for battery-powered sensors provides the best balance of performance, cost, and scalability.

---

# Q3. Switching Techniques and Telephone Networks

## (a) Circuit Switching, Packet Switching, and Message Switching — 6 Marks

### 1. Circuit Switching

**Definition:** A dedicated communication path is established between sender and receiver for the entire duration of the communication session before data transmission begins.

**Phases:**
1. **Circuit Establishment** – A dedicated path is set up.
2. **Data Transfer** – Continuous data flow.
3. **Circuit Disconnection** – Path is released after communication ends.

**Example:** Traditional telephone network (PSTN). When you call someone, a dedicated circuit is reserved.

**Advantages:**
- Guaranteed bandwidth, constant delay.
- Suitable for real-time voice/video.

**Disadvantages:**
- Wasteful if no data is being transmitted (idle circuit still reserved).
- Long setup time.

---

### 2. Packet Switching

**Definition:** Data is broken into small units called **packets**. Each packet travels independently through the network and may take different routes. Packets are reassembled at the destination.

**Types:**
- **Datagram (Connectionless):** Each packet is routed independently. Example: Internet (IP).
- **Virtual Circuit (Connection-oriented):** A logical path is established before sending. Example: ATM, Frame Relay.

**Example:** Sending an email. The email is broken into packets, each routed separately, reassembled at the receiver.

**Advantages:**
- Efficient use of bandwidth (no idle reservations).
- Fault-tolerant (packets can reroute around failures).

**Disadvantages:**
- Variable delay (jitter), packet may arrive out of order.
- Overhead due to headers in each packet.

---

### 3. Message Switching

**Definition:** The entire message is stored at each intermediate node (store-and-forward) before being forwarded to the next node. No dedicated path is established.

**Example:** Email systems in early networks, telegrams.

**Advantages:**
- No dedicated circuit needed.
- Messages can be queued during congestion.

**Disadvantages:**
- Large storage required at intermediate nodes.
- High delay — not suitable for real-time communication.

### Comparison Table

| Feature | Circuit Switching | Packet Switching | Message Switching |
|---|---|---|---|
| Path | Dedicated | Dynamic | Store-and-forward |
| Delay | Low, fixed | Variable | High |
| Bandwidth Use | Inefficient (idle waste) | Efficient | Efficient |
| Suitable For | Voice calls | Internet data | Email, telegram |
| Error Handling | At endpoints | At each node | At each node |

---

## (b) TDM vs Space-Division Switching — 4 Marks

### Time-Division Switching (TDS)
- Time is divided into slots; each connection gets a time slot.
- Uses a **time-slot interchange (TSI)** mechanism.
- Data stored in memory and read out in different time slots.
- **Example:** PCM-based telephone exchanges (digital switching).

### Space-Division Switching (SDS)
- Physical path (space) is switched — different connections use different physical paths simultaneously.
- Uses a crossbar switch matrix.
- **Example:** Analog telephone exchanges (electromechanical crossbar).

| Feature | Time-Division Switching | Space-Division Switching |
|---|---|---|
| Switching Mechanism | Time slot interchange | Physical crosspoint matrix |
| Type | Digital | Analog or Digital |
| Complexity | Lower hardware, more software | More hardware (crosspoints) |
| Blocking | Non-blocking (if designed well) | Can be blocking |
| Cost | Lower (uses memory) | Higher (requires N×N matrix) |
| Example | Digital PBX | Old analog exchanges |

---

## (c) TDM Calculation — 5 Marks

### Given:
- Number of users = 64
- Each user generates = 128 kbps

### Calculation:

**Total Transmission Capacity Required:**

```
Total Capacity = Number of Users × Traffic per User
Total Capacity = 64 × 128 kbps
Total Capacity = 8192 kbps = 8.192 Mbps ≈ 8 Mbps
```

**With Framing Overhead (typically 1 bit per frame):**
- If each frame has 1 framing bit + 64 user bits = 65 bits per frame.
- Overhead = 1/65 ≈ 1.54%
- Effective capacity required ≈ 8.192 × 1.0154 ≈ **8.318 Mbps**

### Impact of Synchronization Errors in TDM:

1. **Frame Slip:** If the clock drifts, a frame slot may be misread — data from user A is assigned to user B's time slot, causing data corruption.
2. **Frame Misalignment:** If synchronization is lost, all 64 channels lose data simultaneously (catastrophic compared to a single packet loss in packet switching).
3. **Bit Errors:** A single synchronization error can cascade and corrupt multiple users' data until the system re-synchronizes.
4. **Recovery Time:** Systems use **elastic buffers** and **clock recovery circuits** (PLL – Phase-Locked Loops) to minimize synchronization loss, but recovery can take tens of milliseconds causing noticeable glitches in voice calls.

---

# Q4. Framing, Error Detection, and Flow Control

## (a) Framing Techniques in Data Link Layer — 5 Marks

### Definition
**Framing** is the process of dividing the stream of bits received from the network layer into manageable data units called **frames**, so that the receiver can identify the beginning and end of each frame.

### Framing Methods

#### 1. Character Count
- The first field in the frame header specifies the **number of characters** in the frame.
- **Problem:** If the count field is corrupted, the receiver loses synchronization.
- **Example:** Used in some early protocols.

```
[ Count=5 | D | A | T | A | 1 ] [ Count=4 | D | A | T | A ]
```

#### 2. Flag Bytes with Byte Stuffing
- Special **flag byte** (e.g., `01111110` in HDLC) marks the start and end of a frame.
- If the flag byte appears in the data, a **stuffing byte (ESC)** is inserted before it.
- **Example:** PPP (Point-to-Point Protocol) uses `0x7E` as flag, `0x7D` as escape byte.

```
FLAG | Header | Data (with ESC stuffing) | Trailer | FLAG
```

#### 3. Flag Bits with Bit Stuffing
- Flag pattern: **01111110** (six consecutive 1s).
- Sender inserts a `0` after every five consecutive `1`s in the data.
- Receiver removes the stuffed `0` after five `1`s.
- **Example:** HDLC, SDLC protocols.

```
Original data:  0 1 1 1 1 1 1 0
After stuffing: 0 1 1 1 1 1 0 1 0   ← '0' inserted after five 1s
```

#### 4. Physical Layer Coding Violations
- Uses invalid signal states (violations of encoding rules) to mark frame boundaries.
- **Example:** Manchester encoding — certain invalid transitions signal frame start/end.
- **Advantage:** No stuffing overhead.

---

## (b) Error Detection vs Error Correction — 4 Marks

### Error Detection
Identifies that an error has occurred but **does not identify or fix** the corrupted bits. The receiver requests retransmission.

| Method | How It Works | Example |
|---|---|---|
| Parity Check | Adds 1 bit to make total 1s even/odd | Simple serial communication |
| Checksum | Sum of all data words; complement sent | TCP/IP, UDP headers |
| CRC | Polynomial division; remainder sent | Ethernet, Wi-Fi frames |

### Error Correction
Identifies **and corrects** errors without retransmission. Requires more redundant bits.

| Method | How It Works | Example |
|---|---|---|
| Hamming Code | Adds parity bits at power-of-2 positions | Memory (ECC RAM) |
| Reed-Solomon | Polynomial-based, corrects burst errors | CDs, DVDs, QR codes |
| Convolutional Code | Encodes using sliding window of bits | 4G/LTE, Wi-Fi (802.11) |

### Key Difference

| Feature | Error Detection | Error Correction |
|---|---|---|
| Action | Detect only | Detect and fix |
| Redundancy | Less (few bits) | More (many bits) |
| Complexity | Low | High |
| Use Case | Wired networks (low error rate, retransmit) | Wireless, storage (high error rate, no retransmit) |

---

## (c) CRC Calculation — 6 Marks

### Given:
- Data: `1101011011`
- Generator: `10011` (degree 4, so append 4 zeros)

### Step 1: Append Zeros
Append **4 zeros** (degree of generator) to the data:
```
Message with zeros: 1101011011 0000
                    = 11010110110000
```

### Step 2: Perform Binary Division (XOR Division)

```
Dividend:  1 1 0 1 0 1 1 0 1 1 0 0 0 0
Generator: 1 0 0 1 1

Step 1:
  1 1 0 1 0
⊕ 1 0 0 1 1
  ---------
  0 1 0 1 1   → bring down next bit → 0 1 0 1 1 1

Step 2:
  0 1 0 1 1   (leading 0, shift → use next bit)
  1 0 1 1 1
⊕ 1 0 0 1 1
  ---------
  0 0 1 0 0   → bring down → 0 0 1 0 0 0

Step 3:
  0 0 1 0 0   (leading 0s, shift → use next bit)
  0 1 0 0 0
  1 0 0 0 1
⊕ 1 0 0 1 1
  ---------
  0 0 0 1 0   → bring down → 0 0 0 1 0 0

Step 4:
  0 0 0 1 0 0  (leading 0s, shift)
  0 1 0 0 1
  1 0 0 1 0
⊕ 1 0 0 1 1
  ---------
  0 0 0 0 1   → bring down → 0 0 0 0 1 0

Step 5:
  0 0 0 0 1 0 → two bits left, degree < 4, this is remainder
```

### Detailed Long Division:

```
11010110110000 ÷ 10011
─────────────────────────────
11010  ÷ 10011 = 1, XOR → 11010 ⊕ 10011 = 01001
Bring down 1 → 10011
10011 ÷ 10011 = 1, XOR → 10011 ⊕ 10011 = 00000
Bring down 1 → 00001
00001 → too small, quotient bit = 0
Bring down 0 → 00010
00010 → too small, quotient bit = 0
Bring down 1 → 00101
00101 → too small, quotient bit = 0
Bring down 1 → 01011
01011 → too small, quotient bit = 0
Bring down 0 → 10110
10110 ÷ 10011 = 1, XOR → 10110 ⊕ 10011 = 00101
Bring down 0 → 01010
01010 → too small, quotient bit = 0
Bring down 0 → 10100
10100 ÷ 10011 = 1, XOR → 10100 ⊕ 10011 = 00111

Remainder = 0111 (pad to 4 bits) = 1110
```

### Step 3: Result
```
CRC Remainder = 1110
Transmitted Codeword = Data + CRC = 1101011011 1110
```

### Verification
At the receiver, divide `11010110111110` by `10011`. If remainder = `0000`, no error.

---

# Q5. Sliding Window Protocol Analysis

## (a) Stop-and-Wait ARQ and One-Bit Sliding Window — 4 Marks

### Stop-and-Wait ARQ

**Definition:** The sender transmits one frame and **waits for an acknowledgment (ACK)** before sending the next frame. If a timeout occurs without ACK, the frame is retransmitted.

```
Sender          Receiver
  │── Frame 0 ──►│
  │◄── ACK 0 ───│
  │── Frame 1 ──►│
  │◄── ACK 1 ───│
  │── Frame 2 ──►│  (lost!)
  │   (timeout)  │
  │── Frame 2 ──►│  (retransmit)
  │◄── ACK 2 ───│
```

**Efficiency:**
```
η = T_transmission / (T_transmission + 2 × T_propagation)
  = T_f / (T_f + 2T_p)
```

**Limitation:** Very inefficient for high-bandwidth, long-delay links (satellite links).

### One-Bit Sliding Window
- Special case of Stop-and-Wait where window size = 1.
- Uses alternating bit protocol — sequence numbers 0 and 1 only.
- After sending frame 0 and receiving ACK 0, sends frame 1, etc.
- Handles lost frames and lost ACKs.

---

## (b) Go-Back-N and Selective Repeat — 6 Marks

### Go-Back-N (GBN) Protocol

**Definition:** The sender can transmit up to **N frames** (window size N) without waiting for ACK. If a frame is lost or corrupted, **all frames from the lost frame onward are retransmitted**.

**Sender Window Size:** Up to 2ⁿ − 1 (for n-bit sequence numbers)
**Receiver Window Size:** 1 (receiver only accepts in-order frames)

```
Sender Window (N=4):
[F0][F1][F2][F3] → sent
         │ F2 lost!
[F2][F3][F4][F5] → retransmit F2, F3, F4, F5 (go back to F2)
```

**State Diagram:**
```
     Send F[i]         Receive ACK[i]
  ┌──────────┐         ┌────────────┐
  │  Ready   │────────►│  Waiting   │
  └──────────┘         └─────┬──────┘
         ▲                   │ Timeout / NAK
         │  Window Slides    │ Go back N
         └───────────────────┘
```

**Advantage:** Simple receiver logic.
**Disadvantage:** Wastes bandwidth by retransmitting correctly received frames.

---

### Selective Repeat (SR) Protocol

**Definition:** Only the **specific lost or damaged frame** is retransmitted. Receiver buffers out-of-order frames.

**Sender Window Size:** 2ⁿ⁻¹
**Receiver Window Size:** 2ⁿ⁻¹ (can buffer out-of-order frames)

```
Sender Window (N=4):
[F0][F1][F2][F3] → sent
         │ F2 lost!
Receiver buffers: [F1(ok)][F2(missing)][F3(ok)]
Only [F2] is retransmitted → [F2]
Receiver delivers: F0, F1, F2, F3 in order
```

**State Diagram:**
```
         Receive NAK/Timeout for F[i]
  ┌──────────────────────────────────┐
  │ Retransmit F[i] only             │
  ▼                                  │
 Send F[i] ──────────────────────────┘
  │
  │ Receive ACK[i]
  ▼
 Slide window, send next frame
```

**Advantage:** Better efficiency — only retransmits lost frames.
**Disadvantage:** Complex receiver logic, buffering required.

---

## (c) Window Sizes for 3-bit Sequence Numbers — 5 Marks

### Given: n = 3 bits → Sequence numbers: 0 to 7 (total 2³ = 8)

### Go-Back-N Protocol:
```
Max Sender Window Size    = 2ⁿ − 1 = 2³ − 1 = 7
Max Receiver Window Size  = 1
```

**Justification for GBN:** If sender window = 8 (= 2ⁿ), and all 8 frames are transmitted but all ACKs are lost, the sender retransmits frames 0–7. The receiver, having already accepted frames 0–7 and moved to expecting frame 0 again, accepts them as **new frames** (duplicates mistaken as new). Hence window must be ≤ 7.

### Selective Repeat Protocol:
```
Max Sender Window Size    = 2ⁿ⁻¹ = 2³⁻¹ = 4
Max Receiver Window Size  = 2ⁿ⁻¹ = 4
```

**Justification for SR:** In SR, receiver window size must be ≤ 2ⁿ/2 = 4. If window = 5 and all ACKs are lost, the sender retransmits frame 0. The receiver's window (shifted to 1–5) might overlap with old sequence numbers, causing confusion between old and new frames. Limiting both windows to 4 ensures no overlap.

### Summary Table:

| Protocol | Sender Window | Receiver Window | Formula |
|---|---|---|---|
| Stop-and-Wait | 1 | 1 | — |
| Go-Back-N | 2ⁿ − 1 = **7** | 1 | sender = 2ⁿ − 1 |
| Selective Repeat | 2ⁿ⁻¹ = **4** | 2ⁿ⁻¹ = **4** | each = 2ⁿ/2 |

---

# Q6. Multiple Access Protocols and Ethernet

## (a) Pure ALOHA and Slotted ALOHA Throughput — 5 Marks

### Pure ALOHA

**Definition:** A random access protocol where any station can transmit **at any time**. If a collision occurs, the station waits a random backoff time and retransmits.

**Throughput Analysis:**
- Let G = average number of transmission attempts per frame time.
- Frame is successful if no other frame is transmitted during the **vulnerable period = 2T** (T = one frame time).

```
P(success) = e^(-2G)
Throughput S = G × e^(-2G)
```

**Maximum Throughput:**
```
dS/dG = 0
e^(-2G) - 2G·e^(-2G) = 0
1 - 2G = 0
G = 0.5

S_max = 0.5 × e^(-2×0.5) = 0.5 × e^(-1) = 0.5/e ≈ 0.184 = 18.4%
```

**Maximum throughput of Pure ALOHA = 18.4%**

---

### Slotted ALOHA

**Definition:** Time is divided into discrete **slots** equal to one frame time. Stations can only begin transmission at the **start of a slot**, reducing the vulnerable period to T (half of Pure ALOHA).

**Throughput Analysis:**
- Vulnerable period = T (instead of 2T).

```
P(success) = e^(-G)
Throughput S = G × e^(-G)
```

**Maximum Throughput:**
```
dS/dG = 0
e^(-G) - G·e^(-G) = 0
1 - G = 0
G = 1

S_max = 1 × e^(-1) = 1/e ≈ 0.368 = 36.8%
```

**Maximum throughput of Slotted ALOHA = 36.8%** (exactly double Pure ALOHA)

---

## (b) CSMA/CD vs ALOHA — 5 Marks

### CSMA/CD (Carrier Sense Multiple Access with Collision Detection)

**Definition:** Before transmitting, a station **senses the channel** (carrier sense). If busy, it waits. If idle, it transmits. During transmission, it **listens for collisions** (collision detection). On collision, it sends a jam signal, stops, and waits a random backoff time (binary exponential backoff).

**Operation Steps:**
1. Sense the channel — if idle, transmit.
2. If busy, wait (persistent or non-persistent).
3. While transmitting, listen for collision.
4. If collision detected, send 32-bit jam signal, stop.
5. Wait random backoff time (2^k - 1 slots, k = attempt number).
6. After max attempts, report failure.

**Example:** Ethernet (IEEE 802.3) uses CSMA/CD.

### Comparison: ALOHA vs CSMA/CD

| Feature | Pure ALOHA | Slotted ALOHA | CSMA/CD |
|---|---|---|---|
| Channel Sensing | No | No | Yes (before & during TX) |
| Collision Detection | No | No | Yes (during TX) |
| Transmission Time | Any time | Start of slot only | After sensing idle |
| Max Throughput | 18.4% | 36.8% | ~50–90% |
| Collision Handling | Backoff + retransmit | Backoff + retransmit | Jam signal + backoff |
| Use Case | Satellite, early radio | Cellular (early), radio | Ethernet LAN |

---

## (c) Evolution of Ethernet — 5 Marks

### Ethernet Timeline

#### 10 Mbps Ethernet (1983 — IEEE 802.3)
- Medium: Thick coax (10BASE5), thin coax (10BASE2), UTP (10BASE-T).
- CSMA/CD used.
- Collision domain spans the entire bus.
- **Example:** Early office LANs.

#### 100 Mbps Fast Ethernet (1995 — IEEE 802.3u)
- Medium: Cat-5 UTP (100BASE-TX), fiber (100BASE-FX).
- Same CSMA/CD, 10× speed increase.
- Introduced Auto-negotiation (speed/duplex).
- **Example:** Home/office LANs in late 1990s.

#### 1 Gbps Gigabit Ethernet (1998 — IEEE 802.3z/ab)
- Medium: Fiber (1000BASE-LX/SX), Cat-5e/6 UTP (1000BASE-T).
- Full-duplex mode eliminates collisions; CSMA/CD rarely used.
- **Example:** Server connections, campus backbones.

#### 10 Gbps Ethernet (2002 — IEEE 802.3ae)
- Medium: Single-mode fiber (10GBASE-LR), multimode fiber (10GBASE-SR).
- **Full-duplex only** — CSMA/CD completely dropped.
- Introduced WAN PHY for metropolitan networks.
- **Example:** Data center interconnects.

#### 40/100 Gbps and 400 Gbps Ethernet
- IEEE 802.3ba (2010): 40G and 100G over fiber/copper.
- IEEE 802.3bs (2017): 200G and 400G Ethernet.
- Used in data center spines and Internet exchange points.

### Key Technological Improvements

| Feature | Evolution |
|---|---|
| Medium | Coax → UTP → Fiber → Optical transceivers |
| Speed | 10 Mbps → 100 M → 1G → 10G → 400G |
| Topology | Bus → Star (hub) → Switched star |
| CSMA/CD | Used → Rarely used → Dropped (full-duplex) |
| Frame Size | Stayed 1518 bytes (Jumbo frames: 9000 bytes added) |

---

# Q7. Wireless LANs and Bluetooth Technologies

## (a) IEEE 802.11 WLAN Architecture and Operation — 5 Marks

### Architecture

#### Components:
- **Station (STA):** Any device with an 802.11 wireless interface (laptop, phone).
- **Access Point (AP):** Bridge between wireless and wired networks.
- **Basic Service Set (BSS):** Group of stations communicating with one AP.
- **Extended Service Set (ESS):** Multiple BSSs connected through a **Distribution System (DS)** — creating a single logical network.
- **IBSS (Ad-hoc mode):** Stations communicate directly without an AP.

```
        [Internet]
            │
        [Router]
            │
    [Distribution System]
       │           │
    [AP 1]       [AP 2]
    /    \       /    \
 [STA1] [STA2] [STA3] [STA4]
   ←── BSS 1 ──→ ←── BSS 2 ──→
   ←────────── ESS ───────────→
```

### Operation (CSMA/CA — Collision Avoidance)

Since wireless stations **cannot detect collisions** while transmitting (unlike wired), 802.11 uses **CSMA/CA**:

1. **Sense the channel** — if idle for DIFS (Distributed Inter-Frame Space), transmit.
2. If busy, wait until idle then wait for additional **random backoff** time.
3. Transmit frame; receiver sends **ACK** after SIFS (Short IFS).
4. No ACK → collision assumed → backoff and retry.

**Optional RTS/CTS (for hidden node problem):**
- Sender sends RTS (Request to Send).
- AP responds with CTS (Clear to Send) heard by all nearby stations.
- All stations defer — prevents hidden node collisions.

### IEEE 802.11 Standards:

| Standard | Frequency | Max Speed | Key Feature |
|---|---|---|---|
| 802.11b | 2.4 GHz | 11 Mbps | First widely adopted |
| 802.11g | 2.4 GHz | 54 Mbps | Backward compatible |
| 802.11n (Wi-Fi 4) | 2.4/5 GHz | 600 Mbps | MIMO introduced |
| 802.11ac (Wi-Fi 5) | 5 GHz | 3.5 Gbps | MU-MIMO, beamforming |
| 802.11ax (Wi-Fi 6) | 2.4/5/6 GHz | 9.6 Gbps | OFDMA, BSS coloring |

---

## (b) Bluetooth vs Wi-Fi — 4 Marks

| Feature | Bluetooth | Wi-Fi (IEEE 802.11) |
|---|---|---|
| Standard | IEEE 802.15.1 | IEEE 802.11 |
| Range | ~10 m (Class 2), ~100 m (Class 1) | ~50 m indoors, ~250 m outdoors |
| Frequency | 2.4 GHz (FHSS) | 2.4 GHz, 5 GHz, 6 GHz |
| Max Speed | ~3 Mbps (BT 3.0), ~50 Mbps (BT 5.0) | Up to 9.6 Gbps (Wi-Fi 6) |
| Power Consumption | Very low (designed for IoT/wearables) | Higher |
| Use Case | Headphones, keyboards, smartwatches | Internet access, file transfer |
| Network Type | PAN (Personal Area Network) | LAN (Local Area Network) |
| Security | AES-128 (BT 4.0+), Secure Simple Pairing | WPA2/WPA3 |

---

## (c) Wireless Network Design for Multi-storey Academic Building — 6 Marks

### Scenario: Multi-storey building, 500 concurrent users

### Design

#### Floor-by-Floor AP Layout:
- **Each floor:** 2–4 APs depending on floor size.
- **AP placement:** Central corridors + classrooms, avoiding walls/obstacles.
- **Coverage per AP:** ~30 m radius indoors → ~2,827 m² per AP.

#### Channel Allocation (2.4 GHz and 5 GHz)

**2.4 GHz band (3 non-overlapping channels: 1, 6, 11):**
```
Floor 1:  AP1(Ch1)  AP2(Ch6)  AP3(Ch11)
Floor 2:  AP4(Ch6)  AP5(Ch11) AP6(Ch1)   ← reuse with sufficient separation
Floor 3:  AP7(Ch11) AP8(Ch1)  AP9(Ch6)
```

**5 GHz band:** 24 non-overlapping channels — use for high-density areas (labs, lecture halls).

#### User Capacity Planning:
```
500 users / ~20 APs = 25 users per AP
Wi-Fi 6 supports 50+ concurrent users per AP comfortably
```

#### Security Choices:

| Security Measure | Justification |
|---|---|
| WPA3-Enterprise (802.1X + RADIUS) | Per-user authentication with university credentials |
| VLAN Segmentation | Separate VLANs for students, faculty, IoT, guests |
| Captive Portal for Guests | Controlled guest access, terms of service |
| IDS/IPS | Detect rogue APs, unauthorized access |
| Certificate-based EAP-TLS | Strong mutual authentication |

#### Recommended Equipment:
- **APs:** Wi-Fi 6 (802.11ax) — handles high density with OFDMA.
- **Controllers:** Cloud-managed WLAN controller for centralized management.
- **Backhaul:** Gigabit Ethernet to each AP.

---

# Q8. IPv4 Addressing and Subnet Design (VLSM)

## Network: 172.16.0.0/16

### Requirement (sorted largest to smallest for VLSM):

| Department | Hosts Required | Hosts Needed (including network + broadcast) |
|---|---|---|
| R&D | 2000 | 2048 → /21 (2046 usable) |
| HR | 1000 | 1024 → /22 (1022 usable) |
| Finance | 500 | 512 → /23 (510 usable) |
| Sales | 250 | 256 → /24 (254 usable) |

**Formula:** Hosts per subnet = 2^(32 - prefix) - 2

---

## (a) VLSM Subnet Allocation — 8 Marks

### Subnet 1: R&D — 2000 Hosts
```
Hosts needed: 2000
Smallest power of 2 ≥ 2002 (hosts + network + broadcast) = 2048 = 2¹¹
Subnet prefix: /21  (32 - 11 = 21)
Block size: 2048 addresses
```
**Network: 172.16.0.0/21**

### Subnet 2: HR — 1000 Hosts
```
Next available block starts at 172.16.8.0
Hosts needed: 1000
Smallest power of 2 ≥ 1002 = 1024 = 2¹⁰
Subnet prefix: /22  (32 - 10 = 22)
Block size: 1024 addresses
```
**Network: 172.16.8.0/22**

### Subnet 3: Finance — 500 Hosts
```
Next available block starts at 172.16.12.0
Hosts needed: 500
Smallest power of 2 ≥ 502 = 512 = 2⁹
Subnet prefix: /23  (32 - 9 = 23)
Block size: 512 addresses
```
**Network: 172.16.12.0/23**

### Subnet 4: Sales — 250 Hosts
```
Next available block starts at 172.16.14.0
Hosts needed: 250
Smallest power of 2 ≥ 252 = 256 = 2⁸
Subnet prefix: /24  (32 - 8 = 24)
Block size: 256 addresses
```
**Network: 172.16.14.0/24**

---

## (b) Network Address, Broadcast, and Valid Host Range — 7 Marks

### R&D Subnet — 172.16.0.0/21
```
Subnet Mask:     255.255.248.0
Network Address: 172.16.0.0
First Host:      172.16.0.1
Last Host:       172.16.7.254
Broadcast:       172.16.7.255
Usable Hosts:    2046
```

### HR Subnet — 172.16.8.0/22
```
Subnet Mask:     255.255.252.0
Network Address: 172.16.8.0
First Host:      172.16.8.1
Last Host:       172.16.11.254
Broadcast:       172.16.11.255
Usable Hosts:    1022
```

### Finance Subnet — 172.16.12.0/23
```
Subnet Mask:     255.255.254.0
Network Address: 172.16.12.0
First Host:      172.16.12.1
Last Host:       172.16.13.254
Broadcast:       172.16.13.255
Usable Hosts:    510
```

### Sales Subnet — 172.16.14.0/24
```
Subnet Mask:     255.255.255.0
Network Address: 172.16.14.0
First Host:      172.16.14.1
Last Host:       172.16.14.254
Broadcast:       172.16.14.255
Usable Hosts:    254
```

### Summary Table

| Dept | Network | Subnet Mask | First Host | Last Host | Broadcast | Usable |
|---|---|---|---|---|---|---|
| R&D | 172.16.0.0 | /21 | 172.16.0.1 | 172.16.7.254 | 172.16.7.255 | 2046 |
| HR | 172.16.8.0 | /22 | 172.16.8.1 | 172.16.11.254 | 172.16.11.255 | 1022 |
| Finance | 172.16.12.0 | /23 | 172.16.12.1 | 172.16.13.254 | 172.16.13.255 | 510 |
| Sales | 172.16.14.0 | /24 | 172.16.14.1 | 172.16.14.254 | 172.16.14.255 | 254 |

---

# Q9. IPv6 Migration and Addressing

## (a) IPv6 Packet Structure vs IPv4 — 5 Marks

### IPv4 Header (20–60 bytes)
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
├───────────────────────────────────────────────────────────────────┤
│Version│ IHL   │Type of Service│          Total Length             │
├───────────────────────────────────────────────────────────────────┤
│         Identification        │Flags│      Fragment Offset        │
├───────────────────────────────────────────────────────────────────┤
│  Time to Live │    Protocol   │         Header Checksum           │
├───────────────────────────────────────────────────────────────────┤
│                       Source Address (32 bits)                    │
├───────────────────────────────────────────────────────────────────┤
│                    Destination Address (32 bits)                  │
├───────────────────────────────────────────────────────────────────┤
│                    Options and Padding (variable)                 │
└───────────────────────────────────────────────────────────────────┘
```

### IPv6 Header (Fixed 40 bytes)
```
├───────────────────────────────────────────────────────────────────┤
│Version│   Traffic Class   │           Flow Label                  │
├───────────────────────────────────────────────────────────────────┤
│         Payload Length            │  Next Header  │  Hop Limit   │
├───────────────────────────────────────────────────────────────────┤
│                                                                   │
│                    Source Address (128 bits)                      │
│                                                                   │
│                                                                   │
├───────────────────────────────────────────────────────────────────┤
│                                                                   │
│                  Destination Address (128 bits)                   │
│                                                                   │
│                                                                   │
└───────────────────────────────────────────────────────────────────┘
```

### Comparison Table

| Field | IPv4 | IPv6 |
|---|---|---|
| Version | 4 | 6 |
| Header Size | 20–60 bytes (variable) | 40 bytes (fixed) |
| Address Size | 32 bits (4 bytes) | 128 bits (16 bytes) |
| Checksum | Present in header | **Removed** (left to upper layers) |
| Fragmentation | At routers and source | **Only at source** |
| Options | In header (variable) | Extension headers (modular) |
| TTL | Time to Live (8-bit) | Hop Limit (8-bit, same function) |
| QoS | ToS (Type of Service) | Traffic Class + Flow Label |
| NAT | Required (due to address shortage) | Not needed |
| Security | Optional (IPSec) | Built-in (IPSec mandatory in design) |

---

## (b) Limitations of IPv4 — 4 Marks

1. **Address Exhaustion:** IPv4 provides only 2³² ≈ 4.3 billion addresses. With billions of devices (IoT, smartphones), addresses ran out — IANA exhausted IPv4 pool in 2011.

2. **NAT Complexity:** Network Address Translation was introduced as a workaround, but it breaks end-to-end connectivity, complicates VoIP, P2P, and requires stateful tracking.

3. **Security:** IPSec is optional in IPv4, leading to insecure deployments. IPv6 made IPSec integration mandatory.

4. **Routing Table Size:** Classful and classless routing with millions of routes creates large routing tables, slowing router lookups.

5. **QoS Limitations:** The ToS field in IPv4 was poorly used; IPv6 introduces a dedicated Flow Label for better QoS.

6. **No Auto-configuration:** IPv4 requires DHCP for address assignment; IPv6 supports **SLAAC (Stateless Address Auto-Configuration)**.

---

## (c) IPv4-to-IPv6 Transition Mechanisms — 6 Marks

### 1. Dual Stack
**Definition:** Nodes run **both IPv4 and IPv6** simultaneously and can communicate using either protocol.

```
[IPv4/IPv6 Host] ←─IPv4─→ [IPv4 Host]
[IPv4/IPv6 Host] ←─IPv6─→ [IPv6 Host]
```

**Advantages:** Full compatibility with both networks.
**Disadvantages:** Requires both protocols in all devices; doesn't help IPv4 exhaustion.
**Example:** Modern OS (Windows, Linux, Android) all dual-stack by default.

---

### 2. Tunneling
**Definition:** IPv6 packets are **encapsulated inside IPv4 packets** to traverse IPv4 networks (or vice versa).

```
[IPv6 Host]─────[IPv4 Tunnel]────[IPv6 Host]
  IPv6 packet wrapped in IPv4 envelope →→→→ unwrapped at endpoint
```

**Types:**
- **6to4:** Automatic tunneling; embeds IPv4 address in IPv6 address.
- **Teredo:** Tunneling through NAT (for home users).
- **ISATAP:** Intra-site tunneling for dual-stack hosts.
- **6in4 (Manual):** Manually configured tunnel endpoints.

**Advantages:** No infrastructure change needed in the IPv4 core.
**Disadvantages:** Added latency, MTU issues, complex troubleshooting.

---

### 3. Translation (NAT64 / DNS64)
**Definition:** **NAT64** translates IPv6 packets to IPv4 and vice versa at the network boundary, allowing IPv6-only clients to reach IPv4-only servers.

```
[IPv6-only Client] → [NAT64 Gateway] → [IPv4-only Server]
DNS64 synthesizes AAAA (IPv6) records for IPv4-only domains
```

**Advantages:** Allows IPv6-only devices to reach legacy IPv4 services.
**Disadvantages:** Application-layer protocols with embedded IP addresses may break; stateful NAT complexity.

### Summary

| Mechanism | How | Use Case |
|---|---|---|
| Dual Stack | Both IPv4 and IPv6 run together | Gradual migration |
| Tunneling | IPv6 in IPv4 packets | Islands of IPv6 over IPv4 |
| Translation (NAT64) | Convert between IPv4 and IPv6 | IPv6-only to reach IPv4 |

---

# Q10. Routing Algorithms and Network Optimization

## Network Graph

| Link | Cost |
|---|---|
| A–B | 2 |
| A–C | 5 |
| B–C | 1 |
| B–D | 3 |
| C–D | 2 |
| C–E | 4 |
| D–E | 1 |

## (a) Dijkstra's Algorithm from Node A — 8 Marks

### Graph Representation:
```
    A ──(2)── B ──(3)── D
    │         │         │
   (5)       (1)       (1)
    │         │         │
    └──────── C ──(4)── E
                └──(2)─┘
```

### Algorithm Steps:

**Initialization:**
```
Visited = {}
Distance: A=0, B=∞, C=∞, D=∞, E=∞
```

**Iteration 1 — Visit A (distance = 0):**
```
Update neighbors:
  B: min(∞, 0+2) = 2
  C: min(∞, 0+5) = 5
Visited = {A}
Distance: A=0, B=2, C=5, D=∞, E=∞
```

**Iteration 2 — Visit B (distance = 2, minimum unvisited):**
```
Update neighbors:
  C: min(5, 2+1) = 3  ← updated via B
  D: min(∞, 2+3) = 5
Visited = {A, B}
Distance: A=0, B=2, C=3, D=5, E=∞
```

**Iteration 3 — Visit C (distance = 3, minimum unvisited):**
```
Update neighbors:
  D: min(5, 3+2) = 5  ← no change
  E: min(∞, 3+4) = 7
Visited = {A, B, C}
Distance: A=0, B=2, C=3, D=5, E=7
```

**Iteration 4 — Visit D (distance = 5, minimum unvisited):**
```
Update neighbors:
  E: min(7, 5+1) = 6  ← updated via D
Visited = {A, B, C, D}
Distance: A=0, B=2, C=3, D=5, E=6
```

**Iteration 5 — Visit E (distance = 6):**
```
No unvisited neighbors.
Visited = {A, B, C, D, E}
```

### Shortest Paths from A:

| Destination | Shortest Distance | Path |
|---|---|---|
| A | 0 | A |
| B | 2 | A → B |
| C | 3 | A → B → C |
| D | 5 | A → B → C → D (or A → B → D) |
| E | 6 | A → B → C → D → E |

---

## (b) Routing Table for Node A — 3 Marks

| Destination | Next Hop | Cost |
|---|---|---|
| A | — (local) | 0 |
| B | B | 2 |
| C | B | 3 |
| D | B | 5 |
| E | B | 6 |

> **Note:** All traffic from A routes through **B** as the next hop for all destinations.

---

## (c) Link State vs Distance Vector Routing — 4 Marks

| Feature | Link State (OSPF) | Distance Vector (RIP) |
|---|---|---|
| **Algorithm** | Dijkstra's SPF | Bellman-Ford |
| **Knowledge** | Complete topology of the network | Only distances to neighbors |
| **Updates** | Flood LSAs (Link State Advertisements) to all routers | Share routing table with adjacent neighbors only |
| **Convergence Speed** | Fast (triggered updates, SPF recalculation) | Slow (count-to-infinity problem) |
| **Scalability** | High (hierarchical areas in OSPF) | Low (limited to 15 hops in RIP) |
| **Overhead** | Higher (stores full topology) | Lower (only distance vectors) |
| **Loops** | No (uses complete topology) | Possible (count-to-infinity) |
| **Example** | OSPF, IS-IS | RIP v1/v2 |

---

# Q11. Dynamic Routing Protocols

## (a) RIP, OSPF, and BGP — 6 Marks

### RIP (Routing Information Protocol)

**Definition:** A **Distance Vector** routing protocol that uses **hop count** as the routing metric. Maximum hop count = 15 (16 = infinity/unreachable).

**Operation:**
- Each router sends its entire routing table to **neighbors** every **30 seconds**.
- Routers update their table using Bellman-Ford algorithm.

**Versions:**
- **RIPv1:** Classful, no subnet info in updates.
- **RIPv2:** Classless (CIDR), supports authentication.

**Limitations:** Slow convergence, count-to-infinity problem, max 15 hops.
**Use:** Small networks only.

---

### OSPF (Open Shortest Path First)

**Definition:** A **Link State** routing protocol that uses **Dijkstra's SPF algorithm**. Metric is based on **bandwidth** (cost = 10⁸/bandwidth).

**Operation:**
- Routers flood **LSAs (Link State Advertisements)** to all routers in the area.
- Each router builds a complete **topology database (LSDB)**.
- SPF algorithm computes shortest paths.
- Supports **hierarchical routing** using areas (Area 0 = backbone).

**Features:**
- Fast convergence (triggered updates).
- No hop limit.
- Supports VLSM, CIDR.
- Authentication supported.

**Example:** Enterprise networks, ISP internal routing.

---

### BGP (Border Gateway Protocol)

**Definition:** A **Path Vector** routing protocol used to exchange routing information **between autonomous systems (AS)** on the Internet. BGP is the routing protocol of the Internet.

**Operation:**
- BGP peers establish **TCP connections** (port 179).
- Exchanges **AS_PATH** to avoid routing loops.
- Uses **policies** (not just metrics) for path selection.

**Types:**
- **iBGP:** Within same AS.
- **eBGP:** Between different ASes.

**Attributes used for path selection:**
1. Weight (Cisco proprietary)
2. LOCAL_PREF
3. AS_PATH length (shorter = better)
4. MED (Multi-Exit Discriminator)

---

## (b) OSPF vs BGP — 4 Marks

| Feature | OSPF | BGP |
|---|---|---|
| Type | Link State (IGP) | Path Vector (EGP) |
| Scope | Within a single AS | Between multiple ASes |
| Metric | Cost (based on bandwidth) | AS_PATH + policy attributes |
| Convergence | Fast (seconds) | Slow (minutes) |
| Scalability | Medium (supports areas) | Very high (Internet-scale) |
| Loop Prevention | SPF algorithm | AS_PATH attribute |
| Protocol | IP (protocol 89) | TCP port 179 |

---

## (c) Why BGP is the Backbone of the Internet — 5 Marks

BGP is called the **"glue of the Internet"** for the following reasons:

1. **Inter-AS Routing:** The Internet consists of ~100,000+ autonomous systems (ISPs, enterprises, CDNs). BGP is the **only standardized protocol** that enables routing **between** these different organizations. No IGP (OSPF/RIP) can scale to this level.

2. **Policy-Based Routing:** Unlike OSPF (pure metric-based), BGP allows organizations to implement **business policies** — e.g., prefer routes through cheaper providers, block routes through competitors, enforce regulatory compliance.

3. **Massive Scalability:** The global BGP routing table contains **~950,000+ IPv4 prefixes** (as of 2024). BGP's incremental update mechanism (only sends changes, not full tables) allows it to manage this scale efficiently.

4. **AS_PATH Loop Prevention:** By carrying the list of all ASes a route has traversed, BGP inherently prevents routing loops between ASes — critical for a globally distributed system.

5. **Traffic Engineering:** BGP attributes (LOCAL_PREF, MED, communities) allow ISPs to perform sophisticated traffic engineering, load balancing, and failover across multiple upstream providers.

6. **Supports both IPv4 and IPv6:** MP-BGP (Multiprotocol BGP) carries routing information for IPv4, IPv6, VPNs (MPLS), and other protocols — making it the universal routing fabric.

> **Example:** When you access google.com from India, BGP in your ISP's router determines the best path through multiple international AS hops to reach Google's AS (AS15169).

---

# Q12. Transport Layer Protocols

## (a) Services of the Transport Layer — 4 Marks

The Transport Layer (Layer 4) provides the following services:

1. **Process-to-Process Delivery:** Uses **port numbers** to deliver data to the correct application process. E.g., port 80 for HTTP, port 25 for SMTP.

2. **Segmentation and Reassembly:** Breaks large messages into segments for transmission; reassembles at the destination.

3. **Connection Management:** Establishes, maintains, and terminates connections (TCP 3-way handshake; TCP 4-way termination).

4. **Reliability and Error Control:** TCP ensures all segments are received correctly through **sequence numbers, ACKs, and retransmission**.

5. **Flow Control:** Prevents sender from overwhelming a slow receiver (TCP uses sliding window / receiver's advertised window size).

6. **Congestion Control:** Prevents the sender from overwhelming the **network** (TCP slow start, congestion avoidance).

7. **Multiplexing/Demultiplexing:** Multiple applications share the network layer via port numbers.

---

## (b) TCP vs UDP vs SCTP — 6 Marks

| Feature | TCP | UDP | SCTP |
|---|---|---|---|
| Full Form | Transmission Control Protocol | User Datagram Protocol | Stream Control Transmission Protocol |
| Connection | Connection-oriented (3-way handshake) | Connectionless | Connection-oriented (4-way handshake) |
| Reliability | Yes (ACK, retransmission) | No | Yes |
| Ordering | In-order delivery | Not guaranteed | In-order per stream |
| Flow Control | Yes (sliding window) | No | Yes |
| Congestion Control | Yes (slow start, etc.) | No | Yes |
| Multi-streaming | No (single stream) | N/A | **Yes** (multiple streams in one connection) |
| Multi-homing | No | No | **Yes** (multiple IP paths) |
| Header Size | 20–60 bytes | 8 bytes | 12 bytes minimum |
| Speed | Slower (overhead) | Fast | Moderate |
| Example Use | HTTP, FTP, SMTP | DNS, DHCP, VoIP | Telephony signaling (SS7 over IP), WebRTC |

---

## (c) Protocol Suitability Analysis — 5 Marks

| Application | Best Protocol | Justification |
|---|---|---|
| **Video Streaming** (YouTube, Netflix) | **UDP** (with QUIC/DASH) | Slight packet loss is acceptable (video can skip a frame); retransmission would cause buffering. Real-time delivery > perfect delivery. Modern: QUIC (UDP-based) adds reliability at app layer. |
| **File Transfer** (FTP, SFTP, HTTP download) | **TCP** | Every byte must arrive correctly. A single missing byte corrupts the file. TCP's reliability and ordering are essential. |
| **Online Gaming** | **UDP** (preferred) or QUIC | Real-time positional updates — late data is worse than lost data. Low latency matters more than reliability. Position updates are overwritten by the next update anyway. |

### Additional Analysis:
- **VoIP calls:** UDP — a 20ms audio packet arriving 500ms late is useless; better to drop it.
- **Email (SMTP):** TCP — the entire message must be received correctly.
- **DNS queries:** UDP — small, single request-response; no need for connection overhead. Falls back to TCP for large responses (>512 bytes).

---

# Q13. TCP Congestion Control and QoS

## (a) TCP Congestion Control Phases — 7 Marks

TCP uses these four mechanisms to control congestion:

### Phase 1: Slow Start (SS)

**Definition:** When a TCP connection begins (or restarts after timeout), the congestion window (cwnd) starts at **1 MSS** (Maximum Segment Size) and **doubles every RTT** (exponential growth) until it reaches the **ssthresh (slow start threshold)**.

```
cwnd:  1 → 2 → 4 → 8 → 16 (doubles each RTT)
```

**Note:** Despite the name, slow start grows *fast* (exponential). It's "slow" only compared to sending everything at once.

**Example:** TCP starts with cwnd=1, sends 1 segment, gets ACK → cwnd=2, sends 2 segments, gets 2 ACKs → cwnd=4, etc.

---

### Phase 2: Congestion Avoidance (CA)

**Definition:** When cwnd ≥ ssthresh, growth slows to **linear** (increases by 1 MSS per RTT) to probe for available bandwidth without causing congestion.

```
cwnd per RTT: 16 → 17 → 18 → 19 → ... (linear growth)
```

---

### Phase 3: Fast Retransmit

**Definition:** If the sender receives **3 duplicate ACKs** (same ACK three times), it assumes the corresponding segment is lost and **retransmits immediately** without waiting for the retransmission timer to expire.

```
Receiver gets: F1, F2, [F3 lost], F4, F5, F6
Receiver sends: ACK2, ACK2, ACK2 (3 dup ACKs for F2)
Sender: Fast retransmit F3 without timeout
```

**Advantage:** Faster recovery than waiting for timeout.

---

### Phase 4: Fast Recovery

**Definition:** After fast retransmit (triggered by 3 dup ACKs), instead of going back to slow start:
- ssthresh = cwnd/2
- cwnd = ssthresh + 3 (credit for 3 dup ACKs)
- Enter **Congestion Avoidance** directly (skip Slow Start)

```
On Timeout:           On 3 Dup ACKs:
ssthresh = cwnd/2     ssthresh = cwnd/2
cwnd = 1 MSS          cwnd = ssthresh + 3
→ Slow Start          → Congestion Avoidance (Fast Recovery)
```

### TCP Congestion Window Graph:
```
cwnd
 ^
 │                         ╱
 │              ╱─────────╱ Congestion Avoidance (linear)
 │         ╱──╱
 │    ╱───╱  ssthresh
 │  ╱  Slow Start (exponential)
 └──────────────────────────────────► time
    SS│CA│FR│ SS│     CA      │
```

---

## (b) Open Loop vs Closed Loop Congestion Control — 4 Marks

### Open Loop Control
- Congestion is **prevented before it occurs** through traffic planning and admission control.
- No feedback from the network.
- **Techniques:** Traffic shaping (Leaky Bucket, Token Bucket), admission control, resource reservation (RSVP).
- **Example:** ATM networks with traffic contracts.

### Closed Loop Control
- Congestion is **detected and remedied** after it occurs using feedback.
- The network signals congestion back to the sender.
- **Techniques:** TCP congestion control (slow start, CA), ECN (Explicit Congestion Notification), AQM (Active Queue Management — RED).
- **Example:** TCP's response to packet loss (timeout, dup ACKs).

| Feature | Open Loop | Closed Loop |
|---|---|---|
| Approach | Preventive | Reactive |
| Feedback | Not required | Required |
| Complexity | Higher (planning) | Lower (adaptive) |
| Example | Token Bucket, RSVP | TCP Slow Start, RED |

---

## (c) TCP Reno vs Earlier Implementations — 4 Marks

### TCP Tahoe (Earlier)
- On any loss (timeout OR 3 dup ACKs): reset cwnd=1, go to slow start.
- **Problem:** Even on 3 dup ACKs (mild congestion), it reset to 1 — too aggressive.

### TCP Reno (Improvement)
**Key Difference:** Distinguishes between **timeout (severe congestion)** and **3 dup ACKs (mild congestion)**:

| Event | TCP Tahoe | TCP Reno |
|---|---|---|
| Timeout | cwnd=1, Slow Start | cwnd=1, Slow Start |
| 3 Dup ACKs | cwnd=1, Slow Start | ssthresh=cwnd/2, cwnd=ssthresh, **CA** (Fast Recovery) |

**Improvements of TCP Reno:**
1. **Fast Retransmit:** Retransmits on 3 dup ACKs without waiting for timeout (reduces recovery time from ~1 RTT timeout to immediate).
2. **Fast Recovery:** Skips slow start after 3 dup ACKs — maintains higher throughput.
3. **Better Throughput:** By not crashing cwnd to 1 on mild congestion, Reno maintains higher average throughput on partially congested paths.

**Example:** A 100 Mbps link with ssthresh=32. On 3 dup ACKs, Tahoe resets to 1 MSS and takes many RTTs to recover. Reno halves to 16 MSS and continues linearly — much faster recovery.

---

# Q14. QoS Mechanisms and Delay Tolerant Networks

## (a) Leaky Bucket and Token Bucket Algorithms — 6 Marks

### Leaky Bucket Algorithm

**Definition:** Models a bucket with a small hole at the bottom. Packets enter the bucket at variable rates but leave at a **constant, fixed rate** — smoothing bursty traffic.

```
Bursty Traffic In    Constant Rate Out
      ▼                     ▼
   ┌──────┐              ─────────►  Network
   │ FIFO │  constant rate
   │  Q   │──────────────────────►
   └──────┘
   (overflow = drop)
```

**Operation:**
- If the bucket (queue) is full → packets are **dropped (or marked)**.
- Output rate is always constant (e.g., 1 packet per clock tick).
- **Enforces**: maximum output rate regardless of input burst.

**Example:** An ISP limits a customer to 10 Mbps. Even if the customer sends 50 Mbps bursts, only 10 Mbps passes through; excess is dropped.

**Limitation:** Cannot accommodate legitimate short bursts — always outputs at a fixed rate.

---

### Token Bucket Algorithm

**Definition:** Tokens are added to a bucket at a constant rate (r tokens/sec). To transmit a packet, a token must be available. **Burst traffic is allowed** as long as tokens are available.

```
Token Generator (r tokens/sec)
        ▼
   ┌──────────┐
   │  Token   │ ← max b tokens
   │  Bucket  │
   └──────────┘
        │ (need token to send)
        ▼
   Packets ──────────────────► Network
```

**Operation:**
- Tokens arrive at rate **r** (tokens/sec).
- Bucket holds max **b** tokens (burst capacity).
- To send 1 packet: consume 1 token.
- If no tokens: packets are queued or dropped.

**Maximum burst size:** b tokens × packet size = b × L bits in one instant.
**Long-term rate:** limited to r tokens/sec.

**Example:** A video server can burst at 50 Mbps for up to 2 seconds (token bucket with b=100 Mb, r=10 Mbps), then sustained rate is 10 Mbps.

### Comparison:

| Feature | Leaky Bucket | Token Bucket |
|---|---|---|
| Output Rate | Always constant | Variable (up to burst limit) |
| Bursts | **Not allowed** | **Allowed** (up to b tokens) |
| Smoothing | Yes (strict) | Yes (with burst allowance) |
| Use | Network policing | Traffic shaping |

---

## (b) Traffic Shaping for 5–20 Mbps Variable Rate — 4 Marks

### Scenario: Variable input rate 5–20 Mbps

### Recommended Mechanism: **Token Bucket combined with Leaky Bucket**

**Design:**

```
Variable rate (5–20 Mbps)
         │
    ┌────┴────────┐
    │ Token Bucket│  r = 10 Mbps (sustained rate)
    │  b = 5 Mb   │  b = 5 Mb burst (0.5 seconds burst at 20 Mbps)
    └────┬────────┘
         │ Burst-shaped output (≤20 Mbps short term)
    ┌────┴────────┐
    │ Leaky Bucket│  Rate = 10 Mbps (smooth output)
    └────┬────────┘
         │
    → Network at max 10 Mbps steady, brief bursts absorbed
```

**Justification:**
- **Token Bucket** absorbs the burst (allows 20 Mbps for short periods) while enforcing a 10 Mbps average.
- **Leaky Bucket** downstream further smooths output for QoS-sensitive traffic.
- Together they implement a **dual-rate shaper** — allowing bursts for interactive applications while protecting network resources.

---

## (c) Delay Tolerant Networks (DTNs) — 5 Marks

### Definition
**DTN (Delay Tolerant Network)** is a network architecture designed for environments where **continuous end-to-end connectivity cannot be assumed** — links may be intermittent, disconnected for hours or days, with very high latency.

### Architecture

DTN uses a **"store-carry-and-forward"** paradigm:
- A node **stores** a bundle (message) when no forwarding path exists.
- It **carries** the bundle as it moves.
- It **forwards** when a connection opportunity arises.

**Key Protocol:** **Bundle Protocol (BP)** — a new overlay layer above the transport layer.

```
[Source] → store → [Relay Node moves] → forward → [Relay] → [Destination]
(no path now)    (hours later, different location)
```

### Applications

#### 1. Space Communications
- **Example:** Mars rover communicating with Earth.
- One-way light time = 3–22 minutes → round-trip delay = 6–44 minutes.
- No continuous link — Earth station visible only when planets align.
- DTN stores telemetry data until contact window opens.
- **CCSDS (Consultative Committee for Space Data Systems)** has adopted Bundle Protocol for deep-space missions.

#### 2. Disaster Recovery
- After earthquakes/floods, infrastructure (cell towers, fiber) is destroyed.
- **DTN on drones or vehicles** act as message ferries carrying emergency communications between survivors and rescue teams.
- **Example:** "Pocket Switched Networks" — first responders carry devices that exchange messages opportunistically when they meet.

#### 3. Rural/Remote Connectivity
- **DakNet:** Buses carry Wi-Fi access points through Indian villages, collecting messages and delivering them when reaching towns with Internet.
- Also used in wildlife tracking, underwater sensor networks (acoustic DTN).

---

# Q15. Application Layer Protocols and Network Security

## (a) DNS, HTTP, SMTP, and FTP — 6 Marks

### DNS (Domain Name System)

**Definition:** A hierarchical, distributed database that translates **human-readable domain names** (www.google.com) into **IP addresses** (142.250.195.68).

**Operation:**
1. Browser asks local DNS resolver for "www.google.com".
2. Resolver queries **Root DNS** → finds .com TLD server.
3. Queries **TLD server** → finds google.com authoritative server.
4. Queries **Authoritative server** → gets IP address.
5. Returns IP to browser; result **cached** with TTL.

**Uses:** Port 53 (UDP for queries, TCP for zone transfers).

---

### HTTP (HyperText Transfer Protocol)

**Definition:** An application-layer protocol for transferring **web pages** (HTML, images, videos) between web servers and browsers.

**Operation:**
- **HTTP/1.1:** Persistent connections, pipelining, text-based.
- **HTTP/2:** Binary framing, multiplexing (multiple requests over one connection), header compression.
- **HTTP/3:** Based on QUIC (UDP), eliminates head-of-line blocking.

**Methods:** GET, POST, PUT, DELETE, HEAD.
**Status Codes:** 200 OK, 404 Not Found, 301 Redirect, 500 Server Error.
**Port:** 80 (HTTP), 443 (HTTPS).

---

### SMTP (Simple Mail Transfer Protocol)

**Definition:** Protocol for **sending email** between mail servers and from mail clients to servers.

**Operation:**
```
[Sender's MUA] → SMTP → [Sender's MTA] → SMTP → [Recipient's MTA] → POP3/IMAP → [Recipient's MUA]
```
- Uses **TCP port 25** (server-to-server), 587 (client submission), 465 (SMTPS).
- Commands: HELO, MAIL FROM, RCPT TO, DATA, QUIT.
- SMTP sends email; **POP3/IMAP** retrieves email.

---

### FTP (File Transfer Protocol)

**Definition:** Protocol for **transferring files** between a client and server.

**Operation:**
- Uses **two TCP connections**:
  - **Control Connection** (port 21): Commands and responses.
  - **Data Connection** (port 20): Actual file transfer.
- **Active mode:** Server initiates data connection to client.
- **Passive mode:** Client initiates data connection (NAT-friendly).

**Commands:** USER, PASS, LIST, RETR (retrieve), STOR (store), QUIT.
**Limitation:** Passwords sent in plaintext → use **SFTP** (SSH-based) or **FTPS** (FTP over SSL) for security.

---

## (b) Public-Key Cryptography and Digital Signatures — 4 Marks

### Public-Key Cryptography (Asymmetric Encryption)

**Definition:** Uses a **pair of keys** — a **public key** (shared openly) and a **private key** (kept secret). Data encrypted with one key can only be decrypted with the other.

**Example — RSA:**
```
Alice generates: Public Key (e, n) and Private Key (d, n)
Bob wants to send a secret to Alice:
  Bob encrypts with Alice's public key → Ciphertext
  Only Alice can decrypt with her private key → Plaintext
```

- **For Confidentiality:** Encrypt with **recipient's public key**; decrypt with **recipient's private key**.
- **For Authentication:** Encrypt (sign) with **sender's private key**; verify with **sender's public key**.

---

### Digital Signatures

**Definition:** A cryptographic mechanism that provides **authentication, integrity, and non-repudiation** — proving that a message came from a specific sender and was not altered.

**Process:**
```
SIGNING (Sender):
  Message → Hash Function (SHA-256) → Hash
  Hash → Encrypt with Sender's PRIVATE KEY → Digital Signature
  Send: Message + Digital Signature

VERIFICATION (Receiver):
  Received Signature → Decrypt with Sender's PUBLIC KEY → Hash₁
  Received Message → Hash Function → Hash₂
  If Hash₁ == Hash₂ → Signature is VALID (authentic, unmodified)
```

**Example:** When you download software, the vendor signs the installer with their private key. Your OS verifies the signature using the vendor's public key — ensuring the installer is genuine and unmodified.

**Provides:**
- **Authentication:** Proves sender identity.
- **Integrity:** Detects any modification.
- **Non-repudiation:** Sender cannot deny sending the message.

---

## (c) Secure E-Commerce Architecture Design — 5 Marks

### Architecture Diagram:
```
[Customer Browser]
       │ HTTPS (TLS 1.3)
       ▼
[Web Application Firewall (WAF)]
       │
[DMZ Zone]
  ┌────┴──────────┐
  │  Load Balancer│
  └────┬──────────┘
       │
  ┌────┴──────────┐
  │  Web Servers  │ (HTTPS)
  └────┬──────────┘
       │ Internal firewall
  ┌────┴──────────┐
  │ App Servers   │ (Business Logic)
  └────┬──────────┘
       │
  ┌────┴──────────┐
  │ Database      │ (Encrypted at rest)
  └───────────────┘
  
[Certificate Authority] → issues SSL/TLS certificates
[RADIUS/LDAP Server] → user authentication
[IDS/IPS] → intrusion detection
```

### Security Mechanisms

#### 1. Confidentiality
- **TLS 1.3** encrypts all customer-to-server communication using **AES-256-GCM**.
- Database encryption at rest using **AES-256**.
- **Example:** Customer's credit card number is encrypted in transit and at rest.

#### 2. Integrity
- **TLS** uses **HMAC (Hash-based MAC)** to detect any modification of data in transit.
- **Digital signatures** on transactions ensure orders cannot be tampered with.
- **Example:** SHA-256 hash of order data is signed — any modification is detectable.

#### 3. Authentication
- **SSL/TLS Certificate** (signed by trusted CA like DigiCert) authenticates the website to the customer — prevents phishing.
- **Multi-Factor Authentication (MFA)** for customer accounts.
- **OAuth 2.0 / JWT** for API authentication.
- **Example:** Customer sees padlock + verified domain in browser.

#### 4. Non-Repudiation
- **Digital signatures** on orders and payment confirmations: merchant signs with private key → customer can prove transaction occurred.
- **Audit logs** stored in tamper-proof, digitally signed log files.
- **Example:** Customer cannot deny placing an order; merchant cannot deny receiving payment.

#### 5. Firewall and IDS
- **WAF (Web Application Firewall):** Blocks SQL injection, XSS, DDoS.
- **IDS/IPS:** Detects and blocks suspicious traffic patterns.
- **DMZ:** Web servers are in a demilitarized zone — compromised web server cannot reach databases directly.

### Summary Table

| Goal | Mechanism |
|---|---|
| Confidentiality | TLS 1.3, AES-256 encryption |
| Integrity | HMAC in TLS, digital signatures |
| Authentication | X.509 certificates, MFA, OAuth |
| Non-repudiation | Digital signatures, audit logs |
| Availability | WAF, IDS/IPS, Load balancer, DDoS protection |

---

# Quick Revision Summary

| Q | Topic | Key Formulas / Key Points |
|---|---|---|
| 1 | OSI Model | 7 layers: Physical→Data Link→Network→Transport→Session→Presentation→Application |
| 2 | Transmission Media | Guided: TP, Coax, Fiber. Unguided: Radio, Micro, IR |
| 3 | Switching | Circuit: dedicated path. Packet: independent routing. Message: store-forward |
| 4 | CRC | Append n-1 zeros, XOR divide by generator, remainder = CRC |
| 5 | Sliding Window | GBN: sender=2ⁿ-1, receiver=1. SR: sender=receiver=2ⁿ⁻¹ |
| 6 | ALOHA | Pure: 18.4% max. Slotted: 36.8% max |
| 7 | 802.11 WLAN | CSMA/CA (not CD), RTS/CTS for hidden node |
| 8 | VLSM | Sort by size, allocate largest first, prefix = 32 - log₂(hosts+2) |
| 9 | IPv6 | 128-bit, fixed 40-byte header, no checksum, no fragmentation by routers |
| 10 | Dijkstra | Visit lowest cost unvisited node, update neighbors, repeat |
| 11 | Routing Protocols | RIP: hop count, max 15. OSPF: bandwidth cost, SPF. BGP: policy, AS_PATH |
| 12 | Transport Protocols | TCP: reliable, ordered. UDP: fast, unreliable. SCTP: multi-stream |
| 13 | TCP Congestion | SS: exponential. CA: linear. 3 dup ACK→Fast Retransmit+Recovery |
| 14 | QoS | Leaky: constant output. Token: burst allowed. DTN: store-carry-forward |
| 15 | App Layer Security | DNS, HTTP, SMTP, FTP. PKI: public key encrypt, private key sign |

---

*Prepared for Semester Examination — Computer Networks (CS)*
*All 15 Questions Covered | 15 Marks Each | Total: 225 Marks*
