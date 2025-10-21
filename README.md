# BEAT-BOX

A minimalist web-based beat maker with an interactive piano keyboard for drone synthesis. Built as a single-file application focusing on simplicity and mobile-first design.

## Quick Start

**Try it live:**
🎵 [https://hashslingers.github.io/beat-box/initial/BeatBox_Pro_Minimal.html](https://hashslingers.github.io/beat-box/initial/BeatBox_Pro_Minimal.html)

**Run locally:**
```bash
open "initial/BeatBox_Pro_Minimal.html"
```

Or simply double-click the HTML file to open it in your browser.

## Features

- 4/8-step drum sequencer (kick, snare, hi-hat)
- Interactive 2-octave piano keyboard for chord/drone synthesis
- Tap tempo detection
- Real-time audio synthesis using Web Audio API (no samples)
- Pattern presets with local storage
- Mobile-first responsive design
- Vertical volume sliders
- Minimal black/white aesthetic

## Technology Stack

- **Frontend**: Vanilla JavaScript, HTML5, CSS3
- **Audio**: Web Audio API
- **Architecture**: Single-page application (self-contained HTML file)

## Project Structure

```
beat-box/
├── initial/
│   └── BeatBox_Pro_Minimal.html  # Single-file application
├── CLAUDE.md                     # Claude Code instructions
└── README.md                     # This file
```

## Architecture

BEAT-BOX is intentionally simple and self-contained:
- **Single-file architecture**: No build process or external dependencies
- **Vanilla JavaScript**: No frameworks required
- **Web Audio API**: All sounds synthesized in real-time (no audio samples)
- **Mobile-first**: Optimized for touch interfaces with 44x44px minimum touch targets
- **GitHub Pages**: Auto-deploys from `main` branch

## Browser Compatibility

Works in all modern browsers supporting Web Audio API:
- Chrome/Chromium (recommended)
- Firefox
- Safari (iOS requires user interaction before audio playback)
- Edge

**Note**: Safari/iOS requires user interaction (tap/click) before AudioContext can be started due to autoplay policies.

## Contributing

Contributions welcome! This project values simplicity and self-contained architecture. When contributing:
- Keep everything in the single HTML file
- Test on multiple browsers and mobile devices
- Ensure touch targets are at least 44x44px
- Maintain the minimalist aesthetic

## License

[License information needed]