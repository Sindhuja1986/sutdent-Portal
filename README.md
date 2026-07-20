# Freshers Portal

A one-stop web portal to help incoming students settle into campus life — events, clubs, faculty, timetable, registration, and an AI assistant, all in one place.

## Unique Features

- 🤖 **AI Chatbot** for instant assistance and queries — helping freshers with campus FAQs, event info, and timetable help.
- 🗓️ **Real-time personalized timetable and registration system**, so every student sees a schedule and course/event registration flow tailored to them.
- 📱 **Responsive, modern UI** tailored for mobile-first freshers — built to feel great on a phone first, desktop second.
- 🗄️ **Centralized SQL-powered database** for all campus data — events, clubs, faculty, users, and registrations in one consistent source of truth.
- 🎨 **High-quality AI-generated Welcome Party Poster** using Nano Banana — eye-catching visuals for the Freshers Welcome Party without needing a designer.
- 🛠️ **Easy admin capabilities** for faculty and club admins to update events and clubs without touching code.

## Tech Stack

- **Framework:** Next.js (App Router) with TypeScript
- **Database/ORM:** PostgreSQL via Prisma
- **UI:** Tailwind-based component library (buttons, dialogs, tabs, cards, etc.)
- **Auth:** Role-based accounts (Student, Club Admin, Staff, Admin) with OAuth/credentials support

## Project Structure

```
prisma/            Database schema and seed data
src/app/            Pages (timetable, registration, API routes)
src/components/
  chatbot/          AI assistant widget
  dashboard/        Welcome banner, weather widget, events carousel
  timetable/        Timetable grid
  events/           Event cards and modals
  clubs/             Club cards
  faculty/           Faculty cards
  calendar/          Calendar view
  layout/            Navbar and shared layout
  ui/                Base UI primitives
src/types/          Shared TypeScript types
```

## Getting Started

```bash
npm install
npx prisma migrate dev
npx prisma db seed
npm run dev
```

Set `DATABASE_URL` in a `.env` file pointing at your PostgreSQL instance before running migrations.
