# Frequently Asked Questions

## General

### What is this mod?

Universum Infinitum: Galaxy Sizes adds six new galaxy size options to Stellaris, ranging from 100 to 2000 star systems. It is a purely declarative mod — no code, no scripts, only configuration files that the Stellaris engine reads at galaxy generation time.

### Which Stellaris versions are supported?

Stellaris 4.5.x (see `supported_version` in descriptor). Tested on 4.5.0 through 4.5.2.

### Is this mod compatible with other mods?

Yes, generally compatible with:
- UI mods (UI Star Names, UI Overhaul, etc.)
- Gameplay mods that don't modify galaxy generation
- Species/empire packs
- Graphics mods

Known hard dependency: **Ludicrous (2000 stars) requires UI Star Names mod** for unique system names. Without it, duplicate names will occur.

### Does this mod affect performance?

Yes. Larger galaxies take longer to generate and use more memory:

| Size | Stars | Generation Time | RAM (approx) |
|------|-------|-----------------|--------------|
| Minuscule | 100 | < 5s | Baseline |
| Gigantic | 1200 | 10-20s | ~2x |
| Immense | 1400 | 15-30s | ~2.5x |
| Colossal | 1600 | 20-40s | ~3x |
| Titanic | 1800 | 30-60s | ~3.5x |
| Ludicrous | 2000 | 60-120s | ~4x |

Recommended: 16GB+ RAM for Titanic/Ludicrous.

### Can I use this mod on existing saves?

No. Galaxy size is determined at galaxy generation. You must start a new game.

### Does this mod work in multiplayer?

Yes, all players must have the mod active with the same version. Host generates the galaxy.

## Installation

### How do I install from Steam Workshop?

1. Subscribe to "Universum Infinitum: Galaxy Sizes" on Steam Workshop
2. Launch Stellaris
3. Open Playsets, create or edit a playset
4. Enable the mod
5. Play

### How do I install manually?

1. Download latest release from GitHub Releases
2. Extract `ui-galaxy-sizes.mod` and `ui-galaxy-sizes/` folder to:
   - Windows: `%USERPROFILE%\Documents\Paradox Interactive\Stellaris\mod\`
   - macOS: `~/Documents/Paradox Interactive/Stellaris/mod/`
   - Linux: `~/.local/share/Paradox Interactive/Stellaris/mod/`
3. Launch Stellaris, enable in Playsets

### The mod doesn't appear in the launcher

Check:
- Files in correct mod directory (see above)
- `ui-galaxy-sizes.mod` and `ui-galaxy-sizes/` folder both present
- Stellaris version matches `v4.5.*`
- No file permission issues
- Restart launcher

### Can I use this with the Paradox Launcher v2?

Yes, works with both launcher versions.

## Gameplay

### Which galaxy size should I choose?

| Playstyle | Recommended Size |
|-----------|------------------|
| Quick games, testing | Minuscule (100) |
| Standard with more room | Gigantic (1200) |
| Long campaigns | Immense (1400) |
| Epic scale | Colossal (1600) |
| Maximum vanilla-like | Titanic (1800) |
| Absolute maximum | Ludicrous (2000) |

### Why are there only 6 sizes? Why not 500, 800, etc.?

Each size requires a separate scenario file with 40+ tuned parameters. Adding sizes increases maintenance burden. The six sizes cover a logarithmic progression from tiny to extreme.

### Can I customise empire counts?

The mod sets defaults and maximums per size. You can adjust in game setup:
- Number of AI Empires (up to scenario max)
- Advanced AI Starts
- Fallen Empires (up to scenario max)

### Why does Ludicrous need UI Star Names?

Stellaris has ~1000 unique system names in vanilla. Ludicrous generates 2000 systems. Without UI Star Names (which adds thousands more), the engine must reuse names, creating duplicates.

### Do galaxy shapes work with all sizes?

Yes. All 11 shapes supported for all 6 sizes:
- Elliptical
- Spiral (2, 3, 4, 6 arms)
- Ring
- Bar
- Starburst
- Cartwheel
- Spoked

### Are crisis strengths balanced?

Crisis strength scales with galaxy size:
- Minuscule: 0.25x
- Gigantic: 1.75x
- Immense: 2.0x
- Colossal: 2.25x
- Titanic: 2.5x
- Ludicrous: 2.75x

This approximates vanilla scaling. Adjust in game setup if desired.

## Technical

### How does this mod work?

It provides `setup_scenario` definitions in `map/setup_scenarios/`. The Stellaris engine reads these at galaxy generation and uses the parameters to configure the galaxy. No code execution involved.

### Can I edit the parameters myself?

Yes. Edit `ui-galaxy-sizes/map/setup_scenarios/ui_<size>.txt`. But:
- Changes affect all new games
- May break balance
- Updates will overwrite your changes
- Consider forking instead

### Why is the localisation folder spelled "engligh"?

Historical typo preserved for compatibility. The engine reads it correctly. Fixing would break existing installations.

### What file format are the scenarios?

Clausewitz script format (Paradox's declarative configuration language). Similar to INI but with nested blocks.

### Can I add more shapes?

Only if Stellaris engine supports them. The mod declares `supports_shape` for shapes the engine already knows. Adding unknown shapes has no effect.

## Troubleshooting

### Galaxy generation takes forever / crashes

- Try smaller size
- Close other applications
- Verify RAM (16GB+ for Titanic/Ludicrous)
- Check `game.log` for errors

### Mod shows but sizes missing from dropdown

- Verify all 6 scenario files exist in `map/setup_scenarios/`
- Check `game.log` for parse errors
- Reinstall mod

### System names are duplicated

- Expected for Ludicrous without UI Star Names
- Install UI Star Names mod
- Or choose smaller size

### Save game won't load after mod update

- Galaxy size mods can break saves if parameters change
- Start new game
- Mod version pinned in descriptor to prevent accidental updates

### Launcher says "mod incompatible"

- Your Stellaris version not in `v4.5.*`
- Update Stellaris or wait for mod update

## Development

### How do I contribute?

See `CONTRIBUTING.md`. Summary:
1. Fork repository
2. Create branch
3. Make changes
4. Run validation scripts
5. Test in-game
6. Submit PR

### How do I build/test locally?

No build needed. Copy files to Stellaris mod directory and test in-game.

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