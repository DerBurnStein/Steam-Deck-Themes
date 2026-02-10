XMBish (Steam Deck Game Mode theme)
==================================

This repo contains a starter CSS Loader theme for Steam Deck (SteamOS 3 Game Mode).  It is a foundation for an XMB-style reskin.

Folder layout
-------------
- XMBish/
  - theme.json
  - shared.css

How to install on Steam Deck
----------------------------
1. Install Decky Loader.
2. Install CSS Loader plugin from Decky store.
3. Copy the `XMBish` folder to:
   `/home/deck/homebrew/themes/`
4. Open Quick Access Menu -> Decky -> CSS Loader -> enable the theme.
5. Use the CSS Loader refresh button after edits.

Development workflow
--------------------
- Enable remote CEF debugging so you can inspect the live DOM and grab stable selectors.
- Make small styling changes first.  Avoid big layout changes until you have a stable map of selectors.

Next steps for us
-----------------
- Capture outerHTML for:
  - Home screen root container
  - Top nav container (Home / Library / Store)
  - A typical vertical list item container
Then we will add:
- A wave-like background overlay
- Crossbar spacing, focus highlights, and icon treatment
- List highlight bar styling
