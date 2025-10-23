# Pull Request: Guitar-Centric UX Visualizations

## 📋 Summary

Add creative and vibrant visualizations to BEAT-BOX specifically designed to enhance the playing experience for guitarists. This PR implements visual rhythm indicators, harmonic analysis, real-time audio visualization, and ambient visual feedback to help musicians lock into the groove, understand chord theory, and create an immersive practice environment.

**Target File:** `initial/BeatBox_Pro_Minimal.html`
**Type:** Feature Enhancement
**Priority:** Medium-High
**Estimated Complexity:** High (multiple interconnected visualizations)

---

## 🎯 Problem Statement

Current BEAT-BOX UX Issues for Guitarists:

1. **No Strong Rhythmic Visualization** - Steps highlight when playing, but no visual pulse, beat emphasis, or subdivision indicators
2. **Missing Harmonic Feedback** - Piano keyboard shows notes but not intervals, chord quality, or harmonic relationships
3. **Static Interface** - No movement, energy, or visual flow with the music
4. **No Sonic Visualization** - Can't see waveforms, frequency content, or how sounds blend
5. **Limited Beat Awareness** - No visual reinforcement of beat 1 (downbeat) or time signature

**Impact:** Guitarists struggle to lock into the pocket, understand chord voicings, practice with good time feel, and maintain engagement during practice sessions.

---

## ✨ Proposed Solution

Implement 7 complementary visualizations in priority order:

### Phase 1: Core Rhythm (High Impact)
1. **Circular Beat Visualizer** - Rotating playhead with beat emphasis
2. **Pulsing Background** - Ambient glow on beats
3. **Enhanced Step Numbers** - Stronger beat 1 visual

### Phase 2: Harmonic Analysis (Medium-High Impact)
4. **Chromatic Circle** - Interval wheel showing harmonic relationships
5. **Chord Detection** - Auto-detect and display chord names
6. **Color-Coded Keys** - Piano keys colored by interval function

### Phase 3: Sonic Feedback (Medium Impact)
7. **Waveform Display** - Real-time oscilloscope/spectrum analyzer

---

## 🔧 Technical Implementation Plan

### Architecture Overview

```
BeatBox_Pro_Minimal.html
├── CSS (Styles Section)
│   ├── Circular Beat Visualizer (.beat-circle, .beat-segment, .beat-playhead)
│   ├── Chromatic Circle (.chromatic-circle, .note-segment, .interval-line)
│   ├── Waveform Canvas (.waveform-canvas, .waveform-container)
│   ├── Pulsing Animations (@keyframes beatPulse, @keyframes flashDrum)
│   └── Visual Controls (.viz-controls, .viz-toggle)
│
├── HTML (Body Section)
│   ├── Visualization Container (new section after .tempo-section)
│   ├── Beat Circle SVG
│   ├── Chromatic Circle SVG
│   ├── Waveform Canvas
│   ├── Chord Display
│   └── Visualization Controls
│
└── JavaScript (Script Section)
    ├── Visualization State Management
    ├── Beat Circle Renderer (updateBeatCircle)
    ├── Chromatic Circle Renderer (updateChromaticCircle)
    ├── Chord Detection Logic (detectChord)
    ├── Waveform Analyzer (initWaveform, drawWaveform)
    ├── Animation Controllers (requestAnimationFrame loops)
    └── Event Handlers (toggle controls, theme changes)
```

---

## 📐 Detailed Component Specifications

### 1. Circular Beat Visualizer

**Location:** New section between `.tempo-section` and `.presets`

**HTML Structure:**
```html
<section class="visualization-section">
  <div class="viz-container">
    <div class="beat-circle-container">
      <svg class="beat-circle" viewBox="0 0 200 200" id="beatCircle">
        <!-- 8 segments (one per step) -->
        <!-- Rotating playhead -->
        <!-- Center step number -->
      </svg>
      <div class="beat-label">BEAT</div>
    </div>
  </div>
</section>
```

**CSS Requirements:**
```css
.beat-circle-container {
  width: 200px;
  height: 200px;
  position: relative;
}

.beat-segment {
  fill: #1a1a1a;
  stroke: #333;
  stroke-width: 1;
  transition: fill 0.1s ease-out;
}

.beat-segment.active {
  fill: #ffffff;
}

.beat-segment.beat-one {
  stroke: #ff4444;
  stroke-width: 2;
}

.beat-playhead {
  stroke: #ffffff;
  stroke-width: 3;
  stroke-linecap: round;
  transform-origin: 100px 100px;
  transition: transform 0.05s linear;
}
```

**JavaScript Logic:**
```javascript
// Initialize beat circle with 8 segments
function initBeatCircle() {
  const svg = document.getElementById('beatCircle');
  const segments = 8; // Match step count
  const anglePerSegment = 360 / segments;

  for (let i = 0; i < segments; i++) {
    const segment = createSegmentPath(i, anglePerSegment);
    segment.classList.add('beat-segment');
    if (i === 0) segment.classList.add('beat-one');
    svg.appendChild(segment);
  }

  // Add rotating playhead
  const playhead = createPlayhead();
  svg.appendChild(playhead);
}

// Update on each step
function updateBeatCircle(currentStep, totalSteps) {
  const segments = document.querySelectorAll('.beat-segment');
  segments.forEach((seg, i) => {
    seg.classList.toggle('active', i === currentStep);
  });

  const playhead = document.querySelector('.beat-playhead');
  const rotation = (currentStep / totalSteps) * 360;
  playhead.style.transform = `rotate(${rotation}deg)`;

  // Flash on beat 1
  if (currentStep === 0) {
    document.querySelector('.beat-circle-container').classList.add('flash');
    setTimeout(() => {
      document.querySelector('.beat-circle-container').classList.remove('flash');
    }, 100);
  }
}
```

**Integration Points:**
- Hook into existing `scheduleNote()` function at line ~2140
- Update on every step change
- Sync with `currentStep` variable

---

### 2. Chromatic Circle / Interval Wheel

**Location:** Beside beat circle in `.viz-container`

**HTML Structure:**
```html
<div class="chromatic-circle-container">
  <svg class="chromatic-circle" viewBox="0 0 200 200" id="chromaticCircle">
    <!-- 12 segments (chromatic notes) -->
    <!-- Connection lines between active notes -->
    <!-- Center chord name display -->
  </svg>
  <div class="chord-name" id="chordName">—</div>
</div>
```

**Chord Detection Algorithm:**
```javascript
const CHORD_PATTERNS = {
  // Triads
  'maj': [0, 4, 7],           // C E G
  'min': [0, 3, 7],           // C Eb G
  'dim': [0, 3, 6],           // C Eb Gb
  'aug': [0, 4, 8],           // C E G#

  // Sevenths
  'maj7': [0, 4, 7, 11],      // C E G B
  'min7': [0, 3, 7, 10],      // C Eb G Bb
  'dom7': [0, 4, 7, 10],      // C E G Bb
  'dim7': [0, 3, 6, 9],       // C Eb Gb A
  'm7b5': [0, 3, 6, 10],      // C Eb Gb Bb (half-diminished)

  // Extensions
  'maj9': [0, 4, 7, 11, 14],  // C E G B D
  'min9': [0, 3, 7, 10, 14],
  'sus2': [0, 2, 7],          // C D G
  'sus4': [0, 5, 7],          // C F G
  'add9': [0, 4, 7, 14],      // C E G D
  '6': [0, 4, 7, 9],          // C E G A
  'min6': [0, 3, 7, 9],       // C Eb G A
};

const NOTE_NAMES = ['C', 'C#', 'D', 'D#', 'E', 'F', 'F#', 'G', 'G#', 'A', 'A#', 'B'];

function detectChord(activeNotes) {
  if (activeNotes.length < 2) {
    return activeNotes.length === 1 ? activeNotes[0] : '—';
  }

  // Convert note names to chromatic indices
  const noteIndices = activeNotes.map(note => {
    const noteName = note.replace(/[0-9]/g, '');
    return NOTE_NAMES.indexOf(noteName);
  }).sort((a, b) => a - b);

  // Try all possible roots
  for (let root of noteIndices) {
    // Normalize to root = 0
    const intervals = noteIndices.map(n => (n - root + 12) % 12).sort((a, b) => a - b);

    // Match against patterns
    for (let [chordName, pattern] of Object.entries(CHORD_PATTERNS)) {
      if (arraysEqual(intervals, pattern)) {
        return NOTE_NAMES[root] + chordName;
      }
    }
  }

  // If no match, show note count
  return `${activeNotes.length} notes`;
}

// Update when drone notes change
function updateChromaticCircle() {
  const activeNotes = Array.from(state.drone.activeNotes);
  const chordName = detectChord(activeNotes);
  document.getElementById('chordName').textContent = chordName;

  // Update visual segments
  const segments = document.querySelectorAll('.note-segment');
  segments.forEach((seg, i) => {
    const noteName = NOTE_NAMES[i];
    const isActive = activeNotes.some(note => note.startsWith(noteName));
    seg.classList.toggle('active', isActive);
  });

  // Draw connection lines between active notes
  drawIntervalLines(activeNotes);
}
```

**Color Coding by Interval:**
```css
.note-segment.root { fill: #4444ff; }      /* Blue */
.note-segment.third { fill: #ffaa00; }     /* Orange */
.note-segment.fifth { fill: #00ff00; }     /* Green */
.note-segment.seventh { fill: #aa00ff; }   /* Purple */
.note-segment.extension { fill: #00ffff; } /* Cyan */
.note-segment.dissonant { fill: #ff4444; } /* Red */
```

**Integration Points:**
- Hook into `updateDroneNotes()` function
- Update when piano keys are pressed/released
- Sync with `state.drone.activeNotes` Set

---

### 3. Waveform / Oscilloscope Display

**Location:** New section in Advanced Options or standalone

**HTML Structure:**
```html
<div class="waveform-container">
  <canvas id="waveformCanvas" class="waveform-canvas"></canvas>
  <div class="waveform-controls">
    <button class="waveform-mode" id="waveformMode">OSCILLOSCOPE</button>
  </div>
</div>
```

**Web Audio Integration:**
```javascript
let analyser = null;
let waveformData = null;
let frequencyData = null;
let waveformMode = 'oscilloscope'; // or 'spectrum'
let animationId = null;

function initWaveform() {
  // Create analyser node
  analyser = audioContext.createAnalyser();
  analyser.fftSize = 2048;
  analyser.smoothingTimeConstant = 0.8;

  // Allocate data arrays
  waveformData = new Uint8Array(analyser.frequencyBinCount);
  frequencyData = new Uint8Array(analyser.frequencyBinCount);

  // Connect audio graph
  // Drone path: droneGain -> analyser -> masterGain
  droneGain.disconnect(masterGain);
  droneGain.connect(analyser);
  analyser.connect(masterGain);

  // Also connect drums for combined visualization
  drumGain.connect(analyser);

  // Start drawing
  drawWaveform();
}

function drawWaveform() {
  const canvas = document.getElementById('waveformCanvas');
  const ctx = canvas.getContext('2d');
  const width = canvas.width;
  const height = canvas.height;

  if (waveformMode === 'oscilloscope') {
    analyser.getByteTimeDomainData(waveformData);

    ctx.fillStyle = '#000000';
    ctx.fillRect(0, 0, width, height);

    ctx.lineWidth = 2;
    ctx.strokeStyle = '#ffffff';
    ctx.beginPath();

    const sliceWidth = width / waveformData.length;
    let x = 0;

    for (let i = 0; i < waveformData.length; i++) {
      const v = waveformData[i] / 128.0;
      const y = v * height / 2;

      if (i === 0) {
        ctx.moveTo(x, y);
      } else {
        ctx.lineTo(x, y);
      }

      x += sliceWidth;
    }

    ctx.stroke();
  } else {
    // Spectrum analyzer mode
    analyser.getByteFrequencyData(frequencyData);

    ctx.fillStyle = '#000000';
    ctx.fillRect(0, 0, width, height);

    const barWidth = (width / frequencyData.length) * 2.5;
    let x = 0;

    for (let i = 0; i < frequencyData.length; i++) {
      const barHeight = (frequencyData[i] / 255) * height;

      // Color gradient based on frequency
      const hue = (i / frequencyData.length) * 360;
      ctx.fillStyle = `hsl(${hue}, 100%, 50%)`;

      ctx.fillRect(x, height - barHeight, barWidth, barHeight);
      x += barWidth + 1;
    }
  }

  // Pulse on beat 1
  if (state.sequencer.currentStep === 0 && state.sequencer.isPlaying) {
    ctx.shadowBlur = 20;
    ctx.shadowColor = '#ffffff';
  } else {
    ctx.shadowBlur = 0;
  }

  animationId = requestAnimationFrame(drawWaveform);
}

// Toggle mode
document.getElementById('waveformMode').addEventListener('click', () => {
  waveformMode = waveformMode === 'oscilloscope' ? 'spectrum' : 'oscilloscope';
  document.getElementById('waveformMode').textContent =
    waveformMode === 'oscilloscope' ? 'OSCILLOSCOPE' : 'SPECTRUM';
});
```

**Performance Optimization:**
```javascript
// Only draw when playing or notes active
function drawWaveform() {
  if (!state.sequencer.isPlaying && state.drone.activeNotes.size === 0) {
    // Draw static grid when idle
    drawIdleWaveform();
    return;
  }

  // Full drawing logic...

  animationId = requestAnimationFrame(drawWaveform);
}
```

**Integration Points:**
- Initialize after `audioContext` is created (line ~1330)
- Connect to existing audio graph
- Update on play/stop state changes

---

### 4. Pulsing Background / Ambient Glow

**CSS Implementation:**
```css
/* Beat pulse animation */
@keyframes beatPulse {
  0% { background-color: #000000; }
  5% { background-color: #0d0d0d; }
  100% { background-color: #000000; }
}

@keyframes beatOnePulse {
  0% { background-color: #000000; }
  5% { background-color: #1a0a0a; } /* Slight red tint */
  100% { background-color: #000000; }
}

body.beat-pulse {
  animation: beatPulse 0.15s ease-out;
}

body.beat-one-pulse {
  animation: beatOnePulse 0.2s ease-out;
}

/* Drum-specific color flashes */
@keyframes kickFlash {
  0%, 100% { box-shadow: none; }
  50% { box-shadow: 0 0 30px rgba(255, 68, 68, 0.3); }
}

@keyframes snareFlash {
  0%, 100% { box-shadow: none; }
  50% { box-shadow: 0 0 30px rgba(255, 170, 0, 0.3); }
}

@keyframes hihatFlash {
  0%, 100% { box-shadow: none; }
  50% { box-shadow: 0 0 30px rgba(0, 170, 255, 0.3); }
}

.sequencer.kick-hit {
  animation: kickFlash 0.2s ease-out;
}

.sequencer.snare-hit {
  animation: snareFlash 0.2s ease-out;
}

.sequencer.hihat-hit {
  animation: hihatFlash 0.2s ease-out;
}
```

**JavaScript Triggers:**
```javascript
function triggerVisualPulse(step, track) {
  // Background pulse
  if (step === 0) {
    document.body.classList.add('beat-one-pulse');
    setTimeout(() => document.body.classList.remove('beat-one-pulse'), 200);
  } else {
    document.body.classList.add('beat-pulse');
    setTimeout(() => document.body.classList.remove('beat-pulse'), 150);
  }

  // Drum color flashes
  if (track && state.pattern[track][step]) {
    const sequencer = document.querySelector('.sequencer');
    sequencer.classList.add(`${track}-hit`);
    setTimeout(() => sequencer.classList.remove(`${track}-hit`), 200);
  }
}

// Integrate into scheduleNote function (around line 2140)
function scheduleNote(step, time) {
  // Existing scheduling logic...

  // Add visual trigger
  const lookahead = time - audioContext.currentTime;
  setTimeout(() => {
    Object.keys(state.pattern).forEach(track => {
      if (state.pattern[track][step]) {
        triggerVisualPulse(step, track);
      }
    });
  }, lookahead * 1000);
}
```

**User Control:**
```html
<!-- Add to advanced options -->
<div class="slider-group">
  <label class="slider-label">
    <input type="checkbox" id="ambientPulse" checked> AMBIENT PULSE
  </label>
</div>
```

---

### 5. Enhanced Step Number Display

**CSS Updates:**
```css
.step-number {
  text-align: center;
  font-size: 10px;
  font-weight: 500;
  color: #666;
  letter-spacing: 0.1em;
  padding: 4px 0;
  transition: all 0.2s ease;
  position: relative;
}

.step-number.active {
  color: #ffffff;
  font-weight: 700;
  transform: scale(1.3);
}

.step-number.beat-one {
  color: #ff4444;
  font-weight: 700;
  font-size: 12px;
}

.step-number.beat-one::before {
  content: '●';
  position: absolute;
  left: -8px;
  color: #ff4444;
  animation: pulse 0.5s ease-in-out infinite;
}

.step-number.active.beat-one {
  color: #ff6666;
  text-shadow: 0 0 10px rgba(255, 68, 68, 0.8);
}
```

**JavaScript Update:**
```javascript
function updateStepNumbers(currentStep) {
  const stepNumbers = document.querySelectorAll('.step-number');
  stepNumbers.forEach((num, i) => {
    num.classList.toggle('active', i === currentStep);
  });
}

// Call in scheduleNote
```

---

### 6. Color-Coded Piano Keys by Interval

**JavaScript Enhancement:**
```javascript
function updatePianoKeyColors() {
  const activeNotes = Array.from(state.drone.activeNotes);

  if (activeNotes.length === 0) {
    // Reset all keys
    document.querySelectorAll('.piano-key').forEach(key => {
      key.style.removeProperty('border-color');
      key.style.removeProperty('background');
    });
    return;
  }

  // Find root (lowest note or most common)
  const root = activeNotes[0]; // Simple: use first note
  const rootIndex = NOTE_NAMES.indexOf(root.replace(/[0-9]/g, ''));

  // Color each active key by interval
  document.querySelectorAll('.piano-key').forEach(keyEl => {
    const note = keyEl.dataset.note;
    if (!activeNotes.includes(note)) return;

    const noteIndex = NOTE_NAMES.indexOf(note.replace(/[0-9]/g, ''));
    const interval = (noteIndex - rootIndex + 12) % 12;

    let color;
    switch (interval) {
      case 0: color = '#4444ff'; break;  // Root - Blue
      case 3: case 4: color = '#ffaa00'; break;  // 3rd - Orange
      case 7: color = '#00ff00'; break;  // 5th - Green
      case 10: case 11: color = '#aa00ff'; break; // 7th - Purple
      case 2: case 9: color = '#00ffff'; break;  // 2nd/9th - Cyan
      default: color = '#ff4444'; break; // Dissonance - Red
    }

    keyEl.style.borderColor = color;
    keyEl.style.boxShadow = `0 0 10px ${color}40`;
  });
}

// Call when notes change
```

---

### 7. Visualization Control Panel

**HTML Structure:**
```html
<section class="viz-controls-section">
  <div class="viz-controls">
    <label class="viz-toggle">
      <input type="checkbox" id="vizBeatCircle" checked>
      <span>Beat Circle</span>
    </label>

    <label class="viz-toggle">
      <input type="checkbox" id="vizChromatic" checked>
      <span>Chord Wheel</span>
    </label>

    <label class="viz-toggle">
      <input type="checkbox" id="vizWaveform" checked>
      <span>Waveform</span>
    </label>

    <label class="viz-toggle">
      <input type="checkbox" id="vizPulse" checked>
      <span>Ambient Pulse</span>
    </label>

    <label class="viz-toggle">
      <input type="checkbox" id="vizColorKeys" checked>
      <span>Color Keys</span>
    </label>
  </div>
</section>
```

**CSS Styling:**
```css
.viz-controls {
  display: flex;
  gap: 20px;
  padding: 20px 40px;
  border-top: 1px solid var(--border);
  justify-content: center;
  flex-wrap: wrap;
}

.viz-toggle {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 10px;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: #999;
  cursor: pointer;
  transition: color 0.2s;
}

.viz-toggle:hover {
  color: #fff;
}

.viz-toggle input[type="checkbox"] {
  appearance: none;
  width: 16px;
  height: 16px;
  border: 1px solid #333;
  background: #000;
  cursor: pointer;
  transition: all 0.2s;
}

.viz-toggle input[type="checkbox"]:checked {
  background: #fff;
  border-color: #fff;
}

.viz-toggle input[type="checkbox"]:checked::after {
  content: '✓';
  display: block;
  text-align: center;
  color: #000;
  font-size: 12px;
  line-height: 16px;
}
```

**JavaScript State Management:**
```javascript
const vizState = {
  beatCircle: true,
  chromatic: true,
  waveform: true,
  pulse: true,
  colorKeys: true
};

// Toggle handlers
document.getElementById('vizBeatCircle').addEventListener('change', (e) => {
  vizState.beatCircle = e.target.checked;
  document.querySelector('.beat-circle-container').style.display =
    e.target.checked ? 'block' : 'none';
});

// ... similar for other toggles

// Persist to localStorage
function saveVizPreferences() {
  localStorage.setItem('beatbox-viz-prefs', JSON.stringify(vizState));
}

function loadVizPreferences() {
  const saved = localStorage.getItem('beatbox-viz-prefs');
  if (saved) {
    Object.assign(vizState, JSON.parse(saved));
    // Update UI checkboxes
  }
}
```

---

## 🤖 Suggested Agent Breakdown

### Agent 1: **rhythm-visualizer-agent**
**Purpose:** Implement circular beat visualizer and step number enhancements

**Tasks:**
1. Create SVG circular beat visualizer with 8 segments
2. Add rotating playhead that syncs with current step
3. Implement beat 1 (downbeat) visual emphasis
4. Add pulsing animation on beat 1
5. Enhance step numbers with active state and beat 1 indicator
6. Hook into existing `scheduleNote` function for timing sync
7. Test synchronization at various tempos (60-180 BPM)

**Files to Modify:**
- `initial/BeatBox_Pro_Minimal.html` (CSS, HTML, JS sections)

**Dependencies:**
- Must understand existing sequencer timing logic
- Sync with `currentStep` and `totalSteps` variables

**Acceptance Criteria:**
- [ ] Circular visualizer displays 8 segments matching step count
- [ ] Playhead rotates smoothly 360° per loop
- [ ] Beat 1 has distinct visual emphasis (color, size, or flash)
- [ ] Step numbers highlight current step
- [ ] Zero audio timing issues or drift

---

### Agent 2: **harmonic-analyzer-agent**
**Purpose:** Implement chromatic circle and chord detection

**Tasks:**
1. Create SVG chromatic circle with 12 note segments
2. Implement chord detection algorithm (triads, 7ths, extensions)
3. Display detected chord name in center of circle
4. Draw connection lines between active notes
5. Color-code piano keys by interval function
6. Add interval labels (root, 3rd, 5th, 7th, etc.)
7. Handle edge cases (single notes, complex voicings, unrecognized chords)

**Files to Modify:**
- `initial/BeatBox_Pro_Minimal.html`

**Dependencies:**
- Hook into `state.drone.activeNotes` Set
- Update when piano keys pressed/released
- Coordinate with existing drone note display

**Acceptance Criteria:**
- [ ] Chromatic circle shows all 12 notes
- [ ] Active notes highlight correctly
- [ ] Chord detection works for common chords (maj, min, 7ths)
- [ ] Piano keys show interval colors when chord is active
- [ ] Displays "—" when no notes, note name for single note
- [ ] Updates in real-time (<50ms latency)

---

### Agent 3: **waveform-visualizer-agent**
**Purpose:** Implement real-time audio visualization with Web Audio API

**Tasks:**
1. Create `<canvas>` element for waveform display
2. Initialize Web Audio AnalyserNode
3. Connect analyser to audio graph (drone + drums)
4. Implement oscilloscope mode (time domain)
5. Implement spectrum analyzer mode (frequency domain)
6. Add mode toggle button
7. Optimize requestAnimationFrame loop for performance
8. Add beat-synchronized visual effects (glow on beat 1)

**Files to Modify:**
- `initial/BeatBox_Pro_Minimal.html`

**Dependencies:**
- Existing `audioContext`, `droneGain`, `drumGain` nodes
- Must not disrupt audio routing
- Performance critical - target 60fps

**Acceptance Criteria:**
- [ ] Oscilloscope displays clean waveform in real-time
- [ ] Spectrum analyzer shows frequency bars
- [ ] Toggle switches between modes smoothly
- [ ] Canvas auto-resizes responsively
- [ ] No audio glitches or latency introduced
- [ ] Runs at 60fps on mobile devices
- [ ] Gracefully handles paused state (shows static grid)

---

### Agent 4: **ambient-pulse-agent**
**Purpose:** Implement pulsing background and drum color flashes

**Tasks:**
1. Create CSS keyframe animations for background pulse
2. Add drum-specific color flash animations (kick=red, snare=orange, hihat=blue)
3. Implement JavaScript triggers in `scheduleNote` function
4. Add user toggle control for ambient pulse
5. Optimize animation timing to match audio perfectly
6. Test performance impact on older devices
7. Add localStorage persistence for user preference

**Files to Modify:**
- `initial/BeatBox_Pro_Minimal.html`

**Dependencies:**
- Sync with `scheduleNote` timing
- Coordinate with existing step playback

**Acceptance Criteria:**
- [ ] Background pulses subtly on every beat
- [ ] Beat 1 has stronger pulse with slight red tint
- [ ] Drum hits trigger appropriate color flashes
- [ ] Toggle control works and persists
- [ ] No visual jank or frame drops
- [ ] Intensity scales with volume settings

---

### Agent 5: **viz-control-panel-agent**
**Purpose:** Create unified control panel for all visualizations

**Tasks:**
1. Create visualization control section with toggles
2. Implement show/hide for each visualization
3. Add localStorage persistence for all preferences
4. Create "Visualization Presets" (minimal, full, performance)
5. Add responsive layout for mobile
6. Implement smooth transitions when toggling
7. Test accessibility (keyboard navigation, screen readers)

**Files to Modify:**
- `initial/BeatBox_Pro_Minimal.html`

**Dependencies:**
- All other visualization agents must be complete
- Must integrate with existing advanced options section

**Acceptance Criteria:**
- [ ] All visualizations can be toggled individually
- [ ] Preferences persist across sessions
- [ ] Presets work correctly (minimal/full/performance)
- [ ] Mobile layout is touch-friendly
- [ ] Keyboard accessible (Tab, Space, Enter)
- [ ] No layout breaks with different combinations

---

### Agent 6: **integration-testing-agent**
**Purpose:** Test all visualizations together and optimize performance

**Tasks:**
1. Test all visualizations simultaneously at max settings
2. Profile CPU/GPU usage and identify bottlenecks
3. Optimize requestAnimationFrame loops
4. Test on mobile devices (iOS Safari, Android Chrome)
5. Verify audio sync across all tempos (60-180 BPM)
6. Test with various screen sizes (320px - 1920px)
7. Create performance mode that disables expensive visualizations
8. Document performance characteristics and recommendations

**Files to Modify:**
- `initial/BeatBox_Pro_Minimal.html`
- Create `PERFORMANCE.md` documentation

**Dependencies:**
- All other agents must be complete

**Acceptance Criteria:**
- [ ] All visualizations work together without conflicts
- [ ] CPU usage <30% on mid-range devices
- [ ] 60fps maintained on mobile
- [ ] Audio sync drift <10ms over 1 minute
- [ ] No memory leaks during extended use
- [ ] Performance mode reduces CPU by 50%+

---

## 📋 Implementation Checklist

### Pre-Implementation
- [ ] Review current `BeatBox_Pro_Minimal.html` structure
- [ ] Identify all integration points
- [ ] Create backup branch
- [ ] Set up local testing environment

### Phase 1: Core Rhythm (Agents 1, 4)
- [ ] Circular beat visualizer
- [ ] Enhanced step numbers
- [ ] Pulsing background
- [ ] Beat sync verification

### Phase 2: Harmonic Analysis (Agent 2)
- [ ] Chromatic circle
- [ ] Chord detection
- [ ] Color-coded piano keys
- [ ] Interval visualization

### Phase 3: Sonic Visualization (Agent 3)
- [ ] Canvas setup
- [ ] Oscilloscope mode
- [ ] Spectrum analyzer mode
- [ ] Performance optimization

### Phase 4: Integration (Agents 5, 6)
- [ ] Control panel
- [ ] User preferences
- [ ] Performance testing
- [ ] Mobile optimization

### Post-Implementation
- [ ] Cross-browser testing (Chrome, Safari, Firefox)
- [ ] Mobile testing (iOS, Android)
- [ ] Documentation updates
- [ ] User guide for new features

---

## 🎨 Design Principles

1. **Maintain Minimalism** - Add visualizations without cluttering
2. **Performance First** - Target 60fps on mobile
3. **User Control** - Everything should be toggleable
4. **Musical Accuracy** - Zero timing drift or audio glitches
5. **Accessibility** - Keyboard navigation, screen reader support
6. **Educational** - Help users learn music theory
7. **Guitarist-Centric** - Focus on rhythm, harmony, and groove

---

## 🧪 Testing Strategy

### Unit Tests
- Chord detection algorithm with known inputs
- Interval calculation accuracy
- SVG path generation correctness

### Integration Tests
- Audio graph connectivity (no broken audio)
- Timing sync between visualizations
- State management consistency

### Performance Tests
- CPU/GPU profiling at max settings
- Memory leak detection (24hr run)
- Frame rate monitoring (target 60fps)
- Audio latency measurement (<20ms)

### User Acceptance Tests
- Can musician lock into the groove?
- Does chord detection feel intuitive?
- Are visualizations helpful or distracting?
- Mobile usability testing

---

## 📊 Success Metrics

### Quantitative
- [ ] 60fps on iPhone 12 and newer
- [ ] <30% CPU usage on mid-range desktop
- [ ] <10ms audio sync drift over 1 minute
- [ ] <2MB total file size increase
- [ ] Zero audio glitches under normal use

### Qualitative
- [ ] Guitarists report improved time feel
- [ ] Users understand chord theory better
- [ ] Practice sessions feel more engaging
- [ ] Interface still feels minimal and clean
- [ ] Mobile experience is smooth

---

## 🚀 Deployment Plan

### Development Branch
```bash
git checkout -b claude/guitar-ux-visualization-011CUP9hYqznGnBTiDY2965B
```

### Testing Checklist
1. Test locally with `open initial/BeatBox_Pro_Minimal.html`
2. Test on mobile devices (iOS Safari, Android Chrome)
3. Verify GitHub Pages deployment
4. Get user feedback from guitarist testers

### Rollout Strategy
1. **Soft Launch** - Deploy with all visualizations off by default
2. **Beta Testing** - Invite guitarists to test and provide feedback
3. **Full Launch** - Enable default visualizations based on feedback
4. **Iteration** - Refine based on usage data

---

## 📝 Documentation Updates Required

### README.md
- Add "Visualizations" section
- Document new features
- Add screenshots of visualizations
- Update feature list

### CLAUDE.md
- Document visualization architecture
- Add performance optimization notes
- Update code standards for canvas usage

### User Guide (new file)
- How to use each visualization
- Explanation of chord detection
- Tips for guitarists
- Troubleshooting guide

---

## ⚠️ Known Limitations & Future Work

### Current Limitations
- Chord detection limited to common chords (no complex jazz voicings)
- Waveform visualization CPU-intensive on older devices
- No support for time signatures other than 4/4
- Limited to 8 steps (no 16-step visualization yet)

### Future Enhancements
- Guitar fretboard visualization (show notes on neck)
- Strumming pattern generator
- Rhythm notation display
- MIDI input support for external keyboards
- Export visualizations as video/GIF
- Customizable color themes
- BPM detection from audio input

---

## 🔗 Related Issues & PRs

- Related to UX improvements for musicians
- Builds on existing piano keyboard implementation
- Complements tap tempo functionality
- Enhances mobile-first design philosophy

---

## 📸 Visual Mockups

_(Agents should generate these during implementation)_

### Beat Circle Mockup
```
     [1]
    /   \
  8|  ○  |2
   |  ●  | (playhead pointing to current step)
  7|     |3
    \   /
   6  5  4

  Colors:
  - Step 1: Red border
  - Active step: White fill
  - Inactive: Dark gray
```

### Chromatic Circle Mockup
```
      C
   B     C#
 Bb        D
A     ◉     E (◉ = center chord name display)
 Ab        F
   G     F#
      G

  Active notes connected with white lines
  Color-coded by interval from root
```

### Waveform Canvas Mockup
```
+---------------------------+
|  OSCILLOSCOPE    [TOGGLE] |
|  ～～～～～～～～～～～～～  |
|        (waveform)          |
|  ～～～～～～～～～～～～～  |
+---------------------------+
  Auto-scales based on amplitude
  Glows on beat 1
```

---

## 💬 Questions for Reviewers

1. Should waveform be always-on or opt-in (performance concerns)?
2. Default state for visualizations - all on or progressive disclosure?
3. Should we add preset themes (minimal/full) or individual toggles?
4. Priority: Guitar fretboard or strumming pattern visualizer?
5. Any specific chord voicings critical for guitarists?

---

## ✅ Definition of Done

- [ ] All 6 agents have completed their tasks
- [ ] Code passes self-review checklist
- [ ] Performance benchmarks met
- [ ] Cross-browser testing complete
- [ ] Mobile testing complete
- [ ] Documentation updated
- [ ] Screenshots/GIFs added to PR
- [ ] User guide created
- [ ] Accessibility tested
- [ ] Code committed with clear messages
- [ ] Pushed to feature branch
- [ ] Ready for review

---

## 🙏 Acknowledgments

This PR addresses the need for enhanced visual feedback specifically for guitarists using BEAT-BOX as a practice tool. The visualizations aim to help musicians:
- Develop stronger internal timing
- Understand chord theory visually
- Stay engaged during practice
- Explore harmonic relationships
- Create an immersive musical environment

**Target Users:** Guitarists at all levels, especially those working on rhythm, chord transitions, and improvisation over drones.

---

**Agent Instructions:** Please implement this PR in phases, following the agent breakdown. Each agent should:
1. Read this entire document thoroughly
2. Focus only on their assigned tasks
3. Test their implementation independently
4. Document any issues or deviations
5. Verify integration points with other agents
6. Commit with clear, descriptive messages
7. Update this PR with progress notes

**Coordination:** Agents should implement in order (1→2→3→4→5→6) to minimize merge conflicts and dependency issues.
