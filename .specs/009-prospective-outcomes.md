# Prospective Outcomes

Status: DRAFT / PHASE 0

## Purpose
Define the accounting and evaluation rules for prospective outcomes without look-ahead bias. This specification prevents premature success or failure claims and preserves uncertain observations as OPEN or NOT_YET_MATURE.

## Scope
This specification covers:
- evaluation horizons
- trade/position outcome accounting
- stop and target handling
- open observations and unresolved positions
- missing market windows and data gaps
- conservative treatment of ambiguous exits

## Inputs
- candidate ledger and target values
- execution or review events
- time-based evaluation windows
- market data and bar history
- source timestamps and review timestamps

## Outputs
- prospective outcome records
- status classification (SUCCESS, FAILURE, OPEN, NOT_YET_MATURE, INVALID)
- review-ready metrics such as MFE and MAE

## Dependencies
- candidate ledger
- alert lifecycle
- performance review
- normalized OHLCV

## Data Contracts
Outcome record should include:
- outcome_id
- candidate_id
- entry_time
- exit_time
- stop_price
- target_price
- actual_exit_price
- outcome_status
- evaluation_horizon
- reason_code
- run_id

## Failure Behavior
- observation with no exit yet: status = OPEN or NOT_YET_MATURE
- ambiguous stop/target event: classify conservatively and document reason
- missing data during the evaluation window: do not convert to success/failure without documented rule
- look-ahead violation: reject the outcome and quarantine review

## Security Considerations
- prospective review must stay separate from live execution
- outcome records must never imply an executed trade unless execution data exists
- only approved data sources may be used for outcome evaluation

## Test Cases
- unresolved candidate remains OPEN
- candidate outside evaluation horizon remains NOT_YET_MATURE
- ambiguous stop/target event is not treated as a success or failure without clear rules
- data gap prevents outcome accounting from being marked as complete

## Acceptance Criteria
- no success/failure claims are made before the observation is complete
- all incomplete outcomes remain OPEN or NOT_YET_MATURE
- decision windows are explicit and fixed before evaluation begins
- look-ahead bias is prevented by design

## Unresolved Questions
- final evaluation horizon definitions by timeframe and strategy class
- interpretation of partial fills and early exits
- required outcome window for research vs production deployment

## Implementation Status
PLANNED. Prospective outcome accounting is required before any performance claims or release decisions are made.
