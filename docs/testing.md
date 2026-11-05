# Testing

## Test Strategy

This mod has no executable code, so traditional unit/integration testing does not apply. Testing focuses on validation of declarative configuration and manual verification of galaxy generation behaviour.

### Test Categories

| Category | Approach | Automation |
|----------|----------|------------|
| Syntax Validation | Schema validation scripts | CI/CD |
| Cross-Reference Consistency | Automated checks | CI/CD |
| Generation Verification | Manual in-game testing | Release checklist |
| Compatibility Testing | Manual on target versions | Release checklist |
| Regression Testing | Manual comparison | Per release |

## Automated Validation (CI)

### Descriptor Validation

**Script:** `scripts/validate-descriptors.sh`

**Checks:**
- Both `.mod` files exist
- Identical `version`, `name`, `path`, `supported_version`, `tags`
- `path="mod/ui-galaxy-sizes"`
- `supported_version` matches pattern `v*.*.*`

### Scenario Validation

**Script:** `scripts/validate-scenarios.sh`

**Checks per scenario file:**
- File exists at `map/setup_scenarios/ui_<size>.txt`
- Parses as valid `setup_scenario` block
- `name` matches filename (`ui_<size>`)
- `priority` is integer
- All required parameters present
- Parameter values within bounds (see data-model.md)
- All 11 `supports_shape` declarations present
- `supports_shape` list identical across all six files

**Cross-scenario checks:**
- Six `priority` values unique
- Priorities ordered: -1 < 5 < 6 < 7 < 8 < 9
- All scenarios define same parameter set

### Localisation Validation

**Script:** `scripts/validate-localisation.sh`

**Checks:**
- `localisation/engligh/ui-galaxy-sizes_l_english.yml` exists
- Valid YAML
- Contains `l_english:` root key
- Six keys: `ui_minuscule`, `ui_gigantic`, `ui_immense`, `ui_colossal`, `ui_titanic`, `ui_ludicrous`
- Each key has `:0 "Label"` format
- Labels non-empty
- Keys match scenario `name` values

## Manual Verification (Release Checklist)

### Per-Scenario Generation Test

For each of the six scenarios:

1. Install mod on Stellaris `4.5.x`
2. Activate in launcher
3. Start New Game
4. Select scenario from Galaxy Size dropdown
5. Verify name displays correctly (localised)
6. Select each of 11 galaxy shapes
7. Generate galaxy
8. Verify:
   - Generation completes without error
   - Star count approximately matches `num_stars`
   - Empire counts within scenario bounds
   - Nebulae visible
   - Hyperlanes connect systems
   - Save game loads correctly

### Ludicrous Specific Tests

1. **With UI Star Names active:**
   - Generate Ludicrous galaxy
   - Verify 2000 unique system names (spot check)
   - No duplicate names in sample

2. **Without UI Star Names:**
   - Generate Ludicrous galaxy
   - Observe duplicate names (expected)
   - Verify generation completes

### Cross-Version Compatibility

Test on each supported Stellaris patch:
- 4.5.0
- 4.5.1
- 4.5.2 (if released)
- Latest 4.5.x

### Edge Case Tests

| Test | Expected |
|------|----------|
| Mod not activated | Six sizes not in dropdown |
| Invalid game version | Mod hidden/incompatible in launcher |
| Missing localisation file | Raw keys in dropdown |
| Corrupted scenario file | Engine logs error, scenario missing |
| Maximum empires (60) | Generation completes |
| All shapes for all sizes | All 11 shapes generate |

## Test Data

### Scenario Test Matrix

| Scenario | Stars | Default Empires | Max Empires | Fallen Default | Crisis | Nebulas |
|----------|-------|-----------------|-------------|----------------|--------|---------|
| Minuscule | 100 | 3 | 3 | 0 | 0.25 | 1 |
| Gigantic | 1200 | 18 | 36 | 5 | 1.75 | 12 |
| Immense | 1400 | 21 | 42 | 6 | 2.0 | 14 |
| Colossal | 1600 | 24 | 48 | 7 | 2.25 | 16 |
| Titanic | 1800 | 27 | 54 | 8 | 2.5 | 18 |
| Ludicrous | 2000 | 30 | 60 | 9 | 2.75 | 20 |

### Shape Test Matrix

All 11 shapes for all 6 scenarios = 66 combinations.

Shapes: `elliptical`, `spiral_2`, `spiral_3`, `spiral_4`, `spiral_6`, `ring`, `bar`, `starburst`, `cartwheel`, `spoked`

## Regression Detection

### Parameter Drift Detection

CI validates:
- No scenario parameter removed
- No new parameter added without review
- `priority` values unchanged unless intentional
- `supports_shape` list unchanged

### Visual Regression

Not automated. Manual spot-check:
- Galaxy topology matches selected shape
- Empire distribution looks reasonable
- Nebulae placement looks reasonable

## Performance Considerations

### Generation Time

| Scenario | Approximate Generation Time |
|----------|----------------------------|
| Minuscule | < 5 seconds |
| Gigantic | 10-20 seconds |
| Immense | 15-30 seconds |
| Colossal | 20-40 seconds |
| Titanic | 30-60 seconds |
| Ludicrous | 60-120 seconds |

### Memory Usage

Scales with `num_stars`. Ludicrous (2000 stars) requires ~2-3x memory of Huge (1000 stars).

## Test Environment Setup

### Local Testing

```bash
# Copy mod to Stellaris mod directory
cp -r ui-galaxy-sizes/ ~/Documents/Paradox\ Interactive/Stellaris/mod/
cp ui-galaxy-sizes.mod ~/Documents/Paradox\ Interactive/Stellaris/mod/

# Launch Stellaris, activate mod in playset
```

### CI Environment

- Ubuntu latest
- Stellaris not installable in CI (no GUI)
- Validation runs on static files only
- Manual testing required for generation verification

## Known Test Gaps

1. **No automated generation testing** — Requires Stellaris engine, not feasible in CI
2. **No save game compatibility testing** — Manual only
3. **No multiplayer testing** — Mod is single-player only (galaxy generation)
4. **No localisation testing beyond English** — Only English supported
5. **No performance benchmarking** — Manual observation only

## Future Test Improvements

- Add Stellaris CLI/headless mode support if available
- Create parameterised test matrix for CI validation
- Add schema validation against Stellaris game files (if accessible)
- Implement visual diff for galaxy topology (screenshot comparison)