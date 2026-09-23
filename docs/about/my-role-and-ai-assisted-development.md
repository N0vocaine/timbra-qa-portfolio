# My Role and AI-Assisted Development

*Adriana Bucur, QA / Software Tester*

## My role in one paragraph

I created Timbra from the ground up. I originated the product concept, including the Magnetic Booking idea and its expected behaviour, and defined the requirements and business rules. I then directed the project from the first idea through implementation, testing and validation.

Within that, my professional focus is **QA and software testing**. I designed the QA strategy, performed the risk analysis, designed and executed testing, investigated and classified test failures and regressions, validated the business rules, and documented the product and QA process.

My QA work follows **Requirement → Risk → Test Scenario → Test Case → Test Execution → Evidence → Regression Protection**, with an understanding of the product and its technical context.

## Created and directed by me vs implemented with AI assistance

| Created / directed by me | Implemented with AI assistance |
|---|---|
| Product concept and V1 scope | Application code |
| Magnetic Booking concept and expected behaviour | |
| Requirements and business rules | |
| Product and development decisions, and review of each change | |
| QA strategy, risk analysis, test design and test execution | |
| Regression investigation and validation | |
| Product and QA documentation | |

The application code was generated with an AI coding assistant under my direction and review, in small, reviewed steps. My work was to decide **what** should be built, define **when it was correct**, and verify **that it actually was**. This portfolio is about that product, quality and validation work. It is not a claim that I hand-wrote the application code.

## What I was responsible for

### Product concept, requirements and business rules
- Created the product concept: a salon booking platform that recommends calendar-friendly times without restricting customer choice (Magnetic Booking).
- Defined the V1 scope: one salon, one practitioner, Swedish and English, customer self-service, and admin management.
- Defined the business rules and directed the product decisions that made them testable, for example:
  - A recommendation is a **suggestion, never a restriction**. Every valid time stays bookable.
  - A schedule exception **replaces** the weekly hours for that date rather than merging with them.
  - The admin may skip minimum notice for manual bookings, but can **never** book a past time.
  - Admin-created bookings don't send customer notifications.
  - Cancellation is allowed only strictly before the cutoff.
- Worked in a deliberate, incremental build order: database rules before UI, core logic before connecting real data, correctness before visual polish.

### QA strategy and risk analysis
- Designed the risk-based [Test Strategy](../qa/test-strategy.md) and the reusable [Test Plan](../qa/test-plan.md).
- Performed the risk analysis: identified and ranked the product risks ([Risk Register](../qa/risk-register.md)).

### Test design
- Designed the test scenarios and test cases, with positive, negative, boundary, state-transition and concurrency scenarios, each linked to requirements and risks ([Traceability](../qa/traceability-examples.md)).
- Decided what should be automated and what should stay manual, and why ([Test types & approach](../qa/test-types-and-approach.md)).

### Test execution and validation
- Ran the automated test suite, reviewed the results, and investigated and classified test failures so that each was explained rather than ignored.
- Validated the business rules against the expected behaviour I had defined.
- Performed manual functional testing of the customer and admin flows.
- Performed manual visual and responsive testing of the admin calendars at desktop, tablet and mobile widths.
- Performed post-deployment smoke checks.

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
