# BeatBox Pro

A web-based drum sequencer and guitar drone synthesizer evolving into a comprehensive music production platform.

## Quick Start

**Option 1: GitHub Pages (Recommended)**
Access the live applications online:
- [Guitar Drone & Beats (Main MVP)](https://hashslingers.github.io/beat-box/Guitar%20Drone%20&%20Beats%20%E2%80%93%20Mobile%20Ready.html)
- [Design Mockups Gallery](https://hashslingers.github.io/beat-box/design_mockups/)
- [All Applications](https://hashslingers.github.io/beat-box/)

**Option 2: Local Development**
```bash
# Clone the repository
git clone https://github.com/hashslingers/beat-box.git
cd beat-box

# Open the main MVP
open "docs/Guitar Drone & Beats – Mobile Ready.html"

# Or serve locally with any HTTP server
python3 -m http.server 8000
# Then visit http://localhost:8000/docs/
```

## Current Features (MVP v1.0)

- 8-step drum sequencer with 3 tracks (kick, snare, hi-hat)
- Guitar drone synthesizer with 11 preset notes
- Real-time audio synthesis using Web Audio API
- Tempo control (60-180 BPM)
- Pattern presets and local storage
- URL-based pattern sharing
- Mobile-responsive design with performance mode
- Bluetooth latency compensation

## Technology Stack

- **Frontend**: Vanilla JavaScript, HTML5, CSS3
- **Audio**: Web Audio API
- **Architecture**: Single-page application (self-contained HTML file)

## Project Structure

```
beat-box/
├── Product_Requirements_Document.md  # Comprehensive product vision & roadmap
├── docs/                             # GitHub Pages site (static web assets)
│   ├── Guitar Drone & Beats – Mobile Ready.html  # Current MVP
│   ├── BeatBox_Minimal.html         # Minimal drum sequencer
│   ├── BeatBox_Pro_Minimal.html     # Enhanced version
│   ├── BeatBox_Pro_Mobile.html      # Mobile-optimized version
│   ├── Design_Mockups.html          # UI design mockups
│   ├── Volume_Control_Mockups.html  # Volume control designs
│   └── design_mockups/              # Interactive design gallery
│       ├── index.html               # Design mockups gallery
│       ├── enhanced_pattern_editor.html
│       ├── rhythm_controls.html
│       ├── guitar_integration.html
│       ├── collaboration_interface.html
│       ├── mobile_interactions.html
│       ├── audio_controls.html
│       └── design_documentation.md
├── initial/                         # Original development files
└── README.md                        # This file
```

## Development Roadmap

See `Product_Requirements_Document.md` for the complete vision transforming this into BeatBox Pro - a collaborative, cloud-based music production platform with:

- Multi-user real-time collaboration
- Unlimited tracks and advanced sequencing
- Plugin ecosystem and marketplace
- AI-powered composition tools
- Professional audio processing
- Microservices architecture

**Target**: 10M monthly users, $50M ARR by Year 3

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