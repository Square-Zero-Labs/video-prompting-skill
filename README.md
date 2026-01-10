# Video Prompting (Agent Skill)

This is an Agent Skill for drafting and refining prompts for video generation models (text-to-video and image-to-video), including LTX2, Sora, Veo 3/3.1, Wan 2.2, and Ovi.

It follows the Agent Skills spec (agentskills.io).

## Tutorial

Agent Skills walkthrough and demo of this skill:

[![Agent Skills tutorial](https://img.youtube.com/vi/6T7OCqBQhSc/0.jpg)](https://youtu.be/6T7OCqBQhSc)

## Contents

- `video-prompting/SKILL.md`: skill entry point and workflow
- `video-prompting/references/models/`: model-specific prompting guides

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
