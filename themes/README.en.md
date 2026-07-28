<!-- i18n: language-switcher -->
[English](README.en.md) | [日本語](README.md)

# Ayame — Theme (JSON Definition)

The appearance of Ayame is **defined in JSON**. It includes a preset for iris-style light themes, and within the app's "Theme" panel, you can **edit → apply → save as custom** (via `localStorage`) → **export (JSON)**.

Preview: Open [`../gui/iris-light.html`](../gui/iris-light.html). In the bottom-right "Theme" panel (submenu), you can switch presets, adjust **background (watercolor / solid color)**, **illustration density**, and edit JSON. The panel can be **hidden with ✕** and reopened from the small "Theme" tab. (Specify a preset with `?theme=mono-paper`, and start with the panel hidden using `?panel=0`.)

## Included Presets (All Light)

| File | Name | Atmosphere |
|---|---|---|
| `iris-light.json` | Iris Light | Classic. Warm white paper × purple iris × gold. |
| `iris-mist.json`  | Iris Mist  | Cool light blue to aqua. Mostly white, light and airy. |
| `iris-dawn.json`  | Iris Dawn  | Dawn-colored orchid × rose × gold. Warm. |
| `sumi-light.json` | Sumi Light | Nearly white ink. Accent color is a single iris. Very minimal. |
| `mono-paper.json` | Mono Paper (Solid Color) | Preset with a single solid background color. No illustrations. |

## Schema

Official definition is [`theme.schema.json`](./theme.schema.json). Key points:

```jsonc
{
  "name": "Iris Light",          // Display name (also used as key for custom saves)
  "type": "light",               // Currently only "light"
  "radius": 12,                  // Panel corner radius (px)
  "font": { "mono": "…", "ui": "…" },   // Optional
  "color": {
    "paper": "#FBF8F1", "paper2": "#F1E9DA",   // Main background / gradient base
    "ink": "#2A2140", "inkDim": "#6E6383", "inkFaint": "#A99DBC",
    "accent": "#7A5CC0", "accent2": "#B98BD6", // Iris (main / secondary)
    "gold": "#C79A2E", "stem": "#4E9C77",      // Flower core gold / stem green
    "edge": "rgba(90,70,140,0.16)",            // Lines
    "ok": "#4E8C4A", "warn": "#B5771D", "err": "#C0506A",
    "markBg": "rgba(199,154,46,0.20)", "markFg": "#6B5510",
    "markCur": "#E8B84B", "markCurFg": "#2A2205"  // Search hits / current hit
  },
  "acrylic": { "tint": "rgba(255,253,248,0.66)", "blur": 30, "noise": 0.05 },
  // ↑ Fluent-style acrylic: panel semi-transparent fill / blur / noise
  "background": { "mode": "watercolor", "solid": "#F1E9DA" },
  // ↑ Background (user-changeable, also reflected behind submenu)
  //    mode:"solid" makes it a single color, for users who want a uniform background.
  "illustration": 0.4,          // Darkness of cutout illustration on the left (0..1, 0=hidden)
  "watercolor": [               // Watercolor iris / bleed layers (multiple layers)
    { "x": "12%", "y": "78%", "r": "46vh", "color": "rgba(122,92,192,0.30)" }
  ]
}
```

`color.*` maps directly to CSS custom properties (`--paper`, `--accent`, etc.), `acrylic.*` affects the panel's `background` / `backdrop-filter`, and `watercolor[]` layers are used in the background's `radial-gradient` layers. The shape of the cutout illustration (print style) also uses `accent / accent2 / gold / stem` colors.

## Implementation Notes (Integrating into the Actual Code)

- The current `crates/ayame-cli/web/style.css` uses `html[data-theme]` method. You can add a lightweight loader to convert JSON to CSS variables here (presets are stored as `themes/*.json` files directly included).
- Custom themes are stored in `localStorage["ayame.theme.customs.v1"]`. In the future, it can be extended to read from user config directories like `~/.config/ayame/themes/*.json` (remaining dependency-free distribution).
- No additional runtime or images are needed. **Lightweight as ever**, themes are shared/distributed as a single JSON file.