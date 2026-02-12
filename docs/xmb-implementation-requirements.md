# XMB Implementation Requirements (Target Behavior + Feasibility)

## Target behavior requested

Desired UX at startup:
- Enter directly into an XMB-like shell.
- Dynamic animated background (PS3/PSP-style).
- Horizontal columns:
  - Home
  - Library
  - Store
  - Social
  - Media
  - Downloads
  - Settings
- Vertical options under each column (example: Library -> Installed/Favorites/Non-Steam).
- Selecting a vertical item should **not** hard-jump to separate Steam pages.
  - Instead, keep the XMB shell and shift content left/right with live item lists.
- Include a bottom item to open the full native page (example: full Library page).

## What CSS Loader can and cannot do

### Confirmed capabilities
- CSS Loader themes are CSS + `theme.json` injections into Steam tabs.
- Patches can toggle stylesheets (dropdown/checkbox/slider).
- CEF debugger is the intended workflow for selector discovery and live styling tests.

### Hard limits for requested behavior
The requested “live XMB shell with in-place data panes + custom navigation behavior” goes beyond CSS-only theming.

Why:
1. CSS changes presentation, not application routing/state logic.
2. Building “Library sub-tab opens in XMB canvas instead of dedicated page” requires JS-level control of navigation and rendering.
3. Dynamic per-column content panes require data + event wiring from Steam UI internals.

## What this means architecturally

To achieve the requested UX, we need two layers:

1. **Theme layer (existing repo scope):**
   - CSS visual/structural treatment.
   - Optional effect/focus/contrast/palette patches.
   - Experimental left-rail layout.

2. **Behavior layer (new workstream, Decky plugin or injected JS app):**
   - Own horizontal XMB category model.
   - Own vertical menu state machine.
   - Intercept/select actions and render in-shell panes.
   - Bridge to Steam data sources for installed/favorites/non-Steam/etc.
   - Offer explicit “open native page” action.

## Requirements for the behavior layer (must-have)

### A) Navigation controller
- Controller-first nav graph for 7 columns.
- Deterministic focus movement (left/right column, up/down item, select/back).
- Focus-safe behavior when DOM elements are hidden/repositioned.

### B) Data adapters
- Adapter for library groupings (Installed, Favorites, Non-Steam).
- Adapter for store/social/download/settings summaries.
- Caching + refresh policy to avoid stutter.

### C) XMB pane renderer
- Main canvas component that renders selected column/item content in-place.
- Animated transitions (slide-left/slide-right) matching XMB intent.
- Fallback card when data provider fails.

### D) Native handoff actions
- Each column has bottom “Open full <section>” action.
- Opens native Steam page and returns back cleanly.

### E) Safety/resilience
- Feature flag to disable behavior layer and return to stock.
- Version guards for Steam updates.
- Crash-safe no-op fallback.

## Requirements for the CSS/theme layer (this repo)

1. Preserve conservative baseline in `shared.css`.
2. Keep high-risk style behavior in optional patches.
3. Use manifest v9 + nav patching support when hiding/reflowing focusable elements.
4. Maintain QuickAccess/MainMenu readability independent from bigpicture layout experiments.

## Delivery plan

### Phase 1 (done in this repo)
- Visual language + optional patches + experimental structural reflow.

### Phase 2 (next)
- Build proof-of-concept behavior layer with one column fully functional (Library).
- Implement in-shell rendering for Installed/Favorites/Non-Steam.
- Add “Open full Library page” at bottom.

### Phase 3
- Expand to Home/Store/Social/Media/Downloads/Settings.
- Polish transitions and controller affordances.
- Add update-compatibility watchdog + fallbacks.

## Open technical questions to answer before Phase 2 build

1. Which Steam internal APIs/stores are stable enough for Library subsets?
2. Should behavior layer run as a Decky plugin frontend overlay vs direct Steam UI injection?
3. How to handle anti-regression tests across Steam client updates?
4. Should startup enter XMB shell via auto-opened panel, or replace home layout directly?

## Notes from local codebase inspection

- Steam client UI bundles in `steamui_copy` are webpack chunks and include React usage, indicating behavior changes require JS-level intervention rather than CSS only.

## Evidence references

- DeckThemes CSS Loader index: https://raw.githubusercontent.com/DeckThemes/DeckThemes-Docs/mkdocs/docs/CSSLoader/index.md
- DeckThemes CSS Loader features: https://raw.githubusercontent.com/DeckThemes/DeckThemes-Docs/mkdocs/docs/CSSLoader/Features.md
- DeckThemes CEF debugger guide: https://raw.githubusercontent.com/DeckThemes/DeckThemes-Docs/mkdocs/docs/CSSLoader/Cef_Debugger.md
- Local Steam UI bundle sample: `steamui_copy/sp.js` (webpack chunk + React hook usage)
