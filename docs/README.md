# BeatBox Pro Documentation

## Mobile-First Music Production Platform

### Current Versions

1. **BeatBox_Pro_Minimal.html** - Minimalist black/white design with advanced features
2. **BeatBox_Pro_Mobile.html** - Modern mobile-first design with dark blue theme

### Features

#### Core Functionality
- 8-step drum sequencer (kick, snare, hi-hat)
- 4/8 step switching capability
- Guitar drone synthesizer with 11 notes
- Tap tempo with median calculation
- Swing timing adjustment
- Pattern presets with adaptive layouts

#### Mobile Optimizations
- 44px minimum touch targets (iOS HIG compliant)
- Haptic feedback support
- Mobile gesture experiments:
  - Swipe to clear steps
  - Long press to copy patterns
  - Pinch to toggle performance mode
- Optimized audio scheduling (20ms intervals on mobile)
- Battery-conscious animations

#### Audio Engine
- Web Audio API synthesis
- Real-time compression and mastering
- Low-latency performance (<20ms)
- Cross-browser compatibility

### Design System

#### Mobile-First Theme
- Background: `#0f172a` (dark blue)
- Panels: `#111827`, `#1f2937`, `#374151`
- Accent: `#f59e0b` (orange)
- Text: `#e5e7eb`, `#9ca3af`
- Border radius: `12px`
- Shadows: `0 8px 32px rgba(0,0,0,0.4)`

#### Typography
- Primary: Inter font family
- Weights: 400, 500, 600, 700
- Optimized for mobile readability

### Performance Monitoring

The app includes built-in performance monitoring:
- Frame drop detection
- Memory usage tracking
- Audio glitch detection
- Auto-optimization for low-end devices

### Development

#### Local Testing
```bash
open initial/BeatBox_Pro_Mobile.html
```

#### Key Files
- `initial/BeatBox_Pro_Mobile.html` - Main mobile app
- `design_mockups/` - UI design explorations
- `.claude/agents/` - Specialist agent configurations
- `CLAUDE.md` - Development instructions

### Browser Support

- Chrome/Edge 80+
- Safari 13+
- Firefox 75+
- Mobile Safari (iOS 13+)
- Chrome Mobile (Android 8+)

### Future Roadmap

See `Product_Requirements_Document.md` for complete platform vision including:
- Real-time collaboration
- Plugin ecosystem
- WebAssembly DSP modules
- Professional mixing features