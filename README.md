# Cessna AWG

A drawable wavetable oscillator plugin for the [Expert Sleepers Disting NT](https://expert-sleepers.co.uk/distingNT.html), inspired by James Cessna's 1965 Arbitrary Waveform Generator — one of the earliest known digital synthesizers, built at the University of Iowa under physicist James Van Allen.

Like the original, the Cessna AWG divides a waveform cycle into discrete amplitude segments and interpolates them to create a linear or curved waveform. Unlike the original, it adds internal modulation engines that animate the waveform's harmonic content in real time.

## Installation

1. Download `cessna_awg.o` from the latest [release](../../releases/latest).
2. Copy it into the `programs/plug-ins/` directory on your Disting NT SD card.
3. Power on the Disting NT — Cessna AWG will appear in the algorithm list.

## Parameters

Parameters are organised into pages on the Disting NT display.

### Routing

| Parameter    | Range    | Default | Description                          |
|--------------|----------|---------|--------------------------------------|
| CV In        | Bus 0-13 | Off     | Pitch CV input (1V/oct, 0V = C4)     |
| FM In        | Bus 0-13 | Off     | Linear FM input                      |
| Out          | Bus 1-13 | 1       | Audio output bus                     |
| Out Mode     | Replace/Mix | Replace | Replace or mix into the output bus |

### Tune

| Parameter  | Range        | Default | Description                        |
|------------|--------------|---------|------------------------------------|
| Octave     | -4 to +4     | 0       | Octave shift                       |
| Transpose  | -24 to +24   | 0       | Semitone transpose                 |
| Fine       | -100 to +100 | 0       | Fine tuning in cents               |

### Main

| Parameter   | Range    | Default | Description                                              |
|-------------|----------|---------|----------------------------------------------------------|
| Level       | 0-100%   | 50%     | Output amplitude. Reduce to create headroom for modulation. |
| Pre-smooth  | 0-100%   | 0%      | Hermite interpolation on the base waveform before modulation. 0% = angular, 100% = curved. |
| Post-smooth | 0-100%   | 100%    | Hermite interpolation on the final output after modulation. |
| Stepped     | Off/On   | Off     | Removes all interpolation — reproduces Cessna's original DAC staircase output. |

### Waveform

16 bins (Bin 1–16), each settable from -100% to +100%. Together they define the shape of one complete waveform cycle. Map these to a 16-fader controller bank for direct hands-on waveform drawing.

### Mod

| Parameter | Range                    | Default | Description               |
|-----------|--------------------------|---------|---------------------------|
| Mod Mode  | Off / Ripple / Kink / Scramble | Off | Selects the internal modulation engine. |

### Ripple

A sine wave that travels continuously across all 16 bins, creating gentle periodic animation of the harmonic content.

| Parameter | Range    | Default | Description                    |
|-----------|----------|---------|--------------------------------|
| Amt       | 0-100%   | 50%     | Modulation depth               |
| Rate      | 1-1000   | 50      | Travel speed                   |

### Kink

A localised perturbation — inspired by soliton wave physics — that travels through the waveform. Two independent kinks (K1 and K2) can collide and interact.

| Parameter  | Range                              | Default   | Description                                           |
|------------|------------------------------------|-----------|-------------------------------------------------------|
| K1 Amt     | 0-100%                             | 50%       | Kink 1 amplitude                                      |
| K1 Rate    | 1-32                               | 16        | Kink 1 travel speed                                   |
| Shape      | Triangle / Pulse                   | Triangle  | Kink envelope shape                                   |
| Width      | 1-16                               | 2         | Number of bins the kink spans                         |
| Range      | 1-16                               | 16        | How far the kink travels before looping               |
| Direction  | Forward / Backward / Alternating / Random | Forward | Kink travel direction                          |
| Slew       | 0-100%                             | 0%        | Smooths kink position movement between steps          |
| Angular    | Off/On                             | Off       | Steps the kink shape only, leaving the base waveform smoothly interpolated |
| K2 Amt     | 0-100%                             | 0%        | Kink 2 amplitude (0% = inactive)                      |
| K2 Rate    | 1-32                               | 12        | Kink 2 travel speed                                   |

K2 starts at the opposite end of Range from K1, so they naturally travel toward each other and collide when both are active.

### Scramble

Randomly reorders adjacent bin pairs on each step, continuously shuffling the waveform's harmonic topology.

| Parameter | Range  | Default | Description                              |
|-----------|--------|---------|------------------------------------------|
| Amt       | 0-100% | 50%     | Scramble depth                           |
| Rate      | 1-32   | 16      | Scramble step rate                       |
| Slew      | 0-100% | 0%      | Crossfades between scramble states rather than jumping abruptly |

### Morph

Store two waveform snapshots (A and B) and crossfade between them. CV-control the Morph parameter to use Cessna AWG as a real-time wavetable scanner.

| Parameter | Range  | Default | Description                                        |
|-----------|--------|---------|----------------------------------------------------|
| Save A    | Off/On | Off     | When On, locks slot A to the current waveform      |
| Save B    | Off/On | Off     | When On, locks slot B to the current waveform      |
| Morph     | 0-100% | 0%      | Crossfades between slot A (0%) and slot B (100%)   |

## Historical Note

James Cessna built the original Arbitrary Waveform Generator in 1965 as a master's thesis project at the University of Iowa, supervised by James Van Allen — the physicist who discovered the Van Allen radiation belts. Cessna's instrument divided a waveform cycle into 96 time segments, each with an amplitude set by a physical potentiometer. An analogue frequency multiplier locked the playback rate to an external audio source, making it a real-time waveform converter.

For more information about the instrument, and its connection to the NASA satellite program, see the video "Lost Signal" on the [Electrum Modular YouTube channel](https://www.youtube.com/@electrummodularmusic).

## Building from Source

Requirements:
- `arm-none-eabi-g++` (ARM GCC toolchain)
- The [Disting NT API](https://github.com/expertsleepersltd/distingNT_API) (included as `distingNT_API/`)

```bash
make    # builds cessna_awg.o
```

## License

MIT — see [LICENSE](LICENSE).

## Author

Nick Yablon (aka Electrum Modular)
