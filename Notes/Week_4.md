# Week 04 – Secure Implementation and Typing

---

## Lazy Intruder Recap

- Without lazy intruder:
  - infinite search tree
  - intruder has infinite choices of messages
- Lazy intruder approach:
  - Layer 1: symbolic search tree
  - Layer 2: constraint solving
- Intruder reasoning:
  - analyze incoming messages (decrypt if possible)
  - generate outgoing messages using:
    - Axiom (reuse known messages)
    - Composition (build from subterms)
- Goal state:
  - all remaining messages are variables
  - intruder can instantiate them arbitrarily

---

## Constraint Solving

- Incoming messages:
  - decrypt if key known
  - add plaintext to knowledge
- Outgoing messages:
  - Axiom:
    - reuse known message via unification
  - Composition:
    - break message into subterms
- Multiple choices:
  - must explore all (backtracking)

---

## Example Insight

- Decryption reveals nested structure
- Signatures reveal inner content
- Outgoing encrypted message:
  - reduce into key + payload
- Two solutions:
  - normal run (intruder plays role)
  - attack (intruder learns key)

---

## Common Protocol Mistake

- Protocol:
  - {{KAB}inv(pk(A))}pk(B)
- Problem:
  - missing recipient identity
- Result:
  - A and B disagree on who shares key
  - intruder learns key

---

## Fix: Identity Binding

- Add recipient identity:
  - {{B, KAB}inv(pk(A))}pk(B)
- Removes ambiguity
- Prevents attack

---

## Alternative: Encrypt then Sign

- {{KAB}pk(B)}inv(pk(A))
- Prevents attack
- Bad practice:
  - signing encrypted data is discouraged

---

## Environment Problem

- Two protocols interact
- Messages reused across contexts
- Leads to confusion
- Causes type-flaw attack

---

## Type-Flaw Attacks

- Message interpreted as wrong type
- Example:
  - tuple treated as key
- Caused by lack of structure

---

## Otway–Rees Protocol

- Symmetric-key protocol with server
- Typed version:
  - secure
- Untyped version:
  - vulnerable

---

## Type-Flaw Issue

- (M, A, B) same structure as KAB
- Intruder replays message
- Receiver accepts wrong value as key

---

## Message Structuring

- Use formats:
  - f1(...)
  - f2(...)
- Represent structured data
- Prevent confusion

---

## Concrete vs Abstract

- Concrete:
  - strings, JSON, TLS
- Abstract:
  - symbolic terms
- Parser/generator connect both

---

## Crypto API

- Encryption/decryption functions
- Must satisfy correctness:
  - decrypt(encrypt(m)) = m
- Intruder limited to API operations

---

## Non-Crypto API

- Parsing/generation functions
- Requirements:
  - unambiguous
  - reversible
  - disjoint formats

---

## Soundness

- No ambiguity in parsing
- No overlap between formats
- Ensures attacker cannot exploit encoding

---

## Formats in Protocols

- Define:
  - f1(N, M, A, B)
  - f2(N, K)
- Intruder can:
  - build and decompose formats
- Cannot:
  - confuse different formats

---

## Ill-Typed Messages

- Intruder may construct wrong types
- Example:
  - f1(a, b, i, b)
- Still accepted
- But not confused with other formats

---

## Sub-Message Patterns

- Include:
  - all messages
  - all subterms
  - inverse keys
- Variables renamed uniquely

---

## Type-Flaw Resistance

- Any unifiable messages must have same type
- Prevents cross-type confusion

---

## Example

- With formats:
  - only same-type messages unify
- Without formats:
  - incorrect unification possible

---

## Typing Theorem

- If protocol is type-flaw resistant:
  - every attack has well-typed version
- Can restrict intruder to well-typed messages

---

## Proof Idea

- Ill-typed substitutions impossible
- Lazy intruder avoids bad substitutions
- All attacks remain well-typed

---

## Implementation Guidelines

- Use standard crypto libraries
- Define clear formats
- Ensure:
  - unambiguous parsing
  - disjoint message formats
- Avoid raw data structures
- Verify with OFMC
