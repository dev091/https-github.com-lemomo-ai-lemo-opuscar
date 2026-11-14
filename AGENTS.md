# Working with Lemo on Dot

Use this launcher from Dot's Linux shell. Set `LEMO_OPUSCAR_HOME` to the existing library checkout. Use `./lemo-dot styles` and `./lemo-dot style <slug>` to locate the upstream instructions. Read the selected style and the library's workflow before writing a film. Keep new project files in the user's intended project folder.

Run `./lemo-dot doctor` and `./lemo-dot smoke` in the same approved runtime where the film will render. Keep the existing library, browser caches and active films intact. Use the documented setup on a separate checkout when dependencies are missing. An explicit `PLAYWRIGHT_CHROME` override changes which executable is tested. Record its result separately from the Playwright default.

Use `./lemo-dot preview` for frame review and `./lemo-dot render` for the upstream picture render. Follow the upstream guide for audio, assembly and final QA. A successful smoke test verifies the renderer in that runtime. For a failure, retain the first decisive error and provide a status that identifies the failed stage.
