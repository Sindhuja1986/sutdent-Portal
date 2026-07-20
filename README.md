# 🎓 Freshers Portal

A centralized, responsive web app that gives new college students everything they need on day one:
campus events, clubs, faculty directory, personal timetable, a color-coded calendar, guided
registration forms, and a Gemini-powered AI chatbot that answers questions using live app data.

Built with **Next.js 14 (App Router)**, **TypeScript**, **Tailwind CSS**, **shadcn/ui-style
components**, **Prisma + PostgreSQL**, **NextAuth.js**, and **Google Gemini**.

---

## ✨ Features

- **Dashboard** — personalized welcome, live weather widget, upcoming events carousel, and a
  prominent Freshers Welcome Party banner.
- **Campus Events** — search/filter by category, event detail modal, one-click registration with
  waitlisting when an event is full.
- **Clubs & Societies** — searchable directory with meeting times, venues, and upcoming club events.
- **Faculty Directory** — fast search across name, department, designation, and email.
- **Timetable** — personal weekly schedule, add/delete classes, color-coded, persisted per user.
- **Calendar** — month view combining events, exams, holidays, and club meetings, color-coded by
  category.
- **Registration Hub** — guided, validated forms for course, hostel, and library registration.
- **AI Chatbot (Gemini)** — floating chat widget with streaming, markdown-formatted answers grounded
  in real data pulled from the database (events, clubs, faculty, and the signed-in student's
  timetable).
- **Dark/light mode**, mobile-first responsive design, toast notifications, loading states.

---

## 🧱 Tech Stack

| Layer          | Choice                                             |
|----------------|-----------------------------------------------------|
| Framework      | Next.js 14 (App Router), React 18, TypeScript       |
| Styling        | Tailwind CSS + shadcn/ui-style components + Lucide  |
| Auth           | NextAuth.js (Credentials + Google OAuth)            |
| Database       | PostgreSQL via Prisma ORM                           |
| AI Chatbot     | Google Gemini (`@google/generative-ai`, streaming)  |
| Weather        | Open-Meteo (free, no API key required)              |
| Deployment     | Vercel                                              |

---

## 📁 Project Structure

```
freshers-portal/
├── prisma/
│   ├── schema.prisma        # All data models (users, events, clubs, faculty, timetable, etc.)
│   └── seed.ts               # Sample data seeding script
├── src/
│   ├── app/
│   │   ├── page.tsx           # Dashboard
│   │   ├── login/              # Sign-in page
│   │   ├── events/             # Campus events
│   │   ├── clubs/               # Clubs & societies
│   │   ├── faculty/              # Faculty directory
│   │   ├── timetable/             # Personal timetable
│   │   ├── calendar/               # Color-coded calendar
│   │   ├── registration/            # Registration hub
│   │   └── api/                      # All API routes (events, clubs, faculty, timetable,
│   │                                    registrations, chat, weather, auth)
│   ├── components/
│   │   ├── ui/                # Reusable primitives (button, card, dialog, etc.)
│   │   ├── layout/             # Navbar
│   │   ├── dashboard/           # Welcome banner, weather widget, events carousel
│   │   ├── events/                # Event card & modal
│   │   ├── clubs/, faculty/, timetable/, calendar/, chatbot/
│   ├── lib/                    # prisma.ts, auth.ts, gemini.ts, weather.ts, utils.ts
│   └── types/                   # Shared TS types (NextAuth session augmentation)
├── .env.example
├── package.json
└── README.md (this file)
```

---

## 🚀 Local Setup

### 1. Prerequisites

- Node.js 18.17+ (Node 20 LTS recommended)
- A PostgreSQL database — locally installed, or a free hosted instance
  ([Neon](https://neon.tech), [Supabase](https://supabase.com), or Vercel Postgres all work)
- A Google Gemini API key from [Google AI Studio](https://aistudio.google.com/app/apikey)
- (Optional) Google OAuth credentials from the
  [Google Cloud Console](https://console.cloud.google.com/apis/credentials) if you want "Sign in
  with Google"

### 2. Install dependencies

```bash
cd freshers-portal
npm install
```

### 3. Configure environment variables

Copy the example file and fill in your values:

```bash
cp .env.example .env.local
```

```env
DATABASE_URL="postgresql://user:password@localhost:5432/freshers_portal"
NEXTAUTH_SECRET="run: openssl rand -base64 32"
NEXTAUTH_URL="http://localhost:3000"
GOOGLE_CLIENT_ID=""
GOOGLE_CLIENT_SECRET=""
GEMINI_API_KEY="your-gemini-api-key"
CAMPUS_LAT="13.0827"
CAMPUS_LON="80.2707"
CAMPUS_NAME="Your College Name"
```

> The weather widget uses the free Open-Meteo API and needs no key — just set `CAMPUS_LAT` /
> `CAMPUS_LON` to your campus coordinates.

### 4. Set up the database

```bash
npm run db:generate   # generate the Prisma client
npm run db:push       # push the schema to your database (or use db:migrate for migrations)
npm run db:seed       # populate sample events, clubs, faculty, and a demo student
```

This creates a demo account you can sign in with immediately:

```
Email:    alex.student@college.edu
Password: password123
```

### 5. Run the dev server

```bash
npm run dev
```

Visit **http://localhost:3000** 🎉

---

## 🔑 Setting up Google OAuth (optional)

1. Go to the [Google Cloud Console](https://console.cloud.google.com/apis/credentials) and create
   an OAuth 2.0 Client ID (Web application).
2. Add authorized redirect URI: `http://localhost:3000/api/auth/callback/google`
   (and your production URL + `/api/auth/callback/google` once deployed).
3. Copy the Client ID and Client Secret into `.env.local`.

---

## 🤖 How the AI Chatbot Works

The chatbot (`src/lib/gemini.ts` + `src/app/api/chat/route.ts`) is context-aware: on every message,
the server queries Prisma for upcoming events, clubs, faculty, and (if signed in) the student's
timetable, and injects a compact summary into Gemini's system context. This means answers like
*"show coding club events this week"* or *"help me with Monday's timetable"* are grounded in your
actual database content rather than being generic. Responses are streamed token-by-token to the
floating chat widget and rendered as markdown.

---

## ☁️ Deploying to Vercel

1. Push this project to a GitHub/GitLab/Bitbucket repository.
2. Go to [vercel.com/new](https://vercel.com/new) and import the repository.
3. Add the same environment variables from `.env.local` in the Vercel project settings
   (**Settings → Environment Variables**). Set `NEXTAUTH_URL` to your production URL
   (e.g. `https://your-app.vercel.app`).
4. For the database, use a serverless-friendly Postgres provider (Neon, Supabase, or Vercel
   Postgres) — traditional local Postgres won't be reachable from Vercel's servers.
5. After the first deploy, run migrations against your production database:
   ```bash
   DATABASE_URL="<production-url>" npx prisma migrate deploy
   DATABASE_URL="<production-url>" npx prisma db seed   # optional: seed sample data
   ```
6. Update the Google OAuth redirect URI to include your production callback URL.
7. Redeploy — you're live!

---

## 🗃️ Prisma Schema Overview

- **User / Account / Session / VerificationToken** — NextAuth-compatible auth models, with `role`
  (`STUDENT`, `CLUB_ADMIN`, `STAFF`, `ADMIN`) for future role-based access.
- **Event / EventRegistration** — events with category, venue, timing, optional capacity, and
  per-user registration (with automatic waitlisting).
- **Club** — clubs/societies with meeting info and a relation to the events they host.
- **Faculty** — searchable directory with office hours and contact info.
- **TimetableEntry** — per-user weekly class schedule.
- **RegistrationSubmission** — flexible JSON-backed submissions for course/hostel/library forms.
- **ChatSession / ChatMessage** — persistent chatbot history (schema included; wire up to the chat
  route if you want full session persistence beyond the current in-memory widget state).

---

## 🛠️ Useful Scripts

| Command              | Description                                  |
|-----------------------|-----------------------------------------------|
| `npm run dev`         | Start the dev server                          |
| `npm run build`       | Production build                              |
| `npm run start`       | Start the production server                   |
| `npm run db:generate` | Regenerate the Prisma client                  |
| `npm run db:push`     | Push schema changes to the database (no migration history) |
| `npm run db:migrate`  | Create/apply a dev migration                  |
| `npm run db:seed`     | Seed sample data                              |
| `npm run db:studio`   | Open Prisma Studio to browse/edit data        |

---

## 📌 Notes & Next Steps

- Chat history is currently kept in the widget's local state per session; the `ChatSession` /
  `ChatMessage` models are ready if you want to persist conversations per user.
- Role-based access control fields exist on `User`, but all routes currently treat every signed-in
  user as a student — extend the API routes with role checks for club-admin/staff dashboards.
- The event/club/faculty "create" endpoints are open for demo purposes — add auth + role checks
  before exposing them publicly.
