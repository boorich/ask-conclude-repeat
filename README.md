# ?

A zero-knowledge commitment scheme for personal reasoning geometry, deployed on StarkNet.

---

## What it does

Most knowledge infrastructure stores conclusions — what you think.  
**?** stores the geometric structure of *how you reasoned* — without storing the reasoning itself.

You assign your insight a position on four axes (your own epistemological coordinates), hash it client-side with a private salt, and commit only the hash on-chain. Content never leaves your machine. Identity is not required. Disclosure is voluntary.

At scale, the aggregate of disclosed commitments across a domain produces a collective reasoning map — what a population is pointing at, without any individual being identifiable.

## The four axes

```
decentralized  ↔  centralized       (-127 to +127)
open           ↔  closed
long-term      ↔  short-term
individual     ↔  collective
```

After Gärdenfors, *Conceptual Spaces: The Geometry of Thought*, MIT Press 2000.

## The contract

Four functions. That is the whole protocol.

```cairo
// commit — hash lands on-chain. content: gone. identity: severed.
fn commit(commitment_hash: felt252, domain: felt252) -> u64

// disclose — voluntary. prove you knew the axes at commit time. irreversible.
fn disclose(commitment_id: u64, axis_0..3: felt252, salt: felt252)

// get_domain_resonance — collective geometry. no individual data.
fn get_domain_resonance(domain: felt252) -> DomainResonance

// verify_commitment — prove you knew without revealing.
fn verify_commitment(commitment_id: u64, axes: felt252, salt: felt252) -> bool
```

## What it does not do

- Store content
- Link commitments to identity beyond what the caller chooses
- Have an admin key
- Have an upgrade path — deploy once, ossify
- Know what your axes mean — that is yours

## Why

Every knowledge system ever built manages `!` — the claim, the conclusion, the decree.  
This manages `?` — the structure of the asking.

You cannot suppress a question. You can only answer it or ignore it.

## Stack

- Cairo 1.0 / StarkNet
- Poseidon hash (ZK-optimized, no trusted setup)
- No foundation. No company. Open protocol.

---

*The right question rather than the right conclusion.*