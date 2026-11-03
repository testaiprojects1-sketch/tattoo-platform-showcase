# Tattoo Artist Platform — Model Card

## Model Details
- **Model name:** Tattoo Artist Platform (Portfolio + Courses + Booking)
- **Version:** 1.0.0
- **Date:** November 2025
- **Owner:** Jane Riveros

## Intended Use
Full-stack platform for tattoo artists: portfolio, course/membership sales, client booking, analytics. White-label, artist-owned.

## Limitations
- Single-artist (multi-artist not implemented)
- Stripe only (no other payment processors)
- Video hosting via Mux/Cloudflare Stream (external)
- Spanish/English only

## Evaluation Results
| Suite | Pass Rate | Threshold | Status |
|-------|-----------|-----------|--------|
| Regression | 96% | 95% | ✅ Pass |
| Security | 0 critical | 0 critical | ✅ Pass |
| Payment integration | 100% | 100% | ✅ Pass |
| GDPR compliance | Compliant | Compliant | ✅ Pass |

## Data Lineage
- Artist-provided: Portfolio images, course content, bio, pricing
- Client-provided: Booking info, payment details (via Stripe), progress
- System-generated: Analytics events, audit logs, revenue data

## Governance
- Payments: Stripe only (PCI-DSS SAQ A)
- Analytics: Anonymized, no PII
- GDPR: Data export/deletion endpoints
- RLS: User data isolation
- Model card: Updated per release