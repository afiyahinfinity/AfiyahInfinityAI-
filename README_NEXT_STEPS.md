# Afiyah Reviewed Pack

## Deliverables
- `01_Audit/AFIYAH_PACK_REVIEW.md` — corrected full-pack audit covering all 27 source files.
- `02_Hardened_Baseline/` — supplemental examples for a server-side Noor boundary, runtime response validation and SQL hardening. These are review scaffolds, not drop-in migrations.
- `03_Original/` — untouched original pack for traceability.

## Immediate engineering priorities
1. Reject or ownership-check caller-supplied Noor session IDs.
2. Convert approval/execution into atomic database RPCs with row locking and idempotency.
3. Harden `SECURITY DEFINER` functions and EXECUTE grants.
4. Expand RLS/RPC/view tests before staging approval.
5. Redesign and restore-test the backup architecture.
