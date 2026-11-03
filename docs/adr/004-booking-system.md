# ADR 004: Booking System — Deposits, Reminders, Calendar

**Date:** 2025-11-01
**Status:** Accepted
**Deciders:** Jane Riveros

## Context
Artists need: availability calendar, deposit collection (reduce no-shows), automated reminders, client CRM.

## Decision
Supabase Realtime for calendar sync, Stripe for deposits, nodemailer/Resend for reminders. PostgreSQL constraints for double-booking prevention.

## Rationale
- Realtime: Instant calendar updates across devices
- Deposits: Stripe PaymentIntent (capture on confirmation, release on cancel)
- Reminders: Scheduled jobs (pg_cron or external) → email/SMS
- Double-booking: DB-level exclusion constraints (transaction-safe)

## Consequences
- Deposit logic complexity: partial refunds, artist cancellation policy
- Reminder delivery: Email deliverability (Resend/Postmark)
- Timezone handling: Artist timezone + client timezone
- Calendar sync: Google/Outlook calendar export (ICS)