# TraviXO Systems

**QR-based asset tracking and VGP compliance automation for French equipment rental companies.**

[travixosystems.com](https://travixosystems.com)

---

## The Problem

French equipment rental companies manage thousands of assets across multiple depots. Every piece of lifting and handling equipment must pass a **VGP inspection** (*Verification Generale Periodique*) mandated by the French Labour Code (Code du Travail, R4323-23 to R4323-27). Missing a deadline means regulatory fines, liability exposure, and potential site shutdowns.

Most operators still track this with spreadsheets. Deadlines slip. Assets go missing. Compliance gaps only surface during DIRECCTE audits — when it's too late.

TraviXO replaces that with a purpose-built SaaS platform: scan a QR code, know the status, never miss an inspection.

---

## What It Does

- **Asset Tracking** — QR code-based real-time tracking across depots and job sites. Scan to check status, location, and compliance in seconds.
- **VGP Compliance Engine** — Automated inspection scheduling with 30/15/7/1-day email alerts. Tracks inspection results (conforme, conditionnel, non conforme) with full audit history.
- **Rental System** — Equipment checkout/return tracking with client association and recall notifications for assets due for inspection while on-site.
- **Bulk Operations** — Import 500+ assets from Excel in under 5 minutes with smart column detection. Generate 500 QR codes as print-ready PDFs in 30 seconds.
- **Team Management** — Role-based access control (owner, admin, manager, member) with secure email invitations.
- **Digital Audits** — Full inventory audits with missing asset notifications and PDF export.
- **Subscription Billing** — Stripe-integrated tiered plans with feature gating.
- **Multi-language** — French (primary) and English, with full i18n across the application.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Framework** | Next.js 15 (App Router, Server Components) |
| **Language** | TypeScript (strict mode) |
| **Database** | Supabase (PostgreSQL) with Row Level Security |
| **Auth** | Supabase Auth (email/password, cookie-based sessions) |
| **Payments** | Stripe (checkout, portal, webhooks) |
| **Email** | Resend + React Email (transactional templates) |
| **File Storage** | UploadThing (VGP certificates, bulk QR PDFs) |
| **Styling** | Tailwind CSS |
| **State** | React Query (TanStack Query) |
| **Monitoring** | Sentry (client + server error tracking) |
| **Hosting** | Vercel |

---

## Architecture Highlights

- **Multi-tenant by design** — Every table scoped to `organization_id`. Supabase RLS policies enforce data isolation at the database level, independent of application code.
- **Server Components first** — Leverages Next.js App Router for reduced client JS bundles. API routes handle server-side operations.
- **Feature gating** — Subscription tier checks run server-side via RPC. Custom hooks (`useSubscription`, `useOrganization`) provide client-side feature gates.
- **Security hardened** — CSRF protection via origin verification, per-IP rate limiting (sliding window), Stripe webhook HMAC validation, HSTS with preload, strict CSP headers.
- **Email automation** — Cron-triggered VGP alert pipeline with deduplication, daily send limits, and per-organization notification preferences.
- **PDF generation** — Client-side bulk QR code generation (jsPDF) and server-side audit reports.

---

## Project Structure

```
app/
  (auth)/          # Login, signup, password reset, email verification
  (dashboard)/     # Protected routes: assets, VGP, audits, settings, team
  api/             # 60+ API routes: CRUD, Stripe, cron, email
  scan/            # Public QR code scan endpoint
components/        # Feature-organized React components
  assets/          # Asset CRUD, QR generation, Excel import
  vgp/             # VGP dashboard, schedule management
  rental/          # Checkout, return, client autocomplete
  subscription/    # Feature gates, plan cards
hooks/             # useOrganization, useProfile, useSubscription
lib/
  billing/         # Entitlements engine, feature guards
  email/           # Email service, React Email templates
  security/        # CSRF, rate limiting
  supabase/        # Client and server Supabase instances
  validations/     # Zod schemas for API input validation
types/             # TypeScript types (database schema, VGP, alerts)
supabase/
  migrations/      # SQL migrations (billing, teams, rentals, pilots)
```

---

## Getting Started

```bash
# Clone and install
git clone <repo-url>
cd travixo-app
npm install

# Configure environment
cp .env.example .env.local
# Fill in your Supabase, Stripe, Resend, and UploadThing credentials

# Run development server
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

---

## Built By

Solo-founded and built end-to-end — from database schema to deployment pipeline. Designed for the operational realities of French equipment rental: warehouse environments, field workers scanning with gloves on, and regulatory deadlines that don't wait.

---

*This is a portfolio version of the TraviXO codebase. Some configuration values have been replaced with placeholders.*
