# Steam Deck UI Frontend Research (Pre-XMB Implementation)

## Goal
Understand how SteamOS 3 Game Mode frontend is built and where CSS Loader theming can safely alter it before implementing an XMB-style interface.

## What the local `steamui_copy` dump tells us

1. **Steam UI is shipped as webpack chunk bundles.**
   - Files are structured as chunks that register into `self.webpackChunksteamui`.
   - Example: `sp.js` starts with `self.webpackChunksteamui...push([[3714], ...`.

2. **The UI code is React-based and minified/bundled for production.**
   - `sp.js` contains React usage (`memo`, `useEffect`, `useState`, `createElement`), indicating the Game Mode frontend is React-driven in production bundles.

3. **Valve uses CSS-module-style class names for many components.**
   - Example in `gamerecording.js`: exported class names map to hashed tokens like `"_1fyr-hKRG1lR-7oPJ_rqmG"`.
   - This means selectors can be unstable between Steam client updates unless anchored to stable container patterns.

4. **Build artifacts include sourcemap references from Steam build paths.**
   - Example: `//# sourceMappingURL=file:///home/buildbot/.../steamui/sourcemaps/sp.js.map` in `sp.js`.

## Online findings

### 1) CEF debugger is the primary inspection workflow for Steam Deck theming
DeckThemes docs explicitly describe Steam UI theming through CEF remote debugging:
- Enable **Allow Remote CEF Debugging** in Decky settings.
- Inspect tabs remotely from a Chromium browser via `{DECK_IP}:8081`.
- Important tabs include `Steam Big Picture Mode`, `QuickAccess`, `MainMenu`.

Source:
- https://raw.githubusercontent.com/DeckThemes/DeckThemes-Docs/mkdocs/docs/CSSLoader/Cef_Debugger.md

### 2) CSS Loader injects CSS per tab/window context
DeckThemes CSS Loader docs describe `theme.json` as an injection manifest:
- Themes live in `~/homebrew/themes/...`.
- `inject` maps CSS files to target tabs.
- Injection can target exact tab names, regex-like matches (e.g. `QuickAccess.*`), and URL fragment matches wrapped in `~...~`.

Sources:
- https://raw.githubusercontent.com/DeckThemes/DeckThemes-Docs/mkdocs/docs/CSSLoader/index.md
- https://raw.githubusercontent.com/DeckThemes/DeckThemes-Docs/mkdocs/docs/CSSLoader/Features.md

### 3) The practical theming model is “inspect in CEF, then codify CSS patches”
DeckThemes step-by-step guidance aligns to:
1. attach CEF debugger,
2. identify/select runtime DOM element(s),
3. test CSS live,
4. copy to `shared.css`,
5. refresh CSS Loader.

This reinforces that Steam Deck UI customization is not plugin-level React source edits for normal theme work; it is CSS injection against a running web UI.

Source:
- https://raw.githubusercontent.com/DeckThemes/DeckThemes-Docs/mkdocs/docs/CSSLoader/theming_step_by_step.md

## Implications for XMB-style implementation

1. **Use a layered approach instead of one-shot full replacement.**
   - Phase 1: visual language (background gradients/waves, typography scale, focus states).
   - Phase 2: navigation rhythm (horizontal category bars, vertical item cadence) by re-skinning existing layout semantics.
   - Phase 3: advanced illusions (cross-media-bar feel) via pseudo-elements/overlays.

2. **Prefer stable selectors first.**
   - Anchor styling to persistent shell containers and known tab contexts (`bigpicture`, `QuickAccess`, `MainMenu`).
   - Treat deeply hashed component selectors as fragile and isolate them in optional patches.

3. **Build for update resilience.**
   - Keep a core stylesheet with conservative selectors.
   - Put risky/high-specificity overrides into separate patch files so breakage can be toggled quickly after Steam updates.

4. **Define strict acceptance checks before broad restyling.**
   - Home, Library, Store each remain navigable with controller.
   - Focus highlight remains obvious at 10-foot distance.
   - QuickAccess/MainMenu overlays remain readable and unclipped.

## Proposed next step (implementation kickoff)
Create a first XMB baseline theme pass with:
- shared background + color tokens,
- top-nav crossbar treatment,
- list/focus highlight restyle,
- no structural reflow yet.

Then validate in CEF debugger against the three core contexts:
- Big Picture root,
- top nav,
- representative game/app tile row.
