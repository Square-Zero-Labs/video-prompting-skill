# MiniMax H3 prompting

Use this reference when the user asks for a MiniMax H3 prompt. Base the output format on MiniMax's official guides:

- Base T2VA / I2VA / FL2VA / L2VA guide: `https://huggingface.co/MiniMaxAI/MiniMax-H3/blob/main/docs/VIDEO_PROMPT_WRITING_GUIDE_base_en.md`
- Full-reference guide: `https://huggingface.co/MiniMaxAI/MiniMax-H3/blob/main/docs/VIDEO_PROMPT_WRITING_GUIDE_ref_en.md`

## Select the mode

- **T2VA**: Generate the full audiovisual timeline from text.
- **I2VA**: Use one image as the exact first frame and develop forward.
- **FL2VA**: Connect an exact first frame to an exact last frame.
- **L2VA**: Infer a plausible opening and converge on one image as the exact last frame.
- **Full-reference**: Use image, video, subject, or audio references for reusable content, editing, continuation, structure, or sound relationships.

Ask only for missing information that changes the schema: mode, reference roles, effective duration for FL2VA/L2VA or timed cuts, exact dialogue/lyrics, and whether non-diegetic music is wanted. For full-reference work, identify whether each asset is a concrete frame, reusable subject, source video, or audio source.

## Base-mode output schema

For T2VA, output exactly these three fields in this order:

```text
integrated_multimodal_description: [Shot 1] ...

overall_soundscape: ...

non_diegetic_music: ...
```

For keyframe modes, prepend the exact applicable instruction and one blank line:

```text
I2VA: For the target video, at 0.00 seconds into the target video, <Picture 1> (from [Shot 1]) is fully referenced.

FL2VA: How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot N) aligns with the S.SS-second mark of the target video.

L2VA: How the reference pictures align with the target video — <Picture 1> (from [Shot N]) aligns with the S.SS-second mark of the target video.
```

Replace `N` with the actual final shot number and `S.SS` with the effective duration to exactly two decimal places. The labels `I2VA:`, `FL2VA:`, and `L2VA:` above identify the alternatives and are not part of the prompt.

### Build the timeline

- Begin `[Shot 1]` with the visual style and initial composition. Do not timestamp it.
- Number later shots sequentially and begin each with a strictly increasing cut time inside the effective duration: `[Shot 2] At 00:03.500, the camera cuts to...`.
- Use a cut only when it adds new subject, spatial, state, viewpoint, or time information. Prefer camera movement for a small distance or angle change.
- Express camera movement naturally in the shot. Choose a motion type such as push/pull, zoom, pan, truck, tilt, pedestal, arc, tracking, static, shake, POV, or roll; add `with small/large amplitude` and `at slow/fast speed` only when meaningful.
- Keep every visual and audio event observable and chronological. Establish identities, positions, environment, lighting, actions, reactions, state changes, and synchronized diegetic sounds.

### Handle keyframes

- **I2VA**: Preserve Picture 1's identity, clothing, colors, objects, composition, and spatial relationships. Write the path as first-frame anchor → action onset → continuous development → result/reaction.
- **FL2VA**: Prefer one continuous shot unless the user specifies cuts. Describe observable intermediate changes that close the gap between Picture 1 and Picture 2; land exactly on Picture 2 at the end.
- **L2VA**: Infer a compatible preceding state, then explicitly converge character pose, objects, camera, lighting, and composition on Picture 1 in the final shot.

## Dialogue, singing, and visible text

- Assign stable speaker IDs `(S1)`, `(S2)`, and so on in order of first vocal event. Do not assign IDs to silent characters. Use `(S1,S2)` for speakers vocalizing together.
- On first vocal appearance, establish enough visual and vocal traits to keep the speaker stable.
- Put only the language tag and exact user-provided words inside `<d>`; preserve their language, wording, and punctuation: `<d>[English] Exact words.</d>`. Never invent or rewrite requested dialogue.
- For voiceover, use `says in an off-screen voiceover` and immediately after `</d>` state that the corresponding on-screen character's lips remain completely closed.
- When speech crosses a cut, place `<scenetrans>` at both connecting points and explicitly state that audio continues across the cut. Use `<cutoff>` when the video ending truncates speech.
- Put visible signs, banners, labels, subtitles, and other on-screen text in English double quotation marks, preserving the original text exactly.

## Sound fields

Write `overall_soundscape` as one continuous paragraph of 1–4 English sentences covering ambience, physical action sounds, and non-verbal human sounds. Do not repeat dialogue, singing, or diegetic music from the timeline. Use `N/A` only when the user explicitly requests complete silence.

Write `non_diegetic_music` as 1–3 English sentences describing audience-only score through instrumentation, tempo/rhythm, and dynamic development. Put music audible to characters in the timeline instead. Use `N/A` when there is no audience-only music.

## Full-reference output schema

Use full-reference mode when assets provide reusable subjects, frame anchors, source-video editing/continuation, audiovisual structure, or audio reuse/reference. Write all six sections in English, preserving original language only inside `<d>` and visible quoted text:

```text
subject_definitions:
...

summary:
...

retention_analysis:
...

detailed_description:
...

overall_soundscape:
...

non_diegetic_music:
...
```

### Define references

- `<Subject N>`: Reusable visible content such as a person, object, environment, clothing, style, action, expression, or pose. Cite its source picture/video in its definition.
- `<Picture N>`: A concrete first frame, keyframe, last frame, edited keyframe, composition anchor, or storyboard reference. Do not define a standalone picture when it only supplies a subject.
- `<Video N>`: A whole source video used for editing, continuation, or temporal/editing structure. Visible content extracted from it remains a `<Subject N>`.
- `<Audio N>`: A copied or referenced audio signal, including soundtrack, voice timbre, dialogue, music style, beat, or sound texture. Do not create one merely because a reference video contains audio.

Give each separately tracked item one definition line and keep labels stable across all sections. If audio maps to a target speaker, reuse that speaker's eventual ID in the definition, for example `<Audio 1> is the voice-timbre reference for <Subject 1> (S1).`

### Summarize task relationships

Begin `summary` with one bracketed combination of applicable fixed task types, joined by ` + ` without repetition:

- `keyframe completion`
- `reference generation`
- `video editing`
- `video continuation`
- `audio reuse`
- `audio reference`

Then write one short paragraph using only labels already defined. For video editing, begin after the prefix with `The target video is an edited version of <Video 1>.`

### Analyze retention

Write one line per reference label. For visible references, use only `fully_preserved`, `partially_preserved`, `attribute_transfer`, or `weak_reference`. For audio, use only `fully_copy`, `partially_copy`, `reference`, or `weak_reference`. State the shots/role and concretely explain what is retained, changed, transferred, copied, or loosely followed.

### Write the detailed description

- Establish style in one or two sentences before `[Shot 1]`; unlike base mode, use `detailed_description`, not `integrated_multimodal_description`.
- Follow all base shot, camera, dialogue, visible-text, and sound rules.
- Cite reference labels naturally at first appearance and wherever their role takes effect. Describe the referenced traits, frame relationship, source-video state, or audio relationship at that moment.
- For generation tasks, normally write 350–500 English words, while prioritizing a complete dialogue timeline and appropriate detail over mechanically meeting the range. Scale editing descriptions to source complexity.
- Use `<Subject N> (Sx)` when a referenced subject physically vocalizes. Use `<Audio N>` without a speaker ID for words heard only inside a directly reused soundtrack/BGM. Write `[unclear]` rather than guessing unintelligible source speech.
- State copied or referenced audio relationships in the matching audible layer: ambience/effects in `overall_soundscape`, audience-only score in `non_diegetic_music`, and dialogue/shot-synchronized audio in `detailed_description`.

## Final compliance check

- Preserve the exact schema, field order, line breaks, labels, and required blank lines; MiniMax H3 overrides the skill's single-line default.
- Keep model name/version, resolution, aspect ratio, and API/control parameter names out of prompt prose.
- Include duration only in required alignment instructions and shot timestamps. Supply any other generation settings separately only if requested.
- Ensure shot times increase and remain within the duration.
- Ensure keyframe paths actually begin from, connect to, or land on the supplied frame as the selected mode requires.
- Preserve dialogue, lyrics, and visible text exactly in their original language; write all other content in English.
