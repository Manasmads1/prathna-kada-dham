# Kada Dham – Sheetla Mata

A production-oriented, responsive temple website built from `utsav_prds.docx`. The site uses genuine clean URLs, a React/Vite frontend, and an Express backend implementing the documented API contracts.

## Run locally

```bash
pnpm install
pnpm build
NODE_ENV=production pnpm start
```

Open `http://localhost:3000`.

For development, use `pnpm dev` (Vite). The production-style API server is the recommended local verification path because it serves the SPA and API together.

## Environment variables

| Variable | Default | Purpose |
|---|---:|---|
| `PORT` | `3000` | HTTP port |
| `JWT_SECRET` | `dev-only-change-me` | HMAC signing secret; set a long random value in production |
| `DATA_FILE` | `data/kada-dham.json` | JSON persistence file used by this standalone build |

The PRD specifies MongoDB as the current production architecture. This independent build uses a file-backed store so it runs without inaccessible external credentials. Replace the small `load/persist` adapter in `server/index.ts` with MongoDB collections (`users`, `otps`, `bookings`) before production launch, and add unique/indexed fields as described in the PRD.

## Implemented routes

- `/` — Hindi-first homepage and featured catalog
- `/pujas` — catalog with exact PRD categories and client-side name/description search
- `/pujas/:id` — puja detail and authenticated booking modal
- `/temple` — temple information, timings, location, contact links, gallery
- `/videos` — YouTube search embed using the documented URL format
- `/login` — Gmail/Yahoo OTP flow; redirects to `/dashboard`
- `/dashboard` — authenticated booking history and empty state
- `/orders/:id` — owner-only order detail, UPI/PhonePe links, manual confirmation
- `/profile` — authenticated display-name update
- `/about`, `/contact` — mapped to the temple/contact experience
- `/manus-routes.json` — route manifest for the runtime

## API implemented

- `POST /api/auth/send-otp`
- `POST /api/auth/verify-otp`
- `GET /api/auth/me`
- `PUT /api/auth/update-profile?name=...`
- `GET /api/pujas`
- `GET /api/pujas/:id`
- `POST /api/bookings`
- `GET /api/bookings`
- `GET /api/bookings/:id`
- `POST /api/payments/initiate`
- `POST /api/payments/confirm/:id`
- `GET /api/temple-info`

Backend validation covers supported email domains, lowercase normalization, six-digit OTPs, OTP expiry/single use, future dates, valid time values, required names, contact-email ownership, active catalog IDs, server-derived puja name/price, booking ownership, and payment ownership.

## Validation evidence

- `pnpm install --frozen-lockfile` completed.
- `pnpm check` completed with no TypeScript diagnostics.
- `pnpm build` completed with Vite and esbuild.
- Direct refresh checks returned `200 text/html` for `/`, `/pujas`, `/temple`, `/videos`, `/login`, `/dashboard`, `/orders`, `/about`, `/contact`, `/pujas/puja_7`, and `/orders/abc`.
- `/manus-routes.json` returned `200 application/json`.
- API smoke flow succeeded: send OTP → verify OTP → authenticated `/me` → booking creation → payment initiation → manual confirmation.
- The frontend contains responsive breakpoints at 900px and 560px; layout grids collapse without relying on horizontal overflow hiding.

## Important remaining production integrations

1. **OTP delivery:** the PRD currently exposes `mock_otp` for testing. This build keeps that documented behavior and labels it in the UI. Replace it with a real email/SMS provider, throttle requests, and limit verification attempts before launch.
2. **Payment verification:** the PRD’s current flow uses UPI/PhonePe deep links and manual confirmation. This build implements that exact behavior and does not claim gateway verification. Add a verified provider callback/webhook and transaction/receipt fields before treating payment as final.
3. **MongoDB:** swap the local file adapter for the documented MongoDB persistence layer and indexes.
4. **Original media:** all temple imagery is clearly labeled as open-source placeholder photography. Replace the centralized image URLs in `client/src/kada-app.jsx` and backend `pujas`/`temple` data when original temple photos and logos are supplied.
5. **Official claims/content:** no undocumented prices, timings, statistics, testimonials, or religious claims were added.

## Contact links

The documented priest contact is wired as `tel:+919651409610` and `https://wa.me/919651409610`. The site does not claim access to phone logs or WhatsApp message status.
