# Attribution rules

Every Produced Work that displays venue geometry from this repo must
display the right attribution string for every upstream the venues come
from. The racer Java code does this via `VenueAttributionRenderer`; if
you're consuming this repo from somewhere else, you must reproduce the
same behavior.

## Required strings (verbatim)

### OSM-derived venues
Provenance: `OSM_RACEWAY` or `OSM_HISTORICAL`.

```
Map data from OpenStreetMap (https://www.openstreetmap.org/copyright)
```

### F1 circuits venues
Provenance: `F1_CIRCUITS_GEOJSON`.

```
Track geometry from bacinger/f1-circuits (MIT, https://github.com/bacinger/f1-circuits)
```

### Driver-owned venues
Provenance: `COURSE_WALK`, `USER_ENTERED`, `USER_SUBMITTED_TO_OSM`.

No upstream attribution required. The contributor owns the data and has
license-granted it under CC BY 4.0 to this repo.

## Display requirements

- Visible in the same surface that renders the venue geometry (map view,
  layout overlay, telemetry comparison view).
- May be collapsed / shown-on-tap after 5 seconds, but must reappear when
  the user interacts with the venue.
- Multiple upstreams contributing to one rendered view must each be
  attributed; deduplicate by upstream, not by venue.
- Never replace the OSM `https://www.openstreetmap.org/copyright` link
  with a paraphrase or a wrapper page. The link is part of the
  attribution.

## Verifying compliance

The racer companion app uses `com.racer.venues.VenueAttributionRenderer`
for this. If you're building a new consumer of this repo, mirror that
class's behavior:

1. Walk the in-use venue list.
2. For each venue, pick its `attribution` field. If empty, fall back to
   the per-provenance default above.
3. Driver-owned provenances contribute nothing.
4. Deduplicate while preserving first-seen order.
5. Render as lines, a multiline block, or an inline string.
