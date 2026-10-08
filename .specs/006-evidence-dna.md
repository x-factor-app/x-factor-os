# Evidence DNA

Status: DRAFT / PHASE 0

## Purpose
Provide a deterministic, immutable, source-traceable evidence layer that links every candidate and alert to the exact facts that support it.

## Scope
This specification includes:
- source metadata and provenance
- evidence citations
- research and catalyst evidence
- cross-market validation links
- confidence and relevance rules
- evidence versioning

## Inputs
- provider records
- research evidence documents
- corporate actions, news, and catalyst metadata
- candidate trigger metadata
- alert lifecycle events

## Outputs
- evidence references attached to candidates and alerts
- confidence and relevance traces
- evidence rejection or quarantine records

## Dependencies
- provider contracts
- normalized OHLCV
- scoring engine
- candidate ledger

## Data Contracts
Evidence record should include:
- evidence_id
- source_provider_id
- source_url_or_reference
- source_timestamp
- retrieval_timestamp
- symbol_or_subject
- evidence_type
- summary
- confidence_score
- relevance_score
- schema_version
- run_id

## Failure Behavior
- missing provenance: reject or quarantine evidence
- stale evidence beyond policy threshold: mark LOW_RELIABILITY
- duplicate evidence: deduplicate with traceability retained
- unsupported evidence type: route to review queue

## Security Considerations
- evidence references must not store secrets or private account credentials
- tests and logs must keep citations but not embed secrets in URLs or tokens
- public web evidence must preserve source URL and retrieval time

## Test Cases
- evidence without source timestamp is rejected
- duplicate research items remain discoverable but deduplicated by policy
- a candidate cannot reference evidence that lacks provenance
- evidence confidence and relevance are reproduced consistently

## Acceptance Criteria
- every candidate has evidence references
- evidence source and retrieval timestamps are preserved
- evidence can be independently audited
- unsupported or low-confidence evidence is clearly marked and cannot silently influence the final decision

## Unresolved Questions
- final confidence model for research, catalysts, and smart-money evidence
- source weighting for public vs licensed data
- integration with options and derivatives evidence

## Implementation Status
PLANNED. Evidence DNA is required before candidate generation and alerting are allowed to proceed.
