# Changelog

All notable changes to WR Auto Converter are documented here.

## [1.1.2] — 2026-09-21

### Added
- **Server-side / add-on vehicle conversion — the whole new mode.** Converts a singleplayer vehicle mod into a real FiveM add-on resource (`data/*.meta` + `stream/*` + a generated `fxmanifest.lua`) that a server distributes to every connected player, instead of just a personal client-side replace. Works from either a mod's `dlc.rpf` or loose stream files (with or without their own `.meta` — missing `vehicles.meta`/`handling.meta` get auto-generated). Available from the sidebar's new "Server-Side" nav group, alongside the existing "Client-Side" tools.
- **Bundled handling presets.** A "Handling" picker on every add-on queue row — keep the car's own handling, or pick a bundled preset (Drift/Rally/Speed/Electric/Offroad/Realistic). When the source has no `handling.meta` of its own, the chosen preset also drives the auto-generated `vehicles.meta`'s physical defaults (vehicle class, wheel type, swankness, etc.).
- **Full, consistent rebrand on custom naming.** Renaming a vehicle now covers every stream file that references the old name (not just the primary model), plus rewrites `carcols.meta`'s tuning-kit part/kit references and `carvariations.meta`'s kit entry to match — so a full custom-name conversion no longer leaves half the tuning kit pointing at the old name.
- **WR credit tag** ("Converted by WR Auto Converter - Warraa_ <3") added to every output `.meta` file and the first line of every generated `fxmanifest.lua`.

### Fixed
- **Real in-game crash fixed**: loose add-on stream files could be missing their inline resource header, depending on how the source mod's own `dlc.rpf` stored them — caused an "Invalid fixup" crash on load. Now synthesized correctly when missing.
- **Multi-vehicle source packs are now hard-blocked instead of silently corrupting the output.** If a mod's `vehicles.meta` bundles more than one vehicle, converting only ever renamed the one you picked and shipped every other vehicle through untouched under its original name — a real, confirmed cause of a live server streaming crash (duplicate/colliding names). Now detected and blocked up front with a clear explanation, instead of producing broken output that looks like it succeeded.
- **Files over 16MB are now correctly detected and blocked for add-on conversion** (RPF-sourced only) instead of silently shipping corrupted output — this size class has a storage quirk that can't be losslessly converted to a loose file; the tool tells you why and what to do instead (re-export the file, or convert manually).
- Add-on loose-folder source now correctly keeps every stream file whenever the source folder has any `.meta` file at all (not just when `carcols.meta` specifically is present), and correctly finds a source's own `.meta` files even when you pick the `stream` subfolder directly instead of its parent.

## [1.1.1] — 2026-09-18

### Fixed
- **Real conversion-breaking bug**: stream files over 16MB (a common size for hi-res weapon/vehicle textures) could come out corrupted from the loose-file converters (weapon folder, vehicle folder). The RPF7 format encodes an oversized resource's true size into specific bytes of its own header instead of the archive's normal size field — that encoding only exists on a file that's already been packed into an RPF before, and a loose file read straight off disk never had it applied. Now applied correctly at pack time; caught this via a real broken conversion during testing (a 17.1MB weapon model read back as garbage after conversion).
- **`_hi`/`_mag1` suffix detection now recognizes `+`/`-` as delimiters, not just `_`.** A real mod used `w_ar_carbinerifle+hi.ytd` (plus sign) — the old exact-match detection treated it as a plain base file, which collided with and silently displaced the real base texture during renaming. Confirmed fixed against the exact real file that triggered this.

## [1.1.0] — 2026-09-18

### Added
- **Vehicle converter now accepts loose stream files, not just a dlc.rpf.** Some vehicle mods ship as raw `.yft`/`.ytd` files instead of a packed archive — added a "Browse Folder (raw files)" option alongside the existing dlc.rpf picker. The two source types share one queue and convert together in a single batch.
- **Auto-created output folder.** Leaving the output folder empty is now a valid choice — it auto-creates `Desktop/WR Converted Files/Vehicles` or `/Weapons` and keeps a `README.md` there listing recently converted files, plus links. The app now says "Leave Empty to Auto Create Folder In Desktop" instead of blocking the Convert button.
- **Bicycles added to the vehicle picker** (bmx, cruiser, fixter, scorcher, tribike x3) — image assets existed but had no dataset entries, so they were previously unselectable.

### Fixed
- **Loose-file vehicle folders no longer sweep in tuning/livery parts as if they were the base model.** The dlc.rpf flow already excludes a mod's separate tuning-kit archive; loose-file mode had no equivalent and would rename *every* non-wheel file to the target, including things like bonnets/bumpers/spoilers/liveries. Now the real base model is identified as the one file basename with both a `.yft` and `.ytd` present (the only reliable structural signal), and everything else is excluded with a clear warning — confirmed against a real 81-file mod (BMW M3 G80) with a `tuning/` subfolder full of extra parts.
- **`va_<model>.ycd` (vehicle animation dict) is now correctly recognized and renamed** (`va_<target>.ycd`) instead of being excluded alongside real tuning-kit extras.
- **Native Windows folder-picker dialogs now open at a sane starting path** (Desktop) instead of cold — mitigates (though doesn't fully eliminate, this is an OS-level dialog quirk) a Windows Explorer shell-view glitch where a folder's contents sometimes render blank on first open.

## [1.0.0] — 2026-09-18

First public release.

### Added
- **Vehicle converter**: convert a singleplayer vehicle mod's `dlc.rpf` into a FiveM client-side replace package. Vanilla-target picker with real vehicle photos, search, and category filters (including bicycles). Batch conversion with per-item output naming.
- **Weapon converter**: convert a singleplayer weapon mod (loose `.ydr`/`.ytd` stream files in a folder) into a FiveM client-side replace package. Same picker experience, weapon icon set included.
- **Crash-safety check for vehicles**: blocks converting a mod onto a vanilla target of a mismatched vehicle class (e.g. a truck mod replacing a compact car), which is a confirmed crash/visual-break risk.
- **Weapon stream-file suffix handling**: `_hi` (high-detail model) and `_mag1`/`_mag2`/... (magazine models/textures) are recognized and preserved correctly instead of being renamed into meaningless numbered duplicates.
- **Live activity log** per conversion: every real step (archive read, stream-file location, renames, package build, output write, self-verification), shown by default, with a "Copy log" button.
- **10-second cooldown** on Convert All to prevent accidental double-conversion spam.
- **Variant-folder warning** on the weapon converter page — some weapon mods ship multiple variant subfolders (different barrel lengths, optional reskins, etc.); the tool doesn't choose between them, so the user is told up front to pick the right one themselves.
- Drag-and-drop support, dark WR-branded UI, particle background.

### Notes
- Both the vehicle and weapon converters have been tested and confirmed working in a real FiveM client.
- Server-side (add-on resource) conversion is not implemented yet — this release is client-side replace only.
- Tuning kits / liveries (`carcols.meta`/`carvariations.meta`) are not parsed or converted yet.
