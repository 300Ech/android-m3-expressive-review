# android-m3-expressive-review

An agent skill that reviews Jetpack Compose screens against Android UI guidelines and
Material 3 Expressive, and helps build new screens that pass the same review.

What makes it different from a generic Material checklist:

- **It respects your design system.** A per-project `M3_REVIEW_MAP.md` tells the
  reviewer which wrappers and tokens stand for which Material concepts, so a screen
  built from `AppButton` and `AppSpacing` is not reported as "not Material".
- **It treats the official docs as the source of truth.** Version-sensitive facts
  (which Expressive APIs are stable, default measurements) are verified against the
  release notes for the project's Material3 version instead of being trusted from a
  table.
- **It separates guideline, project rule and taste** in every finding.
- **It treats Expressive as a decision**: emphasis for hero moments, calm for
  utilitarian flows.

## Contents

```
SKILL.md                               workflow, checklist, report format
references/android-ui-guidelines.md    per-area checks with thresholds and sources
references/expressive-in-compose.md    Expressive in Compose, API status, motion rules
references/project-map-template.md     template for your project's map
```

No scripts, hooks or network calls: the skill is Markdown only.

## Install

Copy the folder into a skills directory your agent reads, for example
`~/.claude/skills/android-m3-expressive-review/` (user-wide) or
`<project>/.claude/skills/android-m3-expressive-review/` (one project).

## Use

- "Review `SettingsScreen.kt` against Material 3 Expressive."
- "Does this bottom sheet follow Android guidelines? Here is a screenshot."
- "Build the invite form and run the M3 review on it before you show me."

Add an `M3_REVIEW_MAP.md` to your project (start from the template) to get findings in
your own component and token names.

## Maintenance note

`references/expressive-in-compose.md` records the status of Expressive APIs as of
2026-11. The skill tells the reviewer to re-check the release notes; update the table
when you notice it has drifted.

## License

MIT, see [LICENSE](LICENSE).
