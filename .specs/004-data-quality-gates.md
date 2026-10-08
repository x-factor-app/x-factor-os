# Data Quality Gates

Status: DRAFT / PHASE 0

## Purpose
Ensure that only trustworthy market data can feed scoring. Quality gates act as explicit policy checks for completeness, consistency, provenance, and continuity.

## Scope
Covers:
- completeness checks
- gap detection
- duplicate detection
- out-of-order detection
- stale data detection
- corporate action awareness
- provider outage checks
- schema validation

## Inputs
- normalized bars
- provider metadata
- bar-level validation state
- corporate action boundaries
- outage and retry state

## Outputs
- quality status for every bar series
- quarantine decisions
- gating summaries for candidate generation
- rejection reasons for downstream systems

## Dependencies
- provider contracts
- normalized OHLCV
- market calendar and corporate action metadata
- persistence and audit log

## Data Contracts
Quality gate status record should include:
- symbol
- timeframe
- provider_id
- gate_name
- pass_fail
- severity
- reason_code
- first_seen_at
- last_seen_at
- run_id
- schema_version

## Failure Behavior
- invalid or stale bar: reject from scoring pipeline
- partial data: does not become a valid candidate input
- unresolved duplicate: quarantine and raise issue
- provider outage: mark affected windows as UNAVAILABLE
- corporate action ambiguity: hold for manual review

## Security Considerations
- gate decisions must be logged without exposing credentials or provider secrets
- failure reasons should be deterministic and traceable
- alerts on gate failures must not leak internal data to unauthorized endpoints

## Test Cases
- provider outage produces no false-valid bar series
- duplicate bars are flagged and deduplicated by policy
- out-of-order bars are quarantined before scoring
- corporate action boundary passes a fresh series validation
- stale data beyond threshold fails gate

## Acceptance Criteria
- no scored candidate may use invalid, stale, or unproven data
- gate failures are recorded with reason codes and timestamps
- quality status is available for all upstream data used by scoring
- human review can be triggered for disputed or ambiguous conditions

## Unresolved Questions
- threshold values for stale data and gap tolerance
- whether to allow partial bar recovery from fallback providers
- audit policy for provider-side data repairs

## Implementation Status
PLANNED. Quality gates define hard boundaries before scoring implementation begins.
