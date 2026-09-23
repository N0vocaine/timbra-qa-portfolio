# Lessons Learned

## About testing

1. **Separate "is it allowed?" from "is it good?"** Keeping eligibility and ranking apart meant each could be tested in isolation. It also let the most important guarantee be stated as a single testable invariant: *ranking never changes eligibility.*

2. **Boundaries deserve their own test cases.** Minimum notice *includes* its boundary; the cancellation cutoff *excludes* it. Only explicit tests at the exact instant catch an off-by-one in either direction.

3. **Make time an input.** Passing the current time into the logic, instead of reading the clock inside it, is what made exact boundary and daylight-saving tests possible and repeatable.

4. **Test critical rules where they are finally enforced.** Double-booking protection is tested against a real database, because only a real database can prove its own rules hold. A mock only proves that the code called the mock.

5. **Protect against the future, not just the present.** Static guards ("every admin action checks authorization", "no secrets in browser code") catch mistakes in code that hasn't been written yet.

6. **Prove the zero.** A test that expects *zero* notifications can pass by accident. Pairing it with a control case that expects notifications makes the zero meaningful.

## About defects

7. **Similar-sounding rules need separate implementations.** "Skip the notice policy" and "don't book the past" were once one filter, so skipping one skipped both ([case study 2](../qa/regression-case-studies.md#case-study-2--admin-notice-bypass-also-bypassed-the-past-time-safety-floor)).

8. **Add the regression test with the fix, not later.** Written straight away, it targets what actually went wrong.

9. **Classify before you fix.** A failing test isn't automatically a product defect. One recurring failure turned out to be demo data and test data colliding in a shared database. Treating it as a product bug, or silencing it with a reset before every run, would both have been wrong.

10. **Manual testing still finds real bugs.** All four product defects in the case studies were found by using the application, not by the automated tests that existed then. Each finding was then turned into permanent automated protection.

## About honesty and scope

11. **An honest gap is worth more than an inflated number.** Stating "no browser automation yet" or "Agenda has no recorded visual check" makes the rest of the evidence more credible.

12. **Keep the kinds of problem separate.** Product defects, test-infrastructure weaknesses and not-yet-built features are different things. Mixing them distorts both the quality picture and the priorities.

## About working with AI-assisted development

13. **Speed of code isn't speed of correctness.** The assistant produced code quickly. Deciding what correct meant, and proving it, remained the real work.

14. **Small steps keep AI-assisted work reviewable.** Every change was small enough to understand, test and verify before moving on.

## What I would do next

- Add a CI quality gate and a first browser E2E test for the customer booking journey.
- Isolate integration test data from demo data.
- Add a staging environment before real notification delivery is switched on.

See [Known limitations & future QA](../qa/known-limitations-and-future-qa.md).
