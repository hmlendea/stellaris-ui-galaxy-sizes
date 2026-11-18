# Frequently Asked Questions

## General

### What is this mod?

Universum Infinitum - Galaxy Sizes includes six novel galaxy size options for Stellaris, ranging from 100 to 2000 star systems. It is a purely declarative mod — no code, no scripts, only configuration files that the Stellaris engine reads at galaxy generation time.

### Which Stellaris versions are supported?

Stellaris 4.5.x (see `supported_version` in the descriptor). Tested against 4.5.0 through 4.5.2.

### Is this mod compatible with other mods?

Yes, it is generally compatible with:
- UI mods (UI Star Names, UI Overhaul, etc.)
- Gameplay mods that do not modify galaxy generation
- Species and empire packs
- Graphics mods

Known hard dependency: **Ludicrous (2000 stars) requires the UI Star Names mod** for unique system names. Without it, duplicate names occur.

### Does this mod affect performance?

Yes. Larger galaxies require longer to generate and consume more memory:

| Size | Stars | Generation Time | Memory (approximate) |
|------|-------|-----------------|--------------|
| Minuscule | 100 | < 5s | Baseline |
| Gigantic | 1200 | 10-20s | ~2x |
| Immense | 1400 | 15-30s | ~2.5x |
| Colossal | 1600 | 20-40s | ~3x |
| Titanic | 1800 | 30-60s | ~3.5x |
| Ludicrous | 2000 | 60-120s | ~4x |

Recommended: 16GB or more memory for Titanic and Ludicrous.

### Can I use this mod on existing saves?

No. Galaxy size is determined at galaxy generation. You must start a new game.

### Does this mod function in multiplayer?

Yes, but every player must have the mod active with an identical version. The host generates the galaxy.

## Installation

### How do I install from the Steam Workshop?

1. Subscribe to "Universum Infinitum - Galaxy Sizes" on the Steam Workshop
2. Launch Stellaris
3. Open Playsets and create or edit a playset
4. Activate the mod
5. Play

### How do I install manually?

1. Download the latest release from GitHub Releases
2. Extract `ui-galaxy-sizes.mod` and the `ui-galaxy-sizes/` directory to:
   - Windows: `%USERPROFILE%\Documents\Paradox Interactive\Stellaris\mod\`
   - macOS: `~/Documents/Paradox Interactive/Stellaris/mod/`
   - Linux: `~/.local/share/Paradox Interactive/Stellaris/mod/`
3. Launch Stellaris and activate it in Playsets

### The mod does not appear in the launcher

Check:
- Files in the correct mod directory (see above)
- Both `ui-galaxy-sizes.mod` and the `ui-galaxy-sizes/` directory present
- Stellaris version corresponds to `v4.5.*`
- No file permission issues
- Restart the launcher

### Can I use this with the Paradox Launcher v2?

Yes, it functions with both launcher versions.

## Gameplay

### Which galaxy size should I select?

| Playstyle | Recommended Size |
|-----------|------------------|
| Rapid games and testing | Minuscule (100) |
| Standard play with more room | Gigantic (1200) |
| Long campaigns | Immense (1400) |
| Epic scale | Colossal (1600) |
| Maximum vanilla-like | Titanic (1800) |
| Absolute maximum | Ludicrous (2000) |

### Why are there only 6 sizes? Why not 500 or 800?

Each size requires a separate scenario file with more than 40 tuned parameters. Including sizes increases the maintenance burden. The six sizes span a logarithmic progression from small to extreme.

### Can I customise empire counts?

The mod establishes defaults and maximums per size. You can adjust them in game setup:
- Number of AI Empires (up to the scenario maximum)
- Advanced AI Starts
- Fallen Empires (up to the scenario maximum)

### Why does Ludicrous require UI Star Names?

Stellaris contains roughly 1000 unique system names in vanilla. Ludicrous generates 2000 systems. Without UI Star Names (which includes thousands more), the engine must reuse names, producing duplicates.

### Do galaxy shapes function with all sizes?

Yes, with one vanilla-derived exception. Gigantic through Ludicrous support all 10 shapes:
- Elliptical
- Spiral (2, 3, 4, and 6 arms)
- Ring
- Bar
- Starburst
- Cartwheel
- Spoked

Minuscule declares the identical four shapes vanilla permits at 100 stars (elliptical, spiral_2, ring, bar), because multi-arm shapes degenerate at that scale.

### Are crisis strengths balanced?

Crisis strength follows the vanilla curve `0.25 + 0.00125 × stars`:
- Minuscule: 0.375x
- Gigantic: 1.75x
- Immense: 2.0x
- Colossal: 2.25x
- Titanic: 2.5x
- Ludicrous: 2.75x

This corresponds exactly to the vanilla scaling, so crisis difficulty feels identical at every size. Adjust it in game setup if desired.

## Technical

### How does this mod function?

It supplies `setup_scenario` definitions in `map/setup_scenarios/`. The Stellaris engine reads them at galaxy generation and uses the parameters to configure the galaxy. No code execution is involved.

### Can I edit the parameters myself?

Yes. Edit `ui-galaxy-sizes/map/setup_scenarios/ui_<size>.txt`. However:
- Changes affect all new games
- They may break balance
- Updates overwrite your changes
- Consider forking instead

### Why is the localisation directory spelled "engligh"?

A historic typo preserved for compatibility. The engine reads it correctly. Correcting it would break existing installations.

### What file format do the scenarios use?

Clausewitz script format (Paradox's declarative configuration language). Similar to INI but with nested blocks.

### Can I include additional shapes?

Only if the Stellaris engine supports them. The mod declares `supports_shape` for shapes the engine already recognises. Including unknown shapes has no effect.

## Troubleshooting

### Galaxy generation takes forever or crashes

- Try a smaller size
- Close other applications
- Verify the available memory (16GB or more for Titanic and Ludicrous)
- Check `game.log` for errors

### The mod appears but sizes are absent from the dropdown

- Verify that all 6 scenario files exist in `map/setup_scenarios/`
- Check `game.log` for parse errors
- Reinstall the mod

### System names are duplicated

- Expected for Ludicrous without UI Star Names
- Install the UI Star Names mod
- Or select a smaller size

### Save game will not load after a mod update

- Galaxy size mods can break saves if parameters change
- Start a new game
- The mod version is pinned in the descriptor to prevent accidental updates

### Launcher reports "mod incompatible"

- Your Stellaris version is not within `v4.5.*`
- Update Stellaris or await a mod update

## Development

### How do I contribute?

See `CONTRIBUTING.md`. Summary:
1. Fork the repository
2. Create a branch
3. Make the changes
4. Run the validation scripts
5. Test in-game
6. Submit a pull request

### How do I build/test locally?

No build is required. Copy the files to the Stellaris mod directory and test in-game.

### Where are the validation scripts?

`scripts/validate-*.sh` (to be created). Run `./scripts/validate-all.sh`.

### How do I release a new version?

See `change-guide.md`. Requires signed Git tag, CI validation, manual platform publishing.

## Miscellaneous

### Who maintains this mod?

See `README.md` and GitHub contributors.

### Where do I report bugs?

GitHub Issues. Include:
- Stellaris version
- Mod version
- Scenario used
- `game.log` excerpt
- Steps to reproduce

### Where do I request features?

GitHub Discussions or Issues with "enhancement" label.

### Is there a Discord?

See `README.md` for community links.

### Can I include this in a mod pack?

Yes, GPL v3 allows redistribution with source. Credit required.

### Does this mod collect data?

No. See `PRIVACY.md`. Zero telemetry, zero network calls.
