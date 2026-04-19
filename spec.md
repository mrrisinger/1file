# Project Listing Homepage — Spec

## Overview

A static GitHub Pages site that serves as a personal project index. A central JSON manifest drives a filterable, sortable card-based listing of HTML projects.

---

## Repository Structure

```
/
├── index.html
├── projects.json
└── projects/
    ├── project-one.html
    ├── project-two.html
    └── ...
```

---

## Data: `projects.json`

A JSON array at the repo root. Each project is an object with the following fields:

| Field | Type | Required | Notes |
|---|---|---|---|
| `name` | string | yes | Display title |
| `description` | string | yes | One-sentence summary |
| `category` | string | yes | Single value: `utility`, `fitness`, `game`, `work-tool` |
| `tags` | string[] | no | Additional keywords for filtering |
| `created` | string | yes | ISO 8601 date: `YYYY-MM-DD` |
| `updated` | string | no | ISO 8601 date: `YYYY-MM-DD` |
| `url` | string | yes | Relative path: `projects/my-tool.html` |

### Example

```json
[
  {
    "name": "Copay Calculator",
    "description": "HUD SMI-based copay calculator for CITC staff.",
    "category": "work-tool",
    "tags": ["HUD", "CITC"],
    "created": "2024-01-15",
    "updated": "2024-03-10",
    "url": "projects/copay-calc.html"
  }
]
```

---

## Homepage: `index.html`

### Layout

- Single-page, no frameworks, no build step — plain HTML/CSS/JS
- Responsive, mobile-friendly
- Minimal/clean aesthetic: neutral background, generous whitespace, monospace or sans-serif type

### Sections

1. **Header** — Site title (e.g. "Projects") and a brief tagline
2. **Controls** — Filter and sort bar
3. **Project Grid** — Card listing, dynamically rendered from JSON

### Controls

**Filter**
- Category dropdown (populated dynamically from manifest values)
- Tag pills or a text search input (filters on `name`, `description`, `tags`)

**Sort**
- Sort by: `Name (A–Z)`, `Date Created`, `Last Updated`
- Sort direction toggle: ascending / descending

### Project Cards

Each card displays:
- `name` (linked to `url`, opens in same tab)
- `category` (badge/label)
- `description`
- `tags` (if present)
- `created` date (formatted: `Jan 15, 2024`)

Cards reflow as filter/sort state changes. No page reload.

---

## Behavior

- On load: fetch `projects.json`, render all cards sorted by `created` descending
- Filter and sort are applied client-side in memory — no server needed
- If no projects match filters, display a "No projects found" message
- Graceful fallback if `projects.json` fails to load

---

## Hosting

- GitHub Pages, served from the `main` branch root (or `/docs` folder if preferred)
- No build tool required — pure static files
- New projects added by: updating `projects.json` + dropping the `.html` file into `/projects/`

---

## Out of Scope

- Authentication
- Project status / WIP flags
- Search across project file contents
- Pagination (not needed at current scale)
