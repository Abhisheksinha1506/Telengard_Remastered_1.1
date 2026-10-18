# Telengard Remastered v1.1

A Commodore 64 dungeon crawler built with Commodore BASIC and 6502 assembly. Explore a dungeon, fight monsters, collect treasure, cast spells, and return to an inn to recover and bank your gold.

This remaster expands the dungeon to **250 levels**, updates the graphics and interface, and moves performance-sensitive rendering into machine language. The repository includes playable disk images, the CBM prg Studio project, graphics source assets, and release notes.

## Contents

- [Play the game](#play-the-game)
- [Starting a character](#starting-a-character)
- [Controls](#controls)
- [Saving and loading](#saving-and-loading)
- [Gameplay and spells](#gameplay-and-spells)
- [Remastered features](#remastered-features)
- [Repository layout](#repository-layout)
- [Working with the source](#working-with-the-source)
- [Troubleshooting](#troubleshooting)
- [Credits and license](#credits-and-license)

## Play the game

### Requirements

- A Commodore 64 emulator with D64 disk-image support, or compatible C64 hardware and a way to mount or transfer a D64 image.
- A writable copy of the disk image if you want to save characters.
- Keyboard access to C64 function keys, cursor keys, and RUN/STOP.

The ready-to-play images do not require a source build or modern application runtime.

### Choose a disk image

| Image | Description |
| --- | --- |
| `Telengard Remastered v1.1.d64` | Standard release disk image. |
| `Telengard Remastered v1.1 - exomized.d64` | Alternate image labeled as Exomized/compressed. |

Both images contain `TELENGARD` and `DISK TELENGARD` program files, plus an existing sequential character file. Make a working copy before playing so your saves do not alter the original distribution image.

### Launch

1. Start your emulator in Commodore 64 mode.
2. Attach your chosen D64 image to **device 8**.
3. Autostart `TELENGARD`, or enter the following at the C64 BASIC prompt:

   ```basic
   LOAD "TELENGARD",8
   RUN
   ```

4. Allow the loader to finish loading `DISK TELENGARD` from the same disk.

Keep that image attached while playing: the game uses device 8 for character storage. Use your emulator's keyboard mapping to send C64 keys if your computer intercepts function keys.

## Starting a character

At the title menu:

- **S** starts a new character.
- **R** reads an existing character from disk.

New characters receive six rolled attributes: strength, intelligence, wisdom, constitution, dexterity, and charisma. Press **Return** to accept the displayed statistics; another key rerolls them.

Enter a name of up to **16 characters**, using letters, digits, or spaces. The game then asks whether to enable permadeath. This choice affects how saved characters are handled; see below.

## Controls

These are the commands shown by the game's help screen and handled by the source. Commands during encounters and room events depend on the displayed prompt.

| Key | Action |
| --- | --- |
| **W** / **8** | Move north. |
| **X** / **2** | Move south. |
| **A** / **4** | Move west. |
| **D** / **6** | Move east. |
| **S** / **5** | Stay in place. |
| **H** | Show help. |
| **P** | Re-plot/redraw the display. |
| **C** | Cast a spell. |
| **F** | Fight when prompted during an encounter. |
| **E** | Evade when prompted during an encounter. |
| **F1** | Save the character to disk. |
| **F3** | Use a scroll of rescue, if available. |
| **F5** | Drink a healing potion, if available. |
| **F7** | Drink a strength potion, if available. |
| **Q** | Quit, with confirmation. |
| **RUN/STOP** | Pause when the game expects a keypress. |
| **Return** | Confirm or interact when prompted. |

**Timing matters:** the game reads commands with a timeout, and waiting at the normal command prompt becomes a stay action. Use RUN/STOP to pause instead of assuming the game will wait indefinitely.

### Character selection

| Key | Action |
| --- | --- |
| **Cursor Up / Down** | Select a character. |
| **Return** | Load the selected character. |
| **D** | Request deletion of the selected character; the game asks for confirmation. |
| **B** | Return to the main menu. |

The release notes describe a menu listing up to 20 character files.

## Saving and loading

Press **F1** at the command prompt or at the inn's save prompt. A successful save returns to the title menu. Saves are sequential files on the disk mounted as device 8, and saving under an existing character name replaces that character's previous file.

### Permadeath

- **Enabled:** loading a character removes its saved file from the disk. You must save again during play to create another stored character.
- **Disabled:** the character's stored name uses an internal `sv` prefix. The interface hides this prefix, and loading preserves the saved file.

The character menu shows the permadeath setting. Character filenames no longer use a `.plr` extension. The loader checks the save-format version; do not assume saves from other Telengard versions are compatible.

For preservation or experimentation, back up the working D64 image before playing. Emulator snapshots are separate from the game's own character-save system.

## Gameplay and spells

The dungeon uses a 200-by-200 coordinate grid and supports depths up to 250. Explore rooms, encounter monsters and dungeon features, and manage your health, spell units, equipment, and temporary effects.

### Inns and exploration

Returning to an inn cashes in carried gold, adds it to experience and the safe, restores health and spell units, and clears temporary effects. The inn offers a choice to re-enter the dungeon or save the character.

The remaster tracks the last **100 locations** visited. Non-monster treasure is not awarded at coordinates still in that history, reducing repeated treasure collection from recently visited locations.

### Casting spells

Press **C**, choose a spell level, and then choose a spell number. At the spell-selection prompt, **=** lists the spells for that level. Spell availability depends on the character's progression and remaining spell units. Some spells are intended for combat and can be wasted outside an encounter.

The source defines six groups of six spells:

| Level | Spells |
| --- | --- |
| 1 | Magic Missile, Sleep, Cure Light Wounds, Light, Turn Undead, Protection from Evil |
| 2 | Web, Levitate, Cause Light Wounds, Detect Traps, Charm, Strength |
| 3 | Lightning Bolt, Cure Serious Wounds, Continual Light, Invisibility, Hold Monster, Phantasmal Forces |
| 4 | Pass Wall, Fireball, Cause Serious Wounds, Flesh to Stone, Fear, Finger of Death |
| 5 | Teleport, Astral Walk, Power Word Kill, Ice Storm, Wall of Fire, Plague |
| 6 | Time Stop, Raise Dead, Holy Symbol, Word of Recall, Restoration, Prismatic Wall |

Some names are abbreviated on the C64 display. Active duration-based spell effects appear in the status area.

## Remastered features

### Version 1.1

The included [release notes](release-notes.txt) document:

- Correct spell-point updates on the character sheet while at an inn.
- Correct wall detection when evading monsters.
- A fix for a FOR/NEXT stack overflow caused by a fountain animation continuing at the command prompt.
- A minimum monster level of `floor(dungeon level / 2 + 1)`.
- Hobbit stealing chances that account for monster level.
- Elven cloak detection checks that account for monster level and give stronger monster types detection bonuses.

### Version 1.0 foundation

- Expanded dungeon depth from 50 to 250 levels and increased item bonuses.
- Multi-sprite player, monster, treasure, and room-feature graphics.
- Gray doors for clearer distinction from walls.
- 64 custom character graphics and sprite-based spell effects.
- A more colorful character sheet and highlighted commands.
- Longer displayed names, lowercase support, and a character selection menu.
- Immediate Light spell rendering and more frequent spell-status updates.
- Machine-language dungeon re-plotting, sprite handling, and light rendering.
- Revised memory use and a shorter action delay, from 1.5 seconds to 1.0 second.

See the release notes for the full change history.

## Repository layout

```text
.
├── readme.md
├── LICENSE
├── release-notes.txt
├── Telengard Remastered v1.1.d64
├── Telengard Remastered v1.1 - exomized.d64
├── Telengard Remastered Breakdown.ods
├── CBM PRG Studio Project/
│   ├── Telengard Remastered.cbmprj
│   ├── telengard.bas
│   ├── disk telengard.bas
│   ├── telengard-ml.asm
│   ├── disk-telengard-ml.asm
│   ├── Telengard Character Set.cst
│   ├── Telengard Sprites.spt
│   ├── telengard-inn.bin
│   └── telengard-music.bin
└── Graphics/
    ├── Telengard Graphics.xcf
    ├── Telengard Remastered.ico
    ├── C64-PAL.gpl
    └── C64-NTSC.gpl
```

| File or directory | Purpose |
| --- | --- |
| `*.d64` | Packaged C64 disk images for playing the game. |
| `Telengard Remastered.cbmprj` | CBM prg Studio C64 project; its metadata records application version 3.5.0. |
| `telengard.bas` | Initial title/loader program, including comments describing binary merging. |
| `disk telengard.bas` | Main BASIC game logic, menus, combat, spells, character storage, and data. |
| `telengard-ml.asm` | Startup, memory setup, and relocation routines; begins at `$38D4`. |
| `disk-telengard-ml.asm` | Runtime machine-language routines and memory-map definitions; begins at `$C000`. |
| `*.cst` / `*.spt` | Editable CBM prg Studio character-set and sprite assets. |
| `telengard-inn.bin` / `telengard-music.bin` | Binary inn display and music data. |
| `Telengard Remastered Breakdown.ods` | Included OpenDocument spreadsheet for project reference. |
| `Graphics/` | Layered graphics source, icon, and PAL/NTSC palette files. |

## Working with the source

### Development tools

Use CBM prg Studio to inspect the project and its BASIC, assembly, character-set, and sprite assets. A C64 emulator is needed to check the assembled game. The `.xcf` artwork and `.ods` spreadsheet require applications supporting those formats if you want to edit them.

### Build status and limitations

**The repository does not include a complete automated build or disk-packaging script.** Use the supplied D64 images to play immediately. Rebuilding the release requires reconstructing the asset-export and binary-merging workflow.

1. Open `CBM PRG Studio Project/Telengard Remastered.cbmprj` in CBM prg Studio.
2. Inspect the source and asset settings before building. The project targets C64, uses BASIC start address `2049` (`$0801`), and names its executable `Telengard Remastered.prg`.
3. Resolve the project's reference to **`Zuul.cst`**, which is not present in this checkout. Its intended role is not documented here.
4. Export the character-set and sprite data needed by the loader. Comments in `telengard.bas` describe concatenating lower and upper character patches, sprites, inn data, sprite configuration, sprite coordinates, music, and machine-language programs into a loader PRG.
5. Recreate that merge step using paths appropriate to your machine. The example is a commented shell script with an author's local paths, rather than an executable build script. It strips the two-byte PRG load address from the appended machine-language programs.
6. Package the loader and main game as `TELENGARD` and `DISK TELENGARD` on a working D64 image, then test in a C64 emulator.

The project has pre/post-build events and automatic disk generation disabled. Several intermediate binary names in the commented merge example are not checked in as standalone files, so building the project alone should not be assumed to reproduce either release image.

### Memory and source considerations

The code uses fixed addresses, direct VIC-II/SID access, shared BASIC/assembly workspace, and explicit data lengths. Review the relocation constants in `telengard-ml.asm` whenever changing loader size or exported asset sizes. The release notes identify addresses 679–766 as machine-language workspace and describe sprites residing in VIC-II bank 3.

BASIC listings contain CBM prg Studio control-code notation such as `{clear}` and `{white}`. Open them with tooling that understands this notation when generating tokenized C64 programs.

### Verification checklist

No automated test suite is included. For source changes, use a separate working disk and check:

- The loader reaches the title menu and loads the main game.
- Character creation, movement, room features, and combat work.
- Help, pause, re-plot, spells, and inventory commands behave correctly.
- The inn restores health and spell units, with the character sheet updated.
- Saving and loading work with both permadeath settings.
- Evading respects walls, and fountain interactions return cleanly to the command prompt.

These are suggested manual checks, not a claim that this checkout has been built or play-tested.

## Troubleshooting

| Symptom | What to check |
| --- | --- |
| Loader cannot find the game | Keep the release image attached to device 8 and launch `TELENGARD`; it loads `DISK TELENGARD` by name. |
| Save fails | Check that the working image is writable, has free space, and remains mounted as device 8. |
| A save disappears after loading | This is expected for a character with permadeath enabled. |
| Movement continues while you are reading | Commands have a timeout. Use RUN/STOP to pause. |
| Function keys do not work | Check the emulator's C64 keyboard mapping and your computer's function-key settings. |
| Display needs refreshing | Use **P** to re-plot the screen. |
| Source project reports a missing asset | The project references `Zuul.cst`, which is absent; the release build workflow also needs exported intermediate assets. |

## Credits and license

The main BASIC source credits **Orion Software** for Telengard/64 V4.18, copyright 1981, and **Eric B. Pratt** for Telengard Remastered v1.0, dated 2016-02-09. The loader displays **Avalon Hill** and a 1983 copyright notice. These historical notices are retained in the source.

This repository includes the **GNU General Public License, version 3** in [LICENSE](LICENSE). Consult that file and the notices in the source when using or redistributing the project.
