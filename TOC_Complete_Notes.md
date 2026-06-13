# THEORY OF COMPUTATION (TOC) — Complete Semester Notes
## R23 Syllabus | All 5 Modules with Definitions, Proofs, and Solved Numericals

---

# MODULE 1: Finite Automata & Minimization

## B. THEORY (Short Answer)

### 1. Formal Definitions (5-tuples)

**DFA (Deterministic Finite Automaton)**
M = (Q, Σ, δ, q0, F)
- Q = finite set of states
- Σ = finite input alphabet
- δ: Q × Σ → Q (single next state — deterministic, total function)
- q0 ∈ Q = start state
- F ⊆ Q = set of final/accepting states

**NFA (Non-deterministic Finite Automaton)**
M = (Q, Σ, δ, q0, F)
- Same as DFA except: δ: Q × Σ → 2^Q (set of possible next states)
- Can have zero, one, or many transitions for a given (state, symbol)

**ε-NFA (NFA with epsilon transitions)**
M = (Q, Σ, δ, q0, F)
- δ: Q × (Σ ∪ {ε}) → 2^Q
- Allows state changes without consuming input

---

### 2. Equivalence of NFA and DFA (Theorem)

**Statement:** For every NFA N, there exists a DFA D such that L(N) = L(D).

**Proof (by Subset Construction):**
Let N = (Qn, Σ, δn, q0, Fn).
Construct D = (Qd, Σ, δd, q0', Fd) where:
- Qd = 2^Qn (all subsets of Qn — power set)
- q0' = {q0}
- δd(S, a) = ⋃_{p∈S} δn(p, a) for all S ∈ Qd, a ∈ Σ
- Fd = {S ∈ Qd | S ∩ Fn ≠ ∅}

**Proof by induction on |w|:**
- Base case: |w| = 0 (w = ε). δd*(q0', ε) = {q0} = δn*(q0, ε). True.
- Inductive step: Assume δd*(q0', x) = δn*(q0, x) for all |x| < n.
  For w = xa, δd*(q0', xa) = δd(δd*(q0',x), a) = ⋃_{p∈δn*(q0,x)} δn(p,a) = δn*(q0, xa).

Hence by induction, for every string w, δd*(q0', w) = δn*(q0, w).
w is accepted by N iff δn*(q0,w) ∩ Fn ≠ ∅ iff δd*(q0',w) ∈ Fd iff w accepted by D.
Therefore L(N) = L(D). ∎

(Note: Many states of D may be unreachable; only reachable subsets need be constructed in practice.)

---

### 3. Equivalence of NFA with and without ε-transitions

**Statement:** For every ε-NFA E, there is an NFA N (without ε) with L(N) = L(E).

**Proof sketch:**
- Compute ECLOSE(q) = set of all states reachable from q using only ε-transitions (including q itself), for every state q.
- Define new transition: δn(q, a) = ECLOSE( ⋃_{p ∈ ECLOSE(q)} δe(p, a) ) for each a ∈ Σ.
- New final states: Fn = { q | ECLOSE(q) ∩ Fe ≠ ∅ }
- Start state remains q0.

Since every ε-path is "absorbed" into the new δn via ECLOSE, and a string is accepted in E iff after consuming all symbols (with ε-moves interleaved anywhere) some final state is reached — which is captured exactly by ECLOSE before/after each real move — L(N) = L(E). ∎

---

### 4. Myhill–Nerode Theorem

**Statement:** A language L over Σ is regular **iff** the number of equivalence classes of the relation ≡_L (called Myhill-Nerode relation) is finite, where:

x ≡_L y  ⟺  for all z ∈ Σ*, xz ∈ L ⟺ yz ∈ L

(x and y are "indistinguishable" — no suffix z can tell them apart with respect to membership in L)

**Application:**
1. **Proving non-regularity**: Show L has infinitely many equivalence classes (infinitely many pairwise distinguishable strings) ⟹ L is not regular.
   Example: L = {0^n1^n} — strings 0^i and 0^j (i≠j) are distinguishable (append 1^i), giving infinitely many classes ⟹ not regular.
2. **DFA Minimization**: The minimum-state DFA for L has exactly one state per equivalence class of ≡_L. This is the theoretical basis of table-filling minimization.

---

### 5. Limitations of Finite State Machines (FSM)

1. **No memory of arbitrary size** — FSMs have only a finite number of states, so they cannot "count" without bound.
2. **Cannot recognize languages requiring matching/counting** — e.g., L = {a^n b^n} (need to remember count of a's, which can be arbitrarily large) — not regular.
3. **Cannot handle nested/recursive structures** — e.g., balanced parentheses, matching tags (needs a stack → PDA).
4. **Cannot recognize palindromes ww^R** over arbitrary length — needs to compare from both ends.
5. **Limited to "local" pattern recognition** — decisions depend only on current state + current symbol, no global context.
6. **Cannot perform arithmetic operations** like multiplication, or recognize {a^p : p prime}.

---

## A. NUMERICAL PROBLEMS

### PROBLEM 1: NFA to DFA — Strings ending in '01'

**NFA:** Σ = {0,1}, accepts strings ending in "01"

States: q0 (start), q1 (seen '0'), q2 (seen '01' — final)

| State | 0 | 1 |
|-------|---|---|
| →q0 | {q0,q1} | {q0} |
| q1 | {q1} | {q2} |
| *q2 | {q1} | {q0} |

**Subset Construction (Conversion to DFA):**

Start: A = {q0}
- δ(A,0) = {q0,q1} = B
- δ(A,1) = {q0} = A

B = {q0,q1}
- δ(B,0) = {q0,q1} = B
- δ(B,1) = {q0,q2} = C  (final, contains q2)

C = {q0,q2}
- δ(C,0) = {q0,q1,q1} = {q0,q1} = B
- δ(C,1) = {q0,q0} = {q0} = A

**Resulting DFA:**

| State | 0 | 1 |
|-------|---|---|
| →A={q0} | B | A |
| B={q0,q1} | B | C |
| *C={q0,q2} | B | A |

3 states total, C is the only final state. Verify: input "001" → A→B→B→C ✓ (ends in 01).

---

### PROBLEM 2: NFA to DFA — Strings ending in '101'

**NFA states**: q0(start) — q1(saw 1) — q2(saw 10) — q3(saw 101, final)

| State | 0 | 1 |
|-------|---|---|
| →q0 | {q0} | {q0,q1} |
| q1 | {q2} | {q1} |
| q2 | {q0} | {q1,q3} |
| *q3 | {q0} | {q0,q1} |

**Subset Construction:**

A = {q0} (start)
- δ(A,0) = {q0} = A
- δ(A,1) = {q0,q1} = B

B = {q0,q1}
- δ(B,0) = {q0,q2} = C
- δ(B,1) = {q0,q1} = B

C = {q0,q2}
- δ(C,0) = {q0} = A
- δ(C,1) = {q0,q1,q1,q3} = {q0,q1,q3} = D (final)

D = {q0,q1,q3}
- δ(D,0) = {q0,q2,q0} = {q0,q2} = C
- δ(D,1) = {q0,q1,q0,q1} = {q0,q1} = B

**Resulting DFA:**

| State | 0 | 1 |
|-------|---|---|
| →A={q0} | A | B |
| B={q0,q1} | C | B |
| C={q0,q2} | A | D |
| *D={q0,q1,q3} | C | B |

4 states. Verify "1101" → A→B→B→C→D ✓ (final, ends in 101).

---

### PROBLEM 3: ε-NFA to DFA (over {a,b,c} with null/ε jumps)

**Given ε-NFA:**

| State | ε | a | b | c |
|-------|---|---|---|---|
| →q0 | {q1} | – | – | – |
| q1 | {q2} | {q1} | – | – |
| q2 | – | – | {q2} | {q3} |
| *q3 | – | – | – | {q3} |

**Step 1: Compute ε-closures**

- ECLOSE(q0) = {q0, q1, q2}  (q0→q1 via ε, q1→q2 via ε)
- ECLOSE(q1) = {q1, q2}
- ECLOSE(q2) = {q2}
- ECLOSE(q3) = {q3}

**Step 2: Subset construction using ECLOSE**

Start state = ECLOSE(q0) = A = {q0,q1,q2}  → contains q3? No, not final.

A = {q0,q1,q2}
- On 'a': moves from q1 (only one with 'a') → {q1}. ECLOSE({q1}) = {q1,q2} = B
- On 'b': moves from q2 → {q2}. ECLOSE({q2}) = {q2} = C
- On 'c': moves from q2 → {q3}. ECLOSE({q3}) = {q3} = D (final, contains q3)

B = {q1,q2}
- On 'a': from q1 → {q1}. ECLOSE = {q1,q2} = B
- On 'b': from q2 → {q2}. ECLOSE = {q2} = C
- On 'c': from q2 → {q3}. ECLOSE = {q3} = D

C = {q2}
- On 'a': ∅ → dead state (or empty/trap), call it E
- On 'b': {q2} → C
- On 'c': {q3} → D

D = {q3} (final)
- On 'a': ∅ → E
- On 'b': ∅ → E
- On 'c': {q3} → D

E = ∅ (trap/dead state)
- On a,b,c: E → E (self loop)

**Final DFA table:**

| State | a | b | c |
|-------|---|---|---|
| →A={q0,q1,q2} | B | C | D |
| B={q1,q2} | B | C | D |
| C={q2} | E | C | D |
| *D={q3} | E | E | D |
| E=∅ | E | E | E |

Language accepted: any string over {a,b,c} containing a 'c' — i.e., (a+b)*c(a+b+c)*.

---

### PROBLEM 4: DFA Minimization (Table-Filling / Myhill-Nerode)

**Given DFA (6 states):** Σ={0,1}

| State | 0 | 1 |
|-------|---|---|
| →A | B | C |
| B | A | D |
| C | E | F |
| D | E | F |
| *E | E | F |
| *F | F | F |

A=start, E,F = final states.

**Step 1: Build 0-equivalence (split by final / non-final)**

Group 0: {A,B,C,D} (non-final)
Group 1: {E,F} (final)

**Step 2: 1-equivalence — check transitions of each group's members lead to same group**

For group {A,B,C,D}:
- A: on 0→B(grp0), on 1→C(grp0) → signature (0,0)
- B: on 0→A(grp0), on 1→D(grp0) → signature (0,0)
- C: on 0→E(grp1), on 1→F(grp1) → signature (1,1)
- D: on 0→E(grp1), on 1→F(grp1) → signature (1,1)

→ Split {A,B,C,D} into {A,B} (sig 0,0) and {C,D} (sig 1,1)

For group {E,F}:
- E: on 0→E(grp1), on 1→F(grp1) → signature (1,1)
- F: on 0→F(grp1), on 1→F(grp1) → signature (1,1)

→ {E,F} stays together (same signature)

**1-equivalent partition:** {A,B}, {C,D}, {E,F}

**Step 3: 2-equivalence — refine using 1-equivalence groups**

Label groups: P1={A,B}, P2={C,D}, P3={E,F}

- A: 0→B∈P1, 1→C∈P2 → (P1,P2)
- B: 0→A∈P1, 1→D∈P2 → (P1,P2)
- C: 0→E∈P3, 1→F∈P3 → (P3,P3)
- D: 0→E∈P3, 1→F∈P3 → (P3,P3)
- E: 0→E∈P3, 1→F∈P3 → (P3,P3)
- F: 0→F∈P3, 1→F∈P3 → (P3,P3)

A,B both (P1,P2) → stay together.
C,D,E,F all (P3,P3) → stay together.

**2-equivalent partition:** {A,B}, {C,D,E,F} → same as before grouping check — but wait, C,D were in P2, E,F in P3. Now C,D,E,F all map to (P3,P3). Check: is {C,D,E,F} consistent as ONE group, splitting further?

Re-examine with merged groups Q1={A,B}, Q2={C,D,E,F}:
- C: 0→E∈Q2, 1→F∈Q2 → (Q2,Q2)
- D: 0→E∈Q2, 1→F∈Q2 → (Q2,Q2)
- E: 0→E∈Q2, 1→F∈Q2 → (Q2,Q2)
- F: 0→F∈Q2, 1→F∈Q2 → (Q2,Q2)
All same → {C,D,E,F} stays merged.

- A: 0→B∈Q1, 1→C∈Q2 → (Q1,Q2)
- B: 0→A∈Q1, 1→D∈Q2 → (Q1,Q2)
Both same → {A,B} stays merged.

**No further splitting → Stable partition:** {A,B}, {C,D,E,F}

**Minimized DFA (2 states):**

Let X = {A,B} (start, non-final), Y = {C,D,E,F} (final, since contains E,F)

| State | 0 | 1 |
|-------|---|---|
| →X | X | Y |
| *Y | Y | Y |

This accepts: any string with at least one '1' (once you hit Y via a 1, you stay in Y).

---

### PROBLEM 5: Direct DFA Design — Even a's, Odd b's

**L = {w | w has even number of a's AND odd number of b's}**

States track parity of (#a mod 2, #b mod 2):
- q00 = (even a, even b) — start
- q01 = (even a, odd b) — **final** (this is what we want)
- q10 = (odd a, even b)
- q11 = (odd a, odd b)

| State | a | b |
|-------|---|---|
| →q00 | q10 | q01 |
| *q01 | q11 | q00 |
| q10 | q00 | q11 |
| q11 | q01 | q10 |

Final state: q01 only.

---

### PROBLEM 6: DFA for strings NOT containing substring '110'

**L = {w | w does not contain '110' as substring}**

States track the longest suffix matching a prefix of "110":
- A = no progress (start) — last char wasn't part of pattern, or empty
- B = seen "1" (suffix matches "1")
- C = seen "11" (suffix matches "11")
- D = DEAD/trap state — "110" seen (reject — non-final, absorbing)

All of A, B, C are final (no "110" yet); D is non-final trap.

| State | 0 | 1 |
|-------|---|---|
| →*A | A | B |
| *B | A | C |
| *C | D | C |
| D | D | D |

Transition logic:
- A on 1 → B (progress: "1")
- B on 1 → C (progress: "11")
- B on 0 → A (no "0" follows "1" pattern... "10" doesn't contain "110", reset)
- C on 1 → C ("111..." — still suffix "11")
- C on 0 → D ("110" found!) → DEAD
- D on anything → D (stuck forever, string permanently rejected)

A, B, C final; D non-final (trap).

---

### PROBLEM 7: DFA — Binary string divisible by 3

**L = {w ∈ {0,1}* | decimal value of w (as binary number) ≡ 0 mod 3}**

States represent remainder mod 3: S0 (start, remainder 0 — final), S1 (remainder 1), S2 (remainder 2)

Transition rule: if current remainder is r and we read bit b, new value = 2r + b, new remainder = (2r+b) mod 3.

| State | 0 | 1 |
|-------|---|---|
| →*S0 | S0 (2·0+0=0) | S1 (2·0+1=1) |
| S1 | S2 (2·1+0=2) | S0 (2·1+1=3≡0) |
| S2 | S1 (2·2+0=4≡1) | S2 (2·2+1=5≡2) |

Final state: S0 (remainder 0 means divisible by 3). Note: empty string ε has value 0, which is divisible by 3, so S0 is start AND final.

**Verify**: "110" = 6, 6 mod 3 = 0. Path: S0→(1)S1→(1)S0→(0)S0 ✓ ends at S0 (final).

**Divisible by 5** (analogous, but 5 states S0..S4):

| State | 0 | 1 |
|-------|---|---|
| →*S0 | S0 | S1 |
| S1 | S2 | S3 |
| S2 | S4 | S0 |
| S3 | S1 | S2 |
| S4 | S3 | S4 |

(new remainder = (2r+b) mod 5)

---

### PROBLEM 8: DFA — Length of w is multiple of 3

**L = {w | |w| ≡ 0 mod 3}** over Σ={0,1} (or any alphabet)

States: S0 (start, final — length 0 is multiple of 3), S1, S2 — track |w| mod 3. Self-loop on every symbol cycling through states.

| State | 0 | 1 |
|-------|---|---|
| →*S0 | S1 | S1 |
| S1 | S2 | S2 |
| S2 | S0 | S0 |

---

### PROBLEM 9: DFA — Starts and ends with different symbols (over {0,1})

**L = {w | first symbol ≠ last symbol, |w|≥2}**

We need to remember the first symbol, then track the last symbol seen.

States:
- start (before reading anything)
- A0 = first symbol was 0, last symbol read so far = 0 (non-final)
- A1 = first symbol was 0, last symbol read so far = 1 (final, since 0≠1)
- B0 = first symbol was 1, last symbol read so far = 0 (final, since 1≠0)
- B1 = first symbol was 1, last symbol read so far = 1 (non-final)

| State | 0 | 1 |
|-------|---|---|
| →start | A0 | B1 |
| A0 | A0 | A1 |
| *A1 | A0 | A1 |
| *B0 | B0 | B1 |
| B1 | B0 | B1 |

Final states: A1, B0 (first ≠ last). 'start' is non-final (empty string excluded; |w|≥1 single char also excluded since first=last trivially... actually for |w|=1, first=last always, so non-final — handled correctly since start transitions directly to A0/B1, both non-final, matching that |w|=1 can't satisfy the condition unless more symbols follow).
# MODULE 2: Finite Automata with Output

## B. THEORY (Short Answer)

### 1. Moore vs Mealy Machines

| Aspect | Moore Machine | Mealy Machine |
|--------|--------------|---------------|
| Output depends on | Current state only | Current state + current input |
| Output function | λ: Q → Δ (output associated with state) | λ: Q × Σ → Δ (output associated with transition/edge) |
| Output length for input of length n | n+1 (including output at start state) | n |
| Output changes | Synchronously with clock (state change) | Can change immediately on input change (asynchronously w.r.t. state) |
| Number of states (generally) | More states needed | Fewer states (often) |
| Diagram convention | Output written INSIDE the state circle | Output written ON the transition arrow, as "input/output" |

**Block Diagrams:**

Moore Machine:
```
Input ──► [Next State Logic] ──► State Register ──► [Output Logic] ──► Output
              ▲                        │
              └────────────────────────┘
        (output depends ONLY on state register value)
```

Mealy Machine:
```
Input ──┬──► [Next State Logic] ──► State Register
        │                                │
        └──────────────┬─────────────────┘
                        ▼
                 [Output Logic] ──► Output
        (output depends on BOTH input and current state)
```

Formal definitions:
- Moore: M = (Q, Σ, Δ, δ, λ, q0) where λ: Q → Δ
- Mealy: M = (Q, Σ, Δ, δ, λ, q0) where λ: Q × Σ → Δ

---

### 2. Equivalent States, Distinguishable States, k-Equivalence

**Equivalent states**: Two states p and q are **equivalent** if, for every input string w, the machine produces the same output sequence starting from p as it does starting from q. Written p ≡ q.

**Distinguishable states**: p and q are **distinguishable** if there exists at least one input string w such that the output sequences from p and q differ. (Negation of equivalent.)

**k-equivalence**: Two states p, q are **k-equivalent** (p ≡ᵏ q) if for every input string w of length ≤ k, the output sequences produced from p and q are identical.

- **0-equivalence**: p ≡⁰ q if λ(p) = λ(q) (same immediate output)
- **k-equivalence**: p ≡ᵏ q if p ≡^(k-1) q AND for every input symbol a, δ(p,a) ≡^(k-1) δ(q,a)

Two states are **truly equivalent** (∞-equivalent) if they are k-equivalent for all k ≥ 0. For a machine with n states, if p ≡^(n-1) q, then p ≡ q (no further splitting possible beyond n-1 iterations).

---

### 3. Lossless and Lossy Machines

**Lossless Machine**: A machine is lossless if, given any output sequence, the corresponding input sequence can be uniquely determined (the input can be "decoded" from the output). I.e., different input sequences always produce different output sequences (for sufficiently long inputs).

**Lossy Machine**: A machine where two or more different input sequences CAN produce the same output sequence — information about the input is "lost" and cannot be recovered from output alone.

**Testing Table**: A table used to check losslessness. For each pair of states (p,q) and each input symbol a, we check whether δ(p,a) and δ(q,a) produce the SAME output. If yes, the pair (δ(p,a), δ(q,a)) is added to the table for further checking (this pair must also be "resolved" — i.e., must eventually become distinguishable by output, or the machine is lossy of some finite order; if it never resolves and keeps regenerating the same pairs in a cycle, machine is NOT lossless / lossy).

Construction:
1. For each pair of distinct states (p,q), and for each input a:
   - If λ(p,a) ≠ λ(q,a) → pair (p,q) is "resolved" at order 1 (distinguishable after 1 input)
   - If λ(p,a) = λ(q,a) → add new pair (δ(p,a), δ(q,a)) to be tested in next level
2. Repeat for increasing input length (order) until either:
   - All pairs resolved → machine is **lossless of finite order μ** (μ = max steps needed)
   - Some pair never resolves (repeats in a cycle without distinguishing) → **lossy**

**Testing Graph**: A directed graph representation of the Testing Table.
- Nodes = pairs of states (p,q), p≠q
- Draw an edge from (p,q) to (p',q') labeled 'a' if δ(p,a)=p', δ(q,a)=q', and λ(p,a)=λ(q,a) (i.e., outputs match, so pair (p,q) is NOT yet resolved, "passes" to (p',q'))
- If a node (p,q) has NO outgoing edges (i.e., for every input the outputs differ) → that pair is resolved (terminal node)
- **Machine is lossless** iff every node in the testing graph eventually leads to a terminal (resolved) node — i.e., no cycles exist that don't pass through resolution, OR more precisely: every pair gets resolved within finite steps (no infinite loop of "unresolved" pairs).
- **Machine is lossy** iff there exists a cycle in the testing graph consisting entirely of unresolved pairs (pairs that never get distinguished by output, no matter how long the input).

---

## A. NUMERICAL PROBLEMS

### PROBLEM 1: Moore to Mealy Conversion

**Given Moore Machine:**

| State | Output | Next (a=0) | Next (a=1) |
|-------|--------|-----------|-----------|
| →q0 | 0 | q1 | q2 |
| q1 | 1 | q1 | q3 |
| q2 | 0 | q3 | q2 |
| q3 | 1 | q0 | q1 |

**Method**: For Moore→Mealy, the output associated with a transition into state Q becomes the output on the EDGE leading to Q (i.e., output of next state, not current state).

**Mealy Machine equivalent:**

| State | Input 0 (Next/Output) | Input 1 (Next/Output) |
|-------|------------------------|------------------------|
| →q0 | q1 / 1 | q2 / 0 |
| q1 | q1 / 1 | q3 / 1 |
| q2 | q3 / 1 | q2 / 0 |
| q3 | q0 / 0 | q1 / 1 |

**Explanation**: 
- q0 → q1 on input 0: in Moore, output is λ(q1)=1, so Mealy edge q0→q1 has output "1".
- q0 → q2 on input 1: λ(q2)=0, so output "0".
- q1 → q1 on 0: λ(q1)=1 → output "1"
- q1 → q3 on 1: λ(q3)=1 → output "1"
- q2 → q3 on 0: λ(q3)=1 → output "1"
- q2 → q2 on 1: λ(q2)=0 → output "0"
- q3 → q0 on 0: λ(q0)=0 → output "0"
- q3 → q1 on 1: λ(q1)=1 → output "1"

Note: The Mealy machine produces ONE FEWER output symbol than Moore for the same input (Moore includes the initial state's output before any input is read; Mealy doesn't).

---

### PROBLEM 2: Mealy to Moore Conversion

**Given Mealy Machine:**

| State | Input 0 (Next/Out) | Input 1 (Next/Out) |
|-------|---------------------|---------------------|
| →q0 | q1/0 | q2/1 |
| q1 | q0/1 | q1/0 |
| q2 | q2/0 | q0/1 |

**Method**: State Splitting — if a state has incoming edges with DIFFERENT outputs, split it into multiple copies, one for each distinct incoming output value.

**Step 1: Find incoming output values for each state**

- q0: incoming from q1 (output 1), from q2 (output 1) → only output "1" → no split needed, but need an arbitrary initial output (say 0, since start state's initial output is unconstrained — pick 0 by convention, OR if q0 also has self-info we examine; here only "1" comes in, but since it's start state we still need SOME output - typically set arbitrarily, commonly 0)
  
  Actually re-examine: incoming edges to q0: from q1 on input1→q0/1 (output1), from q2 on input1→q0/1 (output1). Both are "1". So q0 needs only ONE copy with output 1. (For start state, if no incoming edges existed, output would be arbitrary; here it's determined as 1.)

- q1: incoming from q0 (output 0), from q1 itself (output 0) → only "0" → one copy, output 0

- q2: incoming from q0 (output 1), from q2 itself (output 0) → TWO DIFFERENT outputs (1 and 0) → SPLIT into q2_1 (output 1) and q2_0 (output 0)

**Step 2: Construct Moore states**

- q0/1 (since all incoming = 1)
- q1/0
- q2_1/1 (reached via output 1)
- q2_0/0 (reached via output 0)

**Step 3: Redirect transitions**

Original Mealy transitions:
- q0 -0/0→ q1 ⟹ Moore: q0 -0→ q1 (q1 has output 0 ✓)
- q0 -1/1→ q2 ⟹ Moore: q0 -1→ q2_1 (need output 1)
- q1 -0/1→ q0 ⟹ Moore: q1 -0→ q0 (q0 has output 1 ✓)
- q1 -1/0→ q1 ⟹ Moore: q1 -1→ q1 (output 0 ✓)
- q2 -0/0→ q2 ⟹ Moore: from BOTH q2_1 and q2_0, on input 0 → q2_0 (output 0)
- q2 -1/1→ q0 ⟹ Moore: from BOTH q2_1 and q2_0, on input 1 → q0 (output 1 ✓)

**Final Moore Machine:**

| State | Output | Next(0) | Next(1) |
|-------|--------|---------|---------|
| →q0 | 1 | q1 | q2_1 |
| q1 | 0 | q0 | q1 |
| q2_1 | 1 | q2_0 | q0 |
| q2_0 | 0 | q2_0 | q0 |

(5 states total → could check if q2_1 and q2_0 differ in next-state behavior; here they differ in output so both retained.)

---

### PROBLEM 3: Minimization of Incompletely Specified Sequential Machine

**Given (incomplete Mealy machine, '-' = don't care for next-state or output):**

| State | Input 0 (Next/Out) | Input 1 (Next/Out) |
|-------|---------------------|---------------------|
| S1 | S2/0 | S3/1 |
| S2 | S4/1 | -/- |
| S3 | S2/0 | S4/1 |
| S4 | S1/1 | S3/- |
| S5 | S4/1 | S3/1 |

**Step 1: Compatibility Definition**

Two states p,q are **compatible** if for every input where BOTH have specified outputs, the outputs match, AND their next-states (where both specified) are either equal or also compatible (checked recursively/iteratively).

**Build initial compatibility pairs** — check 0-th order (output compatibility for specified entries):

- (S1,S2): on 0: out(S1)=0, out(S2)=1 → MISMATCH → S1,S2 NOT compatible
- (S1,S3): on 0: out=0,0 match; on 1: out=1,1 match → compatible candidate
- (S1,S4): on 0: out(S1)=0, out(S4)=1 → mismatch → NOT compatible
- (S1,S5): on 0: out(S1)=0, S5 input0 out=1 → mismatch → NOT compatible
- (S2,S3): on 0: out(S2)=1, out(S3)=0 → mismatch → NOT compatible
- (S2,S4): on 0: out(S2)=1, out(S4)=1 match; on1: S2 is don't care(-), S4 out=- (don't care too, "-/​-")→ no constraint → compatible candidate
- (S2,S5): on 0: out(S2)=1, out(S5)=1 match; on1: S2=-, S5 has out=1, don't care doesn't conflict → compatible candidate
- (S3,S4): on 0: out=0 vs 1 → mismatch → NOT compatible
- (S3,S5): on 0: out(S3)=0, out(S5)=1 → mismatch → NOT compatible
- (S4,S5): on 0: out(S4)=1, out(S5)=1 match; on1: out(S4)=-(don't care), out(S5)=1 → no conflict → compatible candidate

**Compatibility Graph** (nodes = states; edge between compatible pairs):
```
S1 — S3
S2 — S4
S2 — S5
S4 — S5
```
(S1 connects only to S3; S2,S4,S5 form a triangle of mutual compatibility)

**Step 2: Check next-state compatibility (close the relation)**

For pair (S1,S3): 
- on 0: next(S1)=S2, next(S3)=S2 → same → OK
- on 1: next(S1)=S3, next(S3)=S4 → need (S3,S4) compatible? We found S3,S4 NOT compatible → therefore (S1,S3) is actually NOT compatible (must remove edge)!

Recheck: (S1,S3) fails because it requires (S3,S4) compatible, which fails. So S1,S3 are NOT compatible after closure. S1 is compatible with NO other state → S1 stands alone.

For pair (S2,S4):
- on 0: next(S2)=S4, next(S4)=S1 → need (S4,S1) compatible? S1 compatible with nobody → (S4,S1) NOT compatible → (S2,S4) fails!

For pair (S2,S5):
- on 0: next(S2)=S4, next(S5)=S4 → same state → OK
- on 1: S2's transition is don't-care entirely → no constraint
→ (S2,S5) remains compatible.

For pair (S4,S5):
- on 0: next(S4)=S1, next(S5)=S4 → need (S1,S4) compatible? Already shown NOT compatible → (S4,S5) fails!

**After closure — Merger Table / final compatible pairs:** Only (S2,S5) survives.

**Step 3: Maximal Compatibles**

- {S1} (alone)
- {S2, S5}
- {S3} (alone)
- {S4} (alone)

**Step 4: Minimized Machine (rename {S2,S5} = A)**

New states: S1, A(={S2,S5}), S3, S4

Merge transitions of S2 and S5 (combine specified entries, don't-cares filled from the other):
- A on 0: S2→S4/1, S5→S4/1 → A -0→ S4 / output 1
- A on 1: S2→ -/- (don't care), S5→S3/1 → use S5's specified: A -1→ S3 / output 1

| State | Input 0 (Next/Out) | Input 1 (Next/Out) |
|-------|---------------------|---------------------|
| S1 | S2→A / 0 | S3 / 1 |
| A | S4 / 1 | S3 / 1 |
| S3 | A / 0 | S4 / 1 |
| S4 | S1 / 1 | S3 / - |

**Minimized Closed State Table (4 states): S1, A, S3, S4** — reduced from 5 states.
# MODULE 3: Regular Expressions & Languages

## B. THEORY (Short Answer)

### 1. Arden's Theorem — Statement and Proof

**Statement**: If P and Q are regular expressions over Σ, and P does NOT contain ε (i.e., ε ∉ L(P)), then the equation:

**R = Q + RP**

has a UNIQUE solution given by:

**R = QP\***

**Proof**:

*(a) R = QP\* satisfies the equation:*
RP = QP\*P = Q(P\*P) = Q(PP\* ) ... actually use identity P* = ε + PP* = ε + P*P
So QP* P = Q(P*P) and we want to show QP*P + Q = QP*
Q + RP = Q + QP\*P = Q(ε + P\*P) = Q·P\* = R ✓ (using identity ε + P*P = P*)

*(b) Uniqueness:* Suppose R also satisfies R = Q + RP. Substitute repeatedly:
R = Q + RP = Q + (Q+RP)P = Q + QP + RP² = Q + QP + QP² + RP³ = ... = Q(ε+P+P²+...+Pⁿ) + RPⁿ⁺¹

Since ε ∉ L(P), every string in L(Pⁿ⁺¹) has length ≥ n+1. As n→∞, for any fixed-length string w ∈ L(R), the term RPⁿ⁺¹ contributes no strings of length ≤ n. Hence in the limit, R = Q(ε+P+P²+...) = QP\*.

So R = QP\* is the UNIQUE solution. ∎

**Application**: Used to convert a DFA into a regular expression by writing one equation per state (in terms of itself and other states via transitions) and solving via substitution + Arden's lemma.

---

### 2. Pumping Lemma for Regular Sets (Statement)

**Statement**: If L is a regular language, then there exists a constant n (called the **pumping length**, depending on L) such that for every string w ∈ L with |w| ≥ n, w can be split into THREE parts w = xyz satisfying:

1. |xy| ≤ n
2. |y| > 0 (y is non-empty)
3. For all i ≥ 0, xyⁱz ∈ L (pumping y any number of times — including 0 times — keeps the string in L)

**Intuition**: n = number of states in the minimal DFA for L. By pigeonhole principle, any string of length ≥ n must revisit some state while being processed — the loop between the two visits to that state is "y", which can be repeated (pumped) any number of times without leaving L.

**Used to prove a language is NOT regular**: by contradiction — assume L is regular, get pumping length n, choose a specific w∈L with |w|≥n, show that for SOME split satisfying conditions 1&2, pumping (condition 3) produces a string NOT in L — contradiction.

---

### 3. Algebraic Identities of Regular Expressions

| Identity | Meaning |
|----------|---------|
| r + r = r | Idempotent union |
| r + ∅ = r | ∅ is identity for union |
| r·ε = ε·r = r | ε is identity for concatenation |
| r·∅ = ∅·r = ∅ | ∅ annihilates concatenation |
| (r*)* = r* | Closure is idempotent |
| ε* = ε, ∅* = ε | Closure of empty/null |
| (r+s) = (s+r) | Union commutative |
| (r+s)+t = r+(s+t) | Union associative |
| (rs)t = r(st) | Concatenation associative |
| r(s+t) = rs+rt | Left distributive |
| (s+t)r = sr+tr | Right distributive |
| r* = ε + rr* = ε + r*r | Recursive definition (Arden form basis) |
| r* = (ε+r)* | |
| (r+s)* = (r*s*)* = r*(sr*)* = (r*s*)* | All denote strings made by interleaving r's and s's |
| r+rs* s ... | (used in derivations) |
| (r*r*)=r* | |

---

### 4. Closure Properties of Regular Sets

**Claim**: Regular languages are closed under Union, Intersection, Complement, Concatenation, Kleene Star, Reversal, and Difference.

**(a) Union**: If L1, L2 are regular (recognized by DFA M1, M2), construct product automaton M with states Q1×Q2, δ((p,q),a)=(δ1(p,a),δ2(q,a)), F = {(p,q) | p∈F1 OR q∈F2}. M accepts L1∪L2. Alternatively, regex r1+r2.

**(b) Intersection**: Same product construction but F = {(p,q) | p∈F1 AND q∈F2}. M accepts L1∩L2.

**(c) Complement**: If L is regular with DFA M=(Q,Σ,δ,q0,F) — IMPORTANT: M must be a COMPLETE DFA (total transition function, including trap states). Construct M' = (Q,Σ,δ,q0,Q-F) (swap final/non-final states). L(M') = Σ* − L = complement of L.

**(d) Difference**: L1 − L2 = L1 ∩ complement(L2). Since regular languages closed under intersection and complement, L1−L2 is regular.

**(e) Concatenation**: If L1,L2 regular with ε-NFAs N1,N2, connect every final state of N1 to start state of N2 via ε-transition. Result accepts L1L2.

**(f) Kleene Star**: Add ε-transitions from all final states of N back to start state, and make start state final (to accept ε). Result accepts L*.

**(g) Reversal**: L^R = {w^R | w ∈ L}. Given DFA/NFA for L, reverse all transition arrows, swap start and final states (make old final states the new start — possibly multiple, requires NFA with multiple starts or a new single start with ε-transitions to each old final state; make old start the unique new final state). Result accepts L^R.

---

## A. NUMERICAL PROBLEMS

### PROBLEM 1: FA to Regular Expression using Arden's Theorem

**Given DFA:**

| State | 0 | 1 |
|-------|---|---|
| →q0 | q0 | q1 |
| *q1 | q1 | q0 |

q0 = start (non-final), q1 = final.

**Step 1: Write equations for each state**

A state equation: (state) = Σ over incoming edges (source · symbol), plus "ε" if it's the start state.

q0 = q0·0 + q1·1 + ε   ... (incoming: self-loop on 0, from q1 on 1; plus ε since start)
q1 = q0·1 + q1·1        ... (incoming: from q0 on 1, self-loop on 1)

**Step 2: Solve q1 first using Arden's Lemma**

q1 = q0·1 + q1·1
Form R = Q + RP where R=q1, Q=q0·1, P=1
By Arden: q1 = (q0·1)·1* = q0·1·1* = q0 1 1*

**Step 3: Substitute into q0's equation**

q0 = q0·0 + (q0·1·1*)·1 + ε
q0 = q0·0 + q0·1·1*·1 + ε
q0 = q0(0 + 11*1) + ε

Now apply Arden again: R=q0, Q=ε, P=(0+11*1)
q0 = ε·(0+11*1)\* = (0 + 11*1)\*

**Step 4: Final answer**

q1 = q0·1·1* = (0+11*1)\* · 1·1\* = (0+11*1)\* 11\*

Since q1 is the only final state, the language = q1's expression:

**L = (0 + 11*1)\* 1 1\***

(This represents strings ending in 1 with no constraint other than parity tracking — verifies: this DFA tracks parity of number of 1's; final state q1 means odd number of 1's. Indeed (0+11*1)* generates strings with even count of 1's interspersed with 0's, followed by 11* adding one more odd-making block of 1's.)

---

### PROBLEM 2: Regular Expression to NFA (Thompson's Construction)

**Thompson's Construction Rules** (build bottom-up):

1. **ε**: single edge labeled ε between new start and new final state.
2. **symbol a**: single edge labeled 'a' between new start and new final state.
3. **R = R1 + R2 (union)**: new start state with ε-edges to starts of N1 and N2; both finals of N1,N2 have ε-edges to a new common final state.
4. **R = R1·R2 (concatenation)**: final state of N1 merges (via ε-edge) into start state of N2. New overall start = start of N1, new overall final = final of N2.
5. **R = R1\* (Kleene star)**: new start state with ε-edge to start of N1 AND ε-edge directly to new final state (to accept ε); final of N1 has ε-edge back to start of N1 (loop) AND ε-edge to new final state.

---

**(a) NFA for (0+1)*1(0+1)**

Construction outline:
- Build N1 for (0+1): two parallel ε-paths, one labeled 0, one labeled 1, between a start s1 and final f1.
- Build N2 = N1* : new start S, new final F; S--ε-->s1, f1--ε-->S(loop) and f1--ε-->F, and S--ε-->F (to allow ε / zero repetitions)
- Build N3 for symbol '1': single edge p--1-->q
- Build N4 for (0+1) again (another copy, states r1 to g1)
- Concatenate: N2·N3·N4 — connect F--ε-->p (N2's final to N3's start), q--ε-->r1 (N3's final to N4's start)
- Overall start = S (of N2), overall final = g1 (of N4)

**Resulting NFA description** (states q0..q8):
- q0 (start) --ε--> q1, q0 --ε--> q8 (skip star, but then must still go through "1(0+1)" — actually overall start only connects into the star portion; the "1(0+1)" is mandatory)

Simplify into a cleaner equivalent NFA (5 states, standard textbook form):
- q0 (start): on 0 → q0 (self-loop, part of star); on 1 → q0 (self-loop) AND on 1 → q1 (guess: this is the mandatory "1")
- q1: on 0 → q2, on 1 → q2
- *q2 (final)

| State | 0 | 1 |
|-------|---|---|
| →q0 | {q0} | {q0,q1} |
| q1 | {q2} | {q2} |
| *q2 | – | – |

This NFA accepts (0+1)*1(0+1): the (0+1)* part is the self-loop at q0, q0→q1 on '1' is the mandatory middle "1", and q1→q2 on (0+1) is the last symbol.

---

**(b) NFA for 10 + (0+11)0\*1**

This is a union of two sub-expressions: R1 = "10" and R2 = "(0+11)0*1"

- N(R1): p0 --1--> p1 --0--> p2 (final)
- N(R2): 
  - sub-expr (0+11): r0 --0--> r3 (final-ish) AND r0--1-->r1--1-->r3 (two paths into r3)
  - then 0*: r3 --0--> r3 (self loop, ε to skip)
  - then 1: r3 --1--> r4 (final)

Overall NFA: new start S with ε-edges to p0 and r0. Final states: p2 and r4 (or merge into single final F via ε-edges from p2,r4).

**Combined transition table** (states: S,p0,p1,p2,r0,r1,r3,r4 — using ε):

| State | ε | 0 | 1 |
|-------|---|---|---|
| →S | {p0,r0} | – | – |
| p0 | – | – | {p1} |
| p1 | – | {p2} | – |
| *p2 | – | – | – |
| r0 | – | {r3} | {r1} |
| r1 | – | – | {r3} |
| r3 | – | {r3} | {r4} |
| *r4 | – | – | – |

---

**(c) NFA for (a+b)\*abb**

Classic example. States q0(start)–q1–q2–q3(final):

| State | a | b |
|-------|---|---|
| →q0 | {q0,q1} | {q0} |
| q1 | – | {q2} |
| q2 | – | {q3} |
| *q3 | – | – |

- q0: self-loop on a and b (the (a+b)* part); also on 'a' can move to q1 (start matching "abb")
- q1 --b--> q2 (matched "ab")
- q2 --b--> q3 (matched "abb") — accept

---

**(d) NFA for (00+11)\*((01+10)(00+11)\*(01+10)(00+11)\*)\*** — "Even 0s, Even 1s"

This expression generates all strings with an EVEN number of 0's AND an EVEN number of 1's.

**Simpler equivalent DFA** (4 states tracking parity of #0 and #1):

- E_E = even 0's, even 0's, even 1's (start, FINAL)
- E_O = even 0's, odd 1's
- O_E = odd 0's, even 1's
- O_O = odd 0's, odd 1's

| State | 0 | 1 |
|-------|---|---|
| →*EE | OE | EO |
| EO | OO | EE |
| OE | EE | OO |
| OO | EO | OE |

This 4-state DFA is the minimal equivalent of the given (complex) regular expression — given expression generates same language: strings with even #0's and even #1's.

---

### PROBLEM 3: Pumping Lemma — Prove languages NOT regular

**(a) L = {aⁿbⁿ | n ≥ 0}**

*Proof by contradiction:*
Assume L is regular. By Pumping Lemma, there exists pumping length p.
Choose w = a^p b^p ∈ L (|w| = 2p ≥ p ✓).

By PL, w = xyz where |xy| ≤ p, |y| > 0, and xyⁱz ∈ L for all i≥0.

Since |xy| ≤ p, both x and y consist entirely of a's (since the first p characters of w are all a's). So y = a^k for some k ≥ 1 (since |y|>0).

Now consider i=2 (pump y once more): xy²z = a^(p+k) b^p.

Number of a's = p+k, number of b's = p. Since k≥1, p+k ≠ p, so xy²z has UNEQUAL numbers of a's and b's ⟹ xy²z ∉ L.

This CONTRADICTS the pumping lemma condition (xyⁱz ∈ L for all i). Hence L is NOT regular. ∎

---

**(b) L = {aᵖ | p is prime}**

*Proof by contradiction:*
Assume L regular, pumping length = n. Choose a prime p ≥ n (primes are infinite, so such p exists). w = a^p ∈ L.

By PL: w=xyz, |xy|≤n≤p, |y|=k≥1, and xyⁱz ∈ L for all i.

xyⁱz = a^(p + (i-1)k)  [since |xyz|=p, and each additional copy of y adds k a's]

We need p + (i-1)k to be PRIME for ALL i ≥ 0.

Choose i = p+1: length = p + p·k = p(1+k).

Since p ≥ 1 and (1+k) ≥ 2 (as k≥1), p(1+k) is a product of two integers each ≥ 2 (assuming p≥2, true since p is prime ≥2) — hence p(1+k) is COMPOSITE, not prime.

So xy^(p+1)z = a^(p(1+k)) ∉ L (length is composite, not prime).

This contradicts pumping lemma (which requires this string ∈ L). Hence L is NOT regular. ∎

---

**(c) L = {ww^R | w ∈ {0,1}\*}** (even-length palindromes)

*Proof by contradiction:*
Assume L regular with pumping length n. Choose w' = 0^n 1 1 0^n (this is of the form ww^R with w = 0^n 1, w^R = 1 0^n). |w'| = 2n+2 ≥ n ✓, and w' ∈ L.

By PL: w' = xyz, |xy| ≤ n, |y|≥1, xyⁱz ∈ L for all i.

Since |xy|≤n, x and y both lie within the first n characters, which are all 0's (the "0^n" prefix). So y = 0^k for some k≥1.

Pump down (i=0): xz = removing k 0's from the front → xz = 0^(n-k) 1 1 0^n.

For xz to be in L (form ww^R), it must be a palindrome of even length. xz has length 2n+2-k. Check: is 0^(n-k) 1 1 0^n a palindrome?

Its reverse = 0^n 1 1 0^(n-k). For xz to equal its own reverse: 0^(n-k) 1 1 0^n = 0^n 1 1 0^(n-k). This requires n-k = n, i.e., k=0. But k≥1 — CONTRADICTION (k=0 violates |y|≥1).

So xz ∉ L, contradicting pumping lemma. Hence L is NOT regular. ∎

---

**(d) L = {0^(n²) | n ≥ 1}**

*Proof by contradiction:*
Assume L regular, pumping length p. Choose w = 0^(p²) ∈ L (n=p gives n²=p² ≥ p for p≥1).

By PL: w=xyz, |xy|≤p, |y|=k where 1≤k≤p, and xyⁱz ∈ L for all i.

|xy^iz| = p² + (i-1)k.

For i=2: length = p² + k. Since 1≤k≤p, we have p² < p²+k ≤ p²+p < p²+2p+1=(p+1)².

So p² < p²+k < (p+1)², meaning p²+k lies STRICTLY BETWEEN consecutive perfect squares p² and (p+1)² — hence p²+k is NOT a perfect square.

So xy²z = 0^(p²+k) has length that is not a perfect square ⟹ xy²z ∉ L.

This contradicts the pumping lemma. Hence L is NOT regular. ∎
# MODULE 4: CFG & Pushdown Automata

## B. THEORY (Short Answer)

### 1. PDA Formal Definition (7-tuple)

**M = (Q, Σ, Γ, δ, q0, Z0, F)**

- Q = finite set of states
- Σ = finite input alphabet
- Γ = finite stack alphabet
- δ: Q × (Σ∪{ε}) × Γ → 2^(Q×Γ*) — transition function: given current state, input symbol (or ε), and top-of-stack symbol, returns a set of (next state, string to replace top-of-stack with)
- q0 ∈ Q = initial state
- Z0 ∈ Γ = initial stack symbol (bottom marker)
- F ⊆ Q = set of final/accepting states

A configuration (instantaneous description) is a triple (q, w, γ) — current state, remaining input, stack contents.

---

### 2. DPDA vs NPDA

| Aspect | DPDA (Deterministic) | NPDA (Non-deterministic) |
|--------|----------------------|---------------------------|
| Transitions | At most ONE choice for each (state, input symbol, stack-top) combination | Multiple choices possible |
| Power | Recognizes a PROPER SUBSET of CFLs (Deterministic CFLs) | Recognizes ALL Context-Free Languages |
| Closure under complement | DCFLs closed under complement | CFLs (via NPDA) NOT closed under complement |
| ε-moves | Must ensure ε-moves don't conflict with input-consuming moves on same stack-top | Can freely have multiple ε and input moves |
| Example | L = {aⁿbⁿ} is DCFL — has DPDA | L = {ww^R} needs NPDA (must "guess" the midpoint) |
| Ambiguity | Cannot accept ambiguous-requiring languages like {ww^R} or palindromes with two interpretations | Can handle such languages via guessing |

---

### 3. Acceptance by Final State vs Empty Stack — Equivalence

**Final State acceptance**: PDA accepts w if after consuming all of w, it can reach SOME state q∈F (stack content irrelevant).

**Empty Stack acceptance**: PDA accepts w if after consuming all of w, the stack becomes EMPTY (state irrelevant — F=∅ typically, or F is irrelevant).

**Theorem**: For every PDA P_F accepting by final state, there's a PDA P_E accepting the same language by empty stack, and vice versa.

**Proof (Final State → Empty Stack):**
Given P_F=(Q,Σ,Γ,δ,q0,Z0,F). Construct P_E:
- Add new bottom-of-stack symbol X0 (below Z0).
- Add new start state q0' that pushes Z0 on top of X0, then ε-moves to q0.
- Add new state qe (empty-stack trigger).
- From EVERY state in F, on ε-input, ε-move to qe and pop the top stack symbol.
- In qe, on ε-input, keep popping (any stack symbol) and stay in qe, regardless of symbol — until stack empties (reaches X0, pop X0 too).

This way, P_E empties its stack exactly when P_F would have accepted (reached a final state), for the same input. L(P_E) = L(P_F).

**Proof (Empty Stack → Final State):**
Given P_E=(Q,Σ,Γ,δ,q0,Z0). Construct P_F:
- Add new bottom symbol X0 below Z0, new start q0' pushes Z0 then ε-moves to q0.
- Add new final state qf.
- Whenever P_E's stack would become empty (i.e., the only remaining symbol popped is X0), instead ε-move to qf (and qf ∈ F).

L(P_F) = L(P_E). ∎

Both constructions are mutual — hence PDA with final-state acceptance and PDA with empty-stack acceptance are EQUIVALENT in power (both define exactly the class of CFLs).

---

### 4. Ogden's Lemma

**Statement**: Let L be a context-free language. Then there exists a constant n such that for any string w∈L with at least n "distinguished" (marked) positions, w can be written as w=uvxyz such that:

1. v and y together contain at least one distinguished position (i.e., not both empty of marks)
2. vxy contains at most n distinguished positions
3. For all i≥0, uv^ix y^iz ∈ L

**Application**: Ogden's lemma is a STRONGER version of the pumping lemma for CFLs — it allows marking specific positions in the string, giving more control/flexibility. Used to prove certain languages are NOT context-free where the ordinary CFL pumping lemma fails or is awkward — e.g., proving L = {a^i b^j c^k | i=j or j=k} is not context-free (ordinary pumping lemma alone is insufficient because you can't force the pumped part to overlap with the "interesting" region without marking).

---

### 5. Right Linear and Left Linear Grammars → Regular

**Right Linear Grammar**: Every production has the form A → wB or A → w, where A,B are non-terminals and w∈Σ* (terminal string). The single non-terminal (if any) is the RIGHTMOST symbol.

**Left Linear Grammar**: Every production has form A → Bw or A → w (non-terminal, if present, is LEFTMOST).

**Theorem**: Languages generated by right-linear or left-linear grammars are regular.

**Proof (Right Linear → Regular, via NFA construction)**:
Given right-linear grammar G with productions A→wB or A→w.
Construct NFA: 
- States = non-terminals of G, plus one new final state F.
- For A → a1a2...akB (terminals a1..ak then non-terminal B): create a chain of states/transitions from A through intermediate states reading a1,...,ak, ending at B.
- For A → a1...ak (no non-terminal, terminal production): chain ending at F (the final state).
- Start state = start symbol S of G.

Every derivation S⟹...⟹w corresponds exactly to a path from S to F reading w. So L(G) = L(NFA), hence regular.

**Proof (Left Linear → Regular)**:
A left-linear grammar generating L can be shown to generate L^R via an equivalent right-linear grammar (reverse each production's RHS: A→Bw becomes A→wB read in reverse sense, or formally: build NFA reading the string in reverse, i.e., construct an automaton for L^R which is right-linear-derivable, then reverse the automaton to get one for L). Since regular languages are closed under reversal, and L^R is regular (by the right-linear argument), L = (L^R)^R is also regular. ∎

---

## A. NUMERICAL PROBLEMS — CFG Simplification, CNF, GNF

### PROBLEM 1: CFG Simplification (3-Step Algorithm)

**Given Grammar G:**
```
S → AB | a
A → b | ε
B → C | b
C → D
D → d
```
(Σ = {a,b,d})

**Step 1: Remove ε-productions**

Identify nullable non-terminals: A→ε, so A is nullable.

For every production containing A, add a version with A removed:
- S → AB | a  →  S → AB | B | a   (since A nullable, S→AB can become S→B)
- A → b | ε   →  A → b  (remove A→ε itself)
- B → C | b  (unchanged, no A)
- C → D (unchanged)
- D → d (unchanged)

**After Step 1:**
```
S → AB | B | a
A → b
B → C | b
C → D
D → d
```

**Step 2: Remove Unit Productions (A→B type)**

Unit productions present: B→C, C→D (both are single-non-terminal RHS)

- C→D, D→d ⟹ replace C→D with C→d
- B→C, and now C→d ⟹ replace B→C with B→d

**After Step 2:**
```
S → AB | B | a
A → b
B → d | b
C → d
D → d
```
(C and D still exist as productions but may become useless — check next step)

**Step 3: Remove Useless Symbols**

A symbol X is useful if: (a) X is reachable from S, AND (b) X can derive some terminal string.

Reachability from S: S→AB|B|a. So A, B reachable. From A→b (terminal, fine). From B→d|b (terminal, fine). 

C and D: are they reachable from S? S's productions only involve A, B, a. Neither A nor B's productions (A→b, B→d|b) reference C or D. So **C and D are UNREACHABLE** from S.

Remove C and D entirely.

**Final Simplified Grammar:**
```
S → AB | B | a
A → b
B → d | b
```

All symbols: generate terminal strings ✓, reachable from S ✓. Simplified.

---

### PROBLEM 2: Convert CFG to Chomsky Normal Form (CNF)

**Given Grammar (already simplified):**
```
S → ABa | aA
A → aab | ε
B → bbA
```
(Σ={a,b})

CNF requires: every production is A→BC (two non-terminals) or A→a (single terminal). No ε-productions (except possibly S→ε if ε∈L, handled separately), no unit productions, no mixed terminal/non-terminal RHS of length>1.

**Step 1: Add new start symbol (if S appears on RHS) — here S doesn't appear on RHS, skip.**

**Step 2: Remove ε-productions**

A→ε is nullable. Productions containing A: S→ABa (A nullable→ also S→Ba), S→aA(also S→a), B→bbA (also B→bb)

```
S → ABa | Ba | aA | a
A → aab
B → bbA | bb
```

**Step 3: Remove unit productions** — none present (no A→B type single-nonterminal). Skip.

**Step 4: Convert remaining productions to CNF form**

For each production with RHS length > 1 containing terminals, introduce new non-terminals for each terminal, and break down RHS into binary form.

Introduce: T_a → a, T_b → b (terminal substitutes)

Rewrite all RHS:
- S → ABa  → S → AB T_a  (now all RHS symbols are non-terminals, but length 3 — need binary breakdown)
- S → Ba   → S → B T_a
- S → aA   → S → T_a A
- S → a    → S → T_a  (this is fine, length 1, terminal — but CNF wants A→a directly, T_a→a already does this, so S→T_a is a UNIT production — need to handle)

Let's redo more carefully. Replace terminals with new variables first:

```
S → A B T_a | B T_a | T_a A | T_a
A → T_a T_a T_b
B → T_b T_b A | T_b T_b
T_a → a
T_b → b
```

Now S→T_a is a unit production (T_a→a) — substitute: S→T_a becomes S→a — but CNF allows S→a (single terminal) directly! That's fine — A→a form IS allowed in CNF.

So: S → ABTa | BTa | TaA | a   (the last one direct since unit-production-eliminated)

Now break down RHS of length 3 (A B T_a, and T_a T_a T_b, and T_b T_b A) into binary using new non-terminals:

```
S → A X1 | B Ta | Ta A | a       where X1 → B Ta
A → Ta X2                          where X2 → Ta Tb
B → Tb X3 | Tb Tb                  where X3 → Tb A
X1 → B Ta
X2 → Ta Tb
X3 → Tb A
Ta → a
Tb → b
```

Check B→Tb Tb : this is already binary (two non-terminals: Tb, Tb) ✓ — fine for CNF.

**Final CNF Grammar:**
```
S  → A X1 | B Ta | Ta A | a
A  → Ta X2
B  → Tb X3 | Tb Tb
X1 → B Ta
X2 → Ta Tb
X3 → Tb A
Ta → a
Tb → b
```

Every production is now either X→YZ (two non-terminals) or X→a (single terminal). ✓ CNF achieved.

---

### PROBLEM 3: Convert CFG to GNF (Greibach Normal Form)

GNF requires every production of form **A → aV*** where a∈Σ (single terminal) and V* is a (possibly empty) string of non-terminals only. (A→a is also allowed as special case, and S→ε if needed.)

**Given Grammar (with left recursion):**
```
A → Aa | Ab | a
```
(classic left-recursive grammar)

**Step 1: Eliminate Left Recursion**

For A → Aα | Aβ | γ (where γ doesn't start with A), rewrite using a new non-terminal A':

```
A  → γ A'
A' → α A' | β A' | α | β
```

Here: γ = a (the non-left-recursive alternative), α=a, β=b (the left-recursive continuations)

```
A  → a A'
A' → a A' | b A' | a | b
```

**Step 2: Verify GNF form**

- A → a A'  → starts with terminal 'a', followed by non-terminal A' ✓ GNF
- A' → a A' → ✓ GNF (terminal 'a' + non-terminal A')
- A' → b A' → ✓ GNF
- A' → a   → ✓ GNF (terminal alone, allowed)
- A' → b   → ✓ GNF

**Final GNF:**
```
A  → aA'
A' → aA' | bA' | a | b
```

This grammar generates L = a(a+b)* — same language as original A→Aa|Ab|a (which generates a followed by zero or more a's/b's).

---

**General GNF Algorithm for larger grammars:**
1. Convert to CNF first (optional but simplifies).
2. Order non-terminals A1 < A2 < ... < An (with A1=start).
3. Eliminate left recursion for each Ai in order (as shown above).
4. For each production Ai → Ajα where j<i, substitute Aj's productions (which by induction already start with terminals) into Ai's production — repeat until every Ai's production starts with a terminal.
# MODULE 4 (continued): Ambiguity Proofs & PDA Design

## A. NUMERICAL — Ambiguity Proofs

### PROBLEM 4: Prove Grammars are Ambiguous

A grammar G is **ambiguous** if SOME string w∈L(G) has TWO OR MORE distinct parse trees (equivalently, two distinct leftmost derivations).

---

**(a) E → E+E | E*E | (E) | id**

Consider the string: **id + id * id**

**Parse Tree 1** (interpreting as id + (id*id), i.e., * binds tighter — but grammar gives no precedence, so BOTH groupings are valid derivations):

```
Leftmost Derivation 1:
E ⟹ E+E ⟹ id+E ⟹ id+E*E ⟹ id+id*E ⟹ id+id*id

Tree 1:
        E
      / | \
     E  +  E
     |    /|\
    id   E * E
         |   |
        id  id
```
(This corresponds to id + (id*id))

**Parse Tree 2** (interpreting as (id+id)*id):

```
Leftmost Derivation 2:
E ⟹ E*E ⟹ E+E*E ⟹ id+E*E ⟹ id+id*E ⟹ id+id*id

Tree 2:
        E
      / | \
     E  *  E
    /|\     |
   E + E   id
   |   |
  id  id
```
(This corresponds to (id+id)*id)

Both derivations produce the SAME STRING "id+id*id" but DIFFERENT parse trees (different structures) ⟹ **G is AMBIGUOUS**. ∎

---

**(b) S → aSbS | bSaS | ε**

Consider the string: **ab**

**Derivation 1**: S ⟹ aSbS ⟹ a(ε)bS ⟹ ab(ε) ⟹ ab
Using S→aSbS, then both inner S's → ε.

Tree 1:
```
      S
   / | | \
  a  S b  S
     |    |
     ε    ε
```

**Derivation 2**: Consider string **abab**. 

Derivation A: S⟹aSbS, first S⟹ε, second S⟹aSbS⟹ab(with inner S's →ε) ... Let's verify with "ab":

Actually for "ab" itself: only S→aSbS with both S's→ε gives "ab". Is there another way? S→bSaS would start with 'b' — doesn't match "ab" at position 1. So for "ab" specifically, seems unique.

**Better example — string "abab":**

Derivation 1: S⟹aSbS 
- First S(after a)→ε, gives "ab"+S(second)
- Second S → aSbS → a(ε)b(ε) = "ab"
- Total: a·ε·b·(ab) = "abab" ✓
Tree: a S(=ε) b S(=aSbS→ab)

Derivation 2: S⟹aSbS
- First S(after a) → bSaS → b(ε)a(ε) = "ba"... gives "a"+"ba"+"b"+S(second, =ε) = "abab" ✓
Tree: a S(=bSaS→ba) b S(=ε)

Both derivations yield "abab" via DIFFERENT parse trees (different groupings of which S expands to what) ⟹ **G is AMBIGUOUS**. ∎

(This grammar generates all strings with equal number of a's and b's — a classic ambiguous grammar for that language.)

---

**(c) S → iCtS | iCtSeS | a   (Dangling-Else)**

(i = "if", C = condition, t="then", e="else", a = simple statement; this models: if C then S, or if C then S else S)

Consider string: **iCtiCtaea** (i.e., "if C then if C then a else a")

**Derivation 1** (else binds to the SECOND/inner if — standard interpretation):
S ⟹ iCtS ⟹ iCt(iCtSeS) ⟹ iCt(iCt a e a) = iCtiCtaea

Tree 1: outer "if" has NO else; inner "if" HAS the else.
```
        S
     /  |  \
    i  Ct   S
            |
          iCtSeS
          /|  |  \
        iCt  a  e  a
```

**Derivation 2** (else binds to the FIRST/outer if):
S ⟹ iCtSeS ⟹ iCt(iCtS)eS ⟹ iCt(iCta)e(a) = iCtiCtaea

Tree 2: outer "if" HAS the else (attached to outer); inner "if" has NO else.
```
        S
     /  | \  \
    i  Ct  S  e  S
        |        |
      iCtS       a
       |
       a
```

Both derivations produce the SAME STRING "iCtiCtaea" but represent DIFFERENT parse trees (the "else" attaches to a different "if") ⟹ **G is AMBIGUOUS**. ∎

This is the famous "dangling else" problem in programming language grammars (e.g., C, Pascal) — resolved in practice by a disambiguation rule (else binds to nearest unmatched if) rather than rewriting the grammar (though an unambiguous equivalent grammar CAN be constructed using "matched"/"unmatched" statement non-terminals).

---

## A. NUMERICAL — PDA Construction

### General PDA Design Strategy
- Push symbols for the "first half" of the pattern.
- Pop symbols (matching) for the "second half".
- Accept when stack returns to bottom marker (Z0) — via empty stack OR move to final state.

---

### PROBLEM 5: PDA for L = {aⁿbⁿ | n≥1}

**Idea**: Push 'a' for each 'a' read; pop one 'a' for each 'b' read; accept if stack empties exactly when input ends.

**States**: q0 (start, pushing a's), q1 (popping a's for b's), q2 (final/accept)

**Stack alphabet**: {A, Z0}

**δ (transition functions):**

| From | Input | Stack Top | To | Stack Action |
|------|-------|-----------|----|--------------|
| q0 | a | Z0 | q0 | push A → (A Z0) |
| q0 | a | A | q0 | push A → (A A) |
| q0 | b | A | q1 | pop A (no push) |
| q1 | b | A | q1 | pop A |
| q1 | ε | Z0 | q2 | no change (stack=Z0 only) |

F = {q2} (accept by final state with Z0 still on stack), OR alternatively accept by empty stack if we also pop Z0 in the last step.

**Trace for "aabb"**: 
(q0,aabb,Z0) →a (q0,abb,AZ0) →a (q0,bb,AAZ0) →b (q1,b,AZ0) →b (q1,ε,Z0) →ε (q2,ε,Z0) ✓ ACCEPT

---

### PROBLEM 6: PDA for L = {wcw^R | w∈{a,b}*}  (Odd-length palindrome with center marker 'c')

**Idea**: Push symbols of w while reading first half; on seeing 'c', switch to popping mode; pop and MATCH against w^R (second half) — for each input symbol, it must equal the popped stack symbol.

**States**: q0 (pushing phase), q1 (matching/popping phase), q2 (final)

**Stack alphabet**: {A, B, Z0} (A represents pushed 'a', B represents pushed 'b')

| From | Input | Stack Top | To | Stack Action |
|------|-------|-----------|----|--------------|
| q0 | a | X (any) | q0 | push A |
| q0 | b | X (any) | q0 | push B |
| q0 | c | X (any) | q1 | no stack change (just switch mode) |
| q1 | a | A | q1 | pop A |
| q1 | b | B | q1 | pop B |
| q1 | ε | Z0 | q2 | no change |

F={q2}. This PDA is DETERMINISTIC (DPDA) since the marker 'c' unambiguously signals the switch point — no guessing needed.

**Trace for "abcba"**: (q0,abcba,Z0)→a(q0,bcba,AZ0)→b(q0,cba,BAZ0)→c(q1,ba,BAZ0)→b(q1,a,AZ0)→a(q1,ε,Z0)→ε(q2,ε,Z0) ✓

---

### PROBLEM 7: PDA for L = {ww^R | w∈{a,b}*}  (Even-length palindrome, NO center marker)

**Idea**: WITHOUT a marker, the PDA cannot know WHERE the midpoint is — it must NON-DETERMINISTICALLY GUESS the midpoint, ε-transitioning from "push mode" to "pop mode" at any point (this guess only "succeeds" — leads to acceptance — at the TRUE midpoint).

**States**: q0 (push phase), q1 (pop/match phase), q2 (final)

**Stack alphabet**: {A,B,Z0}

| From | Input | Stack Top | To | Stack Action |
|------|-------|-----------|----|--------------|
| q0 | a | X | q0 | push A |
| q0 | b | X | q0 | push B |
| q0 | **ε** | X | q1 | no change — **NON-DETERMINISTIC GUESS that midpoint reached** |
| q1 | a | A | q1 | pop A |
| q1 | b | B | q1 | pop B |
| q1 | ε | Z0 | q2 | no change |

F={q2}. 

**Why NPDA required**: at each step during input, the PDA has a CHOICE — either continue pushing (treat current symbol as part of "first half") OR switch to popping mode via the ε-move (guess the midpoint was reached). Only the branch where the guess is made at the EXACT TRUE MIDPOINT will result in successful matching all the way down to Z0 and acceptance. Other branches will get "stuck" (mismatch — no valid transition — that branch dies). Since NFA-style automata accept if ANY branch accepts, this works. A DPDA CANNOT do this — it cannot "guess and backtrack"; hence {ww^R} (without a center marker) is a CFL but NOT a DCFL, requiring an NPDA.

**Trace for "abba"** (correct guess at midpoint, i.e., after "ab"): (q0,abba,Z0)→a(q0,bba,AZ0)→b(q0,ba,BAZ0)→ε[guess](q1,ba,BAZ0)→b(q1,a,AZ0)→a(q1,ε,Z0)→ε(q2,ε,Z0) ✓

---

### PROBLEM 8: PDA for L = {w | w has equal number of a's and b's}

**Idea**: Use the stack to track the "difference" between #a's and #b's seen so far.
- If stack has symbol 'A' on top, it means currently #a's > #b's (excess a's).
- If stack has symbol 'B' on top, it means #b's > #a's (excess b's).
- When counts are equal, stack = Z0 only.

**States**: single state q0 (start=final, since acceptance happens whenever stack=Z0 and... but PDA accepts only at END of input, so really we check at end)

Let F={q0} and acceptance happens when, after consuming ALL input, stack = Z0 (empty stack semantically, with just bottom marker).

**Stack alphabet**: {A,B,Z0}

| From | Input | Stack Top | To | Stack Action |
|------|-------|-----------|----|--------------|
| q0 | a | Z0 | q0 | push A → AZ0 |
| q0 | a | A | q0 | push A → AA... |
| q0 | a | B | q0 | pop B (cancel: one fewer 'b'-excess) |
| q0 | b | Z0 | q0 | push B → BZ0 |
| q0 | b | B | q0 | push B |
| q0 | b | A | q0 | pop A (cancel) |

F={q0} with acceptance condition = stack top is Z0 (i.e., stack=Z0 exactly) at end of input.

**Trace for "aabb"**: (q0,aabb,Z0)→a(q0,abb,AZ0)→a(q0,bb,AAZ0)→b(q0,b,AZ0)→b(q0,ε,Z0) — stack=Z0 ✓ ACCEPT

**Trace for "aab"** (unequal: 2 a's,1 b): (q0,aab,Z0)→a(q0,ab,AZ0)→a(q0,b,AAZ0)→b(q0,ε,AZ0) — stack=AZ0≠Z0 ✗ REJECT

---

### PROBLEM 9: PDA for L = {aⁿb^(2n) | n≥1}

**Idea**: For each 'a', push TWO symbols (or push one symbol but pop TWO per matching mechanism) — simplest: push 2 copies of 'A' for each 'a' read; pop 1 'A' for each 'b' read. This way, after n a's, stack has 2n A's; after 2n b's, stack empties.

**States**: q0 (pushing), q1 (popping), q2(final)

**Stack alphabet**: {A, Z0}

| From | Input | Stack Top | To | Stack Action |
|------|-------|-----------|----|--------------|
| q0 | a | Z0 | q0 | push AA → AAZ0 |
| q0 | a | A | q0 | push AA → AAA...(2 more) |
| q0 | b | A | q1 | pop A |
| q1 | b | A | q1 | pop A |
| q1 | ε | Z0 | q2 | no change |

F={q2}.

**Trace for "abb"** (n=1, expect b^2): (q0,abb,Z0)→a(q0,bb,AAZ0)→b(q1,b,AZ0)→b(q1,ε,Z0)→ε(q2,ε,Z0) ✓

(Note: "push AA" in one move means δ(q0,a,Z0)={(q0,AAZ0)} — pushing a string of length 2 onto the stack in a single transition, which standard PDA transition functions allow since δ:Q×Σ×Γ→2^(Q×Γ*).)

---

## A. NUMERICAL — Pumping Lemma for CFL (Non-CFL Proofs)

### PROBLEM 10: Prove languages are NOT context-free

**Pumping Lemma for CFL (Statement)**: If L is a CFL, there exists n such that any string z∈L with |z|≥n can be written z=uvwxy where:
1. |vwx| ≤ n
2. |vx| ≥ 1 (v and x not both empty)
3. For all i≥0, uvⁱwxⁱy ∈ L

---

**(a) L = {aⁿbⁿcⁿ | n≥1}**

*Proof by contradiction:*
Assume L is a CFL, pumping length=n. Choose z = aⁿbⁿcⁿ ∈ L. |z|=3n≥n ✓.

By CFL-PL: z=uvwxy, |vwx|≤n, |vx|≥1, uvⁱwxⁱy∈L ∀i.

Since |vwx|≤n, the substring vwx CANNOT span all three blocks (aⁿ, bⁿ, cⁿ) — it can span AT MOST TWO adjacent blocks (since each block has length n, and |vwx|≤n means vwx fits within a "window" of size n, which can touch at most 2 block-boundaries... more precisely, vwx is entirely within ONE block, or straddles the boundary between two ADJACENT blocks).

**Case 1**: vwx lies entirely within the a's (or entirely within b's, or entirely within c's).
Then pumping (i=2, i.e., uv²wx²y) increases the count of THAT letter only, while the other two letters' counts stay at n. Result has unequal counts of a,b,c ⟹ not of form aᵏbᵏcᵏ ⟹ ∉L.

**Case 2**: vwx straddles boundary of two adjacent blocks (e.g., spans end of a's and start of b's).
Then v and/or x contain a MIX of two different letters (e.g., v=a^j, x=b^k or similar) — pumping (i=2) inserts extra copies of v and x. Since v,x are non-empty in mixed form, pumping disrupts the ORDER/uniformity — e.g., results in extra a's inserted in the middle of b-region or similar, producing a string NOT of the form a*b*c* (order violated) — definitely ∉ L.

**Case 3**: vwx entirely in one block but pumping down (i=0): uwy removes |vx|≥1 characters from that one block, reducing that letter's count below n while others remain n ⟹ unequal counts ⟹ ∉L.

In ALL cases, pumping produces a string ∉ L, contradicting the pumping lemma. Hence **L = {aⁿbⁿcⁿ} is NOT context-free**. ∎

---

**(b) L = {ww | w∈{a,b}\*}**

*Proof by contradiction:*
Assume L is CFL with pumping length n. Choose z = aⁿbaⁿb ∈ L? Let's verify: is aⁿbaⁿb of form ww? w would need to be a^n b, and ww=a^n b a^n b ✓. So z=a^n b a^n b ∈ L, |z|=2n+2≥n ✓.

By CFL-PL: z=uvwxy, |vwx|≤n, |vx|≥1, uvⁱwxⁱy∈L for all i.

Since |vwx|≤n, vwx lies within a window of length n. The string z = a^n b a^n b has the FIRST a^n occupying positions 1..n. So vwx (window of length≤n) is located somewhere — consider possible positions:

**Case A**: vwx entirely within the first a^n block (positions 1 to n).
Then v,x consist only of a's (v=a^j, x=a^k, j+k≥1). Pumping down (i=0): uwy = a^(n-j-k) b a^n b. For this to be ∈L (form ww), we'd need |uwy| even and first half=second half. |uwy|=2n+2-(j+k). For ww form with w'=first half: first half would be a^(n-j-k) b a^(something)... 

The first half of uwy (length (2n+2-(j+k))/2) and second half must match. But uwy = a^(n-j-k) b a^n b — the FIRST block of a's has length n-j-k while the SECOND block of a's (before the final b) has length n (unchanged, since vwx was entirely in first block). Since j+k≥1, n-j-k ≠ n, so the two a-blocks have different lengths ⟹ first half ≠ second half ⟹ uwy ∉ L (not of form ww).

CONTRADICTION (pumping lemma requires uwy∈L for i=0).

**Case B**: vwx spans the boundary including the middle 'b' or extends into second a^n block — similar contradictions arise: pumping disturbs the symmetry between the two halves (changes the length/content of one half but not corresponding part of the other half), breaking the ww structure.

**Case C**: vwx entirely within second half.
Symmetric to Case A — pumping changes second-half block lengths without matching first half ⟹ ∉L.

In all cases, contradiction arises. Hence **L = {ww | w∈{a,b}\*} is NOT context-free**. ∎
# MODULE 5: Turing Machines & Computability

## B. THEORY (Short Answer)

### 1. Formal Definition of Turing Machine (7-tuple)

**M = (Q, Σ, Γ, δ, q0, B, F)**

- Q = finite set of states
- Σ = input alphabet (Σ ⊆ Γ, B∉Σ)
- Γ = tape alphabet (Σ ⊆ Γ)
- δ: Q × Γ → Q × Γ × {L,R} — transition function: given current state and symbol under tape head, returns (new state, symbol to write, direction to move head — Left or Right)
- q0 ∈ Q = initial state
- B ∈ Γ = blank symbol (B∉Σ; fills the infinite tape outside the input)
- F ⊆ Q = set of final/accepting states

**Tape head movement**: At each step, the TM reads the symbol under the head, consults δ(current_state, symbol_read) = (new_state, symbol_to_write, direction). It WRITES symbol_to_write at the current position (overwriting), changes to new_state, then MOVES the head one cell LEFT (L) or RIGHT (R). The tape is infinite in (at least) one direction (often considered infinite both directions, or one-way infinite with a left-end marker).

A configuration is described as (state, tape contents, head position) — often written as a string with the state inserted at the head position, e.g., x q y means tape=xy, state=q, head pointing at first symbol of y.

---

### 2. Halting Problem — Statement and Undecidability Proof

**Statement**: The Halting Problem asks: "Given a Turing Machine M and an input w, does M halt (eventually stop, either accepting or rejecting) when run on w?" 

**Formally**: HALT = {⟨M,w⟩ | M is a TM that halts on input w}

**Theorem**: HALT is UNDECIDABLE — no algorithm (TM) can decide HALT for ALL (M,w) pairs.

**Proof (by Diagonalization / Contradiction):**

Assume, for contradiction, that HALT IS decidable. Then there exists a TM **H** such that:
- H(⟨M,w⟩) = "yes" (accept) if M halts on w
- H(⟨M,w⟩) = "no" (accept, different output) if M does NOT halt on w
- H ALWAYS HALTS (since H decides HALT, H itself must be a total/halting algorithm)

Now construct a new TM **D** ("Diagonalizer") that takes as input a TM description ⟨M⟩, and does the following:
1. Run H(⟨M,⟨M⟩⟩) — i.e., ask "does M halt when given its OWN description as input?"
2. If H says "M halts on ⟨M⟩" → D enters an INFINITE LOOP (never halts).
3. If H says "M does NOT halt on ⟨M⟩" → D HALTS (immediately, e.g., accept).

D is well-defined (since H always halts by assumption, step 1 always completes).

Now run D on its OWN description: **D(⟨D⟩)**.

- Case 1: Suppose D(⟨D⟩) HALTS. Then by D's logic (step 1), H(⟨D,⟨D⟩⟩) must have said "D does NOT halt on ⟨D⟩" (because that's the only branch — step 3 — where D halts). But this means H incorrectly reported that D does not halt on ⟨D⟩, while we just assumed/observed D(⟨D⟩) DOES halt — CONTRADICTION (H is supposed to be CORRECT).

- Case 2: Suppose D(⟨D⟩) does NOT HALT (loops forever). Then by D's logic (step 2), H(⟨D,⟨D⟩⟩) must have said "D halts on ⟨D⟩" (that's the only branch — step 2 — leading to infinite loop). But this means H incorrectly reported that D halts on ⟨D⟩, while D(⟨D⟩) actually does NOT halt — CONTRADICTION.

In BOTH cases, we reach a contradiction. Therefore, our initial assumption — that H exists (i.e., HALT is decidable) — must be FALSE.

**∴ HALT is UNDECIDABLE.** ∎

---

### 3. Church-Turing Hypothesis (Church's Thesis)

**Statement**: "Any function that can be computed by an algorithm (in the intuitive, informal sense — i.e., by any effective, mechanical procedure) can also be computed by a Turing Machine."

Equivalently: The class of "intuitively computable" functions = the class of "Turing-computable" functions = the class of "λ-calculus computable" functions = "recursive functions" (Church showed λ-calculus, Turing showed TMs, and these and other formalisms like Post systems, register machines, etc., all turn out to be EQUIVALENT in power).

**Importance/Nature**: 
- It is a HYPOTHESIS/THESIS, NOT a theorem — it CANNOT be formally proven, because "algorithm" / "effective procedure" is an intuitive, informal notion not formally defined independently of a computational model. 
- It is widely ACCEPTED based on the fact that EVERY formal model of computation proposed so far (TMs, λ-calculus, recursive functions, RAM machines, cellular automata, etc.) has been proven EQUIVALENT in computational power to TMs.
- It serves as the foundation for arguing "this problem is/isn't algorithmically solvable" — if no TM can solve it, NO algorithm (in any reasonable sense) can.

---

### 4. Universal Turing Machine (UTM)

**Definition**: A Universal Turing Machine U is a SINGLE, FIXED Turing Machine that can SIMULATE the behavior of ANY OTHER Turing Machine M on any input w, when given as input an ENCODING of M (denoted ⟨M⟩) together with the input w, i.e., U(⟨M⟩, w) simulates M(w) — produces the same output / accepts iff M accepts w / halts iff M halts on w, etc.

**Difference from a standard TM**:
- A "standard" TM M is designed/hardwired for ONE SPECIFIC TASK / language (its δ, Q etc. are fixed for that purpose).
- A UTM is a SINGLE TM whose "program" is part of its INPUT — it reads the description ⟨M⟩ of another machine (encoded as a string on the tape) and the input w, and then INTERPRETS/SIMULATES M step-by-step on w, using its own tape to keep track of M's tape, state, and head position.
- The UTM is analogous to a general-purpose COMPUTER (which runs any program given to it as data), whereas a standard TM is analogous to a special-purpose, HARDWIRED circuit for one function.
- The existence of a UTM is the theoretical basis for STORED-PROGRAM computers — the idea that "data" and "program" can be represented in the same way (as strings/encodings) and processed by the same machine.

---

### 5. Variants of Turing Machines

**(a) Multi-tape TM**: Has k≥1 tapes, each with its own independent head. In one move, the TM reads symbols under ALL k heads, and based on (state, k symbols read), writes k symbols (one per tape) and moves each head independently (L/R/stay).

**(b) Multi-track TM**: Has a SINGLE tape, but each cell holds a TUPLE of k symbols (k "tracks") — conceptually like k tapes glued together at each cell, with only ONE head moving across all tracks simultaneously.

**(c) Non-deterministic TM (NTM)**: δ: Q×Γ → 2^(Q×Γ×{L,R}) — multiple possible (state,symbol,direction) choices at each step; NTM accepts w if SOME computation path leads to acceptance.

**Theorem (Multi-tape ≡ Single-tape in power)**:

**Statement**: For every k-tape TM M_k, there exists an equivalent single-tape TM M_1 such that L(M_1)=L(M_k) (same language recognized) — though M_1 may be SLOWER (polynomial-factor simulation overhead, NOT a power difference).

**Proof sketch (Simulation)**:
- M_1 uses a single tape divided into 2k "tracks" (using multi-track technique) — k tracks to represent the contents of M_k's k tapes, and k additional tracks to mark the CURRENT HEAD POSITION of each of M_k's tapes (using a special marker symbol, e.g., a "dot" placed above the cell where each virtual head currently is).
- To simulate ONE MOVE of M_k:
  1. M_1 scans LEFT to RIGHT across its entire used tape, recording (in its finite control / state) the symbols found at each of the k marked head-positions.
  2. Once all k symbols are collected, M_1 (using its state, which encodes M_k's current state PLUS the k symbols read) determines what M_k WOULD do: new state, k symbols to write, k head-movement directions.
  3. M_1 scans again LEFT to RIGHT (or makes another pass), updating the symbol at each marked position and shifting each "head marker" left/right as dictated.
- M_1 simulates each single move of M_k using O(tape length used) moves of its own — POLYNOMIAL slowdown but computationally EQUIVALENT (same set of strings accepted, same set of functions computable).

Since M_1 can simulate M_k step-by-step and reaches an accepting configuration iff M_k does, **L(M_1) = L(M_k)**. Hence multi-tape TMs and single-tape TMs are EQUIVALENT IN POWER (recognize exactly the same class of languages — recursively enumerable languages). ∎

(Similarly, non-deterministic TMs are equivalent in power — though not necessarily efficiency — to deterministic TMs, via a simulation that explores the NTM's computation tree breadth-first using a multi-tape/single-tape deterministic TM.)

---

### 6. Computable Functions

**Definition**: A function f: Σ* → Σ* (or f:ℕ→ℕ etc.) is **computable** (also called **Turing-computable** or **recursive**) if there exists a Turing Machine M such that, for every input x in the domain of f:
- M halts on input x, with the tape containing f(x) (in some agreed encoding) when M halts.

If x is NOT in the domain of f, M either halts with a special "undefined" output, OR M may run forever (never halts) — depending on whether f is total or partial.

- **Total computable function**: M halts on EVERY input (domain = all of Σ*).
- **Partial computable function**: M may not halt on some inputs (those inputs are outside f's domain).

**Example**: 
f(n) = n+1 (successor function) is computable — a TM can be designed that, given a unary or binary representation of n on the tape, scans to find the end, and increments it (e.g., for unary: just append one more '1' symbol).

Another example: f(x,y) = x+y (addition of two numbers in unary, separated by a marker like '0' or 'c') — TM scans, replaces the separator with the symbol used for the numbers (effectively concatenating the two unary blocks), trims one extra symbol if needed, and halts — computable.

---

### 7. Counter Machine

**Definition**: A Counter Machine is a computational model consisting of:
- A finite set of states (finite control), similar to a TM/automaton.
- A finite number of COUNTERS (registers), each holding a NON-NEGATIVE INTEGER.
- A finite set of instructions, each of which can: 
  - INCREMENT a counter by 1,
  - DECREMENT a counter by 1 (only if counter > 0; some models test counter=0 and branch accordingly),
  - TEST whether a counter = 0 (conditional branch),
  - and based on these, transition between states.

Essentially, a Counter Machine = finite automaton + a finite number of unbounded integer counters (instead of a tape/stack).

**Power**: A Counter Machine with 2 OR MORE counters is EQUIVALENT IN POWER to a Turing Machine (Turing-complete) — it can simulate any TM (counters can encode tape contents via Gödel-numbering / prime-power encoding techniques). A Counter Machine with only 1 counter is STRICTLY WEAKER than a TM (equivalent to a restricted class, less than context-free in some formulations) — cannot simulate a general TM.

Counter machines are often used as a simpler/cleaner intermediate model when proving Turing-completeness of other systems (e.g., showing some programming construct or cellular automaton is Turing-complete by simulating a 2-counter machine within it).

---

## A. NUMERICAL — TM Construction Problems

### General TM Design Conventions
- Tape alphabet typically Γ={0,1,B,X,Y,...} where B=blank, and X,Y,etc. are "marker" symbols used to cross off/match input symbols.
- State naming convention: q0=start. Transitions written as δ(state,symbol)=(new_state,write_symbol,direction).

---

### PROBLEM 1: TM for L = {0ⁿ1ⁿ | n≥1}

**Strategy**: Repeatedly find the LEFTMOST unmarked '0', change it to 'X'; find the LEFTMOST unmarked '1', change it to 'Y'; repeat. If at some point we look for a '0' but find none unmarked while there's still an unmarked '1' (or vice versa), or if counts don't match up — reject. Accept when all 0's and 1's are marked (X's and Y's) and tape is "XXX...YYY...B".

**State table**:

| State | Symbol | Write | Move | New State | Meaning |
|-------|--------|-------|------|-----------|---------|
| q0 | 0 | X | R | q1 | mark a 0 as X, go find matching 1 |
| q0 | Y | Y | R | q3 | all 0's done (only Y's/B left), check rest are Y/B |
| q0 | B | B | – | q_reject | empty string — n≥1 required, reject ε |
| q1 | 0 | 0 | R | q1 | skip over unmarked 0's |
| q1 | Y | Y | R | q1 | skip over already-marked Y's |
| q1 | 1 | Y | L | q2 | found matching 1, mark as Y, go back |
| q1 | B | B | – | q_reject | ran out — more 0's than 1's |
| q2 | 0 | 0 | L | q2 | move left back over 0's |
| q2 | X | X | R | q0 | found left boundary marker, go find next 0 |
| q2 | Y | Y | L | q2 | move left back over Y's (shouldn't normally happen here but safe) |
| q3 | Y | Y | R | q3 | skip remaining Y's |
| q3 | B | B | – | q_accept | all matched, end of tape — ACCEPT |
| q3 | 0 or 1 | – | – | q_reject | leftover unmatched symbol — reject |

**Trace for "0011"**:
```
q0: X011  (marked first 0 as X, R)
q1: X011 → X011(skip 0,R) → X011(at Y? no)... 
```
Let's trace properly: tape=0011, head at pos1.

- (q0,0) at pos1: write X, R, →q1. Tape: X011, head pos2.
- (q1,0) at pos2: write 0(no change),R,→q1. Tape:X011,head pos3.
- (q1,1) at pos3: write Y,L,→q2. Tape:X0Y1,head pos2.
- (q2,0) at pos2: write0,L,→q2. Tape:X0Y1,head pos1.
- (q2,X) at pos1: write X,R,→q0. Tape:X0Y1,head pos2.
- (q0,0) at pos2: write X,R,→q1. Tape:XXY1,head pos3.
- (q1,Y) at pos3: writeY,R,→q1. Tape:XXY1,head pos4.
- (q1,1) at pos4: writeY,L,→q2. Tape:XXYY,head pos3.
- (q2,Y) at pos3: hmm — q2 on Y → L → q2. head pos2.
- (q2,X) at pos2: wait pos2 is X. (q2,X)→write X,R,→q0. head pos3.
- (q0,Y) at pos3: write Y,R,→q3. head pos4.
- (q3,Y) at pos4: writeY,R,→q3. head pos5 (blank).
- (q3,B): → q_accept. ✓ ACCEPT

---

### PROBLEM 2: TM for L = {aⁿbⁿcⁿ | n≥1}  (Most frequently asked)

**Strategy**: Same "cross-off" technique extended to three symbol types. Repeatedly: mark one unmarked 'a' as 'X', skip to find one unmarked 'b', mark as 'Y', skip to find one unmarked 'c', mark as 'Z'. Repeat until all a's exhausted; then verify no leftover unmarked b's or c's remain.

**Tape alphabet**: {a,b,c,X,Y,Z,B}

**High-level state table**:

| State | Symbol | Write | Move | New State | Purpose |
|-------|--------|-------|------|-----------|---------|
| q0 | a | X | R | q1 | mark an 'a', start searching for 'b' |
| q0 | Y | Y | R | q5 | all a's done — go verify rest |
| q0 | B | B | – | reject | (handles n=0 case if needed) |
| q1 | a | a | R | q1 | skip remaining unmarked a's |
| q1 | Y | Y | R | q1 | skip already-marked b's (Y) |
| q1 | b | Y | R | q2 | mark a 'b', go find a 'c' |
| q1 | B/c | – | – | reject | no 'b' available — mismatch |
| q2 | b | b | R | q2 | skip unmarked b's |
| q2 | Z | Z | R | q2 | skip already-marked c's |
| q2 | c | Z | L | q3 | mark a 'c', go back to start |
| q2 | B | – | – | reject | no 'c' available |
| q3 | (any of a,b,c,X,Y,Z) | same | L | q3 | move all the way back left |
| q3 | B | B | R | q0 | reached left end, restart cycle |
| q5 | Y | Y | R | q5 | skip marked b's (Y) |
| q5 | Z | Z | R | q6 | found start of c's region |
| q5 | b/c-unmarked | – | – | reject | leftover unmarked — mismatch |
| q6 | Z | Z | R | q6 | skip marked c's |
| q6 | B | – | – | ACCEPT | all matched — accept |
| q6 | (a/b/c) | – | – | reject | leftover unmarked |

**Trace summary for "aabbcc"** (n=2):
- Cycle 1: mark 1st a→X, find 1st b→Y, find 1st c→Z. Tape becomes: XaYbZc (conceptually, exact positions tracked)
- Cycle 2: mark 2nd a→X, find 2nd b→Y, find 2nd c→Z. Tape: XXYYZZ
- q0 now sees Y (no more a's) → q5: skip Y's → see Z → q6: skip Z's → see B → ACCEPT ✓

---

### PROBLEM 3: TM for Palindrome Recognizer — L = {ww^R | w∈{a,b}*}

**Strategy**: Compare first and last symbols; if they match, mark both with X (or blank/erase), move inward; repeat. If mismatch at any point → reject. If they "meet in the middle" (or cross over) with all matches → accept.

**State table**:

| State | Symbol | Write | Move | New State | Purpose |
|-------|--------|-------|------|-----------|---------|
| q0 | a | X | R | q1a | remember first symbol was 'a', go to right end |
| q0 | b | X | R | q1b | remember first symbol was 'b' |
| q0 | X | X | R | q0 | (already-marked symbols at edges — skip) |
| q0 | B | B | – | ACCEPT | empty or fully matched — accept |
| q1a | a/b | same | R | q1a | move right, skip interior symbols |
| q1a | X | X | L | q2a | reached right marked-boundary, step back |
| q1a | B | B | L | q2a | reached blank (right end), step back |
| q2a | a | X | L | q3 | last symbol is 'a' — MATCHES first 'a' → mark, go left |
| q2a | b | – | – | reject | mismatch ('a' vs 'b') |
| q2a | X | X | – | ACCEPT | crossed over — even-length palindrome fully matched |
| (similarly q1b,q2b for first-symbol='b' case, mirroring above with roles of a,b swapped) |
| q3 | a/b | same | L | q3 | move left, skip interior |
| q3 | X | X | R | q0 | reached left marked-boundary, step forward to start next round |

**Trace for "abba"**:
- q0 sees 'a' (pos1) → write X, R → q1a. Tape: Xbba. Move right through b,b to reach 'a' at pos4.
- At pos4 ('a'), q1a→ reads 'a': move R→ now at B (pos5), q1a on B → write B,L→q2a, back to pos4.
- q2a at pos4 sees 'a' → matches first symbol 'a' → write X, L → q3. Tape: Xbb X.
- q3 moves left through 'b','b' until hits 'X' at pos1 → q3 on X → R → q0, now at pos2.
- q0 at pos2 sees 'b' → write X,R→q1b. Tape: XXbX→ wait tape is XXbX now? let's just trust the pattern: continues similarly, matches 'b' with 'b', eventually all become X, q0 sees B → ACCEPT. ✓

---

### PROBLEM 4: TM for 1's Complement and 2's Complement of Binary Number

**1's Complement**: Simply FLIP every bit (0→1, 1→0). 

**State table** (single pass):

| State | Symbol | Write | Move | New State |
|-------|--------|-------|------|-----------|
| →q0 | 0 | 1 | R | q0 |
| q0 | 1 | 0 | R | q0 |
| q0 | B | B | – | ACCEPT (halt) |

Trivial single-state TM: scans left to right, flips each bit, halts on blank.

**2's Complement**: 2's complement = 1's complement + 1. Equivalently: scan from the RIGHT end; copy bits unchanged until the FIRST '1' is found (copy that '1' unchanged too — this is the "add 1" carry-stop point); after that first '1' (from the right), FLIP all remaining bits to its left.

**Strategy**:
1. Move head to the RIGHTMOST symbol of the binary number.
2. Scan RIGHT-TO-LEFT: copy 0's as 0's (these become 1's after... wait let's redo carefully).

Correct rule for 2's complement via single pass from the right: "Copy all trailing 0's unchanged, copy the first 1 encountered (from the right) unchanged, then FLIP all remaining bits to the left of that first 1."

**State table**:

| State | Symbol | Write | Move | New State | Purpose |
|-------|--------|-------|------|-----------|---------|
| →q0 | 0/1 | same | R | q0 | move to rightmost symbol |
| q0 | B | B | L | q1 | reached right end, step back onto last symbol |
| q1 | 0 | 0 | L | q1 | trailing 0 — copy unchanged, continue left |
| q1 | 1 | 1 | L | q2 | found first '1' (from right) — copy unchanged, switch to FLIP mode |
| q2 | 0 | 1 | L | q2 | flip 0→1 |
| q2 | 1 | 0 | L | q2 | flip 1→0 |
| q2 | B | B | – | ACCEPT | reached left end — done |

**Example**: Input "1100" (=12). 2's complement (4-bit) should be "0100" (=4, since 16-12=4).
- Scan right to left: last bit='0'(trailing, q1, copy unchanged)→ next bit '0' (trailing, copy unchanged) → next bit '1' (first 1 from right! copy unchanged, switch to q2) → next bit '1' (q2, FLIP→0).
- Result: 0 1 0 0 = "0100" ✓ Correct!

---

### PROBLEM 5: TM to Add Two Unary Numbers (e.g., 111+11=11111)

**Representation**: Two unary numbers separated by a special symbol (e.g., '+' or '0' as separator), e.g., input = "111+11" (representing 3+2), each number as a block of 1's.

**Strategy**: Simplest method — replace the separator '+' with '1' (effectively concatenating the two blocks of 1's into one), then erase ONE '1' from the end (to compensate, since m 1's + separator + n 1's, if separator becomes '1', gives (m+n+1) ones — we want exactly (m+n) ones, so remove one extra '1').

**State table**:

| State | Symbol | Write | Move | New State | Purpose |
|-------|--------|-------|------|-----------|---------|
| →q0 | 1 | 1 | R | q0 | skip first block of 1's |
| q0 | + | 1 | R | q1 | replace '+' with '1' (merge the blocks), move into second block |
| q1 | 1 | 1 | R | q1 | skip second block of 1's |
| q1 | B | B | L | q2 | reached end (blank), step back onto last 1 |
| q2 | 1 | B | L | q3 | erase the LAST '1' (compensate for the extra merged symbol) |
| q3 | 1 | 1 | L | q3 | (optional) move back to start, or just halt here |
| q3 | B | B | R | ACCEPT | done — head returns to start, halt/accept |

**Trace for "111+11"** (3+2=5, expect output "11111"):
- q0 scans through "111" (three 1's, R each time) → at '+': write '1', R → q1. Tape becomes "1111" + "11" = "111111" (the '+' became '1'). Now at first '1' of what was the second block.
- q1 scans through remaining "11" (two 1's) → at B: write B, L → q2. Now at last '1' of tape "111111".
- q2 at last '1': write B (erase it), L → q3. Tape now "11111" + B (5 ones) ✓ — correct! 3+2=5.
- q3 scans left back to start (optional cleanup), then ACCEPT.

**Result**: Output tape = "11111" (5 ones) = 3+2 ✓ Correct!
