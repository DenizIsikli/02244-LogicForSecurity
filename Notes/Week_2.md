# Week 02 – Modeling Protocols: Dolev–Yao

---

## Recap and Context

- We continue modeling security protocols formally
- Focus shifts from protocol design to **intruder capabilities**
- Goal: define *exactly* what an attacker can and cannot do
- The Dolev–Yao model is the standard symbolic attacker model
- Used by tools such as OFMC for automated protocol analysis

---

## Sixth Version of the Key Exchange Protocol

- Uses Diffie–Hellman to establish a shared key
- Both A and B contribute fresh values
- Trusted server `s` helps authenticate exchanged values
- Server does **not** learn the final session key
- Achieves secrecy and authentication goals under the model

### Message Flow (Conceptual)

- A sends a Diffie–Hellman half-key to B
- B sends both half-keys to the server, encrypted
- Server confirms identities and forwards information to A
- A and B compute the same shared key independently
- Payload is encrypted using the derived shared key

---

## Diffie–Hellman Key Exchange

- Each agent generates a fresh secret value
- Agents exchange public half-keys
- Shared key is derived independently by both parties
- Computational hardness assumption:
  - infeasible to recover secrets from public values
- Trusted party is not required to know the key itself

### Security Properties

- Both parties contribute freshness
- Honest-but-curious server cannot read encrypted payloads
- Provides Perfect Forward Secrecy
- Long-term key compromise does not reveal past sessions

### Why Authentication Is Still Needed

- Half-keys are public values
- Intruder can replace or replay half-keys
- Parties must be assured of the origin of public values
- Trusted server authenticates the half-keys

---

## Modeling Agents and Key Infrastructures

- Uppercase identifiers are **variables**
  - Can be instantiated by any concrete agent
  - Includes the intruder
- Lowercase identifiers are **constants**
  - Fixed across all runs
  - Used for trusted agents such as servers
- Special agent `i` represents the intruder
- Key infrastructures are modeled using functions:
  - shared keys
  - passwords
  - public keys and private keys

---

## Initial Knowledge

- Every protocol role has explicitly defined initial knowledge
- Determines what messages an agent can construct
- Initial knowledge must not contain non-agent variables
- Long-term secrets are modeled using functions
- Fresh values are created by the first agent that uses them
- If the intruder plays a role, it gains that role’s knowledge

---

## Alice–Bob Notation: Syntax Rules

- Uppercase identifiers: variables
- Lowercase identifiers: constants and functions
- All identifiers must have declared types
- Types allow detection of type-flaw attacks
- OFMC can ignore types using the `-untyped` option
- Variables not in initial knowledge are freshly generated

---

## Message Term Algebra

- Messages are constructed from:
  - constants
  - variables
  - concatenation
  - symmetric encryption
  - asymmetric encryption
  - signatures
  - exponentiation
  - user-defined functions
- Public functions can be applied by any agent
- Private functions (e.g. private keys) are restricted
- The intruder name is a distinguished constant

---

## Syntax vs Semantics

- Syntax defines which messages are well-formed
- Semantics define what messages *mean*
- Semantics specify how messages can be derived
- Modeled as rules of a formal game
- Allows automated reasoning about attacker behavior

---

## The Dolev–Yao Intruder Model

- Introduced by Dolev and Yao (1983)
- Cryptography is a black box
- Intruder cannot break cryptography
- Intruder can use crypto operations with known keys
- Intruder controls the entire communication network
- Worst-case assumption for security analysis
- Intruder may act as a dishonest participant

---

## Intruder Deduction

- Defined using a deduction relation
- Based on what the intruder knows
- Determines which messages can be derived
- Models attacker reasoning symbolically
- Independent of message meaning
- Depends only on deduction rules

---

## Dolev–Yao Deduction Rules (Overview)

- Axiom:
  - Intruder can use any message it already knows
- Composition:
  - Intruder can apply public functions
- Decomposition:
  - Intruder can extract parts of known messages
- Encryption and decryption:
  - Allowed only with known keys
- Signatures:
  - Can be created and opened with appropriate keys

---

## Symmetric Cryptography Rules

- Intruder can encrypt known messages with known keys
- Intruder can decrypt messages if key is known
- If key is unknown, ciphertext is opaque
- Demonstrates secrecy under perfect cryptography

---

## Asymmetric Cryptography Rules

- Public keys are known to everyone
- Private keys are secret
- Intruder can encrypt with public keys
- Intruder can decrypt only with private keys
- Models public-key infrastructure assumptions

---

## Signatures

- Intruder can sign messages with known private keys
- Anyone can open a signature using the public key
- Signatures provide authenticity, not secrecy
- Private key ownership is critical

---

## Concatenation

- Intruder can concatenate known messages
- Intruder can split concatenated messages
- Enables message reassembly and manipulation
- Important for modeling realistic attacks

---

## Public Functions

- Any public function can be applied freely
- Includes encryption, pairing, key derivation
- Intruder reasoning is closed under public computation
- Captures realistic attacker capabilities

---

## Infinite Deduction Space

- Deduction rules generate infinitely many terms
- Example: repeated encryption
- Requires careful handling in automation
- Motivates decision procedures

---

## Automating Dolev–Yao Deduction

- Goal: decide whether a message can be derived
- Input:
  - finite intruder knowledge set
  - target message
- Output:
  - yes or no
  - proof in the positive case

---

## Composition-Only Deduction

- Restricts intruder to public composition rules
- No analysis or decryption
- Simpler decision procedure
- Decidable by recursive backward search
- Terminates because terms get smaller

---

## Analysis Steps

- Extract plaintext when decryption keys are available
- Extract message parts from concatenations
- Add newly derived messages to intruder knowledge
- Repeat until no new messages can be added
- Finite because only subterms are added

---

## Full Decision Procedure

- First perform analysis steps to expand knowledge
- Then apply composition-only check
- Procedure is:
  - sound
  - complete
  - terminating
- Ensures correctness of automated tools

---

## Completeness Argument (High-Level)

- Any valid Dolev–Yao proof can be reconstructed
- Analysis steps are discovered first
- Remaining steps are purely compositional
- Proof relies on eliminating unnecessary analysis
- Guarantees that no attack is missed

---

## Negative Deduction

- Can also prove non-derivability
- If procedure returns “no”, derivation does not exist
- Important for proving secrecy properties
