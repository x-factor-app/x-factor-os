# T005 — Scoring Engine

Priority: P1
Status: PLANNED

## Goal
Define the deterministic breakout-scoring engine with explicit versioning and PROPOSED parameters.

## Deliverables
- feature registry
- formulas and weights
- thresholds and missing-data policy
- deterministic fixtures
- score output schema

## Acceptance Criteria
- each feature has formula, weight, missing-data behavior, threshold, and version
- all weights are explicitly labeled PROPOSED
- scoring remains deterministic from fixture inputs
- no predictive accuracy claim is made before validation

## Validation Steps
- execute fixture cases for valid, missing-data, noisy, and rejected scenarios
- assert deterministic score outputs
- verify all scoring metadata is recorded in the candidate ledger

## Notes
All weights are unvalidated and must remain PROPOSED until verification is completed.
