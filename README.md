# Hi, I'm Michael Marker

I build a Windows desktop app for authoring PBR materials and product scenes — and the slim web
viewer that ships the result to customers. Both run the **same engine**, so a material looks
identical in the editor and on the customer's page; that constraint drives most of the technical
decisions below.

- **Real-time viewport on WebGPU** (three.js `WebGPURenderer`), with an **offline path tracer**
  for final images — interactive preview, a render window with history and post, and denoising
  via Intel Open Image Denoise.
- **Editor**: material library, presets, undo/redo, dockable panels, a project container, model
  import with real-world scale (glTF/FBX and STEP via OpenCascade).
- **Desktop shell in Rust** (Tauri/WebView2 — yes, WebGPU works there), Windows installer.
- **No build step, one dependency.** The web viewer is plain ES modules and self-hosted three.js;
  customers host it themselves and own their data.

Most of what shows up on this profile falls out of that work: minimal repros, measurement
harnesses, and bugs that turned out to be upstream's.

### Upstream

I file bugs against [three.js](https://github.com/mrdoob/three.js) and
[three-gpu-pathtracer](https://github.com/gkjohnson/three-gpu-pathtracer) — measured, not
eyeballed: a headless run on a real GPU, an error count with and without the patch, a live repro.
Last one went 1771 errors → 0 and shipped in r186. Not every report lands.
[All of them.](https://github.com/search?q=author%3AMichaMaGit+is%3Aissue&type=issues)

### Writing up

Notes from the same work that aren't published yet — ping me if one of these is what you were
searching for, that moves it up the list:

- **WebGPU runs in Tauri / WebView2 with no flags** — and gets 2 GiB buffers where ANGLE caps at 1.
- **A headless WebGPU test harness** — Edge headless + CDP: real GPU, screenshots, error counts,
  usable in CI-ish loops. I couldn't find an existing one.
- **OIDN in a Tauri app** — the Rust side (crate pinned to a git rev, `bundled`) and the JS side.
