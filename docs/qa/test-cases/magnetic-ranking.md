# Test Cases: Magnetic Ranking

These cases cover **ranking** (*"How good is this valid slot compared with the other valid slots?"*) and the rules that keep ranking from ever affecting eligibility. Background: [Magnetic Booking](../../product/magnetic-booking.md).

**Ranking rules, in strict order:** (1) fewer leftover fragments → (2) larger remaining free block → (3) earlier start.

---

### MAG-001: Fewer leftover fragments ranks higher

| Field | Value |
|---|---|
| Requirement | REQ-MAG-001: a slot leaving fewer free fragments ranks higher |
| Risk | The product's core differentiator silently stops working |
| Priority | Critical |
| Technique | Positive |
| Level | Domain logic |
| Preconditions | Open 09:00–12:00. Existing booking 10:00–10:30. |
| Test data | 30-minute treatment (no prep/buffer). Candidates **09:00** (leaves one 30-min piece → 1 fragment) and **09:15** (leaves two 15-min pieces → 2 fragments). |
| Steps | 1. Rank the eligible candidates |
| Expected result | **09:00 ranks above 09:15** |
| Automation | Automated |
| Evidence | Automated domain-logic test (ranking) |

### MAG-002: Tie-break on the larger remaining free block

| Field | Value |
|---|---|
| Requirement | REQ-MAG-002: when fragment counts are equal, the slot leaving the larger single free block ranks higher |
| Risk | Poorer recommendations; free time split into small, less useful pieces |
| Priority | High |
| Technique | Positive |
| Level | Domain logic |
| Preconditions | Two free gaps: **09:00–10:00** (60 min) and **13:00–15:00** (120 min) |
| Test data | 30-minute treatment. Candidate **09:00** (1 fragment, leftover 30 min). Candidate **13:00** (1 fragment, leftover 90 min). |
| Steps | 1. Rank the eligible candidates |
| Expected result | **13:00 ranks above 09:00**. Fragments are equal, so the larger leftover wins. |
| Automation | Automated |
| Evidence | Automated domain-logic test (ranking) |
| Notes | A third rule (MAG-003) breaks any remaining tie by earliest start, so the result is always deterministic |

### MAG-004: The recommendation never hides other valid times

| Field | Value |
|---|---|
| Requirement | REQ-MAG-005: the recommended highlight never removes, hides or disables other valid times |
| Risk | Customer choice restricted, which erodes trust (a stated product principle) |
| Priority | Critical |
| Technique | Positive |
| Level | Unit (presentation rule) + manual browser check |
| Preconditions | A ranked list of eligible times |
| Steps | 1. Apply the presentation rule that marks the top-ranked time as recommended<br>2. Compare the displayed set with the ranked set |
| Expected result | **Every eligible time is still present and selectable.** Only the top-ranked time is marked "recommended". Times beyond the initial visible count remain reachable through "show more". |
| Automation | Automated (presentation rule). The rendered display is part of the manual customer-flow check in the [Test Plan](../test-plan.md). |
| Evidence | Automated unit test (ranked-times presentation) |

### MAG-005: Ranking never changes which slots are eligible

| Field | Value |
|---|---|
| Requirement | REQ-MAG-004: ranking only reorders; it never adds, removes or changes a slot |
| Risk | An invalid time becomes bookable, or a valid time becomes unbookable |
| Priority | Critical |
| Technique | Property / invariant |
| Level | Domain logic |
| Preconditions | A set of eligible candidate slots |
| Steps | 1. Record the set of eligible slots and their timestamps<br>2. Rank them<br>3. Compare the ranked output with the input set |
| Expected result | **The same set of slots with identical timestamps.** Only the order differs. |
| Automation | Automated |
| Evidence | Automated domain-logic tests (ranking and the combined engine) |
| Notes | This is the eligibility-vs-ranking separation expressed as a testable invariant |
