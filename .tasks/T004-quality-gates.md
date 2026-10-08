# T004 — Quality Gates

Priority: P1
Status: PLANNED

## Goal
Ensure that only validated and trustworthy data reaches scoring.

## Deliverables
- quality gate rules
- quarantine and rejection logic
- missing-data handling policy
- audit log for gate decisions

## Acceptance Criteria
- quality gate pass/fail is deterministic
- all unsupported, stale, mismatched, or duplicate data is rejected or quarantined
- no candidate can be generated from data that fails gates

## Validation Steps
- inject duplicate and stale bar cases
- verify outage handling and corporate action boundary handling
- confirm pipeline stops on invalid schema or malformed records

## Notes
This task must occur before scoring implementation begins.
