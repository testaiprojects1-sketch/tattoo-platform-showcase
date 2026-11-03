# ADR 001: Next.js + Supabase + Stripe for Creator Platform

**Date:** 2025-11-01
**Status:** Accepted
**Deciders:** Jane Riveros

## Context
Need: portfolio, course sales, memberships, booking, payments, video streaming — artist-owned, white-label.

## Decision
Next.js 14 + Supabase (PostgreSQL + Auth + Realtime) + Stripe (Checkout, Billing, Connect) + Vercel.

## Rationale
- Stripe: Best-in-class for creator payments (low fees, global, subscriptions, Connect for future marketplace)
- Supabase: Managed PostgreSQL, Auth, Realtime for booking sync
- Next.js: Image optimization, API routes, SEO, Vercel deploy
- Vercel: Edge caching for global portfolio performance

## Consequences
- Stripe fees (2.9% + 30¢) + Connect fees if marketplace
- Vendor lock-in (Stripe/Supabase/Vercel)
- PCI-DSS via Stripe (SAQ A) — minimal compliance burden
- Video hosting external (Mux/Cloudflare Stream)