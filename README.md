# Leo Interview Preparation Notebook

A personal, offline-first quick-reference app for technical interview preparation and
skill refresh. Single-page PWA, no build step, no dependencies.

Skill areas:

| Skill | Status |
|-------|--------|
| C Concepts | ✅ 12 topics |
| Shell Scripting | ⏳ planned |
| C++ | ⏳ planned |
| OOPS | ⏳ planned |
| Data Structures | ⏳ planned |

Every topic has four blocks: **Notes** (full explanation), **Cheat Sheet** (condensed
recall), **Q&A** (short self-check pairs), and **Interview Questions** graded
Basic → Intermediate → Advanced with click-to-reveal answers.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The whole app — content, styles, hash router. Works from `file://`. |
| `manifest.json` | PWA metadata (name, icons, colors). |
| `sw.js` | Cache-first service worker for offline use. Bump `CACHE_NAME` after editing `index.html`. |
| `icon-192.png`, `icon-512.png` | App icons. |

## Run locally

Just open `index.html` in a browser. For the PWA / offline features (service worker,
"install app"), serve it over HTTP:

```sh
cd Leo-Interview
python -m http.server 8080
# visit http://localhost:8080
```

## Deploy to GitHub Pages

1. Create a new repository (e.g. `leo-interview`) on GitHub.
2. From this folder:
   ```sh
   git remote add origin https://github.com/<you>/leo-interview.git
   git push -u origin main
   ```
3. Repo → **Settings → Pages** → Source: *Deploy from a branch* → Branch: `main` / `/ (root)`.
4. The app will be live at `https://<you>.github.io/leo-interview/` within a minute.
   PWA install and offline caching work once it is served over HTTPS.

## Adding a skill area (later phases)

1. In `index.html`, fill the placeholder `TOPICS.<skill>` array in the `<script>` with
   `[slug, title, blurb]` rows.
2. Add one `<section class="view topic" id="v-<skill>-<slug>">` block per topic, using an
   existing C topic as the template (sidebar `<nav data-sidebar="<skill>">`, then the four
   `<details class="section">` blocks).
3. Flip the skill's card on the home page from `status soon` to `status`.
4. Bump `CACHE_NAME` in `sw.js`.
5. Commit.
