# Micro-8 Tracker

A pattern tracker for the **Micro-8** machine, written in **Lofi**, and its player. It replaces the old JavaScript tracker,
which is no longer maintained.

## The programs

| File | Role |
|---|---|
| `TRACKER.SRC` | **0.1, prototype**: 8 tracks, patterns of 16 to 256 steps, one pattern plays in a loop (no song), screens PATTERN, SETUP, FILE, HELP, ABOUT |
| `TRACKER2.SRC` | **0.2, the complete tracker**: song (SONG), mute/solo, MIDI input, oscilloscopes, 5 screens (PATTERN, SONG, SETUP, FILE, HELP), a grid that scrolls during playback, the author's name and the site www.bsm3d.com at the bottom of the screen. The 10 default instruments are loaded from `/tools/sounded/sounds`. Compiled on the machine: 84 % of the code area |
| `PLAYER.SRC` | the player alone, without editing: to copy into a game or a demo to replay a project |

The programs read and write the **same files**: a project made with one opens in the others, and in Micro-8 Studio.

The tracker's own demo, **SID RUSH**, is in the [`SidRush/`](../SidRush) folder.

## State on the real machine

- **Digital synth (from the FPGA)**: an envelope or a DPCM sound only restarts on a rising edge of the trigger. The three
  programs therefore do `synNoteOff` then `synNoteOn` at each note of the DRM and SYN tracks, like the DRUMBOX example of
  the SD card; without that, on the machine, a note repeated without a note off was not replayed.

- **Compilation.** TRACKER.SRC was compiled on the machine: stop at line 190, `Unsupported operand type` on `>>`,
  then the complete version (then named TRACKER3, today TRACKER2.SRC) around line 297, on `)`. Cause found: the machine's
  compiler only shifts (`<<`, `>>`) a `byte`. `v>>(b%8)` with `b` an `int` is refused; none of the 73 programs of the SD card
  shifts anything other than a `byte`. Fixed in the three programs (`byte s=(byte)(b%8); v>>s`). Micro-8 Studio now
  reports this error before the machine does. Fixed and recompiled since.
- **Line endings.** Second try: TRACKER.SRC refused at line 7 (then 9), SIDRUSH.SRC at line 8, `at ','.
  Expect expression.`, each time on the first empty line of the file. These sources had Windows line endings (CRLF); the
  73 programs of the SD card are all LF. The machine's compiler does not skip the CR (0x0D is the line feed of its screen):
  hidden in the comments, it breaks the first line that is not one. All the sources (tracker, `lib/`) were converted to LF,
  Studio always saves in LF and its analyzer refuses a CR like the machine. Lines stay under 159 characters (117 at most in
  TRACKER2). Recompiled on the machine since.
- **159-character line (2026-10-05).** The first version of the demo did not start while the old SIDRUSH did: the line of the
  `rainbow` array was exactly 159 characters, the machine's read limit (the line break probably counts, the final `;` was
  lost). Rule: **keep lines well under 159 characters (aim for 130 at most)** and split big arrays over several lines;
  Studio's analyzer (`source_rules`) only reports > 159. Arpeggio (a tick at each note, seen on the machine despite a single
  `synNoteOn` per frame): `synNoteOff(1)` at the end of the frame just before `vSync()` and a fast envelope release
  (`synEnv(1,0,5,9,15)`), so the pitch change happens with the gate closed. Same pass: the arpeggio of the old SIDRUSH went
  from `poke` of the registers to one `synNoteOn` per frame (glitch), and `newChord` no longer triggers the first note
  (`arpFrame` plays it: a single trigger).
- **Machine OS.** Third try: `getKeyDwn()` unknown at line 332 with OS 0.1.x. After updating to 0.8.0 (`flash`), stop at
  line 1561 on `Type mismatch`: `memSet(..., 384*n, ...)` with `n` an `int`, while the size is a `nat`. The machine refuses any
  API argument of a type higher than the manual's (no implicit conversion downwards). Fixed with casts in TRACKER, TRACKER2 and
  PLAYER (5 calls); Studio's analyzer now checks these types (table taken from the manual, no false positive on the 73
  programs of the card). Fixed and recompiled since.
- **`break` and `continue` leak the stack (2026-10-05).** `TRACKER2` started, drew its screen, then no longer reacted: the keys
  arrived in the console ("Error ln 1, at '-'"). Cause read in the firmware (`breakStatement`, `continueStatement`,
  `patchLoopJumps`): a `break` only emits a jump, the local variables declared in the loop are not removed from the value stack
  (256 entries). My `while(true) { ... break; }` blocks, which replaced functions, left dozens of them at each turn of the main
  loop. They were rewritten without `break` (a `done` flag), and Studio's analyzer refused a `break` or `continue` that left
  locals in a loop nested in a `while`. **Fixed in OS 0.9** (official changelog: "break/continue in loops with local variables
  corrupted the stack"): Studio's check is removed; the `done` workaround stays in TRACKER2 (it still works, no reason to rewrite
  it).
- **Code size** (estimated from the source, model fitted on the 73 `.BIN` of the SD card, within 5 %): TRACKER.SRC about
  15 KB, PLAYER.SRC about 3 KB. Measured on the machine (2026-10-05): TRACKER2.SRC takes 84 % of the 32 KB code area, TRACKER
  60 %. If `compile` refuses TRACKER2.SRC for lack of room: `data 0xC000` before compiling gives 36 KB of code (the tracker only
  uses about 2.8 KB of data). This setting stays in EEPROM; `data 0xB000` restores the default split.
- **Manual limits** (6.4.5), all far from the maximum: symbol table about 3.7 KB of 11, 16 call levels of 64, at most 20 stack
  values per function of 256.
- Checks on the PC: Micro-8 Studio's analyzer (in the editor, or `python micro8_studio/lofic.py FILE.SRC`) applies the
  machine's rules and estimates the code size.

## Files of a project

A project is **a single `NAME.M8S` file, compressed as on the C64**: the tracks are lists of events, and a track already
written elsewhere, even transposed, is only a 4-byte reference (like the transposed patterns of Future Composer).

| Part | Content |
|---|---|
| header | `M8S1` |
| song | 16 bytes (one bit per non-zero byte of the first 128 settings) then those bytes; the block numbers of the song (as many as its length); `0`, or `1` followed by the transpositions of the song |
| instruments | 2 bytes (one bit per recorded FM slot) then 54 parameters per instrument |
| patterns | for each non-empty block: its number (0-63), a byte (one bit per non-empty track), then for each non-empty track either `254`, the track already written that it repeats (block × 8 + track, 2 bytes) and the transposition (+128), or the number of events then each one: step (0-15) + 16 if the instrument changes + 32 if the velocity changes, the note, then the instrument and the velocity if they change |
| end | `255` |

Sizes: SID RUSH (17 patterns, 34 positions, 2 instruments) is **782 bytes** written by Studio, 1,220 saved by TRACKER2
(which keeps its 10 instruments); 25,500 in raw format. On the machine (the emulator's X1 mode): loading 0.14 s, saving
0.6 s (the tracker looks for repeated tracks).

- **A single file**: TRACKER, TRACKER2, PLAYER, the `song` block of the M8LIB library and Micro-8 Studio write and read
  `NAME.M8S`. The file is erased then rewritten at each save (`fOpen` does not shorten an existing file).
- **Instruments.** A slot of all zeros keeps the machine's instrument. Micro-8 Studio only rewrites the slots it knows.
- **Lengths**: bytes 84 to 91, one bit per block (1 = the block continues the previous one).
- **Muted tracks**: bytes 42 to 49 (TRACKER, TRACKER2). PLAYER respects them.
- **Damaged file**: when loading, each setting is brought back into its range (instrument 0-9, octave 0-7, volume 0-127...)
  and so is each note played: the chip never receives an out-of-range value.
- **TRACKER.SRC 0.1 has no song**: when saving, the single position of the song designates the displayed pattern. PLAYER and
  Studio therefore replay what you heard, long pattern included.

## First launch

1. Copy `TRACKER2.SRC` (or `TRACKER.SRC`) and `PLAYER.SRC` to the SD card, for example into `/TOOLS/TRACKER`.
2. On the machine: `cd /tools/tracker`, then `compile tracker2.src` and `run tracker2.bin`. The tracker loads its 10 default
   instruments from `/tools/sounded/sounds` (the machine's FM bank is empty after `ymInit`).
3. **Notes.** The real programs of the SD card (`EXAMPLES/AUDIO/SIMPLSND`, `DEMOS/ORGAN`) play A 440 Hz with **note 69** on an
   **octave-5 voice**, like a MIDI number; the tracker starts from there.
4. TRACKER2 takes 84 % of the code area (32 KB, default setting `data 0xB000`), TRACKER 60 %.
5. **FM instruments**: on SETUP, `L` loads an `.INS` into the track's slot (for example `/demos/organ/sounds/pad01.ins`), `S`
   saves the edited instrument into an `.INS`. They are also kept with the project (`NAME.M8S`).
6. To replay a project without the tracker: `compile player.src`, `run player.bin` in the folder of the `.M8S` files, then
   choose the song in the list.

## The 8 tracks

| Track | Hardware voice | Use |
|---|---|---|
| FM1 to FM6 | 6 voices of the YM2612 | melodies, basses, chords; bank of 10 instruments |
| DRM | voice 0 of the digital synth | drums (20 DPCM sounds) or oscillator (square, sawtooth, noise) |
| SYN | voice 1 of the digital synth | oscillator (or drums) |

The two synth voices share a filter, two envelopes and an LFO (settings in SETUP).

## TRACKER2.SRC (0.2)

A lighter version of the old TRACKER3 (no on-screen keyboard, no frames, short help) to fit in the code area. A single
screen: toolbar at the top (transport, BPM, octave, edit step), tracks (mute/solo, one oscilloscope per track: the waveform
alone), a central tabbed view (PATTERN, SONG, SETUP, FILE, HELP; `TAB`), status and the author's name (www.bsm3d.com) at the
bottom.

- **Removed to fit on the machine** (2026-10-03 to 05): the mouse, the on-screen keyboard, the mini song view, the envelope
  panel, the Euclidean rhythm, the ABOUT screen and the level bars next to the oscilloscopes. The machine's compiler refuses
  more than 254 functions + parameters and code over 32 KB (36 KB with `data 0xC000`).
- **Value of the cell**: `FCTN` or `CTRL` + up/down changes the note (semitone, `SHIFT`: octave), the instrument or the
  velocity under the cursor; the note is heard when stopped.
- **Repeat**: an arrow held down repeats (after 0.4 s, every 70 ms).
- The **oscilloscopes** are drawn from the notes played: the machine does not read the sound back.

### PATTERN

Grid of 8 tracks, 16 lines at a time. Cell: **note**, **instrument** (0-9), **velocity** (hexadecimal, 01 to 7F).
A pattern is 16, 64, 128 or 256 steps long (4, 8 or 16 blocks of 16 steps that follow each other).

| Key | Action |
|---|---|
| arrows | move the cursor (left/right: note, instrument, velocity, then next track) |
| `SHIFT+up` / `SHIFT+down` | previous / next page of a long pattern |
| `Z S X D C V G B H N J M` | notes C to B of the current octave |
| `Q 2 W 3 E R 5 T 6 Y 7 U I` | same notes one octave higher |
| `1` | note off |
| `Backspace` | clears the cell |
| `-` / `=` | octave -/+ (note field); velocity -/+ (velocity field, `SHIFT`: steps of 16) |
| `[` / `]` | default instrument -/+ |
| `;` / `'` | default velocity -/+ |
| `,` / `.` | previous / next pattern; while a pattern plays, it plays right away, at the same step of the bar |
| `0-9` | on the instrument field: instrument of the cell |
| `FCTN+C` / `FCTN+V` / `FCTN+K` | copy / paste / clear the pattern (all its blocks) |
| `FCTN+L` | pattern length: 16, 64, 128, 256 steps |
| `FCTN+-` / `FCTN+=` | edit step (0 to 16) |
| `FCTN+1` to `FCTN+8` | track muted / audible, right away: its current note is cut (saved in the project) |
| `FCTN+SHIFT+1` to `8` | track in solo (not saved) |
| `FCTN+F` | follow playback or not |
| `FCTN+left` / `FCTN+right` | transpose the current track of the pattern (not the drums) |

On the DRM track, the note keys choose one of the 20 drum sounds (`Z` = acoustic bass drum...). Lengthening a pattern takes the
following blocks (the tracker asks `Y`/`O` if they have notes); shortening keeps the notes of the freed blocks.

### SONG

Arrangement view: a strip of blocks, one per entry, coloured by pattern; the chosen entry highlighted, the one playing in
red.

| Key | Action |
|---|---|
| up / down | choose an entry |
| left / right | previous / next pattern |
| `-` / `=` | transposition -/+ |
| `A` | insert a copy of the entry |
| `Backspace` | delete the entry |
| `L` | song loop on/off |

### SETUP

26 settings: selected track (instrument, voice octave, pan, volume, mute), synth waveforms, envelopes, filter, LFO, **NOTE MODE**
and **NOTE OFFSET**. Up/down: choose; left/right (or `-` `=`): change (`SHIFT`: steps of 8). `L`: load an `.INS` into the
track's slot; `S`: save this instrument into an `.INS`.

### FILE

`S` saves (`NAME.M8S`, a single file), `L` loads, `R` renames the project (8 characters), `N` new.

### Common keys

| Key | Action |
|---|---|
| `Space` | play / stop the current pattern (in a loop) |
| `Enter` | play / stop the song from the chosen entry |
| `9` / `0` | tempo -/+ 1 (`SHIFT`: 10), from 40 to 255 BPM |
| `Esc` | stop |
| `FCTN+Esc` | quit |

The tempo is in quarter notes; a step is a sixteenth note (`15000 / BPM` milliseconds). **MIDI keyboard** (MIDI IN port): each note
plays on the current track; when stopped, on PATTERN, it is written under the cursor.

## TRACKER.SRC (0.1)

The prototype, deliberately reduced: no song (one pattern plays in a loop), no solo, Euclidean rhythm, MIDI input or envelope
panel. Screens PATTERN, SETUP, FILE, HELP, ABOUT, by keyboard.

Keys: those of PATTERN above without `FCTN+SHIFT+1..8`; `FCTN` or `CTRL` + up/down changes the value of the cell or of the
setting; on SETUP, `CTRL` + left/right changes the value by 2, `L` loads an `.INS`. Common: `Space`, `9`/`0`, `TAB`,
`FCTN+Esc`. Same files as 0.2, instruments included.

## PLAYER.SRC

Lists the `.M8S` songs of its folder (up/down to choose, `Enter` to play, `Esc` to go back to the list, `Esc` again to quit) and
replays the chosen one with its instruments: song with transpositions and loop, muted tracks respected, and for a song of a
single position (project 0.1) the whole pattern in a loop. During playback, right away: `1` to `8` mutes or unmutes a track
(its current note is cut), left/right jumps to the previous/next position at the same step. About forty small functions,
without editing code: to copy into a game or a demo.

## Correspondence with the JavaScript tracker

| JS | Micro-8 |
|---|---|
| 4 voices (lead, bass, arpeggio, drums) | 8 tracks (6 FM + 2 synth) |
| ADSR per voice, waveforms | FM instruments (YM2612) and synth envelopes |
| filter per voice, pan, volume | pan and volume per track; one shared filter for the synth |
| delay, saturation, compressor, bitcrusher | **not carried over** (the hardware has no such effects) |
| WAV / stems export | in Micro-8 Studio |
| arrangement, transposition per pattern | SONG screen |

## To check at first launch

1. **Compilation** of TRACKER2: modified on 2026-11-09 (`FCTN+1..8` mute and `FCTN+SHIFT+1..8` solo restored inline, without an
   extra function; TRACKER2 is at 253 functions+parameters of 254). Measured at 84 % of the code area on 2026-10-05, before
   this change: recompile.
2. **`_fError` after `ymLoad`/`ymSave`/`import`**: assumed updated like the other file functions.
3. **`ymInit`**: the project's instruments are loaded after `ymInit`, in case it resets the bank to zero.
4. **Key codes** (appendix B.2): does `FCTN`+digit really give the digit's code (mute and solo of TRACKER2, only checked in the
   emulator)?
5. **VM speed**: at a very high tempo, playback could skip steps. Micro-8 Studio has a "Hardware Speed (X1)" mode to get an
   idea; to calibrate with `micro8_studio/dev/X1BENCH.SRC`.
6. **Pattern memory area** (page `0x90`, from `0x24000` to `0x2B800` with the clipboard): to avoid with a double-buffered
   bitmap.

## Possible next steps

- Effects per cell (arpeggio, glissando, cut): this needs one more file and a finer playback engine; TRACKER2 only has 16 % of
  the code area left as margin (more with `data 0xC000`). Micro-8 Studio already has them ("Studio FX"), in Studio only.
- Visualising the sound with the ADC (`adcSampleL`): the emulator does not simulate the ADC, to try on the machine.
