# System Boundaries

Status: DRAFT / PHASE 0

## Purpose
This specification defines the outer limits of the X-FACTOR OS system for Phase 0. It is intentionally architecture-first and evidence-driven, designed to prevent scope drift before any implementation work begins.

## Scope
The intended lifecycle is:

Provider registry and adapters -> raw data provenance -> normalized OHLCV -> data-quality gates -> deterministic breakout scoring -> evidence DNA -> immutable candidate lifecycle -> alert delivery -> prospective MFE/MAE tracking -> performance review -> controlled release.

The system must support:
- cross-market intelligence
- technical structure and relative strength
- catalyst and fundamental context
- smart-money / positioning evidence where available
- options and derivatives evidence where available
- portfolio risk and industry rankings
- a separate Quantum Computing research subset

The system must not assume data availability where coverage is unverified.

## Inputs
- provider metadata and entitlement registry
- market data bars, snapshots, reference data, and corporate actions
- research and catalyst records
- scoring features and model config
- candidate lifecycle state changes
- prospective trade outcomes and evaluation horizons
- governance and release controls

## Outputs
- normalized and vetted market data
- scored candidate records with evidence references
- alert payloads and message history
- performance review artifacts and release decisions

## Dependencies
- provider registry and adapter layer
- data normalization and time-series schema
- quality gates and data contracts
- deterministic scoring engine
- ledger and audit store
- alert queue / delivery provider
- review and release controls

## Data Contracts
- All entities must carry provenance metadata, run IDs, ingestion timestamps, and schema versions.
- All provider responses must be timestamped and traced to a source snapshot.
- Missing or ambiguous data must remain explicit and never be silently inferred as valid.
- The system must distinguish documented design from implemented behavior and implemented behavior from tested behavior.

## Failure Behavior
- Missing provider coverage: mark as MISSING and suppress downstream scoring inputs.
- Partial provider data: preserve raw data and mark partial coverage; do not backfill silently.
- Untrusted or conflicting evidence: quarantine and require manual review.
- Invalid scoring feature output: reject candidate generation and log the reason.
- Incomplete alert lifecycle: leave candidate in OPEN or PENDING state, never success/failure by assumption.

## Security Considerations
- No secrets in source control, logs, prompts, documents, browser automation, or generated artifacts.
- Access to provider credentials must be managed via environment variables or secret stores.
- Execution and scoring paths must be isolated from live order placement.
- All historic corrections must be audit-visible.

## Test Cases
- provider registry accepts and rejects invalid adapters
- out-of-order bars are quarantined
- missing corporate action metadata is flagged
- scoring without required inputs produces a deterministic rejected record
- evidence DNA references are immutable and auditable
- alert retries do not duplicate state transitions

## Acceptance Criteria
- system lifecycle is documented and traceable
- no production code is modified in Phase 0
- data availability rules are explicit for unavailable datasets
- scoring is deterministic and reproducible from fixtures
- candidate lifecycle is immutable and auditable

## Unresolved Questions
- default runtime stack (Node/TypeScript vs Python/FastAPI vs hybrid)
- provider shortlist and entitlements
- final data retention and review horizon
- alert channel and external provider selection

## Implementation Status
PLANNED. No code implementation yet. This document defines the architecture boundary for the first safe implementation milestone.
