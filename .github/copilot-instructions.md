# GitHub Copilot Instructions for BEAT-BOX

## Project Overview

BEAT-BOX is a minimalist web-based beat maker with an interactive piano keyboard for drone synthesis. This is a single-file application focusing on simplicity and mobile-first design.

### Current State

- **Type**: Single-file HTML application
- **Main File**: `initial/BeatBox_Pro_Minimal.html`
- **Technology**: Vanilla JavaScript + Web Audio API
- **Deployment**: GitHub Pages (auto-deploys from `main` branch)

### Key Features

- 4/8-step drum sequencer (kick, snare, hi-hat)
- Interactive 2-octave piano keyboard for chord/drone synthesis
- Tap tempo detection
- Mobile-first responsive design with touch optimization
- Vertical volume sliders
- Pattern presets with local storage
- Real-time audio synthesis (no audio samples)

## Technology Stack

- **Languages**: HTML5, CSS3, Vanilla JavaScript (no frameworks)
- **Audio Engine**: Web Audio API for all synthesis
- **Architecture**: Self-contained single-file application
- **Fonts**: JetBrains Mono
- **Design**: Minimal black/white aesthetic

## Code Standards

### JavaScript

- Use vanilla JavaScript only (no frameworks or libraries)
- ES6+ syntax preferred
- Descriptive variable and function names
- Mobile-optimized event handling (touch and mouse)
- AudioContext initialization requires user interaction (Safari/iOS compliance)

### Audio Architecture

- **Drums**: Synthesized using oscillators and noise generators (no samples)
- **Drone Synthesis**: Multi-oscillator (sawtooth, square, triangle) with:
  - Multi-note/chord support via JavaScript Set
  - Automatic gain reduction for chords: `Math.max(0.5, 1 / Math.sqrt(noteCount))`
  - Shared lowpass filter and chorus effect
- **Scheduler**: Mobile-optimized with 20-25ms lookahead
- **Target Latency**: <20ms for real-time responsiveness

### Piano Keyboard Specifications

- 25 keys spanning C2 to C4 (2 full octaves)
- White keys: 28px wide × 110px tall
- Black keys: 18px wide × 70px tall (absolutely positioned)
- Middle C (C3) highlighted with black badge and star
- Grid layout spanning full width
- Support for single notes and chords

### UI/UX Standards

- Mobile-first design approach
- Touch targets minimum 44×44px
- Vertical volume sliders use inverted Y-coordinates (bottom = 100%)
- Responsive design that works on all screen sizes
- Performance mode for mobile devices
- No emojis in code unless explicitly requested

### File Organization

- Single HTML file contains all HTML, CSS, and JavaScript
- No external dependencies or imports
- Self-contained for easy deployment and sharing

## Development Workflow

### No Build Process

This is a single-file application with no build step. Changes are made directly to the HTML file.

### Testing

```bash
# Open locally
open "initial/BeatBox_Pro_Minimal.html"

# View live version
open "https://hashslingers.github.io/beat-box/initial/BeatBox_Pro_Minimal.html"
```

### Browser Testing Requirements

- Test in Chrome, Safari, and Firefox
- Safari/iOS requires user interaction before AudioContext works
- Verify touch interactions on mobile devices
- Check audio latency and performance

### Deployment

- Changes pushed to `main` branch auto-deploy to GitHub Pages
- Typically updates within 1-2 minutes
- Live site: https://hashslingers.github.io/beat-box/initial/BeatBox_Pro_Minimal.html

## Important Technical Notes

### Audio Context

- Must be initialized after user interaction (iOS/Safari requirement)
- Use `AudioContext.resume()` on first user gesture
- Handle both `webkitAudioContext` and `AudioContext` for compatibility

### Tempo Calculation

- Tap tempo uses median calculation (not average) for stability
- Supports 60-180 BPM range

### Volume Sliders

- Vertical orientation with inverted coordinates
- Bottom position = 100% volume
- Top position = 0% volume

### Mobile Optimization

- Use passive event listeners where appropriate
- Minimize repaints and reflows
- Optimize audio scheduling for mobile CPUs
- Handle touch events properly (touchstart, touchmove, touchend)

## Key Files

- `initial/BeatBox_Pro_Minimal.html` - Main application (single file)
- `CLAUDE.md` - Claude Code AI assistant instructions
- `README.md` - Project overview and quick start
- `.github/workflows/claude.yml` - Claude Code integration
- `.github/workflows/claude-code-review.yml` - Automated PR reviews
- `.github/copilot-instructions.md` - This file (GitHub Copilot instructions)

## Project Philosophy

BEAT-BOX prioritizes:

1. **Simplicity**: Single-file architecture, no dependencies
2. **Performance**: Real-time audio with minimal latency
3. **Mobile-First**: Touch-optimized, responsive design
4. **Self-Contained**: Everything in one HTML file for easy sharing
5. **Synthesis Over Samples**: All audio generated in real-time

## Common Tasks

### Adding New Drum Sounds

1. Create new oscillator-based synthesis function
2. Add to scheduler for sequencer integration
3. Update UI with new track row
4. Ensure mobile touch targets are adequate

### Modifying Piano Keyboard

1. Maintain key size specifications
2. Keep grid layout structure
3. Preserve chord support via Set data structure
4. Update gain reduction formula if changing oscillator count

### Adjusting Audio Latency

1. Modify scheduler lookahead (currently 20-25ms)
2. Test on multiple devices (especially mobile)
3. Balance between latency and audio dropouts

### UI Changes

1. Maintain minimal black/white aesthetic
2. Keep JetBrains Mono font
3. Ensure mobile touch targets ≥44×44px
4. Test responsive behavior on multiple screen sizes

## Prohibited Practices

- Do not add external dependencies or libraries
- Do not split into multiple files
- Do not use audio sample files
- Do not add emojis to code (unless user explicitly requests)
- Do not add frameworks (React, Vue, etc.)
- Do not break single-file architecture

## Best Practices

- Always test audio features across browsers
- Verify mobile touch interactions
- Maintain performance on older mobile devices
- Keep code readable and well-commented
- Preserve the minimalist design aesthetic
- Test GitHub Pages deployment after changes
