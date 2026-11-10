# Micro-8 User's Manual

## Contents

- [Introduction](#introduction)
- [1. Overview](#1-overview)
  - [1.1. About](#11-about)
  - [1.2. Getting started](#12-getting-started)
  - [1.3. Demos & examples](#13-demos--examples)
  - [1.4. Built-in tools](#14-built-in-tools)
- [2. CLI](#2-cli)
  - [2.1. About the CLI](#21-about-the-cli)
  - [2.2. System commands](#22-system-commands)
  - [2.3. Interactive programming](#23-interactive-programming)
- [3. Graphics](#3-graphics)
  - [3.1. Display](#31-display)
  - [3.2. Colors](#32-colors)
  - [3.3. Mouse pointer](#33-mouse-pointer)
  - [3.4. Text](#34-text)
  - [3.5. Bitmap](#35-bitmap)
  - [3.6. Grid](#36-grid)
  - [3.7. Tiles](#37-tiles)
  - [3.8. Sprites](#38-sprites)
  - [3.9. Raster interrupts](#39-raster-interrupts)
- [4. Audio](#4-audio)
  - [4.1. YM2612-based Synthesizer](#41-ym2612-based-synthesizer)
  - [4.2. Built-in digital synthesizer](#42-built-in-digital-synthesizer)
  - [4.3. Visualizing audio](#43-visualizing-audio)
- [5. Memory](#5-memory)
  - [5.1. Memory organization](#51-memory-organization)
- [6. Coding in Lofi](#6-coding-in-lofi)
  - [6.1. Overview](#61-overview)
  - [6.2. Compiler and VM](#62-compiler-and-vm)
  - [6.3. Language constructs](#63-language-constructs)
  - [6.4. Lofi Known limitations](#64-lofi-known-limitations)
- [7. Lofi API](#7-lofi-api)
  - [7.1. Overview](#71-overview)
  - [7.2. I/O and System](#72-io-and-system)
  - [7.3. Memory](#73-memory)
  - [7.4. File system](#74-file-system)
  - [7.5. Math](#75-math)
  - [7.6. Video](#76-video)
  - [7.7. Character glyphs](#77-character-glyphs)
  - [7.8. Bitmap graphics](#78-bitmap-graphics)
  - [7.9. Tile graphics](#79-tile-graphics)
  - [7.10. Sprite graphics](#710-sprite-graphics)
  - [7.11. Audio](#711-audio)
  - [7.12. ADC](#712-adc)
- [8. Built-in tools](#8-built-in-tools)
  - [8.1. Text Editor](#81-text-editor)
  - [8.2. Compiler](#82-compiler)
  - [8.3. Image Editor](#83-image-editor)
  - [8.4. Sprite Editor](#84-sprite-editor)
  - [8.5. Tile Map Editor](#85-tile-map-editor)
  - [8.6. Sound Editor](#86-sound-editor)
  - [8.7. Music Editor](#87-music-editor)
- [9. Special I/O](#9-special-io)
  - [9.1. User port](#91-user-port)
  - [9.2. MIDI ports](#92-midi-ports)
  - [9.3. Hardware programming ports](#93-hardware-programming-ports)
- [Appendix A. Hardware](#appendix-a-hardware)
  - [A.1. Hardware bloc diagram](#a1-hardware-bloc-diagram)
  - [A.2. Input/Output Ports](#a2-inputoutput-ports)
- [Appendix B. Codes and Tables](#appendix-b-codes-and-tables)
  - [B.1. Glyph codes (hexadecimal)](#b1-glyph-codes-hexadecimal)
  - [B.2. Key codes (decimal)](#b2-key-codes-decimal)
  - [B.3. Keyboard matrix arrangement](#b3-keyboard-matrix-arrangement)
  - [B.4. YM2612 Synth Parameters](#b4-ym2612-synth-parameters)
  - [B.5. File system error codes](#b5-file-system-error-codes)
  - [B.6. More on CLI error messages](#b6-more-on-cli-error-messages)
- [Appendix C. Reserved Words](#appendix-c-reserved-words)
  - [C.1. Language Keywords](#c1-language-keywords)
  - [C.2. API](#c2-api)
- [Appendix D. SD Card](#appendix-d-sd-card)
  - [D.1. Default directory structure](#d1-default-directory-structure)
- [Appendix E. Memory Maps](#appendix-e-memory-maps)
  - [E.1. CPU Memory map](#e1-cpu-memory-map)
  - [E.2. XSRAM Default memory map](#e2-xsram-default-memory-map)
  - [E.3. XSRAM Alt memory map](#e3-xsram-alt-memory-map)
  - [E.4. Registers & buffers overview](#e4-registers--buffers-overview)
  - [E.5. Registers list](#e5-registers-list)
- [Appendix F. Troubleshooting](#appendix-f-troubleshooting)
- [Appendix G. Warranty](#appendix-g-warranty)
  - [Warranty Coverage](#warranty-coverage)
  - [Exclusions](#exclusions)
  - [Remedies](#remedies)
  - [Limitations](#limitations)
  - [Consumer Rights](#consumer-rights)
- [Appendix H. Compliance](#appendix-h-compliance)
  - [Regulatory Compliance](#regulatory-compliance)
  - [EU Declaration of Conformity (Simplified)](#eu-declaration-of-conformity-simplified)
  - [Disposal Notice](#disposal-notice)
- [Appendix I. Acknowledgements](#appendix-i-acknowledgements)
- [Appendix J. Afterword](#appendix-j-afterword)
- [Glossary](#glossary)
- [Acronyms](#acronyms)

---

*©2017-2026 Franck Sauer. All rights reserved. Version 1.0 Rev. F.*

# Introduction

> If you're serious about software, you make your own hardware.
> - Alan Kay

Congratulations, you have acquired what is certainly one of the least powerful recent computers available!

However, power is not everything. Featuring an 8-bit microprocessor clocked at about 15 MHz and backed by a modest 268 KiB[^A1] of RAM, Micro-8 is certainly not your average modern computer. Instead, the aim of this project has been to recreate the experience of owning a home computer during the 8-bit era and to revive what made each of these computers from the early '80s so unique, interesting, and accessible. With a couple modern twists.

Like its granddaddies, Micro-8 sports specialized audio and graphics hardware tailored to make cool little 2D games and applications. But most importantly, Micro-8 features a custom Operating System (OS) in ROM, with a built-in custom programming language (called Lofi), a comprehensive Application Programming Interface (API), and a suite of tools to help you make the most of the system from the get-go.

Zero boot time, no install, no fuss. Turn on and start creating.

Welcome back to the fun of discovering a new home computer as if it were the '80s again, a time when personal computing was an adventure waiting to be explored.

[^A1]: Back in the days, this would have been considered an insane amount of memory.

# 1. Overview

## 1.1. About

> The purpose of abstraction is to hide undesirable properties, not hide desirable ones.
> - Andrew S. Tanenbaum

Micro-8 is built around an Atmel ATMega128 8-bit Micro Controller Unit (MCU) running at 15.34 MHz. This MCU combines an 8-bit RISC CPU with internal memory and some Input/Output (I/O).

The CPU uses a Harvard architecture[^A2], with separate code and data memories, each with its own address space. This means that the OS (stored in internal flash ROM) won't eat into the data RAM space, leaving more space available for the user.

On top of the MCU's internal 4 KiB SRAM, there is a External SRAM (XSRAM) of 256 KiB for storing graphics data and user's programs. However, like most 8-bit CPUs, the RAM address bus is only 16-bits wide and thus can only see 64 KiB at a time.

The Memory, Audio, Graphics, and Interface Controllers are provided by MAGIC, a custom circuit implemented in a Field Programmable Gate Array (FPGA). MAGIC is also the host of system registers and important buffers. See Chapter 5 along with Appendices A and E for more details on architecture and memory mapping.

MAGIC provides both frame buffer graphics (also called bit-mapped graphics) and racing-the-beam[^A3] graphics such as hardware tiles and hardware sprites. All graphics are Color LookUp Table (CLUT)-based, and MAGIC support individual palettes for text, bitmap, tiles, and sprites. Two ports provide simultaneous analog and digital video outputs. See Chapter 3 for more information about graphics.

Micro-8 features two distinct sound synthesis circuits. The first uses a genuine Yamaha YM2612 chip[^A4] and provides 6 voices of FM synthesis. The second circuit is a hardware implementation of a small custom digital subtractive synthesizer in MAGIC. It comes with multiple waveforms, a built-in drum kit, a multimode resonant filter, programmable envelopes and LFOs, and provides two additional voices. See Chapter 4 for more information about audio.

Micro-8 comes with a built-in custom mechanical keyboard and supports a mouse. They allow for instant programming and content creation without the need for an external development PC or tools.

User's programs can be entered interactively with the Command Line Interpreter (CLI), as explained in Chapter 2, or by using the integrated Text Editor, as shown in Chapter 8. The built-in Lofi programming language, inspired by the venerable C language, along with an extensive Application Programming Interface (API), provides easy access to the machine's features. See Chapters 6 and 7 for more information about programming with Lofi.

Two game controllers can be used with the system. Each controller features a directional pad, select and start buttons, and two action buttons.

Micro-8 also comes with Musical Instrument Digital Interface (MIDI) IN and OUT ports for connecting to external music studio equipment. A micro-SD drive provides permanent storage and allows for file exchange with other systems. Finally, a user port provides 8 data lines to the outside world for interacting with custom hardware. More information on special I/O can be found in Chapter 9.

### What it is not

Due to its pure Harvard architecture, Micro-8 cannot execute native code from RAM. Writing native code requires re-flashing the ROM (erasing the OS in the process) using a specialized hardware programming device.

It is not compatible with any of the 8-bit computers of the era and has no native support for peripherals other than those mentioned above.

[^A2]: https://en.wikipedia.org/wiki/Harvard_architecture
[^A3]: A technique ubiquitous in the late 70's and early 80's that generates pixels on the fly as the video signal scans the display.
[^A4]: The same chip that powered the sound of the Sega Megadrive/Genesis console.

## 1.2. Getting started

### 1.2.1. Setup

Figure 1.1 shows a typical setup. If you encounter any problems with the setup of Micro-8, refer to the troubleshooting table in Section F.

> **Note:** No audio transits through video outputs. To benefit from the audio capabilities of Micro-8, please connect a stereo system to the Line-out port.

*Figure 1.1: Typical setup (power adapter, power switch, PS/2 mouse, game controller(s), digital or analog display, stereo sound system)*

> **Warning:** Game controller ports are serial and are not compatible with Atari joystick ports despite using a similar DE9 connector. Connecting an Atari-compatible controller will not work and may damage Micro-8!

See Appendix A.2 for more information on available I/O ports.

> **Note:** When turned on, do not leave Micro-8 unattended for long periods of time. When unused, turn the power off.

### 1.2.2. Keyboard usage

*Figure 1.2: Micro-8's built-in keyboard*

Although the general keyboard layout is pretty standard, a few specifics may require your attention:

- Front-left symbols are accessed using the `CTRL` key.
- Front-right symbols are accessed using the `ALT` key.
- There is no dedicated `CAPS LOCK` key. To toggle `CAPS LOCK`, press both `SHIFT` keys simultaneously.
- Press `CTRL` + `⇤` (Backspace) to switch between the insert (black cursor) and replace (dark cyan cursor) modes.
- Hold `CTRL` + `ALT` + `⇤` (Backspace) to perform a soft (warm) reset.

More shortcuts are available in the CLI environment. See Chapter 2 for more information about CLI.

> **Note:** Some keys or shortcuts may not apply when running specific applications. Check each application's details for more information.

## 1.3. Demos & examples

Before exploring the system in more depth, why not try some cool demos?

Type the following commands:

`cd /demos` followed by the `ENTER` key.

Type `dir` and hit `ENTER`.

You should now see the contents of the DEMO folder. This folder contains subfolders, one for each demo. Type `cd` followed by the name of the subfolder you want to explore and hit `ENTER`. Now type `dir` and hit `ENTER` again.

Executable files have the `.bin` extension. Type `run` followed by the demo name of your choice (including the `.bin` extension) and `ENTER` to start the demo.

You can hit the `ESC` (escape) key anytime to exit a demo.

Other interesting folders to explore include:

- `/EXAMPLES` Lots of small Lofi example programs to learn from.
- `/TOOLS` Tools written in Lofi that can be enhanced and customized by the user.
- `/GAMES` A small collection of games to get you started.

## 1.4. Built-in tools

Micro-8 features a suite of useful tools that are built into the OS or included as Lofi applications:

- **Text Editor** (Built-in): Allows for the creation and editing of text files, typically source code files.
- **Compiler** (Built-in): Allows a source file to be compiled into an executable file. The Compiler can also be called directly from the Text Editor.
- **Memory viewer** (Built-in): Allows one to dump a section of memory to the screen to examine its contents.
- **Image Editor** (Lofi application): A painting application for creating static bitmap images.
- **Sprite Editor** (Lofi application): A sprite drawing and animation editor for hardware sprites.
- **Tilemap Editor** (Lofi application): A tilemap editor for hardware tiles.
- **Sound Editor** (Lofi application): An editor for creating sounds for the YM2612 and the built-in subtractive synthesizer.
- **Music Editor** (Lofi application): A music sequencer for creating music tracks.

The included Lofi applications come with full source code and can be customized or extended by the user. Please check www.micro-8.com for availability and latest updates.

Please refer to Chapter 2 for a detailed description of the available commands to launch applications.

# 2. CLI

## 2.1. About the CLI

The Command Line Interpreter (CLI) interface is the main textual user interface. When Micro-8 is powered on, the user is welcomed with the CLI prompt.

From the CLI prompt, you can perform two main tasks:

1. Enter system commands
2. Enter Lofi program commands

System commands provide access to the File System, the built-in tools (such as the Text Editor) and the Lofi applications (such as tools, demos, and games).

All non-system commands are automatically sent to the compiler as Lofi program commands. Thus, small interactive programs can be entered and executed from within the CLI prompt in immediate mode. See Chapter 6 for more information about the Lofi programming language.

### 2.1.1. Keyboard shortcuts

- Holding `CTRL` and using `↑` move the cursor to the top of the screen.
- Holding `CTRL` and using `↓` move the cursor to the bottom of the screen.
- Holding `CTRL` and using `→` move the cursor to the end of the line.
- Holding `CTRL` and using `←` move the cursor to the beginning of the line.
- Holding `ALT` and using the `↑` and `↓` cursor keys will cycle the foreground color of the character.
- Holding `ALT` and using the `←` and `→` cursor keys will cycle the background color of the character.
- Holding right `SHIFT` and pressing `⇤` (Backspace) will perform a text clear.
- Holding left `SHIFT` and pressing `⇤` (Backspace) will perform a graphics clear (bitmap, tiles, and sprites).

## 2.2. System commands

### 2.2.1. File System

The file system uses the FAT32 format with 8.3 name size convention. Files can be read or written to a PC using a Micro-SD card reader. Filenames must be a maximum of 8 characters long, with an optional extension of a maximum of 3 characters (the same as MSDOS).

#### dir

Shows the contents of a directory.

```text
Usage: dir [path]
```

#### cd

Displays/changes the current directory.

```text
Usage: cd [/](Root) [.](Current) [..](Parent) [path]
```

#### label

Creates or changes the label of a SD card. The maximum size of a label is 8 characters.

```text
Usage: label name
```

#### mkdir

Creates a directory. The maximum size of a directory name is 8 characters.

```text
Usage: mkdir [path]name
```

#### del

Deletes a file or an empty directory.

```text
Usage: del [path]name
```

#### move

Moves or renames a file or directory.

```text
Usage: move [source path]name1 [destination path][name2]
```

Provided that the destination path ends with a `/`, the destination file name can be omitted.

Examples:

```text
move file1.src file2.src
```

Will rename the file `file1.src` located in the current folder to `file2.src`.

```text
move /user/file1.src /user/file2.src
```

Will rename the file `file1.src` located in the `/user/` folder to `file2.src`.

```text
move /user/file1.src /utils/
```

Will move the file `file1.src` from the `/user/` folder to the `/utils/` folder.

#### copy

Makes a copy of a file.

```text
Usage: copy [source path]name1 [destination path][name2]
```

Provided that the destination path ends with a `/`, the destination file name can be omitted.

Examples:

```text
copy file1.src file1.bak
```

Will copy the file `file1.src` to a file called `file1.bak`.

```text
copy file1.src /
```

Will copy the file `file1.src` to the root directory.

### 2.2.2. Configuration

#### data

Allows setting the Data segment origin for a Lofi program. The Data segment contains all globals (variables, arrays, functions, etc.). The default address is `0xB000`.

The address provided must be within the VM memory space, that is, above `0x3000` (where the code segment resides) and below `0xEFFF` (the end of the VM space). It is advisable not to go too extreme and leave enough space for both code and data (see the examples below). The new data origin address will be permanently stored in EEPROM memory.

```text
Usage: data [address]
```

If no address argument is provided, the current data segment address is displayed.

Examples:

| Command | Result |
|---|---|
| `data 0xC000` | 36 KiB for code, 12 KiB for data (this is the highest possible data position. Going above this limit might allow code to enter the compiler buffers) |
| `data 0xB000` | 32 KiB for code, 16 KiB for data (default) |
| `data 0x9000` | 24 KiB for code, 24 KiB for data |
| `data 0x7000` | 16 KiB for code, 32 KiB for data |

> **Warning:** This command must be used before compiling a Lofi program for it to take effect. This command initializes the compiler. All current globals will be lost.

> **Note:** When changing the data segment origin, and because the address is saved in EEPROM, all subsequent programs will be compiled with the new configuration, even after a reset or power off. To restore the default configuration, type `data 0xB000`.

See Chapter 5 and Appendix E for more information about memory organization in Micro-8.

#### flash

This command performs a self-reprogramming of the entire Operating System (OS) (stored in flash memory; see Chapter 5 for more information on memory) and allows upgrading / downgrading of the OS version. The system images for the new OS versions should be placed in the `/SYSTEM/UPDATES` folder. Micro-8 comes with the system image of the original OS stored at that location.

The system image file naming convention is as follows: `SYSTEM.xxx` with the extension indicating the version number.

Once updated, the start screen will display the current version in the x.x.x format, each of the three digits representing the Major release, the Minor release, and the Fix versions, respectively. For example, the extension `123` represents version 1.2.3.

The version convention is as follows:

- **Major**: A major system release with brand new features.
- **Minor**: Possible changes in the system.
- **Fix**: Bug fixes or small improvements.

```text
Usage: flash fileName
```

> **Note:** Before applying the update, a CRC check is performed. If the provided file is not a valid system update file or if, for any reason, the file integrity has been compromised, the update will be canceled, and the computer will prompt the user to restart.

> **Warning:** Do not turn off your computer while the system is updating (updating the system takes about 5 seconds). Interrupting the update procedure will brick your system and will require re-flashing with specialized hardware.

See also the `_ver` built-in variable, as explained in Chapter 7.

### 2.2.3. Tools

#### edit

Launches the Text Editor and creates or opens the provided text file as an argument. The maximum size of a file name is 8 characters + 3 characters for the extension.

```text
Usage: edit fileName
```

The Text Editor is the go-to tool for writing larger programs. See Chapter 8 for a detailed description of the Text Editor.

#### compile

Compiles a Lofi text source file (`.src` extension) into an executable bytecode (`.bin` extension). After compilation, the Symbols, Code, and Data space utilization is displayed (see Section 8.2 for more information).

```text
Usage: compile fileName
```

#### load

The command loads an executable bytecode file (`.bin` extension) into memory. The loading of an executable erases any previous program from memory.

Executable files are loaded at VM address `0x3000`. See Chapter 5 and Appendix E for more information about memory organization in Micro-8.

```text
Usage: load fileName
```

#### run

Either executes a previously loaded executable (`.bin` extension) or loads and executes the file provided as an argument.

Executable files are loaded at VM address `0x3000`. See Chapter 5 and Appendix E for more information about memory organization in Micro-8.

```text
Usage: run [fileName]
```

#### globals

Displays global variables (Symbol table). Globals can be viewed anytime when in immediate mode or right after compiling a source file.

```text
Usage: globals
```

#### new

Resets the Lofi compiler. Clears all globals.

```text
Usage: new
```

#### dump

The command dumps memory to the display. The memory content is shown in hexadecimal and ASCII. 16 values will always be shown per line.

```text
Usage: dump start address length
```

See Chapter 5 and Appendix E for more information about memory organization in Micro-8.

## 2.3. Interactive programming

You can interactively enter Lofi declarations and statements from the CLI prompt in immediate mode. Any global declaration will remain in memory until a `new` command is entered, a soft or hard reset is performed, or an external program is loaded into memory. See Chapter 6 for more information about the Lofi programming language.

### 2.3.1. Example programs in immediate mode

#### Simple counter

The following program declares a global user function called `count()` that performs counting from 1 to the provided parameter (inclusive) and prints the result on each line.

```c
void count(int n){for(int i=1; i<=n; i++)print(i,"⏎");}
```

> **Note:** The new line `⏎` character can be obtained by holding the `FCTN` key and pressing the `ENTER` key.

After entering the above line, the `count()` function can be invoked interactively from the CLI prompt along with a corresponding parameter (integer values 10 and 20 in the example below). When hitting the `ENTER` key, the function is invoked and counting is performed.

```c
count(10); //performs counting from 1 to 10
count(20); //performs counting from 1 to 20
```

#### Random circles

In the following example, we will declare a function that draws a circle of random position, radius, and color:

```c
void rndCircle() {circle(rnd(0,399),rnd(0,239),rnd(1,100),rnd(0,15));}
```

Then we will call that function from within a loop to draw a hundred random circles:

```c
for(int i; i<100; i++) rndCircle();
```

To clear the bitmap screen, you can type:

```c
box(0,0,399,239,0);
```

#### Classic 10 PRINT maze

This last example implements the classic BASIC 10 print... maze[^A5] in a single Lofi line:

```c
text t={1}; while(_keyDwn!=12) {poke(&t,0x0e+rnd(0,1)); print(t);}
```

Press the `ESC` (escape) key anytime to exit.

Larger programs can be entered and compiled from the built-in Text Editor. See Chapter 8 for more information about the Text Editor.

[^A5]: 10print.org

# 3. Graphics

## 3.1. Display

### 3.1.1. Resolution

Micro-8 features a fixed display resolution of 800×480 pixels (WVGA[^B1]) at a 60 Hz refresh rate, with a 15:9 aspect ratio that is suitable for 16:10 (good) and 16:9 (best) modern displays[^B2].

> **Note:** When using digital video, be sure to set your TV/monitor to "Exact scan" (if the feature is present). When using analog video, be sure to use the "Auto adjust" feature in the TV/monitor menu.

To conserve resources and stay true to the 8-bit spirit of the system, most graphics are displayed at half resolution (400×240 pixels).

[^B1]: https://en.wikipedia.org/wiki/Display_resolution_standards
[^B2]: Given the low resolution, we recommend 12 to 17 inches monitors for the best viewing experience.

### 3.1.2. Video output

Video is output simultaneously through the digital and analog ports at the rear of the machine. See Appendix A.2 for more information about I/O Ports. Having two video ports allows, for example, connecting a monitor and a capture device at the same time.

### 3.1.3. Layers

The display is made of a series of superimposed graphics layers with a default order as shown in Figure 3.1.

```text
(top)
  Mouse pointer
  Text
  Sprites
  Tiles
  Sprites
  Grid
  Bitmap
(bottom)
```

*Figure 3.1: Micro-8's graphics layers*

Graphics layers can be controlled by the use of the following register:

| Address | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|---|---|---|---|---|---|---|---|---|
| `0x2F00` | Overlay | Grid | Bmp tr. | Text | Tiles | Sprites | Tile Op. | Bitmap |

- Bit 7 Enable/disable overlay split bitmap. When this bit is set, pixels that use the color indexes [8-15] of the bitmap layer are drawn on top of tiles and sprites. Pixels using color indexes [0-7] are drawn to the lowest layer. Off by default.
- Bit 6 Enable/disable Grid. Off by default.
- Bit 5 Enable/disable Bitmap transparency. Off by default. When this bit is set, pixels with color index 0 in a bitmap become transparent and reveal the background color.
- Bit 4 Enable/disable Text display. On by default.
- Bit 3 Enable/disable Tiles display. Off by default.
- Bit 2 Enable/disable Sprites display. Off by default.
- Bit 1 Enable/disable Tile opacity. Off by default. When this bit is set, pixels with color index 0 in a tile are no longer transparent and the entire tile 0 becomes transparent instead. Tile 0 can then be used to mask entire areas in the display while other tiles are totally opaque.
- Bit 0 Enable/disable Bitmap display. On by default.

## 3.2. Colors

### 3.2.1. Color space

Colors are defined by RGB component values. Each color component is defined using 3 bits (8 different values). Thus, the color space is 8^3 = 512 possible colors.

A component is stored in a byte, but only the 3 lower bits are significant. A single color definition takes 3 bytes:

| Byte | Bits | Field |
|---|---|---|
| R | 7:3 | Unused |
| R | 2:0 | Component value |
| G | 7:3 | Unused |
| G | 2:0 | Component value |
| B | 7:3 | Unused |
| B | 2:0 | Component value |

### 3.2.2. Palettes

Colors are stored in Color LookUp Table (CLUT)s also called Palettes. Each Palette has 16 entries and thus is referenced by a 4-bit index (e.g. stored in a pixel). A palette takes 48 bytes of memory arranged in the following way: 16 bytes of Red values followed by 16 bytes of Green values followed by 16 bytes of Blue values.

Each graphics layer possesses its own palette, with the exception of Sprites that have two selectable palettes.

> **Note:** By default, and for all layers but the bitmap layer, the color value of index 0 is irrelevant as index 0 is used for transparency, and the rendered pixel will be of whatever value the layer underneath happens to be.

### 3.2.3. Background

When pixel values from all layers are transparent (or layers are disabled), the background color is shown. The background color is defined by the following three registers:

| Address | Bits | Field |
|---|---|---|
| `0x2F13` | 7:3 | Unused |
| `0x2F13` | 2:0 | R Component value |
| `0x2F14` | 7:3 | Unused |
| `0x2F14` | 2:0 | G Component value |
| `0x2F15` | 7:3 | Unused |
| `0x2F15` | 2:0 | B Component value |

## 3.3. Mouse pointer

The mouse pointer is on the topmost display layer. The mouse pointer consists of a single 16×16 pixel hardware sprite displayed at full display resolution (800×480 pixels).

The mouse pointer can be enabled or disabled using bit 7 of the status register at address `0x2F0B`. It is enabled by default.

| Address | Bits | Field |
|---|---|---|
| `0x2F0B` | 7 | pointer enable |
| `0x2F0B` | 6:3 | Unused |
| `0x2F0B` | 2 | tileDB |
| `0x2F0B` | 1 | cntRes |
| `0x2F0B` | 0 | vBlank |

The mouse pointer shape can be changed by using the following command:

```text
ptrShape(shape)
```

The `shape` parameter can be one of the following:

0. Arrow (default)
1. Reticle
2. Vertical bar
3. Hand

The color of the mouse pointer can be changed by setting the following registers:

| Address | Bits | Field |
|---|---|---|
| `0x2F10` | 7:3 | Unused |
| `0x2F10` | 2:0 | R Component value |
| `0x2F11` | 7:3 | Unused |
| `0x2F11` | 2:0 | G Component value |
| `0x2F12` | 7:3 | Unused |
| `0x2F12` | 2:0 | B Component value |

## 3.4. Text

Text is displayed on top of all other layers, except for the mouse pointer. The primary usage of the text generator is to allow the user to enter commands and code (while possibly seeing the graphics underneath). However, text display can also be used in applications themselves as text output (e.g. using the `print()` function) or by directly addressing the appropriate buffers.

The character generator outputs text at full display resolution (800 × 480 pixels). The text area consists of a matrix of 40 rows of 80 character columns, as shown in Figure 3.2.

```text
        0 .......................... 79   (80 columns)
   0  +------------------------------+
      |                              |
      |                              |
  39  +------------------------------+
  (40 rows)
```

*Figure 3.2: Text character matrix*

The character matrix is stored in two distinct buffers, the character glyph[^B3] index buffer and the character color index buffer.

[^B3]: The term 'glyph' is a synonym for 'meaningful symbol'. https://en.wikipedia.org/wiki/Glyph

### 3.4.1. Glyphs

The character glyph index buffer stores a byte index that points to one of the 256 glyphs available in the glyph set. The start address of the character glyph index buffer is `0x1200` and it has a size of 80×40 = 3200 bytes.

The character generator reads glyph pixels from FPGA's internal SRAM. At power-up, the glyph set is initialized with the default data shown in Appendix B.

The first half of the default glyph set follows loosely the American Standard Code for Information Interchange (ASCII) standard. The second half defines graphics symbols, similar to PETSCII[^B4] or IBM's extended ASCII[^B5] yet is totally original. These symbols provide an easy and economical way for building functional graphics such as windows and other interface elements.

Each glyph is made of 12 lines of 10 pixels. The entire glyph set is 160×192 pixels and is stored linearly in a 1-bit wide 30720-bit long memory. This memory is not mapped to the CPU address space. It can be addressed by setting a 15-bit address using the following registers:

| Address | Bits | Field |
|---|---|---|
| `0x2FD0` | 7:0 | Pixel address low |
| `0x2FD1` | 7 | Unused |
| `0x2FD1` | 6:0 | Pixel address high |

To write a pixel value, the following register can be used. When the WREN bit [0] is set, the value bit [1] is written to the specified address.

| Address | Bits | Field |
|---|---|---|
| `0x2FD2` | 7:2 | Unused |
| `0x2FD2` | 1 | Value |
| `0x2FD2` | 0 | WREN |

[^B4]: https://en.wikipedia.org/wiki/PETSCII
[^B5]: https://www.ascii-codes.com/

It is not necessary to write to the above registers to define new glyphs. For simplicity, the `glyphSet()` API function is provided as shown in Section 7.7.

> **Note:** The glyph set memory is write-only. It is also persistent across resets. To restore the glyph set to its default state, a power cycle is necessary.

### 3.4.2. Colors

The character generator possesses its own 16 colors palette:

| Start address | Component |
|---|---|
| `0x1140` | Red |
| `0x1150` | Green |
| `0x1160` | Blue |

The default characters palette is as follows:

| Color Index | R | G | B | Color |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | Transparent |
| 1 | 0 | 0 | 0 | Black |
| 2 | 7 | 7 | 7 | White |
| 3 | 3 | 0 | 3 | Purple |
| 4 | 0 | 4 | 7 | Light Blue |
| 5 | 0 | 0 | 4 | Dark Blue |
| 6 | 2 | 2 | 2 | Dark Grey |
| 7 | 0 | 7 | 7 | Cyan |
| 8 | 0 | 2 | 3 | Dark Cyan |
| 9 | 0 | 7 | 0 | Bright Green |
| A | 0 | 3 | 0 | Dark Green |
| B | 7 | 7 | 0 | Yellow |
| C | 7 | 4 | 0 | Orange |
| D | 7 | 0 | 0 | Bright Red |
| E | 4 | 0 | 0 | Dark Red |
| F | 4 | 4 | 4 | Middle Grey |

The character color index buffer has a similar matrix arrangement as the one shown in Figure 3.2. Each element stores a byte, of which the low and high nibbles define the character foreground and background color index, respectively:

| Bits | Field |
|---|---|
| 7:4 | Background color index |
| 3:0 | Foreground color index |

The start address of the character color index buffer is `0x1E80` and it has a size of 80×40 = 3200 bytes.

## 3.5. Bitmap

Bitmap graphics is output at half display resolution (400 × 240 pixels) with a top-left origin, as shown in Figure 3.3. Bitmap graphics use linear buffer(s).

```text
        0 .......................... 399   (400)
   0  +------------------------------+
      |                              |
      |                              |
 239  +------------------------------+
  (240)
```

*Figure 3.3: Bitmap graphics display*

Each pixel is defined by a four-bit index (a nibble) to the bitmap palette and thus can reference one of the sixteen RGB colors defined in the bitmap palette:

| Start address | Component |
|---|---|
| `0x11A0` | Red |
| `0x11B0` | Green |
| `0x11C0` | Blue |

The default bitmap palette is as follows:

| Color Index | R | G | B | Color |
|---|---|---|---|---|
| 0 | 5 | 5 | 5 | Middle grey |
| 1 | 0 | 0 | 0 | Black |
| 2 | 7 | 7 | 7 | White |
| 3 | 4 | 0 | 5 | Violet |
| 4 | 0 | 3 | 7 | Blue |
| 5 | 0 | 0 | 4 | Dark Blue |
| 6 | 7 | 3 | 5 | Rose |
| 7 | 0 | 7 | 7 | Cyan |
| 8 | 0 | 3 | 4 | Dark Cyan |
| 9 | 0 | 7 | 0 | Bright Green |
| A | 0 | 4 | 0 | Dark Green |
| B | 7 | 7 | 0 | Yellow |
| C | 7 | 4 | 0 | Orange |
| D | 7 | 0 | 0 | Bright Red |
| E | 4 | 0 | 0 | Dark Red |
| F | 2 | 2 | 2 | Dark Grey |

A full bitmap takes 400×240×1/2 = 48000 bytes of memory (not counting the palette itself).

The bitmap draw buffer origin (the buffer where the graphics commands from the API draw to) can be changed using page register `0x2F16`.

The bitmap display buffer origin (the buffer whose pixels are scanned out to the video display output) can be changed using the page register `0x2F05`.

Together, these two registers allow the creation of double buffering for smooth bitmap animation. By default, both registers have the same `0x60` page value which sets the base address for bitmap graphics at `0x18000` in the XSRAM. This default single buffer configuration is suitable for static bitmap graphics as the draw buffer happens to also serve as the display buffer. See Chapter 5 and Appendix E for more information about memory mapping.

An extensive collection of bitmap graphics functions can be found in the API as described in Section 7.8.

## 3.6. Grid

On top of the bitmap layer, and below all other layers, comes the hardware grid. The grid is drawn at full 800×480 resolution using a dotted pattern and inverse blending in order to be as non-intrusive as possible. The grid is purely hardware and procedural and comes at zero cost on the CPU side. The grid space is fixed at 32×32 full resolution (equivalent to 16×16 bitmap low-resolution pixels). It can be used as a help for drawing tile sets or other graphics and can be seen at work in the included ImageEd bitmap editor.

## 3.7. Tiles

A hardware tile is a small, fixed-size graphic that the graphics hardware reuses to build larger backgrounds. Instead of storing every pixel of the entire screen in memory, the display is described by a tile buffer, which is a grid of references to tile images stored in memory.

This approach, similar to text display, greatly reduces memory usage: a single tile graphic can appear many times on screen just by reusing its entry in the tile buffer. For example, one "brick" tile can be repeated across an entire wall, without needing to duplicate the pixel data.

Unlike bitmap graphics, which store every pixel explicitly, hardware tiles provide a compact and flexible way to represent patterned graphics and are one of the key techniques in classic console and arcade game design.

In Micro-8, tile graphics is output at half display resolution (400 × 240 pixels). Tiles have a fixed size of 16x16 pixels. Each pixel is a 4-bit index to the tile palette. A tile is thus 128 bytes. A complete tile-set of 256 tiles takes 32 KiB.

Tile graphics has its own 16-entry palette:

| Start address | Component |
|---|---|
| `0x1170` | Red |
| `0x1180` | Green |
| `0x1190` | Blue |

The tiles palette is initialized with the following values:

| Color Index | R | G | B | Color |
|---|---|---|---|---|
| 0 | 5 | 5 | 5 | Transparent |
| 1 | 0 | 0 | 0 | Black |
| 2 | 7 | 7 | 7 | White |
| 3 | 4 | 0 | 5 | Violet |
| 4 | 0 | 3 | 7 | Blue |
| 5 | 0 | 0 | 4 | Dark Blue |
| 6 | 7 | 3 | 5 | Rose |
| 7 | 0 | 7 | 7 | Cyan |
| 8 | 0 | 3 | 4 | Dark Cyan |
| 9 | 0 | 7 | 0 | Bright Green |
| A | 0 | 4 | 0 | Dark Green |
| B | 7 | 7 | 0 | Yellow |
| C | 7 | 4 | 0 | Orange |
| D | 7 | 0 | 0 | Bright Red |
| E | 4 | 0 | 0 | Dark Red |
| F | 2 | 2 | 2 | Dark Grey |

Tiles are stored in the tile set section of the XSRAM memory, and referenced from a double-buffered tile buffer in the internal FPGA SRAM (See Chapter 5 and Appendix E for more information about memory mapping). In a typical double-buffer scenario, one tile buffer is used for display (scan out) while the other is being constructed. Tile buffer pointers are typically swapped during vertical blanking.

Each tile reference in a tile buffer is 8-bit. The tile set can thus contain a maximum of 256 tiles. Each tile buffer contains 32×16 tile references (of which an area equivalent to 25×15 tiles is visible at any given moment, but can overlap 26×16 tiles depending on scrolling offset values) as shown in Figure 3.4.

```text
        0 ............... 25 ........ 31
   0  +-------------------+----------+
      |                   |          |
      |     Display       |  Unused  |
      |   (25x15 tiles)   |          |
  15  +-------------------+----------+
  (sub-pixel horizontal offset / sub-pixel vertical offset
   shift the display window within the 32x16 tile buffer)
```

*Figure 3.4: Tile graphics*

The origin of the tile-set can be set with page register `0x2F03`. Default value is `0x00` and points to memory location `0x00000` in the XSRAM.

Tile graphics feature sub-pixel hardware scrolling. The displayed tile buffer can be offset horizontally and vertically at full resolution (800×480 pixels). That is, with two sub-pixels per tile pixel. This allows for very smooth scrolling, particularly at low speed.

The 32 sub-pixel offsets in the horizontal and vertical directions are specified using the following registers:

| Address | Bits | Field |
|---|---|---|
| `0x2F01` | 7:5 | Unused |
| `0x2F01` | 4:0 | Horizontal tile offset |
| `0x2F02` | 7:5 | Unused |
| `0x2F02` | 4:0 | Vertical tile offset |

Tile-related functions can be found in the API as described in Section 3.7.

## 3.8. Sprites

A hardware sprite is a small, independent image that the graphics hardware can draw over a background without altering it. Because the hardware draws sprites automatically during the refresh of each video frame, moving a sprite is extremely light. A single change in its position registers, and the sprite appears in the new location without having to move data or redraw the background. This technique was common in classic game consoles and computers, where it allowed smooth object movement with minimal CPU usage.

In Micro-8, sprite graphics are output at half the display resolution (400×240 pixels). 32 hardware sprite instances are available simultaneously. Each sprite has a fixed resolution of 24×21 pixels. Each pixel is a 4-bit index to one of the two sprite palettes available per sprite instance.

A sprite takes 256 bytes of memory, but the pixel data only accounts for 252 bytes. The 4 remaining bytes are used to store a user-defined Axis Aligned Bounding Box (AABB) collision box as shown in Figure 3.5. The included Sprite Editor provides an easy-to-use tool for editing AABB. See Section 8.4 for more information about the Sprite Editor.

```text
        0 ........... 23
   0  +--------------+
      |   +------+   |
      |   | AABB |   |
      |   +------+   |
  20  +--------------+
```

*Figure 3.5: A Sprite with a potential AABB*

Each of the 32 sprite instances can select one of the two available 16-entry palettes:

Sprites palette A

| Start address | Component |
|---|---|
| `0x11D0` | Red |
| `0x11E0` | Green |
| `0x11F0` | Blue |

Sprites palette B

| Start address | Component |
|---|---|
| `0x2FA0` | Red |
| `0x2FB0` | Green |
| `0x2FC0` | Blue |

Both sprites palettes are initialized with the following values:

| Color Index | R | G | B | Color |
|---|---|---|---|---|
| 0 | 5 | 5 | 5 | Transparent |
| 1 | 0 | 0 | 0 | Black |
| 2 | 7 | 7 | 7 | White |
| 3 | 4 | 0 | 5 | Violet |
| 4 | 0 | 3 | 7 | Blue |
| 5 | 0 | 0 | 4 | Dark Blue |
| 6 | 7 | 3 | 5 | Rose |
| 7 | 0 | 7 | 7 | Cyan |
| 8 | 0 | 3 | 4 | Dark Cyan |
| 9 | 0 | 7 | 0 | Bright Green |
| A | 0 | 4 | 0 | Dark Green |
| B | 7 | 7 | 0 | Yellow |
| C | 7 | 4 | 0 | Orange |
| D | 7 | 0 | 0 | Bright Red |
| E | 4 | 0 | 0 | Dark Red |
| F | 2 | 2 | 2 | Dark Grey |

Each of the 32 sprite instances maintains an 8-bit reference to the sprite image in the sprite set, allowing a maximum of 256 different sprite images accessible from memory. A complete sprite set takes 64 KiB.

The origin of the sprite-set can be set with the page register `0x2F04`. The default value is `0x20` and points to memory location `0x08000` in the XSRAM.

Similarly to tiles scrolling, sprite position is expressed at full resolution (800×480). This allows sub-pixel positioning and smooth movements. There is no visibility attribute for sprites. To hide a sprite, simply move it to an invisible position such as (0,0). The visible area for sprites is shown in Figure 3.6.

```text
Sprite position space: 0..1023 (X) x 0..1023 (Y)
Visible sprites area:  X 48..847, Y 42..521 (800x480)

(0,0)
  +--------------------------------------------+
  |                                            |
  |   (48,42)                                  |
  |     +------------------------------+       |
  |     |    Visible sprites area      |       |
  |     |                              |       |
  |     +------------------------------+       |
  |                            (847,521)       |
  |                                            |
  +--------------------------------------------+
                                        (1023,1023)
```

*Figure 3.6: Sprites positioning and visible area*

Each sprite instance can be flipped in hardware in the X and Y directions. This has the potential to reduce memory usage if the art style allows.

Each sprite instance can be set to be displayed above or behind the tile layer using a priority attribute.

The overlapping order of sprite instances display is fixed (from 0 below to 31 above). It is up to the user to make use of the appropriate instance indices in order to obtain the desired overlapping behavior.

Sprites attributes are not double buffered (at the hardware level). Instead, the attributes that are the most likely to be modified regularly are deferred to a buffered list when modified by the user during the frame (e.g. using `sprPos()` function). The user can then invoke the `sprUpdt()` function to update the actual sprite instances' attribute registers, typically during vertical blanking. The following table shows which of the sprite instance attributes are deferred or not:

| Attribute | Behavior |
|---|---|
| Position X,Y | Deferred |
| Flip X,Y | Deferred |
| Reference | Deferred |
| Priority | Immediate |
| Palette | Immediate |

If for whatever reason it is required to modify a deferred attribute immediately (such as in the case of sprites multiplexing), the raw sprites registers can be used instead of the API functions.

An extensive collection of sprite-related functions can be found in the API as described in Section 3.8.

## 3.9. Raster interrupts

### 3.9.1. Principles of operation

Raster interrupts are an advanced technique based on the raster-display scanning mechanism (shown in Figure 3.7). By detecting a specific scan line and interrupting the main Lofi execution thread, special effects such as Raster bars, split screen, sprite multiplexing,... and more can be achieved.

*Figure 3.7: Raster display scanning mechanism*

MAGIC features a raster line comparator capable of triggering a hardware interrupt at a given vertical position (480 visible scan lines + 32 invisible lines) during the video refresh of the display.

When a raster interrupt is triggered, the handler executes a list of instructions stored in a display list.

The display list instructs the processor what to do during the interruption. It can:

- Halt the interrupt handler and return to the main thread.
- Perform basic operations on memory (such as writing a value to a specific register).
- Set the next-line position for the comparator. When the video refresh mechanism reaches that new line position, a new interrupt is triggered.
- Set the address of the next list of instructions to be performed at the next interrupt (it can be another address of the current list or the address of another list entirely).
- Wait for a tiny fraction of time doing nothing (this allows for precise horizontal timing control, required for certain effects).

The first instruction of the display list will be executed at the start of the horizontal blanking of the preceding line. Up to two raster instructions can be executed during horizontal blanking.

Before a display list can be executed, raster interrupts have to be enabled and the entry point to the display list must be passed to the handler. Please, see the API at Section 7.6 for more information.

### 3.9.2. Raster instructions

#### Creating a display list

Instruction format (32 bits):

| Bits | Field |
|---|---|
| 31:24 | Op code |
| 23:8 | word (address or value) |
| 7:0 | value |

The instruction can easily be mapped to a custom type in Lofi as follows:

```c
type rasterInstr
{
    byte opcode;
    nat word;
    byte val;
};
```

Then, the list can be created using a simple array like so:

```c
rasterInstr list[10]={ {opcode, word, value}, ...};
```

See examples in the `/examples/graphics/` folder.

#### Available instructions

| Opcode | Code | Word | Value | Role |
|---|---|---|---|---|
| HALT | 0 | n/a | n/a | End handler execution, return from interrupt |
| SET | 1 | address | value | Set a value at a particular address |
| INC | 2 | address | n/a | Increment the value at a particular address |
| DEC | 3 | address | n/a | Decrement the value at a particular address |
| ADD | 4 | address | value | Add the value to the one at the address |
| SUB | 5 | address | value | Subtract the value from the one at the address |
| OR | 6 | address | value | OR the value with the one at the address |
| AND | 7 | address | value | AND the value with the one at the address |
| POS | 8 | line | n/a | Set the line position for the next comparison |
| NEXT | 9 | address | n/a | Set the address of the next display list |
| NOP | 10 | n/a | n/a | Do nothing |

#### Gotchas

> **Warning:** Working with raster interrupts can be tricky, and the interrupt handler will happily write garbage to memory (including system RAM) if the display list is not carefully thought out.

Beware of reentrant interrupts. Give enough time to the handler to complete the current list by not having a new line comparison set too close to the current one (depending on the complexity of the current display list).

In case of dynamically updated display lists, beware of race conditions. Ideally, a display list should be double buffered, and the switch to the active display list should be placed at a safe place where no current interrupt is running, such as during vertical blanking.

# 4. Audio

The audio is generated from two separate sources. A genuine YM2612 chip from Yamaha provides Frequency Modulation (FM) synthesis, and the MAGIC-based built-in digital synthesizer provides sample-based and subtractive synthesis. Then both audio sources are mixed in hardware and output to the line-out port. The mixed audio is also fed back to the MCU Analog to Digital Converter (ADC) and the sampled data can be used by the CPU for special purposes, such as waveform visualization.

## 4.1. YM2612-based Synthesizer

### 4.1.1. Overview

The YM2612-based synthesizer is a higher-level synthesizer based on the YM2612 chip and is accessible through user-friendly API functions (see Section 7.11.1 for more information). The synthesizer holds a bank of up to 10 instruments that can be loaded individually at any given time. Each instrument stores the 54 parameters that define the sound (see Appendix B for the list of parameters). Each of the 6 hardware voices of the YM2612 chip can reference an instrument from the bank. Each voice offers stereo output (left, right, or center), independent volume, and octave selection. The synth also features a global volume setting and can use the built-in hardware sine Low Frequency Oscillator (LFO).

### 4.1.2. Programming model

The registers that control the YM2612 chip are internal and not memory-mapped. The YM2612-based synthesizer can be easily programmed using the provided API functions, as shown in Section 7.11. When direct low-level access to the chip is required, the `ymSetReg()` API function can be used.

### 4.1.3. Voices

Each of the six hardware voices is made up of up to four operators, configured in one of the eight available 'algorithms,' as shown in Figure 4.1. Each configuration provides the basis for constructing a particular timbre by feeding the input of one operator with the output from another operator. The operators shown with a dark box are summed to form the final waveform. The first operator can feed back into itself, and the amount of feedback is selectable.

```text
Algo  Operator routing (-> feeds into, + summed)   Carriers (summed to output)
 0    1 -> 2 -> 3 -> 4                              4
 1    (1 + 2) -> 3 -> 4                             4
 2    (1 + (2 -> 3)) -> 4                           4
 3    ((1 -> 2) + 3) -> 4                           4
 4    (1 -> 2) + (3 -> 4)                           2, 4
 5    1 -> 2, 1 -> 3, 1 -> 4                        2, 3, 4
 6    (1 -> 2) + 3 + 4                              2, 3, 4
 7    1 + 2 + 3 + 4                                 1, 2, 3, 4
Operator 1 has a selectable self-feedback loop in every algorithm.
```

*Figure 4.1: FM operators configuration 'algorithms' (carriers are the dark boxes of the original figure)*

The original Yamaha documentation suggests the following as a starting point for algorithm usage:

0. Distortion guitar, hi-hat chopper, bass
1. Harp, Programmable Sound Generator (PSG) type sounds
2. Bass, electric guitar, brass, piano, woodwinds
3. Strings, folk guitar, chimes
4. Flute, bells, chorus, bass drum, snare drum, tom-tom
5. Brass, organ
6. Xylophone, tom-tom, organ, vibraphone, snare drum, bass drum
7. Pipe organ

Each voice can be modulated by the LFO and has individual frequency and amplitude modulation factors.

### 4.1.4. Operators

Each operator possesses its own envelope (as shown in Figure 4.2), along with a handful of parameters:

- Whether the operator is on or off
- Whether the LFO modulates the amplitude or not
- A frequency multiplier to create harmonic content
- A rate scaling parameter (similar to the 'key follow' parameter found in other synthesizers)
- A detune parameter

*Figure 4.2: FM operator envelope (Total Level, Attack rate, Decay rate (D1R), Sustain level, Sustain rate (D2R), Release rate, between Note On and Note Off)*

## 4.2. Built-in digital synthesizer

### 4.2.1. Overview

MAGIC features a small custom two-voice, stereo, subtractive digital synthesizer, as shown in Figure 4.3. The synthesizer features two oscillators with individual amplification stages, a multi-mode resonant State Variable Filter (SVF), and two hard panners. It additionally includes two envelope generators and two Low Frequency Oscillator (LFO)s that can be mapped by the user to various modulation ports. These hardware modulation sources eliminate the necessity of using precious CPU resources to perform the most common modulation effects.

### 4.2.2. Programming model

Sixteen specific registers are used to control the built-in synthesizer. All registers are memory-mapped starting at address `0x1110` and can be accessed using regular API memory functions such as `peek()` and `poke()`. Likewise, saving or loading a configuration simply requires exporting or importing the appropriate memory locations to/from a file. However, specific API functions that provide access to the most common features are included for convenience. See Section 7.11.2 for more information.

```text
Audio path
  OSC0 (DPCM ROM) -> AMP0 -> Filter -> Pan 0 -> Mixer -> L / R
  OSC1 (DPCM ROM) -> AMP1 ---------> Pan 1 -> Mixer -> L / R
                       (optional OSC1 -> Filter route, see 4.2.9)

Modulation sources -> destinations
  LFO0, LFO1 -> OSC0 / OSC1 Sync/PWM, Pitch
  Env0, Env1 -> AMP0 / AMP1 Gain
  ENV1, LFO1, LFO0 -> Filter Cutoff Freq
  Env0, Env1 (inverted) -> LFO0 / LFO1 Amp
```

*Figure 4.3: MAGIC's built-in digital synthesizer*

### 4.2.3. Oscillators

Each of the two oscillators, OSC0 and OSC1, can be configured to generate one of four available waveforms shown in Figure 4.4. They can be configured using bits [7:6] and [5:4] of register `0x1110`, respectively:

| Address | Bits | Field |
|---|---|---|
| `0x1110` | 7:6 | OSC1 Waveform |
| `0x1110` | 5:4 | OSC0 Waveform |
| `0x1110` | 3 | Sync1 |
| `0x1110` | 2 | Sync0 |
| `0x1110` | 1 | Trigger1 |
| `0x1110` | 0 | Trigger0 |

When DPCM waveform is selected, bits [1:0] enable waveform triggering (note ON/OFF). All other waveforms run continuously and require amplifier gain modulation (either hardware or software) if punctual sounds are to be produced.

*Figure 4.4: Oscillator waveforms (0: Square, 1: Sawtooth, 2: Noise, 3: DPCM)*

Each DPCM source provides access to a drum kit consisting of 20 percussive instruments listed in Table 4.1. To conserve memory, the drum-kit samples are stored as logarithmic-scaled 4-bit Differential Pulse Code Modulation (DPCM) and decompressed on the fly in hardware by MAGIC. The base sampling frequency is 16 KHz for Kick instruments and 32 KHz for all other instruments. The entire drum kit only takes 16 KiB of ROM memory.

When the Sawtooth waveform is selected, bits [3:2] of register `0x1110` allow for synchronizing the reset of the sawtooth ramp with the modulation input of the selected oscillator.

Registers `0x1111` and `0x1113` act as both the MSB for the oscillator's frequency and as an instrument selector if an oscillator is set to DPCM waveform:

| # | Instrument | # | Instrument |
|---|---|---|---|
| 0 | Accoustic Kick | 10 | Accoustic Opened Hi Hat |
| 1 | Electro Kick | 11 | Electro Opened Hi Hat |
| 2 | Techno Kick | 12 | Accoustic Pedal Hi Hat |
| 3 | Accoustic Snare | 13 | Tambourine |
| 4 | Electro Snare | 14 | Ride Cymbal |
| 5 | Techno Snare | 15 | Crash Cymbal |
| 6 | Electro Rim Shot | 16 | Accoustic Tom |
| 7 | Electro Clap | 17 | Electro Clave |
| 8 | Accoustic Closed Hi Hat | 18 | Electro Conga |
| 9 | Electro Closed Hi Hat | 19 | Electro Cow Bell |

*Table 4.1: DPCM Drum Kit instruments list*

| Address | Bits | Field |
|---|---|---|
| `0x1111` | 7:0 | OSC0 Freq MSB / DPCM Instrument |
| `0x1113` | 7:0 | OSC1 Freq MSB / DPCM Instrument |

Registers `0x1112` and `0x1114` act as both the LSB for the oscillator's frequency and as sample playback pitch if the oscillators are set to DPCM waveform:

| Address | Bits | Field |
|---|---|---|
| `0x1112` | 7:0 | OSC0 Freq LSB / DPCM Playback Pitch |
| `0x1114` | 7:0 | OSC1 Freq LSB / DPCM Playback Pitch |

Bits [5:4] of register `0x1115` allow selecting the modulation mode of each oscillator. When asserted, the frequency/pitch of the oscillator (LSB) is modulated by the LFO. When de-asserted, the behavior depends on the waveform:

| Address | Bits | Field |
|---|---|---|
| `0x1115` | 7 | AMP1 Mod |
| `0x1115` | 6 | AMP0 Mod |
| `0x1115` | 5 | OSC1 Mod |
| `0x1115` | 4 | OSC0 Mod |
| `0x1115` | 3:2 | PAN1 |
| `0x1115` | 1:0 | PAN0 |

When the Square waveform is selected, the modulation changes the duty cycle of the Square wave generator, effectively producing low frequency Pulse Width Modulation (PWM).

When the Sawtooth waveform is selected, the modulation changes the moment the ramp is reset, effectively synchronizing the Sawtooth with the modulating LFO. Modulation has no effect on the Sawtooth waveform if the corresponding Sync bit is not set in the `0x1110` register. Oscillator modulation has no effect on the Noise waveform nor on the DPCM waveform.

### 4.2.4. Envelopes

Each of the two envelope generators can be configured as shown in Figure 4.5. The Attack, Decay, Sustain, and Release parameters of each envelope can be configured using the following registers:

| Address | Bits | Field |
|---|---|---|
| `0x1116` | 7:4 | Env0 Decay |
| `0x1116` | 3:0 | Env0 Attack |
| `0x1117` | 7:4 | Env0 Release |
| `0x1117` | 3:0 | Env0 Sustain |
| `0x1118` | 7:4 | Env1 Decay |
| `0x1118` | 3:0 | Env1 Attack |
| `0x1119` | 7:4 | Env1 Release |
| `0x1119` | 3:0 | Env1 Sustain |

*Figure 4.5: Built-in Synthesizer Envelope Generator (Attack time, Decay time, Sustain level, Release time, between Note On and Note Off)*

The Attack, Decay, and Release times use 4-bit exponential values, while the Sustain level uses a 4-bit linear value. The note ON/OFF triggers make use of bits [1:0] from the `0x1110` register.

### 4.2.5. LFOs

Each of the two LFOs can be configured to generate one of four available waveforms, as shown in Figure 4.6. The waveform, frequency, and amplitude of each LFO can be configured using the following registers:

| Address | Bits | Field |
|---|---|---|
| `0x111A` | 7:5 | LFO0 Amplitude |
| `0x111A` | 4:2 | LFO0 Frequency |
| `0x111A` | 1:0 | LFO0 Waveform |
| `0x111B` | 7:5 | LFO1 Amplitude |
| `0x111B` | 4:2 | LFO1 Frequency |
| `0x111B` | 1:0 | LFO1 Waveform |

LFO's amplitude can be modulated by the envelope generator's inverted outputs by setting bits [1:0] of Register `0x111C`:

*Figure 4.6: Low Frequency Oscillator (LFO) waveforms (0: Square, 1: Sawtooth, 2: Sample & Hold, 3: Sine)*

| Address | Bits | Field |
|---|---|---|
| `0x111C` | 7 | Filter |
| `0x111C` | 6 | OSC1 filter |
| `0x111C` | 5:4 | Filter mode |
| `0x111C` | 3:2 | Cutoff control |
| `0x111C` | 1 | LFO1 Mod |
| `0x111C` | 0 | LFO0 Mod |

### 4.2.6. Multi-mode IIR SVF filter

The built-in MAGIC digital synthesizer implements a Infinite Impulse Response (IIR) multi-mode resonant State Variable Filter (SVF) as shown in Figure 4.7.

```text
Signal path of the figure (Chamberlin-style digital SVF):
  Input -> summing node (+), which subtracts the low-pass feedback and the Reso-scaled band-pass feedback
        -> x Cutoff -> integrator (+, z^-1)  => Band-pass output
        -> x Cutoff -> integrator (+, z^-1)  => Low-pass output
  High-pass output taken before the first integrator
  High-pass + Low-pass (+)                  => Band-reject output
```

*Figure 4.7: State Variable Filter (two integrators with z^-1 delays, cutoff multipliers, resonance feedback; outputs High-pass, Band-pass, Low-pass and Band-reject)*

The filter can be turned ON by asserting bit [7] of the `0x111C` register. Four modes of operation are available, as shown in Figure 4.8. The filter modes can be selected using bits [5:4] of the `0x111C` register.

| Address | Bits | Field |
|---|---|---|
| `0x111C` | 7 | Filter |
| `0x111C` | 6 | OSC1 filter |
| `0x111C` | 5:4 | Filter mode |
| `0x111C` | 3:2 | Cutoff control |
| `0x111C` | 1 | LFO1 Mod |
| `0x111C` | 0 | LFO0 Mod |

*Figure 4.8: The four available filter modes (0: Low-pass, 1: High-pass, 2: Band-pass, 3: Band-reject)*

The filter's cutoff frequency can be controlled either by a dedicated register or through hardware modulation. The control modes use bits [3:2] of the `0x111C` register as follows:

0. Modulation disabled
1. Modulation source: ENV1
2. Modulation source: LFO1
3. Modulation source: LFO0

When no modulation is set, the filter cutoff frequency can be adjusted in software using Register `0x111D`:

| Address | Bits | Field |
|---|---|---|
| `0x111D` | 7:0 | Cutoff Frequency |

The resonance 'q' factor can be adjusted using Register `0x111E`:

| Address | Bits | Field |
|---|---|---|
| `0x111E` | 7:0 | Resonance 'q' factor |

### 4.2.7. Amplification

Each of the two amplification stages controls the volume of a voice through Register `0x111F`. Bits [3:0] control the gain of AMP0 and bits [7:4] control the gain of AMP1:

| Address | Bits | Field |
|---|---|---|
| `0x111F` | 7:4 | AMP1 Gain |
| `0x111F` | 3:0 | AMP0 Gain |

Additionally, both amplifiers' gains can be modulated by the envelope generators, as shown in Figure 4.3, using bits [7] for AMP1 and bit [6] for AMP0 of Register `0x1115`:

| Address | Bits | Field |
|---|---|---|
| `0x1115` | 7 | AMP1 Mod |
| `0x1115` | 6 | AMP0 Mod |
| `0x1115` | 5 | OSC1 Mod |
| `0x1115` | 4 | OSC0 Mod |
| `0x1115` | 3:2 | PAN1 |
| `0x1115` | 1:0 | PAN0 |

### 4.2.8. Panners

Two hard panners allow each voice to be sent to the Left or Right channel using bits [3:2] for PAN1 and bits [1:0] for PAN0 of Register `0x1115`. The pattern is as follows:

0. No output
1. Left channel
2. Right channel
3. Both channels

### 4.2.9. Special routing

The output of OSC1 can be routed through the filter by asserting bit [6] of the `0x111C` register. Note that enabling this route does not disable the normal AMP1 → PAN1 → Mixer route. For exclusive routing of OSC1 through the filter, PAN1 can be disabled by clearing bits [3:2] of Register `0x1115`. Alternatively, the volume of AMP1 can be set to zero.

## 4.3. Visualizing audio

Visualizing the audio waveform can be important when designing sounds, but audio waveforms can also be used in many creative ways. As both audio sources are independently generated outside the realm of the CPU and mixed together through analog circuitry, Micro-8 makes use of the Analog to Digital Converter (ADC) built into the MCU to sample back the resulting waveform and make it available to the CPU. Specific API functions are available to trigger sampling and obtain the resulting sampled values. See Section 7.12 for more information. These values can then be used for drawing graphics or even triggering events in a game.

# 5. Memory

## 5.1. Memory organization

There are six physical types of memory in Micro-8:

1. MCU internal EEPROM (4 KiB)
2. MCU internal flash ROM (128 KiB)
3. FPGA internal flash ROM (≈ 16 KiB)
4. MCU internal SRAM (≈ 4 KiB)
5. FPGA internal SRAM (≈ 12 KiB)
6. External SRAM (XSRAM) (256 KiB)

The first type is where the OS configuration information is stored, such as the version number. It is not accessible to the user.

The second type is where the OS and built-in tools reside. This memory is mapped to the internal native code address space and is not accessible to the user.

The third type of memory is where the Drum Kit sound bank and the mouse pointer graphics reside. This memory is mapped to the internal FPGA logic and is not accessible to the user.

The next memory types (4, 5, and 6) are accessible to the user and shown visually in Appendix E.

The fourth type is where the OS variables are stored along with the system stack and heap. This memory is mapped to the CPU 16-bit address space. Although it is visible to the user (from address `0x0000` to `0x10ff`), it is advised not to mess with this memory as chances are it would crash the system. This area is fixed and always visible to the CPU/user.

The fifth type is the main system registers and buffers section (from `0x1100` to `0x2FFF`). This memory physically resides in the FPGA but is mapped to the CPU 16-bit address space. This section is fixed and always visible to the CPU/user.

Next comes a section called the programmable CPU window. This section starts at virtual address `0x3000` and ends at `0xEFFF`. This section is seen at these addresses by the CPU and the VM in particular (See Chapter 6 for more information about the VM and programming Micro-8). A virtual address within this section is mapped to a real address in the sixth memory type, the XSRAM, by the memory controller in MAGIC.

A page offset allows you to specify the position of the programmable CPU window in the physical XSRAM by steps of 1024 bytes. Memory functions such as `poke()` or `memSet()` have an optional page argument where you can specify the page offset of the window in the XSRAM. The page offset system thus allows the CPU to access the entire XSRAM.

Finally, a fixed 4 KiB section of memory from address `0xF000` to `0xFFFF` is used to store various buffers. This virtual memory is mapped to XSRAM address `0x3F000` to `0x3FFFF` in a fixed manner and cannot be changed. It is always visible to the CPU no matter the position of the programmable CPU window.

The physical address calculation, as computed by the memory controller, is shown in Expression 5.1 where S stands for the provided source address and P stands for the actual physical address.

```text
      | (SRAM)  S                        if S < 0x3000
P  =  | (XSRAM) S - 0x3000 + page×1024   if 0x3000 ≤ S < 0xF000        (5.1)
      | (XSRAM) S + 0x30000              if S ≥ 0xF000
```

The following examples show how to address memory in various scenarios:

```c
poke(0x3000,0xFF,0x00);//write 0xff to address 0x00000 in XSRAM
memSet(0x3000,48000,0,0x60);//clear bitmap area (48000 bytes) at address 0x18000
memSet(0x1200,6400,0);//clear char glyph and color buffers (3200 bytes each)
memSet(0x3000,32768,0x20);//clearing 64 KiB of sprite data requires two
memSet(0x3000,32768,0x40);//memSet() calls because of the 48 KiB cpu window limitation
```

When the page argument is omitted, the default `0xCC` page value is used. This defaults the programmable window to address `0x33000` in the XSRAM and is where a Lofi program is stored, as shown in Appendix E.

See the Lofi API Section 7.2 for more examples.

The VM data segment origin address can be adjusted with the `data` command. See the CLI commands in Chapter 2 for more information.

# 6. Coding in Lofi

## 6.1. Overview

Lofi is a small custom procedural imperative programming language embedded in Micro-8 OS. Lofi's aim is to allow you to easily develop your own games or applications directly on the platform, in a totally independent and fun way.

Here are a few of the guidelines we followed when developing Lofi:

- Zero friction. Turn on and start coding immediately.
- Fast learning curve with familiar C-like curly-brace syntax.
- Statically typed for good performance and early error detection.
- Immediate and deferred compilation modes to encourage experimentation.
- Comprehensive API tailored to Micro-8 features and constraints.
- No dynamic memory allocations (although you could implement your own).
- No garbage collector.

## 6.2. Compiler and VM

Lofi source text is typically written using either the CLI prompt in immediate mode or the built-in Text Editor. Before execution, the source code is compiled into bytecode by the built-in compiler.

At run-time, the built-in Virtual Machine (VM) executes the bytecode. The VM is an untyped, stack-based virtual machine. All type checking and symbol resolution are done ahead of time by the compiler. This allows the VM to be reasonably fast, even on a tiny 8-bit CPU, beating in most cases BASIC[^D1] interpreters by at least an order of magnitude.

Both the compiler and the VM reside in internal flash ROM memory, leaving a large[^D2] amount of RAM available to the user's program.

[^D1]: https://en.wikipedia.org/wiki/BASIC
[^D2]: Considering the 16-bit address space limitation

## 6.3. Language constructs

### 6.3.1. Generalities

A Lofi program is a sequence of declarations and statements.

The semicolon `;` acts as the statement terminator. Each individual statement must end with a `;`.

Whitespaces are generally ignored by the compiler. However, a whitespace is mandatory to separate keywords from other elements of a statement.

Any text following a double slash `//` will be ignored by the compiler. This provides a way to add comments to the code.

Example:

```c
int i=5; //declare variable i as integer and initialize to 5
```

In the built-in Text Editor, Lofi comments are shown in a dark green color.

### 6.3.2. Identifiers

A Lofi identifier is a name used to identify a user-defined item such as a variable or a function. An identifier starts with a letter `a` to `z`, `A` to `Z`, or an underscore followed by zero or more letters, underscores, and digits `0` to `9`.

The maximum length of an identifier is 32 characters.

Valid identifiers example: `n`, `myVar`, `temp`, `value2`, `ret_value`

In the built-in Text Editor, Lofi identifiers are shown in a dark cyan color.

### 6.3.3. Reserved words

Some keywords, function names, and built-in variables are reserved by the Lofi language and the Lofi API. They cannot be used as variable or other identifier names.

In the built-in Text Editor, Lofi keywords functions are shown in a dark blue color while Lofi API functions and built-in values are shown in a dark red color.

See Appendix C for the complete list of reserved words.

### 6.3.4. Data types

#### void

The type `void` represents no data. It is used to declare a function with no return value.

Example:

```c
void count(int n)
{
for(int i; i<n; i++) print(i,"⏎");
}
```

#### bool

The type `bool` represents Boolean data. A Boolean value can either be `true` or `false`. However, internally a Boolean value is represented as a byte. Thus, any positive value can represent a Boolean `true` if cast to `bool`. The zero value represents a Boolean `false` if cast to `bool`. See Section 6.3.7 for more information about type casting.

Example:

```c
bool x=8>4; //x is declared and set to true
bool y=false; //y is declared and set to false
bool z; //z is declared and set to default value (false)
```

#### byte

The type `byte` represents natural values in the range [0,255]. The type `byte` is ideal for storing large arrays of small natural values, as it takes less memory space than the types `nat` or `int`.

Example:

```c
byte a; //a is declared and set to default value 0
byte myArray[1024]; //declare an array of 1024 bytes
byte c=-12; //illegal as a byte can only store positive values
```

#### nat

The type `nat` represents natural values in the range [0,65535]. Data of type `nat` are stored as 16-bit unsigned values (2 bytes) and are intended to store large natural values, such as addresses.

Example:

```c
nat a; //a is declared and set to default value 0
nat addr=0x1200; //declare addr and sets to value 0x1200 (the character glyph index buffer origin)
poke(addr,'A'); //displays character 'A' at the top left of the character display
nat c=-12; //illegal as a nat can only store positive values
```

#### int

The type `int` represents integer values. In Lofi, integers are stored as 16-bit signed values (2 bytes). The value range is [-32768,32767].

Example:

```c
int i; //i is declared and set to default value 0
int a=1024; //a is declared and set to value 1024
int b=-500; //b is declared and set to value -500
```

#### real

The type `real` represents real[^D3] values. In Lofi, reals are stored as 16-bit floating point values (2 bytes) using the IEEE754 half-precision representation[^D4]. The effective value range is approximately [-65504,65504]. However, the precision can vary significantly. Precision is best when used with small value ranges, such as [-1,1] where you can get up to 3 significant decimal digits. At higher values, the precision can become pretty rough. This is Lofi after all...

Micro-8 uses a small 8-bit CPU with no hardware support for floating-point math. Operations of the type `real` are thus pretty slow. It is best to keep the type `real` for offline work, such as pre-computing tables or for non-real-time applications.

Example:

```c
real r; //r is declared and set to default value 0.0
real pi=3.1416; //pi is declared and set to 3.1416
```

[^D3]: Irrationals or rationals with a large number of decimals are obviously truncated given the finite nature of the representation
[^D4]: https://en.wikipedia.org/wiki/Half-precision_floating-point_format

#### text

The type `text` represents textual data.

You can declare a text using double quotes to specify a literal text, for example `"Hello world!"`, or using a constant literal size initializer within curly braces such as `{10}`.

Example:

```c
print("Hello world!"); //creates a text literal and prints it
text t={10}; //creates an empty text buffer of size 10 + null char
```

When declaring a text literal, the content is stored within the program (in the code memory segment) as a series of 8-bit characters ending with an implicit null character.

When declaring a text variable and initializing with a size initializer, an empty text buffer is reserved either in the data memory segment (global scope), or on the Value stack (local scope). The size is equal to the specified size + 1 in order to accommodate the extra null character.

When a text literal is assigned to a variable, a copy of the text is stored either in the data memory segment (global scope), or on the Value stack (local scope) and the variable keeps a pointer to the new text data. It is possible to modify the contents of the text by modifying the values in memory, or simply by assigning another text literal to the variable. In that case, the length must be less than or equal to the original text; otherwise, the adjacent memory will be overwritten!

Declaring a text without an initializer produces a compilation error.

Examples:

```c
print("Micro-8"); //creates a text literal in code segment and prints it
text forbidden; //error, requires initialization
text t="Hello World!"; //text literal "Hello World!" is created in code segment, t is declared and points to a copy of "Hello World!" in data segment
print(t); //prints Hello World!
t="Micro-8"; //a new text literal is created and a copy replaces the original text in data segment. The original text literal "Hello World!" is still in code memory but is no longer accessible.
print(t); //prints Micro-8
```

Dumping data memory after executing the above program will show the following contents in the ASCII output: `Micro-8 rld!`. This is because the second text literal is shorter than the original one, leaving the rest visible in memory after the null character ending the `Micro-8` text.

### 6.3.5. Variables

#### Declaration and assignment

Variables are declared using a valid Lofi data type followed by a valid identifier name and an optional initializer.

Example:

```c
int i; //i is declared and initialized to default value 0
real pi=3.1416; //pi is declared and initialized to 3.1416
```

Values are initialized or assigned using the `=` sign followed by an expression. A variable can be declared first, at which point it receives the default value 0, and assigned a value later, except for the `text` type.

Example:

```c
int c; //c is declared and initialized to default value 0
int a=3; //a is declared and initialized to value 3
int b=4; //b is declared and initialized to value 4
c=a+b; //Expression a+b is evaluated and the result is assigned to c
```

#### Global and local scope

Global variables are variables declared out of a bloc scope. A bloc is defined by curly braces `{}`. Global variables persist through the entire execution of the program. In immediate mode, global variables persist until either a new command is entered using the CLI, a soft or hard reset is performed, or an external program is loaded into memory. See Chapter 2 for more information about the immediate mode.

Local variables are declared inside a bloc. Local variables live on the VM Value stack during the bloc execution. As soon as execution exits the bloc scope, the related local variables are deleted. Blocs can be nested, and variables from inner blocs can overshadow variables from outer blocs if they have the same name.

Example:

```c
int a=5; //a is declared in the global scope
{ //enter bloc scope
int b=10; //b is declared in the local scope
int a=20; //another variable a is declared in the local scope, overshadowing the global variable a
print(a+b); //result 30
} //locals a and b are deleted
print(a); //result 5
```

In Lofi, variable name resolution happens at compile time. Since there is no cache (all RAM is SRAM anyways), data locality has little benefit and both globals and locals are accessed at pretty much the same speed.

### 6.3.6. Literals

#### Boolean literals

Boolean literals are `true` and `false` and can appear in any Boolean expression.

Example:

```c
bool x = true;
bool y = x && false; //result false
```

#### Integer literals

Integer literals can be entered in decimal (characters `0` to `9`), in hexadecimal (using prefix `0x` followed by characters `0` to `9` or `a` to `f`) or in binary (using prefix `0b` followed by characters `0` or `1`) and can be used in expressions.

Integer literals are of three types:

1. Any integer literal value in the [0,255] range will be considered of type `byte`
2. Any integer literal value in the [256,65535] range will be considered of type `nat`
3. Any negative integer literal will be considered of type `int`

Up-casting will happen automatically when these types are mixed in expressions, and/or assigned to a higher type. See Section 6.3.7 for more information about type casting.

Example of expressions using various literal values and bases:

```c
int i=(8+512)*2; //1040
int j=(8+0x200)*0b10; //1040
```

#### Real literals

Real literals can be entered in decimal either starting with a `.` followed by characters `0` to `9`, or containing a `.` between characters `0` to `9`.

Example:

```c
real r=.5; //equivalent to 0.5
r*=1.5; //0.75
```

#### Character literals

Character literals can be entered between single quotes. The value type of the character literal is of type `byte` and (for alphanumeric characters) represents the ASCII code.

Example:

```c
byte c='A';
print(c);//65
```

#### Text literals

Text constant literals are declared with double quotes. They are stored in the code segment.

A text constant literal cannot span across multiple lines in the Text Editor; it must be terminated on the same line.

Example:

```c
print("Hello World!");
```

### 6.3.7. Type casting

#### Value types

Lofi follows an automatic type promotion policy for value types. That is, a low value type can implicitly be promoted to a higher value type.

Example:

```c
real r=2; //automatically promotes 2 to 2.0
int i=true; //automatically promotes true to 1
```

All other type conversions need to be explicit using the type cast operator `(type_name)expression`. Failing to provide the adequate type cast operator will throw an error.

Example:

```c
int i=2.5; //type error
int i=(int)2.5; //explicit cast from 2.5 to 2 with data loss
bool x=(bool)4; //4 is cast to true
```

The value type hierarchy is as follows:

```text
bool -> byte -> nat -> int -> real
```

Due to representation limits, be aware that large `nat` values (above 32767) promoted to `int` will overflow to negative values. Of course, loss of information can also occur when performing explicit type casting to a lower type. See Section 6.3.4 for more info about type internal representation.

#### Text type

The `(text)` cast operator can be used to convert an address (typically stored as a `nat`) to a text.

Example:

```c
text t="Hello world!";
void f(nat addr)
{
print((text)addr,"⏎");
}
nat tAddr=&t;
for(int i; i<#t; i++) f(tAddr++);
```

The above example will output:

```text
Hello world!
ello world!
llo world!
lo world!
o world!
 world!
world!
orld!
rld!
ld!
d!
!
```

In the built-in Text Editor, Lofi type casting operators are shown in a dark gray color.

### 6.3.8. Data structures

#### Custom types

Custom types can be declared with the `type` keyword. Custom types allow grouping together various basic data types in a single compound type. It is similar to records or structures found in other languages.

Custom types are declared with a name and a series of member declarations:

```text
type type_name { m1_type m1_name; m2_type m2_name; ... };
```

Once declared, a custom type can be instantiated using:

```text
type_name instance_name;
```

Instances can be initialized at instantiation using:

```text
type_name instance_name={value1,value2, ...};
```

Or:

```text
type_name instance_name=other_instance_name;
```

To access a particular member of a custom type instance, the `.` operator is used like this:

```text
instance_name.member_name
```

Custom types are not reentrant. Only the following basic built-in types are supported in custom types:

- `bool`
- `byte`
- `nat`
- `int`
- `real`
- `text`

Example:

```c
type vec2 //declare a new 2D integer vector type
{
int x;
int y;
};
vec2 swap(vec2 v) //a function to swap the components
{
int t=v.x;
v.x=v.y;
v.y=t;
return v;
}
vec2 j={4,7}; //create a vec2 instance
j=swap(j);
print(j.x,",",j.y); //7,4
```

#### Arrays

Arrays provide an efficient way to store a continuous and sequential collection of elements of the same type. The size of an array is fixed in Lofi and must be declared beforehand with a constant literal.

Arrays are defined the following way:

```text
type_name[size] = {optional_initializer};
```

Elements of an array are accessed through indices. The first index starts at 0, and the indices can be any byte, natural, or positive integer expressions.

There are no boundary checks in Lofi. It is up to the user to ensure that the provided index is within the bounds. Depending on the context, when writing outside the array boundaries, unexpected behavior may occur.

Arrays can be initialized with a list of values in the form: `{value1,value2, ...}`.

The initialization list must not be longer than the array size. If shorter, default values will be provided for the non-initialized values.

Example:

```c
int i[]; //error, invalid array size
int a[10]; //an array of 10 integers is created and initialized to 0
a[5]=25; //the sixth element of a is set to 25
real b[4]={2.5,-1.5,33.33};// an array of 4 real values is created and the first 3 elements are initialized. The fourth element is initialized to default 0.0 value.
text t[4] = {"Zero","One","Two","Three"};//an array of text literals is created
for(int i; i<#t; i++)print(t[i],"⏎");//counts in text
```

#### Text arrays

When declaring an array of text elements, each array index stores the address of the text, not the text content itself. The reason is that all elements in an array must have the same size. You can, however, specify the element (and its size) the array index refers to at any time. For example:

Example:

```c
text names[3]={"John","Mike",""};//we don't know yet element at index 2
text t={10};//reserve a 10 character empty text
names[2]=t;//element at index 2 now refers to text t
input(t,10);//let the user enter a new name
print(names[2]);//print the new name
```

#### Custom type arrays

Arrays of custom types can be declared and optionally initialized with contents. For example:

Example:

```c
type human
{
text name={10};
int age;
int size;
bool sex;//true=male, false=female
};
human h[3]={{"Johnny",24,182,true},
{"Anna",18,162,false},
{"Peter",45,176,true}};
for(int i; i<#h; i++) print(h[i].name,"⏎"); //print all names
```

### 6.3.9. Expressions and operators

An expression produces a value. An expression can contain arithmetic, relational, logical, bitwise, and memory operations, as well as non-void function calls.

The result of an expression can be assigned to a variable using the assignment operator `=`. Compound assignment operators can be used to perform both an assignment and an operation.

Example:

```c
int a=5;
int b=100;
int i=a+sqrt(b);//the expression is evaluated to 15 and assigned to i
i+=5;//equivalent to i=i+5, i is now 20
i--;//i is decremented by one and is now 19
```

#### Arithmetic operators

| Operator | Description | Supported types | Example |
|---|---|---|---|
| `+` | Adds the two operands. | byte, nat, int, real | `a+b` |
| `-` | Subtracts the second operand from the first. Negates operand when used as unary operator. | byte, nat, int, real | `a-b` |
| `*` | Multiplies both operands. | byte, nat, int, real | `a*b` |
| `/` | Divides numerator by denominator. | byte, nat, int, real | `a/b` |
| `%` | Modulus. Remainder of division of first operand by second operand. | byte, nat, int | `a%b` |
| `++` | Pre/post increments operand. | byte, nat, int | `a++` |
| `--` | Pre/post decrements operand. | byte, nat, int | `--a` |

#### Relational operators

| Operator | Description | Supported types | Example |
|---|---|---|---|
| `==` | True if both operands are equal. | bool, byte, nat, int, real | `a==b` |
| `!=` | True if operands are not equal. | bool, byte, nat, int, real | `a!=b` |
| `>` | True if left operand is greater than right operand. | bool, byte, nat, int, real | `a>b` |
| `<` | True if left operand is less than right operand. | bool, byte, nat, int, real | `a<b` |
| `>=` | True if left operand is greater than or equal to right operand. | bool, byte, nat, int, real | `a>=b` |
| `<=` | True if left operand is less than or equal to right operand. | bool, byte, nat, int, real | `a<=b` |

#### Logical operators

| Operator | Description | Supported types | Example |
|---|---|---|---|
| `&&` | Logical AND. True if both operands are true. | bool | `a&&b` |
| `\|\|` | Logical OR. True if any operand is true. | bool | `a\|\|b` |
| `!` | Logical NOT. True if the operand is false. | bool | `!a` |

#### Memory operators

| Operator | Description | Operand | Example |
|---|---|---|---|
| `&` | Address of variable. | identifier | `&a` |
| `$` | Size of variable (in bytes). | identifier | `$a` |
| `#` | Number of elements in variable. | identifier | `#a` |

#### Bitwise operators

Bitwise operators allow performing bit operations on data in a type-agnostic way.

| Operator | Description | Supported types | Example |
|---|---|---|---|
| `~` | Inverts bits of the operand. | bool, byte, nat, int, real | `~a` |
| `\|` | Binary OR. Sets bit if bit is set in either operand. | bool, byte, nat, int, real | `a\|b` |
| `^` | Binary XOR. Sets bit if bit is set in one operand but not both. | bool, byte, nat, int, real | `a^b` |
| `&` | Binary AND. Sets bit if bit is set in both operands. | bool, byte, nat, int, real | `a&b` |
| `>>` | Shift first operand bits to the right by second operand places. | bool, byte, nat, int, real | `a>>b` |
| `<<` | Shift first operand bits to the left by second operand places. | bool, byte, nat, int, real | `a<<b` |

The following truth table shows results for the unary operator on a single bit:

| p | ~p |
|---|---|
| 0 | 1 |
| 1 | 0 |

The following truth table shows results for bit-to-bit binary operators:

| p | q | p\|q | p^q | p&q |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 1 | 0 |
| 1 | 0 | 1 | 1 | 0 |
| 1 | 1 | 1 | 0 | 1 |

Examples:

```c
print("0b",~0b10101010); //result -> 0b01010101
print("0b",0b11110000 | 0b00001111); //result -> 0b11111111
print("0b",0b10101010 ^ 0b00001111); //result -> 0b10100101
print("0b",0b10101010 & 0b11110000); //result -> 0b10100000
print("0b",0b11110000 >> 4); //result -> 0b00001111
print("0b",0b00001111 << 4); //result -> 0b11110000
```

#### Compound assignment operators

| Operator | Description | Supported types | Example |
|---|---|---|---|
| `+=` | Adds the result of the expression to the variable. | byte, nat, int, real | `var+=exp` |
| `-=` | Subtracts the result of the expression from the variable. | byte, nat, int, real | `var-=exp` |
| `*=` | Multiplies the result of the expression with the variable. | byte, nat, int, real | `var*=exp` |
| `/=` | Divides the the variable with the result of the expression. | byte, nat, int, real | `var/=exp` |
| `%=` | Divides the the variable with the result of the expression and assign the remainder. | byte, nat, int | `var%=exp` |
| `<<=` | Shifts the variable bits to the left by expression places. | byte, nat, int, real | `var<<=exp` |
| `>>=` | Shifts the variable bits to the right by expression places. | byte, nat, int, real | `var>>=exp` |
| `&=` | Binary ANDs the variable bits with the result of the expression. | byte, nat, int, real | `var&=exp` |
| `\|=` | Binary ORs the variable bits with the result of the expression. | byte, nat, int, real | `var\|=exp` |
| `^=` | Binary XORs the variable bits with the result of the expression. | byte, nat, int, real | `var^=exp` |

#### Operators precedence

Operators precedence determines how operands and operators in an expression are grouped and evaluated. The following table shows all operators from the highest to the lowest precedence. The higher the precedence, the higher the priority of operation. Operators of the same precedence are evaluated from left to right.

| Operator | Category |
|---|---|
| `( )` | Grouping, Call |
| `!` `-` `++` `--` `~` Cast | Unary |
| `*` `/` `%` `&` `<<` `>>` | Factor |
| `+` `-` `\|` `^` | Term |
| `<` `>` `<=` `>=` | Comparison |
| `==` `!=` | Equality |
| `&&` | Logical AND |
| `\|\|` | Logical OR |
| `=` | Assignment |

### 6.3.10. Selection and flow control

The `if()` and `else` statements allow for selecting and controlling the execution flow of a program. These statements can be nested to form complex selection and flow control patterns.

#### if

The `if(condition)` statement evaluates the Boolean expression inside the parentheses (called the condition) and executes the subsequent statement or block if the condition is true.

Example:

```c
if(12>4) print("Twelve is greater than four");
if(false)
{
print("Will not print");
}
```

#### else

The `else` statement is optional and executes the subsequent statement or block if the condition from the previous `if` statement is not met. The `else` statement must follow an `if()` statement.

Example:

```c
if(5>10) print("Will not print");
else print("Five is not greater than ten");
```

### 6.3.11. Loops

Loops allow for repeating a statement or a block of statements while a condition is met. Loops can be nested.

#### while

The `while(condition)` loop evaluates the Boolean expression inside the parentheses, and if it is true, executes the subsequent statement or block (the body of the loop). After the execution of the body, the process repeats. If the result of the expression is false, the body is skipped, and control is returned to the rest of the program.

Example:

```c
int acc;
int i;
while(i<8)
{
acc+=i;
i++;
}
print(acc);
```

#### for

The `for(init;condition;increment)` loop executes according to the following steps:

1. The `init` statement is executed first and only once. Here, a control variable can be declared and initialized.
2. Next, the `condition` expression is evaluated. The condition typically involves a control variable. If true, the subsequent statement or block (the body of the loop) is executed. If the result of the condition expression is false, the body is skipped, and control is returned to the rest of the program.
3. After the execution of the body of the loop, the `increment` statement executes. This statement allows updating any loop control variables.
4. The process repeats at point 2.

Any of these control statements can be omitted as long as the two `;` appear within the parentheses.

Example:

```c
for(int i; i<10; i++) print(i*i,"⏎"); //all squares up to 81
```

#### break

The `break` statement exits a loop immediately and returns control to the rest of the program.

Example:

```c
int i;
while(true)
{
if(i==10) break;
print(i,"⏎"); //will print from 0 to 9
i++;
}
```

#### continue

The `continue` statement aborts the current iteration and jumps immediately to the next.

Example:

```c
for(byte i; i<10 i++)
{
if(i==5) continue;
print(i,"⏎"); //will print 0 1 2 3 4 6 7 8 9
}
```

### 6.3.12. Functions

User functions can be declared in the global scope and used anywhere in the program. Lofi allows for nested or even recursive function calls but does not allow nested function declarations.

> **Note:** Given the single-pass nature of the compiler, functions must be declared before being used.

#### Declaration

A user function is declared in the following way:

```text
return_type function_name(parameter_list) {function_body}
```

Each parameter requires a type and a name. The parameters are separated by a comma `,`. If the function is to return a value (that is, the return type is not `void`), the `return` statement followed by the return expression is required in the function body. If the return type of the function is `void`, no `return` statement is necessary.

The `text` type is passed to functions as a reference using the regular `text identifier` parameter syntax. When modifying the text inside the function, the originally referenced text gets modified.

Arrays are passed to functions as references using the `type identifier[]` parameter syntax. When an array passed to a function is modified inside the function, the original referenced array gets modified. It is not possible to use the `#` and `$` operators on arrays passed as parameters since the size of the array is unknown to the function at compile time. When required, the size must be provided as a separate parameter. See the example below.

#### Usage

A function is called in the following way:

```text
function_name(argument_list);
```

The argument list must match the declared function signature in both type and arity. The arguments are separated by a `,` character.

Examples:

```c
int fact(int n)
{
if(n==1) return 1;
else return n*fact(n-1);
}
print(fact(5)); //120
real average(real a, real b)
{
return (a+b)*.5;
}
print(average(1.0, 2.0)); //1.5
void hello(text name)
{
print("Hello ", name, "!⏎");
}
hello("John"); //Hello John!
int sum(int a[], nat size)
{
int acc;
for(nat i; i<size; i++)
{
acc += a[i];
}
return acc;
}
int t[10]={1,2,3,4,5,6,7,8,9,10};
print("Sum of elements in t: ", sum(t,#t));
```

## 6.4. Lofi Known limitations

### 6.4.1. Function usage

As Lofi uses a single-pass compiler, functions must be declared before they are called.

### 6.4.2. Function nesting

Function nesting is not supported; all function declarations must have a global scope. That is, you cannot declare a new function inside an existing function or block. However, you can call a function from within another function or block, including the function itself (recursion).

### 6.4.3. Custom types

Custom types are not reentrant (you cannot have a custom type inside another custom type).

### 6.4.4. Optimizations

The compiler does not perform any code optimizations on its own. It is up to the user to write code in an efficient manner.

### 6.4.5. Memory usage

- The value stack (temporaries and locals) is limited to 256 entries.
- Local arrays use the value stack and are limited to a maximum of 256 entries for the `byte` and `bool` types, and 128 entries for all other types; however, depending on context, they can be much more limited. It is recommended to use only tiny local arrays.
- Local text is limited to a maximum of 256 characters; however, depending on the context, it can be much more restricted. It is recommended to use only tiny local texts.
- The frame stack (function call depth) is limited to 64 levels.
- The maximum number of members in a custom type is limited to 16.
- The maximum number of `break` statements in a loop is limited to 64.
- The maximum number of `continue` statements in a loop is limited to 64.
- The maximum number of `return` statements in a `void` function is limited to 64.
- The maximum size of an identifier is 32 characters.
- The symbol table is limited to 11 KiB. The symbol table holds all global variables, but only local variables from the current scope. The compiler buffers (including the symbol table) are always present in immediate mode. In deferred mode, they are used only during compilation.
- The default memory configuration reserves 32 KiB for bytecode (compiled program), starting at VM address `0x3000` (physical address `0x34000` in XSRAM) and 16 KiB for program data (only 4 KiB in immediate mode because of compiler buffers, starting at `0xC000`), starting at VM address `0xB000`. The base VM address of the data segment can be adjusted using the `data` command before compilation (see Chapter 2 for more information about the CLI and Appendix E for more information about memory maps).

# 7. Lofi API

## 7.1. Overview

The Lofi coding API provides a series of built-in functions and variables in the I/O, memory and system, math, graphics, and audio domains to help you develop your games and applications. These native functions are fast and should be favored over potential user function implementations. Always verify that a native function does not exist before trying to implement your own.

The `EXAMPLES`, `DEMOS`, `GAMES` and `TOOLS` folders also provide a great source of information on how to use the API in various contexts.

## 7.2. I/O and System

### 7.2.1. Functions

#### print()

The `print(parameter list)` function prints the content of its parameters. The number of parameters is variable, and the parameters are separated by a `,` character. Parameters can be of any number, of any type, and types can be mixed in a single statement.

Special control characters can be inserted into text literals to allow some degree of control over the printing behavior or the text appearance. In the following example, the `⇥` character is used to tabulate the text, and `⏎` creates a newline:

```c
void formatText(int n)
{
    print("Length is: ",n,"⇥cm.⏎");
}
for(int i=1; i<=10; i++) formatText(i); //will display 10 lines with formatted text
```

The output is as follows:

```text
Length is: 1    cm.
Length is: 2    cm.
Length is: 3    cm.
Length is: 4    cm.
Length is: 5    cm.
Length is: 6    cm.
Length is: 7    cm.
Length is: 8    cm.
Length is: 9    cm.
Length is: 10   cm.
```

##### Control characters

The following table lists all available control characters and their effect on the `print()` function:

| Symbol | Key | Effect |
|---|---|---|
| `⇥` | `FCTN` + `TAB` | Tabulate |
| `⏎` | `FCTN` + `ENTER` | New line |
| `[FG]` (foreground color symbol, underlined A) | `FCTN` + `F` | Select foreground color |
| `[BG]` (background color symbol, highlighted A) | `FCTN` + `K` | Select background color |
| `[INV]` (inverse symbol) | `FCTN` + `V` | Inverse colors |
| `⟲` | `FCTN` + `⇤` | Reset style |
| `←` | `FCTN` + `←` | Move cursor left |
| `→` | `FCTN` + `→` | Move cursor right |
| `↑` | `FCTN` + `↑` | Move cursor up |
| `↓` | `FCTN` + `↓` | Move cursor down |
| `0x` (hex prefix glyph) | `FCTN` + `X` | Print the next value in hexadecimal |
| `0b` (binary prefix glyph) | `FCTN` + `B` | Print the next value in binary |
| `?` (glyph symbol) | `FCTN` + `G` | Print an arbitrary glyph |

To use foreground and background color selection, first insert the foreground (`[FG]`) or the background (`[BG]`) symbol in the text, immediately followed by the single-digit hexadecimal color index (`0` to `f`). Note that the actual color will depend on the RGB values set in the text palette at that particular index.

When changing text colors, all subsequent text will be printed using the new colors. To restore text printing to default colors (black text on a transparent background), use the `⟲` symbol.

Example:

```c
print("Normal [FG]d[BG]1Red on Black⟲");
print("Normal [INV]Inverted⟲");
```

To display values in binary or hexadecimal form, first insert the symbols `0b` or `0x` into a text literal, followed by the numeric expression.

Example:

```c
print("0b",170); //result -> 0b10101010
print("0x",65535); //result -> 0xFFFF
```

When a particular glyph is not available on the keyboard, you can use the `?` symbol in a text literal, immediately followed by the glyph index in double-digit hexadecimal (`00` to `ff`). See Appendix B for a table of all glyph codes.

Example:

```c
print("The diskette glyph is: ?c4");
```

If you need to compute the glyph index via an expression, you can use the following method:

Example:

```c
text t={1}; //reserve memory for one character
byte index = 0xb4 + 0x10; //compute an arbitrary index
poke(&t,index); //write the glyph index into memory
print(t); //print the character
```

#### input()

The `input(text, maxSize)` function prompts the user to enter a single line of text. When the user presses the `ENTER` key, the input text is copied to the provided `text` parameter of type `text`, and the program continues. The `maxSize` parameter is of type `nat` and limits the size of the input in the prompt editor. It should not exceed the size of the text variable; otherwise, adjacent memory will be overwritten.

Example:

```c
text t={10};
print("Enter some text: ");
input(t,10);//limit entry to 10 characters
print("⏎You entered the following text: ",t);
```

#### concat()

The `concat(text1, text2)` function concatenates the contents of two text variables. Both parameters are of type `text`. The content of the `text2` will be appended to the content of `text1`. The size of the text pointed to by parameter `text1` must be sufficient to accommodate the concatenated text; otherwise, adjacent memory will be overwritten.

Example:

```c
text t={12};
print("Enter filename without extension: ");
input(t,8);//enter the word 'image'
concat(t,".bmp");//add extension
print("⏎Filename with extension: ",t); //will print 'image.bmp'
```

#### txtWin()

The `txtWin(x1, y1, x2, y2)` function enables changing the text window in a text output (e.g., used by the `print()` function). Cursor wrapping will respond to the defined window. The parameters `x1, y1` and `x2, y2` define the upper-left and lower-right corner coordinates of the window, respectively. Coordinates are expressed as columns and rows with respect to the character matrix, as shown in Section 3.4. Default full-screen values are (0,0,79,39). All parameters are of type `byte`.

#### curPos()

The `curPos(x, y)` function moves the cursor to the position provided by the parameters `x, y` (respectively, the column and row), with respect to the character matrix, as shown in Section 3.4. All parameters are of type `byte`.

#### clkSet()

The `clkSet()` function resets and starts the built-in timer. To query the timer, use the built-in `_mSec` variable.

#### mouseSens()

The `mouseSens(divider)` function allows for controlling mouse sensitivity. The `divider` parameter applies a right shift to the mouse sensitivity. A value of 1 divides the sensitivity by 2... and so on. The default value is 0 and provides maximum sensitivity. The `divider` parameter is of type `byte` and the valid value range is [0,4].

#### mouseUpdt()

In order to provide simultaneous values on both the X and Y mouse axes using the built-in variables `_mouseX`, `_mouseY`, `_mouseDX`, and `_mouseDY`, the `mouseUpdt()` function must be used beforehand to buffer the values of the separate mouse position axes read by the driver.

Example:

```c
while(_keyDwn != 12) //ESC key to exit
{
    mouseUpdt(); //buffer mouse data
    if(_mouse == 0) skip(_mouseX >> 1, _mouseY >> 1);
    if(_mouse == 1) draw(_mouseX >> 1, _mouseY >> 1, 1); //draw a line if left button is pressed
}
```

#### ptrShape()

The `ptrShape(shape)` function sets the shape of the mouse pointer. The `shape` parameter is of type `byte` and can be one of the following values:

0. Arrow (default)
1. Reticle
2. Vertical bar
3. Hand

#### isPtrBox()

The `isPtrBox(x1, y1, x2, y2)` returns true if the mouse pointer is within the bounds of the provided box coordinates (expressed in full 800 × 480 resolution coordinates), false otherwise. The box is defined using the upper-left corner coordinates (parameters `x1, y1`) and the lower-right corner coordinates (parameters `x2, y2`). All parameters are of type `int`.

#### isPtrGlyph()

The `isPtrGlyph(index)` returns true if the mouse pointer is hovering over the character glyph corresponding to the `index` parameter, false otherwise. The index corresponds to the ASCII code for alphanumeric characters and, more generally, to glyph codes, as shown in Appendix B. The `index` parameter is of type `byte`.

#### isPtrSpr()

The `isPtrSpr(id)` returns true if the mouse pointer is within the bounds of the AABB of the sprite instance identified by the provided `id` parameter, false otherwise. The `id` parameter is of type `byte`, with an effective value in the range [0,31]. See Section 3.8 for more information about sprites' collision boxes.

#### isPtrChar()

The `isPtrChar(x, y)` returns true if the mouse pointer is hovering over a character at the position defined by the `x, y` parameters, false otherwise. The position is expressed in columns `x` and rows `y` with respect to the character matrix, as shown in Section 3.4. All parameters are of type `byte`.

#### isPtrTile()

The `isPtrTile(ref)` returns true if the mouse pointer is hovering over an instance of the tile referenced by the `ref` parameter, false otherwise. The `ref` parameter is of type `byte`, and within the range [0,255].

#### midiUpdt()

The incoming MIDI message stream (IN port) is parsed by the system interrupt handler, and events (such as Note On, Program Change...) are stored in a queue in the system, waiting to be consumed by a Lofi program. The queue can hold up to 16 events.

When events are present in the queue, the `midiUpdt()` function returns true and moves the oldest event content to four dedicated registers (starting at address `0xffd0`), to be read in Lofi. Please see `/examples/io/midi_in.src` for a simple MIDI input handling example.

#### midiSend()

The `midiSend(data)` function sends raw data of the type `byte` to the MIDI-OUT port. Any kind of MIDI message can be built by sending the appropriate bytes in quick succession.

> **Note:** Sending MIDI data involves UART polling. Since Micro-8 is single-threaded and MIDI runs only at 31.25 kbps, it might limit the performance of your program, depending on the circumstances.

Example:

```c
//the following will play a mid C note on channel 0
midiSend(0x90); //status -> Note ON, channel 0
midiSend(0x48); //note --> C4
midiSend(127); //velocity --> maximum
```

#### getKeyDwn()

Key presses are stored in a circular buffer within the OS by the keyboard interrupt handler, waiting to be consumed by a Lofi program. The buffer can hold up to 16 key presses. The history of the last 16 key presses is thus preserved.

When the `getKeyDwn()` function is called, the oldest key code (not the ASCII code) value is retrieved from the buffer in a First In First Out (FIFO) fashion and returned. The returned value is of type `byte`.

#### getKeyUp()

Key releases are stored in a circular buffer within the OS by the keyboard interrupt handler, waiting to be consumed by a Lofi program. The buffer can hold up to 16 key releases. The history of the last 16 key releases is thus preserved.

When the `getKeyUp()` function is called, the oldest key code (not the ASCII code) value is retrieved from the buffer in a First In First Out (FIFO) fashion and returned. The returned value is of type `byte`.

#### led()

The `led(r, g, b)` function allows setting the color of the case internal LED lighting. All parameters are of type `byte`.

#### portDir()

The `portDir(mask)` function allows enabling the user port and setting pin directions. The function must be called at least once to enable the user port. The `mask` parameter is of type `byte` and provides the direction of each I/O pin. Set a bit to '1' for output and '0' for input.

See Section 9.1 for more information about the user port.

#### portSet()

The `portSet(data)` function sets the pin values of the user port. The `data` parameter is of type `byte`. When the pin direction is set as output, the bit directly drives the output voltage of the pin either high (3.3 V) when set to '1', or low (ground) when set to '0'. When the pin direction is set as input, a bit set to '1' enables the internal pull-up resistor, and a bit set to '0' disables the internal pull-up resistor.

See Section 9.1 for more information about the user port.

### 7.2.2. Built-in variables

#### _port

The `_port` built-in variable gives the pin states of the user port. The value is of type `byte`, with each bit indicating whether the pin is high (when the bit value is '1') or low (when the bit value is '0').

See Section 9.1 for more information about the user port.

#### _ver

The `_ver` built-in variable gives the OS version number. The value is of type `nat`, with each of the three lower digits representing the Major release, the Minor release, and the Fix versions, respectively, as seen on the start screen in the x.x.x format. For example, the value 123 represents version 1.2.3.

Use this value for testing your program's potential (in)compatibility with earlier versions of the OS (e.g. if newer features are used).

Example:

```c
byte major=(byte)(_ver/100);
byte minor=(byte)((_ver%100)/10);
byte fix=(byte)(_ver%10);
print("You are currently running version: ",major,".",minor,".",fix);
```

#### _time

The `_time` built-in variable gives the elapsed time in seconds since the last power-on or hardware reset. The value is of type `nat`.

#### _mSec

The `_mSec` built-in variable provides the elapsed time in milliseconds since the last call to the `clkSet()` built-in function. The value is of type `nat`.

This variable is intended to measure short time intervals. Under the hood, there is a 16-bit hardware counter that increases at very short intervals to provide millisecond accuracy. As such, the maximum measurable time with `_mSec` is approximately 3.5 seconds. When the hardware counter exceeds its maximum value, it restarts at 0.

Some use cases:

- Profile performance
- Provide fixed time intervals

Example:

```c
clkSet();
//do some work here
print(_mSec); //elapsed time (in milliseconds) is printed
```

#### _curX

The `_curX` built-in variable provides the current cursor x position (column) with respect to the character matrix, as shown in Section 3.4. The value is of type `byte`.

#### _curY

The `_curX` built-in variable provides the current cursor y position (row) with respect to the character matrix, as shown in Section 3.4. The value is of type `byte`.

#### _keyDwn

The `_keyDwn` built-in variable provides the key code (not the ASCII code) of the currently pressed key. The value is of type `byte`. Key codes can be found in Appendix B.

Example:

```c
while(_keyDwn != 12)
{
    //do some work until the ESC key is pressed
}
```

#### _keyUp

The `_keyUp` built-in variable provides the key code (not the ASCII code) of the last key released. The value is of type `byte`. Key codes can be found in Appendix B.

#### _mouse

The `_mouse` built-in variable provides an identifier for the last used mouse button. The value is of type `byte`.

| Button | ID |
|---|---|
| Released | 0 |
| Left | 1 |
| Right | 2 |
| Middle | 3 |

#### _mouseX

The `_mouseX` built-in variable provides the absolute x mouse pointer coordinate in full 800×480 resolution, with the origin at the top-left corner of the display. The value is of type `int`.

#### _mouseY

The `_mouseY` built-in variable provides the absolute y mouse pointer coordinate in full 800×480 resolution, with the origin at the top-left corner of the display. The value is of type `int`.

#### _mouseDX

The `_mouseDX` built-in variable provides the relative x mouse displacement since the last sampling. The displacement is scaled by the current mouse sensitivity (see `mouseSens()` function above). To obtain an equivalent full 800×480 resolution pixel displacement, the resulting value should be divided by 16. The value is of type `int`.

#### _mouseDY

The `_mouseDX` built-in variable provides the relative y mouse displacement since the last sampling. The displacement is scaled by the current mouse sensitivity (see `mouseSens()` function above). To obtain an equivalent full 800×480 resolution pixel displacement, the resulting value should be divided by 16. The value is of type `int`.

#### _padLft

The `_padLft` built-in variable provides an 8-bit pattern in which each bit corresponds to a button on the left game controller. The variable is of type `byte` and is a mirror of the `0x1100` register. The bit pattern is as follows:

| Address | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|---|---|---|---|---|---|---|---|---|
| `0x1100` | Right | Left | Down | Up | Select | Start | B | A |

#### _padRght

The `_padRght` built-in variable provides an 8-bit pattern in which each bit corresponds to a button on the right game controller. The variable is of type `byte` and is a mirror of the `0x1101` register. The bit pattern is as follows:

| Address | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|---|---|---|---|---|---|---|---|---|
| `0x1101` | Right | Left | Down | Up | Select | Start | B | A |

## 7.3. Memory

### peek()

The `peek(address [, page])` function returns the value from the memory location specified by the provided `address`. The `address` parameter is of type `nat`. The returned value is of type `byte`.

An optional `page` argument, of type `byte`, allows moving the programmable CPU window (`0x3000` to `0xEEFF`) anywhere in the external SRAM in steps of 1024 bytes. The default page is set to the code and data sections used by Lofi. See Appendix E for more information on memory layouts.

Example:

```c
print(peek(0x3000)); //prints the value of the first opcode in code memory
plot(0,0,13);//plots a red pixel at coordinates (0,0)
print(peek(0x3000,0x60));//result --> 208 (the value of the byte containing the red pixel at coordinates (0,0))
```

### poke()

The `poke(address, value [, page])` function stores the provided `value` at the memory location specified by the provided `address`. The `address` parameter is of type `nat`. The `value` parameter is of type `byte`.

An optional `page` argument, of type `byte`, allows moving the programmable CPU window (`0x3000` to `0xEEFF`) anywhere in the external SRAM in steps of 1024 bytes. The default page is set to the code and data sections used by Lofi. See Appendix E for more information on memory layouts.

Example:

```c
text t="Sam is my name."; //store text in data segment
nat addr=&t; //get the address of text
poke(addr++,'T'); //change first character and increment address
poke(addr,'i'); //change second character
print(t); //Tim is my name.
```

### peekW()

The `peekW(address [, page])` function returns the raw 16-bit value from the memory locations at the provided `address`. The `address` parameter is of type `nat`. The returned value is considered raw (although technically it is of type `nat`), and it can be cast to its original 16-bit type, depending on the need.

An optional `page` argument, of type `byte`, allows moving the programmable CPU window (`0x3000` to `0xEEFF`) anywhere in the external SRAM in steps of 1024 bytes. The default page is set to the code and data sections used by Lofi. See Appendix E for more information on memory layouts.

### pokeW()

The `pokeW(address, value [, page])` function stores the provided 16-bit `value` at the memory locations specified by the `address`. The `address` parameter is of type `nat`. The provided value is considered raw (although technically, it is of type `nat`).

An optional `page` argument, of type `byte`, allows moving the programmable CPU window (`0x3000` to `0xEEFF`) anywhere in the external SRAM in steps of 1024 bytes. The default page is set to the code and data sections used by Lofi. See Appendix E for more information on memory layouts.

Example:

```c
pokeW(0x3f00, 60000); //store value in the scratchpad
print(peekW(0x3f00)); //--> 60000
pokeW(0x3f02, -500); //store another value in the scratchpad
print(peekW(0x3f02)); //--> 65036 (the raw representation of -500)
print((int)peekW(0x3f02)); //--> -500 the int value
```

### memSet()

The `memSet(address, size, value [, page])` function sets a linear block of memory of the provided `size` to the provided `value`, starting at the provided memory `address`. The `address` and `size` parameters are of type `nat`. The `value` parameter is of type `byte`.

An optional `page` argument, of type `byte`, allows moving the programmable CPU window (`0x3000` to `0xEEFF`) anywhere in the external SRAM in steps of 1024 bytes. The default page is set to the code and data sections used by Lofi. See Appendix E for more information on memory layouts.

Examples:

```c
memSet(0x3000,48000,0,0x60); //clears the (default) bitmap area
memSet(0x1200,3200,'A'); //fills the display with character A
```

### memCpy()

The `memCpy(srcAddress, dstAddress, size [, page])` function copies a linear block of memory, of the provided `size`, from the source address to the destination address. The `srcAddress`, `dstAddress` and `size` parameters are of type `nat`.

An optional `page` argument, of type `byte`, allows moving the programmable CPU window (`0x3000` to `0xEEFF`) anywhere in the external SRAM in steps of 1024 bytes. The default page is set to the code and data sections used by Lofi. See Appendix E for more information on memory layouts.

Example:

```c
byte arrayA[10]={1,2,3,4,5,6,7,8,9,10};
byte arrayB[10];
memCpy(&arrayA, &arrayB, 10);//copy the contents of array A to array B
```

### areaSet()

The `areaSet(address, stride, x, y, width, height, value [, page])` function sets a 2D area of bytes in the provided buffer to the specified value. The `address` parameter is the starting address of the buffer and is of type `nat`. The `stride` parameter corresponds to the width of the buffer in bytes. The `x, y, width, height` parameters, of type `int`, define the position and size of the area with a top-left origin. The `value` parameter is of type `byte`.

An optional `page` argument, of type `byte`, allows moving the programmable CPU window (`0x3000` to `0xEEFF`) anywhere in the external SRAM in steps of 1024 bytes. The default page is set to the code and data sections used by Lofi. See Appendix E for more information on memory layouts.

Example:

```c
//the character buffer has a width of 80
//we use that value as the stride value for setting a character area:
areaSet(0x1200,80,10,10,40,20,'x');//a rectangle of 'x' in the character glyph buffer
```

> **Note:** While the `areaSet()` function is byte-aligned, pixel graphics in Micro-8 are nibble-aligned. Look for the `box()` function in Section 7.8 if you want to set an area of pixels.

### areaCpy()

The `areaCpy(srcBuffer, srcStride, srcX, srcY, width, height, dstBuffer, dstStride, dstX, dstY [, page])` function performs a 2D byte area copy from a source memory buffer to a destination buffer. Source and destination buffers can be the same, but overlapping areas may result in unexpected behavior. The `stride` parameters correspond to the width of the buffers in bytes. The `srcX, srcY, width, height` parameters define the source area with a top-left origin. The `dstX, dstY` parameters provide the coordinates that define the top-left position of the area in the destination buffer.

An optional `page` argument, of type `byte`, allows moving the programmable CPU window (`0x3000` to `0xEEFF`) anywhere in the external SRAM in steps of 1024 bytes. The default page is set to the code and data sections used by Lofi. See Appendix E for more information on memory layouts.

Example:

```c
//the character buffer has a width of 80
areaCpy(0x1200,80,32,1,16,6,0x1200,80,20,20);//copy the Micro-8 logo to another display location
areaCpy(0x1e8,80,32,1,16,6,0x1e8,80,20,20);//copy the colors
```

> **Note:** While the `areaCpy()` function is byte-aligned, pixel graphics in Micro-8 are nibble-aligned. Look for the `blit()` function in Section 7.8 if you want to perform per-pixel area copies.

## 7.4. File system

### 7.4.1. High-level functions

#### import()

The `import(file name, address, size [, page])` function reads a data file with the provided `file name` and specified linear `size` to the designated `address` in memory. When the size is unknown in advance, setting a `size` parameter of 0 will perform a read of the file's size. This should only be performed with sufficient memory space allocated; otherwise, adjacent memory will be overwritten. The size of the read data can be obtained using the built-in `_fSize` variable. The `file name` parameter is of type `text`. The `address` and `size` parameters are of type `nat`.

An optional `page` argument, of type `byte`, allows moving the programmable CPU window (`0x3000` to `0xEEFF`) anywhere in the external SRAM in steps of 1024 bytes. The default page is set to the code and data sections used by Lofi. See Appendix E for more information on memory layouts.

The following example assumes the existence of the adequate resource folder and data:

```c
import("res/sprites.dat", 0x3000, 0, 0x20);//load a sprite set of unknown size to default location in XSRAM
nat spriteNum = _fSize/256;//number of sprites read
```

#### export()

The `export(file name, address, size [, page])` function writes a file with the provided `file name` and the specified linear `size`, using data from the specified `address` in memory. The `file name` parameter is of type `text`. The `address` and `size` parameters are of type `nat`.

An optional `page` argument, of type `byte`, allows moving the programmable CPU window (`0x3000` to `0xEEFF`) anywhere in the external SRAM in steps of 1024 bytes. The default page is set to the code and data sections used by Lofi. See Appendix E for more information on memory layouts.

Example:

```c
export("charbuff.dat", 0x1200, 3200);//save the display character glyph buffer to file
```

#### import2D()

The `import2D(file name, address, stride, width, height [, page])` function reads byte data from a file and writes it to a 2D buffer memory starting at the specified `address`. The `stride` parameter specifies the width of the destination buffer, and the `width` and `height` parameters specify the size of the area. The `file name` parameter is of type `text`. The `address, stride, width, height` parameters are of type `nat`.

An optional `page` argument, of type `byte`, allows moving the programmable CPU window (`0x3000` to `0xEEFF`) anywhere in the external SRAM in steps of 1024 bytes. The default page is set to the code and data sections used by Lofi. See Appendix E for more information on memory layouts.

#### export2D()

The `export2D(file name, address, stride, width, height [, page])` function writes a 2D array of byte data to a file. The data starts from the specified `address` in memory. The `stride` parameter specifies the width of the source buffer. The `width` and `height` parameters specify the size of the area. The `file name` parameter is of type `text`. The `address, stride, width, height` parameters are of type `nat`.

An optional `page` argument, of type `byte`, allows moving the programmable CPU window (`0x3000` to `0xEEFF`) anywhere in the external SRAM in steps of 1024 bytes. The default page is set to the code and data sections used by Lofi. See Appendix E for more information on memory layouts.

Example:

```c
export2D("logo.dat", 0x1200 + 80 + 35, 80, 10, 2);//save the introductory Micro-8 logo to file
```

### 7.4.2. Low-level functions

#### fCD()

The `fCD(path)` function allows changing the current directory. The `path` parameter is of type `text`.

Special characters are used to navigate through the hierarchical structure:

- `/` is the root folder if used alone or in front of a path. Otherwise, it is used as a child separator.
- `.` is the current folder.
- `..` is the parent folder.

#### fMkDir()

The `fMkDir([path/]name)` function allows for the creation of a new directory. The `[path/]name` parameter is of `text` type.

#### fDel()

The `fDel([path/]name)` function allows for the deletion of a file or an empty directory. The `[path/]name` parameter is of `text` type.

#### fMov()

The `fMov([path/]name, [path/][newName])` function allows renaming or moving a file from the name/location specified by the first parameter to a new name/location. The `[path/]original` and `[path/][newName]` parameters are of `text` type. When the `[path/][newName]` parameter is a path ending with a `/` character, the original name is preserved, and the file is simply moved as is.

Examples:

```c
fMov("test.src","example.src"); //rename the file
fMov("test.src","/user/"); //moves the file to the /user folder
fMov("test.src","/user/test1.src"); //moves the file to the /user folder and renames it to test1.src
```

#### fCpy()

The `fCpy([path/]name, [path/][newName])` function allows copying a file from the location specified by the first parameter to a new location, possibly with a new name. The `[path/]original` and `[path/][newName]` parameters are of `text` type. When the `[path/][newName]` parameter is a path ending with a `/` character, the original name is preserved, and the file is simply copied as it is.

Examples:

```c
fCpy("test1.src","test2.src"); //copy the file in the same directory
fCpy("test.src","/user/"); //makes a copy of the file in the /user folder
fCpy("test.src","/user/test1.src"); //makes a copy of the file in the /user folder with the new name test1.src
```

> **Warning:** The `fCpy()` function makes use of the Scratchpad (See Memory Map at Section E) as a temporary buffer during the copy, erasing any data that might have been present in the process. If you intend to use both the Scratchpad and the `fCpy()` function in your program, make sure that the data in the Scratchpad is irrelevant or has been backed up to another memory location before calling the `fCopy()` function.

#### fOpDir()

The `fOpDir(path)` function opens the specified directory for reading. This function must be called before using the `fRdDir` function. The `path` parameter is of `text` type.

#### fRdDir()

The `fRdDir(nameAddress, sizeAddress)` retrieves an item from the opened directory (See `fOpDir()` function). The function returns true if the item is a subdirectory, false otherwise (is a file). The parameters `nameAddress` and `sizeAddress` are output parameters. They are the addresses of two variables that must be defined prior to calling the function. The first, of type `text`, will contain the name of the found item. The second, of type `nat`, will contain the size of the found item.

When the returned name is empty, there are no more items in the directory.

The following example can also be found in `/examples/files/`:

```c
//display a directory
text name={12};//variable to hold the name of an item
nat size;//variable to hold the size of an item
bool subd;
fOpDir(".");//open current dir for reading
while(true)//iterate over items
{
    subd=fRdDir(&name, &size);//read item from opened dir
    if(peek(&name)==0) break;//exit if no more items
    if(subd) print("[FG]a", name, "⟲⏎");//sub dir in green color
    else print(name, "⇥", size, "⇥Bytes⏎");//file with size
}
```

#### fOpen()

The `fOpen(descriptorID, fileName)` function opens an existing file or creates a new file and opens it if the provided `fileName` parameter, of type `text`, is a new name. The file is opened for both reading and writing. The `descriptorID` parameter is either 0 or 1 and allows for working on up to two files simultaneously. The `descriptorID` parameter is of type `byte`. This function must be called before using the functions `fRead()` or `fWrite`.

#### fClose()

The `fClose(descriptorID)` function closes the file that was opened with the same descriptor ID. The `descriptorID` parameter is of type `byte` and can be 0 or 1. This function must be called when accessing the file is no longer necessary, typically after using the functions `fRead()` or `fWrite()`.

#### fSize()

The `fSize(descriptorID)` function returns the size, in bytes, of the file that was opened with the same descriptor ID. The `descriptorID` parameter is of type `byte` and can be 0 or 1. The returned value is of type `nat`.

#### fRead()

The `fRead(descriptorID, destAddr, offset, size [,page][,fPage])` function reads data from the file that was opened with the same descriptor ID. The `descriptorID` parameter is of type `byte` and can be 0 or 1. The `destAddr` parameter provides the memory address where the read data should be stored. The `offset` parameter specifies the position (in bytes) where to read the data in the file. An offset of 0 corresponds to the beginning of the file. The `offset` parameter is of type `nat`. The `size` parameter specifies the number of bytes to read; it is of type `nat`.

An optional `page` argument, of type `byte`, allows moving the programmable CPU window (`0x3000` to `0xEEFF`) anywhere in the external SRAM in steps of 1024 bytes. The default page is set to the code and data sections used by Lofi. See Appendix E for more information on memory layouts.

An optional `fPage` argument, of type `byte`, allows moving the file pointer by multiples of 32 KiB (32768 bytes). This, in combination with the `offset` value, provides access to data in files larger than 64 KiB. The final pointer position is `fPage × 32768 + offset`. Files of up to 8 MiB can be accessed with this method. The read data is still limited to chunks of 64 KiB maximum, though.

#### fWrite()

The `fWrite(descriptorID, srcAddr, offset, size [,page][,fPage])` function writes data to the file that was opened with the same descriptor ID. The `descriptorID` parameter is of type `byte` and can be 0 or 1. The `srcAddr` parameter provides the memory address from which the data to be written to the file originates. The `offset` parameter specifies the position (in bytes) where to write the data in the file. An offset of 0 corresponds to the beginning of the file. As the offset or the written data moves past the end of the file, the size of the file is automatically extended. Note that if the file is extended with an offset and no data is written in-between, the data in the extended part is undefined. The `offset` parameter is of type `nat`. The `size` parameter specifies the number of bytes to write; it is of type `nat`.

An optional `page` argument, of type `byte`, allows moving the programmable CPU window (`0x3000` to `0xEEFF`) anywhere in the external SRAM in steps of 1024 bytes. The default page is set to the code and data sections used by Lofi. See Appendix E for more information on memory layouts.

An optional `fPage` argument, of type `byte`, allows moving the file pointer by multiples of 32 KiB (32768 bytes). This, in combination with the `offset` value, provides access to data in files larger than 64 KiB. The final pointer position is `fPage × 32768 + offset`. Files of up to 8 MiB can be accessed with this method. The written data is still limited to chunks of 64 KiB maximum, though.

Example:

```c
//create a new file and write some text to it
text t="Hello world!";
fOpen(0,"hello.txt"); //create a new file (if it does not exist) using descriptor 0
fWrite(0,&t,0,#t); //write the 'Hello world!' text at the beginning of the file (and extend the file size accordingly)
fClose(0); //close the file
```

### 7.4.3. Built-in variables

#### _fSize

The `_fSize` built-in variable gives the size of the last file read using the `import()` function. This is useful when the size of the imported data is not known in advance.

#### _fError

The `_fError` built-in variable provides the error code following the use of a file system command. The value is of type `byte`, and a value of 0 is given when there is no error. For a list of all possible errors, please check the file error codes in Section B.5.

## 7.5. Math

### rndSeed()

The `rndSeed(seed)` function sets the seed for the software pseudo-random number generator. This function should be called only once during initialization. It allows the generator to consistently generate the same sequence of pseudo-random numbers from session to session using the `rnd()` function if the same seed is provided. The `seed` parameter is of type `nat`.

### rnd()

The `rnd(min, max)` function returns a pseudo-random value in the [min,max] range. The first parameter can be any expression of type `byte`, `nat`, `int`, or `real`. The second parameter type should be the same as the first parameter type or of a lower type. The return type depends on the type of the first parameter.

Examples:

```c
print(rnd(-100,200));//return type int
print(rnd(0.0,1.0));//return type real
while(_keyDwn != 12)
{
//plot random points of random colors, esc to quit
plot(rnd(0,399), rnd(0,239), rnd(0,15));
}
```

### abs()

The `abs(x)` function returns the absolute value of `x`. The `x` parameter can be any expression of `byte`, `nat`, `int`, or `real` types. The return type depends on the parameter type.

### min()

The `min(x, y)` function returns the smallest of its parameters. The first parameter can be any expression of type `byte`, `nat`, `int`, or `real`. The second parameter type should be the same as the first parameter type or of a lower type. The return type depends on the type of the first parameter.

Examples:

```c
print(min(-4,56)); //-4 -> return type int
real pi=3.1415;
print(min(pi,sqrt(2.0))); //1.414 -> return type real
```

### max()

The `max(x, y)` function returns the largest of its parameters. The first parameter can be any expression of type `byte`, `nat`, `int`, or `real`. The second parameter type should be the same as the first parameter type or of a lower type. The return type depends on the type of the first parameter.

Examples:

```c
print(max(-12,-1)); //-1 -> return type int
real pi=3.1415;
print(max(pi,2*pi)); //6.281 -> return type real
```

### clamp()

The `clamp(input, min, max)` function returns the `input` value clamped within the bounds of the [min,max] range, as shown in Expression 7.1. The `input` parameter can be any expression of type `byte`, `nat`, `int`, or `real`. The `min` and `max` parameter types should be the same as the first parameter type or of a lower type. The return type depends on the type of the first parameter.

```text
         | min     if input < min
result = | max     if input > max          (7.1)
         | input   otherwise
```

Examples:

```c
print(clamp(-12,0,255)); //0 -> return type int
print(clamp(2.5,0,1)); //1 -> return type real
real pi=3.1416;
print(clamp(.707,0,2*pi)); //0.707 -> return type real
```

### lerp()

The `lerp(x, y, t)` function returns a linearly interpolated value between `x` and `y`, weighted by the interpolant `t`, as shown in Expression 7.2. The `t` parameter must be of type `real` and should be in the [0,1] range (extrapolation will occur if `t` is outside this range). The `x` parameter can be any expression of type `byte`, `nat`, `int`, or `real`. The `y` parameter should be of the same type as the first parameter or of a lower type. The return type depends on the type of the first parameter.

```text
result = y*t + x*(1 - t)          (7.2)
```

Examples:

```c
print(lerp(6,10,.5)); //8 -> return type int
print(lerp(1.0,2.0,.25)); //1.25 -> return type real
```

### sqrt()

The `sqrt(x)` function returns the square root of `x`. The `x` parameter can be any expression of type `byte`, `nat`, `int`, or `real`. The return type depends on the parameter type.

Examples:

```c
print(sqrt(25)); //5 -> return type int
print(sqrt(2.0)); //1.414 -> return type real
```

### pow()

The `pow(x, y)` function computes `x` to the power of `y`. The `x` parameter can be any expression of type `byte`, `nat`, `int`, or `real`. If `x` is of type `real`, there are no constraints regarding the second `y` parameter (besides representation limits). In all other cases, the second `y` parameter must be of type `byte`. If the first parameter is of type `real`, the returned value will be of type `real`. In all other cases, the returned value will be of type `int`.

Examples:

```c
print(pow(5,3)); //125 --> return type int
print(pow(5,-3)); //illegal
print(pow(-3,3)); //-27 --> return type int
print(pow(2.0,-1)); //0.5 --> return type real
print(pow(-2.0,-3)); //-0.125 --> return type real
```

### sin()

The `sin(x)` function returns the sine of `x`. The parameter must be of type `real` and expressed in radians. The return type is `real`.

Examples:

```c
real pi=3.1416;
print(sin(.0)); //0
print(sin(pi/2)); //1
print(sin(pi/4)); //0.707
```

### cos()

The `cos(x)` function returns the cosine of `x`. The parameter must be of type `real` and expressed in radians. The return type is `real`.

Examples:

```c
real pi=3.1416;
print(cos(.0)); //1
print(cos(pi/2)); //0
print(cos(pi/4)); //0.707
```

### atan2()

The `atan2(y, x)` function returns the principal arc-tangent of y/x in the [-π,π] range. The `y` and `x` parameters (in that order) can be any expression of type `byte`, `nat`, `int` or `real`. The returned value is of type `real`.

## 7.6. Video

### vSync()

The `vSync()` function waits for the vertical blanking of the video signal. Graphics updates and buffer swapping can then be performed during vertical blanking without display artifacts.

The default setting is configured to a single frame, enabling a 60 Hz frame update. For projects requiring more frame time, the number of frames available before waiting for vertical blanking can be set using the `vCount()` function.

### vCount()

The `vCount(n)` function allows setting the number of frames to count before waiting for vertical blanking. The `n` parameter is of type `byte`. The update frequency will be 60/n Hz. The default value is 1.

### rstrInt()

The `rstrInt(enable)` function allows for enabling or disabling the raster interrupt mechanism. The `enable` parameter is of type `bool`. Raster interrupts are enabled when `enable` is set to `true`; they are disabled otherwise. See Section 3.9 for more information about raster interrupts.

### rstrCmp()

The `rstrCmp(line, address)` function sets the `line` value to be used by the hardware raster line comparator and the `address` of the first instruction to be executed in the display list. Both the `line` and `address` parameters are of type `nat`. See Section 3.9 for more information about raster interrupts.

## 7.7. Character glyphs

### glyphSet()

The `glyphSet(address, pixel)` function allows setting or clearing a pixel in the character glyph set memory. Each glyph is 10×12 pixels. The glyph set (as shown in Appendix B) is 160×192 pixels and is stored linearly in a 1-bit wide, 30,720-bit long memory. The pixel `address` parameter is of `nat` type with a valid range of [0,30719]. The `pixel` parameter is of type `byte` with a valid range of [0,1].

Example:

```c
nat address = 32 * 10 * 12; //32 is the glyph code for space
glyphSet(address, true);//sets the first pixel of the space glyph
```

See Chapter 3 for more information about the usage of text and glyphs.

## 7.8. Bitmap graphics

Bitmap graphics API functions provide immediate access to the bitmap draw buffer. The bitmap buffer has a default visible area of 400×240 pixels. The coordinates are expressed as integer values within the [0,399] range for x and the [0,239] range for y. Coordinates have a top-left origin, as shown in Figure 3.3. When using API functions, clipping is handled by the system and should not be a concern for the user.

Colors are expressed as indices to the bitmap palette (within the [0,15] range). The actual color will depend on the RGB values set in the bitmap palette at that particular index.

> **Note:** The upper nibble of the `color` parameter (in the supporting bitmap functions below) can be used as a 4-bit mask to perform an XOR operation with the underlying pixel at draw time. When using a non-zero mask as the upper nibble color, the lower nibble is no longer relevant. A `0xf0` value, for instance, will inverse all bits of the affected pixel. Calling the same primitive again with the same value will restore the original color.

### bmpWin()

The `bmpWin(minX, minY, maxX, maxY)` function allows defining the clipping window for all bitmap-related built-in API functions. The default window covers the entire bitmap draw buffer, as described in Section 3.5. Valid x coordinates are within the [0,399] range. Valid y coordinates are within the [0,239] range. All parameters are of type `int`.

### bmpOrg()

The `bmpOrg(x, y)` function allows defining the origin coordinates for all bitmap related built-in API functions. The default origin is (0,0). Valid x coordinates are within the [0,399] range. Valid y coordinates are within the [0,239] range. All parameters are of type `int`.

### plot()

The `plot(x, y, color)` function plots a pixel at coordinates (x, y) expressed in low-resolution pixels. The `x` and `y` parameters are of type `int`. The `color` parameter is of type `byte`.

The lower nibble of the `color` argument is an index to the bitmap palette (within the [0,15] range). The upper nibble (if not set to 0) is a mask to perform an XOR operation with the underlying pixel.

As a side effect, `plot()` also updates the current position (see the `draw()` function for more information about the use of the current position).

Example:

```c
for(int i; i<1000; i++)
{
plot(rnd(0,399), rnd(0,239), rnd(0,15)); //plots 1000 points of random position and color
}
```

### skip()

The `skip(x, y)` function updates the current position to the (x, y) coordinates, expressed in low-resolution pixels. This function does not draw any pixels and is typically used in conjunction with the `draw()` function. Both parameters are of type `int`.

### draw()

The `draw(x, y, color)` function draws a line from the current position to the (x, y) coordinates, expressed in low-resolution pixels. The `x` and `y` parameters are of type `int`. The `color` parameter is of type `byte`.

The lower nibble of the `color` argument is an index to the bitmap palette (within the [0,15] range). The upper nibble (if not set to 0) is a mask to perform an XOR operation with the underlying pixel.

As a side effect, `draw()` also updates the current position, so successive calls to the `draw()` function allow for continuous line drawings.

Example:

```c
skip(200,120); //set current position to the center of the screen
draw(399,239,1); //draws a line from the center of the screen to the bottom right corner
```

### line()

The `line(x0, y0, x1, y1, color)` function draws a line from the (x0, y0) coordinates to the (x1, y1) coordinates. Coordinates are expressed in low-resolution pixels. The `x0`, `y0`, `x1`, `y1` parameters are of type `int`. The `color` parameter is of type `byte`.

The lower nibble of the `color` argument is an index to the bitmap palette (within the [0,15] range). The upper nibble (if not set to 0) is a mask to perform an XOR operation with the underlying pixel.

As a side effect, `line()` also updates the current position (see the `draw()` function for more information about the use of the current position).

Example:

```c
for(int x; x<400; x=x+10)
{
line(x,0,399-x,239,(x/10)%16); //colored lines crossing at the center of the screen
}
```

### circle()

The `circle(x, y, radius, color)` function draws a circle of radius `radius` centered around the (x, y) coordinates. Coordinates and radius are expressed in low-resolution pixels. The `x`, `y`, `radius` parameters are of type `int`. The `color` parameter is of type `byte`.

The lower nibble of the `color` argument is an index to the bitmap palette (within the [0,15] range). The upper nibble (if not set to 0) is a mask to perform an XOR operation with the underlying pixel.

Example:

```c
for(byte r; r<240; r+=10)
{
circle(200,120,r,r%16); //color circles of increasing size
}
```

### disk()

The `disk(x, y, radius, color)` function draws a plain disk of radius `radius` centered around the (x, y) coordinates. Coordinates and radius are expressed in low-resolution pixels. The `x`, `y`, `radius` parameters are of type `int`. The `color` parameter is of type `byte`.

The lower nibble of the `color` argument is an index to the bitmap palette (within the [0,15] range). The upper nibble (if not set to 0) is a mask to perform an XOR operation with the underlying pixel.

Example:

```c
for(byte r=240; r>0; r-=10)
{
disk(200,120,r,r%16); //color disks of decreasing size
}
```

### rect()

The `rect(x0, y0, x1, y1, color)` function draws a rectangle from the upper-left corner defined by (x0, y0) coordinates to the lower-right corner defined by (x1, y1) coordinates. Coordinates are expressed in low-resolution pixels. The `x0`, `y0`, `x1`, `y1` parameters are of type `int`. The `color` parameter is of type `byte`.

The lower nibble of the `color` argument is an index to the bitmap palette (within the [0,15] range). The upper nibble (if not set to 0) is a mask to perform an XOR operation with the underlying pixel.

### box()

The `box(x0, y0, x1, y1, color)` function draws a filled rectangle from the upper-left corner defined by (x0, y0) integer coordinates to the lower-right corner defined by (x1, y1) integer coordinates. Coordinates are expressed in low-resolution pixels. The `x0`, `y0`, `x1`, `y1` parameters are of type `int`. The `color` parameter is of type `byte`.

The lower nibble of the `color` argument is an index to the bitmap palette (within the [0,15] range). The upper nibble (if not set to 0) is a mask to perform an XOR operation with the underlying pixel.

This function can also be used to clear the bitmap screen or a particular bitmap window previously set with the `bmpWin()` function by providing the value 0 as the `color` parameter.

### tri()

The `tri(x0, y0, x1, y1, x2, y2, color)` function draws a filled triangle from the three vertices defined by the (x0, y0), (x1, y1) and (x2, y2) coordinates. Coordinates are expressed in low-resolution pixels. The `x0`, `y0`, `x1`, `y1`, `x2`, `y2` parameters are of type `int`. The `color` parameter is of type `byte`.

The lower nibble of the `color` argument is an index to the bitmap palette (within the [0,15] range). The upper nibble (if not set to 0) is a mask to perform an XOR operation with the underlying pixel.

### blit()

The `blit(srcBuffer, srcStride, srcX, srcY, width, height, dstX, dstY)` function performs a 2D pixel area copy from a given (srcX,srcY) coordinate in the source buffer to the (dstX,dstY) coordinate in the bitmap draw buffer. The `width` and `height` parameters define the size of the copy. The `srcBuffer` parameter is of type `nat`. The `srcStride` parameter specifies the width in bytes of the source buffer and is of type `nat`. The `srcX`, `srcY`, `width`, `height`, `dstX`, `dstY` parameters are of type `int`. Transparent (color index 0) pixels from the source image will not be rendered in the bitmap draw buffer. The source buffer can be the bitmap draw buffer itself, but overlapping areas may result in unexpected behavior. The source buffer would typically be the scratchpad (see Appendix E for more information about memory layouts).

> **Note:** This function is implemented in software and is provided for convenience. Do not expect hardware blitter level of performance. It can, however, be very useful for tools (e.g. brushes) or non-real-time usage (e.g. precomputing objects layering or procedural image composition).

Example:

```c
nat scratchpad = 0xf000;
//import some 64x64 image in the scratchpad here
blit(scratchpad,32,0,0,64,64,168,88);//will blit the 64x64 image in the center of the screen
```

### pick()

The `pick(x, y)` function returns the color index of the bitmap pixel at coordinates (x, y) expressed in low resolution pixels (full screen window). All parameters are of type `int`. The returned value is of type `byte`.

### bmpToTile()

The `bmpToTile(x, y, w, h, ref, zoom)` function allows for copying an area of bitmap graphics to a set of tiles arranged in a grid pattern.

The `x` parameter is of type `int` and is the byte-aligned horizontal position of the top-left corner of the source in the bitmap. This is essentially the horizontal low-resolution pixel position divided by 2. The `y` parameter is of type `int` and represents the vertical low-resolution pixel position of the top-left corner of the source.

The `w` and `h` parameters are of type `byte` and represent the number of tiles in width and height that comprise the destination tile array.

The `ref` parameter is of type `byte` and is the first tile reference in the tile-set to which the copy will be performed. As an example, if `w` and `h` are set to 2 and 3, respectively, and `ref` is set to 8, the copy will affect tile references 8, 9, 10, 11, 12, and 13 in the tile set.

During the copy, a scale factor can be applied. The `zoom` parameter supports a range from 1 to 4. The actual scale will be 2^(zoom-1). With a `zoom` value of 1, a pixel-perfect copy is performed. With a `zoom` value of 4, one source pixel is written as a group of 8×8 pixels at the destination.

Because bitmap graphics are linear and tile graphics are not, this function allows all sorts of dynamic tile rendering by reusing all the readily available bitmap functions.

### bmpToSpr()

The `bmpToSpr(x, y, w, h, ref)` function allows for copying an area of bitmap graphics to a set of sprites arranged in a grid pattern.

The `x` parameter is of type `int` and is the byte-aligned horizontal position of the top-left corner of the source in the bitmap. This is essentially the horizontal low-resolution pixel position divided by 2. The `y` parameter is of type `int` and represents the vertical low-resolution pixel position of the top-left corner of the source.

The `w` and `h` parameters are of type `byte` and represent the number of sprites in width and height that make up the destination sprite array.

The `ref` parameter is of type `byte` and is the first image reference in the sprite-set to which the copy will be performed. As an example, if `w` and `h` are set to 2 and 3, respectively, and `ref` is set to 8, the copy will affect images 8, 9, 10, 11, 12, and 13 in the sprite-set.

If six sprite instances are arranged in the same grid pattern as the above example and reference the aforementioned successive images, they will display the copied bitmap content.

Because bitmap graphics are linear and sprite graphics are not, this function allows all sorts of dynamic sprite rendering by reusing all the readily available bitmap functions.

## 7.9. Tile graphics

Tile graphics heavily rely on generic buffer copying and register settings using functions such as `areaCpy()` and `poke()`. As such, the Tile API functions are sparse. For more information about Tile graphics usage in Lofi, please refer to the `/DEMOS/TILES/TILES.SRC` example. For more information about hardware tiles, see Section 3.7.

### tileGetRef()

The `tileGetRef(x, y)` function returns the reference of the tile found at coordinates (x, y). The coordinates are expressed in full 800×480 resolution. The `x` and `y` parameters are of type `int`. The returned value is of type `byte`. The tile reference is extracted from the currently displayed tile buffer (not from the tile draw buffer, if any).

### tilePanUp()

The `tilePanUp(reference)` function performs a single pixel software panning of the referenced tile in the tile-set in the upward direction. This function provides dynamic modification of tile-set contents and allows for background animations or multiple parallax scrolling effects. The `reference` parameter is of type `byte`.

### tilePanDwn()

The `tilePanDwn(reference)` function performs a single pixel software panning of the referenced tile in the tile-set in the downward direction. This function provides dynamic modification of tile-set contents and allows for background animations or multiple parallax scrolling effects. The `reference` parameter is of type `byte`.

### tilePanLft()

The `tilePanLft(reference)` function performs a single pixel software panning of the referenced tile in the tile-set in the left direction. This function provides dynamic modification of tile-set contents and allows for background animations or multiple parallax scrolling effects. The `reference` parameter is of type `byte`.

### tilePanRght()

The `tilePanRght(reference)` function performs a single pixel software panning of the referenced tile in the tile-set in the right direction. This function provides dynamic modification of tile-set contents and allows for background animations or multiple parallax scrolling effects. The `reference` parameter is of type `byte`.

## 7.10. Sprite graphics

Sprite graphics API functions provide easy access to the hardware sprite capabilities of Micro-8. For more information about hardware sprites, see Section 3.8.

> **Note:** All code examples in this section assume sprites are enabled and some sprite data has been loaded into memory prior to running the examples.

### sprPal()

The `sprPal(id, palette)` function allows for the selection of which of the two available palettes to use on the identified sprite instance. The `id` parameter is of type `byte` with an effective value in the [0,31] range. The `palette` parameter is of type `byte` and is either 0 (palette A) or 1 (palette B).

The function is meant to be primarily used during initialization, and its effect is immediate.

Example:

```c
for(byte id; id<32; id++)
{
sprPal(id,0);//all sprites use first palette
}
```

### sprPrio()

The `sprPrio(id, priority)` function allows the selection of the priority of the identified sprite instance. The `id` parameter is of type `byte` with an effective value in the [0,31] range. The `priority` parameter is of type `byte` and is either 0 or 1. A priority of 0 sets the display of the sprite above the tiles layer, while a priority of 1 sets the display of the sprite below the tiles layer.

The function is meant to be primarily used during initialization, and its effect is immediate.

Example:

```c
for(byte id; id<32; id++)
{
sprPrio(id,id%2);//all even sprites go below the tiles layer, all odd sprites go on top
}
```

### sprRef()

The `sprRef(id, reference)` function allows for the selection of the content of the identified sprite instance from the sprite set. The `id` parameter is of type `byte` with an effective value in the [0,31] range. The `reference` parameter is of type `byte` with a maximum range of [0,255]. However, depending on the sprite set size of a particular project, the available range might be smaller.

As the usage of this function is meant to be recurrent (i.e. called every frame), the effect of the function is deferred. The function writes to a sprite list and not to the sprite registers themselves. The change to the sprite registers will occur when the `sprUpdate()` function is called, typically during vertical blanking.

Example:

```c
byte frame;
while(true)
{
sprRef(0,(frame>>1)%16);//set reference to animate sprite 0 over a 16 frames loop @ 30fps
vSync(); //wait for vertical blanking
sprUpdt(); //update sprite registers during blanking
frame++;
}
```

### sprPos()

The `sprPos(id, x, y)` function allows setting the position of the identified sprite instance. The `id` parameter is of type `byte` with an effective value in the [0,31] range. The `x` and `y` parameters are of type `int`.

The given (x, y) coordinates correspond to the top-left corner of the sprite. The position is specified at sub-pixel resolution (800×480) starting at 48 on the left of the display and 42 at the top. A sprite can thus be hidden by setting its position to (0,0). To place a sprite in the top-left corner of the display, set its position to (48,42).

As the usage of this function is meant to be recurrent (i.e. called every frame), the effect of the function is deferred. The function writes to a sprite list and not to the sprite registers themselves. The change to the sprite registers will occur when the `sprUpdate()` function is called, typically during vertical blanking.

Example:

```c
int x=376; //starts in the middle
int speed=10;
while(true)
{
if(x>802 || x<48) speed = -speed; //bounces left and right
x+=speed;
sprPos(0,x,261); //set first sprite's position in the sprite list
vSync(); //wait for vertical blanking
sprUpdt(); //update sprite registers during blanking
}
```

### sprFlip()

The `sprFlip(id, x, y)` function allows flipping the image of the identified sprite instance in the horizontal and vertical directions. The `id` parameter is of type `byte` with an effective value in the [0,31] range. The `x` and `y` parameters are of type `bool`.

As the usage of this function is meant to be recurrent (i.e. called every frame), the effect of the function is deferred. The function writes to a sprite list and not to the sprite registers themselves. The change to the sprite registers will occur when the `sprUpdate()` function is called, typically during vertical blanking.

### sprUpdt()

The `sprUpdt()` function performs the update of all sprite references, positions, and flipping from the sprite list to the actual sprite hardware registers. This function typically allows for deferring the update to the vertical blanking. This way, we can avoid screen tearing or other artifacts, as sprite registers are not double-buffered.

The entire frame is thus available prior to performing the update to handle sprite references, positions, and flipping calculations.

### isSprSpr()

The `isSprSpr(id1, id2)` function performs a collision test between the AABBs of the two specified sprite instances. It returns `true` if a collision occurs, `false` otherwise. The `id1` and `id2` parameters are of type `byte` with an effective value in the [0,31] range.

Collision detection is calculated based on the actual (visible) sprite instance position, not on the deferred ones.

See Section 3.8 for more information about sprites and their associated AABB.

### isSprBox()

The `isSprBox(id, x1, y1, x2, y2)` function performs a collision test between the AABB of the specified sprite instance and the provided box coordinates (expressed in full 800×480 resolution coordinates). It returns `true` if a collision occurs, `false` otherwise. The box is defined using the upper-left corner coordinates (x1, y1) and the lower-right corner coordinates (x2, y2). The `id` parameter is of type `byte` with an effective value in the [0,31] range. The `x1`, `y1`, `x2`, `y2` parameters are of type `int`.

### isSprPoint()

The `isSprPoint(id, x, y)` function performs a collision test between the AABB of the specified sprite instance and the provided point coordinates (expressed in full 800×480 resolution coordinates). It returns `true` if a collision occurs, `false` otherwise. The `id` parameter is of type `byte` with an effective value in the [0,31] range. The `x`, `y` parameters are of type `int`.

### isSprLine()

The `isSprLine(id, x1, y1, x2, y2)` function performs a collision test between the AABB of the specified sprite instance and a line segment defined by the (x1, y1) and (x2, y2) line coordinates (expressed in full 800×480 resolution coordinates). It returns `true` if a collision occurs, `false` otherwise. The `id` parameter is of type `byte` with an effective value in the [0,31] range. The `x1`, `y1`, `x2`, `y2` parameters are of type `int`.

### isSprTile()

The `isSprTile(id, tile_ref, invert)` function performs a collision test between the AABB of the specified sprite instance and tile instances in two different ways. When the `invert` parameter is `false`, the function returns `true` if the AABB of the specified sprite instance collides with an instance of the provided tile reference, `false` otherwise. When the `invert` parameter is `true`, the function returns `true` if the AABB of the specified sprite instance collides with a tile instance that is different from the provided tile reference, `false` otherwise. The `id` parameter is of type `byte` with an effective value in the [0,31] range. The `tile_ref` parameter is of type `byte` with an effective value in the [0,255] range. The `invert` parameter is of type `bool`. The test is performed with the currently displayed tile buffer, not the one being drawn (if any).

### sprOther()

The `sprOther(id, start)` function performs a collision test between the AABB of the specified sprite instance and the AABB of all other sprite instances, starting with the identifier at index `start`. If a collision occurs, the identifier of the other sprite is returned, and the search stops. If no collision is found, the value 255 is returned. To detect multiple collisions, the function can be invoked again, starting with the last identifier found+1 as shown in the example below. The `id` parameter is of type `byte` with an effective value in the [0,31] range. The `start` parameter is of type `byte` with an effective value in the [0,31] range.

Example:

```c
byte other;//start search at id 0
while (true)
{
other = sprOther(4,other);
if(other == 255) break;
print(other,"⏎"); //print all sprites that collide with sprite 4
other++;//skip the last found in next search
}
```

## 7.11. Audio

### 7.11.1. YM2612-based Synth

The YM2612-based Synth API functions provide easy access to the sound capabilities of the YM2612 chip. For more information about the YM2612-based Synth, see Section 4.1.

#### ymInit()

The `ymInit(tune)` function initializes the YM2612-based Synth by setting the master tune and generating the internal notes table. The `tune` parameter is of type `int` with an effective value in the [-128,127] range. The default value is 0.

#### ymLFO()

The `ymLFO(enable, frequency)` function allows the LFO to be turned on or off and to set its frequency. The `enable` parameter is of type `bool`. The `frequency` parameter is of type `byte` and has an effective value in the [0,7] range. The corresponding oscillator frequencies are shown in the following table:

| Value | Frequency |
|---|---|
| 0 | 3.82 Hz |
| 1 | 5.33 Hz |
| 2 | 5.77 Hz |
| 3 | 6.11 Hz |
| 4 | 6.60 Hz |
| 5 | 9.23 Hz |
| 6 | 46.11 Hz |
| 7 | 69.22 Hz |

#### ymVol()

The `ymVol(volume)` function allows for setting the global volume. The `volume` parameter is of type `byte` with an effective value range of [0,127].

#### ymVoice()

The `ymVoice(voice, instrument, octave, pan, volume)` function allows for the setup of a hardware voice. All parameters are of type `byte`. The `voice` parameter is the hardware voice index and has a valid range of [0,5]. The `instrument` parameter is the reference to the instrument from the bank and has a valid range of [0,9]. As an example, for playing chords, several voices can reference the same instrument. The `octave` parameter sets the octave at which the instrument will play on this particular voice. The valid range is [0,7]. The `pan` parameter specifies the stereo panning of this voice. Panning is defined as: `0b00`=off, `0b01`=right, `0b10`=left, `0b11`=center. The `volume` parameter specifies the volume for this voice. The valid range is [0,127].

#### ymNoteOn()

The `ymNoteOn(voice, note, velocity [, extra])` function triggers a note on the specified `voice`. All parameters are of type `byte`. The `note` parameter corresponds to the MIDI note[^F1] value. The `velocity` parameter is used to indicate how hard or soft the note is played for expressive control. The valid range is [0,128]. The optional `extra` parameter is for future expansions, such as effects. Please check examples for usage in future OS revisions.

#### ymNoteOff()

The `ymNoteOff(voice)` function turns off the note playing on the specified voice. The `voice` parameter is of type `byte`.

#### ymGetParam()

The `ymGetParam(instrument, parameter)` function returns the value of a particular `parameter` from the specified `instrument`. All parameters are of type `byte`. The `instrument` valid range is [0,9] and the `parameter` valid range is [0,53]. The table of parameters can be found in Appendix B. The returned value is of type `byte`.

#### ymSetParam()

The `ymSetParam(instrument, parameter, value)` function sets the `value` of a particular `parameter` of the specified `instrument`. All parameters are of type `byte`. The `instrument` valid range is [0,9] and the `parameter` valid range is [0,53]. The valid value range depends on the addressed parameter. The table of parameters can be found in Appendix B.

#### ymLoad()

The `ymLoad(filename, instrument)` function loads the entire set of parameters from a file to the specified instrument. The `instrument` parameter is of type `byte` and has a valid range of [0,9].

#### ymSave()

The `ymSave(filename, instrument)` function saves the entire set of parameters from the specified instrument to a file. The `instrument` parameter is of type `byte` and has a valid range of [0,9].

#### ymSetReg()

The `ymSetReg(bank, register, value)` function provides low-level access to the internal registers of the YM2612 chip. To address the lower registers (including the global registers) set the `bank` parameter to 0. To address the upper registers, set the `bank` parameter to 1. All parameters are of type `byte`. For a detailed list of registers and their usage, please refer to the YM2612 register reference page at https://plutiedev.com/ym2612-registers.

### 7.11.2. Built-in Digital Synth

The Built-in Synth API functions provide easy access to the most common features of the built-in synth. When more precise control is required, generic memory functions such as `peek()` and `poke()` can be used to access specific registers. For more information about the Built-in Synth, see Section 4.2.

#### synInit()

The `synInit(tune)` function initializes the built-in Synth by setting the master tune and generating the internal notes table. The `tune` is of type `int` with an effective value in the [-128,127] range. The default value is 0.

#### synEnv()

The `synEnv(id, attack, decay, sustain, release)` function sets up the identified envelope. Please refer to Section 4.2 for more information about envelope usage. All parameters are of type `byte`. The `id` is either 0 or 1 and refers to one of the two available envelopes. The `attack`, `decay`, `sustain`, `release` parameters all have a valid range of [0,15].

#### synLFO()

The `synLFO(id, waveform, frequency, amplitude)` function sets up the identified LFO. Please refer to Section 4.2 for more information about LFO usage. All parameters are of type `byte`. The `id` is either 0 or 1 and refers to one of the two available LFOs. The `wave` parameter has a valid range of [0,3]. The `frequency` and `amplitude` parameters have a valid range of [0,15].

#### synVoice()

The `synVoice(voice, waveform, octave, pan, volume)` function sets up a hardware voice. All parameters are of type `byte`. The `voice` parameter is the hardware voice index and is either 0 or 1. The `waveform` parameter selects the oscillator waveform. It has a valid range of [0,3]. The `octave` parameter sets the octave at which the oscillator will play on this voice when the selected waveform is either square, sawtooth, or noise. When the voice is DPCM, the `octave` parameter has no effect. The `octave` parameter has a valid range of [0,7]. The `pan` parameter specifies the stereo panning of this voice. Panning is defined as: `0b00`=off, `0b01`=right, `0b10`=left, `0b11`=center. The `volume` parameter specifies the volume for this voice. Valid range is [0,15].

When the selected waveform is either square, sawtooth, or noise, the `synVoice()` function will automatically connect the corresponding envelope to the amplifier (asserting bits [7:6] of register `0x1115`), allowing minimal sound setup, as in the following example:

```c
//simple sound example with the built-in mini synth
synInit(0);//Init tune and build internal notes table
synEnv(0,0,10,0,0);//Fast attack and small decay for a short bell-like sound
synVoice(0,0,5,0b11,15);//square wave, octave 5, centered, max volume
synNoteOn(0,11,128);//Play A 440Hz
```

#### synNoteOn()

The `synNoteOn(voice, note, velocity [, extra])` function triggers a note on the specified `voice`. All parameters are of type `byte`. In normal mode, the `note` parameter corresponds to the MIDI note[^F2] value. The `velocity` parameter is used to indicate how hard or soft the note is played for expressive control. The valid range is [0,128]. The optional `extra` parameter is for future expansions, such as effects. Please check examples for usage in future OS revisions.

If the voice has been set up to produce a DPCM waveform, the `note` parameter refers to the percussion instrument index as defined in Table 4.1. The playback frequency is automatically selected to be 16 kHz for kick instruments and 32 kHz for others.

When a custom playback frequency is required, the specific registers have to be set by hand. See Section 4.2 for more information.

#### synNoteOff()

The `synNoteOff(voice)` function turns off the note for the specified voice. The `voice` parameter is the hardware voice index. It is of type `byte` and is either 0 or 1.

#### synFilter()

The `synFilter(active, mode, cutoff_control)` function allows for a quick setup of the multi-mode SVF filter. The `active` parameter is of type `bool` and turns the filter on or off. The two other parameters are of type `byte` and have a valid range of [0,3]. Please refer to Section 4.2 for more information about the filter.

Example:

```c
//wind sound example using the SVF
synInit(0);//Init tune and build internal notes table
synEnv(0,15,15,15,15);//slow attack and release
synLFO(0,3,0,7);//slow sine wave
synFilter(true,1,3);//filter in hi-pass mode. LFO0 controls cutoff
poke(0x111e,128);//filter resonance set to 50%
synVoice(0,2,5,0b11,15);//noise, octave 5, centered, max volume
synNoteOn(0,11,128);//play sound
nat start=_time;
while((_time-start)<6){}//wait a few seconds
synNoteOff(0);
```

## 7.12. ADC

### adcSampleL()

The `adcSampleL()` function initiates sampling on the left channel of the Analog to Digital Converter (ADC).

### adcSampleR()

The `adcSampleR()` function initiates sampling on the right channel of the Analog to Digital Converter (ADC).

### adcL

The `adcL` built-in variable provides the result from the last sampling from the left channel of the ADC. The value is of type `int` with a range of [-512,512].

### adcR

The `adcR` built-in variable provides the result from the last sampling from the right channel of the ADC. The value is of type `int` with a range of [-512,512].

[^F1]: https://www.phys.unsw.edu.au/jw/notes.html
[^F2]: https://www.phys.unsw.edu.au/jw/notes.html

# 8. Built-in tools

## 8.1. Text Editor

*Figure 8.1: The built-in Text Editor*

The built-in Text Editor allows editing text files and source code files in particular. During editing, the edited file resides on the lowest part of the XSRAM (starting at physical address `0x00000`).

The Text Editor makes use of memory paging and the source code file size is not limited by the CPU 64KiB addressing space.

### 8.1.1. Launching the Text Editor

From the CLI, enter the `edit` file name command to edit a text file. If the file name does not exist, a new file will be created. Any relevant file extension must be provided.

Examples:

To create and edit a new (if it does not exist yet) source file called `myfile`, type:

```text
edit myfile.src
```

To create and edit a new (if it does not exist yet) generic text file called `readme`, type:

```text
edit readme.txt
```

### 8.1.2. Using the Text Editor

#### Files

- `FCTN` + `S` to save the current file.
- `FCTN` + `N` to give a new name to the current file. The extension (e.g. `.src` or `.txt`) must also be provided.

#### Exiting the Text Editor

- `FCTN` + `ESC` to exit the editor.

If the file has been modified (the diskette symbol is present in the bottom bar), the program asks for confirmation before exiting.

#### Display modes

Using the `ALT` + `TAB` keys allows toggling the display of the End Of Line (EOL) (sometimes called Line Feed or New Line) and End Of File (EOF) symbols.

#### Cursor movement

- `↑`, `↓`, `←`, `→` to move the cursor around.
- `CTRL` + `→` to move the cursor to the end of a line.
- `CTRL` + `←` to move the cursor to the start of a line.
- `CTRL` + `↑` to move the cursor to the top line of the display. Pressing the same combination again will move the cursor to the top of the file.
- `CTRL` + `↓` to move the cursor to the bottom line of the display. Pressing the same combination again will move the cursor to the end of the file.
- `ALT` + `↑` to move the cursor up one 'bloc' (24 lines).
- `ALT` + `↓` to move the cursor down one 'bloc' (24 lines).
- `ALT` + `→` to move the cursor to the end of the next 'token'.
- `ALT` + `←` to move the cursor to the end of the previous 'token'.

#### Bookmarks

Bookmarks allow you to jump to specified lines in a text or source file.

Up to 10 bookmarks can be created. Bookmarks are saved in EEPROM and are persistent, even after a power cycle. The number column for a line with a set bookmark is shown in a light gray color.

- `FCTN` + `RSHIFT` + `1` ... `0` to create or remove a bookmark on the current line.
- `FCTN` + `1` ... `0` to jump to the corresponding bookmark.
- `FCTN` + `RSHIFT` + `-` to remove all bookmarks.

Bookmarks are totally file-agnostic. You can, for instance, reserve the first four bookmarks for a given file and the rest for another, or any combination that fits your needs.

#### Selection

Holding `SHIFT` while moving the cursor allows for making a selection (in the right and bottom directions only). Move the cursor backward to deselect.

#### Mouse support

The mouse can be used in the Text Editor to help with navigation and selection.

- Left-click to directly move the cursor to a given position.
- Left-click and hold to create a text selection (only in the right and bottom directions).

#### Text manipulation

- `FCTN` + `I` to clip (cut) current selection. The selection goes to the clipboard.
- `FCTN` + `O` to copy current selection to the clipboard.
- `FCTN` + `P` to paste clipboard to the current cursor position.

Cutting or copying a selection moves it to a persistent clipboard file in the system folder. The persistent clipboard allows one to copy content from one file to another.

#### Compiling from the Text Editor

- `FCTN` + `C` to compile the current file. If errors are detected during compilation, they are reported in the editor. After compilation, the Symbols, Code, and Data space utilization is displayed in the information bar (see Section 8.2 for more information).
- `FCTN` + `R` to compile the current file, exit the editor, and run the compiled bytecode. If errors are detected during compilation, they are reported in the editor, and the process is stopped before execution.

> **Note:** Invoking compilation from the editor automatically saves the file to disk prior to compilation.

## 8.2. Compiler

The built-in compiler allows for the generation of executable (`.bin`) files from Lofi source code (`.src`) files.

The source code can be compiled from the Text Editor (see Section 8.1) or using the following command from the CLI interface:

```text
compile filename
```

Any errors will be reported during the compilation process. When the compilation process ends, some information is displayed:

- **Symb: xx%** The percentage of symbol memory used during compilation. The symbol memory stores the identifier names used in the source code along with useful information for the compiler (such as type). Using shorter names can reduce memory usage.
- **Code: xx%** The percentage of code memory used by the executable with respect to the currently allocated code memory (see Section 2.2.2 for more information on memory configuration).
- **Data: xx%** The percentage of data memory used by the executable with respect to the currently allocated data memory (see Section 2.2.2 for more information on memory configuration).

More information about the compiler and the Lofi language can be found in Chapter 6.

## 8.3. Image Editor

*Figure 8.2: ImagEd: The built-in Image Editor*

The Image Editor enables the creation of static images (bitmap graphics).

When saving an image, the bitmap data is saved with the `.bmp` extension, and the palette is saved with the `.pal` extension. It is trivial to use an image in a Lofi application using the following code (with the default memory configuration):

```c
import("myimage.pal", 0x11a0, 48); //import palette
import("myimage.bmp", 0x3000, 48000, 0x60); //import bitmap
```

See Section 3.5 for more information about bitmap graphics.

### 8.3.1. Launching the Image Editor

The Image Editor is a Lofi application called `IMAGED`, located in the `/TOOLS/IMAGED` folder.

From the CLI, enter the `cd /tools/imaged` command to go to the `/TOOLS/IMAGED` folder. Then, type `dir` and hit `ENTER` to see the contents of the folder.

Both the source code and the compiled versions of the tool are available. To launch the tool, type `run imaged.bin` and hit `ENTER`.

### 8.3.2. Exiting the Image Editor

- `FCTN` + `ESC` to exit the editor.

### 8.3.3. Using the Image Editor

#### Mouse usage

Use the left mouse button to draw or select the drawing color in the color box window.

Use the right mouse button to erase or select the erase color in the color box window.

Left-click on the arrows of the color editor to edit the RGB values of the last selected color.

#### Keyboard shortcuts

- `[` and `]` to decrease/increase brush size.
- `,` and `.` to pick the draw/erase color from the image.
- `M` to change the mouse sensitivity.
- `FCTN` + `⇤` to clear the current image.
- `FCTN` + `N` to name the current image. If no name is given, the name `default` will be assigned. In that case, make sure to rename the image in the CLI afterwards to avoid it being overwritten by a new one.
- `FCTN` + `F` to change the current folder (default is `./images`)
- `FCTN` + `S` to save the current image.
- `FCTN` + `L` to load an image.
- `TAB` to toggle the GUI.
- `Z` to toggle the zoom window.
- `-` and `=` to decrease/increase the zoom factor.
- `1` `2` `3` to change the brush shape.
- `G` to toggle the grid.

For updated information about this tool, please check www.micro-8.com.

## 8.4. Sprite Editor

*Figure 8.3: SpritEd: The built-in Sprite Editor*

The Sprite Editor enables the creation of sprite sets containing both static sprite images and animated sprite sequences. Animation can be previewed in the editor, and both a looping mode and a ping-pong mode are available.

Up to four sprites can be edited simultaneously to create larger images or animations. Two palettes can be edited as well as the AABB of each image.

When saving a sprite set, the sprite image data is saved with the `.spr` extension, and palettes are saved with the `.pla` and `.plb` extensions, respectively.

### 8.4.1. Launching the Sprite Editor

The Sprite Editor is a Lofi application called `SPRITED`, located in the `/TOOLS/SPRITED` folder.

From the CLI, enter the `cd /tools/sprited` command to go to the `/TOOLS/SPRITED` folder. Then, type `dir` and hit `ENTER` to see the contents of the folder.

Both the source code and the compiled versions of the tool are available. To launch the tool, type `run sprited.bin` and hit `ENTER`.

### 8.4.2. Exiting the Sprite Editor

- `FCTN` + `ESC` to exit the editor.

### 8.4.3. Using the Sprite Editor

#### Mouse usage

Use the left mouse button to draw, select tools or actions, or pick the draw color in the color box window.

Use the right mouse button to erase or select the erase color in the color box window.

Left-click on the arrows of the color editor to edit the RGB values of the last selected color.

#### Keyboard shortcuts

- `→` or `↓` to select the next sprite (or group of sprites if more than one sprite is currently being edited).
- `←` or `↑` to select the previous sprite (or group of sprites, if more than one sprite is currently being edited).
- `[` and `]` to decrease/increase brush size.
- `,` and `.` to select the draw/erase color in the image.
- `M` to change the mouse sensitivity.
- `FCTN` + `U` to undo the last change(s)
- `FCTN` + `D` to redo previous change(s)
- `FCTN` + `N` to name the current image. If no name is given, the name `default` will be assigned. In that case, make sure to rename the image in the CLI afterward to avoid it being overridden by a new one.
- `FCTN` + `F` to change the current folder (default is `/sprites`)
- `FCTN` + `S` to save the current sprite set.
- `FCTN` + `L` to load a set of sprites.
- `1` `2` `3` to change the brush shape.

For updated information about this tool, please check www.micro-8.com.

## 8.5. Tile Map Editor

The Tilemap Editor (developed by David Gervais) allows you to create tilemaps quickly and easily. The minimum editable map size is 25×15 tiles, with no fixed maximum apart from the memory limit of 15 KiB. To calculate the memory usage, simply multiply the map's width by its height, since each tile represents one byte.

*Figure 8.4: MapEd: The built-in Tilemap Editor*

### 8.5.1. Launching the Tilemap Editor

The Tilemap Editor is a Lofi application called `MAPED`, located in the `/TOOLS/MAPED` folder.

From the CLI, enter the `cd /tools/maped` command to go to the `/TOOLS/MAPED` folder. Then, type `dir` and hit `ENTER` to see the contents of the folder.

Both the source code and the compiled versions of the tool are available. To launch the tool, type `run maped.bin` and hit `ENTER`.

When the program launches, the screen displays the tileset view along with the options menus.

Use the middle mouse button to swap between the tileset view (allowing tile selection) and the map view (allowing map editing).

In the tileset view, you can select one or more tiles by click-dragging the left mouse button. Use the right mouse button to choose which tile will act as the eraser tile. It will appear in the bottom corner of the screen.

### 8.5.2. Menu options

- **FILES** (always specify names without extension)
  - **NEW**
    Creates a new tilemap with the desired dimensions.
  - **LOAD TILEMAP**
    Loads an existing tilemap (`.LVL` extension). The encoding is as follows:
    - bytes [0,1] : map width
    - bytes [2,3] : map height
    - bytes [4,...end] : map data
  - **SAVE TILEMAP**
    Saves the current tilemap.
  - **LOAD LAYER**
    Loads a layers bitmask (`.LAY` extension). Each byte of the mask encodes up to eight layers, with one bit per layer. For example, if the byte at position (0,0) has the value `0b10000010`, this means that position (0,0) is active on layers 7 and 1. You are free to use these eight layers however you wish - for example, as path maps, collision maps, or any other custom layer types. The encoding is as follows:
    - bytes [0,1] : layers map width
    - bytes [2,3] : layers map height
    - bytes [4,...end] : layers bitmask data
  - **SAVE LAYER**
    Saves the layers mask data.
  - **IMPORT TILESET**
    Loads a tileset into the editor (`.TLS` extension). The encoding is as follows:
    - bytes [0,47] : RGB palette
    - bytes [48,32816] : 32 KiB tileset data
  - **EXIT**
    Quits the program.
- **EDIT**
  - **MODE DRAW**
    Default drawing mode. Copies the current selection onto the tilemap. The selection can be made from either the tileset view or the tilemap view itself.
  - **MODE RANDOM**
    In this mode, when drawing on the tilemap, the editor randomly selects a tile from the current selection. The selection can be made from either the tileset view or the tilemap view itself.
  - **MODE LAYER**
    In this mode, you can edit the currently selected layer, ranging from 0 to 7.
  - **GENERATE PATH LAYER**
    Transform the active tiles into a path on the active layer.
  - **INVERT LAYER**
    Invert the active layer state.
  - **CLEAR LAYER**
    Clear the selected layer.
  - **RESIZE TILEMAP**
    Resizes the tilemap while preserving its contents.

    > **Warning:** Smaller map dimensions will crop the current map, and data outside the boundaries will be lost.
- **PARAMETERS**
  - **SET SELECTED COLOR**
    Changes the color of the tile selection border as well as the minimap highlight. Enter the three values (from 0 to 15) for each RGB component.
  - **ERASER ICON**
    Toggles the visibility of the eraser tile for better clarity.
  - **MINI MAP**
    Toggles the visibility of the minimap. The minimap shows the current working area within the overall tilemap.

#### Keyboard shortcuts

- `FCTN` + `U` to undo the last change in the tilemap view.
- `FCTN` + `O` to select an area to copy in the tilemap view. The selected area becomes the new active tile selection, which can then be copied in DRAW mode or used with RANDOM mode.
- `1` ... `4` to adjust the scrolling speed / block movement speed.
- `↑`, `↓`, `←`, `→` to scroll manually, one tile at a time.
- `FCTN` + `↑`, `↓`, `←`, `→` to scroll manually, one page at a time.
- `FCTN` + `1` ... `8` to select the working layer in LAYER mode.
- `FCTN` + `⇤` to clear the current screen (in TILES or LAYER mode).
- `FCTN` + `V` to revert the layer state of the current screen.
- `FCTN` + `G` to generate a mask for the active layer based on the active tiles on the current screen.

For updated information about this tool, please check www.micro-8.com.

## 8.6. Sound Editor

The Sound Editor allows for editing the sound parameters of the YM2612-based synthesizer and the configuration of the built-in MAGIC-based synthesizer.

On the YM2612-based synthesizer side, up to ten instruments can be loaded simultaneously.

YM2612-based instrument files are saved with the `.ins` extension, and the built-in MAGIC-based synthesizer configuration files are saved with the `.syn` extension.

*Figure 8.5: SoundEd: The built-in Sound Editor*

### 8.6.1. Launching the Sound Editor

The Sound Editor is a Lofi application called `SOUNDED`, located in the `/TOOLS/SOUNDED` folder.

From the CLI, enter the `cd /tools/sounded` command to go to the `/TOOLS/SOUNDED` folder. Then, type `dir` and hit `ENTER` to see the contents of the folder.

Both the source code and the compiled versions of the tool are available. To launch the tool, type `run sounded.bin` and hit `ENTER`.

### 8.6.2. Exiting the Sound Editor

- `FCTN` + `ESC` to exit the editor.

### 8.6.3. Using the Sound Editor

#### Mouse usage

Use the left mouse button to select instruments or modify values.

#### Keyboard shortcuts

- `FCTN` + `N` to name the current instrument.
- `FCTN` + `F` to change the current folder (default is `/sounds`)
- `FCTN` + `S` to save the current instrument.
- `FCTN` + `L` to load an instrument.

For updated information about this tool, please check www.micro-8.com.

## 8.7. Music Editor

Available soon. Please check www.micro-8.com for availability.

# 9. Special I/O

## 9.1. User port

The User Port (also labeled JTAG MCU) provides 8 lines of digital Input/Output (I/O) along with power lines. It allows one to connect custom hardware to Micro-8.

### 9.1.1. Pinout

*Figure 9.1: User Port pinout (rear view)*

| Pin # | Role | Pin # | Role |
|---|---|---|---|
| 1 | 3.3 V VCC (max 150 mA) | 6 | I/O Bit 4 |
| 2 | I/O Bit 0 | 7 | I/O Bit 5 |
| 3 | I/O Bit 1 | 8 | I/O Bit 6 |
| 4 | I/O Bit 2 | 9 | I/O Bit 7 |
| 5 | I/O Bit 3 | 10 | Ground |

> **Warning:** The User Port is directly connected to the MCU I/O pins and Micro-8 may be damaged if a voltage above 3.3 V is applied to the pins. Use voltage dividers or level shifters to interface with 5 V devices.

See API in Section 7.2 for more information on how to access the user port in Lofi.

## 9.2. MIDI ports

The Musical Instrument Digital Interface (MIDI) ports allow one to connect digital musical instrument equipment to Micro-8 and turn it into either a sound module (MIDI-IN), or a sequencer to drive external sound sources (MIDI-OUT).

Some of the examples included with Micro-8 have out-of-the-box minimal MIDI support, such as `/demos/organ.src` and the MusicEd tool.

See API in Section 7.2 for more information on how to use MIDI functions in Lofi.

## 9.3. Hardware programming ports

### 9.3.1. JTAG MCU

This port (also labeled User Port) allows configuring the MCU with a hardware programming tool such as Atmel ICE.

> **Warning:** Flashing the hardware with your own kernel will destroy Micro-8's operating system and void the warranty. To update the OS please use the `flash` command as explained in Chapter 2.

### 9.3.2. JTAG FPGA

This port allows configuring the FPGA with a hardware programming tool such as USB Blaster and Altera's Quartus Prime tool.

> **Warning:** Flashing the FPGA with your own core will destroy Micro-8's MAGIC and void the warranty.

### 9.3.3. ISP

Disabled. Might be removed in future revisions. To update the OS please use the `flash` command as explained in Chapter 2.

# Appendix A. Hardware

## A.1. Hardware bloc diagram

```text
                      256 KiB SRAM
                           | A[17:0], D[7:0]
Digital Video <-- RGB Stream --+
                               |
Analog Video <-- RGB DAC <-- RGB[2:0] -- Altera Max10 M08 FPGA <-- PS/2 Mouse
                                          |   |    |              <-- Game controller (x2)
JTAG ------------------------------------ +   |    +-- PWM Audio --> Audio Mixer
                                    CTRL, IRQ, A/D[7:0], A[15:8]
                                              |
User GPIO <-- I/O[7:0] --> Atmel ATmega 128A 8-bit MCU <-- ADC in -- Audio Mixer
                              |    |     |  I2C
Micro SD Flash Drive <--------+    |     +--> TCA8418 <-- K[17:0] -- Custom Keyboard
ISP/JTAG ---------------------------+
                          CTRL |          | D[7:0]
                     TXB0104 VLT      TXB0108 VLT          (5V Domain)
                               |          |   \
                        Yamaha YM2612 FM Synthesis   MIDI IN/OUT
                               |
                         Audio Mixer --> Analog Audio
```

*Figure A.1: Hardware bloc diagram (MCU and FPGA share the CTRL/IRQ and multiplexed address/data bus; the YM2612 and MIDI sit in the 5 V domain behind TXB level translators)*

## A.2. Input/Output Ports

Rear panel, from top to bottom:

- Reset
- MIDI In
- MIDI Out
- Audio Out (Line)
- Micro-SD
- ISP (MCU)
- User Port
- JTAG (FPGA)
- PS/2 Mouse
- Analog Video (VGA)
- Digital Video
- Power Plug (DC 7-9 V)
- Power Switch
- Game Controllers

# Appendix B. Codes and Tables

## B.1. Glyph codes (hexadecimal)

The glyph set is a 16×16 grid: the row gives the high hex digit, the column the low hex digit (e.g. `0x41` = `A`). The glyphs themselves are pixel art and are only shown as an image in the original PDF; the printable ASCII range is transcribed below.

| | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | A | B | C | D | E | F |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **20** | space | `!` | `"` | `#` | `$` | `%` | `&` | `'` | `(` | `)` | `*` | `+` | `,` | `-` | `.` | `/` |
| **30** | `0` | `1` | `2` | `3` | `4` | `5` | `6` | `7` | `8` | `9` | `:` | `;` | `<` | `=` | `>` | `?` |
| **40** | `@` | `A` | `B` | `C` | `D` | `E` | `F` | `G` | `H` | `I` | `J` | `K` | `L` | `M` | `N` | `O` |
| **50** | `P` | `Q` | `R` | `S` | `T` | `U` | `V` | `W` | `X` | `Y` | `Z` | `[` | `\` | `]` | `^` | `_` |
| **60** | `` ` `` | `a` | `b` | `c` | `d` | `e` | `f` | `g` | `h` | `i` | `j` | `k` | `l` | `m` | `n` | `o` |
| **70** | `p` | `q` | `r` | `s` | `t` | `u` | `v` | `w` | `x` | `y` | `z` | `{` | `\|` | `}` | `~` | block |

- `0x00`-`0x1F`: control and UI glyphs (tabulation, play/stop-style symbols, new line, the two diagonals `0x0E`/`0x0F` used by the 10 PRINT maze example, the foreground/background/inverse color symbols, arrows, `0x`/`0b` prefixes, glyph `?` symbol, tool icons).
- `0x80`-`0xFF`: original graphics symbols (box drawing, blocks, circles, dither patterns, icons such as the diskette at `0xC4`, and the Micro-8 logo in the lower right of the grid).

## B.2. Key codes (decimal)

Key codes follow the keyboard matrix of Appendix B.3: code = row × 10 + column + 1 (e.g. `ESC` is row R1, column C1, code 12).

| Key | Code | Key | Code | Key | Code | Key | Code |
|---|---|---|---|---|---|---|---|
| `1` | 1 | `CTRL` | 11 | `2` | 21 | `TAB` | 31 |
| `3` | 2 | `ESC` | 12 | `4` | 22 | `S` | 32 |
| `5` | 3 | `A` | 13 | `6` | 23 | `F` | 33 |
| `7` | 4 | `D` | 14 | `8` | 24 | `H` | 34 |
| `9` | 5 | `G` | 15 | `0` | 25 | `K` | 35 |
| `-` | 6 | `J` | 16 | `=` | 26 | `;` | 36 |
| `⇤` (Backspace) | 7 | `L` | 17 | `'` | 27 | `ENTER` | 37 |
| `Q` | 41 | `LSHIFT` | 51 | `` ` `` | 61 | `ALT` | 71 |
| `E` | 42 | `X` | 52 | `W` | 62 | `Z` | 72 |
| `T` | 43 | `V` | 53 | `R` | 63 | `C` | 73 |
| `U` | 44 | `N` | 54 | `Y` | 64 | `B` | 74 |
| `O` | 45 | `,` | 55 | `I` | 65 | `M` | 75 |
| `[` | 46 | `/` | 56 | `P` | 66 | `.` | 76 |
| `\` | 47 | `←` | 57 | `]` | 67 | `↑` | 77 |
| | | `RSHIFT` | 58 | | | `SPACE` | 78 |
| | | `→` | 59 | | | `FCTN` | 79 |
| | | | | | | `↓` | 80 |

## B.3. Keyboard matrix arrangement

| | C0 | C1 | C2 | C3 | C4 | C5 | C6 | C7 | C8 | C9 |
|---|---|---|---|---|---|---|---|---|---|---|
| **R0** | `1` | `3` | `5` | `7` | `9` | `-` | `⇤` | | | |
| **R1** | `CTRL` | `ESC` | `A` | `D` | `G` | `J` | `L` | | | |
| **R2** | `2` | `4` | `6` | `8` | `0` | `=` | `'` | | | |
| **R3** | `TAB` | `S` | `F` | `H` | `K` | `;` | `ENTER` | | | |
| **R4** | `Q` | `E` | `T` | `U` | `O` | `[` | `\` | | | |
| **R5** | `LSHFT` | `X` | `V` | `N` | `,` | `/` | `←` | `RSHFT` | `→` | |
| **R6** | `` ` `` | `W` | `R` | `Y` | `I` | `P` | `]` | | | |
| **R7** | `ALT` | `Z` | `C` | `B` | `M` | `.` | `↑` | `SPACE` | `FCTN` | `↓` |

## B.4. YM2612 Synth Parameters

| Param | Value range | Function |
|---|---|---|
| 0 | [0,7] | OP1 Detune 0, +, ++, +++, 0, -, --, --- |
| 1 | [0,15] | OP1 Multiply base freq: 1/2, 1, 2, 3, ..., 15 |
| 2 | [0,127] | OP1 Envelope Total Level |
| 3 | [0,3] | OP1 Envelope Rate Scaling |
| 4 | [0,31] | OP1 Envelope Attack Rate |
| 5 | [0,1] | OP1 Envelope Amplitude modulation |
| 6 | [0,31] | OP1 Envelope Decay Rate |
| 7 | [0,31] | OP1 Envelope Sustain Rate |
| 8 | [0,15] | OP1 Envelope Sustain Level |
| 9 | [0,15] | OP1 Envelope Release Rate |
| 10 | [0,7] | OP2 Detune 0, +, ++, +++, 0, -, --, --- |
| 11 | [0,15] | OP2 Multiply base freq: 1/2, 1, 2, 3, ..., 15 |
| 12 | [0,127] | OP2 Envelope Total Level |
| 13 | [0,3] | OP2 Envelope Rate Scaling |
| 14 | [0,31] | OP2 Envelope Attack Rate |
| 15 | [0,1] | OP2 Envelope Amplitude modulation |
| 16 | [0,31] | OP2 Envelope Decay Rate |
| 17 | [0,31] | OP2 Envelope Sustain Rate |
| 18 | [0,15] | OP2 Envelope Sustain Level |
| 19 | [0,15] | OP2 Envelope Release Rate |
| 20 | [0,7] | OP3 Detune 0, +, ++, +++, 0, -, --, --- |
| 21 | [0,15] | OP3 Multiply base freq: 1/2, 1, 2, 3, ..., 15 |
| 22 | [0,127] | OP3 Envelope Total Level |
| 23 | [0,3] | OP3 Envelope Rate Scaling |
| 24 | [0,31] | OP3 Envelope Attack Rate |
| 25 | [0,1] | OP3 Envelope Amplitude modulation |
| 26 | [0,31] | OP3 Envelope Decay Rate |
| 27 | [0,31] | OP3 Envelope Sustain Rate |
| 28 | [0,15] | OP3 Envelope Sustain Level |
| 29 | [0,15] | OP3 Envelope Release Rate |
| 30 | [0,7] | OP4 Detune 0, +, ++, +++, 0, -, --, --- |
| 31 | [0,15] | OP4 Multiply base freq: 1/2, 1, 2, 3, ..., 15 |
| 32 | [0,127] | OP4 Envelope Total Level |
| 33 | [0,3] | OP4 Envelope Rate Scaling |
| 34 | [0,31] | OP4 Envelope Attack Rate |
| 35 | [0,1] | OP4 Envelope Amplitude modulation |
| 36 | [0,31] | OP4 Envelope Decay Rate |
| 37 | [0,31] | OP4 Envelope Sustain Rate |
| 38 | [0,15] | OP4 Envelope Sustain Level |
| 39 | [0,15] | OP4 Envelope Release Rate |
| 40 | [0,7] | Feedback amount |
| 41 | [0,7] | Algorithm |
| 42 | [0,3] | LFO Amplitude modulation amount |
| 43 | [0,7] | LFO Frequency modulation amount |
| 44 | [0,15] | Operators On/Off (4 bits) |
| 45 | [0,1] | Special mode |
| 46 | [0,255] | Supplementary Freq 1 MSB |
| 47 | [0,255] | Supplementary Freq 1 LSB |
| 48 | [0,255] | Supplementary Freq 2 MSB |
| 49 | [0,255] | Supplementary Freq 2 LSB |
| 50 | [0,255] | Supplementary Freq 3 MSB |
| 51 | [0,255] | Supplementary Freq 3 LSB |
| 52 | [0,255] | Supplementary Freq 4 MSB |
| 53 | [0,255] | Supplementary Freq 4 LSB |

## B.5. File system error codes

| Code | Error |
|---|---|
| 0 | No error, the operation succeeded |
| 1 | Disk error (hardware) |
| 2 | Volume error (e.g. FAT structure error) |
| 3 | Device not ready |
| 4 | File not found |
| 5 | Path/Folder not found |
| 6 | Invalid name |
| 7 | Access denied |
| 8 | File exists |
| 9 | Invalid file |
| 10 | Write protected |
| 11 | Invalid drive |
| 12 | Volume not mounted |
| 13 | Invalid volume |
| 14 | File system creation aborted |
| 15 | Timeout |
| 16 | Operation rejected |
| 17 | Out of memory |
| 18 | Too many opened files |
| 19 | Invalid parameter |

## B.6. More on CLI error messages

- **File error!**

  The file name argument is missing or the file does not exist. Check that the spelling and path are correct.

  Alternatively, a read/write error might have occurred (e.g., bad media). The exact error message can be inferred from the `_fError` built-in variable.

- **Not an executable file!**

  A `load` or `run` command has been invoked with a non-valid executable (`.bin`) file.

- **Out of memory. Program too large.**

  The bytecode memory segment (compiled bytecode) is full. To increase the bytecode memory segment, move the data memory segment origin up using the `data` command and recompile the source code. For example, moving the data segment origin from `0xB000` (default) to `0xC000` will provide 4 KiB of additional code memory (while reducing the data segment by the same amount). More information on the `data` command can be found in Section 2.

- **Out of memory. Too much data.**

  The data memory segment (globals) is full. To increase the data memory segment, move the origin of the data memory segment down using the `data` command and recompile the source code. For example, moving the data segment origin from `0xB000` (default) to `0xA000` will provide 4 KiB of additional data memory (while reducing the code segment by the same amount). More information on the `data` command can be found in Section 2.

- **Symbols buffer full.**

  The compiler buffer used to store symbols information (name, type, address, etc.) is full. Use shorter identifiers to reduce the footprint of symbols.

- **Stack overflow.**

  The stack that holds temporaries and local variables has reached its limits. Try reducing the number of local variables used simultaneously and/or the number of nested scopes.

- **Runtime error.**

  The virtual machine has encountered an illegal opcode. The opcodes may have changed due to a system update. Re-compile the source file and try again. Alternatively, the code memory might have been corrupted with unsuitable data during execution. Please check the source code for potential erroneous memory writes to the code segment.

# Appendix C. Reserved Words

## C.1. Language Keywords

| | | |
|---|---|---|
| `void` | `bool` | `byte` |
| `nat` | `int` | `real` |
| `text` | `if` | `else` |
| `while` | `for` | `break` |
| `continue` | `true` | `false` |
| `return` | `type` | |

## C.2. API

### C.2.1. IO and System

| | | |
|---|---|---|
| `print()` | `input()` | `concat()` |
| `txtWin()` | `curPos()` | `clkSet()` |
| `mouseSens()` | `mouseUpdt()` | `ptrShape()` |
| `isPtrBox()` | `isPtrGlyph()` | `isPtrSpr()` |
| `isPtrChar()` | `isPtrTile()` | `midiUpdt()` |
| `midiSend()` | `getKeyDwn()` | `getKeyUp()` |
| `led()` | `portDir()` | `portSet()` |
| `_port` | `_ver` | `_time` |
| `_mSec` | `_curX` | `_curY` |
| `_keyDwn` | `_keyUp` | `_mouse` |
| `_mouseX` | `_mouseY` | `_mouseDX` |
| `_mouseDY` | `_padLft` | `_padRght` |

### C.2.2. Memory

| | | |
|---|---|---|
| `peek()` | `poke()` | `peekW()` |
| `pokeW()` | `memSet()` | `memCpy()` |
| `areaSet()` | `areaCpy()` | |

### C.2.3. File system

| | | |
|---|---|---|
| `import()` | `export()` | `import2D()` |
| `export2D()` | `fCD()` | `fMkDir()` |
| `fDel()` | `fMov()` | `fCpy()` |
| `fOpDir()` | `fRdDir()` | `fOpen()` |
| `fClose()` | `fSize()` | `fRead()` |
| `fWrite()` | `_fSize` | `_fError` |

### C.2.4. Math

| | | |
|---|---|---|
| `rndSeed()` | `rnd()` | `abs()` |
| `min()` | `max()` | `clamp()` |
| `lerp()` | `sqrt()` | `pow()` |
| `sin()` | `cos()` | `atan2()` |

### C.2.5. Video

| | | |
|---|---|---|
| `vSync()` | `vCount()` | `rstrInt()` |
| `rstrCmp()` | | |

### C.2.6. Graphics

#### Glyphs

| | | |
|---|---|---|
| `glyphSet()` | | |

#### Bitmap

| | | |
|---|---|---|
| `bmpWin()` | `bmpOrg()` | `plot()` |
| `skip()` | `draw()` | `line()` |
| `circle()` | `disk()` | `rect()` |
| `box()` | `tri()` | `blit()` |
| `pick()` | `bmpToTile()` | `bmpToSpr()` |

#### Tiles

| | | |
|---|---|---|
| `tileGetRef()` | `tilePanUp()` | `tilePanDwn()` |
| `tilePanLft()` | `tilePanRght()` | |

#### Sprites

| | | |
|---|---|---|
| `sprPal()` | `sprPrio()` | `sprRef()` |
| `sprPos()` | `sprUpdt()` | `isSprSpr()` |
| `isSprBox()` | `isSprPoint()` | `isSprLine()` |
| `isSprTile()` | `sprOther()` | |

### C.2.7. Audio

#### YM2612

| | | |
|---|---|---|
| `ymInit()` | `ymLFO()` | `ymVol()` |
| `ymVoice()` | `ymNoteOn()` | `ymNoteOff()` |
| `ymGetParam()` | `ymSetParam()` | `ymLoad()` |
| `ymSave()` | `ymSetReg()` | |

#### Built-in Synth

| | | |
|---|---|---|
| `synInit()` | `synEnv()` | `synLFO()` |
| `synVoice()` | `synNoteOn()` | `synNoteOff()` |
| `synFilter()` | | |

#### ADC

| | | |
|---|---|---|
| `adcSampleL()` | `adcSampleR()` | `adcL()` |
| `adcR()` | | |

# Appendix D. SD Card

## D.1. Default directory structure

```text
MICRO-8
├── SYSTEM
│   ├── UPDATES          (OS updates files)
│   ├── SYSTEM.100
│   └── ...              (other temporary system files)
├── TOOLS                (built-in tools written in Lofi)
│   ├── IMAGED
│   ├── SPRITED
│   ├── TILED
│   ├── SOUNDED
│   └── MUSICED
├── (contents folders)
├── UTILS                (small utilities written in Lofi)
├── EXAMPLES             (small Lofi examples)
│   ├── AUDIO
│   ├── CODING
│   ├── GRAPHICS
│   └── IO
├── DEMOS                (larger Lofi examples)
└── GAMES
```

# Appendix E. Memory Maps

## E.1. CPU Memory map

The CPU sees a 16-bit address space (`0x0000` to `0xFFFF`) built from three physical memories. The range `0x3000` to `0xEFFF` is a programmable CPU window: its base address in the External SRAM (XSRAM) is programmable, which allows full XSRAM range access (`0x00000` to `0x3FFFF`). The range `0xF000` to `0xFFFF` is a fixed window on the top 4 KiB of the XSRAM (`0x3F000` to `0x3FFFF`).

| CPU address | Content | Physical memory |
|---|---|---|
| `0x0000` - `0x10FF` | System globals / stack / heap | Internal SRAM |
| `0x1100` - `0x2FFF` | Main system registers / buffers | FPGA SRAM |
| `0x3000` - Data Origin | Code (VM Bytecode & constants), executables are loaded here (`load` of `.bin` files) | External SRAM (XSRAM), programmable CPU window |
| Data Origin (user select, default `0xB000`) - `0xBFFF` | Data (VM Globals) | External SRAM (XSRAM), programmable CPU window |
| `0xC000` - `0xEFFF` | Compiler buffers | External SRAM (XSRAM), programmable CPU window |
| `0xF000` - `0xF7FF` | Scratchpad | Fixed XSRAM (`0x3F000` - `0x3F7FF`) |
| `0xF800` - `0xFFFF` | System data | Fixed XSRAM (`0x3F800` - `0x3FFFF`) |

```text
 CPU address space                        External SRAM (XSRAM)
+--------------------------+ 0x0000       +---------------------+ 0x00000
| System globals /         |  Internal    |                     |
| stack / heap             |  SRAM        |                     |
+--------------------------+ 0x1100       |                     |
| Main system              |  FPGA        |   (whole XSRAM      |
| registers / buffers      |  SRAM        |    reachable        |
+--------------------------+ 0x3000 --+   |    through the      |
| Code                     |          |   |    programmable     |
| (VM Bytecode & constants)|  load    |   |    CPU window       |
|                          |  (.bin)  +-->|    base address)    |
+--------------------------+ 0xB000   |   |                     |
| Data (VM Globals)        |  Data    |   |                     |
|                          |  Origin  |   |                     |
+--------------------------+ 0xC000   |   |                     |
| Compiler buffers         |  (user   |   |                     |
|                          |  select) |   |                     |
+--------------------------+ 0xF000 --+   +---------------------+ 0x3F000
| Scratchpad               |  Fixed   ---> | Fixed XSRAM         |
+--------------------------+ 0xF800  XSRAM | (Scratchpad,        |
| System data              |               |  System data)       |
+--------------------------+ 0xFFFF        +---------------------+ 0x3FFFF
```

## E.2. XSRAM Default memory map

The following figure shows the default External SRAM (XSRAM) memory mapping. The Tiles, Sprites, and Bitmap segments are user-defined and can be modified anytime to allow alternate memory configurations. Values in parentheses next to addresses show the corresponding page settings.

| XSRAM address (page) | Segment | Size |
|---|---|---|
| `0x00000` (`0x00`) | Tiles set | 32 KiB |
| `0x08000` (`0x20`) | Sprite set | 64 KiB |
| `0x18000` (`0x60`) | Bitmap (Draw & Display) | 48000 Bytes (400x240 4bpp) |
| `0x23B80` | Unassigned | |
| `0x33000` (`0xCC`) | VM | 48 KiB |
| `0x3F000` - `0x3FFFF` | Scratchpad / System data (Fixed XSRAM) | 4 KiB |

## E.3. XSRAM Alt memory map

The following figure shows an alternative XSRAM memory configuration. In this configuration, two distinct bitmap areas are declared using registers `0x2F05` & `0x2F16` (draw and display buffers) to allow bitmap double buffering. They are typically swapped during vertical blanking.

| XSRAM address (page) | Segment | Size |
|---|---|---|
| `0x00000` (`0x00`) | Tiles set | 32 KiB |
| `0x08000` (`0x20`) | Sprite set | 64 KiB |
| `0x18000` (`0x60`) - `0x23B7F` | Bitmap Draw (Display) | 48000 Bytes (400x240 4bpp) |
| `0x23C00` (`0x8F`) - `0x2F77F` | Bitmap Display (Draw) | 48000 Bytes (400x240 4bpp) |
| `0x2F780` | Unassigned | |
| `0x33000` (`0xCC`) | VM | 48 KiB |
| `0x3F000` - `0x3FFFF` | Scratchpad / System data (Fixed XSRAM) | 4 KiB |

## E.4. Registers & buffers overview

| CPU address | Content | Physical memory |
|---|---|---|
| `0x0000` | System globals / stack / heap | Internal SRAM |
| `0x1100` | Game Controllers | FPGA SRAM |
| `0x1102` | YM2612 | FPGA SRAM |
| `0x1104` | Unused | FPGA SRAM |
| `0x1110` | Built-in Synth Registers | FPGA SRAM |
| `0x1120` | Unused | FPGA SRAM |
| `0x113C` | Mouse Registers | FPGA SRAM |
| `0x1140` | Palettes | FPGA SRAM |
| `0x1200` | Character Glyph Index Buffer (80x40) | FPGA SRAM |
| `0x1E80` | Character Color Index Buffer (80x40) | FPGA SRAM |
| `0x2B00` | Tile Buffer A (32x16) | FPGA SRAM |
| `0x2D00` | Tile Buffer B (32x16) | FPGA SRAM |
| `0x2F00` | Graphics / System Registers | FPGA SRAM |
| `0x2F20` | Sprites Registers (32) | FPGA SRAM |
| `0x2FA0` | Sprites Palette B - Glyphs | FPGA SRAM |
| `0x3000` | VM | External SRAM |
| `0xF000` | Scratchpad | Fixed XSRAM |
| `0xF800` - `0xFFFF` | System Data | Fixed XSRAM |

## E.5. Registers list

### E.5.1. System

#### Mouse pointer

These registers are write-only.

| Address | Bits | Field |
|---|---|---|
| `0x2F0D` | 7:0 | Pos X Low |
| `0x2F0E` | 7:0 | Pos Y Low |
| `0x2F0F` | 7:5 | Unused |
| `0x2F0F` | 4:3 | Pointer shape |
| `0x2F0F` | 2 | Y High |
| `0x2F0F` | 1:0 | X High |
| `0x2F10` | 7:3 | Unused |
| `0x2F10` | 2:0 | Red color component |
| `0x2F11` | 7:3 | Unused |
| `0x2F11` | 2:0 | Green color component |
| `0x2F12` | 7:3 | Unused |
| `0x2F12` | 2:0 | Blue color component |

#### Status

| Address | Bits | Field |
|---|---|---|
| `0x2F0B` | 7 | pointer enable |
| `0x2F0B` | 6:3 | Unused |
| `0x2F0B` | 2 | tileDB |
| `0x2F0B` | 1 | cntRes |
| `0x2F0B` | 0 | vBlank |
| `0x2F0C` | 7:0 | Frame counter |

#### Raster comparator

| Address | Bits | Field |
|---|---|---|
| `0x2F09` | 7:0 | Raster Comparator Low |
| `0x2F0A` | 7:0 | Raster Comparator High |

#### Memory configuration

| Address | Bits | Field |
|---|---|---|
| `0x2F03` | 7:0 | Tile-set Page |
| `0x2F04` | 7:0 | Sprite-set Page |
| `0x2F05` | 7:0 | Bitmap Display (Scan-out) Page |
| `0x2F16` | 7:0 | Bitmap Draw Page |
| `0x2F06` | 7:0 | CPU Window Page |

#### MIDI event

| Address | Bits | Field |
|---|---|---|
| `0xFFD0` | 7:0 | Event type |
| `0xFFD1` | 7:0 | MIDI Channel |
| `0xFFD2` | 7:0 | Parameter 1 |
| `0xFFD3` | 7:0 | Parameter 2 |

### E.5.2. Graphics

#### Modes

| Address | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|---|---|---|---|---|---|---|---|---|
| `0x2F00` | Overlay | Grid | Bmp tr. | Text | Tiles | Sprites | Tile Op. | Bitmap |

#### Background color

| Address | Bits | Field |
|---|---|---|
| `0x2F13` | 7:3 | Unused |
| `0x2F13` | 2:0 | Red component |
| `0x2F14` | 7:3 | Unused |
| `0x2F14` | 2:0 | Green component |
| `0x2F15` | 7:3 | Unused |
| `0x2F15` | 2:0 | Blue component |

#### Glyph definition

These registers are write only.

| Address | Bits | Field |
|---|---|---|
| `0x2FD0` | 7:0 | Pixel address low |
| `0x2FD1` | 7 | Unused |
| `0x2FD1` | 6:0 | Pixel address high |
| `0x2FD2` | 7:2 | Unused |
| `0x2FD2` | 1 | Value |
| `0x2FD2` | 0 | WREN |

#### Tiles hardware scrolling

| Address | Bits | Field |
|---|---|---|
| `0x2F01` | 7:5 | Unused |
| `0x2F01` | 4:0 | Horizontal tile offset |
| `0x2F02` | 7:5 | Unused |
| `0x2F02` | 4:0 | Vertical tile offset |

#### Sprites

The following registers repeat 32 times (from `0x2F20` to `0x2F9F`):

| Address | Bits | Field |
|---|---|---|
| `0x2F20` | 7:0 | Ref in Sprite-set |
| `0x2F21` | 7:0 | Pos X Low |
| `0x2F22` | 7:0 | Pos Y Low |
| `0x2F23` | 7 | Flip Y |
| `0x2F23` | 6 | Flip X |
| `0x2F23` | 5 | Pal |
| `0x2F23` | 4 | Prio |
| `0x2F23` | 3:2 | Pos Y High |
| `0x2F23` | 1:0 | Pos X High |

### E.5.3. Palettes

The following registers are the base registers for each palette. Each register repeats incrementally 16 times, one for each palette index. For example, the registers for the Red components of the sprite Palette A span from `0x11D0` to `0x11DF`.

In every palette register, bits 7:3 are unused and bits 2:0 hold the color component value.

#### Sprites

| Base address | Bits | Field |
|---|---|---|
| `0x11D0` | 2:0 | Sprite A Red |
| `0x11E0` | 2:0 | Sprite A Green |
| `0x11F0` | 2:0 | Sprite A Blue |
| `0x2FA0` | 2:0 | Sprite B Red |
| `0x2FB0` | 2:0 | Sprite B Green |
| `0x2FC0` | 2:0 | Sprite B Blue |

#### Bitmap

| Base address | Bits | Field |
|---|---|---|
| `0x11A0` | 2:0 | Bitmap Red |
| `0x11B0` | 2:0 | Bitmap Green |
| `0x11C0` | 2:0 | Bitmap Blue |

#### Tiles

| Base address | Bits | Field |
|---|---|---|
| `0x1170` | 2:0 | Tile Red |
| `0x1180` | 2:0 | Tile Green |
| `0x1190` | 2:0 | Tile Blue |

#### Characters

| Base address | Bits | Field |
|---|---|---|
| `0x1140` | 2:0 | Char Red |
| `0x1150` | 2:0 | Char Green |
| `0x1160` | 2:0 | Char Blue |

### E.5.4. Mouse

| Address | Bits | Field |
|---|---|---|
| `0x113F` | 7:0 | Unused |
| `0x113E` | 7:0 | Delta Y |
| `0x113D` | 7:0 | Delta X |
| `0x113C` | 7:5 | Unused |
| `0x113C` | 4 | SignY |
| `0x113C` | 3 | SignX |
| `0x113C` | 2 | Middle |
| `0x113C` | 1 | Right |
| `0x113C` | 0 | Left |

### E.5.5. Built-in Synthesizer

| Address | Bits | Field |
|---|---|---|
| `0x1110` | 7:6 | OSC1 Waveform |
| `0x1110` | 5:4 | OSC0 Waveform |
| `0x1110` | 3 | Sync1 |
| `0x1110` | 2 | Sync0 |
| `0x1110` | 1 | Trigger1 |
| `0x1110` | 0 | Trigger0 |
| `0x1111` | 7:0 | OSC0 Freq MSB / DPCM Instrument |
| `0x1112` | 7:0 | OSC0 Freq LSB / DPCM Playback Pitch |
| `0x1113` | 7:0 | OSC1 Freq MSB / DPCM Instrument |
| `0x1114` | 7:0 | OSC1 Freq LSB / DPCM Playback Pitch |
| `0x1115` | 7 | AMP1 Mod |
| `0x1115` | 6 | AMP0 Mod |
| `0x1115` | 5 | OSC1 Mod |
| `0x1115` | 4 | OSC0 Mod |
| `0x1115` | 3:2 | PAN1 |
| `0x1115` | 1:0 | PAN0 |
| `0x1116` | 7:4 | Env0 Decay |
| `0x1116` | 3:0 | Env0 Attack |
| `0x1117` | 7:4 | Env0 Release |
| `0x1117` | 3:0 | Env0 Sustain |
| `0x1118` | 7:4 | Env1 Decay |
| `0x1118` | 3:0 | Env1 Attack |
| `0x1119` | 7:4 | Env1 Release |
| `0x1119` | 3:0 | Env1 Sustain |
| `0x111A` | 7:5 | LFO0 Amplitude |
| `0x111A` | 4:2 | LFO0 Frequency |
| `0x111A` | 1:0 | LFO0 Waveform |
| `0x111B` | 7:5 | LFO1 Amplitude |
| `0x111B` | 4:2 | LFO1 Frequency |
| `0x111B` | 1:0 | LFO1 Waveform |
| `0x111C` | 7 | Filter |
| `0x111C` | 6 | OSC1 filter |
| `0x111C` | 5:4 | Filter mode |
| `0x111C` | 3:2 | Cutoff control |
| `0x111C` | 1 | LFO1 Mod |
| `0x111C` | 0 | LFO0 Mod |
| `0x111D` | 7:0 | Cutoff Frequency |
| `0x111E` | 7:0 | Resonance 'q' factor |
| `0x111F` | 7:4 | AMP1 Gain |
| `0x111F` | 3:0 | AMP0 Gain |

#### YM2612

| Address | Bits | Field |
|---|---|---|
| `0x1102` | 7:5 | Unused |
| `0x1102` | 4 | `_CS` |
| `0x1102` | 3:0 | Unused |
| `0x1103` | 7:0 | Data |

### E.5.6. Game controllers

#### Left

| Address | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|---|---|---|---|---|---|---|---|---|
| `0x1100` | Right | Left | Down | Up | Select | Start | B | A |

#### Right

| Address | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|---|---|---|---|---|---|---|---|---|
| `0x1101` | Right | Left | Down | Up | Select | Start | B | A |

# Appendix F. Troubleshooting

- **The power LED does not turn ON**

  Check the power cable and the power switch at the rear of the machine.

- **The image does not display on the TV/Monitor**

  Check the video cable is inserted correctly.

  Make sure your TV/Monitor supports WVGA resolution. Alternatively, try the other video output (analog if the TV/Monitor is plugged into the digital output and vice-versa).

- **The image is stretched on the TV/Monitor**

  Make sure to use a TV/monitor with a 16/9 or 16/10 aspect ratio. Navigate the menu of your TV/monitor and search for an 'Exact scan' option or an 'Auto adjust' feature. Some older TVs/Monitors might exhibit black borders. As long as the entire content is visible, this is not a problem.

- **Some part of the image is missing on the TV/monitor**

  Navigate the menu of your TV/monitor and search for an 'Exact scan' option or an 'Auto adjust' feature. Some older TVs/monitors might not support the WVGA resolution. Please use a more modern TV/Monitor.

- **There is no sound coming out of the TV/monitor**

  This is perfectly normal. No audio transits through video outputs. To benefit from the audio capabilities of Micro-8, please connect a stereo system to the line-out port of your unit.

# Appendix G. Warranty

## Warranty Coverage

Your Micro-8 computer system is covered by a two (2) year limited warranty from the date of purchase. This warranty covers defects in materials and workmanship under normal use.

## Exclusions

- The YM2612 sound chip is a vintage component and is therefore excluded from the standard warranty. While every effort is made to ensure the chip is fully functional at the time of assembly, no warranty is provided for this component.
- This warranty does not cover damage resulting from:
  - Improper use, accident, abuse, or modification.
  - Use of power supplies outside the specified 6.5-9V DC range.
  - Use of controllers other than the ones supplied with the unit.
  - Normal wear and tear.
  - Acts of nature (e.g., lightning, flooding).

## Remedies

If a defect covered by this warranty arises, the manufacturer will, at its discretion:

1. Repair the product, or
2. Replace the product with a new or refurbished unit of equivalent function, or
3. Refund the purchase price.

## Limitations

- This warranty is non-transferable and applies only to the original purchaser.
- Shipping costs to return the product for service are the responsibility of the purchaser.
- Liability is limited to the repair, replacement, or refund of the Micro-8 unit as described above.

## Consumer Rights

This warranty does not affect statutory rights that may apply under applicable consumer protection laws in the country of the original purchaser.

# Appendix H. Compliance

## Regulatory Compliance

- This product complies with EMC Directive 2014/30/EU and RoHS Directive 2011/65/EU (as amended).
- This device complies with Part 15 of the FCC Rules. Operation is subject to the following conditions:
  1. This device may not cause harmful interference.
  2. This device must accept any interference received, including interference that may cause undesired operation.

## EU Declaration of Conformity (Simplified)

Hereby, Franck Sauer Digital declares that the Micro-8 computer system is in conformity with the essential requirements and other relevant provisions of the following directives:

- Directive 2014/30/EU (EMC Directive)
- Directive 2011/65/EU (RoHS Directive), as amended by 2015/863/EU

The full text of the EU Declaration of Conformity is available at: www.micro-8.com

## Disposal Notice

This product should not be disposed of with household waste. Please recycle in accordance with local regulations.

# Appendix I. Acknowledgements

The following persons and resources have been instrumental in making this project possible (in no particular order).

- 'Crafting Interpreters' by Robert Nystrom
- 'Small subset of C' code example by Lennart Augustsson
- 'uBasic' code example by Adam Dunkels
- YM2612 code example by Furtek
- Maxim's World of Stuff for the YM2612 documentation
- Casey Muratori at computerenhance.com for the DDA circle algorithm
- The 'LodePNG' library by Lode Vandevenne for early conversion tools
- FatFs FAT file system library by ChanN
- PS/2 Mouse HDL controller by Scott Larson at Digi-Key Electronics
- 'Verilog by example' by Blaine C. Readler
- 'Designing Video Game Hardware in Verilog' by Steven Hugg
- 'Digital Design and Computer Architecture, 2nd edition' by Harris & Harris
- 'Designing Software Synthesizer Plug-ins in C++' by Will C. Pirkle
- 'Designing Audio Effect Plugins in C++' by Will C. Pirkle
- 'Crash Course Electronics and PCB Design' by Andre LaMothe (Udemy)
- All the people at Incubhacker who helped spark my interest in electronics
- Fernando Coello Borges 'Kempston', David Gervais, and Fabrice Lete, who helped find many nasty bugs and wrote several cool Lofi programs
- Chat-GPT, Grok and Gemini who have become increasingly helpful across many topics (when not hallucinating, that is)

This document has been created in LaTeX using Overleaf, Inkscape, and Draw.io.

# Appendix J. Afterword

This project has been a long ride! Although the principal development has reached maturity, this is far from the end. Micro-8 continues its journey through its community of users who explore, tinker, create, and share.

Stay in touch and get the latest news and updates at www.micro-8.com

See you there!

Franck.

# Glossary

**bytecode** - A compact, byte-oriented virtual Instruction Set Architecture (or Instruction Set Assembly) (ISA) to be executed on a Virtual Machine (VM).

**nibble** - Sometimes written 'nybble', is a four-bit aggregation, or half an octet. There are two nibbles in a byte, namely the upper and the lower nibbles.

# Acronyms

**AABB** - Axis Aligned Bounding Box.

**ADC** - Analog to Digital Converter.

**API** - Application Programming Interface.

**ASCII** - American Standard Code for Information Interchange.

**BASIC** - Beginners' All-purpose Symbolic Instruction Code.

**CLI** - Command Line Interpreter.

**CLUT** - Color LookUp Table.

**CPU** - Central Processing Unit.

**CRC** - Cyclic Redundancy Check.

**DPCM** - Differential Pulse Code Modulation.

**EEPROM** - Electrically Erasable Programmable ROM.

**EOF** - End Of File.

**EOL** - End Of Line.

**FIFO** - First In First Out.

**FM** - Frequency Modulation.

**FPGA** - Field Programmable Gate Array.

**GUI** - Graphical User Interface (UI).

**HDL** - Hardware Description Language.

**I/O** - Input/Output.

**IIR** - Infinite Impulse Response.

**ISA** - Instruction Set Architecture (or Instruction Set Assembly).

**LED** - Light Emitting Diode.

**LFO** - Low Frequency Oscillator.

**LSB** - Least Significant Byte.

**MCU** - Micro Controller Unit.

**MIDI** - Musical Instrument Digital Interface.

**MSB** - Most Significant Byte.

**OS** - Operating System.

**PSG** - Programmable Sound Generator.

**PWM** - Pulse Width Modulation.

**RAM** - Random Access Memory.

**RISC** - Reduced Instruction Set Computing.

**ROM** - Read Only Memory.

**SRAM** - Static RAM.

**SVF** - State Variable Filter.

**UART** - Universal Asynchronous Receiver/Transmitter.

**UI** - User Interface.

**VGA** - Video Graphic Array.

**VM** - Virtual Machine.

**WREN** - Write Enable.

**WVGA** - Wide Video Graphic Array (VGA).

**XSRAM** - External SRAM.

---

**WWW.MICRO-8.COM**
