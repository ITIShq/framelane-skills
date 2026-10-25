<!-- Copied from api/mcp/skills/craft/color-and-brand.md (sha256 f1ebc69ea5f98fdfc132a69b45dd0a9e2ee3d62d0b3e71f9ea85edd1cdb3bcec) by scripts/build_skills_bundle.py. Edit the source, not this copy. -->

# Color and brand

For palettes, contrast, gradients and brand assets, and the default palette to use when no brand is supplied.

## Core

- A supplied brand wins. Read a site with `extract_brand_kit` or a saved kit with `get_brand_kit`, match its colors and approved wording, and inspect a logo with `get_asset_preview` before placing it.
- With no brand and no named style, use the default palette below and present it as a proposal, not as the person's brand.
- One accent per frame. It marks the single thing to look at: a rule, a key word or the call to action. It is never the fill of every surface.
- Text over footage needs a plate or a scrim. Judge contrast on the brightest and the darkest frame behind the text, not on one still.
- Never invent, redraw or recolor a logo or another company's mark. A supplied logo is an image element, placed as given.

## Default palette

This is the one palette the workflows share when the brief names no brand and no style.

| Role | Color | Where it goes |
| --- | --- | --- |
| Background | `#111827` | `background_color`; also the outline color for captions |
| Raised surface | `#1F2937` | cards, panels and diagram boxes |
| Primary text | `#FFFFFF` | headlines, labels and captions |
| Secondary text | `#D1FAE5` | supporting lines and small calls to action |
| Accent | `#86EFAC` | one emphasis per frame: a connector, a key word, the active caption word |
| Plate | `#111827DD` | translucent backing behind text over footage |

## Contrast

- Keep a strong lightness gap between text and what is behind it. As a working floor, aim for about 4.5:1 for body text and 3:1 for large headlines; these are review heuristics, not a tested accessibility claim.
- `LOW_CONTRAST` only judges flat backdrops. It stays silent over photos, video and translucent fills, so those need a server frame.
- On text, a plate is `background` set to true with an eight-digit `background_color`. For a large card or a full-frame scrim, use a `shape` with an alpha `fill`.

## Gradients

- A gradient is paint, never an element: a shape's `fill` or the request's `background_gradient`. A CSS linear-gradient string is accepted and stored as the gradient object.
- Close stops read as light; distant hues read as decoration. Identical stops are flagged as `GRADIENT_STOPS_IDENTICAL`.
- A centered radial is the safe default. Read the angle and center conventions in `request_schema` before placing an angled or off-center gradient.

## Color on footage and stills

- Grade video and images with their adjustment fields: `brightness`, `contrast`, `saturation`, `exposure`, `temperature`, `tint` and `vignette`. Named looks are `effects`; read the `effects` section for the list. The adjustment fields apply after `effects`, so `saturation` at -100 removes a tint an effect such as `sepia` adds.
- `color_overlay` is rejected. To tint media, stack a `shape` with a translucent `fill` above it and set a `blend_mode` such as `multiply` or `screen`.
- `lut_url` is applied on video only.

## When the brand and the frame disagree

- If a brand color fails contrast on the real footage, keep it for accents and rules, put a plate behind the text, and say so in the handoff.
- If a brand font is not a verified family, substitute one from `craft.typography` and name the substitution.
