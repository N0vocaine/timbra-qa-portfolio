# Test Cases: Magnetic Booking V2

These cases cover the **V2 edge filter**: which eligible times an online customer is offered, and how the salon owner's view differs. Background: [From V1 to V2](../../product/evolution-v1-to-v2.md) and [Magnetic Booking](../../product/magnetic-booking.md).

The **MB2-** cases are new for V2. They are not part of the original V1 catalogue, so their IDs start at 001.

**Shared scenario** (used by MB2-002 to MB2-004, and the same as the demo day):
- Working hours 09:00–17:00.
- Existing bookings: customer 09:05–10:05 (blocked 09:00–10:05) and 16:15–17:00 (blocked 16:10–17:00).
- One free window, **10:05–16:10**.
- Requested treatment: 60 minutes, 5 minutes of preparation, buffer 0, 5-minute time grid.

---

### MB2-001: Only the edge times of each free window are offered publicly

| Field | Value |
|---|---|
| Requirement | REQ-MB2-001: an online customer is offered at most the earliest and the latest valid start of each free window; times in between are not offered |
| Risk | Short treatments split long free periods into unsellable gaps (the problem raised by the salon owner) |
| Priority | Critical |
| Technique | Positive |
| Level | Domain logic |
| Preconditions | Free window 12:00–16:00, no other bookings in it |
| Test data | 60-minute occupied treatment (5 min preparation + 55 min, buffer 0) |
| Steps | 1. Calculate the valid times<br>2. Apply the V2 edge filter |
| Expected result | Only **12:05** (preparation from 12:00, treatment until 13:05) and **15:00** (ending exactly at 16:00) are offered. No time in between is offered. Related cases in the same automated set: an exact fit is offered once, and each free window is handled independently. |
| Automation | Automated |
| Evidence | Automated domain-logic tests (edge selection). Demo check after deployment: the public page offered only the two edge times of the demo day. |

### MB2-002: A crafted request for an interior time is rejected in the public flow

| Field | Value |
|---|---|
| Requirement | REQ-MB2-002: the server re-checks a public booking against the public offer, not only against eligibility |
| Risk | The edge rule could be bypassed by editing the request, so the salon's calendar is still fragmented |
| Priority | Critical |
| Technique | Negative |
| Level | Unit (booking submit, with the data layer replaced by test doubles) |
| Preconditions | Shared scenario. 12:00 is valid (eligible) but not offered. |
| Steps | 1. Submit a public booking for 12:00 directly, bypassing the page |
| Expected result | The request is rejected with the "time no longer available" message. **No booking is created.** Submitting an offered edge time (10:10 or 15:10) succeeds. |
| Automation | Automated |
| Evidence | Automated submit test. Demo check after deployment: a manually edited link asking for 12:00 did not open the booking form. |

### MB2-003: After a booking, the offered times move inward

| Field | Value |
|---|---|
| Requirement | REQ-MB2-003: availability is recalculated after every booking, from the new edges of the shrunken free window |
| Risk | The offer doesn't follow the calendar, so bookings stop packing together |
| Priority | High |
| Technique | Positive (state change) |
| Level | Unit (booking submit + availability recalculation, with the data layer replaced by test doubles) |
| Preconditions | Shared scenario. The offered times are 10:10 and 15:10. |
| Steps | 1. Book 10:10<br>2. Recalculate the public offer for the same day |
| Expected result | The booking blocks 10:05–11:10 (5 min preparation before 10:10). The offered times become **11:15** and **15:10**. The upper edge moved inward; the lower edge is unchanged. |
| Automation | Automated |
| Evidence | Automated story test. Manual walkthrough in the local environment (29 September 2026). Demo check after deployment: after a test booking at 10:10, the page offered 11:15 and 15:10. |

### MB2-004: The salon owner still sees every valid time

| Field | Value |
|---|---|
| Requirement | REQ-MB2-004: the edge filter applies to online customers only; the admin availability keeps every valid time |
| Risk | The salon owner can't book regular clients where she wants to |
| Priority | Critical |
| Technique | Positive |
| Level | Unit (availability + admin booking submit, with the data layer replaced by test doubles) |
| Preconditions | Shared scenario |
| Steps | 1. Calculate the admin availability<br>2. Submit an admin booking for 12:00 |
| Expected result | The admin availability includes 10:10, 12:00 and 15:10 (and more). The admin booking at 12:00 is accepted. |
| Automation | Automated |
| Evidence | Automated availability and admin submit tests. Not checked on the demo, because the demo's admin area is read-only. |

---

Results: these cases are part of the automated unit and domain-logic suite that passed (654 tests) on 29 September 2026. See [From V1 to V2: Verification](../../product/evolution-v1-to-v2.md#6-verification).
