# Tomorro Brand Embedding in RDE Template — Design Spec

**Date:** 2025-05-25
**Status:** Approved

## Goal

Replace all Qovery branding in the remote-dev-env-template-tmr with Tomorro branding, and create a design system skill that ensures every AI-built app automatically uses Tomorro's visual identity.

## Decisions

- **Scope**: Full replacement — all Qovery references become Tomorro
- **Fonts**: Use real Ozik + Aeonik (commercial fonts, user provides `.woff2` files)
- **Design system behavior**: Applied by default; user must explicitly opt out
- **Deployment references**: Qovery CLI / qovery-deploy skill stays (infrastructure, not branding)

## Changes

### 1. Template Branding

**`resources/welcome.html`**
- Title: `Qovery RDE` -> `Tomorro Workspace`
- Logo SVG: Replace Qovery hexagon with Tomorro wordmark (viewBox 0 0 168 25, white fill)
- Accent color: `#642DFF` -> `#32D200` (Tomorro green-600)
- Logo width: 200px -> 180px

**`Dockerfile`**
- Comment: `Builder Workspace` -> `Tomorro Workspace`
- Extension dir: `qovery.builder-startup` -> `tomorro.builder-startup`
- App name: `Builder Workspace` -> `Tomorro Workspace`

**`builder-startup-extension/package.json`**
- Publisher: `qovery` -> `tomorro`

**`entrypoint.sh`**
- App name, comments: `Builder Workspace` -> `Tomorro Workspace`
- New function: `generate_tomorro_design_skill()`
- Updated `.gitignore` entries to include `.claude/`

### 2. AI Instructions (CLAUDE.md & SKILL.md)

- `Builder Workspace` -> `Tomorro Workspace`
- `Platform Engineering team` -> `Tomorro team`
- New design system directive in "Technical defaults"
- Updated "Make it look good" bullet
- Added Tomorro Design System to "What's available"

### 3. WELCOME.md

- `Builder Workspace` -> `Tomorro Workspace`
- `Platform Engineering team` -> `Tomorro team`

### 4. Tomorro Design System Skill (new)

**`resources/TOMORRO_DESIGN_SYSTEM.md`** — Full design system with:
- Default behavior (apply always, opt-out only)
- Color palette (28 tokens from tomorro.com CSS)
- Typography (Ozik display, Aeonik body, Instrument Serif accent)
- Tailwind CSS configuration
- Component patterns (buttons, cards, nav, forms)
- Logo SVG (white-on-dark and dark-on-light variants)
- Layout principles

Installed to both Claude Code and OpenCode skill directories by entrypoint.sh.

### 5. Font Strategy

- Plumbing for `resources/fonts/` directory (user provides .woff2 files)
- Design system references local fonts with Google Fonts fallback (Inter, Sora)
