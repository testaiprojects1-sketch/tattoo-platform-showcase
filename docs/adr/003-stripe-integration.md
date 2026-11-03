# ADR 003: Stripe Integration — Checkout, Billing, Webhooks

**Date:** 2025-11-01
**Status:** Accepted
**Deciders:** Jane Riveros

## Context
Need: one-time course purchases, recurring memberships, artist payouts (future), PCI compliance.

## Decision
Stripe Checkout (hosted) for purchases, Stripe Billing for subscriptions, Stripe Connect for future artist payouts. Webhook handlers for fulfillment.

## Rationale
- Checkout: Hosted PCI-DSS SAQ A (no card data touches our servers)
- Billing: Subscription management, trials, proration, dunning
- Connect: Future marketplace (platform takes %, artists get payouts)
- Webhooks: Idempotent handlers for fulfillment (course access, booking confirmation)

## Consequences
- Stripe fees (2.9% + 30¢ US; higher intl)
- Webhook reliability: Must handle retries, ordering, idempotency
- Testing: Stripe CLI for local; test mode for CI
- Connect: Additional compliance (KYC, tax) when enabled