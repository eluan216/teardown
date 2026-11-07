# teardown

A Claude Code skill that turns any machine into a 3D simulator you can take apart.

**Also includes free standalone browser simulators** with no local installation. These pages load Three.js from a CDN and require an internet connection.

---

## Free Browser Simulators (No Claude Code)

Download one of these HTML files and open it in a WebGL-capable browser while connected to the internet. GitHub's file preview and Raw view show source code; they do not run the simulator.

| Simulator | File | What it shows |
|-----------|------|---------------|
| **Cross-Plane V8 Engine** | [engine-teardown.html](https://github.com/eluan216/teardown/blob/main/standalone/engine-teardown.html) | Eight-cylinder four-stroke engine, live charts, cutaway and exploded views |
| **Bicycle Derailleur** | [derailleur-teardown.html](https://github.com/eluan216/teardown/blob/main/standalone/derailleur-teardown.html) | 11-speed cassette, parallelogram, jockey wheels, shifting |
| **Watch Escapement** | [escapement-teardown.html](https://github.com/eluan216/teardown/blob/main/standalone/escapement-teardown.html) | Swiss lever, balance wheel, hairspring, impulse phases |

All three share the same features:
- Live gauges driven by each simulator's model (the standalone watch and derailleur use simplified illustrative mechanisms)
- Exploded / assembled views
- Orbit + zoom controls
- Auto-tour after a few seconds of inactivity
- HTML + Three.js — the standalone sources require CDN access; browser caching does not guarantee offline operation

No build step, no Node, no Claude required for these browser pages.

For fully self-contained offline HTML with Three.js and fonts embedded, use the bundled examples instead:

```sh
npm ci
node sims/build.mjs v8-live cessna-live strandbeest-live
```

Open the generated HTML files in `teardown-out/dist/`. These are separate examples from the standalone pages above.

---

## Original Claude Code Skill

Ask how something works. Claude builds it as one offline HTML page: the machine runs in the middle, every panel around it reads the real numbers, a button opens the part doing the work, and when nobody touches it the page tours itself.

![Mellis HB-7 robotic bee simulator](docs/bee.gif)

| Telson SC-8 robotic scorpion | Pelagos OC-8 robotic octopus |
|---|---|
| ![Telson SC-8 robotic scorpion](docs/scorpion.gif) | ![Pelagos OC-8 robotic octopus](docs/octopus.gif) |

### Install

```
npx skills add elvarofficial/teardown -g -a claude-code
```

Then, once, in the installed folder:

```
npm install && npx playwright install chromium
```

You need Node.js 20 or newer. For the video you also need `ffmpeg`, and for the sound Python 3.

### Use

Ask Claude Code in plain words:

> Build me a simulator of a 4-stroke motorcycle engine I can take apart.

> How does a mechanical watch keep time? Show me as a simulator.

> Make a teardown of a hydraulic excavator arm and record a 20-second video of it.

Or open one of the 3 examples first:

```
node sims/build.mjs v8-live          # teardown-out/dist/v8-live.html
node sims/build.mjs cessna-live
node sims/build.mjs strandbeest-live
```

### What is inside

| | |
|---|---|
| `SKILL.md` | What Claude reads: how a machine is built, the rules that make it look right, the review loop. |
| `sims/kit/` | Runtime, stage, shader warm-up. |
| `sims/v8-*`, `sims/cessna-*`, `sims/strandbeest-*` | 3 complete machines. `-shared` is the machine, `-live` the page you drive, `-film` the recorded tour. |
| `sims/build.mjs` | Bundles a machine into one HTML file, three.js and fonts inlined. |
| `sims/capture.mjs` | Renders the film frame by frame to a 60 fps mp4, or to review sheets of stills. |
| `sfx/`, `run_sfx.py` | Procedural sound on the film's timeline, mastered to −14 LUFS. |
| `standalone/` | **Free browser simulators** that load Three.js from a CDN. |

## Credits

Built on [whaleyxbt/claude-motion](https://github.com/whaleyxbt/claude-motion) (MIT): the runtime, frame capture, sound engine and review rules come from there. Recorded samples are CC0 from Freesound, prepared by [Barty-Bart/motion-graphics](https://github.com/Barty-Bart/motion-graphics); sources in `sfx/samples/SOURCES.md`.

MIT licence.
