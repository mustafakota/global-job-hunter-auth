# Global Job Hunter — OAuth consent site

This repository contains the sign-in and consent UI used by the Global Job Hunter MCP server. It is a static site for GitHub Pages; Supabase Auth performs the OAuth authorization-code, PKCE, token, approval, and denial flows.

## Files

- `index.html` — project landing page.
- `oauth/consent/index.html` — actual Supabase OAuth authorization UI.
- `.github/workflows/pages.yml` — GitHub Pages deployment workflow.

## Publish with GitHub Pages (free for this public repository)

1. In GitHub, open **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to **GitHub Actions**.
3. Commit the files on the `main` branch. The `Deploy Global Job Hunter consent site` workflow will run automatically.
4. Wait for the workflow to finish successfully. The site URL should be:
   `https://mustafakota.github.io/global-job-hunter-auth/`
5. The consent page URL should be:
   `https://mustafakota.github.io/global-job-hunter-auth/oauth/consent/`

## Configure Supabase after the Pages site is live

In the Supabase project `Global Job Hunter` (`ngsjbvrdtqitwdwzlbcn`):

1. Open **Authentication → URL Configuration**.
2. Set **Site URL** to:
   `https://mustafakota.github.io/global-job-hunter-auth`
3. Add this exact URL under **Redirect URLs** (needed for the optional email sign-in link):
   `https://mustafakota.github.io/global-job-hunter-auth/oauth/consent`
4. Open **Authentication → OAuth Server**.
5. Keep **Enable OAuth Server** ON.
6. Set **Authorization Path** to `/oauth/consent`.
7. Keep **Allow Dynamic OAuth Apps** OFF until you're ready to let MCP clients self-register. The ChatGPT MCP connection will need a registered OAuth client or dynamic registration enabled, depending on the client flow.
8. Save the settings.

## Public configuration vs secrets

The browser app contains the Supabase project URL and its `sb_publishable_...` key. A publishable key is designed for browser use; database access must still be protected by RLS. **Never put a service-role key, JWT signing secret, OAuth client secret, or personal access token in this repository.**

## What the authorization screen tells the user

The screen displays the requesting OAuth client and the OAuth scopes it requested. It also describes Global Job Hunter's MCP tool capabilities: read the user's profile and preferences, search and save listings, and view/update saved-application tracker entries. It clearly states that this MCP server does not submit job applications or contact employers.

The screen uses Supabase's documented `getAuthorizationDetails`, `approveAuthorization`, and `denyAuthorization` APIs. It supports password sign-in and an optional email one-time sign-in link. The email sign-in link requires the Redirect URL listed above.

## Backend

The MCP endpoint is deployed separately as a Supabase Edge Function:

`https://ngsjbvrdtqitwdwzlbcn.supabase.co/functions/v1/global-job-hunter-mcp`

The endpoint validates a Supabase Bearer token with Supabase Auth and forwards that same token to PostgREST, so the existing `auth.uid() = user_id` RLS policies apply. It does not use a hardcoded candidate user ID or service-role key.
