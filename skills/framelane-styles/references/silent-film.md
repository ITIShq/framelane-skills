# Silent film

Early cinema before synchronized sound: grainy black-and-white footage under a soft vignette, slightly hurried movement, iris transitions, and dialogue carried by intertitle cards of pale serif text in a ruled frame on black. The signature cue is the intertitle between shots.

- **Palette:** monochrome only. Black `#0B0B0B` and paper white `#EFE9DC` for cards and frames; a light sepia tone is the one permitted tint, and there are no color accents.
- **Type:** `Playfair Display` or `Lora`, 64 to 88 px on a 1920-wide canvas, centered, a dozen words at most per card, inside a double-ruled frame.
- **Timing:** cards hold for their reading time plus a second; shots last three to six seconds; footage runs at a `speed` of 1.1 to 1.25 for the hand-cranked feel; shots change with an iris or a dip to black.

## Making it in FrameLane

- Set `frame_rate` to 24.
- Grade each clip with `saturation` at -100, `contrast` around 15 and `vignette` around 45, then add `sepia` and `add_grain` to its `effects`.
- Silence the clips with `muted`: the period had no synchronized sound, and sped-up speech sounds wrong. A piano bed, if wanted, is an audio element from the person's own library.
- Join clips placed back to back with an `iris` or `dip_to_black` transition. Transitions join video elements only, so a card simply appears between shots.
- A card is centered text on the black background, framed by two stroked rectangles.

## Where it goes wrong

- Flicker faked with `strobe_light`: it reads as a fault and can trouble viewers who are sensitive to flashing. Let the grain carry the texture.
- Crushed blacks that hide faces: check a dark shot in a server frame and lower `contrast` before adding more grain.
- A modern sans serif, a lower third or a color accent: each of these breaks the period.
- Wordy cards: split a long line across two cards.

A complete ten-second example:

```json
{
  "width": 1920, "height": 1080, "duration": 10, "frame_rate": 24,
  "output_format": "mp4", "background_color": "#0B0B0B",
  "elements": [
    {
      "type": "shape", "id": "card-frame-outer", "path": "M2 2 H1182 V662 H2 Z", "view_box": [0, 0, 1184, 664],
      "fill": "none", "stroke": "#EFE9DC", "stroke_width": 3,
      "time": 0, "duration": 4, "x": "50%", "y": "50%", "width": "61.667%", "height": "61.481%"
    },
    {
      "type": "shape", "id": "card-frame-inner", "path": "M18 18 H1166 V646 H18 Z", "view_box": [0, 0, 1184, 664],
      "fill": "none", "stroke": "#EFE9DC", "stroke_width": 1.5,
      "time": 0, "duration": 4, "x": "50%", "y": "50%", "width": "61.667%", "height": "61.481%"
    },
    {
      "type": "text", "id": "intertitle", "text": "And so it began.",
      "time": 0, "duration": 4, "x": "50%", "y": "50%", "width": "54%", "height": "30%",
      "font_family": "Playfair Display", "font_size": 80, "text_color": "#EFE9DC",
      "motion": [{"type": "fade", "time": 0, "duration": 0.4}]
    },
    {
      "type": "video", "id": "shot-1", "source_url": "https://cdn-assets.framelane.io/shared/videos/clip1.mp4",
      "time": 4, "in_point": 0, "out_point": 3.6, "speed": 1.2, "muted": true,
      "x": "50%", "y": "50%", "width": "100%", "height": "100%",
      "saturation": -100, "contrast": 15, "vignette": 45,
      "effects": [{"type": "sepia", "intensity": 35}, {"type": "add_grain", "intensity": 45}]
    },
    {
      "type": "video", "id": "shot-2", "source_url": "https://cdn-assets.framelane.io/shared/videos/clip1.mp4",
      "time": 7, "in_point": 6, "out_point": 9.6, "speed": 1.2, "muted": true,
      "x": "50%", "y": "50%", "width": "100%", "height": "100%",
      "saturation": -100, "contrast": 15, "vignette": 45,
      "effects": [{"type": "sepia", "intensity": 35}, {"type": "add_grain", "intensity": 45}]
    }
  ],
  "transitions": [{"type": "iris", "duration": 0.8, "from_id": "shot-1", "to_id": "shot-2"}]
}
```
