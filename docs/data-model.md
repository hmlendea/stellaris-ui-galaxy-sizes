# Data Model

## Domain Entities

### Mod Descriptor

Represents the mod identity and metadata as declared in `.mod` files.

| Field | Type | Description | Constraints |
|-------|------|-------------|-------------|
| `version` | String | Mod version (semantic) | Must match release tag (e.g., "1.4.5") |
| `name` | String | Display name | "Universum Infinitum: Galaxy Sizes" |
| `path` | String | Mount path in Stellaris mod directory | Must be "mod/ui-galaxy-sizes" |
| `tags` | Array[String] | Workshop categories | ["Galaxy Generation"] |
| `supported_version` | String | Game version pattern | "v4.5.*" |

**Instances:** Two — root descriptor (`ui-galaxy-sizes.mod`) and local descriptor (`ui-galaxy-sizes/descriptor.mod`). Both must be identical.

### Localisation Entry

Maps a scenario key to a human-readable label for a specific locale.

| Field | Type | Description | Constraints |
|-------|------|-------------|-------------|
| `key` | String | Scenario identifier | Format: `ui_<size>` (e.g., `ui_ludicrous`) |
| `locale` | String | Language code | `l_english` |
| `label` | String | Display name | Human-readable (e.g., "Ludicrous") |

**Instances:** Six entries in `ui-galaxy-sizes_l_english.yml`, one per scenario.

### Galaxy Scenario (`setup_scenario`)

Defines all parameters for a single galaxy size option. This is the core domain entity consumed by the Stellaris galaxy generator.

#### Identity

| Field | Type | Description | Constraints |
|-------|------|-------------|-------------|
| `name` | String | Scenario key | Must match filename: `ui_<size>` |
| `priority` | Integer | Sort order in UI | Unique, monotonic: -1, 5, 6, 7, 8, 9 |

#### Star System Parameters

| Field | Type | Description | Scaling |
|-------|------|-------------|---------|
| `num_stars` | Integer | Total star systems in galaxy | 100 → 2000 |
| `radius` | Integer | Galaxy radius (arbitrary units) | 100 → 500 |

#### Empire Parameters

| Field | Type | Description | Scaling |
|-------|------|-------------|---------|
| `num_empires` | Range(min, max) | Allowed AI empire count | 1-3 → 1-60 |
| `num_empire_default` | Integer | Default AI empires | 3 → 30 |
| `fallen_empire_default` | Integer | Default fallen empires | 0 → 9 |
| `fallen_empire_max` | Integer | Maximum fallen empires | 1 → 10 |
| `marauder_empire_default` | Integer | Default marauder empires | 1 → 5 |
| `marauder_empire_max` | Integer | Maximum marauder empires | 1 → 6 |
| `advanced_empire_default` | Integer | Default advanced AI empires | 0 → 9 |

#### Planetary Parameters

| Field | Type | Description | Value |
|-------|------|-------------|-------|
| `colonizable_planet_odds` | Float | Habitable planet probability | 1.0 (all) |
| `primitive_odds` | Float | Primitive civilisation probability | 1.0 (all) |

#### Crisis Parameters

| Field | Type | Description | Scaling |
|-------|------|-------------|---------|
| `crisis_strength` | Float | Base crisis multiplier | 0.25 → 2.75 |
| `extra_crisis_strength` | Tuple(int, int) | Additional crisis scaling | {10, 25} (all) |

#### Cluster Parameters

| Field | Type | Description | Scaling |
|-------|------|-------------|---------|
| `cluster_count.method` | Enum | Cluster distribution algorithm | `one_every_x_empire` (all) |
| `cluster_count.value` | Integer | Empires per cluster | 1 (all) |
| `cluster_count.max` | Integer | Maximum clusters | 2 → 6 |
| `cluster_radius` | Integer | Cluster spatial radius | 70 → 200 |
| `cluster_distance_from_core` | Integer | Distance from galactic core | 70 (Minuscule), 300 (others) |

#### Hyperlane Parameters

| Field | Type | Description | Value |
|-------|------|-------------|-------|
| `max_hyperlane_distance` | Integer | Maximum hyperlane length | 50 (all) |

#### Partition Parameters

**Home System Partitions** (identical across all scenarios):

| Field | Type | Value |
|-------|------|-------|
| `max_systems` | Integer | 15 |
| `min_systems` | Integer | 8 |
| `min_bridges` | Integer | 2 |
| `max_bridges` | Integer | 4 |
| `method` | Enum | `breadth_first` |

**Open Space Partitions** (identical across all scenarios):

| Field | Type | Value |
|-------|------|-------|
| `max_systems` | Integer | 10 |
| `min_systems` | Integer | 4 |
| `min_bridges` | Integer | 2 |
| `max_bridges` | Integer | 4 |
| `method` | Enum | `depth_first` |

#### Nebula Parameters

| Field | Type | Description | Scaling |
|-------|------|-------------|---------|
| `num_nebulas` | Integer | Number of nebulae | 1 → 20 |
| `nebula_size` | Integer | Nebula radius | 60 (all) |
| `nebula_min_dist` | Integer | Minimum distance between nebulae | 50 (Minuscule), 200 (others) |

#### Wormhole Parameters

| Field | Type | Description | Value |
|-------|------|-------------|-------|
| `num_wormhole_pairs` | Range(min, max) | Wormhole pair count | {0, 5} (all) |
| `num_wormhole_pairs_default` | Integer | Default wormhole pairs | 1 (all) |

#### Gateway Parameters

| Field | Type | Description | Value |
|-------|------|-------------|-------|
| `num_gateways` | Range(min, max) | Gateway count | {0, 5} (all) |
| `num_gateways_default` | Integer | Default gateways | 1 (all) |

#### Hyperlane Density Parameters

| Field | Type | Description | Scaling |
|-------|------|-------------|---------|
| `num_hyperlanes` | Range(min, max) | Hyperlane density multiplier | {0.5, 3} → {0.5, 5} |
| `num_hyperlanes_default` | Integer | Default hyperlane density | 1 (all) |

#### Shape Support

All scenarios support the same 11 galaxy shapes:

| Shape | Description |
|-------|-------------|
| `elliptical` | Elliptical galaxy |
| `spiral_2` | 2-arm spiral |
| `spiral_3` | 3-arm spiral |
| `spiral_4` | 4-arm spiral |
| `spiral_6` | 6-arm spiral |
| `ring` | Ring galaxy |
| `bar` | Barred spiral |
| `starburst` | Starburst galaxy |
| `cartwheel` | Cartwheel galaxy |
| `spoked` | Spoked wheel galaxy |

## Relationships

```
Mod Descriptor (1) ── declares ──> (6) Galaxy Scenario
Localisation Entry (1) ── labels ──> (1) Galaxy Scenario
Galaxy Scenario (1) ── requires ──> (0..1) UI Star Names Mod
```

### Cardinality

| Relationship | Cardinality | Notes |
|--------------|-------------|-------|
| Descriptor → Scenario | 1:6 | Two descriptors, same six scenarios |
| Localisation → Scenario | 1:1 | Each scenario has exactly one English label |
| Scenario → UI Star Names | 0:1 | Only Ludicrous requires it |

## Persistence

All entities are persisted as static files in the repository:

| Entity | File | Format |
|--------|------|--------|
| Mod Descriptor | `ui-galaxy-sizes.mod`, `ui-galaxy-sizes/descriptor.mod` | Stellaris `.mod` (key-value) |
| Localisation Entry | `localisation/engligh/ui-galaxy-sizes_l_english.yml` | YAML |
| Galaxy Scenario | `map/setup_scenarios/ui_<size>.txt` | Stellaris `setup_scenario` (Clausewitz script) |

## Validation Rules

### Cross-Entity Consistency

1. **Descriptor version match**: Both `.mod` files must have identical `version`
2. **Descriptor path match**: Both must declare `path="mod/ui-galaxy-sizes"`
3. **Supported version match**: Both must declare `supported_version="v4.5.*"`
4. **Scenario name match**: Each scenario `name` must match its filename (`ui_<size>.txt`)
5. **Localisation key match**: Each `l_english` key must match a scenario `name`
6. **Priority uniqueness**: All six `priority` values must be distinct
7. **Priority ordering**: Priorities must increase with galaxy size (-1, 5, 6, 7, 8, 9)
8. **Shape list identity**: All scenarios must declare identical `supports_shape` lists

### Parameter Bounds

| Parameter | Min | Max | Notes |
|-----------|-----|-----|-------|
| `num_stars` | 100 | 2000 | Engine may have hard limits |
| `radius` | 100 | 500 | Proportional to star count |
| `num_empires.max` | 3 | 60 | UI slider limits |
| `crisis_strength` | 0.25 | 2.75 | Multiplier, not absolute |
| `cluster_count.max` | 2 | 6 | Minuscule capped at 2 |
| `num_nebulas` | 1 | 20 | Proportional to star count |
| `num_hyperlanes.max` | 3 | 5 | Minuscule capped at 3 |

## Schema Evolution

### Adding a New Scenario

1. Create `map/setup_scenarios/ui_<newsize>.txt` with valid `setup_scenario`
2. Add localisation entry to `ui-galaxy-sizes_l_english.yml`
3. Assign unique `priority` maintaining sort order
4. Update both `.mod` files' `version`
5. Test generation for all shapes

### Modifying Existing Scenario

1. Edit `map/setup_scenarios/ui_<size>.txt`
2. Update `version` in both descriptors
3. Verify parameter bounds
4. Test generation for all shapes

### Removing a Scenario

1. Delete `map/setup_scenarios/ui_<size>.txt`
2. Remove localisation entry
3. Update `version` in both descriptors
4. Verify remaining priorities still unique and ordered