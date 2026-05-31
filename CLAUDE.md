# Claude Code Instructions for BEAT-BOX

This document provides context for Claude Code when working on the BEAT-BOX project.

## Project Overview

BEAT-BOX is a minimalist web-based beat maker with an interactive piano keyboard for drone synthesis. Built as a single-file application focusing on simplicity and mobile-first design.

## Current State

- **Status**: Functional single-file HTML application (Optimized for Guitarists)
- **Location**: `initial/BeatBox_Pro_Minimal.html`
- **Technology**: Vanilla JavaScript + Web Audio API
- **Focus**: Mobile-first, immediate drone + beat setup
- **Features**:
  - 4/8-step drum sequencer (Kick, Snare, Hi-Hat)
  - **Drone Wheel**: Intuitive pitch selector based on the Circle of Fifths
  - **Custom Audio Drones**: Supports 60-minute `drones.mp3` file with 5-minute loops per key
  - Interactive 2-octave piano keyboard (Advanced Mode)
  - Simplified Drone Selection: Recorded Drone, Tanpura, Pump Organ, Warm Pad
  - Snappy Tap Tempo (Median-based, 2s reset, 5-tap window)
  - Visual Synchronization: `requestAnimationFrame` draw loop with `noteQueue`
  - Screen Wake Lock API integration
  - Presets: Metronome, Rock, Funk, Jazz
  - Haptic feedback and touch-optimized gestures
  - **Day/Night themes**: header toggle (persisted to `localStorage` as `bb-theme`); URL params `?theme=day|night` and `?view=beatbox|fretboard`
  - **"No Frills Studio" visual language** (adapted from the agent-factory design system): pure white paper / inverted ink, hairline borders, Helvetica, semantic highlighter swipes encoding state (green = armed step, coral = playhead), coral-pen play ring

## Development Guidelines

### Testing the Application
```bash
# Run the current app locally
open "initial/BeatBox_Pro_Minimal.html"

# View live version
open "https://hashslingers.github.io/beat-box/initial/BeatBox_Pro_Minimal.html"
```

### Key Files
- `initial/BeatBox_Pro_Minimal.html` - Current working application (single file)
- `CLAUDE.md` - This file - Claude Code instructions
- `README.md` - Project overview and quick start guide
- `.github/workflows/claude.yml` - Claude Code integration for GitHub issues/PRs
- `.github/workflows/claude-code-review.yml` - Automated PR reviews by Claude
- `.gitignore` - Ignores `.DS_Store` files

### Code Standards
- Vanilla JavaScript (no frameworks) for simplicity
- Web Audio API for all audio synthesis (no audio samples)
- Mobile-first responsive design with touch optimization
- Self-contained single-file architecture (no external dependencies)
- "No Frills Studio" aesthetic: pure white/ink, hairline 1.5px borders, Helvetica (no monospace), semantic highlighter swipes; day/night theming via `[data-theme]` on `<html>`
- All code in one HTML file for easy deployment

### Audio Architecture
- **Drums**: Synthesized kick, snare, hi-hat using oscillators and noise
- **Drone**: Multi-oscillator synthesis (sawtooth, square, triangle) with:
  - Multi-note/chord support via JavaScript Set
  - Automatic gain reduction for chords: `Math.max(0.5, 1 / Math.sqrt(noteCount))`
  - Shared lowpass filter and chorus effect
- **Scheduler**: Mobile-optimized with 20-25ms lookahead
- **Latency**: Target <20ms for real-time feel
- **Mix bus topology** (`initAudio`): drums and drone run on **separate buses** so drum hits never duck the drone (no shared compressor). Drums → gentle `compressor` (ratio ~5) → `drumBusGain` → master; drone → `droneBus` → `droneComp` → `droneBusGain` → master. Master = makeup `masterGain` → memoryless **tanh soft-clip** (`makeSoftClipCurve`, `masterLimiter`, no time constant so it cannot pump) → destination. Loudness comes from gain-staging into the soft clip; tune via measurement, not by ear. The recorded `drones.mp3` is ~12 dB quieter than the synth drones, so the `custom_audio` path is boosted by `CUSTOM_DRONE_BOOST` (applied in `playCustomDrone` + `updateDroneVolume`). NB the soft clip is a *shared* nonlinearity: cross-talk is eliminated at the bus level, with a small peak-only residual (~0.5 dB on hard hits, no pumping) at the master.
- **Audio test hook**: loading the app with `?audittest=1` exposes `window.__bbOffline(opts)`, which renders the real engine through an `OfflineAudioContext` and returns `{peakDb, rmsDb, clip}` (modes: `master`/`droneBus`/`drumBus`). Inert without the flag. Use it to regression-test loudness/clipping/cross-talk after any audio change (drive it via Chrome DevTools Protocol with real wall-clock polling — `OfflineAudioContext.startRendering` does not resolve under `--virtual-time-budget`). **The hook mirrors the live graph by hand — keep both master stages identical when editing.**

### Piano Keyboard Implementation
- 25 keys (C2 to C4) - 2 full octaves
- White keys: 28px wide, 110px tall
- Black keys: 18px wide, 70px tall, absolutely positioned
- Middle C (C3) highlighted with black badge and star
- Supports single notes or chords
- Grid layout: Full-width section that spans all columns

### Development Commands
No build process - single HTML file deployment.

**Git & GitHub:**
```bash
git add <file>
git commit -m "message"
git push  # Auto-deploys to GitHub Pages
```

**GitHub Pages:**
- Live site: https://hashslingers.github.io/beat-box/initial/BeatBox_Pro_Minimal.html
- Auto-deploys from `main` branch on push
- Typically updates within 1-2 minutes

### Important Notes
- Always test audio features in multiple browsers (Chrome, Safari, Firefox)
- Safari/iOS requires user interaction before AudioContext works
- Mobile touch targets should be 44x44px minimum
- Vertical volume sliders use inverted Y-coordinates (bottom = 100%)
- Tap tempo uses median calculation for stability (not average)
- No emojis in code unless explicitly requested by user

## GitHub Integration

**Repository**: https://github.com/hashslingers/beat-box
**Live Site**: https://hashslingers.github.io/beat-box/initial/BeatBox_Pro_Minimal.html

**GitHub Actions enabled:**
- **GitHub Pages**: Auto-deploys from `main` branch
- **Claude Code**: Tag `@claude` in issues/PRs for AI assistance
- **Claude Code Review**: Automatic PR reviews

## Project Philosophy

BEAT-BOX is intentionally simple and self-contained. Focus on:
- Minimal UI with maximum functionality
- Mobile-first design and touch optimization
- Real-time audio synthesis (no samples or dependencies)
- Single-file architecture for easy deployment and sharing