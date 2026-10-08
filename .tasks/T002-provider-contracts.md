# T002 — Provider Contracts

Priority: P0
Status: PLANNED

## Goal
Define provider identities, contracts, and access statuses before any integration work begins.

## Deliverables
- provider registry
- access status matrix
- data entitlement and rights checklist
- contract schema for source and retrieval timestamps
- failure modes and retry policy

## Acceptance Criteria
- every provider has clear purpose and status: NOT_CONFIGURED, ACCESS_UNVERIFIED, VERIFIED, or REJECTED
- the contract includes source timestamp, retrieval timestamp, data coverage, entitlement/license, adjustment convention, schema version, request/run ID, and error handling
- future execution integrations are isolated and disabled by default

## Validation Steps
- review provider list and policy
- confirm required fields on each contract
- verify no live execution path is enabled in Phase 0

## Notes
Massive and Bigdata.com remain ACCESS_UNVERIFIED until access is confirmed. Clear Street or IBKR is REJECTED for Phase 0.
