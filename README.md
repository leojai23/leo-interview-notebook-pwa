# Leo Interview Preparation Notebook

A personal, offline-first quick-reference app for technical interview preparation and
skill refresh. Single-page PWA, no build step, no dependencies.

Every topic has four blocks: **Notes** (full explanation), **Cheat Sheet** (condensed
recall), **Q&A** (short self-check pairs), and **Interview Questions** graded
Basic → Intermediate → Advanced with click-to-reveal answers. No quizzes.

## Roadmap — 16 skill areas

Built one skill at a time, with a review checkpoint after each. The app already shows
every area's planned topic list on its landing page (`#/<slug>`).

**Languages**
- ✅ **C Concepts** (`c`) — 12 topics, done
- ⏳ C++ (`cpp`)
- ⏳ Shell Scripting (`shell`) — *next*

**CS fundamentals**
- ⏳ OOPS (`oops`) · Data Structures (`ds`) · Algorithms (`algo`) · DBMS & SQL (`db`) · Computer Networks (`net`)

**Systems**
- ⏳ Operating Systems (`os`) · Concurrency & Multithreading (`concurrency`) · Linux / Systems Programming (`linux`) · Computer Architecture (`arch`) · Build & Toolchain (`build`)

**Design & tools**
- ⏳ Low-Level Design (`lld`) · System Design (`sysd`) · Git (`git`)

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

## How the app is wired

All content, styling and routing live in `index.html`. The `<script>` holds three data
structures:

- `SKILLS` — slug → `{name, blurb}` for every area.
- `GROUPS` — home-page grouping/order.
- `TOPICS` — slug → `[[topicSlug, title, blurb], ...]` for **written** skills only.
- `PLANNED` — slug → `["Topic title", ...]` shown as a preview on skills not yet written.

The home grid and each skill landing page are generated from these; a skill counts as
"ready" once it has a `TOPICS` entry. Topic pages are static
`<section class="view topic" id="v-<skill>-<slug>">` blocks; the sidebar and breadcrumb
are filled by JS.

### Adding the next skill area

1. Move its list from `PLANNED.<skill>` into `TOPICS.<skill>` as `[slug, title, blurb]` rows.
2. Add one `<section class="view topic" id="v-<skill>-<slug>">` per topic, copying an
   existing C topic (sidebar `<nav class="sidebar" data-sidebar="<skill>">`, then the four
   `<details class="section">` blocks: Notes / Cheat Sheet / Q&A / Interview Questions).
3. Bump `CACHE_NAME` in `sw.js`.
4. Commit.
