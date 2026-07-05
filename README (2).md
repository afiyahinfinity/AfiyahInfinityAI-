# Afiyah Backend

Production-oriented backend/API scaffold for Afiyah Infinity AI.

**Release:** 1.0.0  
**Date:** 5 July 2026  
**Target repository:** `afiyahinfinity/mtbdiaries1-afiyah-infinity`  
**Recommended path:** `apps/backend/`

## Purpose

- Provide a secure Backend-for-Frontend (BFF) for web and Flutter clients.
- Validate Supabase access tokens and preserve PostgreSQL Row Level Security.
- Orchestrate user/profile, wellness, educational-insight and Noor requests.
- Keep service-role access isolated to narrowly scoped internal workflows.
- Provide idempotency, audit evidence, webhook deduplication and operational health checks.

## Architecture

```text
Web / Flutter
    |
    | HTTPS + Supabase bearer token + request ID
    v
Afiyah Backend (Fastify / TypeScript)
    |-- Auth middleware: Supabase Auth token verification
    |-- User-scoped Supabase client: RLS preserved
    |-- Noor proxy: forwards to Afiyah AI Services
    |-- Idempotency + audit control plane
    |-- Health, readiness and webhook endpoints
    v
Supabase PostgreSQL/Auth/Storage + AI Services + Resend
```

## Security invariants

1. Browser/mobile clients never receive a Supabase service-role key.
2. User data access uses a user-scoped Supabase client so RLS remains authoritative.
3. Service-role access is restricted to server-only repositories and audited workflows.
4. Consequential Noor actions remain pending until the secured atomic approval/execution flow completes.
5. Financial content is educational and non-advisory; the backend does not execute trades, custody funds or select investments.
6. Unsafe retries are controlled through idempotency keys.
7. Logs redact authorization headers, cookies, tokens, document payloads and high-risk PII fields.
8. Webhooks are authenticated, deduplicated and persisted before side effects.

## Quick start

```bash
cp .env.example .env.local
npm ci
npm run dev
```

Required supporting systems:

- Supabase staging project with migrations 001–013 applied.
- Migration `014_backend_control_plane.sql` applied to staging.
- Afiyah AI Services reachable from the backend network.
- Staging-only test users and synthetic data.

## Main endpoints

- `GET /health/live`
- `GET /health/ready`
- `GET /v1/session`
- `GET /v1/profile`
- `PATCH /v1/profile`
- `GET /v1/wellness/summary`
- `GET /v1/insights`
- `POST /v1/noor/messages`
- `POST /v1/noor/actions/:proposalId/decision`
- `POST /v1/webhooks/resend`

See `openapi/afiyah-backend.openapi.yaml` and `docs/API_CONTRACTS.md`.

## Deployment status

This is a repository-ready backend module, not proof of production deployment. Run the release gate in `docs/RELEASE_CHECKLIST.md` against the actual staging Supabase and AI-services environments before production.
