# Taskflow — Task Manager

A responsive task manager built with **Next.js 14, TypeScript, Supabase and Tailwind CSS**, ready to push to GitHub and deploy on Vercel.

## Features

- Create, edit and delete tasks with start date, due date, description and priority.
- Track status: To do, In progress and Done.
- Dashboard metrics for total, active, completed and overdue tasks.
- Completion percentage updates automatically as tasks are marked done.
- Search and filter tasks, plus a coming-up deadlines panel.
- Browser notifications for tasks due today (while the app is open).
- Supabase email/password authentication and per-user database security.
- Demo mode if Supabase environment variables are not configured.

## Run locally

1. Install Node.js 18.17+.
2. Unzip the project and open a terminal in the project folder.
3. Install packages:
   ```bash
   npm install
   ```
4. Copy `.env.example` to `.env.local` and fill in your Supabase project URL and anon key.
5. Start the app:
   ```bash
   npm run dev
   ```
6. Open http://localhost:3000.

Without Supabase credentials, the app opens in demo mode. Demo tasks are in-memory and will reset on refresh; connect Supabase for persistent accounts and tasks.

## Set up Supabase

1. Create a project at https://supabase.com.
2. In **Project Settings → API**, copy the Project URL and `anon` / publishable key.
3. Add them to `.env.local`:
   ```env
   NEXT_PUBLIC_SUPABASE_URL=https://YOUR_PROJECT.supabase.co
   NEXT_PUBLIC_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
   ```
4. Open **SQL Editor** in Supabase and run the contents of `supabase/schema.sql`.
5. In **Authentication → Providers**, enable Email. For a quick test, you can disable email confirmation; for production, keep confirmation enabled and configure SMTP.
6. Restart `npm run dev`.

The SQL enables Row Level Security. A signed-in user can only read or change their own tasks. Never put a Supabase service-role key in this app or in a `NEXT_PUBLIC_` variable.

## Deploy with GitHub + Vercel

1. Create a new GitHub repository and push this folder:
   ```bash
   git init
   git add .
   git commit -m "Initial Taskflow app"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
   git push -u origin main
   ```
2. Sign in to https://vercel.com and choose **Add New → Project**.
3. Import the GitHub repository. Vercel detects Next.js automatically.
4. In Vercel → Project → Settings → Environment Variables, add:
   - `NEXT_PUBLIC_SUPABASE_URL`
   - `NEXT_PUBLIC_SUPABASE_ANON_KEY`
5. Deploy. Add the same environment variables for Preview and Production if needed.

## Alerts: current scope and next step

The included reminders use the browser Notifications API. They notify about tasks due **today**, once per task per day, while the app is open and notification permission is granted. Browser notifications are not guaranteed if the browser is closed, the device is asleep, or permission is blocked.

For reliable reminders when the app is closed, add a scheduled server-side job (for example, Vercel Cron) plus an email provider or web-push subscription service. The current project intentionally keeps this first version lightweight and does not claim background push/email delivery.

## Progress calculation

Overall progress = completed tasks ÷ total tasks × 100. A task is completed when its status is set to `Done`. Overdue means the due date is before today and the task is not done.

## Useful scripts

- `npm run dev` — local development
- `npm run build` — production build
- `npm start` — run production build locally
