# Fibonacci Fourier

**A seed-based generative system for harmonic line compositions.**

A catalogue of computational textile compositions for fashion, textile and surface design — algorithmically drawn, seed-documented, and ready for production.

---

## Overview

Fibonacci Fourier is a generative design system rather than a single artwork. Each composition is built from a vertical line field — dozens to hundreds of tall strokes, each one whose height is the sum of a small set of sine waves whose frequencies are drawn from the Fibonacci sequence. Over time, the sines drift, and the line field ripples.

The system is designed for:

- **Fashion houses** adapting harmonic ornament for apparel and accessories
- **Textile studios** developing repeat patterns and yardage
- **Surface designers** working across print, wallpaper, and interior applications

Every composition can be licensed, adapted, or commissioned to a brief.

---

## Concept

A line, when its height is *summed from Fibonacci numbers*, becomes a harmonic — mathematical, precise, quietly yours.

The harmonic series — layered frequencies, summed into a single wave — has always carried the structure of ornament. From the Fourier decomposition of a musical note to the harmonic ratios of a Persian garden, the sum-of-sines is one of the oldest systems of mathematical beauty we have. Fibonacci Fourier translates that idea into code. Each composition begins with a colour palette and a set of Fibonacci frequencies, and unfolds through a column of vertical lines whose heights ripple and drift.

The palette, the line count, the number of sine functions, the orientation, and the range of motion are all derived from a single numeric seed.

Unlike most of the still volumes in this series, **Fibonacci Fourier's plate is animated.** The lines need time to ripple; the whole point of the composition is the slow motion of the summed sines. But the framed plate, the surfaces, the archive thumbnails, and the cover are all rendered as single static frames — the frame you would print.

---

## Features

- **Seed-based generation** — every composition is defined by a numeric seed and can be regenerated exactly
- **Deterministic output** — the same seed always produces the same composition at any given frame
- **Fibonacci frequencies** — each sine wave uses a frequency drawn from the Fibonacci sequence
- **Harmonic sum** — each line's height is the sum of up to 25 sine waves, producing a complex harmonic curve
- **Live animated plate** — the lines ripple continuously as the sines advance
- **Static catalogue** — the framed plate, surfaces, and archive thumbnails are frozen single frames
- **Two orientations** — the whole composition can be vertical or horizontal, based on the seed
- **HSL palette** — a base hue jittered by ±30°, producing a coherent colour family
- **Adaptive line widths** — each line's stroke width drifts within a seed-defined range
- **Adaptive surfaces** — one seed applied across print, scarf, textile, and wall formats
- **Archive** — eight curated seeds available for immediate loading
- **Download** — export the current frame as a high-resolution PNG
- **Keyboard shortcuts** — `R` for new seed, `S` to save

---

## Project Structure

```
.
├── index.html          # Main catalogue page
├── images/
│   ├── fav.svg         # Favicon
│   ├── tote.png        # Mockup: tote bag
│   ├── tee.png         # Mockup: t-shirt
│   └── cushion.png     # Mockup: cushion
└── README.md
```

---

## How It Works

### The Seed

A numeric seed (a large integer) initializes a deterministic pseudo-random generator. From this seed, the system derives:

- Background colour (from a palette of 120 deep and light tones)
- Number of vertical lines (50 to 400)
- Number of sine functions (1 to 25)
- The Fibonacci frequency for each sine function
- Base hue for the line palette
- Line width bounds (min and max)
- Orientation (vertical or horizontal)
- Range of motion (how far the lines can move)

Because the generator is deterministic, the same seed always produces the same composition — on any device, at any time.

### The Fibonacci Frequencies

Each composition is driven by a set of **Fibonacci frequencies** — values drawn from the sequence:

```
0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89,
144, 233, 377, 610, 987, 1597, 2584, 4181, 6765
```

Each frequency is used as the angular frequency of one sine wave. Because the Fibonacci numbers grow rapidly, the frequencies cluster at the low end and spread widely at the high end — producing a mixture of slow, large movements and fast, small wobbles.

### The Harmonic Sum

For each vertical line, the system computes:

```
sumOfSines = Σ sin(f[i] · x · 2π / size + t)
```

where `f[i]` is the i-th Fibonacci frequency, `x` is the horizontal position of the line, and `t` is the current time. This sum is then normalized and used to compute the line's start and end y-coordinates:

```
startY = size/2 + range · sumOfSines / n - lineWidth
endY   = size/2 + range · sumOfSines / n + lineWidth
```

The line is drawn from `startY` to `endY`, centered around `size/2`. Because `sumOfSines` changes as `t` advances, the whole line field ripples over time.

### The Colours

Each line has its own colour, drawn from a palette derived from a single base hue:

| Property    | Range                          |
|-------------|--------------------------------|
| Hue         | base ± 30°                     |
| Saturation  | 80% – 100%                     |
| Lightness   | 50% – 70% (with drift)         |

Every frame, the lightness drifts by ±5%, so the palette slowly cycles within its band. The result is a shimmering colour field, coherent but always shifting.

### The Line Widths

Each line has its own stroke width, derived from a seed-defined range. Every frame, the width drifts by ±5, clamped between 2 and 30. So even as the lines ripple up and down, they also pulse slightly in weight — giving the composition a woven, breathing quality.

### The Animation

Fibonacci Fourier is the second animated volume in this series (after Bezier 2 and Brownian Graphe). The live plate:

- **Advances time** by `0.05` per frame
- **Redraws** the entire line field with the new time value
- **Recomputes** each line's height, colour, and width
- **Honors reduced motion** — if `prefers-reduced-motion: reduce`, the plate renders a single frame and does not animate
- **Pauses when hidden** — when the tab is not visible, animation stops to save CPU
- **Freezes for download** — when you click Download, the animation pauses, the current frame is captured, then the animation resumes

### The Surfaces

The same seed is rendered across four surface formats. These are static frames — they represent the print-ready composition.

| Surface  | Aspect | Material          |
|----------|--------|-------------------|
| Print    | 1 : 1  | Cotton rag        |
| Scarf    | 3 : 1  | Twill silk        |
| Textile  | 4 : 3  | Fabric yardage    |
| Wall     | 2 : 3  | Wallpaper         |

Each surface uses the same underlying seed and structural logic — only the repeat, orientation, and scale change.

### Stillness of the Catalogue

The framed plate, the surfaces, the archive, and the cover are all rendered as single frozen frames — captured at `t = 0`. They do not animate.

This is a deliberate design choice. A catalogue exists to present print-ready compositions; a printed scarf is not a moving target. Only the live plate shows the ripple, because that is where you experience the composition as it evolves.

---

## Usage

### In the browser

1. Open `index.html` in any modern browser.
2. The Plate 001 composition ripples on its own.
3. Click **New Seed** to generate a new composition.
4. Click **Download** to save the current frame as a PNG.
5. Scroll to the **Archive** section and click any plate to load it into Plate 001.

### Keyboard shortcuts

| Key | Action          |
|-----|-----------------|
| `R` | New seed        |
| `S` | Save as PNG     |

### Reproducing a composition

Each composition is identified by an 8-digit seed label displayed in the metadata panel. To reproduce a specific composition statically, note the seed and regenerate it programmatically:

```js
const rng = new RandomGenerator(seed);
const features = buildFeatures(rng);
renderComposition(canvas, features, rng, 0);
```

For an animated reproduction, use the same pattern with a `requestAnimationFrame` loop and call `renderComposition(canvas, features, rng, t)` with `t` advancing by `0.05` each frame.

---

## Technical Notes

- **No build step.** The system is a single HTML file with inline CSS and JavaScript.
- **No dependencies.** All drawing is done with the native Canvas 2D API.
- **Deterministic.** The `RandomGenerator` class uses a xorshift-based PRNG seeded by an integer, so identical seeds produce identical outputs.
- **Animation isolated to the plate.** Only the live Plate 001 uses `requestAnimationFrame`. All other canvases render a single frame and stop.
- **Feature isolation.** Cover, framed plate, surfaces, and archive thumbnails each derive their own feature set from their own local RNG, without disturbing the main plate's state.
- **Non-mutating static renders.** Inside `renderComposition`, working copies of the colour and line-width arrays are made, so the stored feature set is only mutated on the live plate — not on the static surfaces.
- **Bounded sine count.** The `numberOfSineFunctions` value is floored to an integer so the sine-sum loop always terminates cleanly.
- **Cleanup on visibility change.** Animation pauses when the tab is hidden, and restarts cleanly when it returns.
- **Responsive.** The layout adapts from large desktop down to very small mobile devices (tested at 360px viewport width).
- **Accessible.** Supports `prefers-reduced-motion`. Pinch-zoom is enabled.

### Browser support

Tested in current versions of:

- Chrome / Edge
- Firefox
- Safari (desktop and iOS)

---

## Licensing

All Fibonacci Fourier compositions are **seed-documented** and available for licensing across textile, surface, and print applications.

- **Standard licenses** cover single-product production runs.
- **Commercial use, custom editions, or exclusive rights** are available on request.

Each license is issued against a specific seed ID. Regeneration of the same seed produces the identical composition — ensuring reproducibility between artist, studio, and manufacturer.

For licensing enquiries: [reyhanehdaneshdoost@gmail.com](mailto:reyhanehdaneshdoost@gmail.com)

---

## Commission

Fibonacci Fourier is a generative design system, not a fixed artwork. It can be adapted for specific briefs:

| Service     | Description                                                       |
|-------------|-------------------------------------------------------------------|
| Licensing   | Existing seeds from the archive, licensed for production use      |
| Commission  | New compositions designed to your palette, repeat, and product    |
| Systems     | A private generative tool built for your studio's ongoing use     |

To begin a conversation: [reyhanehdaneshdoost@gmail.com](mailto:reyhanehdaneshdoost@gmail.com)

---

## Series

Fibonacci Fourier is part of a computational textile series. Each volume approaches ornament from a different structural angle:

| Volume                     | Structure                    | Motion                     |
|----------------------------|------------------------------|----------------------------|
| Girih 1                    | Islamic geometric            | Static                     |
| Arachne                    | Rotating rings               | Static                     |
| Baroque Me Baby            | Baroque frames               | Static                     |
| Bezier 1                   | Concentric curves            | Static                     |
| Bezier 2                   | Single rotating curve        | Animated (plate)           |
| Brownian Graphe            | Graph networks               | Animated + interactive     |
| Celestial Grove            | Recursive branch trees       | Static                     |
| ChaotiColor                | Cellular automata            | Static                     |
| Citrus Mosaic              | Arc-and-triangle tiles       | Static                     |
| Crazy Knight Curve         | Knight's-tour smooth path    | Static                     |
| Crazy Knight Line          | Knight's-tour gradient       | Static                     |
| Crazy Letter               | Framed wavy lines            | Static                     |
| cyPollock                  | Scattered branch field       | Static                     |
| Digital Pollen             | Noise-driven texture         | Static                     |
| Draconic Fractals          | Tiled dragon curve           | Static                     |
| Dreamscape Watercolors     | Layered watercolor blooms    | Static                     |
| Elliott Waves              | Financial chart              | Static                     |
| Ellipses                   | Concentric elliptical rings  | Static                     |
| Enigma Sudoku              | Playable 9×9 puzzle          | Interactive (plate)        |
| Ephemeral Whirls           | Wandering looper field       | Static                     |
| Eyes                       | Layered iris portrait        | Static                     |
| **Fibonacci Fourier**      | **Harmonic line field**      | **Animated (plate)**       |

The series is designed as a coherent whole — same page structure, same seed logic, same licensing and commission terms — so that each volume can be presented individually or as part of a larger body of work.

---

## Credits

- **Design & Generative System** — Reyhaneh Daneshdoost
- **Typefaces** — Cormorant Garamond · DM Mono
- **Platform** — Reyrove Studio
- **Edition** — Fibonacci Fourier, Autumn 2026

### On AI tools

Where technical obstacles were encountered, AI tools were used for debugging and code optimization. Every structural, aesthetic, and conceptual decision remained the artist's own.

---

## Links

- Website — [reyrove.github.io](https://reyrove.github.io/)
- Instagram — [@rey._.rove](https://www.instagram.com/rey._.rove/)
- LinkedIn — [Reyhaneh Daneshdoost](https://www.linkedin.com/in/reyhaneh-daneshdoost-730481160/)
- X — [@reyrove](https://x.com/reyrove)

---

© Fibonacci Fourier · All compositions reproducible by seed · Computational Textile Design