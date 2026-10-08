# Performance Review

Status: DRAFT / PHASE 0

## Purpose
Describe how the system evaluates prospective performance without turning speculative or unvalidated scoring into claims of predictability. The review is a governance and learning mechanism, not a production signal source.

## Scope
This review covers:
- retrospective and prospective analysis
- MFE/MAE accounting
- segmentation by interval, market, and symbol class
- outcome quality review
- release gate findings

## Inputs
- current and historical candidate ledger entries
- outcome records
- scoring versions and fixture metadata
- alert delivery history
- release review notes

## Outputs
- performance summary report
- review notes and release recommendation
- risk and quality flags

## Dependencies
- candidate ledger
- prospective outcomes
- alert lifecycle
- security and governance policy

## Data Contracts
Performance review record should include:
- review_id
- review_window
- scoring_version
- market_scope
- sample_count
- success_count
- failure_count
- open_count
- not_yet_mature_count
- mfe_summary
- mae_summary
- review_status

## Failure Behavior
- no comparison of unvalidated score versions as if they were equivalent
- incomplete or ambiguous observations remain marked OPEN or NOT_YET_MATURE
- underpowered sample sizes trigger a HOLD or NO-RELEASE recommendation
- release recommendations must cite the specific evidence and review window

## Security Considerations
- performance review artifacts should not contain secrets or live brokerage details
- review summaries must avoid claims beyond the current evidence base
- all review data must be tied to versioned score and pipeline metadata

## Test Cases
- review of a new scoring version is clearly separated from prior versions
- underpowered samples generate a HOLD recommendation
- periods with open and not-yet-mature outcomes are not reported as success/failure ratios

## Acceptance Criteria
- review output is version-specific and reproducible
- no predictive accuracy claim is reported without evidence
- open and not-yet-mature results are preserved as such
- release recommendations are conservative and evidence-based

## Unresolved Questions
- exact minimum sample size before performance review becomes informative
- treatment of sector and cross-market review slices
- thresholds for controlled release and rollback

## Implementation Status
PLANNED. Performance review is required before controlled release or public claims of performance.
