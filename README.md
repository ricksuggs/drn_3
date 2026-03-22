# DRN-3 · DRONE SEQUENCER

A 3-step analog drone sequencer running entirely in the browser. No frameworks, no build step — one HTML file, open and play.

**[▶ Launch DRN-3](https://ricksuggs.github.io/drn_3/DRN-3.html)**

---

## What it is

DRN-3 is a monophonic drone sequencer built on the Web Audio API, designed to sound warm, slightly dirty, and mechanically alive. The aesthetic is East German test equipment: anodized aluminum, amber indicators, stamped panel labels. The sound is Trans-Europe Express, Tenebre, a machine left running overnight in a cold room.

Three steps. Each step has a pitch. They loop. That's the whole idea.

## Sound

- **Oscillator** — sawtooth wave, the primary voice
- **Portamento** — continuously blendable glide between steps; linear shape moves pitch at a constant rate (mechanical, Kraftwerk); exponential shape closes distance quickly then settles (gravitational, analog); midpoint is a hybrid that feels neither fully robotic nor fully organic
- **Drift** — a random walk algorithm wanders the oscillator pitch continuously and unpredictably; each tick takes a small random step with gentle center attraction to prevent permanent runaway; smoothed with a long time constant so movement feels geological rather than nervous; never resets on stop/start
- **Filter** — resonant lowpass, cutoff mapped logarithmically from 80 Hz to 8 kHz
- **Filter LFO** — free-running sine LFO modulates the filter cutoff additively; runs independently of the sequencer and never resets on stop/start
- **Tube saturation** — asymmetric tanh WaveShaper with a small class-A bias, producing even harmonics at low drive and progressive compression at high drive; always active, never fully bypassed
- **Tone shelf** — post-saturation high shelf at 4 kHz; rolls off harshness added by drive or adds presence
- **Step probability** — each step has an independent trigger probability evaluated fresh on every cycle; skipped steps leave the oscillator droning at its last pitch; the sequencer clock never stutters
- **Skip routing (HLD/ADV)** — per-step toggle determines what happens on a skip; HLD holds the current pitch silently; ADV chains immediately to the next step's evaluation in the same time slot; chains cascade until a step triggers or all steps are exhausted (silent safety valve); mixed modes across steps produce complex emergent rhythmic behaviour
- **Tape delay** — a physical tape echo simulation in the feedback path: each repeat passes through a lowpass filter (darkening), a tanh WaveShaper (saturation accumulation), and a gain stage; a free-running wow LFO modulates the delay time continuously for pitch wobble that intensifies in later repeats; dry/wet mix preserves the dry signal at any setting
- **Scheduling** — Web Audio clock lookahead (Chris Wilson's method) for tight, drift-free timing

Default pitches: **D3 → C3 → Bb2** — a descending minor motif in the bass register.

## Controls

| Knob | Range | Effect |
|------|-------|--------|
| BPM | 10 – 180 | Sequence tempo (half-note steps) |
| DRIFT | 0 – 100% | Oscillator random walk depth and speed; 30% default gives subtle but perceptible instability; 100% creates clear microtonal drift |
| PITCH × 3 | C1 – C4 | Pitch for each step (chromatic, MIDI) |
| PROB × 3 | 0 – 100% | Per-step trigger probability; evaluated independently on every cycle; skipped steps show a brief dim flicker on the step LED |
| HLD/ADV × 3 | toggle | Per-step skip routing; HLD (default) drones silently on skip; ADV immediately chains to the next step's pitch in the same time slot, cascading until a step triggers or all three steps are exhausted |
| CUTOFF | 80 Hz – 8 kHz | Filter cutoff frequency (logarithmic) |
| RES | 0.5 – 20 | Filter resonance / Q |
| GLIDE TIME | 0.01s – 6s | Portamento duration between steps |
| GLIDE SHAPE | LIN – EXP | Blend from linear (constant rate, mechanical) to exponential (gravitational, organic); midpoint combines both characters |
| DRIVE | 0 – 100% | Tube saturation amount; exponential curve, always slightly warm at minimum |
| TONE | ±10 dB | High shelf post-saturation; centre position is flat |
| LFO RATE | 0.1 – 5 Hz | Filter LFO speed; default 0.30 Hz (very slow breath) |
| LFO DEPTH | 0 – 100% | Filter LFO intensity; depth is exponentially scaled so low values stay subtle |
| TAPE TIME | 25 – 800ms | Delay time; short is slapback, long is spacious echo |
| TAPE FEED | 0 – 97% | Feedback amount; high values approach self-oscillation with saturation bloom |
| TAPE WOW | 0 – 100% | Wow/flutter depth; modulates delay time via free-running LFO for pitch wobble |
| TAPE DARK | 0 – 100% | Per-repeat high frequency rolloff; early repeats bright, late repeats muffled |
| TAPE MIX | 0 – 100% | Dry/wet blend; dry signal always preserved |

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
