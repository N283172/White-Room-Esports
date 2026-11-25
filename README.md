# WHITE ROOM ESPORTS

Responsive Free Fire tournament platform built with React + Vite + Supabase. Includes public home, tournament directory, tournament details, survival standings / clash match cards, leaderboard, Hall of Champions, and a protected admin console.

## 1. Run locally

Requirements: Node.js 18+ and npm.

```bash
npm install
cp .env.example .env
npm run dev
```

Open the local URL printed by Vite. The site displays the empty-state copy until tournaments exist.

## 2. Connect persistent database and authentication

1. Create a Supabase project.
2. Open **SQL Editor**, paste and run `supabase/schema.sql`.
3. In Supabase **Project Settings → API**, copy Project URL and `anon` public key into `.env`:

```env
VITE_SUPABASE_URL=https://YOUR_PROJECT.supabase.co
VITE_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
```

4. Restart `npm run dev`. Public pages read the shared Supabase tables; updates made by an authorized admin appear after refresh.
5. In Supabase **Authentication → Users**, create an admin user. Copy its UUID, then run the final `insert into public.profiles ...` statement in `supabase/schema.sql` with the real UUID. Never expose a service-role key in frontend code.
6. Sign in at `/admin` with that email and password. Row Level Security enforces admin-only writes at the database; hiding the admin URL is not treated as security.

## 3. Admin capabilities

- Create, edit and delete tournaments.
- Set category (Sinh tồn / Tử chiến), mode (Solo / Team 2 / Team 4), schedule, status, cover URL and description.
- Add participant/team records.
- Upsert survival standings (matches, wins/Booyah, kills, total points).
- Publish top-three champions. The Hall of Champions and leaderboard read the same database records.

The `matches` table and admin match-entry form support clash brackets and match scores. Registration workflows and image uploads are not enabled in this initial production foundation; cover art currently uses an image URL.

## 4. Production deployment

Run `npm run build`; deploy the generated `dist/` directory to Vercel, Netlify, or another static host. Configure the same two Vite environment variables in the hosting dashboard. Add the deployed domain to Supabase Auth URL configuration. Keep RLS enabled. Do not put Supabase service-role credentials in the browser.

## Project structure

- `src/App.jsx` — routes, public pages and admin console
- `src/style.css` — responsive dark esports design
- `src/supabase.js` — Supabase client
- `supabase/schema.sql` — database tables, constraints and RLS policies
- `public-logo.png` — supplied White Room emblem
