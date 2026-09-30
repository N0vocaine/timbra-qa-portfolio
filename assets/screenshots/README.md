# Screenshots

Five screenshots, each chosen to support one specific product or QA point. All were captured from a **local development environment with fictional demo data**. None of them comes from production.

**All five were captured with V1.** Screenshots 01 and 02 show V1 behaviour that changed in V2: every valid time offered to the customer, and a buffer segment. They are kept as history ([From V1 to V2](../../docs/product/evolution-v1-to-v2.md)).

## Approved set

| File | Shows | Why it exists | Used in |
|---|---|---|---|
| `01-magnetic-booking-recommended-time.png` | Customer booking: **13:05** recommended, with **09:05** and other valid times still selectable | *V1 (historical):* shows that a valid time isn't automatically the recommended one, and that ranking never removes a valid time | [README](../../README.md#how-timbra-decides-which-times-to-offer-v2), [Magnetic Booking](../../docs/product/magnetic-booking.md#4-the-rules-that-tie-it-together) |
| `02-admin-day-view-prep-treatment-buffer.png` | Admin Day view: preparation 5 min → booking 13:00–13:50 → buffer 10 min | *V1 (historical):* makes the occupied interval visible, which is the interval that availability and collision rules actually use. The V2 pilot uses no buffer. | [README](../../README.md#how-timbra-decides-which-times-to-offer-v2), [Magnetic Booking](../../docs/product/magnetic-booking.md#1-four-timestamps-per-booking), [Booking flow](../../docs/product/booking-flow.md#3-admin-journey) |
| `03-schedule-weekly-hours-and-override.png` | Weekly hours with a split Wednesday (09–12 / 13–17), closed weekends, and a date override 10:00–14:00 | The schedule data behind the split-day and schedule-exception test cases | [Boundary & negative testing](../../docs/qa/boundary-and-negative-testing.md#seen-in-the-application), [Test cases: booking engine](../../docs/qa/test-cases/booking-engine.md) |
| `04-negative-test-validation-errors.png` | Customer form with three field-level validation errors | A visible negative test: invalid input is rejected and nothing is saved | [Boundary & negative testing](../../docs/qa/boundary-and-negative-testing.md#2-negative-testing) |
| `05-boundary-cancellation-after-cutoff.png` | Manage page (cropped): status *Confirmed* plus "can no longer be cancelled online" | The cancellation rule enforced **after** the cutoff. It does not show the exact boundary instant, which is covered by automated tests. | [Boundary & negative testing](../../docs/qa/boundary-and-negative-testing.md#seen-in-the-application), [Test cases: cancellation](../../docs/qa/test-cases/cancellation.md) |

Only 01 and 02 appear in the main README. The others sit next to the QA explanation they support.

The admin area of the application is in Swedish, so captions translate the labels shown.

## How they were produced

- Captured from the running local application against a local database with fictional demo data. The demo customers use a reserved, non-routable email domain.
- Captured as focused page regions, not whole browser windows. No address bar, tabs or browser interface is included.
- Images 02 and 05 were then **cropped only**, to remove empty space (02) and to leave out booking details that weren't needed (05). No pixels were edited, painted over or retouched.
- The PNG files contain no metadata: no text chunks, EXIF, file paths or usernames.
- No automated-test screenshot is included. The portfolio refers to the automated suite by its test count (654 unit and domain-logic tests as of the V2 pilot).

## Safety rules (every screenshot)

- [x] Local environment with fictional demo data only. Never production.
- [x] The real salon name doesn't appear. It is part of the page headers, so all headers are cropped out.
- [x] No real customer, admin or business names, and no real email addresses or phone numbers.
- [x] No browser address bar, tabs or bookmarks, and no URLs.
- [x] No private manage-link or confirmation tokens.
- [x] No admin sign-in form, admin email address or credentials.
- [x] No database or hosting dashboards, terminals, source code or configuration.
- [x] No operating-system username or local file paths.
- [x] No Next.js development indicator.
- [x] Cropping only. Visual content isn't edited.
- [x] Image metadata checked: none present.
