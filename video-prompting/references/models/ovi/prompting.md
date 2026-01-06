# Ovi prompting

Use this reference when the user asks for an Ovi prompt (Character.AI Ovi).

## Choose the right format (ask if missing)

1. Version: Ovi 1.0 vs Ovi 1.1
2. Input mode: t2v vs i2v (if i2v, confirm they will provide a starting image)
3. Duration:
   - Ovi 1.0: 5 seconds only
   - Ovi 1.1: 5 seconds or 10 seconds

If the user doesn’t specify version, ask which they’re using. If they say Ovi 1.0, do not ask 5s vs 10s.

## If i2v: ask to see the image (optional but helpful)

If the user is doing i2v, ask them to share the starting image (optional, but it will help you generate a better prompt). Use the image to anchor:

- Character identity/wardrobe/props
- Setting and overall style
- Then describe motion + camera behavior that should happen next

## Prompt tokens

- Speech: wrap exact spoken words with `<S> ... <E>`
  - Put only spoken words inside.
  - Preserve the user’s exact wording unless they ask you to rewrite it.

## Audio formatting differs by version

- Ovi 1.1+: write an `Audio: ...` line (typically at the end of the prompt).
  - Example: `Audio: Clear female voice speaking over café ambience and light music.`
- Ovi 1.0 / “match the CSV examples”: use `<AUDCAP> ... <ENDAUDCAP>` (often placed at the end).
  - Example: `<AUDCAP>Clear female voice speaking over café ambience and light music.<ENDAUDCAP>`

## Style and structure

Write a natural-language paragraph describing a single coherent clip:

- Subject(s) with concrete visual details
- Environment with lighting/time-of-day
- Action progression (what changes over time)
- Inline speech using `<S> ... <E>` if needed
- Audio in the correct version format

## Official example prompts (CSVs)

Use these as style anchors:

- `references/models/ovi/example_prompts/gpt_examples_t2v.csv`
- `references/models/ovi/example_prompts/gpt_examples_i2v.csv`
- `references/models/ovi/example_prompts/gpt_examples_10s_t2v.csv`
- `references/models/ovi/example_prompts/gpt_examples_10s_i2v.csv`
