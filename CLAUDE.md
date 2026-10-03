# CLAUDE.md — Pizza Raccoon

> This project does NOT follow the conventions in the parent folder's CLAUDE.md
> (that one describes a Python/KVK audit-leads tool and is unrelated).

## What this is
A browser game. A raccoon steals ten real Mexican artefacts back from
Patrimonio S.A. — smugglers in uniform — using a pizza cutter.

## Tech
Single self-contained `index.html`. No build step, no dependencies, no
package manager. Fonts come from Google Fonts; everything else — art, audio,
world generation — is produced in code at runtime.

## Run it
Open `index.html` in a browser, or `python3 -m http.server 8788` and visit
<http://localhost:8788/index.html>. Serving over HTTP (rather than `file://`)
keeps the font loading and audio behaviour the same as in production.

## Conventions
- Keep it a single file. The whole point is that it runs anywhere with no setup.
- All art is drawn with canvas paths in a 16-unit nominal space and rasterised
  at `RS`× — never author pixel data by hand, and never add image assets.
- World coordinates are in pixels; 1 tile = `TS` = 16px. The render transform
  scales nominal units to device pixels, so game code never deals with scale.
- No tests unless asked.

## Gotchas
- The game loop is `requestAnimationFrame`-driven, so it **pauses when the tab
  or preview pane is hidden**. When verifying behaviour headlessly, call
  `update(dt)` in a loop directly rather than waiting on wall-clock time.
- Entities that end up inside solid geometry are rescued by `unstick()`;
  collision blocks both axes otherwise and they would be trapped forever.
- Progress is saved to `localStorage` under `mapache-v2`. Bump that key if the
  save shape changes.
