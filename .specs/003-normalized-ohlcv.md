# Normalized OHLCV

Status: DRAFT / PHASE 0

## Purpose
Define the canonical market-data normalization contract used by all providers and downstream scoring. This protects the system from provider-specific bar semantics and misaligned timestamps.

## Scope
Normalization covers:
- bar timestamps
- symbol representation
- exchange and market venue metadata
- adjusted and unadjusted price conventions
- corporate action handling
- OHLCV validation
- missing-value handling
- point-in-time consistency

## Inputs
- raw bar payloads from provider adapters
- symbol metadata
- market calendar data
- corporate action metadata
- adjustment mode declarations

## Outputs
- canonical market bar records
- normalized timeframe aggregates
- symbol/venue state metadata
- quality gate decisions and warnings

## Dependencies
- provider contracts
- data-quality gates
- market calendar and corporate action registry
- time-series persistence layer

## Data Contracts
Canonical bar record should include:
- provider_id
- symbol
- exchange
- timeframe
- open
- high
- low
- close
- volume
- source_timestamp
- retrieval_timestamp
- adjustment_convention
- schema_version
- is_complete
- out_of_order_flag
- corporate_action_reference

## Failure Behavior
- duplicated bars: quarantine by hash and timestamp
- out-of-order bars: keep raw data but mark and reindex if allowed
- missing close or invalid OHLC: reject bar
- inconsistent volume scale: flag but do not silently coerce
- missing corporate action metadata: suspend affected windows until reviewed

## Security Considerations
- raw provider payloads must not be logged verbatim if they include credentials or sensitive metadata
- bar records do not contain secrets or broker account details
- schema enforcement must protect against malformed input

## Test Cases
- duplicate bar with same timestamp and symbol is rejected
- out-of-order historical bar is flagged
- adjusted vs unadjusted mode is preserved
- missing OHLC item is rejected deterministically
- corporate action causes a clean boundary and a new continuous series

## Acceptance Criteria
- every bar is produced through a documented normalization step
- source and retrieval timestamps are retained
- no bar enters scoring without schema validation
- normalization and provider data remain auditable

## Unresolved Questions
- exact bar timezones and exchange-local conventions
- preferred corporate action handling strategy for splits/dividends
- whether to keep both adjusted and raw series in storage

## Implementation Status
PLANNED. Normalized OHLCV contract is defined before ingestion implementation begins.
