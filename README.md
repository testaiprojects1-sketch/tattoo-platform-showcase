# Tattoo Artist Platform — Showcase
**Artist portfolio + course marketplace for tattoo education — full-stack Next.js with Stripe, Supabase, and modern UX.**  
*Public architecture showcase — implementation is private.*

[![Status](https://img.shields.io/badge/Status-Production%20Ready-00C9A7)]()
[![Governance](https://img.shields.io/badge/Governance-By%20Design-0B2D4A)]()
[![License](https://img.shields.io/badge/License-Private-E85D4D)]()

---

## 🎯 Problem & Outcome

**Problem:** Tattoo artists need: professional portfolio, course/membership sales, client booking, and community — but existing platforms are either generic (Squarespace, Wix) or marketplace-only (no brand ownership). Artists lose 15-30% to platforms and can't customize the client experience.

**Outcome:**
- `Artist portfolio`: Customizable, image-optimized, SEO-ready gallery
- `Course marketplace`: Video courses, memberships, digital downloads — Stripe-powered
- `Client booking`: Integrated calendar, deposits, automated reminders
- `Analytics`: Revenue, enrollment, engagement — owned data
- `Deployed`: Live at julio-canales site (public) + julio-canales-cursos (course platform)

**Domain:** Creator Economy / EdTech · **Regulatory touchpoints:** PCI-DSS (Stripe), GDPR (client data), Chilean consumer protection

---

## 🎥 Demo

| Format | Link |
|--------|------|
| **Full walkthrough (5-8 min)** | [🎥 YouTube / Loom / Vimeo](https://...) |
| **Quick demo (60 sec GIF)** | ![Demo](assets/demo.gif) |
| **Artist portfolio** | [julio-canales](https://github.com/testaiprojects1-sketch/julio-canales) |
| **Course platform** | [julio-canales-cursos](https://github.com/testaiprojects1-sketch/julio-canales-cursos) |

---

## 📐 Architecture

### System Overview
```mermaid
%%{init: {'theme': 'base', 'themeVariables': {
  'primaryColor': '#0B2D4A',
  'primaryTextColor': '#fff',
  'primaryBorderColor': '#00C9A7',
  'lineColor': '#888',
  'secondaryColor': '#1A1A2E',
  'tertiaryColor': '#E85D4D',
  'background': 'transparent',
  'mainBkg': 'transparent',
  'secondBkg': 'transparent',
  'tertiaryBkg': 'transparent'
}}}%%
flowchart LR
    subgraph Public[Public Site]
        Portfolio[Artist Portfolio]
        Gallery[Image Gallery]
        SEO[SEO + Sitemap]
    end
    subgraph Courses[Course Platform]
        Catalog[Course Catalog]
        Player[Video Player]
        Membership[Memberships]
        Checkout[Stripe Checkout]
    end
    subgraph Booking[Booking System]
        Calendar[Availability Calendar]
        Deposit[Deposit Collection]
        Reminders[Automated Reminders]
    end
    subgraph Backend[Backend (Next.js API + Supabase)]
        Auth[Auth + RLS]
        API[API Routes]
        PG[(PostgreSQL)]
        Webhooks[Stripe Webhooks]
    end
    subgraph Governance[Governance]
        Audit[Audit Logger]
        Analytics[Analytics Events]
        Privacy[GDPR Compliance]
    end

    Portfolio --> Auth
    Gallery --> API
    SEO --> API
    Catalog --> Auth
    Player --> API
    Membership --> Checkout
    Checkout --> Webhooks
    Calendar --> API
    Deposit --> Checkout
    Reminders --> API
    API --> PG
    Webhooks --> PG
    API --> Audit
    Audit --> Analytics
    Analytics --> Privacy

    style Public fill:#0B2D4A,color:#fff
    style Courses fill:#1A1A2E,color:#fff
    style Booking fill:#1A1A2E,color:#fff
    style Backend fill:#00C9A7,color:#0B2D4A
    style Governance fill:#E85D4D,color:#fff
```

### High-Level Data Flow (Excalidraw)
![Architecture Overview](diagrams/system-overview.svg)  
*Hand-drawn style for architectural thinking — [editable Excalidraw file](diagrams/system-overview.excalidraw)*

### Key Components

| Component | Responsibility | Tech | Governance Hook |
|-----------|----------------|------|-----------------|
| `Artist Portfolio` | Customizable gallery, bio, press, contact | Next.js 14, React 18, TypeScript, Tailwind, Framer Motion | Analytics events (privacy-respecting) |
| `Course Platform` | Video courses, memberships, downloads, progress | Next.js, Mux/Cloudflare Stream, Stripe | Enrollment audit; revenue tracking |
| `Booking System` | Calendar, deposits, reminders, client CRM | Supabase Realtime, Stripe, nodemailer | Booking audit; deposit reconciliation |
| `Stripe Integration` | Payments, subscriptions, webhooks, payouts | Stripe SDK, webhook verification | PCI-DSS via Stripe; audit trail |
| `Image Optimization` | Portfolio images, course thumbnails, WebP/AVIF | Next.js Image, Cloudflare Images | CDN caching; no PII in filenames |
| `Analytics` | Revenue, enrollment, engagement, funnel | Custom events → PostgreSQL → Dashboard | GDPR: no personal data in analytics |

---

## ⚙️ Tech Stack (What I Built vs. Leveraged)

| Layer | Built by Me | Leveraged / Configured |
|-------|-------------|------------------------|
| **Frontend** | Portfolio, course catalog, video player, booking UI, dashboard | Next.js 14, React 18, TypeScript, Tailwind, Framer Motion, GSAP, Radix UI |
| **Backend** | API routes, Stripe webhooks, booking logic, Supabase schema | Supabase (PostgreSQL + Auth + Realtime), Stripe, nodemailer |
| **Payments** | Checkout sessions, subscription management, webhook handlers | Stripe (Checkout, Billing, Connect) |
| **Video** | Course streaming, progress tracking, DRM | Mux / Cloudflare Stream |
| **Data** | User profiles, courses, enrollments, bookings, analytics | Supabase, PostgreSQL, RLS |
| **Governance** | **Audit logger, analytics privacy, GDPR compliance, PCI-DSS via Stripe** | (Private — not in showcase) |
| **Infra & Deploy** | Vercel config, Supabase migrations, GitHub Actions CI/CD | Vercel, Supabase CLI, GitHub Actions, Stripe CLI |

> **Governance-by-design note:** All payments via Stripe (PCI-DSS SAQ A). Analytics events anonymized. Booking deposits audited. GDPR: data export/deletion endpoints. RLS on user data.

---

## 🛡️ Governance & Compliance Artifacts

| Artifact | Location | Purpose |
|----------|----------|---------|
| **Model Card** | `docs/model-card.md` | Intended use, limitations, eval results, data lineage |
| **Risk Register** | `docs/risk-register.csv` | Identified risks, likelihood, impact, mitigations, owner |
| **Eval Harness** | `evals/` | Regression tests, security tests, threshold gates |
| **Audit Log Schema** | `docs/audit-log-schema.json` | Structured schema for every business event |
| **ADRs** | `docs/adr/` | Architecture Decision Records (numbered, dated) |

---

## 🚀 How to Run (High-Level)

> **Implementation is private.** This section describes the *operational model* for reference.

```bash
# 1. Infrastructure
supabase db push
stripe listen --forward-to localhost:3000/api/webhooks/stripe

# 2. Frontend
cd apps/web && npm install && npm run dev

# 3. Eval harness (local)
cd evals && python -m pytest --tb=short
```

**Environment variables required** (see `.env.example` in private repo):
- `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`, `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET`, `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY`, `MUX_TOKEN_ID`, `MUX_TOKEN_SECRET`, `AUDIT_LOG_LEVEL`

---

## 📁 Repository Structure

```
tattoo-platform-showcase/
├── README.md                    ← This file
├── ARCHITECTURE.md              ← Deep-dive: decisions, trade-offs, failure modes
├── diagrams/
│   ├── system-overview.mermaid  ← Mermaid source for system diagram
│   ├── system-overview.svg      ← Excalidraw export (architecture)
│   ├── data-flow.mermaid
│   ├── data-flow.svg
│   ├── sequence-purchase.mermaid
│   ├── sequence-booking.mermaid
│   └── sequence-webhook.mermaid
├── assets/
│   ├── demo.gif                 ← 60-sec demo GIF (<2MB, 720p)
│   ├── screenshot-portfolio.png ← Artist portfolio (1440px)
│   ├── screenshot-courses.png   ← Course catalog
│   └── screenshot-dashboard.png ← Artist dashboard
├── docs/
│   ├── model-card.md
│   ├── risk-register.csv
│   ├── audit-log-schema.json
│   └── adr/
│       ├── 001-nextjs-supabase-stripe.md
│       ├── 002-video-streaming.md
│       ├── 003-stripe-integration.md
│       ├── 004-booking-system.md
│       └── 005-analytics-privacy.md
└── evals/
    ├── test_regression.py
    ├── test_security.py
    └── thresholds.yaml
```

---

## 🔗 Private Repo Access

**Full implementation, CI/CD, infra-as-code, and eval runs** are in a private repository.

**Access process:**  
1. Initial screening call (30 min)  
2. Mutual NDA (template provided)  
3. Temporary read access to private repo + 1-hour walkthrough with me  
4. Access revoked after evaluation period

**Contact:** `jdriveros232@gmail.com` — subject: `Tattoo Platform Private Repo Access`

---

## 📌 Related Showcases

- [Novaxis OS / Atlas](https://github.com/testaiprojects1-sketch/novaxis-showcase)
- [MAVIS — Personal AI](https://github.com/testaiprojects1-sketch/mavis-showcase)
- [Mission Control](https://github.com/testaiprojects1-sketch/mission-control-showcase)
- [Dental Clinic Platform](https://github.com/testaiprojects1-sketch/dental-clinic-showcase)
- [All showcases](https://github.com/testaiprojects1-sketch?tab=repositories&q=showcase)

---

*Part of the **testaiprojects1-sketch** AI Systems Engineering portfolio — Governance by Design.*