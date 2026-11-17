# Safe Tracker mobile — Milestone 1

Production mobile foundation for Safe Tracker. The approved published HTML prototype remains a separate design reference and is not modified or bundled here.

## Scope

- Expo/React Native/TypeScript for iOS and Android
- verified Australian mobile-number OTP through Supabase Auth
- stable Supabase `auth.users.id` UUID identity (phone number is not the primary key)
- user profile protected by Postgres row-level security
- four-digit Safe PIN creation, verification, change and OTP-based recovery
- server-only Argon2id PIN hash with an environment-held pepper and persistent rate limits
- encrypted OS storage for persisted authentication sessions
- separate development and production configuration/build channels
- no location package, location permission, safety session, or live tracking

## Repository structure

```text
src/                  mobile UI, navigation, auth state and API clients
supabase/migrations/  database schema, RLS and protected server tables
supabase/functions/   authenticated Safe PIN service
tests/                automated unit tests
docs/                 deployment, security and verification records
```

## Local setup

1. Create a development Supabase project and configure an SMS provider.
2. Copy `.env.example` to `.env.development.local` and insert the development URL and anon key.
3. Set the Edge Function secret `PIN_PEPPER` to at least 32 random bytes. Never place it in an Expo environment variable.
4. Link the Supabase CLI, apply migrations, and deploy `safe-pin`.
5. Run `npm start` for Metro or create an EAS development build.

The anon key is intentionally public and is safe only in combination with correctly deployed RLS. The service-role key and PIN pepper must remain server-side.

## Verification commands

```sh
npm run typecheck
npm test
npm run audit:security
npm run doctor
npx expo prebuild --clean --no-install
```

Real OTP, persistence, database isolation, iOS/Android builds, and physical-device tests require configured external projects and devices. They must not be reported as verified until executed against those resources.
