# Video Prompting (Agent Skill)

This is an Agent Skills–compatible skill for drafting and refining prompts for video generation models (text-to-video and image-to-video), including Sora, Veo 3/3.1, Wan 2.2, and Ovi.

It follows the Agent Skills spec (agentskills.io): a skill is a folder with a required `SKILL.md` (YAML frontmatter + Markdown instructions), plus optional `scripts/`, `references/`, and `assets/` folders.

## Contents

- `video-prompting/SKILL.md`: skill entry point and workflow
- `video-prompting/references/models/`: model-specific prompting guides
- `dist/video-prompting.skill`: packaged `.skill` bundle (zip), primarily for import/sharing

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

Note: `$skill-creator` (Codex) is helpful for authoring/editing `SKILL.md`, but it’s not required for packaging—zipping the folder produces the `.skill` bundle.
