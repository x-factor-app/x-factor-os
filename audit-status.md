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

## Proposed pass/fail criteria (PROPOSED until approved)

### Deterministic replay
- A fixed fixture is PASS only if the same input set produces the same score, version metadata, and ledger record for repeated runs.
- Proposed tolerance: numeric equality within PROPOSED ±0.000001 on intermediate or final score values, unless a different policy is approved.
- Proposed sample size: PROPOSED minimum 3 valid replay cases plus 1 missing-data case and 1 outage case before a replay suite is considered stable.
- FAIL if the score changes without a corresponding version change, or if the ledger record changes without a documented reason.

### Point-in-time leakage
- PASS only if all scoring inputs are anchored to timestamps that do not include future bars beyond the evaluation cutoff.
- Proposed rule: a candidate must never use data later than the evaluation cut-off by more than PROPOSED 1 bar tick or PROPOSED 1 minute, whichever is appropriate for the time frame, unless approved otherwise.
- FAIL if future bars are reachable from a feature window or if outcome accounting uses data after the defined exit horizon without explicit justification.

### Missing or corrupt data
- PASS only if missing required fields trigger explicit quarantine or rejection, not silent defaulting.
- Proposed rule: if any required feature field is missing, the candidate is rejected or blocked; no silent zero-fill is allowed unless explicitly approved.
- FAIL if a required field is replaced with a placeholder, zero, or neutral value without an explicit policy and approval.

### Duplicate and out-of-order bars
- PASS only if duplicate bars are either deduplicated under a documented rule or quarantined and rejected.
- Proposed rule: duplicate bar detection must compare symbol, timeframe, and source_timestamp; mismatches trigger quarantine and manual review.
- FAIL if an out-of-order bar is silently re-ordered without source_timestamp and sequence metadata preserving the original state.

### Provider outage handling
- PASS only if outages lead to MISSING coverage and deterministic failure of downstream candidate generation rather than silent success.
- Proposed rule: a provider failure must mark coverage as MISSING and halt dependent features unless an approved fallback is present.
- FAIL if the pipeline reports valid data from an outage window without an explicit provider status record.

### Prospective MFE/MAE accounting
- PASS only if candidate outcomes remain OPEN or NOT_YET_MATURE until the evaluation window has matured.
- Proposed rule: MFE/MAE may only be computed on observations with explicit exit or invalidation metadata; otherwise the outcome remains OPEN or NOT_YET_MATURE.
- Proposed sample size: PROPOSED minimum 30 mature observations before a review summary is considered statistically meaningful; smaller sample sizes require HOLD or NO-RELEASE wording.
- FAIL if unresolved or incomplete observations are reported as success or failure rates.

## Proposed acceptance requirements
- every validation category above must have an explicit test strategy
- the system must distinguish implemented behavior from tested behavior
- every failure mode must have a reproducible test case or manual review path
- no production claim is permitted without passing evidence and review
- all numeric tolerances, sample sizes, and cutoff windows are PROPOSED unless explicitly approved in writing
- no test is allowed to be described as "passed" without a defined result rule and review evidence

## Implementation Status
PLANNED. This validation plan must be executed before any production or broad release work begins. All numeric tolerances, cutoff windows, and sample sizes remain PROPOSED pending approval.

### Explicit approval requirement
No test evidence should be treated as successful validation until pass/fail thresholds, tolerances, and sample-size rules have been approved by a human reviewer.
