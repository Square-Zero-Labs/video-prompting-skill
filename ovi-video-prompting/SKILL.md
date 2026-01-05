---
name: ovi-video-prompting
description: Draft and refine prompts for Character.AI's Ovi audio-video generation model (text-to-video and image-to-video). Use when a user asks for an "ovi prompt" or "ovi model prompt", e.g. "draft a prompt for the ovi model…", "write an ovi prompt that…", "ovi prompt in which…", or "I need an ovi prompt where…".
---

# Ovi Video Prompting

## Overview

Turn a user’s intent into a high-quality Ovi prompt in the same style as the official examples, including dialogue and audio cues when relevant.

For reference examples, consult the official prompt CSVs in `references/ovi/example_prompts/`:

- `gpt_examples_t2v.csv`
- `gpt_examples_i2v.csv`
- `gpt_examples_10s_t2v.csv`
- `gpt_examples_10s_i2v.csv`

## Quick start

1. Identify whether the user wants text-to-video (t2v) or image-to-video (i2v).
2. Draft one cohesive scene description (subject → setting → action beats → camera framing/motion if needed).
3. Encode any spoken words with `<S> ... <E>` (exact text).
4. Add audio in the format that matches the user’s Ovi version (see “Audio format by Ovi version”).
5. Output the final prompt text (and optionally 2–3 variants if the user asked for options).

## Prompt tokens (must match examples)

- Dialogue: `<S> ... <E>`
  - Put only the words meant to be spoken inside.
  - Preserve the user’s exact wording (case/punctuation), unless they ask you to rewrite it.

## Audio format by Ovi version

Ovi changed the audio prompt format in the Ovi 1.1 update (10 Nov 2025).

- Ovi 1.1+: use an `Audio: ...` line (typically at the end of the prompt).
  - Example: `Audio: Clear female voice speaking over club music, crowd ambience.`
- Ovi 1.0 (older examples): use `<AUDCAP> ... <ENDAUDCAP>` (often placed at the end).
  - Example: `<AUDCAP>Clear female voice speaking over club music, crowd ambience.<ENDAUDCAP>`

If the user doesn’t specify a version, ask whether they’re using Ovi 1.0 or Ovi 1.1. If they explicitly want “match the official CSV examples”, use `<AUDCAP> ... <ENDAUDCAP>` (and optionally include a 1.1-converted variant).

## Workflow

### Step 1 — Clarify only what’s necessary

Ask at most 1–3 questions, and only when missing details block a good prompt. Otherwise, make reasonable defaults and proceed.

Useful clarifiers (pick the minimum set):

- Version: Ovi 1.0 vs Ovi 1.1 (determines audio formatting)
- Mode: t2v vs i2v (if i2v, ask for the image or confirm they’ll provide one)
- Duration: only ask 5s vs 10s for Ovi 1.1 (Ovi 1.0 is 5 seconds only)
- Style: realistic / cinematic / anime / documentary / etc.
- Camera: static / handheld / dolly / close-up / wide (optional)
- Dialogue: confirm exact spoken line(s)
- Audio: confirm whether they want Ovi 1.1 `Audio: ...` or Ovi 1.0 `<AUDCAP> ... <ENDAUDCAP>` (and whether there’s music/ambient sound)

If the user didn’t specify the basics, ask these three questions first (in one message):

1. “Are you using Ovi 1.0 or Ovi 1.1?”
2. “Is this text-to-video, or are you providing a starting image (image-to-video)?”
3. If they say Ovi 1.1: “Do you want a 5-second or 10-second video?” (Ovi 1.0 is 5 seconds only.)

### Step 2 — Draft the scene in “example style”

Write a natural-language paragraph like the examples:

- Identify the subject(s) with distinctive visual details.
- Establish the environment with lighting/time-of-day.
- Describe action progression (what changes over time).
- If there is dialogue, embed it inline with `<S> ... <E>`.
- If helpful, add audio in the requested version format (`Audio: ...` or `<AUDCAP> ... <ENDAUDCAP>`).

Avoid:

- Overly abstract adjectives with no visual anchor.
- A long list of disconnected keywords.
- Too many hard cuts unless the user explicitly asks for multiple shots.

### Step 3 — Quality check against examples

Before finalizing, verify:

- All dialogue is wrapped with `<S>` and `<E>`.
- If using Ovi 1.0 audio tags, any `<AUDCAP>` has a matching `<ENDAUDCAP>`.
- If using Ovi 1.1 audio format, there is a clear `Audio: ...` line (typically at the end).
- The prompt reads like a single coherent clip.
- The user’s requirements are explicitly represented (subject, key action, spoken line).

## Output format

Default: output only the final Ovi prompt text.

If the user asks for options: provide 2–3 distinct prompt variants (each a complete prompt), keeping the required content consistent.
