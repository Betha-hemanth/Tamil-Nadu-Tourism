# Tamil Nadu Tourism — Single-Page Site

A single-file, animated tourism landing page for Tamil Nadu: temples, hill stations, beaches and wildlife, with search, an interactive region map, a filterable attractions gallery, tour packages and a simple trip planner. Built with plain HTML/CSS/JS — no framework, no build step.

**🔗 Live demo:** [betha-hemanth.github.io/Tamil-Nadu-Tourism](https://betha-hemanth.github.io/Tamil-Nadu-Tourism/)

`tamil-nadu-tourism.html` → open it in a browser and it runs.

---

## ✨ Features

| Area | What it does |
|---|---|
| **Hero** | Ken Burns background carousel (4 rotating photos), gradient headline, live destination search |
| **Search** | Type-ahead over all 12 destinations, "popular" suggestions when empty, keyboard nav (↑ ↓ ↵ Esc), friendly empty state |
| **Destinations rail** | Horizontal-scroll cards → opens a full **detail view** (overview, best time, attractions, nearby places, gallery) |
| **Experiences grid** | 6 travel "moods" (Spiritual, Hills, Beaches, Wildlife, Food & Culture, Adventure) |
| **Region explorer** | Clickable/hoverable SVG map of Tamil Nadu's 5 regions + tabs + place lists |
| **Attractions directory** | Filterable photo gallery (Temples, Beaches, Waterfalls, Hill Stations, Wildlife, Forts & Palaces) |
| **Packages rail** | 6 pre-built itineraries with days / price / destination count |
| **Trip planner** | Pick days, travel style, budget and interests → generates a draft itinerary client-side |
| **Polish** | Custom cursor particle trail, page-transition veil, scroll-reveal animations, rotating accent-color theme every 14s, mobile burger menu, floating action button |

All photography is pulled live from **Wikimedia Commons** via `Special:FilePath` — no local image assets required.

---

## 🗺️ Site Map

```mermaid
flowchart TD
    NAV["Sticky Navbar + Search"] --> HERO
    HERO["#hero — Ken Burns carousel + search"] --> DEST
    DEST["#destinations — 12 destination cards"] --> DETAIL["Destination Detail View\n(overview, attractions, gallery, nearby)"]
    HERO --> EXP["#experiences — 6 mood cards"]
    EXP --> REG["#regions — SVG map + 5 regions"]
    REG --> ATTR["#attractions — filterable gallery"]
    ATTR --> PKG["#packages — 6 tour packages"]
    PKG --> PLAN["#planner — day/style/budget/interests → itinerary"]
    PLAN --> FOOT["Footer / #contact"]

    DETAIL -. "Plan Your Trip" .-> PLAN
```

---

## 🧩 Data Model

Everything on the page is driven by five in-memory JS arrays/objects — there's no backend or database.

```mermaid
erDiagram
    DESTINATION {
        string id
        string name
        string district
        string region
        string category
        string desc
        string hero_image
        array  gallery
        string best_time
        array  attractions
        array  nearby
    }
    EXPERIENCE {
        string name
        string desc
        string image
    }
    REGION {
        string name
        array  destinations
    }
    PACKAGE {
        string name
        string category
        int    days
        string price
        int    dest_count
        string image
        string desc
    }

    REGION ||--o{ DESTINATION : contains
    EXPERIENCE ||--o{ DESTINATION : "categorizes (loosely)"
    PACKAGE }o--o{ DESTINATION : bundles
```

---

## 📊 Destination Breakdown

**By region** (5 regions defined; 4 currently populated with destinations)

```mermaid
pie showData
    title Destinations by Region
    "West (Ooty, Kodaikanal, Yercaud, Valparai, Coimbatore)" : 5
    "South (Madurai, Kanyakumari, Rameswaram, Courtallam)" : 4
    "North (Chennai, Mahabalipuram)" : 2
    "Delta (Thanjavur)" : 1
```

**By category**

```mermaid
pie showData
    title Destinations by Category
    "Spiritual & Heritage" : 4
    "Hill Station" : 3
    "Beaches & Coast" : 2
    "Wildlife & Nature" : 2
    "Food & Culture" : 1
```

**Tour package length vs. price** (₹, 6 packages)

```mermaid
xychart-beta
    title "Package Duration vs Price"
    x-axis ["Weekend Hills (3d)", "Nilgiri Escape (4d)", "Coastal Honeymoon (5d)", "Wild Ghats (5d)", "Temple Trails (6d)", "Family Circuit (7d)"]
    y-axis "Price (₹ thousands)" 0 --> 40
    bar [12.4, 18.9, 32.0, 27.3, 24.5, 35.8]
```

---

## 🔄 Key User Flows

**Search → Detail**
```mermaid
sequenceDiagram
    participant U as User
    participant S as Search Box
    participant P as Search Panel
    participant D as Detail View
    U->>S: Focus / type query
    S->>P: renderPopular() or renderResults(query)
    U->>P: Click / Enter on a suggestion
    P->>D: openDetail(id)
    D-->>U: Full-screen destination profile
```

**Trip Planner**
```mermaid
flowchart LR
    A[Select Days] --> D[Generate Itinerary]
    B[Select Travel Style] --> D
    C[Select Budget] --> D
    E[Toggle Interests] --> D
    D --> F[Draft itinerary rendered in #itinerary-out]
```

---

## 🎨 Design System

- **Fonts:** Fraunces (serif, headings) + Manrope (sans, body)
- **Palette:** warm ivory background, deep maroon `#8a1538` → terracotta `#c17a3d` accent gradient — auto-rotates between 3 accent pairs every 14 seconds
- **Motion:** all animation respects `prefers-reduced-motion`
- **Radii:** 12 / 18 / 26px scale for small / medium / large surfaces

```mermaid
flowchart LR
    subgraph Palette
    direction LR
    bg["Background\n#ffffff / #faf8f5"] --- ink["Ink\n#2b2118"]
    ink --- accent["Accent\n#8a1538"]
    accent --- accent2["Accent 2\n#c17a3d"]
    end
```

---

## 🛠️ Tech Stack

- Vanilla **HTML5 / CSS3 / JavaScript** (IIFE, no modules, no build tooling)
- **Google Fonts** (Fraunces, Manrope)
- **Wikimedia Commons** `Special:FilePath` for all imagery (dynamically sized via `?width=`)
- Native **SVG** for the region map and loader mark
- `IntersectionObserver` for scroll-reveal, `Canvas 2D` for the cursor particle trail

No dependencies to install — it's a single static file.

---

## 🚀 Running It

**Try it live:** https://betha-hemanth.github.io/Tamil-Nadu-Tourism/

```bash
# just open it
open tamil-nadu-tourism.html

# or serve it locally (recommended, avoids any file:// quirks)
python3 -m http.server 8000
# then visit http://localhost:8000/tamil-nadu-tourism.html
```

---

## ✏️ Customizing

| To change... | Edit... |
|---|---|
| Add/edit a destination | `destinations` array (search `const destinations = [`) |
| Add/edit a tour package | `packages` array |
| Add/edit an experience card | `experiences` array |
| Region → place mapping | `regions` object |
| Accent color themes | `themes` array (`applyTheme()`) |
| Hero carousel photos | `heroSeeds` array |
| Nav / footer links | corresponding `<nav>` / `<footer>` markup near the top of `<body>` |

---

## 📄 Credits

Destination photography courtesy of [Wikimedia Commons](https://commons.wikimedia.org/) contributors, fetched live via `Special:FilePath`.

This is a conceptual redesign / demo project — not an official Tamil Nadu Tourism property.
