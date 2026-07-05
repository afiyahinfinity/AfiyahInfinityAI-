# Afiyah Infinity AI — Master Pack v5

**Release:** 5 July 2026  
**Purpose:** Consolidated engineering, security, deployment, design-system, AI-services, Flutter mobile and backend handoff.

## Start here

1. Read `MASTER_CONTENTS.md` and the latest release notes.
2. Use `09_REPOSITORY_OVERLAY_STAGE1/` for the secured web overlay.
3. Use `06_FIGMA_AND_DESIGN/` for brand tokens, UI specifications and Figma-import assets.
4. Use `10_AI_SERVICES/` for Noor policy, model orchestration and AI contracts.
5. Use `11_MOBILE_APP/` for the Flutter Android/iOS scaffold.
6. Use `12_BACKEND/` for the authenticated web/mobile BFF, migration 014, API contracts and operational controls.
7. Review `03_AUDIT_AND_REMEDIATION/` before any staging or production deployment.
8. Configure live credentials only through platform secret managers using `05_ENVIRONMENT_KEYS/` templates.

## Deployment order

1. Establish the canonical repository and confirm migrations 001–005.
2. Apply and validate migrations 006–013 in staging.
3. Apply migration 014 from the backend package.
4. Deploy AI Services.
5. Deploy the backend BFF to a private staging URL.
6. Integrate the web and mobile clients against the staging backend.
7. Run RLS, Noor concurrency, webhook, recovery and release-gate tests.
8. Promote approved immutable builds to production.

## Critical limitations

- No package is proof of live deployment.
- Migrations 001–005 and the complete canonical web repository remain required.
- Migration 014 has not been executed against the real staging database.
- The Flutter package still requires native platform generation, signing and store accounts.
- No live passwords, private keys, API keys or service-role credentials are included.
- No native `.fig` file is included; Figma-ready tokens and handoff specifications are included.

## Target repository and domain

- Repository: `afiyahinfinity/mtbdiaries1-afiyah-infinity`
- Web: `https://afiyahinfinityai.com`
- Recommended backend path: `apps/backend/`
- Recommended backend host: `https://api.afiyahinfinityai.com`
