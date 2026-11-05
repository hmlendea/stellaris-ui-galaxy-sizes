# Dependencies

## External Dependencies

### Stellaris Game Engine

| Aspect | Detail |
|--------|--------|
| **Name** | Stellaris |
| **Version Constraint** | `v4.5.*` (declared in both `.mod` files) |
| **Type** | Host platform / runtime |
| **Relationship** | Mod is consumed by engine; engine owns execution |
| **Interface** | `setup_scenario` parameter blocks, localisation system, mod descriptor parsing |
| **Compatibility** | Parameters validated against engine version at generation time |

**Version Compatibility Matrix:**

| Mod Version | Stellaris Version | Status |
|-------------|-------------------|--------|
| 1.4.5 | 4.5.x | ✅ Supported |
| 1.3.0 | 3.x | ⚠️ Legacy (Paradox Mods only) |
| < 1.3.0 | < 3.x | ❌ Unsupported |

**Breaking Change Triggers:**
- Stellaris changes `setup_scenario` schema
- Stellaris changes mod descriptor format
- Stellaris changes localisation format
- Stellaris removes/renames galaxy shapes
- Stellaris changes parameter validation bounds

### Universum Infinitum: Star Names

| Aspect | Detail |
|--------|--------|
| **Name** | Universum Infinitum: Star Names |
| **Repository** | https://github.com/hmlendea/stellaris-ui-star-names |
| **Type** | Optional companion mod |
| **Relationship** | Provides extended system name pool for 2,000-star galaxies |
| **Required For** | Ludicrous scenario (2,000 stars) only |
| **Interface** | Stellaris name pool system (implicit) |

**Dependency Flow:**
```
Player selects Ludicrous → Generator checks for UI Star Names →
  If active: Uses extended pool (2000+ unique names)
  If inactive: Uses vanilla pool (~1000 names, duplicates)
```

**Failure Mode:** Graceful degradation — game functions, but system names repeat.

## Internal Dependencies

### Mod Descriptor ↔ Mod Content

| Dependency | Direction | Description |
|------------|-----------|-------------|
| `ui-galaxy-sizes.mod` → `ui-galaxy-sizes/` | Mount | Root descriptor declares `path="mod/ui-galaxy-sizes"` |
| `ui-galaxy-sizes/descriptor.mod` → `ui-galaxy-sizes/` | Mount | Local descriptor same path |
| Both descriptors | Identity | Must have identical `version`, `name`, `path`, `supported_version` |

### Scenario Files ↔ Localisation

| Dependency | Direction | Description |
|------------|-----------|-------------|
| `setup_scenario.name` → `l_english` key | Lookup | Engine maps scenario `name` to localisation key |
| Six scenario files → Six localisation entries | 1:1 | Each scenario requires exactly one label |

**Consistency Rules:**
- Scenario `name` (e.g., `ui_ludicrous`) must match localisation key
- All six scenarios must have localisation entries
- Missing entry → Raw key displayed in UI

### Scenario Files ↔ Each Other

| Dependency | Direction | Description |
|------------|-----------|-------------|
| Priority ordering | Cross-file | All six `priority` values must be unique and monotonic |
| Shape support | Cross-file | All six must declare identical `supports_shape` lists |
| Parameter structure | Cross-file | All six must define same parameter set |

**Validation:**
- Priority collision → Undefined UI sort order
- Shape list divergence → Inconsistent shape availability
- Parameter omission → Engine uses defaults, logs warnings

## Dependency Graph

```mermaid
flowchart TD
    Stellaris[Stellaris Engine v4.5.*] -->|consumes| RootDesc[ui-galaxy-sizes.mod]
    Stellaris -->|consumes| LocalDesc[descriptor.mod]
    Stellaris -->|consumes| Scenarios[map/setup_scenarios/*.txt]
    Stellaris -->|consumes| Loc[localisation/engligh/ui-galaxy-sizes_l_english.yml]

    RootDesc -->|mounts| ModDir[ui-galaxy-sizes/]
    LocalDesc -->|mounts| ModDir
    ModDir -->|contains| Scenarios
    ModDir -->|contains| Loc

    Scenarios -.->|optional, Ludicrous only| StarNames[UI Star Names Mod]

    Scenarios -->|priority ordering| Scenarios
    Scenarios -->|shape list identity| Scenarios
    Scenarios -->|name ↔ localisation key| Loc
```

## Distribution Platform Dependencies

| Platform | Dependency | Notes |
|----------|------------|-------|
| Steam Workshop | Root descriptor (`ui-galaxy-sizes.mod`) | Primary distribution |
| GitHub Releases | Root descriptor + mod directory | Manual download |
| Nexus Mods | Root descriptor + mod directory | Manual download |
| Paradox Mods | Local descriptor (`descriptor.mod`) only | No root `.mod` |
| Manual Install | Local descriptor + mod directory | User places in mod folder |

## Version Synchronisation Requirements

All version-bearing files must be updated together:

| File | Version Field | Sync Rule |
|------|---------------|-----------|
| `ui-galaxy-sizes.mod` | `version` | Must match release tag |
| `ui-galaxy-sizes/descriptor.mod` | `version` | Must match root descriptor |
| GitHub Release | Tag | Must match descriptor version |
| Steam Workshop | Version | Must match descriptor version |
| Nexus Mods | Version | Must match descriptor version |

## Compatibility Testing Matrix

| Stellaris Version | Mod Version | UI Star Names | Test Status |
|-------------------|-------------|---------------|-------------|
| 4.5.0 | 1.4.5 | Any | ✅ Required |
| 4.5.1 | 1.4.5 | Any | ✅ Required |
| 4.5.x | 1.4.5 | Any | ✅ Required |
| 4.6.0 | 1.4.5 | Any | ❓ Untested |
| 3.x | 1.3.0 | Any | ⚠️ Legacy |

## Dependency Update Process

### Stellaris Version Update

1. Test mod on new Stellaris version (beta/PTS)
2. Verify all scenario parameters valid
3. Update `supported_version` in both `.mod` files
4. Update `version` in both `.mod` files
5. Release new mod version

### UI Star Names Update

1. No action required (backward compatible)
2. Verify Ludicrous still works with new version
3. Optional: Update README with new version reference

### Breaking Dependency Change

If Stellaris changes `setup_scenario` schema:
1. Analyse new schema
2. Update all six scenario files
3. Test all 11 shapes for all six sizes
4. Update `supported_version` and `version`
5. Release major version bump