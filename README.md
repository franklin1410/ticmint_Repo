# Ticmint Freshdesk Knowledge Base Editor

Private standalone editor hosted on Render. It retrieves Freshdesk Solution articles, uses the OpenAI API to prepare revised HTML, shows a preview, and only updates Freshdesk after the user explicitly approves.

## Important
- Never commit `.env` or API keys.
- Configure `APP_PASSWORD`, `FRESHDESK_DOMAIN`, `FRESHDESK_API_KEY`, and `OPENAI_API_KEY` as Render secret environment variables.
- The Freshdesk API key belongs only on the server.
- OpenAI API billing is separate from ChatGPT Go.
- The starter is intended for a controlled pilot. Before wider production use, add persistent audit logging, SSO/MFA, CSRF/session hardening, and an approval identity trail.

## Local run
1. Install Node.js 22+.
2. Run `npm install`.
3. Set environment variables (see below).
4. Run `npm start`, then open http://localhost:10000.

Example `.env` (do not commit):
APP_PASSWORD=choose-a-long-random-password
FRESHDESK_DOMAIN=your-account.freshdesk.com\nFRESHDESK_API_KEY=your-freshdesk-api-key
OPENAI_API_KEY=your-openai-api-key
OPENAI_MODEL=gpt-6

## Render deployment
1. Push this folder to a private GitHub repository.
2. In Render choose New > Web Service and connect the repository.
3. Build command: `npm install`
4. Start command: `npm start`
5. Add the four environment variables above in Render > Environment. Keep the keys secret.
6. Deploy and open the `onrender.com` URL.

## Pilot test
1. Sign in with APP_PASSWORD.
2. Choose a Knowledge Base folder and article.
3. Load it and request a small wording-only change.
4. Inspect original and revised previews carefully.
5. Approve only if correct. The server re-fetches the article and refuses to save if it changed since preview creation.

## API behaviour
Freshdesk API calls use `https://$FRESHDESK_DOMAIN/api/v2`. Freshdesk documents that API calls should use the account’s canonical Freshdesk domain, not a custom CNAME. Find this canonical hostname in your Freshdesk account settings or ask Freshdesk support. Article updates use PUT `/solutions/articles/{id}` and preserve the current title and publication status. The app does not delete or create articles.
