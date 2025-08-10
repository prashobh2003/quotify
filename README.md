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
app/
(public)/ → Marketing or public routes
(client)/client → Client dashboard, jobs
(provider)/provider → Provider dashboard, jobs
(admin)/admin → Admin panel
api/ → API routes or server actions
components/ → Shared UI components
lib/ → Utilities (auth, db, validations, rate-limit)
prisma/ → schema.prisma, migrations, seed.ts
styles/ → globals.css
tests/ → Unit and E2E tests
scripts/ → Seeding and maintenance scripts


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

