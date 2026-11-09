# Micro-8 Studio

## Contents

- [1. Overview](#1-overview)
- [2. Editor](#2-editor)
- [3. Run & Debug](#3-run--debug)
- [4. Image Converter](#4-image-converter)
- [5. Asset Editors](#5-asset-editors)
- [6. Icons & Narrow Windows](#6-icons--narrow-windows)
- [7. Code Structure](#7-code-structure)
- [8. Tests](#8-tests)
- [9. What's Validated](#9-whats-validated)
- [10. Known Limitations](#10-known-limitations)

---

## 1. Overview

An IDE for the Micro-8 and its Lofi language: editor, live analyzer, emulator with debugger, image converter. Same
look as the Spectra Workbench: **dark** and **light** themes (View > Dark / Light Theme, or Preferences), a splash
screen with real progress steps at startup (`--no-splash` to skip it), and an **About** box (Help > About) in the
suite's style.

**Portable version** (`Micro8Studio-<version>-win64-portable.zip`, made by `python build.py`): unzip it anywhere
(a drive, a USB stick) and run `Micro8Studio.exe`, nothing to install. The folder holds `Examples\` (the Micro-8
user manual and `OS\`, the default SD card) and `runtime\`; Studio keeps its settings in `data\` next to the
`.exe` (nothing in the registry), and the folder can be copied as it is to another PC. The emulator core is
compiled for the processor: x86-64, AVX2 or AVX-512, picked at startup (shown in About).

From the sources:

    run_studio.bat                       # or: python studio/main.py [FILE[:LINE]] [--root SD_FOLDER]

Requirements: Python 3.11+ with `pip install -r requirements.txt` (in `micro8_studio/`) (PySide6, Pillow, numpy).
The default root folder is the `Examples/OS` SD card (next to the app, with the user manual in `Examples/`)
(`Root...` in the Files panel to pick another one): it is the emulated machine's file system (`import`, `export`, `fOpen`…). The emulation engine itself (analyzer, interpreter,
machine, video, audio, debugger) is the `lofi/` package, documented separately in `../README.md` with its full
internal workings (memory, video, audio, tiles, sprites, raster interrupts...).

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
  `import`: name + `Tab`, or from the list). Picking a function adds its parentheses. **Parameter hints**: inside a
  call, the signature is shown with the current parameter in bold (API and program functions alike).

- **Navigation and refactoring**: go to definition `F12` or `Ctrl+click` (function, type, variable; for the API,
  its signature), rename a symbol `Ctrl+R` (whole-word occurrences outside text and comments, one single undo),
  format the document `Ctrl+Alt+L` (brace-based indentation, the file's own style: real sources use tabs), zoom
  `Ctrl+wheel`.

- **Preferences** (`Ctrl+,`): theme, **language** (English / Français, restart to apply), font and size, tab
  width or tabs, pair auto-close, completion and hints, analysis delay, format on save. The interface (menus,
  toolbar, Preferences) is translated through the standard Qt Linguist mechanism (`self.tr()`, `.ts`/`.qm` files
  under `studio/i18n/`, see `i18n.py`).

- **Live analysis** (0.5 s after typing): errors and warnings underlined, a gutter marker, the Problems panel, the
  program outline. A program with errors does not run. The analysis also knows the real compiler's rules found on
  the machine (a `<<` `>>` shift is by a `byte`, never an `int`) and estimates the bytecode size (a model fitted on
  the SD card's 74 `.BIN` files, within 5%): shown in the memory map, with a warning past the 32 KB code segment.

- **Bytes preserved**: a file is saved byte for byte (CR LF or LF line endings kept as they were). The Micro-8's
  own control codes inside text literals (newline 0x0D, colors…) show as control pictures (␍ ␐…); Edit > Insert
  Control Glyph inserts one.

## 3. Run & Debug

`F5` runs the program on the emulated Micro-8 (screen in the *Micro-8 screen* dock: click it to give it keyboard
focus; mouse; gamepad: arrows, Z=A, X=B, Enter=Start, Tab=Select). Sound plays live (FM via Nuked-OPN2). A
run-time error stops the program on its line with a plain message, also when it comes from an impossible argument
(`5 >> -1`, `sqrt(-1.0)`, a pow() with no real result, an address out of range); a real passed where the machine
expects an integer is truncated as `int()` does.

| Action | Key |
|---|---|
| Run / continue | `F5` (run without breakpoints: `Ctrl+F5`) |
| Stop / restart | `Shift+F5` / `Ctrl+Shift+F5` |
| Pause | `F6` |
| Step out / into / over | `Shift+F11` / `F11` / `F10` |
| Breakpoint | `F9` or click the gutter (right-click: a condition, e.g. `i==2`) |
| Memory breakpoint | Debug > Add Memory Breakpoint, or right-click a byte in Memory: the program stops as soon as that byte (or range, RAM or external SRAM) changes, whatever wrote it (an assignment, `poke`, an API function, the raster handler); Output shows the address, old -> new value and the line that wrote |

Panels: **Variables** (call stack, locals and globals, changed values in orange), **Watch** (Lofi expressions
evaluated while paused, e.g. `data[i]*10`), **Breakpoints** (lines and memory), **Memory** (a hex dump of RAM or external SRAM,
changed bytes highlighted, shortcuts to the useful zones), **Hardware** (layers, scrolling, pages, raster,
palettes, sprites, synth registers).

### 3.1. Emulation speed

- **Default**: the program runs as fast as the PC allows; machine time follows the real clock (excluding time spent
  paused).

- **Run > Hardware Speed (X1)**: the program runs at the real Micro-8's speed. Every VM operation (an expression
  node, a loop test, a jump) costs a fixed time; machine time is the sum of those costs, and the emulator waits for
  the real clock whenever it is ahead. A program therefore does as much work per frame as on the machine.
  Remembered, can be switched while running; the status bar shows `Running (X1)`.

- **Speed**: Preferences > Emulation > Hardware speed (X1), 1000 kHz by default (VM ≈ 1 MHz). To calibrate: run
  `dev/X1BENCH.SRC` on the real machine, then `python dev/x1_calibrate.py 1000 MEASURED_TOTAL` prints the value to
  enter.

- **PC too slow**: if the PC cannot keep up, the whole run slows down (clock and sound included) instead of
  skipping machine time; the status bar says so.

Limitation: this is a cost model, not a cycle-exact emulation of the ATmega128 and its firmware, API calls
(memory copies, drawing) cost the same whatever their size.

Tools menu: array-index checking (disabled like the real machine / warn in Output / stop), sound, the synth's note
convention, PNG screenshot, exporting the session's sound as WAV, **Hardware Link...** (see §5.8).

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
  collision boxes in magenta, PNG export. Formats were checked against the real SD-card resources: MOUNTAIN's
  `.BMP` round-trips identically, and so do the sprites.

## 5. Asset Editors

The desktop equivalents of the SD card's own tools (SPRITED, MAPED, IMAGED, SOUNDED, SCREENED, the built-in
tracker).

Each editor opens in a tab next to the code: toolbar, Tools or File > New Asset, double-click a file in the tree,
or drag and drop. Save `Ctrl+S`, undo/redo/copy/paste from the Edit menu. All of them read then write back the
real SD-card files **byte for byte** (checked by the tests).

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
- Formats: raw `.TLS` (+ `.PAL`) or the tile editor's own shape (a 48-byte palette in front); `.LVL` with a 4-byte
  header (width, height) or raw (width is asked for, as with the TILES demo's 80×24 level).

### 5.3. Bitmap editor

`.BMP` 400×240 + `.PAL`: the same tools as the sprite editor, across the whole image.

- A 16-color palette, a 16×16 guide grid.
- Importing an image with fitting, dithering and an optimized palette.
- Pasting an image from the clipboard, PNG export.

### 5.4. Sound editor

`.INS` 54 bytes, `.SYN` 16 bytes: a bank of 10 YM2612 instruments.

- The 54 parameters (4 operators: detune, multiplier, total level, rate scaling, attack, AM, decay, sustain rate,
  sustain level, release).
- The algorithm with a **diagram** (carriers in orange), feedback, LFO, active operators.
- An **envelope curve** per operator, a **test keyboard** (mouse or Z S X D C V… / Q 2 W 3 E R… keys) played live
  through Nuked-OPN2, the sound's waveform and spectrum.
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

One `.M8S` file compressed the C64 way (tracks as event lists, repeated or transposed tracks stored as references:
782 bytes for SID RUSH; format in `micro8_tracker/README.md`): the Micro-8's built-in music sequencer.

- 8 tracks: 6 FM voices, a drum/oscillator track "DRM", a "SYN" track.
- Note/instrument/velocity cells, patterns from 16 to 256 steps (blocks claimed/released).
- A song (a loopable sequence of transposable patterns), a page or continuous view. While the song plays, the
  grid follows the pattern playing; picking another one by hand shows it until the next entry.
- **SONG band**: one card per entry, 2 rows of 16 (like Elektron banks). The pattern reads as a bank (`A01`…`D16`,
  16 patterns a bank) with its number in hex; an entry that repeats is one card "×N" (Elektron's REP:
  `Ctrl`+wheel or the copy button to add or remove one, right-click for an exact count), and the card playing
  shows which repeat ("2/3"). Past 32 cards, page buttons ("0-31", "32-63"...); the page follows playback, and
  hovering a page button while dragging a card opens it. Wheel: pattern, `Shift`+wheel: transpose, drag: move
  (with its repeats), double-click: play from here.
- OctaMED-style editing: range selection by Shift+arrows or mouse, transpose, copy/cut/paste a range, reverse,
  expand/shrink, fixed/ramp/random velocity.
- A scope per track, mute (click) / solo (double-click), **instant**: the sounding note is cut.
- While a pattern plays, picking another one plays it **at once**, at the same step of the bar (Elektron's direct
  jump), instead of waiting for the end of the pattern. TRACKER, TRACKER2 and PLAYER do the same on the machine.

**Generators** (`trackergen.py`, `trackertools.py`): Euclidean rhythms, polyrhythms, 20 drum presets,
bass/pad/arp/random presets, 12 FM presets, they only *write notes*, so they are 100% compatible with the real
machine.

**Studio FX** (arp, retrigger, delay, cut, probability, YM LFO, swing, track lengths): preview-only in Studio, can
be turned off, playback on the machine stays TRACKER.SRC's own. MIDI, WAV and stems export. `.EXT.json`: a
sidecar file for pattern lengths and FX, not read by the machine.

Tested byte-exact against TRACKER.SRC/TRACKER2.SRC (the two versions of the on-device tracker, see
`../micro8_tracker/`): a project made here opens unchanged on the real machine and back.

### 5.7. Screen designer

`.SCR`, `screendesign.py`: a generic 80×40 text-screen editor (glyphs + colors), in the style of the system tools
(SPRITED/SOUNDED/MAPED).

- A glyph palette (all 256 of the real font), a paper/ink color bar (the machine's own default text palette).
- Paint / box / circle / fill / write tools (a typewriter: click places a cursor, then you type), an eyedropper,
  zoom, undo/redo.
- `Ctrl+←/→` cycles the ink color, `Ctrl+↑/↓` the paper color.

A `.SCR` file is 6400 bytes (3200 glyphs then 3200 colors, exactly the screen-memory layout at 0x1200/0x1E80: a
plain `import(name,0x1200,6400)` displays it from any Lofi program), plus an optional 2-byte Fletcher-16 checksum,
ignored by that same `import`, verified on load here and in the on-device equivalent tool,
`micro8_tracker/SCREENED.SRC` (source removed from the repository). A corrupted file is refused on load, an older file without the checksum still
loads.

### 5.8. Hardware Link (experimental)

Tools > Hardware Link... (`hwlink.py`, `hwlink_panel.py`): groundwork for a future USB-MIDI link to real Micro-8
hardware, through a USB-MIDI interface plugged into the PC.

- Wraps raw bytes in SysEx frames (manufacturer ID reserved for non-commercial use, `0x7D`), versioned and
  numbered (request/reply matching), checksummed (a plain 8-bit checksum).
- Ping and Identify to validate a loopback cable.
- Sending runs on a dedicated thread so it never freezes the interface.

**The real Micro-8 firmware does not speak this protocol**, and SysEx never reaches a Lofi program: to exchange
files with the machine, see §5.11 (Transfer with Machine), which works with the current firmware.

### 5.9. Micro-8 Library

Edit > Insert Micro-8 Library... pastes ready-made Lofi functions into the current program to read and write,
**on the machine**, the files the Studio editors produce (`lib/M8LIB.SRC`, `m8lib.py`). Lofi has no `#include` and
compiles in one pass: the ticked blocks, plus the ones they need, go after the opening comments, in the library's
order. A block already in the program is not copied again, and a single undo removes the insert.

| Block | Functions | File |
|---|---|---|
| `pal` | `loadPal` / `savePal` | 48-byte `.PAL` (16 R, 16 G, 16 B, 0-7) to a palette address |
| `bitmap` | `loadBitmap` / `saveBitmap` | 48000-byte `.BMP`, the displayed bitmap page (0x2F05) |
| `sprites` | `loadSprites` / `saveSprites` | `.SPR`, 256 bytes a sprite, from a given number |
| `tiles` | `loadTiles` / `saveTiles` | `.TLS`, 128 bytes a 16x16 tile |
| `level` | `loadLevel` / `saveLevel` | `.LVL`, one tile number a cell, in a byte array |
| `screen` | `loadScreen` / `saveScreen` | `.SCR` 3200 glyphs + 3200 colours + Fletcher-16 sum (the Screen designer's) |
| `song` | `songLoad` / `songStart` / `songTick` / `songStop` | tracker project `.M8S`, played by the PLAYER core |

All return `true` on success (`_fError` is 0). A damaged `.SCR` is refused, an older `.SCR` without the sum is
accepted. The end of `M8LIB.SRC` (`//@main`) is a self-test to run as is (F5): it writes and reads back every kind
of file and plays `NEWSONG.M8S` when there is one.

### 5.10. Projects

File > Project (`project.py`): a project is **one folder of the SD card** holding a program and its assets,
described by a small JSON manifest `NAME.M8P` (an 8.3 name, so it can travel on the card harmlessly). The manifest
keeps the main program, the project's files, the open tabs, the current tab and the X1 setting.

- **New Project...**: pick the folder and the name (1 to 8 letters, digits or `_`); creates `NAME.SRC` when missing,
  and the assets already in the folder join the project. **Open Project...** (`Ctrl+Shift+O`) reopens the tabs and
  X1 mode; Studio reopens at startup the project that was open when it closed.
- **Save Project** (`Ctrl+Alt+Shift+S`) saves every open file and the manifest; a file opened from the project
  folder joins it (or **Add Current File to Project**). A new file is offered in the project folder by default.
  **Close Project** closes only the project's tabs.
- **F5** with an asset editor in front runs the project's program.
- **Check Project**: what the real machine would refuse (names that are not 8.3 for its FAT32, compile errors, code
  over the 32 KB segment, missing project files) and the files the program names in its strings that are not in the
  folder (only a note: the program may write them itself).
- **Export Project to SD Card...**: copies the manifest, the program and its files into `CARD/FOLDER/` (asks before
  replacing). **Send Project to Machine...**: the same, over MIDI (§5.11).
- **Tools > Write System to SD Card...**: writes the card shipped with Studio (system files, examples and their
  compiled programs) onto a real SD card for the machine. The `.BIN`, shipped encrypted, are decrypted by the C++
  core straight onto the card, which must be a removable drive (never a fixed disk nor Studio's folder); a summary lists the new, replaced and
  identical files before writing, and the machine's clipboard or undo already on the card are kept. The system
  updates (`SYSTEM/UPDATES`) are not shipped.
- The project folder must sit under the Files tree's root (the emulated card) for the program's paths to resolve
  as on the machine: when it does not, the root moves up to its parent.
- A hostile or damaged manifest cannot reach anything outside its folder (`..`, drives, absolute paths ignored).

### 5.11. Transfer with the Machine (MIDI)

Tools > Transfer with Machine... (`m8link.py`, `linkpanel.py`, `lib/LINK.SRC`): send, fetch and list the files on a
real Micro-8's SD card, through its MIDI ports and any USB-MIDI interface.

1. Copy `LINK.SRC` to the card (the **Copy LINK.SRC to the SD Card** button, or by hand), run it on the machine.
2. Cables: the PC's MIDI OUT to the Micro-8's MIDI IN, the Micro-8's MIDI OUT to the PC's MIDI IN.
3. Pick the output and input ports, **Test Connection**, then **List** (a card folder, double-click to enter a
   folder), **Send Files...**, **Send Project** (into the project's `/FOLDER`, subfolders included) or
   **Fetch Selected...**.

Without the machine: pick **Emulator**, **Start LINK in the Emulator**; everything works the same against the
emulator.

Why this protocol: a Lofi program only sees MIDI through `midiUpdt()`, a 16-event queue parsed by the firmware, and
SysEx never reaches it. So everything travels as Note messages: two bytes a message (note, velocity and their top
bits in the channel), 128-byte blocks with a checksum, acknowledgements, resends and automatic slowing down when the
machine's queue overflows. A received file goes to `LINK.TMP` first and replaces the target only once complete: an
interruption never damages it. Speed: about 1.3 KB/s (31.25 kbit/s), so a 48000-byte picture takes about 40 s.
Limits: files up to 64 KB (`fSize()` is a `nat`), 8.3 names.

The emulator models the firmware's MIDI input: 960 µs a message on the cable, a 16-event queue that drops the
overflow, `midiSend()` waiting for the UART (320 µs a byte). The MIDI keyboard chosen in Tools > MIDI Input also
plays into the running program, through `midiUpdt()`, as on the machine.

## 6. Icons & Narrow Windows

- **Hand-drawn icons** (`icons.py`), in the Spectra Workbench style: line-drawn glyphs on a 24×24 grid (1.6 px
  strokes, rounded caps), inked from the theme (light gray on dark, near-black on light), normal / hover /
  disabled states, no image files and no Unicode symbols in buttons. The toolbar, drawing tools, editors, tabs (a
  drawn dot marks a modified tab), Problems and the program outline all use them; they follow theme changes.
- **Narrow window**: every editor scrolls instead of being clipped, toolbars wrap onto several lines
  (`flow.py`), the sound editor stacks its areas (voices, operators, keyboard, oscilloscope) by width, the bottom
  panels scroll. `Ctrl+Shift+M` (Focus Mode) hides every panel.

## 7. Code Structure

`main.py` (entry point) · `splash.py` / `about.py` / `version.py` / `credits.json` (identity) · `prefs.py` ·
`intellisense.py` (symbols, completion, snippets, rename, format) · `window.py` (main window, menus, run,
debugging, tool registration) · `editor.py` (editor, gutter, search) · `lofi_syntax.py` (highlighting, byte ↔
editor conversion) · `panels.py` (Problems, Output, Outline, Files, Search, Variables, Watch, Breakpoints, Memory,
Hardware) · `icons.py` (glyphs) / `flow.py` (wrapping toolbars) · `pixeledit.py` (pixel core: tools, undo, canvas,
palette), `sprited.py` / `mapedit.py` / `bmpedit.py` / `soundedit.py` / `waveedit.py` / `datedit.py` /
`screendesign.py` / `editorbase.py` (asset editors) · `tracker.py` (tracker editor) / `trackergen.py` (generators,
Qt-free) / `trackertools.py` (Generate dialog, export) · `hwlink.py` (SysEx protocol, experimental) /
`hwlink_panel.py` (its panel) · `midiin.py` / `midiout.py` (real MIDI, winmm: short messages and SysEx) ·
`manual.py` (the Markdown viewer for both the hardware manual and this document) · `screen.py` (emulated screen) ·
`session.py` (the running program: threads, clock, sound) · `imgconv.py` / `imgtool.py` (conversion, UI) ·
`m8lib.py` (Micro-8 library) ·
`project.py` (projects) · `m8link.py` / `linkpanel.py` (MIDI transfer with the machine, `../lib/LINK.SRC` on the machine) ·
`theme.py`.

The engine itself is the `lofi/` package (analyzer, interpreter, machine, video, audio, debugger: see
`../README.md`).

## 8. Tests

    python tests/run_studio_tests.py <python with PySide6>     # 47 offscreen scripts: editors, projects, transfer, leaks...
    python tests/run_tests.py                                # engine: 73 programs, checker, audio, hardware, debugger

## 9. What's Validated

- **Lofi engine** (`tests/run_tests.py`): the 73 real SD-card programs compile and run without error; the
  compiler's robustness (`lofi/checker.py`) is fuzz-tested by mutation (thousands of trials against the real
  sources) and adversarial edge cases (extreme nesting, huge literals...), a pathological input reports a clean
  error, it never crashes the editor's live analysis.
- **Asset editors** (`tests/studio_assets_test.py` and dedicated scripts): byte-exact round trips against the real
  SD-card resources (sprites, tiles, bitmap) and against reference Lofi sources (TRACKER.SRC, TRACKER2.SRC)
  for the tracker and the screen designer.
- **Random fuzzing**: random key/mouse sequences (with FCTN/SHIFT/CTRL modifiers) replayed against the three Lofi
  programs (TRACKER, TRACKER2, PLAYER) and against the Studio tools (Screen Designer, Hardware Link),
  hundreds of trials with no crash to date.
- **Leaks** (`tests/studio_leak_test.py`): run/stop cycles (with and without sound) leave no thread, handle or
  memory behind; sessions are freed; 30 tabs and 60 asset editors (every kind, including Screen Designer) opened
  and closed with none surviving; Hardware Link send threads don't pile up and its dialog is freed properly; real
  MIDI port open/close cycles leak no handles (when a port is present on the machine). The audio driver (winmm) is
  released on every stop.
- **Responsiveness**: MIDI SysEx sending (Hardware Link) runs on a dedicated thread, never on the GUI thread, a
  slow or unplugged port no longer freezes the interface. Live analysis is a single pass (problems, outline, memory
  map): ~50 ms on the largest SD-card sources (TRACKER2, HOOGY) instead of three passes.

- **Damaged files**: every asset format (12) is reloaded truncated, empty, doubled, random and mutated (hundreds of
  variants), then redrawn and saved back: a clean error or a load, never a crash.

- **Compiled app**: in the shipped version, all the code of Studio (the tracker included) and of the engine is
  compiled to native code (61 modules); every suite also passes on that compiled version, and each build starts the
  shipped app to check that it plays a song and runs a program.
- **Native core (C++)**: the Lofi interpreter and the machine are C++, checked value for value against the former
  Python emulator during the port. Three builds (x86-64, AVX2, AVX-512) give the same frames and sound to the bit
  (`tests/native_core_test.py`). A check build that stops at the slightest C++ undefined behaviour (overflow,
  invalid shift, out-of-range access) passes the whole suite and extreme cases (`tests/native_edge_test.py`: huge
  integers and reals, negative sizes, coordinates far off the screen, `rnd()` over the whole 64-bit span equal to
  Python's). Endurance: hundreds of runs, errors, stops, F5 sessions with the real sound card and tracker
  playbacks without growing memory, handles or threads.
- **Emulator**: same behaviour, ~23% faster (integer arithmetic path, variables, lexer); X1 mode is tested against
  the wall clock (1690 ms of machine time = 1.7 s of real time for `X1BENCH.SRC`).
- **Saves**: the `.SCR` format (Screen Designer) carries a Fletcher-16 checksum verified on load, bit-identical on
  both sides (Studio and `micro8_tracker/SCREENED.SRC` (source removed from the repository)); a corrupted file is refused, an older file without the
  checksum still loads. See `SCREENED.SRC` (removed from the repository) for the format's details.

- **Hardware (FPGA source)**: the digital synth is rendered from a model of the HDL whose fast version is checked
  sample for sample against a line-by-line one (`tests/digisynth_test.py`); the 800x480 picture, priorities,
  scrolling, sprites, pointer, status registers and the 59.92 Hz frame rate are checked by
  `tests/fpga_video_test.py`; the font matches the FPGA ROM bit for bit.

## 10. Known Limitations

See `../README.md` (conventions guessed on the firmware side; the hardware follows the FPGA source, see "Fidélité au matériel").
The debugger works at line granularity (one statement at a time); `for(a;b;c)` does not stop on its three parts
separately. Hardware Link has no real firmware to talk to yet: tested against real MIDI ports with a solid
protocol on the Studio side, but without a genuine round trip (ping/pong) until a Micro-8 board or a MIDI loopback
cable is available. The MIDI transfer (§5.11) is validated against the model of the firmware's MIDI input; on the
real machine two unknowns are handled (channels counted from 0 or 1, a Lofi loop slower than the cable: resends
and slowing down) but not measured yet.
