# Sympathetic Mycelia

Text-driven generative art. Your feelings become flowing tendrils.

## Stack

- **Frontend:** Vanilla HTML/CSS/JS (single file)
- **Graphics:** p5.js (CDN)
- **Hosting:** Vercel (static)
- **Repo:** https://github.com/alexsrawitz/sympathetic-mycelia

## Run Locally

```bash
open index.html
# or
python -m http.server 8000  # then visit localhost:8000
```

## Deploy

Push to `main` → auto-deploys to https://sympathetic-mycelia.vercel.app

## Architecture

Single self-contained HTML file with inline CSS and JS. No build step.

```
index.html
├── CSS (inline <style>)
├── HTML (sidebar controls + canvas container)
└── JS (inline <script>)
    ├── PALETTES[] — 32 curated color palettes
    ├── MOOD_WORDS{} — ~350 sentiment keywords across 8 dimensions
    ├── setup() — initialize canvas
    ├── flow() — hash text → seed/palette, analyze sentiment, spawn tendrils
    ├── draw() — main animation loop
    ├── class Tendril — individual tendril with growth, phase, drawing
    ├── findConnections() / drawConnections() — resonance between tendrils
    └── analyzeSentiment() — extract mood scores from text
```

## How It Works

1. User types how they're feeling (50 char minimum)
2. Text is hashed → becomes random seed + selects palette
3. Text is analyzed → 8 sentiment dimensions extracted
4. Sentiment scores → art parameters (density, speed, chaos, etc.)
5. Tendrils spawn and grow following Perlin noise flow field
6. Same text = same art (deterministic)

## Linear

- **Project:** Algorithmic Art
- **Parent issue:** NOR-55 (Sympathetic Mycelia)
- **Subtasks:** NOR-108 (sentiment expansion, done), NOR-115 (Claude API fallback, backlog), NOR-116 (hosting, done)

## Status

✅ Live at https://sympathetic-mycelia.vercel.app

Current features:
- 32 palettes, ~350 sentiment keywords, 8 mood dimensions
- Deterministic generation (same text = same art)
- One-shot flow, reset, PNG download
