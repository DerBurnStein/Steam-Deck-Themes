# Big Picture Runtime Anchor Notes (for XMBish)

This note captures the runtime-focused selectors we should prioritize for dramatic, stable theming changes.

## High-value anchors

- `#GamepadUI_Full_Root`
  - root container for game mode rendering.
  - expected background class on current build: `b084mAvY8rgVMr3H04g9g`.
- `#header`
- `#Main`
- `#Footer`

These IDs are preferred over broad generic selectors for strong visual transformations.

## Known tab/focus primitives

- Tab role selector: `[role="tab"]`
- Runtime tab class observed in analysis: `._3eEbSktrstBdLk0dVpnKVI`
- Selected tab class: `._3Gp1bACHx__POxmy6Gd3kG`
- Focus classes: `.gpfocus`, `.gpfocuswithin`

## Why root-anchored background overrides matter

Steam Big Picture paints a background surface on runtime root layers. Styling only `body` can look muted if the root skin remains opaque.

So for strong XMB impact:
1. neutralize root skin class where present,
2. render wave/gradient background as pseudo-elements on `#GamepadUI_Full_Root`,
3. restyle `#header`/`#Footer` translucency to maintain shell coherence.

## Applied in current theme

`XMBish/xmb-anchors.css` now implements this anchor-first strategy:
- root skin neutralization (`#GamepadUI_Full_Root.b084mAvY8rgVMr3H04g9g`),
- wave background via `#GamepadUI_Full_Root::before/::after`,
- translucent header/footer,
- stronger focus bars,
- runtime tab class styling.

## Compatibility note

Hashed class names may shift after Steam updates. Keep fallback role/ID selectors in place and re-capture runtime DOM periodically.
