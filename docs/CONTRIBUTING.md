# Contributing a venue

This repo accepts pull requests that **add a single venue** or **edit a
single venue**. Pull requests that touch more than one venue file in one
commit will be asked to split.

## Before you open a PR

Read these three files in order:

1. `LICENSE-NOTICES.md` — confirm your data source is in the allowed set
   and you understand which license it carries.
2. `docs/VENUE_SCHEMA.md` — the exact JSON shape your file must match.
3. `docs/ATTRIBUTION.md` — the required attribution string for your
   `provenance`.

## Choose a provenance

Pick exactly one:

- **`OSM_RACEWAY`** — geometry came from OpenStreetMap `highway=raceway`
  via Overpass. License: ODbL. The preferred contribution flow for OSM
  data is to fix the data **upstream in OSM** via JOSM or iD. Only PR a
  venue file here if the upstream OSM data is good and you want to
  pre-cache it for racer offline use.
- **`OSM_HISTORICAL`** — geometry came from OpenHistoricalMap. License: CC0.
- **`F1_CIRCUITS_GEOJSON`** — geometry came from
  `bacinger/f1-circuits` MIT GeoJSON. License: MIT.
- **`COURSE_WALK`** — geometry came from a course walk you recorded
  yourself (autocross). You own the data. License-grant under CC BY 4.0
  to this repo by submitting the PR.
- **`USER_ENTERED`** — geometry was entered by hand by you. You own the
  data. License-grant under CC BY 4.0.
- **`USER_SUBMITTED_TO_OSM`** — you submitted this venue to OSM under
  your own OSM account; the local copy here is a temporary cache until
  the OSM sync picks it up.

## File layout

Add or modify exactly one file:

```
venues/venue-{stable_id}.json
```

`{stable_id}` matches the JSON `stable_id` field. Naming convention:

- OSM-derived: `osm-{relation-or-way-id}` (e.g. `osm-12345678`)
- F1 circuits: `f1c-{upstream-id}` (e.g. `f1c-us-2012`)
- Driver-owned: anything you choose, kebab-case, lowercase, ASCII only

## PR checklist

- [ ] One venue file added or modified, no other files touched.
- [ ] File name matches `stable_id`.
- [ ] `provenance` is one of the six allowed values.
- [ ] `attribution` is set when `provenance` is OSM-derived or F1.
- [ ] `centerpoint` is present with valid `lat`/`lon`.
- [ ] `start_finish_line` is empty or exactly two LatLon objects.
- [ ] No data sourced from Podium, RacingCircuits.info, Motorsport
      Magazine, VBOX, RaceChrono, RaceBox, Google Maps, or Bing/Maxar
      (except Bing inside JOSM as part of an OSM contribution).
- [ ] PR description names the upstream source URL (or "course walk by
      <your name>" / "user-entered" for owned data).

## What happens after merge

Racer drivers who pull this repo into their local checkout will see your
venue on the next sync. There is no app-side approval step.

If your venue's `provenance` is `OSM_RACEWAY` and you later improve the
upstream OSM data, the next OSM Overpass sync inside racer will pull the
fresh data and overwrite this file. That is fine — the file is meant to
be a cache, not a fork.
