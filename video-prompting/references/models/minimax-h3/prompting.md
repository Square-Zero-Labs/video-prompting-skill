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

## Motion graphics trailers

For fast motion-graphics trailers, treat the prompt as a compact audiovisual style bible followed by a precisely timed edit:

- Establish the global design system at the start of `[Shot 1]`: animation medium, line and shape language, rendering treatment, palette (including exact color values when supplied), tonal qualities, and exclusions. Keep those rules active across every shot instead of restating the whole style block.
- Define the hero, product, mascot, or brand device with stable identifying traits before the action. Reassert only the traits at risk during silhouettes, duplicates, transformations, extreme poses, or rapid cuts.
- Give each shot one legible graphic beat, such as an entrance, transformation, panel montage, emblem reveal, kinetic-type composition, or final lockup. Describe the incoming composition, the subject action, the graphic response, and the camera behavior in that order.
- Timestamp every cut and synchronize shape snaps, wipes, typography, impacts, lens or logo reveals, and musical accents to the visual action. A regular cut cadence is acceptable when it is an intentional part of the trailer's rhythm.
- Treat visible typography as a whitelist. Quote the exact allowed letters or words and explicitly forbid all other readable text when brand fidelity requires it.
- Describe potentially ambiguous motifs by their unmistakable visual construction. For example, define a photographic lens through curved glass, concentric optics, and aperture blades, then exclude reticles or targeting marks when those would create the wrong reading.
- Keep `overall_soundscape` focused on shot-synchronized whooshes, impacts, mechanical cues, character sounds, and transition effects. Use `non_diegetic_music` to define the trailer's rhythmic engine, instrumentation, build, and final hit.

Use the complete T2VA example at `references/models/minimax-h3/example_prompts/motion_graphics_trailer.md` as a pattern for a tightly art-directed, high-shot-count character trailer. Adapt its structure and level of control; do not copy its character, palette, branding, timings, or sound design unless the user requests them.

## Dialogue, singing, and visible text

- Assign stable speaker IDs `(S1)`, `(S2)`, and so on in order of first vocal event. Do not assign IDs to silent characters. Use `(S1,S2)` for speakers vocalizing together.
- On first vocal appearance, establish enough visual and vocal traits to keep the speaker stable.
- Put only the language tag and exact user-provided words inside `<d>`; preserve their language, wording, and punctuation: `<d>[English] Exact words.</d>`. Never invent or rewrite requested dialogue.
- For voiceover, use `says in an off-screen voiceover` and immediately after `</d>` state that the corresponding on-screen character's lips remain completely closed.
- When speech crosses a cut, place `<scenetrans>` at both connecting points and explicitly state that audio continues across the cut. Use `<cutoff>` when the video ending truncates speech.
- Put visible signs, banners, labels, subtitles, and other on-screen text in English double quotation marks, preserving the original text exactly.

## Sound fields

When the user explicitly requests complete silence, a muted video, or no audio of any kind, write `N/A` for both `overall_soundscape` and `non_diegetic_music`. Do not add ambience, dialogue, singing, sound effects, or music anywhere else in the prompt. Treat requests such as no music, no ambience, or no sound effects as layer-specific rather than as complete silence.

Write `overall_soundscape` as one continuous paragraph of 1–4 English sentences covering ambience, physical action sounds, and non-verbal human sounds. Do not repeat dialogue, singing, or diegetic music from the timeline. Use `N/A` only when the user explicitly requests complete silence.

Write `non_diegetic_music` as 1–3 English sentences describing audience-only score through instrumentation, tempo/rhythm, and dynamic development. Put music audible to characters in the timeline instead. Use `N/A` when there is no audience-only music.

For complete silence:

```text
overall_soundscape: N/A
non_diegetic_music: N/A
```

For no music only, retain any requested ambience and physical sounds:

```text
overall_soundscape: [requested ambience and physical sounds]
non_diegetic_music: N/A
```

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

### Assess reference suitability

- Prefer clean, well-lit reference footage with one unambiguous action when motion, choreography, or performance transfer is important.
- Match framing to the transfer: use full-body source footage and a full-body target reference for body motion, and tight facial views for expression or acting transfer.
- Prefer source and target subjects with compatible body plans and proportions; expect drift when transferring precise motion between substantially different shapes.
- Keep this as optional asset-preparation advice outside the generated prompt. Do not insert reference-quality commentary into prompt prose or reject a usable asset; mention it only when the user asks how to improve the inputs or a clear mismatch is likely to prevent the requested transfer.

### Define references

- `<Subject N>`: Reusable visible content such as a person, object, environment, clothing, style, action, expression, pose, performer/object motion, or choreography. Cite its source picture/video in its definition.
- `<Picture N>`: A concrete first frame, keyframe, last frame, edited keyframe, composition anchor, or storyboard reference. Do not define a standalone picture when it only supplies a subject.
- `<Video N>`: A whole source video used for editing, continuation, or temporal/editing structure. Visible content extracted from it remains a `<Subject N>`.
- `<Audio N>`: A copied or referenced audio signal, including soundtrack, voice timbre, dialogue, music style, beat, or sound texture. Do not create one merely because a reference video contains audio.

When a reference video supplies performer/object motion or choreography to reproduce, adapt, or transfer, always represent it in a `<Subject N>` definition sourced from the video. Incorporate it into the target performer/object's subject definition when it is an attribute of that subject; give it a separate subject label only when the motion or choreography must be tracked or transferred independently. Use `<Video N>` in addition when the whole source video controls editing, continuation, camera movement, cuts, editing rhythm, shot order, or other whole-video temporal structure. Treat referenced camera motion as a `<Video N>` relationship, not as a motion/choreography subject, and do not use `<Video N>` as a substitute for referenced performer/object motion.

```text
<Subject 1> is the dancer whose appearance comes from <Picture 1> and whose choreography comes from <Video 1>.
<Video 1> provides the target video's camera movement, cuts, and pacing structure.
```

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

Write one line per reference label. For visible references, including motion or choreography subjects sourced from video, use only `fully_preserved`, `partially_preserved`, `attribute_transfer`, or `weak_reference`. For audio, use only `fully_copy`, `partially_copy`, `reference`, or `weak_reference`. State the shots/role and concretely explain what is retained, changed, transferred, copied, or loosely followed.

### Write the detailed description

- Establish style in one or two sentences before `[Shot 1]`; unlike base mode, use `detailed_description`, not `integrated_multimodal_description`.
- Follow all base shot, camera, dialogue, visible-text, and sound rules.
- Cite reference labels naturally at first appearance and wherever their role takes effect. For a motion/choreography subject, cite its `<Subject N>` label where the referenced movement takes effect and let the reference video supply the movement's actual timing, sequence, rhythm, body mechanics, spatial path, and performer interactions. Describe only intentional adaptations, target-subject mapping, or synchronization constraints; do not redundantly reconstruct the choreography beat by beat unless the user requests explicit changes. Describe other referenced traits, frame relationships, source-video states, or audio relationships at the moment they take effect.
- For camera movement supplied by `<Video N>`, cite the video where the move takes effect and let it supply the actual path, speed, amplitude, and timing. Describe the target subject, scene, composition, and intentional adaptations; do not redundantly restate the camera move unless the user requests a change to it.
- For generation tasks, normally write 350–500 English words, while prioritizing a complete dialogue timeline and appropriate detail over mechanically meeting the range. Scale editing descriptions to source complexity.
- Use `<Subject N> (Sx)` when a referenced subject physically vocalizes. Use `<Audio N>` without a speaker ID for words heard only inside a directly reused soundtrack/BGM. Write `[unclear]` rather than guessing unintelligible source speech.
- State copied or referenced audio relationships in the matching audible layer: ambience/effects in `overall_soundscape`, audience-only score in `non_diegetic_music`, and dialogue/shot-synchronized audio in `detailed_description`.

## Final compliance check

- Preserve the exact schema, field order, line breaks, labels, and required blank lines; MiniMax H3 overrides the skill's single-line default.
- Keep model name/version, resolution, aspect ratio, and API/control parameter names out of prompt prose.
- Include duration only in required alignment instructions and shot timestamps. Supply any other generation settings separately only if requested.
- Ensure shot times increase and remain within the duration.
- Ensure keyframe paths actually begin from, connect to, or land on the supplied frame as the selected mode requires.
- Ensure every separately tracked reference label has exactly one definition, one `retention_analysis` entry, and at least one use where its role takes effect. Remove inactive labels and resolve conflicting roles without changing a label's meaning between sections.
- Allow a source-only picture or video citation to appear inside another item's definition without its own standalone definition or retention entry; do not mistake that compact source citation for an unresolved label.
- Preserve dialogue, lyrics, and visible text exactly in their original language; write all other content in English.
