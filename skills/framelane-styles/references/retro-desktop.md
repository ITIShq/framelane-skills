# Retro desktop

A late-1990s desktop as a stage set: gray beveled windows on teal, a navy title bar, and monospace copy that types itself out ahead of a blinking cursor. The signature cue is the bevel: a light edge on the top and left of each panel, a dark one on the bottom and right.

- **Palette:** desktop `#008080`, window gray `#C0C0C0`, bevel light `#FFFFFF` and dark `#404040`, title bar `#000080`, black text. Hard edges only: no gradients, blur or soft shadows.
- **Type:** `IBM Plex Mono` for everything, 28 to 40 px on a 1920-wide canvas, bold white in title bars.
- **Timing:** things appear rather than glide. Panels pop in at once or with a short `smooth_pop`, text types at about 15 characters a second, and the cursor blinks once a second.

## Making it in FrameLane

- Build a window from three rectangles in array order: the dark bevel offset 3 to 4 px down and right, the light bevel offset 2 to 3 px up and left, then the gray face. A navy shape inside the top edge is the title bar.
- Typing is the "typewriter" text animation named in `preset` on the text's `motion`, with a `duration` of the character count divided by 15.
- The cursor is a black rectangle. A `custom_animations` element entry steps its `opacity` from 1 to 0 and back with `hold` easing, bound through `custom` with `loop` and a one-second `duration`.
- In `IBM Plex Mono` each character advances 0.6 × `font_size`, so a left-aligned line of n characters ends n × 0.6 × `font_size` pixels past its box's left edge, where the cursor goes.
- Footage inside a window takes the `crt`, `scanlines` or `posterize` effect.

## Where it goes wrong

- Real operating-system trade dress, such as a product's logo or startup sound: keep the panels generic.
- A cursor that drifts from the text: only a monospace face makes its position computable, so type in `IBM Plex Mono` and check a frame.
- Smooth fades and eased slides look modern: keep moves instant or very short.

A complete six-second example:

```json
{
  "width": 1920, "height": 1080, "duration": 6, "frame_rate": 30,
  "output_format": "mp4", "background_color": "#008080",
  "custom_animations": {"element": [{"name": "cursor-blink", "keyframes": [
    {"time": 0, "opacity": 1},
    {"time": 0.5, "opacity": 0, "easing": "hold"},
    {"time": 1, "opacity": 1, "easing": "hold"}]}]},
  "elements": [
    {
      "type": "shape", "id": "window-shade", "path": "M0 0 H100 V100 H0 Z", "fill": "#404040",
      "time": 0, "duration": 6, "x": "50.2%", "y": "50.35%", "width": "56%", "height": "40%"
    },
    {
      "type": "shape", "id": "window-light", "path": "M0 0 H100 V100 H0 Z", "fill": "#FFFFFF",
      "time": 0, "duration": 6, "x": "49.85%", "y": "49.75%", "width": "56%", "height": "40%"
    },
    {
      "type": "shape", "id": "window-face", "path": "M0 0 H100 V100 H0 Z", "fill": "#C0C0C0",
      "time": 0, "duration": 6, "x": "50%", "y": "50%", "width": "56%", "height": "40%"
    },
    {
      "type": "shape", "id": "title-bar", "path": "M0 0 H100 V100 H0 Z", "fill": "#000080",
      "time": 0, "duration": 6, "x": "50%", "y": "32.6%", "width": "55.5%", "height": "4.4%"
    },
    {
      "type": "text", "id": "title", "text": "message.txt",
      "time": 0, "duration": 6, "x": "38%", "y": "32.6%", "width": "30%", "height": "4%",
      "font_family": "IBM Plex Mono", "font_size": 28, "font_weight": 700,
      "text_align": "left", "text_color": "#FFFFFF"
    },
    {
      "type": "text", "id": "typed-line", "text": "Hello. This line types itself.",
      "time": 0.5, "duration": 5.5, "x": "50%", "y": "47%", "width": "52%", "height": "6%",
      "font_family": "IBM Plex Mono", "font_size": 36, "text_align": "left", "text_color": "#000000",
      "motion": [{"preset": "typewriter", "time": 0.5, "duration": 2}]
    },
    {
      "type": "shape", "id": "cursor", "path": "M0 0 H100 V100 H0 Z", "fill": "#000000",
      "time": 2.5, "duration": 3.5, "x": "58.3%", "y": "47%", "width": "1.1%", "height": "3.6%",
      "motion": [{"custom": "cursor-blink", "time": 2.5, "duration": 1, "loop": true}]
    }
  ]
}
```
