<!-- Copied from api/mcp/skills/craft/speech-editing.md (sha256 a2bbf5186d6db4c282fe330d1aa87de4a94552ab0c6f09ce607c4dae9ccd0e7b) by scripts/build_skills_bundle.py. Edit the source, not this copy. -->

# Speech editing

For cutting recorded speech by its transcript: which words go, where each cut lands, how silence is trimmed, and how source seconds become timeline seconds.

## Core

- Decide in text, then place in time. First choose the words that go, by their index in the transcript. Only then turn the choice into cuts, and let the server work out the times from the transcript's own word timings.
- Deletion only. A cleanup removes whole words exactly as they were spoken. It never rewrites, reorders or adds a word, not even to repair an obvious recognition error: that is a caption fix (`skills.caption`), made in caption text.
- One `cut` op in `words` mode carries the decisions. Send it with `dry_run` first, read its report against your decisions, then send the same body without `dry_run`.
- Measure every length from word times or from what the server returns, never from a number restated earlier in the conversation. A word's start and end, a stored media duration and the pieces of a dry run are evidence; a figure you or a model wrote down earlier is not.
- When a phrase is said twice, the later take stays. A cut never enters a word that stays.
- Scenes, titles, captions and B-roll come after the cut and are placed on the cut timeline, so they only ever see kept speech.

## The cut op

- Transcribe with `disfluencies` on, so fillers such as "um" come back as timed words with an index of their own, and with `speaker_labels` on when more than one person talks. A word's index is its position in the result's word list, counted from 0.
- In `words`, `transcript_task_id` names that task, and `remove` or `keep` lists index ranges `[i, j]`: both ends included, in ascending order, each word once. An index is a whole number; the server refuses a string rather than guessing what it meant.
- The server converts the transcript's millisecond timings to source seconds, clamps words the recogniser let overlap, and turns each range into a padded source range (see Where cuts land).
- `ids` lists the targets: the recording's video and, when its sound is a separate audio element, that element too, so both are cut in step. A target is cut only inside its own source window; a range outside every target is skipped and reported once, on the first target.
- `ripple` `all`, the default, closes every gap: later elements move up, an overlay that spans a cut gets shorter, caption words are re-timed and the request `duration` shrinks. A caption only loses the words that were cut, and goes once none are left, but any other element that would sit wholly inside a removed range makes the op fail unless `contained` is `remove`.
- `remove` in source seconds is for a sound with no word index, such as a cough or a knock on the microphone. Take its edges from the word times on either side, and give it a `category` and a `reason`; both come back in the report. `keep`, in seconds or as word ranges, inverts the selection.
- Every new join gets a short audio fade, `join_fade`, 15 ms unless you change it, so a cut through a waveform does not click.
- Sending the same cut again changes nothing, because what it removed no longer plays. Putting a moment back is an explicit edit of the pieces, never a re-run.

## Reading the dry run

The dry run returns the would-be diff, the lint and `cut_report`, with one entry per target:

- `pieces`: the target split at the cuts. The first keeps the element's id; each later one is named after the element and the second its source starts at, in milliseconds. A piece keeps its name for good: when a later cut takes its head, it starts later than its name says, so read where a piece starts from its `in_point`.
- `removed`: each source range taken out, after merging and snapping, with its category and reason. A word range comes back labelled words, with its indices and text; a shortened pause comes back labelled silence. Ranges that touch come back as one, with both labels when a word range meets a pause; the categories you decided stay in your own notes.
- `removed_seconds`: how much shorter the target got on the timeline.
- `skipped_ranges` and `warnings`: a range outside the clip or shorter than a frame, a removal left in because it sits too close to a word that stays, or a mean word confidence under 0.8, which calls for listening to the joins.

Before applying, every decision should appear among the removed ranges or be explained by a warning.

## Where cuts land

- A removal leaves 10% of each neighbouring pause beside the words that stay, and never less than 20 ms; it takes the rest. The rule is the same for one filler and for a whole sentence, so a dropped sentence leaves a short gap made of what remains of the pauses around it.
- When the words that stay sit so close that nothing is left to cut once those margins are kept, the removal is left in and the report says so. Decide again in text: drop the removal, or widen it by a whole word.
- Boundaries snap inward to the frames of the output, where the clip plays them at its speed, so a removal never grows past what was asked for and every join lands on a frame. Ranges that touch or overlap merge first; a removal under one frame is then skipped, and a kept piece under two frames goes with its neighbours.
- A sentence ends at . ! ? 。 ！ or ？, and the last word always ends one, so the final words are never treated as an unfinished sentence.

## Silence

Trim silence last, in the same op, with `silences`. It shortens long pauses between the words that stay and leaves a pad on each side, so speech never starts or stops abruptly:

- the start is cut to just before the first word, and the end keeps 0.25 s after the last word;
- a gap of 1.1 s or more between sentences keeps 0.25 s on each side;
- a pause of 0.5 s or more inside a sentence keeps 0.1 s on each side.

Those are the server's defaults. Let the person choose a mode:

| Mode | What to send |
| --- | --- |
| none | leave `silences` out |
| ends only | `silences` with `within` and `between` set longer than the recording, so only `trim_ends` acts |
| standard | `silences` as an empty object, which takes the defaults |
| custom | the person's own `within`, `keep_within`, `between` and `keep_between` |

Only the part of a pause that still plays counts, so a second silence pass changes nothing. Where words are removed, the pause left at the join is judged like any other pause, so the long gap a dropped sentence can leave is shortened too. A transcript without end punctuation has a single sentence end, at its last word, so each of its pauses is judged as a pause inside a sentence.

## Long transcripts

- Decide in windows of about 150 sentences. Put each boundary at a long pause near that mark, and let every window read a margin of a few sentences beyond both edges while deciding only its own sentences. A retake that starts at the end of one window and finishes in the next is then seen whole.
- Merge the windows' decisions by word index. No window decides words outside its own range, and merging never joins distant parts of the recording into one clip.

## Source time and timeline time

For a clip at constant speed, a source second plays at

timeline second = `time` + (source second − `in_point`) / `speed`

and the clip lasts (`out_point` − `in_point`) / `speed` on the timeline. After a cut each piece is a clip of its own: map a kept word through the piece whose source window holds it. A source second inside no piece was cut.

For example, keep source seconds 12 to 18, place the clip at timeline second 2, and play it at speed 1.25. It lasts 4.8 seconds and ends at 6.8, and a word spoken from source 13 to 13.5 plays from 2.8 to 3.2.

- Convert the transcript's milliseconds to seconds once, before the formula. Never infer a unit from how large a number looks: a long recording and a short clip overlap numerically.
- Clip a word that crosses a piece's edge to the piece, drop words wholly outside it, and keep their order. Never produce a negative start or a word of zero length.
- Caption cue windows and word intervals are timeline seconds, not offsets from the clip's in point.
- A speed ramp breaks the formula, and the cut op refuses a ramped target: cut first, then ramp the pieces.

## Lengths

- A stretch of speech lasts from its first word's start to its last word's end. An excerpt's length is the sum of its kept sentences, each measured that way, not the distance from its first word to its last, which would count whatever was cut between them.
- A standalone excerpt, such as a short, runs 20 to 60 seconds of kept speech.
- Once a dry run has made the pieces, their lengths are the authority: their sum is the clip's new length.

## Chinese, Japanese and Thai

- These scripts do not separate words with spaces. Wherever a rule counts words, such as a title's length, a caption line or a reading time, count characters instead, and judge the result on a server frame: non-Latin rendering is unverified (`craft.typography`).
- When you rebuild text from word indices, join the words without spaces.
- The recogniser may return single characters or short groups as words. An index is still an index, so cuts work the same way.
