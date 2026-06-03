# Coffee & Roast Co. — EU Intelligence Hub

A single-file, self-contained intelligence tool for Coffee & Roast Co.'s expansion into European and Middle East markets. Built with live economic data, AI-generated market narratives, and real-time café density maps — no logins, no spreadsheets, no waiting.

**[Live Demo](https://yourusername.github.io/coffee-roast-hub/)** · [About](#about) · [Features](#features) · [Data Sources](#data-sources) · [How to Use](#how-to-use)

---

## About

This hub was built to compress 12–18 months of market research into weeks of iterative intelligence. It combines:

- **Live public data** from three authoritative sources (World Bank, Eurostat, OpenStreetMap)
- **AI-generated market analysis** via Claude (Anthropic), with JavaScript fallbacks for zero-failure features
- **Interactive tools** for weighted city ranking, customer persona building, and multilingual content localization
- **Brand narrative** that positions Coffee & Roast Co.'s Turkish heritage and third-wave positioning against the European specialty coffee moment

Perfect for:
- 15-minute brand manager demos and pitch support
- Scenario planning and market entry prioritization
- Foundation for a full market intelligence platform

---

## Features

### 1. **Market Intelligence** 🗺️
Four city scorecards (Amsterdam, Budapest, Berlin, Dubai) ranked by Brand Fit, Market Size, and Competition. Click any city to load an AI-generated strategic narrative.

- **Data:** World Bank Open Data API (live GDP per capita, household consumption expenditure)
- **Analysis:** AI-powered market context and competitive positioning for each city

### 2. **City Entry Recommender** 🎯
Weighted scoring model. Set your expansion priorities with three sliders, pick an expansion mode (brand-first, B2B wholesale, DTC), and see the ranking recalculate in real time.

- **Market Data:** Eurostat household food & beverage spend, urbanization rates
- **Map:** Interactive café density visualization from OpenStreetMap via Leaflet + MarkerCluster. Zoom in to individual venues; zoom out to see density by district.

### 3. **Customer AI & Product Recommender** ☕
Answer 4 questions → AI builds a named persona + editorial customer story → recommends the best Coffee & Roast Co. product from the catalog.

- **Fallback:** If the API is unavailable, a JavaScript fallback generates one of 10 curated archetypes — this feature never fails
- **Product Matching:** Tag-based scoring across a 10-item catalog (Ethiopian Yirgacheffe, Rwandan Bourbon, Yemeni Mocha, Turkish Heritage Dark, etc.)
- **Scalable:** Ready to plug into real product catalogs and run at point-of-sale kiosks, websites, or events

### 4. **Content Localizer** 🌍
Paste any Coffee & Roast Co. marketing copy. Get culturally adapted outputs for Dutch, Hungarian, German, and Arabic markets — in seconds.

- **Not translation — cultural adaptation.** Preserves the emotional intent behind your copy while rewriting idioms, register, and cultural reference points for each market.
- **Four content formats:** Instagram Story, Instagram Post, Promotional Message, Brand Statement — each with dedicated AI prompt guidance
- **Voice rules:** Explicit per-market cultural guidelines baked into the prompt (Dutch: direct; Hungarian: warm/poetic; German: quality-focused; Arabic: storytelling-led)

---

## Data Sources

| Source | What | Why | Link |
|--------|------|-----|------|
| **World Bank Open Data API** | GDP per capita, household final consumption expenditure | Most reliable public proxy for premium purchasing power in target markets | [data.worldbank.org](https://data.worldbank.org) |
| **Eurostat** | Household food & beverage spending, urbanization rates, demographics | European Statistical Office; representative, cited data for EU market context | [ec.europa.eu/eurostat](https://ec.europa.eu/eurostat) |
| **OpenStreetMap / Overpass API** | Real café venue counts and locations across target cities | Community-maintained, real-time competitive landscape data | [openstreetmap.org](https://www.openstreetmap.org) |

All data is referenced within the tool.

---

## How to Use

### Online (Recommended)
Open the live demo: **[https://yourusername.github.io/coffee-roast-hub/](https://yourusername.github.io/coffee-roast-hub/)**

No installation needed. Works in any modern browser. All features function with or without an API connection (JavaScript fallbacks ensure zero failures).

### Locally
1. Clone or download this repository
2. Open `index.html` in your browser
3. To use AI features (city narratives, persona builder, content localizer), configure your API endpoint:
   - Edit the `const API_BASE = '/api/anthropic'` line in `index.html`
   - Point it to a backend that proxies requests to Claude (or remove this line to use fallbacks)

### Customization

#### Connect Your Own Product Catalog
Find the `productCatalog` array in the script (around line 2000). Replace the 10-item dummy catalog with your real SKUs, origins, flavors, and tags. The persona builder will instantly match customers to your products.

#### Adjust City Scores
Find the `marketData` object. Base scores for Brand Fit, Market Size, and Competition are editable — adjust them based on your latest market research.

#### Change Target Markets
Rename or add cities in `marketData` and `coffeeVenueData`. Update the grid layout accordingly.

#### Localize for Different Markets
The `fallbacks` object (around line 2100) contains pre-built cultural adaptations for Dutch, Hungarian, German, and Arabic. Extend it with new languages or content types.

---

## Technical Details

### Architecture
- **Single HTML file:** No build step, no npm, no framework dependencies
- **CSS custom properties:** Design system defined in `:root`
- **Vanilla JavaScript:** No jQuery or heavy libraries
- **Lazy-loaded Leaflet:** Map only initializes when the user clicks the recommender tab (avoids hidden-container rendering bugs)
- **Fallback-first design:** Every AI feature has a JS-based fallback, so the tool works perfectly even without an API

### API Integration
The tool expects a backend proxy at `/api/anthropic` that:
- Accepts POST requests with a `prompt` body
- Forwards to the Claude API (Anthropic)
- Returns the full text response

Example Node.js proxy:
```javascript
app.post('/api/anthropic', async (req, res) => {
  const { prompt } = req.body;
  const message = await client.messages.create({
    model: 'claude-opus-4-1',
    max_tokens: 1024,
    messages: [{ role: 'user', content: prompt }]
  });
  res.json({ response: message.content[0].text });
});
