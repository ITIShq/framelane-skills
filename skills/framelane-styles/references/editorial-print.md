# Editorial print

The calm authority of a magazine spread: a large serif headline, an italic standfirst, generous margins, hairline rules and a small sans byline on warm paper. The signature cue is the contrast between a high-contrast display serif and tiny, widely tracked labels, with white space doing the rest.

- **Palette:** paper `#F4EFE6`, ink `#1C1A17`, a softer ink `#3A3631` for secondary lines, and one muted accent, oxblood `#8A2B2B`, for section labels and rules only.
- **Type:** `Playfair Display` bold for headlines at 96 to 130 px on a 1920-wide canvas; `Source Serif 4` or `Lora` in italic for the standfirst at 40 to 48 px; `Libre Franklin` capitals at 24 to 28 px, tracked 3 to 4 px, for labels and bylines. Everything flush left on one column, with headline `leading` about 1.1.
- **Timing:** slow and quiet. Fades of 0.6 to 0.9 seconds, rules drawn over about a second, beats of five seconds or more; no bounces and no per-letter motion.

## Making it in FrameLane

- Hairline rules are stroked shapes with `fill` set to `none`, drawn on with `trim_keyframes`.
- Text enters with `fade`, the standfirst about half a second after the headline. Italic is `font_style` set to `italic`.
- Photographs sit in the column as image or video elements whose box matches the source aspect. Soften them with `saturation` around -15 and a little `contrast`; on video, the `portra` or `filmic` effect gives a printed-photo grade.
- Between photographs on video use `cross_dissolve`; between stills, overlap two `fade` motions.

## Where it goes wrong

- Thin serif strokes shimmer or break up after compression: keep serif text at 40 px or more and set anything smaller in `Libre Franklin`.
- Weight contrast that never appears: weights render as regular or bold, so contrast comes from the face and the size, not from 300 against 500.
- Crowding: keep at least an 8% margin and one headline per frame.
- Italic everywhere: keep it for the standfirst and quotations.

A complete eight-second example:

```json
{
  "width": 1920, "height": 1080, "duration": 8, "frame_rate": 30,
  "output_format": "mp4", "background_color": "#F4EFE6",
  "elements": [
    {
      "type": "text", "id": "section-label", "text": "THE LONG READ",
      "time": 0, "duration": 8, "x": "28%", "y": "20%", "width": "40%", "height": "6%",
      "font_family": "Libre Franklin", "font_size": 26, "tracking": 4, "text_align": "left",
      "text_color": "#8A2B2B",
      "motion": [{"type": "fade", "time": 0, "duration": 0.7}]
    },
    {
      "type": "shape", "id": "hairline", "path": "M0 5 H710", "view_box": [0, 0, 710, 10],
      "fill": "none", "stroke": "#1C1A17", "stroke_width": 2, "stroke_linecap": "butt",
      "time": 0, "duration": 8, "x": "28%", "y": "25%", "width": "40%", "height": "1%",
      "trim": {"start": 0, "end": 0},
      "trim_keyframes": [{"time": 0.2, "end": 0}, {"time": 1.0, "end": 1}]
    },
    {
      "type": "text", "id": "headline", "text": "The quiet return\nof slow news",
      "time": 0.4, "duration": 7.6, "x": "36%", "y": "42%", "width": "56%", "height": "28%",
      "font_family": "Playfair Display", "font_size": 112, "font_weight": 700, "leading": 1.08,
      "text_align": "left", "text_color": "#1C1A17",
      "motion": [{"type": "fade", "time": 0.4, "duration": 0.8}]
    },
    {
      "type": "text", "id": "standfirst", "text": "Why readers are choosing depth over speed",
      "time": 1.0, "duration": 7, "x": "32%", "y": "64%", "width": "48%", "height": "10%",
      "font_family": "Source Serif 4", "font_size": 44, "font_style": "italic", "text_align": "left",
      "text_color": "#3A3631",
      "motion": [{"type": "fade", "time": 1.0, "duration": 0.7}]
    },
    {
      "type": "text", "id": "byline", "text": "WORDS BY A STAFF WRITER",
      "time": 1.5, "duration": 6.5, "x": "28%", "y": "78%", "width": "40%", "height": "5%",
      "font_family": "Libre Franklin", "font_size": 24, "tracking": 3, "text_align": "left",
      "text_color": "#3A3631",
      "motion": [{"type": "fade", "time": 1.5, "duration": 0.6}]
    }
  ]
}
```
