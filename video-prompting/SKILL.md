---
name: video-prompting
description: Draft and refine prompts for video generation models (text-to-video and image-to-video), and create character-sheet prompts for image models when the goal is character consistency before image-to-video. Use when a user asks for a "video prompt", a model-specific prompt such as Seedance 2.0, Ovi, Sora, Veo 3, Wan 2.2, LTX-2, or LTX-2.3, or a consistent-character prompt such as "character sheet prompt", "character turnaround", "character reference sheet", or "photographic identity sheet".
---

# Video Prompting

## Overview

Turn a user’s intent into either:

- a strong, model-compliant video prompt, or
- a strong image-model prompt for a character sheet that will later support image-to-video consistency.

Model-specific video guidance lives in `references/models/`. Character-sheet guidance lives in `references/workflows/character-sheets.md`.
This file is the entry point: route to the right path, ask the minimum clarifying questions, then draft the prompt in the expected format.

## Model Index

- Ovi: `references/models/ovi/prompting.md`
- Sora (Sora 2): `references/models/sora/prompting.md`
- Veo 3 / 3.1: `references/models/veo3/prompting.md`
- Wan 2.2: `references/models/wan22/prompting.md`
- Seedance 2.0: `references/models/seedance2/prompting.md`
- LTX-2: `references/models/ltx2/prompting.md`
- LTX-2.3: `references/models/ltx2-3/prompting.md`

## Workflow Index

- Character sheets for consistent characters: `references/workflows/character-sheets.md`

To add a new model later: create `references/models/<model>/prompting.md`, then add it to this index.

To add a new workflow later: create `references/workflows/<workflow>.md`, then add it to the Workflow Index.

## Workflow

### Step 1 — Route the request

Decide whether the user wants:

- a video-generation prompt, or
- a character-sheet prompt for an image model

Route to the character-sheet workflow when the user wants a reusable reference sheet, turnaround, expression sheet, costume sheet, photographic identity sheet, or a consistent-character starting point for a longer image-to-video project.

If the user is asking for both, do them in this order:

1. Character sheet
2. Scene still / anchor frame
3. Video prompt

### Step 2 — If it is a video prompt, identify the model and input mode

If the user did not name a model, ask which model they are using (or offer supported options from the Model Index).

Then confirm the input mode:

- Text-to-video (t2v), or
- Image-to-video (i2v)

If i2v: ask the user to share the image (optional, but it will help you generate a better prompt). Use the image as an anchor according to the chosen model’s guidance (e.g., keep identity/wardrobe/composition stable; focus your text on motion/camera/what changes).

If the chosen model has versions, duration constraints, or required parameters, ask the minimum questions needed to select the right format (see the model guide).
For LTX-2.3 specifically: default to a 10-second clip when duration is missing, ask if the user wants shorter or longer, and scale motion complexity to match that duration.

### Step 3 — Load the correct reference and follow its format

For video prompts: open the model’s `prompting.md` from the Model Index and follow its rules strictly.

For character sheets: open `references/workflows/character-sheets.md` and follow its structure strictly. Treat this as an image-model prompt, not a video-model prompt.

### Step 4 — Draft the prompt in the right form

For video prompts, default structure (adapt to the model’s style and required sections):

1. Subject(s): who/what, distinctive details
2. Setting: where/when, lighting, mood
3. Action progression: what changes over time (start → beat → beat → end)
4. Camera: framing/movement only if it matters
5. Dialogue/audio: only if the model supports it, using the model’s exact format

Avoid keyword soup. Prefer a single, well-described shot unless the user explicitly wants multiple cuts/shots.

For character sheets, prefer a layout-driven prompt that locks:

1. Identity anchors: age range, face shape, hair, body type, silhouette
2. Wardrobe anchors: core outfit, shoes, accessories, materials
3. Sheet layout: front / 3/4 / side / back, expressions, pose callouts, prop callouts
4. Render constraints: full body, same person in every panel, same outfit, same proportions
5. Background constraints: plain studio sheet by default unless the user wants an in-world presentation board

### Step 5 — Output

Default: output only the final prompt text.
Default formatting: output prompts as a single line with no line breaks unless the user explicitly requests multiline formatting.

If the user asks for options: provide 2–3 distinct prompt variants, each fully self-contained and compliant with the model’s formatting.

If the model uses required API parameters (e.g., duration/size), include a short “Recommended parameters” line only when the user has specified them or explicitly asks for them.

If the user wants the full consistency workflow, after the character-sheet prompt also provide:

- one prompt for a first scene still that uses the character sheet as reference, and
- one prompt for the follow-on image-to-video shot
