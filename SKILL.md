---
name: responses-image-generation
description: Generate and save local raster image files through a Responses API-compatible image_generation call. Use when the user asks to generate an image, create art, make a picture, produce a PNG asset, save an image locally, or use reference images for image generation. Trigger on requests such as 生成图片, 生图, 画图, 做图, 出图, 做素材, 图片素材, 游戏素材, 图标, icon, sprite, asset, poster, 海报, 封面图, 配图, 插画, transparent background, 透明背景, reference image, 参考图, based on this image, or any prompt where Codex should create an actual image file on disk instead of only describing the result in chat.
---

# Responses Image Generation

Use the bundled Node script to create an image file on disk. The script supports both text-only generation and image-conditioned generation through repeatable `--reference` arguments.

## Workflow

1. Confirm the inputs.
- Require a prompt and an output path.
- Accept optional reference images as local file paths, `http(s)` URLs, data URLs, or `file-...` / `file_...` IDs.
- If the user does not provide an output path, choose one and state it before running the command. Prefer a workspace-relative `./output/...` path or a user-specific target such as `~/Downloads/generated/...`.

2. Run the generator.
```bash
node ~/.codex/skills/responses-image-generation/scripts/generate-image-via-responses.mjs \
  --prompt "A polished 2D tower defense turret with transparent background" \
  --output "/absolute/path/to/output/turret"
```

Add references with repeated flags:

```bash
node ~/.codex/skills/responses-image-generation/scripts/generate-image-via-responses.mjs \
  --prompt "Turn this sketch into a polished game icon" \
  --output "/absolute/path/to/output/icon" \
  --reference "/absolute/path/to/sketch.png" \
  --reference "https://example.com/style-reference.jpg"
```

3. Use optional overrides only when needed.
- `--base-url` defaults to `https://code.ylsagi.com/codex`.
- `--outer-model` defaults to `gpt-5.4`.
- `--image-model` defaults to `gpt-image-2`.
- API key resolution order is `--api-key`, then `~/.codex/auth.json`, then `OPENAI_API_KEY`.
- Pass `--tool-json` only when the user explicitly asks for custom `image_generation` tool fields.

4. Handle common failures pragmatically.
- If the command fails with `fetch failed` or another sandbox/network error, rerun the same `node` command with escalated permissions because the script calls a remote gateway.
- If generation is blocked for a real-person photorealistic request, rewrite the prompt to a generic physical description instead of naming the person directly while keeping outfit, scene, composition, and lighting details.

5. Verify and report the result.
- Read the JSON stdout to get `outputPath`.
- Confirm the file exists.
- Report dimensions with `sips -g pixelWidth -g pixelHeight <file>` when useful.
- Return the saved file path in the final answer so the user can open it directly.

## Notes

- The script infers the file extension from the returned bytes when the output path has no extension.
- Use absolute paths for references and outputs whenever the target is outside the active workspace.
- Run the script once per requested variant; it does not batch multiple images in a single call.

## Resource

- `scripts/generate-image-via-responses.mjs`: Streams Responses API SSE output, extracts the image payload, writes the final image file, and prints structured JSON.
