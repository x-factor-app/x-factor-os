# Deterministic Scoring

Status: DRAFT / PHASE 0

## Purpose
Define a deterministic breakout-scoring engine that works from verified data and leaves all weights and thresholds explicitly PROPOSED until validation is completed.

## Scope
This scoring layer covers:
- price momentum and breakout behavior
- relative strength vs peers and index context
- volatility and structure checks
- catalyst salience and evidence weighting
- trend confirmation and risk filters
- candidate generation thresholds

## Inputs
- normalized OHLCV
- quality gate decisions
- cross-market context
- evidence DNA references
- candidate configuration
- scoring version metadata

## Outputs
- candidate score record
- feature values and weights
- pass/fail threshold decision
- scoring metadata and fixture snapshots

## Dependencies
- normalized OHLCV
- data-quality gates
- evidence DNA
- candidate ledger

## Data Contracts
Each scoring pass must record:
- symbol
- timeframe
- scoring_version
- feature_version
- feature_values
- formula_version
- weights
- threshold
- score
- pass_or_fail
- missing_feature_policy
- run_id
- created_at
- source_bar_window

### Proposed Feature Set
The following features are PROPOSED and unvalidated until backtesting/verification is completed.

1. Breakout strength
   - Formula: normalized momentum from recent price vs reference range
   - Weight: PROPOSED 18%
   - Missing-data behavior: fail feature, do not default to zero unless explicitly approved by policy
   - Threshold: PROPOSED 0.70

2. Relative strength
   - Formula: symbol return minus market or sector return over the evaluation window
   - Weight: PROPOSED 15%
   - Missing-data behavior: require peer market context or mark feature missing
   - Threshold: PROPOSED 0.60

3. Volume confirmation
   - Formula: volume ratio relative to rolling average with price participation filter
   - Weight: PROPOSED 14%
   - Missing-data behavior: mark missing if volume is absent or untrusted
   - Threshold: PROPOSED 0.65

4. Trend structure quality
   - Formula: slope consistency across short-, medium-, and long-term windows
   - Weight: PROPOSED 13%
   - Missing-data behavior: fail feature if insufficient bars
   - Threshold: PROPOSED 0.55

5. Catalyst salience
   - Formula: weighted event priority and evidence score across event classes
   - Weight: PROPOSED 12%
   - Missing-data behavior: absent catalysts count as neutral only if documented
   - Threshold: PROPOSED 0.50

6. Risk posture and volatility regime
   - Formula: volatility-adjusted risk filter and position sizing context
   - Weight: PROPOSED 10%
   - Missing-data behavior: if volatility is missing, reject candidate generation
   - Threshold: PROPOSED 0.40

7. Cross-market context
   - Formula: sector/industry/market breadth alignment
   - Weight: PROPOSED 10%
   - Missing-data behavior: absent context counts as neutral, not bullish
   - Threshold: PROPOSED 0.45

8. Fundamental and positioning evidence
   - Formula: weighted evidence class score with provenance requirement
   - Weight: PROPOSED 8%
   - Missing-data behavior: missing evidence does not create a positive score
   - Threshold: PROPOSED 0.35

## Formula and Weighting Notes
- Weights are PROPOSED only and must not be described as validated predictive parameters.
- The system must preserve both the raw feature vector and the scoring version used to compute it.
- The score must remain deterministic with fixed inputs and fixed version metadata.

## Failure Behavior
- candidate generation blocked when required features are missing
- null or malformed values are not silently converted to zero unless policy explicitly says so
- conflicting sources on the same feature must be quarantined for manual review
- invalid score or threshold may not produce a candidate

## Security Considerations
- scoring must not access live brokerage accounts or execution systems
- scoring logic must be pure and deterministic from approved inputs
- score snapshots must be immutable and reproducible from fixtures

## Deterministic Test Fixture
A deterministic fixture must include:
- fixed sample bars for at least one valid breakout case
- a missing-data case
- a noisy-market case
- a provider outage case
- a duplicate bar case
- a case where a high score is not produced because one required feature is missing

## Test Cases
- same input produces same score and ledger record
- candidate is rejected when required feature fields are absent
- missing-data handling is explicit and repeatable
- versioned weights and formulas are reproducible from fixture metadata

## Acceptance Criteria
- each feature has a formula, weight, missing-data rule, threshold, and version
- weights are labeled explicitly PROPOSED
- no predictive accuracy claim is made in Phase 0
- all score outputs are reproducible from deterministic fixtures

## Unresolved Questions
- final feature list after backtesting and provider coverage review
- threshold tuning requirements across markets and timeframes
- whether to include derivative/option signals as a separate feature class

## Implementation Status
PLANNED. Scoring specification is deterministic and versioned, but all weights and thresholds remain PROPOSED pending validation.
