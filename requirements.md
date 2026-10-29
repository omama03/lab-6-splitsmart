# SplitSmart Requirements Inventory

**Source**: `UserRequirement.md` (original customer request — read-only, never modified)
**Feature spec**: [`specs/001-splitsmart-mvp/spec.md`](specs/001-splitsmart-mvp/spec.md)
**Constitution**: [`.specify/memory/constitution.md`](.specify/memory/constitution.md)

This document is the requirements inventory required by the project constitution (Principle IX — Requirement Traceability). It preserves **all** original customer features, assigns stable requirement IDs, and distinguishes **customer requirements** from **implementation decisions**. Nothing is dropped; anything not in the first release is labeled **Deferred**.

## Customer Features → Requirement Traceability Matrix

| Customer feature (UserRequirement.md) | Requirement ID(s) | Spec user story | MVP (Java Swing desktop) | Release plan |
|---------------------------------------|-------------------|-----------------|--------------------------|--------------|
| F1. User accounts (own account, log in, own expense & balance info) | FR-001, FR-002 | US3 (P1) | Yes | Release 1 |
| F2. Group management (create/manage groups of people) | FR-003, FR-004 | US4 (P2) | Yes | Release 1 |
| F3. Expense creation (amount, date, description, shared manner, optional receipt; involved users) | FR-005, FR-006, FR-007, FR-008, FR-009 | US1 (P1) | Yes | Release 1 |
| F4. Notification system (notify users of new group expenses) | FR-010 | US8 (P4) | Yes (in-app only) | Release 1 in-app; external email/push **Deferred** |
| F5. Approval workflow (review before balances adjust; single or multiple levels) | FR-011, FR-004 | US6 (P3) | Yes | Release 1 |
| F6. Balance tracking (owed by each user to every other user) | FR-012 | US2 (P1) | Yes | Release 1 |
| F7. Payment tracking (record settlements) | FR-013 | US5 (P2) | Yes | Release 1 |
| F8. Reporting (summaries; custom reports with date ranges/params) | FR-014, FR-015 | US7 (P3) | Yes | Release 1 |
| F9. OS support (primary web-based; Android & iOS apps) | FR-016 | US9 (P4) | No — **Deferred** | Web Release 2; Android/iOS Release 3 |

## Functional Requirement Detail

| ID | Requirement | Source | Acceptance criterion (summary) |
|----|-------------|--------|--------------------------------|
| FR-001 | Each person has their own user account for registering, logging in, and accessing own expense/balance information | F1 | Register → log in → own views only |
| FR-002 | System authenticates users on login with email + password; invalid credentials rejected with clear message; password reset available | F1 | Wrong credential → fail message, stay logged out |
| FR-003 | Users create and manage groups of people (roommates, trips, parties) | F2 | Group CRUD; membership reflected for future expenses; removed member keeps balances |
| FR-004 | Group approval structure configurable: single review or multiple levels | F5 | Pending expenses never adjust balances until final approval |
| FR-005 | Expense contains amount, date, description, shared manner (equal or %), optional receipt image | F3 | All attributes present; absence of receipt does not block |
| FR-006 | Equal split divides expense exactly among participants with documented remainder rule | F3 | 100.00 ÷ 3 sums back to exactly 100.00 (extra cents → first participants in list order) |
| FR-007 | Percentage split validates percentages (exactly 100%) and computes shares by exact percentage arithmetic | F3 | Shares sum exactly to expense amount; ≠100% rejected |
| FR-008 | Creator specifies which other users were involved; balances adjust accordingly | F3 | Only named participants' balances change |
| FR-009 | Invalid expense input rejected (non-numeric, zero, negative amounts, incomplete fields) with specific message | F3 | Distinct actionable message; no balance change |
| FR-010 | Other group members are notified when a member creates a new expense | F4 | Members notified in-app; creator not self-notified; external channels deferred |
| FR-011 | Expenses reviewed and approved before they adjust balances; single or multiple levels | F5 | Pending → zero balance effect; approved → immediate effect; rejected → never |
| FR-012 | Track balance owed by each user to every other user across all entered (approved) expenses | F6 | Pairwise balance matches computed audit result |
| FR-013 | Users record payments to settle balances; balances reflect settlement state | F7 | Full payment → zero; partial → reduced exactly; payment capped at owed balance |
| FR-014 | Reports and summaries of expenses, balances, payments | F8 | Report matches underlying records |
| FR-015 | Custom reports with specified date ranges and other parameters | F8 | Range-filtered report with correct totals |
| FR-016 | Primary web-based delivery plus Android and iOS applications | F9 | **Deferred**: same behaviors on web/mobile in Releases 2–3 |

## Non-Functional Requirements

| ID | Requirement | Source | Notes |
|----|-------------|--------|-------|
| NFR-001 | Monetary accuracy to the smallest unit (cents), no floating-point drift | F3/F6/F7/F8 | Implementation: `BigDecimal` (constitution Principle VIII) |
| NFR-002 | Privacy: no personal data visible outside shared context | F1 | |
| NFR-003 | Approved expense visible in balances within 1 second | F6 | |
| NFR-004 | First-time user records first expense in under 3 minutes | F3 | |
| NFR-005 | Audit trail: actor, timestamp, outcome for expense lifecycle events | F5 | |
| NFR-006 | Attached receipts remain viewable for the life of the expense | F3 | |

## Customer vs. Implementation Decisions

**Customer requirements** (may not be changed by us): features F1–F9 above, as written in `UserRequirement.md`.

**Implementation decisions** (ours, documented, changeable with review): Java 21 + Maven + Java Swing desktop as the Release-1 platform subset; in-app notifications only in Release 1; single-currency per environment; exact round-to-cent arithmetic with a deterministic remainder rule; PlantUML for UML models; JUnit 5 for tests.

## Verification (lab step)

Checked 2026-10-29 for accidental changes against `UserRequirement.md`; all 9 customer features preserved, each with ≥1 requirement ID and ≥1 user story trace. No Java code written. Human review of AI output recorded: Omama (pending final review before submission).