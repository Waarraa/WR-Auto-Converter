# WR Auto Converter

A free tool that converts GTA V singleplayer vehicle and weapon mods into ready-to-use FiveM client-side mods — no CodeWalker, no OpenIV, no manually editing RPF archives by hand.

## Why I built this

There are thousands of high-quality singleplayer mods out there (vehicles, weapons) that never get ported to FiveM, because converting them by hand is slow, fiddly, and easy to get wrong — wrong file renamed, wrong vanilla target, one mismatched vehicle class and the game can crash. I was doing this conversion manually myself, mod after mod, so I built a tool to automate the process properly: pick your mod, pick what it replaces, and it builds the correctly-structured package for you — with real safety checks so you don't end up with a broken or crash-prone result.

This is a solo project (WR), built and tested against real downloaded mods and real in-game runs, not just theory.

## Features

- **Vehicle converter** — point it at a singleplayer vehicle mod's `dlc.rpf`, pick which vanilla GTA V vehicle it should replace (full photo picker, searchable, grouped by category — including bikes/bicycles), and it builds the converted package.
- **Weapon converter** — point it at a weapon mod's folder (the loose `.ydr`/`.ytd` files), pick the vanilla weapon it replaces, same idea. Correctly handles `_hi` (high-detail model) and `_mag1`/`_mag2` (magazine model/texture) stream files instead of mangling them.
- **Vehicle class crash-safety check** — automatically blocks converting a mod onto a vanilla target of a mismatched vehicle class (e.g. a truck mod replacing a compact car slot). Different classes have different skeletons — this is a real crash/visual-break risk, not a cosmetic warning.
- **Batch conversion** — queue up multiple mods and convert them all in one go, with automatic name collision handling.
- **Drag-and-drop** support for adding mods to the queue.
- **Live activity log** — every real step of a conversion (files read, renamed, packaged, written, self-verified) shown per item, with a "Copy log" button for sharing/debugging.
- **Variant-folder warning** for weapons — some weapon mods ship multiple variant subfolders (different barrels, optional reskins, etc.); the tool doesn't guess which one you want, so it tells you up front to pick the right files yourself.
- **Spam-guard cooldown** on Convert All so you can't accidentally double-fire a batch.
- Dark, WR-branded UI.

## What it doesn't do (yet)

- **Tuning kits / liveries** — mods that add extra tuning parts, custom liveries, or bodykits via `carcols.meta`/`carvariations.meta` aren't parsed or converted. Tested this directly — including the extra files unchanged does not make them work, since the vanilla target's own meta has no entries pointing at them. Real fix needs meta parsing/remapping, which isn't built yet.
- **Server-side / add-on resources** — this tool only builds client-side replace mods (see below), not standalone FiveM add-on resources that a server distributes to every player.
- **Weapon target validation** — for weapons, the tool doesn't check whether your target choice makes sense (e.g. pistol mod onto a pistol slot). That's on you.

## Where does the converted file go?

The output is a single `.rpf` file. This is a **personal client-side mod, not a server resource** — it goes into your own FiveM client's `mods` folder, and only you see it in-game. It's not something you distribute through a server's `resources` folder, and nobody else connected to the same server will see your replaced vehicle/weapon.

## How to use it

1. Download the zip from the [latest release](https://github.com/Waarraa/WR-Auto-Converter/releases/latest) and extract it — the folder has the exe, this README, and the changelog. Run `WR-Auto-Converter.exe`; it's portable, no installer, no admin rights needed.
   > Windows SmartScreen may flag it since it isn't signed with a paid certificate. Click "More info" → "Run anyway".
2. Pick **Convert Vehicles** or **Convert Weapons** from the sidebar.
3. Add your mod:
   - **Vehicle**: drag in (or browse to) the mod's `dlc.rpf`.
   - **Weapon**: browse to the mod's folder. If it has multiple variant subfolders (different barrels, optional reskins, etc.), open it yourself first and remove the variants you don't want — the tool grabs every stream file left in the folder you pick.
4. Click the target picker and choose the vanilla vehicle/weapon it should replace.
5. Pick an output folder.
6. Hit **Convert All**. Check the activity log if you want to see exactly what happened.
7. Drop the converted `.rpf` into your FiveM client's `mods` folder.

## What's coming in future updates

Nothing's promised on a timeline — this is a solo project I work on when I can — but on the roadmap:

- **Tuning kit / livery support** — real `carcols.meta`/`carvariations.meta` parsing and remapping, so extra tuning parts and custom liveries actually work after conversion.
- **Server-side / add-on resource conversion** — converting a singleplayer mod into a proper FiveM add-on resource a server can distribute, not just a personal client-side replace.
- General UI/UX polish and more real-world testing across a wider batch of mods.

Got a feature request or found a bug? Come tell me on Discord - links below.

## Links

- YouTube: https://www.youtube.com/@Warraa__
- Discord: https://discord.com/invite/FKJ27bhqfJ
- GitHub: https://github.com/Waarraa

## License

Closed-source. All rights reserved — this repo exists to distribute the compiled app, not the source.
