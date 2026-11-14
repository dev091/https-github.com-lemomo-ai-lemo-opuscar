# Lemo OpusCar for Dot

Direct Linux commands for Dot to read Lemo styles and run the original
Lemo OpusCar renderer. Dot creates each film's `index.html`, scripts, and
assets in its own project directory. The commands use Node, Playwright and
FFmpeg locally.

The Lemo library is maintained upstream by [Lemomo (LemoLab)](https://github.com/lemomo-ai/lemo-opuscar).
This repository contains the Dot launcher. It uses the upstream library from
an existing or separate pinned checkout. See the
[upstream licence](https://github.com/lemomo-ai/lemo-opuscar/blob/3a7132b996be0241c1ea0ab180d519f06e6f10b6/LICENSE)
for the code, style guides, and media terms.

## Use an existing checkout

On Dot, start with the installed library and run the checks inside the same
execution context Dot will use to render:

```bash
export LEMO_OPUSCAR_HOME=/workspace/shared/lemo-opuscar
bash lemo-dot styles
bash lemo-dot doctor
bash lemo-dot smoke
```

`doctor` checks dependencies, local socket access, and the Playwright browser
path. `smoke` verifies an actual 3-second H.264 render through the upstream
entrypoint, full decoding, and changing frames. Each run uses a temporary
project. Run the checks again if Dot's runtime changes.

## Set up a new checkout

Install Git, Node 20+, Python 3.11+ (or uv), and FFmpeg 7+ on the intended host
using its supported tools. To create a separate checkout:

```bash
git clone --filter=blob:none --sparse https://github.com/lemomo-ai/lemo-opuscar.git "$HOME/lemo-opuscar"
git -C "$HOME/lemo-opuscar" checkout --detach 3a7132b996be0241c1ea0ab180d519f06e6f10b6
git -C "$HOME/lemo-opuscar" sparse-checkout set --no-cone '/*' '!/styles/*/demo/' '!/styleboard/' '!/docs/readme/' '!/docs/cover.jpg' '!/styles/*/poster.jpg'
export LEMO_OPUSCAR_HOME="$HOME/lemo-opuscar"
bash lemo-dot setup
bash lemo-dot smoke
```

Setup invokes the upstream dependency installer. Playwright selects the
matching Chromium Headless Shell. Keep the browser cache used by that setup
available to later Dot runs. Use `setup voice`, `setup music`, or both when a
project needs those optional libraries.

If the browser archive download fails integrity checks, retain the first
download error and use the host's approved acquisition path for the exact
Playwright revision. If Chromium reports `socket() ... EPERM`, record the
executable, first fatal line, and `doctor` output. A missing Headless Shell
and a denied launch of full system Chromium are two independent failures.
Resolve host permission errors through the host's supported controls.

## Make a film

```bash
bash lemo-dot style watercolor
bash lemo-dot preview /absolute/path/to/project 0 1.5 --size 1280x720
bash lemo-dot render /absolute/path/to/project --size 1920x1080 --fps 24 --workers 1
```

Project pages expose `window.DUR`, `window.render(t)`, and `window.READY` as
described in the [upstream renderer documentation](https://github.com/lemomo-ai/lemo-opuscar/blob/3a7132b996be0241c1ea0ab180d519f06e6f10b6/core/README.md).
Video defaults to `out/video.mp4` inside the film project. Add `--out` to
choose a separate version. The upstream audio mix and mux tools are available
from the same library checkout.
