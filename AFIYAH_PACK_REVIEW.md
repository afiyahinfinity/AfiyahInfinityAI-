# Afiyah Complete Pack — Independent Technical Review
**Review date:** 4 July 2026

## Executive verdict
The archive contains **27 files / approximately 3,261 lines**: six SQL migrations, seven Supabase Edge Functions including the shared LLM gateway, six Next.js pages/components, middleware, an RLS test file, an audit, and a standalone demo/schema.

This is a credible **merge pack and prototype baseline**, but it is not independently production-ready. Several controls are directionally strong—vendor-neutral AI, authenticated Edge Functions, approval-gated agent actions, AAL2 admin routing, RLS coverage, payment idempotency, backup export, and governance tables—yet important authorization, data-integrity, privacy, operational and testing gaps remain.

## What is genuinely included
- Admin security/recovery migration and AAL2 middleware.
- Timezone-aware check-in/streak functions and reminder view.
- KYC hardening migration.
- Noor session/message/action schema, action registry and quota function.
- Subscription/payment schema with a riba-free database trigger.
- Shariah rulings, risk register and k-anonymous partner insights view.
- Vendor-neutral text, vision and embedding gateway.
- Identity, document, Barakah, coaching, Noor proposal and deterministic execution functions.
- Password-reset, KYC, membership, Noor and partner-governance pages.
- Nightly JSONL export and a first RLS test suite.

## Critical findings

### P0 — Authorization vulnerabilities
1. **Noor session ownership is not validated.** `noor-agent` accepts a caller-supplied `session_id`, then writes and reads it using the service-role client. A sister could supply another session UUID and contaminate/read context unless the function first proves that session belongs to `user.id`.
2. **Security-definer functions accept arbitrary user IDs.** Functions such as `submit_checkin(p_user)`, `current_streak(p_user)`, `local_date_for(p_user)` and `agent_quota_ok(p_sister)` do not set a safe `search_path` or consistently assert `p_user = auth.uid()`/admin authorization. Direct EXECUTE grants must be removed or the functions hardened.
3. **CORS is wildcarded** on Noor, coach and executor functions. Authentication limits impact, but production should restrict origins and explicitly handle allowed methods/headers.
4. **Middleware uses `getSession()`** for the admin gate. Use a server-validated user/claims path rather than trusting session data alone, and verify the Supabase SSR package/version because `auth-helpers-nextjs` is legacy in newer implementations.

### P0 — Agent-action integrity
- The database allows a sister to change any proposed action to approved/declined, but approval time and immutable payload/version are not enforced by a database transition function.
- Execution reads an approved row and later updates it; without row locking or an atomic claim, duplicate concurrent calls can execute the same action twice.
- Model action payloads are filtered by action type but not validated against strict per-action schemas before storage. Validation occurs partially during execution, leaving malformed proposals and inconsistent UI behaviour.
- The action registry seed embeds board-resolution references, but the pack contains no signed resolutions or migration gate proving those references are valid.

### P0 — Backup/restore limitations
- The nightly export is an application-level JSONL dump, not a full PostgreSQL backup. It excludes schema, functions, policies, triggers, extensions, storage objects, auth data and several new tables such as `agent_messages`, `payment_events`, `shariah_rulings`, `risk_register` and reference/configuration tables.
- PII and potentially sensitive health/wellness data are exported unencrypted at the application layer into storage; bucket policy, encryption/key management and restore tooling are not included.
- Retention cleanup lists only the first 100 folders and one page of files per folder.

## High-priority findings

### P1 — Database security and integrity
- Several `SECURITY DEFINER` functions lack `SET search_path`, increasing object-shadowing risk.
- Function EXECUTE privileges are not explicitly revoked from `PUBLIC` and re-granted narrowly.
- Partner-insights access is described in comments, but the view has no explicit grant/revoke shown in the migration. `security_invoker = false` makes ownership and grants especially important.
- Admin management policies use `USING` without corresponding `WITH CHECK` in places; insert/update behaviour should be tested explicitly.
- The payment trigger enforces a caller-supplied Boolean label, not the economic structure of a payment rail. “Riba-free” requires provider/product governance, not only `riba_free_rail = true`.
- Payment events are called append-only, but the migration needs explicit mutation blocking and grants.
- Reference rates allow unrestricted values unless access is tightly controlled; add positive-value constraints, source, effective timestamp and maker-checker approval.
- Risk-register seed uses `ON CONFLICT DO NOTHING` without a uniqueness constraint that would prevent duplicate seed rows on repeated migrations.

### P1 — AI and safety
- The LLM gateway has no abort timeout, circuit breaker or request-size boundary at the shared layer.
- JSON parsing is syntactic only; outputs are cast to TypeScript types without runtime schemas.
- Vision sends full base64 identity documents to an external model provider. The pack must document lawful basis, processor/subprocessor terms, region, retention, no-training terms, minimisation and deletion evidence.
- Prompt-injection fencing is useful but not a complete defence; tool authorization must remain independent of model output.
- Crisis detection appears keyword/template based. The README itself says helplines must be verified; no evidence of clinical review, locale validation or human-queue integration is included.

### P1 — Product truthfulness and regulatory presentation
- KYC, screening, payments, scholar escalation and partner dashboards must remain labelled **prototype/not live** until contracted integrations, operational ownership, acceptance testing and applicable approvals are complete.
- “DIFC Innovation Licence” does not itself authorise regulated financial, payment, investment, KYC-provider or medical activities. Product copy needs a line-by-line legal/compliance review.
- Pricing, gateways, gold rates, Shariah references, emergency contacts and regulatory citations are configuration/content items requiring verified owners and review dates.

### P1 — Test coverage
The RLS suite is a useful start but is not a full release gate. Add tests for:
- every table, view and RPC;
- insert/update/delete `WITH CHECK` behaviour;
- security-definer authorization;
- cross-session Noor attacks;
- duplicate/concurrent action execution;
- admin roles and AAL2 states;
- partner view grants and inference resistance;
- payment-event immutability and webhook replay;
- backup completeness and restoration.

## Medium-priority findings
- The standalone demo directly calls Anthropic and hardcodes a model; it conflicts with the merge pack’s vendor-neutral architecture and should be clearly isolated from deployable code.
- The shared gateway contains a long refactor guide inside the source file; move this to documentation.
- Pages use broad `any` types and appear to rely on surrounding project aliases/components not included here, so compile success cannot be established from this pack alone.
- No package manifest, lockfile, tsconfig, Supabase config, CI workflow, lint config or complete base migrations 001–005 are included; therefore this ZIP cannot be built or migrated independently.

## Recommended implementation order
1. Fix Noor session ownership and make action approval/execution atomic and idempotent.
2. Harden every security-definer function: safe search path, caller authorization, narrow EXECUTE grants.
3. Complete RLS/RPC/view tests across the actual staging schema.
4. Replace the backup export with a documented backup architecture and tested restore runbook; retain JSONL only as a supplementary export.
5. Add runtime schemas, timeouts, origin restrictions, rate limits and structured error telemetry to Edge Functions.
6. Complete privacy/DPIA, AML/CFT, consumer, payments, cyber, AI-governance and Shariah reviews before public claims.
7. Reconcile this merge pack into the full repository and prove build, migrations, tests and rollback in staging.

## Readiness classification
- Internal product concept/demo: **Yes, with prototype labels**.
- Investor/partner demo: **Yes, with precise disclaimers**.
- Closed alpha using synthetic data: **After P0 fixes**.
- Real-user KYC/payment production: **No**.
