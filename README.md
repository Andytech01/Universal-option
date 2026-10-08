# Universal-option
# Forest Trading AI

Production-oriented Next.js + TypeScript financial-analysis platform foundation. The repository is structured for real market-data, AI, PostgreSQL, authentication, payments, crypto providers, and admin services without exposing secrets to the browser.

## Run locally

1. Copy `.env.example` to `.env`.
2. Set `DATABASE_URL` to a PostgreSQL database.
3. Add provider credentials when you are ready to enable live services.
4. Install dependencies: `npm ci`.
5. Generate Prisma Client: `npx prisma generate`.
6. Apply schema migrations: `npx prisma migrate dev --name init` during development.
7. Start: `npm run dev`.

The application will run at `http://localhost:3000`.

## Production: Vercel + managed PostgreSQL

1. Push this repository to GitHub/GitLab.
2. Import the repository into Vercel.
3. Add all production environment variables from `.env.example` in Vercel Project Settings.
4. Use a managed PostgreSQL provider such as Supabase, Neon, or another PostgreSQL-compatible service.
5. Run `npx prisma migrate deploy` as part of your deployment pipeline.
6. Set `NEXT_PUBLIC_APP_URL` to the production HTTPS domain.
7. Configure the legitimate market-data, news, AI, crypto and payment providers you have licensed.
8. Verify `/api/health` after deployment.

## Production: Docker

Create `.env` from `.env.example`, then:

```bash
docker compose up --build -d
```

The web container exposes port 3000 and PostgreSQL is persisted in a named volume. Put a reverse proxy/load balancer with HTTPS in front of the application for public production traffic.

## Live data behavior

The dashboard does not fabricate live market values. If `MARKET_DATA_API_KEY` is absent or the provider is unavailable, the UI reports that market data is not connected. The current provider adapter uses a server-side Finnhub-compatible quote endpoint; replace or extend `lib/market-data.ts` for your licensed provider.

## Important production work before launch

- Configure real authentication and session management.
- Add a production AI provider and validate structured AI output before storing/displaying it.
- Add licensed market/fundamental/news data providers.
- Add payment-provider webhook verification and idempotency.
- Configure Redis/background jobs for alerts and rate limiting.
- Add migrations, seed data, monitoring, backups and secrets management.
- Complete a security and compliance review for the jurisdiction where the service operates.

Never commit `.env` files or provider secrets. Never store private keys or seed phrases in this application.
