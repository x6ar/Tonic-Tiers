# Tonic Tiers

A competitive Minecraft PvP tier-list leaderboard. Point-based rankings across 8 game modes (Vanilla, UHC, Pot, NethOP, SMP, Sword, Axe, Mace) with high/low tiers, retired-tier multipliers, and combat titles.

Pure HTML/CSS/JS — no build step, no dependencies, no backend.

## Project structure

```
tonic-tiers/
├── index.html      # the whole app — self-contained, open this directly
├── css/
│   └── styles.css  # same CSS as index.html, kept here for easier editing/diffing
├── js/
│   └── app.js       # same JS as index.html, kept here for easier editing/diffing
└── README.md
```

**`index.html` has everything inlined (CSS + JS) and does not depend on the `css/` or `js/` folders.** That's deliberate: if you save or share just that one file, it still works — no broken relative paths, no blank page. The `css/` and `js/` files are identical copies kept alongside for convenience if you'd rather edit styles/logic in separate files (e.g. with a linter, or splitting into a build step later) — just remember to keep `index.html` in sync if you edit those instead.

## Running it locally

Just open `index.html` in a browser — double-click it, no server required. If you want to serve it (closer to production, and required if you switch back to the split css/js files):

```bash
# from the project root
python3 -m http.server 8000
# then visit http://localhost:8000
```

Any static file server works (`npx serve`, VS Code's Live Server extension, etc).

## Deploying with GitHub Pages

1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to `Deploy from a branch`, pick your default branch and the `/ (root)` folder.
4. Save — your site will be live at `https://<username>.github.io/<repo>/` within a minute or two.

## How it works

- **Overall / per-mode rankings** — `Overall` shows every player ranked by total points; each other tab (Vanilla, UHC, etc.) shows a 5-column Tier 1–5 board split into HT (high tier) / LT (low tier) rows.
- **Point system** — points are computed live from each player's tier assignment:

  | Tier | HT points | LT points |
  |------|-----------|-----------|
  | 1    | 60        | 44        |
  | 2    | 28        | 16        |
  | 3    | 10        | 6         |
  | 4    | 3         | 2         |
  | 5    | 1         | 0.5       |

  Retired tiers count at 50% of their value.
- **Combat titles** are derived from a player's total points (Combat Rookie → Combat Grandmaster).
- **Admin mode** — click "Admin" in the header and enter the password to reveal the "Add Player" button and per-row delete controls. The password is `TonicTLAdmin-12`, set as `ADMIN_PASSWORD` near the top of `js/app.js`.

  ⚠️ **This is a client-side-only gate, not real authentication.** The password lives in plain text in `app.js`, which anyone can read via "View Source" or the browser dev tools. It's enough to stop casual clicking, but it will **not** stop someone who actually wants in. For real access control, put player-editing behind a server with proper auth (see the note below).
- **Data persistence** — player data is saved to the browser's `localStorage`, so it survives page reloads on the same device/browser. It is not shared across devices or users. Clear your browser storage (or open in a private window) to reset back to the seed data.

## Customizing

- Seed players live in `seedPlayers()` in `js/app.js`.
- Game modes, colors, and icons are defined in the `MODES` array in `js/app.js`.
- Point values and combat title thresholds are in `POINTS_HT` / `POINTS_LT` / `TITLES`.
- Colors, fonts, and layout are all in `css/styles.css` (see the `:root` custom properties at the top for the palette).

## Notes for production use

This is a front-end demo with no server. For a real leaderboard you'd want to swap `localStorage` for a real backend (database + API) so rankings are shared and admin actions are authenticated — the `loadPlayers()` / `savePlayers()` functions in `js/app.js` are the two places to change.
