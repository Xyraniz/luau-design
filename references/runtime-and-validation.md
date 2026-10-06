# Roblox runtime, interaction, and validation

Use this guide when building or checking a real UI, not for a visual-only concept. Confirm the project's Luau version, UI entry point, parent hierarchy, and runtime before choosing APIs. The same layout can need different parents, focus behavior, and input support in different Roblox contexts.

## Choose the actual UI context

| Target | Integration direction |
| --- | --- |
| Normal player-facing experience UI | Follow the project's `StarterGui` / `PlayerGui` lifecycle and existing LocalScript or component entry point. |
| UI attached to a world object | Preserve the existing `SurfaceGui` or `BillboardGui` placement and its world-facing constraints. |
| Studio plugin or other documented Roblox context | Use the project's established plugin/widget entry point and permissions. |
| Executor-specific project | Confirm that the repository explicitly documents that environment. Do not infer it from a pasted script or add executor APIs to make a UI appear. |

If the repository already uses a UI library, follow its conventions for constructing windows, pages, controls, callbacks, and teardown. Do not replace a working architecture with a new framework for visual convenience alone.

## Layout and input

- Use `UIListLayout` or `UIGridLayout` for repeated rows/cards, `UIPadding` for container spacing, and explicit `LayoutOrder` for stable ordering.
- Use `AutomaticSize` only where content should drive size. Put reasonable min/max bounds on windows that should not grow indefinitely.
- Combine `UDim2` scale/offset with `UISizeConstraint` or `UIAspectRatioConstraint` intentionally. A constraint can override the size a layout would otherwise assign; test the resulting `AbsoluteSize` at real viewport sizes.
- Account for safe areas, orientation changes, long/localized text, scrolling, keyboard coverage, and the number of actual controls. Do not shrink a desktop menu into a phone; reorganize it when space or input changes.
- If the game supports touch, console, or gamepad, verify target sizes, focus, and directional navigation in addition to pointer behavior. Hover is optional feedback for a pointer, never a required state or action.

## Define a behavior contract per control

Before implementing a control, write down its initial value, event, side effect or callback, visual state changes, and cleanup owner. For example: “Settings opens the settings page; while loading, the action is disabled; success switches pages; close destroys its connections.” This catches attractive controls with no working behavior.

Use `GuiButton.Activated` for a button action that must support mouse, touch, and console navigation. Reserve `MouseEnter` / `MouseLeave` for pointer-only visual enhancement. A minimal connection should look like this:

```lua
settingsButton.Activated:Connect(function()
	settingsPage.Visible = true
end)
```

| Control | Visible contract | Interaction checks |
| --- | --- | --- |
| Button | Action label; pressed, disabled, loading, and result states as needed | Correct action, repeat activation, and feedback |
| Toggle | On/off is distinct from hover and disabled | Initial value, repeated changes, programmatic `Set`, and callback/state synchronization |
| Slider | Current value plus necessary range, units, or context | Drag both directions, min/max clamp, increments, and displayed value |
| Dropdown | Current selection plus open/closed state | Open/close, select an item, long list, dismissal, and focus |
| Textbox | Label, meaningful placeholder, validation state | Type/paste, submit/cancel, rejected input, and mobile keyboard overlap |
| Tab/navigation | Current section clearly differs from available sections | Switch away/back, matching content, and preserved or reset state as intended |

Use one source of truth for value and rendered state. A successful input should update its control, underlying value, and callback exactly once. Keep event connections owned by the component/window; disconnect them and cancel obsolete tweens during teardown. Avoid orphaned overlays, popups outside the viewport, and stale page references.

## Verification ladder

Report each level separately; passing one does not imply the next.

1. **Static review:** check instance classes, parent hierarchy, visibility, constraints, clipping, Z-order, event connections, callbacks, and cleanup.
2. **Luau syntax:** use a Luau-aware parser, analyzer, or project tooling when available. A Lua 5.1 syntax check does not establish Luau compatibility or Roblox API availability.
3. **Runtime behavior:** start the real entry point in Studio/client, open and close the UI, exercise each control path, and inspect the output for errors or missing callbacks.
4. **Rendered appearance:** inspect the live interface at the intended viewport. Fix clipping, overlap, contrast, text scale, alignment, and empty/loading/error states based on what is visible.
5. **Device behavior:** if supported, use Studio's device tools for a narrow phone layout and each relevant input mode. Test on real hardware when hardware-specific behavior matters; simulator output is not hardware proof.

At minimum, check a desktop viewport and one narrow viewport when the product claims to support both. Exercise mouse and touch/gamepad paths that the game supports. If Studio/client is unavailable, stop at the evidence actually possible and state that runtime or visual behavior remains unverified.

## Official Roblox documentation

- [`GuiButton.Activated`](https://create.roblox.com/docs/reference/engine/classes/GuiButton/Activated) — mouse, touch, and console UI activation.
- [Implement designs in Studio](https://create.roblox.com/docs/tutorials/curriculums/user-interface-design/implement-designs-in-studio) — ScreenGui, organization, and device preview.
- [UI styling compatibility](https://create.roblox.com/docs/ui/styling/compatibility) — interaction between layouts and size constraints.
- [`UISizeConstraint`](https://create.roblox.com/docs/reference/engine/classes/UISizeConstraint) and [`UIAspectRatioConstraint`](https://create.roblox.com/docs/reference/engine/classes/UIAspectRatioConstraint) — size bounds and aspect ratio.
- [Device Simulator](https://create.roblox.com/docs/studio/device-simulator) — viewport size, touch, keyboard, and device input preview in Studio.
