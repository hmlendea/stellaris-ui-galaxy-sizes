# State and Persistence

## State Model

**Not Applicable** — This mod has no runtime state.

### Reason

The mod is purely declarative configuration. It contains:
- No variables
- No mutable state
- No session data
- No user preferences
- No cache
- No database

All "state" exists in the Stellaris engine's galaxy generation process, which is outside this mod's scope.

## Persistence

### Mod Files (Source of Truth)

The mod's persistent artefacts are its source files:

| File | Persistence Type | Lifetime |
|------|------------------|----------|
| `ui-galaxy-sizes.mod` | File system | Until deleted |
| `ui-galaxy-sizes/descriptor.mod` | File system | Until deleted |
| `ui-galaxy-sizes/map/setup_scenarios/ui_*.txt` | File system | Until deleted |
| `ui-galaxy-sizes/localisation/engligh/ui-galaxy-sizes_l_english.yml` | File system | Until deleted |
| `ui-galaxy-sizes/thumbnail.png` | File system | Until deleted |

**Persistence Mechanism:** File system (user's mod directory)

**Backup:** User responsibility (Steam Workshop auto-updates, manual installs user-managed)

### Engine-Generated State (Not Mod State)

When a player generates a galaxy, the engine creates persistent state:

| State | Location | Mod Influence |
|-------|----------|---------------|
| Galaxy map (stars, hyperlanes) | Save game | Parameters define generation |
| Empire placement | Save game | Parameters define counts/limits |
| System names | Save game | Parameters define count, UI Star Names extends pool |
| Crisis fleets | Save game | `crisis_strength` parameter |
| Nebulae | Save game | `num_nebulae` parameter |

**Mod's Role:** Provides generation parameters only. Does not read or write save games.

## Configuration Persistence

### Player Settings (Engine-Managed)

Players adjust settings in game setup UI. Engine persists per-playthrough:

| Setting | Persisted In | Mod Default |
|---------|--------------|-------------|
| Selected scenario | Save game / playset | N/A |
| Galaxy shape | Save game | N/A |
| AI Empire count | Save game | `num_empires_default` |
| Advanced AI starts | Save game | `num_advanced_empires` |
| Fallen Empires | Save game | `num_fallen_empires_default` |
| Crisis strength | Save game | `crisis_strength` |

**Mod's Role:** Provides defaults and maximums via scenario parameters.

### Playset Persistence (Launcher-Managed)

Launcher persists mod enablement in playset:

| Data | Location | Mod Role |
|------|----------|----------|
| Mod enabled/disabled | `launcher-v2.sqlite` | Passive (valid descriptor) |
| Mod version | `launcher-v2.sqlite` | From descriptor `version` |
| Load order | `launcher-v2.sqlite` | User-controlled |

## Version Persistence

### Descriptor Version

```clausewitz
version="1.4.5"
```

Persisted in:
- Both `.mod` files
- Git tags
- GitHub Releases
- Steam Workshop version
- Nexus Mods version
- Paradox Mods version

### Supported Version

```clausewitz
supported_version="v4.5.*"
```

Persisted in descriptors. Engine uses for compatibility check.

## No Mod-Managed Persistence

The mod does not:
- Write files at runtime
- Read files at runtime (engine reads at generation)
- Maintain caches
- Store user preferences
- Track usage statistics
- Sync state across devices
- Migrate data between versions

## Save Game Compatibility

### Breaking Changes

Mod changes that break existing saves:

| Change | Effect |
|--------|--------|
| Rename scenario `name` | Save references missing scenario |
| Remove scenario file | Save references missing scenario |
| Significantly change `num_stars` | Galaxy topology mismatch |
| Change `supported_version` | Launcher may disable mod |

### Non-Breaking Changes

| Change | Effect |
|--------|--------|
| Adjust `crisis_strength` | Next generation uses new value |
| Adjust `num_nebulae` | Next generation uses new value |
| Add localisation | UI displays new labels |
| Fix typo in scenario | No effect (engine ignores comments) |

### Version Pinning

Descriptor `version` helps launcher detect changes. Players see version in playset.

## State Diagram

```
┌─────────────┐     Install      ┌──────────────┐
│  Mod Files  │ ──────────────▶ │ Mod Directory │
│  (Git/Zip)  │                  │ (Persistent)  │
└─────────────┘                  └──────┬───────┘
                                        │
                                        │ Launcher reads
                                        ▼
                               ┌──────────────────┐
                               │ Launcher Database │
                               │ (launcher-v2.sqlite)│
                               └────────┬─────────┘
                                        │
                                        │ Player enables in playset
                                        ▼
                               ┌──────────────────┐
                               │ Stellaris Engine  │
                               │ (Reads mod files) │
                               └────────┬─────────┘
                                        │
                                        │ Player generates galaxy
                                        ▼
                               ┌──────────────────┐
                               │ Save Game         │
                               │ (Engine state)    │
                               └──────────────────┘
```

## Related Documents

- `data-model.md` — Parameter definitions
- `change-guide.md` — Versioning and breaking changes
- `architecture.md` — System context
- `integrations.md` — Save game integration