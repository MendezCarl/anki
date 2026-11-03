# Issue #5380: test case cross-reference

The following regression cases are implemented in the FSRS simulator tests in
[`simulator.rs`](../rslib/src/scheduler/fsrs/simulator.rs).

## Card conversion

| Test case                                             | Test                                                                                                       | Coverage                                                     |
| ----------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| Future review-card last date is clamped               | [`review_card_future_last_date_is_clamped_to_zero()`](../rslib/src/scheduler/fsrs/simulator.rs#L401)       | Relative due date and non-positive `last_date`               |
| Past review-card last date is preserved               | [`review_card_keeps_past_last_date()`](../rslib/src/scheduler/fsrs/simulator.rs#L410)                      | Negative calculated `last_date` remains unchanged            |
| Day-learning card uses relative due and interval      | [`day_learning_card_uses_relative_due_and_interval()`](../rslib/src/scheduler/fsrs/simulator.rs#L418)      | Relative due date, interval, and `last_date`                 |
| Learning and buried cards have zero due and last date | [`learning_and_buried_cards_have_zero_due_and_last_date()`](../rslib/src/scheduler/fsrs/simulator.rs#L428) | `Learn`, `SchedBuried`, and `UserBuried` queue conversion    |
| Unsupported queues are not converted                  | [`unsupported_queues_are_not_converted()`](../rslib/src/scheduler/fsrs/simulator.rs#L442)                  | `New`, `PreviewRepeat`, and `Suspended` queues return `None` |
| Filtered review card uses original due                | [`filtered_review_card_uses_original_due()`](../rslib/src/scheduler/fsrs/simulator.rs#L456)                | Relative due and `last_date` use the original due date       |

## Request-to-config conversion

| Test case                                                  | Test                                                                                                            | Coverage                                                                      |
| ---------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Missing memory state falls back to SM-2                    | [`missing_memory_state_falls_back_to_sm2()`](../rslib/src/scheduler/fsrs/simulator.rs#L470)                     | Existing card is converted with state derived from its SM-2 ease and interval |
| Invalid SM-2 fallback drops the card                       | [`card_with_invalid_sm2_fallback_is_dropped()`](../rslib/src/scheduler/fsrs/simulator.rs#L487)                  | Invalid fallback excludes the card without failing config creation            |
| Missing memory state populates desired retention and decay | [`missing_memory_state_populates_desired_retention_and_decay()`](../rslib/src/scheduler/fsrs/simulator.rs#L501) | Stored card values match its deck configuration and FSRS parameters           |
| Request learning-step counts are preserved                 | [`request_learning_step_counts_are_preserved()`](../rslib/src/scheduler/fsrs/simulator.rs#L522)                 | Config retains requested learning and relearning step counts                  |
| New-card due offsets account for cards introduced today    | [`new_card_due_offsets_account_for_cards_introduced_today()`](../rslib/src/scheduler/fsrs/simulator.rs#L538)    | Synthetic new-card due dates account for introductions already made today     |

## Acceptance checks

- [x] The scenarios above are covered by the simulator test module.
- [x] Tests pass with the current implementation (11 passed).
- [x] Removing the `.min(0)` clamp from `last_date` makes
  `review_card_future_last_date_is_clamped_to_zero()` fail.
- [x] Replacing the SM-2 fallback with dropping cards makes
  `missing_memory_state_falls_back_to_sm2()` fail.
- [x] Tests use explicit inputs and inspect converted data; they do not depend on
  RNG-based simulation output.

## Related pull requests

- [Fix/last date of existing card should not be positive in simulation #3667](https://github.com/ankitects/anki/pull/3667)
- [Fix/FSRS simulator fallback to memory_state_from_sm2 when converting cards #4189](https://github.com/ankitects/anki/pull/4189)
- [Make simulator fill missing values of DR and decay too #4269](https://github.com/ankitects/anki/pull/4269)
- [Fix/use real step count to simulate #4240](https://github.com/ankitects/anki/pull/4240)
