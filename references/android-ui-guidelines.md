# Android UI guidelines: what to check

Contents: Tokens · Color · Typography · Shape · Components · Layout · Adaptive ·
System · Accessibility · States · Sources.

Numbers below are the commonly cited defaults. Component measurements change between
Material releases, so when a finding depends on an exact value, confirm it on the
component's spec page (m3.material.io/components) or in the Compose defaults object
(`ButtonDefaults`, `TopAppBarDefaults`, …) for the project's version.

## Tokens

- Colors come from `MaterialTheme.colorScheme` or the project's color tokens. A literal
  `Color(0x…)` in a screen is a finding unless the project map allows it.
- Sizes and gaps come from spacing tokens or resources. A raw `.dp` in a feature screen
  is a finding when the project has tokens; a raw `.sp` almost always is, because it
  bypasses the type scale.
- A token used for something it does not mean (a spacing token as a stroke width, a
  surface color as a text color) is worth a "consider": it works today and breaks when
  the token is retuned.

## Color

- Use roles in their pairs: `onPrimary` on `primary`, `onSurface` /
  `onSurfaceVariant` on any `surface*`, `onErrorContainer` on `errorContainer`.
  Unpaired combinations are where contrast breaks in dark theme and dynamic color.
- Depth is expressed with tonal surface containers (`surfaceContainerLowest` …
  `surfaceContainerHighest`) more than with shadows.
- `outline` is for boundaries that matter (text field borders); `outlineVariant` is
  for decorative dividers.
- Check light and dark. Check dynamic color if the app enables it.
- Color is never the only signal for state (error, selected, disabled).

## Typography

- Fifteen roles: Display, Headline, Title, Body, Label × Large/Medium/Small, plus the
  emphasized variants. Use a role for its purpose: Title for short headers, Body for
  reading text, Label for buttons, chips and captions.
- Do not set `fontSize` or `fontWeight` ad hoc on top of a role to "fix" hierarchy;
  pick the right role.
- Test at the largest system font size (Android 14+ scales non-linearly up to 200%).
  Text must wrap or scroll, never clip; fixed heights on text containers are the usual
  cause.
- Long strings: names, emails and translated labels overflow first. Check `maxLines`
  with an overflow strategy, and that truncation does not hide the only distinguishing
  part.

## Shape

- Shape scale: none 0dp, extra-small 4dp, small 8dp, medium 12dp, large 16dp,
  extra-large 28dp, full. Expressive adds large-increased 20dp, extra-large-increased
  32dp and extra-extra-large 48dp.
- Components have default shapes (buttons full, cards medium, dialogs and sheets
  extra-large, text fields extra-small or small depending on variant). Overriding a
  default per screen is a finding; the change belongs in the theme or design system.
- Siblings in one group share one corner size.

## Components

- Pick by job: one filled/primary button per screen for the main action; tonal,
  outlined and text buttons for descending emphasis. Destructive actions are confirmed.
- Top app bar: navigation icon reflects what it does (back arrow navigates up; close
  dismisses a task or modal flow). At most a few actions; the rest go in an overflow.
- Text fields have a visible label (not only a placeholder), supporting or error text
  below, the right keyboard type and IME action, and a length limit when the backend
  has one. A counter is shown when the limit is something the user can hit.
- Switches apply immediately; checkboxes and radios belong to a form that is submitted.
- Dialogs interrupt for decisions and errors; they have a clear title or question and
  one to two actions with the confirming action on the end side. Bottom sheets hold
  secondary content and pickers. Snackbars report a lightweight result and never carry
  an error the user must act on.
- Lists: consistent leading/trailing structure, and the whole row is the touch target
  when the row has one action.
- Progress: indeterminate for unknown duration, determinate when it can be known; the
  control that started the work shows the busy state and is disabled while busy.

## Layout

- Spacing on a 4dp grid with 8dp as the working unit; typical screen side margin is
  16dp on compact widths and larger on wider windows.
- Group related controls and separate groups with more space than exists inside them.
- Reading text and forms should not stretch across a wide window; cap the content
  width (forms commonly 480–600dp, reading text roughly 40–75 characters per line).
- No fixed heights on containers that hold text. No content hidden behind system bars
  or the keyboard.
- Scrolling: the whole form scrolls as one; the primary action stays reachable when
  the keyboard is open.

## Adaptive

- Decide layout from the window size class, not orientation, device model or raw
  screen width. Width classes: compact < 600dp, medium 600–839dp, expanded ≥ 840dp
  (large ≥ 1200dp and extra-large ≥ 1600dp exist for desktop-class windows). Height
  compact is < 480dp.
- Navigation changes with width: bar on compact, rail on medium, rail or drawer on
  expanded. List-detail and supporting-pane layouts replace push navigation on wide
  windows.
- Check a foldable posture if the app targets them: nothing interactive across the
  hinge.
- Multi-window and resizing must not lose state.

## System

- Edge-to-edge is enforced for apps targeting API 35 on Android 15+. Content draws
  behind the bars and is padded with window insets; scaffolds pass the padding to
  their content, and it must actually be applied.
- IME: fields stay visible when focused (`imePadding` or inset-aware scaffolds).
- Back: system back and the app bar's navigation icon do the same thing; predictive
  back needs back handling registered through the back dispatcher, not intercepted key
  events. A back press with unsaved input either saves or asks.
- Dark theme and (if enabled) dynamic color are supported by using roles, not literals.
- Respect system settings: font size, display size, bold text, remove animations.

## Accessibility

- Touch targets at least 48×48dp, including icon-only buttons and small chips; keep
  about 8dp between adjacent targets.
- Contrast: 4.5:1 for normal text, 3:1 for large text and for meaningful non-text
  elements (icons, field borders, focus indicators).
- Every interactive element has an accessible name. Icon-only controls need a
  `contentDescription`; decorative images pass `null`. Do not put a
  `contentDescription` on an editable text field — it replaces the value the screen
  reader would speak; use the field's label.
- Merge semantics for rows that act as one control, set `Role` on custom clickables,
  mark headings as headings.
- State changes that appear without focus moving (errors, "copied", counters reaching
  a limit) are announced with a live region.
- Focus order follows reading order; nothing is reachable by touch but not by
  keyboard or switch access.
- Errors say what is wrong and how to fix it, next to the field that caused them.

## States

Walk each state and confirm it is designed, not accidental:

- Loading (initial and in-place), empty, error with a way to retry, content.
- Disabled and busy controls; double activation of the primary action.
- Offline or slow network; a failed submit keeps the user's input.
- Extremes: zero items, one item, many items; the longest plausible text; missing
  optional data.
- Rotation and process death for anything the user typed.

## Sources

- Material Design 3 — https://m3.material.io (foundations, styles, components)
- Compose Material 3 — https://developer.android.com/develop/ui/compose/designsystems/material3
- Adaptive layouts and window size classes —
  https://developer.android.com/develop/ui/compose/layouts/adaptive
- Edge-to-edge — https://developer.android.com/develop/ui/compose/system/system-bars
- Predictive back — https://developer.android.com/guide/navigation/custom-back/predictive-back-gesture
- Accessibility in Compose — https://developer.android.com/develop/ui/compose/accessibility
- Android accessibility principles — https://developer.android.com/guide/topics/ui/accessibility/principles
- WCAG 2.2 contrast criteria — https://www.w3.org/TR/WCAG22/
