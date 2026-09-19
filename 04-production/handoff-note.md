# Engineering Handoff Note

> Module 4 · Production Specs. Open the black box, make the build legible to an engineer.

## What this is

_One paragraph an engineer can read in 60 seconds._

This is a clickable, front-end-only experiment prototype for a home-services marketplace. It tests one hypothesis: showing a trust panel (four verification proofs plus First Booking Protection) on a zero-review provider's profile increases first bookings. The whole buyer journey — search → provider profile (control vs. trust-panel variant) → booking request with review/edit → confirmation → experiment evidence dashboard — runs in the browser as a single route (/) driven by a small state machine in src/routes/index.tsx. There is no backend, no database, no auth, and no real analytics: every provider, verification status, date, and dashboard number is hardcoded simulated data. Edge states (loading, empty search, unavailable date, no availability, booking failure, general error) are all implemented and reachable both through real interaction and through a "Prototype state" dropdown pinned bottom-right. Stack is TanStack Start (React 19 + Vite 7) with Tailwind v4 and shadcn/ui.

## Architecture (plain language)

- **Frontend:** One route (src/routes/index.tsx) exports PrototypeApp, which holds all journey state (screen, treatment, booked, date, bookingPhase, availabilityMode, failNextBooking, searchEmpty, errorReturn) and renders exactly one screen component at a time. Screens are grouped by feature:  src/features/   search/       SearchResults, SearchEmptyState, SearchResultsList, ProviderCard   provider/     ProviderProfile, ProviderProfileLoading, TrustPanel,                 VerificationDetail, ProofChip   booking/      BookingRequest, BookingSummary, BookingNoAvailability,                 BookingConfirmation   experiment/   ExperimentEvidenceDashboard   shared/       GeneralError, ScenarioControl, NavButton, types.ts src/data/   providers.ts     Maya's profile, other providers, pricing, search defaults   availability.ts  date options incl. the deliberately unavailable date   verification.ts  four proofs + First Booking Protection copy   experiment.ts    per-date-range dashboard data, hypothesis,
- **Backend / data:** None. src/data/* are plain typed constants imported by components. No server functions, no fetch calls, no persistence — refreshing the page resets the journey.
- **Key flows:** Search — the service/location inputs are live; a match requires "clean" in the service and Brooklyn/11217 in the location. Anything else renders the recoverable empty state.
Profile — opening Maya's profile passes through a 900 ms loading placeholder. A Control / Trust panel toggle switches the experiment variant; the trust panel's four proof rows expand into detail views ("what Hearth checked" / "what it doesn't guarantee").
Booking — two phases: select a time, then review (with Edit/Back). The Sep 18 date is intentionally unavailable and surfaces the nearest opening. availabilityMode === "none" renders the no-availability screen. Requesting takes 700 ms and either confirms or, when the error scenario is armed, shows an inline failure that preserves the booking and retries successfully.
Evidence — a dashboard with three date ranges; every metric, the control-vs-treatment bars, the lift figure, and the kill-switch verdict recompute from evidenceByRange. Lift ≥ 20% reads "on track", otherwise "pivot".

## What's solid vs. what's duct tape

| Area | State | Notes |
|---|---|---|
| Typecheck is clean; the full journey and every scenario were verified in a real browser with no console errors. | solid | _____ |
| The "Prototype state" dropdown is demo scaffolding shipped in the UI and must be removed before any real user test. | rough | _____ |

## Risks & assumptions for the team

Measurement is simulated. Nothing here proves the hypothesis. Real validation needs event instrumentation (profile viewed, variant exposed, booking started, first booking completed) and an actual assignment mechanism.
Assumed 20% lift threshold as the continue/pivot rule — a product decision, not a statistical one. No sample-size or significance logic exists.
Verification claims are legally sensitive. The copy describes real checks (ID, criminal background, experience, portfolio) and a refund guarantee. Shipping these requires actual vendors, an operational claims process, and legal review of the wording.
First Booking Protection implies a payout liability with no cost model behind it.
Single-variant test. The control is "no trust panel at all"; the prototype can't isolate which of the four proofs drives any effect.
No auth, payments, provider onboarding, messaging, or cancellation flows — all out of scope and all needed for a real booking.
Content is fixed to Brooklyn home cleaning; another category or geography means new data and imagery.

## How to run it

```
bun install      # or npm install
bun run dev      # dev server on http://localhost:8080
bun run build    # production build
bun run lint     # eslint
Node 20+ (or Bun). No environment variables and no services are required — the app runs fully offline after install. Open /, then use the header toggle for the evidence dashboard and the bottom-right "Prototype state" dropdown to jump straight into loading, empty-search, unavailable-date, no-availability, booking-error, and general-error states.
```
