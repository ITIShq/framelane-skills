<!-- Copied from api/mcp/skills/craft/layout-and-safe-areas.md (sha256 1bec714ff660ac33fc98448b675f73e9ff7e9c5dbe35e5cf96efa8b525d9c8b2) by scripts/build_skills_bundle.py. Edit the source, not this copy. -->

# Layout and safe areas

For positions, boxes, margins, alignment and the regions platform controls cover.

## Core

- `x` and `y` place an element's center as canvas percentages. `width` and `height` are a box around that center, in percentages or pixels.
- Video and images fill their box and crop the overflow; they are never letterboxed. To show a source whole, match the box to its aspect: box width % = source aspect × height % × canvas height / canvas width.
- One dominant element at a time. Cover media sits below text and captions; set `z_index` only when array order is not the stacking you want.
- Keep essential content inside the working region for the canvas. Platform controls move, so these regions are conservative recipes, not official safe zones.
- Check the whole box of a logo, disclosure or caption, not its center point. `OUT_OF_FRAME` reports a box that leaves the canvas.

## Working regions

| Canvas | Keep essential content | Notes |
| --- | --- | --- |
| 1920 × 1080 | inside the central 84% of the width and 80% of the height | a 7 to 8% outer margin suits titles and diagrams |
| 1080 × 1920 | inside x 8 to 92% and y 12 to 80% | reserve the bottom 18% for platform controls; captions sit around 74% height within 82% width |
| 1080 × 1080 | inside an 8% inset on every side | |

## Alignment

- Left-aligned text starts at its box's left edge, which is `x` minus half the `width`. Elements share a column when their left edges match, not when their `x` values do.
- Derive boxes from a simple grid, such as thirds or twelve columns inside the margins, so edges relate to each other.
- When a headline wraps badly, widen its box within the working region first, then break the line on purpose, then shorten it.

## Shapes and cards

- A card, bar or pill is a `shape` with a rectangular `path` and a `fill`. `corner_radius` is a fraction from 0 to 1, where 1 is a pill. Text cannot be rounded: put it on a rounded shape.
- Keep a path's `view_box` aspect equal to its box's pixel aspect, or the path stretches and strokes thicken along one axis (`SHAPE_ASPECT_MISMATCH`).
- `stroke_width` is in view-box units and scales with the box. A stroke thinner than a pixel drops out (`HAIRLINE_BELOW_PIXEL`).

## Groups and stacks

- A group moves its members as one. A `stack` layout arranges equal items in a `row` or `column` with a normalized `gap` and owns their positions, so move the stack rather than a member.
- Group translations are normalized: the same numbers travel further on a larger canvas.

## Changing the canvas

- Recompose instead of scaling: a portrait version usually needs larger relative type, fewer words and stacked blocks. `skills.reframe` covers crops and contained footage.
