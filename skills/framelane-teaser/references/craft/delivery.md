<!-- Copied from api/mcp/skills/craft/delivery.md (sha256 e8caec9c46f3afc242fcba6aa660d440a612a882247ed9b8f90d63e705272272) by scripts/build_skills_bundle.py. Edit the source, not this copy. -->

# Delivery

For the canvas, frame rate, format and length a destination needs, and for exporting variants.

## Core

- Choose the canvas from where the video will play, within the `limits` in capabilities: 1920 × 1080 for landscape players, slides and the web; 1080 × 1920 for vertical feeds and stories; 1080 × 1080 or 1080 × 1350 for square and portrait feed posts. Use the requested size when one is given.
- Set `frame_rate` explicitly. 30 suits most work, 24 gives a film cadence, and 60 suits fast screen recordings. Match the source footage where you can, because mixing 25 and 30 fps material stutters. A graphics-only request without one is flagged as `FRAME_RATE_DEFAULTED`.
- `output_format` is `mp4`, `webm` or `mov`, and `mp4` is the usual deliverable. A transparent overlay needs `alpha` with `webm` or `mov`; with `alpha` on, `background_color` is ignored, so draw any backdrop as a shape.
- Export only when asked, from the reviewed version, with `render_project`. Previews are free; exports spend the plan's export minutes.
- One project per variant. Each aspect ratio, language or length gets its own named project, created from the full render request of the version you reviewed.

## Length

- Let the content set the length, then check it against the destination. Platforms change their limits, so confirm a cap rather than assuming one.
- A longer top-level `duration` does not extend media or text: each element keeps its own window.

## Stills and posters

- A thumbnail or poster is a single-frame preview at a settled, expressive moment, read back as a PNG through `get_render_preview`. It is not a video format.
- A watermark is an image overlaid on the whole output through `watermark_url`.

## Naming the deliverable

- Name each export by its idea and variant, such as the aspect ratio or language, and keep the project link separate from the temporary download link.
