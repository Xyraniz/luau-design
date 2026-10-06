# luau-design

An installable Codex skill for designing and implementing Roblox interfaces in Luau. It helps produce interfaces with a product-specific visual identity, clear hierarchy, responsive layouts, and controls that actually work in their intended runtime.

The design guidance distills four UI code references supplied for this project. It records their useful composition, theming, sizing, and interaction patterns without bundling or executing the original scripts. The collection leans heavily toward dark interfaces, so those palettes are examples, not a default theme.

## What it covers

- A repeatable design process that starts from the product, task, and existing project.
- Visual recipes for launch screens, multi-section tools, and compact settings.
- Roblox-specific implementation guidance for input, layout, and runtime context.
- A verification checklist that distinguishes code review, Luau syntax, runtime behavior, and rendered appearance.

## Install in Codex

Clone the repository into Codex's skills directory, then refresh the session's skill discovery.

PowerShell:

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex\skills" | Out-Null
git clone https://github.com/Xyraniz/luau-design.git "$env:USERPROFILE\.codex\skills\luau-design"
```

macOS / Linux:

```sh
mkdir -p ~/.codex/skills
git clone https://github.com/Xyraniz/luau-design.git ~/.codex/skills/luau-design
```

Invoke it with `$luau-design` when asking for Roblox UI work. Its description also makes it eligible for automatic skill selection on matching tasks.

## Files

- [`SKILL.md`](SKILL.md) — the skill entry point and core workflow.
- [`references/design-recipes.md`](references/design-recipes.md) — patterns extracted from the supplied code references, with adaptable layout recipes and sample palettes.
- [`references/runtime-and-validation.md`](references/runtime-and-validation.md) — Roblox runtime, interaction contracts, and evidence-based verification.

The original reference scripts remain outside this repository. Their code includes behavior beyond visual design; only the UI patterns are documented here.
