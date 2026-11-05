# Invariants

## System Invariants

These invariants must hold for the mod to function correctly. Violations indicate configuration errors.

### Descriptor Invariants

| Invariant | Description | Validation |
|-----------|-------------|------------|
| `version` format | Semantic version `MAJOR.MINOR.PATCH` | CI regex |
| `name` constant | `"Universum Infinitum: Galaxy Sizes"` | CI string match |
| `path` constant | `"mod/ui-galaxy-sizes"` | CI string match |
| `supported_version` pattern | `v*.*.*` (e.g., `v4.5.*`) | CI regex |
| `tags` contains | `"Gameplay"` and `"Map"` | CI array check |
| Root = local descriptor | Both `.mod` files identical | CI diff |

### Scenario Invariants

**Per Scenario File (`map/setup_scenarios/ui_<size>.txt`):**

| Invariant | Description | Validation |
|-----------|-------------|------------|
| File exists | All 6 scenario files present | CI file list |
| Valid Clausewitz syntax | Parses as `setup_scenario` block | CI parser |
| `name` matches filename | `name = "ui_<size>"` | CI string match |
| `priority` unique integer | No duplicates across 6 scenarios | CI uniqueness |
| `priority` ordering | `-1 < 5 < 6 < 7 < 8 < 9` | CI ordering |
| All 11 shapes declared | `supports_shape` = 11 entries | CI count + set |
| Shape set identical | All scenarios support same 11 shapes | CI set equality |
| Required parameters present | 15 core parameters (see below) | CI presence |
| Parameter types correct | Integers, floats, strings as specified | CI type check |

**Required Parameters (15):**

```clausewitz
name                    # string, matches filename
priority                # integer, unique
num_stars               # integer > 0
radius                  # integer > 0
num_empires             # integer >= 0
num_empires_default     # integer >= 0, <= num_empires
num_advanced_empires    # integer >= 0
num_fallen_empires      # integer >= 0
num_fallen_empires_default # integer >= 0, <= num_fallen_empires
crisis_strength         # float >= 0
num_nebulae             # integer >= 0
num_systems_per_empire  # integer > 0
min_distance_between_empires # integer > 0
guaranteed_habitable    # integer >= 0
supports_shape          # 11x string from known set
```

### Cross-Scenario Invariants

| Invariant | Description | Validation |
|-----------|-------------|------------|
| Parameter set identical | All 6 scenarios define same 15 parameters | CI set equality |
| `num_stars` strictly increasing | 100 < 1200 < 1400 < 1600 < 1800 < 2000 | CI ordering |
| `radius` strictly increasing | 50 < 300 < 350 < 400 < 450 < 500 | CI ordering |
| Empire counts scale with stars | Roughly linear scaling | CI ratio check |
| Crisis strength scales | Monotonic increase with stars | CI ordering |
| Nebulae count scales | Monotonic increase with stars | CI ordering |

### Localisation Invariants

| Invariant | Description | Validation |
|-----------|-------------|------------|
| File exists | `localisation/engligh/ui-galaxy-sizes_l_english.yml` | CI file check |
| Valid YAML | Parses without error | CI YAML parser |
| Root key `l_english:` | Exactly one root key | CI structure |
| Six keys present | `ui_minuscule`, `ui_gigantic`, `ui_immense`, `ui_colossal`, `ui_titanic`, `ui_ludicrous` | CI key set |
| Keys match scenarios | Each key matches a scenario `name` | CI cross-ref |
| Format `:0 "Label"` | Each key has `:0 "..."` value | CI format check |
| Labels non-empty | No empty strings | CI length check |

### Shape Invariants

**Known Shape Set (11):**

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
| All shapes known | Each `supports_shape` in known set | CI set membership |
| No unknown shapes | Engine would ignore unknown | CI set membership |
| All scenarios identical | Same 11 shapes in same order | CI list equality |

### Version Invariants

| Invariant | Description | Validation |
|-----------|-------------|------------|
| Descriptor version = Git tag | `version` matches release tag | Release script |
| Descriptor version = Changelog | Latest changelog entry matches | Release checklist |
| `supported_version` current | Matches latest Stellaris 4.5.x | Manual per release |

### File Structure Invariants

| Invariant | Description | Validation |
|-----------|-------------|------------|
| Mod root structure | `ui-galaxy-sizes/` with subdirs | CI directory check |
| No extra files in scenarios | Only 6 `.txt` files in `setup_scenarios/` | CI file list |
| Localisation directory | `localisation/engligh/` (typo preserved) | CI path check |
| Thumbnail exists | `thumbnail.png` in mod root | CI file check |
| Descriptor at root | `ui-galaxy-sizes.mod` at repo root | CI file check |

### Runtime Invariants (Engine-Enforced)

| Invariant | Description | Violation Effect |
|-----------|-------------|------------------|
| `num_stars` ≤ engine max | Engine has internal limit | Generation fails |
| `radius` ≤ engine max | Engine has internal limit | Generation fails |
| `num_empires` ≤ engine max | Engine has internal limit | Clamped, warning |
| Memory sufficient | `num_stars` × overhead < RAM | Generation fails/OOM |
| Shape supported by engine | All 11 shapes in engine | Fallback to default |

## Invariant Violation Response

### CI Failure (Pre-Release)

1. Pipeline fails
2. PR blocked from merge
3. Author fixes configuration
4. Re-run validation

### Runtime Violation (Post-Release)

1. User reports issue
2. Author reproduces
3. Identify violated invariant
4. Patch release with fix
5. Update validation if new invariant discovered

## Invariant Maintenance

### Adding New Invariants

When new constraints discovered:
1. Document in this file
2. Add validation to CI scripts
3. Test against existing mod versions
4. Ensure no false positives

### Removing Invariants

Only if:
- Engine behaviour changes (new Stellaris version)
- Mod design changes fundamentally
- Documented with rationale

### Version-Specific Invariants

For Stellaris version upgrades:
- Review engine limits
- Update `supported_version`
- Re-validate all invariants
- Test generation on new version

## Invariant Summary Table

| Category | Count | Criticality |
|----------|-------|-------------|
| Descriptor | 6 | Critical (mod won't load) |
| Scenario (per file) | 10 | Critical (size missing/broken) |
| Cross-scenario | 6 | High (inconsistent behaviour) |
| Localisation | 7 | Medium (UI shows raw keys) |
| Shape | 3 | High (shapes not available) |
| Version | 3 | Critical (version mismatch) |
| File structure | 5 | Medium (missing files) |
| Runtime | 5 | Engine-enforced |

**Total: 45 invariants** — all validated in CI except runtime (engine-enforced).