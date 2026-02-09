# Week 01 – Modeling Protocols: Alice & Bob

---

## Modeling Security Protocols

- Security protocols are studied using **mathematical abstraction**
- A protocol is modeled as a *game* with clearly defined rules
- “Winning” the game (breaking security) must be precisely defined
- Even with simple rules, the search space can be astronomically large
- Automated tools may outperform humans in exploring protocol behaviors
- The key challenge is to **mind the gap** between model and reality
- All abstractions and assumptions must be made explicit
- Separation of concerns:
  - cryptographic primitives
  - protocol logic
  - system and implementation details

---

## What Is Protocol Security About

- Understanding what constitutes an *attack*
- Distinguishing real attacks from irrelevant behavior
- Automatically finding attacks using formal tools
- Proving security against *all possible attacks*, not only known ones
- Building systems that remain secure under adversarial conditions
- Requires precise definitions of:
  - the protocol
  - the security goals
  - the intruder model

---

## Security Goals in Practice

- Typical security goals include:
  - Authentication / Integrity
  - Confidentiality / Privacy
  - Accountability / Non-repudiation
- Real-world systems combine multiple layers:
  - cryptographic protocols (e.g., TLS)
  - applications (e.g., banking apps)
  - authentication systems (e.g., MitID)
  - operating systems, compilers, hardware
- Failures at any layer can break security
- This course focuses on *protocol-level* reasoning

---

## Alice–Bob Notation (AnB)

- A formal language to describe security protocols
- Represents protocols as message exchanges
- Focuses on *who sends what to whom*
- Abstracts away low-level cryptographic details
- Assumes cryptography is perfect (black-box model)
- Used directly by verification tools such as OFMC

---

## Agents, Variables, and Constants

- Identifiers starting with uppercase letters are **variables**
  - Examples: A, B, KAB
  - Can be instantiated with any concrete agent during execution
  - Includes the intruder
- Identifiers starting with lowercase letters are **constants or functions**
  - Examples: s, sk
- Special agent:
  - `i` represents the intruder
- Trusted agents:
  - Modeled as constants (e.g., server `s`)
  - Cannot be instantiated as the intruder
  - Fixed across all protocol runs

---

## Initial Knowledge

- Every protocol role must specify initial knowledge
- Determines what messages an agent can send or construct
- Typically:
  - All agents know all agent names
  - Long-term secrets are modeled using functions (e.g., `sk(A,s)`)
- The intruder’s knowledge is derived from role instantiation
- If the intruder plays a role, it gains that role’s initial knowledge
- This ensures fairness: the intruder can act as a normal participant

---

## First Key Exchange Protocol (Naive Version)

- Goal:
  - Establish a fresh shared key between A and B
- Assumptions:
  - A and B do not initially share a key
  - Both share long-term keys with a trusted server `s`
- High-level flow:
  - A contacts server with identities of A and B
  - Server generates fresh key
  - Key is forwarded to B
- Fresh values are created by the first agent that uses them

---

## Intruder Capabilities

- Complete control over the network
- Can:
  - intercept messages
  - replay messages
  - fabricate messages
  - choose arbitrary agent identities
- Cannot:
  - break cryptographic primitives
- Without cryptography:
  - there is no reliable sender identity
  - all messages can be spoofed

---

## First Attack: Secrecy Violation

- The server sends the session key in clear text
- The intruder instantiates protocol roles arbitrarily
- The intruder contacts the server pretending to be honest agents
- The server generates a fresh key and sends it
- The intruder immediately learns the secret key
- This violates the secrecy goal
- Demonstrates:
  - secrecy must be explicitly protected
  - trusted behavior is not enough

---

## Encryption Attempt and Executability

- Encryption is added to protect the key
- Problem:
  - some agents are instructed to send messages they cannot construct
- OFMC rejects such protocols as *not executable*
- A protocol must be feasible given agents’ knowledge
- Formal models enforce realistic message construction

---

## Role Confusion and Weak Authentication

- Server encrypts messages for both A and B
- A forwards encrypted data it cannot decrypt
- Intruder chooses different identities for each role
- Different agents disagree on who is communicating with whom
- Results in authentication failure
- Demonstrates:
  - agreement on a key is not enough
  - identities must be bound to protocol data

---

## Identity Binding

- Agent names are added to encrypted messages
- Reduces ambiguity about protocol roles
- Still allows attacks if roles can be swapped
- Shows that:
  - identity binding must be unambiguous
  - order and roles matter

---

## Replay Attacks

- Messages are replayed in a new session
- Receiver treats replay as a fresh protocol run
- Leads to repeated acceptance of old data
- Common real-world consequences:
  - repeated bank transfers
  - reuse of compromised keys
- Replay attacks indicate missing freshness

---

## Freshness and Challenge–Response

- Fresh nonces are introduced
- Each party contributes fresh data
- Encrypted messages include nonces
- Ensures messages are recent
- Prevents replay attacks
- This version satisfies secrecy and authentication goals

---

## Key Takeaways

- Formal modeling exposes subtle protocol flaws
- Small changes can drastically alter security properties
- Intruders exploit:
  - ambiguity
  - missing bindings
  - missing freshness
- Secure protocol design requires:
  - precise goals
  - explicit assumptions
  - careful message construction
