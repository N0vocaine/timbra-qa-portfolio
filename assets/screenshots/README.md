# Screenshots

All screenshots use fictional data. None of them comes from production. Each one is labelled by version and capture date.

## Latest captures (release v3.3.1, captured 17 November 2026 from the public demo)

Fictional demo salon "Studio Lumen (Demo)"; customer names are masked in the read-only admin. Captured with a scripted browser at desktop width (1280 px) and phone width (390 px, mobile emulation).

| File | Shows | What it demonstrates | Used in |
|---|---|---|---|
| `13-v3.3.1-public-booking-times-2026-11-17.jpg` | Public booking in English, day-and-time step for 17–23 November 2026, desktop | Offered times in time order with one recommended time per day (lilac outline, star). On Friday 20 November, **10:15** is offered right after two existing bookings that end at 10:10 | [README](../../README.md) |
| `14-v3.3.1-public-booking-details-mobile-2026-11-17.jpg` | Public booking in Swedish at phone width, day-and-time step with 12:00 on Wednesday 18 November chosen | The chosen time (12:00) in the plum gradient with a check mark, next to recommended times (lilac, star) and plain offered times (white) | — |
| `15-v3.3.1-admin-day-2026-11-20.jpg` | Read-only admin day view, 20 November 2026, desktop | Plum gradient header with the logo; two confirmed bookings in muted lilac with a dark plum leading stripe and status symbol; the hatched preparation strip; masked names; the daily break | [README](../../README.md) |
| `16-v3.3.1-admin-week-2026-11-16.jpg` | Read-only admin week view in English, week of 16 November 2026, desktop | The same bookings in the week grid with gray-plum time labels on an ivory grid | — |
| `17-v3.3.1-admin-overview-mobile-2026-11-17.jpg` | Read-only admin overview at phone width | The admin header and primary button in the plum gradient on a phone | — |
| `18-v3.3.1-public-home-2026-11-17.jpg` | Public start page in Swedish, desktop | The demo notice, the salon name and the "Boka tid" button in the plum gradient | — |

## Release v2.1.0 captures (7 October 2026, from the public demo)

| File | Shows | What it demonstrates | Used in |
|---|---|---|---|
| `09-v2.1.0-public-booking-week-2026-10-20.jpg` | Public booking at desktop width (1024 px), scrolled just below the demo's fictional-data notice: "Classic facial", day-and-time step for days from 20 October 2026 | Days without bookings, split by the 13:00–14:00 break: each window offers its earliest and latest start (09:05, 12:00, 14:05, 16:00; 19:00 on Thursday). Times in time order with the recommended one marked. | [README](../../README.md), [Magnetic Booking](../../docs/product/magnetic-booking.md#current-behaviour-v2-release-v300) |
| `11-v2.1.0-admin-day-2026-10-06.jpg` | Admin day view (read-only demo, desktop width) for 6 October 2026 | Two bookings with their 5-minute preparation strips, no buffer, the 13:00–14:00 break as "Stängt / ej tillgänglig", masked customer names | [Booking flow](../../docs/product/booking-flow.md#3-admin-journey) |
| `12-v2.1.0-admin-week-2026-10-05.jpg` | Admin week view (read-only demo, desktop width), week 41, 5–11 October 2026 | The week's three bookings with preparation strips, the break on Monday–Thursday, closed Friday and Sunday, Saturday 10–13, Thursday until 20:00 | [Booking flow](../../docs/product/booking-flow.md#3-admin-journey) |
| `10-v2.1.0-admin-list-2026-10-06.jpg` | Admin list view (read-only demo) for 6 October 2026 | Preparation as its own 5-minute row before each booking, no buffer, the break as "Stängt / ej tillgänglig", masked customer names | [From V1 to V2: Demo review](../../docs/product/evolution-v1-to-v2.md#demo-review-7-october-2026) |

The v2.1.0 screenshots were captured on 7 October 2026 at desktop width, before the v3.3.0 redesign. Admin captures use past dates that have fictional bookings; those dates are not selectable in the customer flow. Not shown live: on 7 October 2026 the demo had no bookings on any selectable future date, so the time offered next to an existing booking could not be captured. Nothing was booked to create one.

## History that explains V1 → V2

| File | Version and date | Shows | Used in |
|---|---|---|---|
| `06-v2-public-booking-edge-times.png` | First V2, captured 30 September 2026 from the demo (before v2.1.0) | "Classic facial" on 2026-10-06: one free window (10:05–16:10) between two bookings; only **10:10** (recommended) and **15:10** offered, no "Show more" | [From V1 to V2](../../docs/product/evolution-v1-to-v2.md#5-before--after-the-same-day) |
| `01-magnetic-booking-recommended-time.png` | V1, local environment | Every valid time selectable; **13:05** recommended | [Magnetic Booking](../../docs/product/magnetic-booking.md#4-the-rules-that-tie-it-together) |
| `02-admin-day-view-prep-treatment-buffer.png` | V1, local environment | Admin day view: preparation 5 min → booking 13:00–13:50 → buffer 10 min. Shows the occupied interval that availability and collision rules use. The V2 pilot uses no buffer (compare screenshot 10). | [Magnetic Booking](../../docs/product/magnetic-booking.md#1-four-timestamps-per-booking), [Booking flow](../../docs/product/booking-flow.md#3-admin-journey) |

## V1 captures behind the QA documents

Captured from a local development environment with fictional data during V1. They illustrate a test technique rather than the current interface.

| File | Shows | Note | Used in |
|---|---|---|---|
| `03-schedule-weekly-hours-and-override.png` | Weekly hours with a split Wednesday (09–12 / 13–17), closed weekends, and a date override 10:00–14:00 | The schedule data behind the split-day and schedule-exception test cases | [Boundary & negative testing](../../docs/qa/boundary-and-negative-testing.md#seen-in-the-application-history-v1-captures), [Test cases: booking engine](../../docs/qa/test-cases/booking-engine.md) |
| `04-negative-test-validation-errors.png` | Customer form with three field-level validation errors | A visible negative test: invalid input is rejected and nothing is saved | [Boundary & negative testing](../../docs/qa/boundary-and-negative-testing.md#2-negative-testing) |
| `05-boundary-cancellation-after-cutoff.png` | Manage page (cropped): status *Confirmed* plus "can no longer be cancelled online" | The after-cutoff state. The wording is V1's; the current page uses different text, and the cutoff rule itself changed in V2 (see [Test cases: cancellation](../../docs/qa/test-cases/cancellation.md)). It does not show the exact boundary instant, which is covered by automated tests. | [Boundary & negative testing](../../docs/qa/boundary-and-negative-testing.md#seen-in-the-application-history-v1-captures), [Test cases: cancellation](../../docs/qa/test-cases/cancellation.md) |

Two earlier V2 screenshots from 30 September 2026 (a two-window day and the admin agenda) were retired on 7 October 2026: screenshots 09 and 10 show the same points in the current interface. They remain in the Git history.

The admin area of the application is in Swedish, so captions translate the labels shown.
