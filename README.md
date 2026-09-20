# WR Auto Converter

A free tool that converts GTA V singleplayer vehicle and weapon mods into ready-to-use FiveM mods — client-side personal replaces, or full server-side add-on resources — no CodeWalker, no OpenIV, no manually editing RPF archives by hand.

## Why I built this

There are thousands of high-quality singleplayer mods out there (vehicles, weapons) that never get ported to FiveM, because converting them by hand is slow, fiddly, and easy to get wrong — wrong file renamed, wrong vanilla target, one mismatched vehicle class and the game can crash. I was doing this conversion manually myself, mod after mod, so I built a tool to automate the process properly: pick your mod, pick what it replaces, and it builds the correctly-structured package for you — with real safety checks so you don't end up with a broken or crash-prone result.

This is a solo project (WR), built and tested against real downloaded mods and real in-game runs, not just theory.

## Features

### Client-side (personal replace)
- **Vehicle converter** — point it at a singleplayer vehicle mod's `dlc.rpf` (or a folder of loose `.yft`/`.ytd` stream files, for mods that ship without a packed archive), pick which vanilla GTA V vehicle it should replace (full photo picker, searchable, grouped by category — including bikes/bicycles), and it builds the converted package. Automatically excludes tuning-kit/livery extras instead of mistaking them for the base model.
- **Weapon converter** — point it at a weapon mod's folder (the loose `.ydr`/`.ytd` files), pick the vanilla weapon it replaces, same idea. Correctly handles `_hi` (high-detail model) and `_mag1`/`_mag2` (magazine model/texture) stream files instead of mangling them.
- **Vehicle class crash-safety check** — automatically blocks converting a mod onto a vanilla target of a mismatched vehicle class (e.g. a truck mod replacing a compact car slot). Different classes have different skeletons — this is a real crash/visual-break risk, not a cosmetic warning. (Only runs when a `dlc.rpf`'s `vehicles.meta` is available — loose-file mode has nothing to read a class from.)

### Server-side (add-on resource)
- **Vehicle add-on converter** — converts a singleplayer vehicle mod (`dlc.rpf` or loose stream files) into a real FiveM add-on resource a server distributes to every connected player, complete with `.meta` files, `stream/`, and a generated `fxmanifest.lua`. Give it a unique custom name and it's ready to `ensure` on a server.
- **Bundled handling presets** — keep the car's own handling, or pick from Drift/Rally/Speed/Electric/Offroad/Realistic. If the source mod has no `handling.meta` of its own, the chosen preset also fills in the auto-generated `vehicles.meta`'s physical defaults.
- **Full custom-name rebrand** — renaming covers every stream file that references the old name, not just the primary model, and correctly rewrites the tuning-kit references inside `carcols.meta`/`carvariations.meta` to match, so tuning kits keep working after a rename.
- **Multi-vehicle source pack detection** — some mods bundle more than one vehicle in a single `vehicles.meta`. Converting those automatically would only rename the one you picked and ship the rest through untouched under their original names (a real collision/crash risk on a live server) — the tool detects this and blocks with a clear explanation instead.
- **Oversized-file detection** — some source files can't be losslessly converted to a loose add-on file due to a storage limitation in how they were originally packed; the tool detects and blocks these with an explanation instead of shipping output that will crash.

### Both modes
- **Auto-created output folder** — leave the output folder empty and it creates a folder for you on your Desktop automatically, with a running README of what's been converted there.
- **Batch conversion** — queue up multiple mods and convert them all in one go, with automatic name collision handling.
- **Drag-and-drop** support for adding mods to the queue.
- **Live activity log** — every real step of a conversion (files read, renamed, packaged, written, self-verified) shown per item, with a "Copy log" button for sharing/debugging.
- **Variant-folder warning** for weapons and loose-file vehicle mods — some mods ship multiple variant subfolders (different barrels, optional reskins, etc.); the tool doesn't guess which one you want, so it tells you up front to pick the right files yourself.
- **Spam-guard cooldown** on Convert All so you can't accidentally double-fire a batch.
- Dark, WR-branded UI.

## What it doesn't do (yet)

- **Peds / clothing, maps/MLOs, audio, and other asset types** — vehicles and weapons only for now.
- **Weapon target validation** — for weapons, the tool doesn't check whether your target choice makes sense (e.g. pistol mod onto a pistol slot). That's on you.
- **Server-side weapon/ped add-on conversion** — the add-on (server-side) mode currently covers vehicles only; weapons are client-side replace only for now.

## Where does the converted file go?

- **Client-side output** is a single `.rpf` file — a **personal mod, not a server resource**. It goes into your own FiveM client's `mods` folder, and only you see it in-game. Nobody else connected to the same server will see your replaced vehicle/weapon.
- **Server-side (add-on) output** is a full resource folder — drop it into your server's `resources/` folder and `ensure` it. Every connected player will see it.

## How to use it

1. Download the zip from the [latest release](https://github.com/Waarraa/WR-Auto-Converter/releases/latest) and extract it — the folder has the exe, this README, and the changelog. Run `WR-Auto-Converter.exe`; it's portable, no installer, no admin rights needed.
   > Windows SmartScreen may flag it since it isn't signed with a paid certificate. Click "More info" → "Run anyway".
2. Pick a converter from the sidebar — **Client-Side** (Vehicles/Weapons, personal replace) or **Server-Side** (Vehicles, add-on resource).
3. Add your mod:
   - **Vehicle**: drag in (or browse to) the mod's `dlc.rpf`, or use "Browse Folder" if it's loose `.yft`/`.ytd` files instead.
   - **Weapon**: browse to the mod's folder. If it has multiple variant subfolders (different barrels, optional reskins, etc.), open it yourself first and remove the variants you don't want — the tool grabs every stream file left in the folder you pick.
4. **Client-side**: click the target picker and choose the vanilla vehicle/weapon it should replace. **Server-side**: type a unique custom name, and pick a handling preset if the source has none of its own.
5. Pick an output folder, or leave it empty to auto-create one on your Desktop.
6. Hit **Convert All**. Check the activity log if you want to see exactly what happened.
7. **Client-side**: drop the converted `.rpf` into your FiveM client's `mods` folder. **Server-side**: drop the converted folder into your server's `resources/` folder and `ensure` it.

## What's coming in future updates

Nothing's promised on a timeline — this is a solo project I work on when I can — but on the roadmap:

- **Server-side add-on conversion for weapons and other asset types** — vehicles are covered now, weapons/peds/maps are not yet.
- General UI/UX polish and more real-world testing across a wider batch of mods.

Got a feature request or found a bug? Come tell me on Discord - links below.

## Links

- YouTube: https://www.youtube.com/@Warraa__
- Discord: https://discord.com/invite/FKJ27bhqfJ
- GitHub: https://github.com/Waarraa

## License

Closed-source. All rights reserved — this repo exists to distribute the compiled app, not the source.
