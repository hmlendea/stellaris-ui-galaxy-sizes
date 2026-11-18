# Invariants

## System Invariants

These invariants must hold for the mod to function correctly. Violations indicate configuration errors.

### Descriptor Invariants

| Invariant | Description | Validation |
|-----------|-------------|------------|
| `version` format | Semantic version `MAJOR.MINOR.PATCH` | Continuous integration regular expression |
| `name` constant | `"Universum Infinitum - Galaxy Sizes"` | Continuous integration string comparison |
| `path` constant | `"mod/ui-galaxy-sizes"` | Continuous integration string comparison |
| `supported_version` pattern | `v*.*.*` (e.g. `v4.5.*`) | Continuous integration regular expression |
| `tags` contains | `"Galaxy Generation"` | Continuous integration array check |
| Root matches local descriptor | Both `.mod` files are identical | Continuous integration comparison |

### Scenario Invariants

**Per Scenario File (`map/setup_scenarios/ui_<size>.txt`):**

| Invariant | Description | Validation |
|-----------|-------------|------------|
| File exists | All 6 scenario files present | Continuous integration file list |
| Valid Clausewitz syntax | Parses as a `setup_scenario` block | Continuous integration parser |
| `name` corresponds to filename | `name = "ui_<size>"` | Continuous integration string comparison |
| `priority` unique integer | No duplicates throughout the 6 scenarios | Continuous integration uniqueness |
| `priority` ordering | `-1 < 5 < 6 < 7 < 8 < 9` | Continuous integration ordering |
| Shape declarations valid | Minuscule = 4 vanilla tiny shapes; others = all 10 shapes | Continuous integration count and set |
| Fallen empire counts within vanilla type pool | `fallen_empire_max` ≤ 6 | Continuous integration bound |
| Marauder counts within vanilla clan pool | `marauder_empire_max` ≤ 3 | Continuous integration bound |
| Required parameters present | 20 core parameters (see below) | Continuous integration presence |
| Parameter types correct | Integers, floats, and strings as specified | Continuous integration type check |

**Required Parameters (20):**

```clausewitz
name                    # string, matches filename
priority                # integer, unique
num_stars               # integer > 0
radius                  # integer > 0
num_empires             # range, min >= 0
num_empire_default      # integer >= 0, within num_empires
fallen_empire_default   # integer >= 0, <= fallen_empire_max
fallen_empire_max       # integer <= 6 (vanilla type pool)
marauder_empire_default # integer >= 0, <= marauder_empire_max
marauder_empire_max     # integer <= 3 (vanilla clan pool)
nomad_empire_default    # integer >= 0, <= nomad_empire_max
nomad_empire_max        # integer >= 0
advanced_empire_default # integer >= 0
crisis_strength         # float >= 0
extra_crisis_strength   # two integers
cluster_radius          # integer > 0
cluster_distance_from_core # integer > 0
num_nebulas             # integer >= 0
num_hyperlanes          # range, max <= 3
supports_shape          # string from known set (4x Minuscule, 10x others)
```

### Cross-Scenario Invariants

| Invariant | Description | Validation |
|-----------|-------------|------------|
| Parameter set identical | Every one of the 6 scenarios defines the identical 20 parameters | Continuous integration set equality |
| `num_stars` strictly increasing | 100 < 1200 < 1400 < 1600 < 1800 < 2000 | Continuous integration ordering |
| `radius` strictly increasing | 150 < 460 < 470 < 480 < 490 < 500 | Continuous integration ordering |
| Empire counts scale with stars | Approximately linear scaling | Continuous integration ratio check |
| Crisis strength scales | Monotonic increase with stars | Continuous integration ordering |
| Nebulae count scales | Monotonic increase with stars | Continuous integration ordering |

### Localisation Invariants

| Invariant | Description | Validation |
|-----------|-------------|------------|
| File exists | `localisation/engligh/ui-galaxy-sizes_l_english.yml` | Continuous integration file check |
| Valid YAML | Parses without error | Continuous integration YAML parser |
| Root key `l_english:` | Exactly one root key | Continuous integration structure |
| Six keys present | `ui_minuscule`, `ui_gigantic`, `ui_immense`, `ui_colossal`, `ui_titanic`, `ui_ludicrous` | Continuous integration key set |
| Keys correspond to scenarios | Each key corresponds to a scenario `name` | Continuous integration cross-reference |
| Format `:0 "Label"` | Each key possesses a `:0 "..."` value | Continuous integration format check |
| Labels non-empty | No vacant strings | Continuous integration length check |
### Shape Invariants

**Known Shape Set (10):**

```
elliptical
spiral_2
spiral_3
spiral_4
spiral_6
ring
bar
starburst
cartwheel
spoked
```

| Invariant | Description | Validation |
|-----------|-------------|------------|
| All shapes known | Each `supports_shape` within the known set | Continuous integration set membership |
| No unknown shapes | The engine would ignore an unknown shape | Continuous integration set membership |
| Large sizes complete | Gigantic..Ludicrous declare all 10 shapes in an identical order | Continuous integration list equality |

### Version Invariants

| Invariant | Description | Validation |
|-----------|-------------|------------|
| Descriptor version = release | The `version` corresponds to the release tag | Release script |
| Descriptor version = documentation | The latest documented version corresponds | Release checklist |
| `supported_version` current | Corresponds to the latest Stellaris 4.5.x | Manual per release |

### File Structure Invariants

| Invariant | Description | Validation |
|-----------|-------------|------------|
| Mod root structure | `ui-galaxy-sizes/` with subdirectories | Continuous integration directory check |
| No additional files in scenarios | Only 6 `.txt` files in `setup_scenarios/` | Continuous integration file list |
| Localisation directory | `localisation/engligh/` (typo preserved) | Continuous integration path check |
| Thumbnail exists | `thumbnail.png` in the mod root | Continuous integration file check |
| Descriptor at the root | `ui-galaxy-sizes.mod` at the repository root | Continuous integration file check |

### Runtime Invariants (Engine-Enforced)

| Invariant | Description | Violation Effect |
|-----------|-------------|------------------|
| `num_stars` ≤ engine maximum | The engine imposes an internal limit | Generation fails |
| `radius` ≤ engine maximum | The engine imposes an internal limit | Generation fails |
| `num_empires` ≤ engine maximum | The engine imposes an internal limit | Clamped, warning |
| Sufficient memory | `num_stars` × overhead < available memory | Generation fails or exhausts memory |
| Shape supported by the engine | All 10 shapes within the engine | Fallback to the default |

## Invariant Violation Response

### Continuous Integration Failure (Pre-Release)

1. The pipeline fails
2. The pull request is blocked from merging
3. The author corrects the configuration
4. Validation is re-executed

### Runtime Violation (Post-Release)

1. User reports the issue
2. Author reproduces it
3. Identify the violated invariant
4. Release a patch with the correction
5. Include validation if a new invariant is discovered

## Invariant Maintenance

### Adding New Invariants

When new constraints discovered:
1. Document it in this file
2. Include validation in the continuous integration scripts
3. Test against existing mod versions
4. Ensure no false positives

### Removing Invariants

Only if:
- Engine conduct changes (new Stellaris version)
- The mod design changes fundamentally
- Documented with rationale

### Version-Specific Invariants

For Stellaris version upgrades:
- Review the engine limits
- Update `supported_version`
- Re-validate every invariant
- Test generation against the new version

## Invariant Summary Table

| Category | Count | Criticality |
|----------|-------|-------------|
| Descriptor | 6 | Critical (the mod will not load) |
| Scenario (per file) | 10 | Critical (size absent or broken) |
| Cross-scenario | 6 | High (inconsistent conduct) |
| Localisation | 7 | Medium (UI displays raw keys) |
| Shape | 3 | High (shapes unavailable) |
| Version | 3 | Critical (version mismatch) |
| File structure | 5 | Medium (absent files) |
| Runtime | 5 | Engine-enforced |

**Total: 45 invariants** — all validated in continuous integration except the runtime invariants, which the engine enforces.
