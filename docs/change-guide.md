# Change Guide

## Purpose

This guide defines the process for modifying the mod, from development through release. It ensures consistency, traceability, and quality.

## Change Categories

| Category | Description | Version Bump | Review Required |
|----------|-------------|--------------|-----------------|
| Patch | Defect corrections, parameter adjustments, validation updates | PATCH (1.4.5 → 1.4.6) | Yes |
| Minor | New galaxy size, new shape support, new localisation | MINOR (1.4.5 → 1.5.0) | Yes |
| Major | Breaking descriptor changes, Stellaris version upgrade | MAJOR (1.4.5 → 2.0.0) | Yes, with a migration document |

## Development Workflow

### 1. Create a Branch

```bash
git checkout -b change/<category>-<description>
# Examples:
# change/patch-correct-ludicrous-priority
# change/minor-include-quantum-size
# change/major-stellaris-4-6-support
```

### 2. Make the Changes

Edit the files in the `ui-galaxy-sizes/` directory:

| File Type | Location | Notes |
|-----------|----------|-------|
| Descriptors | `ui-galaxy-sizes.mod`, `ui-galaxy-sizes/descriptor.mod` | Keep identical |
| Scenarios | `ui-galaxy-sizes/map/setup_scenarios/ui_*.txt` | 6 files |
| Localisation | `ui-galaxy-sizes/localisation/engligh/ui-galaxy-sizes_l_english.yml` | English only |
| Thumbnail | `ui-galaxy-sizes/thumbnail.png` | 256x256 PNG |

### 3. Validate Locally

```bash
# Execute every validation script
./scripts/validate-all.sh

# Or individually
./scripts/validate-descriptors.sh
./scripts/validate-scenarios.sh
./scripts/validate-localisation.sh
```

### 4. Test In-Game

1. Copy the mod to the Stellaris mod directory
2. Launch Stellaris (corresponding to `supported_version`)
3. Activate the mod
4. Test each changed scenario:
   - Select it from the dropdown
   - Verify the localisation
   - Generate a galaxy (at least one shape)
   - Verify that the star count is approximately correct
   - Save and load the game

### 5. Update the Documentation

| Change Type | Documents to Update |
|-------------|----------------|
| Parameter change | `data-model.md`, `invariants.md` |
| New scenario | `data-model.md`, `repository-structure.md`, `invariants.md`, `testing.md` |
| New shape | `data-model.md`, `invariants.md`, `testing.md` |
| Version bump | `README.md`, `architecture.md` |
| Stellaris version | `architecture.md`, `configuration.md`, `dependencies.md` |
| New validation | `testing.md`, `invariants.md` |

### 6. Commit and Push

```bash
git add -A
git commit -m "<category>: <description>

<details if required>

Fixes #<issue-number>"
git push origin change/<category>-<description>
```

### 7. Create a Pull Request

- Target: `master`
- Title: `<category>: <description>`
- Description: Link the issue, summarise the changes, and note the testing performed
- Continuous integration must pass

### 8. Review and Merge

- At least one review (self-review is acceptable for a patch)
- Continuous integration green
- Merge by squash or rebase

### 9. Release

```bash
# Tag the release (signed)
git tag -s v1.4.6 -m "Release v1.4.6"
git push origin v1.4.6
```

The continuous delivery pipeline publishes to:
- GitHub Releases
- Steam Workshop
- Nexus Mods

## Versioning Rules

### Semantic Versioning

`MAJOR.MINOR.PATCH`

- **MAJOR**: Breaking changes (descriptor format, Stellaris major version)
- **MINOR**: New capabilities (sizes, shapes, localisations) — backward compatible
- **PATCH**: Defect corrections, parameter adjustments, validation — backward compatible

### Version in Descriptors

Both `.mod` files must carry an identical `version` field corresponding to the Git tag (without the `v` prefix):

```clausewitz
version="1.4.6"
```

### Supported Version

Update `supported_version` when:
- A new Stellaris patch is tested and functions correctly
- A new Stellaris minor version is supported (MAJOR bump)

Format: `v4.5.*` (wildcard for patches)

## Parameter Change Guidelines

### Safe Changes (PATCH)

| Parameter | Safe Interval | Notes |
|-----------|------------|-------|
| `num_stars` | ±10% | Test the generation time |
| `radius` | ±10% | Keep proportional to the star count |
| `num_empires` | ±2 | Keep within the engine limits |
| `crisis_strength` | ±0.25 | Test the balance |
| `num_nebulas` | ±2 | Visual only |
| `priority` | Reorder only | Must remain unique |

### Unsafe Changes (Require Testing)

| Parameter | Risk |
|-----------|------|
| `num_stars` > 2000 | Memory, performance |
| `radius` > 500 | Engine limits |
| `num_empires` > 60 | Engine firm limit |
| New `supports_shape` | The engine may not support it |
| Extracting `supports_shape` | Breaks existing saves |

### New Scenario Checklist

- [ ] Unique name `ui_<name>`
- [ ] Unique priority (integer)
- [ ] Every required parameter
- [ ] Every declared `supports_shape` record
- [ ] Localisation key included
- [ ] Parameter scaling consistent with the existing sizes
- [ ] Generation tested for every declared shape
- [ ] Documentation updated

## Stellaris Version Upgrade

### Minor Version (4.5 → 4.6)

1. Test the mod against the new version
2. Update `supported_version` to `v4.6.*`
3. Update the Stellaris version in `architecture.md`
4. Update the Stellaris version in `dependencies.md`
5. Update `configuration.md` if the parameters changed
6. MINOR version bump (1.4.5 → 1.5.0)
7. Document any parameter changes required

### Major Version (4.x → 5.x)

1. Re-test every scenario
2. Check for new or extracted parameters
3. Check for new or extracted shapes
4. Update `supported_version` to `v5.0.*`
5. MAJOR version bump (1.4.5 → 2.0.0)
6. Create a migration guide if saves are affected
7. Update every document

## Validation Script Changes

When including validation rules:

1. Include the check in the appropriate script
2. Document the new invariant in `invariants.md`
3. Test against the current mod (must pass)
4. Test against a known-bad mod (must fail)
5. Update the validation section in `testing.md`

## Documentation Change Rules

### Required Updates Per Change

| Change | Required Document Updates |
|--------|---------------------|
| Any parameter | `data-model.md` parameter table |
| New scenario | `repository-structure.md`, `data-model.md`, `invariants.md`, `testing.md` |
| New shape | `data-model.md`, `invariants.md`, `testing.md` |
| Version bump | `README.md`, `architecture.md` |
| Stellaris version | `architecture.md`, `configuration.md`, `dependencies.md` |
| Validation rule | `testing.md`, `invariants.md` |
| New dependency | `dependencies.md`, `architecture.md` |

### Documentation Style

- Follow the existing Markdown structure
- Use tables for parameter references
- Keep examples current
- Revise the cross-references

## Rollback Procedure

If a release contains a critical issue:

```bash
# 1. Extract the tag
git tag -d v1.4.6
git push origin :refs/tags/v1.4.6

# 2. Revert the commit on master
git revert <merge-commit>
git push origin master

# 3. Correct the issue on a new branch
# 4. Release a patch version (1.4.7)
```

## Emergency Hotfix

For critical issues affecting players:

1. Create a `hotfix/<description>` branch from `master`
2. Make the minimal correction
3. Validate and test
4. PATCH version bump
5. Expedite the pull request (single review, continuous integration only)
6. Release immediately

## Change History Template

For each change, record:

```markdown
### Change: <title>
- **Type**: patch|minor|major
- **Version**: 1.4.5 → 1.4.6
- **Files**: list of modified files
- **Validation**: scripts executed, in-game test results
- **Documentation**: files updated
- **Release**: tag, platforms published
- **Rollback plan**: if applicable
```

## Checklist Summary

### Pre-Commit
- [ ] Validation scripts pass
- [ ] In-game test (affected scenarios)
- [ ] Documentation updated

### Pre-Merge
- [ ] Continuous integration passes
- [ ] Review approved
- [ ] No merge conflicts

### Pre-Release
- [ ] Version bumped in the descriptors
- [ ] Archive tag signed
- [ ] Platform publishing prepared

### Post-Release
- [ ] Verify that the GitHub Release exists
- [ ] Publish to Steam Workshop
- [ ] Publish to Nexus Mods
- [ ] Publish to Paradox Mods
- [ ] Announce (Discord, Reddit, etc.)
