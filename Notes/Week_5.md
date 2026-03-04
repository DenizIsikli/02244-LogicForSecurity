# Week 05 – Channels and Composition

---

## Type-Flaw Attack (Previous Challenge)

Protocol:

- A → B : A, NA
- B → s : A, B, NA, NB, {|A, NA, NB|}sk(B,s)
- s → A : {|B, KAB, NA, NB|}sk(A,s), {|A, KAB|}sk(B,s)
- A → B : {|A, KAB|}sk(B,s), {|NB|}KAB

Attack idea:

- Messages
  - {|A, NA, NB|}sk(B,s)
  - {|A, KAB|}sk(B,s)
- Unifier:
  - KAB = ⟨NA, NB⟩

Result:

- Intruder reuses B’s message
- B accepts it as a valid response
- Intruder controls session key

---

## Fix Using Message Formats

Introduce formats:

- f1(A, NA, NB)
- f2(B, KAB, NA, NB)
- f3(A, KAB)
- f4(NB)

Updated protocol:

- A → B : A, NA
- B → s : A, B, NA, NB, {|f1(A,NA,NB)|}sk(B,s)
- s → A : {|f2(B,KAB,NA,NB)|}sk(A,s), {|f3(A,KAB)|}sk(B,s)
- A → B : {|f3(A,KAB)|}sk(B,s), {|f4(NB)|}KAB

Effect:

- Messages have unique structure
- Prevents unintended unification
- Achieves type‑flaw resistance

---

# Protocol Composition

Large systems combine multiple protocols:

Examples:

- TLS
- authentication protocols
- web application protocols

Security must hold for both:

- individual protocols
- their composition

---

## Parallel Composition

Goal:

- verify protocols independently
- ensure security when executed together

Key idea:

- use disjoint message formats
- prevent cross‑protocol confusion

If protocols satisfy composability conditions and are secure individually, their parallel composition is secure.

---

## Parallel Composability Conditions

Requirements:

- protocols are type‑flaw resistant
- shared secrets are controlled
- message formats are disjoint
- protocols do not leak secrets

Result:

- security of P1 and P2 implies security of P1 ∥ P2

Variant:

- flaws in P2 do not break P1 if conditions hold.

---

# Vertical Composition

Real systems are layered.

Example components:

- TLS secure channel
- identity provider authentication
- application protocol

Each component developed independently.

Security must still hold when components interact.

---

## Layering Example

Application:

- getBalance request
- balance response

Channel:

- encrypted communication between agents

Example:

- getBalance(N)
- balance(N,Bal)

Replay protection achieved using nonces.

Application and channel can be verified separately.

---

# Channel Abstractions

Notation:

Authentic channel

- A •→ B
- receiver knows sender identity
- no confidentiality

Confidential channel

- A →• B
- only receiver can read message

Secure channel

- A •→• B
- both authenticity and confidentiality

---

## Channels in OFMC

Channels can replace cryptographic messages.

Example:

- A →• B : NA, A
- B →• A : NA, NB, B
- A →• B : NB

This models confidential channels without explicit encryption.

---

# Cryptographic Implementation of Channels

Agents have:

Encryption keys:

- ck(A), inv(ck(A))

Signature keys:

- ak(A), inv(ak(A))

Channel encoding:

Authentic channel:

- {atag, B, M}inv(ak(A))

Confidential channel:

- {ctag, M}ck(B)

Secure channel:

- {{stag, B, M}inv(ak(A))}ck(B)

Tags distinguish channel encodings.

---

## Authenticating the Recipient

Recipient identity included in signatures:

Example:

- {atag, B, M}inv(ak(A))

Purpose:

- prevent forwarding attacks
- ensure message intended for correct recipient

---

# Channel Calculus

Relationships between channel types.

Rule 1:

Authentic channel A •→ B
can produce confidential channel B →• A

Rule 2:

Confidential channel A →• B
can produce authentic channel B •→ A

Rule 3:

Authentic + confidential channels
combine into secure channel.

---

# TLS Example

Simplified TLS handshake:

- Diffie‑Hellman exchange
- server certificate authentication
- session keys derived

Payload messages:

- {|data, DATAA|}k1
- {|data, DATAB|}k2

Goal:

- secure communication channel.

---

## TLS Without Client Authentication

Typical web TLS:

- server authenticated
- client anonymous

Result:

- secure channel with pseudonymous client.

---

# Secure Pseudonymous Channels

Client identified by pseudonym:

- exp(g,X)

Properties:

- secure communication
- identity not guaranteed
- ownership defined by knowledge of secret X

Later authentication can bind identity to pseudonym.

---

## Login over TLS

Example login message:

- {|login, A, password(A,B)|}k1

Effects:

- server learns client identity
- password protected by TLS channel

Common threat:

- phishing sites impersonating server.

---

# Vertical Composition Result

Method:

1. verify application assuming secure channel
2. verify channel implements abstraction

If both hold:

- composed system remains secure.

Allows scalable verification of layered security protocols.
