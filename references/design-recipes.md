# Visual patterns and adaptable recipes

This guide distills four Luau UI code references supplied for this project. They were source files, not rendered screenshots. The recorded colors, dimensions, and component structures describe what the code specifies; they do not prove how it looks or behaves at runtime. The original scripts are not included here.

## Findings from the reference set

| Interface type | Useful pattern in the code | Keep this boundary in mind |
| --- | --- | --- |
| Branded launcher | Strong title and primary launch action; secondary community links grouped apart; very dark shell, subtle border, dimmed background art. | The fixed positions and social-link column belong to that launcher. Rebuild the composition for the real content and viewport. |
| Large themed window | Central theme roles; a top bar, vertical navigation, scrollable content, differentiated title/body text, subtle selected and hover states. Example neutral palette: `#101010`, `#181818`, `#F0F0F0`, `#96969B`, with white as accent; 16 px and 8 px corner radii. | Its 605 × 405 starting size and other library constants are not universal design rules. Keep contrast when using translucent surfaces. |
| Compact settings menu | Distinct sidebar and content pane; controls aligned consistently; one lavender accent `#9B96DB` over `#0E0E10` and `#151517` surfaces; 196 px sidebar in the inspected layout. | Its 3 px button radius and remotely loaded fonts are implementation choices. Do not make a UI depend on font downloads to open. |
| Reusable widget library | Named theme roles for background, element, border, text, icon, tab, hover, and accent; examples of toggles, sliders, dropdowns, search, and keybind state. Example preset: `#181A1E` background, `#262632` element, white text, `#4E5FFF` accent. | A complete preset is still a preset. Select a palette and controls for the actual game's identity, content density, and devices. |

The samples lean toward dark palettes. Their strongest shared ideas are readable hierarchy, recognizable navigation, a clear primary action, aligned control labels and values, and feedback when state changes. Preserve those relationships when using a light, colorful, playful, or world-inspired direction.

## Three composition recipes

These are starting structures, not templates. Change the visual direction and layout when the product calls for it.

### 1. Launcher or short entry flow

Use when one primary action starts a short user journey.

```text
Product shell
├── Product name and useful subtitle
├── Context area: selection, real update notes, or concise explanation
└── Action area
    ├── Primary action with a loading/result state
    └── Lower-weight secondary links or actions
```

Give the title, the reason for the action, and the primary button the strongest hierarchy. Keep secondary links visually quieter. Use background art only when a real relevant asset exists, and keep it dim enough that text and controls remain clear. A launcher does not need a social column, changelog, and cards just because one reference has them.

### 2. Multi-section utility window

Use when people switch among stable content categories.

```text
Window
├── Top bar: title, useful status, window actions
├── Navigation: visible current section and clear alternatives
└── Scrollable page
    ├── Named group of related controls
    ├── Rows or a grid only when the content benefits from one
    └── Honest empty, loading, or error state
```

One inspected window uses a 52 px top bar, 34 px tab buttons, and a 130 px navigation rail. Another uses a 196 px sidebar. These measurements can inform a similar desktop tool, but measure against the chosen viewport, real labels, and actual content. Keep related controls together; use a divider instead of a new card when a row needs only a little separation.

### 3. Compact settings panel

Use when people scan and adjust many values in sequence.

```text
Named section
├── Toggle: effect label ................................ state
├── Slider: effect label + current value
│   └── track, fill, draggable thumb
├── Selector: effect label .............................. current value
└── Text field: label, useful placeholder, validation
```

The larger component reference uses rows around 44 px high without a description and 56 px with one, a 40 × 22 px toggle, and a slider with a visible value, slim track, and distinct thumb. Use these as density examples, not hard limits. Allow more height for translated text, wrapping, context, or touch targets. Align each label and value to a consistent axis, and make the active state visible without hover.

## Token and state system

Define a small role-based palette before styling individual instances. Useful roles are `Background`, `Surface`, `SurfaceRaised`, `Text`, `TextMuted`, `Border`, `Accent`, `Success`, `Warning`, and `Danger`; omit roles the screen does not need. Keep each color system together and check contrast after applying transparency.

The palettes above come from different sources; do not combine their values blindly. Keep the accent for selection, progress, or a primary action. Reserve success and danger colors for meaning, not decoration. If the game already has a strong visual palette, derive a compatible UI system from it instead of defaulting to blue or purple.

Plan distinct states for normal, hover (pointer only), pressed, selected, disabled, loading, success, and error as needed. Use a combination of surface, text, and a state indicator; never make hue or motion the only signal. The references use roughly 0.12–0.15 s for hover and up to about 0.3 s for broader transitions. Keep fast actions immediate and make motion easy to reduce or omit when it distracts.

## Anti-template review

- If replacing the logo would make the result fit any game, add one visual decision grounded in the actual product or content.
- If every row has a card, remove containers that do not establish a real grouping.
- If the accent appears in every label, icon, border, fill, and glow, reduce it to a clear hierarchy.
- If copy adds no information, remove it. Do not invent stats, users, version numbers, or activity to fill a blank region.
- If an icon has no real asset, use a clear label or a simple native shape rather than an emoji or arbitrary character.
- If an image, texture, blur, gradient, shadow, or animation does not improve identity or usability, remove it.
- If the desktop layout only works at one fixed size, redesign the hierarchy for narrow screens instead of scaling the whole panel down.
