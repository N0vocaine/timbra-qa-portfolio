# Risk Register

Risks drive how much testing each area gets. Each risk is classified by **type**, because not every risk is a product defect risk.

| Type | Meaning |
|---|---|
| **Product risk** | Could harm customers, the business or data if it happened |
| **Test-infrastructure risk** | A weakness in how well we can *detect* problems, not a known product problem |
| **Production-readiness limitation** | A capability not yet live; a known scope boundary, not a failure |

## Register

| ID | Risk | Severity | Type | How it's mitigated and tested | Residual gap |
|---|---|---|---|---|---|
| **RISK-001** | Double booking / overlapping bookings | Critical | Product | Occupied-interval collision logic (unit); server re-check before saving; **database-level overlap rejection**; real concurrent-request test; cancellation releases the time | None identified: enforced by the database itself and tested directly |
| **RISK-002** | Wrong time conversion around daylight-saving changes | Critical | Product | Explicit conversion with non-existent/ambiguous times rejected; boundary tests at real transition dates; week navigation across DST | None identified at the logic layer |
| **RISK-003** | Booking offered or created in the past | High | Product | Separate, unconditional past-time filter for every caller, including the admin; regression tests for a real defect | The thin admin form layer itself has no dedicated automated test |
| **RISK-004** | Unauthorized admin access | Critical | Product | Two independent gates; allow-list authorization; fail-closed configuration; static guard that every admin action is protected | None identified. Not a substitute for a formal security review. |
| **RISK-005** | Secrets leaking into browser code | Critical | Product | Server-only modules; project-wide static scan of all browser-side files | None identified |
| **RISK-006** | Notification duplicated or lost | Medium | Test-infrastructure | Safe job claiming under concurrency; retry-then-fail rule; a uniqueness rule against duplicate jobs | The uniqueness rule is exercised only indirectly (no test deliberately creates a duplicate) |
| **RISK-007** | Calendar view regression (Day / Week / Agenda) | Medium | Test-infrastructure | Layout and navigation logic automated; Day and Week visually verified at three widths | No rendered-UI automation; **Agenda has no separately recorded visual check** |
| **RISK-008** | Customer booking flow broken in the browser | High | Test-infrastructure | Every rule behind the flow automated; manual end-to-end verification | **No browser E2E automation**, so manual evidence is not continuously refreshed |
| **RISK-009** | Test-environment contamination (demo data vs. test data) | Low | Test-infrastructure (not a product risk) | Diagnosed and documented; a diagnose-first procedure instead of blind resets | Real test-data isolation not yet implemented |
| **RISK-010** | Real notification delivery not yet available | High (business readiness) | Production-readiness limitation | The pipeline is tested up to the delivery provider; admin-created bookings are proven not to notify | Real email/SMS delivery and automatic scheduling are future work |

## How the register is used

- **Test depth.** Each Critical risk has at least two independent layers of automated protection, such as logic *and* database.
- **Release decisions.** The [Test Plan](test-plan.md) prioritises P0 checks directly from the Critical and High risks.
- **Defect triage.** A new failure is linked to a risk ID first, which sets its urgency.
- **Honest reporting.** RISK-009 and RISK-010 are deliberately **not** described as defects. One is a test-environment condition; the other is a feature not yet live. Mixing these up with product defects would make both the risk picture and the quality picture wrong.

## Related

- [Traceability examples](traceability-examples.md): risks linked to requirements, cases and evidence
- [Known limitations & future QA](known-limitations-and-future-qa.md): the plan for the residual gaps
