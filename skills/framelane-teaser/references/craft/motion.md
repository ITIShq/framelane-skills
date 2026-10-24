<!-- Copied from api/mcp/skills/craft/motion.md (sha256 e591d85ae55fedad3970308b820c97a9b31f5f156d280892eea6f3615366f4b6) by scripts/build_skills_bundle.py. Edit the source, not this copy. -->

# Motion

For entrances, exits, loops and transitions: which preset works where, and how long it runs.

## Core

- Read `motions` before choosing a preset. Each row's flags say where it runs: element_entrance, element_exit, character (per glyph on text) and text_block (the whole text). A preset on the wrong surface is rejected, so never infer a name from an adjective in the brief.
- An entrance starts at its element's `time`. An exit ends with the element, so its `time` is the element's `time` plus `duration` minus the motion's `duration`.
- Entrances usually take 0.3 to 0.6 seconds, and exits the same or less. Stagger related elements by 0.1 to 0.2 seconds instead of moving everything at once.
- A text element takes one animation route: `motion`, `word_animation` or `animation_preset`. Adding `motion` to either of the others is rejected; `word_animation` with `animation_preset` passes lint and silently drops the preset.
- Move with purpose: one entrance per element, one motion idea per beat, and stillness while the viewer reads.

## Presets

- `reversed` plays the exit form, and only presets that ship both forms accept it: `fade`, every `slide_*`, `wipe_*` and `rotate_*` preset, and `bounce`. Exit-only presets have their own names: `zoom_out`, `evaporate`, `whip_down`, `drift_out`, `swing_out` and `elastic_drop`.
- `easing` on a motion entry is ignored, because each preset's curve is fixed. For another curve, or a move no preset makes, define it in `custom_animations` and bind it with `custom`.
- `scope` set to `character` needs a per-glyph preset; block presets such as `fade`, `slide_up` and `zoom_in` reject it. A `spacing` below 1 overlaps the glyph windows so a word lands faster.
- `preset` names a renderer animation directly, such as the "typewriter" text animation. An unknown name is rejected rather than silently skipped.
- A loop preset takes `duration` as one cycle. Run one ambient loop at a time.
- A `counter` animates as one block; per-glyph motion on it is rejected.

## Transitions

- `transitions` join two video elements only, placed back to back: the incoming clip's `time` is the outgoing clip's end. An image or text target is rejected, and each clip takes at most one incoming and one outgoing transition.
- To cross-fade stills, text or shapes, overlap them and fade one out while the other fades in.

## Groups

- A group's animations move, scale, rotate and fade its members together. Wipe and Ken Burns presets do nothing on a group: put them on the members.

## Three clocks

- Motion `time`, `trim_keyframes`, `path_keyframes` and caption word timings are output-timeline seconds.
- A `custom_animations` keyframe `time` is a fraction from 0 to 1 of the motion window that binds it.
- A counter's `start_time` is seconds into its own text element.

## Draw-ons

- A stroked shape draws itself with `trim` start and end at 0 and `trim_keyframes` taking `end` to 1. A fill appears at once, so a draw-on uses `fill` set to `none`.

## Cost and review

- `motion_blur` renders every frame `samples` times and falls back to software encoding, so the render takes roughly that many times longer. Keep it for fast moves that strobe without it.
- Judge motion from a short preview window, not from one still, and check a frame just after each entrance settles.
