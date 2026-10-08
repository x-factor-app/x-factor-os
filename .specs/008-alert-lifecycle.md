# Alert Lifecycle

Status: DRAFT / PHASE 0

## Purpose
Define how approved candidates become actionable alerts, how they are retried, and how lifecycle status is preserved without duplicate delivery or silent state changes.

## Scope
Covers:
- candidate approval to alert
- delivery payload generation
- retry and idempotency safeguards
- alert acknowledgement and status transitions
- alert expiry or invalidation
- human review workflow

## Inputs
- approved candidates from ledger
- delivery provider contract and channel metadata
- evidence references and score summary
- retry policy and delivery status

## Outputs
- alert payloads
- acknowledgement and failure events
- status transitions in the candidate ledger

## Dependencies
- candidate ledger
- evidence DNA
- provider contracts
- delivery provider

## Data Contracts
Alert record should include:
- alert_id
- candidate_id
- channel
- payload_version
- created_at
- delivery_status
- retry_count
- last_attempt_at
- acknowledgement_at
- expiry_at
- run_id

## Failure Behavior
- retryable delivery failure: retry with idempotency check
- nonretryable failure: mark alert FAILED and record reason
- duplicate alert processing: ignore duplicate by idempotency key and preserve original event
- expired candidate: move to EXPIRED with reason

## Security Considerations
- alert payloads must not expose secrets or broker account details
- no unapproved external distribution channel may be used in Phase 0
- alert content must remain within approved data classes and policy

## Test Cases
- same candidate produces exactly one successful alert under retry conditions
- duplicate retries do not mutate ledger history
- failed delivery logs deterministic reason and retry count
- candidate invalidation prevents alert dispatch after invalidation

## Acceptance Criteria
- alert delivery is idempotent and traceable
- candidate state and alert state are linked through immutable records
- retries are explicit and auditable
- no alert is dispatched from an invalid or expired candidate

## Unresolved Questions
- exact delivery channel for Phase 1
- alert payload format and user routing model
- whether digest or streamed alerting is used first

## Implementation Status
PLANNED. Alert lifecycle is defined before external communications or release paths are introduced.
