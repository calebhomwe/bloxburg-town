# Bloxburg Town

A Unity WebGL build (published from Unity; the Unity project is not in this repo). `index.html` boots `Build/webgl-20260911_222125.*`; `dashboard.html` is the test-evidence page (bot screenshots in `shots/`, `latest-summary.md`). Also listed in [Caleb's Arcade](https://calebhomwe.github.io/arcade/).

## Play / Test

- **Play online:** https://calebhomwe.github.io/bloxburg-town/ (GitHub Pages, deployed from `main`)
- **Run locally:** `python3 -m http.server 8080`, then open http://localhost:8080/ (any static server works: the `.unityweb` files are gzip with Unity's decompression fallback, so no special headers are needed)
- **Smoke test** (the `Smoke` workflow runs it on every push and PR): boots the build in headless Chromium and fails if it does not finish loading, shows the Unity error dialog, throws, or requests a missing file.

  ```sh
  npm install --no-save --no-package-lock playwright@1.58.2 && npx playwright install chromium
  python3 -m http.server 8080 &
  IGNORE_ERRORS='Unable to decode audio data' SETTLE_MS=90000 \
    READY="document.querySelector('#unity-loading-bar').style.display === 'none'" \
    node .github/smoke.mjs http://127.0.0.1:8080/ index.html
  ```

  Playwright's Chromium has no AAC decoder, so Unity's audio clips fail to decode there and Unity shows its error dialog; that one message is tolerated. Real Chrome, Safari and Edge decode AAC.

Known from the 2026-10-06 run: the town renders with characters walking, but the console warns `Failed to create agent because there is no valid NavMesh` (NPC agents have no baked NavMesh), and the HUD text at the left edge of the 960x600 canvas looks clipped. Both need the Unity project to fix.
