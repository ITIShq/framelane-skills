# Hologram HUD

A heads-up display from science fiction: thin cyan line work, rings and brackets floating over a blue-black field, monospace readouts that count up and type themselves, and slow rotation. The signature cue is that everything is drawn in light, as strokes rather than solid panels, with one warm warning color.

- **Palette:** field `#03070D`; cyan `#3DE0FF` for lines and type; the same cyan at about a third of its opacity, `#3DE0FF59`, for secondary marks; and amber `#FFB547` for warnings only. Any panel stays under 10% opacity.
- **Type:** `IBM Plex Mono` in capitals, 22 to 36 px on a 1920-wide canvas and tracked 3 to 6 px, with large readouts up to about 72 px bold.
- **Timing:** brisk and mechanical. Lines draw on in 0.3 to 0.6 seconds, readouts settle within 1.5 seconds, and an ambient ring turns once every 10 to 20 seconds. Nothing bounces.

## Making it in FrameLane

- Line work is stroked shapes with `fill` set to `none`, drawn on with `trim_keyframes`. Rings are arcs, and a ring of ticks is a thicker arc with `stroke_dash`.
- The glow is a second copy of a stroke underneath it, three to five times wider in faint cyan. Text `glow` also exists; inspect a server frame before relying on it.
- Ambient rotation is `loop_rotate_smooth` on a dashed ring, with `duration` as one turn.
- Readouts use a `counter` with `tabular_numbers` and a block `fade`; labels arrive with `glitch_pop` at `character` scope.
- Over footage, set `blend_mode` to `screen` on the line shapes and add a low-intensity `scanlines` or `chromatic_aberration` effect to the video.

## Where it goes wrong

- Solid panels turn the display into a dashboard: keep fills nearly transparent and let strokes carry it.
- Hairlines that drop out: strokes under a device pixel vanish, which `HAIRLINE_BELOW_PIXEL` reports.
- Several loops at once: keep one ambient loop and let everything else settle.
- A readout that jitters while it counts: set `tabular_numbers`.
- Small cyan text over busy footage: put a faint dark plate behind it.

A complete six-second example:

```json
{
  "width": 1920, "height": 1080, "duration": 6, "frame_rate": 30,
  "output_format": "mp4", "background_color": "#03070D",
  "elements": [
    {
      "type": "shape", "id": "ring-halo", "path": "M50 4 A46 46 0 0 1 50 96 A46 46 0 0 1 50 4",
      "view_box": [0, 0, 100, 100], "fill": "none", "stroke": "#3DE0FF33", "stroke_width": 2.4,
      "time": 0, "duration": 6, "x": "50%", "y": "48%", "width": "25%", "height": "44.444%",
      "motion": [{"type": "fade", "time": 0, "duration": 0.6}]
    },
    {
      "type": "shape", "id": "ring", "path": "M50 4 A46 46 0 0 1 50 96 A46 46 0 0 1 50 4",
      "view_box": [0, 0, 100, 100], "fill": "none", "stroke": "#3DE0FF", "stroke_width": 0.5,
      "time": 0, "duration": 6, "x": "50%", "y": "48%", "width": "25%", "height": "44.444%",
      "trim": {"start": 0, "end": 0},
      "trim_keyframes": [{"time": 0.1, "end": 0}, {"time": 0.6, "end": 1}]
    },
    {
      "type": "shape", "id": "tick-ring", "path": "M50 10 A40 40 0 0 1 50 90 A40 40 0 0 1 50 10",
      "view_box": [0, 0, 100, 100], "fill": "none", "stroke": "#3DE0FF59", "stroke_width": 3,
      "stroke_linecap": "butt", "stroke_dash": [1, 2.2],
      "time": 0, "duration": 6, "x": "50%", "y": "48%", "width": "25%", "height": "44.444%",
      "motion": [{"type": "loop_rotate_smooth", "time": 0, "duration": 12}]
    },
    {
      "type": "shape", "id": "bracket-left", "path": "M24 2 H2 V98 H24", "view_box": [0, 0, 26, 100],
      "fill": "none", "stroke": "#3DE0FF", "stroke_width": 1.5,
      "time": 0, "duration": 6, "x": "35.417%", "y": "48%", "width": "2.708%", "height": "18.519%",
      "trim": {"start": 0, "end": 0},
      "trim_keyframes": [{"time": 0.3, "end": 0}, {"time": 0.7, "end": 1}]
    },
    {
      "type": "shape", "id": "bracket-right", "path": "M2 2 H24 V98 H2", "view_box": [0, 0, 26, 100],
      "fill": "none", "stroke": "#3DE0FF", "stroke_width": 1.5,
      "time": 0, "duration": 6, "x": "64.583%", "y": "48%", "width": "2.708%", "height": "18.519%",
      "trim": {"start": 0, "end": 0},
      "trim_keyframes": [{"time": 0.3, "end": 0}, {"time": 0.7, "end": 1}]
    },
    {
      "type": "text", "id": "readout", "text": "0.0%",
      "time": 0.4, "duration": 5.6, "x": "50%", "y": "46%", "width": "20%", "height": "10%",
      "font_family": "IBM Plex Mono", "font_size": 72, "font_weight": 700, "text_color": "#3DE0FF",
      "tabular_numbers": true,
      "counter": {"from_value": 0, "to_value": 98.6, "decimals": 1, "suffix": "%",
                  "easing": "quad_out", "start_time": 0, "length": 1.4},
      "motion": [{"type": "fade", "time": 0.4, "duration": 0.3}]
    },
    {
      "type": "text", "id": "readout-label", "text": "SIGNAL INTEGRITY",
      "time": 0.6, "duration": 5.4, "x": "50%", "y": "55%", "width": "20%", "height": "5%",
      "font_family": "IBM Plex Mono", "font_size": 24, "tracking": 5, "text_color": "#3DE0FF",
      "motion": [{"type": "glitch_pop", "scope": "character", "time": 0.6, "duration": 0.5}]
    },
    {
      "type": "text", "id": "status", "text": "STANDBY",
      "time": 1.2, "duration": 4.8, "x": "14%", "y": "88%", "width": "16%", "height": "5%",
      "font_family": "IBM Plex Mono", "font_size": 24, "tracking": 4, "text_align": "left",
      "text_color": "#FFB547",
      "motion": [{"type": "fade", "time": 1.2, "duration": 0.3}]
    }
  ]
}
```
