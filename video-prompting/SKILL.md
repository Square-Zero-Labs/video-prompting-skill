---
name: video-prompting
description: Draft and refine prompts for video generation models (text-to-video and image-to-video), including scene description, dialogue, audio cues, and style constraints. Use when a user asks for a video model prompt, e.g. "video prompt", "text-to-video prompt", "image-to-video prompt", "ovi prompt", "sora prompt", or "wan prompt".
---

# Video Prompting

## Overview

Turn a user’s intent into a strong prompt for a specific video generation model, using the model’s preferred formatting and tokens.

Model-specific guidance lives in `references/models/`.

## Workflow

### Step 1 — Identify the model and input mode

If the user did not name a model, ask which model they are using (e.g., Ovi, Sora, Wan).

Then confirm whether this is:

- Text-to-video (t2v), or
- Image-to-video (i2v), and whether they will provide a starting image

If i2v: ask the user to share the image (optional, but it will help you generate a better prompt). Use the image as an anchor according to the chosen model’s guidance.

If the chosen model has versions or duration constraints, ask the minimum questions needed to select the right format (see the model reference).

### Step 2 — Load the model reference and follow its format

Pick the relevant model reference and follow it strictly:

- Ovi: `references/models/ovi/prompting.md`
- Sora: `references/models/sora/prompting.md`
- Veo3: `references/models/veo3/prompting.md`
- Wan 2.2: `references/models/wan22/prompting.md`

When adding more models later, add more files under `references/models/` and link them here.

### Step 3 — Draft the prompt as a coherent clip

Default structure (adapt to the model’s style):

1. Subject(s): who/what, distinctive details
2. Setting: where/when, lighting, mood
3. Action progression: what changes over time (start → beat → beat → end)
4. Camera: framing/movement only if it matters
5. Dialogue/audio: only if the model supports it, using the model’s exact format

Avoid keyword soup. Prefer a single, well-described shot unless the user explicitly wants multiple cuts.

### Step 4 — Output

Default: output only the final prompt text.

If the user asks for options: provide 2–3 distinct prompt variants, each fully self-contained and compliant with the model’s formatting.
