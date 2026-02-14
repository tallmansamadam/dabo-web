# DABO // Ferengi Wheel

A browser-based implementation of the Dabo wheel from *Star Trek: Deep Space Nine* — the Ferengi casino game found at Quark's Bar.

![HTML5](https://img.shields.io/badge/HTML5-static-green)
![Zero Dependencies](https://img.shields.io/badge/dependencies-0-blue)
![License](https://img.shields.io/badge/license-MIT-yellow)

## Overview

DABO is a single-file, zero-dependency web game styled as a retro Ferengi gaming terminal. Players select slots on a 36-position wheel, place bets in gold-pressed latinum, and spin three independent symbol rings to match combinations for payouts.

## How to Play

1. **Select Slots** — Click 1–3 numbered slots on the outer ring of the wheel. Red-shaded slots (5–7, 17–19, 29–31) are house-reserved and cannot be bet on.
2. **Place a Bet** — Enter a wager between 10–100 latinum per slot. Your total stake is the bet multiplied by the number of selected slots.
3. **Spin** — Hit the Spin button. Three inner rings (A, B, C) spin independently and land on random offsets.
4. **Collect Winnings** — For each selected slot, the three symbols at that position are evaluated:

| Combination | Payout |
|---|---|
| Q + 🌀 + 🌀 (Quark + two Swirls) | **4000x** bet (Jackpot) |
| Three of a kind | **100x** bet |
| Two of a kind | **5x** bet |
| No match | 0x |

Starting bankroll is **1,000 latinum**. If your bankroll drops below the minimum bet (10), a reset is required.

## Symbols

| Glyph | Name |
|---|---|
| ▮▮ | BAR |
| ● | ORB |
| ☄ | COMET |
| ⚡ | BOLT |
| ☾ | MOON |
| ☉ | SUN |
| 🌀 | SWIRL |
| ⬡ | DS9 |
| Q | QUARK |

## Features

- **Single HTML file** — no build tools, no frameworks, no dependencies
- **SVG wheel** — procedurally generated 36-slot wheel with three independent symbol rings
- **Cryptographic RNG** — uses `crypto.getRandomValues()` for unbiased random outcomes, with `Math.random()` fallback
- **WebAudio sound engine** — synthesized spin noise, win/loss tones, and a speech synthesis "DABO!" callout on wins
- **Accessible** — supports `prefers-reduced-motion` (wheel snaps instantly), keyboard navigation, ARIA labels, and focus management on modals
- **Responsive** — fluid layout from mobile portrait to widescreen desktop with adaptive ASCII art sizing
- **Persistent audio preferences** — mute state and volume are saved to `localStorage`
- **Quark result modal** — ASCII art portrait with randomized Ferengi commentary on wins and losses

## Running Locally

Open the file directly in any modern browser:

```
# macOS
open index.html

# Linux
xdg-open index.html

# Windows
start index.html
```

Or serve it with any static server:

```bash
npx serve .
# or
python -m http.server 8000
```

## Technical Details

### Architecture

Everything lives in `index.html`:

- **CSS** (~445 lines) — custom properties, CRT-style scanline overlay, responsive grid with breakpoints at 980px, 760px, 520px, and 560px height
- **HTML** (~130 lines) — semantic markup with SVG wheel, control panel, and a result modal
- **JavaScript** (~940 lines, IIFE) — wheel geometry, ring builder, animation loop, payout engine, audio engine, modal management

### Wheel Geometry

- 36 slots at 10° each
- Four concentric zones: slot numbers (outer), Ring A, Ring B, Ring C (inner)
- Text auto-rotates to stay upright regardless of position

### Animation

- Three-phase system: `cruise` (constant velocity) → `decel` (cubic ease-out to target) → `stopped`
- Each ring receives independent random velocity, extra revolutions, and deceleration timing for a staggered stop effect
- `requestAnimationFrame` loop with delta-time tracking

### RNG

- Rejection sampling over `Uint32Array` via `crypto.getRandomValues()` to eliminate modulo bias
- Falls back to `Math.random()` if the Web Crypto API is unavailable

## Browser Support

Any modern browser with ES2017+ support (Chrome, Firefox, Safari, Edge). The Web Crypto API and Web Audio API are used where available with graceful fallbacks.

## License

MIT
