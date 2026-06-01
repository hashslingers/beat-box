# BEAT-BOX — Handoff

**Status:** All work is committed and pushed to `main` (synced with `origin/main`, clean tree). The app was redesigned ("No Frills Studio", day/night) and its audio engine reworked across four shipped commits. Nothing is blocked or in-progress in code — the one open item is **subjective ear-confirmation by the user** of the audio level/tone choices (an agent can't hear; everything was tuned by offline measurement).

---

## Where the work lives

| What | Where |
|---|---|
| Branch | `main` (deploys to GitHub Pages on push) |
| Live app | `initial/BeatBox_Pro_Minimal.html` (single file; **this is the primary/live app**) |
| Live URL | https://hashslingers.github.io/beat-box/initial/BeatBox_Pro_Minimal.html |
| Alt designs (reference only) | `initial/redesign-deepink.html`, `initial/redesign-swiss.html` — **visual snapshots that predate the audio work; they do NOT have the audio fixes** |
| Design source | `~/code/codalabs/agent-factory` (`mockups/v6-nofrills.html` + `_design-brief.md` = the chosen language) |
| Audio test entry point | open the app with `?audittest=1` → `window.__bbOffline(opts)` renders the real engine offline |
| Project memory | `~/.claude/projects/-Users-j-mac-mini-Documents-intent-prototypes-beat-box/memory/` |
| Recent commits | `db98c4d` alts · `5a1f5cc` subsonic HPF · `f5ad518` level normalization · `07f3902` cross-talk/loudness · `470553e` redesign |

---

## Open decisions / items for the user (not blockers — they need ears, not code)

1. **Confirm the drone↔drum balance by ear, for the sound + key you actually play.** Tuned by offline RMS to land the drone ~1–2 dB under the *audible* drums at 60–70% fader. If it feels a hair loud/quiet, it's a one-number change: scale `DRONE_SOURCE_TRIM` (global) or a single entry. **Recommendation:** leave as-is until the user reports a specific symptom.
2. **Tonal-shape EQ was deliberately left to the user.** Only the objectively-justified subsonic high-pass was applied. The mids are slightly scooped (good — leaves room for guitar); highs were left alone (measured 8 kHz+ includes soft-clip harmonics, so "adding air" = adding distortion). **Recommendation:** only act on a concrete complaint ("boomy" / "dull" / "harsh"), then target that band.
3. **Residual ~15% sub-energy <30 Hz on drums** (down from 56%; DC eliminated). Could push `masterHP` to ~35 Hz for more, but that risks the kick's 40 Hz body. **Recommendation:** leave at 30 Hz unless the user wants a tighter low end.
4. **Two non-blocking UI polish notes** from the redesign review: desktop play-ring clearance is a touch tight; the fretboard "SHOW NOTES" card hugs the right edge. Cosmetic. **Recommendation:** skip unless asked.

---

## What's done (shipped)

- **UI redesign** → "No Frills Studio": white/ink hairline, Helvetica, highlighter-swipe state, coral-pen play ring; **day/night** toggle (localStorage `bb-theme`; URL `?theme=` / `?view=`). CSS-override layer + toggle script appended to the original file — base markup/JS untouched there.
- **Audio: cross-talk fixed** — drums & drone were on one shared 12:1 compressor (drum hits ducked the drone). Split into independent drum/drone buses; recorded mp3 rerouted onto the drone bus. Replaced an over-engineered async AudioWorklet limiter with a synchronous tanh **soft-clip** master (iOS-safe, can't pump).
- **Audio: loudness + level matching** — `DRONE_SOURCE_TRIM` (per-sound) + `DRONE_KEY_TRIM` (per-key, the 12 mp3 recordings were ~11 dB apart) normalize every voice so the drone tracks the drums and switching sound/key no longer jumps level. Fixed cold-start `droneSound` desync (inited to `tanpura` while menu showed `custom_audio`).
- **Audio: subsonic/DC high-pass** (`masterHP`, 30 Hz) — the kick swept to ~0 Hz, dumping ~56% of drum energy + DC below 30 Hz (inaudible, wasted headroom).
- All verified: no clipping (peak ≤ −1.6 dBFS, 0 clips), drum punch preserved, every drone sound + drums + faders work live with **zero console errors**. See `CLAUDE.md` → Audio Architecture for the node graph and rationale.

---

## Reproduce / verify (audio changes)

Headless-Chrome + CDP is required because `OfflineAudioContext.startRendering()` does **not** resolve under `--virtual-time-budget` — you must poll on real wall-clock via DevTools Protocol.

- **Screenshots** (UI, JS executed): `"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless --disable-gpu --hide-scrollbars --screenshot=out.png --window-size=440,2200 --virtual-time-budget=4000 "file://<abs>/initial/BeatBox_Pro_Minimal.html?theme=day&view=beatbox"` then Read the PNG.
- **Audio measurement:** load `BeatBox_Pro_Minimal.html?audittest=1`, call `window.__bbOffline({mode:'master'|'droneBus'|'drumBus', droneType:'tanpura'|'wurlitzer'|'pump_organ_classic'|'warm_pad'|'mp3', droneGain, drumGain, notes, mp3Url, ...})` → returns `{peakDb, rmsDb, clip, channelL/R}`. Drive it from a small Node CDP runner (Node 25 has global `WebSocket`/`fetch`): launch Chrome with `--remote-debugging-port`, `--allow-file-access-from-files`, navigate, poll `document.title==='DONE'`. **NB: the scratch harnesses/runners were in `/tmp` and `initial/_*.html` and were deleted after use — recreate from the pattern in `CLAUDE.md`.** For mp3 measurement, extract a slice first: `ffmpeg -ss <keyOffsetSec> -t 10 -i initial/drones.mp3 -ac 2 -ar 44100 initial/_slice.wav` (key offsets = `DRONE_FILE_MAP` minutes × 60).
- **Live functional test:** iframe the app (same-origin file://), fire the events handlers actually listen for (drone keys = `pointerdown`, sliders = `input`, select = `change`), assert DOM state + capture `console.error`/`window.onerror`. The benign `navigator.vibrate` headless warning is NOT an app error.
- **Key invariants to re-check after ANY audio edit:** isolated `droneBus` RMS invariant to drums (cross-talk = 0); master peak ≤ ~−1.5 dBFS & 0 clips at `droneGain:1.0, drumGain:1.0` with a chord; drone-vs-drum offset and per-source/per-key spread; sub-30 % + DC offset. **The `__bbOffline` hook hand-mirrors the live `initAudio` graph — keep both master stages identical when editing.**

---

## When you return

Everything is shipped and synced; there is no broken state to fix. **Start by asking the user what they heard** — the audio was tuned entirely by measurement (no ears in the loop), so the next real signal is their feedback on level/tone for the specific drone sound and key they play. If they report a symptom, it maps to a small, localized change: "drone too loud/quiet" → scale `DRONE_SOURCE_TRIM`; "boomy/dull/harsh" → a targeted EQ band (re-measure the spectrum first, per `CLAUDE.md`); "X sound is off-level" → its `DRONE_SOURCE_TRIM` entry. Re-run the offline measurement invariants above after any audio change before pushing. If the user is happy, the lowest-risk leftover polish is the two cosmetic UI notes in Open Decision #4.
