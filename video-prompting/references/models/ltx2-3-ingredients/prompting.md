# LTX-2.3 Ingredients IC-LoRA Prompting Guide

Use this reference when the user asks for LTX-2.3 Ingredients, the Ingredients LoRA, the IC-LoRA for ingredients, or LTX-2.3 reference-sheet control.

Sources:

- Model card: `https://huggingface.co/Lightricks/LTX-2.3-22b-IC-LoRA-Ingredients`
- Example Space: `https://huggingface.co/spaces/ltx-community/ltx-2.3-ingredients-distilled`

## Overview

This is not a general text-to-video prompt format. It is an IC-LoRA workflow for generating a video from a supplied reference sheet: one composite image that inventories the scene ingredients, such as characters, props, and location. The prompt must tell the model what is in the sheet and what the generated shot should do.

The model was trained to read a two-part prompt:

```text
Reference sheet: <description of the panels in the sheet: characters, props, location>

Generated video: <description of the action, camera, dialogue, and sound>
```

The reference sheet supplies identity and visual consistency. The `Generated video:` section drives action, performance, shot design, dialogue, and audio.
When multiple characters could be confused, prioritize character identity clarity over cinematic density.

## Required Inputs

- A reference sheet image, supplied externally as the IC-LoRA control input. In most native/Comfy workflows this is a static video made by looping the sheet to the output frame count; the public Space builds that static video from the uploaded sheet.
- A `Reference sheet:` prompt field that describes only elements actually present on that sheet.
- A `Generated video:` prompt field that describes the desired clip.

Do not use this guide if the user has no reference sheet or subject images. Ask for the sheet, or help them draft a reference-sheet image prompt first.

## Reference Sheet Requirements

The sheet should be a single composite image with clean, separated visual elements:

- Characters: include face close-up and full-body/turnaround details when available.
- Props: use product-style, uncluttered views for handled or recurring objects.
- Location: include one clean setting panel if the place must stay consistent.
- Background: black is preferred.
- Text: avoid text labels on the sheet.

Important elements should occupy meaningful panel space. Small or crowded elements are more likely to drift or disappear.

## Prompt Writing Rules

### Reference sheet section

Write a dense inventory of the visual anchors present in the sheet.

Include:

- Character names when the user provides them, immediately bound to stable visual identifiers: `Betty is the girl turtle: ...`; `Fred is the boy turtle: ...`.
- Each character's visible identity markers: age range, face, hair/fur, clothing, body shape, costume, expression, and distinctive features.
- Each prop's shape, material, color, scale, markings, and how it is likely to be handled.
- The location's layout, lighting, key structures, natural features, and atmosphere.

Do not invent elements absent from the sheet. If the generated shot needs a prop, character, or setting detail that is not in the reference, tell the user to add it to the sheet.
If two characters share a species, body type, costume family, or color palette, make the differences explicit and repeatable: name, role, eye color, clothing, accessory/no accessory, side of frame, and action.

### Generated video section

Write a complete shot direction that uses the listed ingredients. Favor the example Space's pattern:

1. Name the genre/style and shot scale.
2. Re-anchor each main character by name and visual identifier before their first action.
3. Describe action in a simple clear chronological sequence.
4. Include exact dialogue in quotation marks when speech matters.
5. Describe camera movement.
6. Describe audio: voices, environmental ambience, Foley, music, or "no music".

Use direct, concrete motion. Keep actions readable. Do not overload the clip with too many subjects moving at once. For multi-character scenes, repeatedly pair the name with the identifier at key moments: `Betty, the girl turtle in pink pajamas...`; `Fred, the boy turtle with no bow...`.

Keep the generated-video section simpler than the reference-sheet section by default. Use one shot, one main camera setup, one location, and one or two clear action beats unless the user asks for more complexity. This LoRA can confuse similar characters when the action prompt is too dense.

For dark or nighttime scenes, explicitly request readable lighting. Use phrases such as `bright cozy nighttime lighting`, `clearly lit by the warm bedside lamp and soft blue moonlight`, or `faces remain clearly visible`. Avoid making the room "very dark" unless darkness is the primary creative goal.

## Output Format

Return the final prompt as two labeled sections, exactly:

```text
Reference sheet: ...

Generated video: ...
```

This guide is an exception to the global one-line output rule because the LoRA was trained with these labels.

If the user asks for only one field, provide that field only and say which paired field is still needed.

If the user asks for parameters, provide them after the prompt under `Recommended parameters:`. Otherwise, do not include parameters.

## Example Patterns

Use these as style/structure guidance, not as templates to copy.

### Character + props + store location

```text
Reference sheet: Hazel is the friendly cartoon hedgehog: rounded brown fur, cream belly, large dark eyes and tiny ears, shown as a face close-up and upright full-body view; Ruby is the cheerful rabbit: grey-and-white fur, long upright ears and a fluffy tail, shown in a body turnaround; a green coiled garden hose on a wall reel; green trigger spray bottles; a bright home-and-garden store interior with plant shelves, terracotta pots, warm overhead lighting and a green storefront sign

Generated video: cheerful family-animation commercial scene, a simple medium shot inside the bright garden store. Hazel, the brown hedgehog with the cream belly, waddles toward the camera, waves a tiny paw and says in a warm sing-song voice: "welcome to Greenfield!". Ruby, the grey-and-white rabbit with long ears, stands behind Hazel holding the green spray bottle and gives one playful mist spray. the camera gently pushes in; bright clear store lighting, soft footstep sounds, one spray-bottle puff, cheerful voices and a light acoustic jingle
```

### Character + animal + mountain location

```text
Reference sheet: Mei is the young Asian woman: dark center-parted hair in two long braids, olive short-sleeved t-shirt, khaki cargo pants, hiking boots and a black wristwatch; a large blue external-frame hiking backpack with pouches and black straps; a rough wooden walking stick; Tashi is the shaggy yak: white-and-blonde fur, curved grey horns, ornate blue-red-yellow saddle blanket, metal stirrups and colorful tassels; a mountain dirt path with green rocky slopes, snow-capped peaks, blue sky, and a small traditional stone shrine with blue trim, red door and yellow roof valance

Generated video: cinematic adventure documentary scene, a steady medium-wide shot on the mountain path. Mei, the woman with two braids and olive shirt, sits beside Tashi, the shaggy yak with the colorful saddle blanket. Mei pats Tashi's neck and says gently: "we've got a long way to go... big guy." Tashi shifts its weight and the stirrups jingle. Mei picks up the wooden walking stick and looks toward the snowy ridge. the camera holds mostly steady with a small push-in; crisp sunlight, wind over the path, the yak's low breathing and no background music
```

## Troubleshooting Guidance

- Identity drift: simplify the generated-video prompt and explicitly bind every character name to a stable visual identifier in both sections.
- Character swaps: repeat name + identifier near each action, e.g. `Betty, the girl turtle with the aqua bow...` and `Fred, the boy turtle with no bow...`; assign stable sides of frame when useful.
- Missing prop: add a dedicated prop panel and mention it in `Reference sheet:`.
- Setting mismatch: add a location panel and describe it in the `Reference sheet:` section.
- Too dark: ask for brighter readable nighttime lighting, visible faces, and a practical light source such as a lamp or moonlight.
- Weak action: add clearer verbs, sequence, camera movement, and audio to `Generated video:`.
- Overstuffed result: reduce simultaneous character, prop, and camera motion.
