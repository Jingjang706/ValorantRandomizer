# VALORANT Challenge Roulette

A mobile-friendly, fan-made challenge randomizer. Players can roll agents, maps, primary/secondary weapons, optional skins and challenge rules; build a squad; lock results; set budget/role/gun filters; share a challenge link; manage an owned-item collection; and revisit up to 30 recent rolls.

## Run it locally

The app works without an account. Serve this folder with any static web server (for example, `python3 -m http.server 8000`) and open `http://localhost:8000`. `file://` is not recommended because browser storage and OAuth redirects are restricted there.

## Optional account sync (Google/Discord)

Account sync stores a player’s manually selected collection and recent challenge history. It does **not** connect to Riot, read inventory, or request Riot credentials.

1. Create a Supabase project.
2. In its SQL Editor, run [`supabase/schema.sql`](supabase/schema.sql). Row Level Security restricts each row to its owner.
3. In Supabase Authentication, enable Google and/or Discord. Create OAuth credentials in the provider’s developer console, paste the client ID/secret into Supabase (not this app), and add Supabase’s displayed callback URL (`https://<project-ref>.supabase.co/auth/v1/callback`) to the provider’s allowed redirect/callback URLs.
4. In Supabase **Authentication → URL Configuration**, set the deployed site URL as the Site URL. Add the exact local test URL and production URL to the allowed Redirect URLs.
5. Copy the Supabase Project URL and the browser-safe anon/publishable key into `config.js`:

   ```js
   window.VCR_CONFIG = {
     supabaseUrl: "https://YOUR-PROJECT.supabase.co",
     supabaseAnonKey: "YOUR-PUBLIC-ANON-OR-PUBLISHABLE-KEY"
   };
   ```

6. Deploy the folder to a static host such as Netlify, Vercel, or Cloudflare Pages. Add the final deployed URL to Supabase’s allowed Redirect URLs.

Only the public anon/publishable key belongs in `config.js`; never put a Supabase service-role key there. The SQL policies enforce per-user access. If OAuth is not configured, the randomizer and local saving still work.

## Notes

- Game content and images are fetched from the community VALORANT API at `valorant-api.com`, with browser caching and offline fallback data.
- The app is fan-made and not affiliated with or endorsed by Riot Games. VALORANT and its related marks/art are owned by Riot Games.
- No user secrets are embedded. Google/Discord sign-in requires the site owner’s own OAuth provider setup.
