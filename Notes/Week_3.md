# Week 03 – Symbolic Analysis: The Lazy Intruder

---

## Dolev–Yao Recap

- Intruder knowledge represented as a set of messages M
- Deduction relation M ⊢ m defines what the intruder can derive
- Rules:
  - Axiom: use known messages
  - Composition: apply public functions
  - Decomposition: extract parts of messages
  - Decryption: only with known keys
- Deduction space is infinite due to repeated composition

---

## Decision Procedure (Recap)

- Goal: decide whether M ⊢ m
- Approach:
  - First perform analysis (decomposition)
  - Then apply composition-only reasoning
- Guarantees:
  - soundness
  - completeness
  - termination
- Negative results are meaningful: if procedure returns “no”, no derivation exists

---

## Needham–Schroeder Public-Key Protocol (NSPK)

- Classical authentication protocol (1978)
- Uses public-key encryption
- Each party contributes a nonce

### Message Flow

- A → B: encrypted message containing NA and identity of A
- B → A: encrypted message containing NA and fresh NB
- A → B: encrypted message containing NB

### Goal

- Shared secret derived from NA and NB
- Intended to ensure authentication between A and B

---

## Roles and Strands

- Each protocol role is a sequence of send/receive actions
- A strand is a concrete execution of a role
- All variables are instantiated in a strand
- Execution may be partial
- Multiple strands may execute in parallel

---

## Attacks as Strand Spaces

- An attack is modeled as interacting strands
- Messages sent by honest agents are received by the intruder
- Messages received by honest agents are constructed by the intruder
- Intruder uses deduction rules to generate messages
- Successful completion violates a protocol goal

---

## Infinite State Space Problem

- Unbounded sessions → infinitely many strands
- Intruder has infinite choice of messages
- Even bounded sessions still lead to infinite branching
- Naive exploration is infeasible

---

## Lazy Intruder: Core Idea

- Avoid enumerating all possible messages
- Use symbolic representations instead
- Messages may contain variables
- Each state is a constraint problem
- Use backward search from attack goal

---

## Symbolic States

- Represent interaction sequences with variables
- Intruder messages are partially unspecified
- Exact values determined later
- Reduces infinite branching to constraint solving

---

## Intruder Playing a Role

- Intruder can instantiate protocol roles
- Must satisfy initial knowledge requirements
- Can simulate honest participants
- Used to check executability of roles

---

## Attack Scenario (NSPK)

- A communicates with intruder instead of B
- B communicates with A in a separate session
- Intruder relays messages between sessions
- Intruder attempts to derive shared key h(NA, NB)

---

## Choose and Check

- Represent intruder communication symbolically
- Track incoming and outgoing messages
- Check if intruder can derive required outputs
- Combine protocol execution with deduction reasoning

---

## Needham–Schroeder–Lowe Fix (NSL)

- Modification of NSPK
- Adds B’s identity to second message
- Prevents replay and man-in-the-middle attacks
- Ensures correct identity binding

---

## Lazy Intruder Architecture

- Layer 1: symbolic search tree
- Layer 2: constraint solving
- Separates protocol structure from attacker reasoning

---

## Constraint Solving

- Process constraints step-by-step
- Incoming messages:
  - try decryption
  - add derived messages to knowledge
- Outgoing messages:
  - variables handled lazily
  - otherwise apply:
    - composition
    - axiom
- Backtracking used for exploration

---

## Key Observations

- Lazy approach avoids infinite branching
- Symbolic reasoning replaces concrete enumeration
- Backward search reduces complexity
- Intruder behavior modeled precisely

---

## Complexity

- Protocol insecurity (bounded sessions) is NP-complete
- Attack = guessed symbolic trace + constraint solution
- Hardness via reduction from Boolean satisfiability
