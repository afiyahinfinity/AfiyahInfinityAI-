# Afiyah Mobile App

**Version:** 1.0.0 engineering scaffold  
**Release date:** 5 July 2026  
**Target:** Flutter 3.44 stable, Android and iOS

This package turns the original placeholder into a source-first Flutter application scaffold for Afiyah Infinity AI. It includes app architecture, core navigation, secure session storage, Noor safety middleware, API clients, representative product screens, tests, release documentation, CI and platform-bootstrap tooling.

## Product boundary

The mobile app provides education, wellness tracking, portfolio visibility and non-advisory AI insights. It does **not** custody funds, execute trades, provide regulated investment advice or make autonomous consequential decisions. Transactions and regulated services must be delivered through appropriately licensed partners and server-side controls.

## Included experience

- Welcome, consent and authentication flows
- Dashboard and Barakah snapshot
- Educational portfolio overview
- Personalized insight feed with explicit non-advisory labels
- Noor chat with local pre-flight safety checks and server-side enforcement
- Profile, privacy and disclosure screens
- Secure token storage and optional biometric re-entry
- Environment separation and compile-time configuration
- Accessible Material 3 theme aligned with Afiyah brand tokens
- RTL-ready layout guidance and localization boundaries

## Package status

This is a **staging-oriented source scaffold**, not a store-ready binary. The generated Android and iOS platform shells are intentionally not committed because they should be produced with the approved Flutter SDK and signing configuration. Run `scripts/bootstrap_platforms.sh` once after installing Flutter.

## Quick start

```bash
flutter --version
./scripts/bootstrap_platforms.sh
flutter pub get
flutter analyze
flutter test
flutter run \
  --dart-define=AFIYAH_ENV=staging \
  --dart-define=AFIYAH_API_BASE_URL=https://staging-api.afiyahinfinityai.com \
  --dart-define=AFIYAH_MOCK_MODE=true
```

## Important security rules

- Never embed Supabase service-role, Anthropic, OpenAI, Resend or other privileged keys in the app.
- Mobile receives only short-lived user tokens issued by the backend.
- All Noor authorization, session ownership, policy enforcement and audit logging remain server-side.
- Cycle data, raw identity documents and private health data must never be included in model prompts.
- Biometric access is a local re-entry convenience, not a substitute for server authentication.
- No sensitive response body is written to logs.

## Repository placement

Recommended monorepo path:

```text
apps/mobile/
```

The package integrates with:

- `Afiyah_AI_Services_v1.0.0` for API and Noor contracts
- `Afiyah_Design_System_v1.0.0` for tokens and interaction patterns
- Supabase Auth and the existing Afiyah backend through server-side APIs

## Start here

1. Read `docs/ARCHITECTURE.md`.
2. Review `docs/SECURITY.md` and `docs/PRIVACY_AND_COMPLIANCE.md`.
3. Confirm API routes against `docs/API_INTEGRATION.md`.
4. Generate platform shells with `scripts/bootstrap_platforms.sh`.
5. Configure staging with `docs/ENVIRONMENTS.md`.
6. Run the checks in `docs/TEST_PLAN.md`.
7. Complete Android and iOS release checklists only after staging approval.

## Known blockers before production

- Real API authentication and refresh-token endpoints must be confirmed.
- App Store and Play Store signing identities are not included.
- Device attestation is documented but not wired to a production provider.
- Push notification credentials and entitlements are not included.
- Privacy, medical, AI and Shariah disclosures require final legal and governance approval.
- A native editable Figma file has not been supplied; this package maps to the existing token and handoff assets.
