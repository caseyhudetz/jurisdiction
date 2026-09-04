# Jurisdiction — project notes for Claude Code

A single-page map of every civic, political and service boundary that contains a
given East Lakeview address, plus notes on who owns the physical stuff out front.

## Shape of the repo

- `index.html` — the whole app. Vanilla JS, Leaflet from CDN, no build step, no framework.
  This is the only file to edit for design or behaviour changes. Keep it under ~40KB.
- `data/layers.json` — generated boundary data, ~1.5MB. **Never hand-edit this file
  and never read it in full.** If you need to know its shape, read the first 2000
  characters or read `tools/build.py`, which produces it.
- `tools/build.py` — fetches from the city's ArcGIS service and open data portal,
  clips to East Lakeview, writes `data/layers.json`.
- `tools/content.py` — all the prose. Layer descriptions, contacts, the
  "who owns the ground" entries, the unmapped-jurisdiction list.

## Working rules

- Copy edits and new explanatory text go in `tools/content.py`, then re-run the build.
  Do not hardcode prose in `index.html`.
- New boundary layers: add to `FETCH_ARC` or `FETCH_PORTAL`, then `SPECS`, then
  `content.py`. The build script's docstring has the steps.
- `tools/.cache/` holds raw downloads. Delete it to force a fresh pull from the city.
- Test locally with `python3 -m http.server` then open localhost:8000.
  Opening `index.html` from the filesystem will fail because the fetch of
  `data/layers.json` is blocked by CORS.
- The app must keep working with no address selected. That empty state is deliberate.

## Writing style

- No em dashes.
- No Oxford comma.
- Plain declarative sentences. Say what a thing does and who to call about it.
  Avoid marketing tone.

## Data caveats worth preserving

- The city's neighborhood file still says "Boystown". Northalsted renamed in 2020.
  The app calls this out rather than silently correcting it.
- Officials' names come from the city's GIS attributes and can lag. If you update
  one, update it in `content.py` and note the source.
