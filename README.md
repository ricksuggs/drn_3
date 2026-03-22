# DRN-3 · DRONE SEQUENCER

A 3-step analog drone sequencer running entirely in the browser. No frameworks, no build step — one HTML file, open and play.

---

## What it is

DRN-3 is a monophonic drone sequencer built on the Web Audio API, designed to sound warm, slightly dirty, and mechanically alive. The aesthetic is East German test equipment: anodized aluminum, amber indicators, stamped panel labels. The sound is Trans-Europe Express, Tenebre, a machine left running overnight in a cold room.

Three steps. Each step has a pitch. They loop. That's the whole idea.

## Sound

- **Oscillator** — sawtooth wave, the primary voice
- **Portamento** — exponential glide between steps; weighted and gravitational, not snappy
- **Drift** — a very slow LFO (~0.07 Hz) modulates the oscillator's detune by ±4.5 cents, with its own frequency randomized over time so the wander never repeats
- **Filter** — resonant lowpass, cutoff mapped logarithmically from 80 Hz to 8 kHz
- **Filter LFO** — free-running sine LFO modulates the filter cutoff additively; runs independently of the sequencer and never resets on stop/start
- **Waveshaper** — tanh soft-clipping curve for tube warmth on the output
- **Scheduling** — Web Audio clock lookahead (Chris Wilson's method) for tight, drift-free timing

Default pitches: **D3 → C3 → Bb2** — a descending minor motif in the bass register.

## Controls

| Knob | Range | Effect |
|------|-------|--------|
| BPM | 40 – 180 | Sequence tempo (half-note steps) |
| PITCH × 3 | C1 – C4 | Pitch for each step (chromatic, MIDI) |
| CUTOFF | 80 Hz – 8 kHz | Filter cutoff frequency (logarithmic) |
| RES | 0.5 – 20 | Filter resonance / Q |
| GLIDE | 0.01s – 1.2s | Portamento time (exponential feel) |
| LFO RATE | 0.1 – 5 Hz | Filter LFO speed; default 0.30 Hz (very slow breath) |
| LFO DEPTH | 0 – 100% | Filter LFO intensity; depth is exponentially scaled so low values stay subtle |

Drag knobs **up** to increase, **down** to decrease. Touch drag works on mobile.

**SPACE** toggles play/stop from the keyboard.

## Usage

```
open DRN-3.html
```

That's it. No server required. Works in any modern browser with Web Audio API support (Chrome, Firefox, Safari, Edge).

On first play the browser may require a user gesture to start the AudioContext — clicking **RUN** satisfies this.

## Design references

**Sound:** Kraftwerk — *Autobahn*, *Trans-Europe Express*, *Radio-Activity* · Goblin — *Suspiria*, *Tenebre*, *Profondo Rosso*

**Visual:** Robotron, Vermona, early Korg and Roland hardware — utilitarian panel design from before aesthetics were considered a feature

## File structure

```
DRN-3.html    — the entire application
```

Single file. Vanilla HTML, CSS, JavaScript. No dependencies.

---

*VEB KOMBINAT · ELEKTRONIK · DDR*
