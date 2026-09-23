<h1 align="center">Netflix Content Visualization</h1>
<p align="center">
  An interactive, three scenes exploration of a Netflix titles dataset.
</p>
<p align="center">
  <img src="https://img.shields.io/badge/Language-JavaScript-F7DF1E?style=flat-square" alt="Language: JavaScript">
  <img src="https://img.shields.io/badge/Charts-D3.js-F9A03C?style=flat-square" alt="Charts: D3.js">
  <img src="https://img.shields.io/badge/Maps-Leaflet-199900?style=flat-square" alt="Maps: Leaflet">
  <img src="https://img.shields.io/badge/Type-Narrative%20Visualization-E50914?style=flat-square" alt="Type: Narrative Visualization">
</p>
<p align="center">
  <a href="#overview">Overview</a> ·
  <a href="#features">Features</a> ·
  <a href="#quick-start">Quick Start</a> ·
  <a href="#explore-the-scenes">Explore the Scenes</a> ·
  <a href="#architecture">Architecture</a> ·
  <a href="#known-issues">Known Issues</a>
</p>

---

# Overview
Explore how the titles in a bundled Netflix dataset differ by **content type**, **genre**, and **listed country**. The project is a browser-based narrative visualization: start with a timeline, compare genre rankings, then explore an interactive world map.
> **Reading the charts:** Counts describe titles recorded in the supplied dataset. They do not measure Netflix production output, the full live catalog, or audience viewing behavior.

---

# Features
| | Feature | Implementation |
| :---: | --- | --- |
| 📈 | Content timeline | Movie and TV Show title counts by year added, plus an overall type breakdown |
| 🎬 | Genre comparison | Top-ten genre bar charts with a Movie / TV Show toggle |
| 🌍 | Interactive map | Title counts by listed country, split between Movies and TV Shows |
| 🎛️ | Exploration controls | Year slider and multiselect genre filter on the map |
| 🖱️ | Chart interactions | Hover tooltips on chart marks and map markers |
| 🧭 | Navigation | Links between three scenes and an optional intro page |

---

# Quick Start
## 1. Prepare your environment
- Install **Python 3** or use another local HTTP server.
- Use a modern browser with internet access for the libraries loaded from CDNs and the map tiles.
- Download or clone the repository, keeping the `resources/` directory alongside the HTML files.

No Python packages or Node.js build step are required by the supplied project.

## 2. Start a local server
Open a terminal in the repository root, where `scene1.html` and `resources/` are located:

```sh
python -m http.server 8000
```

On Windows, `py -m http.server 8000` is an alternative if `python` is not recognized. Leave this terminal running while browsing the project.

## 3. Open the project

| Starting point | Local URL |
| --- | --- |
| Intro page with video | [http://localhost:8000/text.html](http://localhost:8000/text.html) |
| First visualization | [http://localhost:8000/scene1.html](http://localhost:8000/scene1.html) |

Use the **Scene 1 / Scene 2 / Scene 3** links in the interface to move between views. Stop the server with **Ctrl + C** in the terminal.

<details>
<summary><strong>Using Visual Studio Code</strong></summary>
<ol>
<li>Open the repository root in VS Code.</li>
<li>Open **Terminal → New Terminal**.</li>
<li>Run <code>`python -m http.server 8000`</code>.</li>
<li>Open the local URL above in your browser.</li>

Use an HTTP server instead of opening the HTML files directly with `file://`: the JavaScript loads CSV files with `d3.csv(...)` and the map uses an ES module.
</ol>
</details>

<details>
<summary><strong>Troubleshooting</strong></summary>

| Symptom | Check |
| --- | --- |
| `python` is not recognized | Install Python 3, or try `py -m http.server 8000` on Windows. |
| Browser shows 404 for a CSV | Start the server in the repository root; keep the `resources/data/` files in place. |
| Charts or map are blank | Check the browser developer console and confirm the CDN scripts load. |
| Map tiles do not load | Allow external tile requests; see the HTTP tile URL note under Known Issues. |
| Port 8000 is occupied | Run `python -m http.server 8001` and use `localhost:8001`. |

</details>

---

# Explore the Scenes
| Scene | What it shows | How to interact |
| --- | --- | --- |
| [**Scene 1 · Timeline**](scene1.html) | Counts of Movies and TV Shows by `date_added` year; a pie chart compares the two types. | Hover chart points to see year and count. |
| [**Scene 2 · Genres**](scene2.html) | Top-ten `listed_in` categories for Movies and TV Shows. | Click the toggle button to switch content type; hover bars for counts. |
| [**Scene 3 · Map**](scene3.html) | Movie and TV Show counts for countries listed on titles with available coordinates. | Enable the year slider and select one or more genres; inspect markers. |

The [intro page](text.html) presents the project and links into the visualization. On Scene 3, select all genres with **Ctrl + A** (or **⌘ + A**) inside the genre selector to restore the full genre selection.

<details>
<summary><strong>How the data is interpreted</strong></summary>

- Scene 1 groups records by the year in `date_added`. This is the year a title was added to the dataset's Netflix catalog, not necessarily its `release_year`.
- Scene 2 reads `listed_in` values from `netflix_titles.csv` to rank categories. A title can contribute to multiple genre categories if its field lists multiple values.
- Scene 3 uses the cleaned CSV. A title listed with multiple countries can contribute to more than one country's total. Markers need a country match in `geo.csv`.
- The map's year slider spans **2008–2021**, as set in `scene3.html`. Earlier years have fewer records in the bundled data.

</details>

---

# Architecture

| File | Responsibility |
| --- | --- |
| `text.html` | Intro page and local video |
| `scene1.html` + `resources/js/scene1.js` | Timeline visualization |
| `resources/js/piechart.js` | Movie / TV Show pie chart on Scene 1 |
| `scene2.html` + `resources/js/scene2.js` | Genre ranking charts and type toggle |
| `scene3.html` + `resources/js/map.js` | Leaflet map, country totals, year and genre filters |
| `resources/js/utils.js` | Cleaned CSV path, coordinate CSV path, and data helpers |
| `resources/js/navigation.js` | Navigation behavior between scenes |
| `resources/css/` | Styles for the introduction, charts, and map |

<details>
<summary><strong>Data files and dependencies</strong></summary>

| Path / library | Purpose |
| --- | --- |
| `resources/data/netflix_titles.csv` | Main title data for the charts; **8,790** data rows in the supplied archive |
| `resources/data/netflix_clean.csv` | Cleaned title data for the map; **7,787** data rows in the supplied archive |
| `resources/data/geo.csv` | Country coordinates; **245** data rows in the supplied archive |
| D3.js | CSV loading, aggregation, SVG charts, and interaction |
| Leaflet | Interactive map and markers |
| Bootstrap, jQuery, Font Awesome, Splide | Frontend resources referenced from CDNs on some pages |

The CSV row counts above were measured from the supplied files. The repository contains static HTML, CSS, JavaScript, image/video assets, and CSV files; it has no package manifest or build configuration.

</details>

---

# Project Files

| Path | Contents |
| --- | --- |
| `text.html` | Optional introductory page |
| `scene1.html`, `scene2.html`, `scene3.html` | Three visualization pages |
| `resources/data/` | Title records, cleaned records, and country coordinates |
| `resources/js/` | Chart, map, navigation, and helper code |
| `resources/css/` | Page styling |
| `resources/image/` | Intro video and logo image |

---

# Known Issues

**The old README describes Scene 2 as countries and Scene 3 as age ratings.** The supplied HTML and JavaScript implement genre charts in Scene 2 and a country map in Scene 3, as documented above.

<details>
<summary><strong>View source-review findings</strong></summary>

| Area | Finding |
| --- | --- |
| Terminology | Some page text calls title additions or listed countries “production”; the data does not establish production volume. |
| Local files | Loading CSV through `file://` can be blocked by the browser; use a local HTTP server. |
| External assets | CDN libraries and external map tiles require network access. |
| Map tiles | One configured OpenStreetMap layer uses `http://`, which may be blocked as mixed content on an HTTPS deployment. |
| D3 versions | Scene 1 includes more than one D3 v7 script; Scene 3 loads D3 v5, while the other scenes load v7. |
| Data coverage | The cleaned map dataset has fewer rows than the chart dataset; country coordinates may not cover every listed name. |
| Testing | No automated tests or build checks are included in the archive. |

</details>

---

# Roadmap

- [ ] Align page descriptions with the actual chart definitions.
- [ ] Standardize D3 versions and remove unused library includes.
- [ ] Use HTTPS tile URLs and verify hosted map behavior.
- [ ] Document dataset provenance, cleaning steps, and field limitations.
- [ ] Add a reproducible smoke check for each scene.

---

# Contributing

Open an issue or submit a focused pull request. Include the affected scene, browser version, reproduction steps, and a screenshot or console error when reporting a display problem.

# License and Data Attribution

No `LICENSE` file or explicit source license is included in the supplied archive. Before redistributing the code, dataset, video, or logo, confirm their respective origins and permissions. The coordinate helper in `utils.js` links to a Google country-coordinate sample as attribution for that resource.

---

<p align="center"><a href="#netflix-content-visualization">Back to top ↑</a></p>
