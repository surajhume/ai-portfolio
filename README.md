# Suraj Hume Portfolio — Vercel deployment

## Project files
- `index.html` — portfolio frontend. Chat requests send only the visitor's question.
- `api/chat.js` — Vercel serverless endpoint. The portfolio prompt and Gemini API call are server-side.
- `vercel.json` — configures a 30-second maximum function duration.

## Required Vercel environment variables

In Vercel, open **Project → Settings → Environment Variables** and add:

1. `GEMINI_API_KEY` — your Gemini API key.
2. `UPSTASH_REDIS_REST_URL` — the REST URL from an Upstash Redis database.
3. `UPSTASH_REDIS_REST_TOKEN` — the REST token from the same Upstash database.

Add them to Production, and Preview if you want preview deployments to work. Keep all values secret. Redeploy after adding or changing environment variables.

The endpoint allows **10 requests per IP per 60-second window**. It uses Upstash Redis so counters are shared across Vercel serverless instances. If Upstash is not configured in a Vercel deployment, the chat endpoint fails closed rather than silently running without a reliable rate limit.

## Deploy
1. Upload these files to a GitHub repository (do not commit any API keys or tokens).
2. In Vercel, select **Add New → Project** and import the repository.
3. Add the three environment variables listed above.
4. Deploy or redeploy the project.
5. Test both ordinary portfolio questions and prompt-injection attempts.

## Local development
Run with the Vercel CLI (`vercel dev`). Set the environment variables in your local environment or in a local `.env` file that is excluded from Git. Never commit `.env`.

## Security notes
- `GEMINI_API_KEY` is read only by the server from `process.env.GEMINI_API_KEY`.
- `UPSTASH_REDIS_REST_URL` and `UPSTASH_REDIS_REST_TOKEN` must also remain private.
- The server ignores any `systemInstruction` sent by the browser.
- Prompt instructions reduce prompt-injection risk but cannot guarantee that a model will never be tricked. Keep secrets out of model context and responses.
- Rate limiting is not a substitute for monitoring usage, configuring provider quotas, or bot protection.
