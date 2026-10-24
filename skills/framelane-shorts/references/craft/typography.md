<!-- Copied from api/mcp/skills/craft/typography.md (sha256 d0803f514f774b3b10eb0f88b25146b45b593043d149966759f3bd5e7130fd3f) by scripts/build_skills_bundle.py. Edit the source, not this copy. -->

# Typography

For fonts, sizes, weights, text treatments and how long text stays on screen.

## Core

- Name only a verified family. The renderer looks `font_family` up by name and does not reject one it cannot find: it quietly draws another face. Use the list below, or preview a one-frame probe of any other family before building on it.
- Use one family, or one display face with one text face, and two weights. Weight is binary: `font_weight` 600 or more renders bold and anything lower renders regular, so hierarchy comes from size, face and color.
- `font_size` is in pixels relative to the canvas width. The same value is the same visual size at any height, so convert between canvases by the width ratio only.
- Hold text for at least words/3 + 1 seconds, counted after the entrance settles and before an exit or a transition covers it. Shorten the copy before you shrink the type.

## Verified families

Each of these rendered as itself, not as a fallback, in a probe on the render farm. Only Latin text was probed.

| Family | Character | Good for |
| --- | --- | --- |
| `Inter` | neutral sans | interfaces, keynotes, captions |
| `Libre Franklin` | classic gothic sans | labels, bylines, news |
| `Archivo Black` | heavy display sans, one weight | posters, big numerals |
| `Archivo Narrow` | narrow sans | dense labels, tables |
| `Barlow Condensed` | condensed sans | kinetic type, sport |
| `Oswald` | condensed display sans | kinetic type, headlines |
| `Roboto Condensed` | condensed neutral sans | captions in narrow columns |
| `Playfair Display` | high-contrast display serif | editorial and period titles |
| `Source Serif 4` | text serif | standfirsts, body copy |
| `Lora` | soft text serif | quotes, warm editorial |
| `Zilla Slab` | slab serif | friendly labels, explainers |
| `IBM Plex Mono` | monospace | code, interfaces, readouts |

## Sizes to start from

These are editorial starting points, not limits. Preview the export before reducing a size.

| Output | Headline | Secondary text | Captions |
| --- | --- | --- | --- |
| 1920 × 1080 landscape | 88 to 112 px | 40 to 52 px | 40 to 48 px |
| 1080 × 1920 portrait | 64 to 80 px | 40 to 48 px | 48 to 60 px |
| 1080 × 1080 square | 64 to 88 px | 36 to 44 px | 40 to 48 px |

A 96 px headline on a 1920-wide canvas keeps its share of the frame at 1080 wide as 96 × 1080 / 1920 = 54 px. Decide whether that share is right: a portrait version usually wants a larger share and fewer words. Never scale by the height ratio.

## Reading time

- A six-word title needs about three seconds of settled time, so a six-second piece holds a promise and a short call to action, not two paragraphs.
- Unfamiliar names, numbers and dense terms need longer than the formula gives.
- Captions: three to seven words per phrase, at most two lines, and at least 120 milliseconds per caption word where the speech allows it. Keep a short negation with the phrase it changes.

## How text renders

- A translucent plate is `background` set to true with an eight-digit `background_color` such as `#000000AA`. `background_opacity`, `x_padding` and `y_padding` never reach the renderer. `background_style` `line_round` or `line_hard` puts a plate behind each line.
- Only one of `background`, `stroke` (outline only) and `shadow` can be true on a text element. An outline around filled text is `stroke_color` with `stroke_width`, a fraction of the font size; about 0.06 suits captions.
- The `color` and `box` word-animation styles paint with `background_color` and require it.
- `text_decoration` renders only `none`. Underline single words through `words` with a `style`, and draw a strike-through as a thin shape.
- `tracking` is letter spacing in pixels and `leading` is line height as a multiple of the font size. `tabular_numbers` stops a `counter` from jittering.
- Text `opacity` never reaches the renderer: fade text with a `fade` motion.
- Line breaks: check them in server frames, because a browser preview can shape text differently. Put a newline in `text` rather than letting a headline break before a lone last word.
- Scripts other than Latin have not been checked in a hosted render; treat them as unverified until you inspect the frames.
