# Test Strategy

This document explains **how and why** Timbra is tested. It changes rarely. The [Test Plan](test-plan.md) describes **what** is executed in a specific validation cycle.

## 1. Quality objectives

These objectives are tied to what V1 actually does:

1. **No double bookings**, enforced by the database and not only in application code.
2. **No bookings in the past**, for any user, including the admin.
3. **Correct times around daylight-saving changes** (Europe/Stockholm), including local times that don't exist or occur twice.
4. **A deterministic, explainable recommendation.** Ranking must never make an invalid time bookable or a valid time unbookable.
5. **A protected admin area**, with authorization checked at two independent layers.
6. **Secrets and private links kept out of the browser.**
7. **Customer choice preserved.** The recommendation never hides valid times.

## 2. Risk-based approach

Effort follows risk, not an equal spread across all code. The full register is in the [Risk Register](risk-register.md). Summary:

| Severity | Risks |
|---|---|
| **Critical** | Double booking · Wrong time conversion around DST · Unauthorized admin access · Secret leakage to the browser |
| **High** | Booking offered or created in the past · Customer booking flow broken in the browser · Real notification delivery not yet available |
| **Medium** | Notification duplicated or lost · Calendar view (Day/Week/Agenda) regression |
| **Low** | Test-environment contamination (demo data vs. test data) |

The Critical risks get **multi-layer automated coverage**: pure logic tests *and* real database tests, not one or the other.

## 3. Test levels

This is Timbra's **actual** testing model today, not an idealized one:

```
              ┌───────────────────────────────────────────┐
  Manual      │ Manual functional, visual & responsive QA │  ← browser journeys, calendar layouts,
              ├───────────────────────────────────────────┤    post-deployment smoke checks
  (gap)       │   Browser E2E automation: not yet built   │
              ├───────────────────────────────────────────┤
              │ Authentication / authorization integration│
  Automated   │ Real-database integration tests           │  ← constraints, atomicity, concurrency
              │ Pure unit & domain-logic tests            │  ← largest layer: eligibility, ranking,
              └───────────────────────────────────────────┘    time zones, cutoffs, validation
  Alongside all layers: static regression & security guards (source-scanning tests)
```

- The **base is wide**: pure, deterministic tests of business rules run in milliseconds.
- **Database integration tests run against a real local PostgreSQL database, not mocks.** The most important guarantees (no overlapping bookings, atomic booking creation, safe concurrent requests) are enforced by the database. Only a real database can prove them.
- **Static guards** scan the source code for dangerous patterns, such as secret-handling code in browser code or an admin action with no authorization check.
- **The top layer, browser automation, doesn't exist yet.** Manual testing covers it today. This is the clearest QA-infrastructure gap, and it is stated openly.

Details of each type: [Test types & approach](test-types-and-approach.md).

## 4. What gets automated first

Automate first and most where a rule is **deterministic** and the risk is **high**:

- Booking duration maths (preparation, treatment, buffer)
- Eligibility and ranking, including the rule that ranking never changes eligibility
- Timezone and DST conversion
- Collision and concurrency behaviour
- Database constraints and database-side booking logic
- Authentication and authorization
- Cancellation boundaries (before, at and after the cutoff)
- Notification *job* logic (creation, claiming, retry), but not real delivery
- Every defect whose reproduction is deterministic (see §7)

**Not automated yet:** full browser journeys, rendered UI appearance, cross-browser behaviour and accessibility. These aren't automated because the tooling isn't in place yet, not because they're unimportant.

## 5. Manual testing

Manual testing covers what automation can't reach yet:

- The customer's end-to-end booking journey in a real browser
- Admin sign-in and general admin use
- The Day, Week and Agenda calendars as they actually render
- Responsive layouts at desktop (~1440 px), tablet (~768 px) and mobile (~375 px) widths
- Horizontal scrolling in the Week view on narrow screens
- Readability of very short bookings in the calendar
- Smoke checks after a production deployment

**Rule: a manual result is recorded as PASS only when there is real evidence for it.** For example, the Day and Week views have recorded manual verification across all three widths. The Agenda view does **not** yet have a separate recorded visual round, so it is listed as a gap, not assumed to pass.

## 6. Special focus areas

### Database
Tested against a real local database: booking overlap protection, working-hour overlap protection, schedule-exception conflicts, atomic booking creation (a failed booking leaves nothing behind), and simultaneous booking requests (exactly one wins).

Some database rules exist without a test that deliberately triggers the violation. They are named as gaps in [Known limitations](known-limitations-and-future-qa.md).

### Time and daylight saving
Tested with the current time injected explicitly:

- Winter (UTC+1) and summer (UTC+2) conversion
- Spring-forward local times that **don't exist** are rejected, not guessed
- Autumn local times that **occur twice** are rejected, not guessed
- Date arithmetic across month, year and DST boundaries, independent of the server's own timezone
- Minimum-notice and cancellation boundaries measured in **real elapsed time**, not clock-face time

Manually clicking through a booking at 02:30 on a DST Sunday isn't practical or repeatable. An automated test that injects the exact instant is.

### Security
Tested areas: admin sign-in requirement, allow-list authorization, fail-closed behaviour when authorization configuration is missing, independent protection of every admin action, server-only secret handling, no private tokens or personal data on public pages, and identical responses for invalid and unknown manage links.

**These are not a substitute for a formal security review. No penetration test has been performed.**

### UI and calendars
- **Automated:** view selection, date and week navigation (including across DST), layout calculations (positions, heights, content levels for short bookings).
- **Manual:** actual appearance, readability, clipping and overflow, horizontal scrolling, responsive behaviour.

### Notifications
Two things are kept strictly separate:
- **Tested:** job creation, reminder scheduling, no notifications for admin-created bookings, safe job claiming, retry and failure handling, and message content in both languages (including no personal data in SMS).
- **Not production-ready, and not claimed as tested:** real email/SMS delivery and an automatic scheduler.

## 7. Regression approach

> **A defect fix adds regression protection at the lowest reliable layer that would have caught it.**

- A logic defect gets a pure-function test.
- A database behaviour defect gets a real-database integration test.
- A structural defect that no runtime test can see gets a static source-scan guard.

In every resolved case, the regression test was added **together with the fix**, not later. Worked examples: [Regression case studies](regression-case-studies.md).

## 8. Defect workflow

```
Detect → Reproduce → Classify (product defect / test defect / environment issue / known limitation)
  → Assess severity → Link to requirement & risk → Fix
  → Add regression test at the lowest reliable layer
  → Run focused tests → Run broader regression → Manual check if UI-related
  → Document significant incidents
```

**A failing test isn't automatically a product defect.** Classifying failures correctly is part of the job. See the test-environment case study in [Regression case studies](regression-case-studies.md#case-study-5--test-environment-finding-demo-data-vs-integration-test-data).

## 9. Severity model

| Severity | Meaning |
|---|---|
| **Critical** | Security or data-integrity failure, or a core booking failure with major impact |
| **High** | Major booking or admin behaviour broken, with no reasonable workaround |
| **Medium** | Important but contained functional or UI issue |
| **Low** | Minor, cosmetic, or test/developer-experience issue |

Severity describes **impact**, not the order in which fixes are made.

## 10. Test data principles

- Prefer self-contained, deterministic fixtures. Most logic tests need no external data at all.
- Tests must not depend on whether demo data happens to be loaded.
- Only synthetic data is used, for example reserved non-routable email domains for demo customers. No real customer data or secrets appear in tests or QA documents.
- **Production data is never reset or reseeded as part of testing.**

## 11. Entry and exit criteria

**Entry:** the change is scoped; affected requirements and risks are identified; the environment and test data state are known; known environment issues are documented beforehand, so they aren't mistaken for new failures.

**Exit:** the relevant automated tests have run and any failure is explained; no unresolved Critical defect; any High defect is explicitly reviewed and accepted in writing; required manual checks are done with evidence; relevant gaps are documented.

## 12. Current automation snapshot

**500+ automated tests** (Vitest). Most need no infrastructure at all; the database integration tests run against a real local PostgreSQL database.

The exact number changes as the project grows. Coverage of the risks matters more than the count.

## 13. CI/CD status

There is no CI pipeline yet, so the automated suite does **not** currently block a deployment. Adding a CI quality gate (lint, type check, automated tests) is a planned improvement. See [Known limitations](known-limitations-and-future-qa.md).

## Related documents

[Test Plan](test-plan.md) · [Test types & approach](test-types-and-approach.md) · [Test cases](test-cases/README.md) · [Risk Register](risk-register.md) · [Traceability examples](traceability-examples.md) · [Regression case studies](regression-case-studies.md)
