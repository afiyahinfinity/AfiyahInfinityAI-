# Validation Report

**Package:** Afiyah AI Services v1.0.0  
**Validated:** 5 July 2026

## Passed

- Required-file and package-structure validation.
- JSON parsing for package, token and schema files.
- JSON Schema draft 2020-12 structural validation.
- OpenAPI YAML parsing.
- High-signal secret scan.
- Python script compilation.
- TypeScript/TSX syntax transpilation across 22 files.
- Seven executable unit tests covering crisis routing, sensitive-data blocking, financial-advice boundary, normal routing, request validation, strict action shaping and unknown-action rejection.
- ZIP integrity test.

## Database validation boundary

Migration `013_ai_services_control_plane.sql` was statically checked for the required security controls, including RLS, restricted grants, `SECURITY DEFINER` functions and protected `search_path`. It has **not** been executed against the user’s real Supabase staging database in this chat. Database migration, RLS and concurrency behaviour must therefore be verified in staging before release.

## Deployment boundary

The package is a repository overlay. It depends on the existing application, migrations 001–012, Supabase configuration and production secret stores. No production deployment was performed.
