# Clarification Log — SplitSmart

**Spec reviewed**: `UserRequirement.md` (authoritative customer request) vs. `specs/001-splitsmart-mvp/spec.md`
**Date**: 2026-10-29
**Rule applied**: No answer was silently invented; every decision below was confirmed by the customer representative before the specification was updated.

| # | Topic | Question | Decision (confirmed 2026-10-29) | Spec section updated |
|---|-------|----------|--------------------------------|----------------------|
| 1 | Payer vs. participants | Are payer and participant sets independent? | Payer and participants are set independently; the payer may include themselves as a participant; nobody ever owes themselves | Actors; FR-008; Edge Cases |
| 2 | Group membership | May expenses involve non-members? What happens to a removed member's balances? | Expenses may involve any registered user; a removed member keeps past balances and is excluded from that group's future participant selection | FR-003; Edge Cases |
| 3 | Rounding | How is an equal split that does not divide into exact cents handled? | Round each share half-up to cents; leftover cents go to the first participants in participant-list order until exhausted | FR-006; Edge Cases |
| 4 | Invalid amounts | How are zero/negative/non-numeric amounts handled? | Rejected at entry with a specific message; amounts require exactly two decimal places | FR-009; Edge Cases |
| 5 | Percentage totals | Must percentages sum to exactly 100%? | Yes — exactly 100% at submission; otherwise rejected with a clear message | FR-007; Edge Cases |
| 6 | Authentication | Which login method? | Email + password; session until logout; password reset available | FR-002; Actors |
| 7 | Notifications | Delivery channel for the first release? | In-app notification inbox only; email/push deferred to a later release | FR-010; NFR; Assumptions |
| 8 | Balance settlement | May payments exceed owed balances? Partial? | Partial payments allowed; a payment cannot exceed the currently owed pairwise balance | FR-013; Edge Cases |

**Traceability note**: Decisions 3, 4, 5 are implementation-consistent with constitution Principle VIII (exact decimal arithmetic for money); decisions 6 and 7 are documented as Release-1 scopes in `requirements.md`.