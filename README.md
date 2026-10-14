# fm-save-editor

# Forbidden Memories Save Editor

A save editor for **Yu-Gi-Oh! Forbidden Memories** (PlayStation, US version) that runs entirely in your web browser. Change your deck, the cards in your trunk, and your starchips, then load the edited save in your emulator.

**Use it here:** https://shualyons.github.io/fm-save-editor/

Nothing to install, and your save never leaves your computer.

## Features

- **Deck editing:** put any of the game's 722 cards into any of your 40 deck slots.
- **Trunk editing:** set how many copies of each card you own.
- **Starchips:** set your starchip total (up to 999,999).
- **Card search and sorting** by name, number, ATK, DEF or type, with an "owned only" filter.
- **Deck check:** warns about anything the game would refuse, such as empty slots or more than 3 copies of a card (or more than 1 of each Exodia piece).
- **Styled after the game's own menus.**

## How to use

1. **Close your emulator** so it isn't holding the memory card file.
2. Open the editor and click **Choose file**, then pick your memory card file.
3. Click a slot in your deck, then double-click a card in the trunk to put it there.
4. Click **Save changes**. Your edited memory card file downloads with the same name.
5. Replace your old memory card file with the new one. **Keep a copy of the old one as a backup.**
6. Start the game and use the game's **Load** option. Don't load an emulator save state: states contain the old data from before your edits.

### Supported files

| Format | Used by |
| --- | --- |
| `.mcd` | DuckStation (tested) |
| `.mcr` | ePSXe, Mednafen and others |
| `.srm` | RetroArch |
| `.gme` | DexDrive |

Most raw 128 KB PS1 memory card images should work.

## Is it safe?

- **Your file stays on your computer.** Everything happens in your browser, and nothing is uploaded anywhere.
- **The game checks its own saves.** Forbidden Memories protects its save data with a checksum and refuses saves that don't match. The editor recalculates this exactly as the game does, so edited saves load normally. This has been tested in DuckStation.
- **It won't touch saves it doesn't recognise.** If a save doesn't pass the game's check when you open it, the editor refuses to edit it.

Even so, always keep a backup of your original memory card file.

## Limitations

- **US version only** (SLUS-01411). European versions aren't supported yet because they haven't been tested.
- Card names are shown in English.
- The editor doesn't change Library progress, campaign progress or duelist records.

## How it works

The game keeps its save data in a 0x680-byte block that it writes to the memory card twice, as a main copy and a backup. Inside the block:

| Offset | Size | Contents |
| --- | --- | --- |
| `+0x000` | 80 bytes | Deck: 40 card IDs (16-bit) |
| `+0x050` | 722 bytes | Trunk: number of copies of each card |
| `+0x5E0` | 4 bytes | Starchips |

Three regions of the block are sealed with a CRC-16 and a run of pseudo-random words generated from it. When it loads a save, the game recomputes the seal and treats the save as corrupt if anything differs. The editor reseals both copies every time it saves.

## Credits

- Save format and integrity check: [ygofm-decomp](https://github.com/MaChInEgUn3/ygofm-decomp), a decompilation of the game.
- Card names and stats: [YGO-FM-FusionCalc](https://github.com/Solumin/YGO-FM-FusionCalc).

## Disclaimer

This is an unofficial fan-made tool. It is not affiliated with or endorsed by Konami. Yu-Gi-Oh! is a trademark of Konami. No game code, artwork or other assets are included.

## License

The editor's code is released under the [MIT License](LICENSE). The license covers this project's code only, not the card data or anything belonging to Konami.
