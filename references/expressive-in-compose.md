# Material 3 Expressive in Jetpack Compose

Contents: what it is · theme setup · motion · component status · how to verify ·
review rules · sources.

## What it is

Expressive is an evolution of Material 3, not a new system. It keeps the same color
roles, type scale and shape scale and adds: a spring-based motion system, emphasized
type styles, extra corner sizes and shape morphing, more size and shape variants for
existing components, and a set of new components (button groups, split buttons,
floating toolbars, FAB menu, loading indicator, wavy progress, flexible app bars,
expressive list items, menus and search).

It is Compose-first on Android. The Material Web library is in maintenance mode and
does not implement Expressive, so never transfer a web token or component assumption
to a Compose review.

## Theme setup

Expressive is switched on at the theme. A project either uses the expressive theme
composable from `androidx.compose.material3` with a motion scheme, or wraps it in its
own theme function. In a review:

- There should be one theme entry point per screen. A second, nested theme is only
  justified when it changes something on purpose (a different color scheme for a
  subtree); otherwise it is a leftover.
- The motion scheme is part of the theme (`MaterialTheme.motionScheme`). Read motion
  specs from it rather than building `spring()`/`tween()` by hand, so the whole app
  moves together and a theme change retunes everything.

## Motion

From the `MotionScheme` API reference:

- `MotionScheme.expressive()` — "recommended motion scheme for prominent UI elements
  and hero interactions. It provides a visually engaging motion feel."
- `MotionScheme.standard()` — "basic motion scheme for utilitarian UI elements and
  recurring interactions. It provides a linear motion feel."
- Each scheme exposes six specs, in two families and three speeds
  (`default`, `fast`, `slow`):
  - **Spatial** (`defaultSpatialSpec`, `fastSpatialSpec`, `slowSpatialSpec`) — for
    animations "that may change the shape or bounds of the component".
  - **Effects** (`defaultEffectsSpec`, `fastEffectsSpec`, `slowEffectsSpec`) — for
    animations "that do not change the shape or bounds of the component. For example,
    color animation."

Review rules that follow:

- Size, position, offset, corner radius → a spatial spec. Color, alpha → an effects
  spec. A spatial (bouncy) spec on color or alpha is a bug: overshoot has no meaning
  for opacity and can flash.
- Hand-written durations and easings on component-level animation are a finding when a
  scheme spec would do. Screen transitions may still use easing and duration.
- Respect the system "remove animations" setting; nothing essential should depend on
  an animation finishing.
- Bounce on a control the user hits repeatedly (list rows, keyboard-adjacent buttons,
  toggles in a settings list) is usually a regression in perceived speed.

## Component status

Expressive APIs moved from experimental to stable gradually across the Material3 1.4
and 1.5 release lines, and a few were promoted and then reverted. The table records
what the official release notes said when this file was written (2026-11); treat it as
a pointer to *what to check*, not as current truth.

| API | Status noted in release notes (1.5.0 alphas) |
|---|---|
| Expressive theme, expressive color scheme | promoted to stable |
| `MotionScheme` | graduated from experimental |
| `ButtonGroup`, toggle buttons, expressive buttons | promoted to stable |
| `SplitButton` | non-experimental; the older `SplitButtonLayout` deprecated in its favor |
| Floating toolbar, FAB and FAB menu | non-experimental |
| Wavy progress indicators | promoted |
| Expressive list items, menus | no longer experimental |
| Flexible / two-row top app bars | no longer require the expressive opt-in |
| `LoadingIndicator`, `MaterialShapes` | stable promotion **reverted** — check before use |
| Expressive time picker, expressive text field options | added; graduation not noted |
| `LocalMotionScheme` | removed; use `MaterialTheme.motionScheme` |

## How to verify for the project's version

1. Find the Material3 version: an explicit version in the catalog wins over the BOM;
   otherwise map the BOM to its Material3 version on the BOM mapping page.
2. Open the Compose Material 3 release notes and read the entries up to that version
   for the API in question.
3. Check the API reference page for the composable: an experimental API shows its
   opt-in annotation there.
4. If the source uses `@OptIn(ExperimentalMaterial3ExpressiveApi::class)`, note it in
   the report as an accepted risk (the API can change), not as an error.

## Review rules for expressive treatment

- **Earned emphasis.** One primary action per screen gets the strongest treatment
  (largest size, most saturated container, fullest shape). If everything is emphasized,
  nothing is.
- **Shape carries meaning.** Larger or more unusual shapes signal importance or
  selection. Do not mix corner sizes arbitrarily within one group of siblings.
- **Emphasized type is for hierarchy**, not decoration: headings, key numbers, the
  selected item. Body copy stays in the regular styles.
- **Color does not replace structure.** Vibrant containers must still pass contrast
  and must still work in dark theme and with dynamic color if the app supports it.
- **Containment over dividers.** Grouping related controls in a tonal container reads
  better than stacking dividers; a divider inside a container should use the
  outline-variant role.
- **Utilitarian flows stay calm.** Forms, settings, pickers and confirmation dialogs
  should feel fast and predictable. Recommend expressive components there only when
  they remove a step or clarify state.

## Sources

- MotionScheme API reference —
  https://developer.android.com/reference/kotlin/androidx/compose/material3/MotionScheme
- Compose Material 3 release notes —
  https://developer.android.com/jetpack/androidx/releases/compose-material3
- androidx.compose.material3 package reference —
  https://developer.android.com/reference/kotlin/androidx/compose/material3/package-summary
- Material 3 for Web (maintenance mode, no Expressive) — https://m3.material.io/develop/web
- Material Design 3 — https://m3.material.io
