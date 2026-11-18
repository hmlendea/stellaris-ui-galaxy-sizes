# Repository Overview

## Purpose

Universum Infinitum - Galaxy Sizes is a Stellaris mod that extends the galaxy generation system by adding six new galaxy-size scenarios ranging from 100 to 2,000 stars. Each scenario defines generation defaults for AI empires, fallen empires, marauders, advanced empires, nebulae, crisis strength, hyperlane topology, and supported galaxy shapes.

## Scope

The mod is purely declarative — it contains no executable code, scripts, or compiled artefacts. It consists entirely of static configuration files consumed by the Stellaris engine at galaxy generation time.

### What the Mod Possesses

- Two mod descriptor files (`ui-galaxy-sizes.mod` and `descriptor.mod`) that declare mod identity, version, supported game version, and filesystem path
- An English localisation file mapping scenario keys to human-readable labels
- Six `setup_scenario` definition files in `map/setup_scenarios/` that define every galaxy generation parameter

### What the Mod Does Not Possess

- Stellaris game engine or galaxy generator implementation
- System name generation (delegated to the optional Universum Infinitum - Star Names mod for the 2,000-star Ludicrous scenario)
- Distribution platform logic (Steam Workshop, Nexus Mods, Paradox Mods, GitHub Releases)
- Player save games or runtime state

## Principal Capabilities

| Capability | Description |
|------------|-------------|
| Six galaxy sizes | Minuscule (100), Gigantic (1,200), Immense (1,400), Colossal (1,600), Titanic (1,800), Ludicrous (2,000) |
| Scaled defaults | Each size defines appropriate AI empire counts, fallen empires, marauders, advanced empires, crisis strength, nebulae, and hyperlane settings |
| Shape support | Every scenario supports elliptical, ring, bar, starburst, cartwheel, spoked, and 2-, 3-, 4-, or 6-arm spiral galaxy shapes |
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
| Universum Infinitum - Star Names | Optional mod | Supplies 2,000 system names for Ludicrous | Only for Ludicrous |

## System-Wide Invariants

- Every scenario file is a valid `setup_scenario` block parseable by Stellaris `v4.5.*`
- Each scenario defines a unique `name` key corresponding to its filename
- Priority values are unique and monotonically increasing (-1, 5, 6, 7, 8, 9)
- Every scenario defines a valid `supports_shape` list (Minuscule 4 shapes, others 10)
- Fallen empire and marauder counts remain within the vanilla type pools (6 and 3)
- Localisation keys correspond exactly to scenario `name` values
- The mod version in both descriptors corresponds to the release version (1.5.0)
- The supported game version in both descriptors corresponds (`v4.5.*`)
