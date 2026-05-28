# Y — Triangle Synth Engine

**Y** is a browser-based synthesizer built around a trilinear morphing system between three iconic synthesis architectures. No install, no plugin — open the HTML file and play.

---

## Concept

At the heart of Y is a triangle with a draggable joystick. Each corner represents a synthesis engine:

- **YOOG** — Warm monophonic analog (Minimoog-inspired). Sawtooth + sub-octave, Moog Ladder filter (Huovilainen 4-pole), key tracking, drive.
- **YUNO** — Polyphonic chorus machine (Juno-inspired). Dual DCO with random per-note drift, analog LFO instability, BBD stereo chorus.
- **YX7** — 4-operator FM digital (DX7-inspired). Correct FM index (dimensionless), per-operator envelopes, OP1 feedback, 8 configurable algorithms.

Drag the joystick and the three engines blend proportionally using **barycentric coordinates**. The timbre, polyphony, and number of active FM operators all morph in real time.

---

## Features

### Synthesis
- **3 engines** running simultaneously, mixed by barycentric weight
- **Morph polyphony** — from 1 voice (full Yoog) to 16 voices (full YX7), with Juno's 6-voice character in between
- **Morph FM operators** — FM synthesis activates progressively as you approach YX7 (0 ops < 25%, 2 ops 25–50%, 3 ops 50–75%, 4 ops > 75%)
- **Moog Ladder filter** via AudioWorklet (Huovilainen model) with key tracking and filter envelope
- **BBD Juno chorus** — dual delay 7ms/11ms, LFO 180° phase offset, 3.2kHz presence peak

### Interface
- **Unified parameter panel** — OSC, Filter, ADSR × 2, Volume
- **Contextual zone** — per-engine parameters (SUB/DRIVE for Yoog, PWM/CHORUS for Yuno, ALGO/RATIO/FEEDBACK for YX7), opacity tied to engine weight
- **Knob interaction** — drag vertical (Shift = fine), double-click = reset to center
- **ADSR sliders** — ENV1 (filter), ENV2 (amplitude)

### Modulation
- **2 LFOs** — sine / triangle / saw / square / S&H, rate knob, 2 targets each
- **2 Envelopes** — 2 targets each
- **Target dropdown** — NONE / PITCH / CUTOFF / RESO / AMP / FM INDEX / MORPH X / MORPH Y / CHORUS
- **MORPH X/Y** as LFO destination — the joystick moves automatically

### Effects (3 pedals)
- **Delay** — WARM TAPE (BBD analog) / MEMORY (modulated) / DIGITAL DLY
- **Chorus** — BBD / BLUE / SMALL — global post-signal, independent from Yuno's internal chorus
- **Reverb** — SPRING / PLATE / INFINITE
- **FX Global** knob — scales all effect mix simultaneously

### Arpeggiator / Sequencer
- **ARP** — UP / DOWN / UD / RANDOM, 1–3 octaves, 1/4 to 1/32 rate, multi-note LATCH
- **SEQ** — 16 steps, 4×4 grid, independent melodic sequencer
  - Click = toggle on/off + keyboard/MIDI note assignment
  - Double-click = note dropdown (C2–C6)
  - Right-click = gate cycle (NORMAL / LONG / SHORT)
  - Drag = velocity
  - LEARN mode — play notes to fill steps in order
- **BPM** — drag to set (40–240), shared clock for ARP and SEQ
- **Metronome** — warm sine tone, accented beat 1

### Presets
20 curated presets covering all zones of the triangle:

| Zone | Presets |
|------|---------|
| Yoog | BASS INIT, DEEP BASS, BASS LEAD, ACID LINE |
| Yuno | POLY INIT, AURORA, SILK STRINGS |
| YX7  | FM INIT, FM BELLS, FM STAB, GHOST KEYS |
| Hybrid | HYBRID PAD, WARM DIGITAL, METAL STRINGS, BASS FM, VELVET NIGHT, BRASS STAB, TRON PAD, DREAMER, THREE WORLDS |

---

## Usage

1. Open `Y_synth_v0.2_final.html` in any modern browser (Chrome recommended for AudioWorklet support)
2. Click any key on the mini-keyboard to initialize the audio context
3. Drag the joystick to morph between engines
4. Connect a MIDI controller — notes and pitch bend are supported automatically

### Tips
- **Shift + drag** a knob for fine adjustment
- **Double-click** a knob to reset to center value
- **Right-click** a SEQ step to cycle gate (NORMAL → LONG → SHORT)
- The joystick can be automated by routing a LFO to **MORPH X** or **MORPH Y**
- In SEQ mode, click a step then play a note on the keyboard to assign it
- OCT −/+ buttons on the keyboard panel shift the playable range

---

## Architecture

```
OSC (Yoog: SAW×2 + SUB | Yuno: DCO×2 + drift | YX7: FM 4-op)
    ↓
FILTER (Ladder 24dB | BPF Juno | bypass)
    ↓
VCA (ADSR envelope)
    ↓
MORPH BUS (barycentric gain × 3)
    ↓
DELAY → CHORUS BBD → REVERB
    ↓
MASTER
```

### Barycentric morphing

The joystick position defines three weights `(wYoog, wYuno, wYX7)` with `wYoog + wYuno + wYX7 = 1`. Each engine plays into its own gain node weighted by its coordinate. Parameters, polyphony, and FM operator count all interpolate from these weights.

---

## Technical

- **Pure Web Audio API** — no dependencies, no framework
- **AudioWorklet** — Moog Ladder filter (Huovilainen model) runs in a dedicated audio thread
- **Single HTML file** — everything embedded, works offline
- **MIDI Web API** — automatic detection, all connected inputs
- Tested on Chrome 120+, Firefox 121+

---

## Roadmap

- [ ] Chorus pedal fully wired into audio chain
- [ ] Improved reverb IR (less synthetic)
- [ ] Preset calibration at audio level
- [ ] Modular synth integration (Eurorack CV via Web MIDI)
- [ ] Preset save/load (localStorage)
- [ ] Touch / mobile optimization

---

## Credits

A Positve Lofi project by Yuwa  
Engine concepts inspired by Moog, Roland, and Yamaha synthesis architectures  
Built with Web Audio API — no frameworks, no dependencies

---

*Y is not affiliated with or endorsed by Moog Music, Roland Corporation, or Yamaha Corporation.*
