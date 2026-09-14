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
  album_data_fr.json       — French translation of album/card names, keyed off the English names
atkspeed/
  buffs.json                — every ATK Speed talent/insignia/pet/etc. level→% table
  images/                   — buff & pet icons referenced by buffs.json / the app
tutorials/
  index.json                — ordered list of tutorials shown on the Tutorials tab
  <tutorial-id>/            — one folder per tutorial: data.json + its own images
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

### `album/album_data_fr.json`

The French translation, **keyed off the English names** in `album_data.json` (not by
position) — so a missing or not-yet-translated entry safely falls back to English instead of
showing the wrong card. Two top-level sections:

```json
{
  "albums": {
    "An Autumn Adventure": "Une Aventure Automnale"
  },
  "cards": {
    "An Autumn Adventure": {
      "Letter from the Autumn Wind": "Lettre du vent d'automne"
    }
  }
}
```

- `albums.<English album name>` → the French album name.
- `cards.<English album name>.<English card name>` → the French card name.
- Leave a value as `""` (empty string) if you don't have a translation yet — the app falls
  back to the English name automatically. Partial PRs (a handful of albums, not all 135
  cards) are welcome.
- If `album_data.json` changes (new season, renamed card), the **English key must match
  exactly** for the translation to be picked up — a stale key just falls back to English
  rather than breaking anything.

### `atkspeed/buffs.json`

Each talent/insignia/pet entry is a **level → percentage** array, in order (index 0 = level
1). If the game rebalances a talent's percentages, update the array here. `icon` paths are
relative to `atkspeed/`.

### `atkspeed/images/`

PNG/JPEG icons, referenced by `buffs.json` (`icon` field) or directly by the app. Keep
filenames stable when possible — renaming means also updating `buffs.json` and the app.

### `tutorials/`

Community-written guides for the app's Tutorials tab — one folder per tutorial, each with a
`data.json` (title + content blocks) and its own images. See
[`tutorials/README.md`](tutorials/README.md) for the full format and how to add a new one.

## What does NOT belong here

No API keys, no user data, no Firebase config, no account info. If in doubt, ask before
opening a PR — this repo is public.
