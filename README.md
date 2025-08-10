# Quotify
_A two-sided marketplace connecting Clients and Providers for service-based jobs._

---

## Overview
Quotify is a platform where:
- **Clients** post jobs via a guided chat-like intake.
- **Providers** submit quotes and collaborate with clients through messaging until project completion.
- **Admins** manage categories, users, and analytics.

---

## Core Features
- **Authentication & Roles:** Client, Provider, Admin (role-based access)
- **Job Intake:** Multi-step or chat-like form with automated job summary
- **Quotes:** Amount, timeline, short pitch from providers
- **Dashboards:**
  - Client: jobs, quotes, spend summary
  - Provider: jobs, quotes, earnings
  - Admin: categories, users, jobs, analytics
- **Messaging:** Per-job chat after quote acceptance
- **Reviews:** Ratings and feedback upon completion
- **Notifications:** Email + in-app (quotes, messages, status changes)
- **Analytics:** Jobs, averages per category, platform volume

---

## Tech Stack
- **Framework:** Next.js (App Router) + TypeScript  
- **UI:** Tailwind CSS, shadcn/ui  
- **Auth:** NextAuth.js  
- **Database:** PostgreSQL + Prisma  
- **Hosting:** Vercel (frontend + API)  
- **DB Hosting:** Neon or Supabase  
- **Email:** Resend or Postmark  
- **Realtime (optional):** Pusher or Ably  
- **Storage (optional):** UploadThing or Supabase Storage  
- **Payments (optional):** Stripe Connect  

---

## Folder Structure
```
app/
  (public)/         → Marketing or public routes
  (client)/client   → Client dashboard, jobs
  (provider)/provider → Provider dashboard, jobs
  (admin)/admin     → Admin panel
  api/              → API routes or server actions
components/         → Shared UI components
lib/                → Utilities (auth, db, validations, rate-limit)
prisma/             → schema.prisma, migrations, seed.ts
styles/             → globals.css
tests/              → Unit and E2E tests
scripts/            → Seeding and maintenance scripts
```

---

## Data Model (Prisma Sketch)
- **User:** `id`, `email`, `passwordHash`, `role`, `createdAt`
- **Profile:** `userId`, `name`, `avatarUrl`, `about`, `skills[]`, `portfolioLinks[]`, `ratingAvg`, `ratingCount`
- **Category:** `id`, `name`, `slug`, `active`
- **Job:** `id`, `clientId`, `categoryId`, `title`, `description`, `budgetMin`, `budgetMax`, `deadline`, `status`
- **JobIntake:** `id`, `jobId`, `intakeJson`, `summary`
- **Quote:** `id`, `jobId`, `providerId`, `amount`, `timelineWeeks`, `pitch`, `status`
- **Message:** `id`, `jobId`, `senderId`, `content`, `createdAt`
- **Review:** `id`, `jobId`, `reviewerId`, `revieweeId`, `rating`, `comment`
- **Notification:** `id`, `userId`, `type`, `payloadJson`, `readAt`
- **Transaction (optional):** `id`, `jobId`, `payerId`, `payeeId`, `amount`, `status`

---

## Getting Started

### Prerequisites
- Node.js LTS
- pnpm or npm
- PostgreSQL (Neon/Supabase recommended)
- Vercel account
- Email provider key (Resend/Postmark)

---

### Installation
```
git clone <your-repo-url>
cd quotify
pnpm install
```

---

### Environment Variables
Create `.env` (and `.env.local` for local dev):
```
DATABASE_URL=postgres://user:pass@host:port/db
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=your-strong-secret
EMAIL_FROM=notifications@yourdomain.com
RESEND_API_KEY=your_resend_key
# Optional:
PUSHER_APP_ID=...
PUSHER_KEY=...
PUSHER_SECRET=...
PUSHER_CLUSTER=...
STRIPE_SECRET_KEY=...
STRIPE_WEBHOOK_SECRET=...
```

---

### Database Setup
```
pnpm prisma generate
pnpm prisma migrate dev
pnpm ts-node prisma/seed.ts
```

---

### Run Development Server
```
pnpm dev
```
App runs at: **http://localhost:3000**

---

## Feature Walkthrough

### Clients
- Post a job via chat-like or multi-step form
- Get auto-generated job summary
- Receive & compare provider quotes
- Accept a quote → starts project + messaging thread
- Dashboard: jobs, statuses, spend summary
- Leave provider reviews

### Providers
- Browse/filter jobs
- Submit quotes (price, timeline, pitch)
- Chat after quote acceptance
- Dashboard: jobs, quotes, earnings
- Profile: skills, portfolio, about

### Admins
- Manage categories, users, jobs
- Verify providers, handle disputes
- View platform analytics

---

## Security & Access
- Role-based routes via middleware + server-side checks
- Zod validation for all writes
- Rate limiting for sensitive actions
- File upload validation (if enabled)

---

## Scripts
```
pnpm dev        # Start dev server
pnpm build      # Build for production
pnpm start      # Start production server
pnpm prisma:generate
pnpm prisma:migrate
pnpm seed
pnpm test
pnpm lint
pnpm format
```

---

## Testing
- **Unit:** Zod schemas & server actions
- **E2E:** Playwright — register → post job → quote → accept → message → complete → review
- Use a dedicated test database with seed fixtures

---

## Deployment
- **Frontend/API:** Vercel
- **Database:** Neon/Supabase (pooled connections)
- **Prisma:** Ensure `prisma generate` runs during build
- **Webhooks:** `/api/stripe/webhook` (if using payments)

---

## Roadmap
- **v1 (MVP):** Auth, intake, quotes, messaging, dashboards, email notifications, reviews, admin basics
- **v1.1:** Chat-like intake UI, in-app notifications, search & filters
- **v1.2:** Realtime chat, provider verification, advanced analytics
- **v2:** Stripe escrow, AI-assisted intake & summaries

---

## Troubleshooting
- **DB connection errors:** Check `DATABASE_URL` & run migrations
- **Missing roles in session:** Update NextAuth callbacks
- **403 errors:** Verify middleware & role checks
- **Email issues:** Confirm API keys & sender domain
- **Vercel build fails:** Ensure `prisma generate` runs

---

## License
MIT License (recommended) — add a `LICENSE` file.

---

## Credits
- Built with **Next.js**, **Prisma**, **Tailwind CSS**, **shadcn/ui**
- Initial product spec & implementation plan by the Quotify team

---

## Screenshots (Placeholder)
_Add screenshots or GIFs here:_
- Client job posting
- Provider job list & quote form
- Messaging thread
- Admin dashboard
