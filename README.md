# Chord to Riff

**Interactive piano roll that shows how plain block chords turn into a film-score style riff, one step at a time.**

Beatmakers often know how to build a chord on the white keys, yet film and game soundtracks still sound unreachable. The point of this tool: the chords are usually the same. What changes is *how the notes sit in the piano roll*. Chord to Riff makes that visible and audible.

Single HTML file. No build step, no dependencies, no server. Open `index.html` in a browser.

## What it does

- **Eight toggleable steps** from "whole-note block chords, flat velocity" to a riff: registers, one borrowed note (harmonic minor), broken chords, velocity accents, anticipation, motif vs. wandering melody, bass octaves. Each step can be switched on and off; hold the A/B button to hear the previous step.
- **Chord detection with Roman numerals.** Every half bar is labelled with the chord name (`Am`, `E/G♯`, `D/F♯`), the Roman numeral with inversion (`i`, `V⁶`, `IV⁶`) and its function colour: tonic, subdominant, dominant. Click a chord to hear it in isolation, double-click to correct it by hand.
- **Note roles.** Filled notes are chord tones. Outlined notes are non-chord tones: passing, neighbour or suspension, with a dotted line to the note they resolve to. Notes outside the key get a bright frame. Hover any note for a plain-language explanation ("suspension on a strong beat over Am, resolves to E5").
- **Key-aware keyboard.** Every row is labelled with its note name; scale notes are brighter. Switch to scale degrees (1, ♭3, 5, 7) to think in the key instead of in letters.
- **MIDI import.** Load any `.mid` file: it is split into bass, chord and melody, the key and tempo are detected, and the same eight steps are rebuilt *from your file*, so step 8 is the original and the lower steps are the same track reduced to block chords.
- **Piano sound in the browser** via Web Audio: bright inharmonic partials, hammer noise, a short room and a compressor.

## Try it

```bash
git clone https://github.com/delawer33/chord-to-riff.git
cd chord-to-riff
xdg-open index.html   # or just double-click the file
```

A demo MIDI with a chromatic bass line, anticipations and octave doublings is in [`examples/demo-riff.mid`](examples/demo-riff.mid). Load it with **Load MIDI** to see the analysis on a file instead of the built-in etude.

## How the analysis works

- **Key**: Krumhansl-style pitch-class profile correlation, with extra votes from the lowest note near each downbeat (the first bar counts double).
- **Chords**: per half bar, every triad, seventh and sus template is scored against onset-weighted pitch classes. The bass mostly decides the slash, not the chord; a seventh only counts if it sounds above the bass; an accompaniment that spells a complete triad gets a bonus. Anticipated notes are assigned to the window they belong to.
- **Voices** (MIDI import): skyline heuristic. The highest sounding note is melody, the lowest is bass (capped one octave above the lowest note in the file), octave doublings follow the bass, everything else is chord.
- **Note roles**: chord tone if the pitch class is in the current chord; otherwise passing, neighbour or suspension depending on the surrounding melody notes and beat strength. An augmented second into a chord tone counts as a step.

All of this is heuristic. When it gets a chord wrong, double-click the label and type the right one; corrections are kept in the browser.

## Roadmap

- "Harmony" tab: constructive steps from bare Roman numerals to the finished riff (inversions, borrowed notes, bass line, chord-tone melody, decorations).
- Per-bar constructor: the simplest white-key version of a bar next to the original, with each transformation listed and toggleable.
- Automatic detection of devices: chromatic bass lines, pedal points, sequences, common-tone voice leading.

## License

MIT
