# Venue file schema

Every file in `venues/` is a single JSON object describing one venue. The
shape mirrors the racer-native `com.racer.venues.Venue` Java value object
exactly so the file round-trips through racer's `LocalVenueCache` without
translation.

## File name

```
venues/venue-{stableId}.json
```

`{stableId}` matches the JSON `stable_id` field. For OSM-derived venues
the convention is `osm-{relation-or-way-id}`; for F1 circuits venues it is
`f1c-{upstream-id}`; for driver-owned venues it is whatever the
contributor chooses, kebab-case, lowercase, ASCII only, max 64 chars.

## JSON shape

```json
{
  "stable_id": "lime-rock-fcp",
  "osm_relation_id": null,
  "osm_way_ids": [],
  "name": "Lime Rock Park",
  "country": "US",
  "configuration": "Full Course Plus",
  "surface": "asphalt",
  "length_meters": 2410.0,
  "centerpoint": { "lat": 41.9286110, "lon": -73.3844440 },
  "centerline": [
    { "lat": 41.9286110, "lon": -73.3844440 },
    { "lat": 41.9287000, "lon": -73.3843000 }
  ],
  "start_finish_line": [
    { "lat": 41.9286500, "lon": -73.3844200 },
    { "lat": 41.9286700, "lon": -73.3844000 }
  ],
  "sector_points": [
    { "lat": 41.9287500, "lon": -73.3843500 },
    { "lat": 41.9288500, "lon": -73.3842500 }
  ],
  "provenance": "OSM_RACEWAY",
  "attribution": "Map data from OpenStreetMap (https://www.openstreetmap.org/copyright)",
  "last_synced_at": "2026-05-09T12:00:00Z"
}
```

## Field rules

| Field | Type | Required | Notes |
|---|---|---|---|
| `stable_id` | string | yes | Matches the file name. |
| `osm_relation_id` | integer or null | no | When the venue is an OSM `type=circuit` relation. |
| `osm_way_ids` | array of integer | no | When the venue is one or more OSM `highway=raceway` ways. Empty array if none. |
| `name` | string | yes | Human-readable. |
| `country` | string or null | no | ISO-2 country code or full country name. |
| `configuration` | string or null | no | E.g. "Full Course Plus", "Infield". |
| `surface` | string or null | no | E.g. "asphalt", "concrete", "tarmac". |
| `length_meters` | number or null | no | Track length in meters. |
| `centerpoint` | LatLon object | yes | Approximate venue center. |
| `centerline` | array of LatLon | no | Polyline of the racing surface. Empty array if not yet captured. |
| `start_finish_line` | array of LatLon | no | Empty, or **exactly two** LatLon objects (the two ends of the line). |
| `sector_points` | array of LatLon | no | Each LatLon is the midpoint of a sector line. |
| `provenance` | enum string | yes | One of: `OSM_RACEWAY`, `OSM_HISTORICAL`, `F1_CIRCUITS_GEOJSON`, `COURSE_WALK`, `USER_ENTERED`, `USER_SUBMITTED_TO_OSM`. |
| `attribution` | string or null | depends | **Required** for `OSM_RACEWAY`, `OSM_HISTORICAL`, `F1_CIRCUITS_GEOJSON`. Optional for driver-owned provenances. |
| `last_synced_at` | ISO 8601 string or null | no | When the file was last refreshed from upstream. |

### LatLon shape

```json
{ "lat": 41.9286110, "lon": -73.3844440 }
```

`lat` must be in `[-90, 90]`, `lon` must be in `[-180, 180]`. Both must
be finite numbers. Racer rejects NaN or out-of-range values.

## Authoritative-layout note

For autocross venues, the **walked course** owns the layout. A driver
recording a course walk for an event should set `provenance: "COURSE_WALK"`
on the venue file the racer companion app produces, and the racer
analyzer will treat that file as authoritative for the session. Any
`OSM_RACEWAY` venue at the same lat/lon is basemap context only.

## Validating a file

A future `racer-tracks-data` CI workflow will validate every PR against
this schema. Today, validation is manual: load the file with
`com.racer.venues.LocalVenueCache.findByStableId` and confirm it returns
without an exception. If `LocalVenueCache` rejects the file, the file is
not yet valid.
