# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

DRN-3 is a single-file browser application — one HTML file containing all HTML, CSS, and JavaScript. No build system, no dependencies, no server required. Open `DRN-3.html` directly in a browser to run it.

## Architecture

Everything lives in `DRN-3.html`. The structure within the file is:

1. **CSS** — all styles in a `<style>` block in `<head>`. Sections: base layout, machine/panel structure, knob components, LED indicators, bottom bar, responsive breakpoints (≤620px tablet, ≤460px mobile stack).

2. **HTML** — the machine panel, left to right: TEMPO → DRIFT → SEQUENCE (3 steps) → GLIDE → FILTER → TUBE → LFO. This order mirrors the audio signal flow and should be maintained.

3. **JavaScript** — a single `<script>` block at the end of `<body>`, organized in this order:
   - Constants and `params` object (all knob values live here)
   - Audio node variable declarations
   - Curve/helper functions (`makeTubeCurve`, `toneGainDb`, `blendedFreqAtTime`, `glideTimeSecs`, `lfoDepthHz`, `driftWalkTick`)
   - `initAudio()` — builds the Web Audio graph, called on first play
   - Filter/sequencer helpers (`updateFilterParams`, `scheduleStep`, `scheduler`)
   - LED sync
   - Play/stop
   - Knob rendering (`valToAngle`, `drawTickRing`, `updateKnobVisual`)
   - Param display (`formatParam`, `updateParamDisplay`)
   - Knob interaction (mouse + touch drag handlers)
   - `applyParam()` — live audio updates when a knob changes
   - `initKnobs()` / `initKnobRings()` — boot

## Audio Signal Chain

```
osc (sawtooth) → oscGain → filter (lowpass) → shaper (tube WaveShaper) → toneFilter (highshelf) → masterGain → destination
```

Parallel modulations:
- `driftWalkTick()` → `osc.detune` (random walk, runs continuously)
- `filterLFO` → `filterLFOGain` → `filter.frequency` (additive, free-running)

## Key Conventions

**Knob system** — every knob is a `.knob` div with `data-param`, `data-min`, `data-max`, `data-val` attributes. The `params` object is the single source of truth. Adding a knob requires: HTML markup, entry in `params`, case in `formatParam()`, case in `applyParam()`, and adding the ID to `initKnobRings()`.

**Large vs small knobs** — BPM, DRIFT, and the three PITCH knobs use the large (56px) knob size. All others use the `knob-sm` class (44px). The `largeKnobs` array in `initKnobRings()` controls which get larger tick rings.

**Scheduling** — uses the Chris Wilson lookahead pattern: a `setTimeout` loop running every 25ms schedules Web Audio events up to 100ms ahead. LEDs are synced to audio time via a separate `setTimeout` computed from the difference between scheduled audio time and `audioCtx.currentTime`.

**Glide** — `glideState` tracks the previous step's `{ startFreq, targetFreq, startTime, duration, shape }` so `blendedFreqAtTime()` can compute the correct in-flight frequency when a new step fires. Shape 0 = native `linearRampToValueAtTime`, shape 1 = native `exponentialRampToValueAtTime`, intermediate = 64-point interpolated blend.

**Tube saturation** — `makeTubeCurve(drive)` generates a `Float32Array` and must be reassigned to `shaper.curve` when drive changes (Web Audio requires a new array, not mutation).

**Drift** — `driftWalkTick()` runs on a randomized 125–500ms interval. It reads `params.oscDrift` live on each tick, so knob changes take effect within one tick without any explicit `applyParam` handler.
