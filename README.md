# Esfahan Metro Data

An open JSON dataset and interactive network visualization for the Esfahan Metro in Iran. The dataset currently describes 28 stations across Line 1 and the documented section of Line 2, including station names, operating status, connections, coordinates, addresses, and available facilities.

## Project structure

```text
.
├── data/
│   └── stations.json       # Canonical station dataset
└── visualization/
    └── index.html          # Interactive network graph
```

The visualization loads `data/stations.json` at runtime. Station data is not duplicated in the HTML.

## Run the visualization

Because the page fetches a local JSON file, serve the project over HTTP instead of opening `index.html` directly with a `file://` URL.

Using Python:

```bash
python -m http.server 8000
```

Then open <http://localhost:8000/visualization/>.

You can search stations by Persian name, English name, address, or line number. Select a station in the graph or search results to inspect its status, address, coordinates, lines, and recorded facilities.

## Dataset format

`stations.json` is an object keyed by a stable station identifier. Each value follows this general shape:

```json
{
  "ImamHossein": {
    "name": "Imam Hossein",
    "translations": { "fa": "امام حسین" },
    "lines": [1, 2],
    "colors": ["#E0001F", "#0072BC"],
    "status": "active_interchange",
    "isOperational": true,
    "relations": ["Takhti", "Enqelab", "Falestin"],
    "address": "...",
    "longitude": "51.669345",
    "latitude": "32.658118",
    "coordinateSource": "...",
    "coordinateVerified": true
  }
}
```

Important fields:

- `name` and `translations.fa`: English and Persian display names.
- `lines` and `colors`: metro lines serving the station and their display colors.
- `status`: `active`, `active_interchange`, or `under_construction`.
- `relations`: identifiers of directly connected stations.
- `latitude` and `longitude`: station coordinates stored as strings.
- `coordinateSource` and `coordinateVerified`: coordinate provenance and verification state.
- Facility fields such as `wc`, `elevator`, and `atm`: `true`, `false`, or `null` when unknown.

## Editing the data

When adding or changing a station:

1. Keep the top-level identifier stable and unique.
2. Ensure every value in `relations` matches another top-level identifier.
3. Add a relation in both directions for a bidirectional connection.
4. Keep `lines` and `colors` in matching order.
5. Record the coordinate source and set `coordinateVerified` accurately.
6. Validate that `stations.json` remains valid UTF-8 JSON.

## Technology

The visualization is a dependency-light static page built with HTML, CSS, vanilla JavaScript, and [VivaGraphJS](https://github.com/anvaka/VivaGraphJS). It requires no build step.
