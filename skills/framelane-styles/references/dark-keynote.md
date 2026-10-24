# Dark keynote

A stage presentation recorded as video: one statement at a time, set large in white on a near-black field lit by a soft central glow. The signature cue is restraint. A single accent color appears once per frame, and nothing moves while the viewer reads.

- **Palette:** background `#0B0E14` lifted to `#1B2230` at the center, statements in `#F5F7FA`, labels in `#8B95A7`, and one accent, `#5B8CFF` here or the brand's primary. Never two accents in one frame.
- **Type:** `Inter` only. Statements bold at 96 to 120 px on a 1920-wide canvas, eight words at most. Labels regular at 32 to 40 px, in capitals, tracked 3 to 5 px.
- **Timing:** one idea per beat, held for its reading time plus a breath. Entrances are fades or short slides of 0.5 to 0.7 seconds; no bounces, spins or loops.

## Making it in FrameLane

- The stage light is a `background_gradient` of `kind` `radial`, centered, with two close stops.
- Statements are text elements with a `fade` or `slide_up` entrance and, when the next beat replaces them, a reversed `fade` exit that ends with the element.
- Color one word through `words`: give it a `style` whose `color` is the accent.
- A thin accent rule is a `shape` with a rectangular path and a flat `fill`.
- A product still takes `ken_burns_in` for a slow push. Between footage beats use `dip_to_black` or `cross_dissolve`, which join video elements only.

## Where it goes wrong

- Several statements on screen at once: split them into beats rather than shrinking the type.
- Accent creep, where every label and rule turns blue: keep the accent to one element per frame.
- Labels that sink into the background in the export: keep them at `#8B95A7` or lighter here, and check a server frame.
- Sluggish pacing from one-second fades and long holds: keep fades under 0.7 seconds and move on once the reading time is met.
- A stage light so strong it reads as a spotlight on nothing: keep the center stop within a few shades of the edge.

A complete nine-second example:

```json
{
  "width": 1920, "height": 1080, "duration": 9, "frame_rate": 30,
  "output_format": "mp4", "background_color": "#0B0E14",
  "background_gradient": {"kind": "radial", "center": {"x": 0.5, "y": 0.5},
    "stops": [{"offset": 0, "color": "#1B2230"}, {"offset": 1, "color": "#0B0E14"}]},
  "elements": [
    {
      "type": "text", "id": "eyebrow", "text": "PRODUCT UPDATE",
      "time": 0, "duration": 9, "x": "50%", "y": "33%", "width": "60%", "height": "8%",
      "font_family": "Inter", "font_size": 34, "tracking": 4, "text_color": "#8B95A7",
      "motion": [{"type": "fade", "time": 0, "duration": 0.6}]
    },
    {
      "type": "shape", "id": "accent-rule", "path": "M0 0 H100 V100 H0 Z", "fill": "#5B8CFF",
      "time": 0, "duration": 9, "x": "50%", "y": "38%", "width": "4%", "height": "0.4%",
      "motion": [{"type": "fade", "time": 0, "duration": 0.6}]
    },
    {
      "type": "text", "id": "statement-1", "text": "One clear idea per slide",
      "time": 0.3, "duration": 4.2, "x": "50%", "y": "50%", "width": "84%", "height": "16%",
      "font_family": "Inter", "font_size": 112, "font_weight": 700, "text_color": "#F5F7FA",
      "motion": [{"type": "fade", "time": 0.3, "duration": 0.6},
                 {"type": "fade", "time": 3.9, "duration": 0.6, "reversed": true}]
    },
    {
      "type": "text", "id": "statement-2", "text": "Then let it breathe",
      "time": 4.5, "duration": 4.5, "x": "50%", "y": "50%", "width": "84%", "height": "16%",
      "font_family": "Inter", "font_size": 112, "font_weight": 700, "text_color": "#F5F7FA",
      "words": [{"word": "breathe", "start": 4.5, "end": 9, "style": {"color": "#5B8CFF"}}],
      "motion": [{"type": "fade", "time": 4.5, "duration": 0.6}]
    }
  ]
}
```
