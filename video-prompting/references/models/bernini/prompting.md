# Bernini prompting

Use this reference when the user asks for a Bernini prompt.

Sources reviewed:

- Project page: `https://bernini-ai.github.io/`
- Paper: `https://arxiv.org/abs/2605.22344`
- GitHub repo and bundled test cases: `https://github.com/bytedance/Bernini`

## What Bernini is optimized for

Bernini is a unified generation and editing framework with an MLLM-based semantic planner and a DiT renderer. The planner reasons over text, source images, source videos, and target placeholders, then the renderer synthesizes the result from semantic guidance plus visual features. Prompting should therefore read like a precise semantic edit or generation plan, not like a short tag list.

Bernini supports text-to-video, video editing, motion-changing video editing, reference-guided video editing, content insertion, and reference-to-video. It can use multiple visual inputs, so prompts should explicitly name which source or reference each detail comes from.

## Choose the right internal mode

Ask only for missing information needed to understand the prompt type:

- `t2v`: text-to-video generation from text only.
- `v2v`: video editing when the source video motion mostly stays the same.
- `mv2v`: video editing when the main subject's motion or pose must change.
- `rv2v`: reference-image-guided video editing, such as replacing a garment or material from a reference image.
- `ads2v`: video insertion / content insertion from a reference video into a source video.
- `r2v`: reference-to-video generation from one or more reference images.
- `i2i` / `t2i`: image tasks exposed by the repo; use only when the user asks for Bernini image generation/editing.

Use these labels only as internal drafting context. Do not output routing labels, guidance modes, model names, duration, resolution, frame rate, command-line flags, or case-file fields unless the user explicitly asks for implementation details.

## Prompt structure

Write one dense but readable natural-language instruction. Prefer 2-4 sentences or short paragraphs.

Use this order:

1. Main instruction: what should be generated or changed.
2. Strong positive description of the target result: style, material, rendering language, color, lighting, texture, and visible transformation.
3. Subject and source anchors: which person/object/environment/reference supplies each detail.
4. Motion and temporal behavior: start state, progression, continuation, and smoothness.
5. Minimal preservation constraints only for details that must not drift.

For editing tasks, do not over-index on preservation. Bernini is strong at preserving source identity, pose, and layout, so a prompt dominated by "keep unchanged" language can cause the output to under-edit. Lead with what must visibly change, describe the new visual state in concrete detail, and keep preservation to one short clause at the end unless the user specifically needs a surgical local edit.

For style-transfer edits, be especially forceful about the style change:

- Say the whole source should be re-rendered or transformed, not lightly filtered.
- Name the visual features that must replace the source look.
- Name the source look that must disappear, such as photoreal skin texture, camera noise, live-action fabric, or documentary lighting.
- Use preservation language only for performance timing, layout, and identity continuity.

## Mode-specific guidance

### Text-to-video (`t2v`)

Use a fully specified cinematic prompt:

- Shot size and composition.
- Subject appearance and materials.
- Environment, time of day, weather, lighting.
- Continuous motion for the subject and environment.
- Camera behavior if needed.

Keep the action coherent as one clip. Bernini examples use concrete motion such as paws strumming, body swaying, snowflakes drifting, or head movement rather than abstract mood language.

### Video editing (`v2v`)

Use `v2v` when the source motion remains intact and the edit adds, removes, restyles, or changes an object while the original performance continues.

Prompt pattern:

`[Strong edit instruction]. [Detailed target appearance and visible replacement of the original look]. [Motion/contact/stability details if needed]. [Brief preservation clause for identity, timing, layout, or unrelated objects].`

Be explicit about object placement and spatial relation. If adding an object, describe where it sits in frame, how it contacts the ground or host surface, and how it should remain stable over time.

For whole-video style changes, focus more on the new style than the source preservation. Describe the transformation across people, props, background, lighting, surfaces, shadows, and motion accents. Include an explicit "no remaining [old style]" clause when the model is preserving too much of the original look.

### Motion-changing video editing (`mv2v`)

Use `mv2v` when the edit changes a person's or object's action, pose path, or body mechanics. Preserve identity and scene anchors while describing the new motion in physical terms.

Include:

- The new pose or action.
- Body mechanics and balance.
- Contact with the ground or interacted objects.
- Which source-video details remain unchanged.

### Reference-guided video editing (`rv2v`)

Use `rv2v` when one or more reference images provide object, material, clothing, weather, or style guidance for editing a source video.

Refer to the supplied reference plainly:

- `the shirt from the reference image`
- `the metal texture from image1`
- `the stormy sky from image2`

Explain how the reference detail adapts to the moving source: fit, drape, scale, perspective, reflections, shadows, and motion.

### Content insertion (`ads2v`)

Use `ads2v` when inserting a reference video or moving content into a target video. Strong prompts should identify:

- The source video that receives the insert.
- The reference video or image content to insert.
- The target surface or region.
- Perspective, occlusion, lighting, playback, and temporal stability.

### Reference-to-video (`r2v`)

Use `r2v` when generating a new video from up to five reference images. Label source roles explicitly with `image0`, `image1`, etc., matching the provided reference order.

Prompt pattern:

`The [main subject] from image0, wearing/holding/using [object] from image1, in the [environment] from image2, [action] in [shot framing]. Keep [specific identity/material/environment anchors] unchanged while adding [motion].`

When multiple references are composited, specify which reference controls subject identity, wardrobe/props, background, and style. Repeat the key mappings if they are important.

## Preservation language

Bernini editing prompts can use explicit "keep unchanged" clauses, but keep them short. Use preservation language as a constraint, not as the center of the prompt.

Use preservation clauses for:

- Subject identity, face, hair, body proportions, wardrobe items not being edited.
- Existing source-video motion when not intentionally changed.
- Camera framing, lens feel, and perspective.
- Lighting direction, shadow softness, contact shadows, reflections.
- Background, ground texture, unrelated objects, weather, and atmosphere.
- Temporal stability: no flicker, drifting, scale changes, or sliding contact.

Do not over-preserve the exact thing the user wants to change. If the edit is a global style transformation, avoid long lists of unchanged visual details because those details may pull the output back toward the original source style. For `mv2v`, do not ask to keep the old motion unchanged; preserve identity and scene instead.

## Final output format

Output only the final prompt text. Do not add recommended routing, guidance mode, parameters, JSON, or implementation notes unless the user explicitly asks for them.

## Examples

### Text-to-video

`Medium shot, center composition. A large white polar bear sits upright on a snowbank holding a wooden acoustic guitar, its right paw steadily strumming the strings while its heavy body sways side to side and its head nods gently. Snowflakes drift across a bright icy landscape under a deep blue sky, with crisp daylight shadows on the snow and subtle highlights on the polished guitar.`

### Add object to source video

`Add a realistic snowman on the right side of the snowy path, positioned in the mid-right ground beside the trail without blocking the dog. Match the overcast winter lighting, snowy atmosphere, perspective, soft shadows, and ground contact, with the snowman's base partially embedded in the snow and no flicker, drifting, or scale changes. Keep the dog, road, trees, bushes, distant post, source camera framing, and overall winter scene unchanged.`

### Strong whole-video style change

`Completely transform the video into a high-energy 2D anime rock concert sequence. Replace the live-action look with bold ink outlines, clean cel shading, expressive anime faces, stylized hair, sharp animated costume shapes, illustrated guitars, bass, drums, microphones, amps, stage lights, smoke, shadows, and background details. Push the style change strongly with saturated concert colors, dramatic rim lighting, painted spotlight beams, manga-style motion accents, simplified anime fabric folds, crisp graphic shadows, and a polished high-budget music-anime finish. The result should look hand-animated from scratch, with no photorealistic skin texture, live-action fabric texture, realistic stage surfaces, camera noise, or documentary concert-video feel remaining.`

### Change source-video motion

`Change the person's motion so the person crouches naturally beside the seated dog, lowering into a balanced squat with bent knees while staying on the right side of the frame and leaning slightly toward the dog. Preserve the same person, black winter jacket, black pants, boots, camera framing, daylight, mountain background, ground texture, shadows, and the dog's seated position. The new motion should be smooth and realistic, with stable body proportions and consistent contact with the ground.`

### Reference-to-video

`The marble statue from image0, wearing the headphones from image1 and the shirt from image2, sits on the bench in the beach sunset setting from image3, centered in a medium shot facing the camera. Keep the statue's carved marble body, sculpted hair, and stone material from image0, adapt the clothing and headphones naturally to its body, and preserve the bench, ocean, palm trees, promenade, and warm sunset colors from image3. The statue gently nods its head, sways its upper body, and bounces subtly to music while remaining seated and facing the camera, with smooth playful motion and no deformation.`
