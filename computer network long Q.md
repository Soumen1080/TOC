# Computer Networks (R21_CS601) — Detailed Solutions
## 12 Multi-Part Long Answer Questions

---

# Q1

## Q1(a) OSI Reference Model

**Definition:** The Open Systems Interconnection (OSI) model is a conceptual framework developed by ISO that standardizes communication functions of a system into **7 layers**, each with specific responsibilities, enabling interoperability between diverse systems and vendors.

### The 7 Layers (top to bottom)

| Layer No. | Layer Name | Key Function | Example Protocol/Device |
|---|---|---|---|
| 7 | Application | User interface, network services | HTTP, FTP, SMTP, DNS |
| 6 | Presentation | Data translation, encryption, compression | SSL/TLS, JPEG, ASCII |
| 5 | Session | Session establishment, dialog control | NetBIOS, RPC |
| 4 | Transport | End-to-end delivery, segmentation | TCP, UDP |
| 3 | Network | Routing, logical addressing | IP, ICMP, Routers |
| 2 | Data Link | Framing, MAC addressing, error detection | Ethernet, Switches |
| 1 | Physical | Bit transmission, cables, signals | Hubs, Cables, NICs |

### Diagram (conceptual)

```
 Sender                              Receiver
+-------------+                   +-------------+
| Application |<----------------->| Application |
+-------------+                   +-------------+
| Presentation|<----------------->| Presentation|
+-------------+                   +-------------+
|   Session   |<----------------->|   Session   |
+-------------+                   +-------------+
|  Transport  |<----------------->|  Transport  |
+-------------+                   +-------------+
|   Network   |<----------------->|   Network   |
+-------------+                   +-------------+
|  Data Link  |<----------------->|  Data Link  |
+-------------+                   +-------------+
|  Physical   |<--- Physical Link --->| Physical |
+-------------+                   +-------------+
```

### Layer-wise Functions with Examples

1. **Physical Layer** – Deals with raw bit transmission over a physical medium: voltage levels, cable types, data rates, topology, connectors.
   - *Example:* Ethernet cable (RJ-45), fiber optic cable, hubs and repeaters operate here. Converts bits (0s/1s) into electrical/optical/radio signals.

2. **Data Link Layer** – Provides node-to-node delivery, framing, MAC (physical) addressing, error detection/correction, and flow control over a single link.
   - *Example:* Switches operate here. Ethernet protocol adds source/destination MAC addresses and a CRC checksum to each frame.

3. **Network Layer** – Provides logical addressing (IP addresses) and routing of packets across different networks to find the best path.
   - *Example:* Routers use IP protocol to forward packets from a source network to a destination network across the internet.

4. **Transport Layer** – Provides end-to-end communication between processes on source and destination hosts; handles segmentation, reassembly, flow control, and error control.
   - *Example:* TCP (reliable, connection-oriented, used for web browsing/email) vs UDP (unreliable, connectionless, used for video streaming/DNS).

5. **Session Layer** – Establishes, manages, synchronizes, and terminates sessions (dialogs) between applications on two devices.
   - *Example:* Maintaining a login session, checkpointing during a large file transfer so it can resume after interruption.

6. **Presentation Layer** – Handles data translation between application format and network format: encryption/decryption, compression, character encoding conversion.
   - *Example:* Converting EBCDIC to ASCII, SSL/TLS encryption of HTTPS traffic, JPEG/MP3 encoding/decoding.

7. **Application Layer** – Provides network services directly to end-user applications; closest layer to the user.
   - *Example:* HTTP (web browsing), FTP (file transfer), SMTP (email sending), DNS (name resolution).

### Data Flow — Encapsulation/Decapsulation

```
Application Data
   ↓ + Header (Presentation)
   ↓ + Header (Session)
   ↓ + Header (Transport)  → called a SEGMENT
   ↓ + Header (Network)    → called a PACKET
   ↓ + Header (Data Link)  → called a FRAME
   ↓ → → → Bits transmitted on wire (Physical)
```
At the receiver, each layer strips off its corresponding header — this is **decapsulation**.

---

## Q1(b) Comparison of OSI and TCP/IP Models

**TCP/IP Model Definition:** The TCP/IP (Transmission Control Protocol/Internet Protocol) model is a practical, **4-layer** protocol suite (sometimes shown with 5 layers) that forms the actual foundation of the modern Internet. It was developed before OSI and is implementation-driven rather than purely theoretical.

### TCP/IP Layers

```
4. Application       (merges OSI's Application + Presentation + Session)
3. Transport
2. Internet          (= OSI's Network layer)
1. Network Access /  (= OSI's Data Link + Physical)
   Link Layer
```

### Layer Mapping Table

| OSI Layer | TCP/IP Layer | Example Protocols |
|---|---|---|
| Application | Application | HTTP, FTP, SMTP, DNS, Telnet |
| Presentation | Application | SSL/TLS, JPEG, MPEG |
| Session | Application | NetBIOS, RPC, SIP |
| Transport | Transport | TCP, UDP |
| Network | Internet | IP, ICMP, ARP, IGMP |
| Data Link | Network Access | Ethernet, PPP, Frame Relay |
| Physical | Network Access | Cables, hubs, NICs |

### Key Differences

| Basis | OSI Model | TCP/IP Model |
|---|---|---|
| Number of layers | 7 | 4 (sometimes 5) |
| Development approach | Theoretical model first, protocols designed later | Protocols (TCP, IP) existed first, model described them |
| Usage | Reference/teaching model, rarely implemented as-is | Actually implemented; backbone of the Internet |
| Layer boundaries | Strict, each layer clearly separated | More loosely defined, some layers merged |
| Reliability | Reliability is guaranteed at the Transport layer (connection-oriented mode possible) | Reliability depends on protocol used (TCP = reliable, UDP = unreliable) |
| Session/Presentation layers | Present as separate layers | Not present separately; functions absorbed into Application layer |
| Protocol independence | Model is independent of protocols — generic | Model is tightly coupled to TCP and IP protocols |
| Approach | Vertical approach (layer-by-layer service definition) | Horizontal approach (protocol-driven) |

### Example Illustration

When you open a website (`https://example.com`):

- **Application layer (both models):** Browser uses HTTP/HTTPS to request the page.
- **Transport layer (both models):** TCP establishes a reliable connection (3-way handshake), breaks data into segments.
- **Network/Internet layer (both models):** IP addresses source and destination, routers forward packets hop by hop.
- **Data Link + Physical (OSI) / Network Access (TCP/IP):** Ethernet frames carry packets over the physical cable/Wi-Fi to the next hop.

This shows that although OSI has more layers, both models describe the *same* journey of data — OSI simply subdivides the upper and lower layers in more detail.

---

# Q2

## Q2(a) Network Topologies and Their Characteristics

**Definition:** Network topology refers to the **arrangement/layout** of nodes (computers, switches, etc.) and links in a computer network, describing how devices are physically or logically connected to each other.

### Types of Topologies

#### 1. Bus Topology
- All devices are connected to a single central cable (the "bus" or "backbone").
- **Characteristics:** Easy to install, low cost, but a single cable failure brings down the whole network; performance degrades as more devices are added (collision domain shared).
- *Example:* Old Ethernet networks using coaxial cable (10BASE2).

```
   Device1   Device2   Device3   Device4
      |         |         |         |
======●=========●=========●=========●======  (Bus/Backbone cable)
```

#### 2. Star Topology
- All devices connect individually to a central device (hub/switch).
- **Characteristics:** Easy to install and manage, failure of one cable doesn't affect others, but failure of the central hub brings down the entire network. Most common in modern LANs.
- *Example:* Typical office LAN with a central switch.

```
        Device1
           |
Device4 — [Switch] — Device2
           |
        Device3
```

#### 3. Ring Topology
- Each device is connected to exactly two other devices, forming a circular path for signals.
- **Characteristics:** Data travels in one (or both) direction(s); reduces collisions, but a single break in the ring can disrupt the network (unless dual ring is used, as in FDDI).
- *Example:* Token Ring networks, FDDI.

```
Device1 — Device2
   |          |
Device4 — Device3
(forms a closed loop)
```

#### 4. Mesh Topology
- Every device is connected to every other device (full mesh) or to many others (partial mesh).
- **Characteristics:** Highly reliable (multiple paths), no single point of failure, but very expensive and complex to wire — requires n(n-1)/2 links for full mesh of n nodes.
- *Example:* Backbone of the Internet between major routers/ISPs.

#### 5. Tree (Hierarchical) Topology
- A combination of star topologies connected via a bus; forms a hierarchical/parent-child structure.
- **Characteristics:** Scalable, easy to expand and troubleshoot, but if the root/parent node fails, the entire sub-tree below it is affected.
- *Example:* Corporate networks with main switch connected to floor switches connected to department switches.

#### 6. Hybrid Topology
- A combination of two or more different topologies (e.g., star-bus, star-ring).
- **Characteristics:** Flexible and scalable, combines advantages of multiple topologies, but design and management can be complex.
- *Example:* Large campus networks combining star topology in each building with a bus/ring backbone connecting buildings.

### Comparison Table

| Topology | Cost | Fault Tolerance | Scalability | Cable Required |
|---|---|---|---|---|
| Bus | Low | Low (single point of failure) | Low | Least |
| Star | Medium | Medium (hub failure = total failure) | High | Medium |
| Ring | Medium | Low (single break disrupts) | Medium | Medium |
| Mesh | Very High | Very High | Low (complex) | Highest |
| Tree | Medium | Medium | High | Medium |
| Hybrid | High | High | Very High | High |

---

## Q2(b) Guided and Unguided Transmission Media

**Definition:** Transmission media is the physical pathway through which data travels from sender to receiver. It is broadly classified into **Guided (Wired)** media, where signals travel through a solid physical medium, and **Unguided (Wireless)** media, where signals propagate through free space (air/vacuum).

### Guided Transmission Media

#### 1. Twisted Pair Cable
- Two insulated copper wires twisted together to reduce electromagnetic interference (crosstalk).
- **Types:** UTP (Unshielded Twisted Pair) — cheaper, common in LANs; STP (Shielded Twisted Pair) — extra shielding, reduces noise further.
- **Characteristics:** Low cost, easy to install, but susceptible to noise/attenuation over long distances (~100m for Ethernet).
- *Example:* CAT5e/CAT6 cables used for Ethernet LAN connections.

#### 2. Coaxial Cable
- A central copper conductor surrounded by insulation, metallic shield, and outer jacket.
- **Characteristics:** Better shielding than twisted pair, supports higher bandwidth and longer distances, but bulkier and more expensive.
- *Example:* Cable TV connections, older Ethernet (10BASE2).

#### 3. Fiber Optic Cable
- Transmits data as light pulses through a glass or plastic core.
- **Types:** Single-mode (long distance, laser source) and Multi-mode (shorter distance, LED source).
- **Characteristics:** Extremely high bandwidth, immune to electromagnetic interference, very low attenuation, supports very long distances, but expensive and fragile, requires special equipment for splicing.
- *Example:* Backbone of internet service providers, undersea communication cables.

### Comparison of Guided Media

| Medium | Bandwidth | Attenuation | Cost | Interference Immunity |
|---|---|---|---|---|
| Twisted Pair | Low–Medium | High | Low | Low |
| Coaxial | Medium–High | Medium | Medium | Medium |
| Fiber Optic | Very High | Very Low | High | Very High |

### Unguided Transmission Media (Wireless)

#### 1. Radio Waves
- Frequency range: 3 kHz – 1 GHz. Omnidirectional, can penetrate walls.
- **Characteristics:** Easy to generate, travel long distances, but susceptible to interference from other devices.
- *Example:* AM/FM radio broadcasting, cordless phones.

#### 2. Microwaves
- Frequency range: 1 GHz – 300 GHz. Require line-of-sight between antennas (unidirectional).
- **Types:** Terrestrial microwave (using parabolic dish antennas) and Satellite microwave (using satellites as relay stations).
- **Characteristics:** Higher bandwidth than radio waves, but affected by weather conditions (rain attenuation) and requires unobstructed line of sight.
- *Example:* Satellite TV, long-distance telephone communication, Wi-Fi (uses microwave frequencies like 2.4 GHz/5 GHz).

#### 3. Infrared Waves
- Frequency range: 300 GHz – 400 THz. Short range, cannot penetrate walls.
- **Characteristics:** Used for very short-range communication, secure (cannot pass through solid objects, hence no interference from adjacent rooms), but line-of-sight required.
- *Example:* TV remote controls, IR-based wireless mouse/keyboard, older mobile data transfer (IrDA).

### Comparison of Unguided Media

| Medium | Frequency Range | Directionality | Example Use |
|---|---|---|---|
| Radio Waves | 3 kHz – 1 GHz | Omnidirectional | Radio broadcast, cordless phones |
| Microwaves | 1 GHz – 300 GHz | Unidirectional (line-of-sight) | Satellite, Wi-Fi |
| Infrared | 300 GHz – 400 THz | Line-of-sight, short range | TV remotes |

---

# Q3

## Q3(a) Framing and Error Control Techniques in Data Link Layer

### Framing

**Definition:** Framing is the process by which the Data Link Layer divides the stream of bits received from the Network Layer into manageable units called **frames**, adding header and trailer information so the receiver can identify the start and end of each frame.

### Framing Methods

#### 1. Character/Byte Stuffing (Byte-Oriented Framing)
- Uses special characters: **FLAG** (e.g., a defined byte) to mark the beginning/end of a frame, and **ESC** (escape character) to "stuff" before any data byte that accidentally matches the FLAG byte, so the receiver doesn't misinterpret it as a frame boundary.
- *Example:* If FLAG = `01111110` appears in actual data, an ESC byte is inserted before it: `ESC 01111110`. If ESC itself appears in data, it is stuffed with another ESC: `ESC ESC`.

#### 2. Bit Stuffing (Bit-Oriented Framing)
- Uses a special bit pattern (commonly `01111110`, used in HDLC) as the flag. Whenever **five consecutive 1s** appear in the data, a `0` is automatically inserted (stuffed) after them by the sender. The receiver removes this extra `0` upon seeing five 1s followed by a 0.
- *Example:* Data `011111 1` → after stuffing becomes `0111110 1` (a `0` inserted after five 1's), preventing it from being confused with the flag `01111110`.

#### 3. Length Field Based Framing
- The frame header contains a field specifying the length of the frame, allowing the receiver to know exactly how many bytes to read.
- *Drawback:* If the length field gets corrupted during transmission, framing breaks entirely (used in older protocols, e.g., DDCMP).

### Error Control Techniques

**Definition:** Error control refers to mechanisms to detect and correct errors that occur during transmission of bits due to noise, attenuation, or interference.

#### 1. Error Detection

**(i) Parity Check**
- A single extra bit (parity bit) is added to make the total number of 1s either even (even parity) or odd (odd parity).
- *Example:* Data `1011001` (four 1s) — for even parity, append `0` → `10110010` (still four 1s = even). If a single bit flips during transmission, the parity check fails, signaling an error. *Limitation:* Cannot detect even number of bit errors.

**(ii) Checksum**
- Data is divided into segments, segments are summed (with wraparound), and the complement of the sum is sent as the checksum. Receiver adds all segments + checksum; if result is all 1s (or zero), no error detected.
- *Example:* Used in TCP/IP/UDP headers for error detection.

**(iii) Cyclic Redundancy Check (CRC)**
- A polynomial division technique: data is treated as a binary polynomial, divided by a generator polynomial, and the remainder (CRC bits) is appended to the data. The receiver performs the same division; a non-zero remainder indicates an error.
- *Example:* CRC-32 is widely used in Ethernet frames — highly effective at detecting burst errors.

#### 2. Error Correction

**(i) Hamming Code**
- Adds multiple redundant (parity) bits at specific positions (powers of 2: 1, 2, 4, 8...) so that not only can errors be detected, but the **exact bit position** of a single-bit error can be identified and corrected.
- *Example:* For 4 data bits, 3 redundant bits are needed (2^r ≥ m+r+1), giving a 7-bit Hamming code capable of single-bit error correction.

**(ii) Forward Error Correction (FEC) / Retransmission (ARQ)**
- FEC: Receiver corrects errors itself using redundant codes (no need to ask sender to resend) — suitable for real-time applications.
- ARQ (Automatic Repeat reQuest): Receiver detects error and requests retransmission from sender (covered in Q3(b)).

---

## Q3(b) Stop-and-Wait ARQ Protocol

**Definition:** Stop-and-Wait ARQ (Automatic Repeat reQuest) is the simplest flow and error control protocol in which the **sender transmits one frame and then waits for an acknowledgment (ACK)** from the receiver before sending the next frame. It combines flow control (one frame at a time) with error control (retransmission on error/timeout).

### Working Principle

1. Sender transmits **Frame 0**.
2. Sender starts a **timer** and waits.
3. Receiver receives Frame 0, checks for errors:
   - If correct → sends **ACK 0**.
   - If erroneous → discards the frame (sends nothing, or a NAK depending on variant).
4. Sender, upon receiving ACK 0, sends **Frame 1**, and so on.
5. If the **timer expires** before an ACK arrives (due to frame loss or ACK loss), the sender **retransmits** the same frame.

### Diagram — Normal Operation

```
Sender                          Receiver
  |--------- Frame 0 ----------->|
  |  (timer starts)               |
  |<---------- ACK 0 -------------|
  |--------- Frame 1 ----------->|
  |<---------- ACK 1 -------------|
```

### Diagram — Frame Lost (Timeout)

```
Sender                          Receiver
  |--------- Frame 0 ----------->|   X  (frame lost in transit)
  |  (timer starts)               |
  |  (timer expires - no ACK)     |
  |--------- Frame 0 (resent) --->|
  |<---------- ACK 0 -------------|
```

### Diagram — ACK Lost

```
Sender                          Receiver
  |--------- Frame 0 ----------->|
  |                               | (Frame 0 received correctly)
  |       X  ACK 0 lost           |<--- ACK 0 sent but lost
  |  (timer expires)              |
  |--------- Frame 0 (resent) --->| (receiver gets duplicate)
  |<---------- ACK 0 -------------|
```
*Note:* To handle duplicate frames (as in the ACK-lost case), frames and ACKs use **sequence numbers (0 and 1, alternating)** — this is called "alternating bit protocol."

### Characteristics

- **Advantages:** Simple to implement, requires only 1-bit sequence numbers, minimal buffer requirement (only 1 frame at a time).
- **Disadvantages:** Very inefficient for high-bandwidth or long-delay links — the sender remains idle while waiting for ACK, leading to poor **channel utilization**.

### Efficiency Calculation Example

If transmission time of a frame `Tt = 1 ms` and propagation delay `Tp = 5 ms` (one-way), then:

```
Total cycle time = Tt + 2×Tp + Tt(ACK, often negligible)
                 = 1 + 2(5) + (negligible)
                 = 11 ms

Efficiency (η) = Tt / (Tt + 2Tp) = 1 / 11 ≈ 9.1%
```

This shows that for links with high propagation delay (e.g., satellite links), Stop-and-Wait wastes most of the time idle — motivating sliding window protocols like **Go-Back-N** and **Selective Repeat** (Q4).

---

# Q4

## Q4(a) Go-Back-N and Selective Repeat Protocols

**Definition:** These are **sliding window protocols** that improve on Stop-and-Wait by allowing the sender to transmit **multiple frames** before requiring an acknowledgment, significantly improving channel utilization on links with high propagation delay.

### Go-Back-N (GBN) ARQ

- Sender can send up to **N frames** (window size) without waiting for individual ACKs.
- Receiver uses a window size of **1** — it only accepts frames in correct sequence order.
- If a frame is lost or corrupted, the receiver **discards all subsequent frames** (even if correct) until it gets the expected one.
- Sender, on timeout or receiving a NAK, **retransmits ALL frames** starting from the lost one — hence "Go-Back-N".
- Uses **cumulative acknowledgments**: ACK n means "all frames up to n-1 received correctly, expecting frame n next."

**Diagram — Frame 2 Lost:**
```
Sender                     Receiver
 |--- F0 --->|              
 |--- F1 --->|              
 |--- F2 -X->|   (lost)
 |--- F3 --->|   discarded (out of order)
 |--- F4 --->|   discarded (out of order)
 |<-- ACK1 --|   (cumulative ACK for F0,F1)
 (timeout for F2)
 |--- F2 --->|
 |--- F3 --->|   (resent - go back to 2)
 |--- F4 --->|   (resent)
 |<-- ACK4 --|
```

- **Window size formula:** For GBN, sender window size must satisfy `Ws ≤ 2^m - 1`, where `m` = number of bits in sequence number.

### Selective Repeat (SR) ARQ

- Both sender and receiver maintain a window of size **N**.
- Receiver **buffers out-of-order frames** and sends **individual ACKs** for each correctly received frame.
- On timeout, sender retransmits **only the specific lost frame**, not all subsequent frames — more efficient but requires more buffer and processing at receiver.
- **Window size formula:** `Ws = Wr ≤ 2^(m-1)` (sender and receiver window sizes must be equal and at most half of sequence number space, to avoid ambiguity).

**Diagram — Frame 2 Lost:**
```
Sender                     Receiver
 |--- F0 --->|  <-- ACK0 --|
 |--- F1 --->|  <-- ACK1 --|
 |--- F2 -X->|   (lost)
 |--- F3 --->|   buffered   <-- ACK3 --|
 |--- F4 --->|   buffered   <-- ACK4 --|
 (timeout for F2)
 |--- F2 --->|   (only F2 resent)
 |<-- ACK2 --|   (F2,F3,F4 now delivered in order to upper layer)
```

### Comparison Table

| Feature | Go-Back-N | Selective Repeat |
|---|---|---|
| Receiver window size | 1 | N (>1) |
| Acknowledgment type | Cumulative | Individual |
| Retransmission | All frames from lost frame onward | Only the lost/erroneous frame |
| Receiver buffer | Minimal | Larger (must buffer out-of-order frames) |
| Efficiency | Lower (wastes bandwidth on retransmission) | Higher (efficient use of bandwidth) |
| Complexity | Simple | More complex |

---

## Q4(b) CSMA/CD vs ALOHA

### ALOHA

**Definition:** ALOHA is a random-access protocol where a station transmits data **whenever it has data to send**, without checking if the channel is busy. If a collision occurs (two stations transmit simultaneously), both retransmit after a random backoff time.

#### Pure ALOHA
- A station can transmit at **any time**.
- Vulnerable time for collision = **2 × Tt** (frame transmission time), since a frame can collide with another frame that started up to one Tt before or after it.
- **Maximum throughput ≈ 18.4%** (efficiency = 1/(2e)).

#### Slotted ALOHA
- Time is divided into discrete **slots** equal to frame transmission time; a station can only transmit at the beginning of a slot.
- Reduces collision probability by half.
- Vulnerable time = **Tt**.
- **Maximum throughput ≈ 36.8%** (efficiency = 1/e).

```
Pure ALOHA:    Station can send anytime →  [---frame---]
                                       collision window = 2×Tt

Slotted ALOHA: |slot|slot|slot|slot|  → send only at slot boundary
                                       collision window = Tt
```

### CSMA/CD (Carrier Sense Multiple Access with Collision Detection)

**Definition:** CSMA/CD is a media access control method used in wired LANs (classic Ethernet) where a station **listens to the channel before transmitting** (carrier sense), and if a collision is detected during transmission, it **stops immediately**, sends a jam signal, and retransmits after a random backoff using the **binary exponential backoff** algorithm.

#### Working Steps:
1. **Sense** the channel — if idle, transmit; if busy, wait.
2. While transmitting, **continue sensing** for collisions (by comparing transmitted and received signal).
3. If a collision is detected → **stop transmission immediately**, send a brief **jam signal** to notify all stations.
4. Wait for a **random backoff time** (using binary exponential backoff: wait time doubles with each successive collision, up to a limit) and retry.

```
Station A: Sense channel idle → Start transmitting
Station B: Sense channel idle (A's signal not yet arrived) → Start transmitting
           ↓
        COLLISION detected by both
           ↓
   Both stop, send jam signal, wait random backoff time, retry
```

### Comparison Table

| Feature | ALOHA | CSMA/CD |
|---|---|---|
| Carrier sensing | No (transmits regardless of channel state) | Yes (senses channel before transmitting) |
| Collision detection | No (detected only via lack of ACK) | Yes (detected during transmission) |
| Efficiency | Low (Pure: 18.4%, Slotted: 36.8%) | Much higher (~efficient for LANs) |
| Usage | Satellite/wireless packet networks | Traditional wired Ethernet (10/100 Mbps) |
| Collision handling | Random retransmission after timeout | Immediate stop + jam signal + binary exponential backoff |

---

# Q5

## Q5(a) Ethernet Standards: Fast Ethernet and Gigabit Ethernet

**Definition:** Ethernet is a family of wired LAN technologies standardized under **IEEE 802.3**, defining the physical layer and data link layer's MAC sublayer for local area networks, using CSMA/CD (in legacy shared-media versions) and frame formats for data transmission.

### Evolution of Ethernet Standards

| Standard | IEEE | Speed | Cable Type | Max Distance |
|---|---|---|---|---|
| Ethernet | 802.3 | 10 Mbps | Coaxial/UTP (10BASE-T) | 100 m (UTP) |
| Fast Ethernet | 802.3u | 100 Mbps | UTP Cat5 (100BASE-TX) | 100 m |
| Gigabit Ethernet | 802.3z/802.3ab | 1000 Mbps (1 Gbps) | Fiber (1000BASE-X) / UTP Cat5e (1000BASE-T) | 100 m (copper), up to 5 km (fiber) |
| 10 Gigabit Ethernet | 802.3ae | 10 Gbps | Fiber / Cat6a | Varies (up to 40 km on fiber) |

### Fast Ethernet (100 Mbps) — IEEE 802.3u
- Introduced in 1995, increased speed 10× over original Ethernet while retaining the same frame format and CSMA/CD (in half-duplex mode).
- **Variants:**
  - **100BASE-TX:** Uses two pairs of Cat5 UTP cable, most common.
  - **100BASE-FX:** Uses fiber optic cable for longer distances.
  - **100BASE-T4:** Uses four pairs of Cat3 UTP (legacy, rarely used now).
- **Characteristics:** Backward compatible with 10BASE-T (auto-negotiation), supports both half-duplex and full-duplex modes.

### Gigabit Ethernet (1000 Mbps) — IEEE 802.3z (fiber) / 802.3ab (copper)
- Achieves 1 Gbps by using more efficient encoding and all four pairs of UTP cable simultaneously (in 1000BASE-T).
- **Variants:**
  - **1000BASE-T:** Uses Cat5e/Cat6 UTP cable, up to 100m — common in modern LANs/desktops.
  - **1000BASE-SX:** Short-wavelength fiber (multimode), up to ~550m.
  - **1000BASE-LX:** Long-wavelength fiber (single-mode), up to several km.
- **Characteristics:** Operates almost exclusively in **full-duplex mode** (no collisions, so CSMA/CD is effectively unused), uses switches instead of hubs.

### Why These Matter (Example)
A typical modern office LAN: end-user desktops connect via **1000BASE-T (Gigabit Ethernet)** to an access switch, which connects via **10 Gigabit fiber uplinks** to the core switch — demonstrating the layered speed hierarchy in real deployments.

---

## Q5(b) VLANs and Switching Techniques

### VLAN (Virtual Local Area Network)

**Definition:** A VLAN is a logical grouping of devices on one or more physical LANs that allows devices to communicate as if they were on the same physical network segment, **regardless of their physical location**, by using switches that tag frames with VLAN identifiers.

#### Why VLANs are Used
- **Broadcast domain segmentation:** Reduces broadcast traffic by dividing a large LAN into smaller logical segments.
- **Security:** Isolates sensitive departments (e.g., HR, Finance) from general traffic even if on the same physical switch.
- **Flexibility:** Devices can be grouped logically (e.g., by department) rather than by physical location/cabling.

#### VLAN Tagging — IEEE 802.1Q
- A 4-byte **VLAN tag** is inserted into the Ethernet frame header, containing a **VLAN ID (12 bits, supports up to 4096 VLANs)**.
- **Trunk ports** carry tagged traffic for multiple VLANs between switches; **access ports** carry traffic for a single VLAN to end devices.

```
Original Ethernet Frame:
| Dest MAC | Src MAC | Type | Data | CRC |

802.1Q Tagged Frame:
| Dest MAC | Src MAC | 802.1Q Tag (VLAN ID) | Type | Data | CRC |
```

#### Example
A company has employees from **Sales (VLAN 10)** and **Engineering (VLAN 20)** on the same floor connected to the same physical switch. VLANs ensure that broadcast traffic from Sales PCs doesn't reach Engineering PCs, even though they share the same switch hardware — improving both security and performance.

### Switching Techniques

**Definition:** Switching techniques define how a network switch forwards/processes frames from input ports to output ports.

#### 1. Store-and-Forward Switching
- The switch receives the **entire frame**, stores it in buffer, checks for errors (CRC), and only then forwards it.
- **Pros:** Error checking prevents propagation of corrupted frames. **Cons:** Higher latency (must wait for entire frame).

#### 2. Cut-Through Switching
- The switch starts forwarding the frame **as soon as the destination MAC address** (in the header) is read — without waiting for the whole frame.
- **Pros:** Very low latency. **Cons:** No error checking; corrupted frames may be forwarded.

#### 3. Fragment-Free Switching
- A compromise: the switch reads the first **64 bytes** of the frame (enough to detect most collision-related errors/fragments) before forwarding.
- **Pros:** Balances latency and error checking. **Cons:** Still doesn't catch all errors (e.g., CRC errors beyond 64 bytes).

### Comparison Table

| Technique | Latency | Error Checking | Use Case |
|---|---|---|---|
| Store-and-Forward | High | Full (CRC check) | High reliability needed |
| Cut-Through | Lowest | None | Low latency critical (e.g., HPC) |
| Fragment-Free | Medium | Partial (first 64 bytes) | Balanced approach |

---

# Q6

## Q6(a) IPv4 Addressing and Subnetting

### IPv4 Addressing

**Definition:** IPv4 (Internet Protocol version 4) uses **32-bit addresses**, typically represented in **dotted-decimal notation** (four octets separated by dots, each ranging 0–255), to uniquely identify a device's network interface on a network, providing approximately 4.3 billion (2^32) unique addresses.

#### IPv4 Address Classes

| Class | Leading Bits | Range of First Octet | Default Subnet Mask | Use |
|---|---|---|---|---|
| A | 0 | 1 – 126 | 255.0.0.0 (/8) | Large networks |
| B | 10 | 128 – 191 | 255.255.0.0 (/16) | Medium networks |
| C | 110 | 192 – 223 | 255.255.255.0 (/24) | Small networks |
| D | 1110 | 224 – 239 | N/A (Multicast) | Multicasting |
| E | 1111 | 240 – 255 | N/A (Reserved) | Experimental |

*(127.x.x.x is reserved for loopback; 10.x.x.x, 172.16-31.x.x, 192.168.x.x are private address ranges per RFC 1918.)*

### Subnetting

**Definition:** Subnetting is the process of dividing a single large network into multiple smaller sub-networks (subnets) by **borrowing bits from the host portion** of an IP address to create additional network bits, improving address utilization and network organization.

#### Subnet Mask
- A 32-bit mask that distinguishes the **network portion** (1s) from the **host portion** (0s) of an IP address.
- *Example:* `255.255.255.0` (/24) means the first 24 bits are network bits, last 8 bits are host bits → 2^8 = 256 addresses (254 usable, excluding network and broadcast addresses).

#### Subnetting Example — Step by Step

**Problem:** Given the network `192.168.1.0/24`, divide it into **4 subnets**.

**Step 1:** To create 4 subnets, we need `2^n ≥ 4` → n = 2 bits borrowed from the host portion.

**Step 2:** New subnet mask = /24 + 2 = **/26** = `255.255.255.192`

**Step 3:** Each subnet size = 2^(32-26) = 2^6 = **64 addresses** (62 usable hosts per subnet).

**Resulting Subnets:**

| Subnet | Network Address | Usable Host Range | Broadcast Address |
|---|---|---|---|
| 1 | 192.168.1.0/26 | 192.168.1.1 – 192.168.1.62 | 192.168.1.63 |
| 2 | 192.168.1.64/26 | 192.168.1.65 – 192.168.1.126 | 192.168.1.127 |
| 3 | 192.168.1.128/26 | 192.168.1.129 – 192.168.1.190 | 192.168.1.191 |
| 4 | 192.168.1.192/26 | 192.168.1.193 – 192.168.1.254 | 192.168.1.255 |

#### Formula Summary

```
Number of subnets       = 2^(borrowed bits)
Number of hosts/subnet  = 2^(remaining host bits) - 2
                          (subtract 2 for network address & broadcast address)
```

#### Another Example — VLSM Concept
For a `/24` network needing departments with 100, 50, 25, and 25 hosts:
- Department A (100 hosts) needs `2^7=128 ≥ 100+2` → /25 (126 usable)
- Department B (50 hosts) needs `2^6=64 ≥ 50+2` → /26 (62 usable)
- Department C, D (25 hosts each) need `2^5=32 ≥ 25+2` → /27 each (30 usable)

This is **Variable Length Subnet Masking (VLSM)** — allocating different subnet sizes based on actual need, avoiding address wastage.

---

## Q6(b) Advantages of IPv6 over IPv4

**Definition:** IPv6 (Internet Protocol version 6) is the successor to IPv4, using **128-bit addresses** (vs IPv4's 32-bit), represented in **eight groups of four hexadecimal digits** separated by colons (e.g., `2001:0db8:85a3:0000:0000:8a2e:0370:7334`), designed primarily to solve IPv4 address exhaustion and improve protocol efficiency.

### Key Advantages

#### 1. Vastly Larger Address Space
- IPv4: 2^32 ≈ 4.3 billion addresses (already exhausted globally).
- IPv6: 2^128 ≈ 3.4 × 10^38 addresses — enough for every device on Earth to have multiple unique addresses, supporting IoT growth.

#### 2. Simplified Header Format
- IPv6 header is **fixed at 40 bytes** with fewer fields (8 fields vs IPv4's 13), enabling faster processing by routers.
- IPv4 header includes a variable-length "Options" field requiring extra processing; IPv6 moves optional information to **extension headers**, processed only when needed.

#### 3. No Need for NAT (Network Address Translation)
- IPv4's limited addresses forced widespread use of NAT (private IPs shared via one public IP), which breaks true end-to-end connectivity and complicates peer-to-peer applications.
- IPv6's abundant address space allows every device to have a globally unique address, restoring end-to-end connectivity.

#### 4. Built-in Security (IPSec)
- IPv6 was designed with **IPSec (Internet Protocol Security)** as a mandatory/integral part of the protocol suite, providing authentication and encryption at the network layer by default.
- In IPv4, IPSec is optional and added separately.

#### 5. Auto-configuration (Stateless Address Autoconfiguration - SLAAC)
- IPv6 devices can automatically configure their own addresses using **SLAAC**, without needing a DHCP server, simplifying network administration.

#### 6. Improved Multicast and Anycast Support
- IPv6 eliminates broadcast entirely (replaced by multicast), reducing unnecessary network traffic; native anycast support improves routing to nearest server (e.g., CDN, DNS root servers).

#### 7. Better Quality of Service (QoS)
- IPv6 header includes a **Flow Label** field, allowing routers to identify packets belonging to the same flow (e.g., a video stream) and apply consistent QoS handling without deep packet inspection.

#### 8. No Checksum Field at Network Layer
- IPv4 header includes a checksum recalculated at every hop (adding overhead); IPv6 removes this, relying on error-checking at the Data Link and Transport layers, speeding up router processing.

### Comparison Table

| Feature | IPv4 | IPv6 |
|---|---|---|
| Address length | 32 bits | 128 bits |
| Address notation | Dotted decimal (e.g., 192.168.1.1) | Hexadecimal colon-separated (e.g., 2001:db8::1) |
| Header size | Variable (20–60 bytes) | Fixed (40 bytes) |
| Address configuration | Manual / DHCP | SLAAC / DHCPv6 |
| Security | Optional (IPSec add-on) | Mandatory/built-in IPSec |
| NAT requirement | Common (due to address scarcity) | Not required |
| Broadcast | Supported | Not supported (uses multicast) |
| Checksum in header | Present | Absent (handled at other layers) |

### Example: IPv6 Address Notation
```
Full:        2001:0db8:0000:0000:0000:ff00:0042:8329
Compressed:  2001:db8::ff00:42:8329   (consecutive zeros replaced by ::, used only once)
```

---

# Q7

## Q7(a) Functions of ARP, ICMP, and DHCP Protocols

### ARP (Address Resolution Protocol)

**Definition:** ARP is a network layer protocol used to map a known **IP address (logical address)** to its corresponding **MAC address (physical address)** on a local network, since data link layer devices (switches) forward frames based on MAC addresses, not IP addresses.

#### Working
1. Host A wants to send data to Host B (knows B's IP, not MAC).
2. Host A **broadcasts** an ARP Request: "Who has IP 192.168.1.5? Tell 192.168.1.2."
3. All hosts on the LAN receive this broadcast, but only Host B (owner of 192.168.1.5) replies.
4. Host B sends a **unicast ARP Reply** containing its MAC address.
5. Host A caches this mapping in its **ARP cache/table** for future use (avoiding repeated broadcasts).

```
Host A (192.168.1.2)                    Host B (192.168.1.5)
       |--- ARP Request (broadcast) -------->| (and all others)
       |    "Who has 192.168.1.5?"           |
       |<-------- ARP Reply (unicast) -------|
       |    "192.168.1.5 is at MAC: xx:xx"   |
```

*Example:* This happens every time your computer needs to talk to another device on the same Wi-Fi/LAN for the first time.

### ICMP (Internet Control Message Protocol)

**Definition:** ICMP is a network layer protocol used by hosts and routers to send **error messages and operational information** (e.g., a requested service is unavailable, or a host/router is unreachable) — it is NOT used for regular data exchange but for diagnostics and control.

#### Common ICMP Message Types

| Message Type | Purpose |
|---|---|
| Echo Request/Reply | Used by `ping` to test reachability |
| Destination Unreachable | Sent when a packet cannot reach its destination (host/network/port unreachable) |
| Time Exceeded | Sent when a packet's TTL (Time To Live) reaches 0 (used by `traceroute`) |
| Redirect | Informs a host of a better route to a destination |
| Source Quench | (Deprecated) Requested sender to slow down due to congestion |

#### Example: `ping` Command
When you run `ping google.com`, your computer sends **ICMP Echo Request** packets; if google.com's server is reachable, it replies with **ICMP Echo Reply** packets, and `ping` reports round-trip time (RTT).

#### Example: `traceroute`
Sends packets with increasing TTL (1, 2, 3...); each router along the path that decrements TTL to 0 sends back an **ICMP Time Exceeded** message, revealing the path hop-by-hop.

### DHCP (Dynamic Host Configuration Protocol)

**Definition:** DHCP is an application layer protocol that automatically assigns IP addresses and other network configuration parameters (subnet mask, default gateway, DNS server) to hosts on a network, eliminating the need for manual configuration.

#### DHCP Process — DORA

1. **Discover:** Client broadcasts a `DHCPDISCOVER` message to find available DHCP servers.
2. **Offer:** DHCP server(s) respond with `DHCPOFFER`, proposing an IP address and configuration.
3. **Request:** Client broadcasts `DHCPREQUEST`, accepting one specific offer (and notifying other servers their offers are declined).
4. **Acknowledge:** Server responds with `DHCPACK`, confirming the lease of the IP address for a specified **lease time**.

```
Client                              DHCP Server
   |---- DHCPDISCOVER (broadcast) ------>|
   |<--------- DHCPOFFER -----------------|
   |---- DHCPREQUEST (broadcast) -------->|
   |<--------- DHCPACK --------------------|
```

*Example:* When you connect your laptop to a new Wi-Fi network, DHCP automatically assigns it an IP address like `192.168.1.105`, subnet mask `255.255.255.0`, gateway `192.168.1.1`, and DNS server addresses — all without manual configuration.

---

## Q7(b) Mobile IP in Wireless Networks

**Definition:** Mobile IP is a protocol (defined in RFC 5944/6275) that allows a mobile device to **maintain the same permanent IP address** while moving between different networks (changing its point of attachment to the Internet), ensuring ongoing connections (like a VoIP call) aren't disrupted during movement.

### Key Entities

1. **Mobile Node (MN):** The device that moves between networks (e.g., smartphone).
2. **Home Network:** The network where the mobile node's permanent (home) IP address is registered.
3. **Home Agent (HA):** A router on the home network that maintains the current location info of the mobile node and tunnels packets to it when away.
4. **Foreign Network:** The network the mobile node is currently visiting.
5. **Foreign Agent (FA):** A router on the foreign network that assists the mobile node, often providing a temporary address.
6. **Care-of Address (COA):** A temporary IP address assigned to the mobile node in the foreign network, indicating its current location.

### Working — Three Phases

#### Phase 1: Agent Discovery
- Home Agent and Foreign Agent periodically broadcast **Agent Advertisement** messages so mobile nodes can detect whether they are on their home network or a foreign network.

#### Phase 2: Registration
- When the mobile node moves to a foreign network, it obtains a **Care-of Address** (either from the Foreign Agent, or via DHCP for a co-located COA).
- The mobile node sends a **Registration Request** to its Home Agent (often via the Foreign Agent), informing it of the current COA.
- The Home Agent updates its mapping table (Home Address ↔ COA) and sends a **Registration Reply**.

#### Phase 3: Tunneling and Data Transfer
- When a sender sends a packet to the mobile node's **home address**, it first arrives at the Home Agent (as normal IP routing would deliver it there).
- The Home Agent **encapsulates (tunnels)** this packet inside another IP packet addressed to the COA, and forwards it to the Foreign Agent/mobile node — this is called **IP-in-IP tunneling**.
- The Foreign Agent de-tunnels the packet and delivers it to the mobile node.
- For the **reverse direction**, the mobile node can send packets directly to the correspondent host (this is called "triangle routing", a known inefficiency of Mobile IP).

```
Correspondent Host
       |
       | (sends packet to MN's Home Address)
       ▼
   Home Agent ---- Tunnel (IP-in-IP) ----> Foreign Agent ---> Mobile Node
   (Home Network)                          (Foreign Network)
```

### Example
A user on a video call walks from their office Wi-Fi (home network) to a coffee shop Wi-Fi (foreign network) without the call dropping — Mobile IP ensures packets addressed to the user's home IP are tunneled to their new location transparently.

### Key Issues
- **Triangle Routing:** Inefficient path (sender → Home Agent → Mobile Node) instead of a direct path — Mobile IPv6 introduces **route optimization** to address this.
- **Security:** Registration messages must be authenticated to prevent malicious redirection of traffic.

---

# Q8

## Q8(a) Distance Vector Routing and Link State Routing

### Distance Vector Routing (DVR)

**Definition:** Distance Vector Routing is a decentralized routing algorithm where each router maintains a table (vector) of the **distance (cost/number of hops) to every other router** in the network, and periodically shares this table with its **directly connected neighbors only**, based on the principle that "I know the distance to X via my neighbor."

#### Algorithm (Bellman-Ford based)
1. Each router initializes its table with distance 0 to itself, ∞ (infinity) to all others, except direct neighbors (known link cost).
2. Each router periodically sends its **entire distance vector** to all directly connected neighbors.
3. Upon receiving a neighbor's vector, the router updates its own table using the **Bellman-Ford equation**:
   ```
   Dx(y) = min{ c(x,v) + Dv(y) }  for each neighbor v
   ```
   where `Dx(y)` = estimated distance from x to y, `c(x,v)` = cost of link from x to v, `Dv(y)` = neighbor v's distance to y.
4. This process repeats (converges) until no router's table changes — known as **"routing by rumor"**.

#### Example
```
   Router A --- (cost 1) --- Router B --- (cost 1) --- Router C

Initially: A's table: A=0, B=1, C=∞
After receiving B's vector (B=0, A=1, C=1):
A updates: C = min(∞, cost(A,B) + B's distance to C) = min(∞, 1+1) = 2
A's table: A=0, B=1, C=2
```

#### Problems
- **Count-to-Infinity Problem:** When a link fails, routers can take a long time to converge, with distance estimates slowly incrementing toward infinity through repeated bad updates between neighbors.
- *Example protocols:* RIP (Routing Information Protocol).

### Link State Routing (LSR)

**Definition:** Link State Routing is a routing algorithm where each router builds a **complete map (topology) of the entire network** by flooding information about its directly connected links (link states) to **all routers** in the network, then independently computes shortest paths using **Dijkstra's algorithm**.

#### Algorithm Steps
1. **Discover neighbors:** Each router discovers its directly connected neighbors and the cost of the link to each.
2. **Create Link State Packet (LSP):** Each router creates an LSP containing its ID, list of neighbors, and link costs.
3. **Flood LSPs:** Each router floods its LSP to **all other routers** in the network (via reliable flooding — every router forwards LSPs it receives to all its neighbors except the one it came from).
4. **Build topology database:** Every router now has a complete, identical map of the network topology.
5. **Compute shortest paths:** Each router independently runs **Dijkstra's Shortest Path Algorithm** on this topology map to determine the best route to every destination.

```
Router A floods: "I am A, connected to B (cost 1) and C (cost 4)"
Router B floods: "I am B, connected to A (cost 1) and C (cost 2)"
Router C floods: "I am C, connected to A (cost 4) and B (cost 2)"

Every router now knows the full topology and runs Dijkstra independently.
```

- *Example protocols:* OSPF (Open Shortest Path First), IS-IS.

### Comparison Table

| Feature | Distance Vector | Link State |
|---|---|---|
| Information shared | Distance vector (to all known destinations) | Link state (only own links) |
| Shared with | Neighbors only | All routers (flooding) |
| Algorithm used | Bellman-Ford | Dijkstra |
| Convergence speed | Slow (count-to-infinity issue) | Fast |
| Topology knowledge | Partial (no global view) | Complete (global view) |
| Resource usage | Lower memory, less CPU | Higher memory & CPU (full topology stored) |
| Example protocol | RIP | OSPF, IS-IS |

---

## Q8(b) RIP, OSPF, and BGP Routing Protocols

### RIP (Routing Information Protocol)

**Definition:** RIP is a simple **Distance Vector** routing protocol used within an autonomous system (interior gateway protocol), which uses **hop count** as its metric, with a maximum allowed value of **15 hops** (16 = unreachable).

#### Key Functions/Characteristics
- Broadcasts/multicasts (RIPv2 uses multicast 224.0.0.9) its entire routing table to neighbors every **30 seconds**.
- Uses **hop count** as the only metric — does not consider bandwidth or link reliability.
- Maximum network diameter: **15 hops** (limits its use to small networks).
- Employs **split horizon** and **route poisoning** to mitigate the count-to-infinity problem.
- *Example use:* Small enterprise networks with simple, flat topologies.

### OSPF (Open Shortest Path First)

**Definition:** OSPF is a **Link State** interior gateway protocol that uses **Dijkstra's algorithm** to compute the shortest path based on **cost** (typically derived from bandwidth), and supports hierarchical network design through the use of **Areas**.

#### Key Functions/Characteristics
- Each router maintains a complete topology database (Link State Database) of its **area**.
- Supports **hierarchical routing** via Areas — Area 0 (backbone area) connects to other areas, reducing routing overhead and improving scalability for large networks.
- Converges much faster than RIP after topology changes (sends updates only when changes occur — "triggered updates", plus periodic refresh).
- Supports **load balancing** across multiple equal-cost paths, VLSM, and authentication.
- *Example use:* Large enterprise networks, ISP internal networks.

### BGP (Border Gateway Protocol)

**Definition:** BGP is the **path vector** routing protocol used to exchange routing information **between different Autonomous Systems (AS)** on the Internet — it is the protocol that makes the global Internet's routing possible, often called the "protocol of the Internet."

#### Key Functions/Characteristics
- Classified as an **Exterior Gateway Protocol (EGP)**, unlike RIP/OSPF which are Interior Gateway Protocols (IGPs).
- Uses **path vector** routing: each route advertisement includes the entire **list of AS numbers (AS-PATH)** the route has traversed, helping prevent routing loops.
- Routing decisions are based on **policies** (business agreements, AS-PATH length, etc.) rather than purely on metric/cost — making BGP highly configurable for traffic engineering.
- Runs over **TCP (port 179)** for reliable transport between BGP peers.
- Two types: **eBGP** (between different ASes) and **iBGP** (within the same AS, to propagate external routes internally).
- *Example use:* ISPs exchanging routing information with each other to determine paths across the global Internet.

### Comparison Table

| Feature | RIP | OSPF | BGP |
|---|---|---|---|
| Type | Interior Gateway Protocol (IGP) | Interior Gateway Protocol (IGP) | Exterior Gateway Protocol (EGP) |
| Algorithm | Distance Vector (Bellman-Ford) | Link State (Dijkstra) | Path Vector |
| Metric | Hop count (max 15) | Cost (based on bandwidth) | Policy-based (AS-PATH, etc.) |
| Convergence | Slow | Fast | Slow (but stable for large scale) |
| Scope | Small networks | Large enterprise networks | Internet-wide (between ASes) |
| Transport | UDP (port 520) | Runs directly over IP (protocol 89) | TCP (port 179) |

---

# Q9

## Q9(a) TCP and UDP Protocols with Diagrams

### TCP (Transmission Control Protocol)

**Definition:** TCP is a **connection-oriented**, reliable transport layer protocol that provides ordered, error-checked delivery of a stream of bytes between applications, using mechanisms like the three-way handshake, sequence numbers, acknowledgments, flow control, and congestion control.

#### TCP Header Format (20 bytes minimum)

```
 0                   16                  31
+-------------------+-------------------+
|   Source Port     |  Destination Port |
+-------------------+-------------------+
|         Sequence Number                |
+-----------------------------------------+
|        Acknowledgment Number            |
+----+----+--------------------+----------+
|HLEN|Resvd| Flags(URG,ACK,PSH, |  Window  |
|    |     | RST,SYN,FIN)       |          |
+----+----+--------------------+----------+
|    Checksum        | Urgent Pointer      |
+-------------------+-------------------+
|        Options (if any) + Padding       |
+-----------------------------------------+
|                Data                      |
+-----------------------------------------+
```

#### Three-Way Handshake (Connection Establishment)

```
Client                                Server
   |------ SYN (seq=x) -------------->|
   |<--- SYN-ACK (seq=y, ack=x+1) ----|
   |------ ACK (ack=y+1) ------------>|
   |        Connection Established     |
```

#### Connection Termination (Four-way)

```
Client                                Server
   |------ FIN ----------------------->|
   |<----- ACK -------------------------|
   |<----- FIN ---------------------------|
   |------ ACK -------------------------->|
```

#### Key Characteristics
- **Connection-oriented:** Requires handshake before data transfer.
- **Reliable:** Uses sequence numbers + acknowledgments + retransmission of lost segments.
- **Flow control:** Uses sliding window mechanism (receiver advertises window size).
- **Congestion control:** Adjusts sending rate based on network congestion (covered in Q10).
- *Example use:* Web browsing (HTTP/HTTPS), email (SMTP), file transfer (FTP) — applications requiring guaranteed delivery.

### UDP (User Datagram Protocol)

**Definition:** UDP is a **connectionless**, unreliable (best-effort) transport layer protocol that sends independent packets (datagrams) without establishing a connection, without guaranteeing delivery, ordering, or duplicate protection — prioritizing speed and low overhead over reliability.

#### UDP Header Format (8 bytes — fixed, much simpler than TCP)

```
 0                   16                  31
+-------------------+-------------------+
|   Source Port     |  Destination Port |
+-------------------+-------------------+
|   Total Length    |     Checksum      |
+-------------------+-------------------+
|                Data                    |
+-----------------------------------------+
```

#### Key Characteristics
- **Connectionless:** No handshake — sender simply transmits datagrams.
- **Unreliable:** No acknowledgment, no retransmission — lost packets are simply lost.
- **No flow/congestion control:** Sender can transmit as fast as it wants (application must handle this if needed).
- **Low overhead:** Only 8-byte header vs TCP's minimum 20 bytes — faster processing.
- *Example use:* DNS queries, video/audio streaming, VoIP, online gaming — applications where speed matters more than guaranteed delivery (a few lost packets in a video call are tolerable, but a delayed retransmission would cause more disruption).

### Comparison Table

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented (3-way handshake) | Connectionless |
| Reliability | Reliable (ACK, retransmission) | Unreliable (best-effort) |
| Ordering | Guaranteed in-order delivery | No ordering guarantee |
| Header size | 20 bytes (minimum) | 8 bytes (fixed) |
| Speed | Slower (overhead of reliability mechanisms) | Faster |
| Flow/Congestion control | Yes | No |
| Examples | HTTP, FTP, SMTP, SSH | DNS, DHCP, streaming, VoIP |

---

## Q9(b) Comparison of TCP, UDP, and SCTP

### SCTP (Stream Control Transmission Protocol)

**Definition:** SCTP is a transport layer protocol that combines features of both TCP and UDP — it is **connection-oriented and reliable like TCP**, but introduces **multi-streaming** (multiple independent streams within one connection) and **multi-homing** (using multiple IP addresses/network paths for a single connection), originally designed for telephony signaling (SS7 over IP).

#### Key Features of SCTP

1. **Multi-streaming:** A single SCTP connection (called an "association") can carry multiple independent streams of data. A lost segment in one stream doesn't block delivery of data in other streams — solving TCP's **"head-of-line blocking"** problem.

2. **Multi-homing:** An SCTP endpoint can be reached via multiple IP addresses (e.g., on different network interfaces). If the primary path fails, SCTP automatically fails over to a backup path — improving fault tolerance.

3. **Message-oriented:** Unlike TCP's byte-stream model, SCTP preserves message boundaries (similar to UDP), useful for applications that naturally work with discrete messages.

4. **Four-way handshake (with cookie):** Uses an `INIT`/`INIT-ACK`/`COOKIE-ECHO`/`COOKIE-ACK` exchange, which is more resistant to SYN-flood-type DoS attacks than TCP's 3-way handshake.

```
SCTP Association (single connection, multiple streams):

  Association
  +-------------------------------+
  |  Stream 1: [msg1][msg2][msg3]  |
  |  Stream 2: [msg1][msg2]        |
  |  Stream 3: [msg1][msg2][msg3]  |
  +-------------------------------+
  (loss in Stream 2 doesn't block Stream 1 or 3)
```

### Three-Way Comparison Table

| Feature | TCP | UDP | SCTP |
|---|---|---|---|
| Connection type | Connection-oriented | Connectionless | Connection-oriented (association) |
| Reliability | Reliable | Unreliable | Reliable |
| Data model | Byte stream | Datagram (message) | Message-oriented (multiple streams) |
| Ordering | Strict in-order (single stream) | None | Ordered within each stream (independent streams) |
| Multi-streaming | No | No | Yes |
| Multi-homing | No | No | Yes |
| Head-of-line blocking | Yes | N/A | No (per-stream delivery) |
| Handshake | 3-way | None | 4-way (with cookie, DoS-resistant) |
| Typical use | Web, email, file transfer | DNS, streaming, VoIP | VoIP signaling (SS7/SIGTRAN), WebRTC data channels |

### Example Scenario
A web browser loading a page with **HTTP/2 or HTTP/3** benefits conceptually from SCTP-like multi-streaming (multiple resources fetched in parallel without one blocking another) — this is part of why **QUIC** (used in HTTP/3, built over UDP) was designed to bring multi-streaming benefits without TCP's head-of-line blocking, addressing the same problem SCTP was designed to solve.

---

# Q10

## Q10(a) Congestion Control Mechanisms in TCP

**Definition:** Congestion control refers to mechanisms used by TCP to prevent the sender from overwhelming the network (routers/links) with more data than it can handle, by dynamically adjusting the sender's transmission rate based on perceived network conditions — distinct from flow control, which prevents overwhelming the *receiver*.

### Key Concept: Congestion Window (cwnd)
- TCP maintains a **congestion window (cwnd)**, which limits how much data can be sent before receiving an ACK.
- The actual amount of data sent = `min(cwnd, receiver's advertised window)`.

### TCP Congestion Control Algorithms

#### 1. Slow Start
- TCP begins cautiously: `cwnd` starts at **1 MSS** (Maximum Segment Size).
- For each ACK received, `cwnd` is **increased by 1 MSS** — this results in **exponential growth** (doubling every round-trip time, RTT) since each successfully ACKed segment allows one more segment to be sent.
- This continues until `cwnd` reaches a threshold called **ssthresh (slow start threshold)**.

```
RTT 1: cwnd = 1   → send 1 segment
RTT 2: cwnd = 2   → send 2 segments
RTT 3: cwnd = 4   → send 4 segments
RTT 4: cwnd = 8   → send 8 segments  (exponential growth)
```

#### 2. Congestion Avoidance
- Once `cwnd ≥ ssthresh`, TCP switches to **Congestion Avoidance**: `cwnd` grows **linearly** — increased by approximately 1 MSS per RTT (additive increase), rather than doubling.
- This cautious linear growth ("Additive Increase") probes for additional available bandwidth without risking severe congestion.

```
After ssthresh reached:
RTT n:   cwnd = 8
RTT n+1: cwnd = 9
RTT n+2: cwnd = 10   (linear growth, +1 MSS per RTT)
```

#### 3. Fast Retransmit
- Normally, TCP waits for a timeout to detect packet loss. **Fast Retransmit** speeds this up: if the sender receives **3 duplicate ACKs** (i.e., 4 ACKs total for the same segment), it assumes the next segment was lost and **retransmits immediately**, without waiting for the timer to expire.

#### 4. Fast Recovery
- After Fast Retransmit, instead of dropping `cwnd` all the way back to 1 (as in slow start), TCP sets `cwnd = ssthresh` (halved) and proceeds in congestion avoidance mode — avoiding the severe penalty of a full slow start restart.

### Overall Behavior — AIMD (Additive Increase, Multiplicative Decrease)

```
cwnd
  |
  |        /\        /\
  |      /    \    /    \      ← sawtooth pattern
  |    /        \/        \
  |  /
  |/_________________________________ time
   Slow Start → Cong. Avoidance → Loss detected (cwnd halved) → repeat
```

- **On timeout (severe congestion):** `ssthresh = cwnd/2`, `cwnd` reset to **1 MSS**, restart Slow Start — this is the **Multiplicative Decrease**.
- **On 3 duplicate ACKs (mild congestion):** `ssthresh = cwnd/2`, `cwnd = ssthresh`, enter Congestion Avoidance directly (Fast Recovery) — avoids restarting from scratch.

### Summary Table

| Phase | Trigger | cwnd Growth |
|---|---|---|
| Slow Start | Connection start / after timeout | Exponential (doubles per RTT) |
| Congestion Avoidance | cwnd ≥ ssthresh | Linear (+1 MSS per RTT) |
| Fast Retransmit | 3 duplicate ACKs | Immediate retransmission of lost segment |
| Fast Recovery | After fast retransmit | cwnd = ssthresh (halved), continue in cong. avoidance |
| Timeout | No ACK received in time | cwnd reset to 1, ssthresh halved, restart slow start |

---

## Q10(b) Leaky Bucket and Token Bucket Algorithms

**Definition:** These are **traffic shaping** algorithms used to control the rate at which data is sent into a network, smoothing out bursty traffic to conform to agreed-upon rate limits (a key part of QoS — Quality of Service — mechanisms).

### Leaky Bucket Algorithm

**Definition:** The Leaky Bucket algorithm regulates the output rate of data to a **constant rate**, regardless of how bursty the input traffic is — analogous to a bucket with a small hole at the bottom: water (data) can be poured in at any rate, but it leaks out at a fixed constant rate, and excess water (data) that doesn't fit overflows (is dropped).

#### Working
1. Incoming packets/bits are placed into a finite-size buffer (the "bucket").
2. The bucket "leaks" — i.e., transmits data onto the network — at a **fixed, constant rate**, irrespective of the burstiness of the input.
3. If the bucket is full when new data arrives, that data is **discarded** (or marked, depending on implementation).

```
   Bursty Input                Output
   (variable rate)             (constant rate)
        ↓                            ↓
   +----------+                +-----------+
   |  BUCKET  | ---leak rate--> |  Network  |
   |  (buffer)|     (fixed)     |           |
   +----------+                +-----------+
        ↑
   overflow (dropped) if bucket full
```

#### Characteristics
- Output rate is always **constant** — completely smooths bursts.
- Simple to implement (a counter/queue + fixed-rate transmitter).
- **Drawback:** Cannot send bursts even if the network is idle and could handle them — strict and inflexible.

### Token Bucket Algorithm

**Definition:** The Token Bucket algorithm allows for some burstiness in transmission by accumulating **"tokens"** in a bucket at a fixed rate (each token represents permission to send a certain amount of data); a host can transmit data only if it has enough tokens, and unused tokens accumulate (up to bucket capacity) allowing the host to send a **burst** when tokens have built up.

#### Working
1. Tokens are added to the bucket at a **fixed rate `r`** (e.g., 1 token per unit time), up to a maximum bucket capacity `b` (tokens beyond capacity are discarded — bucket doesn't overflow with tokens, just stops filling).
2. To transmit a packet of size `n`, the host must remove `n` tokens from the bucket.
3. If sufficient tokens are available, the packet is sent immediately (even in a burst, as long as tokens are available).
4. If insufficient tokens, the packet must wait (or is dropped, depending on policy) until enough tokens accumulate.

```
   Tokens generated at rate 'r' ----> [TOKEN BUCKET] (capacity 'b')
                                            |
                                            v
   Packet arrives -----> needs 'n' tokens -----> if available, SEND immediately
                                              -----> if not, WAIT or DROP
```

#### Characteristics
- Allows **bursty transmission** up to the bucket's accumulated token capacity — more flexible than leaky bucket.
- Long-term average rate is still bounded by the token generation rate `r`, but short-term bursts up to `b` are permitted.
- More suitable for applications with variable but bounded burstiness (e.g., compressed video).

### Comparison Table

| Feature | Leaky Bucket | Token Bucket |
|---|---|---|
| Output rate | Always constant | Variable (allows bursts) |
| Burst handling | Not allowed — strictly smooths | Allowed, up to bucket capacity |
| Idle period benefit | None (unused capacity wasted) | Tokens accumulate during idle periods, enabling future bursts |
| Flexibility | Less flexible/rigid | More flexible |
| Use case | Strict rate enforcement | Applications needing burst tolerance (e.g., video streaming) |

### Numerical Example (Token Bucket)
- Token generation rate `r = 2 Mbps`, bucket capacity `b = 8 Mb` (megabits).
- If the host is idle for 4 seconds, tokens accumulate: `4 × 2 = 8 Mb` (bucket full, capped at capacity).
- The host can now burst at a rate **higher than 2 Mbps** (limited by actual link bandwidth) until the 8 Mb of tokens is exhausted, after which it's limited back to 2 Mbps (the token generation rate).

---

# Q11

## Q11(a) Working of DNS and SMTP Protocols

### DNS (Domain Name System)

**Definition:** DNS is an application layer protocol that translates human-readable **domain names** (e.g., `www.example.com`) into machine-readable **IP addresses** (e.g., `93.184.216.34`), functioning as a distributed, hierarchical database system across the Internet.

#### DNS Hierarchy (Namespace Structure)

```
                         "." (Root)
                          |
        +-----------------+------------------+
        |                 |                  |
      .com              .org               .in   (Top-Level Domains - TLD)
        |
    example.com   (Second-level domain)
        |
   www.example.com  (Subdomain/Host)
```

- **Root servers:** Know the addresses of TLD servers (.com, .org, .net, country codes like .in).
- **TLD servers:** Know the addresses of authoritative servers for domains under that TLD.
- **Authoritative servers:** Hold the actual DNS records (A, AAAA, MX, CNAME, etc.) for a specific domain.

#### DNS Resolution Process (Recursive Query Example)

```
User's PC                Local DNS              Root        TLD (.com)    Authoritative
                          Resolver               Server      Server        Server (example.com)
   |                          |                    |             |               |
   |--- query www.example.com-->|                  |             |               |
   |                          |---query----------->|             |               |
   |                          |<-- refer to .com TLD ------------|             |
   |                          |---query (www.example.com)------------------->|  |
   |                          |<-- refer to authoritative server -------------|  |
   |                          |---query------------------------------------------>|
   |                          |<--------- IP address (A record) -------------------|
   |<--- IP address ----------|                  |             |               |
```

#### Types of DNS Records

| Record Type | Purpose |
|---|---|
| A | Maps hostname to IPv4 address |
| AAAA | Maps hostname to IPv6 address |
| MX | Specifies mail server for the domain |
| CNAME | Alias — maps one domain name to another |
| NS | Specifies authoritative name servers for the domain |

*Example:* Typing `www.google.com` in a browser triggers a DNS lookup that returns Google's server IP address, allowing your browser to establish a TCP connection to that IP for HTTP/HTTPS.

### SMTP (Simple Mail Transfer Protocol)

**Definition:** SMTP is an application layer protocol used to **send (push) email** from a client to a mail server, or relay email between mail servers — it operates over **TCP port 25** (or 587 for submission with authentication).

#### SMTP Working — Email Sending Process

1. The sender's mail client (or Mail User Agent, MUA) connects to its outgoing mail server (Mail Transfer Agent, MTA) using SMTP.
2. SMTP commands establish the connection and transfer the message:
   - `HELO`/`EHLO` — Client greets the server, identifies itself.
   - `MAIL FROM:<sender@example.com>` — Specifies the sender's address.
   - `RCPT TO:<receiver@domain.com>` — Specifies the recipient's address.
   - `DATA` — Begins the message body, terminated by a line containing only a period (`.`).
   - `QUIT` — Terminates the session.
3. The sending MTA uses **DNS MX records** to find the recipient domain's mail server, and relays the message there (also via SMTP).
4. The receiving mail server stores the message in the recipient's mailbox.

```
Sender's MUA --SMTP--> Sender's MTA --SMTP (relay)--> Receiver's MTA --(stores in mailbox)
                                                              |
                                                   Recipient retrieves mail via
                                                   POP3 or IMAP (not SMTP!)
```

#### Important Note
SMTP is a **"push" protocol** — used only for sending mail between servers and from client to server. **Retrieving** mail from the mailbox to the recipient's device uses different protocols: **POP3** (downloads and typically deletes from server) or **IMAP** (synchronizes mail, keeping it on the server).

#### Example
When you send an email from Gmail to a Yahoo address:
1. Your Gmail client sends the message to Gmail's SMTP server.
2. Gmail's SMTP server looks up Yahoo's MX record via DNS.
3. Gmail's server relays the message via SMTP to Yahoo's mail server.
4. The recipient retrieves it from Yahoo's server using IMAP/POP3 (or webmail).

---

## Q11(b) Cryptography and Digital Signatures in Network Security

### Cryptography

**Definition:** Cryptography is the practice of securing communication by transforming readable data (**plaintext**) into an unreadable format (**ciphertext**) using mathematical algorithms and keys, such that only authorized parties with the correct key can convert it back (decrypt) to plaintext.

#### Types of Cryptography

##### 1. Symmetric Key Cryptography
- The **same secret key** is used for both encryption and decryption.
- **Characteristics:** Fast and computationally efficient, but key distribution is a challenge — both parties must securely share the same key beforehand.
- *Examples:* AES (Advanced Encryption Standard), DES, 3DES.

```
Plaintext --[Encrypt with Key K]--> Ciphertext --[Decrypt with Key K]--> Plaintext
                  (Same key K used by both sender and receiver)
```

##### 2. Asymmetric Key Cryptography (Public Key Cryptography)
- Uses a **pair of mathematically related keys**: a **public key** (shared openly) and a **private key** (kept secret).
- Data encrypted with the public key can only be decrypted with the corresponding private key (and vice versa for signing).
- **Characteristics:** Solves the key distribution problem, but computationally slower than symmetric cryptography.
- *Examples:* RSA, ECC (Elliptic Curve Cryptography), Diffie-Hellman.

```
Sender encrypts with Receiver's PUBLIC key
                |
                v
Receiver decrypts with their own PRIVATE key
(only the receiver, who holds the private key, can decrypt)
```

#### Why Importance in Network Security?
- Ensures **Confidentiality** — only intended recipients can read the data (used in HTTPS/TLS, VPNs).
- Forms the basis of **secure key exchange** (e.g., Diffie-Hellman key exchange establishes a shared symmetric key over an insecure channel).

### Digital Signatures

**Definition:** A digital signature is a cryptographic technique using **asymmetric cryptography** that allows a sender to "sign" a message using their **private key**, such that anyone can verify the signature using the sender's **public key**, thereby providing **authentication, non-repudiation, and integrity**.

#### How Digital Signatures Work

1. **Sender side:**
   - Compute a **hash** (e.g., SHA-256) of the message — producing a fixed-size digest.
   - Encrypt this hash using the sender's **private key** — this encrypted hash is the **digital signature**.
   - Send the original message + the digital signature.

2. **Receiver side:**
   - Decrypt the received signature using the sender's **public key**, obtaining the original hash.
   - Independently compute the hash of the received message.
   - **Compare** the two hashes — if they match, the signature is valid.

```
SENDER:
Message --[Hash function]--> Hash --[Encrypt with Sender's PRIVATE key]--> Digital Signature
   |_______________________________________________________________________|
                          Send: Message + Digital Signature

RECEIVER:
Received Message --[Hash function]--> Hash_A
Received Signature --[Decrypt with Sender's PUBLIC key]--> Hash_B

If Hash_A == Hash_B  →  Signature VALID (message authentic & unaltered)
If Hash_A != Hash_B  →  Signature INVALID (message tampered or sender forged)
```

#### Properties Provided

| Property | Explanation |
|---|---|
| **Authentication** | Verifies the message genuinely came from the claimed sender (only they hold the private key) |
| **Integrity** | Any modification to the message changes its hash, causing signature verification to fail |
| **Non-repudiation** | The sender cannot later deny sending the message, since only their private key could have produced a valid signature |

#### Example: Digital Certificates and HTTPS
When you visit an HTTPS website, the server presents a **digital certificate** (issued by a Certificate Authority, CA) containing the server's public key, **digitally signed by the CA's private key**. Your browser verifies this signature using the CA's well-known public key (pre-installed in browsers/OS) to confirm the website's identity before establishing a secure connection — this is the foundation of trust on the modern web.

---

# Q12

## Q12(a) Firewall Concepts and Their Significance in Network Security

**Definition:** A firewall is a network security device/software that monitors and controls **incoming and outgoing network traffic** based on a defined set of **security rules**, acting as a barrier between a trusted internal network and untrusted external networks (e.g., the Internet).

### Types of Firewalls

#### 1. Packet Filtering Firewall
- Operates at the **Network Layer** (Layer 3) and Transport Layer (Layer 4).
- Examines each packet's header (source/destination IP, port numbers, protocol) against a set of rules (Access Control List - ACL), and allows or drops the packet — **stateless** (each packet examined independently, without context of the connection).
- **Characteristics:** Fast (low overhead), but limited security — cannot detect attacks that span multiple packets, vulnerable to IP spoofing.
- *Example rule:* "Block all incoming traffic on port 23 (Telnet)."

#### 2. Stateful Inspection Firewall
- Maintains a **state table** tracking active connections (e.g., TCP connection state: SYN sent, established, etc.).
- Makes filtering decisions based on the **context of the connection**, not just individual packet headers — e.g., only allows incoming packets that are part of an established outgoing connection.
- **Characteristics:** More secure than packet filtering, moderate performance overhead.
- *Example:* If an internal host initiates a connection to an external web server (outgoing SYN), the firewall allows the corresponding return traffic (SYN-ACK, data) but blocks unsolicited incoming connections.

#### 3. Application Layer (Proxy) Firewall
- Operates at the **Application Layer** (Layer 7), acting as an intermediary (**proxy**) between internal clients and external servers.
- Can inspect the actual **content/payload** of traffic (e.g., HTTP requests, email content), enabling filtering based on application-specific data (URLs, file types, commands).
- **Characteristics:** Highest security (deep packet inspection), but highest performance overhead/latency; can be application-specific (e.g., a web proxy only understands HTTP).
- *Example:* A corporate web proxy that blocks access to specific websites or scans downloaded files for malware before delivering them to the user.

#### 4. Next-Generation Firewall (NGFW)
- Combines traditional firewall capabilities with additional features: **Intrusion Prevention System (IPS)**, deep packet inspection, application awareness, and SSL/TLS inspection.

### Firewall Architecture / Placement

```
         Internet (Untrusted)
              |
         +---------+
         | Firewall |
         +---------+
              |
       Internal Network (Trusted)
       (e.g., DMZ for public-facing servers
        sits between firewall layers)
```

A common architecture uses a **DMZ (Demilitarized Zone)** — a buffer subnetwork containing public-facing servers (web, email) placed between two firewalls: one facing the Internet, one facing the internal LAN, so even if the DMZ is compromised, the internal network remains protected.

### Significance in Network Security

1. **Access Control:** Enforces organizational security policy by permitting/denying traffic based on rules (IP addresses, ports, protocols).
2. **Traffic Monitoring & Logging:** Records traffic for auditing and incident investigation.
3. **Network Segmentation:** Isolates sensitive internal segments (e.g., finance department) from general traffic.
4. **Protection Against Common Attacks:** Can block port scans, certain DoS patterns, unauthorized access attempts.
5. **NAT Functionality:** Often combined with NAT to hide internal IP addressing scheme from external networks.

### Comparison Table

| Type | OSI Layer | State Awareness | Inspection Depth | Performance |
|---|---|---|---|---|
| Packet Filtering | Network/Transport | Stateless | Header only | Fast |
| Stateful Inspection | Network/Transport | Stateful | Header + connection state | Moderate |
| Application Proxy | Application | Stateful | Full payload | Slower |
| NGFW | All layers | Stateful | Full payload + threat intel | Moderate-Fast (hardware accelerated) |

---

## Q12(b) TCP Socket and UDP Socket Programming

### Socket Programming — Overview

**Definition:** A **socket** is an endpoint for network communication, identified by a combination of an **IP address and a port number**, providing an API (Application Programming Interface) through which application programs can send and receive data over a network using TCP or UDP.

### TCP Socket Programming (Connection-Oriented)

**Server-side steps:**
1. `socket()` — Create a socket.
2. `bind()` — Bind the socket to a specific IP address and port.
3. `listen()` — Put the socket into listening mode, ready to accept connections.
4. `accept()` — Accept an incoming client connection, returns a new socket for that specific connection.
5. `recv()`/`send()` — Receive/send data over the connection.
6. `close()` — Close the connection.

**Client-side steps:**
1. `socket()` — Create a socket.
2. `connect()` — Connect to the server's IP address and port (this initiates TCP's 3-way handshake).
3. `send()`/`recv()` — Send/receive data.
4. `close()` — Close the connection.

```
        TCP SERVER                          TCP CLIENT
   socket()                              socket()
   bind(IP, PORT)
   listen()
   accept() ◄------------------------- connect(IP, PORT)
        |                                    |
   (3-way handshake happens automatically)
        |                                    |
   recv() <----------------------------- send()
   send() -----------------------------> recv()
        |                                    |
   close()                              close()
```

#### Example Code Snippet (Python — TCP Server)
```python
import socket

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  # SOCK_STREAM = TCP
server.bind(('0.0.0.0', 8080))
server.listen(5)
print("Server listening on port 8080...")

conn, addr = server.accept()
print(f"Connected by {addr}")
data = conn.recv(1024)
print(f"Received: {data.decode()}")
conn.send(b"Hello from server!")
conn.close()
```

#### Example Code Snippet (Python — TCP Client)
```python
import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(('127.0.0.1', 8080))
client.send(b"Hello from client!")
data = client.recv(1024)
print(f"Server replied: {data.decode()}")
client.close()
```

### UDP Socket Programming (Connectionless)

**Server-side steps:**
1. `socket()` — Create a socket (with `SOCK_DGRAM` type).
2. `bind()` — Bind to an IP address and port.
3. `recvfrom()`/`sendto()` — Receive/send datagrams (no connection needed; each datagram carries its own address info).
4. `close()` — Close the socket.

**Client-side steps:**
1. `socket()` — Create a socket.
2. `sendto()`/`recvfrom()` — Send/receive datagrams directly to/from a specified address (no `connect()` required).
3. `close()` — Close the socket.

```
        UDP SERVER                          UDP CLIENT
   socket()                              socket()
   bind(IP, PORT)
        |                                    |
   recvfrom() <------------------------ sendto(IP, PORT, data)
   sendto()   ------------------------> recvfrom()
        |                                    |
   close()                              close()

   (No handshake — each datagram is independent)
```

#### Example Code Snippet (Python — UDP Server)
```python
import socket

server = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  # SOCK_DGRAM = UDP
server.bind(('0.0.0.0', 9090))
print("UDP server listening on port 9090...")

data, addr = server.recvfrom(1024)
print(f"Received from {addr}: {data.decode()}")
server.sendto(b"Hello from UDP server!", addr)
server.close()
```

#### Example Code Snippet (Python — UDP Client)
```python
import socket

client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
client.sendto(b"Hello from UDP client!", ('127.0.0.1', 9090))
data, addr = client.recvfrom(1024)
print(f"Server replied: {data.decode()}")
client.close()
```

### Comparison Table

| Aspect | TCP Socket | UDP Socket |
|---|---|---|
| Socket type constant | `SOCK_STREAM` | `SOCK_DGRAM` |
| Connection setup | `connect()` / `accept()` required (handshake) | No connection setup |
| Data transfer functions | `send()` / `recv()` | `sendto()` / `recvfrom()` (address needed each time) |
| Reliability | Guaranteed delivery, ordering | No guarantees |
| Use case example | File transfer server, chat application (reliable messages) | DNS lookup tool, simple ping-like utility |

---

# End of Document
