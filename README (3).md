# Afiyah AI Services

**Version:** 1.0.0  
**Release date:** 5 July 2026  
**Target repository:** `afiyahinfinity/mtbdiaries1-afiyah-infinity`

This package contains the AI service architecture, model orchestration, API contracts, security controls, Noor request middleware, Supabase Edge Functions, governance migration, tests, and deployment guidance for Afiyah Infinity AI.

## Operating boundary

Afiyah AI Services is designed for:

- financial-wellness education and planning support;
- deterministic calculators and progress tracking;
- non-diagnostic wellness coaching;
- Shariah-aligned content routing from approved knowledge sources;
- AI-assisted review queues where humans make final decisions.

It is **not** designed to:

- provide regulated investment, credit, legal, medical, or tax advice;
- custody funds or execute financial transactions;
- issue autonomous Shariah rulings;
- approve identity, KYC, partner, payment, or compliance decisions without a human;
- replace emergency or clinical services.

## Architecture at a glance

```text
Web / mobile client
    |
    v
Next.js server boundary (`nextjs/app/api/noor/route.ts`)
    |
    v
Noor Guard Edge Function
  - authentication and origin checks
  - body-size and schema validation
  - sensitive-data and crisis screening
  - financial-advice boundary
  - idempotency reservation
    |
    v
Hardened Noor Agent
  - owned session only
  - approved prompt version
  - vendor-neutral LLM gateway
  - strict structured-output validation
  - proposed actions only
    |
    v
Atomic action decision/execution RPC
  - user approval required
  - row lock + idempotency
  - immutable execution evidence
```

## Package map

| Path | Purpose |
|---|---|
| `docs/` | Architecture, governance, security, service catalogue, deployment and incident response |
| `openapi/` | OpenAPI 3.1 contract for public AI-service endpoints |
| `schemas/` | JSON Schemas for requests, responses and proposed actions |
| `src/` | Provider-neutral policy, validation and orchestration utilities |
| `supabase/functions/` | Noor Guard, health check and hardened Stage 1 AI functions |
| `supabase/migrations/` | AI-services control-plane migration |
| `nextjs/` | Optional server-only Noor API boundary for the existing Next.js app |
| `tests/` | Unit, contract and security test specifications |
| `scripts/` | Package validation and high-signal secret scanning |

## Integration order

1. Merge this folder into the existing application repository on a feature branch.
2. Confirm migrations `001` through `012` already exist and pass in staging.
3. Apply migration `013_ai_services_control_plane.sql` to staging.
4. Configure secrets using `.env.example` and `supabase/.env.functions.example`.
5. Deploy the Edge Functions listed in `docs/DEPLOYMENT.md`.
6. Point the frontend Noor client to `/api/noor`, not directly to a model vendor.
7. Run all security, RLS, idempotency and rollback checks.
8. Release to production only after the human-governance and legal gates are signed off.

## Non-negotiable invariants

1. AI assists; people decide consequential outcomes.
2. Noor can propose actions but cannot execute them without explicit user approval.
3. Crisis handling is deterministic and bypasses generative AI.
4. Deterministic calculations stay outside the LLM.
5. No service-role key or model key is exposed to the browser.
6. Provider names and model IDs are configuration, not application dependencies.
7. Every AI request has a prompt version, request hash, idempotency key and audit record.
8. Raw cycle data and unnecessary identity data never enter general-purpose prompts.
9. AI failure must degrade safely without blocking the rest of Afiyah.
10. The production kill switch must work without a code deployment.

## Important dependencies

This is a **repository overlay**, not a complete standalone product. It requires:

- the existing Next.js application;
- Supabase Auth, Postgres, RLS and Storage;
- migrations `001`–`012`, especially the Noor atomic-security migration;
- the application’s approved legal text and verified support resources;
- a configured model provider and transactional email service.

No live credentials are included.
