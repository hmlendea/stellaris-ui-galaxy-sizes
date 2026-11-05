# Components

## Presentation Layer

### Galaxy Size Selector Integration

The mod integrates with Stellaris' New Game screen by providing six additional entries in the Galaxy Size dropdown. The Stellaris engine reads all `setup_scenario` blocks from installed mods and presents them alongside vanilla sizes.

**Component:** `map/setup_scenarios/*.txt` (six files)

**Responsibilities:**
- Declare scenario identity (`name`, `priority`)
- Define all generation parameters consumed by the galaxy generator
- Declare supported galaxy shapes via `supports_shape`

**Dependencies:**
- Stellaris engine (consumer)
- Localisation system (for display names)

**Data Flow:**
```
setup_scenario.txt → Engine parser → Galaxy Size UI dropdown → Player selection → Galaxy generator
```

**Configuration:**
- `priority` determines sort order in dropdown (lower = earlier)
- `name` must match localisation key

**Failure Modes:**
- Invalid parameter → Engine logs error, scenario may not appear
- Missing localisation → Raw key displayed (e.g., "ui_ludicrous")
- Duplicate priority → Undefined sort order

### Localisation Provider

**Component:** `localisation/engligh/ui-galaxy-sizes_l_english.yml`

**Responsibilities:**
- Map scenario keys to English display labels
- Follow Stellaris localisation format (`key:0 "Label"`)

**Dependencies:**
- Stellaris localisation system (consumer)

**Data Flow:**
```
YAML file → Localisation loader → UI string table → Galaxy Size dropdown labels
```

**Configuration:**
- Locale directory name: `engligh` (note: typo in directory name, should be `english`)
- File naming: `*_l_english.yml`

**Failure Modes:**
- Missing file → All scenario names show as raw keys
- Malformed YAML → Localisation load failure
- Key mismatch → Specific scenario shows raw key

## Host and Composition

### Mod Descriptor (Root)

**Component:** `ui-galaxy-sizes.mod`

**Responsibilities:**
- Declare mod identity for Steam Workshop, GitHub Releases, Nexus Mods
- Specify mount path (`mod/ui-galaxy-sizes`)
- Declare supported game version (`v4.5.*`)

**Dependencies:**
- Stellaris Launcher (consumer)

**Data Flow:**
```
.mod file → Launcher mod loader → Mod registry → Content mounted at path
```

**Configuration:**
- `path="mod/ui-galaxy-sizes"` must match directory name
- `supported_version` must match target Stellaris version

### Mod Descriptor (Local)

**Component:** `ui-galaxy-sizes/descriptor.mod`

**Responsibilities:**
- Declare mod identity for Paradox Mods and manual installation
- Identical content to root descriptor

**Dependencies:**
- Stellaris Launcher (consumer)

**Data Flow:**
```
.descriptor.mod → Launcher mod loader → Mod registry → Content mounted at path
```

### Mod Content Directory

**Component:** `ui-galaxy-sizes/` (directory)

**Responsibilities:**
- Container for all mod content files
- Mounted by launcher at `mod/ui-galaxy-sizes`

**Structure:**
```
ui-galaxy-sizes/
├── descriptor.mod
├── localisation/engligh/ui-galaxy-sizes_l_english.yml
├── map/setup_scenarios/*.txt (6 files)
└── thumbnail.png
```

## Application Services

### Galaxy Generation Parameter Provider

The six `setup_scenario` files collectively form a parameter service for the Stellaris galaxy generator. Each scenario is a complete, self-contained parameter set.

**Component:** `map/setup_scenarios/ui_minuscule.txt` through `ui_ludicrous.txt`

**Responsibilities:**
- Provide all 40+ generation parameters for a specific galaxy size
- Scale parameters appropriately for the star count
- Declare shape compatibility

**Parameters by Category:**

| Category | Parameters |
|----------|------------|
| Identity | `name`, `priority` |
| Scale | `num_stars`, `radius` |
| Empires | `num_empires`, `num_empire_default`, `fallen_empire_default`, `fallen_empire_max`, `marauder_empire_default`, `marauder_empire_max`, `advanced_empire_default` |
| Planets | `colonizable_planet_odds`, `primitive_odds` |
| Crisis | `crisis_strength`, `extra_crisis_strength` |
| Clusters | `cluster_count`, `cluster_radius`, `cluster_distance_from_core` |
| Hyperlanes | `max_hyperlane_distance`, `num_hyperlanes`, `num_hyperlanes_default` |
| Partitions | `home_system_partitions`, `open_space_partitions` |
| Nebulae | `num_nebulas`, `nebula_size`, `nebula_min_dist` |
| Wormholes | `num_wormhole_pairs`, `num_wormhole_pairs_default` |
| Gateways | `num_gateways`, `num_gateways_default` |
| Shapes | `supports_shape` (×11) |

**Dependencies:**
- Stellaris Galaxy Generator (consumer)

**Data Flow:**
```
Player selects size → Engine loads scenario → Generator applies all parameters → Galaxy generated
```

**Invariants:**
- All scenarios define identical parameter set
- `supports_shape` list identical across all scenarios
- `priority` values unique and ordered

## Browser State and Localisation

Not applicable — this is a Stellaris mod, not a web application. The "browser" concept maps to the Stellaris launcher and in-game UI, which are native components.

## Integration Models

### Stellaris Engine Integration

**Interface:** `setup_scenario` consumption

**Contract:**
- Engine reads all `map/setup_scenarios/*.txt` from active mods
- Each file must contain a valid `setup_scenario` block
- Parameters must be valid for declared `supported_version`

**Data Exchanged:**
- Input: Scenario parameters (40+ fields)
- Output: Generated galaxy (internal engine state)

### UI Star Names Integration

**Interface:** Optional dependency for Ludicrous scenario

**Contract:**
- If UI Star Names mod active, generator uses extended name pool
- If inactive, generator uses vanilla name pool (insufficient for 2000 stars)

**Data Exchanged:**
- Input: Name pool (2000+ unique system names)
- Output: Named star systems

**Failure Mode:**
- Duplicate/generic names if dependency missing