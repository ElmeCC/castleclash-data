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
  structure.json            — the shape: 15 albums × 9 cards, emoji + star rating, no names
  en.json                    — English album/card names, positionally keyed
  fr.json                    — French album/card names, same keys, community-filled
  album_data_season_1.json  — previous season's old-format data, kept for reference
atkspeed/
  buffs.json                — every ATK Speed talent/insignia/pet/etc. level→% table
  images/                   — buff & pet icons referenced by buffs.json / the app
```

Names and structure are split on purpose: **structure.json never needs translating**, and
**en.json / fr.json never need to worry about star ratings or album order** — a translator
only ever touches the language files.

### `album/structure.json`

One object keyed `ALBUM_1` … `ALBUM_15` (in in-game collection order), each with an `emoji`
and its 9 cards in order, `CARD_1` … `CARD_9`, each with a `star` rating (1–5):

```json
{
  "ALBUM_1": {
    "emoji": "🍂",
    "cards": [
      { "id": "CARD_1", "star": 1 },
      ...
    ]
  }
}
```

Edit this file for a seasonal album swap (order/count/star changes) or a rarity correction —
never for a name change, that belongs in `en.json`.

### `album/en.json` and `album/fr.json`

Same shape, one per language, keyed by the same `ALBUM_N` / `CARD_N` positional IDs as
`structure.json` — **not** by name, so renaming a card doesn't break its translation:

```json
{
  "ALBUM_1": {
    "name": "An Autumn Adventure",
    "cards": {
      "CARD_1": "Letter from the Autumn Wind",
      ...
    }
  }
}
```

- To translate: open `fr.json`, find the same `ALBUM_N` / `CARD_N` key you see filled in
  `en.json`, and fill in the matching French value.
- Leave a value as `""` (empty string) if you don't have a translation yet — the app falls
  back to English automatically. Partial PRs (a handful of albums, not all 135 cards) are
  welcome.
- When a seasonal swap changes `structure.json`'s card count or order, `en.json` gets updated
  to match (new English names at those positions) — `fr.json` entries at those same
  positions become stale and should be re-translated, but nothing breaks in the meantime
  since a stale/missing translation just falls back to English.

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
