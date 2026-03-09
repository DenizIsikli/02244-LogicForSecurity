# Week 06 – Privacy in Security Protocols

---

## Channel Calculus Recap

Channel rule:

- If we have a secure channel
  - A •→• B
- We can construct a secure channel in the opposite direction:
  - B •→• A

Example construction:

- A •→• B : K
- B → A : {|Payload|}K

Another rule:

- If we have:
  - authenticated channel A •→ B
  - confidential channel A →• B
- Then we can achieve:
  - secure channel A •→• B

Example:

- A →• B : Payload
- A •→ B : h(Payload)

---

## Building Secure Channels via a Server

Scenario:

- Secure channels exist between server s and agents A and B

Channels:

- s •→• A
- s •→• B

Possible solution:

- s •→• A : A, B, KAB
- s •→• B : A, B, KAB
- A → B : {|Payload|}KAB

Server distributes a session key.

Important requirement:

- s must be honest.

Reason:

- If intruder replaces s:
  - channels become A ↔ i and B ↔ i
  - intruder controls communication
  - no secure channel between A and B possible.

---

## Perfect Forward Secrecy Construction

Alternative approach:

- A •→• s : PKA
- B •→• s : PKB
- s •→• A : A, B, PKA, PKB
- s •→• B : A, B, PKA, PKB

Then:

- A → B : {{B,Payload}inv(PKA)}PKB

Properties:

- even if server compromised later
- payload remains confidential

This achieves **perfect forward secrecy**.

---

# Banking Example Extension

Application extension:

Transfer operation:

- transfer(N, Recip, Amount)

Replay protection:

- bank sends challenge N
- transfer message must include N

Protocol:

- B → A : N
- A → B : {|N, transfer(Recip,Amount)|}sk(A,B)

Purpose:

- prevents replay of transfer messages.

---

## Non-Repudiation Problem

Scenario:

- A sends transfer request
- later denies sending it

Example message:

- {|transfer(N,Recip,Bal)|}sk(A,B)

Problem:

- B cannot prove message came from A
- B could have created it himself

Solution:

- require digital signature

Example:

- {|{transfer(N,Recip,Bal)}inv(pk(A))|}sk(A,B)

Now:

- third party can verify A signed the transfer.

---

# Guessing Attacks

Example protocol:

- B → A : NB
- A → B : h(pw(A,B), NB)

Goal:

- B authenticates A on NB

Weakness:

- password pw(A,B) may have low entropy.

---

## Low Entropy Passwords

Examples:

- dictionary words
- short passwords

Intruder strategy:

1. observe message:

   NB, h(pw(A,B), NB)

2. build dictionary of possible passwords

3. compute:

   h(guess, NB)

4. compare with observed message

If equal:

- guessed password is correct.

This is an **offline dictionary attack**.

---

## Example: Password Still Secure

Protocol:

- A → B : {A, pw(A,B), K}pk(B)
- B → A : {|NB|}K

Properties:

- password encrypted with public key
- intruder cannot test guesses directly

Attack difficulty:

- intruder must guess both:

  - pw(A,B)
  - K

This makes guessing infeasible.

Modern protocols rely on this principle.

---

# Guessing Attacks in OFMC

OFMC supports modeling guessing attacks.

Method:

1. mark password as **guessable secret**
2. introduce constant guessPW known to intruder
3. test if intruder can derive:

   h(guessPW, NB)

If derivable:

- guessing attack exists.

---

# Privacy Motivation

Example scenario:

Parents vs teenager:

- teenager wants privacy
- parents want safety information

Solution idea:

- sealed letter describing location
- opened only in emergencies

Goal:

- reveal only necessary information
- hide everything else.

---

# Privacy in Voting Systems

Example protocol:

- FOO’92 voting protocol

Participants:

- voters V1...VN
- administrator A
- counter C

Votes:

- vi ∈ {0,1}

Goals:

- privacy of votes
- correct counting
- eligibility
- fairness
- verifiability.

---

# Simplified FOO Protocol

Steps:

1. voter sends vote to administrator
2. administrator signs ballot
3. voter submits signed ballot to counter
4. counter publishes ballots in random order

Problem:

- administrator sees votes.

Solution:

- blind signatures.

---

# Blind Signatures

Idea:

- voter blinds message before sending to authority

Process:

1. voter computes:

   blind(b,m)

2. authority signs:

   {blind(b,m)}inv(pk(A))

3. voter unblinds signature:

   {m}inv(pk(A))

Result:

- authority signs message
- without learning message content.

---

# Commitment Schemes

Commitment function:

- commit(m,r)

Properties:

Hiding:

- before revealing r
- message m cannot be learned

Binding:

- impossible to open commitment as different message.

Purpose:

- voter commits vote first
- reveals vote later.

---

# Final FOO Protocol Structure

1. voter commits vote

2. administrator signs blinded commitment

3. voter submits signed commitment

4. counter publishes commitments

5. voters reveal randomness

6. commitments opened and votes counted.

---

# Privacy Goals

Typical voting protocol goals:

- soundness
- completeness
- voter privacy
- unreusability
- eligibility
- fairness
- verifiability.

---

## Why Privacy Is Hard

Standard secrecy goal fails.

Example:

- vote vi ∈ {0,1}

Intruder already knows possible values.

Thus secrecy alone does not capture privacy.

Instead:

- privacy defined via **indistinguishability**.

---

# Indistinguishability Privacy

Examples:

Intruder cannot distinguish:

- voter V4 voted yes
- voter V4 voted no

Also:

- cannot tell if

  v4 = v5

or

  v4 ≠ v5.

Privacy defined as inability to distinguish scenarios.

---

# α–β Privacy

Framework for defining privacy.

Two formulas:

α

- high-level information intentionally revealed

β

- observable protocol information.

Idea:

- from β the intruder learns nothing
- beyond what α already implies.

---

## Model-Theoretic Definition

Definition:

- every Σ₀-model of α
- can be extended to a Σ-model of β

Interpretation:

- β does not rule out any world allowed by α.

Therefore:

- protocol leaks no additional information.

---

# Example: Vote Privacy

Public information:

- number of votes
- final result

Example:

- 100 voters
- 52 votes for "yes"

α:

- v1...v100 ∈ {0,1}
- v1 + ... + v100 = 52

Privacy requirement:

- intruder cannot deduce:

  v4 = 1

or

  v4 = v5

unless implied by α.

---

# Advantages of α–β Privacy

Declarative specification:

- specify protocol → defines β
- specify intended information → defines α

Benefits:

- clear modeling
- avoids complex equivalence proofs
- flexible privacy specification.

Privacy becomes reachability problem:

- can intruder derive new information from β not implied by α?

---

# Privacy Leakage Example

Example protocol:

RFID passport authentication (ICAO BAC).

Observation:

- different error messages
- reveal internal state differences.

Example:

- "nonce error"
- "format error"

Intruder learns whether two tags are identical.

Shows:

- privacy can be broken via observable protocol behavior.

---

# Key Takeaways

Privacy goals differ from secrecy.

Key techniques:

- blind signatures
- commitment schemes
- anonymous channels

Formal privacy definitions:

- indistinguishability
- α–β privacy

Privacy verification is an active research area in protocol security.
