# Implementation Plan: Interactive GIS Analyst Website & Portfolio

Build a state-of-the-art 2-in-1 website and interactive portfolio tailored for a GIS Analyst. The application will feature a high-end **Geo-Tech Dark Mode** theme, live **Leaflet.js WebGIS map engine** with layer controls and spatial attribute inspection, 5 realistic sample GIS case studies with detailed drawer views and before/after swipe controls, skill matrix, interactive storymap timeline, and full desktop/mobile responsiveness.

---

## User Review Required

> [!IMPORTANT]
> **Selected Preferences**:
> - **Theme**: Geo-Tech Dark Mode (Obsidian `#0B0F19` base, Emerald `#10B981` & Cyan `#06B6D4` spatial glow accents).
> - **Map Engine**: Leaflet.js WebGIS with interactive layer toggles (Urban Heat, Flood Inundation, Transit Isochrones, LULC Classification, Sensors), basemap switcher (Dark Matter, Satellite, Topo), attribute inspection, legend, and popups.
> - **Content**: 5 pre-populated GIS projects with easy JSON configuration in `src/data/gisData.js` for quick user editing.

---

## Proposed Project Location

Project directory to be created:
`C:\Users\rohma\.gemini\antigravity\scratch\gis-portfolio`

*Recommendation*: Set `C:\Users\rohma\.gemini\antigravity\scratch\gis-portfolio` as your active workspace in your editor.

---

## Proposed Changes

### Component Architecture & Structure

```
gis-portfolio/
├── package.json
├── index.html
├── vite.config.js
├── src/
│   ├── main.jsx
│   ├── index.css                    # Design system tokens, obsidian dark theme, glassmorphism, map styles
│   ├── data/
│   │   └── gisData.js               # Structured JSON for personal info, projects, layers, skills, timeline
│   ├── components/
│   │   ├── Navbar.jsx               # Header with logo, nav links, status badge, mobile drawer
│   │   ├── Hero.jsx                 # Animated spatial grid canvas background, stats counters, CTA
│   │   ├── MapShowcase.jsx          # Live Leaflet WebGIS viewer, layer toggles, basemap switcher, legend, attribute panel
│   │   ├── ProjectsSection.jsx      # Project gallery with category filters & project cards
│   │   ├── ProjectModal.jsx         # Case study detail modal with spatial methodology, tech tags, before/after slider
│   │   ├── SkillMatrix.jsx          # Categorized GIS toolstack (ArcGIS, QGIS, Python, PostGIS, GEE, WebGIS)
│   │   ├── StorymapTimeline.jsx     # Spatial journey & career timeline
│   │   ├── ContactSection.jsx       # Interactive GIS inquiry form & social links
│   │   └── Footer.jsx               # Footer with coordinate display, copyright, quick links
│   └── utils/
│       └── generateSampleMaps.js    # Canvas/SVG map graphic generators for high-res project imagery
```

---

## Detailed Component Specifications

### 1. [NEW] `package.json` & Project Setup
- Framework: Vite + React + Lucide Icons (`lucide-react`) + Leaflet (`leaflet`, `react-leaflet`) + Canvas-confetti + Tailwind-compatible styling/Vanilla CSS variables.
- Fast local development server execution.

### 2. [NEW] `src/index.css` & Design System
- Obsidian dark palette (`#0B0F19`, `#111827`), glowing neon emerald (`#10B981`), electric cyan (`#06B6D4`), soft gray typography (`#94A3B8`).
- Custom map container styling, glowing spatial markers, glassmorphism modal overlays, custom scrollbars, vector grid animations.

### 3. [NEW] `src/data/gisData.js`
- Complete customizable dataset containing:
  - Personal Bio & GIS Analyst Profile
  - 5 GIS Case Studies:
    1. *Urban Heat Island & Microclimate Risk Analysis* (Remote Sensing & Landsat 9 / QGIS)
    2. *Coastal Flood Inundation & Sea Level Rise Modeling* (Spatial Analysis & HEC-RAS / ArcGIS Pro)
    3. *Public Transit Isochrone & Spatial Accessibility Dashboard* (Web GIS & OpenStreetMap / PostGIS)
    4. *Land Use / Land Cover (LULC) Sentinel-2 Deep Learning Classification* (GeoAI & Python PyTorch)
    5. *Wildfire Hazard & Vegetation Density Mapping* (Google Earth Engine & Cartographic Layout)
  - Skill Matrix breakdown (Software, Databases, Languages, Web Mapping)
  - Map Layers with GeoJSON features and attribute data for Leaflet showcase

### 4. [NEW] `src/components/MapShowcase.jsx`
- Fully interactive Leaflet map featuring:
  - Basemap Options: Dark Matter (CartoDB), Satellite Hybrid (Esri/OSM), Topographic Terrain.
  - Interactive Layers:
    - **Urban Heat Risk**: Heatmap & Polygon choropleth.
    - **Flood Inundation**: Hydrological vulnerability zones.
    - **Transit Accessibility**: Isochrone rings & station markers.
    - **Sensors / Weather Stations**: Clickable markers with real-time popup data.
  - Attribute Inspection Panel: Interactive side drawer that populates live spatial stats when clicking map features.

### 5. [NEW] `src/components/ProjectModal.jsx`
- Case study detail drawer featuring:
  - Methodology workflow badges.
  - Interactive Before/After image comparison slider (e.g. 2016 vs 2026 LULC or Pre-Flood vs Post-Flood).
  - Key spatial insights, raster resolution details, CRS (Coordinate Reference System), tools used.

### 6. [NEW] Responsive Design Verification
- Tested for desktop (1920px+), laptop (1440px/1280px), tablet (768px), and mobile (375px/414px).

---

## Verification Plan

### Automated Build Verification
- Execute `npm run build` in `C:\Users\rohma\.gemini\antigravity\scratch\gis-portfolio` to ensure 0 lint or compilation errors.

### Dev Server Launch & Testing
- Start `npm run dev` local server and verify:
  - Leaflet map loads correctly without tile missing errors.
  - All 5 project modals open smoothly.
  - Category filters work seamlessly.
  - Mobile drawer menu opens and functions cleanly on small viewports.
  - Contact form feedback state functions as expected.
