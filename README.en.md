<!-- i18n: language-switcher -->
[English](README.en.md) | [日本語](README.md)

# Ayame — Design Proposal Gallery

A redesign of the current UI appearance. The goals are **beautiful, lightweight, modern, hackable, and exciting for engineers**.
This is a pure visual A/B comparison, showing the same screen (same toolbar / same 1.36 GiB TSV data / same search state) with different appearances.

> ## ★ Latest Direction (light only) — Iris Light + JSON Theme
> Based on feedback, we’ve consolidated to **light-only**: the calmness of iris × **slight transparency (acrylic) in Fluent style** ×
> An **asymmetric** layout. No flower illustrations; the left side features **the essence of printmaking (cutouts / pochoir)** —
> Flat cutout shapes reminiscent of Matisse’s *Jazz* — in iris purple with subtle green and gold accents. The default theme is **Sumi Light**.
> Themes can be **defined in JSON**, edited within the app → applied → **saved locally (localStorage)** → exported.
> - Design + theme editing: [`gui/iris-light.html`](./gui/iris-light.html) (supports `?theme=iris-mist` / `?panel=0`)
> - Theme definitions and schemas: [`themes/`](./themes/) (`iris-light` / `iris-mist` / `iris-dawn` / `sumi-light` / `mono-paper`)
> - Submenus can be **collapsed** / background can switch between **watercolor / solid color** (including **Mono Paper** with all solid color considerations) / illustration quantity adjustable
> - Rendering: `renders/iris-*.png`
>
> The following 18 proposals are records of color and layout explorations leading to this direction.

## How to View

For quick comparison, use the static catalog centered on PNGs: [`design-list.html`](./design-list.html) and the overview table [`DESIGN_LIST.md`](./DESIGN_LIST.md).

Opening `design/index.html` in a browser displays a live thumbnail list of all proposals (click to view full size).

```sh
# Any is fine. No build or server needed (all proposals are a single self-contained HTML)
xdg-open design/index.html        # Linux
open design/index.html            # macOS
# Or you can serve it with your existing ayame serve
```

For individual viewing, just open `design/mockups/NN-*.html` directly. **No JavaScript build or external assets needed**, works offline.

## 18 Proposals (by Category)

### Japanese
| # | Name | Brief Description |
|---|-------|-------------------|
| 01 | **Ayame Zen** | Deep navy night × iris colors × gold leaf. The main feature is text floating on Japanese paper texture. |
| 02 | **Kanagawa 神奈川** | Hokusai’s Mount Fuji. Calm ukiyo-e palette with indigo, violet, and a hint of seigaiha wave pattern. |
| 03 | **Sumi-e Ink Painting** | Monochrome ink. Only a touch of vermilion for the seal. Minimalist with white space and tranquility. |

### Neon & Hacker
| # | Name | Brief Description |
|---|-------|-------------------|
| 04 | **Neon Glass Terminal** | Frosted chrome reflecting neon city lights. Line numbers shift from magenta to cyan, hits glow. |
| 05 | **Synthwave '84** | Outrun sunset. Horizon with perspective grid and glowing text. |
| 06 | **Matrix Phosphor** | Pitch black with phosphorescent green. Digital rain, scan lines, blinking cursor. Straightforward hacker vibe. |

### Dev Classics (Popular among developers)
| # | Name | Brief Description |
|---|-------|-------------------|
| 07 | **Tokyo Night** | Most beloved night theme. Elegant blue and purple palette. |
| 08 | **Catppuccin Mocha** | Warm pastel tones. Rounded, soft appearance. |
| 09 | **Rosé Pine Moon** | Elegant night with iris, rose, and gold accents. |

### Calm · Light · Retro
| # | Name | Brief Description |
|---|-------|-------------------|
| 10 | **Nord Frost** | Arctic twilight. Cold slate with a single ice blue accent. |
| 11 | **Gruvbox** | Warm retro. Amber and orange, with subtle film grain. |
| 12 | **Alabaster Light** | Bright white paper. **Beautiful even when bright** — a high-quality light theme. |
| 15 | **Amber CRT** | Nostalgic VT220. Amber phosphor glow, scan lines, curved glass vignette. |

### Modern & Experimental
| # | Name | Brief Description |
|---|-------|-------------------|
| 13 | **Frostbite Wireframe** | Minimalist in the line-art class. Very fine grid, icy white text, silver shimmer. |
| 14 | **Aurora Silk Glass** | Aurora slowly breathes behind frosted glass. Stripe-like texture. |

### Layout Studies — Changing Structure (not just color)
| # | Name | Brief Description |
|---|-------|-------------------|
| 16 | **Command Palette (⌘K)** | Thinner chrome. All functions consolidated into a central floating palette (like Zed / Raycast). |
| 17 | **Power Split** | Keep search hits in a separate pane for jumping. River mini-map (like klogg). Ideal for large files. |

### Reference
| # | Name | Brief Description |
|---|-------|-------------------|
| 00 | **Graphite (base)** | Neutral chrome serving as the foundation for all proposals (`_base.html`). |

## How It Works — Why It’s Lightweight and Hackable

All proposals share the **same DOM** via `_base.html`, and only change **`:root` CSS custom properties (palette)** and a few *signature effects* (glow, aurora, scan lines, etc.). Specifically:

- **Theme = just a set of CSS variables**. To implement, simply add more `html[data-theme="…"]` styles to the current `style.css` (currently three: light/dark/black).
- No increase in runtime overhead (no images, no JS, no dependencies). **Appearance changes without affecting performance.**
- Users can add their own themes in a single file → **highly hackable**.

### The “Surprising” CSS Techniques Used
- Glowing search hits / accent bar on the left of the current line (`box-shadow` bloom)
- Frosted chrome (`backdrop-filter: blur`) — proposals 04 / 14
- Breathing aurora, neon streetlights, horizon of OutRun (`radial/linear-gradient` + `@keyframes`)
- CRT effects: scan lines, phosphor glow, blinking cursor — proposals 06 / 15
- Gradient-colored line number gutter (`background-clip: text`) — proposal 04
- The “river” mini-map at the right edge: flowing glowing dots for search hits — all proposals
- Vim-style mode indicator in the status bar (NORMAL / INSERT / SEARCH)

## Recommended Approach (My Opinion)

1. Start with **01 Ayame Zen**. It matches the brand, with purple × gold, establishing “Ayame-ness” that stands out from other editors.
2. Include preset themes like **07 Tokyo Night / 08 Catppuccin / 09 Rosé Pine / 13 Frostbite / 12 Alabaster Light**. Engineers appreciate having “their own color.”
3. Focus on **17 Power Split and 16 ⌘K** as the main flow. The “feel” and comfort come from here. Show large files’ advantage with search results in a separate pane + river mini-map.
4. As fun extras, hide themes like **06 Matrix / 05 Synthwave / 15 Amber CRT** as hidden themes.

## Additional — Modernizing the Frontend (Optional)

If you want a visual refresh, the raw JS in `crates/ayame-cli/web` can also be modernized (the “engine is Rust, UI uses the latest tools” setup is itself exciting). You can adopt the latest stack:

| Layer | Proposal | Effect |
|-------|----------|---------|
| Types | **tsgo (`@typescript/native-preview` / TypeScript 7 in Go)** | Extremely fast type checking compatible with `tsc` |
| Lint | **oxlint 1.7x** | Fast, type-aware linting (`--type-aware`, integrated with tsgolint) |
| Format | **oxfmt 0.5x** | Much faster than Prettier, nearly 100% compatible |
| Integration | **Vite+ / VoidZero (oxc family)** | Combine individual tools into a single chain |

*Note:* These are **optional alternative proposals**; the current design proposals are dependency-free and maintain their lightweight nature. Adoption is optional and can be decided separately.

## How to Add a New Proposal

```
1. Copy _base.html to design/mockups/NN-yourtheme.html
2. Change <title> and .theme-tag
3. Modify the palette values in :root { … } (add signature effects if needed)
4. Add a line to the DESIGNS array in index.html
```

The DOM, data, and layout remain shared. Only color and a bit of light are changed, enabling surprising variations.

## License

0BSD. You can use, copy, modify, and distribute this project for almost any purpose.