# Validation Plan

Status: DRAFT / PHASE 0

## Purpose
Define the validation plan required before production implementation or a controlled release.

## Validation Categories

### 1. Unit tests
- provider adapter validation
- normalized OHLCV transforms
- score feature calculations
- evidence DNA provenance checks
- ledger lifecycle transitions

### 2. Integration tests
- provider payload to normalized bar pipeline
- quality gate to candidate generation handoff
- ledger to alert lifecycle integration
- outcome accounting to review summary integration

### 3. Database tests
- migration order and schema integrity
- append-only ledger behavior
- retention and archive policy checks
- idempotent record writes

### 4. Deterministic replay tests
- fixed bar fixtures must produce identical outputs across runs
- scoring results must be reproducible from a record of inputs
- evidence references must remain stable and comparable

### 5. Failure injection tests
- closed provider endpoint
- malformed provider payload
- corrupted schema version
- delayed or duplicate first bars
- ambiguous corporate action metadata

### 6. Point-in-time leakage tests
- ensure no future data is used to score historical windows
- prevent look-ahead bias in scoring and outcomes
- validate evaluation windows are anchored to the correct timestamps

### 7. Duplicate and out-of-order bar tests
- duplicate bars are rejected or deduplicated by policy
- out-of-order bars are quarantined and not silently used
- time-order integrity is preserved per symbol and timeframe

### 8. Provider outage tests
- provider timeouts, partial responses, and retries
- missing coverage windows are marked MISSING instead of assumed valid
- downstream scoring must halt predictably on outage conditions

### 9. Corporate action tests
- split, dividend, and symbol event boundaries are validated
- adjusted and unadjusted conventions are preserved
- series continuity is re-established only with documented policy

### 10. Alert retry tests
- duplicate delivery attempts do not create additional ledger events
- retries are logged and tracked with idempotency keys
- failed or expired alerts remain visible for review

### 11. Outcome-accounting tests
- unresolved candidates remain OPEN or NOT_YET_MATURE
- ambiguous exits are documented conservatively
- prospective MFE/MAE accounting is versioned and not treated as live performance evidence

## Acceptance Requirements
- every validation category above must have an explicit test strategy
- the system must distinguish implemented behavior from tested behavior
- every failure mode must have a reproducible test case or manual review path
- no production claim is permitted without passing evidence and review

## Implementation Status
PLANNED. This validation plan must be executed before any production or broad release work begins.
