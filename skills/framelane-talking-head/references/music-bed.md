# Music bed

One instrumental track under the whole video, kept well below the voice. FrameLane does not generate music: the bed is a track the person owns, from their workspace (`list_assets`) or a URL they supply, and confirming the right to use it is theirs.

## Choosing the track

- The person's request wins, whether it names a track, a mood, or no music at all.
- Otherwise write a one-sentence brief, in English, from the opening minute of kept speech and the scene titles. Give the mood in two or three adjectives, one to three instruments, the tempo or the way the energy moves, and a theme tied to the topic. For example: "Calm and quietly hopeful, felt piano and brushed drums at a walking pace that lifts for the last scene, for a video about building a morning routine."
- Before sourcing a new track, look for a close match among the person's assets and reuse it.
- Use one bed for the whole video, not a new track for each scene.
- When the recording already carries music, such as a sting or a song played on camera, leave those stretches uncut, because a cut inside music jumps audibly, and keep the bed out of them.

## Placing it

- Add the bed after the cut. A ripple shortens any audio element that spans a removed range, so a bed placed earlier would end early.
- Speech sits around volume 100; start the bed around 12 to 18 and listen. There is no automatic ducking.
- Set the fades explicitly: about half a second in and a second out, each shorter than the clip.
- When the track is shorter than the video, repeat it as explicit clips with their own `in_point`, `out_point` and `time`. A longer `duration` never extends a source.

On a cleaned project of 30 seconds,

```json
{"width":1920,"height":1080,"duration":30,"frame_rate":30,"output_format":"mp4","background_color":"#111827","elements":[{"id":"talk","type":"video","source_url":"https://example.com/talk.mp4","time":0,"in_point":2,"out_point":32,"x":"50%","y":"50%","width":"100%","height":"100%","volume":100}]}
```

this adds a bed from a supplied track for the whole length:

```json-edit
{"if_version":5,"author":"agent","run_id":"talking-head-music","ops":[{"op":"add_element","element":{"id":"music-bed","type":"audio","source_url":"https://example.com/supplied/calm-piano.mp3","time":0,"in_point":0,"out_point":30,"volume":14,"fade_in_duration":0.5,"fade_out_duration":1}}]}
```

Listen to a window where the voice is quiet and one where it is loud before calling the balance right; a still frame cannot tell you either.
