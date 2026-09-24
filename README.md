# EX NIHILO

A 43-second demoscene film where every pixel and every sound comes from a single HTML file (~310 KB with the player and exporter): no images, no audio files, no fonts, no libraries.

It was made by Claude Opus 5.5 from a one-line prompt asking it to build the most impressive demo of its own capabilities. The project and film are by **Justin Perea**:

- Write-up: [Ex Nihilo: the whole film is one HTML file](https://justinperea.com/lab/ex-nihilo)
- Thread on X: [@JustinPerea](https://x.com/JustinPerea/status/2102893186330841502)

This repository keeps a copy of the file, with video-style playback controls added (see below).

## Run it

Open `index.html` in a desktop browser (tested in Chrome). The soundtrack is synthesized first ("synthesizing sound…"), then click to begin, with sound on.

| Control | Action |
|---|---|
| Click image, `Space` / `K` | Play / pause |
| Progress bar | Click or drag to seek; ticks mark scene cuts |
| `←` / `→` | Back / forward 2 s (`Shift`: 0.1 s) |
| `Home` / `End` | Jump to start / end |
| `F` | Fullscreen |
| `C` / `</>` | Show the code: the scene's GLSL next to the film, following the playhead |
| `E` / `⤓` | Export the film to MP4, rendered in your browser |

## Export

The export dialog re-renders every frame offline, the same way the published film was made (several samples per frame for motion blur and anti-aliasing), then encodes H.264 + AAC with WebCodecs and writes the MP4 with a small muxer in the file. Nothing is uploaded. At 1080p60, 1 sample per frame takes under a minute on an M-series Mac; 4 samples takes about 10 minutes, and 12 samples (as published) about 30. Needs a recent Chrome or Edge.

The live page renders one sample per pixel and scales its resolution to hold 60 fps, so it looks softer than the recorded film, especially on weaker GPUs.

## What's inside

- **Renderer:** a small WebGL2 engine. Each scene is a fragment shader that raymarches signed distance fields. There is no stored geometry.
- **Sound:** a synthesizer and score in plain JavaScript that generates 48 kHz audio sample by sample before playback.
- **Sync:** the shaders read kick, snare and impact envelopes from the same event list the synth plays, so picture and sound stay locked together.
- **Type:** system fonts drawn with Canvas2D after tonemapping.
- **End card:** the file's own source, typeset onto one frame.

| Scene | Time | Technique |
|---|---|---|
| Void | 0–4 s | Light echoes through dust shells, lens glare, bokeh |
| 01 Matter | 4–12 s | SDF mercury blobs freezing into a kaleidoscopic IFS, thin-film iridescence |
| 02 World | 12–20 s | Eroded fractal terrain, Rayleigh/Mie sky, raymarched cloud sea |
| 03 City | 20–28 s | Grid-traced (DDA) endless city, haze, wet reflections |
| 04 Cosmos | 28–36 s | Black hole with Schwarzschild geodesics, Doppler-beamed accretion disk |
| End card | 36–43 s | The source code itself |

## How it was made

From the write-up:

- Claude wrote the engine, a build script and a still/contact-sheet renderer before any scene, so every agent could look at its own frames.
- Five scene agents and one composer worked in parallel. Critic agents rendered and measured each scene, and revisers applied their notes.
- The composer can't hear, so it mixed by measurement: loudness, true peak, spectrograms, click detection and kick timing (−14.3 LUFS, −1.4 dBTP).
- The film was captured frame by frame in headless Chrome: 2,580 frames × 12 samples each for motion blur, anti-aliasing and depth of field, then muxed with ffmpeg.

| | |
|---|---|
| File | 1 HTML file, 286,346 bytes, 0 assets |
| Code | 4,056 lines of GLSL, 631 lines of music code |
| Agents | 36 runs across two workflows |
| Time | 4 h 52 m active, 18.7 h wall clock |

See [the full write-up](https://justinperea.com/lab/ex-nihilo) for the details, including what broke along the way.
