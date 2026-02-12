# Phase 2 Start: Library Behavior Prototype Scope

This document begins implementation planning for the behavior layer requested by the user.

## Objective
Build the first interactive XMB column (`Library`) with in-shell vertical items:
- Installed
- Favorites
- Non-Steam
- Open Full Library

## What starts now
1. **Visual shell scaffold in CSS** (`xmb-shell-overlay.css`) to make the target structure obvious during iteration.
2. **Behavior API contract draft** for later JS/plugin implementation.

## Behavior API contract (draft)

### Inputs
- `activeColumn`: one of `home|library|store|social|media|downloads|settings`
- `activeLibraryNode`: one of `installed|favorites|nonsteam|open_full`
- `libraryItems`: list from Steam source adapters

### Actions
- `xmb.selectColumn(column)`
- `xmb.selectNode(node)`
- `xmb.openNative(section)`
- `xmb.back()`

### Outputs
- Rendered pane content list for current node.
- Transition state (`slide-left` / `slide-right`).

## Integration assumptions
- CSS Loader alone cannot implement this logic; JS behavior layer is required.
- Candidate implementation paths:
  1. Decky plugin frontend overlay (recommended for maintainability)
  2. Steam UI JS injection (higher break risk)

## Exit criteria for this prototype phase
- Controller navigation works across `Library` vertical nodes.
- Selecting `Installed/Favorites/Non-Steam` updates in-shell list content.
- Selecting `Open Full Library` opens native full library view.
- Back returns to XMB shell without losing state.
