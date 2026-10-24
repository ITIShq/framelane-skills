# B-roll

Cutaway footage over the presenter's voice: how much to use, what earns it, and how to place it. Add B-roll only after the cut, against kept speech, and only from footage that really exists.

## How much

| Density | Share of the kept speech covered | Notes |
| --- | --- | --- |
| none | none | |
| minimal | about 10% | only lines whose picture is unmistakable |
| light | about 30% | |
| balanced | about 40% | the default |
| rich | about 60% | visual analogies become eligible |
| heavy | about 90% | |
| full | every eligible line | |

Quality comes before the quota. A strong edit often lands near 25% even when the target is higher; never fill the share with a weak match.

## What earns a clip

Good candidates show something concrete:

- the creator's own workflow, or a look behind the scenes;
- a physical action;
- a particular place or object;
- a process, step by step;
- data or results;
- people working or talking together;
- a visible reaction.

These stay on the presenter:

- the first and the last line;
- a self-introduction;
- branded products, proprietary interfaces and screen demonstrations: show the category generically instead, such as hands on a keyboard rather than a named app;
- opinions with no visual subject;
- transitions between topics, fillers and calls to action.

An abstract line gets B-roll only when the density needs it, and then in this order: a literal picture, a prop that stands for the idea, a visual metaphor. Give adjacent lines with distinct visuals separate clips, and split a long line that names two visuals into two clips.

## Rights

No logos, and no recognisable portrait of a named real person. Use a stand-in instead: hands, a silhouette, a crowd, or the place rather than the person.

## Timing

- Clips run 3 to 15 seconds. Group consecutive short lines until a clip reaches 3 seconds, and split a line longer than about 10 seconds that names two visuals.
- Adjacent clips either butt together or leave at least 2 seconds of presenter between them.
- Never put B-roll over a full-screen quote card or a text slide.
- Start a clip at the first kept word of its line, mapped onto the cut timeline through the piece that plays it (`craft.speech-editing`), and end it with the line.

## Footage

FrameLane does not search stock libraries or generate footage. Use clips from `list_assets` or URLs the person supplies. Check a workspace clip's length with `get_asset` before placing it, and preview a supplied URL before relying on it; a clip cannot play past its own source. When a slot has no footage, give the person one to three search phrases for it instead: plain English, from specific to general, naming a subject and an action and, when it matters, a setting, such as "hands sliding a fader on a mixing desk in a studio", "adjusting audio levels", "audio mixing". Never invent a URL.

A B-roll clip is a video element above the presenter's pieces, full frame and muted, so the voice keeps coming from the presenter. On a project after its cleanup,

```json
{"width":1920,"height":1080,"duration":30,"frame_rate":30,"output_format":"mp4","background_color":"#111827","elements":[{"id":"talk","type":"video","source_url":"https://example.com/talk.mp4","time":0,"in_point":2,"out_point":14,"x":"50%","y":"50%","width":"100%","height":"100%","volume":100},{"id":"talk~15400","type":"video","source_url":"https://example.com/talk.mp4","time":12,"in_point":15.4,"out_point":33.4,"x":"50%","y":"50%","width":"100%","height":"100%","volume":100}]}
```

this adds a five-second cutaway from a supplied clip while the speaker describes the mixing desk:

```json-edit
{"if_version":4,"author":"agent","run_id":"talking-head-broll","ops":[{"op":"add_element","element":{"id":"broll-mixing-desk","type":"video","source_url":"https://example.com/supplied/mixing-desk.mp4","time":13.2,"in_point":0,"out_point":5,"muted":true,"x":"50%","y":"50%","width":"100%","height":"100%","z_index":1}}]}
```
