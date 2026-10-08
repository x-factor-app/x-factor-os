# Provider Contracts

Status: DRAFT / PHASE 0

## Purpose
Define the minimum contract for every data provider and external integration. The contract exists to preserve provenance, enforce licensing and entitlement checks, and keep all downstream processing reproducible.

## Scope
This specification covers:
- Massive (OHLCV and reference data)
- Bigdata.com (research and catalyst evidence)
- PostgreSQL/Supabase (durable records and migrations)
- GitHub Actions (CI and PR validation)
- hosting platform (staging and production separation)
- Browserbase (optional browser-based research)
- future alert-delivery provider
- future execution integration (Clear Street or IBKR), isolated and disabled by default
- options data providers (future, coverage/rights pending)
- error monitoring and secrets management

## Inputs
- provider identity and endpoint metadata
- request configuration and run IDs
- retrieval timestamps and source timestamps
- coverage metadata and entitlement status
- schema version and adjustment conventions
- error payloads and retry metadata

## Outputs
- validated provider records
- normalized data payloads
- evidence of data access and coverage scope
- failure classifications and retry guidance

## Dependencies
- provider registry
- data ingestion adapters
- schema registry
- secrets manager
- audit and event log
- quality gates

## Data Contracts
Each provider must provide:
- provider_identity
- provider_type
- source_timestamp
- retrieval_timestamp
- data_coverage
- entitlement_or_license_status
- adjustment_convention
- schema_version
- request_run_id
- error_handling_mode
- status: NOT_CONFIGURED | ACCESS_UNVERIFIED | VERIFIED | REJECTED

Provider record contract:
- provider_id
- adapter_name
- source_name
- source_url_or_endpoint
- entitlement_status
- coverage_summary
- schema_version
- run_id
- fetched_at
- source_at
- record_count
- partial_ok
- error_code
- retryable

## Failure Behavior
- provider not configured: reject and log as NOT_CONFIGURED
- access not verified: block integration and mark ACCESS_UNVERIFIED
- verification failure: quarantine all records from that provider
- schema mismatch: reject the batch without mutation of downstream state
- provider outage: retry per policy but do not treat missing data as valid

## Security Considerations
- credentials never stored in source, docs, browser automation, prompts, or logs
- all provider config must be environment-scoped
- public README content must not reference secrets or private endpoints
- future execution integrations must remain disabled by default and isolated from scoring

## Third-Party Integration Plan

### 1. Massive
Status: ACCESS_UNVERIFIED
Purpose: OHLCV/reference data for equity and cross-market coverage.
Responsibility: provider adapter, entitlement review, bar validation.
Risk: incorrect coverage or delayed bars.
Acceptance tests: schema validation, duplicate-bar detection, outage handling.

### 2. Bigdata.com
Status: ACCESS_UNVERIFIED
Purpose: research and catalyst evidence.
Responsibility: evidence retrieval and citation metadata.
Risk: incomplete or stale news context.
Acceptance tests: evidence traceability, source timestamp validation, deduplication.

### 3. PostgreSQL/Supabase
Status: VERIFIED for compatibility review only; not yet configured
Purpose: durable records and migrations.
Responsibility: database schema, migrations, retention, backups.
Risk: inconsistent schema evolution.
Acceptance tests: migration integrity, replay support, audit table design.

### 4. GitHub Actions
Status: VERIFIED for repo checks and CI
Purpose: pull-request checks, validations, and evidence capture.
Responsibility: repository automation and test gate enforcement.
Risk: insufficient validation or hiding failures.
Acceptance tests: unit/integration test workflow run, artifact upload, required checks.

### 5. Existing hosting platform
Status: NOT_CONFIGURED
Purpose: staging and production separation.
Responsibility: environment isolation and release promotion.
Risk: cross-environment contamination.
Acceptance tests: staging access and production guardrails.

### 6. Browserbase
Status: NOT_CONFIGURED
Purpose: optional browser research collection.
Responsibility: controlled browser use for web-based evidence collection.
Risk: web scraping rights violations and inconsistent provenance.
Acceptance tests: source capture with URL/timestamp metadata, no secrets in browser logs.

### 7. Future alert-delivery provider
Status: NOT_CONFIGURED
Purpose: deliver signals and candidate updates.
Responsibility: alert protocol, deduplication, retry policy.
Risk: duplicate or stale alerts.
Acceptance tests: idempotent delivery and lifecycle status updates.

### 8. Clear Street or IBKR
Status: REJECTED for implementation in Phase 0; isolated and disabled by default
Purpose: future execution/portfolio integration.
Responsibility: execution-only service behind an explicit firewall.
Risk: live-order risk and confusion with scoring logic.
Acceptance tests: disabled-by-default flag, zero execution path in scoring and review.

### 9. Options data providers
Status: ACCESS_UNVERIFIED
Purpose: future option chain and derivatives evidence.
Responsibility: contract validation and coverage verification.
Risk: misleading option signals without coverage rights.
Acceptance tests: contract coverage check, schema validation, licensing review.

### 10. Error monitoring and secrets management
Status: NOT_CONFIGURED
Purpose: operational monitoring and secret handling.
Responsibility: telemetry, access control, encryption.
Risk: exposure of operational or account data.
Acceptance tests: no secret values in logs; alerting for provider failures.

## Test Cases
- invalid provider identity is rejected
- schema mismatch blocks the ingest pipeline
- outage triggers retry without false success
- missing entitlement is flagged before data is used

## Acceptance Criteria
- every provider has explicit status and owner
- all provider records include required contract fields
- no integration proceeds with ACCESS_UNVERIFIED or NOT_CONFIGURED status
- future execution integration remains isolated from scoring

## Unresolved Questions
- exact provider roster and pricing/rights
- final database platform selection (Postgres/Supabase or alternative)
- approval process for production alert channels

## Implementation Status
PLANNED. Provider contracts are defined before any integration or implementation work begins.
