# Changelog

All notable changes to WR Auto Converter are documented here.

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
