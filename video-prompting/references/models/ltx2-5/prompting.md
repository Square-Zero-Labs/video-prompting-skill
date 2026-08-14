# LTX-2.5 Prompting Guide

Use this reference when the user asks for an LTX-2.5 prompt.

Official sources:

- LTX-2.5 prompt guide: `https://ltx.io/blog/ltx-2-5-prompt-guide`
- LTX-2.5 model and automatic-duration reference: `https://docs.ltx.io/models/ltx-2-5`
- LTX-2.5 model overview: `https://ltx.io/model/ltx-2-5`
- Official inference repository: `https://github.com/Lightricks/LTX-2`

## Contents

- [Overview](#overview)
- [Choose the Prompt Form First](#choose-the-prompt-form-first)
- [Duration and Beat Planning](#duration-and-beat-planning)
- [Core Content](#core-content)
- [Single-Shot Prompting](#single-shot-prompting)
- [Multi-Shot Prompting](#multi-shot-prompting)
- [Screenplay-Style Prompting](#screenplay-style-prompting)
- [Image-to-Video](#image-to-video)
- [Audio and Dialogue](#audio-and-dialogue)
- [Dub-It](#dub-it-speech-replacement)
- [Video Editing IC-LoRA](#video-editing-ic-lora)
- [Known Weaknesses](#known-weaknesses)
- [Output Formatting](#output-formatting)
- [Final Check](#final-check)

## Overview

LTX-2.5 rewards focused cinematic direction and supports both continuous single shots and native multi-shot scenes. Write concrete visual and audio instructions in chronological order. Use explicit cut and transition language only when the user wants multiple shots.

The model has stronger prompt adherence and can predict an appropriate clip duration from the prompt in supported workflows. Treat duration as an external setting; never write the duration or model name/version into the final prompt.

## Choose the Prompt Form First

Select the form that matches the request:

- **Single continuous shot:** Default for one take, intimate performance, smooth camera movement, lip-synced dialogue in one framing, and image-to-video.
- **Multi-shot scene:** Use for 2–4 connected shots with deliberate cuts and continuity across them.
- **Screenplay-style scene:** Use for dialogue-heavy scenes, multiple performance beats, or precise dramatic sequencing. Preserve readable scene headers, character cues, and dialogue line breaks.
- **Dub-It:** Use its dedicated speech-replacement format, not the generation format.
- **Video Editing IC-LoRA:** Use one concrete edit instruction, not a scene-generation prompt.

Do not force every prompt into a legacy `Style:` prefix or screenplay slugline. For a simple shot, state the shot and visual style naturally near the beginning. Use screenplay structure only when it improves dialogue or beat clarity.

## Duration and Beat Planning

- When the user asks for settings, has no fixed delivery length, and the interface supports it, recommend automatic duration as an external setting (`duration: null`). Otherwise, use automatic duration only as internal planning context and return the prompt alone.
- Let prompt complexity imply useful length: keep a one-action shot concise; allow a connected multi-shot sequence to contain more beats.
- When the user specifies a duration, fit the number of actions and cuts to it. Prefer readable beats over simultaneous micro-actions.
- When image-to-video uses a required last frame, use a fixed external duration. Automatic duration and last-frame conditioning cannot be combined.
- Never include duration, resolution, frame rate, aspect ratio, model variant, or generation parameter names inside the prompt.

## Core Content

Include the details that materially control the result:

1. **Shot:** Name the shot scale, angle, genre, or cinematic treatment.
2. **Scene:** Establish setting, lighting, color palette, textures, and atmosphere.
3. **Action:** Describe a natural beginning-to-end progression in present tense.
4. **Characters:** Give relevant age, hair, wardrobe, and distinguishing features. Show emotion through gaze, posture, expression, gesture, and delivery rather than abstract labels.
5. **Camera:** State how and when the camera moves relative to the subject, plus the resulting framing or reveal.
6. **Audio:** Describe relevant ambience, sound effects, music, speech, or singing in sync with the action.

Keep the scene focused, the lighting logic coherent, and every detail compatible with the same physical space and timeline.

## Single-Shot Prompting

- Write roughly 4–8 descriptive sentences as one flowing paragraph for a typical shot.
- Use present-tense, active language and chronological connectors such as `as`, `while`, `then`, and `after`.
- Match detail to framing: specify fine performance and material detail in close-ups; prioritize geography and movement in wide shots.
- Describe camera movement relative to the subject and say what the move reveals or how the framing ends.
- Keep one continuous soundscape, aligning sound effects and speech to visible actions.
- Add only requested dialogue. Put exact spoken words in quotation marks and state language, accent, tone, or vocal quality when relevant.

Recommended order:

`shot and style -> setting and lighting -> subject detail -> chronological action -> camera behavior and resulting frame -> synchronized audio`

## Multi-Shot Prompting

Use multi-shot only when the user wants actual cuts. Write the scene as one chronological paragraph or a short prose sequence, not a numbered shot list. Prefer 2–4 shots in one generation.

At every cut:

1. Name the transition in natural language, such as `A hard cut transitions to...`, `The view cuts to...`, `A match cut connects...`, or `The image dissolves into...`.
2. Re-establish shot scale, angle, subject placement, setting, and any intentional lighting change.
3. Re-identify recurring characters or objects with the same concise visual anchors.
4. State whether music, dialogue, sound effects, and ambience continue, change, or stop.

Give each shot one clear purpose, such as `establish -> detail -> reaction`. Preserve geography, wardrobe, identity, lighting, visual style, and voice unless the prompt explicitly motivates a change in time or place.

Do not confuse camera motion with editing: `the camera pans` preserves one take; `a hard cut transitions` creates a new shot.

## Screenplay-Style Prompting

Use screenplay-style formatting for sustained dialogue, multiple performance beats, or precise dramatic sequencing:

- Use a concise scene header when useful.
- Describe visible action and sound in present tense.
- Use character cues and exact quoted dialogue.
- Express emotion physically, not as an internal-state label.
- Keep each beat concrete; length may exceed 8 sentences when every line adds visual, performance, timing, or audio direction.
- If the scene also contains cuts, describe every cut explicitly in prose. A slugline alone does not reliably communicate an edit.

## Image-to-Video

- Treat the input image as the first-frame anchor for identity, composition, setting, and style.
- Describe what changes: subject movement, performance, environmental motion, camera behavior, and audio.
- Do not re-list static image details unless a detail must change or remain especially constrained.
- Prefer a single continuous take unless the user intentionally wants to cut away from the opening image.
- If a final frame is supplied, describe a plausible chronological transition and the intended ending state. Keep the fixed duration outside the prompt.
- If a dedicated camera control is used, keep the text direction compatible with it; do not request a conflicting move.

## Audio and Dialogue

- Build the soundscape from specific sources: room tone, weather, traffic, footsteps, machinery, score, speech, or singing.
- Place sound cues beside the actions that cause them.
- Put all spoken dialogue in quotation marks.
- Specify language and accent when relevant, and use pronunciation-friendly spelling for ambiguous terms.
- For multi-shot scenes, explicitly state audio continuity at each cut.
- For silent output, omit invented sound direction and recommend the external `generate_audio: false` setting only if parameters are requested.

## Dub-It (Speech Replacement)

Use Dub-It when the user supplies an existing speaking video and wants replacement dialogue. Return only:

`[Speaker] is speaking [language/accent], saying: "[full replacement dialogue]"`

Add delivery or emotion only when requested.

Requirements:

- Supply the complete replacement dialogue; Dub-It does not translate it automatically.
- Write the dialogue in the target language's native script.
- Keep to one speaker; the beta workflow does not distinguish multiple speakers.
- Match the replacement's timing and approximate syllable length to the source. Slightly longer is safer than substantially shorter; an overly long prompt may skip words, while a short one may sound slow.
- Officially validated languages listed in the guide are English, French, Spanish, German, and Russian.

## Video Editing IC-LoRA

Use one concrete instruction that names the desired visible change and what must remain unchanged. Phrase the target additively as the result to create, rather than relying only on a negation or removal request.

Template:

`[Desired edited state]. Preserve [identity, action, timing, camera motion, background, lighting, and other unchanged elements].`

Keep one edit per pass when possible. Do not pad the instruction with cinematography or scene details unrelated to the edit.

## Known Weaknesses

- **On-screen text:** Keep it short and prominent, but do not promise exact spelling or frame-to-frame stability. Recommend adding critical titles, labels, and logos in post.
- **Complex physics:** Prefer plausible, readable motion over highly chaotic interactions.
- **Crowded direction:** Avoid too many characters, simultaneous actions, or competing camera instructions.
- **Continuity gaps:** Avoid unexplained geography, wardrobe, lighting, identity, or voice changes across cuts.
- **Abstract emotion:** Replace labels like `sad` or `nervous` with observable performance cues.

## Output Formatting

- Return a simple single-shot, image-to-video, or multi-shot prompt as one paragraph with no heading by default.
- Preserve multiline screenplay formatting when dialogue or beat structure benefits from it.
- Return Dub-It as its one-line dedicated template.
- Return Video Editing IC-LoRA as one concise instruction.
- Put requested generation settings on a separate `Recommended parameters` line, never inside the prompt.

## Final Check

Before returning the prompt, verify that:

- the chosen form is single-shot, multi-shot, screenplay, Dub-It, or editing—not an accidental mixture;
- actions and sound progress chronologically;
- every multi-shot cut names the transition, new framing, recurring identity, and audio continuity;
- dialogue is exact and quoted;
- image-to-video describes change rather than redundantly describing the input;
- no model name, duration, aspect ratio, resolution, frame rate, or parameter name remains in the prompt text.
