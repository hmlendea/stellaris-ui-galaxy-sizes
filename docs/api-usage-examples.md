# API Usage Examples

## Overview

This mod has no programming API — it is purely declarative configuration consumed by the Stellaris engine. This document provides usage examples for the configuration "API" (scenario parameters) and integration patterns.

## Scenario Configuration Examples

### Reading a Scenario File

```clausewitz
# ui-galaxy-sizes/map/setup_scenarios/ui_ludicrous.txt
setup_scenario = {
    name = "ui_ludicrous"
    priority = 9
    num_stars = 2000
    radius = 500
    num_empires = { min = 0 max = 60 }
    num_empire_default = 30
    fallen_empire_default = 6
    fallen_empire_max = 6
    marauder_empire_default = 3
    marauder_empire_max = 3
    nomad_empire_default = 6
    nomad_empire_max = 20
    advanced_empire_default = 9
    crisis_strength = 2.75
    num_nebulas = 20
    cluster_radius = 200
    cluster_distance_from_core = 300
    num_hyperlanes = { min=0.5 max= 3 }
    num_hyperlanes_default = 1
    supports_shape = elliptical
    supports_shape = spiral_2
}
```

### Parameter Reference

| Parameter | Type | Ludicrous Value | Description |
|-----------|------|-----------------|-------------|
| `name` | string | `"ui_ludicrous"` | Unique identifier, matches filename |
| `priority` | int | `9` | UI sort order, higher = later in list |
| `num_stars` | int | `2000` | Target star count |
| `radius` | int | `500` | Galaxy radius (affects density) |
| `num_empires` | range | `{ 0 60 }` | Allowed AI empire count range |
| `num_empire_default` | int | `30` | Default AI empires in UI |
| `fallen_empire_default` | int | `6` | Default fallen empires |
| `fallen_empire_max` | int | `6` | Maximum fallen empires (vanilla type pool) |
| `marauder_empire_default` | int | `3` | Default marauder clans |
| `marauder_empire_max` | int | `3` | Maximum marauder clans (vanilla clan pool) |
| `nomad_empire_default` | int | `6` | Default nomad empires |
| `nomad_empire_max` | int | `20` | Maximum nomad empires |
| `advanced_empire_default` | int | `9` | Default advanced AI starts |
| `crisis_strength` | float | `2.75` | Crisis fleet power multiplier |
| `num_nebulas` | int | `20` | Number of nebulae to place |
| `cluster_radius` | int | `200` | Cluster radius |
| `cluster_distance_from_core` | int | `300` | Cluster distance from galactic core |
| `supports_shape` | string | 10 entries | Supported galaxy shapes |

## Creating a Custom Scenario (Fork Example)

### 1. Copy Existing Scenario

```bash
cp ui-galaxy-sizes/map/setup_scenarios/ui_gigantic.txt \
   ui-galaxy-sizes/map/setup_scenarios/ui_custom.txt
```

### 2. Modify Parameters

```clausewitz
setup_scenario = {
    name = "ui_custom"
    priority = 3          # Between Minuscule (-1) and Gigantic (5)
    num_stars = 800       # Custom star count
    radius = 450          # Keep below the 500 engine limit
    num_empires = { min = 0 max = 24 }
    num_empire_default = 12
    fallen_empire_default = 3
    fallen_empire_max = 6
    marauder_empire_default = 3
    marauder_empire_max = 3
    nomad_empire_default = 3
    nomad_empire_max = 12
    advanced_empire_default = 3
    crisis_strength = 1.25   # Vanilla curve: 0.25 + 0.00125 x stars
    num_nebulas = 8
    cluster_radius = 150
    cluster_distance_from_core = 300
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

### 3. Add Localisation

```yaml
# ui-galaxy-sizes/localisation/engligh/ui-galaxy-sizes_l_english.yml
l_english:
  ui_custom:0 "Custom"
```

### 4. Validate

```bash
./scripts/validate-all.sh
```

### 5. Test In-Game

1. Install modified mod
2. New Game → Galaxy Size → "Custom"
3. Generate and verify

## Integration Examples

### Mod Load Order (for UI Star Names)

```text
# launcher-v2.sqlite playset order (first = highest priority)
1. UI Star Names (provides names)
2. Universum Infinitum: Galaxy Sizes (consumes names)
3. Other mods...
```

### Playset Configuration

```json
// Example playset with mod enabled
{
  "name": "My Playset",
  "mods": [
    "ugc_123456789",  // UI Star Names (Steam Workshop ID)
    "ugc_987654321",  // Universum Infinitum: Galaxy Sizes
    "mod/ui-galaxy-sizes"  // Manual install path
  ],
  "game_version": "4.5.2"
}
```

### Multiplayer Host Setup

```bash
# All players must have identical mod versions
# Host generates galaxy
# Verify in launcher: all players show same mod version
```

## Parameter Tuning Examples

### Scaling Formulas (Derived from Vanilla)

```text
num_empire_default  = num_stars / 66.67       (vanilla ratio, rounded)
num_empires.max     = 2 x num_empire_default
crisis_strength         = 0.25 + 0.00125 x num_stars
num_nebulas             = num_stars / 100
cluster_radius          = radius - 300        (for radius >= 440)
```

Example: 800 stars -> 12 default empires (max 24), crisis 1.25, 8 nebulae.

### Conservative Scaling (For New Sizes)

```clausewitz
# For a 900-star size (between Minuscule and Gigantic)
num_stars = 900
radius = 455
num_empires = { min = 0 max = 27 }
num_empire_default = 13
fallen_empire_default = 5
fallen_empire_max = 6
marauder_empire_default = 3
marauder_empire_max = 3
nomad_empire_default = 3
nomad_empire_max = 13
advanced_empire_default = 5
crisis_strength = 1.375
num_nebulas = 9
```

### Aggressive Scaling (For Performance Testing)

```clausewitz
# Minimal empires for maximum stars (stress test)
num_stars = 2000
num_empires = { min = 0 max = 60 }
num_empire_default = 1
advanced_empire_default = 0
fallen_empire_default = 0
fallen_empire_max = 1
marauder_empire_default = 0
marauder_empire_max = 1
nomad_empire_default = 0
nomad_empire_max = 2
crisis_strength = 0.5
num_nebulas = 5
```

## Validation Examples

### Running Validation Scripts

```bash
# All validations
./scripts/validate-all.sh

# Individual
./scripts/validate-descriptors.sh
./scripts/validate-scenarios.sh
./scripts/validate-localisation.sh
```

### Expected Output (Success)

```
✓ Descriptor validation passed
✓ Scenario validation passed (6/6 files)
✓ Localisation validation passed (6/6 keys)
✓ All checks passed
```

### Expected Output (Failure)

```
✗ Scenario validation failed: ui_custom.txt
  - Missing required parameter: crisis_strength
  - supports_shape count: 10 (expected 11)
  - priority 3 conflicts with existing (use unique value)
```

## Documentation Generation Examples

### Extracting Parameter Table

```bash
# Generate markdown table from scenario files
./scripts/extract-params.sh > docs/parameter-reference.md
```

### Output Format

| Parameter | Minuscule | Gigantic | Immense | Colossal | Titanic | Ludicrous |
|-----------|-----------|----------|---------|----------|---------|-----------|
| num_stars | 100 | 1200 | 1400 | 1600 | 1800 | 2000 |
| radius | 150 | 460 | 470 | 480 | 490 | 500 |
| ... | ... | ... | ... | ... | ... | ... |

## CI/CD Integration Examples

### GitHub Actions Workflow Snippet

```yaml
# .github/workflows/github-release.yml
- name: Update the mod version
  run: |
    MOD_VERSION="${VERSION#v}"
    sed -i "s/^version=.*/version=\"${MOD_VERSION}\"/" ui-galaxy-sizes.mod ui-galaxy-sizes/descriptor.mod

- name: Build the release archive
  run: |
    zip -r "ui-galaxy-sizes_${VERSION}.zip" "ui-galaxy-sizes/" "ui-galaxy-sizes.mod"
```

### Pre-Release Check

```bash
# Verify descriptor versions match the tag before publishing
grep '^version=' ui-galaxy-sizes.mod ui-galaxy-sizes/descriptor.mod
git describe --tags --abbrev=0
```

## Not Applicable: Traditional API Examples

Since this mod has no runtime API, the following are **not applicable**:

- REST API calls
- GraphQL queries
- SDK usage
- Function/method calls
- Event handlers
- Callbacks
- Webhooks
- Authentication flows
- Rate limiting
- Pagination
- Error response handling

## Related Documents

- `data-model.md` — Complete parameter reference
- `change-guide.md` — Process for adding scenarios
- `testing.md` — Validation and testing procedures
- `invariants.md` — Constraints that must hold