# Porting a Micro-8 tracker song to another system

For a developer who composes in the Micro-8 Studio tracker and wants the same music in a game or demo on another
system: Mega Drive, NES, C64, Game Boy, Amiga, a PC engine such as Unity or Godot, or your own player.

The tracker is a plain step sequencer driving the Micro-8's sound chips. Everything a port needs is either in the song
file or in one of the exports below. This page says what each export holds, how the song is timed, how a note becomes a
sound on the chip, and gives a recipe per target.

## 1. What you get from the tracker

| What | Where | Holds | Good for |
|---|---|---|---|
| **VGM** | tracker, Export menu, *Export VGM* | every YM2612 register write with its time (VGM 1.50, with a loop point) | Mega Drive and every player that reads VGM: the FM sound is exact |
| **Note events, JSON** | *Export note events (JSON...)* | tempo, tracks, FM instruments as named fields, every note with time, length, MIDI note and pitch in Hz | your own player or converter, any language |
| **Note events, CSV** | *Export note events (CSV)* | the same notes, one row each | spreadsheets, quick scripts |
| **MIDI** (one file, or one per track) | *Export MIDI...* | notes, tempo, one MIDI channel per track, drums on channel 10 | DAWs, MIDI editors, and the MIDI importers and converters of other systems' trackers |
| **WAV** (one file, or stems, one per track) | *Export WAV...* | the song as the Studio plays it, 44.1 kHz stereo | streaming music, sampled formats (MOD, XM), engines that play audio files |
| **`.M8S`** | the song itself | the song as the Micro-8 reads it (§3) | reading the song without Studio |
| **`.INS`** | an instrument | 54 bytes, one FM instrument (§4) | reusing a sound on another YM2612 or YM2151 player |

The Studio effects (arpeggio, retrigger, delay, cut, probability, swing, glide, vibrato and tremolo) are part of the
export: arpeggios, retriggers and glides are written out as ordinary notes, so a port never has to know them. Vibrato and
tremolo reach the VGM as chip writes and the JSON as `studio_fx`.

## 2. How the song is timed

- A **step** is a sixteenth note. The tempo is in beats per minute and a beat is four steps, so a step lasts
  `15 / tempo` seconds (120 BPM: 0.125 s). The JSON gives it as `step_seconds`.
- A **pattern** has 16 steps and holds 8 tracks. A song is a list of up to 128 positions, each naming a pattern (0 to 63)
  and a transposition in semitones. A *long pattern* (16 to 256 steps) is several patterns claimed one after the other.
- The effects use **6 ticks per step**, so notes can fall between two steps. Times in the exports already include that,
  and the swing.
- Each track is **monophonic**, like the chip's channel: a new note ends the one before it. `duration_s` in the JSON is
  the time to the next event on the same track, a note off, or the end of the song.
- The song loops when `loops` is true. The VGM carries the loop point (the start of the song).

### The 8 tracks

| Track | Chip | Kind |
|---|---|---|
| FM1 to FM6 | YM2612 (6 FM channels of 4 operators) | melodic, each one plays one of 10 instruments |
| DRM | digital synth, voice 0 | 20 sampled drums (or a synth voice) |
| SYN | digital synth, voice 1 | synth voice: square, saw, noise, or the 20 drums |

The YM2612 is the Mega Drive's FM chip: a VGM of the six FM tracks plays unchanged on a Mega Drive, an emulator, or a VGM
player. DRM and SYN are the Micro-8's own digital synth: they are not in the VGM, take them from the MIDI or JSON export.

## 3. Notes

A note in a pattern is a value from 1 to 120 (255 is a note off), with an instrument (0 to 9) and a velocity (1 to 127).

- `midi_note = value - 1 + offset`, with `offset` the song's key offset (`transpose_offset` in the JSON, usually 0). Studio's own note names count one octave higher than MIDI: its A-3 is the A of 110 Hz.
- Each FM track also has an **octave** setting. The pitch the chip sounds is
  `hz = 440 * 2^((midi_note - 69) / 12 + (octave - 5))`. The JSON gives that frequency as `hz` for the FM tracks, so a
  port does not need the formula; MIDI exports send the note itself.
- **Drums** (DRM and SYN in drum mode): the value 1 to 20 picks the drum. The JSON gives it as `drum`, with the General MIDI
  percussion note in `gm_drum_note`: 1 acoustic kick (35), 2 electro kick (36), 3 techno kick (36), 4 acoustic snare (38),
  5 electro snare (40), 6 techno snare (40), 7 rim shot (37), 8 clap (39), 9 closed hat (42), 10 electro closed hat (42),
  11 open hat (46), 12 electro open hat (46), 13 pedal hat (44), 14 tambourine (54), 15 ride (51), 16 crash (49),
  17 tom (47), 18 clave (75), 19 conga (63), 20 cowbell (56).
- **Velocity** lowers the total level of the carrier operators (the output ones) on the chip, together with the voice
  volume and the master volume: `gain = volume/127 * master/127 * velocity/128`, then the carriers' total level goes up by
  `-20 * log10(gain) / 0.75`, capped at 127 (one total level step is 0.75 dB).

## 4. FM instruments

An `.INS` file or a slot of the song is 54 bytes. The JSON export names them (`fm_instruments`, by slot):

| Bytes | Content |
|---|---|
| 0 to 39 | the four operators, 10 bytes each, in the YM2612's operator 1, 2, 3, 4 order |
| 40 | feedback (0 to 7) |
| 41 | algorithm (0 to 7) |
| 42 | LFO amplitude depth (AMS, 0 to 3) |
| 43 | LFO frequency depth (PMS, 0 to 7) |
| 44 | operators enabled, a mask of 4 bits (0 is read as all four) |
| 45 | special mode (0 or 1, the YM2612's channel 3 mode; Studio does not use it) |
| 46 to 53 | not used by Studio |

Per operator, in order: detune (0-7), multiple (0-15), total level (0-127), rate scaling (0-3), attack rate (0-31), AM
enable (0-1), decay rate (0-31), sustain rate (0-31), sustain level (0-15), release rate (0-15). They are the YM2612's own
values, so nothing needs converting on a Mega Drive. How they land in the registers of channel *c* of bank *b* (operator
register offsets are 0x00, 0x08, 0x04, 0x0C for operators 1 to 4):

| Register | Value |
|---|---|
| 0x30 + offset | detune << 4 \| multiple |
| 0x40 + offset | total level (the carriers get the velocity attenuation added) |
| 0x50 + offset | rate scaling << 6 \| attack rate |
| 0x60 + offset | AM enable << 7 \| decay rate |
| 0x70 + offset | sustain rate |
| 0x80 + offset | sustain level << 4 \| release rate |
| 0xB0 + c | feedback << 3 \| algorithm |
| 0xB4 + c | left << 7 \| right << 6 \| AMS << 4 \| PMS |
| 0xA4 + c, 0xA0 + c | block << 3 \| fnum high, then fnum low |
| 0x28 | key on or off: the operator mask << 4, then the channel (0 to 2 and 4 to 6) |

The frequency number is `fnum = hz * 144 * 2^(21 - block) / clock`, with the smallest block that keeps it under 2048
and the clock of the Mega Drive's chip, 7,670,454 Hz (NTSC). A PAL console (7,600,489 Hz) plays everything about 1% lower.

## 5. Recipes

**Mega Drive / Genesis.** Export the VGM. Play it with any VGM player to hear what the console will play. With SGDK,
give the VGM to the resource compiler (`XGM name "song.vgm"` in a `.res` file; `xgmtool` does the same conversion by hand), then call
`XGM_startPlay(name)`. The drums and the SYN track are not in the VGM: either leave them to the PSG and DAC of your
game, or replace them with a PCM drum kit played by the XGM driver. To rebuild the song in a Mega Drive tracker instead, read
the instruments from the JSON (the fields are the YM2612's) and the notes from the MIDI or the JSON.

**NES.** Export MIDI, one file per track, and the JSON. FM1 and FM2 make good pulse channels, FM3 the triangle, DRM the noise
and DPCM channels. Enter the parts in your NES tracker (or script it from the JSON) and make new sounds: the FM instruments have no
NES equivalent, so use `hz`, `velocity` and `duration_s` to set the pitch and the volume envelope of each note.

**C64.** Export MIDI per track, and the JSON. The SID has 3 voices: pick the three tracks that carry the song (bass, lead,
drums) and fold the rest into arpeggios (the arpeggio notes are already in the events).

**Game Boy.** Export MIDI per track, and the JSON. Four channels: two pulses (leads), a wave (bass), noise (drums).

**Amiga and MOD / XM trackers.** Export the WAV stems (one per track). Load them as samples, or cut one note of each
instrument from the stem and play it at the notes of the JSON.

**Unity, Godot, any engine that plays audio.** Export the WAV (or the stems for adaptive music) and loop it. The JSON `step_seconds`
and `length_seconds` give the beat grid for syncing the game to the music; `loops` and the length tell where it repeats.

**Your own player.** Read the JSON. A minimal reader that turns the events into one record per note on a 60 Hz frame
counter:

```python
import json
doc = json.load(open('song.events.json'))
FPS = 60
records = []
for e in doc['events']:
    if e['type'] != 'on':
        continue
    frame = round(e['time_s'] * FPS)
    length = round(e.get('duration_s', 0.1) * FPS)
    note = e.get('midi_note', e.get('gm_drum_note'))
    records.append((frame, e['track'], note, e['velocity'], length))
```

Sort by frame, store the differences between frames and write one byte per field: a typical 8-bit player is a loop that
waits for the next frame and starts the note on the channel of the system. For the FM tracks, keep `hz` and the
instrument slot to play the same sound on a YM2612 or YM2151 driver.

## 6. Checking a port

- Play the **WAV** next to your port: the song's length, tempo and loop point should match `length_seconds`, `tempo_bpm`
  and `loops`.
- The **VGM** is the reference for the FM sound: if a VGM player and your port disagree, the port is wrong.
- Compare note counts: the JSON has one `on` event per note of the song, the MIDI one note on per `on` event of the same
  track.
- The effects are expanded in all exports. If a note is missing in your port, check whether the song uses probability
  (`P` effect): its notes depend on the song's random seed, and an export keeps the roll it made.
