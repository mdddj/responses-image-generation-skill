# responses-image-generation

Codex skill for generating and saving local image assets through a Responses API-compatible `image_generation` call.

## Install

Clone or copy this repository to:

```bash
~/.codex/skills/responses-image-generation
```

Example:

```bash
git clone https://github.com/mdddj/responses-image-generation-skill.git ~/.codex/skills/responses-image-generation
```

## Requirements

- Node.js
- Codex auth in `~/.codex/auth.json` or `OPENAI_API_KEY`

## Example

```bash
node ~/.codex/skills/responses-image-generation/scripts/generate-image-via-responses.mjs \
  --prompt "A polished 2D tower defense turret with transparent background" \
  --output ~/Downloads/generated/turret
```

In Codex chat you can also invoke it explicitly with:

```text
$responses-image-generation
```
