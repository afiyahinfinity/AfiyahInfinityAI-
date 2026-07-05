# Afiyah Infinity AI Design System

**Version:** 1.0.0  
**Release:** 5 July 2026  
**Status:** Implementation-ready starter system; no native `.fig` file is included.

This package defines the shared visual language and interface foundations for Afiyah Infinity AI across web, mobile, partner, reviewer and administration experiences.

## Purpose

- Provide one source of truth for brand and semantic design tokens.
- Standardise reusable components, states and interaction patterns.
- Support accessible web and mobile implementation.
- Prepare structured assets for import into Figma through Tokens Studio or variable-import tooling.
- Preserve Afiyah's faith-led, women-first and trust-centred product identity.

## Brand character

Afiyah should feel:

- **Trustworthy:** clear consent, visible control and no hidden action.
- **Calm:** low-noise interfaces with generous spacing.
- **Dignified:** elegant typography without ornamental excess.
- **Warm:** sister-centred language and humane feedback.
- **Accountable:** security, audit and approval states are explicit.
- **Inclusive:** accessible contrast, readable text and RTL-ready patterns.

## Package map

| Folder | Contents |
|---|---|
| `tokens/` | DTCG tokens, CSS variables, Tailwind preset, React Native tokens and Tokens Studio JSON |
| `components/` | Web component source, shared CSS and component documentation |
| `patterns/` | Noor, KYC, membership and governance interaction patterns |
| `figma/` | Import-ready variables, component inventory and Figma setup instructions |
| `icons/` | Interface SVG icons; these are not the official Afiyah logo |
| `accessibility/` | WCAG rules and verified contrast values |
| `examples/` | Standalone browser preview and integration examples |
| `governance/` | Versioning, contribution and release rules |

## Quick start — web

```css
@import "./tokens/afiyah.css";
@import "./components/web/afiyah-components.css";
```

```tsx
import { AfButton, AfCard, AfBadge } from "./components/web";
```

## Quick start — Tailwind

Merge `tokens/tailwind.preset.ts` into the project's Tailwind configuration:

```ts
import afiyahPreset from "./design-system/tokens/tailwind.preset";

export default {
  presets: [afiyahPreset],
  content: ["./app/**/*.{ts,tsx}", "./components/**/*.{ts,tsx}"],
};
```

## Quick start — mobile

Import values from `tokens/react-native.ts`. Font files are deliberately not bundled. Load licensed fonts through the application's normal font pipeline.

## Figma status

This package includes Figma-ready tokens and component specifications, but not a native `.fig` file. Follow `figma/README_FIGMA.md` to build or import the design system into a Figma file. Do not claim that a native Figma library exists until a real editable file is created and reviewed.

## Official brand assets

The SVGs in `icons/` are functional UI icons only. They do **not** replace the approved Afiyah Infinity logo. Add the master logo files only after confirming the authoritative SVG/AI/EPS source.
