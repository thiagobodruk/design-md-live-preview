# DESIGN.md Live Preview

**[Live demo →](https://thiagobodruk.github.io/design-md-live-preview/)**

A single-page tool that turns a `DESIGN.md` file into a live UI preview. Paste or upload your design tokens on one side and watch a mock landing page — buttons, cards, form, badges — restyle itself in real time on the other.

No build step, no dependencies. It's one static `index.html` file.

## Features

- **Live editor** — paste, upload, or drag-and-drop a `.md` file; the preview updates on every keystroke.
- **Tolerant parser** — reads color and typography tokens from Markdown tables, CSS custom-property blocks, Markdown lists, or (as a fallback) any `#hex` value found near a label in the document.
- **Semantic role mapping** — heuristically maps token names (`primary`, `background`, `border`, `success`, ...) to the roles used in the mockup, so most real-world `DESIGN.md` files work without any special formatting.
- **Real font loading** — font families mentioned in the doc are loaded live from Google Fonts, so typography specimens render in the actual typeface.
- **Detected-tokens panel** — every parsed color and font is listed with its role, for a quick sanity check of what got picked up.
- **Local persistence** — your last-edited content is kept in the browser's `localStorage`, so refreshing doesn't lose your work.
- **Light/dark theme** — the app shell follows your OS preference by default; the toggle in the top-right corner pins an explicit choice, remembered across visits.
- **Collapsible editor** — the panel icon next to the theme toggle hides the editor so the preview fills the whole screen; the choice is remembered across visits.
- **Grid, spacing and icon tokens** — also detects a column grid (`--grid-columns` / `--grid-gutter` / `--grid-margin` / `--grid-max-width`), a spacing scale (`--space-*`), and icon library tokens (a library link, `--icon-*` sizes, a weight/style table), each rendered as its own visual preview.
- **Real icon glyphs, not one generic placeholder** — recognizes Phosphor, Lucide, Feather, Tabler, Heroicons and Font Awesome by name and renders that library's actual icon shape (real path data, no network fetch); an unrecognized library falls back to a generic star so nothing ever looks broken.
- **Handles messy or partial files** — every section degrades to a plain "nothing detected here" note instead of breaking when a doc only defines some of these; an actual parsing crash shows an error panel (with the underlying message) instead of a blank or frozen preview.

## Usage

Open `index.html` in any modern browser, or use the [live demo](https://thiagobodruk.github.io/design-md-live-preview/) served from this repo via GitHub Pages.

For local development with a proper HTTP origin (needed for some browser features like clipboard access):

```bash
python3 -m http.server 4173
```

Then open `http://localhost:4173`.

## Supported `DESIGN.md` formats

The parser looks for color tokens in this order of priority, merging whatever it finds:

1. **Markdown tables**, e.g.:
   ```
   | Token | Hex | Role |
   |---|---|---|
   | primary | #4F46E5 | Primary actions, links |
   ```
2. **CSS custom-property blocks**:
   ````
   ```css
   :root { --primary: #4F46E5; }
   ```
   ````
3. **Markdown lists**: `- primary: #4F46E5`
4. **Fallback scan** — any `#hex` value found near a short label, used only when fewer than 3 colors were found above.

Typography works the same way, reading `font-family` / `size` / `weight` from a table or a `font-family: ...` declaration. A `radius: 12px` (or `raio` in Portuguese source docs) mention anywhere sets the mockup's corner radius.

Grid, spacing and icons follow the same table-or-CSS-block convention:

- **Grid**: `--grid-columns`, `--grid-gutter`, `--grid-margin`, `--grid-max-width` in a CSS block, or just a "12 columns" mention in prose.
- **Spacing**: `--space-1`, `--space-2`, … in a CSS block, or a table under a heading containing "spacing"/"espaçamento".
- **Icons**: a library name+link near an "Icons"/"Ícones" heading, `--icon-sm` / `--icon-md` / `--icon-lg` (or any `--icon-*`) for sizes, and any table whose token column contains "icon" for weights/styles.

## How it's built

Everything lives in `index.html`: inline CSS for the app shell and the token-driven preview, and vanilla JavaScript for the parser, role-assignment heuristics, and rendering. Google Fonts are loaded dynamically as fonts are detected in the document.

## License

MIT — see [LICENSE](LICENSE).

---

Created by [Thiago Bodruk](https://github.com/thiagobodruk).
