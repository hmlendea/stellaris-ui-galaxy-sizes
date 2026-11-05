# API Reference Index

## Overview

**Not Applicable** — This mod has no programming API.

### Reason

The mod is purely declarative configuration consumed by the Stellaris engine. It exposes no:
- Functions
- Methods
- Classes
- Endpoints
- Events
- Hooks
- Interfaces

## Configuration Reference (Substitute)

Instead of an API reference, this mod's "interface" is its configuration schema:

### Scenario Parameters

Full parameter reference: [`data-model.md`](../data-model.md)

| Category | Parameters |
|----------|-----------|
| Identity | `name`, `priority` |
| Scale | `num_stars`, `radius` |
| Empires | `num_empires`, `advanced_empire_default`, `fallen_empire_range`, `fallen_empire_default`, `marauder_empire_range`, `marauder_empire_default` |
| Planets | `colonizable_planet_odds`, `primitive_odds`, `guaranteed_colonizable_pool` |
| Crisis | `crisis_strength`, `extra_crisis_strength` |
| Clusters | `cluster_count`, `cluster_radius`, `cluster_distance_from_core` |
| Hyperlanes | `max_hyperlane_distance`, `num_hyperlanes` |
| Partitions | `home_system_partitions`, `open_space_partitions` |
| Nebulae | `num_nebulas`, `nebula_size`, `nebula_min_dist` |
| Wormholes/Gateways | `num_wormhole_pairs`, `num_gateways` |
| Shapes | `supports_shape` (11 entries) |

### Descriptor Fields

| Field | Type | Value |
|-------|------|-------|
| `name` | string | `"Universum Infinitum: Galaxy Sizes"` |
| `version` | string | `"1.4.5"` |
| `path` | string | `"mod/ui-galaxy-sizes"` |
| `supported_version` | string | `"v4.5.*"` |
| `tags` | string array | `{"Gameplay" "Map"}` |

### Localisation Keys

| Key | Label |
|-----|-------|
| `ui_minuscule` | Minuscule |
| `ui_gigantic` | Gigantic |
| `ui_immense` | Immense |
| `ui_colossal` | Colossal |
| `ui_titanic` | Titanic |
| `ui_ludicrous` | Ludicrous |

## Usage Examples

See [`api-usage-examples.md`](../api-usage-examples.md) for:
- Reading scenario files
- Creating custom scenarios
- Parameter tuning
- Validation

## Related Documents

- [`data-model.md`](../data-model.md) — Complete parameter reference
- [`configuration.md`](../configuration.md) — Configuration guide
- [`invariants.md`](../invariants.md) — Parameter constraints
- [`api-usage-examples.md`](../api-usage-examples.md) — Usage examples