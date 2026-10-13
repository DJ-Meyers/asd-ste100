# ASD-STE100 APM integration

An unofficial [Agent Package Manager (APM)](https://github.com/microsoft/apm)
integration for GitHub Copilot CLI and Copilot in VS Code.

This wrapper adds an always-active instruction for automatic STE mode selection.
It installs [danyuchn/asd-ste100-skill](https://github.com/danyuchn/asd-ste100-skill)
as a dependency. It does not copy, modify, or replace the upstream skill.
DJ-Meyers maintains this wrapper, not the upstream skill.

## Install

Install APM first with the [APM installation instructions](https://microsoft.github.io/apm/getting-started/installation/).
Package validation used APM 0.33.0.
Run these commands from the project where you use Copilot.

Install the pinned wrapper release:

```sh
apm install DJ-Meyers/asd-ste100#v0.1.0 --target copilot
apm compile --target copilot
```

For development, install the default branch instead:

```sh
apm install DJ-Meyers/asd-ste100 --target copilot
apm compile --target copilot
```

Start a new Copilot session after installation.
The instruction asks the agent to load `asd-ste100` before its first answer.
The agent applies the skill only to requests within the upstream skill's stated scope.
Creative and marketing text remains outside that scope.

## Modes and labels

The agent selects a mode for each response:

- **Strict**: Procedures, errors, tool descriptions, safety text, and instructions for agents.
- **STE-flavored**: READMEs, descriptions of pull requests, changelogs, and explanatory prose.

The upstream skill defines the rules for both modes.
An explicit mode request takes precedence when the skill applies.
The wrapper changes only mode reporting, which upstream normally keeps internal.

The first response begins with exactly one bold blockquote label:

> **STE mode: Strict**

or:

> **STE mode: STE-flavored**

Later responses include a label only when the mode changes.
The label describes the selected mode, not certified compliance with ASD-STE100.
The upstream skill does not reproduce ASD's approved dictionary.

The label appears in assistant text.
This package cannot create a native collapsible chat activity row such as
“Applied ASD-STE100 · Strict mode”.
That feature requires support from the host or an extension.
Instructions guide model behavior. They cannot guarantee that every response follows the rules.

## Files and deployment

The source instruction is `.apm/instructions/asd-ste100-mode.instructions.md`.
Its `applyTo: "**"` setting applies to all files.
APM deploys the instruction under `.github/instructions/`.
APM may add a package suffix to the deployed filename.
With APM 0.33.0, the dependency's skill is
`.agents/skills/asd-ste100-skill/SKILL.md`.
Its frontmatter names the skill `asd-ste100`, independent of the directory name.
APM also installs the skill's supporting files and license.
Compilation prepares Copilot context where the target requires it.

This is a regular APM package, not a native Copilot Agent Plugin.
It has no session-start hook, shell lifecycle script, or MCP dependency.
No hook downloads upstream files at session start.

## Pins and updates

The manifest follows `danyuchn/asd-ste100-skill#master`.
APM records the exact resolved commit in `apm.lock.yaml`.
This repository commits that lockfile.

The wrapper release tag pins the wrapper's source.
It does not promise that a new consumer resolves the same upstream commit.
APM resolves dependencies into the consumer's own lockfile.
Check that lockfile after installation, then commit it for repeatable installs.
Use `apm install --frozen --target copilot` to replay a matching consumer lockfile.

To obtain updates in a consumer project or this wrapper checkout:

```sh
apm update
git diff apm.lock.yaml
```

Review the resolved commits and upstream changes.
Commit the lockfile only after you accept those changes.
An existing lockfile retains its resolved commits until an explicit update.
For a pinned wrapper release, change its tag in the consumer manifest to obtain a newer wrapper.

## Validate this package

```sh
apm install --target copilot
apm compile --validate --target copilot
apm compile --dry-run --target copilot --verbose
apm audit
```

Also test a real install into an empty consumer directory outside the wrapper checkout.
Do not nest the consumer inside the wrapper, because local installation copies the source directory.

```sh
apm install /absolute/path/to/asd-ste100 --target copilot
apm compile --target copilot
apm audit
```

Check the deployed instruction, transitive skill, and resolved commit.
A dry run alone does not check deployment.
Compilation can produce no new files when `.github/instructions/` already contains the instruction.
APM omits duplicate context in that case.

Release validation used APM 0.33.0 on October 12, 2026.
Manifest validation, compilation preview, and audit passed.
A real local consumer install resolved the transitive upstream skill and deployed the wrapper instruction.
The consumer audit found no drift or scan issues.

## Attribution and license

The wrapper uses the [MIT license](LICENSE), copyright 2026 DJ-Meyers.
The upstream skill uses MIT, copyright 2026 Dustin Yuchen Teng (`danyuchn`).
Its license and notices remain with the installed dependency.
This wrapper claims no ownership of upstream work.
Neither project is an official ASD standard or a certified STE authoring tool.

## APM references

- [Manifest schema](https://microsoft.github.io/apm/reference/manifest-schema/)
- [Lockfile specification](https://microsoft.github.io/apm/reference/lockfile-spec/)
- [Package installation](https://microsoft.github.io/apm/consumer/install-packages/)
