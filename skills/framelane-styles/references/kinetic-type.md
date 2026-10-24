# Kinetic type

Words are the picture. Short phrases arrive one beat at a time in heavy condensed capitals, each word animated letter by letter and timed to a voice or a music beat. The signature cue is scale contrast: one huge word against small connecting words.

- **Palette:** black `#0A0A0A` and white `#FFFFFF`, plus one hot accent such as `#FF4D1F` for the word that matters. Nothing behind the type but flat color.
- **Type:** one condensed display face, `Oswald` or `Barlow Condensed` bold in capitals, or `Archivo Black`. On a 1080-wide portrait canvas, key words run 180 to 300 px and connecting words about a third of that.
- **Timing:** a new word or short phrase every 0.4 to 0.8 seconds, entrances of 0.3 to 0.5 seconds, and a final line that holds for its full reading time before anything leaves.

## Making it in FrameLane

- Give each word or phrase its own text element, so each gets its own `time`, size and `motion`.
- Per-glyph entrances are the presets whose `character` flag is true in `motions`, run with `scope` set to `character`: `whip_up`, `rubber_in`, `glitch_pop` and `drift_in`. Their exits are `whip_down`, `evaporate` and `drift_out`.
- A `spacing` below 1 overlaps the letters, so a long word still lands on the beat.
- For speech, drive one caption from transcript word times with `word_animation` in the `scale_pop` style.
- To flip the ground between sections, start a full-frame shape of the other color under the type on the beat.

## Where it goes wrong

- Letters clipped at the box edge: per-glyph moves overshoot, so give each text box spare room and keep `OUT_OF_FRAME` clean.
- Unreadable pace: a word on screen for less than about 0.3 seconds is a flicker, so merge it with a neighbor.
- Too many entrance styles: choose two for the whole piece and repeat them.
- `scope` set to `character` on a block preset such as `fade` or `slide_up` is rejected: use a per-glyph preset.
- Stacking routes on one element: `motion`, `word_animation` and `animation_preset` are alternatives, not layers.

A complete five-second portrait example:

```json
{
  "width": 1080, "height": 1920, "duration": 5, "frame_rate": 30,
  "output_format": "mp4", "background_color": "#0A0A0A",
  "elements": [
    {
      "type": "text", "id": "word-make", "text": "MAKE",
      "time": 0, "duration": 5, "x": "50%", "y": "30%", "width": "84%", "height": "14%",
      "font_family": "Oswald", "font_size": 200, "font_weight": 700, "text_color": "#FFFFFF",
      "motion": [{"type": "whip_up", "scope": "character", "time": 0, "duration": 0.4, "spacing": 0.5},
                 {"type": "whip_down", "scope": "character", "time": 4.6, "duration": 0.4}]
    },
    {
      "type": "text", "id": "word-every", "text": "EVERY",
      "time": 0.5, "duration": 4.5, "x": "50%", "y": "44%", "width": "84%", "height": "14%",
      "font_family": "Oswald", "font_size": 200, "font_weight": 700, "text_color": "#FFFFFF",
      "motion": [{"type": "whip_up", "scope": "character", "time": 0.5, "duration": 0.4, "spacing": 0.5},
                 {"type": "whip_down", "scope": "character", "time": 4.6, "duration": 0.4}]
    },
    {
      "type": "text", "id": "word-word", "text": "WORD",
      "time": 1.0, "duration": 4, "x": "50%", "y": "60%", "width": "90%", "height": "18%",
      "font_family": "Oswald", "font_size": 290, "font_weight": 700, "text_color": "#FF4D1F",
      "motion": [{"type": "rubber_in", "scope": "character", "time": 1.0, "duration": 0.5, "spacing": 0.5},
                 {"type": "whip_down", "scope": "character", "time": 4.6, "duration": 0.4}]
    },
    {
      "type": "text", "id": "word-count", "text": "count.",
      "time": 1.7, "duration": 3.3, "x": "50%", "y": "74%", "width": "60%", "height": "8%",
      "font_family": "Oswald", "font_size": 90, "text_color": "#FFFFFF",
      "motion": [{"type": "drift_in", "scope": "character", "time": 1.7, "duration": 0.4},
                 {"type": "drift_out", "scope": "character", "time": 4.6, "duration": 0.4}]
    }
  ]
}
```
