# NPL Fantasy — Preview

A preview build of **NPL Fantasy**, a companion app prototype for the Nepal Premier League. It covers matches, stats, tables, news and season-long fantasy cricket.

This repository holds only the **compiled static web build**, which is served with GitHub Pages at `/npl-fantasy-preview/`. The application source and the data/API backend live elsewhere. This repo is what gets deployed.

> Prototype: the page is marked `noindex, nofollow` and is not meant for public release.

## What's in here

| Path | Purpose |
| --- | --- |
| `index.html` | App entry point (a Vite-built React single-page app) |
| `assets/` | Hashed JS/CSS bundles and self-hosted fonts (Inter, Space Grotesk) |
| `assets/licensed/` | Drop-in folder for officially licensed player photos and team logos (see its [README](assets/licensed/README.md)) |
| `sw.js`, `workbox-*.js` | Service worker (Workbox) for offline caching and installability |
| `manifest.webmanifest`, `icons/` | PWA manifest and app icon |
| `runtime-config.json` | Points the app at its backend API, with no rebuild needed |
| `.nojekyll` | Stops GitHub Pages from running Jekyll on the build |
| `.github/workflows/pages.yml` | Deploys the repository root to GitHub Pages |

## Backend API configuration

The app reads its data from a separate API. It finds the API base URL in this order:

1. **`?api=<url>` query parameter** (also accepted in the hash, e.g. `#/?api=...`). This works on `localhost` / `127.0.0.1` only, and the value is saved in `localStorage`. Use `?api=reset` to clear it.
2. **A saved override** from step 1 (also localhost only).
3. **`runtime-config.json`** → `apiBase`. This is what the deployed site uses.
4. **Same origin**, as a fallback.

To repoint the deployed site at a new backend, such as a new tunnel URL, edit `runtime-config.json` and push:

```json
{
  "apiBase": "https://your-api.example.com",
  "updatedAt": "2026-11-11T20:37:26.777Z",
  "channel": "pc-tunnel"
}
```

The app fetches this file with `cache: "no-store"`, so changes take effect on the next load without a new build.

## Deployment

Every push to `main` (or a manual run from the Actions tab) triggers the **Deploy Pages** workflow. It uploads the repository root as-is, with no build step, and publishes it to GitHub Pages.

To ship a new version of the app:

1. Build the app in the source project. Its Vite `base` must be `/npl-fantasy-preview/`.
2. Replace the contents of this repo with the new build output. Keep `README.md`, `.github/`, `.nojekyll`, `runtime-config.json` and `assets/licensed/`.
3. Commit and push to `main`.

## Running locally

The build uses absolute paths under `/npl-fantasy-preview/`, so serve it from a parent directory:

```bash
cd .. && python3 -m http.server 8080
```

Then open `http://localhost:8080/npl-fantasy-preview/`. Add `?api=http://localhost:<port>` to point it at a local backend.

## Licensed assets

Player photos and team logos are resolved in this order: a licensed file in `assets/licensed/`, then an attributed Wikimedia Commons photo, then an initials avatar in franchise colours. See [`assets/licensed/README.md`](assets/licensed/README.md) for file naming, and record licences in `assets/licensed/LICENCES.md`.
