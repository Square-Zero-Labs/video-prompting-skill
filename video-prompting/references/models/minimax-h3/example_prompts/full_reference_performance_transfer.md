# Full-reference exact performance transfer

Use this example as a pattern when a target image supplies two stylized characters and a reference video supplies two performers whose facial acting, lip sync, reactions, and audio should transfer one-to-one without speaker swapping or visual-style drift.

The example assumes:

- `<Picture 1>` contains two illustrated target characters in fixed left/right positions.
- `<Video 1>` contains two source performers in corresponding fixed left/right positions.
- The reference video supplies facial performance, subtle head/upper-body motion, timing, turn-taking, and reactions.
- The target image supplies character identity, wardrobe, composition, environment, and illustration style.
- `<Audio 1>` and `<Audio 2>` are the copied vocal tracks for the left and right source performers respectively.

Adapt labels and ownership rules when the actual workflow exposes the audio differently.

```text
subject_definitions:
<Subject 1> is the LEFT target character whose complete visible identity, face design, hair, clothing, accessories, body proportions, seated position, and left-side placement come from <Picture 1>. <Subject 1> is speaker (S1).
<Subject 2> is the RIGHT target character whose complete visible identity, face design, hair, clothing, accessories, body proportions, seated position, and right-side placement come from <Picture 1>. <Subject 2> is speaker (S2).
<Subject 3> is the complete performance of the LEFT source performer in <Video 1>, including exact lip articulation, jaw motion, facial expressions, eye movement, blinks, eyebrow motion, subtle head motion, restrained upper-body conversational movement, listening reactions, speaking reactions, pauses, and timing. <Subject 3> transfers exclusively to <Subject 1> (S1).
<Subject 4> is the complete performance of the RIGHT source performer in <Video 1>, including exact lip articulation, jaw motion, facial expressions, eye movement, blinks, eyebrow motion, subtle head motion, restrained upper-body conversational movement, listening reactions, speaking reactions, pauses, and timing. <Subject 4> transfers exclusively to <Subject 2> (S2).
<Subject 5> is the complete illustrated visual style and environment established by <Picture 1>, including its 2D medium, clean controlled line art, stylized facial construction, cel-shaded and painted tonal modeling, simplified illustrated skin and hair treatment, color palette, decorative rendering, lighting treatment, and background environment. <Subject 5> is mandatory for every frame.
<Audio 1> is the copied vocal performance belonging only to the LEFT source performer in <Video 1> and maps exclusively to <Subject 1> (S1), preserving the exact voice, words, pronunciation, cadence, pauses, breaths, timing, and vocal inflection.
<Audio 2> is the copied vocal performance belonging only to the RIGHT source performer in <Video 1> and maps exclusively to <Subject 2> (S2), preserving the exact voice, words, pronunciation, cadence, pauses, breaths, timing, and vocal inflection.

summary:
[reference generation + audio reuse] Generate <Subject 1> and <Subject 2> from <Picture 1>, fully preserving <Subject 5>. Transfer <Subject 3> exclusively to <Subject 1> (S1) and <Subject 4> exclusively to <Subject 2> (S2). Copy <Audio 1> only to S1 and <Audio 2> only to S2. Performer ownership remains fixed for the entire target video.

retention_analysis:
<Subject 1>: fully_preserved — Preserve the LEFT target character's identity, face design, hair, clothing, accessories, proportions, and fixed left-side role from <Picture 1>. Do not import the appearance of either source-video performer.
<Subject 2>: fully_preserved — Preserve the RIGHT target character's identity, face design, hair, clothing, accessories, proportions, and fixed right-side role from <Picture 1>. Do not import the appearance of either source-video performer.
<Subject 3>: attribute_transfer — Transfer the LEFT source performer's lip sync, facial acting, eye behavior, blinks, head motion, listening reactions, and conversational micro-gestures only to <Subject 1> (S1).
<Subject 4>: attribute_transfer — Transfer the RIGHT source performer's lip sync, facial acting, eye behavior, blinks, head motion, listening reactions, and conversational micro-gestures only to <Subject 2> (S2).
<Subject 5>: fully_preserved — Preserve the same illustrated medium and rendering language in every frame. <Video 1> supplies performance only; do not transfer photographic skin, realistic hair, live-action lighting, source-performer facial identity, or other source-video rendering traits.
<Audio 1>: fully_copy — Preserve the left source performer's vocal material and timing exactly and assign it only to <Subject 1> (S1).
<Audio 2>: fully_copy — Preserve the right source performer's vocal material and timing exactly and assign it only to <Subject 2> (S2).

detailed_description:
The target video remains a living version of the illustration in <Picture 1>. Preserve <Subject 5> in every frame: stable drawn contours, stylized facial proportions, illustrated skin and hair, consistent cel-shaded or painted modeling, the same palette, and the same illustrated environment. Do not reinterpret the characters as live-action people, realistic 3D humans, or photorealistic recreations. <Video 1> contributes performance only, not identity or visual rendering.

[Shot 1] Keep <Subject 1> (S1) as the LEFT target and <Subject 2> (S2) as the RIGHT target. Their performer mapping remains fixed for the entire video:

LEFT source performer → <Subject 3> → LEFT target <Subject 1> (S1) → <Audio 1>.
RIGHT source performer → <Subject 4> → RIGHT target <Subject 2> (S2) → <Audio 2>.

Do not cross-map these relationships during speech, silence, reactions, interruptions, head turns, pauses, or overlapping dialogue.

Whenever the LEFT source performer speaks, only <Subject 1> (S1) physically vocalizes <Audio 1>. S1 follows <Subject 3> for phoneme-driven mouth shapes, coarticulation, jaw motion, lip compression and rounding, facial expression, gaze, eyelid behavior, blink timing, eyebrow motion, subtle head movement, breathing-related motion, and restrained conversational micro-gestures. During those moments, <Subject 2> (S2) remains non-speaking and follows only the concurrent listening or reaction performance from <Subject 4>. S2 must not mirror S1's mouth motion or lip-sync <Audio 1>.

Whenever the RIGHT source performer speaks, only <Subject 2> (S2) physically vocalizes <Audio 2>. S2 follows <Subject 4> for the same categories of speech articulation, facial acting, eye behavior, head movement, and micro-gestures. During those moments, <Subject 1> (S1) remains non-speaking and follows only <Subject 3>'s concurrent listening or reaction performance. S1 must not mirror S2's mouth motion or lip-sync <Audio 2>.

If the source performers overlap, allow both targets to vocalize only for the exact duration of the corresponding overlap: S1 stays synchronized only to <Audio 1> and <Subject 3>, while S2 stays synchronized only to <Audio 2> and <Subject 4>. When neither source performer speaks, neither target performs generic talking motion; preserve only the mapped silent reactions, blinks, gaze changes, breathing, and subtle head or body movement.

All transferred facial animation remains native to <Subject 5>. Mouth shapes, eyes, brows, cheeks, and jaw motion should animate through stable illustrated contours and shading rather than realistic facial reconstruction. Preserve both target identities, clothing, hair, accessories, proportions, environment, and left/right roles. Avoid speaker swapping, cross-mapped lip sync, mirrored speech animation, random mouth movement, unrelated blinking, identity drift, wardrobe drift, position swapping, photorealistic skin, photographic hair, live-action lighting, or gradual style drift.

overall_soundscape:
Fully preserve the copied source-video vocal audio. <Audio 1> belongs only to <Subject 1> (S1), and <Audio 2> belongs only to <Subject 2> (S2). Preserve exact timing, pauses, interruptions, overlaps, breaths, cadence, pronunciation, and vocal delivery. Do not reassign either speaker's audio or generate replacement dialogue.

non_diegetic_music:
Preserve audience-only music only if it is already part of the referenced source audio and intended to be copied; otherwise use no additional non-diegetic music.
```

## Single-performer adaptation

For one speaker or singer, collapse the mapping to one target, one performance subject, and one audio reference. Explicitly state that the mapped target is the only visible subject allowed to vocalize, and that all background characters remain non-vocal and must not inherit the primary performer's lip sync or facial-performance track.

For singing, make the source performer authoritative for sustained vowel shapes, phrase timing, breath placement, expressive eye behavior, blinks, facial tension, tiny head movements, and other micro-gestures. Preserve the target illustration style in the same way as the two-performer example.
