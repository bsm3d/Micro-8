# Micro-8 Tracker

A pattern tracker for the **Micro-8** machine, written in **Lofi**, and its player. It replaces the old
JavaScript tracker, which is no longer maintained.

## The programs

| File | Role |
|---|---|
| `TRACKER.SRC` | **0.1, prototype**: 8 tracks, patterns of 16 to 256 steps, one pattern loops (no song), PATTERN, SETUP, FILE, HELP, ABOUT screens |
| `TRACKER2.SRC` | **0.2, the full tracker**: song (SONG), mute/solo, MIDI input, oscilloscopes, 5 screens (PATTERN, SONG, SETUP, FILE, HELP), a grid that scrolls during playback, the author's name and www.bsm3d.com at the bottom of the screen. The 10 default instruments are loaded from `/tools/sounded/sounds`. Compiled on the machine: 84% of the code area |
| `PLAYER.SRC` | the player alone, no editing: drop it into a game or a demo to replay a project |
| `SIDRUSH.SRC` | **SID RUSH**: an original C64-style music demo and cracktro (galloping FM bass, frame-rate arpeggios, a vibrato melody, DPCM drums, a "copper" raster bar, a rainbow title, parallax stars, "Code, Design, Music" credits). Arpeggio: one `synNoteOn` a frame (the firmware writes both frequency bytes at once), with no register `poke` |
| `SIDRUSH.M8S` (+ `.EXT.json`, `.NOTES.json` for Studio) | SID RUSH's music as a tracker project, a single file to open in Micro-8 Studio's tracker, TRACKER2 (FILE: `R` SID RUSH, `L`) or PLAYER (pick it from its list): FM bass (BASS01), FM melody (LEAD02), DRM drums, SYN arpeggio, one pattern a bar (17), 34 positions, tempo 150. The machine plays the arpeggio one note a step; with Studio's FX active (`Axy` codes and `Vxy` vibrato, read only by Studio), Studio replays the demo's fast arpeggio. Generated from the same data as the demo |

The programs read and write the **same files**: a project made with one opens in the others, and in Micro-8
Studio.

## Status on real hardware

- **Digital synth (per the FPGA)**: an envelope or a DPCM sound only restarts on a rising edge of the trigger.
  All three programs therefore do `synNoteOff` then `synNoteOn` on every note of the DRM and SYN tracks, like
  the SD card's DRUMBOX example; without that, on the machine, a repeated note with no note-off did not replay.

- **Compiling.** TRACKER.SRC was compiled on the machine: stopped at line 190, `Unsupported operand type` on
  `>>`, then the full version (then named TRACKER3, now TRACKER2.SRC) around line 297, on `)`. Cause found: the
  machine's compiler only shifts (`<<`, `>>`) by a `byte`. `v>>(b%8)` with `b` as an `int` is refused; none of
  the SD card's 73 programs shift by anything but a `byte`. Fixed in all three programs
  (`byte s=(byte)(b%8); v>>s`). Micro-8 Studio now flags this error before the machine does. Fixed and
  recompiled since.
- **Line endings.** Second attempt: TRACKER.SRC refused at line 7 (then 9), SIDRUSH.SRC at line 8, `at ','.
  Expect expression.`, each time on the file's first blank line. These sources had Windows line endings (CRLF);
  all 73 of the SD card's programs are LF only. The machine's compiler does not skip the CR (0x0D is its
  screen's own line break): hidden inside comments, it breaks the first line that isn't one. All sources
  (tracker, `lib/`) were switched to LF, Studio always saves as LF, and its analyzer rejects a CR the same way
  the machine does. Lines stay under 159 characters (117 at most in TRACKER2). Recompiled on the machine since.
- **The 159-character line (2026-10-05).** The demo's first version would not start although the older SIDRUSH
  did: the `rainbow` array's line was exactly 159 characters, the machine's reading limit (the line break
  probably counts, and the trailing `;` was lost). Rule: **keep lines well under 159 characters (aim for 130 at
  most)** and split large arrays over several lines; Studio's analyzer (`source_rules`) only flags > 159.
  Arpeggio (a tick on every note, observed on the machine despite a single `synNoteOn` a frame): `synNoteOff(1)`
  at the end of the frame just before `vSync()`, and a fast envelope release (`synEnv(1,0,5,9,15)`), so the
  pitch change happens with the gate closed. Same pass: the older SIDRUSH's arpeggio moved from `poke`-ing
  registers to one `synNoteOn` a frame (no more glitch), and `newChord` no longer re-triggers the first note
  (`arpFrame` plays it: a single trigger).
- **The machine's OS.** Third attempt: `getKeyDwn()` unknown at line 332 with OS 0.1.x. After updating to 0.8.0
  (`flash`), stopped at line 1561 on `Type mismatch`: `memSet(..., 384*n, ...)` with `n` as an `int`, while the
  size is a `nat`. The machine refuses any API argument of a wider type than the manual's (no implicit narrowing
  conversion). Fixed with casts in TRACKER, TRACKER2 and PLAYER (5 calls); Studio's analyzer now checks these
  types (a table drawn from the manual, no false positive on the SD card's 73 programs). Fixed and recompiled
  since.
- **`break` and `continue` leak the stack (2026-10-05).** TRACKER2 would start, draw its screen, then stop
  reacting: keys landed in the console ("Error ln 1, at '-'"). Cause found by reading the firmware
  (`breakStatement`, `continueStatement`, `patchLoopJumps`): a `break` only emits a jump, the local variables
  declared inside the loop are not popped off the value stack (256 entries). My `while(true) { ... break; }`
  blocks, used in place of functions, left dozens of them on every pass of the main loop. They were rewritten
  without `break` (a `done` flag), and Studio's analyzer now rejects a `break` or `continue` that leaves locals
  behind inside a loop nested in a `while`.
- **Code size** (estimated from source, a model fitted on the SD card's 73 `.BIN` files, within 5%): TRACKER.SRC
  about 15 KB, PLAYER.SRC about 3 KB. Measured on the machine (2026-10-05): TRACKER2.SRC takes up 84% of the
  32 KB code area, TRACKER 60%. If `compile` refuses TRACKER2.SRC for lack of room: `data 0xC000` before
  compiling gives 36 KB of code (the tracker only uses about 2.8 KB of data). This setting stays in EEPROM;
  `data 0xB000` restores the default split.
- **Manual limits** (6.4.5), all far from the maximum: symbol table about 3.7 KB out of 11, 16 call levels out
  of 64, 20 stack values at most per function out of 256.
- Checks on the PC: the analyzer built into Micro-8 Studio's editor applies the machine's rules and estimates
  the code size.

## Files of a project

A project is **a single `NAME.M8S` file, compressed the C64 way**: tracks are event lists, and a track already
written elsewhere, even transposed, is just a 4-byte reference (like Future Composer's transposed patterns).

| Part | Content |
|---|---|
| header | `M8S1` |
| song | 16 bytes (one bit per non-zero byte of the first 128 settings) then those bytes; the song's block numbers (as many as its length); `0`, or `1` followed by the song's transpositions |
| instruments | 2 bytes (one bit per recorded FM slot) then 54 parameters per instrument |
| patterns | for each non-empty block: its number (0-63), one byte (one bit per non-empty track), then for each non-empty track either `254`, the already-written track it repeats (block x 8 + track, 2 bytes) and the transposition (+128), or the event count followed by each one: step (0-15) + 16 if the instrument changes + 32 if the velocity changes, the note, then the instrument and velocity if they change |
| end | `255` |

Sizes: SID RUSH (17 patterns, 34 positions, 2 instruments) is **782 bytes** written by Studio, 1220 saved by
TRACKER2 (which keeps its 10 instruments); 25500 in raw format. On the machine (the emulator's X1 mode):
loading 0.14 s, saving 0.6 s (the tracker looks for repeated tracks).

- **A single file**: TRACKER, TRACKER2, PLAYER, the M8LIB library's `song` block and Micro-8 Studio all read and
  write `NAME.M8S`. The file is erased then rewritten on every save (`fOpen` does not truncate an existing
  file).
- **Instruments.** An all-zero slot keeps the machine's own instrument. Micro-8 Studio only rewrites the slots
  it knows.
- **Lengths**: bytes 84 to 91, one bit per block (1 = the block extends the previous one).
- **Muted tracks**: bytes 42 to 49 (TRACKER, TRACKER2). PLAYER honors them.
- **A damaged file**: on load, every setting is clamped into its range (instrument 0-9, octave 0-7, volume
  0-127...) and so is every note played: the chip never receives an out-of-range value.
- **TRACKER.SRC 0.1 has no song**: on save, the song's single position names the displayed pattern. PLAYER and
  Studio therefore replay what you were hearing, long patterns included.

## First run

1. Copy `TRACKER2.SRC` (or `TRACKER.SRC`) and `PLAYER.SRC` to the SD card, for example into `/TOOLS/TRACKER`.
2. On the machine: `cd /tools/tracker`, then `compile tracker2.src` and `run tracker2.bin`. The tracker loads
   its 10 default instruments from `/tools/sounded/sounds` (the machine's FM bank is empty after `ymInit`).
3. **Notes.** The SD card's real programs (`EXAMPLES/AUDIO/SIMPLSND`, `DEMOS/ORGAN`) play A440 with **note 69**
   on an **octave-5 voice**, like a MIDI number; the tracker starts from there.
4. TRACKER2 takes up 84% of the code area (32 KB, default setting `data 0xB000`), TRACKER 60%.
5. **FM instruments**: on SETUP, `L` loads an `.INS` into the track's slot (for example
   `/demos/organ/sounds/pad01.ins`), `S` saves the edited instrument to an `.INS`. They are also kept with the
   project (`NAME.M8S`).
6. To replay a project without the tracker: `compile player.src`, `run player.bin` in the `.M8S` files' folder,
   then pick the song from the list.

## The 8 tracks

| Track | Hardware voice | Use |
|---|---|---|
| FM1 to FM6 | the YM2612's 6 voices | melodies, basses, chords; a bank of 10 instruments |
| DRM | digital synth voice 0 | drums (20 DPCM sounds) or an oscillator (square, sawtooth, noise) |
| SYN | digital synth voice 1 | an oscillator (or drums) |

The synth's two voices share a filter, two envelopes and an LFO (settings on SETUP).

## TRACKER2.SRC (0.2)

A lighter version of the old TRACKER3 (no on-screen keyboard, no frames, a short help) to fit the code area. A
single screen: a toolbar at the top (transport, BPM, octave, edit step), tracks (mute/solo, a per-track
oscilloscope: the waveform only), a tabbed center view (PATTERN, SONG, SETUP, FILE, HELP; `TAB`), status and the
author's name (www.bsm3d.com) at the bottom.

- **Removed to fit the machine** (2026-10-03 to 05): the mouse, the on-screen keyboard, the song mini-view, the
  envelope panel, the Euclidean rhythm, the ABOUT screen and the level bars next to the oscilloscopes. The
  machine's compiler refuses more than 254 functions + parameters and more than 32 KB of code (36 KB with
  `data 0xC000`).
- **Cell value**: `FCTN` or `CTRL` + up/down changes the note (a semitone, `SHIFT`: an octave), the instrument
  or the velocity under the cursor; the note is heard while stopped.
- **Repeat**: a held arrow key repeats (after 0.4 s, every 70 ms).
- The **oscilloscopes** are drawn from the notes played: the machine does not read the sound back.

### PATTERN

An 8-track grid, 16 rows at a time. Cell: **note**, **instrument** (0-9), **velocity** (hex, 01 to 7F). A
pattern is 16, 64, 128 or 256 steps (4, 8 or 16 consecutive 16-step blocks).

| Key | Action |
|---|---|
| arrows | move the cursor (left/right: note, instrument, velocity, then next track) |
| `SHIFT+up` / `SHIFT+down` | previous / next page of a long pattern |
| `Z S X D C V G B H N J M` | notes C to B of the current octave |
| `Q 2 W 3 E R 5 T 6 Y 7 U I` | the same notes one octave up |
| `1` | note off |
| `Backspace` | clear the cell |
| `-` / `=` | octave -/+ (note field); velocity -/+ (velocity field, `SHIFT`: steps of 16) |
| `[` / `]` | default instrument -/+ |
| `;` / `'` | default velocity -/+ |
| `,` / `.` | previous / next pattern; while a pattern plays, it starts at once, at the same step of the bar |
| `0-9` | on the instrument field: the cell's instrument |
| `FCTN+C` / `FCTN+V` / `FCTN+K` | copy / paste / clear the pattern (all its blocks) |
| `FCTN+L` | pattern length: 16, 64, 128, 256 steps |
| `FCTN+-` / `FCTN+=` | edit step (0 to 16) |
| `FCTN+1` to `FCTN+8` | mute / unmute a track, at once: its current note is cut (saved in the project) |
| `FCTN+SHIFT+1` to `8` | solo a track (not saved) |
| `FCTN+F` | follow playback or not |
| `FCTN+left` / `FCTN+right` | transpose the pattern's current track (not the drums) |

On the DRM track, the note keys pick one of the 20 drum sounds (`Z` = acoustic bass drum...). Lengthening a
pattern claims the following blocks (the tracker asks `Y`/`O` if they hold notes); shortening keeps the notes of
the released blocks.

### SONG

Arrangement view: a strip of blocks, one per entry, colored by pattern; the selected entry highlighted, the one
playing in red.

| Key | Action |
|---|---|
| up / down | pick an entry |
| left / right | previous / next pattern |
| `-` / `=` | transposition -/+ |
| `A` | insert a copy of the entry |
| `Backspace` | delete the entry |
| `L` | song loop on/off |

### SETUP

26 settings: the selected track (instrument, voice octave, pan, volume, mute), the synth's waveforms, envelopes,
filter, LFO, **NOTE MODE** and **NOTE OFFSET**. Up/down: pick; left/right (or `-` `=`): change (`SHIFT`: steps
of 8). `L`: load an `.INS` into the track's slot; `S`: save that instrument to an `.INS`.

### FILE

`S` saves (`NAME.M8S`, a single file), `L` loads, `R` renames the project (8 characters), `N` new.

### Common keys

| Key | Action |
|---|---|
| `Space` | play / stop the current pattern (looped) |
| `Enter` | play / stop the song from the selected entry |
| `9` / `0` | tempo -/+ 1 (`SHIFT`: 10), 40 to 255 BPM |
| `Esc` | stop |
| `FCTN+Esc` | quit |

Tempo is in quarter notes; a step is an eighth note (`15000 / BPM` milliseconds). **MIDI keyboard** (MIDI IN
port): every note plays on the current track; while stopped, on PATTERN, it is written under the cursor.

## TRACKER.SRC (0.1)

The prototype, deliberately reduced: no song (one pattern loops), no solo, no Euclidean rhythm, no MIDI input
and no envelope panel. PATTERN, SETUP, FILE, HELP, ABOUT screens via the keyboard.

Keys: the same as PATTERN above without `FCTN+SHIFT+1..8`; `FCTN` or `CTRL` + up/down changes the cell's or the
setting's value; on SETUP, `CTRL` + left/right changes the value by 2, `L` loads an `.INS`. Common: `Space`,
`9`/`0`, `TAB`, `FCTN+Esc`. Same files as 0.2, instruments included.

## PLAYER.SRC

Lists the `.M8S` songs in its folder (up/down to pick, `Enter` to play, `Esc` to return to the list, `Esc` again
to quit) and replays the chosen one with its instruments: song with transpositions and loop, muted tracks
honored, and for a single-position song (a 0.1 project) the whole pattern loops. During playback, at once: `1`
to `8` mutes or unmutes a track (its current note is cut), left/right jumps to the previous/next position at
the same step. About forty small functions, with no editing code: drop it into a game or a demo.

## Comparison with the JavaScript tracker

| JS | Micro-8 |
|---|---|
| 4 voices (lead, bass, arpeggio, drums) | 8 tracks (6 FM + 2 synth) |
| ADSR per voice, waveforms | FM instruments (YM2612) and synth envelopes |
| per-voice filter, pan, volume | per-track pan and volume; a shared filter for the synth |
| delay, saturation, compressor, bitcrusher | **not carried over** (the hardware has no such effects) |
| WAV / stems export | in Micro-8 Studio |
| arrangement, per-pattern transposition | the SONG screen |

## To check on first run

1. **Compiling** TRACKER2: changed on 2026-11-09 (`FCTN+1..8` mute and `FCTN+SHIFT+1..8` solo restored inline,
   with no extra function; TRACKER2 is at 253 functions+parameters out of 254). Measured at 84% of the code
   area on 2026-10-05, before this change: needs recompiling.
2. **`_fError` after `ymLoad`/`ymSave`/`import`**: assumed to be updated like the other file functions.
3. **`ymInit`**: the project's instruments are loaded after `ymInit`, in case it resets the bank to zero.
4. **Key codes** (appendix B.2): does `FCTN`+digit really give the digit's own code (TRACKER2's mute and solo,
   only checked in the emulator so far)?
5. **VM speed**: at a very high tempo, playback could skip steps. Micro-8 Studio has a "Hardware Speed (X1)"
   mode to get a sense of this; calibrate with `X1BENCH.SRC`.
6. **Pattern memory area** (page `0x90`, from `0x24000` to `0x2B800` with the clipboard): to avoid in a bitmap
   double buffer.

## Possible next steps

- Per-cell effects (arpeggio, glide, cut): would need one more file and a finer playback engine; TRACKER2 only
  has 16% of the code area left as margin (more with `data 0xC000`). Micro-8 Studio already has these ("Studio
  FX"), in Studio only.
- Visualizing sound with the ADC (`adcSampleL`): the emulator does not simulate the ADC, worth trying on the
  machine.
