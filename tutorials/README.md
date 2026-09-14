# tutorials/

Community-written guides shown in the app's **Tutorials** tab. Each tutorial is a folder
containing one `data.json` (its text/images) plus its own image files — no code changes
needed to add, edit, or remove one.

## Structure

```
tutorials/
  index.json              — ordered list of tutorials shown on the Tutorials tab
  getting-started/
    data.json             — the tutorial's title + content blocks
    some-screenshot.png    — any images the blocks reference (flat, next to data.json)
  another-tutorial/
    data.json
    ...
```

## `index.json`

An array, in display order, of every tutorial that should appear on the Tutorials tab:

```json
[
  { "id": "getting-started", "title": "Getting Started", "icon": "📖" }
]
```

- `id` — must match the tutorial's folder name exactly.
- `title` — shown under the icon on the Tutorials tab.
- `icon` — **either** an emoji (shown directly, like above) **or** an image filename that
  lives in that tutorial's own folder (e.g. `"icon.png"` → renders
  `tutorials/<id>/icon.png`). Use a roughly square image if you go this route.

## `<id>/data.json`

```json
{
  "title": "Getting Started",
  "blocks": [
    { "type": "heading", "text": "1. Do the thing" },
    { "type": "text", "text": "Plain paragraph text. One block per paragraph." },
    { "type": "image", "src": "step1.png", "caption": "Optional caption under the image" }
  ]
}
```

Block types:
- `heading` — a section title inside the tutorial.
- `text` — a paragraph.
- `image` — `src` is a filename in the same folder as `data.json` (not a full path);
  `caption` is optional.

Blocks render top to bottom in the order you list them — that's the whole layout.

## How to contribute

1. Adding a new tutorial: create a new folder under `tutorials/` with a short kebab-case
   name (e.g. `hero-priority-basics/`), add a `data.json` inside it, drop in any images it
   references, then add one entry for it to `index.json`.
2. Editing an existing tutorial: just edit its `data.json` (or replace/add an image).
3. Keep `data.json` valid JSON (a linter/CI check may reject malformed files).
4. Fork this repo, make your change, open a PR — explain what the tutorial covers or what
   you fixed.

As with the rest of this repo: a merged PR here doesn't go live until the main app is
redeployed with the updated submodule pointer.
