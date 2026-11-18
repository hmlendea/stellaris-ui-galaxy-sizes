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

**Persistence Mechanism:** File system (the user's mod directory)

**Backup:** The responsibility of the user (Steam Workshop updates automatically; manual installations are user-managed)

### Engine-Generated State (Not Mod State)

When a player generates a galaxy, the engine creates persistent state:

| State | Location | Mod Influence |
|-------|----------|---------------|
| Galaxy map (stars, hyperlanes) | Save game | Parameters define generation |
| Empire placement | Save game | Parameters define counts and limits |
| System names | Save game | Parameters define the count; UI Star Names extends the pool |
| Crisis fleets | Save game | `crisis_strength` parameter |
| Nebulae | Save game | `num_nebulas` parameter |

**Mod's Role:** Supplies generation parameters only. It does not read or write save games.

## Configuration Persistence

### Player Settings (Engine-Managed)

Players adjust settings in game setup UI. Engine persists per-playthrough:

| Setting | Persisted In | Mod Default |
|---------|--------------|-------------|
| Selected scenario | Save game or playset | Not applicable |
| Galaxy shape | Save game | Not applicable |
| AI Empire count | Save game | `num_empire_default` |
| Advanced AI starts | Save game | `advanced_empire_default` |
| Fallen Empires | Save game | `fallen_empire_default` |
| Crisis strength | Save game | `crisis_strength` |

**Mod's Role:** Supplies defaults and maximums through the scenario parameters.

### Playset Persistence (Launcher-Managed)

Launcher persists mod enablement in playset:

| Data | Location | Mod Role |
|------|----------|----------|
| Mod activation state | `launcher-v2.sqlite` | Passive (valid descriptor) |
| Mod version | `launcher-v2.sqlite` | From the descriptor `version` |
| Load order | `launcher-v2.sqlite` | User-controlled |

## Version Persistence

### Descriptor Version

```clausewitz
version="1.5.0"
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
- Read files at runtime (the engine reads them at generation)
- Maintain caches
- Store user preferences
- Track usage statistics
- Synchronise state throughout devices
- Migrate data between versions

## Save Game Compatibility

### Breaking Changes

Mod changes that break existing saves:

| Change | Effect |
|--------|--------|
| Renaming a scenario `name` | The save references an absent scenario |
| Extracting a scenario file | The save references an absent scenario |
| Substantially changing `num_stars` | Galaxy topology mismatch |
| Changing `supported_version` | The launcher may deactivate the mod |

### Non-Breaking Changes

| Change | Effect |
|--------|--------|
| Adjusting `crisis_strength` | The next generation uses the new value |
| Adjusting `num_nebulas` | The next generation uses the new value |
| Including localisation | The UI displays new labels |
| Correcting a scenario typo | No effect (the engine ignores comments) |

### Version Pinning

The descriptor `version` assists the launcher in detecting changes. Players observe the version in the playset.

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
