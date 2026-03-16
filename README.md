# Americana Pedestrian Map Styles

Pedestrian map styles for [MapLibre GL JS](https://maplibre.org/maplibre-gl-js/docs/), built on the [OpenStreetMap Americana](https://github.com/osm-americana/openstreetmap-americana) base style using the [OpenMapTiles schema](https://openmaptiles.org/schema/) hosted at `tiles.openstreetmap.us`.

This is intended as a proof-of-concept, developed for the [Pedestrian Walking Group (PWG)](https://github.com/lumikeiju/osm-us-pwg) to support walking trip planning and OpenStreetMap data quality review.

---

## Styles

### Trip Planning (in progress)
A pedestrian overlay built on top of the [OpenStreetMap Americana](https://github.com/osm-americana/openstreetmap-americana) base style (minus highway shields).

Style file: `styles/americana-pedestrian-trip-planning.json`

### QA (coming soon)
A data quality visualization showing where OSM pedestrian tags are missing or incomplete, per the PWG Bronze → Diamond tier schema.

---

## Tile Schema

### Pedestrian Features

Pedestrian style layers are limited by what is available in the [OpenMapTiles Schema](https://openmaptiles.org/schema/#transportation). Currently visualized:
| Layer ID | Filter | What It Shows | Color |
|----------|--------|---------------|-------|
| `path-unpaved` | `class=path`, `surface=unpaved` | Unpaved paths and sidewalks | Green dashed |
| `path-paved` | `class=path`, surface ≠ `unpaved` | Paved paths and sidewalks | grey with green casing |
| `steps` | `subclass=steps` | Stairways | Dark green dashed |

Potential refinements:
| Layer ID | Filter | What It Shows |
|----------|--------|---------------|
| `sidewalk` | `subclass=footway` | Sidewalks, distinct from parks in paths (currently lumped in with path styles) |
| `pedestrian-street` | `subclass=pedestrian` | Pedestrian-only streets (currently lumped in with path styles) |

### Limitations

These Limitations are the result of shortcuts to keep this proof-of-concept simple. There are solutions, they just require extra effort to implement.

#### OpenMapTiles

The OpenMapTiles schema (on which Americana is based) does not include all pedestrian attributes, e.g.:

- `tactile_paving`, `kerb`, `kerb:height`
- `crossing:markings`, `crossing:signals`, `button_operated`
- `incline`, `incline:across`
- `width`, `lit`
- `sidewalk:left/right/both`

To style these, we would add in sources. There are a variety of approaches for doing this that depend on scale and size of each data layer overall.

#### Americana

These pedestrian styles were developed explicitly to harmonize with Americana map without modifying the existing styles. This limitation means the pedestrian features are limited in how prominent we can make them, given how strongly Americana styles motor-vehicle roads.

The alternative would be to redevelop the Americana map style to give more prominence to pedestrian features.

Shields and icons (e.g. for POIs) do not work on this version of the Americana map style because the developers of Americana have custom, dynamic ways of rendering these.

---

## View Locally

```bash
# Python (no dependencies)
python3 -m http.server 8080
```

Then open `http://localhost:8080`.

### Edit (in Maputnik)

The hosted [Maputnik editor](https://maputnik.github.io) requires CORS headers to load styles from localhost. Use `npx serve` instead:

```bash
npx serve --cors -p 8080
```

Then in Maputnik: **Open → Load from URL** → `http://localhost:8080/styles/americana-pedestrian-trip-planning.json`

---

## Data Sources

Tile hosting by [OSMUS](https://openstreetmap.us/). Map data © [OpenStreetMap contributors](https://www.openstreetmap.org/copyright).
