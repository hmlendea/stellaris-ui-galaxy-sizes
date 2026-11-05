# Repository Overview

## Purpose

Universum Infinitum: Galaxy Sizes is a Stellaris mod that extends the galaxy generation system by adding six new galaxy-size scenarios ranging from 100 to 2,000 stars. Each scenario defines generation defaults for AI empires, fallen empires, marauders, advanced empires, nebulae, crisis strength, hyperlane topology, and supported galaxy shapes.

## Scope

The mod is purely declarative — it contains no executable code, scripts, or compiled artefacts. It consists entirely of static configuration files consumed by the Stellaris engine at galaxy generation time.

### What the Mod Owns

- Two mod descriptor files (`ui-galaxy-sizes.mod` and `descriptor.mod`) that declare mod identity, version, supported game version, and filesystem path
- English localisation file mapping scenario keys to human-readable labels
- Six `setup_scenario` definition files in `map/setup_scenarios/` that define all galaxy generation parameters

### What the Mod Does Not Own

- Stellaris game engine or galaxy generator implementation
- System name generation (delegated to the optional Universum Infinitum: Star Names mod for the 2,000-star Ludicrous scenario)
- Distribution platform logic (Steam Workshop, Nexus Mods, Paradox Mods, GitHub Releases)
- Player save games or runtime state

## Principal Capabilities

| Capability | Description |
|------------|-------------|
| Six galaxy sizes | Minuscule (100), Gigantic (1,200), Immense (1,400), Colossal (1,600), Titanic (1,800), Ludicrous (2,000) |
| Scaled defaults | Each size defines appropriate AI empire counts, fallen empires, marauders, advanced empires, crisis strength, nebulae, and hyperlane settings |
| Shape support | All scenarios support elliptical, spiral (2/3/4/6-arm), ring, bar, starburst, cartwheel, and spoked galaxy shapes |
| Localisation | English labels for all six scenario names |
| Version compatibility | Declared support for Stellaris `v4.5.*` |

## Entry Points

The mod has no programmatic entry points. The Stellaris launcher and engine serve as the sole entry points:

1. **Launcher** — Reads `.mod` descriptor files to register the mod
2. **Engine** — Parses `setup_scenario` files when the player selects a galaxy size in the New Game screen

## Architectural Decomposition

```
stellaris-ui-galaxy-sizes/
├── ui-galaxy-sizes.mod          # Root descriptor (Steam/GitHub/Nexus)
├── ui-galaxy-sizes/
│   ├── descriptor.mod           # Local descriptor (Paradox Mods/manual)
│   ├── localisation/
│   │   └── engligh/
│   │       └── ui-galaxy-sizes_l_english.yml
│   └── map/
│       └── setup_scenarios/
│           ├── ui_minuscule.txt
│           ├── ui_gigantic.txt
│           ├── ui_immense.txt
│           ├── ui_colossal.txt
│           ├── ui_titanic.txt
│           └── ui_ludicrous.txt
```

## Major Integrations

| Integration | Type | Purpose | Required |
|-------------|------|---------|----------|
| Stellaris Launcher | Platform | Mod registration and version validation | Yes |
| Stellaris Engine | Runtime | Galaxy generation parameter consumption | Yes |
| Universum Infinitum: Star Names | Optional mod | Provides 2,000 system names for Ludicrous | Only for Ludicrous |

## System-Wide Invariants

- All scenario files are valid `setup_scenario` blocks parseable by Stellaris `v4.5.*`
- Each scenario defines a unique `name` key matching its filename
- Priority values are unique and monotonically increasing (-1, 5, 6, 7, 8, 9)
- All scenarios support the same set of 11 galaxy shapes
- Localisation keys match scenario `name` values exactly
- Mod version in both descriptors matches the release version (1.4.5)
- Supported game version in both descriptors matches (`v4.5.*`)