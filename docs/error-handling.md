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

| Error Type | Engine Conduct | Mod Impact |
|------------|------------------|------------|
| Invalid `.mod` descriptor | Mod hidden in launcher, error logged | Mod unusable |
| Malformed scenario file | Scenario ignored, error logged | Size absent from dropdown |
| Absent required parameter | Default value used, warning logged | Unexpected generation |
| Invalid parameter value | Clamped to bounds, warning logged | Unexpected generation |
| Duplicate scenario name | First prevails, warning logged | One size absent |
| Unknown parameter | Ignored, warning logged | Parameter ineffective |

### Generation-Time Errors

During galaxy generation:

| Error Type | Engine Conduct | Mod Impact |
|------------|------------------|------------|
| Out of memory | Generation fails, error dialogue | Player cannot play size |
| Invalid shape for size | Falls back to default shape | Incorrect topology |
| Insufficient system names | Duplicates used, warning logged | Duplicate names |
| Empire placement failure | Fewer empires placed, warning | Less competition |
| Hyperlane generation failure | Sparse connections, warning | Disconnected systems |

### Runtime Errors (Engine)

| Error Type | Engine Conduct |
|------------|------------------|
| Save game corruption | Load fails, backup offered |
| Mod version mismatch | Mod deactivated, save may break |
| Absent mod dependency | Capabilities unavailable |

## Mod-Level Error Prevention

### Static Validation (Continuous Integration)
**Prevents errors before release:**

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
| Parameter bounds | Documented in data-model.md, validated in continuous integration |
| Required parameters | Checklist in the validation scripts |
| Cross-scenario consistency | Continuous integration validates identical parameter sets |
| Shape support consistency | Continuous integration validates declared shapes against the engine set |
| Priority uniqueness | Continuous integration validates unique, ordered priorities |
| Localisation completeness | Continuous integration validates that all six keys are present |

### Parameter Validation Rules

**Enforced by continuous integration:**

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
supports_shape: each record from the known set
priority: unique integer throughout scenarios
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
- Continuous integration validates the descriptor syntax
- Version pinning in the descriptor
- Clear installation instructions in the README

### Scenario: Size Absent from Dropdown

**Symptoms:** Five or fewer sizes displayed

**Causes:**
- Scenario file absent
- Scenario parse error
- Duplicate scenario name
- Localisation absent (displays a raw key)

**Mitigations:**
- Continuous integration validates that all six scenario files exist
- Continuous integration validates the syntax
- Continuous integration validates unique names
- Continuous integration validates the localisation keys

### Scenario: Generation Fails or Hangs

**Symptoms:** "Generating galaxy..." indefinitely, or error dialogue

**Causes:**
- `num_stars` too high for available memory
- Invalid parameter combination
- Engine defect

**Mitigations:**
- Parameter bounds documented and validated
- Tested against the target Stellaris versions
- Ludicrous (2000) tested on systems with 16GB or more memory

### Scenario: Unexpected Generation Results

**Symptoms:** Incorrect star count, absent empires, no nebulae, etc.

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

Users observe engine errors, not mod errors. Frequent messages:

| Engine Message | Probable Mod Cause | User Action |
|----------------|------------------|-------------|
| "Mod incompatible" | Version mismatch | Update Stellaris or the mod |
| "Failed to load mod" | Descriptor error | Reinstall the mod, verify the files |
| "Galaxy generation failed" | Parameters too high | Try a smaller size |
| "Absent localisation" | Localisation file issue | Verify the mod integrity |

### Debugging Steps for Users

1. Check `game.log` for mod-related records
2. Verify the Stellaris version corresponds to `supported_version`
3. Verify the mod files are intact (reinstall)
4. Try a smaller galaxy size
5. Deactivate other mods to isolate the issue
6. Report on GitHub with a log excerpt

## Mod Author Error Handling

### Development Workflow

1. Edit the scenario files
2. Run the validation scripts locally
3. Test in-game against the target version
4. Continuous integration executes validation on the pull request
5. Release only after continuous integration passes

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
- [ ] Manual test on every declared shape (spot check)
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
