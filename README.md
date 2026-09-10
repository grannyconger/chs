# chs

Rock the Block travel itinerary as an offline-capable, mobile-first web app.

## Use it

- **Live:** enable GitHub Pages (Settings → Pages → Deploy from branch → `main` / root). The app is served at `https://<user>.github.io/chs/`.
- **Offline:** open the page once with a connection. A service worker caches every asset, so it then works with no signal. On a phone, use **Add to Home Screen** (iOS: Share → Add to Home Screen; Android/Chrome: the **Install app** button) for a full-screen, launchable copy.
- **No build step.** Plain HTML, CSS, and JavaScript. Opening `index.html` directly in a browser also works (the service worker is skipped over `file://`, everything else runs).

## What's here

| File | Purpose |
| --- | --- |
| `index.html` | The whole app — markup, styles, itinerary data, and logic in one file. |
| `service-worker.js` | Precaches assets for offline use. |
| `manifest.webmanifest` | Installable-app metadata. |
| `icons/`, `favicon.ico` | App and tab icons. |
| `source/` | Original itinerary spreadsheet and the style guide the design follows. |

## Itinerary data

Every stop lives in the `DAYS` array near the top of the `<script>` block in `index.html`. Each address carries a Google Maps `destination` query used by its **Directions** button. Edit the array and redeploy to update the itinerary.

## Design

Follows the Pit Crew / GMC Athletics style guide (`source/GMCAthleticsStyleGuide.md`): Arial only, one navy accent, square edges, square bullets, the divider under the title.
