XMBish (Steam Deck Game Mode theme)
==================================

This repo contains a CSS Loader theme for Steam Deck (SteamOS 3 Game Mode). It is focused on an XMB-style reskin that keeps the stock layout semantics intact.

Folder layout
-------------
- XMBish/
  - theme.json
  - shared.css
  - effects-wave.css
  - focus-strong.css
  - crossbar-emphasis.css
  - accessibility-contrast.css
  - palette-teal.css
  - palette-purple.css
  - palette-monochrome.css

How to install on Steam Deck
----------------------------
1. Install Decky Loader.
2. Install CSS Loader plugin from Decky store.
3. Copy the `XMBish` folder to:
   `/home/deck/homebrew/themes/`
4. Open Quick Access Menu -> Decky -> CSS Loader -> enable the theme.
5. Use CSS Loader refresh after edits.

Current implementation
----------------------
- `shared.css` is the stable baseline:
  - blue gradient shell, crossbar-like top tabs, flatter controls, and readable overlay menus.
- Optional patches in `theme.json`:
  - **Background Effects**: enables/disables wave/glow overlays (`effects-wave.css`).
  - **Strong Focus Highlight**: enables/disables high-visibility focus affordances (`focus-strong.css`).
  - **Crossbar Emphasis**: stronger tab underline/list rhythm treatment (`crossbar-emphasis.css`).
  - **High Contrast UI**: readability-first text and panel contrast (`accessibility-contrast.css`).
  - **Color Palette** (dropdown): Ocean Blue (default), Teal, Purple, Monochrome.

Development workflow
--------------------
- Enable remote CEF debugging and inspect live selectors.
- Keep stable/default selectors in `shared.css`.
- Put risky or high-intensity visuals into optional patch files.

Next steps for us
-----------------
- Capture outerHTML for:
  - Home screen root container
  - Top nav container (Home / Library / Store)
  - A typical vertical list item container
- Replace fragile class-fragment selectors with captured stable anchors where possible.

Research notes
--------------
- Frontend architecture + theming workflow research: `docs/steamdeck-ui-frontend-research.md`


CSS completion status
---------------------
- Baseline implementation is complete for this phase: structure, effects, focus, contrast, and palette variants are now configurable via `theme.json` patches.
- Remaining work is selector hardening against live CEF captures on-device (post-implementation tuning, not missing feature coverage).
