# Notes App

A modern, responsive note-taking app built with **Next.js 14**, **TypeScript**, **Tailwind (shadcn/ui)**, and **Supabase**.

---

## ✨ Features

- Create, edit, delete notes
- Pin/unpin notes (pinned appear first)
- Color-coded notes (6 presets)
- Search by title and content
- Optimistic UI + toasts
- Responsive layout (mobile → desktop)

---

## 🛠 Tech Stack

- **Frontend:** Next.js 14 (App Router), TypeScript, Tailwind CSS, shadcn/ui
- **Backend:** Supabase (Postgres + RLS), @supabase/supabase-js
- **Deploy:** Vercel

---

## 🚀 Quick Start

### 1) Install
```bash
npm install
```

### 2) Environment
Create a file named **.env.local** in the project root (this file is gitignored) and add:
```bash
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
```
There’s a template at `.env.example` you can copy.

### 3) Run Dev Server
```bash
npm run dev
```
Local: http://localhost:3000

---

## 🗄️ Supabase Setup

Create a project at https://supabase.com, then add a `notes` table with columns:

| Column      | Type      | Default           | Notes                                           |
|-------------|-----------|-------------------|-------------------------------------------------|
| id          | uuid      | gen_random_uuid() | Primary key                                     |
| title       | text      | —                 | Nullable                                        |
| content     | text      | —                 | Required                                        |
| color       | text      | 'default'         | One of: default/yellow/green/blue/pink/purple   |
| pinned      | boolean   | false             |                                                 |
| created_at  | timestamp | now()             |                                                 |
| updated_at  | timestamp | now()             | (Optional) update on write trigger              |

> **Development policies (permissive):**
> Enable RLS and allow all operations while developing. Tighten before prod.
>
> ```sql
> alter table public.notes enable row level security;
>
> drop policy if exists "dev select" on public.notes;
> drop policy if exists "dev insert" on public.notes;
> drop policy if exists "dev update" on public.notes;
> drop policy if exists "dev delete" on public.notes;
>
> create policy "dev select" on public.notes for select to public using (true);
> create policy "dev insert" on public.notes for insert to public with check (true);
> create policy "dev update" on public.notes for update to public using (true) with check (true);
> create policy "dev delete" on public.notes for delete to public using (true);
> ```

---

## 📦 Scripts

```bash
npm run dev     # start dev server
npm run build   # production build
npm run start   # run production build locally
```

---

## 📁 Project Structure

```
notes-app/
├── app/
│   ├── layout.tsx
│   └── page.tsx            # notes list, search, composer, editor
├── components/
│   ├── NoteCard.tsx
│   ├── NoteComposer.tsx
│   ├── NoteEditorDialog.tsx
│   └── ui/                 # shadcn/ui primitives
├── hooks/
│   └── use-toast.ts
├── lib/
│   └── supabase.ts         # supabase client + CRUD
├── public/
├── styles/ (or app/globals.css)
├── .env.example
├── README.md
└── package.json
```

---

## ☁️ Deploy to Vercel

1. Push the repo to GitHub.
2. In Vercel, **Import Project** → select the repo.
3. Add Environment Variables:
   - `NEXT_PUBLIC_SUPABASE_URL`
   - `NEXT_PUBLIC_SUPABASE_ANON_KEY`
4. Deploy.

---

## 🔧 Troubleshooting

- **400 Bad Request on insert/update**  
  Make sure your Supabase table has all columns used by the app (`color`, `pinned`, etc.). The payload must match column names.

- **“Could not find the 'color' column”**  
  Add the `color` text column (default `'default'`), then redeploy/rerun.

- **Cannot update/delete (RLS errors)**  
  In dev, use permissive policies (allow all). In prod, write proper auth-based policies.

- **Pinned notes don’t move**  
  Ensure the client sorts: pinned first, then `created_at` desc. (This repo’s `page.tsx` already does it.)

---

## 📝 License

MIT
