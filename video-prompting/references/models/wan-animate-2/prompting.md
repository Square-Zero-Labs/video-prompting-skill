# Wan Animate 2 prompting

Use this reference when the user asks for Wan Animate 2, Wan-Animate-2, `Wan2.2-Animate-2-14B`.

Sources:

- Official repository and inference examples: `https://github.com/Wan-Video/Wan-Animate-2`
- Official paper: `https://arxiv.org/abs/2608.06009`
- Community input-alignment discussion: `https://www.reddit.com/r/comfyui/comments/1utn1va/official_wan_22_animate_character_replacement/`

## Understand the conditioning roles

Wan Animate 2 is not a conventional text-to-video or image-to-video model. At generation time it uses:

- **Character reference image:** anchors character appearance, clothing, and the image background.
- **Driving video:** supplies the performance, including motion, timing, facial expression, and fine hand movement.
- **Main prompt (`prompt`):** objectively captions the reference image; it is not the primary motion script.
- **Motion prompt (`prompt_ref`):** separately identifies the driving video as the motion reference.

Do not rewrite the driving performance as action beats. Do not add camera moves, gestures, or expressions merely because they occur in the driving video. The model consumes that video directly.

The public Wan Animate 2 repository exposes character animation. Do not promise the older Wan Animate `replacement` mode unless the user's particular interface explicitly provides it.

## Gather the input

Ask for or inspect only the character reference image. It is enough to write both prompts. The user does not need to provide the driving video to the agent; they will supply it separately to Wan Animate 2 when generating.

If the image is unavailable, draft from the user's description and state that the main prompt assumes those details. Ask about the target interface only when the user wants generation parameters or fields beyond `prompt` and `prompt_ref`. Do not ask for duration or a description of the action; the driving video defines the temporal performance.

## Write the main prompt

Follow the official two-part caption schema:

`人物外观描述：[objective character appearance]. 背景描述：[objective background].`

The labels mean:

- `人物外观描述` — character appearance description
- `背景描述` — background description

Write the prompt in Chinese by default because the official repository instructs an LLM to produce this exact Chinese schema and demonstrates inference with Chinese captions. If the user's host application explicitly expects another language, preserve the same two-field semantics in that language.

## Write the separate motion prompt

Always provide this fixed value separately from the main prompt:

`人物动作的参考视频`

Label it `Motion prompt (prompt_ref)` so the user does not paste it into the main `prompt` field. Its English translation is `Reference video of the person's movements.`

### Character appearance field

Describe only visible, identity-bearing facts:

- character type, approximate age presentation, and build when visually clear
- face shape and distinctive facial features
- hair or fur color, texture, and style
- clothing, accessories, colors, materials, and visible motifs
- non-human anatomy or design features

Use concrete nouns and visible attributes. Keep uncertain details out.

### Background field

Describe only the reference image's environment:

- setting or backdrop
- dominant visible objects
- lighting that is visibly evident
- background cleanliness, complexity, and color when useful

For a cutout or blank studio background, say so directly, such as `纯白色背景，光线均匀，无其他物体。`

### Exclude

Do not include:

- actions, poses, gestures, choreography, or motion timing
- inferred feelings, personality, intent, or subjective praise
- camera movement copied from the driving video
- model name, version, duration, resolution, FPS, CFG, steps, seed, or node names
- instructions to “copy the motion” or “use the reference video”; those are already supplied structurally

## Optional viewpoint control

Wan Animate 2 introduces text-driven viewpoint control that can decouple the result from the driving video's viewpoint. Use it only when the user's checkpoint/workflow includes the optional Viewpoint LoRA or explicitly exposes viewpoint control.

- If no viewpoint change is requested, omit viewpoint language and preserve the driving viewpoint.
- If requested and supported, append one short, unambiguous view phrase.
- Prefer the vocabulary demonstrated in the paper, such as `right 60-degree view`, `top angle`, `eye-level front view`, or a clear left/right side view.
- Do not combine conflicting angles or describe a moving camera path; the documented control describes a target viewpoint, not a shot-by-shot camera move.

Because workflow support may lag the paper, label this as an optional control outside the prompt when support is unknown.

## Input-quality checks

Prompt wording cannot repair badly matched conditioning. Before drafting, flag visible problems that are likely to dominate the result:

- Prefer one clear character with sharp, unobstructed identity details.
- Avoid reference images where props or occluders hide important face, hand, or body features.
- When advising on generation, mention that community reports find roughly matched reference-image and driving-video framing more reliable, such as full-body to full-body or waist-up to waist-up. This does not require the agent to inspect the driving video.

Treat community observations as practical heuristics, not official guarantees.

## Negative prompts and parameters

Do not invent a negative prompt unless the user's interface exposes one. The official repository contains a configured artifact-oriented negative list, but the documented Diffusers call does not require the user to supply one.


## Output format

Provide all four items, clearly separating the two copyable Chinese prompt fields from their translations:

1. **Main prompt (`prompt`) — copy this:** the single-line Chinese appearance/background caption.
2. **Motion prompt (`prompt_ref`) — copy this separately:** `人物动作的参考视频`
3. **Main prompt — English translation:** a faithful translation of item 1 that preserves the field labels as `Character Description:` and `Background Description:`.
4. **Motion prompt — English translation:** `Reference video of the person's movements.`

Use fenced text blocks for the two copyable Chinese prompts so the user can copy each field without its label. Keep translations outside those blocks. If viewpoint control was requested and confirmed available, append the supported viewpoint phrase to the main prompt only. If the user asks for setup help, place media-fit observations and recommended parameters after the prompts.

## Examples

### Human portrait on a plain backdrop

Main prompt (`prompt`) — copy this:

```text
人物外观描述：一名年轻女性，椭圆形脸庞，深棕色齐肩卷发，身穿米白色针织衫和深蓝色高腰长裤，佩戴细小的银色圆形耳环。背景描述：背景为浅灰色无缝摄影棚背景，光线柔和均匀，无其他物体。
```

Motion prompt (`prompt_ref`) — copy this separately:

```text
人物动作的参考视频
```

Main prompt — English translation: 

Character Description: A young woman with an oval face and shoulder-length dark-brown curly hair, wearing an off-white knitted top and dark-blue high-waisted trousers with small silver circular earrings. Background Description: A light-gray seamless studio backdrop with soft, even lighting and no other objects.

Motion prompt — English translation: 

Reference video of the person's movements.

### Stylized non-human character

Main prompt (`prompt`) — copy this:

```text
人物外观描述：一只银灰色虎斑猫角色，圆润的脸庞，竖立的耳朵和巨大的圆形眼睛，身穿带金色纽扣的深蓝色制服外套、白色衬衫和红色蝴蝶结。背景描述：背景为纯白色，光线均匀明亮，无其他杂物或装饰。
```

Motion prompt (`prompt_ref`) — copy this separately:

```text
人物动作的参考视频
```

Main prompt — English translation: 

Character Description: A silver-gray tabby cat character with a rounded face, upright ears, and enormous round eyes, wearing a dark-blue uniform jacket with gold buttons, a white shirt, and a red bow tie. Background Description: A pure white background with bright, even lighting and no clutter or decoration.

Motion prompt — English translation: 

Reference video of the person's movements.

Notice that neither example describes what the character does. Motion and expression come from the driving video.
