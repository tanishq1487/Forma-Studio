# Forma Studio

Forma Studio is an independent, local-first visual design editor. 

## What works

- Start from four original, editable template layouts or a blank project.
- Create multiple pages and add, move, edit, and delete text, shapes, and uploaded images.
- Change page backgrounds, font family, size, colors, and element positions.
- Keep project notes with their author and page.
- Autosave projects in this browser using local storage.
- Export the current page to PNG and download/import a JSON project backup.
- Optional Supabase magic-link sign-in, cloud project sync, private image storage, email invitations, viewer/editor access, and live project updates.
- Responsive home and editor layouts.

The editor uses an SVG canvas and stores its design model as JSON. It is deliberately a compact foundation: it does not include generative AI, a commercial asset marketplace, video editing, print production, or an unlimited cloud service.

## Requirements

- Node.js 20.19+ (or 22.12+)
- npm
- A modern browser
- Optional: a Supabase project for sign-in and collaboration. Supabase has plan and usage limits; this app cannot guarantee that a provider will remain free or available.

## Run locally

```powershell
npm install
npm run dev
```

Open the local URL printed by Vite (normally `http://localhost:5173`). You can create, edit, save, and export designs without setting up Supabase. Local projects are stored only in this browser profile; use **Download project backup** to move a project to another device.

## Optional Supabase setup

1. Create a Supabase project and enable email authentication. Configure the site's URL and redirect URL to include your local Vite origin and your eventual deployed origin.
2. In the Supabase SQL editor, run [`supabase/schema.sql`](./supabase/schema.sql). It creates the project, membership, invitation, and private asset-storage tables/policies and enables project updates for Realtime.
3. Copy `.env.example` to `.env.local` and fill in the project URL and **anon/public** client key:

   ```dotenv
   VITE_SUPABASE_URL=https://YOUR_PROJECT.supabase.co
   VITE_SUPABASE_ANON_KEY=YOUR_SUPABASE_PUBLIC_ANON_KEY
   ```

4. Restart `npm run dev`, then use **Sign in**. Supabase emails a one-time sign-in link.
5. Open a project and choose **Sync to cloud**. Synced changes autosave to the project row. To invite someone, sync the project, choose **Share**, and enter their email. They must sign into this same app with that email and accept the invitation from **Shared with me**.
6. To deploy, host the built `dist` folder on a static host, add the same environment variables to the host, and add its URL to Supabase's allowed redirect URLs.

Never put a Supabase service-role key in this frontend. This app only uses the public anon key; authorization is enforced by the row-level security policies in `schema.sql`.

## Commands

```powershell
npm run dev       # local development server
npm test          # focused model tests
npm run build     # TypeScript checks and production bundle
```

## Notes and limits

- The optional Google Fonts import requires internet access; the app falls back to system fonts.
- Local image imports are limited to 2.5 MB. They remain in the browser's local storage until the project is synced; synced images are uploaded to the private Supabase Storage bucket.
- Cloud sync and Realtime require a configured Supabase project and correct auth redirect settings. Provider quotas, outbound email configuration, and plan terms are controlled by Supabase.
- Concurrent users can overwrite one another's latest project JSON. This starter is not a conflict-free collaborative editor; use it for lightweight shared editing, not simultaneous high-stakes production work.
- Public Canva pages informed only the broad product categories and generic workflows. This implementation uses original names, content, templates, styling, and code. Authenticated and paid Canva workflows were not inspected or replicated.
