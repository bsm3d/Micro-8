# Micro-8 Studio

## Contents

- [1. Overview](#1-overview)
- [2. Editor](#2-editor)
- [3. Run & Debug](#3-run--debug)
- [4. Image Converter](#4-image-converter)
- [5. Asset Editors](#5-asset-editors)
- [6. Small Windows](#6-small-windows)
- [7. Known Limitations](#7-known-limitations)

---

## 1. Overview

An IDE for the Micro-8 and its Lofi language: editor, live analyzer, emulator with debugger, image converter.
**Dark** and **light** themes (View > Dark / Light Theme, or Preferences), a splash screen with real progress
steps at startup (`--no-splash` to skip it), and an **About** box (Help > About).

Unzip the portable version anywhere (a drive, a USB stick) and run `Micro8Studio.exe`, nothing to install; the
installer places the same files under Program Files or a folder of your choice. The folder holds `Examples\`
(the Micro-8 user manual and `OS\`, the default SD card) and `runtime\`; Studio keeps its settings in `data\`
next to the `.exe` (nothing in the registry), so the folder can be copied as it is to another PC. The emulator
core is compiled for the processor: x86-64, AVX2 or AVX-512, picked at startup (shown in About).

The default root folder is the `Examples/OS` SD card (next to the app, with the user manual in `Examples/`)
(`Root...` in the Files panel to pick another one): it is the emulated machine's file system (`import`,
`export`, `fOpen`...).

## 2. Editor

- Tabs, line numbers, Lofi syntax highlighting (types, keywords, API, `_xxx` built-in variables, numbers, text,
  comments), cut / copy / paste / undo / redo, current-line highlight, matching parentheses, occurrences of the
  word under the cursor.

- Find `Ctrl+F` (count, case, whole word, regex), replace `Ctrl+H`, `F3` / `Shift+F3`, find in files
  `Ctrl+Shift+F`, go to line `Ctrl+G`, bookmarks `F2` / `Ctrl+F2`.

- Auto-indent, auto-close `( [ { "`, `Tab` / `Shift+Tab` on a selection, comment `Ctrl+/`, duplicate line
  `Ctrl+D`, delete line `Ctrl+Shift+K`, move lines `Alt+↑/↓`.

- **Completion** (as you type, or `Ctrl+Space`): your functions, variables and types, the whole API (with the
  manual's own signatures), built-in variables, keywords, **members** after `variable.` or `array[i].` for your
  custom types, and **snippets** (`for`, `while`, `if`, `ifelse`, `main`, `func`, `type`, `loop`, `sprite`,
  `import`: name + `Tab`, or from the list). Picking a function adds its parentheses. **Parameter hints**: inside
  a call, the signature is shown with the current parameter in bold (API and program functions alike).

- **Navigation and refactoring**: go to definition `F12` or `Ctrl+click` (function, type, variable; for the API,
  its signature), rename a symbol `Ctrl+R` (whole-word occurrences outside text and comments, one single undo),
  format the document `Ctrl+Alt+L` (brace-based indentation, the file's own style: real sources use tabs), zoom
  `Ctrl+wheel`.

- **Preferences** (`Ctrl+,`): theme, **language** (English / Français, restart to apply), font and size, tab
  width or tabs, pair auto-close, completion and hints, analysis delay, format on save. The interface (menus,
  toolbar, Preferences) is translated through the standard Qt Linguist mechanism.

- **Live analysis** (0.5 s after typing): errors and warnings underlined, a gutter marker, the Problems panel,
  the program outline. A program with errors does not run. The analysis also knows the real compiler's rules
  found on the machine (a `<<` `>>` shift is by a `byte`, never an `int`) and estimates the bytecode size (a
  model fitted on the SD card's own `.BIN` files, within 5%): shown in the memory map, with a warning past the
  32 KB code segment.

- **Bytes preserved**: a file is saved byte for byte (CR LF or LF line endings kept as they were). The Micro-8's
  own control codes inside text literals (newline 0x0D, colors…) show as control pictures (␍ ␐…); Edit > Insert
  Control Glyph inserts one.

## 3. Run & Debug

`F5` runs the program on the emulated Micro-8 (screen in the *Micro-8 screen* dock: click it to give it keyboard
focus; mouse; gamepad: arrows, Z=A, X=B, Enter=Start, Tab=Select). Sound plays live (FM via Nuked-OPN2). A
run-time error stops the program on its line with a plain message, also when it comes from an impossible
argument (`5 >> -1`, `sqrt(-1.0)`, a pow() with no real result, an address out of range); a real passed where
the machine expects an integer is truncated as `int()` does.

| Action | Key |
|---|---|
| Run / continue | `F5` (run without breakpoints: `Ctrl+F5`) |
| Stop / restart | `Shift+F5` / `Ctrl+Shift+F5` |
| Pause | `F6` |
| Step out / into / over | `Shift+F11` / `F11` / `F10` |
| Breakpoint | `F9` or click the gutter (right-click: a condition, e.g. `i==2`) |
| Memory breakpoint | Debug > Add Memory Breakpoint, or right-click a byte in Memory: the program stops as soon as that byte (or range, RAM or external SRAM) changes, whatever wrote it (an assignment, `poke`, an API function, the raster handler); Output shows the address, old -> new value and the line that wrote |

Panels: **Variables** (call stack, locals and globals, changed values in orange), **Watch** (Lofi expressions
evaluated while paused, e.g. `data[i]*10`), **Breakpoints** (lines and memory), **Memory** (a hex dump of RAM or
external SRAM, changed bytes highlighted, shortcuts to the useful zones), **Hardware** (layers, scrolling,
pages, raster, palettes, sprites, synth registers).

### 3.1. Emulation speed

- **Default**: the program runs as fast as the PC allows; machine time follows the real clock (excluding time
  spent paused).

- **Run > Hardware Speed (X1)**: the program runs at the real Micro-8's speed. Every VM operation (an expression
  node, a loop test, a jump) costs a fixed time; machine time is the sum of those costs, and the emulator waits
  for the real clock whenever it is ahead. A program therefore does as much work per frame as on the machine.
  Remembered, can be switched while running; the status bar shows `Running (X1)`.

- **Speed**: Preferences > Emulation > Hardware speed (X1), 1000 kHz by default (VM ≈ 1 MHz). To calibrate: run
  `dev/X1BENCH.SRC` on the real machine and enter the measured total in Preferences to get the value to use.

- **PC too slow**: if the PC cannot keep up, the whole run slows down (clock and sound included) instead of
  skipping machine time; the status bar says so.

Limitation: this is a cost model, not a cycle-exact emulation of the ATmega128 and its firmware, API calls
(memory copies, drawing) cost the same whatever their size.

Tools menu: array-index checking (disabled like the real machine / warn in Output / stop), sound, the synth's
note convention, PNG screenshot, exporting the session's sound as WAV.

## 4. Image Converter

`Ctrl+Shift+I` opens the *Image tools* window: drag and drop an image (PNG, JPG, GIF, TGA…).

- **400×240 bitmap** → `.BMP` (48000 bytes, 4 bits/pixel) + `.PAL` (48 bytes, 3 bits per component).

- **24×21 sprites** → `.SPR` (252 pixel bytes + a computed AABB collision box) + `.PAL`.

- **16×16 tiles** → `.TLS` + `.PAL`; **tiles + map**: the image is sliced, identical tiles merged, `.TLS` +
  `.LVL` (one byte per cell) + `.PAL`. A warning past 256 tiles / 256 sprites.

- Fit (fit / fill / stretch / 1:1), resampling, brightness / contrast / saturation, a palette optimized for the
  image, or one loaded from a `.PAL`, or a hand-edited palette (click a color: 512 colors), dithering (none,
  Floyd-Steinberg, Bayer 4×4), index-0 transparency, before/after comparison, zoom.

- Export to a chosen folder plus **ready-to-paste Lofi code** (`import(...)`, palette addresses, layer
  activation).

- **Asset viewer** tab: opens existing `.BMP`, `.SPR`, `.TLS` files with their palette (found automatically),
  collision boxes in magenta, PNG export.

## 5. Asset Editors

The desktop equivalents of the SD card's own tools (SPRITED, MAPED, IMAGED, SOUNDED, SCREENED, the built-in
tracker).

Each editor opens in a tab next to the code: toolbar, Tools or File > New Asset, double-click a file in the
tree, or drag and drop. Save `Ctrl+S`, undo/redo/copy/paste from the Edit menu. All of them read then write back
the real SD-card files **byte for byte**.

### 5.1. Sprite editor

`.SPR` + `.PLA` / `.PLB`: up to 256 24×21 sprites.

- A thumbnail list (new, duplicate, delete, move).
- Pixel-by-pixel drawing: pencil, eraser, line, rectangle, oval, fill, eyedropper, selection with move / copy /
  paste, X/Y mirror, brush sizes 1-4, wheel zoom, grid, onion skin (the previous sprite shown transparently).
- Per-sprite undo, a palette editor (512 colors, palettes A and B).
- An **AABB collision box** (fields, auto-fit), flips, color replace/swap.
- An **animation preview** (range, frames per second), importing a PNG sheet, PNG export.

### 5.2. Tile / map editor

`.TLS` + `.PAL` + `.LVL`: up to 256 16×16 tiles.

- Tiles edited pixel by pixel, a map painted with paint, fill, eyedropper, selection copy / paste.
- Map resizing, a 25×15 screen frame guide, grid, zoom.
- Deleting a tile renumbers the map automatically.
- Importing an image into tiles + a map (identical tiles merged), PNG export.
- Formats: raw `.TLS` (+ `.PAL`) or the tile editor's own shape (a 48-byte palette in front); `.LVL` with a
  4-byte header (width, height) or raw (width is asked for, as with the TILES demo's 80×24 level).

### 5.3. Bitmap editor

`.BMP` 400×240 + `.PAL`: the same tools as the sprite editor, across the whole image.

- A 16-color palette, a 16×16 guide grid.
- Importing an image with fitting, dithering and an optimized palette.
- Pasting an image from the clipboard, PNG export.

### 5.4. Sound editor

`.INS` 54 bytes, `.SYN` 16 bytes: a bank of 10 YM2612 instruments.

- The 54 parameters (4 operators: detune, multiplier, total level, rate scaling, attack, AM, decay, sustain
  rate, sustain level, release).
- The algorithm with a **diagram** (carriers in orange), feedback, LFO, active operators.
- An **envelope curve** per operator, a **test keyboard** (mouse or Z S X D C V… / Q 2 W 3 E R… keys) played
  live through Nuked-OPN2, the sound's waveform and spectrum.
- Copy / paste / init / randomize / mutate, loading an `.INS` into a slot, a bank of 10 files.
- A **digital synth** tab: oscillators, DPCM drums (20 sounds), envelopes, LFO, SVF filter, gains and panning,
  the 16 registers at 0x1110. The preset saves as `.SYN` (16 bytes, the studio's own format).

### 5.5. Wave editor

`.wav`: waveform with a selection, zoom, scrolling, a spectrum view.

- Cut / copy / paste / delete / trim, silence, reverse, gain, normalize, fades, DC-offset removal, mono.
- Resampling (8-48 kHz), N-bit clipping (the synth's DPCM drums are 4-bit), signal generators, looped playback,
  WAV export.
- "Send to wave editor" (sound editor) and Tools > Sound of this Run feed it with the synth's or a program's
  sound.

### 5.6. Tracker

One `.M8S` file compressed the C64 way (tracks as event lists, repeated or transposed tracks stored as
references; format described in the Tracker documentation): the Micro-8's built-in music sequencer.

- 8 tracks: 6 FM voices, a drum/oscillator track "DRM", a "SYN" track.
- Note/instrument/velocity cells, patterns from 16 to 256 steps (blocks claimed/released).
- A song (a loopable sequence of transposable patterns), a page or continuous view. While the song plays, the
  grid follows the pattern playing; picking another one by hand shows it until the next entry.
- **SONG band**: one card per entry, 2 rows of 16 (like Elektron banks). The pattern reads as a bank
  (`A01`…`D16`, 16 patterns a bank) with its number in hex; an entry that repeats is one card "×N" (Elektron's
  REP: `Ctrl`+wheel or the copy button to add or remove one, right-click for an exact count), and the card
  playing shows which repeat ("2/3"). Past 32 cards, page buttons ("0-31", "32-63"...); the page follows
  playback, and hovering a page button while dragging a card opens it. Wheel: pattern, `Shift`+wheel: transpose,
  drag: move (with its repeats), double-click: play from here.
- OctaMED-style editing: range selection by Shift+arrows or mouse, transpose, copy/cut/paste a range, reverse,
  expand/shrink, fixed/ramp/random velocity.
- A scope per track, mute (click) / solo (double-click), **instant**: the sounding note is cut.
- While a pattern plays, picking another one plays it **at once**, at the same step of the bar (Elektron's
  direct jump), instead of waiting for the end of the pattern. The on-device trackers and player do the same on
  the machine.

**Generators**: Euclidean rhythms, polyrhythms, 20 drum presets, bass/pad/arp/random presets, 12 FM presets,
they only *write notes*, so they are 100% compatible with the real machine.

**Studio FX** (arp, retrigger, delay, cut, probability, YM LFO, swing, track lengths): preview-only in Studio,
can be turned off, playback on the machine stays the on-device tracker's own. MIDI, WAV and stems export.
`.EXT.json`: a sidecar file for pattern lengths and FX, not read by the machine.

Tested byte-exact against the on-device trackers (the two versions of the on-device tracker, and the player): a
project made here opens unchanged on the real machine and back.

### 5.7. Screen designer

`.SCR` files: a generic 80×40 text-screen editor (glyphs + colors), in the style of the system tools
(SPRITED/SOUNDED/MAPED).

- A glyph palette (all 256 of the real font), a paper/ink color bar (the machine's own default text palette).
- Paint / box / circle / fill / write tools (a typewriter: click places a cursor, then you type), an eyedropper,
  zoom, undo/redo.
- `Ctrl+←/→` cycles the ink color, `Ctrl+↑/↓` the paper color.

A `.SCR` file is 6400 bytes (3200 glyphs then 3200 colors). A plain `import(name,0x1200,6400)` displays it
from any Lofi program. A corrupted file is refused on load.

### 5.8. Micro-8 Library

Edit > Insert Micro-8 Library... pastes ready-made Lofi functions into the current program to read and write,
**on the machine**, the files the Studio editors produce (`lib/M8LIB.SRC`). Lofi has no `#include` and compiles
in one pass: the ticked blocks, plus the ones they need, go after the opening comments, in the library's order.
A block already in the program is not copied again, and a single undo removes the insert.

| Block | Functions | File |
|---|---|---|
| `pal` | `loadPal` / `savePal` | 48-byte `.PAL` (16 R, 16 G, 16 B, 0-7) to a palette address |
| `bitmap` | `loadBitmap` / `saveBitmap` | 48000-byte `.BMP`, the displayed bitmap page (0x2F05) |
| `sprites` | `loadSprites` / `saveSprites` | `.SPR`, 256 bytes a sprite, from a given number |
| `tiles` | `loadTiles` / `saveTiles` | `.TLS`, 128 bytes a 16x16 tile |
| `level` | `loadLevel` / `saveLevel` | `.LVL`, one tile number a cell, in a byte array |
| `screen` | `loadScreen` / `saveScreen` | `.SCR` 3200 glyphs + 3200 colours (the Screen designer's) |
| `song` | `songLoad` / `songStart` / `songTick` / `songStop` | tracker project `.M8S`, played by the PLAYER core |

All return `true` on success (`_fError` is 0). A damaged `.SCR` is refused. The end of `M8LIB.SRC` (`//@main`) is a self-test to run as is (F5): it writes and reads back every
kind of file and plays `NEWSONG.M8S` when there is one.

### 5.9. Projects

File > Project: a project is **one folder of the SD card** holding a program and its assets, described by a
small JSON manifest `NAME.M8P` (an 8.3 name, so it can travel on the card harmlessly). The manifest keeps the
main program, the project's files, the open tabs, the current tab and the X1 setting.

- **New Project...**: pick the folder and the name (1 to 8 letters, digits or `_`); creates `NAME.SRC` when
  missing, and the assets already in the folder join the project. **Open Project...** (`Ctrl+Shift+O`) reopens
  the tabs and X1 mode; Studio reopens at startup the project that was open when it closed.
- **Save Project** (`Ctrl+Alt+Shift+S`) saves every open file and the manifest; a file opened from the project
  folder joins it (or **Add Current File to Project**). A new file is offered in the project folder by default.
  **Close Project** closes only the project's tabs.
- **F5** with an asset editor in front runs the project's program.
- **Check Project**: what the real machine would refuse (names that are not 8.3 for its FAT32, compile errors,
  code over the 32 KB segment, missing project files) and the files the program names in its strings that are
  not in the folder (only a note: the program may write them itself).
- **Export Project to SD Card...**: copies the manifest, the program and its files into `CARD/FOLDER/` (asks
  before replacing). **Send Project to Machine...**: the same, over MIDI (§5.10).
- **Tools > Write System to SD Card...**: writes the card shipped with Studio (system files, examples and their
  compiled programs) onto a real SD card for the machine. A summary lists the new, replaced and identical files
  before writing, and the machine's clipboard or undo already on the card are kept. The target must be a
  removable drive (never a fixed disk nor Studio's own folder).
- The project folder must sit under the Files tree's root (the emulated card) for the program's paths to
  resolve as on the machine: when it does not, the root moves up to its parent.

### 5.10. Transfer with the Machine (MIDI)

Tools > Transfer with Machine...: send, fetch and list the files on a real Micro-8's SD card, through its MIDI
ports and any USB-MIDI interface. Run `LINK.SRC` on the machine (copy it to the card first, with the **Copy
LINK.SRC to the SD Card** button or by hand), cable the PC's MIDI OUT to the Micro-8's MIDI IN and the Micro-8's
MIDI OUT to the PC's MIDI IN, then pick the ports, **Test Connection**, and use **List**, **Send Files...**,
**Send Project** or **Fetch Selected...**.

Without the machine: pick **Emulator**, **Start LINK in the Emulator**; everything works the same against the
emulator.

Speed is about 1.3 KB/s, so a 48000-byte picture takes about 40 s. Files up to 64 KB, 8.3 names. A received
file replaces the target only once complete: an interruption never damages it.

The MIDI keyboard chosen in Tools > MIDI Input also plays into the running program, as on the machine.

## 6. Small Windows

Every editor and panel adapts to a narrower window instead of being cut off: toolbars wrap onto several lines,
the sound editor stacks its areas (voices, operators, keyboard, oscilloscope) to fit the width, and the bottom
panels scroll. `Ctrl+Shift+M` (Focus Mode) hides every panel to get more room.

## 7. Known Limitations

The debugger works at line granularity (one statement at a time); `for(a;b;c)` does not stop on its three parts
separately.
