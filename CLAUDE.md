# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A static GitHub Pages site — a personal project index for **2357 Studios**. A central JSON manifest (`projects.json`) drives a filterable, sortable card grid on `index.html`. Individual projects live as self-contained single-file HTML pages in `projects/`.

No build step, no framework, no dependencies. Everything is plain HTML/CSS/JS.

## Adding a New Project

1. Drop the `.html` file into `projects/`.
2. Add an entry to `projects.json` (see schema below).
3. Apply the standard back-nav snippet to the new file (see below).
4. Set the page `<title>` to `"Project Name | 2357 Studios"`.

## projects.json Schema

```json
{
  "name": "Display Title",
  "description": "One sentence about what it does.",
  "category": "utility",
  "tags": ["optional", "keywords"],
  "created": "YYYY-MM-DD",
  "updated": "YYYY-MM-DD",
  "url": "projects/my-tool.html"
}
```

**Categories:** `utility` · `game` · `work-tool` · `fitness`

Update the `updated` field on any session where a project file is meaningfully changed.

## Standard Back-Nav (required on every project file)

Every file in `projects/` must include this CSS block and HTML immediately after `<body>`. It renders a fixed "← Projects" badge (top-left) and a "2357 Studios" credit (bottom-right).

```html
  <style>
    .home-nav { position: fixed; top: 0; left: 0; z-index: 9999; padding: 6px 14px;
      background: rgba(0,0,0,0.55); backdrop-filter: blur(6px);
      -webkit-backdrop-filter: blur(6px); border-bottom-right-radius: 8px; }
    .home-nav a { color: rgba(255,255,255,0.75); text-decoration: none;
      font-family: monospace; font-size: 0.8rem; letter-spacing: 0.06em; }
    .home-nav a:hover { color: #fff; }
    .studios-credit { position: fixed; bottom: 0; right: 0; z-index: 9999; padding: 5px 12px;
      background: rgba(0,0,0,0.55); backdrop-filter: blur(6px);
      -webkit-backdrop-filter: blur(6px); border-top-left-radius: 8px;
      font-family: monospace; font-size: 0.7rem; color: rgba(255,255,255,0.4);
      letter-spacing: 0.05em; pointer-events: none; }
  </style>
</head>
<body>
  <nav class="home-nav"><a href="../index.html">&#8592; Projects</a></nav>
  <div class="studios-credit">2357 Studios</div>
```

## index.html Architecture

- Fetches `projects.json` at load time; fails gracefully with an error message if the fetch fails (only works over HTTP, not `file://`).
- Cards are rendered as `<a class="card">` elements so the entire card is clickable.
- Filter (search + category) and sort (field + direction) are applied in-memory on every input event — no server round-trips.
- The category dropdown is populated dynamically from the values present in `projects.json`.
- The `updated` field is used by the "Last Updated" sort option; falls back to `created` if absent.

## Design Conventions

- Dark background (`#0f0f0f` or similar), light text, accent gold (`#f0c040` or similar).
- Font stack: typically `DM Mono` or similar monospace for body; `Bebas Neue` for display/headings.
- All projects are fully self-contained — no shared CSS files, no external JS libraries.
- `z-index: 9999` is reserved for the back-nav and studios-credit overlays.

## Hosting

GitHub Pages, served from the `main` branch root. Pushing to `main` deploys automatically.
