# Video Prompting (Agent Skill)

This is an Agent Skill for drafting and refining prompts for video generation models (text-to-video and image-to-video), and for creating character-sheet prompts for image models when the goal is consistent characters before image-to-video.

Supported video-model guides include Seedance 2.0, LTX-2, LTX-2.3, Sora, Veo 3/3.1, Wan 2.2, and Ovi.

The skill also includes a character-sheet workflow for:

- reusable turnaround sheets
- expression sheets and prop callouts
- scene-still handoff prompts for image-to-video pipelines
- photoreal photographic identity sheets for real-person consistency

It follows the Agent Skills spec (agentskills.io).

## Tutorial

Agent Skills walkthrough and demo of this skill:

[![Agent Skills tutorial](https://img.youtube.com/vi/6T7OCqBQhSc/0.jpg)](https://youtu.be/6T7OCqBQhSc)

## Contents

- `video-prompting/SKILL.md`: skill entry point and workflow
- `video-prompting/references/models/`: model-specific prompting guides
- `video-prompting/references/workflows/character-sheets.md`: character-sheet and identity-sheet workflow for image-model prompting

## What It Covers

- video prompt drafting for text-to-video and image-to-video
- model-specific routing for supported video models
- character-sheet prompting for consistent characters across longer projects
- photoreal identity-sheet prompting when the user wants real-person reference photography rather than a stylized turnaround
- recommended handoff order: character sheet -> scene still -> video prompt

## Install

### Claude

In Claude, install the skill using whichever format your Claude product prompts for:

- If it asks for a `.skill` file: see instructions below to build a `.skill` bundle.
- If it asks for a folder: select `video-prompting-skill/video-prompting/`.

### Codex

Codex loads skills from skill directories (user-scoped or repo-scoped). Pick one:

- User-scoped (default): `~/.codex/skills/<skill-name>/`
- Repo-scoped: `.codex/skills/<skill-name>/` (committed with your project)

To install user-scoped:

```sh
mkdir -p ~/.codex/skills
cp -R video-prompting-skill/video-prompting ~/.codex/skills/video-prompting
```

Restart Codex so it re-scans skills.

## Build (`.skill` file)

The `.skill` file is just a zip archive of the skill folder (useful for Claude import/sharing). Codex does not require a `.skill` bundle.

To generate the `.skill` zip file:

```sh
cd video-prompting-skill
mkdir -p dist
rm -f dist/video-prompting.skill
zip -r dist/video-prompting.skill video-prompting -x '**/.DS_Store'
```
