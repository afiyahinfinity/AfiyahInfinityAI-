# Afiyah Infinity AI — Master Pack v3

**Release:** 5 July 2026  
**Purpose:** Consolidated engineering, security, deployment, design-system and AI-services handoff.

## Start here

1. Read `MASTER_CONTENTS.md`.
2. Use `09_REPOSITORY_OVERLAY_STAGE1/` for the secured repository overlay.
3. Use `06_FIGMA_AND_DESIGN/Afiyah_Design_System_v1.0.0/` for design tokens, components and Figma-import assets.
4. Use `10_AI_SERVICES/Afiyah_AI_Services_v1.0.0/` for AI architecture, API contracts, Noor Guard, model orchestration and governance controls.
5. Review `03_AUDIT_AND_REMEDIATION/` before deployment.
6. Configure secrets using templates in `05_ENVIRONMENT_KEYS/`; never commit live credentials.
7. Deploy to staging before production.

## Critical limitations

- The repository overlay and AI-services module are not a complete standalone Next.js repository. They require the existing application root and migrations 001–012.
- Migration 013 must be executed and tested in a staging Supabase project before production.
- No live keys, passwords or production secrets are included.
- No native `.fig` file is included. The master pack contains genuine Figma-import assets and specifications, not an editable Figma library.
- Production deployment requires access to the real GitHub repository, Supabase projects and hosting platform.

## Repository

Target repository: `afiyahinfinity/mtbdiaries1-afiyah-infinity`

## Domain

Target production domain: `https://afiyahinfinityai.com`
