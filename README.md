# Jurisdiction

Every civic, political and service boundary the City of Chicago has drawn through a
single East Lakeview address, on one map. Search an address, see the 36 jurisdictions
that contain it, the five more the city does not publish, and 15 notes on who actually
owns the sidewalk, the parkway tree, the alley and the pipe underneath.

Static site. One HTML file plus one generated JSON file. No framework, no build step
for the front end, no API keys.

## Run locally

```bash
python3 -m http.server
# open http://localhost:8000
```

Opening `index.html` directly from disk will not work. The page fetches
`data/layers.json` and the browser blocks that over `file://`.

## Rebuild the data

```bash
pip install -r tools/requirements.txt
python3 tools/build.py
```

Pulls from the City of Chicago ArcGIS service and the open data portal, clips to
East Lakeview, and rewrites `data/layers.json`. Raw downloads are cached in
`tools/.cache/`; delete that directory to force a fresh pull.

## Deploy

Cloudflare Workers, static assets only. There is no build step and no server code.

The Worker is connected to this repo through Cloudflare Workers Builds. A push to
`main` triggers a build that runs `npx wrangler deploy` from the repo root. There
is nothing to configure in GitHub.

To deploy by hand:

```bash
npx wrangler deploy
```

`wrangler.jsonc` serves the repo root. `.assetsignore` keeps the source and the
notes out of the bundle.

## Sources

- City of Chicago ArcGIS: `gisapps.chicago.gov/arcgis/rest/services/ExternalApps`
- Chicago open data portal: `data.cityofchicago.org`
- Geocoding: Nominatim (OpenStreetMap)
- Base map: OpenStreetMap
