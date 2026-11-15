---
name: android-m3-expressive-review
description: >
  Audit and guide Jetpack Compose screens against Android UI guidelines and Material 3
  Expressive. Use this whenever the user asks to build, change, review or polish an
  Android screen, composable, dialog, bottom sheet or form in Compose, asks whether a UI
  "follows Material", "looks Android", "is Material 3 Expressive", or wants a design /
  accessibility / adaptive-layout check before a PR. Works on source files, previews and
  device screenshots, and respects the project's own design system through a mapping file.
---

# Android + Material 3 Expressive review

Review a Compose UI the way a careful Android design reviewer would: against the
official guidance first, and against the project's own design system second. The
output is a short, evidence-based report, not a rewrite.

## Why this skill exists

Generic "is this Material?" checks produce two kinds of noise. They flag a project's
design-system wrappers as violations (the screen uses `AppButton`, not `Button`), and
they repeat token tables that may be older than the library the project ships. This
skill avoids both: it reads the project's mapping file to learn what the wrappers
mean, and it treats the official documentation as the source of truth whenever a
version-sensitive fact matters.

## Before you review

1. **Find the project map.** Look for `M3_REVIEW_MAP.md` (or `m3-review-map.md`) in the
   repository root, the workspace root, `docs/` or `.claude/`. It lists the project's
   theme entry point, tokens and wrapper components, plus rules that override the
   defaults here. If there is none, review against raw Material 3 and say so in the
   report; offer `references/project-map-template.md` as a starting point.
2. **Establish the versions.** Read the version catalog or build files for the Compose
   BOM, `androidx.compose.material3`, `compileSdk`, `targetSdk` and `minSdk`. Expressive
   APIs changed status across Material3 releases, so a rule that holds for one version
   can be wrong for another. `references/expressive-in-compose.md` explains how to check.
3. **Collect evidence.** Prefer, in this order: the composable source and its state
   class, the screen's previews, then a screenshot or the running app. A screenshot
   alone cannot prove semantics, touch targets or theming; source alone cannot prove
   contrast or clipping. Say which evidence each finding rests on.

## What to check

Work through these areas. Each one names the question that matters; the reference
files hold the detail. Skip an area that does not apply and say so.

| Area | The question | Detail |
|---|---|---|
| Theme and tokens | Do colors, type, shape and spacing come from the theme (or the project's tokens) instead of literals? | `references/android-ui-guidelines.md` §Tokens |
| Color roles | Is every foreground on its intended background pair, in light and dark? | §Color |
| Typography | Are type roles used for their purpose, and does the screen survive 200% font scale? | §Typography |
| Shape | Do corner sizes follow the shape scale and the component's default? | §Shape |
| Components | Is each need met by the right Material component and variant, used as documented? | §Components |
| Layout and spacing | Is spacing on the 4/8dp grid, with readable line length and no fixed sizes that clip? | §Layout |
| Adaptive | Does the layout respond to window size class, not orientation or device type? | §Adaptive |
| System integration | Edge-to-edge, insets, IME, predictive back, dark theme. | §System |
| Accessibility | 48dp targets, contrast, semantics, labels, focus order, announcements. | §Accessibility |
| States | Loading, empty, error, disabled, pressed, long text, missing data. | §States |
| Expressive | Is expressive treatment used where it helps (hero moments) and absent where it hurts (utilitarian flows)? | `references/expressive-in-compose.md` |
| Motion | Does motion come from the theme's motion scheme, with the spatial/effects split respected? | `references/expressive-in-compose.md` §Motion |

Two habits make the review trustworthy:

- **Separate the layers.** A finding is either *guideline* (Android or Material says
  so, with a link), *project* (the map or the project's standard says so), or *taste*
  (your judgment). Label each one. Mixing them is how reviews lose credibility.
- **Verify before asserting a version-sensitive fact.** Component availability, opt-in
  annotations and default values move between releases. If a finding depends on one,
  check the release notes or API reference for the project's version, or mark the
  finding "unverified for this version".

## Expressive is a decision, not a default

Material 3 Expressive adds stronger shape, color, type and spring motion. The official
API reference describes the expressive motion scheme as recommended "for prominent UI
elements and hero interactions" and the standard scheme "for utilitarian UI elements
and recurring interactions". Carry that distinction through the whole review: a form
that someone fills in ten times a day is utilitarian; an onboarding hero or a
call-to-action is prominent. Flag expressive treatment that slows a utilitarian task,
and flag a hero moment that reads as flat. Do not ask for expressive components just
because they exist.

## Report format

Keep it short enough to read in a PR. Use this structure:

```
# UI review: <screen or composable>

Evidence: <source files, previews, screenshots, device/OS, light/dark, font scale>
Versions: Compose BOM <x>, material3 <y>, targetSdk <z>
Project map: <path, or "none — reviewed against raw Material 3">

## Verdict
<one or two sentences: ready / ready with fixes / needs work, and why>

## Must fix
- [guideline|project] <finding> — <file:line or screenshot region>. <fix>. <link when guideline>

## Should fix
- ...

## Consider
- [taste] ...

## Checked and fine
<areas that passed, in one line each, so the reader knows they were looked at>

## Not verified
<what the evidence could not show, and what would show it>
```

Rank by user impact: something that blocks a task or fails accessibility outranks a
token nit. Give the smallest fix that resolves each finding, in the project's own
vocabulary (its wrapper names, its tokens). If the right fix belongs in the design
system rather than the screen, say that instead of proposing a per-screen override.

## When asked to build rather than review

Use the same references as a checklist while writing: start from the project map so the
screen uses the project's wrappers and tokens from the first line, then run the review
on your own result before presenting it, and include the "Not verified" section so the
author knows what still needs a device.

## Reference files

- `references/android-ui-guidelines.md` — the per-area checks with thresholds and the
  official pages to cite.
- `references/expressive-in-compose.md` — what Expressive means in Compose, API status
  and how to verify it for a given version, motion rules.
- `references/project-map-template.md` — template for a project's design-system map.
