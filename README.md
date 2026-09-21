# Chord to Riff

**See how plain block chords turn into a film-score style riff, one step at a time.**

Interactive piano roll with chord detection, Roman numerals and MIDI import.
One HTML file. No build, no dependencies, no server.

![Chord to Riff: eight toggleable steps, chord labels with Roman numerals, note roles on a piano roll](docs/screenshot.png)

## Why

You know the chords. You can build them on the white keys.
Still, film and game soundtracks sound out of reach.

The chords are usually the same.
What changes is *where the notes sit* in the piano roll.
This tool makes that visible and audible.

## Features

**Eight steps, each one a switch**

1. Blocks: whole-note chords, flat velocity
2. Registers: bass down, melody up
3. One black key: harmonic minor, chromatic bass
4. Break the block: no vertical stacks
5. Velocity: accents on the "and"
6. Anticipation: chord a sixteenth early
7. Motif, not a stroll: one cell, then copies
8. Bass octaves: double at +12

Turn any step on or off. Hold **A/B** to hear the previous step.

**Chord labels**

- Every half bar gets a chord name: `Am`, `E/G♯`, `D/F♯`
- Roman numeral with inversion: `i`, `V⁶`, `IV⁶`
- Colour by function: tonic, subdominant, dominant
- Click a chord to hear it alone
- Double-click to correct it by hand

**Note roles**

- Filled note: chord tone
- Outlined note: passing, neighbour or suspension
- Dotted line: where a non-chord tone resolves
- Bright frame: note outside the key
- Hover any note for a plain-language explanation

**Key-aware keyboard**

- Every row has its note name
- Scale notes are brighter
- Switch to scale degrees: `1`, `♭3`, `5`, `7`

**MIDI import**

- Load any `.mid` file
- Key and tempo are detected
- Notes are split into bass, chord and melody
- Step 8 is your original
- Lower steps are the same track reduced to block chords

**Piano sound in the browser**

Web Audio synth: inharmonic partials, hammer noise, a short room, a compressor.

## Try it

```bash
git clone https://github.com/delawer33/chord-to-riff.git
cd chord-to-riff
xdg-open index.html
```

Or just double-click `index.html`.

There is a demo file in [`examples/demo-riff.mid`](examples/demo-riff.mid).
It has a chromatic bass line, anticipations and octave doublings.
Press **Load MIDI** and pick it.

## How the analysis works

**Key**

- Krumhansl-style pitch-class profile
- Extra votes from the lowest note near each downbeat
- First bar counts double

**Chords**

- One window per half bar
- Every triad, seventh and sus template gets a score
- Score is based on note onsets, weighted by voice
- The bass mostly decides the slash, not the chord
- A seventh only counts if it sounds above the bass
- An accompaniment that spells a full triad gets a bonus
- Anticipated notes go to the window they belong to

**Voices (MIDI import)**

- Highest sounding note: melody
- Lowest sounding note: bass
- Octave doublings follow the bass
- Everything else: chord

**Note roles**

- In the current chord: chord tone
- Otherwise: passing, neighbour or suspension
- Decided by the surrounding notes and beat strength
- An augmented second into a chord tone counts as a step

All of this is heuristic.
If a chord is wrong, double-click the label and type the right one.
Corrections are saved in the browser.

## Roadmap

- **Harmony tab.** From bare Roman numerals to the finished riff: inversions, borrowed notes, bass line, chord-tone melody, decorations.
- **Per-bar constructor.** The simplest white-key version of a bar next to the original. Each transformation listed and toggleable.
- **Device detection.** Chromatic bass lines, pedal points, sequences, common-tone voice leading.

## License

MIT
