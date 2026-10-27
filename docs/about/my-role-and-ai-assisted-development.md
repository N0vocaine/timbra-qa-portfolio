# My Role and AI-Assisted Development

*Adriana Bucur, QA / Software Tester*

## My role in one paragraph

Timbra began when a salon owner I know asked whether I could help create a booking application. She and her colleagues were dissatisfied with the solution they were using, and I took on the challenge of finding a way to build an alternative, despite having no prior software development experience.

I developed the Magnetic Booking approach, defined requirements and business rules with input from the salon owner, and coordinated implementation using an AI coding assistant. Throughout development, I reviewed the results and tested the application.

Within that, my professional focus is **QA and software testing**. I designed the QA strategy, performed the risk analysis, designed the testing and carried out or directed its execution, investigated and classified test failures and regressions, validated the business rules, and documented the product and QA process.

My QA work follows **Requirement → Risk → Test Scenario → Test Case → Test Execution → Evidence → Regression Protection**, with an understanding of the product and its technical context.

## Created and directed by me vs implemented with AI assistance

| Created / directed by me | Implemented with AI assistance |
|---|---|
| Product definition and V1 scope, based on the salon owner's request and input | Application code |
| Magnetic Booking concept and expected behaviour | Automated test code, written to the test cases and rules I defined |
| Requirements and business rules | Drafting of documentation, which I reviewed and edited |
| Product and development decisions, and review of each change | |
| QA strategy, risk analysis, test design and test execution | |
| Regression investigation and validation | |
| Product and QA documentation | |

The application code and the automated test code were generated with an AI coding assistant under my direction and review, in small, reviewed steps. The assistant also ran many of the automated and browser checks in my working sessions; I decided what to check, reviewed the results and decided what counted as done. My work was to decide **what** should be built, define **when it was correct**, and verify **that it actually was**. This portfolio is about that product, quality and validation work. It is not a claim that I hand-wrote the application code.

## What I was responsible for

### Product concept, requirements and business rules
- Turned the salon owner's request for a better booking application into a product definition, and developed its central approach: a booking platform that recommends calendar-friendly times (Magnetic Booking). In V1, this was done without restricting customer choice.
- Defined the V1 scope: one salon, one practitioner, Swedish and English, customer self-service, and admin management.
- Turned the salon owner's feedback on V1 into the V2 rules:
  - technically available is not the same as offered;
  - online customers are offered only a few edge times, while the salon owner keeps every valid time (refined in v2.1.0 to the times next to bookings plus the day's earliest and latest start);
  - preparation is 5 minutes and the buffer is 0 in the pilot;
  - customers and the admin have separate booking windows.

  See [From V1 to V2](../product/evolution-v1-to-v2.md).
- Specified multi-service booking for v3.0.0, which the salon owner requested: I clarified and approved its preparation rules (the first treatment's preparation once, then the treatments back to back) and required that the customer's chosen order is kept.
- Defined the business rules and directed the product decisions that made them testable, for example:
  - V1: a recommendation is a **suggestion, never a restriction**, and every valid time stays bookable. In V2 this still holds for the salon owner, while online customers are offered a restricted set of edge times.
  - A schedule exception **replaces** the weekly hours for that date rather than merging with them.
  - The admin may skip minimum notice for manual bookings, but can **never** book a past time.
  - Admin-created bookings don't send customer notifications.
  - Cancellation is allowed up to the cutoff (V1: only strictly before it; V2 includes the cutoff instant).
- Worked in a deliberate, incremental build order: database rules before UI, core logic before connecting real data, correctness before visual polish.

### QA strategy and risk analysis
- Designed the risk-based [Test Strategy](../qa/test-strategy.md) and the reusable [Test Plan](../qa/test-plan.md).
- Performed the risk analysis: identified and ranked the product risks ([Risk Register](../qa/risk-register.md)).

### Test design
- Designed the test scenarios and test cases, with positive, negative, boundary, state-transition and concurrency scenarios, each linked to requirements and risks ([Traceability](../qa/traceability-examples.md)).
- Decided what should be automated and what should stay manual, and why ([Test types & approach](../qa/test-types-and-approach.md)).

### Test execution and validation
- Directed and reviewed the automated test runs, and investigated and classified test failures so that each was explained rather than ignored.
- Validated the business rules against the expected behaviour I had defined.
- Performed manual functional testing of the customer and admin flows; for example, my local testing of multi-service booking found that only two treatments could be selected, which was fixed before the release.
- Performed manual visual and responsive testing of the admin calendars at desktop, tablet and mobile widths.
- Directed and reviewed the post-deployment smoke checks; many of them, including the v3.0.0 read-only checks of both live sites, were run by the AI assistant.

### Regression investigation
- Investigated real defects, including their root cause and classification, and made sure each fix came with regression protection ([Regression case studies](../qa/regression-case-studies.md)).
- Classified a recurring local test failure correctly as a **test-environment issue** rather than a product defect, and defined a diagnose-first procedure instead of a blind reset habit.

### Documentation
- Documented the project and QA process, directing and reviewing the full documentation set behind this portfolio: product requirements, QA strategy, test plan, test case catalogue, traceability matrix and regression history.

### Safety and data discipline
- Set strict rules for working with the live system: production data is never reset or reseeded, credentials are never handled in plain text or written into documents, and demo data is kept separate from real data.

## How I worked with the AI assistant

- **Small, verifiable steps.** Each change was planned, implemented, tested, manually verified and reviewed before being accepted.
- **Evidence over assertion.** A feature counted as working when tests and manual checks showed it, not when the assistant said it was done.
- **Honest documentation.** Limitations and gaps were written down as they were, including the ones that don't look impressive.

## Why this matters for a QA role

AI-assisted development makes code quicker to produce. It doesn't make it **correct**. Knowing what the rules are, where the risks are, what "done" means, and how to prove it is the QA work, and it matters more when code arrives faster.
