# M3 review map — <project name>

Copy this file to the repository or workspace root as `M3_REVIEW_MAP.md` and fill it
in. The review skill reads it first, so findings are phrased in the project's own
vocabulary and its wrappers are not reported as violations.

## Theme

- Theme entry point: `<AppTheme>` in `<path>`
- Expressive: `<how it is enabled, or "not used">`
- Dark theme / dynamic color: `<supported? how>`

## Tokens

| Kind | Use | Instead of |
|---|---|---|
| Color | `<AppColors.*>` | `MaterialTheme.colorScheme.*`, `Color(0x…)` |
| Typography | `<AppTypography.*>` | ad hoc `fontSize` / `fontWeight` |
| Spacing | `<AppSpacing.xs…xxl = 4…48dp>` | raw `.dp` |
| Shape | `<AppShapes.*>` | raw `RoundedCornerShape(n.dp)` |
| Layout limits | `<max form width, dialog width…>` | inline numbers |

## Components

List each wrapper and the Material component it stands for. Anything not listed is
expected to be the raw Material 3 composable.

| Need | Use | Wraps |
|---|---|---|
| Screen scaffold | `<AppScaffold>` | `Scaffold` |
| Top app bar | `<AppTopAppBar>` | `TopAppBar` |
| Button | `<AppButton(style = …)>` | `Button` / `FilledTonalButton` / … |
| Text field | `<AppTextField>` | `OutlinedTextField` |
| Dialog | `<AppAlertDialog>` | `AlertDialog` |
| … | | |

## Project rules that override the defaults

- `<e.g. errors are a dialog driven by state, never a toast or snackbar>`
- `<e.g. a design change to a standard control goes in the design-system component>`
- `<e.g. forms are capped at N dp by the scaffold>`

## Known, accepted deviations

- `<deviation>` — `<why>` — `<owner / ticket>`

## Where the screen standard lives

- `<path to architecture / screen standard docs the reviewer should also apply>`
