# Test Case Catalogue (Selected)

The full private catalogue has more than 100 scenario-level test cases. This portfolio shows **25 representative cases**, chosen to show the range of risks, techniques and test levels in Timbra. The original case IDs are kept, so gaps in the numbering are expected.

## Selected cases

| File | Cases | Focus |
|---|---|---|
| [Booking engine](booking-engine.md) | 10 | Four timestamps, occupied intervals, adjacency, split days, schedule overrides, lead time, collisions, concurrency, customer vs admin |
| [Magnetic ranking](magnetic-ranking.md) | 4 | Ranking rules, recommendation never hides options, ranking never changes eligibility |
| [Timezone / DST](timezone-dst.md) | 3 | Non-existent and ambiguous local times, cutoff across DST |
| [Cancellation](cancellation.md) | 4 | Before / at / after the cutoff, slot released after cancellation |
| [Authentication & security](auth-and-security.md) | 4 | Unauthenticated, unauthorized, fail-closed configuration, every admin action protected |
| **Total** | **25** | |

## Case format

| Field | Meaning |
|---|---|
| **ID** | Stable ID from the full catalogue (area prefix + number) |
| **Requirement** | The requirement the case protects |
| **Risk** | The risk ID from the [Risk Register](../risk-register.md) |
| **Priority** | Critical / High / Medium / Low |
| **Technique** | Positive, negative, boundary, state transition, concurrency… |
| **Level** | Unit · Domain logic · DB integration · Auth integration · Static guard · Manual |
| **Preconditions** | What must be true beforehand |
| **Test data** | Concrete example values |
| **Steps** | What is done |
| **Expected result** | What must be true afterwards |
| **Automation** | Automated / Partially automated / Manual / Not automated |
| **Evidence** | The kind of evidence that exists. Private file names are not published. |

## How to read the evidence fields

- **Automated** means an automated test for this case exists in the private repository and runs as part of the suite.
- **Manual evidence** is recorded separately, and only where real evidence exists. An automated case without a separately recorded manual run is marked *Not applicable*, not *PASS*.
- **Test data values are illustrative.** They are consistent with the rule being tested and chosen to make the case readable. The automated suite uses its own fixtures.

## Summary by technique and level

| Technique | Count |
|---|---|
| Positive | 7 |
| Negative | 8 |
| Boundary | 6 |
| Concurrency | 1 |
| Configuration / fail-closed | 1 |
| Structural (static guard) | 1 |
| Property / invariant | 1 |
| **Total** | **25** |

| Primary level | Count |
|---|---|
| Unit / domain logic | 17 |
| DB integration (real local PostgreSQL) | 6 |
| Auth integration | 1 |
| Static guard | 1 |
| **Total** | **25** |

*Counts use each case's primary technique and level. Several cases combine more than one: for example, MAG-004 also has a manual browser check, and ADM-007 has both unit and integration coverage.*

All 25 selected cases have automated coverage. Areas that depend on **manual** testing (the full browser journey, calendar appearance, responsive layout) are described in [Test types & approach](../test-types-and-approach.md) and the [Test Plan](../test-plan.md).

## Related

- [Traceability examples](../traceability-examples.md): how these cases connect to requirements and risks
- [Boundary & negative testing](../boundary-and-negative-testing.md): the techniques behind many of these cases
