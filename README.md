# racer-tracks-data

Curated, license-tagged race venue data files for the
[`racer`](https://github.com/drewid74/racer) project.

This repo is a **public, clean-room dataset of race venues** that the racer
companion app can consume offline. Drive-by mappers can contribute a single
venue (a `venue-{stableId}.json` file) without ever touching the racer Java
codebase.

## What's here

```
venues/
  venue-{stableId}.json     ← one file per venue, racer-native schema
docs/
  ATTRIBUTION.md            ← mandatory upstream credits + clean-room rules
  CONTRIBUTING.md           ← how to add or edit a venue safely
  VENUE_SCHEMA.md           ← exact JSON shape racer reads
LICENSE-NOTICES.md          ← per-source license posture (mixed-license repo)
```

## How racer consumes this repo

`racer` ships a `LocalVenueCache` that reads `venue-{stableId}.json` files
from a local directory and surfaces each one as a `Venue` value object. The
recommended setup is:

1. `git clone https://github.com/drewid74/racer-tracks-data.git`
   into a local directory of your choice
2. Point racer's `LocalVenueCache` at `<that-directory>/venues/`
3. Pull when you want fresh data; the cache is never written by racer at
   runtime — racer only **reads** from this checkout

## Clean-room rules

This repo is the public face of the racer venue dataset. To stay defensible
against the upstream license terms and against any competitor-positioning
concerns, only these source paths are allowed:

| Source | License | Allowed? |
|---|---|---|
| OpenStreetMap (`highway=raceway` via Overpass) | ODbL | YES, attribute upstream |
| OpenHistoricalMap | CC0 | YES, attribute upstream |
| `bacinger/f1-circuits` GeoJSON | MIT | YES, attribute upstream |
| Driver-recorded course walks | owned by driver | YES |
| User-entered venues | owned by contributor | YES |
| Podium.live | proprietary | NO, off-limits |
| RacingCircuits.info | all rights reserved | NO, off-limits |
| Motorsport Magazine, VBOX, RaceChrono, RaceBox, Google Maps | proprietary | NO, off-limits |
| Bing/Maxar imagery (except inside JOSM as part of OSM contribution) | restricted | NO, off-limits |

See `LICENSE-NOTICES.md` for the full posture and `docs/ATTRIBUTION.md` for
exact attribution strings.

## Contributing

Add or edit a venue by opening a pull request that adds or modifies a single
`venues/venue-{stableId}.json` file matching the schema in
`docs/VENUE_SCHEMA.md`. Every venue must declare its `provenance` and, when
the provenance is upstream-derived, its `attribution` string. See
`docs/CONTRIBUTING.md` for the step-by-step.

For OSM-derived venues, the preferred contribution flow is to fix or extend
the data in OpenStreetMap directly via JOSM or iD, then re-run the racer
Overpass sync. PRs to this repo are best for venues OSM does not yet cover
or for non-OSM provenances (course walks, user-entered).

## License posture

Mixed-license. See `LICENSE-NOTICES.md` for details.
