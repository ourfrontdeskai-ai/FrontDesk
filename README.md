# FrontDesk

A Next.js (App Router, TypeScript) web app with a PostgreSQL database via Prisma.

## Getting started

```bash
npm install
cp .env.example .env   # then set DATABASE_URL to your Postgres instance
npm run db:push        # sync the Prisma schema to the database
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) to see the result.

## Scripts

- `npm run dev` — start the dev server (Turbopack)
- `npm run build` — production build
- `npm run start` — run the production build
- `npm run lint` — lint with ESLint
- `npm run db:push` — push the Prisma schema to the database without a migration
- `npm run db:migrate` — create and apply a Prisma migration
- `npm run db:studio` — open Prisma Studio
