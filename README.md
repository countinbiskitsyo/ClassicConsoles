# 2A03

A browser emulator front end covering six systems in a single self-contained HTML file.

The NES runs on an emulator written from scratch for this page. The other five
tabs drive RetroArch cores compiled to WebAssembly, loaded through EmulatorJS.
One remapping interface configures all of them.

**No games are included.** Bring your own cartridge dumps and disc images.

---

## Systems

| Tab | Runs on | Notes |
|---|---|---|
| **NES** | written for this project | 6502 CPU, 2C02 picture unit, all five audio channels. Mappers NROM, MMC1, UxROM, CNROM, MMC3, AxROM, GxROM |
| **SNES** | snes9x | |
| **N64** | mupen64plus-next | Needs WebGL2 and a reasonably quick machine. Compatibility varies by game |
| **PlayStation** | pcsx_rearmed | Single-file images load directly; a `.cue` with separate `.bin` tracks is bundled automatically. BIOS optional |
| **Atari** | stella2014, a5200, prosystem, handy, virtualjaguar | 2600, 5200, 7800, Lynx, Jaguar — pick the machine before loading |
| **Arcade** | FinalBurn Neo, MAME 2003-Plus | Romsets are version-specific to the core |

---

## Running it

Open the HTML file in a browser. That is the whole installation.

It works straight from the filesystem, but two things are fetched over the
network: the display font, and the core files for every tab except the NES. The
NES tab needs no network at all.

For a fully offline setup, see [Self-hosting the cores](#self-hosting-the-cores).

---

## Controls

Defaults for player one on the NES tab:

| | |
|---|---|
| D-pad | Arrow keys or WASD |
| A / B | X and Z |
| Start / Select | Enter and Right Shift |

Every system keeps its own layout, and every button is remappable: open
**Controls**, pick Player 1, Player 2 or System, press **Add key**, then press
the key you want. Click a key to remove it. A key can only do one job within a
system, but the same key can mean different things on different tabs.

System-wide keys, also rebindable:

| | |
|---|---|
| Pause | P |
| Reset | R |
| Fullscreen | F |
| Screenshot | F2 |
| Mute | M |

**Save layout** writes a `controls.json` you can reload later. Nothing is stored
in the browser, so save the file if you want a layout to survive a reload.

Controllers are picked up automatically where the browser allows it. Some
embedded contexts block the gamepad API, in which case keyboard input carries on
as normal.

---

## Core sources

Core files are downloaded on first use and cached afterwards. Each tab has a
**Core source** picker that falls back through several options in turn:

1. The EmulatorJS CDN
2. A jsDelivr mirror, which takes a different network route
3. A local `data/` folder

If every source fails, the status line names the ones it tried. A network
blocking one host does not necessarily block the others, so the fallback is
worth letting run before changing anything.

### Self-hosting the cores

Download an EmulatorJS release, put its `data` folder beside the HTML file, and
set **Core source** to *Local data/ folder*.

One catch: browsers block the requests cores need when a page is opened from
`file://`. Serve the folder over HTTP instead —

```
python -m http.server 8080
```

— then open `http://localhost:8080/index.html`. Hosting the repo on GitHub Pages
has the same effect without running anything locally.

Note that `data/` is excluded by `.gitignore` by default. The cores carry their
own licences, some stricter than others about redistribution. Read them before
committing that folder to a public repository.

---

## What not to commit

`.gitignore` excludes ROMs, disc images, BIOS files, arcade romsets and save
data. Keep it in place from the first commit.

Emulator code is fine to publish. Game dumps and console BIOS images are
copyrighted, and public repositories hosting them attract takedown notices. Git
keeps deleted files in history, so a dump committed once is still in the
repository after it is removed from the working tree.

---

## Version history

| | |
|---|---|
| **8.1** | Atari and Arcade tabs, each with a machine picker |
| 8.0 | PlayStation tab, multi-track disc bundling, optional BIOS |
| 7.0 | Fullscreen on all tabs; wake lock fix |
| 6.0 | Fallback core sources with a per-system picker |
| 5.0 | Fix for hosts shipping a partial console object |
| 4.0 | Gamepad polling degrades quietly where the API is blocked |
| 3.0 | SNES and N64 tabs; remapper extended across systems |
| 2.0 | Remappable controls for both players, saveable to a file |
| 1.0 | The NES emulator |

The running version is shown beside the wordmark and in the browser tab. Trust
that badge over the filename.

---

## Credits

The NES emulator is original work. The other tabs are a front end over
[EmulatorJS](https://emulatorjs.org) and the RetroArch cores it loads; those
projects carry their own licences and deserve the credit for the emulation on
those five tabs.
