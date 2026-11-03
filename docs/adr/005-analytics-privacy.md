# ADR 005: Analytics Privacy — GDPR-Compliant, Anonymized

**Date:** 2025-11-01
**Status:** Accepted
**Deciders:** Jane Riveros

## Context
Need: revenue analytics, enrollment funnel, engagement metrics — without PII, GDPR-compliant.

## Decision
Custom analytics events (no Google Analytics). Events: course_purchase, booking_created, video_play, portfolio_view. Anonymized: user_id hashed, no IP, no personal data. Stored in PostgreSQL (RLS).

## Rationale
- GDPR Art. 25: Privacy by design (no PII in analytics)
- Custom: Full control over schema, retention, access
- RLS: Artists see only their analytics
- Export/Deletion: GDPR Art. 15/17 endpoints for analytics data
- No cookie consent banner needed (no tracking cookies)

## Consequences
- No third-party analytics (GA, Mixpanel) — less feature-rich
- Attribution limited (no cross-site tracking)
- Self-hosted dashboard maintenance
- Data retention policy: 2 years default, configurable