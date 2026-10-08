# Known Limitations and Future QA Work

Knowing what isn't covered is part of QA. These items are **engineering and QA maturity opportunities** for a pilot product (V2, release v2.1.0). None of them is a known, hidden product defect.

## 1. Current limitations

| # | Limitation | What it means today | Current mitigation |
|---|---|---|---|
| L1 | **No browser end-to-end automation** | Full customer and admin journeys aren't re-checked automatically on every change | All rules *behind* the journeys are automated; the journeys themselves are tested manually |
| L2 | **No rendered UI component tests** | What the screen actually looks like isn't machine-checked | Layout *calculations* are automated; appearance is checked manually at three widths |
| L3 | **No CI test gate** | The automated suite doesn't yet block a deployment | The suite is run manually before behaviour-changing releases, following the [Test Plan](test-plan.md) |
| L4 | **No dedicated staging environment** | There is no permanent staging site | Releases are tested on an isolated test database, deployed to the demo first as a release candidate, then to production, and verified on both; production data is never reset |
| L5 | **Real notification delivery not yet production-ready** | Confirmation and reminder messages are created and managed correctly, but real email/SMS delivery and automatic scheduling aren't live | The pipeline is tested up to the delivery provider. Delivery is not claimed as tested. |
| L6 | **Test-data isolation** | During development, local demo data and some integration tests could share one database and collide ([case study 5](regression-case-studies.md#case-study-5--test-environment-finding-demo-data-vs-integration-test-data)). Release runs use an isolated, throwaway database. | Diagnose-first procedure. A dedicated, separate test database for all database-backed tests is implemented and in review (October 2026), not yet merged. |
| L7 | **Some database rules lack a direct violation test** | A few integrity rules (e.g. preventing deletion of records still referenced by bookings, and duplicate-notification prevention) exist but aren't deliberately triggered by a test | Exercised indirectly; low exposure, because the application offers no path that would violate them |
| L8 | **No separately recorded visual check for the Agenda view** | Day and Week views have recorded manual verification at three widths; Agenda doesn't yet | Agenda uses the same underlying data as the Day view, which is automated |
| L9 | **Limited accessibility, no cross-browser or performance testing** | Colour contrast of the theme is checked automatically, and keyboard focus and accessible names were checked manually for key pages; there is no screen-reader, cross-browser or performance evidence | Manual use in a single modern browser |
| L10 | **No formal security review** | Security properties are tested individually, but no penetration test has been done | Two-layer authorization, fail-closed configuration, static secret guards |
| L11 | **Salon-owner validation pending** | The released V2 behaviour (v2.1.0) is approved by the product owner but not yet validated by the salon owner | Tracked as a separate milestone; behaviour stays adjustable |
| L12 | **Two known issues in v2.1.0** | On short admin calendar cards, preparation is visible only as a hatched strip without text. A customer who tries to cancel an appointment that has already started sees the "less than 24 hours left" message instead of a message saying it has started. | Cancellation is correctly refused in both cases. Fixes are implemented and in review (7 October 2026), **not released**. |

## 2. Planned QA improvements (roadmap)

Prioritised by risk reduction per unit of effort:

| Priority | Improvement | Risk reduced | First step |
|---|---|---|---|
| 1 | **CI quality gate**: lint, type check and the automated suite before deployment | Regressions reaching production unnoticed | Run the infrastructure-free tests in CI first, then add a database service for the integration tests |
| 2 | **Test-data isolation** for integration tests | False failures hiding real ones (RISK-009) | Give integration tests their own isolated data space |
| 3 | **Browser E2E smoke suite** (e.g. Playwright) for the customer happy path and cancellation | Customer flow broken in the browser (RISK-008) | Automate one journey: treatment → date → recommended time → confirm → cancel |
| 4 | **Staging environment** mirroring production | Integration issues only visible outside local | A separate environment fed the same database changes and smoke checks |
| 5 | **Direct violation tests** for the remaining database rules (L7) | Silent loss of an integrity guarantee | One targeted test per rule |
| 6 | **Agenda visual check** at three widths (L8) | Agenda layout regression (RISK-007) | One recorded manual round |
| 7 | **Accessibility checks**: automated scan plus keyboard-only walkthrough | Excluding users; legal/compliance exposure | Scan the customer booking flow first |
| 8 | **Notification delivery test plan**, once a real provider is connected | Messages not delivered, or sent to the wrong recipient (RISK-010) | Define test recipients, safe environments and delivery evidence *before* enabling production delivery |

## 3. Out of scope for the pilot

Not limitations, but deliberate product boundaries: multiple salons or employees, customer self-rescheduling (the salon owner can move bookings), add-on services, online payments, and customer accounts.

## Related

- [Risk Register](risk-register.md)
- [Test Strategy](test-strategy.md)
- [Lessons learned](../about/lessons-learned.md)
