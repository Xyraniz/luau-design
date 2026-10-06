---
name: luau-design
description: Design, build, or refine Roblox interfaces in Luau, including menus, windows, HUDs, launchers, dashboards, and controls. Use when turning a UI request or visual reference into a distinctive, responsive interface with working behavior in its intended Roblox runtime.
---

# Roblox UI design in Luau

Make the interface feel specific to its game or product and make every visible control do what its label promises. Do not start from a generic dark panel, glass blur, neon gradient, or card grid.

## Workflow

1. **Inspect the actual project.** Read its instructions, UI entry point, existing component library, assets, code conventions, and intended runtime before changing files. Preserve working architecture and behavior.
2. **Identify the target experience.** Determine where the UI appears, who uses it, the main task, the supported devices and input types, and how it opens and closes. Reuse facts from the project. Ask only if a missing answer would change the implementation materially; otherwise choose a reasonable direction and keep moving.
3. **Write a visual thesis.** Choose one coherent visual direction and one memorable move in composition, typography, color, or interaction. Explain the choice briefly when useful. Read [design recipes](references/design-recipes.md) when creating or translating a visual direction; use them as evidence and inspiration, never as a fixed skin.
4. **Map behavior before styling controls.** For each control, define its starting value, action, resulting state, feedback, and any callback or project behavior it must invoke. Read [runtime and validation](references/runtime-and-validation.md) when implementing or verifying a Roblox UI.
5. **Build the hierarchy and system.** Separate navigation, content, and primary actions. Define a small set of color, spacing, type, and shape tokens. Use Roblox layout primitives for repeated content, then style and animate only what supports identity or usability.
6. **Run and inspect the real user path.** Open the UI from its actual entry point, exercise each control, and inspect the rendered result at the target viewport and a narrow viewport when applicable. Fix observed defects before delivery.

## Visual quality bar

- The references are mostly dark, compact utility interfaces. Transfer their hierarchy, restrained surfaces, useful state changes, and control alignment; do not assume every product should be dark or compact.
- Make one deliberate visual choice that belongs to this game: a shape language, art direction, branded accent, unusual but legible composition, or meaningful motion. Keep everything else disciplined enough to support it.
- Keep depth legible: distinguish the background, content surface, raised overlays, primary text, secondary text, border, and semantic states. One accent used with intention usually beats multiple competing highlights.
- Establish a consistent spacing and type scale. Give labels, descriptions, values, and actions different roles. Let real text and localization determine component height.
- Use `UIListLayout`, `UIGridLayout`, `UIPadding`, and `LayoutOrder` where they fit. Use constraints intentionally; verify which layout or constraint owns an object's size.
- Use real project content and available assets. Do not invent telemetry, user data, asset IDs, downloadable fonts, or decorative copy to fill empty space. Prefer a clear text label to a fake icon made from emoji or arbitrary Unicode.
- Avoid card soup, identical pill buttons everywhere, excessive radii, blur, glow, gradients, and shadow layers. Remove an effect if it does not improve hierarchy, feedback, or product identity.

## Functional quality bar

- Treat the visual design and the interaction as one component. A button needs a real action; a toggle must reflect the value it controls; a slider must clamp and report its value; a selector must show and apply the selected option; a text field needs a defined submit and error behavior.
- Use `GuiButton.Activated` for actions that need to work with mouse, touch, and console UI navigation. Hover may enhance pointer use, but must not be the only feedback or activation path.
- Use one source of truth for control state and its rendered appearance. Avoid duplicate event connections, stale closures, and callbacks that silently stop working after a page switch.
- Provide clear selected, pressed, disabled, loading, success, and error states where relevant. Keep text contrast and focus/navigation cues understandable without color alone.
- Connect only behavior requested by the task or already established by the product. UI work does not authorize unrelated network calls, file writes, telemetry, or dynamic code loading.

## Runtime boundaries

- Preserve the project's real UI context. For a normal Roblox experience, follow its `StarterGui` / `PlayerGui` path; do not move a screen to `CoreGui` to work around a parent or lifecycle bug.
- Do not assume executor globals or APIs such as `getgenv`, `gethui`, `loadstring`, `syn`, `writefile`, or `game:HttpGet`. Use specialized APIs only when the repository explicitly establishes that runtime and the task needs them.
- Inspect third-party or user-provided references statically. Do not execute them just to see a design, and do not port unrelated downloads, hooks, or service behavior into a UI task.
- Prefer existing Roblox-native components over adding a UI framework. Do not introduce external asset or font downloads when the interface can use project assets or native elements.

## Delivery standard

Report the changed UI, the interactions exercised, and the exact verification performed. Keep static inspection, Luau syntax checking, runtime execution, rendered visual inspection, and real-device testing as separate claims. If Studio or the client was unavailable, say that runtime or visual behavior remains unverified; never infer it from a successful parse.
