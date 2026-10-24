# Scenes and layouts

How a cleaned talking-head recording becomes a run of scenes, each with a title and a layout. Work on the cut timeline: build scenes from kept speech only, and place everything in timeline seconds mapped through the pieces that play each word (`craft.speech-editing`).

## Scenes

- One idea per scene, told in 3 to 10 sentences.
- Leave at least 15 seconds between scene starts. A would-be scene that starts sooner is not a scene of its own: demote its title to a sub-heading inside the scene before it.
- The one shorter scene is a full-screen quote card, which holds a single sentence.
- The first scene starts at timeline second 0, and its title is the video's title.
- Plan the scenes as a numbered list that names each scene's first and last sentence. Before building, check that every sentence named exists in the kept speech, that no scene number repeats, and that the scenes run in ascending order without overlapping. When a check fails, repair the plan; never drop the failing scene, and never renumber the scenes after it.

## Titles

- A title is 1 to 6 words in one of three styles: curiosity ("The habit nobody films"), descriptive ("Lighting a small room") or prescriptive ("Script the first line"). Keep one style through the video unless the person asks for another.
- Never "Introduction" or "Conclusion", and no question or exclamation mark.
- Write titles in the video's language, with names and terms spelled as the person's vocabulary spells them.
- A title style the person asks for applies to this project only; it is part of this project's direction once they accept it, not a rule for their other videos.
- A title enters in the pause before its scene's first word. When that pause lasts 1 second or more, the title enters 1 second before the speech; otherwise it enters as the speech starts. Either way it stays for at least its words/3 + 1 seconds (`craft.typography`).

## Layouts

Choose a layout per scene from what the scene says and what the frame already shows. Write the reasoning first, one line per scene: the kind of content (story, demonstration, facts, steps, list, recap, quotable line), what the footage shows, and so the layout. Only then build.

| Layout | Use it for | In FrameLane |
| --- | --- | --- |
| Presenter only | footage that carries the information itself: a screen share, a demonstration, full-frame B-roll | the scene's video pieces full frame; its title, if shown, as a short plated label |
| Presenter + title | stories, openings, moments with little information | full-frame video, with the title as a plated text element in the upper or lower part of the working region |
| Presenter + text panel | information-rich speech: facts, figures, how-tos, lists; not when the footage itself is informative | the scene's video pieces in one half of the frame and a raised-surface panel in the other, holding the title and two to four short lines |
| Full-screen quote card | one quotable sentence | an opaque full-frame shape above the video, with that sentence in large type; the voice keeps playing underneath |
| Text slide | recaps, calls to action, information with nothing to look at | the same cover shape, with a title and up to four short lines |

The on-screen text setting shifts the mix:

- **Presenter-first:** mostly presenter only and presenter + title; panels only for figures and lists.
- **Balanced** (the default): panels for the information-rich scenes, a quote card for the strongest line or two, and a text slide for a recap or a call to action.
- **Text-heavy:** panels for most scenes that carry information, quote cards for strong lines and slides for recaps; the presenter alone only when the footage is the point.

## Building a layout

- Text on panels and slides quotes or condenses the kept speech of that scene, in its language. A spoken list becomes a lead-in line and one short line per item.
- Colors, sizes and plates come from `craft.color-and-brand`, `craft.typography` and `craft.layout-and-safe-areas`, or from the project's direction.
- Video fills its box and crops the overflow, so a 16:9 recording in a half-frame box shows only about the middle half of the frame. Check that the speaker is inside it, and aim the crop at them with the crop arithmetic in `skills.reframe`.
- The presenter's box can change only where one piece ends and the next begins. When a panel scene starts or ends in the middle of a piece, split the piece in the pause at that boundary: a `cut` that removes two frames from the middle of the pause, taken from the word times on either side. Then set the box on the scene's pieces with `set_fields`.
- On a portrait canvas, stack instead of splitting the width: the presenter's pieces above, the panel below.

This landscape example shows three scenes after a cleanup. The first is presenter + title, carrying the video's title; the second is presenter + text panel, with the presenter's piece in the left half; the third is a quote card over the last piece, whose sound keeps playing. The source is illustrative.

```json
{
  "width": 1920, "height": 1080, "duration": 36, "frame_rate": 30,
  "output_format": "mp4", "background_color": "#111827",
  "elements": [
    {"id": "talk", "type": "video", "source_url": "https://example.com/talk.mp4", "time": 0, "in_point": 1.5, "out_point": 17.5, "x": "50%", "y": "50%", "width": "100%", "height": "100%", "volume": 100},
    {"id": "talk~18200", "type": "video", "source_url": "https://example.com/talk.mp4", "time": 16, "in_point": 18.2, "out_point": 34.2, "x": "25%", "y": "50%", "width": "50%", "height": "100%", "volume": 100},
    {"id": "talk~35000", "type": "video", "source_url": "https://example.com/talk.mp4", "time": 32, "in_point": 35, "out_point": 39, "x": "50%", "y": "50%", "width": "100%", "height": "100%", "volume": 100},
    {"id": "scene-1-title", "type": "text", "text": "Three habits that slow editing", "time": 0, "duration": 4, "x": "50%", "y": "17%", "width": "70%", "height": "14%", "font_family": "Inter", "font_size": 88, "font_weight": 700, "text_color": "#FFFFFF", "background": true, "background_color": "#111827DD", "z_index": 2, "motion": [{"type": "fade", "time": 0, "duration": 0.4}, {"type": "fade", "time": 3.6, "duration": 0.4, "reversed": true}]},
    {"id": "scene-2-panel", "type": "shape", "path": "M0 0 H100 V100 H0 Z", "fill": "#1F2937", "time": 16, "duration": 16, "x": "75%", "y": "50%", "width": "50%", "height": "100%", "z_index": 1},
    {"id": "scene-2-title", "type": "text", "text": "Plan the first line", "time": 16, "duration": 16, "x": "75%", "y": "28%", "width": "42%", "height": "12%", "font_family": "Inter", "font_size": 64, "font_weight": 700, "text_color": "#FFFFFF", "z_index": 2, "motion": [{"type": "fade", "time": 16, "duration": 0.4}]},
    {"id": "scene-2-points", "type": "text", "text": "• Write the opening sentence\n• Say it twice before filming\n• Keep the later take", "time": 16.6, "duration": 15.4, "x": "75%", "y": "55%", "width": "42%", "height": "30%", "font_family": "Inter", "font_size": 44, "text_color": "#D1FAE5", "z_index": 2, "motion": [{"type": "fade", "time": 16.6, "duration": 0.4}]},
    {"id": "scene-3-cover", "type": "shape", "path": "M0 0 H100 V100 H0 Z", "fill": "#111827", "time": 32, "duration": 4, "x": "50%", "y": "50%", "width": "100%", "height": "100%", "z_index": 3},
    {"id": "scene-3-quote", "type": "text", "text": "The first sentence decides who stays.", "time": 32, "duration": 4, "x": "50%", "y": "50%", "width": "76%", "height": "36%", "font_family": "Lora", "font_size": 88, "text_color": "#FFFFFF", "z_index": 4, "motion": [{"type": "fade", "time": 32, "duration": 0.5}]}
  ]
}
```

Preview a settled frame of every layout before calling the structure done, and check the panel text against the speech it condenses.
