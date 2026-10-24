# Swiss motion

The International Typographic Style in motion: an asymmetric grid, flush-left grotesque type and flat color fields that slide into place with mechanical precision. The signature cue is the grid: every edge in the frame lines up with another.

- **Palette:** warm paper `#F2F0EB`, black `#111111` and one signal red `#E30613`. Flat color only: no gradients, shadows, glows or textures.
- **Type:** `Inter` bold for display and regular for text, or `Archivo Black` for one heavy numeral. Flush left, `leading` 1.0 to 1.1, and a steep size jump: display at 140 px or more against 32 to 40 px captions on a 1920-wide canvas.
- **Timing:** quick and exact. Wipes and slides take 0.3 to 0.4 seconds, elements arrive in reading order 0.1 to 0.15 seconds apart, and then everything holds still. No rotation, bounce or scale.

## Making it in FrameLane

- Choose a twelve-column grid inside 6% margins and derive every box from it. `x` is the box center, so a block whose left edge sits on the 6% margin with a 46% `width` has its `x` at 29%.
- Color fields and rules are rectangular shapes with a flat `fill`, revealed with `wipe_down` or `wipe_right`.
- Text enters with `slide_up` or `fade`, with `text_align` set to `left`.
- Break headlines by hand with newlines rather than letting the box wrap them.
- Footage sits in a grid cell as a cover box; `saturation` at -100 gives the style's black-and-white photography. Join clips with `wipe_left` or `band_wipe`.

## Where it goes wrong

- Centered text breaks the grid: align left edges, not centers.
- Left edges that almost line up: compute each box from its column's left edge plus half its width instead of placing it by eye.
- A wipe on text is rejected: the wipe presets reveal shapes, images and video, so slide text in.
- A wipe on a group does nothing at all: put it on each member.
- Decoration creeping in, such as a second accent, a shadow or a rounded corner: remove it. The grid and one red carry the style.

A complete six-second example:

```json
{
  "width": 1920, "height": 1080, "duration": 6, "frame_rate": 30,
  "output_format": "mp4", "background_color": "#F2F0EB",
  "elements": [
    {
      "type": "shape", "id": "red-field", "path": "M0 0 H100 V100 H0 Z", "fill": "#E30613",
      "time": 0, "duration": 6, "x": "79%", "y": "50%", "width": "42%", "height": "100%",
      "motion": [{"type": "wipe_down", "time": 0, "duration": 0.35}]
    },
    {
      "type": "text", "id": "headline", "text": "Form\nfollows\nfunction.",
      "time": 0.15, "duration": 5.85, "x": "29%", "y": "34%", "width": "46%", "height": "48%",
      "font_family": "Inter", "font_size": 150, "font_weight": 700, "leading": 1.0,
      "text_align": "left", "text_color": "#111111",
      "motion": [{"type": "slide_up", "time": 0.15, "duration": 0.35}]
    },
    {
      "type": "shape", "id": "rule", "path": "M0 0 H100 V100 H0 Z", "fill": "#111111",
      "time": 0.3, "duration": 5.7, "x": "29%", "y": "74%", "width": "46%", "height": "0.4%",
      "motion": [{"type": "wipe_right", "time": 0.3, "duration": 0.3}]
    },
    {
      "type": "text", "id": "caption", "text": "Grid, type and one color.",
      "time": 0.45, "duration": 5.55, "x": "26%", "y": "81%", "width": "40%", "height": "7%",
      "font_family": "Inter", "font_size": 36, "text_align": "left", "text_color": "#111111",
      "motion": [{"type": "fade", "time": 0.45, "duration": 0.3}]
    },
    {
      "type": "text", "id": "numeral", "text": "04",
      "time": 0.6, "duration": 5.4, "x": "77%", "y": "80%", "width": "30%", "height": "26%",
      "font_family": "Archivo Black", "font_size": 220, "text_align": "left", "text_color": "#F2F0EB",
      "motion": [{"type": "slide_up", "time": 0.6, "duration": 0.35}]
    }
  ]
}
```
