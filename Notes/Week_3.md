# Week 03 – Symbolic Analysis: The Lazy Intruder

---

## Dolev–Yao Deduction System

- Intruder knowledge is a finite set M
- Deduction M ⊢ m defines derivable messages
- Rules:
  - Axiom: m ∈ M
  - Composition: apply public functions
  - Projection: split pairs
  - Decryption: requires key
  - Signature opening: requires public key
- Cryptography is perfect

---

## Automating Deduction

- Input: M and target m
- Output: whether m is derivable
- Must terminate despite infinite possibilities

---

## Composition-Only

- Only Axiom + Composition
- Solve by backward search
- Break goal into subterms
- Terminates since terms shrink

---

## Analysis Steps

- Decrypt if key known
- Split pairs
- Add subterms to knowledge
- Repeat until closure

---

## Full Procedure

- Apply analysis
- Then composition-only
- Guarantees:
  - soundness
  - completeness
  - termination

---

## NSPK Protocol

- A → B: {NA, A}pk(B)
- B → A: {NA, NB}pk(A)
- A → B: {NB}pk(B)
- Goal: h(NA, NB) secret

---

## Strands

- Role = abstract behavior
- Strand = concrete execution
- Many strands run in parallel

---

## Attacks

- Intruder intercepts all messages
- Constructs messages using deduction
- Attack if goal is violated

---

## Infinite State Problem

- Infinite sessions
- Infinite message space
- Cannot brute-force explore

---

## Lazy Intruder

- Use symbolic messages with variables
- Delay choices
- Represent attack as constraints
- Solve via backward search

---

## Producing Messages

- Axiom: already known
- Composition: reduce to subterms
- Analysis: decrypt/split

---

## Lazy Variables

- Do not instantiate immediately
- Only assign when necessary

---

## Backtracking

- Try alternatives when failing
- Needed to explore all possibilities

---

## Unification

- Match messages via substitution
- Example:
  - NA = n1
  - NB = n2

---

## Attack Completion

- Intruder learns n1, n2
- Can compute h(n1, n2)
- Secrecy broken

---

## Lowe Attack

- Intruder relays between sessions
- Breaks authentication

---

## NSL Fix

- Add B identity to message
- Prevents attack

---

## Architecture

- Layer 1: symbolic search
- Layer 2: constraint solving

---

## Constraint Solving

- Incoming: decrypt if possible
- Outgoing:
  - variable → delay
  - otherwise:
    - try composition
    - try axiom
- Use backtracking

---

## Complexity

- Problem is NP-complete
