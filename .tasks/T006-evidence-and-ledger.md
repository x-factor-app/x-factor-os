# T006 — Evidence and Ledger

Priority: P1
Status: PLANNED

## Goal
Preserve immutable candidate history, evidence provenance, and lifecycle status changes.

## Deliverables
- evidence DNA schema
- candidate ledger model
- lifecycle event records
- correction and invalidation process

## Acceptance Criteria
- original candidate record and scoring version are preserved
- evidence references remain auditable
- corrections are appended as events, not silent rewrites
- lifecycle transitions are logged and queryable

## Validation Steps
- create a candidate and append correction events
- verify original history remains intact
- verify invalidation is a recorded event rather than a deletion

## Notes
The ledger is required before alert delivery or outcome accounting begins.
