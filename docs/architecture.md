# Architecture

This document elaborates on the root `ARCHITECTURE.md` with implementation-level detail.

## Architectural Style

**Declarative data-driven Stellaris mod** — The mod consists entirely of static configuration files consumed by the Stellaris engine. There is no runtime logic, no scripting layer, no compiled artefacts, and no executable code. The engine possesses all execution; the mod only supplies parameters.

## System Context

```mermaid
flowchart LR
    Player[Player] -->|Selects galaxy size| Launcher[Stellaris Launcher]
    Launcher -->|Loads mod descriptors| Engine[Stellaris Engine]
    Engine -->|Reads setup_scenario| GalaxyGen[Galaxy Generator]
    GalaxyGen -->|Generates galaxy| Save[Save Game]
    StarNames[UI Star Names Mod] -.->|Supplies 2000 names| GalaxyGen
```

### External Boundaries

| Boundary | Description |
|----------|-------------|
| Stellaris Launcher | Loads `.mod` descriptors, validates `supported_version`, and registers the mod |
| Stellaris Engine | Parses `setup_scenario` blocks at galaxy generation time |
| Universum Infinitum - Star Names | Optional mod supplying an extended system name pool for 2,000-star galaxies |

## Component Catalogue

| Component | Type | Responsibility | Location |
|-----------|------|----------------|----------|
| Root Descriptor | Mod metadata | Declares mod identity, version, path, and supported version | `ui-galaxy-sizes.mod` |
| Local Descriptor | Mod metadata | Alternative descriptor for Paradox Mods and manual installation | `ui-galaxy-sizes/descriptor.mod` |
| Localisation | Display strings | Maps scenario keys to English labels | `localisation/engligh/ui-galaxy-sizes_l_english.yml` |
| Minuscule Scenario | Generation parameters | 100-star galaxy definition | `map/setup_scenarios/ui_minuscule.txt` |
| Gigantic Scenario | Generation parameters | 1,200-star galaxy definition | `map/setup_scenarios/ui_gigantic.txt` |
| Immense Scenario | Generation parameters | 1,400-star galaxy definition | `map/setup_scenarios/ui_immense.txt` |
| Colossal Scenario | Generation parameters | 1,600-star galaxy definition | `map/setup_scenarios/ui_colossal.txt` |
| Titanic Scenario | Generation parameters | 1,800-star galaxy definition | `map/setup_scenarios/ui_titanic.txt` |
| Ludicrous Scenario | Generation parameters | 2,000-star galaxy definition | `map/setup_scenarios/ui_ludicrous.txt` |

## Data Architecture

### Mod Descriptor Schema

Both `.mod` files share an identical structure:

```text
version="1.5.0"
name="Universum Infinitum - Galaxy Sizes"
path="mod/ui-galaxy-sizes"
tags={ "Galaxy Generation" }
supported_version="v4.5.*"
```

| Field | Purpose | Constraints |
|-------|---------|-------------|
| `version` | Mod version | Must correspond to the release tag |
| `name` | Display name | Human-readable |
| `path` | Mount path | Must be `mod/ui-galaxy-sizes` |
| `tags` | Workshop categories | Array of strings |
| `supported_version` | Game version constraint | Stellaris version pattern |

### Localisation Schema

```yaml
l_english:
  ui_minuscule:0 "Minuscule"
  ui_gigantic:0 "Gigantic"
  ui_immense:0 "Immense"
  ui_colossal:0 "Colossal"
  ui_titanic:0 "Titanic"
  ui_ludicrous:0 "Ludicrous"
```

| Key | Format | Purpose |
|-----|--------|---------|
| `ui_<size>` | `key:0 "Label"` | Display name in galaxy size selector |

### Scenario Schema (`setup_scenario`)

Every scenario shares an identical parameter structure:

```text
setup_scenario = {
    name = "ui_<size>"
    priority = <int>
    num_stars = <int>
    radius = <int>
    num_empires = { min = <int> max = <int> }
    num_empire_default = <int>
    fallen_empire_default = <int>
    fallen_empire_max = <int>
    marauder_empire_default = <int>
    marauder_empire_max = <int>
    nomad_empire_default = <int>
    nomad_empire_max = <int>
    advanced_empire_default = <int>
    colonizable_planet_odds = <float>
    primitive_odds = <float>
    crisis_strength = <float>
    extra_crisis_strength = { <int> <int> }

    cluster_count = {
        method = one_every_x_empire
        value = <int>
        max = <int>
    }
    cluster_radius = <int>
    cluster_distance_from_core = <int>

    max_hyperlane_distance = <int>

    home_system_partitions = {
        max_systems = <int>
        min_systems = <int>
        min_bridges = <int>
        max_bridges = <int>
        method = breadth_first
    }

    open_space_partitions = {
        max_systems = <int>
        min_systems = <int>
        min_bridges = <int>
        max_bridges = <int>
        method = depth_first
    }

    num_nebulas = <int>
    nebula_size = <int>
    nebula_min_dist = <int>

    num_wormhole_pairs = { min = <int> max = <int> }
    num_wormhole_pairs_default = <int>
    num_gateways = { min = <int> max = <int> }
    num_gateways_default = <int>
    num_hyperlanes = { min=<float> max=<float> }
    num_hyperlanes_default = <int>

    # Gigantic..Ludicrous declare all 10 shapes; Minuscule declares only
    # elliptical, spiral_2, ring and bar (the vanilla tiny set).
    supports_shape = elliptical
    supports_shape = spiral_2
    supports_shape = spiral_3
    supports_shape = spiral_4
    supports_shape = spiral_6
    supports_shape = ring
    supports_shape = bar
    supports_shape = starburst
    supports_shape = cartwheel
    supports_shape = spoked
}
```

### Parameter Scaling Throughout Scenarios

| Parameter | Minuscule | Gigantic | Immense | Colossal | Titanic | Ludicrous |
|-----------|-----------|----------|---------|----------|---------|-----------|
| `priority` | -1 | 5 | 6 | 7 | 8 | 9 |
| `num_stars` | 100 | 1200 | 1400 | 1600 | 1800 | 2000 |
| `radius` | 150 | 460 | 470 | 480 | 490 | 500 |
| `num_empires.min` | 0 | 0 | 0 | 0 | 0 | 0 |
| `num_empires.max` | 3 | 36 | 42 | 48 | 54 | 60 |
| `num_empire_default` | 2 | 18 | 21 | 24 | 27 | 30 |
| `fallen_empire_default` | 0 | 5 | 6 | 6 | 6 | 6 |
| `fallen_empire_max` | 1 | 6 | 6 | 6 | 6 | 6 |
| `marauder_empire_default` | 1 | 3 | 3 | 3 | 3 | 3 |
| `marauder_empire_max` | 1 | 3 | 3 | 3 | 3 | 3 |
| `nomad_empire_default` | 0 | 3 | 4 | 4 | 5 | 6 |
| `nomad_empire_max` | 2 | 12 | 14 | 16 | 18 | 20 |
| `advanced_empire_default` | 0 | 5 | 6 | 7 | 8 | 9 |
| `crisis_strength` | 0.375 | 1.75 | 2.0 | 2.25 | 2.5 | 2.75 |
| `cluster_count.max` | 2 | 6 | 6 | 6 | 6 | 6 |
| `cluster_radius` | 60 | 160 | 170 | 180 | 190 | 200 |
| `cluster_distance_from_core` | 90 | 300 | 300 | 300 | 300 | 300 |
| `num_nebulas` | 1 | 12 | 14 | 16 | 18 | 20 |
| `num_hyperlanes.max` | 3 | 3 | 3 | 3 | 3 | 3 |
| `supports_shape` count | 4 | 10 | 10 | 10 | 10 | 10 |

### Invariant Parameters (Identical Across Every Scenario)

| Parameter | Value |
|-----------|-------|
| `colonizable_planet_odds` | 1.0 |
| `primitive_odds` | 1.0 |
| `extra_crisis_strength` | { 10 25 } |
| `cluster_count.method` | one_every_x_empire |
| `cluster_count.value` | 1 |
| `max_hyperlane_distance` | 50 |
| `home_system_partitions` | Identical throughout every scenario |
| `open_space_partitions` | Identical throughout every scenario |
| `nebula_size` | 60 (40 on Minuscule) |
| `nebula_min_dist` | 100 (Minuscule), 200 (others) |
| `num_wormhole_pairs` | { min = 0 max = 5 } |
| `num_wormhole_pairs_default` | 1 |
| `num_gateways` | { min = 0 max = 5 } |
| `num_gateways_default` | 1 |
| `num_hyperlanes.min` | 0.5 |
| `num_hyperlanes_default` | 1 |
| `supports_shape` | Minuscule: vanilla tiny set (4); all others: all 10 shapes |

## Interfaces and Integrations

### Stellaris Launcher Interface

- **Input**: `.mod` descriptor files
- **Output**: Mod registration, version validation
- **Contract**: `supported_version` must match running game version pattern

### Stellaris Engine Interface

- **Input**: `setup_scenario` blocks from `map/setup_scenarios/`
- **Output**: Generated galaxy with applied parameters
- **Contract**: All `setup_scenario` parameters must be valid for Stellaris `v4.5.*`

### UI Star Names Integration

- **Trigger**: Player selects Ludicrous (2,000 stars)
- **Dependency**: UI Star Names mod must be active
- **Effect**: Galaxy generator draws from extended name pool
- **Failure mode**: Duplicate or generic system names if mod absent

## Key Flows

### Mod Registration Flow

```mermaid
sequenceDiagram
    participant Launcher
    participant Descriptor
    Launcher->>Descriptor: Reads ui-galaxy-sizes.mod
    Launcher->>Descriptor: Validates supported_version=v4.5.*
    Launcher->>Descriptor: Registers mod at path=mod/ui-galaxy-sizes
    Launcher-->>Player: Mod appears in playset
```

### Galaxy Generation Flow

```mermaid
sequenceDiagram
    participant Player
    participant Engine
    participant GalaxyGen
    Player->>Engine: Starts New Game
    Player->>Engine: Selects "Ludicrous" galaxy size
    Engine->>GalaxyGen: Loads ui_ludicrous.txt setup_scenario
    GalaxyGen->>GalaxyGen: Applies all 40+ parameters
    alt UI Star Names active
        GalaxyGen->>StarNames: Requests 2000 system names
        StarNames-->>GalaxyGen: Returns name pool
    else UI Star Names inactive
        GalaxyGen->>GalaxyGen: Uses the vanilla name pool (duplicates probable)
    end
    GalaxyGen-->>Player: Returns generated galaxy
```

## Cross-Cutting Concerns

### Version Compatibility

- Mod declares `supported_version="v4.5.*"` in both descriptors
- Scenario parameters must remain valid for all `4.5.x` patch versions
- Breaking changes in Stellaris galaxy generator would require mod update

### Localisation Completeness

- Every scenario `name` key must have a corresponding `l_english` record
- Absent localisation keys display as raw keys in the UI

### Parameter Consistency

- Every scenario must define an identical parameter set
- The `supports_shape` list must be identical throughout scenarios
- `priority` values must be unique and ordered

## Dependency Direction and Rules

```
Stellaris Engine (consumer)
    ↑ consumes
setup_scenario files (provider)
    ↑ references
Localisation keys (provider)
    ↑ declares
Mod descriptors (provider)
```

**Rules:**
- Engine never writes to mod files (read-only consumption)
- Mod files never reference engine internals
- Scenario parameters are engine-version-dependent
- Localisation is independent of engine version

## External Dependencies

| Dependency | Type | Version Constraint | Purpose |
|------------|------|-------------------|---------|
| Stellaris | Game engine | `v4.5.*` | Host platform |
| UI Star Names | Optional mod | Any compatible | Extended name pool for Ludicrous |

## Deployment and Operations

### Packaging

Distribution packages contain:
- `ui-galaxy-sizes.mod` (root descriptor)
- `ui-galaxy-sizes/` directory (entire mod content)

### Installation Paths

| Platform | Install Location |
|----------|------------------|
| Steam Workshop | `<Steam>/steamapps/workshop/content/281990/<mod_id>/` |
| Manual | `<User>/Documents/Paradox Interactive/Stellaris/mod/` |
| Paradox Mods | `<User>/Documents/Paradox Interactive/Stellaris/mod/` |

### Activation

Player includes the mod in a playset in the Stellaris launcher and activates it.

## Compatibility Contracts

| Contract | Description | Validation |
|----------|-------------|------------|
| `supported_version` | Mod works with Stellaris `4.5.x` | Launcher validates at load |
| Scenario schema | All `setup_scenario` parameters valid | Engine validates at generation |
| Localisation keys | Every scenario name localised | The engine displays raw keys if they are absent |
| Priority ordering | Unique, monotonic priorities | Engine sorts by priority |

## Testing and Verification

### Manual Verification

1. Install mod via target platform
2. Activate in launcher playset
3. Start New Game
4. Verify all six sizes appear in Galaxy Size selector
5. Select each size and verify generation completes
6. For Ludicrous: verify with and without UI Star Names

### Automated Validation (CI)

- Descriptor syntax validation
- Localisation key completeness check
- Scenario parameter schema validation
- Version consistency between descriptors

### Regression Testing

- Test each scenario on each supported Stellaris `4.5.x` patch
- Verify no duplicate system names in Ludicrous with UI Star Names
- Verify every declared galaxy shape generates correctly for each scenario
