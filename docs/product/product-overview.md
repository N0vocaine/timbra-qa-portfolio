# Product Overview

## What Timbra is

*I created Timbra from the ground up, from product concept to validated V1. The application code was generated with AI assistance under my direction; see [My role](../about/my-role-and-ai-assisted-development.md).*

Timbra is an online appointment-booking platform. Its first version (V1) runs for **a beauty salon in Sweden**, with one salon and one practitioner (the salon owner).

- **Customers** open the booking site, choose a treatment, a date and a time, enter their contact details, and get an immediate confirmation. They don't need to phone the salon, send a message or create an account.
- **The salon owner** manages treatments, weekly working hours, one-off schedule exceptions and all bookings from a protected admin area.

The booking site works in **Swedish and English**.

## The problem it solves

| Problem | Why it matters |
|---|---|
| Treatments have different lengths, plus preparation and cleanup time | A calendar treated as identical time blocks either double-books or wastes time |
| Awkwardly placed bookings leave small, unsellable gaps | Every unusable 15-minute fragment is lost revenue for a small business |
| Customers dislike restricted choice | A system that hides valid times to "optimise" the calendar damages trust |
| Booking by phone or message takes the owner's time | Time spent answering booking messages is time not spent with clients |

Timbra's central idea, **Magnetic Booking**, addresses the first three together. It calculates every time that is genuinely bookable and then *recommends* the ones that keep the calendar efficient. It never hides or blocks the other valid times. See [Magnetic Booking](magnetic-booking.md).

## Main features (V1)

### Customer booking
- Bilingual booking flow (Swedish / English)
- Treatment selection with price and duration
- Date selection limited to a configurable booking window
- Available times calculated in real time, with one **recommended time** highlighted
- Validation of the customer's name, email and phone number
- Immediate booking confirmation page. A confirmation email is queued through the notification pipeline (see the limitation under *Notifications*).
- A private **manage link**, so the customer can view or cancel their own booking without an account. Cancellation is allowed up to a configurable cutoff before the appointment.

### Scheduling rules
- Recurring weekly working hours, including split days (for example a lunch break)
- Date-specific overrides: a closed day, or different hours that **replace** the normal week for that date
- Preparation, treatment and buffer (cleanup) time tracked separately for every treatment
- Minimum notice ("lead time") for customer bookings
- Correct handling of the Europe/Stockholm timezone, including daylight-saving transitions

### Admin area
- Sign-in required. Only explicitly authorized admin accounts get access.
- Booking list and detail view, with status changes (completed, no-show, cancelled)
- **Manual booking** on a customer's behalf, for bookings taken by phone or in person
- Treatment management (create, edit, activate/deactivate)
- Working-hours and schedule-exception management
- Calendar in **Day, Week and Agenda** views, showing preparation, treatment and buffer time

### Notifications
- A notification pipeline for confirmation and reminder messages (email and SMS)
- Bookings the admin creates manually don't trigger customer notifications, because the owner has already spoken to the customer
- *Current limitation:* real email/SMS delivery is not yet production-ready. The pipeline is tested up to the point where a message would be handed to a delivery provider. See [Known limitations](../qa/known-limitations-and-future-qa.md).

## Key business rules

These rules were defined as product decisions. Each one became a testable requirement.

| Rule | Decision |
|---|---|
| Calendar blocking | A booking blocks its **whole occupied time** (preparation + treatment + buffer), not only the time the customer sees |
| Back-to-back bookings | Allowed. One booking may start exactly when the previous one's occupied time ends. |
| Split working days | A booking may never span a break between two working intervals |
| Schedule exceptions | A date override **replaces** the weekly schedule for that date; the two are never merged |
| Recommendation | A suggestion only. All valid times remain visible and bookable. |
| Minimum notice | Applies to customers. The admin may skip it for manual bookings. |
| Past times | Can **never** be booked, by anyone, including the admin |
| Cancellation cutoff | A customer can cancel only *strictly before* the cutoff. At the cutoff instant, cancellation is rejected. |
| Booking statuses | Confirmed → Completed / No-show / Cancelled. These three final statuses can't be undone. |
| Admin-created bookings | Don't send customer notifications |

## V1 scope

**In scope:** one salon, one practitioner, one timezone, Swedish/English, customer self-service booking and cancellation, and admin management.

**Out of scope for V1 (possible future work):** several salons or employees, rescheduling, add-on services, online payment, and live notification delivery.

## Related documents

- [Magnetic Booking — Eligibility vs Ranking](magnetic-booking.md)
- [Booking flow](booking-flow.md)
- [Architecture overview](architecture-overview.md)
- [Test Strategy](../qa/test-strategy.md)
