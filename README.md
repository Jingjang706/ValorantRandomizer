# VALORANT Challenge Roulette

A mobile-friendly, installable fan-made challenge randomizer. Roll agents, maps, primary/secondary weapons, optional skins and challenge rules; build a squad of up to five players; lock results; set budget/role/gun filters; share validated challenge links; manage an owned-item collection; and revisit up to 30 recent rolls.

## Run locally

Serve this folder with any static web server (for example, `python3 -m http.server 8000`) and open `http://localhost:8000`. Do not use `file://`: browser storage, service workers and OAuth redirects require a local or hosted HTTP(S) origin.

## Features

- Live agents, maps, weapons and skin art from the community VALORANT API, with a cached-data and offline fallback.
- Squad rolls for one to five players, role and weapon-class filters, buy-budget selection, and per-result locks.
- Owned-agent and owned-skin collection with owned-only filtering.
- Shareable challenge URLs with validation of the shared payload before it is applied.
- Local challenge history (up to 30 rolls), copy-to-clipboard and restore.
- Responsive mobile layout, keyboard accessibility and reduced-motion support.
- Installable PWA shell with a service worker for repeat visits and offline loading.
- Optional Google/Discord account sync through Supabase. The app does not connect to Riot, read a Riot account inventory, or request Riot credentials.

## Deploy

This is a static site: publish the repository root as the site directory. No build command or server runtime is required. It can be hosted on GitHub Pages, Cloudflare Pages, Netlify or Vercel. Use HTTPS in production; service workers and installability require a secure context (localhost is allowed for development).

For Vercel, the included `vercel.json` serves the root as a static site and applies safe MIME/cache headers. For other static hosts, publish the root directory as-is.

## Optional account sync (Google/Discord)

Local randomization, collection and history work without an account. Account sync stores a player's manually selected collection and recent challenge history. It does **not** connect to Riot, read inventory, or request Riot credentials.

1. Create a Supabase project.
2. In its SQL Editor, run [`schema.sql`](schema.sql) from the repository root. Row Level Security restricts each row to its owner.
3. In Supabase Authentication, enable Google and/or Discord. Create OAuth credentials in the provider's developer console, paste the client ID/secret into Supabase (not this app), and add Supabase's displayed callback URL (`https://<project-ref>.supabase.co/auth/v1/callback`) to the provider's allowed redirect/callback URLs.
4. In Supabase **Authentication → URL Configuration**, set the deployed site URL as the Site URL. Add the exact local test URL and production URL to the allowed Redirect URLs.
5. Copy the Supabase Project URL and browser-safe anon/publishable key into `config.js`:

   ```js
   window.VCR_CONFIG = {
     supabaseUrl: "https://YOUR-PROJECT.supabase.co",
     supabaseAnonKey: "YOUR-PUBLIC-ANON-OR-PUBLISHABLE-KEY"
   };
   ```

6. Deploy the site and add its final URL to Supabase's allowed Redirect URLs.

Only the public anon/publishable key belongs in `config.js`; never put a Supabase service-role key there. The SQL policies enforce per-user access. The checked-in `config.js` intentionally has blank placeholders, so Google/Discord sign-in remains unavailable until the owner supplies their own Supabase project and OAuth provider credentials. Do not commit private keys.

## Validation

The GitHub Actions workflow checks the inline application JavaScript syntax and verifies the static deployment files are present on each push and pull request.

## Notes

- Game content and images are fetched from `valorant-api.com`; the app stores a local cache and includes a text-only fallback dataset.
- Fan-made project; not affiliated with or endorsed by Riot Games. VALORANT and its related marks/art are owned by Riot Games.
