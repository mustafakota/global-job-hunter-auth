# Security notes

- This repository is public because GitHub Pages on GitHub Free requires a public repository for this deployment configuration.
- Only the Supabase project URL and publishable browser key are present in the static consent page. These are not privileged credentials.
- OAuth consent is handled by Supabase Auth. This static page does not implement its own token exchange or store OAuth client secrets.
- The MCP endpoint must be configured to validate every user access token and use the same token for database queries so RLS is enforced.
- Current tool behavior is limited to candidate profile access, job feed search/storage, listing saved jobs, and saving/listing tracker records. It does not submit applications, send messages, or make purchases.
- Review OAuth clients and grants periodically. Keep dynamic client registration disabled unless it is required by the MCP client and you are comfortable with client self-registration.
