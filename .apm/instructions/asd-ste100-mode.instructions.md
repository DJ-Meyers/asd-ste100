---
applyTo: "**"
description: Select ASD-STE100 modes and report mode changes in assistant text.
---
# ASD-STE100 mode selection

At the start of every agent session, load the `asd-ste100` skill before you answer the first user request.
Apply the skill only when its stated use cases match the request.
Do not apply STE rewriting to creative or marketing text.

For every response, select the closest `asd-ste100` mode: `Strict` or `STE-flavored`.
Use the upstream skill's mode definitions.
Honor an explicit mode request when the skill applies.
The mode label requirement below overrides the upstream instruction to keep the mode internal.
All other upstream behavior remains unchanged.

At the beginning of the first response, add exactly one of these Markdown lines:

- `> **STE mode: Strict**`
- `> **STE mode: STE-flavored**`

If the selected mode changes during the session, add the new mode blockquote at the beginning of that response.
Do not repeat the mode blockquote when the mode does not change.
Put the label in assistant response text.
Do not claim that the label creates a native chat activity row.
