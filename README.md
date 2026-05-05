# New Image Landscaping — React Site

A modern, single-page React website for New Image Landscaping, built with Vite. Features interactive, state-driven tab components in two places:

- **Services section** — click any service to see its full description and feature list. Active state managed via `useState`.
- **Portfolio section** — filter projects by category (All / Hardscape / Gardens / Lawns / Lighting). Filtered list memoized with `useMemo`.

## Prerequisites

- **Node.js 18 or newer** (check with `node -v`)
- **npm** (comes with Node)

## How to run

From the project folder, in your terminal:

```bash
# 1. Install dependencies
npm install

# 2. Start the dev server (opens automatically at http://localhost:5173)
npm run dev
```

That's it. The site will hot-reload as you edit files.

## Other commands

```bash
# Build for production (output goes to /dist)
npm run build

# Preview the production build locally
npm run preview
```

## Project structure

```
new-image-landscaping/
├── index.html              # Vite entry HTML
├── package.json
├── vite.config.js
└── src/
    ├── main.jsx            # React entry point
    ├── App.jsx             # Top-level page composition
    ├── styles.css          # All styles (CSS variables + responsive)
    ├── data.jsx            # Services & projects data — edit here
    └── components/
        ├── Nav.jsx           # Sticky nav with scroll detection
        ├── ServicesTabs.jsx  # Interactive tabbed services (useState)
        └── Portfolio.jsx     # Filterable portfolio (useState + useMemo)
```

## How the tabs work

### Services tabs (`ServicesTabs.jsx`)

```jsx
const [activeId, setActiveId] = useState(SERVICES[0].id);
const active = SERVICES.find((s) => s.id === activeId);
```

Clicking a tab button calls `setActiveId(s.id)`, which triggers a re-render. The right-hand panel reads from `active` and re-mounts on each change (`key={active.id}`) so the fade-in animation re-runs.

ARIA attributes (`role="tablist"`, `role="tab"`, `aria-selected`, `aria-controls`) are set for accessibility.

### Portfolio filters (`Portfolio.jsx`)

```jsx
const [filter, setFilter] = useState('all');
const visible = useMemo(
  () => (filter === 'all' ? PROJECTS : PROJECTS.filter((p) => p.category === filter)),
  [filter]
);
```

Each filter button updates `filter`, and `visible` is recomputed only when `filter` changes.

## Customizing content

All editable content lives in **`src/data.jsx`**:

- `SERVICES` — name, headline, body copy, and feature list for each service tab
- `PROJECTS` — title, category, location, and image for each portfolio item
- `PROJECT_FILTERS` — the filter button labels

The portfolio currently uses inline-SVG placeholder images. To swap in real photos:

1. Drop your images into a folder like `src/assets/`
2. In `data.jsx`, replace each project's `image:` SVG with `image: <img src={photoUrl} alt="..." />`

## Customizing the look

All colors, fonts, and spacing tokens are CSS variables defined at the top of `src/styles.css`:

```css
:root {
  --forest: #1f3a2e;
  --moss: #4a6b48;
  --sage: #8ea88a;
  --cream: #f4ede0;
  --rust: #b65a3c;
  /* ... */
}
```

Change those once and the whole site updates.

## Deploying

Run `npm run build`, then upload the contents of the `/dist` folder to any static host (Netlify, Vercel, Cloudflare Pages, GitHub Pages, etc.).
