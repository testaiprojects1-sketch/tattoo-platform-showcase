# ADR 002: Video Streaming — Mux / Cloudflare Stream

**Date:** 2025-11-01
**Status:** Accepted
**Deciders:** Jane Riveros

## Context
Courses need: video hosting, DRM, progress tracking, analytics, global CDN, cost-effective at scale.

## Decision
Mux (primary) / Cloudflare Stream (fallback) for video infrastructure. Signed URLs for access control.

## Rationale
- Mux: Developer-friendly API, per-title encoding, signed URLs, webhooks for progress
- Cloudflare Stream: Cheaper at scale, integrated with Cloudflare CDN
- Both: No video storage on our infra; signed URLs enforce enrollment
- Progress tracking via Mux/Cloudflare webhooks → Supabase

## Consequences
- External dependency (mitigated: abstraction layer for switching)
- Cost per minute watched (monitoring + alerts)
- DRM: Signed URLs + short expiry (not Hollywood-grade)
- Latency: Global CDN (good)