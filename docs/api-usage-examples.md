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
    num_empires = 60
    num_empires_default = 30
    num_advanced_empires = 15
    num_fallen_empires = 9
    num_fallen_empires_default = 9
    crisis_strength = 2.75
    num_nebulae = 20
    num_systems_per_empire = 4
    min_distance_between_empires = 3
    guaranteed_habitable = 2
    supports_shape = "elliptical"
    supports_shape = "spiral_2"
    supports_shape = "spiral_3"
    supports_shape = "spiral_4"
    supports_shape = "spiral_6"
    supports_shape = "ring"
    supports_shape = "bar"
    supports_shape = "starburst"
    supports_shape = "cartwheel"
    supports_shape = "spoked"
}
```

### Parameter Reference

| Parameter | Type | Ludicrous Value | Description |
|-----------|------|-----------------|-------------|
| `name` | string | `"ui_ludicrous"` | Unique identifier, matches filename |
| `priority` | int | `9` | UI sort order, higher = later in list |
| `num_stars` | int | `2000` | Target star count |
| `radius` | int | `500` | Galaxy radius (affects density) |
| `num_empires` | int | `60` | Maximum AI empires player can select |
| `num_empires_default` | int | `30` | Default AI empires in UI |
| `num_advanced_empires` | int | `15` | Maximum advanced AI starts |
| `num_fallen_empires` | int | `9` | Maximum fallen empires |
| `num_fallen_empires_default` | int | `9` | Default fallen empires |
| `crisis_strength` | float | `2.75` | Crisis fleet power multiplier |
| `num_nebulae` | int | `20` | Number of nebulae to place |
| `num_systems_per_empire` | int | `4` | Initial systems per empire |
| `min_distance_between_empires` | int | `3` | Minimum hyperlane distance |
| `guaranteed_habitable` | int | `2` | Guaranteed habitable worlds per empire |
| `supports_shape` | string[] | 11 entries | Supported galaxy shapes |

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
    radius = 250          # Proportional radius
    num_empires = 12      # Scaled from Gigantic (18)
    num_empires_default = 6
    num_advanced_empires = 3
    num_fallen_empires = 3
    num_fallen_empires_default = 3
    crisis_strength = 1.25
    num_nebulae = 8
    num_systems_per_empire = 4
    min_distance_between_empires = 3
    guaranteed_habitable = 2
    supports_shape = "elliptical"
    supports_shape = "spiral_2"
    supports_shape = "spiral_3"
    supports_shape = "spiral_4"
    supports_shape = "spiral_6"
    supports_shape = "ring"
    supports_shape = "bar"
    supports_shape = "starburst"
    supports_shape = "cartwheel"
    supports_shape = "spoked"
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

### Scaling Formula (Used for Original 6 Sizes)

```python
# Approximate scaling used by mod author
def scale_parameter(base_value, star_ratio, exponent=1.0):
    return round(base_value * (star_ratio ** exponent))

# Example: crisis_strength scales with exponent ~0.3
# Gigantic (1200) / Huge (1000) = 1.2
# crisis: 1.5 * 1.2^0.3 ≈ 1.55 → 1.75 (rounded/tuned)
```

### Conservative Scaling (For New Sizes)

```clausewitz
# For a 900-star size (between Minuscule and Gigantic)
num_stars = 900
radius = 220
num_empires = 10
num_empires_default = 5
num_advanced_empires = 2
num_fallen_empires = 2
num_fallen_empires_default = 2
crisis_strength = 1.0
num_nebulae = 6
```

### Aggressive Scaling (For Performance Testing)

```clausewitz
# Minimal empires for maximum stars (stress test)
num_stars = 2000
num_empires = 5
num_empires_default = 1
num_advanced_empires = 0
num_fallen_empires = 0
num_fallen_empires_default = 0
crisis_strength = 0.5
num_nebulae = 5
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
| radius | 50 | 300 | 350 | 400 | 450 | 500 |
| ... | ... | ... | ... | ... | ... | ... |

## CI/CD Integration Examples

### GitHub Actions Workflow Snippet

```yaml
# .github/workflows/validate.yml
- name: Validate Mod
  run: |
    chmod +x scripts/validate-all.sh
    ./scripts/validate-all.sh
```

### Pre-commit Hook

```bash
# .git/hooks/pre-commit
#!/bin/bash
./scripts/validate-all.sh || exit 1
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