# Drone Sound Enhancement - Issue #16

## Summary
This PR implements the Wurlitzer Electric Piano drone voice and improves overall audio quality across all drone synthesizers.

## Changes

### 1. New Wurlitzer Electric Piano Drone (`createWurlitzerDrone`)
A faithful synthesis of the classic Wurlitzer 200A electric piano character:

**FM Synthesis for Bell-like Attack:**
- 2:1 modulator-to-carrier ratio creates the characteristic metallic bell tone
- FM depth envelope starts strong and decays for the distinctive "bark" on attack
- Smooth `setTargetAtTime` envelopes eliminate clicks and pops

**Tremolo (6-8 Hz):**
- Classic 7 Hz amplitude modulation for authentic Wurlitzer vibrato
- Master tremolo node affects all voice components uniformly
- Adjustable depth for subtle to pronounced effect

**Characteristic "Bark":**
- Resonant peaking filter that sweeps from 1400 Hz down to 600 Hz on attack
- Filter gain envelope creates the percussive "bark" then fades
- Simulates the reed/tine attack characteristic of real Wurlitzers

**Warm Low-Mids:**
- Peaking EQ boost at 350 Hz for rich warmth
- Multiple lowpass stages for vintage character
- High-frequency rolloff at 4500 Hz for natural sound

### 2. Subtle Room Reverb (Global Enhancement)
Added algorithmic reverb to the master audio chain:
- Multi-tap delay network with early reflections and late reverb
- Prime-number-based delay times for natural diffusion
- Feedback loop with lowpass filtering for smooth decay
- Very subtle mix (15%) for room ambience without muddiness
- Connected as parallel send from compressor

### 3. Improved Existing Drones

**All Drones:**
- Replaced `exponentialRampToValueAtTime` with `setTargetAtTime` for smoother, click-free envelopes
- Reduced filter Q values to minimize resonance ringing/artifacts
- Added warmth EQ boosting low-mids

**Tanpura Drone:**
- Added very subtle shimmer LFO (0.15 Hz) for string vibration character
- Added presence EQ at 2500 Hz for improved clarity
- Smoother envelope attack

**Warm Pad Drone:**
- Added chorus LFO modulation for movement
- Reduced filter Q from 5 to 2.5 for cleaner sound
- Smoother filter sweep using `setTargetAtTime`

**Air Organ Drone:**
- Reduced vibrato depth (8→6) and speed (5→4.5 Hz) for cleaner sound
- Added warmth EQ
- Reduced filter Q (3→1.5) to minimize ringing

## Technical Details

### Audio Chain
```
Sound Sources → Compressor → Master Gain → Destination
                    ↓
              Reverb Send → Reverb → Master Gain
```

### Wurlitzer Signal Flow
```
Modulator → ModGain → Carrier Frequency (FM)
Carrier → CarrierGain ↘
TineOsc → TineGain   → TremoloMaster → WarmthEQ → BarkFilter → Lowpass → HighCut → Output
SubOsc  → SubGain    ↗
```

## Testing
- Select "Wurlitzer EP (Synth)" from the Drone Sound dropdown
- Play notes on the piano keyboard or use the drone wheel
- Listen for the bell-like attack, tremolo effect, and warm character
- Compare with other drones to verify consistent volume levels

## Files Changed
- `initial/BeatBox_Pro_Minimal.html` - All changes in single-file app
