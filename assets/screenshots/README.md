# Screenshots

Eight screenshots, each chosen to support one specific product or QA point. None of them comes from production.

- **06–08 (current, V2):** captured on 30 September 2026 from the **live public demo** ([timbra-booking-demo.vercel.app](https://timbra-booking-demo.vercel.app)), which runs the V2 pilot with fictional data.
- **01–05 (V1):** captured earlier from a **local development environment with fictional demo data**. Screenshots 01 and 02 show V1 behaviour that changed in V2: every valid time offered to the customer, and a buffer segment. They are kept as history ([From V1 to V2](../../docs/product/evolution-v1-to-v2.md)). Screenshots 03–05 show behaviour that is unchanged in V2.

## Current set (V2)

| File | Shows | What it demonstrates | Used in |
|---|---|---|---|
| `06-v2-public-booking-edge-times.png` | Public booking: "Classic facial" on 2026-10-06, **10:10** recommended, **15:10** as the only other time | V2 edge-only offer on a day with one free window (10:05–16:10): only its two edge times are offered, with no "Show more" | [README](../../README.md#v2-in-the-live-demo) |
| `07-v2-public-booking-two-free-windows.png` | Public booking: "Classic facial" on 2026-10-01, **11:20** recommended, then 19:00, 09:05, 09:25 | One existing booking (10:30–11:15, preparation from 10:25) splits the day into two free windows, and each offers its own two edges. Also shows that ranking orders the offered times. | [README](../../README.md#v2-in-the-live-demo) |
| `08-v2-admin-agenda-preparation-no-buffer.png` | Admin agenda (read-only demo) for 2026-10-06 | Preparation shown as its own row before each treatment, no buffer rows, and the free window 10:05–16:10. Customer names are masked by the demo. | [README](../../README.md#v2-in-the-live-demo) |

What these screenshots **don't** show: the server-side re-check that rejects a crafted request for an interior time, the recalculation after a booking, or automated test results. That evidence is described in [From V1 to V2: Verification](../../docs/product/evolution-v1-to-v2.md#6-verification).

## Historical and unchanged set (V1)

| File | Shows | Why it exists | Used in |
|---|---|---|---|
| `01-magnetic-booking-recommended-time.png` | Customer booking: **13:05** recommended, with **09:05** and other valid times still selectable | *V1 (historical):* shows that a valid time isn't automatically the recommended one, and that ranking never removes a valid time | [README](../../README.md#historical-v1-screenshots), [Magnetic Booking](../../docs/product/magnetic-booking.md#4-the-rules-that-tie-it-together) |
| `02-admin-day-view-prep-treatment-buffer.png` | Admin Day view: preparation 5 min → booking 13:00–13:50 → buffer 10 min | *V1 (historical):* makes the occupied interval visible, which is the interval that availability and collision rules actually use. The V2 pilot uses no buffer. | [README](../../README.md#historical-v1-screenshots), [Magnetic Booking](../../docs/product/magnetic-booking.md#1-four-timestamps-per-booking), [Booking flow](../../docs/product/booking-flow.md#3-admin-journey) |
| `03-schedule-weekly-hours-and-override.png` | Weekly hours with a split Wednesday (09–12 / 13–17), closed weekends, and a date override 10:00–14:00 | The schedule data behind the split-day and schedule-exception test cases | [Boundary & negative testing](../../docs/qa/boundary-and-negative-testing.md#seen-in-the-application), [Test cases: booking engine](../../docs/qa/test-cases/booking-engine.md) |
| `04-negative-test-validation-errors.png` | Customer form with three field-level validation errors | A visible negative test: invalid input is rejected and nothing is saved | [Boundary & negative testing](../../docs/qa/boundary-and-negative-testing.md#2-negative-testing) |
| `05-boundary-cancellation-after-cutoff.png` | Manage page (cropped): status *Confirmed* plus "can no longer be cancelled online" | The cancellation rule enforced **after** the cutoff. It does not show the exact boundary instant, which is covered by automated tests. | [Boundary & negative testing](../../docs/qa/boundary-and-negative-testing.md#seen-in-the-application), [Test cases: cancellation](../../docs/qa/test-cases/cancellation.md) |

The main README shows 06–08 (V2) and 01–02 (V1, historical). The others sit next to the QA explanation they support.

The admin area of the application is in Swedish, so captions translate the labels shown.

## How they were produced

**V2 (06–08)**
- Captured from the live public demo with a headless browser and a fresh, empty browser profile (no signed-in account), so no address bar, tabs or browser interface is included.
- Existing demo data only. No booking was created or cancelled to produce them.
- **Cropped only**, to remove empty space at the bottom of the page. No pixels were edited, painted over or retouched.
- The fictional demo salon name ("Studio Lumen (Demo)") appears in the page headers; it is not a real business.

**V1 (01–05)**
- Captured from the running local application against a local database with fictional demo data. The demo customers use a reserved, non-routable email domain.
- Captured as focused page regions, not whole browser windows. No address bar, tabs or browser interface is included.
- Images 02 and 05 were then **cropped only**, to remove empty space (02) and to leave out booking details that weren't needed (05).

**All**
- The PNG files contain no metadata: no text chunks, EXIF, file paths or usernames.
- No automated-test screenshot is included. The portfolio refers to the automated suite by its test count (654 unit and domain-logic tests as of the V2 pilot).

## Safety rules (every screenshot)

- [x] Fictional demo data only (local environment for V1, the public demo for V2). Never production.
- [x] The real salon name doesn't appear. V1 headers are cropped out; V2 headers show only the fictional demo salon.
- [x] No real customer, admin or business names, and no real email addresses or phone numbers. In the V2 admin screenshot, the demo masks customer names.
- [x] No browser address bar, tabs or bookmarks, and no URLs.
- [x] No private manage-link or confirmation tokens.
- [x] No admin sign-in form, admin email address or credentials.
- [x] No database or hosting dashboards, terminals, source code or configuration.
- [x] No operating-system username or local file paths.
- [x] No Next.js development indicator.
- [x] Cropping only. Visual content isn't edited.
- [x] Image metadata checked: none present.
