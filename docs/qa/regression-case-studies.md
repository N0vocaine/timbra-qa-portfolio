# Regression Case Studies

These are real defects from Timbra's development, rewritten as short case studies. Each one shows the same workflow: **observe → reproduce → find the root cause → fix → add regression protection → record the lesson.**

Implementation details (file names, function names, version identifiers) are left out on purpose. The reasoning is what matters here.

| # | Area | Classification | Severity | Status |
|---|---|---|---|---|
| 1 | Customer and admin booking pages | Integration defect | Critical | Resolved |
| 2 | Availability engine (admin manual booking) | Functional regression | High | Resolved |
| 3 | Admin day view date picker | Functional regression | Medium | Resolved |
| 4 | Admin calendar rendering | UI/layout regression | Medium | Resolved |
| 5 | Local test environment | Test-environment finding | Low | Open, understood |

Severity scale: see the [Test Strategy](test-strategy.md#9-severity-model).

---

## Case study 1 — Server/browser boundary broke the booking pages

| | |
|---|---|
| **Symptom** | After a deployment, choosing a treatment on the customer booking page returned a **server error**. The admin's manual-booking page failed the same way. The two main entry points into booking were both unusable. |
| **Impact** | Critical. No new bookings could be made through the affected pages. |
| **Found by** | Use of the deployed application. Existing automated tests did not detect it. |
| **Investigation** | Both pages shared the same pattern: a server-rendered page passed a small **function** to an interactive browser component, to build links. |
| **Root cause** | In the framework's server/browser component model, data sent from the server to the browser must be serializable. A plain function isn't. Neither the type checker nor the existing (non-rendering) unit tests could detect this. It only failed at runtime. |
| **Fix** | Replaced the function with plain data that the browser component uses to build the links itself. |
| **Regression protection** | A **static source-scan test** that fails if those components ever declare a function-typed input again, or if the pages pass one. |
| **Lesson** | Some structural rules can't be seen by type checks or logic tests. When that's the case, write a targeted guard against **the exact pattern that broke**, and don't rely on a general rule being remembered. |

## Case study 2 — Admin notice bypass also bypassed the past-time safety floor

| | |
|---|---|
| **Symptom** | In the admin manual-booking flow, times **hours in the past** were offered and could be booked. |
| **Impact** | High. Bookings could be created at times that had already passed. |
| **Found by** | Manual use of the admin manual-booking flow |
| **Investigation** | The admin flow is intentionally allowed to skip the *minimum notice* rule. The investigation looked at what else that skip affected. |
| **Root cause** | There was only **one** time-based filter, and it combined two different rules: *"give the salon enough notice"* (a policy) and *"the time must not have passed"* (a physical fact). Skipping the policy for the admin silently skipped the safety floor too. A subtler edge case followed from the same cause: a slot starting "now" whose **preparation time had already begun**. |
| **Fix** | Split the logic into two separate filters: an **unconditional past-time filter** applied to every caller (measured from the start of preparation), and a separate notice filter that only the admin may skip. |
| **Regression protection** | Unit tests of the new past-time filter, including the exact reported scenario, plus integration tests of availability for **both** admin and customer callers. The [Test Plan](test-plan.md#7-regression-checklist) regression checklist now requires both rules to be checked together. |
| **Lesson** | Two rules that sound similar can have very different safety meanings. Implement and test them **separately from the start**. Also, "a bypass" should be tested for what it must *not* bypass. |

Related test case: [ADM-007](test-cases/booking-engine.md#adm-007-the-admin-still-cannot-book-a-past-time)

## Case study 3 — Admin date picker showed a stale date

| | |
|---|---|
| **Symptom** | In the admin day view, after picking a date manually, the previous / today / next buttons updated the page and the booking list correctly, but the **date field still showed the old date**. |
| **Impact** | Medium. The data was correct, but the display was misleading. |
| **Found by** | Manual use of the admin area after the feature had shipped |
| **Root cause** | The date field was set up to take its value only once, when first shown. The page reused the same component when navigating instead of recreating it, so after a manual change the field never updated again. |
| **Fix** | Made the date field always reflect the current date. Moved the previous / today / next date calculation into a separate, testable function. |
| **Regression protection** | Unit tests of the date navigation, covering fallback to today, month ends and Swedish DST transition dates. |
| **Lesson** | UI state bugs often come from *how* a component is kept alive between navigations. Extracting the logic made it testable, but the original bug was found by a person using the screen. |

## Case study 4 — Calendar text overflow on very short bookings

| | |
|---|---|
| **Symptom** | In the admin calendar, very short blocks (for example a 5-minute gap, only a few pixels tall) showed text that **spilled over into neighbouring blocks**. |
| **Impact** | Medium. The calendar was hard to read in exactly the busy moments where it matters. |
| **Found by** | Manual visual review of the calendar |
| **Root cause** | Block content didn't account for the block's actual height, and nothing clipped content that didn't fit. |
| **Fix** | Added a height-aware rule that chooses how much content a block shows (full / reduced / minimal), plus hard clipping and truncation for short blocks. |
| **Regression protection** | Unit tests of the content-level rule, including a check that a taller block never gets *less* content than a shorter one. The rendered result was re-checked manually. |
| **Lesson** | The **decision logic** can be tested automatically, but whether a 10-pixel block is actually readable still needs a human to look. Automated and manual testing each answer a different question. |

## Case study 5 — Test-environment finding: demo data vs. integration test data

> **This isn't a product defect.** It is included because correctly classifying a failure is a core QA skill.

| | |
|---|---|
| **Symptom** | With local demo data loaded, a small number of database integration tests about working hours failed locally. |
| **Investigation** | The failing tests were checked one by one. All of them failed because of the database's own **overlap protection for working hours**. That is the rule the tests exist to verify. |
| **Root cause** | The demo data and these tests share one local database. The demo data adds a recurring working interval on a particular weekday. The tests add their own temporary interval on the **same weekday** for the same practitioner. The database correctly refuses the overlap. |
| **Classification** | **Test-environment issue.** The product behaved correctly, and the database rule did exactly its job. This isn't a regression, and production data is unaffected. |
| **Current handling** | Documented and diagnosed. The procedure is **diagnose first**: confirm the failures match this known pattern before acting. Resetting demo data before every run was rejected as a habit, because it could hide a genuine new failure that looks similar. |
| **Real fix (planned)** | **Test-data isolation**: integration tests get their own isolated data space, so they never share mutable state with demo data. |
| **Lesson** | When two sources of test and demo data share one mutable database, they will eventually collide. The durable fix is isolation, not carefully picking values. A failing test is a signal to investigate, not automatically a product bug. |

---

## Patterns across these cases

- **The regression test was added together with the fix every time**, never deferred.
- **Protection was added at the lowest reliable layer**: a static guard for a structural issue (1), pure-logic tests for rule and date issues (2, 3, 4), and integration tests where real behaviour mattered (2).
- **All four product defects were found through real use and manual testing**, not by the automated tests that existed then. That is why every fix also added a *new* automated guard: the manual finding became permanent automated protection.
- **Classifying correctly matters**: case 5 is reported honestly as an environment issue. It isn't presented as a product defect or hidden.
