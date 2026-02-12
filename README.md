XMBish (Steam Deck Game Mode theme)
==================================

This repo contains a CSS Loader theme for Steam Deck (SteamOS 3 Game Mode). It is focused on an XMB-style reskin that keeps the stock layout semantics intact.

Folder layout
-------------
- XMBish/
  - theme.json
  - shared.css
  - layout-xmb-structure.css
  - xmb-shell-overlay.css
  - xmb-anchors.css
  - xmb-runtime-layout.css
  - xmb-columns-hard.css
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
- `layout-xmb-structure.css` is the legacy class-fragment structural attempt (kept for fallback testing).
  - attempts to pin recent/shelf/carousel content into a persistent left rail and shifts core content to a right-side canvas.
- `theme.json` uses manifest v9 + `REQUIRE_NAV_PATCH` and tab mappings (`xmb_core`, `xmb_overlay`) for safer multi-tab injection and hidden-focus navigation fixes.
- Optional patches in `theme.json`:
  - **XMB Structural Layout (Experimental)**: older class-fragment layout reflow (`layout-xmb-structure.css`, now default off).
  - **XMB Shell Overlay Prototype**: explicit top categories + Library vertical scaffold to mirror target IA (`xmb-shell-overlay.css`).
  - **Runtime Anchor Overrides**: strong styling against real runtime anchors (`#GamepadUI_Full_Root`, `#header`, `#Main`, `#Footer`) and tab classes (`xmb-anchors.css`).
  - **XMB Runtime Layout (ID Anchored)**: ID-based left-rail + right-canvas reflow using `#Main` and `#header` (`xmb-runtime-layout.css`).
  - **XMB Hard Column Mode**: aggressive column transformation that suppresses native tab rows and forces carousel-to-column behavior (`xmb-columns-hard.css`, default on).
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
- Behavior-layer requirements for full XMB interaction model: `docs/xmb-implementation-requirements.md`
- Behavior layer phase-2 kickoff (Library prototype scope): `docs/behavior-layer/phase2-library-prototype.md`
- Runtime anchor notes from Big Picture DOM analysis: `docs/bp-runtime-notes.md`


CSS completion status
---------------------
- Baseline implementation is complete for this phase: structure, effects, focus, contrast, and palette variants are now configurable via `theme.json` patches.
- Remaining work is selector hardening against live CEF captures on-device (especially for `layout-xmb-structure.css`) to reduce break risk across Steam updates.


Runtime snapshot note
---------------------
- If `docs/bp.html` is present locally on your machine, use it to re-validate hashed class selectors before each Steam client update.
