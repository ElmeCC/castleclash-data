# castleclash-data

Public game data for [Album Trader](https://github.com/ElmeCC/album_trader) — no code, no
secrets, just album/card names, star ratings, ATK Speed buff tables, and the icon images they
use. This repo exists so the community can keep the data current (e.g. each new seasonal
album swap) without needing access to the main app's codebase.

The main app pulls this repo in as a git submodule and reads these files directly — a merged
PR here doesn't go live until the app is redeployed with the updated submodule pointer.

## How to contribute

1. Fork this repo, make your change, open a PR.
2. Keep JSON valid (a linter/CI check may reject malformed files).
3. Explain *why* in the PR description — e.g. "Season X update, verified in-game" or a
   screenshot for star-level corrections.

## Structure

```
album/
  album_data.json          — current season: 15 albums × 9 cards, {name, star}
  album_data_season_1.json — previous season, kept for reference
atkspeed/
  buffs.json                — every ATK Speed talent/insignia/pet/etc. level→% table
  images/                   — buff & pet icons referenced by buffs.json / the app
```

### `album/album_data.json`

One JSON object keyed by **album name** (the key IS the display name shown in the app), each
holding an array of exactly 9 cards:

```json
{
  "An Autumn Adventure": [
    { "name": "Letter from the Autumn Wind", "star": 1 },
    ...
  ]
}
```

- `star` is 1–5, matching the in-game rarity shown on the card.
- Keep the **order of albums** and **order of cards within an album** matching the in-game
  collection screen — the app pairs card `N` in album `M` positionally with slot `M-N`.
- When the game does a seasonal album swap, copy the current file to
  `album_data_season_<N>.json` first, then overwrite `album_data.json` with the new season.

### `atkspeed/buffs.json`

Each talent/insignia/pet entry is a **level → percentage** array, in order (index 0 = level
1). If the game rebalances a talent's percentages, update the array here. `icon` paths are
relative to `atkspeed/`.

### `atkspeed/images/`

PNG/JPEG icons, referenced by `buffs.json` (`icon` field) or directly by the app. Keep
filenames stable when possible — renaming means also updating `buffs.json` and the app.

## What does NOT belong here

No API keys, no user data, no Firebase config, no account info. If in doubt, ask before
opening a PR — this repo is public.
