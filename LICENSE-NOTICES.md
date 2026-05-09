# License notices

This repo is intentionally a **mixed-license dataset**. Different files in
the `venues/` directory carry different upstream license obligations,
recorded in each file's `provenance` and `attribution` fields. This file
states the racer-side rules so contributors and consumers are not surprised.

## Repository scaffolding (everything outside `venues/`)

Files in this repo other than `venues/*.json` (this `LICENSE-NOTICES.md`,
`README.md`, `docs/*.md`, schema files, contribution guides) are licensed
**MIT** by the racer-tracks-data maintainer, copyright © 2026 Drew Fair.
You may copy, modify, redistribute, fork, or reuse them under the MIT terms.

## Venue data files (`venues/*.json`)

The license that applies to a given venue file is determined by its
`provenance` field, not by this file. The maintainer of this repo does not
override upstream licenses.

### `provenance: "OSM_RACEWAY"` and `provenance: "OSM_HISTORICAL"`

Source: OpenStreetMap and OpenHistoricalMap. License: **ODbL 1.0**
(<https://opendatacommons.org/licenses/odbl/1-0/>).

Anyone using these files must:

- Attribute "Map data from OpenStreetMap" linked to
  <https://www.openstreetmap.org/copyright> in any rendered Produced Work.
- License any **Derived Database** they ship to other users under ODbL 1.0
  with access to the alterations.
- Personal local use is exempt from share-alike.

### `provenance: "F1_CIRCUITS_GEOJSON"`

Source: <https://github.com/bacinger/f1-circuits>. License: **MIT**.

Anyone using these files must include the upstream MIT notice and attribute
the upstream repo in any rendered Produced Work.

### `provenance: "COURSE_WALK"`, `provenance: "USER_ENTERED"`, `provenance: "USER_SUBMITTED_TO_OSM"`

Source: contributor-owned. By submitting a pull request that adds or
modifies a venue file with one of these provenances, the contributor
license-grants the file under **CC BY 4.0**
(<https://creativecommons.org/licenses/by/4.0/>) so racer and other
downstream consumers can use, modify, and redistribute it with attribution.

For `USER_SUBMITTED_TO_OSM`, the contributor is also encouraged to upload
the same data to OpenStreetMap under their own OSM account; once OSM has it,
the canonical record becomes OSM and this file should be marked stale or
deleted.

## Off-limits sources

The racer-tracks-data repo will not accept pull requests that derive data,
in whole or in part, from any of the following — even with attribution.
This is a clean-room rule that protects both the upstream rights holder and
the racer project's competitive positioning.

- **Podium.live** (any URL, any field, any schema)
- **RacingCircuits.info**
- **Motorsport Magazine** circuit database
- **Racelogic VBOX** track database (`.bdb` or otherwise)
- **RaceChrono** and **RaceBox** track libraries
- **Google Maps** / **Google Earth** imagery as a programmatic source
- **Bing** / **Maxar** imagery as a redistributable source (Bing inside
  JOSM as part of an OSM contribution is the only allowed touch point)

See the racer repo's `docs/track_map_data_sources.md` for the full
clean-room rationale.

## Provenance audit

Every venue file's `provenance` field is the binding declaration of which
license applies. PRs that add a venue without a `provenance` field, or with
a value not in the enum above, will be rejected.
