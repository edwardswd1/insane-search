# Reality Fabricator (rfab.ai) API

Paid API for AI-made pictures, video and game art, on the repo owner's own account.

**No phone calls.** The owner does not want calls. Never use the phone-call endpoints.

- **Key:** read it from the `RFAB_API_KEY` environment variable (or the git-ignored `.env` at the repo root). Send it as the `X-API-Key` header. Never write the key into a tracked file.
- **Base URL:** `https://api.rfab.ai`
- **Full docs:** <https://rfab.ai/llms-full.txt>. Read the part for a job before you start it.
- **What leaves the machine:** only what you write for a job (a picture or video prompt). Never code, file contents, passwords or private data.

## When to use it

When a job needs something to look good (art, a page header, buttons, a loading animation), it is usually worth making the real thing here rather than a placeholder; a picture costs a few cents.

- **Art:** `POST /api/image-generation/generate` with `{ prompt, modelId: "openai:gpt-image-2", width, height, async: true }`, then poll `GET /api/image-generation/job/:jobId` until completed. Image URLs are permanent. Leave `quality` out: the default tier looks the same at normal size, so to get a better picture change the model or the prompt instead. Ranked model list: `GET /api/image-generation/models`.
- **Video:** draw a still, then `POST /api/image-generation/generate-video` with `{ imageUrl, prompt, videoModelId: "atlascloud:seedance-2.5-i2v", duration }`. Video is always a job; poll the same job endpoint for `result.videoUrl`. Describe only what moves, and write "camera locked" when the camera should stay still.
- **Game assets:** `{ style: "pixel-art" }` or `{ style: "game-assets" }` for sprites, `alphaRegion` for HUD frames with a see-through window, `POST /api/image-generation/isolate-figure` for a transparent cut-out. Write ONE character paragraph and start every prompt in a set with it, so the set matches.
- **Page headers:** draw wider and taller than the banner (e.g. 1536x1024) and crop to fit. Never stretch, pad or blur the edges. Keep words out of the picture and put the real heading over it in HTML, because image models misspell words.
- **Buttons:** draw the button art (a glossy illustrated panel, an icon, a themed frame) and put the real label on top in HTML/CSS, so the label stays sharp and editable. Give it a hover state.
- **Loading animations:** follow "Transparent Looping Animation" in the docs (character on solid green, animate it, key out the green with ffmpeg, loop it as a transparent .webm). Use one for any wait over two seconds.
- **One look per project:** write a short style paragraph once (characters, palette, art style), keep it in the project, and start every prompt with it.

## Cost

Spends tokens from the owner's rfab.ai balance (`GET /api/tokens/balance`; $1 = 50,000 tokens). A picture is a few cents.
