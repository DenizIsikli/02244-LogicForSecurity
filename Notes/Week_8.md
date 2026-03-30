# Week 08 – Information Flow (Denning’s Approach)

---

## Motivation: IFTTT and Privacy Risks

IFTTT:

- platform connecting IoT devices and services via applets
- applets consist of:
  - trigger
  - filter (hidden JavaScript)
  - action

User visibility:

- can see trigger and action
- cannot see filter code

Security concern:

- filter may leak sensitive data without user knowledge fileciteturn9file0

---

# Applet-Based Attacks

Normal behavior:

- photo uploaded to IFTTT server URL
- Google Drive downloads image from URL

Attack idea:

- attacker modifies URL to include sensitive data

Example:

- attacker URL contains encoded photo URL
- Google Drive requests attacker URL
- attacker learns photo location and downloads it

Result:

- app works correctly
- attacker gets a copy of data fileciteturn9file0

---

## Explicit Information Flow Attack

Example:

- location data (loc) inserted into attacker-controlled URL

Flow:

- sensitive data directly copied into outgoing message

Definition:

- explicit flow = direct assignment or use

Example pattern:

- y := x + 1

Information flows:

- x → y

---

## Implicit Information Flow Attack

Example:

- driver name reconstructed using conditional checks

Mechanism:

- no direct assignment
- information leaks through control flow

Example pattern:

if x > 0 then y := 1 else y := 2

Information flows:

- x → y

Definition:

- implicit flow = via control structure

---

# Information Flow Control (IFC)

Goal:

- track how information moves through program

Variables represent:

- data
- files
- behavior

Types:

- explicit flows
- implicit flows

Security policy:

- defines allowed flows

Enforcement:

- static analysis detects violations fileciteturn9file0

---

# Denning’s Approach

Assign labels to variables.

Labels:

- Low
- High

Ordering:

- Low ⊑ High

Meaning:

- Low → High allowed
- High → Low forbidden

---

## Label Operations

Join (⊔):

- least upper bound
- Low ⊔ High = High

Meet (⊓):

- greatest lower bound
- Low ⊓ High = Low

Flow notation:

- x ⇝ y

---

# Example Flows

Variables:

- f1,f2 Low
- f3,f4 High
- x,i Low
- y High

Flows:

- f1 → x
- f2 → y
- x → f3
- y → f4
- y → x (problem)

---

## Flow Constraints

Each flow gives:

- label(x) ⊑ label(y)

Example:

- y ⊑ x violates if High ⊑ Low

---

# Program Certification

Steps:

1 parse program
2 build syntax tree
3 compute flows
4 generate constraints
5 verify constraints

If all hold:

- program secure fileciteturn9file0

---

# Declaration Rules

- variable assigned label from declaration

Example:

- integer High x → x = High

---

# Expression Rules

- constants → Low
- variable → its label
- E1 op E2 → E1 ⊔ E2

Boolean:

- same join rule

---

# Statement Rules

Assignment:

- E ⊑ var

Input:

- source ⊑ target

Output:

- E ⊑ var

If:

- B ⊑ S
- S = S1 ⊓ S2

While:

- B ⊑ S

Sequence:

- S = S1 ⊓ S2 fileciteturn9file0

---

# Example Violation

y := y + x * 3

- x High
- y Low

Result:

- High ⊑ Low (invalid)

Leak detected.

---

# Key Points

- track explicit + implicit flows
- enforce via labels
- check constraints statically
- prevents hidden leaks

---

# Challenges

- arrays
- procedures
- complex structures

Need extended rules.

---

# Takeaways

- information flow ≠ access control
- Denning model = label + ordering
- security = constraint satisfaction
- detects subtle leaks in real systems
