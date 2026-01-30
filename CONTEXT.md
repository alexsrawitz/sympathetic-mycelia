# Sympathetic Mycelia — Context for Continuation

## What This Is

A generative art piece built with p5.js. Single self-contained HTML file that creates flowing, organic tendrils inspired by mycelium networks. The user's emotional state (via text input) shapes every aspect of the art — colors, patterns, and energy.

**File location:** `/Users/alexrawitz/sympathetic-mycelia.html`

**Linear tracking:** Project "Algorithmic Art" under Northwoods Products, Issue NOR-55 (with subtask NOR-115 for future Claude API fallback)

---

## Core Concept

Users answer "How are you feeling?" and their words **deterministically generate** the art:
- Text hash → seed (same text = same random seed)
- Text hash → palette selection (from 32 curated palettes)
- Sentiment keywords → resonance parameters (calm → slow, anxious → chaotic, connected → dense, etc.)
- 50 character minimum ensures enough content to analyze
- The user never sees the technical details — just types and clicks Flow

---

## User Flow

1. **Page loads** → Neutral dark background, Flow button disabled
2. **Type feeling** → "I'm feeling peaceful and connected..." (50 char minimum)
3. **Character counter** → Shows progress toward minimum, turns green when ready
4. **Click Flow** → Hashes text → derives seed + palette → analyzes sentiment → spawns tendrils
5. **Watch** → Tendrils grow, connect, pulse with the energy of your words
6. **Reset All** → Clears everything, re-enables input
7. **Download** → Saves PNG

---

## Technical Architecture

### Palettes (32 total)
Each palette has: `name`, `labels` (3 color names), `colors` (3 hex values)
Background hue derived dynamically from palette colors via weighted average.

Categories include: Forest/botanical, autumn/harvest, desert/mesa, twilight/moonlit, ocean/tidal, dried/weathered, berry/vineyard, and more.

### Text → Art Pipeline
```
feeling text (50+ chars)
    ↓
hashString(text) → numeric hash
    ↓
├── hash → params.seed (Perlin noise seed)
├── hash % 32 → palette selection
└── analyzeSentiment(text) → 8 mood scores
        ↓
    resonance parameters (tendrilCount, growthRate, etc.)
```

### Sentiment Analysis (8 dimensions, ~350 words)
| Dimension | Range | Affects |
|-----------|-------|---------|
| `energy` | calm ↔ energetic | Growth rate, vibration |
| `anxiety` | 0 ↔ high | Adds chaos to energy |
| `connection` | isolated ↔ connected | Connection strength, density |
| `density` | sparse ↔ abundant | Tendril count |
| `clarity` | turbulent ↔ clear | Noise scale (field smoothness) |
| `hope` | despairing ↔ hopeful | Brightness bias |
| `temporality` | nostalgic ↔ present | Color warmth |
| `playfulness` | serious ↔ playful | Vibration frequency modulation |

### Key Parameters
| Param | What it controls |
|-------|-----------------|
| `tendrilCount` | Number of tendrils (1500-5000) |
| `growthRate` | Speed of tendril growth |
| `vibrationFreq` | Global heartbeat / color pulse speed |
| `connectionStrength` | How far apart tendrils can resonate |
| `noiseScale` | Perlin noise scale (low=sweeping, high=chaotic) |

### Tendrils (class `Tendril`)
- Spawned from grid covering canvas + 150px overflow on all sides
- Follow Perlin noise flow field
- Each has internal `phase` for vibration/resonance
- Connections form between tendrils with compatible phases
- Colors pulse with global heartbeat

---

## Current State

✅ Working features:
- Text-driven generation (feeling text is the ONLY input)
- 50 character minimum with live counter
- Deterministic: same text → same art every time
- 32 curated warm/natural palettes
- 8-dimension sentiment analysis (~350 keywords)
- Palette-matched backgrounds
- One-shot Flow generation
- Reset All functionality
- PNG download
- Retina display support
- Smooth anti-aliased curves

---

## Code Structure (key functions)

```
setup()                  - Initialize canvas, neutral background, disable Flow
updateCharCounter()      - Track input length, enable/disable Flow button
flow()                   - Hash text → seed/palette, analyze sentiment, spawn tendrils
applyPalette()           - Set colors from PALETTES array
drawNebulaBackground()   - Simple dark background tinted with palette hue
draw()                   - Main loop (only runs after Flow)
class Tendril            - Individual tendril with growth, phase, drawing
findConnections()        - Detect resonant tendril pairs
drawConnections()        - Render connection lines and glows
analyzeSentiment(text)   - Extract 8 mood scores from text
hashString(str)          - Deterministic hash for text → seed
resetAll()               - Clear and reset to initial state
```

---

## Where We Left Off

### Just Completed (NOR-108)
- **Feeling text is now the ONLY input** — removed seed controls, palette randomize, color pickers
- **Text hash derives everything**: seed, palette selection
- **50 character minimum** with live character counter (green when ready)
- **Expanded to 32 palettes** (was 14)
- **Expanded word banks to ~350 words** across 8 sentiment dimensions
- **Added 3 new sentiment dimensions**: hope, temporality, playfulness
- **Simplified UI**: just textarea, Flow button, Reset, Download

### Known Issues to Watch For
- **Canvas discoloration at top**: Was caused by draw() loop running before Flow. Fixed with `if (!hasFlowed) return;` but watch for regression

### Design Philosophy Established
- **One-shot generation**: User types, clicks Flow once, watches. No mid-generation tweaking.
- **Feeling shapes art deterministically**: Same words = same art. No randomness exposed to user.
- **Warm/natural aesthetic**: All palettes are earthy, botanical, warm.

### User's Stated Preferences
- Wants the art to evoke "internal, spiritual vibration" — peaceful AND strong, gentle BUT forceful
- Likes dense canvas coverage (originally asked for <10% blank space)
- Prefers simplified UI — removed features that felt like clutter
- Appreciates determinism: typing the same feeling should produce the same art

---

## Future Ideas (see NOR-115)

- **Claude API fallback**: If no keywords match, send text to Claude for sentiment analysis
- Shareable URLs with feeling text encoded
- Mobile touch support
- Gallery of saved generations
- Audio reactivity
