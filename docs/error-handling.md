# Error Handling

## Error Handling Model

**Not Applicable** — This mod has no runtime error handling code.

### Reason

The mod is purely declarative configuration. It contains:
- No executable code
- No try/catch blocks
- No exception handling
- No error recovery logic
- No validation code

All error handling occurs within the Stellaris engine when it parses and consumes the mod files.

## Engine Error Handling (Context)

### Parse-Time Errors

When Stellaris loads the mod, it validates each file:

| Error Type | Engine Behaviour | Mod Impact |
|------------|------------------|------------|
| Invalid `.mod` descriptor | Mod hidden in launcher, error logged | Mod unusable |
| Malformed scenario file | Scenario ignored, error logged | Size missing from dropdown |
| Missing required parameter | Default value used, warning logged | Unexpected generation |
| Invalid parameter value | Clamped to bounds, warning logged | Unexpected generation |
| Duplicate scenario name | First wins, warning logged | One size missing |
| Unknown parameter | Ignored, warning logged | Parameter ineffective |

### Generation-Time Errors

During galaxy generation:

| Error Type | Engine Behaviour | Mod Impact |
|------------|------------------|------------|
| Out of memory | Generation fails, error dialog | Player cannot play size |
| Invalid shape for size | Falls back to default shape | Wrong topology |
| Insufficient system names | Duplicates used, warning logged | Duplicate names |
| Empire placement failure | Fewer empires placed, warning | Less competition |
| Hyperlane generation failure | Sparse connections, warning | Disconnected systems |

### Runtime Errors (Engine)

| Error Type | Engine Behaviour |
|------------|------------------|
| Save game corruption | Load fails, backup offered |
| Mod version mismatch | Mod disabled, save may break |
| Missing mod dependency | Features unavailable |

## Mod-Level Error Prevention

### Static Validation (CI)

Prevents errors before release:

```bash
# Descriptor validation
scripts/validate-descriptors.sh

# Scenario validation
scripts/validate-scenarios.sh

# Localisation validation
scripts/validate-localisation.sh
```

### Design-Time Safeguards

| Safeguard | Implementation |
|-----------|----------------|
| Parameter bounds | Documented in data-model.md, validated in CI |
| Required parameters | Checklist in validation scripts |
| Cross-scenario consistency | CI validates identical parameter sets |
| Shape support consistency | CI validates 11 shapes in all scenarios |
| Priority uniqueness | CI validates unique, ordered priorities |
| Localisation completeness | CI validates 6 keys present |

### Parameter Validation Rules

**Enforced by CI:**

```yaml
# Per scenario
num_stars: integer > 0
radius: integer > 0
num_empires: integer >= 0
num_empires_default: integer >= 0, <= num_empires
num_advanced_empires: integer >= 0
num_fallen_empires: integer >= 0
num_fallen_empires_default: integer >= 0, <= num_fallen_empires
crisis_strength: float >= 0
num_nebulae: integer >= 0
supports_shape: exactly 11 entries, all from known set
priority: unique integer across scenarios
```

**Enforced by Engine (at parse time):**
- All above, plus type checking
- Unknown parameters ignored

## Error Scenarios and Mitigations

### Scenario: Mod Not Loading

**Symptoms:** Mod not in launcher playset, not in dropdown

**Causes:**
- `supported_version` mismatch
- Descriptor syntax error
- File permissions
- Wrong mod directory

**Mitigations:**
- CI validates descriptor syntax
- Version pinning in descriptor
- Clear installation instructions in README

### Scenario: Size Missing from Dropdown

**Symptoms:** 5 or fewer sizes shown

**Causes:**
- Scenario file missing
- Scenario parse error
- Duplicate scenario name
- Localisation missing (shows raw key)

**Mitigations:**
- CI validates all 6 scenario files exist
- CI validates parseable syntax
- CI validates unique names
- CI validates localisation keys

### Scenario: Generation Fails or Hangs

**Symptoms:** "Generating galaxy..." indefinitely, or error dialog

**Causes:**
- `num_stars` too high for memory
- Invalid parameter combination
- Engine bug

**Mitigations:**
- Parameter bounds documented and validated
- Tested on target Stellaris versions
- Ludicrous (2000) tested on 16GB+ RAM systems

### Scenario: Unexpected Generation Results

**Symptoms:** Wrong star count, missing empires, no nebulae, etc.

**Causes:**
- Parameter value error
- Engine clamping values
- Shape incompatibility

**Mitigations:**
- Parameter documentation in data-model.md
- Cross-scenario consistency checks
- Manual testing per release

## Error Reporting for Users

### User-Facing Errors

Users see engine errors, not mod errors. Common messages:

| Engine Message | Likely Mod Cause | User Action |
|----------------|------------------|-------------|
| "Mod incompatible" | Version mismatch | Update Stellaris or mod |
| "Failed to load mod" | Descriptor error | Reinstall mod, verify files |
| "Galaxy generation failed" | Parameters too high | Try smaller size |
| "Missing localisation" | Localisation file issue | Verify mod integrity |

### Debugging Steps for Users

1. Check `game.log` for mod-related entries
2. Verify Stellaris version matches `supported_version`
3. Verify mod files intact (reinstall)
4. Try smaller galaxy size
5. Disable other mods to isolate
6. Report on GitHub with log excerpt

## Mod Author Error Handling

### Development Workflow

1. Edit scenario files
2. Run validation scripts locally
3. Test in-game on target version
4. CI runs validation on PR
5. Release only after CI passes

### Validation Scripts

```bash
# Run all validations
./scripts/validate-all.sh

# Individual
./scripts/validate-descriptors.sh
./scripts/validate-scenarios.sh
./scripts/validate-localisation.sh
```

### Release Checklist

- [ ] All validation scripts pass
- [ ] Manual generation test on all 6 sizes
- [ ] Manual test on all 11 shapes (spot check)
- [ ] Localisation displays correctly
- [ ] Version bumped in both descriptors
- [ ] Changelog updated
- [ ] Git tag signed

## Summary

| Aspect | Status |
|--------|--------|
| Mod error handling code | None (not applicable) |
| Engine error handling | Yes (Stellaris internal) |
| Mod-controllable error handling | No |
| Static validation | Yes (CI scripts) |
| Design-time safeguards | Yes (parameter bounds, consistency) |
| User-facing error control | Indirect (via valid config) |
| Error recovery | Engine responsibility |
| Error reporting | Engine logs + GitHub issues |