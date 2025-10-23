# BEAT-BOX

A minimalist web-based beat maker with interactive piano keyboard and authentic pump organ drone synthesis.

## Quick Start

**Live Demo:**
🌐 https://hashslingers.github.io/beat-box/

**Run Locally:**
```bash
open "initial/BeatBox_Pro_Minimal.html"
```

Or simply double-click the HTML file to open it in your browser.

## Current Features

### Drum Sequencer
- 4/8-step drum sequencer with 3 synthesized tracks (kick, snare, hi-hat)
- Tap tempo detection with median calculation
- Pattern presets (3 built-in patterns)
- Real-time sequencer with <20ms latency
- Mobile-optimized scheduler (20-25ms lookahead)

### Drone Synthesis
- **7 Pump Organ Variations:**
  - **Pump Organ - Wheezy** (Default) - Vintage with exaggerated air wobble
  - Pump Organ - Classic - Traditional with slow attack
  - Pump Organ - Church - Rich harmonics, powerful & stable
  - Pump Organ - Celeste - Shimmering detuned ranks
  - Pump Organ - Vox Humana - Voice-like with formant filtering
  - Harmonium - Nasal Indian harmonium with quick attack
  - Theater Organ - Dramatic vibrato, bright tone
- Interactive 2-octave piano keyboard (C2-C4, 25 keys)
- Multi-note chord support with automatic gain reduction
- Smooth volume control without audio glitches
- Authentic LFO/vibrato for realistic organ feel

### Audio Features
- Real-time audio synthesis using Web Audio API (no samples)
- Tempo control (60-180 BPM)
- Vertical volume sliders for drums and drone
- Performance monitoring built-in
- Haptic feedback on mobile
- Battery optimization for mobile devices

### Visual Feedback (NEW)
- **Circular Beat Visualizer** - Rotating playhead showing current step with beat 1 emphasis
- **Enhanced Step Numbers** - Active step highlighting with special beat 1 indicator
- **Ambient Pulse** - Subtle background pulse synchronized with beats
- **Drum Color Flashes** - Visual feedback for kick (red), snare (orange), hi-hat (blue)
- **Toggleable Visualizations** - User controls to show/hide each visualization
- **Performance Optimized** - Smooth 60fps animations on mobile devices

## Technology Stack

- **Frontend**: Vanilla JavaScript, HTML5, CSS3
- **Audio**: Web Audio API
- **Architecture**: Single-page application (self-contained HTML file)

## Project Structure

```
beat-box/
├── initial/
│   └── BeatBox_Pro_Minimal.html      # Main application
├── CLAUDE.md                          # Claude Code instructions
├── Product_Requirements_Document.md   # Product vision & roadmap
├── README.md                          # This file
└── index.html                         # Redirects to main app
```

## Design Philosophy

BEAT-BOX is intentionally simple and self-contained:
- **Minimal UI** with maximum functionality
- **Mobile-first** design with touch optimization
- **Real-time audio synthesis** (no samples or external dependencies)
- **Single-file architecture** for easy deployment and sharing
- **Black/white aesthetic** with JetBrains Mono font

## Development Roadmap

See `Product_Requirements_Document.md` for the complete vision of evolving into a collaborative, cloud-based music production platform.

## Browser Compatibility

Works in all modern browsers supporting Web Audio API:
- Chrome/Chromium
- Firefox  
- Safari
- Edge

## GitHub Integration with Claude

This repository has Claude AI integration enabled through GitHub Actions. You can get AI assistance directly in GitHub issues and pull requests.

### How to Use Claude Integration

**In GitHub Issues:**
- Create a new issue or comment on existing ones
- Type `@claude` followed by your request
- Example: `@claude can you review the audio latency in the drum sequencer?`

**In Pull Requests:**
- Comment with `@claude` and your question
- Example: `@claude please review this code for performance issues`
- Claude automatically reviews new pull requests

### What Claude Can Help With
- Review code changes
- Suggest improvements
- Answer questions about the codebase
- Help with debugging
- Provide implementation guidance

### Example Usage
1. Go to https://github.com/hashslingers/beat-box/issues
2. Click "New issue"
3. Write: `@claude help me optimize the Web Audio API performance`
4. Claude will respond with analysis and suggestions

## License

[License information needed]