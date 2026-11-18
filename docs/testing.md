# Testing

## Test Strategy

This mod contains no executable code, so traditional unit and integration testing does not apply. Verification concentrates on validation of the declarative configuration and manual confirmation of galaxy generation conduct.

### Test Categories

| Category | Method | Automation |
|----------|----------|------------|
| Syntax Validation | Schema validation scripts | Continuous integration |
| Cross-Reference Consistency | Automated checks | Continuous integration |
| Generation Verification | Manual in-game testing | Release checklist |
| Compatibility Testing | Manual against target versions | Release checklist |
| Regression Testing | Manual comparison | Per release |

## Automated Validation (Continuous Integration)

### Descriptor Validation

**Script:** `scripts/validate-descriptors.sh`

**Checks:**
- Both `.mod` files exist
- Identical `version`, `name`, `path`, `supported_version`, and `tags`
- `path="mod/ui-galaxy-sizes"`
- `supported_version` corresponds to the pattern `v*.*.*`

### Scenario Validation

**Script:** `scripts/validate-scenarios.sh`

**Checks per scenario file:**
- File exists at `map/setup_scenarios/ui_<size>.txt`
- Parses as a valid `setup_scenario` block
- `name` corresponds to the filename (`ui_<size>`)
- `priority` is an integer
- Every required parameter is present
- Parameter values within bounds (see data-model.md)
- Minuscule declares 4 shapes; the remaining five declare all 10
- `supports_shape` lists identical throughout Gigantic..Ludicrous

**Cross-scenario checks:**
- Six `priority` values unique
- Priorities ordered: -1 < 5 < 6 < 7 < 8 < 9
- Every scenario defines an identical parameter set

### Localisation Validation

**Script:** `scripts/validate-localisation.sh`

**Checks:**
- `localisation/engligh/ui-galaxy-sizes_l_english.yml` exists
- Valid YAML
- Contains the `l_english:` root key
- Six keys: `ui_minuscule`, `ui_gigantic`, `ui_immense`, `ui_colossal`, `ui_titanic`, `ui_ludicrous`
- Each key possesses the `:0 "Label"` format
- Labels are non-empty
- Keys correspond to scenario `name` values

## Manual Verification (Release Checklist)

### Per-Scenario Generation Test

For each of the six scenarios:

1. Install the mod on Stellaris `4.5.x`
2. Activate it in the launcher
3. Start a New Game
4. Select the scenario from the Galaxy Size dropdown
5. Verify that the name displays correctly (localised)
6. Select every declared galaxy shape
7. Generate the galaxy
8. Verify:
   - Generation concludes without error
   - Star count approximately corresponds to `num_stars`
   - Empire counts within scenario bounds
   - Nebulae visible
   - Hyperlanes connect systems
   - Save game loads correctly

### Ludicrous Specific Tests

1. **With UI Star Names active:**
   - Generate a Ludicrous galaxy
   - Verify 2000 unique system names (spot check)
   - No duplicate names in the sample

2. **Without UI Star Names:**
   - Generate a Ludicrous galaxy
   - Observe duplicate names (expected)
   - Verify generation concludes

### Cross-Version Compatibility

Test against every supported Stellaris patch:
- 4.5.0
- 4.5.1
- 4.5.2 (if released)
- The latest 4.5.x

### Edge Case Tests

| Test | Expected |
|------|----------|
| Mod not activated | Six sizes not in the dropdown |
| Invalid game version | Mod hidden or incompatible in the launcher |
| Absent localisation file | Raw keys in the dropdown |
| Corrupted scenario file | The engine logs an error and the scenario is absent |
| Maximum empires (60) | Generation concludes |
| Every shape for every size | 54 combinations generate |

## Test Data

### Scenario Test Matrix

| Scenario | Stars | Default Empires | Max Empires | Fallen Default | Crisis | Nebulas |
|----------|-------|-----------------|-------------|----------------|--------|---------|
| Minuscule | 100 | 2 | 3 | 0 | 0.375 | 1 |
| Gigantic | 1200 | 18 | 36 | 5 | 1.75 | 12 |
| Immense | 1400 | 21 | 42 | 6 | 2.0 | 14 |
| Colossal | 1600 | 24 | 48 | 6 | 2.25 | 16 |
| Titanic | 1800 | 27 | 54 | 6 | 2.5 | 18 |
| Ludicrous | 2000 | 30 | 60 | 6 | 2.75 | 20 |

### Shape Test Matrix

Minuscule 4 shapes + 5 sizes × 10 shapes = 54 combinations.

Shapes: `elliptical`, `spiral_2`, `spiral_3`, `spiral_4`, `spiral_6`, `ring`, `bar`, `starburst`, `cartwheel`, `spoked`

## Regression Detection

### Parameter Drift Detection

Continuous integration validates:
- No scenario parameter was extracted
- No parameter was included without review
- `priority` values unchanged unless intentional
- `supports_shape` list unchanged

### Visual Regression

Not automated. Manual spot-check:
- Galaxy topology corresponds to the selected shape
- Empire distribution appears reasonable
- Nebulae placement appears reasonable

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

Scales with `num_stars`. Ludicrous (2000 stars) requires approximately 2-3x the memory of Huge (1000 stars).

## Test Environment Setup

### Local Testing

```bash
# Copy the mod to the Stellaris mod directory
cp -r ui-galaxy-sizes/ ~/Documents/Paradox\ Interactive/Stellaris/mod/
cp ui-galaxy-sizes.mod ~/Documents/Paradox\ Interactive/Stellaris/mod/

# Launch Stellaris and activate the mod in a playset
```

### Continuous Integration Environment

- Ubuntu latest
- Stellaris cannot be installed in continuous integration (no graphical interface)
- Validation runs against static files only
- Manual testing required for generation confirmation

## Known Test Gaps

1. **No automated generation testing** — Requires the Stellaris engine, which is not feasible in continuous integration
2. **No save game compatibility testing** — Manual only
3. **No multiplayer testing** — The mod is single-player only (galaxy generation)
4. **No localisation testing beyond English** — Only English is supplied
5. **No performance benchmarking** — Manual observation only

## Future Test Improvements

- Include Stellaris command-line or headless mode support if available
- Create a parameterised test matrix for continuous integration validation
- Include schema validation against the Stellaris game files (if accessible)
- Implement visual comparison of galaxy topology (screenshot comparison)
