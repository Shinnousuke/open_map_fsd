# OpenMap — Explore Nearby

**OpenMap** is a lightweight, browser-based map application for exploring places around a location. Search for a place or address, view nearby points of interest on an interactive map, filter results by category, adjust the search radius, inspect place details, save places for later, and open directions.

> **Project type:** Front-end web application  
> **Map library:** Leaflet 1.9.4  
> **Map data:** OpenStreetMap  
> **Location search:** Nominatim  
> **Nearby-place data:** Overpass API  
> **Persistence:** Browser `localStorage`

OpenMap uses open map data and public services. Availability and response times depend on those services and your network connection.

## Table of contents

- [Overview](#overview)
- [Features](#features)
- [Technology stack](#technology-stack)
- [How it works](#how-it-works)
- [Getting started](#getting-started)
- [Using OpenMap](#using-openmap)
- [External services and attribution](#external-services-and-attribution)
- [Project structure](#project-structure)
- [Privacy and permissions](#privacy-and-permissions)
- [Troubleshooting](#troubleshooting)
- [Known limitations](#known-limitations)
- [Future improvements](#future-improvements)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

OpenMap brings location search, an interactive map, nearby-place discovery, filters, place details, saved locations, and directions into one interface. It is designed as a client-side website and does not require a user account or a dedicated application server for its core features.

The project focuses on making nearby discovery easier to understand. Clear validation, loading feedback, useful empty states, and actionable error messages are important because the application relies on network-based services.

## Features

### Interactive map
- Interactive map rendered with Leaflet.
- OpenStreetMap map tiles and attribution.
- Map markers for the selected location and nearby places.
- Controls to refresh nearby results, recenter the map, and request the current location.
- Map-layer selector for switching between the map presentations configured by the application.

### Location search
- Search for a place or address.
- Resolve a query to coordinates through Nominatim.
- Move the map to the matching location and load nearby places.
- Check HTTP responses, parse JSON, and validate coordinates before using them.
- Provide feedback when a request fails or a location cannot be found.

### Nearby-place discovery
Available category filters include:
- **All**
- **Café**
- **Restaurant**
- **Shop**
- **Park**
- **Pharmacy**
- **ATM**
- **Other**

The places returned depend on OpenStreetMap coverage and the query/category rules in the application.

### Adjustable search radius
Users can adjust the nearby search radius, approximately **500 metres to 5 kilometres**. A larger radius may return more places and take longer to query.

### Current location
The browser Geolocation API can provide the user's coordinates after permission is granted. If permission is denied or location is unavailable, users can still search manually.

### Place details and directions
Selecting a result opens a details view or modal with available information, such as the place name, type/category, address, and coordinates. The directions action opens an external directions destination; navigation itself is handled by the external service.

### Saved places
- Save a place for later.
- View, reopen, and remove saved places.
- Store saved-place data in browser `localStorage`.
- No account is required for local saving.

Saved places belong to the browser profile and site origin where they were created. They are not automatically synchronised across devices.

### Request feedback
The interface includes status/toast feedback and loading-state handling for key operations. Search logic checks HTTP status, parses JSON responses, validates location coordinates, and uses request identification to help prevent older search responses from replacing newer ones.

## Technology stack

| Technology | Purpose |
|---|---|
| HTML5 | Page structure, forms, controls, lists, modal and status regions |
| CSS | Layout, visual styling and responsive presentation |
| JavaScript (ES6+) | Event handling, application logic, map interaction and asynchronous requests |
| Leaflet 1.9.4 | Interactive map, markers, views and tile layers |
| OpenStreetMap | Geographic data and map tiles |
| Nominatim | Place/address search and reverse geocoding |
| Overpass API | Queries nearby OpenStreetMap points of interest |
| Fetch API | HTTP requests to external services |
| JSON | API data exchange and local saved-place serialization |
| Browser Geolocation API | Requests current coordinates |
| `localStorage` | Stores saved places in the browser |
| External directions service | Opens a destination for navigation |

**Styling note:** The current project uses HTML and CSS. Tailwind CSS is an optional future styling approach; do not list it as an installed dependency unless it has actually been added.

## How it works

1. The user searches for a place/address or requests their current location.
2. Nominatim resolves a text query to coordinates, or the browser Geolocation API provides coordinates after permission.
3. Leaflet updates the map view.
4. OpenMap sends an Overpass query based on the selected coordinates, category, and radius.
5. The response is parsed and used to update the nearby-place list and map markers.
6. The user can refine the category/radius, inspect a place, save it locally, or open directions.

```text
Search / current location
          |
          v
 Resolve coordinates
 (Nominatim / Geolocation)
          |
          v
   Update Leaflet map
          |
          v
 Query nearby places (Overpass)
          |
          v
 Parse JSON and validate data
          |
          v
 Update markers and result list
       /             \
      v               v
 Place details    Save locally
      |
      v
 External directions
```

### Fetch API and JSON

The request flow is generally:

1. Build a URL and its query parameters.
2. Send the request with `fetch()`.
3. Check `response.ok`.
4. Parse the response using `response.json()`.
5. Validate the expected structure and required fields.
6. Validate coordinates before updating the map.
7. Update the UI or show an error/empty state.

Saved places are serialised with `JSON.stringify()` before being written to `localStorage` and restored with `JSON.parse()`. Storage access and parsing should be handled defensively because browser storage may be unavailable or contain invalid data.

## Getting started

### Requirements
- A modern browser (Chrome, Firefox, Edge, Safari, or equivalent).
- Internet access for map tiles, geocoding, and nearby-place requests.
- A local web server for the most reliable development experience, especially when testing geolocation.

### 1. Clone the repository

Replace the placeholders with your GitHub username and repository:

```bash
git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
cd YOUR-REPOSITORY
```

### 2. Run locally

If the main page is `index.html`, you can open it for basic testing. For geolocation and consistent local testing, run a local HTTP server from the project folder:

```bash
python -m http.server 8000
```

Open <http://localhost:8000> in your browser.

Alternatively, open the folder in Visual Studio Code and use a local server extension such as Live Server.

No API key is normally required for the public endpoints used by the project, but public-service availability, policies, and rate limits apply.

### 3. Deploy with GitHub Pages

1. Push the project to a GitHub repository.
2. Open **Settings → Pages**.
3. Choose the branch and folder that contain the website (commonly `main` and `/root`).
4. Save and wait for deployment.
5. Open the published URL and test search, map tiles, current location, and nearby results.

Ensure the publishing folder contains the entry-point HTML file and all referenced assets. GitHub Pages menu labels may change over time.

## Using OpenMap

1. Enter a place name or address and submit the search.
2. Or select the current-location control and grant browser permission.
3. Explore the map by panning and zooming.
4. Select a category to narrow the results.
5. Adjust the search radius to include a smaller or larger area.
6. Use refresh/recenter controls when needed.
7. Select a marker or result to inspect place details.
8. Save a place to keep it in the saved list.
9. Reopen or remove saved places.
10. Use the directions action to continue in an external navigation service.

Results depend on the selected area, OpenStreetMap coverage, category mapping, radius, and service availability.

## External services and attribution

Each external service has its own terms, usage policies, and operational limits.

### OpenStreetMap
- Website: <https://www.openstreetmap.org/>
- Copyright and attribution: <https://www.openstreetmap.org/copyright>

Keep the required OpenStreetMap attribution visible. OpenStreetMap data is open, but its standard public tile service is not an unlimited tile-hosting service. Review the tile usage policy before deploying a high-traffic application.

### Leaflet
- Website: <https://leafletjs.com/>
- Documentation: <https://leafletjs.com/reference.html>

The project references Leaflet 1.9.4 through a CDN. If changing the version or delivery method, update the relevant CSS and JavaScript references together.

### Nominatim
- API documentation: <https://nominatim.org/release-docs/latest/api/Overview/>
- Public usage policy: <https://operations.osmfoundation.org/policies/nominatim/>

The public Nominatim service has usage requirements and rate limits. Avoid unnecessary or repeated requests, follow its identification requirements, and do not assume the public endpoint is suitable for unrestricted high-volume production use.

### Overpass API
- Documentation: <https://wiki.openstreetmap.org/wiki/Overpass_API>
- Public instance information: <https://wiki.openstreetmap.org/wiki/Overpass_API#Public_Overpass_API_instances>

Overpass can be slow or temporarily unavailable. Query only the data needed and handle failed responses. If the project defines an alternate endpoint, confirm in the code whether it is actually used as a fallback.

### Geolocation
- Documentation: <https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API>

Geolocation generally requires a secure context such as HTTPS or a local origin such as `localhost`. Users may deny permission or their device may be unable to determine a location, so manual search should remain available.

## Project structure

A simple static layout may look like this:

```text
openmap/
├── index.html          # Main application page
├── style.css           # Optional external stylesheet
├── script.js           # Optional external JavaScript
└── README.md           # Project documentation
```

If your current implementation keeps CSS and JavaScript inside `index.html`, separate `style.css` and `script.js` files are not required. Keep the README aligned with the actual files in your repository.

## Privacy and permissions

- **Location permission:** The browser controls the prompt. Current coordinates are requested when the location feature is used.
- **Manual search:** Users can search for a place without using device geolocation.
- **Saved places:** Data is stored in the current browser's `localStorage`, not in a cloud account.
- **External requests:** Nominatim and Overpass receive the query or coordinates needed to provide results and may receive ordinary request metadata such as IP address.
- **No login required:** The core project does not require an application account.

Avoid storing sensitive personal information in saved-place records. Clearing site data in the browser may delete saved places.

## Troubleshooting

| Problem | Possible reason | What to try |
|---|---|---|
| Map is blank | Network issue, blocked tiles, or JavaScript error | Check the connection and browser console; verify Leaflet CSS/JS load |
| No search result | Ambiguous, misspelled, or unindexed query | Try a more specific place name or address |
| Search is slow or fails | Network/service delay or rate limit | Wait briefly and retry; avoid repeated rapid submissions |
| No nearby places | Sparse map data, active category filter, small radius, or Overpass failure | Select All, increase the radius, or try another location |
| Current location fails | Permission denied, unsupported context, disabled device location, or timeout | Enable permission or use manual search |
| Saved places do not sync | Browser storage is local to a browser profile and origin | Use the same browser profile; cross-device sync requires additional infrastructure |
| Saved places disappear | Storage cleared or restricted | Check browser settings and use a normal browsing session |
| Directions do not open | External URL or browser navigation behaviour | Check browser settings and retry |

For debugging, inspect the browser developer tools **Console** and **Network** tabs. Do not post private tokens or sensitive location data in public issue reports.

## Accessibility and responsive design

When maintaining or restyling OpenMap:
- Use semantic HTML and correctly associated form labels.
- Keep controls keyboard accessible and provide visible focus styles.
- Do not communicate status through colour or icons alone.
- Make loading, error, and empty-result messages understandable.
- Give icon-only buttons accessible names.
- Ensure modals can be closed with supported controls and keyboard interaction.
- Keep map attribution visible.
- Test narrow viewport widths and browser zoom.

The project uses CSS for styling today. A Tailwind CSS migration is optional and should be documented only after it is implemented.

## Known limitations

- **External dependencies:** Map tiles, geocoding, and nearby-place data depend on public services that may be slow, unavailable, or rate-limited.
- **Data completeness:** OpenStreetMap may not contain every place; addresses, opening hours, and other details can be missing.
- **Category mapping:** Filters depend on tags and query rules, so some places may not match the expected category.
- **Local-only saved places:** Saved places do not automatically sync across devices or browsers.
- **No built-in navigation engine:** Directions are handed off to an external service.
- **Geolocation permission:** Current-location functionality depends on browser/device support and user permission.
- **No backend account system:** There is no built-in authentication or cloud storage in the core client-side app.
- **Production scale:** Public API and tile endpoints may not suit high-traffic commercial deployment.

## Future improvements

Possible next steps:
- Add more explicit inline search validation and correction guidance.
- Make loading states consistent across search, nearby results, and geolocation.
- Add clearer empty states with suggestions such as expanding the radius or changing categories.
- Improve error messages and add safe retry controls.
- Strengthen keyboard navigation and screen-reader announcements.
- Apply stale-request protection consistently to all asynchronous operations.
- Sort nearby results by distance where reliable coordinates are available.
- Add categories or improve OpenStreetMap tag coverage.
- Add optional route previews or distance estimates.
- Support export/import of saved places or opt-in cloud sync.
- Add automated tests for search validation, storage, API errors, and responsive layouts.
- Document a production data-service strategy that respects each provider's policy.

These are ideas for future work, not claims about existing functionality.

## Contributing

Bug reports, suggestions, and pull requests are welcome.

1. Fork the repository.
2. Create a branch:

   ```bash
   git checkout -b feature/your-change
   ```

3. Make a focused change and test the affected workflow in a browser.
4. Check the browser console and confirm loading states are cleared after failures.
5. Commit your changes:

   ```bash
   git add .
   git commit -m "Describe your change"
   ```

6. Push your branch and open a pull request.

Before submitting, test valid and empty searches, no-result searches, category and radius changes, location permission granted/denied, saving/removing places, API failure states, and narrow-screen layouts. Preserve required map attribution and follow external-service policies.

## License

No project license is specified here. Before publishing the repository for reuse, choose a license that reflects your intentions and add a `LICENSE` file at the repository root.

Also review the licenses, attribution requirements, and usage policies for third-party libraries, map data, tiles, and APIs. A license for your own code does not replace those separate requirements.

---

## Acknowledgements

- **OpenStreetMap contributors** — geographic data.
- **Leaflet** — interactive mapping library.
- **Nominatim** — geocoding and reverse geocoding.
- **Overpass API** — nearby OpenStreetMap feature queries.
- **Browser APIs** — Fetch, Geolocation, and Web Storage.

---

**OpenMap — Explore Nearby**  
*Search locations. Discover nearby places. Save what matters.*
