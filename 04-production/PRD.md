# Living PRD

> Module 4 · Production Specs. Refactor for readability; extract a living PRD that stays true as the build evolves.

## Problem

_What user problem does this solve? Tie to the validated hypothesis._

tied to the validated hypothesis: first-booking rate for zero-review providers is 2.3% vs 14% reviewed; the prototype tests whether visible verification + early proof lifts first bookings. Include the two buyer quotes as problem evidence and the −4% QoQ / 68% exit metrics as business context.

## Users & jobs

- **Primary user:** primary user: the buyer browsing home-service providers with no track record.
- **Job to be done:** Job to be done: commit money to a new provider with confidence. Secondary: new zero-review providers (Maya as the case) who need to win a first booking.

## Scope

- **In:** In: the five screens and all edge states the prototype covers (search + empty state, profile loading, trust panel + verification detail, availability/no-dates, booking review/edit, booking error + retry, general error, evidence dashboard with date ranges, control/treatment toggle).
- **Out (explicitly):** Out: real payments, real booking persistence, real provider verification pipeline, multi-provider marketplace, accounts/auth.

## Requirements

| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 1 | R1 — Trust panel on new-provider profiles | Must | verified identity, background check, experience, portfolio, and explicit "No reviews yet" on the treatment profile, with the control state free of verification detail. Acceptance criteria: all four proofs render with check dates; each proof row opens a detail view stating what was checked, what it means, and what it doesn't guarantee; control/treatment toggle switches the panel; booking remains disabled until a time is selected. |
| 2 | R2 — First-booking lift is measurable (MUST): | Must | the evidence dashboard recomputes metrics per selected date range and shows control vs. treatment booking rate with lift and a reactive kill switch. Acceptance criteria: switching Last 7 days / Last 30 days / This quarter updates every metric card, comparison bars, and the kill-switch state (on track at ≥ 20% lift, pivot below); buyer quotes remain on screen; all numbers are deterministic and labeled as simulated prototype data. |

## Data & events

_What gets stored, what gets tracked._

the prototype's tracked events (profile viewed, booking started, booking completed) and the simulated dashboard data model (metrics per date range, control vs treatment, lift, kill-switch threshold ≥ 20%).

## Open questions

e.g. how verification is actually performed, whether trust-panel proof types generalize to other services, sample size needed to call the experiment, what happens after First Booking Protection claims.
