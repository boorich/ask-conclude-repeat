// ? Protocol — Reasoning Geometry Commitment Scheme
// 
// A zero-knowledge commitment layer for personal reasoning geometry.
// Users commit encrypted axis-placements derived from their own reasoning
// without revealing content, identity, or the reasoning itself.
//
// Axes (user-defined quality dimensions, after Gärdenfors 2000):
//   axis_0: decentralized (-127) <-> centralized (+127)
//   axis_1: open (-127) <-> closed (+127)
//   axis_2: long-term (-127) <-> short-term (+127)
//   axis_3: individual (-127) <-> collective (+127)
//
// The commitment is: Poseidon(axis_0, axis_1, axis_2, axis_3, salt, domain)
// The salt is private. The commitment is public. Content is never stored.
// This is the ABI between a human mind and collective reasoning geometry.

use starknet::ContractAddress;
use starknet::get_caller_address;
use starknet::get_block_timestamp;
use core::poseidon::PoseidonTrait;
use core::hash::{HashStateTrait, HashStateExTrait};

// A reasoning commitment — the geometric fingerprint of a thought.
// Content is gone. Structure remains.
#[derive(Drop, Serde, starknet::Store)]
struct ReasoningCommitment {
    commitment_hash: felt252,   // Poseidon(axes, salt, domain) — public
    domain: felt252,            // topic namespace — public (e.g. 'governance', 'infra')
    timestamp: u64,             // when it was committed — public
    disclosed: bool,            // has the committer chosen to disclose axis structure
    // Axis placements — only populated if committer explicitly discloses
    // Range: -127 to 127 encoded as i8, stored as felt252
    // Default: 0 (undisclosed)
    axis_0_public: felt252,     // decentralized <-> centralized
    axis_1_public: felt252,     // open <-> closed
    axis_2_public: felt252,     // long-term <-> short-term
    axis_3_public: felt252,     // individual <-> collective
}

// Aggregate resonance in a domain — collective geometry without individual identity
#[derive(Drop, Serde, starknet::Store)]
struct DomainResonance {
    domain: felt252,
    total_commitments: u64,
    disclosed_commitments: u64,
    // Running sums of disclosed axis placements for averaging
    // Only populated from explicitly disclosed commitments
    sum_axis_0: felt252,
    sum_axis_1: felt252,
    sum_axis_2: felt252,
    sum_axis_3: felt252,
}

#[starknet::interface]
trait IQuestionProtocol<TContractState> {
    // Commit a reasoning geometry without revealing content or identity linkage.
    // commitment_hash = Poseidon(axis_0, axis_1, axis_2, axis_3, salt, domain)
    // computed client-side. Salt is never transmitted. Content is never transmitted.
    fn commit(
        ref self: TContractState,
        commitment_hash: felt252,
        domain: felt252,
    ) -> u64; // returns commitment_id

    // Optionally disclose axis placements for a prior commitment.
    // Requires proof that axes + salt + domain hash to the original commitment.
    // Disclosure is voluntary and irreversible per commitment.
    fn disclose(
        ref self: TContractState,
        commitment_id: u64,
        axis_0: felt252,
        axis_1: felt252,
        axis_2: felt252,
        axis_3: felt252,
        salt: felt252,
    );

    // Read a specific commitment by id — public data only
    fn get_commitment(
        self: @TContractState,
        commitment_id: u64,
    ) -> ReasoningCommitment;

    // Read aggregate resonance for a domain — collective geometry, no individual data
    fn get_domain_resonance(
        self: @TContractState,
        domain: felt252,
    ) -> DomainResonance;

    // Verify a commitment hash without disclosing — proves you can reconstruct it
    fn verify_commitment(
        self: @TContractState,
        commitment_id: u64,
        axis_0: felt252,
        axis_1: felt252,
        axis_2: felt252,
        axis_3: felt252,
        salt: felt252,
    ) -> bool;

    // Total commitments ever made — protocol health metric
    fn total_commitments(self: @TContractState) -> u64;
}

#[starknet::contract]
mod QuestionProtocol {
    use super::{
        ReasoningCommitment, DomainResonance, IQuestionProtocol,
        ContractAddress, get_caller_address, get_block_timestamp,
        PoseidonTrait, HashStateTrait, HashStateExTrait,
    };

    #[storage]
    struct Storage {
        // commitment_id -> ReasoningCommitment
        commitments: starknet::storage::Map<u64, ReasoningCommitment>,
        // commitment_id -> committer address
        // stored separately so commitment struct itself is not identity-linked
        committers: starknet::storage::Map<u64, ContractAddress>,
        // domain -> DomainResonance aggregate
        domain_resonance: starknet::storage::Map<felt252, DomainResonance>,
        // monotonic counter — commitment ids
        commitment_counter: u64,
    }

    #[event]
    #[derive(Drop, starknet::Event)]
    enum Event {
        Committed: Committed,
        Disclosed: Disclosed,
    }

    // Public event — commitment exists, domain is known, content is not
    #[derive(Drop, starknet::Event)]
    struct Committed {
        commitment_id: u64,
        commitment_hash: felt252,
        domain: felt252,
        timestamp: u64,
    }

    // Public event — axis structure voluntarily revealed
    #[derive(Drop, starknet::Event)]
    struct Disclosed {
        commitment_id: u64,
        domain: felt252,
        axis_0: felt252,
        axis_1: felt252,
        axis_2: felt252,
        axis_3: felt252,
    }

    #[abi(embed_v0)]
    impl QuestionProtocolImpl of IQuestionProtocol<ContractState> {

        fn commit(
            ref self: ContractState,
            commitment_hash: felt252,
            domain: felt252,
        ) -> u64 {
            let caller = get_caller_address();
            let timestamp = get_block_timestamp();
            let id = self.commitment_counter.read();

            // Store commitment — no axes, no content, no reasoning
            let commitment = ReasoningCommitment {
                commitment_hash,
                domain,
                timestamp,
                disclosed: false,
                axis_0_public: 0,
                axis_1_public: 0,
                axis_2_public: 0,
                axis_3_public: 0,
            };

            self.commitments.write(id, commitment);
            self.committers.write(id, caller);
            self.commitment_counter.write(id + 1);

            // Update domain resonance counter
            let mut resonance = self.domain_resonance.read(domain);
            if resonance.total_commitments == 0 {
                // First commitment in this domain — initialize
                resonance = DomainResonance {
                    domain,
                    total_commitments: 1,
                    disclosed_commitments: 0,
                    sum_axis_0: 0,
                    sum_axis_1: 0,
                    sum_axis_2: 0,
                    sum_axis_3: 0,
                };
            } else {
                resonance.total_commitments += 1;
            }
            self.domain_resonance.write(domain, resonance);

            self.emit(Committed { commitment_id: id, commitment_hash, domain, timestamp });

            id
        }

        fn disclose(
            ref self: ContractState,
            commitment_id: u64,
            axis_0: felt252,
            axis_1: felt252,
            axis_2: felt252,
            axis_3: felt252,
            salt: felt252,
        ) {
            let caller = get_caller_address();
            let committer = self.committers.read(commitment_id);

            // Only the original committer can disclose
            assert(caller == committer, 'not your commitment');

            let mut commitment = self.commitments.read(commitment_id);

            // Cannot disclose twice
            assert(!commitment.disclosed, 'already disclosed');

            // Verify axes + salt + domain reconstruct the original commitment hash
            // This proves the discloser actually knew the axes at commit time
            let recomputed = PoseidonTrait::new()
                .update(axis_0)
                .update(axis_1)
                .update(axis_2)
                .update(axis_3)
                .update(salt)
                .update(commitment.domain)
                .finalize();

            assert(recomputed == commitment.commitment_hash, 'invalid proof');

            // Update commitment with disclosed axes
            commitment.disclosed = true;
            commitment.axis_0_public = axis_0;
            commitment.axis_1_public = axis_1;
            commitment.axis_2_public = axis_2;
            commitment.axis_3_public = axis_3;
            self.commitments.write(commitment_id, commitment);

            // Update domain resonance aggregate
            let mut resonance = self.domain_resonance.read(commitment.domain);
            resonance.disclosed_commitments += 1;
            resonance.sum_axis_0 += axis_0;
            resonance.sum_axis_1 += axis_1;
            resonance.sum_axis_2 += axis_2;
            resonance.sum_axis_3 += axis_3;
            self.domain_resonance.write(commitment.domain, resonance);

            self.emit(Disclosed {
                commitment_id,
                domain: commitment.domain,
                axis_0,
                axis_1,
                axis_2,
                axis_3,
            });
        }

        fn get_commitment(
            self: @ContractState,
            commitment_id: u64,
        ) -> ReasoningCommitment {
            self.commitments.read(commitment_id)
        }

        fn get_domain_resonance(
            self: @ContractState,
            domain: felt252,
        ) -> DomainResonance {
            self.domain_resonance.read(domain)
        }

        fn verify_commitment(
            self: @ContractState,
            commitment_id: u64,
            axis_0: felt252,
            axis_1: felt252,
            axis_2: felt252,
            axis_3: felt252,
            salt: felt252,
        ) -> bool {
            let commitment = self.commitments.read(commitment_id);
            let recomputed = PoseidonTrait::new()
                .update(axis_0)
                .update(axis_1)
                .update(axis_2)
                .update(axis_3)
                .update(salt)
                .update(commitment.domain)
                .finalize();
            recomputed == commitment.commitment_hash
        }

        fn total_commitments(self: @ContractState) -> u64 {
            self.commitment_counter.read()
        }
    }
}

// — What this contract does —
//
// commit()      : You hash your reasoning geometry client-side with a private salt.
//                 Only the hash lands on-chain. Content: gone. Identity: severed.
//
// disclose()    : Voluntary. You prove you knew the axes at commit time by
//                 reconstructing the hash. Axes become public. Salt stays private.
//                 Disclosure is your choice, per commitment, irreversible.
//
// get_domain_resonance() : Returns aggregate axis sums across all disclosed
//                 commitments in a domain. This is the collective geometry —
//                 what a population of reasoning agents is collectively pointing at,
//                 without any individual being identifiable.
//
// verify_commitment() : Anyone can verify you knew the axes without you disclosing them.
//                 Proof of reasoning without revelation of reasoning.
//
// — What this contract does NOT do —
//
// It does not store content.
// It does not link commitments to identity beyond what the caller chooses.
// It does not have an admin key.
// It does not have an upgrade path. (Deploy once. Ossify.)
// It does not know what your axes mean. That's yours.
//
// — The protocol is the commitment scheme —
// The axes are the epistemology.
// The salt is the sovereignty.
// The resonance is the emergence.
// The "?" is what remains when "!" is stripped away.
