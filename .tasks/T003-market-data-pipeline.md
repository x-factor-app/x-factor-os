# T003 — Market Data Pipeline

Priority: P1
Status: PLANNED

## Goal
Define the ingestion and normalization path from provider records to canonical OHLCV data.

## Deliverables
- raw data provenance model
- normalized OHLCV schema
- adapter contract and time-series persistence pattern
- data coverage and corporate action handling plan

## Acceptance Criteria
- raw provider payloads are retained with provenance
- all canonical bars carry source and retrieval timestamps
- bars pass schema validation before entering scoring
- duplicate and out-of-order cases are explicitly handled

## Validation Steps
- run schema validation tests on fixture provider payloads
- verify duplicate bars are quarantined
- verify out-of-order bars are flagged and not silently used

## Notes
This work must not connect to live market feeds until provider contracts and data rights have been approved.
