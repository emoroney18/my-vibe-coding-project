# Full-Stack: Data, Access Rules, Edge Cases, Deploy

> Module 5 · Full-Stack. Add data schemas, access rules, and edge cases; stress-test and deploy.

## Deployed link

_The working, shareable link that survives real users._

https://trust-boost-prototypev3build.lovable.app/

## Data schema

| Entity | Key fields | Notes |
|---|---|---|
| profiles (display name, zip) linked to signed-in user. | display name, zip | _____ |
| protection_policies (title, teaser, detail, bullets). | title, teaser, detail, bullets | _____ |

## Access rules

_Who can see / do what? Where are the auth boundaries?_

Public read: providers, slots (available only), verifications, portfolio, quotes, protection policy.
profiles: owner read/update only.
bookings: owner create/read/cancel; cannot set status "confirmed" or change price; totals computed server-side.
experiment_events: insert only, with session id and allowed event names; no reads except admins.
experiment assignments: insert/read own session only.
Reporting function, baseline metrics editing, config: admins only (role check, never client flags).
Every new table gets grants plus RLS in the same change.

## Edge cases hardened

| Case | Before | After |
|---|---|---|
| Empty / first-run state | Slot taken between select and request (double booking) | friendly error, pick another time. |
| Bad / malicious input | No providers / no slots returned | existing empty states driven by real data. |
| Failure / offline | Network or database failure | existing booking error and general error screens. |

## Stress test results

_What you threw at it, and what held / broke._

It asked for me to confirm the email through email, but i never received an email confirmation.
