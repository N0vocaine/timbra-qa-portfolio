# Test Cases: Timezone and Daylight Saving Time

The salon operates in **Europe/Stockholm**: UTC+1 in winter and UTC+2 in summer. Twice a year, local clock times either **don't exist** (spring forward) or **happen twice** (fall back). A booking system that guesses at these moments can silently book the wrong hour.

**Design rule under test:** never guess. Reject a non-existent or ambiguous local time explicitly, and measure time limits in **real elapsed time**.

All cases inject the exact instant under test, so they are deterministic and repeatable.

---

### TZ-002: A local time that doesn't exist (spring forward) is rejected

| Field | Value |
|---|---|
| Requirement | REQ-TZ-002: a non-existent local time is rejected and never silently guessed |
| Risk | RISK-002 A booking created at a time that never existed |
| Priority | Critical |
| Technique | Negative (boundary date) |
| Level | Unit |
| Preconditions | Timezone Europe/Stockholm |
| Test data | **29 March 2026, 02:30** local (clocks jump from 02:00 to 03:00) |
| Steps | 1. Convert the local date and time to an absolute instant |
| Expected result | A clear **"non-existent local time" error**. No instant is returned. |
| Automation | Automated |
| Evidence | Automated unit test (timezone conversion) |

### TZ-003: An ambiguous local time (fall back) is rejected

| Field | Value |
|---|---|
| Requirement | REQ-TZ-003: an ambiguous local time is rejected and never silently guessed |
| Risk | RISK-002 A booking created at the wrong one of two possible instants |
| Priority | Critical |
| Technique | Negative (boundary date) |
| Level | Unit |
| Preconditions | Timezone Europe/Stockholm |
| Test data | **25 October 2026, 02:30** local (02:00–02:59 happens twice) |
| Steps | 1. Convert the local date and time to an absolute instant |
| Expected result | A clear **"ambiguous local time" error**. Neither candidate instant is picked silently. |
| Automation | Automated |
| Evidence | Automated unit test (timezone conversion) |
| Notes | Normal salon hours are far from 02:00–03:00, so this shouldn't happen in practice. The code still checks explicitly instead of relying on that assumption. |

### TZ-007: The cancellation cutoff stays correct across a DST change

| Field | Value |
|---|---|
| Requirement | REQ-TZ-005: cutoff boundaries use real elapsed time, not clock-face time |
| Risk | RISK-002 Customer allowed or blocked incorrectly around a DST change |
| Priority | High |
| Technique | Boundary |
| Level | Unit |
| Preconditions | Cancellation cutoff 24 hours. Timezone Europe/Stockholm. |
| Test data | Appointment **Sunday 29 March 2026, 15:00** (summer time, UTC+2). The clocks moved forward earlier that morning. |
| Steps | 1. Evaluate cancellation just before, exactly at, and after the cutoff instant |
| Expected result | The cutoff is **24 real hours** before the appointment, which is **Saturday 14:00 local time** (winter time, UTC+1), *not* 15:00. Before 14:00 → allowed. At 14:00 or later → rejected. The mirror case at the autumn change is also covered. |
| Automation | Automated |
| Evidence | Automated unit test (cancellation cutoff) |
| Notes | The illustrative date is chosen to show the effect clearly. The automated suite uses its own transition dates. |
