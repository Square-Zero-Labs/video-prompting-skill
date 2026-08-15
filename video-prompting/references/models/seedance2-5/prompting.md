# Seedance 2.5 prompting

Use this reference when the user asks for a Seedance 2.5 prompt.

Sources:

- ByteDance Seed model page and official showcase prompts: `https://seed.bytedance.com/en/seedance2_5`
- fal prompting guide with ten generated examples: `https://fal.ai/learn/devs/seedance-2-5-prompting-guide`
- fal model overview and endpoint comparison: `https://fal.ai/learn/tools/what-is-seedance-2-5`
- Dreamina prompt guide: `https://dreamina.capcut.com/seedance/seedance-2-5-prompt`
- Dreamina motion-reference guide: `https://dreamina.capcut.com/seedance/seedance-2-5-motion-reference-guide`

Treat endpoint limits and settings below as fal-specific unless the user confirms another provider. Treat the ByteDance model page as the authority for base-model capabilities.

## Contents

- Capabilities and routing
- Clarify the job
- Build the prompt
- Direct time and continuity
- Direct references
- Direct camera, performance, physics, and audio
- Apply mode-specific guidance
- Use templates
- Refine a miss
- Check the result

## Capabilities and routing

Build around the capabilities ByteDance emphasizes for Seedance 2.5:

- up to 30 seconds in one generation
- joint audio-video generation
- precise visual, video, and audio reference control
- stronger long-form motion and continuity
- video extension and targeted editing
- production-oriented blocking, including white-model and green-screen references where the platform supports them

Choose the input mode before drafting:

- **Text-to-video:** create the subject, environment, action, camera, style, and sound from text.
- **Image-to-video:** use a still as the opening frame; use an end frame too when the provider supports it and the landing composition matters.
- **Reference-to-video:** use indexed image, video, and audio assets to control identity, design, environment, motion, timing, or sound.
- **Edit:** state what to preserve and the one localized change to make.
- **Extend:** continue from the supplied clip or its extracted final frame without replaying completed action.

On fal, text-to-video, image-to-video, and reference-to-video are separate endpoints. The available settings include `4` to `30` seconds or `auto`, `480p` or `720p`, native audio, and aspect ratios from `21:9` to `9:16`. Reference-to-video accepts at most `30` images, `10` videos, and `10` audio files, with no more than `50` files total. Keep these values outside the final prompt unless the user's platform requires otherwise.

## Clarify the job

Ask only for missing information that changes the prompt materially:

1. Confirm text-to-video, image-to-video, reference-to-video, edit, or extend.
2. Ask for the effective duration. Use it to allocate beats, but keep the duration outside the final prompt.
3. Confirm a continuous take or a sequence with cuts.
4. Ask what reference assets exist and what each must control.
5. Confirm dialogue, ambience, effects, music, or intentional silence.
6. Ask for the desired end state when the shot contains several actions, a transition, product handling, or a continuation.

When duration is missing, do not automatically fill 30 seconds. Recommend the shortest duration that fits the action. A simple movement often works better as a short shot; use longer clips only when the brief contains enough ordered behavior to occupy them.

## Build the prompt

Write a short production note rather than a keyword pile. Include only sections that solve a real control problem in the shot.

For a simple shot, use:

1. subject and setting
2. one action arc
3. shot size and one camera move
4. lighting and one coherent visual treatment
5. sound
6. final image

For a complex or long shot, use:

1. **Format:** continuous take or planned cuts; natural speed or a specific speed effect
2. **Reference roles:** one narrow responsibility per asset plus exclusions
3. **Starting state:** positions, object ownership, environment state, and camera position
4. **Timeline:** chronological beats that inherit the prior beat's physical state
5. **Camera:** framing, screen-space placement, path, triggers, and stopping point
6. **Continuity:** the few identities, objects, counts, directions, and visual properties that cannot drift
7. **Audio:** dialogue, room tone, contact sounds, music, or silence
8. **Ending state:** the exact final arrangement and whether to hold it
9. **Constraints:** only specific likely failure modes

Prefer concrete visible direction over adjectives. Replace `dynamic`, `epic`, or `cinematic` with observable choices such as subject placement, lens feel, camera height, light direction, motion speed, performance, and sound.

Keep one coherent style and one lighting logic. Do not stack conflicting aesthetics or incompatible light sources.

## Direct time and continuity

Use timestamps when several events must occur in order, actions exchange physical state, dialogue must fit, or the ending must land at a specific point. Omit timestamps for a simple short action.

Treat timestamps as approximate allocations, not frame-accurate commands. Make adjacent blocks cover the whole externally selected duration without gaps or overlaps.

Make each beat begin from the result of the prior beat:

- Record which hand holds each object.
- Record whether a door, lid, container, or garment is open, closed, attached, or removed.
- Preserve object counts and identities after transfers.
- Preserve travel direction and screen direction.
- State how an action settles instead of stopping at peak motion.

Write cause before reaction. Separate contact, resulting motion, sound, and human response. For example: the tray contacts the cup handle; the cup tips; the ceramic strikes the counter; the customer hears it and looks down.

Handle occlusion explicitly. State how a subject enters the obstruction, how long it remains hidden when timing matters, and which identity, clothing, object, speed, and direction must reappear on the other side. Do not allow the hidden interval to become a reset point.

End deliberately. Specify the subject and object positions, camera composition, residual motion, and hold. Do not leave unused time after the final described action.

## Direct references

Assign each reference one narrow job. Use the provider's exact reference labels, commonly `@Image1`, `@Video1`, and `@Audio1`.

Use images primarily for appearance or layout:

- identity or character design
- product shape and materials
- wardrobe or props
- environment, lighting, or storyboard composition
- exact opening or ending frame

Use video primarily for time-based behavior:

- camera path and framing rhythm
- performance or choreography
- mechanism or product operation
- motion timing

Use audio primarily for:

- voice identity or delivery
- dialogue timing
- music rhythm
- ambience or sound design

When a supplied audio reference contains the dialogue that should be spoken in the scene, treat that audio as the sole source of the spoken content. Do not quote, transcribe, summarize, or paraphrase its words anywhere in the prompt. Instead, direct the named speaker to perform or lip-sync the dialogue from the indexed audio reference, preserving its wording, timing, voice, and delivery. State that the model must not generate replacement or additional speech.

Name both the desired transfer and the forbidden transfer:

`@Image1 controls only the product geometry, materials, and color. Do not copy its background, pose, lighting, text, or camera angle.`

`@Video1 controls only the low parallel camera path and real-time motion rhythm. Do not copy its subject, wardrobe, props, or location.`

Avoid making an image and a video compete for the same job. Prefer a small, purposeful reference set over filling every available slot. Verify upload order before returning indexed labels.

For storyboard or white-model input, map each frame or proxy object to its role and describe the intended shot size, blocking, entrances, exits, and camera route. Treat rough geometry as spatial guidance, not as the final material or style.

## Direct camera, performance, physics, and audio

### Camera

Specify composition as well as motion:

- initial shot size and camera height
- subject position in the frame
- movement path and speed
- visible event that starts the move
- event or mark where it stops
- whether to preserve the action axis

Prefer one motivated camera idea per beat. For a continuous take, describe a physically connected route. For cuts, name each shot and transition instead of combining incompatible moves.

### Performance and dialogue

Block acting into observable actions, gaze, pauses, breath, and restrained facial changes. Avoid vague emotional labels when a performance note can show the emotion.

When dialogue must be generated from text, put exact spoken lines in straight double quotes. State who speaks, when the line begins and ends, what the speaker does before and after it, and who stays silent. Keep dialogue short enough for the allocated time. Use a dedicated block for an important line rather than stacking complex movement over it.

When supplied audio already contains what should be said, never include those words in the prompt, even if the user provides a transcript. Refer only to the indexed audio asset, for example: `The woman lip-syncs the dialogue from @Audio1 exactly, matching its timing and delivery; @Audio1 is the sole dialogue track, with no generated replacement or additional speech.`

### Physical motion

Break motion into contacts and transfers of force:

- approach
- contact or weight shift
- resulting motion
- recovery or settling state

For liquids, fabric, hair, smoke, particles, and wheeled objects, state the force direction, contact surface, and final state. Replace `realistic physics` with the actual physical sequence.

### Audio

Treat audio as part of the same timeline:

- name room tone and environmental ambience
- name contact sounds at the action that causes them
- specify music style and when it enters, changes, or stops
- say `no music` when only production sound is wanted
- say `silent` or `faint room tone only` when silence is intentional

Avoid redundant sound layers. Keep generated sound restrained when the clip will sit under an external voiceover or licensed track.

## Apply mode-specific guidance

### Text-to-video

Define every visual anchor the model must invent: subject, environment, time of day, light, action, camera, style, audio, and end state. Use chronological blocks for longer scenes.

### Image-to-video

Animate; do not rewrite. Treat the image as the authority for identity, wardrobe, scene, palette, and opening composition. Describe motion, camera, performance, physics, sound, and deliberate changes.

When an end image is available, describe the path between the frames and the final action state. Do not overconstrain every intermediate pose.

### Reference-to-video

Define every reference role before the timeline. Repeat only the invariants essential to later beats. Use indexed labels exactly as the platform exposes them.

### Edit

State the preserved context first, then the target region or element, then the requested change. Leave successful camera, timing, identity, and lighting instructions untouched. Avoid asking for a full redesign when only one localized correction is needed.

### Extend

Use the previous clip's exact final frame as the opening image when possible. State what has already finished, what remains fixed, and the first new action. Do not narrate or replay the earlier clip.

## Use templates

Use a single line for a simple shot. Use the multiline schema for a complex timeline; Seedance 2.5 is an exception to the skill's default single-line output when structure improves temporal control.

### Simple shot

`[Subject] in [setting and light] [performs one action arc]. [Shot size and one motivated camera move]. [Coherent visual treatment]. [Audio]. End on [specific final image and residual motion]. [Only necessary constraints].`

### Complex timed shot

```text
FORMAT: [continuous take or cuts; natural speed or intentional speed effect]
STARTING STATE: [subject positions, object ownership, environment, camera]
TIMELINE:
[0-Xs]: [first action]
[X-Ys]: [next action beginning from the prior result]
[Y-Zs]: [final action and settling state]
CAMERA: [framing, subject placement, path, triggers, stopping point]
CONTINUITY: [essential identity, object, count, direction, wardrobe, and geometry invariants]
AUDIO: [dialogue, ambience, contact sounds, music or silence]
ENDING STATE: [exact final composition and hold]
CONSTRAINTS: [specific cuts, repetitions, duplicates, transfers, text, logos, or artifacts to prevent]
```

Do not write the duration, aspect ratio, resolution, or model name into `FORMAT`. Use the externally selected duration only to replace the timestamp placeholders.

### Reference-driven shot

```text
REFERENCE ROLES:
@Image1 controls only [appearance or layout invariant]. Do not copy [unwanted properties].
@Video1 controls only [camera, performance, mechanism, or rhythm]. Do not copy [unwanted subjects, objects, or setting].
@Audio1 controls only [voice, timing, music, or ambience].
STARTING STATE: [state anchored by the references]
TIMELINE: [ordered beats]
CAMERA: [composition and route]
CONTINUITY: [invariants]
AUDIO: [how supplied and generated sound should interact]
ENDING STATE: [final frame]
CONSTRAINTS: [specific likely failures]
```

### Continuation or edit

`Use @Image1 as the exact opening frame. Preserve [successful identities, objects, layout, light, camera, and completed state]. Do not replay [completed action]. [Ordered continuation or localized change]. End with [new exact state]. [Audio]. [Specific constraints].`

## Refine a miss

Change one variable at a time:

- If action order fails, rewrite the starting state and the transition between the affected beats.
- If continuity drifts, shorten and repeat only the essential invariant list.
- If a reference leaks unwanted content, narrow its role and add explicit exclusions.
- If the camera fails, preserve the successful action and rewrite only composition, trigger, path, or stopping mark.
- If dialogue overlaps action, give the line its own block and state when mouths remain closed.
- If the ending drifts, reserve a final settling block and define the last frame.
- If a simple shot stretches or repeats, shorten the external duration instead of inventing filler.

Do not rewrite the whole brief when most of the result works.

## Check the result

Before returning the prompt, verify:

- Match beat complexity to the selected duration.
- Cover the complete timeline without contradictory actions.
- Make each beat inherit physical state from the prior beat.
- Specify cause before reaction and contact before motion.
- Assign each reference one job and exclude unwanted transfers.
- Give the camera composition, motivation, and stopping point.
- Preserve essential identity, wardrobe, props, counts, directions, and object ownership.
- For text-generated dialogue, give the dialogue a speaker, quoted line, time allocation, and performance.
- For supplied dialogue audio, omit its spoken text and refer only to the indexed audio as the sole source of wording, timing, voice, and delivery; prohibit replacement or additional speech.
- Give fluids, fabric, particles, and impacts a settling state.
- Define the exact ending frame or continuation state.
- Use only targeted negative constraints.
- Remove the model name, version, duration statement, aspect ratio, resolution, and API settings from the final prompt text; keep timestamp blocks when needed for temporal control.
