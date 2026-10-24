# Trim-path whiteboard

A diagram that draws itself, stroke by stroke, on a clean white board, like a teacher sketching an idea with markers. The signature cue is order: every line appears the way a hand would draw it, and each label arrives only after its line is finished.

- **Palette:** board `#FAFAF7`, marker black `#222222` for structure, blue `#1F5FBF` and red `#D64545` with one meaning each, and a pale yellow highlight `#FFE58A` behind one phrase at most.
- **Type:** `Zilla Slab` for labels and `Libre Franklin` for sentences, 40 to 56 px on a 1920-wide canvas, in sentence case. No handwriting face is verified, so do not imitate one.
- **Timing:** strokes draw one at a time, each over 0.6 to 1.2 seconds and an arrowhead in about 0.3; a label fades in over 0.3 seconds as its stroke ends; the finished diagram holds for its reading time.

## Making it in FrameLane

- Every stroke is a `shape` with `fill` set to `none`, a `stroke` color, `stroke_linecap` set to `round`, and `trim` start and end at 0. `trim_keyframes` take `end` from 0 to 1 in timeline seconds.
- A circle is two arcs. An arrow is a shaft followed by a separate short head path that draws after it.
- A dotted connector adds `stroke_dash`; the trim applies before the dash, so the pattern does not crawl while it draws.
- Keep each `view_box` in the pixel aspect of its box so strokes keep an even width, and size `stroke_width` for the box it scales with.
- A highlight is a translucent yellow rectangle behind the words, revealed with `wipe_right`.

## Where it goes wrong

- A filled shape: the trim draws the outline, but the fill appears at once.
- Arrows drawn from the head back to the tail: a path draws in the order it is written, so start it at the tail.
- Strokes that vanish or swell: `HAIRLINE_BELOW_PIXEL` and `SHAPE_ASPECT_MISMATCH` name the cause.
- Everything drawing at once: stagger the strokes so the eye can follow one at a time.
- Keyframe times written as fractions: `trim_keyframes` use timeline seconds, unlike `custom_animations`.

A complete eight-second example:

```json
{
  "width": 1920, "height": 1080, "duration": 8, "frame_rate": 30,
  "output_format": "mp4", "background_color": "#FAFAF7",
  "elements": [
    {
      "type": "shape", "id": "circle-idea", "path": "M50 4 A46 46 0 0 1 50 96 A46 46 0 0 1 50 4",
      "view_box": [0, 0, 100, 100], "fill": "none", "stroke": "#222222", "stroke_width": 1.6,
      "stroke_linecap": "round", "time": 0, "duration": 8,
      "x": "25%", "y": "50%", "width": "15.625%", "height": "27.778%",
      "trim": {"start": 0, "end": 0},
      "trim_keyframes": [{"time": 0.2, "end": 0}, {"time": 1.2, "end": 1}]
    },
    {
      "type": "text", "id": "label-idea", "text": "Idea",
      "time": 1.2, "duration": 6.8, "x": "25%", "y": "50%", "width": "14%", "height": "10%",
      "font_family": "Zilla Slab", "font_size": 56, "text_color": "#222222",
      "motion": [{"type": "fade", "time": 1.2, "duration": 0.3}]
    },
    {
      "type": "shape", "id": "arrow-shaft", "path": "M0 10 H620", "view_box": [0, 0, 620, 20],
      "fill": "none", "stroke": "#222222", "stroke_width": 5, "stroke_linecap": "round",
      "time": 0, "duration": 8, "x": "50%", "y": "50%", "width": "32.292%", "height": "1.852%",
      "trim": {"start": 0, "end": 0},
      "trim_keyframes": [{"time": 1.5, "end": 0}, {"time": 2.3, "end": 1}]
    },
    {
      "type": "shape", "id": "arrow-head", "path": "M10 4 L34 20 L10 36", "view_box": [0, 0, 40, 40],
      "fill": "none", "stroke": "#222222", "stroke_width": 5,
      "stroke_linecap": "round", "stroke_linejoin": "round",
      "time": 0, "duration": 8, "x": "65.938%", "y": "50%", "width": "2.083%", "height": "3.704%",
      "trim": {"start": 0, "end": 0},
      "trim_keyframes": [{"time": 2.3, "end": 0}, {"time": 2.6, "end": 1}]
    },
    {
      "type": "shape", "id": "circle-video", "path": "M50 4 A46 46 0 0 1 50 96 A46 46 0 0 1 50 4",
      "view_box": [0, 0, 100, 100], "fill": "none", "stroke": "#1F5FBF", "stroke_width": 1.6,
      "stroke_linecap": "round", "time": 0, "duration": 8,
      "x": "75%", "y": "50%", "width": "15.625%", "height": "27.778%",
      "trim": {"start": 0, "end": 0},
      "trim_keyframes": [{"time": 2.7, "end": 0}, {"time": 3.7, "end": 1}]
    },
    {
      "type": "text", "id": "label-video", "text": "Video",
      "time": 3.7, "duration": 4.3, "x": "75%", "y": "50%", "width": "14%", "height": "10%",
      "font_family": "Zilla Slab", "font_size": 56, "text_color": "#1F5FBF",
      "motion": [{"type": "fade", "time": 3.7, "duration": 0.3}]
    },
    {
      "type": "text", "id": "caption", "text": "Sketch it before you build it",
      "time": 4.2, "duration": 3.8, "x": "50%", "y": "77%", "width": "70%", "height": "8%",
      "font_family": "Libre Franklin", "font_size": 44, "text_color": "#222222",
      "motion": [{"type": "fade", "time": 4.2, "duration": 0.3}]
    },
    {
      "type": "shape", "id": "underline", "path": "M4 14 C150 6 400 20 572 10", "view_box": [0, 0, 576, 22],
      "fill": "none", "stroke": "#D64545", "stroke_width": 5, "stroke_linecap": "round",
      "time": 0, "duration": 8, "x": "50%", "y": "83%", "width": "30%", "height": "2.037%",
      "trim": {"start": 0, "end": 0},
      "trim_keyframes": [{"time": 4.6, "end": 0}, {"time": 5.2, "end": 1}]
    }
  ]
}
```
