# Idea Map

A lightweight flowchart / blueprint tool for mapping out ideas. It runs entirely in the browser and has no backend, no build step and no dependencies. It is also installable as a Chrome app (PWA) that works offline.

## What it does

- **Boxes:** drag on empty canvas to create one, then type a title. Add lines of text, and the box grows and shrinks to fit them. Resize a box from its corners. Collapse it with the corner button, or hold that button for 2s to collapse or expand every box.
- **Arrows:** drag from a box border to another box. Arrows route along the grid with rounded corners and take the shortest path around other boxes. A selected arrow can be set to ignore boxes instead.
- **Nodes:** drag off an arrow, or drop onto one, to create a junction node there. Nodes can be dragged. A node left with one arrow in and one arrow out merges its two arrows back into one.
- **Unique titles:** no two boxes can share a title (case-insensitive). A clash turns the title red and blocks Enter; leaving the field renames a new box ("A 2") or reverts an existing one. Saved files with duplicates are fixed on load.
- **Selection:** click to select, Shift-click to add or remove, **Ctrl+A** to select every box, node and arrow (with one box selected, it selects that box's text instead). Selected items move, recolor and delete together.
- **Styling:** boxes and arrows can be colored. Boxes default to *Auto* (the half-white swatch), which follows the light/dark theme. Arrows also have a thickness, a line style (solid, dashed or dotted) and a label, and can be reversed.
- **Canvas:** pan and zoom, undo and redo, and light or dark theme (follows the system by default).
- **Saving:** the map autosaves in the browser. **Save** and **Load** export and import `.json` files.

## Files

```
ideamap-app/
├── index.html            the entire app (HTML + CSS + JS)
├── manifest.webmanifest  app name, icons, window mode (makes it installable)
├── sw.js                 service worker (offline cache)
└── icons/                app icons (192, 512, maskable 512)
```

## Hosting it on the Express server

Chrome only installs apps served over **HTTPS** (or `localhost`), so the folder needs to be served by a web server. Opening `index.html` directly won't offer an install.

1. **Put the folder in the server project**, for example at `public/ideamap-app/`, and commit it to the repo so the normal deploy picks it up.

2. **Serve it as static files** in the Express app, before any catch-all routes or auth middleware:

   ```js
   const path = require('path');
   app.use('/ideamap', express.static(path.join(__dirname, 'public', 'ideamap-app')));
   ```

3. **Restart the server** (e.g. `pm2 restart <app-name>`).

4. **Open `https://lionwolf.org/ideamap/`**. The Cloudflare Tunnel already provides HTTPS, so nothing else is needed.

5. **Install it:** click **⬇ Install** in the app's toolbar, or the install icon at the right end of Chrome's address bar.

Notes:

- **Keep the trailing slash** (`/ideamap/`). The app uses relative paths, so it works under any sub-path. Express redirects `/ideamap` to `/ideamap/` automatically.
- **Keep it public, or put it behind your login.** If it sits behind session auth, the service worker still caches it for offline use after the first visit.
- **Don't long-cache `sw.js`.** If you add caching headers, keep them off `sw.js` so browsers pick up new versions. Express's default (`maxAge: 0`) is fine.

## Testing locally (no server)

From inside the `ideamap-app` folder:

```
npx serve
```

Open the `http://localhost:...` link it prints. Chrome treats `localhost` as secure, so installing works there too.

## Updating the app

1. Replace `index.html` (and any other changed files) on the server.
2. The service worker fetches pages network-first, so the next online launch loads the new version automatically.
3. When you change the list of cached files in `sw.js`, also bump `CACHE` (e.g. `'ideamap-v2'`) so the old cache is cleared.

## Where data lives

- **Maps are stored in the browser's `localStorage`,** per origin. The installed app shares storage with `https://lionwolf.org` in Chrome, but not with other browsers, devices or a `localhost` copy.
- **To move a map,** use **Save** (downloads `ideamap.json`), then **Load** in the other copy.
- **Server-side storage isn't built in.** Syncing maps across devices would need an API route (for example `POST /api/maps`) plus a MySQL table.
