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
- Minimal black/white aesthetic with JetBrains Mono font
- All code in one HTML file for easy deployment

### Audio Architecture
- **Drums**: Synthesized kick, snare, hi-hat using oscillators and noise
- **Drone**: Multi-oscillator synthesis (sawtooth, square, triangle) with:
  - Multi-note/chord support via JavaScript Set
  - Automatic gain reduction for chords: `Math.max(0.5, 1 / Math.sqrt(noteCount))`
  - Shared lowpass filter and chorus effect
- **Scheduler**: Mobile-optimized with 20-25ms lookahead
- **Latency**: Target <20ms for real-time feel

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