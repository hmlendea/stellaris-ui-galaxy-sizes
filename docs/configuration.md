# Configuration

## Configuration Sources

The mod has no runtime configuration system. All configuration is static, declared in files that are read by the Stellaris engine at galaxy generation time.

### Configuration Files

| File | Purpose | Modifiable By |
|------|---------|---------------|
| `ui-galaxy-sizes.mod` | Root mod metadata | Mod author (version bump) |
| `ui-galaxy-sizes/descriptor.mod` | Local mod metadata | Mod author (version bump) |
| `localisation/engligh/ui-galaxy-sizes_l_english.yml` | Display names | Mod author / translators |
| `map/setup_scenarios/ui_*.txt` | Generation parameters | Mod author |

### Configuration Precedence

No precedence — each file serves a distinct purpose and is consumed by a different subsystem:

```
Launcher → .mod files → Mod registration
Engine → setup_scenario.txt → Galaxy generation
Localisation → *_l_english.yml → UI display
```

## Mod Descriptor Configuration

### Schema

```text
version="1.4.5"
name="Universum Infinitum: Galaxy Sizes"
path="mod/ui-galaxy-sizes"
tags={ "Galaxy Generation" }
supported_version="v4.5.*"
```

### Fields

| Field | Required | Description | Validation |
|-------|----------|-------------|------------|
| `version` | Yes | Mod version | Semantic version, matches release |
| `name` | Yes | Display name | Fixed string |
| `path` | Yes | Mount path | Must be `mod/ui-galaxy-sizes` |
| `tags` | Yes | Workshop categories | Array, at least one |
| `supported_version` | Yes | Game version pattern | Must match running Stellaris |

### Version Management

Both descriptor files must have identical `version` and `supported_version`. Update both when releasing.

## Localisation Configuration

### Schema

```yaml
l_english:
  ui_minuscule:0 "Minuscule"
  ui_gigantic:0 "Gigantic"
  ui_immense:0 "Immense"
  ui_colossal:0 "Colossal"
  ui_titanic:0 "Titanic"
  ui_ludicrous:0 "Ludicrous"
```

### Fields

| Key | Format | Description |
|-----|--------|-------------|
| `ui_<size>` | `key:0 "Label"` | Display name for scenario |

### Locale Support

Currently only English (`l_english`). To add languages:
1. Create `localisation/<locale>/` directory
2. Add `ui-galaxy-sizes_l_<locale>.yml`
3. Translate all six keys

### Directory Name Note

The directory is named `engligh` (typo). Should be `english` for consistency with Stellaris conventions, but engine accepts any directory name.

## Scenario Configuration

### Schema

Each `map/setup_scenarios/ui_<size>.txt` contains a `setup_scenario` block with ~40 parameters.

### Parameter Categories

#### Identity (Required)
| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | String | Scenario key, must match filename |
| `priority` | Integer | UI sort order, unique across all scenarios |

#### Scale (Required)
| Parameter | Type | Description |
|-----------|------|-------------|
| `num_stars` | Integer | Total star systems |
| `radius` | Integer | Galaxy radius |

#### Empires (Required)
| Parameter | Type | Description |
|-----------|------|-------------|
| `num_empires` | Range | Min/max AI empires |
| `num_empire_default` | Integer | Default AI empires |
| `fallen_empire_default` | Integer | Default fallen empires |
| `fallen_empire_max` | Integer | Max fallen empires |
| `marauder_empire_default` | Integer | Default marauders |
| `marauder_empire_max` | Integer | Max marauders |
| `advanced_empire_default` | Integer | Default advanced AI |

#### Planetary (Constant)
| Parameter | Type | Value |
|-----------|------|-------|
| `colonizable_planet_odds` | Float | 1.0 |
| `primitive_odds` | Float | 1.0 |

#### Crisis (Scaled)
| Parameter | Type | Description |
|-----------|------|-------------|
| `crisis_strength` | Float | Base multiplier |
| `extra_crisis_strength` | Tuple | Additional scaling |

#### Clusters (Scaled)
| Parameter | Type | Description |
|-----------|------|-------------|
| `cluster_count` | Block | Method, value, max |
| `cluster_radius` | Integer | Spatial radius |
| `cluster_distance_from_core` | Integer | Core distance |

#### Hyperlanes (Partially Scaled)
| Parameter | Type | Description |
|-----------|------|-------------|
| `max_hyperlane_distance` | Integer | Max length (50) |
| `num_hyperlanes` | Range | Density multiplier |
| `num_hyperlanes_default` | Integer | Default density |

#### Partitions (Constant)
| Parameter | Type | Description |
|-----------|------|-------------|
| `home_system_partitions` | Block | Breadth-first, 8-15 systems |
| `open_space_partitions` | Block | Depth-first, 4-10 systems |

#### Nebulae (Scaled)
| Parameter | Type | Description |
|-----------|------|-------------|
| `num_nebulas` | Integer | Count |
| `nebula_size` | Integer | Radius (60) |
| `nebula_min_dist` | Integer | Min separation |

#### Wormholes (Constant)
| Parameter | Type | Value |
|-----------|------|-------|
| `num_wormhole_pairs` | Range | 0-5 |
| `num_wormhole_pairs_default` | Integer | 1 |

#### Gateways (Constant)
| Parameter | Type | Value |
|-----------|------|-------|
| `num_gateways` | Range | 0-5 |
| `num_gateways_default` | Integer | 1 |

#### Shapes (Constant)
| Parameter | Type | Value |
|-----------|------|-------|
| `supports_shape` | String (×11) | All 11 shapes |

### Validation Rules

1. `name` must match filename (`ui_<size>.txt`)
2. `priority` unique and ordered (-1, 5, 6, 7, 8, 9)
3. `num_stars` > 0
4. `num_empires.min` ≤ `num_empires.max`
5. `num_empire_default` within `num_empires` range
6. All `*_default` within respective `*_max`
7. `crisis_strength` ≥ 0
8. `cluster_count.max` ≥ `cluster_count.value`
9. `num_hyperlanes.min` ≤ `num_hyperlanes.max`
10. All 11 `supports_shape` present and identical across scenarios

## Player-Configurable Settings

The following scenario parameters are exposed to players in the New Game screen:

| Setting | Scenario Parameter | Player Control |
|---------|-------------------|----------------|
| AI Empires | `num_empires` range, `num_empire_default` | Slider within min/max |
| Fallen Empires | `fallen_empire_max`, `fallen_empire_default` | Slider 0 to max |
| Marauders | `marauder_empire_max`, `marauder_empire_default` | Slider 0 to max |
| Advanced AI | `advanced_empire_default` | Slider 0 to max |
| Crisis Strength | `crisis_strength` | Multiplier slider |
| Hyperlane Density | `num_hyperlanes` range, `num_hyperlanes_default` | Slider within range |
| Wormhole Pairs | `num_wormhole_pairs` range, `num_wormhole_pairs_default` | Slider within range |
| Gateways | `num_gateways` range, `num_gateways_default` | Slider within range |
| Galaxy Shape | `supports_shape` | Dropdown of supported shapes |

Non-configurable (fixed per scenario): `num_stars`, `radius`, `cluster_*`, `partition_*`, `nebula_*`, `colonizable_planet_odds`, `primitive_odds`, `extra_crisis_strength`, `max_hyperlane_distance`.

## Environment-Specific Configuration

None — the mod has no environment-specific configuration. It is purely declarative and version-bound to Stellaris `v4.5.*`.

## Configuration Change Process

### Modifying Scenario Parameters

1. Edit `map/setup_scenarios/ui_<size>.txt`
2. Update `version` in both `.mod` files
3. Test generation for all 11 shapes
4. Verify parameter bounds
5. Release new version

### Adding Localisation

1. Create `localisation/<locale>/` directory
2. Add `ui-galaxy-sizes_l_<locale>.yml` with six keys
3. Update `version` in both `.mod` files
4. Test in-game language switch

### Updating Supported Version

1. Update `supported_version` in both `.mod` files (e.g., `v4.6.*`)
2. Verify all scenario parameters valid for new version
3. Update `version` in both `.mod` files
4. Test on target Stellaris version