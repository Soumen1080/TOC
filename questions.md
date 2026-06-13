Module 1: Finite Automata & Minimization
A. Long Answer / Numerical Problems (10-15 Marks)
•	 Convert given NFA to DFA. 
Standard patterns tested:
- NFA accepting strings ending in '01' or '101'.
- NFA with epsilon transitions (e-NFA) to DFA (e.g., transitions over {a,b,c} with null jumps). You MUST calculate e-closure for all states first.NFA to DFA Conversion (Subset Construction):
•	 Minimize a given 6-to-8 state DFA. 
Must know: How to draw the state distinguishability staircase table and find the 0-equivalent, 1-equivalent, and 2-equivalent sets.DFA Minimization (Table-Filling / Myhill-Nerode):
•	 Construct DFA for the following exact languages:
- L = {w | w has an even number of a's and odd number of b's}.
- L = {w | w does not contain the substring '110'}.
- L = {w | the decimal equivalent of the binary string w is divisible by 3} (or divisible by 5).
- L = {w | length of w is a multiple of 3}.
- L = {w | starts and ends with different symbols over {0,1}}.Direct DFA Design:
B. Short Answer / Theoretical Questions (2-5 Marks)
•	Formal definitions (5-tuples) of DFA, NFA, and e-NFA.
•	Prove that for every NFA, there exists an equivalent DFA. (Theorem proof).
•	Prove the equivalence of NFA with and without empty (epsilon) transitions.
•	State the Myhill-Nerode Theorem and explain its application.
•	What are the limitations of Finite State Machines (FSM)?
Module 2: Finite Automata with Output
A. Long Answer / Numerical Problems (10 Marks)
•	 Given a Moore machine state table, convert it to a Mealy machine. (Remember: Mealy has output on transitions, so duplicate the Moore outputs onto incoming edges).Moore to Mealy Conversion:
•	 Given a Mealy machine, convert it to a Moore machine. (Remember: State splitting is required if a state has incoming transitions with different outputs).Mealy to Moore Conversion:
•	 Given an incomplete Mealy machine (with 'don't care' outputs/transitions).
Steps you must show: 
1. Draw the Compatibility Graph.
2. Construct the Merger Table.
3. Find Maximal Compatibles.
4. Draw the minimized closed state table.Minimization of Incompletely Specified Machines (Toughest Type):
B. Short Answer / Theoretical Questions (2-5 Marks)
•	Difference between Moore and Mealy machines with block diagrams.
•	Define Equivalent states, Distinguishable states, and k-equivalence.
•	Explain Lossless and Lossy Machines with the help of a Testing Table and Testing Graph.
Module 3: Regular Expressions & Languages
A. Long Answer / Numerical Problems (10-15 Marks)
•	 Given a DFA, write the algebraic equations for each state (e.g., q1 = q0.0 + q1.1) and solve using Arden's Lemma (R = Q + RP => R = QP*) to find the final Regular Expression.FA to Regular Expression (Arden's Theorem):
•	 Draw the NFA (with epsilon transitions) for the following exact expressions:
- (0+1)*1(0+1)
- 10 + (0+11)0*1
- (a+b)*abb
- (00+11)*((01+10)(00+11)*(01+10)(00+11)*)* (Even 0s, Even 1s - highly repeated).Regular Expression to NFA (Thompson's Construction):
•	 Prove the following languages are NOT regular:
- L = {a^n b^n | n >= 0}
- L = {a^p | p is a prime number}
- L = {ww^R | w is a string of 0s and 1s}
- L = {0^(n^2) | n >= 1}Pumping Lemma for Regular Languages (Non-Regularity Proofs):
B. Short Answer / Theoretical Questions (2-5 Marks)
•	State and mathematically prove Arden's Theorem.
•	State the Pumping Lemma for Regular Sets.
•	Write the algebraic rules/identities for Regular Expressions (e.g., (r*)* = r*, r+e = r).
•	Prove the closure properties of regular sets (Union, Intersection, Complement, Kleene Closure).
Module 4: CFG & Pushdown Automata (Largest Weightage)
A. Long Answer / Numerical Problems (15 Marks)
•	 Given a grammar, perform:
1. Removal of Null (epsilon) productions.
2. Removal of Unit productions (A -> B).
3. Removal of Useless symbols/productions.CFG Simplification (3-Step Algorithm):
•	 Convert a simplified CFG into CNF (All productions must be A -> BC or A -> a). Highly expected numerical.Chomsky Normal Form (CNF):
•	 Convert CFG to GNF (A -> aV*). (Less frequent than CNF, but must know the Left Recursion elimination step).Greibach Normal Form (GNF):
•	 Prove the following grammars are ambiguous by drawing two different parse trees (Leftmost & Rightmost derivations) for the same string:
- E -> E+E | E*E | (E) | id
- S -> aSbS | bSaS | epsilon
- S -> iCtS | iCtSeS | a (Dangling Else problem).Ambiguity Proofs:
•	 Construct PDA (draw transition diagram and write delta functions) for:
- L = {a^n b^n | n >= 1}
- L = {w c w^R | w belongs to {a,b}*} (Odd Palindrome)
- L = {w w^R | w belongs to {a,b}*} (Even Palindrome - requires Non-Deterministic PDA)
- L = {w | w has an equal number of a's and b's}
- L = {a^n b^(2n) | n >= 1}Pushdown Automata (PDA) Design:
•	 Prove the following are NOT context-free:
- L = {a^n b^n c^n | n >= 1}
- L = {ww | w belongs to {a,b}*}Pumping Lemma for CFL (Non-CFL Proofs):
B. Short Answer / Theoretical Questions (2-5 Marks)
•	Formal definition (7-tuple) of a Pushdown Automaton.
•	Differentiate between DPDA (Deterministic) and NPDA (Non-Deterministic).
•	Explain Acceptance by Final State vs. Acceptance by Empty Stack in PDA. Prove their equivalence.
•	State Ogden's Lemma and mention its application.
•	Define Right linear and Left linear grammars. Prove they generate regular languages.
Module 5: Turing Machines & Computability
A. Long Answer / Numerical Problems (10-15 Marks)
•	 Construct the TM (tape configuration, transition table, and state diagram) for:
- L = {0^n 1^n | n >= 1}
- L = {a^n b^n c^n | n >= 1} (Most frequently asked TM question).
- L = {w w^R | w is over {a,b}} (Palindrome recognizer).
- TM to compute 1's complement and 2's complement of a binary number.
- TM to add two unary numbers (e.g., 111 + 11 = 11111).Turing Machine (TM) Design:
B. Short Answer / Theoretical Questions (2-5 Marks)
•	Formal definition (7-tuple) of a Turing Machine. Explain the tape head movement.
•	The Halting Problem: State it and prove mathematically/logically that it is undecidable. (Guaranteed short note).
•	State and explain the Church-Turing Hypothesis (Church's Thesis).
•	What is a Universal Turing Machine (UTM)? How is it different from a standard TM?
•	Discuss the variants of Turing Machines (Multi-tape, Multi-track, Non-deterministic). Prove/state that a Multi-tape TM is equivalent in power to a Single-tape TM.
•	Define Computable functions and provide an example.
•	What is a Counter Machine?
