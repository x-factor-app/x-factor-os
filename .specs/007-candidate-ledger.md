# Candidate Ledger

Status: DRAFT / PHASE 0

## Purpose
Define the immutable ledger for each candidate. The ledger records the original candidate and scoring version, evidence references, times, triggers, invalidation events, and every lifecycle update without silently rewriting history.

## Scope
The candidate ledger covers:
- candidate creation
- scoring version and evidence references
- trigger conditions and target definitions
- invalidation and correction events
- lifecycle status transitions
- alert delivery state
- prospective outcome linkage

## Inputs
- score outputs
- evidence references
- alert lifecycle updates
- invalidation and correction metadata
- outcome records

## Outputs
- immutable candidate records
- signed or append-only event history
- auditable corrections and invalidations

## Dependencies
- deterministic scoring
- evidence DNA
- alert lifecycle
- prospective outcomes

## Data Contracts
Candidate ledger record must include:
- candidate_id
- symbol
- strategy_name
- candidate_version
- scoring_version
- evidence_ids
- trigger_type
- trigger_timestamp
- target_definition
- confidence_definition
- invalidation_reason
- invalidated_at
- created_at
- source_bar_window
- status

Lifecycle event record must include:
- candidate_id
- event_type
- timestamp
- metadata
- actor_or_system
- run_id
- previous_status
- new_status

## Failure Behavior
- corrections must append a new event; do not rewrite a prior record silently
- invalidation is recorded as an event, not as a deletion
- if target or confidence rules change, the new version is tracked separately
- alert duplicates are prevented through idempotent event handling

## Security Considerations
- ledger entries must be append-only or versioned with explicit correction events
- no account-level or secret data should enter the ledger
- audit trail must be tamper-evident through system design and retention policy

## Test Cases
- a corrected candidate is appended with a new event instead of overwriting prior history
- invalidation preserves the original trigger and scoring version
- target or confidence changes are represented as separate ledger history entries
- alert retries do not create duplicate candidates

## Acceptance Criteria
- original candidate data and scoring version are preserved
- evidence references are retained and immutable
- every status transition is logged with timestamp and reason
- corrections are auditable and explainable

## Unresolved Questions
- final ledger storage technology and indexing strategy
- retention policies for historical candidate records
- whether candidate IDs should be UUIDs or deterministic hashes

## Implementation Status
PLANNED. Candidate ledger design is required before alerting or performance tracking begins.
